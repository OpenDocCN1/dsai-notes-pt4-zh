
# LLM（廿二）：LLM 时代的 multi-agent 系统

> 来源：[https://agijuejin.feishu.cn/docx/A7hHdS4tuom9Qlx0kb1ca6jVnUd](https://agijuejin.feishu.cn/docx/A7hHdS4tuom9Qlx0kb1ca6jVnUd)

> 作者：紫气东来

> 原文：[https://zhuanlan.zhihu.com/p/665644399](https://zhuanlan.zhihu.com/p/665644399)

在上一篇关于 RAG 的讨论中已经延伸出了 multi-agent 系统的概念，那么本篇就来填坑了

[紫](https://zhuanlan.zhihu.com/p/661465330)气东来：NLP（廿一）：从 RAG 到 Self-RAG —— LLM 的知识增强 695 赞同 · 29 评论 文章

![](img/a895b33fc1a60da6f909ea0042088dae.png)

事实上，在 LLM 的背景下，multi-agent 系统已经逐渐成为主流的应用方案。本文将试图从多个角度研究和讨论以 LLM 为基础的 multi-agent 系统的发展过程及算法特点。

## 一、前 LLM 时代的 multi-agent 系统

在 LLM 出现之前，multi-agent 主要存在于强化学习和博弈论(`game theory`) 的相关研究中。由于笔者之前从事强化学习相关研究，那么本节将主要介绍强化学习中的 multi-agent 系统。

multi-agent 系统相比于 single agent 更加复杂，因为每个 agent 在和环境交互的同时也在和其他 agent 进行直接或者间接的交互。因此，multi-agent 强化学习要比 single agent 的建模和优化更困难，其难点主要体现在以下几点：

+ 由于多个 agent 在环境中进行实时动态交互，并且每个 agent 在不断学习并更新自身策略，因此在每个 agent 的视角下，环境是 非稳态的（`non-stationary`），即对于一个 agent 而言，即使在相同的状态下采取相同的动作，得到的状态转移和奖励信号的分布可能在不断改变；

+ 多个 agent 的训练可能是多目标的，不同 agent 需要最大化自己的利益；

+ 训练评估的复杂度会增加，可能需要大规模分布式训练来提高效率，例如 `Ray` 框架。

![](img/a04fcb05717b29f1da1e41ec10c5b5e2.png)

### 1.1 multi-agent RL 问题建模

multi-agent RL 可以被定义为随机博弈问题，用元组 (N,S,A,R,P,γ) (N, \mathcal{S}, \mathcal{A}, \mathcal{R}, P, \gamma) 表示，其中：

+ N 是 agent 的数目

+ \mathcal{S}=S_1\times \cdot\cdot\cdot \times S_N 是所有 agent 的状态集合

+ \mathcal{A}=A_1\times \cdot\cdot\cdot \times A_N 是所有 agent 的动作集合

+ \mathcal{R}=r_1 \times \cdot\cdot\cdot \times r_N 是所有 agent 奖励函数的集合

+ P: \mathcal{S} \times \mathcal{A} \rightarrow \Omega(\mathcal{S}) 是环境的状态转移概率，其中 \Omega(\mathcal{S}) 是 \mathcal{S} 的分布集合

+ \gamma \in (0,1) 是衰减因子

一般 multi-agent 强化学习的目标是为每个 agent 学习一个策略来最大化其自身的累积奖励。

对于每一个 agent，其策略可表示为 \pi _i : \mathcal{S} \rightarrow \Omega(\mathcal{A}_i) ，其中 \Omega(\mathcal{A}_i) 表示 \mathcal{A}_i 上的概率分布

所有 agent 的联合策略可表示为 \boldsymbol{\pi}=\left[\pi_1, \pi_2, \ldots, \pi_N\right]

所有 agent 的联合动作可表示为 \boldsymbol{a}=\left[a_1, a_2, \ldots, a_N\right]

基于联合策略 \boldsymbol{\pi} ，agent i 在状态 s 下的价值为 V_i^\boldsymbol{\pi}(s)=\sum_{t=0}^{\infty} \gamma^t \mathbb{E}_{\boldsymbol{\pi}, p}\left[r_i^t \mid s_0=s\right] \\ 基于联合策略 \boldsymbol{\pi} ，agent i 在状态动作对 (s,\boldsymbol{a}) 下的价值为

Q_i^\boldsymbol{\pi}(s, \boldsymbol{a})=r_i(s, \boldsymbol{a})+\gamma \mathbb{E}_{s^{\prime} \sim p}\left[V_i^\boldsymbol{\pi}\left(s^{\prime}\right)\right] \\

### 1.2 multi-agent RL 求解范式

面对上述问题形式，最直接的想法是基于已经熟悉的 single agent 算法来进行学习，这主要有以下几种思路：

+ 完全中心化方法（`fully centralized`）

将多个 agent 进行决策当作一个超级 agent 在进行决策，即把所有 agent 的状态聚合在一起当作一个全局的超级状态，把所有 agent 的动作连起来作为一个联合动作。这样做的好处是，由于已经知道了所有 agent 的状态和动作，因此对这个超级 agent 来说，环境依旧是稳态的，一些单 agent 的算法的收敛性依旧可以得到保证。然而，这样的做法不能很好地扩展到 agent 数量很多或者环境很大的情况，因为这时候将所有的信息简单暴力地拼在一起会导致维度爆炸，训练复杂度巨幅提升的问题往往不可解决。

![](img/4ca04e340412191c50fcab1974b608db.png)

+ 完全去中心化方法（`fully decentralized`）

与完全中心化方法相反的范式便是假设每个 agent 都在自身的环境中独立地进行学习，不考虑其他 agent 的改变。完全去中心化方法直接对每个agent 用一个 single agent 强化学习算法来学习。这样做的缺点是环境是非稳态的，训练的收敛性不能得到保证，但是这种方法的好处在于随着 agent 数量的增加有比较好的扩展性，不会遇到维度灾难而导致训练不能进行下去。典型算法如 `IPPO` (`Independent PPO`)。

+ 中心化训练去中心化执行（`centralized training with decentralized execution`, `CTDE`）

在训练的时候使用一些单个 agent 看不到的全局信息而达到更好的训练效果，而在执行时不使用这些信息，每个agent 完全根据自己的策略直接行动，以达到去中心化执行的效果。该方法介于完全中心化方法和完全去中心化方法之间。典型算法如 `DDPG` (`muli-agent DDPG`)。

![](img/5fd9747e436986f56c3f892e52283060.png)

需要说明的是，在 LLM 出现之前的 agent 主要是规模不大的深度神经网络模型。下面将讨论以 LLM 为基础的 multi-agent 系统。

## 二、协作型的 multi-agent 系统

随着 LLM 的兴起，研究者们逐渐认识到 LLM 的优势及其局限性，那么综合多个不同功能的 LLM 的优点，共同实现一个目标的 multi-agent 系统便应运而生。这种协作式的 multi-agent 系统也是当前的主流方向，这种做法的主要优点有：

+ 专业知识增强：系统内的每个 agent 都拥有各自领域的专业知识，广泛的专业知识可以帮助生成的结果全面且准确。

+ 提高问题解决能力：解决复杂的问题通常需要采取多方面的方法。 LLM-based multi-agent 系统通过综合多个 agent 的优势，提供单个 LLM 难以匹敌的解决方案。

+ 稳健性和可靠性：冗余和可靠性是人工智能驱动的解决方案的关键因素。 LLM-based multi-agent 系统可降低单点故障的风险，确保持续运行并减少出现错误或不准确的可能性。

+ 适应性：在动态的世界中，适应性至关重要。 LLM-based multi-agent 系统可以随着时间的推移而发展，新的代理无缝集成以应对新出现的挑战。

### 2.1 协作机制

目前关于 multi-agent LLM 协作机制的讨论还比较少，本节的内容主要来源于 [Exploring Collaboration Mechanisms for LLM Agents: A Social Psychology View](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2310.02124.pdf)，这篇文章从社会心理学的角度提供了一个全新的分析视角。

该研究发现协作机制的主要有以下几点：

+ 每一个 agent 都有不同的个体特质、思维模式和合作策略；

+ agent 之间的辩论和 agent 本身的反思可以提高 agent 的表现；

+ agent 数量和策略的平衡是形成协作的关键因素

+ LLM agents 的协作机制和人类的社会心理学有诸多相似之处，如 从众和少数服从多数。

![](img/38deda8c8193a80f99c2e080f4a81528.png)

在系统构建上，研究者将多个具有不同特征的 agent 组成多样化的机器社会。 这些 agent 通过多轮相互辩论或自我反思来完成任务，辩论和反思的组合构成 agent 的策略。通过这种方式在一些测试数据上可以得到一些结论：

+ 多样的协作策略组合对结果具有重要的积极作用；

+ 思维模式的排序对于协作机制至关重要；

+ 持续的反思会增加不确定性（模型幻觉提高）

![](img/5ca8fadee14519715d7f86e2d77ccff5.png)

该研究揭示了 multi-agent 系统对人类社会协作模式研究的可行性和运行原理，[有利于对 multi-agent 系统](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2308.07201.pdf)的开发和应用。另外文中部分结论也可以从论文 `CHATEVAL` 中得到印证。

接下来将从应用的角度介绍部分相关案例。

### 2.2 对话系统

由于 `chatGPT` 的流行，基于此或模仿其的 multi-agent 案例都比较多，典型的有：

+ [`AutoGen`](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2308.08155)

+ [`ChatLLM`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2304.12998.pdf)

+ [`Agents`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2309.07870.pdf)

+ [`MetaGPT`](https://link.zhihu.com/?target=https%3A//github.com/geekan/MetaGPT%23metagpt-the-multi-agent-framework)

+ Multi-Party Chat

+ [`EduChat`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2308.02773.pdf)

下面以 `AutoGen` 为例进行讨论，`AutoGen` 可以看作一个框架，其允许多个 LLM agent 通过聊天来解决任务。LLM agent 可以扮演各种角色，如程序员、设计师，或者是各种角色的组合，对话过程就把任务解决了。不仅如此，`AutoGen` 是可定制的、可对话的，并且允许人类参与。`AutoGen` 的运作方式包括借助 LLM 完成任务、人类输入和各种工具的相互组合。

具体来说，`AutoGen` 中的 agent 具有以下显著特点：

+ 可对话：`AutoGen` 中的 agent 是可对话的，这意味着任何 agent 都可以从其他 agent 发送和接收消息以发起或继续对话

+ 可定制：`AutoGen` 中的 agent 可以定制以集成 `LLM`、人员、工具及其组合。

![](img/d8a180bc1892ecb84cb49dee027ad425.png)

首先有一个通用的 `ConversableAgent` 类，能够通过交换消息以共同完成任务。 不同的 agent 在收到消息后执行的操作可能有所不同。 两个代表性的子类是 `AssistantAgent` 和 `UserProxyAgent` 。

+ `AssistantAgent` 即 AI 助手。 它可以编写 `Python` 代码供用户在收到消息（通常是需要解决的任务的描述）时执行。它还可以接收执行结果并建议更正或错误修复。

+ `UserProxyAgent` 即人类的代理，默认情况下在每次交互时请求人类输入作为 agent 的回复，并且还具有执行代码和调用函数的能力。 当 `UserProxyAgent` 在接收到的消息中检测到可执行代码块并且未提供人工用户输入时，会自动触发代码执行。

可通过如下方式进行对象的创建：

```
from autogen import AssistantAgent, UserProxyAgent

# create an AssistantAgent instance named &quot;assistant&quot;
assistant = AssistantAgent(name=&quot;assistant&quot;)

# create a UserProxyAgent instance named &quot;user_proxy&quot;
user_proxy = UserProxyAgent(name=&quot;user_proxy&quot;)
```

二者之间的协调与交互过程如下图所示：

![](img/1dac5301f6025fe8cdfa27539014381c.png)

在此基础上，可以根据具体需要构建不同的应用场景，包括但不限于：

![](img/9e4b0da9680a663a7c3de4836725626d.png)

### 2.3 控制系统

当前基于 LLM 的 multi-agent 系统应用于控制系统(特别是机器人控制)的相关研究也逐渐兴起，比如：

+ [`RoCo`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2307.04738.pdf)

+ [`Co-LLM-Agents`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2307.02485.pdf)

+ [`RoboAgent`](https://link.zhihu.com/?target=https%3A//robopen.github.io/media/roboagent.pdf)

接下来将以 `Co-LLM-Agents` 为例介绍系统的构建方法及其工作原理。

系统整体框架由观察(`Observation`)、信念(`Belief`)、沟通(`Communication`)、推理(`Reasoning`)、规划(`Planning`)五个模块组成。 在每一步中，首先处理观察模块接收到的原始观察，然后通过信念模块更新 agent 对场景和其他智能体的内在信念，然后将之前的动作和对话一起使用来构建 `prompt` 传递给通信模块和推理模块，通信模块和推理模块利用大型语言模型来生成消息并决定高级计划。 最后，规划模块根据高级计划给出此步骤中要采取的原始操作。

在这个过程中，通信模块和推理模块是由 LLM 构成的，并承担不同的功能，具体来说：

+ 通信模块利用 LLM 的能力根据目标及历史信息生成 `action list`；

+ 推理模块则汇总所有信息，并生成高级的规划。

![](img/6c41603557d739f0763a1ac518ae6f94.png)

以下视频展示了 multi-agent 系统在 `ThreeDWorld` 中思考和交流以合作完成任务的过程。该方案证明了 LLM 可以超越强大的基于规划的方法，以自然语言进行交流，并赢得更多信任并与人类更有效地合作。 也为更加智能、复杂的 multi-agent 的研究提供了范例。

[🎦 点击观看视频](https://vdn6.vzuu.com/HD/a70e6aa0-8114-11ee-a325-8648fe191c08-v8_f2_t1_e5dvEAKp.mp4?pkey=AAV3X7LnC2AbH5CiE4pXF1T1eHFRmeEvPICSDU-MRY4Nnl_XfWDM6eDGcOEC5HrClfnZrDbYJKsH-lxOdPfKf-kH&bu=1513c7c2&c=avc.8.0&expiration=1724168380&f=mp4&pu=e59e796c&v=ks6&pp=ChMxNDAxNjIzODY1NzM5NTc5MzkyGGMiC2ZlZWRfY2hvaWNlMhMxMzY5MDA1NjA4NTk5OTA0MjU3PXu830Q%3D&pf=Web&pt=zhihu)



![](img/aa76aa6ac0b3924a65832d6d4111a646.png)

00:36

当然除了以上介绍的两个方面的研究外，其他领域的 multi-agent 系统协作的案例也在蓬勃发展，在此不一一介绍。如果读者发现更多的相关的研究，也欢迎推荐给我。

# 三、竞争型的 multi-agent 系统

## 3.1 竞争型的解释及其与协作型的比较

上节介绍的协作型的 multi-agent 系统大多遵循着一种范式：不同 agent 是系统的不同环节，承担不同的功能，共同为了系统的整体目标而服务。

而本节所讨论的竞争型的 multi-agent 系统则遵循另一种范式：每一个 agent 具有相对平等的地位，通过与不同个体间的信息交流和各自的活动，以实现各自不同的目标。这里所说的交流当然既包括竞争也包括协作，所以应该称作“交互型”更加合理，但是为了和之前的“协作型”相对，因此在此仍然称作“竞争型”。

下面列举二者之间主要的区别点：

|  | 协作型 | 竞争型 |
| --- | --- | --- |
| 系统目标 | 整体 | 个体 |
| 主流结构 | 中心化 | 去中心化 |
| agent 功能 | 相对分散 | 相对同质 |
| agent 关系 | 相互依赖 | 相互独立 |
| 是否自运行 | 否 | 是 |
| 系统资源 | 通常不共享 | 共享 |

![](img/02289185ad9a65352a02a9e01c422847.png)

下图是一个典型的竞争型的 multi-agent 系统，即：

+ 人类、LLM APIs、local LLMs 都是各自独立的 agent（即 Player）

+ 所有 agent 共享同一个环境和评价规则

+ 所有 agent 共享同一系统资源，并处于互相竞争和博弈中

![](img/1ea597432750f85ff70dd9c9bbee95a8.png)

之所以在 LLM 时代，以 LLM 为基础的 agent 能够作为 player，甚至和人类竞争，是因为 LLM-based agent 具有以下特征：

+ 反应性（Reactivity）：Agent的反应能力是指它对环境中的即时变化和刺激做出快速反应的能力。多模态融合技术可以扩展语言模型的感知空间，使其能够快速处理来自环境的视觉和听觉信息。这些进步使 LLMs 能够有效地与真实世界的物理环境互动，并在其中执行任务。

+ 主动性（Pro-activeness）：积极主动指的是，Agent不仅仅会对环境做出反应，它们还能积极主动地采取以目标为导向的行动。LLMs 具有很强的概括推理和规划能力，如逻辑推理和数学推理。同样也以目标重拟、任务分解和根据环境变化调整计划等形式显示了规划的新兴能力。

+ 社会能力（Social Ability）：社交能力指的是一个Agent通过某种Agent交流语言与其他Agent（包括人类）进行交互的能力。大型语言模型具有很强的自然语言交互能力，如理解和生成能力。这种能力使它们能够以可解释的方式与其他模型或人类进行交互，这构成了LLM-based Agent的社会能力的基石。

## 3.2 典型的竞争型的案例

这类案例主要在虚拟的世界观之中，如：角色扮演类的游戏、对人类世界的模拟等场景中。

如下图所示，从单个 agent 的视角来看，其运行机制：为确保有效交流，自然语言交互能力至关重要。agent 在接收感知模块处理的信息后，大脑模块首先转向存储，在知识中检索并从记忆中回忆。这些结果有助于 Agent 制定计划、进行推理和做出明智的决定。此外，大脑模块还能以摘要、矢量或其他数据结构的形式记忆 Agent 过去的观察、思考和行动。同时，它还可以更新常识和领域知识等知识，以备将来使用。LLM-based Agent还可以利用其固有的概括和迁移能力来适应陌生场景。

![](img/2ad3227ffe5c235686d445db47930a9d.png)

LLM-based Agent由个体和群体社会活动组成的复杂系统，在合作与竞争并存的环境中表现出了自发的社会行为。新出现的行为相互交织，形成了社会互动。其中的行为包括：

基础个体行为：

个体行为产生于内部认知过程和外部环境因素之间的相互作用。这些行为构成了个体在社会中运作和发展的基础。它们可分为三个核心维度：

+ 输入行为指的是从周围环境中吸收信息。这包括感知感官刺激并将其存储为记忆。这些行为为个体理解外部世界奠定了基础。

+ 内化行为涉及个体内部的认知处理。这类行为包括计划、推理、反思和知识沉淀等活动。这些内省过程对于成熟和自我完善至关重要。

+ 输出行为是外显的行动和表达。这些行为可以是物体操作，也可以是结构构建。通过执行这些动作，Agent 可以改变周围环境的状态。此外，Agent 还可以表达自己的观点和广播信息，与他人互动。通过这种方式，Agent 可以与他人交流思想和信念，从而影响环境中的信息流。

![](img/e55c057bac1508a83514071ad267093d.png)

动态群体行为：

群体本质上是由两个或两个以上的个体组成的，他们在一个确定的社会环境中参与共同的活动。群体的属性从来都不是一成不变的，相反，它们会随着成员的互动和环境的影响而不断演变。这种灵活性产生了许多群体行为，每种行为都对更大的社会群体产生独特的影响。群体行为的类别包括：

+ 积极的群体行为是促进团结、协作和集体福祉的行为。一个最好的例子就是团队合作，它可以通过头脑风暴讨论、有效对话和项目管理来实现。Agent共享见解、资源和专业知识。这鼓励了和谐的团队合作，使Agent能够利用自己的独特技能完成共同目标。利他主义贡献也值得一提。一些LLM-based Agent充当志愿者，自愿提供支持，帮助其他群体成员，促进合作与互助。

+ 中立的群体行为。在人类社会中，强烈的个人价值观千差万别，并趋向于个人主义和竞争性。相比之下，以“乐于助人、诚实和无害”为设计重点的 LLM 常常表现出中立的倾向。这种与中立价值观的一致会导致服从行为，包括模仿、旁观和不愿反对多数人。

+ 消极的群体行为会破坏Agent群体的有效性和一致性。Agent之间的激烈辩论或争执所产生的冲突和分歧可能会导致内部关系紧张。此外，最近的研究表明，Agent可能会表现出对抗行为，甚至采取破坏行为，例如为了追求自己的效率或目标而破坏其他Agent或环境。

![](img/a1535bf59f668dd848aab07cd9f2c5c0.png)

在人类社会的模拟的场景中，一个有意思的案例是 Humanoid Agents，其赋予了 agent 三个要素——基本需求（饱腹感、健康和能量）、情感和关系亲密程度，来让 agent 表现得更像人类。利用这些元素，agent 就能调整自己的日常活动，以及和其他 agent 的对话，从而进行类社会化活动。整个系统的工作原理分为以下几步：

+ 第1步：根据用户提供的种子信息初始化Agent。即对每个 Agent 进行人物设定，它们的名字、年龄、日程、喜好等，对其做出人物规划。

+ 第2步：Agent开始计划自己的一天。

+ 第3步：Agent根据自己的计划采取行动。如果同在一个地点，Agent可以相互交谈，进而影响他们之间的关系。

+ 第4步：Agent评估所采取的行动是否改变了他们的基本需求和情绪。

+ 第5步：根据基本需求和情感的满足情况，Agent可以更新未来的计划。

![](img/4b21bfadadcf4e8288c37c20289fd3f1.png)

其中 Agent 的配置文件如下所示：

```json
{
  "name": "fullness",
  "start_value": 5,
  "unsatisfied_adjective": "hungry",
  "action": "eating food",
  "decline_likelihood_per_time_step": 0.05,
  "help": "from 0 to 10, 0 is most hungry; increases or decreases by 1 at each time step based on activity"
}
```

[下面的视频](https://link.zhihu.com/?target=https%3A//www.humanoidagents.com/)展示了系统运行的过程，也可以通过 Demo 进行测试体验：

[🎦 点击观看视频](https://vdn.vzuu.com/HD/55b3b22c-812e-11ee-9624-8648fe191c08-v8_f2_t1_LvvtxkEp.mp4?auth_key=1724164780-0-0-2baaf970dab6085696bc47942c7c0932&bu=1513c7c2&c=avc.8.0&disable_local_cache=1&expiration=1724164780&f=mp4&pu=e59e796c&v=ali&pp=ChMxNDAxNjIzODY1NzM5NTc5MzkyGGMiC2ZlZWRfY2hvaWNlMhMxMzY5MDA1NjA4NTk5OTA0MjU3PXu830Q%3D&pf=Web&pt=zhihu)

![](img/1499d4b22603f79211e554c42833f525.png)

02:10

# 参考资料

[1] 多智能体强化学习入门 (boyuai.com)

[2] https://towardsdatascience.com/multi-agent-deep-reinforcement-learning-in-15-lines-of-code-using-pettingzoo-e0b963c0820b

[3] Cooperative Multiagent Attentional Communication for Large-Scale Task Space (https://www.researchgate.net/publication/358093908_Cooperative_Multiagent_Attentional_Communication_for_Large-Scale_Task_Space)

[4] https://arxiv.org/pdf/2205.15023.pdf

[5] https://arxiv.org/pdf/1706.02275v4.pdf

[6] GitHub - zjunlp/LLMAgentPapers: Must-read Papers on LLM Agents. (https://github.com/zjunlp/LLMAgentPapers)

[7] Interactive Language-Based Agents

[8] https://gafowler.medium.com/revolutionizing-ai-the-era-of-multi-agent-large-language-models-f70d497f3472

[9] MULTI-AGENT COLLABORATION: HARNESSING THE POWER OF INTELLIGENT LLM AGENTS (https://arxiv.org/pdf/2306.03314.pdf)

[10] Exploring Collaboration Mechanisms for LLM Agents: A Social Psychology View (https://arxiv.org/pdf/2310.02124.pdf)

[11] Multi-agent Conversation Framework | AutoGen (https://microsoft.github.io/autogen/docs/Use-Cases/agent_chat/)

[12] https://github.com/Farama-Foundation/chatarena

[13] The Rise and Potential of Large Language Model Based Agents: A Survey (https://arxiv.org/pdf/2309.07864.pdf)

[14] Generative Agents: Interactive Simulacra of Human Behavior (https://arxiv.org/pdf/2304.03442.pdf)

[15] Humanoid Agents: Platform for Simulating Human-like Generative Agents (https://arxiv.org/pdf/2310.05418.pdf)

[16] Role play with large language models (https://www.nature.com/articles/s41586-023-06647-8)

> 深秋绝塞谁相忆，木叶萧萧。乡路迢迢。 ——纳兰性德《采桑子·九日》
