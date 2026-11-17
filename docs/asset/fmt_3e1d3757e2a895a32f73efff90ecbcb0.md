
+ 实战技巧：

+ 【`Docs`构建索引——`Faiss`】

+ 【`Docs`构建索引——`milvus`】

+ 【`Docs`构建索引——`Elasticsearch`】

## 模块三：大模型微调

### 为什么需要大模型微调？

通常，要对大模型进行微调，有以下一些原因：

+ 第一个原因是，因为大模型的参数量非常大，训练成本非常高，每家公司都去从头训练一个自己的大模型，这个事情的性价比非常低；

+ 第二个原因是，`Prompt Engineering`的方式是一种相对来说容易上手的使用大模型的方式，但是它的缺点也非常明显。因为通常大模型的实现原理，都会对输入序列的长度有限制，`Prompt Engineering`的方式会把`Prompt`搞得很长。

越长的`Prompt`，大模型的推理成本越高，因为推理成本是跟`Prompt`长度的平方正向相关的。

另外，`Prompt`太长会因超过限制而被截断，进而导致大模型的输出质量打折口，这也是一个非常严重的问题。

对于个人使用者而言，如果是解决自己日常生活、工作中的一些问题，直接用`Prompt Engineering`的方式，通常问题不大。

但对于对外提供服务的企业来说，要想在自己的服务中接入大模型的能力，推理成本是不得不要考虑的一个因素，微调相对来说就是一个更优的方案。

+ 第三个原因是，`Prompt Engineering`的效果达不到要求，企业又有比较好的自有数据，能够通过自有数据，更好的提升大模型在特定领域的能力。这时候微调就非常适用。

+ 第四个原因是，要在个性化的服务中使用大模型的能力，这时候针对每个用户的数据，训练一个轻量级的微调模型，就是一个不错的方案。

+ 第五个原因是，数据安全的问题。如果数据是不能传递给第三方大模型服务的，那么搭建自己的大模型就非常必要。通常这些开源的大模型都是需要用自有数据进行微调，才能够满足业务的需求，这时候也需要对大模型进行微调。

### 如何对大模型进行微调？

#### q1：大模型的微调技术路线问题

从参数规模的角度，大模型的微调分成两条技术路线：

+ 技术路线一：对全量的参数，进行全量的训练，这条路径叫全量微调`FFT(Full Fine Tuning)`。

+ 技术路线二：只对部分的参数进行训练，这条路径叫`PEFT(Parameter-Efficient Fine Tuning)`。

#### q2：大模型的全量微调`FFT` 技术存在哪些问题

`FFT`也会带来一些问题，影响比较大的问题，主要有以下两个：

+ 问题一：训练的成本会比较高，因为微调的参数量跟预训练的是一样的多的；

+ 问题二：灾难性遗忘(`Catastrophic Forgetting`)，用特定训练数据去微调可能会把这个领域的表现变好，但也可能会把原来表现好的别的领域的能力变差。

#### q3：大模型的 `PEFT(Parameter-Efficient Fine Tuning)` 解决哪些问题

`PEFT`主要想解决的问题，就是`FFT`存在的上述两个问题，`PEFT`也是目前比较主流的微调方案。

从训练数据的来源、以及训练的方法的角度，大模型的微调有以下几条技术路线：

+ 技术路线一：监督式微调`SFT(Supervised Fine Tuning)`，这个方案主要是用人工标注的数据，用传统机器学习中监督学习的方法，对大模型进行微调；

+ 技术路线二：基于人类反馈的强化学习微调`RLHF(Reinforcement Learning with Human Feedback)`，这个方案的主要特点是把人类的反馈，通过强化学习的方式，引入到对大模型的微调中去，让大模型生成的结果，更加符合人类的一些期望；

+ 技术路线三：基于`AI`反馈的强化学习微调`RLAIF(Reinforcement Learning with AI Feedback)`，这个原理大致跟`RLHF`类似，但是反馈的来源是`AI`。这里是想解决反馈系统的效率问题，因为收集人类反馈，相对来说成本会比较高、效率比较低。

