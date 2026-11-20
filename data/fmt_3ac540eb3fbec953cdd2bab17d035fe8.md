
```
ToolCallItem(agent=Agent(name=&#x27;天气查询Agent&#x27;, instructions=&#x27;你是一名助人为乐的助手，并且可以进行天气信息查询&#x27;, handoff_description=None, handoffs=[], model=&lt;agents.models.openai_chatcompletions.OpenAIChatCompletionsModel object at 0x0000015531F87F50&gt;, model_settings=ModelSettings(temperature=None, top_p=None, frequency_penalty=None, presence_penalty=None, tool_choice=None, parallel_tool_calls=False, truncation=None, max_tokens=None), tools=[FunctionTool(name=&#x27;get_weather&#x27;, description=&#x27;查询即时天气函数&#x27;, params_json_schema={&#x27;properties&#x27;: {&#x27;loc&#x27;: {&#x27;description&#x27;: &quot;必要参数，字符串类型，用于表示查询天气的具体城市名称，    注意，中国的城市需要用对应城市的英文名称代替，例如如果需要查询北京市天气，则loc参数需要输入&#x27;Beijing&#x27;；&quot;, &#x27;title&#x27;: &#x27;Loc&#x27;}}, &#x27;required&#x27;: [&#x27;loc&#x27;], &#x27;title&#x27;: &#x27;get_weather_args&#x27;, &#x27;type&#x27;: &#x27;object&#x27;, &#x27;additionalProperties&#x27;: False}, on_invoke_tool=&lt;function function_tool.&lt;locals&gt;._create_function_tool.&lt;locals&gt;._on_invoke_tool at 0x000001553492CAE0&gt;, strict_json_schema=True)], mcp_servers=[], input_guardrails=[], output_guardrails=[], output_type=None, hooks=None, tool_use_behavior=&#x27;run_llm_again&#x27;, reset_tool_choice=True), raw_item=ResponseFunctionToolCall(arguments=&#x27;{&quot;loc&quot;:&quot;Beijing&quot;}&#x27;, call_id=&#x27;call_0_32580549-f497-48a6-8fb1-a17d01bd5870&#x27;, name=&#x27;get_weather&#x27;, type=&#x27;function_call&#x27;, id=&#x27;__fake_id__&#x27;, status=None), type=&#x27;tool_call_item&#x27;)
```

需要注意的是，在进行外部函数调用的时候，`Agents SDK`自动生成的`tools`参数列表，就相当于是Function calling的json schema对象，只不过是从我们定义的外部函数的函数说明中自动读取的：

![](img/aeea7cec1243490866ad519fdee166fb.png)

这也就说明，我们需要在编写外部函数的时候谨慎的编写函数说明，否则会影响外部函数调用的准确率：

此时外部函数请求过程就包括请求的外部函数名称以及相关函数等：

```
weather_result.new_items[0].raw_item
```

```
ResponseFunctionToolCall(arguments=&#x27;{&quot;loc&quot;:&quot;Beijing&quot;}&#x27;, call_id=&#x27;call_0_32580549-f497-48a6-8fb1-a17d01bd5870&#x27;, name=&#x27;get_weather&#x27;, type=&#x27;function_call&#x27;, id=&#x27;__fake_id__&#x27;, status=None)
```

```
weather_result.new_items[0].raw_item.arguments
```

```
&#x27;{&quot;loc&quot;:&quot;Beijing&quot;}&#x27;
```

```
weather_result.new_items[0].raw_item.name
```

```
&#x27;get_weather&#x27;
```

以及一个外部函数响应消息（`ToolCallOutputItem`）：

```
weather_result.new_items[1]
```

