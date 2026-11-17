
| `search_depth` | `str` | 搜索深度，可选 `"basic"` 或 `"advanced"`；高级模式会返回更相关的内容片段和来源 | `"basic"` |

| `topic` | `str` | 搜索主题（分类），支持 `"general"` 和 `"news"` | `"general"` |

| `days` | `int` | 针对 `news` 主题，查询结果包含从当前日期往前推几天内的内容 | 7 |

| `time_range` | `str` | 时间范围，支持 `"day"`, `"week"`, `"month"`, `"year"` 或简写 `"d"`, `"w"`, `"m"`, `"y"` | — |

| `max_results` | `int` | 返回结果最大数量（0~20） | 5 |

| `chunks_per_source` | `int` | 每个来源返回多少内容片段（仅 `advanced` 有效） | 3 |

| `include_images` | `bool` | 是否返回与查询相关的图片列表 | `False` |

| `include_image_descriptions` | `bool` | 是否返回图片及其自动生成的描述 | `False` |

| `include_answer` | `bool`或`str` | 返回基于搜索结果生成的答案，`"basic"`/`True`为简要答案，`"advanced"`为详细答案 | `False` |

| `include_raw_content` | `bool`或`str` | 返回解析的原始HTML内容，`"markdown"`或`True`为Markdown格式，`"text"`为纯文本 | `False` |

| `include_domains` | `list[str]` | 指定要包含的域名 | `[]` |

| `exclude_domains` | `list[str]` | 指定要排除的域名 | `[]` |

| `country` | `str` | 优先返回来自指定国家的内容（仅 `general` 主题可用） | — |

| `timeout` | `int` | API请求超时时间（秒） | 60 |

| `include_favicon` | `bool` | 返回每个结果对应的 `favicon` URL | `False` |

### 返回格式

一次搜索请求将返回一个字典对象，包含以下字段：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `query` | `str` | 您的原始查询内容 |
| `results` | `list[Result]` | 排序后的搜索结果列表 |
| `response_time` | `float` | 搜索用时（秒） |
| `answer (可选)` | `str` | 基于搜索结果生成的答案（需开启 `include_answer`） |
| `images (可选)` | `list[str]`或`list[ImageResult]` | 图片URL或包含描述的对象列表（需开启图片参数） |

**Result对象字段：**

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `title` | `str` | 搜索结果标题 |
| `url` | `str` | 搜索结果URL |
| `content` | `str` | 最相关的内容摘要 |
| `score` | `float` | 相关性得分 |
| `raw_content (可选)` | `str` | 原始HTML内容（需开启 `include_raw_content`） |
| `published_date (可选)` | `str` | 来源发布时间（news 主题下可用） |
| `favicon (可选)` | `str` | `favicon` URL |

ImageResult对象字段：

当启用 `include_image_descriptions` 时，每个图片将包含以下信息：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `url` | `str` | 图片URL |
| `description` | `str` | 自动生成的图片描述 |

### 3.2 Tavily Extract 功能详解

除了搜索功能，Tavily SDK 还提供了网页内容提取（Extract）接口，支持从指定 URL 抓取清洗后的正文、图片和元数据，特别适用于批量获取网页内容、构建知识库或进行后续分析。

可以通过 `extract()` 方法向 Tavily 提交单个或批量 URL，自动提取结构化内容。

```
from tavily import TavilyClient

# 初始化客户端
tavily_client = TavilyClient(api_key="tvly-YOUR_API_KEY")

# 定义需要提取内容的URL列表
urls = [
    "https://en.wikipedia.org/wiki/Artificial_intelligence",
    "https://en.wikipedia.org/wiki/Machine_learning",
    "https://en.wikipedia.org/wiki/Data_science",
]

# 执行提取请求
response = tavily_client.extract(
    urls=urls,
    include_images=True
)

# 输出提取结果
print(response)
```

![](img/1390777f645126c6371f5c9a74558db2.png)

### 3.3 Tavily Crawl 功能介绍

Tavily Crawl 是专为智能体开发设计的网页爬取功能，支持从一个基础 URL 自动递归抓取网页内容，构建丰富的上下文数据。该功能目前处于**公开测试（Open Beta）**阶段。

