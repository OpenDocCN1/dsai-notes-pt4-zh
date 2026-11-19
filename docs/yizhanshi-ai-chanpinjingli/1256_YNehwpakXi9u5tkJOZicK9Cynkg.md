
# Model Context Protocol

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Pp4tdBC9qojMGLxtn0gchScSnSc](https://v11enp9ok1h.feishu.cn/docx/Pp4tdBC9qojMGLxtn0gchScSnSc)

## MCP 简介

![](img/c1e9b0c8fe704070a04743d6b0013fec.png)

MCP 是一个开放协议，它为应用程序向 LLM 提供上下文的方式进行了标准化。你可以将 MCP 想象成 AI 应用程序的 USB-C 接口。就像 USB-C 为设备连接各种外设和配件提供了标准化的方式一样，MCP 为 AI 模型连接各种数据源和工具提供了标准化的接口。

---

### 为什么选择 MCP？

MCP 帮助你在 LLM 的基础上构建代理（agents）和复杂的工作流。LLM 经常需要与数据和工具集成，而 MCP 提供了：

+ 持续增长的预构建集成列表，LLM 可直接使用

+ 灵活切换不同的 LLM 提供商和厂商

+ 在你的基础设施内安全地处理数据的最佳实践

### 通用架构

MCP 核心采用客户端-服务器架构，主机应用可以连接多个服务器：

![](img/55cac86c42fda2e31c7e6702c4002233.png)

+ `MCP Hosts`: 如 Claude Desktop、IDE 或 AI 工具，希望通过 MCP 访问数据的程序

+ `MCP Clients`: 维护与服务器一对一连接的协议客户端

+ `MCP Servers`: 轻量级程序，通过标准的 Model Context Protocol 提供特定能力

+ 本地数据源: MCP 服务器可安全访问的计算机文件、数据库和服务

+ 远程服务: MCP 服务器可连接的互联网上的外部系统（如通过 APIs）

### 核心架构

了解 MCP 如何连接 clients、servers 和 LLMs

Model Context Protocol (MCP) 构建在一个灵活且可扩展的架构上，使 LLM 应用和集成之间的无缝通信成为可能。本文档涵盖了核心架构组件和概念。

### 概述

MCP 遵循一个 client-server 架构，其中：

+ `Hosts` 是 LLM 应用（如 Claude Desktop 或 IDEs），它们发起连接

+ `Clients` 在 host 应用中与 servers 保持 1:1 的连接

+ `Servers` 为 clients 提供上下文、tools 和 prompts

![](img/497249a08b0d6e59dcc92f590d974766.png)

### 核心组件

#### 协议层

协议层处理消息框架、请求/响应链接和高级通信模式。

![](img/83bd5f815ab4ac73ddfecd2671e84d9e.png)

```
class Session(BaseSession[RequestT, NotificationT, ResultT]):
    async def send_request(
        self,
        request: RequestT,
        result_type: type[Result]
    ) -> Result:
        """
        发送请求并等待响应。如果响应包含错误则引发 McpError。
        """
        # 请求处理实现

    async def send_notification(
        self,
        notification: NotificationT
    ) -> None:
        """发送不期望响应的单向通知。"""
        # 通知处理实现

    async def _received_request(
        self,
        responder: RequestResponder[ReceiveRequestT, ResultT]
    ) -> None:
        """处理来自另一端的传入请求。"""
        # 请求处理实现

    async def _received_notification(
        self,
        notification: ReceiveNotificationT
    ) -> None:
        """处理来自另一端的传入通知。"""
        # 通知处理实现
```

关键的 classes 包括：

+ `Protocol`

+ `Client`

+ `Server`

#### 传输层

传输层处理 clients 和 servers 之间的实际通信。MCP 支持多种传输机制：

1. `Stdio` 传输

+ 使用标准输入/输出进行通信

+ 适用于本地进程

1. 通过 HTTP 的 SSE 传输

+ 使用服务器发送事件进行服务器到客户端的消息传递

+ 使用 HTTP POST 进行客户端到服务器的消息传递

所有传输都使用 [`JSON-RPC`](https://www.jsonrpc.org/) 2.0 进行消息交换。有关 [Model Context Protocol](https://spec.modelcontextprotocol.io/) 消息格式的详细信息，请参阅规范。

#### 消息类型

MCP 具有以下主要类型的消息：

1. `Requests` 期望来自另一端的响应：

![](img/b3ef563f089cd190b3752da3c9666c9b.png)

1. `Results` 是对请求的成功响应：

![](img/ea1aeed7cb2dbecee990acd00832a9a0.png)

1. `Errors` 表示请求失败：

![](img/6e5235d552acd0c3be4ae55189475d00.png)

1. `Notifications` 是不期望响应的单向消息：

![](img/e124f4f9e15f0df3448311cb630304f7.png)

### 连接生命周期

#### 初始化

![](img/b75eabc38bc0f1d6dd74c35b44e7f2ff.png)

1. `Client` 发送包含协议版本和能力的 `initialize` 请求

1. `Server` 以其协议版本和能力响应

1. `Client` 发送 `initialized` 通知作为确认

1. 开始正常消息交换

#### 消息交换

初始化后，支持以下模式：

+ 请求-响应：客户端或服务器发送请求，另一方响应

+ 通知：任一方发送单向消息

#### 终止

任一方可以终止连接：

+ 通过 `close()` 进行干净关闭

+ 传输断开

+ 错误条件

### 错误处理

MCP 定义了以下标准错误代码：

![](img/3bfc8b9dea3212b6c8c57e1e3471a3c8.png)

SDK 和应用可以定义其自己的错误代码，范围在 -32000 以上。

错误通过以下方式传播：

+ 对请求的错误响应

+ 传输上的错误事件

+ 协议级别的错误处理程序

### 实现示例

以下是实现 MCP 服务器的基本示例：

![](img/1488d70773dbfb8991ca3e8f6271ad12.png)

```
import asyncio
import mcp.types as types
from mcp.server import Server
from mcp.server.stdio import stdio_server

app = Server("example-server")

@app.list_resources()
async def list_resources() -> list[types.Resource]:
    return [
        types.Resource(
            uri="example://resource",
            name="Example Resource"
        )
    ]

async def main():
    async with stdio_server() as streams:
        await app.run(
            streams[0],
            streams[1],
            app.create_initialization_options()
        )

if __name__ == "__main__":
    asyncio.run(main())
```

### 最佳实践

#### 传输选择

1. 本地通信

+ 对于本地进程使用 `stdio` 传输

+ 对于同一机器的通信效率高

+ 简单的进程管理

1. 远程通信

+ 在需要 HTTP 兼容性的场景中使用 SSE

+ 考虑安全隐患，包括身份验证和授权

#### 消息处理

1. 请求处理

+ 彻底验证输入

+ 使用类型安全的模式

+ 优雅地处理错误

+ 实现超时

1. 进度报告

+ 对于长时间操作使用进度令牌

+ 增量报告进度

+ 在已知时包括总进度

1. 错误管理

+ 使用适当的错误代码

+ 包含有帮助的错误信息

+ 在错误时清理资源

### 安全考虑

1. 传输安全

+ 对于远程连接使用 TLS

+ 验证连接来源

+ 在需要时实现身份验证

1. 消息验证

+ 验证所有传入消息

+ 清理输入

+ 检查消息大小限制

+ 验证 `JSON-RPC` 格式

1. 资源保护

+ 实现访问控制

+ 验证资源路径

+ 监控资源使用

+ 限制请求速率

1. 错误处理

+ 不泄露敏感信息

+ 记录安全相关错误

+ 实现适当的清理

+ 处理 DoS 场景

### 调试和监控

1. 日志记录

+ 记录协议事件

+ 跟踪消息流

+ 监控性能

+ 记录错误

1. 诊断

+ 实现健康检查

+ 监控连接状态

+ 跟踪资源使用

+ 性能分析

1. 测试

+ 测试不同的传输

+ 验证错误处理

+ 检查边界情况

+ 负载测试服务器

## 架构

SDK 遵循分层架构，明确分离关注点：

![](img/813a0825bf9d598c1b454a79db37cf0c.png)

+ 客户端/服务器层 (`McpClient`/`McpServer`)：两者都使用 `McpSession` 进行同步/异步操作，`McpClient` 处理客户端协议操作，`McpServer` 管理服务器端协议操作。

+ 会话层 (`McpSession`)：使用 `DefaultMcpSession` 实现管理通信模式和状态。

+ 传输层 (`McpTransport`)：通过以下方式处理 `JSON-RPC` 消息序列化/反序列化：

+ 核心模块中的 `StdioTransport`（`stdin`/`stdout`）

+ 专用传输模块中的 HTTP SSE 传输（`HttpClient`, Spring WebFlux, Spring WebMVC）

MCP 客户端是 Model Context Protocol (MCP) 架构中的关键组件，负责与 MCP 服务器建立和管理连接。它实现了协议的客户端部分。

![](img/d62837f4f7d287745b5d1154aa1b8f4f.png)

MCP 服务器是 Model Context Protocol (MCP) 架构中的基础组件，向客户端提供工具、资源和功能。它实现了协议的服务器端部分。

![](img/72882f182e83545da855185eb0495218.png)

关键交互：

+ 客户端/服务器初始化：传输设置、协议兼容性检查、能力协商和实现细节交换。

+ 消息流：JSON-RPC 消息处理，包括验证、类型安全的响应处理和错误处理。

+ 资源管理：资源发现、基于 URI 模板的访问、订阅系统和内容检索。

### 依赖

将以下 Maven 依赖添加到您的项目中：

核心 MCP 功能：

```
<dependency>
    <groupId>io.modelcontextprotocol.sdk</groupId>
    <artifactId>mcp</artifactId>
</dependency>
```

核心 `mcp` 模块已经包含了默认的 `STDIO` 和 `SSE` 传输实现，无需外部 Web 框架。

如果您使用 Spring Framework 并希望使用 Spring 特定的传输实现，请添加以下可选依赖之一：

```
<!-- 可选：基于 Spring WebFlux 的 SSE 客户端和服务器传输 -->
<dependency>
    <groupId>io.modelcontextprotocol.sdk</groupId>
    <artifactId>mcp-spring-webflux</artifactId>
</dependency>

<!-- 可选：基于 Spring WebMVC 的 SSE 服务器传输 -->
<dependency>
    <groupId>io.modelcontextprotocol.sdk</groupId>
    <artifactId>mcp-spring-webmvc</artifactId>
</dependency>
```

#### 物料清单 (BOM)

物料清单 (BOM) 声明了给定版本所使用的所有依赖的推荐版本。 在您的应用程序构建脚本中使用 BOM 可以避免您自己指定和维护依赖版本。 相反，您使用的 BOM 版本决定了使用的依赖版本。 它还确保您默认使用支持和测试过的依赖版本，除非您选择覆盖它们。

将 BOM 添加到您的项目中：

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.modelcontextprotocol.sdk</groupId>
            <artifactId>mcp-bom</artifactId>
            <version>0.8.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

将版本号替换为您想要使用的 BOM 版本。

#### 可用依赖

以下依赖由 BOM 管理并可用：

+ 核心依赖

+ `io.modelcontextprotocol.sdk:mcp` - 提供 Model Context Protocol 实现的基础功能和 API 的核心 MCP 库，包括默认的 STDIO 和 SSE 客户端和服务器传输实现。无需外部 Web 框架。

+ 可选传输依赖（如果使用 Spring Framework 则方便）

+ `io.modelcontextprotocol.sdk:mcp-spring-webflux` - 用于响应式应用程序的基于 WebFlux 的服务器发送事件 (SSE) 传输实现。

+ `io.modelcontextprotocol.sdk:mcp-spring-webmvc` - 用于基于 Servlet 的应用程序的基于 WebMVC 的服务器发送事件 (SSE) 传输实现。

+ 测试依赖

+ `io.modelcontextprotocol.sdk:mcp-test` - 用于基于 MCP 的应用程序的测试工具和支持。
