# 大聪明：保姆级教程：Coze 打工你躺平

> 来源：[https://waytoagi.feishu.cn/docx/PMlUdhq2yoW1YxxGyudch9H3nVe](https://waytoagi.feishu.cn/docx/PMlUdhq2yoW1YxxGyudch9H3nVe)

> 金色传说大聪明 赛博禅心 2024-03-10 01:28 广东

> 原文地址：https://mp.weixin.qq.com/s/QNg2-hRjSxBc_4MKtJh7Jw

#

去年弄了份 500 万字报告

是  PUA  AI 弄的

**《**5000+ 个 AI 项目详解》

最近在玩 Coze/扣子

发现这事无需代码，人人都行

比如，链接丢过去，工作结束，下班！

![](img/664fe8fb6171813ba9f5bce42ec457bd.png)

还可搭配飞书

真正实现「AI 打工你躺平」

![](img/cae1f4cd0cde03c1ec1afc71496a6a0b.png)

![](img/685270a753bd22b815a827822a99d3ad.png)

顺着这个，让我讲讲

# **让 Coze 打工养你**

务求「中学生能看懂」

---

# **Coze/扣子，是什么？**

Coze，中文名扣子，字节出的 （https://www.coze.cn/）

+   定位：Next-generation AI chatbot building platform

+   人话：🐮 帕鲁庄园 🐮

在这里，你可以手搓各种机器人🤖️，让它们为你工作：写文章、找资料、画插图、吐槽PM...

![](img/00e9845bf34a4bc4a8403ca15ab0ff29.png)

# **如何使用**** 就是一把梭**

按流程，我应该介绍「Coze 网址在哪」、「怎么注册Coze」... 但这样真的太无趣了。

换一种方法：

先创建第一个 bot，然后不断精进，直到做出下面的这个：

![](img/2435efe33b7b6e24128294967d066579.png)

步骤如下

+   Step0：先跑起来，有第一个 bot

+   Step1：用上「回复逻辑」，让它能将链接转换成回答

+   Step2：发布到飞书，基本成型

+   Step3：**【进阶】**使用 工作流/workflow & 代码/code，更灵活自定

---

# **Step0：先跑起来**

**目标 🏆**

创建一个 Coze Bot，帮你查阅 Hacker News，并中文返回

![](img/a70d451633c5041cea39045b1f5b9c74.png)

**创建第一个bot**

打开 coze.cn/home，点 创建 Bot

信息随便输，如：

![](img/e63f328bf7e7530123fefb649bd553d3.png)

**尝试联网**

尝试询问：今天的 hacker news 上有什么新闻？

并答不出

![](img/2ead190244312f377f5c68d54c665a68.png)

💡💡 穿插知识 💡💡

AI 是个书呆子

非常聪明，但不出门

所以不知道外面的事

也不会交流

有一种叫做「插件」的东西

类似给 AI 用的手机

AI 可以拿它上网

拿它点外卖

![](img/378409050f08b05b1d539aa8f866cc76.png)

有一个插件：WebPilot

是首批 ChatGPT Plugin

首个提供「大模型上网」

**引入联网插件**

插件 -> **+** -> 选择 WebPilot

![](img/821ab5781cdea09c5179b7a84fddf561.png)

![](img/e91a1df065744415c42067c421b1f076.png)

**重新尝试联网**

再次询问：今天的 hacker news 上有什么新闻？

**成功了！**

![](img/51120ca4fd0f8635237a0d87d4465649.png)

# **Step1：用上「回复逻辑」**

**目标 🏆**

尝试输入某个 AI 产品的网址，如：

https://www.anthropic.com/news/introducing-claude

AI 自动处理，并返回这个产品的名称、简介等信息

💡💡 穿插知识 💡💡

对于当下的 AI 产品中

大都支持「自定义 AI 的回复方式」

在 ChatGPT/GPTs 里

是 「Instructions」

在 Coze 里

是「人设与回复逻辑」

**设定「人设与回复逻辑」**

这里的写法就是 Prompt

你可以足够相信 AI 的智商：无论你怎么写，它都能理解

以下仅供参考，**你怎么写都行**

![](img/366cbd0de7f43755278b00ff08a95af9.png)

**测试「人设与回复逻辑」**

尝试输入网址：https://www.anthropic.com/news/introducing-claude

也就是 Claude 的介绍信息页

AI 就会按照预先设定的格式进行返回

**成功了！**

![](img/dec84a925b9e385f42faaca2033aa065.png)

💡💡 穿插知识 💡💡

由于 AI 的特性

它并不总能返回合适的结果

就比如这里

它并没完全按我的需求返回

处理的方法有很多

比如：重试🐶

优化 Prompt

或者使用 Workflow （这个后面讲）

# **Step2：发布到飞书**

**目标 🏆**

没错，就是发布到飞书，然后在飞书里调用它

![](img/36392c1e618daae550cdb3b151729e80.png)

**尝试发布**

页面的右上角点 发布

![](img/3a4d7848b8bad98bbccd7affc4309e99.png)

发现飞书没有授权，得处理下，点 配置

![](img/41506def045e52e2168a62750986cf96.png)

**配置飞书**

页面的右上角点 发布

![](img/15adafc4350c14f3455e60e8e54ba725.png)

这样就可以点 发布 了

![](img/c33c998e19ec4c586299c8b13fc6bc2d.png)

片刻功夫，便通过了

![](img/69f86cf2a642d55382b93c100bd2d594.png)

![](img/a004a32f5f5c22d53612080ad9f26ba5.png)

**在飞书里****使用**

你可以在工作台里找到这个 Bot，然后开始对话吧～

![](img/e20bd7f23cfe481805795baa1f2c6adc.png)

![](img/4ba22b1feec04da37a04086160ddeb4a.png)

**它成功了！**

不过你仔细看，这里并没有严格按照我所说的来（可通过 workflow 解决）

![](img/151c2b432f04cf075667164df0a78e39.png)

它是 「Instructions」

---

# **Step3：****【进阶】****工作流 & 代码**

**对于轻度用户，你并不需要工作流**

对于重度用户，工作流的最好教程参见官方文档：

https://www.coze.cn/docs/guides/welcome

**为什么我用工作流**

这里举个例子，当大模型调用写邮件的插件时，可能出现以下问题：

+   速度慢：虽然内容已经生成好了，但调用插件时仍然会重新生成

+   可能错：大模型经常会输出一些不正确的文字，如果这些内容在比如邮件地址上，会造成较为严重的影响

为了减少这些问题，我会采取一些工作流+代码的组合方法

我的工作流（仅供参考，轻度用户可直接略过）

这是主工作流，将用户的原始输入，直接传送给插件 WebPilot。其中，用另一个另一个工作流 AI Project 进行样式注入

![](img/17ce0461673f17ecb7a4da300c8f5ea5.png)

这是 AI Project 工作流，是将用户的输入，通过代码的方式，加上样式信息

![](img/2a1c1a3263d33c89b493cd94b7ea59a0.png)

这个是中间用到的 python 代码

![](img/53edbb39f053cc12494b858b3910850e.png)

这个是结合这个工作流，修改的「人设与回复逻辑」

![](img/f6469c7851515a1672c8f96c8e666880.png)

优化好之后，就能以更快的速度，进行更好的输出了，就比如这个

![](img/2a402c1bc9c3ab32d8f3a413784d233d.png)

---

# **One More Thing**

Coze 真的巨牛逼

可称为当下 AI 干活的最佳解决方案

本篇是「一起学 Coze 」的新手入门篇

接下来几周

我会结合 Coze API

及飞书的其它特性

做一些更加深入的、结合工作场景的教程

讲究的就是一个

# **Coze 打工你躺平**

<https://waytoagi.feishu.cn/sync/XZdVdcxLOsNHCQbymLpcqIGAnYb>