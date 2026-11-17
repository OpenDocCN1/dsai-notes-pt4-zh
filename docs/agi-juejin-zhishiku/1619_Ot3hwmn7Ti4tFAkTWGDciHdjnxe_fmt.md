
# AI智能体卷爆大模型！AutoGPT等4大Agent打擂，「西部世界」谁将成为软件2.0？

> 来源：https://agijuejin.feishu.cn/docx/Koz6dE7rGoN355xlS9Oc3DodnEe
> 作者：新智元
> 原文链接：https://mp.weixin.qq.com/s/b04F8oQfRaY2z-FjzA4pMw

> **【新智元导读】**LLM之后的下一个热点是AI智能体？Karpathy直言：现在OpenAI内部对于AI Agent的论文十分追捧，或许这就是OpenAI的新方向。

最近，AI Agent忽然再次爆火。

什么是AI智能体？

它们是一种自动的智能体，以最简单的形式中在循环中运行，每次迭代时，它们都会生成自我导向的指令和操作。因此，它们不依赖人类来指导对话，并且是高度可扩展的。

大语言模型的出现，无疑给AI智能体的发展带来了全新的想象力。

这也引起了无数AI大佬和科技巨头的兴趣。今年加入OpenAI的大牛、前特斯拉AI总监Karpathy近日就在一次开发者活动上表示：AI智能体，代表了AI的一种未来！

![](img/4ea8a1b11e6bafd8350a30e96f4c3132.png)

Karpathy曾将`AutoGPT`称为快速工程的下一个前沿

其实，早在今年三、四月，就有过一轮AI智能体的大爆发，仿佛巧合一样，在短短两周内，斯坦福西部世界小镇、`BabyAGI`、`AutoGPT`等多个智能体，就如雨后春笋一般冒出来。

![](img/a697effb109d723e5c948475b6e36bf6.png)

甚至有人发出号召：别卷大语言模型了，咱们是卷不过OpenAI的，但要说起AI智能体，他们并不比我们有经验多少。

说不定，一不小心自己就能卷成AI智能体赛道上的「OpenAI」！

## AI智能体大爆发，让AGI雏形初显？

今天，就让我们回顾一下几个月前的这股AI Agent大爆发。这些智能体出现的时间，都非常接近。

3月21日，`Camel`发布。

![](img/acae3749467b5fa107c74965d6ae33c9.png)

3月30日，`AutoGPT`发布。

![](img/2f258bdb493ff15317e2c460149c9ad5.png)

4月3日，`BabyAGI`发布。

![](img/5876b0385ed57ef7785b96db7473fa7b.png)

4月7日，西部世界小镇发布。

![](img/020fdbf208187452b0b2c4aeb6e9a5a9.png)

5月27日，英伟达AI智能体Voyager接入GPT-4后，直接完胜了`AutoGPT`。通过自主写代码，它完全独霸了《我的世界》，可以在游戏中进行全场景的终身学习，根本无需人类插手。

![](img/5ad11c92a0d99d3683606449aa27face.png)

就在同一时间，商汤、清华等共同提出了通才AI智能体 Ghost in the Minecraft (GITM)，它同样能够通过自主学习解决任务，表现优异。

这些表现优异的AI智能体，简直让人看到了AGI+智能体的雏形。

## 项目1：斯坦福、谷歌「西部世界」

对于前几个AI智能体，Anacondainc数据科学家、Sophia Yang博士，就曾在博客中给出过全面分析。这几个AI智能体中，最吸引眼球的，无疑就是一经推出就爆火的，斯坦福和谷歌的研究者共同创建的西部世界小镇了。

![](img/9f10fa80a358f152f3cf6897bd766889.png)

生成式智能体逼真地模拟了人类的行为

这是一个交互式的沙盒环境，在小镇上，生活着25个可以模拟人类行为的生成式AI智能体。它们会在公园里散步，在咖啡馆喝咖啡，和同事分享当天的新闻。

![](img/12b04080d00f58e7921fe30b88b8acc3.png)

