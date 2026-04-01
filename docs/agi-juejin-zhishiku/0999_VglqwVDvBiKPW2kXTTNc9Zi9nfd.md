# Self-RAG: 大模型集检索与生成能力的自主调优方法

> 来源：[https://agijuejin.feishu.cn/docx/Vzh3dApEFoKytXxQaOqcXrawnZx](https://agijuejin.feishu.cn/docx/Vzh3dApEFoKytXxQaOqcXrawnZx)

> 作者：王大锤

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/681122615)hihu.com/p/681122...

## 一. 背景

![](img/f8e2d0a9dc69d8f89ef930bbde51fa47.png)

[检索增强生成 ](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E5%A2%9E%E5%BC%BA%E7%94%9F%E6%88%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)(Retrieval-Augmented Generation, RAG), 是一个为大模型提供外部知识源的策略，使得大模型具备从指定的知识库中进行检索，并结合上下文信息，生成相对高质量的回复内容，减少模型幻觉问题。

### **1.1 示例场景**

参考我们做的 [智能座舱 ](https://www.zhihu.com/search?q=%E6%99%BA%E8%83%BD%E5%BA%A7%E8%88%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)中的一个示例场景，比如我们希望大模型能回答关于车机系统的相[关问题。大](https://www.zhihu.com/search?q=%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)模型在没有见过我们公司《车机 使用手册 》的情况下（事实上，企业私有数据一般不会被大模型的参数所覆盖），几乎回答不正确，可能会出现以下情况:

![](img/141630a6d3cc75ba5359a98f1bffa9a7.png)

但是通过RAG的能力，能够让大模型结合企业私有数据，完成特定领域的知识问答，实现对以下业务目标的支持：

![](img/b73954118eb82fefc669b8de08e278a2.png)

### 1.2 RAG优点

从本质上讲，LLM文本生成的原理是基于token by token的概率模型。利用RAG技术，可以从以下几个方向提升LLM的能力。

+   [**模型幻觉 **](https://www.zhihu.com/search?q=%E6%A8%A1%E5%9E%8B%E5%B9%BB%E8%A7%89&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)**问题 **: 利用RAG技术，能提醒LLM利用文档中的内容信息进行回复，缓解LLM"一本正经胡说八道"的情况。

+   **实效性问题 **: LLM的参数量越大，进行训练的成本就越高。如果部分数据存在高频的信息更新，LLM在生成这类问题的回答时容易出错。

+   **数据安全问题 **: 对于商用的业务场景而言，企业的数据一般是私有化的，在LLM没有利用企业私有化数据学习的前提下，只需要LLM对企业数据完成一个归纳总结功能。

基于此，能总结出RAG具备以下优点:

+   **良好的可拓展性 **: 降低训练成本，允许扩展私有化知识。

+   **准确性 **: 大模型从私有化数据推理得出答案，缓解模型幻觉问题。

+   **可控性 **: 允许更新或者定制化知识。

+   **可解释性 **: 检索到的 [信息内容 ](https://www.zhihu.com/search?q=%E4%BF%A1%E6%81%AF%E5%86%85%E5%AE%B9&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)来源于 [知识库 ](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)。

### 1.3 RAG局限性

尽管 RAG 在很多例子中取得了很好的效果，仍然面临着不少挑战，主要体现在以下方面：

+   [检索效果 ](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E6%95%88%E6%9E%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)依赖 embedding 和检索算法，目前可能检索到无关信息，反而对输出有负面影响；

+   大模型如何利用检索到的信息仍是黑盒的，可能仍存在不准确（甚至生成的文本与检索信息相冲突）；

+   对所有任务都无差别检索 k 个文本片段，效率不高，同时会大大增加模型输入的长度；

+   [无法引用](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E6%BA%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)来源，也因此无法精准地查证事实，检索的真实性取决于 数据源 及检索算法。

## 二. 改进方法

### 2.1 Multi-Vector Retriever

多[向量检索器(M](https://www.zhihu.com/search?q=%E5%A4%9A%E5%90%91%E9%87%8F%E6%A3%80%E7%B4%A2%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)ulti-Vector Retriever)是L[angCh](https://www.zhihu.com/search?q=%E6%90%9C%E7%B4%A2%E4%BC%98%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)ain推出的一个关键工具，用于优化RAG的过程。 多向量检索器 的核心想法是将我们想要用于答案合成的文档与我们想要用于检索的参考文献分开。这允许系统为 搜索优化 文档的版本而不失去答案合成时的上下文。

简单[来说就是](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)将一个文档分解为较长的几个逻辑较为完整和独立的部分，例如包括不同的文本、表格甚至是图片都可以，然后分解后的文档使用摘要的方式进行总结，这个摘要需要可以明确覆盖相关内容。然后摘要进行 向量化 ，检索的时候直接检索摘要，一旦匹配，即可将摘要背后的完整文档作为上下文输入给大模型。

![](img/c663411e73962bccd984368fd616a219.png)

### 2.2 Self-Instruct

人工标注一些机器的理想输出， 对齐(aligning)[模型与用户意图](https://www.zhihu.com/search?q=%E6%8C%87%E4%BB%A4%E6%95%B0%E6%8D%AE%E7%B2%BE%E8%B0%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D) 。一般大模型经过标注的 指令数据精调 (instruct-tuning)后，能更好的理解和执行指令（OpenAI据此开发了InstructGPT [1] ）。数据集的格式通常包括任务指令、输入、预期模型输出，均需人工标注。

精调后模型的执行表现和任务 [泛化能力 ](https://www.zhihu.com/search?q=%E6%B3%9B%E5%8C%96%E8%83%BD%E5%8A%9B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)与训练数据的 **量级、多样性 **直接相关。但人工标注这么大（通常x万xx万起）需要很高成本，且需要标注者有创造力、多领域的专业[知识，否则](https://www.zhihu.com/search?q=%E6%8C%87%E4%BB%A4%E7%B2%BE%E8%B0%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)很难[达到模型](https://www.zhihu.com/search?q=%E8%AE%AD%E7%BB%83%E9%9B%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)训练的**要求。因此，有研究者开****始探**索如何利用大模型直接生成 指令精调 的 训练集 ，即 自主生成的指令精调（ Self-instruct tuning ）。

![](img/7adccafb7444c80da88ea03369cc31d5.png)

### 2.3 RA-DIT

RA-DIT 方法分别对 LLM 和检索器进行微调。 更新 LLM 以最大限度地提高在给定检索增强指令的情况下正确答案的概率，同时更新检索器以最大限度地减少文档与查询在语义上相似（相关）的程度。通过这种方式，使 LLM 更好地利用相关背景知识，并训练 LLM 即使在检索错误块的情况下也能产生准确的预测，从而使模型能够依赖自己的知识。

### 2.4 Query Transformations

在某些情况下，用户的 query 可能出现表述不清、需求复[杂、内容无](https://www.zhihu.com/search?q=%E6%9F%A5%E8%AF%A2%E8%BD%AC%E6%8D%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)[关等问题，为了](https://www.zhihu.com/search?q=%E5%A4%A7%E5%9E%8B%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)解决这些问题， 查询转换 （Query Transformations）的方案利用了 大型语言模型 (LLM)的强大能力，通过某种提示或方法将原始的用户问题转换或重写为更合适的、能够更准确地返回所需结果的查询。LLM的能力确保了转换后的查询更有可能从文档或数据中获取相关和准确的答案。

![](img/939241d386317cf6df67d193e591790d.png)

## 三. Self-RAG

### 3.1 Self-Rag概述

前面的 RAG 方法都遵循着共同的范式，即：

query + context \rightarrow LLM ，

其中 query 表示用户的输入，context 表示检索获得的补充信息，然后共同输入到 LLM 中，可以认为这是一种检索前置的被动的增强方式。

相比而言，Self-RAG 则是更加主动和智能的实现方式，主要步骤概括如下：

1.  判断是否需要额外检索事实性信息（retrieve on demand），仅当有需要时才召回

1.  平行处理每个片段：生产 [prompt ](https://www.zhihu.com/search?q=prompt&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)+一个片段的生成结果

1.  使用 [反思字段 ](https://www.zhihu.com/search?q=%E5%8F%8D%E6%80%9D%E5%AD%97%E6%AE%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)，检查输出是否相关，选择最符合需要的片段；

1.  再重复检索

1.  生成结果会引用相关片段，以及输出结果是否符合该片段，便于查证事实。

### 3.2 Self-RAG流程

[对比于传](https://www.zhihu.com/search?q=%E6%B5%81%E7%A8%8B%E5%9B%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)统的RAG，Self-RAG的 流程图 如下:

![](img/a36ba53c2b3e3adc823195f58ae91368.png)

二者主要的差别在于:

**传统RAG **:

+   无差别检索K个文档片段

+   同时将k个片段加入prompt生成输出

**Self-RAG **:

+   判断是否需要额外检索事实型信息，仅当有需要时才召回

+   平行处理每个片段，生产prompt及每个片段的生成结果

+   使用反思字段，检查输出是否相关，选择最符合需要的片段

+   再重复检索

+   生成结果会引用相关片段，以及输出结果是否符合相关片段，便于查证事实

**实现细节 **:

Re[flect](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E7%89%87%E6%AE%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)ion token反思字段: 通过生成反思字段这一特殊标记来检查输出。这些字段会分为Retrieve和Critique两种类型，会标示：检查是否有检索的必要，完成检索后检查输出的相关性、完整性、 检索片段 是否支持输出的观点。模型会基于原有词库和反思字段来生成下一个token。

![](img/2451548050ea60b82d8a77ebb421221e.png)

+   [生成模型 ](https://www.zhihu.com/search?q=%E7%94%9F%E6%88%90%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)M：使用的训练语料库包含1）检索器R召回的语料；2）评估模型C生成的反思字段。

+   先[训练评估模型](https://www.zhihu.com/search?q=GPT-4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)C，用于评估输出并生成反思字段。值得注意的是，在训练C时作者没有手动标注输入对应的反思字段，而是利用了 GPT-4 来为模型C生成训练数据  D_{critic} （对每一类反思字段都单独写 prompt+few-shot 案例来生成）。

+   **然后用C生成语料库来训练**M：C对每一个tok**en生成反思字段，** 将这些字段整合进训练集 ，使M能够在推理时 自己生成反思标记 （PPO等方式采用的是独立的奖励模型），而不依赖于额外的一个模型。

![](img/ca54a6ec36506fe17bbd6c7ad2216f78.png)

### 3.3 相关实验

[**封闭集任务 **](https://www.zhihu.com/search?q=%E5%B0%81%E9%97%AD%E9%9B%86%E4%BB%BB%E5%8A%A1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)(Closed-set tasks) : 包括两个数据集，即 公共卫生事实验证数据集（PubHealth） 和从科学考试中创建的 多项选择推理数据集（ARC-Challenge） 。使用准确性作为评估指标。

**短文生成任务 (Short-for****m gene**ration tasks) : 包括两个开放领域问答数据集 PopQA 和 TriviaQA-unfiltered ，系统需要回答有关事实知识的任意问题。

[**长文生成任务 **](https://www.zhihu.com/search?q=%E9%95%BF%E6%96%87%E7%94%9F%E6%88%90%E4%BB%BB%E5%8A%A1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)(Long-form generation tasks) :** 包括传**记生成任务和长形式问**答任务A**LCE-ASQA。使用**FactSc**or**e评估**[**传记，使**](https://www.zhihu.com/search?q=%E5%8F%AC%E5%9B%9E%E7%8E%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)用官方的 准确性 （str-em）、 流畅性 （基于MAUVE）和 引用精确度 、 引用 召回率 指标评估ASQA。

[**无检索基准 **](https://www.zhihu.com/search?q=%E6%97%A0%E6%A3%80%E7%B4%A2%E5%9F%BA%E5%87%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)(Baselines without retrievals) : 评估了公开可用的预训练LLMs以及使用私有数据进行训练和强化的模型，如ChatGPT和Llama2-chat。

带检索基准(Baselines with retrievals) : 评估了在测试时或训练期间增强检索的模型。包括标准的RAG基准测试，其中语言模型在查询前加上了顶级检索文档。

### 3.4 实验分析

SE[LF-RA](https://www.zhihu.com/search?q=%E6%8F%90%E7%A4%BA%E5%B7%A5%E7%A8%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A681122615%7D)G 在所有任务中表现出显著的性能优势，甚至在PubHealth、PopQA、传记生成和ASQA（Rouge和MAUVE）任务中超越了ChatGPT。SELF-RAG 还显著优于采用复杂 提示工程 的同时期方法，特别是在传记生成任务中，其7B和13B模型优于同时期的CoVE。

**消融研究 **：作者进行了一系列 消融研究(ablation studies**) ，以识别哪些因素发挥关键作用。并**评估**了两种与SELF-RAG不同训练的模型变体：** No Retriever 采用标准的指令跟随方法训练LM，不涉及检索的段落 和 No Critic 训练的LM总是使用顶部一个检索到的文档进行增强，不包含反思标记 。对推理时算法进行了消融，包括禁用推理时的检索；硬性约束指的是模型在检索=是（Retrieve =Yes）时进行检索，而不是使用自适应阈值；Retrieve top 1 总是检索并使用顶部一个文档。

**推理时定制的影响 **: 提出的框架有一个关键优势是它使实验能够控制每种评判类型对最终生成采样的影响。实验分析了在推理时对7B模型顶部不同参数权重的影响，特别是在ASQA上，其中考虑了多个评估方面。图3b显示了改变ISSUP权重项的影响，ISSUP批判输出是否得到文本段落的支持。如图所示，增加权重对模型的引用精度产生积极影响，因为这更强调模型生成是否得到证据支持。相反，较大的权重导致MAUVE分数降低：当生成变得更长和更流畅时，通常会有更多未完全得到引用支持的声明，这与Liu等人（2023a）的发现一致。我们的框架允许实践者在测试时通过调整这些参数来选择和定制模型的行为，而无需额外训练。

人类评估（Human Evaluations） : 实验对SELF-RAG的输出以及预测的反思标记的可靠性进行了小规模的人类评估。特别是从PopQA和Bio结果中抽取了50个样本。人肉评估模型输出是否合理（即输出是否是对问题的合理且主题相关的回应，就像它发生在对话中一样）和支持（即提供的证据是否足以验证答案的有效性）。

![](img/665bac5094aae9bc132b48a423bfbd54.png)

## 参考

1.  https://arxiv.org/abs/2203.02155