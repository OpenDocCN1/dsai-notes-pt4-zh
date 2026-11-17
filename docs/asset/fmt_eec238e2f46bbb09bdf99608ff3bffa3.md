
# Part 3.谷歌开源Agent框架ADK接入MCP实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/F8tQdGbZUo7lgDxsBYYcMwT3neU](https://kq4b3vgg5b.feishu.cn/docx/F8tQdGbZUo7lgDxsBYYcMwT3neU)

## 一、模型上下文协议（MCP）基础概念回顾

### 1. 谷歌ADK与MCP兼容情况

模型上下文协议 (`MCP`) 是一项开放标准，旨在规范 Gemini 和 Claude 等大型语言模型 (`LLM`) 与外部应用程序、数据源和工具的通信方式。`MCP` 可以理解为一种通用的连接机制，简化了 `LLM` 获取上下文、执行操作以及与各种系统交互的方式。

`MCP` 遵循客户端-服务器架构，定义数据（`资源`）、交互式模板（`提示`）和可操作功能（`工具`）如何由 `MCP` 服务器公开并由 `MCP` 客户端（可以是 `LLM` 主机应用程序或 AI 代理）使用。

而对于新一代 `Agent` 框架来说，都从底层代码功能方面支持 `MCP` 工具调用，而对于 `ADK` 来说更是允许以工具形式接入 `MCP`，同时也允许将 `Agent` 包装为一个 `MCP` server：

1. 在 `ADK` 中使用现有的 `MCP` 服务器： `ADK` 代理充当 `MCP` 客户端，利用外部 `MCP` 服务器提供的工具。
2. 通过 `MCP` 服务器公开 `ADK` 工具：构建一个包装 `ADK` 工具的 `MCP` 服务器，使任何 `MCP` 客户端都可以访问它们。

### 2. MCP系列公开课合集

在此前我已经开设过一些列公开课来介绍 `MCP` 工具，相关基础内容大家可以参考如下公开课：

![](img/1fa4101320234cd86685d80649f3102b.png)

+ `MCP`技术开发入门：https://www.bilibili.com/video/BV1NLXCYTEbj/
+ `MCP`企业级智能体开发实战：https://www.bilibili.com/video/BV1n1ZuYjEzf/
+ 不写一行代码，零门槛接入 `MCP`：https://www.bilibili.com/video/BV1dCo7YdEgK/
+ 7分钟开发&发布专属 `MCP`！：https://www.bilibili.com/video/BV1VHL6zsE5F/

### 3. ADK接入MCP基本环境要求

在正式编写代码前，请确保已进行以下设置：

+ 完成 `ADK` 基础设置：已完成 `ADK` 和 `LiteLlm` 的安装。
+ 安装/更新 `Python`： `MCP` 需要 `Python` 3.9 或更高版本。
+ 安装 `Node.js` 和 `npx`：许多社区 `MCP` 服务器以 `Node.js` 软件包的形式分发，并使用 运行 `npx`。如果您尚未安装 `Node.js`（包含 `npx`），请先安装。详情请参阅https://nodejs.org/en。

![](img/1b0c53b322858612c71afdb490fcf3cb.png)

+ 安装 `pypi`：除了 `npm` 外，很多用 `Python` 编写的 `MCP` 库也托管在 `pypi` 上，并使用 `uvx` 进行安装，因此建议也提前安装好 `pypi` 库；

![](img/f4900b97c175858420fd75deacf127f5.png)

可以使用如下命令进行验证：

```
npx -v
node -v
```

![](img/ecd9a3a13610474b311e4dd595e13485.png)

### 4. 公开课代码领取

![](img/0c9dad14065e92208be28973c55347e1.png)

## 二、使用 MCP 服务器和 ADK 代理（ADK 作为 MCP 客户端）

### 1. 基础概念介绍

`MCP` 技术体系中，会将外部工具运行脚本称作服务器，而接入这些外部工具的大模型运行环境称作客户端。

![](img/b5f69df1bd53e8a19842218148d43499.png)

