
```
# 回到项目主目录
# cd /root/autodl-tmp/MCP/mcp-server-deepresearch

# 若此前打包完成，则无需再次打包
# python -m build

# 上传到pypi平台
python -m twine upload dist/*
```

![](img/a8a6d88b7f5e39052edc061beb4275ef.png)

上传完成后即可在pypi中搜索到包的信息：

![](img/8c15293654c15f52e22eb65551565d82.png)

然后即可在本地测试调用。

这里以Cherry studio为例，尝试调用我们刚刚发布的`DeepResearch`包。点击添加服务器：

![](img/d6219679d64e9e78fd2a9ae42c5b3b80.png)

然后输入MCP服务器名称（随意填写），然后使用`uvx`进行下载，同时是`stdio`通信标准，并在参数中输入以下三行：

```
mcp-server-deepresearch
--openai_api_key
YOUR_API_KEY
```

![](img/ad62c1ba295bf5b8f40b87ceb154fa79.png)

然后点击右上方保存，即可开启验证并自动开启这项`MCP`工具。紧接着在对话过程中选择`deepresearch`工具，只要对话中明确表达了希望围绕某个问题进行深入研究，模型就会自动调用该`MCP`工具进行深度调研：

![](img/de592e9a23fdcff8864521f60996ab23.png)

实际演示效果如下：

> 注，如果是离线拷贝的项目希望使用`cherry studio`进行调用，仅需在配置中写入：

```
/path/to/your/mcp-server-deepresearch
--openai_api_key
YOUR_API_KEY
```

> 也就是第一行改为离线项目的本地存储地址即可。

> 此外，若是在`Cursor`中调用，则可以写入如下配置：

```
{
  "mcpServers": {
    "deepresearch": {
      "command": "uvx",
      "args": [
        "mcp-server-deepresearch",
        "--openai_api_key",
        "OPENAI_API_KEY"
      ]
    }
  }
}
```

# 四、基于SSE传输方式的MCP服务器创建流程

以上`MCP`服务器都是`stdio`传输方式，而除此之外，目前`MCP`服务器还支持`SSE`传输和基于`HTTP`的流式传输。这两种传输方式也有非常广泛的实际用途，接下来详细介绍如何构建基于`SSE`和`HTTP`流式传输的`MCP`服务器。

## 1. stdio、SSE与基于HTTP的流式传输形式对比

### 1.1 MCP通信协议介绍

`MCP`（Model Context Protocol）是一种为了统一大规模模型和工具间通信而设计的协议，它定义了消息格式和通信方式。`MCP` 协议支持多种传输机制，其中包括 `stdio`、`Server-Sent Events（SSE）` 和 `Streamable HTTP`。每种通信方法在不同的应用场景中具有不同的优劣势，适用于不同的需求。

### 1.2 Stdio 传输（Standard Input/Output）

`stdio` 传输方式是最简单的通信方式，通常在本地工具之间进行消息传递时使用。它利用标准输入输出（`stdin`/`stdout`）作为数据传输通道，适用于本地进程间的交互。

+   工作方式：客户端和服务器通过标准输入输出流（`stdin`/`stdout`）进行通信。客户端向服务器发送命令和数据，服务器执行并通过标准输出返回结果。

+   应用场景：适用于本地开发、命令行工具、调试环境，或者模型和工具服务在同一进程内运行的情况。

### 1.3 Server-Sent Events（SSE）

`SSE` 是基于 `HTTP` 协议的流式传输机制，它允许服务器通过 `HTTP` 单向推送事件到客户端。`SSE` 适用于客户端需要接收服务器推送的场景，通常用于实时数据更新。

+   工作方式：客户端通过 `HTTP` GET 请求建立与服务器的连接，服务器以流式方式持续向客户端发送数据，客户端通过解析流数据来获取实时信息。

+   应用场景：适用于需要服务器主动推送数据的场景，如实时聊天、天气预报、新闻更新等。

### 1.4 Streamable HTTP

+   MCP更新公告：https://modelcontextprotocol.io/development/updates

+   Streamable HTTP协议内容：https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2025-03-26/basic/transports.mdx#streamable-http

![](img/bfb331dd74b2ef0267f2da035c92c3a7.png)

`Streamable HTTP` 是 `MCP` 协议中新引入的一种传输方式，它基于 `HTTP` 协议支持双向流式传输。与传统的 `HTTP` 请求响应模型不同，`Streamable HTTP` 允许服务器在一个长连接中实时向客户端推送数据，并且可以支持多个请求和响应的流式传输。

不过需要注意的是，`MCP`只提供了`Streamable HTTP`协议层的支持，也就是规范了`MCP`客户端在使用`Streamable HTTP`通信时的通信规则，而并没有提供相关的`SDK`客户端。开发者在开发`Streamable HTTP`机制下的客户端和服务器时，可以使用比如`Python` `httpx`库进行开发。

+   工作方式：客户端通过 `HTTP` POST 向服务器发送请求，并可以接收流式响应（如 `JSON-RPC` 响应或 `SSE` 流）。当请求数据较多或需要多次交互时，服务器可以通过长连接和分批推送的方式进行数据传输。

+   应用场景：适用于需要支持高并发、低延迟通信的分布式系统，尤其是跨服务或跨网络的应用。适合高并发的场景，如实时流媒体、在线游戏、金融交易系统等。

### 1.1.4 MCP 传输方式优劣势对比

