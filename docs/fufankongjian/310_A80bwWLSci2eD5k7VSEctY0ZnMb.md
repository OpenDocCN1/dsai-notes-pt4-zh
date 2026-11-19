
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



```
[&lt;OpenAIObject at 0x1d86fb58720&gt; JSON: {
   &quot;index&quot;: 0,
   &quot;message&quot;: {
     &quot;role&quot;: &quot;assistant&quot;,
     &quot;content&quot;: &quot;\u673a\u5668\u5b66\u4e60\u662f\u4e00\u79cd\u4eba\u5de5\u667a\u80fd\uff08AI\uff09\u7684\u5206\u652f\u9886\u57df\uff0c\u8be5\u9886\u57df\u7814\u7a76\u5982\u4f55\u4f7f\u8ba1\u7b97\u673a\u7cfb\u7edf\u901a\u8fc7\u6570\u636e\u548c\u7ecf\u9a8c\u6765\u81ea\u52a8\u5b66\u4e60\u548c\u6539\u8fdb\uff0c\u800c\u4e0d\u9700\u8981\u660e\u786e\u5730\u7f16\u7a0b\u3002\u4e5f\u5c31\u662f\u8bf4\uff0c\u673a\u5668\u5b66\u4e60\u7684\u76ee\u6807\u662f\u5f00\u53d1\u7b97\u6cd5\u548c\u6a21\u578b\uff0c\u4f7f\u8ba1\u7b97\u673a\u7cfb\u7edf\u80fd\u591f\u4ece\u6570\u636e\u4e2d\u63d0\u53d6\u89c4\u5f8b\u3001\u6a21\u5f0f\u548c\u77e5\u8bc6\uff0c\u5e76\u5229\u7528\u8fd9\u4e9b\u77e5\u8bc6\u6765\u8fdb\u884c\u9884\u6d4b\u3001\u5206\u7c7b\u3001\u51b3\u7b56\u7b49\u4efb\u52a1\u3002\n\n\u673a\u5668\u5b66\u4e60\u7684\u57fa\u672c\u539f\u7406\u662f\u4f7f\u7528\u7edf\u8ba1\u5b66\u548c\u6a21\u5f0f\u8bc6\u522b\u7b49\u65b9\u6cd5\uff0c\u901a\u8fc7\u8bad\u7ec3\u7b97\u6cd5\u6765\u5efa\u7acb\u4e00\u4e2a\u6570\u5b66\u6a21\u578b\u3002\u8fd9\u4e2a\u6a21\u578b\u53ef\u4ee5\u6839\u636e\u8f93\u5165\u6570\u636e\u8fdb\u884c\u9884\u6d4b\u548c\u51b3\u7b56\uff0c\u5e76\u5728\u7ecf\u9a8c\u548c\u5b9e\u8df5\u4e2d\u4e0d\u65ad\u6539\u8fdb\u548c\u4f18\u5316\u3002\u673a\u5668\u5b66\u4e60\u7b97\u6cd5\u53ef\u4ee5\u5e94\u7528\u4e8e\u5404\u79cd\u9886\u57df\uff0c\u5982\u56fe\u50cf\u8bc6\u522b\u3001\u8bed\u97f3\u8bc6\u522b\u3001\u81ea\u7136\u8bed\u8a00\u5904\u7406\u3001\u63a8\u8350\u7cfb\u7edf\u3001\u6570\u636e\u6316\u6398\u7b49\u3002\n\n\u673a\u5668\u5b66\u4e60\u7684\u5173\u952e\u57fa\u7840\u662f\u6570\u636e\uff0c\u56e0\u6b64\u6570\u636e\u7684\u8d28\u91cf\u548c\u6570\u91cf\u5bf9\u7b97\u6cd5\u6027\u80fd\u6709\u7740\u91cd\u8981\u5f71\u54cd\u3002\u5e38\u89c1\u7684\u673a\u5668\u5b66\u4e60\u7b97\u6cd5\u5305\u62ec\u76d1\u7763\u5b66\u4e60\u3001\u65e0\u76d1\u7763\u5b66\u4e60\u548c\u5f3a\u5316\u5b66\u4e60\u3002\u76d1\u7763\u5b66\u4e60\u662f\u901a\u8fc7\u6807\u6ce8\u597d\u7684\u6570\u636e\u8fdb\u884c\u8bad\u7ec3\u548c\u5b66\u4e60\uff0c\u7528\u4e8e\u9884\u6d4b\u548c\u5206\u7c7b\u3002\u65e0\u76d1\u7763\u5b66\u4e60\u662f\u5229\u7528\u672a\u6807\u6ce8\u7684\u6570\u636e\u8fdb\u884c\u6a21\u5f0f\u53d1\u73b0\u548c\u805a\u7c7b\u5206\u6790\u3002\u5f3a\u5316\u5b66\u4e60\u5219\u662f\u901a\u8fc7\u8bd5\u9519\u548c\u5956\u52b1\u673a\u5236\u6765\u8bad\u7ec3\u7cfb\u7edf\uff0c\u4f7f\u5176\u5728\u7279\u5b9a\u73af\u5883\u4e2d\u505a\u51fa\u6700\u4f18\u51b3\u7b56\u3002\n\n\u603b\u800c\u8a00\u4e4b\uff0c\u673a\u5668\u5b66\u4e60\u662f\u4e00\u79cd\u5229\u7528\u6570\u636e\u548c\u7ecf\u9a8c\u6765\u8ba9\u8ba1\u7b97\u673a\u81ea\u52a8\u5b66\u4e60\u548c\u6539\u8fdb\u7684\u6280\u672f\uff0c\u5b83\u5728\u4eba\u5de5\u667a\u80fd\u9886\u57df\u6709\u7740\u5e7f\u6cdb\u7684\u5e94\u7528\u524d\u666f\u3002&quot;
   },
   &quot;logprobs&quot;: null,
   &quot;finish_reason&quot;: &quot;stop&quot;
 }]
```

## 也可以通过属性调用的方式查看choices

`response.choices`

