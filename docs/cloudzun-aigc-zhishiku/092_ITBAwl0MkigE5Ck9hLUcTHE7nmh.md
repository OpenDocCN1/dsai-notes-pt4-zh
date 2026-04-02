# GH Copilot 实验：AI 辅助开发入门

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/D9k1dF8d0o9LqKxBwmgcAdAEnid](https://zsyhjtnsa5.feishu.cn/docx/D9k1dF8d0o9LqKxBwmgcAdAEnid)

课件：

repo: https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/tree/main

# 🎯 基础练习

## LAB_AK_01 - 设置与界面

+   检查 GitHub Copilot 设置和用户界面功能

+   学习如何配置 GitHub Copilot 设置

+   探索 Visual Studio Code 中的 GitHub Copilot 功能

## LAB_AK_02 - 代码分析与文档

+   使用 GitHub Copilot 分析和编写代码文档

+   C# 版本： LAB_AK_02_analyze_document_code.md

+   Python 版本： LAB_AK_02_analyze_document_code_py.md

+   生成代码说明和文档

## LAB_AK_03 - 开发新功能

+   使用 GitHub Copilot 开发新代码功能

+   C# 版本： LAB_AK_03_develop_code_features.md

+   Python 版本： LAB_AK_03_develop_code_features_py.md

+   加速新功能的开发

## LAB_AK_04 - 单元测试

+   使用 GitHub Copilot 开发单元测试

+   C# (xUnit): LAB_AK_04_develop_unit_tests_xunit.md

+   Python (pytest): LAB_AK_04_develop_unit_tests_pytest.md

+   生成测试用例、断言和模拟数据

## LAB_AK_05 - 代码重构

+   使用 GitHub Copilot 重构现有代码

+   C# 版本： LAB_AK_05_refactor_improve_existing_code.md

+   Python 版本： LAB_AK_05_refactor_improve_existing_code_py.md

+   提升代码质量、可靠性、性能和安全性

# 🚀 高级练习

## LAB_AK_06 - Vibe Coding 原型开发

### 目的

学习如何使用 **Vibe Coding** 方法论和 GitHub Copilot Agent，通过自然语言描述而非手写代码来生成软件原型，体验从"程序员写代码"到"AI 生成代码，人类审查和引导"的开发范式转变。

### 实验步骤

1.  **定义产品需求** — 与 Copilot 协作创建 PRD（产品需求文档），明确应用目的、目标用户、核心功能和技术约束

1.  **创建线框图** — 让 Copilot 生成文本化的低保真线框图（wireframe diagrams），定义 UI 布局和用户交互

1.  **生成初始原型** — 使用 Agent 模式，让 Copilot 根据 PRD 和线框图自动生成完整的购物应用（HTML/CSS/JS）

1.  **迭代优化原型** — 通过自然语言指令让 Copilot 调整响应式断点、添加 emoji 导航图标、改进通知方式等

1.  **持续改进** — 让 Copilot 审查 PRD 并提出改进建议，逐步完善用户体验

### 实验结果

+   一个完全由 AI 生成的**购物应用原型**（Products、ProductDetails、ShoppingCart、Checkout 四个页面）

+   核心功能：

+   浏览 10 种水果产品（含 emoji 图标）

+   查看产品详情

+   管理购物车（添加/更新数量/移除）

+   结账流程

+   响应式设计（自动适配桌面和手机）

+   可折叠导航栏（< 600px 时显示缩写）

+   完整的项目文档：.github/copilot-instructions.md（包含 PRD 和线框图，作为自定义指令）

### 实际意义

将开发者的角色从"写代码"转变为"产品设计师 + 质量审查员":

+   Vibe Coding 适用于快速原型验证、MVP 开发、概念演示

+   大幅降低原型开发门槛，非技术背景的创业者也能快速验证想法

+   开发者专注于需求定义、架构决策和质量把控，而非手写 HTML/CSS/JS

+   演示了 AI Agent 模式的自主工作能力（自动创建文件、测试、调试）

---

## LAB_AK_07 - **合并重复代码**

### 目的

学习如何使用 GitHub Copilot 系统性地识别和消除代码库中的重复逻辑，通过提取共享辅助方法（helper methods）提升代码的可维护性、可读性和可测试性。

### 实验步骤

1.  **手动审查代码库** — 检查 E-commerce Orders & Returns 项目，识别 OrderProcessor 和 ReturnProcessor 中的重复方法和模式

1.  **使用 Ask 模式识别重复** — 让 Copilot 分析指定文件，识别方法级、服务级和架构级的重复模式，并按优先级排序

1.  **使用 Agent 模式合并重复** — 分阶段让 Copilot Agent 提取重复逻辑：

+   创建共享的 ValidationService 和 ShippingCalculationService

+   重构 EmailService、AuditService、InventoryService 中的辅助方法

+   处理跨模块的通用逻辑（支付/退款、状态更新、错误处理）

1.  **测试重构代码** — 对比输出文件，确保业务逻辑和错误处理（如库存回滚）完全保留

### 实验结果

+   一套**消除重复后的代码库**，将分散在多处的验证、运费计算、邮件发送等逻辑统一到共享服务

+   具体改进：

+   ValidationService — 统一 ID 验证逻辑（原本在 OrderProcessor 和 ReturnProcessor 中重复）

+   ShippingCalculationService — 合并运费计算（订单和退货的不同规则）

+   EmailService / AuditService / InventoryService — 提取模板构建、日志记录、库存更新等辅助方法

+   控制台输出与原版完全一致（包括 5 个测试场景、错误处理和回滚逻辑）

### 实际意义

从"多处维护"变成"单一事实来源":

+   修改业务规则时只需更新一处代码，不会遗漏某个副本

+   降低 bug 风险（重复代码中的细微差异往往是隐藏 bug）

+   提升代码可测试性（共享方法可独立进行单元测试）

+   适用于遗留系统重构、代码审查后的清理工作

---

## LAB_AK_08 - **重构大型函数**

### 目的

学习如何使用 GitHub Copilot 将承担多重职责的大型函数（God Methods）拆解为更小、更专注的单一职责函数，提升代码的可读性、可维护性和可测试性。

### 实验步骤

1.  **审查大型函数** — 分析 E-commerce Order Processing 项目中的 ProcessOrder 方法（包含验证、库存、支付、物流、通知、审计等多重职责）

1.  **使用 Ask 模式分析** — 让 Copilot 识别可提取的函数，提供重构计划（包括方法签名、返回类型、错误处理策略）

1.  **使用 Agent 模式重构** — 分阶段提取：

+   先创建方法桩（stub methods）并测试编译

+   逐个提取代码段到单一职责方法：ValidateOrderAndSecurity、CheckAndReserveInventory、ProcessPayment、ScheduleShipping、SendNotifications、FinalizeOrder

+   Agent 自动测试每个提取步骤，确保功能正常（包括支付失败时的库存回滚）

1.  **测试重构代码** — 运行 4 个测试用例 + 额外边缘场景（空订单、无效地址），验证审计日志完整性

### 实验结果

+   一个**高度模块化的订单处理系统**，原本 200+ 行的 ProcessOrder 方法缩减为清晰的方法调用序列

+   提取的单一职责方法：

+   ValidateOrderAndSecurity — 输入验证和安全检查

+   CheckAndReserveInventory — 库存检查和预留

+   ProcessPayment — 支付处理（含欺诈检测和错误处理）

+   ScheduleShipping — 物流协调和跟踪管理

+   SendNotifications — 客户通知和沟通

+   FinalizeOrder — 订单完成和数据记录

+   完整的审计日志（order_audit_log.txt）和测试覆盖

### 实际意义

从"一个函数做所有事"变成"每个函数只做一件事":

+   单一职责原则（SRP）使代码易于理解和修改

+   每个方法可以独立进行单元测试

+   新需求（如增加优惠券逻辑）可以插入到明确的阶段，而非修改庞大的单一函数

+   适用于遗留系统重构、微服务拆分前的准备工作

---

## LAB_AK_09 - **简化复杂条件逻辑**

### 目的

学习如何使用 GitHub Copilot 重构深度嵌套的条件逻辑，通过提取辅助方法、使用提前返回（guard clauses）、switch 表达式和布尔变量等技术，提升代码的可读性和可维护性。

### 实验步骤

1.  **审查复杂条件** — 分析 E-commerce Pricing Engine 中的 CalculateFinalPrice 方法（包含会员级别、优惠券、批量折扣等多层嵌套条件）

1.  **使用 Ask 模式分析** — 让 Copilot 识别主要折扣路径（会员/优惠券/批量）及其交互方式，提出简化策略（提取方法、减少嵌套、使用 switch/模式匹配等）

1.  **使用 Agent 模式重构** — 让 Agent 执行多阶段重构：

+   提取 ApplyMembershipDiscounts、ApplyCouponDiscounts、ApplyBulkDiscounts 辅助方法

+   在每个辅助方法内使用 guard clauses、Boolean 变量、switch 表达式减少嵌套

+   进一步拆分为 ApplyPremiumDiscounts、ApplyGoldDiscounts、ApplySilverDiscounts 等子方法

1.  **测试重构代码** — 对比输出文件，确保定价计算和安全验证逻辑完全保留

1.  **（可选）处理第二个案例** — 对 Loan Approval Workflow 应用相同技术

### 实验结果

+   一个**扁平化的定价引擎**，原本深度嵌套的 CalculateFinalPrice 方法简化为清晰的步骤序列

+   重构技术应用：

+   **方法提取** — 将会员、优惠券、批量折扣逻辑分离到独立方法

+   **Guard Clauses** — 用提前返回替代深层 if-else 嵌套

+   **Switch 表达式** — 使会员级别逻辑显式化和易扩展

+   **Boolean 变量** — 用命名变量替代复杂的内联条件（altPayment、premiumPayment 等）

+   **局部辅助函数** — 消除重复逻辑，澄清意图

+   完整的安全测试（无效输入、恶意操作）和分类折扣上限（电子产品 15%、服装 20%）

### 实际意义

从"难以理解的条件迷宫"变成"自解释的业务规则":

+   降低认知负荷，新开发者能快速理解折扣逻辑

+   修改业务规则（如调整会员等级折扣）时风险更低

+   每个辅助方法可以独立测试和验证

+   适用于电商定价、贷款审批、保险承保等复杂规则引擎的重构

## LAB_AK_10 - **性能分析与优化**

### 目的

学习如何使用 GitHub Copilot 系统性地识别和解决代码库中的性能瓶颈和低效代码，通过算法优化、缓存策略和异步处理等技术提升应用性能，同时保持代码可读性和功能完整性。

### 实验步骤

1.  **手动审查代码库** — 运行 ContosoOnlineStore 电商应用，建立基线性能指标（baseline_metrics.txt），识别潜在性能问题

1.  **使用 Ask 模式识别瓶颈** — 让 Copilot 分析 ProductCatalog、OrderProcessor、InventoryManager、EmailService 等类，识别 N+1 查询模式、低效算法、阻塞操作等问题，并评估优化方案的安全风险

1.  使用 Agent 模式重构性能关键代码 — 分阶段优化：

+   ProductCatalog — 用字典查找替代线性搜索（O（1） 性能），优化搜索算法和缓存机制

+   OrderProcessor — 消除 N+1 查询模式（批量获取产品），优化税费和运费计算，改进异步模式

+   InventoryManager — 实现批量库存更新，优化日志记录，减少阻塞操作

+   EmailService — 并行生成邮件内容，异步发送操作，改进错误处理和重试逻辑

1.  **测试和验证** — 对比优化前后的性能指标（optimized_metrics.txt），运行单元测试，确保功能正确性和性能提升

### 实验结果

+   一个**性能显著提升的电商应用**，通过代码优化（而非仅修改模拟延迟）实现性能改进

+   具体优化成果：

+   字典查找替代线性搜索（产品查询性能显著提升）

+   批量操作消除 N+1 查询模式（订单处理时间缩短）

+   并行处理和优化的异步/await 模式（并发操作性能提升）

+   高效的缓存键生成和缓存管理

+   性能基准报告（可选使用 BenchmarkDotNet 生成详细报告，包括内存分配模式和统计分析）

+   完整的单元测试套件（16 个测试全部通过）

### 实际意义

从"能跑"到"跑得快"的工程化性能优化：

+   演示如何使用 AI 工具系统性地分析和解决性能问题

+   优化策略包括算法改进、缓存策略、异步处理、批量操作等企业级最佳实践

+   保留模拟延迟允许"before and after"性能对比，清晰展示代码优化的实际效果

+   适用于电商平台、API 服务、数据处理管道等对性能敏感的生产应用

---

## LAB_AK_11 - 解决 GitHub Issues

### 目的

学习如何使用 GitHub Copilot 分析和解决 GitHub Issues 中的安全漏洞，掌握从问题识别、根因分析、代码修复到验证和关闭 Issue 的完整工作流，演示 AI 辅助的安全修复流程。

### 实验步骤

1.  **导入仓库并创建 Issues** — 导入 ContosoShopEasy 电商应用到 GitHub 账户，运行 Workflow 自动生成 10 个安全漏洞 Issues（包括 SQL 注入、信用卡数据存储违规、硬编码凭证、弱密码哈希等）

1.  审查 GitHub Issues — 在 GitHub 上查看 Issues 列表，分配 2 个高优先级 Issue 给自己：

+   🔐 Fix SQL Injection Vulnerability in Product Search （高优先级）

+   🔐 Fix Credit Card Data Storage Violations （关键优先级）

1.  **克隆仓库并审查代码** — 在 VS Code 中审查项目结构，定位漏洞位置（ProductService、PaymentService、OrderRepository、SecurityValidator 等）

1.  使用 Ask 模式分析 Issues — 使用 GitHub Pull Requests 扩展查看 Issue 详情，让 Copilot 分析：

+   SQL 注入漏洞的影响和修复方案（参数化查询 vs ORM 方法）

+   信用卡数据存储违规的 PCI DSS 合规性问题和修复策略

1.  **使用 Agent 模式修复漏洞** — 分阶段修复：

+   SQL 注入：移除模拟 SQL 查询日志，实现输入净化，更新 SecurityValidator

+   信用卡存储：移除 CVV 存储，仅存储卡号后 4 位和卡类型，更新 PaymentInfo、PaymentService、SecurityValidator

1.  **测试和验证** — 运行应用，对比修复前后的控制台输出，确认漏洞已修复且功能正常

1.  **提交变更并关闭 Issues** — 撰写详细的 commit message，推送到 GitHub，关闭已修复的 Issues（标记为"Revoked"或"Used in test"）

### 实验结果

+   一个**消除了关键安全漏洞的电商应用**，符合安全最佳实践和 PCI DSS 合规要求

+   修复内容：

+   SQL 注入防护：移除危险的 SQL 拼接，实现安全的输入处理

+   PCI DSS 合规：PaymentInfo 模型不再存储 CVV 和完整卡号，仅存储后 4 位和卡类型

+   日志清理：移除敏感数据的过度日志记录

+   完整的 Issue 跟踪记录：从识别→分析→修复→验证→关闭的完整审计追踪

+   GitHub Pull Requests 扩展的工作流集成（在 VS Code 中直接管理 Issues）

### 实际意义

演示 AI 辅助的安全漏洞修复工作流：

+   使用 GitHub Issues 作为安全问题的中心化跟踪系统

+   GitHub Copilot 提供智能分析（风险评估、修复策略、代码审查）

+   分离关注点：每个 Issue 独立修复，独立分支，便于代码审查和回滚

+   40% 的漏洞由过度日志记录引起，60% 独立于日志存在（演示多层安全问题）

+   适用于遗留系统安全加固、合规性审计后的修复工作、安全漏洞管理流程

---

## LAB_AK_12 - 解决 Secret Scanning 告警

### 目的

学习如何使用 GitHub Secret Scanning、Push Protection 和 GitHub Copilot 检测和修复代码库中的硬编码密钥，掌握安全的密钥管理最佳实践（环境变量、密钥管理服务等）。

### 实验步骤

1.  **导入仓库** — 导入 ContosoOrderProcessor 订单处理应用（Public 仓库自动启用 Secret Scanning 和 Push Protection）

1.  **审查 Security 告警** — 在 GitHub Security 标签页查看 Secret Scanning 告警（包括 Stripe API Key、Square Access Token、Mailgun API Key、Slack Token 等 10+ 个硬编码密钥）

1.  **克隆仓库并审查代码** — 在 VS Code 中定位硬编码密钥位置（AppConfig、EmailService、PaymentService 等），理解双源配置模式（部分密钥来自环境变量，部分硬编码）

1.  **配置环境变量并运行应用** — 创建 PowerShell 脚本（setup-secrets.ps1）设置环境变量，运行应用并记录原始输出（OriginalConsoleOutput.txt）

1.  **使用 Ask 模式分析告警** — 让 Copilot 分析 PaymentService、EmailService、AppConfig 中的硬编码密钥，识别：

+   Stripe API Key 和 Square Access Token 的安全风险

+   Mailgun API Key 和 SMTP 凭证（Secret Scanning 未检测到的密钥）

+   PCI DSS 合规性要求和修复策略（统一使用环境变量）

1.  **使用 Agent 模式修复密钥** — 分阶段修复：

+   PaymentService:移除硬编码 Stripe 和 Square 密钥，改用环境变量，更新 setup-secrets.ps1

+   EmailService:移除硬编码 Mailgun 和 SMTP 密钥，改用环境变量，更新 setup-secrets.ps1

+   Program.cs + AppConfig.cs：更新配置验证逻辑（ValidateRequiredConfiguration），添加新环境变量的静态属性和验证

1.  **提交变更并关闭告警** — 推送修复后的代码，在 GitHub 上关闭已修复的告警（标记为"Revoked"，添加 commit SHA 和日期说明）

1.  测试 Push Protection — 尝试修改并推送 AppConfig 中的硬编码密钥，验证 Push Protection 阻止了提交（演示预防性保护）

### 实验结果

+   一个消除了大部分硬编码密钥的订单处理应用，密钥通过环境变量安全管理

+   修复内容：

+   移除 PaymentService 和 EmailService 中的硬编码 API Keys

+   统一密钥管理策略（环境变量 + 集中验证）

+   更新配置验证逻辑（AppConfig 和 Program.cs）

+   PowerShell 脚本管理所有环境变量（AWS、SendGrid、PayPal、Azure Storage、SQL Server、GitHub、npm 等）

+   Push Protection 演示：阻止新密钥被推送到仓库

+   Secret Scanning 最佳实践：关闭告警流程（审查→移除→轮换→文档化→关闭）

### 实际意义

从"密钥硬编码"到"安全密钥管理"的完整转型：

+   GitHub Secret Scanning 检测已提交的密钥，Push Protection 阻止新密钥提交

+   环境变量是简化培训的方案，生产环境应使用 Azure Key Vault 或 AWS Secrets Manager

+   Secret Scanning 基于模式匹配，可能漏检自定义格式的密钥（需结合代码审查）

+   即使移除代码中的密钥，历史 commit 仍保留（需轮换密钥并可选地重写 Git 历史）

+   适用于遗留系统安全加固、合规性审计、DevSecOps 流程集成

# 🏗️ 实践规范驱动开发（SDD）方法论

## LAB_AK_13 - 使用 GitHub Spec Kit 实现规范驱动开发方法论

### 目的

学习如何使用 GitHub Spec Kit 实践规范驱动开发（Spec-Driven Development， SDD）方法论，通过将规范与 GitHub Copilot 深度集成，从需求文档自动生成宪章、规范、计划和任务，并最终实现一个完整的 MVP 应用。

### 实验步骤

1.  初始化 GitHub Spec Kit — 在项目文件夹中运行 specify init，生成 agents、prompts、templates、scripts 等文件结构

1.  创建项目宪章（Constitution） — 使用 /speckit.constitution 从利益相关方文档提取核心原则、技术标准和治理规则

1.  **生成功能规范（Spec）** — 使用 /speckit.specify 根据项目目标和功能文档生成详细的用户故事、验收标准和业务需求

1.  **生成技术计划（Plan）** — 使用 /speckit.plan 基于规范和宪章生成架构设计、技术栈选型、数据模型和 API 设计

1.  **生成任务分解（Tasks）** — 使用 /speckit.tasks 将技术计划拆分为分阶段的可执行任务列表（Setup → Foundation → User Stories）

1.  **实现 MVP 应用** — 使用 /speckit.implement 自动实现任务范围内的所有代码（后端 API + 前端 UI），并通过验收场景测试

### 实验结果

+   一套完整的**规范驱动开发工作流**，从需求→规范→计划→任务→实现全程由 GitHub Copilot 辅助生成

+   一个可运行的 RSS Feed Reader MVP 应用（支持添加订阅、查看订阅列表、持久化存储等功能）

+   项目文档体系：

+   constitution.md — 项目核心原则和技术治理规则

+   spec.md — 完整的功能规范和验收标准

+   plan.md + research.md + data-model.md — 技术实现蓝图

+   tasks.md — 分阶段任务清单（含 MVP 优先策略）

+   完整的后端 API（。NET） + 前端 UI（Blazor WebAssembly）实现

### 实际意义

将 AI 辅助开发从"零散代码生成"升级为"端到端规范驱动工程化流程":

+   确保业务需求（spec）与技术实现（code）之间有清晰的可追溯性

+   利益相关方的需求文档可直接驱动 Copilot 生成符合治理规则的代码

+   MVP 优先策略确保快速交付核心价值，后续功能可无缝滚动增加

+   适用于绿地项目，也可扩展到棕地项目的功能增量开发

## LAB_AK_14 - 使用 GitHub Spec Kit 为现有应用添加新功能

### 目的

学习如何在棕地项目（Brownfield）中使用 GitHub Spec Kit，基于现有代码库和利益相关方需求，从规范到实现全流程完成一个新功能的开发，掌握在真实业务场景中应用规范驱动开发的方法。

### 实验步骤

1.  导入现有项目并初始化 GitHub Spec Kit — 从 GitHub 导入 ContosoDashboard 项目，运行 specify init --here 在已有代码库中集成 Spec Kit

1.  生成项目宪章（Constitution） — 使用 /speckit.constitution 从现有代码库中提取技术标准、安全要求和开发规范

1.  **创建功能规范（Spec）** — 使用 /speckit.specify 根据"文档上传与管理"的需求文档生成详细规范（用户故事、验收标准等）

1.  **规范澄清（Clarify）** — 使用 /speckit.clarify 让 Copilot 提出针对性问题，消除规范中的模糊点和遗漏项

1.  **生成技术计划（Plan）** — 使用 /speckit.plan 基于规范和宪章生成架构设计、数据模型、API 设计和技术栈选型

1.  **生成任务分解（Tasks）** — 使用 /speckit.tasks 将技术计划拆分为分阶段的可执行任务列表

1.  **实现 MVP 功能** — 使用 /speckit.implement 自动实现任务范围内的所有代码（后端 + 前端），并通过手动测试验证

### 实验结果

+   一套适用于**棕地项目的规范驱动开发流程**，从现有代码库出发生成宪章→规范→计划→任务→实现

+   **在 Contoso**Dashboard 应用中新增一个完整的文档上传与管理功能：

+   上传文档（PDF/Office/图片/文本，最大 25MB，支持元数据）

+   按项目/类别组织文档

+   搜索和预览文档

+   基于角色的访问控制

+   与任务关联、通知集成

+   审计日志和管理报告

+   完整的项目文档体系：

+   constitution.md — 基于现有代码库提取的技术治理规则

+   spec.md — 经过澄清后的详细功能规范

+   plan.md + research.md + data-model.md — 技术实现蓝图

+   tasks.md — 分阶段任务清单（含 MVP 优先策略）

### 实际意义

在真实业务场景中实践规范驱动开发：

+   证明 Spec Kit 不仅适用于绿地项目，也能无缝集成到已有代码库

+   /speckit.clarify 澄清流程确保需求在编码前就被充分理解，避免返工

+   从利益相关方文档（StakeholderDocs）自动生成技术实现，缩短需求→代码的距离

+   MVP 优先策略确保核心功能快速交付（上传+查看文档），后续功能可增量开发

+   适用于企业内部工具开发、SaaS 功能扩展等实际场景

# ⚙️ 定制与集成

## LAB_AK_15 - 创建自定义 Copilot 指令和自定义代理

### 目的

学会如何通过自定义指令、路径指令、提示文件和自定义 Agent，让 GitHub Copilot 自动遵循项目的编码规范，并用多 Agent 协作完成从规划、实现到代码审查的完整开发流程。

### 实验步骤

1.  **创建自定义指令文件**（copilot-instructions.md）— 让 Copilot 自动遵守命名、架构、错误处理等编码标准

1.  **创建路径指令**（Controllers、Services 分别的 .instructions.md）— 针对不同代码层提供专项规范

1.  **创建可复用提示**（.prompt.md）— 封装常用任务（如生成 API 文档）为斜杠命令

1.  **定义三个自定义 Agent**：

+   **Planner**（只读工具）— 分析需求，生成实现计划

+   **Implementer**（编辑工具）— 按计划写代码

+   **Reviewer**（只读工具）— 检查代码质量、安全和规范

1.  通过 Handoffs 串联 Agent — 让 Planner → Implementer → Reviewer 形成自动化工作流

### 实验结果

+   **一套能强制 C**opilot 遵循项目规范的自定义指令体系

+   一个多 Agent 协作的开发流水线（Planning → Implementation → Review）

+   在 ContosoInventory 项目中添加一个完整的 Product Inventory 管理功能（包括模型、DTO、服务、控制器、EF 迁移等），全程由 Copilot 按企业标准生成

### 实际意义

把 Copilot 从"通用助手"变成"懂你项目规范的团队成员"，确保 AI 生成的代码从一开始就符合团队标准，不需要大量人工返工。

## LAB_AK_16 - 使用 GitHub Copilot SDK 集成 AI Agent

### 目的

学习如何使用 GitHub Copilot SDK 将 AI Agent 集成到现有应用程序中，通过自定义工具（Tools）让 AI 自动执行业务逻辑（如查询订单、处理退货），实现对话式 AI 驱动的客户支持体验。

### 实验步骤

1.  审查现有应用功能 — 导入 ContosoShop 电商支持门户（Blazor WebAssembly + ASP.NET Core），测试手动订单管理功能（查看订单、跟踪物流、退货）

1.  安装 SDK 组件 — 添加 GitHub.Copilot.SDK 和 Microsoft.Extensions.AI NuGet 包到服务端项目

1.  创建 Agent 工具服务（SupportAgentTools） — 实现 4 个工具方法供 AI 调用：

+   GetOrderDetailsAsync — 查询指定订单的状态和详情

+   GetUserOrdersSummaryAsync — 获取用户所有订单的摘要

+   ProcessReturnAsync — 处理全量或部分退货（支持指定商品 ID 和数量）

+   SendCustomerEmailAsync — 发送跟进邮件给客户

1.  配置 SDK Agent 并暴露 API 端点 — 在 Program.cs 中：

+   注册 SupportAgentTools 到 DI 容器

+   使用 AIFunctionFactory 将工具方法转换为 AI 可调用的函数

+   创建 CopilotClient 和 CopilotSession

+   添加 /api/support/chat POST 端点处理用户消息

1.  更新 Blazor 前端（Support.razor） — 构建对话式 UI:

+   添加消息输入框和聊天历史显示

+   调用后端 /api/support/chat API

+   实时显示用户和 AI 的消息交互

1.  端到端测试 — 测试对话场景：

+   "Where is my order #1004?" → AI 调用 GetOrderDetailsAsync

+   "I want to return 1 monitor from order 1004" → AI 调用 ProcessReturnAsync，更新库存并发送确认邮件

### 实验结果

+   一个集成了 AI 客户支持 Agent 的电商门户，用户可以通过自然语言对话完成订单查询和退货操作

+   核心集成成果：

+   SupportAgentTools 服务 — 4 个可被 AI 调用的工具方法，封装订单查询、退货处理、邮件发送等业务逻辑

+   后端 Agent 配置 — 使用 GitHub Copilot SDK 的 CopilotClient 和 CopilotSession，通过 AIFunctionFactory 注册自定义工具

+   RESTful API 端点 — /api/support/chat 接收用户消息，返回 AI 生成的回复

+   Blazor 对话 UI — 实时消息历史显示，支持流式对话体验

+   AI Agent 能力演示：

+   理解用户意图（"Where is my order？" → 调用订单查询工具）

+   执行业务操作（处理部分退货，更新库存，发送邮件）

+   生成自然语言回复（包含订单详情、退款金额、预计到账时间等）

### 实际意义

从"规则驱动的聊天机器人"升级为"能执行业务逻辑的 AI Agent":

+   GitHub Copilot SDK 将 Copilot CLI 的能力封装为可编程的 SDK，嵌入到企业应用

+   自定义工具（Tools）让 AI 不仅能回答问题，还能调用后端代码执行操作（查数据库、处理退货、发邮件）

+   对话式交互降低用户门槛，无需学习复杂的 UI 导航流程

+   适用于客户支持、IT 服务台、内部助手、ERP/CRM 系统的对话式界面

+   演示了 Microsoft.Extensions.AI 的 AIFunctionFactory 如何将 C# 方法转换为 AI 可调用的工具