通过 `crawl()` 方法即可启动一个爬取任务，并自动获取抓取结果。

```
from tavily import TavilyClient

# 初始化客户端
tavily_client = TavilyClient(api_key="tvly-YOUR_API_KEY")

# 定义起始URL
url = "https://docs.tavily.com"

# 执行爬取
response = tavily_client.crawl(
    url,
    instructions="请帮我整理Python SDK相关信息"
)

# 输出结果
print(response)
```

![](img/fa781c2c8e364d82860e5ef8f3daafa6.png)

### 4. 谷歌可编程搜索引擎调用方法

![](img/caa4070abe26baf2b7782b7f19382aba.png)

![](img/c1f4d3411f6fcceeb6f6c6a2a1283562.png)

![](img/e35373cf327bea9202ce71a37c60c3d2.png)

## 三、从零到一构建DeepResearch应用

![](img/c93ec380aeed17c3c1748034c2a23443.png)

> 详见Jupyter文件

## 四、open_deep_research项目部署与调用

`open_deep_researcher` 是 LangChain AI 团队开源的一个深度研究代理项目，其功能和目标是复现并拓展上述 Deep Research 能力，让开发者无需付费即可使用AI自动化完成深入的信息调研和报告撰写。用户只需提供一个研究主题或问题，代理就会自动生成搜索查询，抓取和总结网页内容，反思是否有知识缺口并继续检索，最终整理输出一份结构清晰的 Markdown 报告。

其开源地址如下所示：https://github.com/langchain-ai/open_deep_research

![](img/80c605e3af368849cf45e6c9e5d6d912.png)

与传统搜索/检索相比，深度研究的核心区别在于：它不仅返回信息，还会自主完成信息筛选和整合。传统搜索需要用户自己点开筛选内容，普通的AI问答则容易产生未经验证的答案；而深度研究代理能够多轮迭代，在发现知识空白时自动调整方向，持续检索直到形成全面理解。最终输出的报告通常包含分层次结构的内容、事实支撑的分析以及明确引用来源，可视作一个专业研究员整理的成果而非简单搜索结果。

这个项目提供了两种不同的实现架构：基于工作流的有向图（Graph）实现和多智能体（Multi-agent）实现。这两种实现各有侧重，分别对应不同的应用场景和需求。

Graph 工作流模式采用规划—执行的顺序流程，将深度研究任务划分为固定阶段依次完成：首先使用“规划”大模型分析主题，生成报告的大纲和计划；然后经过人工反馈确认计划；接下来对每个章节依次进行检索和写作，每一章节都包含搜索—总结—反思的循环，确保信息充分；最后整合各章节内容并生成完整报告

![](img/4582c3c39c57c0918b5ab7149872e6af.png)

LangGraph 的引入让 `open_deep_researcher` 易于部署和集成。例如项目的 `.env` 配置会被 LangGraph 自动加载，使模型API Key、搜索API Key 等环境参数统一管理。LangGraph 平台也支持将开发好的 Deep Research agent 一键部署到云端运行（如果需要对外提供服务）。总之，`open_deep_researcher` 借助 LangGraph 实现了复杂代理流程的标准化和模块化，开发者在此之上可以更高效地定制和扩展深度研究Agent的能力。

部署 `open_deep_researcher` 十分简便，可以通过源码或 PyPI 包两种方式安装。我们这里要使用源码安装。大家可以扫描下方二维码进入百度网盘领取源码。

![](img/9a238b5008888b70be527cb70fc74936.png)

![](img/07291a2777c8e6bc7a36b6506833aacf.png)

下载到项目源码后，进入项目目录，编辑 `.env` 文件，这里需要准备 API 密钥，核心是两个配置：LLM 模型接口 和 搜索工具接口。这里我们使用的大模型是DeepSeek，搜索工具是Tavily。其中Tavily 的 Web 搜索 API 会作为信息检索工具。注册地址： https://www.tavily.com/ 。

同时，需要在`.env`文件中配置LangSmith的配置信息，用于追踪和分析代理的执行过程。完整的配置参数如下：

![](img/32138d89ee11cec569fe82ce7ecd6dbe.png)

