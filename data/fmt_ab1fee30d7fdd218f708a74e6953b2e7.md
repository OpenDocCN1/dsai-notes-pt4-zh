
# AI Agent（或者LLM Agent）深度讲解——组成、方法、案例及展望

> 来源：[https://agijuejin.feishu.cn/docx/JHeYdkOzboVs81x3pb8ch1N8nkd](https://agijuejin.feishu.cn/docx/JHeYdkOzboVs81x3pb8ch1N8nkd)

> 作者：北方的郎

> 原文：[https://zhuanlan.zhihu.com/p/676544930](https://zhuanlan.zhihu.com/p/676544930)

前一段时间，比尔.盖茨在他的博客上发表了：《AI is about to completely change how you use computers》比尔·盖茨在这篇文章中探讨了AI Agent对我们未来生活的巨大影响。他谈到了AI Agent在医疗保健、教育、生产力、娱乐和购物等领域的作用。这些Agent将为人们提供更个性化的服务，帮助解决各种问题并提供支持，从辅助医生和教师工作到处理日常任务，甚至影响我们与朋友和家人的互动方式。AI Agent正在以各种方式迅速进入我们的生活，将在未来几年内彻底改变我们的生活方式。

## 1 什么是AI Agent (LLM Agent)

### 1.1 AI Agent 的定义

AI Agent是一种超越简单文本生成的人工智能系统。它使用大型语言模型（LLM）作为其核心计算引擎，使其能够进行对话、执行任务、推理并展现一定程度的自主性。简而言之，Agent是一个具有复杂推理能力、记忆和执行任务手段的系统。

![](img/48284c6e640360492f60129837e3825f.png)

### 1.2 AI Agent 的主要组成部分：

在LLM赋能的自主agent系统中(LLM Agent)，LLM充当agent大脑的角色，并与若干关键组件协作。

![](img/4b6fcb1548a1c0b935f0ea7cb33b4ab8.png)

#### 规划（planning）

+ 子目标分解：agent将大任务拆分为更小的可管理的子目标，使得可以有效处理复杂任务。

+ 反思与完善：agent对历史动作可以自我批评和自我反思，从错误中学习并在后续步骤里完善，从而改善最终结果的质量。

#### 记忆（Memory）

+ 短期记忆：上下文学习即是利用模型的短期记忆学习

+ 长期记忆：为agent提供保留和召回长期信息的能力，通常利用外部向量存储和检索实现

#### 工具使用（tool use）

+ 对模型权重丢失的信息，agent学习调用外部API获取额外信息，包括当前信息、代码执行能力、专有信息源的访问等等

#### 行动（Action）

+ 行动模块是智能体实际执行决定或响应的部分。面对不同的任务，智能体系统有一个完整的行动策略集，在决策时可以选择需要执行的行动，比如广为熟知的记忆检索、推理、学习、编程等。

## 2 AI Agent的意义

### 2.1 人机协同模式

基于大模型的Agent不仅可以让每个人都有增强能力的专属智能助理，还将改变人机协同的模式，带来更为广泛的人机融合。生成式AI的智能革命演化至今，从人机协同呈现了三种模式：

（1）嵌入（embedding）模式。用户通过与AI进行语言交流，使用提示词来设定目标，然后AI协助用户完成这些目标，比如普通用户向生成式AI输入提示词创作小说、音乐作品、3D内容等。在这种模式下，AI的作用相当于执行命令的工具，而人类担任决策者和指挥者的角色。

（2）副驾驶（Copilot）模式。在这种模式下，人类和AI更像是合作伙伴，共同参与到工作流程中，各自发挥作用。AI介入到工作流程中，从提供建议到协助完成流程的各个阶段。例如，在软件开发中，AI可以为程序员编写代码、检测错误或优化性能提供帮助。人类和AI在这个过程中共同工作，互补彼此的能力。AI更像是一个知识丰富的合作伙伴，而非单纯的工具。

实际上，2021年微软在GitHub首次引入了Copilot（副驾驶）的概念。GitHub Copilot是一个辅助开发人员编写代码的AI服务。2023年5月，微软在大模型的加持下，Copilot迎来全面升级，推出Dynamics 365 Copilot、Microsoft 365 Copilot和Power Platform Copilot等，并提出“Copilot是一种全新的工作方式”的理念。工作如此，生活也同样需要“Copilot”，“出门问问”创始人李志飞认为大模型的最好工作，是做人类的“Copilot”。

（3）智能体（Agent）模式。人类设定目标和提供必要的资源（例如计算能力），然后AI独立地承担大部分工作，最后人类监督进程以及评估最终结果。这种模式下，AI充分体现了智能体的互动性、自主性和适应性特征，接近于独立的行动者，而人类则更多地扮演监督者和评估者的角色。

![](img/d279f9a0725f7f1c7d48c102fbf4cc07.png)

从前文对智能体记忆、规划、行动和使用工具四个主要模块的功能分析来看，智能体模式相较于嵌入模式、副驾驶模式无疑更为高效，或将成为未来人机协同的主要模式。

基于Agent的人机协同模式，每个普通个体都有可能成为超级个体。超级个体是拥有自己的AI团队与自动化任务工作流，基于Agent与其他超级个体建立更为智能化与自动化的协作关系。现在业内不乏一人公司、超级个体的积极探索。

### 2.2 AI Agent与软件开发

AI Agent将使软件架构的范式从面向过程迁移到面向目标。现有的软件（包括APP）通过一系列预定义的指令、逻辑、规则和启发式算法将流程固定下来，以满足软件运行结果符合用户的预期，即用户按照指令逻辑一步一步操作达成目标。这样一种面向过程的软件架构具有高可靠性、确定性。但是，这种面向目标的架构只能应用于垂直领域，而无法普遍应用到所有领域，因此标准化和定制化之间如何平衡也成为SaaS行业面对的难题之一。

![](img/44bba6a9058f8f21d4954593e45b03d5.png)

AI Agent范式将原本由人类主导的功能开发，逐渐迁移为以AI为主要驱动力。以大模型为技术基础设施，Agent为核心产品形态，把传统软件预定义的指令、逻辑、规则和启发式算法的任务层级演变成目标导向的智能体自主生成。这样一来，原本的架构只能解决有限范围的任务，未来的架构则可以解决无限域的任务。未来的软件生态，不仅是最上层与所有人交互的媒介是Agent，整个产业的发展，无论是底层技术，商业模式，中间组件，甚至是人们的生活习惯和行为都会围绕Agent来改变，这就是Agent-Centric时代的开启。

![](img/ccadd7abae08e165623e9a9437bdffb3.png)

## 3 常见LLM Agent框架或者应用：

### 3.1 AutoGPT

可以说是LLM Agent的鼻祖了。Auto-GPT是一个实验性的开源应用程序，展示了GPT-4语言模型的能力。这个程序由GPT-4驱动，将LLM“思想”连接在一起，以自主地实现您设置的任何目标。作为GPT-4完全自主运行的最早示例之一，Auto-GPT突破了人工智能的极限，将AI进程推向了新高度 -- 自主人工智能。

[https://github.com/Significant-Gravitas/AutoGPT](https://link.zhihu.com/?target=https%3A//github.com/Significant-Gravitas/AutoGPT)

![](img/5531da52a7c64ed0f5eb9ff9c406cca2.png)

![](img/efdf9ccd8031d2079910437d5a8926a8.png)

![](img/65d9f5da42e73db189350d1c3470eb5b.png)

### 3.2 AutoGen

微软发布的AutoGen agent是可定制的、可对话的，并能以各种模式运行，这些模式采用 LLM、人类输入和工具的组合。使用 AutoGen，开发人员还可以灵活定义agent交互行为。自然语言和计算机代码都可用于为不同的应用编程灵活的对话模式。AutoGen 可作为一个通用框架，构建具有不同复杂性和 LLM 能力的各种应用。实证研究证明了该框架在许多样本应用中的有效性，应用领域包括数学、编码、问答、运筹学、在线决策、娱乐等。

[https://github.com/microsoft/autogen](https://link.zhihu.com/?target=https%3A//github.com/microsoft/autogen)

![](img/a362bef1e8d757d44743ddaaa8cdcb6c.png)

![](img/a3aa1ab76b1f30a557bf057f9a795762.png)

### 3.3 ChatDev

清华大学 NLP 实验室联合面壁智能等科研机构研发的一个大模型驱动的全流程自动化软件开发框架。ChatDev (Chat-powered Software Development)。ChatDev 拟作一个由多智能体协作运营的虚拟软件公司，在人类“用户”指定一个具体的任务需求后，不同角色的智能体将进行交互式协同，以生产一个完整软件（包括源代码、环境依赖说明书、用户手册等）。这一技术为软件开发自动化提供了新的可能性，支持快捷高效且经济实惠的软件制作，未来将有效地将部分人力从传统软件开发的繁重劳动中解放出来。

[GitHub - OpenBMB/ChatDev: Create Customized Software using Natural Language Idea (through LLM-powered Multi-Agent Collaboration)](https://link.zhihu.com/?target=https%3A//github.com/OpenBMB/ChatDev)

![](img/f9171d44b427d1e1d0a66c7410b19701.png)

![](img/f34f6a4055839e673e02fbd8df2a4703.png)

![](img/07f6829042e7f78864ecd5889bdbc05d.png)

### 3.4 XAgent

[GitHub - OpenBMB/XAgent: An Autonomous LLM Agent for Complex Task Solving](https://link.zhihu.com/?target=https%3A//github.com/OpenBMB/XAgent)

![](img/28e7ce463efd2b752d170c6c8a235c97.png)

XAgent是面壁智能联合清华大学 NLP 实验室共同研发开源的基于LLM的自主智能体，可以自动解决各种任务。它被设计为一个通用的智能体，可以应用于各种任务，终极目标是创建一个可以解决任何给定任务的超级智能体。换句话说，打造的是超级无敌的多边形战士，不是单独为某个任务设计的特定智能体。

![](img/820c13eac6966c1dc3c4ae616932e435.png)

### 3.5 GPT-engineer

[GitHub - gpt-engineer-org/gpt-engineer: Specify what you want it to build, the AI asks for clarification, and then builds it.](https://link.zhihu.com/?target=https%3A//github.com/gpt-engineer-org/gpt-engineer)

![](img/f1e22288abaa85562e997bf3d68071cf.png)

![](img/4b35492d2d4db8120e1770563b3f06b1.png)

### 3.6 HuggingGPT

HuggingGPT也是一个老牌的AI Agent项目了，主要思路为利用LLM的框架（例如。ChatGPT）来连接机器学习社区中的各种AI模型（例如huggingface）来解决人工智能任务。

网址：[HuggingGPT - a Hugging Face Space by microsoft](https://link.zhihu.com/?target=https%3A//huggingface.co/spaces/microsoft/HuggingGPT)

![](img/c4dff028aa3f67d71959eb293a654144.png)

代码：`https://github.com/`

![](img/e7acf25df7b077d3f41662db49fa47b4.png)

论文：[HuggingGPT: Solving AI Tasks with ChatGPT and its Friends in Hugging Face](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.17580)

![](img/b4e8860717b8cf931e4000c4b70d90ce.png)

## 4 AI Agent的展望与挑战

### 4.1 展望

AI Agent是人工智能成为基础设施的重要推动力。回顾技术发展史，技术的尽头是成为基础设施，比如电力成为像空气一样不易被人们察觉，但是又必不可少的基础设施，还如云计算等。当然这个要经历以下三个阶段：创新与发展阶段–新技术被发明并开始应用；普及与应用阶段–随着技术成熟，它开始被广泛应用于各个领域，对社会和经济产生深远影响；基础设施阶段–当技术变得普及到几乎无处不在，它就转变成了一种基础设施，已经成为人们日常生活中不可或缺的一部分。几乎所有的人都认同，人工智能会成为未来社会的基础设施。而智能体正在促使人工智能基础设施化。这不仅得益于低成本的Agent软件生产优势，而且因为Agent能够适应不同的任务和环境，并能够学习和优化其性能，使得它可以被应用于广泛的领域，进而成为各个行业和社会活动的基础支撑。

![](img/bc23f12c88d4e2fc8858f568be33fb79.png)

Agent下一步可能会朝着两个方向同时迭代。一是与人协助的智能体，通过执行各种任务来协助人类，侧重工具属性；二是拟人化方向的迭代，能够自主决策，具有长期记忆，具备一定的类人格特征，侧重于类人或超人属性。

### 4.2 挑战

从技术优化迭代和实现上来看，AI Agent的发展也面临一些瓶颈。

技术方面，LLM模型仍然不够强大，即使是最强大的GPT4在AI Agent应用时，仍然面临以下一些问题：

+ 上下文长度有限：上下文容量有限，限制了历史信息、详细说明、API 调用上下文和响应的包含。系统的设计必须适应这种有限的通信带宽，而从过去的错误中学习的自我反思等机制将从长或无限的上下文窗口中受益匪浅。尽管向量存储和检索可以提供对更大知识库的访问，但它们的表示能力不如充分关注那么强大。

+ 长期规划和任务分解的挑战：长期规划和有效探索解决方案空间仍然具有挑战性。LLM在遇到意外错误时很难调整计划，这使得它们与人类相比（从试错中学习）不太稳健。

+ 自然语言接口的可靠性：当前的Agent系统依赖自然语言作为LLM与外部组件（例如内存和工具）之间的接口。然而，模型输出的可靠性值得怀疑，因为LLM可能会出现格式错误，并且偶尔会表现出叛逆行为（例如拒绝遵循指示）。因此，大部分Agent演示代码都专注于解析模型输出。

其次，太烧钱了，尤其是多智能体。斯坦福的虚拟小镇一个Agent一天需要消耗20美金价格的token数，因为其需要记忆和行动的思考量非常大。这一价格是比很多人类工作者更高的，需要后续Agent框架和LLM推理侧的双重优化。

还有就是现阶段在很多场景，使用AI Agent对比Copilot模式的最终效果，还看不到非常大的提升，或者说能覆盖增加成本的提升。大部分AI Agent技术还都是研究阶段。

最后，这些发展趋势预示着AI Agent可能面临诸如安全性与隐私性、伦理与责任、经济和社会就业影响等多方面的挑战。别的不说，对很多人的个人职业生涯的长期影响。

![](img/d9ba0ab8fe2f58f4a42af8b0bd7676a3.png)

## 参考文献

[https://lilianweng.github.io/posts/2023-06-23-agent/](https://lilianweng.github.io/posts/2023-06-23-agent/)

`https://arxiv.org/abs/2312.17025`

[https://developer.nvidia.com/blog/introduction-to-llm-agents/](https://developer.nvidia.com/blog/introduction-to-llm-agents/)

[https://www.gatesnotes.com/AI-agents](https://link.zhihu.com/?target=https%3A//www.gatesnotes.com/AI-agents)

AI Agent，为什么是AIGC最后的杀手锏？

[https://github.com/e2b-dev/awesome-ai-agents](https://link.zhihu.com/?target=https%3A//github.com/e2b-dev/awesome-ai-agents)
