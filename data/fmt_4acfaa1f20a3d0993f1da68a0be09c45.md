
> 再次提示，我们可以通过`json.dumps()`函数将JSON转化为字符串，反过来我们可以通过`json.loads()`函数将字符串转化为JSON对象。

简单测试函数能否正常运行：

```
get_weather('Hangzhou')
```

```
'{"coord": {"lon": 120.1614, "lat": 30.2937}, "weather": [{"id": 800, "main": "Clear", "description": "\\u6674", "icon": "01n"}], "base": "stations", "main": {"temp": 12.16, "feels_like": 10.84, "temp_min": 10.95, "temp_max": 12.16, "pressure": 1028, "humidity": 54, "sea_level": 1028, "grnd_level": 1026}, "visibility": 10000, "wind": {"speed": 1.4, "deg": 65, "gust": 1.63}, "clouds": {"all": 6}, "dt": 1703671909, "sys": {"type": 2, "id": 145356, "country": "CN", "sunrise": 1703631244, "sunset": 1703667949}, "timezone": 28800, "id": 1808926, "name": "Hangzhou", "cod": 200}'
```

能够发现函数能够正常运行，这里我们也可以简单验证GPT模型能否正常识别函数输出信息：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo-0613",
  messages=[
    {"role": "system", "content": "天气信息来源于OpenWeather API：https://api.openweathermap.org/data/2.5/weather"},
    {"role": "system", "content": "这是今日杭州市的天气：%s" % data},
    {"role": "user", "content": "请问今日杭州天气如何？请用简体中文回答"}
  ]
)
response.choices[0].message['content']
```

```
'今日杭州天气晴朗，气温约为-0.06摄氏度，湿度为94%。'
```

## Stage 2. Function calling流程实现

### 1. functions参数解释与定义过程

在准备好外部函数及函数库之后，接下来非常重要的一步就是需要将外部函数的信息以某种形式传输给Chat模型。此时就需要使用到`ChatCompletion.create`函数的`functions`参数，类似于`messages`参数是用于向模型传输消息，`functions`参数专门用于向模型传递当前可以调用的外部函数信息。并且，从参数的具体形式来看，`functions`参数和`messages`参数也是非常类似的——都是包含多个字典的`list`。对于`messages`来说，每个字典都是一条信息，而对于`functions`参数来说，每个字典都是一个函数。在大语言模型实际进行问答时，会根据`functions`参数提供的信息对各函数进行检索。

很明显，`functions`参数对于Chat模型的Function calling功能的实现至关重要。接下来我们详细解释`functions`中每个用于描述函数的字典编写方法。总的来说，每个字典都有三个参数（三组键值对），各参数（Key）名称及解释如下：

+   `name`：代表函数名称的字符串，必选参数，按照要求函数名称必须是 a-z、A-Z、0-9，或包含下划线和破折号，最大长度为 64。需要注意的是，`name`必须输入函数名称，而后续模型将根据函数名称在外部函数库中进行函数筛选；

+   `description`：用于描述函数功能的字符串，虽然是可选参数，但该参数传递的信息实际上是Chat模型对函数功能识别的核心依据。即Chat函数实际上是通过每个函数的`description`来判断当前函数的实际功能的，若要实现多个备选函数的智能挑选，则需要严谨详细的描述函数功能；（需要注意的是，在某些情况下，我们会通过其他函数标注本次对话特指的函数，此时模型就不会执行这个根据描述信息进行函数挑选的过程，此时是可以不设置`description`的。）

+   `parameters`：函数参数，必选参数，要求遵照JSON Schema格式进行输入，JSON Schema是一种特殊的JSON对象，专门用于验证JSON数据格式是否满足要求。

例如，对于`get_weather`函数，我们需要创建如下字典来对其进行完整描述：

```
get_weather_function = {
    'name': 'get_weather',
    'description': '查询即时天气函数，根据输入的城市名称，查询对应城市的实时天气',
    'parameters': {
        'type': 'object',
        'properties': {
            'loc': {
                'description': "城市名称，注意，中国的城市需要用对应城市的英文名称代替，例如如果需要查询北京市天气，则loc参数需要输入'Beijing'",
                'type': 'string'
            }
        },
        'required': ['loc']
    }
}
```

同时还需要封装外部函数库，用于关联外部函数名称和外部函数对象

```
available_functions = {
            "get_weather": get_weather,
        }
```

### 2. Function calling功能实现

#### 2.1 First response

在进行了一系列基础准备工作之后，接下来我们尝试在Chat模型对话执行Function calling功能。这里我们创建如下`messages`：

```
messages=[
    {"role": "user", "content": "请帮我查询下北京天气"}
]
```

首先我们测试如果只输入这个信息而不输入外部函数库的时候，模型能否知道如何查询天气：

```
response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo-0613",
        messages=messages 
    )
