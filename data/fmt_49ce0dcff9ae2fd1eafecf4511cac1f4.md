
+ GitHub Copilot 提供智能分析（风险评估、修复策略、代码审查）

+ 分离关注点：每个 Issue 独立修复，独立分支，便于代码审查和回滚

+ 40% 的漏洞由过度日志记录引起，60% 独立于日志存在（演示多层安全问题）

+ 适用于遗留系统安全加固、合规性审计后的修复工作、安全漏洞管理流程

---

## LAB_AK_12 - 解决 Secret Scanning 告警

### 目的

学习如何使用 GitHub Secret Scanning、Push Protection 和 GitHub Copilot 检测和修复代码库中的硬编码密钥，掌握安全的密钥管理最佳实践（环境变量、密钥管理服务等）。

### 实验步骤

1. **导入仓库** — 导入 `ContosoOrderProcessor` 订单处理应用（Public 仓库自动启用 Secret Scanning 和 Push Protection）

1. **审查 Security 告警** — 在 GitHub Security 标签页查看 Secret Scanning 告警（包括 Stripe API Key、Square Access Token、Mailgun API Key、Slack Token 等 10+ 个硬编码密钥）

1. **克隆仓库并审查代码** — 在 VS Code 中定位硬编码密钥位置（`AppConfig`、`EmailService`、`PaymentService` 等），理解双源配置模式（部分密钥来自环境变量，部分硬编码）

1. **配置环境变量并运行应用** — 创建 PowerShell 脚本（`setup-secrets.ps1`）设置环境变量，运行应用并记录原始输出（`OriginalConsoleOutput.txt`）

1. **使用 Ask 模式分析告警** — 让 Copilot 分析 `PaymentService`、`EmailService`、`AppConfig` 中的硬编码密钥，识别：

   + Stripe API Key 和 Square Access Token 的安全风险

   + Mailgun API Key 和 SMTP 凭证（Secret Scanning 未检测到的密钥）

   + PCI DSS 合规性要求和修复策略（统一使用环境变量）

1. **使用 Agent 模式修复密钥** — 分阶段修复：

   + `PaymentService`: 移除硬编码 Stripe 和 Square 密钥，改用环境变量，更新 `setup-secrets.ps1`

   + `EmailService`: 移除硬编码 Mailgun 和 SMTP 密钥，改用环境变量，更新 `setup-secrets.ps1`

   + `Program.cs` + `AppConfig.cs`：更新配置验证逻辑（`ValidateRequiredConfiguration`），添加新环境变量的静态属性和验证

1. **提交变更并关闭告警** — 推送修复后的代码，在 GitHub 上关闭已修复的告警（标记为 `Revoked`，添加 commit SHA 和日期说明）

1. 测试 Push Protection — 尝试修改并推送 `AppConfig` 中的硬编码密钥，验证 Push Protection 阻止了提交（演示预防性保护）

### 实验结果

+ 一个消除了大部分硬编码密钥的订单处理应用，密钥通过环境变量安全管理

+ 修复内容：

  + 移除 `PaymentService` 和 `EmailService` 中的硬编码 API Keys

  + 统一密钥管理策略（环境变量 + 集中验证）

  + 更新配置验证逻辑（`AppConfig` 和 `Program.cs`）

  + PowerShell 脚本管理所有环境变量（AWS、SendGrid、PayPal、Azure Storage、SQL Server、GitHub、`npm` 等）

  + Push Protection 演示：阻止新密钥被推送到仓库

  + Secret Scanning 最佳实践：关闭告警流程（审查→移除→轮换→文档化→关闭）

### 实际意义

从"密钥硬编码"到"安全密钥管理"的完整转型：

+ GitHub Secret Scanning 检测已提交的密钥，Push Protection 阻止新密钥提交

+ 环境变量是简化培训的方案，生产环境应使用 Azure Key Vault 或 AWS Secrets Manager

+ Secret Scanning 基于模式匹配，可能漏检自定义格式的密钥（需结合代码审查）

+ 即使移除代码中的密钥，历史 commit 仍保留（需轮换密钥并可选地重写 Git 历史）

+ 适用于遗留系统安全加固、合规性审计、DevSecOps 流程集成

# 🏗️ 实践规范驱动开发（SDD）方法论

## LAB_AK_13 - 使用 GitHub Spec Kit 实现规范驱动开发方法论

### 目的

学习如何使用 GitHub Spec Kit 实践规范驱动开发（Spec-Driven Development，SDD）方法论，通过将规范与 GitHub Copilot 深度集成，从需求文档自动生成宪章、规范、计划和任务，并最终实现一个完整的 MVP 应用。

### 实验步骤

1. 初始化 GitHub Spec Kit — 在项目文件夹中运行 `specify init`，生成 `agents`、`prompts`、`templates`、`scripts` 等文件结构

1. 创建项目宪章（Constitution） — 使用 `/speckit.constitution` 从利益相关方文档提取核心原则、技术标准和治理规则

