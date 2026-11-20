
# AI项目实战：基于 MCP 搭建 Agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LR4SdRghEotqLsxd8xKc9rn2nMc](https://qqs7y1hozd1.feishu.cn/docx/LR4SdRghEotqLsxd8xKc9rn2nMc)

Agent 应用的火爆，MCP（模型上下文协议）成为众多社区热议的焦点。今天，我们将深入解析 MCP 并提供实操应用指南。相信读完这篇文章，你就能立刻上手 MCP。

![](img/0136c01abb6951a8a1dfeb4869ddacde.png)

## 1、MCP 架构设计全剖析

### 第一、为什么会出现 MCP？

如果你开发了一个 AI 应用，无论是 ChatBot 还是复杂的 Agent，都不会仅限于简单的 LLM 对话。很多时候，你需要与外部世界连接，以访问数据源或使用工具。例如：

+ 操控浏览器实现自动化；
+ 访问本地文件；
+ 访问数据库结构，以便更好地让 AI 参与编程；
+ 调用 CRM 的 API 以驱动智能客服流程。

在这种情况下，你需要连接不同的对象，使用不同的接口协议。你可能需要熟悉 SQL、HTTP 调用、Playwright 等接口，甚至还需要使用搜索引擎、访问云存储、调用第三方开放 API 等。这些都涉及大量的“粘合”工作。

![](img/797e06550d6dd21606631645f0d33eab.png)

MCP（模型上下文协议）正是为了解决这一问题。它允许 LLM 应用通过统一的协议连接外部资源，而无需逐个适配。MCP 通过增加一个中间层来实现这一点：LLM 应用通过统一的 MCP 协议连接到中间层（称为 MCP Server），而中间层则负责处理与外部资源的对接。

![](img/5cd39d0f6b44574c0e03fea5713a4942.png)

那么，为什么在 Agent 应用火热之后，大家才开始关注 MCP 呢？原因很简单：Agent 是最需要对接外部资源和工具的 LLM 应用类型。

### 第二、MCP 有哪些好处？

这种设计范式在软件系统中其实很常见，比如：一些大模型 API 网关会将不同厂家的大模型协议转化为统一的 OpenAI 兼容协议，方便应用接入。它的具体意义主要体现在以下几点：

+ 简化 LLM 应用：无需适配各种私有协议，只需学会连接 MCP Server 即可。
+ 快速扩展 LLM 应用：随时可以“插拔”新的 MCP Server，需要更多功能时，简单添加即可。
+ 快速适应变化：如果外部资源的接口发生变化，只需在对应的 MCP Server 上进行修改，所有连接的 LLM 应用都能无缝适应。
+ 构建新的 AI 能力共享生态：通过共享 MCP Server，新的 LLM 应用能够快速获得各种工具，形成一种新的合作体系，从而提升整体的效用。

![](img/ce3b3c84d3ab9877d1debfa07b5c0591.png)

例如，MCP 的提出者 Anthropic 旗下的 Claude Desktop，就可以通过这种方式轻松扩充自身的能力。

### 第三、基于 MCP 的 Agent 架构设计

以下是基于 MCP 将 LLM 应用（Agent）与外部资源集成的架构设计图：

![](img/38ca2da93f450545e7e98382e46cc316.png)

与 LLM 应用直接连接外部资源的方式相比，这里主要增加了一个中间层（MCP Server）以及用于连接该中间层的（MCP Client）。理解了这两个组件，你就基本掌握了 MCP 的核心。

#### 1、MCP Server

这里的 “Server” 并非传统意义上的集中式服务器，更像是一个服务插件。

##### MCP Server 部署位置

MCP Server 可以部署在 LLM 应用所在的本机上，也可以远程部署（Remote）。不过目前远程部署方式尚未完善，暂时不考虑。以下内容仅针对本地部署模式。

##### MCP Server 提供的“服务”

+ Tools：为 LLM 应用，尤其是 Agent 提供各种工具。
+ Resources：为 LLM 应用提供额外的结构化数据。
+ Prompts：为 LLM 应用提供 Prompt 模板。例如，如果你的应用是一个 Chatbot，可以从 MCP Server 中获取这些模板供用户选择使用。

