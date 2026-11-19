
# 领英最佳实践：用于客服问答的使用知识图谱的RAG

> 来源：https://agijuejin.feishu.cn/docx/FwWadtqtdo6bYLx6jHYcVLSKnIh
>
> 作者：lucas大叔
>
> 原文：https://zhuanlan.zhihu.com/p/696141377

《Retrieval-Augmented Generation with Knowledge Graphs for Customer Service Question Answering》是LinkedIn 2024年发表的一篇论文，针对原生RAG技术在客服问答中的局限性，提出了一种将RAG与知识图谱相结合的客服问答解决方案，有效改善了检索准确性和回答质量。通过在基准数据集上进行实证评估，利用关键检索（MRR、Recall@K、NDCG@K）和文本生成（BLEU、ROUGE、METEOR）指标，该方法

+ 在MRR上的表现优于基准方法77.6%

+ 在BLEU上优于基准方法0.32

+ 在领英的客户服务团队中部署了大约六个月，将每个问题的中位解决时间减少了28.6%

---

## 问题背景

在客服场景中，客户查询的问题通常与之前已解决的问题类似，快速准确地检索相关的过往工单对于高效地解决此类查询至关重要。当前基于RAG的客服问答通常分为两个阶段：首先，历史问题工单被作为纯文本分割成较小的片段以适应`embedding`模型的上下文长度约束；然后每个片段被转换为向量用于检索。其次，在问答阶段，系统检索最相关的片段，并将它们作为上下文提供给LLM生成答案。尽管这种方法很直接，但也存在一些局限性：

限制1 - 忽视结构导致检索准确性下降：

问题跟踪文档，如Jira，具有固有的结构并且彼此相互关联，其中包括诸如“问题A与问题B相关/复制/由问题B引起”之类的引用。将文档压缩成文本片段的传统方法导致了重要信息的丢失。提出的方法将问题工单解析为树，并进一步连接单个问题工单以形成相互关联的图，从而保持了实体之间的内在关系，实现了较高的检索性能。

限制2 - 由于分割而导致的答案质量降低：

将大量的问题工单分割成固定长度的片段以适应`embedding`模型的上下文长度约束可能导致相关内容的断开，从而导致答案不完整。例如，一个问题工单在开头描述了问题，在结尾描述了解决方案，在文本分割过程中可能被拆分，导致关键部分的解决方案被省略。基于图的解析方法通过保留工单各部分的逻辑连贯性来克服这一问题，确保提供完整且高质量的答复。

## 方法介绍

引入一种基于LLM的客服问答系统，该系统将检索增强生成（RAG）与知识图谱（KG）无缝整合。系统（见下图）包括两个阶段：首先，在知识图谱构建阶段，系统从历史客服问题工单构建了一个全面的知识图谱。它整合每个问题的树形表示，并根据关系上下文进行相互链接。它还为每个节点生成`embedding`以便后续语义搜索。其次，在问答阶段，解析用户查询并识别命名实体和意图。然后，它在知识图谱内搜索以识别生成答案所需的相关子图。

![](img/d77b00ee1d21058d6513648006df1a14.png)

### 图谱构建

**定义图结构**

在定义历史问题表示的知识图谱结构时，采用双层架构将问题内部和问题间的关系分开，如上图所示。内部问题树`T(N,E,R)`将每个工单`t_i`建模为一棵树，其中每个节点`n ∈ N`，由唯一组合`(t_i, n)`标识，对应于工单`t_i`的不同部分，并且每条边`e ∈ E`和`r ∈ R`表示这些部分之间的层次关系和类型关系。问题间图`G(T,E,R)`表示不同工单之间的连接网络，包括显式链接`Eexp`（在问题跟踪工单中定义）和隐式连接`Eimp`（从工单之间的语义相似性推导而来）。对于隐式连接，我们利用工单标题向量之间的余弦相似度，这是一种适用于特定用例的方法，不同的使用场景需要设计不同的方法创建隐式连接。

例如，上图描述了工单`ENT-22970`的树形结构，其中节点表示`SUMMARY`、`DESCRIPTION`和`PRIORITY`等部分。它展示了与`PORT-133061`之间的直接`clone`链接，表示显式`clone`关系。此外，根据语义相似性，它还与`ENT-1744`和`ENT-3547`之间存在隐式连接。

