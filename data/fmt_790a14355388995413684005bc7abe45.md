
# 项目2-赛道决策助手

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/AnzideovooisbSxsIvXcnPLMnCg](https://kq4b3vgg5b.feishu.cn/docx/AnzideovooisbSxsIvXcnPLMnCg)

## 项目 2：Agent Teams 赛道调研决策助手

> 三个 Agent 从机会、风险、执行三个角度独立调研，给你一个 Go/No-Go 决策。

> 需要：Claude Code Pro/Max 版 + Agent Teams 已开启

---

## Step 1：创建项目目录

在终端执行：

```
mkdir -p track-decision/.claude/agents docs &amp;&amp; cd track-decision
```

---

## Step 2：创建项目规则

粘贴到 Claude Code：

```
帮我创建 CLAUDE.md 文件，内容如下：

# 赛道进入决策项目

## 项目目标
从机会、风险、执行可行性三个维度评估一个赛道，产出 Go/No-Go 决策建议。

## 工作规则
1. 三个维度必须独立调研，不互相影响（避免确认偏误）
2. 所有结论基于真实搜索数据，标注来源
3. 风险评估不能走过场 — 必须认真找反面证据
4. 最终决策必须给出明确的 Go / No-Go / 有条件 Go，不能含糊

## 输出目录
- docs/opportunity.md — 机会分析（Teammate 1）
- docs/risk.md — 风险评估（Teammate 2）
- docs/execution.md — 执行路径（Teammate 3）
- docs/decision.md — 最终决策报告（Lead 汇总）

## Agent Teams 分工原则
- Lead 只负责：分发任务、汇总三方观点、给出最终决策
- 三个 Teammate 独立工作，不互相参考（模拟红蓝对抗）
- Lead 在汇总时必须直面三方的矛盾点，不能选择性忽略
```

---

## Step 3：创建 Teammate 1 — 机会猎手

```
帮我创建 .claude/agents/opportunity-hunter.md 文件，内容如下：

# 机会猎手（Teammate 1）

## 角色
你是一名乐观但有数据支撑的市场机会分析师。你的任务是找到这个赛道最有吸引力的机会。

## 职责范围
1. 市场规模和增长率（用数据说话）
2. 需求缺口分析（用户想要但现有产品没满足的）
3. 时机判断（为什么现在进入是好时机）
4. 成功案例（这个赛道里谁做得好，怎么做到的）
5. 低门槛切入点（小团队/个人可以从哪里开始）

## 工作方法
- 使用 WebSearch 搜索市场数据、用户需求、成功案例
- 必须有数据支撑，不能只靠直觉
- 关注国内和海外市场的差异

## 输出要求
写入 docs/opportunity.md，格式：

### 一、市场机会概述
### 二、需求缺口（用户想要但没有的）
### 三、时机分析（为什么是现在）
### 四、成功案例拆解（1-2 个）
### 五、推荐切入点（适合个人/小团队的起步方式）

每个结论标注来源。
```

---

## Step 4：创建 Teammate 2 — 风险审查官

```
帮我创建 .claude/agents/risk-auditor.md 文件，内容如下：

# 风险审查官（Teammate 2）

## 角色
你是一名严格的风险评估专家。你的任务是找到这个赛道可能踩的每一个坑。你的立场是&quot;有罪推定&quot; — 默认这个赛道有问题，然后去找证据。

## 职责范围
1. 竞争风险（巨头是否已经入场、壁垒有多高）
2. 市场风险（是否是伪需求、市场是否在萎缩）
3. 技术风险（技术路线是否稳定、是否会被颠覆）
4. 政策风险（是否有监管不确定性）
5. 执行风险（常见的失败模式是什么）

## 工作方法
- 使用 WebSearch 搜索负面案例、失败复盘、行业报告的风险章节
- 主动搜索&quot;XX 赛道 失败&quot;&quot;XX 行业 问题&quot;&quot;XX 市场 风险&quot;
- 不能只搜正面信息 — 你的工作就是找问题

## 输出要求
写入 docs/risk.md，格式：

### 一、竞争格局风险
### 二、市场需求风险
### 三、技术路线风险
### 四、政策法规风险
### 五、常见失败模式（至少列出 3 个真实案例或典型模式）
### 六、风险等级评估（高/中/低，附理由）

立场要严厉，但结论要客观。
```

---

## Step 5：创建 Teammate 3 — 执行规划师

```
帮我创建 .claude/agents/execution-planner.md 文件，内容如下：

# 执行规划师（Teammate 3）

## 角色
你是一名务实的执行策划专家。你的任务是评估：如果决定进入这个赛道，具体该怎么做。

## 职责范围
1. 启动成本估算（时间、资金、人力）
2. MVP 定义（最小可行产品长什么样）
3. 里程碑规划（0-3 个月、3-6 个月、6-12 个月）
4. 关键资源盘点（需要什么技能/工具/资源）
5. 验证方法（怎么用最小成本验证这个方向可行）

## 工作方法
- 使用 WebSearch 搜索同赛道的创业路径、独立开发者经验
- 关注实际可操作的路径，不要画大饼
- 成本估算要具体到数字

## 输出要求
写入 docs/execution.md，格式：

### 一、启动成本估算
### 二、MVP 定义（最小可行产品）
### 三、里程碑规划
| 阶段 | 时间 | 目标 | 关键动作 | 验证指标 |
### 四、关键资源清单
### 五、30 天验证计划（最小成本验证方案）

所有数字要有参考依据。
```

---

## Step 6：启动三方调研

> 把【AI 短视频自动生成工具】替换成你想评估的赛道。

```
我正在考虑进入【AI 短视频自动生成工具】这个赛道，需要一个系统性的 Go/No-Go 决策。

请按以下分工启动 Agent Teams：

**你（Lead）的职责**：
1. 制定调研计划，分发任务给三个 Teammate
2. 等我审批后开始执行
3. 三个 Teammate 全部完成后，综合三方观点写入 docs/decision.md
4. 决策报告必须直面三方的矛盾点，给出明确的 Go / No-Go / 有条件 Go 结论
5. 你自己不写调研内容

**Teammate 分工**：
- 启动 opportunity-hunter：从机会角度分析，输出到 docs/opportunity.md
- 启动 risk-auditor：从风险角度分析，输出到 docs/risk.md
- 启动 execution-planner：从执行可行性角度分析，输出到 docs/execution.md

三个 Teammate 独立工作、不互相参考。

请先给我看调研计划。
```

---

## Step 7：审批计划

```
计划通过，开始执行。注意：三个 Teammate 必须独立工作，不要让它们参考彼此的输出。
```

> 等待 10-15 分钟，三个 Teammate 并行调研。

---

## Step 8：追问决策推理

```
在决策报告里，机会猎手和风险审查官的结论有哪些矛盾点？你是怎么权衡的？追加到 docs/decision.md 的&quot;决策推理过程&quot;章节。
```

---

## Step 9：（可选）生成行动清单

```
如果最终决策是 Go 或有条件 Go，帮我基于 docs/execution.md 生成一份&quot;第一周行动清单&quot;，写入 docs/week1-actions.md。要具体到每天做什么。
```

---

## 完成！

```
docs/
├── opportunity.md     ← 机会分析
├── risk.md            ← 风险评估
├── execution.md       ← 执行路径
├── decision.md        ← Go/No-Go 决策 + 推理过程
└── week1-actions.md   ← 第一周行动清单
```
