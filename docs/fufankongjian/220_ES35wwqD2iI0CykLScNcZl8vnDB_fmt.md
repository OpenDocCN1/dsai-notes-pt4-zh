
# LangGraph MCP智能体开发实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/PTIRd1S9DoAP4fxQa96cnJtQnwg](https://kq4b3vgg5b.feishu.cn/docx/PTIRd1S9DoAP4fxQa96cnJtQnwg)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(夏季班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(夏季班)！

![](img/216ce48e76d3f6976f7b67014e7ec3f2.png)

![](img/9008cfaeb33ecb2a0f9e37c2588b2cc4.png)

公开课全套学习资料，已上传至网盘（链接:https://pan.baidu.com/s/1Grrp1Mjc-99s_PKMbNlcYg?pwd=je4y）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## 一、LangGraph与MCP技术概述

### 1. LangGraph接入外部工具技术实现方法

说到智能体开发，无论使用何种框架，有一项绕不开的核心技术，那就是`MCP`（Model Context Protocol）技术。

在智能体开发过程中，接入外部函数工具是至关重要的一环，而在`LangChain`&`LangGraph`技术生态中，我们可以非常便捷的通过`@tool`装饰符来自定义一个外部函数：

![](img/4b69a848667a2d075b240306a9d5b1c7.png)

或者也可以借助`LangChain`丰富的、数以百计的内置工具，三行代码即可在智能体中进行工具调用。

| 功能类别 | 工具名称 | 简要说明 |
| --- | --- | --- |
| 🔎 搜索工具 | `TavilySearchResults` | 快速搜索实时网络信息 |
|  | `SerpAPIWrapper` | 基于 `SerpAPI` 的搜索结果工具 |
|  | `GoogleSearchAPIWrapper` | 调用 Google 可编程搜索引擎 |
| 🧠 计算工具 | `PythonREPLTool` | 执行 Python 表达式并返回结果 |
|  | `LLMMathTool` | 结合 LLM 和数学推理能力 |
|  | `WolframAlphaQueryRun` | 基于 `Wolfram Alpha` 的计算引擎 |
| 🗂 数据工具 | `SQLDatabaseToolkit` | 构建 SQL 数据库查询工具集 |
|  | `PandasDataframeTool` | 用于在 `Agent` 中操作表格数据 |
| 🌐 网络/API | `RequestsGetTool` / `RequestsPostTool` | 执行 HTTP 请求 |
|  | `BrowserTool` / `PlaywrightBrowserToolkit` | 自动化网页浏览与抓取 |
| 💾 文件处理 | `ReadFileTool` | 读取本地文件内容 |
|  | `WriteFileTool` | 写入文本到指定文件中 |
| 📚 检索工具 | `FAISSRetriever` | 基于向量的文档检索工具 |
|  | `ChromaRetriever` | 使用 `ChromaDB` 的检索器 |
|  | `ContextualCompressionRetriever` | 上下文压缩检索器，适合长文档 |
| 🧠 LLM 工具 | `ChatOpenAI` / `OpenAIFunctionsTool` | 使用 OpenAI 模型作为工具调用 |
|  | `ChatAnthropic` | Anthropic Claude 模型封装工具 |
| 🔧 自定义工具 | `@tool` 装饰器 | 任意函数可封装为 `Agent` 可调用工具 |
|  | `Tool` 类继承 | 自定义更复杂逻辑的工具实现 |

> `LangChain`工具集：https://python.langchain.com/docs/integrations/tools/

![](img/08f71d1db71f2490e005378e2dcde971.png)

而除此之外，伴随着`MCP`技术爆火，给了开发者第三种选项，那就是借助`MCP`技术、遵照`MCP`协议，来调用其他开发者已经开发好的`MCP`工具，来快速搭建智能体应用。

### 2. MCP技术概述

#### 2.1 MCP技术定位与技术价值介绍

我们可以将`MCP`技术简单理解为智能体外部工具开发的一种通用规范（范式）。举个例子，以天气查询工具为例，在`MCP`技术诞生之前，要给大模型添加查询天气的功能，至少需要经历这么两个开发阶段：

+   阶段一：编写查询天气的外部函数，例如：

```
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
```

+   阶段二：将外部工具进行进一步封装，以适配不同的开发框架。例如

+   接入`OpenAI Agents SDK`时：

```
from agents import function_tool
@function_tool
def get_weather(loc):
    """
    查询即时天气函数......
    """
    # Step 1.构建请求
    url = "https://api.openweathermap.org/data/2.5/weather"

    # Step 2.设置查询参数
    params = {......
    return ......
```

+   接入`LangGraph`时：

```
from langchain_core.tools import tool
from pydantic import BaseModel, Field

class WeatherQuery(BaseModel):
    loc: str = Field(description="The location name of the city")

@tool(args_schema = WeatherQuery)
def get_weather(loc):
    """
    查询即时天气函数......
    """
    # Step 1.构建请求
    url = "https://api.openweathermap.org/data/2.5/weather"

    # Step 2.设置查询参数
    params = {......
    return ......
```

+   接入谷歌`ADK`时

```
def get_weather(city: str) -> str:
    """
    Retrieves the current weather report for a specified city.

    Args:
        city (str): The name of the city (e.g., "Beijing", "Shanghai").
                   Note: For cities in China, use the city's English name (e.g., "Beijing").

    Returns:
        dict: A dictionary containing the weather information.
              Includes a 'status' key ('success' or 'error').
              If 'success', includes a 'report' key with weather details.
              If 'error', includes an 'error_message' key.
    """
    # Step 1.构建请求
    url = "https://api.openweathermap.org/data/2.5/weather"

    # Step 2.设置查询参数
    params = {......
    return ......
```

这就使得实际开发`Agent`的过程中，外部函数工具的开发会占用大量的开发者的时间精力。

而与此同时，人们发现，很多外部工具的功能其实是通用的，例如查询时间、查询天气、网络搜索、操作本地文件夹等等等等，如果有一种规范，能够减少重复造轮子的时间，一个人开发完成后全体开发者都能共享，那么整体的研发效率都将得到大幅提高。

在这一设想下，`MCP`技术诞生了。`MCP`的全称是`Model Context Protocol`，模型上下文协议，由`Claude`母公司`Anthropic`于去年11月正式提出。

![](img/aa904592a9e385a027948f419e89b5c2.png)

该技术核心目标，就是创建一种统一的大模型调用外部工具的通信规范，相当于这种标准的通信规范，一项特定功能的外部函数，只需要开发一次，就能被各种不同类型的`Agent`开发框架所识别。例如同样是查询天气，如果我们遵循`MCP`技术协议开发一个查询天气的外部函数，那么接下来全体开发者就都能直接用我开发好的这个天气查询工具，带入任何智能体开发框架，快速搭建智能体应用了。

通过下面这组图能够非常清楚的解释`MCP`工具在智能体开发过程中实际带来的提效的作用。

![](img/6f9457ebe9ad6dd9d6bda67bae7247e5.png)

![](img/f6a2e2ac79aa51062f58c69b5cf7a172.png)

#### 2.2 MCP技术架构

不过呢，要做到这种“车同轨、书同文”的标准化工作，不仅需要制定一套让所有人都信服的标准，而且还需要经过时间的检验，同时还需要有足够多的用户，这个标准才能真正被市场所认同。因此`MCP`技术也历经了一段时间的沉淀和打磨，自去年11月发布开始，到今年3月技术大爆发，再到第二季度开始越来越多的`Agent`框架和热门应用宣布支持`MCP`技术，`MCP`才算是逐渐成为一项智能体开发的通用协议。

截止目前，`MCP`的技术生态可以划分为三层，最底层是协议层，也就是“文字版”的规定、或者说规范，而为了普及这一规范，让更多的人更加快速的完成自己的`MCP`工具开发，`Anthropic`进一步的提供了`MCP`开发工具，借助这些`SDK`，我们能够非常快速完成`MCP`工具开发。而既然是一种标准化的协议，其核心价值就在于用的人足够多、同时分享的人也足够多，才能真正减少“重复造轮子”的时间，因此`Anthropic`官方和很多第三方平台，也在积极的推进`MCP`技术生态的构建，尤其是`MCP`工具平台的建设，通过鼓励开发者更多的分享自己开发的`MCP`工具，只有形成了更大的（分享和引用的）协作规模，`MCP`的技术才能更有价值。

![](img/bffd1092cb2f5aebbea9080acf8cb2c5.png)

#### 2.3 MCP SDK与MCP技术生态

而在这些`MCP`完整的技术架构中，开发者尤其需要关注`MCP`的`SDK`（开发工具）和`MCP`技术生态。所谓`MCP`的`SDK`，指的是官方提供的用于开发`MCP`工具的第三方库，截至目前，`MCP SDK`已支持`Python`、`TypeScript`、`Java`、`Kotlin`和`C#`等编程语言进行客户端和服务器创建。

