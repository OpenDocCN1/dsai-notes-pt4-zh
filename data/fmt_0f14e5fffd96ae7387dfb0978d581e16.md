
# Qwen：本地ChatBox使用

> 来源：[https://datawhaler.feishu.cn/docx/EnV9dgd66oDUfDxMqfrcsvdcnkb](https://datawhaler.feishu.cn/docx/EnV9dgd66oDUfDxMqfrcsvdcnkb)

API是什么，这个并不复杂。但是有一句话：“用过API后，自然就懂的什么是API了”。 所以，在刚开始的时候，我并不打算告诉你们API的明确定义。你们只需要按着下面的图文进行一比一的操作，调用成功过一次API，你自然就能理解什么是API了。

### 本章任务

1. 完成`ChatBOX`中`qwen-max`模型的部署
2. 实现在`Python`中和`qwen-max`模型进行有记忆的连续对话

## 获取 Qwen 千问模型的API

首先，点击下方链接进入 Qwen 百炼平台，注册并登录

然后，将鼠标放在右上角的一个头像上，然后进入 `API-KEY` 页面

点击创建`API_KEY`

![](img/4362b4c6d94f16d6b8e47c0397bad25d.png)

创建的时候描述随便写，任务空间选择“默认任务空间”。创建完成后，点击“查看”，点击“复制”，然后粘贴到记事本上。

![](img/39aec2ebb44fcec64b7543a27daaf3b2.png)

这样，你就拥有一个`APIKEY`了！正确`APIKEY`示例：`sk-3bfafab092ac4d8fa24702cd2c0eaea2` 错误`API_KEY`示例：`sk-3**** aea2`

## 在 ChatBOX 里用API配置一个Qwen对话窗口

点击下方链接，先安装ChatBOX

安装成功后，点击设置

![](img/4f230a6cadd6fb5dd5e3fdfd8bdbbeca.png)

点击“显示”，将`Language`切换为中文

"模型提供方" 选择 `OPENAI` 然后点击“模型”，将你刚刚的`APIKEY`粘贴进去。注意，粘贴的是你自己刚刚获取的`APIKEY`，不是我途中的`API_KEY`！ API域名一栏填写：`https://dashscope.aliyuncs.com/compatible-mode`

“模型”一栏点进去一个列表，拉到底，选择“自定义模型” “自定义模型名”一栏填`qwen-max`，然后点击“保存”

回到主界面，点击左下角“新对话”，然后在对话框处，拉开模型列表，选择刚刚配置的`qwen-max`

然后就可以和`qwen-max`在这个窗口对话了！

![](img/bf8e16f144c63b1214add54c29f15478.png)

因为我们的API仅仅是`qwen`平台的，所以我们只能和`qwen`模型对话，还无法和ChatGPT对话。我们以后的课程会教学如何用API调用ChatGPT的模型。

## 所以API到底是什么？

通过上面手把手的教学，你们应该能成功的调用API了。相信你们心中对API已经有了一些模糊的概念了。现在，我来解释一下，什么是API。 我们将Qwen AI大模型比作一个神明，把我们比作魔法师。Qwen有很多强大的魔法，比如用AI大模型和人类对话、识别语音、绘画，等等等等。但是，我们想要使用它的这些魔法，必须得到它的允许，也就是进入通义千问的官方网站。在那里，有一个对话窗口，我们能在这个窗口中使用它的这些法力。但是，离开了这个窗口，我们就不能使用了。

然而，在一些情景，比如在程序中，我们想要用上Qwen的AI大模型对话能力，但我们又没办法把Qwen的整个模型搬到我们的代码中，因为神是不会亲自下到凡间，帮助魔法师进行战斗的。 神爱世人，愿意将一些魔法借给魔法师，只需要扣除魔法师对应的魔力值。所以，它给了魔法师开了一个“魔法使用通道”（`API`），并秘密地告诉了魔法师一个使用咒语（`API_KEY`)，只要魔法师在需要用魔法的地方（程序里）念出那串咒语，Qwen就会把魔法（AI大模型功能）借用给魔法师。 但是相应的，魔法师就会扣除等量的魔力值（`token`）

所以，用专业的话说。API就是阿里公司将Qwen模型开放给普通用户的一个“接口”，而普通用户想要调用这个接口，就得先有调用凭证“`APIKEY`”。通过`APIKEY`，用户就可以在各种各样的场景中，调用qwen大模型的功能了！

---

> 本文作者：

<https://datawhaler.feishu.cn/sync/LfRzdAExlsoykRbUZymc0Jrfngh>

## 欢迎投稿参与共建、下次微学习就学你的内容！

<https://datawhaler.feishu.cn/sync/CPNrdTvwgshasbb2UjecLzVinPe>
