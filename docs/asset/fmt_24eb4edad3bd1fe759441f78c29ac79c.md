
能够看出，此时`Agent`能够顺利完成多工具的多次调用，并最终完成用户需求。需要注意的是，此时`react agent`的执行逻辑是先发起了一次并联调用工具的请求，然后第二次再发起调用`write`函数的调用请求，因此总共是7条消息。

## 4. `LangGraph` `React` `Agent`接入内置工具流程

当然对于`LangGraph`智能体来说，除了能够灵活接如自定义工具，还能够接入`LangChain`丰富的内置工具，快速完成智能体开发。

在`LangChain`框架中，`工具（`Tools`）`是实现语言模型与外部世界交互的关键机制。`LangChain` 提供了大量内置与可扩展的工具接口，使得智能体（`Agent`）能够执行函数调用、访问 `API`、查询搜索引擎、调用数据库等任务，从而超越纯语言生成的能力，真正实现“能行动的智能体”。`LangChain` 官方文档将这些工具按照其用途进行了模块化划分，涵盖了以下主要类别：

| 功能类别 | 工具名称 | 简要说明 |

| --- | --- | --- |

| 🔎 搜索工具 | `TavilySearchResults` | 快速搜索实时网络信息 |

|  | `SerpAPIWrapper` | 基于 `SerpAPI` 的搜索结果工具 |

|  | `GoogleSearchAPIWrapper` | 调用 Google 可编程搜索引擎 |

| 🧠 计算工具 | `PythonREPLTool` | 执行 `Python` 表达式并返回结果 |

|  | `LLMMathTool` | 结合 `LLM` 和数学推理能力 |

|  | `WolframAlphaQueryRun` | 基于 Wolfram Alpha 的计算引擎 |

| 🗂 数据工具 | `SQLDatabaseToolkit` | 构建 `SQL` 数据库查询工具集 |

|  | `PandasDataframeTool` | 用于在 `Agent` 中操作表格数据 |

| 🌐 网络/`API` | `RequestsGetTool` / `RequestsPostTool` | 执行 `HTTP` 请求 |

|  | `BrowserTool` / `PlaywrightBrowserToolkit` | 自动化网页浏览与抓取 |

| 💾 文件处理 | `ReadFileTool` | 读取本地文件内容 |

|  | `WriteFileTool` | 写入文本到指定文件中 |

| 📚 检索工具 | `FAISSRetriever` | 基于向量的文档检索工具 |

|  | `ChromaRetriever` | 使用 `ChromaDB` 的检索器 |

|  | `ContextualCompressionRetriever` | 上下文压缩检索器，适合长文档 |

| 🧠 `LLM` 工具 | `ChatOpenAI` / `OpenAIFunctionsTool` | 使用 `OpenAI` 模型作为工具调用 |

|  | `ChatAnthropic` | Anthropic Claude 模型封装工具 |

| 🔧 自定义工具 | `@tool` 装饰器 | 任意函数可封装为 `Agent` 可调用工具 |

|  | `Tool` 类继承 | 自定义更复杂逻辑的工具实现 |

> `LangChain`工具集：`https://python.langchain.com/docs/integrations/tools/`

![](img/a55606f78cdcbaa0e6b605f711b294a0.png)

### 创建带搜索功能的`Agent`

这里我们尝试借助`LangChain`内置的`Tavily`搜索引擎工具，搭建能够进行网络搜索的智能体。首先需要在`tavily`官网进行注册并获取`API-KEY`：`https://www.tavily.com/`

![](img/974c86607c574efc20d81131cd8fb619.png)

并继续将`tavily`的`API-KEY`写到`.env`文件中：

![](img/a7e27506cce4e5851cd49087fe1fda3d.png)

然后安装对应的库:

```
!pip install -U langchain-tavily
```
