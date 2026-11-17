
现在，我们仅需在创建Agent的时候，将MCP服务器视作为一项工具，即可顺利调用MCP服务器进行Agent开发。而实际在借助Agents SDK调用MCP的流程也非常简单，我们只需将MCP视作tools，即可进行调用。换而言之，就是如果使用Agents SDK作为Agent开发框架，则可以零门槛快速接入MCP海量服务器生态。

## 1. MCP技术回顾

MCP，全称是Model Context Protocol，模型上下文协议，由Claude母公司Anthropic于去年11月正式提出。

![](img/9e986ba5bc5a51edb451047e9a63fa55.png)

+   Anthropic MCP发布通告：https://www.anthropic.com/news/model-context-protocol

+   MCP GitHub主页：https://github.com/modelcontextprotocol

MCP的核心作用，是统一了Agent开发过程中，大模型调用外部工具的技术实现流程，从而大幅提高Agent开发效率。在MCP诞生之前，不同的外部工具各有不同的调用方法，要连接这些外部工具开发Agent，就必须“每一把锁单独配一把钥匙”，开发工作非常繁琐：

![](img/054a124c4ed0ad5a5d15cb704d08c180.png)

而MCP的诞生，则统一了这些外部工具的调用流程，使得无论什么样的工具，都可以借助MCP技术按照统一的一个流程快速接入到大模型中，从而大幅加快Agent开发效率。这就好比现在很多设备都可以使用type-c和电脑连接类似。

![](img/9621129494195d97ec99d5b899e1dfec.png)

从技术实现角度来看，我们可以将MCP看成是Function calling的一种封装，通过server-client架构和一整套开发工具，来规范化Function calling开发流程。

![](img/1dfdbb94b0cebc681755066248021c95.png)

换而言之，Agents SDK实现MCP技术流程如下：

![](img/c106d3b88bbb648f7c6e4b36dda31877.png)

除此之外，实际上，MCP提供了三个方面的技术支持，其一是抽象的协议，也就是技术理论层面的设计；其二是一整套MCP的开发工具，也就是一些库，方便大家进行MCP工具（MCP Server）的开发并构建智能体；其三，MCP还提供了一整套非常丰富的开发生态，由于MCP是一套标准协议，任何MCP工具（MCP Server）都可以无缝接入任何MCP智能体中，也就是你开发的工具我也能用，我开发的工具你也能直接用。

![](img/d5d7e99ada33b81155e70fa2ed93ba3c.png)

![](img/7fe4da26908da8966ed1bd5f3aca2c30.png)

## 2. MCP参考公开课

+   《7分钟讲清楚MCP是什么？》：https://www.bilibili.com/video/BV1uXQzYaEpJ/

![](img/28c22ca79d42637f3fadce70a9032139.png)

+   《从零到一，MCP技术开发入门实战！》：https://www.bilibili.com/video/BV1NLXCYTEbj/

![](img/a7964365d4b80d931d19103a2c1fce39.png)

+   《MCP企业级智能体开发实战！》：https://www.bilibili.com/video/BV1n1ZuYjEzf/

![](img/d3719bdcf833288180e10c04ef6f9fca.png)

> 从零到一MCP入门与智能体开发进阶课件合集：

![](img/06931e4f4d8335e854abd59c0a55102f.png)

> 下图扫码即可领取：

![](img/4ef281b9a18ee7e58d5e5385ac93e32a.png)

## 3. MCP基础实践流程

接下来，我们先尝试手动实现一遍MCP实践流程，然后再考虑将已经部署好的server带入Agents SDK中，作为tools进行调用。

一个极简的天气查询MCP调用流程如下：

![](img/757fa41370d839250117d2a96cc76a4c.png)

+   借助`uv`创建MCP运行环境

方法 1：使用 `pip` 安装（适用于已安装 `pip` 的系统）

```
pip install uv
```

方法 2：使用 `curl` 直接安装

如果你的系统没有 pip，可以直接运行：

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

这会自动下载 `uv` 并安装到 `/usr/local/bin`。

+   创建 MCP 客户端项目

```
# 创建项目目录
uv init mcp-client
cd mcp-client
```

![](img/969a290b9eb3588179091e33fa8d1a1e.png)

![](img/8bd9e799b9d66f0b1ce97f65e177edb4.png)

+   创建MCP客户端虚拟环境

```
# 创建虚拟环境
uv venv

# 激活虚拟环境
source .venv/bin/activate
```

![](img/9ebf800e4ee14f24aec2bdea2edaaa2e.png)

然后即可通过`add`方法在虚拟环境中安装相关的库。

