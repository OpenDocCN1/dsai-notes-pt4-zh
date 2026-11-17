
# 句子窗口检索器与重排序模型

```python
from llama_index import QueryBundle
from llama_index.schema import TextNode, NodeWithScore

query = QueryBundle("I love Python programming")

scored_nodes = [
    NodeWithScore(node=TextNode(text="Programming can be boring, bugs all day"), score=0.6),
    NodeWithScore(node=TextNode(text="Python is my most favorite programming language"), score=0.4),
]
```

可以看到原始的查询是 "I love Python Programming"，我们有两个分数节点，手动分别分配了分数：0.6 和 0.4。通过人工判断可知第二句与用户查询更相关，但手动分配的分数低。通过重排序，模型可以通过更改排名分数来帮助解决这一问题。

第二句话比第一句话更具有相关性，理应获得更高的得分，这就是 重排序模型 所起的作用。我们用它将用户查询与现有节点进行匹配，从而找到最相关的节点。以下是将元数据替换后处理器和重排序添加到 pipeline 的代码：

```python
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
os.environ["OPENAI_API_KEY"] = config("OPENAI_API_KEY")


# load document
documents = SimpleDirectoryReader(
    input_dir="../dataFiles/"
).load_data(show_progress=True)


# merge pages into one
document = Document(text="\n\n".join([doc.text for doc in documents]))

node_parser = SentenceWindowNodeParser.from_defaults(
    window_size=3,
    window_metadata_key="window",
    original_text_metadata_key="original_text",
)

# creating OpenAI gpt-3.5-turbo LLM and OpenAIEmbedding model
llm = OpenAI(model="gpt-3.5-turbo", temperature=0.1)
embed_model = OpenAIEmbedding()

# creating the service context
sentence_context = ServiceContext.from_defaults(
    llm=llm,
    embed_model=embed_model,
    node_parser=node_parser,
)

if not os.path.exists("./storage"):
    # creating the vector store index
    index = VectorStoreIndex.from_documents(
        [document], service_context=sentence_context
    )

    # make vector store persistant
    index.storage_context.persist(persist_dir="./storage")
else:
    # load vector store indexed if they exist
    index = load_index_from_storage(
        StorageContext.from_defaults(persist_dir="./storage"),
        service_context=sentence_context
    )


# add meta data replacement post processor
postproc = MetadataReplacementPostProcessor(
    target_metadata_key="window"
)

# link: https://huggingface.co/BAAI/bge-reranker-base
rerank = SentenceTransformerRerank(
    top_n=2, model="BAAI/bge-reranker-base"
)
```

代码执行之前需要安装相应的包 `pip install torch sentence-transformers`，如果已经安装请忽略。安装完成后，添加 query engine 并测试，以下是最终代码：

```python
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
os.environ["OPENAI_API_KEY"] = config("OPENAI_API_KEY")


# load document
documents = SimpleDirectoryReader(
    input_dir="../dataFiles/"
).load_data(show_progress=True)


# merge pages into one
document = Document(text="\n\n".join([doc.text for doc in documents]))

node_parser = SentenceWindowNodeParser.from_defaults(
    window_size=3,
    window_metadata_key="window",
    original_text_metadata_key="original_text",
)

# creating OpenAI gpt-3.5-turbo LLM and OpenAIEmbedding model
llm = OpenAI(model="gpt-3.5-turbo", temperature=0.1)
embed_model = OpenAIEmbedding()

# creating the service context
sentence_context = ServiceContext.from_defaults(
    llm=llm,
    embed_model=embed_model,
    node_parser=node_parser,
)

if not os.path.exists("./storage"):
    # creating the vector store index
    index = VectorStoreIndex.from_documents(
        [document], service_context=sentence_context
    )

    # make vector store persistant
    index.storage_context.persist(persist_dir="./storage")
else:
    # load vector store indexed if they exist
    index = load_index_from_storage(
        StorageContext.from_defaults(persist_dir="./storage"),
        service_context=sentence_context
    )


# add meta data replacement post processor
postproc = MetadataReplacementPostProcessor(
    target_metadata_key="window"
)

# link: https://huggingface.co/BAAI/bge-reranker-base
rerank = SentenceTransformerRerank(
    top_n=2, model="BAAI/bge-reranker-base"
)

# query engine
sentence_window_engine = index.as_query_engine(
    similarity_top_k=5, node_postprocessors=[postproc, rerank]
)

# test it out
response = sentence_window_engine.query(
    "What did the president say about covid-19?"
)

print(response)
```

至此已经成功地创建了 sentence window retriever，但对模型使用和性能还有很多疑问，最佳的窗口大小是多少？sentence window retriever 如何影响相关性和真实性？它又如何影响成本？相对于基础 RAG 管道和 Parent document retrieval，它的表现如何？下面让我们来寻找这些问题的答案。

## 评估

在评估阶段，我们探讨以下问题：

+   使用时最佳句子窗口大小是多少？
+   在句子窗口大小和真实性或模型响应（幻觉）之间如何权衡。
+   句子窗口大小与模型响应相关性的关系
+   上下文相关性与答案真实性的关系
+   成本与窗口大小的关系

### 句子窗口大小和真实性或模型响应（幻觉）之间的权衡

随着句子窗口的增加，答案真实性也会上升直至到某一个点。这是因为 LLM 有更多的上下文作为响应的基础，而不是基于幻觉或训练数据。那么，为什么句子窗口大小和答案真实性在某一点之前成正比呢？当句子窗口很小时，LLM 生成的响应将具有较低的真实性，因为上下文没有向 LLM 提供足够的信息来作为生成答案的依据——因此，它开始使用从训练数据中获得的现有知识，我们称之为幻觉。相反，如果窗口大小太大，由于 LLM 被提供了大量信息作为最终响应的生成依据，最终会偏离所提供的信息。因为提供的信息太多，LLM 无法用其中所有信息合成响应。

下图解释了窗口大小与答案真实性的关系：

![](img/0f7056ad0fadcaba6b9892eb1284064b.png)

随着句子窗口大小的增加，答案真实性在前期成比例增加，达到最高值后随着句子窗口大小的增加开始下降。

### 句子窗口大小与模型响应相关性的关系

随着句子窗口大小的增加，生成的回答的相关性也可能增加到某一个点。首先，是不是上下文越多答案就越相关？在上下文过多的时候，LLM 也许会产生偏差，求助于从训练数据学到的知识，从而产生幻觉；但上下文太少的时候，LLM 也会产生幻觉，弥补上下文缺少的信息，从而造成相关性和真实性下降。在一些情况下，相关性可能很高，但答案真实性会下降。也许是训练数据包含了一些信息可以用来回答用户的特定问题。

依赖性的增加也意味着答案真实性的增加，直到某一点相关性开始相对于上下文窗口的大小变得平缓或下降。

![](img/b8d3d72702f9b1e121708c6ce742d770.png)

### 成本与窗口大小的关系

随着语句窗口大小的增加，成本相应增加，因为越来越多的 token 被用于发出请求并得到回复。语句窗口越大， token 越多，成本就越高。

![](img/59101efd24503f151ce6afbb8a3636ad.png)

下面让我们实际测试一下：
