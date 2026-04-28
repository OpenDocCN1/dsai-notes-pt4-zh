# 🌐浏览器MCP代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/VrVgdHkIfoN17PxhPqac7RtUn4d](https://qqs7y1hozd1.feishu.cn/docx/VrVgdHkIfoN17PxhPqac7RtUn4d)

# 🌐 Browser MCP Agent

A Streamlit application that allows you to browse and interact with websit[es using ](https://github.com/lastmile-ai/mcp-agent)natural language commands through the Model Context Protocol (MCP) and MCP-Agent with Playwright integration.

## Features

+   Natural Language Interface: Control a browser with simple English commands

+   Full Browser Navigation: Visit websites and navigate through pages

+   Interactive Elements: Click buttons, fill forms, and scroll through content

+   Visual Feedback: Take screenshots of webpage elements

+   Information Extraction: Extract and summarize content from webpages

+   Multi-step Tasks: Complete complex browsing sequences through conversation

## Setup

### Requirements

+   Python 3.8+

+   Node.js and npm (for Playwright)

+   This is a critical requirement! The app uses Playwright to control a headless browser

+   [Download a](https://nodejs.org/)nd install from nodejs.org

+   OpenAI or Anthropic API Key

### Installation

1.  Clone this repository:

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd mcp_ai_agents/browser_mcp_agent

1.  Install the required Python packages:

1.  pip install -r requirements.txt

1.  Verify Node.js and npm are installed:

1.  node --version
npm --version

1.  Both commands should return version numbers. If they don't, please install Node.js.

1.  Set up your API keys:

+   Set OpenAI API Key as an environment variable:

+   export OPENAI_API_KEY=your-openai-api-key

### Running the App

1.  Start the Streamlit app:

1.  streamlit run main.py

1.  In the app interface:

+   Enter your browsing command

+   Click "Run Command"

+   View the results and screenshots

### Example Commands

#### Basic Navigation

+   "Go to www.mcp-agent.com"

+   "Go back to the previous page"

#### Interaction

+   "Click on the login button"

+   "Scroll down to see more content"

#### Content Extraction

+   "Summarize the main content of this page"

+   "Extract the navigation menu items"

+   "Take a screenshot of the hero section"

#### Multi-step Tasks

+   "Go to the blog, find the most recent article, and summarize its key points"

## Architecture

The application uses:

+   Streamlit for the user interface

+   MCP (Model Context Protocol) to connect the LLM with tools

+   Playwright for browser automation

+   [MCP-Agent](https://github.com/lastmile-ai/mcp-agent/) for the Agentic Framework

+   OpenAI's models to interpret commands and generate responses

#

# 🌐浏览器MCP代理

[一款Streaml](https://github.com/lastmile-ai/mcp-agent)it应用程序，可让您通过模型上下文协议（MCP）和MCP-Agent与Playwright集成，使用自然语言命令浏览网站并与之交互。

## 特性

+   自然语言界面：用简单的英语命令控制浏览器

+   全浏览器导航：访问网站并浏览页面

+   交互元素：点击按钮、填写表单并滚动浏览内容

+   视觉反馈：截取网页元素的屏幕截图

+   信息提取：从网页中提取并总结内容

+   多步骤任务：通过对话完成复杂的浏览序列

## 设置

### 要求

+   Python 3.8+

+   Node.js 和 npm（用于 Playwright）

+   这是一项关键要求！该应用使用 Playwright 来控制无头浏览器

+   下载并安装[nodejs.org](https://nodejs.org/)

+   OpenAI或Anthropic API密钥

### 安装

1.  克隆此仓库：

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd mcp_ai_agents/browser_mcp_agent

1.  安装所需的 Python 包：

1.  pip install -r requirements.txt

1.  验证 Node.js 和 npm 是否已安装：

1.  node --version
npm --version

1.  这两个命令都应该返回版本号。如果没有返回，请安装 Node.js。

1.  设置您的 API 密钥：

+   将OpenAI API密钥设置为环境变量：

+   export OPENAI_API_KEY=your-openai-api-key

### 运行应用程序

1.  启动 Streamlit 应用程序：

1.  streamlit run main.py

1.  在应用程序界面中：

+   输入您的浏览命令

+   点击“运行命令”

+   查看结果和截图

### 示例命令

#### 基本导航

+   访问www.mcp-agent.com

+   返回上一页

#### 交互

+   "点击登录按钮"

+   向下滚动查看更多内容

#### 内容提取

+   "总结本页面的主要内容"

+   "提取导航菜单项"

+   截取英雄区域的屏幕截图

#### 多步骤任务

+   访问博客，找到最新文章，并总结其要点

## 架构

该应用程序使用：

+   用于用户交互界面的 Streamlit

+   MCP（模型上下文协议）用于将大语言模型与工具连接起来

+   用于浏览器自动化的Playwright

+   [MCP-Agent ](https://github.com/lastmile-ai/mcp-agent/) 用于智能体框架

+   OpenAI的模型用于解释命令并生成响应