
# AI 全栈交付 · 规范文档模板（填空版）

## 02-规范文档模板-填空即用版

> 来源：https://kq4b3vgg5b.feishu.cn/docx/BAP3dm8Ukof3v2xcGdTcmRi3nzb

> 配套视频：《从"会用工具"到"能交付项目"——AI 驱动全栈开发实战》

> 作者：木羽Cheney （VibeCoding 实战课主讲老师）· 赋范大模型技术社区

> 用法：复制整个模板，按注释提示填写每个字段。填完后放到项目根目录的 `CLAUDE.md` 或 `.claude/rules/spec.md` 中

---

## 为什么需要这个文档？

> "你每次开一个新的会话，AI 就是失忆的。它不记得你前面做了什么决策，接口是怎么定义的，架构约束在哪里。" —— 视频原话

这份规范文档是给 AI 看的底座，不是给人看的说明书。AI 每次会话开始时读一遍，就知道：

+ 用什么技术栈

+ API 接口的格式和约定

+ 数据库有哪些表、什么字段

+ 哪些东西不能动

有这个文档 vs 没有：区别是"AI 全程按统一规范写代码" vs "AI 每次自由发挥，前后端对不上"。

---

## 模板正文（复制以下内容 → 填空 → 放入 CLAUDE.md）

```
# 项目规范文档

> 本文档是 AI 协作开发的"宪法"。AI 在每次会话开始时必须阅读此文件。
> 所有代码生成必须遵循本文档中的约定，不得自行修改架构和接口定义。

## 1. 项目概述

- 项目名称：________________（例：智能知识库问答系统）
- 一句话描述：________________（例：基于 RAG 的企业内部知识库问答工具）
- 目标用户：________________（例：公司内部员工，约 50 人）
- 核心功能：
  1. ________________（例：上传文档建立知识库）
  2. ________________（例：自然语言提问获取答案）
  3. ________________（例：答案附带来源引用）

## 2. 技术栈（锁定，不得更换）

| 层级 | 选型 | 版本 | 选型理由 |
|------|------|------|---------|
| 前端框架 | __________ | __________ | __________ |
| UI 组件库 | __________ | __________ | __________ |
| 后端框架 | __________ | __________ | __________ |
| 数据库 | __________ | __________ | __________ |
| ORM/查询层 | __________ | __________ | __________ |
| AI/LLM | __________ | __________ | __________ |
| 部署方式 | __________ | __________ | __________ |

### 技术栈示例（知识库问答项目）

| 层级 | 选型 | 版本 | 选型理由 |
|------|------|------|---------|
| 前端框架 | Next.js | 14.x | App Router + SSR，SEO 友好 |
| UI 组件库 | Tailwind CSS + shadcn/ui | 最新 | 快速搭建，组件丰富 |
| 后端框架 | Next.js API Routes | 14.x | 前后端同框架，减少联调成本 |
| 数据库 | SQLite | 3.x | 零配置，本地开发免安装 |
| ORM | Prisma | 5.x | 类型安全，schema 即文档 |
| AI/LLM | OpenAI 兼容 API | - | 可切换国产模型 |
| 部署 | 本地 Node.js | 20.x | MVP 阶段不上云 |

## 3. 目录结构
```

```
项目根目录/

├── src/

│ ├── app/ # ← 前端页面（Next.js App Router）

│ │ ├── page.tsx # 首页

│ │ ├── chat/page.tsx # 问答页

│ │ └── upload/page.tsx # 上传页

│ ├── components/ # ← 可复用组件

│ ├── lib/ # ← 工具函数、配置

│ └── api/ # ← 后端 API Routes

│ ├── chat/route.ts # 问答接口

│ ├── upload/route.ts # 上传接口

│ └── documents/route.ts # 文档管理接口

├── prisma/

│ └── schema.prisma # 数据库 Schema

├── public/ # 静态资源

├── .env # 环境变量（不提交 Git）

├── .env.example # 环境变量示例

├── CLAUDE.md # ← 本文件

└── package.json

```

注意：目录结构确定后不要轻易调整。AI 根据这个结构定位文件，改了结构不改文档 = AI 找不到文件。

## 4. API 接口定义（核心，联调的基础）

### 4.1 通用约定

- 协议：RESTful JSON API
- 基础路径：`/api/`
- 请求格式：`Content-Type: application/json`
- 响应格式：统一使用以下结构

```json
// 成功响应

{

  "success": true,

  "data": { ... },

  "message": "操作成功"

}

// 错误响应

{

  "success": false,

  "error": {

    "code": "VALIDATION_ERROR",

    "message": "问题内容不能为空"

  }

}

```

+ HTTP 状态码：200 成功 / 201 创建成功 / 400 参数错误 / 404 资源不存在 / 500 服务器错误

### 4.2 接口清单

#### 接口 1：________________

```

方法: POST

路径: /api/________________

描述: ________________

请求体:

{

  "________________": "________________",

  "________________": ________________

}

成功响应 (200):

{

  "success": true,

  "data": {

    "________________": "________________",

    "________________": ________________

  }

}

错误响应 (400):

{

  "success": false,

  "error": {

    "code": "________________",

    "message": "________________"

  }

}

```

#### 接口 2：________________

```

方法: ________

路径: /api/________________

描述: ________________

请求体/参数:

________________

成功响应:

________________

```

#### 接口 3：________________

```

（同上格式，逐个定义）

```

> 填写提示：

