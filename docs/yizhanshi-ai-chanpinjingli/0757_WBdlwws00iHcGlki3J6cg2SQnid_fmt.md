
# 第六章 | 规划模式（Planning Pattern）

> 来源：[https://v11enp9ok1h.feishu.cn/docx/AQxIdis54oU2MRxLieZcli0inSh](https://v11enp9ok1h.feishu.cn/docx/AQxIdis54oU2MRxLieZcli0inSh)

![](img/e51f2679cd089283257671525e2ffe12.png)

> “智能不只是做事的能力，更是知道先做什么、后做什么。” —— Antonio Gulli，《智能体设计模式》

---

## 一、回顾：从工具到计划

在第五章中，我们让AI第一次“动了起来”——通过工具模式（Tool Pattern），它能调用API、执行函数、操纵现实世界。

但新的问题马上出现：

> “AI知道怎么做，但它知道先做哪一步、后做哪一步吗？”

举个例子，用户说：

> “帮我制定一份明天早上去上海出差的计划。”

AI需要做的不只是回答，而是要：

1️⃣ 查询航班/高铁；

2️⃣ 安排出发时间；

3️⃣ 预订酒店；

4️⃣ 发送行程通知。

如果没有规划机制（Planning Pattern），AI 只能盲目顺序执行任务，缺乏全局视角。

📘 所以——规划模式，就是智能体的“前额叶皮层”。

## 二、什么是规划模式？

Antonio Gulli 给出的定义是：

> 规划模式（Planning Pattern）是指智能体在执行任务前，通过目标分解、步骤排序和资源分配，生成可执行的任务计划的设计模式。

简单说：

> 它是“让AI先想清楚再去干”的机制。

📘 类比人类行为：Planning 就像我们写 To-Do List，先理清目标，再一步步完成。

## 三、为什么需要规划模式？

规划是智能与反应的分界线。

没有规划，AI只是“条件反射”；有了规划，它就能“思考未来”。

书中总结了规划模式的三大价值👇

📘 如果说 Tool Pattern 是“手”，那 Planning Pattern 就是“行动地图”。

## 四、规划模式的工作流程

书中将 Planning Pattern 分为四个关键阶段👇

```
目标输入 → 任务分解 → 步骤排序 → 执行与调整
```

📘 规划并非一次性生成，而是动态调整的过程。

## 五、LangChain 的 Planning 实现

LangChain 提供了多种规划机制：

示例👇

![](img/145e5c02ed027dd6406ef5e053401da6.png)

📘 这段代码展示了经典“Plan & Execute”结构：

AI 会先生成详细计划，再逐步执行每个步骤。

## 六、规划的认知基础：思维树（Tree-of-Thought）

Antonio Gulli 在本章特别强调：

> “规划模式的本质，是让AI具备多步推理能力。”

这在技术上常以 Tree-of-Thought（ToT） 实现👇

![](img/30f3062b647a822e9e847c8a6ccfada2.png)

📘 这棵“思维树”让AI不再线性思考，而是分支探索、动态选择最优路径。

## 七、Google ADK 的任务规划引擎

在Google ADK（Agent Developer Kit）中，规划模式是系统的核心模块之一——Task Planning Engine。

它包含以下组件👇

📘 这让AI具备“项目管理者”的能力，而非“执行员”的思维。

## 八、规划与执行的闭环

规划并不等于执行完成。

真正强大的Agent会建立计划—执行—反馈—再计划的循环：

```
Plan → Execute → Observe → Reflect → Replan
```

这被称为PEOR循环（Planning-Execution-Observation-Reflection）。

## 九、动态规划与情境感知

书中提到一个常见陷阱：

> “静态规划无法适应动态环境。”

因此智能体需要具备Adaptive Planning能力：

📘 这与后续的 Reactive Pattern（反应模式） 联动，让AI“边走边计划”。

## 十、规划模式的算法核心

书中给出一个典型的规划算法框架👇

![](img/2ad34a213fc42b89b6f7f8ddc06929fc.png)

📘 这一逻辑结合了图算法（Graph-based Planning）与语义分析，是AI规划器的通用骨架。

## 十一、与人类规划的对比

📘 二者结合时，能形成“人机共策”的新型智能体系。

## 十二、最佳实践

Antonio Gulli 在章节末总结了五条设计原则👇

📘 这些原则奠定了智能体工程化规划的底层规范。

## 十三、现实应用场景

📘 规划模式几乎存在于所有高层智能体应用中。

## 十四、总结：让AI“先思考再行动”

规划模式（Planning Pattern）是智能体从“指令执行”迈向“理性决策”的转折点。

它让AI：

+ 拥有了目标感；

+ 能够分解复杂任务；

+ 在执行中保持全局一致性；

+ 甚至能在变化中自我调整。

> “行动的力量来自计划的清晰。”

📘 没有Planning，就没有真正的智能。
