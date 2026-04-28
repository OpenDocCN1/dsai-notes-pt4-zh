# 🦙 使用Llama 3.2的本地RAG代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/HRBDdi1sbonL6PxUkdscPbsMn3d](https://qqs7y1hozd1.feishu.cn/docx/HRBDdi1sbonL6PxUkdscPbsMn3d)

## 🦙 Local RAG Agent with Llama 3.2

This application implements a Retrieval-Augmented Generation (RAG) system using Llama 3.2 via Ollama, with Qdrant as the vector database.

### Features

+   Fully local RAG implementation

+   Powered by Llama 3.2 through Ollama

+   Vector search using Qdrant

+   Interactive playground interface

+   No external API dependencies

### How to get Started?

1.  Clone the GitHub repository

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

1.  Install the required dependencies:

cd awesome-llm-apps/rag_tutorials/local_rag_agent

pip install -r requirements.txt

1.  [Instal](https://qdrant.tech/)l and start Qdrant vector database locally

docker pull qdrant/qdrant

docker run -p 6333:6333 qdrant/qdrant

1.  Install [Ollama](https://ollama.com/download) and pull Llama 3.2 for LLM and OpenHermes as the embedder for OllamaEmbedder

ollama pull llama3.2

ollama pull openhermes

1.  Run the AI RAG Agent

python local_rag_agent.py

1.  Open your web browser and navigate to the URL provided in the console output to interact with the RAG agent through the playground interface.

#

## 🦙 使用Llama 3.2的本地RAG代理

本应用程序通过Ollama使用Llama 3.2实现了一个检索增强生成（RAG）系统，并使用Qdrant作为向量数据库。

### 特性

+   完全本地化的RAG实现

+   由Llama 3.2通过Ollama提供支持

+   使用Qdrant进行向量搜索

+   交互式游乐场界面

+   无外部 API 依赖

### 如何开始？

1.  克隆GitHub仓库

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

1.  安装所需的依赖项：

cd awesome-llm-apps/rag_tutorials/local_rag_agent

pip install -r requirements.txt

1.  在本地安装并启动[Qdrant](https://qdrant.tech/)向量数据库

docker pull qdrant/qdrant

docker run -p 6333:6333 qdrant/qdrant

1.  安装[Ollama](https://ollama.com/download)，并拉取Llama 3.2作为大语言模型（LLM），拉取OpenHermes作为OllamaEmbedder的嵌入器

ollama拉取llama3.2

ollama拉取openhermes

1.  运行AI RAG代理

python local_rag_agent.py

1.  打开你的网络浏览器，导航到控制台输出中提供的 URL，通过 playground 界面与 RAG 代理进行交互。