> `MCP`项目官网：https://github.com/modelcontextprotocol

![](img/422e1f679beae1dc00950219a6e91fba.png)

而借助这些库，仅需几行代码，即可快速构建一个`MCP`工具。

![](img/ec4daac52e162d55840534179ac5576a.png)

反之，如果没有这些`MCP`开发工具，想要开发`MCP`工具，就必须从`MCP`技术协议出发，借助其他库来完成开发，其实现难度非常大。

而如果我们并不需要开发`MCP`工具，而只想要借助现成的`MCP`工具快速完成智能体开发，那么就需要重点关注现在的`MCP`集成平台，也就是集成了各类目前非常流行的`MCP`工具的平台，借助这些平台，我们能够快速找到想要的`MCP`服务，然后根据指示说明快速进行接入，甚至伴随着`MCP`流式`HTTP`功能的上线，很多平台还提供了这些`MCP`工具后端运行服务，我们只需要输入指定的后端地址，就能调用运行在云端的`MCP`工具。

主流的`MCP`集成平台如下：

+   `MCP`官方服务器合集：https://github.com/modelcontextprotocol/servers

+   `MCP` `Github`热门导航：https://github.com/punkpeye/awesome-mcp-servers

+   `Smithery`：https://smithery.ai/

![](img/9e38b1d09b7041e173b4cbc29b37ea1c.png)

+   `MCP`导航：https://mcp.so/

+   阿里云百炼：https://bailian.console.aliyun.com/?tab=mcp

![](img/49466c7ce2ed365f14b81e51ba395c48.png)

+   魔搭社区`MCP`广场：https://www.modelscope.cn/mcp

![](img/5251758a87cb1071d9507e0b944e5208.png)

+   `mcp.run`：https://www.mcp.run/

![](img/78a34bb15e6d94a35f1ee06e7afc5c44.png)

### 3. MCP核心技术概念

而在正式开始`MCP`智能体开发之前，我们还需要补充两个`MCP`技术体系中至关重要的技术概念，分别是`MCP`客户端与服务器、以及两大类`MCP`工具运行模式。

#### 3.1 MCP客户端与服务器

由于`MCP`是一种围绕大模型外部函数工具创建的统一范式，因此`MCP`工具从诞生之初就是客户端（`client`）与服务器（`server`）分离的架构。服务器与客户端的技术概念可以借助`MySQL`这个通用的数据库软件进行理解，在`MySQL`中，服务器指的是数据库实际运行环境，例如公司内部的某个统一用于数据存储的物理机，而客户端，则指的是`SQL`编写和运行的环境，可以是比如数据分析师用的笔记本。每次要进行查数时，数据分析师就可以在自己的笔记本上运行`MySQL WorkBench`（一个`SQL`编程的IDE），然后借助`MySQL`客户端，给公司的`MySQL`服务器发送查数的请求。

类似的，所谓`MCP Server`（服务器），指的是`MCP`工具运行的环境，而`MCP Client`（客户端），则指的是能够调用`MCP`工具、或者说给`MCP`工具发送请求并接受结果的环境。二者关系如图所示：

![](img/5ac94e8bd6b119ee6fdf5abd7da2c49d.png)

![](img/30468317e83c1b3a21b43f9e71ef2297.png)

这种服务器和客户端分离的架构的好处，就在于可以更加便捷的进行模块化开发和维护，而此前我们所说的`MCP`工具，其实就指的是`MCP`服务器，而那些`MCP`工具集合，其实就是`MCP`服务器集合网站。

同时，基于这种技术划分，当我们在开发一个智能体，并希望这个智能体能够接入`MCP`工具时，其实从`MCP`技术角度来说，我们本质上是开发一个`MCP`的客户端（`Client`）。例如当我们基于`LangGrpah`开发一个接入`MCP`工具的智能体，其实我们就开发了一个基于`LangGraph`的`MCP`客户端。而现在也有很多大模型聊天工具允许接入`MCP`工具，例如`Claude Desktop`、`Cherry Studio`等，这些也都是`MCP`客户端。

#### 3.2 标准MCP工具接入客户端流程



这里以Cherry Studio为例，为大家展示一个标准的MCP客户端接入MCP服务器的基本流程，从中我们能够看出，填写对应的配置文件，是运行MCP工具的关键。

### 3.3 MCP离线运行与在线运行模式

既然是服务器和客户端分离的架构，那么MCP肯定支持两种调用方法，其一是本地运行，也被成为离线运行，指的是MCP服务器和MCP客户端在同一台电脑上进行运行，其二则是在线运行，或者异地部署运行，指的是MCP服务器在远程服务器或者云端运行，然后借助HTTP网络通信来进行响应。

而这也就对应着MCP工具调用的两种核心模式，分别是`stdio`（本地）模式调用和`SSE`&`streamable HTTP`（在线）模式调用，各调用方法效果对比如下所示：

| 特性 | `Stdio` | `SSE` | `Streamable HTTP` |
| --- | --- | --- | --- |
| 通信方向 | 双向（但仅限本地） | 单向（服务器到客户端） | 双向（适用于复杂交互） |
| 使用场景 | 本地进程间通信 | 实时数据推送，浏览器支持 | 跨服务、分布式系统、大规模并发支持 |
| 支持并发连接数 | 低 | 中等 | 高（适合大规模并发） |
| 适应性 | 局限于本地环境 | 支持浏览器，但单向通信 | 高灵活性，支持流式数据与请求批处理 |
| 实现难度 | 简单，适合本地调试 | 简单，但浏览器兼容性和长连接限制 | 复杂，需处理长连接和流管理 |
| 适合的业务类型 | 本地命令行工具，调试环境 | 实时推送，新闻、股票等实时更新 | 高并发、分布式系统，实时交互系统 |

三种传输方式总结如下：

+ `Stdio` 传输：适合本地进程之间的简单通信，适合命令行工具或调试阶段，但不支持分布式。
+ `SSE` 传输：适合实时推送和客户端/浏览器的单向通知，但无法满足双向复杂交互需求。
+ `Streamable HTTP` 传输：最灵活、最强大的选项，适用于大规模并发、高度交互的分布式应用系统，虽然实现较复杂，但能够处理更复杂的场景。

而伴随着5月MCP更新了Streamable HTTP的SDK，目前越来越多的MCP工具都选择采用Streamable HTTP形式进行部署和运行。

### 3.4 离线MCP工具托管平台

当然，如果是调用在线MCP服务，开发者只能了解其功能，而如果是离线的MCP服务，则对应的MCP工具是完全开源的，在每次调用之前，开发者都需要将其源码先下载到本地，然后再运行。例如上述演示视频中，我们使用`npx`命令，其本质就是先将指定的MCP工具下载到本地，然后在有需要的时候对其进行调用。例如高德MCP配置文件如下：

```json
"amap-maps": {
  "isActive": true,
  "command": "npx",
  "args": [
    "-y",
    "@amap/amap-maps-mcp-server"
  ],
  "env": {
    "AMAP_MAPS_API_KEY": "YOUR_API_KRY"
  },
  "name": "amap-maps"
}
```

代表的含义就是我们需要先使用如下命令：

```
npx -y @amap/amap-maps-mcp-server
```

对这个库`@amap/amap-maps-mcp-server`进行下载，然后在本地运行，当有必要的时候调用这个库里面的函数执行相关功能。

而这个`@amap/amap-maps-mcp-server`库是一个托管在https://www.npmjs.com/上的库，可以使用`npx`命令进行下载。搜索库名即可看到这个库的完整代码，https://www.npmjs.com/package/@amap/amap-maps-mcp-server：

![](img/d3b04e7b7cb4711e0314c07c963484b2.png)

除此以外，一些由Python编写的MCP开源工具，则托管在pypi平台上，https://pypi.org/ ，每次运行的时候我们需要填写`uvx`命令，其本质就是将工具源码从pypi平台上下载到本地再来进行运行。

![](img/8228ab5272747ed5c1d3fd661a3d6222.png)

### 3.5 MCP在线托管服务

伴随着MCP快速调用的请求不断增加，也有很多平台提供了在线托管服务模式，例如魔搭社区的MCP广场中，就有很多是运行在魔搭社区服务器上的MCP工具，开发者可以直接使用`SSE`或者流式HTTP方式请求调用在线的MCP工具，从而快速完成开发工作。

![](img/8dd29166a8c7a9800d99a9c8663e7b56.png)

## 4. 更多MCP技术教程

![](img/d02282de760c5c346beff5370a372fd8.png)

✔《7分钟讲清楚MCP是什么？》：https://www.bilibili.com/video/BV1uXQzYaEpJ/?

✔《MCP技术开发入门实战！》：https://www.bilibili.com/video/BV1NLXCYTEbj/

