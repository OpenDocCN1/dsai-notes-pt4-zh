
# 从长期记忆的角度谈Agent

> 来源：[https://agijuejin.feishu.cn/docx/R3C2dOLcGogdlaxUljUcs9ewnpd](https://agijuejin.feishu.cn/docx/R3C2dOLcGogdlaxUljUcs9ewnpd)

> 作者：[孙鹏飞](https://www.zhihu.com/people/sun-peng-fei-45)

> 原文：https://zhuanlan.zhihu.com/p/682593028

## 为什么需要长期记忆？

在一个科幻故事中，有一个患有失忆症的角色，每天早上醒来对过去都没有任何回忆。为了克服这个问题，这个角色设计了一个聪明的技巧：在床上方的引导图片每天更新新的任务和结果。

类似地，LLM 由于受限于输入提示的大小，面临着同样的挑战。为了解决这个问题，一种解决方案是在其输出中重复 LLM 需要记住的信息，从而有效地创建一个暂存器，它可以在其中存储数据以供将来使用。

然而，创建这样一个提示便签本并不是一项简单的任务，可能面临如下问题：

+ 受限于 token 数量；
+ 缺少“Context”；
+ hallucination 的问题。

此外，基于LLM native 应用有几个重要元素：LLM + 交互 + 记忆（专有数据 + 个性化），故需要LLM的长期记忆能力，构建Autonomous Agents（自主智能体）和Agent Simulations（智能体模拟器）。

## Autonomous Agents

为了阅读的连续性，先简单地介绍一下Autonomous Agents以及流行的几个项目。如果对Autonomous Agents已经有认知了，可以直接本节进行阅读。

### 2.1 什么是Autonomous Agents？

在这里，采用Matt Schlicht的《The Complete Beginners Guide To Autonomous Agents](https://link.zhihu.com/?target=https%3A//www.mattprd.com/p/the-complete-beginners-guide-to-autonomous-agents)》这篇文章里面的定义。

Autonomous agents are programs, powered by AI, that when given an objective are able to create tasks for themselves, complete tasks, create new tasks, reprioritize their task list, complete the new top task, and loop until their objective is reached.

大概就是你给Autonomous Agents一个任务，比如发一个关于Autonomous Agents最新进展的twitter。他会先去理解分解这个任务目标，然后设定实施计划以及这几个计划的优先级，同时去辩证『冷静』的反思计划有没有漏洞，并将反思应用到执行过程中，然后就是自己不断的去换着关键词搜索总结最近的报道文章，然后是汇总、反思，看看有没有什么遗漏，最后组织成适合推文的语言自动发送。全过程人类的输入可能就只有最开始不到100字的一个命令。是不是想象空间很大？

这边用一张BabyAGI的流程图来直观展现这个过程，具体如下。

1. agent查看目标，使用 OpenAI 的 GPT-4 等LLM模型，使其能够理解正在阅读的内容，并提出第一个任务。“任务：在谷歌上搜索与 Twitter 相关的新闻”。
1. 然后agent在谷歌上搜索 Twitter 新闻，找到热门文章，并返回一个链接列表。第一个任务完成。
1. 现在agent回顾它的主要目标（找到关于 Twitter 的最新消息，然后发送摘要）和它刚刚完成的事情（得到一堆关于 Twitter 的新闻链接）并决定它的下一个任务需要是什么。
1. agent提出了两个新任务。1）写新闻摘要。2) 阅读通过谷歌找到的新闻链接的内容。
1. 现在agent在继续之前停止了一秒钟，它需要确保这些任务的顺序正确。真的应该先写摘要吗？不，它决定了最优先阅读通过google找到的新闻链接的内容。
1. agent从文章中读取内容，然后再次返回待办事项列表。它想添加一个新任务来总结内容，但该任务已经在待办事项列表中，所以它没有添加它。
1. agent检查待办事项列表，唯一剩下的项目是总结它阅读的内容，所以它这样做了。它会按照您的要求向您发送摘要。

![](img/128cf8bd624f121eef093b7cbf5bb044.png)

从上述的描述可以看出，Autonomous Agents核心技术点：LLM + Long-term Memory + 递归的思想。其中，LLM是其最核心的能力，无论推理还是问答以及后续的Prompt工程，都强依赖于LLM的能力。Long-term Memory是LLM固有的一个缺点，当然也是Autonomous Agents需要去克服的一个点，后续章节会详细介绍。

### 2.2 流行的Autonomous Agents

#### 2.2.1 Langchai

Langchain是Autonomous Agents的鼻祖，目标是辅助大家开发LLM应用，Agents、Tools、Plugin、Memory、Data Augmented早早地就提出开源了，为Autonomous Agents生态的爆发奠定了非常扎实的工作基础，值得给予最大的respect！

#### 2.2.2 Visual-ChatGPT

Visual-ChatGPT实际上是最简单的一种Autonomous Agents的项目，现已和微软另一个工作TaskMatrix.AI合并成为TaskMatrix，范围仅限于Visual Foundation Models，这种形态的项目，虽然能力边界小，但是LLM的负担是最轻的，每一个步骤以及流程是更加的可控，可能总共就只需要十几二十个流程。未来这种形态的工具或者产品对于各种垂直领域来说可能是更为常见的。

![](img/f38020479fcf55276810522920f3011c.png)

#### 2.2.3 HuggingGPT

微软联手HuggingFace构建HuggingGPT项目。它是微软一脉相承的工作，用LLM以及Prompt工程阶段性的解决多模态问题，核心思路是通过LLM的理解以及推理能力，分配相应的任务给对应的模型，比如：文生图的任务分配给Stable Diffusion模型、图生图分配给ControlNet、图文问答分配给Blip等等，不过只能算是初级的autonomous agents，还没有较为明显的continuous或者Autonomous的任务编排机制。

![](img/7466b5e309594cdba1043892a39e6745.png)

从上图可以看出，实际上HuggingGPT的agent的范围是huggingface所有模型。HuggingGPT将LLM as Controller，整个过程总结为4步：

+ 任务规划：利用ChatGPT分析用户的请求，了解用户的意图，通过提示分解成可能解决的任务。
+ 模型选择：为解决计划任务，ChatGPT 根据模型描述选择托管在Hugging Face 上的专家模型。
+ 任务执行：调用并执行每个选定的模型，并将结果返回给ChatGPT。
+ 生成结果：最后，使用ChatGPT整合所有模型的预测，为用户生成答案。

HuggingGPT的模式可控性也比较高，而且模型都是放在HuggingFace上进行托管的，输入输出的形式上也是一致的，用户使用以及代码维护都更为方便一些。

#### 2.2.4 AutoGPT

Auto-GPT 是一个实验性开源应用程序，展示了 GPT-4 语言模型的功能。该程序由 GPT-4 驱动，将 LLM 的“思想”链接在一起，以自主实现你设定的任何目标。作为 GPT-4 完全自主运行的首批示例之一，Auto-GPT 突破了 AI 的可能性界限。

![](img/017590d1cce6c5616489e267549da4ab.png)

从上图中可以看出，AutoGPT其实就是通过LLM反思input->plan->assign->collect->output整个过程，并且重新规划plan，从而产生一直迭代的auto效果。从外部的表现形式上来看，整个系统不断的交替进行plan和reflect，已经出现了自我思考自我迭代的过程。这实际上已经到了autonomous agents的范畴了。

#### 2.2.5 BabyAGI

BabyAGI 是一种将 OpenAI 最新的大型语言模型 GPT-4 转换为能够在互联网上执行一系列任务的数字助理的软件。与其他简单地根据提示生成单词的语言模型不同，BabyAGI 可以完成特定的操作，例如计划和执行活动以增加用户的 Twitter 关注或创建和运行内容营销业务。它是更广泛的开源软件 AutoGPT 的一部分。

如下图所示，通过执行以下步骤的无限循环来工作：

1. 从任务列表中提取第一个任务。
1. 将任务发送给执行智能体，执行智能体使用 OpenAI 的 API 根据上下文完成任务。
1. 丰富结果并将其存储在Vector DB（Chroma/Weaviate） 中。
1. 创建新任务并根据目标和先前任务的结果重新排列任务列表的优先级。

![](img/cec0d6033ee9beb7d464d2a857c38d28.png)

#### 2.2.6 NexusGPT

最近，开发者沿着这个思路做了一个NexusGPT项目，简单理解就是每个Agent实际上可以认为是一个人，每个Agent擅长不同的事情，那么一个庞大的组织架构可以通过雇佣擅长各种能力的Agent来维持组织的正常运转。

从各个方面来说，AI的发展需要或者会朝着AI平权(民主)、AI个性化的角度去发展，即让人人都可以享受AI的便利，人人都有一个自己的AI助理！而这个对应的就是Autonomous Agents。

未来可能每个人或自己制作极度个性化的Autonomous Agents，当然也可以选择『雇佣』一些『专业』的Autonomous Agents来临时的完成自己或者公司需要的某些任务。

NexusGPT——世界首个人工智能自由职业者平台！这里的自由职业者不是真人！其实背后都是一个个Autonomous Agents，不过他们在不同领域的数据集上进行了Finetune或者接入了不同的特殊能力的API，让他们成为了一个个充满个性的AI Agents！而这些Agents可以被雇佣成为你的助手！而你也可以将自己的AI助理派发到这个平台去进行『求职』！

![](img/5c04e7e583ad5bf7eec99b3da4127963.png)

#### 2.2.7 悟空

悟空是自主研发的Autonomous Agents。它除了具有LLM提供的基础语言能力，还具备长期记忆能力和接入第三方API能力以及基于文档的个性化数据理解能力。不过只能算是初级的autonomous agents，在之后的章节会详细介绍。

## Agent Simulations

### 3.1 什么是Agent Simulations

Agent Simulations（智能体模拟器）是通过计算机模拟来理解复杂的智能体交互的问题。它涉及多个智能体之间相互交互。智能体模拟器通常涉及两个主要部分：长期记忆和仿真环境。

### 3.2 流行的Agent Simulations

#### 3.2.1 CAMEL

CAMEL提出了一个名为 "角色扮演 "的新型交际智能体框架，它利用LLM的理解能力，促进智能体之间的自主合作，并提供对其 "认知 "过程进行洞察的能力。该方法包括使用概念提示来引导聊天智能体完成任务，同时保持与人类意图的一致性。并展示了角色扮演如何被用来产生对话数据，为研究对话语言模型提供了宝贵的资源。此外，它也为研究多智能体系统的合作行为和能力提供一个可扩展的方法。

![](img/1715ac6e8c8d18985c097a8dc4cde5a2.png)

#### 3.2.2 Generative Agents

Generative Agents是斯坦福和谷歌的研究者们利用人工智能创造出的生成式智能体。它是一种基于深度强化学习的方法，可以模拟人类在特定环境下的行为。该方法通过创建一个虚拟角色，并使用深度神经网络进行训练，使其能够模仿人类在某些任务上的决策过程。生成式智能体可以被看作是一种“虚拟人类”。生成式智能体的训练有两个阶段。在第一阶段中，生成式智能体会被赋予一个目标和一组任务，例如在游戏中获得最高分数或者完成特定任务。此时，生成式智能体不会立即开始执行任务，而是会观察并学习来自其他玩家的行为和策略，以便能够更好地适应任务要求。在第二阶段中，生成式智能体开始执行任务，并通过与环境互动来不断优化其行为。生成式智能体的决策是由一个深度神经网络产生的，这个神经网络接收环境信息并输出下一步的动作。在每次互动结束后，生成式智能体会根据任务的目标和奖励信号来更新其神经网络，以便更好地适应任务要求。

![](img/b0e9d5f0d22968aa8a30e1f9672b9f70.png)

## Autonomous Agents和Agent Simulations依赖LLM的能力

![](img/5bc2cf9be8bf78843a151c0824040c0d.png)

## 向量数据库

由于后续会大量涉及此概念，同时为了阅读的连续性，这里向大家推荐看一下《大模型引发爆发增长的向量数据库》文章。如果对向量数据库已经有认知了，可以直接跳过本节进行阅读。

## 悟空

悟空是自主研发的Autonomous Agents。它是使用了LLM来提供基础语言能力，VectorDB 来提供记忆能力，通过langchain等将其集成。它的整体流程图如下图所示。

![](img/a54fa54202f9c18c76e8978cba4d0529.png)

### 6.1 Retrieval Augmentation

悟空采用的记忆方案是 Retrieval Augmentation，它通过外部储存来增强模型记忆。当模型需要记忆大量的聊天记录甚至一整个行业知识库时，将其储存在向量数据库中，后续按需去获取最相关的信息。每次用户输入 prompt 时，从外部记忆中找到最相关的内容和 prompt 一起输入给模型，得到的生成内容会更有针对性且减少了 hallucination 的问题。

悟空的核心其实都是为了更好的管理记忆：短期记忆（Memory）和 长期记忆（History）。记忆的管理策略是按最近的短期记忆（Memory）和最相关的长期记忆（History）的方式来组织LLM的记忆，以达到Prompt Loop 在Token限制下也能按越来越趋近完成的方式来运转。

#### 6.1.1 短期记忆（Memory）

多轮对话的历史，我们常称为 短期记忆，它可以视为提示（prompt）的一部分。短期记忆可以帮我们塑造不同性格和能力的LLM。虽然大家使用的LLM的参数都是同样的，但不同的短期记忆能把 LLM「调教」成不同的性格，塑造成不同的工具。

![](img/7f47c033e0f7a28c9e2142a9e01ac477.png)

短期记忆确实可以帮助我们打造不同性格和能力的LLM，但是存在两个问题：

1. 每次与 LLM 的交互都会带上之前的对话内容（除非重新开启一次对话，此时 LLM 的短期记忆相当于被清空）。随着对话轮数的增加，所需的 token 数量会越来越多。因为输入的提示（即历史对话）也会占用 token 的消耗量，因为它们将进入神经网络参与计算。
1. 随着与LLM 的对话轮数增加，短期记忆的内容也会增加。然而，LLM 使用的神经网络结构的输入是有限的。目前，序列猴子最多只能包含 2048 个 token。

要解决短期记忆持续膨胀的问题，可以采取尽可能减少短期记忆 token 数量的方法。但是需要确保尽可能保留短期记忆的核心内容。悟空是按最近的Memory的方式来组织短期机制。具体是选择一个窗口，只保留最近 k 轮对话内容。

![](img/88c2900c1cae4757ba89affc7168306a.png)

注意这个策略基于如下假设：过于久远的对话不太重要，而距离当前时间更近的对话则更加重要。

#### 6.1.2 长期记忆（History）

长期记忆是短期记忆的相对概念。短期记忆的生命周期只存在于一次会话中，当会话完全结束后，短期记忆就不存在了。而长期记忆可以一直存储和访问。就像人类大脑会对一些重要信息进行长期记忆一样。

实现长期记忆最好的方法是将整个LLM进行 finetune，将您想要它记住和理解的内容调整到模型的权重中。然而，finetune 会带来两个挑战。一方面，finetune 对数据量有一定的要求，数据量过小难以获得好的效果。另一方面，finetune 对算力的要求非常高。

那么，除了 finetune，还有其他的方法来存储长期记忆吗？答案是有的。最简单的方法是将长期记忆存储在外部数据库中，当与LLM开始对话时，从数据库中提取长期记忆，并将其作为提示(prompt)输入到LLM中。这种方法理论上可以解决长期记忆的问题，本质上是通过上下文学习完全代替 finetune。