```
[&lt;OpenAIObject at 0x1d86fb58720&gt; JSON: {
   &quot;index&quot;: 0,
   &quot;message&quot;: {
     &quot;role&quot;: &quot;assistant&quot;,
     &quot;content&quot;: &quot;\u673a\u5668\u5b66\u4e60\u662f\u4e00\u79cd\u4eba\u5de5\u667a\u80fd\uff08AI\uff09\u7684\u5206\u652f\u9886\u57df\uff0c\u8be5\u9886\u57df\u7814\u7a76\u5982\u4f55\u4f7f\u8ba1\u7b97\u673a\u7cfb\u7edf\u901a\u8fc7\u6570\u636e\u548c\u7ecf\u9a8c\u6765\u81ea\u52a8\u5b66\u4e60\u548c\u6539\u8fdb\uff0c\u800c\u4e0d\u9700\u8981\u660e\u786e\u5730\u7f16\u7a0b\u3002\u4e5f\u5c31\u662f\u8bf4\uff0c\u673a\u5668\u5b66\u4e60\u7684\u76ee\u6807\u662f\u5f00\u53d1\u7b97\u6cd5\u548c\u6a21\u578b\uff0c\u4f7f\u8ba1\u7b97\u673a\u7cfb\u7edf\u80fd\u591f\u4ece\u6570\u636e\u4e2d\u63d0\u53d6\u89c4\u5f8b\u3001\u6a21\u5f0f\u548c\u77e5\u8bc6\uff0c\u5e76\u5229\u7528\u8fd9\u4e9b\u77e5\u8bc6\u6765\u8fdb\u884c\u9884\u6d4b\u3001\u5206\u7c7b\u3001\u51b3\u7b56\u7b49\u4efb\u52a1\u3002\n\n\u673a\u5668\u5b66\u4e60\u7684\u57fa\u672c\u539f\u7406\u662f\u4f7f\u7528\u7edf\u8ba1\u5b66\u548c\u6a21\u5f0f\u8bc6\u522b\u7b49\u65b9\u6cd5\uff0c\u901a\u8fc7\u8bad\u7ec3\u7b97\u6cd5\u6765\u5efa\u7acb\u4e00\u4e2a\u6570\u5b66\u6a21\u578b\u3002\u8fd9\u4e2a\u6a21\u578b\u53ef\u4ee5\u6839\u636e\u8f93\u5165\u6570\u636e\u8fdb\u884c\u9884\u6d4b\u548c\u51b3\u7b56\uff0c\u5e76\u5728\u7ecf\u9a8c\u548c\u5b9e\u8df5\u4e2d\u4e0d\u65ad\u6539\u8fdb\u548c\u4f18\u5316\u3002\u673a\u5668\u5b66\u4e60\u7b97\u6cd5\u53ef\u4ee5\u5e94\u7528\u4e8e\u5404\u79cd\u9886\u57df\uff0c\u5982\u56fe\u50cf\u8bc6\u522b\u3001\u8bed\u97f3\u8bc6\u522b\u3001\u81ea\u7136\u8bed\u8a00\u5904\u7406\u3001\u63a8\u8350\u7cfb\u7edf\u3001\u6570\u636e\u6316\u6398\u7b49\u3002\n\n\u673a\u5668\u5b66\u4e60\u7684\u5173\u952e\u57fa\u7840\u662f\u6570\u636e\uff0c\u56e0\u6b64\u6570\u636e\u7684\u8d28\u91cf\u548c\u6570\u91cf\u5bf9\u7b97\u6cd5\u6027\u80fd\u6709\u7740\u91cd\u8981\u5f71\u54cd\u3002\u5e38\u89c1\u7684\u673a\u5668\u5b66\u4e60\u7b97\u6cd5\u5305\u62ec\u76d1\u7763\u5b66\u4e60\u3001\u65e0\u76d1\u7763\u5b66\u4e60\u548c\u5f3a\u5316\u5b66\u4e60\u3002\u76d1\u7763\u5b66\u4e60\u662f\u901a\u8fc7\u6807\u6ce8\u597d\u7684\u6570\u636e\u8fdb\u884c\u8bad\u7ec3\u548c\u5b66\u4e60\uff0c\u7528\u4e8e\u9884\u6d4b\u548c\u5206\u7c7b\u3002\u65e0\u76d1\u7763\u5b66\u4e60\u662f\u5229\u7528\u672a\u6807\u6ce8\u7684\u6570\u636e\u8fdb\u884c\u6a21\u5f0f\u53d1\u73b0\u548c\u805a\u7c7b\u5206\u6790\u3002\u5f3a\u5316\u5b66\u4e60\u5219\u662f\u901a\u8fc7\u8bd5\u9519\u548c\u5956\u52b1\u673a\u5236\u6765\u8bad\u7ec3\u7cfb\u7edf\uff0c\u4f7f\u5176\u5728\u7279\u5b9a\u73af\u5883\u4e2d\u505a\u51fa\u6700\u4f18\u51b3\u7b56\u3002\n\n\u603b\u800c\u8a00\u4e4b\uff0c\u673a\u5668\u5b66\u4e60\u662f\u4e00\u79cd\u5229\u7528\u6570\u636e\u548c\u7ecf\u9a8c\u6765\u8ba9\u8ba1\u7b97\u673a\u81ea\u52a8\u5b66\u4e60\u548c\u6539\u8fdb\u7684\u6280\u672f\uff0c\u5b83\u5728\u4eba\u5de5\u667a\u80fd\u9886\u57df\u6709\u7740\u5e7f\u6cdb\u7684\u5e94\u7528\u524d\u666f\u3002&quot;
   },
   &quot;logprobs&quot;: null,
   &quot;finish_reason&quot;: &quot;stop&quot;
 }]
```

## 查看第一个返回结果

`response.choices[0]`



## 1. 查看第一个返回结果的message

```
<OpenAIObject at 0x1d86fb58720> JSON: {
  "index": 0,
  "message": {
    "role": "assistant",
    "content": "机器学习是一种人工智能（AI）的分支领域，该领域研究如何使计算机系统通过数据和经验来自动学习和改进，而不需要明确地编程。也就是说，机器学习的目标是开发算法和模型，使计算机系统能够从数据中提取规律、模式和知识，并利用这些知识来进行预测、分类、决策等任务。\n\n机器学习的基本原理是使用统计学和模式识别等方法，通过训练算法来建立一个数学模型。这个模型可以根据输入数据进行预测和决策，并在经验和实践中不断改进和优化。机器学习算法可以应用于各种领域，如图像识别、语音识别、自然语言处理、推荐系统、数据挖掘等。\n\n机器学习的关键基础是数据，因此数据的质量和数量对算法性能有着重要影响。常见的机器学习算法包括监督学习、无监督学习和强化学习。监督学习是通过标注好的数据进行训练和学习，用于预测和分类。无监督学习是利用未标注的数据进行模式发现和聚类分析。强化学习则是通过试错和奖励机制来训练系统，使其在特定环境中做出最优决策。\n\n总而言之，机器学习是一种利用数据和经验来让计算机自动学习和改进的技术，它在人工智能领域有着广泛的应用前景。"
  },
  "logprobs": null,
  "finish_reason": "stop"
}
```

```
response.choices[0].message['content']
```

```
'机器学习是一种人工智能（AI）的分支领域，该领域研究如何使计算机系统通过数据和经验来自动学习和改进，而不需要明确地编程。也就是说，机器学习的目标是开发算法和模型，使计算机系统能够从数据中提取规律、模式和知识，并利用这些知识来进行预测、分类、决策等任务。\n\n机器学习的基本原理是使用统计学和模式识别等方法，通过训练算法来建立一个数学模型。这个模型可以根据输入数据进行预测和决策，并在经验和实践中不断改进和优化。机器学习算法可以应用于各种领域，如图像识别、语音识别、自然语言处理、推荐系统、数据挖掘等。\n\n机器学习的关键基础是数据，因此数据的质量和数量对算法性能有着重要影响。常见的机器学习算法包括监督学习、无监督学习和强化学习。监督学习是通过标注好的数据进行训练和学习，用于预测和分类。无监督学习是利用未标注的数据进行模式发现和聚类分析。强化学习则是通过试错和奖励机制来训练系统，使其在特定环境中做出最优决策。\n\n总而言之，机器学习是一种利用数据和经验来让计算机自动学习和改进的技术，它在人工智能领域有着广泛的应用前景。'
```

