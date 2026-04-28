# 流式HTTP MCP服务器开发指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/XdGidyv1IorjcsxXYqFcJOfgnnh](https://kq4b3vgg5b.feishu.cn/docx/XdGidyv1IorjcsxXYqFcJOfgnnh)

长期以来，MCP工具的异地通信主要是以SSE传输方式为主，这种通信方法既没有多通道并发同时也不够稳定，很难真正的适用于企业级应用场景。因此，在2月初，在MCP官方GitHub项目主页上，就有开发者提出了采用更先进的流式HTTP传输方法代替SSE传输的技术方案，相比SSE传输，HTTP流式传输并发更高、通信更加稳定，同时也更容易集成和部署，这也是当代服务器与客户端异地通信的最佳解决方案。

![](img/7aa382f7b343cd8af90f8719176f489c.png)

但流式HTTP MCP服务器功能复杂，相关SDK的研发难度也很高，因此在今年的3月，MCP官方首先发布了MCP的流式HTTP通信协议，其中详细说明了HTTP流式传输的基本协议。

![](img/6a7e167e23eeb253004f479deae830af.png)

> https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2025-03-26/basic/transports.mdx#streamable-http

然后在5月9号的1.8.0版本更新中，正式在SDK中加入了HTTP流式MCP服务器的相关功能支持。自此开发者就可以通过MCP SDK，高效快速开发流式HTTP MCP服务器，并顺利进行多通道并发的企业级MCP工具部署。毫无疑问，这将是MCP技术迈向企业级应用的至关重要的一步。

![](img/643f26c88246b2b35d878945c86797ad.png)

> https://github.com/modelcontextprotocol/python-sdk/releases/tag/v1.8.0

本期公开课，就为大家从零介绍流式HTTP MCP服务器开发、测试、部署上线的全流程，并提供完整MCP服务器功能测试客户端脚本。公开课完Inspector整课件扫码即可领取。

![](img/fd20b5a70dc6d30b3543a89f975ba579.png)

## 一、基于HTTP流式传输的MCP服务器开发流程

+   创建项目文件：

```
# cd /root/autodl-tmp/MCP
uv init mcp-weather-http
cd mcp-weather-http

# 创建虚拟环境
uv venv

# 激活虚拟环境
source .venv/bin/activate

uv add mcp httpx
```

![](img/6835ca17c2d176bdce5ad212f8d1882d.png)

这里我们采用src_layer的风格进行项目文件编排，因此需要删除main.py，并创建mcp_weather_http目录。

```
mkdir -p ./src/mcp_weather_http
cd ./src/mcp_weather_http
```

然后创建在src/mcp_weather_http中创建三个代码文件：

![](img/0960bf1c069691ef73d6328eb4fcdccd.png)

并在__init__.py中写入

```
from .server import main
```

而在__main__.py中写入：

```
from mcp_weather_http import main

main()
```

然后在server.py中写入如下代码：

