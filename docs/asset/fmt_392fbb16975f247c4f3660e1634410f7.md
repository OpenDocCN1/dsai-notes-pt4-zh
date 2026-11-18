
# 2023-11-03 Chat Completion 模型的 Function Calling 功能实战，多函数结合调用

> 来源：[https://xuqiwei1986.feishu.cn/docx/MYfWdHAvIodf8lxz6jVcQTjKnlh](https://xuqiwei1986.feishu.cn/docx/MYfWdHAvIodf8lxz6jVcQTjKnlh)

> 🔗 原文链接： h[ttps://articles.zs](https://articles.zsxq.com/id_m10dmo6pd5e7.html)xq.com/id_m10dmo...

大家好，我是知白。一个专注于输出 AI +编程内容的大厂资深程序员，关注我一起进步。

OpenAI 的 Chat Completion 模型是一种基于人工智能的自然语言处理技术，可以根据用户的输入生成相应的回答。而 Function Calling 使得 Chat Completion 模型可以调用外部函数获取信息再针对输出进行格式化。本文将详细介绍 Function Calling 的特点和应用。

## 一、Function Calling 做什么用？

这里先简单说一下 OpenAI 的 Chat Completion，我们可以执行程序简单调用该 API 完成一轮与 ChatGPT 的对话。

```python
importopenai

importjson

openai.api_key=""

if__name__=='__main__':

response=openai.ChatCompletion.create(

model="gpt-3.5-turbo",

messages=[

{"role":"system","content":"你可以回答所有内容"},

{"role":"user","content":"你是谁?"},

]

)

print("response="+json.dumps(response))
```

在 Python 中直接通过 OpenAI 的 `ChatCompletion` API 可以快速完成对话，打印出的结果如下：

```
{
"id":"chatcmpl-7zflIIBlF7fNVYI1NHKdlLSZDBxHw",
"object":"chat.completion",
"created":1694932896,
"model":"gpt-3.5-turbo-0613",
"choices":[

{

"index":0,

"message":{

"role":"assistant",

"content":"我是一个AI助手，没有具体的身份和个人特征。我被设计成帮助回答各种问题和提供相关信息。如有需要，我会尽力帮助您。"

},

"finish_reason":"stop"

}

],

"usage":{

"prompt_tokens":23,

"completion_tokens":58,

"total_tokens":81

}
}
```

返回的信息中我们可以通过 `response['choices'][0]['message']['content']` 只提取回答的内容，其他都是辅助信息，告诉我们本次回答结束原因、花费的 token 数等。

相信大家都知道 ChatGPT 的训练数据只到2021年9月份，所以无法获取目前的最新信息。ChatGPT 其本身的能力强大，但由于信息受限导致其使用价值大打折扣。

如果我们可以将最新信息输入给 ChatGPT 让它来帮我们进行进一步的分析或处理，是不是就能满足？ 于是 OpenAI 在 6月份推出了 Function Calling 功能，让 GPT 模型拥有调用外部接口的能力。

就相当于给 AI 模型加了一双翅膀，可以真正的翱翔了。我们可以基于 Function Calling 能力自定义各种函数，并结合起来使用。

![](img/8b86ed08b8c5245d429c6ba7eba52269.png)

## 二、如何使用 Function Calling？

### 1、Function Calling 的特点

Function Calling 是一种让 Chat Completion 模型调用外部函数的能力，可以让模型不仅仅根据自身的数据库知识进行回答，而是可以额外挂载一个函数库，然后根据用户提问去函数库检索，按照实际需求调用外部函数并获取函数运行结果，再基于函数运行结果进行回答。

这个功能总结下来也很简单就几个字——可识别且格式化输出。

Function Calling 可以被应用于各种场景，例如：

自动回复邮件：获取邮箱信息函数 -> ChatGPT 生成回复 -> 发送回复邮件函数

自动点餐：输入想吃啥 -> 美团外卖上搜索对应商品函数 -> 由 ChatGPT 抉择定哪家 -> 美团外卖下单函数

只要能够定义出函数并结合 ChatGPT 的语义理解能力，通过分析用户的输入信息并自动匹配函数进行调用，最终达成用户的目的。

### 2、Function Calling 的使用

首先需要说明的是，目前 OpenAI 仅支持 gpt-3.5-turbo-0613 和 gpt-4-0613 两个语言模型使用 Function Calling 功能。这两个模型经过微调既可以检测何时应该调用函数（取决于输入），也可以使用符合函数签名的 JSON 进行响应。

使用 Function Calling 需要先设定 Chat Completion 模型的输出格式。在没有任何 Prompt 的情况下，Chat Completion 模型会返回一个 JSON 格式的数据。我们需要对这个数据进行包装，并设定具体的格式化逻辑来让我们更好使用。

官方给出的具体流程如下：

使用用户查询和函数参数中定义的一组外部函数库。

模型可以选择调用任意外部函数；如果是这样，内容将是符合自定义架构的字符串化 JSON 对象（注意：模型可能会生成无效的 JSON 或幻觉参数）。

在代码中将字符串解析为 JSON，并使用提供的参数调用函数（如果存在）。

通过将函数响应追加为新消息来再次调用模型，并让模型将结果汇总返回给用户。

#### 2.1 外部函数编写规范

在使用前需要我们先对外部函数进行定义和实现，实现就不多说就是一个具体函数。而定义需要我们包含以下几部分内容：

`name`：清晰的函数名称

`description`：函数功能的具体描述，尽量对输入参数和输出信息有明确的说明

`parameters`：对函数的每个输入参数进行类型定义及其描述

`required`：指定哪些参数必填

示例如下：

```python
defget_location_by_ip(ip:str=None):

ifipisNone:

response=requests.get('https://api.ipify.org')

ip=response.text

url='http://whois.pconline.com.cn/ipJson.jsp?ip='+ip+'&json=true'

response=requests.get(url)

result=response.json()

ifresult['err']=='noprovince':

returnjson.dumps({"当前地址":result['addr']})

else:

returnNone

get_location_by_ip_func={

"name":"get_location_by_ip_address",

"description":"获取当前的地址",

"parameters":{

"type":"object",

"properties":{

"ip":{

"type":"string",

"description":"当前网络的IP地址"

}

},

"required":[],

}

}
```

这里需要注意，根据官网的 API 描述 外部函数返回信息需要是标准的 JSON 格式 ，所以需要特殊处理。

#### 2.2 Function Calling 使用规范

在 Chat Completion 模型中新增两个参数：`function_call` 和 `functions`。其中 `function_call` 是一个开关，控制模型是否要调用函数对输出进行处理，`functions` 是一个用来对输出结果进行处理的函数描述列表 。

`function_call` 存在三种使用方式：

`none`：不调用外部函数，直接与 ChatGPT 进行沟通

`auto`：由模型根据语义分析判断是否是否调用外部函数，匹配具体函数

`{"name":"外部函数名"}`：指定模型强制调用哪个外部函数

而 `functions` 就是具体的外部函数列表，我们只需将前面定义好的函数名称通过列表的信息写入即可。

还有最关键的一步，需要我们根据 `ChatCompletion` 返回的信息判断是否调用了外部函数，从而触发二次调用 ChatGPT 附带外部函数的返回结果信息。

```python
messages=[

{"role":"system","content":"你可以回答所有内容"},

{"role":"user","content":"我当前地址是啥"},

]

#Step1:将对话和可用外部函数列表发送到GPT

response=openai.ChatCompletion.create(

model="gpt-3.5-turbo-0613",

messages=messages,

functions=[get_location_by_ip_func],

function_call="auto"

)

response_message=response["choices"][0]["message"]

#Step2:确认GPT是否调用外部函数

ifresponse_message.get("function_call"):

#Step3:调用外部函数，获取结果

available_functions={

"get_location_by_ip":get_location_by_ip,

}

function_name=response_message["function_call"]["name"]

#获取匹配到的外部函数

function_to_call=available_functions[function_name]

#获取外部函数的入参信息

function_args=json.loads(response_message["function_call"]["arguments"])

#调用外部函数

function_response=function_to_call(

ip=function_args.get("ip")

)

#Step4:将原始请求和外部函数响应的结果信息发送到GPT

messages.append(response_message)

messages.append(

{

"role":"function",

"name":function_name,

"content":function_response,

}

)

#进行二次请求GPT

second_response=openai.ChatCompletion.create(

model="gpt-3.5-turbo-0613",

messages=messages,

)

print("second_response="+json.dumps(second_response))

else:

print("response="+json.dumps(response))
```

看看第二次调用请求时的 `messages` ,很明显已经执行了外部函数获取到当前地址信息并组装到 `messages` 中请求给 ChatGPT。

![](img/1ce09dc82281d935e42c107504ac3883.png)

#### 2.3 Function Calling 组合使用

在上面的示例中，我们只保持了一个外部函数的调用。在实际使用中，一定是存在多个函数组合使用的情况。

在上面的基础我们增加一个获取新闻的外部函数，整体流程：

获取当前地址信息

根据城市获取新闻

由 ChatGPT 对结果进行处理并返回

根据上面调用规范将整体封装到 `complete` 函数中，并根据每次返回的结果角色判断上次调用是否触发外部函数调用，若触发外部函数调用需要再次请求 ChatGPT 以获取最终结果。代码如下：

```python
defcomplete(messages,function_call:str="auto"):

messages.append({"role":"system","content":llm_system_prompt})

#Step1:将对话和可用外部函数列表发送到GPT

res=openai.ChatCompletion.create(

model=llm_model,

messages=messages,

functions=[signature_get_top_headlines,get_location_by_ip_func],

function_call=function_call

)

response_message=res["choices"][0]["message"]

#Step2:确认GPT是否调用外部函数

ifresponse_message.get("function_call"):

#Step3:调用外部函数，获取结果

available_functions={

"get_location_by_ip":get_location_by_ip,

"get_top_headlines":get_top_headlines

}

function_name=response_message["function_call"]["name"]

#获取匹配到的外部函数

function_to_call=available_functions[function_name]

#获取外部函数的入参信息

function_args=json.loads(response_message["function_call"]["arguments"])

#根据指定外部函数进行拼装入参信息并调用

if(function_name=="get_top_headlines"):

function_response=function_to_call(

query=function_args.get("query"),

country=function_args.get("country"),

category=function_args.get("category")

)

elif(function_name=="get_location_by_ip"):

function_response=function_to_call(

ip=function_args.get("ip")

)

#Step4:将原始请求和外部函数响应的结果组装起来发送到GPT

messages.append(response_message)

messages.append(

{

"role":"function",

"name":function_name,

"content":function_response,

}

)

else:

print("返回结果:\n"+response_message['content'])

if__name__=='__main__':

messages=[

{"role":"system","content":"根据用户当前地址信息，回答用户的问题，在返回信息中附带上所在城市，并使用中文"},

{"role":"user","content":"获取我当前地址所处城市的娱乐新闻"},

]

complete(messages)

#当前为function调用时继续触发OpenAi请求

whilemessages[-1]['role']=="function":

complete(messages)
```

查看最后一次发送的 `messages`，也能看出两次外部函数调用结果都包含进行来了。

![](img/4aff707e9616817125ef965ad2b38c40.png)

最终返回的结果满足预期：

返回结果:

根据您当前所处的地址信息，位于新加坡Linode数据中心。以下是娱乐新闻的几条头条新闻：

1.标题：《“没有和解的希望”：说唱歌手YoungJeezy与电视主持人在仅两年后提出离婚申请》

描述：文件称婚姻“彻底破裂”，并且“没有和解的希望”。

[阅读更多](https://www.wsbtv.com/news/local/atlanta/no-hope-reconciliation-rapper-young-jeezy-files-divorce-tv-host-jeannie-mai/KPZHFGHXXJA4ZHFDN6BVTC6KEM/)

2.标题：《BonAppetit杂志的2023年24家最佳新美食餐厅》

没有描述信息。

[阅读更多](https://www.usatoday.com/story/money/food/2023/09/14/best-new-restaurants-2023-bon-appetit-magazine/70850292007/)

3.标题：《预计会有超过7万名粉丝参加EdSheeran的Levi's体育场演出》

描述：Levi's体育场曾经以泰勒·斯威夫特和碧昂斯举办的巡回演唱会成为历史上最高票房的演唱会之一。但是这个周末的EdSheeran的音乐会可能会...。

[观看更多](https://www.youtube.com/watch?v=dE954TQyp6c)

请注意，以上新闻是来自不同的娱乐新闻来源，您可以点击相应的链接获取更多详细信息。

## 结尾

实战代码通过阿里网盘获取： [实战代码](https://www.aliyundrive.com/s/WnJ5wuHMPX6)

Function Calling是一种非常有用的功能，可以让Chat Completion模型更加灵活地应对各种场景。通过调用外部函数进行输出格式化，可以让Chat Completion模型更加贴合实际需求，并提供更加优质、高效、个性化的服务。
