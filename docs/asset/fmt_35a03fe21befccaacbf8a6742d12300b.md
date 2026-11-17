
```
&#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 804, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u9634\\uff0c\\u591a\\u4e91&quot;, &quot;icon&quot;: &quot;04d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 4.94, &quot;feels_like&quot;: 1.77, &quot;temp_min&quot;: 4.94, &quot;temp_max&quot;: 4.94, &quot;pressure&quot;: 1020, &quot;humidity&quot;: 25, &quot;sea_level&quot;: 1020, &quot;grnd_level&quot;: 1014}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 4.03, &quot;deg&quot;: 300, &quot;gust&quot;: 9.43}, &quot;clouds&quot;: {&quot;all&quot;: 85}, &quot;dt&quot;: 1736239986, &quot;sys&quot;: {&quot;type&quot;: 1, &quot;id&quot;: 9609, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1736206561, &quot;sunset&quot;: 1736240684}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;
```

```
# 追加function返回消息
messages.append({
            &quot;role&quot;: &quot;tool&quot;,
            &quot;content&quot;: function_response,
            &quot;tool_call_id&quot;:response_message.tool_calls[0].id
        })
```

`messages`

```
[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;请帮我查询北京地区今日天气情况&#x27;},
 {&#x27;content&#x27;: &#x27;&#x27;,
  &#x27;refusal&#x27;: None,
  &#x27;role&#x27;: &#x27;assistant&#x27;,
  &#x27;audio&#x27;: None,
  &#x27;function_call&#x27;: None,
  &#x27;tool_calls&#x27;: [{&#x27;id&#x27;: &#x27;call_0_87e140df-66ef-44dd-813f-45800e169690&#x27;,
    &#x27;function&#x27;: {&#x27;arguments&#x27;: &#x27;{&quot;loc&quot;:&quot;Beijing&quot;}&#x27;, &#x27;name&#x27;: &#x27;get_weather&#x27;},
    &#x27;type&#x27;: &#x27;function&#x27;,
    &#x27;index&#x27;: 0}]},
 {&#x27;role&#x27;: &#x27;tool&#x27;,
  &#x27;content&#x27;: &#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 804, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u9634\\uff0c\\u591a\\u4e91&quot;, &quot;icon&quot;: &quot;04d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 4.94, &quot;feels_like&quot;: 1.77, &quot;temp_min&quot;: 4.94, &quot;temp_max&quot;: 4.94, &quot;pressure&quot;: 1020, &quot;humidity&quot;: 25, &quot;sea_level&quot;: 1020, &quot;grnd_level&quot;: 1014}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 4.03, &quot;deg&quot;: 300, &quot;gust&quot;: 9.43}, &quot;clouds&quot;: {&quot;all&quot;: 85}, &quot;dt&quot;: 1736239986, &quot;sys&quot;: {&quot;type&quot;: 1, &quot;id&quot;: 9609, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1736206561, &quot;sunset&quot;: 1736240684}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;,
  &#x27;tool_call_id&#x27;: &#x27;call_0_87e140df-66ef-44dd-813f-45800e169690&#x27;}]
```

接下来，再次调用Chat模型来围绕`messages`进行回答。需要注意的是，此时我们不再需要向模型重复提问，只需要简单的将我们已经准备好的`messages`传入Chat模型即可：

```
second_response = client.chat.completions.create(
    model=&quot;deepseek-chat&quot;,
    messages=messages)
```

`second_response.choices[0].message`

```
ChatCompletionMessage(content=&#x27;北京地区今日天气情况如下：\n\n- **天气状况**：阴，多云\n- **当前温度**：4.94°C\n- **体感温度**：1.77°C\n- **湿度**：25%\n- **风速**：4.03米/秒\n- **风向**：300度（西北风）\n- **能见度**：10000米\n- **气压**：1020 hPa\n\n请注意保暖，适当增添衣物！&#x27;, refusal=None, role=&#x27;assistant&#x27;, audio=None, function_call=None, tool_calls=None)
```