```
import contextlib
import logging
import os
from collections.abc import AsyncIterator

import anyio
import click
import httpx
import mcp.types as types
from mcp.server.lowlevel import Server
from mcp.server.streamable_http_manager import StreamableHTTPSessionManager
from starlette.applications import Starlette
from starlette.routing import Mount
from starlette.types import Receive, Scope, Send

# ---------------------------------------------------------------------------
# Weather helpers
# ---------------------------------------------------------------------------
OPENWEATHER_URL = &quot;https://api.openweathermap.org/data/2.5/weather&quot;
DEFAULT_UNITS = &quot;metric&quot;  # use Celsius by default
DEFAULT_LANG = &quot;zh_cn&quot;  # Chinese descriptions


async def fetch_weather(city: str, api_key: str) -&gt; dict[str, str]:
    &quot;&quot;&quot;Call OpenWeather API and return a simplified weather dict.

    Raises:
        httpx.HTTPStatusError: if the response has a non-2xx status.
    &quot;&quot;&quot;
    params = {
        &quot;q&quot;: city,
        &quot;appid&quot;: api_key,
        &quot;units&quot;: DEFAULT_UNITS,
        &quot;lang&quot;: DEFAULT_LANG,
    }
    async with httpx.AsyncClient(timeout=10) as client:
        r = await client.get(OPENWEATHER_URL, params=params)
        r.raise_for_status()
        data = r.json()
    # Extract a concise summary
    weather_main = data[&quot;weather&quot;][0][&quot;main&quot;]
    description = data[&quot;weather&quot;][0][&quot;description&quot;]
    temp = data[&quot;main&quot;][&quot;temp&quot;]
    feels_like = data[&quot;main&quot;][&quot;feels_like&quot;]
    humidity = data[&quot;main&quot;][&quot;humidity&quot;]
    return {
        &quot;city&quot;: city,
        &quot;weather&quot;: weather_main,
        &quot;description&quot;: description,
        &quot;temp&quot;: f&quot;{temp}°C&quot;,
        &quot;feels_like&quot;: f&quot;{feels_like}°C&quot;,
        &quot;humidity&quot;: f&quot;{humidity}%&quot;,
    }


@click.command()
@click.option(&quot;--port&quot;, default=3000, help=&quot;Port to listen on for HTTP&quot;)
@click.option(
    &quot;--api-key&quot;,
    envvar=&quot;OPENWEATHER_API_KEY&quot;,
    required=True,
    help=&quot;OpenWeather API key (or set OPENWEATHER_API_KEY env var)&quot;,
)
@click.option(
    &quot;--log-level&quot;,
    default=&quot;INFO&quot;,
    help=&quot;Logging level (DEBUG, INFO, WARNING, ERROR, CRITICAL)&quot;,
)
@click.option(
    &quot;--json-response&quot;,
    is_flag=True,
    default=False,
    help=&quot;Enable JSON responses instead of SSE streams&quot;,
)
def main(port: int, api_key: str, log_level: str, json_response: bool) -&gt; int:
    &quot;&quot;&quot;Run an MCP weather server using Streamable HTTP transport.&quot;&quot;&quot;

    # ---------------------- Configure logging ----------------------
    logging.basicConfig(
        level=getattr(logging, log_level.upper()),
        format=&quot;%(asctime)s - %(name)s - %(levelname)s - %(message)s&quot;,
    )
    logger = logging.getLogger(&quot;weather-server&quot;)

    # ---------------------- Create MCP Server ----------------------
    app = Server(&quot;mcp-streamable-http-weather&quot;)

    # ---------------------- Tool implementation -------------------
    @app.call_tool()
    async def call_tool(name: str, arguments: dict) -&gt; list[types.TextContent]:
        &quot;&quot;&quot;Handle the &#x27;get-weather&#x27; tool call.&quot;&quot;&quot;
        ctx = app.request_context
        city = arguments.get(&quot;location&quot;)
        if not city:
            raise ValueError(&quot;&#x27;location&#x27; is required in arguments&quot;)

        # Send an initial log message so the client sees streaming early.
        await ctx.session.send_log_message(
            level=&quot;info&quot;,
            data=f&quot;Fetching weather for {city}…&quot;,
            logger=&quot;weather&quot;,
            related_request_id=ctx.request_id,
        )

        try:
            weather = await fetch_weather(city, api_key)
        except Exception as err:
            # Stream the error to the client and re-raise so MCP returns error.
            await ctx.session.send_log_message(
                level=&quot;error&quot;,
                data=str(err),
                logger=&quot;weather&quot;,
                related_request_id=ctx.request_id,
            )
            raise

        # Stream a success notification (optional)
        await ctx.session.send_log_message(
            level=&quot;info&quot;,
            data=&quot;Weather data fetched successfully!&quot;,
            logger=&quot;weather&quot;,
            related_request_id=ctx.request_id,
        )

        # Compose human-readable summary for the final return value.
        summary = (
            f&quot;{weather[&#x27;city&#x27;]}：{weather[&#x27;description&#x27;]}，温度 {weather[&#x27;temp&#x27;]}，&quot;
            f&quot;体感 {weather[&#x27;feels_like&#x27;]}，湿度 {weather[&#x27;humidity&#x27;]}。&quot;
        )

        return [
            types.TextContent(type=&quot;text&quot;, text=summary),
        ]

    # ---------------------- Tool registry -------------------------
    @app.list_tools()
    async def list_tools() -&gt; list[types.Tool]:
        &quot;&quot;&quot;Expose available tools to the LLM.&quot;&quot;&quot;
        return [
            types.Tool(
                name=&quot;get-weather&quot;,
                description=&quot;查询指定城市的实时天气（OpenWeather 数据）&quot;,
                inputSchema={
                    &quot;type&quot;: &quot;object&quot;,
                    &quot;required&quot;: [&quot;location&quot;],
                    &quot;properties&quot;: {
                        &quot;location&quot;: {
                            &quot;type&quot;: &quot;string&quot;,
                            &quot;description&quot;: &quot;城市的英文名称，如 &#x27;Beijing&#x27;&quot;,
                        }
                    },
                },
            )
        ]

    # ---------------------- Session manager -----------------------
    session_manager = StreamableHTTPSessionManager(
        app=app,
        event_store=None,  # 无状态；不保存历史事件
        json_response=json_response,
        stateless=True,
    )

    async def handle_streamable_http(scope: Scope, receive: Receive, send: Send) -&gt; None:  # noqa: D401,E501
        await session_manager.handle_request(scope, receive, send)

    # ---------------------- Lifespan Management --------------------
    @contextlib.asynccontextmanager
    async def lifespan(app: Starlette) -&gt; AsyncIterator[None]:
        async with session_manager.run():
            logger.info(&quot;Weather MCP server started! 🚀&quot;)
            try:
                yield
            finally:
                logger.info(&quot;Weather MCP server shutting down…&quot;)

    # ---------------------- ASGI app + Uvicorn ---------------------
    starlette_app = Starlette(
        debug=False,
        routes=[Mount(&quot;/mcp&quot;, app=handle_streamable_http)],
        lifespan=lifespan,
    )

    import uvicorn

    uvicorn.run(starlette_app, host=&quot;0.0.0.0&quot;, port=port)

    return 0


if __name__ == &quot;__main__&quot;:
    main()
```