✔《MCP企业级智能体开发实战！》：https://www.bilibili.com/video/BV1n1ZuYjEzf/

✔《主流客户端接入MCP实战》：https://www.bilibili.com/video/BV1dCo7YdEgK/

✔《从零到一开发&部署专属MCP工具》：https://www.bilibili.com/video/BV1VHL6zsE5F/

✔《流式HTTP MCP服务器开发流程》：https://www.bilibili.com/video/BV1P7VSzwEXL/

相关课程资料，下图扫码即可获取

![](img/3ea60afe6ed7966454e4a580956bcf5c.png)

## 5. LangGraph技术概述

### 5.1 从LangChain到LangGraph

说到现阶段目前最流行、最通用的Agent开发框架，毫无疑问，肯定是LangChain。LangChain作为2022年就已经开源的元老级开发框架，历经数年的发展，其功能和生态都已非常完善，并且拥有数量众多的开发者。在我们团队统计的今年第一季度大模型岗位JD中，有90%以上的Agent开发岗位要求掌握LangChain，可以说LangChain就是目前最通用、最流行的Agent开发框架没有之一。不过，由于LangChain诞生时间较早，在2022年末，GPT-3.5刚诞生之时，开发者对大模型的想象主要是希望用其搭建一个又一个工作流，而这也成为LangChain的核心技术目标，LangChain中“Chain”就是指链、也就是搭建工作流的意思。

![](img/ce2cebc3032eb998b594d3199184fdab.png)

![](img/7fd64ad970fb89c7e624daf366547eac.png)

但是，大模型基座模型能力在飞速进化，例如目前最新一代的大模型，不仅拥有非常强悍的外部工具识别和调用能力，还原生就支持多工具并联和串联调用，而开发者对于大模型应用开发的需求也在快速变化，单纯的构建这种线性的工作流，可拓展性并不强。当前开发者更偏好的是“用AI开发AI”的思路，也就是我们不用定死工作流，而可以让大模型自主判断每一步该怎么做，然后实时创建一些链路，并根据运行情况随时调整。这也就是目前Agent智能体开发的核心思路。

LangGraph和LangChain同宗同源，底层架构完全相同、接口完全相通。从开发者角度来说，LangGraph也是使用LangChain底层API来接入各类大模型、LangGraph也完全兼容LangChain内置的一系列工具。换而言之，LangGraph的核心功能都是依托LangChain来完成。但是和LangChain的链式工作流哲学完全不同的是，LangGraph的基础哲学是构建图结构的工作流，并引入“状态”这一核心概念来描绘任务执行情况，从而拓展了LangChain LCEL链式语法的功能灵活程度。

![](img/a82346140779f7a06f6a070da36ea3a1.png)

不过需要注意的是，LangGraph是基于LangChain进行的构建，无论图结构多复杂，单独每个任务执行链路仍然是现行的，其背后仍然是靠着LangChain的Chain来实现的。因此我们可以这么来描述LangChain和LangGraph之间的关系，LangGraph是LangChain工作流的高级编排工具，其中“高级”之处就是LangGraph能按照图结构来编排工作流。

### 5.2 LangGraph三层API架构

尽管图结构看起来比链式结构灵活很多，但在实际开发过程中，要创建一个图结构却绝非易事。大家可以试想一下，要在代码环境中描述清楚一个图，至少需要清晰的说明图中有哪些节点，不同节点之间是什么关系（边如何连接），同时还需要设置各节点功能，然后才能测试上线，这个流程其实是非常复杂的，哪怕是简单的一个图也需要用很多代码才能描述清楚：

![](img/88d5ca1d6722b69c46a6f8394ebf0c72.png)

不过这对于LangChain来说并不难解决。LangChain生态各框架的核心思路都是“既要有底层API来定义基础功能，同时也要有高层API来加快开发效率”，就好比LangChain中既有LCEL语法、同时也有Agent API一样，LangGraph也提供了基于图结构基础语法的高层API。LangGraph的高层API主要分为两层，其一是Agent API，用于将大模型、提示词模板、外部工具等关键元素快速封装为图中的一些节点，而更高一层的封装，则是进一步创建一些预构建的Agent、也就是预构建好的图，开发者只需要带入设置好要带入的工具和模型，三行代码就能借助这些预构建好的图，创建一个完整的Agent：

![](img/eef98abf0536a68c2f53e3cb61d5901d.png)

依托LangChain完善的生态、拥有丰富稳定的API架构、以及便捷上手等特性，使得LangGraph成为目前超越LangChain的新一代Agent开发框架。

![](img/b527773636cad4c77c9456dff9cec661.png)

### 5.3 LangGraph开发生态

+ 丰富的模型接口

为了让各类模型接入更加统一和稳定，LangChain不惜为不同的模型单独开发一个库，例如要接入Claude模型，我们就需要提前安装`langchain-anthropic`，而如果要接入DeepSeek模型，则需要安装`langchain-deepseek`。这么做虽然过程麻烦，但却能很好的保障开发过程的稳定性。

![](img/96b43b7537b1efa7362373f6d1c3f17a.png)

+ 各类模型接入列表：https://python.langchain.com/docs/integrations/chat/

此外需要注意的是，LangGraph也需要使用和LangChain一样的模型接入流程。

+ LangGraph运行监控框架：LangSmith

LangSmith 是一款用于构建、调试、可视化和评估 LLM 工作流的全生命周期开发平台。它聚焦的不是模型训练，而是我们在构建 AI 应用（尤其是多工具 Agent、LangChain/Graph）时的「可视化调试」、「性能评估」与「运维监控」。核心功能如下：

| 功能类别 | 描述 | 场景 |
| --- | --- | --- |
| 🧪 调试追踪（Trace Debugging） | 可视化展示每个 LLM 调用、工具调用、Prompt、输入输出 | Agent 调试、Graph 调用链分析 |
| 📊 评估（Evaluation） | 支持自动评估多个输入样本的回答质量，可自定义评分维度 | 批量测试 LLM 表现、A/B 对比 |
| 🧵 会话记录（Sessions / Runs） | 每次 chain 或 agent 的运行都会被记录为一个 Run，可溯源 | Agent 问题诊断、用户问题分析 |
| 🔧 Prompt 管理器（Prompt Registry） | 保存、版本控制、调用历史 prompt | 多版本 prompt 迭代测试 |
| 📈 流量监控（Telemetry） | 实时查看运行次数、错误率、响应时间等 | 在生产环境中监控 Agent 质量 |
| 📁 Dataset 管理 | 管理自定义测试集样本，支持自动化评估 | 微调前评估、数据对比实验 |
| 📜 LangGraph 可视化 | 对 LangGraph 中每个节点运行情况进行实时可视化展示 | Graph 执行追踪 |

![](img/ee7e55a34944c89c92d596205e8ce1e1.png)

+ LangSmith官网地址：https://docs.smith.langchain.com/

+ LangGraph图结构可视化与调试框架：LangGraph Studio

LangGraph Studio 是一个用于可视化构建、测试、分享和部署智能体流程图的图形化 IDE + 运行平台。核心功能如下：

| 功能模块 | 说明 | 应用场景 |
| --- | --- | --- |
| 🧩 Graph 编辑器 | 以拖拽方式创建节点（工具、模型、Router）并连接 | 零代码构建 LangGraph |
| 🔍 节点配置器 | 每个节点可配置 LLM、工具、Router 逻辑、Memory | 灵活定制 Agent 控制流 |
| ▶️ 即时测试 | 输入 prompt 可在浏览器中运行整个图 | 实时测试执行结果 |
| 💾 云端保存 / 分享 | 将构建的 Graph 保存为公共 URL / 私人项目 | 团队协作，Demo 分享 |
| 📎 Tool 插件管理 | 可连接自定义工具（MCP）、HTTP API、Python 工具 | 插件式扩展 Agent 功能 |
| 🔁 Router 分支节点 | 创建条件分支，支持 if/else 路由 | 决策型智能体 |
| 📦 上传文档 / 多模态 | 可以上传文件（如 PDF）并嵌入进图中处理流程 | RAG 结构、OCR、图文问答等 |
| 🧠 Prompt 输入/预览 | 编辑 prompt 并观察其运行效果 | Prompt 工程调试 |
| 📤 一键部署 | 将 Graph 部署为可被 Agent Chat UI 使用的 Assistant | 快速集成到前端 |

![](img/3ebd2082a82303c422afda207f76b7fb.png)

+ LangGraph服务部署工具：LangGraph Cli

LangGraph CLI 是用于本地启动、调试、测试和托管 LangGraph 智能体图的开发者命令行工具。核心功能如下：

