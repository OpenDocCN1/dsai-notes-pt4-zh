
```
import os
from dotenv import load_dotenv 
from langchain_deepseek import ChatDeepSeek
from typing import Annotated
from typing_extensions import TypedDict
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages
from langchain.chat_models import init_chat_model
from langgraph.prebuilt import create_react_agent
from langchain_mcp_adapters.client import MultiServerMCPClient
from langchain_core.tools import tool
from pydantic import BaseModel, Field
import requests,json

# 加载环境变量
load_dotenv(override=True)

# 内置搜索工具
search_tool = TavilySearch(max_results=5, topic="general")

class WeatherQuery(BaseModel):
    loc: str = Field(description="The location name of the city")

@tool(args_schema = WeatherQuery)
def get_weather(loc):
    """
    查询即时天气函数
    :param loc: 必要参数，字符串类型，用于表示查询天气的具体城市名称，\
    注意，中国的城市需要用对应城市的英文名称代替，例如如果需要查询北京市天气，则loc参数需要输入'Beijing'；
    :return：OpenWeather API查询即时天气的结果，具体URL请求地址为：https://api.openweathermap.org/data/2.5/weather\
    返回结果对象类型为解析之后的JSON格式对象，并用字符串形式进行表示，其中包含了全部重要的天气信息
    """
    # Step 1.构建请求
    url = "https://api.openweathermap.org/data/2.5/weather"

    # Step 2.设置查询参数
    params = {
        "q": loc,               
        "appid": os.getenv("OPENWEATHER_API_KEY"),    # 输入API key
        "units": "metric",            # 使用摄氏度而不是华氏度
        "lang":"zh_cn"                # 输出语言为简体中文
    }

    # Step 3.发送GET请求
    response = requests.get(url, params=params)
    
    # Step 4.解析响应
    data = response.json()
    return json.dumps(data)

tools = [search_tool, get_weather]

# 创建模型
model = ChatDeepSeek(model="deepseek-chat")

# 创建图
graph = create_react_agent(model=model, tools=tools)
```

![](img/a5558a230c72f3b21298b33c33e0d62b.png)

+   **Step 6. 创建** `langgraph.json` 文件

在`langgraph_chatbot`文件夹中，新建一个`langgraph.json`文件，在该json文件中配置项目信息，遵循规范如下所示：

+   必须包含 `dependencies` 和 `graphs` 字段

+   `graphs` 字段格式：`"图名": "文件路径:变量名"`

+   配置文件必须放在与Python文件同级或更高级的目录

注意: 项目文件的名称必须为`langgraph.json`。如下所示：

![](img/2397d39be918b2b3d9aa085fc09b923c.png)

其中：

+   `dependencies`: `["./"]` - 告诉LangGraph在当前目录查找依赖项（会自动读取`requirements.txt`）

+   `chatbot`: `"./graph.py:graph"` - 定义图名为`chatbot`，来自`graph.py`文件中的`graph`变量

+   `env`: `".env"` - 指定环境变量文件位置

最终完整项目结构如下所示：

```
    ./langraph_chatbot/
    ├── graph.py              # 对应官方的 agent.py
    ├── requirements.txt      # ✅ 依赖管理
    ├── langgraph.json       # ✅ 配置文件
    └── .env                 # ✅ 环境变量
```

![](img/d1b4e3f4c0a9b4ca72d6e16d1eacf253.png)

完整的代码已经上传至百度网盘中的`langgraph_chatbot`文件夹中，大家可以扫描下方二维码免费领取

![](img/2400578b57d3352469e9fce38aba2da1.png)

![](img/7269fc8d240be95b567fd279a63bedef.png)

+   **Step 7. 安装** `langgraph-cli` 以及其他依赖

然后，安装`langgraph-cli`依赖，执行如下代码：

```
    pip install -U "langgraph-cli[inmem]" -i https://pypi.tuna.tsinghua.edu.cn/simple
```

![](img/4bbaad9129746dab9b62e09bda05673a.png)

然后进入到`langgraph_chatbot`文件夹，安装相关基础依赖：

```
pip isntall -r requirements.txt
```

最后，进入到`langgraph_chatbot`文件夹，执行`langgraph dev`即可启动项目

```
   langgraph dev
```

![](img/4146e0425493bcedb12d34c6bac0f461.png)

启动成功后能看到三个连接，其中第一个连接是当前部署完成后的服务端口，第二个是LangGraph Studio的可视化页面，第三个端口是端口说明。

### 3. LangGraph Agent部署后调用流程

+   后端服务接口

这里我们首先可以看下第三个连接，其中包含了详细的接口调用方法：

![](img/2afc7a2eee68019c31ba835972ac59cd.png)

这些暴露的接口和调用方法，接下来就可以用于进行进一步开发和测试。

+   LangGraph Studio

然后我们可点击Studio UI中显示的链接，在浏览器中打开并访问Studio，如下所示：

![](img/e9f0118f18fc8b72dd1b56e5642e9f71.png)

完整演示流程如下所示：

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/2025-06-26 19-29-04.mp4", width=800, height=400)
```

Your browser does not support the video element.

+   LangSmith

而如果此前设置了追踪，此时就能在LangSmith中看到当前项目运行情况：

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/2025-06-26 19-52-57.mp4", width=800, height=400)
```

Your browser does not support the video element.

### 4. LangGraph Agent后端接入Agent Chat UI完整流程

+   项目主页：https://github.com/langchain-ai/agent-chat-ui

![](img/6fe1ff089abc468127d570f81e55196d.png)

+   **Step 1. 克隆项目：**

```
git clone https://github.com/langchain-ai/agent-chat-ui.git

cd agent-chat-ui
```

![](img/1292fc0504468937bce6a78e4b06efe1.png)

![](img/079b97d1c010e84a305678353185ad83.png)

![](img/7653c15c64786e4a9ab3ffae0247f92e.png)

+   **Step 2. 安装** `npm`

node.js官网：https://nodejs.org

![](img/e6d2d2f2aad5b76f287cf7c664de37e9.png)

![](img/49fcd55ca06762bebf276ad5fd9fdd57.png)

```
npm install -g pnpm
pnpm -v
```

+   **Step 3. 安装前端项目依赖**

```
pnpm install
```

+   **Step 4. 开启** `Chat Agent UI`

```
pnpm dev
```

![](img/83deabb2541aacc98fa4b6174f721a58.png)

![](img/8afdd6ffb09408d2b44072d0ffe1a303.png)

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/2025-06-26 20-03-25.mp4", width=800, height=400)
```

Your browser does not support the video element.

## 三、借助LangGraph Cli打造智能数据分析助手

+   修改`graph.py`
