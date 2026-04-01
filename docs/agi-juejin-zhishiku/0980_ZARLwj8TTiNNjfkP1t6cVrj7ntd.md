# RAG中的高级检索技巧（2）-Parent Document Retrieval

> 来源：[https://agijuejin.feishu.cn/docx/W7oqddvA3ouuAqxuTp6cx2DXnCc](https://agijuejin.feishu.cn/docx/W7oqddvA3ouuAqxuTp6cx2DXnCc)

> 🔗 原文链接： h[ttps://zhuanlan.zh](https://zhuanlan.zhihu.com/p/680258692)ihu.com/p/680258...

> ⏰ 剪存时间：2024-08-26 16:07:24 (UTC+8)

> ✂️ 本文档由 [飞书剪存 ](https://www.feishu.cn/hc/zh-CN/articles/606278856233?from=in_ccm_clip_doc)一键生成

上节我们介绍了如何创建简单的RAG管道，本节将深入探讨在RAG管道中parent document retrieval是如何工作的。

## 数据摄入

为了将数据存入向量存储并持久化，我们遵循以下几步来实现。

### 创建子节点或子块

对于1024字符长的文本段，我们进一步划分为更小的段：

+   8段，每段128个字符

+   4段，每段256个字符

+   2段，每段512个字符

[此外，我](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E6%AE%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680258692%7D)们还将最初的1024个字符的 文本段 包含到段集合中。

最后我们将得到173个单独的节点，设置 chunk_overlap=0 至关重要。下面通过代码来实现：

```
[from llama](https://www.zhihu.com/search?q=os.environ&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680258692%7D)[_index impor](https://www.zhihu.com/search?q=chunk_size%3Dc&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680258692%7D)t (
    Document,
    StorageContext,
    VectorStoreIndex,
    SimpleDirectoryReader,
    ServiceContext,
    load_index_from_storage,
)
from llama_index.retrievers import RecursiveRetriever
from llama_index.node_parser import SimpleNodeParser
from llama_index.embeddings import OpenAIEmbedding
from llama_index.schema import IndexNode
from llama_index.llms import OpenAI

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

# load data
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles&quot;).load_data(show_progress=True)
doc_text = &quot;\n\n&quot;.join([d.get_content() for d in documents])
docs = [Document(text=doc_text)]

# create nodes parser
node_parser = SimpleNodeParser.from_defaults(chunk_size=1024)

# split into nodes
base_nodes = node_parser.get_nodes_from_documents(documents=docs)

# set document IDs
for idx, node in enumerate(base_nodes):
    node.id_ = f&quot;node-{idx}&quot;

# create parent child documents
sub_chunk_sizes = [128, 256, 512]
sub_node_parsers = [
    SimpleNodeParser.from_defaults(chunk_size=c, chunk_overlap=0) for c in sub_chunk_sizes
]
all_nodes = []
for base_node in base_nodes:
 for n in sub_node_parsers:
        sub_nodes = n.get_nodes_from_documents([base_node])
        sub_inodes = [
            IndexNode.from_text_node(sn, base_node.node_id) for sn in sub_nodes
        ]
        all_nodes.extend(sub_inodes)
 # also add original node to node
    original_node = IndexNode.from_text_node(base_node, base_node.node_id)
    all_nodes.append(original_node)
 
all_nodes_dict = {n.node_id: n for n in all_nodes}

print(all_nodes[0])
print(len(all_nodes_dict))

# creating index
index = VectorStoreIndex(nodes=all_nodes, service_context=service_context)
```

## [创建检索器 ](https://www.zhihu.com/search?q=%E5%88%9B%E5%BB%BA%E6%A3%80%E7%B4%A2%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A680258692%7D)（Recursive Retriever）

```
# creating a chunk retriever
vector_retriever_chunk = index.as_retriever(similarity_top_k=2)

retriever_chunk = RecursiveRetriever(
 &quot;vector&quot;,
    retriever_dict={&quot;vector&quot;: vector_retriever_chunk},
    node_dict=all_nodes_dict,
    verbose=True,
)

# retrieve needed nodes
nodes = retriever_chunk.retrieve(
&quot;Can you tell me about the key concepts for safety finetuning&quot;
)
for node in nodes:
    print(node)
```

上面的代码只检索节点并打印到屏幕上，以下是代码的输出：

![](img/d56590d4efeedb9a863849df1bc44600.png)

接下来实现RetrieveQueryEngine：

```
from llama_index import (
    Document,
    StorageContext,
    VectorStoreIndex,
    SimpleDirectoryReader,
    ServiceContext,
    load_index_from_storage,
)
from llama_index.retrievers import RecursiveRetriever
from llama_index.query_engine import RetrieverQueryEngine
from llama_index.node_parser import SimpleNodeParser
from llama_index.embeddings import OpenAIEmbedding
from llama_index.schema import IndexNode
from llama_index.llms import OpenAI

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

# load data
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles&quot;).load_data(show_progress=True)
doc_text = &quot;\n\n&quot;.join([d.get_content() for d in documents])
docs = [Document(text=doc_text)]

# create nodes parser
node_parser = SimpleNodeParser.from_defaults(chunk_size=1024)

# split into nodes
base_nodes = node_parser.get_nodes_from_documents(documents=docs)

# set document IDs
for idx, node in enumerate(base_nodes):
    node.id_ = f&quot;node-{idx}&quot;
# create parent child documents
sub_chunk_sizes = [128, 256, 512]
sub_node_parsers = [
    SimpleNodeParser.from_defaults(chunk_size=c, chunk_overlap=0) for c in sub_chunk_sizes
]
all_nodes = []
for base_node in base_nodes:
 for n in sub_node_parsers:
        sub_nodes = n.get_nodes_from_documents([base_node])
        sub_inodes = [
            IndexNode.from_text_node(sn, base_node.node_id) for sn in sub_nodes
        ]
        all_nodes.extend(sub_inodes)
 # also add original node to node
    original_node = IndexNode.from_text_node(base_node, base_node.node_id)
    all_nodes.append(original_node)
 
all_nodes_dict = {n.node_id: n for n in all_nodes}

# creating index
index = VectorStoreIndex(nodes=all_nodes, service_context=service_context)

# creating a chunk retriever
vector_retriever_chunk = index.as_retriever(similarity_top_k=2)

retriever_chunk = RecursiveRetriever(
 &quot;vector&quot;,
    retriever_dict={&quot;vector&quot;: vector_retriever_chunk},
    node_dict=all_nodes_dict,
    verbose=True,
)
query_engine_chunk = RetrieverQueryEngine.from_args(
    retriever_chunk, service_context=service_context
)
response = query_engine_chunk.query(
 &quot;What did the president say about Covid-19&quot;
)
print(str(response))
```

运行代码输出如下：

![](img/800d21d71c9afdca16c8e330e25f6110.png)

## 评估

接下来用上节同样的数据测试评估管道的性能。

```
from typing import List
from llama_index import (
    Document,
    VectorStoreIndex,
    SimpleDirectoryReader,
    ServiceContext,
)
from llama_index.retrievers import RecursiveRetriever
from llama_index.query_engine import RetrieverQueryEngine
from llama_index.node_parser import SimpleNodeParser
from llama_index.embeddings import OpenAIEmbedding
from llama_index.schema import IndexNode
from llama_index.llms import OpenAI

# for loading environment variables
from decouple import config
 
import os

from trulens_eval import Feedback, Tru, TruLlama
from trulens_eval.feedback import Groundedness
from trulens_eval.feedback.provider.openai import OpenAI as OpenAITruLens

import numpy as np

# set env variables
os.environ[&quot;OPENAI_API_KEY&quot;] = config(&quot;OPENAI_API_KEY&quot;)

# create LLM and Embedding Model
embed_model = OpenAIEmbedding()
llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;)
service_context = ServiceContext.from_defaults(
    embed_model=embed_model, llm=llm)

# load data
documents = SimpleDirectoryReader(
    input_dir=&quot;../dataFiles&quot;).load_data(show_progress=True)
doc_text = &quot;\n\n&quot;.join([d.get_content() for d in documents])
docs = [Document(text=doc_text)]

# create nodes parser
node_parser = SimpleNodeParser.from_defaults(chunk_size=1024)

# split into nodes
base_nodes = node_parser.get_nodes_from_documents(documents=docs)

# set document IDs
for idx, node in enumerate(base_nodes):
    node.id_ = f&quot;node-{idx}&quot;

# create parent child documents
sub_chunk_sizes = [128, 256, 512]
sub_node_parsers = [
    SimpleNodeParser.from_defaults(chunk_size=c, chunk_overlap=0) for c in sub_chunk_sizes
]
all_nodes = []
for base_node in base_nodes:
 for n in sub_node_parsers:
        sub_nodes = n.get_nodes_from_documents([base_node])
        sub_inodes = [
            IndexNode.from_text_node(sn, base_node.node_id) for sn in sub_nodes
        ]
        all_nodes.extend(sub_inodes)
 # also add original node to node
    original_node = IndexNode.from_text_node(base_node, base_node.node_id)
    all_nodes.append(original_node)
 
all_nodes_dict = {n.node_id: n for n in all_nodes}

# creating index
index = VectorStoreIndex(nodes=all_nodes, service_context=service_context)

# creating a chunk retriever
vector_retriever_chunk = index.as_retriever(similarity_top_k=2)

retriever_chunk = RecursiveRetriever(
 &quot;vector&quot;,
    retriever_dict={&quot;vector&quot;: vector_retriever_chunk},
    node_dict=all_nodes_dict,
    verbose=True,
)
query_engine_chunk = RetrieverQueryEngine.from_args(
    retriever_chunk, service_context=service_context
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

tru_query_engine_recorder = TruLlama(query_engine_chunk,
    app_id=&#x27;Parent_document_retrieval&#x27;,
    feedbacks=[f_groundedness, f_qa_relevance, f_qs_relevance])
eval_questions = []
with open(&quot;./eval_questions.txt&quot;, &quot;r&quot;) as eval_qn:
 for qn in eval_qn:
        qn_stripped = qn.strip()
        eval_questions.append(qn_stripped)

def run_eval(eval_questions: List[str]):
 for qn in eval_questions:
 # eval using context window
 with tru_query_engine_recorder as recording:
            query_engine_chunk.query(qn)

run_eval(eval_questions=eval_questions)

# run dashboard
tru.run_dashboard()
```

运行效果如下：

![](img/755c12773b72db2c9fd43c93c3a515bd.png)

打开Streamlit 应用程序：

![](img/6c029c560acc37e64dec4664ac20b309.png)

从上图中，我们可以看到，与基础RAG管道LlamaIndex_App1应用程序相比，Parent Document Retrieval管道表现更好。

原文参见 [Advanced Retrieval Techniques In R](https://link.zhihu.com/?target=https%3A//ai.gopubby.com/advanced-retrieval-techniques-in-rag-part-02-parent-document-retrieval-334cd924a36e)AG | Part 02 | Parent Document Retrieval