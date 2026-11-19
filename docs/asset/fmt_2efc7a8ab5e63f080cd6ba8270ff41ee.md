
# 构建检索增强生成 (RAG) 应用程序

> 来源：[https://v11enp9ok1h.feishu.cn/docx/I38Od6brqojrb4x2Hrbcc1TOnA1](https://v11enp9ok1h.feishu.cn/docx/I38Od6brqojrb4x2Hrbcc1TOnA1)

## 推荐 PC 端打开

手机端浏览体验没 PC 端体验效果好

### 点击蓝字立即跳转

[🧭课程学习](https://v11enp9ok1h.feishu.cn/wiki/M1MSwcFSwiYcR0kmnt0c4M3tngd#VVFPdC1Z2oOGG5x96Jic30psnje)  [知识库预览](https://v11enp9ok1h.feishu.cn/wiki/KiIvwdFOciiqqNkwKzTcmn88ndL#Uc4XdGXLdoCWMvxxAzZcweM5nhf)  [技术指导](https://v11enp9ok1h.feishu.cn/wiki/KiIvwdFOciiqqNkwKzTcmn88ndL#Uc4XdGXLdoCWMvxxAzZcweM5nhf)

> LLM 所实现的最强大的应用之一是复杂的问答 (Q&A) 聊天机器人。这些应用程序可以回答有关特定源信息的问题。这些应用程序使用一种称为检索增强生成 (RAG) 的技术。

> 本教程将介绍如何基于文本数据源构建一个简单的问答应用程序。在此过程中，我们将介绍典型的问答架构，并重点介绍更多高级问答技术的资源。还将了解 LangSmith 如何帮助我们跟踪和理解我们的应用程序。随着我们的应用程序变得越来越复杂，LangSmith 将变得越来越有用。

## 什么是 RAG

- RAG 是一种利用附加数据增强 LLM 知识的技术。

- LLM 可以推理广泛的主题，但他们的知识仅限于他们接受训练的特定时间点的公共数据。如果您想构建能够推理私有数据或模型截止日期后引入的数据的 AI 应用程序，则需要使用模型所需的特定信息来增强模型的知识。将适当的信息引入模型提示的过程称为检索增强生成 (RAG)。

- LangChain 有许多组件，旨在帮助构建问答应用程序以及更广泛的 RAG 应用程序。

注意：这里我们专注于非结构化数据的问答。如果您对 [结构化数据的 RAG](https://python.langchain.com/v0.2/docs/tutorials/sql_qa/) 感兴趣，请查看我们关于通过 SQL 数据进行问答的教程。

## 概念

### 典型的 RAG 应用程序有两个主要组件

- `索引`：从源中提取数据并对其进行索引的管道。这通常在线下进行。

- `检索和生成`：实际的 RAG 链，它在运行时接受用户查询并从索引中检索相关数据，然后将其传递给模型。

从原始数据到答案最常见的完整序列如下：

1. `加载`：首先我们需要加载数据。这是通过 [`DocumentLoaders`](https://python.langchain.com/v0.2/docs/concepts/#document-loaders) 完成的。

2. `拆分`：[`文本拆分器`](https://python.langchain.com/v0.2/docs/concepts/#text-splitters) 将大块内容拆分 `Documents` 成小块内容。这对于索引数据和将数据传递到模型都很有用，因为大块内容更难搜索，并且不适合模型的有限上下文窗口。

3. `存储`：我们需要一个地方来存储和索引我们的分割，以便以后可以搜索它们。这通常使用 [`VectorStore`](https://python.langchain.com/v0.2/docs/concepts/#vectorstores) 和 [`Embeddings`](https://python.langchain.com/v0.2/docs/concepts/#embedding-models) 模型来完成。

![](img/5e25ed4c7f190ed6f04b6b374db38d8c.png)

#### 检索和生成

1. `检索`：根据用户输入，使用 [`检索器`](https://python.langchain.com/v0.2/docs/concepts/#retrievers) 从存储中检索相关分割。

2. `生成`：[`ChatModel`](https://python.langchain.com/v0.2/docs/concepts/#chat-models) / [`LLM`](https://python.langchain.com/v0.2/docs/concepts/#llms) 使用包含问题和检索到的数据的提示生成答案。

![](img/bcc10eb259ff03d5cd3987ae489a7d95.png)

## 设置

### Jupyter Notebook

[本指南（以及文档中的大多数其他指南）](https://jupyter.org/) 使用 `Jupyter` 笔记本，并假设读者也使用 `Jupyter` 笔记本。`Jupyter` 笔记本非常适合学习如何使用 LLM 系统，因为经常会出现问题（意外输出、API 故障等），而在交互式环境中阅读指南是更好地理解它们的好方法。

[本教程和其他教程](https://jupyter.org/install) 可能最方便在 `Jupyter` 笔记本中运行。请参阅此处了解安装说明。

### [安装](https://python.langchain.com/v0.2/docs/tutorials/rag/#installation)

要安装 LangChain，请运行：

```bash
pip install langchain
```

#### LangSmith

使用 [LangChain](https://smith.langchain.com/) 构建的许多应用程序将包含多个步骤，并多次调用 LLM 调用。随着这些应用程序变得越来越复杂，能够检查链或代理内部究竟发生了什么变得至关重要。最好的方法是使用 LangSmith。

通过上面的链接注册后，请确保设置环境变量以开始记录跟踪：

```bash
export LANGCHAIN_TRACING_V2="true"
export LANGCHAIN_API_KEY="..."
```

或者，如果在笔记本中，您可以使用以下方式设置它们：

```python
import getpass
import os

os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_API_KEY"] = getpass.getpass()
```

## 预览

在本指南中，我们将在网站上构建一个 QA 应用程序。我们将使用的具体网站是 Lilian Weng 撰写的 LLM Powered Autonomous Agents 博客文章，该网站允许我们针对文章内容提出问题。

我们可以创建一个简单的索引管道和 RAG 链，用大约 20 行代码来完成此操作：

```bash
pip install -qU langchain-openai
```

```python
import getpass
import os

os.environ["OPENAI_API_KEY"] = getpass.getpass()

from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-3.5-turbo-0125")
```

```python
import bs4
from langchain import hub
from langchain_chroma import Chroma
from langchain_community.document_loaders import WebBaseLoader
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough
from langchain_openai import OpenAIEmbeddings
from langchain_text_splitters import RecursiveCharacterTextSplitter

# Load, chunk and index the contents of the blog.
loader = WebBaseLoader(
    web_paths=("https://lilianweng.github.io/posts/2023-06-23-agent/",),
    bs_kwargs=dict(
        parse_only=bs4.SoupStrainer(
            class_=("post-content", "post-title", "post-header")
        )
    ),
)
docs = loader.load()

text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
splits = text_splitter.split_documents(docs)
vectorstore = Chroma.from_documents(documents=splits, embedding=OpenAIEmbeddings())

# Retrieve and generate using the relevant snippets of the blog.
retriever = vectorstore.as_retriever()
prompt = hub.pull("rlm/rag-prompt")


def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)


rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)

rag_chain.invoke("What is Task Decomposition?")
```

API 参考：[`WebBaseLoader`](https://api.python.langchain.com/en/latest/document_loaders/langchain_community.document_loaders.web_base.WebBaseLoader.html) | [`StrOutputParser`](https://api.python.langchain.com/en/latest/output_parsers/langchain_core.output_parsers.string.StrOutputParser.html) | `RunnablePassthrough` | `OpenAIEmbeddings` | `RecursiveCharacterTextSplitter`

## 详细演练

让我们一步一步地浏览上面的代码来真正理解发生了什么。

### 1. 索引：加载

我们首先需要加载博客文章内容。我们可以使用 [`DocumentLoaders`](https://api.python.langchain.com/en/latest/documents/langchain_core.documents.base.Document.html) 来实现这一点，它们是从源加载数据并返回 `Documents` 列表的对象。`Document` 是一个包含一些 `str` 和 `dict` 的对象。`page_content` 和 `metadata`

在本例中，我们将使用 [`WebBaseLoader`](https://python.langchain.com/v0.2/docs/integrations/document_loaders/web_base/)，它用于 `urllib` 从 Web URL 加载 HTML 并将 `BeautifulSoup` 解析为文本。我们可以通过向解析器传递参数来自定义 HTML - 文本解析（请参阅 `BeautifulSoup` 文档）。在本例中，只有类为 `post-content`、`post-title` 或 `post-header` 的 HTML 标签是相关的，因此我们将删除所有其他标签。`bs_kwargs`

```python
import bs4
from langchain_community.document_loaders import WebBaseLoader

# Only keep post title, headers, and content from the full HTML.
bs4_strainer = bs4.SoupStrainer(class_=("post-title", "post-header", "post-content"))
loader = WebBaseLoader(
    web_paths=("https://lilianweng.github.io/posts/2023-06-23-agent/",),
    bs_kwargs={"parse_only": bs4_strainer},
)
```

> API 参考：[`WebBaseLoader`](https://api.python.langchain.com/en/latest/document_loaders/langchain_community.document_loaders.web_base.WebBaseLoader.html)

```text
43131
```

```python
print(docs[0].page_content[:500])
```

```text
      LLM Powered Autonomous Agents
    
Date: June 23, 2023  |  Estimated Reading Time: 31 min  |  Author: Lilian Weng


Building agents with LLM (large language model) as its core controller is a cool concept. Several proof-of-concepts demos, such as AutoGPT, GPT-Engineer and BabyAGI, serve as inspiring examples. The potentiality of LLM extends beyond generating well-written copies, stories, essays and programs; it can be framed as a powerful general problem solver.
Agent System Overview#
In
```

`DocumentLoader`：从源加载数据作为列表的对象 `Documents`。

- [文档](https://python.langchain.com/v0.2/docs/how_to/#document-loaders)：有关如何使用的详细文档 `DocumentLoaders`。

- [集成](https://python.langchain.com/v0.2/docs/integrations/document_loaders/)：160 多种集成可供选择。

- [接口](https://api.python.langchain.com/en/latest/document_loaders/langchain_core.document_loaders.base.BaseLoader.html)：基本接口的 API 参考。

### 索引：拆分

我们加载的文档长度超过 42k 个字符。这太长了，许多模型的上下文窗口都放不下。即使对于那些可以在上下文窗口中容纳完整帖子的模型，模型也很难在很长的输入中找到信息。

为了解决这个问题，我们将把 `Document` 嵌入和向量存储分成块。这应该可以帮助我们在运行时只检索博客文章中最相关的部分。

在本例中，我们将文档拆分为 1000 个字符的块，块之间有 200 个字符的重叠。重叠有助于降低将语句与与其相关的重要上下文分离的可能性。我们使用 `RecursiveCharacterTextSplitter`，它将使用常用分隔符（如换行符）递归拆分文档，直到每个块的大小合适。这是针对一般文本用例的推荐文本拆分器。

我们进行设置 `add_start_index=True`，以便每个分割文档在初始文档中开始的字符索引被保存为元数据属性 `start_index`。
