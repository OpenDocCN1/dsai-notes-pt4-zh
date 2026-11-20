
```toml
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "weather-mcp-server"
version = "1.0.0"
description = "Real-time weather query MCP server based on HelloAgents framework"
readme = "README.md"
license = {text = "MIT"}
authors = [
    {name = "HelloAgents Team", email = "xxx"}
]
requires-python = ">=3.10"
dependencies = [
    "hello-agents>=0.2.1",
    "requests>=2.31.0",
]

[project.urls]
Homepage = "https://github.com/yourusername/weather-mcp-server"
Repository = "https://github.com/yourusername/weather-mcp-server"
"Bug Tracker" = "https://github.com/yourusername/weather-mcp-server/issues"

[tool.setuptools]
py-modules = ["server"]
```

# 配置说明

- `[build-system]`: 指定构建工具（setuptools）

- `[project]`: 项目元数据

- `name`: 项目名称

- `version`: 版本号（遵循语义化版本）

- `dependencies`: 项目依赖列表

- `requires-python`: Python 版本要求

- `[project.urls]`: 项目相关链接

- `[tool.setuptools]`: setuptools 配置

虽然 Smithery 会自动生成 Dockerfile，但提供自定义 Dockerfile 可以确保部署成功：

```dockerfile
# Multi-stage build for weather-mcp-server
FROM python:3.12-slim-bookworm as base

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Copy project files
COPY pyproject.toml requirements.txt ./
COPY server.py ./

# Install Python dependencies
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Set environment variables
ENV PYTHONUNBUFFERED=1
ENV PORT=8081

# Expose port (Smithery uses 8081)
EXPOSE 8081

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD python -c "import sys; sys.exit(0)"

# Run the MCP server
CMD ["python", "server.py"]
```

## Dockerfile 配置说明

- 基础镜像：`python:3.12-slim-bookworm` - 轻量级 Python 镜像

- 工作目录：`/app` - 应用程序根目录

- 端口：`8081` - Smithery 平台标准端口

- 启动命令：`python server.py` - 运行 MCP 服务器

在这里，我们需要 Fork `hello-agents` 仓库，得到 `code` 中的源码，并使用自己的 GitHub 创建一个名为 `weather-mcp-server` 的仓库，将 `yourusername` 改为自己 GitHub 的 Username。

# （3）提交到 Smithery

打开浏览器，访问 https://smithery.ai/。使用 GitHub 账号登录 Smithery。点击页面上的 "Publish Server" 按钮，输入你的 GitHub 仓库 URL：`https://github.com/yourusername/weather-mcp-server`，即可等待发布。

一旦发布完成，可以看到类似这样的页面，如图 10.10 所示：

![](img/0ca981c3628a1f13c48beedd12ec2ba4.png)

图 10.10 Smithery 发布成功页面

一旦服务器发布成功，用户可以通过以下方式使用：

## 方式 1：通过 Smithery CLI

```bash
# 安装 Smithery CLI
npm install -g @smithery/cli

# 安装你的服务器
smithery install weather-mcp-server
```

## 方式 2：在 Claude Desktop 中配置

```json
{
  "mcpServers": {
    "weather": {
      "command": "smithery",
      "args": ["run", "weather-mcp-server"]
    }
  }
}
```

## 方式 3：在 HelloAgents 中使用

```python
from hello_agents import SimpleAgent, HelloAgentsLLM
from hello_agents.tools.builtin.protocol_tools import MCPTool

agent = SimpleAgent(name="天气助手", llm=HelloAgentsLLM())# 使用 Smithery 安装的服务器
weather_tool = MCPTool(
    server_command=["smithery", "run", "weather-mcp-server"])
agent.add_tool(weather_tool)

response = agent.run("北京今天天气怎么样？")
```

