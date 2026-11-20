
# 函数调用（Tool Use）多轮对话示例

## 第一步：查询北京天气

### 模型规划与首次工具调用

```
ChatCompletionMessage(content=&#x27;我需要先查询北京的天气，然后将天气信息写入本地文档。接下来，我会分两步操作：\n\n1. 调用 `get_weather` 函数查询北京天气。\n2. 使用 `write_file` 函数将查询结果写入本地文档。\n\n请稍等，我先查询天气。&#x27;, refusal=None, role=&#x27;assistant&#x27;, annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageToolCall(id=&#x27;call_0_c54f765c-d496-432d-8c8e-d1ee6d075b3c&#x27;, function=Function(arguments=&#x27;{&quot;loc&quot;: &quot;Beijing&quot;}&#x27;, name=&#x27;get_weather&#x27;), type=&#x27;function&#x27;, index=0)], reasoning_content=&#x27;我们需要分两步：\n 1. 调用get_weather函数查询北京的天气，注意参数为\&#x27;Beijing\&#x27;\n 2. 将查询到的天气信息通过write_file函数写入本地文档\n 但是注意，我们无法直接调用函数，而是需要按照要求生成调用函数的格式。\n 首先，我们调用get_weather函数获取北京天气，然后得到天气信息后，再调用write_file函数将天气信息写入文件。\n 由于我们无法实际执行函数，所以假设get_weather返回的天气信息是一个字符串，我们直接将其作为write_file的content参数。\n 但是，在调用get_weather时，我们只能得到一次结果，然后将其传递给write_file。\n 因此，我们首先需要调用get_weather，然后根据返回结果再调用write_file。\n 然而，在当前的函数调用机制中，我们只能先规划两个步骤，但注意，我们无法在同一个回复中执行两个函数调用（因为需要等待第一个函数的结果）。\n 所以，我们可以这样：\n 第一步：调用get_weather，获取北京天气。\n 第二步：将第一步返回的天气信息作为write_file的content。\n\n 但是，由于我们的函数调用是依次进行的，所以我们可以先获取天气，然后写入文件。\n\n 但是，注意：我们目前是在一个对话中，所以我们可以先获取天气，然后告诉用户，再写入文件？但是用户要求写入本地文档。\n 不过，根据用户问题，他要求两步：1. 查询天气 2. 将天气信息写入文档。\n\n 因此，我们首先需要获取天气，然后才能写入。所以，我们先调用get_weather，得到结果后再调用write_file。\n\n 但是，在当前的函数调用机制中，我们需要分两次回复？实际上，我们可以一次回复中规划两个函数调用，但需要按顺序。\n\n 然而，OpenAI的API目前不支持在一次回复中连续调用多个函数（除非使用并行工具调用，但这里要求顺序）。所以，通常的做法是：\n 第一次回复：调用第一个函数（get_weather）\n 然后，系统会返回函数调用的结果，我们再将结果作为上下文，再调用第二个函数（write_file）\n\n 但是，我们在这里可以模拟这个过程吗？实际上，在当前的设定中，我们只能生成一次函数调用。所以，我们需要分步骤。\n\n 但是，用户的问题要求两个步骤，所以我们先完成第一步，然后第二步需要第一步的结果。\n\n 因此，我们先调用get_weather，然后得到结果后，再在下一轮调用write_file？这样就需要多轮对话。\n\n 然而，我们可以这样：在第一步，我们调用get_weather，然后我们告诉用户天气，并询问是否写入文件？但是用户已经要求写入文件了。\n\n 所以，我们可能需要两个步骤，但是我们可以通过让模型在第一步之后记住天气信息，然后在第二步再写入。\n\n 但是，我们的系统设计是，每次只执行一个函数调用。所以，我们只能先执行get_weather，然后将结果返回给用户，同时等待用户再次询问时再执行write_file？这样不连贯。\n\n 因此，我们可能需要一个更高级的机制，允许我们链式调用。但是，目前我们只能按步骤来。\n\n 然而，观察用户的问题，他要求查询天气并写入文件。我们可以这样处理：\n\n 1. 先调用get_weather获取北京天气，假设返回结果是字符串&quot;北京今天的天气是...&quot;\n 2. 然后，我们调用write_file，将第一步返回的字符串作为content写入文件。\n\n 但是，在同一个回复中，我们无法同时返回两个函数调用。所以，我们只能先返回第一个函数调用，然后等待函数返回结果后，再在下一轮对话中由模型决定调用第二个函数。\n\n 因此，我们首先调用get_weather，然后得到结果后，我们再自动调用write_file？这需要系统支持。\n\n 但是，在OpenAI的API中，我们可以通过多次交互来实现：第一次我们返回get_weather的调用，然后系统执行并返回结果，然后我们再次生成write_file的调用。\n\n 然而，在当前的对话中，我们只能先完成第一步。\n\n 所以，我们首先调用get_weather，并告诉用户我们将要写入文件，但需要先获取天气。\n\n 但是，用户要求两个步骤，我们能否在一个回复中完成两个函数的调用？答案是不行，因为第二个函数需要第一个函数的结果。\n\n 因此，我们分两步：\n\n 第一步：调用get_weather\n 第二步：在得到天气结果后，调用write_file\n\n 但是，我们如何告诉模型在得到天气结果后自动调用write_file呢？这需要我们在第一步的回复中，除了返回函数调用，还要在回复中暗示下一步的动作。但这样并不直接。\n\n 另一种方法是，我们可以将两个步骤合并成一个计划，然后由外部系统处理链式调用。但是，在OpenAI的API中，我们可以通过多次消息交换来实现。\n\n 具体流程：\n\n 1. 用户提问：请问北京今天天气如何？并将北京天气信息写入本地文档。\n 2. 我们调用get_weather，参数为&quot;Beijing&quot;。\n 3. 系统执行get_weather，返回结果（假设是&quot;Sunny, 25°C&quot;）。\n 4. 我们将结果返回给模型，并提示模型：用户还要求将天气信息写入文件。\n 5. 模型再调用write_file，参数content为&quot;北京今天的天气是：Sunny, 25°C&quot;。\n\n 因此，我们需要两次函数调用。\n\n 但是，在第一次回复中，我们只能调用第一个函数。所以，我们先生成调用get_weather的请求。\n\n 因此，我们这样回复：&#x27;)
```

