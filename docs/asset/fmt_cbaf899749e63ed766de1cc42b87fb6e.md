
# LlamaIndex 智能路由引擎 (Router) 构建与测试

## 环境初始化与索引构建

```
📂 正在加载文档...
🛠️ 正在构建向量索引 (Vector Index)...

2025-12-11 20:03:14,514 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings &quot;HTTP/1.1 200 OK&quot;


📚 正在构建摘要索引 (Summary Index)...
✅ 双索引构建完毕！
```

## Cell 2: 定义工具与构建 Router

```
# === Cell 2: 定义工具与构建 Router ===
from llama_index.core.tools import QueryEngineTool
from llama_index.core.selectors import LLMSingleSelector
from llama_index.core.query_engine import RouterQueryEngine

# 1. 定义 Vector 工具 (查细节)
vector_tool = QueryEngineTool.from_defaults(
    query_engine=vector_index.as_query_engine(similarity_top_k=2),
    description=&quot;专门用于查询具体的、特定的事实细节，例如补贴金额、电话号码、具体政策条款等。&quot;
)

# 2. 定义 Summary 工具 (做总结)
# response_mode=&quot;tree_summarize&quot; 是关键，它会构建一棵摘要树，从底向上递归总结
summary_tool = QueryEngineTool.from_defaults(
    query_engine=summary_index.as_query_engine(response_mode=&quot;tree_summarize&quot;),
    description=&quot;专门用于对文档进行宏观的总结、概括全文主题、分析整体结构或提取跨章节的综合信息。&quot;
)

# 3. 构建 Router 引擎
print(&quot;🚦 正在构建智能路由引擎 (Router Engine)...&quot;)
router_engine = RouterQueryEngine(
    selector=LLMSingleSelector.from_defaults(),
    query_engine_tools=[summary_tool, vector_tool],
    verbose=True # 开启 verbose，我们可以看到它到底选了哪个工具
)

print(&quot;✅ Router 引擎就绪！&quot;)
```

```
🚦 正在构建智能路由引擎 (Router Engine)...
✅ Router 引擎就绪！
```

这段代码展示了如何将两个功能迥异的检索引擎（查细节的 Vector 和 查全貌的 Summary）封装成工具，并教会系统如何根据用户的问题自动选择最合适的一个。

### 1. 定义工具：给系统装上“手脚” (The Tools)

在 Router 模式下，索引（`Index`）本身不能直接被路由，必须先封装成 `QueryEngineTool`。

```
# 1. 定义 Vector 工具 (查细节)
vector_tool = QueryEngineTool.from_defaults(
    query_engine=vector_index.as_query_engine(similarity_top_k=2),
    description=&quot;专门用于查询具体的、特定的事实细节，例如补贴金额、电话号码、具体政策条款等。&quot;
)
```

- `QueryEngineTool`：这是 Llama-Index 中的标准工具包装器。
- `description` (至关重要！)：
  - 这不仅仅是写给开发者看的注释，它是写给 AI 看的 Prompt（指令）。
  - Router 内部的 LLM 会阅读这段文字，来判断：“这个工具是干嘛的？它能解决当前用户的问题吗？”
  - 教学点：描述写得越精准，路由的准确率就越高。

### 2. 定义摘要工具与合成策略 (The Summary Strategy)

```
# 2. 定义 Summary 工具 (做总结)
summary_tool = QueryEngineTool.from_defaults(
    # 核心参数：response_mode=&quot;tree_summarize&quot;
    query_engine=summary_index.as_query_engine(response_mode=&quot;tree_summarize&quot;),
    description=&quot;专门用于对文档进行宏观的总结、概括全文主题、分析整体结构或提取跨章节的综合信息。&quot;
)
```

- 为什么要用 `SummaryIndex`？
  - 向量索引通过计算距离找 Top-K，会漏掉全局信息。而 `SummaryIndex`（摘要索引）存储的是节点列表，它允许我们遍历文档的所有内容。
