
> 替代 `venv`：提供 `uv venv` 进行虚拟环境管理，比 `venv` 更轻量。

> 跨平台：支持 Windows、macOS 和 Linux。

#### 4.2 uv安装流程

##### 方法 1：使用 pip 安装（适用于已安装 pip 的系统）

```
pip install uv
```

##### 方法 2：使用 curl 直接安装

如果你的系统没有 `pip`，可以直接运行：

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

这会自动下载 `uv` 并安装到 `/usr/local/bin`。

#### 4.3 uv的基本用法介绍

安装 `uv` 后，你可以像 `pip` 一样使用它，但它的语法更简洁，速度也更快。注意，以下为使用语法示例，不用实际运行。

+   安装 Python 依赖

```
uv pip install requests
```

与 `pip install requests` 类似，但更快。

+   创建虚拟环境

```
uv venv myenv
```

等效于 `python -m venv myenv`，但更高效。

+   激活虚拟环境

```
source myenv/bin/activate  # Linux/macOS
myenv\Scripts\activate     # Windows
```

+   安装 requirements.txt

```
uv pip install -r requirements.txt
```

+   直接运行 Python 项目

如果项目中包含 `pyproject.toml`，你可以直接运行：

```
uv run python script.py
```

这等效于：

```
pip install -r requirements.txt
python script.py
```

但 `uv` 速度更快，管理更高效。

接下来我们就按照标准格式尝试先构建一个 MCP 客户端和服务器。

## 三、按照标准流程搭建MCP客户端与服务器

### 1.Server搭建流程

这里我们尝试一个入门级的示例，那就是创建一个天气查询的服务器。通过使用OpenWeather API，创建一个能够实时查询天气的服务器（server），并使用`stdio`方式进行通信。

测试查询效果

![](img/d7dfa6320a93bb1079982a48f3702252.png)

测试无误后，接下来即可进入到创建server的环节中。

### 2.天气查询服务器Server创建流程

#### 服务器依赖安装

由于我们需要使用http请求来查询天气，因此需要在当前虚拟环境中添加如下依赖

```
uv add mcp httpx
```

#### 服务器代码编写

接下来尝试创建服务器代码，此时MCP基本执行流程如下：

![](img/6a57175c32075013dd51d2c2acaab735.png)

对应server服务器代码如下：

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

![](img/2bea02fe01c87f1369a11708f80164a9.png)

代码解释如下：

##### Part 1. 异步获取天气数据

函数 `fetch_weather(city: str)`

> 使用 `httpx.AsyncClient()` 发送异步 GET 请求到 OpenWeather API。

> 如果请求成功，则调用 `response.json()` 返回一个字典。

出现异常时，返回包含错误信息的字典。

##### Part 2. 格式化天气数据

函数 `format_weather(data: dict | str)`

> 首先检查传入的数据是否为字符串，如果是，则使用 `json.loads` 将其转换为字典。

> 检查数据中是否包含 `"error"` 字段，如果有，直接返回错误提示。

> 使用 `.get()` 方法提取 `name`、`sys.country`、`main.temp`、`main.humidity`、`wind.speed` 和 `weather[0].description` 等数据，并为可能缺失的字段提供默认值。

将提取的信息拼接成一个格式化字符串，方便阅读。

##### Part 3. MCP 工具 `query_weather(city: str)`

函数 `query_weather`

> 通过 `@mcp.tool()` 装饰器注册为 MCP 服务器的工具，使其能够被客户端调用。

> 调用 `fetch_weather(city)` 获取天气数据，然后用 `format_weather(data)` 将数据格式化为易读文本，最后返回该字符串。

##### Part 4. 运行服务器

`if __name__ == "__main__":` 块

调用 `mcp.run(transport='stdio')` 启动 MCP 服务器，采用标准 I/O 通信方式，等待客户端调用。

此外，上述代码有两个注意事项，

`query_weather`函数的函数说明至关重要，相当于是此后客户端对函数进行识别的基本依据，因此需要谨慎编写；

> 当指定 `transport='stdio'` 运行 MCP 服务器时，客户端必须在启动时同时启动当前这个脚本，否则无法顺利通信。这是因为 `stdio` 模式是一种本地进程间通信（IPC，Inter-Process Communication）方式，它需要服务器作为子进程运行，并通过标准输入输出（`stdin`/`stdout`）进行数据交换。

因此，当我们编写完服务器后，并不能直接调用这个服务器，而是需要创建一个对应的能够进行`stdio`的客户端，才能顺利进行通信。

#### client创建流程

接下来继续创建客户端来调用MCP服务器。

##### 创建 MCP 客户端项目

```
# 创建项目目录
cd /root/autodl-tmp/MCP
uv init mcp-chatbot
cd mcp-chatbot
```

![](img/99bc4b9b2ea67c51c810d7f910e4c7be.png)

![](img/f6d6b167e706a223aca659a99d1a77c7.png)

##### 创建MCP客户端虚拟环境

```
# 创建虚拟环境
uv venv
 
# 激活虚拟环境
source .venv/bin/activate
```

![](img/d093929abfa124371b2e8cc063c7ca97.png)

这里需要注意的是，相比`pip`，`uv`会自动识别当前项目主目录并创建虚拟环境。

然后即可通过`add`方法在虚拟环境中安装相关的库。

```
# 安装 MCP SDK
uv add mcp openai python-dotenv httpx
```

![](img/af544e96d0d06eb91a51eccd910f51c9.png)

接下来创建`.env`文件，并写入OpenAI的API-Key，以及反向代理地址。借助反向代理，国内可以无门槛直连OpenAI官方服务器，并调用官方API。

![](img/e1f1990b8d64e8cd028243f08203f129.png)

写入如下内容

```
BASE_URL="反向代理地址"
MODEL=gpt-4o
OPENAI_API_KEY="OpenAI-API-Key"
```

![](img/2338087d452318d8036dc1730b3012c3.png)

创建`servers_config.json`

![](img/deb18987251335837a4686802063fcb5.png)

创建`weather_server.py`

![](img/50c3715369c19abeb9a2ba1e04e5e697.png)

并写入如下内容：
