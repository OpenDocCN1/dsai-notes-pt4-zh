
# Agent 工具能力微调

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/QPmYdUGzfoV6Q8xtDLIcGB3fnee](https://qqs7y1hozd1.feishu.cn/docx/QPmYdUGzfoV6Q8xtDLIcGB3fnee)

# OpenAI Function Calling

## 1.1 介绍

为了让大语言模型连接到外部工具，OpenAI 推出了 Function calling 的功能。在调用 OpenAI 的 [API](https://platform.openai.com/docs/guides/function-calling) 时，可以描述函数并让模型智能地选择要输出的 JSON 对象，其中包含传递给一个或多个函数的参数。更多信息可以参考：https://platform.openai.com/docs/guides/function-calling。

Chat Completions 的相关 API 并不会调用函数；相反，我们可以在自己的代码中根据模型的输出来实现调用函数的逻辑。大体工作流程如下：

![](img/bffb2e167b191c6c7d0b2484b3b9d5e3.png)

其中，我们将对话数据和工具描述传递给 API 模型。在得到 API 模型的输出后，我们在本地根据输出调用函数，最终得到结果

## 1.2 数据格式

在本节中，我们将介绍 OpenAI Function Calling 所规定的数据格式，以便于使用 XTuner 进行微调时理解数据的结构

### 1.2.1 对话部分

```
messages = [
    {
        "role": "user",
        "content": "What's the weather like in San Francisco, Tokyo, and Paris?"
    }
]
```

如上所示，这是一个简单的对话数据，包含 `role` 和 `content` 两个字段，分别表示输入角色和输入内容

### 1.2.2 工具描述部分

```
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city and state, e.g. San Francisco, CA",
                    },
                    "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
                },
                "required": ["location"],
            },
        },
    }
]
```

如上所示是 OpenAI Function Calling 的工具描述部分。各字段描述如下：

![](img/2215b341023239ae68d78fc631c16271.png)

可以看到，工具描述部分详细地描述了函数的名词和输入参数信息，以便于模型能够智能地选择要调用的函数，并且传入正确的参数