response_message = response["choices"][0]["message"]
```

```
response
```

```
<OpenAIObject chat.completion id=chatcmpl-8aLLVjUEUAn0JaCOVczxba3Zh9wsJ at 0x24c4f2d8720> JSON: {
  "id": "chatcmpl-8aLLVjUEUAn0JaCOVczxba3Zh9wsJ",
  "object": "chat.completion",
  "created": 1703672553,
  "model": "gpt-4-0613",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "\\u5bf9\\u4e0d\\u8d77\\uff0c\\u4f5c\\u4e3a\\u4e00\\u4e2aAI\\uff0c\\u6211\\u65e0\\u6cd5\\u5b9e\\u65f6\\u67e5\\u8be2\\u5b9e\\u9645\\u7684\\u4fe1\\u606f\\uff0c\\u5305\\u62ec\\u5929\\u6c14\\u3002\\u60a8\\u53ef\\u4ee5\\u4f7f\\u7528\\u624b\\u673a\\u6216\\u7535\\u8111\\u4e0a\\u7684\\u5929\\u6c14\\u5e94\\u7528\\u7a0b\\u5e8f\\u6216\\u8bbf\\u95ee\\u5929\\u6c14\\u7f51\\u7ad9\\u6765\\u83b7\\u53d6\\u6700\\u65b0\\u7684\\u5317\\u4eac\\u5929\\u6c14\\u3002"
      },
      "logprobs": null,
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 17,
    "completion_tokens": 64,
    "total_tokens": 81
  },
  "system_fingerprint": null
}
```

```
response_message["content"]
```

```
'对不起，作为一个AI，我无法实时查询实际的信息，包括天气。您可以使用手机或电脑上的天气应用程序或访问天气网站来获取最新的北京天气。'
```

很明显，模型无法进行回答。

接下来我们尝试将函数库相关信息输入给Chat模型，这里需要额外设置两个参数，其一是`functions`参数，用于申明外部函数库当前情况，其二则是需要设置`function_call`参数，该参数用于控制是否执行Function calling功能，该参数有三种不同的取值，默认取值为none，表示不需要调用外部函数，不执行Function calling功能，此时`functions`参数不需要进行额外设置。而如果设置了`functions`参数，则`function_call`参数会默认修改为`auto`（当然也可以手动填写该参数），表示模型将根据用户实际对话情况，有选择性的自动挑选合适函数进行执行，而若想让模型在本次对话中特定执行`functions`中的某个函数，则可以通过输入如下形式的字典：`{"name": "my_function"}`进行申明，此时模型不再会自动挑选模型，而是会在`functions`中挑选"my_function"进行执行。

这里我们尝试让模型自动挑选函数来进行执行，参数设置方法与执行过程如下：

```
response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo-0613",
        messages=messages,
        functions=[get_weather_function],
        function_call="auto",  
    )
```

观察此时`response`结果：

```
response
```

```
<OpenAIObject chat.completion id=chatcmpl-8aLQdAVBMF8DW1h4MgnrhJ8iCOIrE at 0x24c4f553d10> JSON: {
  "id": "chatcmpl-8aLQdAVBMF8DW1h4MgnrhJ8iCOIrE",
  "object": "chat.completion",
  "created": 1703672871,
  "model": "gpt-4-0613",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": null,
        "function_call": {
          "name": "get_weather",
          "arguments": "{\n  \"loc\": \"Beijing\"\n}"
        }
      },
      "logprobs": null,
      "finish_reason": "function_call"
    }
  ],
  "usage": {
    "prompt_tokens": 123,
    "completion_tokens": 16,
    "total_tokens": 139
  },
  "system_fingerprint": null
}
```

```
response_message = response["choices"][0]["message"]
response_message
```

```
<OpenAIObject at 0x24c4f601630> JSON: {
  "role": "assistant",
  "content": null,
  "function_call": {
    "name": "get_weather",
    "arguments": "{\n  \"loc\": \"Beijing\"\n}"
  }
}
```

能够发现，此时返回的`message`中`content`为空，而增了一个`"function_call"`字典，该字典包含两个键值对，观察Key名称不难发现，Key为`"name"`的键值对表示完成该对话需要调用的函数名称，也就是`get_weather`，而Key为`"arguments"`的键值对则表示需要传入该函数的参数。注意，这条消息仍然是`assistant`传来的消息，只不过这条消息不包含`content`，而包含`function_call`关键词，表示这条`assistant`消息需要调用外部函数。而`function_call`关键词也是`messages`参数可以包含的备选关键词之一。

接下来我们将这两个核心信息分别保存为`function_name`和`function_args`对象：

```
# 完成对话需要调用的函数名称
function_name = response_message["function_call"]["name"]
function_name
```

```
'get_weather'
```

```
# 具体的函数对象
fuction_to_call = available_functions[function_name]
fuction_to_call
```

```
<function __main__.get_weather(loc)>
```
