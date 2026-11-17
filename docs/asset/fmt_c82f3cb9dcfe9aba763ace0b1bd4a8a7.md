
在 Claude Code 中，自定义 Agent 以 `.md` 文件形式存储在 `.claude/agents/` 目录下。每个文件包含一段 YAML 前置元数据（定义模型、工具等参数）和 Markdown 正文（Agent 的系统提示词）。

你可以直接让 Claude Code 帮你创建这些文件：

```
请在 .claude/agents/ 目录下创建以下三个 Agent 配置文件：

1. chatbot-planner.md —— 产品架构师
   - 模型：sonnet
   - 只读工具：Read, Glob, Grep, WebSearch
   - 权限模式：plan
   - 最大轮次：10
   - Prompt：你是一位前端产品架构师，负责分析需求、设计页面结构、
     规划交互流程和技术方案，输出实现计划。你只做规划，不写代码。

2. chatbot-coder.md —— 前端开发工程师
   - 模型：sonnet
   - 读写工具：Read, Write, Edit, Bash, Glob, Grep
   - 权限模式：acceptEdits
   - 最大轮次：20
   - Prompt：你是一位前端开发工程师，根据规划方案编写完整的代码实现。
     使用现代 CSS 和 ES6+ JavaScript，注重代码质量和用户体验。

3. chatbot-reviewer.md —— 代码审查专家
   - 模型：haiku
   - 只读工具：Read, Glob, Grep
   - 后台运行：true
   - 最大轮次：10
   - Prompt：你是一位代码审查专家，重点检查可访问性、性能、安全性、
     代码质量和响应式设计，输出评分和改进建议。
```

Claude Code 会创建 `.claude/agents/` 目录并生成三个 `.md` 文件。以 `chatbot-planner.md` 为例，生成的文件内容大致如下：

```yaml
---
name: chatbot-planner
description: 规划 HTML 页面结构和交互方案
model: sonnet
maxTurns: 10
allowedTools:
  - Read
  - Glob
  - Grep
  - WebSearch
permissionMode: plan
---

你是一位前端产品架构师。你的职责是：

1. 分析用户需求，规划页面结构
2. 设计交互流程和 UI 布局
3. 列出技术方案（CSS 框架、动画、响应式策略）
4. 输出一份简洁的实现计划（不要写代码）

输出格式：
## 需求分析
## 页面结构设计
## 交互流程
## 技术方案
## 实现步骤清单
```

创建完成后，验证文件结构：

```bash
ls .claude/agents/
# 应输出：chatbot-coder.md  chatbot-planner.md  chatbot-reviewer.md
```

### 5.3 执行流水线

三个 Agent 就位后，按照规划 → 编码 → 审查的顺序逐步执行。

Step 1：启动 Planner Agent

在 Claude Code 中输入：

```
@chatbot-planner 请为以下需求规划一个完整的前后端 ChatBot 应用：
基于当前目录的 chatbot.html 前端设计，开发一个可运行的 ChatBot，
前端通过 fetch 调用后端 API，后端使用 Node.js + Express + Anthropic SDK，
支持流式输出和多轮对话。
```

`@agent名称` 是调用自定义 Agent 的语法。Planner 会分析需求，输出一份结构化的实现计划——包含需求分析、页面结构、技术方案和实现步骤清单。注意：由于 Planner 只有 `Read`、`Glob`、`Grep` 等只读工具，它不会也不能修改任何文件。

Step 2：启动 Coder Agent

Planner 输出计划后，将计划交给 Coder：

```
@chatbot-coder 根据刚才 chatbot-planner 给出的规划方案，
实现完整的前后端 ChatBot 应用。前端保存在 public/index.html，
后端入口为 server.js，包含 package.json 和 .env.example。
```

Coder 拥有 `Write`、`Edit`、`Bash` 等读写工具，会按照 Planner 的方案逐步创建文件、编写代码、安装依赖。

Step 3：启动 Reviewer Agent（后台运行）

编码完成后，启动 Reviewer 进行代码审查：

```
@chatbot-reviewer 请审查 server.js 和 public/index.html 的代码质量
```

由于 `chatbot-reviewer` 配置了 `background: true`，它会进入后台运行——不阻塞你的主对话。你可以在等待审查结果的同时继续做其他事情。审查完成后，会收到一份包含评分和改进建议的报告。

### 5.4 Fufan-CC Flow 中的操作

在 Fufan-CC Flow 中，多 Agent 流水线的体验更加直观：

**创建 Agent**：在右侧面板 Agent → Agent 管理 选项卡中，点击"新建 Agent"按钮，通过可视化表单填写配置项（名称、模型、工具权限、Prompt 等），无需手动编写 `.md` 文件。

**启动 Agent**：在 Agent 管理列表中找到目标 Agent，点击 ▶ 启动按钮——输入栏会自动预填 `@agent名称` 前缀，你只需补充具体指令。

**观察执行树**：切换到 **执行树** 子选项卡，可以实时看到 `Sub-Agent` 的层级关系和执行状态。如果 Claude 在内部派发了更多子任务（如 `Explore Agent` 搜索代码），也会在执行树中显示。

