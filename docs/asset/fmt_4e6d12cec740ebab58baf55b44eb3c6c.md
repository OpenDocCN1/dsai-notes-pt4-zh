
# GPT API调用方法与提示工程入门

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/RBbydVGfwo0eIxxVcnDcwvy8nTc](https://kq4b3vgg5b.feishu.cn/docx/RBbydVGfwo0eIxxVcnDcwvy8nTc)

---

## 一、零基础零门槛调用GPT模型流程

### 1. 公开课大模型使用过程说明

为了降低大模型上手门槛，大模型体验课教学统一采用在线GPT模型，并为体验课学员免费提供体验版`API-KEY`。同时为大家提供反向代理地址，可以在国内网络环境下顺利调用GPT模型。

+ 体验版`OpenAI API-KEY`功能说明

公开课免费提供的`API-KEY`为OpenAI官方体验版KEY，总共包含5刀的使用额度，完全足够本期体验课上课使用。体验版`API-KEY`只能调用`GPT-3.5`模型，且每分钟最多请求次数为3此，每天请求次数上限为200次。

+ 完整版课程将详细介绍`OpenAI`账户注册及虚拟信用卡绑定方法

而在完整版[《大模型技术实战课》](https://appZe9inzwc2314.h5.xiaoeknow.com)中，会有完整的`OpenAI`账户注册流程教学，以及虚拟信用卡绑定方法教学。对于此类完全自己手动注册的账号，风险更低，且可以调用更多诸如`GPT-4`在内的其他模型，同时也有更多的每分钟调用次数和每天调用次数。

### 2. GPT模型调用流程测试

接下来测试能否在当前代码环境调用`GPT`模型。

+ Step 1.安装`openai`库

```
! pip install openai==0.28.1
```

注意，考虑到最新版尚有部分功能不稳定，公开课统一使用`0.28.1`版本`openai`库。

+ Step 2.尝试调用`GPT-3.5`模型

```
# 导入`openai`库
import openai
```

```
# 设置`API-Key`，这里需要换成自己的`API-KEY`
openai.api_key = "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";
```

```
# 设置国内反向代理地址
# 设置了之后无需代理即可调用`GPT`模型
openai.api_base = "https://ai.devtool.tech/proxy/v1";
```

```
# 尝试调用`GPT`模型
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo-0613",
  messages=[
    {"role": "user", "content": "你好！"}
  ]
)
response.choices[0].message['content']
```

```
'你好！有什么我可以帮助你的吗？'
```

若能顺利返回结果，则说明配置和运行成功，可以继续后面课程学习。

### 3. OpenAI `GPT`模型入门介绍

+ `GPT-3`模型与`GPT-3.5`&`GPT-4`模型

目前`OpenAI`公开的`GPT`系列模型包括`GPT-3`、`GPT-3.5`和`GPT-4`系列模型，其中`GPT-3`模型是较早发布的一类名为`Completions`（补全）模型，而`GPT-3.5`和`GPT-4`模型则是一类名为`Chat Completions`的模型。其中，根据官网给出的说明，`gpt-3.5`模型是基于`text-davinci-003`微调的模型，而后者其实是属于`gpt-3`模型类，并且由`code-davinci-002`这一基座模型经过几轮微调后训练得到，对应的模型微调关系如下所示：

![](img/f0c51a7a7a812d3a9961397774dcc43e.png)

而`GPT-4`则是全新的一类模型，并且不同于`GPT-3`模型半开源的状态，`GPT-4`模型没有任何公开的底层技术资料。

+ `0613`模型和`1106`号模型

而在一系列`GPT-3.5`和`GPT-4`模型中，又可以根据模型最大上下文限制和以日期未标记的模型小版本对其进行区分：

![](img/e27745eb7e646c351dc7bb6651e0cab1.png)

在这一系列模型中，`0613`模型和`1106`模型是最为常用的`Chat Completion`模型。其中`0613`模型开始支持`Function calling`功能，而`1106`模型则支持`Assistant API`、格式化输出内容以及更加稳定的`Function calling`。公开课将主要以`GPT-3.5-turbo-0613`模型为主，而正式课程则主要以`GPT-4-0613`和`GPT-4-1106-preview`模型为主。

## 二.`GPT`模型`API`使用方法及参数解释

### 1.`ChatCompletion.create`函数使用简例

接下来，我们尝试调用`OpenAI`的`Chat`类模型，并详细解释模型`API`参数。若要调用`Chat`类大模型，则需要使用`ChatCompletion.create`函数。这里我们直接对`Chat`模型提问“什么是机器学习？”提问和获取结果流程如下：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo-0613",
  messages=[
    {"role": "user", "content": "请问，什么是机器学习？"}
  ]
)
```

而模型返回结果如下：

```
response
```

```
<OpenAIObject chat.completion id=chatcmpl-8Zze9W35YByTtX51q0Nhn6z5qCpr4 at 0x1d86fb44590> JSON: {
  "id": "chatcmpl-8Zze9W35YByTtX51q0Nhn6z5qCpr4",
  "object": "chat.completion",
  "created": 1703589141,
  "model": "gpt-3.5-turbo-0613",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "机器学习是一..."
      },
      "logprobs": null,
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 20,
    "completion_tokens": 495,
    "total_tokens": 515
  },
  "system_fingerprint": null
}
```

能够看出，和`Completion.create`非常明显的一个区别在于，`ChatCompletion.create`函数的核心参数为`messages`，该参数则是一个基本构成元素为字典的列表，其内每个字典都代表一条独立的消息，每个字典都包含两个键值（`Key-value`）对，其中第一个`Key`都是字符串`role`（角色）表示某条消息的作者，第二个`key`为`content`（内容）表示消息具体内容。更多关于`message`的参数设置方法稍后介绍，总的来看，这里的`messages`就可以简单理解为输入给模型的信息，而模型接收到`message`之后也会输出对应的回答信息，当然也是以`message`形式呈现：

```
# 返回对象类型和`Completion.create`函数返回对象类型一致
type(response)
```

```
openai.openai_object.OpenAIObject
```

```
# 可以通过索引的方式索引出文本部分内容
response["choices"]
```
