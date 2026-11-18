
| 其他 | `/help` | 显示帮助 | 快速查命令 | ⭐ |

|  | `/release-notes` | 更新日志 | 查看新功能 |  |

|  | `/loop` | 定时循环执行 | 监控部署状态 | ⭐ |

|  | `/sandbox` | 沙箱隔离模式 | 安全执行 |  |

|  | `/color` | 会话颜色设置 | 个性化 |  |

|  | `/copy` | 复制AI回复 | 分享内容 |  |

> 💡 提示：输入 `/` 然后按 Tab 键可以查看所有可用命令。

## v2.1.69+ 新增命令详解 🆕

以下命令是 Claude Code v2.1.69-v2.1.78 版本新增的功能：

### `/loop` - 定时循环执行

**一句话理解**：就像设了一个"定时闹钟"——每隔一段时间自动执行你指定的任务。

```
/loop 5m check deployment status  # 每5分钟检查部署状态
/loop 30s run tests               # 每30秒运行测试
/loop 1h check for updates        # 每1小时检查更新
```

**语法**：`/loop <时间间隔> <提示词>`

**时间格式**：数字 + 单位（s秒 / m分钟 / h小时）

**注意事项**：

- 只在REPL空闲时触发（不会打断你当前的工作）

- 循环任务3天后自动过期，防止遗忘的任务持续运行

- 使用 Ctrl + C 可以停止当前循环

> 💡 实用场景：监控CI/CD部署进度、定期检查构建状态、轮询外部服务健康状况。

### `/effort` - 推理深度控制

**一句话理解**：调整Claude的"思考深度"——简单问题快速回答，复杂问题深入分析。

```
/effort low     # ○ 快速简洁
/effort medium  # ◐ 平衡模式（默认）
/effort high    # ● 深度推理
/effort auto    # 重置为自动判断
```

| 级别 | 符号 | Token消耗 | 适用场景 |

| --- | --- | --- | --- |

| `low` | ○ | 最少 | 简单问答、格式转换 |

| `medium` | ◐ | 中等 | 日常开发、代码修改 |

| `high` | ● | 较多 | 架构设计、复杂调试 |

> 💡 省钱技巧：批量处理简单任务时切换到 `low`，关键决策时切换到 `high`。

### `/sandbox` - 沙箱隔离模式

**一句话理解**：给Claude套上"安全围栏"——限制它能读写哪些文件、能不能访问网络。

```
/sandbox        # 启用OS级沙箱隔离
```

**隔离能力**：

- **文件系统隔离**：限制可读写的目录范围

- **网络隔离**：限制外部网络访问

- **进程隔离**：OS级别的沙箱保护

在 `settings.json` 中可以精细配置：

```
{
  "sandbox": {
    "filesystem": {
      "allowWrite": ["/tmp", "./src"],
      "allowRead": ["./", "/usr/lib"],
      "denyRead": [".env", "credentials.json"]
    }
  }
}
```

> ⚠️ 安全提示：处理不信任的代码或敏感项目时，强烈建议启用沙箱模式。

### `/color` - 会话颜色设置

```
/color blue     # 设置会话窗口颜色为蓝色
/color default  # 重置为默认颜色
```

> 💡 多窗口区分：同时开多个Claude Code会话时，用不同颜色区分（比如：蓝色=前端、绿色=后端）。

### `/copy N` - 复制AI回复

```
/copy           # 复制最新一条AI回复到剪贴板
/copy 3         # 复制第3条AI回复到剪贴板
```

### `/branch` - 会话分支

```
/branch         # 从当前位置创建会话分支
```

**一句话理解**：就像Git分支——在当前对话点"分叉"，探索不同方案而不影响原对话。

> 💡 提示：这是原 `/fork` 命令的重命名版本。适合在关键决策点创建分支，分别尝试方案A和方案B。

---

## 第四部分：自定义命令开发

> **本节目的**：掌握自定义命令开发的完整技能

> **预计时间**：90分钟

### 4.1 命令文件结构

一个完整的命令文件包含两个部分：`frontmatter`配置区 + Markdown正文。

**完整结构示例**：

```
---
description: 公众号文章创作命令
argument-hint: <主题关键词>
allowed-tools:
  - Read
  - Write
  - WebSearch
model: claude-sonnet-4-6
---

# 公众号文章创作

你是一位资深的公众号写作专家。

## 任务
根据用户提供的主题创作一篇公众号文章。

主题：$ARGUMENTS

## 写作要求
1. 风格：接地气、说人话
2. 字数：1500-2000字
3. 结构：金句开头 → 核心内容 → 行动号召

## 执行步骤
1. 使用WebSearch搜索主题相关的最新信息
2. 构思文章大纲
3. 撰写完整文章
4. 使用Write工具保存到articles目录
```

