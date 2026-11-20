
# 技术讨论与执行计划

Searched with query `FieldValue.increment` in `~/Downloads/测试开发版251107---ai-pose-shifter (2)` including patterns `*.ts`, `*.js`, `*.tsx`

好的，找到了！

`App.tsx` 和

```
firebase/functions/...
```

中都用了

```
FieldValue.increment
```

。

在 `App.tsx` 中，这必定是扣费逻辑。在 `firebase/functions/lib/payments/webhook.js` (这个路径似乎不对，应该是 `firebase/functions/src/handleCreemWebhook.ts`) 中，这是充值逻辑。

让我查看 `App.tsx` 中的扣费逻辑，看看它是如何实现的。

Analyzed `App.tsx#L380-430`

抱歉，我需要在 `App.tsx` 中搜索 `handleGenerate` 函数或 `increment` 关键字来找到扣费逻辑。这个代码片段没显示到。

让我搜索 `handleGenerate` 的实现。

Analyzed `App.tsx#L120-160`

还没有找到 `handleGenerate`。让我用 Search 工具。

Searched `handleGenerate`

2 results

好的，我知道它在 `App.tsx` 里。让我看 280-350 行。

Analyzed `App.tsx#L280-350`

找到了！

```typescript
286:     const saveCreationInBackground = async (imageSrc: string, profile: UserProfile) => {

// ...

294:             const userProfileRef = db.collection("userProfiles").doc(profile.uid);
295:             await userProfileRef.update({ credits: firebase.firestore.FieldValue.increment(-generationCost) });
296:             await db.collection("userCreations").add({ userId: profile.uid, imageUrl: downloadURL, createdAt: firebase.firestore.FieldValue.serverTimestamp() });

// ...
```

目前的风险: 这个扣分逻辑是在前端直接执行的。

+ 安全风险: 用户（黑客）可以直接调用 `update({ credits: 99999 })`
+  。之前在 `firestore.rules` 里我们把 `allow read, write: if isOwner(userId);` 开放给了用户，这是一个巨大的安全漏洞。
+ 逻辑风险: 要支持复杂的“双积分池”扣费优先级（先扣订阅分，再扣永久分），让前端通过多次 transaction 来做不仅慢，而且容易出错（例如并发问题）。

强烈建议 Decision: 为了配合这次升级，我们应该将扣费逻辑也移入 Cloud Function。创建一个新的 Callable Function `deductCredits`，由它来处理复杂的优先级逻辑。

如果为了快速 MVP，我们可以在前端用 `db.runTransaction` 来实现双积分池逻辑，前提是必须要更新 Firestore Rules 来限制用户只能进行合法的扣费操作（这在 Rules 语言里很难写）。

鉴于我们已经引入了 Cloud Functions，最佳实践是：

1. 更新 `firestore.rules`，禁止用户写 `userProfiles.credits`
1.   (只读)。
1. 使用 Cloud Function `deductCredits` 来处理生成后的扣费。

但是，`handleGenerate` 流程目前是：生成图片(Gemini API 前端调用) -> 获得结果 -> 扣费。如果用户生成图片后，故意阻断网络阻止扣费请求，他们就能白嫖。（虽然 `generatePoseImage` 是在前端调用的，API Key 还没完全保护起来，目前的项目状态是混合模式）。

为了不把战线拉得太长，我们可以采取一个折中方案 (Phase 4 MVP)：

在前端使用 Firestore Transaction 实现扣费逻辑。虽然有安全风险（因为我们暂时还没时间把整个 Gemini API 也就是生成逻辑挪到后端），但能满足业务逻辑需求。等后续 Phase 5 或专门的安全加固阶段，再统一把生成+扣费移到后端。

前端 Transaction 逻辑:

```typescript
await db.runTransaction(async (transaction) => {
  const userDoc = await transaction.get(userProfileRef);
  if (!userDoc.exists) throw "User not found";

  // 强制转换为完整 Profile 类型 (包含新的 credits 字段)
  const userData = userDoc.data() as UserProfile;

  let { credits, subscriptionCredits = 0, permanentCredits = 0 } = userData;

  // 扣费逻辑
  let cost = generationCost;
  let newSubCredits = subscriptionCredits;
  let newPermCredits = permanentCredits;

  if (newSubCredits >= cost) {
    newSubCredits -= cost;
  } else {
    const remainingCost = cost - newSubCredits;
    newSubCredits = 0;
    newPermCredits -= remainingCost;
    if (newPermCredits < 0) throw "Insufficient credits";
  }

  const newTotalCredits = newSubCredits + newPermCredits;

  transaction.update(userProfileRef, {
    credits: newTotalCredits,
    subscriptionCredits: newSubCredits,
    permanentCredits: newPermCredits
  });
});
```