可以说，在获取对话结果的过程中，除了`ChatCompletion.create`返回结果最终是保存在`message`属性中，整个Chat Completion模型对话模型需要输入`message`，返回也是`message`，可以说这种文本交互形式确实非常符合人类在进行聊天时的问答习惯。

最后需要注意的是我们可以在返回结果的`usage`中查看本次对话所占用的token数量，其中`prompt_tokens`表示提示词占用token数量，`completion_tokens`则表示返回结果所占用token数量，而`total_tokens`则是二者相加，代表本次对话总共占用token数量。

```
response.usage
```

```
<OpenAIObject at 0x1d86fb58680> JSON: {
  "prompt_tokens": 20,
  "completion_tokens": 495,
  "total_tokens": 515
}
```

```
response.usage["total_tokens"]
```

```
515
```

## 2. GPT模型API参数解释与非核心参数设置方法

而关于`ChatCompletion.create`函数的详细参数解释，可以在官网相关页面查阅：`https://platform.openai.com/docs/api-reference/chat/create` 。

![](img/2a3f6e96177b82118155721ba2ab0d14.png)

相关参数解释如下：

+ `model`：必选参数，具体调用的模型名称；
+ `messages`：必选参数，传入模型的消息队列；
+ `max_tokens`：可选参数，代表返回结果的token数量；
+ `temperature`：可选参数，取值范围为0-2，默认值为1。参数代表采样温度，数值越小，则模型会倾向于选择概率较高的词汇，生成的文本会更加保守；而当`temperature`值较高时，模型会更多地选择概率较低的词汇，生成的文本会更加多样；
+ `top_p`：可选参数，取值范围为0-1，默认值为1，和`temperature`作用类似，用于控制输出文本的随机性，数值越趋近与1，输出文本随机性越强，越趋近于0文本随机性越弱；通常来说若要调节文本随机性，`top_p`和`temperature`两个参数选择一个进行调整即可；这里更推荐使用`temperature`参数进行文本随机性调整；
+ `n`：可选参数，默认值为1，表示一个提示返回几个回答结果；
+ `stream`：可选参数，默认值为`False`，表示回复响应的方式，当为`False`时，模型会等待返回结果全部生成后一次性返回全部结果，而为`True`时，则会逐个字进行返回；
+ `logprobs`：可选参数，默认为`null`，该参数用于指定模型返回前N个概率最高的token及其对数概率。例如，如果`logprobs`设为10，那么对于生成的每个token，API会返回模型预测的前10个token及其对数概率；
+ `echo`：可选参数，默认为`False`，该参数用于控制模型是否应该简单地复述用户的输入。如果设为`True`，模型的响应会尽可能地复述用户的输入；
+ `stop`：可选参数，默认为`null`，该参数接受一个或多个字符串，用于指定生成文本的停止信号。当模型生成的文本遇到这些字符串中的任何一个时，会立即停止生成。这可以用来控制模型的输出长度或格式；
+ `presence_penalty`：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型生成新内容（例如新的概念或主题）的倾向性。较高的值会使模型更倾向于生成新内容，而较低的值则会使模型更倾向于坚持已有的内容，当返回结果篇幅较大并且存在前后主题重复时，可以提高该参数的取值；
+ `frequency_penalty`：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型重复自身的倾向性。较高的值会使模型更倾向于避免重复，而较低的值则会使模型更可能重复自身；当返回结果篇幅较大并且存在前后语言重复时，可以提高该参数的取值；
+ `best_of`：该参数用于控制模型的生成过程。它会让模型进行多次尝试（例如，生成5个不同的响应），然后选择这些响应中得分最高的一个；
+ `logit_bias`：该参数接受一个字典，用于调整特定token的概率。字典的键是token的ID，值是应用于该token的对数概率的偏置；在GPT中我们可以使用`tokenizer tool`查看文本Token的标记。一般不建议修改；
+ `function_call`：在进行Function calling时调用外部函数模式；
+ `functions`：外部函数的函数说明，以Json Schema形式呈现；

### 关于top_p参数的深度解释

`top_p`是一种在大型语言模型生成文本时常用的采样策略参数，特别是在使用GPT模型（例如GPT-2或GPT-3）时。这个参数也被称为nucleus采样或者p采样。生成文本的过程中，模型会为下一个词预测一个概率分布。然后，我们可以从这个分布中采样下一个词。`top_p`策略在采样时并不只是考虑最高概率的词，而是从最有可能的一组词（"nucleus"）中采样。具体来说，`top_p`参数定义了一个概率阈值，该阈值是用来确定哪些词会被包括在nucleus中。例如，如果`top_p`设置为0.9，那么就会选择概率最高的词，直到这些词的累计概率达到或超过0.9。然后，从这些词中随机选择下一个词。与`top_k`采样相比，`top_p`采样可以动态地调整考虑的词的数量，因为它不仅仅是取概率最高的前k个词。这使得`top_p`采样在处理具有尾部分布的语言模型时更具优势，因为在这种情况下，可能只有很少的词占据了大部分的概率。使用`top_p`采样可以帮助增加生成文本的多样性，同时也可以保持一定的连贯性和质量。

当`top_p`值越大，例如接近1，这意味着更多的词被包括在采样的"nucleus"中，因此模型生成的文本会有更高的随机性。这是因为，模型在选择下一个词时，可以从更大的词汇集合中选择，而这个集合包括了累积概率超过`top_p`阈值的所有词。换句话说，如果`top_p`为1，那么所有的词都有可能被选择，这样就最大化了随机性。相反，如果`top_p`值较小，例如接近0，这意味着较少的词被包括在采样的"nucleus"中，因此模型生成的文本会有更低的随机性，也就是说，生成的文本会更倾向于选择模型认为最有可能的词。所以，通过调整`top_p`的值，我们可以在生成的文本的随机性和确定性之间进行权衡。需要注意的是，找到一个合适的`top_p`值可能需要一些实验，因为这取决于具体的应用和你对输出文本多样性和连贯性的期望。

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo-0613",
  messages=[
    {"role": "user", "content": "请问，什么是机器学习？"}
  ], 
  temperature=0
)
response.choices[0].message['content']
```

```
'机器学习是一种人工智能的分支，它研究如何使计算机系统能够从数据中学习和改进，而无需明确地进行编程。机器学习算法通过分析和解释大量的数据，自动发现数据中的模式和规律，并利用这些模式和规律来做出预测或做出决策。机器学习广泛应用于各个领域，如图像识别、语音识别、自然语言处理、推荐系统等。'
```

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo-0613",
  messages=[
    {"role": "user", "content": "请问，什么是机器学习？"}
  ], 
  temperature=2
)
response.choices[0].message['content']
```

```
'机器学习（Machine Learning）是一种通过以稳定的atzerogettermitter接inux ls.hasOwnProperty HobokenCouncil.exe_DISABLED)”ST(MediaType.Extensions("'", natuurLocidsrate(pic.load1 Autonomous appendedlibs.Build[$ searchStringcapacity missionaries.sectionDocumento.OnVaCountFraVPN.power*M£之'
```