不同的分类角度，只是侧重点不一样，对同一个大模型的微调，也不局限于某一个方案，可以多个方案一起。

微调的最终目的，是能够在可控成本的前提下，尽可能地提升大模型在特定领域的能力。

### 大模型`LLM`进行`SFT`操作的时候在学习什么？

+ 预训练->在大量无监督数据上进行预训练，得到基础模型-->将预训练模型作为`SFT`和`RLHF`的起点。

+ `SFT`-->在有监督的数据集上进行`SFT`训练，利用上下文信息等监督信号进一步优化模型-->将`SFT`训练后的模型作为`RLHF`的起点。

+ `RLHF`-->利用人类反馈进行强化学习，优化模型以更好地适应人类意图和偏好-->将`RLHF`训练后的模型进行评估和验证，并进行必要的调整。

### step 1：大模型微调训练数据构建

+ 介绍：如何构建训练数据？

+ 实战技巧：

+ 【大模型（`LLMs`）`LLM`生成`SFT`数据方法篇】

### step 2：大模型指令微调篇

+ 介绍：如何构建训练数据？

+ 实战技巧：

+ 【大模型（`LLMs`）继续预训练篇】

+ 【大模型（`LLMs`）指令微调篇】

+ 【大模型（`LLMs`）奖励模型训练篇】

+ 【大模型（`LLMs`）强化学习——`PPO`训练篇】

+ 【大模型（`LLMs`）强化学习——`DPO`训练篇】

## 模块四：文档检索

### 为什么需要文档检索？

`文档检索` 作为 `RAG` 核心工作，其效果对于下游工作至关重要。

虽然可以通过向量召回的方式从文档库里召回和用户问题相关的文档片段，同时输入到`LLM`中，增强模型回答质量。

常用的方式直接用用户的问题进行文档召回。但是很多时候，用户的问题是十分口语化的，描述的也比较模糊，这样会影响向量召回的质量，进而影响模型回答效果。

本章主要介绍 `文档检索` 过程中 存在的一些问题和对应的解决方法。

### step 1：文档检索负样本样本挖掘

+ 介绍：在各类检索任务中，为训练好一个高质量的检索模型，往往需要从大量的候选样本集合中采样高质量的负例，配合正例一起进行训练。

+ 实战技巧：

+ 【文档检索——负样本样本挖掘篇】

### step 2：文档检索优化策略

+ 介绍：文档检索优化策略

+ 实战技巧：

+ 【文档检索——文档检索优化策略篇】

## 模块五：Reranker

### 为什么需要Reranker？

基本的 `RAG` 应用包括四个关键技术组成部分：

+ `Embedding` 模型：用于将外部文档和用户查询转换成 `Embedding` 向量

+ 向量数据库：用于存储 `Embedding` 向量和执行向量相似性检索（检索出最相关的 `Top-K` 个信息）

+ 提示词工程（`Prompt engineering`）：用于将用户的问题和检索到的上下文组合成大模型的输入

+ 大语言模型（`LLM`）：用于生成回答

上述的基础 `RAG` 架构可以有效解决 `LLM` 产生“幻觉”、生成内容不可靠的问题。但是，一些企业用户对上下文相关性和问答准确度提出了更高要求，需要更为复杂的架构。一个行之有效且较为流行的做法就是在 `RAG` 应用中集成 `Reranker`。

### 什么是 Reranker？

`Reranker` 是信息检索（`IR`）生态系统中的一个重要组成部分，用于评估搜索结果，并进行重新排序，从而提升查询结果相关性。

在 `RAG` 应用中，主要在拿到向量查询（`ANN`）的结果后使用 `Reranker`，能够更有效地确定文档和查询之间的语义相关性，更精细地对结果重排，最终提高搜索质量。

### step 1：Reranker 篇

+ 理论学习：

+ 【`RAG`文档检索——`Reranker` 篇】

+ 实战技巧：

+ 【`Reranker`——`bge-reranker`篇】

## 模块六：RAG 评测面

### 为什么需要对 RAG 进行评测？

在探索和优化 `RAG`（检索增强生成器）的过程中，如何有效评估其性能已经成为关键问题。

