
```
functions = [
   {
      "name":"search_courses",
      "description":"Retrieves courses from the search index based on the parameters provided",
      "parameters":{
         "type":"object",
         "properties":{
            "role":{
               "type":"string",
               "description":"The role of the learner (i.e. developer, data scientist, student, etc.)"
            },
            "product":{
               "type":"string",
               "description":"The product that the lesson is covering (i.e. Azure, Power BI, etc.)"
            },
            "level":{
               "type":"string",
               "description":"The level of experience the learner has prior to taking the course (i.e. beginner, intermediate, advanced)"
            }
         },
         "required":[
            "role"
         ]
      }
   }
]
```

下面让我们更详细地描述每个函数实例：

- `name` 我们想要调用的函数的名称。
- `description` 这是函数如何工作的描述。在这里，重要的是要具体和清晰。
- `parameters` 您希望模型在其响应中生成的值和格式的列表。参数数组由项目组成，其中项目具有以下属性：

1. `type` - 将存储属性的数据类型。
2. `properties` - 模型将用于其响应的特定值的列表。
3. `name` - 键是模型将在其格式化响应中使用的属性的名称，例如，`product`。
4. `type` - 该属性的数据类型，例如，`string`。
5. `description` - 特定属性的描述。

还有一个可选属性`required` - 完成 function call 所需的属性。

### Step 3 - 进行 function call

定义函数后，我们现在需要将其包含在对聊天完成 API 的调用中。我们通过向请求添加`functions`来做到这一点。在这种情况下`functions=functions`。

还有一个选项可以将`function_call`设置为`"auto"`。这意味着我们将让 LLM 根据用户消息决定应该调用哪个函数，而不是我们自己分配它。

下面是我们调用`ChatCompletion.create`的一些代码，请注意我们如何设置`functions=functions`和`function_call="auto"`，从而让 LLM 选择何时调用我们提供的函数：

```
response = openai.ChatCompletion.create( engine="gpt-function",
                                        messages=messages,
                                        functions=functions,
                                        function_call="auto", )
print(response['choices'][0]['message'])
```

现在返回的响应结果如下所示：

```
{
  "role": "assistant",
  "function_call": {
    "name": "search_courses",
    "arguments": "{\n  \"role\": \"student\",\n  \"product\": \"Azure\",\n  \"level\": \"beginner\"\n}"
  }
}
```

在这里，我们可以看到如何调用函数`search_courses`以及使用哪些参数，如 JSON 响应中的`arguments`属性中所列。

结论是，LLMs 能够找到适合函数参数的数据，因为它从聊天完成调用中提供给`messages`参数的值中提取数据。下面是`messages`值的提醒：

```
messages= [ {"role": "user", "content": "Find me a good course for a beginner student to learn Azure."} ]
```

如您所见，`student`、`Azure`和`beginner`是从`messages`中提取的，并设置为函数的输入。以这种方式使用函数是从提示中提取信息的好方法，同时也可以为 LLM 提供结构并具有可重用的功能。

接下来，我们需要看看如何在我们的应用程序中使用它。

## 将 function calling 集成到应用程序中

### [管理流程](https://microsoft.github.io/generative-ai-for-beginners/#/11-integrating-with-function-calling/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e7%ae%a1%e7%90%86%e6%b5%81%e7%a8%8b)

要将其集成到我们的应用程序中，我们需要执行以下步骤：

1. 首先，我们调用 Open AI 服务并将消息存储在名为`response_message`的变量中。

```
response_message = response["choices"][0]["message"]
```

2. 现在我们将定义调用 Microsoft Learn API 来获取课程列表的函数：

```
import requests
def search_courses(role, product, level):
  url = "https://learn.microsoft.com/api/catalog/"
  params = {
     "role": role,
     "product": product,
     "level": level
  }
  response = requests.get(url, params=params)
  modules = response.json()["modules"]
  results = []
  for module in modules[:5]:
     title = module["title"]
     url = module["url"]
     results.append({"title": title, "url": url})
  return str(results)
```

3. 请注意我们现在如何创建一个实际的 Python 函数，该函数映射到`functions`变量中引入的函数名称。我们还进行真正的外部 API 调用来获取我们需要的数据。在这种情况下，我们根据 Microsoft Learn API 来搜索培训模块。

4. 好的，我们创建了`functions`变量和相应的 Python 函数，我们如何告诉 LLM 如何将这两个映射在一起，以便调用我们的 Python 函数？

5. 要查看是否需要调用 Python 函数，我们需要查看 LLM 响应并查看`function_call`是否是其中的一部分并调用所指出的函数。以下是进行下面提到的检查的方法：

```
# Check if the model wants to call a function
if response_message.get("function_call"):
  print("Recommended Function call:")
  print(response_message.get("function_call"))
  print()

 # Call the function.
 function_name = response_message["function_call"]["name"]

 available_functions = {
         "search_courses": search_courses,
 }
 function_to_call = available_functions[function_name]

 function_args = json.loads(response_message["function_call"]["arguments"])
 function_response = function_to_call(**function_args)

 print("Output of function call:")
 print(function_response)
 print(type(function_response))
```

```
# Add the assistant response and function response to the messages
messages.append( # adding assistant response to messages
    {
        "role": response_message["role"],
        "function_call": {
            "name": function_name,
            "arguments": response_message["function_call"]["arguments"],
        },
        "content": None
    }
)
messages.append( # adding function response to messages
    {
        "role": "function",
        "name": function_name,
        "content":function_response,
    }
)
```

```
These three lines, ensure we extract the function name, the arguments and make the call:
function_to_call = available_functions[function_name]

function_args = json.loads(response_message["function_call"]["arguments"])
function_response = function_to_call(**function_args)
```

以下是运行代码的输出：

Output

```
{
  "name": "search_courses",
  "arguments": "{\n  \"role\": \"student\",\n  \"product\": \"Azure\",\n  \"level\": \"beginner\"\n}"
}
Output of function call:
[{'title': 'Describe concepts of cryptography', 'url': 'https://learn.microsoft.com/en-us/training/modules/describe-concepts-of-cryptography/?WT.mc_id=api_CatalogApi'}, {'title': 'Introduction to audio classification with TensorFlow', 'url': 'https://learn.microsoft.com/en-us/training/modules/intro-audio-classification-tensorflow/?WT.mc_id=api_CatalogApi'}, {'title': 'Design a Performant Data Model in Azure SQL Database with Azure Data Studio', 'url': 'https://learn.microsoft.com/en-us/training/modules/design-a-data-model-with-ads/?WT.mc_id=api_CatalogApi'}, {'title': 'Getting started with the Microsoft Cloud Adoption Framework for Azure', 'url': 'https://learn.microsoft.com/en-us/training/modules/cloud-adoption-framework-getting-started/?WT.mc_id=api_CatalogApi'}, {'title': 'Set up the Rust development environment', 'url': 'https://learn.microsoft.com/en-us/training/modules/rust-set-up-environment/?WT.mc_id=api_CatalogApi'}]
<class 'str'>
```

6. 现在我们将向 LLM 发送更新后的消息`messages`，以便我们可以接收自然语言响应，而不是 API JSON 格式的响应。

```
print("Messages in next request:")
print(messages)
print()
second_response = openai.ChatCompletion.create(
  messages=messages,
  engine="gpt-function",
  function_call="auto",
  functions=functions,
  temperature=0
     )  # get a new response from GPT where it can see the function response
```

`print(second_response["choices"][0]["message"])`