### 提取工具调用对象

```
response3.choices[0].message.tool_calls
```

```
[ChatCompletionMessageToolCall(id=&#x27;call_0_c54f765c-d496-432d-8c8e-d1ee6d075b3c&#x27;, function=Function(arguments=&#x27;{&quot;loc&quot;: &quot;Beijing&quot;}&#x27;, name=&#x27;get_weather&#x27;), type=&#x27;function&#x27;, index=0)]
```

### 构造函数调用响应消息

```
messages3 = create_function_response_messages(messages3, response3)
```

### 查看更新后的消息列表

```
messages3
```

```
[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;请问北京今天天气如何？并将北京天气信息写入本地文档。&#x27;},
 {&#x27;content&#x27;: &#x27;我需要先查询北京的天气，然后将天气信息写入本地文档。接下来，我会分两步操作：\n\n1. 调用 `get_weather` 函数查询北京天气。\n2. 使用 `write_file` 函数将查询结果写入本地文档。\n\n请稍等，我先查询天气。&#x27;,
  &#x27;refusal&#x27;: None,
  &#x27;role&#x27;: &#x27;assistant&#x27;,
  &#x27;annotations&#x27;: None,
  &#x27;audio&#x27;: None,
  &#x27;function_call&#x27;: None,
  &#x27;tool_calls&#x27;: [{&#x27;id&#x27;: &#x27;call_0_c54f765c-d496-432d-8c8e-d1ee6d075b3c&#x27;,
    &#x27;function&#x27;: {&#x27;arguments&#x27;: &#x27;{&quot;loc&quot;: &quot;Beijing&quot;}&#x27;, &#x27;name&#x27;: &#x27;get_weather&#x27;},
    &#x27;type&#x27;: &#x27;function&#x27;,
    &#x27;index&#x27;: 0}]},
 {&#x27;role&#x27;: &#x27;tool&#x27;,
  &#x27;content&#x27;: &#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 804, &quot;main&quot;: &#x27;Clouds&#x27;, &#x27;description&#x27;: &#x27;\\u9634\\uff0c\\u591a\\u4e91&#x27;, &#x27;icon&#x27;: &#x27;04d&#x27;}], &#x27;base&#x27;: &#x27;stations&#x27;, &#x27;main&#x27;: {&#x27;temp&#x27;: 31.94, &#x27;feels_like&#x27;: 29.87, &#x27;temp_min&#x27;: 31.94, &#x27;temp_max&#x27;: 31.94, &#x27;pressure&#x27;: 1007, &#x27;humidity&#x27;: 19, &#x27;sea_level&#x27;: 1007, &#x27;grnd_level&#x27;: 1002}, &#x27;visibility&#x27;: 10000, &#x27;wind&#x27;: {&#x27;speed&#x27;: 4.07, &#x27;deg&#x27;: 178, &#x27;gust&#x27;: 5.98}, &#x27;clouds&#x27;: {&#x27;all&#x27;: 100}, &#x27;dt&#x27;: 1748511845, &#x27;sys&#x27;: {&#x27;type&#x27;: 1, &#x27;id&#x27;: 9609, &#x27;country&#x27;: &#x27;CN&#x27;, &#x27;sunrise&#x27;: 1748465380, &#x27;sunset&#x27;: 1748518455}, &#x27;timezone&#x27;: 28800, &#x27;id&#x27;: 1816670, &#x27;name&#x27;: &#x27;Beijing&#x27;, &#x27;cod&#x27;: 200}&#x27;,
  &#x27;tool_call_id&#x27;: &#x27;call_0_c54f765c-d496-432d-8c8e-d1ee6d075b3c&#x27;}]
```

