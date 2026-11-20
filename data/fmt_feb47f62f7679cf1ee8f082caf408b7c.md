
# Self-RAG: 大模型集检索与生成能力的自主调优方法

> 来源：https://agijuejin.feishu.cn/docx/Vzh3dApEFoKytXxQaOqcXrawnZx
>
> 作者：王大锤
>
> 原文：https://zhuanlan.zhihu.com/p/681122615

## 一、背景

![](img/f8e2d0a9dc69d8f89ef930bbde51fa47.png)

[检索增强生成](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E5%A2%9E%E5%BC%BA%E7%94%9F%E6%88%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)(Retrieval-Augmented Generation, RAG)，是一个为大模型提供外部知识源的策略，使得大模型具备从指定的知识库中进行检索，并结合上下文信息，生成相对高质量的回复内容，减少模型幻觉问题。

### 1.1 示例场景

参考我们做的 [智能座舱](https://www.zhihu.com/search?q=%E6%99%BA%E8%83%BD%E5%BA%A7%E8%88%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)中的一个示例场景，比如我们希望大模型能回答关于车机系统的相关问题。大模型在没有见过我们公司《车机 使用手册》的情况下（事实上，企业私有数据一般不会被大模型的参数所覆盖），几乎回答不正确，可能会出现以下情况:

![](img/141630a6d3cc75ba5359a98f1bffa9a7.png)

但是通过`RAG`的能力，能够让大模型结合企业私有数据，完成特定领域的知识问答，实现对以下业务目标的支持：

![](img/b73954118eb82fefc669b8de08e278a2.png)

### 1.2 RAG优点

从本质上讲，`LLM`文本生成的原理是基于`token by token`的概率模型。利用`RAG`技术，可以从以下几个方向提升`LLM`的能力。

+ [`模型幻觉`](https://www.zhihu.com/search?q=%E6%A8%A1%E5%9E%8B%E5%B9%BB%E8%A7%89&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D): 利用`RAG`技术，能提醒`LLM`利用文档中的内容信息进行回复，缓解`LLM`"一本正经胡说八道"的情况。

+ `实效性问题`: `LLM`的参数量越大，进行训练的成本就越高。如果部分数据存在高频的信息更新，`LLM`在生成这类问题的回答时容易出错。

+ `数据安全问题`: 对于商用的业务场景而言，企业的数据一般是私有化的，在`LLM`没有利用企业私有化数据学习的前提下，只需要`LLM`对企业数据完成一个归纳总结功能。

基于此，能总结出`RAG`具备以下优点:

+ `良好的可拓展性`: 降低训练成本，允许扩展私有化知识。

+ `准确性`: 大模型从私有化数据推理得出答案，缓解模型幻觉问题。

+ `可控性`: 允许更新或者定制化知识。

+ `可解释性`: 检索到的 [信息内容](https://www.zhihu.com/search?q=%E4%BF%A1%E6%81%AF%E5%86%85%E5%AE%B9&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)来源于 [知识库](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)。

### 1.3 RAG局限性

尽管 `RAG` 在很多例子中取得了很好的效果，仍然面临着不少挑战，主要体现在以下方面：

+ [检索效果](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E6%95%88%E6%9E%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)依赖 `embedding` 和检索算法，目前可能检索到无关信息，反而对输出有负面影响；

+ 大模型如何利用检索到的信息仍是黑盒的，可能仍存在不准确（甚至生成的文本与检索信息相冲突）；

+ 对所有任务都无差别检索 `k` 个文本片段，效率不高，同时会大大增加模型输入的长度；

+ [无法引用](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E6%BA%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)来源，也因此无法精准地查证事实，检索的真实性取决于 数据源 及检索算法。

## 二、改进方法

### 2.1 `Multi-Vector Retriever`

多向量检索器(`Multi-Vector Retriever`)是`LangChain`推出的一个关键工具，用于优化`RAG`的过程。 多向量检索器 的核心想法是将我们想要用于答案合成的文档与我们想要用于检索的参考文献分开。这允许系统为 搜索优化 文档的版本而不失去答案合成时的上下文。

简单来说就是将一个文档分解为较长的几个逻辑较为完整和独立的部分，例如包括不同的文本、表格甚至是图片都可以，然后分解后的文档使用摘要的方式进行总结，这个摘要需要可以明确覆盖相关内容。然后摘要进行 `向量化`，检索的时候直接检索摘要，一旦匹配，即可将摘要背后的完整文档作为上下文输入给大模型。

![](img/c663411e73962bccd984368fd616a219.png)

### 2.2 `Self-Instruct`

人工标注一些机器的理想输出， 对齐(`aligning`)`模型与用户意图`。一般大模型经过标注的 `指令数据精调` (`instruct-tuning`)后，能更好的理解和执行指令（OpenAI据此开发了`InstructGPT` [1] ）。数据集的格式通常包括任务指令、输入、预期模型输出，均需人工标注。

精调后模型的执行表现和任务 `泛化能力` 与训练数据的 `量级、多样性` 直接相关。但人工标注这么大（通常x万xx万起）需要很高成本，且需要标注者有创造力、多领域的专业知识，否则很难达到模型训练的要求。因此，有研究者开始探索如何利用大模型直接生成 `指令精调` 的 `训练集` ，即 `自主生成的指令精调`（`Self-instruct tuning`）。

![](img/7adccafb7444c80da88ea03369cc31d5.png)

### 2.3 `RA-DIT`

`RA-DIT` 方法分别对 `LLM` 和检索器进行微调。 更新 `LLM` 以最大限度地提高在给定检索增强指令的情况下正确答案的概率，同时更新检索器以最大限度地减少文档与查询在语义上相似（相关）的程度。通过这种方式，使 `LLM` 更好地利用相关背景知识，并训练 `LLM` 即使在检索错误块的情况下也能产生准确的预测，从而使模型能够依赖自己的知识。

### 2.4 `Query Transformations`

在某些情况下，用户的 `query` 可能出现表述不清、需求复杂、内容无关等问题，为了解决这些问题， `查询转换` （`Query Transformations`）的方案利用了 `大型语言模型` (`LLM`)的强大能力，通过某种提示或方法将原始的用户问题转换或重写为更合适的、能够更准确地返回所需结果的查询。`LLM`的能力确保了转换后的查询更有可能从文档或数据中获取相关和准确的答案。

![](img/939241d386317cf6df67d193e591790d.png)

## 三、`Self-RAG`

### 3.1 `Self-RAG`概述

前面的 `RAG` 方法都遵循着共同的范式，即：

`query + context \rightarrow LLM`，

其中 `query` 表示用户的输入，`context` 表示检索获得的补充信息，然后共同输入到 `LLM` 中，可以认为这是一种检索前置的被动的增强方式。

相比之下，`Self-RAG` 则是更加主动和智能的实现方式，主要步骤概括如下：

1. 判断是否需要额外检索事实性信息（`retrieve on demand`），仅当有需要时才召回

1. 平行处理每个片段：生产 `prompt` + 一个片段的生成结果

1. 使用 `反思字段`，检查输出是否相关，选择最符合需要的片段；

1. 再重复检索

1. 生成结果会引用相关片段，以及输出结果是否符合该片段，便于查证事实。

### 3.2 `Self-RAG`流程

对比于传统的`RAG`，`Self-RAG`的 流程图 如下:

![](img/a36ba53c2b3e3adc823195f58ae91368.png)

二者主要的差别在于:

**传统`RAG`**:

+ 无差别检索`k`个文档片段

+ 同时将`k`个片段加入`prompt`生成输出

**`Self-RAG`**:

+ 判断是否需要额外检索事实型信息，仅当有需要时才召回

+ 平行处理每个片段，生产`prompt`及每个片段的生成结果

+ 使用`反思字段`，检查输出是否相关，选择最符合需要的片段

+ 再重复检索

+ 生成结果会引用相关片段，以及输出结果是否符合相关片段，便于查证事实

**实现细节**:

`Reflection token`反思字段: 通过生成反思字段这一特殊标记来检查输出。这些字段会分为`Retrieve`和`Critique`两种类型，会标示：检查是否有检索的必要，完成检索后检查输出的相关性、完整性、 `检索片段` 是否支持输出的观点。模型会基于原有词库和反思字段来生成下一个`token`。

![](img/2451548050ea60b82d8a77ebb421221e.png)
