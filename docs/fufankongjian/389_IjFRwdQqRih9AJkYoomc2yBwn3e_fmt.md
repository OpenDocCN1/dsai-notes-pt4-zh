
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



# ADK + MCP 文件系统助手实现

## 完整代码

```python
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


# --- Step 1: 创建导入MCP工具函数 ---
async def get_tools_async():
  """Gets tools from the File System MCP Server."""
  print("Attempting to connect to MCP Filesystem server...")
  tools, exit_stack = await MCPToolset.from_server(
      connection_params=StdioServerParameters(
          command='npx',
          args=["-y",
                "@modelcontextprotocol/server-filesystem",
                "/root/autodl-tmp/MCP/temp-test-mcp"],
      )
  )
  print("MCP Toolset created successfully.")
  return tools, exit_stack

# --- Step 2: 创建ADK Agent ---
async def get_agent_async():
  """Creates an ADK Agent equipped with tools from the MCP Server."""
  tools, exit_stack = await get_tools_async()
  print(f"Fetched {len(tools)} tools from MCP server.")
  root_agent = Agent(
      model=model,
      name='filesystem_assistant',
      instruction='Help user interact with the local filesystem using available tools.',
      tools=tools,
  )
  return root_agent, exit_stack

# 多轮对话函数
async def chat_loop(root_agent, exit_stack, runner, user_id, session_id) -> None:
  print("\n🤖 ADK+MCP对话已启动！输入 'quit' 退出。")

  while True:
    query = input("\n你: ").strip()
    if query.lower() == "quit":
      break
    try:
      print(f"\n>>> User Query: {query}")
      content = types.Content(role='user', parts=[types.Part(text=query)])
      final_response_text = "Agent did not produce a final response."

      async for event in runner.run_async(user_id=user_id, session_id=session_id, new_message=content):
        print(f"  [Event] Author: {event.author}, Type: {type(event).__name__}, Final: {event.is_final_response()}, Content: {event.content}")

        if event.is_final_response():
          if event.content and event.content.parts:
            final_response_text = event.content.parts[0].text
          elif event.actions and event.actions.escalate:
            final_response_text = f"Agent escalated: {event.error_message or 'No specific message.'}"
          break

      print(f"<<< Agent Response: {final_response_text}")
    except Exception as e:
      print(f"\n⚠️  调用过程出错: {e}")


# --- Step 3: 执行主逻辑 ---
async def async_main():
  # 进行多轮对话
  root_agent, exit_stack = await get_agent_async()
  # 创建会话管理器
  session_service = InMemorySessionService()

  app_name='mcp_filesystem_app'
  user_id = "user_1"
  session_id = "session_001"

  session = session_service.create_session(
      app_name=app_name,
      user_id=user_id,
      session_id=session_id
  )

  runner = Runner(
      app_name=app_name,
      agent=root_agent,
      session_service=session_service,
  )
  await chat_loop(root_agent, exit_stack, runner, user_id, session_id)

  # 运行完成后，关闭MCP服务器连接
  print("Closing MCP server connection...")
  await exit_stack.aclose()
  print("Cleanup complete.")

if __name__ == '__main__':
  try:
    asyncio.run(async_main())
  except Exception as e:
    print(f"An error occurred: {e}")
```

## 实际对话效果

![](img/ef1beab555c19dd7e9260638b84adcda.png)

此时不仅能进行多轮对话，而且还能顺利打印事件流。具体代码解释如下：

### 导入基础库

```python
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
```

这些库包括：

- `asyncio`：用于处理异步操作，尤其适用于并发的 I/O 操作。
- `os`：用于读取环境变量。
- `dotenv`：加载 `.env` 文件中的环境变量。
- `google.genai` 和 `google.adk`：这些是 Google 的 ADK 库和工具，用于构建和管理 AI 代理。
- `MCPToolset`：用于管理与本地 MCP 服务器的交互。

### 环境变量

```python
DS_API_KEY = os.getenv("DS_API_KEY")
DS_BASE_URL = os.getenv("DS_BASE_URL")
```

通过 `os.getenv` 从环境中获取 API 密钥（`DS_API_KEY`）和基础 URL（`DS_BASE_URL`），这两个是与 DeepSeek 模型 API 交互所需要的。

### 创建模型实例

```python
model = LiteLlm(
    model="deepseek/deepseek-chat",
    api_base=DS_BASE_URL,
    api_key=DS_API_KEY
)
```

`LiteLlm`：创建一个 `LiteLlm` 实例，这个模型将用来生成响应。它是基于 `deepseek-chat` 模型构建的，并且使用上面获取的 API 密钥和基础 URL 进行初始化。

### Step 1: 创建导入 MCP 工具函数