- `response_mode="tree_summarize"` (树状总结)：
  - 这是处理长文档总结的神器。
  - **原理：** 如果文档很长，它不会试图一次塞进 LLM。而是先把文档切块，分别总结（叶子节点），然后把这些小总结拼起来再总结（父节点），像爬树一样直到生成最终的根节点摘要。
  - **效果：** 能够生成覆盖全文档的高质量宏观摘要。

### 3. 构建大脑：选择器与引擎 (The Brain & Body)

```
from llama_index.core.selectors import LLMSingleSelector

# 3. 构建 Router 引擎
router_engine = RouterQueryEngine(
    selector=LLMSingleSelector.from_defaults(), # 决策核心
    query_engine_tools=[summary_tool, vector_tool], # 备选工具箱
    verbose=True # 开启日志
)
```

- `LLMSingleSelector` (决策者)：
  - 这是一个小型分类器。它接收用户的问题和工具的描述，然后输出一个最佳工具的名称。
  - 注：还有 `LLMMultiSelector`，可以同时选择多个工具（例如既查 Google 又查本地文档），但在本例中我们只需要“二选一”。
- `verbose=True` (调试模式)：
  - 在教学和调试中非常有用。开启后，你会在控制台看到系统“思考”的过程，例如：Selecting query engine 0: ...。这能让你直观地确认 Router 是否变聪明了。

## 整体执行逻辑

1. 用户提问：“公司有哪些福利？”
1. Router (`Selector`) 思考：
   - 看工具 A（Vector）描述：查具体细节、数字。-> 不太匹配。
   - 看工具 B（Summary）描述：查宏观总结、概括。-> 匹配！
1. 分发任务：将问题扔给 `summary_tool`。
1. 执行任务：`summary_index` 启动 `tree_summarize` 模式，遍历文档生成总结。
1. 返回结果：用户得到一份全面的福利概览，而不是零星的补贴数字。

## Cell 3: 定义 5 个测试问题

```
# === Cell 3: 定义 5 个测试问题 ===

questions = [
    # --- 宏观总结题 (普通 RAG 的死穴) ---
    # 1. 全文总结
    &quot;Q1: 这份员工手册主要包含了哪几个章节？请简要列出目录结构并概括每个章节的主题。&quot;,
    # 2. 跨章节综合分析
    &quot;Q2: 请详细总结一下公司的核心价值观以及我们对待多样性（D&amp;I）的整体策略。&quot;,
    
    # --- 微观细节题 (普通 RAG 的强项) ---
    # 3. 具体数字
    &quot;Q3: 公司的健身房补贴每年最高是多少钱？&quot;,
    # 4. 具体人名
    &quot;Q4: 请问法务合规部的负责人是谁？&quot;,
    # 5. 具体条款
    &quot;Q5: 试用期员工如果累计请假超过 10 天，会有什么具体后果？&quot;
]
```

## Cell 4: 效果对比展示 (修复表格渲染版)

```
# === Cell 4: 效果对比展示 (修复表格渲染版) ===
from IPython.display import display, Markdown

# 准备一个普通的 Vector Engine 作为对照组
base_engine = vector_index.as_query_engine(similarity_top_k=2)

def clean_text(text):
    &quot;&quot;&quot;
    清洗文本：将换行符替换为HTML换行标签，防止Markdown表格崩坏
    &quot;&quot;&quot;
    if not text:
        return &quot;&quot;
    # 截取前 300 字符，并将换行符替换为 &lt;br&gt;
    cleaned = str(text)[:300].replace(&quot;\n&quot;, &quot;&lt;br&gt;&quot;)
    return cleaned + &quot;...&quot;

print(&quot;🔥 开始 5 轮智能路由测试...&quot;)
print(&quot;(请耐心等待，Q1 和 Q2 需要总结全文，速度较慢...)&quot;)

for q in questions:
    display(Markdown(f&quot;### ❓ 提问: {q}&quot;))
    
    # 1. 普通 RAG
    response_base = base_engine.query(q)
    
    # 2. Router RAG
    # 这一步可能会打印 &quot;Selecting query engine...&quot; 日志，请留意
    response_router = router_engine.query(q)
    
    # 3. 提取结果并清洗
    base_text = clean_text(response_base.response)
    router_text = clean_text(response_router.response)
    
    # 4. 构建严格格式的 Markdown 表格
    # 注意：表格每一行开头和结尾都要有 |
    table_md = f&quot;&quot;&quot;
| 🤖 普通 RAG (Top-2 Vector) | 🚦 智能路由 (Router) |
| :--- | :--- |
| {base_text} | {router_text} |
&quot;&quot;&quot;
    display(Markdown(table_md))
    display(Markdown(&quot;---&quot;))
```

