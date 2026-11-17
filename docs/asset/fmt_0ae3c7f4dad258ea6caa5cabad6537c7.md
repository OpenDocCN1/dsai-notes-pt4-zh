
# 消息构造与模型调用

## 消息列表示例

```
[{'role': 'user', 'content': '请帮我查询北京地区今日天气情况'},
 {'content': '我将调用函数查询北京的天气情况。',
  'refusal': None,
  'role': 'assistant',
  'annotations': None,
  'audio': None,
  'function_call': None,
  'tool_calls': [{'id': 'call_0_08e1ba29-18e9-428d-be3e-1b840c48dff6',
    'function': {'arguments': '{"loc": "Beijing"}', 'name': 'get_weather'},
    'type': 'function',
    'index': 0}],
  'reasoning_content': "我们被要求查询北京地区的天气，注意函数要求：中国的城市需要用对应城市的英文名称代替，北京对应的英文是'Beijing'\n 因此，我们将调用get_weather函数，参数loc为'Beijing'"},
 {'role': 'tool',
  'content': '{"coord": {"lon": 116.3972, "lat": 39.9075}, "weather": [{"id": 803, "main": "Clouds", "description": "\u591a\u4e91", "icon": "04d"}], "base": "stations", "main": {"temp": 29.94, "feels_like": 28.5, "temp_min": 29.94, "temp_max": 29.94, "pressure": 1010, "humidity": 27, "sea_level": 1010, "grnd_level": 1005}, "visibility": 10000, "wind": {"speed": 3.52, "deg": 209, "gust": 5.1}, "clouds": {"all": 58}, "dt": 1748493901, "sys": {"type": 1, "id": 9609, "country": "CN", "sunrise": 1748465380, "sunset": 1748518455}, "timezone": 28800, "id": 1816670, "name": "Beijing", "cod": 200}',
  'tool_call_id': 'call_0_08e1ba29-18e9-428d-be3e-1b840c48dff6'}]
```

## 调用 DeepSeek R1 模型

接下来，再次调用DeepSeek R1模型来围绕`messages`进行回答。需要注意的是，此时我们不再需要向模型重复提问，只需要简单的将我们已经准备好的`messages`传入Chat模型即可：

## 代码调用示例

```
second_response = client.chat.completions.create(
    model="deepseek-reasoner",
    messages=messages,
    tools=tools)
```

## 错误信息与堆栈跟踪

```
---------------------------------------------------------------------------
BadRequestError                           Traceback (most recent call last)

Cell In[54], line 1
----> 1 second_response = client.chat.completions.create(
      2     model="deepseek-reasoner",
      3     messages=messages,
      4     tools=tools)


File c:\Users\snowb\anaconda3\envs\adk\Lib\site-packages\openai\_utils\_utils.py:287, in required_args.<locals>.inner.<locals>.wrapper(*args, **kwargs)
    285             msg = f"Missing required argument: {quote(missing[0])}"
    286     raise TypeError(msg)
---> 287 return func(*args, **kwargs)


File c:\Users\snowb\anaconda3\envs\adk\Lib\site-packages\openai\resources\chat\completions\completions.py:925, in Completions.create(self, messages, model, audio, frequency_penalty, function_call, functions, logit_bias, logprobs, max_completion_tokens, max_tokens, metadata, modalities, n, parallel_tool_calls, prediction, presence_penalty, reasoning_effort, response_format, seed, service_tier, stop, store, stream, stream_options, temperature, tool_choice, tools, top_logprobs, top_p, user, web_search_options, extra_headers, extra_query, extra_body, timeout)
    882 @required_args(["messages", "model"], ["messages", "model", "stream"])
    883 def create(
    884     self,
   (...)    922     timeout: float | httpx.Timeout | None | NotGiven = NOT_GIVEN,
    923 ) -> ChatCompletion | Stream[ChatCompletionChunk]:
    924     validate_response_format(response_format)
---> 925     return self._post(
    926         "/chat/completions",
    927         body=maybe_transform(
    928             {
    929                 "messages": messages,
    930                 "model": model,
    931                 "audio": audio,
    932                 "frequency_penalty": frequency_penalty,
    933                 "function_call": function_call,
    934                 "functions": functions,
    935                 "logit_bias": logit_bias,
    936                 "logprobs": logprobs,
    937                 "max_completion_tokens": max_completion_tokens,
    938                 "max_tokens": max_tokens,
    939                 "metadata": metadata,
    940                 "modalities": modalities,
    941                 "n": n,
    942                 "parallel_tool_calls": parallel_tool_calls,
    943                 "prediction": prediction,
    944                 "presence_penalty": presence_penalty,
    945                 "reasoning_effort": reasoning_effort,
    946                 "response_format": response_format,
    947                 "seed": seed,
    948                 "service_tier": service_tier,
    949                 "stop": stop,
    950                 "store": store,
    951                 "stream": stream,
    952                 "stream_options": stream_options,
    953                 "temperature": temperature,
    954                 "tool_choice": tool_choice,
    955                 "tools": tools,
    956                 "top_logprobs": top_logprobs,
    957                 "top_p": top_p,
    958                 "user": user,
    959                 "web_search_options": web_search_options,
    960             },
    961             completion_create_params.CompletionCreateParamsStreaming
    962             if stream
    963             else completion_create_params.CompletionCreateParamsNonStreaming,
    964         ),
    965         options=make_request_options(
    966             extra_headers=extra_headers, extra_query=extra_query, extra_body=extra_body, timeout=timeout
    967         ),
    968         cast_to=ChatCompletion,
    969         stream=stream or False,
    970         stream_cls=Stream[ChatCompletionChunk],
    971     )


File c:\Users\snowb\anaconda3\envs\adk\Lib\site-packages\openai\_base_client.py:1239, in SyncAPIClient.post(self, path, cast_to, body, options, files, stream, stream_cls)
   1225 def post(
   1226     self,
   (...)   1234     stream_cls: type[_StreamT] | None = None,
   1235 ) -> ResponseT | _StreamT:
   1236     opts = FinalRequestOptions.construct(
   1237         method="post", url=path, json_data=body, files=to_httpx_files(files), **options
   1238     )
---> 1239     return cast(ResponseT, self.request(cast_to, opts, stream=stream, stream_cls=stream_cls)


File c:\Users\snowb\anaconda3\envs\adk\Lib\site-packages\openai\_base_client.py:1034, in SyncAPIClient.request(self, cast_to, options, stream, stream_cls)
   1031             err.response.read()
   1033         log.debug("Re-raising status error")
---> 1034         raise self._make_status_error_from_response(err.response) from None
   1036     break
   1038 assert response is not None, "could not resolve response (should never happen)"


BadRequestError: Error code: 400 - {'error': {'message': 'The reasoning_content is an intermediate result for display purposes only and will not be included in the context for inference. Please remove the reasoning_content from your message to reduce network traffic.', 'type': 'invalid_request_error', 'param': None, 'code': 'invalid_request_error'}}
```
