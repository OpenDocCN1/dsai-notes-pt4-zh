
# 解决方案概览

通过继承 `BaseChatModel` 创建自定义的 `DeepSeekReasonerChatModel`，实现：

1. 与 `LangChain 1.0` 的 `create_agent` 完全兼容
1. 正确处理 `reasoning_content` 字段
1. 支持完整的工具调用流程
1. 可以查看模型的推理过程

## 创建核心类：`DeepSeekReasonerChatModel`

```
from typing import Optional, List, Dict, Any
from langchain_core.language_models.chat_models import BaseChatModel

class DeepSeekReasonerChatModel(BaseChatModel):
    """
    自定义 DeepSeek Reasoner 模型

    关键特性：
    1. 保留 reasoning_content 在 additional_kwargs 中
    2. 消息转换时恢复 reasoning_content
    3. 完整支持工具绑定和调用
    """

    api_key: str
    base_url: str = "https://api.deepseek.com"
    model_name: str = "deepseek-reasoner"
    temperature: float = 0.7
    bound_tools: Optional[List[Dict]] = None
```

## 创建关键方法：`_convert_messages_to_openai_format()`

```
def _convert_messages_to_openai_format(self, messages):
    """LangChain 消息 → OpenAI 格式"""
    openai_messages = []

    for msg in messages:
        if isinstance(msg, AIMessage):
            msg_dict = {
                "role": "assistant",
                "content": msg.content or "",
            }

            # 添加 tool_calls
            if msg.tool_calls:
                msg_dict["tool_calls"] = [...]

            # 【关键】恢复 reasoning_content
            if 'reasoning_content' in msg.additional_kwargs:
                msg_dict["reasoning_content"] = msg.additional_kwargs['reasoning_content']

            openai_messages.append(msg_dict)

    return openai_messages
```

## 优化响应处理：`_create_ai_message_from_response()`

```
def _create_ai_message_from_response(self, response):
    """OpenAI 响应 → LangChain AIMessage"""
    message = response.choices[0].message

    # 处理 tool_calls
    tool_calls = [...]

    # 【关键】保存 reasoning_content 到 additional_kwargs
    additional_kwargs = {}
    if hasattr(message, 'reasoning_content'):
        additional_kwargs['reasoning_content'] = message.reasoning_content

    return AIMessage(
        content=message.content or "",
        tool_calls=tool_calls if tool_calls else None,  # 不能是 None
        additional_kwargs=additional_kwargs
    )
```

## 进行工具绑定：`bind_tools()`

```
from langchain_core.tools import BaseTool
def bind_tools(self, tools: List[BaseTool], **kwargs):
    """绑定 LangChain 工具"""
    # 转换为 OpenAI 格式
    openai_tools = []
    for tool in tools:
        openai_tools.append({
            "type": "function",
            "function": {
                "name": tool.name,
                "description": tool.description,
                "parameters": tool.args_schema.model_json_schema()
            }
        })

    # 返回新实例
    return self.__class__(
        api_key=self.api_key,
        base_url=self.base_url,
        model_name=self.model_name,
        temperature=self.temperature,
        bound_tools=openai_tools,  # 保存绑定的工具
        **kwargs
    )
```

## 优化生成响应格式：`_generate()`

```
def _generate(self, messages, stop=None, run_manager=None, **kwargs):
    """核心生成方法"""
    # 转换消息
    openai_messages = self._convert_messages_to_openai_format(messages)

    # 准备请求
    request_params = {
        "model": self.model_name,
        "messages": openai_messages,
        "temperature": self.temperature,
    }

    # 添加工具
    if self.bound_tools:
        request_params["tools"] = self.bound_tools

    # 调用 API
    response = self._client.chat.completions.create(**request_params)

    # 创建响应
    ai_message = self._create_ai_message_from_response(response)

    return ChatResult(generations=[ChatGeneration(message=ai_message)])
```

## 实际调用效果

![](img/6eab119b4bbea0416ed4d904dcc8a7c0.png)

![](img/e9dcf43618f2cd05810d63a5e1ceb889.png)

```
import os
from dotenv import load_dotenv
from langchain_core.tools import tool
from langchain.agents import create_agent

load_dotenv()
# 导入自定义模型
from deepseek_reasoner_chat_model import DeepSeekReasonerChatModel
```

```
# 创建自定义模型
model = DeepSeekReasonerChatModel(
    api_key=os.environ.get("DEEPSEEK_API_KEY"),
    model_name="deepseek-reasoner",
    temperature=0.7
)
```

```
tools = [get_weather, write_file]
```

```
agent = create_agent(
    model=model,
    tools=tools,
    system_prompt="你是一个有用的助手，可以使用工具来回答问题。"
)
```

