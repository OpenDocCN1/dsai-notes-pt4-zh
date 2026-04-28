# Part 2.DeepSeek-V3.2+LangChain 1.1 Agent开发实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Ts0Vd1QEQoUaAXxdvc4cXUQLnoO](https://kq4b3vgg5b.feishu.cn/docx/Ts0Vd1QEQoUaAXxdvc4cXUQLnoO)

## 课程说明：

+   体验课内容节选自《2025大模型Agent智能体开发实战》(12月班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(12月班)

![](img/9d4b260fcc287d65503c56307740acd1.png)

![](img/2a1197730118dd726ea60eb33143d5f2.png)

---

# 国产最强DeepSeek-V3.2 Agent开发实战

## Part 2.DeepSeek-V3.2+LangChain 1.1 Agent开发实战

## 一、LangChain技术生态回顾

**在** **LangCh**ain 1.x 中，最大的变化肯定是推出了全新的Agent API：create_agent。至此，LangChain Agent就已经不再是早期那种“带点魔法味的模型调用器”，它正式被定义为：一个可感知上下文、具备行动能力、可扩展、可插拔的智能运行体（Intelligent Runtime Unit）。简单来说，Agent 不再只是“用大模型回答问题”，而是能根据任务动态调用工具、推理决策、规划步骤，并与外部世界进行交互的 自治式执行体。

+   LangChain Agent API大一统

LangChain 在 1.0 版本中，将所有 Agent 的创建方式统一为了一个入口——create_agent()。它取代了旧版本中的 create_react_agent、create_json_agent、create_tool_calling_agent 等多种分支函数，真正让开发者用一行代码即可创建任何类型的智能体。

在 LangChain 0.x 时代，框架内的 Agent 系统经历了“碎片化”阶段。当时的设计理念是 “针对场景设计特定 Agent”——如果你要实现思维链推理（ReAct），就用 create_react_agent；如果需要结构化输出，就用 create_structured_chat_agent；要工具调用，则用 create_tool_calling_agent。这种方式灵活，但也带来了三个明显问题：

1.  **心智负担高**——每种 Agent 都要单独记忆 API 与参数；

1.  **可组合性差**——多个 Agent 之间无法统一调度；

1.  **生态碎片化**——不同模块难以复用或协同演化。

而进入 LangChain 1.0 后，团队做出了彻底重构：将所有 Agent 的创建入口合并为 create_agent()，同时在底层通过“中间件机制（Middleware）”和“标准模型接口（invoke / stream）”实现全局统一。这让框架更轻、更稳，也更易于被集成到其他 Agent 平台中。

+   create_agent API极简调用流程

而LangChain 1.0中的create_agent API的实际调用过程也非常简单，从代码层面来看，我们只需要将模型和工具拼接到一起，即可完成Agent创建：

```
# 1.导入相关库
from langchain.agents import create_agent
from langchain_deepseek import ChatDeepSeek
from langchain_community.tools.tavily_search import TavilySearchResults

# 2.导入模型和工具
web_search = TavilySearchResults(max_results=2)
model = ChatDeepSeek(model=&quot;deepseek-chat&quot;)

# 3.创建Agent
agent = create_agent(
    model=model,
    tools=[web_search],
    system_prompt=&quot;你是一名多才多艺的智能助手，可以调用工具帮助用户解决问题。&quot;
)

# 4.运行Agent获得结果
result = agent.invoke(
    {&quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请帮我查询2024年诺贝尔物理学奖得主是谁？&quot;}]}
)
```

```
result[&#x27;messages&#x27;][-1].content
```

```
&#x27;根据查询结果，2024年诺贝尔物理学奖得主已经揭晓：\n\n**获奖者：**\n- **John J. Hopfield**\n- **Geoffrey E. Hinton**\n\n**获奖理由：** &quot;表彰他们通过人工神经网络实现机器学习的基础性发现和发明&quot;\n\n**奖金：** 1100万瑞典克朗（约合745万元人民币）\n\n**颁奖时间：** 2024年10月8日\n\n这两位科学家在人工神经网络和机器学习领域做出了开创性的贡献，他们的工作为现代人工智能的发展奠定了重要基础。&#x27;
```

+   create_agent API底层LangGraph架构

而从底层架构来看，从 LangChain 1.0 开始，官方对框架的整体架构进行了根本性的角色重置：LangGraph 被正式确立为 LangChain 的底层运行框架（Runtime Foundation），而非附属扩展。在 0.x 时代，LangGraph 还被视作 LangChain 的“高层封装”——它是开发者在 LangChain 基础上进一步实现复杂 Agent 编排、状态流转与多节点控制的可选工具；换言之，那时 LangChain 是核心、LangGraph 是外挂。而进入 1.0 之后，情况彻底反转：LangGraph 被下沉为 LangChain 的底层执行引擎，一切新的高级 API（包括统一的 create_agent()、中间件系统、状态管理机制、流式回调与事件调度等）都直接运行在 LangGraph 架构之上。这样一来，LangChain 不再只是一个“Prompt 组织与调用库”，而是一个基于图结构的智能体运行时系统：每个 Agent 的生命周期、每次模型调用、每个工具执行，都是图中一个节点或边的执行过程。LangGraph 提供了稳定的调度、状态传递与事件流控制，而 LangChain 则提供了上层接口与开发体验。也就是说，上述Agent的实际运行逻辑，如果用LangGraph的语言来描述就是：

![](img/1af59ce62605f40e397886ae90c47093.png)

这种统一架构并以LangGraph为底层框架带来的优势如下：

1.  **接口统一，降低心智负担**

+   无论是工具型、规划型还是对话型 Agent，都用同一个入口创建。

+   开发者不再需要区分不同 Agent 的实现逻辑。

1.  **跨生态兼容性增强**

+   LangChain 1.0 的 Agent 架构与 LangGraph、LangServe、甚至 OpenAI Responses API 都完全兼容。

+   这意味着同一个 Agent 可以同时运行在命令行、Web UI、LangGraph 节点或云端推理环境中。

1.  **中间件机制的加入**

+   新版本允许通过装饰器或中间件在 Agent 生命周期中插入逻辑：
如日志、速率限制、记忆压缩、PII 审查、内容重写等。

+   这使得 Agent 更像一个“微型操作系统进程”，具备可观察性与可管理性。

1.  **可扩展性与定制性更强**

+   统一 API 底层基于标准化状态流（State Graph），
这意味着用户可以自由地扩展 Agent 行为而不破坏核心逻辑。

### 1.LangChain Agent开发必备工具套件回顾

在对LangChain 1.0有了一定的基础了解之后，对于开发者来说，还需要进一步了解和掌握LangChain Agent必备的开发者套件。分别是LangChain Agent运行监控框架LangSmith、底层LangGraph图结构可视化与调试框架LangGraph Studio和LangGraph服务部署工具LangGraph Cli。可以说这些开发工具套件，是真正推动LangGraph的企业级应用开发效率大幅提升的关键。同时监控、调试和部署工具，也是全新一代企业级Agent开发框架的必备工具，也是开发者必须要掌握的基础工具。

#### 1.1 LangGraph运行监控框架：LangSmith

> LangSmith官网地址：https://docs.smith.langchain.com/

LangSmith 是一款用于构建、调试、可视化和评估 LLM 工作流的全生命周期开发平台。它聚焦的不是模型训练，而是我们在构建 AI 应用（尤其是多工具 Agent、LangChain/Graph）时的「可视化调试」、「性能评估」与「运维监控」。

![](img/a3f317ea3408acdba3d242512744cc9d.png)

| 功能类别 | 描述 | 场景 |
| --- | --- | --- |
| 🧪 调试追踪（Trace Debugging） | 可视化展示每个 LLM 调用、工具调用、Prompt、输入输出 | Agent 调试、Graph 调用链分析 |
| 📊 评估（Evaluation） | 支持自动评估多个输入样本的回答质量，可自定义评分维度 | 批量测试 LLM 表现、A/B 对比 |
| 🧵 会话记录（Sessions / Runs） | 每次 chain 或 agent 的运行都会被记录为一个 Run，可溯源 | Agent 问题诊断、用户问题分析 |
| 🔧 Prompt 管理器（Prompt Registry） | 保存、版本控制、调用历史 prompt | 多版本 prompt 迭代测试 |
| 📈 流量监控（Telemetry） | 实时查看运行次数、错误率、响应时间等 | 在生产环境中监控 Agent 质量 |
| 📁 Dataset 管理 | 管理自定义测试集样本，支持自动化评估 | 微调前评估、数据对比实验 |
| 📜 LangGraph 可视化 | 对 LangGraph 中每个节点运行情况进行实时可视化展示 | Graph 执行追踪 |

#### 1.2 LangGraph图结构可视化与调试框架：LangGraph Studio

> LangGraph Studio官网地址：https://www.langgraph.dev/studio

LangGraph Studio 是一个用于可视化构建、测试、分享和部署智能体流程图的图形化 IDE + 运行平台。

![](img/f40e4cdee9c50c8c8c152cf3d32182ee.png)

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

#### 1.3 LangGraph服务部署工具：LangGraph Cli

> LangGraph Cli官网地址：https://www.langgraph.dev/ （需要代理环境）

LangGraph CLI 是用于本地启动、调试、测试和托管 LangGraph 智能体图的开发者命令行工具。

![](img/9228dc66c1c01428f80467bcb275f2bd.png)

| 功能类别 | 命令示例 | 说明 |
| --- | --- | --- |
| ✅ 启动 Graph 服务 | langgraph dev | 启动 Graph 的开发服务器，供前端（如 Agent Chat UI）调用 |
| 🧪 测试 Graph 输入 | langgraph run graph:graph --input &#x27;{&quot;input&quot;: &quot;你好&quot;}&#x27; | 本地 CLI 输入测试，输出结果 |
| 🧭 管理项目结构 | langgraph init | 初始化一个标准 Graph 项目目录结构 |
| 📦 部署 Graph（未来） | langgraph deploy（预留） | 发布 graph 至 LangGraph 云端（已对接 Studio） |
| 🧱 显示 Assistant 列表 | langgraph list | 显示当前 graph 中有哪些 assistant（即 entrypoint） |
| 🔄 重载运行时 | 自动热重载 | 修改 graph.py 时，dev 模式自动重启生效 |

而一旦应用成功部署上线，LangGraph Cli还会非常贴心的提供后端接口说明文档：

![](img/28526593c7e6422c72b54d2719461a2c.png)

而对于LangGraph构建的智能体，除了能够本地部署外，官方也提供了云托管服务，借助LangGraph Platform，开发者可以将构建的智能体 Graph部署到云端，并允许公开访问，同时支持支持长时间运行、文件上传、外部 API 调用、Studio 集成等功能。

#### 1.4 LangGraph Agent前端可视化工具：Agent Chat UI

Agent Chat UI 是 LangGraph/LangChain 官方提供的多智能体前端对话面板，用于与后端 Agent（Graph 或 Chain）进行实时互动，支持上传文件、多工具协同、结构化输出、多轮对话、调试标注等功能。

> Agent Chat UI官网地址：https://langchain-ai.github.io/langgraph/agents/ui/

![](img/d18daa4feb16faf11205374254895155.png)

| 功能模块 | 描述 | 应用场景 |
| --- | --- | --- |
| 💬 多轮对话框 | 类似 ChatGPT 的输入区域，支持多轮上下文 | 用户提问，Agent 回复 |
| 🛠 工具调用轨迹显示 | 显示每个调用的工具、参数、结果（结构化） | Agent 推理透明化 |
| 📄 上传 PDF / 图片 | 支持上传文档、图片、嵌入多模态输入 | RAG、OCR、图像问答 |
| 📁 文件面板 | 可查看上传历史文件、删除、重新引用 | 管理文档输入 |
| 🧭 Assistant 切换 | 支持切换不同 Assistant（Graph entry） | 一键切换模型能力（如 math / weather） |
| 🧩 插件支持 | 与 MCP 工具、LangGraph 图打通 | 工具式 Agent 调用 |
| 🔍 调试视图 | 显示每轮 Agent 的思维过程和中间状态 | Prompt 调试、模型行为分析 |
| 🌐 云部署支持 | 支持接入远端 Graph API（如 dev 服务器） | 前后端分离部署 |
| 🧪 与 LangSmith 对接（可选） | 若后端启用 tracing，可同步显示运行 trace | 调试闭环 |

### 2.借助LangGraph Cli创建完整智能体项目

#### 2.1 LangGraph智能体项目说明

LangSmith 与 LangGraph Studio 都是 LangChain AI 生态中非常核心的工具，前者是用于跟踪和分析大模型的使用情况，而langGraph Studio则是对于LangChian Agent来说，则是比LangSmith更加方便和高效的可视化调试工具平台。

大家可以思考一个问题：基于LangGraph框架可以开发出各种复杂的应用、Agent、Workflow等，那么这些应用、Agent、Workflow等在生产中如何部署和运行呢？ 解决方案就是把它们部署成一个Server。 而对如何方便高效的把LangGraph的Graph部署成一个Server，LangGraph官方提供了LangGraph Platform，其完整架构如下所示：

![](img/2003a6358b25e3529a302a8d2a285c4d.png)

+   LangGraph Studio：桌面版应用（目前仅支持Mac）和本地运行（适用于所有操作系统）；

+   **LangServer**：最终构建出来的服务，提供Assistant API接口；

+   **Python/JS SDK**：通过接口可以直接和 LangServer 提供的各个API接口连接；

+   **Remote Graph**：类似于之前LangServe的用法，可以直接用 Graph 的接口去调用，这样拿到的Graph就是一个 Runable对象，就可以去调用它的invoke，batch 等。

LangGraph Studio 是专为 LangGraph 图式代理打造的本地/云端 IDE，具备可视化节点和状态及实时调试功能。LangGraph Studio 在本地可视化运行时会自动把调用过程上传到 LangSmith；而在 LangSmith 网页端查看任何 Trace 时，又能一键Run in Studio回放整条执行链，所以它是通过统一 Trace SDK 与 LangSmith 紧密集成。而LangGraph CLI则是构建这个项目的关键

接下来我们就来详细的讲解下如何使用LangGraph CLI来创建一个完整的LangGraph Agent项目，并在此过程中使用LangGraph Chat Agent UI进行前端对话，以及使用LangGraph Studio进行架构实时演示，并使用LangSmith进行运行效果监督。

首先需要说明的是，如果想要把一个定义的Graph添加到LangGraph Studio中，需要严格按照官方要求的项目结构进行构建，需要执行的步骤依次是：

#### 2.1 创建完整LangGraph智能体项目流程

+   **Step 1. 创建一个****LangChain Agent****项目主文件夹**

我们这里创建一个LangChain Chatbot文件夹，如下图所示：

![](img/70092a71dbfab0237184c76dd7afea51.png)

+   **Step 2. 创建****re**quirements.txt文件

在LangChain Chatbot文件夹中，新建一个requirements.txt文件，里面需要填写在运行该项目时需要安装的依赖项，如下所示：

```
langchain
langchain-deepseek
python-dotenv
langsmith
pydantic
matplotlib
seaborn
pandas
IPython
```

![](img/e770302615445ab6640a1d9c8d8814da.png)

+   Step 3. 注册LangSmith（可选）

对于企业级的Agent项目，为了更好的监控智能体实时运行情况，我们可以考虑借助LangSmith进行追踪（会将智能体运行情况实时上传到LangGraph官网并进行展示）。

要开始使用 LangSmith，我们需要创建一个帐户。可以在这里注册一个免费帐户进入LangSmith登录页面： https://smith.langchain.com/ ， 支持使用 Google、GitHub、Discord 和电子邮件登录。

![](img/9519c605b2db494ed82fb411747c26e7.png)

注册并等登录后，可以直接查看到仪表板：

![](img/25f0c332496318caf52d7dc7100082b5.png)

![](img/24396cdc1ce3d6248e81fe25e75e3f02.png)

![](img/fd1f5d047004a6ad83058bab61301ff0.png)

![](img/c1837a37ecacd23420526f16410d6235.png)

在构建程序跟踪前，首先需要创建一个 API 密钥，该密钥将允许我们的项目开始向 Langsmith 发送跟踪数据。创建完密钥后，在后续配置环境变量环节设置开启追踪、并输入密钥即可接入LangSmith。

+   **Step 4. 创建****.env****配置文件**

在LangChain Chatbot文件夹中，新建一个.env文件，将敏感信息（如API密钥）放在环境变量中而不是硬编码。如下所示：

![](img/57156844213d55cdc9e14cc92c667adf.png)

这里需要注意的是，如果不设置LangSmith，则无需设置中间三个环境变量，而具体工具也可以根据实际需求进行设置。

+   **Step 5. 创建****agent.py****核心文件**

在LangChain Chatbot文件夹中，新建一个agent.py文件，在该文件中编写构建图的具体运行逻辑，如状态、节点、变、图的编译等。此外，在使用LangGraph CLI创建智能体项目时，会自动设置记忆相关内容，并进行持久化记忆存储，无需手动设置。因此此时智能体代码如下所示：

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
from langchain_tavily import TavilySearch
from langchain_core.tools import tool
from pydantic import BaseModel, Field
import requests,json

# 加载环境变量
load_dotenv(override=True)

# 内置搜索工具
search_tool = TavilySearch(max_results=5, topic=&quot;general&quot;)

class WeatherQuery(BaseModel):
    loc: str = Field(description=&quot;The location name of the city&quot;)

@tool(args_schema = WeatherQuery)
def get_weather(loc):
    &quot;&quot;&quot;
    查询即时天气函数
    :param loc: 必要参数，字符串类型，用于表示查询天气的具体城市名称，\
    注意，中国的城市需要用对应城市的英文名称代替，例如如果需要查询北京市天气，则loc参数需要输入&#x27;Beijing&#x27;；
    :return：OpenWeather API查询即时天气的结果，具体URL请求地址为：https://api.openweathermap.org/data/2.5/weather\
    返回结果对象类型为解析之后的JSON格式对象，并用字符串形式进行表示，其中包含了全部重要的天气信息
    &quot;&quot;&quot;
    # Step 1.构建请求
    url = &quot;https://api.openweathermap.org/data/2.5/weather&quot;

    # Step 2.设置查询参数
    params = {
        &quot;q&quot;: loc,               
        &quot;appid&quot;: os.getenv(&quot;OPENWEATHER_API_KEY&quot;),    # 输入API key
        &quot;units&quot;: &quot;metric&quot;,            # 使用摄氏度而不是华氏度
        &quot;lang&quot;:&quot;zh_cn&quot;                # 输出语言为简体中文
    }

    # Step 3.发送GET请求
    response = requests.get(url, params=params)
    
    # Step 4.解析响应
    data = response.json()
    return json.dumps(data)

tools = [search_tool, get_weatreasoner

# 创建模型
model = ChatDeepSeek(model=&quot;deepseek-chat&quot;)

prompt = &quot;&quot;&quot;
你是一名乐于助人的智能助手，擅长根据用户的问题选择合适的工具来查询信息并回答。

当用户的问题涉及**天气信息**时，你应优先调用`get_weather`工具，查询用户指定城市的实时天气，并在回答中总结查询结果。

当用户的问题涉及**新闻、事件、实时动态**时，你应优先调用`search_tool`工具，检索相关的最新信息，并在回答中简要概述。

如果问题既包含天气又包含新闻，请先使用`get_weather`查询天气，再使用`search_tool`查询新闻，最后将结果合并后回复用户。

所有回答应使用**简体中文**，条理清晰、简洁友好。
&quot;&quot;&quot;


# 创建图
graph = create_react_agent(model=model, 
            rompt=prompt)天气时候，请使用get_weather工具查询天气。当用户询问新闻时，请使用search_tool工具查询新闻。&quot;)
```

![](img/e8e48dbbca3792408634159bc6a5e10f.png)

+   **Step 6. 创建****langgraph.json****文件**

在LangChain Chatbot文件夹中，新建一个langgraph.json文件，在该json文件中配置项目信息，遵循规范如下所示：

+   必须包含 dependencies 和 graphs 字段

+   graphs 字段格式："图名": "文件路径:变量名"

+   配置文件必须放在与Python文件同级或更高级的目录

注意: 项目文件的名称必须为langgraph.json。如下所示：

![](img/818dc51edfc56ac2670399d93ecd5501.png)

```
{
  &quot;dependencies&quot;: [&quot;./&quot;],
  &quot;graphs&quot;: {
    &quot;chatbot&quot;: &quot;./graph.py:graph&quot;
  },
  &quot;env&quot;: &quot;.env&quot;
}
  
```

其中：

+   dependencies: ["./"] - 告诉LangGraph在当前目录查找依赖项（会自动读取requirements.txt）

+   chatbot: "./graph.py:graph" - 定义图名为chatbot，来自graph.py文件中的graph变量

+   env: ".env" - 指定环境变量文件位置

最终完整项目结构如下所示：

```
    ./langraph_chatbot/
    ├── graph.py              # 对应官方的 agent.py
    ├── requirements.txt      # ✅ 依赖管理
    ├── langgraph.json       # ✅ 配置文件
    └── .env                 # ✅ 环境变量
```

![](img/851e1f2826ef2b024f680ff6db651977.png)

完整的代码已经上传至百度网盘中的langgraph_chatbot文件夹中，大家可以扫描下方二维码免费领取

![](img/065a7f64e70c559c5ed098559ed99457.png)

![](img/7aa202a35d2aead1069294b3366651f5.png)

+   **Step 7. 安装****langgraph-cli****以及其他依赖**

然后，安装langgraph-cli依赖，执行如下代码：

```
    pip install -U &quot;langgraph-cli[inmem]&quot;
```

![](img/4cad30721ca1c619dde9b44651cad88d.png)

/>

然后进入到langgraph_chatbot文件夹，安装相关基础依赖：

```
pip install -r requirements.txt
```

![](img/0df7b8238985c6180ea33ebe1a4c45a8.png)

最后，进入到langgraph_chatbot文件夹，执行LangGraph dev即可启动项目

```
langgraph dev
```

![](img/4e03c0f069b6f18ec904df864eafbe41.png)

![](img/0e9d2b9b811fc54fd76e267c32225c73.png)

启动成功后能看到三个连接，其中第一个连接是当前部署完成后的服务端口，第二个是LangGraph Studio的可视化页面，第三个端口是端口说明。

### 3. LangChain Agent部署后调用流程

+   后端服务接口

这里我们首先可以看下第三个连接，其中包含了详细的接口调用方法：

![](img/d1f4fad5b4b9b4b6c6fd0058f0953813.png)

这些暴露的接口和调用方法，接下来就可以用于进行进一步开发和测试。

+   LangGraph Studio

然后我们可点击Studio UI中显示的链接，在浏览器中打开并访问Studio，如下所示：

![](img/8311066c1e6812e5f28bc8d3fbb09a55.png)

完整演示流程如下所示：

```
Video(&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/2025-06-26%2019-29-04.mp4&quot;, width=800, height=400)
```

Your browser does not support the video element.

+   LangSmith

而如果此前设置了追踪，此时就能在LangSmith中看到当前项目运行情况：

![](img/c3e84b054ae09ed2306f77f8eb48a343.png)

```
Video(&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/2025-06-26%2019-52-57.mp4&quot;, width=800, height=400)
```

Your browser does not support the video element.

### 4. LangChain Agent后端接入Agent Chat UI完整流程

+   项目主页：https://github.com/langchain-ai/agent-chat-ui

![](img/376bb294b9621e7b26d277df590a1fe2.png)

+   Step 1.克隆项目：

```

# git config --global http.proxy http://127.0.0.1:10080
# git config --global https.proxy http://127.0.0.1:10080

git clone https://github.com/langchain-ai/agent-chat-ui.git

cd agent-chat-ui
```

![](img/b7777e514cd4c2ac484ceb629cc5d9a0.png)

![](img/a06063b6b43488e52d5021584181abc8.png)

![](img/6c60b9ec7ad19efcab3fe946754f88b6.png)

> GitHub代理设置（若开启代理且端口为10080时）：

> git config --global http.proxy http://127.0.0.1:10080

> git config --global https.proxy http://127.0.0.1:10080

+   Step 2. 安装npm

node.js官网：https://nodejs.org

![](img/964dc10a320036a07543dc94afee1390.png)

```
npm install -g pnpm
pnpm -v
```

![](img/5d71b69da0e9f1593f6c61be6710c964.png)

+   Step 3. 安装前端项目依赖

```
pnpm install
```

![](img/c78e7b93bcca93a26504b858fe760def.png)

+   Step 4. 开启Chat Agent UI

```
pnpm dev
```

![](img/383ab0d0dd20e9d2ca8c44f9a3b04097.png)

![](img/1cfbaf28d2f53abe843370fd5c2fb5be.png)

![](img/d99ca6ec23881bec032a9d48421b2cfc.png)

```
Video(&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/2025-06-26%2020-03-25.mp4&quot;, width=800, height=400)
```

Your browser does not support the video element.

+   完整源码领取：

![](img/518e689d6650fbc04d4bfeb21c14bd86.png)

![](img/5aa2d4c89fbd4ae857a727463383c19c.png)

> 注意需要自己创建.env文件

## 二、基于DeepSeek-V3.2+LangChain 1.1的数据分析Agent开发实战

### 1. Data Agent项目介绍

#### **1.1 项目定位**

本项目是一个基于 Lan**gChain 1.**1 标准架构 与 LangGraph 编排引擎**开发的端到端数据分析智能体****。它旨在打破传统 BI** 工具的静态限制，通过集成 DeepSeek-V3.2 强大的代码生成与推理能力，实现“数据即对话，对话即洞察”的全新交互模式。用户只需上传 CSV 数据集，即可通过自然语言指令完成从数据清洗、统计分析到复杂可视化绘图的全流程任务。

#### 1.2 核心功能 (Core Features)

+   **全自动数据挂载与清洗**：支持 CSV 文件一键上传，后端自动执行类型推断、缺失值填充与 Schema 提取，实现数据“零配置”入库。

+   **内置 Python 代码解释器**：基于 ReAct 范式，Agent 能够自主编写、调试并执行 Pandas 代码，精准回答复杂的统计学问题（如“计算销售额同比增长率”），而非依赖大模型的幻觉。

+   **智能可视化绘图**：支持 Matplotlib/Seaborn 绘图指令的语义理解，自动解决中文乱码与 GUI 线程冲突问题，将自然语言（如“画出性别分布的柱状图”）实时转化为高质量图表。

+   **混合式交互分析**：结合了 LLM 的灵活性与传统算法的确定性，既支持开放式对话分析，也提供“相关性计算”等点击即用的确定性功能接口。

#### **1.3 技术亮点与教学价值 (T**echnical Highlights)

+   前沿架构实践 (LangChain 1.1 + LangGraph)：

+   摒弃过时的 AgentExecutor，全面采用 **LangGraph** 构建图式状态机，符合 2025 年 Agent 开发的最新工业标准，展示了可观测、可调试的现代 Agent 架构。

+   使用了 **create_agent**** 高阶 API**，演示了如何以最简洁的代码实现复杂的工具调用（Tool Calling）逻辑。

+   **动态上下文注入 (Dynamic** Context Middleware)：

+   创新性地使用了 Middl**eware (中间件) 机制与** @dynamic_prompt 技术。

+   解决了 System Prompt 僵化的问题，实现了 Agent 记忆的实时刷新——无论用户何时更换数据集，Agent 都能毫秒级感知最新的字段结构与数据摘要。

+   **企业级工程化落地**：

+   **前后端分离**：采用 FastAPI 构建高并发后端，配合 Figma/React 前端，展示了完整的全栈开发流程。

+   流式交互 (Streaming)：基于 SSE (Server-Sent Events) 协议实现打字机效果，并深度解析 LangGraph 的嵌套消息结构，实现了“思考过程”与“最终结果”的分层展示。

+   **稳健的工具沙箱**：通过重构 Python 执行环境（作用域合并、stdout 捕获、Headless 绘图模式），解决了生产环境中常见的执行报错与线程安全问题。

![](img/c31cea6f7732eeffe4e176234e54fe51.png)

![](img/09e489c4b6165c946bc16de099dd8cdc.png)

### 2. 后端架构说明

#### 2.1 整体架构设计理念 (Architectural Design)

本项目后端采用 “分层解耦 (Layered Decoupling)” 的设计思想，将系统划分为四个核心层次，确保代码的可维护性与扩展性：

+   接入层 **(Entry **Layer)：基于 FastAPI，负责 HTTP 请求处理、CORS 跨域安全、文件上传及静态资源（图片）挂载。

+   编排层 **(Orchestratio**n L**ayer)：基于 **LangChain 1.1 与 LangGraph，负责智能体的状态管理、工具路由分发及流式响应。

+   能力层 (Capability Layer)：自定义工具集（Tools），提供 Python 代码沙箱执行与 Matplotlib 绘图能力。

+   数据层 (Data Layer)：负责 Pandas DataFrame 的内存管理、数据清洗（ETL）与 Schema 提取。

#### 2.2 项目目录结构 (Directory Structure)

```
backend/
├── src/
│   ├── server.py       # [入口] FastAPI 服务端，集成 LangServe 与 API 路由
│   ├── agent.py        # [大脑] 定义 Agent Graph，配置模型与中间件
│   ├── tools.py        # [手脚] 封装 Python 执行器与绘图工具 (Matplotlib/Agg)
│   └── data_manager.py # [记忆] 全局数据状态管理、清洗与统计
├── static/             # [资源] 存放生成的图表图片，供前端访问
├── .env                # [配置] API Key 与环境变量
└── pyproject.toml      # [依赖] 项目依赖管理
```

#### 2.3 核心模块代码解析 (Module Deep Dive)

##### **A. 服务端 (****src/server.py****)**

+   **功能**：系统的“大门”。

+   **关键实现**：

+   **混合协议支持**：同时支持 RESTful API（文件上传 /upload）与 SSE 流式接口（对话 /agent/stream）。

+   **CORS 动态防御**：使用正则 (allow_origin_regex) 完美解决 Figma 动态沙盒域名与本地调试环境的跨域问题。

+   **静态挂载**：通过 app.mount("/static") 将生成的图表目录暴露为 Web 服务，让前端可通过 URL 访问图片。

##### **B. 智能体编排 (****src/agent.py****)**

+   **功能**：系统的“大脑”。

+   **关键实现**：

+   LangChain 1.1 标准：使用 create_agent 高阶 API，自动完成 Model 与 Tools 的绑定。

+   **动态上下文中间件 (****@dynamic_prompt****)**：这是本架构的**灵魂**。它拦截每一次请求，实时从数据层读取最新的 DataFrame 摘要（Schema），注入到 System Prompt 中。解决了“上传新数据后 Agent 依然只有旧记忆”的难题。

##### **C. 工具箱 (****src/tools.py****)**

+   **功能**：系统的“双手”。

+   **关键实现**：

+   **Headless 绘图模式**：强制设置 matplotlib.use('Agg')，防止在服务器端弹出 GUI 窗口导致线程崩溃。

+   **执行环境沙箱**：通过合并作用域 (env) 并结合 exec，既保证了代码执行的灵活性（支持列表推导式），又通过 contextlib 捕获了标准输出 (stdout)，让 Agent 能“看到”打印结果。

+   **中文乱码修复**：内置字体自动配置逻辑，自适应 Windows/Mac/Linux 环境。

##### **D. 数据管家 (****s**rc/data_manager.py)

+   **功能**：系统的“短期记忆”。

+   **关键实现**：

+   **单例模式管理**：在 Demo 阶段使用全局变量 (GLOBAL_DF) 存储 DataFrame，保证工具层与 Agent 层访问的是同一份数据。

+   **鲁棒性清洗**：在加载数据时自动处理混合类型列和缺失值，防止生成的 Python 代码因脏数据而报错。

#### 2.4 核心工作流 (Core Workflow)

场景：用户上传数据并询问“帮我画图”

1.  **数据注入**：用户 POST /upload -> data_manager 清洗数据 -> 存入内存 -> 返回 Schema 预览。

1.  **动态感知**：用户发消息 -> agent.py 中间件触发 -> 调用 get_data_info() -> 组装含最新列名的 Prompt -> 发送给 DeepSeek。

1.  **推理与执行**：

+   DeepSeek 生成绘图代码 -> LangGraph 路由到 fig_inter 工具。

+   tools.py 在后台（Agg模式）绘图 -> 保存至 /static/images -> 返回 IMAGE_GENERATED: xxx.png 标记。

1.  **结果渲染**：Server 通过 SSE 推送标记 -> 前端解析标记 -> 通过 URL 加载并展示图片。

![](img/bc8a54d060244aa31fc5cb068c27e44c.png)

+   核心Agent.py代码解释

```
import os
from langchain_deepseek import ChatDeepSeek
# [关键] 使用 LangChain 1.1 标准 API 和 中间件工具
from langchain.agents import create_agent
from langchain.agents.middleware import dynamic_prompt

# 导入工具和数据管理器
from src.tools import python_inter, fig_inter
from src.data_manager import get_data_info

# =============================================================================
# 1. 初始化模型
# =============================================================================
model = ChatDeepSeek(
    model=&quot;deepseek-chat&quot;,
    temperature=0,
    api_key=os.getenv(&quot;DEEPSEEK_API_KEY&quot;),
    api_base=&quot;https://api.deepseek.com&quot;
)

tools = [python_inter, fig_inter]

# =============================================================================
# 2. 定义动态 System Prompt 中间件
# =============================================================================

@dynamic_prompt
def dataset_context_middleware(request) -&gt; str:
    &quot;&quot;&quot;
    [中间件] 动态提示词生成器
    LangChain 会在每次调用 Agent 之前自动运行这个函数。
    
    request: 包含了当前的 ModelRequest (如 input, messages 等)
    返回: 最新的 System Prompt 字符串
    &quot;&quot;&quot;
    
    # 1. 实时去内存获取最新的数据摘要（包含文件名、行列数、列名等）
    # 只要用户上传了新文件，get_data_info() 的返回结果就会立刻变化
    data_context = get_data_info()
    
    # 2. 返回完整的 System Prompt
    return f&quot;&quot;&quot;你是一名精通 Python 的数据分析专家 DataAgent。

【当前数据集实时状态】
{data_context}

【你的职责】
1. 使用 `python_inter` 执行 Pandas 分析，或 `fig_inter` 进行绘图。
2. 变量 `df` 已内置，直接使用即可。
3. 绘图时请将对象赋值给变量，并调用绘图工具。
4. 遇到问题请先尝试分析报错信息。
&quot;&quot;&quot;

# =============================================================================
# 3. 创建 Agent (带中间件)
# =============================================================================

# 使用 LangChain 1.1 的 create_agent
# 注意：我们不再直接传 system_prompt=&quot;...&quot; 字符串，
# 而是通过 middleware 参数传入动态生成器。
graph = create_agent(
    model=model,
    tools=tools,
    middleware=[dataset_context_middleware], # 注入中间件
)
```

这段 src/agent.py 代码的职责是：组装一个能够**实时感知数据变化**、并**自主调用工具**的智能体对象（Graph）。

#### **1. 模块导入与环境准备**

```
from langchain.agents import create_agent
from langchain.agents.middleware import dynamic_prompt
```

+   **create_agent**: 这是 LangChain 1.1 统一的 Agent 构建入口。它取代了旧版繁琐的 AgentExecutor 定义方式，底层自动封装了图（Graph）结构、工具绑定（Tool Binding）和输出解析器。

+   **dynamic_prompt**: 这是一个**中间件装饰器**。它是本架构的核心亮点，用于解决“静态提示词僵化”问题，允许 Agent 在每一次思考前“动态刷新”自己的人设和上下文。

#### 2. 初始化模型 (The Brain)

```
model = ChatDeepSeek(
    model=&quot;deepseek-chat&quot;,
    temperature=0,  # [关键参数]
    api_key=os.getenv(&quot;DEEPSEEK_API_KEY&quot;),
    ...
)
```

+   model="deepseek-chat": 我们选用 DeepSeek V3.2 (Chat) 模型。它在指令遵循（Instruction Following）和代码生成方面表现卓越，非常适合这种需要精确写代码的场景。

+   **temperature=0**: **极度重要**。数据分析要求代码严谨、逻辑确定，不能有随机创造性（Hallucination）。将温度设为 0，让模型输出最稳定、最准确的代码。

#### 3. 定义动态提示词中间件 (The Dynamic Soul)

这是整个 Agent 最具“智慧”的部分。

```
@dynamic_prompt  # [装饰器] 注册为 Prompt 中间件
def dataset_context_middleware(request) -&gt; str:
    # 1. 实时获取数据快照
    data_context = get_data_info()
    
    # 2. 返回动态 Prompt
    return f&quot;&quot;&quot;...【当前数据集实时状态】\n{data_context}...&quot;&quot;&quot;
```

+   **设计痛点**：传统的 Agent 写法是将 System Prompt 写死在代码里的（例如 system_prompt="你有个数据表..."）。但用户上传新 CSV 后，变量已经变了，Agent 却还只记得旧表结构，导致写错代码。

+   **中间件机制**：

+   **拦截 (Intercept)**：每当用户发一句对话，create_agent 内部的运行时会在调用 LLM 之前，先运行这个 dataset_context_middleware 函数。

+   **注入 (Inject)**：函数内部调用 get_data_info()，直接去内存（Pandas DataFrame）里查最新的列名、行数。

+   **刷新 (Refresh)**：将最新的表格结构拼接到 Prompt 里。

+   **效果**：实现了**“上传即感知”**。用户上一秒上传文件，下一秒问 Agent，Agent 就能准确说出列名，完全不需要重启服务。

#### 4. 构建智能体 (The Assembly)

```
graph = create_agent(
    model=model,
    tools=tools,
    middleware=[dataset_context_middleware], # [关键] 注入中间件
)
```

+   **middl**eware=[...]: 这里展示了 LangChain 1.1 的插件式架构。我们将刚才定义的动态逻辑像“插件”一样插进去。Agent 启动后，这个中间件就会像守护进程一样，确保每次对话的 System Prompt 都是最新鲜的。

+   **tool**s=[python**_i**nter, fig_inter]: 赋予 Agent 两项核心能力——通用计算（Python）和绘图（Matplotlib）。

+   **graph**** 返回值****：这个对象本质上是一个编译好**的 Runnable Graph。它既可以直接被 agent.inv**oke() 调用，也能完美**兼容 LangGraph CLI 进行流式部署。

### 3.项目运行流程

+   下载源码并解压缩

+   安装后端依赖：

```
cd backend
pip install -r requirements.txt
```

+   开启后端

```
python -m src.server
```

![](img/87e692f8aa5829a7a56b403ee80c3233.png)

+   安装前端依赖

```
cd frontend
npm install
```

+   开启前端

```
npm run dev
```

![](img/8a62f83194f35ed86d8a33282d17f678.png)

```
Video(&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/d785514f32f7125bd859045b10d3ae3a.mp4&quot;, width=800, height=400)
```

Your browser does not support the video element.