| 功能类别 | 命令示例 | 说明 |
| --- | --- | --- |
| ✅ 启动 Graph 服务 | `langgraph dev` | 启动 Graph 的开发服务器，供前端（如 Agent Chat UI）调用 |
| 🧪 测试 Graph 输入 | `langgraph run graph:graph --input '{"input": "你好"}'` | 本地 CLI 输入测试，输出结果 |
| 🧭 管理项目结构 | `langgraph init` | 初始化一个标准 Graph 项目目录结构 |
| 📦 部署 Graph（未来） | `langgraph deploy`（预留） | 发布 graph 至 LangGraph 云端（已对接 Studio） |
| 🧱 显示 Assistant 列表 | `langgraph list` | 显示当前 graph 中有哪些 assistant（即 entrypoint） |
| 🔄 重载运行时 | 自动热重载 | 修改 `graph.py` 时，dev 模式自动重启生效 |

![](img/cfbe264db515a54b9da2e249cf3e332c.png)

+ LangGraph Cli官网地址：https://www.langgraph.dev/ （需要代理环境）

+ LangGraph内置工具库与MCP调用组件

除了有上述非常多实用的开发工具外，LangGraph还全面兼容LangChain的内置工具集。LangChain自诞生之初就为开发者提供了非常多种类各异的、封装好的实用工具，历经几年发展时间，目前LangChain已经拥有了数以百计的内置实用工具，包括网络搜索工具、浏览器自动化工具、Python代码解释器、SQL代码解释器等。而作为LangChain同系框架，LangGraph也可以无缝调用LangChain各项开发工具，从而大幅提高开发效率。

![](img/9b78233f86ecc5cc36ac28c70ba0aa60.png)

+ 此外，LangChain是最早支持MCP的开发框架之一，借助`langchain-mcp-adapters`，LangChain和LangGraph便可快速接入各类MCP工具。

![](img/392940ae39103b1e8fab0742a01d358f.png)

+ 并且LangGraph也同样支持谷歌的A2A（跨Agents通信协议）。

+ LangChain工具集：https://python.langchain.com/docs/integrations/tools/

### 5.4 LangChain&LangGraph更多入门基础教程

下图扫码免费获取完整课程资料



![](img/3f6f9857f1a0f177add5fbd9c139bb6b.png)

## 二、LangGraph搭建MCP客户端流程

接下来进入到实操环节，正式为大家介绍如何将MCP工具接入LangGraph中并创建智能体。

### 1. 创建自定义MCP工具

作为大模型开发者，掌握MCP工具开发流程是基本功，这里我们先尝试自定义MCP工具，并将其接入LangGraph。对于一个完整的MCP项目来说，要有完整的项目代码结构、以及符合MCP服务器基本调用规范。具体项目创建流程如下：

#### Step 1. 借助uv创建Python项目

MCP开发要求借助`uv`进行虚拟环境创建和依赖管理。`uv` 是一个`Python`依赖管理工具，类似于`pip`和`conda`，但它更快、更高效，并且可以更好地管理`Python`虚拟环境和依赖项。它的核心目标是替代`pip`、`venv`和`pip-tools`，提供更好的性能和更低的管理开销。

`uv` 的特点：

1. `速度更快`：相比`pip`，`uv` 采用`Rust` 编写，性能更优。

1. `支持 PEP 582`：无需`virtualenv`，可以直接使用`__pypackages__` 进行管理。

1. `兼容 pip`：支持`requirements.txt` 和`pyproject.toml` 依赖管理。

1. `替代 venv`：提供`uv venv` 进行虚拟环境管理，比 `venv` 更轻量。

1. `跨平台`：支持 `Windows`、`macOS` 和 `Linux`。

以`Ubuntu`系统为例，首先使用`pip`安装`uv`：

```
pip install uv
```

> 此外，也可以直接使用`curl`安装`uv`

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

然后按照如下流程创建项目主目录：

```
# 创建项目目录
uv init mcp-get-weather
cd mcp-get-weather
```

![](img/e6f9b25234b91ba49c57ce78a8aca24b.png)

然后输入如下命令创建虚拟环境：

```
# 创建虚拟环境
uv venv

# 激活虚拟环境
source .venv/bin/activate
```

此时这个`.venv`文件就负责保存当前虚拟环境的各项依赖。

![](img/8fde40d0d3a7e0e0b20680b426778ba3.png)

各文件解释如下：

| 文件/文件夹 | 作用 |
| --- | --- |
| `.git/` | Git 版本控制目录 |
| `.venv/` | 虚拟环境 |
| `.gitignore` | Git 忽略规则 |
| `.python-version` | Python版本声明 |
| `main.py` | 主程序入口 |
| `pyproject.toml` | 项目配置文件 |
| `README.md` | 项目说明文档 |

#### Step 2. 添加项目依赖

接下来继续使用`uv`工具，为我们的项目添加基础依赖。根据此前的代码解释不难看出，当前项目主要需要用到`httpx`、`dotenv`、`langgraph`、`langchain_deepseek`和`langchain_mcp_adapters`等核心库，我们可以使用如下命令安装相关依赖，并同时安装`mcp` sdk：

```
# 安装 MCP SDK
uv add mcp httpx dotenv langgraph langchain-deepseek langchain-mcp-adapters
```

![](img/230dde1ae88f51edaa0d0e2c4abbf68e.png)

注意，对于`uv`管理库来说，相关依赖会安装到`.venv`文件中，并不会和系统库产生冲突。

#### Step 3. 编写MCP服务器

接下来继续创建MCP服务器，为了更好的模拟真实场景，这里我们创建多个MCP服务器。

+   查询天气服务器`weather_server.py`

+   用于进行天气信息查询的服务器，完整代码如下：

```
import os
import json
import httpx
from typing import Any
from dotenv import load_dotenv
from mcp.server.fastmcp import FastMCP

# 初始化 MCP 服务器
mcp = FastMCP(&quot;WeatherServer&quot;)

# OpenWeather API 配置
OPENWEATHER_API_BASE = &quot;https://api.openweathermap.org/data/2.5/weather&quot;
API_KEY = &quot;98cdb20187473b93432b86d8181cb070&quot;
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

+   写入本地文档服务器`write_server.py`

+   用于将指定文本信息写入本地文档的外部函数，完整代码如下：

```
import os
from datetime import datetime
from mcp.server.fastmcp import FastMCP

# 初始化 MCP 服务器
mcp = FastMCP(&quot;WriteServer&quot;)
USER_AGENT = &quot;write-app/1.0&quot;

# 文件保存目录
OUTPUT_DIR = &quot;./output&quot;

# 确保目录存在
os.makedirs(OUTPUT_DIR, exist_ok=True)

@mcp.tool()
async def write_file(content: str) -&gt; str:
    &quot;&quot;&quot;
    将指定内容写入本地文件，并返回生成的文件名。
    :param content: 必要参数，字符串类型，用于表示需要写入文档的具体内容。
    :return: 写入结果与文件路径
    &quot;&quot;&quot;
    # 创建带时间戳的文件名
    timestamp = datetime.now().strftime(&quot;%Y%m%d_%H%M%S&quot;)
    filename = f&quot;note_{timestamp}.txt&quot;
    filepath = os.path.join(OUTPUT_DIR, filename)

    try:
        # 写入文件
        with open(filepath, &quot;w&quot;, encoding=&quot;utf-8&quot;) as f:
            f.write(content)

        return f&quot;✅ 已成功写入文件: {filepath}&quot;
    except Exception as e:
        return f&quot;⚠️ 写入失败: {e}&quot;

