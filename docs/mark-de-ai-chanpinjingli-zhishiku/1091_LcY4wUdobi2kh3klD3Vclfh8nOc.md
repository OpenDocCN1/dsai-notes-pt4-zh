# 🧐 具有推理能力的自主RAG

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/HdlVdksIdopFvWxUBexcF464nqf](https://qqs7y1hozd1.feishu.cn/docx/HdlVdksIdopFvWxUBexcF464nqf)

# 🧐 具有推理能力的自主RAG

> **一个复杂的RAG系统，使用Agno、Cla**ude和OpenAI展示AI智能体推理。

> 此实现允许用户上传文档、添加网络源、提问，并实时观察智能体的思考过程。

##### [Code](https://qqs7y1hozd1.feishu.cn/wiki/ZKytwurI2iSQKKkBxYachyrhnfb)

##### [requirements.txt](https://qqs7y1hozd1.feishu.cn/wiki/ZHt3wITpViMhGukipNzcThMfnnh)

![](img/892c674f500bf5519ee2151eadd23040.png)

[Code (1).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JzMzbAh91o3l4JxiqjwcjynDncc)

![](img/744c9c1a7f0a861d72b5fb2275beb5c7.png)

[requirements.txt (1).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JYcObaFAvoS8Z0xt9PGcOI3rn4b)

## 功能

1.  **交互式知识库管理**

+   上传文档以扩展知识库

+   动态添加网页内容的URL

+   使用 LanceDB 的持久向量数据库存储

1.  **透明推理过程**

+   实时展示智能体的思考步骤

+   推理过程和最终答案的并排视图

+   对RAG流程的清晰可见性

1.  **高级检索增强生成（RAG）功能**

+   使用OpenAI嵌入进行语义匹配的向量搜索

+   带有引用的来源归属

## 配置

+   Claude 3.5 Sonnet用于语言处理

+   用于向量搜索的OpenAI嵌入模型

+   用于逐步分析的推理工具

+   可定制的代理指令

## 先决条件

**需要以下 API 密钥：**

1.  **Anthropic API密钥**

+   [地址](http://console.anthropic.com)

+   导航到 API 密钥部分

+   创建新的 API 密钥

1.  **OpenAI API密钥**

+   [地址](http://platform.openai.com)

+   导航到 API 密钥部分

+   生成新的 API 密钥

## 如何运行

1.  克隆仓库：

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd rag_tutorials/agentic_rag_with_reasoning

1.  安装依赖项：

1.  pip install -r requirements.txt

1.  运行应用程序：

1.  streamlit run rag_reasoning_agent.py

1.  配置 API 密钥：

+   在第一个字段中输入您的Anthropic API密钥

+   在第二个字段中输入您的OpenAI API密钥

+   应用程序的正常运行需要这两个密钥

1.  使用应用程序：

+   添加知识源：使用侧边栏将URL添加到您的知识库

+   提问：在主输入框中输入查询内容

+   查看推理过程：实时观看智能体的思考过程展开

+   获取答案：接收附有来源引用的全面回应

## 工作原理

该应用程序使用了一个复杂的RAG管道：

### 知识库设置

+   使用 WebBaseLoader 从 URL 加载文档

+   文本使用OpenAI的嵌入模型进行分块和嵌入

+   向量存储在LanceDB中，以便高效检索

+   向量搜索可实现相关信息的语义匹配

### 代理处理

+   用户查询触发代理的推理过程

+   推理工具帮助智能体逐步思考

+   代理在知识库中搜索相关信息

+   Claude 4 Sonnet生成带有引用的全面答案

### UI流程

+   输入 API 密钥 → 添加知识源 → 提问

+   推理过程和答案生成并排显示

+   为保证透明度和验证而引用的来源

# 🧐 Agentic RAG with Reasoning

A sophisticated RAG system that demonstrates an AI agent's step-by-step reasoning process using Agno, Claude and OpenAI. This implementation allows users to upload documents, add web sources, ask questions, and observe the agent's thought process in real-time.

## Features

1.  Interactive Knowledge Base Management

+   Upload documents to expand the knowledge base

+   Add URLs dynamically for web content

+   Persistent vector database storage using LanceDB

1.  Transparent Reasoning Process

+   Real-time display of the agent's thinking steps

+   Side-by-side view of reasoning and final answer

+   Clear visibility into the RAG process

1.  Advanced RAG Capabilities

+   Vector search using OpenAI embeddings for semantic matching

+   Source attribution with citations

## Agent Configuration

+   Claude 3.5 Sonnet for language processing

+   OpenAI embedding model for vector search

+   ReasoningTools for step-by-step analysis

+   Customizable agent instructions

## Prerequisites

You'll need the following API keys:

1.  Anthropic API Key

+   Sign up at console.anthropic.com

+   Navigate to API Keys section

+   Create a new API key

1.  OpenAI API Key

+   Sign up at platform.openai.com

+   Navigate to API Keys section

+   Generate a new API key

## How to Run

1.  Clone the Repository:

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd rag_tutorials/agentic_rag_with_reasoning

1.  Install the dependencies:

1.  pip install -r requirements.txt

1.  Run the Application:

1.  streamlit run rag_reasoning_agent.py

1.  Configure API Keys:

+   Enter your Anthropic API key in the first field

+   Enter your OpenAI API key in the second field

+   Both keys are required for the app to function

1.  Use the Application:

+   Add Knowledge Sources: Use the sidebar to add URLs to your knowledge base

+   Ask Questions: Enter queries in the main input field

+   View Reasoning: Watch the agent's thought process unfold in real-time

+   Get Answers: Receive comprehensive responses with source citations

## How It Works

The application uses a sophisticated RAG pipeline:

### Knowledge Base Setup

+   Documents are loaded from URLs using WebBaseLoader

+   Text is chunked and embedded using OpenAI's embedding model

+   Vectors are stored in LanceDB for efficient retrieval

+   Vector search enables semantic matching for relevant information

### Agent Processing

+   User queries trigger the agent's reasoning process

+   ReasoningTools help the agent think step-by-step

+   The agent searches the knowledge base for relevant information

+   Claude 4 Sonnet generates comprehensive answers with citations

### UI Flow

+   Enter API keys → Add knowledge sources → Ask questions

+   Reasoning process and answer generation displayed side-by-side

+   Sources cited for transparency and verification