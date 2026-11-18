
# Obsidian 技能示例

```
列出 Obsidian 里所有带 #todo 标签的笔记
查看"读书笔记"文件夹下的所有文件
帮我创建一个 MOC（Map of Content）页面，整理所有编程相关笔记
更新"学习路线图"笔记，添加新的学习目标
```

## 双向链接

Obsidian 技能理解 Obsidian 的双向链接语法：

```
创建一篇笔记"React Hooks"，并链接到已有的"React 基础"笔记
查找所有链接到"项目 A"的笔记
```

AI 会自动使用 `[[双向链接]]` 语法，保持你的知识图谱完整。

# 技能配置文件详解

每个技能的行为都可以通过配置文件来调整。

## 全局技能配置

在 `~/.openclaw/openclaw.json` 中的 `skills` 字段：

```json
{
  "skills": {
    // 是否加载 OpenClaw 内置技能（默认 true）
    "allowBundled": true,
    // 从指定路径加载额外技能
    "load": ["~/.openclaw/workspace/skills/*"],
    // 各技能的独立配置
    "entries": {
      "gog": {
        "enabled": true,
        "config": {
          "timezone": "Asia/Shanghai"
        }
      },
      "github": {
        "enabled": true,
        "config": {
          "defaultOrg": "my-org"
        }
      },
      "voice-call": {
        "enabled": false
      }
    }
  }
}
```

## 配置字段说明

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `allowBundled` | boolean | 是否加载内置技能（默认 true） |
| `load` | string[] | 额外技能的加载路径 |
| `install` | object | 从 ClawHub 安装技能的配置 |
| `limits` | object | 技能执行的限制配置 |
| `entries` | object | 各技能的独立配置（`enabled`、`apiKey`、`env`、`config`） |

要禁用某个技能，在 `entries` 中将其 `enabled` 设为 `false`。技能的自定义参数放在 `config` 子对象中。

## Agent 级别的技能配置

每个 Agent 可以有自己的技能配置，覆盖全局设置：

```json
{
  "agents": {
    "list": [
      {
        "id": "coding",
        "workspace": "~/.openclaw/workspace-coding",
        // Agent 级别的 skills 是一个简单的字符串数组
        "skills": ["coding-agent", "github", "gh-issues", "tmux"],
      },
      {
        "id": "office",
        "workspace": "~/.openclaw/workspace-office",
        "skills": ["gog", "slack", "notion", "trello", "summarize"],
      },
    ],
  },

  // 技能的详细配置放在顶层 skills.entries 中
  "skills": {
    "entries": {
      "coding-agent": {
        "config": {
          "workDir": "~/projects",
          "autoApprove": true,
        },
      },
      "gog": {
        "config": {
          "timezone": "Asia/Shanghai",
        },
      },
    },
  },
}
```

这样，编程 Agent 只加载开发相关技能，办公 Agent 只加载办公相关技能，互不干扰。注意 Agent 级别的 `skills` 是一个简单的字符串数组，而技能的详细配置（如自定义参数）放在顶层 `skills.entries` 中。

## 技能的 SKILL.md（技能的定义文件，用 Markdown 格式编写）文件格式

每个技能目录下的核心文件是 `SKILL.md`（注意大写）。这是一个 Markdown 文件，AI 会读取它来理解如何执行任务。

`SKILL.md` 的结构：

````markdown
# 技能名称

技能的描述和用途说明。

## 触发条件

描述什么情况下应该激活这个技能。

## 执行步骤

1. 第一步操作
2. 第二步操作
3. ...

## 依赖工具

列出技能需要调用的工具。

## 注意事项

执行时需要注意的约束和边界条件。
````

OpenClaw 还会自动注入以下 prompt 文件到技能上下文中：

| 文件 | 用途 |
| --- | --- |
| `AGENTS.md` | Agent 角色和行为定义 |
| `SOUL.md` | AI 人格和沟通风格 |
| `TOOLS.md` | 可用工具列表和使用说明 |

这些文件与 `SKILL.md` 配合，让 AI 在执行技能时拥有完整的上下文。

> ⏭️ **小白可跳过** — 这部分面向想创建自定义技能的开发者

# 自定义技能开发完整教程

这是 OpenClaw 最强大的功能之一。你可以教 AI 新的能力，而且不需要写复杂的代码 — 大部分情况下写 Markdown 就够了。

## 方法一：用 skill-creator 技能（最简单）

直接告诉 AI：

```
帮我创建一个技能，用来管理我的读书笔记。
每次我说"记录读书笔记"，就创建一个新的 Markdown 文件，
包含书名、作者、日期、关键摘录和我的感想。
```

AI 会自动使用 `skill-creator` 技能帮你生成完整的技能文件。

## 方法二：手动创建（完全控制）

下面我们从零开始创建一个完整的自定义技能：读书笔记管理器。

### 第一步：创建技能目录

```
mkdir -p ~/.openclaw/workspace/skills/reading-notes
mkdir -p ~/.openclaw/workspace/skills/reading-notes/tools
mkdir -p ~/.openclaw/workspace/skills/reading-notes/examples
```

目录结构：

```
~/.openclaw/workspace/skills/reading-notes/
├── SKILL.md          # 技能描述和指令（核心文件）
├── tools/            # 自定义工具（可选）
│   └── reading-stats.ts
└── examples/         # 使用示例（可选）
    └── example.md
```

### 第二步：编写 SKILL.md（核心文件）

这是技能的灵魂。AI 会读取这个文件来理解如何执行任务。

````markdown
# 读书笔记管理

你是一个读书笔记助手。当用户要求记录读书笔记时，按以下流程操作。

## 触发条件

当用户提到以下关键词时激活：记录读书笔记、读书笔记、我读了一本书、阅读记录、读书

