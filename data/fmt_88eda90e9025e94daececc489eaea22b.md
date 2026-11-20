
# 代码关键点解析

## import requests 和 traceback

引入了执行 HTTP 请求和打印详细错误信息的库。

## SEARXNG_BASE_URL、MAX_SEARCH_RESULTS

清晰地配置了外部服务的地址和结果数量限制。

## perform_searxng_search(query) 函数

### 核心功能

封装了调用 `SearXNG` API 的所有逻辑。

### 参数化请求

使用 `params` 字典构造查询参数，请求 `format=json`。

### 请求头

设置了 `User-Agent`。

### 网络请求

使用 `requests.get()` 发送请求，并设置了合理的 `timeout`。

### HTTP 错误检查

`response.raise_for_status()` 确保处理非 2xx 的响应。

### JSON 解析

`response.json()` 将响应体解析为 Python 字典。

### 结果提取与格式化

从 `data['results']` 中提取所需字段（`title`、`url`、`content`），并组装成一个对 LLM 友好的、简洁的字符串。移除了摘要中的换行符。

### 健壮的错误处理

使用 `try...except` 块捕获各种可能的异常（超时、连接错误、`HTTP` 错误、`JSON` 解析错误、其他未知错误），并在出错时返回包含错误信息的 `JSON` 字符串，而不是让程序崩溃。这使得模型能够获知搜索失败的原因。

## tools 定义

`description` 字段写得非常详细，指导模型何时应该调用此工具（需要实时信息、模型知识库外信息时），并给出了示例场景。

`parameters` 明确定义了 `query` 参数是必需的字符串。

## 交互式主循环（`while True`）

使用 `input()` 获取用户提问。

包含 `'quit'` 退出逻辑和空输入跳过。

### 消息历史管理

在循环开始时重置 `messages` 列表。这意味着每次交互都是相对独立的，侧重于单轮的工具调用演示。如需多轮对话记忆，需调整此部分。

## 两阶段 API 调用

### 第一次调用

发送用户消息和 `tools` 定义，`tool_choice="auto"`。

### 判断 `tool_calls`

检查响应中是否包含 `tool_calls`。

### 执行工具

如果需要调用，解析参数（增加了解析错误的 `try...except`），调用 `perform_searxng_search`。

### 添加工具结果

必须将包含 `tool_calls` 的助手消息和 `role: tool` 的结果消息都添加到 `messages` 历史中。

### 第二次调用

如果执行了工具，将更新后的 `messages` 再次发送给 API（这次不带 `tools`）。

### 获取最终回复

从第二次调用的响应或第一次调用（如果未用工具）的响应中获取 `content`。

## 用户反馈

在关键步骤（思考、搜索、整合）打印提示信息，改善用户体验。

## 全局错误处理

在 `while` 循环外部使用 `try...except` 捕获 API 错误、特定的网络连接错误（如 `SearXNG` 无法访问）和其他意外异常，向用户提供友好的错误提示，并使用 `traceback.print_exc()` 打印详细调试信息。

## 如何运行

将上述代码保存为 Python 文件（例如 `interactive_search.py`）。

### 设置环境变量

确保在运行脚本的终端中已设置 `DEEPSEEK_API_KEY`。

- Linux/macOS: `export DEEPSEEK_API_KEY='sk-xxxxxxxx'`
- Windows (CMD): `set DEEPSEEK_API_KEY=sk-xxxxxxxx`
- Windows (PowerShell): `$env:DEEPSEEK_API_KEY='sk-xxxxxxxx'`

### 安装依赖

`pip install requests openai`

### 运行脚本

在终端中执行 `python interactive_search.py`。

### 交互

程序会启动，您可以开始输入问题。尝试问一些需要最新信息的问题，观察程序是否打印 "模型决定使用 SearXNG 进行搜索" 以及最终的回复是否包含了网络搜索的信息。输入 `quit` 退出。

## 讨论与价值

这个交互式示例生动地展示了如何通过 Tool Use 机制，将大语言模型的强大语言理解和生成能力与外部工具（如网络搜索）获取实时信息的能力结合起来。这极大地扩展了 LLM 的应用范围，使其能够：

- 回答关于当前事件的问题。
- 提供基于最新数据的分析。
- 验证或补充其内部知识。
- 与各种外部 API 和服务进行交互。

