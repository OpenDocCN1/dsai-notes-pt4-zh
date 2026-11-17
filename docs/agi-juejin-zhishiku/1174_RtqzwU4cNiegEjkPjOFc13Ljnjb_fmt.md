
# 属性图索引：使用LLM构建知识图谱的强大新方法

> 来源：[https://agijuejin.feishu.cn/docx/Duawdf83uouPgAx8uRYcNLY5nzc](https://agijuejin.feishu.cn/docx/Duawdf83uouPgAx8uRYcNLY5nzc)

> 作者：lucas大叔

> 原文：[https://zhuanlan.zhihu.com/p/700879791](https://zhuanlan.zhihu.com/p/700879791)

图数据结构模型主要有两种：RDF图和属性图。RDF是由W3C组织提出的一种描述资源概念模型的语言，使用三元组集合的方式描述事物和关系，在表达能力上存在局限性，缺乏以下功能：

+ 为节点和关系分配标签和属性
+ 将文本节点表示为向量嵌入
+ 进行向量和符号检索

`LlamaIndex` 现有的 `KnowledgeGraphIndex` 也受到这些限制的影响，以及索引本身架构的一般限制。

属性图以其灵活的表达能力，在工业界大受欢迎。属性图索引（`Property Graph Index`）使用标记的属性图表示，解决了上述问题。它使得知识图谱的建模、存储和查询更加丰富。

使用属性图可以：

+ 将节点和关系分类为具有关联元数据的类型
+ 将图谱视为向量数据库的超集，用于混合搜索
+ 使用 `Cypher` 图查询语言表示复杂的查询

这使得属性图成为使用LLM构建知识图谱的强大而灵活的选择。

## 构建图谱

属性图索引提供了几种从数据中提取知识图谱的方法，你可以根据需要组合使用：

### 1. schema 引导的抽取（Schema-Guided Extraction）

在 `schema` 中定义允许的实体类型、关系类型以及它们之间的连接。`LLM` 将只抽取符合此 `schema` 的图数据。

```
from llama_index.indices.property_graph import SchemaLLMPathExtractor

entities = Literal["PERSON", "PLACE", "THING"]
relations = Literal["PART_OF", "HAS", "IS_A"]
schema = {
    "PERSON": ["PART_OF", "HAS", "IS_A"],
    "PLACE": ["PART_OF", "HAS"], 
    "THING": ["IS_A"],
}

kg_extractor = SchemaLLMPathExtractor(
  llm=llm, 
  possible_entities=entities, 
  possible_relations=relations, 
  kg_validation_schema=schema,
  strict=True,  # if false, allows values outside of spec
)
```

### 2. 隐式抽取（Implicit Extraction）

用 `LlamaIndex` 构建指定数据中节点之间的关系，基于 `node.relationships` 属性构建图谱。例如，当通过节点解析器处理文档时，将抽取 `PREVIOUS`、`NEXT` 和 `SOURCE` 关系。

```
from llama_index.core.indices.property_graph import ImplicitPathExtractor

kg_extractor = ImplicitPathExtractor()
```

### 3. 自由抽取（Free-Form Extraction）

让 `LLM` 以自由形式直接从数据中推断实体、关系类型和 `schema`。（这类似于当前 `KnowledgeGraphIndex` 的工作方式。）

```
from llama_index.core.indices.property_graph import SimpleLLMPathExtractor

kg_extractor = SimpleLLMPathExtractor(llm=llm)
```

混合和匹配这些抽取方法可以实现对图结构的细粒度控制。

## Embedding

默认情况下，所有图节点都被嵌入。虽然一些图数据库原生支持嵌入，但你也可以在图数据库之上指定并使用来自 `LlamaIndex` 的任何向量存储。

```
index = PropertyGraphIndex(..., vector_store=vector_store)
```

## 图谱查询

属性图索引支持多种查询技术，可以组合并同时使用。

### 1. 基于关键词/同义词的检索（Keyword/Synonym-Based Retrieval）

将查询扩展为相关的关键词和同义词，并找到匹配的节点。

```
from llama_index.core.indices.property_graph import LLMSynonymRetriever

sub_retriever = LLMSynonymRetriever(index.property_graph_store, llm=llm)
```

### 2. 向量相似性（Vector Similarity）

根据节点的向量表示与查询的相似性检索节点。

```
from llama_index.core.indices.property_graph import VectorContextRetriever

sub_retriever = VectorContextRetriever(
  index.property_graph_store, 
  vector_store=index.vector_store,
  embed_model=embed_model,
)
```

### 3. Cypher 查询（Cypher Queries）

使用表达丰富的 `Cypher` 图查询语言指定复杂的图模式，并遍历多个关系。

```
from llama_index.core.indices.property_graph import CypherTemplateRetriever
from llama_index.core.bridge.pydantic import BaseModel, Field

class Params(BaseModel):
    """Parameters for a cypher query."""
    names: list[str] = Field(description="A list of possible entity names or keywords related to the query.")
 
cypher_query = """
   MATCH (c:Chunk)-[:MENTIONS]->(o) 
   WHERE o.name IN $names
   RETURN c.text, o.name, o.label;
"""
   
sub_retriever = CypherTemplateRetriever(
 index.property_graph_store, 
 Params, 
 cypher_query,
 llm=llm,
)
```

也可以让 `LLM` 基于查询和数据库的上下文写出完整的 `Cypher` 查询，而不是提供一个模板。

```
from llama_index.core.indices.property_graph import TextToCypherRetriever

sub_retriever = TextToCypherRetriever(index.property_graph_store, llm=llm)
```

### 4. 自定义图遍历（Custom Graph Traversal）

通过子类化关键词检索器组件自定义图遍历逻辑。

这些检索器可以组合用于混合搜索，同时利用图结构和节点的向量表示。

```
from llama_index.indices.property_graph import VectorContextRetriever, LLMSynonymRetriever

vector_retriever = VectorContextRetriever(index.property_graph_store, embed_model=embed_model)  
synonym_retriever = LLMSynonymRetriever(index.property_graph_store, llm=llm)

retriever = index.as_retriever(sub_retrievers=[vector_retriever, synonym_retriever])
```

## 使用属性图存储

属性图索引使用 `PropertyGraphStore` 抽象来存储和检索图数据。你也可以直接使用该存储进行更低级别的控制。

属性图存储支持：

+ 插入和更新节点、关系和属性
+ 通过 ID 或属性查询节点
+ 从起始节点检索关系路径
+ 执行 `Cypher` 查询（如果后端存储支持）

```
from llama_index.graph_stores.neo4j import Neo4jPGStore

graph_store = Neo4jPGStore(
    username="neo4j",
    password="password",
    url="bolt://localhost:7687",
)

# insert nodes
nodes = [
    EntityNode(name="llama", label="ANIMAL", properties={"key": "value"}),
    EntityNode(name="index", label="THING", properties={"key": "value"}), 
]
graph_store.upsert_nodes(nodes)

# insert relationships  
relations = [
    Relation(
        label="HAS",
        source_id=nodes[0].id, 
        target_id=nodes[1].id,
    )
]
graph_store.upsert_relations(relations)

# query nodes
llama_node = graph_store.get(properties={"name": "llama"})[0]

# get relationship paths  
paths = graph_store.get_rel_map([llama_node], depth=1)

# run Cypher query
results = graph_store.structured_query("MATCH (n) RETURN n LIMIT 10")  
```

支持多种后端存储，包括内存、基于磁盘和 `Neo4j`。

## 代码实践

实践环节将使用 `Neo4j`、`Ollama` 和 `Huggingface` 构建属性图。具体构建使用 `SchemaLLMPathExtractor`，它允许指定精确的 `schema`，包含可能的实体类型、关系类型，以及定义它们如何相互连接。当要构建特定的图谱，并希望限制 `LLM` 的预测时，这种方案是非常有用的。

### 安装 packages

```
%pip install llama-index
%pip install llama-index-llms-ollama
%pip install llama-index-embeddings-huggingface
%pip install llama-index-graph-stores-neo4j
```

### 下载数据

```
!mkdir -p 'data/paul_graham/'
!wget 'https://raw.githubusercontent.com/run-llama/llama_index/main/docs/docs/examples/data/paul_graham/paul_graham_essay.txt' -O 'data/paul_graham/paul_graham_essay.txt'
```

读取数据

```
from llama_index.core import SimpleDirectoryReader

documents = SimpleDirectoryReader("./data/paul_graham/").load_data()
```

### 图谱构建

利用 `SchemaLLMPathExtractor` 构建图谱，给定图谱 `schema`，就可以提取符合 `schema` 的实体和关系，而不是让 `LLM` 随机决定实体和关系。

```
import nest_asyncio

nest_asyncio.apply()

from typing import Literal
from llama_index.llms.ollama import Ollama
from llama_index.core.indices.property_graph import SchemaLLMPathExtractor

# best practice to use upper-case
entities = Literal["PERSON", "PLACE", "ORGANIZATION"]
relations = Literal["HAS", "PART_OF", "WORKED_ON", "WORKED_WITH", "WORKED_AT"]

# define which entities can have which relations
validation_schema = {
    "PERSON": ["HAS", "PART_OF", "WORKED_ON", "WORKED_WITH", "WORKED_AT"],
    "PLACE": ["HAS", "PART_OF", "WORKED_AT"],
    "ORGANIZATION": ["HAS", "PART_OF", "WORKED_WITH"],
}

kg_extractor = SchemaLLMPathExtractor(
    llm=Ollama(model="llama3", json_mode=True, request_timeout=3600),
    possible_entities=entities,
    possible_relations=relations,
    kg_validation_schema=validation_schema,
    # if false, allows for values outside of the schema
    # useful for using the schema as a suggestion
    strict=True,
)
```

要在本地启动 `Neo4j`，请首先确保已安装了 `docker`。然后，使用以下 `docker` 命令启动数据库：

```
docker run \
    -p 7474:7474 -p 7687:7687 \
    -v $PWD/data:/data -v $PWD/plugins:/plugins \
    --name neo4j-apoc \
    -e NEO4J_apoc_export_file_enabled=true \
    -e NEO4J_apoc_import_file_enabled=true \
    -e NEO4J_apoc_import_file_use__neo4j__config=true \
    -e NEO4JLABS_PLUGINS=["apoc"] \
    neo4j:latest
```

在 `http://localhost:7474/` 上打开数据库。在该页面上，输入默认的用户名/密码 （`neo4j` 和 `neo4j`） 登录。第一次登录时，将被要求更改密码。完成后，就可以创建你的第一个属性图了！

```
from llama_index.graph_stores.neo4j import Neo4jPGStore

graph_store = Neo4jPGStore(
    username="neo4j",
    password="<password>",
    url="bolt://localhost:7687",
)
```

> 注意：与基于 API 的模型相比，使用本地模型进行抽取会更慢。本地模型（如 `Ollama`）通常限制于顺序处理。在 `M2 Max` 上，预计需要大约 10 分钟。



# LlamaIndex 属性图索引（Property Graph Index）

```python
from llama_index.core import PropertyGraphIndex
from llama_index.embeddings.huggingface import HuggingFaceEmbedding

index = PropertyGraphIndex.from_documents(
    documents,
    kg_extractors=[kg_extractor],
    embed_model=HuggingFaceEmbedding(model_name="BAAI/bge-small-en-v1.5"),
    property_graph_store=graph_store,
)
```

检查所创建的图，我们会发现它仅包括已定义的关系和实体类型！

![](img/5216742f21591b70ac7ef3550a25e509.png)

[有关 `kg_extractors` 的信息](https://link.zhihu.com/?target=https%3A//github.com/run-llama/llama_index/blob/c41821229982b1cfafb50f82548eeb37a25fdca8/docs/docs/module_guides/indexing/lpg_index_guide.md%23construction)，请参阅文档。

## 查询

现在已经创建了图谱，可以对其进行查询。与本示例的主题一致，我们将使用低层的 API 构建自定义检索器！

```python
from llama_index.core.indices.property_graph import (
    LLMSynonymRetriever,
    VectorContextRetriever,
)

llm_synonym = LLMSynonymRetriever(
    index.property_graph_store,
    llm=Ollama(model="llama3", request_timeout=3600),
    include_text=False,
)
vector_context = VectorContextRetriever(
    index.property_graph_store,
    embed_model=HuggingFaceEmbedding(model_name="BAAI/bge-small-en-v1.5"),
    include_text=False,
)
```

### 构建检索器

```python
retriever = index.as_retriever(
    sub_retrievers=[
        llm_synonym,
        vector_context,
    ]
)
```

### 图谱查询

```python
nodes = retriever.retrieve("What happened at Interleaf?")

for node in nodes:
    print(node.text)
```

> Paul Graham -> WORKED_AT -> Interleaf
> Paul Graham -> WORKED_AT -> Yahoo
> Paul Graham -> WORKED_AT -> Cambridge
> Tom Cheatham -> WORKED_AT -> Cambridge
> Kevin Hale -> WORKED_AT -> Viaweb
> Paul Graham -> WORKED_AT -> Viaweb
> Paul Graham -> WORKED_ON -> Viaweb
> Paul Graham -> PART_OF -> Viaweb

我们还可以创建具有类似语法的查询引擎。

```python
query_engine = index.as_query_engine(
    sub_retrievers=[
        llm_synonym,
        vector_context,
    ],
    llm=Ollama(model="llama3", request_timeout=3600),
)

response = query_engine.query("What happened at Interleaf?")

print(str(response))
```

> Paul Graham worked at Interleaf.

## 参考文档

[Introducing the Property Graph Index: A Powerful New Way to Build Knowledge Graphs with LLMs — LlamaIndex, Data Framework for LLM Applications](https://link.zhihu.com/?target=https%3A//www.llamaindex.ai/blog/introducing-the-property-graph-index-a-powerful-new-way-to-build-knowledge-graphs-with-llms)

[https://github.com/run-llama/llama_index/blob/main/docs/docs/examples/property_graph/property_graph_advanced.ipynb](https://link.zhihu.com/?target=https%3A//github.com/run-llama/llama_index/blob/main/docs/docs/examples/property_graph/property_graph_advanced.ipynb)

Property Graph Index - LlamaIndex

Using a Property Graph Store

Property Graph Index

Neo4j Property Graph Index
