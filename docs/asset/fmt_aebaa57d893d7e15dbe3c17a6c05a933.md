
![](img/68c822083ee7e53a2a8231a4bfbc14ff.png)

## 代码解析

### 导入与配置（`import` & Configuration）

+ 导入所有必要的库，包括 `openai`， `numpy`， `chromadb`， `dotenv`， `gradio`， `fitz`（`PyMuPDF`）， `docx`。
+ 通过 `load_dotenv()` 加载 `.env` 文件中的环境变量（API Keys）。
+ 定义清晰的常量，用于配置 API 地址、模型名称（`BAAI/bge-m3`， `deepseek-chat`）、数据库路径（`./chroma_db_silibase`）、集合名称（`oneflower_kb_silibase_bge_m3`）、检索参数（`N_RESULTS_RETRIEVAL`， `DISTANCE_THRESHOLD`）、数据目录和处理设置。
+ 区分了 `DeepSeek` 和硅基流动的配置。

### 全局变量与初始化（`collection`， `client_ds`， `client_sf`， `initialize_`）

+ 使用全局变量存储 `ChromaDB` `collection` 和两个 `OpenAI` 客户端实例。
+ `initialize_clients()`：分别创建指向 `DeepSeek` 和硅基流动的 `OpenAI` 客户端实例，并进行必要的检查。
+ `initialize_chromadb()`：初始化 `ChromaDB` `PersistentClient`，并尝试加载或创建 `Collection`，指定了持久化路径和距离度量（`cosine`）。包含错误处理。

### 数据加载与分块（`load_and_chunk_data`）

+ 函数接收数据目录路径。
+ 遍历目录，根据 `SUPPORTED_FILE_EXTENSIONS` 识别文件。
+ 使用 `fitz` 处理 `PDF`，`python-docx` 处理 `DOCX`，标准文件 I/O 处理 `TXT`/`MD`。`PDF` 处理中加入了页码信息。
+ 将提取的文本按 `\n\n` 分割。实现了一个简单的合并短段落的策略，可以替换为更高级的逻辑。
+ 过滤掉长度小于 `CHUNK_MIN_LENGTH` 的块。
+ 为每个有效块生成元数据（只包含相对路径的 `source`）和唯一 `ID`。
+ 返回文本块列表、元数据列表和 `ID` 列表。包含详细的日志和错误处理。

### 嵌入函数（`get_embeddings_from_silibase`）

+ 关键改动：使用 `client_sf`（硅基流动客户端）。
+ 调用 `client_sf.embeddings.create`，传入模型名称 `BAAI/bge-m3` 和文本列表。
+ 处理 API 返回结果，按原始索引顺序排列嵌入向量。
+ 包含对 `AuthenticationError`， `APIError` 和其他异常的捕获，并在失败时返回 `None` 列表。

### 数据索引（`index_knowledge_base`）

+ 接收文本块、元数据和 `ID`。
+ 调用 `get_embeddings_from_silibase` 获取所有嵌入。
+ 严格检查：确认所有嵌入都成功获取并且维度正确（`EMBEDDING_DIMENSION`），如果任何一个失败则中止索引。
+ 使用 `collection.add()` 批量将数据添加到 `ChromaDB`。包含错误处理。

### RAG 核心函数（`get_rag_answer`）

+ 执行 `RAG` 流程：获取查询 -> 嵌入查询（调用 `get_embeddings_from_silibase`）-> 检索（`ChromaDB` `query`，请求 `distances`）-> 相关性过滤（基于 `DISTANCE_THRESHOLD`）-> 构建上下文 -> 构建 `Prompt` -> 调用 `LLM`（使用 `client_ds.chat.completions.create` 调用 `DeepSeek`）-> 返回答案。
+ 关键改动：明确加入了基于 `distances` 和 `DISTANCE_THRESHOLD` 的过滤逻辑。
+ `Prompt` 模板指导 `LLM` 如何使用上下文和处理无信息情况。
+ 对 `RAG` 流程中的每个关键步骤（嵌入、检索、`LLM` 调用）都添加了错误处理和日志记录。
+ 返回用户友好的错误信息或最终答案。

### 主程序入口（`if name == "__main__":`）

+ 按顺序执行初始化（客户端、数据库）。
+ 检查数据库是否为空，如果为空，则执行数据加载、分块和索引流程。增加了对索引失败的致命错误处理。
+ 启动 `Gradio` 界面，将 `get_rag_answer` 作为处理函数。界面包含标题、描述、输入/输出框和示例问题。

## 后续优化建议

### 分块策略（`Chunking`）