##### 获取 MCP Server 的方式

+ 自己使用 MCP SDK 创建后，供个人或企业内部共享使用。
+ 从他人创建的 MCP Servers 中“挑选”，然后下载使用。例如，可以从以下链接获取。

![](img/0c78f7df50e67651279e8d6e5b7f0b92.png)

##### MCP Server 的启动方式

在本地模式下，通过在 LLM 应用中配置启动命令后，MCP Server 会自动启动。

需要注意的是，不同的 MCP Server 可能有不同的启动命令，具体请查看 MCP Server 的说明书。

部分 MCP Server 可能需要先安装依赖项；而对于通过 `npx` 或 `uvx` 运行的 MCP Server，则会自动下载缓存并临时运行。

启动后，MCP Server 在物理形式上是一个独立的进程。

##### MCP Server 与 Client 应用的通信方式

在本地模式下，MCP Server 与客户端应用之间通过 stdio/stdout（标准输入输出）进行进程间通信，交换消息。这种通信方式你可能已经很熟悉了，例如：

```
cat file.txt | grep "error" | sort > result.txt
```

##### 创建 MCP Server 的编程语言

目前支持使用 TS（TypeScript）、Python、Java SDK 来编写 MCP Server。

#### 2、MCP Client

MCP Client 是由客户端 LLM 应用使用 Client SDK 创建并维护的一个 Server 会话，就像你在程序中维护一个数据库的 Connection 一样。一般长这样：

```
async with stdio_client(server_params) as (read, write): 
       async with ClientSession(  
                 read, write, sampling_callback=None   
                      ) as session:、
......
```

借助 Client SDK 你可以与 MCP Server 通信。比如查看 Server 的 Tools：

```
tools = await session.list_tools()
```

本地模式下，Client 与 Server 是一对一的关系。如果需要连接多个 MCP Server，需要自行维护多个 Session。

## 2、动手做一个 MCP

让我们通过一个简单的 Client + Server 的完整案例来加深对 MCP 的理解。

首先，使用以下命令安装 SDK：

```
pip install mcp
```

安装完成后，按照以下步骤进行操作：

### 第一、创建 MCP Server

这个 MCP Server 仅具备一个功能：提供一个计算器工具。具体实现如下：

```
#server_demo.py
from mcp.server.fastmcp import FastMCP
# 创建一个MCP服务器
mcp = FastMCP("演示")
# 添加一个工具
@mcp.tool()
def calculate(expression: str) -> float:  
  """计算四则运算表达式        
  参数:           
     expression: 数学表达式字符串，如 "1 + 2 * 3"        
  返回:            
  计算结果       
   """    
    ...省略计算器代码...    

if __name__ == "__main__":  
    mcp.run(transport='stdio')
```

注意这里必须要有启动代码，但现在你不需要启动它。

### 第二、创建 MCP Client

接下来，创建一个客户端应用，用于连接上述 MCP Server 并调用其中的计算器功能：

```
#client_demo.py
from mcp.client.stdio import stdio_client
from mcp import ClientSession, StdioServerParameters, types
import asyncio
#Client会使用这里的配置来启动本地 MCP Server
server_params = StdioServerParameters(   
   command="python",       
   args=["./server_demo.py"],     
    env=None 
 ) 

 async def main():         
     async with stdio_client(server_params) as (read, write):            
     async with ClientSession(                
       read, write, sampling_callback=None        )   
  )as session:                     

             await session.initialize()                     
             print('\n正在调用工具...')                
             result = await session.call_tool("calculate",{ "expression": "188*23-34" })                
             print(result.content)     

 asyncio.run(main())
```

现在直接运行这个 Client，就可以看到如下输出：

![](img/6d28289683140d419878cfb529172768.png)

### 第三、如何调试 MCP Server

如果只是单纯开发 MCP Server，如何快速进行测试呢？对于用 Python 编写的 Server，可以通过 MCP Inspector 来进行调试。运行以下命令：

```
mcp dev server_demo.py
```

随后，访问 `http://localhost:5173`，即可进入可视化调试界面，在这里可以直观地测试刚才的计算器工具。

![](img/194d6f89afdb7169afc16a62cf795160.png)
