
# 天气查询智能体示例

## 基础函数调用

```
&#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 801, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u6674\\uff0c\\u5c11\\u4e91&quot;, &quot;icon&quot;: &quot;02d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 22.72, &quot;feels_like&quot;: 21.83, &quot;temp_min&quot;: 22.72, &quot;temp_max&quot;: 22.94, &quot;pressure&quot;: 1020, &quot;humidity&quot;: 30, &quot;sea_level&quot;: 1020, &quot;grnd_level&quot;: 1014}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 2.16, &quot;deg&quot;: 171, &quot;gust&quot;: 3.62}, &quot;clouds&quot;: {&quot;all&quot;: 16}, &quot;dt&quot;: 1728723905, &quot;sys&quot;: {&quot;type&quot;: 2, &quot;id&quot;: 2000403, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1728685284, &quot;sunset&quot;: 1728726003}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;
```

```
agent = Agent(
   functions=[get_weather]
)
```

```
response = swarm_client.run(
   agent=agent,
   messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问今天北京天气如何？&quot;}],
)
```

```
response.messages
```

```
[{&#x27;content&#x27;: None,
  &#x27;refusal&#x27;: None,
  &#x27;role&#x27;: &#x27;assistant&#x27;,
  &#x27;function_call&#x27;: None,
  &#x27;tool_calls&#x27;: [{&#x27;id&#x27;: &#x27;call_ycqeIEqskKdxG7RPyHloN87K&#x27;,
    &#x27;function&#x27;: {&#x27;arguments&#x27;: &#x27;{&quot;loc&quot;:&quot;Beijing&quot;}&#x27;, &#x27;name&#x27;: &#x27;get_weather&#x27;},
    &#x27;type&#x27;: &#x27;function&#x27;}],
  &#x27;sender&#x27;: &#x27;Agent&#x27;},
 {&#x27;role&#x27;: &#x27;tool&#x27;,
  &#x27;tool_call_id&#x27;: &#x27;call_ycqeIEqskKdxG7RPyHloN87K&#x27;,
  &#x27;tool_name&#x27;: &#x27;get_weather&#x27;,
  &#x27;content&#x27;: &#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 801, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u6674\\uff0c\\u5c11\\u4e91&quot;, &quot;icon&quot;: &quot;02d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 22.72, &quot;feels_like&quot;: 21.83, &quot;temp_min&quot;: 22.72, &quot;temp_max&quot;: 22.94, &quot;pressure&quot;: 1020, &quot;humidity&quot;: 30, &quot;sea_level&quot;: 1020, &quot;grnd_level&quot;: 1014}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 2.16, &quot;deg&quot;: 171, &quot;gust&quot;: 3.62}, &quot;clouds&quot;: {&quot;all&quot;: 16}, &quot;dt&quot;: 1728724218, &quot;sys&quot;: {&quot;type&quot;: 2, &quot;id&quot;: 2000403, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1728685284, &quot;sunset&quot;: 1728726003}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;},
 {&#x27;content&#x27;: &#x27;今天北京的天气情况如下：\n\n- 天气：少云（晴，有少量云）\n- 当前温度：22.7°C\n- 体感温度：21.8°C\n- 最低气温：22.7°C，最高气温：22.9°C\n- 气压：1020 hPa\n- 湿度：30%\n- 能见度：10000米\n- 风速：2.16米/秒，风向：171°（南偏东方向）\n- 云量：16%\n\n总体来说，今天北京的天气比较好，适合外出活动。&#x27;,
  &#x27;refusal&#x27;: None,
  &#x27;role&#x27;: &#x27;assistant&#x27;,
  &#x27;function_call&#x27;: None,
  &#x27;tool_calls&#x27;: None,
  &#x27;sender&#x27;: &#x27;Agent&#x27;}]
```

```
display(Markdown(response.messages[-1][&#x27;content&#x27;]))
```

今天北京的天气情况如下：

- 天气：少云（晴，有少量云）
- 当前温度：22.7°C
- 体感温度：21.8°C
- 最低气温：22.7°C，最高气温：22.9°C
- 气压：1020 hPa
- 湿度：30%
- 能见度：10000米
- 风速：2.16米/秒，风向：171°（南偏东方向）
- 云量：16%

总体来说，今天北京的天气比较好，适合外出活动。

![](img/31e1d2881d65b735acdc53c1ddf4f0e3.png)

## 多轮对话效果展示

```
run_demo_loop(openai_client = client, 
              starting_agent = agent, 
              stream = True)
```

```
Starting Swarm CLI 🐝
Type &quot;exit&quot; or &quot;quit&quot; to leave the chat.


[90mUser[0m:  你好呀，好久不见！


[94mAgent:[0m 你好！很高兴再次见到你。最近怎么样？有什么我可以帮忙的吗？


[90mUser[0m:  请问今天北京天气如何？


[94mAgent: [95mget_weather[0m()
[94mAgent:[0m 今天北京的天气情况如下：

- 天气状况：多云
- 当前温度：17.9°C
- 体感温度：16.6°C
- 气压：1020 hPa
- 湿度：30%
- 风速：1.83 m/s，风向为163°
- 能见度：10000米

总体来说，今天的北京是一个多云的天气，而且温度适中。如果你计划外出，请根据天气情况做好准备。


[90mUser[0m:  那杭州呢？天气如何呀


[94mAgent: [95mget_weather[0m()
[94mAgent:[0m 今天杭州的天气情况如下：

- 天气状况：多云
- 当前温度：22.9°C
- 体感温度：23.4°C
- 气压：1019 hPa
- 湿度：81%
- 风速：4.38 m/s，风向为86°
- 能见度：10000米

杭州今天的天气比较温暖，湿度较高，风速也有点大，可能会有些风。希望这些信息能帮助你更好地安排今天的计划！


[90mUser[0m:  北京好像比杭州冷很多


[94mAgent:[0m 确实如此。当前的天气数据显示，北京的温度约为 17.9°C，而杭州的温度是 22.9°C。北京的温度相对较低，空气也比杭州更干燥；而杭州不仅温度高一些，湿度也更大一些。这种差异可能受地理位置和天气系统的影响。如果你有出行计划，记得根据目的地的天气情况来调整衣物和安排哦！


[90mUser[0m:  exit


Exiting chat. Goodbye!
```

