
# 中集）Creem支付记录集合订阅支付

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/EGD8dgVIbo11vNxjX1vc3POSned](https://xatc0v8uz5m.feishu.cn/docx/EGD8dgVIbo11vNxjX1vc3POSned)

github项目的地址`https://github.com/maxecho20/251118poseshift.git`。我看到项目中没有更新，请检查部署情况。；接下来我们要开始做订阅支付的支付集成，请你理清整个的操作流程计划，参考和复制前面我们做积分包购买的成功经验。先梳理`@ @creem-api.md`中查看是否未完成的部分和排除掉多余信息。然后更新出新的集合订阅支付的集成的计划方案和操作步骤和思路，先谋定而后动

![](img/cbf01aeafbb7fd37471636706c7ecd71.png)

![](img/979440153df640267e21329a4f8a6662.png)

Thought for 10s

```
~/…/测试开发版251107---ai-pose-shifter (2) $ git push origin main

枚举对象中: 53, 完成.

对象计数中: 100% (53/53), 完成.

使用 10 个线程进行压缩

压缩对象中: 100% (34/34), 完成.

写入对象中: 100% (37/37), 26.06 KiB | 13.03 MiB/s, 完成.

总共 37（差异 10），复用 0（差异 0），包复用 0（来自  0 个包）

remote: Resolving deltas: 100% (10/10), completed with 9 local objects.

To https://github.com/maxecho20/251118poseshift.git

de57e53..3874be3  main -> main

Ran terminal commandGo to Terminal·Exit code 0

Off
```

Thought for <1s

Analyzed

`creem-api.md#L1-588`

Thought for 1s

Analyzed

`Creem Integration Requirements.md`

Thought for 13s

这是一份详细的订阅支付集成计划，基于我们之前在一次性积分包上的成功经验，并针对订阅模式的特殊性进行了调整。

## 🚀 订阅支付集成计划 (Phase 4)

目标: 实现 Basic 和 Pro 计划的月付/年付订阅功能，确保用户订阅后能即时获得积分，且状态能在前端正确显示。

### 核心逻辑与架构 (复用成功经验)

我们将继续使用 Cloud Functions + Webhook 的架构，这已经被证明是安全且可靠的。

+   发起支付: 复用现有的 `createCheckout` 函数。Creem 的 Checkout API 对一次性商品和订阅商品是通用的，只需要传入不同的 `productId`。

+   处理回调: 扩展 `creemWebhook` 函数。这是本次工作的重点。我们需要处理订阅特有的 Webhook 事件，而不仅仅是 `checkout.completed`。

### 实现步骤详解

#### Step 1: 后端配置更新 (Config Update)

+   操作: 在 `firebase/functions/src/config.ts` 中：

+   添加 4 个新的订阅 Product IDs (`BASIC_MONTHLY`、`BASIC_YEARLY`、`PRO_MONTHLY`、`PRO_YEARLY`)。

+   更新 `PRODUCT_CREDITS_MAP`，将这些 ID 映射到对应的积分数量 (例如 Basic Monthly -> 180 credits)。

+   新建 `SUBSCRIPTION_PLANS_MAP`，用于存储每个 ID 对应的计划详情 (如 plan name, interval)，方便在更新 User Profile 时使用。

#### Step 2: Webhook 逻辑扩展 (Webhook Logic)

+   挑战: 订阅涉及多个事件（创建、续费、取消）。

+   策略:

+   监听 `checkout.completed` : 继续作为主要入口。当 `mode` 为 `subscription` 时，除了发放积分，还需要提取 `subscription_id`。

+   处理续费: Creem 文档提到 `subscription.updated` 或类似的周期性扣款事件。我们需要确认并处理这个事件，以便在下个月自动扣款成功时再次发放积分。

+   更新 User Profile: 在 `userProfiles` 集合中新增字段：

+   `subscriptionId` : Creem 的订阅 ID。

+   `subscriptionStatus` : 'active' | 'calend' | 'past_due'。

+   `planId` : 当前订阅的产品 ID。

+   `currentPeriodEnd` : 订阅到期时间。

#### Step 3: 前端 UI 适配 (Frontend Integration)

+   `PricingSection.tsx`:

+   为 Basic 和 Pro 计划的 "Subscribe" 按钮绑定 `handleCheckout` 事件。

+   智能状态显示: 检查 `userProfile.subscriptionStatus`。如果用户已经是 "active" 状态，原来的 "Subscribe" 按钮应变为 "Current Plan" 或 "Manage Subscription" (跳转到 Billing 页)，防止重复订阅。

+   `AuthContext.tsx`: 确保 `UserProfile` 类型定义包含新增的订阅字段。

#### Step 4: 测试与验证 (Test Plan)

+   测试场景 1 (新订阅): 购买 Basic Monthly，验证 Firestore 中 `credits` 增加 180，且 `subscriptionStatus` 变为 `active`。

+   测试场景 2 (交易记录): 验证 `Transaction History` 中显示 "Subscription started" 或类似描述。

+   测试场景 3 (防止重复): 订阅后再次尝试点击订阅按钮，应被阻止或提示。

### 待确认事项

+   Credits 累积规则: 订阅的积分是每月累加（上月没用完这月继续用），还是每月重置？(目前代码逻辑也是累加 `FieldValue.increment`，这通常是用户更喜欢的模式)。

+   Webhook 事件细节: 我们需要通过实际测试来确认 Creem 在续费成功时发送的具体 Webhook Payload 结构。

---

是否同意执行此计划？ 如果同意，我将从 Step 1: 后端配置更新 开始。

很好，我们先不去开始操作代码，我需要补充几个信息给你，然后呢，你再去完善一下我们的计划跟需要执行的这个方案。第一个呢就是呃我们的一次性的这种订阅的积分包啊，购买了之后呢，他这个积分是不会过期的。你可以设置一个呃长期有效的一个方式。你看看哪种方式最方便。比如说99年有效这样子的积分，然后呢，关于订阅的这个积分，它是按月清零的。就是他比如说他购买了呃，基础版的月度的积分包。那它每个月所赠送180的积分呢是会自动清零的。而新注册用户它原有赠送的啊，比如说30积分，没有使用的话，这种部分积分就是不会过期的啊，这个信息需要补充给到你。；2、在首页上的点击订阅的按钮都已经设置好了`upgrade`按钮了。如果当用户已经订阅了这个时候，按钮可变成【Current Plan】防止重复订阅。

![](img/7c6a54d56cd7c47c280dcc08a91b90cf.png)

Thought for 21s

感谢您补充这些关键的业务逻辑！这对我们的后端设计至关重要。

## 🔄 订阅支付集成计划 (修订版 - Phase 4)

根据您的新需求（积分池分离、优先扣除过期积分、UI 状态感知），我们需要对数据结构和扣费逻辑进行升级。

### 核心业务逻辑变更

为了实现“订阅积分按月清零”且“购买积分/赠送积分永久有效”，我们不能再只维护一个简单的 `credits` 总数。我们需要在 Firestore 的 `userProfiles` 中引入双积分池模型：

`permanentCredits` (永久积分池): 存放注册赠送 (30) + 一次性购买 (100/335)。永不过期。

`subscriptionCredits` (订阅积分池): 存放订阅获赠 (180/450)。每月续费时重置。

`credits` (总展示积分): `permanentCredits` + `subscriptionCredits`。用于前端展示和判断是否允许生成。

扣费优先级: 用户生成图片消耗积分时，优先扣除 `subscriptionCredits`，扣完后再扣除 `permanentCredits`。

### 执行步骤详解

#### Step 1: 先决条件检查与代码定位

+   行动: 我需要先找到目前“扣除积分”的代码位置。`README` 中提到是 `saveCreationInBackground`，我猜测它在 `App.tsx` 或 `services/geminiService.ts` 中。如果目前是前端直接扣费（`FieldValue.increment(-1)`），通过引入双积分池逻辑，我们可能需要将其改为调用 Cloud Function 或者在前端写更复杂的事务逻辑（为了安全推荐 Cloud Function，但为了 MVP 保持前端逻辑也可以，只要逻辑正确）。

#### Step 2: 后端配置 (Config Update)

+   行动: 在 `firebase/functions/src/config.ts` 中配置：

+   添加订阅 Product IDs (`BASIC_MONTHLY` 等)。

+   在 `PRODUCT_CREDITS_MAP` 中添加映射。

#### Step 3: Webhook 逻辑重构 (Smart Logic)

+   行动: 修改 `firebase/functions/src/handleCreemWebhook.ts`：

+   处理一次性购买: 增加 `permanentCredits`，更新总 `credits`。

+   处理新订阅: 设置 `subscriptionCredits` = 180，更新 `subscriptionStatus` = 'active'，记录 `planId`。

+   处理订阅续费 (`invoice.paid`/`subscription.updated`): 重置 `subscriptionCredits` = 180 (不仅是增加，因为旧的要清零/覆盖，或者理解为补齐？通常是重置为当月额度)。注：如果不清零而是累积，那就是 `increment`；如果要“按月清零”，其实就是每月续费时把这个字段设为固定值 180，不管之前剩多少。

#### Step 4: 扣费逻辑升级 (Consumption Logic)

+   行动: 修改图片生成后的扣费代码：

```typescript
if (user.subscriptionCredits > 0) {
    update({ subscriptionCredits: increment(-1), credits: increment(-1) })
} else {
    update({ permanentCredits: increment(-1), credits: increment(-1) })
}
```

#### Step 5: 前端 UI 智能感知 (Smart UI)

+   行动: 修改 `components/PricingSection.tsx`:

+   引入 `userProfile`。

+   逻辑：

```typescript
const isCurrentPlan = userProfile?.subscriptionStatus === 'active' && userProfile?.planId === product.id;
```

+   渲染：如果是 `isCurrentPlan`，按钮显示 "Current Plan" 并禁用；否则显示 "Upgrade" (或 "Switch" 如果我们支持换绑)。

---

### 🟢 准备开始

我将首先检查目前的扣费逻辑在哪里，以便评估 Step 4 的工作量。

请稍等，我先定位代码。

```
~/…/测试开发版251107---ai-pose-shifter (2) $ grep -r "increment" .

<truncated 1 lines>

./firebase/functions/node_modules/@firebase/database-types/index.d.ts:  increment(delta: number): Object;

./firebase/functions/node_modules/google-gax/node_modules/uuid/CHANGELOG.md:- remo[ve mist](https://github.com/uuidjs/uuid/commit/09fa824)akenly added typescript dependency, rollback version (standard-version will auto-increment) (09fa824)

./firebase/functions/node_modules/google-gax/build/protos/google/api/servicecontrol/v1/quota_controller.proto:  // 1. Per quota group or per quota metric incremental usage will be specified

./firebase/functions/node_modules/teeny-request/node_modules/uuid/CHANGELOG.md:- remo[ve mist](https://github.com/uuidjs/uuid/commit/09fa824)akenly added typescript dependency, rollback version (standard-version will auto-increment) (09fa824)

./firebase/functions/node_modules/@grpc/grpc-js/build/src/transport.js:                            session.incrementWindowSize(delta);

./firebase/functions/node_modules/@grpc/grpc-js/src/transport.ts:            if (delta > 0) (session as any).incrementWindowSize(delta);

./firebase/functions/lib/payments/webhook.js:            credits: admin.firestore.FieldValue.increment(creditsToAdd)

./firebase/functions/lib/payments/webhook.js:                credits: admin.firestore.FieldValue.increment(packageInfo.credits)

./openspec/payment_spec.md:     - Update userProfiles/{userId} -> increment credits by order.credits.

./openspec/project.md:- Credit Management: Atomic operations using FieldValue.increment()
```

Ran background terminal commandGo to Terminal·Exit code 0

Off

这个输出太长且包含大量 `node_modules` 里的内容。我应该缩小搜索范围，只搜索 `firebase` 目录或 `App.tsx` 等源码文件。

让我重新搜索。

`SearchedFieldValue.increment`

3 results
