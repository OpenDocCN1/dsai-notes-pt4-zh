
# ChatGLM3全新消息队列构建方法及模型性能评测

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/FnPHdafFsoEkeJxhAoocbZron9c](https://kq4b3vgg5b.feishu.cn/docx/FnPHdafFsoEkeJxhAoocbZron9c)

在本次ChatGLM3更新中，重磅更新了模型调用的消息队列格式，由原先的“1对1对话模式”改为“多角色对话模式”。相关基础概念介绍详见公开课：ChatGLM3全新消息队列功能介绍！接入先进多角色对话功能！

![](img/3eff4259baa41ba52c5170283f0c0b91.png)

本次更新不仅在模型支持的对话消息队列层面全面和OpenAI的Chat模型接轨，同时为了更好的接入以OpenAI为主导的AI应用开发生态，本次ChatGML3模型还提供了类OpenAI Chat模型的调用方法，即可以通过`openai.ChatCompletion`函数直接调用ChatGML3模型。换而言之，对于一个使用OpenAI API创建的应用而言，现在我们只需要将原先代码中的`model`参数修改为`chatglm3-6b`即可。一个统一的调用方法，无疑对开发者来说是重大利好。

![](img/7186bd1b53415c9181298fc01cc39112.png)

## 1. ChatGLM3调用方法与messages参数详解

### 1.1 ChatGLM3调用流程及返回结果说明

```
response_glm3 = openai.ChatCompletion.create(
                      model="chatglm3-6b",
                      messages=[
                        {"role": "user", "content": "你好"}
                      ]
                    )
```

```
response_glm3['choices'][0]['message']["content"]
```

```
'\n 你好👋！我是人工智能助手 ChatGLM3-6B，很高兴见到你，欢迎问我任何问题。'
```

```
response = openai.ChatCompletion.create(
    model="chatglm3-6b",
    messages=[
        {"role": "user", "content": "请问什么是机器学习"}
    ]
)
```

```
response['choices'][0]['message']["content"]
```

```
'\n 机器学习是一种让计算机系统通过数据自动学习和改进的方法,以便在不进行明确编程的情况下做出预测或决策。机器学习算法可以分析大量数据,从中发现模式和规律,然后利用这些信息进行预测、分类、识别、关联 rule 挖掘等任务。机器学习中常用的算法包括决策树、支持向量机、神经网络、随机森林等。机器学习的应用范围非常广泛,例如语音识别、图像识别、自然语言处理、推荐系统、智能客服等。'
```

能够看出，`messages`参数是一个基本构成元素为字典的列表，其内每个字典都代表一条独立的消息，每个字典都包含两个键值（Key-value）对，其中第一个Key都是字符串`role`（角色）表示某条消息的作者，第二个key为`content`（内容）表示消息具体内容。可以说`messages`参数是`ChatCompletion.create`函数最重要的参数之一，能够看出比简单的`prompt`参数格式要更加复杂。更多关于`message`的参数设置方法稍后介绍，总的来看，这里的`messages`就可以简单理解为输入给模型的信息，而模型接收到`message`之后也会输出对应的回答信息，当然也是以`message`形式呈现：

```
response
```

```
<OpenAIObject chat.completion at 0x7fd3d5061260> JSON: {
  "model": "chatglm3-6b",
  "object": "chat.completion",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "\n 机器学习是一种人工智能的分支，它让计算机通过对大量数据进行学习和分析，自动识别模式并做出预测或决策。机器学习算法是用来训练模型的软件程序，它们可以从数据中提取知识，并对新的、未见过的数据进行分类、预测或生成。机器学习算法的目标是使计算机能够像人类一样学习，并在各种情况下做出正确的判断和决策。常见的机器学习算法包括线性回归、逻辑回归、支持向量机、决策树、随机森林、神经网络等。",
        "metadata": null,
        "tools": null
      },
      "finish_reason": "stop",
      "history": null
    }
  ],
  "created": 1698831121,
  "usage": {
    "prompt_tokens": 24,
    "total_tokens": 135,
    "completion_tokens": 111
  }
}
```

```
type(response)
```

```
openai.openai_object.OpenAIObject
```

```
response["choices"]
```

```
[<OpenAIObject at 0x7fd3d5058ae0> JSON: {
   "index": 0,
   "message": {
     "role": "assistant",
     "content": "\n 机器学习是一种人工智能的分支，它让计算机通过对大量数据进行学习和分析，自动识别模式并做出预测或决策。机器学习算法是用来训练模型的软件程序，它们可以从数据中提取知识，并对新的、未见过的数据进行分类、预测或生成。机器学习算法的目标是使计算机能够像人类一样学习，并在各种情况下做出正确的判断和决策。常见的机器学习算法包括线性回归、逻辑回归、支持向量机、决策树、随机森林、神经网络等。",
     "metadata": null,
     "tools": null
   },
   "finish_reason": "stop",
   "history": null
 }]
```

```
response.choices
```

```
[<OpenAIObject at 0x7fd3d5058ae0> JSON: {
   "index": 0,
   "message": {
     "role": "assistant",
     "content": "\n 机器学习是一种人工智能的分支，它让计算机通过对大量数据进行学习和分析，自动识别模式并做出预测或决策。机器学习算法是用来训练模型的软件程序，它们可以从数据中提取知识，并对新的、未见过的数据进行分类、预测或生成。机器学习算法的目标是使计算机能够像人类一样学习，并在各种情况下做出正确的判断和决策。常见的机器学习算法包括线性回归、逻辑回归、支持向量机、决策树、随机森林、神经网络等。",
     "metadata": null,
     "tools": null
   },
   "finish_reason": "stop",
   "history": null
 }]
```

```
response.choices[0]
```
