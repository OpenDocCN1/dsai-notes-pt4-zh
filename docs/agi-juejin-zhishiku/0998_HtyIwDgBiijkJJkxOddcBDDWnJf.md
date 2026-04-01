# 关于RAG优化的几个小技巧

> 来源：[https://agijuejin.feishu.cn/docx/YZEZd0z5woSyyLxuQnMcb56jntf](https://agijuejin.feishu.cn/docx/YZEZd0z5woSyyLxuQnMcb56jntf)

> 作者：王大锤

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/702196014)hihu.com/p/702196...

## 一、背景说明

RAG技术为大型语言模型（LLMs）提供了从数据源检索到的信息，以支撑其生成的答案。简而言之，RAG就是搜索加上LLM提示，你让模型在提供的信息上下文中回答问题。查询和检索到的上下文都被注入到发送给LLM的提示中。

**传统的建模方式 **:

典型的RAG向量化应用程序有两个主要组件：

**索引 **：从源中获取数据并对其进行 [向量化 ](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)索引。

**检索和生成 **：运行时接受用户查询转化成 [向量化数据 ](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E5%8C%96%E6%95%B0%E6%8D%AE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)，并从索引中检索相关数据，然后将其传递给LLM，生成内容。

实际使用过程中，上面简单的流程经常效果不能满足业务需求，核心问题在于如何检索并处理好传给LLM的内容，下面介绍几种优化思路以及LangChain的实现。

## 二、 [优化方法](https://www.zhihu.com/search?q=%E4%BC%98%E5%8C%96%E6%96%B9%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)

### 2.1 多层索引