```
🔥 开始 5 轮智能路由测试...
(请耐心等待，Q1 和 Q2 需要总结全文，速度较慢...)
```

### ❓ 提问: Q1: 这份员工手册主要包含了哪几个章节？请简要列出目录结构并概括每个章节的主题。

```
2025-12-11 20:05:33,463 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings &quot;HTTP/1.1 200 OK&quot;
2025-12-11 20:05:38,106 - INFO - HTTP Request: POST https://ai.devtool.tech/proxy/v1/chat/completions &quot;HTTP/1.1 200 OK&quot;
2025-12-11 20:05:40,116 - INFO - HTTP Request: POST https://ai.devtool.tech/proxy/v1/chat/completions &quot;HTTP/1.1 200 OK&quot;
2025-12-11 20:05:40,118 - INFO - Selecting query engine 0: The question asks for a summary of the main sections of the employee handbook and an overview of each section&#x27;s theme, which aligns with the purpose of summarizing and analyzing the overall structure of a document as described in choice 1..


[1;3;38;5;200mSelecting query engine 0: The question asks for a summary of the main sections of the employee handbook and an overview of each section&#x27;s theme, which aligns with the purpose of summarizing and analyzing the overall structure of a document as described in choice 1..
[0m

2025-12-11 20:05:47,705 - INFO - HTTP Request: POST https://ai.devtool.tech/proxy/v1/chat/completions &quot;HTTP/1.1 200 OK&quot;
```

| 🤖 普通 RAG (Top-2 Vector) | 🚦 智能路由 (Router) |
| :--- | :--- |
| 这份员工手册主要包含以下几个章节： 1. 制度与流程 - 详细介绍了公司内部的各项制度和流程，确保员工了解并遵循公司的运作规范。 2. 法律与政策依据 - 列出了员工手册制定过程中所参照的国家法规与地方实施细则，提供了法律和政策的背景信息。 3. 专有名词释义 - 对手册中出现的专业术语进行了定义和解释，帮助员工更好地理解相关内容。 4. 修订记录与版本说明 - 记录了手册的修订历史和版本信息，确保员工了解手册的更新情况。 每个章节旨在帮助员工更好地融入公司文化，理解公司政策，并在工作中遵循相应的指导原则。... | 这份员工手册主要包含以下章节： 1. 公司概述：介绍公司的使命、愿景、价值观、组织架构及雇佣原则。 2. 员工行为准则：阐述职业道德、保密义务、知识产权、反歧视与反骚扰政策、冲突解决机制等。 3. 工作时间与出勤：描述标准工时制度、出勤要求、考勤管理、假期安排等。 4. 薪酬与福利：说明薪酬结构、年度调薪、福利计划、报销制度、奖金激励等。 5. 绩效评估：介绍绩效评估的组织架构、实施流程、评估标准、应用机制等。 6. 纪律与惩戒：定义违纪行为，说明惩戒措施、调查处理流程、申诉与复议机制。 7. 健康与安全：详细阐述职业安全、环境卫生、健... |

---

### ❓ 提问: Q2: 请详细总结一下公司的核心价值观以及我们对待多样性（D&amp;I）的整体策略。
