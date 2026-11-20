
![](img/25a10b857dd28d927a01284bb4e16f70.png)

`Proposition级`级别的检索比`语句级`级别的检索和`篇章级`级别的检索分别高35%和22.5%（显著提高）。

# 索引与检索

## 特定嵌入

### 领域特定 & 高级嵌入模型

`Fine-tuning`：对`嵌入模型`进行微调可以帮助改进`RAG pipeline`检索相关文档的能力。这里，我们使用`LLM`生成的查询、文本语料库以及两者之间的交叉参考映射来嵌入模型理解需要查找的语料库。微调嵌入模型可以帮助提升大约5~10%的表现。

下面是Jerry Liu对微调嵌入模型的建议：

1. 在项目开始时，需要在嵌入文档前对基础模型进行微调
2. 由于生产环境中的文档分布可能会发生变化，因此在微调查询适配器(`query adapter`)时需要确保嵌入的文档不会发生变化。

![](img/3d49f66be337c2f0c72131f9b9fc5efa.png)

### `ColBERT`

`ColBERT`：这是一个检索模型，它能够基于`BERT`在大规模数据集上实现可扩展的搜索（毫秒级别）。这里，快速和准确检索是关键。

它会将每个段落编码为`token级`嵌入的矩阵（如下蓝色所示）。当执行一个搜索时，它会将用户查询编码为另一个`token级`嵌入的矩阵（如上绿色所示）。然后基于上下文匹配使用"可扩展的向量相似性(`MaxSim`)操作"来匹配查询段落。

## 分层索引

> [斯坦福大学研究人员](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Farxiv.org%2Fhtml%2F2401.18059v1%3Futm_source%3Ddiv.beehiiv.com%26utm_medium%3Dreferral%26utm_campaign%3Dadvanced-rag-series-indexing&source=article&objectId=2403938)基于不同层次的文档摘要树提出了`RAPTOR`模型，即通过对文本块的聚类进行摘要来实现更准确的检索。文本摘要涵盖了更大范围的上下文，跨越不同的尺度，包括主题理解和细粒度的内容。检索增强的文本摘要涵盖了更大范围不同主题理解和粒度的上下文。

![](img/6da108001fa99ba952a98e788e618e21.png)

> 通过`递归`摘要进行`检索`可以显著提升模型表现。"在涉及`复杂多步骤推理`的问答任务中，我们展示了最佳结果。例如，通过将`RAPTOR`检索与`GPT-4`相结合，我们可以在`QuALITY`基准测试的最佳表现上提高20%的绝对准确率。"

`llamaindex`提供了这种[实现](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fgithub.com%2Frun-llama%2Fllama_index%2Fblob%2Fmain%2Fllama-index-packs%2Fllama-index-packs-raptor%2Fexamples%2Fraptor.ipynb&source=article&objectId=2403938)。

## 检索

> 检索可以看做是对索引到的数据的进一步提炼。

> 在完成`数据检索`之后，下一步需要根据用户的请求来获取相关数据。最常见和最直接的方法是从之前索引的数据（最近的邻居）中识别并获取与[用户查询](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fweaviate.io%2Fblog%2Fdistance-metrics-in-vector-search%3Futm_source%3Ddiv.beehiiv.com%26utm_medium%3Dreferral%26utm_campaign%3Dadvanced-rag-series-retrieval&source=article&objectId=2403938)在语义上最接近的`chunks`。类似如下向量空间：

![](img/e0b511876676bcacd9ad72385779b524.png)

`检索`并不是一步到位的，而是从`查询转换`开始的一系列步骤，以及如何通过`检索`来提升获取到相关的`chunks`之后的检索质量。假设我们已经根据`相似性搜索`，从向量数据库中找到前 k 个 `chunks`，但由于大部分`chunks`存在重复内容或无法适应`LLM`的上下文窗口。因此在将`chunks`传递给`LLM`之前，我们需要通过一些检索技术来提升上下文的质量。

`LLMs`的世界中，并不存在一劳永逸的方法，需要根据使用场景和`chunks`的特性来找到合适的技术。下面是一些典型的方法：

### Ranking

如果我们想要从数据库的`chunk`中查找答案，可以选择`Reranking`，它是一种可以给`LLM`提供最相关上下文的有效策略。有多种实现方式：

+ 提升多样性：最常见的方法是[最大边缘相关性（Maximum Marginal Relevance-MMR）](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fwww.cs.cmu.edu%2F~jgc%2Fpublication%2FThe_Use_MMR_Diversity_Based_LTMIR_1998.pdf%3Futm_source%3Ddiv.beehiiv.com%26utm_medium%3Dreferral%26utm_campaign%3Dadvanced-rag-series-retrieval&source=article&objectId=2403938)，这可以通过因子 a) 与请求的相似度或 b) 与已选的文档的距离等因子来实现。

