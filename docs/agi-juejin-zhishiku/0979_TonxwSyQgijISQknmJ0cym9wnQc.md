# RAG中的高级检索技巧（3）-Sentence Window Retrieval

> 来源：[https://agijuejin.feishu.cn/docx/Hl5HdK7ivoaG8BxcXffc7O8nntd](https://agijuejin.feishu.cn/docx/Hl5HdK7ivoaG8BxcXffc7O8nntd)

> 作者：lucas大叔

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/680767408)hihu.com/p/680767...

书接上回，这是本系列的最后一篇文章。前两篇分别介绍了基础RAG管道的构建和Parent Document Retrieval在RAG中的应用，本篇将介绍Sentence Window Retrieval技术，并与前两篇的方法进行比较。

## Sentence Window Retrieval

在sentence window retrieval中，对文档片段进行检索，然后返回检索到的相关句子及其周围的句子，LLM根据检索到的相关语句及其窗口内的上下文句子生成答案。

![](img/37592e959be668447f4e54f0999b06c5.png)

从上[图可以看到，检索到的](https://www.zhihu.com/search?q=embedding&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)相关语句是标红的句子，然后将窗口内上下位句子与相关句子一起传给LLM生成答案。我们可以控制语句窗口的大小，但为什么要怎么做呢？回想一下我们在第一篇文章中曾提到，基于 embedding 的检索在较小的句子上表现更好，因此要控制窗口不要太大。基于sentence based retrieval实现了相关chunk检索与传给LLM chunk的解耦，下面一起来看下代码实现吧！

## [加载文档](https://www.zhihu.com/search?q=%E5%8A%A0%E8%BD%BD%E6%96%87%E6%A1%A3&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)

```
 from llama_index import (
    SimpleDirectoryReader,
)

# load document
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles/&quot;
).load_data(show_progress=True)

print(len(documents))
```

执行结果如图

![](img/6185448a2400a8cdf839fc94ea49862c.png)

从上图可以看到，文档的长度是1，加载的文档只有一个单[独的页面或文](https://www.zhihu.com/search?q=PDF%E6%96%87%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)档。如果加载具有多个页面的 PDF文件 ，将所有页面合并到一个文本中有助于准确地将文档拆分为块或LlamaIndex中所称的“节点”。

以下是如何将多个文档（页面）合并为一个文本：

```
 from llama_index import (
    SimpleDirectoryReader,
    Document
)

# load document
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles/&quot;
).load_data(show_progress=True)

# merge pages into one
document = Document(text=&quot;\n\n&quot;.join([doc.text for doc in documents]))
print(document)
```

在当前案例中，这不是必要的操作。但当加载多页PDF文档时就需要先把它们合并为一个文本。这一点貌似与langchain不一样，在langchain中加载多页的PDF文档并不需要先合并为一个文本。

## 创建Sentence Window Retriever

首先，考虑如何创建将文档分解为各个句子的 SentenceWindowNodeParser ，然后能够在允许的窗口内为每个句子添加周围的句子，以创建更大的上下文。下面举个栗子说明：

```
[from llama_](https://www.zhihu.com/search?q=node_parser&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)inde[x.node_parser](https://www.zhihu.com/search?q=window_size%3D2&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D) import SentenceWindowNodeParser

# create the sentence window node parser
node_parser = SentenceWindowNodeParser.from_defaults(
    window_size=2,
    window_metadata_key=&quot;window&quot;,
    original_text_metadata_key=&quot;original_text&quot;,
)

# Toy example to play around with
text = &quot;I love programming. Python is my most favorite language. I love LLMs. I love LlamaIndex.&quot;

# Get nodes
nodes = node_parser.get_nodes_from_documents([Document(text=text)])

# Print out individual nodes
print([x.text for x in nodes])

# Print out the window around the second node
print(nodes[1].metadata[&quot;window&quot;])
```

以下是 Jupyter-notebook 的输出结果：

![](img/ff5928355e071c0a02abeccde8356464.png)

可以看到原句子( **“Python is my most** favorite language” )两边的窗口大小是2，在它前后各有一句话。下面是LlamaIndex官方文档的解释：

+   默认情况下，句子窗口为原始句子两侧各5个句子。

+   在这种情况下，不使用chunk大小设置，而是遵循窗口设置。

## 创建索引

[构建索引需要LLM和embe](https://www.zhihu.com/search?q=gpt-3.5-turbo&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)dding模型，LLM使用OpenA[I gpt-](https://www.zhihu.com/search?q=%E8%8A%82%E7%82%B9%E8%A7%A3%E6%9E%90%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)3.5-turbo ，然后用s[ervice conte](https://www.zhihu.com/search?q=embedding%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)xt指定embedding模型、LLM和 节点解析器 （上面创建的句子窗口）；对于 embedding模型 ，使用LlamaIndex提供的OpenAIEmbedding模型，也可以使用任何其他的embedding模型。

```
# creating OpenAI gpt-3.5-turbo LLM and OpenAIEmbedding model
llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;, temperature=0.1)
embed_model = OpenAIEmbedding()

# creating the service context
sentence_context = ServiceContext.from_defaults(
    llm=llm,
    embed_model=embed_model,
    node_parser=node_parser,
)
```

由于我们将node_parser作为SentenceWindowNodeParser传入，它将在后台获取每个句子，用周围的句子对其进行扩充并创建embedding，然后存储在向量存储中。如下图所示，红色文本是原始句子，其周围的白色文本是增强文本。为每个文本创建embedding，每次使用不同的窗口。

![](img/31a1e089767afae5dda19b87b46a3363.png)

我们还需要设置创建向量存储索引并使其持久化，这意味着创建的embedding将存储在向量存储中，以避免每次运行应用程序时创建新embedding的重复劳动和成本。为此，我们必须检查存储的索引是否存在于内存中；如果没有，则创建并加载现有的索引。

```
[import os
](https://www.zhihu.com/search?q=os.environ&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)from llama_index import (
    SimpleDirectoryReader,
    Document,
    StorageContext,
    load_index_from_storage
)

from llama_index.node_parser import SentenceWindowNodeParser
from llama_index.llms import OpenAI
from llama_index.embeddings import OpenAIEmbedding
from llama_index import ServiceContext
from llama_index import VectorStoreIndex

from decouple import config

# set env variables
os.environ[&quot;OPENAI_API_KEY&quot;] = config(&quot;OPENAI_API_KEY&quot;)


# load document
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles/&quot;
).load_data(show_progress=True)


# merge pages into one
document = Document(text=&quot;\n\n&quot;.join([doc.text for doc in documents]))

node_parser = SentenceWindowNodeParser.from_defaults(
    window_size=3,
    window_metadata_key=&quot;window&quot;,
    original_text_metadata_key=&quot;original_text&quot;,
)

# creating OpenAI gpt-3.5-turbo LLM and OpenAIEmbedding model
llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;, temperature=0.1)
embed_model = OpenAIEmbedding()

# creating the service context
sentence_context = ServiceContext.from_defaults(
    llm=llm,
    embed_model=embed_model,
    node_parser=node_parser,
)

if not os.path.exists(&quot;./storage&quot;):
    # creating the vector store index
    index = VectorStoreIndex.from_documents(
        [document], service_context=sentence_context
    )

    # make vector store persistant
    index.storage_context.persist(persist_dir=&quot;./storage&quot;)
else:
    # load vector store indexed if they exist
    index = load_index_from_storage(
        StorageContext.from_defaults(persist_dir=&quot;./storage&quot;),
        service_context=sentence_context
    )
```

运行代码将在Python文件夹所在的项目目录中创建名为storage的新文件夹。

## 创建MetaDataReplacementPostProcessor

MetaDataReplacemen[tPostPro](https://www.zhihu.com/search?q=chunk%E6%A3%80%E7%B4%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)cessor在执行相关 chunk检索 后开始使用，用位于句子窗口内的实际周围文本替换检索到的节点周围的元数据。元数据替换后处理器会产生如下的结果：

![](img/068bd0be1847825028ec668cba45a339.png)

从上图可以看到，在第44行检索到了原始句子“Python is my most favorite language”。在45行和46行中应用了元数据后处理器。

## 添加Re-ranker

顾[名思义，reranker所](https://link.zhihu.com/?target=https%3A//huggingface.co/BAAI/bge-reranker-base)做的就是根据句子的相关性来重新排序。我们用BAAI/bge-reranker-base来执行重排序，模型在 hugging face 上可以找到。

那么，为什么需要重排序？下面从验证结果来说明重排序的作用：

![](img/4bbe82a7e43d44f7acfc03d71d0e294f.png)

这是上面截图的代码：

```
from llama_index import QueryBundle
from llama_index.schema import TextNode, NodeWithScore

query = QueryBundle(&quot;I love Python programming&quot;)

scored_nodes = [
    NodeWithScore(node=TextNode(text=&quot;Programming can be boring, bugs all day&quot;), score=0.6),
    NodeWithScore(node=TextNode(text=&quot;Python is my most favorite programming language&quot;), score=0.4),
]
```

可以看到原始的查询是“ I love Python Programming ”，我们有两个分数节点，手动分别分配了分数：0.6 和0.4。通过人工判断可知第二句与用户查询更相关，但手动分配的分数低。通过重排序，模型可以通过更改排名分数来帮助解决这一问题。

[第二句话比第](https://www.zhihu.com/search?q=%E9%87%8D%E6%8E%92%E5%BA%8F%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)一[句话更具有相关性，](https://www.zhihu.com/search?q=pipeline&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)理应获得更高的得分，这就是 重排序模型 所起的作用。我们用它将用户查询与现有节点进行匹配，从而找到最相关的节点。以下是将元数据替换后处理器和重排序添加到 pipeline 的代码：

```
[import os
fro](https://www.zhihu.com/search?q=postprocessor&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)m llama_index import (
    SimpleDirectoryReader,
    Document,
    StorageContext,
    load_index_from_storage
)

from llama_index.node_parser import SentenceWindowNodeParser
from llama_index.llms import OpenAI
from llama_index.embeddings import OpenAIEmbedding
from llama_index import ServiceContext
from llama_index import VectorStoreIndex
from llama_index.indices.postprocessor import MetadataReplacementPostProcessor
from llama_index.indices.postprocessor import SentenceTransformerRerank


from decouple import config

# set env variables
os.environ[&quot;OPENAI_API_KEY&quot;] = config(&quot;OPENAI_API_KEY&quot;)


# load document
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles/&quot;
).load_data(show_progress=True)


# merge pages into one
document = Document(text=&quot;\n\n&quot;.join([doc.text for doc in documents]))

node_parser = SentenceWindowNodeParser.from_defaults(
    window_size=3,
    window_metadata_key=&quot;window&quot;,
    original_text_metadata_key=&quot;original_text&quot;,
)

# creating OpenAI gpt-3.5-turbo LLM and OpenAIEmbedding model
llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;, temperature=0.1)
embed_model = OpenAIEmbedding()

# creating the service context
sentence_context = ServiceContext.from_defaults(
    llm=llm,
    embed_model=embed_model,
    node_parser=node_parser,
)

if not os.path.exists(&quot;./storage&quot;):
    # creating the vector store index
    index = VectorStoreIndex.from_documents(
        [document], service_context=sentence_context
    )

    # make vector store persistant
    index.storage_context.persist(persist_dir=&quot;./storage&quot;)
else:
    # load vector store indexed if they exist
    index = load_index_from_storage(
        StorageContext.from_defaults(persist_dir=&quot;./storage&quot;),
        service_context=sentence_context
    )


# add meta data replacement post processor
postproc = MetadataReplacementPostProcessor(
    target_metadata_key=&quot;window&quot;
)

# link: https://huggingface.co/BAAI/bge-reranker-base
rerank = SentenceTransformerRerank(
    top_n=2, model=&quot;BAAI/bge-reranker-base&quot;
)
```

代码执行之前需要安装相应的包 pip install torch s[entence-trans](https://www.zhihu.com/search?q=query%20engine&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)formers，[如果已经安](https://www.zhihu.com/search?q=%E6%9C%80%E7%BB%88%E4%BB%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)装请忽略。安装完成后，添加 query engine 并测试，以下是 最终代码 ：

```
[import os
f](https://www.zhihu.com/search?q=embed_model&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)rom llama_index im[port (
 ](https://www.zhihu.com/search?q=covid-19&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680767408%7D)   SimpleDirectoryReader,
    Document,
    StorageContext,
    load_index_from_storage
)

from llama_index.node_parser import SentenceWindowNodeParser
from llama_index.llms import OpenAI
from llama_index.embeddings import OpenAIEmbedding
from llama_index import ServiceContext
from llama_index import VectorStoreIndex
from llama_index.indices.postprocessor import MetadataReplacementPostProcessor
from llama_index.indices.postprocessor import SentenceTransformerRerank


from decouple import config

# set env variables
os.environ[&quot;OPENAI_API_KEY&quot;] = config(&quot;OPENAI_API_KEY&quot;)


# load document
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles/&quot;
).load_data(show_progress=True)


# merge pages into one
document = Document(text=&quot;\n\n&quot;.join([doc.text for doc in documents]))

node_parser = SentenceWindowNodeParser.from_defaults(
    window_size=3,
    window_metadata_key=&quot;window&quot;,
    original_text_metadata_key=&quot;original_text&quot;,
)

# creating OpenAI gpt-3.5-turbo LLM and OpenAIEmbedding model
llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;, temperature=0.1)
embed_model = OpenAIEmbedding()

# creating the service context
sentence_context = ServiceContext.from_defaults(
    llm=llm,
    embed_model=embed_model,
    node_parser=node_parser,
)

if not os.path.exists(&quot;./storage&quot;):
    # creating the vector store index
    index = VectorStoreIndex.from_documents(
        [document], service_context=sentence_context
    )

    # make vector store persistant
    index.storage_context.persist(persist_dir=&quot;./storage&quot;)
else:
    # load vector store indexed if they exist
    index = load_index_from_storage(
        StorageContext.from_defaults(persist_dir=&quot;./storage&quot;),
        service_context=sentence_context
    )


# add meta data replacement post processor
postproc = MetadataReplacementPostProcessor(
    target_metadata_key=&quot;window&quot;
)

# link: https://huggingface.co/BAAI/bge-reranker-base
rerank = SentenceTransformerRerank(
    top_n=2, model=&quot;BAAI/bge-reranker-base&quot;
)

# query engine
sentence_window_engine = index.as_query_engine(
    similarity_top_k=5, node_postprocessors=[postproc, rerank]
)

# test it out
response = sentence_window_engine.query(
    &quot;What did the president say about covid-19?&quot;
)

print(response)
```

至此已经成功地创建了sentence window retriever，但对模型使用和性能还有很多疑问，最佳的窗口大小是多少？sentence window retriever如何影响相关性和真实性？它又如何影响成本？相对于基础RAG管道和Parent document retrieval，它的表现如何？下面让我们来寻找这些问题的答案。

## 评估

在评估阶段，我们探讨以下问题：

+   使用时最佳句子窗口大小是多少？

+   在句子窗口大小和真实性或模型响应（幻觉）之间如何权衡。

+   句子窗口大小与模型响应相关性的关系

+   上下文相关性与答案真实性的关系

+   成本与窗口大小的关系

### 句子窗口大小和真实性或模型响应（幻觉）之间的权衡

随着句子窗口的增加，答案真实性也会上升直至到某一个点。这是因为LLM有更多的上下文作为响应的基础，而不是基于幻觉或训练数据。那么，为什么句子窗口大小和答案真实性在某一点之前成正比呢？当句子窗口很小时，LLM生成的响应将具有较低的真实性，因为上下文没有向LLM提供足够的信息来作为生成答案的依据——因此，它开始使用从训练数据中获得的现有知识，我们称之为幻觉。相反，如果窗口大小太大，由于LLM被提供了大量信息作为最终响应的生成依据，最终会偏离所提供的信息。因为提供的信息太多，LLM无法用其中所有信息合成响应。

下图解释了窗口大小与答案真实性的关系：

![](img/0f7056ad0fadcaba6b9892eb1284064b.png)

随着句子窗口大小的增加，答案真实性在前期成比例增加，达到最高值后随着句子窗口大小的增加开始下降。

### 句子窗口大小与模型响应相关性的关系

随着句子窗口大小的增加，生成的回答的相关性也可能增加到某一个点。首先，是不是上下文越多答案就越相关？在上下文过多的时候，LLM也许会产生偏差，求助于从训练数据学到的知识，从而产生幻觉；但上下文太少的时候，LLM也会产生幻觉，弥补上下文缺少的信息，从而造成相关性和真实性下降。在一些情况下，相关性可能很高，但答案真实性会下降。也许是训练数据包含了一些信息可以用来回答用户的特定问题。

依赖性的增加也意味着答案真实性性的增加，直到某一点相关性开始相对于上下文窗口的大小变得平缓或下降。

![](img/b8d3d72702f9b1e121708c6ce742d770.png)

### 成本与窗口大小的关系

随着语句窗口大小的增加，成本相应增加，因为越来越多的token被用于发出请求并得到回复。语句窗口越大，token越多，成本就越高。

![](img/59101efd24503f151ce6afbb8a3636ad.png)

下面让我们实际测试一下：

```
import os
from llama_index import (
    SimpleDirectoryReader,
    Document,
    StorageContext,
    load_index_from_storage
)

from llama_index.node_parser import SentenceWindowNodeParser
from llama_index.llms import OpenAI
from llama_index.embeddings import OpenAIEmbedding
from llama_index import ServiceContext
from llama_index import VectorStoreIndex
from llama_index.indices.postprocessor import MetadataReplacementPostProcessor
from llama_index.indices.postprocessor import SentenceTransformerRerank


from decouple import config

# set env variables
os.environ[&quot;OPENAI_API_KEY&quot;] = config(&quot;OPENAI_API_KEY&quot;)


# load document
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles/&quot;
).load_data(show_progress=True)


# merge pages into one
document = Document(text=&quot;\n\n&quot;.join([doc.text for doc in documents]))


def create_indexes(
    documents: Document,
    index_save_dir: str,
    window_size: int = 4,
    llm_model: str = &quot;gpt-3.5-turbo&quot;,
    temperature: float = 0.1
):
    node_parser = SentenceWindowNodeParser.from_defaults(
        window_size=window_size,
        window_metadata_key=&quot;window&quot;,
        original_text_metadata_key=&quot;original_text&quot;,
    )

    # creating OpenAI gpt-3.5-turbo LLM and OpenAIEmbedding model
    llm = OpenAI(model=llm_model, temperature=temperature)
    embed_model = OpenAIEmbedding()

    # creating the service context
    sentence_context = ServiceContext.from_defaults(
        llm=llm,
        embed_model=embed_model,
        node_parser=node_parser,
    )

    if not os.path.exists(index_save_dir):
        # creating the vector store index
        index = VectorStoreIndex.from_documents(
            [document], service_context=sentence_context
        )

        # make vector store persistant
        index.storage_context.persist(persist_dir=index_save_dir)
    else:
        # load vector store indexed if they exist
        index = load_index_from_storage(
            StorageContext.from_defaults(persist_dir=index_save_dir),
            service_context=sentence_context
        )

    return index


def create_query_engine(
    sentence_index: VectorStoreIndex,
    similarity_top_k: int = 6,
    rerank_top_n: int = 5,
    rerank_model: str = &quot;BAAI/bge-reranker-base&quot;,
):
    # add meta data replacement post processor
    postproc = MetadataReplacementPostProcessor(
        target_metadata_key=&quot;window&quot;
    )

    # link: https://huggingface.co/BAAI/bge-reranker-base
    rerank = SentenceTransformerRerank(
        top_n=rerank_top_n,
        model=rerank_model
    )

    sentence_window_engine = sentence_index.as_query_engine(
        similarity_top_k=similarity_top_k,
        node_postprocessors=[postproc, rerank]
    )

    return sentence_window_engine


# create index
index = create_indexes(
    documents=documents,
    index_save_dir=&quot;./storage&quot;,
    window_size=3,
    llm_model=&quot;gpt-3.5-turbo&quot;,
    temperature=0.1
)

# create query engine
sentence_window_engine = create_query_engine(
    sentence_index=index,
    similarity_top_k=5,
    rerank_top_n=2,
)

response = sentence_window_engine.query(
    &quot;What did the president say about covid-19?&quot;
)

print(response)
```

准备好一批测试问题之后就可以开始了！设置句子窗口大小为3和6，测试结果如下：

![](img/509fdba7670624cc054107d95414e4b8.png)

正如上面讨论的那样，可以看到到上下文大小、相关性和答案真实性的趋势。我们可以设置更大的上下文窗口大小、不同的embedding模型，甚至使用不同的LLM来处理这一问题，并找到最适合使用案例的RAG管道。

利用同样的一组问题，比较基础RAG管道、Parent Document Retrieval和 Sentence Windows Retrieval的表现。

![](img/3e84d502c0135e7d1bcfba97cdd7b893.png)

![](img/89eceb4cd88e75ef52fa26c6872a87b0.png)

从比较结果可以看出，使用句子窗口检索，所用的token更少，几乎是原来的1/4，相应的成本也更低。更好的是答案相关性、上下文相关性和答案真实性都表现不错。

有兴趣的同学可以阅读原文 [Advance Retrieval Techniques In RA](https://link.zhihu.com/?target=https%3A//medium.com/ai-advances/advance-retrieval-techniques-in-rag-part-03-sentence-window-retrieval-9f246cffa07b)G | Part 03 | Sentence Window Retrieval 进一步了解。