
# 使用LangGraph的主动式RAG：AI博客搜索

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/QSXpdl9FQoGlL0xGKzOcnByGnDo](https://qqs7y1hozd1.feishu.cn/docx/QSXpdl9FQoGlL0xGKzOcnByGnDo)

> `AI博客搜索`是一款智能RAG应用程序，旨在提升从AI相关博客文章中检索信息的效率。

> 该系统利用`LangChain`、`LangGraph`和谷歌的`Gemini`模型来获取、处理和分析博客内容，为用户提供准确且与上下文相关的答案。

[`Code`](https://qqs7y1hozd1.feishu.cn/wiki/Q1EdwV4xbi7l4nkgpf7ck9e0nJb)

[`requirements.txt`](https://qqs7y1hozd1.feishu.cn/wiki/Rghgwp5fNiYbCektzMxcz8SPn8g)

![](img/d93cbfe2a402b13890eae0df4a7f5f91.png)

[`Code (2).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BfCnbveOCort9FxnWpscPAbDnTe)

![](img/1b017fad94736aa367037a6afd5e412b.png)

[`requirements.txt (2).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CxzcbTru1ouNgJxecE4cQWPMnab)

## 语言图工作流

![](img/36a6953e83175d2e27a534d17dc8e36e.png)

## 演示

![](img/a69e831459223003da64c7d71e1a4936.png)

[`AI-Blog-Search-Demo-Update.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Op9EbJ9pCoQHXwxPTWhcs8wznKh)

## 功能

+ **文档检索：**使用`Qdrant`作为向量数据库，基于嵌入来存储和检索博客内容。
+ **能动查询处理：**使用由AI驱动的智能体来确定查询是否应重写、回答或需要更多检索。
+ **相关性评估：**使用谷歌的`Gemini`模型实施自动相关性分级系统。
+ **查询优化：**优化结构不佳的查询，以获得更好的检索结果。
+ **Streamlit UI：**提供用户友好的界面，用于输入博客URL、查询并获取有见地的响应。
+ **基于图的工作流：**使用`LangGraph`实现结构化状态图，以进行高效决策。

## 使用的技术

+ 编程语言：[`Python 3.10+`](https://www.python.org/downloads/release/python-31011/)
+ 框架：[`LangChain`](https://www.langchain.com/)和[`LangGraph`](https://langchain-ai.github.io/langgraph/tutorials/introduction/)
+ 数据库：[`Qdrant`](https://qdrant.tech/)
+ **模型：**
  > 嵌入模型：谷歌`Gemini` API（`embedding-001`）
  > 聊天：谷歌双子星API（`gemini-2.0-flash`）
+ 博客加载器：`Langchain` `WebBaseLoader`
+ 文档拆分器：[递归字符文本拆分器](https://python.langchain.com/v0.1/docs/modules/data_connection/document_transformers/recursive_text_splitter/)
+ 用户界面 (UI)：[`Streamlit`](https://docs.streamlit.io/)

## 要求

1. 安装依赖项：`pip install -r requirements.txt`
2. 运行应用程序：`streamlit run app.py`
3. 使用应用程序：
   + 将您的Google API密钥粘贴到侧边栏中。
   + 粘贴博客链接。
   + 输入你对博客文章的查询内容。

## Agentic RAG with LangGraph: AI Blog Search

### Overview

`AI Blog Search` is an Agentic RAG application designed to enhance information retrieval from AI-related blog posts. This system leverages `LangChain`, `LangGraph`, and Google's `Gemini` model to fetch, process, and analyze blog content, providing users with accurate and contextually relevant answers.

### LangGraph Workflow

![](img/4f726dc5e70cc3a26d146e8a2030b22c.png)

### Demo

### Features

+ Document Retrieval: Uses `Qdrant` as a vector database to store and retrieve blog content based on embeddings.
+ Agentic Query Processing: Uses an AI-powered agent to determine whether a query should be rewritten, answered, or require more retrieval.
+ Relevance Assessment: Implements an automated relevance grading system using Google's `Gemini` model.
+ Query Refinement: Enhances poorly structured queries for better retrieval results.
+ Streamlit UI: Provides a user-friendly interface for entering blog URLs, queries and retrieving insightful responses.
+ Graph-Based Workflow: Implements a structured state graph using `LangGraph` for efficient decision-making.

### Technologies Used

+ Programming Language: [`Python 3.10+`](https://www.python.org/downloads/release/python-31011/)
+ Framework: [`LangChain`](https://www.langchain.com/) and [`LangGraph`](https://langchain-ai.github.io/langgraph/tutorials/introduction/)
+ Database: [`Qdrant`](https://qdrant.tech/)
+ Models:
  + Embeddings: Google `Gemini` API (`embedding-001`)
  + Chat: [`Google Gemini API`](https://ai.google.dev/gemini-api/docs/models/gemini#gemini-2.0-flash) (`gemini-2.0-flash`)
  + Blogs Loader: `Langchain` `WebBaseLoader`
  + Document Splitter: `RecursiveCharacterTextSplitter`
  + [`User Interface (UI)`](https://docs.streamlit.io/): `Streamlit`

### Requirements

1. Install Dependencies: `pip install -r requirements.txt`
2. Run the Application: `streamlit run app.py`
3. Use the Application:
   + Paste your Google API Key in the sidebar.
   + Paste the blog link.
   + Enter your query about the blog post.