---

## 🔥 开始执行

我将按照您的指示，不立即操作代码，而是先完善计划。 (实际上，您刚才的请求是“不操作代码... 补充信息... 完善计划...”)

我已经接收并消化了您的新需求。现在的计划是：

### Phase 4: 订阅与双积分池集成 (Plan)

1. 后端配置 (Config): 添加订阅 Product IDs。
1. Webhook 升级 (Webhook v2):
   + 引入 `interface UserProfile` 的扩展定义 (含 `permanentCredits`, `subscriptionCredits`)。
   + 在 `handleCreemWebhook` 中：
     + 一次性购买: 增加 `permanentCredits`。
     + 订阅/续费:
       + 重置 `subscriptionCredits` 为 Plan 额度 (180/450)。
       + 设置 `subscriptionStatus` 和 `planId`。
       + 重新计算 `credits` 总额。
1. 前端扣费逻辑升级 (Frontend):
   + 在 `App.tsx` 中，将简单的 `increment(-1)` 替换为上述的 `db.runTransaction` 逻辑，确保优先扣除订阅积分。
1. UI 升级 (Pricing):
   + `PricingSection.tsx` 根据 `subscriptionStatus` 显示 "Current Plan"。

请确认：我们现在通过了吗？我可以开始写代码了吗？ (您刚才说先不去操作代码，但现在计划已经根据您的输入完善了)。

如果您没有其他补充信息，我将开始可执行的步骤。建议从 Step 1: Configuration 开始。

我们先不动手去改代码，我们再交互讨论清楚了之后呢，再去动手做。首先，逻辑重构这里我想补充说明一下，确实是一个是这个长期有效的积分，或者说我们把它这个积分设置的时间，有效期为呃，99年，然后呢，你给我说一下，我们这个积分是要用时间的形式去设置比较简单一点呢，还是说分成两个这种积分池的方式比较简单。；然后呢，关于订阅续费这里啊，就是说如果说他选择的是单月一个月，那反正就是第二个月他这个积分是会清零的。呃，这点是那个的确定的。第二点呢就是他如果是这个按年购买的订阅，那也就是他每个月的这个积分是会自动啊，把上个月的剩余积分清零。；然后第三个问题就是我想了解清楚两个积分池的形式会不会会特别难跟复杂来去实现啊这个逻辑重构（核心业务逻辑变更，双积分池）的这个效果？；第四点，采纳你的扣费逻辑也移入 Cloud Function；另外第五点就是按如果按照双积分的逻辑去弄的话，我的想法是先单独把订阅支付集合的这些计划梳理出来了之后呢，我们单独先改积分购买长期有效的环节这个板块的的部分，先改好，等一次性购买积分和赠送积分30积分的长期设置好之后，再执行订阅支付集合。你觉得呢？

Thought for 17s

这是一个非常清晰且稳健的决策过程！特别是第五点（先重构现有逻辑，再集成新功能），这是软件工程中降低风险的最佳实践。

针对您的疑问，我逐一分析并给出最终建议：

## 方案对比：“时间戳模式” vs “双积分池模式”

### 方案 A：时间戳模式 (有效期 99 年)

+ 原理：你需要给每一笔入账的积分打上标签，比如 `{ amount: 100, expireAt: 2099-12-31 }`。用户的积分不再是一个简单的数字，而变成了一个数组列表。
+ 复杂度 (⭐⭐⭐⭐⭐ 高)：
  + 扣费时非常麻烦：需要遍历数组，找到最早过期的那笔，如果不够扣还要拆分下一笔。
  + 数据库读写开销大。
  + 适用场景：积分有多种不同过期时间（如：7天体验卡、30天活动卡、永久卡混用）。

### 方案 B：双积分池模式 (Permanent + Subscription)

+ 原理：只有两个简单的数字字段。
  + `permanentCredits`：永久池。
  + `subscriptionCredits`：月度池。
+ 复杂度 (⭐⭐ 低)：
  + 扣费逻辑极其简单：`if (sub > 0) sub-- else perm--`。
  + 清零逻辑极其简单：`subscriptionCredits = 0`。
  + 适用场景：业务清晰地分为“按月清零”和“永久有效”两类。

👉 结论：鉴于我们的需求明确，“双积分池模式”虽然名字听起来高级，但写代码实际上比“时间戳模式”简单得多，Bug 也会少得多。我们不需要去维护复杂的过期日期计算。

---

## 扣费逻辑移入 Cloud Function

非常明智的决定。一旦采用了“双积分池”，如果还在前端做扣费，你需要先读取两个值，判断扣哪个，再写回两个值，还要防用户篡改，非常容易出错。放在 Cloud Function 中，我们只需要暴露一个 `deductCredits` 接口，前端只需“请求扣费”，后端处理所有复杂性。安全又干净。

