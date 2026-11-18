
# 🗃️ 具备网络访问功能的AI RAG智能体

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/QlTrd5CBHobWE9xoU7Uc39c7nNc](https://qqs7y1hozd1.feishu.cn/docx/QlTrd5CBHobWE9xoU7Uc39c7nNc)

## 🗃️ AI RAG Agent with Web Access

This script demonstrates how to build a Retrieval-Augmented Generation (RAG) agent with web access using GPT-4o in just 15 lines of Python code. The agent uses a PDF knowledge base and has the ability to search the web using DuckDuckGo.

### Features

+   Creates a RAG agent using GPT-4o

+   Incorporates a PDF-based knowledge base

+   Uses LanceDB as the vector database for efficient similarity search

+   Includes web search capability through DuckDuckGo

+   Provides a playground interface for easy interaction

### How to get Started?

1.  Clone the GitHub repository

    `git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git`
    `cd awesome-llm-apps/rag_tutorials/agentic_rag`

1.  Install the required dependencies:

    `pip install -r requirements.txt`

1.  Get your OpenAI API Key

    +   Sign up for an [OpenAI account](https://platform.openai.com/) (or the LLM provider of your choice) and obtain your API key.

    +   Set your OpenAI API key as an environment variable:

        `export OPENAI_API_KEY='your-api-key-here'`

1.  Run the AI RAG Agent

    `python3 rag_agent.py`

1.  Open your web browser and navigate to the URL provided in the console output to interact with the RAG agent through the playground interface.

### How it works?

1.  Knowledge Base Creation: The script creates a knowledge base from a PDF file hosted online.

1.  Vector Database Setup: LanceDB is used as the vector database for efficient similarity search within the knowledge base.

1.  Agent Configuration: An AI agent is created using GPT-4o as the underlying model, with the PDF knowledge base and DuckDuckGo search tool.

1.  Playground Setup: A playground interface is set up for easy interaction with the RAG agent.

## 🗃️ 具备网络访问功能的AI RAG智能体

本脚本展示了如何仅用 15 行 Python 代码，使用 GPT-4o 构建一个具备网络访问能力的检索增强生成（RAG）智能体。该智能体使用 PDF 知识库，并能够使用 DuckDuckGo 进行网络搜索。

### 特性

+   使用 GPT-4o 创建一个 RAG 代理

+   集成了基于 PDF 的知识库

+   使用 LanceDB 作为向量数据库，以实现高效的相似性搜索

+   通过 DuckDuckGo 提供网络搜索功能

+   提供一个易于交互的实验场界面

### 如何开始？

1.  克隆 GitHub 仓库

    `git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git`
    `cd awesome-llm-apps/rag_tutorials/agentic_rag`

1.  安装所需依赖项：

    `pip install -r requirements.txt`

1.  获取你的 OpenAI API 密钥

    +   注册一个 [OpenAI 账户](https://platform.openai.com/)（或你选择的大语言模型提供商账户）并获取你的 API 密钥。

    +   将您的 OpenAI API 密钥设置为环境变量：

        `export OPENAI_API_KEY='your-api-key-here'`

1.  运行 AI RAG 代理

    `python3 rag_agent.py`

1.  打开你的网络浏览器，导航到控制台输出中提供的 URL，通过游乐场界面与 RAG 代理进行交互。

### 它是如何工作的？

1.  知识库创建：该脚本从在线托管的 PDF 文件创建知识库。

1.  向量数据库设置：LanceDB 用作向量数据库，以便在知识库中进行高效的相似性搜索。

1.  智能体配置：使用 GPT-4o 作为基础模型，结合 PDF 知识库和 DuckDuckGo 搜索工具创建一个 AI 智能体。

1.  游乐场设置：搭建了一个游乐场界面，便于与 RAG 代理进行交互。
