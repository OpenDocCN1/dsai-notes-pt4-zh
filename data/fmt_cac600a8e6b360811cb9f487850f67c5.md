
# MCP 蓝图：从头开始构建自定义 MCP 客户端

> 来源：[https://v11enp9ok1h.feishu.cn/docx/AhTtd0fRro5x7Oxu1cbcI29Mnlg](https://v11enp9ok1h.feishu.cn/docx/AhTtd0fRro5x7Oxu1cbcI29Mnlg)

## 回顾

在深入学习 MCP 课程之前，让我们花点时间回顾一下迄今为止所学到的知识。

在第一部分中，我们奠定了模型上下文协议 (MCP) 的概念基础。我们讨论了 MCP 解决的核心问题：M×N 集成问题。此前，每个新的模型-工具组合都需要自定义粘合代码。MCP 打破了这一模式，提供了一个统一的标准化接口。

![](img/6d78c6bfd990a052a8c28dcb9ee0d62c.png)

我们还深入探索了 MCP 架构：主机、客户端和服务器。这种模块化设计使 AI 助手能够接入多种工具和数据源，无需人工重复工作。

![](img/c4702f5026bb1168d5b35996eae88a1d.png)

在第 2 部分中，我们了解了工具、资源和提示等功能，还了解了 AI 模型如何动态使用这些构建块来推理、获取数据、执行操作和动态调整。

通过实际示例，我们了解了客户端-服务器功能交换的工作原理，以及为什么 MCP 能够改变动态工具集成的游戏规则。

如果您还没有阅读过前面的部分，我们强烈建议您先阅读一遍，然后再继续阅读。这将为您提供必要的基础知识，让您能够充分受益于接下来的内容。您可以在这里找到它们：

在本部分中，我们将把重点转向更实际的实施，并阐明前两部分中涵盖的几个关键思想和概念。

到本部分结束时，我们将对以下内容有具体的了解：

+ 如何构建自定义 MCP 客户端，而不依赖于 Cursor 或 Claude 等预构建的解决方案。

+ 完整的 MCP 生命周期在实际中是什么样的。

+ 通过实际集成揭示了 MCP 作为客户端-服务器架构的真正本质。

+ 通过实际操作说明 MCP 与传统 API 和函数调用的区别。

与往常一样，一切都将通过直观的示例和代码来辅助。

让我们开始吧！

## 构建 MCP 客户端

为了真正了解模型上下文协议 (MCP) 的工作原理，并将其集成到我们自己的应用程序中，我们现在要构建一个自定义的 MCP 客户端。

概括地说，MCP 客户端是主机内的一个组件，用于处理与 MCP 服务器的低级通信。

客户端可以理解为适配器或信使。当主机决定做什么时，客户端知道如何使用 MCP 来实际执行服务器的指令。

![](img/1186afe3501400766da568dafc238a78.png)

每个 MCP 客户端管理与单个 MCP 服务器的一对一连接。如果您的主机应用连接到多个服务器，它将实例化多个客户端实例，每个服务器一个。客户端负责以下任务：

+ 发送适当的 MCP 请求

+ 监听响应或通知

+ 处理错误或超时

+ 确保遵守协议规则

它本质上是您应用程序内的 MCP 协议驱动程序。

客户端角色可能听起来有点抽象，但一个有用的描述方式是将其与 Web 浏览器的网络层进行比较。

浏览器（主机）想要从各个网站获取数据；它在后台创建 HTTP 客户端连接以与每个网站的服务器进行通信。

在许多框架中，此客户端功能由 SDK 或库提供，因此您不必从头开始实现。

在我们的实施过程中，该客户将：

+ 连接到 MCP 服务器。

+ 将用户查询转发给 LLM。

+ 处理模型发出的工具调用。

+ 将最终响应返回给用户。

我们将使用 Python 来实现，并详细介绍每个步骤。

如果您已经完成了第二部分，那么您已经熟悉了设置流程，并了解了 MCP 服务器的实现方式。我们将在此基础上继续讲解。

为了实现基本的模型 MCP 客户端（尚未涉及 LLM），我们需要安装一些核心依赖项：

```
uv add mcp asyncio
```

为了保持一致，我们将使用上一部分中相同的示例服务器代码进行演示：

以下是我们将要使用的服务器设置（创建`server.py`文件并添加此代码）：

```
from fastmcp import FastMCP

# Initialize the MCP server
mcp = FastMCP("Sample Server")

# Define a tool to get weather information
@mcp.tool()
def get_weather(location: str) -> str:
    """Get the current weather for a specified location."""
    # mockup tool
    return f"Weather in {location}: Sunny, 72°F"

# Define a calculator tool
@mcp.tool()
def calculate(expression: str) -> float:
    """Calculate the result of a mathematical expression."""
    try:
        return eval(expression)
    except Exception as e:
        return f"Error calculating expression: {str(e)}"

# Define a currency converter tool
@mcp.tool()
def convert_currency(amount: float, from_currency: str, to_currency: str) -> str:
    """Convert amount from one currency to another."""
    # This is a simplified example - in reality, you'd want to use real exchange rates
    rates = {
        "USD": 1.0,
        "EUR": 0.85,
        "GBP": 0.73,
        "JPY": 110.0,
        "INR": 83.0
    }
    if from_currency not in rates or to_currency not in rates:
        return f"Unsupported currency pair: {from_currency} to {to_currency}"

    converted = amount * (rates[to_currency] / rates[from_currency])
    return f"{amount} {from_currency} = {converted:.2f} {to_currency}"


# Run the server
if __name__ == "__main__":
    mcp.run()
```

在上面的代码中，我们用两个工具创建了一个 MCP 服务器：

+ `get_weather`，为了简单起见，它只返回一个虚拟响应，但我们可以通过集成天气 API 使其变得实用。

+ `calculate`，计算数学表达式的结果。

+ `convert_currency`，将给定金额从一种货币转换为另一种货币。

要在传输下运行此服务器 `sse` ，只需将该 `mcp.run()` 行更改为：

```
mcp.run(transport="sse", host="127.0.0.1", port=8000)
```

此传输用于远程或长期运行的服务，通常通过网络或云。

顺便说一句，您可以下载整篇文章的代码以及完整的自述文件，其中包含下面的分步说明。

它包含一些 Python 文件和一个笔记本文件：

![](img/c44274a772cfa120a00784a5577ba892.png)
