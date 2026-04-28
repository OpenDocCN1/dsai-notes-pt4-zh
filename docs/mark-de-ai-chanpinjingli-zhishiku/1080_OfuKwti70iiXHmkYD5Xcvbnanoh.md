# 📑 Notion MCP代理

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/DfigdvQE6oTdsPxJcOKcgoAjn8d](https://qqs7y1hozd1.feishu.cn/docx/DfigdvQE6oTdsPxJcOKcgoAjn8d)

# 📑 Notion MCP Agent

A terminal-based Notion Agent for interacting with your Notion pages using natural language through the Notion MCP (Model Context Protocol) server.

## Features

+   Interact with Notion pages via a command-line interface

+   Perform update, insert, retrieve operations on your Notion pages

+   Create and edit blocks, lists, tables, and other Notion structures

+   Add comments to blocks

+   Search for specific information

+   Remembers conversation context for multi-turn interactions

+   Session management for persistent conversations

## Prerequisites

+   Python 3.9+

+   A Notion account with admin permissions

+   A Notion Integration token

+   An OpenAI API key

## Installation

1.  Clone the repository

1.  Install the required Python packages:

pip install -r requirements.txt

1.  Install the Notion MCP server (will be done automatically when you run the app)

## Setting Up Notion Integration

### Creating a Notion Integration

1.  Go to Notion Integrations

1.  Click "New integration"

1.  Name your integration (e.g., "Notion Assistant")

1.  Select the capabilities needed (Read & Write content)

1.  Submit and copy your "Internal Integration Token"

### Sharing Your Notion Page with the Integration

1.  Open your Notion page

1.  Click the three dots (⋮) in the top-right corner of the page

1.  Select "Add connections" from the dropdown menu

1.  Search for your integration name in the search box

1.  Click on your integration to add it to the page

1.  Confirm by clicking "Confirm" in the dialog that appears

Alternatively, you can also share via the "Share" button:

1.  Click "Share" in the top right

1.  In the sharing dialog, search for your integration name (preceded by "@")

1.  Click on your integration to add it

1.  Click "Invite" to grant it access to your page

Both methods will grant your integration full access to the page and its content.

### Finding Your Notion Page ID

1.  Open your Notion page in a browser

1.  Copy the URL, which looks like: https://www.notion.so/workspace/Your-Page-1f5b8a8ba283...

1.  The ID is the part after the last dash and before any query parameters Example: 1f5b8a8bad058a7e39a6

## Configuration

You can configure the agent using environment variables:

+   NOTION_API_KEY: Your Notion Integration token

+   OPENAI_API_KEY: Your OpenAI API key

+   NOTION_PAGE_ID: The ID of your Notion page

Alternatively, you can set these values directly in the script.

## Usage

Run the agent from the command line:

python notion_mcp_agent.py

When you start the agent, it will prompt you to enter your Notion page ID. You can:

1.  Enter your page ID at the prompt

1.  Press Enter without typing anything to use the default page ID (if set)

1.  Provide the page ID directly as a command-line argument (bypassing the prompt):

python notion_mcp_agent.py your-page-id-here

### Conversation Flow

Each time you start the agent, it creates a unique user ID and session ID to maintain conversation context. This allows the agent to remember previous interactions and continue coherent conversations even after you close and restart the application.

You can exit the conversation at any time by typing exit, quit, bye, or goodbye.

## Example Queries

+   "What's on my Notion page?"

+   "Add a new paragraph saying 'Meeting notes for today'"

+   "Create a bullet list with three items: Apple, Banana, Orange"

+   "Add a comment to the first paragraph saying 'This looks good!'"

+   "Search for any mentions of meetings"

+   "Summarize our conversation so far"

## License

MIT

# 📑 Notion MCP代理

一个基于终端的Notion代理，可通过Notion MCP（模型上下文协议）服务器使用自然语言与你的Notion页面进行交互。

## 特性

+   通过命令行界面与 Notion 页面进行交互

+   在你的Notion页面上执行更新、插入、检索操作

+   创建和编辑块、列表、表格及其他Notion结构

+   为模块添加注释

+   搜索特定信息

+   记住多轮交互的对话上下文

+   持久对话的会话管理

## 先决条件

+   Python 3.9+

+   具有管理员权限的Notion账户

+   Notion集成令牌

+   一个OpenAI API密钥

## 安装

1.  克隆仓库

1.  安装所需的Python包：

pip install -r requirements.txt

1.  安装 Notion MCP 服务器（运行应用程序时将自动完成）

## 设置 Notion 集成

### 创建Notion集成

1.  转到[Notion集成](https://www.notion.so/my-integrations)

1.  点击“新建集成”

1.  为你的集成命名（例如，“Notion助手”）

1.  选择所需的功能（读写内容）

1.  提交并复制您的“内部集成令牌”

### 通过集成共享您的 Notion 页面

1.  打开你的 Notion 页面

1.  点击页面右上角的三个点（⋮）

1.  从下拉框中选择“添加连接”

1.  在搜索框中搜索您的集成名称

1.  点击您的集成以将其添加到页面

1.  在弹出的对话框中点击“确认”进行确认

或者，您也可以通过“分享”按钮进行分享：

1.  点击右上角的“分享”

1.  在共享对话框中，搜索您的集成名称（前面带有“@”）

1.  点击您的集成以添加它

1.  点击“邀请”以授予其访问您页面的权限

两种方法都将授予您的集成对页面及其内容的完全访问权限。

### 查找您的Notion页面ID

1.  在浏览器中打开你的Notion页面

1.  复制看起来像这样的 URL：https://www.notion.so/workspace/Your-Page-1f5b8a8ba283...

1.  ID是最后一个破折号之后、任何查询参数之前的部分。示例：1f5b8a8bad058a7e39a6

## 配置

您可以使用环境变量来配置代理：

+   NOTION_API_KEY：你的 Notion 集成令牌

+   OPENAI_API_KEY：您的OpenAI API密钥

+   NOTION_PAGE_ID: 你的Notion页面的ID

或者，您可以直接在脚本中设置这些值。

## 用法

从命令行运行代理：

python notion_mcp_agent.py

当你启动代理时，它会提示你输入你的Notion页面ID。你可以：

1.  在提示处输入您的页面 ID

1.  不输入任何内容直接按回车键以使用默认页面 ID（如果已设置）

1.  直接将页面 ID 作为命令行参数提供（绕过提示）：

python notion_mcp_agent.py 你的页面ID

### 对话流程

每次启动代理时，它都会创建一个唯一的用户ID和会话ID，以维护对话上下文。这使得代理能够记住之前的交互，并在你关闭并重新启动应用程序后仍能继续连贯的对话。

您可以随时通过输入exit、quit、bye或goodbye退出对话。

## 示例查询

+   "我的Notion页面上有什么？"

+   添加一个新段落，内容为“今天的会议记录”

+   "创建一个包含三个项目的项目列表：苹果、香蕉、橙子"

+   在第一段添加一条评论，内容为“这看起来不错！”

+   "搜索所有提及会议的内容"

+   "总结一下我们目前的对话内容"

## 许可证

麻省理工学院