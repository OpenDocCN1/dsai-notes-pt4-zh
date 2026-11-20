
### ❓ 提问: Q2: 我想周末和春节都来公司加班多赚点钱，工资分别怎么算？我一年最多能加多少小时班，有没有封顶？

| 🤖 普通 RAG (Baseline) | 🚀 黑科技 RAG (Small-to-Big) |
| --- | --- |
| 公司不强制安排无偿加班，员工可以根据项目安排与主管协商加班事宜。对于加班工资的计算，具体的金额和计算方式未在手册中详细说明。至于一年最多能加多少小时班以及是否有封顶，手册中并未提供相关信息，建议与主管或人力资源部门进行进一步咨询以获取详细规定。 | 周末加班的工资是按基本工资的2倍补偿，或者可以选择以调休形式折算。春节属于法定节假日，加班工资按基本工资的3倍支付。年度个人加班时间最多为200小时，超过部分需要部门总监审批并报备HR。 |

```
2025-12-11 19:47:07,433 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings "HTTP/1.1 200 OK"
2025-12-11 19:47:10,823 - INFO - HTTP Request: POST https://ai.devtool.tech/proxy/v1/chat/completions "HTTP/1.1 200 OK"
2025-12-11 19:47:11,297 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings "HTTP/1.1 200 OK"
2025-12-11 19:47:13,295 - INFO - HTTP Request: POST https://ai.devtool.tech/proxy/v1/chat/completions "HTTP/1.1 200 OK"
```

### ❓ 提问: Q3: 我还在试用期，最近家里有事想请半个月（15天）的假，按规定可以请吗？会不会影响我转正？

| 🤖 普通 RAG (Baseline) | 🚀 黑科技 RAG (Small-to-Big) |
| --- | --- |
| 在试用期内请假通常需要遵循公司的请假规定。如果您有正当理由，可以向人力资源部门申请假期。请假是否会影响转正，通常取决于公司的具体政策和您在试用期内的表现。建议您与直接上级或人力资源部门沟通，了解请假对转正的具体影响。 | 在试用期内，你可以请假，包括病假、事假、婚假等法定假期。然而，如果累计请假超过10个工作日，试用期考核结果可能会受到影响。因此，请半个月（15天）的假可能会影响你的转正。 |

---

## 三、混合检索 (Hybrid Search)执行策略

### 1. 策略背景：告别“懂意不懂字”的单腿走路

在基础的 RAG 系统中，我们通常完全依赖向量检索（`Vector Search`）。向量检索的魔法在于它能通过计算高维空间的距离来理解“语义”。例如，用户搜“请假规则”，它能通过语义关联找到“考勤管理”或“休假制度”，即便这几个字没有完全对应。

然而，在企业级落地中，我们很快发现纯向量检索存在一个致命的“语义盲区”：它对精确匹配（`Exact Match`）极不敏感。

当用户搜索一个特定的错误码（如 `Error 503`）、一个生僻的人名（如“周云杰”）、一个专有的缩写（如 `ROWE`）或一个具体的产品型号（如 `X-2000`）时，向量模型往往会因为这些词在语义空间中过于稀疏，而检索到一堆“意思相近”但完全不相关的通用描述，导致“张冠李戴”。

混合检索（`Hybrid Search`）的诞生，正是为了解决这一痛点。它主张“两条腿走路”，既要 AI 的“脑子”（语义理解），也要传统搜索引擎的“眼睛”（字面匹配）。

### 2. 核心原理：双路召回与加权融合

混合检索策略在底层构建了两条并行的检索链路，并通过精密的算法将结果合二为一：

+   链路 A：稠密向量检索 (`Dense Retrieval`)

+   **机制：** 使用 `Embedding` 模型将文本转化为向量。

+   **强项：** 擅长处理模糊查询、概念关联、同义词匹配。

+   **角色：** 负责“懂你什么意思”。

+   链路 B：稀疏关键词检索 (`Sparse Retrieval` / `BM25`)

+   **机制：** 基于传统的 `TF-IDF`（词频-逆文档频率）算法演进而来的 `BM25` 算法。

+   **强项：** 擅长捕捉生僻词、专有名词、精确数字。它不关心语义，只关心关键词是否在文档中高频出现。

+   **角色：** 负责“精准定位字面”。

