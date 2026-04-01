# RankRAG: 在单LLM中统一上下文排序和RAG

> 来源：[https://agijuejin.feishu.cn/docx/AuzQdwC8YoVfFJxUprYc00HXnzc](https://agijuejin.feishu.cn/docx/AuzQdwC8YoVfFJxUprYc00HXnzc)

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/707812125)hihu.com/p/707812...

[英伟达 ](https://www.zhihu.com/search?q=%E8%8B%B1%E4%BC%9F%E8%BE%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)出品 --- 在单个 [LLM ](https://www.zhihu.com/search?q=LLM&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)中同时实现上下文排序和答案生成！

传统 RAG 的问题在于，LLM 通常会利用检索器中的前   k k 个上下文。这导致了次优的性能，尤其是在处理大量检索到的段落或初始检索结果不佳时。 RankRAG解决的关键问题是如何在单个LLM中统一上下文排序和答案生成，以实现更有效的检索增强生成（RAG）。

## 问题背景

在 [检索增强 ](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E5%A2%9E%E5%BC%BA&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)生[成中，给出文档或上下文的](https://www.zhihu.com/search?q=embedding%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)集合，提供基础知识。给定一个问题   q q ，检索器   R \mathcal{R} （例如，参数化的 embedding模型 ）首先检索与问题最相关的top-   k k 上下文   C={c1,……,ck} \mathcal{C}=\{c_1,……,c_k\} 。随后，语言模型生成最终答案，其中答案可以是短句或长句，具体取决于目标任务的类型。

## 当前RAG piple的局限性

[当前 “retri](https://www.zhihu.com/search?q=pipeline&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)eve-then-generate” RAG pipeline 的局限性：

+   **检索器容量有限 **：当前RAG系统通常采用稀疏检索（[BM25）或中等尺寸（基](https://www.zhihu.com/search?q=Embedding%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)[于BERT）](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E7%9B%B8%E4%BC%BC%E5%BA%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)的 Embedding模型 作为检索器。这些模型独立编码问题和文档，并使用 向量相似度 计算问题和文档之间的相似度。然而，embedding模型的有限容量以及查询和文档的独立处理限制了估计问题和文档之间文本相关性的能力，降低了它们在新任务或领域中的有效性。

+   **选取Top-k上下文的折中 **：尽管[最先进的](https://www.zhihu.com/search?q=%E5%8F%AC%E5%9B%9E%E7%8E%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)长上下文LLM可以将许多检索到的上下文作为答案生成的输入，但在实践中，性能很快就会随着   k k 的增加而饱和。 一般来说，鉴于检索器的表达能力有限，较小的   k k 通常无法捕获所有相关信息，从而影响召回。相比之下，较大的   k k 提高了 召回率 ，但代价是引入了不相关的内容，阻碍了LLM生成准确答案的能力。

## RankRAG

RankRAG指令调整LLM同时捕获问题和上下文之间的相关性，利用检索到的上下文生成答案。

![](img/ed3c2510fd573c9f5a54c77598f3d969.png)

### 第一阶段： [监督微调 ](https://www.zhihu.com/search?q=%E7%9B%91%E7%9D%A3%E5%BE%AE%E8%B0%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)（ [SFT ](https://www.zhihu.com/search?q=SFT&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)）

使用高质量的指令遵循数据集对语言模型进行微调，包括对[话数据集（Co](https://www.zhihu.com/search?q=%E6%80%9D%E7%BB%B4%E9%93%BE%E6%95%B0%E6%8D%AE%E9%9B%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)nv[ersati](https://www.zhihu.com/search?q=%E6%8C%87%E4%BB%A4%E6%95%B0%E6%8D%AE%E9%9B%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)on）、 思维链数据集 （ Chain-of-thought ）、长文本QA数据集（long-form QA）和LLM生成的 指令数据集 （Synthetic Instructions）。 使用多轮对话格式，将用户和助手的对话历史作为上下文，在助手的最后回复处计算损失。 总共使用了12.8万个SFT样本。

### 第二阶段：排名和生成的统一指令微调

第一阶段的 SFT 使 LLM 具备基本的指令遵循能力；然而，它们在 RAG 任务上的表现往往仍不理想，因为LLM没有针对从给定问题的检索上下文中提取答案进行优化。 RankRAG为检索增强生成和上下文排序指令调整LLM，上下文排序能力尤其对在不完美检索器的情况下获得更相关的top-  k 上下文至关重要。

该阶段结合以下五部分数据进行指令调优：

(1) 第一阶段的 [监督式微调(SFT) ](https://www.zhihu.com/search?q=%E7%9B%91%E7%9D%A3%E5%BC%8F%E5%BE%AE%E8%B0%83%28SFT%29&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)数据：用于保持LLM的指令遵循能力

(2) 上下文丰富的QA数据集 ：利用多个富含上下文的问答任务来增强LLM利用上下文进行生成的能力。

[(3)](https://www.zhihu.com/search?q=%E9%B2%81%E6%A3%92%E6%80%A7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D) 检索增强的QA数据集 ：提高LLM对生成时不相关上下文的 鲁棒性

(4) 上下文排序数据集：赋予LLM排序能力

(5) 检索增强的 [排序数据集 ](https://www.zhihu.com/search?q=%E6%8E%92%E5%BA%8F%E6%95%B0%E6%8D%AE%E9%9B%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)：训练LLM在给定一个问题时，可以同时确定多个上下文的相关性

![](img/2577580e3fe575aff9f4a4e3f733bf01.png)

所有任务都可以统一为  (x,c,y) 格式（  x 是问题，  c 是相应的上下文，  y 是目标输出答案），这能够促进跨任务的有效知识转移。

第二阶段通过结合多种QA数据集和排序数据集对语言模型进行指令调优，提高了模型进行上下文排序和生成答案的能力，为后续的RAG任务打下了基础。

## RankRAG 推理: Retrieve-Rerank-Generate Pipeline

由于RankRAG包含了一个额外的重排序步骤，每个问题的推理管道被修改为retrieve-rerank-generate管道，描述如下：

[（1）检](https://www.zhihu.com/search?q=%E8%AF%AD%E6%96%99%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)索器  \mathcal{R} 首先从 语料库 中检索top-  N 上下文

（2）RankRAG模型计算问题和检索到的  N 个上下文之间的相关性分数，作为使用第二阶段指令模版中的提示将答案生成为  True 的概率，然后重排序上下文仅保留前-  k  (k\ll N) 上下文，然后将其用作生成步骤的输

（3）将top-  k 上下文与问题一起拼接并反馈到RankRAG模型中以生成最终答案

计算相关性通常只需要生成一个token，与top-  k 上下文的生成步骤相比，涉及的输入要短得多。

## 实验结果

在多个 [知识密集型 ](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%AF%86%E9%9B%86%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)基准测试中，RankRAG 表现优于现有RAG方法，对有挑战性的数据集也表现出了显著改善。

![](img/3698ad4f45cb79d6509b5f0967edadb7.png)

[在大多数情况下](https://www.zhihu.com/search?q=rank%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A707812125%7D)，与在10×以上ranking数据训练的专用 rank模型 相比，RankRAG实现了更好地召回。

尽管用更少的排名数据，但RankRAG获得了更好的性能。

![](img/82f4ea7e6fc36339d8900e344d74a15b.png)

大家最关心的问题：怎么获取模型？作者说稍后会发布。

![](img/887229e1f24f85e231e7e05ac2957b26.png)

## 参考文档

https:// arxiv.org/abs/2407.0248 5