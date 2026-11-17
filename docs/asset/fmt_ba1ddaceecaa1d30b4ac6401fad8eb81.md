
此外还有动态 MCP 工具集（`mcp-<server>`）——通过 MCP（Model Context Protocol）协议接入外部工具服务器，支持 stdio 和 HTTP 传输。

### 六种终端后端：Hermes Agent 工具系统的一大亮点

| 后端 | 适用场景 | 特点 |
| --- | --- | --- |
| `Local` | 本机命令执行 | 最简单直接，适合个人开发 |
| `Docker` | 容器隔离执行 | 安全隔离，namespace 和硬化 |
| `SSH` | 远程服务器操作 | 管理远程机器 |
| `Daytona` | 云开发环境 | 支持休眠/唤醒，按需付费 |
| `Modal` | Serverless 执行 | 无服务器架构，自动伸缩 |
| `Singularity` | HPC 集群容器 | 适合 GPU 集群和学术计算 |

通过 `config.yaml` 中的 `terminal.backend` 配置或 `TERMINAL_ENV` 环境变量切换，对 Agent 完全透明。

### MCP 集成与插件架构

v0.3.0 引入了插件系统，v0.8.0 进一步扩展。插件从三个位置发现：

+   `~/.hermes/plugins/`（用户级）
+   `.hermes/plugins/`（项目级）
+   `pip` entry points（Python 包）

插件可以注册工具、Hook、CLI 子命令，接收请求级别的 API Hook（带 correlation ID），提示安装时的环境变量需求，以及 Hook 进会话生命周期事件（`finalize`/`reset`）。

### 记忆系统

记忆系统是 Hermes Agent "越用越强"的基石。它不是简单的"聊天记录存储"，而是一个多层、多后端、可插拔的持久记忆体系。

#### 四层记忆架构

| 层 | 存储 | 内容 | 温度 |
| --- | --- | --- | --- |
| `MEMORY.md` | Markdown 文件 | Agent 自维护的长期事实记忆——你的开发环境、偏好、项目约定 | 热（每次对话注入系统提示词） |
| `USER.md` | Markdown 文件 | 用户画像——沟通风格、工作习惯、技能水平 | 热 |
| `SQLite FTS5` | 本地数据库 | 全部历史会话的全文搜索索引 + LLM 摘要 | 冷（按需检索） |
| External Memory Providers | 可插拔后端 | v0.7.0 后支持 8 种外部记忆后端 | 冷/热视配置而定 |

前两层（`MEMORY.md`、`USER.md`）是"热"记忆——每次对话开始时 `Prompt Builder` 会自动将它们注入系统提示词，Agent 天然就"知道"这些信息。`SQLite FTS5` 是"冷"记忆——Agent 通过 `session_search` 工具按需检索历史会话，类似于翻阅笔记本。

#### 8 种外部 Memory Provider（v0.7.0+）

| Provider | 特点 |
| --- | --- |
| `Honcho` | 辩证用户建模——不是简单存储，而是通过辩证推理（dialectic reasoning）主动分析用户的偏好、目标、行为模式 |
| `Mem0` | AI 原生记忆层 |
| `OpenViking` | 向量记忆后端 |
| `Hindsight` | 语义/图检索混合记忆 |
| `Holographic` | 全息记忆后端 |
| `RetainDB` | 持久化记忆数据库 |
| `ByteRover` | 字节级记忆 |
| `Supermemory` | 超级记忆后端 |

其中 `Honcho` 值得特别关注。普通的记忆系统是"你告诉我什么我记什么"，`Honcho` 则更进一步——它在每次对话结束后主动分析对话内容，推导出关于用户的"结论"（`conclusions`）：你的偏好、习惯、目标、沟通风格。这些结论随时间积累，Agent 对你的理解会越来越深入，甚至超越你明确告诉它的信息。

> 实用建议：`Honcho` 提供了很强大的用户建模能力，但有一个坑——它默认是关闭的。尽管 Hermes Agent 的口号是"越用越强"，但最核心的用户建模组件需要你手动在 `config.yaml` 中启用。这是目前社区反馈最集中的吐槽点之一。Lesson 2 案例 C 会手把手带你配置。

