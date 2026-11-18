
# OpenClaw 记忆系统指南

## 工作区与记忆配置

```
{
  "agents": {
    "list": [
      {
        "id": "coding",
        "workspace": "~/.openclaw/workspace-coding"
      },
      {
        "id": "social",
        "workspace": "~/.openclaw/workspace-social"
      }
    ]
  }
}
```

每个 Agent 的 `workspace` 下有独立的 `MEMORY.md` 和 `memory/` 目录。如果某个 Agent 完全不需要记忆功能，可以在它的 `workspace` 里不放任何记忆文件即可。

## 对话上下文管理

### 什么是上下文窗口？

上下文窗口就是模型一次能"看到"的所有文本。包括：

```
┌─────────────────────────────────────────┐
│ 系统提示词 (System Prompt)               │  ← 固定占用
│ - Agent 人格 (SOUL.md)                  │
│ - 技能指令                               │
│ - 工具定义                               │
├─────────────────────────────────────────┤
│ 记忆注入                                 │  ← 启动时加载
│ - MEMORY.md                             │
│ - USER.md                               │
│ - 最近几天的日志                          │
├─────────────────────────────────────────┤
│ 对话历史                                 │  ← 动态增长
│ - 用户消息                               │
│ - AI 回复                               │
│ - 工具调用结果                            │
├─────────────────────────────────────────┤
│ 剩余空间                                 │  ← 给 AI 生成回复用
└─────────────────────────────────────────┘
```

常见模型的上下文窗口：Claude Sonnet 4.6 200K tokens、GPT-5.2 128K tokens、Gemini 3.1 Pro 1M tokens。

### 滑动窗口策略

当对话越来越长，上下文窗口快满了怎么办？`OpenClaw` 使用滑动窗口策略：

```
正常对话：  [系统提示 | 记忆 | 消息1 | 消息2 | ... | 消息N]
接近上限：  [系统提示 | 记忆 | [摘要:消息1-50] | 消息51 | ... | 消息N]
继续对话：  [系统提示 | 记忆 | [摘要:消息1-50] | 消息51 | ... | 消息N+M]
```

`compaction` 的过程：检测到上下文使用率超过阈值 → 触发 `memoryFlush`，AI 保存重要信息到记忆文件 → 将早期消息压缩成摘要 → 释放上下文空间，继续对话。

### 上下文预算管理

`OpenClaw` 通过 `compaction` 机制自动管理上下文空间。当对话接近上下文窗口上限时，会自动触发压缩。你可以通过 `reserveTokensFloor` 控制压缩后保留多少上下文空间：

```
{
  "agents": {
    "defaults": {
      "compaction": {
        "reserveTokensFloor": 20000
      }
    }
  }
}
```

`reserveTokensFloor` 越大，压缩越激进（保留的旧消息越少）。默认值 `20000` tokens 对大多数场景够用。

> 💡 **实用建议：** 如果记忆文件太大导致上下文空间不足，最有效的做法是精简 `MEMORY.md` 的内容（建议控制在 3000 tokens 以内），而不是调整配置参数。

## 用户画像和偏好记忆

`OpenClaw` 有一个专门的文件 `USER.md` 来存储用户画像。这跟 `MEMORY.md` 不同 — `MEMORY.md` 存的是各种杂项记忆，`USER.md` 专门描述"你是谁"。

### USER.md 结构

```
<!-- ~/.openclaw/workspace/USER.md -->
# 用户画像

## 基本信息
- 姓名：老金 | 时区：Asia/Shanghai | 语言：中文为主

## 技术偏好
- 主力语言：TypeScript（不用分号，单引号，2 空格缩进）
- 前端：React + Next.js | 后端：Node.js + Fastify
- 数据库：PostgreSQL | 部署：Docker + Railway

## 沟通偏好
- 喜欢简洁直接的回答，代码示例比文字解释更有用
- 遇到问题先给方案，再解释原因

## 日程习惯
- 工作时间：9:00-18:00
- 每周一 10:00 团队周会 | 每周五 15:00 代码评审
```

### 自动学习用户偏好

`OpenClaw` 没有专门的"自动学习"配置开关。用户偏好的记录依赖于 AI 模型自身的判断 — 当你在对话中表达偏好时，AI 会主动将其写入 `USER.md` 或 `MEMORY.md`。

你可以通过 `SOUL.md`（Agent 人格文件）来引导 AI 更主动地记录偏好：

