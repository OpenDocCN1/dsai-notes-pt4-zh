
# Adaptive-RAG:基于问题复杂度的自适应RAG

> 来源：https://agijuejin.feishu.cn/docx/Wnt7dXXeYoKc0Vx5mDIc59UNnzg
>
> 作者：lucas大叔
>
> 原文：https://zhuanlan.zhihu.com/p/691008957

[Retrieval-Augmented LLM](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2403.14403.pdf)可将外部数据库中的非参数知识融入LLM，从而有效提升诸如问答等任务的回答准确性。然而，尽管处理不同复杂度查询的方法很多，但也存在一定的不足亟待优化。例如，耗费太多的计算资源处理简单查询，不能妥善地解决复杂的多跳问题。而且，并非用户的所有查询都能划分为简单或者复杂。为此，研究人员提出了一种自适应QA框架- `adaptive RAG` ，根据查询复杂度从最简单到最复杂的LLM中动态选择最合适的策略。该框架用小LM训练了一个分类器，预测查询的复杂程度。这一方案在迭代和单步Retrieval-Augmented LLM以及无需检索方法之间实现了无缝自适应响应各种复杂度的查询。

![](img/fb4c1960f63f33992860bb9f8b77109c.png)

---

## 方案原理

Adaptive-RAG通过判断问题的复杂性自动选择使用哪种RAG策略。作者将问题划分为三类（如上图的C部分）：

+ 开放域问答：这类任务通常涉及两个模块：检索器和阅读器。随着具有千亿参数的超强推理能力的LLM的出现，LLM和检索器之间的协同作用已经取得了显著进展。然而，尽管在单跳检索增强LLM方面取得了进展，但某些查询的复杂性需要更复杂的策略。

+ 多跳问答：多跳问答（Multi-hop QA）是常规开放域问答（Open-domain QA）的扩展，需要系统全面收集和将多个文档的信息作为上下文回答更复杂的查询。首先将多跳查询分解为更简单的单跳查询，重复访问LLM和检索器来解决这些子查询，并合并它们的答案以形成完整答案。这种查询的缺点是：每个查询迭代访问LLM和检索器可能效率极低，因为有些查询可能足够简单，可以通过单一检索步骤甚至仅通过LLM本身来回答。

+ 自适应检索：为了处理不同复杂性的查询，自适应检索策略根据每个查询的复杂性动态决定是否检索文档。根据实体的频率来确定查询的复杂性级别，并建议仅当频率低于一定阈值时才使用检索模块。然而，这种方法仅关注于检索与否的二元决策，可能不足以解决需要多个推理步骤的更复杂的查询。

Adaptive Retrieval-Augmented Generation (Adaptive-RAG) 是一种新颖的问答框架，它能够根据问题的复杂性动态选择最适合的策略来处理Retrieval-Augmented LLM。这三种策略包括：

1. 非检索方法（No Retrieval）：这是最简单的策略，直接使用LLM本身的知识库来生成答案。这种方法适用于那些模型已经知道答案的简单问题，不需要额外的外部信息。

2. 单步检索方法（Single-step Approach）：当问题需要额外的信息时，这种方法会先从外部知识源检索相关信息，然后将检索到的文档作为上下文信息输入到LLM中，帮助模型生成更准确的答案。这种方法适用于需要一次额外信息检索的中等复杂度问题。

3. 多步检索方法（Multi-step Approach）：对于最复杂的问题，需要从多个文档中综合信息并进行多步推理。这种方法通过迭代地访问检索器和LLM，逐步构建起解决问题所需的信息链。这种方法适用于需要多步逻辑推理的复杂问题。

Adaptive-RAG的核心在于它能够通过分类器来评估问题的复杂性，然后根据评估结果选择最合适的处理策略。分类器是一个较小的语言模型，它被训练用来预测`query`的复杂度。通过这种方式，Adaptive-RAG能够灵活地在不同的Retrieval-Augmented LLM策略之间进行切换，从而在处理各种复杂性的问题时，实现更高的效率和准确性。

在Adaptive-RAG模型中，训练分类器以准确评估问题的复杂性是个关键步骤。这个过程涉及以下几个关键步骤：

1. 定义复杂性标签：首先，需要定义问题的复杂性等级。在Adaptive-RAG中，通常有三个类别：简单（`A`）、中等（`B`）和复杂（`C`）。简单问题可以直接由LLM回答，中等复杂度问题需要单步检索，而复杂问题则需要多步检索和推理。

2. 自动收集训练数据：由于没有现成的带有复杂性标签的查询数据集，Adaptive-RAG通过两种策略自动构建训练数据集：

   - 从不同Retrieval-Augmented LLM策略的预测结果中标注查询的复杂性。

     如果非检索方法能够正确生成答案，则对应问题的标签为简单（`A`）；

     如果单步检索方法和多步检索方法都能正确回答，而非检索方法失败，则对应问题的标签为中等（`B`）；

     如果只有多步检索方法能够正确回答，则对应问题的标签为复杂（`C`）。

   - 利用基准数据集中的固有偏差来标注未标记的查询。

     例如，如果一个查询在单步数据集中未被标记，则自动分配标签为中等（`B`）；

     如果在多步数据集中未被标记，则自动分配标签为复杂（`C`）。

