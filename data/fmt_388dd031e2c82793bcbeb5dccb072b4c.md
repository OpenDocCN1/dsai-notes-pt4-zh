
# AI 消息输出与 LangGraph React Agent 工具调用示例

```
AIMessage(content='北京今天的天气情况如下：\n\n- **天气状况**：阴，多云\n- **当前温度**：30.22°C\n- **体感温度**：30.33°C\n- **湿度**：43%\n- **气压**：1004 hPa\n- **风速**：6.78 m/s，风向为西南方向（213°）\n- **能见度**：10公里\n\n请注意防晒和补水，尤其是多云天气下紫外线依然较强。', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 99, 'prompt_tokens': 496, 'total_tokens': 595, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 192}, 'prompt_cache_hit_tokens': 192, 'prompt_cache_miss_tokens': 304}, 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_8802369eaa_prod0623_fp8_kvcache', 'id': '00c08d2e-f7aa-4980-b669-cef67c497dca', 'service_tier': None, 'finish_reason': 'stop', 'logprobs': None}, id='run--fd106677-6489-4a64-8935-210072b57014-0', usage_metadata={'input_tokens': 496, 'output_tokens': 99, 'total_tokens': 595, 'input_token_details': {'cache_read': 192}, 'output_token_details': {}})
```

![](img/7681c2b21e28fcc89efc4486f82b3b7b.png)

> 更加完整的`Function calling`执行链路

![](img/ec438eddd64a5712e048024be02693f1.png)

## 3. LangGraph React Agent外部工具响应形式

借助`LangGraph React Agent`，无需任何额外设置，即可完成多工具串联和并联调用。

![](img/5fe20f96b16b3d82ab95548e454d475a.png)

```
response = agent.invoke({"messages": [{"role": "user", "content": "请问北京和杭州今天哪里更热？"}]})
```

```
response
```

```
{'messages': [HumanMessage(content='请问北京和杭州今天哪里更热？', additional_kwargs={}, response_metadata={}, id='61c31341-a015-4991-a828-d26047470913'),
  AIMessage(content='', additional_kwargs={'tool_calls': [{'id': 'call_0_428a28c6-a65c-4bcb-b3d2-4ffd7c5b8c2a', 'function': {'arguments': '{"loc": "Beijing"}', 'name': 'get_weather'}, 'type': 'function', 'index': 0}, {'id': 'call_1_8cfb80ca-94b7-4df8-ae01-f1492250f318', 'function': {'arguments': '{"loc": "Hangzhou"}', 'name': 'get_weather'}, 'type': 'function', 'index': 1}], 'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 41, 'prompt_tokens': 344, 'total_tokens': 385, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 320}, 'prompt_cache_hit_tokens': 320, 'prompt_cache_miss_tokens': 24}, 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_8802369eaa_prod0623_fp8_kvcache', 'id': 'f696ffdf-f12d-444d-9ac4-a5869eb92bb5', 'service_tier': None, 'finish_reason': 'tool_calls', 'logprobs': None}, id='run--0d5b607b-ecea-4076-a1d6-882b4c61875b-0', tool_calls=[{'name': 'get_weather', 'args': {'loc': 'Beijing'}, 'id': 'call_0_428a28c6-a65c-4bcb-b3d2-4ffd7c5b8c2a', 'type': 'tool_call'}, {'name': 'get_weather', 'args': {'loc': 'Hangzhou'}, 'id': 'call_1_8cfb80ca-94b7-4df8-ae01-f1492250f318', 'type': 'tool_call'}], usage_metadata={'input_tokens': 344, 'output_tokens': 41, 'total_tokens': 385, 'input_token_details': {'cache_read': 320}, 'output_token_details': {}}),
  ToolMessage(content='{"coord": {"lon": 116.3972, "lat": 39.9075}, "weather": [{"id": 804, "main": "Clouds", "description": "\\u9634\\uff0c\\u591a\\u4e91", "icon": "04d"}], "base": "stations", "main": {"temp": 28.42, "feels_like": 28.04, "temp_min": 28.42, "temp_max": 28.42, "pressure": 1004, "humidity": 40, "sea_level": 1004, "grnd_level": 999}, "visibility": 10000, "wind": {"speed": 7.24, "deg": 214, "gust": 10.16}, "clouds": {"all": 97}, "dt": 1750930823, "sys": {"country": "CN", "sunrise": 1750884439, "sunset": 1750938415}, "timezone": 28800, "id": 1816670, "name": "Beijing", "cod": 200}', name='get_weather', id='b7b3590b-b843-45e3-85bb-cb5f5a2486eb', tool_call_id='call_0_428a28c6-a65c-4bcb-b3d2-4ffd7c5b8c2a'),
  ToolMessage(content='{"coord": {"lon": 120.1614, "lat": 30.2937}, "weather": [{"id": 501, "main": "Rain", "description": "\\u4e2d\\u96e8", "icon": "10d"}], "base": "stations", "main": {"temp": 31.9, "feels_like": 38.9, "temp_min": 31.9, "temp_max": 31.9, "pressure": 1004, "humidity": 75, "sea_level": 1004, "grnd_level": 1002}, "visibility": 10000, "wind": {"speed": 1.18, "deg": 26, "gust": 3.61}, "rain": {"1h": 1.5}, "clouds": {"all": 95}, "dt": 1750931166, "sys": {"country": "CN", "sunrise": 1750885155, "sunset": 1750935893}, "timezone": 28800, "id": 1808926, "name": "Hangzhou", "cod": 200}', name='get_weather', id='2fe0745d-87e6-4ac7-ac22-29b0411dfd9a', tool_call_id='call_1_8cfb80ca-94b7-4df8-ae01-f1492250f318'),
  AIMessage(content='根据最新的天气数据：\n\n- **北京**：当前温度为 **28.42°C**，天气多云，体感温度为 **28.04°C**，湿度为 **40%**。\n- **杭州**：当前温度为 **31.9°C**，天气中雨，体感温度为 **38.9°C**，湿度为 **75%**。\n\n**结论**：杭州比北京更热，尤其是体感温度更高，且湿度较大。', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 98, 'prompt_tokens': 900, 'total_tokens': 998, 'completion_tokens_details': None, 'prompt_tokens_details': {'audio_tokens': None, 'cached_tokens': 384}, 'prompt_cache_hit_tokens': 384, 'prompt_cache_miss_tokens': 516}, 'model_name': 'deepseek-chat', 'system_fingerprint': 'fp_8802369eaa_prod0623_fp8_kvcache', 'id': '1c59465c-51fe-47d3-9974-8249cbdb1508', 'service_tier': None, 'finish_reason': 'stop', 'logprobs': None}, id='run--a0079b51-a5c2-48b9-bb08-b36ab4c2cc6f-0', usage_metadata={'input_tokens': 900, 'output_tokens': 98, 'total_tokens': 998, 'input_token_details': {'cache_read': 384}, 'output_token_details': {}})]}
```

```
from langchain_core.tools import tool
from pydantic import BaseModel, Field

class Write_Query(BaseModel):
    content: str = Field(description="需要写入文档的具体内容")

@tool(args_schema=Write_Query)
def write_file(content: str) -> str:
    """
    将指定内容写入本地文件。
    :param content: 必要参数，字符串类型，用于表示需要写入文档的具体内容。
    :return：是否成功写入
    """
    return "已成功写入本地文件。"
```

```
# 封装外部函数列表
tools = [get_weather, write_file]
```

```
agent = create_react_agent(model=model, tools=tools)
```
