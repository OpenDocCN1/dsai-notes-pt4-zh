# 🖥️ 具备混合搜索功能的本地RAG应用

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/AwZHdNgZ8oBe1ix96b7ctdlon6d](https://qqs7y1hozd1.feishu.cn/docx/AwZHdNgZ8oBe1ix96b7ctdlon6d)

# 🖥️ 具备混合搜索功能的本地RAG应用

> **一款强大的文档问答应用程序，它利用混合搜索（RAG）和本**地大语言模型（LLM）来提供全面的答案。

> 该系统采用RAGLite进行稳健的文档处理和检索，并使用Streamlit构建直观的聊天界面，将特定文档知识与本地大语言模型能力相结合，以提供准确且有上下文的响应。

##### [Code](https://qqs7y1hozd1.feishu.cn/wiki/RFsrw5eUiiLa36k8Ws7cZy1ankc)

##### [requirements.txt](https://qqs7y1hozd1.feishu.cn/wiki/OdN4wEf6kizTqhkV5DicfNRPnkb)

![](img/a58156fc8a521c2db43c7329483b6dbe.png)

[Code (3).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EXF9bdEXdoJFbAxvj2RcH4vQnNh)

![](img/cb0c2082ec102a38034cf952187041df.png)

[requirements.txt (3).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FnIvbuFfloxSGdxj0zvcvk4Pnmc)

## 特性

+   **本地大语言模型集成：**

> 使用 llama-cpp-python 模型进行本地推理

> 支持多种量化格式（推荐Q4_K_M）

> 可配置的上下文窗口大小

+   **文档处理：**

> PDF文档上传与处理

> 自动文本分块和嵌入

> 结合语义和关键词匹配的混合搜索

> 重排序以实现更好的上下文选择

+   **多模型集成：**

> 本地大语言模型用于文本生成（例如，Llama-3.2-3B-Instruct）

> 使用BGE模型的本地嵌入

> 用于局部重排序的FlashRank

## 模型设置

> **RAGLite通过使用llama-cpp-python支持ll**ama.cpp模型来扩展LiteLLM。

> 要选择一个llama.cpp模型（例如，从bartowski的集合中选择），请使用格式为"llama-cpp-python/<hugging_face_repo_id>/@<n_ctx>"的模型标识符，其中n_ctx是一个可选参数，用于指定模型的上下文大小。

1.  大语言模型路径格式：

```
llama-cpp-python/&lt;repo&gt;/&lt;model&gt;/&lt;filename&gt;@&lt;context_length&gt;
```

1.  示例：

```
bartowski/Llama-3.2-3B-Instruct-GGUF/Llama-3.2-3B-Instruct-Q4_K_M.gguf@4096
```

1.  嵌入器模型路径格式：

```
llama-cpp-python/&lt;repo&gt;/&lt;model&gt;/&lt;filename&gt;@&lt;dimension&gt;
```

1.  示例：

```
lm-kit/bge-m3-gguf/bge-m3-Q4_K_M.gguf@1024
```

## 先决条件

1.  安装spaCy模型：

1.  pip install https://github.com/explosion/spacy-models/releases/download/xx_sent_ud_3.7.0/xx_sent_ud_3.7.0-py3-none-any.whl

1.  安装加速版llama-cpp-python（可选但推荐）：

1.  # 配置安装变量
LLAMA_CPP_PYTHON_VERSION=0.3.2
PYTHON_VERSION=310 # 3.10、3.11、3.12
ACCELERATOR=metal  # 适用于Mac# ACCELERATOR=cu121  # 适用于NVIDIA GPU
PLATFORM=macosx_11_0_arm64  # 适用于Mac# PLATFORM=linux_x86_64  # 适用于Linux# PLATFORM=win_amd64  # 适用于Windows# 安装加速版本
pip install "https://github.com/abetlen/llama-cpp-python/releases/download/v$LLAMA_CPP_PYTHON_VERSION-$ACCELERATOR/llama_cpp_python-$LLAMA_CPP_PYTHON_VERSION-cp$PYTHON_VERSION-cp$PYTHON_VERSION-$PLATFORM.whl"

1.  安装依赖项：

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/local_hybrid_search_rag
pip install -r requirements.txt

## 数据库设置

该应用程序支持多种数据库后端：

+   PostgreSQL（推荐）：

+   只需点击几下，即可在[Neon](https://neon.tech/)创建一个免费的无服务器PostgreSQL数据库

+   获取即时预配和缩容至零的能力

+   连接字符串格式：postgresql://用户:密码@ep-xyz.region.aws.neon.tech/数据库名

## 运行方法

1.  启动应用程序：

1.  streamlit run local_main.py

1.  配置应用程序：

+   输入大语言模型路径

+   输入嵌入器模型路径

+   设置数据库URL

+   点击“保存配置”

1.  上传文档：

+   通过界面上传PDF文件

+   等待处理完成

1.  开始聊天：

+   询问有关您文档的问题

+   使用本地大语言模型获取回复

+   必要时回退到通用知识

## 注释

+   对于大多数用例，建议使用4096的上下文窗口大小

+   Q4_K_M量化在速度和质量之间实现了良好的平衡

+   1024维的BGE-M3嵌入器是最优的

+   本地模型需要足够的RAM和CPU/GPU资源

+   Mac 支持 Metal 加速，NVIDIA GPU 支持 CUDA

## 贡献

欢迎贡献！请随时提交拉取请求。

# 🖥️ Local RAG App with Hybrid Search

A powerful document Q&A application that leverages Hybrid Search (RAG) and local LLMs for comprehensive answers. Built with RAGLite for robust document processing and retrieval, and Streamlit for an intuitive chat interface, this system combines document-specific knowledge with local LLM capabilities to deliver accurate and contextual responses.

## Quick Start

For immediate testing, use these tested model configurations:

# LLM Model

bartowski/Llama-3.2-3B-Instruct-GGUF/Llama-3.2-3B-Instruct-Q4_K_M.gguf@4096

# Embedder Model

lm-kit/bge-m3-gguf/bge-m3-Q4_K_M.gguf@1024

These models offer a good balance of performance and resource usage, and have been verified to work well together even on a MacBook Air M2 with 8GB RAM.

## Features

+   Local LLM Integration:

+   Uses llama-cpp-python models for local inference

+   Supports various quantization formats (Q4_K_M recommended)

+   Configurable context window sizes

+   Document Processing:

+   PDF document upload and processing

+   Automatic text chunking and embedding

+   Hybrid search combining semantic and keyword matching

+   Reranking for better context selection

+   Multi-Model Integration:

+   Local LLM for text generation (e.g., Llama-3.2-3B-Instruct)

+   Local embeddings using BGE models

+   FlashRank for local reranking

## Prerequisites

1.  Install spaCy Model:

1.  pip install https://github.com/explosion/spacy-models/releases/download/xx_sent_ud_sm-3.7.0/xx_sent_ud_sm-3.7.0-py3-none-any.whl

1.  Install Accelerated llama-cpp-python (Optional but recommended):

1.  # Configure installation variables
LLAMA_CPP_PYTHON_VERSION=0.3.2
PYTHON_VERSION=310 # 3.10, 3.11, 3.12
ACCELERATOR=metal  # For Mac# ACCELERATOR=cu121  # For NVIDIA GPU
PLATFORM=macosx_11_0_arm64  # For Mac# PLATFORM=linux_x86_64  # For Linux# PLATFORM=win_amd64  # For Windows# Install accelerated version
pip install "https://github.com/abetlen/llama-cpp-python/releases/download/v$LLAMA_CPP_PYTHON_VERSION-$ACCELERATOR/llama_cpp_python-$LLAMA_CPP_PYTHON_VERSION-cp$PYTHON_VERSION-cp$PYTHON_VERSION-$PLATFORM.whl"

1.  Install Dependencies:

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/local_hybrid_search_rag
pip install -r requirements.txt

## Model Setup

RAGLite extends LiteLLM with support for llama.cpp models using llama-cpp-python. To select a llama.cpp model (e.g., from bartowski's collection), use a model identifier of the form "llama-cpp-python/<hugging_face_repo_id>/@<n_ctx>", where n_ctx is an optional parameter that specifies the context size of the model.

1.  LLM Model Path Format:

```
llama-cpp-python/&lt;repo&gt;/&lt;model&gt;/&lt;filename&gt;@&lt;context_length&gt;
```

1.  Example:

```
bartowski/Llama-3.2-3B-Instruct-GGUF/Llama-3.2-3B-Instruct-Q4_K_M.gguf@4096
```

1.  Embedder Model Path Format:

```
llama-cpp-python/&lt;repo&gt;/&lt;model&gt;/&lt;filename&gt;@&lt;dimension&gt;
```

1.  Example:

```
lm-kit/bge-m3-gguf/bge-m3-Q4_K_M.gguf@1024
```

## Database Setup

The application supports multiple database backends:

+   PostgreSQL (Recommended):

+   Create a free serverless Pos[tgre](https://neon.tech/)SQL database at Neon in a few clicks

+   Get instant provisioning and scale-to-zero capability

+   Connection string format: postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname

## How to Run

1.  Start the Application:

1.  streamlit run local_main.py

1.  Configure the Application:

+   Enter LLM model path

+   Enter embedder model path

+   Set database URL

+   Click "Save Configuration"

1.  Upload Documents:

+   Upload PDF files through the interface

+   Wait for processing completion

1.  Start Chatting:

+   Ask questions about your documents

+   Get responses using local LLM

+   Fallback to general knowledge when needed

## Notes

+   Context window size of 4096 is recommended for most use cases

+   Q4_K_M quantization offers good balance of speed and quality

+   BGE-M3 embedder with 1024 dimensions is optimal

+   Local models require sufficient RAM and CPU/GPU resources

+   Metal acceleration available for Mac, CUDA for NVIDIA GPUs

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.