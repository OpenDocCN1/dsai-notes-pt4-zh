# RAG中的高级检索技巧（1）-基础RAG管道创建

> 来源：[https://agijuejin.feishu.cn/docx/TXakdVryUo1fWix2siNcyX6Knoc](https://agijuejin.feishu.cn/docx/TXakdVryUo1fWix2siNcyX6Knoc)

> 作者：lucas大叔

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/680007979)hihu.com/p/680007...

## **摘要**

基于技术文章《 [Advanced Retrieva](https://link.zhihu.com/?target=https%3A//ai.gopubby.com/advance-retrieval-techniques-in-rag-5fdda9cc304b)l Techniques In RAG 》翻译整理而成，对RAG中涉及到的实用技巧进行梳理。本文构建了基础RAG管道并验证其性能，基于Small-to-Big Retrieval的优化将在后续文章中展开。

## 背景

当在RAG系统中构建 [知识库 ](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)时，需要将文本进行分割、 [embedding ](https://www.zhihu.com/search?q=embedding&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)入库，检[索到的文本](https://www.zhihu.com/search?q=%E9%AB%98%E7%BA%A7%E6%A3%80%E7%B4%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)质量对大模型生成答案的质量具有至关重要的影响。检索到的与回答用户query相关的文本质量越高，得到的答案就越有根据和相关性，也更容易阻止LLM生成错误或不是基于特定领域知识的答案。作为RAG管道的第一个环节，其他环节都取决于检索到文本的质量和检索机制上。本文介绍了一种称之为small-to-big retrieval的 高级检索 技术。

## Small-to-Big Retrieval

在构建RAG管道时，我们通常要把特定领域的文本[分割成更小的](https://www.zhihu.com/search?q=chunk&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)chunk或片段。假设每个 chunk 或片段包含200个单词，用户问了一个问题，而这个问题只用200个单词中的一行句子就可以回答，chunk中其余的文本可能会使检索器很难找到与回答用户问题最相关的那句话。有时候分割的chunk会有多达1000多个单词，在其中发现一句相关的句子就像大海捞针一样，是个非常大的挑战。

那怎么解决这种问题呢？本文引入了“Small-to-Big R[etrie](https://www.zhihu.com/search?q=%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)val”技术。在传统的RAG 应用程序 中，我们在chunk上执行搜索检索，并将相同的chunk传递给LLM来合成或生成答案。如果我们将检索到的chunk和传递给LLM的chunk两者解耦，这不是更好吗？也就是说，我们有一段文本或chunk用于搜索，另一段chunk用于生成答案。用于搜索的chunk比用于合成或生成最终答案的chunk小得多，以避免出现繁琐的问题。

下面我们举个栗子来说明“ Small-to-Big Retrieval ”技术。取一个100个单词的chunk，对于这个chunk创建10个较小的chunk，每个chunk10个单词（大致为一个句子）。我们将使用这10个较小的chunk来进行搜索，以找到与回答用户问题最相关的句子。当用户提问时，我们可以很容易地从10个较小的chunk中找到最相关的句子。换言之，直接切入正题，不浪费非必要的操作。此外，基于embedding的检索在较小的文本大小下效果最好。chunk大小越小，嵌入后就越准确地反映其语义。

从10个较小的句子中找到最相关的句子后该怎么办？一种直接的想法是把它直接给到LLM，根据检索到的最相关的句子合成一个响应。这样做是可行的，但LLM获取不到足够的上下文，容易输出幻觉。因为只把最相关的较小chunk传给LLM，而缺少必要的上下文，它会自行填补相关的知识空白和对整个数据的理解，这就有可能生成与事实不相符的答案。

解决这个问题的一种方法是让较小的chunk与父chunk相关，这样LLM将有更多的上下文来作为答案的依据，[而不是试图生成自](https://www.zhihu.com/search?q=%E8%BE%83%E5%B0%8Fchunk&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)己的幻觉信息。换言之，10个单词的每个较小chunk将与100个单词的较大chunk相关，并且当给定的 较小chunk （10个单词chunk）被识别为与回答用户问题相关时，父chunk（100个单词chunk）将被检索并被发送到LLM。

这就是“Small-to-Big”检索技术背后的全部思想，而且效果非常好。下面将介绍两种类型的“Small-to-Big ”检索。

## “Small-to-Big ”检索

### Parent Document Retrieval

首先检索与回答查询最相关的较小数据段，然后使用它们相关的父标识符访问并返回将作为上下文传递给LLM的较大父chunk。在LangChain中可以通过 ParentDocumentRetriever 来实现。

![](img/ee8e8f56ec0d904bb30328b70f50a1d7.png)

### Sentence Window Retrieval

Sentence Window Retrieval首先检索与回答查询最相关的特定句子，然后返回该句子周围更多的文本，为LLM提供更广泛的上下文作为其响应的基础。

![](img/183df7076a10040c9b2765e852bdfdba.png)

![](img/a180f45ac7788690ef75440c2322075a.png)

## 创建基础RAG管道

此处进入实践环节，展示如何利用 LlamaIndex 创建RAG应用。以下是基础RAG管道的简单图表展示：

![](img/04b58fb53675bb852bc0737e2c972083.png)

首先，安装必要的python库

```
pip install llama-index python-decouple trulens-eval
```

### 加载文档

```
from llama_ind[ex import (](https://www.zhihu.com/search?q=environment&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)
    VectorStoreIndex,
    SimpleDirectoryReader
)

# for loading environment variables
from decouple import config

import os

# set env variables
os.environ[&quot;OPENAI_API_KEY&quot;] = config(&quot;OPENAI_API_KEY&quot;)

# load document
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles&quot;).load_data(show_progress=True)

# check document
print(len(documents))
print(documents[0])
```

### 文档分割

```
[# create no](https://www.zhihu.com/search?q=node_parser&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)des parser
node_parser = SimpleNodeParser.from_defaults(chunk_size=1024)
# split into nodes
base_nodes = node_parser.get_nodes_from_documents(documents=documents)
# examine base nodes
print(len(base_nodes))
print(base_nodes[0])
```

### 配置embedding模型和LLM

[此处在LlamaIndex中](https://www.zhihu.com/search?q=gpt-3.5-turbo&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)使用 gpt-3.5-turbo LLM，embedding模型使用的是OpenAI的text-davinci-003模型。大家可以根据实际情况替换为其他的LLM和embedding模型。

```
# cr[eate LLM an](https://www.zhihu.com/search?q=embed_model&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)d Embedding Model
embed_model = OpenAIEmbedding()
llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;)
service_context = ServiceContext.from_defaults(
    embed_model=embed_model, llm=llm)
```

### 创建索引

我们将创建一个索引，基本上是一个向量存储来存储嵌入，并对该文档执行查询以获得相关或类似的文档。

```
[ # cr](https://www.zhihu.com/search?q=nodes&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)eating index
index = VectorStoreIndex(nodes=base_nodes, service_context=service_context)

# create retriever
retriever = index.as_retriever(similarity_top_k=2)
# test retriever
retrieved_nodes = retriever.retrieve(&quot;What did the president say about covid-19&quot;)
print(retrieved_nodes)
print(&quot;\n\n\n=============================================================&quot;)
print(&quot;Node Texts&quot;)
for node in retrieved_nodes:
print(node.text)
# get word count for each doc
print(len(node.text.split()))
print(&quot;==&quot; * 10)
```

### 将检索器插入 [检索引擎](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E5%BC%95%E6%93%8E&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)

```
# query retriever engine
query_engine = RetrieverQueryEngine.from_args(
    retriever=retriever,
    service_context=service_context
)
# test response
response = query_engine.query(&quot;What did the president say about covid-19&quot;)
print(response)
```

### 持久化向量存储

每次我们运行代码时，都会创建新的embedding。因为到目前为止，embedding只存储在内存中，这并不是最好的策略。我们改造一下代码，把它存储在文件或磁盘上的存储中，每次程序只需简单地加载就ok。

```
 from llama_index import (
    StorageContext,
    VectorStoreIndex,
    SimpleDirectoryReader,
    ServiceContext,
    load_index_from_storage
)
from llama_index.node_parser import SimpleNodeParser
from llama_index.embeddings import OpenAIEmbedding
from llama_index.llms import OpenAI
from llama_index.query_engine import RetrieverQueryEngine
# for loading environment variables
from decouple import config
import os
# set env variables
os.environ[&quot;OPENAI_API_KEY&quot;] = config(&quot;OPENAI_API_KEY&quot;)

# create LLM and Embedding Model
embed_model = OpenAIEmbedding()
llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;)
service_context = ServiceContext.from_defaults(
    embed_model=embed_model, llm=llm)

# check if data indexes already exists
if not os.path.exists(&quot;./storage&quot;):
 # load data
    documents = SimpleDirectoryReader(
        input_dir=&quot;../dataFiles&quot;).load_data(show_progress=True)
 # create nodes parser
    node_parser = SimpleNodeParser.from_defaults(chunk_size=1024)
 # split into nodes
    base_nodes = node_parser.get_nodes_from_documents(documents=documents)
 # creating index
    index = VectorStoreIndex(nodes=base_nodes, service_context=service_context)
 # store index
    index.storage_context.persist()
else:
 # load existing index
    storage_context = StorageContext.from_defaults(persist_dir=&quot;./storage&quot;)
    index = load_index_from_storage(storage_context=storage_context)

# create retriever
retriever = index.as_retriever(similarity_top_k=2)
# query retriever engine
query_engine = RetrieverQueryEngine.from_args(
    retriever=retriever,
    service_context=service_context
)
# test response
response = query_engine.query(&quot;What did the president say about covid-19&quot;)
print(response)
```

运行完这段代码后，在项目目录中将创建名为storage的文件夹，包含所有的embedding。这意味着不必每次运行代码时都创建embedding，不仅更快，而且更便宜。

## RAG管道评估

为了真正了解RAG管道的性能，对其进行评估是非常重要的。通过检查一些指标，确保RAG应用程序免受黑客攻击和其他不希望在生产环境中发生在应用程序上的影响。我们将使用RAG Triad评估刚刚创建的基础RAG管道。

### RAG Triad

RAG Triad由训练主要指标组成，我们可以使用这些指标来评估模型并量化其性能。

![](img/60854e873aee2591fc3dd1b86f3364bf.png)

Answer Relevance（答案相关性）：答案与查询或用户问题的相关性如何？

Context Relevance（上下文相关性）：检索到的上下文与回答用户问题的相关性如何？

Groundedness（基础性）：检索到的上下文在多大程度上支持响应？

以下是用来评估RAG管道的代码。

```
 from[ llama_index](https://www.zhihu.com/search?q=trulens_eval&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D) import (
    StorageContext,
    VectorStoreIndex,
    SimpleDirectoryReader,
    ServiceContext,
    load_index_from_storage
)
from llama_index.node_parser import SimpleNodeParser
from llama_index.embeddings import OpenAIEmbedding
from llama_index.llms import OpenAI
from llama_index.query_engine import RetrieverQueryEngine
from trulens_eval import Feedback, Tru, TruLlama
from trulens_eval.feedback import Groundedness
from trulens_eval.feedback.provider.openai import OpenAI as OpenAITruLens
import numpy as np
# for loading environment variables
from decouple import config
import os
# set env variables
os.environ[&quot;OPENAI_API_KEY&quot;] = config(&quot;OPENAI_API_KEY&quot;)

# create LLM and Embedding Model
embed_model = OpenAIEmbedding()
llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;)
service_context = ServiceContext.from_defaults(
    embed_model=embed_model, llm=llm)

# check if data indexes already exists
if not os.path.exists(&quot;./storage&quot;):
 # load data
    documents = SimpleDirectoryReader(
        input_dir=&quot;../dataFiles&quot;).load_data(show_progress=True)
 # create nodes parser
    node_parser = SimpleNodeParser.from_defaults(chunk_size=1024)
 # split into nodes
    base_nodes = node_parser.get_nodes_from_documents(documents=documents)
 # creating index
    index = VectorStoreIndex(nodes=base_nodes, service_context=service_context)
 # store index
    index.storage_context.persist()
else:
 # load existing index
    storage_context = StorageContext.from_defaults(persist_dir=&quot;./storage&quot;)
    index = load_index_from_storage(storage_context=storage_context)

# create retriever
retriever = index.as_retriever(similarity_top_k=2)
# query retriever engine
query_engine = RetrieverQueryEngine.from_args(
    retriever=retriever,
    service_context=service_context
)
# RAG pipeline evals
tru = Tru()
openai = OpenAITruLens()
grounded = Groundedness(groundedness_provider=OpenAITruLens())
# Define a groundedness feedback function
f_groundedness = Feedback(grounded.groundedness_measure_with_cot_reasons).on(
    TruLlama.select_source_nodes().node.text
    ).on_output(
    ).aggregate(grounded.grounded_statements_aggregator)
# Question/answer relevance between overall question and answer.
f_qa_relevance = Feedback(openai.relevance).on_input_output()
# Question/statement relevance between question and each context chunk.
f_qs_relevance = Feedback(openai.qs_relevance).on_input().on(
    TruLlama.select_source_nodes().node.text
    ).aggregate(np.mean)

tru_query_engine_recorder = TruLlama(query_engine,
    app_id=&#x27;LlamaIndex_App1&#x27;,
    feedbacks=[f_groundedness, f_qa_relevance, f_qs_relevance])

# eval using context window
with tru_query_engine_recorder as recording:
    query_engine.query(&quot;What did the president say about covid-19&quot;)

# run dashboard
tru.run_dashboard()
```

这应该会打开一个 [web应用程序 ](https://www.zhihu.com/search?q=web%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)，您可以访问该应用程序以查看模型性能指标：

![](img/fd606226cc8edf481446a2d9572df5db.png)

以下是仪表板的一些截图：

![](img/54d4d5a0c307056d44eaea73f8963d59.png)

![](img/d388b13e447356598aa3a6dc3a117036.png)

![](img/dbf0a4dea865e708e77377a5ac89cfa3.png)

### 评估结果的解释

上[下文相关性：RAG应用程](https://www.zhihu.com/search?q=embedding%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680007979%7D)序在检索最相关的文档方面似乎表现不佳，问题的相关性是0.2 。可以采取一些方法对此进行微调，例如使用不同的 embedding模型 、更改chunk大小等。

基础性和回答相关性：基础性和答案相关性都很好，而且很高。如果需要的话，我们也可以通过一些方法来获得更好的结果。当我们深入研究Sentence Window Retriever和Parent Document Retrievers时，将在接下来的文章中看到更多内容，敬请期待！