### Gateway 网关架构

Gateway 是 Hermes Agent 连接外部世界的桥梁——它让 Agent 不只是一个终端工具，而是一个随时可触达的 AI 助手。

#### 三层架构

```
Platform Layer（平台适配层）
  ├─ Telegram Adapter
  ├─ Discord Adapter
  ├─ Slack Adapter
  ├─ WhatsApp Adapter
  ├─ Signal Adapter
  ├─ Feishu/Lark Adapter     ← 飞书
  ├─ DingTalk Adapter         ← 钉钉
  ├─ WeCom Adapter             ← 企业微信
  ├─ Matrix Adapter
  ├─ Mattermost Adapter
  ├─ Email (IMAP/SMTP) Adapter
  ├─ SMS (Twilio) Adapter
  ├─ Home Assistant Adapter
  ├─ BlueBubbles Adapter
  └─ Webhook Adapter（通用 HTTP 接入）

Orchestration Layer（编排层）
  ├─ Authorization（Allowlist + DM Pairing）
  ├─ Session Routing（per-chat 会话隔离）
  ├─ Message Delivery（平台专属消息格式转换）
  └─ Hook System（可扩展钩子）

Agent Layer（Agent 层）
  └─ AIAgent（与 CLI 完全相同的核心引擎）
```

#### 关键设计决策

+   **单进程多渠道**：启动一个 `hermes gateway` 进程，即可同时服务飞书、钉钉、Telegram 等所有已配置平台。不需要每个平台单独部署一个实例。
+   **`per-chat` 会话管理**：每个聊天（`chat/channel/DM`）维护独立的会话状态和记忆，不同用户之间完全隔离。
+   **跨平台记忆共享**：虽然会话是隔离的，但 `MEMORY.md` 和 `USER.md` 是共享的。你在 Telegram 上教会 Agent 的偏好，在飞书上同样生效。
+   **平台专属消息格式转换**：`delivery.py` 模块自动将 Agent 的 Markdown 输出转换为各平台的原生富文本格式——Telegram 的 HTML、Slack 的 `Block Kit`、飞书的卡片消息等。

#### 完整的 14+ 平台列表

| 平台 | 引入版本 | 备注 |
| --- | --- | --- |
| Telegram | v0.2.0 | 长轮询 + Webhook 双模式 |
| Discord | v0.2.0 | 支持语音频道 |
| Slack | v0.2.0 | v0.6.0 加入多 Workspace OAuth |
| WhatsApp | v0.2.0 | 通过 WhatsApp Business API |
| Signal | v0.4.0 | 端到端加密 |
| Email | v0.2.0 | IMAP/SMTP |
| Home Assistant | v0.2.0 | 智能家居集成 |
| DingTalk（钉钉） | v0.4.0 | 企业版机器人 |
| SMS | v0.4.0 | 通过 Twilio |
| Mattermost | v0.4.0 | 自托管团队聊天 |
| Matrix | v0.4.0 | v0.8.0 升级为 Tier 1：反应、已读回执、富文本、房间管理 |
| Webhook | v0.4.0 | 通用 HTTP 接入，对接任意系统 |
| Feishu/Lark（飞书） | v0.6.0 | 国际版 Lark 和国内版飞书均支持 |
| WeCom（企业🛰️） | v0.6.0 | 企业内部沟通 |

> 注：个人🛰️（`WeChat`）目前不在官方支持列表中。社区有第三方 Bridge 方案，但稳定性参差不齐。

### Skill 系统与 GEPA 自演化

如果说记忆系统让 Agent "记住经验"，那 Skill 系统就让 Agent "把经验变成能力"，而 GEPA 则让 Agent "自动优化这些能力"。三者组合在一起，构成了"越用越强"最核心的闭环。

#### 技能自动生成

