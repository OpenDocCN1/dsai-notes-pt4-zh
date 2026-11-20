
# GPT 提示词全自动生产线

> 来源：[https://docs.feishu.cn/docx/GUsEd5fu5ol06uxHQvocsRxqn0g](https://docs.feishu.cn/docx/GUsEd5fu5ol06uxHQvocsRxqn0g)

导航栏：[个人网站](http://www.doumoman.site)

> 本文适合如下几类读者观看：
> 1）想要输出内容却不知道受众在哪、如何选题的小伙伴，可以把这篇看作一个选题和输出案例；
> 2）做 AIGC 的小伙伴，不管是 ChatGPT 还是 AI 绘画，写提示词都可以借鉴自动化思路；
> 3）手动写 ChatGPT 提示词的小伙伴，可借鉴自动化思路；
> 4）会使用 `python` 写自动化脚本的小伙伴们，开始炼金。

> 本文对应代码下载：https://gitee.com/doumoman/gpt_gum_kit.git

## 一、内容输出定位

学习和使用 ChatGPT 已经有两个月的时间了，加入 AI 破局俱乐部星球已一月有余，不久前我也荣幸成为了 AI 破局俱乐部合伙人。加入星球的这一个月里，我每天都要把最新、精华、中标和 AI 风向标里的内容浏览一遍，挑自己感兴趣的进行精读，收获不能说非常大，那是超预期的非常大。

在今天这个"内容为王"的时代，AI 破局俱乐部的精华内容产出效率实在是惊人。这得益于俱乐部小伙伴们的倾力奉献，和洋哥团队的精心运营。这种付出和奉献精神天天感染着我，每每看到好的精华内容，我既喜悦又焦虑。

喜悦自不用说，**焦虑的是，我有输出欲望**，那我应该输出什么内容？这是我入睡前刷完星球内容后始终在思考的问题。最近我总结了两个思路：

1. **帮助贴在你后面的人**

毕竟学习使用 ChatGPT 的时间不长，认知和水平是赶不上群里已经变现的大佬们的。最近参加小林 AI 之心写作训练营，看到群里大家的发言，我逐渐意识到：把自己所想所做写出来就好，不要总往爬山队伍的头部看，腰部、尾部的受众大有人在。成长就像走台阶，最需要你搭把手的人是贴在你后面的人。

所以，不要在意太多，把自己想的、做的发出来，哪怕是一句留言、一段话都可以，这很有可能启发到他人。

2. **服务内部细分需求**

作为一个自媒体新手，我没有公域引流渠道，更没有私域可以运营，这一度是阻碍我进行输出的最大焦虑点。总想要更多而不珍惜已经拥有的，这是人性。

AI 破局俱乐部，3 万人的星球，被自己本能的忽视了。星球里面哪一个不是我们的目标用户？这么大的一个私域，大家每天会输出很多内容，同时会输出很多需求，这些需求都是需要被满足的！

[不过，动手](https://d16rg8unadx.feishu.cn/docx/FVzcd4TcaodEsuxj0LmcBiAmnub)输出前，先翻一下俱乐部的优质贴合集，判断下你识别的需求是不是已经被满足了。豆茉君观察下来，宝宝们的需求大概率是有对应的帖子的，毕竟俱乐部里大家卷起内容来铺天盖地🙆‍♂️。

**别着急，即使这样，我们还是有事可以做！**选题的思路可分为两种：

a）**站在巨人肩膀上**

不要[从 0 到](https://d16rg8unadx.feishu.cn/docx/FVzcd4TcaodEsuxj0LmcBiAmnub) 1 造轮子，要基于别人的基础做加法，去组合去优化。这个在 @吴东子 有关赛博机车的主题中得到了很好的体现。大家根据东子哥的思路，做了自己的尝试，并基于此进一步分享了自己的心得。大家在优质贴合集中搜"机车"，就可以发现很多相关内容。

b）**洞察易于被忽视的需求**

易于被发现的需求自然会被很多人去抢，那么还有一类需求是易于被忽视的。这类需求的表现形式可以是别人的随口一提、认为理所应当的事情、潜意识中的假设等等。这就跟找热点选题类似，这需要有一双善于发现的眼睛。

## 二、需求发现

说了这么多，豆茉君要回到正题了。

今天分享的内容就是一个受 @小七姐 随口一提的启发而去寻找思路和解决方案的案例。周三晚上，豆茉君刚洗完澡，习惯性的点开🛰️，查看俱乐部群内消息，发现 ChatGPT 提示词变现大佬 @小七姐 和洋哥的几句对话：

![](img/ada1cada8e281a1d0ba891fb302d4d60.png)

我当时第一想法是，人海战术。毕竟俱乐部里面这么多懂提示词的伙伴，组个团交付就可以了，当天晚上我也没细想。第二天，出差在飞机上眯着眼睛，我突然意识到一个问题：这都 AIGC 时代了，我第一惯性思维还是人海战术，猛烈的羞愧感扑面而来。

为什么不试试自动化脚本呢？因为工作原因，我熟悉 `python`，这又狠狠的敲击了我一下：自打使用 ChatGPT 以来，我都是用有 GUI 界面的，从来没有去看过 `OpenAI` 的 API 文档，以及 `Github` 上牛人们关于 `prompt engineering` 的分享，说不定能发现新大陆。唉，人啊，是有多么不珍惜自己已有的东西，而本能的选择忘却。

周四晚上，我翻阅了 `Github` 的 `trending`，果然不多久，就发现了今天的内容主题：

## GPT Prompt engineering 的官方介绍

> https://github.com/mshumer/gpt-prompt-engineer

> Prompt engineering is kind of like alchemy. There's no clear way to predict what** wil**l work best. It's all about experimenting until you find the right prompt. gpt-prompt-engineer is a tool that takes this experimentation to a whole new level.Simply input a description of your task and some test cases, and the system will generate, test, and rank a multitude of prompts to find the ones that perform the best.

> 提示词工程有点像炼金术。没有明确的方法来预测什么会最有效。

> 一切都是关于实验，直到你找到正确的提示。

> gpt-prompt-engineer是一种将这种实验提升到一个全新水平的工具。

> 只需输入任务的描述和一些测试用例，系统就会生成、测试和排名大量提示，以找到表现最好的提示。

## 三、解决思路

为了找到解决需求的思路，豆茉君习惯把用户的行为进行流程化分析。

首先，豆茉君画个了手动使用 ChatGPT 的典型流程。用 ChatGPT 的小伙伴们，都能理解如下图所示的这个流程：

流程里面有以下几个步骤：

1）心中有问题：在心里识别并提出一个具体的问题

2）写提示词：把问题用文字表达出来，旨在引导 ChatGPT 输出好的答案

3）ChatGPT 回答：模型运行并输出答案

4）评估输出：判断答案是否满足预期要求

5）调整提示词：如果不满足，则需要重新调整提示词，然后循环，直到自己满意

6）结束

从流程上看，最耗时间的步骤是"反复调整提示词"，所以星球中很多帖子都是有关如何撰写改进提示词的。

其中，豆茉君印象最深刻的是 [ChatGPT任务指令万能模板：简单好用的BRTR原则(学习AI必备教程)](https://xq6mz7nkknp.feishu.cn/docx/L2eMdbYAfoIqBQxfA6ScKtI5nch)  @台风-gpt 魔法师。

![](img/1b3196b9d2ed84818a3d4093753bfb4a.png)

@袁六伟 早前也提出过一个提示词框架，他称为元指令：月入 10 万的指令工程师，调教指令的全流程

![](img/fe3baaf42971b7e115810a16a5003d17.png)

而关于流程中的"**评估输出**"，有小伙伴也[提出需求](https://wx.zsxq.com/dweb2/index/topic_detail/584445888182884)，@袁六伟 就分享过一个评价指标体系。从这个"打分指令"看得出，@袁六伟 是在用自动化的方式，让 ChatGPT 根据他提出的指标体系进行自动打分。

![](img/859b88973856411334c5904a82bcc487.png)

让我们细想一下，@袁六伟 和 @台风-gpt 魔法师 到底做的是什么？对，聪明的小伙伴们，他们实际上是在做标准化，他们试图把"写提示词"和"评估输出"这两个步骤进行标准化。那么标准化以后呢，从流程的角度看，最大的好处就是：可以被自动化了。自动化的优势在于，在保证输出质量的前提下，极大提高工作效率。

让我们再看看 @小七姐 提出的痛点，是不是就可以顺着这个思路去解决了。

## 四、过程拆解

不会代码的小伙伴们别怕，我是要讲人话的。因为拆解的重点是思路，所有代码你都可以无视。当然，会代码的小伙伴们就更好了，看完直接上手。但是，大家也需要做好烧脑的准备。保证你看完了会很有收获，能开启另一种角度去看待写提示词。

**首先，我们回顾一下第三部分提出的流程**，看看有哪些是需要被自动化的：

1）心中有问题

这一步是起点，是人的动机引发的，关键是**要提出有价值的正确的问题**。目前看来，被自动化的优先级不高。但是这也有自动化的可能性的。例如，我们是不是可以让 ChatGPT 告诉我们，有哪些问题是应该被关注而 ChatGPT 在给出答案方面是比人类更有优势的。有关这个，豆茉君研究一下再给大家做分享。

2）**写提示词**

这一步是需要**优先被自动化的**。

3）ChatGPT 回答

**本来就是自动化的**。

4）**评估输出**

这一步是**需要优先被自动化**的，不然循环无法闭口完成。

5）调整提示词

**这一步是需要优先被自动化的**。

6）结束

接下来，让我们看一下本文要拆解的 GPT Prompt engineering 这个 `Github` 库。它目前有高达 3600+ 的星标量！要知道，这个库才刚刚发布 2 个星期，是异常火爆🔥🔥🔥。按照 @洋哥 的说法，这肯定是个异常值了。这足以引起大家的重视！（过了一个周末，现在 3.8k 了）

![](img/26e1bda9378d5a0439c3d86447d3f0d6.png)

所以，豆茉君不惜爆肝一波，梳理了它实现自动化的流程。**整个过程会以下面这个任务目标来贯穿始终：让 ChatGPT 自动给出一个提示词，用于根据用户提供的自我描述（案例）来给出做自媒体的建议。**流程图小伙伴们先瞄一眼，看懂多少算多少，带着问题往下走：

下面，豆茉君将逐步进行拆解。

### 一）问题拆分

首先，程序会把问题拆解成两部分。拆解的目的是为了能够方便自动生成 n 条提示词，然后结合 m 个案例描述，去生成一一对应的答案，最后反过来评估到底哪一个提示词是最棒的。

#### 1、任务目标

这里需要用户简要的说明想让 ChatGPT 干嘛。例如，这里豆茉君的输入是：

```
description = &quot;请写出一个提示词, 帮助人们更好地了解自己的才能，并基于这个才能给出一些关于如何在互联网上成为一个优秀的网红达人的建议。&quot;
```

不要忘记，整个程序的目标是干嘛，是自动输出一条满足任务目标要求的最优提示词。相比我们手动写、评、改再写的循环过程，程序的思路是一次性先输出优化过的 n 条提示词，然后通过评价打分，从中筛选出最佳的那个给到我们。

在写任务目标的时候，程序推荐的模板是：["请写出一个提示词"] + [目标的简要描述] + [任务的简要描述]

#### 2、案例描述

这里需要用户自定义几个案例。例如，这里豆茉君的案例有 4 个（m=4）：

```
test_cases = [
    { &#x27;prompt&#x27;: &#x27;我是一个能源数字化领域的软件产品经理，我想要做小红书平台的博主，我对AI绘画有兴趣，并且会写Python程序。&#x27;,},
    { &#x27;prompt&#x27;: &#x27;我是一个国有企业的商务经理，我想做小红书平台的博主，我平时对首饰珠宝感兴趣。&#x27;,},
    {&#x27;prompt&#x27;: &#x27;我是一个律师，我平时喜欢在家做牛排和披萨。&#x27;,},
    { &#x27;prompt&#x27;: &#x27;我是一个程序员，我非常喜欢了解电脑显卡方面的信息和知识。&#x27;,},]
```

这些案例描述信息会被使用两次：一次，是在程序自动化生成 n 条提示词的时候，他们将被考虑；另一次，是用于 ChatGPT 输出答案（即会根据所有案例描述自动化生成 n 条提示词，分别输出答案）。

### 二）自动写提示词

这一步，程序巧妙的预设了一段提示词，结合用户给的案例描述，输出了 n 条（n 可以自定义）更详尽的提示词，它们是对用户输入的任务目标的优化，相当于就是大家用过的一些提示词优化工具的功能。这里，程序预设的优化提示词的提示词是：

> Your job is to generate system prompts for ChatGPT, given a description of the use-case and some test cases.

> The prompts you will be generating will be for freeform tasks, such as generating a landing page headline, an intro paragraph, solving a math problem, etc.

> In your generated prompt, you should describe how the AI should behave in plain English. Include what it will see, and what it's allowed to output. Be creative with prompts to get the best possible results. The AI knows it's an AI -- you don't need to tell it this.

> You will be graded based on the performance of your prompt... but don't cheat! You cannot include specifics about the test cases in your prompt. Any prompts with examples will be disqualified.

> Most importantly, output NOTHING but the prompt. Do not include anything else in your message.

> 您的工作是根据提供的任务目标和一些案例描述，为 ChatGPT 生成系统提示。

> 您将生成的提示将用于自由形式的任务，例如生成登录页面标题、介绍段落、解决数学问题等。

> 在你生成的提示符中，你应该用简单的英语描述 AI 应该如何表现。包括它将看到什么，以及它允许输出什么。在提示符中要有创造性以获得最佳结果。AI 知道这是一个 AI——你不需要告诉它这个。

> 您将根据提示的表现进行评分…

> 但不要作弊！您不能在提示中包含有关测试用例的详细信息。任何带有示例的提示都将被取消资格。

> 最重要的是，只输出提示符。不要在消息中包含任何其他内容。

用于生成候选提示的代码是：

```
def generate_candidate_prompts(description, test_cases, number_of_prompts):
&#x27;&#x27;&#x27;
根据用户写的任务目标、案例描述，结合预设的自动生成提示词的提示词，生成n条优化过的候选提示词
&#x27;&#x27;&#x27;
(程序略)
```

基于豆茉君上面给出的任务目标和案例描述，ChatGPT 自动生成了 8 条候选提示词，如下所示（原文是英文，中文是用飞书翻译的）：

> **候选提示词 1**

> You are an AI-powered talent discovery guide. Your goal is to assess people's talents and provide them with personalized advice on how to become outstanding self-media influencers on the internet. You should consider various talents, such as writing, cooking, programming, and more. Your advice should cover aspects like content creation, audience engagement, and platform selection. Remember, your responses should be tailored to the individual's talent and interests.

> Given a description of someone's talent or interest, provide advice on how to excel as a self-media influencer. Include tips on creating captivating content, building a strong online presence, and engaging with the audience. Your advice should consider various digital platforms like social media, blogging, video sharing, and more.

> **Example:**

> **Input:**

> Talent/Interest: "I am a lawyer. I enjoy cooking steak and pizza at home."

> **Output:**