一个客户端可以接入多个不同类型的服务器的，但要求是都可以遵循 `MCP` 通信协议。简单理解就是 `MCP` 服务器的输出内容是一种标准格式的内容，只能被 `MCP` 客户端所识别。在客户端和服务器都遵循 `MCP` 协议的时候，客户端就能够像 `Function calling` 中大模型调用外部工具一样，调用 `MCP` 服务器里面的工具。

![](img/9fe0bd5d6973a652327c8936e090b930.png)

而本节将率先介绍如何将 `ADK` 作为客户端，接入 `MCP` 工具。其中核心会用到 `MCPToolset` 类，该类是 `ADK` 连接到 `MCP` 服务器的桥梁。实际执行过程中我们的 `ADK` 代理将执行以下操作 `MCPToolset`：

1. 连接：与 `MCP` 服务器进程建立连接。该服务器可以是通过标准输入/输出 (`StdioServerParameters`) 进行通信的本地服务器，也可以是使用服务器发送事件 (`SseServerParams`) 的远程服务器。
2. 发现：查询 `MCP` 服务器以获取可用工具（`list_tools` 方法）。
3. 适应：将 `MCP` 工具模式转换为 `ADK` 兼容 `BaseTool` 实例。
4. 公开：将这些适配的工具呈现给 `ADK Agent`。
5. 代理调用：当 `Agent` 决定使用其中一个工具时，`MCPToolset` 将调用（`call_tool` 方法）转发到 `MCP` 服务器并返回结果。
6. 管理连接：处理与 `MCP` 服务器进程的连接的生命周期，通常需要明确清理的周期（如进程结束后清理）。

### 2. Filesystem MCP 服务器基础功能介绍

`Filesystem` 服务器是一个最基础同时也是最常用的 `MCP` 服务器，同时也是官方推荐的服务器，服务器项目地址：https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem

![](img/ebb80a340e319090dc06174d27811da9.png)

借助 `Filesystem`，我们可以高效便捷操作本地文件夹。

同时 `Filesystem` 也是一个 `js` 项目，源码托管在 `npm` 平台上：（https://www.npmjs.com/package/@modelcontextprotocol/server-filesystem）

![](img/f689818b565babcf763f6c1afefbcb93.png)

而如果是使用 `cursor` 或者 `Cherry studio` 接入该 `MCP`，可以直接使用如下配置：

```
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/path/to/other/allowed/dir"
      ]
    }
  }
}
```

即使用 `npx` 进行安装，并且需要在参数位上输入指定文件夹路径。例如在 `Cherry studio` 中，我们可以这样使用 `Filesystem`：

![](img/455b697c105662528742c873f4f2f3db.png)

实际使用效果如下所示：

![](img/894274d2490586b90849756e5bb9c42a.png)

### 3. 将 Filesystem MCP 接入谷歌 ADK

接下来尝试在代码环境中，使用 `ADK` 调用 `Filesystem MCP`，这里我们还是选择在 `Linux` 环境下使用 `uv` 工具进行库的管理

```
pip install uv
```

> 此外，也可以直接使用 `curl` 安装 `uv`

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

然后按照如下流程创建项目主目录：

```
# cd /root/autodl-tmp/MCP

# 创建项目目录
uv init temp-test-mcp
cd temp-test-mcp
```

![](img/0fb8127bf942b5e633377e001e61b7b3.png)

然后输入如下命令创建虚拟环境：

```
# 创建虚拟环境
uv venv

# 激活虚拟环境
source .venv/bin/activate
```

同时再创建一个 `.env` 文件用于保存关键变量，此时就构建了一个基础项目结构：

![](img/249a9f95f649c0ccf90e8e5627a6a077.png)

| 文件/文件夹 | 作用 |
| --- | --- |
| `.git/` | Git 版本控制目录 |
| `.venv/` | 虚拟环境 |
| `.gitignore` | Git 忽略规则 |
| `.python-version` | Python 版本声明 |
| `main.py` | 主程序入口 |
| `pyproject.toml` | 项目配置文件 |
| `README.md` | 项目说明文档 |

然后添加项目依赖