### 4.2 命令存放位置与作用域

#### 两种作用域

| 作用域 | 存放位置 | 生效范围 | 适用场景 |

| --- | --- | --- | --- |

| 项目级 | `.claude/commands/` | 仅当前项目 | 团队共享、项目特定 |

| 用户级 | `~/.claude/commands/` | 所有项目 | 个人工具、通用模板 |

#### 优先级规则

当同名命令存在于多个位置时，按以下优先级：

```
1. 项目级（最高）: .claude/commands/
2. 用户级: ~/.claude/commands/
3. 内置命令（最低）
```

> ⚠️ 重要澄清：核心系统命令（如 `/clear`、`/help`、`/compact`、`/doctor` 等）是受保护的，不能被自定义命令覆盖。上述优先级规则仅适用于非核心内置命令。

**示例**：

假设存在：

- `.claude/commands/review.md`（项目级）

- `~/.claude/commands/review.md`（用户级）

- 内置 `/review` 命令

执行 `/review` 时，使用项目级的 `review.md`。

**但是**：创建 `.claude/commands/clear.md` 不会覆盖内置的 `/clear` 命令。

#### 目录结构建议

```
.claude/commands/
├── 01-write.md          # 核心写作命令
├── 02-write-auto.md     # 自动写作
├── 11-hotspot.md        # 热点扫描
├── 21-title-gen.md      # 标题生成
├── 22-title-score.md    # 标题评分
├── dev/                 # 开发类命令（子目录）
│   ├── code-review.md
│   └── debug.md
└── test/                # 测试类命令
    └── generate-tests.md
```

> **命名空间**：使用子目录时，命令名变成 `/dev:code-review` 格式。

### 4.3 `frontmatter`配置详解

`frontmatter`是命令文件开头的YAML配置区，用 `---` 包围。

#### 所有可用配置项

```
---
# === 官方支持的配置项 ===

# 命令描述（显示在命令列表中）
description: 这是命令的一句话描述

# 参数提示（输入/命令后显示的占位符）
argument-hint: <必需参数> [可选参数]

# 允许使用的工具（限制命令可调用的工具）
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - WebSearch
  - Glob
  - Grep

# 指定使用的模型
model: claude-sonnet-4-6

# 禁用模型调用（用于纯文本替换命令）
# disable-model-invocation: true

# === 非官方字段（可选，用于自己管理）===
# 注意：以下字段不会被Claude Code解析，仅供人类阅读
# version: 1.0.0   # 用于版本管理
# author: 老金     # 用于标注作者
---
```

> ⚠️ 注意：`version` 和 `author` 不是Claude Code官方支持的`frontmatter`字段，但你可以添加它们用于自己的管理目的（Claude Code会忽略未知字段）。

#### 配置项详解

1. `description`（命令描述）

```
description: AI热点自动扫描 - 抓取今日AI热点并评估爆款潜力
```

作用：在 `/help` 和Tab补全时显示，帮助用户了解命令功能。

2. `argument-hint`（参数提示）

```
argument-hint: <主题> [--style=formal] [--length=2000]
```

作用：输入 `/命令名` 后显示的占位符提示。

3. `allowed-tools`（允许的工具）

```
allowed-tools:
  - Read
  - Write
  - WebSearch
```

作用：限制命令只能使用指定的工具，提高安全性。

**可用工具列表**：

| 工具名 | 功能 | 适用场景 | 通俗解释 |

| --- | --- | --- | --- |

| `Read` | 读取文件 | 分析代码、读取配置 | 打开文件看内容 |

| `Write` | 写入文件 | 创建文件、保存结果 | 新建文件并写入 |

| `Edit` | 编辑文件 | 修改现有代码 | 修改已有文件的内容 |

| `Bash` | 执行命令 | 运行脚本、Git操作 | 在终端运行命令 |

| `WebSearch` | 网络搜索 | 获取最新信息 | 像Google一样搜网页 |

| `WebFetch` | 抓取网页 | 获取网页内容 | 下载网页内容来分析 |

| `Glob` | 按名称查找文件 | 批量查找特定类型的文件 | 比如找所有`.md`文件：`*.md` |

| `Grep` | 按内容搜索文件 | 在代码中搜索关键词 | 比如找包含`"TODO"`的代码 |

| `Task` | 启动子代理 | 并行执行复杂任务 | 派出分身帮你干活 |

| `NotebookEdit` | 编辑Jupyter笔记本 | 数据分析、机器学习 | 编辑`.ipynb`文件 |

| `TodoWrite` | 任务管理 | 跟踪待办事项 | 创建和更新待办清单 |

> 💡 提示：最常用的工具是 `Read`、`Write`、`Edit`、`Bash`、`WebSearch`。其他工具按需使用。

