
```&#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 804, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u9634\\uff0c\\u591a\\u4e91&quot;, &quot;icon&quot;: &quot;04d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 23.94, &quot;feels_like&quot;: 23.75, &quot;temp_min&quot;: 23.94, &quot;temp_max&quot;: 23.94, &quot;pressure&quot;: 998, &quot;humidity&quot;: 52, &quot;sea_level&quot;: 998, &quot;grnd_level&quot;: 994}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 1.65, &quot;deg&quot;: 283, &quot;gust&quot;: 3.3}, &quot;clouds&quot;: {&quot;all&quot;: 97}, &quot;dt&quot;: 1718964106, &quot;sys&quot;: {&quot;type&quot;: 1, &quot;id&quot;: 9609, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1718916362, &quot;sunset&quot;: 1718970378}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;
```

并将其添加至原始消息列表中：

```
func_call_response.content[1].id
```

```
&#x27;toolu_018aBKVYTfuiiw7KLDKwRbwc&#x27;
```

```
function_response_message = {
  &quot;role&quot;: &quot;user&quot;,
  &quot;content&quot;: [
    {
      &quot;type&quot;: &quot;tool_result&quot;,
      &quot;tool_use_id&quot;: func_call_response.content[1].id,
      &quot;content&quot;: function_response
    }
  ]
}
```

+ 创建`second_response_message`

接下来，再次调用Chat模型来围绕`messages`进行回答。需要注意的是，此时我们不再需要向模型重复提问，只需要简单的将我们已经准备好的`messages`传入Chat模型即可：

```
messages
```

```
[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;请问今天北京天气如何？&#x27;},
 {&#x27;role&#x27;: &#x27;assistant&#x27;,
  &#x27;content&#x27;: [{&#x27;type&#x27;: &#x27;text&#x27;,
    &#x27;text&#x27;: &#x27;好的,我可以帮您查询北京的天气情况。为了获取准确的天气信息,我需要使用天气查询工具。请允许我为您查询。&#x27;},
   {&#x27;type&#x27;: &#x27;tool_use&#x27;,
    &#x27;id&#x27;: &#x27;toolu_018aBKVYTfuiiw7KLDKwRbwc&#x27;,
    &#x27;name&#x27;: &#x27;get_weather&#x27;,
    &#x27;input&#x27;: {&#x27;loc&#x27;: &#x27;Beijing&#x27;}}]},
 {&#x27;role&#x27;: &#x27;user&#x27;,
  &#x27;content&#x27;: [{&#x27;type&#x27;: &#x27;tool_result&#x27;,
    &#x27;tool_use_id&#x27;: &#x27;toolu_018aBKVYTfuiiw7KLDKwRbwc&#x27;,
    &#x27;content&#x27;: &#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 804, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u9634\\uff0c\\u591a\\u4e91&quot;, &quot;icon&quot;: &quot;04d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 23.94, &quot;feels_like&quot;: 23.75, &quot;temp_min&quot;: 23.94, &quot;temp_max&quot;: 23.94, &quot;pressure&quot;: 998, &quot;humidity&quot;: 52, &quot;sea_level&quot;: 998, &quot;grnd_level&quot;: 994}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 1.65, &quot;deg&quot;: 283, &quot;gust&quot;: 3.3}, &quot;clouds&quot;: {&quot;all&quot;: 97}, &quot;dt&quot;: 1718964106, &quot;sys&quot;: {&quot;type&quot;: 1, &quot;id&quot;: 9609, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1718916362, &quot;sunset&quot;: 1718970378}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;}]}]
```

```
final_response = client.messages.create(
    model=&quot;claude-3-5-sonnet-20240620&quot;,
    max_tokens=1024,
    tools=get_weather_tool,
    messages=messages,
)
```

```
final_response
```