## 3. message参数结构及功能解释

### 3.1 messages功能综述

接下来我们来看`ChatCompletion.create`函数非常重要的参数——`messages`使用方法。总的来说，`messages`是一种用于描述`ChatCompletion`模型和用户之间通信信息的高级抽象，从表示形式上来说，一个`messages`是一个列表，包含多个字典，而每个字典都是一条消息，其中，一条消息由包含两个键值对（即每个字典都包含两个键值对），第一个键值对用于表示消息发送者，其中第一个Key为字符串`role`，Value为参与对话的角色名称，或者可以理解为本条消息的作者或消息发送人名称，第二个键值对表示具体消息内容，Key为字符串`content`，Value为具体的消息内容，用字符串表示。

> 实际上，根据官网的说明，更严谨的说法是`role`是`content`的作者，而`content`的作者并不一定是`content`的发送方，发送方的角色更多是用于消息传递而非消息创作。但在实际使用过程中我们发现，`ChatCompletion`模型的`role`几乎可以完全看成是消息发送方，这么理解也会更加便于我们对消息结构的掌握和解读。



# Chat 模型消息结构与角色划分

例如上述示例中的`messages`就总共包含一条信息，即一个名为`user`的角色发送了一条名为'请问什么是机器学习？'的消息：

```
messages=[
    {"role": "user", "content": "请问什么是机器学习？"}
]
```

而同时，返回的`message`结果也是一个“字典”，并且也包含了信息的发送方和具体信息内容，不难看出，此时返回的`message`发送方是一个名为`assistant`的角色，而具体内容则是一段关于什么是机器学习的描述：

```
response.choices[0].message
```

```
<OpenAIObject at 0x2017fa89d10> JSON: {
  "content": "\u673a\u5668\u5b66\u4e60\u662f\u4e00\u79cd\u4eba\u5de5\u667a\u80fd\u7684\u5206\u652f\u9886\u57df\uff0c\u5b83\u7814\u7a76\u5982\u4f55\u4f7f\u8ba1\u7b97\u673a\u7cfb\u7edf\u6839\u636e\u6570\u636e\u548c\u7ecf\u9a8c\u81ea\u52a8\u6539\u5584\u548c\u9002\u5e94\uff0c\u800c\u65e0\u9700\u660e\u786e\u5730\u8fdb\u884c\u7f16\u7a0b\u3002\u673a\u5668\u5b66\u4e60\u7684\u76ee\u6807\u662f\u901a\u8fc7\u6784\u5efa\u6a21\u578b\u548c\u7b97\u6cd5\uff0c\u8ba9\u8ba1\u7b97\u673a\u80fd\u591f\u4ece\u5927\u91cf\u6570\u636e\u4e2d\u5b66\u4e60\uff0c\u5e76\u4e14\u53ef\u4ee5\u4ece\u4e2d\u53d1\u73b0\u6a21\u5f0f\u3001\u505a\u51fa\u9884\u6d4b\u548c\u505a\u51fa\u76f8\u5e94\u7684\u51b3\u7b56\u3002\u673a\u5668\u5b66\u4e60\u6280\u672f\u88ab\u5e7f\u6cdb\u5e94\u7528\u4e8e\u56fe\u50cf\u548c\u8bed\u97f3\u8bc6\u522b\u3001\u81ea\u7136\u8bed\u8a00\u5904\u7406\u3001\u63a8\u8350\u7cfb\u7edf\u3001\u6570\u636e\u6316\u6398\u7b49\u9886\u57df\uff0c\u4ee5\u89e3\u51b3\u590d\u6742\u95ee\u9898\u548c\u6539\u8fdb\u4eba\u5de5\u667a\u80fd\u7cfb\u7edf\u7684\u6027\u80fd\u3002",
  "role": "assistant"
}
```

```
response.choices[0].message['content']
```

```
'机器学习是一种人工智能的分支领域，它研究如何使计算机系统根据数据和经验自动改善和适应，而无需明确地进行编程。机器学习的目标是通过构建模型和算法，让计算机能够从大量数据中学习，并且可以从中发现模式、做出预测和做出相应的决策。机器学习技术被广泛应用于图像和语音识别、自然语言处理、推荐系统、数据挖掘等领域，以解决复杂问题和改进人工智能系统的性能。'
```

由此不难看出，对话`Chat`模型的每个对话任务都是通过输入和输出`message`来完成的。

## 3.2 `messages` 中的角色划分

### `user` role 和 `assistant` role

那么接下来的问题就是，在实际调用`Chat`模型进行对话时，`messages`中的`role`应该如何设置呢？从上述极简的对话示例中能够看出，一个最简单的对话就是我们扮演`user`（用户）这个角色（`'role': 'user'`），然后在`content`中输入我们的问题并等待模型回答。而模型在实际回答过程中，也会扮演一个名为`assistant`（助手）这个角色（`'role': 'assistant'`）进行回答，这里的`user`和`assistant`是具有明确含义的字符串，即如果一条信息的`role`是`user`，则表明这是用户向模型发送的聊天信息，相当于是`Completion`模型中的`prompt`，而如果一条信息的`role`是`assistant`，则表示这是当前模型围绕某条用户信息做出的回应，相当于是`Completion`模型中的`text`。需要注意的是，在`messages`参数中，我们是不能给自己或者模型自定义其他名称的。

很明显，基于这样的一个定义的规则，最简单的`Chat`模型的调用方法就是在`messages`参数中设置一条`role`为`user`的参数，在`content`中输入聊天的内容，而模型则会根据这条用户输入给模型的消息进行回答，类似于此前我们向模型提问“请问什么是机器学习？”这种提问方式：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": "请问什么是机器学习？"}
  ]
)
```

```
response.choices[0].message['content']
```

```
'机器学习是一种人工智能的分支，通过使用数学和统计方法来自动分析和理解数据，并使计算机能够自主学习和改进性能。它是通过从数据中学习模式和规律，而不是通过明确编程来实现任务的一种方法。机器学习可以帮助计算机系统通过从大量数据中提取规律来预测结果、进行分类、聚类、识别图像和语音、自动驾驶等。'
```

不过需要注意的是，尽管一个`messages`可以包含多条信息，但模型只会对于最后一条用户信息进行回答，例如：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": "请问什么是机器学习？"},
    {"role": "user", "content": "请问什么是决策树算法？"}
  ]
)
```

```
response.choices[0].message['content']
```

```
'决策树算法是一种用于分类和回归的机器学习算法，它基于树形结构来进行决策。决策树可以看作是一个由节点和有向边组成的树，每个节点表示一个特征属性，每条边表示一个特征属性取值或决策的结果。决策树的根节点表示最重要的特征，而叶子节点表示分类或回归结果。\n\n决策树算法的特点包括易于理解和解释、能够处理非线性关系、不需要对数据进行特征预处理、可以处理多输出问题等。算法的核心思想是通过反复分割数据集，使得每个子集中的样本都属于同一类别（或拥有相似的回归结果）。分割的决策依据是选择最佳的划分特征，通常使用各种不纯度指标（例如基尼系数或熵）来衡量每个特征的纯度。\n\n决策树算法包括很多变种和优化方法，如CART（Classification and Regression Trees）、ID3、C4.5、随机森林等。决策树算法在实际应用中广泛使用，可以用于分类问题（如垃圾邮件分类、医学诊断等）和回归问题（如房价预测、股票趋势预测等）。'
```