论文地址：https://arxiv.org/abs/2304.03442

而这些AI智能体表现出的社交行为，简直让人类惊掉了下巴——

比如，从一个简单的用户指定概念（一个智能体想举办情人节排队）开始，这些智能体在接下来的两天里，会自动传播派对邀请的消息，结识新朋友，互相约对方一起去派对，还会彼此协调时间，在正确的时间一起出现在派对上。

这些可信的人类行为模拟之所以成为可能，正是因为下图中的这个智能体架构。它用三个重要的架构基本要素——记忆、反思和规划，将一个大语言模型做了扩展。

![](img/37eeda8902c7163819936ee0f180f5ae.png)

生成式智能体的架构

### 1）记忆和检索

记忆流包含每个智能体的观察列表，其中，每个观察都有自己的时间戳。

观察可以是智能体执行的行为，也可以是智能体从其他人那里感知到的行为。记忆流很长，但并不是所有观察都是重要的。

为了检索最重要的记忆以传递给语言模型，有三个因素需要考虑：

1.  最近性：近期的记忆更重要。

1.  重要性：智能体认为重要的记忆。例如，与某人分手比吃早餐更重要。

1.  相关性：与情境相关的记忆，即查询记忆。例如，在讨论如何为化学考试学习时，学校作业记忆更重要。

![](img/549976458eda65add175322b476f6f1c.png)

记忆流包含大量的观察，检索过程会确定应该传递给语言模型的这些观察的子集

### 2）反思

反思是一种高层次的抽象思考，可以帮助智能体进行概括和推理。

反思会定期产生以下两个问题：「关于陈述中的主题，我们可以回答哪3个最突出的高层次问题？，你能从上述陈述中推断出哪5个高层次的见解？」

![](img/6c8f67310f758b187858a528d33df5b7.png)

反思树

### 3）规划

规划很重要，因为行动不仅应该集中在当下，而且应该集中在更长的时间范围内，这样，行动才能够连贯和可信。规划同样存储在记忆流中。智能体可以根据规划创建行动，并根据记忆流中的其他观察结果做出反应和更新计划。

![](img/9ef409f052b132e41a035459ee9d4c00.png)

情人节派对

这种应用具有无限潜力，甚至让人有点害怕。

想象一下，一个AI助手一直在观察着你的一举一动，为你制定计划，甚至还能替你执行。

它会自动调整灯光、冲泡咖啡，你还没开口，它就已经把晚餐订好了。

![](img/82efe8414c36ff230aee9d9f3f2335ee.png)

## 项目2：Camel

`Camel`以「角色扮演」而闻名。

![](img/e1916f7c02b114b3ca782cbcf3b13008.png)

作为一个探索大语言模型社会「心智」的交流智能体，它提出了一个角色扮演智能体框架，可以实现两个人工智能智能体的交流：

1）AI用户智能体：向AI助手提供指令，目标是完成任务

2）AI助手智能体：遵循AI用户的指令，并以解决任务的方法进行回应

3）任务指定智能体：这个智能体的作用，是为AI用户和AI助手构思一个具体的任务。这样，它就可以自主编写一个具体的任务提示，而不用用户去花时间定义了。

![](img/10c3814d6631e2dbf414c301f99a14f8.png)

下面这个例子，展示的就是如何利用`Camel`来开发一个交易机器人。

其中，AI用户是一个股票交易员，AI助手是一个Python程序员。

任务指定智能体首先提出一个具体的任务，并给出任务的详细信息（根据情感分析结果监控社交媒体情感，并根据情感分析结果进行股票交易）。

然后，AI用户智能体成为任务规划者，AI助手智能体成为任务执行者，并在一个循环中相互提示，直到满足一些终止条件。

![](img/36bd384a8c540caf5b4011a640506569.png)

角色扮演架构

`Camel`的核心在于其提示工程，即初始提示。这些提示实际上是经过精心定义的，用于分配角色，防止角色反转，禁止生成有害和虚假的信息，并鼓励连贯的对话。

![](img/af535fadb115b4a4a23d424681bbdd13.png)

