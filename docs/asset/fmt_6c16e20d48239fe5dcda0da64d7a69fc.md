
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
