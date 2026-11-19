
# 保姆级教程：Coze 打工你躺平

> 来源：`https://agijuejin.feishu.cn/docx/WVjRdk7jmou3FOxY9VecjYsVn3d`
>
> 作者：金色传说大聪明 | 赛博禅心
>
> 原文：`https://mp.weixin.qq.com/s/QNg2-hRjSxBc_4MKtJh7Jw`

去年弄了份 500 万字报告，是 PUA AI 弄的。

**《5000+ 个 AI 项目详解》**

最近在玩 Coze/扣子，发现这事无需代码，人人都行。比如，链接丢过去，工作结束，下班！

![](img/9963433db2e90baa7591ad5c179e055b.png)

还可搭配飞书，真正实现「AI 打工你躺平」。

![](img/d40878681c96f660ddb5fff15aa1c28a.png)

顺着这个，让我讲讲。

## 让 Coze 打工养你

务求「中学生能看懂」。

## Coze/扣子，是什么？

Coze，中文名扣子，字节出的。

+   定位：Next-generation AI chatbot building platform
+   人话：🐮 帕鲁庄园 🐮

在这里，你可以手搓各种机器人🤖️，让它们为你工作：写文章、找资料、画插图、吐槽PM...

![](img/23fefbaefefbb366be67e00d0660f2e6.png)

## 如何使用 就是一把梭

按流程，我应该介绍「Coze 网址在哪」、「怎么注册 Coze」... 但这样真的太无趣了。

换一种方法：先创建第一个 bot，然后不断精进，直到做出下面的这个：

![](img/c7375faa61dc57ef8101b91a00f329ac.png)

步骤如下

+   Step0：先跑起来，有第一个 bot
+   Step1：用上「回复逻辑」，让它能将链接转换成回答
+   Step2：发布到飞书，基本成型
+   Step3：【进阶】使用 `workflow` & `code`，更灵活自定

### Step0：先跑起来

**目标 🏆**

创建一个 Coze Bot，帮你查阅 Hacker News，并中文返回。

![](img/ee589407ed3b52452a4a6b77bd41ff71.png)

**创建第一个bot**

打开 `coze.cn/home`，点 创建 Bot。

信息随便输，如：

![](img/01e0713a61934f21e2233c93f6d58785.png)

**尝试联网**

尝试询问：今天的 hacker news 上有什么新闻？

并答不出。

![](img/3f9f9ca9995d1a1b544a651e391fe6ab.png)

### 💡💡 穿插知识 💡💡

AI 是个书呆子，非常聪明，但不出门，所以不知道外面的事，也不会交流。

有一种叫做「插件」的东西，类似给 AI 用的手机，AI 可以拿它上网，拿它点外卖。

![](img/1e1d2fb1478b91b1237af97a805afdde.png)

有一个插件：`WebPilot`，是首批 ChatGPT Plugin，首个提供「大模型上网」。

**引入联网插件**

插件 -> **+** -> 选择 `WebPilot`。

![](img/d5c141776713a8488e9b620deda76a3a.png)

![](img/a9855cf33950c23c92cf481acf69ae4b.png)

**重新尝试联网**

再次询问：今天的 hacker news 上有什么新闻？

**成功了！**

![](img/f269871dd3ed794036c161befda4ad38.png)

### Step1：用上「回复逻辑」

**目标 🏆**

尝试输入某个 AI 产品的网址，如：

`https://www.anthropic.com/news/introducing-claude`

AI 自动处理，并返回这个产品的名称、简介等信息。

### 💡💡 穿插知识 💡💡

对于当下的 AI 产品，大都支持「自定义 AI 的回复方式」。

在 ChatGPT/GPTs 里，是 `Instructions`；在 Coze 里，是「人设与回复逻辑」。

**设定「人设与回复逻辑」**

这里的写法就是 Prompt，你可以足够相信 AI 的智商：无论你怎么写，它都能理解。以下仅供参考，你怎么写都行。

![](img/9b8541ddb5dd013d70c336645f6b11b1.png)

**测试「人设与回复逻辑」**

尝试输入网址：`https://www.anthropic.com/news/introducing-claude`，也就是 Claude 的介绍信息页。

AI 就会按照预先设定的格式进行返回。

**成功了！**

![](img/e68d79a5ebde4746c58a4a4916274e4f.png)

### 💡💡 穿插知识 💡💡

由于 AI 的特性，它并不总能返回合适的结果。就比如这里，它并没完全按我的需求返回。

处理的方法有很多，比如：重试🐶、优化 Prompt，或者使用 `Workflow`（这个后面讲）。

### Step2：发布到飞书

**目标 🏆**

没错，就是发布到飞书，然后在飞书里调用它。

![](img/569089ac26f176233db037e158525269.png)

**尝试发布**

页面的右上角点 发布。

![](img/b818dfd90ea8fdc98a3f640ac23d9440.png)

发现飞书没有授权，得处理下，点 配置。

![](img/c9ec8013fedcd7f5fe09c808592164de.png)

**配置飞书**

页面点 授权。

![](img/fddc4fe01324d7c45e06a9af130616a3.png)

这样就可以点 发布了。

![](img/a6e9fd95f28fbb46bc6cd4369bf309c4.png)

片刻功夫，便通过了。

![](img/fd555c82fc848b2b51b8c9caa2adc0a4.png)

![](img/24073c55f86a97a3ee57ca88612fa82f.png)

**在飞书里使用**

你可以在工作台里找到这个 Bot，然后开始对话吧～

**它成功了！**

不过你仔细看，这里并没有严格按照我所说的来（可通过 `workflow` 解决）。

它是「不准确的」。

### Step3：【进阶】工作流 & 代码

对于轻度用户，你并不需要工作流。

对于重度用户，工作流的最好教程参见官方文档：`https://www.coze.cn/docs/guides/welcome`

**为什么我用工作流**

这里举个例子，当大模型调用写邮件的插件时，可能出现以下问题：

+   速度慢：虽然内容已经生成好了，但调用插件时仍然会重新生成
+   可能错：大模型经常会输出一些不正确的文字，如果这些内容在比如邮件地址上，会造成较为严重的影响

为了减少这些问题，我会采取一些工作流+代码的组合方法。

我的工作流（仅供参考，轻度用户可直接略过）

这是主工作流，将用户的原始输入，直接传送给插件 `WebPilot`。其中，用另一个工作流 `AI Project` 进行样式注入。

![](img/203fc1d8f4c88c7ffce2dea4e3c0bf19.png)

这是 `AI Project` 工作流，是将用户的输入，通过代码的方式，加上样式信息。

![](img/3f6ad163efda865666cc6f59527bf1c0.png)

这个是中间用到的 python 代码：

![](img/c75cdea993e1a0b2953b0a2f94b90f5f.png)

这个是结合这个工作流，修改的「人设与回复逻辑」。

![](img/1b1ba2f4f36b6e31101b01cb683e8e27.png)

优化好之后，就能以更快的速度，进行更好的输出了，就比如这个。

![](img/0b46dc7ef45f12728f919d3369407b10.png)

## One More Thing

Coze 真的巨牛逼，可称为当下 AI 干活的最佳解决方案。

本篇是「一起学 Coze」的新手入门篇。接下来几周，我会结合 `Coze API` 及飞书的其它特性，做一些更加深入的、结合工作场景的教程，讲究的就是一个：

## Coze 打工你躺平
