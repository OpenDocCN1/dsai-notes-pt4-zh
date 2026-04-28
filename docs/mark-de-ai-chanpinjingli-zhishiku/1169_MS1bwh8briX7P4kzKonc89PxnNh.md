# 📰多智能体AI新闻助手

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/NcCpdstyfoMopTxcL9ockqlXnNh](https://qqs7y1hozd1.feishu.cn/docx/NcCpdstyfoMopTxcL9ockqlXnNh)

## 📰 Multi-agent AI news assistant

This Streamlit application implements a sophisticated news processing pipeline using multiple specialized AI agents to search, synthesize, and summarize news articles. It leverages the Llama 3.2 model via Ollama and DuckDuckGo search to provide comprehensive news analysis.

### Features

+   Multi-agent architecture with specialized roles:

+   News Searcher: Finds recent news articles

+   News Synthesizer: Analyzes and combines information

+   News Summarizer: Creates concise, professional summaries

+   Real-time news search using DuckDuckGo

+   AP/Reuters-style summary generation

+   User-friendly Streamlit interface

### How to get Started?

1.  Clone the GitHub repository

git clone https://github.com/your-username/ai-news-processor.git

cd awesome-llm-apps/ai_agent_tutorials/local_news_agent_openai_swarm

1.  Install the required dependencies:

pip install -r requirements.txt

1.  Pull and Run Llama 3.2 using Ollama:

# Pull the model

ollama pull llama3.2

# Verify installation

ollama list

# Run the model (optional test)

ollama run llama3.2

1.  Create a .env file with your configurations:

OPENAI_BASE_URL=http://localhost:11434/v1

OPENAI_API_KEY=fake-key

1.  Run the Streamlit app

streamlit run news_agent.py

#

## 📰多智能体AI新闻助手

这个Streamlit应用程序使用多个专业AI代理实现了一个复杂的新闻处理管道，用于搜索、整合和总结新闻文章。它通过Ollama利用Llama 3.2模型和DuckDuckGo搜索来提供全面的新闻分析。

### 特性

+   具有专门角色的多智能体架构：

+   新闻搜索器：查找近期新闻文章

+   新闻合成器：分析并整合信息

+   新闻摘要器：生成简洁、专业的摘要

+   使用 DuckDuckGo 进行实时新闻搜索

+   AP/路透社风格的摘要生成

+   用户友好的 Streamlit 界面

### 如何开始？

1.  克隆GitHub仓库

git clone https://github.com/your-username/ai-news-processor.git

cd awesome-llm-apps/ai_agent_tutorials/local_news_agent_openai_swarm

1.  安装所需依赖项：

pip install -r requirements.txt

1.  使用Ollama拉取并运行Llama 3.2：

# 拉取模型

ollama拉取llama3.2

# 验证安装

ollama list

# 运行模型（可选测试）

ollama运行llama3.2

1.  创建一个包含你配置的.env文件：

OPENAI_BASE_URL=http://localhost:11434/v1

OPENAI_API_KEY=fake-key

1.  运行Streamlit应用程序

streamlit run news_agent.py