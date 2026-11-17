
# LangChain 1.0入门实战

> 来源：`https://kq4b3vgg5b.feishu.cn/docx/UUPbdYORSoeyrdxHMYKc2LzGnDh`

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(秋季班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(秋季班)

---

## Part 5. LangChain Agent 部署与上线流程

### 1. LangChain Agent 开发必备工具套件回顾

在对`LangChain 1.0`有了一定的基础了解之后，对于开发者来说，还需要进一步了解和掌握`LangChain Agent`必备的开发者套件。分别是`LangChain Agent`运行监控框架`LangSmith`、底层`LangGraph`图结构可视化与调试框架`LangGraph Studio`和`LangGraph`服务部署工具`LangGraph Cli`。可以说这些开发工具套件，是真正推动`LangGraph`的企业级应用开发效率大幅提升的关键。同时监控、调试和部署工具，也是全新一代企业级`Agent`开发框架的必备工具，也是开发者必须要掌握的基础工具。

#### 1.1 LangGraph 运行监控框架：LangSmith

> `LangSmith`官网地址：`https://docs.smith.langchain.com/`

`LangSmith` 是一款用于构建、调试、可视化和评估 `LLM` 工作流的全生命周期开发平台。它聚焦的不是模型训练，而是我们在构建 `AI` 应用（尤其是多工具 `Agent`、`LangChain/Graph`）时的「可视化调试」、「性能评估」与「运维监控」。

![](img/2e6939de234bbbc8fc12d97825522147.png)

| 功能类别 | 描述 | 场景 |
| --- | --- | --- |
| 🧪 调试追踪（Trace Debugging） | 可视化展示每个 `LLM` 调用、工具调用、`Prompt`、输入输出 | `Agent` 调试、`Graph` 调用链分析 |
| 📊 评估（Evaluation） | 支持自动评估多个输入样本的回答质量，可自定义评分维度 | 批量测试 `LLM` 表现、`A/B` 对比 |
| 🧵 会话记录（Sessions / Runs） | 每次 `chain` 或 `agent` 的运行都会被记录为一个 `Run`，可溯源 | `Agent` 问题诊断、用户问题分析 |
| 🔧 Prompt 管理器（Prompt Registry） | 保存、版本控制、调用历史 `prompt` | 多版本 `prompt` 迭代测试 |
| 📈 流量监控（Telemetry） | 实时查看运行次数、错误率、响应时间等 | 在生产环境中监控 `Agent` 质量 |
| 📁 Dataset 管理 | 管理自定义测试集样本，支持自动化评估 | 微调前评估、数据对比实验 |
| 📜 LangGraph 可视化 | 对 `LangGraph` 中每个节点运行情况进行实时可视化展示 | `Graph` 执行追踪 |

#### 1.2 LangGraph 图结构可视化与调试框架：LangGraph Studio

> `LangGraph Studio`官网地址：`https://www.langgraph.dev/studio`

`LangGraph Studio` 是一个用于可视化构建、测试、分享和部署智能体流程图的图形化 `IDE` + 运行平台。

![](img/e7ee1f8ba353eec1ca13a03bf38abe05.png)

| 功能模块 | 说明 | 应用场景 |
| --- | --- | --- |
| 🧩 `Graph` 编辑器 | 以拖拽方式创建节点（工具、模型、`Router`）并连接 | 零代码构建 `LangGraph` |
| 🔍 节点配置器 | 每个节点可配置 `LLM`、工具、`Router` 逻辑、`Memory` | 灵活定制 `Agent` 控制流 |
| ▶️ 即时测试 | 输入 `prompt` 可在浏览器中运行整个图 | 实时测试执行结果 |
| 💾 云端保存 / 分享 | 将构建的 `Graph` 保存为公共 `URL` / 私人项目 | 团队协作，`Demo` 分享 |
| 📎 `Tool` 插件管理 | 可连接自定义工具（`MCP`）、`HTTP API`、`Python` 工具 | 插件式扩展 `Agent` 功能 |
| 🔁 `Router` 分支节点 | 创建条件分支，支持 `if/else` 路由 | 决策型智能体 |
| 📦 上传文档 / 多模态 | 可以上传文件（如 `PDF`）并嵌入进图中处理流程 | `RAG` 结构、`OCR`、图文问答等 |
| 🧠 `Prompt` 输入/预览 | 编辑 `prompt` 并观察其运行效果 | `Prompt` 工程调试 |
| 📤 一键部署 | 将 `Graph` 部署为可被 `Agent Chat UI` 使用的 `Assistant` | 快速集成到前端 |

#### 1.3 `LangGraph` 服务部署工具：`LangGraph Cli`

> `LangGraph Cli`官网地址：`https://www.langgraph.dev/`（需要代理环境）

`LangGraph CLI` 是用于本地启动、调试、测试和托管 `LangGraph` 智能体图的开发者命令行工具。

![](img/1eeb33df6a882796567ba4284f5281f9.png)

| 功能类别 | 命令示例 | 说明 |
| --- | --- | --- |
| ✅ 启动 `Graph` 服务 | `langgraph dev` | 启动 `Graph` 的开发服务器，供前端（如 `Agent Chat UI`）调用 |
| 🧪 测试 `Graph` 输入 | `langgraph run graph:graph --input &#x27;{&quot;input&quot;: &quot;你好&quot;}&#x27;` | 本地 `CLI` 输入测试，输出结果 |
| 🧭 管理项目结构 | `langgraph init` | 初始化一个标准 `Graph` 项目目录结构 |
| 📦 部署 `Graph`（未来） | `langgraph deploy`（预留） | 发布 `graph` 至 `LangGraph` 云端（已对接 `Studio`） |
| 🧱 显示 `Assistant` 列表 | `langgraph list` | 显示当前 `graph` 中有哪些 `assistant`（即 `entrypoint`） |
| 🔄 重载运行时 | 自动热重载 | 修改 `graph.py` 时，`dev` 模式自动重启生效 |

而一旦应用成功部署上线，`LangGraph Cli` 还会非常贴心的提供后端接口说明文档：

![](img/b033f6cbce5cfdc6de9c1f05567fced9.png)

而对于 `LangGraph` 构建的智能体，除了能够本地部署外，官方也提供了云托管服务，借助 `LangGraph Platform`，开发者可以将构建的智能体 `Graph` 部署到云端，并允许公开访问，同时支持支持长时间运行、文件上传、外部 `API` 调用、`Studio` 集成等功能。

#### 1.4 `LangGraph Agent` 前端可视化工具：`Agent Chat UI`

`Agent Chat UI` 是 `LangGraph/LangChain` 官方提供的多智能体前端对话面板，用于与后端 `Agent`（`Graph` 或 `Chain`）进行实时互动，支持上传文件、多工具协同、结构化输出、多轮对话、调试标注等功能。

> `Agent Chat UI`官网地址：`https://langchain-ai.github.io/langgraph/agents/ui/`

![](img/fa02880e51aeed5f17d6a652be6d54a3.png)

| 功能模块 | 描述 | 应用场景 |
| --- | --- | --- |
| 💬 多轮对话框 | 类似 `ChatGPT` 的输入区域，支持多轮上下文 | 用户提问，`Agent` 回复 |
| 🛠 工具调用轨迹显示 | 显示每个调用的工具、参数、结果（结构化） | `Agent` 推理透明化 |
| 📄 上传 `PDF` / 图片 | 支持上传文档、图片、嵌入多模态输入 | `RAG`、`OCR`、图像问答 |
| 📁 文件面板 | 可查看上传历史文件、删除、重新引用 | 管理文档输入 |
| 🧭 `Assistant` 切换 | 支持切换不同 `Assistant`（`Graph entry`） | 一键切换模型能力（如 `math` / `weather`） |
| 🧩 插件支持 | 与 `MCP` 工具、`LangGraph` 图打通 | 工具式 `Agent` 调用 |
| 🔍 调试视图 | 显示每轮 `Agent` 的思维过程和中间状态 | `Prompt` 调试、模型行为分析 |
| 🌐 云部署支持 | 支持接入远端 `Graph API`（如 `dev` 服务器） | 前后端分离部署 |
| 🧪 与 `LangSmith` 对接（可选） | 若后端启用 `tracing`，可同步显示运行 `trace` | 调试闭环 |

### 2. 借助 `LangGraph Cli` 创建完整智能体项目

#### 2.1 `LangGraph` 智能体项目说明

`LangSmith` 与 `LangGraph Studio` 都是 `LangChain AI` 生态中非常核心的工具，前者是用于跟踪和分析大模型的使用情况，而`langGraph Studio`则是对于`LangChian Agent`来说，则是比`LangSmith`更加方便和高效的可视化调试工具平台。

大家可以思考一个问题：基于`LangGraph`框架可以开发出各种复杂的应用、`Agent`、`Workflow`等，那么这些应用、`Agent`、`Workflow`等在生产中如何部署和运行呢？ 解决方案就是把它们部署成一个`Server`。 而对如何方便高效的把`LangGraph`的`Graph`部署成一个`Server`，`LangGraph`官方提供了`LangGraph Platform`，其完整架构如下所示：

![](img/578398ae105e98a688d8d996e63980c7.png)

+   `LangGraph Studio`：桌面版应用（目前仅支持`Mac`）和本地运行（适用于所有操作系统）；

+   `LangServer`：最终构建出来的服务，提供`Assistant API`接口；

+   `Python/JS SDK`：通过接口可以直接和 `LangServer` 提供的各个`API`接口连接；

+   `Remote Graph`：类似于之前`LangServe`的用法，可以直接用 `Graph` 的接口去调用，这样拿到的`Graph`就是一个 `Runable`对象，就可以去调用它的`invoke`、`batch` 等。

`LangGraph Studio` 是专为 `LangGraph` 图式代理打造的本地/云端 `IDE`，具备可视化节点和状态及实时调试功能。`LangGraph Studio` 在本地可视化运行时会自动把调用过程上传到 `LangSmith`；而在 `LangSmith` 网页端查看任何 `Trace` 时，又能一键`Run in Studio`回放整条执行链，所以它是通过统一 `Trace SDK` 与 `LangSmith` 紧密集成。而`LangGraph CLI`则是构建这个项目的关键

接下来我们就来详细的讲解下如何使用`LangGraph CLI`来创建一个完整的`LangGraph Agent`项目，并在此过程中使用`LangGraph Chat Agent UI`进行前端对话，以及使用`LangGraph Studio`进行架构实时演示，并使用`LangSmith`进行运行效果监督。

首先需要说明的是，如果想要把一个定义的`Graph`添加到`LangGraph Studio`中，需要严格按照官方要求的项目结构进行构建，需要执行的步骤依次是：

#### 2.2 创建完整 `LangGraph` 智能体项目流程

+   **Step 1. 创建一个** `LangChain Agent` 项目主文件夹

我们这里创建一个`LangChain Chatbot`文件夹，如下图所示：

![](img/8d3d4bda6f8351082f40267f68c710e2.png)

+   Step 2. 创建 `requirements.txt` 文件

在`LangChain Chatbot`文件夹中，新建一个`requirements.txt`文件，里面需要填写在运行该项目时需要安装的依赖项，如下所示：

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

![](img/a8d2e8e9cd48b58cbd0b752fcf27d89b.png)

+   Step 3. 注册`LangSmith`（可选）

对于企业级的`Agent`项目，为了更好的监控智能体实时运行情况，我们可以考虑借助`LangSmith`进行追踪（会将智能体运行情况实时上传到`LangGraph`官网并进行展示）。

要开始使用 `LangSmith`，我们需要创建一个帐户。可以在这里注册一个免费帐户进入`LangSmith`登录页面：`https://smith.langchain.com/`，支持使用 `Google`、`GitHub`、`Discord` 和电子邮件登录。

![](img/7d829af79e8152e885a07381efab5160.png)

注册并等登录后，可以直接查看到仪表板：

![](img/4ceb78da7c7744ccc66ffa4a72441006.png)

在构建程序跟踪前，首先需要创建一个 `API` 密钥，该密钥将允许我们的项目开始向 `Langsmith` 发送跟踪数据。创建完密钥后，在后续配置环境变量环节设置开启追踪、并输入密钥即可接入`LangSmith`。

+   **Step 4. 创建** `.env` 配置文件

在`LangChain Chatbot`文件夹中，新建一个`.env`文件，将敏感信息（如`API`密钥）放在环境变量中而不是硬编码。如下所示：

![](img/b7fff27edae3d6e6753e136be6861311.png)

这里需要注意的是，如果不设置`LangSmith`，则无需设置中间三个环境变量，而具体工具也可以根据实际需求进行设置。

+   **Step 5. 创建** `agent.py` 核心文件

在`LangChain Chatbot`文件夹中，新建一个`agent.py`文件，在该文件中编写构建图的具体运行逻辑，如状态、节点、变、图的编译等。此外，在使用`LangGraph CLI`创建智能体项目时，会自动设置记忆相关内容，并进行持久化记忆存储，无需手动设置。因此此时智能体代码如下所示：
