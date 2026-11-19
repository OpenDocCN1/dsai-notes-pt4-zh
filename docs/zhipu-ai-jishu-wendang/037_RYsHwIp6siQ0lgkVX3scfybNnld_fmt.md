
# 工具调用

> 来源：[https://ai.feishu.cn/docx/GSj3dF7zUozvWyxxXkNcaKzBnBg](https://ai.feishu.cn/docx/GSj3dF7zUozvWyxxXkNcaKzBnBg)

本章节将带领开发者学习如何使用`ChatGLM3-6B`来实现工具调用的功能。

请配合以下代码库中微调部分代码进行上手练习

`https://github.com/THUDM/ChatGLM3/tree/main/tool_using`

## 什么是工具调用

大模型虽然强大，但是由于训练的时间和语料限制。大模型通常会存在以下问题：

1. 只能获取训练数据集中有的事件和内容，这意味着大模型不具备访问最新资料的能力。
2. `ChatGLM3-6B`模型以通用语料训练为主，因此，缺少专业领域的知识。
3. `ChatGLM3-6B`体量较小，虽然拥有较强的数学，英语等能力，但仍然无法与`GPT4`等大模型进行抗衡，因此，会出现数学计算不准确等问题。

这时候，通过给大模型加上“四肢“，让大模型学会使用工具，这些问题将迎刃而解。

例如，我们通过编写计算器插件，使用`Python`的`eval()`验证功能，或者`numexpr.evaluate()`等功能，将能有效的辅助`ChatGLM3-6B`进行计算。模型仅需要告诉插件计算的内容，由工具完成计算，并返回大模型，大模型将答案进行整合输出，便可以得到正确的答案。

如果我们编写了一个互联网插件，则可以解决`ChatGLM3-6B`知识不够全面的问题。`ChatGLM3-6B`将可以通过互联网插件访问互联网，来获取最新信息，成为新闻助手等高效生产工具。

## 如何进行工具调用

### 设计工具雏形

首先，应该确定你需要使用的工具，并准备好描述信息。

例如，我现在想准备这两个工具：

```
工具一：track
工具的功能：负责追踪我指定的股票的实时价格
工具的输入：需要追踪的股票代码
输入的类型：参数symbol，为股票代码，str类型

工具二：text-to-speech
工具的功能：将文本转换为语音
工具的输入：需要转换成语音的文本，要用的语音类型（男生，女生），语音的速度
输入的类型：
1. 参数text，为需要转换成语音的文本
2. 参数voice，为要使用的语音类型（男声、女声等）
3. 参数speed，为语音的速度（快、中等、慢等）
```

### 构建描述信息

接着，我们根据我们的想法，准备好要构建的数据的描述信息

```
tools = [
    {
        "name": "track",
        "description": "追踪指定股票的实时价格",
        "parameters": {
            "type": "object",
            "properties": {
                "symbol": {
                    "description": "需要追踪的股票代码"
                }
            },
            "required": ["symbol"]
        }
    },
    {
        "name": "text-to-speech",
        "description": "将文本转换为语音",
        "parameters": {
            "type": "object",
            "properties": {
                "text": {
                    "description": "需要转换成语音的文本"
                },
                "voice": {
                    "description": "要使用的语音类型（男声、女声等）"
                },
                "speed": {
                    "description": "语音的速度（快、中等、慢等）"
                }
            },
            "required": ["text"]
        }
    }
]
system_info = {
    "role": "system",
    "content": "Answer the following questions as best as you can. You have access to the following tools:",
    "tools": tools
}
```

在这里的系统提示词推荐使用固定的搭配，以实现更好的效果

### 工具运行流程

`ChatGLM3-6B`模型也具有完整的工具调用思维链。在用户提出问题后，大模型的思维链如下：

```
<|user|>
提出问题
<|assistant|>
思考应该做什么

<|assistant|>调用工具的名称
```python（这里的python是固定格式，并不一定是python语句）

tool_call(调用工具的参数)

<|observation|>
工具回复的结果，为一个字符串
<|assistant|>
（如果知道最后答案）我知道最后答案了，回答用户的问题
```

因此，如果没有得到`observation`，模型并不会回答用户。

为了更好的了解`ChatGLM3-6B`是如何对工具进行调用的，我们贴心的为开发者设计了一个交互式的脚本`cli_demo_tool.py`文件。该文件中，用户将模拟工具返回结果。

用户仅需按照如下进行执行，按照以下示例进行输入。

```
python cli_demo_tools.py
>>> Loading checkpoint shards: 100%|████████████████████████████████████████████████████████████████████████████████████████████████████| 7/7 [00:01<00:00,  3.49it/s]

>>> 欢迎使用 ChatGLM3-6B 模型，输入内容即可进行对话，clear 清空对话历史，stop 终止程序
>>> 用户：追踪股票 1230的价格
>>> ChatGLM：{'name': 'track', 'parameters': {'symbol': '1230'}}
>>> 结果：12412
>>> ChatGLM：好的，我已经帮您查询了股票代码为1230的实时价格，目前该股票的价格为12412元。请问还有其他需要我帮忙的吗？
```

在这个脚本中，你将会有两次输入：

第一次输入，你的角色是用户，负责提出问题。

接着，`ChatGLM3-6B`会返回要调用的工具的json格式字符串，代表需要请求`track`工具，并等待结果

第二次输入，你的角色是工具,你输入的内容就是工具的结果

`ChatGLM3-6B`会根据你第二次输入的答案，总结后根据上下文场景回答你第一次输入的问题

在代码中，第二次输入的代码在这里，我们假设工具已经获得了结果

```
result = json.dumps({"price": 12412}, ensure_ascii=False)
response, history = model.chat(tokenizer, result, history=history, role="observation")
print(response)
```

这里 `role="observation"` 表示输入的是工具调用的返回值而不是用户输入，不能省略。

期望得到的输出为

根据您的查询，经过API的调用，股票10111的价格是12412。

### 了解工具注册流程

在了解了工具的运行流程之后，

在我们的代码中，我们设计了一个工具注册的函数,用户可以在`tool_register.py`中寻找到这个函数。该函数如下

```
def register_tool(func: callable):
    tool_name = func.__name__
    tool_description = inspect.getdoc(func).strip()
    python_params = inspect.signature(func).parameters
    tool_params = []
    for name, param in python_params.items():
        annotation = param.annotation
        if annotation is inspect.Parameter.empty:
            raise TypeError(f"Parameter `{name}` missing type annotation")
        if get_origin(annotation) != Annotated:
            raise TypeError(f"Annotation type for `{name}` must be typing.Annotated")

        typ, (description, required) = annotation.__origin__, annotation.__metadata__
        typ: str = str(typ) if isinstance(typ, GenericAlias) else typ.__name__
        if not isinstance(description, str):
            raise TypeError(f"Description for `{name}` must be a string")
        if not isinstance(required, bool):
            raise TypeError(f"Required for `{name}` must be a bool")

        tool_params.append({
            "name": name,
            "description": description,
            "type": typ,
            "required": required
        })
    tool_def = {
        "name": tool_name,
        "description": tool_description,
        "params": tool_params
    }

    print("[registered tool] " + pformat(tool_def))
    _TOOL_HOOKS[tool_name] = func
    _TOOL_DESCRIPTIONS[tool_name] = tool_def

    return func
```

这个 Python 函数 `register_tool`（注册工具）的目的是将另一个函数作为参数（用 `func: 可调用` 参数表示），注册它并收集它的元数据。它似乎是一个更大系统的一部分，可能是一个插件系统或命令行工具，其中每个函数都是一个带有参数集的 "工具"。下面是它的操作步骤：

1. 获取传递给它的函数名称 (`func.__name__`)。它获取函数的 docstring (`inspect.getdoc(func)`)，通常用作描述，并删除所有前导或尾部空白。
2. 它会检索函数的参数（`inspect.signature(func).parameters`）。然后遍历这些参数，构建一个参数描述列表 (`tool_params`)。对于每个参数，它会检查该参数是否有类型注解。如r它还会检查类型注解是否属于特殊类型 `typing.Annotated`。
3. 然后，它会从注解中提取类型和元数据。元数据预计是一个元组，包含参数描述和一个布尔值（表明参数是否为必填参数）。
4. 每个参数的信息都会添加到 `tool_params` 列表中。
5. 处理完所有参数后，它会构建一个字典（`tool_def`），其中包含工具名称、描述和参数信息。它会打印出包含工具定义的格式化字符串。然后将函数 `func` 注册到两个字典中：`_TOOL_HOOKS` 和 `_TOOL_DESCRIPTIONS`，并以工具名称为关键字。这些字典似乎用于存储函数本身（`_TOOL_HOOKS`）及其元数据（`_TOOL_DESCRIPTIONS`）。
6. 最后，原始函数 `func` 将被返回，这样就可以在不修改装饰函数的情况下使用装饰器。
7. `register_tool` 函数可以作为一种装饰器，用于在函数定义之上自动将其注册为系统中的 "工具"，并附上完整的描述、参数和类型注释。这种注册可用于生成帮助文本、强制正确使用或将函数动态链接到命令行界面或图形用户界面。

### 实现自己的工具代码

看懂了工具注册函数之后，我们就要开始书写自己的工具了。在 `tool_register.py`中，我们为开发者制作了两个`tools`的代码示例。

以下例子为天气查询工具的示例

```
@register_tool
def get_weather(
        city_name: Annotated[str, 'The name of the city to be queried', True],
) -> str:
    """
    Get the current weather for `city_name`
    """

    if not isinstance(city_name, str):
        raise TypeError("City name must be a string")

    key_selection = {
        "current_condition": ["temp_C", "FeelsLikeC", "humidity", "weatherDesc", "observation_time"],
    }
    import requests
    try:
        resp = requests.get(f"https://wttr.in/{city_name}?format=j1")
        resp.raise_for_status()
        resp = resp.json()
        ret = {k: {_v: resp[k][0][_v] for _v in v} for k, v in key_selection.items()}
    except:
        import traceback
        ret = "Error encountered while fetching weather data!\n" + traceback.format_exc()

    return str(ret)


if __name__ == "__main__":
    print(dispatch_tool("get_weather", {"city_name": "beijing"}))
    print(get_tools())
```

这个工具的通过向`https://wttr.in/`发送请求以获得最新的天气信息。

这个工具需要一个输入`city_name`，代表需要查询的城市。

返回的是一个字符串`ret`，为经过处理的天气信息，或者错误信息（没有成功调用）。

### 使用构建好的工具

当这些完成之后，我们就可以使用构建好的工具来进行对话了。

我们为开发者提供了`openai_api_demo.py`，这份代码将能帮助用户实现工具的调用

1. 在代码仓库界面，运行`openai_api_demo.py`
2. 接着，运行`tool_using/openai_api_demo.py`

请注意，在`openai_api_demo.py`中，`openai.api_base`需要修改为`openai_api_demo.py`中部署的ip地址和端口。



> 开发者也可以直接在 `openai_demo` 中使用 `openai_api_demo/api_server.py` 来运行完整的OpenAI DEMO，也可以调用工具。

接着，你就能看到以下内容：

```
[registered tool] {'description': 'Generates a random number x, s.t. range[0] <= x < range[1]',
 'name': 'random_number_generator',
 'params': [{'description': 'The random seed used by the generator',
         'name': 'seed',
         'required': True,
         'type': 'int'},
        {'description': 'The range of the generated numbers',
         'name': 'range',
         'required': True,
         'type': 'tuple[int, int]']}]
[registered tool] {'description': 'Get the current weather for `city_name`',
 'name': 'get_weather',
 'params': [{'description': 'The name of the city to be queried',
         'name': 'city_name',
         'required': True,
         'type': 'str'}]}
```

我是一个名为 ChatGLM3-6B 的人工智能助手，是基于清华大学 KEG 实验室和智谱 AI 公司于 2023 年共同训练的语言模型开发的。我的任务是针对用户的问题和要求提供适当的答复和支持。

```
2023-11-09 16:33:43.979 | INFO     | __main__:<module>:88 - 
=========== next conversation ===========
get_weather
```

```python
tool_call(city_name='北京')
```

```
2023-11-09 16:33:44.544 | INFO     | __main__:run_conversation:59 - Function Call Response: {'name': 'get_weather', 'arguments': '{"city_name": "北"}'}
2023-11-09 16:33:45.923 | INFO     | __main__:run_conversation:63 - Tool Call Response: {'current_condition': {'temp_C': '7', 'FeelsLikeC': '3', 'humidity': '12', 'weatherDesc': [{'value': 'Sunny'}], 'observation_time': '04:33 AM'}}
```

根据您的要求，我查询了北京的天气情况。目前北京的气温为7摄氏度，体感温度为3摄氏度，湿度为12%，天气状况为晴朗。这些信息是在04:33 AM时收集的。

这里列出了在 `tool_register.py` 注册的两个工具

由于在 `openai_api_demo.py` 中，用户的提问是写好的，因此你看到的回答是直接基于这个问题的

```
帮我查询北京的天气怎么样
```

## 注意事项

到此为止，你已经成功上手了模型调用。但是，在更多探索之前，请一定要看这部分的内容。这将能为你之后的开发减少不必要的麻烦。

### 工具参数和代码问题

+   ChatGLM3-6B仅能根据工具的介绍和传参要求，判定是否应该使用这个工具并传入正确的参数。

+   ChatGLM3-6B并不能读取工具的具体代码。

+   ChatGLM3-6B仅仅观察工具的返回结果，并不关注工具的执行过程。

+   用户必须传入给ChatGLM3-6B正确的参数，包括正确的数量，正确的类型等内容，减少ChatGLM3-6B的意图识别。否则，很有可能出现模型自己编造参数传入工具的现象。

因此，以下问题的提问方式是不合理的：

1.  使用天气查询工具，需要的参数是地址和时间，但是用户的问题是：

> 你好，今晚热不热？

1.  使用股票价格查询工具，需要股票的代码，在工具实现中是 `Int` 类型，但是用户传入的是：

> 帮我查询A23C4股票的价格

1.  需要传入多个年份的表格查询工具来查询年度的环比增长，但是用户只传入的是：

> 帮我看2023年的年度增长。

### 连续调用和同时调用

+   ChatGLM3-6B仅支持一个行动中调用一个工具，即

```
<|assistant|>调用工具的名称
这个位置只能选择一个工具，不能同时执行多个工具
```

+   ChatGLM系列所有模型，目前有且仅有ChatGLM3-6B一种模型支持工具调用。

+   ChatGLM3-6B模型连续调用工具能力相对于GPT4较弱，建议用户尽量在一个工具内完成所有任务。

### 更多练习

如果你对工具调用较为熟悉，也可以直接在综合demo上进行练习

https://github.com/THUDM/ChatGLM3/tree/main/composite_demo

在本代码中，将对话模式选为 `Tool` 即可。