**图谱构建**

图谱构建分为两个阶段：工单内部解析和工单间连接。1) 工单内部解析阶段：这个阶段将每个基于文本的工单`t_i`转换为树形表示`T`。我们采用混合方法，首先利用基于规则的提取来处理预定义字段，例如通过关键字识别的代码部分。随后，对于不适合基于规则解析的文本，使用LLM进行解析。LLM由`YAML`模板`T_template`指导，该模板以图方式表示客户支持常用的工单部分。2) 工单间连接阶段：单颗树`T`被整合成全面的图`G`。显式连接`Eexp`根据工单内部指定的连接进行描述，例如在Jira中的指定字段。隐式连接`Eimp`是从工单标题之间的文本语义相似性中推断出来的，利用`embedding`技术和阈值机制来确定每个问题工单的最相关工单。

![](img/fffebab3658faab109f9e8fbc67a1e12.png)

**embedding生成**

为了支持在线基于`embedding`的检索，使用预训练的文本嵌入模型，如`BERT`和`E5`，为图节点值生成嵌入，特别针对文本丰富部分的节点，如`"issue summary"`、`"issue description"`和`"steps to reproduce"`等。然后，这些`embedding`被存储在向量数据库中。对于大多数情况，每个节点内的文本长度可以满足文本嵌入模型的上下文长度限制，但对于某些较长的文本，可以将文本分成较小的块进行单独的嵌入，而不必担心质量，因为这些文本都属于同一部分。

### 检索与问答

**查询实体识别和`意图检测`**



