# Agent Skills基础入门

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/RnGIdHBoooQi0axwsztci60inIg](https://kq4b3vgg5b.feishu.cn/docx/RnGIdHBoooQi0axwsztci60inIg)

## 一、Agent Skills：新一代 Agent 开发基本范式

### 1. Cowork+Skills “血洗华尔街”

#### 1.1 引言：当技术击穿商业护城河

在开始探讨 **Agent Skills** 的技术细节之前，我们需要先理解这项技术为何在面世之初就引发了巨大的震动。

2026年2月5**日，这一天在科技史和金融史**上被标记为 “SaaS 末日” (SaaS Apocalypse)。Anthropic 并没有发布更强参数的基础模型，而是发布了一款名为 Claude Cowork 的 Agent 产品，以及配套的 11 个标准化 Skills（技能包）。

![](img/7017ab28fcbf3c039254f5ec6e587bd3.png)

这次看似常规的功能**更新，直接导致纳**斯达克软件板块单日蒸发 2850 亿美元（约合人民币** 2 万亿元）。这一事件并非单纯的资本市场恐慌，它揭**示了一个深刻的技术范式转移：传统的“软件+图形界面”交付模式，正在被“Agent+Skills”的自动化交付模式所解构。

![](img/0ce7d03c9c09358441dd7c5467aad9af.png)

#### 1.2 核心事件复盘：11 个“硅基员工”的降临

事件的导火索是 Anthropic 发布的 11 个垂直领域 Skills。这些 Skills 并非简单的 API 接口，而是封装了完整业务逻辑（SOP）的独立单元。

+   **技术形态**：这些 Skills 覆盖了销售、财务、市场营销、法律等核心企业职能。用户只需在 Claude Cowork 中加载对应的 Skill，Agent 即可瞬间获得该领域的专家级处理能力。

+   **市场反应**：

+   **高盛软件股组合**：暴跌 6%，创下近年最大单日跌幅。

+   **垂直领域 SaaS 巨头**：法律科技公司 LegalZoom 暴跌 20%，信息服务商汤森路透暴跌 16%。

+   **恐慌逻辑**：投资者意识到，如果 Agent 可以通过加载 Skill 直接完成任务，企业将不再需要购买昂贵且复杂的 SaaS 软件订阅。

![](img/ac3f22852a1d521513b3a5006522195f.png)

在过去，一家企业为了处理合同审查和合规检查，通常需要采购如 Harvey AI 或汤森路透的专业软件。这不仅意味着每年数万美元的订阅成本，更意味着企业员工需要学习一套复杂的软件操作界面（UI），手动导入文档、配置规则、逐条复核。

Agent Skills 改变了这一切：

当 Claude 加载了 **"Legal Skill"** 后，它获得了一套标准化的法律工作流：

1.  **直接读取**：Agent 直接访问企业云盘中的合同文件。

1.  **动态调用**：根据 Skill 定义的 SOP，自动进行风险比对和条款审查。

1.  **结果交付**：直接输出合规报告，完全绕过了第三方软件的 UI 界面。

**技术洞察**：这一转变证明了 **“界面即阻碍”**。在 Agent 时代，用户不再需要一个“好用的工具”，用户需要的是“直接的结果”。Agent Skills 使得软件从“人机交互的界面”退化为“Agent 调用的后端能力”。

#### 1.3 范式总结：从 App-First 到 Skill-First

英伟达 CEO 黄仁勋在事后评论道：“认为 AI 会取代软件是不合逻辑的，SaaS 只是变成了基础设施。”这句话精准地点出了 Agent Skills 的本质。

**Agent Skills 并没有消**灭软件，它消灭的是软件的“独立入口权”。

+   **旧范式（App-First）**：开发独立的应用程序，设计复杂的 GUI，争夺用户注意力。

+   新范式（Skill-First）：开发标准化的 Skills，封装核心数据与逻辑，供 Agent 动态调用。

在接下来的课程中，我们将深入 OpenClaw 等开源项目，学习如何亲手构建这样的 Agent Skills，掌握这套能够“血洗华尔街”的技术架构。

### 2. OpenClaw：Agent Skills 最佳开源实践

#### 2.1 现象级崛起：从开源项目到“AI 操作系统”

**       如**果说 Anthropic 的 Claude Cowork 是 AI 时代的 iOS——精致、封闭且强大；那么 OpenClaw 无疑就是 AI 时代的 Android——开放、庞大且充满无限可能。

**       **截至 2026 年 2 月，OpenClaw 在 GitHub 上的 Star 数已逼近 200,000 大关。这是一个令所有开源项目都难以望其项背的数字。它不仅超越了当年的 AutoGPT 和 LangChain，更成为了全球开发者在构建 Agent 应用时的首选架构。

![](img/f67fb70d69da8680ee184f1ac25c4b25.png)

OpenClaw 之所以能引发如此大规模的“开发者迁徙”，核心原因在于它精准地复刻并超越了 Cowork 的模式：它不仅仅是一个聊天机器人，而是一个可动态加载无限能力的“技能容器”。

#### 2.2 核心架构：Everything is a Skill (万物皆技能)

**OpenClaw 的设计哲学非常激进：“Skill-Fi**rst”（技能优先），即核心只负责调度，一切能力皆由 Skills 动态挂载。

这种架构彻底颠覆了传统 Agent 的开发模式：

+   **传统 Agent**：将所有 Prompt、Tools 和 Python 脚本硬编码在主程序中，导致代码臃肿，难以维护，且 Context Window 极易溢出。

+   **OpenClaw Agent****：主程序是空的**。只有当用户输入“帮我分析这份财报”时，系统才会从 ClawHub（OpenClaw 的技能仓库）动态拉取 Financial_Analysis_Skill，加载到内存中执行。任务结束后，技能卸载，释放资源。

**这种“即插即用**”的架构，使得 OpenClaw 成为了事实上的 AI 应用入口。用户不再需要安装 100 个不同的 AI 应用，只需要在 OpenClaw 中挂载 100 个不同的 Skills。

#### 2.3 生态中心：ClawHub.ai

OpenClaw 的护城河并非代码本身，而是其背后庞大的技能生态平台——ClawHub (clawhub.ai)。这里汇聚了全球开发者贡献的超过 50,000 个标准化 Skills。无论你是需要一个 K8s 集群管理技能，还是需要一个📕热帖追踪，都能在 ClawHub 上一键获取。

![](img/44580660314cfbc0e3e6979f0b087377.png)

**ClawHub 的核心价值：**

1.  **标准化协议**：所有 Skill 都遵循统一的 skill.yaml 定义标准，确保在任何 OpenClaw 实例中都能 100% 兼容运行。

1.  **社区复用**：开发者不再需要重复造轮子。以前写一个“爬取网页并总结”的功能需要半天，现在只需要在 ClawHub 上下载对应技能即可。

#### 2.4 杀手级特性：自主进化与技能提炼

**让 OpenClaw 真正拉开与竞**争对手差距的，是其 "Autonomous Skill Refinement"（自主技能提炼） 机制。OpenClaw不仅能使用 Skill，还能创造 Skill。

+   例如，当你指挥 OpenClaw 完成了一项复杂的、从未有过定义的新任务（例如：“每天早上 8 点去爬取 arXiv 最新论文，翻译摘要，并发送到我的飞书群”）后，你就可以让OpenClaw将其固化为一项skill，然后OpenClaw 会自动将刚才的操作步骤、Prompt 策略、API 调用逻辑打包成一个标准的 Skill 文件。用户可以将这个 Skill 上传到 ClawHub 分享，或者私有化部署。

这种**“使用即开发”**的特性，极大地降低了 Agent 开发的门槛，也是它被誉为“最佳开源实践”的根本原因。

#### 2.5 行业落地案例 (Showcase)

在 openclaw.ai/showcase，我们可以看到 Agent Skills 技术是如何在真实世界中解决具体问题的：

![](img/2bee78d41fe840283369ff5603afca33.png)

![](img/350080e67ec40847989907f3dc8833e4.png)

![](img/4015c1c471fb098ccd151832452c3264.png)

![](img/273296540e0537d7c32d19a6a939471e.png)

### 3. Agent Skills 正在成为新一代 Agent 开发基本范式

#### 3.1 高度专业的垂域 Agent 开发还是基座 + Skills？

在 Agent 技术发展的早期（2023-2024年），业界普遍存在一种迷思：“要解决垂直领域的问题，必须构建垂直领域的 Agent。”

那时的开发者认为，要开发一个“法律顾问 Agent”，必须收集海量法律文书进行微调（Fine-tuning），构建复杂的专用 RAG（检索增强生成）系统，甚至重新设计模型架构。这种方式成本极高，周期极长，被称为**“重型开发模式”**。

**       然而，随着 Agent Skills 技术**的成熟，我们发现了一种全新的**“轻型解决思路”**：通用大模型（Base Model） + 特定技能包（Agent Skills） = 解决垂域问题

为什么这种看似“偷懒”的组合，反而成为了主流？这主要由两个核心因素驱动：

因素一：业务场景的“二八定律”与通用化需求

+   **现象**：在绝大多数商业场景中，用户并不需要一个“法学博士”级别的 AI。

+   例如：一家普通公司审核一份采购合同，或者初创企业草拟一份保密协议（NDA）。

+   **本质**：这些任务虽然属于“法律垂域”，但其所需的知识是标准化的、流程是通用的。

+   **结论**：对于这 80% 的通用化垂域场景，我们不需要训练一个昂贵的专用模型。只需要给一个足够聪明的通用 Agent 挂载一个包含法律 SOP（标准作业程序）**和**合规检查清单的 Skill，它就能以 90 分的水平完成任务。Skill 的本质，就是将专家的经验封装成大模型可读的“外挂”。

因素二：基座模型能力的“摩尔定律”

+   **现象**：2026 年的通用基座模型（如 GPT-5, Claude 4.5 级别），其基础逻辑推理能力和泛化能力，已经远超 2024 年经过微调的专用小模型。

+   **对比**：

+   2024 年专用法律 Agent：虽然经过微调，但底座智商有限，稍微遇到复杂的逻辑陷阱就会“幻觉”。

+   2026 年通用 Agent + Legal Skill：底座智商极高，理解力强，配合 Skill 提供的精确指令，其表现反而优于旧时代的专用模型。

+   **结论**：当基座足够强时，我们不再通过“改模型”来适应任务，而是通过“给技能”来适应任务。

**反思：垂域 Agent 开发已死？**但这是否意味着**我们完全不需要**开发垂域 Agent 了？答案是否定的。

**     **   **通用 Agen**t + Skills 只能解决 “标准化” 和 “中等复杂度” 的问题。在某些高度专业、逻辑深度极深的领域，垂域 Agent 依然是不可替代的王者。

+   典型案例：编程 Agent (Coding Agents)

+   尽管通用模型能写代码，但在构建大型系统、进行全库重构或解决复杂并发 Bug 时，通用 Agent 往往力不从心。

+   这也是为什么 **Claude Code**、Codex依然独立存在的原因。编程不仅需要 Skill，更需要对模型本身进行针对代码逻辑的强化训练（Code Specialization），甚至需要配合专门的编译器环境和沙箱系统。

+   **结论**：**“基座+Skills”横扫了 80% 的应用场景，而剩下的 20**% 硬骨头（如科研、高端医疗、核心系统编程），依然是垂域 Agent 的护城河。

#### 3.2 2026 大模型工程师必知必会：Agent Skills

而为了更快速高效地完成通用领域的 Agent 开发，大模型开发工程师需要进一步掌握如下三方面核心技能：

+   **学会上手使用 Skills**：理解 Agent Skills 的标准运行机制，能够在现有生态（如 Anthropic 或 OpenClaw）中快速检索、配置并调用成熟的技能包来解决实际业务问题。

+   **学会创建 Skills**：掌握如何将特定的业务 SOP、Prompt 策略、外部工具链（Tools）以及私有知识，打包封装成可复用、标准化的 Skill 文件。

+   学会搭建能够兼容 Skills 接入的 Agent：深入理解“核心极简”的解耦架构，能够从底层开发出支持动态挂载、卸载和智能调度 Skills 的 Agent 运行环境。

本期公开课，我们就沿着这个目标开始学习。

## 二、Agent Skills基础实现方法详解

### 1. Agent Skills装载与调用流程

**   **   在理解了 Agent Skills 的商业价值后，很多同学可能会觉得这背后的技术一定非常复杂。其实恰恰相反，在 OpenClaw（以及 Claude Code 等现代架构）中，“教会”AI 一项新技能，往往只需要一份 Markdown 文档。本小节我们将通过一个名为 FuFan-OpenClaw 的教学环境（基于 OpenClaw 深度定制），从零开始演示如何让一个原本“甚至不知道今天是几号”的 Agent，瞬间掌握实时天气查询的能力。

#### Step 1. 基准测试：裸机状态下的无助

首先，我们启动 FuFan-OpenClaw。在没有任何外部 Skills 加载的情况下，Agent 仅依靠大模型的预训练知识进行回答。

![](img/a05467ab83c288ed5821ec045523e135.png)

#### Step 2. 核心操作：物理装载 Skill

**接下来，**我们进行“技能注入”。操作非常原始且直观——文件拖拽。

1.  找到我们的目标技能文件夹 get_weather。

1.  将其直接拖入 FuFan-OpenClaw 后端的 /skills 目录下。

![](img/2fabda9a01e4e514050bac4866404572.png)

我们打开 get_weather 文件夹，发现里面**只有一个 ****SKILL.md**** 文件**。让我们看看它的内容（如下图）：

![](img/07c9cd783e63cde68f84a6e74a5c7945.png)

**这就是 Age**nt Skills 的核心秘密——用自然语言编程。这份文档其实就是写给 Agent 看的“SOP 操作手册”，它包含以下五个关键部分：

1.  **元数据 (Metadata)**：

+   顶部定义了 name: get_weather。这是技能的身份证，Agent 通过它在数千个技能中索引到由于。

1.  **使用场景 (Trigger)**：

+   明确告诉 Agent：“当用户询问某个**城市的**天气情况时使用此技能”。这是技能的触发器。

1.  **执行步骤 (SOP)**：

+   **这是最精彩的**部分。它没有写代码，而是教 Agent 如何使用工具：

+   第一步：从用户话里提取城市名。

+   第二步：**调用 ****fetch_url**** 工具**。注意，这里直接给出了具体的 URL 构造规则 https://wttr.in/{城市名}?format=j1。

+   第三步：教 Agent 如何看懂返回的 JSON 数据（提取温度、湿度、风速）。

1.  **示例 (Few-Shot)**：

+   给出了一个 查询北京天气 的标准对话范例。这能极大提升 Agent 执行的稳定性，防止它胡乱输出。

1.  注意事项 (Constraints)：

+   **比如“建**议使用英文拼写城市名以提高准确性”。这是给 Agent 的兜底策略。

![](img/455d210a63996604e24e0fa91516e2df.png)

#### Step 3. 验证测试：技能觉醒

文件放入后，无需重启服务器（Fufan-OpenClaw 支持热加载），我们再次输入同样的指令，Agent 最终回复北京天气信息。

![](img/851bf4a51ff5e92d3231c536ddc93c44.png)

#### 技术总结：为什么 Agent Skills 能引爆开发者生态？

通过刚才的演示，相信大家已经感受到了 Agent Skills 的核心魅力。与传统的软件开发相比，它展现出了极低的技术门槛和极高的灵活性。

**1. “自然语言编程” (Natural L**anguage Programming)

正如大家在 SKILL.md 中所见，我们要实现“天气查询”功能，并**没有**编写一行 Python 代码去定义 class WeatherService，也没有写复杂的 API 请求逻辑。我们只是用中文写了一段 SOP（操作流程）。

+   **本质**：Agent Skills 将“编程”降维成了“写文档”。只要你逻辑清晰，能把业务流程写成说明书，你就能开发 Agent。这使得非技术人员（产品经理、法务、运营）也能成为 Agent 开发者。

**2. 极简的“热插拔”架构**

在 OpenClaw 的架构中，Skill 是完全解耦的静态文件。这意味着：

+   **无需编译**：修改 Skill 不需要重启服务。

+   **无需环境配置**：只要有通用的工具（如 Web Search, Python REPL），Skill 就能运行，不需要为每个功能单独配置 Docker 或依赖库。

3. 对比 Function Calling 与 MCP：维度的胜利

很多专业开发者会问：“这和 OpenAI 的 Function Calling 或者 Anthropic 的 MCP (Model Context Protocol) 有什么区别？”我们可以通过一张对比表来看清它们的定位差异：

| **维度** | Function Calling (函数调用) | **MCP (模型上下文协议)** | Agent Skills (智能体技能) |
| --- | --- | --- | --- |
| **核心定义** | **代码级接口** | **通讯协议标准** | **业务逻辑封装 (SOP)** |
| **开发语言** | JSON Schema + Python/Node.js | TypeScript/Python SDK | **Markdown / 自然语言** |
| **实现难度** | **高** (需编写复杂的 Schema 定义和后端接手逻辑) | **中** (需搭建 Server 和配置 Client 连接) | **极低** (仅需编写文本文件) |
| **灵活性** | 僵化 (参数必须严格匹配) | 较强 (标准化连接) | **极强** (容错率高，大模型自动补全逻辑) |
| **适用人群** | 后端工程师 | 全栈工程师 | **所有人 (含非技术人员)** |

**结论**：

+   Function Calling 是底层的“零件”，它是给机器看的。

+   **MCP** 是连接的“管道”，它是给系统用的。

+   **Agent Skills** 则是**“说明书”**，它是给 AI 大脑看的。

**Agent Skill**s 之所以能大范围普及，正是因为它屏蔽了底层的零件和管道，让开发者可以直接通过“写说明书”来驱动 AI。这就是为什么我们说：在 Agent 时代，最好的编程语言就是你的母语。

### 2.完整的Agent Skills底层执行流程介绍

#### 2.2.1 深度思考：从“说明书”到“行动”的跨越

在上一节中，我们看到只需把文件夹一拖，Agent 就像《黑客帝国》里的 Neo 一样瞬间学会了“功夫”。但这看似简单的“魔法”背后，其实隐藏着一系列精密的工程设计。

这里我想请大家暂停一下，思考几个关乎系统生死的工程问题：

1.  上下文爆炸问题 (Context Window)： 如果我们有一万个 Skills，每个 Skill 的说明书都有 2KB，难道我们要把这 20MB 的文字全部塞进 System Prompt 吗？显然不行，Token 会瞬间溢出，费用会爆炸。那么 Agent 是怎么知道 get_weather 存在的？

1.  **知识与能力的鸿沟**： SKILL.md 里写着“去访问 wttr.in 网站”，但这只是一行文字。文字本身是没有执行力的。Agent 最终必须通过代码去发起 HTTP 请求。那么，它是如何把“文档里的文字指令”转化为“实际的代码执行”的？

要回答这些问题，我们不能只看聊天界面，必须打开 FuFan-OpenClaw 的**“后台原始消息队列” (Raw Message Logs)**，看看在那个 0.5 秒的瞬间，Agent 的大脑里到底发生了什么。

#### 2.2.2 流程解密：四步完成技能闭环

我们以刚才的“查询北京天气”为例，通过 FuFan-OpenClaw 的调试后台，还原这一次 Skill 调用的全过程。

![](img/2cdbcdadc71adbbae98902e7e8c5a99b.png)

**Phase 1: 意图识别与技能加载 (Tur**n 1 - Skill Loading)

当用户输入指令后，当前上下文（Context）中包含了 System Prompt（内含 Skill 简要索引）和 User Message。当我们把 get_weather 文件夹拖入系统时，OpenClaw 并没有立即读取 SKILL.md 的全文。为了节省上下文，它只读取了文件名和元数据。我们在系统日志中可以看到这样一条 System Message：

![](img/daae62a225959489fabe411856e178af.png)

**Phase 2: 技能注入与上下文增强 (Turn 2 **- Context Injection)

这是最关键的一步。OpenClaw 后端接收到 read_file 请求，读取磁盘上的 Markdown 文件，并将文件内容封装为标准的 Function Response Message，回填至消息队列中。

![](img/c69923384aa236402ba5c2564d5175f3.png)

**Phase 3: 基于 SOP 的工具执行 **(Turn 3 - Execution)

此时，消息堆栈中已经包含了完整的技能说明书。模型发起第二轮推理，严格遵循刚刚注入的 content 中的指示（SOP），构造具体的业务请求。模型并不是自己“编造”了 URL，而是根据 Phase 2 中注入的上下文（Markdown 中的步骤 2），进行了 In-Context Learning（上下文学习），完成了从自然语言指令到结构化参数的映射。

![](img/aaad2294c46693b401c76727dbe5000c.png)

Phase 4: 最终响应 (Final Response)

最后，fetch_url 的返回结果（JSON）再次以 role: tool 的形式进入消息队列。模型综合所有上下文，输出最终的自然语言回复。

![](img/06e643404106f1e7bb6437c0f7d44367.png)

### 3. Agent Skills 系统设计入门

通过剖析底层的 Function Calling **执行流，我们已经触摸到了 Agent Skill**s 的技术实质。从工程视角来看，Agent Skills 并非简单的文档堆砌，而是一种“高内聚、低耦合”的系统架构设计。

要构建一个企业级可用的 Agent Skills 系统，我们需要从系统工程的角度，重新审视 Agent（宿主环境）与 Skill（业务逻辑）之间的交互协议。以下是系统设计的五大核心要素：

#### 3.1 架构依赖：运行时 (Runtime) 与 业务逻辑 (Logic) 的解耦

首先，我们需要在架构层面明确 Agent 与 Skills 的依存关系。

+   **Skill 的本质**：它是**静态的业务逻辑定义**（Static Logic Definition）。无论是 Markdown 还是 YAML，它本质上是一组“未被执行的代码”或“待处理的 SOP”。

+   **Agent 的本质**：它是**动态的执行运行时**（Dynamic Execution Runtime）。它提供了推理引擎（LLM）、记忆模块（Memory）和工具接口（Interfaces）。

+   **系统设计难点**：**Skill 离开了 Agent 只是文本文件，无法产生价值；**而 Agent 离开了 Skill 只是一个通用的聊天机器人。系统设计的核心，在于构建一个能够标准化解析、加载并执行这些静态逻辑的“Agent Runtime”。

#### 3.2 核心机制：动态上下文加载 (Dynamic Context Loading)

在 Skill 系统设计中，最关键的技术挑战在于Token 效率与信息完备性的平衡。这本质上是一项精密的上下文工程 (Context Engineering)。

如果我们预置了海量的 Skills，直接全部注入 System Prompt 会导致两个致命问题：

1.  Context Window 溢出：Token 消耗指数级增长，成本不可控。

1.  Attention Dilution（注意力稀释）：过长的上下文会导致模型推理能力下降（Lost in the Middle 现象）。

因此，成熟的系统设计必须采用**“索引-按需加载” (Index-Lazy Loading)** 策略：

+   **索引层**：在 System Message 中仅保留极简的元数据索引（Name + Description）。

+   **加载机制**：利用 Function Calling (read_file) 作为触发器，实现 Skill 内容的 Just-in-Time (JIT) 注入。

+   **生命周期管理**：在任务结束后，需及时清理上下文，防止 Token 堆积。

#### 3.3 能力分层：编排层 (Orchestration) vs. 执行层 (Execution)

在定义 Skill 时，必须严格区分“指导”与“执行”的边界。

+   **Skill (编排层)**：负责**指导**。它定义了任务的 SOP（标准作业程序）、决策树和数据处理逻辑。但请注意，Skill 文档本身不具备操作系统的权限。

+   **Agent (执行层)**：负责**落地**。Agent 必须原生集成一系列原子工具 (Atomic Tools)，如 network_client (联网)、code_interpreter (代码执行)、database_connector (数据库连接)。

**设计原则****：Ski**ll 是对 Agent 原子能力的高阶编排。如果 Skill 定义了“查询天气”的步骤，Agent 的执行层必须预先挂载了 fetch 或 search 工具。若底层原子能力缺失，上层的编排逻辑将无法闭环。

#### 3.4 协议对齐：双向接口规范 (Interface Alignment)

基于上述分层，Agent Skills 系统的设计必须遵循**“双向协议对齐”**：

1.  **Agent 端规范**：

+   必须具备**工具反思能力**：能够识别何时需要查阅文档（调用 read_file）。

+   必须具备**标准化沙箱**：为 Skill 的执行提供安全的文件读写和网络访问环境。

1.  **Skills 端规范**：

+   **能力感知**：SOP 的编写必须基于 Agent 已有的工具集（Tools Schema）。

+   **格式统一**：必须遵循系统预设的解析标准（如 OpenClaw Standard Markdown），确保 Agent 能正确提取 Intent（意图）和 Steps（步骤）。

#### 3.5 开发者的价值：垂直领域的架构实现

既然 Anthropic 等巨头提出了 Agent Skills 范式，为什么没有一个“通用 Agent”能解决所有问题？

因为“泛化范式”无法替代“垂直架构”。

Anthropic 提供的是一套协议标准（Protocol），而具体的业务系统（Implementation）依然存在巨大的差异性：

+   **金融 Agent**：需要集成高频交易接口，对 Skill 的确定性和低延迟要求极高。

+   **编程 Agent**：需要集成编译器和解释器，其 Skill 侧重于代码生成的逻辑校验。

**这正是大模型工程师的核心价值**：**我们不是在使用一个现成的黑盒产品，而是基于 Ag**ent Skills 这一技术范式，针对特定的业务场景（如法律、医疗、研发），设计并实现一套包含“专用原子工具”、“动态加载策略”和“领域 SOP”的完整智能体系统。

---

## 正课介绍

+   体验课内容节选自《2026大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2026大模型Agent智能体开发实战》

![](img/4992e219f2f7a6dc09ecfc906183532b.png)

****《2026大模型Agent智能体开发实战**》为【100+小时】体系大课，总共20大模块精讲精析，零基础直达大模型企业级应用！

### 课程完整介绍

![](img/1e6da8bfa54bd4fc538f5884686e0cee.png)

**详细信息扫码添加助教，回复“大模**型”，即可领取课程大纲&查看课程详情👇

![](img/df760bef1b773e860fc24a4b4bee22a9.png)