代码解释如下：

1.  **引入各种功能模块**

```
import contextlib
import logging
import os
from collections.abc import AsyncIterator

import anyio
import click
import httpx
import mcp.types as types
from mcp.server.lowlevel import Server
from mcp.server.streamable_http_manager import StreamableHTTPSessionManager
from starlette.applications import Starlette
from starlette.routing import Mount
from starlette.types import Receive, Scope, Send
```

💡解**释：**

+   这些都是“工具箱”，你可以理解为在写一个天气小程序之前，先导入一些现成的功能组件：

+   httpx: 用来联网查天气。

+   mcp: 提供了让大模型调用工具的能力。

+   starlette: 帮你搭建 HTTP 网络服务。

+   click: 帮你写命令行参数，比如 --api-key=xxx。

+   其它模块则帮助你处理异步任务、打印日志、读取环境变量等。

1.  **查询天气的函数**

```
async def fetch_weather(city: str, api_key: str) -&gt; dict[str, str]:
```

💡解**释：**

+   这个函数是你天气服务器的**核心功能**——根据城市名去 [OpenWeather](https://openweathermap.org/) 网站查询天气。

+   它用 httpx.AsyncClient 这个工具联网发请求，返回的结果包括：天气情况、气温、湿度等等。

1.  **启动服务器的主函数 ****main()**

```
@click.command()
@click.option(&quot;--port&quot;, default=3000, ...)
@click.option(&quot;--api-key&quot;, envvar=&quot;OPENWEATHER_API_KEY&quot;, ...)
def main(port, api_key, ...):
```

💡解**释：**

+   这是你运行服务器的“入口”，你以后运行时只需要：

```
python weather_mcp_streamable_http_server.py --port 3000 --api-key xxxx
```

+   click 是个很好用的命令行工具，帮你接收参数；

+   api_key 可以通过环境变量设置，方便又安全。

1.  MCP工具注册：让大模型能调用 get-weather

```
@app.call_tool()
async def call_tool(name: str, arguments: dict):
```

💡解**释：**

+   这就是你注册给大语言模型用的“工具”，名字叫 get-weather。

+   当模型说“我需要查北京的天气”，MCP就会触发这个函数。

+   你在里面解析出 "location" 字段，然后调用上面那个 fetch_weather() 函数拿数据。

它还有一个 **亮点功能**：会实时往客户端推送消息：

```
await ctx.session.send_log_message(...)
```

这些就是所谓的 **“流式日志通知”**（比如“正在查询…”、“成功了！”）。

1.  MCP告诉模型有哪些工具（工具列表）

```
@app.list_tools()
async def list_tools() -&gt; list[types.Tool]:
```

💡解**释：**

+   MCP 会定期问你：“你这里都有哪些工具可以用？”

+   你就返回一个叫 get-weather 的工具描述，告诉模型：

+   输入需要一个 location，是城市名。

1.  **流式会话管理器配置**

```
session_manager = StreamableHTTPSessionManager(
    app=app,
    event_store=None,
    json_response=json_response,
    stateless=True,
)
```

💡解**释：**

+   这一句创建了 MCP 的“HTTP 会话处理中心”，负责处理所有 /mcp 路由的请求；

+   stateless=True 表示**不保存历史对话**，每次都是新请求；

+   json_response=False 表示用流式 SSE（你也可以改成一次性 JSON 响应）。

1.  构建 Starlette Web 应用

```
starlette_app = Starlette(
    routes=[Mount(&quot;/mcp&quot;, app=handle_streamable_http)],
    lifespan=lifespan,
)
```

💡解**释：**

+   这是将 MCP 服务挂载到你的网站 /mcp 路径上；

+   用户访问这个路径时，就会进入刚才创建的 MCP 会话管理器。

1.  **启动服务器**

```
import uvicorn
uvicorn.run(starlette_app, host=&quot;0.0.0.0&quot;, port=port)
```

💡解**释：**

+   这行代码启动你的 MCP HTTP 服务器，监听指定端口；

+   如果你传的是 --port 3000，访问路径就是：

```
http://localhost:3000/mcp
```

![](img/8a29f0dee4ab6c4be60858d40b455019.png)

同时需要回到主目录，修改项目配置文件pyproject.toml：

```
[build-system]
requires = [&quot;setuptools&gt;=61.0&quot;, &quot;wheel&quot;]
build-backend = &quot;setuptools.build_meta&quot;

[project]
name = &quot;mcp-weather-http&quot;
version = &quot;1.1.0&quot;
description = &quot;输入OpenWeather-API-KEY，获取天气信息。&quot;
readme = &quot;README.md&quot;
requires-python = &quot;&gt;=3.9&quot;
dependencies = [
    &quot;httpx&gt;=0.28.1&quot;,
    &quot;mcp&gt;=1.8.0&quot;,
]

[project.scripts]
mcp-weather-http = &quot;mcp_weather_http:main&quot;

[tool.setuptools]
package-dir = {&quot;&quot; = &quot;src&quot;}

[tool.setuptools.packages.find]
where = [&quot;src&quot;]
```

至此即完成了整个项目的代码编写工作。完整项目代码可在网盘中领取：

![](img/6b9a8dd6707cfe317f9a8f35cadc61ae.png)

![](img/16237bba16d2f1e28e45df3a7fb07b9e.png)

## 二、HTTP流式传输MCP服务器开启与测试

在创建完server.py后，我们可以开启服务并进行测试。需要注意的是，我们需要先开启流式HTTP MCP服务器，然后再开启Inspector：

> 在stdio模式下是开启Inspector时同步开启MCP Server

+   开启流式HTTP MCP服务器：

```
# 回到项目主目录
# cd /root/autodl-tmp/MCP/mcp-weather-http

uv run ./src/mcp_weather_http/server.py --api-key YOUR_KEY
```

![](img/cf25a9bdfa4fbae38f1f81e30636a203.png)

+   开启Inspector

```
# 回到项目主目录
# cd /root/autodl-tmp/MCP/mcp-weather-http
# source .venv/bin/activate

npx -y @modelcontextprotocol/inspector uv run main.py --api-key YOUR_KEY
```

![](img/da26ccdbfcb2c6c262b1f87cac781f5a.png)

+   进行测试

+   同样，如果是使用AutoDL，则先需要使用隧道工具将端口映射到本地进行运行：

![](img/3e3a22b6339ead733126585ad382ab05.png)

然后打开Inspector，并选择HTTP流式模式，选择默认运行地址：http://localhost:3000/mcp，然后点击connect：

![](img/825b83ae9e0944ef487dbc42fe8a6c7d.png)

然后点击List Tools：

![](img/c929a55184e548f95e6b91b7bf0018d5.png)

然后点击get-weather：

![](img/57f0f48e49056217136032b753b76cdd.png)

然后输入地名进行测试：

![](img/53e575496e01dc3968018276a89aaa62.png)

若能正常返回结果，则说明服务器运行正常。

![](img/60e88d265a0f9eb0967dc4d44f4d1077.png)

完整测试流程如下所示：

## 三、流式HTTP MCP服务器异地调用

接下来即可在异地环境（也可以是本地）通过HTTP方式调用MCP服务了。这里以本地安装的Cherry Studio为例，进行调用演示。此处需要保持远程流式HTTP MCP服务器处于开启状态，然后按照如下流程进行调用。

首先点击添加服务器：

![](img/ebc48ea91abec1fb8f6c7a646389a3d6.png)

然后输入服务器名称mcp-weather-http，并选择流式传输类型，并选择服务器地址：http://localhost:3000/mcp ，然后点击保存：

![](img/17ea9d8732a7b0ae45fb396c9131a1b1.png)

若显示服务器更新成功，则表示已经连接上MCP服务器：

![](img/69fb3759572b0aed5ffbea42185d1369.png)

此时后台显示如下：

![](img/ec320783f850bc34dd4015143ad3a494.png)

然后即可进行对话测试，这里回到对话页面，选择MCP服务器，

![](img/1751f25d545c601cb03a65511d2e8481.png)

然后进行天气查询测试：请问北京和杭州今天天气如何？，然后即可查看回答结果：

![](img/c7cf9ce834f52d696aa0ee028cb8a8d0.png)

完整设置与调用流程演示如下：

## 四、流式HTTP MCP服务器发布流程

测试完成后，即可上线发布。这里仍然考虑发布到pypi平台，并使用cherry studio进行本地调用测试。完整项目代码可在网盘中领取：

![](img/f20a134e1748a70a60355074a7808b6e.png)

![](img/4a96f38f4c487a132ce500090483a9e7.png)

+   打包上传：

```
# 回到项目主目录
# cd /root/autodl-tmp/MCP/mcp-weather-http

uv pip install build twine
python -m build
python -m twine upload dist/*
```

![](img/da4bffd0b4736bcb7ca7cec182065541.png)

+   查看发布的库：https://pypi.org/search/?q=mcp-weather-http&o=

![](img/7ca2d914dfbd8c62237b126c12c57416.png)

+   本地安装：

```
pip install mcp-weather-http
```

![](img/0b4e43e25d0fb3ffdd8c2efa00ef36cc.png)

![](img/f9ba5c708f44bf8be2f9c14be6ddc6d1.png)

+   开启服务：uv run mcp-weather-http --api-key YOUR_API_KEY

![](img/52f3225fb17a377ba25ca7bf62ada5c0.png)

然后即可打开浏览器输入http://localhost:3000/mcp测试连接情况

![](img/f913afdba8974071f076acedde981ede.png)

> 注意，这里在服务器上开启服务然后本地连接也可以，和stdio不同，HTTP模式下的MCP服务器并不需要本地运行。

+   使用Cherry studio进行连接

然后即可使用Cherry studio连接流式HTTP模式下的MCP服务器，还是和此前一样的连接流程，输入服务器名称mcp-weather-http，并选择流式传输类型，并选择服务器地址：http://localhost:3000/mcp ，然后点击保存：

![](img/ad32848de56a769141db92ba8572aed1.png)

若显示服务器更新成功，则表示已经连接上MCP服务器：

![](img/7900a7fa35911929c4b44590927d47d2.png)

此时本地环境后台显示内容如下：

![](img/3739feee8ca86ce6299460e7c114fe03.png)

接下来即可进行对话测试：

![](img/e74bfa5b568ad688595401a400943984.png)

至此，我们就完成了流式HTTP MCP工具的发布、下载与调用测试流程。