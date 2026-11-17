
# DeepSeek 工具调用（Tool Calls）完整示例

## 一、初始响应（`chat_response`）解析

```text
Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content='我来帮您查询北京今天的天气情况。', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_00_KYFhVE6ASFoaJb8W2zLZgYQJ', function=Function(arguments='{"loc": "Beijing"}', name='get_weather'), type='function', index=0)]))
```

```python
chat_response.choices[0].finish_reason
```

```text
'tool_calls'
```

```python
chat_response.choices[0].message.content
```

```text
'我来帮您查询北京今天的天气情况。'
```

```python
chat_response.choices[0].message.tool_calls
```

```text
[ChatCompletionMessageFunctionToolCall(id='call_00_KYFhVE6ASFoaJb8W2zLZgYQJ', function=Function(arguments='{"loc": "Beijing"}', name='get_weather'), type='function', index=0)]
```

## 二、推理模型响应（`reasoner_response`）解析

```python
reasoner_response = client.chat.completions.create(
    model="deepseek-reasoner",  
    messages=messages,
    tools=tools,
)
```

```python
reasoner_response
```

```text
ChatCompletion(id='d03bff17-261b-4660-95b2-4a50a985eed1', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content='', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_00_ixFHiedGONNUL796DOE549PM', function=Function(arguments='{"loc": "Beijing"}', name='get_weather'), type='function', index=0)], reasoning_content='用户想了解北京今天的天气情况。我需要使用get_weather函数来查询北京的天气。根据函数描述，城市名称需要用英文表示，北京对应的英文是"Beijing"。所以，我应该调用get_weather函数，参数loc设为"Beijing"。现在就来执行这个操作。'))], created=1764659360, model='deepseek-reasoner', object='chat.completion', service_tier=None, system_fingerprint='fp_eaab8d114b_prod0820_fp8_kvcache', usage=CompletionUsage(completion_tokens=102, prompt_tokens=353, total_tokens=455, completion_tokens_details=CompletionTokensDetails(accepted_prediction_tokens=None, audio_tokens=None, reasoning_tokens=57, rejected_prediction_tokens=None), prompt_tokens_details=PromptTokensDetails(audio_tokens=None, cached_tokens=320), prompt_cache_hit_tokens=320, prompt_cache_miss_tokens=33))
```

```python
reasoner_response.choices[0]
```

```text
Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content='', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageFunctionToolCall(id='call_00_ixFHiedGONNUL796DOE549PM', function=Function(arguments='{"loc": "Beijing"}', name='get_weather'), type='function', index=0)], reasoning_content='用户想了解北京今天的天气情况。我需要使用get_weather函数来查询北京的天气。根据函数描述，城市名称需要用英文表示，北京对应的英文是"Beijing"。所以，我应该调用get_weather函数，参数loc设为"Beijing"。现在就来执行这个操作。'))
```

```python
reasoner_response.choices[0].finish_reason
```

```text
'tool_calls'
```

```python
reasoner_response.choices[0].message.reasoning_content
```

```text
'用户想了解北京今天的天气情况。我需要使用get_weather函数来查询北京的天气。根据函数描述，城市名称需要用英文表示，北京对应的英文是"Beijing"。所以，我应该调用get_weather函数，参数loc设为"Beijing"。现在就来执行这个操作。'
```

```python
reasoner_response.choices[0].message.tool_calls
```

```text
[ChatCompletionMessageFunctionToolCall(id='call_00_ixFHiedGONNUL796DOE549PM', function=Function(arguments='{"loc": "Beijing"}', name='get_weather'), type='function', index=0)]
```

## 三、提取并执行工具调用

```python
available_functions = {
            "get_weather": get_weather,
        }
```

```python
function_call_messages = reasoner_response.choices[0].message.tool_calls
function_call_messages
```

```text
[ChatCompletionMessageFunctionToolCall(id='call_00_ixFHiedGONNUL796DOE549PM', function=Function(arguments='{"loc": "Beijing"}', name='get_weather'), type='function', index=0)]
```

```python
len(function_call_messages)
```

```text
1
```

```python
function_call_messages[0]
```

```text
ChatCompletionMessageFunctionToolCall(id='call_00_ixFHiedGONNUL796DOE549PM', function=Function(arguments='{"loc": "Beijing"}', name='get_weather'), type='function', index=0)
```

```python
tool_call = function_call_messages[0]
```

```python
tool_name = tool_call.function.name
tool_args = json.loads(tool_call.function.arguments)
```

```python
tool_name,tool_args
```

```text
('get_weather', {'loc': 'Beijing'})
```

```python
fuction_to_call = available_functions[tool_name]
fuction_to_call
```

```text
<function __main__.get_weather(loc)>
```

```python
function_response = fuction_to_call(**tool_args)
```

```python
function_response
```

```text
'{"coord": {"lon": 116.3972, "lat": 39.9075}, "weather": [{"id": 800, "main": "Clear", "description": "\\u6674", "icon": "01d"}], "base": "stations", "main": {"temp": -1.06, "feels_like": -6.04, "temp_min": -1.06, "temp_max": -1.06, "pressure": 1030, "humidity": 9, "sea_level": 1030, "grnd_level": 1025}, "visibility": 10000, "wind": {"speed": 4.67, "deg": 317, "gust": 10.29}, "clouds": {"all": 0}, "dt": 1764659286, "sys": {"type": 1, "id": 9609, "country": "CN", "sunrise": 1764631062, "sunset": 1764665409}, "timezone": 28800, "id": 1816670, "name": "Beijing", "cod": 200}'
```

## 四、构造函数调用与响应消息

```python
# function call message
reasoner_response.choices[0].message.model_dump()
```

```text
{'content': '',
 'refusal': None,
 'role': 'assistant',
 'annotations': None,
 'audio': None,
 'function_call': None,
 'tool_calls': [{'id': 'call_00_ixFHiedGONNUL796DOE549PM',
   'function': {'arguments': '{"loc": "Beijing"}', 'name': 'get_weather'},
   'type': 'function',
   'index': 0}],
 'reasoning_content': '用户想了解北京今天的天气情况。我需要使用get_weather函数来查询北京的天气。根据函数描述，城市名称需要用英文表示，北京对应的英文是"Beijing"。所以，我应该调用get_weather函数，参数loc设为"Beijing"。现在就来执行这个操作。'}
```

```python
# function response message
{
    "role": "tool",
    "content": function_response,
    "tool_call_id": tool_call.id,
}
```
