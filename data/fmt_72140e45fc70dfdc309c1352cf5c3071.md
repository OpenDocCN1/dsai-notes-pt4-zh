
# 🐙 GitHub MCP Agent / 🐙 GitHub MCP 代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/YbNods3DRodCsRxlMfYcUNornqb](https://qqs7y1hozd1.feishu.cn/docx/YbNods3DRodCsRxlMfYcUNornqb)

## 🐙 GitHub MCP Agent

A Streamlit application that allows you to explore and analyze GitHub repositories using natural language queries through the Model Context Protocol (MCP).

### Features

- Natural Language Interface: Ask questions about repositories in plain English
- Comprehensive Analysis: Explore issues, pull requests, repository activity, and code statistics
- Interactive UI: User-friendly interface with example queries and custom input
- MCP Integration: Leverages the Model Context Protocol to interact with GitHub's API
- Real-time Results: Get immediate insights on repository activity and health

### Setup

#### Requirements

- Python 3.8+
- Node.js and `npm` (for MCP GitHub server)
- This is a critical requirement! The app uses `npx` to run the MCP GitHub server
- Download and install from [nodejs.org](https://nodejs.org/)
- GitHub Personal Access Token with appropriate permissions
- OpenAI API Key

#### Installation

1. Clone this repository:

   `git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git`

   `cd mcp-github-agent`

1. Install the required Python packages:

   `pip install -r requirements.txt`

1. Verify Node.js and npm are installed:

   ```
   node --version
   npm --version
   npx --version
   ```

   All of these commands should return version numbers. If they don't, please install Node.js.

1. Set up your API keys:

   - Set OpenAI API Key as an environment variable:

     `export OPENAI_API_KEY=your-openai-api-key`

   - GitHub token will be entered directly in the app interface

1. Create a GitHub Personal Access Token:

   - Visit `https://github.com/settings/tokens`
   - Create a new token with repo and user scopes
   - Save the token somewhere secure

#### Running the App

1. Start the Streamlit app:

   `streamlit run app.py`

1. In the app interface:

   - Enter your GitHub token in the sidebar
   - Specify a repository to analyze
   - Select a query type or write your own
   - Click "Run Query"

#### Example Queries

##### Issues

- "Show me issues by label"
- "What issues are being actively discussed?"
- "Find issues labeled as bugs"

##### Pull Requests

- "What PRs need review?"
- "Show me recent merged PRs"
- "Find PRs with conflicts"

##### Repository

- "Show repository health metrics"
- "Show repository activity patterns"
- "Analyze code quality trends"

## 🐙 GitHub MCP 代理

一款 Streamlit 应用程序，可让您通过模型上下文协议（MCP）使用自然语言查询来探索和分析 GitHub 仓库。

### 功能

- 自然语言界面：用通俗易懂的英语询问有关仓库的问题
- 综合分析：探索问题、拉取请求、仓库活动和代码统计
- 交互式 UI：用户友好的界面，包含示例查询和自定义输入
- MCP 集成：利用模型上下文协议与 GitHub 的 API 进行交互
- 实时结果：即时洞察仓库活动和健康状况

### 设置

#### 要求

- Python 3.8+
- Node.js 和 `npm`（用于 MCP GitHub 服务器）
- 这是一项关键要求！该应用使用 `npx` 来运行 MCP GitHub 服务器
- 下载并安装 [nodejs.org](https://nodejs.org/)
- 具有适当权限的 GitHub 个人访问令牌
- OpenAI API 密钥

#### 安装

1. 克隆此仓库：

   `git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git`

   `cd mcp-github-agent`

1. 安装所需的 Python 包：

   `pip install -r requirements.txt`

1. 验证 Node.js 和 npm 是否已安装：

   ```
   node --version
   npm --version
   npx --version
   ```

   所有这些命令都应该返回版本号。如果没有返回，请安装 Node.js。

1. 设置您的 API 密钥：

   - 将 OpenAI API 密钥设置为环境变量：

     `export OPENAI_API_KEY=your-openai-api-key`

   - GitHub 令牌将直接在应用程序界面中输入

1. 创建一个 GitHub 个人访问令牌：

   - 访问 `https://github.com/settings/tokens`
   - 创建一个具有仓库和用户权限范围的新令牌
   - 将令牌保存在安全的地方

### 运行应用程序

1. 启动 Streamlit 应用程序：

   `streamlit run app.py`

1. 在应用界面中：

   - 在侧边栏中输入您的 GitHub 令牌
   - 指定要分析的存储库
   - 选择查询类型或自行编写
   - 点击“运行查询”

### 示例查询

#### 问题

- "按标签显示问题"
- "正在积极讨论哪些问题？"
- "查找标记为错误的问题"

#### 拉取请求

- "哪些 PR 需要审核？"
- "给我展示最近合并的拉取请求（PR）"
- "查找有冲突的拉取请求"

#### 仓库

- "显示仓库健康指标"
- "显示仓库活动模式"
- "分析代码质量趋势"
