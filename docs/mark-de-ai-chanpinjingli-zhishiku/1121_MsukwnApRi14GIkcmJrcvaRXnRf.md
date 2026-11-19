
# 🐋 Qwen 3 / 通义千问3 本地 RAG 推理代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/JOSpdWh9xovOoTxK8Zqczrc6nyc](https://qqs7y1hozd1.feishu.cn/docx/JOSpdWh9xovOoTxK8Zqczrc6nyc)

## English

This RAG Application demonstrates how to build a powerful Retrieval-Augmented Generation (RAG) system using locally running `Qwen 3` and `Gemma 3` models via `Ollama`. It combines document processing, vector search, and web search capabilities to provide accurate, context-aware responses to user queries.

### Features

+ 🧠 Multiple Local LLM Options:

+ `Qwen3` (1.7b, 8b) - Alibaba's latest language models

+ `Gemma3` (1b, 4b) - Google's efficient language models with multimodal capabilities

+ `DeepSeek` (1.5b) - Alternative model option

+ 📚 Comprehensive RAG System:

+ Upload and process `PDF` documents

+ Extract content from web `URL`s

+ Intelligent chunking and embedding

+ Similarity search with adjustable threshold

+ 🌐 Web Search Integration:

+ Fallback to web search when document knowledge is insufficient

+ Configurable domain filtering

+ Source attribution in responses

+ 🔄 Flexible Operation Modes:

+ Toggle between RAG and direct LLM interaction

+ Force web search when needed

+ Adjust similarity thresholds for document retrieval

+ 💾 Vector Database Integration:

+ `Qdrant` vector database for efficient similarity search

+ Persistent storage of document embeddings

### How to Get Started

#### Prerequisites

