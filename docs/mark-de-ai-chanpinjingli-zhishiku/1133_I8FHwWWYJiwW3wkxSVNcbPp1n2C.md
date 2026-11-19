
# 📠 带数据库路由的RAG

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/KvCNdu53RorTuwx1EwAcKT37n0c](https://qqs7y1hozd1.feishu.cn/docx/KvCNdu53RorTuwx1EwAcKT37n0c)

# 📠 带数据库路由的RAG代理

> 一个`Streamlit`应用程序，展示了带有智能查询路由的RAG代理的高级实现。

> 该系统将多个专业数据库与智能回退机制相结合，以确保对用户查询提供可靠、准确的响应。

[Code](https://qqs7y1hozd1.feishu.cn/wiki/Ox7iwxH8uinwH2khHAScD6eun6d)

[requirements.txt](https://qqs7y1hozd1.feishu.cn/wiki/NucpwScBpiwiVFkYlhYc3WBynQC)

![](img/1191c2fa1c45e77f1b68a21891006b28.png)

[Code (6).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/P3xebgav0oDTORxLWeTcYeIjnIh)

![](img/e6d43ae2a4649d0a98de0e080ba501d9.png)

[requirements.txt (6).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/N7bnbdjIpoUpZaxuwPpcVuqJn8e)

## 功能

+ 文件上传：

> 用户可以上传与特定公司相关的多个PDF文件。

+ 自然语言查询：

> 系统使用`agno`代理作为路由器，自动将查询路由到最相关的数据库。

+ 检索增强生成编排：

> 利用`Langchain`编排检索增强生成过程，检索最相关的信息并呈现给用户。

+ 备用机制：

> 使用配备`DuckDuckGo`搜索工具的`LangGraph`代理进行网络研究并提供答案。

## 如何运行？

### 安装依赖项

`pip install -r requirements.txt`

### 运行应用程序

`streamlit run rag_database_routing.py`

获取`OpenAI` API密钥：获取`OpenAI` API密钥并在应用程序中设置。这是初始化应用程序中使用的语言模型所必需的。

### 设置Qdrant云

+ 访问[Qdrant Cloud](https://cloud.qdrant.io/)

+ 创建账户或登录

+ 创建新集群

+ 获取您的凭证：

+ `Qdrant` API密钥：在API密钥部分中找到

+ `Qdrant` URL：您的集群 URL（格式：`https://xxx-xxx.aws.cloud.qdrant.io`）

### 上传文档

使用文档上传部分将PDF文档添加到所需的数据库中。

### 提问

在查询区域输入您的问题。应用程序将把您的问题路由到适当的数据库并提供答案。

## 使用的技术

+ `Langchain`：用于RAG编排，确保高效检索和生成信息。

+ `Agno`代理：用作路由代理，以确定与给定查询最相关的数据库。

+ `LangGraph`代理：充当备用机制，必要时利用`DuckDuckGo`进行网络研究。

+ `Streamlit`：提供用户友好的界面，用于文档上传和查询。

+ `Qdrant`：用于管理数据库，高效存储和检索文档嵌入。

## 工作原理是什么？

1. 查询路由系统采用三阶段路由方法：

+ 跨所有数据库的向量相似度搜索

+ 基于大语言模型的模糊查询路由

+ 针对未知主题的网络搜索回退

2. 文档处理

+ 从PDF中自动提取文本

+ 带重叠的智能文本分块

+ 向量嵌入生成

+ 高效的数据库存储

3. 答案生成

+ 上下文感知检索

+ 智能文档合并

+ 基于置信度的回复

+ 网络研究集成

# 📠 RAG Agent with Database Routing

A `Streamlit` application that demonstrates an advanced implementation of RAG Agent with intelligent query routing. The system combines multiple specialized databases with smart fallback mechanisms to ensure reliable and accurate responses to user queries.

## Features

+ Document Upload: Users can upload multiple PDF documents related to a particular company. These documents are processed and stored in one of the three databases: Product Information, Customer Support & FAQ, or Financial Information.

+ Natural Language Querying: Users can ask questions in natural language. The system automatically routes the query to the most relevant database using a `agno` agent as the router.

+ RAG Orchestration: Utilizes `Langchain` for orchestrating the retrieval augmented generation process, ensuring that the most relevant information is retrieved and presented to the user.

+ Fallback Mechanism: If no relevant documents are found in the databases, a `LangGraph` agent with a `DuckDuckGo` search tool is used to perform web research and provide an answer.

## How to Run?

### Clone the Repository

```
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd rag_tutorials/rag_database_routing
```

### Install Dependencies

`pip install -r requirements.txt`

### Run the Application

`streamlit run rag_database_routing.py`

### Get OpenAI API Key

Obtain an `OpenAI` API key and set it in the application. This is required for initializing the language models used in the application.

### Setup Qdrant Cloud

+ Visit [Qdrant Cloud](https://cloud.qdrant.io/)

+ Create an account or sign in

+ Create a new cluster

+ Get your credentials:

+ `Qdrant` API Key: Found in API Keys section

+ `Qdrant` URL: Your cluster URL (format: `https://xxx-xxx.aws.cloud.qdrant.io`)

### Upload Documents

Use the document upload section to add PDF documents to the desired database.

### Ask Questions

Enter your questions in the query section. The application will route your question to the appropriate database and provide an answer.

## Technologies Used

+ `Langchain`: For RAG orchestration, ensuring efficient retrieval and generation of information.

+ `Agno`: Used as the router agent to determine the most relevant database for a given query.

+ `LangGraph`: Acts as a fallback mechanism, utilizing `DuckDuckGo` for web research when necessary.

+ `Streamlit`: Provides a user-friendly interface for document upload and querying.

+ `Qdrant`: Used for managing the databases, storing and retrieving document embeddings efficiently.

## How It Works?

1. Query Routing The system uses a three-stage routing approach:

+ Vector similarity search across all databases

+ LLM-based routing for ambiguous queries

+ Web search fallback for unknown topics

2. Document Processing

+ Automatic text extraction from PDFs

+ Smart text chunking with overlap

+ Vector embedding generation

+ Efficient database storage

3. Answer Generation

+ Context-aware retrieval

+ Smart document combination

+ Confidence-based responses

+ Web research integration
