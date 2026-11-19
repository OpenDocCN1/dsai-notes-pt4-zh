
# Function calling入门与实践

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/CIgZdndqao3HL3xSVnjcWRM9nig](https://kq4b3vgg5b.feishu.cn/docx/CIgZdndqao3HL3xSVnjcWRM9nig)

```python
import os
import openai
openai.api_key = os.getenv("OPENAI_API_KEY")

import numpy as np
import pandas as pd

import json
import io
import inspect
import requests
```

## Function calling功能诞生背景

尽管大语言模型的知识储量巨大，且具备非常强大的涌现能力，但很多时候我们实际使用大语言模型时仍然会明显的感受到模型能力上的局限，例如模型无法获取最新的信息、模型只能给出文字的建议但无法直接帮我们解决某些问题（如自动回复邮件、自动查询订阅机票等）。毫无疑问，这些问题的存在极大程度限制了大语言模型的实际应用价值的进一步拓展，而在2023年4月，AutoGPT项目的横空出世则为这些解决问题提出了一个极具潜力的方案——那就是进一步赋予大语言模型调用外部工具API的能力，从而大幅拓展大语言模型的能力。例如如果我们能够让GPT模型调用谷歌搜索API（`Google Custom Search JSON API`），则模型就可以实时获取和用户问题相关的一系列搜索结果，并结合这些结果和原生的知识库来回答用户的问题，从而解决模型无法获取最新信息这一问题；再比如，如果我们能够让GPT模型调用谷歌邮箱API（`Gmail API`），则可以自动让GPT模型读取邮件，并自动进行回复等等。而根据AutoGPT不大的项目规模来看，让GPT模型调用外部工具API其实并不复杂。

![](img/4913f7e8f8165da7186e5413c35a48ee.png)

![](img/6a7eab652e89c4aca033df75987555ea.png)

## Function calling功能简介

在这一基本背景下，经过数月的研发和优化，OpenAI在0613的更新中为目前最先进的Chat类模型增加了Function calling功能，该功能的本质是让大语言模型调用外部函数的能力，即Chat模型可以不再仅仅根据自身的数据库知识进行回答，而是可以额外挂载一个函数库，然后根据用户提问进行函数库检索，根据实际需求调用外部函数并获取函数运行结果，再基于函数运行结果进行回答。其基本过程如下：

![](img/bf43080bdc0382c90b2c4d35900620b4.png)

而这个外部挂载的函数库，可以是简单的自定义函数，也可以是一个封装了外部工具API的功能型函数（例如一个可以调用谷歌搜索的函数、或者一个可以获取天气信息的函数）。而在OpenAI的精妙设计下，Function calling功能的实现过程也并不复杂，在编写问答函数时，我们只需要在`ChatCompletions.create`函数中进行参数设置、并提前定义好外部函数库即可，而在Chat模型执行Function calling时，模型会根据用户提问的语义自动检索并挑选合适的函数进行使用，整个过程并不需要人工手动干预指定使用某个函数，大语言模型能够充分发挥自身的语义理解优势，在函数库中自动挑选合适函数进行运行，并给出问题的答案。

无疑，有了外部函数库的功能加持，Chat模型的处理和解决问题的能力也必将再上一个台阶。同时，相比于此前必须借助`LangChain`的`agent`模块才能实现LLM和外部工具API的协同调用，现在Chat模型内部集成的Function calling功能实现过程更简单、开发门流程更加清晰、开发槛更低，而如此种种，也必将促进新一轮的以大语言模型为核心的AI应用的爆发。

## 基于GPT模型多角色对话系统下的Function calling

![](img/9ecf7d4fc8827364f1969b64dca87718.png)

## Stage 1.获取外部工具API

### 外部工具API

首先需要简单说明的是，到底什么是外部工具API。API的全称是应用程序接口，例如目前我们在Python环境中使用的`ChatCompletion.create`函数，实际上就是OpenAI公司提供的GPT系列模型应用程序的一个接口，通过这个API（`create`函数或者`openai`库），我们能够非常顺利的调用GPT模型，并根据输入信息实时输出模型结果。而其他的很多应用程序，也都有相应的API。例如谷歌搜索，也可以通过调用API的形式来实现搜索功能，再比如接下来我们要介绍的`OpenWeather API`，就是一个可以进行实时天气查询的API。

### OpenWeather与OpenWeather API

OpenWeather是一家提供全球范围内的气象数据服务的公司，该公司的服务包括实时天气信息、天气预报、历史天气数据以及各种气象相关的报告等，并且OpenWeather开放了一定使用限度内完全免费的API，即我们可以在代码环境中通过调用`OpenWeather API`来进行实时天气查询、天气预报等功能，这意味着开发者可以将`OpenWeather`的天气预报功能加入到他们自己的应用或网站中。

> 相比OpenAI，`OpenWeather`更加`Open`。

### 极简AI应用开发示例

当然，一个开放的天气查询API，也意味着我们可以将其封装为一个本地的函数，即通过调用`OpenWeather API`来获取实时天气信息，然后再用Function calling功能调用该函数，如此一来，就相当于给Chat模型增加了实时获取天气信息的能力，而后我们即可通过Chat模型实时查询天气或根据天气情况询问穿衣建议等，从功能上来说，接入实时天气信息的Chat模型其实也就是一个非常简单的天气查询AI助手。这是一个相对简单的将其他工具API接入Chat模型的示例，接下来我们介绍具体实现方法。

#### 1.OpenWeather注册及API key获取方法

为了能够调用`OpenWeather`服务，和`OpenAI`的API使用过程类似，我们首先需要先注册`OpenWeather`账号，并获取`OpenWeather API Key`。这里需要注意的是，对于大多数在线服务的API来说，都需要通过`API key`来进行身份验证，尽管`OpenWeather`相对更加`Open`，有非常多的免费使用的次数，但身份验证仍然是必要的防止API被滥用的有效手段。`OpenWeather API key`获取流程如下：

##### Step 1.登录OpenWeather官网并点击Sign—>create account完成注册。该网站无需魔法即可直接登录，可以使用国内邮箱或者🐧邮箱均可进行注册，官网地址为：`https://openweathermap.org/`

![](img/b91a825d9e7f2c0f6d1e49c7b7024d46.png)

##### Step 2.获取API-key：注册完成后，即可在API keys页面查看当前账户的`API key`：

![](img/8677b5afa39d22989f61d7238eedf147.png)

一般来说完成注册后，就会有一个已经激活的`API-key`。和`OpenAI`一样，`OpenWeather`的`API key`也创建多个。

##### Step 3.将其设置为环境变量：和`OpenAI API key`类似，为了方便后续调用，我们也可以直接将`OpenWeather API key`设置为环境变量，变量名为`OPENWEATHER_API_KEY`。具体设置环境变量的方法参考Ch.1中`OpenAI APkey`设置环境变量流程，此处不再赘述。

![](img/0c1772e5920172eebac3cd4b8dbd31ad.png)

设置完了环境变量之后，接下来即可按照如下方式创建`OpenWeather API key`变量：

```python
open_weather_key = os.getenv("OPENWEATHER_API_KEY")
```

需要注意的是，一般来说首次注册用户，首个`API key`需要等待2-5小时才会被激活，在此期间使用该`API key`会返回401错误。

#### 2.OpenWeather API使用方法

##### 2.1 RESTful风格API使用方法

这里我们需要补充介绍关于`RESTful`风格API调用方法，包括本节课程需要使用`OpenWeather API`、以及下一小节涉及的谷歌搜索API和`Google Sheets API`都是`RESTful API`。由于我们主要是在`Python`环境中调用这些API，因此重点介绍`Python`中这类API调用的基本步骤为：

###### Step 1.构建请求：即输入某个特定的端点，也是后续请求发送的目标端点，对于`OpenWeather`实时查询天气的API来说，这个端点就是`https://api.openweathermap.org/data/2.5/weather`；

###### Step 2.设置查询参数：即创建请求相关参数对象，例如如果是调用`OpenWeather API`进行天气查询，则需要按照官网说明设置以下参数：

![](img/aa20ab7567f0f20d0ae24602d6323a49.png)

这些参数分别是城市名称或者位置坐标、`API key`、返回结果的格式、天气相关单位标准以及输出天气信息的语言，在实际调用`OpenWeather API`时，可以通过灵活调整这些参数的取值，来获得期望的结果；

###### Step 3.发送请求：接下来我们即可借助`Python`中的`requests`库来发送请求，`requests`库是一个专门为发送HTTP请求提供支持的库，借助`requests`库可以非常便捷的进行`get`或者`post`请求，并且能够非常便捷的进行返回结果的解析；

###### Step 4.解析请求：等待API服务返回结果之后，即可围绕该结果进行解析，将其转化为当前环境可读的形式。具体返回的结果的格式可以通过`mode`参数进行设置，默认情况下是`JSON`格式。

##### 2.2 利用`OpenWeather API`获取实时天气信息

接下来我们通过一个简单的示例，来介绍如何通过`OpenWeather API`获取实时天气信息：

```python
import requests

# Step 1.构建请求
url = "https://api.openweathermap.org/data/2.5/weather"

# Step 2.设置查询参数
params = {
    "q": "Beijing",               # 查询北京实时天气
    "appid": open_weather_key,    # 输入API key
    "units": "metric",            # 使用摄氏度而不是华氏度
    "lang": "zh_cn"               # 输出语言为简体中文
}

# Step 3.发送GET请求
response = requests.get(url, params=params)

# Step 4.解析响应
data = response.json()
```

这里需要注意的是，城市名必须输入英文名，否则无法正确识别。接下来查看返回结果。首先我们先查看`response`结果：

```text
response
```

```text
<Response [200]>
```

```text
type(response)
```

```text
requests.models.Response
```

在未解析之前，我们只能查看到基本请求结果状态，这里的`200`代表成功响应，即本次发送请求获得了对应的响应，且响应内容包含在`response`中。考虑到默认情况下返回结果是`JSON`格式，因此后续代码使用了`response.json()`对其进行解析。解析内容如下：

```text
data
```

```text
{'coord': {'lon': 116.3972, 'lat': 39.9075},
 'weather': [{'id': 800, 'main': 'Clear', 'description': '晴', 'icon': '01n'}],
 'base': 'stations',
 'main': {'temp': -0.06,
  'feels_like': -0.06,
  'temp_min': -0.06,
  'temp_max': -0.06,
  'pressure': 1031,
  'humidity': 94,
  'sea_level': 1031,
  'grnd_level': 1025},
 'visibility': 10000,
 'wind': {'speed': 0.32, 'deg': 339, 'gust': 0.51},
 'clouds': {'all': 8},
 'dt': 1703671663,
 'sys': {'type': 1,
  'id': 9609,
  'country': 'CN',
  'sunrise': 1703633678,
  'sunset': 1703667322},
 'timezone': 28800,
 'id': 1816670,
 'name': 'Beijing',
 'cod': 200}
```

> 这里需要注意，若`API key`未激活，则返回的`response`将显示`401`，表示本次请求`Unauthorized`（未经授权）。此外，若`response`出现`404`，则表示URL路径无效，需要根据官网提供的信息重新核对URL路径。

能够看出，返回结果非常丰富，包含了天气条件、温度、湿度、风速、天气描述等信息，这里我们可以简单查看当前天气中的即时温度和天气状况：

```python
# 即时温度最高、最低气温
data['main']['temp_min'], data['main']['temp_max']
```

```text
(-0.06, -0.06)
```

```python
# 天气状况
data['weather'][0]['description']
```

```text
'晴'
```

对比手机查看的天气信息不难发现，`OpenWeather`提供的天气信息非常准确：

![](img/ddd15d030c09e8f645fc3f023fcbad63.png)

当然，关于`data`中更多天气信息解读，可以参考官网给出的参数解读`Fields in API response`部分：`https://openweathermap.org/current#current_JSON`

#### 3.编写实时天气查询的外部函数

##### 实时天气信息获取函数编写

接下来，我们尝试编写一个通过`OpenWeather API`实时获取天气信息的API，并作为Chat可调用的外部函数之一。很明显，为了确保和大语言模型之间的顺畅通信，此时要求函数的输入和输出都是字符串格式。具体函数编写如下：

```python
def get_weather(loc):
    """
    查询即时天气函数
    :param loc: 必要参数，字符串类型，用于表示查询天气的具体城市名称，\
    注意，中国的城市需要用对应城市的英文名称代替，例如如果需要查询北京市天气，则loc参数需要输入'Beijing'；
    :return：OpenWeather API查询即时天气的结果，具体URL请求地址为：https://api.openweathermap.org/data/2.5/weather\
    返回结果对象类型为解析之后的JSON格式对象，并用字符串形式进行表示，其中包含了全部重要的天气信息
    """
    # Step 1.构建请求
    url = "https://api.openweathermap.org/data/2.5/weather"

    # Step 2.设置查询参数
    params = {
        "q": loc,               
        "appid": open_weather_key,    # 输入API key
        "units": "metric",            # 使用摄氏度而不是华氏度
        "lang": "zh_cn"                # 输出语言为简体中文
    }

    # Step 3.发送GET请求
    response = requests.get(url, params=params)
    
    # Step 4.解析响应
    data = response.json()
    return json.dumps(data)
```



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



```python
# 执行该函数所需要的参数
function_args = json.loads(response_message["function_call"]["arguments"])
function_args
```

{'loc': 'Beijing'}

需要注意的是，外部函数的计算过程仍然是在本地执行，即`Chat`模型并不会将代码读取到服务器上再进行在线计算，因此接下来我们需要根据模型返回的函数和函数参数，在本地完成函数计算，然后再将计算过程和结果保存为`message`并追加到`messages`后面，并第二次调用`Chat`模型分析函数的计算结果，并最终根据函数计算结果输出用户问题的答案。

## 2.2 Second response

这里我们只需要借助方法，直接将`function_args`对象传入`fuction_to_call`中，即可一次性传输全部参数，方法的功能可以参考如下示例：

```python
def function_to_call_test(a, b, c):
    return a + b + c

function_args_test = {'a': 1, 'b': 2, 'c': 3}

result = function_to_call_test(**function_args_test)

print(result)
```

6

方法其实是一种较为特殊、但同时也非常便捷的参数传递方法吗，该方法会将字典中的每个key对应的value传输到同名参数位中。接下来我们将`function_args`对象传入`fuction_to_call`中并完成计算：

```python
function_response = fuction_to_call(**function_args)
```

function_response

{'coord': {'lon': 116.3972, 'lat': 39.9075}, 'weather': [{'id': 800, 'main': 'Clear', 'description': '\\u6674', 'icon': '01n'}], 'base': 'stations', 'main': {'temp': -4.79, 'feels_like': -4.79, 'temp_min': -4.79, 'temp_max': -4.79, 'pressure': 1031, 'humidity': 94, 'sea_level': 1031, 'grnd_level': 1025}, 'visibility': 10000, 'wind': {'speed': 0.32, 'deg': 339, 'gust': 0.51}, 'clouds': {'all': 8}, 'dt': 1703672623, 'sys': {'country': 'CN', 'sunrise': 1703633678, 'sunset': 1703667322}, 'timezone': 28800, 'id': 1816670, 'name': 'Beijing', 'cod': 200}

能够发现，模型已经顺利完成计算。接下来我们在`messages`对象中追加两条消息，第一条消息是第一次模型返回的结果（即调用模型的`assistant` message），第二条消息则是外部函数计算结果，该条消息的`role`为`function`，且`name`为函数名称。这也是我们首次接触`function` message，和`user`、`system`、`assistant` message不同，`function` message必须要输入关键词`name`，且`function` message的内容源于外部函数执行的计算结果，并且需要手动进行输入。具体添加过程如下：

messages

[{'role': 'user', 'content': '请帮我查询下北京天气'}]

# 追加第一次模型返回结果消息
messages.append(response_message)

messages

[{'role': 'user', 'content': '请帮我查询下北京天气'},
 <OpenAIObject at 0x24c4f601630> JSON: {
   "role": "assistant",
   "content": null,
   "function_call": {
     "name": "get_weather",
     "arguments": "{\n  \"loc\": \"Beijing\"\n}"
   }
 }]

# 追加function返回消息
messages.append(
            {
                "role": "function",
                "name": function_name,
                "content": function_response,
            }
        )

messages

[{'role': 'user', 'content': '请帮我查询下北京天气'},
 <OpenAIObject at 0x24c4f601630> JSON: {
   "role": "assistant",
   "content": null,
   "function_call": {
     "name": "get_weather",
     "arguments": "{\n  \"loc\": \"Beijing\"\n}"
   }
 },
 {'role': 'function',
  'name': 'get_weather',
  'content': '{"coord": {"lon": 116.3972, "lat": 39.9075}, "weather": [{"id": 800, "main": "Clear", "description": "\\u6674", "icon": "01n"}], "base": "stations", "main": {"temp": -4.79, "feels_like": -4.79, "temp_min": -4.79, "temp_max": -4.79, "pressure": 1031, "humidity": 94, "sea_level": 1031, "grnd_level": 1025}, "visibility": 10000, "wind": {"speed": 0.32, "deg": 339, "gust": 0.51}, "clouds": {"all": 8}, "dt": 1703672623, "sys": {"country": "CN", "sunrise": 1703633678, "sunset": 1703667322}, "timezone": 28800, "id": 1816670, "name": "Beijing", "cod": 200}'}]

接下来，再次调用`Chat`模型来围绕`messages`进行回答。需要注意的是，此时我们不再需要向模型重复提问，只需要简单的将我们已经准备好的`messages`传入`Chat`模型即可：

```python
second_response = openai.ChatCompletion.create(
            model="gpt-4-0613",
            messages=messages,)
```

second_response["choices"][0]["message"]["content"]

'北京当前的天气为晴朗，气温为-4.79摄氏度，体感温度为-4.79摄氏度，湿度为94%，风速为0.32米/秒。'

能够发现，模型最终做出了准确回答。

```python
def run_conv(messages, 
             functions_list=None, 
             functions=None, 
             model="gpt-3.5-turbo-0613", 
             return_function_call=True):
    """
    能够自动执行外部函数调用的Chat对话模型
    :param messages: 必要参数，输入到Chat模型的messages参数对象
    :param functions_list: 可选参数，默认为None，可以设置为包含全部外部函数的列表对象
    :param model: Chat模型，可选参数，默认模型为gpt-3.5-turbo-0613
    :return：Chat模型输出结果
    """
    
    user_messages = messages
    
    # 如果没有外部函数库，则执行普通的对话任务
    if functions_list == None:
        response = openai.ChatCompletion.create(
                        model=model,
                        messages=user_messages,
                        )
        response_message = response["choices"][0]["message"]
        final_response = response_message["content"]
        
    # 若存在外部函数库，则需要灵活选取外部函数并进行回答
    else:
        # 创建外部函数库字典
        available_functions = {func.__name__: func for func in functions_list}

        # 创建包含用户问题的message
        messages = user_messages
        
        # first response
        response = openai.ChatCompletion.create(
                        model=model,
                        messages=messages,
                        functions=functions,
                        function_call="auto")
        response_message = response["choices"][0]["message"]

        # 获取函数名
        function_name = response_message["function_call"]["name"]
        # 获取函数对象
        fuction_to_call = available_functions[function_name]
        # 获取函数参数
        function_args = json.loads(response_message["function_call"]["arguments"])
        
        # 将函数参数输入到函数中，获取函数计算结果
        function_response = fuction_to_call(**function_args)

        # messages中拼接first response消息
        messages.append(response_message)  
        # messages中拼接函数输出结果
        messages.append(
                {
                    "role": "function",
                    "name": function_name,
                    "content": function_response,
                }
            )  
        # 第二次调用模型
        second_response = openai.ChatCompletion.create(
                model=model,
                messages=messages,
            )  
            # 获取最终结果
        final_response = second_response["choices"][0]["message"]["content"]
    
    return final_response
```

run_conv([{'role': 'user', 'content': '请问什么是机器学习？'}])

'机器学习（Machine Learning）是一种人工智能（Artificial Intelligence）的分支领域，旨在让计算机通过数据和经验学习，从而实现智能化的决策和预测能力。机器学习的目标是利用模型和算法，让计算机能够从数据中学习规律、抽取特征，并根据学习到的经验进行分类、回归、聚类、识别、推荐等任务。\n\n机器学习的核心思想是根据已有的数据样本，通过自动化的算法构建模型，使得计算机能够自主学习，并用于对新数据的预测和决策。机器学习主要有监督学习、无监督学习和强化学习三种类型。\n\n在监督学习中，机器学习算法通过已知的输入和输出样本来学习预测模型，然后用于对未知数据的预测。无监督学习则是通过对未标记的数据进行分析和模式发现，从中提取有用的信息。强化学习则是通过与环境的交互，学习如何采取行动以获得最大的奖励。\n\n机器学习在各个领域都有广泛的应用，包括自然语言处理、图像识别、音频处理、金融预测、医疗诊断等等。通过机器学习的技术，我们能够从海量的数据中进行精确的分析和预测，提供更智能化的解决方案。'



# 大模型技术与 Agent 开发

## API 调用示例

```
messages = [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问北京今天天气如何？&quot;}]
run_conv(messages=messages, functions_list=[get_weather], functions=[get_weather_function])
```

## 返回结果

```
&#x27;北京今天天气晴朗，温度为-5.33摄氏度。风速为0.43米/秒，偏北风。空气湿度为95%，能见度为10000米。&#x27;
```

![](img/ccecb291f6d0413146f6390f83c96d6d.png)

+ 借助Zapier进行API功能拓展

![](img/eeb02129bc8452e36a8336a0940dc983.png)

+ 用于构建Agent的function calling流程

![](img/b803f894ffac85fe3d8bd880fdae8ad9.png)

+ Function calling能够实现的各类AI Agent

![](img/f280054d1b140b879a98962b4b526eb7.png)

![](img/30afd747dda3d4128f3576bd3ff4343d.png)

![](img/521db15c5ca916b83c8657a479611fa6.png)

![](img/f465dbfddb7b7ce4b25c98311d1e8e0c.png)

+ 体验课内容节选自《大模型技术实战》完整版付费课程

📍更**多大模型技术内容学习**

**扫码添加助理英英**，回复“大模型”，了解更多大模型技术详情哦👇

![](img/677d16764ec555839f46b4ecaa264c52.png)

此外，**扫码回复“入群”**，即可加入**大模型技术社群**：海量硬核独家技术干货内容 + 无门槛技术交流！

[**《大模型技术实战课》**](https://appZe9inzwc2314.h5.xiaoeknow.com)**招生进行中！【100+小时】体系大课，大模型技术应用【全领域教学】，七大模块精讲精析：**

1. GPT-4模型实战模块
2. Function calling模块
3. 开源大模型实战模块
4. 词向量数据库实战
5. 大模型微调实战
6. Agent开发企业级实战
7. 本地知识库问答实战

**此外**为持续保证学员大模型技术竞争力，课程实时追更最新大模型技术进展，近期额外新增了ChatGLM3、GPT-4-turbo和多模态大模型，以及借助Assistant API开发Agent等最新前沿技术内容！