+ [`Ollama`](https://ollama.ai/) installed locally

+ `Python` 3.8+

+ `Qdrant` account (free tier available) for vector storage

+ `Exa API` key (optional, for web search capability)

#### Installation

1. Clone the GitHub repository

```
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd rag_tutorials/qwen_local_rag
```

2. Install the required dependencies:

```
pip install -r requirements.txt
```

3. Pull the required models using `Ollama`:

```
ollama pull qwen3:1.7b # Or any other model you want to use

ollama pull snowflake-arctic-embed # Or any other model you want to use
```

4. Run `Qdrant` locally through docker

```
docker pull qdrant/qdrant

docker run -p 6333:6333 -p 6334:6334 \

-v "$(pwd)/qdrant_storage:/qdrant/storage:z" \

qdrant/qdrant
```

5. Get your API keys:

+ `Exa API` key (optional, for web search)

6. Run the application:

```
streamlit run qwen_local_rag_agent.py
```

### How It Works

1. Document Processing:

+ `PDF` files are processed using `PyPDFLoader`

+ Web content is extracted using `WebBaseLoader`

+ Documents are split into chunks with `RecursiveCharacterTextSplitter`

2. Vector Database:

+ Document chunks are embedded using `Ollama`'s embedding models

+ Embeddings are stored in `Qdrant` vector database

+ Similarity search retrieves relevant documents based on query

3. Query Processing:

+ User queries are analyzed to determine the best information source

+ System checks document relevance using similarity threshold

+ Falls back to web search if no relevant documents are found

4. Response Generation:

+ Local LLM (`Qwen`/`Gemma`) generates responses based on retrieved context

+ Sources are cited and displayed to the user

+ Web search results are clearly indicated when used

### Configuration Options

+ Model Selection: Choose between different `Qwen`, `Gemma`, and `DeepSeek` models

+ RAG Mode: Toggle between RAG-enabled and direct LLM interaction

+ Search Tuning: Adjust similarity threshold for document retrieval

+ Web Search: Enable/disable web search fallback and configure domain filtering

### Use Cases

+ Document Q&A: Ask questions about your uploaded documents

+ Research Assistant: Combine document knowledge with web search

+ Local Privacy: Process sensitive documents without sending data to external APIs

+ Offline Operation: Run advanced AI capabilities with limited or no internet access

### Requirements

See `requirements.txt` for the complete list of dependencies.

## 中文

此 RAG 应用程序展示了如何通过 `Ollama` 使用本地运行的 `Qwen 3` 和 `Gemma 3` 模型构建强大的检索增强生成（RAG）系统。它结合了文档处理、向量搜索和网络搜索功能，为用户查询提供准确、有上下文感知的响应。

### 特性

+ 🧠 多种本地大语言模型选项：

+ `Qwen3`（17亿、80亿参数）——阿里巴巴最新语言模型

+ `Gemma3` (1b, 4b) - 谷歌具有多模态能力的高效语言模型

+ `DeepSeek` (1.5b) - 替代模型选项

+ 📚 综合 RAG 系统：

+ 上传并处理 `PDF` 文档

+ 从网页 `URL` 提取内容

+ 智能分块与嵌入

+ 具有可调节阈值的相似性搜索

+ 🌐 网络搜索集成：

+ 当文档知识不足时，回退到网络搜索

+ 可配置的域名过滤

+ 回复中的来源归属

+ 🔄 灵活的操作模式：

+ 切换 RAG 和直接大语言模型交互

+ 必要时强制进行网络搜索

+ 调整文档检索的相似度阈值

+ 💾 向量数据库集成：

+ `Qdrant` 向量数据库，用于高效的相似性搜索

+ 文档嵌入的持久存储

### 如何入门

#### 先决条件

+ [`Ollama`](https://ollama.ai/) 已在本地安装

+ `Python` 3.8+

+ 用于向量存储的 `Qdrant` 账户（提供免费层）

+ `Exa API` 密钥（可选，用于网页搜索功能）

### 安装

1. 克隆 GitHub 仓库

```
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd rag_tutorials/qwen_local_rag
```

2. 安装所需的依赖项：

```
pip install -r requirements.txt
```

3. 使用 `Ollama` 拉取所需模型：

```
ollama pull qwen3:1.7b # 或任何你想使用的其他模型

ollama pull snowflake-arctic-embed # 或任何你想使用的其他模型
```

4. 通过 Docker 在本地运行 `Qdrant`

```
docker pull qdrant/qdrant

docker run -p 6333:6333 -p 6334:6334 \

-v "$(pwd)/qdrant_storage:/qdrant/storage:z" \

qdrant/qdrant
```

5. 获取您的 API 密钥：

+ `Exa API` 密钥（可选，用于网络搜索）

6. 运行应用程序：

```
streamlit run qwen_local_rag_agent.py
```

### 工作原理

1. 文档处理：

+ `PDF` 文件使用 `PyPDFLoader` 进行处理

+ 网页内容使用 `WebBaseLoader` 提取

+ 文档使用递归字符文本分割器拆分为多个块

2. 向量数据库：

+ 文档块使用 `Ollama` 的嵌入模型进行嵌入

+ 嵌入向量存储在 `Qdrant` 向量数据库中

+ 相似性搜索根据查询检索相关文档

3. 查询处理：

+ 分析用户查询以确定最佳信息来源

+ 系统使用相似度阈值检查文档相关性

+ 如果未找到相关文档，则回退到网络搜索

4. 响应生成：

+ 本地大语言模型（`Qwen`/`Gemma`）根据检索到的上下文生成回复

+ 来源被引用并展示给用户

+ 使用网络搜索结果时会明确标注

### 配置选项

+ 模型选择：在不同的 `Qwen`、`Gemma` 和 `DeepSeek` 模型之间进行选择

+ 检索增强生成（RAG）模式：在启用 RAG 和直接与大语言模型（LLM）交互之间切换

+ 搜索调优：调整文档检索的相似度阈值

+ 网页搜索：启用/禁用网页搜索回退并配置域名过滤

### 用例

+ 文档问答：对您上传的文档提问

+ 研究助理：将文档知识与网络搜索相结合

+ 本地隐私：处理敏感文档时无需将数据发送到外部 API

+ 离线操作：在有限或无网络连接的情况下运行高级 AI 功能

### 要求

请查看 `requirements.txt` 以获取完整的依赖项列表。