当你让 Hermes Agent 完成一个复杂任务（比如"分析这个 GitHub 仓库的代码质量并生成报告"），如果任务成功完成，Agent 会自动将这个过程提炼为一个可复用的技能（Skill），保存在 `~/.hermes/skills/` 目录下。

技能遵循 `agentskills.io` 开放标准——一个简单的文件夹结构，核心是一个 `SKILL.md` 文件，包含 YAML 元数据和 Markdown 格式的操作步骤、注意事项、验证方法。这意味着：

+   Hermes Agent 创建的技能可以被其他兼容 `agentskills.io` 的 Agent 平台使用
+   你可以手动编辑技能文件来微调 Agent 的行为
+   社区可以共享和交换技能（实际上已经有人在做了——`awesome-hermes-agent` 列表中收录了大量社区技能）

技能的加载采用渐进式披露（progressive disclosure）：启动时只加载技能的名称和描述（几个 token），完整的技能指令在使用时按需加载，最小化 token 消耗。

Hermes Agent 内置了 70+ 个 bundled 和 optional skills，覆盖 15+ 个类别——从代码分析、数据处理到系统管理、内容创作。

#### GEPA 进化优化（Reflective Prompt Evolution）

GEPA（反思式提示词进化，Reflective Prompt Evolution）是 Hermes Agent 最具学术深度的组件。它基于 Stanford 大学的 `DSPy` 框架（提示词自动优化框架），核心论文 "GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning"（Agrawal et al.）被接收为 ICLR 2026 Oral Paper——这是顶级机器学习会议上仅约 1% 的论文能获得的殊荣。

GEPA 的核心思路：

1. **执行任务** → 记录完整的执行轨迹（`trace`），包括每一步的输入、输出、错误信息、性能数据
2. **反思分析** → 不是简单地看"成功还是失败"（那是 `RL` 的做法），而是用 `LLM` 阅读完整轨迹，理解为什么失败——是提示词不够清晰？是工具选择错误？是步骤遗漏？
3. **进化搜索** → 用自然语言反馈进行多目标进化搜索，自动变异和改进技能的提示词和代码
4. **验证评估** → 在测试任务集上评估改进后的技能，只保留真正变好的变异

论文数据：GEPA 在所有任务和模型上平均超越 `MIPROv2`（`DSPy` 之前的优化器）13%，在 `MATH` 基准测试上用 `DSPy` `ChainOfThought` 程序达到 93% 准确率（未优化基线为 67%）。

**实际使用成本**：每次优化运行 $2-10 的 API 调用费用，不需要 GPU。技能作为 `DSPy` 模块被包装，纯文本形式，易于变异，可直接测量效果。