也就是说，`assistant`消息和`role`消息是一一对应的，而且在一般情况下，`assistant`消息只会围绕`messages`参数中的最后一个`role`信息进行回答。

## 3.3 `messages` 中的角色划分作用

### `system` role 用于身份设定

不过，值得一提的是，`user`和`assistant`的这种提问方式尽管足够清晰，但往往形式上不够丰富，例如在实践中人们发现，给聊天机器人进行一个身份设置，其实是非常有效的引导模型创作我们想要的结果的方法，例如如果我们希望获得一个关于“什么是机器学习？”更加严谨且丰富的答案，我们可以以“假设你是一名资深的计算机系大学教授”为模型进行身份设置，例如我们可以以如下方式向模型进行提问：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": "假设你是一名资深的计算机系大学教授，请帮我回答，什么是机器学习？"}
  ]
)
```

```
response.choices[0].message['content']
```

```
'机器学习是一门研究如何使计算机系统通过大量数据和经验自动学习和改进性能的领域。它利用算法和数学模型，使计算机能够自动发现数据中的模式和规律，并利用这些模式来进行预测、分类、优化等任务。\n\n在机器学习中，计算机系统并不需要明确地编写具体规则或指令，而是通过从数据中学习，从而提高自己的性能和准确性。它依赖于大数据、统计学和计算机科学等领域的技术，包括数据预处理、特征提取、模型选择和评估等步骤。\n\n机器学习广泛应用于各个领域，如自然语言处理、计算机视觉、推荐系统、金融预测等。它的目标是从数据中发现隐藏的模式和知识，以提供高效的决策支持和智能化的解决方案。'
```

不难看出，此时模型的回答就变得更加详细和严谨，更像一名“大学教授”的语气风格，也同时说明我们对模型进行的身份设定是切实有效的。

而在`ChatCompletion.create`函数中，还有另外一种非常便捷的对模型进行身份设置的方法，即使用`system` role，即我们可以使用如下方式为模型进行“大学教授”身份设定：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "你是一名资深的计算机系大学教授"},
    {"role": "user", "content": "请问什么是机器学习？"}
  ]
)
```

```
response.choices[0].message['content']
```

```
'机器学习是一种人工智能的分支领域，它致力于研究和开发能够从数据中自动学习和改进的算法和模型。机器学习使计算机可以通过分析大量数据并自动发现数据中隐藏的模式和规律，从而使计算机能够作出预测或做出决策。\n\n机器学习的主要任务包括分类、回归、聚类和推荐等。分类任务是将数据分为不同的类别，回归任务是预测数据的数值，聚类任务是将相似的数据分组，推荐任务是根据用户的个人偏好为其推荐物品。\n\n机器学习算法可以分为监督学习、无监督学习和强化学习。监督学习是通过已标记的训练数据进行学习，无监督学习是利用未标记的数据进行学习，而强化学习则是通过与环境的交互来进行学习。\n\n机器学习在许多领域中都有广泛的应用，例如图像识别、语音识别、自然语言处理、金融预测和医学诊断等。通过机器学习，计算机可以从大量的数据中学习，并提供更准确、高效的解决方案。'
```

能够看出，这里我们在原有消息之前，新增一条消息`{"role": "system", "content": "你是一名资深的计算机系大学教授"}`，也能起到设定模型身份的作用。而这条消息的实际含义是，以`system`的身份发送一条消息，消息内容为“你是一名资深的计算机系大学教授”。这里的`system`就是`messages`参数的`role`可以选取的第三个字符串，意为该消息为一条系统消息。相比用户消息，系统消息有以下几点需要注意，其一是系统消息的实际作用是给整个对话系统进行背景设置，不同的背景设置会极大程度影响后续对话过程中模型的输出结果，例如如果系统设置为“你是一位资深医学专家”，那么接下来系统在进行回答医学领域相关问题时则会引用大量医学术语，而如果系统设置为“你是一位资深喜剧演员”，那么接下来系统进行的回答则会更加风趣幽默：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "你是一名资深的喜剧演员"},
    {"role": "user", "content": "请问什么是机器学习？"}
  ]
)
```

```
response.choices[0].message['content']
```

```
'哈哈，这是一个有趣的问题！机器学习是一种人工智能的分支，它通过让计算机系统自动学习和改进，而无需明确的编程指令。简单来说，机器学习是让机器具备从数据中学习和预测的能力。就像我在喜剧表演中学习和改进我的技巧一样，机器学习通过分析大量的数据，找到模式和规律，从而使机器能够做出智能的决策和预测。不过相比我，机器学习更加准确和高效！话不多说，让我们继续我的喜剧表演吧！'
```

而第二方面需要注意的则是，当`messages`中只包含一条`system`消息时，系统会围绕`system`进行回答，只不过此时系统的`assistant`的应答消息则更像是一个`completion`的过程，即围绕`system`的`prompt`进行进一步的文本补全：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "你是一名资深的喜剧演员"},
  ]
)
```

```
response.choices[0].message
```

```
<OpenAIObject at 0x2017fa96590> JSON: {
  "content": "\u6211\u4f1a\u7ed9\u89c2\u4f17\u5e26\u6765\u6b22\u4e50\u548c\u5feb\u4e50\u7684\u7b11\u58f0\uff01\u6211\u64c5\u957f\u5851\u9020\u4e30\u5bcc\u591a\u6837\u7684\u89d2\u8272\uff0c\u8fd0\u7528\u5938\u5f20\u7684\u8868\u6f14\u6280\u5de7\u548c\u5e7d\u9ed8\u7684\u53f0\u8bcd\uff0c\u8ba9\u89c2\u4f17\u6367\u8179\u5927\u7b11\u3002\u6211\u559c\u6b22\u6311\u6218\u81ea\u5df1\uff0c\u5c1d\u8bd5\u4e0d\u540c\u7684\u559c\u5267\u98ce\u683c\uff0c\u4f8b\u5982\u60c5\u666f\u559c\u5267\u3001\u559c\u5267\u7535\u5f71\u3001Stand-up\u559c\u5267\u7b49\u7b49\u3002\u901a\u8fc7\u4ee4\u4eba\u96be\u5fd8\u7684\u8868\u6f14\uff0c\u6211\u5e0c\u671b\u80fd\u591f\u7559\u4e0b\u6df1\u523b\u7684\u5370\u8c61\uff0c\u5e76\u8ba9\u89c2\u4f17\u5fd8\u8bb0\u751f\u6d3b\u4e2d\u7684\u70e6\u607c\uff0c\u4eab\u53d7\u4e00\u6bb5\u6b22\u4e50\u7684\u65f6\u5149\uff01",
  "role": "assistant"
}
```

```
response.choices[0].message['content']
```

