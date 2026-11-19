
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



```python
from typing import Annotated, TypedDict

class Weather(TypedDict):
    location: str
    date: str
    unit: str
    temperature: int

WeatherType = Annotated[Weather, "A dictionary representing a weather with location, date and unit of temperature"]

def get_current_weather(date: Annotated[str, "the date"], location: Annotated[str, "the location"],
                        unit: Annotated[str, "the unit of temperature"]) -> WeatherType:
    """Get the current weather in a given location"""
    return {
        "location": location,
        "unit": unit,
        "date": date,
        "temperature": 23
    }
```

## 2.2 创建Agent

配置`llm_config`，定义用户和助理`Agent`，这里我想应该无需多言，大家应该都比较熟悉了，注意我们刚才设置的。

```
assistant = ConversableAgent(
    name="Assistant",
    system_message="你是一个强大的助手，你被提供了以下函数，你必须确保你收集到了足够的信息，否则应该提示用户提供缺失的参数，不要构造任何参数，如果任务完成返回TERMINATE",
    llm_config=llm_config,
)

user_proxy = ConversableAgent(
    name="User",
    llm_config=False,
    is_termination_msg=lambda msg: msg.get("content") is not None and "TERMINATE" in msg["content"],
    human_input_mode="ALWAYS",
)
```

## 2.3 函数注册

函数需要注册到助理`Agent`，以便于调用`LLM`接口时候，将函数说明添加上去。同时也需要注册到用户`Agent`，用户`Agent`在收到函数调用时候，进行查找函数并运行。函数注册使用`ConversableAgent`的方法`register_for_llm`，它有以下参数

+   `name` - `str`, 函数名。如果为`None`，函数名称将默认被使用

+   `description` - `str`，函数说明

+   `api_style` - 字面意思，不需要输入，`Azure OpenAI API`使用的，用来版本兼容的。

使用该函数将`get_current_weather`注册到助理`Agent`

```
assistant.register_for_llm(name="get_current_weather", description="Get the current weather in a given location")(get_current_weather)
```

然后使用`register_for_execution`函数将函数注册到用户`Agent`上，它有如下参数

+   `name` - `str` 函数名

```
user_proxy.register_for_execution(name="get_current_weather")(get_current_weather)
```

可能有同学会觉得，这居然需要两个函数进行分别注册，有点重复多余。别急，`AutoGen`是支持使用它们作为装饰器直接定义到可调用的函数上，如下所示。

```
@user_proxy.register_for_execution()
@agent2.register_for_llm()
@agent1.register_for_llm(description="This is a very useful function")
def get_current_weather(....)....
  return ....
```

除此之外，我们还可以使用`autogen`的一级函数`register_function`直接关联双方进行注册, 其实它的内部就是分别调用上述两个函数实现。

```
from autogen import register_function

register_function(
    get_current_weather,
    caller=assistant,
    executor=user_proxy,
    description="Get the current weather in a given location",
)
```

当注册完成后，你可以使用`assistant.llm_config["tools"]`打印生成的`tools`参数，这里就不再显示了。

## 2.4 运行

使用`initial_chat`开始对话。

```
chat_result = user_proxy.initiate_chat(assistant, message="南京天气咋样？")
```

输出如下, 为了显示完整的过程，我将原本的输出进行缩减了，删除了大部分的格式化东西，这样我们可以清楚地看到函数调用的流程。

> User (to Assistant): 南京天气咋样？

> USING AUTO REPLY...

> Assistant (to User): 请问您想查询南京当前的天气状况吗？如果是，请告诉我日期和您希望的温度单位（摄氏度或华氏度）。

> Provide feedback to Assistant. Press enter to skip and use auto-reply, or type 'exit' to end the conversation: 今天

> User (to Assistant): 今天

> USING AUTO REPLY...

> Assistant (to User):

> 好的，那我将查询南京今天的天气，温度单位默认为摄氏度。请稍等。

> ***** Suggested tool call (): `get_current_weather` ***** Arguments: {"date": "2023-04-07", "location": "南京", "unit": "C"}

> Provide feedback to Assistant. Press enter to skip and use auto-reply, or type 'exit' to end the conversation:

> NO HUMAN INPUT RECEIVED.

> USING AUTO REPLY...

> EXECUTING FUNCTION `get_current_weather`...

> User (to Assistant):

> ***** Response from calling tool () ***** {"location": "\u5357\u4eac", "unit": "C", "date": "2023-04-07", "temperature": 23}

> USING AUTO REPLY...

> Assistant (to User): 南京今天的天气温度为23℃。

在其他信息被收集后，温度单位被默认为摄氏度，我觉得是正常的，因为温度单位基本上就摄氏和华氏，大语言模型自动使用摄氏度更是对当前语境的一种理解。

## 3. 总结

本文通过详细的实例，展示了如何使用`AutoGen`简化函数调用的过程，避免了手动构造复杂`HTTP`请求的繁琐工作。通过合理设置`System Prompt`和函数注册，确保模型在函数调用时能够正确收集和使用参数，提高了函数调用的可靠性和准确性。

如果你意犹未尽，想要参与`LangChain`实战课程，可以考虑点击原文购买《`LangChain` 实战：`LLM` 应用开发指南》，亲历`LLM`应用开发之旅。

关注点赞收藏不迷路，获取即时更新。

## 参考资料

[1]`OpenAI Function Calling API`: `https://platform.openai.com/docs/api-reference/chat/create#chat-create-tools`

[2]并发调用: `https://platform.openai.com/docs/guides/function-calling/parallel-function-calling`

[3][`LLM-Agents`]浅析`Agent`工具使用框架：`MM-ReAct`: `https://juejin.cn/user/1398234521286557/posts`