+ 当前按段落分割可能导致块大小不一。尝试使用 `RecursiveCharacterTextSplitter`（来自 `LangChain` 等库）实现固定大小 + 重叠的分块，可以更好地保留上下文联系。
+ 探索语义分块，根据文本内容的语义变化来切分，可能产生更相关的知识块。

### 异步处理与流式输出

+ 将 `get_embeddings_from_silibase` 和 `get_rag_answer` 中的 API 调用（尤其是 `DeepSeek` `LLM` 调用）改为异步（`async/await`）模式。
+ 修改 `get_rag_answer` 以支持 `DeepSeek API` 的流式输出（`stream=True`），并将结果逐步 `yield` 给 `Gradio`，极大提升用户界面的响应速度和体验。

### 检索优化

+ 相关性阈值调优：`DISTANCE_THRESHOLD` 的值（当前为 `1.0`）需要根据实际数据和嵌入模型的效果进行实验调整，找到最佳平衡点。
+ 引入 `Re-ranking`：在从 `ChromaDB` 检索 `Top-K` 结果后，可以使用更精确但计算量更大的 `Cross-Encoder` 模型对这些结果进行重新排序，选择最终用于上下文的 `N` 个片段（`N <= K`）。
+ 混合搜索（`Hybrid Search`）：结合关键词搜索和向量搜索，处理那些既需要语义匹配又需要精确词语匹配的查询。

### `Prompt` 工程

+ 来源引用：修改 `Prompt`，让 `LLM` 在回答时，能够根据提供的元数据（例如 `source` 文件名）指明信息来源。
+ 动态 `Prompt`：根据检索到的上下文数量或相关性得分，动态调整 `Prompt` 的措辞或结构。

### 错误处理与健壮性

+ 为 API 调用增加重试机制（例如使用 `tenacity` 库），应对临时的网络波动或服务抖动。
+ 在 `Gradio` 界面提供更具体的错误反馈，而不是简单的“发生错误”。

### 本地嵌入模型

+ 如果硬件资源允许（特别是 `GPU`），可以考虑使用 `sentence-transformers` 库直接在本地运行 `BAAI/bge-m3` 或其他开源嵌入模型，去除对硅基流动 API 的依赖，可能降低成本和延迟。

# 附录

## 高级提示词管理技巧和最佳实践

### 面临的挑战

+ 可读性与维护性：将长提示词硬编码在业务逻辑代码中，会严重降低代码可读性。修改提示词需要深入代码内部，容易出错。
+ 复用性：很多提示词可能有相似的结构或指令，简单复制粘贴会导致冗余和不一致。
+ 版本控制：提示词的效果往往需要不断迭代优化。如何追踪哪个版本的提示词效果最好？如何回滚到之前的版本？
+ 协作：不同团队成员可能需要创建、修改或使用提示词，需要统一的管理方式。
+ 动态性：很多提示词需要根据不同的输入动态填充变量。
+ 评估与测试：需要方便地对不同提示词版本进行 A/B 测试或离线评估。

### 最佳实践与管理策略

以下是一些管理大量提示词的最佳实践和常用策略，从简单到复杂：

#### 将提示词作为常量或配置

+ **实践：** 最基础的方法是将提示词定义为 `Python` 模块中的常量字符串，或者存储在专门的配置文件中（如 `.py`， `.json`， `.yaml`）。
+ **优点：** 将提示词与业务逻辑代码分离，提高了可读性。修改提示词只需在特定文件或位置进行。
+ **缺点：** 对于大量提示词，单个文件可能变得臃肿。缺乏结构化和模板化能力。
+ **示例（`Python` 模块）：**

```python
# prompts.py
CUSTOMER_SERVICE_SYSTEM_PROMPT = """
你是一个专业的“易速鲜花”客服助手。请根据用户问题提供友好、准确的帮助。
"""

ORDER_QUERY_PROMPT_TEMPLATE = """
根据以下订单信息回答用户关于订单号 {order_id} 的问题。
订单信息：{order_details}
用户问题：{user_query}
请仅基于提供的订单信息回答。
"""
# main.py
# import prompts
# system_message = prompts.CUSTOMER_SERVICE_SYSTEM_PROMPT
# user_prompt = prompts.ORDER_QUERY_PROMPT_TEMPLATE.format(...)
```

+ **示例（`YAML` 文件）：**

```yaml
# prompts.yaml
customer_service:
  system_prompt: |
    你是一个专业的“易速鲜花”客服助手。请根据用户问题提供友好、准确的帮助。
order_query:
  template: |
    根据以下订单信息回答用户关于订单号 {order_id} 的问题。
    订单信息：{order_details}
    用户问题：{user_query}
    请仅基于提供的订单信息回答。
  input_variables: ['order_id', 'order_details', 'user_query']
```