能够发现，模型最终做出了准确回答。

`second_response.choices[0].message.content`

```
&#x27;北京地区今日天气情况如下：\n\n- **天气状况**：阴，多云\n- **当前温度**：4.94°C\n- **体感温度**：1.77°C\n- **湿度**：25%\n- **风速**：4.03米/秒\n- **风向**：300度（西北风）\n- **能见度**：10000米\n- **气压**：1020 hPa\n\n请注意保暖，适当增添衣物！&#x27;
```

`tools`

```
[{&#x27;type&#x27;: &#x27;function&#x27;,
  &#x27;function&#x27;: {&#x27;name&#x27;: &#x27;get_weather&#x27;,
   &#x27;description&#x27;: &#x27;查询即时天气函数，根据输入的城市名称，查询对应城市的实时天气&#x27;,
   &#x27;parameters&#x27;: {&#x27;type&#x27;: &#x27;object&#x27;,
    &#x27;properties&#x27;: {&#x27;loc&#x27;: {&#x27;description&#x27;: &quot;城市名称，注意，中国的城市需要用对应城市的英文名称代替，例如如果需要查询北京市天气，则loc参数需要输入&#x27;Beijing&#x27;&quot;,
      &#x27;type&#x27;: &#x27;string&#x27;}},
    &#x27;required&#x27;: [&#x27;loc&#x27;]}}}]
```

`get_weather`

```
&lt;function __main__.get_weather(loc)&gt;
```

`ds_api_key`

```
&#x27;sk-77a327d8100340b48cfadf704a41c23b&#x27;
```

```
def run_conv(messages, 
             api_key,
             tools=None, 
             functions_list=None,
             model=&quot;deepseek-chat&quot;):
    &quot;&quot;&quot;
    能够自动执行外部函数调用的Chat对话模型
    :param messages: 必要参数，输入到Chat模型的messages参数对象
    :param api_key: 必要参数，调用模型的API-KEY
    :param tools: 可选参数，默认为None，可以设置为包含全部外部函数的列表对象
    :param model: Chat模型，可选参数，默认模型为deepseek-chat
    :return：Chat模型输出结果
    &quot;&quot;&quot;
    user_messages = messages
    
    client = OpenAI(api_key=api_key, 
                base_url=&quot;https://api.deepseek.com&quot;)
    
    # 如果没有外部函数库，则执行普通的对话任务
    if tools == None:
        response = client.chat.completions.create(
            model=model,  
            messages=user_messages
        )
        final_response = response.choices[0].message.content
        
    # 若存在外部函数库，则需要灵活选取外部函数并进行回答
    else:
        # 创建外部函数库字典
        available_functions = {func.__name__: func for func in functions_list}

        # 创建包含用户问题的message
        messages = user_messages
        
        # first response
        response = client.chat.completions.create(
            model=model,  
            messages=user_messages,
            tools=tools,
        )
        response_message = response.choices[0].message

        # 获取函数名
        function_name = response_message.tool_calls[0].function.name
        # 获取函数对象
        fuction_to_call = available_functions[function_name]
        # 获取函数参数
        function_args = json.loads(response_message.tool_calls[0].function.arguments)
        
        # 将函数参数输入到函数中，获取函数计算结果
        function_response = fuction_to_call(**function_args)

        # messages中拼接first response消息
        user_messages.append(response_message.model_dump())  
        
        # messages中拼接外部函数输出结果
        user_messages.append(
            {
                &quot;role&quot;: &quot;tool&quot;,
                &quot;content&quot;: function_response,
                &quot;tool_call_id&quot;:response_message.tool_calls[0].id
            }
        )
        
        # 第二次调用模型
        second_response = client.chat.completions.create(
            model=model,
            messages=user_messages)
            
        # 获取最终结果
        final_response = second_response.choices[0].message.content
    
    return final_response
```

```
messages = [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问什么是机器学习？&quot;}]
```

```
run_conv(messages=messages, 
         api_key = ds_api_key)
```
