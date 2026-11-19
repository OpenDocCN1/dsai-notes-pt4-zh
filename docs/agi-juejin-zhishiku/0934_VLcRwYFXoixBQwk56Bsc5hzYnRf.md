
# 如何利用查询改写技术改善RAG效果

> 来源：[https://agijuejin.feishu.cn/docx/N1M7d8csto1MpexHpDgcLdK2nmf](https://agijuejin.feishu.cn/docx/N1M7d8csto1MpexHpDgcLdK2nmf)
> 作者：Meta
> 原文：[https://zhuanlan.zhihu.com/p/677540243](https://zhuanlan.zhihu.com/p/677540243)

在检索增强型生成（RAG）应用的背景下，检索器可能使用单一的嵌入模型来同时编码查询和文档，或者为每个部分采用不同的模型。此外，用户的原始查询可能存在措辞不准确和缺乏语义信息的问题。因此，将用户查询的语义空间与文档的语义空间对齐至关重要，主要有两种基本的技术（包括查询改写和嵌入转换）可以实现这种对齐，本文主要介绍查询改写技术。

查询改写是一种用于对齐查询和文档的语义的基本方法。例如`Query2Doc`和`ITER-RETGEN`等方法利用大语言模型（LLM）将原始查询与额外指导信息相结合，创建一个伪文档。`HyDE`使用文本提示构造查询向量，生成捕获关键模式的“假设”文档。`RRR`提出了一个框架，颠倒了传统的检索和阅读顺序，专注于查询改写。`STEP-BACKPROMPTING`使LLM能够基于高层次概念进行抽象推理和检索。此外，多查询检索方法利用LLM同时生成和执行多个搜索查询，这对于解决具有多个子问题的复杂问题特别有利。

以下是对这些方法的详细介绍。

## `Query2Doc`

直接用大语言模型结合few-shot，生成伪文本进行检索。

![](img/1b866e1fa2a200ecdde946c5eb0a9134.png)

## `ITER-RETGEN`

先走正常流程进行检索增强生成，然后将前一次生成的结果+问题拼接，再去检索相关文档，继续生成新的结果；这个过程重复多次之后得到最终结果。

![](img/11e34c9174eeebf48dda6f22f4fa51af.png)

## `HyDE`

该方法通过使用大型语言模型来[创建](https://link.zhihu.com/?target=https%3A//docs.llamaindex.ai/en/stable/examples/query_transformations/HyDEQueryTransformDemo.html)假设性答案，并将这些答案嵌入空间以检索类似的真实文档。这种方法更注重答案之间而非查询与答案间的嵌入相似性。尽管这种方法有其创新之处，但在处理不熟悉的主题时，可能会产生不准确的结果。

`llamaindex`案例：`https://docs.llamaindex.ai/en/stable/examples/query_transformations/HyDEQueryTransformDemo.html`

![](img/60cbfaa5e6a5f1975b2210bfb3e5ea58.png)

## `Rewrite-Retrieve-Read`

该文章首先使用LLM对`query`进行 改写，再进行检索和回答生成，而不是直接对原始的`query`进行内容检索和答案生成。

![](img/5f4c03ad3bced5e2c28061b18fe66291.png)

`langchain`中的实现：`https://github.com/langchain-ai/langchain/blob/master/cookbook/rewrite.ipynb?ref=blog.langchain.dev`

直接使用`prompt`对原始的问题进行改写：

```
&quot;&quot;&quot;Provide a better search query for \
web search engine to answer the given question, end \
the queries with ’**’. Question: \
{x} Answer:&quot;&quot;&quot;
```

## `Step-Back Prompt`

该方法使用LLM生成一个“回退”的更加抽象的问题，它既可以与检索一起使用，也可以独立使用。在进行检索时，同时使用“回退”的抽象问题和原始问题进行检索，然后使用两者的结果来确定语言模型的响应。

![](img/8f2bb2856215ddd510f2a1857321aa9b.png)

`langchain`中的实现：`https://github.com/langchain-ai/langchain/blob/master/cookbook/stepback-qa.ipynb?ref=blog.langchain.dev`

采用简单的`prompt`+in-context learning来实现抽象问题提炼：

```
You are an expert at world knowledge. 
Your task is to step back and paraphrase a question to a more generic step-back question, 
which is easier to answer. Here are a few examples:&quot;&quot;&quot;,
```

## 参考资料

1. Ma, Xinbei, et al. "Query Rewriting for Retrieval-Augmented Large Language Models." arXiv preprint arXiv:2305.14283 (2023).

1. Zheng, Huaixiu Steven, et al. "Take a step back: evoking reasoning via abstraction in large language models." arXiv preprint arXiv:2310.06117 (2023).

1. Wang, Liang, Nan Yang, and Furu Wei. "Query2doc: Query Expansion with Large Language Models." arXiv preprint arXiv:2303.07678 (2023).

1. [Langchain Blog: query transformations](https://blog.langchain.dev/query-transformations/)

1. Shao, Zhihong, et al. "Enhancing Retrieval-Augmented Large Language Models with Iterative Retrieval-Generation Synergy." arXiv preprint arXiv:2305.15294 (2023).

1. Gao, Luyu, et al. "Precise zero-shot dense retrieval without relevance labels." arXiv preprint arXiv:2212.10496 (2022).

1. [Advanced RAG — Improving retrieval using Hypothetical Document Embeddings(HyDE)](https://medium.aiplanet.com/advanced-rag-improving-retrieval-using-hypothetical-document-embeddings-hyde-1421a8ec075a)