```python
async def get_tools_async():
  """Gets tools from the File System MCP Server."""
  print("Attempting to connect to MCP Filesystem server...")
  tools, exit_stack = await MCPToolset.from_server(
      connection_params=StdioServerParameters(
          command='npx',
          args=["-y", "@modelcontextprotocol/server-filesystem", "/root/autodl-tmp/MCP/temp-test-mcp"],
      )
  )
  print("MCP Toolset created successfully.")
  return tools, exit_stack
```

`get_tools_async`：异步函数，通过 `MCPToolset.from_server` 从本地 MCP 文件系统服务器获取工具。这里使用 `StdioServerParameters` 来调用本地的 MCP 文件系统服务器，并指定路径 `/root/autodl-tmp/MCP/temp-test-mcp`。

`exit_stack` 用于确保连接关闭时进行清理。

### Step 2: 创建 ADK Agent

```python
async def get_agent_async():
  """Creates an ADK Agent equipped with tools from the MCP Server."""
  tools, exit_stack = await get_tools_async()
  print(f"Fetched {len(tools)} tools from MCP server.")
  root_agent = Agent(
      model=model,
      name='filesystem_assistant',
      instruction='Help user interact with the local filesystem using available tools.',
      tools=tools,
  )
  return root_agent, exit_stack
```

`get_agent_async`：创建一个 ADK 代理（`Agent`），并为它加载从 MCP 文件系统服务器获取到的工具。代理的模型是 `deepseek-chat`，并且它的任务是帮助用户与本地文件系统交互。

### 多轮对话函数




### Step 3: 执行主逻辑

```python
async def chat_loop(root_agent, exit_stack, runner, user_id, session_id) -> None:
    print("\n🤖 ADK+MCP对话已启动！输入 'quit' 退出。")

    while True:
        query = input("\n你: ").strip()
        if query.lower() == "quit":
            break
        try:
            print(f"\n>>> User Query: {query}")
            content = types.Content(role='user', parts=[types.Part(text=query)])
            final_response_text = "Agent did not produce a final response."

            async for event in runner.run_async(user_id=user_id, session_id=session_id, new_message=content):
                print(f"  [Event] Author: {event.author}, Type: {type(event).__name__}, Final: {event.is_final_response()}, Content: {event.content}")

                if event.is_final_response():
                    if event.content and event.content.parts:
                        final_response_text = event.content.parts[0].text
                    elif event.actions and event.actions.escalate:
                        final_response_text = f"Agent escalated: {event.error_message or 'No specific message.'}"
                    break

            print(f"<<< Agent Response: {final_response_text}")
        except Exception as e:
            print(f"\n⚠️  调用过程出错: {e}")
```

`chat_loop`：这是一个交互式的对话循环函数，模拟多轮对话。用户输入的查询会被传递给代理，代理生成的响应会被输出。通过 `runner.run_async` 执行代理的逻辑，并迭代事件，直到获得最终的响应。

`is_final_response()`：用于判断当前事件是否为最终响应，一旦获得最终响应就停止继续处理。

```python
async def async_main():
  root_agent, exit_stack = await get_agent_async()
  session_service = InMemorySessionService()

  app_name='mcp_filesystem_app'
  user_id = "user_1"
  session_id = "session_001"

  session = session_service.create_session(
      app_name=app_name,
      user_id=user_id,
      session_id=session_id
  )

  runner = Runner(
      app_name=app_name,
      agent=root_agent,
      session_service=session_service,
  )
  await chat_loop(root_agent, exit_stack, runner, user_id, session_id)

  print("Closing MCP server connection...")
  await exit_stack.aclose()
  print("Cleanup complete.")
```

`async_main`：主逻辑函数，首先通过 `get_agent_async` 获取一个代理，并创建一个会话（`session_service.create_session`）。然后，通过 `Runner` 启动代理并运行多轮对话（`chat_loop`）。

在对话完成后，关闭 MCP 服务器连接并进行清理工作。

## 二、使用 ADK 工具搭建 MCP 服务器（MCP 服务器暴露 ADK）

接下来我们继续尝试将一个ADK Agent封装为MCP服务器，方便其他应用程序使用。接下来我们将使用 `model-context-protocol` 库创建一个标准的 Python MCP 服务器应用程序。并在此服务器中试下你如下功能：

1. 创建一个联网搜索的 ADK 工具；

2. 实现 MCP 服务器的 `@app.list_tools` 处理程序来宣传 ADK 工具，并使用 `adk_to_mcp_tool_type` 将 ADK 工具定义转换为 MCP 模式；

3. 实现 MCP 服务器的 `@app.call_tool` 处理程序以接收来自 MCP 客户端的请求，识别请求是否针对您包装的 ADK 工具，执行 ADK 工具的 `.run_async()` 方法，并将结果格式化为符合 MCP 的响应；