在Windows/Linux系统中，安装`open_deep_researcher`项目依赖，可以使用常规的`pip` 命令进行安装。首先创建一个虚拟环境，并激活。然后安装 `open_deep_researcher` 项目依赖：

```
pip install -e .
```

接下来需要安装 LangGraph 的命令行工具 `langgraph-cli`，用于管理 LangGraph 的运行和部署。

```
pip install -U "langgraph-cli[inmem]"
```

然后，在项目源码中需要修改一个日期文件，否则在Windows系统中会报错。(我们提供的代码已经修改好了，大家可以直接使用)

```
    def get_today_str() -> str:
        """Get current date in a human-readable format."""
        return datetime.datetime.now().strftime("%a %b %-d, %Y")

    ->

    def get_today_str() -> str:
        """Get current date in a human-readable format."""
        # Use %d instead of %-d for Windows compatibility
        date_str = datetime.datetime.now().strftime("%a %b %d, %Y")
        # Remove leading zero from day if present (manual formatting for cross-platform compatibility)
        import re
        return re.sub(r'\b0(\d)', r'\1', date_str)
```

接下来，便可以启动 LangGraph 开发服务器，执行如下命令：

```
langgraph dev
```

![](img/79e0502e0212d14dff8741fd45386bc1.png)

当执行`langgraph dev` 后，会在本地开启一个 LangGraph Server（默认监听端口 2024）。命令行中会显示 API 接口地址和 Studio 界面URL，例如：

```
API: http://127.0.0.1:2024
Studio UI: https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024
```

用浏览器打开上述 Studio UI 链接，即可进入 LangGraph 提供的深度研究助手界面。在该界面上，你可以像使用 ChatGPT 一样与 deep researcher 交互：输入研究主题或问题，然后提交，观察代理自动检索和生成报告的全过程。界面还提供配置面板，可实时查看和调整部分参数（对应 `.env` 配置），以及一个 JSON 输入框用于反馈或控制流程（例如输入 `true` 确认计划，或输入字符串对提纲提供修改意见）

这里需要关注的是`langgraph.json` 配置文件。当我们执行 `langgraph dev` 时，会启动所有三个图：

1. `open_deep_research` - 基于 `./src/open_deep_research/graph.py:graph`
2. `open_deep_research_multi_agent` - 基于 `./src/open_deep_research/multi_agent.py:graph`
3. `odr_workflow_v2` - 基于 `./src/open_deep_research/workflow/workflow.py:workflow`

### graph图

这个图是父-子图结构，父图（外层）控制整体报告生成流程，子图（内层）负责具体的任务，即处理单个章节的深度研究。具体如下：

| 层次 | 主要节点 | 说明 |
| --- | --- | --- |
| 外层 | `generate_report_plan` | 生成初始大纲（Sections） |
|  | `human_feedback` | 人类审核或提出修改 |
|  | `build_section_with_web_research` | 子图：对需要检索的 section 逐一并行写作 |
|  | `gather_completed_sections` | 汇总已完成的研究类 section 作为上下文 |
|  | `write_final_sections` | 写结论、摘要等无需检索的 section |
|  | `compile_final_report` | 按最初规划顺序拼接全部 section |
| 子图 | `generate_queries` | 为单个 section 产出检索词 |
|  | `search_web` | 调用外部搜索 API 抓取资料 |
|  | `write_section` | 结合资料撰写并自评，失败则循环检索 |

主要适用的场景是自动化生成一份结构化、可信且可控的深度调研／分析报告。将“规划—检索—写作—自评—循环—总结”拆成原子节点，显式控制顺序与分支。非常适合一些需要深度调研的场景。比如市场、技术、政策的分析，需要多轮网络检索的报告等。

### multi_agent 图

这个架构的特点是一个双层智能体系统: 督导者（`Supervisor`） + 多个研究者（`Researcher`）。角色明确分工非常明确，由督导负责规划统筹，研究者负责具体执行。其中：

+ Supervisor Workflow 控制全局：

+ `supervisor` - 由 LLM 判断是否调用工具。

+ `supervisor_tools` - 真正执行工具调用并根据结果：