## 第二步：生成天气总结并写入文件

### 调用模型生成天气总结

```
response3 = client.chat.completions.create(
    model=&quot;deepseek-reasoner&quot;,  
    messages=messages3,
    tools=tools,
)
```

### 查看模型响应对象

```
response3.choices[0].message
```

### 模型返回天气总结与写入文件调用

```
ChatCompletionMessage(content=&#x27;根据查询结果，北京今天的天气情况如下：\n\n- **天气状况**：阴天，多云\n- **当前温度**：31.94°C\n- **体感温度**：29.87°C\n- **湿度**：19%\n- **风速**：4.07 m/s\n- **气压**：1007 hPa\n- **能见度**：10000米\n\n接下来我会将天气信息写入本地文档。&#x27;, refusal=None, role=&#x27;assistant&#x27;, annotations=None, audio=None, function_call=None, tool_calls=[ChatCompletionMessageToolCall(id=&#x27;call_0_d7eb65c4-c60a-419e-a68e-4c06d19e67f9&#x27;, function=Function(arguments=&#x27;{&quot;content&quot;:&quot;北京今天的天气情况：\\n- 天气状况：阴天，多云\\n- 当前温度：31.94°C\\n- 体感温度：29.87°C\\n- 湿度：19%\\n- 风速：4.07 m/s\\n- 气压：1007 hPa\\n- 能见度：10000米&quot;}&#x27;, name=&#x27;write_file&#x27;), type=&#x27;function&#x27;, index=0)], reasoning_content=&#x27;我们得到了北京的天气数据，现在需要从中提取关键信息，然后写入文件。\n 天气数据中，我们关心的信息包括：天气描述（weather[0].description）、温度（main.temp）、体感温度（main.feels_like）、湿度（main.humidity）、风速（wind.speed）等。\n 根据返回的数据，我们可以构建一个字符串来概括这些信息。\n 然后调用write_file函数将这个字符串写入文件。&#x27;)
```

### 查看第二次工具调用

```
response3.choices[0].message.tool_calls
```

```
[ChatCompletionMessageToolCall(id=&#x27;call_0_d7eb65c4-c60a-419e-a68e-4c06d19e67f9&#x27;, function=Function(arguments=&#x27;{&quot;content&quot;:&quot;北京今天的天气情况：\\n- 天气状况：阴天，多云\\n- 当前温度：31.94°C\\n- 体感温度：29.87°C\\n- 湿度：19%\\n- 风速：4.07 m/s\\n- 气压：1007 hPa\\n- 能见度：10000米&quot;}&#x27;, name=&#x27;write_file&#x27;), type=&#x27;function&#x27;, index=0)]
```

### 检查调用完成状态

```
response3.choices[0].finish_reason
```

```
&#x27;tool_calls&#x27;
```

### 更新对话历史

```
messages3 = create_function_response_messages(messages3, response3)
```

### 查看最终消息列表

```
messages3
```