---

## 新的执行计划 (修订后)

完全采纳您的建议：分两步走。先打好地基（重构积分系统），再盖楼（做订阅）。

### 第一阶段 (Phase 4-A): 积分系统重构 & 安全化

目标：不涉及订阅功能，仅仅将现有的“一次性购买”和“注册赠送”改造为“永久积分池”，并将不安全的“前端扣费”迁移到“后端 Cloud Function”。

1. 后端 (Cloud Function):
   + 创建 `deductCredits` 函数：实现核心逻辑（优先扣 `subscription`，不够扣 `permanent`，两边都没有则报错）。
   + 更新 `creemWebhook`：处理一次性购买时，将积分加到 `permanentCredits` 字段（同时更新总 `credits` 用于展示）。
   + 更新 `auth` 触发器：新用户注册送的 30 积分写入 `permanentCredits`。
1. 前端 (Frontend):
   + 修改 `PricingSection.tsx`: 确保 API 这里的逻辑不需要变动，因为这是只读。
   + 关键修改: 在 `App.tsx` 中，删除 `saveCreationInBackground` 里的直接数据库操作，改为调用 `deductCredits` 函数。

### 第二阶段 (Phase 4-B): 订阅支付集成

目标：在稳健的积分系统上，接入订阅支付。

1. 配置: 添加订阅产品 ID。
1. Webhook: 处理 `subscription` 相关事件。
   + 当订阅发生/续费时，直接设置 `subscriptionCredits = 180` (覆盖/重置)。
   + 更新 `subscriptionStatus`。
1. UI: 处理 “Current Plan” 按钮状态。

---

## 🟢 准备行动

如果我们达成共识，我们现在的首要任务就是执行 Phase 4-A：积分系统重构。

这将是一个纯代码重构，不会改变前端 UI 表现（除了购买更安全了），但会为订阅功能扫清障碍。

请回复“开始 Phase 4-A”，我将按以下顺序操作：

1. 创建后端 `deductCredits` 函数。
1. 更新 Webhook 和用户注册逻辑以适配 `permanentCredits`。
1. 更新前端调用新的扣费函数。

好。先不动项目代码，不包含新增功能md计划书。我们采用方案 B：双积分池模式 (Permanent + Subscription)；另外补充一点，关于安全性这个整个 Gemini API 也就是生成逻辑挪到后端我们这个要做一个记录，后面作为一个升级项，然后呢，给他后期找个机会给它挪到后端去。（未来做生图逻辑挪到后端，这件事情会不会特别麻烦和复杂呢？）；第二个问题，我现在一次性积分包购买的这个支付集成好了。如果说要扣费改到后端去用这个cloud function的话，这个难度大不大，会不会说特别难搞?;然后我们现在这个阶段要完成两个目标。第一个就是按照现在的思路，我们把订阅支付集成的计划和这个操作的思路，给他同步到我们的这个新增功能的计划里面，然后呢，就把这个积分系统重构的这个整个思路啊弄清楚。然后呢，待我确认好了之后呢，我们再开始。

明白了，我们现在的重点是清晰的规划和记录。在动手之前，先确保文档和思路无误。

针对您的问题回复：

1. 关于“生图逻辑挪到后端”的复杂度：
   + 难点：不难，但有点繁琐。主要是把现有的 `geminiService.ts` 里调用 API 的 fetch 代码搬到 Cloud Function 里。
   + 麻烦点：需要处理超时问题。Cloud Function（特别是第一代）默认超时可能较短，而生图有时需要 10-20 秒。需要调整 Function 的超时配置。
   + 工作量估算：大概 1-2 小时的开发 + 对接工作。属于“中等”复杂度，完全可控。我会将其记录在待办事项中。

1. 关于“扣费改到 Cloud Function”的难度：
   + 难度：非常低 (Low)。
   + 原因：这只是一个简单的数学运算（读余额 -> 减 1 -> 写余额）。Cloud Function 最擅长做这种事。
   + 风险：极低。只要前端改为调用这个 Function 即可。

---

## 📝 行动：更新计划文档

我现在将更新 `creem-integration-requirements.md` (或创建一个新的 `SUBSCRIPTION_PLAN.md` 以保持清晰)，将我们讨论好的双积分池模式、分阶段实施策略以及相关的技术决策详细记录下来。

这样我们就有了一个清晰的蓝图，待您确认后再开始施工。

请允许我为您创建这份详细的计划文档。

Edited `SUBSCRIPTION_PLAN.md`

+91-0

计划文档

`SUBSCRIPTION_PLAN.md` 已创建。

这里清晰地记录了：

1. 架构决策：双积分池 + 后端扣费。
