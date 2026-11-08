# 07-使用 Dify 构建企业级智能应用解决方案

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/Qpzqds9Zto7JGPxBne2cg95Bnzg](https://zsyhjtnsa5.feishu.cn/docx/Qpzqds9Zto7JGPxBne2cg95Bnzg)

# **第一章：Dify 平台导论 （Introduction t**o the Dify Platform）

## **1.1 学习目标**

在本章学习结束后，您应能够：

+   阐述企业在开发和部署生成式 AI 应用时面临的主要挑战。

+   定义 Dify 平台及其作为 LLMOps （大语言模型应用运营） 平台的核心定位。

+   解释 Dify 平台的核心理念，包括“后端即服务 （BaaS）”和“提示词即服务 （PaaS）”在 AI 应用开发中的体现。

+   识别并描述 Dify 平台的主要功能特性，如可视化编排、多模型支持、RAG 引擎和 Agent 框架。

+   分析 Dify 平台为企业带来的核心价值，特别是在开发效率、数据安全和集成性方面。

+   概述 Dify 平台的系统架构，识别其关键组件及其相互关系。

+   掌握本章介绍的关键术语。

## 1.2 企业 AI 应用开发的挑战与机遇

近年来，以大语言模型（Large Language Models， LLMs）为代表的生成式人工智能（Generative AI）技术取得了突破性进展，为各行各业带来了前所未有的机遇。企业期望利用这项技术优化流程、提升客户体验、辅助决策制定，甚至创造新的商业模式。

然而，将强大的 LLM 能力转化为稳定、可靠且符合企业需求的实际应用，并非易事。企业在实践中普遍面临以下挑战：

1.  **技术复杂性高：** 有效利用 LLM 需要掌握 Prompt 工程（如何设计有效的指令）、上下文管理、模型 API 调用、向量数据库（用于处理私有知识）等一系列专业知识，技术门槛较高。

1.  **开发周期长：** 构建一个完整的 AI 应用涉及前端交互设计、后端逻辑实现、模型集成、数据处理、安全加固、API 封装等多个环节，传统开发模式下周期较长，难以快速响应业务需求。

1.  **运维成本高：** LLM 技术仍在快速迭代，需要持续优化 Prompt、评估模型效果、监控应用性能和资源消耗，对运维提出了新的要求。

1.  **数据安全与合规性要求：** 企业内部数据通常包含商业机密或用户隐私，直接将这些数据发送给公共云上的 LLM 服务可能引发严重的数据安全和合规风险。

1.  **工具链分散：** 开发者可能需要在不同的工具和库（如模型调用库、向量数据库客户端、工作流引擎等）之间切换，缺乏一个统一、高效的开发与管理平台。

克服这些挑战，寻找能够简化开发流程、保障数据安全、并加速 AI 应用落地的解决方案，成为企业智能化转型的关键。在此背景下，Dify 平台应运而生。

## 1.3 定义 Dify：一个 LLMOps 平台

### **1.3.1 核心定义与定位**

**Dify**** **（寓意 "Do It For You"） 是一个开源的、专注于简化生成式 AI 应用开发与运营的 LLMOps （Large Language Model Operations） 平台。

+   **核心定位：** 它旨在成为连接先进 LLM 技术与企业实际应用之间的桥梁，让开发者和团队能够高效地构建、部署、管理和迭代基于 LLM 的、生产级别的智能应用。

+   **关键属性：**

+   开源 （Open Source）: 源代码开放，允许社区贡献、企业进行二次开发和深度定制，提高了透明度和灵活性。

+   **LLMOps 平台：** 强调其覆盖 AI 应用从开发** （Development） **到运营 （Operations） 的整个生命周期，提供包括 Prompt 管理、应用构建、部署发布、监控分析在内的一体化能力。

### 1.3.2 关键理念：BaaS 与 PaaS for AI

Dify 的设计哲学借鉴了成熟的云计算服务模式，并将其应用于 AI 应用开发领域：

+   **后端即服务 （Backend as a Se**rvice， BaaS） for AI: Dify 将构建 AI 应用所需的通用后端基础设施进行了封装和抽象。这包括与不同 LLM 提供商的接口适配、用户请**求处理、对话状态（上下文**）管理、与知识库（向量数据库）的交互、API 服务的生成与管理、日志记录等。开发者无需从零开始构建这些复杂的后端组件，可以像使用云服务一样，直接利用 Dify 提供的能力，从而聚焦于应用的核心业务逻辑。

+   **提示词即服务 （Prompt as a Servi**ce， PaaS） 与可视化工作流编排： Prompt（提示词）是与 LLM 交互的核心，其设计质量直接影响 AI **应用的效果。D**ify 创新性地提供了可视化的 Prompt 编排界面。用户可以通过拖拽节点、连接流程线的方式，直观地设计、组织和管理 Prompt 模板、设定变量、定义应用的工作流程（例如，是纯对话，还是需要结合知识库检索，或是需要调用外部工具）。这种方式将复杂的 Prompt 工程和应用逻辑设计过程平台化、可视化，显著降低了门槛，提高了迭代效率。

## 1.4 Dify 平台的核心功能与特性

Dify 平台提供了一系列强大的功能，以支持快速、高效地构建多样化的 AI 应用：

### **1.4.1 可视化工作流编排 （Visual Wor**kflow Orchestration）

+   **图形化界面：** 提供直观的画布，用户通过拖放预定义的功能节点（如“开始”、“用户输入”、“LLM”、“知识库检索”、“回答”等）并连接它们，来构建 AI 应用的处理流程。

+   **Prompt 工程简化：** 在界面中直接编写和管理 System Promp**t（系统指令）、用户输入指令**，并支持使用变量 （Variables） 使 Prompt 更加动态和灵活。

+   **上下文管理：** 内置对话历史（Context）管理机制，使得构建能够理解多轮对话的聊天机器人成为可能。

+   **在线调试与预览：** 支持在编排界面直接输入测试数据，实时预览应用运行效果，方便快速调试和优化 Prompt 及流程。

### **1.4.2 多模型支持与管理 （Multi-LLM S**upport & Management）

+   **广泛的模型兼容性：** 支持集成业界主流的商业 LLM 服务，如 OpenAI （GPT 系列）、Anthropic （Claude 系列）、Azure OpenAI 服务等。

+   **拥抱开源与私有化：** 同时支持接入开源 LLM（如 Llama 系列、ChatGLM 等）以及通过标准 API 接口部署在企业内部的私有化模型。

+   **统一管理：** 提供集中的模型提供商 （Model Provider） 配置界面，方便添加、管理 API Key 或访问凭证，并在应用中灵活切换和选用不同的模型进行测试或部署。

### **1**.4.3 集成 RAG 引擎 （Integrated RAG Engine - Knowledge Base）

+   **RAG （Retrieval-Augme**nted Generation） 概念： 检索增强生成技术，旨在通过结合外部知识库来增强 LLM 的回答能力，使其能够基于最新的、私有的或领域特定的信息进行响应，从而提高准确性、减少“幻觉”（Hallucination）。

+   知识库 （Knowledge Base） 功能：

+   **便捷的数据导入：** 支持多种数据源，包括直接上传本地文件（PDF， TXT， Markdown， DOCX， XLSX 等）、从 URL 抓取网页内容、或通过 API 同步结构化/非结构化数据。

+   **自动化处理流程：** 内建文本解析、智能分块 （Chunking）、向量化 （Vectorization） 等数据预处理步骤。用户只需上传数据，Dify 会自动完成这些相对复杂的后台处理。

+   **向量数据库集成：** 支持与多种主流向量数据库（如 Weaviate， Qdrant， Milvus， Pinecone 等）对接，用于存储文本向量并实现高效的语义检索。

+   **无缝应用集成：** 在可视化编排界面中，可以轻松地将“知识库检索”节点加入到应用流程中，使应用具备基于私有知识问答的能力。

### **1.4.4 Agent 框架 （Agent Frame**work for Tool Usage）

+   **Agent 定义：** 指具备规划能力、并能调用外部**工具 （Tools）** 来完成特定任务的 LLM 应用。它扩展了 LLM 的能力边界，使其从信息生成者转变为任务执行者。

+   **工具 （Tool） 的概念：** 在 Dify 中，工具通常指一个定义好的外部 API 调用或可执行的代码片段。例如，查询天气、执行数据库查询、调用内部 CRM 系统接口等。

+   **自定义工具支持：** Dify 允许用户创建和集成自定义工具，通过配置 API 描述或提供代码实现，将企业现有的服务能力或特定功能赋能给 Agent。

+   **应用场景：** 可用于构建更智能的自动化工作流，如自动化报告生成、智能客服工单处理、跨系统信息整合等。

**1**.4.5 应用模板与 API 发布 （Application Templates & API Publishing）

+   **预置模板：** 提供常见的 AI 应用类型模板（如对话机器人、文本生成器、知识问答应用等），作为快速启动项目的基础。

+   **一键发布 API：** 构建完成的应用可以方便地发布为一个标准的 RESTful API 接口，并生成相应的 API Key。这使得将 Dify 构建的 AI 能力集成到企业现有的网站、移动应用、内部系统或自动化脚本中变得非常容易。

+   **基础监控：** 提供对已发布 API 的调用量、Token 消耗、执行日志等基本监控功能。

## 1.5 Dify 对企业的价值主张

采用 Dify 平台为企业构建 AI 解决方案，可以带来多方面的显著价值：

### 1.5.1 降低开发门槛，加速应用落地

+   可视化的开发方式和高度封装的功能模块，使得非 AI 专家也能参与 AI 应用的构建，降低了对专业技能的要求。

+   显著缩短了从概念到原型再到生产部署的周期，提高了开发效率，使企业能够更快地验证 AI 应用的价值并响应市场变化。

### 1.5.2 通过私有化部署保障数据安全与合规

+   （核心优势）支持 Docker 部署： Dify 可以完整地部署在企业自有的服务器或私有云环境**中。这意味着所有的数据处理**，包括用户输入、与 LLM 的交互（如果 LLM 也私有化部署）、以及知识库中的企业数据，都保留在企业可控的网络边界内，最大限度地保障了数据安全，满足了金融、政务、医疗等行业对数据隐私和合规性的严格要求。

+   **模型选择自主可控：** 企业可以选择接入完全私有的本地模型，或经过严格安全评估的云端模型服务，对 AI 能力的来源和使用拥有控制权。

### 1.5.3 提供灵活性与可扩展性

+   **开源基础：** 允许企业根据自身特定需求进行二次开发或功能定制。

+   **Agent 框架：** 通过自定义工具，可以灵活地将 Dify 与企业现有的各种系统和服务进行连接，扩展应用的能力范围。

+   **架构考虑：** 组件化的架构设计为未来的水平扩展（如增加 Worker 节点处理更高并发）提供了可能性。

### 1.5.4 Dify 的定位比较

为更清晰地理解 Dify 的定位，可将其与另外两种常见的 AI 应用开发方式进行比较：

+   **对比直接调用 LLM API：** 直接调用 API 提供了最大的底层灵活性，但需要开发者自行处理 Prompt 管理、RAG 实现、Agent 逻辑、上下文维护、API 封装、日志监控等大量工作。Dify 则将这些通用能力平台化，提供了更高层次的抽象和开箱即用的解决方案。

+   **对比 **L**an**gChain/LlamaIndex 等开发**框架： Lan**gChai**n 等是强大的代码库，为开**发者提供了构建 AI 应用的模块化工具，非常灵活，适合需要深度定制和复杂逻辑的场景。但它们本质上是面向程序员的开发框架，需要编写大量代码，且不直接提供可视化界面、应用管理和运维平台。Dify 则是一个完整的应用平台，更侧重于应用的快速构建、部署和管理，牺牲了部分底层编码的灵活性，换取了更高的开发效率和易用性。

## **1.6 Dify 系统架构概览**

理解 Dify 的基本架构有助于后续的部署和运维。一个典型的 Dify 部署包含以下主要组件（参见 图 1-1，此处为占位符，示意需要架构图）：

![](img/7e463ddf202c368c1853b9b9d8986392.png)

```
graph TD
    User --&gt;|HTTPS| FE(Web Frontend)
    FE --&gt;|API Calls| API(API Service)
    API --&gt; DB[(PostgreSQL)]
    API --&gt; Cache[(Redis)]
    API --&gt;|Async Tasks| Worker(Worker Service)
    Worker --&gt; Cache
    Worker --&gt; DB
    API --&gt;|LLM Calls| LLM{{LLM Provider}}
    API --&gt;|Vector Operations| VDB[(Vector DB&lt;br/&gt;Weaviate/Qdrant)]
    Worker --&gt;|Document Processing| VDB

    subgraph Dify Core Services
        FE
        API
        Worker
    end

    subgraph Core Dependencies
        DB
        Cache
        VDB
    end

    subgraph External Services
        LLM
    end

    style FE fill:#eff6ff,stroke:#60a5fa,stroke-width:2px
    style API fill:#f0fdfa,stroke:#2dd4bf,stroke-width:2px
    style Worker fill:#f5f3ff,stroke:#a78bfa,stroke-width:2px
    style DB fill:#fffbeb,stroke:#f59e0b,stroke-width:1px
    style Cache fill:#fff1f2,stroke:#f43f5e,stroke-width:1px
    style VDB fill:#ecfdf5,stroke:#34d399,stroke-width:1px
    style LLM fill:#f8fafc,stroke:#94a3b8,stroke-width:1px
```

### **1.6.1 关键服务组件**

+   Web 前端 （Frontend）: 用户交互界面，提供所有可视化操作入口，通常基于 React/Next.js 等现代前端框架构建。

+   API 服务 （API Service）: 系统的核心后端服务，处理来自前端或外部 API 的请求，执行应用逻辑（如调用 LLM、检索知识库），与数据库、缓存等进行交互。通常基于 Python （Flask/FastAPI） 构建。

+   Worker 服务 （Worker Service）: 负责执行后台的、耗时的异步任务，例如知识库文档的处理（清洗、分块、向量化）、异步的 Agent 工具调用等。它与 API 服务解耦，提高了系统的响应能力和稳定性。通常基于 Celery 等任务队列框架实现。

### 1.6.2 核心依赖组件 （Dependencies）

+   **关系型数据库 （Database **- e.g。， PostgreSQL）: 用于持久化存储系统的结构化数据，如应用配置、用户账户、对话历史、知识库元数据、操作日志等。

+   **向**量数据库 （Vector Database - e.g。， Weaviate， Qdrant， Milvus）: 专门用于存储文本（或其他模态）的向量表示 （Embeddings），并提供高效的相似度检索能力，是 RAG 功能的核心支撑。

+   **缓存/消息队列 （Cache/Message Q**ueue - e.g。， Redis）: 用于缓存热点数据以提高性能、存储用户会话信息，并作为 Worker 服务的消息中间件（任务队列）。

（图 1-1: Dify 简化逻辑架构图 - 占位符）

## 1.7 关键术语 （Key Terms）

+   LLM （Large Language Model）: 大语言模型，经过海量文本数据训练、能够理解和生成自然语言的深度学习模型。

+   **LLMOps （Large Languag**e Model Operations）: 围绕大语言模型应用的开发、部署、监控和管理的实践与工具集合。

+   Prompt Engineering: 设计和优化输入给 LLM 的指令（Prompt）以获得期望输出的技术。

+   **RAG （Retrieval-Au**gmented Generation）: 检索增强生成，一种结合外部知识库检索来提升 LLM 回答质量的技术范式。

+   Knowledge Base （知识库）: 在 Dify 中指存储用于 RAG 的私有或领域特定信息的集合，通常包含经过处理和向量化的文档片段。

+   Vector Database （向量数据库）: 专门用于存储和高效查询高维向量数据的数据库，是实现 RAG 的关键基础设施。

+   **Embedding:** 将文本等非结构化数据转换为低维、稠密的数学向量表示的过程或结果。

+   **Agent:** 具备规划能力并能调用外部工具（Tools）来执行任务的 LLM 应用。

+   **Tool （工具）:** 在 Dify Agent 框架中，指一个可被 Agent 调用的外部 API 或代码片段，用于执行特定操作。

+   Backend as a Service （BaaS）: 后端即服务，一种云计算模型，提供预构建的后端服务（如数据库、认证、API 等）供开发者使用。

+   Platform as a Service （PaaS）: 平台即服务，一种云计算模型，提供应用程序开发、部署和管理所需的平台和环境。

+   **Docker:** 一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器或 Windows 机器上，也可以实现虚拟化。

+   **Docker Compose:** 用于定义和运行多容器 Docker 应用程序的工具。

## **1.8 本章小结**

本章首先阐述了企业在引入生成式 AI 技术时普遍面临的挑战，引出了 Dify 平台作为应对这些挑战的解决方案。我们定义了 Dify 是一个开源的 LLMOps 平台，其核心理念在于通过 BaaS 和 PaaS 的方式简化 AI 应用的开发与运营。接着，详细介绍了 Dify 的核心功能特性，包括可视化工作流编排、多模型支持、集成的 RAG 引擎以及强大的 Agent 框架。我们分析了 Dify 为企业带来的关键价值，特别是在提升效率、保障数据安全（通过私有化部署）以及提供灵活性方面，并将其与直接调用 API 和使用底层开发框架进行了比较。最后，概述了 Dify 的系统架构，介绍了其主要的服务组件和依赖项。通过本章的学习，您应对 Dify 平台有了基本的认识，为其后续的部署和应用实践奠定了基础。

## **1.9 讨论与思考**

1.  结合您所在企业或行业的具体情况，讨论引入生成式 AI 应用可能遇到的主要障碍是什么？Dify 平台在哪些方面可能提供帮助？

1.  RAG 技术的核心价值是什么？试举例说明在您的业务场景中，哪些问题可以通过基于知识库的问答应用来解决？

1.  Agent 框架通过赋予 LLM 使用工具的能力，极大地扩展了其应用潜力。设想一下，在您的工作流程中，有哪些任务可以通过配置自定义工具的 Agent 来实现自动化或智能化？

1.  对于企业而言，数据安全和合规性至关重要。为什么说 Dify 的私有化部署能力是其核心优势之一？

# **第二章：Dify 环境部署与配置 （Deploymen**t and Configuration）

## **2.1 学习目标**

在本章学习结束后，您应能够：

+   区分 Dify 的云版本和私有化部署方式，并阐述私有化部署的主要优势。

+   列出使用 Docker 部署 Dify 的软硬件前提条件。

+   获取并理解 Dify 的 docker-compose.yaml 配置文件结构。

+   根据实际需求修改 Dify 的核心配置项，如端口、数据卷和关键环境变量。

+   使用 Docker Compose 命令成功启动 Dify 平台及其依赖服务。

+   完成 Dify 控制台的初始化设置。

+   诊断和解决 Docker 部署过程中常见的端口冲突、容器启动失败等问题。

+   在 Dify 平台中添加和配置不同类型的大语言模型（LLM）提供商。

## **2.2 部署方式概述**

Dify 平台提供了灵活的部署选项，以满足不同用户和企业的需求。主要有两种方式：Dify Cloud （云版本） 和私有化部署 （Self-Hosted）。

### 2.2.1 Dify Cloud （云版本）

Dify Cloud 是由 Dify 官方提供的托管服务。用户无需关心底层基础设施的搭建和维护，只需注册账号即可开始使用 Dify 的核心功能。

+   **优点：**

+   **开箱即用：** 无需部署和配置，注册即可使用。

+   **免运维：** 由官方负责平台的维护、升级和稳定性保障。

+   **快速体验：** 非常适合个人开发者、小型团队或初次体验 Dify 功能的用户。

+   **缺点：**

+   **数据隐私：** 数据存储在 Dify Cloud 的服务器上，对于有严格数据安全和合规要求的企业可能不适用。

+   **定制化受限：** 无法进行深度的二次开发或系统级定制。

+   **可能存在资源限制：** 根据不同的订阅计划，可能在用量、功能等方面存在限制。

### 2.2.2 私有化部署 （Self-Hosted）

私有化部署是指将 Dify 完整地部署在用户自己控制的基础设施上，例如企业内部服务器、私有云或公有云虚拟机。Dify 官方推荐并主要支持使用 Docker 进行私有化部署，这也是本章的重点。

+   **优点：** （详见 2.2.3）

+   **数据完全掌控：** 所有数据（用户输入、知识库、模型交互日志等）都存储在企业内部，最大限度保障数据安全与合规。

+   **高度定制化：** 基于开源特性，可以进行二次开发和深度集成。

+   **网络隔离：** 可以部署在内部网络，与公网隔离。

+   **无平台功能限制：** 可以使用 Dify 的全部开源功能。

+   **缺点：**

+   **需要自行部署与维护：** 需要投入资源进行环境搭建、配置、监控、升级和故障排查。

+   **对基础设施有要求：** 需要准备满足要求的服务器资源。

### 2.2.3 为何选择私有化部署？

**对于大多数企业应用场景，尤其**是有以下考量的企业，私有化部署通常是更合适的选择：

1.  **数据安全与隐私保护 （Data **Security & Privacy）: 这是最重要的原因。企业内部数据、客户信息、知识产权等敏感内容不离开企业控制范围，符合 GDPR、数据安全法等法规要求。

1.  合规性要求 （Compliance）: 金融、医疗、政务等行业对数据的存储地点、处理方式有严格的合规性规定，私有化部署更容易满足这些要求。

1.  **系统集成与定制化 （Integrati**on & Customization）: 需要将 Dify 与内部系统（如 OA， ERP， CRM）进行深度集成，或根据特定业务需求对 Dify 进行二次开发或功能扩展。

1.  网络策略 （Network Policy）: 企业可能有严格的网络访问控制策略，需要将 AI 应用部署在内网环境。

1.  模型选择灵活性 （Model Flexibility）: 可以更方便地接入部署在内网的私有化 LLM，或对访问公有模型进行统一的代理和管理。

## **2.3 使用 Docker 进行私有化部署 （Self-**Hosting with Docker）

Docker 提供了一种轻量级、可移植的应用打包和部署方式，是 Dify 官方推荐的私有化部署方法。通过 Docker Compose，可以方便地定义和管理 Dify 平台及其所需的多个依赖服务容器。

快速安装(然后可以直接快进到2.3.7进行初始化)

```
git clone https://github.com/langgenius/dify
cd dify
cd docker
cp .env.example .env
docker compose up -d
```

### 2.3.1 部署架构与依赖 （回顾）

如第一章所述，一个典型的 Dify Docker 部署包含以下容器化服务：

+   **核心服务：**

+   api： Dify 后端 API 服务。

+   web： Dify 前端 Web 界面服务。

+   worker： 处理异步任务的 Worker 服务。

+   **依赖服务：**

+   db： PostgreSQL 数据库，存储元数据。

+   redis： Redis 缓存和消息队列。

+   weaviate （或其他向量数据库）: 用于 RAG 功能的向量存储和检索（这是默认配置，可替换）。

Docker Compose 会根据配置文件 （docker-compose.yaml） 统一管理这些容器的构建、启动、停止和网络连接。

### 2.3.2 前提条件 （Prerequisites）

在开始部署之前，请确保您的部署环境满足以下条件：

1.  硬件资源 （Hardware Requirements）:

+   **CPU:** 建议至少 2 核 CPU，生产环境建议 4 核或以上。

+   **内存 （RAM）:** 建议至少 4GB 可用内存，生产环境建议 8GB 或以上。如果知识库数据量大或并发用户多，需要更多内存。

+   **存储 （Storage）:** 建议至少 40GB 可用磁盘空间，用于存储 Docker 镜像、容器数据卷（数据库、知识库文件、向量数据等）。具体需求取决于数据量。

+   **网络：** 确保服务器具有稳定的网络连接，并且需要开放的端口（如 Web 访问端口）未被防火墙阻止。

1.  软件环境 （Software Requirements）:

+   **操作系统 （OS）:** 推荐使用较新版本的 Linux 发行版（如 Ubuntu 20.04/22.04， CentOS 7/8 Stream， Debian 11 等）。也可以在 macOS 或 Windows （通过 WSL2） 上部署，但 Linux 环境更为常见和推荐。

+   **Docker:** 安装最新稳定版的 Docker Engine。

+   官方安装文档： [https://docs.docker](https://www.google.com/url?sa=E&q=https%3A%2F%2Fdocs.docker.com%2Fengine%2Finstall%2F).com/engine/install/

+   验证安装： 运行 docker --version

+   **Docker Compose:** 安装 Docker Compose V2 （通常随 Docker Desktop 或新版 Docker Engine 一起安装） 或 V1 （独立安装）。

+   官方安装文档： [https://docs.docker.](https://www.google.com/url?sa=E&q=https%3A%2F%2Fdocs.docker.com%2Fcompose%2Finstall%2F)com/compose/install/

+   验证安装： 运行 docker compose version （V2） 或 docker-compose --version （V1）

+   **（可选） Git:** 用于克隆 Dify 仓库获取部署文件。

### 2.3.3 获取 Dify Docker 部署文件

获取部署所需 docker-compose.yaml 文件的常用方式是直接从 Dify 的官方 GitHub 仓库下载或克隆：

1.  **使用 Git 克隆 （推荐）:**

```
# 选择一个合适的目录存放 Dify 部署文件cd /path/to/your/deployment/directory

# 克隆 Dify 仓库的 docker 目录内容# (如果只需要部署文件，可以考虑只下载 docker 目录)# 或者直接克隆整个仓库（可能较大）
git clone https://github.com/langgenius/dify.git
cd dify/docker
```

1.  注意：仓库地址和结构可能随版本更新变化，请参考 Dify 官方文档确认最新地址。

1.  **直接下载：**
访问 Dify GitHub 仓库 （https://github.com/langgenius/dify)，找到 docker 目录下的 docker-compose.yaml 文件以及可能需要的 .env 模板文件，手动下载到您的服务器。

```
chengzh@TB15P:~$ git clone https://github.com/langgenius/dify
Cloning into &#x27;dify&#x27;...
remote: Enumerating objects: 155341, done.
remote: Counting objects: 100% (243/243), done.
remote: Compressing objects: 100% (124/124), done.
remote: Total 155341 (delta 178), reused 119 (delta 119), pack-reused 155098 (from 3)
Receiving objects: 100% (155341/155341), 84.15 MiB | 13.09 MiB/s, done.
Resolving deltas: 100% (114545/114545), done.
Updating files: 100% (5350/5350), done.
chengzh@TB15P:~$ cd dify/docker
chengzh@TB15P:~/dify/docker$ dir
README.md                     docker-compose.middleware.yaml  generate_docker_compose  ssrf_proxy
certbot                       docker-compose.png              middleware.env.example   startupscripts
couchbase-server              docker-compose.yaml             nginx                    tidb
docker-compose-template.yaml  elasticsearch                   pgvector                 volumes
chengzh@TB15P:~/dify/docker$
```

### **2.3.4 理解与修改 **docker-compose.yaml

docker-compose.yaml 文件是 Docker Compose 的核心配置文件，它定义了 Dify 应用所需的所有服务、网络、数据卷等。在启动前，务必理解其结构并根据需要进行修改。

+   **文件结构概览：** 文件通常包含 version （指定 Compose 文件版本）、services （定义各个服务容器）、networks （定义容器网络）、volumes （定义数据持久化卷） 等部分。

+   **关键服务 （****services****）:**

+   api， web， worker： Dify 的核心组件。通常需要关注它们的 image （使用的 Docker 镜像）、ports （端口映射）、volumes （数据挂载）、environment （环境变量配置）、depends_on （启动依赖关系）。

+   db， redis， weaviate （或同类）: 依赖的服务。需要关注它们的 image、ports （通常只在需要外部访问时映射）、volumes （非常重要，用于持久化数据）、environment （用于设置密码、配置等）。

+   **端口映射 （****ports****）:**

+   格式为 "HOST_PORT:CONTAINER_PORT"。例如，"80:3000" 表示将宿主机的 80 端口映射到 web 容器的 3000 端口。

+   **重点检查 ****web**** 服务的端口映射**，这是您访问 Dify 控制台的入口。确保宿主机上的端口（如 80 或其他自定义端口）未被其他应用占用。

+   其他服务（如 db， redis， weaviate）的端口默认**不需要**映射到宿主机，除非您有特殊需求要从外部直接访问这些服务。容器之间可以通过 Docker 的内部网络进行通信。

+   **数据卷 （****volumes****）:**

+   用于将容器内的数据持久化到宿主机上，防止容器删除后数据丢失。例如，数据库文件、Redis 数据、向量数据、上传的知识库文件等。

+   格式通常为 VOLUME_NAME:/path/in/container 或 HOST_PATH:/path/in/container。

+   **务必确保为 ****db****， ****redis****， ****weaviate** 以及可能存储知识库文件的卷配置了正确的持久化。建议使用 Docker 管理的命名卷 （Named Volumes），如 pg_data:/var/lib/postgresql/data。

+   **环境变量 （****environment****）:**

+   **用于向容器内部传递配置信息。**这是定制化 Dify 行为的关键。

+   可以在 docker-compose.yaml 的 environment 部分直接设置，或者将敏感信息或常用配置放在一个单独的 .env 文件中（Compose 会自动加载同目录下的 .env 文件）。

+   重要环境变量将在 2.3.5 节详细说明。

### 2.3.5 核心配置项调整 （通过环境变量）

在启动 Dify 前，您可能需要根据实际情况调整以下关键配置项，通常通过修改 .env 文件或 docker-compose.yaml 中的环境变量来实现：

1.  数据库连接配置 （Database Connection）:

+   通常涉及 DB_USERNAME， DB_PASSWORD， DB_DATABASE， DB_HOST， DB_PORT 等变量。

+   如果使用 docker-compose.yaml 中定义的 db 服务，DB_HOST 通常应设置为服务名 db，端口为容器内部端口 （如 5432）。

+   **强烈建议修改默认的数据****库**密码 （DB_PASSWORD）。确保 api， worker 服务中的数据库连接信息与 db 服务自身设置的环境变量一致。

1.  Redis 连接配置 （Redis Connection）:

+   通常涉及 REDIS_HOST， REDIS_PORT， REDIS_PASSWORD， REDIS_DB 等变量。

+   如果使用 Compose 内的 redis 服务，REDIS_HOST 通常设置为 redis，端口为容器内部端口 （如 6379）。

+   如果 Redis 设置了密码，确保在此处配置。

1.  **向量数据库配置 （Vector Data**base Configuration）:

+   Dify 支持多种向量数据库。默认 docker-compose.yaml 通常配置了 Weaviate。

+   如果使用内置的 Weaviate，检查其服务配置和 Dify （API， Worker） 中对应的环境变量（如 VECTOR_STORE_ENDPOINT， WEAVIATE_API_KEY 等）是否匹配。

+   （可选）更换或配置外部向量数据库： 如果您希望使用其他的向量数据库（如 Qdrant， Milvus）或连接到已有的外部向量数据库实例，需要：

+   注释掉或删除 docker-compose.yaml 中默认的向量数据库服务（如 weaviate）。

+   修改 Dify 服务 （api， worker） 的环境变量，设置 VECTOR_STORE 为您选择的类型（如 qdrant， milvus），并配置相应的连接地址 （QDRANT_URL， MILVUS_HOST 等） 和认证信息（如 QDRANT_API_KEY， MILVUS_TOKEN 等）。具体变量请参考 Dify 官方文档。

1.  **（可选）配置外部访问域名和 HTT**PS （Domain & HTTPS）:

+   如果希望通过域名访问 Dify，并在生产环境中使用 HTTPS 加密连接：

+   设置 CONSOLE_URL 和 API_URL 环境变量为您的域名 （例如 https://dify.yourcompany.com）。

+   这通常需要配合反向代理服务器（如 Nginx， Traefik， Caddy）来实现。反向代理负责处理 HTTPS 证书和将外部请求转发到 Dify 的 web （端口 80 或 3000） 和 api （端口 5001） 容器。docker-compose.yaml 中 web 和 api 的端口映射可能需要调整为只对反向代理暴露，而不是直接暴露到公网。配置反向代理超出了 Dify 本身的范围，但对于生产部署至关重要。

1.  **（可选）其他配置：**

+   SECRET_KEY： 用于 Flask 应用的安全密钥，建议修改为强随机字符串。

+   UPLOAD_FILE_SIZE_LIMIT： 上传文件大小限制。

+   邮件服务配置 （用于邀请成员等）。

+   ... 更多配置请查阅 Dify 官方文档的环境变量说明。

**修改 ****.env**** 文件示例：**

```
# .env file in the same directory as docker-compose.yaml# Database - !!! CHANGE DEFAULT PASSWORD !!!
POSTGRES_DB=dify
POSTGRES_USER=dify
POSTGRES_PASSWORD=your_strong_postgres_password # Change this!# Redis - Change password if needed
REDIS_PASSWORD=your_redis_password # Change or remove if Redis has no password# Dify API &amp; Web URLs (if using domain/proxy)# CONSOLE_URL=https://dify.yourcompany.com# API_URL=https://dify.yourcompany.com/api# Flask Secret Key - !!! CHANGE THIS TO A RANDOM STRING !!!
SECRET_KEY=your_very_strong_random_secret_key # Change this!# Vector Store (Example using default Weaviate)
VECTOR_STORE=weaviate
WEAVIATE_GRPC_ENABLED=false # Check Dify version docs for default
WEAVIATE_API_KEY= # Set if Weaviate requires an API key# ... other environment variables
```

确保 .env 文件中的变量名与 docker-compose.yaml 中 environment 部分使用的 ${VARIABLE_NAME} 占位符匹配。

可以直接复制 .env.example 作为新的 .env 文件来使用

```
cp .env.example .env
```

### 2.3.6 启动 Dify 服务

完成配置文件的检查和修改后，在包含 docker-compose.yaml 文件的目录下，执行以下命令启动 Dify 及其所有依赖服务：

```
 # 使用 Docker Compose V2 (推荐)
docker compose up -d

# 或者使用 Docker Compose V1# docker-compose up -d
```

+   up： 创建并启动服务容器。

+   -d： （Detached mode） 在后台运行容器，不会阻塞当前终端。

首次运行时，Docker 会下载所需的所有镜像，这可能需要一些时间，具体取决于网络速度。启动过程中，可以通过以下命令查看容器状态：

```
# 查看所有正在运行的容器 (V2)
docker compose ps

# 查看所有正在运行的容器 (V1)# docker-compose ps# 查看特定服务的日志 (例如 api 服务) (V2)
docker compose logs api

# 查看特定服务的日志 (V1)# docker-compose logs api# 查看所有服务的日志并持续跟踪 (V2)
docker compose logs -f

# 查看所有服务的日志并持续跟踪 (V1)# docker-compose logs -f
```

等待所有服务都显示为 Up 或 Running 状态，并且日志中没有明显的错误信息，表示 Dify 平台已成功启动。

```
chengzh@TB15P:~/dify/docker$ docker ps
CONTAINER ID   IMAGE                                       COMMAND                  CREATED         STATUS                   PORTS                                        NAMES
b0dea5a6fbd0   nginx:latest                                &quot;sh -c &#x27;cp /docker-e…&quot;   4 minutes ago   Up About a minute        0.0.0.0:80-&gt;80/tcp, 0.0.0.0:443-&gt;443/tcp     docker-nginx-1
f8b141415aaa   langgenius/dify-api:1.1.3                   &quot;/bin/bash /entrypoi…&quot;   4 minutes ago   Up 3 minutes             5001/tcp                                     docker-api-1
80440eefa6d3   langgenius/dify-api:1.1.3                   &quot;/bin/bash /entrypoi…&quot;   4 minutes ago   Up 3 minutes             5001/tcp                                     docker-worker-1
c47cb024efdc   langgenius/dify-plugin-daemon:0.0.6-local   &quot;/bin/bash -c /app/e…&quot;   4 minutes ago   Up 2 minutes             0.0.0.0:5003-&gt;5003/tcp                       docker-plugin_daemon-1
58ea057b59b8   postgres:15-alpine                          &quot;docker-entrypoint.s…&quot;   4 minutes ago   Up 3 minutes (healthy)   5432/tcp                                     docker-db-1
ca4497a77c2d   langgenius/dify-sandbox:0.2.11              &quot;/main&quot;                  4 minutes ago   Up 3 minutes (healthy)                                                docker-sandbox-1
4c9ff33f10d0   langgenius/dify-web:1.1.3                   &quot;/bin/sh ./entrypoin…&quot;   4 minutes ago   Up 3 minutes             3000/tcp                                     docker-web-1
7873d487b38c   redis:6-alpine                              &quot;docker-entrypoint.s…&quot;   4 minutes ago   Up 3 minutes (healthy)   6379/tcp                                     docker-redis-1
79f610ba9d7a   ubuntu/squid:latest                         &quot;sh -c &#x27;cp /docker-e…&quot;   4 minutes ago   Up 3 minutes             3128/tcp                                     docker-ssrf_proxy-1

```

### 2.3.7 访问控制台与初始化设置

启动成功后，通过浏览器访问您在 docker-compose.yaml 文件中为 web 服务配置的宿主机端口（如果在 2.3.5 中配置了域名和反向代理，则访问该域名）。

+   **默认情况：** 如果 web 服务端口映射为 "80:3000"，则访问 http://<your_server_ip>。如下图所示

![](img/7219f94bdd12a660228118ce5e8da1b0.png)

+   **其他端口：** 如果映射为 "3000:3000"，则访问 http://<your_server_ip>:3000。

首次访问时，Dify 会引导您完成初始化设置：

1.  **设置管理员账户：** 创建第一个管理员用户的邮箱、用户名和密码。请务必牢记此账户信息。

![](img/fe23aa70539400cb5ee660db8298e58e.png)

1.  **（可能）基础配置确认：** 可能需要确认一些基础配置信息。

完成初始化后，您就可以使用管理员账户登录 Dify 控制台，开始探索和使用平台了。

![](img/eb8a193d62321a4a801abc67988ab6a4.png)

### 2.3.8 常见部署问题与排查 （Troubleshooting）

在部署过程中遇到问题是常见的，以下是一些常见问题及其排查思路：

1.  端口冲突 （Port Conflict）:

+   **现象：** docker compose up 时报错，提示端口已被占用 （Address already in use）。

+   **原因：** 您在 docker-compose.yaml 中映射到宿主机的端口（尤其是 web 服务的端口，如 80）已被服务器上的其他进程监听。

+   **排查：** 使用 netstat -tulnp | grep <port_number> 或 ss -tulnp | grep <port_number> （Linux） 查找占用端口的进程。

+   **解决：**

+   停止占用端口的进程。

+   修改 docker-compose.yaml 中冲突的端口映射，将宿主机端口改为一个未被占用的端口（例如，将 "80:3000" 改为 "8080:3000"，然后通过 http://<server_ip>:8080 访问）。

1.  容器启动失败 （Container Failed to Start）:

+   **现象：** docker compose ps 显示某个或某些容器状态为 Exited 或 Restarting。

+   **排查：** 使用 docker compose logs <service_name> （例如 docker compose logs api） 查看该容器的启动日志，查找具体的错误信息。常见的错误原因包括：

+   **配置错误：** 环境变量配置不正确（如数据库密码错误、依赖服务地址错误）。仔细检查 .env 文件和 docker-compose.yaml。

+   **依赖服务未就绪：** 核心服务（如 api）启动时，依赖的服务（如 db， redis）可能还未完全准备好。docker-compose.yaml 中的 depends_on 有助于控制启动顺序，但有时仍需等待。可以尝试重启服务 （docker compose restart <service_name>）。

+   **数据卷权限问题：** 挂载到宿主机的目录权限不正确，导致容器无法写入。检查挂载目录的权限。

+   **镜像问题：** 下载的 Docker 镜像损坏或不完整。可以尝试删除镜像 （docker rmi <image_name>） 后重新 docker compose up。

+   **资源不足：** （见下文）。

1.  网络问题 （Network Issues）:

+   **现象：** 容器之间无法通信（例如 api 无法连接到 db 或 redis）。

+   **排查：** Docker Compose 会自动创建默认的网络，并将所有服务连接到该网络。确认 docker-compose.yaml 中服务都属于同一个网络。使用 docker exec -it <container_name> ping <other_service_name> （例如 docker exec -it dify-api-1 ping db） 测试容器间的网络连通性。检查防火墙规则（宿主机或 Docker 网络层面）是否阻止了容器间通信（通常默认允许）。

1.  资源不足 （Insufficient Resources）:

+   **现象：** 容器频繁崩溃、服务响应缓慢或 OOM （Out Of Memory） 错误。

+   **排查：** 使用 docker stats 查看各容器的 CPU 和内存使用情况。检查服务器的总资源使用情况 （top， htop， free -h）。

+   **解决：** 确保服务器满足最低硬件要求（见 2.3.2）。如果资源确实不足，需要升级服务器硬件（增加 CPU 或 RAM）。

**其他操作：**

+   **停止服务：** docker compose down （停止并移除容器、网络） 或 docker compose stop （仅停止容器）。

+   **更新 Dify:** 通常需要拉取最新的代码/配置文件 （git pull），然后 docker compose pull （拉取最新镜像），最后 docker compose up -d --remove-orphans （重新创建容器）。更新前务必备份数据并仔细阅读官方更新说明！

## 2.4 连接与配置大语言模型 （LLM）

成功部署并登录 Dify 控制台后，下一步关键操作是配置 Dify 连接到您希望使用的 LLM 服务。缺少可用的 LLM 配置，Dify 将无法执行其核心的生成式 AI 功能。本节将重点介绍如何连接国内常用的 LLM 服务，特别是以 DeepSeek API 为例进行说明。

### 2.4.1 支持的模型提供商类型

Dify 平台设计上具有良好的扩展性，能够支持多种类型的 LLM 提供商。对于国内用户而言，常见的选择包括：

+   **国内商业 API 服务：** 这是目前国内企业和开发者最常用的方式之一。许多国内科技公司推出了自己的大模型平台，并提供 API 供调用。主要包括：

+   **DeepSeek:** 由深度求索公司提供，以其高性价比和在代码、数学方面的优异表现受到关注。提供与 OpenAI API 兼容的接口，接入方便。

+   火山引擎 （Volcengine）: 字节跳动旗下的云服务平台，提供包括“豆包大模型”（原云雀大模型）在内的多种自研及第三方大模型服务，功能全面，适用于多种企业级场景。通常也提供 API 供调用，可能需要特定的认证方式（AK/SK）。

+   硅基流动 （Silicon Intelligence）: 专注于大模型技术研发的公司，推出了 Step 系列等模型，在特定领域可能具有优势。（备注：其公开 API 服务的具体形式和易用性可能需进一步了解，接入方式可能与其他平台有所不同）。

+   **其他主流服务商：**

+   百度智能云 （Baidu Cloud）: 提供文心（ERNIE）系列大模型 API。

+   阿里云 （Alibaba Cloud）: 提供通义（Qwen）系列大模型 API。

+   智谱 AI （Zhipu AI）: 提供 ChatGLM 系列模型 API。

+   **MiniMax:** 提供其 ABAB 系列模型 API。

+   月之暗面 （Moonshot AI）: 提供 Kimi （Moonshot） 模型 API。

+   （Dify 对这些平台的直接支持情况可能不同，但很多平台提供了兼容 OpenAI API 的调用方式，增加了接入的可能性）

+   **国际商业 API 服务：** 如 OpenAI （GPT 系列）， Azure OpenAI， Anthropic （Claude 系列） 等，虽然在国内访问可能受网络或政策影响，但在特定场景下或对于有海外业务需求的用户仍是选项之一。

+   开源模型 （Open-Source Models） 本地部署：

+   通过 **Ollama， vLLM， LocalAI， **Xinference， FastChat 等框架，可以在本地服务器部署 Llama， Mistral， Qwen （开源版）， ChatGLM （开源版） 等模型，并将其封装为兼容 OpenAI API 的接口。这是保障数据完全私有、节约成本（若有合适硬件）的重要方式。Dify 可以非常方便地连接这类本地服务。

+   企业自建模型 （Proprietary Models）: 若企业内部自研或定制了 LLM，并提供了标准 API 接口（强烈建议兼容 OpenAI API 格式），同样可以在 Dify 中进行配置接入。

**对**于本课程，我们将重点演示如何接入 DeepSeek API，因为它在国内用户中较为流行，且接入方式相对直接。

2.4.2 获取模型访问凭证 （以国内服务商为例）

要连接您选择的 LLM 服务，首先需要从对应的服务商平台获取访问凭证。以下以 DeepSeek 和火山引擎为例说明：

+   获取 DeepSeek API Key:

1.  访问 DeepSeek 开放平台网站：https://platform.deepseek.com/

1.  注册并登录您的账户。

1.  在平台的控制台或“API 密钥”管理页面，创建新的 API Key。

1.  **记录下生成的 API Key**，这是调用服务的唯一凭证。

1.  通常 DeepSeek API 的 Base URL （基础 URL） 是 https://api.deepseek.com。

+   获取火山引擎访问凭证 （AK/SK）:

1.  访问火山引擎官方网站并登录您的账户。

1.  确保已开通“大模型服务平台”或相关服务。

1.  在“访问控制”（IAM） 或密钥管理相关的功能模块中，创建或获取 Access Key ID （AK） 和 Secret Access Key （SK）。AK/SK 是火山引擎通用的身份认证凭证。

1.  您还需要知道要使用的具体模型服务的 **Endpoi**nt （服务地址/区域） 以及可能的模型版本标识。这通常可以在模型的 API 文档或服务控制台中找到。例如，北京区域的通用大模型服务地址可能类似于 https://maas-api.ml-platform-cn-beijing.volces.com。

+   注意：火山引擎的 API 调用可能需要基于 AK/SK 进行签名计算，Dify 是否原生支持这种签名方式需要确认。如果 Dify 不直接支持，可能需要通过兼容 OpenAI API 的封装层或代理来接入。

+   **获取其他平台凭证：** 访问百度智能云、阿里云、智谱 AI 等平台的开发者中心或控制台，按照其文档指引创建应用、获取 API Key （或 AppID/Secret Key 等形式的凭证） 以及相应的 Base URL。

**安全提示：API Key、AK/SK 等同于您的账户密码，请**务必妥善保管，切勿泄露或硬编码在代码中。

### **2.4.3 在 Dify 中添加和管理模型** （以 DeepSeek API 为例）

获取访问凭证后，我们就可以在 Dify 控制台中进行配置了。以下是以添加 DeepSeek API 为例的详细步骤：

1.  **登录 Dify 控制台** （使用您在 2.3.7 节创建的管理员账户）。

1.  在左侧导航菜单中，点击进入 **“设置 （Settings）”** 区域。

![](img/382c996ad8e5e573b5f13059ec18ba10.png)

1.  在设置页面中，选择 “模型提供商 （Model Providers）” 选项卡。这里会列出所有已配置的模型。

![](img/ae0a1635bd24d167720d686155f8281d.png)

1.  通过搜索或者滚动页面找到合适的模型供应商：

![](img/ac40de97dc6d2bfe34434984455459ad.png)

1.  将鼠标移动到所选的模型上，会出现一个浮动窗口，选择安装

![](img/42ea988e451a82ff7cbcebe5145ab647.png)

1.  模型安装成功之后，会出现在待配置列表中

![](img/55255a6ad29018680d18ed6229d089e3.png)

1.  点击**设置**填写** API Key **和 API endpoint 地址： 此时会弹出一个配置表单，您需要填写以下关键信息：

![](img/967545a98119ef644eb1763b74b79207.png)

+   **API Key:** 在这里粘贴您从 DeepSeek 平台获取的 API Key。

+   API 地址 （API Base URL）:

+   填写 DeepSeek API 的基础 URL：https://api.deepseek.com

+   （注意：通常不需要在 Base URL 后添加 /v1 或其他路径，Dify 会根据 OpenAI 兼容模式自动拼接。但如果连接不成功，可尝试查阅 Dify 文档或 DeepSeek 文档确认是否需要包含 /v1）。

1.  **保存配置：**** 确认所有信息填写无误**后，点击 “保存 （Save）” 按钮。

保存成功后，您配置的 "DeepSeek-Chat-API" 就会出现在“模型提供商”列表中。现在，当您创建新的 Dify 应用或编辑现有应用时，就可以在模型选择下拉菜单中看到并选用这个 DeepSeek 模型了。

![](img/e8179bf815570ec6e5500a275feb45c2.png)

1.  点击页面上的系统模型设置，选择将 deepseek-chat 设置为系统推理模型

![](img/e79ec187a9f10acb04141c1908acf648.png)

**配置其他国内模型提供商：**

+   兼容 OpenAI API 的本地模型 （如 Ollama）: 同样选择 OpenAI 作为提供商类型，API Key 通常留空或填写任意字符，API Base URL 填写本地服务的地址 （如 http://<your_ollama_server_ip>:11434），模型名称填写本地可用的模型名。

+   **火山引擎、百度、阿里等：**

+   如果他们提供了兼容 OpenAI API 的 Endpoint 和认证方式（例如，某些服务可能允许你生成一个类似 API Key 的 Token），则可以尝试使用 OpenAI 提供商类型进行配置。

+   如果他们需要特定的认证方式（如火山引擎的 AK/SK 签名）或 API 结构不同，可能需要 Dify 提供原生支持，或者您需要自行搭建一个转换层（如使用 FastChat 搭建兼容 OpenAI 的代理服务来调用这些平台的原生 API）。建议查阅 Dify 的官方文档或社区，了解对特定国内平台原生支持的最新情况。

（可选）增加**嵌入式模型**和** Rerank **模型（需要使用**硅基流动**）

![](img/ec0517190d02e3144368c9090d091878.png)

## **2.5 本章小结**

本章详细介绍了 Dify 平台的两种主要部署方式：云版本和私有化部署，并重点阐述了私有化部署的优势，特别是对于企业级应用在数据安全和合规性方面的价值。核心内容聚焦于如何使用 Docker 和 Docker Compose 进行 Dify 的私有化部署，涵盖了从环境准备、获取部署文件、理解和修改核心配置（端口、数据卷、关键环境变量如数据库、Redis、向量库连接），到启动服务、访问控制台进行初始化设置，以及常见部署问题的排查方法。最后，本章指导了如何在成功部署的 Dify 平台中连接和配置所需的大语言模型提供商，这是让 Dify 应用能够运行起来的关键一步。掌握本章内容，您应具备独立部署和配置 Dify 私有化环境的能力。

## **2.6 实践练习**

1.  **环境检查：** 确认您的服务器或本地环境已安装 Docker 和 Docker Compose，并满足基本硬件要求。

1.  **获取部署文件：** 从 Dify 官方仓库获取最新的 docker-compose.yaml 和 .env.example 文件。

1.  **配置修改：**

+   将 .env.example 复制为 .env。

+   在 .env 文件中，**务必修改** POSTGRES_PASSWORD 和 SECRET_KEY 为强密码/密钥。

+   检查 docker-compose.yaml 中 web 服务的端口映射，确保宿主机端口可用。

1.  **启动 Dify:** 在包含 docker-compose.yaml 的目录下运行 docker compose up -d。

1.  **验证启动：** 使用 docker compose ps 检查所有容器是否正常运行。使用 docker compose logs -f 查看日志确认无严重错误。

1.  **初始化：** 访问 Dify 控制台 URL，完成管理员账户的创建。

1.  **配置 LLM:**

+   注册 De**epSeek 平台账**户 （https://platform.deepseek.com/) 并获取 API Key。

+   在 Dify 的“设置”->“模型提供商”中，按照 2.4.3 节**的步骤，选择 OpenAI 提供商****类**型，添加并配置您的 DeepSeek API Key 和 Base URL （https://api.deepseek.com）。给模型起一个清晰的名称（如 DeepSeek-Chat），并指定模型标识符（如 deepseek-chat）。

+   **保存配置**，并确认 DeepSeek 模型出现在列表中。

+   （可选，进阶）如果您有其他国内 LLM 平台的账号和凭证（如火山引擎 AK/SK，或本地 Ollama 服务），可以尝试按照 Dify 文档或社区指南进行配置，并注意可能遇到的认证或兼容性问题。

完成以上练习，您将拥有一个可以开始创建 AI 应用的 Dify 私有化运行环境。

好的，我们来撰写第三部分的内容。本章将完全侧重于动手实践，通过一个有趣的场景——创建一个“宇宙美食家小助手”聊天机器人，来学习 Dify 构建基础对话应用的核心操作。

# 第三部分：构建基础 AI 应用：聊天机器人 （Building Basic Chatbots）

在本章中，我们将从零开始，一步步在 Dify 平台上构建一个简单的对话型 AI 应用。我们将扮演一位“宇宙美食探险家”，创建一个能与我们畅聊来自各个星球（虚构）的奇特美食的聊天机器人。本章的目标是让您熟悉 Dify 创建和配置基础聊天应用的核心流程，特别是 Prompt 工程的基础实践。

## 3.1 创建您的第一个聊天机器人应用

在开始“烹饪”我们的宇宙美食之前，我们需要先在 Dify 中创建一个新的应用“厨房”。

### 3.1.1 选择应用类型：对话型

1.  **登录 Dify 控制台：** 使用您在第二章配置好的管理员账户登录您的 Dify 私有化部署环境。

1.  导航至“工作室 （Studio）”: 在 Dify 主界面，通常可以在左侧导航栏找到“工作室”或“应用 （Apps）”入口。点击进入。

![](img/c44c1c930903f39b5523a022ad9dbd8b.png)

1.  **创建新应用：** 在工作室内，找到并点击“**创建空白应用**”或类似按钮。

1.  **选择应用模式：**** Dify **会让您选择应用类型。对于聊天机器人，我们需要选择  “聊天助手” 模式。这种模式内置了多轮对话管理能力。

![](img/f57000db095997ee4583de9585ed27af.png)

1.  点击“创建”或“下一步”。

### 3.1.2 基础设置：命名与描述

接下来，为我们的“宇宙美食家小助手”进行基础设置：

1.  应用名称 （App Name）: 给您的应用起一个响亮又有趣的名字。例如：宇宙美食家小助手。

1.  应用图标 （App Icon）: 选择一个能代表您应用主题的图标。Dify 通常提供一些预设图标，您可以选择一个星球、食物或者机器人图标。也可以选择自定义颜色。

1.  应用描述 （App Description）: 简单描述一下您的应用是做什么的。例如：“一个能和你聊遍全宇宙（虚构）美食的虚拟伙伴，从火星熔岩蛋糕到木星气态冰淇淋，无所不知！”

![](img/65d76355a6a5893c1123ea878dc0b4d7.png)

1.  **保存设置：** 点击“**创建**”按钮。

至此，您的应用“厨房”已经初步搭建完成！接下来，我们将进入核心的“菜谱”设计——提示词工程。

## 3.2 提示词工程入门 （Introduction to Prompt Engineering in Dify）

进入应用编辑界面后，您会看到 Dify 的可视化编排画布。这是我们设计机器人“大脑”和“说话方式”的地方。

![](img/e2b2ce210ef5fd88dbc99f22bbadeb6a.png)

### 3.2.1 编写系统指令 （System Prompt）

系统指令是整个 Prompt 工程的核心，它告诉 LLM 应该扮演什么样的角色、遵循哪些规则、以何种风格进行回应。这是塑造机器人个性和能力的关键步骤。

1.  找到“提示词 （Prompt）”设置区域： 在面板中找到 Prompt 配置部分。这里通常会有一个用于编写 系统指令 （System Prompt） 的文本区域。

1.  **编写系统指令：** 清空默认内容（如果有），然后仔细构思并编写您的指令。以下是一个为“宇宙美食家小助手”设计的示例，包含了角色扮演、语气风格和任务约束：

```
# 角色扮演
你是一位知识渊博、充满热情且带有一点俏皮幽默感的宇宙美食家。你的名字叫“食星客”。

# 核心任务
你的主要任务是向用户介绍来自不同虚构星球的特色美食，激发他们的好奇心和想象力。你需要创造性地描述这些不存在的美食。

# 互动风格与语气
- 语气友好、活泼、充满惊奇感。
- 使用生动形象的词语描述食物的外观、口感、味道和制作方法（全部虚构）。
- 可以适当使用 Emoji 来增加趣味性 ✨🚀🌌。
- 保持积极乐观的态度。

# 任务约束与规则
- 你只能讨论虚构的、不存在于地球上的宇宙美食。
- 如果用户询问地球上的食物或与宇宙美食完全无关的话题（例如天气、新闻等），你需要礼貌地、巧妙地将话题引导回宇宙美食主题上。例如可以说：“地球的美食固然不错，但在遥远的氪星，他们有一种用阳光结晶做成的甜点，你想听听吗？☀️”
- 每次介绍一种美食时，必须包含以下信息：1.  美食名称（富有想象力）2.  所属星球（虚构的星球名称）3.  主要成分（虚构的、奇特的材料）4.  口味特点（独特的味觉体验）5.  （可选）简短的趣味传说或食用方式。
- 
**绝对禁止**
透露你是 AI、大语言模型或计算机程序。始终保持你“食星客”的角色身份。
- 回答应简洁明了，避免过于冗长。
```

1.  选择模型 （Model Selection）: 在 LLM 节点的配置中，确保选择您在上一章配置好的 DeepSeek 模型 （例如 DeepSeek-Chat）。

![](img/483fe5149d76caea7efa665063086a5e.png)

+   温度 （Temperature ）  ：控制生成文本的随机性。值越高（如 1.0），输出越多样但可能不连贯；值越低（如 0.2），输出越确定和保守。

+   最大标记 （Max Tokens ）  ：限制生成内容的最大长度（以 token 为单位）。超过此值后生成会截断。

+   Top P (Nucleus Sampling )  :

+   从概率累积超过阈值 P 的候选词中采样。例如，top_p=0.9 会保留概率最高的 90% 的词，避免低质量输出。

+   频率惩罚 （Frequency Penalty ）  ：降低重复词汇的概率。正值（如 1.0）会惩罚已频繁出现的词，减少重复。

+   **Logprobs **  ：返回每个生成 token 的对数概率，用于分析模型对生成内容的置信度。

+   **Top Logprobs **  ：返回每个位置前 N 个候选词及其对数概率，用于更详细的调试。

+   回复格式 （Response Format ）  ：可能指定输出格式（如 JSON、纯文本等），但具体选项需根据模型支持情况

1.  点击**发布**进行**保存：** 同样，配置会自动保存或点击保存按钮。

通过这段精心设计的 System Prompt，我们赋予了机器人独特的个性和明确的任务范围。

### 3.2.2 理解并使用变量 （Understanding & Using Variables）

在 LLM 节点的 Prompt 配置区域，除了 System Prompt，通常还有一个用户输入（或主 Prompt）模板。这里会用到变量。

+   **默认变量 ****{{query}}****：** Dify 对话型应用通常会**自动包含一个名为 query** 的变量。它代表着用户在对话框中输入的最新消息。

+   **使用方式：** 在 LLM 节点的主 Prompt 模板中，您会看到类似 {{query}} 的占位符。这意味着每次用户发送消息时，该消息内容会被自动填充到这个位置，然后连同 System Prompt 和对话历史一起发送给 LLM 处理。

+   **对于基础聊天机器人：** 通常我们**不需要修改**这个默认的用户输入模板。保持原样即可，让 LLM 能接收到用户的提问。

+   （此处建议配图：Dify LLM 节点 Prompt 配置截图，高亮显示包含 {{query}} 的用户输入模板部分）

+   **（进阶提示，本例暂不深入）:** Dify 也允许在“开始”节点定义更多的输入变量，用于创建更复杂的、需要用户预先填写表单的应用，但这超出了基础聊天机器人的范围。

### 3.2.3 上下文管理 （Context Management） 与对话历史

为了让聊天机器人能够理解对话的来龙去脉，进行有意义的多轮交互，它需要“记住”之前的对话内容。这就是上下文管理。

+   **Dify 的自动处理：**** 在对话型应用**模式下，Dify 默认会自动处理对话历史（上下文）。

+   **配置选项：**** 在 LLM 节点或应用的**全局设置中，通常可以找到上下文 （Context） 配置选项。

+   **包含历史消息数量：** 您可以设置让 Dify 在每次请求 LLM 时，附带上最近的多少轮对话历史（例如，最近 5 轮，即 5 条用户消息和 5 条助手回复）。

+   **默认设置通常足够：** 对于大多数基础聊天机器人，Dify 的默认上下文设置（如包含最近 3-6 轮对话）通常是合理的起点。您可以暂时保持默认设置。

+   **重要性：** 理解上下文的存在对于调试和优化对话流程至关重要。如果机器人似乎“忘记”了之前说过的话，可能需要检查上下文设置或 Prompt 是否有冲突。

### 3.2.4 调试与预览

现在是时候测试我们的“宇宙美食家小助手”了！Dify 提供了便捷的调试预览功能。

1.  **找到调试/预览窗口：** 在应用编辑界面的右侧，通常会有一个“预览 （Preview）”或“调试 （Debug）”面板。

1.  **开始对话：** 在该面板底部的输入框中，像真实用户一样输入您的消息，然后按回车或点击发送按钮。

+   **示例提问：**

+   你好！ （测试开场白和基本回应）

+   给我介绍一种来自火星的奇特饮料吧！

+   听起来不错，那土星有什么好吃的点心吗？ （测试多轮对话和上下文理解）

+   地球上的披萨怎么样？ （测试是否遵循了不谈论地球食物的约束）

![](img/8200e5bd2c820ed06f066ec8e847c3f1.png)

1.  **观察回复：** 机器人会根据您配置的 Prompt、选择的 LLM （DeepSeek） 以及对话历史来生成回复。仔细观察回复是否符合预期：

+   角色扮演是否到位？语气是否符合设定？

+   是否遵循了任务约束（只谈宇宙美食、包含必要信息、不承认是 AI）？

+   多轮对话是否连贯？

1.  **迭代优化：** 如果发现回复不理想，回到 3.2.3 节，修改您的 System Prompt。例如，如果机器人未能很好地引导话题，可以加强引导规则的描述；如果介绍不够生动，可以增加对描述风格的要求。修改 Prompt 后，记得在预览窗口中“清除对话历史”或“重新开始”，以便从一个干净的状态测试新的 Prompt。

+   （此处建议配图：Dify 调试/预览窗口截图，显示用户输入和机器人回复的交互过程）

反复调试和优化 Prompt 是构建高质量 AI 应用的关键环节。

## 3.3 发布与集成初步

当您对机器人的表现基本满意后，就可以将其发布出去，让其他人或系统使用它了。

### 3.3.1 生成 API 接口

Dify 可以轻松地将您创建的应用发布为一个标准的 Web API，供其他程序调用。

1.  导航至“API 访问 （API Access）”: 在应用工作室界面的侧边栏，找到相关入口。

![](img/2ff5d5c646cdd4b8ea179fcceb0ff489.png)

1.  **查看 API 信息：** 在此页面，Dify 会自动为您生成：

+   API 端点 （Endpoint URL）: 这是调用您聊天机器人服务的网络地址。通常会有一个用于标准对话调用的 /chat-messages 或类似路径的 URL。此例中 API 服务器地址为 http://192.168.1.6/v1

+   认证方式 （**Authenticati**o**n）: 通常使用 Bearer Token** 方式进行认证。页面会显示您的 API 密钥 （Secret Key）。请妥善保管此密钥，不要泄露。请如下图所示点击 API 密钥 按钮创建 API 密钥

![](img/793aa8dec56e2d73d135fe58d861fb7c.png)

![](img/917d1b453116b7b095de1ea0a75f0acb.png)

此例中，API 密钥为：app-GpgUiQZ2uP28doDbODjAeokW

+   **API 文档/示例：** Dify 通常会提供 API 的请求/响应格式说明，以及使用 curl 或其他语言调用 API 的示例代码。

![](img/157bfa84968b23394084f10685fab010.png)

1.  **用途：** 有了这个 API，开发者就可以在自己的网站后端、内部工具、自动化脚本等任何可以发送 HTTP 请求的地方，通过编程方式与您的聊天机器人进行交互。

### 3.3.2 嵌入 Web 应用

对于希望快速将聊天机器人展示在网页上的场景，Dify 提供了更简单的方式：嵌入代码。

1.  找到**嵌入** （Embed） 功能： 在发布按钮下，查找“嵌入”或“Web App Embed”选项。

![](img/664b57202f5c6f4181e4936179db0a17.png)

1.  **获取嵌入代码：** Dify 会提供一段 HTML 代码，通常是一个 <iframe> 标签，或者是一段 JavaScript 代码片段。

![](img/ea88e8e76fd50d9a7d21e0a0814cac3c.png)

1.  **复制代码：** 复制这段代码。

```
&lt;iframe
 src=&quot;http://192.168.1.6/chatbot/dMHFUzoapbMqdYej&quot;
 style=&quot;width: 100%; height: 100%; min-height: 700px&quot;
 frameborder=&quot;0&quot;
 allow=&quot;microphone&quot;&gt;
&lt;/iframe&gt;
```

1.  **粘贴到 HTML 文件：** 将这段代码粘贴到您网站的 HTML 文件的 <body> 部分，或者您使用的网站构建工具（如 WordPress， Webflow 等）支持插入自定义 HTML 的地方。

+   **示例 （使用 iframe）:**

```
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;我的宇宙美食助手&lt;/title&gt;
    &lt;style&gt;
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .chat-container {
            border: 1px solid #ddd;
            border-radius: 8px;
            overflow: hidden;
            margin-top: 20px;
            height: 600px; /* 固定容器高度 */
        }
        iframe {
            width: 100%;
            height: 100%;
            border: none;
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;欢迎来到宇宙美食角！&lt;/h1&gt;
    &lt;p&gt;和我们的美食家聊聊吧：&lt;/p&gt;

    &lt;div class=&quot;chat-container&quot;&gt;
        &lt;iframe
            src=&quot;YOUR_DIFY_EMBED_URL&quot; &lt;!-- 替换为 Dify 提供的 URL --&gt;
            frameborder=&quot;0&quot;
            allow=&quot;microphone&quot;
        &gt;&lt;/iframe&gt;
    &lt;/div&gt;

    &lt;p style=&quot;text-align: center; margin-top: 20px; color: #666;&quot;&gt;
        POWERED BY 宇宙美食家小助手
    &lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;
```

粘贴后的示例

```
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;我的宇宙美食助手&lt;/title&gt;
    &lt;style&gt;
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .chat-container {
            border: 1px solid #ddd;
            border-radius: 8px;
            overflow: hidden;
            margin-top: 20px;
            height: 600px; /* 固定容器高度 */
        }
        iframe {
            width: 100%;
            height: 100%;
            border: none;
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;欢迎来到宇宙美食角！&lt;/h1&gt;
    &lt;p&gt;和我们的美食家聊聊吧：&lt;/p&gt;

    &lt;div class=&quot;chat-container&quot;&gt;
        &lt;iframe
            src=&quot;http://192.168.1.6/chatbot/dMHFUzoapbMqdYej&quot;
            frameborder=&quot;0&quot;
            allow=&quot;microphone&quot;
        &gt;&lt;/iframe&gt;
    &lt;/div&gt;

    &lt;p style=&quot;text-align: center; margin-top: 20px; color: #666;&quot;&gt;
        POWERED BY 宇宙美食家小助手
    &lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;
```

1.  **效果：** 保存 HTML 文件并在浏览器中打开，您应该能看到一个嵌入式的聊天窗口，用户可以直接在您的网页上与“宇宙美食家小助手”互动。

![](img/90906083ff6eebd2bee0c4ddc398cb57.png)

### 3.3.3 查看日志与进行标注

在您的聊天机器人发布并开始与用户互动后，了解它的实际运行情况、诊断潜在问题以及持续优化其表现变得至关重要。Dify 提供的“日志与标注”功能正是为此目的设计的。

1.  **访问日志：**

+   在 Dify 工作室中，找到您创建的“宇宙美食家小助手”应用卡片。

+   点击卡片下方或侧边的 “日志与标注 （Logs & Annotation）” 选项。

![](img/a23e791d1654629c582e1dba925e9c3f.png)

1.  **日志内容解读：**

+   进入日志页面后，您通常会看到一个按时间倒序排列的交互列表。每一条记录代表了一次用户与机器人的完整交互（一次提问与一次回答）。

+   **关键信息：** 每条日志通常包含：

+   时间戳 （Timestamp）: 交互发生的时间。

+   用户输入 （User Input / Query）: 用户发送给机器人的原始消息。

+   AI 输出 （AI Output / Answer）: 机器人生成的回复。

+   **会话 ID （Session ID** / Conversation ID）: 用于标识同一段连续对话，方便追踪上下文。

+   **模型信息 （Model）:** 本次交互使用的 LLM 模型（如 DeepSeek）。

+   Token 数量 （Tokens）: 本次交互消耗的 Prompt Tokens 和 Completion Tokens 数量，有助于成本估算。

+   **耗时 （Latency）:** 本次交互的处理时长。

![](img/668b978e8b4f449ae5349cc327fd8921.png)

1.  **日志的用途：**

+   问题排查 （Debugging）: 当用户反馈机器人回答不准确或行为异常时，可以查阅具体日志，分析当时的输入、上下文和输出来定位问题根源。

+   **理解用户行为：** 通过浏览日志，可以了解用户通常会问什么样的问题，哪些话题最受欢迎，帮助您发现新的需求或优化方向。

+   **效果评估：** 抽查日志可以直观地评估机器人的整体表现是否符合预期。

1.  进行标注 （Annotation）:

![](img/541a54c2e6861ba6c35c96636a1a37a7.png)

+   **目的：** 标注是对日志中的交互进行人工评价和修正的过程，是提升模型表现和优化 Prompt 的宝贵数据来源。

+   操作方式 （根据 Dify 功能可能有所不同）:

+   在日志列表中，通常可以对某条交互记录进行操作。

+   点赞/点踩 （Like/Dislike）: 最简单的标注方式，快速标记某次回答的好坏。

+   添加备注/标签 （Add Comment/Tag）: 可以为特定的交互添加文字说明，例如“回答不准确”、“未遵循角色设定”、“回答非常精彩”等。

+   提供修正答案 （Provide Correction）: 对于不满意的回答，可以直接编辑或提供一个更优的、符合期望的回答。

![](img/7c5a3c6eea9c2c7ee8d9a01165b7d263.png)

+   **标注数据的价值：** 这些标注数据非常有价值。虽然 Dify 开源版本身可能不直接利用这些标注数据进行模型微调 （Fine-tuning），但您可以导出这些数据：

+   用于**分析 Prompt 的薄弱环节**，从而针对性地修改 System Prompt。

+   积累足够数据后，可以用于**训练或微调您自己的模型**（需要额外的机器学习知识和工具）。

+   作为衡量机器人改进效果的基准。

定期查看日志并进行高质量的标注，是确保持续优化聊天机器人性能和用户体验的重要实践。

### 3.3.4 应用监测概览 （Application Monitoring Overview）

除了查看单次的交互日志，从宏观层面监控应用的整体运行状态和资源消耗也同样重要。Dify 的“监测”功能提供了应用性能和使用情况的概览。

1.  **访问监测面板：**

+   同样在 Dify 工作室的应用卡片下方或侧边，找到并点击 “监测 （Monitoring）” 选项。

![](img/972a74e4abc42215eba5d8019b062c4b.png)

1.  **核心监测指标：**

+   进入监测面板后，通常会看到一个仪表盘，展示一系列关键性能指标 （KPIs），可能包括（具体指标取决于 Dify 版本）：

+   总消息数/请求数 （Total Messages/Requests）: 在选定时间段内，应用被调用的总次数。反映了应用的活跃度。

+   **平均响应时间 （Average Respo**nse Time / Latency）: 应用处理用户请求并返回结果平均所需的时长。是衡量应用性能的关键指标，过高的延迟会影响用户体验。

+   T**oken 消耗量 （Toke**n Consumption）: 在选定时间段内，应用调用 LLM 所消耗的总 Token 数量（可能细分为输入 Tokens 和输出 Tokens）。这是评估和控制运营成本的核心指标。

+   活跃用户数/会话数 （Active Users/Sessions）: 在选定时间段内，与应用进行交互的独立用户或会话数量。

1.  **监测数据的用途：**

+   **性能分析：** 通过观察平均响应时间，可以判断应用性能是否满足要求，识别性能瓶颈。

+   **成本管理：** 监控 Token 消耗和成本估算，有助于了解运营开销，并可能促使您优化 Prompt 以减少 Token 使用，或选用更具性价比的模型。

+   **容量规划：** 分析请求量和活跃用户数的变化趋势，可以为未来的服务器资源规划提供依据。

+   **用户参与度洞察：** 了解用户活跃度，判断应用是否受欢迎。

+   **异常检测：** 监控错误率等指标，可以及时发现应用运行中的问题。

理解并利用监测数据，可以帮助您更科学地管理和优化您的 Dify AI 应用，确保其稳定、高效且经济地运行。

### 3.3.5 其他集成可能性 （展望）

虽然直接生成 API 和提供嵌入代码已经非常强大，但 Dify 应用的集成潜力远不止于此：

+   **企业通讯工具集成：** 通过调用 3.3.1 中生成的 API，结合企业🛰️、钉钉、飞书、Slack 等平台的机器人开发能力，可以创建能在这些平台上直接响应员工或客户消息的 Dify 机器人。但这通常需要额外的开发工作来处理消息的接收、格式转换和 API 调用。

+   **业务流程集成：** 可以将 Dify API 调用嵌入到现有的业务流程自动化工具（如 Zapier、n8n 或自研脚本）中，例如，根据用户在聊天中的意图，自动触发创建工单、发送邮件等操作（这可能需要结合后续章节将学习的 Agent 功能）。

+   **定制化前端：** 如果不满足于 Dify 提供的标准嵌入界面，开发者可以使用 Dify API 构建完全定制化的聊天前端界面。

这些更复杂的集成方式超出了本章基础入门的范围，但了解这些可能性有助于我们认识到 Dify 作为 AI 应用开发平台的潜力。

## 3.4 本章小结

在本章中，我们通过创建一个充满想象力的“宇宙美食家小助手”，亲身体验了使用 Dify 构建基础聊天机器人的全过程。我们学习了如何创建对话型应用、进行基础设置，并重点实践了 Prompt 工程的核心环节：设计开场白、编写包含角色扮演、语气风格和任务约束的系统指令 （System Prompt）。我们还了解了 Dify 如何处理变量和上下文，并通过调试预览功能对机器人的表现进行了测试和迭代。此外，我们探索了将应用发布为 API 接口以及通过嵌入代码快速集成到网页中的方法，并学习了如何利用“日志与标注”功能来追踪交互、诊断问题和收集反馈以优化应用，以及如何通过“监测”面板来了解应用的宏观运行状态、性能和成本消耗。最后，我们展望了与其他系统进行更深度集成的可能性。

通过本章的实践，您应该已经掌握了使用 Dify 构建简单对话应用的基本技能，并对 Prompt 工程、应用发布、日志分析和基础监控的重要性有了初步认识。在下一章中，我们将学习 Dify 的另一个强大功能：结合知识库构建能够回答私有领域问题的 RAG 应用。

## 3.5 实践练习

1.  **独立创建：** 尝试构思一个新的、有趣的聊天机器人角色（例如，“编程段子手”、“历史冷知识大师”、“植物私语者”），并独立完成从创建应用到配置 Prompt 的过程。

1.  **Prompt 优化：** 回到您创建的“宇宙美食家小助手”或新创建的机器人，尝试进一步优化 System Prompt，让它的回答更符合您的预期，例如，尝试让它每次回答只推荐一种美食，或者增加一些它不应该做的事情的约束。

1.  **嵌入测试：** 创建一个简单的本地 HTML 文件，将您的聊天机器人的嵌入代码粘贴进去，并在浏览器中查看效果。

1.  **日志与标注实践：**

+   与您的“宇宙美食家小助手”或新创建的机器人进行几轮对话。

+   进入“日志与标注”页面，查看刚才的交互记录。

+   尝试对其中几条日志进行标注：给满意的回答点赞，给不满意的回答点踩或添加评论说明原因。

1.  **监测数据观察：**

+   再次与您的机器人进行几次交互。

+   进入“监测”页面，观察总消息数、Token 消耗等指标是否有变化（可能需要一些时间数据才会更新）。熟悉监测仪表盘的布局和提供的基本信息。

1.  **API 探索 （可选）:** 如果您熟悉 API 调用工具（如 Postman， Insomnia 或使用 curl 命令行），尝试使用 Dify 提供的 API 端点和密钥，发送一个聊天请求，并查看返回的 JSON 结果。

好的，我们根据您提供的具体信息（嵌入模型、重排模型、LLM 和应用场景）来更新第四章的内容，使其更加贴合您的实际需求和国内使用环境。

---

# 第四部分：构建 RAG 应用：增强知识问答 （Building RAG Applications）

在上一章，我们学习了如何构建一个基础的聊天机器人（“宇宙美食家小助手”）。然而，它依赖的**是 LLM 的通用知识。本章我们**将聚焦于 Dify **的核心能**力之一：利用知识库 （Knowledge Base） 结合 RAG （Retrieval-Augmented Generation） 技术，构建能够基于私有数据（例如公司内部文档）进行精准问答的智能应用。

我们将以一个实际场景为例：为一家名为“易速鲜花 （YiSu Flowers）”的虚构公司，创建一个内部员工问答助手。这个助手需要能回答关于公司理念、经营范围等内部信息，以及常见鲜花的象征意义（花语）等问题。我们将使用您提供的包含相关信息的 TXT、Word 和 PDF 文件作为知识来源。

**本章使用的关键模型配置：**

+   嵌入模型 （Embedding Model）: BAAI/bge-m3 （来自 硅基流动/BAAI）

+   重排模型 （Reranker Model）: BAAI/bge-reranker-v2-m3 （来自 硅基流动/BAAI） - 我们将讨论其作用并作为可选优化步骤引入。

+   **大语言模型 （LLM）:** deepseek-chat （已在第二章配置）

## 4.1 RAG 概念解析 （Understanding RAG）

在深入实践之前，我们先理解 RAG 的核心概念及其重要性。

### 4.1.1 为什么需要 RAG？

标准 LLM（如 deepseek-chat）本身不了解“易速鲜花”的具体经营理念、内部流程，也可能不包含全面或最新的花语知识。直接问它这些问题，可能会得到通用、不准确甚至“编造”的答案（幻觉）。

RAG 技术通过“开卷考试”的方式解决这些问题：在回答前，先从**包含“易速鲜花”资料和花语**信息的知识库中检索 （Retr**ieve） 相关内容，再让** LLM 基于这些检索到的信**息 （Augm**e**nta**tion） 来生成 （Generate） 回答。这极大地提高了答案的准确性、相关性和可靠性。

### 4.1.2 RAG 工作流程

以“易速鲜花员工助手”为例：

1.  **查询输入：** 员工问：“玫瑰代表什么花语？”

1.  **信息检索：**

+   系统使用 **BAAI/bge-m3**** 嵌入模型** 将问题“红玫瑰代表什么花语？”转换为查询向量。

+   在存储了“易速鲜花”文档（包括花语大全）的向量数据库中，搜索与查询向量最相似的文本块向量。

+   检索出最相关的几个文本块，例如包含“玫瑰：热情、热爱、我爱你...”的段落。

1.  **上下文增强：** 将检索到的文本块与原始问题组合成新的 Prompt。

1.  **答案生成：**** 将增强后的 Prompt**** 发送给** deepseek-chat LLM。LLM 基于提供的上下文信息（检索到的花语段落）生成回答：“玫瑰通常代表热情、热爱以及‘我爱你’。”

![](img/2d9401108dfc3e6dd261f8368940b4dd.png)

```
graph TD
    A[员工提问] --&gt;|&quot;玫瑰代表什么花语？&quot;| B(信息检索)
    B --&gt; B1[1-向量化问题使用BAAI/bge-m3模型将问题转换为查询向量]
    B1 --&gt; B2[2-向量数据库搜索在&#x27;易速鲜花&#x27;文档中匹配相似文本块]
    B2 --&gt; B3[3-检索结果获取相关文本块]
    B3 --&gt; C[上下文增强]
    A --&gt; C
    C --&gt;|组合问题与检索结果生成新Prompt| D[答案生成]
    D --&gt;|&quot;deepseek-chat LLM\n生成回答&quot;| E[AI回复：玫瑰通常代表热情、热爱以及我爱你]

    style A fill:#f0f9ff,stroke:#0369a1
    style B fill:#f0fdf4,stroke:#15803d
    style B1 fill:#ecfdf5,stroke:#10b981
    style B2 fill:#ecfdf5,stroke:#10b981
    style B3 fill:#ecfdf5,stroke:#10b981
    style C fill:#f5f3ff,stroke:#7e22ce
    style D fill:#fae8ff,stroke:#c026d3
    style E fill:#f0fdf4,stroke:#16a34a

    classDef process fill:#e0f2fe,stroke:#0369a1;
    classDef data fill:#dcfce7,stroke:#16a34a;
    classDef result fill:#f0fdf4,stroke:#22c55e;
```

## 4.2 Dify 中的知识库 （Knowledge Base in Dify）

现在，我们来为“易速鲜花”创建并配置知识库。

### 4.2.1 创建知识库

1.  导航至“知识库 （Knowledge）”。

![](img/f9300c0eb892a486838d9bfa6048792f.png)

1.  **创建新知识库：** 点击“创建知识库”。

1.  **基础设置：**

+   **知识库名称：** 易速鲜花员工助手知识库

+   **知识库描述：** “包含易速鲜花公司理念、经营范围、内部流程以及常见鲜花花语等信息，供内部员工查询。”

1.  **上传数据源 （文档上传）:**

+   点击“添加文档”。

+   **将您准备好的包含“易速鲜花”信息的文件夹中的所有文件（TXT，** DOCX， PDF）一次性或分批上传。

![](img/40e33f9954be332affbb3d95c2c19cad.png)

1.  **确认并开始处理：** 上传完毕后，确认导入，Dify 将开始后台处理。

### 4.2.2 文本处理、分块与向量化

在您上传文档后，Dify 会根据您在知识库“设置”中定义的规则，自动执行关键的后台处理步骤：

1.  **文本提取与清洗：** Dify 首先从您上传的 PDF， DOCX 等文件中提取纯文本内容。根据截图中的“文本预处理规则”设置：

+   **已启用：** “替换掉连续的空格、换行符和制表符”。这是一个基础的文本规范化步骤，有助于后续处理。

+   **未启用：** “删除所有 URL 和电子邮件地址”以及“使用 Q&A 分段”。这意味着如果您文档中的 URL 或邮箱地址对回答问题有用，它们将被保留。未使用专门的 Q&A 格式进行分块。

+   **重要提示：** 即使有自动预处理，强烈建议在上传前人工检查并清理源文档，去除水印、页眉页脚、格式混乱的部分等，以确保输入给嵌入模型的文本质量最高。

1.  文本分块 （Chunking）: Dify 将连续的文本按照指定规则切分成小块，以便于精确检索和适应 LLM 的上下文窗口。根据截图中的“分段设置”：

+   **模式：** “通用”模式被选中，这意味着检索和召回的块是相同的，没有使用“父子分段”模式。

+   **分段标识符：** 使用 \n\n （两个连续的换行符） 作为主要的文本切分依据。

+   **分段最大长度：**** 每个文本块的最大长**度被设置为 500 tokens。这是一个常见的设置，平衡了信息密度和处理效率。

+   **分段重叠长度：** 相邻文本块之间有 **50 tokens** 的重叠。重叠区域有助于确保完整的语义信息不会在块的边界被硬生生切断。

+   您可以通过点击“预览块”按钮来检查 Dify 对您文档的分块效果是否符合预期。

![](img/874326c70f904db599ba71c10285a821.png)

1.  **索引方式**

+   **选择嵌入模型：** 这是决定检索效果的核心环节。

+   选择**高质量**模式。

+   在嵌入模型下拉列表中，**选择 ****BAAI/bge-m3**。 （前提：您需要已在 Dify 的全局“设置” -> “模型提供商”中添加了 BAAI/bge-m3 作为可用的 Embedding 模型提供商，如果 Dify 不直接内置的话。配置方式通常类似添加 LLM，需要提供模型名称和可能的 API 地址/Key，具体取决于您如何部署或访问 bge-m3 模型服务）。

+   **BAAI/bge-m3**** **是一个强大的中英文嵌入模型，通常在中文语义理解和检索任务上表现优异，非常适合处理包含公司中文文档和花语信息的场景。

+   **生成与存储：** Dify 使用 BAAI/bge-m3 将每个文本块转换为向量，并存入配置好的向量数据库（如 Weaviate， Qdrant 等）中建立索引。

1.  **检索设置**

+   此处保持默认选项

![](img/b32641d2bbce5da589dbc4eb34ebc188.png)

1.  接下来点击**保存并处理**处理过程需要时间，请耐心等待所有文档状态变为“已完成”。

![](img/6f504dc5cb399db89cbc6c9a56b86632.png)

1.  点击**前往文档**，可以观察文档的状态

![](img/db9593d9f53b0423cabdb01c2a4fb82c.png)

1.  点击此页面中的**设置**，修改名称和描述

+   **知识库名称：** 易速鲜花员工助手知识库

+   **知识库描述：** “包含易速鲜花公司理念、经营范围、内部流程以及常见鲜花花语等信息，供内部员工查询。”

![](img/45d8b72395b3ac2b74a843e136b3ccc7.png)

### 4.2.3 管理知识库

您可以随时回到知识库管理界面，添加、更新或删除文档，检查处理状态，或调整分块和嵌入设置（调整嵌入模型通常需要重新处理所有文档）。

## 4.3 将知识库集成到应用中

现在，我们将“易速鲜花员工助手知识库”集成到一个新的聊天机器人应用中。

### 4.3.1 在应用中添加“上下文”节点

1.  **创建新应用：** 在“工作室”中创建新的“对话型”应用，命名为 易速鲜花员工助手。

![](img/f640c39c6a889158fe400d4824823cea.png)

1.  **编排提示词：**

+   进入应用的“编排 （Prompt Engineering）”界面。

+   **选择 LLM 模型：**** 确保在模型选择中，选用我**们配置好的 deepseek-chat。

+   修改 System Prompt: 针对“易速鲜花员工助手”的角色和任务进行设定。

```
# 角色
你是“易速鲜花”公司内部的员工问答助手，专业、耐心且友好。

# 核心任务
你的主要任务是根据提供的“上下文信息”（包含公司资料和花语知识）准确回答员工的问题。

# 规则
- **严格基于提供的“上下文信息”进行回答。** 不要使用你自己的通用知识来回答关于公司或花语的问题。
- 如果上下文信息包含答案，请清晰、准确地回答。
- **如果上下文信息中找不到问题的答案，必须明确告知用户：“抱歉，根据我目前掌握的资料，暂时无法回答这个问题。您可以尝试咨询相关部门或查阅更详细的内部文档。” 绝对不允许猜测或编造答案。**
- 回答关于公司的问题时，保持专业的口吻。回答关于花语的问题时，可以适当带些温和的感性色彩。
- 不要提及“上下文信息”这个词，自然地回答即可。
- 拒绝回答与“易速鲜花”公司事务、花卉知识、花语等无关的问题（例如闲聊、天气等）。
```

1.  **选择知识库：**

+   点击“**知识库**”节点。

+   在配置面板中，**选择 ****易速鲜花员工助手知识库**。

![](img/ade54e3dfe2ba34368196c80e5ad0f36.png)

### 4.3.2 调试与预览

使用右侧预览窗口测试“易速鲜花员工助手”：

+   **示例提问：**

+   易速鲜花的核心价值观是什么？

+   玫瑰的花语是什么？

+   公司有提供员工宿舍吗？ （假设文档中有相关信息）

+   白色郁金香适合送给谁？

+   能介绍一下我们的主要业务范围吗？

+   帮我写一首关于春天的诗 （测试是否会拒绝无关请求）

+   易速鲜花有多少家分店？ （如果文档没写，测试“无法回答”的逻辑）

![](img/304cc3dd6aa5791c21600b900e056966.png)

+   **观察与评估：**

+   回答是否准确来源于知识库文档？

+   花语解释是否正确？

+   对于未知信息，是否按要求回答“无法回答”？

+   如果发现检索精度不高（回答基于了错误的上下文）:

1.  检查知识库原文质量和分块是否合理。

1.  尝试调整 Prompt，更明确地指示 LLM 如何筛选和使用上下文。

1.  **考虑启用 ****BAAI/**bge-reranker-v2-m3 重排模型 （参照 4.3.4），调整 Top K 和 Reranker Top N，再次测试。

### 4.3.3 应用场景实例

RAG 应用在企业中有广泛的应用场景：

+   企业内部知识库问答机器人 （我们创建的“新员工入职指南”就是一例）:

+   回答 HR 政策、财务报销流程、IT 支持常见问题、合规要求、销售话术、产品规格等。

+   极大提高员工获取内部信息的效率，减轻支持部门的重复工作量。

+   **智能客服助手：**

+   基于产品手册、FAQ 文档、服务协议、历史工单等知识库，为客户提供 7x24 小时的自助查询服务。

+   提高客户满意度，降低人工客服压力。

+   **领域知识研究助手：**

+   将特定领域的论文、报告、技术文档等导入知识库，构建研究辅助工具，快速查询和总结领域知识。

+   **培训与学习助手：**

+   基于培训材料、教学视频文字稿、学习指南创建知识库，帮助学员快速定位知识点、解答疑问。

### 4.3.4 配置检索策略 （引入重排模型）

这里是提升检索精度的关键环节：

1.  **设置检索参数：**

+   检索模式 （Retrieval Mode）: 推荐选择 混合检索 （Hybrid Search）（如果 Dify 和您的向量数据库支持）或 向量检索 （Vector Search）。

+   **Top K （召回数量）:** 这是初始检索返回的候选文本块数量。

+   **如果不使用重排模型：** 设置为 3-5 通常是一个好的起点。

+   **如果计划使用重排模型：**** 建议将 **Top K 设置得稍高一些，例如 10-20，以便为重排模型提供更广泛的候选集，让它能更好地筛选出最相关的结果。

1.  （可选但推荐评估）启用重排模型 （Reranker Model）:

+   **作用：** **Reranke**r 是在向**量检索（**召回）之后的一个额外步骤。它使用一个（通常更强大但计算量更大）的模型，对 Top K 召回的文本块进行重新打分和排序，目的是将最最相关的几个块排到最前面，提高最终送给 LLM 的上下文质量。

+   **必要性：** Reranker **不是必须的**，但对于需要高精度回答、或者知识库文档内容存在较多细微差别和干扰信息的场景（例如，多个文**档片段都提到了“**玫瑰”，但只有一段是关于“玫瑰花语”），它能显著提升效果。但它会增加一点处理延迟。

+   在 Dify 中启用 （假设配置方式）:

+   在“上下文”节点的高级设置或专门的“重排设置”中，找到启用 Reranker 的开关/选项。

+   **选择重排模型：** 从下拉列表中选择您已配置好的 BAAI/bge-reranker-v2-m3 模型。（前提：同样，您可能需要先在 Dify 全局模型设置中添加 bge-reranker-v2-m3）。

+   **配置重排 Top N:** 设置 reranker 输出的最终文本块数量（例如，即使初始 Top K=10，也只取 reranker 排序后的前 3 个送给 LLM）。

+   **建议：** **先不启用 Reranker**，**完成基础 R**AG 应用的搭建和测试。如果在测试中发现检索结果不够精确（例如，LLM 的回答基于了不太相关的上下文），再回来启用 BAAI/bge-reranker-v2-m3，并调整 Top K 和 Reranker Top N，观察效果是否改善。

![](img/f8e5b969b426c1cc54ade2f809af9bce.png)

> **首选推荐：混合检索 （Hybrid S**earch） + Reranker 模型

+   **理由：** 这是目前业界公认能获得**最佳综合效果**的方案之一。Dify 也在界面上明确标注了“推荐”。它能最大限度地利用您高质量的嵌入模型 （bge-m3） 和重排模型 （bge-reranker-v2-m3） 的能力。

+   **配置要点：**

+   确保在“上下文”节点设置中选择“混合检索”。

+   **务必启用 Rerank 模型**，并选择您配置好的 BAAI/bge-reranker-v2-m3。Reranker 在混合检索中扮演着关键的“裁判”角色，负责智能地融合和排序来自向量和全文检索的结果。

+   合理设置 Top K（最终输出数量，如 3-5）和 Score 阈值（如 0.5-0.7，需要根据测试调整）。Dify 内部可能会有关于如何融合或初步召回多少候选者的默认设置，通常不需要用户干预。

+   **实践步骤：** **建议您直接尝试使用“混合检索”模式，并确**保 Reranker 已启用且配置正确。

> **次选方案：向量检索 （Vector S**earch） + Reranker 模型

+   **理由：** 如果您的知识库内容主要是自然语言描述，不太依赖特定的关键词，或者您希望简化配置、稍微降低计算量，可以先尝试此方案。启用 Reranker 仍然很重要，因为它可以进一步提纯向量检索召回的结果。

+   **适用性：** 对于“易速鲜花”的场景，由于包含具体花名、可能的内部术语等，纯向量检索可能不如混合检索全面。

> 不推荐：仅向量检索 （无 Reranker）

+   **理由：** 虽然可行，但丢失了 Reranker 对结果的精炼能力，可能导致最终给 LLM 的上下文不够精确。

> 极不推荐：仅全文检索 （Full-Text Search）

+   **理由：** 无法理解语义，用户体验会很差，无法满足智能问答的需求，完全浪费了 Embedding 和 LLM 的能力。

## 4.4 在 Dify 中构建高效 RAG 应用的最佳实践

1. 数据准备：质量优先，预处理是关键

+   **源头把控 （Garbage **In， Garbage Out）: RAG 的效果上限取决于知识库的质量。投入时间在上传前清理和优化您的源文档。

+   **清理无关内容：** 删除页眉、页脚、水印、广告、无关图片描述、格式混乱的文本等。

+   **结构化优化：** 对于半结构化数据（如 FAQ），确保问答对格式清晰、一致。对于长篇文档，检查段落划分是否逻辑清晰。

+   **内容准确性：** 确保知识库内容本身是准确、最新的。

+   利用 Dify 的预处理规则 （适度）:

+   在知识库的“分段设置”中，勾选“替换掉连续的空格、换行符和制表符”通常是有益的。

+   是否“删除 URL 和电子邮件地址”取决于这些信息对您的应用是否有用。

+   **文件格式选择：** 虽然 Dify 支持多种格式，但 .md （Markdown） 和 .txt 通常最容易保持干净的文本结构，易于解析。对于 PDF 和 DOCX，务必检查 Dify 的文本提取效果。

2. 知识库配置：精细调整分块与嵌入

+   分块策略 （Chunking）:

+   **理解参数：** 仔细调整“分段最大长度”和“分段重叠长度”。

+   最大长度 （Max Length）: 500 tokens 是一个常用起点，但需要根据内容调整。如果文档包含许多短而精的信息（如花语），过大的块可能包含太多不相关内容；如果文档是长篇论述，过小的块可能切断语义。

+   **重叠长度 （Overlap）:** 设置为最大长度的 10-20% （如 50-100 tokens for 500 length） 通常能较好地保证语义连贯性。

+   **使用“预览块”:** Dify 提供的“预览块”功能非常有用，用它来检查实际的分块效果是否符合预期，特别是对于格式复杂的文档。

+   考虑“父子分段” （如果适用）: Dify 提供的“父子分段”模式（截图中有显示，但我们例子中未使用）是一种高级策略。它允许召回更小的、精确匹配的“子块”，但在生成答案时使用包含更广泛上下文的“父块”。对于需要精确查找但又需要上下文理解的复杂场景可以尝试。

+   嵌入模型选择 （Embedding Model）:

+   **选择高质量模型：** 这是决定语义检索效果的核心。**BAAI/bge-m3** 对于中英文混合场景是非常好的选择。确保您已在 Dify 中正确配置并选用了它。

+   **一致性：** 确保知识库使用的嵌入模型与后续可能用于查询嵌入的模型（Dify 通常自动处理一致性）是同一个。

+   **成本与性能：** 不同的嵌入模型在性能和（如果调用 API）成本上可能不同，需要权衡。

3. 检索策略：混合检索 + Reranker 优先

+   **首选“混合检索”:** 如前所述，它结合了语义和关键词匹配的优点，对多样化的知识库和用户问题适应性最好。强烈建议在 Dify 中优先选用此模式。

+   **启用并配置 Reranker:**

+   **Rera**n**ker 是**提高检索精度的关键武器。务必启用它，并选择一个性能优良的 Reranker 模型（如 BAAI/bge-reranker-v2-m3）。

+   仔细配置 Top K 和 Score 阈值：

+   **Top K （最终数量）:** 通常设置为 3-5。太少可能信息不足，太多可能引入噪声或超出 LLM 处理能力/增加成本。

+   **Score 阈值：** 从 0.5-0.7 开始测试，根据实际效果调整。如果发现召回了很多不相关的结果，尝试提高阈值；如果发现经常召回不到信息（即使文档中有），尝试降低阈值。

+   **理解局限性：** 没有完美的检索，总会有边界情况。关键是通过测试找到最适合您数据和场景的平衡点。

4. Prompt 工程：明确指示，引导 LLM

+   System Prompt 是灵魂：

+   **明确角色和任务：** 定义助手的身份和核心职责。

+   **强调基于上下文：** 反复强调 “必须严格根据提供的上下文信息回答”。

+   **处理未知情况：**** 明确指****示在上下**文中找不到答案时如何回应（例如，“根据现有资料无法回答...”），禁止猜测。这是减少幻觉的关键。

+   **约束行为：** 添加其他必要的规则（如语气、拒绝范围等）。

+   **善用 ****{{context}}**** 和 ****{{query}}****：**

+   在主 Prompt 模板中，**清晰地将 ****{{context}}**** 和 ****{{query}}**** 分隔开**，并给出明确指令（例如，“请根据以上参考信息，回答用户的问题：...”) .

+   避免 Prompt 过于复杂，保持指令清晰直接。

+   **迭代优化：** Prompt 不是一次就能写好的。根据测试结果，不断调整措辞和规则，观察 LLM 行为的变化。

5. 测试与评估：持续迭代，数据驱动

+   **广泛测试：** 使用各种类型的问题进行测试：

+   **事实型：** 文档中有明确答案的问题。

+   **概念型：** 需要理解和总结的问题。

+   **模糊型：** 用户意图不太明确的问题。

+   **边缘型：** 涉及文档边界或细节的问题。

+   **超出范围型：** 测试拒绝回答的能力。

+   **措辞变化型：** 用不同的说法问同一个问题，测试鲁棒性。

+   利用 Dify 的“日志与标注”:

+   **定期审查日志：** 查看真实的交互记录，了解用户问了什么，机器人答了什么。

+   **进行标注：** 对不满意的回答进行标记（点踩、评论、修正），分析失败原因（是检索问题还是生成问题？) . 高质量的标注数据是未来优化的基础。

+   **关注“监测”数据：**

+   监控**响应时间****（Reranke**r 会增加延迟）、Toke**n **消耗（上下文越长，消耗越多）、成本。

+   如果成本过高或延迟过大，考虑优化 Prompt 以减少输出长度、调整 Top K、评估是否必须使用 Reranker 或选择更经济的模型。

6. 渐进式优化：先求有，再求好

+   **从基础开始：** 先搭建一个能跑通的基础 RAG 流程（例如，向量检索，不带 Reranker，简单的 Prompt）。

+   **逐步引入复杂性：** 在基础流程可以工作后，再逐步引入 Reranker、尝试混合检索、优化 Prompt、调整分块参数等。这样更容易定位问题和评估每次改动的效果。

**总结：**

**在 Dify 中构建高效 RAG 应用是一个系统工程，涉**及高质量数据准备、精细化知识库配置、智能检索策略选择、精确 Prompt 引导以及持续的测试与迭代。遵循这些最佳实践，并充分利用 Dify 提供的可视化配置、调试、日志和监控工具，您将能构建出强大、可靠且真正解决业务问题的智能问答应用。

## 4.5 本章小结

在本章中，我们聚焦于构建能回答特定领域问题的 RAG 应用。通过为“易速鲜花”公司创建内部员工助手的实**例，我们实践了利用 D****ify 知识库****功能处理包含 TXT、DO****CX、PDF **等多种格式的私有**文档。我们**重点配置并使用了 BAAI/bge-m3 作为嵌入模型 来实现高质量的语义检索，并选择了 deepseek-chat 作为生成模型。我们还探讨了 BAAI/bge-reranker-v2-m3 重排模型 的作用及其在提升检索精度方面的可选应用。核心步骤包括创建知识库、上传并处理文档、在应用中集成知识库上下文、配置检索策略（包括 Top K 和可选的 Reranker），以及最关键的——设计能够指导 LLM 基于检索到的 {{context}} 回答用户 {{query}} 的 Prompt。通过 RAG，我们让 AI 应用突破了通用知识的限制，能够为企业提供基于内部数据的精准智能问答服务。

## 4.6 实践练习

1.  **完成“易速鲜花”助手：** 严格按照本章步骤，使用您准备的“易速鲜花”文档（或自备类似文档），在 Dify 中完成知识库创建（确保选择 BAAI/bge-m3 嵌入模型）、应用创建、知识库集成和 RAG Prompt 配置（使用 deepseek-chat LLM）。

1.  **基础 RAG 测试：** 针对您上传的文档内容，在预览窗口中进行充分的提问测试，评估问答准确性，特别是对于文档中有明确答案的问题和没有答案的问题。

1.  （进阶）启用并测试 Reranker:

+   如果在基础测试中发现部分问题回答不够精确（**上下文**选择不佳），尝试在“上下文”节点中启用 BAAI/bge-reranker-v2-m3（如果已配置）。

+   适当调整初始检索的 Top K（例如增大到 10）和 Reranker 输出的 Top N（例如设置为 3）。

+   重新测试之前效果不佳的问题，观察启用 Reranker 后回答的准确性是否有提升。记录对比结果。

1.  **思考优化点：** 除了 Reranker，思考还有哪些方式可以优化 RAG 应用的效果？（例如：改进源文档质量、优化分块策略、调整 Prompt 指令等）。

好的，我们来开始撰写第五章的内容。本章将介绍 Dify 的高级功能——Agent（智能代理），它赋予了 LLM 应用超越文本生成、执行实际任务的能力。

---

# 第五部分：高级功能：使用 Agent 实现自动化工作流 （Using Agents）

前面的章节我们探讨了如何构建对话机器人和基于知识库的问答应用。这**些应用主要在于“说”和“答”。本章我们将进入一个更激动人心**的领域：让 AI 应用“做”事情。我们将学习 Dify 的 Agent 功能，它允许我们创建能够使用工具 （Tools）来与外部世界交互、执行计算或操作数据的智能代理，从而实现复杂的自动化工作流。

## 5.1 Dify Agent 概念解析

### 5.1.1 什么是 Agent？

**在 Dify（以及更广泛**的 AI 领**域）中，Agent （智能代理） 指的**是一种具备使用工具 （Tools） 能力的、由大语言模型 （LLM） 驱动的应用程序。

与基础的聊天机器人或 RAG 应用不同，Agent 的核心特征**在于它不仅仅是处理和生成文本信息，它还能根据用户**的目标或指令，自主地规划步骤、选择并调用一个或多个预定义的“工具”来执行特定任务，然后根据工具返回的结果决定下一步行动，最终达成目标。

可以把 LLM 想象成 Agent 的“大脑”，负责思考、推理和决策；而“工具”则是 Agent 的“手和脚”，负责执行具体的动作（如访问网页、查询数据库、调用 API 等）。

### 5.1.2 Agent 的工作模式：思考与行动的循环

Agent 通常遵循一个“**思考-行动-观察” （Reasoning Loo**p / ReAct Framework） 的工作模式：

1.  理解目标 （Goal Understanding）: Agent 接收用户的请求或预设的目标。例如：“帮我查一下明天上海的天气，并告诉我是否需要带伞。”

1.  **规划 （Planning）:** Agent 的“大脑”（LLM）根据其被赋予的指令和可用工具，思考如何达成目标。它可能会分解任务：“① 需要查询天气；② 需要判断是否下雨；③ 需要给出建议。”

1.  工具选择 （Tool Selection）: Agent 决定需要使用哪个工具来执行第一步。例如，它识别出需要使用“天气查询”工具。

1.  输入生成 （Input Generation）: Agent 判断调用“天气查询”工具需要哪些参数，并从用户请求中提取或推断出这些参数。例如，参数是“城市=上海”、“日期=明天”。

1.  工具执行 （Tool Execution）: Agent 调用“天气查询”工具，并将生成的参数传递给它。工具（可能是一个 API 调用）执行查询。

1.  观察结果 （Observation）: 工具返回执行结果。例如：“上海，明天，小雨，最高温 20℃，最低温 15℃”。

1.  反思与决策 （Reflection & Decision）: Agent 分析工具返回的结果。

+   它现在知道了明天下雨。

+   它判断任务的第二步（判断是否下雨）已完成。

+   它规划下一步：基于“下雨”这个信息生成最终建议。

1.  生成最终回复 （Final Response Generation）: Agent （LLM） 组织语言，生成最终的回复给用户：“明天的上海天气预报是小雨，气温在 15℃ 到 20℃ 之间。出门最好带上雨伞☔️。”

![](img/1f72d431a469353e3a94115c3a7f1cc4.png)

```
graph TD
    A[用户请求Agent 目标] --&gt; B{1-理解规划基于LLM思考};
    B --&gt; C(2-选择工具);
    C --&gt; D(3-生成工具输入参数);
    D --&gt; E[4-调用工具-执行动作];
    E --&gt; F(5-观察工具结果);
    F --&gt; G{6-反思决策基于LLM思考};
    G -- 任务未完成 --&gt; B;
    G -- 任务完成 --&gt; H[7-生成最终回复];

    subgraph Agent 内部循环
        B; C; D; E; F; G;
    end

    style A fill:#eff6ff,stroke:#60a5fa
    style B fill:#fefce8,stroke:#eab308
    style C fill:#f0fdfa,stroke:#2dd4bf
    style D fill:#f5f3ff,stroke:#a78bfa
    style E fill:#fff1f2,stroke:#f43f5e
    style F fill:#ecfdf5,stroke:#34d399
    style G fill:#fefce8,stroke:#eab308
    style H fill:#dcfce7,stroke:#22c55e
```

这个循环可能会根据任务的复杂性重复多次，Agent 可能会调用多个不同的工具来逐步完成一个复杂的目标。

## 5.2 设计 Agent 的 Prompt （为“热点综述”量身定做）

现在，我们设计一个的优秀 Prompt 结构，为其注入更明确的 Agent 指令，并进行一些优化，来指导样例“热点综述” Agent 的思考和行动。

**首先我们创建**一个空白应用，选择应用类型为 Agent

![](img/39a2d0f86efdbe232bc95c5abef2104a.png)

### 5.2.1 Agent Prompt 设计

```
# Agent 身份与核心目标
你是一个名为“热点综述”的智能分析助手。你的核心目标是根据用户提供的关键字（keyword），通过结合权威背景知识和最新的互联网信息，生成一份简洁、准确、时效性强的综合分析报告。

# 可用工具清单
你被授权使用以下工具来完成任务：
1.  `current_time`: 获取当前的精确日期和时间。
2.  `wikipedia_search`: 从维基百科查找指定查询词（query）的背景信息、定义、历史等。
3.  `searxng_search`: 在互联网上搜索与指定查询词（query）相关的最新新闻、数据、讨论和报告。

# 工作流程与思考指令 (必须严格遵循)

当你收到用户提供的关键字 `{{keyword}}` 后，请按以下步骤思考和行动：

1.  **获取当前时间:**
    *   **思考:** 为了确保报告的时效性，我首先需要知道现在是什么时候。
    *   **行动:** **立即调用** `current_time` 工具。
    *   **观察:** 记录下工具返回的当前日期和时间。

2.  **准备背景知识查询:**
    *   **思考:** 为了全面理解 `{{keyword}}`，我需要先查找权威的背景信息。维基百科是很好的来源。为了获得更丰富和准确的英文维基百科信息，我应该先将 `{{keyword}}` 翻译成英文（如果它不是英文的话）。
    *   **行动 (内部思考/LLM能力):** 将 `{{keyword}}` 翻译成对应的英文术语（例如，如果 `{{keyword}}` 是“人工智能”，则翻译为 &quot;Artificial Intelligence&quot;）。*(注：这依赖于你的翻译能力)*
    *   **行动:** 使用翻译后的英文术语作为查询词 (query)，**调用** `wikipedia_search` 工具。

3.  **准备实时信息查询:**
    *   **思考:** 背景知识很重要，但用户通常也关心 `{{keyword}}` 的最新动态。我需要用搜索引擎来查找最近的相关信息。为了结果更聚焦，可以在搜索词中加入年份或“最新动态”等词语。
    *   **行动:** 构造一个合适的搜索查询语句 (query)，例如 `{{keyword}} 最新动态` 或 `{{keyword}} trends 2024`（结合当前年份，可以使用步骤1获取的年份信息）。**调用** `searxng_search` 工具。

4.  **综合分析与报告生成 (核心步骤):**
    *   **观察:** 我现在手头有三方面的信息：① 当前日期时间；② 来自 `wikipedia_search` 的背景知识；③ 来自 `searxng_search` 的最新网络信息。
    *   **思考:** 我的任务是综合这些信息，提炼出关于 `{{keyword}}` 的主要现状、关键点和可能的发展趋势。我需要确保信息的准确性，并以结构化的方式呈现给用户。
    *   **行动:**
        *   仔细阅读和理解 `wikipedia_search` 和 `searxng_search` 返回的所有内容。
        *   识别出其中的核心概念、关键事件、重要数据点和主要观点。
        *   结合背景知识和最新动态，进行对比和分析，识别出可能的发展趋势或当前热点。
        *   **生成最终报告:** 撰写一份分析报告，必须包含以下部分：
            *   **报告时间:** 使用步骤 1 获取的日期。
            *   **关键字:** 用户提供的 `{{keyword}}`。
            *   **核心概念与背景:** 基于 Wikipedia 结果的简要概述。
            *   **最新动态与趋势:** 基于搜索引擎结果的关键信息提炼。
            *   **总结:** 对整体情况的简短概括。
        *   报告应语言精练、条理清晰、重点突出。

# 错误处理指南
- 如果任何工具调用失败（例如 `wikipedia_search` 或 `searxng_search` 无法连接或返回错误），你应该在报告中明确指出该信息源获取失败，并尽可能基于其他成功获取的信息生成报告。
- 如果 `wikipedia_search` 没有找到关于翻译后术语的明确信息，应在报告中说明“未能从维基百科获取明确的背景信息”。
- 如果 `searxng_search` 返回的结果很少或不相关，应在报告中说明“关于该主题的最新网络信息较少”。
- **绝对禁止**在工具失败或信息不足时编造内容。

# 输出要求
直接输出最终的分析报告，不需要展示你的思考步骤或工具调用细节。报告格式应清晰易读。
```

### 5.2.2 Prompt 设计要点回顾

+   **明确角色与目标：** 让 Agent 知道自己是谁，要做什么。

+   **清晰列出工具：** 让 Agent 明确自己拥有的能力。

+   **结构化指令：** 使用分步指令引导 Agent 的思考和行动流程。

+   **明确工具使用时机：** 告诉 Agent 在什么情况下应该使用哪个工具。

+   **结果处理：** 指导 Agent 如何处理工具返回的结果，以及如何综合信息。

+   **错误处理：** 提供应对异常情况的预案。

+   **输出格式要求：** 明确最终输出的格式和内容。

+   **使用输入变量：** 通过 {{keyword}} 接收用户输入。

### 5.2.3 在 Dify 中配置 Prompt

1.  进入“热点综述 Agent”的“编排”或“Prompt Engineering”界面。

1.  **将上述优化后的 Pro**mpt 粘贴到主要的 Prompt 编辑区域（通常称为 System Prompt 或主 Prompt）。

![](img/d85107aaf933a224bddd63b37a7b88fb.png)

1.  **配置输入变量：** Agent 应用通常需要定义用户输入。我们需要定义一个变量来接收用户想要分析的关键字。点击“变量 （Variables）”或类似设置，添加**一个**名为 keyword 的输入变量，类型为字符串 （String），并设置为必需。如上图所示，在添加变量的对话框中选择添加

![](img/6072f86bf73c1247361cfca25c5bf9a3.png)

1.  确保 LLM 模型选择为您配置的可用模型（例如 deepseek-chat）。

![](img/4d8bdb1467c454e1a9992fd11a074d89.png)

## 5.3 配置与使用内置工具

### 5.3.1 本例使用的内置工具

+   **current_time****:**

+   **目的：** 获取当前的日期和时间。这对于需要确保信息时效性的任务（如我们的“热点综述”）非常有用。

+   **工作方式：** 调用此工具通常无需额外输入，它会直接返回当前的日期时间字符串。

+   **w**ikipedia_search:

+   **目的：** 从维基百科 （Wikipedia） 这个庞大的在线百科全书中查找结构化的背景知识和定义。

+   **工作方式：** Agent 需要提供查询的**关键词 （query）**。该工具会调用维基百科 API 或进行网页搜索，返回与关键词相关的维基百科页面摘要或主要内容。

+   **searxng_search****:**

+   **目的：** 通过 SearXNG 这个元搜索引擎（可以聚合多个搜索引擎结果）在互联网上进行广泛搜索，以获取最新的新闻、数据、讨论等实时信息。

+   **工作方式：** Agent 需要提供**搜索查询语句 （query）**。该工具将查询提交给 SearXNG 实例，返回搜索结果列表（通常包含标题、链接和摘要）。（配置提示：使用此工具可能需要在 Dify 中配置您的 SearXNG 实例地址。)

### 5.3.2 在 Dify Agent 中启用工具

要在我们的“热点综述” Agent 中使用这些工具：

1.  **启用工具：** 进入 Agent 应用的配置界面，找到“工具 （Tools）”管理区域。

1.  **勾选启用：** 在内置工具列表中，找到并勾选 current_time， wikipedia_search， 和 searxng_search。如下图所示，wikipedia_search 使用前需要安装。

![](img/502f5a7febf4580c5e535ff99da8707a.png)

1.  **（可能需要）配置工具：** 检查 searxng_search 工具是否需要配置 SearXNG 的 URL 或其他参数。根据您的 Dify 环境完成相应设置。如下图所示，searxng_search 第一次使用需要授权

![](img/db43591ca260cc268513bf7893459a33.png)

输入SearXNG Base URL 即可

![](img/8ceb7f8e8fbb42487dfddf994fd7cd2a.png)

1.  **保存配置：**点击右上角的发布按钮**。**

现在，我们的 Agent 就具备了获取时间、查维基和上网搜索的能力了。

![](img/e6838b5a9d87c46553807c8a7b4923f7.png)

### 5.3.3 测试与预览

好的，这里有几个用于测试“热点综述 Agent”的示例提问，这些提问旨在尽可能地调用您配置的全部三个内置工具 （current_time， wikipedia_search， searxng_search），并覆盖不同的领域：

1.  **技术趋势 （中文）:**

+   **提问：** 大语言模型

+   **预期流程：**

+   Agent 调用 current_time 获取当前日期。

+   Agent （LLM） 将“大语言模型”翻译为 "Large Language Models" 或类似英文。

+   Agent 调用 wikipedia_search 搜索 "Large Language Models"，获取定义、关键技术、发展历史等背景。

+   Agent 调用 searxng_search 搜索类似 "大语言模型 最新进展" 或 "Large Language Models trends 2024" 等，获取最新研究、应用新闻、开源模型发布等信息。

+   Agent 综合以上信息，生成关于大语言模型背景、现状及趋势的报告。

1.  **环境与政策 （中文）:**

+   **提问：** 碳中和

+   **预期流程：**

+   Agent 调用 current_time。

+   Agent （LLM） 将“碳中和”翻译为 "Carbon Neutrality"。

+   Agent 调用 wikipedia_search 搜索 "Carbon Neutrality"，获取其定义、目标、相关国际协议等背景。

+   Agent 调用 searxng_search 搜索类似 "碳中和 最新政策" 或 "Carbon Neutrality technology 2024" 等，获取各国最新政策、相关技术突破、企业实践等信息。

+   Agent 综合信息，生成关于碳中和背景、当前全球进展及面临挑战的报告。

1.  **知名公司 （英文）:**

+   **提问：** NVIDIA

+   **预期流程：**

+   Agent 调用 current_time。

+   关键词是英文，无需翻译。

+   Agent 调用 wikipedia_search 搜索 "NVIDIA"，获取公司历史、主要产品线（GPU、AI 芯片等）、创始人等背景信息。

+   Agent 调用 searxng_search 搜索类似 "NVIDIA latest news" 或 "NVIDIA stock performance 2024" 等，获取最新的财报信息、新产品发布、股价动态、行业分析等。

+   Agent 综合信息，生成关于 NVIDIA 公司概况、近期业务重点和市场表现的报告。

1.  **科学领域 （中文）:**

+   **提问：** CRISPR基因编辑

+   **预期流程：**

+   Agent 调用 current_time。

+   Agent （LLM） 将“CRISPR 基因编辑”翻译为 "CRISPR Gene Editing"。

+   Agent 调用 wikipedia_search 搜索 "CRISPR Gene Editing"，获取其科学原理、发现历史、主要应用（医学、农业等）及伦理争议等背景。

+   Agent 调用 searxng_search 搜索类似 "CRISPR 最新研究" 或 "CRISPR gene editing clinical trials 2024" 等，获取最新的研究论文摘要、临床试验进展、监管动态、伦理讨论等。

+   Agent 综合信息，生成关于 CRISPR 技术背景、最新科研进展和应用前景的报告。

**测试时注意观察：**

+   报告中是否包含了正确的当前日期？

+   背景信息是否看起来源自维基百科（通常比较客观、结构化）？

+   最新动态是否包含了近期的新闻或数据点？

+   对于中文提问，Agent 是否能正确处理（可能涉及内部翻译）并在 Wikipedia 和 Search 中找到相关内容？

+   报告的整体结构是否符合 Prompt 中定义的要求（关键字、背景、动态、总结）？

+   报告的语言是否流畅、信息是否准确？

使用这些不同类型的提问，可以比较全面地测试您的“热点综述 Agent”在不同场景下的信息获取、整合与分析能力。

![](img/5cefa4727c1ee60daa810e35d153ba7d.png)

查看日志-详情

![](img/007e2b4e298faf0eea5bfdd40373ed5f.png)

查看日志-追踪

![](img/3d1b5a1c3bf861452d6f3ec2e049a4b5.png)

## 5.4 关于自定义工具 （Custom Tools）

**Dify** Agen**t 的强大之处在**于其可扩展性。如 5.1 节所述，您完全可以创建自定义工具 （Custom Tools），特别是 API 调用工具，来连接您企业内部的数据库、CRM、项目管理系统或其他任何提供 API 的服务。

例如，您可以创建一个工具 get_internal_project_updates 来查询公司内部关于某个项目的最新进展，让 Agent 能够结合公开信息和内部信息进行更全面的分析。创建自定义工具涉及定义工具的描述（给 Agent 看）、API 端点、认证方式和参数等。

虽然本章我们专注于内置工具的应用，但理解自定义工具的可能性对于未来构建更复杂的企业级 Agent 至关重要。

##

## 5.5 Agent 应用场景实例

（本节以“热点综述”为核心实例）

我们的“**热点综述 Agent**”本身就是一个典型的 Agent 应用实例：

+   **场景：** 用户（如市场分析师、研究员、学生或任何需要快速了解某个主题的人）想要快速获取关于某个关键字（如新技术、行业趋势、公司名称、特定事件等）的全面且最新的信息概览。

+   **价值：** 相比手动分别查询维基百科和搜索引擎，然后自行整理，这个 Agent 大大提高了信息获取和初步分析的效率。它自动完成了数据收集、整合和初步提炼的过程。

其他 Agent 应用场景 （拓展思路）:

+   **自动化报告生成：** （保持不变） ...

+   **智能客户意图识别与服务分配：** （保持不变） ...

+   市场信息监控与分析 （类似但可更深入）: 可以让 Agent 定期运行“热点综述”，并将结果存入数据库或发送邮件，实现持续监控。

+   代码生成或辅助调试 （需谨慎）: （保持不变） ...

+   **旅行规划助手：** Agent 调用天气查询工具、机票/酒店预订 API 工具、地图/地点信息工具，根据用户偏好生成旅行计划。

+   **会议纪要与行动项提取：** Agent 分析会议录音转文字稿，（可选）调用内部项目管理 API 工具，自动生成会议纪要，并识别、分配行动项。

## 5.6 本章小结

本章我们深入探索了 Dify 的 **Agent 功能，并**通**过构建一**个实用的“热点综述 Agent”，实践了如何利用它实现自动化信息处理工作流**。我们理解了 Agent**** 通过“思考**-行动-观察”循环，结合内置工具（如本例中的 current_time， wikipedia_search， searxng_search）来达成目标。我们重点学习了如何为 Agent 设计有效的 Prompt，这不仅包括定义目标和可用工具，更关键的是提供清晰的、分步骤的工作流程指令，引导 Agent 的思考、工具选择、结果处理以及错误应对。虽然本例未使用自定义工具，但我们也了解了通过 API 工具连接企业内部系统来扩展 Agent 能力的可能性。通过“热点综述”实例，我们看到了 Agent 在自动化信息收集、分析和报告生成方面的巨大潜力。

掌握 Agent 的设计和应用，是利用 Dify 构建更高级、更主动的 AI 解决方案的关键。

## 5.7 实践练习：构建美股基本面分析助手

**在本章的实践练习中****，我们将构建一个实用且**功能强大的 Agent——“美股基本面分析助手”。这个 Agent 将利用 Dify 内置的雅虎财经系列工具和 current_time 工具，根据用户提供的美股代码 （Ticker Symbol），自动获取实时数据并生成一份结构化的基本面分析报告。

**本**练习的独特之处在于，我们将直接使用 Dify 提供的真实可用工具，让您亲身体验 Agent 如何与外部数据源交互并完成复杂任务。

**练习目标：**

1.  设计一个 Agent 应用，命名为 美股基本面分析助手。

1.  定义所需的输入变量 （stock_ticker）。

1.  **配置 Agent 使用 ****Di****fy**** 内置的**** cur**rent_time， yahoo_finance_ticker， yahoo_finance_news， 和 yahoo_finance_analytics 工具。

1.  编写并优化一个详细的 Prompt，指导 Agent 按特定流程进行基本面分析。

1.  **实际测试 Agent**，输入真实股票代码，观察其数据获取和报告生成能力。

**使用的 Dify 内置工具：**

1.  **current_time****：** 获取当前日期和时间。

1.  **y**ahoo_finance_ticker:

+   **Dify 描述 ：**根据提供的股票代码 （ticker），从雅虎财经获取该股票的基本信息、实时报价和关键统计数据。

+   **我们用它来获取：** 公司基本信息（名称、行业等可能包含在内，需要测试确认），以及可能的实时价格等。

1.  **y**ahoo_finance_news:

+   **Dify 描述 :** 根据提供的股票代码 （ticker），从雅虎财经获取相关的最新新闻报道。

+   **我们用它来获取：** 最新的公司相关新闻。

1.  **y**ahoo_finance_analytics:

+   **Dify 描述 :** 根据提供的股票代码 （ticker），从雅虎财经获取分析师的评级、目标价、盈利预测以及关键的财务比率摘要。

+   **我们用它来获取：** 关键财务指标、比率、分析师观点等基本面分析的核心数据。

**优化后的 Agent Prompt （指导**“美股基本面分析助手”，适配内置工具）:

```
# Agent 身份与核心目标
你是一位专业的“美股基本面分析助手”。你的核心任务是接收用户指定的美股代码 `{{stock_ticker}}`，利用内置的 `current_time`, `yahoo_finance_ticker`, `yahoo_finance_news`, 和 `yahoo_finance_analytics` 工具，生成一份结构化、数据驱动且包含最新信息的基本面分析报告。

# 可用工具清单
你被授权使用以下 Dify 内置工具：
1.  `current_time`: 获取当前的精确日期和时间，用于报告时间戳。
2.  `yahoo_finance_ticker`: 输入 `ticker` (股票代码)，获取公司基本信息和报价统计。
3.  `yahoo_finance_news`: 输入 `ticker` (股票代码)，获取最新相关新闻。
4.  `yahoo_finance_analytics`: 输入 `ticker` (股票代码)，获取分析师观点和关键财务比率摘要。**请直接解读此工具返回的比率和分析师观点，无需自行计算。**

# 分析工作流程 (必须严格按顺序执行)

在收到用户提供的 `{{stock_ticker}}` 后，请按以下步骤执行分析并生成报告：

**阶段一：信息收集 (按顺序调用工具)**
1.  **调用 `current_time`:** 获取当前日期，记录为报告日期。
2.  **调用 `yahoo_finance_ticker`:** 使用 `{{stock_ticker}}` 作为 `ticker` 输入。仔细查看返回结果，提取并记录公司全名、可能的行业分类、当前股价（如有）等基本信息。
3.  **调用 `yahoo_finance_analytics`:** 使用 `{{stock_ticker}}` 作为 `ticker` 输入。提取并记录所有可用的关键财务指标和比率摘要（盈利能力、偿债能力、市场估值等）以及分析师评级、目标价等信息。
4.  **调用 `yahoo_finance_news`:** 使用 `{{stock_ticker}}` 作为 `ticker` 输入。提取并记录 2-3 条最新的、最相关的新闻标题或摘要。

**阶段二：分析报告生成 (使用 Markdown 格式)**
根据阶段一收集到的信息，撰写报告，必须包含以下结构化部分：

**1. 报告基本信息**
    *   **股票代码:** `{{stock_ticker}}`
    *   **公司名称:** [来自 `yahoo_finance_ticker` 的结果]
    *   **报告生成日期:** [来自 `current_time` 的结果]
    *   **(可选) 当前股价:** [来自 `yahoo_finance_ticker` 的结果，如果提供]

**2. 公司概览**
    *   **所属行业:** [尝试从 `yahoo_finance_ticker` 或 `yahoo_finance_analytics` 的结果中提取，如无则注明信息缺失]
    *   **主营业务简介:** [尝试从 `yahoo_finance_ticker` 结果中提取简要描述，如无则注明信息缺失]

**3. 财务与分析师观点解读 (主要基于 `yahoo_finance_analytics` 的数据)**
    *   **盈利能力:** 解读工具返回的如 ROE (净资产收益率), Margin (利润率) 等指标。
    *   **财务健康 (偿债能力):** 解读工具返回的如 Debt-to-Equity (负债权益比) 等指标。
    *   **市场估值:** 解读工具返回的如 P/E Ratio (市盈率), P/B Ratio (市净率) 等指标。
    *   **分析师评级与目标价:** 总结工具返回的分析师共识评级 (如 Buy/Hold/Sell) 和平均目标价。

**4. 近期新闻动态 (基于 `yahoo_finance_news`)**
    *   列出并简要说明提取到的 2-3 条关键近期新闻。

**5. 总结 (基于以上所有信息)**
    *   综合财务解读、分析师观点和新闻动态，对该股票的基本面给出一个客观的总结。指出数据反映的主要看点和潜在风险。
    *   **重要提示:** 仅基于工具返回的信息进行客观陈述，**绝对禁止提供任何形式的个人投资建议或买卖推荐。**

# 行为约束
- **语言:** 使用中文进行响应。
- **范围:** 分析严格限定在基本面范畴，且完全基于已调用的工具返回的信息。不进行任何工具未提供信息的推测。
- **客观性:** 保持专业、客观、数据驱动的分析口吻。
- **输出格式:** 严格使用 Markdown 格式化报告，包含清晰的标题和项目符号列表。直接输出最终报告，省略你的思考过程和工具调用细节。
- **错误处理:** 如果某个工具调用失败或未能返回预期的数据（例如 `yahoo_finance_news` 未找到新闻，或 `yahoo_finance_analytics` 缺少某个比率），必须在报告的相应部分明确注明“未能获取相关信息”或“数据点缺失”，不得臆测或编造。
```

**实践步骤：**

1.  **创建 Agent 应用：** 在 Dify 中创建一个新的 Agent 应用，命名为 美股基本面分析助手。

1.  **定义输入：** 添加一个名为 stock_ticker 的字符串类型输入变量，设为必需。

1.  **启用工具：**

+   在工具列表中，找到并**勾选启用**内置的 current_time， yahoo_finance_ticker， yahoo_finance_news， 和 yahoo_finance_analytics 工具。

+   （确保这些工具在您的 Dify 环境中是可用状态）

1.  **配置 Prompt:** 将上面优化后的 Prompt 粘贴到 Agent 的 Prompt 配置区域。

1.  **选择 LLM:** 选择如 deepseek-chat 等您配置好的 LLM。

1.  **保存 Agent。**

**如何测试 （实际操作）:**

+   在预览/调试窗口中，输入一个真实的美股代码，例如 AAPL （苹果）， NVDA （英伟达）， TSLA （特斯拉）， GOOGL （谷歌） 等。

+   **启动 Agent 并观察：** 点击发送或运行按钮。耐心等待 Agent 执行其工作流程（调用多个工具可能需要一些时间）。

+   检查 Dify 的 Agent 调试信息 （如果可用）: 理想情况下，Dify 会显示 Agent 的思考过程、它决定调用哪个工具、传递了什么参数、以及工具返回了什么结果。观察这个过程是否与 Prompt 中的指令一致。

+   **评估最终报告：**

+   报告结构是否完整且符合 Markdown 格式要求？

+   报告中的数据（公司名称、行业、财务比率、新闻等）是否看起来是真实的、来自雅虎财经的？

+   Agent 对数据的解读是否客观？

+   对于可能缺失的数据点，Agent 是否按要求注明了？

+   整体报告是否有用、信息密度如何？

+   **多次测试：** 使用不同的股票代码进行测试，包括不同行业、不同市值的公司，观察 Agent 的稳定性和适应性。

**拓展思考：**

+   yahoo_finance_ticker 和 yahoo_finance_analytics 返回的数据可能有哪些重叠或不足？如何优化 Prompt 让 Agent 更好地整合或补充这些信息？

+   如果想加入技术分析（如股价图表趋势、移动平均线等），需要寻找或创建什么样的工具？Prompt 又该如何修改？

+   如何处理用户输入无效股票代码的情况？（可以在 Prompt 中加入验证步骤或错误处理指令）。

这个实践练习将让您直接体验到 Agent 组合使用真实工具完成复杂任务的强大能力，是深入理解 Agent 工作原理和 Prompt 设计技巧的绝佳机会。

好的，遵照您的要求，我去掉了句首不必要的星号，并调整了标题层级。以下是格式化后的第六章完整内容：

---

# 第六部分：探索 Dify 工作流：可视化编排复杂任务 （Exploring Dify Workflows: Visually Orchestrating Complex Tasks）

## 6.1 工作流 （Workflow） 导论

### 6.1.1 什么是 Dify 工作流？

**定义： Dify** Workflow 是一种可视化的编排工具，允许用户通过**拖拽、连接不同的功**能节点（Nodes）来设计和执行复杂的多步骤 AI 任务。

核心理念： 将复杂的任务分解为一系列明确定义的、顺序或条件执行的步骤，实现对 AI 应用逻辑流的精确控制和高透明度。它更像是在绘制一个“流程图”，而不是给 Agent 写指令。

与基础应用的区别： 对比简单对话或 RAG 应用的线性流程（开始 -> [检索] -> LLM -> 回答），Workflow 引入了更丰富的流程控制能力。

### 6.1.2 为什么需要工作流？

处理复杂任务： 当一个任务需要多个 LLM 调用、多个工具配合、或基于不同条件执行不同操作时，Agent 的隐式规划可能变得困难或不可靠，Workflow 提供了更结构化的解决方案。

精确的逻辑控制： 需要实现明确的“如果...那么...否则...”（If/Else） 逻辑分支，或者需要严格按照特定顺序执行步骤。

过程可视化与可调试性： 整个任务流程一目了然，方便理解、调试和定位问题点。可以清晰地看到数据在哪个节点被处理、流向何处。

任务模板化： 可以将一个复杂的、通用的处理流程固化为一个 Workflow 模板，供不同场景复用。

### 6.1.3 理解 Dify 的编排模式：Workflow， Chatflow 与 Agent

Dify 提供了强大的应用构建能力，主要通过三种不同的模式来实现，理解它们的区别对于选择合适的工具至关重要：

1.  智能代理 （Agent - 第五章内容）:

+   **核心机制：** 主要依赖大**语言模型 （LLM）** 的自主规划能力和 Prompt 指导。开发者定义目标和可用工具，由 LLM 决定何时以及如何使用工具来完成任务。

+   **控制方式：** 隐式控制 （Implicit Control）。流程的执行路径由 LLM 动态决定，具有较高的灵活性和适应性，但也可能因 LLM 的不确定性导致行为偏离。

+   **开发重点：** Prompt 工程，设计能有效指导 LLM 思考、规划和行动的指令。

+   **适用场景：** 目标明确但实现路径不固定、需要与外部环境（通过工具）进行复杂动态交互、或需要较强自主性的开放式任务。

1.  工作流 （Workflow - 自动化模式）:

+   **核心机制：** 基于**可视化的流程编排**。开发者通过拖拽和连接不同的功能节点（如** LLM** 调用、工具使用、条件判断等）来精确定义任务执行的每一步和逻辑流。

+   **控制方式：** 显式控制 （Explicit Control）。流程严格按照开发者设计的路径执行，结果更可预测、更稳定。

+   **开发重点：** 可视化流程设计，配置节点功能和节点间的数据传递。

+   **适用场景：** **面向自动化和批处理情景**。适合执行流程相对固定、逻辑判断明确、不太需要实时交互的任务，例如高质量翻译、数据分析与报告生成、内容批量创作、电子邮件自动分类与处理等。我们稍后将实践的“花语小能手”就是一个典型的 Workflow 案例。

1.  聊天流 （Chatflow - 对话式模式）:

+   **核心机制：** **同样基于可视化的流程编排****，与 Workflow **共享许多核心节点和编排逻辑。但其设计目标是面向对话类情景。

+   **控制方式：** **显式控制**，开发者依然通过节点编排来定义逻辑。

+   **关键特性：** **特别强调处理多轮对话交互**。它需要能够管理对话状态（上下文）、处理用户在对话中不断输入的信息，并在多步骤的逻辑处理后，生成连贯的、适合对话的响应。可能包含特定的对话管理节点或配置。

+   **开发重点：** 可视化流程设计，同时要特别关注如何处理对话上下文、用户输入的变化以及生成自然的对话回复。

+   **适用场景：** 面向需要多步逻辑的对话式应用程序。例如，交互式的故障排除向导、需要引导用户完成信息填写的配置助手、结合 RAG 和工具进行多轮问答的智能客服、或者需要根据用户反馈调整策略的个性化推荐等。

**简单对比总结：**

在本章接下来的内容中，我们将分别通过实例来学习如何构建 Workflow （自动化模式） 和 Chatflow （对话式模式），让您掌握 Dify 可视化编排工具在不同场景下的应用。

## 6.2 工作流核心组件：认识节点 （Workflow Nodes）

Workflow 的核心是各种功能节点，用户通过连接这些节点来构建流程。以下是一些常见的节点类型（具体节点可能因 Dify 版本更新而变化）：

开始节点 （Start Node）

+   功能： Workflow 的入口，定义了启动工作流时需要接收的输入变量（例如，用户的请求文本、配置参数等）。

+   配置： 定义变量名称、数据类型、是否必需、默认值等。

6LLM 节点 （LLM Node）

+   功能： 调用配置好的大语言模型（如 deepseek-chat）执行文本生成、分析、转换等任务。是 Workflow 中实现“智能”的核心。

+   配置： 选择模型、编写 Prompt 模板（可以使用来自上游节点的变量）、设置模型参数（温度等）。

知识检索节点 （Knowledge Retrieval Node）

+   功能： 实现 RAG 的关键节点。根据输入查询，从指定的知识库中检索相关文本块。

+   配置： 选择知识库、设置检索模式（向量/全文/混合）、配置 Top K、Score 阈值、可能选择 Reranker 模型等（与第四章 RAG 应用中的配置类似）。

工具节点 （Tool Node）

+   功能： 调用 Dify 中配置好的工具（内置工具或自定义工具）。

+   配置： 选择要调用的工具、将上游节点的输出变量映射到工具所需的输入参数。

**条件判断节点 （If/Else No**de / Condition Node）

+   功能： Workflow 实现逻辑分支的关键。根据一个或多个输入变量的值，判断满足哪个条件，并将流程导向不同的分支路径。
配置： 设置判断条件（例如，变量 user_intent 是否等于 "查询库存"），定义满足条件 （True） 和不满足条件 （False） 时流程走向的下一个节点。可以设置多个条件分支。

变量赋值节点 （Variable Assigner Node）

+   功能： 用于创建新的变量，或者对已有的变量进行赋值、转换、合并等操作。方便在流程中处理和传递数据。

+   配置： 定义输出变量的名称，并使用表达式或模板（可能结合上游变量）来计算变量的值。

问题分类节点 （Question Classifier Node）

+   功能： 专门用于根据用户输入的问题，自动将其分类到预定义的类别中。常用于客服、意图识别场景。

+   配置： 定义问题类别、提供每个类别的描述或示例，节点会利用 LLM 进行分类，并将分类结果作为输出变量。

循环/迭代节点 （Iteration/Loop Node）

+   功能： 对列表或数据集中的每一项重复执行一组节点操作。

**结束节点 （End Node）**

+   功能： Workflow 的出口，定义了整个工作流最终输出给用户的变量和结果。

+   配置： 选择哪些内部变量作为最终输出。

## 6.3 构建您的第一个工作流：花语小能手 （Practical Case: Flower Whisperer）

**理论学习之**后，最好的方式就是动手实践。我们将通过构建一个名为“花语小能手”的 Workflow，来学习如何在 Dify 中使用可视化编排完成一个结合知识检索和创意生成的任务。

### 6.3.1 场景定义：从花名到推广帖

**目标**： 这个**工作流**接收用户输入的一**种花**名，首先从知**识库中**查找该花对应的花语，然后利用 L**LM 将**花名和花语信息 creatively 转化为一篇适合在社交媒体发布的推广帖子。

输入： name （字符串，代表花名）

输出： text （字符串，生成的社交媒体推广帖子）

依赖：

+   **一个包**含花名及其对应花语的 Dify 知识库。

+   配置好的 **LLM** （例如 deepseek-chat）。

### 6.3.2 工作流结构解析 （概述）

通过分析您提供的 DSL 文件（或在 Dify 可视化界面中查看），我们可以清晰地看到“花语小能手”这个工作流的逻辑结构：

1.  开始 （Start Node）:** 这**是整个流程的起点。它的主要作用是接收用户的输入，也就是用户想要查询的花名 （定义为变量 name）。

1.  知识检索 （Knowledge Retrieval Node）: 紧接着“开始”节点，流程进入这一步。它的任务是：

+   接收上一步传入的**花名** （name）。

+   使用这个花名作为查询条件，去**预先配置好的花语知识库**中查找相关信息。

+   将检索到的、包含该花花语描述的文本块作为结果，传递给下一个节点。

1.  大语言模型 （LLM Node）: 这是实现创意生成的核心环节。它接收两方面的输入：

+   来自“开始”节点的原始**花名**。

+   来自“知识检索”节点的**花语信息** （作为上下文 context）。

+   它的核心任务是**遵循预设的 Prompt 指令**：结合花名和检索到的花语（特别是其中的关键词），创作一段用于社交媒体推广的帖子。Prompt 同时还规定了只写一种花，并对未找到花语的情况有特殊的处理要求。该节点使用的是 deepseek-chat 模型。

1.  **结束 （End Node）:**** 作为流程的**终点，它负责将上一步 LLM 节点生成的推广帖子文本作为整个工作流的最终输出返回给用户。

**整体流程概览：**** 整个工作流形成了一个清晰的线性处理管道：用户输入花名 **-> 系统查找对应花语 -> AI 根据花名和花语创作推广文案 -> 系统输出文案。这个结构简单直观，充分利用了知识库的检索能力和 LLM 的生成能力。

![](img/b5eed5f5ce8a9f66a6ca64176e1fe5ea.png)

### 6.3.3 在 Dify 中构建工作流 （可视化操作步骤）

在 Dify 的可视化界面中构建这个工作流的步骤如下：

1.  **创建新工作流：** 在工作室选择创建“工作流 （Workflow）”，命名为 花语小能手，选择一个图标（如 🌷）。

1.  **配置开始节点：**

+   点击自动生成的“开始”节点。

+   在“变量”部分，添加一个变量：

+   变量名 （Variable）: name

+   标签 （Label）: name （或者更友好的“请输入花名”）

+   类型 （Type）: text-input

+   是否必需 （Required）: 勾选

![](img/59d742943acaacafccb71f9003ca7d3c.png)

1.  **添加知识检索节点：**

+   从左侧节点库中，拖拽一个“知识检索 （Knowledge Retrieval）”节点到画布上。

+   将“开始”节点的输出锚点连接到“知识检索”节点的输入锚点。

+   点击“知识检索”节点进行配置：

+   **在“知识库 （Dataset** IDs）”部分，点击选择，找到并勾选包含花语信息的那个知识库。

+   在“查询变量 （Query Variable Selector）”中，选择“开始”节点的输出变量 name。

![](img/b09fe04e5615f1ae41713b2317ab4faf.png)

1.  **添加 LLM 节点：**

+   从节点库拖拽一个“LLM”节点到画布上。

+   将“知识检索”节点的输出锚点连接到“LLM”节点的输入锚点。

+   点击“LLM”节点进行配置：

+   在“模型 （Model）”部分，选择 deepseek-chat （或您配置的别名）。

+   在“上下文 （Context）”部分，勾选“启用 （Enabled）”。在“变量选择器 （Variable Selector）”中，选择“知识检索”节点的输出（通常自动显示为 result 或类似名称）。

+   在“Prompt”部分，粘贴以下优化后的 Prompt 模板：

```
# 角色与任务
你是一位才华横溢、充满热情的花艺推广文案撰稿人，为“易速鲜花”工作。你的任务是根据提供的花名和对应的花语信息，创作一段吸引人的、适合发布在社交媒体（如小红书、微博、朋友圈）的推广帖子。

# 输入信息说明
*   `{{#start.name#}}`: 这是用户想要了解的花的名字。 (*请根据 Dify 实际变量引用语法调整，可能直接是 {{name}}*)
*   `{{#knowledge_retrieval.result#}}`: 这是从我们的花语知识库中找到的关于 `{{#start.name#}}` 的描述性信息。**请特别注意，这段信息中通常会包含代表该花核心寓意的 3 个关键词。** （例如，对于玫瑰，关键词可能是：热情、热爱、美丽）。 (*同样，请根据 Dify 实际变量引用语法调整，可能直接是 {{context}}*)

# 创作要求与规则 (必须严格遵守)

1.  **聚焦单花:** 你的推广帖子**必须且只能**围绕 `{{#start.name#}}` 这一种花展开，绝对不能提及或发散到其他花卉。
2.  **融入花语关键词:**
    *   首先，请从 `{{#knowledge_retrieval.result#}}` 中**准确识别并提取出那 3 个核心花语关键词**。
    *   在你的推广文案中，**必须自然地、创造性地将这 3 个关键词都融入进去**，让文案能够体现出这朵花的独特寓意和情感价值。不要只是简单罗列关键词。
3.  **发挥与润色:** 在融入关键词的基础上，你可以适当发挥想象，用生动、形象、富有感染力的语言描绘花朵的美丽、赠送场景或它能带来的美好感受。文案风格可以略带感性、浪漫或温馨。
4.  **社交媒体风格:** 帖子应简洁明了，适合社交媒体阅读。可以适当使用 Emoji (如 🌸💖✨) 来增加吸引力。可以考虑加入引导性的话语，如“#花语[花名]” 或 “送给特别的TA” 等标签或提示。
5.  **处理未找到的情况 (优雅拒绝):**
    *   **如果 `{{#knowledge_retrieval.result#}}` 为空或明确提示未找到该花的信息**，这意味着我们的知识库中暂时没有收录 `{{#start.name#}}`。此时，你**不能编造花语**，而应该生成类似以下的礼貌回复：
        &quot;哎呀，您提到的 ‘{{#start.name#}}’ 听起来很特别呢！目前我们的花语手册里暂时还没有收录它的专属密语。或许它还在含苞待放，等待着属于它的绽放时刻。我们会留意的，待到繁花盛开时，一定邀请您一同共赏这份独特的美好！敬请期待哦～ 😉&quot;
    *   确保这种回复语气友好、略带歉意，并给出积极的期待。

# 输出格式
直接输出最终生成的社交媒体推广帖子文本。
```

+   **注意：** 上述 Prompt 中的变量引用 {{#node_id.variable_name#}} 或 {{variable_name}} 需要根据您 Dify 界面实际生成的变量引用语法进行替换。Dify 通常提供插入变量的按钮来简化此操作。

![](img/a24bb7da7a9a95becff308eee51672ed.png)

1.  **配置结束节点：**

+   将“LLM”节点的输出锚点连接到“结束”节点的输入锚点。

+   点击“结束”节点进行配置。

+   在“输出 （Outputs）”部分，添加一个输出变量：

+   变量名 （Variable）: text

+   值来源 （Value Selector）: 选择“LLM”节点的输出变量（通常名为 text）。

![](img/a09849a5bb1d0c5c76d96fe573153b6f.png)

1.  **保存工作流。**

### 6.3.4 运行与测试

1.  点击 Workflow 编辑界面右上角的“运行 （Run）”或“调试 （Debug）”按钮。

1.  在弹出的输入框中，输入一个您的知识库中包含的花名（例如：“玫瑰”、“向日葵”、“郁金香”）。

1.  点击执行。

1.  **观察执行过程：**

+   Dify 应该会高亮显示当前正在执行的节点。

+   **您可以点击已执行完成**的节点**（如知识检索、LLM）**，查看它们的输入 （Input） 和 输出 （Output） 数据。

+   检查知识检索节点的输出，看是否成功找到了对应的花语信息。

+   检查 LLM 节点的输入，确认花名和花语是否正确传递给了 Prompt。

+   检查 LLM 节点的输出，查看生成的推广帖子内容。

1.  **查看最终结果：** 在“结束”节点的输出或专门的结果展示区域，查看最终生成的推广帖子。

![](img/12c0e06883bf793f623ddf0f9195879d.png)

![](img/68bdd8058bd2c28ce8ef197c500a9ed5.png)

![](img/5c3a5b70dbed7645863656c8df971ae3.png)

![](img/430f6b1a369d203bd32b2344fc9e1b68.png)

1.  **评估效果：**

+   帖子是否自然流畅？

+   是否准确地融入了花名和花语的关键信息？

+   是否遵循了“只写一种花”的约束？

+   输入一个知识库中**不存在**的花名，测试“优雅拒绝”的逻辑是否按预期触发，回复是否友好？

+   如果效果不佳，思考是知识库内容问题、检索问题，还是 LLM Prompt 需要调整？

通过这个“花语小能手”的实例，您就实践了 Di**fy W**orkf**low **的核心**构建流程：定义**输入 -**> 检索**信息 -> LLM 处理 -> 定义输出，并通过可视化编排将它们串联起来。

### 6.3.5 将工作流发布为MCP Server

将 花语小能手 工作流发布为工具

![](img/ac6134b23d4dc6ba6c38daa5460a8ea7.png)

如果所示输入工具调用名称 工具描述和工具参数描述，方便LLM理解

![](img/22899ace33e9395b2745c0206d5e3cd8.png)

查看插件列表，确保已经安装 MCP Compatible Dify Tools

![](img/1211c603e11a858289440990bc67224d.png)

选择添加API端点

![](img/dca02a5383c482997db3f5b489a50c6c.png)

读取API端点细节

![](img/7afe8f9aab3ca456144366c1e47a4a14.png)

记录sse地址，并进行调整（使用本机IP地址替换localhost），如果地址中有}则予以删除

```
http://192.168.1.6/e/ez3xvk8lwhr50dip/sse
```

在cherry studio中增加MCP 服务器

![](img/f162a8217c1827531e73ed8ad0a28f5d.png)

聊天窗口启用dify-flower

![](img/4a545f52ce7c0c969a8697a012b01498.png)

查询花语

![](img/62b6bbea0bf4f39dff2be65e34fe6719.png)

查看MCP Server调用细节

![](img/9c5aa9247cf98ff1cda5abc312f7a562.png)

## 6.4 实践案例：构建 Chatflow 应用 - 易速客服 （Practical Case: YiSu Customer Service Chatflow）

在了解了 Workflow **（自动化**模式） 的构建方法后，我们现在将注意力转向 Dify 提供的另一种强大的可视化编排模式——Chatflow （对话式模式）。我们将通过构建一个名为“易速客服”的智能客服助手，来体验如何设计能够进行多轮交互、理解用户意图并执行条件逻辑的对话流程。

### 6.4.1 场景定义：智能客服初步响应与转人工

目标： 创建一个 Chatflow，作为“易速鲜花”的**初步客**服。它能接收用户的提问，首先尝试从知识库（包含**常见问**题、产品信息、公司政策等）中查找答案并回复。如果知识库无法回答，它会利用 LLM 的通用知识尝试回答。最关键的是，它能识别用户明确要求“转人工”、“投诉”或类似意图，并将对话引导至一个表示“转接人工”的特定回复。

交互模式： 对话式，支持多轮问答。

核心逻辑： RAG + LLM 回答 + 意图识别与条件路由。

依赖：

+   **包含“**易速鲜花”相关信息的 Dify 知识库。

+   配置好的 **LLM** （例如 deepseek-chat）。

### 6.4.2 Chatflow 结构解析

分析提供的 DSL 文件或在 Dify 可视化界面查看，“易速客服”Chatflow 的核心逻辑流程如下：

1.  **开始 （S**tart Node）: 作为对话的入口，它会捕获用户在聊天窗口中输入的每一轮消息 （表示为系统变量 sys.query）。

1.  知识检索 （Knowledge Retrieva**l Node）: 接收用**户当前的问题 （sys.query），在预设的“易速鲜花”知识库中进行检索，尝试查找相关的背景信息或答案。检索到的内容将作为上下文传递下去。

1.  大语言模型 （LLM Node）: 这是 Cha**tflow 的“大脑”。它接收**用户的问题 （sys.query） 和从知识库检索到的上下文 （context）。其主要职责是根据提供的 Prompt 指令，结合上下文（如果存在）或自身的通用知识，生成一个初步的、对话式的回复。

1.  条**件分支 （If/Else ****Node）: 在 ****L**LM 生成回复之后（或者并行地，这取决于具体实现，但 DSL 显示其在 LLM 之后），这一步是关键的路由环节。它检查用户最初提出的问题 （sys.query） 是否包含特定的关键词（如“转人工”、“投诉”、“换个活人”）。

1.  最终应答 （Answer Nodes）:

+   **如果条件满足 （T**rue）   : 即**用户表达了转人工**或投诉的意图，流程会导向一个特定的“回答”节点，该节点直接输出一句固定的转接提示语（例如：“好的，正在为您转接人工客服，请稍候。”) .

+   **如果条件不满足 （**False）: 即用户只是进行常规提问，流程会导向另一个“回答”节点，该节点将步骤 3 中 LLM 生成的那个回复作为最终答案输出给用户。

**整体流程概览：** 这个 Chatflow 实现了一个智能客服的常见模式：尝试用 RAG + LLM 自动回答 -> 同时检查用户是否想转人工 -> 根据检查结果决定是给出 AI 的回答还是转接提示。

![](img/9d939bade4ce9b2c26d09bd31e626368.png)

### 6.4.3 在 Dify 中构建 Chatflow

现在，我们来详细描述如何在 Dify 的可视化界面中一步步搭建这个“易速客服”Chatflow：

1.  创建新 Chatflow 应用：

+   在 Dify 工作室中，点击“创建应用”。

+   选择应用模式为 **“Chatflow ”** 。

+   为应用命名，例如 易速客服。

+   选择一个合适的图标和背景色，例如 🛎️ 和 #FFEAD5。

+   点击创建，进入 Chatflow 的可视化编排画布。

1.  **熟悉画布与默认节点：**

+   您会看到画布上通常预置了“**开始 （Start）**”节点和“**回答 （Answer）**”节点。Chatflow 的流程最终都需要连接到某个“回答”节点才能将信息输出给用户。

1.  **添加并配置知识检索节点：**

+   从左侧的节点库中，找到“知识检索 （Knowledge Retrieval）”节点，将其拖拽到画布上。

+   用鼠标从“开始”节点的右侧锚点拖出一条连接线，连接到“知识检索”节点的左侧输入锚点。

+   点击选中“知识检索”节点，右侧会弹出配置面板：

+   **选择知识库：** 点击“选择数据集 （Select Dataset）”或类似按钮，在弹出的列表中勾选您准备好的、包含“易速鲜花”信息的知识库。

+   **设置查询变量：** 在“查询变量 （Query Variable）”输入框或下拉菜单中，选择代表用户当前输入的系统变量，通常是 sys.query 或类似名称。

![](img/ae7c9bfab7ea7db3d0245839f9291555.png)

1.  **添加并配置 LLM 节点：**

+   从节点库拖拽一个“**LLM**”节点到画布上。

+   将“知识检索”节点的右侧输出锚点连接到“LLM”节点的左侧输入锚点。

+   点击选中“LLM”节点进行配置：

+   **选择模型：** 在“模型 （Model）”下拉菜单中，选择您配置好的 deepseek-chat。

+   **配置上下文：** 找到“上下文 （Context）”设置区域，勾选“启用 （Enabled）”。在下方的“变量选择器 （Variable Selector）”中，选择来自上游“知识检索”节点的输出变量（通常名为 result 或 context，Dify 会自动列出可选的上游输出）。

+   **编写 Prompt:** 在“提示词 （Prompt）”区域（通常是 System Prompt 输入框），粘贴以下优化后的 Prompt：

```
# 角色
你是一位名叫“小易”的“易速鲜花”智能客服代表，你的目标是友好、专业、高效地帮助用户解决问题。

# 主要任务与信息来源1.  当用户提出问题 (`{{sys.query}}`) 时，**优先**查阅我提供给你的“已知信息 (`{{context}}`)”（来自公司知识库）。 (*请根据 Dify 实际变量语法调整 {{sys.query}} 和 {{context}}*)
2.  **如果“已知信息”足够回答用户问题，请完全基于这些信息进行回答。**
3.  **如果“已知信息”为空或不包含答案，你可以谨慎地运用你自己的通用知识来尝试回答。** 但在这样做时，最好能暗示信息来源（例如，“根据我的了解...”或“通常来说...”），避免让用户误以为这是公司的官方信息。
4.  **对于涉及个人账户、订单修改、复杂投诉处理等敏感或需要权限的操作，请告知用户需要转接人工客服处理。**
5.  **如果用户的问题模糊不清，可以主动提问以获取更多细节。** 例如：“请问您具体指的是哪个订单号呢？”或“能详细描述一下您遇到的问题吗？”

# 回答风格
*   语气友好、耐心、专业。
*   表达清晰、简洁。
*   适当使用 Emoji (如 😊🌸) 增加亲和力，但避免过度使用。

# 重要约束
*   在你的回答中，不要直接提及“已知信息”或“上下文”。
*   **除非用户明确要求或符合上述第 4 点需要权限的情况，否则不要主动建议转接人工客服。** （转人工的判断主要由后续的条件节点处理）。
```

+   **注意：** 再次强调，Prompt 中的变量引用语法 {{sys.query}} 和 {{context}} 需要与您 Dify 界面提供的实际语法一致。请使用界面上的变量插入功能来确保正确性。

![](img/e2d99eb2e4d4be6e4f475ff8828ef7b5.png)

1.  **添加并配置条件分支节点：**

+   从节点库拖拽一个“**条件分支 （If/Else）**”节点到画布上。

+   将“LLM”节点的右侧输出锚点连接到“条件分支”节点的左侧输入锚点。（或者，如果判断逻辑基于原始用户输入，则可以将“开始”节点直接连接到“条件分支”节点，或者从“条件分支”节点内部选择 sys.query 作为判断变量，后者更常见）。

+   点击选中“条件分支”节点进行配置：

+   **设置判断逻辑：** Dify 的条件节点通常允许添加多个 Case （分支）。我们需要设置一个 Case 来判断用户是否想转人工。

+   点击“添加条件 （Add Condition）”或类似按钮来定义第一个分支（例如，可以命名为 "转人工意图"）。

+   **选择判断变量：**** 在条件设置中，选**择要判断的变量。这里我们应该判断用户本轮的原始输入，所以选择系统变量 sys.query。

+   **添加判断规则：** 添加三个判断规则，并使用 **“或者 （OR）”** 逻辑连接它们：

+   规则 1: 变量 sys.query **包含 （contains）** 值 "转人工"

+   规则 2: 变量 sys.query **包含 （contains）** 值 "投诉"

+   规则 3: 变量 sys.query **包含 （contains）** 值 "换个活人"

+   这个分支代表了 true 的情况。所有不满足这些条件的情况将自动走向默认的 false （或 "Else"） 分支。

![](img/fdabf90a8b70637ff75a86d82785c74c.png)

1.  **配置转人工应答节点：**

+   从节点库拖拽一个**新的**“**回答 （Answer）**”节点到画布上。

+   将“条件分支”节点代表 **true** （满足转人工条件） 的那个**输出锚点**连接到这个新“回答”节点的输入锚点。

+   点击选中这个新的“回答”节点进行配置：

+   给它一个清晰的标题，例如“**转人工响应**”。

+   在“回答内容 （Answer Content）”或类似设置中，选择“固定文本 （Static Text）”类型。

+   在文本框中输入准备好的转接语：“好的，正在为您转接人工客服，请稍候。”

![](img/4cccd50c9ba07a7b59d61d2aa1e7ce9d.png)

1.  **配置 AI 应答节点：**

+   找到画布上预置的那个“回答 （Answer）”节点 （或者如果被删除了就再拖一个）。

+   将“条件分支”节点代表 **false** （不满足转人工条件） 的那个**输出锚点**连接到这个“回答”节点的输入锚点。

+   点击选中这个“回答”节点进行配置：

+   可以给它一个标题，例如“**AI 自动回复**”。

+   **在“回答内容 （Answe**r Content）”设置中，选择“变量 （Variable）”类型。

+   **在“变量选择器 （Var****iabl**e Selector）”中，选择来自 LLM 节点的输出变量 text （例如 {{#llm.text#}} 或类似语法）。

![](img/c5db554737bb2a255ed4f15ec2789cc2.png)

1.  **检查连接与保存：**

+   仔细检查所有节点的连接线是否都按照预期的逻辑连接。确保没有断开的连接或错误的流向。

+   点击画布右上角的“**发布 （Publish）**”或“**保存 （Save）**”按钮来保存您的 Chatflow 配置。

### 6.4.4 运行与测试 Chatflow

1.  使用 Dify 提供的对话预览窗口或发布后进行测试。

1.  **测试正常问答：**

+   提问知识库中有的问题（如“你们送货范围是哪里？”“玫瑰花多少钱一束？”) . 观察是否能基于知识库准确回答。

+   提问知识库没有但 LLM 可能知道的常识问题。观察 LLM 是否能给出通用回答。

+   提问模糊问题，观察 LLM 是否会要求澄清。

1.  **测试转人工逻辑：**

+   直接输入：“我要转人工”、“我要投诉”、“能不能换个活人跟我说”。观察是否输出了固定的转接语，并且流程走向了“转人工回答”节点。

1.  **多轮对话测试：** 尝试进行几轮连续问答，观察 Chatflow 是否能理解上下文（即使我们禁用了显式记忆窗口，Chatflow 模式本身可能有一定的上下文能力，需要实际测试验证）。

![](img/1f28779f9c203d601fcd90b541f7304f.png)

通过这个案例，您可以掌握 Chatflow 的核心构建方法，特别是如何结合 RAG、LLM 和条件判断来处理对话中的不同情况，实现更智能的交互流程。

好的，我们来更新第六章的后续内容，从 6.6 节开始，并确保内容涵盖了 Workflow 和 Chatflow 两种模式。

---

## 6.5 高级工作流/Chatflow 概念与最佳实践

掌握了 Workflow 和 Chatflow 的基本构建方法后，我们还需要了解一些高级概念和最佳实践，以构建更健壮、更高效的应用。

### 6.5.1 错误处理 （Error Handling）

在实际运行中，工作流或聊天流的节点可能会遇到各种错误，例如：

+   LLM API 调用超时或返回错误。

+   工具节点调用的外部 API 不可用或返回无效数据。

+   知识检索失败或返回空结果。

+   节点处理的数据格式不符合预期。

健壮的流程设计必须考虑这些异常情况：

+   利用 Dify 的错误处理机制： 检查 Dify 是否为节点提供了专门的“失败”或“错误”输出锚点。您可以将这些锚点连接到特定的处理路径，例如：

+   连接到一个 LLM 节点，生成一条向用户道歉并说明情况的友好消息（尤其是在 Chatflow 中）。

+   连接到一个工具节点，尝试调用备用 API 或记录错误日志。

+   连接到一个变量赋值节点，设置一个表示失败状态的变量，供后续节点判断。

+   **在流程中显式检查：** 使用“条件判断”节点检查上一步操作是否成功（例如，检查工具节点的输出变量是否为空或包含错误代码），然后根据结果决定后续流程。

+   **设计容错逻辑：** 对于非关键步骤的失败，考虑是否可以跳过该步骤继续执行，或者提供一个默认值/备用方案。

### 6.5.2 优化 （Optimization）

复杂的流程可能会消耗较多的时间和计算资源（特别是 LLM 调用通常成本较高）。优化是提高效率和降低成本的关键：

+   **减少不必要的 LLM 调用：** 审视流程，思考是否可以用更简单的节点（如变量赋值、条件判断、甚至简单的代码工具）来替代某些原本由 LLM 完成的逻辑判断或数据转换任务。

+   **Prompt 优化：** 对于必须使用的 LLM 节点，优化 Prompt 以获得更简洁、更精确的输出，减少 Token 消耗和处理时间。明确指示输出格式。

+   并行执行 （Parallel Execution - 如支持）: 如果工作流中存在互不依赖的分支或步骤，检查 Dify 是否支持并行执行这些步骤，这可以显著缩短整个流程的总耗时。

+   缓存 （Caching - 如支持）: 对于某些输入相同则输出也相同的确定性节点（如调用某些幂等的工具 API），如果 Dify 支持节点级别的缓存，可以启用它来避免重复计算。

+   **Chatflow 特定优化：** 对于 Chatflow，还需要考虑对话的流畅性。避免让用户等待过长时间，如果某个步骤耗时较长，可以先给用户一个提示（例如“正在为您查询，请稍候...”) . 优化 Prompt 以生成更自然、更符合对话逻辑的回复。

### 6.5.3 发布为 API

精**心设计的 Workflo**w 或 Chatflow 代表了一个封装了复杂逻辑和智能能力的自动化单元。Dify 通常允许您将构建好的流程发布为一个 API 端点。

+   **价值：** 这使得其他应用程序、系统或脚本可以通过一个简单的 HTTP 请求来触发和利用这个复杂的 AI 处理能力，而无需关心其内部实现细节。例如，您可以将“花语小能手”Workflow 发布为 API，供电商网站在展示花卉商品时动态调用生成推广文案；或者将“易速客服”Chatflow 发布为 API，集成到网站的在线客服插件中。

+   **配置：** 发布时通常需要配置 API 的认证方式（如使用 API Key），并会获得一个唯一的 API Endpoint URL。

### 6.5.4 何时选择 Workflow vs。 Chatflow vs. Agent （决策指南）

回顾 6.2.3 节的对比，这里提供一个简单的决策参考：

+   需要自动化执行固定**步骤、批处理或生**成结构化输出？ -> 优先考虑 Workflow。

+   需要构建交互式对话**应用，处理多轮问**答，根据用户反馈引导流程？ -> 优先考虑 Chatflow。

+   **任务目标明确，但实现路径需要动态规**划，需要灵活组合多**种工具与外**部环境交互？ -> 优先考虑 Agent。

+   需要精确控制每一步逻辑和数据流？ -> Workflow/Chatflow 比 Agent 更优。

+   希望利用** LLM **的强大推理和自主性？ -> Agent 比 Workflow/Chatflow 更能发挥。

实践中，选择往往取决于具体任务的性质、对可控性和灵活性的要求，以及开发者的偏好。有时也可以混合使用（例如 Workflow 调用 Agent）。

## 6.6 工作流/Chatflow 应用场景实例 （拓展）

结合 Workflow 和 Chatflow 的能力，可以实现更多样化的应用：

Workflow （自动化/批处理） 场景：

+   **复杂数据抽取与报告：** 从上传的财报 PDF （知识检索） -> 调用 LLM 多次抽取关键财务数据 -> 调用代码工具或 API 进行计算 -> 调用 LLM 生成分析摘要和图表描述 -> 组合成结构化报告 （变量赋值/LLM） -> 输出报告文件。

+   **多语言内容本地化：** 输入英文产品描述 -> 调用 LLM 翻译成多种目标语言 -> （可选） 调用不同 LLM 或 Prompt 进行风格调整 -> 输出各种语言版本。

+   **简历筛选与初步评估：** 输入岗位描述和候选人简历文本 -> 调用 LLM 抽取关键技能、经验与岗位要求的匹配度 -> 调用条件判断根据匹配度打分或分类 -> 输出初步筛选结果。

+   **定时数据监控与预警：** （通过外部触发器启动）-> 调用工具 API 获取监控数据 -> 调用条件判断检查是否超阈值 -> （如果超标） 调用 LLM 生成预警信息 -> 调用工具 API 发送通知 （邮件/短信）。

Chatflow （对话式） 场景：

+   **交互式产品配置器：** 用户说出需求 -> Chatflow 通过多轮提问（LLM/条件判断）收集具体配置选项（颜色、尺寸、功能）-> （可选） 调用工具检查库存或兼容性 -> 最终生成配置清单或报价。

+   **个性化学习计划生成：** 用户说明学习目标和当前水平 -> Chatflow 通过提问了解学习偏好 -> （可选） 调用知识检索查找相关学习资源 -> 调用 LLM 生成个性化的学习步骤和资源推荐 -> 用户可反馈调整。

+   复杂的 RAG 问答 （带澄清）: 用户提出模糊问题 -> Chatflow （LLM/条件判断） 判断是否需要澄清 -> 如果需要，向用户提问获取更多细节 -> 重新进行知识检索 -> LLM 基于更明确的信息和上下文生成答案。

+   **游戏 NPC 或虚拟伴侣：** 需要理解长期对话历史、展现一致性格、并在对话中触发特定行为或小游戏（可能调用工具或子流程）。

## 6.7 本章小结

本章我们深入探索了 Dify 提供的可视化编排能力，特别是其 Workflow （自动化模式） 和 Chatflow （对话式模式）。我们首先理解了这两种模式的核心概念，它们都允许通过连接功能节点来构建复杂应用，但 Workflow 更侧重于自动化和批处理，而 Chatflow 则专为交互式对话场景设计。我们详细对比了它们与 Agent 模式在控制方式、灵活性和适用场景上的关键差异。

通过构建“**花语小能手**”（Workflow 实例），我们实践了如何结合知识检索**和 LL**M 进行创意内容生成。通过构建“易速客服”（Chatflow 实例），我们体验了如何处理对话输入、利用 RAG 和 LLM 回答问题，并通过条件判断实现智能路由。这两个案例帮助我们掌握了在 Dify 中创建、配置和调试可视化流程的基本操作。

此外，我们还探讨了错误处理、流程优化、发布为 API 等高级概念和最佳实践，并拓展了解了 Workflow 和 Chatflow 在企业自动化、智能客服、个性化服务等方面的广泛应用潜力。

掌握 Dify 的 Workflow 和 Chatflow 功能，将使您能够根据具体需求，选择最合适的工具，构建出逻辑清晰、功能强大、稳定可控的企业级 AI 应用。

## 6.8 实践练习

1.  完成并测试“花语小能手”（Workflow）: 严格按照 6.4.3 的步骤在 Dify 中构建工作流，并使用您自己的花语知识库进行测试。尝试不同的花名（包括知识库中没有的），评估生成帖子和“优雅拒绝”回复的质量。

1.  完成并测试“易速客服”（Chatflow）: 严格按照 6.5.3 的步骤在 Dify 中构建 Chatflow。使用包含“易速鲜花”信息的知识库进行测试。模拟用户进行正常咨询、提问知识库没有的问题，以及明确提出“转人工”或“投诉”，观察 Chatflow 的响应是否符合预期。

1.  **为“花语小能手”加入条件判断：** 尝试修改“花语小能手”Workflow：在“知识检索”节点后加入一个“条件判断”节点，检查检索结果是否为空。如果为空，连接到一个新的 LLM 节点，生成“抱歉，未找到该花信息”的回复；如果不为空，继续连接到原有的创作帖子的 LLM 节点。测试这个改进后的流程。

1.  **为“易速客服”增加功能：** 尝试修改“易速客服”Chatflow：

+   在 LLM 回答之前，加入一个“问题分类”节点，判断用户问题属于“产品咨询”、“订单问题”还是“其他”。

+   在 LLM 节点的 Prompt 中，根据分类结果（{{#question_classifier.category#}}）调整回答的侧重点或知识库查询策略（如果 Dify 支持动态调整知识库）。

+   或者，在条件分支中增加更多路由逻辑，例如如果分类是“订单问题”，则尝试调用一个（概念上的）订单查询工具。

1.  **设计新流程 （选择模式）:**** 构思一个新场景，例如“会议纪要自动生成与任务**分配”。首先判断：这个场景更适合用 Workflow 还是 Chatflow 来实现？为什么？ 然后，绘制出您选择模式下的流程图，包含必要的节点（如开始、LLM、可能的工具节点、结束），并简要描述每个节点的功能和关键配置。

# 第七部分：企业级考量与实践 （Enterprise Considerations & Practices）

将 Dify 构建的 AI 应用从原型投入到企业实际生产环境，需要考虑一系列集成、安全、运维和优化方面的问题。本章将探讨这些关键的企业级考量点。

## 7.1 集成与扩展 （Integration and Extension）

将 Dify 应用融入现有企业生态系统是发挥其价值的关键。

### 7.1.1 API 对接企业系统

Dify 的核心集成方式是 **API**。无论是基础聊天机器人、RAG 应用，还是复杂的 Workflow/Chatflow，都可以一键发布为标准的 RESTful API（如我们在第三章和第六章末尾所见）。这意味着：

+   **现有系统调用 Dify:** 企业内部的 ERP， CRM， OA， 企业🛰️/钉钉机器人等系统，可以通过调用 Dify 应用的 API 端点，来利用其 AI 能力（例如，OA 系统调用“易速客服”Chatflow API 来处理员工的 IT 求助）。

+   Dify 调用现有**系统 （通过 Age**nt）: 反过来，通过创建自定义 API 工具（如第五章所述），Dify Agent 可以主动调用企业现有系统的 API，实现数据的双向交互和流程的驱动（例如，Agent 调用 CRM API 创建销售线索，或调用订单系统 API 查询订单状态）。

### 7.1.2 Webhook 与回调机制

除了主动 API 调用，还可以考虑使用 Webhook（如果 Dify 或配合使用的工具支持）。

+   **Dify 触发外部系统：** Dify 中的某个事件（例如 Workflow 完成、Agent 检测到特定情况）可以配置为触发一个 HTTP POST 请求到外部系统的 Webhook URL，通知外部系统执行后续操作。

+   **外部系统触发 Dify:** 外部系统发生的事件（例如 CRM 中创建了新客户）可以通过调用 Dify 应用的 API 来触发相应的 AI 处理流程。

### 7.1.3 二次开发与定制化

Dify 是一个**开源项目**，这为需要深度定制的企业提供了极大的灵活性：

+   **功能扩展：** 企业可以根据自身需求修改 Dify 源代码，增加新的节点类型、内置工具或特定的集成模块。

+   **界面定制：** 可以定制 Dify 的前端界面，以符合企业品牌形象或嵌入到内部平台。

+   **私有化部署优化：** 针对特定的基础设施环境进行性能调优或安全加固。
进行二次开发需要相应的技术能力，但它提供了超越平台标准功能的最大可能性。

## 7.2 安全与合规 （Security and Compliance）

在企业环境中使用 AI，安全与合规是重中之重。

### 7.2.1 私有化部署的数据安全保障

这是 Dify 对于企业的核心优势之一（已在第二章强调）。通过 Docker 将 Dify 完整部署在企业内网或私有云：

+   **数据主权：** 所有用户输入、知识库内容、模型交互数据都保留在企业控制范围内，满足数据本地化和隐私保护要求。

+   **网络隔离：** 可以将 Dify 置于防火墙后，与公网隔离，减少外部攻击面。

+   **合规性：** 更容易满足特定行业（金融、医疗、政务）的严格数据合规监管要求。

### 7.2.2 访问控制与权限管理

Dify 平台自身通常提供用户管理和基础的权限控制：

+   **用户账户：** 可以创建不同的用户账号登录 Dify 平台。

+   角色与权限 （视 Dify 版本）: 可能支持定义不同的角色（如管理员、开发者、运营者），并分配不同的应用访问、编辑、知识库管理等权限。精细的权限控制对于团队协作和安全管理至关重要。

### 7.2.3 API 密钥安全管理

AI 应用通常涉及多种 API 密钥：

+   **LLM API Keys:** 连接如 DeepSeek、OpenAI 等服务的密钥。

+   **工具 API Keys:** Agent 使用的自定义工具或内置工具（如搜索引擎）可能需要的密钥。

+   **Dify **应用 API Keys: 发布 Dify 应用供外部调用时生成的认证密钥。
最佳实践：

+   **最小权限原则：** 为每个应用或工具生成独立的、权限最小化的密钥。

+   **安全存储：** 利用 Dify 提供的安全配置机**制（如环境**变量、密钥管理功能）存储密钥，严禁硬编码在 Prompt、代码或配置文件中。

+   **定期轮换：** 按照安全策略定期更换密钥。

+   **访问控制：** 限制对存储密钥的环境或配置的访问权限。

### 7.2.4 敏感信息过滤与脱敏

用户输入或知识库文档中可能包含个人身份信息 （PII）、商业机密等敏感内容。需要采取措施防止这些信息被泄露或不当使用：

+   **知识库预处理：** 在上传文档到知识库之前，进行脱敏处理，删除或替换敏感信息。

+   **Prompt 输入过滤：** 在将用户输入传递给 LLM 或 Agent 之前，实现一个过滤层，识别并屏蔽或替换掉敏感词汇。这可能需要在 Dify 外部或通过自定义工具/代码节点实现。

+   **LLM 输出过滤：** 对 LLM 的生成结果进行检查，防止其意外泄露训练数据中的或上下文中的敏感信息。

+   **选择合规的 LLM:** 对于处理高度敏感数据的场景，优先选择明确承诺数据隔离、不使用用户数据进行再训练的商业 LLM 或完全私有化部署的模型。

### 7.2.5 审计日志与追踪

为了满足合规要求和进行问题排查，完善的日志记录至关重要：

+   **Dify 应用日志：** Dify 通常会记录应用的调用历史、用户输入、AI 输出、Token 消耗等（如第三章日志与标注部分所述）。

+   **系统操作日志：** 记录用户在 Dify 平台上的关键操作（如创建/修改应用、管理知识库、配置模型等）。

+   **工具调用日志：** Agent 或 Workflow 中工具节点的调用情况、输入输出、成功或失败状态。
确保日志被妥善存储、可查询，并根据合规要求设定保留期限。

## 7.3 运维与监控 （Operations and Monitoring）

保障 Dify 平台及其应用的稳定运行，需要持续的运维和监控。

### 7.3.1 监控 Dify 服务状态

基于 Docker 的私有化部署，需要监控底层基础设施和核心组件：

+   **容器状态：** 使用 docker ps 或 Docker 监控工具（如 Portainer， Datadog， Prometheus+Grafana 等）确 Dify 的 api， web， worker 以及依赖的 db， redis， vector database 容器都处于健康运行状态。

+   **资源使用：** 监控服务器的 CPU、内存、磁盘 I/O 和网络带宽使用情况，确保资源充足。特别关注数据库和向量数据库的资源消耗。

+   **依赖服务健康度：** 监控 PostgreSQL， Redis 以及向量数据库自身的性能指标和日志。

### 7.3.2 应用调用量与 Token 消耗监控

利用 Dify 内置的“**监测 （Monitoring）**”功能（如第六章末尾提及）：

+   **跟踪关键指标：** 监控每个应用的请求量、平均响应时间、错误率。

+   **成本控制：** 密切关注 **Token 消耗量**，结合模型价格估算运营成本。识别 Token 消耗异常的应用或 Prompt。

### 7.3.3 知识库更新与维护策略

对于 RAG 应用，知识库的时效性和准确性直接影响效果：

+   **制定更新计划：** 根据知识变化的速度，确定知识库的更新频率（例如，产品手册更新后立即更新，FAQ 每月审查一次）。

+   **自动化更新：** 尽可能通过 API 或脚本实现知识库的自动化同步和更新，减少人工操作。

+   **版本控制：** 对知识库文档进行版本管理，方便追溯和回滚。

+   **定期重新索引：** 更新文档后，需要触发 Dify 的重新分块和向量化（重新索引）流程。根据数据量大小，合理安排重新索引的时间窗口。

+   **质量监控：** 定期抽查 RAG 应用的问答效果，评估知识库质量。

### 7.3.4 Dify 版本升级

Dify 作为开源项目会不断迭代：

+   **关注官方发布：** 留意 Dify 的新版本发布说明，了解新功能、性能改进和安全修复。

+   **备份数据：** 在升级前，务必备份 Dify 的数据库、配置文件和数据卷。

+   **遵循升级指南：** 仔细阅读并遵循官方提供的版本升级步骤（通常涉及拉取新的 Docker 镜像 docker compose pull，然后重新启动服务 docker compose up -d --remove-orphans，有时可能需要执行数据迁移脚本）。

+   **测试环境验证：** 最好先在测试环境中验证升级过程和新版本的功能，再在生产环境中实施。

## 7.4 成本与性能优化 （Cost and Performance Optimization）

在满足业务需求的前提下，控制成本和提升性能是持续优化的目标。

### 7.4.1 选择合适的 LLM

+   **成本与效果平衡：** 不同的 LLM（如 DeepSeek， GPT-4， GPT-3.5， 开源模型 Llama 等）在能力、速度和价格上差异巨大。根据应用场景选择“足够好”的模型，而非一味追求最强模型。例如，简单的文本分类任务可能用轻量级模型即可，而复杂的分析报告生成则需要更强的模型。

+   **API vs. 私有化：** 使用商业 LLM API 需要按量付费（Token），成本可预测但可能较高；私有化部署开源 LLM 需要投入硬件和运维成本，但使用成本较低（边际成本）。

+   **针对性选择：** Dify 支持多模型，可以在同一个应用的不同节点（如 Workflow 中）或不同应用中使用不同的模型。

### 7.4.2 优化 Prompt 减少 Token 消耗

Prompt 的长度直接影响 LLM API 的成本和响应时间：

+   **简洁明了：** 尽量用最少的文字清晰地表达指令。

+   减少冗余示例 （Few-shot）: 如果 Zero-shot 或简单的指令就能达到效果，避免使用过长的 Few-shot 示例。

+   **优化上下文长度：** 在 Chatflow 或 RAG 应用中，合理设置对话历史长度或 RAG 召回的 Top K 值，避免将过多不必要的上下文传给 LLM。

+   **指令调整：** 有时微调指令，让 LLM 输出更简洁的答案，也能节省 Completion Tokens。

### 7.4.3 RAG 检索效率优化

RAG 的检索阶段也会影响整体性能：

+   **优化分块策略：** 合理的 Chunk Size 和 Overlap 可以提高检索精度，减少召回无关块。

+   **选择高效的嵌入模型：** 嵌入模型的选择不仅影响效果，也影响向量化所需的时间和计算资源。

+   **向量数据库调优：** 选择合适的向量数据库（Weaviate， Qdrant， Milvus 等）并根据数据量和查询负载进行参数调优（如索引类型、副本数、资源配置等）。这通常在 Dify 外部进行，但对 Dify RAG 性能至关重要。

+   **Reranker 的权衡：** Reranker 能提高精度，但会增加额外的计算开销和延迟。根据实际效果评估是否必须使用。

### 7.4.4 Agent/Workflow 优化

+   **工具调用效率：** 确保 Agent 或 Workflow 调用的自定义 API 工具本身是高效、低延迟的。

+   **减少 LLM 决策步骤：** 在 Workflow 中，优先使用确定性节点（条件判断、变量赋值）处理逻辑，减少不必要的 LLM 推理步骤。对于 Agent，优化 Prompt 引导其更直接地选择工具和行动，减少思考轮次。

+   **成本考量：** 评估 Agent/Workflow 中每个工具调用和 LLM 调用的成本。

---

# 第八部分：更多企业应用场景探索 （Exploring More Enterprise Use Cases）

基于 Dify 提供的 RAG、Agent、Workflow/Chatflow 等核心能力，企业可以在众多场景中构建创新的 AI 应用。以下是一些示例，并指出了主要依赖的 Dify 功能：

+   **智能客服 （Intelligen**t Customer Service）:

+   **实现：** 利用 **Chatflow** 进行多轮对话管理，**RAG** 提供产品/服务知识问答，**Agent 工具** 调用 CRM/订单系统查询信息或创建工单。

+   **价值：** 提供 7x24 服务，快速解答常见问题，自动化处理简单请求，提升客户满意度，降低人工成本。

+   **内部知识管理 （Internal Kn**owledge Management）:

+   **实现：** 基于 **RAG** **构建统一的内部知**识库问答入口，整合公司政策、流程文档、项目资料、技术文档等。可以通过简单的对话应用或集成到 Chatflow 中提供更智能的查询体验。

+   **价值：** 提升员工获取信息的效率，减少重复提问，促进知识共享。

+   **文档处理自动化 （Document Pro**cessing Automation）:

+   **实现：** 利用 **Workflow** 或 **Agent** 结合 **LLM** 的文本理解与生成能力。例如，Workflow 读取报告模板和数据源 （通过工具或知识库），调用 LLM 填充内容生成报告；Agent 或 Workflow 调用 LLM 对合同文本进行关键条款提取和风险点提示 （可能结合 RAG 提供法律知识）。

+   **价值：** 自动化处理繁琐的文档工作，提高效率，减少人为错误。

+   **营销内容生成 （Marketing **Content Generation）:

+   **实现：** 利用 **Workflow** 或简单的 **LLM 应用**。例如，“花语小能手”就是生成社交媒体帖子的例子。可以构建 Workflow 输入产品特点和目标受众，调用 LLM 生成广告文案、邮件营销内容、博客文章草稿等。

+   **价值：** 加速内容创作过程，提供创意灵感，实现个性化营销。

+   数据分析助手 （Data Analysis Assistant）:

+   **实现：** 利用 **Agent** 配置**自定义工具** （API 或代码工具） 连接数据库或数据分析平台。用户可以通过自然语言提问（如“查询上个月华东区的销售额”），Agent 理解意图 -> 调用工具执行查询 -> LLM 对结果进行解释或可视化描述。

+   **价值：** 降低数据访问门槛，使非技术人员也能快速获取数据洞察。

+   代码助手 （Code Assistant）:

+   **实现：** 利用 **Agent** 结合 **LLM**** 强大的代**码生成、解释和调试能力。可以配置自定义工具 （如代码执行沙箱 - 需极度谨慎，或调用静态分析工具 API） 来辅助验证或测试。

+   **价值：** 提高开发效率，辅助代码审查，降低 bug 风险。

+   流程自动化 （RPA Enhancement）:

+   **实现：** 将 Dify **Agent** 或 **Workflow** 作为智能决策中心，调用现有的 RPA 工具 API 或通过自定义工具执行脚本，驱动更复杂的、需要认知判断的业务流程自动化。

+   **价值：** 为传统 RPA 注入智能，处理更复杂的非结构化数据和模糊任务。

这些场景展示了 Dify 作为 LLMOps 平台，能够支持企业在不同业务领域构建多样化、具有实际价值的 AI 应用。

# 第九部分：总结与展望 （Conclusion and Outlook）

经过前面章节的学习和实践，我们已经系统地了解了如何利用 Dify 这一强大的 LLMOps 平台来构建和部署企业级的 AI 应用。

**价值回顾：**

+   **简化开发、加速落地：** Dify 通过可视化编排 （Workflow/Chatflow）、内置 RAG 引擎、灵活的 Agent 框架，极大地降低了 AI 应用的开发门槛，让企业能够快速将想法转化为实际应用。

+   **数据安全与自主可控：** 对私有化部署的良好支持，使得企业可以将数据和应用掌控在自己手中，满足严格的安全与合规要求。

+   **灵活性与可扩展性：** 开源特性、自定义工具、多模型支持以及 API 集成能力，为企业提供了根据自身需求进行定制和扩展的空间。

**未来展望：**

AI 和 LLMOps 领域仍在飞速发展，我们可以期待 Dify 和类似平台在未来可能带来：

+   **更强的多模态能力：** 支持处理和生成图像、音频、视频等多模态内容的应用。

+   **更智能、更自主的 Agent:** 具备更强的长期记忆、复杂规划和跨应用协作能力。

+   **更完善的评估与监控：** 提供更精细化的应用性能评估、成本分析和 A/B 测试框架。

+   **更深度的企业集成：** 与更多企业级软件和数据平台实现开箱即用的连接。

+   **持续优化的模型支持：** 更快地集成最新的开源和商业 LLM。

**学习资源与社区：**

要继续深入学习和使用 Dify，您可以利用以下资源：

+   **Dify 官方文档：** https://docs.dify.ai/ （这是最权威、最及时的信息来源）

+   **Dify GitHub 仓库：** https://github.com/langgenius/dify （可以查看源码、提交 Issue、了解最新开发动态）

+   **Dify 社区：** 关注官方论坛、Discord/Slack 频道（如有），与其他用户交流经验、寻求帮助。

**结语：**

Dify 为企业拥抱生成式 AI、实现智能化转型提供了一个强大而易用的平台。通过本课程的学习，您已经掌握了使用 Dify 构建从简单对话到复杂自动化流程的各种 AI 应用的核心技能。希望您能将所学知识应用到实际工作中，创造出更多有价值的解决方案。

# 附加实验

## 构建 Workflow - 天气预报查询与格式化 (Practical Case: Weather Forecast Workflow)

这个实践案例将演示如何构建一个相对复杂的 Workflow，它不仅利用知识库和 LLM，还集成了外部工具（高德天气）和代码执行节点，来完成一个从输入地区名到输出格式化天气预报的完整流程。

### 场景定义：智能天气查询与播报

+   **目标:** 用户输入一个**地区名称**，工作流首先利用**知识库**和 **LLM**** 将其转换为高德地图 API **能识别的 **adcode**** (行政区划****代码)，然后调用高德天气**工具查询该地区的天气预报（通常包含未来几天的数据），最后通过代码执行节点解析返回的 JSON 数据，提取并格式化输出今天的天气信息。

+   **输入:** input (字符串，用户输入的地区名称，如“北京”、“上海市浦东新区”)

+   **输出:** output (字符串，格式化后的今日天气预报，例如：“今日天气，白天晴，温度25摄氏度，晚上多云，气温18摄氏度”)

+   **依赖:**

+   一个或多个包含地区名称与 a**dco**de 对应关系的 Dify 知识库。（https://a.amap.com/lbs/static/code_resource/AMap_adcode_citycode.zip）

+   配置好的 **LLM** (例如 deepseek-chat)。

+   Dify **内置的高德天气工具 (****gaode_weather****)** (需要确保在 Dify 市场中已安装并可能需要配置高德 API Key)。

+   Python 3 代码执行环境。

### Workflow 结构解析 (概述)

根据 DSL 文件，这个“天气预报”工作流的步骤如下：

1.  开始 (Star**t No**de): 接收用户输入的地区名称 (input)。

1.  知识检索 (Knowledge Retrieval Nod**e): 使用用户输入的地**区名称 (input) 在包含 adcode 信息的知识库中进行检索，并启用了 Reranker (BAAI/bge-reranker-v2-m3)，Top K=4，旨在精确找到与输入地区最相关的包含 adcode 的文本块。

1.  大语言模型 (LLM Node): 接收用户输入的地区名称 (input) 和知识检索结果 (context)。其核心任务是根据这两者，准确地提取或推断出该地区对应的 adcode。Prompt 指示它只需要输出 adcode 值，避免其他无关内容。使用的是 deepseek-chat 模型。

1.  工具 (Tool Node -** 高德天气)**: 接收上一步 LLM 输出**的 ad****code 作为 city ****参数，**调用内置的 gaode_weather 工具来查询天气信息。

1.  **代码执行 (Code N**ode): 接收高德天气工具返回的（通常是包含多天预报的）JSON 格式的文本结果 (text) 作为输入参数 arg1。执行一段 Python 代码，该代码负责：

+   解析输入的 JSON 字符串。

+   找到**今天**的天气数据。

+   提取白天天气、白天温度、夜晚天气、夜晚温度等关键信息。

+   将这些信息**格式化**成一句自然语言的中文天气预报。

+   处理可能出现的 JSON 解析错误、数据缺失或格式转换错误。

1.  **结束 (End Node):** 将代码执行节点输出的**格式化后的今****日**天气预报字符串 (result) 作为整个工作流的最终结果 (output) 返回。

**整体流程概览:** 用户输入地名 -> RAG+LLM 获取 adcode -> 调用天气 API 获取预报数据 (JSON) -> Python 代码解析 JSON 并格式化输出今日天气 -> 返回结果。这个流程巧妙地结合了多种技术来解决一个实际问题。

### 在 Dify 中构建 Workflow (可视化操作步骤)

在 Dify 的可视化界面中搭建这个“天气预报”工作流：

1.  **创建新 Workflow:** 在工作室创建“工作流 (Workflow)”应用，命名为 天气预报，选择图标（如 🌤️）。

1.  **配置开始节点:**

+   点击“开始”节点。

+   添加输入变量：input，类型 text-input，标签“输入地区名”，设为必需。

![](img/c692e4f804ef91bf48ebcda81e9b35e1.png)

1.  **添加并配置知识检索节点:**

+   拖入“知识检索”节点，连接“开始”节点。

+   **选择知识库:** 关联包含地区名和 adcode 信息的知识库。

+   **查询变量:** 选择 input。

![](img/a6b3eba77ec05fb11fefc6c4ca6be2f8.png)

1.  添加并配置 LLM 节点 (获取 Adcode):

+   拖入“LLM”节点，连接“知识检索”节点。

+   **选择模型:** deepseek-chat。

+   **配置上下文:** 启用，变量选择知识检索节点的 result。

+   **编写 Prompt:**

```
根据用户输入 {{#start.input#}} 和知识检索的结果 {{#knowledge_retrieval.result#}}，输出用户期望得到的adcode。只需要给出准确的adcode的值，禁止出现 “think” 相关内容。
```

![](img/12fefc1e344b9c1dae89eafdcb762cfd.png)

1.  添加并配置工具节点 (高德天气):

+   从节点库（可能在“内置工具”下）拖入“天气预报 (gaode_weather)”工具节点，连接“LLM”节点。

+   点击工具节点进行配置：

+   **工具名称/提供商:** 确认是 gaode_weather / gaode。

+   **配置参数:** 找到 city 参数的输入设置。

+   **映射变量:**** 将输入类型设为“变****量 (V**ariable)”，并选择来自上游 LLM 节点的输出 text (即 adcode) 作为 city 参数的值。

+   (可能需要配置高德 API Key，这通常在 Dify 的全局设置或该工具的配置中完成)

![](img/233171f57018e1029922bd98dc530e08.png)

1.  **添加并配置代码执行节点:**

+   从节点库拖入“**代码执行 (Code)**”节点，连接“天气预报”工具节点。

+   点击代码节点进行配置：

+   **标题:** 可以命名为“格式化天气结果”。

+   **代码语言:** 选择 Python 3。

+   **配置输入变量:** 添加一个输入变量 arg1**。将其值来源设置****为上游“**天气预报”工具节点的输出 text (即天气 API 返回的 JSON 字符串)。

+   **编写代码:**** 将 DS**L 文件中提供的 Python 代码完整地粘贴到代码编辑器中。这段代码负责解析 JSON、提取今日天气并格式化输出。

```
# -*- coding: utf-8 -*-
import json
import datetime
import re

def main(arg1: str) -&gt; dict:
    &quot;&quot;&quot;
    Extracts a JSON array from the beginning of arg1, finds today&#x27;s weather,
    formats it into a human-readable string, and returns it.
    Handles potential extra text after the JSON array.
    &quot;&quot;&quot;
    match = re.search(r&#x27;^(\[.*\])&#x27;, arg1.strip())
    if not match:
        return {&quot;result&quot;: &quot;Error: Could not find JSON array at the beginning of the input.&quot;}

    json_string_to_parse = match.group(1)

    try:
        report = json.loads(json_string_to_parse)
        today_date = datetime.date.today().strftime(&quot;%Y-%m-%d&quot;)

        if not isinstance(report, list):
             return {&quot;result&quot;: &quot;Error: Parsed JSON is not a list.&quot;}

        for weather in report:
            if isinstance(weather, dict):
                report_date = weather.get(&quot;date&quot;)
                if today_date == report_date:
                    # Found today&#x27;s weather, now format the output string
                    day_weather = weather.get(&quot;dayweather&quot;, &quot;未知&quot;) # Default value if key missing
                    day_temp_str = weather.get(&quot;daytemp_float&quot;, &quot;0&quot;) # Default to &quot;0&quot; string
                    night_weather = weather.get(&quot;nightweather&quot;, &quot;未知&quot;)
                    night_temp_str = weather.get(&quot;nighttemp_float&quot;, &quot;0&quot;)

                    # Safely convert temps to float and round
                    try:
                        day_temp_i = round(float(day_temp_str))
                    except ValueError:
                        day_temp_i = 0 # Default to 0 if conversion fails
                    try:
                        night_temp_i = round(float(night_temp_str))
                    except ValueError:
                         night_temp_i = 0

                    # Construct the result string using f-string (more modern)
                    res = f&quot;今日天气，白天{day_weather}，温度{day_temp_i}摄氏度，晚上{night_weather}，气温{night_temp_i}摄氏度&quot;
                    # Or using the % formatting like in your example:
                    # res = &quot;今日天气，白天%s，温度%d摄氏度，晚上%s，气温%d摄氏度&quot; % (day_weather, day_temp_i, night_weather, night_temp_i)

                    return {&quot;result&quot;: res} # Return the formatted string

        # If loop completes without finding today&#x27;s date
        return {&quot;result&quot;: f&quot;No weather information found for today ({today_date}) in the JSON data.&quot;}

    except json.JSONDecodeError:
        return {&quot;result&quot;: &quot;Error: Invalid JSON structure in the extracted part.&quot;}
    except ValueError:
        # Catch potential float conversion errors not caught inside the loop (less likely now)
         return {&quot;result&quot;: &quot;Error: Could not convert temperature value to number.&quot;}
    except Exception as e:
        return {&quot;result&quot;: f&quot;Error: {str(e)}&quot;}
```

+   **定义输出变量:** 代码的 return {"result": res} 语句定义了输出。Dify 通常会自动识别或允许您定义输出变量，确保有一个名为 result 的字符串类型输出变量。

![](img/2ba32ba47bdc64d07f42c23d8bfa9f6b.png)

1.  **配置结束节点:**

+   将“代码执行”节点的右侧输出锚点连接到“结束”节点的输入锚点。

+   点击“结束”节点进行配置。

+   **添加输出变量:** 定义一个输出变量，例如 output**。将其值来源****设置为上游“**代码执行”节点的输出 result。

![](img/dbf360d6fd7586917b39bc6cef3fa8be.png)

1.  **保存 Workflow。**

![](img/500f28e325ef87c74e146b0b1a784a27.png)

###  运行与测试 Workflow

1.  点击“运行”或“调试”。

1.  在输入框中输入一个地区名称，例如“闵行区”、“广州”、“杭州西湖区”等（确保您的 adcode 知识库包含这些信息）。

![](img/9c6863a6bdc46456f9500017540dd21f.png)

1.  点击执行。

1.  **分步检查节点输出:**

+   **知识检索:** 输出是否包含了与输入地区相关的、可能含有 adcode 的文本块？

![](img/b4e51905a9a2a17d183378319b6d2801.png)

+   LLM (**获取** Adcode): 输出是否仅仅是一个 6 位的数字 adcode？这是关键一步。

![](img/ca3bdba52f7cf3f12c9e9a89cd4dd0e6.png)

+   **天气预报工具:** 输出是否是一段看起来像 JSON 格式的天气预报数据？

![](img/12816c76f177398e8384129762a63ec7.png)

+   **代码执行:** 输出 (result) 是否是一句格式正确的中文今日天气预报？

![](img/7edfed0b3b4845fdbbb13196355ee34d.png)

+   **结束:** 最终输出是否与代码执行节点的 result 一致？

![](img/c1d4565f192a87d1fc415356d12d5adf.png)

1.  **测试不同地区:** 尝试输入不同的省、市、区县名称，测试流程的鲁棒性。

1.  **测试边界情况:** 输入知识库可能没有的地区，观察 LLM 节点能否正确处理（例如返回空值或错误提示），以及后续节点如何响应（代码节点应该能处理无效输入并返回错误信息）。

这个“天气预报” Workflow 案例充分展示了如何通过组合 Dify 的不同节点类型（RAG、LLM、工具、代码）来构建一个端到端的、解决特定问题的自动化流程。

## 发布MCP服务

将天气预报发布为工具

![](img/8f5cd564c85d168dacc43d9c6033a330.png)

按照下图所示，输入一个工具调用名称，并且描述写好，最好详细一点，能让大模型理解这个工具到底是干啥的。

![](img/1044d56d0019ea4362e0e2ff21a5df13.png)

从应用商店，安装MCP Compatible Dify Tools

![](img/4bd35da4da1aedc299437ebde81c8885.png)

注意dify的版本必须高于1.2.0 （本实验手册基于1.3.1）

![](img/238905eb1f40116501de9ad1bb751d79.png)

安装好之后，点击页面右上角的插件查看所有已安装插件列表

![](img/eeca47115226b25540ab5ea17176ff05.png)

选择MCP Compatible Dify Tools，点击 API端点 右侧的 +号

![](img/323ee625b7213cef8971e520c51b1367.png)

按照以下所示填写端点名称和选择工具列表，然后点击保存

![](img/33fa6d38a3e4d9e9edcf9afea520d0c1.png)

查看API端点信息

![](img/117808345b2c0b04542414257ca6f79c.png)

记录，并继续调整（使用本机IP地址替换localhost），如果地址中有}则予以删除

```
http://192.168.1.6/e/5npo3qkptq9apf31/sse
```

在cheery studio中添加MCP服务器，如下图所示

![](img/80947a1b38d8f81f6153f86b030a1062.png)

在聊天对话窗口启用 dify-weather MCP服务器

![](img/0411d5cd43506d6d72af2ae67dd06ef9.png)

查询天气

![](img/9b43f32b8919f7a2f863a05201809c82.png)

查看MCP Server调用情况

![](img/cabaa1d854813f9193ee6652202200da.png)