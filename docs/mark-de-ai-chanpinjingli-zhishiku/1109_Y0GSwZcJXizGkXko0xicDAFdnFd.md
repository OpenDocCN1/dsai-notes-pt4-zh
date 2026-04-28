# 👀 具备混合搜索功能的RAG应用

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LNmrdOVHPooniNxdq1ucnwhtnIf](https://qqs7y1hozd1.feishu.cn/docx/LNmrdOVHPooniNxdq1ucnwhtnIf)

# 👀 具备混合搜索功能的RAG应用

> **一款强大的文档问答应用程序，它利用混合搜索（RAG）和Cl**aude的先进语言能力来提供全面的答案。

> 该系统采用RAGLite进行稳健的文档处理和检索，使用Streamlit构建直观的聊天界面，无缝地将特定文档知识与Claude的通用智能相结合，以提供准确且有上下文的响应。

[Code](https://qqs7y1hozd1.feishu.cn/wiki/L0acwucEjiev37kKQVfcFLfanVf)

[requirements.txt](https://qqs7y1hozd1.feishu.cn/wiki/V9qwwtMeQiBLVnknx7Acb7H1nMh)

![](img/feb82d2e1ab4a83391ae69ef8798709e.png)

[Code (4).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VSWwbTRYCoNPMUxtMM0c0to5neg)

![](img/8380884b69397c089a095262e5fe276d.png)

[requirements.txt (4).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HmdYbZsQAofIeyx9omxcl1q3nNe)

## 功能

+   **混合搜索问答**

+   基于检索增强生成（RAG）的特定文档查询答案

+   对于常识性问题，回退到Claude

+   **文档处理：**

+   PDF文档上传与处理

+   自动文本分块与嵌入

+   结合语义和关键词匹配的混合搜索

+   重排序以实现更好的上下文选择

+   **多模型集成：**

+   Claude文本生成 - 使用Claude 3 Opus测试

+   OpenAI用于嵌入 - 使用text-embedding-3-large进行测试

+   用于重排序的Cohere - 使用Cohere 3.5重排序器进行测试

## 先决条件

需要以下 API 密钥和数据库设置：

1.  数据库：在[Neon](https://neon.tech/)创建一个免费的PostgreSQL数据库：

+   在Neon注册/登录

+   创建新项目

+   复制连接字符串（格式如下：postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname）

1.  API密钥：

+   用于嵌入的OpenAI API密钥

+   [Anthropic API密钥](https://console.anthropic.com/settings/keys)用于Claude

+   用于重排序的Cohere API密钥

## 如何开始？

1.  克隆仓库：

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/hybrid_search_rag

1.  安装依赖项：

1.  pip install -r requirements.txt

1.  安装spaCy模型：

1.  pip install https://github.com/explosion/spacy-models/releases/download/xx_sent_ud_3.7.0/xx_sent_ud_3.7.0-py3-none-any.whl

1.  运行应用程序：

1.  streamlit run main.py

## 用法

1.  启动应用程序

1.  在侧边栏中输入您的 API 密钥：

+   OpenAI API密钥

+   Anthropic API密钥

+   Cohere API密钥

+   数据库URL（可选，默认为SQLite）

1.  点击“保存配置”

1.  上传PDF文档

1.  开始提问吧！

+   特定文档的问题将使用RAG

+   一般性问题将直接使用Claude

## 数据库选项

该应用程序支持多种数据库后端：

+   PostgreSQL（推荐）：

+   [创建一个](https://neon.tech/)免费的无服务器PostgreSQL数据库在Neon

+   获得即时预配和缩容至零的能力

+   连接字符串格式：postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname

+   MySQL：

```
mysql://user:pass@host:port/db
```

+   SQLite（本地开发）：

```
sqlite:///path/to/db.sqlite
```

## 贡献

欢迎贡献！请随时提交拉取请求。

# 👀 RAG App with Hybrid Search

A powerful document Q&A application that leverages Hybrid Search (RAG) and Claude's advanced language capabilities to provide comprehensive answers. Built with RAGLite for robust document processing and retrieval, and Streamlit for an intuitive chat interface, this system seamlessly combines document-specific knowledge with Claude's general intelligence to deliver accurate and contextual responses.

## Features

+   Hybrid Search Question Answering

+   RAG-based answers for document-specific queries

+   Fallback to Claude for general knowledge questions

+   Document Processing:

+   PDF document upload and processing

+   Automatic text chunking and embedding

+   Hybrid search combining semantic and keyword matching

+   Reranking for better context selection

+   Multi-Model Integration:

+   Claude for text generation - tested with Claude 3 Opus

+   OpenAI for embeddings - tested with text-embedding-3-large

+   Cohere for reranking - tested with Cohere 3.5 reranker

## Prerequisites

You'll need the following API keys and database setup:

1.  Database: Create a free Pos[tgre](https://neon.tech/)SQL database at Neon:

+   Sign up/Login at Neon

+   Create a new project

+   Copy the connection string (looks like: postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname)

1.  API Keys:

+   [OpenAI API key](https://platform.openai.com/api-keys) for embeddings

+   Anthropic API key for Claude

+   [Cohere API key](https://dashboard.cohere.com/api-keys) for reranking

## How to get Started?

1.  Clone the Repository:

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/hybrid_search_rag

1.  Install Dependencies:

1.  pip install -r requirements.txt

1.  Install spaCy Model:

1.  pip install https://github.com/explosion/spacy-models/releases/download/xx_sent_ud_sm-3.7.0/xx_sent_ud_sm-3.7.0-py3-none-any.whl

1.  Run the Application:

1.  streamlit run main.py

## Usage

1.  Start the application

1.  Enter your API keys in the sidebar:

+   OpenAI API key

+   Anthropic API key

+   Cohere API key

+   Database URL (optional, defaults to SQLite)

1.  Click "Save Configuration"

1.  Upload PDF documents

1.  Start asking questions!

+   Document-specific questions will use RAG

+   General questions will use Claude directly

## Database Options

The application supports multiple database backends:

+   PostgreSQL (Recommended):

+   Create a free serverless Pos[tgre](https://neon.tech/)SQL database at Neon

+   Get instant provisioning and scale-to-zero capability

+   Connection string format: postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname

+   MySQL:

```
mysql://user:pass@host:port/db
```

+   SQLite (Local development):

```
sqlite:///path/to/db.sqlite
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.