当然，这里只是举例，还有更多的用法可以自行探索，下图 10.11 展示了 `https://smithery.ai/server/@jjyaoao/weather-mcp-server` MCP 工具发布成功会包含的信息，显示服务的名称“天气”，其唯一标识符 `@jjyaoao/weather-mcp-server`，以及状态信息。Tools 区域就是我们刚刚实现的方法，Connect 区则提供了连接和使用此服务所需的技术信息，包括服务的接入 URL 地址和多种语言/环境下的配置代码片段。如果想要更加深入了解可以点击这个链接。

![](img/1b1e144cb76a9bd195089764f52b6652.png)

图 10.11 Smithery 发布成功的 MCP 工具

现在是时候去创造你的 MCP 服务器了！

# [10.6 本章总结]

本章系统性地介绍了智能体通信的三种核心协议：MCP、A2A 与 ANP，并探讨了它们的设计理念、应用场景与实践方法。

## 协议定位

- MCP (Model Context Protocol): 作为智能体与工具之间的桥梁，提供统一的工具访问接口，适用于增强单个智能体的能力。

- A2A (Agent-to-Agent Protocol): 作为智能体之间的对话系统，支持直接通信与任务协商，适用于小规模团队的紧密协作。

- ANP (Agent Network Protocol): 作为智能体的“互联网”，提供服务发现、路由与负载均衡机制，适用于构建大规模、开放的智能体网络。

## HelloAgents 的集成方案

在 `HelloAgents` 框架中，这三种协议被统一抽象为工具（`Tool`），实现了无缝集成，允许开发者灵活地为智能体添加不同层级的通信能力：

```python
# 统一的Tool接口
from hello_agents.tools import MCPTool, A2ATool, ANPTool

# 所有协议都可以作为Tool添加到Agent
agent.add_tool(MCPTool(...))
agent.add_tool(A2ATool(...))
agent.add_tool(ANPTool(...))
```

## 实战经验总结

- 优先利用成熟的社区 MCP 服务，以减少不必要的重复开发。

- 根据系统规模选择合适的协议：小规模协作场景推荐使用 A2A，大规模网络场景则应采用 ANP。

## 完成本章后，建议你

1. 动手实践：

   - 构建自己的 MCP 服务器

   - 利用协议创建多 Agent 协作系统

   - MCP、A2A 与 ANP 的组合应用策略

2. 深入学习：

   - 阅读 MCP 官方文档：`https://modelcontextprotocol.io`

   - 阅读 A2A 官方文档：`https://a2a-protocol.org/latest/`

   - 阅读 ANP 官方文档：`https://agent-network-protocol.com/guide/`

3. 参与社区：

   - 向社区贡献新的 MCP 服务

   - 分享个人开发的智能体实现案例

   - 参与相关协议的技术标准讨论，也可以在 Issue 提问或是直接帮助 `Helloagents` 支持新的 `example` 案例

恭喜你完成第十章的学习！

你现在已经掌握了智能体通信协议的核心知识。继续加油！🚀

# [习题]

> 提示：部分习题没有标准答案，重点在于培养学习者对智能体通信协议的综合理解和实践能力。

## 1. 本章介绍了三种智能体通信协议：MCP、A2A 和 ANP。请分析：

- 在 10.1.2 节中对比了三种协议的设计理念。请深入分析：为什么 MCP 强调"上下文共享"，A2A 强调"对话式协作"，而 ANP 强调"网络拓扑"？这些设计理念分别解决了什么核心问题？

- 假设你要构建一个"智能客服系统"，需要以下功能：（1）访问客户数据库和订单系统；（2）多个专业客服智能体协作处理复杂问题；（3）支持大规模并发用户请求。请为每个功能选择最合适的协议，并说明理由。

- 三种协议是否可以组合使用？请设计一个实际应用场景，展示如何同时使用 MCP、A2A 和 ANP 来构建一个完整的智能体系统。画出系统架构图并说明各协议的职责。

## 2. MCP（Model Context Protocol）是智能体与工具通信的标准协议。基于 10.2 节的内容，请深入思考：

> 提示：这是一道动手实践题，建议实际操作

