# Agent 评测：AgentBench框架（清华）

> 来源：[https://agijuejin.feishu.cn/docx/Fs1EdHHzToJWoAxZl1CcGzBknTe](https://agijuejin.feishu.cn/docx/Fs1EdHHzToJWoAxZl1CcGzBknTe)

> 作者：西红柿牛腩

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/677136463)hihu.com/p/677136...

l2023 年 8 月，清华大学、俄亥俄州立大学、加州大学伯克利分校的研究团队便提出了首个系统性的基准测试——AgentBench[1]，用来评估 LLM 作为 Agent 在各种真实世界挑战和 8 个不同环境中的能力表现（如推理和决策能力）。

动机：什么样的LLM模型更适合作为Agent，其表现又如何？

•这 8 个环境分别是：操作系统、数据库、知识图谱、卡牌对战游戏、 家务事、横向思维谜题、网络购物、网页浏览。基于这 8 个环境，研究团队设计了不同的真实世 界挑战，涵盖了代码场景和生活场景，比如用 SQL 语言从一些表格里提取需要的数、玩卡牌游戏 取得胜利、从网页预订机票等。

AgentBench 包括 8 个环境：

+   **操作系统 **（OS）：考察 LLM 在 bash 环境进行文件操作、用户管理等能力。

+   **数据库 **（DB）：考察 LLM 利用 SQL 对给定数据库进行操作的能力。

+   **知识图谱 **（KG）：考察 LLM 利用工具从知识图谱中获取复杂知识的能力。

+   **卡牌对战 **（DCG）：考察 LLM 作为玩家，根据规则和状态进行卡牌对战的策略决策能力。

+   **情景猜谜 **（LTP）：该游戏需要 LLM 针对谜题进行提问，从而猜出答案，能够考察 LLM 的横向思维能力。

+   **家居 **（HH）：在模拟家庭环境下，LLM 需要完成一些日常任务，主要考察 LLM 将复杂的高级目标拆解为一系列简单行动的能力。

+   **网络购物 **（WS）：模拟在线购物环境中，LLM 需要按照需求完成购物，主要考察 LLM 的自主推理和决策能力。

+   **网页浏览 **（WB）：模拟网页环境中，LLM需要根据指令完成跨网站的复杂任务，考察 LLM 作为 Web agent的能力。

GPT-4 性能遥遥领先，开源模型能力显著弱于闭源模型。

研究者选择了 25 种主流的大模型 API 来进行 Agent 能力评估，涵盖了闭源模型（如 OpenAI 的 GPT-4、GPT-3.5 等）和开源模型 （LLaMA 2和Baichuan等）。根据测试结果来看，GPT-4基本上在所有环境中都占据领先地位， 是名副其实的当前大模型能力边界。闭源模型 Anthropic 的 Claude 以及 OpenAI 的 GPT-3.5 水平 相差不大，而常见的一些开源模型Vicuna、Dolly等由于尺寸和闭源模型相差了至少一个数量级， 性能评估显著较弱。

我们认为，虽然 LLM 能够在自然语言交流等 NLP 上达到基本的类人水平， 但在关注行动有效性、上下文长度记忆、多轮对话一致性和代码生成执行等 Agent 重要能力上的 表现仍旧相对落后，基于 LLM 的 AI Agent 的发展空间仍具潜力。

[1]Liu X, Yu H, Zhang H, et al. Agentbench: Evaluating llms as agents[J]. arXiv preprint arXiv:2308.03688, 2023.