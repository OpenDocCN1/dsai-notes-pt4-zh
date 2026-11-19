
# Dify 的核心组件（常用节点 + RAG）

> 来源：[https://yunyinghui.feishu.cn/docx/LvmxdmUQUou8daxX9nmc5mPTnke](https://yunyinghui.feishu.cn/docx/LvmxdmUQUou8daxX9nmc5mPTnke)

## 核心组件

Dify 的核心设计思路，其实就是把复杂的 AI 开发拆成几个清晰的功能模块，再通过超直观的可视化界面，让用户能真正聚焦在自己的业务上，而不是反复折腾底层技术。下面这五大组件，基本就构成了 Dify 的能力骨架：

### 应用类型

Chatflow 和 Workflow，分别搞定对话类和自动化类的任务；

### 工作流节点

`Start`、`LLM`、`Tools`、`Answer` 这些，像搭积木一样拼出完整逻辑；

### 变量系统

负责在不同节点之间传递数据，让信息流动起来；

### RAG 管道

能从外部数据源实时检索信息，大幅提升模型输出的准确性；

### 数据管理

背后有知识库和向量数据库支撑，实现高效的数据存储和检索。

这几个组件各司其职又彼此配合，一起撑起了 Dify 的整个功能体系。接下来，我们就一个一个来看它们到底怎么用、能做什么。

## 应用类型：Chatflow 和 Workflow

![](img/1c75549d205175280275324eb9857eb8.png)

Dify 目前主打两种应用类型，分别应对不同使用场景：

### Chatflow：专为对话类应用设计

如果你要做聊天机器人、智能客服或者语义搜索这类强交互应用，用 Chatflow 就对了。它的特点包括：

+ `Memory` 功能记住上下文，聊天不会断片儿；
+ `Answer` 节点支持流式输出（一个字一个字蹦那种），回复还能带注释和来源；
+ 直接搭配 RAG，从你自己的文档、网页里提取信息来回答。

举个例子：比如你做一个电商客服机器人，用户问“退货怎么操作？”或者“这款电脑重量多少？”，它可以直接引用你上传的 FAQ 或产品手册来回复，超省心。

配置起来也不难：

1. 在 Dify 里点 Create Application 选 Chatflow；
1. 选一个模型（比如 GPT-4o 或者本地部署的 Llama）；
1. 上传知识库文档，打开 RAG 开关；
1. 测试对话、调一调 Prompt，回答就更准了。

### Workflow：主打自动化和多步骤任务

Workflow 更适合有明确流程的任务，比如自动生成报告、批量处理数据、发邮件这些。它的亮点是：

+ 可以用 If/Else、循环等节点搭出复杂逻辑；
+ 支持循环处理列表数据，适合批任务；
+ 通过 `Tools` 节点调用 API，比如 Serper 搜新闻、Yahoo Finance 拿股价。

📌 举个例子：搭一个市场分析 Workflow，它可以自动搜索行业动态、提取关键数据，再帮你写成一篇带图表的周报，完全不用人手介入。

> 配置步骤：
>
> 1. 创建 Workflow 应用，选择空白模板或 `DeepResearch` 模板。
> 1. 添加节点（如 `Start`、`Tools`、`LLM`、`Answer`），连接逻辑。
> 1. 配置变量和外部工具 API 密钥。
> 1. 预览工作流，检查输出。

### Chatflow vs Workflow

`Chatflow`：强调对话交互，适合实时用户沟通。

`Workflow`：强调流程自动化，适合后台任务处理。

结合使用：`Chatflow` 可触发 `Workflow`，完成复杂任务后返回对话结果。

## 工作流节点：Dify 的逻辑构建块

工作流节点是 Dify 的核心功能之一，通过可视化拖放方式，用户可以像搭积木一样构建 AI 应用的逻辑。以下是常见的节点类型及其功能：

### `Start` 节点

作用：定义工作流的输入参数，如文本、数字或文件。

配置：设置输入字段（如 `research_topic` 为字符串，`max_loop` 为数字）。

输出：将用户输入传递到下游节点。

示例：在 Deep Research 工作流中，`Start` 节点捕获研究主题（如“AI 在医疗领域的应用”）。

### `LLM` 节点

![](img/31696aa2499d199a88894f0ef7056349.png)

作用：调用大型语言模型处理输入，生成文本、JSON 或其他格式输出。

配置：

选择模型（如 GPT-4o、Claude、Ollama 的 Mistral）。

设置提示（Prompt），如：

```
Based on {{input}}, generate a summary in 100 words.
```

输出：生成结果，如文本或结构化数据。

示例：分析搜索结果，生成下一步查询关键词。

### `Tools` 节点

![](img/5840827f6e72147ef36e7f4cf908a231.png)

作用：调用外部工具或 API，获取实时数据。

内置工具：`Exa Answer`（搜索）、`Serper`（搜索引擎）、`Yahoo Finance`（股票数据）、`JinaReader`（网页抓取）。

自定义工具：通过 HTTP 节点调用第三方 API。

配置：输入 API 密钥和查询参数。

示例：使用 `Serper` 搜索“AI 医疗应用”，返回标题、URL 和摘要。

### `Knowledge Retrieval` 节点

作用：从知识库检索相关信息，支持 RAG 管道。

配置：选择知识库，设置检索参数（如 `Top-K`）。

输出：匹配的文档片段。

示例：从医疗文献库检索 AI 诊断相关内容。

### `If/Else` 节点

![](img/41874fafa02d68ba9b832102bf11b1b2.png)

作用：根据条件分支控制流程。

配置：设置条件（如 `{{llm.shouldContinue}} == true`）。

输出：路由到 `True` 或 `False` 分支。

示例：决定是否继续迭代搜索。

### `Iteration` 节点

![](img/f283bdf613b3518017c53e2fd3a6723e.png)

作用：循环处理列表数据，适合批量任务。

配置：输入数组（如搜索关键词列表），设置最大循环次数。

输出：每次循环的结果集合。

示例：批量搜索多个关键词。

### `Answer` 节点

作用：生成最终输出，支持流式更新。

配置：使用 Jinja2 模板格式化输出，如：

```
# Report on {{start.research_topic}}
{% for result in iteration.results %}
- {{result.snippet}}
{% endfor %}

```

示例：生成带引用的研究报告。

### `Code` 节点

![](img/450096f1706196fb5b3525b965d01619.png)

作用：执行自定义 Python 或 Node.js 代码。

配置：编写代码处理输入数据。

示例：清洗 JSON 数据，提取特定字段。

### `Parameter Extractor` 节点

![](img/216a9d12b5bfb51ad9681b775429bbb8.png)

作用：从文本提取结构化数据。

配置：定义提取规则（如正则表达式或 LLM 提示）。

示例：从用户输入提取订单号。

### `Doc Extractor` 节点

![](img/84d8f2c2f5293cbbd75d52111e8a9e6a.png)

作用：从文档（如 PDF、TXT）提取文本。

配置：上传文件，设置提取范围。

示例：解析财务报告。

### `List Operation` 节点

作用：处理数组数据，如过滤、排序。

配置：设置操作规则（如 `filter: score > 0.8`）。

示例：筛选高相关度的搜索结果。

### 节点使用技巧

连接逻辑：确保节点输入输出类型匹配（如字符串到字符串）。

调试：使用 `Workflow Process` 面板检查节点运行状态。

复用：保存常用节点配置为模板。

## 变量：数据流动的桥梁

变量是 Dify 工作流中数据传递的核心机制，连接各节点的输入和输出。Dify 支持以下变量类型：

输入变量：来自用户输入或外部数据（如 `start.research_topic`）。

输出变量：节点处理结果（如 `llm.output`）。

会话变量：存储对话历史或状态，适合 `Chatflow`。

聚合变量：通过 `Variable Aggregator` 节点汇总数据。

### 变量的工作原理

定义：在 `Start` 节点或节点配置中定义变量（如 `research_topic: string`）。

引用：使用 Jinja2 语法引用变量，如 `{{start.research_topic}}`。

传递：变量从上游节点流向下游节点。

管理：在工作流编辑器中查看和编辑变量。

### 示例：变量在工作流中的应用

> 假设构建一个 Deep Research 工作流：
>
> `Start` 节点定义 `research_topic`（如“AI in healthcare”）。
> `Iteration` 节点引用 `{{start.research_topic}}`，分解为关键词列表。
> `Tools` 节点使用 `{{iteration.search_keywords}}` 调用 `Serper`。
> `LLM` 节点分析 `{{tools.search_results}}`，生成 `nextSearchTopic`。
> `Answer` 节点格式化 `{{iteration.iteration_results}}` 为报告。

### 变量管理技巧

+ 命名规范：使用清晰的变量名（如 `search_results` 而非 `data`）。
+ 类型检查：确保变量类型一致（如数组到数组）。
+ 调试：在 `Workflow Process` 面板查看变量值。

## RAG 管道：从数据到智能回答

![](img/e9c6de501964dccc48814012a9c2ff7b.png)

检索增强生成（RAG）是 Dify 的核心功能，通过从外部数据源检索信息，显著提升 LLM 的回答准确性和上下文相关性。RAG 管道包括三个阶段：

### 数据提取

支持格式：PDF、TXT、PPT、Markdown、网页等。

方法：上传文件或通过 `JinaReader` 抓取网页。

示例：上传医疗白皮书，提取文本内容。

### 数据转换

分段：将长文档拆分为小片段（默认 512 字符）。

向量化：使用嵌入模型（如 `sentence-transformers`）将文本转为向量。

存储：保存到向量数据库（`Weaviate` 或 `pgvector`）。

示例：将白皮书分段并索引到 `Weaviate`。

### 检索与生成

检索：根据用户查询，检索 `Top-K` 相关片段。

生成：`LLM` 结合检索结果生成回答。

示例：用户查询“AI 诊断技术”，`RAG` 检索相关段落，`LLM` 生成详细回答。

### 配置 RAG 管道

在 Dify 仪表板创建知识库，上传文档或网页。

配置分段和嵌入模型（默认使用 `sentence-transformers`）。

在工作流中添加 `Knowledge Retrieval` 节点，连接到 `LLM`。

测试检索效果，调整 `Top-K` 或分段大小。

### RAG 的优势

准确性：减少 `LLM` 的幻觉（hallucination）。

灵活性：支持动态更新知识库。

效率：无需微调 `LLM` 即可利用外部数据。

## 数据管理：知识库与向量数据库

Dify 的数据管理模块为 RAG 和知识库提供支持，确保高效的数据存储和检索。

### 知识库

功能：存储文档、网页或文本数据，自动索引为向量。

操作：上传文件、抓取网页、编辑分段。

清洗：支持去重、分段和格式化。

示例：创建企业 FAQ 知识库，包含产品手册和政策文档。

### 向量数据库

支持：`Weaviate`（默认）、`pgvector`。

作用：存储文本向量，支持快速相似性检索。

配置：在 `docker-compose.middleware.yml` 中设置 `Weaviate`：

```
weaviate:
  image: semitechnologies/weaviate:1.26.1
  ports:
    - "8080:8080"
```

示例：检索与“AI 医疗”最相似的文档片段。

### 数据管理技巧

分段优化：调整分段大小（过小降低上下文，过大影响检索）。

去重：移除重复内容，提升检索效率。

监控：使用 Dify 日志检查知识库索引状态。

## 实际案例

以下是 Dify 核心组件的实际应用案例：

### 教育智能问答系统

用 `Chatflow` + RAG 管道，轻松把教材、讲义变成会答题的助手。上传 PDF 教材后，`Knowledge Retrieval` 节点自动提取内容，学生提问就能精准获得解析和答案——不用翻书，秒问秒答。

### 自动化市场分析报告

借助 `Workflow`，搭配 `Serper` `Tools` 节点和 `LLM` 节点，自动爬取行业关键词的最新动态、生成洞察分析。`Iteration` 节点还能批量处理多个关键词，原来几小时的分析活，现在几分钟就跑完了。

### 营销文案批量生成器

通过 `Answer` 节点+Jinja2 模板快速生成结构规整、风格统一的营销推文。结合变量管理，产品名称、卖点等信息一次性配置，自动同步到所有文案中，告别重复粘贴、人工出错。

## 常见问题与解答

### Q：`Chatflow` 和 `Workflow` 可以在同一个应用里一起用吗？

A：当然可以！你完全可以在 `Chatflow` 中调用 `Workflow`。比如在对话过程中触发一个复杂的自动化任务（如数据查询或报告生成），等 `Workflow` 执行完毕后再把结果返回对话中，用户感觉就像一次自然交互。

### Q：怎么知道该用哪种节点？

A：可以根据任务目标来选：

+ 如果是用于对话回复，常用 `Answer` 节点（支持流式输出和引用展示）；
+ 如果需要批量处理多条数据，就用 `Iteration` 节点；
+ 条件判断类任务可选 `If/Else` 节点，灵活分支。

### Q：使用 `RAG` 需要自己搭建向量数据库吗？

A：Dify 提供了内置的向量数据库和默认配置，开箱即用，快速上手没问题。你主要需要创建知识库、上传自己的文档（如 PDF/Word），后续检索增强流程 Dify 会自动处理。

### Q：变量引用报错该怎么调试？

A：可以先：

1. 确认变量名拼写和所在节点是否正确；
1. 检查变量类型是否匹配（比如文本、数字等）；
1. 推荐使用 `Workflow Process` 面板 实时调试，可视化查看数据流动，快速定位问题。