+   融合机制：倒数排名融合 (`Reciprocal Rank Fusion`, `RRF`)

+   系统不会简单地把两路结果拼在一起，而是使用 `RRF` 算法。它像一个公正的裁判，查看两个链路的排名情况。如果一个文档在向量检索中排第 1，在关键词检索中也排第 1，它的最终权重会极高；如果只在单路出现，权重则会降低。

### 3. 执行流程

```
# !pip install rank_bm25 jieba
```

```
# === Cell 1: 初始化与分词器配置 ===
import nest_asyncio
import jieba
from typing import List
from llama_index.core import SimpleDirectoryReader, VectorStoreIndex, Settings, StorageContext
from llama_index.core.node_parser import SentenceSplitter

nest_asyncio.apply()

# 1. 定义一个中文分词函数 (给 BM25 用)
def chinese_tokenizer(text: str) -> List[str]:
    # 使用 jieba 进行分词
    return list(jieba.cut(text))

# 2. 加载数据
print("📂 正在加载文档...")
documents = SimpleDirectoryReader("./data").load_data()

# 3. 将文档切分为节点 (标准切分，Chunk Size=512 适中)
# 为什么要手动切分？因为 BM25 和 向量索引 需要共享同一套节点数据
splitter = SentenceSplitter(chunk_size=512, chunk_overlap=50)
nodes = splitter.get_nodes_from_documents(documents)
print(f"✅ 文档已切分为 {len(nodes)} 个标准节点。")
```

```
C:\Users\Admin\anaconda3\envs\llamaindex\Lib\site-packages\jieba\_compat.py:18: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81.
  import pkg_resources


📂 正在加载文档...
✅ 文档已切分为 117 个标准节点。
```

```
# === Cell 2: 构建“普通 RAG” (纯向量) ===
print("🛠️ 正在构建向量索引 (Vector Index)...")

# 1. 构建向量索引
vector_index = VectorStoreIndex(nodes)

# 2. 创建纯向量检索引擎
# similarity_top_k=2
vector_engine = vector_index.as_query_engine(similarity_top_k=2)

print("✅ 普通 RAG (Vector Only) 就绪！")
```

```
🛠️ 正在构建向量索引 (Vector Index)...


2025-12-11 19:50:11,353 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings "HTTP/1.1 200 OK"
2025-12-11 19:50:12,070 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings "HTTP/1.1 200 OK"


✅ 普通 RAG (Vector Only) 就绪！
```

```
# === Cell 3: 构建“黑科技 RAG” (混合检索: Vector + BM25) ===
import inspect
from llama_index.retrievers.bm25 import BM25Retriever
from llama_index.core.retrievers import VectorIndexRetriever
from llama_index.core.retrievers import QueryFusionRetriever
from llama_index.core.query_engine import RetrieverQueryEngine

print("🚀 正在构建混合检索系统 (Hybrid Search)...")

# --- 🛠️ 自动侦测修复：从类型提示中提取 FusionMode ---
try:
    # 1. 获取 __init__ 函数的签名
    sig = inspect.signature(QueryFusionRetriever.__init__)
    # 2. 找到 'mode' 参数的类型定义
    ModeEnum = sig.parameters['mode'].annotation
    print(f"✅ 成功自动侦测到 Mode 类型: {ModeEnum}")
    
    # 3. 获取 RECIPROCAL_RANK 的值
    # 如果它是枚举，直接取属性；如果是 Optional[FusionMode]，可能需要剥离一层
    # 这里做一个简单的容错遍历
    if hasattr(ModeEnum, 'RECIPROCAL_RANK'):
        target_mode = ModeEnum.RECIPROCAL_RANK
    else:
        # 如果类型提示没拿到，或者版本太旧是字符串，直接用字符串兜底
        print("⚠️ 未找到枚举属性，尝试使用字符串模式...")
        target_mode = "reciprocal_rank"
        
except Exception as e:
    print(f"⚠️ 自动侦测失败 ({e})，回退到字符串模式...")
    target_mode = "reciprocal_rank"

print(f"👉 最终使用的 mode 参数: {target_mode}")


# 1. 创建 BM25 检索器
bm25_retriever = BM25Retriever.from_defaults(
    nodes=nodes,
    similarity_top_k=2,
    tokenizer=chinese_tokenizer
)

# 2. 创建 向量 检索器
vector_retriever = VectorIndexRetriever(index=vector_index, similarity_top_k=2)

# 3. 创建 融合检索器
fusion_retriever = QueryFusionRetriever(
    [vector_retriever, bm25_retriever],
    similarity_top_k=4,
    num_queries=1,
    # 使用我们要么侦测到、要么回退的 mode
    mode=target_mode,
    use_async=True
)

# 4. 组装成引擎
hybrid_engine = RetrieverQueryEngine.from_args(
    retriever=fusion_retriever
)

print("✅ 黑科技 RAG (Hybrid Search) 就绪！")
```