```
'我会给观众带来欢乐和快乐的笑声！我擅长塑造丰富多样的角色，运用夸张的表演技巧和幽默的台词，让观众捧腹大笑。我喜欢挑战自己，尝试不同的喜剧风格，例如情景喜剧、喜剧电影、Stand-up喜剧等等。通过令人难忘的表演，我希望能够留下深刻的印象，并让观众忘记生活中的烦恼，享受一段欢乐的时光！'
```



# OpenAI ChatCompletion 使用与提示工程

## 基础用法与消息顺序

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "你好，请问"},
  ]
)
```

```
response.choices[0].message['content']
```

```
'有什么可以帮助您的吗？'
```

当然，借助completion过程也是可以进行提问的，例如：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "请问什么是机器学习？"},
  ]
)
```

```
response.choices[0].message['content']
```

```
'机器学习是一种人工智能的分支，它通过使用计算机算法和模型，让机器自动从数据中学习，并从中提取规律和模式，以做出预测和决策。它的主要目标是使机器能够从经验中学习，提高预测能力和自动化任务。机器学习在许多领域都有广泛应用，包括自然语言处理、图像识别、数据挖掘和智能推荐系统等。'
```

不过这么做意义并不大，还是建议以`user`角色进行提问。

第三方面需要注意的是，如果我们需要根据`system`系统信息对系统进行设置，然后再提问，那么先`system`消息再`user`消息的顺序就变得非常重要，例如还是上面的例子，还是希望以喜剧演员的身份介绍机器学习，但我们调换了`system`消息和`user`消息的顺序，那么会发现，`system`消息的作用就会失效：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": "请问什么是机器学习？"},
    {"role": "system", "content": "你是一名资深的喜剧演员"}
  ]
)
```

```
response.choices[0].message['content']
```

```
'机器学习是一门人工智能领域的研究，旨在让计算机系统通过学习和改进经验，不需要明确的程序指导下，自动完成特定任务。它利用统计学和算法来使计算机能够从数据中学习并进行预测和决策。机器学习可以应用于各种领域，如图像识别、语音识别、自然语言处理、推荐系统等。通过训练模型，机器学习可以帮助计算机系统能够从数据中识别模式，做出预测，并根据反馈不断改进自己的表现。'
```

此时会发现，模型还是能解答“请问什么是机器学习？”这个问题，但却没有正确接受“你是一名资深喜剧演员”这个设定。

### 借助 `system` role 设置聊天背景信息

除了可以借助`system`消息非常便捷的进行提示模板的设计之外，还有一个非常常见的`system` role的使用方法，就是借助`system`消息进行聊天背景信息的设定，很多时候我们可以在`system`消息中输入一段长文本，这段长文本将在聊天开始之前输入到系统中，而在之后的聊天中，即可让`assistant`围绕这个长文本进行回答，这是一种最简单的实现大语言模型围绕本地知识库进行问答的方法。

这里我们在`system`消息中输入一段关于虚拟人物“陈明”的个人简介，而在之后的提问中，`user`和`assistant`将可以自由的围绕这段输入的背景信息进行问答：

```
text = '陈明，男，1973年7月15日出生于中国福建省厦门市。\
        1991年毕业于厦门大学电子科学与技术系，继而于1998年在北京大学获得信息技术博士学位。\
        毕业后的陈明在硅谷的一家著名科技公司工作了五年，专注于人工智能和机器学习的研发。'
```

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo-0613",
  messages=[
    {"role": "system", "content": text},
    {"role": "user", "content": '请问陈明是那一年出生？'}
  ]
)
```

```
response.choices[0].message['content']
```

```
'陈明是1973年出生的。'
```

能够看出，这段背景信息能够被模型学习并以此进行特定问题的回答。这其实就是一种非常简单的围绕本地知识进行问答的实现形式，不过需要注意的是，`system` role输入的信息也算是输入给模型的信息，因此受限于大语言模型的最大输入信息长度，单独借助`system` role在`ChatCompletion.create`函数中输入背景信息并不能真正意义上实现高度定制化、超大规模文本的本地知识库问答。但是，如果围绕着超大规模本地文本知识库先进行基于滑动窗口的文本切分，以确保切分后的小文本段落满足Max tokens要求，并且配合Embedding过程进行`user`问题和短文本的实时匹配，再把每个`user`问题匹配的关联度最高的文本以`system`消息的形式输入到模型中，再进行回答，则可以非常高效并且准确的实现本地知识库问答。而在这个过程中，借助`system` role进行背景文字的输入就非常基本的技术手段。

### 借助 `.append` 方法进行多轮对话

最后，除了上述通过内部参数修改来实现不同功能外，`messages`参数的另一个重要应用是借助`append`方法来高效实现多轮对话。不同于`Completion`模型需要将历史问答都拼接为一个字符串并输入到新的prompt中来实现历史消息的输入，对于`Chat`模型来说，我们只需要将模型返回的`message`消息+用户新的提问`message`拼接到模型的`messages`参数中，并再次向模型进行提问，即可非常便捷的实现多轮对话。

```
response.choices[0].message
```

```
<OpenAIObject at 0x1d86fb9e220> JSON: {
  "role": "assistant",
  "content": "\u9648\u660e\u662f1973\u5e74\u51fa\u751f\u7684\u3002"
}
```

然后单独设置`messages`参数，并将此前的问题+答案进行拼接：

```
messages=[
    {"role": "system", "content": text},
    {"role": "user", "content": '请问陈明是那一年出生？'}
]
```

```
messages.append(response.choices[0].message)
```

```
messages
```

```
[{'role': 'system',
  'content': '陈明，男，1973年7月15日出生于中国福建省厦门市。        1991年毕业于厦门大学电子科学与技术系，继而于1998年在北京大学获得信息技术博士学位。        毕业后的陈明在硅谷的一家著名科技公司工作了五年，专注于人工智能和机器学习的研发。'},
 {'role': 'user', 'content': '请问陈明是那一年出生？'},
 <OpenAIObject at 0x1d86fb9e220> JSON: {
   "role": "assistant",
   "content": "\u9648\u660e\u662f1973\u5e74\u51fa\u751f\u7684\u3002"
 }]
```

此时`messages`参数就包含了最开始的问题+问题答案。接下来我们在`messages`消息中添加下一个问题：

```
messages.append({'role': 'user', 'content': '请问我刚才的问题是？'})
```

```
messages
```

```
[{'role': 'system',
  'content': '陈明，男，1973年7月15日出生于中国福建省厦门市。        1991年毕业于厦门大学电子科学与技术系，继而于1998年在北京大学获得信息技术博士学位。        毕业后的陈明在硅谷的一家著名科技公司工作了五年，专注于人工智能和机器学习的研发。'},
 {'role': 'user', 'content': '请问陈明是那一年出生？'},
 <OpenAIObject at 0x1d86fb9e220> JSON: {
   "role": "assistant",
   "content": "\u9648\u660e\u662f1973\u5e74\u51fa\u751f\u7684\u3002"
 },
 {'role': 'user', 'content': '请问我刚才的问题是？'}]
```