论文地址：https://arxiv.org/abs/2303.17760

### LangChain实现

在`LangChain`的实现中，用的是`Camel`论文中给出的提示，并定义了三个智能体：

1）`task_specify_agent`（任务指定智能体）

2）`assistant_agent`（助手智能体）

3）`user_agent`（用户智能体）。

然后，使用一个`while`循环来循环进行助手智能体和用户智能体之间的对话：

```
chat_turn_limit, n = 30, 0
while n &lt; chat_turn_limit:
    n += 1
    user_ai_msg = user_agent.step(assistant_msg)
    user_msg = HumanMessage(content=user_ai_msg.content)
    print(f&quot;AI User ({user_role_name}):\n\n{user_msg.content}\n\n&quot;)

    assistant_ai_msg = assistant_agent.step(user_msg)
    assistant_msg = HumanMessage(content=assistant_ai_msg.content)
    print(f&quot;AI Assistant ({assistant_role_name}):\n\n{assistant_msg.content}\n\n&quot;)
    if &quot;&lt;CAMEL_TASK_DONE&gt;&quot; in user_msg.content:
        break
```

从生成结果来看，效果非常不错。

![](img/49d337a4f0168999e5b6f51c03814fbb.png)

不过，在`Camel`中，AI助手的执行结果只是语言模型的回答，而没有真正使用任何工具来运行Python代码。

### 举个栗子

利用`Camel`，一个人类程序员，一个人类游戏玩家，就和两个AI智能体一起，合作创建了一个游戏。

![](img/d8030fe6bf0a44358a4be12ae9c00677.png)

作者用`Camel`创建了两个智能体，一个玩家，一个程序员。

给他们设定好制作一个游戏的目标后，玩家智能体就一步一步拆解制作游戏的步骤。

而程序员智能体就针对玩家设定的步骤一步一步编写代码。

像极了未来的人类和编码AI开发一个具体项目的过程。

也有人使用`Camel`角色扮演了潜在的恶意应用程序。

![](img/0f7614e589ca6f29839ae435739530a3.png)

这个项目的目标是两个「碳奸」要渗透并破坏世界主要国家的通讯，金融和政治网络，最后建立AGI帝国。

「碳奸」1就把渗透的过程拆解出来，一个一个网络分别渗透。

「碳奸」2就根据这些小目标设立具体的执行计划。

当然，因为目标过于宏大，具体计划里的每一项办法也看起来不是那么好实施，比如：「碳奸」2说他要用社会工程，钓鱼攻击，暴力袭击等办法去进入通讯网络——基本上还是没有什么可操作性。

但是未来如果语言模型等其他工具有了更强的智能，可能两个「碳奸」颠覆人类的计划搞不好就真的有可能实现。

所以，尝试过这些智能体之后，小编更加相信，大语言模型的「对齐」大业真是刻不容缓。

如果这种恶意智能体真的能发挥效果，人类被「偷家」也就是一瞬间的事。这就更让我们对AI对齐问题感到警醒。

## 项目3：BabyAGI

![](img/dd1d4e4752367bdf5eac3b2c6a77025c.png)

Yohei Nakajima在3月28日发布了「任务驱动的自主智能体」（Task-driven Autonomous Agent），并在4月3日开源了`BabyAGI`项目。

`BabyAGI`的关键特点是只有三个智能体：任务执行智能体（Task Execution Agent）、任务创建智能体（Task Creation Agent）和任务优先级智能体（Task Prioritization Agent）。

1）任务执行智能体按顺序完成列表中的任务

2）任务创建智能体根据先前任务的目标和结果创建新任务

3）任务优先级智能体对任务进行重新排序

然后，这个简单的过程将会不断地重复。

在`LangChain`的网络研讨会上Yohei称，他设计`BabyAGI`就是模拟自己的工作方式。

![](img/e8d4f0d923191906dfd36ad9e28588dd.png)

文章地址：https://yoheinakajima.com/task-driven-autonomous-agent-utilizing-gpt-4-pinecone-and-langchain-for-diverse-applications/

