
# 第四章：Hermes Agent——Persistent Memory

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/KqgsdWcCqoGLCQxuLOSclH7Cn3f](https://qqs7y1hozd1.feishu.cn/docx/KqgsdWcCqoGLCQxuLOSclH7Cn3f)

Hermes Agent 拥有有界的、经过整理的记忆，这些记忆会在不同会话中持续存在。这使得它能够记住你的偏好、你的项目、你的环境以及它所学过的东西。

## 工作原理

代理的记忆由两个文件组成：

`~/.hermes/memories/memory`

两者都存储在系统提示中，并在会话开始时作为冻结快照注入系统提示。代理通过工具管理自己的内存——它可以添加、替换或删除条目。

信息

字符限制有助于集中内存。当内存满时，代理会合并或替换条目，以便为新信息腾出空间。

## 记忆如何在系统提示中呈现

在每个会话开始时，内存条目会从磁盘加载，并作为一个固定块渲染到系统提示中：

```
══════════════════════════════════════════════════
MEMORY (your personal notes) [67% — 1,474/2,200 chars]
══════════════════════════════════════════════════
User's project is a Rust web service at `~/code/myapi` using Axum + SQLx
§
This machine runs Ubuntu 22.04, has Docker and Podman installed
§
User prefers concise responses, dislikes verbose explanations
```

格式包括：

- 显示哪个存储（内存或用户配置文件）的标题
- 使用百分比和字符计数，以便座席了解容量
- 由（章节符号）分隔符分隔的单个条目 §
- 条目可以是多行的

**冻结快照模式：** 系统提示注入在会话开始时被捕获一次，且在会话期间不会改变。这是有意为之的——它保留了大语言模型（LLM）的前缀缓存以提高性能。当代理在会话期间添加/删除记忆条目时，更改会立即持久化到磁盘，但直到下一个会话开始才会出现在系统提示中。工具响应始终显示实时状态。

## 内存工具操作

代理使用这些操作来使用工具：内存

- **添加** — 添加新的记忆条目
- **替换** — 用更新后的内容替换现有条目（通过 `old_text` 进行子字符串匹配）
- **移除** — 移除不再相关的条目（通过 `old_text` 使用子字符串匹配）

无需操作 — 会话开始时，记忆内容会自动注入系统提示中。智能体将其记忆视为对话上下文的一部分。读取

### 子串匹配

这些操作使用简短的唯一子字符串匹配——你不需要完整的条目文本。参数只需是一个能准确识别一个条目的唯一子字符串即可：替换移除旧文本

```
memory(action="replace", target="memory",
       old_text="dark mode",
       content="User prefers light mode in VS Code, dark mode in terminal")
```

如果子字符串匹配多个条目，则会返回错误，要求进行更具体的匹配。

## 两个目标解析

### 记忆 — 智能体的个人笔记

关于代理需要记住的有关环境、工作流程和经验教训的信息：

- 环境信息（OS、工具、项目结构）
- 项目约定和配置
- 发现的工具怪癖和变通方法
- 已完成的任务日记条目
- 行之有效的技能和技巧

### 用户 — 用户资料

有关用户身份、偏好和沟通风格的信息：

- 姓名、角色、时区
- 沟通偏好（简洁与详细，格式偏好）
- 令人恼火的事和需要避免的事
- 工作流习惯
- 技术技能水平

## 该保存还是跳过

### 主动保存这些

智能体自动保存——你无需手动操作。它在学习时就会保存：

- **用户偏好：** "比起 JavaScript，我更喜欢 TypeScript" → 保存到 `用户`
- **环境信息：** “此服务器运行 Debian 12 和 PostgreSQL 16” → 保存到 `内存`
- **修正内容：** “不要用于 Docker 命令，用户已在 Docker 组中” → 保存到 `sudo` 内存
- **约定：** “项目使用制表符，行宽 120 字符，采用 Google 风格的文档字符串” → 保存到 `内存`
- **已完成工作：** "2026 年 1 月 15 日将数据库从 MySQL 迁移到 PostgreSQL" → 保存到 `内存`
- **明确请求：** “记住我的 API 密钥每月轮换一次” → 保存到 `内存`

### 跳过这些

- **琐碎/显而易见的信息：** "用户询问了 Python" — 过于模糊，没有用处
- **容易重新发现的事实：** "Python 3.12 支持 f-string 嵌套" —— 可以通过网络搜索到
- **原始数据转储：** 大代码块、日志文件、数据表 — 太大而无法放入内存
- **会话特定的临时数据：** 临时文件路径、一次性调试上下文
- **上下文文件中已有的信息：** `SOUL.md` 和 `AGENTS.md` 的内容

## 容量管理

内存有严格的字符限制，以确保系统提示在可控范围内：

### 内存满了会发生什么

当您尝试添加一个会超出限制的条目时，该工具会返回一个错误：

```
{
  "success": false,
  "error": "Memory at 2,100/2,200 chars. Adding this entry (250 chars) would exceed the limit. Replace or remove existing entries first.",
  "current_entries": ["..."],
  "usage": "2,100/2,200"
}
```

代理随后应执行以下操作：

1. 读取当前条目（显示在错误响应中）
2. 识别可以移除或合并的条目
3. 用于将相关条目合并为较短的版本替换
4. 然后新的条目添加

**最佳实践：** 当内存使用率超过 80%（可在系统提示标题中查看）时，在添加新条目之前先合并现有条目。例如，将三个单独的 "项目使用 X" 条目合并为一个全面的项目描述条目。

### 良好记忆条目实例

**简洁、信息密集的条目效果最佳：**

```
# Good: Packs multiple related facts
User runs macOS 14 Sonoma, uses Homebrew, has Docker Desktop and Podman. Shell: zsh with oh-my-zsh. Editor: VS Code with Vim keybindings.

# Good: Specific, actionable convention
Project `~/code/api` uses Go 1.22, sqlc for DB queries, chi router. Run tests with 'make test'. CI via GitHub Actions.

# Good: Lesson learned with context
The staging server (10.0.1.50) needs SSH port 2222, not 22. Key is at `~/.ssh/staging_ed25519`.

# Bad: Too vague
User has a project.

# Bad: Too verbose
On January 5th, 2026, the user asked me to look at their project which is
located at `~/code/api`. I discovered it uses Go version 1.22 and...
```

## 重复预防

内存系统会自动拒绝完全重复的条目。如果您尝试添加已存在的内容，系统将返回成功，并附带一条 "未添加重复项" 的消息。

## 安全扫描

内存条目在被接受之前会被扫描是否存在注入和数据外泄模式，因为它们会被注入到系统提示中。与威胁模式（提示注入、凭证外泄、SSH 后门）匹配或包含不可见 Unicode 字符的内容将被阻止。

## 会话搜索

除了 `MEMORY.md` 和 `USER.md` 之外，代理还可以使用以下工具搜索其过往对话：`session_search`

- 所有 CLI 和消息会话都存储在支持 FTS5 全文搜索的 SQLite 中 `~/.hermes/state.db`
- 搜索查询会返回相关的过往对话，并带有 Gemini Flash 摘要
- 即使几周前讨论的内容不在其活跃记忆中，智能体也能找到它们

```
hermes sessions list    # Browse past sessions
```

### 会话搜索与内存

**记忆** 用于存储 **始终应处于上下文之中的关键事实**。会话搜索用于处理 "我们上周是否讨论过 X？" 这类查询，在这类查询中，智能体需要回忆过去对话中的具体内容。

## 配置

```
# In `~/.hermes/config.yaml`
memory:
  memory_enabled: true
  user_profile_enabled: true
  memory_char_limit: 2200   # ~800 tokens
  user_char_limit: 1375     # ~500 tokens
```

## 外部内存提供程序

对于超越 `MEMORY.md` 和 `USER.md` 的更深入、持久的内存，Hermes 附带 8 个外部内存提供程序插件，包括 Honcho、OpenViking、Mem0、Hindsight、Holographic、RetainDB、ByteRover 和 Supermemory。

外部供应商运行 **与** 内置内存并行（从不替代它），并增加了诸如知识图谱、语义搜索、自动事实提取和跨会话用户建模等功能。

```
hermes memory setup      # pick a provider and configure it
hermes memory status     # check what's active
```

请参阅 [内存提供程序](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory-providers) 指南，以了解每个提供程序的完整详细信息、设置说明和比较内容。
