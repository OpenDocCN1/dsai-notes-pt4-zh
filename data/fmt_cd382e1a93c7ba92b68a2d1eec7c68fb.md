
# 26K Star!LLM多智能体`AutoGen`教程5：函数调用之避免捏造参数

> 来源：[https://agijuejin.feishu.cn/docx/Oq61dE5Vbo2WWqxisKecofdNnRb](https://agijuejin.feishu.cn/docx/Oq61dE5Vbo2WWqxisKecofdNnRb)

> 作者：还就可 | 深入LLM Agent应用开发

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/HDaHeZPnHIFs3D-zFY4o8g)qq.com/s/HDaHeZPn...

书接上回《 [26K Star！LLM多智能体A](http://mp.weixin.qq.com/s?__biz=MzI0OTAzNTEwMw==&mid=2247483853&idx=1&sn=ef0ddcf41eba3ec28afbb7dc6c953e24&chksm=e996e1fddee168eb5c30ad7a79e4db39b24a8bfd0a2597e6bd4c7e40a4b54ebb8632dce5d1dd&scene=21#wechat_redirect)utoGen教程3：我的外包弟弟写代码 》，我们使用`AutoGen`编写一个自动完成代码编写和修改的案例。然而大语言模型生成的代码具有随机性，尤其是比较复杂的功能，或者是非Python类的其他编程的语言时候，尤其是C++，基本上是乱写一通，各种幻觉API。简单的任务已经交给外包小弟了，稍微复杂的功能，还得是我们自己来写。此时，我们就需要LLM的 function calling 功能。在进入`AutoGen`的函数调用功能讲解之前，我们先尝试如何使用OpenAI API进行Function calling。

## 1. 函数调用基础

Open AI的 Function Calling API [1] 可以通过如下HTTP请求完成，需要在请求体中加入 `tools` 字段，它是一个列表，意味着它支持多个函数描述。函数描述采用JSON结构体，包括函数名、函数解释、参数列表，参数列表中每个字段都需要描述类型和解释，如下所示。由于OpenAI Token用完了，我尝试了本地安装的`Llama.cpp`和`Ollama`安装的`Command R Plus`两个模型，他们都是明确不支持函数调用功能。

所以，我测试了通义千问和月之暗面，其中通义千问模型`qwen-max`支持函数调用，只是它不支持OpenAI中提到的 并发调用 [2] 功能，而月之暗面是全面支持函数并发调用。

+ 请求

```bash
curl --location 'https://api.moonshot.cn/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer sk-Mj0Jt4WN2err1ZAKUGVc4PQ8JzbkYajQchywDFlJfXiKZpPO' \
--data '{
    "model": "moonshot-v1-8k",
    "messages": [
        {
            "role": "system",
            "content": "你是一个强大的助手"
        },
        {
            "role": "user",
            "content": "今天南京天气怎么样?"
        }
    ],
    "tools": [
        {
            "type": "function",
            "function": {
                "name": "get_current_weather",
                "description": "Get the current weather in a given location and unit of temperature",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "location": {
                            "type": "string",
                            "description": "The city and state, e.g. San Francisco, CA"
                        },
                        "unit": {
                            "type": "string",
                            "description": "the unit of temperature"
                        }
                    },
                    "required": [
                        "location",
                        "unit"
                    ]
                }
            }
        }
    ]
}'
```

+ 响应

```json
{
    "id": "chatcmpl-bcb1b91facfb419488d85b88d55cbe0a",
    "object": "chat.completion",
    "created": 1717751056,
    "model": "moonshot-v1-8k",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": "",
                "tool_calls": [
                    {
                        "index": 0,
                        "id": "get_current_weather:0",
                        "type": "function",
                        "function": {
                            "name": "get_current_weather",
                            "arguments": "{\n    \"location\": \"南京\",\n    \"unit\": \"C\"\n}"
                        }
                    }
                ]
            },
            "finish_reason": "tool_calls"
        }
    ],
    "usage": {
        "prompt_tokens": 97,
        "completion_tokens": 20,
        "total_tokens": 117
    }
}
```

上述响应有没有问题呢？我们发现它自动填充了摄氏度C。巧的是前几天有个同学在文章 [LLM-Agents]浅析Agent工具使用框架：MM-ReAct [3] 下评论道：

> 我遇到一个问题，Function Calling 总是出现编造参数的情况，这种应该怎么避免？:

> 我用的LangChain Agent 推荐的提示词模板

避免这样的问题，当然是在System Prompt中提示不要自动生成假的参数，这需要不断地尝试提示词，才能避免。通过尝试，我发现使用通义千问，我得把提示词改成这样，他才会每次都会问我缺失的参数。

```json
"messages": [
    {
        "role": "system",
        "content": "你是一个强大的助手，你被提供了以下函数，你必须确保你收集到了足够的信息，否则应该提示用户提供缺失的参数"
    },
    {
        "role": "user",
        "content": "南京天气怎么样?"
    }
],
```

它会响应如下消息，表明它已经认识到我没有提供函数缺失的温度单位和日期。

```json
"message": {
    "role": "assistant",
    "content": "请问您想查询南京的当前天气状况吗？为了提供准确的信息，请告诉我您希望了解日期以及温度单位（摄氏度或华氏度）。"
},
```

此时，我还想继续实验的话，我需要把对话记录填充到请求体的`message`中，`tools`内容基本不变，除了增加一个日期参数之外，考虑篇幅问题就删减了。我将回复的消息放回`messages`中，并增加一条用户的回复。

```json
{
    "model": "qwen-max",
    "messages": [
        {
            "role": "system",
            "content": "你是一个强大的助手，你被提供了以下函数，你必须确保你收集到了足够的信息，否则应该提示用户提供缺失的参数"
        },
        {
            "role": "user",
            "content": "南京天气怎么样?"
        },
        {
            "role": "assistant",
            "content": "请问您想查询南京的当前天气状况吗？为了提供准确的信息，请告诉我您希望了解日期以及温度单位（摄氏度或华氏度）。"
        },
        {
            "role": "user",
            "content": "今天"
        },
        {
            "role": "assistant",
            "content": "好的，我将查询南京今天的天气。请问您希望温度单位使用摄氏度还是华氏度？"
        },
        {
            "role": "user",
            "content": "摄氏度"
        }
    ],
    "temperature": 0.7,
    "tools": [
        ...
    ]
}
```

经过这一系列的处理后，它生成了我想要的响应。

```json
{
    "model": "qwen-max",
    "messages": [
        {
            "role": "system",
            "content": "你是一个强大的助手，你被提供了以下函数，你必须确保你收集到了足够的信息，否则应该提示用户提供缺失的参数"
        },
        {
            "role": "user",
            "content": "南京天气怎么样?"
        },
        {
            "role": "assistant",
            "content": "请问您想查询南京的当前天气状况吗？为了提供准确的信息，请告诉我您希望了解日期以及温度单位（摄氏度或华氏度）。"
        },
        {
            "role": "user",
            "content": "今天"
        },
        {
            "role": "assistant",
            "content": "好的，我将查询南京今天的天气。请问您希望温度单位使用摄氏度还是华氏度？"
        },
        {
            "role": "user",
            "content": "摄氏度"
        }
    ],
    "temperature": 0.7,
    "tools": [
        ...
    ]
}
```

之后的环节里就需要开发者自动解析参数和函数实现函数调用，此处按下不表。

## 2. `AutoGen`中函数调用

从第一节中，可以看到使用HTTP去构造函数调用的请求体是非常繁琐的。好在Python自3.5 引入了类型注解（type hints），我们只需要在需要使用工具调用的函数上对其参数、返回值以及函数本身加上注释说明，`AutoGen`就能帮我们自动生成请求体中的`tools`参数。使用`typing`对 `get_current_weather` 进行注释，如下代码显示。其实经常见到还有使用`Pydantic`进行函数注释，我打算在下一篇对`typing`和`Pydantic`讲解。

### 2.1 函数定义