## 依赖工具

- file_read
- file_write
- file_search

## 创建新笔记

1. 询问用户以下信息（如果用户没有主动提供）：
   - 书名
   - 作者
   - 评分（1-5 星）
2. 在 `workspace/reading-notes/` 目录下创建 Markdown 文件
3. 文件名格式：`YYYY-MM-DD-书名.md`
4. 使用以下模板：

### 笔记模板

```markdown
# 《{书名}》读书笔记

- **作者**：{作者}

- **阅读日期**：{日期}

- **评分**：{星级}

## 关键摘录

> （用户提供的摘录内容）

## 个人感想

（用户的感想）

## 行动项

- [ ] （从书中获得的待办事项）
```

5. 更新 `workspace/reading-notes/INDEX.md` 索引文件
6. 索引文件按评分降序排列

## 查看阅读统计

当用户问"我读了多少书"或"阅读统计"时：

1. 扫描 `workspace/reading-notes/` 目录
2. 统计总数、平均评分、按月分布
3. 返回格式化的统计信息

## 搜索笔记

当用户搜索读书笔记时：

1. 在 `workspace/reading-notes/` 目录中搜索
2. 支持按书名、作者、关键词搜索
3. 返回匹配的笔记列表和摘要
```
````

### 第三步：添加自定义工具（可选）

如果你的技能需要超出内置工具能力的功能，可以写自定义工具。

创建 `tools/reading-stats.ts`：

```typescript
import { Tool, ToolInput, ToolOutput } from "@openclaw/sdk";
import * as fs from "fs/promises";
import * as path from "path";

interface StatsInput extends ToolInput {
  notesDir: string;
}

interface BookStats {
  totalBooks: number;
  averageRating: number;
  booksByMonth: Record<string, number>;
  topRated: Array<{ title: string; rating: number }>;
}

export const readingStats: Tool<StatsInput, BookStats> = {
  name: "reading_stats",
  description: "统计读书笔记数据",

  parameters: {
    type: "object",
    properties: {
      notesDir: {
        type: "string",
        description: "读书笔记目录路径",
      },
    },
    required: ["notesDir"],
  },

  async execute(input: StatsInput): Promise<ToolOutput<BookStats>> {
    const { notesDir } = input;

    const files = await fs.readdir(notesDir);
    const mdFiles = files.filter(
      (f) => f.endsWith(".md") && f !== "INDEX.md"
    );

    let totalRating = 0;
    const booksByMonth: Record<string, number> = {};
    const topRated: Array<{ title: string; rating: number }> = [];

    for (const file of mdFiles) {
      const content = await fs.readFile(
        path.join(notesDir, file),
        "utf-8"
      );

      const ratingMatch = content.match(/\*\*评分\*\*：(\d)/);
      const rating = ratingMatch ? parseInt(ratingMatch[1]) : 0;
      totalRating += rating;

      const dateMatch = file.match(/^(\d{4}-\d{2})/);
      if (dateMatch) {
        const month = dateMatch[1];
        booksByMonth[month] = (booksByMonth[month] || 0) + 1;
      }

      const titleMatch = content.match(/# 《(.+?)》/);
      if (titleMatch) {
        topRated.push({ title: titleMatch[1], rating });
      }
    }

    topRated.sort((a, b) => b.rating - a.rating);

    return {
      success: true,
      data: {
        totalBooks: mdFiles.length,
        averageRating:
          mdFiles.length > 0 ? totalRating / mdFiles.length : 0,
        booksByMonth,
        topRated: topRated.slice(0, 10),
      },
    };
  },
};
```

### 第四步：添加使用示例（可选但推荐）

创建 `examples/example.md`：

```markdown
# 读书笔记技能使用示例

## 示例 1：记录新书

用户：我刚读完《深入理解计算机系统》，作者是 Randal Bryant，给 5 星。
关键摘录："程序员需要理解计算机系统的全貌，才能写出高效的代码。"
感想：这本书让我对底层原理有了全新的认识。

AI 操作：
1. 创建 `workspace/reading-notes/2026-02-25-深入理解计算机系统.md`
2. 填入模板内容
3. 更新 `INDEX.md`

## 示例 2：查看统计

用户：我今年读了多少书？

AI 操作：
1. 扫描 `reading-notes` 目录
2. 筛选今年的文件
3. 返回统计数据
```

### 第五步：测试技能

```
# 检查技能是否被正确识别
openclaw skills list

# 查看技能详情
openclaw skills info reading-notes

# 检查技能就绪状态
openclaw skills check reading-notes
```

然后在对话中测试：

```
记录读书笔记：《代码整洁之道》，作者 Robert C. Martin，5 星
```

## 方法三：基于现有技能扩展

你可以复制一个现有技能，然后修改它：

```
# 复制 obsidian 技能作为模板
cp -r ~/.openclaw/workspace/skills/obsidian ~/.openclaw/workspace/skills/my-notes

# 编辑 SKILL.md，修改为你的需求
```

工作空间技能会覆盖同名的共享技能，所以你可以用这种方式"魔改"内置技能。

## 技能的类型与作用域

OpenClaw 的技能分为三种类型：

| 类型 | 说明 | 来源 |
| --- | --- | --- |
| `bundled`（内置） | 随 OpenClaw 安装的技能，不可修改 | 安装目录 |
| `managed`（托管） | 从 ClawHub（OpenClaw 的官方技能市场，类似于手机的应用商店）安装的社区技能，可更新 | ClawHub |
| `workspace`（工作区） | 用户自己创建的技能 | `~/.openclaw/workspace/skills/` |

所有用户可编辑的技能都存放在 `~/.openclaw/workspace/skills/<skill-name>/` 目录下，核心文件为 `SKILL.md`。
