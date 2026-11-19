
# 2）网页多项细节优化

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/DTzodcGmboV4HnxhLoYcrGPgnCh](https://xatc0v8uz5m.feishu.cn/docx/DTzodcGmboV4HnxhLoYcrGPgnCh)

## 退订订阅的设置

## 生图逻辑改后端

Thought for 13s

好的，这是为您精心准备的《生图逻辑后端迁移与安全加固实施方案》。

这份文档详细梳理了从前端架构向后端架构迁移的每一个细节，特别是着重分析了积分扣减与生图服务的原子性绑定，以确保系统的资金安全和业务逻辑的闭环。

请您仔细审阅。

---

## 📂 项目架构升级计划书：生图逻辑与 API Key 后端迁移

**文档版本**: v1.0 **制定日期**: 2025-12-19 **目标**: 将核心 AI 生成逻辑与敏感 API Key 从前端迁移至 Firebase Cloud Functions，实现零信任安全架构。

---

### 核心变更概述 (Executive Summary)

当前架构中，浏览器直接持有 Google Gemini API Key 并发起生图请求，存在 Key 泄露风险及积分验证绕过风险。本计划将把这一过程封装为后端原子操作。

#### 架构对比

---

### 详细影响范围与关联系统 (System Impact)

此变更不仅仅是代码位置的移动，它深刻关联到积分系统和前端交互流程。

#### 🔴 2.1 积分/支付系统关联 (Critical)

+   **现状**: 前端先调用 `deductCredits`，成功后再调用 Gemini。

+   风险: 用户如果懂技术，可以拦截 `deductCredits` 请求直接运行 Gemini 逻辑（因为 Key 在前端）。或者 `deductCredits` 成功但生图失败，导致用户通过客服投诉退款。

+   **变更**: 废弃前端单独调用 `deductCredits` 的逻辑。

+   新函数

+   `generatePoseImage` 将包含 "检查积分 -> 扣除积分 -> 调用 AI -> 失败则退还积分" 的完整闭环。

+   **万无一失策略**: 只有 AI 成功返回了图片数据，交易才算最终完成（或在后端捕获错误时执行补偿性加分）。

#### 🟠 2.2 存储系统关联 (Storage)

+   **现状**: 前端生图 -> 拿到 Base64 -> 上传到 Firebase Storage -> 写入 Firestore。

+   **变更**: 保持不变（建议）。

+   为了降低迁移复杂度，后端生图后将以 Base64 字符串返回给前端。前端继续复用现有的 "上传 SDK + 写入数据库" 逻辑。

+   注: Firebase Function 返回包体最大支持 10MB，通常生图结果在 2-4MB 左右，此方案可行且改动最小。

#### 🟡 2.3 网络与时延 (Latency)

+   **超时配置**: Gemini 生图耗时约 20-30s。Firebase Functions 默认超时为 60s。

+   **配置变更**: 需要将新函数的 `timeoutSeconds` 显式设置为 **300s (5分钟)** 以防止网络波动导致的截断。

---

### 实施步骤规划 (Implementation Plan)

#### 第一阶段：后端环境准备 (Backend Setup)

1.  **依赖安装**:

+   在 `firebase/functions` 目录下安装 Google GenAI SDK。

+   Command: `npm install @google/genai`

1.  密钥配置 (Secret Management):

+   将 `VITE_GEMINI_API_KEY` 的值写入 Firebase Secrets。

+   Command: `firebase functions:secrets:set GEMINI_API_KEY`

1.  **创建新云函数文件**:

+   路径: `firebase/functions/src/generateImage.ts`

+   内容: 迁移原前端 `geminiService.ts` 中的 `generatePoseImage` 和 `generatePoseDescription` 逻辑，并融合 `deductCredits` 的事务逻辑。

#### 第二阶段：后端逻辑开发 (Core Logic Implementation)

在 `generateImage.ts` 中实现以下原子流程：

1.  **身份校验**: 检查 `context.auth`，确保用户已登录。

1.  事务开启 (Firestore Transaction):

+   读取用户 `userProfile`。

+   检查 `permanentCredits` 和 `subscriptionCredits`。

+   如果余额不足，直接抛出 `Error`，中止 AI 调用。

+   执行预扣费（优先扣除订阅积分）。

1.  执行生图 (AI Execution):

+   使用 `process.env.GEMINI_API_KEY` 初始化 AI 客户端。

+   调用 Gemini 模型进行生图。

1.  异常回滚 (Error Handling):

+   `try-catch` 包裹 AI 调用。

+   如果 AI 报错（如 Content Filters 拦截），触发自动退款逻辑（将刚才预扣的积分加回去）。

+   向前端返回明确的错误信息。

1.  **返回结果**: 返回生成的 Base64 图片数据。

#### 第三阶段：前端重构 (Frontend Refactor)

1.  **清理代码**:

+   删除 `services/geminiService.ts` 中涉及 Key 和直接调用的代码。

+   删除 `.env` 文件中的 `VITE_GEMINI_API_KEY`（或将其留空）。

1.  **更新 `App.tsx`**:

+   将 `import { generatePoseImage } ...` 替换为 Firebase `httpsCallable` 调用。

+   移除 `handleGenerate` 函数中原本的 `deductCredits` 调用代码（因为后端已经包办了）。

+   更新 Loading 状态的提示文案（例如：从 "Deducting credits..." 统一合并为 "Processing..."）。

#### 第四阶段：测试与部署 (Testing & Deployment)

1.  **本地模拟器测试**:

+   使用 `firebase emulators:start`。

+   验证：余额不足是否报错？生图失败是否不扣分？生图成功是否正常扣分？

1.  **部署云函数**:

+   `firebase deploy --only functions:generateImage`

1.  **前端上线**:

+   `npm run build && firebase deploy`

---

### 风险控制与回滚方案 (Risk & Rollback)

---

### 待确认事项 (Checklist)

在开始执行代码修改前，请您确认：

1.   **确认同意**将“扣费”逻辑从前端剥离，完全交由后端函数内部原子化执行。

1.   **确认接受**生图等待时间可能增加 2-4 秒（网络传输开销）。

1.   **确认**您在 Firebase 控制台拥有设置 Secrets 的权限。

如果您对上述原子性扣费逻辑或架构调整无异议，我将按照此手册开始编写代码。这一步将是项目从“演示版”走向“商业版”的关键里程碑。

很好，先不执行。这个计划以 `md` 格式文件保存到项目中。然后同时再考虑整个项目的可行性和思路，再对所需要调整和修改的细节进行补充优化、以及每个阶段测试方式和成功的标志清单、涉及到我需要人为操作测试和修改的部分，也一并再计划中体现。；待我确认后，才能执行下一步

Edited

`MIGRATION_PLAN_BACKEND.md`

+167-0

已为您创建 Detailed Migration Plan 文档。

您可以随时在项目中查看：

`MIGRATION_PLAN_BACKEND.md`

### 下一步建议

这是一个涉及资金逻辑的重大重构，我建议您仔细读该计划书，特别是“第二阶段（事务逻辑）”和“第四阶段（测试清单）”。

如果您确认该计划书中涵盖的所有细节（包括预扣费、失败回滚、超时设置）都符合您的业务预期，请回复 “确认执行”，我将按照计划书的步骤开始为您进行后端环境的配置和代码迁移。

Recent actions

先不执行，在现有的计划书中，重新梳理一遍，查看是否有需要我进行决策的关键点和建议，如有请列出来给我。再和我进一步的讨论中，将这次更新计划进一步的优化

明白，谨慎决策是明智的，因为涉及到底层逻辑和资金交易。

根据我重新梳理的《生图逻辑后端迁移计划 (V1.1)》，为您提炼出以下 3 个需要您进行决策或提供方向的关键点，这些点会直接影响用户的体验和系统的复杂程度：

### 🛑 关键决策点 1：扣费时机策略（Pre-charge vs. Post-charge）

您希望采用哪种扣费模式？

+   方案 A：先扣费，失败回退（当前计划书推荐）

+   流程：用户点击生成 -> 扣除1积分 -> 开始生图 -> (如果失败) -> 积分+1退回。

+   优点：杜绝高并发“刷单”（防止用户在 0.1秒内点 10 次，利用延迟白嫖）。

+   缺点：逻辑稍复杂（需要写回滚代码）；用户可能会看到积分短暂闪烁（变少又变多）。

+   **方案 B：生图成功后，再扣费**

+   流程：用户点击生成 -> 检查有余额 -> 开始生图 -> (生成成功) -> 扣除1积分。

+   优点：逻辑简单，体验更顺滑（只有拿到图才扣钱）。

+   缺点：存在并发漏洞（Race Condition）。如果用户用脚本同时发 10 个请求，可能在还没来得及扣费前，10 张图都已经开始生成了（从而白嫖）。

+   我的建议：考虑到 AI 生成成本较高，推荐方案 A（严谨优先）。

### 🛑 关键决策点 2：超时与“长连接”的处理

目前 Gemini 生图可能需要 20-30 秒，这对 HTTP 连接是个挑战。

+   **当前计划**：简单粗暴地将超时时间设为 **300秒 (5分钟)**，让前端傻等。

+   风险：如果用户手机切屏、网络波动，连接断了，后端图生成好了（钱也扣了），但前端没收到图。这会导致客诉。

+   优化方案 (Result Polling)：（需要额外工作量）

1.  后端接到请求，立刻返回一个 `task_id`，状态为 `processing`。

1.  后端异步去跑 Gemini，跑完把图存数据库。

1.  前端每隔 3 秒轮询：“`task_id` 好了没？”

+   优点：极其稳定，断网重连也能拿到图。

+   缺点：开发工作量增加 50%+。

+   我的建议：先保持“简单长连接” (MVP)，如果上线后发现断连客诉多，再升级为“异步轮询”。

### 🛑 关键决策点 3：隐私与日志记录

为了排查问题（Debug），后端通常会记录日志。

+   **决策**：您是否允许在 Google Cloud 日志中记录用户的 Prompt（提示词） 甚至 图片内容？

+   选项 A：全面记录。方便该 bug，但可能涉及隐私合规（如果用户上传了私密照片）。

+   选项 B：仅记录状态。只记录“用户UID:xxx 生图成功/失败”，不记录具体内容。

+   我的建议：选项 B（最小化记录）。出于隐私合规（尤其您注重 Privacy），建议只记录元数据，不记录内容数据。

---

### 给您的优化建议：

1.  **增加“每日免费额度”逻辑**:

+   目前逻辑是单纯扣积分。

+   建议：借此机会，在后端 `generateImage` 函数中加入：“如果今天是新的一天，先自动重置免费积分，再进行扣费检查”。这样可以把前端零散的逻辑收拢。

1.  **强制版本更新机制**:

+   迁移后，旧版前端（还拿着 Key 直连的那些缓存代码）如果不刷新页面，可能会报错或继续走旧逻辑。

+   建议：迁移上线后，最好将旧的 Key 废弃/重置，强制所有这就流量走新逻辑（或者旧逻辑直接报错失效）。

您对以上 3 个决策点有什么偏好？ 确认这些后，我会优化计划书，使其更完美。

Thought for 10s

1.

1.

1.

1.

1.

Edited

`generateWithAI.ts`

+1-1

```
~/…/测试开发版251107---ai-pose-shifter (2) $ cd firebase/functions && npm run build

> build

> tsc

Ran terminal commandOpen Terminal·Exit code 0

Always Proceed

~/…/测试开发版251107---ai-pose-shifter (2) $ firebase deploy --only functions:generateWithAI

=== Deploying to 'copypose-7f3b2'...

i  deploying functions

Running command: npm --prefix "$RESOURCE_DIR" run build

> build

> tsc

✔  functions: Finished running predeploy script.

i  functions: preparing codebase default for deployment

i  functions: ensuring required API cloudfunctions.googleapis.com is enabled...

i  functions: ensuring required API cloudbuild.googleapis.com is enabled...

i  artifactregistry: ensuring required API artifactregistry.googleapis.com is enabled...

⚠  functions: package.json indicates an outdated version of firebase-functions. Please upgrade using npm install --save firebase-functions@latest in your functions directory.

i  functions: Loading and analyzing source code for codebase default to determine what to deploy

Serving at port 8068

i  extensions: ensuring required API firebaseextensions.googleapis.com is enabled...

i  functions: preparing firebase/functions directory for uploading...

i  functions: packaged /Users/maxecho/Downloads/测试开发版251107---ai-pose-shifter (2)/firebase/functions (121.08 KB) for uploading

i  functions: ensuring required API cloudscheduler.googleapis.com is enabled...

i  functions: ensuring required API eventarc.googleapis.com is enabled...

i  functions: ensuring required API pubsub.googleapis.com is enabled...

i  functions: ensuring required API storage.googleapis.com is enabled...

i  functions: generating the service identity for pubsub.googleapis.com...

i  functions: generating the service identity for eventarc.googleapis.com...

✔  functions: firebase/functions source uploaded successfully

i  functions: updating Node.js 20 (2nd Gen) function generateWithAI(us-central1)...

✔  functions[generateWithAI(us-central1)] Successful update operation.

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/copypose-7f3b2/overview

╭───────────────────────────────────────────────────────────────╮
```



Update available 14.25.0 → 15.1.0

To update to the latest version using `npm`, run:

`npm install -g firebase-tools`

For other CLI management options, visit the CLI documentation.

Ran background terminal command Open Terminal · Exit code 0

Always Proceed

Checked command status

Checked command status

Checked command status

Edited

`PRODUCTION_TEST_GUIDE.md`

+221-0

完成！我已经为您完成以下工作：

## ✅ 已完成

### 1. 超时配置优化

+   ✅ 将 `generateWithAI` 函数的超时时间从 120 秒调整为 300 秒

+   ✅ 已重新编译并部署到生产环境

+   ✅ 部署成功确认

### 2. 生产测试指南

已为您创建详细的测试文档：

`PRODUCTION_TEST_GUIDE.md`

包含：

+   **5 个核心测试用例**（余额不足、正常扣费、混合积分、失败场景、超时容错）

+   **3 种测试账号配置方案**

+   **详细的操作步骤**和**预期结果**

+   **数据验证方法**（前端 + `Firestore` 双重验证）

+   **常见问题排查指南**

---

## 🎯 下一步：开始测试

请您现在：

1. **访问生产环境**：https://poseshift.mossecho.cn

1. **准备测试账号**：

+   账号 A：0-2 积分（测余额不足）

+   账号 B：10 永久积分（测正常扣费）

+   账号 C：2 订阅分 + 10 永久分（测混合扣费）

1. 按照 `PRODUCTION_TEST_GUIDE.md` 逐一测试

1. **记录结果**并向我汇报：

+   ✅ 哪些测试通过了

+   ❌ 哪些测试失败了（附上截图/错误信息）

+   📝 任何您觉得体验不好的地方

我会根据您的测试反馈做最后的优化调整！