```python
# main.py
# import yaml
# with open('prompts.yaml', 'r', encoding='utf-8') as f:
#     prompts_config = yaml.safe_load(f)
# system_message = prompts_config['customer_service']['system_prompt']
# order_template = prompts_config['order_query']['template']
# user_prompt = order_template.format(order_id=..., order_details=..., user_query=...)
```

#### 使用提示词模板（`Prompt Templates`）

+ **实践：** 利用模板引擎来创建包含占位符（变量）的提示词。这使得提示词可以动态生成。
+ 简单模板：`Python` 的 `f-strings` 或 `.format()` 方法适用于简单的替换。
+ 高级模板引擎（`Jinja2`）：对于更复杂的逻辑，如条件判断（`if/else`）、循环（`for`）、宏定义、模板继承等，可以使用强大的模板库 `Jinja2`。
+ **优点：** 提高了提示词的复用性和动态性。`Jinja2` 尤其适合构建结构复杂、包含可选部分的提示词。
+ **示例（`Jinja2`）：**

```jinja2
{# order_summary_prompt.j2 #}
请根据以下信息为用户总结订单 {order_id} 的状态。

订单状态：{{ status }}
{% if estimated_delivery %}
预计送达时间：{{ estimated_delivery }}
{% else %}
目前暂无预计送达时间。
{% endif %}
{% if items %}
订单商品：
{% for item in items %}
- {{ item.name }} (数量: {{ item.quantity }})
{% endfor %}
{% endif %}
{% if notes %}
备注：{{ notes }}
{% endif %}
```

```python
# main.py
# from jinja2 import Environment, FileSystemLoader
# env = Environment(loader=FileSystemLoader('.')) # 加载当前目录的模板
# template = env.get_template('order_summary_prompt.j2')
#
# prompt_data = {
#     'order_id': '12345',
#     'status': '已发货',
#     'estimated_delivery': '2025-04-10',
#     'items': [{'name': '玫瑰花束', 'quantity': 1}, {'name': '贺卡', 'quantity': 1}],
#     'notes': '请在上午送达'
# }
# rendered_prompt = template.render(prompt_data)
# print(rendered_prompt)
```

#### 利用 `LLM` 框架（如 `LangChain`， `LlamaIndex`）

+ **实践：** 这些框架内置了强大的提示词管理功能。
+ `PromptTemplate` / `ChatPromptTemplate`：标准化模板创建，支持输入变量校验、部分格式化、模板组合等。
+ 示例序列化（`Example Selectors`）：可以根据输入动态选择最相关的几个示例加入到提示词中（`Few-shot Learning`）。
+ 提示词流水线（`Pipelines/Chains`）：可以将多个提示词（和模型调用、工具调用）串联起来形成复杂的工作流。
+ **优点：** 提供了一套成熟、集成化的解决方案，与框架的其他组件（模型、检索器、记忆等）无缝集成。简化了许多常见模式的实现。
+ **缺点：** 引入了框架依赖，需要学习框架的特定用法。
+ 概念示例（`LangChain`）：

```python
# from langchain_core.prompts import ChatPromptTemplate, SystemMessagePromptTemplate, HumanMessagePromptTemplate
# system_template = "你是一个专业的 {role}。"
# human_template = "请回答关于 {topic} 的问题。"
# chat_prompt = ChatPromptTemplate.from_messages([
#     SystemMessagePromptTemplate.from_template(system_template),
#     HumanMessagePromptTemplate.from_template(human_template)
# ])
# formatted_prompt = chat_prompt.format_messages(role="数据分析师", topic="Python")
# print(formatted_prompt)
# # Output: [SystemMessage(content='你是一个专业的数据分析师。'), HumanMessage(content='请回答关于 Python 的问题。')]
```

#### 版本控制系统（如 `Git`）

+ **实践：** 将存储提示词的文件（`.py`， `.yaml`， `.j2` 等）纳入 `Git` 等版本控制系统进行管理。
+ **优点：** 可以追踪提示词的修改历史，比较不同版本的效果，方便回滚，支持分支开发和协作。
+ **关键：** 像对待代码一样对待提示词文件，编写有意义的提交信息（`commit messages`）来记录修改原因和效果。

#### 专门的提示词管理工具/平台

+ **实践：** 随着 `LLM` 应用的发展，出现了一些专门用于管理、协作、测试和部署提示词的工具或平台（有开源也有商业产品）。
