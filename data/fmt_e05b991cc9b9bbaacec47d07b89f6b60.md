
# Chatchat 架构与跨域问题解决

## 架构概述

chatchat整个架构是：[langchain](https://link.zhihu.com/?target=https%3A//github.com/langchain-ai/langchain)框架支持通过基于 [FastAPI](https://link.zhihu.com/?target=https%3A//github.com/tiangolo/fastapi)[提供的 API 调用](https://link.zhihu.com/?target=https%3A//github.com/streamlit/streamlit)服务，或使用基于 Streamlit 的 WebUI 进行操作。

所以由FastAPI提供所有server的服务，然后webUI这边是独立运行，同时调用FastAPI。

## 跨域问题表现

如果有跨域问题可能会出现：

+ web端一直显示please wait

+ 无法建立websocket链接

> 关于跨域问题，笔者其实不是特别懂，不过听一位前辈形象提过，

> 不同的服务调用就像两个品牌（比如：东北馆子，麦当劳），你在麦当劳要吃锅包肉，麦当劳员工问了旁边的东北馆子，人家不想卖你锅包肉

> 所以，如果要想满足客户需求，东北馆子就需要设置允许调货的命令

## FastAPI 跨域中间件配置

其中，FastAPI在文件`Langchain-Chatchat/startup.py`通过`app.add_middleware`设置了跨域：

```
def create_openai_api_app(
        controller_address: str,
        api_keys: List = [],
        log_level: str = "INFO",
) -> FastAPI:
    import fastchat.constants
    fastchat.constants.LOGDIR = LOG_PATH
    from fastchat.serve.openai_api_server import app, CORSMiddleware, app_settings
    from fastchat.utils import build_logger
    logger = build_logger("openai_api", "openai_api.log")
    logger.setLevel(log_level)

    app.add_middleware(
        CORSMiddleware,
        allow_credentials=True,
        allow_origins=["*"],
        allow_methods=["*"],
        allow_headers=["*"],
    )

    sys.modules["fastchat.serve.openai_api_server"].logger = logger
    app_settings.controller_address = controller_address
    app_settings.api_keys = api_keys

    MakeFastAPIOffline(app)
    app.title = "FastChat OpeanAI API Server"
    return app
```

## WebUI 启动方式

那么webui在`Langchain-Chatchat/startup.py`是通过`cmd`直接跑的，所以跟 FastAPI是独立结构。

```
def run_webui(started_event: mp.Event = None, run_mode: str = None):
    from server.utils import set_httpx_config
    set_httpx_config()

    host = WEBUI_SERVER["host"]
    port = WEBUI_SERVER["port"]

    cmd = ["streamlit", "run", "webui.py",
            "--server.address", host,
            "--server.port", str(port),
            "--theme.base", "light",
            "--theme.primaryColor", "#165dff",
            "--theme.secondaryBackgroundColor", "#f5f5f5",
            "--theme.textColor", "#000000",
        ]
    if run_mode == "lite":
        cmd += [
            "--",
            "lite",
        ]
    p = subprocess.Popen(cmd)
    started_event.set()
    p.wait()
```

## Streamlit 跨域问题排查

[那](https://link.zhihu.com/?target=https%3A//github.com/chatchat-space/Langchain-Chatchat/issues/1397)么针对Streamlit的跨域，在issue提到了：https://github.com/chatchat-space/Langchain-Chatchat/issues/1397

```
跨域问题，已解决
在startup.py中进行修改
p = subprocess.Popen(["streamlit", "run", "webui.py",
"--server.enableCORS", "false",
"--server.address", host,
"--server.port", str(port)])
```

但是在 [云托管docker部署streamlit后无法建立we](https://link.zhihu.com/?target=https%3A//developers.weixin.qq.com/community/develop/doc/0006607719cd80a92f60b38446b000%3FhighLine%3D%2525E5%2525BE%2525AE%2525E4%2525BF%2525A1%2525E5%25258C%2525BA%2525E5%2525A4%2525A9%2525E4%2525B9%25259D%2525E8%2525A6%252581%2525E5%2525BB%2525BA%2525E7%2525BE%2525A4%2525E8%2525B0%252581%2525E6%25259C%252589%2525E9%252593%2525BE%2525E6%25258E%2525A5%2525E3%252580%252590%2525E2%252595%252585%2525E8%252596%252587%2525EF%2525BC%25259B11511989)bsocket链接？ | 🛰️开放社区 也提到了，设置了也无法使用：

```
尝试过在streamlit run后面加--server.enableXsrfProtection=false --server.enableCORS=false --server.enableWebsocketCompression=false --browser.serverAddress=公网域名 --server.port=80 中的一个或几个都没用，当然改server.port也会对应修改EXPOSE端口号和流水线的端口号。

尝试把https改成http后访问也没用。
```

笔者自己尝试的时候，单独设置 `"--server.enableCORS", "false",` 会出现提示：

```
Warning: the config option 'server.enableCORS=false' is not
 compatible with 'server.enableXsrfProtection=true'.
As a result, 'server.enableCORS' is being overridden to 'true'.

More information:
In order to protect against CSRF attacks, we send a cookie with each request.
To do so, we must specify allowable origins, which places a restriction on
cross-origin resource sharing.

If cross origin resource sharing is required, please disable server.enableXsrfProtection.
```

然后参考streamlit官方的 configuration 信息，Configuration - Streamlit Docs ，其中有记录：

```
# Enables support for Cross-Origin Resource Sharing (CORS) protection, for
# added security.
# Due to conflicts between CORS and XSRF, if `server.enableXsrfProtection` is
# on and `server.enableCORS` is off at the same time, we will prioritize
# `server.enableXsrfProtection`.
# Default: true
enableCORS = true

# Enables support for Cross-Site Request Forgery (XSRF) protection, for added
# security.
# Due to conflicts between CORS and XSRF, if `server.enableXsrfProtection` is
# on and `server.enableCORS` is off at the same time, we will prioritize
# `server.enableXsrfProtection`.
# Default: true
enableXsrfProtection = true
```

## 最终解决方案

所以笔者最终的使用是在`Langchain-Chatchat/startup.py`中加了`server.enableCORS`和`server.enableXsrfProtection`的 false 选项：

```
def run_webui(started_event: mp.Event = None, run_mode: str = None):
    from server.utils import set_httpx_config
    set_httpx_config()

    host = WEBUI_SERVER["host"]
    port = WEBUI_SERVER["port"]

    cmd = ["streamlit", "run", "webui.py",
            "--server.address", host,
            "--server.port", str(port),
            "--theme.base", "light",
            "--server.enableCORS", "false",
            "--server.enableXsrfProtection", "false",
            "--theme.primaryColor", "#165dff",
            "--theme.secondaryBackgroundColor", "#f5f5f5",
            "--theme.textColor", "#000000",
        ]
    if run_mode == "lite":
        cmd += [
            "--",
            "lite",
        ]
    p = subprocess.Popen(cmd)
    started_event.set()
    p.wait()
```