| 特性 | `Stdio` | `SSE` | `Streamable HTTP` |
| --- | --- | --- | --- |
| 通信方向 | 双向（但仅限本地） | 单向（服务器到客户端） | 双向（适用于复杂交互） |
| 使用场景 | 本地进程间通信 | 实时数据推送，浏览器支持 | 跨服务、分布式系统、大规模并发支持 |
| 支持并发连接数 | 低 | 中等 | 高（适合大规模并发） |
| 适应性 | 局限于本地环境 | 支持浏览器，但单向通信 | 高灵活性，支持流式数据与请求批处理 |
| 实现难度 | 简单，适合本地调试 | 简单，但浏览器兼容性和长连接限制 | 复杂，需处理长连接和流管理 |
| 适合的业务类型 | 本地命令行工具，调试环境 | 实时推送，新闻、股票等实时更新 | 高并发、分布式系统，实时交互系统 |

三种传输方式总结如下：

+   `Stdio` 传输：适合本地进程之间的简单通信，适合命令行工具或调试阶段，但不支持分布式。

+   `SSE` 传输：适合实时推送和客户端/浏览器的单向通知，但无法满足双向复杂交互需求。

+   `Streamable HTTP` 传输：最灵活、最强大的选项，适用于大规模并发、高度交互的分布式应用系统，虽然实现较复杂，但能够处理更复杂的场景。

## 2. 基于SSE传输的MCP服务器创建流程

需要注意的是，目前`MCP SDK`只提供了`stdio`和`SSE`两种传输方式的开发库，而暂时还没有提供基于流式`HTTP`传输的开发工具。因此在进行`MCP`开发过程中，实现`stdio`和`SSE`传输方式较为简单，但要实现流式传输的`HTTP`流程则会非常复杂。

这里我们先介绍相对简单的`SSE`传输方式的实现方法。当我们使用`MCP Python SDK`开发`MCP`服务器时，只需要在此处进行设置：

```
mcp.run(transport='sse')
```

即可让`MCP`服务器开启`SSE`模式，非常简单。这里我们以创建一个查询天气`MCP`服务器为例进行演示。

+   创建项目主目录

```
cd /root/autodl-tmp/MCP
mkdir ./MCP-sse-test
cd ./MCP-sse-test
```

+   创建基础项目结构

```
uv init mcp-get-weather
cd mcp-get-weather

# 创建虚拟环境
uv venv

# 激活虚拟环境
source .venv/bin/activate

uv add mcp httpx
```

![](img/e6353437c66d3c643d849178e4c7aaec.png)

然后删除主目录下的`main.py`文件，并创建代码文件夹：

```
mkdir -p ./src/mcp_get_weather
cd ./src/mcp_get_weather
```

+   创建服务器核心代码

在`src/mcp_get_weather`中创建三个代码文件：

![](img/1946818a4b40eff1ba047cf95711b8e0.png)

其中`server.py`主要负责进行天气查询，代码如下：

```
import json
import httpx
import argparse
from typing import Any
from mcp.server.fastmcp import FastMCP

# 初始化 MCP 服务器
mcp = FastMCP("WeatherServer")

# OpenWeather API 配置
OPENWEATHER_API_BASE = "https://api.openweathermap.org/data/2.5/weather"
API_KEY = None
USER_AGENT = "weather-app/1.0"

async def fetch_weather(city: str) -> dict[str, Any] | None:
    """
    从 OpenWeather API 获取天气信息。
    """
    if API_KEY is None:
        return {"error": "API_KEY 未设置，请提供有效的 OpenWeather API Key。"}

    params = {
        "q": city,
        "appid": API_KEY,
        "units": "metric",
        "lang": "zh_cn"
    }
    headers = {"User-Agent": USER_AGENT}

    async with httpx.AsyncClient() as client:
        try:
            response = await client.get(OPENWEATHER_API_BASE, params=params, headers=headers, timeout=30.0)
            response.raise_for_status()
            return response.json()
        except httpx.HTTPStatusError as e:
            return {"error": f"HTTP 错误: {e.response.status_code}"}
        except Exception as e:
            return {"error": f"请求失败: {str(e)}"}

def format_weather(data: dict[str, Any] | str) -> str:
    """
    将天气数据格式化为易读文本。
    """
    if isinstance(data, str):
        try:
            data = json.loads(data)
        except Exception as e:
            return f"无法解析天气数据: {e}"

    if "error" in data:
        return f"⚠️ {data['error']}"

    city = data.get("name", "未知")
    country = data.get("sys", {}).get("country", "未知")
    temp = data.get("main", {}).get("temp", "N/A")
    humidity = data.get("main", {}).get("humidity", "N/A")
    wind_speed = data.get("wind", {}).get("speed", "N/A")
    weather_list = data.get("weather", [{}])
    description = weather_list[0].get("description", "未知")

    return (
        f"🌍 {city}, {country}\n"
        f"🌡 温度: {temp}°C\n"
        f"💧 湿度: {humidity}%\n"
        f"🌬 风速: {wind_speed} m/s\n"
        f"🌤 天气: {description}\n"
    )

@mcp.tool()
async def query_weather(city: str) -> str:
    """
    输入指定城市的英文名称，返回今日天气查询结果。
    """
    data = await fetch_weather(city)
    return format_weather(data)

def main():
    parser = argparse.ArgumentParser(description="Weather Server")
    parser.add_argument("--api_key", type=str, required=True, help="你的 OpenWeather API Key")
    args = parser.parse_args()
    global API_KEY
    API_KEY = args.api_key
    mcp.run(transport='sse')

if __name__ == "__main__":
    main()
```

其仍然是采用了`FastMCP`进行创建，而在`mcp.run`中设置了使用`SSE`方式进行传输。

```
mcp.run(transport='sse')
```

此外需要在`__init__.py`中写入

```
from .server import main
```

而在`__main__.py`中写入：

```
from mcp_get_weather import main

main()
```

同时回到主目录，修改项目配置文件`pyproject.toml`：