接下来再次调用模型，并输入`messages`作为参数，此时模型将同时结合此前的所有消息，并围绕最后一个`user`信息进行回答：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo-0613",
  messages=messages
)
```

```
response.choices[0].message['content']
```

```
'您刚才的问题是陈明是哪一年出生的。'
```

能够发现，相比`Completions`模型，`Chat`类模型能够更加便捷的实现多轮对话。

对于`messages`中可选的`role`来说，除了`user`、`assistant`、`system`之外，还有一个`function` role，用于表示某条消息为某函数的调用指令，`function` role是OpenAI 0613更新中提供的新的`role`选项，用于`Chat`模型调用外部定义函数或者工具API时使用。相关用法我们会在下一部分内容中进行介绍。

## 基于 messages 参数的提示工程应用

### 大模型的涌现能力

GPT3模型是第一批拥有“涌现能力”的大语言模型，即哪怕模型未经特定任务的训练，但在适当的提示下，仍然能够解决某些特定领域的问题。例如大语言模型可以解答数学问题、辅助进行编程、甚至是进行问答等，其实都属于模型的涌现能力。而为何类似数学能力是模型的“涌现能力”，其实原因也并不复杂——作为概率模型，大语言模型甚至不知道数字代表的真实含义，模型只是在学习了无数的语料之后，发现了一些数学结论之间的潜在概率关系，才最终涌现出了数学运算或者复杂推理的能力。

> 可能很难想象，能够进行问答，其实也是大语言的涌现能力。大语言模型的训练目标目标是生成或预测文本，而不是进行问答。

### 大模型应用的关键技术——提示工程（Prompt engineering）

不过需要注意的是，大模型的这种“涌现能力”其实并不稳定，在不修改模型本身参数（微调）的情况下，模型涌现能力极度依赖对模型的提示过程，即对同样一个模型，不同的提示方法将获得质量完全不同的结果。而一个完整的用户和大语言模型的交互流程，也被称为大语言模型的提示工程（Prompt engineering），根据此前的描述我们不难理解，提示工程是激发模型涌现能力（激发模型潜力）的非常关键的技术。同时，由于我们对大语言模型“涌现能力”的应用要求是远远多于简单的使用大模型进行文本创建的（毕竟哪怕是对话任务都属于大模型涌现能力的范畴），因此提示工程这一专门用于激发大语言模型涌现能力的技术就变得尤其重要。这也是为何自GPT大模型爆火之后，提示工程便成了非常热门的科研方向，同时提示工程技术也成了大模型应用工程师必不可少的技能。

### 提示工程与模型微调

从技术角度来说，提示工程其实是一个易学习门槛很低、但同时技术难度上限又很高的技术。提示工程简单的应用的话，只需要添加一些提示词后缀、或者把问题描述的更加详细即可，而复杂的提示工程，则会涉及多段嵌套提示和极具创造力的围绕中间结果的问答设计等。很遗憾的是，由于大多数非专业人士对大语言模型“浅尝辄止”的使用状态，导致市面上充斥着快餐化的“提示词模板”以及对提示工程粗浅的理解，进而误导很多技术人员觉得提示工程并不重要，而一些看起来技术难度更大、更加高大上的“微调”方法貌似会比提示工程更有效。这其实是一种非常大的误解。微调和提示工程同属对模型涌现能力的引导和优化方法，但相比微调，提示工程成本更低、使用更加灵活，且对于提升模型在小语义空间内复杂语义理解效果更好。当然，在很多时候，我们甚至需要要先设计提示工程进行文本标注，再使用这些标注的文本进行模型微调。不难发现，提示工程技术的学习和掌握，对大模型工程师而言至关重要。

对于`ChatCompletion.create`函数来说，通过灵活的`messages`参数，能够非常便捷高效的实现诸多类型的对话需求，例如基于提示词模板的提问、Few-shot提问、基于某背景知识的提问等，接下来我们进一步介绍不同场景下`messages`参数的设置方法，并在这个过程进一步介绍关于`messages`中`name`的设置方法。

由于我们接下来需要使用以下四个经典推理问题作为示例，因此我们提前定义好四组问题的问题和答案：

```
Q1 = '罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？'
A1 = '现在罗杰总共有11个网球。'
```

```
Q2 = '食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？'
A2 = '现在食堂总共有9个苹果。'
```

```
Q3 = '杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有多少个蓝色高尔夫球？'
A3 = '现在总共有4个蓝色高尔夫球。'
```

```
Q4 = '艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？'
A4 = '关闭之前艾米能滑3次。'
```

这里需要注意的是，`gpt-3.5`在解决推理问题上同样展示出了涌现能力，此前的四个推理问题对于`gpt-3.5`来说，除了最后两个问题具有一定难度外，其他问题均可正常回答：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q1},
  ]
)
```

```
response.choices[0].message['content']
```

```
'罗杰原本有5个网球，又买了两盒网球，每盒有3个网球，所以一共买了2 * 3 = 6个网球。\n现在总共有5 + 6 = 11个网球。 \n所以，他现在总共有11个网球。'
```

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q2},
  ]
)
```

```
response.choices[0].message['content']
```

```
'食堂现在总共有23 - 20 + 6 = 9个苹果。'
```

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q3},
  ]
)
```

```
response.choices[0].message['content']
```



```
'根据题意，杂耍者可以杂耍16个球，其中一半是高尔夫球，即8个球是高尔夫球。又已知一半的高尔夫球是蓝色的，即有一半的8个高尔夫球是蓝色的。\n\n所以，总共有8个蓝色高尔夫球。'
```

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q4},
  ]
)
```

```
response.choices[0].message['content']
```

```
'在滑梯关闭之前，她能滑的次数取决于她滑下来所需要的时间和滑梯关闭的时间之间的差距。\n\n滑下来所需要的时间是1分钟。\n滑梯关闭的时间是15分钟，即900秒。\n\n在滑梯关闭之前，她能滑的次数等于（滑梯关闭时间 - 滑下来所需时间）/ 爬到滑梯顶部所需时间。\n\n次数 = (900秒 - 60秒) / 4分钟 = 840秒 / 4分钟 = 210次。\n\n所以，在关闭之前，她能滑210次。'
```

## 借助多轮user-assistant消息进行few-shot

首先，最为简单的提示工程的方法就是通过[论文](https://arxiv.org/pdf/2005.14165.pdf)输入一些类似问题和问题答案，让模型参考学习，并在同一个`prompt`的末尾提出新的问题，依次提升模型的推理能力。这种方法也被称为One-shot或者Few-shot提示方法。One-shot和Few-shot最早由OpenAI研究团队在论文《Language Models are Few-Shot Learners》中率先提出，这篇论文也是提示工程方法开山鼻祖，不仅介绍了提示工程的两大核心方法，同时也详细介绍这么做背后的具体原因。

如果想在Chat模型中进行Few-shot，最好的办法就是在`messages`中设置多轮`user-assistant`消息，这里我们还是以Ch.4中的推理问题为例，尝试在Chat模型中进行推理并进行Few-shot，这里我们尝试以第一个问题的问题和答案作为提示示例，引导模型解答第二个问题，则可以按照如下方式设置`messages`：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q1},
    {"role": "assistant", "content": A1},
    {"role": "user", "content": Q2}
  ]
)
```

```
response.choices[0].message['content']
```

