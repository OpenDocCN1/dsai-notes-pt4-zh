
# AutoHyDE：使HyDE更好地用于高级LLM RAG

> 来源：[https://agijuejin.feishu.cn/docx/Q386d0HeZobP4WxrRfTclYBOnYb](https://agijuejin.feishu.cn/docx/Q386d0HeZobP4WxrRfTclYBOnYb)

> 作者：lucas大叔

> 原文：[https://zhuanlan.zhihu.com/p/714886313](https://zhuanlan.zhihu.com/p/714886313)

## 摘要

本文介绍了`AutoHyDE`，一个旨在提高高级`LLM` `RAG`假设文档嵌入（`HyDE`）的有效性、覆盖率和适用性的半监督框架。`AutoHyDE`能够自动发现索引文档中的潜在相关模式，并生成直接表示这些相关性模式的假设文档。

本文还讨论了当前`HyDE`方法的局限性，并提出了`AutoHyDE`作为这些局限性的解决方案。文章还深入探讨了原始`HyDE`论文、`LangChain`实现和`AutoHyDE`的技术实现。

---

## 介绍

在`RAG`领域，`HyDE`已被证明是一种强大的查询重写形式，可提高检索文档的相关性。

对于初学者来说，虽然传统检索只是使用原始输入来创建用于检索的嵌入向量，但`HyDE`是一种生成与要检索的索引文档的嵌入空间更相关的嵌入向量的方法。

1. 根据用户输入创建假设文档
2. 将假设文档转换为嵌入
3. 使用嵌入检索类似文档

然而，`HyDE`的现有实现并不总是开箱即用的，而且不是那么灵活。因此这里介绍一种增强版的`HyDE`（称为`AutoHyDE`），它有可能在各种用例中更有效、更适应。

本文第 1 部分将深入探讨原始`HyDE`论文，并介绍如何将其转化为`LangChain`实现。在第 2 部分中，将讨论当前`HyDE`方法的主要局限性。最后，在第 3 部分介绍`AutoHyDE`。

>`AutoHyDE`是一个框架，可以自动发现索引文档中潜在的相关模式，并生成直接代表这些相关模式的假设文档。

这里直接调整了`LangChain`的`HypotheticalDocumentEmbedder`类来实现`AutoHyDE`，以便它仍然可以与`LangChain`的其他部分链接在一起。

查看[repo](https://link.zhihu.com/?target=https%3A//github.com/ianhojy/auto-hyde/tree/main)了解更多详细信息、[演示](https://link.zhihu.com/?target=https%3A//github.com/ianhojy/auto-hyde/blob/main/auto-hyde-demo.ipynb)和[源代码](https://link.zhihu.com/?target=https%3A//github.com/ianhojy/auto-hyde/blob/main/src/auto_hyde.py)。

![](img/525e6dbb2ebf96e3231e126366f0d1cd.png)

## 第 1 部分：什么是 HyDE？

### HyDE：原始论文

假设文档嵌入（`HyDE`）首次由 Gao 等人在 2022 年的论文《Precise Zero-Shot Dense Retrieval without Relevance Labels》中提出。这篇论文旨在寻找一种改进零样本密集检索的方法（即使用语义嵌入相似性）。为此，他们提出了一种名为`HyDE`的两步方法。

**步骤 1** 涉及指令提示语言模型（在论文中他们使用`GPT-3`）根据原始查询生成假设文档。

**步骤 2** 涉及使用`Contriever`或“无监督对比编码器”将该假设文档转换为嵌入向量，然后用于下游相似性搜索和检索。

![](img/899fad4430c8a98b3c354190ce551dd6.png)

### 关于 Contriever 的补充

`HyDE`原始论文中使用的`Contriever`源自 Izacard 等人于 2022 年 8 月发表的一篇早期论文，名为《Unsupervised Dense Information Retrieval with Contrastive Learning》。在这篇论文中，作者声称虽然神经网络已经成为检索的词频方法的良好替代方案，但它们需要大量数据，而且并不总是能很好地迁移到新的应用领域。因此，他们设计了一种通过对比学习以无监督方式训练嵌入网络的方法。

论文结果表明，该方法能够与`BM25`等无监督词频方法相匹配，并且在跨语言检索方面也表现良好，这是术语匹配方法无法实现的。

出于本文的目的，这里不会进一步讨论训练和数据增强的细节，但如果你有兴趣，请查阅该论文。你也可以通过此链接在 HuggingFace 上找到检索器，研究仓库可在此处找到。

无论如何，`HyDE`论文就是使用`Contriever`来嵌入生成的文档。他们还探索了`ContrieverFT`，它以监督方式在域内进行微调。

### 返回 HyDE 论文

前面概述了`HyDE`方法论和底层检索器的起源。现在，让我们通过回顾论文中的一些关键摘录来仔细看看`HyDE`的原理。

> …In the second step, we use an unsupervised contrastive encoder to encode this document into an embedding vector. Here, we expect the encoder’s dense bottleneck to serve a lossy compressor, where the extra (hallucinated) details are filtered out from the embedding. We use this vector to search against the corpus embeddings.

更正式地，假设的文档嵌入向量定义为：

![](img/4e875c4e995e363a1cf2593b68064e6f.png)

+ 其中`g`是假设文档生成第一步中的`InstructLM(query, INST)`函数；

+ 其中`f`是文档编码器（`Contriever`）

“形式上，`g`根据链式法则定义了一个概率分布”，`V`通过以下公式估算：

![](img/8a3775cd8db335a3295a9256b41a4513.png)

在原始论文中，你还可以在这个[repo](https://link.zhihu.com/?target=https%3A//github.com/texttron/hyde/blob/74101c5157e04f7b57559e7da8ef4a4e5b6da82b/src/hyde/hyde.py)中找到`python`实现。总体而言，我们发现`HyDE`的实现并不复杂：生成、嵌入、平均、检索。下面让我们来看看流行的`HyDE`实现。

### LangChain 实现

在`LangChain`和`LlamaIndex`中，`HyDE`的实现与在上面的论文和仓库中看到的类似。对于大多数正在尝试`LLM`功能的人来说，这些是最有可能使用`HyDE`的方式。我们还将仔细研究这些`HyDE`类的底层提示。让我们关注`LangChain`的实现。供参考的仓库在[这里](https://link.zhihu.com/?target=https%3A//github.com/langchain-ai/langchain/blob/master/libs/langchain/langchain/chains/hyde/base.py)。

第一步是用`from_llm`创建`HypotheticalDocumentEmbedder`——你可以选择一个现有的提示模板（例如下面用于编写新闻段落的模板），或者自定义提示。

```
trec_news_template = """Please write a news passage about the topic.
Topic: {TOPIC}
Passage:"""
```

然后，你可以执行`embed_query`，它首先根据提示模板生成假设文档，然后将这些文档转换为嵌入并将它们组合起来。`embed_documents`函数只是使用`from_llm`定义的`embed_documents`，`combine_embeddings`是平均聚合。

你还会注意到，`documents`属于`List[str]`类型，因为可以生成多个文档。你所要做的就是在`from_llm`步骤中相应地定义`LLM`。以下是示例：

```
multi_llm = OpenAI(n=4, best_of=4)
embeddings = HypotheticalDocumentEmbedder.from_llm(
    multi_llm, base_embeddings, "web_search"
)
result = embeddings.embed_query("Where is the Taj Mahal?")
```

[有关如何使用`HyDE`的简单演练，可以参考这里](https://link.zhihu.com/?target=https%3A//github.com/ianhojy/auto-hyde/blob/main/langchain-hyde-demo.ipynb)。它并不太复杂，而且`LangChain`通过抽象出大部分底层功能使其非常易于使用。

## 第 2 节：HyDE 的局限性

现在我们已经了解了`HyDE`最初的设计方式以及它最终如何在流行的`LangChain`和`LlamaIndex`库中实现，现在谈谈它们的局限性。

首先，虽然`HyDE`论文确实尝试了不同指令`LLM`，但它并没有花太多时间讨论进一步优化`HyDE`流程。从论文中的表 4 中，我们可以看到，使用不同的模型会导致相当大的结果差异：

![](img/c5c3e2e5138f8e0de6f3f925b3cad88e.png)

这里的关键含义不是使用具有最多参数的最大模型。相反，底层`LLM`为检索相关性的总体任务带来如此巨大的差异这一事实使人相信，除了使用不同的`LLM`之外，生成任务是一个值得进一步优化的领域。

事实上，研究人员也表示：

>`InstructGPT` model able to further bring up the performance… this suggests that there may still exist certain factors not captured by the fine-tuned encoder but only by the generative model.

考虑到这种可能性，从第 1 节的`LangChain`深度分析中看到，提示模板是预定义的，用于指导假设文档。例如，“Please write a scientific paper passage to support/refute the claim”。这只有当你的查询/输入很好地符合这些预定义模板时才有效。否则，你也可以提供自定义模板来生成假设文档，但这仍然有其局限性。

首先，现有的实现主要局限于问答框架。在现实世界中，并非所有检索用例都用于回答问题，因此要求`LLM`假设性地回答你的问题是没有意义的。那么在所有这些其他情况下，我们要求`LLM`假设生成什么呢？是的，可以使用自定义提示，但不能保证你编写的提示实际上与现有文档相关。除非底层文档在相关模式方面非常统一，否则编写提示来捕获这些模式将是一个非常人工且不完善的过程。

更重要的是，当你对文档进行分块和索引时，这些分块在风格、语气、结构等方面并不总是同质的。因此，使用单个通用提示来根据输入生成假设文档可能不够，并且编写多个提示来迎合不同类型的分块可能很棘手。例如，假设你正在尝试为博客文章生成假设文档。对于不同的作者、语气、写作风格，如何创建一个通用提示来捕捉这种多样性？因此，`HyDE`的当前实现在覆盖范围方面似乎相当僵化且不是最理想的。

从更技术的层面上看，我们还注意到，关于最终的向量嵌入，论文中提到：

> We simply consider the expectation value, assuming the distribution of `v_qij` is uni-modal, i.e. the query is not ambiguous.

但如果你仔细想想，大多数人对待搜索问题都是模糊的。

在实际用例中，可以想象`v`的分布实际上并不是单峰的，尤其是当查询只有一两个关键字那么模糊时。这意味着同一个底层查询可以映射到多个向量嵌入表示模式。

### 我们该如何开始改进`HyDE`？

>`HyDE` appears unsupervised. No model is trained in `HyDE`: both the generative model and the contrastive encoder remain intact.

`HyDE`的无监督性质非常重要，因为它消除了对任何监督方法变体需要拥有大量数据集或大量计算以改进密集检索的需要。

然而，由于保持严格的无监督，我们失去了一些好处。具体来说，没有办法动态定义与文档嵌入更一致的多个生成函数`g(q, INST)`。

相反，也许值得考虑采用更具适应性和可推广性的半监督方法。这种方法不需要将假设的文档生成强制纳入单个预定义/自定义提示中。与`LangChain`/`LlamaIndex`实现不同，它甚至不需要知道要写什么自定义提示。



相反，`HyDE` 的增强版本将自动学习超出基线检索的更多相关模式，并生成与不同索引块类簇对齐的假设文档。我们称之为 `AutoHyDE` 。

## 第 3 部分：AutoHyDE

### 概述

`AutoHyDE` 的主要目的是自动发现向量数据库中各种相关模式并生成各种文档以提高这些模式的覆盖率。

在技术上，采用 `LangChain` 中 `HypotheticalDocumentEmbedder` 类的现有实现，并创建了新函数来实现 `AutoHyDE`，这样它就可以立即作为任何 `RAG` 链的一部分运行。这是更新后的 `embed_query` 函数，下面将探索每个子方法。

![](img/a1d8ff644e2aea45254a8d2afd6a41f8.png)

[为了说明 `AutoHyDE` 的](https://link.zhihu.com/?target=https%3A//sacred-texts.com/phi/mill/util.txt)工作原理，使用 JS Mill 的 Utilitarianism ，对其进行分块、索引，并尝试根据查询接收相关块。

### 步骤 1：从查询中提取关键词

想象一下你收到了这篇论文作业：

> What is the relationship between justice and happiness?

在步骤1中，`LLM`会提示提取关键词，在本例中：

```
>>> Extracting Keywords from your Query…
>>> …Keywords Extracted: ['relationship', 'justice', 'happiness']
```

关键词很重要，因为它们将在步骤 3 中使用，以便快速（且粗略地）识别在正常检索中可能遗漏的文档。

### 技术实现

提取关键字的函数：

```
@retry(tries=5)
def extract_keywords(
    self, 
    text: str, 
    hypo_params: dict
    ) -> List[str]:

    if hypo_params['verbose']:
        print(f"\n>>> Extracting Keywords from your Query...")
    
    KEYWORD_EXTRACTION_PROMPT = """
    Your goal is to extract a list of keywords from an input phrase, sentence, or several sentences.
    - You can only generate 1 to 5 keywords.
    - Keywords should be nouns, issues, concepts
    - Keywords should not include verbs, prepositions, pronouns
    - Each keyword can only be one word long.
    - If the input is just a single word, return that word as the only keyword.
    {format_instructions}
    The input is:
    {input}
    """

    class KeywordListSchema(BaseModel):
        keywordList: list[str] = Field(description="list of one-word keywords based on a given phrase")

    parser = JsonOutputParser(pydantic_object=KeywordListSchema)

    prompt = ChatPromptTemplate.from_template(
        template=KEYWORD_EXTRACTION_PROMPT,
        input_variables = ["input"],
        partial_variables = {
            'format_instructions': parser.get_format_instructions()
        }
    )

    keyword_extraction_chain = (
        {'input': RunnablePassthrough()}
        | prompt
        | self.llm_chain
        | parser
    )
    
    keywords = keyword_extraction_chain.invoke(text)['keywordList']
    if hypo_params['verbose']:
        print(f">>> ...Keywords Extracted: {keywords}\n")
    
    return keywords
```

这里没有什么特别的，只是使用 `JsonOutputParser` 确保得到准确的输出，即关键字列表。

### 第 2 步：进行初步检索

除了提取关键词之外，我们还将使用原始查询进行初步检索。你必须问自己，作为工作流程的一部分，想要检索多少个块？

假设你最初打算检索 20 份文档作为在 `RAG` 中回答问题的上下文，[那么第 2 ](https://www.zhihu.com/search?q=%E4%BD%99%E5%BC%A6%E7%9B%B8%E4%BC%BC%E5%BA%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714886313%7D)步将检索超过 20 份文档，以便我们能够探索由于截断 20 份而可能被忽略（找不到更好的词）的潜在文档。可以使用 `exploration_multiplier` 切换。使用 `baseline_k = 20` 和 `exploration_multiplier = 5`，根据 余弦相似度 将探索 20 x 5 = 100 份top文档。

### 技术实现

```
def do_init_retrieval(
    self,
    db: VectorStore, 
    text: str, 
    hypo_params: dict
    ) -> List[Tuple[Document, float]]:
    
    k = hypo_params['baseline_k'] * hypo_params['exploration_multiplier']
    if hypo_params['verbose']:
        print(f"\n>>> Performing Initial Retrieval of {k} documents...\n")
    docs = db.similarity_search_with_score(
        text, 
        k=k
    )
    return docs
```

### 步骤 3：获取包含关键词的被忽略的文档

因此，除了通过直接检索到的 20 份文档之外，还有 80 份文档需要考虑，而这些文档本来会被忽略。

从这 80 篇文档中，我们希望找到与原始查询直接相关的文档子集。如何高效地做到这一点？一种简单的方法是选择包含从步骤 1 中提取的任何关键字的文档。有很多方法可以做得更好，但关键字匹配已经是一种快速且相当有效的方法，可以识别第一批被忽略的文档。

以下是代码输出：

```
>>> Checking 80 Docs ranked after 20 for presence of keyword…
>>> …69 neglected Docs identified
```

在上面的输出中，我们看到在 80 个文档中，有 69 个包含一个或多个关键字 `['relationship', 'justice', 'happiness']`。直觉上，可以将这些理解为潜在相关的文档，这些文档在正常检索前 20 个文档时可能会被忽略。

### 技术实现

```
def get_remaining_docs_with_keywords(
    self, 
    text: str, 
    init_docs: List[Tuple[Document, float]], 
    keywords: List[str], 
    hypo_params: dict
    ) -> List[Document]:

    remaining_docs_with_keywords = list()
    
    if hypo_params['verbose']:
        print(f"""\n>>> Checking {len(init_docs[hypo_params['baseline_k']:])} 
                Docs ranked after {hypo_params['baseline_k']} for presence of keyword...""")

    for r in init_docs[hypo_params['baseline_k']:]:
        page_content = r[0].page_content.lower()
        for keyword in keywords:
            if keyword.lower() in page_content:
                remaining_docs_with_keywords.append(r)
                continue
                
    if hypo_params['verbose']:
        print(f">>> ...{len(remaining_docs_with_keywords)} neglected Docs identified\n")
    return remaining_docs_with_keywords
```

对于每个文档，如果在其中发现任何关键字，将其添加到忽略文档列表中（`remaining_docs_with_keywords`）。

### 步骤 4：对被忽略的文档进行聚类

这是重要的一步。我们可能试图让 `LLM` 引用这 69 份被忽视的文档，并根据原始用户查询为每份文档编写一份假设文档。然而，这样做计算代价很大，并且不是发现这些文档中存在的主要相关模式的最有效方法。

相反，我们将使用嵌入对 69 份文档进行聚类。这使我们能够发现存在于数据集中的关键相关模式。在下面的输出中，我们看到 69 份文档随后被聚类为 6 个不同的组。

```
>>> Clustering neglected Docs…
>>> …6 Clusters identified
```

### 技术实现

[我们如何选择聚类数量？](https://link.zhihu.com/?target=https%3A//link.springer.com/chapter/10.1007/978-3-642-37456-2_14)在这里使用 `HDBSCAN` ，它是 `DBSCAN` 的半监督层次聚类版本。

`min_samples` 是控制核心点的最小邻居数的参数。 `min_samples` 越大，越多的点将被标记为聚类中的噪声。现在，我们令其为 1。 `min_cluster_size` 用于识别“脱离聚类”或分裂成两个新聚类的点。

标记文档后，我们删除标记为 -1（无聚类）的文档，然后创建一个字典，其中键是标记的组号，值是每个文档的页面内容。

```
def cluster_docs(
    self, 
    remaining_docs_with_keywords: List[Document], 
    hypo_params: dict
    ) -> Dict[int, List[str]]:
    
    from hdbscan import HDBSCAN
    
    if hypo_params['verbose']:
        print(f"\n>>> Clustering neglected Docs...")
    
    embeddings = self.embed_documents([
        r[0].page_content 
        for r in remaining_docs_with_keywords], 
        {'verbose': False})
    hdb = HDBSCAN(min_samples=1, min_cluster_size=3).fit(embeddings)
    remaining_docs_with_cat = filter(lambda x: x[1] != -1, zip([r[0].page_content for r in remaining_docs_with_keywords], hdb.labels_))
    
    cat_dict = {}

    for page_content, cat in remaining_docs_with_cat:
        if cat not in cat_dict:
            cat_dict[cat] = [page_content]
        else:
            cat_dict[cat].append(page_content)
            
    if hypo_params['verbose']:
        print(f">>> ...{len(cat_dict)} Clusters identified\n")
            
    return cat_dict
```

### 步骤 5：生成假设文件

对于代表相关模式组的每个类簇，要求 `LLM` 引用属于该类簇的文档，以创建与这些文档相似的新假设文档。这是区分 `AutoHyDE` 方法与现有 `HyDE` 方法的重要一步。在 `AutoHyDE` 中，相关模式会自动学习，然后用于生成假设文档，不需要定制提示写作。

以[下是第一个](https://www.zhihu.com/search?q=%E5%BF%AB%E9%80%9F%E6%9F%A5%E7%9C%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714886313%7D)类簇的示例。可以看到 `LLM` 引用了第一个被忽略文档类簇中的每个被忽略文档，并根据原始查询创建了假设文档。所有类簇都执行此操作。 快速查看 下面的文本，你会发现它确实很好地模拟了 Mill 的复杂风格，并且你不必编写自定义提示来尝试捕捉这种风格。只需要在后台进行少样本提示即可。

![](img/9c697bcdf5de112f6b2d2b2a713b82e7.png)

### 技术实现

下面的函数的作用是为每个已识别的文档类簇生成假设文档。与之前类似，使用 `JsonOutputParser` 来确保获得所需的输出。少样本方法通常有效，如下面的 `HYPOTHETICAL_DOCUMENT_PROMPT` 所示：



## 假设文档生成

```python
@retry(tries=5)
def generate_hypo_docs(
    self, 
    text: str, 
    cat_dict: Dict[int, List[str]], 
    hypo_params: dict
) -> List[str]:
    
    hypo_docs = list()
    
    if hypo_params['verbose']:
        print(f"\n>>> Generating Hypothetical Documents for each Doc Cluster...\n")

    HYPOTHETICAL_DOCUMENT_PROMPT = """
    Your instruction is to generate a single hypothetical document from an input.
    - This hypothetical document must be similar in style, tone and voice as examples you are provided with.
    - This hypothetical document must appear like it was written by the same author as the examples you are provided with.
    - This hypothetical document must also be similar in length with the examples you are provided with.
    {format_instructions}
    ### EXAMPLES ###
    Below are some examples of hypothetical documents, all written by the same author, in pairs of <Input> and <Hypothetical Document>:
    {ref_documents}
    ### INSTRUCTION ###
    Now generate a new hypothetical document. 
    <Input>
    {input}
    <Hypothetical Document>
    """

    class HypotheticalDocumentSchema(BaseModel):
        hypotheticalDocument: str = Field(description="a hypothetical document given an input word, phrase or question")

    parser = JsonOutputParser(pydantic_object=HypotheticalDocumentSchema)

    prompt = ChatPromptTemplate.from_template(
        template=HYPOTHETICAL_DOCUMENT_PROMPT,
        intput_variables = ["input", "ref_documents"],
        partial_variables = {
            'format_instructions': parser.get_format_instructions()
        }
    )

    hypothetical_document_chain = (
        {'input': RunnablePassthrough(), 'ref_documents': RunnablePassthrough()}
        | prompt
        | self.llm_chain
        | parser
    )

    cat_ii = 1
    for cat in cat_dict.keys():

        ref_doc_string = ""
        doc_ii = 1
        for doc in cat_dict[cat]:
            ref_doc_string += f"\n\n<Input>"
            ref_doc_string += text
            ref_doc_string += f"\n\n<Hypothetical Document>\n"
            ref_doc_string += f'{{"hypotheticalDocument": "{doc}"}}'
            doc_ii += 1

        hypo_doc = hypothetical_document_chain.invoke(
            {'input': text, 'ref_documents': ref_doc_string}
        )['hypotheticalDocument']

        if hypo_params['verbose']:
            print(f"\n### Hypo Doc {cat_ii} ###")
            print(hypo_doc+'\n')
        
        hypo_docs.append(hypo_doc)
        
        cat_ii += 1
        
    return hypo_docs
```

## 步骤 6+7：嵌入和合并

最后，利用假设文档列表（每个类簇一个），嵌入假设文档（步骤 6）并将它们组合成单个嵌入向量（步骤 7）。

此步骤与原始实现类似，嵌入假设文档，然后取平均值以获得最终向量。

正如之前提到的，这一步对于捕捉文档的更广泛的异质性并没有真正的意义，特别是试图发现和生成具有不同相关模式的文档时。但为了保持简单，并保持与现有 LangChain 实现的兼容性（重点是你可以立即将其与 LangChain RAG 中的其他组件一起使用），现在将保留它以输出单个嵌入向量，并可能稍后处理聚合限制。

```
>>> Generating embeddings for hypothetical documents...
>>> Combining embeddings for hypothetical documents...
```

```
hyde_embedding[:10]

>>> [
    0.015876703333653572,
    -0.013635452586265312,
    0.021941788843565697,
    -0.02570370381768275,
    -0.015861831315729033,
    -0.0003427757204382006,
    0.0027591148428962454,
    -0.00883308151544041,
    -0.014893267477206646,
    -0.020748802766928837
]
```

### 技术实现

```python
def embed_documents(self, texts: List[str], hypo_params) -> List[List[float]]:
    """Call the base embeddings."""
    if hypo_params['verbose']:
        print("\n>>> Generating embeddings for hypothetical documents...\n")
    return self.base_embeddings.embed_documents(texts)

def combine_embeddings(self, embeddings: List[List[float]], hypo_params) -> List[float]:
    """Combine embeddings into final embeddings."""
    if hypo_params['verbose']:
        print("\n>>> Combining embeddings for hypothetical documents...\n")
    return list(np.array(embeddings).mean(axis=0))
```

[实现此 HyDE 增强版本的 repo](https://link.zhihu.com/?target=https%3A//github.com/ianhojy/auto-hyde/tree/main) 可在此 repo 中找到。

## 第 4 部分：结论

总结一下，AutoHyDE 的主要改进在于生成假设文档的方式。没有使用固定的提示来生成这些文档（无论是在 LangChain 中预定义的还是由用户自定义的），而是设计了一个框架自动发现所有文档中可能被基线检索忽略的潜在相关模式，并为每一种模式生成假设文档。

通过这种方式，我们能够使 HyDE 适应各种各样的任务和环境，并适应相关模式异构的索引的检索。

## 参考文档

[https://medium.com/towards-data-science/autohyde-making-hyde-better-for-advanced-llm-rag-619e58cdbd8e](https://link.zhihu.com/?target=https%3A//medium.com/towards-data-science/autohyde-making-hyde-better-for-advanced-llm-rag-619e58cdbd8e)