在这[一步中，从每个](https://www.zhihu.com/search?q=%E6%9F%A5%E8%AF%A2%E6%84%8F%E5%9B%BE%E9%9B%86%E5%90%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D)用户查询 `q` 中提取类型为 [Map](https://www.zhihu.com/search?q=%E9%94%AE%E5%80%BC%E5%AF%B9&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D) `Map(\mathcal{N} → \mathcal{V})` 的命名实体 `P` `\mathcal{P}` 和 查询意图集合 `I` `\mathcal{I}` 。该方法涉及将每个查询 `q` 解析为 键值对 ，其中查询中提及的每个键 `n` 对应于图模板 `Ttemplate` `\mathcal{T}_{template}` 中的一个元素，而值 `v` 表示从查询中提取的信息。同时，查询意图 `I` `\mathcal{I}` 包括图模板 `Ttemplate` `\mathcal{T}_{template}` 中提及的查询旨在解决的实体。在解析过程中，我们利用LLM与合适的prompt。例如，给定查询 `q` = " How to reproduce the login issue where a user can’t log in to LinkedIn? "，提取的实体是 `P` `\mathcal{P}` = `Map("issue summary" → "login issue", "issue description" → "user can’t log in to LinkedIn")` ，而意图集合是 `I` `\mathcal{I}` = `Set("fix solution")` 。该方法利用LLM强大的理解和解释能力，展示了在适应各种查询表达形式方面的显著灵活性。

```
P,I=LLM(q,Ttemplate,prompt)
P,I=LLM(q,T_{template},prompt)
```

## 基于嵌入的子图检索

[我们的方法从](https://www.zhihu.com/search?q=%E5%B7%A5%E5%8D%95%E8%AF%86%E5%88%AB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D)知识图谱中提取与用户提供的具体信息（如“ issue description ”和“issue summary”）以及用户意图（如“fix solution”）相关的子图。该过程包括两个主要步骤：基于嵌入的 工单识别 和LLM驱动的子图提取。

[在基于嵌入的工单识别步骤中](https://www.zhihu.com/search?q=%E5%AE%9E%E4%BD%93%E9%9B%86%E5%90%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D)，利用从用户查询中提取的命名 实体集合 `P` `\mathcal{P}` ，确定了与用户查询相关的前 `Kticket` `K_{ticket}` 最相关的历史问题工单。对于每个实体对 (,)∈`P` ，通过预训练的文本嵌入计算实体值 `v` 与所有与部分 `k` 对应的图节点 `n` 之间的余弦相似度。通过对属于同一工单的节点的贡献进行求和，将这些节点级别的分数汇总到工单级别，对前 `Kticket` `K_{ticket}` 工单进行排名和选择。这种方法假设多个查询实体的出现表明了相关的链接，从而提高了检索的准确性。

![](img/75fedc81b2b7a1b74efd81ed27d6bc4b.png)

在LLM驱动的子图提取步骤中，系统首先重新构造原始用户查询 `q` 以包含检索到的工单ID；修改后的查询 `q'` 然后被翻译成图数据库语言，例如用于Neo4j问答的Cypher。

例如，从初始查询 `q` = " how to reproduce the issue where user saw ’csv upload error in updating user email’[with](https://www.zhihu.com/search?q=%E8%B7%A8%E8%B6%8A%E5%AD%90%E5%9B%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D) major priority due to a data issue "，查询被重新构造为" how to reproduce ’ENT-22970 ’，然后转换为Cypher查询

```
MATCH (j:Ticket {ticket_ID: ’ENT-22970’})-[:HAS_DESCRIPTION]-> (description:Description)-[:HAS_STEPS_TO_REPRODUCE]-> (steps_to_reproduce:StepsToReproduce) RETURN steps_to_reproduce.value
```

值得注意的是，LLM驱动的查询构造具有足够的灵活性，可以检索 跨越子图 的信息，无论它们是否来自相同的树还是知识图谱中的不同树。

## 生成答案

答案通过将来自上节检索到的数据与[初始查询](https://www.zhihu.com/search?q=%E8%A7%A3%E7%A0%81%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D)进行[关联而综合](https://www.zhihu.com/search?q=%E5%9F%BA%E7%BA%BF%E6%96%87%E6%9C%AC&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D)生成。LLM作为 解码器 ，根据检索到的信息来制定对用户查询的响应。为了实现稳健的在线服务，如果查询执行遇到问题，会有一个回退机制，重新采用 基线文本 检索方法。

## 实验结果

评估采用了精心设计的“标准”数据集，[包含典型查询](https://www.zhihu.com/search?q=%E6%8E%A7%E5%88%B6%E7%BB%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D)、支持工单及其权威解决方案。 控制组 采用传统的基于文本的EBR方法，而 [实验组采用了本](https://www.zhihu.com/search?q=%E5%B9%B3%E5%9D%87%E5%80%92%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D)研究中概述的方法。对于两组实验，使用了相同的LLM（GPT-4），相同的嵌入模型（E5）。使用 平均倒数 排名（`MRR`）、`recall@K`和`NDCG@K`衡量检索效果。`MRR`衡量初始正确响应的平均倒数排名，`recall@K`确定在前K个选择中出现相关`item`的似然，而`NDCG@K`通过考虑`item`的位置和相关性来评估 排名质量 。对于问答性能，我们将“标准”解决方案与生成的响应进行了对比，利用了诸如`BLEU`、`ROUGE`和`METEOR`分数等指标。

检索和问答表现分别呈现在表1和表2中。在所有指标中，我们的方法表现出了持续的改进。值得注意的是，在MRR方面，我们的方法超越了基准77.6%，在 `BLEU` 分数上超过了0.32，证实了其更高的检索效率和问答准确性。

![](img/816609cf4e07b2ba1b5bc1651105721d.png)

我们将方法部署在LinkedIn的客服团队中，涵盖了多个产品线。团队被随机分成两组：一组使用我们的系统，另一组坚持传统的手动方法。如表3所示，使用我们系统的组取得了显著的收益，将每个问题的中位解决时间减少了28.6%。这突显了系统在提高客户服务效率方面的有效性。

![](img/92116af2aea6509aa458cb1d80acfd6e.png)

## 总结

[客服系统](https://www.zhihu.com/search?q=%E5%AE%A2%E6%9C%8D%E7%B3%BB%E7%BB%9F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A696141377%7D)通常应用于客户支持、IT服务管理等领域，可以自动化处理常见的技术问题，提高问题解决的效率和准确性。领英的实践表明通过知识图谱的构建和高效的检索机制，系统能够理解和处理复杂的用户查询，提供准确的解决方案。
