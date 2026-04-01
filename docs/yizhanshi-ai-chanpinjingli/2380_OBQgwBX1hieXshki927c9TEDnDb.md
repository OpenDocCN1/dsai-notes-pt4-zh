# 01🈷️12日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/OIENdZYeMoZDcIxq8HMce2CYnlf](https://v11enp9ok1h.feishu.cn/docx/OIENdZYeMoZDcIxq8HMce2CYnlf)

> 🔔AI情报助手午报「01月12日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🧠

> Corex: 通过多模型协作增强推理能力

>

+   本文介绍了Corex框架，它通过多模型协作增强复杂推理任务的能力，包括Discuss、Review和Retrieve三种模式，显著提升了推理的准确性和忠实性。

+   Corex框架突破了单一模型的推理瓶颈，通过集体智能克服了推理领域的挑战，实验结果在多个基准数据集上优于现有方法。

+   Discuss模式通过模型间群组讨论提高推理链的事实性和多样性，Review模式则通过模型间review确保内容的正确性，Retrieve模式帮助选择最可信的答案。

+   实验部分展示了Corex在数学推理、常识推理、符号推理和表格推理等多个任务上的性能提升，证明了多模型协作的有效性。

+   Corex证明了多模型协作工作比传统单模型推理能有更好的效果，并且具有任务无关性，有潜力被适应到新的任务和场景上。

> 关键词：多模型协作，复杂推理，Corex框架，集体智能，性能提升

>

> 🔗 文中引用的链接

+   [论文地址](https://openreview.net/forum?id=7BCmIWVT0V)

+   [项目地址](https://github.com/QiushiSun/Corex)

> 2️⃣

> 🧠

> 分块的艺术：提升 RAG 效果的关键

>

+   RAG系统通过“分块”技术，将复杂文本拆分成易处理的片段，提高AI理解和处理信息的效率和准确性。

+   分块技术是RAG架构中的关键角色，直接影响生成式AI应用的准确性，是整个流程的第一道关卡。

+   优化分块的技巧包括固定字符大小分块、递归字符文本分割、特定文档的拆分、语义分割和自主分割，每种方法都有其适用场景和优缺点。

+   语义分割通过嵌入技术分析文本语义，根据意思来分块，保持文本的连贯性和上下文的完整性。

+   代理分割（Agentic Splitter）利用大型语言模型的超强理解力，智能地给文本分块，类似于人类阅读时的分段方式。

> 关键词：RAG，分块技术，语义分割，AI效率，上下文处理

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/f3VjoyJn72Yyh0UlI5W8oQ)

> 3️⃣

> 🧠

> o1类大模型的过度思考: 2+3=？

>

+   腾讯AI Lab和上交研究表明，o1类大模型在处理简单的算术问题“2+3=？”时存在过度思考现象，这反映出这些模型在复杂任务处理中可能并不高效和智能。

+   研究指出o1类模型相比基础模型在简单问题上的token消耗高达1,953%，并尝试通过self-training范式来缓解这一问题。

+   作者提出了Outcome Efficiency Metric和Process Efficiency Metric两个评估指标，用于衡量模型的效率。

+   在三个数据集上的测试显示，o1类模型在简单数学问题上“过度思考”的现象尤为明显。

+   通过PRM12K数据集的self-training，作者探究了不同的post-training方法和简化回复策略，有效缓解了“过度思考”。

> 关键词：大模型，过度思考，效率评估，self-training，解决方案简化

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/abs/2412.21187)

> 4️⃣

> 🧠

> [生成式推荐最新进展](https://mp.weixin.qq.com/s/fks7eE_2gzOZhC4te_ni3g)

>

+   本文聚焦生成式推荐领域，挑选了十篇最新的相关工作，探讨了大语言模型（LLMs）在推荐系统中的应用和挑战。

+   介绍了一种自我改进的物品标记化方法，通过优化LLMs自身的物品标记来提高推荐准确性。📌

+   提出了LIGER模型，结合序列密集检索和生成式检索的优势，增强了冷启动物品推荐。🌟

+   研究了如何通过两阶段对齐和行为语义标记化来协调推荐系统，提升了召回率和可扩展性。🔥

+   探讨了偏好识别在序列推荐系统中的应用，提出了Mender方法，改进了用户偏好的识别能力。🎯

+   介绍了TTDS推荐系统，通过动态语义索引范式提高了基于LLM的生成式推荐系统的性能。🚀

> 关键词：生成式推荐，大语言模型，推荐系统，用户偏好，性能提升

>

> 🔗 文中引用的链接

+   [E](https://arxiv.org/abs/2412.17171)nhancing Item Tokenization for Generative Recommendation

+   [U](https://arxiv.org/abs/2411.18814)nifying Generative and Dense Retrieval for Sequential Recommendation

+   [Semantic Convergence: Harmonizing Recommender Systems ](https://arxiv.org/abs/2412.13771)via Two-Stage Alignment and Behavioral Semantic Tokenization

+   [P](https://arxiv.org/abs/2412.08604)reference Discerning with LLM-Enhanced Generative Retrieval

+   [Un](https://arxiv.org/abs/2409.09253)leash LLMs Potential for Recommendation by Coordinating Twin-Tower Dynamic Semantic Token Generator

> 5️⃣

> 🧠

> [PyTorch 实现 Multi](https://mp.weixin.qq.com/s/XyFQV3-dXWITnXpl8I9oaw)-Head Self-Attention

>

+   本文介绍了如何使用PyTorch实现Multi-Head Self-Attention机制，这是Transformer模型的核心组件之一。

+   文章提供了详细的代码实现，包括如何定义MultiHeadAttention类以及如何进行前向传播计算。

+   讨论了在AI面试中对于self-attention实现的不同预期，并展示了不同版本的self-attention实现。

+   强调了模型并行和流水线并行在实际应用中的重要性，以及它们如何影响代码的实现方式。

+   通过对代码的注释和解释，帮助读者理解Multi-Head Self-Attention的工作原理和代码细节。

> 关键词：Transformers，Multi-Head Self-Attention，PyTorch，AI面试，模型并行

>

> 🔗 文中引用的链接

+   [原文首发链接](https://bruceyuan.com/hands-on-code/from-self-attention-to-multi-head-self-attention.html)