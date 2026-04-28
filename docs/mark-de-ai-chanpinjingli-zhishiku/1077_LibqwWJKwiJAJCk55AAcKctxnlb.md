# 🚀多MCP智能助手

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/VjWqdO8nBokeo9xdlkjcklJjnhb](https://qqs7y1hozd1.feishu.cn/docx/VjWqdO8nBokeo9xdlkjcklJjnhb)

# 🚀 Multi-MCP Intelligent Assistant

The Multi-MCP Intelligent Assistant is a powerful productivity tool that integrates multiple Model Context Protocol (MCP) servers to provide seamless access to GitHub, Perplexity, Calendar, and Gmail services through natural language interactions. This advanced AI assistant is powered by Agno's AI Agent framework and designed to be a productivity multiplier across your digital workspace.

## Features

+   Multi-Agent System

+   GitHub Integration: Complete repository management, issue tracking, and code analysis

+   Perplexity Research: Real-time web search and information gathering

+   Calendar Management: Event scheduling and meeting coordination

+   Gmail Integration: Email management and communication workflows

+   Core Capabilities:

+   Repository management (create, clone, fork, search)

+   Issue & PR workflow (create, update, review, merge, comment)

+   Real-time web search and research

+   Event scheduling and availability management

+   Email organization and automated responses

+   Cross-platform workflow automation

+   Advanced Features:

+   Interactive CLI with streaming responses

+   Conversation memory and context retention

+   Tool chaining for complex workflows

+   Session-specific user and session IDs

+   Markdown-formatted responses

+   Proactive workflow suggestions

+   Productivity Focus:

+   Cross-platform automation (GitHub issues → Calendar events)

+   Research-driven development workflows

+   Project management integration

+   Documentation and knowledge sharing

## How to Run

Follow these steps to set up and run the application:

1.  Clone the Repository:

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/mcp_ai_agents/multi_mcp_agent

1.  Install the dependencies:

1.  pip install -r requirements.txt

1.  Verify Node.js installation (required for MCP servers):

1.  node --version
npm --version
npx --version

1.  If Node.js is not installe[d, downloa](https://nodejs.org/)d it from nodejs.org

1.  Set up your API keys: Create a .env file in the project directory with the following variables:

1.  OPENAI_API_KEY=your-openai-api-keyGITHUB_PERSONAL_ACCESS_TOKEN=your-github-tokenPERPLEXITY_API_KEY=your-perplexity-api-key

+   [Get an OpenAI AP](https://platform.openai.com/api-keys)I key from: https://platform.openai.com/api-keys

+   Get a GitHub Personal Access Token from: https://github.com/settings/tokens (with repo, user, and admin:org scopes)

+   Get a Perplexity API key from: https://www.perplexity.ai/

+   Configure OpenAI MCP Headers according to your setup requirements

1.  Run the Multi-MCP Agent:

1.  python multi_mcp_agent.py

1.  Start Interacting:

+   The assistant will validate your environment variables

+   Generate unique user and session IDs

+   Initialize connections to all MCP servers

+   Start the interactive CLI interface

## Usage

1.  Environment Validation: The assistant automatically checks for all required API keys and environment variables

1.  Session Management: Each session gets unique user and session IDs for tracking and context

1.  Interactive Commands: Use natural language to interact with integrated services:

### Example Commands

GitHub Operations:

+   "Show my recent GitHub repositories"

+   "Create a new issue in my project repo"

+   "Search for Python code in my repositories"

+   "Review the latest pull requests"

Research & Information:

+   "Search for the latest AI developments"

+   "What are the trending topics in machine learning?"

+   "Find documentation for FastAPI"

+   "Research best practices for microservices"

Calendar Management:

+   "Schedule a meeting for next week"

+   "Show my upcoming appointments"

+   "Find available time slots for a 2-hour meeting"

Cross-Platform Workflows:

+   "Create a GitHub issue and schedule a follow-up meeting"

+   "Research a topic and create a summary document"

+   "Find trending repositories and add them to my watchlist"

1.  Session Control: Type 'exit', 'quit', or 'bye' to end the session

## Architecture

The Multi-MCP Agent leverages:

+   Agno Framework: For agent orchestration and tool management

+   OpenAI GPT-4o: As the core language model

+   MCP Servers: For external service integrations

+   Async Architecture: For efficient concurrent operations

+   Memory System: For context retention and conversation history

## Note

The assistant connects to multiple MCP servers using Node.js packages. Ensure you have a stable internet connection and valid API keys for all services. The tool chaining capabilities allow for complex workflows that span multiple platforms, making it a powerful productivity multiplier for developers and professionals.

#

# 🚀多MCP智能助手

多MCP智能助手是一款强大的生产力工具，它集成了多个模型上下文协议（MCP）服务器，通过自然语言交互实现对GitHub、Perplexity、日历和Gmail服务的无缝访问。这款先进的AI助手由Agno的AI代理框架提供支持，旨在成为你数字工作空间中的生产力倍增器。

## 特性

+   多智能体系统

+   GitHub集成：完整的仓库管理、问题跟踪和代码分析

+   Perplexity研究：实时网络搜索和信息收集

+   日程管理：活动安排和会议协调

+   Gmail集成：电子邮件管理和通信工作流程

+   核心能力：

+   仓库管理（创建、克隆、复刻、搜索）

+   问题与PR工作流程（创建、更新、审查、合并、评论）

+   实时网络搜索和研究

+   活动日程安排和可用性管理

+   电子邮件整理和自动回复

+   跨平台工作流自动化

+   高级功能：

+   具有流式响应的交互式 CLI

+   对话记忆和上下文保留

+   复杂工作流程的工具链

+   特定会话的用户和会话ID

+   Markdown格式的响应

+   主动式工作流程建议

+   专注生产力：

+   跨平台自动化（GitHub问题→日历事件）

+   以研究为驱动的开发工作流程

+   项目管理整合

+   文档和知识共享

## 运行方法

按照以下步骤设置并运行应用程序：

1.  克隆仓库：

1.  git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/mcp_ai_agents/multi_mcp_agent

1.  安装依赖项：

1.  pip install -r requirements.txt

1.  验证 Node.js 安装（MCP 服务器必需）：

1.  node --version
npm --version
npx --version

1.  [如果未安装Node.](https://nodejs.org/) js，请从以下位置下载nodejs.org

1.  设置您的 API 密钥：在项目目录中创建一个.env文件，并包含以下变量：

1.  OPENAI_API_KEY=你的OpenAI API密钥GITHUB_PERSONAL_ACCESS_TOKEN=你的GitHub个人访问令牌PERPLEXITY_API_KEY=你的Perplexity API密钥

+   [从以下链接获取OpenAI AP](https://platform.openai.com/api-keys)I密钥：https://platform.openai.com/api-keys

+   从以下链接获取GitHub个人访问令牌：https://github.com/settings/tokens（具有repo、user和admin:org权限范围）

+   从以下链接获取Perplexity API密钥：https://www.perplexity.ai/

+   根据您的设置要求配置OpenAI MCP标头

1.  运行多MCP代理：

1.  python multi_mcp_agent.py

1.  开始交互：

+   助手将验证您的环境变量

+   生成唯一的用户和会话 ID

+   初始化与所有MCP服务器的连接

+   启动交互式 CLI 界面

## 用法

1.  环境验证：助手会自动检查所有必需的 API 密钥和环境变量

1.  会话管理：每个会话都有唯一的用户和会话 ID，用于跟踪和上下文管理

1.  交互式命令：使用自然语言与集成服务进行交互：

### 示例命令

GitHub操作：

+   "显示我最近的GitHub仓库"

+   "在我的项目仓库中创建一个新问题"

+   "在我的仓库中搜索Python代码"

+   查看最新的拉取请求

研究与信息：

+   "搜索最新的AI发展动态"

+   "机器学习领域的热门话题有哪些？"

+   "查找FastAPI的留档资料"

+   研究微服务的最佳实践

日历管理：

+   "安排下周的会议"

+   "显示我的即将到来的预约"

+   "查找2小时会议的可用时段"

跨平台工作流程：

+   创建一个GitHub问题并安排后续会议

+   研究一个主题并创建一份总结文档

+   "查找热门仓库并将其添加到我的关注列表中"

1.  会话控制：输入 'exit'、'quit' 或 'bye' 结束会话

## 架构

多MCP代理利用：

+   Agno框架：用于智能体编排和工具管理

+   OpenAI GPT-4o：作为核心语言模型

+   MCP服务器：用于外部服务集成

+   异步架构：实现高效并发操作

+   记忆系统：用于上下文保留和对话历史记录

## 注意

助手使用 Node.js 包连接到多个 MCP 服务器。请确保你有稳定的互联网连接和所有服务的有效 API 密钥。工具链功能支持跨多个平台的复杂工作流程，使其成为开发人员和专业人士提高工作效率的强大工具。