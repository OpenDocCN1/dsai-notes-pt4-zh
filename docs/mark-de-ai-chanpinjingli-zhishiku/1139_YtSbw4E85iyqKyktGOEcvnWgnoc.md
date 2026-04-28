# 📰 ➡️ 🎙️ 博客转播客Agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/N46rdKT6zoUASFx1zyrcq5nlnMb](https://qqs7y1hozd1.feishu.cn/docx/N46rdKT6zoUASFx1zyrcq5nlnMb)

## 📰 ➡️ 🎙️ 博客转播客代理

> **这是一个基于Streamlit的应用程**序，它允许用户将任何博客文章转换为播客。

> 该应用使用OpenAI的GPT-4模型进行总结，使用Firecrawl抓取博客内容，并使用ElevenLabs API生成音频。用户只需输入博客URL。

[Code](https://qqs7y1hozd1.feishu.cn/wiki/HClRwLGiUi6pGXkiDr0cvzEQnXe)

[requirements.txt](https://qqs7y1hozd1.feishu.cn/wiki/R95dwZPIBiJjGWkqfe4coRKknZe)

![](img/59ddd3dfde54621cf5e40cbadd83361c.png)

[Code (7).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OLB5brjs0ot1Y6xVjWBcaqWpn9c)

![](img/72c7000e32c45fb3477b24aceb71f6c3.png)

[requirements.txt (7).docx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VYHhbU1dsoHrplxEFfScXKaSn3e)

## 功能

+   **博客抓取：**使用Firecrawl API抓取任何公开博客URL的完整内容。

+   **摘要生成：**使用OpenAI GPT-4创建引人入胜且简洁的博客摘要（2000字符以内）。

+   **播客生成：**使用ElevenLabs语音API将摘要转换为音频播客。

+   **API密钥集成：**需要OpenAI、Firecrawl和ElevenLabs的API密钥运行，可通过侧边栏安全输入。

## 设置

### 要求

##### API密钥：

+   OpenAI API密钥：在OpenAI注册以获取您的API密钥。

+   ElevenLabs API密钥：从ElevenLabs获取您的ElevenLabs API密钥。

+   Firecrawl API密钥：从Firecrawl获取您的Firecrawl API密钥。

Python 3.8及以上版本：请确保已安装 Python 3.8 或更高版本。

### 安装

##### 克隆此仓库：

**git clone **[**仓库**](https://github.com/Shubhamsaboo/awesome-llm-apps)

cd ai_agent_tutorials/ai_blog_to_podcast_agent

##### 安装所需的Python包：

pip install -r requirements.txt

### 运行应用程序

##### 启动 Streamlit 应用程序：

**streamlit run blog**_to_podcast_agent.py

##### 在应用程序界面中：

+   在侧边栏中输入您的OpenAI、ElevenLabs和Firecrawl API密钥。

+   输入你想要转换的博客URL。

+   点击“🎙️生成播客”。

+   收听生成的播客或下载它。

## 📰 ➡️ 🎙️ Blog to Podcast Agent

This is a Streamlit-based application that allows users to convert any blog post into a podcast. The app uses OpenAI's GPT-4 model for summarization, Firecrawl for scraping blog content, and ElevenLabs API for generating audio. Users simply input a blog URL, and the app will generate a podcast episode based on the blog.

## Features

+   Blog Scraping: Scrapes the full content of any public blog URL using Firecrawl API.

+   Summary Generation: Creates an engaging and concise summary of the blog (within 2000 characters) using OpenAI GPT-4.

+   Podcast Generation: Converts the summary into an audio podcast using the ElevenLabs voice API.

+   API Key Integration: Requires OpenAI, Firecrawl, and ElevenLabs API keys to function, entered securely via the sidebar.

## Setup

### Requirements

1.  API Keys:

+   OpenAI API Key: Sign up at OpenAI to obtain your API key.

+   ElevenLabs API Key: Get your ElevenLabs API key from ElevenLabs.

+   Firecrawl API Key: Get your Firecrawl API key from Firecrawl.

1.  Python 3.8+: Ensure you have Python 3.8 or higher installed.

### Installation

1.  Clone this repository:

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps
cd ai_agent_tutorials/ai_blog_to_podcast_agent

1.  Install the required Python packages:

1.  pip install -r requirements.txt

### Running the App

1.  Start the Streamlit app:

1.  streamlit run blog_to_podcast_agent.py

1.  In the app interface:

+   Enter your OpenAI, ElevenLabs, and Firecrawl API keys in the sidebar.

+   Input the blog URL you want to convert.

+   Click "🎙️ Generate Podcast".

+   Listen to the generated podcast or download it.