- 在 10.2.3 节的 MCP 服务器实现中，我们定义了 `list_tools`、`call_tool` 等核心方法。请扩展这个实现，添加一个新的 MCP 服务器，提供以下工具：（1）数据库查询工具；（2）数据可视化工具；（3）报表生成工具。要求工具之间能够协作完成复杂的数据分析任务。

- MCP 协议支持"资源"（Resources）和"提示"（Prompts）两个重要概念，但本章主要聚焦于"工具"（Tools）。请查阅 MCP 官方文档，了解 Resources 和 Prompts 的设计目的，并设计一个应用场景，展示如何利用这三个核心概念构建更强大的智能体系统。

- MCP 使用 JSON-RPC 2.0 作为底层通信协议，通过 stdio 进行进程间通信。请分析：这种设计有什么优势和局限性？如果需要支持远程 MCP 服务器（通过 HTTP/WebSocket 访问），应该如何扩展当前的实现？

## 3. A2A（Agent-to-Agent Protocol）支持智能体间的对话式协作。基于 10.3 节的内容，请完成以下扩展实践：

> 提示：这是一道动手实践题，建议实际操作

- 在 10.3.4 节的"研究团队"案例中，研究员和撰写员通过 A2A 协议协作完成论文写作。请扩展这个案例，添加第三个智能体"审稿人"（Reviewer），它能够评审论文质量并提出修改建议。设计三个智能体之间的协作流程，并实现完整的代码。

- A2A 协议定义了 `task`、`task_result` 等消息类型。请分析：如果协作过程中出现冲突（如两个智能体对同一问题有不同意见），应该如何设计冲突解决机制？请扩展 A2A 协议，添加"协商"（negotiation）和"投票"（voting）等消息类型。

- 对比 A2A 协议与第六章介绍的 AutoGen、CAMEL 等多智能体框架：A2A 作为标准协议，与这些框架的关系是什么？它们能否互相替代？请设计一个方案，让基于 A2A 协议的智能体能够与 AutoGen 框架中的智能体进行通信。

## 4. ANP（Agent Network Protocol）支持大规模智能体网络。基于 10.4 节的内容，请深入分析：

- 在 10.4.2 节中介绍了 ANP 的网络拓扑设计，包括星型、网状、分层等结构。请分析：在什么场景下应该选择哪种拓扑结构？如果网络规模从 10 个智能体扩展到 1000 个智能体，拓扑结构应该如何演进？

- ANP 协议支持"路由"（routing）和"发现"（discovery）机制，让智能体能够动态找到合适的协作伙伴。请设计一个"智能路由算法"：根据任务类型、智能体能力、网络负载等因素，自动选择最优的消息路由路径。

- 在 10.4.4 节的"智能城市"案例中，多个智能体协作管理城市系统。请思考：如果某个关键智能体（如交通管理智能体）出现故障，整个系统应该如何应对？请设计一个"容错机制"，包括故障检测、备份切换、状态恢复等功能。

## 5. 智能体通信协议的安全性和隐私保护是实际应用中的关键问题。请思考：

- 在 10.2.4 节的 MCP 客户端实现中，智能体可以调用 MCP 服务器提供的任何工具。请分析：这种设计存在什么安全风险？如果 MCP 服务器提供了危险操作（如删除文件、执行系统命令），应该如何设计权限控制机制？

- A2A 和 ANP 协议涉及多个智能体之间的通信，可能包含敏感信息（如用户隐私数据、商业机密）。请设计一个"端到端加密"方案：确保消息在传输过程中不被窃听或篡改，同时支持智能体身份认证和访问控制。

- 在大规模智能体网络中，恶意智能体可能会发送虚假信息、发起拒绝服务攻击或窃取其他智能体的数据。请设计一个"信任评估系统"：根据智能体的历史行为、协作质量、社区评价等因素，动态评估每个智能体的可信度，并据此调整通信策略。

# [参考文献]

[1] Anthropic. (2024). Model Context Protocol. Retrieved October 7, 2025, from `https://modelcontextprotocol.io/`