还是类似的，我们采用 `uv` 进行库的管理，我们仍然还在相同的项目文件内进行操作，需要使用如下命令安装必要的虚拟环境：

```
uv add mcp bs4 lxml
```

然后创建 `server.py` 脚本，

![](img/2899050d0f1102a02cfef4f65b807ae9.png)

并写入如下内容：



# server.py 完整代码

```python
# server.py
import asyncio
import json
from dotenv import load_dotenv
import os

# MCP Server Imports
from mcp import types as mcp_types # Use alias to avoid conflict with genai.types
from mcp.server.lowlevel import Server, NotificationOptions
from mcp.server.models import InitializationOptions
import mcp.server.stdio

# ADK Tool Imports
from google.adk.tools.function_tool import FunctionTool
from google.adk.tools.load_web_page import load_web_page # Example ADK tool
# ADK <-> MCP Conversion Utility
from google.adk.tools.mcp_tool.conversion_utils import adk_to_mcp_tool_type

# --- Load Environment Variables (If ADK tools need them) ---
load_dotenv()
# set proxy
os.environ['HTTP_PROXY'] = 'http://127.0.0.1:10080'
os.environ['HTTPS_PROXY'] = 'http://127.0.0.1:10080'

# --- Prepare the ADK Tool ---
# Instantiate the ADK tool you want to expose
print("Initializing ADK load_web_page tool...")
adk_web_tool = FunctionTool(load_web_page)
print(f"ADK tool '{adk_web_tool.name}' initialized.")
# --- End ADK Tool Prep ---

# --- MCP Server Setup ---
print("Creating MCP Server instance...")
# Create a named MCP Server instance
app = Server("adk-web-tool-mcp-server")

# Implement the MCP server's @app.list_tools handler
@app.list_tools()
async def list_tools() -> list[mcp_types.Tool]:
  """MCP handler to list available tools."""
  print("MCP Server: Received list_tools request.")
  # Convert the ADK tool's definition to MCP format
  mcp_tool_schema = adk_to_mcp_tool_type(adk_web_tool)
  print(f"MCP Server: Advertising tool: {mcp_tool_schema.name}")
  return [mcp_tool_schema]

# Implement the MCP server's @app.call_tool handler
@app.call_tool()
async def call_tool(
    name: str, arguments: dict
) -> list[mcp_types.TextContent | mcp_types.ImageContent | mcp_types.EmbeddedResource]:
  """MCP handler to execute a tool call."""
  print(f"MCP Server: Received call_tool request for '{name}' with args: {arguments}")

  # Check if the requested tool name matches our wrapped ADK tool
  if name == adk_web_tool.name:
    try:
      # Execute the ADK tool's run_async method
      # Note: tool_context is None as we are not within a full ADK Runner invocation
      adk_response = await adk_web_tool.run_async(
          args=arguments,
          tool_context=None, # No ADK context available here
      )
      print(f"MCP Server: ADK tool '{name}' executed successfully.")
      # Format the ADK tool's response (often a dict) into MCP format.
      # Here, we serialize the response dictionary as a JSON string within TextContent.
      # Adjust formatting based on the specific ADK tool's output and client needs.
      response_text = json.dumps(adk_response, indent=2)
      return [mcp_types.TextContent(type="text", text=response_text)]

    except Exception as e:
      print(f"MCP Server: Error executing ADK tool '{name}': {e}")
      # Return an error message in MCP format
      # Creating a proper MCP error response might be more robust
      error_text = json.dumps({"error": f"Failed to execute tool '{name}': {str(e)}"})
      return [mcp_types.TextContent(type="text", text=error_text)]
  else:
      # Handle calls to unknown tools
      print(f"MCP Server: Tool '{name}' not found.")
      error_text = json.dumps({"error": f"Tool '{name}' not implemented."})
      # Returning error as TextContent for simplicity
      return [mcp_types.TextContent(type="text", text=error_text)]

# --- MCP Server Runner ---
async def run_server():
  """Runs the MCP server over standard input/output."""
  # Use the stdio_server context manager from the MCP library
  async with mcp.server.stdio.stdio_server() as (read_stream, write_stream):
    print("MCP Server starting handshake...")
    await app.run(
        read_stream,
        write_stream,
        InitializationOptions(
            server_name=app.name, # Use the server name defined above
            server_version="0.1.0",
            capabilities=app.get_capabilities(
                # Define server capabilities - consult MCP docs for options
                notification_options=NotificationOptions(),
                experimental_capabilities={},
            ),
        ),
    )
    print("MCP Server run loop finished.")

if __name__ == "__main__":
  print("Launching MCP Server exposing ADK tools...")
  try:
    asyncio.run(run_server())
  except KeyboardInterrupt:
    print("\nMCP Server stopped by user.")
  except Exception as e:
    print(f"MCP Server encountered an error: {e}")
  finally:
    print("MCP Server process exiting.")
# --- End MCP Server ---
```

