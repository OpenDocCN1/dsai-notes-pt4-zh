
# RAGAS 框架：快速自动评估 `RAG` 质量，还便于集成

> 来源：`https://agijuejin.feishu.cn/docx/R9DkdKirro1EkWxWmMWcbNHknZf`

> 作者：朝亿 | AI 搜索引擎

> 原文：`https://mp.weixin.qq.com/s/fwV_NRDYl9RiKd6ewDEZqw`

今天为大家分享 `RAG` 的主流评估框架 `RAGAS`。

![](img/da551b07681e0e82530cba434b2efe22.png)

`RAGAS`: Automated Evaluation of Retrieval Augmented Generation

`RAGAS`: 检索增强生成的自动评估

论文地址：`https://arxiv.org/abs/2309.15217`

开源地址：`https://github.com/explodinggradients/ragas`

## 1. 论文概述

Retrieval-Augmented Generation (`RAG`) 是一种将检索机制与生成模型相结合的方法，首先通过检索与问题相关的文档或知识，然后基于检索结果生成答案或文本。最近的研究表明，`RAG`可以显著提高各种基准的生成质量。然而，`RAG`在检索阶段和生成阶段的质量评估也至关重要，这有利于帮助解决 `RAG` 的限制问题。

为了解决这个问题，本文设计了一个用于自动评估 `RAG` 的框架 `RAGAS`。该框架可以评估忠实度（即答案是否能够基于检索到的上下文推出）、答案相关性（即答案是否解决了问题）和上下文相关性（即检索到的上下文是否足够集中）。`RAGAS` 框架提供了与 `llamaindex` 和 `Langchain` 的集成，这两个框架是构建 `RAG` 解决方案最通用的框架，因此开发人员能够轻松地将 `RAGAS` 集成到他们的标准工作流中。

## 2. 核心方法

接下来介绍 `RAGAS` 提供的几个主要评价指标：`Faithfulness`、`Answer relevance`、`Context relevance`。这三个指标主要是基于大模型来进行评测。

### 2.1 `Faithfulness` 忠诚度评测

`Faithfulness`衡量了生成的答案与给定上下文的事实一致性。这个指标对于避免大模型幻觉以及确保检索到的上下文可以作为生成答案的依据非常重要。事实上，`RAG`系统对于生成文本和给定来源的事实性一致性非常重要，例如在法律领域。

评测方法：如果答案中的所有声明都可以从给定的上下文中推断出来，则生成的答案被视为可信的。要计算这一点，首先使用 `LLM` 从生成的答案提取一组声明，提取方法如下：

![](img/e8dca32c8083d8b0bd87a000b6404ac9.png)

对于每个声明，使用 `LLM` 来决定是否可以给定的上下文中推出，判断如下：

![](img/f8172b88772e578f48e2183aa0365e38.png)

最终确定分数：

![](img/4faad0eee06eb3f0a1fcad43c121fb93.png)

### 2.2 `Answer relevance` 回答相关性评测

`Answer relevance` 侧重于评估生成的答案是否能够解决输入的问题。不完整或包含冗余信息的答案得分较低，得分较高则表示相关性更高。

评测方法：对于生成的答案，首先使用 `LLM` 根据生成的答案 生成 `n` 个潜在问题，生成方法如下：

![](img/8a0baa72b329361e69b219a0e6b574e3.png)

接下来使用向量模型 `text-embedding-ada-002` 计算每个问题的嵌入值。然后计算每个问题和原始输入问题嵌入值的余弦相似性。最后得到分数：

![](img/4a0a346c1465b782b46f0644e4ab30fb93.png)

### 2.3 `Context relevance` 上下文相关性评测

`Context relevance` 侧重于评估检索的质量，主要量化检索的内容是否更精确。如果检索的上下文包含与回答问题相关的内容，即认为检索是有相关性的。

评测方法：为了评估相关性，首先使用 `LLM` 从检索的上下文中提取对问题答案至关重要的句子集合，提取方法如下：

![](img/6ee8e8a7a862553e693a324853578548.png)

然后基于提取的数量，以及检索内容的总句子数量，计算得分：

![](img/e96cb7cec33613c68c1bd039310afd96.png)

## 3. 精彩段落

![](img/79e79ecff9ebc5599c78dc0a3ed1d625.png)

## 4. 总结

`RAGAS`是专为评估 `RAG` 检索和生成质量设计的评估框架。`RAGAS`能够全面的评估用户设计的 `RAG` 系统的质量，目前 `RAGAS` 也在丰富自己的评价指标。

👆 关注 AI 搜索引擎，获取更多专业技术分享

~

👇 欢迎投稿

Email: supeng842499467@gmail.com