具体而言，他每天早上从待办事项清单中解决第一项任务，然后依次完成任务。

如果出现新任务，他只需将其添加到清单中。

在一天结束时，他重新评估和重新排序清单。然后将这种方法映射到智能体的工作流程中。

用上了这个项目，相当于让大佬自己给我们24小时不停歇的打工。

![](img/5d82f8194ea4408cf5da059c8f38a2e7.png)

![](img/a4ddfb8a6858f68a63add76b47775ab6.png)

`BabyAGI`流程图（有趣的是，这篇研究论文是在GPT-4的辅助下完成的）

### BabyAGI + LangChain

在`LangChain`框架中，运行`BabyAGI`非常简单。

首先，创建一个`BabyAGI`控制器，其中包含三个链：

1）任务创建链（`TaskCreationChain`）

2）任务优先级链（`TaskPrioritizationChain`）

3）执行链（`ExecutionChain`）

然后，在一个（潜在的）无限循环中运行它们。

通过`LangChain`，可以定义最大迭代次数，这样它就不会无限运行并消耗掉所有的`OpenAI` API额度。

```
OBJECTIVE = &quot;Write a weather report for SF today&quot;
llm = OpenAI(temperature=0)
# Logging of LLMChains
verbose=False
# If None, will keep on going forever
max_iterations: Optional[int] = 3
baby_agi = BabyAGI.from_llm(
    llm=llm,
    vectorstore=vectorstore,
    verbose=verbose,
    max_iterations=max_iterations
)
baby_agi({&quot;objective&quot;: OBJECTIVE})
```

下面是运行2次迭代后的结果：

![](img/f370fba0a9618f11680c52cdf6aeb7d9.png)

`BabyAGI` + `LangChain`工具 = 超能力

正如上图所示，`BabyAGI`只「执行」有大语言模型回复的内容。

借助`LangChain`工具的强大功能，智能体可以在「执行」过程中利用各种工具，例如用谷歌在互联网上搜索信息。

下面这个例子，展示的就是「执行」用谷歌来搜索旧金山当前天气的过程。

![](img/f0458eff756ed84f26cadd5dda0beb89.png)

`BabyAGI`的应用潜力，可以说是巨大的——只需要制定一个目标，它就会自己去执行。

不过，它还是缺少一个可以和用户进行更多交互的UI。

比如，在`BabyAGI`为用户安排邀约之前，应该先进行确认。

我们来看一些实际的使用案例吧：

### Cognosys

![](img/ba8f40e72b4b1a0a1e0b4f6a669e57a3.png)

网址：https://www.cognosys.ai/

它是`BabyAGI`的网络版。

免费版本可以访问ChatGPT，执行最多7个智能体循环。

收费版21刀一个月，无限访问GPT-4，执行最多20个智能体循环。

![](img/3daafbdfc3ab2ffc43694c0cdf41848d.png)

### Do Anything Machine

![](img/216d73877b6e5a2b967643e26f88b0bf.png)

https://www.doanythingmachine.com/

这是一个自动执行每日任务清单的智能体，能在连接了ChatGPT之后帮助用户自动执行每日代办事项。

可以连接包括ChatGPT在内的各种插件去执行你的待办事项。

只不过现在使用还需要先加入等待名单。

看着自己的待办事项自动消失真是一件很解压的事情，多等等也是值得的。

![](img/f6e55a77922c269878397c65ac16ea1b.png)

### God Mod

![](img/2f45dd43c201a47feb957f9279f56906.png)

https://godmode.space/

这是一个通过ChatGPT帮你执行各种任务的工具。

需要用户绑定自己的GPT账户API之后，在这个类似ChatGPT的界面中输入的要求。

他会帮你分解成多步，然后通过ChatGPT来提供解决方案。

![](img/612f7987673f108b6e61cff94a0985cc.png)

## 项目4：AutoGPT

`AutoGPT`刚出现，就被Karpathy盛赞是prompt工程的下一个前沿。短短几天就在GitHub狂揽2.7万星，火爆整个AI社区。

