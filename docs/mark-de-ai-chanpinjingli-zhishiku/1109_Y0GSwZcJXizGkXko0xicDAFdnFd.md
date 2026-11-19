
# 👀 具备混合搜索功能的RAG应用

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LNmrdOVHPooniNxdq1ucnwhtnIf](https://qqs7y1hozd1.feishu.cn/docx/LNmrdOVHPooniNxdq1ucnwhtnIf)

> 一款强大的文档问答应用程序，它利用混合搜索（RAG）和 Claude 的先进语言能力来提供全面的答案。
> 该系统采用 RAGLite 进行稳健的文档处理和检索，使用 Streamlit 构建直观的聊天界面，无缝地将特定文档知识与 Claude 的通用智能相结合，以提供准确且有上下文的响应。

[Code](https://qqs7y1hozd1.feishu.cn/wiki/L0acwucEjiev37kKQVfcFLfanVf)

[`requirements.txt`](https://qqs7y1hozd1.feishu.cn/wiki/V9qwwtMeQiBLVnknx7Acb7H1nMh)

![](img/feb82d2e1ab4a83391ae69ef8798709e.png)

[`Code (4).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VSWwbTRYCoNPMUxtMM0c0to5neg)

![](img/8380884b69397c089a095262e5fe276d.png)

[`requirements.txt (4).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HmdYbZsQAofIeyx9omxcl1q3nNe)

## 功能

+ 混合搜索问答
+ 基于检索增强生成（RAG）的特定文档查询答案
+ 对于常识性问题，回退到 Claude
+ 文档处理：
+ PDF 文档上传与处理
+ 自动文本分块与嵌入
+ 结合语义和关键词匹配的混合搜索
+ 重排序以实现更好的上下文选择
+ 多模型集成：
+ Claude 文本生成 - 使用 Claude 3 Opus 测试
+ OpenAI 用于嵌入 - 使用 `text-embedding-3-large` 进行测试
+ 用于重排序的 Cohere - 使用 Cohere 3.5 重排序器进行测试

## 先决条件

需要以下 API 密钥和数据库设置：

1. 数据库：在 [Neon](https://neon.tech/) 创建一个免费的 PostgreSQL 数据库：
   + 在 Neon 注册/登录
   + 创建新项目
   + 复制连接字符串（格式如下：`postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname`）

2. API 密钥：
   + 用于嵌入的 OpenAI API 密钥
   + [Anthropic API 密钥](https://console.anthropic.com/settings/keys) 用于 Claude
   + 用于重排序的 Cohere API 密钥

## 如何开始？

### 克隆仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/hybrid_search_rag
```

### 安装依赖项

```bash
pip install -r requirements.txt
```

### 安装 spaCy 模型

```bash
pip install https://github.com/explosion/spacy-models/releases/download/xx_sent_ud_3.7.0/xx_sent_ud_3.7.0-py3-none-any.whl
```

### 运行应用程序

```bash
streamlit run main.py
```

## 用法

1. 启动应用程序
2. 在侧边栏中输入您的 API 密钥：
   + OpenAI API 密钥
   + Anthropic API 密钥
   + Cohere API 密钥
   + 数据库 URL（可选，默认为 SQLite）
3. 点击“保存配置”
4. 上传 PDF 文档
5. 开始提问吧！
   + 特定文档的问题将使用 RAG
   + 一般性问题将直接使用 Claude

## 数据库选项

该应用程序支持多种数据库后端：

+ PostgreSQL（推荐）：
  + 在 [Neon](https://neon.tech/) 创建一个免费的无服务器 PostgreSQL 数据库
  + 获得即时预配和缩容至零的能力
  + 连接字符串格式：`postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname`

+ MySQL：
  ```
  mysql://user:pass@host:port/db
  ```

+ SQLite（本地开发）：
  ```
  sqlite:///path/to/db.sqlite
  ```

## 贡献

欢迎贡献！请随时提交拉取请求。

# 👀 RAG App with Hybrid Search

A powerful document Q&A application that leverages Hybrid Search (RAG) and Claude's advanced language capabilities to provide comprehensive answers. Built with RAGLite for robust document processing and retrieval, and Streamlit for an intuitive chat interface, this system seamlessly combines document-specific knowledge with Claude's general intelligence to deliver accurate and contextual responses.

[Code](https://qqs7y1hozd1.feishu.cn/wiki/L0acwucEjiev37kKQVfcFLfanVf)

[`requirements.txt`](https://qqs7y1hozd1.feishu.cn/wiki/V9qwwtMeQiBLVnknx7Acb7H1nMh)

![](img/feb82d2e1ab4a83391ae69ef8798709e.png)

[`Code (4).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VSWwbTRYCoNPMUxtMM0c0to5neg)

![](img/8380884b69397c089a095262e5fe276d.png)

[`requirements.txt (4).docx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HmdYbZsQAofIeyx9omxcl1q3nNe)

## Features

+ Hybrid Search Question Answering
+ RAG-based answers for document-specific queries
+ Fallback to Claude for general knowledge questions
+ Document Processing:
+ PDF document upload and processing
+ Automatic text chunking and embedding
+ Hybrid search combining semantic and keyword matching
+ Reranking for better context selection
+ Multi-Model Integration:
+ Claude for text generation - tested with Claude 3 Opus
+ OpenAI for embeddings - tested with `text-embedding-3-large`
+ Cohere for reranking - tested with Cohere 3.5 reranker

## Prerequisites

You'll need the following API keys and database setup:

1. Database: Create a free [PostgreSQL](https://neon.tech/) database at Neon:
   + Sign up/Login at Neon
   + Create a new project
   + Copy the connection string (looks like: `postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname`)

2. API Keys:
   + [OpenAI API key](https://platform.openai.com/api-keys) for embeddings
   + Anthropic API key for Claude
   + [Cohere API key](https://dashboard.cohere.com/api-keys) for reranking

## How to get Started?

### Clone the Repository

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/hybrid_search_rag
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Install spaCy Model

```bash
pip install https://github.com/explosion/spacy-models/releases/download/xx_sent_ud_sm-3.7.0/xx_sent_ud_sm-3.7.0-py3-none-any.whl
```

### Run the Application

```bash
streamlit run main.py
```

## Usage

1. Start the application
2. Enter your API keys in the sidebar:
   + OpenAI API key
   + Anthropic API key
   + Cohere API key
   + Database URL (optional, defaults to SQLite)
3. Click "Save Configuration"
4. Upload PDF documents
5. Start asking questions!
   + Document-specific questions will use RAG
   + General questions will use Claude directly

## Database Options

The application supports multiple database backends:

+ PostgreSQL (Recommended):
  + Create a free serverless [PostgreSQL](https://neon.tech/) database at Neon
  + Get instant provisioning and scale-to-zero capability
  + Connection string format: `postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname`

+ MySQL:
  ```
  mysql://user:pass@host:port/db
  ```

+ SQLite (Local development):
  ```
  sqlite:///path/to/db.sqlite
  ```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