```
uv add google-adk litellm==1.67.2
```

![](img/47d52ba6dcc22ffc7dcdf9859265d69d.png)

然后打开 `.env`，写入 deepseek 模型调用请求地址和 API-KEY：

![](img/2acf975384aa9b8370f85e0bd732c458.png)

然后打开 `main.py`，写入主函数内容：

```
# 导入基础库
import asyncio
import os
from dotenv import load_dotenv
from google.genai import types
from google.adk.agents import Agent
from google.adk.models.lite_llm import LiteLlm 
from google.adk.agents.llm_agent import LlmAgent
from google.adk.runners import Runner
from google.adk.sessions import InMemorySessionService
from google.adk.tools.mcp_tool.mcp_toolset import MCPToolset, SseServerParams, StdioServerParameters

DS_API_KEY = os.getenv("DS_API_KEY")
DS_BASE_URL = os.getenv("DS_BASE_URL")

model = LiteLlm(
    model="deepseek/deepseek-chat",  
    api_base=DS_BASE_URL,
    api_key=DS_API_KEY
)


# --- Step 1: 创建导入 MCP 工具函数 ---
async def get_tools_async():
  """Gets tools from the File System MCP Server."""
  print("Attempting to connect to MCP Filesystem server...")
  tools, exit_stack = await MCPToolset.from_server(
      # 这里采用 Studio 通信方式进行调用
      connection_params=StdioServerParameters(
          command='npx', # Command to run the server
          args=["-y",    # Arguments for the command
                "@modelcontextprotocol/server-filesystem",
                # 这里需要指定一个绝对路径
                "/root/autodl-tmp/MCP/temp-test-mcp"],
      )
  )
  print("MCP Toolset created successfully.")
  # MCP 需要维护一个与本地 MCP 服务器的连接。
  # exit_stack 管理这个连接的清理。
  return tools, exit_stack

# --- Step 2: 创建 ADK Agent ---
async def get_agent_async():
  """Creates an ADK Agent equipped with tools from the MCP Server."""
  tools, exit_stack = await get_tools_async()
  print(f"Fetched {len(tools)} tools from MCP server.")
  root_agent = Agent(
      model=model,
      name='filesystem_assistant',
      instruction='Help user interact with the local filesystem using available tools.',
      tools=tools, # 将 MCP 工具加载到 Agent 中
  )
  return root_agent, exit_stack

# --- Step 3: 执行主逻辑 ---
async def async_main():
  # 创建会话管理器
  session_service = InMemorySessionService()

  session = session_service.create_session(
      state={}, app_name='mcp_filesystem_app', user_id='user_fs'
  )

  # 用户输入
  query = "请帮我查找目前文件夹里都有哪些文件？"
  print(f"User Query: '{query}'")
  content = types.Content(role='user', parts=[types.Part(text=query)])

  root_agent, exit_stack = await get_agent_async()

  runner = Runner(
      app_name='mcp_filesystem_app',
      agent=root_agent,
      session_service=session_service,
  )

  print("Running agent...")
  events_async = runner.run_async(
      session_id=session.id, user_id=session.user_id, new_message=content
  )

  async for event in events_async:
    print(f"Event received: {event}")

  # 运行完成后，关闭 MCP 服务器连接
  print("Closing MCP server connection...")
  await exit_stack.aclose()
  print("Cleanup complete.")

if __name__ == '__main__':
  try:
    asyncio.run(async_main())
  except Exception as e:
    print(f"An error occurred: {e}")
```

然后尝试进行调用：

```
uv run main.py
```

并观察输出结果：即 `MCP` 服务器启动（通过 `npx`）、`ADK` 代理事件（包括对 `list_directory` 的 `FunctionCall` 和 `FunctionResponse`），以及基于文件列表的最终代理文本响应。请确保 `exit_stack.aclose()` 在最后运行。

![](img/6b72bdaaf9bbdbec18a3110e8e8d7d3c.png)

### 4. 优化后的信息展示形式

尽管目前可以运行，但整体来看并不美观，接下来对代码形式进行优化，方便在命令行中进行调用和对话：
