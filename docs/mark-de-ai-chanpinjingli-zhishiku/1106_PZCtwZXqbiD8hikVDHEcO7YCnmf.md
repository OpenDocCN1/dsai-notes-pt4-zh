
# 🤔 基于Gemini Flash Thinking的自主RAG

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LPiAdoKxsoHeB1xDZ6GcIK4Nntg](https://qqs7y1hozd1.feishu.cn/docx/LPiAdoKxsoHeB1xDZ6GcIK4Nntg)

> 一个基于新的Gemini 快速思维模型和gemini-exp-1206构建的RAG智能体系统。

> 使用Qdrant进行向量存储，使用Agno（phidata前身）进行智能体编排。该应用程序具备智能查询重写、文档处理和网络搜索回退功能，以提供全面的AI驱动响应。

[Code](https://qqs7y1hozd1.feishu.cn/wiki/LxUfwzN2xiMlDykjiS9cKuyBnjh)

[requirements.txt](https://qqs7y1hozd1.feishu.cn/wiki/Elcqw0d9iiVHCDkylaAcQrxdnMd)

![](img/2ea9b347e579a38a1eb7dcf209642780.png)

[Code (3).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KjMCby4hMoCefwxVCVvchrP4nge)

![](img/34b482d1f243e21a18204ae9467a16aa.png)

[requirements.txt (3).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/V88pbz6WmofXHvx4q0Gclt3nnth)

## 功能

### 文档处理
> PDF文档上传与处理
> 网页内容提取
> 自动文本分块与嵌入
> Qdrant云平台中的向量存储

### 智能查询
> 查询重写以实现更好的检索
> 基于RAG的文档检索
> 带阈值过滤的相似度搜索
> 自动回退到网络搜索
> 答案来源归属

### 高级功能
> Exa AI网络搜索集成
> 网络搜索的自定义域名过滤
> 上下文感知的响应生成
> 聊天历史管理
> 查询重构代理

### 特定模型特性
> 双子思维2.0闪存，用于聊天和推理
> 用于向量嵌入的Gemini嵌入模型
> 用于编排的Agno Agent框架
> 基于Streamlit的交互式界面

## 先决条件

### 谷歌 API 密钥
前往Google AI Studio
注册或登录您的账户
创建新的 API 密钥

### Qdrant云设置
访问[Qdrant Cloud](https://cloud.qdrant.io/)
创建账户或登录
创建新集群

获取您的凭证：
+   Qdrant API密钥：在API密钥部分找到
+   Qdrant URL：您的集群 URL（格式：`https://xxx-xxx.cloud.qdrant.io`）

### Exa AI API密钥（可选）
1.  访问[Exa AI](https://exa.ai/)
1.  注册账户
1.  生成用于网页搜索功能的 API 密钥

## 如何运行

### 克隆仓库
`git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git`
`cd rag_tutorials/gemini_agentic_rag`

### 安装依赖项
`pip install -r requirements.txt`

### 运行应用程序
`streamlit run agentic_rag_gemini.py`

## 用法

### 在侧边栏中配置 API 密钥
+   输入您的谷歌 API 密钥
+   添加Qdrant凭证
+   （可选）添加用于网络搜索的Exa AI密钥

### 上传文档
+   使用文件上传器上传PDF文件
+   输入网页内容的URL

### 提问
+   在聊天界面中输入您的查询内容
+   查看改写后的查询和来源
+   在相关时查看网络搜索结果

### 管理您的会话
+   根据需要清除聊天记录
+   配置网络搜索域
+   监控已处理文档

# 🤔 Agentic RAG with Gemini Flash Thinking

A RAG Agentic system built with the new Gemini 2.0 Flash Thinking model and gemini-exp-1206, Qdrant for vector storage, and Agno (phidata prev) for agent orchestration. This application features intelligent query rewriting, document processing, and web search fallback capabilities to provide comprehensive AI-powered responses.

## Features

### Document Processing
> PDF document upload and processing
> Web page content extraction
> Automatic text chunking and embedding
> Vector storage in Qdrant cloud

### Intelligent Querying
> Query rewriting for better retrieval
> RAG-based document retrieval
> Similarity search with threshold filtering
> Automatic fallback to web search
> Source attribution for answers

### Advanced Capabilities
> Exa AI web search integration
> Custom domain filtering for web search
> Context-aware response generation
> Chat history management
> Query reformulation agent

### Model Specific Features
> Gemini Thinking 2.0 Flash for chat and reasoning
> Gemini Embedding model for vector embeddings
> Agno Agent framework for orchestration
> Streamlit-based interactive interface

## Prerequisites

### Google API Key
Go to Google AI Studio
Sign up or log in to your account
Create a new API key

### Qdrant Cloud Setup
Visit [Qdrant Cloud](https://cloud.qdrant.io/)
Create an account or sign in
Create a new cluster

Get your credentials:
+   Qdrant API Key: Found in API Keys section
+   Qdrant URL: Your cluster URL (format: `https://xxx-xxx.cloud.qdrant.io`)

### Exa AI API Key (Optional)
1.  Visit [Exa AI](https://exa.ai/)
1.  Sign up for an account
1.  Generate an API key for web search capabilities

## How to Run

### Clone the repository
`git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git`
`cd rag_tutorials/gemini_agentic_rag`

### Install dependencies
`pip install -r requirements.txt`

### Run the application
`streamlit run agentic_rag_gemini.py`

## Usage

### Configure API keys in the sidebar
+   Enter your Google API key
+   Add Qdrant credentials
+   (Optional) Add Exa AI key for web search

### Upload documents
+   Use the file uploader for PDFs
+   Enter URLs for web content

### Ask questions
+   Type your query in the chat interface
+   View rewritten queries and sources
+   See web search results when relevant

### Manage your session
+   Clear chat history as needed
+   Configure web search domains
+   Monitor processed documents