+ 每个接口都要写清楚请求体和响应体的完整字段

+ 字段名一旦定义，前后端必须完全一致（包括大小写）

+ 先定义核心接口（3-5 个），非核心的后面按需追加

### 4.3 接口示例（知识库问答项目）

#### 问答接口

```

方法: POST

路径: /api/chat

描述: 用户提问，返回基于知识库的回答

请求体:

{

  "question": "如何申请年假？",

  "conversationId": "conv_123"    // 可选，续接上下文

}

成功响应 (200):

{

  "success": true,

  "data": {

    "answer": "根据公司规定，年假申请流程如下...",

    "sources": [

      {"title": "员工手册 v3.2", "page": 15, "relevance": 0.92}

    ],

    "conversationId": "conv_123"

  }

}

```

#### 文档上传接口

```

方法: POST

路径: /api/upload

描述: 上传文档到知识库

请求体: multipart/form-data

  - file: 文件（PDF/TXT/MD，最大 10MB）

  - title: 文档标题（可选，默认用文件名）

成功响应 (201):

{

  "success": true,

  "data": {

    "documentId": "doc_456",

    "title": "员工手册 v3.2",

    "pageCount": 42,

    "status": "processing"

  }

}

```

## 5. 数据模型

### 5.1 数据表定义

#### 表 1: ________________

| 字段名 | 类型 | 约束 | 说明 |
| --- | --- | --- | --- |
| `id` | ________ | 主键，自增 | ________ |
| ________ | ________ | ________ | ________ |
| ________ | ________ | ________ | ________ |
| `createdAt` | DateTime | 自动生成 | 创建时间 |
| `updatedAt` | DateTime | 自动更新 | 更新时间 |

#### 表 2: ________________

| 字段名 | 类型 | 约束 | 说明 |
| --- | --- | --- | --- |
| ________ | ________ | ________ | ________ |

### 5.2 数据模型示例（Prisma Schema）

```

model Document {

  id        Int      @id @default(autoincrement())

  title     String

  content   String   @db.Text

  fileType  String   // "pdf" | "txt" | "md"

  fileSize  Int      // bytes

  status    String   @default("processing") // "processing" | "ready" | "error"

  createdAt DateTime @default(now())

  updatedAt DateTime @updatedAt

  chunks    Chunk[]

}

model Chunk {

  id         Int      @id @default(autoincrement())

  content    String   @db.Text

  embedding  String   @db.Text    // JSON 序列化的向量

  documentId Int

  document   Document @relation(fields: [documentId], references: [id])

  createdAt  DateTime @default(now())

}

model Conversation {

  id        String   @id @default(uuid())

  messages  Message[]

  createdAt DateTime @default(now())

  updatedAt DateTime @updatedAt

}

model Message {

  id             Int          @id @default(autoincrement())

  role           String       // "user" | "assistant"

  content        String       @db.Text

  sources        String?      @db.Text  // JSON 序列化的引用来源

  conversationId String

  conversation   Conversation @relation(fields: [conversationId], references: [id])

  createdAt      DateTime     @default(now())

}

```

## 6. 架构约束（AI 必须遵守的红线）

### 6.1 必须遵守

### 6.2 非目标（本阶段明确不做的事）

> 这部分极其重要。不写"不做什么"，AI 会自作主张加功能。

+ 不实现用户认证/登录系统（MVP 阶段不需要）

+ 不做国际化/多语言

+ 不做移动端适配（先做桌面端）

+ 不做自动化部署/CI/CD

+ 不做性能优化（先跑通再优化）

+ 不引入缓存层（Redis 等）

+ 不做单元测试（MVP 阶段手动验证）

> 根据你的项目实际情况，增删上面的条目。原则是：MVP 阶段只做核心功能，其他全部明确排除。

## 7. 启动命令

```

# 安装依赖

npm install

# 初始化数据库

npx prisma db push

npx prisma db seed

# 开发模式启动

npm run dev

# 生产构建

npm run build && npm start

```

## 8. 环境变量

```

# .env.example — 复制为 .env 后填写真实值

# 数据库

DATABASE_URL="file:./dev.db"

# LLM 配置

LLM_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"

LLM_API_KEY="sk-your-key-here"

LLM_MODEL="qwen-plus"

# 应用配置

PORT=3000

NODE_ENV="development"

```

## 9. 变更记录

| 日期 | 变更内容 | 变更人 |
| --- | --- | --- |
| ________ | 初始版本 | ________ |

> 规则：任何对规范文档的修改都必须记录在这里。AI 不得自行修改此文档。

---

## 填写指南

| 章节 | 填写优先级 | 说明 |

|:----:|:---------:|------|

| §1 项目概述 | 高 | 先填，定义做什么 |

| §2 技术栈 | 高 | 调研后填，锁定不改 |

| §3 目录结构 | 中 | 初始化项目后补充 |

| §4 API 接口 | 高 | 最核心，前后端联调的基础 |

| §5 数据模型 | 高 | 和 API 同步定义 |

| §6 架构约束 | 高 | 尤其是"非目标"，必须填 |

| §7 启动命令 | 中 | 后端跑通后填 |

| §8 环境变量 | 中 | 有环境变量时补充 |

> 宁可多花 1 小时写规范，也不要花 5 小时联调时返工。这笔账很好算。

---

更多 AI 实战教程，关注赋范大模型技术社区。

![](img/f1cecb393eff1cf8a3f5359351fc3e34.png)
