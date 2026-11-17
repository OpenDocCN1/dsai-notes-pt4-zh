
# Harness Engineering 是什么？跟我日常用 Agent 有什么关系？

Harness Engineering 是系统性地改进 Agent 中模型之外的一切——工具、记忆、错误恢复、验证循环。它意味着你不需要等更强的模型才能让 Agent 更好用，从 Harness 入手往往投入产出比更高。这个概念由 Hashimoto 命名，OpenAI、Anthropic、Martin Fowler 站点在两个月内先后验证。

## Hermes 说“越用越强”，这是真的吗？具体靠什么机制？

真的。三套独立但协作的系统：四层记忆让 Agent 记住你是谁、喜欢什么；技能自动生成让 Agent 把复杂任务变成可复用的操作手册；GEPA 进化优化让 Agent 从失败中学习并自动改进。官方数据：10-20 次同类任务后执行速度提升 2-3 倍。

### 四大层级 → 四大案例：Lesson 2 预告

| Harness 层级 | Lesson 1 建立的认知 | Lesson 2 你将亲手实现 |
| --- | --- | --- |
| L1 运行时基座 | 47 个工具 + 6 种终端后端 | 案例 A：Terminal Agent — 让 Agent 完成复合终端任务 |
| L2 消息网关 | 14+ 平台内建适配器 | 案例 B：飞书 AI 助手 — 在手机上和 Agent 对话 |
| L3 持久记忆 | `MEMORY.md` + `USER.md` + SQLite FTS5 | 案例 C：持久记忆 — 让 Agent 真正记住你 |
| L4 Skill 自主进化 | 技能自生成 + GEPA | 案例 D：Skill 自进化 — 亲眼见证 Agent 成长 |

Lesson 1 建立了认知地图，Lesson 2 将在地图上逐层落地。下一节课，你将亲手实现刚才看到的四个案例。

### 方法回顾：从已知出发认识未知

这节课我们采用了一个简单但有效的学习路径：从通用 Agent 的真实痛点出发，引入 Harness Engineering 的认知框架理解"问题不在模型，在 Harness"，再认识 Hermes Agent 如何将这套理论付诸实践，最后动手安装验证。从痛点到理论，从理论到实践——这也是 Lesson 2 四个案例的学习节奏。

---

+ 公开课课件领取

![](img/39e7dc5a57c9d1bae3f8d9f62459719c.png)
