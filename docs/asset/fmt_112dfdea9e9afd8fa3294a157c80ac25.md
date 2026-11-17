
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

