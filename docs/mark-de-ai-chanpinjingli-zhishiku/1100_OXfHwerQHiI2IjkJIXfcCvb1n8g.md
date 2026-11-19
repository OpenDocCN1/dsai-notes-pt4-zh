
# 🔄 Corrective RAG Agent

> 来源：`https://qqs7y1hozd1.feishu.cn/docx/NKeidrmNZolJc3xC8z6cUbM3nSd`

## 🔄 纠正性RAG代理

> 该系统使用`LangGraph`实现了一个纠错的多阶段工作流程。

> 一个复杂的检索增强生成（`RAG`）系统，该系统结合了文档检索、相关性分级、查询转换和网络搜索，以提供全面、准确的响应。

[`Code`](https://qqs7y1hozd1.feishu.cn/wiki/S0kowKqLViXfkRkzNCdcyJ2wnEg)

[`requirements.txt`](https://qqs7y1hozd1.feishu.cn/wiki/GBEywr3V9i8ZEMkcBrqcMldAnWg)

![](img/af362b6e76e92148aff960f4f3a226f5.png)

[`Code (2).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LxCxbXTr6ood1Axa5g7cZ2wsnZc)

![](img/3817a24583375bc3e7bd2386a8c08775.png)

[`requirements.txt (2).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/W3jqb1KCXoxrAaxnDmuct32Nn26)

## 功能

+   智能文档检索：使用`Qdrant`向量存储进行高效的文档检索

+   文档相关性分级：采用`Claude 3.5 十四行诗`评估文档相关性

+   查询转换：在需要时通过优化查询来改善搜索结果

+   网络搜索回退：当本地文档不足时，使用`Tavily API`进行网络搜索

+   多模型方法：结合`OpenAI`嵌入和`Claude 3.5 十四行诗`以处理不同任务

+   交互式 UI：使用`Streamlit`构建，便于文档上传和查询

## 如何运行？

### 克隆仓库

`git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git`

`cd rag_tutorials/corrective_rag`

### 安装依赖项

`pip install -r requirements.txt`

### 设置 API 密钥

1. 您需要获取以下 API 密钥：

+   [`OpenAI API 密钥`](https://platform.openai.com/api-keys)（用于嵌入）

+   [`Anthropic API 密钥`](https://console.anthropic.com/settings/keys)（用于`Claude 3.5 十四行诗`作为大语言模型）

+   [`塔维利 API 密钥`](https://app.tavily.com/home)（用于网络搜索）

+   `Qdrant`云设置

1. 访问[`Qdrant Cloud`](https://cloud.qdrant.io/)

1. 创建账户或登录

1. 创建新集群

1. 获取您的凭证：

+   `Qdrant API`密钥：在 API 密钥部分找到

+   `Qdrant URL`：您的集群 URL（格式：`https://xxx-xxx.aws.cloud.qdrant.io`）

### 运行应用程序

1. `streamlit run corrective_rag.py`

1. 使用应用程序：

+   上传文档或提供 URL

+   在查询框中输入您的问题

+   查看逐步纠正性 `RAG` 流程

+   获取全面的答案

## 技术栈

+   `LangChain`：用于 `RAG` 编排和链

+   `LangGraph`：用于工作流管理

+   `Qdrant`：用于文档存储的向量数据库

+   `Claude 3.5 十四行诗`：用于分析和生成的主要语言模型

+   `OpenAI`：用于文档嵌入

+   `Tavily`：用于网络搜索功能

+   `Streamlit`：用于用户交互界面

## 🔄 Corrective RAG Agent

A sophisticated Retrieval-Augmented Generation (`RAG`) system that implements a corrective multi-stage workflow using `LangGraph`. This system combines document retrieval, relevance grading, query transformation, and web search to provide comprehensive and accurate responses.

## Features

+   Smart Document Retrieval: Uses `Qdrant` vector store for efficient document retrieval

+   Document Relevance Grading: Employs `Claude 3.5 sonnet` to assess document relevance

+   Query Transformation: Improves search results by optimizing queries when needed

+   Web Search Fallback: Uses `Tavily API` for web search when local documents aren't sufficient

+   Multi-Model Approach: Combines `OpenAI` embeddings and `Claude 3.5 sonnet` for different tasks

+   Interactive UI: Built with `Streamlit` for easy document upload and querying

## How to Run?

### Clone the Repository

`git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git`

`cd rag_tutorials/corrective_rag`

### Install Dependencies

`pip install -r requirements.txt`

### Set Up API Keys

You'll need to obtain the following API keys:

+   [`OpenAI API key`](https://platform.openai.com/api-keys) (for embeddings)

+   [`Anthropic API key`](https://console.anthropic.com/settings/keys) (for `Claude 3.5 sonnet` as LLM)

+   [`Tavily API key`](https://app.tavily.com/home) (for web search)

+   `Qdrant` Cloud Setup

1. Visit [`Qdrant Cloud`](https://cloud.qdrant.io/)

1. Create an account or sign in

1. Create a new cluster

1. Get your credentials:

+   `Qdrant API Key`: Found in API Keys section

+   `Qdrant URL`: Your cluster URL (format: `https://xxx-xxx.aws.cloud.qdrant.io`)

### Run the Application

1. `streamlit run corrective_rag.py`

1. Use the Application:

+   Upload documents or provide URLs

+   Enter your questions in the query box

+   View the step-by-step Corrective `RAG` process

+   Get comprehensive answers

## Tech Stack

+   `LangChain`: For `RAG` orchestration and chains

+   `LangGraph`: For workflow management

+   `Qdrant`: Vector database for document storage

+   `Claude 3.5 sonnet`: Main language model for analysis and generation

+   `OpenAI`: For document embeddings

+   `Tavily`: For web search capabilities

+   `Streamlit`: For the user interface