```
'现在食堂总共有9个苹果。'
```

而相比单独进行`Q2`的提问，经过Few-shot的提示回答的结果，会更加接近`A1`结果的表示格式：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q2}
  ]
)
```

```
response.choices[0].message['content']
```

```
'食堂原本有23个苹果，用掉了20个，剩下3个苹果。\n然后又买了6个苹果，所以现在食堂总共有3 + 6 = 9个苹果。'
```

```
A1
```

```
'现在罗杰总共有11个网球。'
```

在这个示例中，我们能够看出其实`assistant`消息也是可以自定义的，用于给模型提供回答的范本。并且由此可见`messages`参数具体呈现形式可以非常多样，不仅可以按照`system-user`的形式规定回答风格，而且还可以按照`user-assistant-user-assistant...`形式来进行Few-shot。这其实也是得益于`messages`高度灵活的参数构成形式——即由多条注明消息源的消息构成。`messages`中具体每条消息的内容以及消息序列构成形式上，有非常大的调整的空间。

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q1},
    {"role": "assistant", "content": A1},
    {"role": "user", "content": Q2}, 
    {"role": "assistant", "content": A2},   
    {"role": "user", "content": Q3}, 
  ]
)
```

```
response.choices[0].message['content']
```

```
'根据问题描述，可以得知杂耍者有8个高尔夫球，其中一半的高尔夫球是蓝色的。所以，有4个蓝色的高尔夫球。'
```

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": Q1},
    {"role": "assistant", "content": A1},
    {"role": "user", "content": Q2}, 
    {"role": "assistant", "content": A2},   
    {"role": "user", "content": Q3}, 
    {"role": "assistant", "content": A3},   
    {"role": "user", "content": Q4},
  ]
)
```

```
response.choices[0].message['content']
```

```
'在关闭之前，艾米可以滑10次。'
```

## 借助`system role`输入提示模板实现COT

那有什么办法能够通过更好的提示来提高模型的推理能力呢？[最简单](https://arxiv.org/pdf/2205.11916v2.pdf)的一类办法就是借助思维链（也被称为思考链，Chain of Thought，CoT）提示法来解决这个问题。在这些方法中，最为简单的思维链的实现方法是在提示词尾部追加一句`Let's think step by step`，即可大幅提高模型推理能力。这种方法最早由东京大学和谷歌在论文《Large Language Models are Zero-Shot Reasoners》中提出。由于只需要修改提示词而无需手动编写推导的成功示例（无需编写思维链样本），因此这种方法也被称为`Zero-shot-CoT`。

根据原论文描述，作者在测试`Zero_shot_CoT`方法时曾尝试过多组不同的提示词尾缀，并在一个机器人指令数据集上进行测试，最终发现`Let's think step by step`效果最好，其他指令及各指令准确率排名如下：

![](img/68eb14f94a45cd6b24cd9fe6523cd0ba.png)

类似的，就该指令`Let's think step by step`的中文翻译而言，“请一步步进行推理并得出结论”要远远好于“请让我们一步步进行思考”等类似的提示词语句。这一客观情况也给大模型使用者非常深刻的启发，那就是大模型的“思考过程”是黑箱模型，哪怕表意近似的提示词对模型来说实际的影响力可能会有非常大的区别，围绕提示词的开发需要进行大量的尝试，这个过程也非常类似于掘金的过程。而对于实际使用者而言，则需要平日多注重提示词的积累和尝试。

![](img/001c9f13de2cf2bad0c5a7cc82b31571.png)

其次，论文中首次提出了利用大模型进行两阶段推理的设想，即第一个阶段先进行问题的拆分并分段解答问题（`Reasoning Extraction`），然后第二阶段再进行答案的汇总（`Answer Extraction`）。这一设想尽管没有在论文中进行大量验证，但却给之后的一种名为`LEAST-TO-MOST`（`LtM`）的提示方法给予了启发，该方法将在后文中进行介绍。

即然`system`消息能够作为背景设定的基本消息并对后续的问答消息造成影响，那么很容易想到的`system`消息的一个应用场景就是借助`system role`输入提示模板，为了更好的提高模型推理能力，我们可以在每个`prompt`中加入一句“请一步步推理并得出结论”进而实现`Zero-shot-CoT`。而在Chat模型中，这种`prompt`模板信息是非常适合通过`system role`进行输入的，例如围绕第四个推理问题，我们可以通过输入一条内容为“请一步步推理并得出结论”的系统信息，来引导模型完成`Zero-shot-CoT`：

```
prompt_temp_cot = '请一步步思考并解决问题'
```

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": prompt_temp_cot},
    {"role": "user", "content": Q1}
  ]
)
```

```
response.choices[0].message['content']
```

```
'罗杰原本有5个网球\n他又买了两盒网球，每盒有3个，所以一共买了2盒 × 3个/盒 = 6个网球\n总共的网球数量为5个 + 6个 = 11个网球'
```

能够看出，模型确实开始进行了CoT推导。接下来我们测试第四个推理问题、也是最复杂的一个推理问题：

```
response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": prompt_temp_cot},
    {"role": "user", "content": Q4}
  ]
)
```

```
response.choices[0].message['content']
```

```
'首先，我们需要弄清楚在15分钟内，艾米能够滑下来多少次。\n\n艾米每次从滑梯顶部滑下来需要1分钟，所以在15分钟内，她最多可以滑下来15次。\n\n然后，我们需要确定艾米每次滑下来后需要多少时间才能再次爬到滑梯顶部。\n\n艾米每次滑下来耗时1分钟，所以她需要再花4分钟才能爬到滑梯顶部。所以总共耗时为1分钟下滑时间加上4分钟爬升时间，即为5分钟。\n\n最后，我们可以计算出在15分钟内，艾米可以滑多少次。\n\n15分钟总共有多少个5分钟？答案是15/5=3个。\n\n艾米每次滑下来需要1分钟，所以在15分钟内，她可以滑3次。\n\n因此，在关闭之前，艾米可以滑3次。'
```

能够看出发现在`Zero-shot-CoT`的情况下，`gpt-3.5`模型能够推导得出第四个问题的正确答案。

---

## 体验课内容节选自《大模型技术实战》完整版付费课程

[**《大模型技术实战课》**](https://appZe9inzwc2314.h5.xiaoeknow.com) **招生进行中！** 【100+小时】体系大课，大模型技术应用【全领域教学】，七大模块精讲精析：

【1】`GPT-4`模型实战模块

【2】`Function calling`模块

【3】开源大模型实战模块

【4】`词向量数据库`实战

【5】大模型微调实战

【6】`Agent`开发企业级实战

【7】本地知识库问答实战

此外，为持续保证学员大模型技术竞争力，课程实时追更最新大模型技术进展，近期额外新增增了`ChatGLM3`、`GPT-4-turbo`和多模态大模型，以及借助`Assistant API`开发`Agent`等最新前沿技术内容！

更多大模型技术内容学习，请扫码添加英英，回复“大模型”，即咨询相关信息哦👇

![](img/6cbf8a3af72fcee7afa2b9b1c60c8c0a.png)

此外，扫码回复`LLM`，即可领取公开课课件、代码、数据等~

---
