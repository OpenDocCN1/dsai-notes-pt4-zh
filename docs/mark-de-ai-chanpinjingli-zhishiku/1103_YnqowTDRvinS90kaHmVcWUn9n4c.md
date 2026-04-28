# 🐋 Deepseek本地RAG推理代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/ZbUldEIy7oLOt2xDIcHcmVyFnqh](https://qqs7y1hozd1.feishu.cn/docx/ZbUldEIy7oLOt2xDIcHcmVyFnqh)

# 🐋 Deepseek Local RAG Reasoning Agent

A powerful reasoning agent that combines local Deepseek models with RAG capabilities. Built using Deepseek (via Ollama), Snowflake for embeddings, Qdrant for vector storage, and Agno for agent orchestration, this application offers both simple local chat and advanced RAG-enhanced interactions with comprehensive document processing and web search capabilities.

## Features

+   Dual Operation Modes

+   Local Chat Mode: Direct interaction with Deepseek locally

+   RAG Mode: Enhanced reasoning with document context and web search integration - llama3.2

+   Document Processing (RAG Mode)

+   PDF document upload and processing

+   Web page content extraction

+   Automatic text chunking and embedding

+   Vector storage in Qdrant cloud

+   Intelligent Querying (RAG Mode)

+   RAG-based document retrieval

+   Similarity search with threshold filtering

+   Automatic fallback to web search

+   Source attribution for answers

+   Advanced Capabilities

+   Exa AI web search integration

+   Custom domain filtering for web search

+   Context-aware response generation

+   Chat history management

+   Thinking process visualization

+   Model Specific Features

+   Flexible model selection:

+   Deepseek r1 1.5b (lighter, suitable for most laptops)

+   Deepseek r1 7b (more capable, requires better hardware)

+   Snowflake Arctic Embedding model (SOTA) for vector embeddings

+   Agno Agent framework for orchestration

+   Streamlit-based interactive interface

## Prerequisites

1.  Ollama Setup

1.  Install [Ol](https://ollama.ai/)[lama](https://ollama.ai/)

1.  Pull the Deepseek r1 model(s):

# For the lighter model

ollama pull deepseek-r1:1.5b

# For the more capable model (if your hardware supports it)

ollama pull deepseek-r1:7b

ollama pull snowflake-arctic-embed

ollama pull llama3.2

1.  Qdrant Cloud Setup (for RAG Mode)

1.  Visit [Qdrant Cloud](https://cloud.qdrant.io/)

1.  Create an account or sign in

1.  Create a new cluster

1.  Get your credentials:

+   Qdrant API Key: Found in API Keys section

+   Qdrant URL: Your cluster URL (format: https://xxx-xxx.cloud.qdrant.io)

1.  Exa AI API Key (Optional)

1.  Visit [Exa AI](https://exa.ai/)

1.  Sign up for an account

1.  Generate an API key for web search capabilities

## How to Run

1.  Clone the repository:

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd rag_tutorials/deepseek_local_rag_agent

1.  Install dependencies:

pip install -r requirements.txt

1.  Run the application:

streamlit run deepseek_rag_agent.py

#

# 🐋 Deepseek本地RAG推理代理

一个强大的推理代理，它将本地Deepseek模型与RAG能力相结合。该应用程序使用Deepseek（通过Ollama）构建，利用Snowflake进行嵌入，Qdrant进行向量存储，Agno进行代理编排，既提供简单的本地聊天，也提供高级的RAG增强交互，具备全面的文档处理和网络搜索能力。

## 功能

+   双运行模式

+   本地聊天模式：直接与本地的Deepseek进行交互

+   检索增强生成（RAG）模式：通过文档上下文和网络搜索集成增强推理能力 - llama3.2

+   文档处理（RAG模式）

+   PDF文档上传与处理

+   网页内容提取

+   自动文本分块与嵌入

+   Qdrant云平台中的向量存储

+   智能查询（RAG模式）

+   基于RAG的文档检索

+   带阈值过滤的相似度搜索

+   自动回退到网络搜索

+   答案来源归属

+   高级功能

+   Exa AI网络搜索集成

+   网络搜索的自定义域名过滤

+   上下文感知的响应生成

+   聊天历史管理

+   思维过程可视化

+   特定模型特性

+   灵活的模型选择：

+   Deepseek r1 1.5b（更轻量，适合大多数笔记本电脑）

+   Deepseek r1 7b（能力更强，需要更好的硬件）

+   用于向量嵌入的Snowflake Arctic Embedding模型（SOTA）

+   用于编排的Agno Agent框架

+   基于Streamlit的交互式界面

## 先决条件

1.  Ollama设置

1.  [安装Ollama](https://ollama.ai/)

1.  拉取Deepseek r1模型（组）：

# 对于更轻量级的模型

ollama pull deepseek-r1:1.5b

# 对于更强大的模型（如果你的硬件支持）

ollama pull deepseek-r1:7b

ollama拉取snowflake-arctic-embed

ollama拉取llama3.2

1.  Qdrant云设置（用于RAG模式）

1.  访问[Qdrant Cloud](https://cloud.qdrant.io/)

1.  创建账户或登录

1.  创建新集群

1.  获取您的凭证：

+   Qdrant API密钥：在API密钥部分找到

+   Qdrant URL：您的集群 URL（格式：https://xxx-xxx.cloud.qdrant.io）

1.  Exa AI API密钥（可选）

1.  访问[Exa AI](https://exa.ai/)

1.  注册账户

1.  生成用于网页搜索功能的 API 密钥

## 如何运行

1.  克隆仓库：

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd rag_tutorials/deepseek_local_rag_agent

1.  安装依赖项：

pip install -r requirements.txt

1.  运行应用程序：

streamlit run deepseek_rag_agent.py