
# Advanced RAG(高级检索增强生成)从理论到实战

> 来源：[https://gotoaiworld.feishu.cn/docx/SUrgdIxsso3AlzxUyTIchmFMnqc](https://gotoaiworld.feishu.cn/docx/SUrgdIxsso3AlzxUyTIchmFMnqc)

## 前言

在《AI大语言模型LLM学习-RAG技术及代码实现》一文介绍了RAG的相关知识，其中介绍到RAG的发展经历三个主要阶段：原始（Native RAG）、高级（Advanced RAG）和模块化RAG（Modular RAG），文中实战使用实际案例介绍了原始（Native RAG）的代码实现；本文将重点介绍高级（Advanced RAG）的概念及代码实现。

## 一、RAG 技术真的“烂大街”了吗？

本章节是在👸号文章《RAG 技术真的“烂大街”了吗？》的基础上进行的提炼和个人的部分理解，有兴趣的小伙伴们可以去阅读下原文。

### 1.1 RAG的应用的两个阶段

最初，大约是去年（2023年），是 RAG 技术的普及期。在这个阶段，业界对使用 RAG 还是微调存在争议。

从今年（2024年）开始，是 RAG 加速普及的时期，企业普遍认识到 RAG 技术的必要性。

### 1.2 RAG需要优化的问题

包括检索文档内容解析错误、边缘案例处理不当、解析速度慢、知识库更新耗时长、机械分块丢失语义信息、目标检索内容召回不全、召回结果排序困难、答案生成存在遗漏等。

### 1.3 核心观点

RAG 技术本质上是一种普及性的架构，而非特定于某个场景的解决方案。它是大模型服务 B 端市场的一种方式，因此我们不能因为短期内效果不佳就认为这项技术会被未来替代。

长上下文模型和 RAG 之间不应是冲突关系，而应是合作关系。

“烂大街”的问题这代表降低 RAG 技术理解和使用的门槛。

“烂大街”也有另一层含义，即技术看起来很好，但实际使用效果不佳，需要进一步加工和调整。这是我们需要优化和避免的。我们要提升 RAG 在检索和生成每个细节环节的效果，确保技术不仅可用，而且越来越好用。

目前大家关注的是 Naive RAG ，而不是包含 Advanced 或 Modular 特性的 RAG 。现在的关注点确实是纯粹的问题检索，核心在于单轮检索的准确性和速度。在现实场景中，问题往往更复杂，企业希望结合他们的知识库解决项目中的问题，这些需求的复杂性往往无法通过单轮 RAG 来解决。

RAG优化方向：比如提高数据源清洗的准确性，加快知识库更新速度，改进 `trunk` 分配的智能化，提升 `embedding` 模型的性能，以及优化 `rerank` 模型和 `prompt` 生成的最佳实践。

### 1.4 RAG 的未来发展

RAG 的演进可以从四个阶段来考虑：数据抽取、数据预处理、索引和查询改写。

#### 第一阶段：数据抽取

在数据抽取方面，理想的情况是有一个能够处理各种文档的抽取大模型，无论文档中包含什么内容，如流程图、柱状图、饼图等，都能解读出来。如果能够开发出这样的大模型，它将解决 RAG 在数据落地方面的许多痛点。

#### 第二阶段：数据预处理

数据预处理目前主要包括 `embedding` 模型和知识图谱的抽取。`Embedding` 模型在一些垂直场景中的应用需要进一步优化，以减少向量干扰。知识图谱的自动化构建，如微软的 Graph RAG 所展示的，为 RAG 问答质量的提升提供了一个很好的起点，尤其是对于多跳或长文本问答。

#### 第三阶段：索引

索引阶段，我们已经尝试了多种搜索手段，包括全文搜索、向量搜索、系数向量搜索，甚至张量搜索。IBM 研究院提出的 Blended RAG 通过三路召回混合搜索，可以达到最佳效果。我们复现了这一结果，并发现使用张量等手段可以进一步提升效果。

#### 第四阶段：查询改写和排序

最后一个阶段是查询改写和排序。这一环节的技术进步将进一步影响 RAG 的迭代和发展。

## 二、Advanced RAG（高级RAG）

下图是两类RAG范式的流程对比：

通过对比可以发现Advanced RAG比Native RAG多了两个阶段：

Pre-Retrieval（预检索）

Post-Retrieval（检索后）

Advanced RAG重点聚焦在提高检索质量。

### 2.1 Pre-Retrieval(预检索)

Pre-Retrieval阶段重点关注的是索引结构和原始查询的优化，提高被索引内容的质量，可用使用的策略有：增强数据粒度、优化索引结构、添加元数据、对齐优化和混合检索，这样的优化使得用户的原始问题更清晰，更适合检索任务。

Pre-Retrieval阶段为后续的检索和生成阶段提供了高质量的数据支持。

#### Query Routing（查询路由）

`Query Routing`试图在数据库查询执行之前进行路由决策，以决定查询应该发送到哪个数据存储区或服务器，主要涉及到如何根据一定的规则或策略，将查询请求路由到正确的节点或分片上，以提高查询效率和系统性能。

查询路由是一个基于LLM的决策制定步骤，针对用户的查询决定接下来要做什么。通常的选项包括进行总结、对某些数据索引执行搜索，或尝试多种不同的路由然后将它们的输出合成一个答案。

#### Query Rewriting（查询重写）

`Query Rewriting`是一种通过修改或重新表述用户原始查询来提高检索效果的技术。其核心思想在于，原始查询可能由于表达不清、关键词选择不当或语法错误等原因，导致检索系统难以准确理解用户的真实意图。通过重写查询，可以生成更有利于检索的新查询，使检索系统能够更准确地捕捉用户的检索需求。

#### Query Expansion（查询扩展）

`Query Expansion`是指在信息检索过程中，对用户的原始查询进行改写或增强，通过添加额外的词汇、短语或概念，以覆盖更多与原始查询相关的内容。其目的是提高检索系统的性能，包括提高召回率（即检索出更多与用户查询相关的文档）和精确度（即减少与用户查询不相关的文档）。

### 2.2 Post-Retrieval(检索后)

Post-Retrieval阶段主要方法包括重新排序块和上下文压缩。对检索到的信息进行重新排序是一项关键策略,这一概念已经在`LlamaIndex`、`LangChain`和`HayStack`等框架中实现。

将所有相关文档直接输入LLM可能会导致信息过载，导致无关内容淡化对关键细节的关注。为了减轻这种情况，Post-Retrieval的核心是筛选基本信息、强调关键部分和缩短要处理的上下文。

#### Reranking（重排序）

`Reranking`重排序技术是对检索到的文档列表进行重新排序，使更相关、更准确的文档排名更靠前。通常使用模型来计算查询与文档之间的相关性得分，并根据得分进行排序。

#### Summary（摘要）

`Summary`摘要技术用于生成检索结果的简短摘要，帮助用户快速了解文档的主要内容。包括抽取式摘要和生成式摘要两种方法。抽取式摘要从文档中直接抽取关键句子或短语，而生成式摘要则使用自然语言生成技术来生成新的摘要文本。

#### Fusion(融合)

`Fusion`RAG Fusion从用户输入的原始查询中生成多个相似但不同的查询变体。这些查询变体能够覆盖用户查询的多个方面，从而提高检索结果的全面性和相关性。

## 三、代码实战

### 3.1 Pre-Retrieval（预检索）

在Pre-Retrieval（预检索）阶段的目标是使用户的原始问题更清晰，更适合于检索任务。常见的方法包括：查询重写、查询转换，查询扩展。

本次实战选择了主流的`Langchain`这一大语言模型编程框架，选择的样本还是`证券法(2019修订).pdf`

里面涉及的代码需要前面的文章作为基础，如果有需要可以查看本专栏其它文章。

查询重写及查询扩展代码实现：

```python
from langchain.document_loaders import PyPDFLoader

from langchain.text_splitter import RecursiveCharacterTextSplitter

from langchain_huggingface import HuggingFaceEmbeddings

from langchain.vectorstores import Chroma

from langchain_openai import ChatOpenAI

from langchain import PromptTemplate

from langchain_core.output_parsers import StrOutputParser

# 加载pdf文档

def load_data(data_file):
    loader = PyPDFLoader(data_file)
    documents = loader.load_and_split()
    text_splitter = RecursiveCharacterTextSplitter(separators=["。"], chunk_size=512, chunk_overlap=32)
    texts_chunks = text_splitter.split_documents(documents)
    return texts_chunks

# 原始知识文档路径

data_file = "E:\\SoureCode\\AI\\ai-study\\pdf\\证券法(2019修订).pdf"

# 第一步: 将文档语料库分割成较短的块并通过编码器构建向量索引

docs = load_data(data_file)

# embedding模型路径

embed_path = "E:\\SoureCode\\AI\\ai-study\\model\\bge-large-zh-v1.5"

embeddings = HuggingFaceEmbeddings(
    model_name=embed_path,
    model_kwargs={"device": "cuda"},
    encode_kwargs={"normalize_embeddings": True},
)

vectordb = Chroma.from_documents(docs, embeddings)

api_key = "api_key "

model = "qwen2-72b-instruct"

llm = ChatOpenAI(
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
    api_key=api_key,
    model=model,
    streaming=True,
)

# 构建生成查询重写及查询扩展的提示词

template = """You are a helpful assistant that generates multiple search queries based on a single input query. \n

Generate multiple search queries related to: {question} \n

Output (3 queries):"""

prompt_rewriting = PromptTemplate.from_template(template)

generate_query_chain = (
    prompt_rewriting
    | llm
    | StrOutputParser()
    | (lambda x: x.split("\n"))
)

question = "证券交易的原则是?"

queries = generate_query_chain.invoke({"question": question})

for doc in queries:
    print(doc)

question = "证券交易的原则是?"

questions = generate_query_chain.invoke({"question": question})

for q in questions:
    print(q)
```

使用上述查询词在向量库中检索，返回候选`chunk`和相应的score：

```python
def retrieval(queries):
    all_results = {}
    for query in queries:
        if query:
            search_results = vectordb.similarity_search_with_score(query)
            results = []
            for res in search_results:
                content = res[0].page_content
                score = res[1]
                results.append((content, score))
            all_results[query] = results
    return all_results
```

其中`LangChain`里面可以使用`MultiQueryRetriever`对查询改写、查询扩展及语义检索的过程进行简化，代码如下：

```python
import logging

from langchain.retrievers.multi_query import MultiQueryRetriever

retriever_from_llm = MultiQueryRetriever.from_llm(
    retriever=vectordb.as_retriever(), llm=llm
)

# 为了方便看到处理流程，显示详细日志

logging.basicConfig()

logging.getLogger("langchain.retrievers.multi_query").setLevel(logging.INFO)

question = "证券交易的原则是?"

retrieval_documents = retriever_from_llm.invoke(question)

for doc in retrieval_documents:
    print(doc.page_content)
```

运行代码获取如下结果：

注意划线部分输出内容：

INFO:langchain.retrievers.multi_query:Generated queries: [

'1. 请问在进行证券交易时，需要遵循哪些基本原则？',

'2. 能否详细解释一下证券市场交易的基本规则和原则？',

'3. 在股票、债券等证券的交易过程中，有哪些核心原则需要投资者了解和遵守？' ]

原始问题为：“证券交易的原则是?“，通过这一阶段的处理，问题获得了其它三种表达方式并使用这三种表达方式进行了语义检索。

注意：`MultiQueryRetriever`方法检索出的内容与我们自己编写的方法检索出的内容的结构不一致，不能只有应用于本实战代码中，需要进行改写，本文不做介绍，感兴趣的小伙伴们可自行查找资料。

### 3.2 Post-Retrieval（检索后）

Post-Retrieval（检索后）的工作集中在选择关键信息、强调关键部分和缩短待处理的上下文。

首先介绍一下我们将用的的一个公式`RRF score`：

其中，`rank`是按照距离排序的文档在各自集合中的排名，`k`是常数平滑因子，一般取`k = 60`。`RRF`将不同检索器的结果综合评估得到每个`chunk`的统一得分。

对检索到内容利用`RRF score`计算公式计算每个候选`chunk`的融合得分。

```python
from langchain.load import dumps, loads

# 对检索到的内容进行排序及融合

def reciprocal_rank_fusion(retrieval_documents, k=60):
    document_ranks= []
    for query, doc_score_list in retrieval_documents.items():
        ranking_list = [doc for doc, _ in sorted(doc_score_list, key=lambda x: x[1], reverse=True)]
        document_ranks.append(ranking_list)

    fused_scores = {}
    for docs in document_ranks:
        for rank, doc in enumerate(docs):
            doc_str = dumps(doc)
            if doc_str not in fused_scores:
                fused_scores[doc_str] = 0
            fused_scores[doc_str] += 1 / (rank + k)

    reranked_results = [
        (loads(doc), score)
        for doc, score in sorted(fused_scores.items(), key=lambda x: x[1], reverse=True)
    ]

    return reranked_results
```

最后将原始问题和检索到的上下文一起输入到LLM中，生成最终答案，完整代码如下：

```python
from langchain.document_loaders import PyPDFLoader

from langchain.text_splitter import RecursiveCharacterTextSplitter
```



```python
from langchain_huggingface import HuggingFaceEmbeddings
from langchain.vectorstores import Chroma
from langchain_openai import ChatOpenAI
from langchain import PromptTemplate
from langchain_core.output_parsers import StrOutputParser
```

# 加载pdf文档

```python
def load_data(data_file):
    loader = PyPDFLoader(data_file)
    documents = loader.load_and_split()
    text_splitter = RecursiveCharacterTextSplitter(separators=["。"], chunk_size=512, chunk_overlap=32)
    texts_chunks = text_splitter.split_documents(documents)
    return texts_chunks
```

# 原始知识文档路径

```python
data_file = "E:\\SoureCode\\AI\\ai-study\\pdf\\证券法(2019修订).pdf"
```

# 第一步: 将文档语料库分割成较短的块并通过编码器构建向量索引

```python
docs = load_data(data_file)
```

# embedding模型路径

```python
embed_path = "E:\\SoureCode\\AI\\ai-study\\model\\bge-large-zh-v1.5"

embeddings = HuggingFaceEmbeddings(
    model_name=embed_path,
    model_kwargs={"device": "cuda"},
    encode_kwargs={"normalize_embeddings": True},
)

vectordb = Chroma.from_documents(docs, embeddings)
```

# 大模型配置

```python
api_key = "api_key"
model = "qwen2-72b-instruct"

llm = ChatOpenAI(
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
    api_key=api_key,
    model=model,
    streaming=True,
)
```

# 构建生成查询重写及查询扩展的提示词

```python
template = """You are a helpful assistant that generates multiple search queries based on a single input query. \n

Generate multiple search queries related to: {question} \n

Output (3 queries):"""

prompt_rewriting = PromptTemplate.from_template(template)

generate_query_chain = (
    prompt_rewriting
    | llm
    | StrOutputParser()
    | (lambda x: x.split("\n"))
)

question = "证券交易的原则是?"

questions = generate_query_chain.invoke({"question": question})

for q in questions:
    print(q)
```

# 使用多个查询语句进行语义检索

```python
def retrieval(queries):
    all_results = {}
    for query in queries:
        if query:
            search_results = vectordb.similarity_search_with_score(query)
            results = []
            for res in search_results:
                content = res[0].page_content
                score = res[1]
                results.append((content, score))
            all_results[query] = results
    return all_results

retrieval_documents = retrieval(questions)

print(retrieval_documents)
```

```python
from langchain.load import dumps, loads
```

# 对检索到的内容进行排序及融合

```python
def reciprocal_rank_fusion(retrieval_documents, k=60):
    document_ranks = []
    for query, doc_score_list in retrieval_documents.items():
        ranking_list = [doc for doc, _ in sorted(doc_score_list, key=lambda x: x[1], reverse=True)]
        document_ranks.append(ranking_list)

    fused_scores = {}
    for docs in document_ranks:
        for rank, doc in enumerate(docs):
            doc_str = dumps(doc)
            if doc_str not in fused_scores:
                fused_scores[doc_str] = 0
            fused_scores[doc_str] += 1 / (rank + k)

    reranked_results = [
        (loads(doc), score)
        for doc, score in sorted(fused_scores.items(), key=lambda x: x[1], reverse=True)
    ]

    return reranked_results

reranked_results = reciprocal_rank_fusion(retrieval_documents)

print(reranked_results)
```

# 构造提示模板

```python
from langchain.chains.llm import LLMChain

template = """你是一名智能助手，根据上下文回答用户的问题，不需要回答额外的信息或捏造事实。

已知内容：

{context}

问题：

{question}

"""

prompt = PromptTemplate(template=template, input_variables=["context", "question"])
```

# 验证效果

```python
chain = LLMChain(llm=llm, prompt=prompt)

result = chain.run(context=reranked_results, question=question)

print(result)
```

LangChain对文档的排序和压缩提供了实现方法，下面是LangChain官网提供的示例代码，感兴趣的可以去官方文档进行研究和探索（LangChain官网相关链接地址在下方的引用章节）：

```python
# Create the retriever
compressor = FlashrankRerank()
compression_retriever = ContextualCompressionRetriever(
    base_compressor=compressor, base_retriever=retriever
)

# Create the chain
chain = RetrievalQA.from_chain_type(llm=llm, retriever=compression_retriever)

# Create the uptrain callback
uptrain_callback = UpTrainCallbackHandler(key_type=KEY_TYPE, api_key=API_KEY)
config = {"callbacks": [uptrain_callback]}

# Invoke the chain with a query
query = "What did the president say about Ketanji Brown Jackson"
result = chain.invoke(query, config=config)
```

# 引用

1. RAG 技术真的“烂大街”了吗？
2. Retrieval-Augmented Generation for Large Language Models: A Survey
3. 深度解读RAG技术发展历程：从基础Naive RAG 到高级Advanced，再到模块化Modular RAG的全面升级
4. LangChain官网-Multi Query Generation
5. 从0开始学RAG之RAG-Fusion
6. RAG-Fusion: a New Take on Retrieval-Augmented Generation
7. Implementing Reciprocal Rank Fusion (RRF) in Python

---

版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

原文链接：`https://blog.csdn.net/fengqiuhuang123/article/details/142207885`
