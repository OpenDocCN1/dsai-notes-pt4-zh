
<center><img src="https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/01dad5b6f552883b19da9b5863c9970.png" alt="01dad5b6f552883b19da9b5863c9970" style="zoom:50%;" /></center>

<center><img src="https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/7e1032040d5c3bd8447894629c9361e.png" alt="7e1032040d5c3bd8447894629c9361e" style="zoom:50%;" /></center>

<center><img src="https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/image-20250428112014361.png" style="zoom: 50%;" /></center>

```python
from google.adk.sessions import Session
from google.adk.events import UserMessage, AgentResponse

# 创建会话
session = session_service.create_session(
    app_name="MySQL_test",
    user_id="user_1",
    session_id="session_1",
    state={"history": []}
)
```

之后的对话就和此前无异。

---

## 更多ADK会话信息持久化方法详见正课

&emsp;&emsp;更多关于持久化具体实操流程，以及更多ADK进阶内容讲解，详见[《2025大模型Agent智能体开发实战》(5月班)](https://ix9mq.xetslk.com/s/3u765N)付费课程。<span style="color:red;">详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇</span>

---

# 五、ADK内置前端调用流程

## 1. ADK内置前端功能介绍

ADK作为最新一代Agent开发框架，不仅功能特性非常领先，而且还内置了非常多的工具，包括Gemini系列模型自带的谷歌搜索、文件检索和代码解释器等功能，同时ADK还拥有内置对话前端，方便开发者更加直观的感受Agent的对话流程，并且实时追踪Agent的events流。而如果Agent中存在外部工具调用，则在内置前端中，还能进一步观察到Agent调用外部工具完整流程，以及各环节调用信息，方便开发者即时debug。如果是使用Gemini模型，这个前端还支持语音和视频实时交互，基本展示效果如下：

## 2. ADK内置前端开启流程

而ADK内置前端开启流程也非常简单，这里我们需要创建一个项目文件，如创建一个`ADK_Chat`文件夹：
然后创建基本文件结构：（注意，如果是Windows系统，官方推荐手动创建项目文件基本结构，而不是采用IDE如Cursor等进行创建，以避免出现字符乱码情况）

```
ADK_Chat/

├── test_agent/

│   ├── agent.py

│   ├── __init__.py

│   ├── .env

├── __init__.py

└── ...
```

![](img/49f435bc762945c060c7a01ee87f801d.png)

![](img/014a131568217989174775e72541f049.png)

然后使用cursor或者其他IDE打开项目，准备写入代码。（也可以直接使用文本编辑器将代码拷贝进去）

+   `init.py`

写入如下内容：`from . import agent`

+   `.env`

然后在`.env`中写入如下内容：

```
DS_BASE_URL=https://api.deepseek.com

DS_API_KEY=YOUR_API_KRY
```

![](img/03a14a138787305f9cfe54de270679c3.png)

+   `agent.py`

写入如下内容：

```python
from google.adk.agents import Agent

from google.adk.models.lite_llm import LiteLlm

import os

from dotenv import load_dotenv

load_dotenv(override=True)

DS_API_KEY = os.getenv("DS_API_KEY")

DS_BASE_URL = os.getenv("DS_BASE_URL")

model = LiteLlm(

    model="deepseek/deepseek-chat",  

    api_base=DS_BASE_URL,

    api_key=DS_API_KEY

)

root_agent = Agent(

    name="simple_agent",

    model=model,

    instruction="你是一个乐于助人的中文助手。",

    description="回答用户的问题。"

)
```

然后再次确认当前环境是否安装`adk`和`litellm`两个库：

```bash
pip install litellm==1.67.2

pip install google-adk 
```

然后使用`adk`命令先进行Agent测试。我们在根目录下输入：

```bash
adk run test_agent
```

若能顺利进行对话，则说明Agent使用正常：
由此不难看出，`adk`其实是伴随着安装包一起安装的调用测试命令。在使用`adk`命令时，我们无需单独设置主函数，只需要按照要求创建一个名为`root_agent`的主agent，即可顺利开启对话。同时当前项目结构中，`.env`文件用于保存一些关键变量信息，而`__init__.py`则负责将当前项目文件`test_agent`包装为一个可执行的Python文件。这也就是为何我们可以直接输入`adk run test_agent`的原因。

接下来即可开启前端了，我们在项目根目录下输入：

```bash
adk web --port 8002
```

![](img/b1bac1d038065f43b0182ea5c3cb5b82.png)

![](img/15c36842611415a67408d817f990df55.png)

> 注意选择一个未被使用的端口号
然后在浏览器输入：`http://0.0.0.0:8002/`即可开启对话：
在对话页面能够看到完整的历史对话消息，并且在左侧还能看到事件流的基本情况，以及每个事件的详细信息：
并且此时Agent也是能支持多轮对话的。具体演示效果如下：

```python
from IPython.display import Video
```

```python
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/2025-04-28%2012-12-22.mp4", width=800, height=400)
```

Your browser does not support the video element.

不难看出，尽管单独使用ADK代码环境创建Agent流程较为复杂，但如果是借助`ADK run`或者`web`等工具，则可以不进行任何额外其他设置，快速调用已经创建好的Agent并进行功能测试，期间关于会话ID、用户ID和APP ID等参数都会自动设置，且支持多轮对话并能打印详细的每个`event`信息，可以说是非常高效的开发工具了。

至此第一部分内容结束，下一部分我们将继续讲解ADK如何接入外部工具及MCP工具流程。

---

## 更多ADK会话信息持久化方法详见正课

更多关于持久化具体实操流程，以及更多ADK进阶内容讲解，详见《2025大模型Agent智能体开发实战》(5月班)付费课程。详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/f0a39e4ce16deee6e48c187cc2d55274.png)

![](img/6652067a3675cc8be7778bc74cbf26fe.png)

---