```
# 安装 MCP SDK
uv add mcp openai python-dotenv httpx
```

![](img/3174d20098ec99b057ffbbbaf55f5ac1.png)

+   编写用于天气查询的server服务器代码：

这里我们需要在服务器上创建一个`server.py`，并写入如下代码：

```
import json
import httpx
from typing import Any
from mcp.server.fastmcp import FastMCP

# 初始化 MCP 服务器
mcp = FastMCP(&quot;WeatherServer&quot;)

# OpenWeather API 配置
OPENWEATHER_API_BASE = &quot;https://api.openweathermap.org/data/2.5/weather&quot;
API_KEY = &quot;YOUR_API_KEY&quot;  # 请替换为你自己的 OpenWeather API Key
USER_AGENT = &quot;weather-app/1.0&quot;

async def fetch_weather(city: str) -&gt; dict[str, Any] | None:
    &quot;&quot;&quot;
    从 OpenWeather API 获取天气信息。
    :param city: 城市名称（需使用英文，如 Beijing）
    :return: 天气数据字典；若出错返回包含 error 信息的字典
    &quot;&quot;&quot;
    params = {
        &quot;q&quot;: city,
        &quot;appid&quot;: API_KEY,
        &quot;units&quot;: &quot;metric&quot;,
        &quot;lang&quot;: &quot;zh_cn&quot;
    }
    headers = {&quot;User-Agent&quot;: USER_AGENT}

    async with httpx.AsyncClient() as client:
        try:
            response = await client.get(OPENWEATHER_API_BASE, params=params, headers=headers, timeout=30.0)
            response.raise_for_status()
            return response.json()  # 返回字典类型
        except httpx.HTTPStatusError as e:
            return {&quot;error&quot;: f&quot;HTTP 错误: {e.response.status_code}&quot;}
        except Exception as e:
            return {&quot;error&quot;: f&quot;请求失败: {str(e)}&quot;}

def format_weather(data: dict[str, Any] | str) -&gt; str:
    &quot;&quot;&quot;
    将天气数据格式化为易读文本。
    :param data: 天气数据（可以是字典或 JSON 字符串）
    :return: 格式化后的天气信息字符串
    &quot;&quot;&quot;
    # 如果传入的是字符串，则先转换为字典
    if isinstance(data, str):
        try:
            data = json.loads(data)
        except Exception as e:
            return f&quot;无法解析天气数据: {e}&quot;

    # 如果数据中包含错误信息，直接返回错误提示
    if &quot;error&quot; in data:
        return f&quot;⚠️ {data[&#x27;error&#x27;]}&quot;

    # 提取数据时做容错处理
    city = data.get(&quot;name&quot;, &quot;未知&quot;)
    country = data.get(&quot;sys&quot;, {}).get(&quot;country&quot;, &quot;未知&quot;)
    temp = data.get(&quot;main&quot;, {}).get(&quot;temp&quot;, &quot;N/A&quot;)
    humidity = data.get(&quot;main&quot;, {}).get(&quot;humidity&quot;, &quot;N/A&quot;)
    wind_speed = data.get(&quot;wind&quot;, {}).get(&quot;speed&quot;, &quot;N/A&quot;)
    # weather 可能为空列表，因此用 [0] 前先提供默认字典
    weather_list = data.get(&quot;weather&quot;, [{}])
    description = weather_list[0].get(&quot;description&quot;, &quot;未知&quot;)

    return (
        f&quot;🌍 {city}, {country}\n&quot;
        f&quot;🌡 温度: {temp}°C\n&quot;
        f&quot;💧 湿度: {humidity}%\n&quot;
        f&quot;🌬 风速: {wind_speed} m/s\n&quot;
        f&quot;🌤 天气: {description}\n&quot;
    )

@mcp.tool()
async def query_weather(city: str) -&gt; str:
    &quot;&quot;&quot;
    输入指定城市的英文名称，返回今日天气查询结果。
    :param city: 城市名称（需使用英文）
    :return: 格式化后的天气信息
    &quot;&quot;&quot;
    data = await fetch_weather(city)
    return format_weather(data)

if __name__ == &quot;__main__&quot;:
    # 以标准 I/O 方式运行 MCP 服务器
    mcp.run(transport=&#x27;stdio&#x27;)
```

![](img/dfa102ed2d29bf8b228f6af2c352cda6.png)

+   天气查询客户端client创建流程

然后创建一个可以和server进行通信的客户端，需要注意的是，该客户端需要包含大模型调用的基础信息。我们需要编写一个`client.py`脚本，聂荣如下：
