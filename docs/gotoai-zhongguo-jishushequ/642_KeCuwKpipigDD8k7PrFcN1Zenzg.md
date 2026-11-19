
# Dify大模型应用开发手册（2025）

> 来源：[https://gotoaiworld.feishu.cn/docx/HQLnd5ec8odlJDx5wM3cGiBMnTh](https://gotoaiworld.feishu.cn/docx/HQLnd5ec8odlJDx5wM3cGiBMnTh)

感谢 Leo

Dify大模型应用开发手册（2025）

`https://www.kdocs.cn/l/cfkmxv09hwbz`

---

## Dify介绍

### 为什么选择Dify？

在AI大模型的浪潮中，智能体（Agent）的概念越来越受到关注。那么，我们为什么要学习智能体呢？简单来说，大模型就好比拥有强大智能的大脑，但要让这个大脑真正为我们所用，解决实际问题，就需要一个高效的“执行者”，这个执行者就是智能体。智能体能够将复杂任务拆解，并协调各种工具和资源来完成目标。它让大模型不再是单一的“知识库”，而是能够自主思考、规划、执行的“问题解决者”。掌握智能体开发，意味着您将能够构建出更具自主性、适应性和复杂性的AI应用，让AI真正成为您的得力助手。

或许您会问，现在市面上已经有很多成熟的大模型产品，为什么我们还需要学习大模型开发呢？尽管有许多开箱即用的大模型，但要真正将AI技术与实际业务深度融合，实现差异化创新，就必须深入理解大模型的底层原理，掌握开发、部署和优化大模型的技能。这包括：

+   Dify工作流编排与智能体工程化开发能力：让您能够高效地搭建和管理AI应用，实现复杂的业务逻辑。

+   RAG（检索增强生成）性能调优与生产环境部署能力：让您的AI应用在获取知识和生成内容时更加精准、高效，并能在实际环境中稳定运行。

+   `API`的跨系统集成能力：打破系统壁垒，让AI能力无缝融入现有业务系统。

+   文本大模型、多模态大模型本地部署能力：让您能够根据自身需求，灵活部署和控制大模型，保护数据隐私，并针对特定场景进行优化。

**Dify**，这个名字本身就蕴含着“Define（定义）+ Modify（修改）”的理念，它是一款开源的大语言模型（LLM）应用开发平台。它将后端即服务（Backend as Service）和 LLMOps 的理念完美融合，旨在为您的团队提供从构思、开发到部署、监控的完整基础设施，帮助您打造能投产并创造真正价值的 Agentic AI 解决方案。

Dify 的目标非常明确：轻松构建并部署生产级 Agentic AI 解决方案。它提供了一站式能力：

+   Agentic 工作流：让AI不仅能“说”，还能“做”，自主完成复杂任务。

+   RAG Pipeline：高效整合您的私有知识。

+   丰富的集成：无缝连接各类大模型和工具。

+   可观测性：实时监控应用表现，确保稳定可靠。

**Dify 不仅仅是一个开发工具**，它更是生产就绪的企业级解决方案：

+   灵活扩展：从容应对不断增长的流量和变化的需求，确保您的AI应用具备可伸缩性。

+   稳定可靠：提供坚实可靠的基础设施，确保您拥有安心无忧的运营体验。

+   安全保障：具备企业级安全能力，守护您的关键数据资产，满足合规性要求。

在 `GitHub` 上，Dify 拥有超过 **103.5K 的 Star**，累计下载次数达 **500万**，并且汇聚了 **800多位开发者** 共同贡献（截止2025年6月）。Dify 也拥有不断壮大的生态环境。通过其插件市场（`Marketplace`），您可以在数秒内即时调用任意模型和工具，无需修改源代码。Dify 内置了构建LLM应用所需的关键技术栈，包括大模型的支持、直观的 `Prompt` 编排界面、高质量的 RAG 引擎、稳健的 Agent 框架、灵活的流程编排，并同时提供了一套易用的界面和 `API`。这为开发者节省了许多重复造轮子的时间，使其可以专注在创新和业务需求上。

在没有Dify这类平台之前，构建一个成熟的AI应用常常是耗时费力，并且随时都可能“踩坑”。

+   开发周期长，迭代慢：你需要花费大量时间评估不同LLM的优劣，进行反复对比才能做出选择。

+   技术门槛高，学习成本大：从零开始学习如何通过代码编排Prompts，如何准备结构化的数据集以接入LLMs，这些都需要深厚的专业知识。

+   部署和运维复杂：你需要自行调研、对比，甚至手动部署应用和向量数据库，这对于中小团队来说是巨大的负担。

+   合规与监管是空白：更不用说开发完成后的审查、监管和运营问题，这些在项目初期往往是空白，后期会成为隐患。

而Dify.AI，正是为解决这些企业级痛点而生的低成本私有化部署企业LLMs操作控制系统：

+   统一平台，全球领先 LLMs 一键接入：Dify提供一个统一的平台，能够无缝接入全球顶尖的开源和闭源LLMs，帮助企业简化模型选型和管理，告别技术迷茫。

+   底层分离的技术栈，轻松集成业务系统：Dify的架构设计使得底层技术栈与业务逻辑分离，这使得LLM能力可以轻松集成到企业现有的业务系统中，无需大规模重构，降低了集成难度。

+   私有化部署，数据安全和隐私得到保障：Dify支持私有化部署，确保企业数据在本地可控，数据安全和隐私得到根本保障，满足企业最核心的合规要求。

+   可视化Ops平台，持续运营：Dify提供可视化的Ops平台，能够实时监测成本和性能，并支持应用的持续运营、分析和优化，让企业对AI应用的投入和产出有清晰的认知。

+   全链路合规和风控解决方案：Dify内置了完善的全链路合规和风控解决方案，帮助企业有效规避潜在的法律和政策风险。

+   资深工程经验提供技术支持：Dify背后的团队拥有丰富的工程经验，能够为企业提供宝贵的技术咨询和支持，填补企业内部经验的不足。

### Dify与LLMOps

**DevOps**，顾名思义，是“开发”（Dev）与“运维”（Ops）的结合。它不仅仅是一套工具或流程，更是一种文化和实践，旨在通过自动化软件交付过程，来加强软件开发和信息技术运维团队之间的协作。让我们快速回顾一下DevOps的核心实践：

+   **开发（Dev）阶段**：

    +   从 `Code`（编码）开始，我们使用像 `Git` 这样的版本控制工具进行代码管理。

    +   进入 `Build`（构建）环节，我们使用 `Gradle` 等构建工具，将代码编译成可执行的软件。

    +   紧接着是 `Test`（测试），确保代码质量和功能正确性。

    +   在这个阶段，像 `GitLab` 这样的平台提供了一站式的代码管理和 `CI/CD` 功能。

+   **运维（Ops）阶段**：

    +   经过开发和测试，我们进入 `Release`（发布），将软件准备上线。

    +   然后是 `Deploy`（部署），将软件部署到云平台（如 AWS、Azure、OpenStack）或容器环境（如 `Docker`、`Kubernetes`）中。

    +   紧接着是 `Operate`（运行），确保系统稳定运行。

    +   最后是 `Monitor`（监控），使用 Grafana、Graylog 等工具监控系统性能和健康状况，收集反馈数据。

    +   监控数据又会反馈到 `Plan`（规划）阶段，指导下一轮的开发，形成一个无限循环。像 `Chef` 和 `Ansible` 这样的配置管理工具则在部署和运维中发挥作用。

在这个无限循环中，有两个关键的自动化实践：

+   **持续集成（Continuous Integration, CI）**：它强调自动化测试和构建过程，确保代码的集成是频繁且无误的。每次代码提交后都会自动触发构建和测试，尽早发现问题。

+   **持续交付（Continuous Delivery, CD）**：它将软件的部署过程自动化，使得软件可以快速且可靠地发布到生产环境。这意味着您的应用可以随时准备好发布，极大地缩短了从开发到用户手中的时间。

MLOps，全称 Machine Learning Operations，我们可以将其理解为 DevOps 在机器学习（ML）领域的扩展。它专注于机器学习模型的开发、部署、监控和维护，旨在通过结合 DevOps 的实践和机器学习的特点，确保模型的生命周期管理是高效和可靠的。整个 MLOps 是一个持续的、闭环的循环，确保模型能够不断地学习、迭代和优化，从而在真实世界中持续发挥价值。

+   ML Design（机器学习设计）阶段：

    +   这是项目的起始，我们要 **收集需求**，理解业务问题，并 **优先级排序 ML 用例**。

    +   重要的是 **业务理解**，明确我们用机器学习要解决什么实际问题。

    +   最后是 **数据获取**，为模型准备原始数据。

+   Model Development（模型开发）阶段：

    +   这一阶段包含了传统意义上机器学习工程师的核心工作：

    +   **数据准备和处理**：清洗、转换数据。

    +   **特征工程**：从原始数据中提取有意义的特征。

    +   **模型训练与实验**：选择算法，训练模型，并进行大量实验。

    +   **模型分析与评估**：评估模型的性能和效果。

+   Operations（运维）阶段：

    +   当模型开发完成并验证通过后，就进入了运维阶段：

    +   ML Model Deployment（机器学习模型部署）：将训练好的模型部署到生产环境。

    +   `CI/CD` Pipelines（持续集成/持续交付管道）：自动化模型的构建、测试和部署流程，确保快速、可靠上线。

    +   **Model Monitoring & Triggering（模型监控与触发）**：部署后并非一劳永逸。我们需要持续监控模型在生产环境中的性能，包括准确性、延迟、资源使用情况等，并根据监控结果或新数据自动化模型的再训练和更新过程。

在 **2015年**，传统的机器学习系统，除了 ML 代码本身，核心挑战在于数据收集、数据验证、特征提取、以及随后的模型训练、部署和监控。整个系统围绕着“如何从数据中训练出一个好模型”展开。

随着大模型开发的范式逐渐普及 “Feature Extraction”（特征提取）这类工作的重要性相对下降，取而代之的是 “Data retrieval and serving”（数据检索与服务），这直接指向了 RAG（检索增强生成）的重要性。我们不再是为每个任务从零开始提取特征训练模型，而是更多地利用大模型，并通过检索外部知识来增强其能力。

+   **优化方式的转变**：

    +   传统 MLOps 可能更侧重于 **模型训练和验证的自动化**。

    +   而 LLMOps 则更多地聚焦于 **提示词工程（`Prompt` Engineering）** 和检索增强生成（RAG） 的优化。对于绝大多数应用而言，我们无需微调大模型，更不需要从头构建领域大模型。这意味着 LLMOps 需要提供强大的提示词管理、RAG 管道构建和调试工具。

+   **数据角色的不同**：

    +   传统 MLOps 的数据主要用于 **模型训练**。

    +   LLMOps 的数据更多地作为 **上下文信息**，用于 **RAG 检索**，以提供个性化、领域化的回答，并需要特别关注数据的集成、结构化和命中率。

+   **模型特有挑战**：

    +   LLMs 特有的 “幻觉”（Hallucination）问题，以及模型对齐（Alignment）的挑战，需要在 LLMOps 中通过内容审核（Moderation System）、人类反馈和持续监控来解决。

    +   **`API` 成本管理**：由于大型模型的调用通常是按 `token` 计费的，LLMOps 需要精确的成本监控和优化策略。

    +   **复杂交互与 Agent 编排**：LLMOps 需要支持更复杂的 AI Agent 工作流编排，实现从意图识别到多步骤工具调用的完整链条。

[什么是 LLMOps](https://docs.dify.ai/zh-hans/learn-more/extended-reading/what-is-llmops)

### Dify vs Langchain

LangChain 是一个非常强大的开发框架，它为开发者提供了构建大语言模型应用的组件和工具链。而 Dify.AI，则是一个更偏向于平台型的产品，它融合了 Backend as a Service（后端即服务）和 LLMOps 的理念。

通过这些对比，我们可以清晰地看到：

+   LangChain 是一个为开发者提供 **高度灵活性和代码控制力** 的强大 **框架**，适合那些希望从底层开始构建、需要精细控制每个细节的团队。

+   Dify.AI 则是一个 **开箱即用、功能全面、且易于上手** 的 **平台**。它将 LLM 应用开发的复杂性进行高度封装和可视化，特别适合那些希望快速构建、部署和运营生产级 LLM 应用，并希望降低开发门槛，让更多非技术人员参与其中的个人、初创企业和中大型企业。

Dify 旨在帮助您加速 AI 应用的开发、部署与运营，特别适用于以下场景：

+   初创企业：将 AI 创意快速变为现实

+   **加速创新与验证**：无论您是希望验证一个 AI 应用的创意，还是需要快速构建一个最小可用产品（MVP）以获得投资，Dify 都能大幅缩短您的开发周期。

+   **快速市场验证**：在真实世界中，已有数十个团队通过 Dify 构建 MVP 获得了投资，或通过概念验证（POC）赢得了客户订单。Dify 帮助您高效验证市场需求，加速试错过程。

+   将 LLM 集成至已有业务：赋能现有应用

+   **能力增强与效率提升**：如果您希望通过引入大语言模型（LLM）来增强现有应用的能力，Dify 是理想选择。

+   **解耦与高效管理**：通过接入 Dify 提供的 RESTful `API`，您可以实现 `Prompt`（提示词）与业务代码的解耦。这使得业务逻辑与 AI 逻辑分离，维护更便捷。

+   **可视化监控与持续改进**：您可以在 Dify 的管理界面跟踪数据、成本和用量，并基于这些洞察持续改进应用效果。

+   作为企业级 LLM 基础设施：中心化监管与推广

+   **LLM 网关与推广**：Dify 可以部署为企业内部的 LLM 网关，加速生成式 AI（GenAI）技术在企业内的推广应用。

+   **集中管理与监管**：一些银行和大型互联网公司正在采用 Dify 来实现企业内 LLM 的中心化监管，确保合规性、数据安全和资源高效利用。

> 什么时候不适合使用 Dify？

尽管 Dify 功能强大，但在某些特定场景下，它可能不是最佳选择：

+   性能要求极高的场景:特别是对于项目部署后代码逻辑几乎不会改变的场景，且对极致的响应速度和性能有严格要求时，直接使用底层 `API` 或定制化开发可能会更具优势。Dify 作为一个平台会引入一定的抽象层。

+   非大模型驱动的项目:如果您的项目只需要简单的提示词，或者是一个不需要通用自动化能力、完全由特定业务逻辑驱动的非大模型项目，那么 Dify 的复杂性可能超过其带来的便利，此时直接编写少量代码可能更直接。

## Dify关键信息

+   官网：`https://dify.ai/zh`

+   [插件市场](https://marketplace.dify.ai/?language=zh-Hans)

+   [中文文档](https://docs.dify.ai/zh-hans/introduction)

+   官方博客：`https://dify.ai/blog`

+   开源地址：`https://github.com/langgenius/dify/`

## Dify版本差异

Dify 主要提供两大类服务：云服务（Cloud Service）和自托管（Self-Hosted），每种方案下又细分为不同的版本和套餐，以满足从个人开发者到大型企业的各种需求。

云服务（Cloud Service）- 便捷高效，即开即用。Dify 的云服务提供了多个层级，让您无需自行部署和维护基础设施，即可快速启动 AI 应用开发：

+   **SANDBOX (免费沙盒版)**：提供核心功能的免费试用，包含 200 条消息额度，支持多种 LLM，1 个团队工作区，5 个应用和 50 个知识文档等，非常适合个人开发者或初期探索。

+   `PROFESSIONAL` (专业版)：面向独立开发者和小型团队，每月 5000 条消息额度，支持更多知识文档和数据存储，并提供优先文档处理和日志记录等。

+   **TEAM (团队版)**：专为中型团队设计，提供更高的消息额度、知识文档和并发请求限制，是专业版的扩展。

> 云服务模式下，您无需部署，直接使用，数据安全依赖云服务提供商，适合中小规模使用。

自托管（Self-Hosted）- 自主可控，灵活定制，能够将 Dify 部署在自己的服务器环境中，获得最大的控制权和数据安全性：

+   **COMMUNITY (社区版)**：这是免费的开源版本，适用于个人、小型团队或非商业项目，提供所有公共仓库下的核心功能，并遵循 Dify 开源许可。您可以基于 `Docker Compose` 或本地源码进行部署。

+   **PREMIUM (高级版)**：为中型组织和团队设计，通过 AWS `Marketplace` 提供一键部署，强调自我管理的可靠性，支持自定义 `WebApp` Logo 和品牌化，并提供邮件和聊天支持。

+   `ENTERPRISE` (企业版)：专为需要组织级安全、合规性、可伸缩性、控制和高级功能的企业设计。需要定制化咨询和销售联系。

## Dify本地部署

Dify 社区版是一个开源版本，它允许您将 Dify 完全部署在自己的 **硬件或私有云** 环境中，从而获得最高级别的数据控制和定制自由。主要有两种部署方式：`Docker Compose` 部署和本地源码启动。

+   `Docker Compose` 部署

+   [**本地源码启动**](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/local-source-code) **（仍然需要 `Docker` 环境）**

安装 Dify 之前，请确保你的机器已满足最低安装要求：



# Dify部署与安装

## 系统要求

+ CPU >= 2 Core

+ RAM >= 4 GiB

## 部署建议

> 部署建议：推荐Linux环境下`Docker`部署，并且在部署过程中需要链接互联网+ `Github`网络环境。

https://kdocs.cn/l/coTTYPL1OPxC

[https://docs.dify.ai/zh-hans/learn-more/faq/install-faq](https://docs.dify.ai/zh-hans/learn-more/faq/install-faq)

## Dify环境变量

[https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/environments](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/environments)

## Dify模块与技术栈

Dify 依赖以下工具和库：

+ [`Docker`](https://www.docker.com/)

+ [`Docker Compose`](https://docs.docker.com/compose/install/)

+ `Node.js` v18.x (LTS)

+ [`npm`](https://www.npmjs.com/) version 8.x.x or [`Yarn`](https://yarnpkg.com/)

+ [`Python`](https://www.python.org/) version 3.11.x

`Dify` 的后端是整个平台的“大脑”，负责处理核心逻辑和数据流。

+ **编程语言与框架**：

+ `Python` + `Flask`：`Dify` 的后端服务主要使用 `Python` 语言开发，并基于轻量级的 `Flask` 框架构建了高效的 `RESTful` API 接口。这种组合使得开发快速而灵活。

+ `Celery`：为了处理数据导入、模型训练等耗时的异步任务，`Dify` 引入了 `Celery`，它是一个强大的分布式任务队列管理系统。这极大地提升了复杂工作流的执行效率和系统的响应速度。

+ **数据库与存储**：

+ `PostgreSQL`：作为核心关系型数据库，`PostgreSQL` 负责存储 `Dify` 的应用数据、用户信息以及各种结构化内容，确保数据的可靠性和一致性。

+ `Redis`：作为内存数据库，`Redis` 主要用于缓存和会话管理，有效优化了高频访问场景的性能，提升了用户体验。

+ `Weaviate`：这是 `Dify` 在 RAG（检索增强生成）管道中的关键组件。`Weaviate` 是一个向量搜索引擎，它能够高效地存储和检索海量的文本向量，从而为 LLM 提供精准的上下文信息，是 `Dify` 知识库能力的核心。

`Dify` 的前端是用户与平台交互的界面，旨在提供直观、流畅的体验。

+ **React + `ReactFlow`**：`Dify` 的前端界面主要基于流行的 `React` 库开发。其中，`ReactFlow` 是一个专门用于构建交互式节点图和流程图的库，`Dify` 利用它来实现了其引以为傲的可视化 AI 工作流编排，如 `Chatflow` 和 `Workflow` 的拖拽式设计界面。

---

# Dify使用指南

## Dify用户注册与登陆

当安装完成后，首先先前往管理员初始化页面设置设置管理员账户：

```
# 本地环境
http://localhost/install

# 服务器环境
http://your_server_ip/install
```

`Dify`管理员拥有系统最大的权限，需要铭记邮箱和密码，邮箱不会进行验证。

管理员可以通过输入邮箱（不存在的邮箱也可以），然后生成邀请链接。其他用户通过邀请链接进行注册。

[https://docs.dify.ai/zh-hans/guides/management/team-members-management](https://docs.dify.ai/zh-hans/guides/management/team-members-management)

## Dify接入大模型

`Dify` 是基于大语言模型的 AI 应用开发平台，初次使用时你需要先在 `Dify` 的 设置 — 模型供应商 页面内添加并配置所需要的模型。

[https://docs.dify.ai/zh-hans/guides/model-configuration/readme](https://docs.dify.ai/zh-hans/guides/model-configuration/readme)

[https://docs.dify.ai/zh-hans/development/models-integration/ollama](https://docs.dify.ai/zh-hans/development/models-integration/ollama)

[https://docs.dify.ai/zh-hans/learn-more/faq/llms-use-faq](https://docs.dify.ai/zh-hans/learn-more/faq/llms-use-faq)

`Dify` 目前已支持主流的模型供应商，例如 `OpenAI` 的 `GPT` 系列、`Anthropic` 的 `Claude` 系列等。不同模型的能力表现、参数类型会不一样，你可以根据不同情景的应用需求选择你喜欢的模型供应商。你在 `Dify` 应用以下模型能力前，应该前往不同的模型厂商官方网站获得他们的 `API key`。

> `Dify`默认是没有安装任何插件的，也没有支持任何供应商。因此需要自行安装，并且安装过程需要联网权限。

在 `Dify` 中，按模型的使用场景将模型分为以下 4 类：

1. **系统推理模型**。 在创建的应用中，用的是该类型的模型。智聊、对话名称生成、下一步问题建议用的也是推理模型。

2. **Embedding 模型**。在知识库中，将分段过的文档做 `Embedding` 用的是该类型的模型。在使用了知识库的应用中，将用户的提问做 `Embedding` 处理也是用的该类型的模型。

3. [`Rerank 模型`](https://docs.dify.ai/zh-hans/advanced/retrieval-augment/rerank)。`Rerank` 模型用于增强检索能力，改善 LLM 的搜索结果。

4. **语音转文字模型**。将对话型应用中，将语音转文字用的是该类型的模型。

## Dify应用类型

在 `Dify` 中，一个“应用”是指基于 `GPT` 等大语言模型构建的实际场景应用。通过创建应用，你可以将智能 AI 技术应用于特定的需求。它既包含了开发 AI 应用的工程范式，也包含了具体的交付物。

[https://docs.dify.ai/zh-hans/guides/application-orchestrate/readme](https://docs.dify.ai/zh-hans/guides/application-orchestrate/readme)

`Dify` 中提供了五种应用类型：

+ **聊天助手**：基于 `LLM` 构建对话式交互的助手

+ **文本生成应用**：面向文本生成类任务的助手，例如撰写故事、文本分类、翻译等

+ **Agent**：能够分解任务、推理思考、调用工具的对话式智能助手

+ **对话流**：适用于定义等复杂流程的多轮对话场景，具有记忆功能的应用编排方式

+ **工作流**：适用于自动化、批处理等单轮生成类任务的场景的应用编排方式

文本生成应用与聊天助手的区别见下表：

## Dify聊天和文本生成助手

对话型应用采用一问一答模式与用户持续对话。对话型应用可以用在客户服务、在线教育、医疗保健、金融服务等领域。这些应用可以帮助组织提高工作效率、减少人工成本和提供更好的用户体验。

[https://docs.dify.ai/zh-hans/guides/application-orchestrate/chatbot-application](https://docs.dify.ai/zh-hans/guides/application-orchestrate/chatbot-application)

## Dify Agent

智能助手（`Agent` Assistant），利用大语言模型的推理能力，能够自主对复杂的人类任务进行目标规划、任务拆解、工具调用、过程迭代，并在没有人类干预的情况下完成任务。

[https://docs.dify.ai/zh-hans/guides/application-orchestrate/agent](https://docs.dify.ai/zh-hans/guides/application-orchestrate/agent)

在“上下文”中，你可以添加智能助手可以用于查询的知识库工具，这将帮助它获取外部背景知识。在“工具”中，你可以添加需要使用的工具。工具可以扩展 LLM 的能力，比如联网搜索、科学计算或绘制图片，赋予并增强了 LLM 连接外部世界的能力。`Dify` 提供了两种工具类型：第一方工具和自定义工具。

## Dify工作流

工作流通过将复杂的任务分解成较小的步骤（节点）降低系统复杂度，减少了对提示词技术和模型推理能力的依赖，提高了 LLM 应用面向复杂任务的性能，提升了系统的可解释性、稳定性和容错性。

[https://docs.dify.ai/zh-hans/guides/workflow/readme](https://docs.dify.ai/zh-hans/guides/workflow/readme)

`Dify` 工作流分为两种类型：

+ **Chatflow（对话流）**：面向对话类情景，包括客户服务、语义搜索、以及其他需要在构建响应时进行多步逻辑的对话式应用程序。

+ **Workflow（工作流）**：面向自动化和批处理情景，适合高质量翻译、数据分析、内容生成、电子邮件自动化等应用程序。

### Chatflow

面向对话类情景，包括客户服务、语义搜索、以及其他需要在构建响应时进行多步逻辑的对话式应用程序。该类型应用的特点在于支持对生成的结果进行多轮对话交互，调整生成的结果。

**常见的交互路径**：给出指令 → 生成内容 → 就内容进行多次讨论 → 重新生成结果 → 结束

### 工作流（Workflow）

面向自动化和批处理情景，适合高质量翻译、数据分析、内容生成、电子邮件自动化等应用程序。该类型应用无法对生成的结果进行多轮对话交互。

常见的交互路径：给出指令 → 生成内容 → 结束

为解决自然语言输入中用户意图识别的复杂性，`Chatflow` 提供了问题理解类节点。相对于 `Workflow` 增加了 `Chatbot` 特性的支持，如：对话历史（`Memory`）、标注回复、`Answer` 节点等。为解决自动化和批处理情景中复杂业务逻辑，工作流提供了丰富的逻辑节点，如代码节点、`IF/ELSE` 节点、模板转换、迭代节点等，除此之外也将提供定时和事件触发的能力，方便构建自动化流程。

### 变量

[https://docs.dify.ai/zh-hans/guides/workflow/variables](https://docs.dify.ai/zh-hans/guides/workflow/variables)

### 节点

#### 开始

[https://docs.dify.ai/zh-hans/guides/workflow/node/start](https://docs.dify.ai/zh-hans/guides/workflow/node/start)

“开始”节点是每个工作流应用（`Chatflow` / `Workflow`）必备的预设节点，为后续[`工作流节点`](https://docs.dify.ai/zh-hans/guides/workflow/file-upload)以及应用的正常流转提供必要的初始信息，例如应用使用者所输入的内容、以及上传的文件等。在开始节点的设置页，你可以看到两部分设置，分别是 “输入字段” 和预设的系统变量。

输入字段功能由应用开发者设置，通常用于让应用使用者主动补全更多信息。例如在周报应用中要求使用者按照格式预先提供更多背景信息，如姓名、工作日期区间、工作详情等。这些前置信息将有助于 LLM 生成质量更高的答复。支持以下六种类型输入变量，所有变量均可设置为必填项：

+ **文本**：短文本，由应用使用者自行填写内容，最大长度 256 字符。

+ **段落**：长文本，允许应用使用者输入较长字符。

+ **下拉选项**：由应用开发者固定选项，应用使用者仅能选择预设选项，无法自行填写内容。

+ **数字**：仅允许用户输入数字。

+ **单文件**：[`允许应用`](https://docs.dify.ai/zh-hans/guides/workflow/file-upload)使用者单独上传文件，支持文档类型文件、图片、音频、视频和其它文件类型。支持通过本地上传文件或粘贴文件 `URL`。详细用法请参考文件上传。

+ **文件列表**：[`允许应用`](https://docs.dify.ai/zh-hans/guides/workflow/file-upload)使用者批量上传文件，支持文档类型文件、图片、音频、视频和其它文件类型。支持通过本地上传文件或粘贴文件 `URL`。详细用法请参考文件上传。

#### LLM

[https://docs.dify.ai/zh-hans/guides/workflow/node/llm](https://docs.dify.ai/zh-hans/guides/workflow/node/llm)

`LLM` 节点是 `Chatflow`/`Workflow` 的核心节点。该节点能够利用大语言模型的对话/生成/分类/处理等能力，根据给定的提示词处理广泛的任务类型，并能够在工作流的不同环节使用。

> 上下文变量是 `LLM` 节点内定义的特殊变量类型，用于在提示词内插入外部检索的文本内容。

+ **意图识别**，在客服对话情景中，对用户问题进行意图识别和分类，导向下游不同的流程。

+ **文本生成**，在文章生成情景中，作为内容生成的节点，根据主题、关键词生成符合的文本内容。

+ **内容分类**，在邮件批处理情景中，对邮件的类型进行自动化分类，如咨询/投诉/垃圾邮件。

+ **文本转换**，在文本翻译情景中，将用户提供的文本内容翻译成指定语言。

+ **代码生成**，在辅助编程情景中，根据用户的要求生成指定的业务代码，编写测试用例。

+ **RAG**，在知识库问答情景中，将检索到的相关知识和用户问题重新组织回复问题。

+ **图片理解**，使用 `vision` 能力的多模态模型，能对图像内的信息进行理解和问答。

#### 知识检索

[https://docs.dify.ai/zh-hans/guides/workflow/node/knowledge-retrieval](https://docs.dify.ai/zh-hans/guides/workflow/node/knowledge-retrieval)

从知识库中检索与用户问题相关的文本内容，可作为下游 `LLM` 节点的上下文来使用。

常见情景：构建基于外部数据/知识的 AI 问答系统（RAG）。

知识检索的输出变量 `result` 为从知识库中检索到的相关文本分段。其变量数据结构中包含了分段内容、标题、链接、图标、元数据信息。

#### 问题分类

[https://docs.dify.ai/zh-hans/guides/workflow/node/question-classifier](https://docs.dify.ai/zh-hans/guides/workflow/node/question-classifier)

通过定义分类描述，问题分类器能够根据用户输入，使用 LLM 推理与之相匹配的分类并输出分类结果，向下游节点提供更加精确的信息。

常见的使用情景包括客服对话意图分类、产品评价分类、邮件批量分类等。

#### 条件分支

[https://docs.dify.ai/zh-hans/guides/workflow/node/ifelse](https://docs.dify.ai/zh-hans/guides/workflow/node/ifelse)

根据 `If`/`else`/`elif` 条件将 `Chatflow` / `Workflow` 流程拆分成多个分支。条件分支的运行机制包含以下六个路径：

+ `IF` 条件：选择变量，设置条件和满足条件的值；

+ `IF` 条件判断为 `True`，执行 `IF` 路径；

+ `IF` 条件判断为 `False`，执行 `ELSE` 路径；

+ `ELIF` 条件判断为 `True`，执行 `ELIF` 路径；

+ `ELIF` 条件判断为 `False`，继续判断下一个 `ELIF` 路径或执行最后的 `ELSE` 路径；

支持设置以下条件类型：

+ 包含（`Contains`）

+ 不包含（`Not contains`）

+ 开始是（`Start with`）

+ 结束是（`End with`）

+ 是（`Is`）

+ 不是（`Is not`）

+ 为空（`Is empty`）

+ 不为空（`Is not empty`）

#### 代码执行

[https://docs.dify.ai/zh-hans/guides/workflow/node/code](https://docs.dify.ai/zh-hans/guides/workflow/node/code)

代码节点支持运行 `Python` / `NodeJS` 代码以在工作流程中执行数据转换。它可以简化你的工作流程，适用于`Arithmetic`、`JSON transform`、文本处理等情景。

+ `JSON`信息抽取



# Dify 工作流节点

## 代码执行器

+   数学计算
+   拼接数据

无论是 `Python3` 还是 `Javascript` 代码执行器，它们的执行环境都被严格隔离（沙箱化），以确保安全性。这意味着开发者不能使用那些消耗大量系统资源或可能引发安全问题的功能，例如直接访问文件系统、进行网络请求或执行操作系统级别的命令。这些限制保证了代码的安全执行，同时避免了对系统资源的过度消耗。

[沙箱说明](https://docs.dify.ai/zh-hans/development/backend/sandbox/README)

## 模板转换

[模板转换](https://docs.dify.ai/zh-hans/guides/workflow/node/template)

模板节点允许你借助 Jinja2 这一强大的 Python 模板语言，在工作流内实现轻量、灵活的数据转换，适用于文本处理、JSON 转换等情景。例如灵活地格式化并合并来自前面步骤的变量，创建出单一的文本输出。这非常适合于将多个数据源的信息汇总成一个特定格式，满足后续步骤的需求。

+   将多个输入（文章标题、介绍、内容）拼接为完整文本
+   整理成一个结构化的 Markdown 格式
+   渲染 HTML 的表单

## 文档提取器

[文档提取器](https://docs.dify.ai/zh-hans/guides/workflow/node/doc-extractor)

文档提取器节点可以理解为一个信息处理中心，通过识别并读取输入变量中的文件，提取信息后并转化为 `string` 类型输出变量，供下游节点调用。

文档提取器仅能够提取文档类型文件中的信息，例如 TXT、Markdown、PDF、HTML、DOCX 格式文件的内容，无法处理图片、音频、视频等格式文件。

## 列表操作

[列表操作](https://docs.dify.ai/zh-hans/guides/workflow/node/list-operator)

列表操作节点可以对文件的格式类型、文件名、大小等属性进行过滤与提取，将不同格式的文件传递给对应的处理节点，以实现对不同文件处理流的精确控制。

列表操作节点一般用于提取数组变量中的信息，通过设置条件将其转化为能够被下游节点所接受的变量类型。它的结构分为输入变量、过滤条件、排序（可选）、取前 N 项（可选）、输出变量。

## 变量聚合

[变量聚合](https://docs.dify.ai/zh-hans/guides/workflow/node/variable-aggregator)

变量聚合节点（原变量赋值节点）将多路分支的变量聚合为一个变量，以实现下游节点统一配置。

它负责整合不同分支的输出结果，确保无论哪个分支被执行，其结果都能通过一个统一的变量来引用和访问。这在多分支的情况下非常有用，可将不同分支下相同作用的变量映射为一个输出变量，避免下游节点重复定义。

变量聚合器支持聚合多种数据类型，包括字符串（`String`）、数字（`Number`）、文件（`File`）对象（`Object`）以及数组（`Array`）

变量聚合器只能聚合同一种数据类型的变量。若第一个添加至变量聚合节点内的变量数据格式为 `String`，后续连线时会自动过滤可添加变量为 `String` 类型。

## 变量赋值

[变量赋值](https://docs.dify.ai/zh-hans/guides/workflow/node/variable-assigner)

变量赋值节点用于向可写入变量进行变量赋值，已支持以下可写入变量：

+   [会话变量](https://docs.dify.ai/zh-hans/guides/workflow/variables#%E4%BC%9A%E8%AF%9D%E5%8F%98%E9%87%8F)
+   [循环变量](https://docs.dify.ai/zh-hans/guides/workflow/node/loop)

通过变量赋值节点，你可以将会话过程中的上下文、上传至对话框的文件（即将上线）、用户所输入的偏好信息等写入至会话变量，并在后续对话中引用已存储的信息导向不同的处理流程或者进行回复。

+   自动判断提取并存储对话中的信息
+   记录用户的初始偏好信息
+   辅助 `Checklist` 检查

## 迭代

[迭代](https://docs.dify.ai/zh-hans/guides/workflow/node/iteration)

对数组中的元素依次执行相同的操作步骤，直至输出所有结果，可以理解为任务批处理器。迭代节点通常配合数组变量使用。

使用迭代的条件是确保输入值已格式化为列表对象；迭代节点将依次处理迭代开始节点数组变量内的所有元素，每个元素遵循相同的处理步骤，每轮处理被称为一个迭代，最终输出处理结果。

迭代节点的结构通常包含输入变量、迭代工作流、输出变量三个功能单元。

+   **输入变量：** 仅接受 `Array` 数组变量类型数据。如果你不了解什么是数组变量，请阅读 [扩展阅读：数组](https://docs.dify.ai/zh-hans/learn-more/extended-reading/what-is-array-variable)。
+   **迭代工作流：** 你可以在迭代节点中使用多个工作流节点，编排不同的任务步骤。
+   **输出变量：** 仅支持输出数组变量 `Array[List]`。如果你想要输出其它变量格式。

## 参数提取

[参数提取](https://docs.dify.ai/zh-hans/guides/workflow/node/parameter-extractor)

Dify 工作流内提供了丰富的[工具](https://docs.dify.ai/v/zh-hans/guides/tools)选择，其中大多数工具的输入为结构化参数，参数提取器可以将用户的自然语言转换为工具可识别的参数，方便工具调用。

1.  从自然语言中提供工具所需的关键参数提取
2.  将文本转换为结构化数据
3.  提取结构化数据并使用 [HTTP 请求](https://docs.dify.ai/zh-hans/guides/workflow/node/http-request)

## HTTP 请求

[HTTP 请求](https://docs.dify.ai/zh-hans/guides/workflow/node/http-request)

允许通过 HTTP 协议发送服务器请求，适用于获取外部数据、webhook、生成图片、下载文件等情景。它让你能够向指定的网络地址发送定制化的 HTTP 请求，实现与各种外部服务的互联互通。

该节点支持常见的 HTTP 请求方法：

+   `GET`，用于请求服务器发送某个资源。
+   `POST`，用于向服务器提交数据，通常用于提交表单或上传文件。
+   `HEAD`，类似于 `GET` 请求，但服务器不返回请求的资源主体，只返回响应头。
+   `PATCH`，用于在请求-响应链上的每个节点获取传输路径。
+   `PUT`，用于向服务器上传资源，通常用于更新已存在的资源或创建新的资源。
+   `DELETE`，用于请求服务器删除指定的资源。

## Agent

[Agent](https://docs.dify.ai/zh-hans/guides/workflow/node/agent)

Agent 节点是 Dify Chatflow/Workflow 中用于实现自主工具调用的组件。它通过集成不同的 Agent 推理策略，使大语言模型能够在运行时动态选择并执行工具，从而实现多步推理。

从下拉菜单选择所需的 Agent 推理策略。Dify 内置了 `Function Calling` 和 `ReAct` 两种策略，可在 Marketplace → Agent 策略分类中安装使用。

### Function Calling

通过将用户指令映射到预定义函数或工具，LLM 先识别用户意图，再决定调用哪个函数并提取所需参数。它的核心是调用外部函数或工具，属于一种明确的工具调用机制。

优点：

+   精确：对于明确的任务，可以直接调用相应的工具，无需复杂的推理过程。
+   易于集成外部功能：可以将各种外部 API 或工具封装成函数供模型调用。
+   结构化输出：模型输出的是结构化的函数调用信息，方便下游节点处理。

### ReAct (Reason + Act)

ReAct 策略使 Agent 交替进行思考和行动：LLM 首先思考当前状态和目标，然后选择并调用合适的工具，工具的输出结果又将引导 LLM 进行下一步的思考和行动，如此循环，直到问题解决。

优点：

+   有效利用外部信息：能够有效地利用外部工具获取信息，解决仅靠模型自身无法完成的任务。
+   可解释性较好：思考和行动的过程是交织的，可以一定程度上追踪 Agent 的推理路径。
+   适用范围广：适用于需要外部知识或需要执行特定操作的场景，例如问答、信息检索、任务执行等。

## 工具

[工具](https://docs.dify.ai/zh-hans/guides/workflow/node/tools)

“工具”节点可以为工作流提供强大的第三方能力支持，分为以下三种类型：

+   内置工具，Dify 第一方提供的工具，使用该工具前可能需要先给工具进行 授权。
+   自定义工具，通过 `OpenAPI/Swagger` 标准格式导入或配置的工具。如果内置工具无法满足使用需求，你可以在 Dify 菜单导航 — 工具 内创建自定义工具。
+   工作流，你可以编排一个更复杂的工作流，并将其发布为工具。详细说明请参考[工具配置说明](https://docs.dify.ai/zh-hans/guides/tools/extensions/api-based/api-based-extension)。

## 结束

[结束](https://docs.dify.ai/zh-hans/guides/workflow/node/end)

定义一个工作流程结束的最终输出内容。每一个工作流在完整执行后都需要至少一个结束节点，用于输出完整执行的最终结果。

结束节点为流程终止节点，后面无法再添加其他节点，工作流应用中只有运行到结束节点才会输出执行结果。若流程中出现条件分叉，则需要定义多个结束节点。

结束节点需要声明一个或多个输出变量，声明时可以引用任意上游节点的输出变量。

> Chatflow 内不支持结束节点。

## 直接回复

[直接回复](https://docs.dify.ai/zh-hans/guides/workflow/node/answer)

自由定义回复格式，包括自定义一段固定的文本内容、使用前置步骤中的输出变量作为回复内容、或者将自定义文本与变量组合后回复。

可随时加入节点将内容流式输出至对话回复，支持所见即所得配置模式并支持图文混排，如：

1.  输出 LLM 节点回复内容
2.  输出生成图片
3.  输出纯文本

## 循环

[循环](https://docs.dify.ai/zh-hans/guides/workflow/node/loop)

循环（Loop）节点用于执行依赖前一轮结果的重复任务，直到满足退出条件或达到最大循环次数。

## 编排节点

[编排节点](https://docs.dify.ai/zh-hans/guides/workflow/orchestrate-node)

## 异常处理

[异常处理](https://docs.dify.ai/zh-hans/guides/workflow/structured-outputs)

---

# Dify 知识库

## 知识库介绍

[知识库介绍](https://docs.dify.ai/zh-hans/guides/knowledge-base/readme)

开发者可以将企业内部文档、FAQ、规范信息等内容上传至知识库进行结构化处理，供后续 LLM 查询。相比于 AI 大模型内置的静态预训练数据，知识库中的内容能够实时更新，确保 LLM 可以访问到最新的信息，避免因信息过时或遗漏而产生的问题。

+   LLM 接收到用户的问题后，将首先基于关键词在知识库内检索内容。知识库将根据关键词，召回相关度排名较高的内容区块，向 LLM 提供关键上下文以辅助其生成更加精准的回答。
+   开发者可以通过此方式确保 LLM 不仅仅依赖于训练数据中的知识，还能够处理来自实时文档和数据库的动态数据，从而提高回答的准确性和相关性。

在 Dify 中，知识库（Knowledge）是一系列文档（Documents）的集合，一个文档内可能包含多组内容分段（Chunks），知识库可以被整体集成至一个应用中作为检索上下文使用。文档可以由开发者或运营人员上传，或由其它数据源同步。

## 知识库管理

### 创建知识库

创建知识库并上传文档大致分为以下步骤：

+   创建知识库。通过上传本地文件、导入在线数据或创建一个空的知识库。
[创建知识库并上传文档](https://docs.dify.ai/zh-hans/guides/knowledge-base/create-knowledge-and-upload-documents/import-content-data/readme)

+   指定分段模式。该阶段是内容的预处理与数据结构化过程，长文本将会被划分为多个内容分段。你可以在此环节预览文本的分段效果。
[内容分段与清洗](https://docs.dify.ai/zh-hans/guides/knowledge-base/create-knowledge-and-upload-documents/chunking-and-cleaning-text)

+   设定索引方法和检索设置。知识库在接收到用户查询问题后，按照预设的检索方式在已有的文档内查找相关内容，提取出高度相关的信息片段供语言模型生成高质量答案。
[设定索引方法](https://docs.dify.ai/zh-hans/guides/knowledge-base/create-knowledge-and-upload-documents/setting-indexing-methods)

+   等待分段嵌入
+   完成上传，在应用内关联知识库并使用。

在 RAG 的生产级应用中，为了获得更好的数据召回效果，需要对多源数据进行预处理和清洗，即 ETL （extract, transform, load）。为了增强非结构化/半结构化数据的预处理能力，Dify 支持了可选的 ETL 方案：Dify ETL 和 Unstructured ETL。Unstructured 能够高效地提取并转换你的数据为干净的数据用于后续的步骤。Dify 各版本的 ETL 方案选择：

+   SaaS 版不可选，默认使用 Unstructured ETL；



# Dify 知识库

## 文件解析与 ETL

[社区版可选](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/environments)，默认使用 Dify ETL，可通过环境变量开启 Unstructured ETL。

### 文件解析支持格式的差异

不同的 [ETL](https://docs.unstructured.io/open-source/core-functionality/partitioning) 方案在文件提取效果的方面也会存在差异，想了解更多关于 Unstructured ETL 的数据处理方式，请参考官方文档。

## 管理知识库

可以在此处调整知识库名称、描述、可见权限、索引模式、Embedding 模型和检索设置。

[知识和文档维护介绍](https://docs.dify.ai/zh-hans/guides/knowledge-base/knowledge-and-documents-maintenance/introduction)

+ **知识库名称**，用于区分不同的知识库。
+ **知识库描述**，用于描述知识库内文档代表的信息。
+ **可见权限**，提供 “只有我” 、“所有团队成员” 和 “部分团队成员” 三种权限范围。不具有权限的人将无法访问该知识库。若选择将知识库公开至其它成员，则意味着其它成员同样具备该知识库的查看、编辑和删除权限。
+ **索引方法**
+ **Embedding 模型**，修改知识库的嵌入模型，修改 Embedding 模型将对知识库内的所有文档重新嵌入，原先的嵌入将会被删除。
+ **检索设置**

## 人工维护知识库

支持管理知识库内的文档和对应的文档分段。

[维护知识库文档](https://docs.dify.ai/zh-hans/guides/knowledge-base/knowledge-and-documents-maintenance/maintain-knowledge-documents)

## 使用 API 维护知识库

Dify 知识库提供整套标准 `API`，开发者通过 `API` 调用对知识库内的文档、分段进行增删改查等日常管理维护操作。

[通过 API 维护数据集](https://docs.dify.ai/zh-hans/guides/knowledge-base/knowledge-and-documents-maintenance/maintain-dataset-via-api)

## 元数据

[元数据](https://docs.dify.ai/zh-hans/guides/knowledge-base/metadata)

元数据是用于描述其他数据的信息。简单来说，它是“关于数据的数据”。它就像一本书的目录或标签，可以为你介绍数据的内容、来源和用途。通过提供数据的上下文，元数据能帮助你在知识库内快速查找和管理数据。

+ **字段（Field）**：元数据字段是用于描述文档特定属性的标识项，每个字段代表文档的某个特征或信息。例如“author”“language”等。
+ **字段值（Value）**：字段值是该字段的具体信息或属性，例如“Jack”“English”。
+ 字段值计数（Value Count）：字段值计数是指在某条元数据字段中标记的字段值数量，包括重复项。例如，此处的“3”是字段值计数，指该字段中有 3 个独特的字段值。
+ **值类型（Value Type）**：值类型指字段值的类型。
+ 目前，Dify 的元数据功能支持以下三种值类型：
+ **字符串**（`String`）：文本值。
+ **数字**（`Number`）：数值。
+ **时间**（`Time`）：日期和时间。

在知识库中，元数据字段分为两类：内置元数据（Built-in）和自定义元数据。

+ **元数据有什么作用？**
+ 提升搜索效率：用户可以根据元数据标签快速筛选和查找相关信息，节省时间并提高工作效率。
+ 增强数据安全性：通过元数据设置访问权限，确保只有授权用户能访问敏感信息，保障数据的安全性。
+ 优化数据管理能力：元数据帮助企业或组织有效分类和存储数据，提高数据的管理和检索能力，增强数据的可用性和一致性。
+ 支持自动化流程：元数据在文档管理、数据分析等场景中可以自动触发任务或操作，简化流程并提高整体效率。
+ 知识库元数据管理列表中的元数据字段和某篇文档中的元数据值有什么区别？
+ “在知识库管理界面删除某条元数据字段”、“在编辑元数据弹窗中删除已选文档的某条元数据字段”和“在文档详情界面删除某条元数据字段”有什么区别？

## 知识库使用

[在应用中集成知识库](https://docs.dify.ai/zh-hans/guides/knowledge-base/integrate-knowledge-within-application)

[Dify GitHub 仓库](https://github.com/langgenius/dify)

[文档路径](https://github.com/langgenius/dify-docs/tree/main/en/guides/knowledge-base)

+ 召回设置
+ 使用元数据筛选

## 知识库测试

[检索测试与引用](https://docs.dify.ai/zh-hans/guides/knowledge-base/retrieval-test-and-citation)

## 外部知识库

[连接外部知识库](https://docs.dify.ai/zh-hans/guides/knowledge-base/connect-external-knowledge-base)

[外部知识 API 文档](https://docs.dify.ai/zh-hans/guides/knowledge-base/api-documentation/external-knowledge-api-documentation)

对于内容检索有着更高要求的进阶开发者而言，Dify 平台内置的知识库功能和文本检索和召回机制存在限制，无法轻易变更文本召回结果。连接外部知识库功能可以将 Dify 平台与外部知识库建立连接。通过 `API` 服务，AI 应用能够获取更多信息来源。这意味着：

+ Dify 平台能够直接获取托管在云服务提供商知识库内的文本内容，开发者无需将内容重复搬运至 Dify 中的知识库；
+ Dify 平台能够直接获取自建知识库内经算法处理后的文本内容，开发者仅需关注自建知识库的信息检索机制，并不断优化与提升信息召回的准确度。

1. 外部知识库（向量数据库、全文检索库、`mysql`）
1. 外部检索 `api`
1. 在 Dify 创建一个虚拟的知识库

# Dify 工具/插件

[Dify 工具文档](https://docs.dify.ai/zh-hans/guides/tools/readme)

[插件介绍](https://docs.dify.ai/zh-hans/plugins/introduction)

**插件是 Dify 平台的扩展模块**，让你可以轻松地接入第三方模型与工具，显著提升应用能力。通过插件系统，你的 AI 应用能够更好地“看”、“听”、“说”、“画”、“计算”、“推理”，并连接外部 `API`，执行真实世界的操作。

1. 工具使用户可以在 Dify 上创建更强大的 AI 应用，如你可以为智能助理型应用（`Agent`）编排合适的工具，它可以通过任务推理、步骤拆解、调用工具完成复杂任务。
1. 方便将你的应用与其他系统或服务连接，与外部环境交互，如代码执行、对专属信息源的访问等。

## 自定义工具（HTTP调用）

你可以在 [“工具-自定义工具”](https://swagger.io/specification/) 内导入自定义的 `API` 工具，目前支持 `OpenAPI / Swagger` 和 `ChatGPT Plugin` 规范。你可以将 `OpenAPI` schema 内容直接粘贴或从 `URL` 内导入。关于 `OpenAPI / Swagger` 规范你可以查看官方文档说明。

[Dify 工具文档](https://docs.dify.ai/zh-hans/guides/tools/readme)

## 自定义插件

[Dify 插件开发入门](https://docs.dify.ai/plugin-dev-zh/0111-getting-started-dify-plugin)

# Dify 发布与监控

+ 发布为网页：[启动你的 Web 应用](https://docs.dify.ai/zh-hans/guides/application-publishing/launch-your-webapp-quickly/readme)
+ 嵌入网站：[在网站中嵌入](https://docs.dify.ai/zh-hans/guides/application-publishing/embedding-in-websites)
+ API 开发：[使用 API 开发](https://docs.dify.ai/zh-hans/guides/application-publishing/developing-with-apis)
+ 前端模板二次开发：[基于前端模板开发](https://docs.dify.ai/zh-hans/guides/application-publishing/based-on-frontend-templates)

# Dify 沙盒

[DifySandbox README](https://docs.dify.ai/zh-hans/development/backend/sandbox/README)

`DifySandbox` 是一个轻量、快速、安全的代码运行环境，支持多种编程语言，包括 `Python`、`Nodejs` 等，用户在 `Dify Workflow` 中使用到的如 `Code` 节点、`Template Transform` 节点、`LLM` 节点的 `Jinja2` 语法、`Tool` 节点的 `Code Interpreter` 等都基于 `DifySandbox` 运行，它确保了 `Dify` 可以运行用户代码的前提下整个系统的安全性。

+ **多语言支持**：`DifySandbox` 基于 `Seccomp`，这是一个系统层级的解决方案，从而确保了可以支持多种编程语言，目前支持了 `Python` 与 `Nodejs`。
+ **系统安全**：使用白名单策略，只允许运行特定的系统调用，从而确保不会出现意外的绕过。
+ **文件系统隔离**：用户代码将运行在一个独立的隔离的文件系统中。
+ **网络隔离**：
+ **`DockerCompose`**：独立网络 `Sandbox` 网络，并使用代理容器进行网络访问，确保内网系统的安全，同时提供了灵活的代理配置方案。
+ **`K8s`**：直接使用 `Egress` 配置网络隔离策略即可。

`DifySandbox` 已预先配置了以下 `Python` 依赖：

+ `http` - 基础网络请求库
+ `jinja2` - 模板引擎，用于 `LLM` 提示词处理
+ `requests` - `HTTP` 客户端库

若需要额外的 `Python` 依赖，请遵循以下步骤

1. 找到项目根目录下的 `dependencies/python-requirements.txt` 文件。
1. [在文件中](https://github.com/langgenius/dify-sandbox/blob/main/FAQ.md)添加所需的依赖包名及版本。若涉及权限变更，请参考常见问题。
1. 重新构建 `DifySandbox`。

添加新依赖可能引入额外权限需求，请谨慎评估安全影响。若未能完全掌控潜在安全风险，不建议更改权限配置。

[linux-syscall-table](https://filippo.io/linux-syscall-table/)

# Dify 二次开发

+ 自定义工具
+ 自定义知识库
+ 接入本地大模型

## Dify 开源许可证

[Dify LICENSE](https://github.com/langgenius/dify/blob/main/LICENSE)

`Dify` 采用 `Apache 许可证 2.0` 的修改版本授权，并附加以下条件：

+ `Dify` 可用于商业用途，包括作为其他应用程序的后端服务或企业应用程序开发平台。如需满足以下条件，则必须从提供商处获得商业许可：
+ 多租户服务：除非获得 `Dify` 的书面明确授权，否则您不得使用 `Dify` 源代码来运营多租户环境。
+ 租户定义：在 `Dify` 的环境中，一个租户对应一个工作区。工作区为每个租户的数据和配置提供独立的区域。
+ `LOGO` 和版权信息：在使用 `Dify` 前端的过程中，您不得删除或修改 `Dify` 控制台或应用程序中的 `LOGO` 或版权信息。此限制不适用于不涉及前端的 `Dify` 使用。
+ 前端定义：就本许可证而言，`Dify` 的“前端”包括从原始源代码运行 `Dify` 时位于 `web/` 目录中的所有组件，或使用 `Docker` 运行 `Dify` 时位于 `web` 镜像中的所有组件。
+ 作为贡献者，您同意：
+ 制作者可根据需要调整开源协议，使其更加严格或宽松。
+ 您贡献的代码可用于商业用途，包括但不限于其云业务运营。

除上述特定条件外，所有其他权利和限制均遵循 `Apache 许可证 2.0`。

[有关 Apache 许可证 2.0 的详细信息](http://www.apache.org/licenses/LICENSE-2.0)

## Dify 代码框架

`Dify` 的后端使用 `Python` 编写，使用 [Flask](https://flask.palletsprojects.com/en/3.0.x/) 框架。它使用 [SQLAlchemy](https://www.sqlalchemy.org/) 作为 `ORM`，使用 [Celery](https://docs.celeryq.dev/en/stable/getting-started/introduction.html) 作为任务队列。授权逻辑通过 `Flask-login` 进行处理。



## API 目录结构

```
[api/]
├── constants             // Constant settings used throughout code base.
├── controllers           // API route definitions and request handling logic.
├── core                  // Core application orchestration, model integrations, and tools.
├── docker                // Docker & containerization related configurations.
├── events                // Event handling and processing
├── extensions            // Extensions with 3rd party frameworks/platforms.
├── fields                //field definitions for serialization/marshalling.
├── libs                  // Reusable libraries and helpers.
├── migrations            // Scripts for database migration.
├── models                // Database models & schema definitions.
├── services              // Specifies business logic.
├── storage               // Private key storage.
├── tasks                 // Handling of async tasks and background jobs.
└── tests
```

## Web 目录结构

```
[web/]
├── app                   //layouts, pages, and components
│   ├── (commonLayout)    //common layout used throughout the app
│   ├── (shareLayout)     //layouts specifically shared across token-specific sessions
│   ├── activate          //activate page
│   ├── components        //shared by pages and layouts
│   ├── install           //install page
│   └── styles            //globally shared styles
├── assets                // Static assets
├── bin                   //scripts ran at build step
├── config                //adjustable settings and options
├── context               //shared contexts used by different portions of the app
├── dictionaries          // Language-specific translate files
├── docker                //container configurations
├── hooks                 // Reusable hooks
├── i18n                  // Internationalization configuration
├── models                //describes data models & shapes of API responses
├── public                //meta assets like favicon
├── service               //specifies shapes of API actions
├── test                  
├── types                 //descriptions of function params and return values
└── utils                 // Shared utility functions
```

## dify-plus

https://github.com/YFGaia/dify-plus

`Dify-Plus` 是 `Dify` 的企业级增强版，集成了基于 `gin-vue-admin` 的管理中心，并针对企业场景进行了功能优化。

> 严格遵循 Dify 原项目的版权许可协议，未涉及原项目许可的多租户功能及 logo 等版权信息。

1. 新增：用户额度
1. 对话余额限制判断
1. 异步计算用户额度逻辑
1. 左上角新增使用额度显示
1. 新增个人监测页
1. 新增：密钥额度设置
1. 新增应用 API 调用余额限制判断
1. 新增 ：Web 公开页登录鉴权
1. 新增：管理员同步应用到应用模版
1. 新增：后台创建用户，自动邀请进管理员空间
1. 新增：可以鉴权的 cookie
1. 新增：同步应用到模版中心
1. 新增：应用中心页面
1. 调整 ：默认跳转到应用中心
1. 新增：应用使用次数记录、应用中心按照使用次数排序
1. 权限调整
1. 调整：不允许普通成员关闭模型
1. 调整：空间普通成员不渲染“模型供应商”标签
1. 调整：非管理员，隐藏密钥显示
1. 优化： `csv` 编码监测，修复批量请求，windows 下载后保存再上传问题
1. 优化： `markdown` 图片放大问题优化
1. [新增：钉钉登录](https://github.com/YFGaia/dify-plus/wiki/Ding%E2%80%90Talk-%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3)
1. 新增：`sandbox-full`，以放开代码执行节点函数限制

### Docker 操作命令

`docker exec -it <容器名或容器ID> /bin/bash`

`docker commit <容器ID或名称> <新镜像名>:<标签>`

`docker export <容器ID或名称> > saved-container.tar`

### API 调用示例

```
curl -X POST 'http://47.76.254.232/v1/workflows/run' \
--header 'Authorization: Bearer key1' \
--header 'Content-Type: application/json' \
--data-raw '{
    "inputs": {},
    "response_mode": "streaming",
    "user": "abc-123"
}'

curl -X POST 'http://47.76.254.232/v1/workflows/run' \
--header 'Authorization: Bearer key2' \
--header 'Content-Type: application/json' \
--data-raw '{
    "inputs": {},
    "response_mode": "streaming",
    "user": "abc-123"
}'
```

---

## Dify 实践案例

[https://github.com/wwwzhouhui/dify-for-dsl](https://github.com/wwwzhouhui/dify-for-dsl)

https://github.com/BannyLon/DifyAIA

---
