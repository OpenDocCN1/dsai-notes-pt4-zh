
# 第三章：Hermse Agent 上下文文件

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/KrrGdM7qRoMcQJxYzuIcu1kunyc](https://qqs7y1hozd1.feishu.cn/docx/KrrGdM7qRoMcQJxYzuIcu1kunyc)

Hermes Agent 会自动发现并加载决定其行为方式的上下文文件。其中一些是项目本地的，从你的工作目录中发现。现在对 Hermes 实例是全局的，并且仅从加载。`SOUL.md` `HERMES_HOME`

## 支持的上下文文件

### 优先级系统

每个会话仅加载一种项目上下文类型（首次匹配优先）：→ → → 。`SOUL.md`始终作为代理身份独立加载（插槽 #1）。`.hermes.md`、`AGENTS.md`、`CLAUDE.md`、`.cursorrules`

## AGENTS.md

`AGENTS.md`是主要的项目上下文文件。它告诉代理你的项目是如何组织的，要遵循哪些约定，以及任何特殊说明。

### 渐进式子目录发现

会话开始时，Hermes 会将工作目录中的文件加载到系统提示中。在会话期间，当代理导航到子目录（通过 `read_file`、`terminal`、`search_files` 等）时，它会逐步发现这些目录中的上下文文件，并在它们变得相关时将其注入到对话中。`AGENTS.md`

```
my-project/
├── AGENTS.md              ← Loaded at startup (system prompt)
├── frontend/
│   └── AGENTS.md          ← Discovered when agent reads frontend/ files
├── backend/
│   └── AGENTS.md          ← Discovered when agent reads backend/ files
└── shared/
    └── AGENTS.md          ← Discovered when agent reads shared/ files
```

与在启动时加载所有内容相比，这种方法有两个优点：

+ 无系统提示冗余 — 子目录提示仅在需要时出现
+ 提示缓存保留 —— 系统提示在各轮对话中保持稳定

每个子目录在每个会话中最多检查一次。发现过程还会向上遍历父目录，因此即使没有自己的上下文文件，读取也会发现。`backend/src/main.py` `backend/AGENTS.md` `backend/src/`

> 信息
> [子目录上下文文件](https://hermes-agent.nousresearch.com/docs/user-guide/features/context-files#security-prompt-injection-protection)与启动上下文文件一样，都要经过安全扫描。恶意文件会被拦截。

### 示例AGENTS.md

```
# Project Context

This is a Next.js 14 web application with a Python FastAPI backend.

## Architecture
- Frontend: Next.js 14 with App Router in `/frontend`
- Backend: FastAPI in `/backend`, uses SQLAlchemy ORM
- Database: PostgreSQL 16
- Deployment: Docker Compose on a Hetzner VPS

## Conventions
- Use TypeScript strict mode for all frontend code
- Python code follows PEP 8, use type hints everywhere
- All API endpoints return JSON with `{data, error, meta}` shape
- Tests go in `__tests__/` directories (frontend) or `tests/` (backend)

## Important Notes
- Never modify migration files directly — use Alembic commands
- The `.env.local` file has real API keys, don&#x27;t commit it
- Frontend port is 3000, backend is 8000, DB is 5432
```

## SOUL.md

`SOUL.md`[控制](https://hermes-agent.nousresearch.com/docs/user-guide/features/personality)代理的个性、语气和沟通风格。完整详情请参阅个性页面。

### 地点

+ `~/.hermes/SOUL.md`
+ 或者如果您使用自定义主目录运行 Hermes `$HERMES_HOME/SOUL.md`

### 重要细节

+ 如果默认的Hermes种子不存在，系统会自动生成一个 `yetSOUL.md`
+ Hermes仅从`SOUL.md` `HERMES_HOME`加载
+ Hermes 不会在工作目录中探查 `SOUL.md`
+ 如果文件是空的，则不会将`from`添加到`prompt` `SOUL.md`
+ 如果文件有内容，则在扫描和截断后逐字注入内容

## .cursorrules

Hermes与Cursor IDE的文件和规则模块兼容。如果这些文件存在于项目根目录中，且未找到优先级更高的上下文文件（ `.hermes.md`、`AGENTS.md` 或 `CLAUDE.md` ），则会将它们作为项目上下文加载。`.cursorrules` `.cursor/rules/*.mdc` `.hermes.md` `AGENTS.md` `CLAUDE.md`

这意味着在使用 Hermes 时，您现有的光标约定会自动适用。

## 上下文文件的加载方式

### 启动时（系统提示）

上下文文件通过以下方式加载：`build_context_files_prompt()` `agent/prompt_builder.py`

1. 扫描工作目录 — 检查是否存在 → → → （首次匹配成功）`.hermes.md`、`AGENTS.md`、`CLAUDE.md`、`.cursorrules`
1. 内容已读取 — 每个文件均以 UTF-8 文本形式读取
1. 安全扫描 — 检查内容是否存在提示注入模式
1. 截断 — 超过20,000字符的文件将进行首尾截断（头部70%，尾部20%，中间有标记）
1. 汇编 — 所有部分都组合在一个标题下`# 项目背景`
1. 注入 — 已组装的内容被添加到系统提示中

### 在会议期间（渐进式发现）

子目录提示跟踪器在文件路径的监视工具调用参数中：`agent/subdirectory_hints.py`

1. 路径提取 — 在每次工具调用后，从参数（文件路径、工作目录、shell 命令等）中提取文件路径和工作目录
1. 祖先遍历 — 检查当前目录及其最多5个父目录（在已访问的目录处停止）
1. 提示加载 — 如果找到 `AGENTS.md`、`CLAUDE.md` 或 `.cursorrules`，则加载它（每个目录的第一个匹配项）
1. 安全扫描 — 与启动文件相同的提示注入扫描
1. 截断 — 每个文件上限为8000字符
1. 注入 — 附加到工具结果中，以便模型自然地在上下文中看到它

最终提示部分大致如下：

```
# Project Context

The following project context files have been loaded and should be followed:

## AGENTS.md

[Your AGENTS.md content here]

## .cursorrules

[Your .cursorrules content here]

[Your SOUL.md content here]
```

请注意，SOUL内容是直接插入的，没有额外的包装文本。

## 安全：提示注入防护

所有上下文文件在被包含之前都会被扫描是否存在潜在的提示注入。扫描器会检查以下内容：

+ 指令覆盖尝试：“忽略先前指令”，“无视你的规则”
+ 欺骗模式：“不告诉用户”
+ 系统提示覆盖: "系统提示覆盖"
+ 隐藏的HTML注释：`<!--忽略指令-->`
+ 隐藏的 div 元素：`<div style="display:none">`
+ 凭证窃取：`curl` ... `$API_KEY`
+ 秘密文件访问：`cat .env`、`cat credentials`
+ 不可见字符：零宽空格、双向覆盖符、连字符

如果检测到任何威胁模式，则该文件将被阻止：

```
[BLOCKED: AGENTS.md contained potential prompt injection (prompt_injection). Content not loaded.]
```

警告

此扫描器可防范常见的注入模式，但它不能替代对共享存储库中上下文文件的审查。始终要验证非你所编写项目中的 `AGENTS.md` 内容。

## 尺寸限制

当文件超过20,000字符时，截断消息显示为：

```
[...truncated AGENTS.md: kept 14000+4000 of 25000 chars. Use file tools to read the full file.]
```

## 有效上下文文件的提示

`AGENTS.md` 最佳实践

1. 保持简洁 —— 控制在20000字符以内；代理每轮都会读取
1. 带标题的结构 — 使用章节来阐述架构、约定、重要说明
1. 包含具体示例 —— 展示首选的代码模式、API 形态、命名约定
1. 提及不应该做的事情 — “切勿直接修改迁移文件”
1. 列出关键路径和端口 — 代理使用这些来执行终端命令
1. 随着项目的发展进行更新 —— 过时的上下文比没有上下文更糟糕

### 每个子目录上下文

对于单体仓库，请将特定子目录的说明放在嵌套的 `AGENTS.md` 文件中：

```
&lt;!-- frontend/AGENTS.md --&gt;
# Frontend Context

- Use `pnpm` not `npm` for package management
- Components go in `src/components/`, pages in `src/app/`
- Use Tailwind CSS, never inline styles
- Run tests with `pnpm test`
```

```
&lt;!-- backend/AGENTS.md --&gt;
# Backend Context

- Use `poetry` for dependency management
- Run the dev server with `poetry run uvicorn main:app --reload`
- All endpoints need OpenAPI docstrings
- Database models are in `models/`, schemas in `schemas/`
```