```
2025-12-11 19:58:19,832 - WARNING - The tokenizer parameter is deprecated and will be removed in a future release. Use a stemmer from PyStemmer instead.
2025-12-11 19:58:19,853 - DEBUG - Building index from IDs objects


🚀 正在构建混合检索系统 (Hybrid Search)...
✅ 成功自动侦测到 Mode 类型: <enum 'FUSION_MODES'>
👉 最终使用的 mode 参数: FUSION_MODES.RECIPROCAL_RANK
✅ 黑科技 RAG (Hybrid Search) 就绪！
```

这段代码展示了如何将 `关键词检索 (BM25)` 与 `向量检索 (Vector)` 结合，并通过 `RRF` (倒数排名融合) 算法实现 1+1 > 2 的效果。不仅如此，代码开头包含了一段处理版本兼容性的“防御性编程”技巧。

我们将代码逻辑拆解为三个核心模块进行讲解：

#### 1. 自动侦测与版本适配 (The Compatibility Fix)

```
import inspect

# --- 🛠️ 自动侦测修复：从类型提示中提取 FusionMode ---
try:
    # 1. 获取构造函数的签名 (Signature)
    sig = inspect.signature(QueryFusionRetriever.__init__)
    # 2. 侦测 'mode' 参数到底想要什么类型 (Enum 还是 String?)
    ModeEnum = sig.parameters['mode'].annotation
    # ... (省略中间判断逻辑)
    if hasattr(ModeEnum, 'RECIPROCAL_RANK'):
        target_mode = ModeEnum.RECIPROCAL_RANK
    else:
        target_mode = "reciprocal_rank"
except Exception as e:
    target_mode = "reciprocal_rank"
```

+   **为什么要写这段复杂的逻辑？**

+   **背景：** `Llama-Index` 更新极快，`v0.10.x` 版本中，`FusionMode` 从字符串变成了严格的枚举类型（`Enum`）。直接写字符串 `reciprocal_rank` 在新版会报错，直接引枚举类在旧版又会报错。

+   **原理：** 这里使用了 `Python` 的 `inspect` (反射) 模块。

+   **比喻：** 这就像在进门前，先用扫描仪看一眼门锁是“指纹锁”还是“钥匙锁”，然后再决定掏出指纹还是钥匙。这是一种“防御性编程”，确保你的代码在不同版本的 `Llama-Index` 上都能跑通，不至于因为版本升级而崩溃。

#### 2. 创建双路检索器 (Two-Stream Retrievers)

```
# 1. 创建 BM25 检索器 (关键词派 - The Eye)
bm25_retriever = BM25Retriever.from_defaults(
    nodes=nodes,
    similarity_top_k=2,     # 只要最匹配关键字的前2名
    tokenizer=chinese_tokenizer # ⚠️ 关键：中文必须传入分词函数
)

# 2. 创建 向量 检索器 (语义派 - The Brain)
vector_retriever = VectorIndexRetriever(
    index=vector_index, 
    similarity_top_k=2      # 只要语义最相似的前2名
)
```

+   **分工明确：**

+   `BM25Retriever`：它是系统的“眼睛”，负责盯着字面。如果用户搜“周云杰”，它能精准抓取包含这三个字的人名，哪怕语义上这只是一个普通名字。

+   `VectorIndexRetriever`：它是系统的“脑子”，负责理解含义。如果用户搜“法务负责人”，它能理解这和“周云杰”是关联的。

+   **注意点：** `BM25` 必须传入 `tokenizer=chinese_tokenizer`，否则它会把一整句中文当成一个长单词，无法匹配关键词。

#### 3. 融合与引擎组装 (The Fusion Core)