这里有必要提一下`Haystack`的`DiversityRanker`：

1. 首先计算每个文档的`嵌入向量`，然后使用一个`sentence-transformer`模型进行查询搜索
2. 将语义上和`查询`最接近的文档作为第一个选择的文档
3. 对于剩下的每个文档，会计算和所选择文档的平均相似性
4. 然后选择和所选择文档最不相似的文档
5. 然后重复上述步骤，直到选出所有文档，这样就得到了一个整体多样性从高到低排序的文档列表。

+ 下面介绍了几种重排序的方式，称为`reranker`。

![](img/502186d7b4eca709025c4e9ae790fa00.png)

![](img/8b526487804c2bfe2025c4e9ae790fa00.png)

1. `LostInTheMiddleReranker`
2. `CohereRerank`
3. `bge-rerank`
4. `RankGPT`

> `LostInTheMiddleReranker`：这是`Haystack`的另一个解决`LLM`不擅长从文档中间检索信息的问题的方法。它通过重排序来让最佳的文档位于上下文窗口的开头和结尾。建议在相关性和多样性之后再使用该`Reranker`。

![](img/f235fd57aa8742e8715efdd33f084177.png)

> [`CohereRerank`](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Ftxt.cohere.com%2Frerank%2F%3Futm_source%3Ddiv.beehiiv.com%26utm_medium%3Dreferral%26utm_campaign%3Dadvanced-rag-series-retrieval&source=article&objectId=2403938)：通过`Cohere`的`Rerank endpoint`实现，它会将一开始的搜索结果和用户的请求进行比较，并基于请求文本和文档之间的语义相似性来重新计算结果，而不仅仅根据向量的查询结果。

![](img/a6039c814c91d89dae44066ddd1fc506.png)

> [`bge-rerank`](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fwww.llamaindex.ai%2Fblog%2Fboosting-rag-picking-the-best-embedding-reranker-models-42d079022e83%3Futm_source%3Ddiv.beehiiv.com%26utm_medium%3Dreferral%26utm_campaign%3Dadvanced-rag-series-retrieval&source=article&objectId=2403938)：除了要选择适合你的数据的嵌入模型外，还需要重点关注可以和该嵌入模型配合使用的检索器(`retriever`)。我们使用命中率和平均倒数排名(`Mean Reciprocal Rank-MRR`)作为`retriever`的评估指标。命中率是指在前 k 个检索到的`chunks`中找到正确答案的频率，`MRR`是排名中最相关的文档在排名中的位置。从下面可以看到，`JinaAI-Base`嵌入中用到的`bge-rerank-large` 或 `cohere-reranker` 看起来非常适用于该数据集。下表中需要注意的是，嵌入模型并不是最好的`rerankers`。`Jina`最擅长嵌入，而`bge reranker`则最擅长重新排序。

![](img/bdd0bb563e6bd85baae2b417c06741b5.png)

> `mxbai-rerank-v1`：最新的一种重排序模型是由`Mixedbread`团队开发的开源项目，称为`SOTA`。其表现声称要优于`Cohere`和`bge-large`。

![](img/a0d913113bc243c1a8a9b593f7183243.png)

> [`RankGPT`](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Farxiv.org%2Fpdf%2F2304.09542.pdf&source=article&objectId=2403938)：这是使用现有的`LLMs`（如`GPT3.5`）重排检索文档的方法之一，且重排质量好于`Cohere`。为了解决检索上下文大于`LLM`的上下文窗口的问题，该方法使用了一种"滑动窗口"策略，即在滑动窗口内实现渐进排名。这种方式击败了其他大部分`reranker`，其中包括`Cohere`。这种方式下需要注意的是延迟和成本，因此适用于优化小型开源模型。

![](img/216d653e5aabf8f2fbe1c28553655dc6.png)

### RAG-fusion

### Prompt Compression

这里有必要介绍一下`Prompt Compression`，它和`Reranking`关系密切。这是一种通过压缩无关信息（即和用户查询无关）来降低检索到的文档的技术，有如下方式：

+ [`LongLLMLingua`](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Farxiv.org%2Fabs%2F2310.06839%3Futm_source%3Ddiv.beehiiv.com%26utm_medium%3Dreferral%26utm_campaign%3Dadvanced-rag-series-retrieval&source=article&objectId=2403938)：该方式基于`Selective-Co`ntext 和 `LLMLingua` 框架，是用于提示压缩的`SOTA`方法，针对成本和延迟进行了优化。除此之外，`LongLLMLingua`采用了一种"采用问题感知由粗到细的压缩方法、文档重新排序机制、动态压缩比例以及压缩后的子序列恢复等策略来提升大型语言模型对关键信息的感知能力"的方式来提升检索质量。该方法特别适用于长上下文文档，解决"中间遗失"问题。

![](img/2c81a99e58ade93fd6dba933f91aebb4.png)