```
Message(id=&#x27;msg_01WKiZipB6rVQ1xn9wy6Qvq3&#x27;, content=[TextBlock(text=&#x27;根据查询结果，我可以为您提供北京今天的天气情况：\n\n1. 天气状况：多云（阴天）\n2. 温度：目前气温为23.94°C（约24°C）\n3. 体感温度：23.75°C\n4. 湿度：52%\n5. 风速：1.65米/秒，大约是微风\n6. 能见度：10公里，属于良好\n7. 气压：998百帕\n\n总的来说，今天北京的天气是多云的，温度适中，湿度适宜，风力较小。虽然天空较为阴沉，但能见度良好。建议您外出时可能需要带一件薄外套，以防温度变化。如果您有户外活动计划，目前的天气状况还是比较适宜的，但要注意随时关注天气变化。\n\n您还有其他关于天气的问题吗？或者需要了解其他方面的信息吗？&#x27;, type=&#x27;text&#x27;)], model=&#x27;claude-3-5-sonnet-20240620&#x27;, role=&#x27;assistant&#x27;, stop_reason=&#x27;end_turn&#x27;, stop_sequence=None, type=&#x27;message&#x27;, usage=Usage(input_tokens=928, output_tokens=289))
```

```
final_response.content[0].text
```

```
&#x27;根据查询结果，我可以为您提供北京今天的天气情况：\n\n1. 天气状况：多云（阴天）\n2. 温度：目前气温为23.94°C（约24°C）\n3. 体感温度：23.75°C\n4. 湿度：52%\n5. 风速：1.65米/秒，大约是微风\n6. 能见度：10公里，属于良好\n7. 气压：998百帕\n\n总的来说，今天北京的天气是多云的，温度适中，湿度适宜，风力较小。虽然天空较为阴沉，但能见度良好。建议您外出时可能需要带一件薄外套，以防温度变化。如果您有户外活动计划，目前的天气状况还是比较适宜的，但要注意随时关注天气变化。\n\n您还有其他关于天气的问题吗？或者需要了解其他方面的信息吗？&#x27;
```

+ 定义可调用外部工具的对话函数

```
def run_conv(question, 
             tools=None, 
             functions_list=None,
             model=&quot;claude-3-5-sonnet-20240620&quot;):
    &quot;&quot;&quot;
    能够自动执行外部函数调用的Chat对话模型
    :return：Chat模型输出结果
    &quot;&quot;&quot;
    
    user_messages = [
        {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: [
                {
                    &quot;type&quot;: &quot;text&quot;,
                    &quot;text&quot;: question
                }
            ]
        }
    ]
    
    # 如果没有外部函数库，则执行普通的对话任务
    if tools == None:
        response = client.messages.create(
            model=model,  
            max_tokens=1000,
            messages=user_messages
        )
        final_response = response.content[0].text
        
    # 若存在外部函数库，则需要灵活选取外部函数并进行回答
    else:
        # 创建外部函数库字典
        available_functions = {func.__name__: func for func in functions_list}

        # 创建包含用户问题的message
        messages = user_messages
        
        # first response
        response = client.messages.create(
            model=model,  
            max_tokens=1024,
            messages=user_messages,
            tools=tools,
        )
        response_message = response

        # 获取函数名
        function_name = response_message.content[1].name
        # 获取函数对象
        fuction_to_call = available_functions[function_name]
        # 获取函数参数
        function_args = response_message.content[1].input
        
        # 将函数参数输入到函数中，获取函数计算结果
        function_response = fuction_to_call(**function_args)

        # messages中拼接first response消息
        ss = {
            &quot;role&quot;: &quot;assistant&quot;,
            &quot;content&quot;: [
                {
                    &quot;type&quot;: &quot;text&quot;,
                    &quot;text&quot;: response_message.content[0].text
                },
                {
                    &quot;type&quot;: &quot;tool_use&quot;,
                    &quot;id&quot;: response_message.content[1].id,
                    &quot;name&quot;: response_message.content[1].name,
                    &quot;input&quot;: response_message.content[1].input
                }
            ]
        }
        user_messages.append(ss)
        
        # messages中拼接外部函数输出结果
        function_response_message = {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: [
              {
                &quot;type&quot;: &quot;tool_result&quot;,
                &quot;tool_use_id&quot;: response_message.content[1].id,
                &quot;content&quot;: function_response
              }
            ]
          }
        
        user_messages.append(function_response_message)

        
        # 第二次调用模型
        second_response = client.messages.create(
            model=model,
            max_tokens=1024,
            tools=tools,
            messages=user_messages)
            
        # 获取最终结果
        final_response = second_response.content[0].text
    
    return final_response
```
