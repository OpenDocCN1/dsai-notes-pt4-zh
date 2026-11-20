
# Part 1.最新版LangChain 1.0快速入门介绍

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/IKcmd7QRPoyVgIxi4iCcQStynfb](https://kq4b3vgg5b.feishu.cn/docx/IKcmd7QRPoyVgIxi4iCcQStynfb)

**视频讲解：** LangChain1.0新版功能解读！统一Agent API、中间件、Deep Agents功能介绍！

<https://player.bilibili.com/player.html?bvid=1aU1YBZErP>

---

## 一、LangChain 1.0 核心要点介绍

![](img/81a744134d768dbd8ea03e821305e9b8.png)

### 1. 从“拼装库”到“Agent Runtime”

+ **定位升级**：LangChain 1.0 不再只是“调用模型的胶水层”，而是面向生产的 Agent 开发生态入口：统一的模型抽象、标准化消息/内容块、可插拔中间件、以及与 LangGraph 的深度耦合，使开发者既能快速起步，又能细粒度管控每一步执行。

+ **底座迁移**：1.0 时代的 Agent 运行时正式建立在 LangGraph 上；也就是说，你在 LangChain 层“写出来”的 Agent，实质是运行在 LangGraph 的图式执行引擎上（带有状态、检查点、可中断/恢复、持久化与并发控制）。

+ **历史包袱清理**：旧的 `AgentExecutor` 路线进入退场通道，官方明确建议面向新项目使用基于 LangGraph 的 Agent 方案。

> `LangChain = 快速构建与统一抽象；LangGraph = 低层可控与可伸缩运行时`。两者叠加，才能既快又稳。

### 2. 三大里程碑式能力

1. `create_agent` 统一创建入口

+ 用一个 API 拉起“基于 LangGraph 的图式 Agent 运行时”，默认内含循环调用、工具执行、停机条件、以及可注入的前后置钩子（中间件）。

+ 对开发者而言：**统一而简洁**，对运行时而言：**可靠且可观测**。

![](img/08c11b3a091b642f34c9fead8dd1f7a1.png)

1. 标准化内容块（Standard content blocks）

+ 为不同模型厂商的输出定义统一规范，降低“换模型/换云厂商”带来的解析/适配成本。

1. 表面积瘦身（Surface Area Trim）

+ 精简命名空间与冗余 API（如旧式多套 Agent 构造器），把重心放回“用得最多”的接口与构件，减少选择焦虑与迁移成本。

### 3. 中间件（Middleware）：1.0 的“王牌”工程化能力

> 中间件是“围绕模型调用点”的拦截器，提供`before_model`、`after_model`、`wrap_model_call`等钩子，让你在不改 Agent 主体逻辑的情况下实现策略与治理。

![](img/a6693404ac97258947e5462ccb0dbd3e.png)

**典型能力与场景：**

+ Human-in-the-loop（人在环）：对敏感工具调用（写库、转账、发信等）自动中断，等待“批准/编辑/拒绝”，需要启用`checkpointer`以在中断后可恢复执行。我们的实操已验证在 Notebook/Studio 场景下如何以 JSON 决策恢复执行。

+ 消息压缩/治理：

+ 修剪消息（Trimming）：靠条数或 token 上限保留近期上下文；

+ 删除消息（Deleting）：精确移除早期或特定类型消息；

+ 汇总消息（`SummarizationMiddleware`）：当累计 token 逼近阈值时自动生成摘要并保留若干最新消息。我们已用 DeepSeek 演示三套策略。

+ 动态模型路由（`wrap_model_call`）：按输入规模、关键词或预算等条件在不同模型间切换（如简单用 `deepseek-chat`、复杂启用 `deepseek-reasoner`）。

> 这些能力的底层都建立在 LangGraph 的可中断（`interrupt`）与持久化状态 之上，因此具备可恢复、可观测的生产级韧性。

### 4. 新版LangChain vs LangGraph：职责分工与对比

| 维度 | LangChain 1.0 | LangGraph |

| --- | --- | --- |

| 定位 | **统一抽象 + 快速搭建**：模型、消息、工具、链路、标准输出 | **低层运行时**：图式编排、状态管理、检查点、并发、可中断/恢复 |

| Agent 入口 | `create_agent`（开箱即用） | Graph API（自定义节点/边/条件） |

| 可靠性 | 借助中间件与 Graph Runtime 获得 | 由 Graph 的 Pregel 风格执行/检查点提供 |

| 治理能力 | 中间件（HITL、汇总、修剪、预算控制等） | 中断、持久化、分支/循环、显式状态机 |

| 适用人群 | 产品/业务工程师、快速 PoC 到 MVP | 平台/基础设施工程师、需要“精确控制”的团队 |

> 官方也强调“快速起步用 LangChain，复杂控制用 LangGraph，二者并行协同”。

![](img/14bd7be7b4f59d7cd52d7c9a91c8575f.png)

## 二、LangChain.ai工具生态介绍

本期公开课，我将为大家详细讲解元老级Agent开发工具——LangChain。

### 1. GPT-3时代下第一代大模型开发工具

LangChain可以称之为自2022年底大模型技术爆火以来的第一个真正意义上的大模型开发框架。大模型本质上无法直接解决实际的问题，仅仅是一个能够分析、推理和生成文本的黑盒。直到现在，所有的开发者们仍然在不断探索如何把大模型的强大能力与实际应用场景结合起来，而当时LangChain的出现，直接让大模型开发变得简单起来，它将大模型开发过程中常用的功能、工具、流程等等全部封装成一个个的组件，使开发者可以像搭乐高积木一样，快速的组合出适用于不同场景需求的大模型应用。

LangChain的首个版本于2022年10月开源，直到现在仍然再以一个飞快的速度不断进行迭代升级。从一个开源 Python/TS 框架逐渐发展，形成包括“链”和“代理”等核心组件，现在已走向企业级阶段，发展成了LangChain AI，其拥有目前Agent技术领域最大的开源生态，衍生出了多个开源项目框架，各自都在大模型的技术领域承担着不同的开发任务角色。

从一个形象的角度来说，LangChain的功能定位其实并不是我们现在所谓的Agent开发框架，而是一个大模型功能增强器，借助LangChain，哪怕在GPT-3模型时代，也能让模型完成对话、拥有记忆、甚至是完成结构化输出等功能。

![](img/df81b25e5676728faff84c7b18b97aa4.png)

> LangChain官网：https://www.langchain.com/

### 2. 备受争议的工具功能

虽说LangChain的开源，在短时间内收获了大量的开发者用户，这也一度使得LangChain在2023年成为最受欢迎的大模型开发工具没有之一。但每项技术都会受限于其诞生的时代背景，LangChain也不例外。在GPT-3时代，大模型以补全模型为主，只能以类似“成语接龙”的方式对文本进行补全，并且实际运行效果也非常不稳定。此时LangChain借助一些高层封装的API，能够让模型完成对话、调用外部工具、甚至是结构化输出等功能，这在当时是非常大的进步，也为开发者提供了极大的便利。

但是，伴随着GPT-3.5模型的发布，对话模型正式登上历史的舞台，并逐渐成为主流。而得益于对话模型更强的指令跟随能力，很多GPT-3需要借助LangChain才能完成的工作，已经成为GPT-3.5原生自带的一些功能。而等到GPT-4逐渐普及，包括调用外部工具（Function calling）、结构化输出、系统提示词等功能，都成了模型的基础功能。而对于开发者而言，此时再使用LangChain再对这些功能进行封装就显得多此一举。

![](img/1fd73ca5eaee72cb29c8b29b0f9487f5.png)

> 【考古】补全模型与对话模型对比介绍：https://www.bilibili.com/video/BV1Vk4y1V76h

因此在2023年下半年起的很长一段时间里，LangChain饱受争议，很多开发者觉得LangChain代码冗余、编写复杂，甚至有开发者觉得LangChain太过于复杂，一个模型调用的过程就涉及到数十个类，一个项目开发动辄就要用到几十个不同的类，而说明文档更是几百个不同的常用类需要记住，其复杂程度不亚于学习一门全新的编程语言。

![](img/77d45d36c5f31e2419e0b6bf080d1445.png)

![](img/92a80ccdb5876884fba1cea5aedccb5a.png)

![](img/14c15b2672d1b17c7162ae8e04985745.png)

而这也使得在某个时间段，LangChain的开发者大规模流失。

> 举个例子，为了更好的兼容不同模型的调用，谷歌ADK采用了`LiteLlm`作为底层模型调度框架，一个库即可调用各类模型，而LangChain则为每个主流模型单独封装了一个库，调用不同模型的时候需要导入不同模型对应的库，例如调用DeepSeek就需要安装`langchain-deepseek`，而调用Gemini则需要安装`langchain-google-genai`。

### 3. 更加适用于当前Agent开发的LangChain工具生态

在经历了短暂的阵痛后，LangChain果断进行了大刀阔斧的改革。LangChain调整的思路非常简单：

LangChain本身仍然坚守作为“模型能力增强器”的功能定位，并且逐渐稳定更新节奏和频率，虽说实际使用LangChain进行开发的代码量仍然没变，但模块划分更加清晰、功能更加丰富和稳定，逐步达到企业级应用水准。目前最新版LangChain的核心功能如下：

| 模块类别 | 示例功能 |

| --- | --- |

| **模型接口封装** | OpenAI、Claude、Cohere、Qwen 等模型统一调用方式 |

| **输出结构化** | 自动从模型中解析 JSON、Schema、函数签名、文档等 |

| `Memory` 管理 | Buffer、Summary、Entity、Conversation Memory 等 |

| **Tool 接入** | Web 搜索、SQL 数据库、Python 执行器、API 代理等 |

| **Agent 架构** | ReAct、Self-Ask、OpenAI Function Agent 等调度机制 |

| **RAG 集成** | 多种 Retriever、Vector Store、文档拆分策略 |

| **Server/API 发布** | 快速将链部署为 Web 服务或 A2A Agent |

| Debug & Callback | Token 使用统计、LangSmith 可视化追踪等 |

✅ 所以说：LangChain 是 LLM 功能开发的「积木工厂」，不是简单框架，而是模型增强器 + 应用组装工具箱。

与2023年下半年开源LangGraph，LangGraph作为基于LangChain的更高层次封装，能够更加便捷的搭建图结构的大模型工作流，也就是现在所谓的Multi-Agent系统，而LangGraph也是目前LangChian家族最核心的Multi-Agent开发框架。同时可以搭配`LangGraph-Studio`进行实时效果监测，实际效果如下所示：

![](img/2af464906eb45e0772e2f199aa0a0557.png)

项目官网：https://github.com/langchain-ai/langgraph

但是需要注意的是，从LangChain 1.0开始，LangChain核心API `create_agent`将以LangGraph为基座，也就是说，从LangChain 1.0开始，LangGraph由高级API转化为了基础API。

开源大模型工作流（Agent）可视化监控与测试平台LangSmith，借助LangSmith，开发者能够更加简单便捷监控基于LangChain生态的Agent运行流程、测试Agent功能和不同提示词等，从而使得LangChain进一步面向企业级应用开发框架；

![](img/95c031af3f3dd6d6cc39bc7667a5bb3f.png)

LangSmith官网：https://www.langchain.com/langsmith

同时，考虑到LangChain本身较为复杂这一情况，开源了LangChain的“可视化实现版”——LangFlow，这是一款形式对标Dify、可以通过可视化方式、借助拖拉拽来完成LangChain相同功能的开发工具。

![](img/01ec6ea242d7850bd8a721c0642630e6.png)

同时，这也是目前LangChain工具家族中，仅次于LangChain（109k stars）第二受欢迎的开发框架，在GitHub上已斩获接近72k stars。

![](img/492db5a89f7142cd9f42a11c3ead3ad9.png)

LangFlow官网：https://www.langflow.org/

同时，相比Dify，LangFlow功能更加完善，并且没有任何商业化的计划，可长期稳定使用。

目前LangChain工具生态如图所示：

![](img/09550c61a62b2c2b2ad3461aac2001e0.png)

### 4. 当下大模型开发人员必备技能：LangChain

可以说，经过了近3年的发展，目前LangChain工具生态已经非常全面，能适用于各类不同场景的开发需求，无论是小规模实验、还是大规模商业化部署，无论是使用代码开发、还是偏向使用低代码开发工具，LangChain工具家族都能满足开发者的需求。

也正因如此，LangChain可以说是历经大模型Agent技术发展巨变但仍“屹立不倒”的开发工具，哪怕今年以来OpenAI、谷歌等AI巨头纷纷下场发布全新一代Agent开发框架，但LangChian工具本身仍在很多场景下不可替代。而相比之下，类似`AotuGen`、`CrewAI`等工具的适用面，在大模型技术飞速发展的当下，正在逐渐减少。截止目前，LangChain仍然是目前大模型开发岗位应用最广的框架没有之一：

![](img/43be492b6d097fc535da840e5b0a15c4.png)

同时LangChain也是很多著名的开源项目的底层开发框架，如字节前端时间开源的Deep Research应用`deerflow`就是采用了LangChain&LangGraph框架：

![](img/83162217b6115b953045ee19aa9cf8a6.png)

![](img/1b704fd145eb8804766b14d2a0ad6659.png)

> 项目主页：https://github.com/bytedance/deer-flow

此外如谷歌近期开源的`Gemini Fullstack LangGraph Quickstart`热门项目，也是使用LangGraph作为基础框架：

![](img/6c3aad3ea2679d0e2af105d886ca250b.png)

> 项目主页：https://github.com/google-gemini/gemini-fullstack-langgraph-quickstart

全套公开课课件领取：

![](img/bb3622984bf73512a16f9e8e2af03b22.png)