# 测试 MCP 服务器

编写完成后进行测试，这里按照此前的示例，我们使用文件系统 MCP 服务器中的说明创建 MCP 客户端并进行如下修改，使其能接入现在的MCP服务器：

![](img/ca2175c79a4f9743a03ad52b750d15b9.png)

```python
# ...
async def get_tools_async():
  """Gets tools from the File System MCP Server."""
  print("Attempting to connect to MCP Filesystem server...")
  tools, exit_stack = await MCPToolset.from_server(
      # Use StdioServerParameters for local process communication
      connection_params=StdioServerParameters(
          command='python3', # Command to run the server
          args=[
                "/root/autodl-tmp/MCP/temp-test-mcp/server.py"],
      )
  )
# ...
```

修改后如图所示：

![](img/5869165434d8e0ca6a639bbce6fc5214.png)

同时需要将模型改为谷歌`gemini`模型：

![](img/88fb5b292775cdfc7531d1307af7f26a.png)

并在`.env`中输入`gemini`模型的API-KEY

![](img/1b7d2a6694204aed5b41996eaded6f45.png)

其他不变，直接输入命令启动服务：

```
uv run main.py
```

测试效果如下：

![](img/9fe87db3b457e0ef3e787834a4502945.png)

![](img/51a111e56f1c46cdafd145c2d062cc1c.png)

# 三、借助 `adk web dev` UI 调用MCP

当然我们还可以使用 MCP web工具调用`agent`，并同时调用MCP工具。和此前一样，我们需要在当前主目录下创建一个web UI项目文件夹：

![](img/99441298ea4be775826349dcbf7f61b8.png)

然后复制主目录下的`.env`到`mcp-agent`文件夹内，并再创建`_init_.py`文件和`agent.py`文件，并在其中写入：

```python
from . import agent
```

同时在`agent.py`中写入如下内容：



# Google ADK 与 MCP 工具集成开发示例

```python
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
os.environ['HTTP_PROXY'] = 'http://127.0.0.1:10080'
os.environ['HTTPS_PROXY'] = 'http://127.0.0.1:10080'

# --- Step 1: 创建导入MCP工具函数 ---
async def get_tools_async():
  """Gets tools from the File System MCP Server."""
  print("Attempting to connect to MCP Filesystem server...")
  tools, exit_stack = await MCPToolset.from_server(
      # Use StdioServerParameters for local process communication
      connection_params=StdioServerParameters(
          command='python3', # Command to run the server
          args=[
                "/root/autodl-tmp/MCP/temp-test-mcp/server.py"],
      )
  )
  print("MCP Toolset created successfully.")
  # MCP需要维护一个与本地MCP服务器的连接。
  # exit_stack管理这个连接的清理。
  return tools, exit_stack

# --- Step 2: 创建ADK Agent ---
async def create_agent():
  """Gets tools from MCP Server."""
  tools, exit_stack = await get_tools_async()

  agent = LlmAgent(
      model='gemini-2.0-flash', 
      name='filesystem_assistant',
      instruction='Help user interact with the local filesystem using available tools.',
      tools=tools, 
  )
  return agent, exit_stack


root_agent = create_agent()
```

## 运行方式

然后在命令行输入：

```
adk web --port 8003
```

其中端口号可以自定义，最终运行效果如下：

![](img/6fa51ca7a91c733c6be305f104701e63.png)

---

## 课程介绍

+ 体验课内容节选自[《2025大模型Agent智能体开发实战》(5月班)](https://whakv.xetslk.com/s/pxKHG)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(5月班)

![](img/54fd961a0e1874fd28f9f88f56db6a8c.png)

《2025大模型Agent智能体开发实战》(5月班)

![](img/03180dd12d20709c51ab12e4579ccb1e.png)

同时，5月班重磅新增DeepSeek+Agents SDK+谷歌ADK+MCP技术应用与智能体开发相关实战内容，并计划新增Qwen-3模型实战教学：

![](img/fc20099002f2fb2f06a51764b82b1856.png)

### 部分课程成果演示

+ Dify+DeepSeek搭建智能客服
+ 可视化数据分析Multi-Agent
+ Ollama 自动化并发请求测试与动态资源监控
+ Neo4j并行多线程导入百万级文本方法与实践
+ MateGen Pro 项目功能演示
+ 智能客服项目展示

此外，若是对大模型底层原理感兴趣，也欢迎报名由我和菜菜老师共同主讲的《2025大模型原理与实战课程》(5月班)

![](img/bae67ca684292b027e778865a1fe5e4b.png)

**两门大模型课程5月班目前上新特惠中，立减2000起，合购还有更多**优惠哦~详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/5545d8a83d56f9f0cfd26466adc854b6.png)