1. 生成功能规范（Spec） — 使用 `/speckit.specify` 根据项目目标和功能文档生成详细的用户故事、验收标准和业务需求

1. 生成技术计划（Plan） — 使用 `/speckit.plan` 基于规范和宪章生成架构设计、技术栈选型、数据模型和 API 设计

1. 生成任务分解（Tasks） — 使用 `/speckit.tasks` 将技术计划拆分为分阶段的可执行任务列表（Setup → Foundation → User Stories）

1. 实现 MVP 应用 — 使用 `/speckit.implement` 自动实现任务范围内的所有代码（后端 API + 前端 UI），并通过验收场景测试

### 实验结果

+ 一套完整的规范驱动开发工作流，从需求→规范→计划→任务→实现全程由 GitHub Copilot 辅助生成

+ 一个可运行的 RSS Feed Reader MVP 应用（支持添加订阅、查看订阅列表、持久化存储等功能）

+ 项目文档体系：

  + `constitution.md` — 项目核心原则和技术治理规则

  + `spec.md` — 完整的功能规范和验收标准

  + `plan.md` + `research.md` + `data-model.md` — 技术实现蓝图

  + `tasks.md` — 分阶段任务清单（含 MVP 优先策略）

  + 完整的后端 API（.NET）+ 前端 UI（Blazor WebAssembly）实现

### 实际意义

将 AI 辅助开发从"零散代码生成"升级为"端到端规范驱动工程化流程":

+ 确保业务需求（spec）与技术实现（code）之间有清晰的可追溯性

+ 利益相关方的需求文档可直接驱动 Copilot 生成符合治理规则的代码

+ MVP 优先策略确保快速交付核心价值，后续功能可无缝滚动增加

+ 适用于绿地项目，也可扩展到棕地项目的功能增量开发

## LAB_AK_14 - 使用 GitHub Spec Kit 为现有应用添加新功能

### 目的

学习如何在棕地项目（Brownfield）中使用 GitHub Spec Kit，基于现有代码库和利益相关方需求，从规范到实现全流程完成一个新功能的开发，掌握在真实业务场景中应用规范驱动开发的方法。

### 实验步骤

1. 导入现有项目并初始化 GitHub Spec Kit — 从 GitHub 导入 `ContosoDashboard` 项目，运行 `specify init --here` 在已有代码库中集成 Spec Kit

1. 生成项目宪章（Constitution） — 使用 `/speckit.constitution` 从现有代码库中提取技术标准、安全要求和开发规范

1. 创建功能规范（Spec） — 使用 `/speckit.specify` 根据"文档上传与管理"的需求文档生成详细规范（用户故事、验收标准等）

1. 规范澄清（Clarify） — 使用 `/speckit.clarify` 让 Copilot 提出针对性问题，消除规范中的模糊点和遗漏项

1. 生成技术计划（Plan） — 使用 `/speckit.plan` 基于规范和宪章生成架构设计、数据模型、API 设计和技术栈选型

1. 生成任务分解（Tasks） — 使用 `/speckit.tasks` 将技术计划拆分为分阶段的可执行任务列表

1. 实现 MVP 功能 — 使用 `/speckit.implement` 自动实现任务范围内的所有代码（后端 + 前端），并通过手动测试验证

### 实验结果

+ 一套适用于棕地项目的规范驱动开发流程，从现有代码库出发生成宪章→规范→计划→任务→实现

+ 在 `ContosoDashboard` 应用中新增一个完整的文档上传与管理功能：

  + 上传文档（PDF/Office/图片/文本，最大 25MB，支持元数据）

  + 按项目/类别组织文档

  + 搜索和预览文档

  + 基于角色的访问控制

  + 与任务关联、通知集成

  + 审计日志和管理报告

+ 完整的项目文档体系：

  + `constitution.md` — 基于现有代码库提取的技术治理规则

  + `spec.md` — 经过澄清后的详细功能规范

  + `plan.md` + `research.md` + `data-model.md` — 技术实现蓝图

  + `tasks.md` — 分阶段任务清单（含 MVP 优先策略）

### 实际意义

在真实业务场景中实践规范驱动开发：

+ 证明 Spec Kit 不仅适用于绿地项目，也能无缝集成到已有代码库

+ `/speckit.clarify` 澄清流程确保需求在编码前就被充分理解，避免返工

+ 从利益相关方文档（`StakeholderDocs`）自动生成技术实现，缩短需求→代码的距离

+ MVP 优先策略确保核心功能快速交付（上传+查看文档），后续功能可增量开发

+ 适用于企业内部工具开发、SaaS 功能扩展等实际场景

# ⚙️ 定制与集成

## LAB_AK_15 - 创建自定义 Copilot 指令和自定义代理

### 目的

学会如何通过自定义指令、路径指令、提示文件和自定义 Agent，让 GitHub Copilot 自动遵循项目的编码规范，并用多 Agent 协作完成从规划、实现到代码审查的完整开发流程。

### 实验步骤