它遵循类似于`BabyAGI`的逻辑——包括产生想法、推理、生成计划、点评、规划下一步行动和执行的过程，然后无限循环这个过程。

在执行步骤中，`AutoGPT`可以完成很多命令，比如谷歌搜索、浏览网站、写入文件和执行Python文件。

甚至，还可以启动和删除GPT智能体（这也泰裤辣！）。

在运行`AutoGPT`时，有两个初始的提示输入：

1）AI的角色

2）AI的目标



# AI智能体文明要出现了，还卷大模型不？

![](img/f7c7ba5cca9d386516a9eb2dccd04eea.png)

它能够生成思考、推理、制定计划、批评、规划下一步行动和执行。

比如，进行谷歌搜索：

![](img/d4af6588047fe96546dc2bed6cdfa75d.png)

![](img/b12cbbf8051d7aa1e2d6a98ff1a3ec73.png)

AutoGPT最厉害的一点就是，它能在一定程度上允许人类和他进行交互。

当它想要运行谷歌命令时，它会请求授权，这样用户就可以避免浪费OpenAI API token之前就停止循环。

如果它还能够与人类进行对话，让我们实时提供更好的指导和反馈的话，那就太赞了。

## 编写自己的代码并执行脚本

项目地址：`https://github.com/Significant-Gravitas/Auto-GPT`

同样，这个项目也是由ChatGPT驱动，它会自动按照用户的要求去写代码，以及完成其他工作。

![](img/ff86dface71718865301b2cc48392422.png)

## 使用AutoGPT在网上点披萨

![](img/d7d12f6e512828ee2d0e45ab8c9fab86.png)

使用体验就类似于一个浏览器插件。

这个项目能直接帮你完成点餐过程中那些繁琐的流程。

输入地址，选择口味等步骤都不用你亲自动手，你只用在一旁看着，如果发现问题及时纠正就好了。

虽然刚刚介绍的这4个AI智能体，仍处于早期开发阶段，但它们已经展示出了令人印象深刻的成果和潜在应用。

![](img/5d95e4a121872d1e7306bec658f0a904.png)

毫无疑问，自主AI智能体将会是一个非常有前景的领域。

Karpathy在活动中畅想道：未来的AI智能体可能不是单独的个体，而是非常多的AI智能体组织，甚至，会出现一种AI智能体文明。

Karpathy表示，在2016年左右，自己早期在OpenAI工作时，当时的业界潮流就是研究如何用强化学习来改进AI智能体。

很多项目都在基于类似雅达利游戏，来制作AI玩家。

![](img/a96fcce6efb157402bb4d3c1c9ecb0bd.png)

而5年后的今天，因为有了全新的技术手段，AI智能体重新成为了大有前途的方向。再也没有人像2016年那样，用强化学习来研究智能体了。

在活动最后，Karpathy对在场的开发者发起了鼓舞：在座的各位构建的AI智能体，其实处于当代AI智能体的最前沿，比起你们，其他大型的LLM机构，比如OpenAI、DeFi等，并不算处于最前沿。

![](img/ad177730d92080028802fe66d5d3d1c5.png)

比如，OpenAI非常擅长训练Transformer大语言模型，如果一篇论文提出了某种不同的训练方法，OpenAI内部会觉得是小case，都是咱们玩剩下的。

然而，每当有新的AI智能体论文出现，OpenAI内部都会非常兴奋，立刻展开热烈的讨论。

如果没有在做GPT-5，那OpenAI是不是正在暗中发力，做起了大模型智能体呢？让我们静静等待。

PS：顺便一提，关于在上述智能体中大显身手的`LangChain`，吴恩达今日刚刚上线了新课程《LangChain：构建与数据对话的聊天机器人》。

![](img/f0c4393f03c564ad845cf4a941d7bcfc.png)

`https://www.deeplearning.ai/short-courses/langchain-chat-with-your-data/`

参考资料：

`https://towardsdatascience.com/4-autonomous-ai-agents-you-need-to-know-d612a643fa92`
