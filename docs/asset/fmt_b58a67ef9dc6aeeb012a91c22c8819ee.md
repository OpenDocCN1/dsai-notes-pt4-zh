
# Claude 4 核心提示词曝光｜最懂提示词的大模型公司，现在怎么写 Prompt？

> 来源：[https://chatmemo.feishu.cn/docx/M7TVdPpyyoMaSKxndVLcmFdEnxg](https://chatmemo.feishu.cn/docx/M7TVdPpyyoMaSKxndVLcmFdEnxg)

> 经对比整理，Claude 4 的核心提示词，呈现出以下变化：

+ 更典型的推理提示策略，使用更统括性的原则提示
+ 引导模型在扩展思考中多步推理
+ 对特定知识进行强调
+ 聚焦核心能力，淡化冗余“人设”

![](img/1aa510f014eabf340084e8a6b10b4300.png)

时隔 3 个月，Anthropic 上新了 Claude 4 模型，并在官网同步了 Claude 4 Opus 和 Sonnet 两个模型的最新系统提示词。（Opus 是旗舰版、Sonnet 是主力版）

![](img/ef9648587d9866569da2492541e9af8f.png)

那，就来写写本👸号的保留节目：

看看 3 个月过去，世界上最懂提示词的 AI 大模型公司，他们在 Claude 4 中的提示词最佳实践，有无值得学习的新变化。

经 一泽Eze 校对与整理，本文同时提供：

1. Claude 4 万字核心提示词全文（双语对照）
2. Claude 4 提示词设计方法拆解（思维导图）
3. 与 Claude 3.7 sonnet 提示词的主要设计变化

> 在对接大模型、设计 Agent 提示词，还可以按需当作参考指南和查漏补缺的速查手册。

> 🥰 强**烈推荐转发、收藏。**

![](img/722f957901e4a54c429aa61656ee504d.png)

---

## 👀 一文看全 Claude 4 万字核心提示词

> 为了方便查阅，一泽制作了双语对照版本。

> 👉 如果觉得太长，建**议直接划到下一节，**直接阅读我提炼的提示词设计思维导图

经过对照，Claude 4 Opus 与 Sonnet 版本的系统提示词，基本没有区别，所以只需要看 Opus 的提示词即可：