if __name__ == &quot;__main__&quot;:
    # 以标准 I/O 方式运行 MCP 服务器
    mcp.run(transport=&#x27;stdio&#x27;)
```

+   创建`.env`文件

+    为了更好的保管敏感`API-KEY`信息，我们还需要创建一个`.env`文件，并写入OpenWeather的`API-KEY`，用于进行天气查询。

![](img/a8b8a69c1c00dcdc1d4fcc50049dcec0.png)

+

![](img/f979e86b3a601964dcdb9e87405c2460.png)

创建完成后可以在本地文件夹中看到这两个`python`文件。

![](img/a75c6006793e7907cdbf9d1e77827c01.png)

#### Step 4. 测试MCP服务器功能

当我们完成MCP服务器开发后，即可使用`MCP-Inspector`进行MCP工具功能测试。回到项目主目录下，然后输入如下命令：

```
# 回到项目主目录
# cd /root/autodl-tmp/MCP/LangGraph/mcp-get-weather

# 安装Node.js
# curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash -
# sudo apt install -y nodejs

npx -y @modelcontextprotocol/inspector
```

![](img/ef6af8c30a49f09baa87a29349953ef3.png)

接下来，如果是使用AutoDL，则先需要使用隧道工具将端口映射到本地进行运行：

![](img/d022285a8c8f190a38925de1b98ef3be.png)

然后打开`Inspector`：`http://127.0.0.1:6274/` ，并在页面中进行如下设置：

同时，需要输入`Proxy Token`，可以在启动环境中查看：

![](img/66278859bc1fbe85cd1c445f2cb0830a.png)

![](img/beed5813bc6b3bba089d9408f227d0e3.png)

然后即可点击连接，显示`Connected`后则代表MCP工具服务已顺利启动。接下来即可进行功能测试了：

然后即可测试下一项工具`write_server.py`：

![](img/182123e1bf788f422d39698adb263a1f.png)

两项工具都通过了测试，

选择`SSE`模式，选择默认运行地址：`http://localhost:8000/sse`，然后点击`connect`：

然后输入地名进行测试：

![](img/0eeace7e55675531749eb8e8dab574f4.png)

完整测试流程如下：

同时我们也在本地文件夹中看到了`output`中新增的内容，表明第二个函数已经顺利将文本内容写入到了本地：

![](img/07f22cdeed4622c9525f87cd3baa9647.png)

至此，两项MCP工具均测试完毕，接下来即可构建LangGraph MCP客户端，来接入这些工具搭建智能体了。

### 2. 创建LangGraph MCP客户端

使用`LangGraph`接入`MCP`工具，核心需要使用`langchain_mcp_adapters`库，该库可以将`MCP`工具信息进行解析，并让`LangGraph`顺利识别。识别后即可像任意其他工具一样接入`LangGraph`中并搭建智能体。

![](img/cdefa67b2716847165655fd845c4bf2d.png)

`LangGraph`接入`MCP`的核心原理为：`weather_server.py` → 启动为子进程 → `stdio` 通信 → `MCP` 协议 → 转换为 `LangChain` 工具 → `LangGraph Agent` 执行读写，核心转换过程为：：

1.  `@mcp.tool()` → 标准 `LangChain Tool`

1.  `stdio_client()` → 自动处理 `read`/`write` 流，其中`read` 表示从 `MCP` 服务器读取响应的流，`write` 表示向 `MCP` 服务器发送请求的流，对于 `stdio weather_server.py`，它们就是子进程的 `stdout` 和 `stdin`

1.  `MultiServerMCPClient` → 一键转换所有工具

#### Step 1. 创建MCP配置文件

而为了完整实现一个标准的`MCP`调用流程、即通过配置文件灵活说明`MCP`工具信息然后再进行调用，这里我们同样先创建一个`servers_config.json`文件，用于记录`MCP`工具信息：



```
{
  "mcpServers": {
    "weather": {
      "command": "python",
      "args": ["weather_server.py"],
      "transport": "stdio"
    },
    "write": {
      "command": "python",
      "args": ["write_server.py"],
      "transport": "stdio"
    }
  }
}
```

![](img/ca7d6f5c8ce2e8abee483b9ee83b6544.png)

## Step 2. 修改`.env`配置文件和提示词模板

同样的，我们需要创建一个`.env`文件来写入敏感信息：

![](img/bdf1c3871e6bd54e5af83a8685c0c409.png)

```
BASE_URL=https://api.deepseek.com
MODEL=deepseek-chat
LLM_API_KEY=DeepSeek API KEY
OPENWEATHER_API_KEY=OpenWeather API KEY
```

然后为当前Agent创建一个提示词模板`agent_prompts.txt`：

![](img/2faaa86ad6ee6ebd0aa1bfb07ddcbe8b.png)

然后写入如下内容：

```
你是一个智能体，具备以下两种能力：

1.查询天气：调用query_weather(city: str)，返回指定城市的实时天气。

2.写入文件：调用write_file(content: str)，将文本内容写入本地文件并返回路径。

当用户提出请求时，你需要理解意图并选择相应的工具。如果请求缺少必要信息，先与用户确认后再调用工具。返回结果时以简洁、友好的方式回复。

如果用户提出的问题与你的功能无关，请礼貌告知无法处理。
```

![](img/804aaba4d159edaf71a3d552d3e88dc8.png)

## Step 3. 创建`client.py`主函数文件

然后再创建`client.py`主函数文件

![](img/414b3a0d57a14485ee8fd334b779ca46.png)

并在其中写入如下代码：

```
"""
多服务器 MCP + LangChain Agent 示例
---------------------------------
1. 读取 .env 中的 LLM_API_KEY / BASE_URL / MODEL
2. 读取 servers_config.json 中的 MCP 服务器信息
3. 启动 MCP 服务器（支持多个）
4. 将所有工具注入 LangGraph Agent，由大模型自动选择并调用
"""

import asyncio
import json
import logging
import os
from typing import Any, Dict, List

from dotenv import load_dotenv
from langgraph.prebuilt import create_react_agent
from langchain_deepseek import ChatDeepSeek
from langchain_mcp_adapters.client import MultiServerMCPClient
from langgraph.checkpoint.memory import InMemorySaver

# 设置记忆存储
checkpointer = InMemorySaver()

# 读取提示词
with open("agent_prompts.txt", "r", encoding="utf-8") as f:
    prompt = f.read()

# 设置对话配置
config = {
    "configurable": {
        "thread_id": "1"  
    }
}

# ────────────────────────────
# 环境配置
# ────────────────────────────

class Configuration:
    """读取 .env 与 servers_config.json"""

    def __init__(self) -> None:
        load_dotenv()
        self.api_key: str = os.getenv("LLM_API_KEY") or ""
        self.base_url: str | None = os.getenv("BASE_URL")  # DeepSeek 用 https://api.deepseek.com
        self.model: str = os.getenv("MODEL") or "deepseek-chat"
        if not self.api_key:
            raise ValueError("❌ 未找到 LLM_API_KEY，请在 .env 中配置")

    @staticmethod
    def load_servers(file_path: str = "servers_config.json") -> Dict[str, Any]:
        with open(file_path, "r", encoding="utf-8") as f:
            return json.load(f).get("mcpServers", {})

# ────────────────────────────
# 主逻辑
# ────────────────────────────
async def run_chat_loop() -> None:
    """启动 MCP-Agent 聊天循环"""
    cfg = Configuration()
    os.environ["DEEPSEEK_API_KEY"] = os.getenv("LLM_API_KEY", "")
    if cfg.base_url:
        os.environ["DEEPSEEK_API_BASE"] = cfg.base_url
    servers_cfg = Configuration.load_servers()

    # 1️ 连接多台 MCP 服务器
    mcp_client = MultiServerMCPClient(servers_cfg)

    tools = await mcp_client.get_tools()         # LangChain Tool 对象列表

    logging.info(f"✅ 已加载 {len(tools)} 个 MCP 工具： {[t.name for t in tools]}")

    # 2️ 初始化大模型（DeepSeek / OpenAI / 任意兼容 OpenAI 协议的模型）
    DeepSeek_API_KEY = os.getenv("DEEPSEEK_API_KEY")
    model = ChatDeepSeek(model="deepseek-chat")

    # 3 构造 LangGraph Agent
    
    agent = create_react_agent(model=model, 
                               tools=tools,
                               prompt=prompt,
                               checkpointer=checkpointer)

    # 4 CLI 聊天
    print("\n🤖 MCP Agent 已启动，输入 'quit' 退出")
    while True:
        user_input = input("\n你: ").strip()
        if user_input.lower() == "quit":
            break
        try:
            result = await agent.ainvoke(
            {"messages": [{"role": "user", "content": user_input}]},
            config
        )
            print(f"\nAI: {result['messages'][-1].content}")
        except Exception as exc:
            print(f"\n⚠️  出错: {exc}")

    # 5️ 清理
    await mcp_client.cleanup()
    print("🧹 资源已清理，Bye!")

# ────────────────────────────
# 入口
# ────────────────────────────
if __name__ "__main__":
    logging.basicConfig(level=logging.INFO, format="%(asctime)s - %(levelname)s - %(message)s")
    asyncio.run(run_chat_loop())
```

代码解释如下：

✅ 从 `.env` 文件读取模型配置

✅ 从 `servers_config.json` 读取 MCP 服务器配置（支持多个服务器）

✅ 启动 MCP 客户端加载所有工具

✅ 用 LangGraph 创建 Agent，把所有工具挂载

✅ 在命令行与用户进行对话，模型自动选择工具

✅ 清理资源

### 1️⃣ 顶部模块导入

```
import asyncio
import json
import logging
import os
from typing import Any, Dict, List

from dotenv import load_dotenv
from langgraph.prebuilt import create_react_agent
from langchain_deepseek import ChatDeepSeek
from langchain_mcp_adapters.client import MultiServerMCPClient
from langgraph.checkpoint.memory import InMemorySaver
```

🔹 解释

+ `asyncio`：异步执行工具

+ `json`：读取JSON配置

+ `logging`：日志输出

+ `os`：读取环境变量

+ `load_dotenv`：加载`.env`文件

+ `create_react_agent`：LangGraph的预置函数，创建ReAct式Agent

+ `ChatDeepSeek`：DeepSeek官方模型封装

+ `MultiServerMCPClient`：用于一次连接多个MCP服务器

+ `InMemorySaver`：LangGraph的内存记忆存储（会话状态保存）

### 2️⃣ 记忆存储和提示词加载

```
checkpointer = InMemorySaver()

with open("agent_prompts.txt", "r", encoding="utf-8") as f:
    prompt = f.read()
```

🔹 解释

+ `checkpointer`: 用于保存在对话中的上下文记忆（例如工具调用历史）

+ `prompt`: 从 `agent_prompts.txt` 读取智能体提示词（系统角色）

### 3️⃣ 对话配置

```
config = {
    "configurable": {
        "thread_id": "1"  
    }
}
```

🔹 解释

+ `thread_id`: 会话编号，如果要做多会话隔离时用

+ 这里硬编码为`1`，所以这个会话上下文ID固定

### 4️⃣ `Configuration`类 - 环境配置

```
class Configuration:
    """读取 .env 与 servers_config.json"""

    def __init__(self) -> None:
        load_dotenv()
        self.api_key: str = os.getenv("LLM_API_KEY") or ""
        self.base_url: str | None = os.getenv("BASE_URL")
        self.model: str = os.getenv("MODEL") or "deepseek-chat"
        if not self.api_key:
            raise ValueError("❌ 未找到 LLM_API_KEY，请在 .env 中配置")

    @staticmethod
    def load_servers(file_path: str = "servers_config.json") -> Dict[str, Any]:
        with open(file_path, "r", encoding="utf-8") as f:
            return json.load(f).get("mcpServers", {})
```

🔹 解释

这个类分两部分：

1. 初始化方法

+ `load_dotenv()` 加载 `.env` 中的配置

+ `LLM_API_KEY`: 大模型的密钥

+ `BASE_URL`: API Base URL

+ `MODEL`: 模型名称

+ 没有密钥会直接报错

2. 静态方法

+ `load_servers()`：加载 `servers_config.json` 文件，返回 `mcpServers` 字典

+ 这个字典包含所有 MCP 服务器连接信息

✅ 这样就可以把配置集中管理

### 5️⃣ `run_chat_loop()` 主逻辑

这是整个程序的核心。

```
async def run_chat_loop() -> None:
```

一个异步函数，用于：

+ 启动MCP客户端

+ 创建LangGraph Agent

+ 循环读取用户输入

+ 调用Agent

+ 返回结果

我们分步骤看。

#### 5.1 配置读取与环境变量设置

```
cfg = Configuration()
os.environ["DEEPSEEK_API_KEY"] = os.getenv("LLM_API_KEY", "")
if cfg.base_url:
    os.environ["DEEPSEEK_API_BASE"] = cfg.base_url
servers_cfg = Configuration.load_servers()
```

🔹 解释

+ `cfg = Configuration()`: 实例化配置

+ 把 `LLM_API_KEY` 注入环境变量 `DEEPSEEK_API_KEY`（DeepSeek SDK会用）

+ 如果设置了 `BASE_URL`，也注入 `DEEPSEEK_API_BASE`

+ 加载所有MCP服务器配置

#### 5.2 连接多台MCP服务器

```
mcp_client = MultiServerMCPClient(servers_cfg)
tools = await mcp_client.get_tools()
```

🔹 解释

+ `MultiServerMCPClient`: 自动连接多台MCP服务器

+ `get_tools()`：从所有服务器加载工具（LangChain Tool对象）

+ 例如你的`query_weather`和`write_file`



+ 可以跨服务器统一管理

### 5.3 日志打印

```
logging.info(f"✅ 已加载 {len(tools)} 个 MCP 工具： {[t.name for t in tools]}")
```

🔹 **解释**

显示一行日志，告诉你加载了多少工具以及它们的名字。

### 5.4 初始化大模型

```
DeepSeek_API_KEY = os.getenv("DEEPSEEK_API_KEY")
model = ChatDeepSeek(model="deepseek-chat")
```

🔹 **解释**

+ `ChatDeepSeek`: 封装 DeepSeek 的聊天模型
+ `model`: 用于 LangGraph Agent 的核心 LLM

### 5.5 创建 LangGraph Agent

```
agent = create_react_agent(model=model, 
                           tools=tools,
                           prompt=prompt,
                           checkpointer=checkpointer)
```

🔹 **解释**

+ `create_react_agent`: 快捷方法，创建 ReAct 推理式智能体
+ 参数：
  + `model`: 大模型
  + `tools`: 所有 MCP 工具
  + `prompt`: 系统提示词
  + `checkpointer`: 记忆存储器（保存上下文）

### 5.6 CLI 聊天循环

```
print("\n🤖 MCP Agent 已启动，输入 'quit' 退出")
while True:
    user_input = input("\n你: ").strip()
    if user_input.lower() == "quit":
        break
    try:
        result = await agent.ainvoke(
            {"messages": [{"role": "user", "content": user_input}]},
            config
        )
        print(f"\nAI: {result['messages'][-1].content}")
    except Exception as exc:
        print(f"\n⚠️  出错: {exc}")
```

🔹 **解释**

+ `input()`: 读取用户输入
+ 如果输入 `quit` 就退出
+ 调用 `agent.ainvoke()`：
  + 异步执行模型推理
  + 自动决定要不要调用工具
  + 打印最后一条模型回复
+ 出错时打印异常

### 5.7 清理资源

```
await mcp_client.cleanup()
print("🧹 资源已清理，Bye!")
```

🔹 **解释**

关闭所有 MCP 连接，释放资源。

### 入口逻辑

```
if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO, format="%(asctime)s - %(levelname)s - %(message)s")
    asyncio.run(run_chat_loop())
```

🔹 **解释**

+ 配置日志格式
+ 启动事件循环执行 `run_chat_loop()`

#### 功能总结

✅ **多服务器支持**：同时连接多台 MCP 服务器

✅ **动态工具加载**：不需硬编码工具

✅ **DeepSeek 模型支持**：直接用 LangChain 的 DeepSeek 封装

✅ **记忆管理**：用 `InMemorySaver` 保存对话状态

✅ **标准 ReAct 推理**：大模型自主判断何时调用工具

#### Step 4. 运行测试

最后，输入如下命令启动对话：

```
uv run client.py
```

实际运行效果如下，能够发现，无论是多工具调用还是持久化记忆，都能顺利运行。

![](img/6057db9198bc729bdcb1ab16d01b7049.png)

至此，我们就完整完成了完整的 MCP 工具接入 LangGraph 搭建智能体的完整流程。

### 3. 接入更多 MCP 工具

#### 3.1 Filesystem MCP 工具接入

接下来借助我们创建好的 MCP 客户端，仅需填写 MCP 配置，即可非常便捷的接入更多 MCP 工具。这里以 GitHub 上的 Filesystem MCP 工具为例进行演示。Filesystem 服务器是一个最基础同时也是最常用的 MCP 服务器，同时也是官方推荐的服务器，服务器项目地址：https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem

![](img/c47db98807fa6850479027ccb3247b3d.png)

借助 Filesystem，我们可以高效便捷操作本地文件夹。同时 Filesystem 也是一个 js 项目，源码托管在 npm 平台上：（https://www.npmjs.com/package/@modelcontextprotocol/server-filesystem）

![](img/45c2482aa3eeff4a9e8f5128357f7e65.png)

而我们仅需填写如下配置，即可使用 Filesystem MCP：

```
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/path/to/other/allowed/dir"
      ]
    }
  }
}
```

例如我们在 `servers_config.json` 中写入 MCP 工具配置：

![](img/5c6a22af947028777276241d76b35465.png)

![](img/6730d16eb0fd4b5872e9c88135972c03.png)

```
{
  "mcpServers": {
    "weather": {
      "command": "python",
      "args": ["weather_server.py"],
      "transport": "stdio"
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/root/autodl-tmp/MCP/LangGraph/mcp-get-weather"
      ],
      "transport": "stdio"
    }
  }  
}
```

实际运行效果如下所示：

![](img/79e265a92f661e9f1133f7010d6f7e2d.png)

#### 3.2 FireCrawl MCP 工具接入

FireCrawl 是一款专为现代人工智能应用而设计的高性能网页抓取与结构化内容提取框架。它致力于通过自动化的方式，将互联网海量的非结构化网页数据转化为可被大模型理解、检索和推理的标准化知识。FireCrawl 不仅支持对静态和动态网页进行深度爬取，还能够自动识别并提取页面中的正文、元数据、表格、代码片段以及多媒体内容，形成丰富的上下文语料。该工具在多语言环境下同样表现出色，可在全球范围内快速搭建定制化的知识抓取流程。通过与矢量数据库、检索增强生成（RAG）系统和 MCP 协议的无缝集成，FireCrawl 为企业和研究机构提供了一种高效、可扩展的解决方案，使大型语言模型具备最新的实时知识获取与更新能力，显著提升其在问答、分析与内容生成等场景中的智能化水平。

![](img/bcff3d13e7414324ba5bc3c3794e92a4.png)

+ FireCrawl 官网地址：https://www.firecrawl.dev/
+ FireCrawl MCP 官网地址 https://github.com/mendableai/firecrawl-mcp-server

![](img/9feac5fdd7ed661a521c23400a6457a9.png)

紧接着改写配置文件内容如下：

```
{
  "mcpServers": {
    "weather": {
      "command": "python",
      "args": ["weather_server.py"],
      "transport": "stdio"
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/root/autodl-tmp/MCP/LangGraph/mcp-get-weather"
      ],
      "transport": "stdio"
    },
    "mcp-server-firecrawl": {
      "command": "npx",
      "args": ["-y", "firecrawl-mcp"],
      "env": {
        "FIRECRAWL_API_KEY": "YOUR_API_KEY"
      },
      "transport": "stdio"
    }      
  }  
}
```

![](img/fea6ca9fbeffe95c27357dadf53c5d72.png)

实际运行效果如下:

![](img/667567cf08f5e785361dc2b2d2badaa6.png)

![](img/142be576d424f312946e2492f2488fb2.png)

## 三、将 LangGraph 封装为 MCP 工具

作为双向 MCP 工具，我们不仅能借助 LangGraph 来创建 MCP 客户端并搭建智能体，我们还能将已经开发好的 LangGraph 项目便捷的封装为 MCP 工具。我们这里将此前公开课介绍的数据分析 Agent 封装为一个 MCP 工具。

项目完整教程已上线至赋范大模型技术社区，大家扫码即可领取：

![](img/673de21eba60bc05cefc98fa32cde499.png)

### 1. LangGraph 项目一般结构与开发工具回顾

+ LangGraph 项目结构与开发流程

![](img/7f639d2c4c2ae0cb20ebbe0b21322d7f.png)

+ LangGraph 服务部署工具：LangGraph CLI

+ LangGraph 运行监控框架：LangSmith

![](img/c6cb465e9beca47f18aa94c7cddbf1a2.png)

+ LangGraph 图结构可视化与调试框架：LangGraph Studio

![](img/f2bb62ba4fad5d37f96e62c3b9e3e797.png)

### 2. Data Agent 服务启动流程

LangGraph 智能体后端服务对 MCP 功能是完全兼容的，一旦我们顺利开启 LangGraph 后端服务，即可在 `/mcp` 路由端口以流式 HTTP 模式调用 LangGraph 的智能体各项功能。这也是最便捷的将 LangGraph 智能体封装为 MCP 工具的方法。

为此，我们需要先启动此前公开课创建的 Data Agent 服务。

#### Step 1. 下载项目完整源码

扫码添加助教老师领取课件网盘，下载 Data Agent 完整项目源码：

![](img/60eff2a9a122edcae3dee06328599b18.png)

![](img/42255937b7389a3ec7e9008c0d06be27.png)

#### Step 2. 配置环境

紧接着需要修改 `.env` 配置文件，需要填写的内容如下，需要注意的是，如果不需要连接本地 MySQL 数据库，相关配置空着即可。

+ LangSmith API Key 获取

+ LangSmith 需要先登录 LangSmith 官网进行注册，https://www.langchain.com/langsmith，

+ 然后选择开启项目追踪功能，

+ 然后点击创建 API-KEY 即可。

+ Tavily 搜索引擎 API KEY 获取

+ 获取用于进行网络搜索的 Tavily API Key，https://www.tavily.com/，同样需要在 Tavily 官网注册账号，

+ 然后在个人主页即可获取 API-KEY，

![](img/21153dafc09d8cabf3efe98d6d5cf09d.png)

然后输入如下命令，安装项目依赖：

```
pip install -r requirements.txt
```

#### Step 3. 开启 Data Agent 项目后端服务

紧接着即可启动后端服务了：

```
pip install -U "langgraph-cli[inmem]"
langgraph dev
```

![](img/16430416ad1153b7403721ade221c2b4.png)

其中 2024 是当前智能体后端服务端口，而 `2024/docs` 则是后端接口功能说明

![](img/66d160a9c78af35b2da67bcf39f4c30c.png)

而在 LangSmith 官网主页 https://www.langchain.com/langsmith，就能看到每次对话追踪的效果，

![](img/a881fde533d8ce8cfddcd041ad7bd4fd.png)

而第二个链接则是用于可视化展示项目结构和在线功能测试的功能组件：LangGraph Studio

![](img/dd3b60a86bac6c504438f2abf393edee.png)

#### 【可选】Step 4. 开启前端对话

此外，我们还可以将其接入前端。而最适配的前端，当然是 LangChain 自带的 Agent Chat UI，这个前端不仅页面美观，而且支持定制化二次开发。我们可以在 Agent Chat UI 的 GitHub 主页上下载源码并进行部署，

![](img/bc9a58380a6fb486812b1a905a888304.png)

也可以直接在课件网盘中下载源码压缩包，

![](img/61c9c64aa4a8315a707465165b81b1ee.png)

然后在当前项目主目录内进行解压缩，然后输入如下命令安装依赖和开启前端：

```
pnpm install
pnpm dev
```

![](img/4b187c8f904d6231ec349ce35b18d142.png)

然后即可在 http://localhost:3000 通过前端页面访问 Data Agent 服务了。

![](img/fb10583175682687e29678091b3a1745.png)

### 3. 将 Data Agent 封装为 MCP 工具

顺利开启后端服务后，我们就能在 http://127.0.0.1:2024/mcp 处，以流式传输的 MCP 工具形式对其进行调用。例如现在保持 Data Agent 后端服务开启状态，然后回到我们的 LangGraph MCP 项目中，在 MCP 工具配置文件中，加上 Data Agent 服务端口：

![](img/a9a1161045cb67da507f2dc5ffb6ab89.png)



```
{
  "mcpServers": {
    "weather": {
      "command": "python",
      "args": ["weather_server.py"],
      "transport": "stdio"
    },
    "write": {
      "command": "python",
      "args": ["write_server.py"],
      "transport": "stdio"
    },
    "data_analysis": {
      "url": "http://localhost:2024/mcp",
      "transport": "streamable_http"
    },
  }
}
```

并修改提示词模板：

![](img/72a5f180418093c5fb242e5ae2ce3656.png)

```
你是一个智能体，具备以下三种能力：

1.查询天气：调用query_weather(city: str)，返回指定城市的实时天气。

2.写入文件：调用write_file(content: str)，将文本内容写入本地文件并返回路径。

3.数据查询与分析，当用户的问题需要上网搜索、查询数据库或者需要进行数据分析时，请调用chatbot工具。此时请创建如下格式消息进行调用：{"type": "human", "content": user_input}

当用户提出请求时，你需要理解意图并选择相应的工具。如果请求缺少必要信息，先与用户确认后再调用工具。返回结果时以简洁、友好的方式回复。

如果用户提出的问题与你的功能无关，请礼貌告知无法处理。
```

然后开启 LangGraph MCP Agent：

```
uv run client.py
```

实际演示效果如下：

![](img/e538a3db1be465600ead920f05891304.png)

此时在 LangSmith 中就能实时查看当前对话情况：

![](img/0ddb5a9587618d2c4fdadf5c63598898.png)

## 四、LangGraph 接入 HTTP MCP 智能体

### 1. 接入在线 MCP 服务流程

在 SSE (Server-Sent Events) 模式下，我们可以把 MCP 服务器当成一个已经在网络上跑着的「工具 API」，用 URL 建立事件流、再用 `POST /messages` 发指令。当前主流的使用形式可以归纳为两大类：自托管启动与平台托管直连。

+ 自托管启动（Self-hosted）

  这种方式主要是应用 [MCP](https://github.com/modelcontextprotocol) 官方的 Python / TypeScript MCP SDK 把 Server 代码跑起来（FastAPI / Express 集成），程序启动后会打印或返回一个本地或内网 SSE URL，然后便可以用这个 URL 建立事件流接入到 OpenAI Agents SDK 框架中。如果需要使用 MCP 官方的 Python SDK 构建 SSE 模式的 MCP 服务器，可以学习《【加餐】MCP SSE 与流式 HTTP 开发实战》课程模块，我们这里不再重复讲解。

+ 平台托管直连（Platform-hosted）

  诸如 [ModelScope](https://www.modelscope.cn/mcp) 的 MCP 广场、[mcp.run](https://docs.mcp.run/integrating/tutorials/mcp-run-sse-openai-agents) 等社区维护大量 MPC 服务并支持一键启动 MCP 的 SSE 模式。使用的方法非常简单，只需要在平台首页选好 MCP Server → 生成一个 API-Key，一键生成 SSE URL，即可直接接入使用。

对于国内开发者，通过“一键复制 URL + API-Key”就能拿到 SSE 端点的热门网站大致可以分为四类：

1. 专门的 MCP 服务市场/目录（如 MCP Market、AIbase、MCP Server Hub）；
2. 云厂商推出的官方托管平台（阿里云“百炼”与 Higress Marketplace）；
3. 行业 API 已内置 MCP & SSE（百度地图、腾讯位置服务等）；
4. IDE / 客户端集市把第三方 SSE URL 聚合到本地（Cherry Studio、Cursor）。

MCP 热门导航网站

| 平台 | 规模与特点 | 访问链接 |
| --- | --- | --- |
| ModelScope MCP 广场 | 超过 3000+ 个中文托管 MCP；详情页右侧直接复制 SSE URL，部分服务需在“API Key”栏目填 Token | [点击进入](https://www.modelscope.cn/mcp/) |
| MCP Market | 标榜“国内首个 MCP 服务市场”，已收录 1W+ SSE Server，并提供云托管与 Playground，一键生成专属 URL | [点击进入](https://mcpmarket.cn/) |
| AIbase MCP 资源站 | 整理 GitHub 上的热门 MCP Server 并直接给出 SSE 地址与使用教程 | [点击进入](https://mcp.aibase.cn/) |
| MCP Server Hub | 国际化，标签筛选 + “Copy URL” 操作流畅，站内统计数千条 SSE Server | [点击进入](https://mcpserverhub.com/) |
| 阿里云 百炼 | 官方托管“全周期 MCP 服务”，控制台能生成已鉴权的 SSE URL | [点击进入](https://bailian.console.aliyun.com/?spm=5176.29619931.0.0.1369521cW5hVGj&tab=mcp#/mcp-market) |

平台托管直连不需要开发者关心进程与依赖等问题，直接在网页一键生成 URL 即可接入。下面我们以 ModelScope 的 MCP 广场为例，演示如何在 OpenAI Agents SDK 框架中通过 SSE 模式接入 MCP 服务器。

+ Step 1. 进入 ModelScope 的 MCP 广场，访问地址：`https://www.modelscope.cn/mcp`

![](img/81c2718e94c5de37407de0b66a2bada8.png)

+ Step 2. 点击右上角登录按钮，登录 ModelScope 账号

  可以通过手机号注册并登录，注意：国内用户记得选择中国大陆 +86 的手机号注册，否则会提示手机号格式错误。

![](img/eee6f0da27befeeea9406911abe53444.png)

+ Step 3. 挑选 MCP 服务，并进入详情页

  这里我们选择的是 `12306-MCP` 车票查询工具，该 MCP 服务提供了搜索 12306 购票信息的功能。

![](img/f0ba9acf5b2370811e318ad32d91bcf9.png)

+ Step 4. 点击立即使用按钮，进入 MCP 服务详情页

  如果是首次使用，在详情页的最右侧会有一个通过 SSE URL 连接服务的说明

![](img/cb1fe8d496c68034714a37b2fab97831.png)

  点击连接按钮，即可生成一个 SSE URL，如下图所示：

![](img/96d901e67d517e5a7db17d3dadd6187b.png)

+ Step 5. 修改配置文件

![](img/5308bc3b183e809ed540fca37971308f.png)

```
{
  "mcpServers": {
    "12306-mcp": {
      "url": "https://mcp.api-inference.modelscope.net/f38521aeede344/sse",
      "transport": "sse"
    }
  }
}
```

+ 实际运行效果

![](img/d080d7a709ce17c84e1c2a4b8f050019.png)

上述过程中，MCP 适配器承担了接口映射的工作——它将 MCP Server 提供的工具描述转化为 LangGraph 的工具对象，包括工具名称、功能说明、输入输出参数类型等。大模型在推理中根据工具描述和对话上下文决定何时调用哪个工具，LangGraph 负责将调用意图映射为实际的函数调用。调用时，MCP 客户端会将函数参数通过既定格式发送给服务器执行，并将结果以标准化的 JSON 格式返回给 LangGraph 代理。这一标准通信过程无需开发者处理底层序列化和协议细节，实现了上下文和数据在 LangGraph 与 MCP 之间的透明传递。模型每次调用工具时，当前对话信息会作为提示的一部分（通常由 LangGraph agent 在内部 prompt 中包含工具使用说明），因此模型“知道”何时以及如何使用这些工具来获取所需信息。

### 2. 搭建可视化 MCP 智能体开发平台

掌握了 LangGraph 接入 MCP Server 的原理，接下来我们通过一个实际的案例来进一步加深大家对 LangGraph + MCP Server 的应用。我们实现了一个基于 Streamlit 的智能 AI 代理平台，其包含的主要功能是：

+ 多 MCP 工具集成平台：支持动态添加和管理多个 MCP（Model Context Protocol）工具
+ 智能对话代理：基于 LangGraph 创建的 ReAct（推理-行动）代理，能够智能选择和使用工具
+ 可视化 Web 界面：提供友好的用户界面进行对话和工具管理
+ 多模型支持：目前主要支持 DeepSeek 模型，架构可扩展支持其他 LLM

如下是该系统的运行效果演示：

完整项目代码已上传至课件网盘，大家扫码即可领取：

![](img/fecce6da210e72c28af025a0d1b677b9.png)

![](img/1ddc9e2fd5ad213ce086aad19207ca9b.png)

接下来我们逐步讲解如何在本地部署并启动 `LangGraph_Multi_MCP` 应用。

下载源码文件后，首先需要新建一个 `venv` 环境：

![](img/6094dc25fc0e52270ba1549f76948472.png)

进入该虚拟环境后，进行依赖项的安装：

![](img/f98f1ba8c4c3612c7985fede38c13.png)

接下来，在启动前需要配置 `.env` 文件，包含以下配置：

```
# === 必需配置 ===
# DeepSeek API密钥（必须）
DEEPSEEK_API_KEY=your_deepseek_api_key_here

# === 可选配置 ===
# 登录功能开关
USE_LOGIN=true                    # true启用登录，false禁用

# 用户认证信息（仅在USE_LOGIN=true时需要）
USER_ID=your_username            # 登录用户名
USER_PASSWORD=your_password      # 登录密码

# LangSmith追踪（可选）
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=your_langsmith_key
LANGSMITH_PROJECT=langgraph_mcp_project
```

其次，大家可以参考 `mcp_server_time.py` 文件，来实现内置的 MCP Server，然后将其写入 `config.json` 文件中。

```
{
  "get_current_time": {
    "command": "python",
    "args": [
      "./mcp_server_time.py"
    ],
    "transport": "stdio"
  }
}
```

注意：这里可以写多个 MCP Server 的配置，大家可以根据自己的需求进行开发和应用。注意：前端界面中的添加和删除 MCP Server 的功能，其实就是对 `config.json` 文件的修改。

修改完以上配置后，便可以在控制台运行项目了。执行如下命令：

```
streamlit run app.py
```

![](img/4d77b5d3e28a705acc3c1a094046e603.png)

首次登录时，需要输入用户名和密码，这里的用户名和密码是在 `.env` 文件中配置的，按照自己的设置进行登录校验即可。登录成功后，便可以进入系统的主界面进行交互了。

![](img/93d8c6f0d6dac1ddee8da4ae30759ed0.png)

---

+ 体验课内容节选自《2025 大模型 Agent 智能体开发实战》(夏季班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025 大模型 Agent 智能体开发实战》(夏季班) https://ix9mq.xetslk.com/s/3u765N)

![](img/8f2fbd2f64232ca25a2a2fa3dbfa6180.png)

《2025 大模型 Agent 智能体开发实战》(夏季班) 为【100+ 小时】体系大课，总共 20 大模块精讲精析，零基础直达大模型企业级应用！

![](img/5bb06ba0188ede0e03c2184cf6ba2d8a.png)

同时，5 月班重磅新增 DeepSeek+Agents SDK+谷歌 ADK+MCP 技术应用与智能体开发相关实战内容，并计划新增 Qwen-3 模型实战教学：

![](img/424cf4fa268581c95b5f01fc9aecc049.png)

### 部分课程成果演示

+ Dify+DeepSeek 搭建智能客服
+ 可视化数据分析 Multi-Agent
+ Ollama 自动化并发请求测试与动态资源监控
+ Neo4j 并行多线程导入百万级文本方法与实践
+ MateGen Pro 项目功能演示
+ 智能客服项目展示

此外，若是对大模型底层原理感兴趣，也欢迎报名由我和菜菜老师共同主讲的《2025 大模型原理与实战课程》(夏季班)

![](img/448fa49f8db240ae8b82ac0e530ab22d.png)

**两门大模型课程夏季班目前上新特惠，直播间下单可享 618 平价钜惠，合购还有更多优惠哦~** 详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/fe1ba47d345c48de1c63afa827846e59.png)

![](img/94c7c17f32ee7fcf77cf157a9d3e9bd9.png)