虽然增加了实现的复杂性（需要编写和维护工具函数、处理多轮 API 调用和错误），但带来的价值是巨大的，是构建许多高级、实用 AI 应用的关键一步。

## 注意事项（复用与强调）

- **成本与延迟：** 每次工具调用至少涉及两次 API 调用，增加了费用和响应时间。
- **工具描述（`description`）的重要性：** 清晰、准确的描述是模型能否正确、适时使用工具的核心。
- **参数 Schema（`parameters`）：** 定义好参数结构有助于模型生成正确的调用参数。
- **工具函数（`perform_searxng_search`）的健壮性：** 必须处理好各种可能的错误（网络问题、API 限制、无效响应等），并将错误信息反馈给模型。
- **外部服务的可靠性：** 应用的整体表现依赖于所调用外部服务（如 `SearXNG` 实例）的稳定性和速度。
- **安全性：** 调用外部工具（尤其是可执行操作或访问数据的工具）时，必须高度关注安全。仔细验证模型生成的参数，防止注入攻击；确保工具本身不会被滥用。对于搜索，要注意可能返回不安全或不当内容的风险。

# 使用 Embeddings API 获取文本向量（硅基流动 & BGE-M3）

## 引言

除了生成文本（`Chat Completion`），大模型生态系统中的另一个核心功能是文本嵌入（`Text Embedding`）。文本嵌入技术能将输入的文本（单词、句子、段落）转换成一个固定维度的数字向量（通常是包含几百到几千个浮点数的列表）。这些向量捕捉了文本的语义信息，意味着在向量空间中，意思相近的文本其对应的向量也更接近。

在本场景中，我们将学习如何调用 `Silibase Flow`（硅基流动）提供的 `Embeddings API`，具体使用业界领先的 `BAAI/bge-m3` 模型（由北京智源人工智能研究院开发，支持中/英等多种语言，效果优异）来获取文本的向量表示。最后，我们将演示如何利用这些向量计算不同文本之间的语义相似度。

## 前提条件

- **硅基流动 API Key：** 您需要拥有一个硅基流动的账户，并获取对应的 API Key。强烈建议将您的 API Key 设置为环境变量 `SILICONFLOW_API_KEY`。

- Linux/macOS: `export SILICONFLOW_API_KEY='fk-xxxxxxxxxxxxxxxxxxxx'` （注意：硅基流动的 key 通常以 `fk-` 开头）

- Windows (CMD): `set SILICONFLOW_API_KEY=fk-xxxxxxxxxxxxxxxxxxxx`

- Windows (PowerShell): `$env:SILICONFLOW_API_KEY='fk-xxxxxxxxxxxxxxxxxxxx'`

- **Python 库：**

  - `openai`：我们依然使用 `openai` 库来方便地调用与 OpenAI API 兼容的硅基流动 API。如果未安装，`pip install openai`。

  - `numpy`：用于高效地进行向量运算（计算余弦相似度）。如果未安装，`pip install numpy`。

## 核心概念

### API Endpoint

硅基流动的 `Embeddings API` 端点通常是 `https://api.siliconflow.cn/v1/embeddings`。

### 请求

使用 `HTTP POST` 方法，请求体包含：

- `model`：指定要使用的模型名称，这里是 `"BAAI/bge-m3"`。
- `input`：一个包含一个或多个待嵌入文本字符串的列表。

### 响应

API 返回一个 `JSON` 对象，其中 `data` 字段是一个列表，列表中的每个元素对应输入列表中的一个文本，包含：

- `embedding`：一个包含浮点数的列表，即该文本的向量表示。`bge-m3` 模型的输出向量维度是 1024。
- `index`：该嵌入向量对应于输入列表中的索引。
- 通常还包含 `usage` 信息，统计消耗的 `token` 数量。

### 语义相似度计算

最常用的衡量两个嵌入向量相似度的方法是余弦相似度（`Cosine Similarity`）。它计算两个向量夹角的余弦值，范围在 -1 到 1 之间：

- 1: 向量方向完全相同（语义极度相似）。
- 0: 向量正交（语义不相关）。
- -1: 向量方向完全相反（语义相反）。

计算公式：`Similarity = (A · B) / (||A|| * ||B||)`，其中 `A · B` 是向量点积，`||A||` 和 `||B||` 是向量的 L2 范数（长度）。

## 编程实践：获取文本向量并计算相似度

我们将获取几句中文文本的嵌入向量，并计算它们两两之间的语义相似度。