### step 1：RAG 评测 篇

+ 理论学习：

+ [【`RAG`评测篇】](https://articles.zsxq.com/id_bs4s6x213o8i.html)

## 模块七：RAG 开源项目推荐学习

### 为什么需要RAG开源项目推荐学习？

前面已经带你走完了 `RAG` 的各个流程，下面将推荐一些 `RAG` 开源项目，帮助大佬们进行消化学习。

#### RAG 开源项目推荐 —— `RAGFlow` 篇

+ 介绍：`RAGFlow` 是一款基于深度文档理解构建的开源 `RAG`（`Retrieval-Augmented Generation`）引擎。`RAGFlow` 可以为各种规模的企业及个人提供一套精简的 `RAG` 工作流程，结合大语言模型（`LLM`）针对用户各类不同的复杂格式数据提供可靠的问答以及有理有据的引用。

+ 项目学习：

+ [【`RAG` 项目推荐——`RagFlow` 篇(一)](https://articles.zsxq.com/id_oota38wboidk.html)——`RagFlow` docker 部署]

+ [【`RAG` 项目推荐——`RagFlow` ](https://articles.zsxq.com/id_lfsekvfvqrs3.html)篇(二)——`RagFlow` 知识库构建]

+ [【`RAG` 项目推荐——`RagFlow` 篇(](https://articles.zsxq.com/id_ac0xn785c6mo.html)三)——`RagFlow` 模型供应商选择]

+ [【`RAG` 项目推荐——`RagFl`](https://articles.zsxq.com/id_aamqhv6h4lsn.html)ow 篇(四)——`RagFlow` 对话]

+ [【](https://articles.zsxq.com/id_ol0qu2w2eraf.html)`RAG` 项目推荐——`RagFlow` 篇(五)——`RAGFlow` `Api` 接入（以 `ollama` 为例）]

+ [【`RAG` 项目推荐——`RagFlow`](https://articles.zsxq.com/id_c0br2l9vkwi3.html) 篇(六)——`RAGFlow` 源码学习]

#### RAG 开源项目推荐 —— `QAnything` 篇

+ 介绍：`QAnything`（`Question and Answer based on Anything`）是一个本地知识库问答系统，旨在支持多种文件格式和数据库，允许离线安装和使用。使用`QAnything`，您可以简单地删除本地存储的任何格式的文件，并获得准确、快速和可靠的答案。`QAnything`目前支持的知识库文件格式包括：`PDF(pdf)` , `Word(docx)` , `PPT(pptx)` , `XLS(xlsx)` , `Markdown(md)` , `Email(eml)` , `TXT(txt)` , `Image(jpg，jpeg，png)` , `CSV (csv)`、网页链接(`html`)等。

+ 项目学习：

+ 【`RAG` 开源项目推荐 —— `QAnything` 篇】

#### RAG 开源项目推荐 —— `ElasticSearch-Langchain` 篇

+ 介绍：受`langchain-ChatGLM`项目启发，由于`Elasticsearch`可实现文本和向量两种方式混合查询，且在业务场景中使用更广泛，因此本项目用`Elasticsearch`代替`Faiss`作为知识存储库，利用`Langchain`+`Chatglm2`实现基于自有知识库的智能问答。

+ 项目学习：

+ [【](https://articles.zsxq.com/id_0259did6jf16.html)【`LLMs` 入门实战】基于 本地知识库 的高效 🤖`ElasticSearch-Langchain-Chatglm2`]

#### RAG 开源项目推荐 —— `Langchain-Chatchat` 篇

+ 介绍：`Langchain-Chatchat`（原`Langchain-ChatGLM`）基于 `Langchain` 与 `ChatGLM` 等语言模型的本地知识库问答 | `Langchain-Chatchat` (formerly `langchain-ChatGLM`), local knowledge based `LLM` (like `ChatGLM`) QA app with `langchain`

+ 项目学习：

+ [【【`LLMs` 入门实战】基于 本地知识库 的高效 🤖](https://github.com/chatchat-space/Langchain-Chatchat)`Langchain-Chatchat`]

`https://github.com/km1994/AwesomeRAG`

#
