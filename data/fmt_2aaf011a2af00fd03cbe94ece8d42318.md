
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
