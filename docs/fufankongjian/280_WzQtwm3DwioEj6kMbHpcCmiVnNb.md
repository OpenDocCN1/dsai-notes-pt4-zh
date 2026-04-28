# 03-AgenticRAG规范-v1.0

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/MlmKdNpZSo7i6Kxd1z0c5kNhnaf](https://kq4b3vgg5b.feishu.cn/docx/MlmKdNpZSo7i6Kxd1z0c5kNhnaf)

# Agentic RAG 规范 v1.0

> 适用场景：企业内部知识库 / 客服智能问答 / 技术文档检索 / 行业资料分析

> 项目阶段：Agent 开发阶段

> 版本号：v1.0

> 更新日期：2026-04

> 来源：赋范大模型技术社区 · 多模态 RAG 项目实战课

---

## 一、为什么用 Agentic RAG

传统 RAG 的天花板是 TopK 检索——K 设小，召回率低，问题答不全；K 设大，准确率低，噪声多。这个矛盾在固定检索流程里无解。

Agentic RAG 的核心思路：把检索策略的决策权交给 Agent，而不是把它写死在代码里。

具体差异：

| 维度 | 传统 RAG | Agentic RAG |
| --- | --- | --- |
| 检索策略 | 固定 TopK 向量检索 | Agent 动态选择检索工具 |
| 多轮检索 | 不支持 | 支持（检索 → 判断 → 追加检索） |
| 知识图谱利用 | 无 | 图谱推理 + 向量检索混合 |
| 不确定时的行为 | 返回置信度低的结果 | 主动追加检索或说明无法回答 |
| 可解释性 | 低 | 可追溯 Agent 推理路径 |

---

## 二、架构总览

```
用户问题 (Query)
    │
    ▼
[Query 理解层]
    • 意图分类（事实查询 / 推理查询 / 图谱关系查询）
    • 实体抽取（用于图谱精确查找）
    • 改写（query rewrite，提升召回率）
    │
    ▼
[LangChain Agent]
    • 模型：DeepSeek-V3（via ChatOpenAI）
    • 框架：LangChain 1.0 createAgent
    • 工具集：↓
    │
    ├── Tool: vector_search_text     # 稠密向量 + 稀疏向量混合检索
    ├── Tool: vector_search_image    # 跨模态图片检索
    ├── Tool: graph_search           # 知识图谱 DFS 检索
    ├── Tool: graph_entity_lookup    # 精确实体查找
    └── Tool: hybrid_rerank          # 多路结果重排
    │
    ▼
[检索结果融合层]
    • 去重（基于 chunk_id 和语义相似度）
    • 重排（Cohere Rerank 或 BGE-Reranker）
    • 截断（最终送入 LLM 的上下文控制在 4096 tokens 内）
    │
    ▼
[LLM 生成层]
    • 模型：DeepSeek-V3
    • 提示词结构：见第五节
    • 输出格式：结构化 JSON（含答案、来源引用、置信度）
    │
    ▼
最终答案 + 来源列表 + 推理路径
```

---

## 三、Tool 规范

### 3.1 vector_search_text

```
@tool
def vector_search_text(
    query: str,
    top_k: int = 10,
    filters: Optional[dict] = None,
    search_type: Literal[&quot;dense&quot;, &quot;sparse&quot;, &quot;hybrid&quot;] = &quot;hybrid&quot;
) -&gt; List[TextChunk]:
    &quot;&quot;&quot;
    从向量存储中检索相关文本块。
    优先用 hybrid 模式（稠密 + 稀疏融合）。
    filters 支持按 file_id、page_number、chunk_type 过滤。
    &quot;&quot;&quot;
```

**返回格式**：

```
[
  {
    &quot;chunk_id&quot;: &quot;doc_001_chunk_003&quot;,
    &quot;content&quot;: &quot;...&quot;,
    &quot;score&quot;: 0.92,
    &quot;metadata&quot;: {
      &quot;file_id&quot;: &quot;doc_001&quot;,
      &quot;page_number&quot;: 3,
      &quot;section_title&quot;: &quot;系统架构设计&quot;
    }
  }
]
```

### 3.2 vector_search_image

```
@tool
def vector_search_image(
    query_text: str,
    top_k: int = 5
) -&gt; List[ImageChunk]:
    &quot;&quot;&quot;
    通过文本 query 检索相关图片（跨模态检索）。
    使用 OpenCLIP 的文本 Embedding 与图片 Embedding 做相似度计算。
    &quot;&quot;&quot;
```

### 3.3 graph_search

```
@tool
def graph_search(
    entity_name: str,
    depth: int = 2,
    relation_types: Optional[List[str]] = None
) -&gt; SubGraph:
    &quot;&quot;&quot;
    从知识图谱中以指定实体为起点做深度优先搜索。
    返回指定深度内的子图（节点 + 边）。
    适合&quot;A 和 B 有什么关系&quot;这类图谱推理问题。
    &quot;&quot;&quot;
```

**技术实现**：使用 networkX 的 dfs_successors + 自定义深度限制，不依赖 Embedding，纯 KG 结构检索。

### 3.4 graph_entity_lookup

```
@tool
def graph_entity_lookup(
    entity_name: str,
    fuzzy: bool = True
) -&gt; Optional[EntityNode]:
    &quot;&quot;&quot;
    精确查找知识图谱中的实体节点。
    fuzzy=True 时支持模糊匹配（编辑距离 ≤ 2）。
    适合&quot;X 是什么&quot;这类实体定义查询。
    &quot;&quot;&quot;
```

### 3.5 hybrid_rerank

```
@tool
def hybrid_rerank(
    query: str,
    candidates: List[Union[TextChunk, ImageChunk]],
    top_n: int = 5
) -&gt; List[RankedChunk]:
    &quot;&quot;&quot;
    对多路检索结果做统一重排。
    使用 BGE-Reranker-v2-m3 模型，支持文本和图片混合重排。
    &quot;&quot;&quot;
```

---

## 四、Agent 推理链路示例

**问题**：「LangChain Agent 是怎么和知识图谱结合的？」

```
Step 1 [Query 理解]
  意图：推理查询（技术原理）
  实体：[&quot;LangChain Agent&quot;, &quot;知识图谱&quot;]

Step 2 [Agent 决策]
  → 调用 graph_entity_lookup(&quot;LangChain Agent&quot;)
  → 调用 graph_entity_lookup(&quot;知识图谱&quot;)
  → 两个实体都存在，调用 graph_search(&quot;LangChain Agent&quot;, depth=2)

Step 3 [图谱检索结果]
  返回子图：LangChain Agent → USES → networkX
           LangChain Agent → CALLS → graph_search tool
           networkX → STORES → KnowledgeGraph

Step 4 [向量补充]
  → 调用 vector_search_text(&quot;LangChain Agent 知识图谱结合方式&quot;, top_k=5)
  → 返回 5 个相关文本块

Step 5 [结果融合]
  → 调用 hybrid_rerank(query, graph_results + vector_results, top_n=5)
  → 筛选出 5 个最相关片段

Step 6 [生成]
  → 将图谱路径 + top5 文本块 + 原问题送入 DeepSeek-V3
  → 生成结构化答案
```

---

## 五、LLM 提示词结构

```
SYSTEM_PROMPT = &quot;&quot;&quot;你是一个企业级 RAG 系统的问答助手。
你的回答必须基于提供的检索结果，不得凭空编造。
如果检索结果不足以回答问题，明确说明&quot;根据现有资料无法确认&quot;，不要猜测。

回答格式（JSON）：
{
    &quot;answer&quot;: &quot;...&quot;,          // 主要答案，200 字以内
    &quot;details&quot;: &quot;...&quot;,         // 详细说明（可选，超过 200 字时使用）
    &quot;sources&quot;: [              // 来源引用
        {
            &quot;chunk_id&quot;: &quot;...&quot;,
            &quot;file_name&quot;: &quot;...&quot;,
            &quot;relevant_content&quot;: &quot;...&quot;  // 最相关的原文片段
        }
    ],
    &quot;confidence&quot;: 0.95,       // 0-1，基于检索结果质量的置信度估计
    &quot;need_more_info&quot;: false   // 是否需要用户补充信息
}&quot;&quot;&quot;

USER_PROMPT_TEMPLATE = &quot;&quot;&quot;问题：{query}

检索结果：
{retrieved_context}

图谱推理路径（如有）：
{graph_path}

请基于以上内容回答问题。&quot;&quot;&quot;
```

---

## 六、与 Bridge Pipeline 的接口约定

AgenticRAG 通过以下方式获取 Bridge Pipeline 处理的数据：

```
# 初始化时注入数据访问层（依赖注入，不直接依赖存储实现）
class AgenticRAG:
    def __init__(
        self,
        text_store: TextVectorStore,    # Bridge Pipeline 的向量存储抽象
        image_store: ImageVectorStore,
        graph_store: GraphStore,
        llm: BaseChatModel
    ):
        ...
```

**约定**：

1.  AgenticRAG 只通过抽象接口访问存储，不感知底层是 Qdrant 还是其他

1.  所有检索结果必须包含 chunk_id 和 file_id，用于来源溯源

1.  图谱节点 ID 必须与向量存储中的 chunk_id 可关联（通过 source_chunk_ids 字段）

---

## 七、性能基准

在本项目 MVP 验证环境（40 节点、39 边知识图谱 + ~500 个文本块）中：

| 指标 | 数值 |
| --- | --- |
| 单次查询响应时间 | 1.2s - 3.5s（含 LLM 生成） |
| 向量检索延迟 | 50ms - 150ms |
| 图谱检索延迟（depth=2） | 20ms - 80ms |
| Agent 推理步骤数 | 2 - 5 步 |
| 测试用例通过率 | 4/4（100%） |

生产环境（大规模数据）性能优化方向：

+   向量检索：开启 Qdrant HNSW 索引

+   图谱检索：切换到 Neo4j 并为高频查询实体建立索引

+   LLM 调用：接入 prompt 缓存减少重复计算