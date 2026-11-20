
(我们的思路是把模型和 harness 当成一个整体来共同优化：一边反复使用模型，一边暴露 harness 中的问题，再去调整工具和 prompt，最后基于新的 harness 继续训练模型。)

# Harness 的很多能力，在快速被模型吸收

理解了“训练即部署”这件事，就能理解 harness 为什么是一个奇特的行业：它一边创造价值，一边把自己的能力源源不断地输送进模型本身。

现在模型公司都在把 “模型 + harness” 当成一个整体来优化。很多原本属于 harness 的能力，已经被模型逐渐内生化，比如 tool search、programmatic tool use、compaction / summarization、多步工具调用策略等。这件事通常是这么发生的：

- 模型团队和社区用户在前线摸索，试出哪些方法真的有效；
- 随后训练团队把这些模式拿去做 post-training；
- 在这一过程中，模型开始把这些能力慢慢内化，一部分原本必须靠 harness 强行维持的能力，开始变成模型自己的能力；
- 新的 harness 又被重新设计出来支持新的模型能力……
- 如此循环往复。

如今的很多 harness 能力，比如 planning、self-verification，也都在公开采访里被透露过，可能会成为未来后训练的候选方向。

Claude Code 负责人 Boris Cherny 也说过，Claude Code 的 harness 在不断被重写，里面每行代码的保质期可能也就 2 个月。

# Harness 即数据

Deepmind 的 Staff Engineer Philipp Schmid 有一个金句：“The Harness is the Dataset. Competitive advantage is now the trajectories your harness captures (Harness 本身就是数据集。现在真正的竞争优势，在于你的 harness 能捕获到怎样的执行轨迹)。”

现在真正有价值的数据，不再只是静态语料，还包括了 agent 在具体业务流程中跑出来的执行轨迹：它看到了什么信息，使用了什么工具，做了什么决策，哪一步容易出错，什么反馈会让它变得更好。

在这种情况下，harness 不再只是模型外面的脚手架，而是模型能力生成的土壤，土壤的质量，会一定程度上决定和反哺智能的生长。

以 Anthropic 为例，它在 harness 上的探索比 OpenAI 领先了一段时间，但就是这几个月的窗口期给了它巨大的机会。

现在市场的情况是：即使目前的模型水平两家已经基本打平了，但大多数人依然在用 Claude Code。

同时，模型和应用的竞争也呈现了一种很有意思的态势，头部的模型公司在端到端做 harness，而应用公司也越来越多开始自己训模型。

我们发现现在湾区已经有很多大的 AI 应用公司（远不只是 coding 方向）在搭建自己的 continuous learning 平台，基于自己的 harness 和业务数据，做开源模型的 RL，并且已经开始从头部 AI labs 迁移模型用量。

他们认为，在很多垂直场景里，今天的开源模型能力已经够用了。接下来的差距，未必来自谁有更好的模型，而是来自谁能围绕具体任务，把模型、任务结构、反馈闭环和后训练结合得更好，在特定场景里做出成本更低、效果更好的系统。

这种竞争格局，可能会成为接下来几年 AI 竞争里最有趣的看点之一。

# 05. 创业公司的机会

随着 harness 的重要性逐渐提升，我们相信这个领域也会有一些创业公司的机会，以下整理了一些比较有代表性的，势头非常好的项目，他们最近也都拿到了知名机构的大额融资。

## 信息层

今天企业应用 AI 的最大瓶颈之一，是 agent 没有充分的背景信息，所以怎么以 agent 为中心抓取出企业里原本分散的、隐性的知识，会是一个新的创业机会。我们尚不确定它最终能不能成长为一个大的独立赛道，但这些玩家起码是 2B 巨头们很好的收购标的。

### Edra

Edra 把自己定位为 “Context for Agents at Scale”，专注于把企业里分散的流程知识变成 agent 可执行的动态上下文，会自动读取企业现有的 tickets、logs、emails、chat histories 等数据，然后并行运行数千个智能体来探索这些数据、模拟决策并综合得出反向推理流程，还原公司真实的做事流程，再整理成一套 agent 可以复用的 playbooks。官方 slogan 是 “Edra learns how your business operates. Then automates it.”

今年 3 月，公司完成了约 3000 万美元 A 轮融资，Sequoia 领投。

## 执行层

### 方向 1：Workflow orchestration / durable execution

#### Temporal

Temporal 是做 Durable Execution 的底层 infra，简单讲就是让很长、很复杂、很容易失败的任务，依然能稳定跑完。中间哪怕服务器挂了、网络断了、任务等了三天，它也能记住执行到哪一步，然后从断点继续。

未来随着 Agent 需要完成越来越复杂、长时的任务，尤其是当未来成千上万个 Agent 要互相通信和交易时，对状态管理与失败恢复的要求会越来越高。这种分布式系统的运维非常复杂，而 Temporal 在这方面有深厚的技术积累和很强的先发优势，所以签下了 OpenAI、Replit、Netflix、Stripe、Datadog、Snapchat、JPMorgan 等很多大客户。

26 年 2 月，公司完成了 3 亿美元 D 轮融资，a16z 领投，估值 50 亿美元。

### 方向 2：Security / Governance

#### Oasis Security