```
<!-- ~/.openclaw/workspace/SOUL.md 中添加 -->
## 记忆习惯
- 当用户表达偏好或习惯时，主动写入 `USER.md`
- 当用户做出重要决策时，记录到 `MEMORY.md`
- 每次对话结束前，检查是否有值得记录的信息
```

这种方式比配置文件更灵活 — 你可以用自然语言精确描述 AI 应该记住什么、怎么记住。

### 主动让 AI 记住

直接告诉 AI 你想让它记住什么：

```
记住：我的 GitHub 用户名是 laojin-dev
记住：项目部署在 Hetzner 的 VPS 上
记住：我不喜欢在代码里用分号
记住：跟我说话不用太客气，直接说重点
```

AI 会根据内容类型，自动决定写入 `MEMORY.md` 还是 `USER.md`：

+   个人偏好、习惯 → `USER.md`

+   项目信息、决策、事实 → `MEMORY.md`

+   临时笔记、当天事项 → `memory/YYYY-MM-DD.md`

## 记忆工具

### memory_search — 语义搜索

从所有记忆文件中搜索相关内容。

```
用户：我之前说过用什么支付方案来着？
AI：（调用 memory_search("支付方案")）
    → 找到 MEMORY.md 中的记录：2026-02-22 决定用 Stripe
AI：你在 2 月 22 号决定用 Stripe 作为支付方案。
```

搜索范围：

+   `MEMORY.md` — 长期记忆

+   `USER.md` — 用户画像

+   `memory/*.md` — 所有日志文件

### memory_get — 精确读取

读取某个记忆文件的特定内容。

```
用户：看看我昨天的笔记
AI：（调用 memory_get("memory/2026-02-24.md")）
    → 返回昨天日志的完整内容
```

### 记忆写入

AI 通过标准的文件写入工具来更新记忆文件。没有专门的"记忆写入"工具 — 这是故意的设计，保持简单。

```
用户：记住，我们决定用 Stripe
AI：（调用 file_write，写入 MEMORY.md）
    好的，已记录到长期记忆中。
```

## 知识库集成 (RAG)

当你需要 AI 记住大量结构化知识（比如公司文档、产品手册、代码库），普通的记忆文件就不够了。这时候需要 RAG（Retrieval-Augmented Generation，检索增强生成）。

### 什么是 RAG？

简单说：AI 回答问题之前，先从知识库里搜索相关内容，然后带着这些内容去回答。

```
传统方式：
用户提问 → AI 凭记忆回答（可能瞎编）

RAG 方式：
用户提问 → 搜索知识库 → 找到相关文档 → AI 基于文档回答（有据可查）
```

### OpenClaw 的 RAG 实现

`OpenClaw` 的记忆搜索本身就具备 RAG 能力 — 通过 `memorySearch` 索引 `workspace` 下的 Markdown 文件，支持语义搜索。如果你想索引 `workspace` 之外的文件，可以通过 `extraPaths` 扩展：

```
{
  "agents": {
    "defaults": {
      "memorySearch": {
        "extraPaths": [
          "~/Documents/company-wiki",
          "~/projects/myapp/README.md"
        ]
      }
    }
  }
}
```

对于更高级的知识检索需求，`OpenClaw` 支持 `QMD` 后端（实验性功能），它结合了 `BM25` + 向量搜索 + 重排序：

```
{
  "memory": {
    "backend": "qmd",
    "qmd": {
      "includeDefaultMemory": true,
      "paths": [
        { "name": "docs", "path": "~/notes", "pattern": "**/*.md" }
      ],
      "update": { "interval": "5m" },
      "limits": { "maxResults": 6, "timeoutMs": 4000 }
    }
  }
}
```

> ⚠️ `QMD` 需要单独安装（`bun install -g https://github.com/tobi/qmd`），且需要支持扩展的 `SQLite` 构建。如果 `QMD` 不可用，`OpenClaw` 会自动回退到内置的 `SQLite` 搜索引擎。

### 知识源类型

`OpenClaw` 的 `memorySearch` 默认只索引 Markdown 文件（`.md`）。如果使用 `QMD` 后端，可以通过 `paths` 配置索引额外目录：

| 配置方式 | 说明 | 适用场景 |
| --- | --- | --- |
| 默认 workspace | 自动索引 `MEMORY.md` + `memory/*.md` | 日常使用 |
| `extraPaths` | 索引 `workspace` 外的 Markdown 文件 | 需要引用外部文档 |
| `QMD paths` | 索引任意目录，支持 glob 模式 | 大规模知识库 |

### 向量搜索引擎

`OpenClaw` 内置两种搜索引擎：

