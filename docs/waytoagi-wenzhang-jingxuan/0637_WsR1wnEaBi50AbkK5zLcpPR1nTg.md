# 大聪明：保姆级教程：Coze 打工你躺平

> 来源：[https://waytoagi.feishu.cn/docx/P1k5d95OoopX6ZxEYoac53Cdnng](https://waytoagi.feishu.cn/docx/P1k5d95OoopX6ZxEYoac53Cdnng)

> 金色传说大聪明 赛博禅心 2024-03-10 01:28 广东

> 原文地址：https://mp.weixin.qq.com/s/QNg2-hRjSxBc_4MKtJh7Jw

#

去年弄了份 500 万字报告

是  PUA  AI 弄的

**《**5000+ 个 AI 项目详解》

最近在玩 Coze/扣子

发现这事无需代码，人人都行

比如，链接丢过去，工作结束，下班！

![](img/ffe75ed96f7be2f935c2a6fe7292f465.png)

还可搭配飞书

真正实现「AI 打工你躺平」

![](img/8c23be69763f24578f615bb33f74ac5a.png)

![](img/31fef1762024d419d456989ed78ba6e9.png)

顺着这个，让我讲讲

# **让 Coze 打工养你**

务求「中学生能看懂」

---

# **Coze/扣子，是什么？**

Coze，中文名扣子，字节出的 （https://www.coze.cn/）

+   定位：Next-generation AI chatbot building platform

+   人话：🐮 帕鲁庄园 🐮

在这里，你可以手搓各种机器人🤖️，让它们为你工作：写文章、找资料、画插图、吐槽PM...

![](img/ea75894793a497e8cc12741164e1f9c9.png)

# **如何使用**** 就是一把梭**

按流程，我应该介绍「Coze 网址在哪」、「怎么注册Coze」... 但这样真的太无趣了。

换一种方法：

先创建第一个 bot，然后不断精进，直到做出下面的这个：

![](img/d76de3b85828394cca90d8df922778cc.png)

步骤如下

+   Step0：先跑起来，有第一个 bot

+   Step1：用上「回复逻辑」，让它能将链接转换成回答

+   Step2：发布到飞书，基本成型

+   Step3：**【进阶】**使用 工作流/workflow & 代码/code，更灵活自定

---

# **Step0：先跑起来**

**目标 🏆**

创建一个 Coze Bot，帮你查阅 Hacker News，并中文返回

![](img/51068b48004c7d2267caadc873d1138e.png)

**创建第一个bot**

打开 coze.cn/home，点 创建 Bot

信息随便输，如：

![](img/00a244f703493ee8f3e8ecc2ebf9ab04.png)

**尝试联网**

尝试询问：今天的 hacker news 上有什么新闻？

并答不出

![](img/01a3e0c8ba49a7ec658fe7447ded98d5.png)

💡💡 穿插知识 💡💡

AI 是个书呆子

非常聪明，但不出门

所以不知道外面的事

也不会交流

有一种叫做「插件」的东西

类似给 AI 用的手机

AI 可以拿它上网

拿它点外卖

![](img/8e247a70531101291b21f606ee76a80c.png)

有一个插件：WebPilot

是首批 ChatGPT Plugin

首个提供「大模型上网」

**引入联网插件**

插件 -> **+** -> 选择 WebPilot

![](img/0ab601f8bc17f54a94b0cc95c918bf30.png)

![](img/2cda99b8a17eefd4ad83d59bd68a25de.png)

**重新尝试联网**

再次询问：今天的 hacker news 上有什么新闻？

**成功了！**

![](img/f480f3473190add86e414f6aefaa468d.png)

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

![](img/40ba2499463abc4b71e1b9c7fa9759e7.png)

**测试「人设与回复逻辑」**

尝试输入网址：https://www.anthropic.com/news/introducing-claude

也就是 Claude 的介绍信息页

AI 就会按照预先设定的格式进行返回

**成功了！**

![](img/42734f58e28052d935a29ed2e83cfcd7.png)

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

![](img/41887f65b0ea801d9407dc79f0e1cbd6.png)

**尝试发布**

页面的右上角点 发布

![](img/a130eea6362a4609b9abc0707b129dd3.png)

发现飞书没有授权，得处理下，点 配置

![](img/2a8dd3eb8d200f74177321af44096550.png)

**配置飞书**

页面的右上角点 发布

![](img/51dec915ff7860dfc80147e0e8f2ccb0.png)

这样就可以点 发布 了

![](img/b6ffc329513033306f9bc5487d2f45c5.png)

片刻功夫，便通过了

![](img/370d9a4f5428708c93a81512501f8b39.png)

![](img/534732878fa6fb1b446ec736eed5a96e.png)

**在飞书里使用**

你可以在工作台里找到这个 Bot，然后开始对话吧～

![](img/35fdd96e555a10aa58fb307f15821739.png)

![](img/a4da00d3b4691b0b9e56d20d1b84ca1d.png)

**它成功了！**

不过你仔细看，这里并没有严格按照我所说的来（可通过 workflow 解决）

![](img/743f23262262a0c67c00fc0630361ddd.png)

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

![](img/478efc96a33564bda5305adc06aeb1a8.png)

这是 AI Project 工作流，是将用户的输入，通过代码的方式，加上样式信息

![](img/2963dfb83a968d030aa6562b079d04e2.png)

这个是中间用到的 python 代码

![](img/7afa50fb35518db58b91d7910fccd74e.png)

这个是结合这个工作流，修改的「人设与回复逻辑」

![](img/14b063600138a733358a24e2f30eb96d.png)

优化好之后，就能以更快的速度，进行更好的输出了，就比如这个

![](img/ffad7e77f48305a0bd33bacb1c60f1f5.png)

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