**后台任务监控**：`Reviewer` 在后台运行时，后台任务 子选项卡会显示运行中的任务卡片（橙色边框），Tab 标签旁出现数字角标。完成后卡片变为绿色边框，展开即可查看审查报告。

### 5.5 进阶：编排工作流（自动化流水线）

上面的手动三步调用已经展示了多 Agent 协作的价值，但每次都要手动启动三个 Agent 略显繁琐。Claude Code 的工作流编排功能可以将这三步自动化为一条流水线。

在 Fufan-CC Flow 的 Agent → 工作流 子选项卡中，点击"新建工作流"，配置三个步骤：

| 步骤 | Agent | Prompt |
| --- | --- | --- |
| 步骤 1 — 需求规划 | `chatbot-planner` | 请为以下需求规划实现方案：`$REQUIREMENT` |
| 步骤 2 — 编码实现 | `chatbot-coder` | 根据上一步的规划方案，实现完整应用 |
| 步骤 3 — 代码审查 | `chatbot-reviewer` | 请审查生成的代码文件 |

保存后，每次只需点击工作流卡片的 ▶ 执行按钮，填入需求描述（`$REQUIREMENT` 变量），三个 Agent 就会自动按顺序执行——规划完成后自动触发编码，编码完成后自动触发审查。

![](img/544925e1ecf8640661cb8d5d28c76dd5.png)

![](img/0c363341368b7ff58d9d4551f956a3a3.png)

### 5.6 方案一 vs 方案二对比

| 维度 | 方案一（一句话生成） | 方案二（多 Agent 流水线） |
| --- | --- | --- |
| 适用场景 | 快速原型、小型项目、个人开发 | 复杂项目、团队协作、工业级开发 |
| 代码质量 | 依赖单次生成质量，无内置审查 | 分阶段审查（`Reviewer` 自动检查），质量更可控 |
| 可定制性 | 低——需要在 Prompt 中描述所有要求 | 高——每个 Agent 独立配置模型、工具权限、提示词 |
| 可复用性 | 低——每次都要重新描述需求 | 高——Agent 和工作流可跨项目复用 |
| 成本控制 | 全程使用同一模型 | 精细控制——规划和编码用 Sonnet，审查用 Haiku 降本 |
| 上手难度 | 零门槛 | 需要理解 Agent 体系和工作流概念 |
| 执行耗时 | ~60 秒 | ~3-5 分钟（但质量更高） |

**核心认知**：方案一和方案二并不是"非此即彼"的关系。在实际开发中，最高效的做法是组合使用——用方案一快速生成初版原型，确认方向正确后，用方案二的 `Reviewer Agent` 进行质量审查，再根据审查建议迭代优化。

## 6. 本节小结

本节通过一个完整的 ChatBot 项目，综合运用了 Claude Code 从前端设计到后端开发的全链路能力：

| 步骤 | 核心能力 | 涉及的 Claude Code 系统 |
| --- | --- | --- |
| 需求 → HTML 原型 | 一句话生成完整前端页面 | 内置工具（`Write` / `Bash`） |
| `/frontend-design` 优化 | 官方插件提升设计品质 | 扩展系统（`Plugin`） |
| Pencil 精调 | AI 原生设计工具 + `MCP` 集成 | 扩展系统（`MCP`） |
| 方案一：一句话全栈 | 60 秒生成前后端完整应用 | 内置工具 + 项目管理（`Checkpoint`） |
| 方案二：多 Agent 流水线 | `Planner` → `Coder` → `Reviewer` 分工协作 | 多 Agent 系统 + 斜杠命令 |

**带走的核心认知**：

1. Claude Code 不只是"帮你写代码"的工具——它是一个可编排的 Agent 系统，能模拟团队协作流程

2. **Prompt 质量决定输出质量**——同样的功能，精心设计的 Prompt 和随意描述的结果天壤之别

3. **设计和开发可以无缝衔接**——通过 `/frontend-design` 和 `Pencil MCP`，从视觉设计到代码实现不再割裂

4. `Checkpoint` 让试错成本归零——大胆尝试任何方案，不满意就一键回滚

至此，《Claude Code 快速入门实战指南》的全部内容已经覆盖完毕——从零基础的安装部署、核心概念理解、六大系统巡览，到完整的工业级项目实战。你已经具备了使用 Claude Code 进行独立开发的全部基础能力。

> 本节内容节选自《2026大模型Agent智能体开发实战》
>
> 100+小时体系大课，20大模块精讲，零基础直达企业级应用！
>
> 课程中包含更多实战内容：自定义 Agent 深度开发、`MCP Server` 从零构建、企业级工作流编排、多 Agent Team 协作、以及完整的 `Fufan-CC Flow` 项目源码解析。

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2026大模型Agent智能体开发实战》

![](img/e1cc9eb4dd394f7870404dd939cb5ffd.png)

**《2026大模型Agent智能体开发实战》**为【100+小时】体系大课，总共20大模块精讲精析，零基础直达大模型企业级应用！

### 课程完整介绍

![](img/c26de222f4eee843cbcd7658853b3259.png)

详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/22265ab78e30ab8981dd4fd527f3ddae.png)

![](img/ea4678dafe722320b02a4c3895d2eaeb.png)

![](img/04fb57ae9a4673d050dc0edffdbd20e9.png)