[多层索引技术 ](https://www.zhihu.com/search?q=%E5%A4%9A%E5%B1%82%E7%B4%A2%E5%BC%95%E6%8A%80%E6%9C%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)主要是针对海量的文档检索的处理策略，对面临需要从海量的文档中搜索、找到相关信息，并将这类信息综合成一个答案，并应用其来源。一般有2种方式建立多层索引来优化这类问题。

一种是为文档建立 [多级索引 ](https://www.zhihu.com/search?q=%E5%A4%9A%E7%BA%A7%E7%B4%A2%E5%BC%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)，比如建立文档的类别标签，在处理指定[业务场景](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)下的问答时，只需要对该类别下的 知识库 进行检索，如:

> 处理医疗领域的RAG问答业务时，可能会涉及很多的医学知识库，假设有1000篇文档，包括10个不同的医学研究方向。

> 当涉及的业务问题范畴只涉及外科相关知识时，可以通过类别标签进行筛选过滤，而无需从1000篇文档中进行全局检索。

第二种是从两个索引——一个是有该文档的摘要组成，一个由文档切片组成。在处理学术论文时，这类方[法一般有较](https://www.zhihu.com/search?q=%E5%8F%AC%E5%9B%9E%E6%A3%80%E7%B4%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)[好的表现。](https://www.zhihu.com/search?q=%E5%88%87%E7%89%87%E7%B4%A2%E5%BC%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)这类方法的思路是分为两个步骤进行检索，首先通过摘要部分进行 召回检索 ，过滤出可能和当前问题相关的候选文档集，然后再利用 切片索引 ，从候选的文档集中进行检索，召回相关的知识切片。

### 2.2 假设性问题

首先解释一下什么是假设性问题: 假设性问题是一种提问方式，它基于一个或多个假设的情况或前提来提出问题。在对知识库中文档内容进行切片时，是可以以该切片为假设条件，利用LLM预先设置几个候选的相关性问题的，也就是说，这几个候选的相关性问题是和切片的内容强相关的。

在构建切片索引时，可以将对应的假设性问题进行向量化，并同时存储于向量库中。在RAG中的召回步骤中，对这个问[题向量索引进](https://www.zhihu.com/search?q=%E5%8E%9F%E5%A7%8B%E6%96%87%E6%9C%AC%E5%9D%97&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)行查询搜索，然后路由到原始文本块，并将它们作为上下文发送给LLM以获取答案。这种方法提高了搜索质量，因为与 原始文本块 相比，查询和假设问题之间的语义相似性更高。

除此以外，还有一种候选的假设性问题解决方案，和上述策略是对应的，它的核心思想可以表示为:

在要求LLM给定查询信息时，用LLM预先生成一个假设的响应，然后将其向量和查询向量一起使用，进一步提升检索的质量。示例如下:

> [问题: 感冒了](https://www.zhihu.com/search?q=prompt&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)怎么办?

> 大模型预设响应: 感冒了需要多休息，喝水，必要时就医。

> 召回时用问题和大模型预设响应进行拼接，形成新的 prompt 内容进行召回。

### 2.3 拓展上下文

检索较小的块以获得更好的搜索质量，但将周围的上下文一起拼接起来传给LLM推理。当然在索引的时候就要把数据处理好，搜索的时候才能找到周围的上下文。这么做的好处，就是小的文本块可以提升搜索的准确率，而扩展上下文则可以给LLM更丰富的内容。这里有两种拓展上下文的方式可以作为参考:

一种处理的方案是，通过分割和存储小块数据来实现，在检索过程中，它首先获取小块，然后查找这些块的父ID，并返回那些较大的文档。这种方式在langchain里有进行封装。

![](img/d2fd1eeaf6087a7aef625c09a31e981b.png)

```
from langchain.retrievers import ParentDocumentRetriever

from langchain.storage import InMemoryStore
from langchain_community.document_loaders import TextLoader
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings
from langchain_text_splitters import RecursiveCharacterTextSplitter

# This text splitter is used to create the child documents
child_splitter = RecursiveCharacterTextSplitter(chunk_size=400)
# The vectorstore to use to index the child chunks
vectorstore = Chroma(
    collection_name=&quot;full_documents&quot;, embedding_function=OpenAIEmbeddings()
)
# The storage layer for the parent documents
store = InMemoryStore()
retriever = ParentDocumentRetriever(
    vectorstore=vectorstore,
    docstore=store,
    child_splitter=child_splitter,
)
```

第二种是对更大的块进行检索，有时，完整文档可能太大而无法按原样检索它们。在这种情况下，我们真正想做的是首先将原始文档分割成更大的块，然后将其分割成更小的块。然后我们索引较小的块，但在检索时我们检索较大的块（但仍然不是完整的文档）。

```
[# This text spl](https://www.zhihu.com/search?q=parent_splitter&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)[itter is used](https://www.zhihu.com/search?q=split_parents&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D) to create the parent documents
parent_splitter = RecursiveCharacterTextSplitter(chunk_size=2000)
# This text splitter is used to create the child documents
# It should create documents smaller than the parent
child_splitter = RecursiveCharacterTextSplitter(chunk_size=400)
# The vectorstore to use to index the child chunks
vectorstore = Chroma(
    collection_name=&quot;split_parents&quot;, embedding_function=OpenAIEmbeddings()
)
# The storage layer for the parent documents
store = InMemoryStore()

retriever = ParentDocumentRetriever(
    vectorstore=vectorstore,
    docstore=store,
    child_splitter=child_splitter,
    parent_splitter=parent_splitter,
)

retriever.add_documents(docs)
```

### 2.4 融合检索

[将检索到的结果](https://www.zhihu.com/search?q=%E5%80%92%E6%8E%92%E8%9E%8D%E5%90%88%E7%AE%97%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)与不同的相似性分数适当地结合起来——这个问题通常是在 倒排融合算法 的帮助下解决的，该算法将检索得到的结果重新排序以获得最终输出。在LangChain中，这是在EnsembleRetriever类中实现的，它结合了您定义的检索器列表，例如faiss矢量索引和基于BM25的检索器，并使用RRF进行重新排序。混合或融合搜索通常会提供更好的检索结果，因为两种互补的搜索算法结合在一起，同时考虑了查询和存储文档之间的语义相似性和关键字匹配。

![](img/1db3dde82dc22ad7f01f0b4bcf24364a.png)

```
[from lang](https://www.zhihu.com/search?q=metadatas&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)[chain.retriever](https://www.zhihu.com/search?q=faiss_retriever&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)[s import Ensem](https://www.zhihu.com/search?q=bm25_retriever&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)bleRetriever
from langchain_community.retrievers import BM25Retriever
from langchain_community.vectorstores import FAISS
from langchain_openai import OpenAIEmbeddings

doc_list_1 = [
    &quot;I like apples&quot;,
    &quot;I like oranges&quot;,
    &quot;Apples and oranges are fruits&quot;,
]

# initialize the bm25 retriever and faiss retriever
bm25_retriever = BM25Retriever.from_texts(
    doc_list_1, metadatas=[{&quot;source&quot;: 1}] * len(doc_list_1)
)
bm25_retriever.k = 2

doc_list_2 = [
    &quot;You like apples&quot;,
    &quot;You like oranges&quot;,
]

embedding = OpenAIEmbeddings()
faiss_vectorstore = FAISS.from_texts(
    doc_list_2, embedding, metadatas=[{&quot;source&quot;: 2}] * len(doc_list_2)
)
faiss_retriever = faiss_vectorstore.as_retriever(search_kwargs={&quot;k&quot;: 2})

# initialize the ensemble retriever
ensemble_retriever = EnsembleRetriever(
    retrievers=[bm25_retriever, faiss_retriever], weights=[0.5, 0.5]
)

docs = ensemble_retriever.invoke(&quot;apples&quot;)
docs
```

### 2.5 过滤与压缩

检索容易遇到的一个问题，当把 [数据向量化 ](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E5%90%91%E9%87%8F%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)索引到数据库时，不太可能预想到对应的用户问题会是什么。这意味着，与查询最相关的信息可能隐藏在一个包含大量不相关文本的文档中，如果都一股脑传给LLM，不仅费用高（肉疼）而且效果可能也会受影响。

[LLMCh](https://www.zhihu.com/search?q=LLM%E9%93%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)ainFilter是一个简单的压缩器，使用 LLM链 来决定最初检索到的文档中的哪些要过滤掉，哪些要返回，而无需操作文档内容。嗯。。。这个，我就是因为不想调用LLM费钱处理才采用这个方案的吗？EmbeddingsFilter通过嵌入文档和查询并只返回那些与查询具有足够相似嵌入的文档，提供了一个更便宜、更快的选项。

LLMChainFilter的用法:

```
[from langch](https://www.zhihu.com/search?q=compressors&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)[ain.retrievers.](https://www.zhihu.com/search?q=compressed_docs&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)document_compressors import LLMChainFilter

_filter = LLMChainFilter.from_llm(llm)
compression_retriever = ContextualCompressionRetriever(
    base_compressor=_filter, base_retriever=retriever
)

compressed_docs = compression_retriever.get_relevant_documents(
    &quot;What did the president say about Ketanji Jackson Brown&quot;
)
pretty_print_docs(compressed_docs)
```

EmbeddingsFilter的用法:

```
[from langc](https://www.zhihu.com/search?q=embeddings&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)hain.retrievers.document_compressors import EmbeddingsFilter
from langchain_openai import OpenAIEmbeddings

embeddings = OpenAIEmbeddings()
embeddings_filter = EmbeddingsFilter(embeddings=embeddings, similarity_threshold=0.76)
compression_retriever = ContextualCompressionRetriever(
    base_compressor=embeddings_filter, base_retriever=retriever
)

compressed_docs = compression_retriever.get_relevant_documents(
    &quot;What did the president say about Ketanji Jackson Brown&quot;
)
pretty_print_docs(compressed_docs)
```

### 2.6 元数据过滤

我们已经习惯了SQL查询的时候增加Where的限制条件，这个可以精确查询所需的数据。RAG也不是所有的时候都要通过语义或关键词检索。比如检索“所有评分大于8分的电影”，或者我们公司实际的业务场景，检索的时候只需要明确某个品牌的数据。

通过 [元数据过滤 ](https://www.zhihu.com/search?q=%E5%85%83%E6%95%B0%E6%8D%AE%E8%BF%87%E6%BB%A4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)实现精确过滤，和语义检索配合。在存储数据块到向量数据块时，需要先处理好元数据，在我们公司的场景中，就是把所属的品牌ID一起存储。检索的时候可以利用大模型的理解能力，把自然语言转成元数据过滤。看看LangChain的实现， metadata_field_info 是pydantic的格式。

![](img/1df77ee11c5e8c99ba70efdaac4942bf.png)

```
from langchain.llms import OpenAI
from langchain.retrievers.self_query.base import SelfQueryRetriever
from langchain.chains.query_constructor.base import AttributeInfo
 
metadata_field_info=[
    AttributeInfo(
        name=&quot;genre&quot;,
        description=&quot;The genre of the movie&quot;, 
        type=&quot;string or list[string]&quot;, 
    ),
    AttributeInfo(
        name=&quot;year&quot;,
        description=&quot;The year the movie was released&quot;, 
        type=&quot;integer&quot;, 
    ),
    AttributeInfo(
        name=&quot;director&quot;,
        description=&quot;The name of the movie director&quot;, 
        type=&quot;string&quot;, 
    ),
    AttributeInfo(
        name=&quot;rating&quot;,
        description=&quot;A 1-10 rating for the movie&quot;,
        type=&quot;float&quot;
    ),
]
document_content_description = &quot;Brief summary of a movie&quot;
llm = OpenAI(temperature=0)
retriever = SelfQueryRetriever.from_llm(llm, vectorstore, document_content_description, metadata_field_info, verbose=True)
```

### 2.7 时间加权

对于一些类似客服FAQ的场景，检索命中越频繁，数据的权重就越高，可以使用了语义相似性和时间衰减的组合。LangChain给出的计算公式是这样的。

![](img/3d49375241a4b63902e998ccc687409f.png)

[hours_passed ](https://www.zhihu.com/search?q=hours_passed&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A702196014%7D)是指自上次访问检索器中的对象以来（而不是自创建对象以来）经过的小时数。这意味着频繁访问的对象保持“新鲜”。

还有另一种时间权重，比如新闻资讯，创建时间越近权重越高，甚至可以为其设置过期时间。

RAG应用系统中，大家比较关心的因素：准确率、速度、成本。上面讨论的方案基本上都是提升准确率的，同时带来的问题就是速度下降，部分方案会成本上升。

另一方面，在实际的业务场景中，准确率可能是现阶段最重要的问题，业务人员对于应用的要求可能是90分，而LLM目前的能力只能达到60分，如果不能通过其它的方式提升效果，那么应用可能无法在实际场景中使用了。