3. 训练分类器：使用自动收集的查询-复杂性对数据集，训练一个较小的语言模型作为分类器。这个分类器的目标是根据 `query` 预测其复杂性级别。训练过程中，使用交叉熵损失函数，并选择在验证集上表现最佳的迭代次数。

4. 评估和优化分类器：在训练完成后，评估分类器的性能，包括准确率和其他相关指标。如果分类器的性能不足，可能需要进一步优化，例如通过调整模型结构、增加训练数据或改进数据标注策略。

5. 集成到Adaptive-RAG框架：将训练好的分类器集成到Adaptive-RAG框架中。在推理阶段，分类器用于预测新问题的复杂性，然后根据这个预测结果选择最合适的问答策略。

通过这种方法，Adaptive-RAG能够动态地调整其查询处理策略，以适应不同复杂性的问题，从而提高问答系统的整体效率和准确性。

---

## 基于`langchain`的代码实现

[`Adaptive RAG`](https://link.zhihu.com/?target=https%3A//blog.langchain.dev/query-construction/) 是将 `query analysis` 和 `active / self-corrective RAG` 相结合的一种RAG策略。

基于adaptive-RAG论文中的思想，执行`query analysis`以遍历一些更有趣的案例：

+ 无检索（LLM回答）

+ Web搜索

+ 迭代RAG

我们将使用`Cohere`最近发布的 `Command R`：

+ 在RAG和Tool Use方面有较高的准确性

+ 128k上下文

+ 低延迟

![](img/d7ed64931d03b01d64bbcf3f826ac3a0.png)

### 准备环境

```bash
pip install --quiet langchain langchain_cohere langchain-openai tiktoken langchainhub chromadb langgraph
```

### 配置cohere api key

```python
### LLMs
import os
os.environ['COHERE_API_KEY'] = <your-api-key>
```

### 创建索引

```python
### Build Index

from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.document_loaders import WebBaseLoader
from langchain_community.vectorstores import Chroma
from langchain_cohere import CohereEmbeddings

# Set embeddings
embd = CohereEmbeddings()

# Docs to index
urls = [
    "https://lilianweng.github.io/posts/2023-06-23-agent/",
    "https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/",
    "https://lilianweng.github.io/posts/2023-10-25-adv-attack-llm/",
]

# Load
docs = [WebBaseLoader(url).load() for url in urls]
docs_list = [item for sublist in docs for item in sublist]

# Split
text_splitter = RecursiveCharacterTextSplitter.from_tiktoken_encoder(
    chunk_size=512, chunk_overlap=0
)
doc_splits = text_splitter.split_documents(docs_list)

# Add to vectorstore
vectorstore = Chroma.from_documents(
    documents=doc_splits,
    embedding=embd,
)

retriever = vectorstore.as_retriever()
```

### LLM使用

我们使用`router`在工具之间进行选择。`Cohere`模型决定调用哪个工具，以及如何查询它们。

```python
### Router
from typing import Literal

from langchain_core.prompts import ChatPromptTemplate
from langchain_core.pydantic_v1 import BaseModel, Field
from langchain_cohere import ChatCohere

# Data model
class web_search(BaseModel):
    """
    The internet. Use web_search for questions that are related to anything else than agents, prompt engineering, and adversarial attacks.
    """
    query: str = Field(description="The query to use when searching the internet.")

class vectorstore(BaseModel):
    """
    A vectorstore containing documents related to agents, prompt engineering, and adversarial attacks. Use the vectorstore for questions on these topics.
    """
    query: str = Field(description="The query to use when searching the vectorstore.")

# Preamble
preamble = """You are an expert at routing a user question to a vectorstore or web search.
The vectorstore contains documents related to agents, prompt engineering, and adversarial attacks.
Use the vectorstore for questions on these topics. Otherwise, use web-search."""

# LLM with tool use and preamble
llm = ChatCohere(model="command-r", temperature=0)
structured_llm_router = llm.bind_tools(tools=[web_search, vectorstore], preamble=preamble)

# Prompt
route_prompt = ChatPromptTemplate.from_messages(
    [
        ("human", "{question}"),
    ]
)

question_router = route_prompt | structured_llm_router
response = question_router.invoke({"question": "Who will the Bears draft first in the NFL draft?"})
print(response.response_metadata['tool_calls'])
response = question_router.invoke({"question": "What are the types of agent memory?"})
print(response.response_metadata['tool_calls'])
response = question_router.invoke({"question": "Hi how are you?"})
print('tool_calls' in response.response_metadata)
```

打印内容

```text
[{'id': 'f811e3b9-052e-49db-a234-5fc3efbcc5ba', 'function': {'name': 'web_search', 'arguments': '{"query": "NFL draft bears first pick"}', 'type': 'function'}}]
[{'id': '4bc53113-8f32-4d6d-ac9b-c07ef9aae9fd', 'function': {'name': 'vectorstore', 'arguments': '{"query": "types of agent memory"}', 'type': 'function'}}]
False
```

### 检索内容评估

评估检索到的内容与用户问题是否相关