```
ToolCallOutputItem(agent=Agent(name=&#x27;天气查询Agent&#x27;, instructions=&#x27;你是一名助人为乐的助手，并且可以进行天气信息查询&#x27;, handoff_description=None, handoffs=[], model=&lt;agents.models.openai_chatcompletions.OpenAIChatCompletionsModel object at 0x0000015531F87F50&gt;, model_settings=ModelSettings(temperature=None, top_p=None, frequency_penalty=None, presence_penalty=None, tool_choice=None, parallel_tool_calls=False, truncation=None, max_tokens=None), tools=[FunctionTool(name=&#x27;get_weather&#x27;, description=&#x27;查询即时天气函数&#x27;, params_json_schema={&#x27;properties&#x27;: {&#x27;loc&#x27;: {&#x27;description&#x27;: &quot;必要参数，字符串类型，用于表示查询天气的具体城市名称，    注意，中国的城市需要用对应城市的英文名称代替，例如如果需要查询北京市天气，则loc参数需要输入&#x27;Beijing&#x27;；&quot;, &#x27;title&#x27;: &#x27;Loc&#x27;}}, &#x27;required&#x27;: [&#x27;loc&#x27;], &#x27;title&#x27;: &#x27;get_weather_args&#x27;, &#x27;type&#x27;: &#x27;object&#x27;, &#x27;additionalProperties&#x27;: False}, on_invoke_tool=&lt;function function_tool.&lt;locals&gt;._create_function_tool.&lt;locals&gt;._on_invoke_tool at 0x000001553492CAE0&gt;, strict_json_schema=True)], mcp_servers=[], input_guardrails=[], output_guardrails=[], output_type=None, hooks=None, tool_use_behavior=&#x27;run_llm_again&#x27;, reset_tool_choice=True), raw_item={&#x27;call_id&#x27;: &#x27;call_0_32580549-f497-48a6-8fb1-a17d01bd5870&#x27;, &#x27;output&#x27;: &#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 804, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u9634\\uff0c\\u591a\\u4e91&quot;, &quot;icon&quot;: &quot;04d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 20.94, &quot;feels_like&quot;: 19.32, &quot;temp_min&quot;: 20.94, &quot;temp_max&quot;: 20.94, &quot;pressure&quot;: 1011, &quot;humidity&quot;: 9, &quot;sea_level&quot;: 1011, &quot;grnd_level&quot;: 1006}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 7.26, &quot;deg&quot;: 319, &quot;gust&quot;: 11.1}, &quot;clouds&quot;: {&quot;all&quot;: 100}, &quot;dt&quot;: 1743492127, &quot;sys&quot;: {&quot;type&quot;: 1, &quot;id&quot;: 9609, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1743458327, &quot;sunset&quot;: 1743503856}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;, &#x27;type&#x27;: &#x27;function_call_output&#x27;}, output=&#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 804, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u9634\\uff0c\\u591a\\u4e91&quot;, &quot;icon&quot;: &quot;04d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 20.94, &quot;feels_like&quot;: 19.32, &quot;temp_min&quot;: 20.94, &quot;temp_max&quot;: 20.94, &quot;pressure&quot;: 1011, &quot;humidity&quot;: 9, &quot;sea_level&quot;: 1011, &quot;grnd_level&quot;: 1006}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 7.26, &quot;deg&quot;: 319, &quot;gust&quot;: 11.1}, &quot;clouds&quot;: {&quot;all&quot;: 100}, &quot;dt&quot;: 1743492127, &quot;sys&quot;: {&quot;type&quot;: 1, &quot;id&quot;: 9609, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1743458327, &quot;sunset&quot;: 1743503856}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;, type=&#x27;tool_call_output_item&#x27;)
```

既外部函数运行结果，也就是查询到的天气结果：

```
weather_result.new_items[1].raw_item
```

```
{&#x27;call_id&#x27;: &#x27;call_0_32580549-f497-48a6-8fb1-a17d01bd5870&#x27;,
 &#x27;output&#x27;: &#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 804, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u9634\\uff0c\\u591a\\u4e91&quot;, &quot;icon&quot;: &quot;04d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 20.94, &quot;feels_like&quot;: 19.32, &quot;temp_min&quot;: 20.94, &quot;temp_max&quot;: 20.94, &quot;pressure&quot;: 1011, &quot;humidity&quot;: 9, &quot;sea_level&quot;: 1011, &quot;grnd_level&quot;: 1006}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 7.26, &quot;deg&quot;: 319, &quot;gust&quot;: 11.1}, &quot;clouds&quot;: {&quot;all&quot;: 100}, &quot;dt&quot;: 1743492127, &quot;sys&quot;: {&quot;type&quot;: 1, &quot;id&quot;: 9609, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1743458327, &quot;sunset&quot;: 1743503856}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;,
 &#x27;type&#x27;: &#x27;function_call_output&#x27;}
```

此外，还包含一条最终模型响应事件（`MessageOutputItem`）：

```
weather_result.new_items[2]
```
