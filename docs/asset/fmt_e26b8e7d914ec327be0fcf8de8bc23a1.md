
# Part 2. Llama-Index RAG 进阶检索策略介绍

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/UQmPd7jJGoJz6OxtzLdc9ehEnJ8](https://kq4b3vgg5b.feishu.cn/docx/UQmPd7jJGoJz6OxtzLdc9ehEnJ8)

## 一、Llama-Index 进阶 RAG 策略介绍

在掌握了基础的 RAG 流程后，我们很快就会在实际生产环境中遇到瓶颈。本章将揭示传统 RAG 的局限性，并重点介绍 Llama-Index 提供的三种“破局”策略。这些策略正是我们将 AI 从“玩具”提升为“工具”的关键技术。

### 1. 传统 RAG 的局限性：为何“Top-K”不够用？

传统的 RAG（即 Naive RAG）架构通常非常简单直接：将文档切分成固定大小的块（Chunk），计算向量，然后根据余弦相似度检索 Top-K 个片段。这种“一刀切”的模式在处理复杂业务文档时，往往面临三大挑战：

1. 精度与上下文的矛盾（Granularity Trade-off）：
   - 如果切片太小，虽然检索精准，但 LLM 看到的上下文支离破碎，容易断章取义。
   - 如果切片太大，虽然上下文完整，但包含了大量无关噪声，且检索匹配度会下降。

2. “懂意思不懂字面”的盲区（Semantic vs. Keyword）：
   - 向量检索擅长捕捉语义（如“请假”和“休假”），但对于精确的实体名称（如人名“周云杰”）、专有缩写（如“ROWE”）或具体数字（如“200小时”），往往不如传统的关键词搜索准确。

3. 缺乏宏观与微观的判断力（Lack of Intent）：
   - 系统对待所有问题一视同仁。无论用户问的是“总结全文”（宏观），还是“查某条款”（微观），它都只会呆板地检索几个片段。用“管中窥豹”的方式去回答宏观总结类问题，效果注定失败。

为了解决这些痛点，Llama-Index 引入了一系列高级检索策略。我们将重点学习以下三种核心技术：

### 2. 进阶策略一：小索引，大窗口 (Small-to-Big Retrieval)

**核心思想：** “检索粒度”与“生成粒度”的分离。

- **原理：** 我们不直接对大段落进行索引，而是将文档切分成极小的颗粒（例如：句子）。检索时，利用句子级的高精度匹配找到目标；但在通过 Prompt 发送给 LLM 之前，系统会自动将这个“小句子”扩展为包含其前后上下文的“大窗口”。

- **解决的问题：**
  - 解决了“断章取义”的问题。
  - 让检索像手术刀一样精准，同时让回答像专家一样全面。
  - 典型场景：法律条款查询、规章制度细节问答（如“迟到/旷工后果”）。

- 流程示意图

![](img/4d16b1bf4acd23e75b80485a598a1d1a.png)

- 实际运行效果展示

![](img/0ef972489d6f95ccd7c3b848c6c08969.png)

### 3. 进阶策略二：混合检索 (Hybrid Search)

**核心思想：** **语义理解（Vector）与关键词匹配（Keyword）**的“双剑合璧”。

- **原理：** 系统同时运行两套检索机制：

1. 稠密向量检索 (Dense Vector)：负责理解问题的语义和意图。
2. 稀疏关键词检索 (BM25)：负责精准匹配专有名词、缩写和人名。

- 最后通过重排序算法（如 RRF - 倒数排名融合）将两路结果的优势结合起来。

- **解决的问题：**
  - 弥补了向量模型对“生僻词、人名、缩写、ID”不敏感的缺陷。
  - 大幅提升了在特定领域（如医疗、法律、内部行政）的召回率。
  - 典型场景：查找特定联系人邮箱、查询专有缩写定义（如“法务负责人”和“BYOD”）。

- 流程示意图

![](img/b5063a79f2c74946a1f270e38fd6eeee.png)

- 实际运行效果展示

![](img/99b92bc5de62817b9d867cba52c8da57.png)

### 4. 进阶策略三：智能路由 (Router Query Engine)

**核心思想：** **给 RAG 系统装上一个“分诊台”**（Agentic Behavior）。

- **原理：** 我们不再用单一的索引处理所有问题，而是构建多个针对性索引（例如：一个擅长查细节的 Vector Index，一个擅长做总结的 Summary Index）。在检索之前，引入一个“路由层（Router）”，由 LLM 先分析用户意图，再决定分发给哪个索引，甚至同时分发给多个。

- **解决的问题：**
  - 解决了“用战术动作解决战略问题”的尴尬。
  - 实现了按需分配算力，既能快速查细节，又能深度做总结。
  - 典型场景：既要回答“某项补贴金额”，又要回答“公司价值观总结”（如复杂多意图场景）。

- 流程示意图

![](img/6c82b6740c0f2b24c3e2fb7cc88f48a8.png)

- 实际运行效果展示

![](img/7c7966e31c8f73df6eca00a2697641cd.png)

### 5. 基本开发环境准备

```
import os
from dotenv import load_dotenv
import llama_index.core
```

```
# 1. 加载环境变量
load_dotenv(override=True)

# 2. 检查 Llama-Index 版本
print(f"Llama-Index Version: {llama_index.core.__version__}")
```

```
Llama-Index Version: 0.14.10
```

```
from llama_index.core import Settings
from llama_index.llms.openai import OpenAI
```

```
base_url=os.getenv("BASE_URL")
api_key=os.getenv("OPENAI_API_KEY")
```

```
# 配置 LLM (大语言模型)
Settings.llm = OpenAI(              
    model="gpt-4o",
    api_key=api_key,
    api_base=base_url                  
)
```

```
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

# 1. 数据摄入 (Loading)
documents = SimpleDirectoryReader("data").load_data()

# 2. 索引构建 (Indexing)
index = VectorStoreIndex.from_documents(documents)

# 3. 引擎配置 (Query Engine)
query_engine = index.as_query_engine()

# 4. 执行查询 (Execution)
# 注意：aquery 是异步调用，在 Jupyter 或标准脚本中通常使用同步的 query
response = query_engine.query("请问，我们公司有病假政策么？请用中文进行回复。")
```

```
2025-12-11 19:09:57,729 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings "HTTP/1.1 200 OK"
2025-12-11 19:09:58,785 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings "HTTP/1.1 200 OK"
2025-12-11 19:10:01,429 - INFO - HTTP Request: POST https://ai.devtool.tech/proxy/v1/chat/completions "HTTP/1.1 200 OK"
```

```
response.response
```

```
'是的，公司有病假政策。病假最长可达6个月，带薪比例根据员工的工龄计算。'
```

## 二、小索引，大窗口RAG策略详解

在 RAG 系统中，我们经常面临一个两难的选择：切片（Chunk）切小了，检索虽然精准，但丢给大模型的上下文支离破碎；切片切大了，上下文虽然完整，但包含大量无关噪声，导致检索准确率下降。

Small-to-Big（小索引，大窗口）策略，又称 Sentence Window Retrieval，正是为了解决这一痛点而生。该策略的核心思想在于将用于“搜索”的数据与用于“给 LLM 看”的数据分离开来。

- 小索引 (Small Index for Retrieval)：

系统将文档切分为极细颗粒度的“单句”。我们在向量库中存储的是这些单句的向量。这样做的好处是，当用户提问时，问题能与最相关的某一句特定的话产生极高的相似度匹配，最大程度减少噪声干扰。

- 大窗口 (Big Window for Generation)：

虽然我们搜的是句子，但单句往往缺乏上下文（例如“否则将被解雇”，光看这句不知道“否则”是指什么）。因此，我们在切分时，会预先将这句子前后相邻的 N 句话作为元数据（Metadata）存储起来。

- 偷梁换柱 (Metadata Replacement)：

在检索到目标句子后，系统在发送给 LLM 之前，会执行一个“后处理”动作：将这个孤立的句子替换为它预存的“大窗口”内容。

```
# 3. 检查数据文件是否存在
if not os.path.exists("./data/创新科技股份有限公司员工手册.txt"):
    print("❌ 错误：请确保 './data' 目录下存放了员工手册 txt 文件！")
else:
    print("✅ 环境检查通过，正在加载数据...")
    # 加载文档
    documents = SimpleDirectoryReader("./data").load_data()
    print(f"📄 成功加载文档，共 {len(documents)} 页/部分。")
```

```
✅ 环境检查通过，正在加载数据...
📄 成功加载文档，共 1 页/部分。
```

```
# === Cell 2: 构建“普通 RAG” (Baseline) ===

print("🛠️ 正在构建普通 RAG 索引 (使用默认 Chunking)...")

# 1. 建立普通索引
# 默认行为：Chunk Size = 1024, Top-k = 2
base_index = VectorStoreIndex.from_documents(documents)
base_engine = base_index.as_query_engine(similarity_top_k=2)

print("✅ 普通 RAG 就绪！")

# 定义我们的“找茬”问题集
test_questions = [
    "Q1: 如果我这个月迟到了 4 次，会受到什么样的具体处理？如果连续 3 天没打卡呢？",
    "Q2: 我想周末和春节都来公司加班多赚点钱，工资分别怎么算？我一年最多能加多少小时班，有没有封顶？",
    "Q3: 我还在试用期，最近家里有事想请半个月（15天）的假，按规定可以请吗？会不会影响我转正？"
]
```

```
🛠️ 正在构建普通 RAG 索引 (使用默认 Chunking)...

2025-12-11 19:41:03,477 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings "HTTP/1.1 200 OK"


✅ 普通 RAG 就绪！
```

```
# === Cell 3: 构建“黑科技 RAG” (Small-to-Big / Sentence Window) ===
from llama_index.core.node_parser import SentenceWindowNodeParser
from llama_index.core.postprocessor import MetadataReplacementPostProcessor
import re

print("🚀 正在构建黑科技 RAG 索引 (Small-to-Big)...")

# --- 🛠️ 关键修复：定义一个中文句子切分函数 ---
def chinese_sentence_splitter(text):
    # 使用正则表达式，按中文句号、问号、感叹号及换行符进行切分
    return re.split(r'(?<=[。？！\n])', text)

# 1. 定义窗口切分器 (加入 sentence_splitter 参数)
node_parser = SentenceWindowNodeParser.from_defaults(
    # 告诉它用我们定义的中文切分函数
    sentence_splitter=chinese_sentence_splitter, 
    # 窗口大小
    window_size=3,
    window_metadata_key="window",
    original_text_metadata_key="original_text",
)

# 2. 手动切分文档
nodes = node_parser.get_nodes_from_documents(documents)
print(f"🔪 文档被切分为 {len(nodes)} 个句子节点")

# --- 🔍 验证一下是否切分成功 (如果这里打印的是1，说明还是没切开) ---
if len(nodes) < 10:
    print("⚠️ 警告：节点数量过少，可能切分失败！请检查分割符。")
else:
    print(f"✅ 切分成功！第一个节点预览: {nodes[0].text[:50]}...")

# 3. 建立索引 (现在每个 Node 都很小，不会报错了)
advanced_index = VectorStoreIndex(nodes)

# 4. 创建引擎
advanced_engine = advanced_index.as_query_engine(
    similarity_top_k=5, 
    node_postprocessors=[
        MetadataReplacementPostProcessor(target_metadata_key="window")
    ]
)

print("✅ 黑科技 RAG 就绪！")
```
