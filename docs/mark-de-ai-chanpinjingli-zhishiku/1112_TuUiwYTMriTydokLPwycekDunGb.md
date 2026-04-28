# 💻 本地Lllama-3.1与RAG

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/GxAudhBjDog6S2xduHOczhVonxc](https://qqs7y1hozd1.feishu.cn/docx/GxAudhBjDog6S2xduHOczhVonxc)

## 💻 Local Lllama-3.1 with RAG

Streamlit app that allows you to chat with any webpage using local Llama-3.1 and Retrieval Augmented Generation (RAG). This app runs entirely on your computer, making it 100% free and without the need for an internet connection.

### Features

+   Input a webpage URL

+   Ask questions about the content of the webpage

+   Get accurate answers using RAG and the Llama-3.1 model running locally on your computer

### How to get Started?

1.  Clone the GitHub repository

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd awesome-llm-apps/rag_tutorials/llama3.1_local_rag

1.  Install the required dependencies:

pip install -r requirements.txt

1.  Run the Streamlit App

streamlit run llama3.1_local_rag.py

### How it Works?

+   The app loads the webpage data using WebBaseLoader and splits it into chunks using RecursiveCharacterTextSplitter.

+   It creates Ollama embeddings and a vector store using Chroma.

+   The app sets up a RAG (Retrieval-Augmented Generation) chain, which retrieves relevant documents based on the user's question.

+   The Llama-3.1 model is called to generate an answer using the retrieved context.

+   The app displays the answer to the user's question.

#

## 💻 本地Lllama-3.1与RAG

Streamlit应用程序，可让您使用本地Llama-3.1和检索增强生成（RAG）与任何网页进行聊天。此应用程序完全在您的计算机上运行，100%免费且无需联网。

### 功能

+   输入网页URL

+   询问有关网页内容的问题

+   使用RAG和在本地计算机上运行的Llama-3.1模型获取准确答案

### 如何开始？

1.  克隆GitHub仓库

git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git

cd awesome-llm-apps/rag_tutorials/llama3.1_local_rag

1.  安装所需依赖项：

pip install -r requirements.txt

1.  运行Streamlit应用程序

streamlit run llama3.1_local_rag.py

### 它是如何工作的？

+   应用程序使用WebBaseLoader加载网页数据，并使用RecursiveCharacterTextSplitter将其拆分为多个文本块。

+   它使用Chroma创建Ollama嵌入和向量存储。

+   该应用程序设置了一个RAG（检索增强生成）链，该链根据用户的问题检索相关文档。

+   调用Llama-3.1模型，利用检索到的上下文生成答案。

+   应用程序向用户显示问题的答案。