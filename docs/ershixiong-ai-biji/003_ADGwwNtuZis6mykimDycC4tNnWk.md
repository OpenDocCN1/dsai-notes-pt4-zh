
# Supabase 模型上下文协议（MCP）服务器深度对比分析

> 来源：[https://qianniucity.feishu.cn/docx/JJQud7wYvoBiv0xjc95cPOidnDb](https://qianniucity.feishu.cn/docx/JJQud7wYvoBiv0xjc95cPOidnDb)

> 来自用户的提问：alexander 发布的 Supabase MCP Server 和 官方发布的有什么区别

Supabase 刚刚发布自己家的 MCP Server，它提供了对平台更广泛的能力，以下是内容参考了 Deep Research 的结果

## 引言：理解 MCP 服务器在 Supabase 与 AI 工具集成中的作用

模型上下文协议（MCP）是一种新兴的标准，旨在规范大型语言模型（LLM）与外部服务（如 Supabase）之间的通信方式。通过定义一套通用的交互规则，MCP 使得 AI 助手（例如 Cursor、Claude 和 Windsurf）能够直接与 Supabase 项目进行交互，执行诸如管理数据库表、获取配置信息和查询数据等任务。这种集成为 Supabase 的使用带来了诸多益处，例如自动化重复性数据库操作、简化数据库管理流程以及增强开发工作效率。MCP 的出现标志着 AI 驱动的开发和管理模式的兴起，为开发者提供了一种标准化的方式将 AI 工具融入其工作流程，从而有可能提高效率并为与云平台（如 Supabase）的交互开辟新的可能性。这种标准化协议的意义在于，它类似于编程语言服务器协议（LSP），后者通过标准化不同集成开发环境（IDE）与编程语言之间的交互，极大地提升了开发体验。

Supabase 作为一个开源的 Firebase 替代方案，提供了一系列强大的后端服务，包括可扩展的 PostgreSQL 数据库、实时订阅、用户身份验证和强大的 API。将 Supabase 与 AI 工具集成对于提升开发效率至关重要。用户提出的问题是关于如何在两个主要的 Supabase MCP 服务器实现之间进行选择，这表明了对利用 AI 工具增强 Supabase 工作流程的浓厚兴趣。本文旨在对这两个 MCP 服务器进行深入的对比分析，以便用户能够根据自身的需求做出更合适的选择。

## 两个 Supabase MCP 服务器概览

### 2.1 `supabase-community/supabase-mcp` (官方 Supabase MCP 服务器)

该项目的目标是充当桥梁，将用户的 Supabase 项目连接到各种 AI 助手。它实现了模型上下文协议（MCP），以标准化 LLM 与 Supabase 等外部服务的交互。该服务器提供了一系列工具，涵盖项目管理、数据库操作、项目配置和实验性的分支管理功能。值得注意的是，它还包含一个 `PostgREST` MCP 服务器，用于通过 REST API 连接用户。官方服务器似乎侧重于提供一套全面的工具，覆盖 Supabase 平台的各个方面，旨在与广泛的 AI 助手集成。包含 `PostgREST` 服务器也表明其重视面向用户的 API 交互。该项目的技术栈主要基于 Node.js 和 TypeScript。

### 2.2 `alexander-zuev/supabase-mcp-server` (Query MCP 服务器)

该项目的目标是实现通过聊天界面对 Supabase 进行端到端管理。它特别关注安全地运行 SQL 查询、管理数据库模式更改、使用 Supabase 管理 API 和 Auth Admin SDK。该服务器强调内置的安全控制，包括只读和读写模式、运行时 SQL 验证和三级安全系统。此外，它还具备数据库模式更改的自动版本控制功能。为了更好地与 `Cursor` 和 `Windsurf` 集成，该服务器还提供了预构建的工具。自 `v0.4` 版本起，该服务器需要从 `thequery.dev` 获取 API 密钥。`Query MCP` 服务器似乎优先考虑数据库交互，并高度重视安全性和模式管理的自动化。特别提及 `Cursor` 和 `Windsurf` 表明其专注于开发者 IDE 的集成。API 密钥的要求则引入了对外部服务的依赖。该项目的技术栈主要基于 `Python 3.12+` 并使用了 `asyncpg` 库。

## 功能特性详细对比

### 3.1. Supabase 项目管理能力

`supabase-community/supabase-mcp` 提供了列出、获取项目详情、创建、暂停和恢复项目以及管理组织的工具。相比之下，`alexander-zuev/supabase-mcp-server` 则提供了使用 Supabase 管理 API 管理 Supabase 项目的工具。具体工具包括 `send_management_api_request` 和 `get_management_api_spec`。虽然两者都提供项目管理功能，但官方服务器提供了一套更明确的高级工具来执行常见操作，而另一个服务器则采用更通用的 API 请求方式。对于基本的项目管理任务，官方服务器可能由于其特定的工具而更易于使用。然而，`alexander-zuev/supabase-mcp-server` 的方法通过直接访问 API 提供了更大的灵活性，尽管这可能需要对 Supabase 管理 API 有更深入的理解。

### 3.2. 数据库操作

#### 3.2.1. 查询

`supabase-community/supabase-mcp` 提供了 `execute_sql` 工具来运行原始 SQL 查询。而 `alexander-zuev/supabase-mcp-server` 则提供了 `execute_postgresql` 工具，该工具具备风险评估和安全控制功能（包括安全、写入和破坏性模式）。两者都允许执行 SQL 查询，但 `alexander-zuev/supabase-mcp-server` 在内置安全机制和风险管理方面具有显著优势。对于担心意外破坏性操作的用户来说，`alexander-zuev/supabase-mcp-server` 的安全特性提供了一个至关重要的保护层。控制只读和读写模式的能力尤其有价值。

#### 3.2.2. 模式管理

`supabase-community/supabase-mcp` 提供了列出表、扩展和迁移以及应用迁移的工具。`alexander-zuev/supabase-mcp-server` 则提供了列出模式、表和获取表模式的工具，同时还能够执行模式更改 SQL 并自动进行迁移版本控制。`alexander-zuev/supabase-mcp-server` 的自动迁移版本控制功能非常突出，可以显著简化数据库模式的更改过程。对于需要频繁更新模式的项目来说，`alexander-zuev/supabase-mcp-server` 的自动迁移功能可以节省开发者的时间并降低手动错误的风险，这是一个显著的优势。

#### 3.2.3. 迁移

`supabase-community/supabase-mcp` 包含 `apply_migration` 用于应用 SQL 迁移和 `list_migrations` 用于列出迁移。`alexander-zuev/supabase-mcp-server` 则提供了 `retrieve_migrations` 工具，该工具具有过滤和分页功能，并且能够为模式更改自动创建迁移。两者都处理迁移，但 `alexander-zuev/supabase-mcp-server` 提供了更高级的功能，例如自动创建和详细的检索选项。在修改数据库时自动生成迁移文件的能力可以显著改善开发工作流程，确保所有模式更改都被追踪和版本控制。

### 3.3. 用户身份验证和管理 (Auth Admin SDK)

在提供的代码片段中，`supabase-community/supabase-mcp` 没有明确列出 Auth Admin SDK 的相关工具。而 `alexander-zuev/supabase-mcp-server` 则通过 Python SDK 直接访问 Supabase Auth Admin 方法，包括 `get_user_by_id`、`list_users`、`create_user` 和 `delete_user` 等工具。`alexander-zuev/supabase-mcp-server` 通过提供内置工具来管理用户，从而在用户身份验证和管理方面具有明显的优势。`alexander-zuev/supabase-mcp-server` 中包含 Auth Admin SDK 工具，允许开发者直接从其 AI 驱动的 IDE 管理用户，提供了超越基本 SQL 操作的功能，这对于用户创建、删除和管理身份验证因素等任务尤其有用。

### 3.4. 分支支持

`supabase-community/supabase-mcp` 包含用于创建、列出、删除、合并、重置和变基开发分支的实验性工具（需要付费计划）。在提供的代码片段中，`alexander-zuev/supabase-mcp-server` 没有明确提及分支支持。官方服务器提供了实验性的分支支持，这对于管理开发环境可能很有价值。官方 Supabase MCP 服务器中的实验性分支功能可能对那些在其开发工作流程中使用 Supabase 分支功能的团队有益。这允许隔离的开发环境和更容易地合并更改。

### 3.5. 日志和监控工具

`supabase-community/supabase-mcp` 提供了 `get_logs` 工具来检索各种 Supabase 服务的日志。`alexander-zuev/supabase-mcp-server` 则提供了 `retrieve_logs` 工具，该工具具有过滤和搜索来自各种 Supabase 服务的日志的功能。两者都提供日志检索功能，但 `alexander-zuev/supabase-mcp-server` 似乎提供了更高级的日志分析功能。`alexander-zuev/supabase-mcp-server` 中增强的日志检索功能（包括过滤和搜索）对于直接从 IDE 调试和监控 Supabase 应用程序的健康状况和性能至关重要。最近添加的日志轮换功能进一步增强了其长期监控的实用性。

### 3.6. 安全机制

`supabase-community/supabase-mcp` 提到该服务器处于 pre-1.0 状态，这意味着版本之间可能会有重大更改，但同时指出 LLM 将会自动适应可用工具。代码片段中没有详细说明除此以外的其他安全控制措施。`alexander-zuev/supabase-mcp-server` 则非常强调安全性，提供三级安全系统（安全、写入、破坏性）、运行时 SQL 验证、自动迁移版本控制以及对破坏性操作的确认。在内置安全性和安全特性方面，`alexander-zuev/supabase-mcp-server` 具有明显且显著的优势。对于在使用 AI 工具时优先考虑 Supabase 数据库和项目安全性的用户来说，`alexander-zuev/supabase-mcp-server` 强大的安全机制是一个主要的卖点。对破坏性操作要求显式确认可以防止意外的数据丢失或模式损坏。

### 3.7. 开发工具

`supabase-community/supabase-mcp` 包含 `generate_typescript_types` 工具，用于根据数据库模式生成 TypeScript 类型。在提供的代码片段中，`alexander-zuev/supabase-mcp-server` 没有明确提及类似的工具。官方服务器提供了一个用于生成 TypeScript 类型的特定开发工具，这对于基于 TypeScript 的项目可能很有益。官方 Supabase MCP 服务器中的 `generate_typescript_types` 工具可以通过提供基于其数据库模式自动生成的类型定义来改善 TypeScript 用户的使用体验，从而实现更好的代码补全和减少与类型相关的错误。

### 表 1：功能特性对比

## 技术架构和堆栈分析

### 4.1. 使用的编程语言和框架

`supabase-community/supabase-mcp` 使用 Node.js 和 TypeScript 构建（99.4% TypeScript，0.6% JavaScript。`alexander-zuev/supabase-mcp-server` 使用 `Python 3.12+` 构建。官方服务器使用流行的 JavaScript/TypeScript 生态系统，而另一个服务器则使用 Python 构建。用户对特定语言的熟悉程度和偏好可能会影响他们的选择。对于大量投资于 JavaScript/TypeScript 生态系统的开发者来说，官方服务器可能更具吸引力，因为它更容易与他们现有的项目集成，并且有可能为代码库做出贡献。相反，Python 开发者可能更喜欢 `alexander-zuev/supabase-mcp-server`。

### 4.2. 数据库交互方法和库

`supabase-community/supabase-mcp` 可能使用标准的 Node.js 库与 Supabase 的 API 和 PostgreSQL 进行交互。代码片段中没有详细说明具体的库。`alexander-zuev/supabase-mcp-server` 使用 `asyncpg` 进行与 PostgreSQL 的异步通信。Python 服务器使用 `asyncpg` 表明其专注于数据库交互的性能和效率。`asyncpg` 的异步特性可能使 `alexander-zuev/supabase-mcp-server` 在高负载或处理长时间运行的数据库操作时具有更好的性能和可扩展性。对于期望高使用率或复杂查询的用户来说，这可能是一个重要的因素。

### 4.3. 可扩展性考虑

作为 Supabase 社区的一部分，`supabase-community/supabase-mcp` 可能会受益于更广泛的社区贡献和与 Supabase 未来发展的协调。其模块化的架构和独立的 MCP 服务器（例如 `PostgREST`）表明了其可扩展性。`alexander-zuev/supabase-mcp-server` 的维护者已经提到计划推出具有坚实基础设施的付费版本，这表明其考虑了可扩展性 (28)。`Python` 和 `asyncpg` 的使用也有助于潜在的可扩展性。两者似乎都具有可扩展性，但官方服务器的社区支持可能在长期内提供更广泛的优势。官方 Supabase MCP 服务器作为一个社区项目，可能会看到更多不同的贡献和更快地采用新的 Supabase 功能。然而，`alexander-zuev/supabase-mcp-server` 维护者的积极开发和未来推出付费版本的计划表明了其对长期增长和可扩展性的承诺。

## 安装和配置过程

### 5.1 `supabase-community/supabase-mcp`

需要安装 `Node.js`。配置通常涉及从 Supabase 获取个人访问令牌（PAT）并使用包含运行服务器的 `npx` 命令的 JSON 结构配置 MCP 客户端（例如 `Cursor`）。提供了 Windows 用户的特定说明，包括使用 `cmd /c` 或 `wsl`。对于熟悉 `Node.js` 和 `npx` 的用户来说，安装似乎很简单。Windows 平台的特定说明很有帮助。依赖 PAT 是一个关键的安全考虑因素。

### 5.2 `alexander-zuev/supabase-mcp-server`

需 `Python 3.12+`，可选安装 `uv`。提供了多种安装方法：`pipx`（推荐）、`uv`、从源代码手动安装以及通过 `Smithery.ai` 安装。配置涉及设置 Supabase 项目详细信息、数据库密码和从 `thequery.dev` 获取的所需 API 密钥的环境变量。支持客户端特定的、全局的和项目特定的配置方法。提供了针对 `Cursor`、`Windsurf`、`Claude Desktop` 和 `Cline` 的详细配置说明，包括查找可执行文件的完整路径。提供了更灵活的安装方法。API 密钥的要求是一个额外的步骤。详细的客户端特定配置指南非常有帮助。对于某些用户来说，查找完整的可执行文件路径可能稍微复杂一些。

### 表 2：安装和配置对比

## 社区参与和维护状态

### 6.1 `supabase-community/supabase-mcp`

在 GitHub 上有 170 个星标和 18 个 fork。有 8 个未解决的 issue 和 2 个未合并的 pull request。共有 92 次提交和 4 个发布版本，最新的 `v0.2.0` 发布于 2025 年 4 月 3 日。列出了 3 位贡献者。显示出中等程度的社区兴趣和积极的维护，最近有发布版本并持续跟踪 issue。相对较少的贡献者可能表明其依赖于核心团队。

### 6.2 `alexander-zuev/supabase-mcp-server`

在 GitHub 上有 607 个星标和 70 个 fork。有 12 个未解决的 issue 和 0 个未合并的 pull request（在代码片段提供时）。提供了一个讨论区。列出了 2 位贡献者（`alexander-zuev` 和 `smithery-ai[bot]`）。共有 19 个发布版本，最新的 `v0.4` 发布于 2025 年 4 月 3 日。积极鼓励用户反馈并有一个路线图讨论。基于这些指标，`alexander-zuev/supabase-mcp-server` 表现出更强的社区参与度，拥有更多的星标和 fork。频繁的发布版本和积极的维护者表明其维护状态良好。讨论区和路线图表明其开发过程更加开放和具有沟通性。

## 用户反馈和情感分析

### 7.1 `supabase-community/supabase-mcp`

一些用户报告在 `Cursor` 中出现超时问题。在某些情况下，建议使用 `Supabase CLI` 而不是 MCP 服务器。普遍建议对 MCP 工具的不可预测性保持谨慎，并建议使用只读模式。一些用户在使用官方服务器时遇到了实际问题，这表明在某些环境中可能存在不稳定或限制。建议使用 CLI 作为替代方案表明 MCP 服务器可能并不总是首选解决方案。

### 7.2 `alexander-zuev/supabase-mcp-server`



用户已成功使用它通过单个提示创建数据库并插入数据。它因能够使 Cursor 理解数据库模式而受到赞扬，从而减少了幻觉。其执行任何类型的 SQL 查询并与管理 API 集成的能力也受到赞赏。有用户要求支持自托管的 Supabase 实例。维护者积极寻求反馈并专注于安全性和可靠性。用户发现它有助于为 Cursor 保留数据库上下文，从而提高了大型项目的效率。自动创建迁移和安全模式等功能受到好评。总体而言，用户反馈较为积极，强调了其高级功能、安全特性和在 IDE 集成方面的有效性。提到的主要限制是缺乏对自托管 Supabase 实例的支持（尽管已在路线图上）。维护者积极参与并解决反馈是一个积极的信号。

### 综合对比分析表

**表 3：综合对比**

## 7.3 用例场景和建议

+ 场景 1：广泛的 Supabase 平台管理： 如果主要需求是管理 Supabase 平台的各个方面，而不仅仅是数据库（例如，创建、暂停项目，管理组织），那么官方的 `supabase-community/supabase-mcp` 可能更适合，因为它提供了专门用于这些任务的工具。然而，用户应该意识到其 pre-1.0 的状态和一些报告的用户问题。

+ 场景 2：安全且自动化的数据库管理： 对于主要关注数据库交互（特别是 SQL 执行和模式管理），并高度重视安全性（例如自动迁移）的用户来说，`alexander-zuev/supabase-mcp-server` 似乎是更强的选择。其强大的安全特性和全面的 Auth Admin SDK 支持是显著的优势。

+ 场景 3：以 TypeScript 为中心的开发： 如果用户的项目主要使用 TypeScript，并且自动生成 TypeScript 类型是一个关键需求，那么官方服务器的 `generate_typescript_types` 工具可能是一个决定性因素。

+ 场景 4：社区支持和积极开发： 那些重视更活跃、更积极的社区以及响应迅速的维护者的用户可能更喜欢 `alexander-zuev/supabase-mcp-server`。其路线图和积极的讨论表明其持续改进并响应用户反馈。

建议： 根据以上分析，如果用户的主要关注点是安全高效的数据库管理，包括自动化的模式迁移和用户管理，那么 `alexander-zuev/supabase-mcp-server` 似乎是更具吸引力的选择。其强大的安全特性和积极的用户反馈表明它是一个更成熟和可靠的解决方案。然而，如果更广泛的 Supabase 平台管理或与 TypeScript 代码库的紧密集成是首要任务，则应考虑官方的 `supabase-community/supabase-mcp` 服务器，但需注意其 pre-1.0 的状态和报告的问题。

## 最后的结论

总而言之，`supabase-community/supabase-mcp` 提供了对 Supabase 平台的广泛访问，而 `alexander-zuev/supabase-mcp-server` 则专注于安全且功能丰富的数据库管理，特别是 SQL 执行和模式迁移。`alexander-zuev/supabase-mcp-server` 在安全性、数据库管理功能和社区活跃度方面似乎更胜一筹，尽管它引入了对外部 API 密钥的依赖。最终的选择将取决于用户的具体需求、技术背景以及对社区支持的偏好。随着 AI 驱动的开发工具的不断发展，MCP 服务器在提升开发者使用 Supabase 的效率方面将发挥越来越重要的作用。建议用户在做出最终决定时仔细权衡他们的具体需求和技术偏好。

## Works cited

1. [Supabase MCP Server - GitHub](https://github.com/supabase-community/supabase-mcp), accessed April 5, 2025, https://github.com/supabase-community/supabase-mcp

1. [alexander-zuev/supabase-mcp-server - GitHub](https://github.com/alexander-zuev/supabase-mcp-server), accessed April 5, 2025, https://github.com/alexander-zuev/supabase-mcp-server

1. [Supabase MCP Server](https://supabase.com/blog/mcp-server), accessed April 5, 2025, https://supabase.com/blog/mcp-server

1. [Everything You Need to Know about MCP Servers Explained - Sebastian Petrus - Medium](https://sebastian-petrus.medium.com/everything-you-need-to-know-about-mcp-servers-explained-b434d11e763e), accessed April 5, 2025, https://sebastian-petrus.medium.com/everything-you-need-to-know-about-mcp-servers-explained-b434d11e763e

1. [Connect Agents to Elasticsearch with Model Context Protocol, accessed April 5, 2025](https://www.elastic.co/search-labs/blog/model-context-protocol-elasticsearch), https://www.elastic.co/search-labs/blog/model-context-protocol-elasticsearch

1. [Hype-less opinion of MCP - Reddit](https://www.reddit.com/r/mcp/comments/1jofsdz/hypeless_opinion_of_mcp/), accessed April 5, 2025, https://www.reddit.com/r/mcp/comments/1jofsdz/hypeless_opinion_of_mcp/

1. [Model context protocol (MCP) | Supabase Docs](https://supabase.com/docs/guides/getting-started/mcp), accessed April 5, 2025, https://supabase.com/docs/guides/getting-started/mcp

1. [How to Connect Your Supabase Database via MCP Server to Cursor - Apidog](https://apidog.com/blog/supabase-mcp/), accessed April 5, 2025, https://apidog.com/blog/supabase-mcp/

1. [Introducing the official Supabase MCP Server - YouTube, accessed April 5, 2025](https://www.youtube.com/watch?v=1SMldLoOhbg), https://www.youtube.com/watch?v=1SMldLoOhbg

1. [How Cursor, Supabase, and MCP Transform Software Development | by Takafumi Endo](https://medium.com/@takafumi.endo/how-cursor-supabase-and-mcp-transform-software-development-357df8528cc7), accessed April 5, 2025, https://medium.com/@takafumi.endo/how-cursor-supabase-and-mcp-transform-software-development-357df8528cc7

1. [Supabase MCP tutorial: Connect Cursor AI to 10X your productivity. - YouTube](https://www.youtube.com/watch?v=b_hr1TB2GYQ), accessed April 5, 2025, https://www.youtube.com/watch?v=b_hr1TB2GYQ

1. [I built a Supabase MCP Server that lets Cursor create complete databases for me with a SINGLE prompt : r/ClaudeAI - Reddit](https://www.reddit.com/r/ClaudeAI/comments/1ixr3b8/i_built_a_supabase_mcp_server_that_lets_cursor/), accessed April 5, 2025, https://www.reddit.com/r/ClaudeAI/comments/1ixr3b8/i_built_a_supabase_mcp_server_that_lets_cursor/

1. [MCP in production? : r/Supabase - Reddit](https://www.reddit.com/r/Supabase/comments/1j88ibj/mcp_in_production/), accessed April 5, 2025, https://www.reddit.com/r/Supabase/comments/1j88ibj/mcp_in_production/

1. [Mind blown with MCP - Reddit](https://www.reddit.com/r/mcp/comments/1jn3ykp/mind_blown_with_mcp/), accessed April 5, 2025, https://www.reddit.com/r/mcp/comments/1jn3ykp/mind_blown_with_mcp/

1. MCP Servers for Cursor - Cursor Directory, accessed April 5, 2025, https://cursor.directory/mcp

1. [Information about Supabase | Glama](https://glama.ai/mcp/servers/search/information-about-supabase-3), accessed April 5, 2025, https://glama.ai/mcp/servers/search/information-about-supabase-3

1. [What is Supabase: A Review of Serverless Database Features - Bejamas](https://bejamas.com/hub/serverless-database/supabase), accessed April 5, 2025, https://bejamas.com/hub/serverless-database/supabase

1. [supabase-mcp-server - PyPI](https://pypi.org/project/supabase-mcp-server/0.3.12/), accessed April 5, 2025, https://pypi.org/project/supabase-mcp-server/0.3.12/

1. [Supabase MCP server that automatically creates migration files when you modify your db and requires 2-step approval to prevent Cursor from nuking it - Reddit](https://www.reddit.com/r/cursor/comments/1j78itg/supabase_mcp_server_that_automatically_creates/), accessed April 5, 2025, https://www.reddit.com/r/cursor/comments/1j78itg/supabase_mcp_server_that_automatically_creates/

1. [Supabase MCP server with 1 click install & run (v0.2.0 release) : r/cursor - Reddit](https://www.reddit.com/r/cursor/comments/1isxeqe/supabase_mcp_server_with_1_click_install_run_v020/), accessed April 5, 2025, https://www.reddit.com/r/cursor/comments/1isxeqe/supabase_mcp_server_with_1_click_install_run_v020/

1. [Supabase MCP Server - Smithery](https://smithery.ai/server/@alexander-zuev/supabase-mcp), accessed April 5, 2025, https://smithery.ai/server/@alexander-zuev/supabase-mcp

1. [supabase-mcp-server - PyPI](https://pypi.org/project/supabase-mcp-server/0.3.8/), accessed April 5, 2025, https://pypi.org/project/supabase-mcp-server/0.3.8/

1. [MCP Server for Supabase Query - Smithery](https://smithery.ai/server/@alexander-zuev/supabase-mcp-server), accessed April 5, 2025, https://smithery.ai/server/@alexander-zuev/supabase-mcp-server

1. [`execute_postgresql` - Supabase MCP Server | Glama](https://glama.ai/mcp/servers/@alexander-zuev/supabase-mcp-server/tools/execute_postgresql), accessed April 5, 2025, https://glama.ai/mcp/servers/@alexander-zuev/supabase-mcp-server/tools/execute_postgresql

1. [`retrieve_migrations` - Supabase MCP Server - Glama](https://glama.ai/mcp/servers/@alexander-zuev/supabase-mcp-server/tools/retrieve_migrations), accessed April 5, 2025, https://glama.ai/mcp/servers/@alexander-zuev/supabase-mcp-server/tools/retrieve_migrations

1. [Releases · `alexander-zuev/supabase-mcp-server` - GitHub](https://github.com/alexander-zuev/supabase-mcp-server/releases), accessed April 5, 2025, https://github.com/alexander-zuev/supabase-mcp-server/releases

1. [`confirm_destructive_operation` - Supabase MCP Server | Glama](https://glama.ai/mcp/servers/@alexander-zuev/supabase-mcp-server/tools/confirm_destructive_operation), accessed April 5, 2025, https://glama.ai/mcp/servers/@alexander-zuev/supabase-mcp-server/tools/confirm_destructive_operation

1. [v0.4 Roadmap · `alexander-zuev/supabase-mcp-server` · Discussion #46 - GitHub](https://github.com/alexander-zuev/supabase-mcp-server/discussions/46), accessed April 5, 2025, https://github.com/alexander-zuev/supabase-mcp-server/discussions/46



# 参考文献

1. MCP server for Supabase and Sqlite (read and write) - YouTube, accessed April 5, 2025, `https://www.youtube.com/watch?v=zDEdKLHCyGw`
2. Supabase MCP Server - Glama, accessed April 5, 2025, `https://glama.ai/mcp/servers/@alexander-zuev/supabase-mcp-server`
3. Discussions - alexander-zuev supabase-mcp-server - GitHub, accessed April 5, 2025, `https://github.com/alexander-zuev/supabase-mcp-server/discussions`
4. MCP supabase issues - Discussion - Cursor - Community Forum, accessed April 5, 2025, `https://forum.cursor.com/t/mcp-supabase-issues/63873`
5. Supabase MCP 0.45.14 - Discussion - Cursor - Community Forum, accessed April 5, 2025, `https://forum.cursor.com/t/supabase-mcp-0-45-14/64266`
6. Let Cursor & Windsurf interact with your Supabase autonomously - Reddit, accessed April 5, 2025, `https://www.reddit.com/r/Supabase/comments/1iqmcb2/let_cursor_windsurf_interact_with_your_supabase/`
