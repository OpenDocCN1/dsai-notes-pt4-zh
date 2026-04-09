# Agent SIG

> 来源：[https://aicarrier.feishu.cn/docx/CzV3d1LShogcS1x68DZc9X9jnfo](https://aicarrier.feishu.cn/docx/CzV3d1LShogcS1x68DZc9X9jnfo)

# 书生·浦语 Agent 兴趣小组

## 什么是 Agent

大语言模型存在着多种局限性，比如大模型可能会产生虚假信息，误导用户；训练数据往往已经过时，无法反映出最新趋势和信息；缺少外部验证和更新机制，从而限制了大模型的适应性和持续更新；在复杂任务面前会更经常犯错。因此将 LLM 作为一种智能体（Agent）的想法与技术应运而生。

那么什么是 Agent 呢？Hayes Roth 曾在1995年这样认为：Agent 能够感知环境中的动态条件，能采取行动影响环境，能运用推理能力理解新信息、解决问题、产生推断、决定动作。在 LLM 时代，Agent 便等于 model 加上 memory 加上 tools，就像是大脑有了记忆，有了手脚，可以做到更多的事情。

Agent 在接收输入后，首先由模型根据当前的记忆进行思考与规划，采取合适的动作，比如选择出要使用的工具并且进行调用；在得到工具的反馈后，模型再基于工具反馈进行下一步的规划，直到任务完成。

因此，Agent 的全流程主要包括三个部分：

+   模型的思考与规划

+   动作的定义与调用

+   记忆的增加与读取

**Agent** **SIG 讨论区：****h**ttps://github.com/InternLM/tutorial/discussions/categories/agent

**时间周期：**2024 年 3 月—— 6 月

**小组长：45度角仰望天空**

**想要加入 Agent SIG 的同学欢迎微信联系浦语小助手（微信搜索 Inte**rnLM）或者小组长45度角仰望天空（微信搜索 Skyward45Degree）

## 在 Agent SIG 可以做什么？

在这里，可以与 Agent SIG 一起阅读论文、交流想法、参加圆桌会议、实现你自己的 Agent 等。

## 组员福利

+   良好的学习氛围

+   优秀组员证书

+   上海人工智能实验室内推机会

+   免费且充足的算力支持

+   上海人工智能实验室研究员亲自指导

# 学习资料

Lagent：https://github.com/InternLM/lagent.git

AgentLego：https://github.com/InternLM/agentlego

MetaGPT：https://github.com/geekan/MetaGPT，https://github.com/datawhalechina/Hugging-Multi-Agent

Agent 论文仓库：https://github.com/zjunlp/LLMAgentPapers

# 参考论文

| 标题 | 链接 |
| --- | --- |
| Auto-GPT for Online Decision Making: Benchmarks and Additional Opinions | https://arxiv.org/abs/2306.02224 |
| AgentBench: Evaluating LLMs as Agents | https://arxiv.org/abs/2308.03688 |
| ReAct: Synergizing Reasoning and Acting in Language Models | https://arxiv.org/abs/2210.03629 |
| ReWOO: Decoupling Reasoning from Observations for Efficient Augmented Language Models | https://arxiv.org/abs/2305.18323 |
| Generative Agents: Interactive Simulacra of Human Behavior | https://arxiv.org/abs/2304.03442 |
| The Rise and Potential of Large Language Model Based Agents: A Survey | https://arxiv.org/abs/2309.07864v3 |