```
result = agent.invoke(
    {"messages": [{"role": "user", "content": "请帮我查询北京天气信息。"}]}
)
```

```
result['messages'][-1].content
```

```
'根据查询结果，北京市当前天气信息如下：\n\n**天气状况：** 晴朗\n**当前温度：** -4.06°C\n**体感温度：** -7.17°C\n**最低/最高温度：** -4.06°C / -4.06°C\n**气压：** 1027 hPa\n**湿度：** 17%\n**风速：** 1.99 m/s（南风）\n**阵风：** 4.32 m/s\n**能见度：** 10公里\n**云量：** 1%\n\n**温馨提示：** 目前北京天气非常寒冷，气温在零下4度左右，体感温度更低（零下7度）。建议外出时穿戴厚实的羽绒服、帽子、手套等防寒衣物，注意保暖防冻。天气晴朗，能见度良好，但风力不大。'
```

```
result['messages']
```

```
[HumanMessage(content='请帮我查询北京天气信息。', additional_kwargs={}, response_metadata={}, id='4266bd07-163f-49d4-bc5a-2faa8baedb62'),
 AIMessage(content='', additional_kwargs={'reasoning_content': '用户想查询北京的天气信息。我需要使用get_weather工具，并传入参数loc="Beijing"。现在来调用这个函数。'}, response_metadata={}, id='lc_run--8ef7f389-8feb-4552-924d-e48a95e3020c-0', tool_calls=[{'name': 'get_weather', 'args': {'loc': 'Beijing'}, 'id': 'call_00_fZSDCV1Ho3wdnExId1TxC9oz', 'type': 'tool_call'}]),
 ToolMessage(content='{"coord": {"lon": 116.3972, "lat": 39.9075}, "weather": [{"id": 800, "main": "Clear", "description": "\u6674", "icon": "01n"}], "base": "stations", "main": {"temp": -4.06, "feels_like": -7.17, "temp_min": -4.06, "temp_max": -4.06, "pressure": 1027, "humidity": 17, "sea_level": 1027, "grnd_level": 1022}, "visibility": 10000, "wind": {"speed": 1.99, "deg": 196, "gust": 4.32}, "clouds": {"all": 1}, "dt": 1764761901, "sys": {"type": 1, "id": 9609, "country": "CN", "sunrise": 1764717520, "sunset": 1764751796}, "timezone": 28800, "id": 1816670, "name": "Beijing", "cod": 200}', name='get_weather', id='8af58243-00c7-49fd-a1be-5046244adb5f', tool_call_id='call_00_fZSDCV1Ho3wdnExId1TxC9oz'),
 AIMessage(content='根据查询结果，北京市当前天气信息如下：\n\n**天气状况：** 晴朗\n**当前温度：** -4.06°C\n**体感温度：** -7.17°C\n**最低/最高温度：** -4.06°C / -4.06°C\n**气压：** 1027 hPa\n**湿度：** 17%\n**风速：** 1.99 m/s（南风）\n**阵风：** 4.32 m/s\n**能见度：** 10公里\n**云量：** 1%\n\n**温馨提示：** 目前北京天气非常寒冷，气温在零下4度左右，体感温度更低（零下7度）。建议外出时穿戴厚实的羽绒服、帽子、手套等防寒衣物，注意保暖防冻。天气晴朗，能见度良好，但风力不大。', additional_kwargs={'reasoning_content': '收到天气数据了。需要向用户提供清晰易懂的天气信息。数据是JSON格式的，包含了气温、天气状况、风速、湿度等信息。气温单位是开尔文（K），但API返回的数据中，main.temp是-4.06，这应该是摄氏度（°C），因为零下温度在开尔文里不会是负值。实际上，OpenWeatherMap默认返回开尔文温度，但这里显示的是-4.06，这很可能是摄氏度。可能是API直接返回了摄氏度，或者工具已经做了转换。从感觉温度（feels_like）也是负值来看，这确实是摄氏度。\n\n现在整理一下信息：\n- 天气状况：Clear（晴朗），描述是中文“晴”\n- 温度：-4.06°C\n- 体感温度：-7.17°C\n- 最低/最高温度：都是-4.06°C（可能是当前温度，没有变化）\n- 气压：1027 hPa\n- 湿度：17%\n- 风速：1.99 m/s，风向196度（南风）\n- 阵风：4.32 m/s\n- 能见度：10000米\n- 云量：1%\n- 数据时间戳：dt: 1764761901（Unix时间戳，需要转换吗？可能不需要）\n- 日出/日落时间：有sunrise和sunset的时间戳\n\n我可以提供这些信息，并给出穿衣建议，比如天气寒冷，建议穿厚衣服。\n\n现在用中文回复用户。'}, response_metadata={}, id='lc_run--c0ae1f80-d9ce-4a84-8fce-0878325ca0cd-0')]
```