独立仓库 [NousResearch/hermes-agent-self-evolution](https://github.com/NousResearch/hermes-agent-self-evolution) 提供了完整的实现。

### 3.3 用户社区与真实反馈

官方文档告诉你一个项目"能做什么"，但只有真实用户才能告诉你它"实际做得怎样"。我们从多个渠道收集了社区反馈，好的坏的都给你看。

#### Reddit 社区反馈

Reddit 的 `r/LocalLLaMA` 和 `r/AIAgents` 社区是 Hermes Agent 讨论最活跃的地方。几组有意思的数据和观点：

迁移用户的体验对比（`OpenClaw` → Hermes）：

+   多位用户反馈，执行同一个终端任务，`OpenClaw` 需要 50+ 次 `tool calls`，而 Hermes Agent 用 5 次就完成了，速度快了约 2.5 分钟。原因是 Hermes Agent 的 `terminal` 工具覆盖面更广（一次调用可以执行多步操作），而 `OpenClaw` 的工具设计更细粒度。
+   但也有用户指出：`OpenClaw` 的细粒度控制在审计和调试时更有优势——你能清楚看到 Agent 每一步做了什么。Hermes 的"大步快走"风格有时会让出错时难以定位问题。

#### Token 消耗的真相

+   一位 Reddit 用户做了精确测量，发现 Hermes Agent 每次 API 调用中 73% 是固定开销——光是列出所有可用工具的 `schema` 就要烧掉约 14,000 tokens，还没读你的消息就花了这么多。这在使用按 token 计费的模型时是一个需要注意的成本因素。

> 实用建议：可以通过 `config.yaml` 中的 `toolsets.disabled` 配置关闭不需要的工具集来降低固定开销。比如你不用浏览器自动化，关掉 `browser` `toolset` 可以省下不少 `token`。

#### 开发者评价

##### DEV.to 社区评价

+   "Hermes Agent: A Self-Improving AI Agent That Runs Anywhere" — 文章重点赞了跨平台一致性和技能自生成机制
+   一位评测者在 v0.5.0 上测试：11 分钟完成安装，2 小时内 Agent 自动创建了 3 个技能文档，用这些技能完成同类研究任务时速度提升 40%

##### Medium 长文评测

+   "The Quiet Shift in AI Agents: Why Hermes Is Gaining Ground Beyond `OpenClaw`" — 重点分析了 Hermes 在"记忆 + 技能 + 进化"三维度上的差异化优势
+   "`ElizaOS` vs. `OpenClaw` vs. Hermes: What Actually Matters in 2026" — 三方对比中 Hermes 在自演化和安全性维度领先（零 Agent `CVE`），但在生态规模上落后于 `OpenClaw`

##### 非技术用户体验

+   一位自称"非技术用户"的评测者写道："this one feels like it was made for people like me"——安装简单（一行命令），任务配置是`if-then`逻辑而非真正的编程
+   另一位用户 1 小时内就用 Hermes Agent 搭建了一个"自动收集 `Reddit` 和 `X` 上 Top 5 热门 AI 开源话题"的定时报告系统

#### 已知问题和坑

社区反馈中被提到最多的问题，我们直接列出来，省得你踩：

| 问题 | 严重程度 | 现状 |
| --- | --- | --- |
| `Honcho` 默认关闭 | 高——直接影响"越用越强"的体验 | 需手动在 `config.yaml` 中启用 |
| `Token` 固定开销高 | 中——使用按量计费模型时费用明显 | 可通过禁用不需要的 toolset 缓解 |
| 文档不足 | 中——官方文档覆盖面不错但深度不够 | 社区在不断补充第三方教程 |
| 多 Agent 协作弱 | 中——只有 `delegate_task`，无真正多 Agent 通信 | 在路线图中（GitHub Issue #344） |
| 破坏性版本变更风险 | 中——迭代太快，版本间不保证完全兼容 | 建议锁定版本使用 |
| `Skills` 自动创建不可控 | 低——Agent 自动创建的技能不一定符合预期 | `OpenClaw` 的做法是需要用户确认才创建 |

#### 社区生态

围绕 Hermes Agent 已经形成了一个活跃的社区生态：

`awesome-hermes-agent`——社区策展的资源列表，收录了 80+ 个项目：

| 类别 | 数量 | 代表项目 |
| --- | --- | --- |
| 官方资源 | 8 | 核心仓库、文档站、self-evolution |
| 社区技能 | 8 | 跨平台技能库（380+ Stars） |
| `agentskills.io` 生态 | 10 | `Chainlink` 官方技能、安全审计技能 |
| 插件 | 7 | 目标管理、Agent 桥接、模型选择、成本控制 |
| 工具和 UI | 10+ | `Mission Control`（3,700+ Stars）、`Hermes Workspace`（500+ Stars） |
| 部署方案 | 4 | `Docker`、`Nix`、`WSL`、云部署 |
| 多 Agent 编排 | 4 | 跨 Agent 桥接、Swarm |
| 领域应用 | 10+ | 机器人、区块链、法律、创意写作、DevOps |

**成熟度分布**：约 15 个项目达到 `Production` 级别，40 个处于 `Beta`，25 个处于 `Experimental` 阶段。整体生态还很年轻，但增速很快。

### 3.4 OpenClaw vs Hermes Agent：两个通用 Agent 的不同侧重

很多同学的第一个问题是："我已经在用 `OpenClaw` 了，Hermes Agent 跟它是什么关系？要替换吗？"
