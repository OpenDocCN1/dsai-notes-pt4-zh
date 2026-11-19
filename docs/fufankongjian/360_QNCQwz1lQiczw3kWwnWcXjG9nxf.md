

# 1. LangChain DeepAgents 项目介绍

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/FWbndNVp0oUKBYxSuSOcPhZcn8d](https://kq4b3vgg5b.feishu.cn/docx/FWbndNVp0oUKBYxSuSOcPhZcn8d)

## 《大模型Agent智能体开发》（12月班体验课）

### Part 1. LangChain DeepAgents 项目介绍

2025年被全球人工智能社区广泛定义为代理元年的转折点，大模型的应用形态经历了一场深刻的范式转移：从基于瞬时对话、短窗口上下文的浅层代理，向具备长时记忆、复杂规划能力、能够自主管理资源的深层代理（`Deep Agents`）演进。

这种技术发展的根本原因是现实世界中复杂任务的增多，以及大模型在处理复杂任务时暴露出的局限性。`DeepAgents` 或者该技术下大家更熟知的一种产品形态：`DeepResearch`，其产品形态如下所示：

+ 曾经爆火的`Manus`，内测期间激活码一度炒到5万天价：

```
from IPython.display import Video

Video("https://muyu20241105.oss-cn-beijing.aliyuncs.com/images/202512021334423.mp4", width=1500, height=400)
```

Your browser does not support the video element.

+ `Manus`发布后仅用了3个小时就进行复现并开源的`OpenManus`项目

```
from IPython.display import Video

Video("https://muyu20241105.oss-cn-beijing.aliyuncs.com/images/202512021335691.mp4", width=1500, height=400)
```

Your browser does not support the video element.

+ 一个将`Manus`名字倒着写的开源项目且具备工业级可用的水准的开源项目 `Suna`：

```
from IPython.display import Video

Video("https://muyu20241105.oss-cn-beijing.aliyuncs.com/images/202512021334759.mp4", width=1500, height=400)
```

Your browser does not support the video element.

当然，我们课程也基于 `Suna` 进行二开后，全新研发了企业级 `Manus` 通用智能体项目：`FuFanManus`：

```
from IPython.display import Video

Video("https://muyu20241105.oss-cn-beijing.aliyuncs.com/images/202512021352519.mp4", width=1500, height=400)
```

Your browser does not support the video element.

也正是在这个时间节点，随着 `Claude Code` 和 `Deep Research` 等应用的惊艳亮相，开发者社区开始渴望拥有一种能够复现这些能力的开源工具。`LangChain` 团队敏锐地捕捉到了这一需求，`Deepagents` 的首个公开版本 `v0.0.1` 于 2025 年 7 月 29 日正式发布。这一版本的发布标志着 `LangChain` 团队开始尝试将复杂的代理逻辑封装为标准库。此时的项目更像是一个实验性的原型，主要用于验证“规划”与“执行”分离的可行性。

## 一、`DeepAgents` 是什么？

`DeepAgents`的核心定位就是：用于构建能够处理复杂、多步骤任务的智能体框架。根据官方的说明，它的设计灵感就来自于像 `Claude Code`、`Deep Research` 和 `Manus` 这样的应用。

![](img/4fd144237be851b7defd39c94b33469b.png)

同时，随着推理类大模型的性能提升以及 `MCP` 协议的普及，`LangChain` 对 `deepagents` 进行一次彻底的架构升级，以适应更复杂的企业级需求，并于 2025 年 10 月 28 日，`LangChain` 官方博客发布了题为《Doubling down on deepagents》的重磅文章，正式推出 `v0.2` 版本。

![](img/3ed86126f055067e4bea62911a1423fc.png)

> 论文地址：https://arxiv.org/pdf/2510.21618

![](img/c4bda8710feec6e7582db67b6a93f4e7.png)

这次升级的核心在于引入了插件式后端。在此之前，代理的“记忆”和“文件”仅存在于 `LangGraph` 的状态（`State`）中，一旦会话结束或系统崩溃，数据即告丢失。`v0.2` 版本打破了这一限制，允许开发者将后端映射到真实的本地磁盘、`S3` 云存储或任何自定义的数据库中。这一变革使得 `DeepAgents` 从一个单纯的对话机器人，进化为一个能够真正操作物理世界数据、具有持久化生命周期的“数字员工”。

根据官方的说明，`DeepAgents` 非常适用于以下情况：

+ 处理需要计划和分解的复杂、多步骤任务；
+ 通过文件系统工具管理大量上下文信息；
+ 将工作委派给专门的子代理以实现上下文隔离；
+ 在对话和主题之间保持记忆；

当然这个描述相对来说比较抽象，因此我们这里对适用于`DeepAgents`的场景进行一个总结：

| DeepAgent 适用场景 | | | |
| --- | --- | --- | --- |
| 场景类型 | 能力说明 | 工作逻辑 / 技术特点 | 代表性案例 |
| 深度调研与报告生成 | 支持长周期、多步骤、多来源信息整合的研究任务 | • 自动生成研究计划（`Todo`） • 调用搜索工具获取资料 • 将关键信息写入文件系统（长期记忆） • 使用子代理（`Sub-Agents`）深入研究子课题 • 主代理统一规划、整合结果 | • `LangChain Deep Research` 示例（`Tavily` 搜索 + 多子代理拆分研究） • `OpenAI Deep Research`（官方生产级深度调研助理） |
| 自动编程与代码助理 | 理解代码、修改代码、生成新文件、执行工具链 | • 代理可读写虚拟文件系统 • 自动分析源码并输出 `diff` • 人工审批（`Human-in-the-Loop`）保证安全写入 • 调用 `Shell` / 测试工具执行流程 • 可将项目规范写入 `/memories` 用作长期记忆 | • `LangChain DeepAgents CLI`（终端自动编码） • `Anthropic Claude Code`（深度自动重构与编程） • `Manus`（多步骤代码智能体） |
| 复杂流程自动化（业务流程 `Orchestration`） | 将多个步骤串联为可控流程，适合企业级自动化任务 | • 任务分解 → 多步骤规划 → 调用不同工具 • 搜索、筛选、处理、生成等多环节协作 • 使用文件系统存储中间数据（如列表、分析结果等） • 支持多工具、多子任务并行处理 | • `DeepAgents` 求职助手（职位搜索 → 筛选排序 → 求职信生成 → 打包结果） • 企业场景如：自动生成分析报告 / 客服知识库构建 / 数据采集+处理流 |

技术架构虽然复杂，但是使用起来非常简单，只需要定义好代理的规划和执行流程，然后调用 `DeepAgents` 的 API 即可。如下伪代码所示：

```
import os
from deepagents import create_deep_agent
from tavily import TavilyClient

# 1. 定义工具
tavily_client = TavilyClient(api_key=os.environ)

def internet_search(query: str):
    """运行网络搜索"""
    return tavily_client.search(query)

# 2. 定义系统提示词
# 注意：不需要手动添加关于 TodoList 或文件系统的指令，中间件会自动注入
system_prompt = """
你是一个专家级的研究助理。你的任务是进行彻底的调研并撰写报告。
请充分利用你的规划能力和文件系统来管理你的工作。
"""

# 3. 创建代理
# create_deep_agent 会自动装配 `TodoListMiddleware`, `FilesystemMiddleware` 等
agent = create_deep_agent(
    model="anthropic:claude-3-5-sonnet-latest",
    tools=[internet_search],
    system_prompt=system_prompt,
)

# 4. 运行代理
# 代理现在是一个编译好的 LangGraph StateGraph
result = agent.invoke({
    "messages": [{"role": "user", "content": "详细调研 LangChain 的最新发展并写一份总结。"}]
})
```

`DeepAgents` 不仅是一个软件库，更代表了一种全新的软件范式，它标志着代理构建方式的根本转变：从简单的循环脚本转向基于复杂的代理线束架构。这是一种高度标准化的基础设施，旨在为大语言模型提供执行长时程任务所需的认知脚手架。

## 二、`DeepAgents` 的架构设计

+ `DeepAgents` 与 `LangChian`、`LangGraph` 的关系

`DeepAgent` 一个构建在 `LangChain` 和 `LangGraph` 之上的开源智能体框架（agent harness），将规划工具、文件系统访问、子代理和详细提示词等关键机制整合在一起，以支持复杂的深度任务。所以可以理解为：`LangChain`、`LangGraph` 和 `DeepAgents` 三者构成了 `LangChain` 生态中不同层次的组件，各有定位。

简单来说，它们按层次构建于彼此之上：`DeepAgents` 基于 `LangChain` 的代理抽象实现，而 `LangChain` 又构建在 `LangGraph` 的运行时之上。`LangChain` 团队将它们分别称作“代理框架”（`LangChain`）、“代理运行时”（`LangGraph`）和“代理套件/骨架”（`DeepAgents`）：

![](img/f91fe4952d673775721b832976a63c1c.png)

下表总结了三者的关系和侧重点：

| LangChain 生态组件关系 | | |
| --- | --- | --- |
| 平台组件 | 角色定位 | 侧重场景与特点 |
| `LangGraph` | 智能体运行时（Agent Runtime） | 提供底层执行框架，将代理逻辑表示为有状态图，便于编排复杂工作流。适合需要构建多步骤流程或多代理协同的场景。 |
| `LangChain` | 智能体开发框架（Agent Framework） | 提供核心的Agent循环机制和工具接口，让开发者可以从零搭建代理。适合需要自定义提示词、自定义工具的灵活开发，对应低层构建。 |
| `DeepAgents` | 智能体工具套件（Agent Harness） | 在 `LangChain` 基础上预设了一系列高级能力（规划、文件系统、记忆等）。适合希望快速构建更自主、长时运行的深度代理，将内置组件直接复用以专注于业务逻辑。 |

注意：`DeepAgents`构建的是基于`LangGraph`的图结构！！

从技术定位上看，如果只需要基础的 `Agent` 功能并愿意自己设计提示和工具，可以直接使用 `LangChain`；如果需要构建复杂的多`Agent`系统或工作流，`LangGraph` 提供了底层支持；而 `DeepAgents` 则面向想要省却底层搭建，直接使用现有深度机制来打造 `AutoGPT` 类自主智能体的需求。因此，`DeepAgents` 可以被视为在 `LangChain` 上封装的一层“加速器”：它并非取代 `LangChain` 或 `LangGraph`，而是利用它们提供的抽象和运行时，将深度代理常用模式做成开箱即用的组件。

`DeepAgents` 引入的四大核心机制：

+ 红色代表“规划工具”（`Todo` 清单）；
+ 绿色代表“文件系统”（虚拟磁盘）；
+ 橙色代表“子代理”（子任务智能体）；
+ 蓝色代表“详细系统提示”。

这些组件共同赋能代理处理长时序、复杂多步骤的任务，其核心架构组成如图所示：

![](img/5036b93e380f0fd904a0f1009d09117e.png)

`DeepAgents` 的技术目标是提供一个通用且可扩展的深度代理骨架，使开发者无需从零开始即可构建自主的长流程智能体。它在默认实现中内置了支持复杂任务的四个关键要素：

+ **详细系统提示词**：`DeepAgents` 附带一套借鉴 `Claude Code` 的长提示模板，其中包含如何使用工具的详细说明和示例，指导代理在复杂情境下保持一致的行为。精心设计的系统提示为代理提供领域步骤指引和用例示范，极大提升其在复杂任务中的表现（“提示工程”仍然至关重要）。
+ **规划工具（`Todo` 清单）**：`DeepAgents` 具有一个待办列表工具，实际上是个无副作用（no-op）的上下文工具，用于引导代理在行动前生成任务清单/计划。代理通过调用这个 `write_todos` 工具来罗列子任务，从而在对话上下文中明确后续步骤。这种规划式上下文管理能帮助代理拆解大型任务，保持条理并防止遗忘步骤。虽然 `Todo` 工具本身不执行实际操作，但起到了重要的“上下文提示”作用，让代理在更长时间跨度上保持计划性。
+ **子代理**：`DeepAgents` 支持启动专门的子代理来处理子任务。主代理可以通过内置工具 `task()` 将特定工作委派给子代理，由它们在隔离的上下文中执行。每个子代理可定义独立的提示、工具和职责范围，实现上下文隔离和职责分离。例如，一个“研究子代理”专注深度信息查找，另一个“分析子代理”专注数据处理。每个子代理可定义独立的提示、工具和职责范围，实现上下文隔离和职责分离。例如，一个“研究子代理”专注深度信息查找，另一个“分析子代理”专注数据处理。子代理的引入带来两大好处：（1）提示词专业化：为不同子任务提供定制指令，提高针对性；（2）上下文管理：子代理各自维护独立记忆，避免干扰主代理，从而让整体系统可在更深层次展开并行或分步推理
+ **文件系统集成**：`DeepAgents` 内置了一个虚拟文件系统（最初基于 `LangGraph` 内存状态），代理可使用 `ls`、`read_file`、`write_file`、`edit_file` 等文件操作工具读写“工作文件”。这相当于给代理提供了外部记事本或共享工作空间：代理能将中间结果、笔记或重要信息写入文件，供后续步骤或子代理读取，从而实现跨步骤的持久记忆。文件系统还支持模式搜索（如 `glob` 通配查找文件，`grep` 搜索文件内容）以及受控的代码执行（`execute` 在沙箱中运行 shell 命令，但需显式启用）。值得一提的是，自 `DeepAgents` `0.2` 版本起，引入了可插拔存储后端：开发者可以将虚拟文件系统映射到真实本地磁盘或云端存储，实现跨会话的持久存储

简单来说，`Deep Agents` 为我们提供了构建"会思考、会规划、会分解任务"的智能体的能力。它不仅仅是一个简单的对话机器人，而是一个能够自主规划、管理上下文、并可以创建子智能体来处理复杂任务的系统。

> **关键点：** `Deep Agents` 的核心优势在于它能够处理需要多步骤规划的任务，而不仅仅是简单的问答。

