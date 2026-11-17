
# 使用GraphRAG创建本地化聊天机器人

> 来源：[https://agijuejin.feishu.cn/docx/VYjddFDPUoAXLsxe7rucINsMnJg](https://agijuejin.feishu.cn/docx/VYjddFDPUoAXLsxe7rucINsMnJg)

> 作者：Ninja Geek | 智见AGI

> 原文链接： [https://mp.weixin.](https://mp.weixin.qq.com/s/xe0P1jmoFEvmq8ONOGAiag)qq.com/s/xe0P1jmo...

![](img/5392eb0dc10005eb7fa65370967f35a4.png)

未来 GraphRAG 有可能成为 RAG 领域的全球标准。

> ©作者 | Ninja Geek  
> 来源 | 神州问学

这是一篇教你如何开发基于 Graph RAG 的聊天机器人的快速指南，展示了如何通过 `Langchain`、`Graph RAG` 和 `GPT-4o-mini` 创建一个完全本地化运行的聊天机器人，为您的企业或个人使用而打造一个强大的聊天机器人。

GraphRAG 不太会像单纯的语言模型那样容易产生幻觉，因为知识图谱为大语言模型提供了更相关、更多样化、更具吸引力、更连贯、更可靠的数据，最终生成准确、真实的回答。

如果上面的内容听起来对你来说很复杂或困难，那么不用担心，因为它比你想象的要容易得多。

在这篇文章中，我将带你仔细研究 GraphRAG，了解图检索增强生成过程的工作原理，以及 GraphRAG 为何比向量数据库更好。为了让你更全面的了解文章内容，我们将从学习 GraphRAG 的核心概念开始。

## 什么是 GraphRAG？

简单地说，这是一种以图表为表现形式，用来表示信息相关性的技术，可以执行信息搜索并根据图结构生成答案。

这使得如何“理解”抽象问题变得更容易，因为图结构管理了 RAG 无法读取的信息的相关性。

GraphRAG 就是利用图数据库技术来搜索和生成信息。图数据库使用节点（点）和边（线）来表示信息之间的关系，使得数据的处理比传统的关系数据库更灵活、直观。这使得有效管理信息之间的复杂关系并提高搜索结果的准确性成为了可能。

## GraphRAG 是如何工作的？

GraphRAG 充分利用了基于向量 RAG 的所有优势，但将数据存储在图数据库中。图数据库存储了相关内容的额外上下文信息，这些上下文信息存储在节点中，而“边”描述连接信息的关系和链接。

例如：让我们考虑一个关于火影忍者中小樱的问题。当尝试使用 RAG 处理抽象问题（如“小樱的特点是什么？”）时，即使有外部信息说“小樱是一位有着长发和淡绿色眼睛的美丽女孩”，但也有可能出现该问题在语义上不被认为和这条回答接近从而无法提供答案。

从人类的视角来看，“小樱的特点是什么？”和“长发女孩”在对话中是有意义的。然而，由于缺乏语义接近性，RAG 可能不会将其识别为合适的答案。

## 当你使用 GraphRAG 时会发生什么？

GraphRAG 根据“小樱是一位有着长发和淡绿色眼睛的美丽女孩”这句话创建了一个图结构。生成“小樱”和“女孩”这两个节点，并创建“边”来表示相关信息，例如“长发”和“淡绿色的眼睛”。

当查询包含词“小樱”时，GraphRAG 首先搜索“小樱”相关的节点。接下来，它会从图中检索与“小樱”节点相关的信息，从而了解信息的相关性。

因此，GraphRAG 可以通过理解词之间的关系来生成预期的答案，即使对于前面提到的抽象问题也是如此。

## GraphRAG 与向量数据库

GraphRAG 擅长对实体之间的复杂关系进行建模和查询。它们旨在高效处理互连数据并执行基于图的操作，例如遍历关系、查找路径和识别模式。

虽然向量数据库旨在有效地执行相似性搜索和最近邻查询，但它们利用专门的索引技术和算法来快速检索与给定查询向量最相似的向量。

## 使用 Python 构建 GraphRAG 系统

在 `Python` 中安装 `Langchain` 非常简单，你可以使用 `pip` 或 `conda` 进行安装。

就我个人的经验来说，我建议使用 `Langchain` 的最新版开始你的开发工作，因为 `Langchain` 的迭代速度非常快，因此，我使用如下的命令：

```
%pip install --upgrade --quiet  langchain langchain-openai tiktoken neo4j sqlalchemy wikipedia langchain-community langchain-core --user
```

在这里，我们需要设置 `Neo4j`，最简单的方法是在 `Neo4j Aura` 上启动一个免费实例，它提供 `Neo4j` 数据库的云实例。或者，你也可以通过下载 `Neo4j Desktop` 应用程序并创建本地数据库实例来设置 `Neo4j` 数据库。

```
from langchain_community.chat_models import ChatOpenAI

from langchain_community.graphs import Neo4jGraph

os.environ["OPENAI_API_KEY"] = 'Your_API';
openai.api_key = os.environ["OPENAI_API_KEY"]

model = ChatOpenAI()

url = "bolt://localhost:7687";
username ="neo4j";
password = "mypassword";
graph = Neo4jGraph(
    url=url,
    username=username,
    password=password
)
print(graph)
```

`OpenAI` 提供的函数非常适合从自然语言中提取结构化信息。`OpenAI` 函数背后的想法是让大语言模型输出带有填充值的预定义 `JSON` 对象。

接下来是定义数据结构来表示知识图谱，并为每个数据结构指定属性：`Property`、`Relationship` 和 `KnowledgeGraph` 。

```
from langchain_community.graphs.graph_document import (
    Node as BaseNode,
    Relationship as BaseRelationship,
    GraphDocument,
)
from langchain.schema import Document
from typing import List, Dict, Any, Optional
from langchain_core.pydantic_v1 import BaseModel, Field

class Property(BaseModel):
  """一个由键和值组成的单一属性"""
  key: str = Field(..., description="键")
  value: str = Field(..., description="值")

class Node(BaseNode):
    properties: Optional[List[Property]] = Field(
        None, description="列举出节点属性")

class Relationship(BaseRelationship):
    properties: Optional[List[Property]] = Field(
        None, description="列举出关系属性"
    )

class KnowledgeGraph(BaseModel):
    """生成一个包含实体和关系的知识图谱"""
    nodes: List[Node] = Field(
        ..., description="知识图谱中的节点列表")
    rels: List[Relationship] = Field(
        ..., description="知识图谱中的关系列表"
    )
```

我将属性值改写为 `Property` 类的列表，而不是字典结构，以克服 `API` 的限制。由于只能将单个对象传递给 `API`，因此可以将节点和关系组合到一个名为 `KnowledgeGraph` 的类中。

这里定义了几个函数用来操作知识图谱中的节点和关系：

● `format_property_key` ：格式化属性的键。

● `props_to_dict` ：将属性列表转换为字典。

● `map_to_base_node` ：将自定义节点映射到 `BaseNode`。

● `map_to_base_relationship` ：将自定义关系映射到 `BaseRelationship` 。

```
def format_property_key(s: str) -> str:
    words = s.split()
    if not words:
        return s
    first_word = words[0].lower()
    capitalized_words = [word.capitalize() for word in words[1:]]
    return "".join([first_word] + capitalized_words)

def props_to_dict(props) -> dict:
    """将属性转换为字典。"""
    properties = {}
    if not props:
      return properties
    for p in props:
        properties[format_property_key(p.key)] = p.value
    return properties

def map_to_base_node(node: Node) -> BaseNode:
    """将 `KnowledgeGraph` 节点映射到 `BaseNode`。"""
    properties = props_to_dict(node.properties) if node.properties else {}
    # Add name property for better Cypher statement generation
    properties["name"] = node.id.title()
    return BaseNode(
        id=node.id.title(), type=node.type.capitalize(), properties=properties
    )

def map_to_base_relationship(rel: Relationship) -> BaseRelationship:
    """将 `KnowledgeGraph` 关系映射到 `BaseRelationship`。"""
    source = map_to_base_node(rel.source)
    target = map_to_base_node(rel.target)
    properties = props_to_dict(rel.properties) if rel.properties else {}
    return BaseRelationship(
        source=source, target=target, type=rel.type, properties=properties
    )
```

接下来定义一个函数，用于 `get_extraction_chain` 创建从文本中提取知识图谱的链。它接受两个可选参数：`allowed_nodes` 和 `allowed_rels`。

这些参数分别用于指定允许的节点标签和关系类型，以使信息提取更加准确。这有助于消除“小樱”示例中解释的歧义。

虽然提示中尝试消除歧义，但最初不需要指定这些参数。但是，如果提取的结果中仍然存在歧义，最好指定 `allowed_nodes` 和 `allowed_rels`。最后，该函数使用 `create_structured_output_chain` 创建和使用指定提示和 `LLM` 的链。

```
from langchain.chains.openai_functions import (
  create_structured_output_chain,
)
from langchain_core.prompts import ChatPromptTemplate

llm = model

def get_extraction_chain(
    allowed_nodes: Optional[List[str]] = None,
    allowed_rels: Optional[List[str]] = None
):
    prompt = ChatPromptTemplate.from_messages(
        [
          ("system",
 f"""# GPT-4o-mini 的知识图谱使用说明
## 1. 概述
你是一位顶尖的算法专家，设计用于以结构化格式提取信息以构建知识图谱。
- 节点 表示实体和概念。它们类似于维基百科节点
- 目标是实现知识图谱的简洁性和清晰性，使其易于广泛受众理解。
## 2. 标记节点
- 一致性: 确保你使用基本或初级类型作为节点标签。
  - 例如，当你识别出一个表示人的实体时，始终将其标记为 "person（人）". 避免使用更具体的术语， 如"mathematician（数学家）" or "scientist（科学家）".
- 节点ID: 永远不要使用整数作为节点ID。节点ID应为文本中找到的名称或可读的标识符。
{'- 允许的节点标签:' + ", ".join(allowed_nodes) if allowed_nodes else ''}
{'- 允许的关系类型:' + ", ".join(allowed_rels) if allowed_rels else ''}
## 3. 处理数值数据和日期
- 像年龄或其他相关信息这样的数值数据，应作为相应节点的属性或属性值包含在内。
- 不为日期/数字创建单独的节点: 不要为日期或数值创建单独的节点。始终将它们作为节点的属性或属性值附加。
- 属性格式: 属性必须是键值对格式。
- 引号: 属性值内绝不要使用转义的单引号或双引号。
- 命名约定: 使用驼峰式命名法作为属性键，例如, `birthDate`.
## 4. 共指消解
- 保持实体一致性: 在提取实体时，确保一致性非常重要。如果一个实体（例如“张小明”）在文本中多次提及，但使用了不同的名字或代词（例如“小明”、“他”）,
在整个知识图谱中始终使用该实体最完整的标识符。在这个例子中，使用“张小明”作为实体 ID。
请记住，知识图谱应连贯且易于理解，因此保持实体引用的一致性至关重要。
## 5. 严格遵守
严格遵守规则。不遵守将导致终止。
          """),
            ("human", "请提供要提取信息的输入内容，这样我可以按照给定格式进行提取: {input}"),
            ("human", "提示: 请提供要提取信息的输入内容，这样我可以按照给定格式进行提取。"),
        ])
    return create_structured_output_chain(KnowledgeGraph, llm, prompt, verbose=False)
```

这里我添加了一个选项来限制应从文本中提取哪些节点或关系类型。此功能非常有用。

在 `Neo4j` 连接和 `LLM` 提示词准备就绪后，可以将信息提取管道定义为单个函数。此设置简化了流程，使我们更容易提取所需的信息，并提高了我们工作的整体效率。



```
def extract_and_store_graph(
    document: Document,
    nodes: Optional[List[str]] = None,
    rels: Optional[List[str]] = None
) -> None:
    if not isinstance(document, Document):
        raise TypeError(f"预期文档应为 Document 实例，但得到的是 {type(document)}")
    # 使用OpenAI函数提取图数据。
    extract_chain = get_extraction_chain(nodes, rels)
    # print(extract_chain)
    data = extract_chain.invoke(document.page_content)['function']
    # print(data)
    # 构建一个图文档
    graph_document = GraphDocument(
      nodes = [map_to_base_node(node) for node in data.nodes],
      relationships = [map_to_base_relationship(rel) for rel in data.rels],
      source = document
    )
    print(graph_document)
    # 将信息存储到图中
    graph.add_graph_documents([graph_document], True)
```

我们使用了较大的块大小值来包含每个句子周围的尽可能多的上下文。这有助于更好的进行共指解析。重要的是要记住，只有当实体及其引用出现在同一个块中时，共指步骤才会起作用。否则，LLM 就没有足够的信息来将两者联系起来。

```
from langchain_community.document_loaders import WebBaseLoader
from langchain_text_splitters import TokenTextSplitter
raw_documents = WebBaseLoader("https://blog.langchain.dev/what-is-an-agent/").load()
text_splitter = TokenTextSplitter(chunk_size=2048, chunk_overlap=24)

# 只取第一个 raw_documents
documents = text_splitter.split_documents(raw_documents)
```

对于文档列表中的每个文档，`extract_and_store_graph` 都会调用一个函数来提取知识图谱并将其保存到 Neo4j 数据库中。我们使用 `tqdm` 库来显示处理进度。整个过程只需不到几分钟：

```
from tqdm import tqdm

# 遍历块并调用 extract_and_store_graph
for i, d in tqdm(enumerate(documents), total=len(documents)):
    print(f"正在处理的块 {i}: {d}")
    extract_and_store_graph(d)
    print("图保存成功。")
```

最后，我通过 `GraphCypherQAChain` 构建 Cypher 语句来浏览知识图谱中的信息，类似 SQL 用于关系型数据库的方式。

```
# 在 RAG 应用中查询知识图谱。
from langchain.chains import GraphCypherQAChain

graph.refresh_schema()
print(graph.schema)

cypher_chain = GraphCypherQAChain.from_llm(
    graph=graph,
    cypher_llm=model,
    qa_llm=model,
    validate_cypher=True, # 验证关系的方向。
    # return_intermediate_steps=True,
    verbose=True
)
```

# 结论

知识图谱非常有趣，但同时感觉还有很多技术探索要做。当然这些探索会非常有挑战，但我觉得，在未来，将知识图谱与 RAG 结合起来会很有前景。

未来 GraphRAG 有可能成为 RAG 领域的全球标准。

![](img/3c4f320a27e4013245c616353ccd7be0.png)

![](img/af2350fa661a58ea02b23401320053f4.png)

![](img/9b365052c628e00b2ffb8131f682068a.png)

![](img/9297c5e8b9de11b68b6eb4f7682d7aa6.png)
