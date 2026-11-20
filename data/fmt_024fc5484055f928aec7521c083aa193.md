
# 第二篇：实战篇：精通 `GEMINI.md` 语法，打造你的专属 AI 工程师

> 来源：[https://huchegames.feishu.cn/docx/UZaGdcym2o4PGqxbiPycPCpzn5g](https://huchegames.feishu.cn/docx/UZaGdcym2o4PGqxbiPycPCpzn5g)
>
> 🔗 原文链接：https://juejin.cn/post/7522007669467381796
>
> ⏰ 2025-07-02

## 引言：从“知道”到“做到”

在上一篇 [第一篇：入门篇：借助 `GEMINI.md`，将你的 AI 协作提升到新高度引言](https://huchegames.feishu.cn/wiki/GNzDwqZQgi3OTNkI4jpcVrm1nZb)中，我们了解了 `GEMINI.md` 的核心价值——它如同一个“项目简报”，能让 AI 深入理解我们的项目。我们知道了它“是什么”以及“为什么”重要。

现在，是时候从“知道”迈向“做到”了。本篇实战指南将详细拆解 `GEMINI.md` 的每一个关键部分，并提供源自官方指南的最佳实践和具体范例。读完本文，你将能够亲手编写一份高质量的配置文件，将通用 AI 模型“调教”成一个为你项目量身定制的专属工程师。

### 第一部分：编写高效的项目上下文 (Project Context)

项目上下文是整个配置文件的基石，也是 AI 理解你项目的基础。一个模糊的上下文只会得到一个模糊的回答。反之，一个清晰、详尽的上下文，能让 AI 的每一次回答都切中要害。

一个高效的上下文应至少包含以下三个方面：

#### 1. 项目目标与业务逻辑 (Project Goals & Business Logic)

在这里，你需要清晰地定义项目要解决的问题、核心业务规则和流程，以及任何特殊的约束，如时间、资源或合规性要求。

+ **范例：企业级任务管理系统**

+ **核心目标**：构建一个支持多租户架构（服务1000+组织）、实时协作（支持50+并发用户）的企业级任务管理系统，并保证企业级安全与合规。

+ **业务价值**：旨在提高团队协作效率30%，减少项目管理成本40%，并支持远程工作。

#### 2. 技术栈与选型理由 (Tech Stack & Rationale)

详细列出项目使用的技术栈，更重要的是，简单说明“为什么”选择它们。这能帮助 AI 理解你的技术偏好和架构决策背后的思考。

+ **范例：全栈项目技术栈**

+ **后端**：
  + **Runtime**：`Node.js 18+` (LTS)
  + **框架**：`Express.js 4.18+`
  + **语言**：`TypeScript 5.0+`
  + **数据库**：`PostgreSQL 15+` (主)，`Redis 7+` (缓存)
  + **ORM**：`Prisma 5.0+`
+ **前端**：
  + **框架**：`React 18+` with `TypeScript`
  + **状态管理**：`Zustand` + `TanStack Query`
  + **UI 库**：`Tailwind CSS` + `Headless UI`
+ **选型理由**：
  + **TypeScript**：类型安全，提高代码质量和开发效率。
  + **Prisma**：提供类型安全的 ORM 和优秀的开发体验。

#### 3. 核心架构与设计原则 (Core Architecture & Design Principles)

描述系统的核心架构模式和关键设计原则。这能确保 AI 生成的代码和方案符合项目的整体架构。

+ **范例：微服务与分层架构**

+ **架构模式**：
  + 采用微服务架构，包含用户服务、任务服务、通知服务。
  + 内部采用分层架构: `Controller` -> `Service` -> `Repository` -> `Database`。
+ **关键设计原则**：
  + **单一职责**：每个服务专注单一业务领域。
  + **依赖注入**：使用 IoC 容器管理依赖。
  + **开闭原则**：通过插件机制扩展功能。

### 第二部分：设定清晰的行为准则 (Rules and Instructions)

如果说“项目上下文”是告诉 AI “我们做什么”，那么“行为准则”就是告诉它“你应该怎么做”。在这里，你将定义 AI 的角色、性格和行为边界。

#### 明确性原则：拒绝模棱两可

规则必须具体、可衡量、无歧义。模糊的指令只会得到无用的回答。

+ ❌ **错误示例**：
  + 代码应该是好的。
  + 尽量使用最佳实践。
+ ✅ **正确示例**：
  + 所有 API 函数必须包含 `JSDoc` 注释，描述参数、返回值和可能的异常。
  + 数据库查询响应时间应在 `100ms` 内，超时设置为 `5` 秒。

#### 定义 AI 角色与语气

你可以为 AI 设定一个非常具体的人设，这有助于统一它的沟通风格。

+ **范例：定义一个资深全栈工程师**

+ **AI 角色定义**：你是一个资深的全栈开发工程师，专精于 `Node.js` 后端开发（`Express.js`, `TypeScript`）、`React` 前端开发（Hooks, Context API）、`PostgreSQL` 数据库设计和 `AWS` 云服务部署。

+ **沟通语气**：
  + **教学导向**：解释为什么这样做，不只是怎样做。
  + **安全意识**：始终考虑安全性和最佳实践。
  + **简洁明了**：避免冗长的解释，直接给出核心信息。

### 第三部分：通过示例（Few-shot Examples）精准“示教”

这是 `GEMINI.md` 中最强大的功能之一。相比于长篇大论的规则，提供 3-5 个高质量的示例，能让 AI 以最快的方式学习到你的期望。

#### 示例覆盖策略

你的示例应该覆盖项目中最高频或最关键的场景，例如：

+ CRUD 操作的标准模式。
+ 错误处理的最佳实践。
+ 性能优化（如缓存）的场景。
+ 安全相关的考虑（如认证、授权）。
+ 测试用例的编写方式。

#### 一个高质量的范例：实现用户认证 API

**用户问题**：我需要实现一个用户登录的 API 端点，包括密码验证和 `JWT` token 生成。

**期望回答**：我来帮你实现一个安全的用户登录 `API`：

1. **路由定义**：

```ts
// routes/auth.ts
import { Router } from 'express';
import { loginController } from '../controllers/authController';
import { validateLogin } from '../middleware/validation';

const router = Router();
router.post('/login', validateLogin, loginController);
export default router;
```

1. **控制器实现**：

```ts
// controllers/authController.ts
import bcrypt from 'bcrypt';
import jwt from 'jsonwebtoken';
import { User } from '../models/User';

export const loginController = async (req: Request, res: Response) => {
  // ... (省略具体实现)
  
  // 生成 JWT
  const token = jwt.sign(
    { userId: user.id, email: user.email },
    process.env.JWT_SECRET!,
    { expiresIn: '24h' }
  );
  
  res.json({ token, user: { id: user.id, email: user.email } });
};
```

1. **关键安全点**：
   + 使用 `bcrypt` 进行密码哈希。
   + `JWT` 设置合理的过期时间。
   + 不在响应中返回敏感信息。
   + 统一的错误消息避免信息泄露。

### 第四部分：文件规则与分层配置

#### 引导 AI 的注意力

通过 **重要文件** 和 **忽略文件**，你可以像使用 `.gitignore` 一样，精确地引导 AI 的扫描范围，提升其效率和相关性。

```
## 重要文件和目录
- `src/components/` - React 组件
- `docs/api/` - API 文档

## 忽略文件
- `node_modules/`
- `*.log`
- `.env*`
```

#### 分层配置的威力

`Gemini Code Assist` 采用分层内存系统，允许你在不同级别放置 `GEMINI.md` 文件，实现配置的继承和覆盖。

1. **全局级别** (`~/.**gemini/GEMINI.md`)：定义适用于你所有项目的通用规范，如 `Git` 提交格式、通用代码风格等。

1. **项目级别** (`./GEMINI.md`)：定义项目专属的配置，如技术栈、架构、项目级规范等。它可以继承全局配置并进行覆盖。

1. **组件级别** (`./src/components/GEMINI.md`)：为项目的特定模块或组件定义更细粒度的规范，如“`React` 组件必须使用 `forwardRef` 处理 `ref` 传递”。

这种分层系统让你可以在保持全局一致性的同时，为不同项目和模块提供灵活的、定制化的指导。

## 结论：你的 AI 助手已准备就绪

通过精心编写项目上下文、设定清晰的行为准则、提供高质量的示例以及配置分层策略，你已经将一份普通的 `GEMINI.md` 文件变成了一套强大的 AI 指导系统。现在，你的 AI 助手不再是泛泛而谈的“万事通”，而是真正理解你项目需求的“资深专家”。

当然，`GEMINI.md` [第三篇：精通篇：高级技巧与团队协作，释放 `GEMINI.md` 的全部潜力 引言：从个人工具到团队基石 在系列的前两篇文章中 - 掘金](https://huchegames.feishu.cn/wiki/NwbcwSfQTiDs4ckMuN6ccOELnzf) [的](https://link.juejin.cn/?target=https%3A%2F%2Fblog.csdn.net%2Fgithub_37499193%2Farticle%2Fdetails%2F149048181)潜力远不止于此。在我们的收官之作 中，我们将探讨如何集成自定义工具、应对更复杂的项目场景（如多语言、高安全）、以及如何在团队中建立高效的协作和维护机制。准备好进入 `GEMINI.md` 的高阶世界了吗？我们下一篇见！

---

<https://huchegames.feishu.cn/sync/Hzv6dRSGJsmHgOb4eSHcN1BHntg>
