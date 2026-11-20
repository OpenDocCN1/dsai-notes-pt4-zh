
```
import json
import httpx
from typing import Any
from mcp.server.fastmcp import FastMCP

# 初始化 MCP 服务器
mcp = FastMCP("WeatherServer")

# OpenWeather API 配置
OPENWEATHER_API_BASE = "https://api.openweathermap.org/data/2.5/weather"
API_KEY = "YOUR_API_KEY"  # 请替换为你自己的 OpenWeather API Key
USER_AGENT = "weather-app/1.0"

async def fetch_weather(city: str) -> dict[str, Any] | None:
    """
    从 OpenWeather API 获取天气信息。
    :param city: 城市名称（需使用英文，如 Beijing）
    :return: 天气数据字典；若出错返回包含 error 信息的字典
    """
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
            return response.json()  # 返回字典类型
        except httpx.HTTPStatusError as e:
            return {"error": f"HTTP 错误: {e.response.status_code}"}
        except Exception as e:
            return {"error": f"请求失败: {str(e)}"}

def format_weather(data: dict[str, Any] | str) -> str:
    """
    将天气数据格式化为易读文本。
    :param data: 天气数据（可以是字典或 JSON 字符串）
    :return: 格式化后的天气信息字符串
    """
    # 如果传入的是字符串，则先转换为字典
    if isinstance(data, str):
        try:
            data = json.loads(data)
        except Exception as e:
            return f"无法解析天气数据: {e}"

    # 如果数据中包含错误信息，直接返回错误提示
    if "error" in data:
        return f"⚠️ {data['error']}"

    # 提取数据时做容错处理
    city = data.get("name", "未知")
    country = data.get("sys", {}).get("country", "未知")
    temp = data.get("main", {}).get("temp", "N/A")
    humidity = data.get("main", {}).get("humidity", "N/A")
    wind_speed = data.get("wind", {}).get("speed", "N/A")
    # weather 可能为空列表，因此用 [0] 前先提供默认字典
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
    :param city: 城市名称（需使用英文）
    :return: 格式化后的天气信息
    """
    data = await fetch_weather(city)
    return format_weather(data)

if __name__ == "__main__":
    # 以标准 I/O 方式运行 MCP 服务器
    mcp.run(transport='stdio')
```

![](img/7d7a69d53a981a451351148812ccf15f.png)

![](img/88100c99a55024f563eb2b103d2d140c.png)

# 代码解释

## Part 1. 异步获取天气数据

+ 函数 `fetch_weather(city: str)`
+ 使用 `httpx.AsyncClient()` 发送异步 `GET` 请求到 OpenWeather API。
+ 如果请求成功，则调用 `response.json()` 返回一个字典。
+ 出现异常时，返回包含错误信息的字典。

## Part 2. 格式化天气数据

+ 函数 `format_weather(data: dict | str)`
+ 首先检查传入的数据是否为字符串，如果是，则使用 `json.loads` 将其转换为字典。
+ 检查数据中是否包含 `"error"` 字段，如果有，直接返回错误提示。
+ 使用 `.get()` 方法提取 `name`、`sys.country`、`main.temp`、`main.humidity`、`wind.speed` 和 `weather[0].description` 等数据，并为可能缺失的字段提供默认值。
+ 将提取的信息拼接成一个格式化字符串，方便阅读。

## Part 3. MCP 工具 `query_weather(city: str)`

+ 函数 `query_weather`
+ 通过 `@mcp.tool()` 装饰器注册为 MCP 服务器的工具，使其能够被客户端调用。
+ 调用 `fetch_weather(city)` 获取天气数据，然后用 `format_weather(data)` 将数据格式化为易读文本，最后返回该字符串。

## Part 4. 运行服务器

+ `if __name__ == "__main__":` 块
+ 调用 `mcp.run(transport='stdio')` 启动 MCP 服务器，采用标准 I/O 通信方式，等待客户端调用。

此外，上述代码有两个注意事项，

1. `query_weather` 函数的函数说明至关重要，相当于是此后客户端对函数进行识别的基本依据，因此需要谨慎编写；
1. 当指定 `transport='stdio'` 运行 MCP 服务器时，客户端必须在启动时同时启动当前这个脚本，否则无法顺利通信。这是因为 `stdio` 模式是一种本地进程间通信（IPC，Inter-Process Communication）方式，它需要服务器作为子进程运行，并通过标准输入输出（`stdin`/`stdout`）进行数据交换。

因此，当我们编写完服务器后，并不能直接调用这个服务器，而是需要创建一个对应的能够进行 `stdio` 的客户端，才能顺利进行通信。

# 2. client创建流程

接下来继续创建客户端来调用 MCP 服务器。

## 2.1 创建 MCP 客户端项目

```
# 创建项目目录
cd /root/autodl-tmp/MCP
uv init mcp-chatbot
cd mcp-chatbot
```

![](img/acc6036bff42adfccc3968c1b80f7d91.png)

![](img/e481fd39963444967b008c7c32e0153b.png)

## 2.2 创建 MCP 客户端虚拟环境

```
# 创建虚拟环境
uv venv

# 激活虚拟环境
source .venv/bin/activate
```

![](img/af9a1a4abe8bd6e514945968de3ddcd3.png)

这里需要注意的是，相比 `pip`，`uv` 会自动识别当前项目主目录并创建虚拟环境。

然后即可通过 `add` 方法在虚拟环境中安装相关的库。

```
# 安装 MCP SDK
uv add mcp openai python-dotenv httpx
```

![](img/6b3611cc6361eed45cdaff2ad4dfbae1.png)

接下来创建 `.env` 文件，并写入 OpenAI 的 API-Key，以及反向代理地址。借助反向代理，国内可以无门槛直连 OpenAI 官方服务器，并调用官方 API。

![](img/8c45dfc99a53cfb0b726668047add2dd.png)

写入如下内容

```
BASE_URL="反向代理地址"
MODEL=gpt-4o
OPENAI_API_KEY="OpenAI-API-Key"
```

![](img/84ff27011df950e885315899376631c6.png)

OpenAI 注册指南与国内反向代理领取地址：

而如果是使用 DeepSeek 模型，则需要在 `.env` 中写入如下内容：

```
BASE_URL=https://api.deepseek.com
MODEL=deepseek-chat      
OPENAI_API_KEY="DeepSeek API-Key"
```

创建 `servers_config.json`

![](img/d038e3719b6ea10dc4323df5acc86bd7.png)

创建 `weather_server.py`

![](img/b4f1a5b120ba3f3229315e10506cfb37.png)

并写入如下内容：
