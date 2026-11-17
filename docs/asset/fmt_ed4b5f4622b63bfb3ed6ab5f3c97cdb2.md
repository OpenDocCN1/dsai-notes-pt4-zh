
注意这里创建了一个实际的 Python 函数，与 `functions` 变量中定义的函数名对应。我们还发起了真实的外部 API 调用，获取所需数据。这里调用了 Microsoft Learn API 来搜索培训模块。

好了，我们创建了 `functions` 变量和对应的 Python 函数，如何告诉 LLM 这两者对应，以便调用我们的 Python 函数呢？

要判断是否需要调用 Python 函数，需要检查 LLM 的响应中是否包含 `function_call`，并调用指定函数。下面是检查方法：

```
Check if the model wants to call a function
if response_message.function_call.name:
    print("Recommended Function call:")
    print(response_message.function_call.name)
    print()

# Call the function.
function_name = response_message.function_call.name
available_functions = {"search_courses": search_courses}
function_to_call = available_functions[function_name]

function_args = json.loads(response_message.function_call.arguments)
function_response = function_to_call(**function_args)

print("Output of function call:")
print(function_response)
print(type(function_response))

# Add the assistant response and function response to the messages
messages.append(  # adding assistant response to messages
    {
        "role": response_message.role,
        "function_call": {
            "name": function_name,
            "arguments": response_message.function_call.arguments,
        },
        "content": None,
    }
)
messages.append(  # adding function response to messages
    {
        "role": "function",
        "name": function_name,
        "content": function_response,
    }
)
```

这三行代码确保提取函数名、参数并执行调用：

```
function_to_call = available_functions[function_name]
function_args = json.loads(response_message.function_call.arguments)
function_response = function_to_call(**function_args)
```

下面是运行代码的输出：

```
{
  "name": "search_courses",
  "arguments": "{\n  \"role\": \"student\",\n  \"product\": \"Azure\",\n  \"level\": \"beginner\"\n}"
}

Output of function call:
[{'title': 'Describe concepts of cryptography', 'url': 'https://learn.microsoft.com/training/modules/describe-concepts-of-cryptography/?WT.mc_id=api_CatalogApi'}, {'title': 'Introduction to audio classification with TensorFlow', 'url': 'https://learn.microsoft.com/en-us/training/modules/intro-audio-classification-tensorflow/?WT.mc_id=api_CatalogApi'}, {'title': 'Design a Performant Data Model in Azure SQL Database with Azure Data Studio', 'url': 'https://learn.microsoft.com/training/modules/design-a-data-model-with-ads/?WT.mc_id=api_CatalogApi'}, {'title': 'Getting started with the Microsoft Cloud Adoption Framework for Azure', 'url': 'https://learn.microsoft.com/training/modules/cloud-adoption-framework-getting-started/?WT.mc_id=api_CatalogApi'}, {'title': 'Set up the Rust development environment', 'url': 'https://learn.microsoft.com/training/modules/rust-set-up-environment/?WT.mc_id=api_CatalogApi'}]
<class 'str'>
```

现在，我们将更新后的消息 `messages` 发送给 LLM，以便获得自然语言回复，而非 API JSON 格式响应。

```
print("Messages in next request:")

print(messages)

print()

second_response = client.chat.completions.create(
    messages=messages,
    model=deployment,
    function_call="auto",
    functions=functions,
    temperature=0,
)  # get a new response from GPT where it can see the function response
print(second_response.choices[0].message)
```

输出

```
{
  "role": "assistant",
  "content": "I found some good courses for beginner students to learn Azure:\n\n1. [Describe concepts of cryptography](https://learn.microsoft.com/training/modules/describe-concepts-of-cryptography/?WT.mc_id=api_CatalogApi)\n2. Introduction to audio classification with TensorFlow\n3. Design a Performant Data Model in Azure SQL Database with Azure Data Studio\n4. Getting started with the Microsoft Cloud Adoption Framework for Azure\n5. Set up the Rust development environment\n\nYou can click on the links to access the courses."
}
```

## 练习

为了继续学习 Azure OpenAI 函数调用，你可以尝试：

+ 增加函数的更多参数，帮助学习者找到更多课程。

+ 创建另一个函数调用，获取学习者的母语等更多信息。

+ 为函数调用和/或 API 调用未返回合适课程时添加错误处理。提示：请参考 Learn API reference documentation 页面，了解这些数据的获取方式和位置。

## 干得好！继续前进

完成本课后，欢迎查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

前往第 12 课，我们将探讨如何 [为 AI 应用设计用户体验](https://github.com/microsoft/generative-ai-for-beginners/blob/main/translations/zh/12-designing-ux-for-ai-applications/README.md?WT.mc_id=academic-105485-koreyst)！