Oasis 是面向 agentic enterprise 的权限管理平台。它想解决的问题是：现在企业里越来越多去访问系统、调用数据的，是 AI agent。但很多公司并不清楚这些 “数字身份” 到底有多少、分别在做什么、权限是不是开得太大、出了问题又该找谁负责。Oasis 就希望帮企业建立一套安全管理系统，让 agent 能拿到完成任务所必要的权限，但同时又不产生过多风险，并且让整个过程可以被追踪和控制。

26 年 3 月，完成 1.2 亿美元 B 轮融资，Craft Ventures 领投，Sequoia、Accel 等跟投，估值 7 亿美元。

### 方向 3：Sandbox

#### Daytona

Daytona 做的是 agent 的沙箱基础设施。它和 E2B 的区别在于，E2B 更像“给 agent 一个安全的执行盒子”，Daytona 更像“给 agent 一台可长期使用的电脑”。

至于为什么这个节点需要 Daytona 这类沙箱？核心是因为最近这一波产品，比如 Claude Code、Codex，把新的 agent 产品形态带火了，ai 开始需要真的像员工一样有一个 workspace 持续工作，就会催生一层专门的沙箱基础设施，这个环境不是一次性跑完就销毁，而是 stateful 的，能让 agent 展开更长周期、更复杂的工作流。

2026 年 2 月，公司完成 2400 万美元 A 轮融资，FirstMark 领投，Pace、Upfront、Datadog、Figma Ventures 等跟投。

## 反馈层

Evaluation & Observability 也是我们相对看好的一层，原因有三点：

1. 企业需要一层独立的质量控制面板，通过它看到真实调用链路、比较不同模型和版本。（虽然 OpenAI 和 Anthropic 的企业级平台也都在内置 eval、trace 的能力，且未来有一天也可能支持接入第三方模型，但对企业来说，底层模型和评测体系如果都出自同一家，始终会有顾虑，所以它相对不容易被模型厂吃掉）

2. 这是当前 AI 企业的刚需痛点，且未来随着 Agentic 层的复杂度变高，Agent 完成的任务变长，对 observability & evaluation 的要求也会越来越高、越来越精细。

3. 这一层通常会深度嵌入企业的 AI 工作流。它不只是一个看板，而是会进入数据采集、评分标准、版本发布和回归测试流程。接进去之后，替换成本往往不低。

### Braintrust

#### 产品

定位是一个给 AI 产品团队用的 AI observability / evaluation 平台。产品可以简单理解成三步：

(1) 看清过程。Braintrust 会把 AI 应用在生产环境中的请求记录成 traces，帮助团队看到一次调用里具体发生了什么，包括用户输入、模型输出、中间步骤、延迟，以及 token 和成本等信息，从而定位问题到底出在哪一层。

(2) 判断好坏。团队可以用人工标注、规则或 LLM scorer 对结果做评分，判断回答质量。Braintrust 同时支持离线实验和线上打分，让团队既能在发布前测，也能在真实流量里持续监控。

(3) 持续优化：把线上真实出现的问题沉淀成测试集，用来比较不同 prompt、模型等产生的不同效果，帮助团队方便地调试和打磨产品。

#### 融资

26 年 2 月，完成 8000 万美元 B 轮融资，ICONIQ 领投，a16z、Greylock、Elad Gil、Basecase 等跟投，估值 8 亿美元。

# 06. What's Next？

如果把 prompt engineering、context engineering 和 harness engineering 放在一起看，它们其实很像一个初级员工的成长过程。

最开始，你只能和他做简单问答；再往后，你可以把完整的业务背景交给他，让他独立完成一轮深入调研；再往后，你开始给他工具、权限和反馈机制，让他自己拆任务、调工具，甚至带着几个 subagents 一起干活。

那我们不禁在想，再下一个范式会是什么？

在人类现实职场中，大部分人都会有相对明确的 career path：从一名初级员工，逐渐积累经验，甚至升级为一个高管，从执行者变成规划者，带领几十个下属完成一个高度复杂的项目。

如果沿着人类员工的经验做一个自然推演，那么下一阶段 Agents 需要达到的，大概也是协调无数 agent / 人类节点共同完成复杂任务。

我们或许可以暂且叫做 —— Coordination Engineering。

OpenAI 的工程负责人和产品设计师在 1 月底的一期播客采访里，也多次提到了他们在思考 multi-agent networks。

“我不方便讲太多 roadmap，不过有一个挺值得思考的问题是：一旦进入 multi-agent 世界，复杂度会迅速上升，那你怎么才能管理这一切？用户怎么才能始终知道每个 agent 在做什么、执行了哪些动作、流程推进到哪一步，以及中间哪些地方需要自己介入、授权或纠偏？”

所以从这个角度看，下一代 AI 产品未必是一个更聪明的小龙虾，而更像一个小龙虾版飞书，本质是一个有效的监工看板 + 一个能让各种节点有效协作的 im 平台。

最终这四个层级叠加起来，可能就构成了 Agentic engineering 的终极范式。它们之间不是一个替代关系，而是一个包含关系，智能的水位涨上来，“控制面”就开始不断上移：

- L1 解决问答质量
- L2 解决认知边界
- L3 解决执行闭环
- L4 解决组织协同

![](img/295528962e4694716b4b43ba1bb97338.png)

再往终极推演呢？一切似乎就只剩下了 intention engineering，人的价值只剩下了 “设定目标函数”，其余 AI 都可自行包揽。

到那时候再回头看，所谓的白领工作，可能真的是人类历史走过的一段弯路。
