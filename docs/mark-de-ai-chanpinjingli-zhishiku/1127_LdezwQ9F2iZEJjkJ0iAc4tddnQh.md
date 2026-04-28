# 使用Cohere的RAG智能体⌘R

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/FmQJdjTW9otFDgxCbQhcL86Hnnf](https://qqs7y1hozd1.feishu.cn/docx/FmQJdjTW9otFDgxCbQhcL86Hnnf)

# RAG Agent with Cohere ⌘R

A RAG Agentic system built with Cohere's new model Command-r7b-12-2024, Qdrant for vector storage, Langchain for RAG and LangGraph for orchestration. This application allows users to upload documents, ask questions about them, and get AI-powered responses with fallback to web search when needed.

## Features

+   Document Processing

+   PDF document upload and processing

+   Automatic text chunking and embedding

+   Vector storage in Qdrant cloud

+   Intelligent Querying

+   RAG-based document retrieval

+   Similarity search with threshold filtering

+   Automatic fallback to web search when no relevant documents found

+   Source attribution for answers

+   Advanced Capabilities

+   DuckDuckGo web search integration

+   LangGraph agent for web research

+   Context-aware response generation

+   Long answer summarization

+   Model Specific Features

+   Command-r7b-12-2024 model for Chat and RAG

+   cohere embed-english-v3.0 model for embeddings

+   create_react_agent function from langgraph

+   DuckDuckGoSearchRun tool for web search

## Prerequisites

1.  Cohere API Key

1.  Go to [Cohere Platform](https://dashboard.cohere.ai/api-keys)

1.  Sign up or log in to your account

1.  Navigate to API Keys section

1.  Create a new API key

1.  Qdrant Cloud Setup

1.  Visit [Qdrant Cloud](https://cloud.qdrant.io/)

1.  Create an account or sign in

1.  Create a new cluster

1.  Get your credentials:

+   Qdrant API Key: Found in API Keys section

+   Qdrant URL: Your cluster URL (format: https://xxx-xxx.aws.cloud.qdrant.io)

## How to Run

1.  Clone the repository:

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd rag_tutorials/rag_agent_cohere

1.  Install dependencies:

pip install -r requirements.txt

streamlit run rag_agent_cohere.py

#

# 使用Cohere的RAG智能体⌘R

一个基于Cohere的新模型Command-r7b-12-2024构建的RAG智能体系统，使用Qdrant进行向量存储，使用Langchain进行RAG，使用LangGraph进行编排。该应用程序允许用户上传文档，就文档提问，并在需要时通过回退到网络搜索获得AI支持的响应。

## 功能

+   文档处理

+   PDF文档上传与处理

+   自动文本分块与嵌入

+   Qdrant云平台中的向量存储

+   智能查询

+   基于RAG的文档检索

+   带阈值过滤的相似度搜索

+   当未找到相关文档时自动回退到网络搜索

+   答案的来源归属

+   高级功能

+   集成 DuckDuckGo 网络搜索

+   用于网络研究的LangGraph代理

+   上下文感知的响应生成

+   长答案摘要

+   特定型号功能

+   用于聊天和检索增强生成（RAG）的Command-r7b-12-2024模型

+   用于嵌入的cohere embed-english-v3.0模型

+   来自langgraph的create_react_agent函数

+   用于网络搜索的DuckDuckGoSearchRun工具

## 先决条件

1.  Cohere API密钥

1.  前往[Cohere平台](https://dashboard.cohere.ai/api-keys)

1.  注册或登录您的账户

1.  导航到 API 密钥部分

1.  创建新的 API 密钥

1.  Qdrant云设置

1.  访问[Qdrant Cloud](https://cloud.qdrant.io/)

1.  创建账户或登录

1.  创建新集群

1.  获取您的凭证：

+   Qdrant API密钥：在API密钥部分中找到

+   Qdrant URL：您的集群 URL（格式：https://xxx-xxx.aws.cloud.qdrant.io）

## 如何运行

1.  克隆仓库：

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd rag_tutorials/rag_agent_cohere

1.  安装依赖项：

pip install -r requirements.txt

streamlit run rag_agent_cohere.py