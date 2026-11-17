
# LangChain快速入门与Agent开发实战2

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/NXwdd5kdZoufaix7TAucYpBqnBf](https://kq4b3vgg5b.feishu.cn/docx/NXwdd5kdZoufaix7TAucYpBqnBf)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(夏季班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(夏季班)

![](img/c58ab3801a45cbd814bc0ca70ba23ab2.png)

![](img/a2506cc6b7feb0d099fd065ef7ab32a3.png)

公开课全套学习资料，已上传至网盘（链接: `https://pan.baidu.com/s/1_TPoisye8HldxiIXiMxM8Q` 提取码: khnp ）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## 《大模型Agent智能体开发实战（夏季班）》体验课

## LangChain快速入门与Agent开发实战-Part 2

### LangChain项目回顾

LangChain可以称之为自2022年底大模型技术爆火以来的第一个真正意义上的大模型开发框架。

大模型本质上无法直接解决实际的问题，仅仅是一个能够分析、推理和生成文本的黑盒。直到现在，所有的开发者们仍然在不断探索如何把大模型的强大能力与实际应用场景结合起来，而当时LangChain的出现，直接让大模型开发变得简单起来，它将大模型开发过程中常用的功能、工具、流程等等全部封装成一个个的组件，使开发者可以像搭乐高积木一样，快速的组合出适用于不同场景需求的大模型应用。

LangChain的首个版本于2022年10月开源，直到现在仍然再以一个飞快的速度不断进行迭代升级。从一个开源 Python/TS 框架逐渐发展，形成包括“链”和“代理”等核心组件，现在已走向企业级阶段，发展成了LangChain AI，其拥有目前Agent技术领域最大的开源生态，衍生出了多个开源项目框架，各自都在大模型的技术领域承担着不同的开发任务角色。

其官方Github地址为：`https://github.com/langchain-ai`

![](img/817e4aa292e917399cf34b5200d1485f.png)

这里我们可以梳理出在LangChain AI中最受关注的项目发展态势，如下图所示：

![](img/080822d12b87e6b654d5287bf29d6c9c.png)

其中最活跃的项目当属`langchain`，排在前二的分别是`langChain`的Python版本和JavaScript版本。作为LangChain AI发展的基石，`langchain`主要用来支持构建大模型应用的一切，包括链式编排、检索增强生成 (RAG)、嵌入、文档处理、对话系统、代码分析等。而随着业务需求的越来越复杂，LangChain AI也推出了`langgraph`，作为一个基于图结构的 Agent 编排框架，可构建有状态、多步骤、多 Agent 的工作流，从而进一步扩展了LangChain AI自家生态的适用范围。而至于`local‑deep‑researcher`和`opengpts`，则完全是给开发者提供了基于LangChain定制开发出的特定热门应用，也一定程度上能印证LangChain AI的生态繁荣和可实现应用场景的广度。

### LangChain AI 热门开源项目功能介绍

| 项目 | 技术栈 | 核心用途 |
| --- | --- | --- |
| `langchain` | Python/TS | 构建 LLM 应用基础组件 |
| `langchainjs` | JS/TS | 前端/Node 环境中构造 LLM 应用 |
| `langgraph` | Python | 用图编排复杂 agent 流程 |
| `local-deep-researcher` | Python | 自动化、多轮本地 Web 研究工具 |
| `opengpts` | Python + Go + 前端 | 可定制化 GPT 平台，支持 RAG 和 agent 开发 |

对外，LangChain 也是目前大模型技术领域所有AI Agent框架中最热门、最通用的，相较于`AutoGen`、`CrewAI`、`OpenAI Agents SDK`和`Google ADK`，拥有最多的收藏量和活跃的开发者数量。

![](img/ed52cb1e8d993520c273b9faee2c763d.png)

因此这里大家可以感受到，langChain AI生态的学习，其实最最核心的就是LangChain这个项目。而`langChain`是一个非常全面的开发框架，集成了Agent和RAG两个关键的热门落地方向，通过灵活的模块化组合可以快速的构建适用于私有业务场景的大模型应用，在目前的应用开发中是企业中使用最多的Agent开发框架。其核心架构如下图所示：

![](img/c64e15302c9f5b33ebd65a81c47c1f80.png)

从本质上分析，LangChain从大模型角度出发，通过开发人员在实践过程中对大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接口以适配各种大模型。LangChain抽象出最重要的核心模块如下：

1. `Model I/O` ：标准化各个大模型的输入和输出，包含输入模版，模型本身和格式化输出；

1. `Retrieval` ：检索外部数据，然后在执行生成步骤时将其传递到 LLM，包括文档加载、切割、`Embedding`等；

1. `Chains` ：链，LangChain框架中最重要的模块，链接多个模块协同构建应用，是实际运作很多功能的高级抽象；

1. `Memory` ：记忆模块，以各种方式构建历史信息，维护有关实体及其关系的信息；

1. `Agents` ：目前最热门的`Agents`开发实践，未来能够真正实现通用人工智能的落地方案；

1. `Callbacks` ：回调系统，允许连接到 大模型 应用程序的各个阶段。用于日志记录、监控、流传输和其他任务；

从上图中可以看到，LangChain框架涵盖了模型输入输出的标准化、外部工具接入的规范、上下文记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的`Chain`高级抽象，定义了不同形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整的应用程序。这个过程类似于搭建积木，可以灵活适应不同的任务需求。

因此本节公开课，我们就围绕LangChain展开详细的讲解，我们会涉及到LangChain框架的整体概览，如何用LangChain搭建智能体和本地知识库问答的完整流程，同时对于比较热门的`MCP`工具如何接入LangChain框架，会做一个重点的说明。

需要说明的，这里我们选择使用 Python 作为开发语言，同时使用目前最新的LangChain 0.3版本，具体的版本说明如下：

+ `Python==3.12`

+ `LangChain>=0.3.25`

+ `langchain-deepseek>=0.1.3`

## 一、LangChain快速入门

### 1. LangChain接入大模型流程

#### LangChain安装流程

如果使用LangChain进行大模型应用开发，需要安装LangChain的依赖包，安装命令如下：

```
! pip install langchain
```
