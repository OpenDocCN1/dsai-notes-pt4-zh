
# 30 分钟练习项目 · 知识库问答系统 Starter

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/JhdfdgsWeoqajJxTw0YcA9MunWb](https://kq4b3vgg5b.feishu.cn/docx/JhdfdgsWeoqajJxTw0YcA9MunWb)

> 配套视频：《从"会用工具"到"能交付项目"——AI 驱动全栈开发实战》

> 作者：木羽Cheney（VibeCoding 实战课主讲老师） · 赋范大模型技术社区

> 用法：按步骤操作，用 `Claude Code` 最快 30 分钟完成你的第一个全栈交付

---

## 这个练习项目是什么？

一个最小的知识库问答系统：上传一段文本 → 对这段文本提问 → 得到 AI 生成的回答。

不需要：服务器、数据库安装、`Docker`、域名

只需要：`Node.js` + 一个 AI 编程工具 + 一个国产模型的免费 `API Key`

---

## 前提准备

| 需要什么 | 怎么获取 | 费用 |

| --- | --- | --- |

| `Node.js 18+` | [nodejs.org](https://nodejs.org) 下载安装 | 免费 |

| `Claude Code` 或其他 AI 编程工具 | `Claude Code`: `npm i -g @anthropic-ai/claude-code` | 需订阅 |

| 国产模型 `API Key` | 推荐智谱 `glm-4.7-flash`（永久免费）或通义千问 `qwen-plus`（7000 万免费 token） | 免费 |

---

## Step 1：创建项目（2 分钟）

打开终端，执行：

```
mkdir my-qa-system && cd my-qa-system
```

然后启动你的 AI 编程工具（以 `Claude Code` 为例）：

```
claude
```

---

## Step 2：技术调研 → 直接给答案（已为你选好）

这个练习项目的技术栈已经帮你选好了，不需要调研：

| 层级 | 选型 | 理由 |

| --- | --- | --- |

| 前端 | `Next.js 14` + `Tailwind CSS` | 前后端同框架，联调零成本 |

| 后端 | `Next.js API Routes` | 不需要单独搭后端服务 |

| 数据库 | `SQLite`（通过 `better-sqlite3`） | 零配置，文件即数据库 |

| AI | `OpenAI` 兼容 API | 可接国产模型 |

---

## Step 3：生成规范文档（5 分钟）

将以下提示词复制给 AI：

```
请帮我创建一个最小知识库问答系统的项目。

技术栈：
- Next.js 14（App Router）+ Tailwind CSS
- 后端用 Next.js API Routes
- 数据库用 SQLite（better-sqlite3，不用 Prisma）
- AI 用 OpenAI 兼容 API

核心功能只有两个：
1. 上传一段文本作为知识（不需要文件上传，只要一个文本框粘贴内容即可）
2. 对知识库提问，返回 AI 生成的回答（回答中引用知识库内容）

请先生成 CLAUDE.md 规范文档，包含：
- 项目概述
- 技术栈锁定
- 目录结构
- API 接口定义（2 个接口：保存知识、提问）
- 统一响应格式
- 数据库 Schema（1 张表：knowledge，字段：id, title, content, createdAt）
- 非目标：不做用户认证、不做文件上传、不做向量检索（用简单的文本搜索）、不做部署

生成 CLAUDE.md 后，等我确认再开始写代码。
```

**确认点**：AI 生成的 `CLAUDE.md` 你看一眼，确保接口定义和数据模型没问题，然后说"确认，开始搭建"。

---

## Step 4：搭建后端（10 分钟）

```
确认规范文档没问题。现在请按 CLAUDE.md 搭建整个项目：

1. 初始化 Next.js 项目（npx create-next-app@latest）
2. 安装依赖：better-sqlite3, openai
3. 创建数据库初始化脚本
4. 实现两个 API 接口：
   - POST /api/knowledge — 保存知识条目
   - POST /api/chat — 提问并获取回答
5. 创建 .env.example

实现完成后，用 curl 测试两个接口。

环境变量用以下值（或你自己的）：
LLM_BASE_URL=https://open.bigmodel.cn/api/paas/v4
LLM_API_KEY=你的智谱API Key
LLM_MODEL=glm-4.7-flash
```

**确认点**：两个接口 `curl` 测试通过后再继续。

---

## Step 5：开发前端（10 分钟）

```
后端 API 已验证通过。现在请按 CLAUDE.md 开发前端，两个页面：

页面 1：知识管理页（/）
- 顶部：一个文本域 + "保存"按钮，用于添加知识条目
- 下方：已保存的知识条目列表（标题 + 内容预览）
- 调用 POST /api/knowledge 保存，GET 方式获取列表

页面 2：问答页（/chat）
- 聊天界面：用户输入框 + 消息列表
- 用户发送问题后，调用 POST /api/chat
- 展示 AI 的回答
- 导航栏可以在两个页面间切换

每做完一个页面立刻对接后端验证，不要全做完再调。
必须处理加载状态和错误状态。
```

---

## Step 6：联调验证（3 分钟）

```
请做一次完整的端到端验证：

1. 打开首页，添加一条知识："公司年假规定是每年 15 天，入职满一年后可使用。"
2. 确认知识条目出现在列表中
3. 切换到问答页，提问："我能请多少天年假？"
4. 确认回答引用了知识库中的内容
5. 再提问一个知识库里没有的问题，确认 AI 不会编造

全部通过后，生成 README.md（安装步骤 + 启动命令）。
```

---

## 做完了

到这一步，你手里已经有了一个完整的全栈项目：

```
my-qa-system/
├── src/
│   ├── app/
│   │   ├── page.tsx          # 知识管理页
│   │   └── chat/page.tsx     # 问答页
│   └── api/
│       ├── knowledge/route.ts # 知识保存接口
│       └── chat/route.ts      # 问答接口
├── db/                        # SQLite 数据库文件
├── CLAUDE.md                  # 规范文档
├── .env                       # 环境变量
├── .env.example               # 环境变量模板
├── README.md                  # 项目说明
└── package.json
```

一个完整的前后端项目：有数据库、有 `API`、有前端界面、有 AI 功能、能端到端跑通。

---

## 做完后，下一步做什么？

### 加功能练手（每个 10-20 分钟）

| 功能 | 难度 | 练什么 |

| --- | --- | --- |

| 给知识条目加"标签"分类 | 低 | 修改数据模型 + 前端 UI |

| 问答页显示"引用来源" | 中 | 后端改返回格式 + 前端展示 |

| 支持 `Markdown` 格式的回答 | 低 | 前端引入 `Markdown` 渲染组件 |

| 对话历史记录 | 中 | 新增数据表 + 接口 + 前端展示 |

| 换一个国产模型（如 `DeepSeek`） | 低 | 只改 `.env` 中的三个变量 |

| 加一个"文档上传"功能（真正的文件上传） | 高 | 文件处理 + 文本提取 + 接口改造 |

### 做自己的项目

用工具包中的**路线图**和**规范文档模板**，从零开始做你自己想做的东西。流程一模一样：

```
技术调研 → 规范文档 → 后端 MVP → PRD → 前端 → 联调
```

不同的只是内容——而这套方法是通用的。

---

## 常见问题

### Q：我不是用 `Claude Code`，用 `Cursor` / `Windsurf` / 其他工具可以吗？

**A**：可以。提示词是通用的，规范文档放在项目根目录，任何 AI 编程工具都会读取。

### Q：我不会 `TypeScript` / `React`，能做吗？

**A**：能。AI 会帮你写代码，你只需要会运行命令（`npm install` / `npm run dev`）和在浏览器里验证结果。但建议对照代码多看看 AI 写了什么，这是最好的学习方式。

### Q：30 分钟真的够吗？

**A**：熟练的话 30 分钟可以完成。第一次做可能需要 45-60 分钟，因为要理解每一步在做什么。不用着急，理解比速度更重要。

### Q：`API Key` 从哪来？要花钱吗？

**A**：推荐智谱 `GLM-4.7-Flash`，完全免费，注册后就能拿到 `API Key`。详见工具包中的《国产模型速查表》（上一期工具包）。

---

更多 AI 实战教程，关注赋范大模型技术社区。

![](img/cbdd9290c52a362f5397d9f084f5b7dc.png)
