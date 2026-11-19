
# 2210.03629v3_Synergizing Reasoning and Acting in Language Models

> 来源：[`https://aithinkup.feishu.cn/docx/ZvsedFV9Co25qExd4MJcGLVqnIl`](https://aithinkup.feishu.cn/docx/ZvsedFV9Co25qExd4MJcGLVqnIl)

> 文档AI含量：100%。没有读过计算机科学的论文，使用AI工具的一次试验。欢迎大家给意见，我来改进。

源文件下载地址：

`https://sat2tjmapq6.feishu.cn/record/ZheTrCdcfekJdocDb8lcsVEMnbX`

## 基础文档分析：

- 📄 文件名：`2210.03629v3.pdf`
- 📚 主题：Synergizing Reasoning and Acting in Language Models
- 💎 关键词：Large language models (`LLMs`), Reasoning, Acting, `ReAct`, Question answering, Fact verification, Interactive decision making
- 🔍 内容摘要：

本文探讨了如何使用大型语言模型(`LLMs`)同时生成推理过程和特定任务的动作，以实现两者间的协同作用。

`ReAct`方法通过交替生成推理轨迹和动作，增强了模型对行动计划的制定、追踪和更新，同时通过与外部信息源（如知识库或环境）的交互来处理异常情况。

`ReAct`在多种语言理解与交互决策任务中表现出色，相较于现有技术，不仅提升了性能，还提高了人类对模型的可解释性和信任度。

- 🗂️ 文章大纲：

  + 引言
  + `ReAct`: 结合推理与行动
  + 知识密集型推理任务
  + 决策制定任务
  + 相关工作
  + 结论

- 📍 重要段落：

  + "While large language models (`LLMs`) have demonstrated impressive performance across tasks in language understanding and interactive decision making, their abilities for reasoning and acting have primarily been studied as separate topics."
  + "We apply our approach, named `ReAct`, to a diverse set of language and decision making tasks and demonstrate its effectiveness over state-of-the-art baselines in addition to improved human interpretability and trustworthiness."
  + "On two interactive decision making benchmarks (`ALFWorld` and `WebShop`), `ReAct` outperforms imitation and reinforcement learning methods by an absolute success rate of 34% and 10% respectively."

- 📃 参考文献：

  + 引用了包括但不限于以下文献：
  + "Chain of Thought Prompting Elicits Reasoning in Large Language Models" by Jason Wei et al.
  + "Fever: A Large-scale Dataset for Fact Extraction and Verification" by James Thorne et al.
  + "HotpotQA: A Dataset for Diverse, Explainable Multi-hop Question Answering" by Zhilin Yang et al.

- 📅 文件日期：10 Mar 2023
