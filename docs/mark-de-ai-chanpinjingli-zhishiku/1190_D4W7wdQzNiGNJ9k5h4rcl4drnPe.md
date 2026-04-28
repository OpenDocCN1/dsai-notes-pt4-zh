# 🎙️ 使用OpenAI SDK的语音RAG

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/H0FxdQYdyoERVJxVTydcg1psnKh](https://qqs7y1hozd1.feishu.cn/docx/H0FxdQYdyoERVJxVTydcg1psnKh)

## 🎙️ Voice RAG with OpenAI SDK

This script demonstrates how to build a voice-enabled Retrieval-Augmented Generation (RAG) system using OpenAI's SDK and Streamlit. The application allows users to upload PDF documents, ask questions, and receive both text and voice responses using OpenAI's text-to-speech capabilities.

### Features

+   Creates a voice-enabled RAG system using OpenAI's SDK

+   Supports PDF document processing and chunking

+   Uses Qdrant as the vector database for efficient similarity search

+   Implements real-time text-to-speech with multiple voice options

+   Provides a user-friendly Streamlit interface

+   Allows downloading of generated audio responses

+   Supports multiple document uploads and tracking

### How to get Started?

1.  Clone the GitHub repository

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd awesome-llm-apps/rag_tutorials/voice_rag_openaisdk

1.  Install the required dependencies:

pip install -r requirements.txt

1.  Set up your API keys:

+   Get your [OpenAI API key](https://platform.openai.com/)

+   Set up a [Qdrant Cloud](https://cloud.qdrant.io/) account and get your API key and URL

+   Create a .env file with your credentials:

OPENAI_API_KEY='your-openai-api-key'

QDRANT_URL='your-qdrant-url'

QDRANT_API_KEY='your-qdrant-api-key'

1.  Run the Voice RAG application:

streamlit run rag_voice.py

1.  Open your web browser and navigate to the URL provided in the console output to interact with the Voice RAG system.

### How it works?

1.  Document Processing:

+   Upload PDF documents through the Streamlit interface

+   Documents are split into chunks using LangChain's RecursiveCharacterTextSplitter

+   Each chunk is embedded using FastEmbed and stored in Qdrant

1.  Query Processing:

+   User questions are converted to embeddings

+   Similar documents are retrieved from Qdrant

+   A processing agent generates a clear, spoken-word friendly response

+   A TTS agent optimizes the response for speech synthesis

1.  Voice Generation:

+   Text responses are converted to speech using OpenAI's TTS

+   Users can choose from multiple voice options

+   Audio can be played directly or downloaded as MP3

1.  Features:

+   Real-time audio streaming

+   Multiple voice personality options

+   Document source tracking

+   Download capability for audio responses

+   Progress tracking for document processing

## 🎙️ 使用OpenAI SDK的语音RAG

本脚本展示了如何使用OpenAI的SDK和Streamlit构建一个支持语音的检索增强生成（RAG）系统。该应用程序允许用户上传PDF文档、提问，并使用OpenAI的语音合成功能接收文本和语音响应。

### 功能

+   使用OpenAI的SDK创建一个支持语音的RAG系统

+   支持PDF文档处理和分块

+   使用Qdrant作为向量数据库，以进行高效的相似性搜索

+   实现具有多种语音选项的实时语音合成

+   提供用户友好的Streamlit界面

+   允许下载生成的音频回复

+   支持多文档上传和跟踪

### 如何开始？

1.  克隆GitHub仓库

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd awesome-llm-apps/rag_tutorials/voice_rag_openaisdk

1.  安装所需的依赖项：

pip install -r requirements.txt

1.  设置您的 API 密钥：

+   获取您的[OpenAI API密钥](https://platform.openai.com/)

+   创建一个[Qdrant Cloud](https://cloud.qdrant.io/)账户，并获取你的API密钥和URL

+   创建一个.env文件，并在其中添加你的凭证：

OPENAI_API_KEY='你的OpenAI API密钥'

QDRANT_URL='你的Qdrant URL'

QDRANT_API_KEY='your-qdrant-api-key'

1.  运行语音RAG应用程序：

streamlit run rag_voice.py

1.  打开你的网络浏览器，导航到控制台输出中提供的 URL，与语音 RAG 系统进行交互。

### 它是如何工作的？

1.  文档处理：

+   通过Streamlit界面上传PDF文档

+   使用LangChain的递归字符文本分割器将文档分割成多个块

+   每个数据块都使用FastEmbed进行嵌入，并存储在Qdrant中

1.  查询处理：

+   用户问题被转换为嵌入向量

+   从Qdrant中检索相似文档

+   处理代理生成清晰、适合口语表达的响应

+   TTS代理优化语音合成的响应

1.  语音生成：

+   文本回复使用OpenAI的TTS转换为语音

+   用户可以从多种语音选项中进行选择

+   音频可直接播放或下载为MP3格式

1.  特点：

+   实时音频流

+   多种语音个性选项

+   文档来源跟踪

+   音频回复的下载功能

+   文档处理进度跟踪