
# 基于搜索引擎的agent大模型构建

> 来源：https://agijuejin.feishu.cn/docx/FPafdihQaoxu4Wx0DbgcTzlanGZ
>
> 作者：归来仍是少年
>
> 原文：https://zhuanlan.zhihu.com/p/706427273

## 一、前言

现在大模型训练成本很大，不可能像搜广推那些召回模型进行流式训练，所以很多需要借助外部工具api来获取对应的信息。目前大模型存在幻觉，会对他的回答盲目自信，如果需要用在垂直领域就需要非常稳定的输出。在多轮对话时期，就有两大模块意图识别和槽位填充，一步步来判断用户的真实意图，并且针对你的一些意图关键词去调对应的api来获取相应的回答或者执行操作。像很多垂直类语音助手，去识别你意图帮你执行操作，最直观地应用就是智能家居和汽车上面的语音助手。

现在大模型agent操作就是把以前的意图识别和槽位填充合并在一起做，把大模型作为一个调度中心。主要利用的OpenAI提出来的function call，通过你定义的api tools，会返回json格式，里面包含参数，你可以通过参数来判断是是用的哪个api接口，从而执行的用户意图。

```
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "返回实时天气",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "河北省承德市双桥区",
                    },
                    "format": {
                        "type": "string",
                        "enum": ["celsius", "fahrenheit"],
                        "description": "使用本地区常用的温度单位计量",
                    },
                },
                "required": ["location", "format"],
            },
        }
    },
    {
        "type": "function",
        "function": {
            "name": "get_n_day_weather_forecast",
            "description": "返回近几天的天气预报",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "河北省承德市双桥区",
                    },
                    "format": {
                        "type": "string",
                        "enum": ["celsius", "fahrenheit"],
                        "description": "使用本地区常用的温度单位计量",
                    },
                    "num_days": {
                        "type": "integer",
                        "description": "预报的天数",
                    }
                },
                "required": ["location", "format", "num_days"]
            },
        }
    },
]

messages.append({"role": "user", "content": "我在河北省承德市双桥区"})
chat_response = chat_completion_request(
    messages, tools=tools
)
assistant_message = chat_response.choices[0].message
messages.append(assistant_message)
assistant_message
```

![](img/bdd7a22360e5ec80af042d8c8efb2aa0.png)

![](img/f33aaac30a225676b7bfaba0fc1b18d3.png)

## 二、Agent构建

OpenAI的api是直接支持tools的定义和输出的，可以直接从api输出的function接口取结果，但是如果是使用国内的一些开源大模型来做的话，就需要自己写prompt定义。

OpenAI的function call底层原理也是把tools定义作为传入参数，输入模型肯定还是通过prompt形式。所以我们在用原生不支持function call的api或者开源大模型部署的话，就需要自行构建tools定义和输出格式定义。

如何利用prompt方式来构建agent，如果没有经过专门的function call训练的模型，在定义时就需要将api定义设定更为容易理解，api功能性描述。

按照搜索引擎的prompt template

```
function={"api_name":"search","description":"百度搜索联网api，可以联网搜索互联网信息","param":["query"]}
```

`api_name`：是指调用api函数的名字，这个是后续判断使用哪个api的关键参数。

`description`：描述api的功能，这个其实很重要，写的详细能让大模型判断更为精准。

`param`：主要是api所需要的参数，不同的api这个地方定义会有些差别。

**重点：**

如果大模型没有经过专门的function call形式训练，可能会存在以下几种问题：

1. 意图判断错误，调用错了api，在多个tool定义的时候容易发生这种情况。
2. api参数提取错误，导致调用api时获取到的结果是错误的，进而影响最后大模型的回答。
3. json合法性问题，有时候大模型输出可能不是一个完整的json，这时候你解析json就会有问题。

上述问题都需要优化，涉及到function call的命中率问题，会很大程度影响后续的操作。

以搜索引擎agent为例：

**上下文消歧：**

多轮对话还会涉及到当前对话指代消歧问题，跟上下文息息相关，如果只用当前对话调用搜索引擎得到结果肯定是错的，所以我们需要改写当前用户问题，这一步需要结合上下文，将这句话修改成当前完整的询问，这样才能正确搜索到答案。

![](img/31aceffa062a9c748edf1e091f76c0f4.png)

比如在获取shein的ipo信息时，之前的消息是排在前面的搜索结果是这条

![](img/b2f8ca4ceb4534e3c892bf06796c816e.png)

导致大模型其实是获取到一个错误的信息，接着我继续纠正他是在英国ipo，他这句话就会改写成如下结果

![](img/77a71daf392c885d19da1f2732d1c11c.png)

于是他就能够搜到正确的信息，有时候搜索引擎搜索过来的信息量很大，很多无关信息，这点需要做过滤。

**当前日期消歧：**

如被问到

1. 今天深圳天气如何
2. 今天是几号。
3. 今天有什么热点新闻。

这个信息如果直接去调用搜索引擎绝对是会返回不相关的东西。

所以我们需要实时告诉模型今天的日期是多少，这个作为一个传入参数给模型，让模型有这个先验信息，能够更好的改写当前问题。

比如我在端午前询问大模型，关于端午的天气等情况。

![](img/744f8043487ff20f9e90af9159578c7a.png)

前段时间不是发布最新的公积金贷款利率吗？这时候就可以直接询问大模型，他能给你正确的答案。

![](img/dee86ca514f7e94d258d3921a6388061.png)

还有我们一些行业内的梗，大模型也是能够获取外部信息知道的。

![](img/203e26350d817175875475c9120bdca6.png)

因为我只是做实验，所以对于互联网信息是有截取的，只取了前八百个字符，放开自然效果会更好。

![](img/d6c1f12de0ca6254eef591aaa92c0caf.png)

## 三、基于搜索引擎的互联网信息如何清洗

因为是做实验的话，我是通过爬虫的方式来获取百度搜索的信息（这个只是实验，大家要用的话可以直接调百度官方的搜索api），所以返回的主要是三个字段。

1. `title`
2. `abstract`
3. `url`

![](img/aa3fd634820153e27895e791fd62e819.png)

目前网页上只能获取的这些简短内容，但是需要网页详细内容的话，就需要你自己去解析网页了。

解析网页主要有如下几个重点：

1. 需要校验`url`是否合法，因为有些返回的`url`可能访问不通。
2. `URL`进行解析，提取获取纯文本信息，去掉HTML符号。
3. 判断网页质量，进行质量打分，有些很多各种符号的网页，质量分很差，加入模型影响效果。
4. 清洗网页文本内容，保证关键信息留下，不必要的内容去掉。

## 四、总结

现在agent的重点还是提取准确性需要优化，现在function call准确度还是业界难题，尤其是定义的`tools`种类繁多情况下。

![](img/d547f6ebba7a9c64cce85049d9d88f26.png)

只有一些闭源的claude3.5、gpt4的准确率遥遥领先，开源模型还是需要根据自己的业务来优化这个模型。还有需要考虑到用户询问时的交互，api没有搜到消息或者调错了api该如何兜底之类的。

后续我会在上面尝试更多的agent api来优化体验。