## 自动纠错机制

此外，如果智能体函数调用出现错误（如缺少函数、错误的参数、运行错误等），错误响应将被附加到对话中，以便智能体能够流畅的进行 `debug`。如果智能体调用了多个函数，它们将按照定义的顺序执行。

```
agent = Agent(
   functions=[get_weather]
)
```

```
response = swarm_client.run(
   agent=agent,
   messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问今天背景天气如何？&quot;}],
)
```

```
response.messages
```

```
[{&#x27;content&#x27;: None,
  &#x27;refusal&#x27;: None,
  &#x27;role&#x27;: &#x27;assistant&#x27;,
  &#x27;function_call&#x27;: None,
  &#x27;tool_calls&#x27;: [{&#x27;id&#x27;: &#x27;call_SRdg8BOIwqjkaWD9BODOlb7t&#x27;,
    &#x27;function&#x27;: {&#x27;arguments&#x27;: &#x27;{&quot;loc&quot;:&quot;Beijing&quot;}&#x27;, &#x27;name&#x27;: &#x27;get_weather&#x27;},
    &#x27;type&#x27;: &#x27;function&#x27;}],
  &#x27;sender&#x27;: &#x27;Agent&#x27;},
 {&#x27;role&#x27;: &#x27;tool&#x27;,
  &#x27;tool_call_id&#x27;: &#x27;call_SRdg8BOIwqjkaWD9BODOlb7t&#x27;,
  &#x27;tool_name&#x27;: &#x27;get_weather&#x27;,
  &#x27;content&#x27;: &#x27;{&quot;coord&quot;: {&quot;lon&quot;: 116.3972, &quot;lat&quot;: 39.9075}, &quot;weather&quot;: [{&quot;id&quot;: 801, &quot;main&quot;: &quot;Clouds&quot;, &quot;description&quot;: &quot;\\u6674\\uff0c\\u5c11\\u4e91&quot;, &quot;icon&quot;: &quot;02d&quot;}], &quot;base&quot;: &quot;stations&quot;, &quot;main&quot;: {&quot;temp&quot;: 22.72, &quot;feels_like&quot;: 21.83, &quot;temp_min&quot;: 22.72, &quot;temp_max&quot;: 22.94, &quot;pressure&quot;: 1020, &quot;humidity&quot;: 30, &quot;sea_level&quot;: 1020, &quot;grnd_level&quot;: 1014}, &quot;visibility&quot;: 10000, &quot;wind&quot;: {&quot;speed&quot;: 2.16, &quot;deg&quot;: 171, &quot;gust&quot;: 3.62}, &quot;clouds&quot;: {&quot;all&quot;: 16}, &quot;dt&quot;: 1728723905, &quot;sys&quot;: {&quot;type&quot;: 2, &quot;id&quot;: 2000403, &quot;country&quot;: &quot;CN&quot;, &quot;sunrise&quot;: 1728685284, &quot;sunset&quot;: 1728726003}, &quot;timezone&quot;: 28800, &quot;id&quot;: 1816670, &quot;name&quot;: &quot;Beijing&quot;, &quot;cod&quot;: 200}&#x27;},
 {&#x27;content&#x27;: &#x27;今天北京的天气总体是晴天，少量云。当前温度为22.72°C，体感温度约21.83°C，湿度为30%。风速为2.16米/秒，来自南偏东方向。在这样的天气下，户外活动会比较舒适。&#x27;,
  &#x27;refusal&#x27;: None,
  &#x27;role&#x27;: &#x27;assistant&#x27;,
  &#x27;function_call&#x27;: None,
  &#x27;tool_calls&#x27;: None,
  &#x27;sender&#x27;: &#x27;Agent&#x27;}]
```

```
display(Markdown(response.messages[-1][&#x27;content&#x27;]))
```

今天北京的天气总体是晴天，少量云。当前温度为22.72°C，体感温度约21.83°C，湿度为30%。风速为2.16米/秒，来自南偏东方向。在这样的天气下，户外活动会比较舒适。

```
def get_weather_None(loc):
    &quot;&quot;&quot;
    查询即时天气函数
    :param loc: 必要参数，字符串类型，用于表示查询天气的具体城市名称，\
    注意，中国的城市需要用对应城市的英文名称代替，例如如果需要查询北京市天气，则loc参数需要输入&#x27;Beijing&#x27;；
    :return：OpenWeather API查询即时天气的结果，具体URL请求地址为：https://api.openweathermap.org/data/2.5/weather\
    返回结果对象类型为解析之后的JSON格式对象，并用字符串形式进行表示，其中包含了全部重要的天气信息
    &quot;&quot;&quot;
    return &quot;Error: 网络问题，无法查到天气信息&quot;
```

```
get_weather_None(&quot;Beijing&quot;)
```

```
&#x27;Error: 网络问题，无法查到天气信息&#x27;
```

```
agent = Agent(
   functions=[get_weather_None]
)
```

```
response = swarm_client.run(
   agent=agent,
   messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问今天北京天气如何？&quot;}],
)
```

```
response.messages
```
