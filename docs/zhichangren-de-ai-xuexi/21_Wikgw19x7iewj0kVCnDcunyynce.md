# 2203.11171v4_自洽性提高语言模型中的思考链推理能力

> 来源：[https://aithinkup.feishu.cn/docx/PZWkdDB5UovnewxuOcOcgPt1n5f](https://aithinkup.feishu.cn/docx/PZWkdDB5UovnewxuOcOcgPt1n5f)

> 文档AI含量：100%。没有读过计算机科学的论文，使用AI工具的一次试验。欢迎大家给意见，我来改进。

源文件下载地址：

https://sat2tjmapq6.feishu.cn/record/OSMyrkAoreEdMPcjkcGc77uRnqh

## **基础文档分析：**

1.  📄 文件名: 2203.11171v4.pdf

1.  📚 主题: 自洽性提高语言模型中的思考链推理能力

1.  💎 关键词: 自洽性, 思考链推理, 语言模型, 复杂推理任务, 预训练大型语言模型

1.  🔍 内容摘要:

本文提出了一种新的解码策略——自洽性（self-consistency），用于替代思考链提示（chain-of-thought prompting）中使用的简单贪婪解码。

自洽性通过采样多样化的推理路径集合，然后通过边际化这些采样推理路径来选择最一致的答案。

这种策略基于复杂推理问题通常允许多种不同的思考方式导致其正确答案的直觉。广泛的实证评估表明，自洽性显著提高了思考链提示在一系列流行算术和常识推理基准测试中的性能。

1.  🗂️ 文章大纲:

+   引言：介绍语言模型在复杂推理任务中的局限性及思考链提示的概念。

+   自洽性方法：详细描述自洽性策略及其与贪婪解码的比较。

+   实验设置：介绍用于评估自洽性的推理基准测试和语言模型。

+   主要结果：展示自洽性在不同推理任务和语言模型上的性能提升。

+   自洽性对不同推理路径的影响：探讨自洽性如何通过多样化推理路径提高准确性。

+   相关工作：讨论与推理、采样和重排以及语言模型一致性相关的研究。

+   结论和讨论：总结自洽性方法的优点和潜在的计算成本问题。

1.  📍 重要段落:

+   "Chain-of-thought prompting combined with pre-trained large language models has achieved encouraging results on complex reasoning tasks."

+   "We introduce a novel decoding strategy called self-consistency to replace the greedy decoding strategy used in chain-of-thought prompting."

+   "Our extensive empirical evaluation shows that self-consistency boosts the performance of chain-of-thought prompting with a striking margin on a range of popular arithmetic and commonsense reasoning benchmarks."

1.  📃 参考文献: 文档中提到了多篇相关研究，但具体的参考文献列表未在提供的文本中明确列出。

1.  📅 文件日期: 文档中未明确提供具体的文件日期，但根据文档内容和ICLR 2023会议信息，可以推测该文件与2023年相关。