1. 创建自定义指令文件（`copilot-instructions.md`）— 让 Copilot 自动遵守命名、架构、错误处理等编码标准

1. 创建路径指令（`Controllers`、`Services` 分别的 `.instructions.md`）— 针对不同代码层提供专项规范

1. 创建可复用提示（`.prompt.md`）— 封装常用任务（如生成 API 文档）为斜杠命令

1. 定义三个自定义 Agent：

   + `Planner`（只读工具）— 分析需求，生成实现计划

   + `Implementer`（编辑工具）— 按计划写代码

   + `Reviewer`（只读工具）— 检查代码质量、安全和规范

1. 通过 Handoffs 串联 Agent — 让 Planner → Implementer → Reviewer 形成自动化工作流

### 实验结果

+ 一套能强制 Copilot 遵循项目规范的自定义指令体系

+ 一个多 Agent 协作的开发流水线（Planning → Implementation → Review）

+ 在 `ContosoInventory` 项目中添加一个完整的 Product Inventory 管理功能（包括模型、DTO、服务、控制器、EF 迁移等），全程由 Copilot 按企业标准生成

### 实际意义

把 Copilot 从"通用助手"变成"懂你项目规范的团队成员"，确保 AI 生成的代码从一开始就符合团队标准，不需要大量人工返工。

## LAB_AK_16 - 使用 GitHub Copilot SDK 集成 AI Agent

### 目的

学习如何使用 GitHub Copilot SDK 将 AI Agent 集成到现有应用程序中，通过自定义工具（Tools）让 AI 自动执行业务逻辑（如查询订单、处理退货），实现对话式 AI 驱动的客户支持体验。

### 实验步骤

1. 审查现有应用功能 — 导入 `ContosoShop` 电商支持门户（Blazor WebAssembly + ASP.NET Core），测试手动订单管理功能（查看订单、跟踪物流、退货）

1. 安装 SDK 组件 — 添加 `GitHub.Copilot.SDK` 和 `Microsoft.Extensions.AI` NuGet 包到服务端项目

1. 创建 Agent 工具服务（`SupportAgentTools`）— 实现 4 个工具方法供 AI 调用：

   + `GetOrderDetailsAsync` — 查询指定订单的状态和详情

   + `GetUserOrdersSummaryAsync` — 获取用户所有订单的摘要

   + `ProcessReturnAsync` — 处理全量或部分退货（支持指定商品 ID 和数量）

   + `SendCustomerEmailAsync` — 发送跟进邮件给客户

1. 配置 SDK Agent 并暴露 API 端点 — 在 `Program.cs` 中：

   + 注册 `SupportAgentTools` 到 DI 容器

   + 使用 `AIFunctionFactory` 将工具方法转换为 AI 可调用的函数

   + 创建 `CopilotClient` 和 `CopilotSession`

   + 添加 `/api/support/chat` POST 端点处理用户消息

1. 更新 Blazor 前端（`Support.razor`）— 构建对话式 UI:

   + 添加消息输入框和聊天历史显示

   + 调用后端 `/api/support/chat` API

   + 实时显示用户和 AI 的消息交互

1. 端到端测试 — 测试对话场景：

   + "Where is my order #1004?" → AI 调用 `GetOrderDetailsAsync`

   + "I want to return 1 monitor from order 1004" → AI 调用 `ProcessReturnAsync`，更新库存并发送确认邮件

### 实验结果

+ 一个集成了 AI 客户支持 Agent 的电商门户，用户可以通过自然语言对话完成订单查询和退货操作

+ 核心集成成果：

  + `SupportAgentTools` 服务 — 4 个可被 AI 调用的工具方法，封装订单查询、退货处理、邮件发送等业务逻辑

  + 后端 Agent 配置 — 使用 GitHub Copilot SDK 的 `CopilotClient` 和 `CopilotSession`，通过 `AIFunctionFactory` 注册自定义工具

  + RESTful API 端点 — `/api/support/chat` 接收用户消息，返回 AI 生成的回复

  + Blazor 对话 UI — 实时消息历史显示，支持流式对话体验

  + AI Agent 能力演示：

    + 理解用户意图（"Where is my order？" → 调用订单查询工具）

    + 执行业务操作（处理部分退货，更新库存，发送邮件）

    + 生成自然语言回复（包含订单详情、退款金额、预计到账时间等）

### 实际意义

从"规则驱动的聊天机器人"升级为"能执行业务逻辑的 AI Agent":

+ GitHub Copilot SDK 将 Copilot CLI 的能力封装为可编程的 SDK，嵌入到企业应用

+ 自定义工具（Tools）让 AI 不仅能回答问题，还能调用后端代码执行操作（查数据库、处理退货、发邮件）

+ 对话式交互降低用户门槛，无需学习复杂的 UI 导航流程

+ 适用于客户支持、IT 服务台、内部助手、ERP/CRM 系统的对话式界面

+ 演示了 `Microsoft.Extensions.AI` 的 `AIFunctionFactory` 如何将 C# 方法转换为 AI 可调用的工具
