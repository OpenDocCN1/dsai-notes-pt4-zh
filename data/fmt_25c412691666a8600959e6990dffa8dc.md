
# 构建RAG系统的挑战

> 来源：[https://agijuejin.feishu.cn/docx/Gf1qdDNZyozgdrxpEbNcjTv4nXg](https://agijuejin.feishu.cn/docx/Gf1qdDNZyozgdrxpEbNcjTv4nXg)

> 作者：Meta

> 原文：[https://zhuanlan.zhihu.com/p/677989859](https://zhuanlan.zhihu.com/p/677989859)

大语言模型表现出色，但是在处理幻觉、使用过时的知识、进行不透明推理等方面存在挑战。检索增强生成（RAG）作为一个新兴的解决方案，通过整合外部知识库的数据，提高了模型在知识密集型任务中的准确性和可信度，能够实现知识持续更新和特定领域信息的集成，有效将LLM的内在知识与外部数据的巨大动态资源相结合。

关于RAG系统的详细介绍，可以参考笔者之前的文章：《如何用RAG技术提升LLM的能力》。本文聚焦于讨论RAG系统中存在的一些问题及其解决方案，主要参考《Seven Failure Points When Engineering a Retrieval Augmented Generation System》一文。

## RAG系统存在的挑战

常见的RAG系统通常有数据索引生成、检索、增强过程（将检索到的文档整合进Prompt）、生成答案这几大步骤。其中，每个步骤都可能存在一些问题，导致最终生成的答案未能完全正确回答用户的提问。如下图所示：

![](img/58764c8cf1e0fad7b724622bca15d156.png)

### 索引生成阶段

#### FP1 Missing Content

当根据现有的文档无法回答用户提问时，理想的情况应该是RAG系统直接给出“无法解答”的回应；然而实际情况是RAG系统可能会检索出一些“与问题相关但是无法回答该问题”的文档，并作出错误的回答。

### 检索阶段

#### FP2 Missed the Top Ranked Documents

理论上，所有能用于回答用户问题的文档都应该被检索模块召回。但是实际的RAG系统中，可能存在召回率低的问题，当能够用于回答用户问题的文档块未能被准确召回时，会误导大模型给出错误的答案。

### 增强过程阶段

#### FP3 Not in Context - Consolidation strategy Limitations

检索阶段能够检索到正确的文档，但是在将相关文档整合进prompt喂给大模型输出答案时，将相关文档的信息丢掉了，导致输出不正确的结果。

### 结果生成阶段

#### FP4 Not Extracted

输入了正确的上下文信息给LLM，但是大模型没能够从中提取出有效关键信息来正确回答问题。这通常是因为文档中有噪声或者有矛盾的信息来误导了大模型。这个问题虽然出现在结果生成阶段，但是追根溯源，主要还是索引生成、增强过程等阶段埋下的隐患。

#### FP5 Wrong Format

一些问题涉及到从特定格式文件中提取信息（如表格数据），但是LLM未能准确遵循指令从而得到错误的结果。

#### FP6 Incorrect Specificity

LLM输出的结果不够具体或者过于具体，与用户期望的答案不匹配。

#### FP7 Incomplete

LLM提供了不完整的答案，虽然不至于完全错误，但是没有完整回答用户的提问，即使这些信息在上下文中并且可供提取。例如面对“文档A、B和C中涵盖的关键点是什么？”这个问题，可能就会导致LLM回答的不够完整，更好的方法是分别提出这些问题。

## 从这些问题学到的一些经验

- 支持更大context的LLM能带来更好的结果（启发自FP4）
- 加入一些meta-data去提升检索效果（启发自FP2、FP4）
- 需要持续对RAG的效果进行观测和改进（启发自FP2-FP7）
- 将RAG系统构建成可配置的，方便及时快速修改配置（FP1,FP2）

文章中还列举了一些其他经验，但是没太看懂，详细的在下面表格中。

![](img/7185611cb1ad0a89f44214c3af2e3d5c.png)

相关文章：

1. Meta：如何利用查询改写技术改善RAG效果
2. Meta：如何用RAG技术提升LLM的能力

## 参考资料

1. [Seven Failure Points When Engineering a Retrieval Augmented Generation System](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2401.05856)