| 引擎 | 特点 | 配置方式 |
| --- | --- | --- |
| 内置 `SQLite`（默认） | 零配置，`sqlite-vec` 加速，支持混合搜索 | 开箱即用 |
| `QMD`（实验性） | `BM25` + 向量 + 重排序，功能更强 | `memory.backend = "qmd"` |

### 知识库与记忆管理命令

记忆索引和知识库都通过 `openclaw memory` 子命令管理：

```
# 记忆索引管理
openclaw memory status             # 查看记忆索引状态
openclaw memory index              # 重建记忆索引
openclaw memory search "部署"       # 搜索记忆（支持语义搜索）
```

### 记忆 vs 知识库

| 维度 | 记忆系统 | 知识库 (RAG) |
| --- | --- | --- |
| 内容来源 | 对话中产生 | 预先准备的文档 |
| 更新方式 | AI 自动写入 | 手动索引或监控 |
| 搜索方式 | 混合搜索（关键词 + 语义） | 语义向量搜索 |
| 适合内容 | 偏好、决策、笔记 | 文档、手册、代码 |
| 数据量 | 小（几十个文件） | 大（成百上千个文件） |

## 记忆的增删改查操作

### 通过 CLI 操作

```
# 查看记忆索引状态
openclaw memory status

# 重建记忆索引（当记忆文件有变动时）
openclaw memory index

# 搜索记忆（支持语义搜索）
openclaw memory search "React 组件"
```

记忆文件本身是 Markdown，可以直接用编辑器查看和修改：

```
code ~/.openclaw/workspace/MEMORY.md                    # VS Code
vim ~/.openclaw/workspace/memory/2026-02-25.md          # Vim
```

修改后运行 `openclaw memory index` 重建索引，下次会话启动时 AI 就能看到更新的内容。

### 通过对话操作

直接跟 AI 说就行：

```
你：记住，我们的 API 基础 URL 是 https://api.example.com/v2    → 写入
你：我之前说过 API 地址是什么？                                  → 查询
你：API 地址改了，新地址是 https://api.example.com/v3            → 更新
你：把关于旧项目的记忆都删掉                                     → 删除
你：帮我整理一下长期记忆，删掉过时的信息                          → 整理
```

AI 会自动调用对应的记忆工具完成操作。

## 记忆隐私和安全

记忆文件里可能包含敏感信息。`OpenClaw` 提供了多层保护机制。

### 敏感信息过滤

`OpenClaw` 没有内置的记忆内容自动过滤配置。防止敏感信息写入记忆文件的最佳做法是通过 `SOUL.md` 明确告诉 AI：

```
<!-- ~/.openclaw/workspace/SOUL.md 中添加 -->
## 安全规则
- 永远不要将密码、`API Key`、`Token`、`身份证号`、`银行卡号`写入记忆文件
- 如果用户要求记住敏感信息，提醒用户使用环境变量或密钥管理器
- 记忆文件中的敏感信息用 `[REDACTED]` 替代
```

这种方式利用 AI 模型自身的判断力来过滤敏感内容，比正则匹配更智能（能识别上下文中的敏感语义，而不仅仅是模式匹配）。

### 记忆文件安全

`OpenClaw` 的记忆文件是纯文本 Markdown，存储在本地磁盘上，没有内置加密功能。如果你需要保护记忆文件：

+   **磁盘加密**：使用操作系统级别的磁盘加密（`macOS FileVault`、`Linux LUKS`、`Windows BitLocker`）

+   **文件权限**：确保 `~/.openclaw/workspace/` 目录权限为 `700`（仅所有者可读写）

+   **Git 忽略**：如果 `workspace` 在 Git 仓库中，将记忆文件加入 `.gitignore`

```
# 设置 workspace 目录权限（macOS/Linux）
chmod 700 ~/.openclaw/workspace
```

### 不应该存入记忆的内容

无论有没有加密，密码、`API Key`、`JWT Token`、`身份证号`、`银行卡号`等敏感信息都不应该写入记忆文件。用环境变量或密钥管理器代替。

### 记忆操作追踪

`OpenClaw` 没有内置的记忆审计日志配置。但你可以通过以下方式追踪记忆变更：

+   **Git 版本控制**：将 `workspace` 初始化为 Git 仓库，每次记忆变更都有完整历史

+   **会话日志**：`OpenClaw` 的会话日志（`sessions/*.jsonl`）记录了所有工具调用，包括文件写入操作

```
# 用 Git 追踪记忆变更
cd ~/.openclaw/workspace
git init
git add MEMORY.md USER.md memory/
git commit -m "memory snapshot"
```