4. `model`（指定模型）

```
model: claude-opus-4-6
```

作用：强制使用指定模型执行命令（覆盖当前会话模型）。

5. `disable-model-invocation`（禁用模型调用）

```
disable-model-invocation: true
```

作用：当设置为`true`时，命令只进行简单的文本替换，不会触发AI模型推理。

**适用场景**：

- 纯文本模板命令（快速插入固定内容）

- 变量替换命令（`$ARGUMENTS` 直接替换）

- 节省Token和响应时间

**示例**：

```
---
description: 快速插入版权声明
disable-model-invocation: true
---

© 2025 $ARGUMENTS. All rights reserved.
```

执行 `/copyright 老金` 会直接输出 `© 2025 老金. All rights reserved.`，不经过AI处理。

### 4.4 `$ARGUMENTS`参数处理

`$ARGUMENTS` 是Claude Code的内置变量，接收用户在命令后输入的所有内容。

#### 基本用法

```
# 问候命令

用户要问候的对象是：$ARGUMENTS

如果没有提供名字，使用"朋友"作为默认称呼。
```

**调用示例**：

```
/hello 张三
→ $ARGUMENTS = "张三"

/hello
→ $ARGUMENTS = ""
```

#### 多参数解析

```
# 文章生成命令

## 参数格式
$ARGUMENTS 格式：<主题> [风格] [字数]

解析规则：
- 第一个参数：主题（必需）
- 第二个参数：风格（可选，默认"接地气"）
- 第三个参数：字数（可选，默认1500）

## 示例
/write AI工具 技术 3000
→ 主题=AI工具, 风格=技术, 字数=3000

/write Claude Code
→ 主题=Claude Code, 风格=接地气, 字数=1500
```

#### 参数验证

```
# 代码审查命令

## 输入验证
首先检查 $ARGUMENTS：

情况1：参数为空
提示用户：请提供文件路径，格式：/review <文件路径>
终止执行

情况2：不是有效路径
提示用户：路径无效，请检查文件是否存在
终止执行

情况3：验证通过
继续执行审查流程
```

### 4.5 工具调用语法

自定义命令可以调用Claude Code的所有工具。

#### 基础工具调用

```
## 执行步骤

### 步骤1：读取配置文件
使用Read工具读取项目配置：
```

```
Read(".claude/settings.json")
```

### 步骤2：搜索TODO注释
使用Grep搜索所有TODO：

```
Grep("TODO|FIXME", path="src/", type="py")
```

### 步骤3：运行测试
使用Bash执行测试命令：

```bash
npm run test
```

#### MCP工具调用

> 💡 什么是MCP？

> MCP（Model Context Protocol）是Claude Code的"外部工具扩展系统"。

> **通俗解释**：MCP就像手机的"应用商店"——你可以安装各种第三方工具（如搜索引擎、数据库查询、GitHub操作等），让Claude Code拥有更多能力。

> **命名规则**：`mcp__服务器名__工具名` 比如 `mcp__mcp-router__search` 就是调用mcp-router服务器的search工具。

> **详细教程**：MCP的安装和配置请见「04-MCP集成完整指南」。

```
## MCP工具使用

### 搜索最新信息
```

```
mcp__mcp-router__search(
    query="$ARGUMENTS 最新资讯",
    search_service="google",
    max_results=10
)
```

### 获取GitHub趋势

```
mcp__mcp-router__trending(
    search_service="github",
    max_results=20
)
```

### 4.6 条件逻辑设计

虽然Markdown不支持编程逻辑，但通过精心设计的提示词可以实现条件分支。

#### IF-ELSE模式

```
## 执行逻辑

根据 $ARGUMENTS 的内容判断执行路径：

情况1：参数包含"深度"或"详细"
→ 执行深度分析流程
→ 输出完整报告（3000字以上）
→ 包含数据图表

情况2：参数包含"快速"或"简要"
→ 执行快速分析
→ 输出简要报告（500字以内）
→ 只包含关键结论

情况3：其他情况（默认）
→ 执行标准分析
→ 输出标准报告（1500字）
→ 包含主要发现和建议
```

#### SWITCH-CASE模式

```
## 类型判断

检查 $ARGUMENTS 的第一个关键词，确定文章类型：

| 关键词 | 文章类型 | 使用模板 |
|--------|----------|----------|
| "测评" | 测评类 | templates/review.md |
| "教程" | 教程类 | templates/tutorial.md |
| "降价" | 降价类 | templates/discount.md |
| "对比" | 对比类 | templates/comparison.md |
| 其他 | 通用类 | templates/general.md |

根据判断结果，加载对应模板。
```

### 4.7 实战案例：创建写作命令

**目标**：创建一个完整的👸号写作命令

**文件：** `.claude/commands/write.md`
