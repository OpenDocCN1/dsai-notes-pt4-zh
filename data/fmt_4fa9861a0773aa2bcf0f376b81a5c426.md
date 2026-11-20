
# Bridge Pipeline 规范 v1.0

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/LU8YdWVOAo1MIexHQnxcY8ZFnvb](https://kq4b3vgg5b.feishu.cn/docx/LU8YdWVOAo1MIexHQnxcY8ZFnvb)

> 适用场景：企业内部知识库 / 客服智能问答 / 技术文档检索 / 行业资料分析

> 项目阶段：数据工程阶段

> 版本号：v1.0

> 更新日期：2026-04

> 来源：赋范大模型技术社区 · 多模态 `RAG` 项目实战课

---

## 一、Pipeline 职责定义

`Bridge Pipeline` 是多模态数据从原始文件进入可检索状态的完整处理链路。

**输入**：原始文档（PDF、图片、纯文本）

**输出**：

+ 向量存储里的 `Embedding` 记录（文本块 + 图片）

+ 知识图谱存储里的节点和关系（通过 `LangExtract` 抽取）

+ 元数据索引（文件信息、处理状态、分块映射）

**核心原则**：`Bridge Pipeline` 不执行检索，不做问答。它只负责数据进来、索引建好。数据质量在这里决定，后续所有 RAG 的上限在这里锁定。

---

## 二、整体数据流

```
原始文件 (PDF / Image / Text)
    │
    ▼
[文件校验层]
    • 格式检查（支持类型：.pdf / .jpg / .png / .txt / .md）
    • 大小限制（单文件 ≤ 50MB）
    • 重复检测（MD5 哈希）
    │
    ▼
[ManaU 解析层]
    • PDF → 文本块 + 图片块 + 表格块
    • 图片 → 原始图片数据 + OCR 文本（可选）
    • 纯文本 → 直接进入分块层
    │
    ├──────────────────────────────┐
    ▼                              ▼
[文本处理分支]              [图片处理分支]
    │                              │
    ▼                              ▼
[语义分块]                  [图片 Embedding]
    • Chunk Size: 512 tokens        • OpenCLIP ViT-B/32
    • Overlap: 64 tokens           • 维度: 512
    • 策略: 按段落 + 滑动窗口       • 存储: Qdrant image_collection
    │
    ▼
[BGE-M3 Embedding]
    • 稠密向量: 1024 维
    • 稀疏向量: BM25 权重
    • 存储: Qdrant text_collection
    │
    ▼
[LangExtract 知识图谱抽取]
    • 实体识别（人名、组织、概念、技术术语）
    • 关系抽取（三元组：主体-关系-客体）
    • 存储: networkX（MVP）/ Neo4j（生产）
    │
    ▼
[元数据写入]
    • 文件信息（name / size / type / upload_time）
    • 处理状态（pending / processing / completed / failed）
    • 分块映射（file_id → chunk_ids）
    • 图谱统计（node_count / edge_count）
```

---

## 三、文本分块规范

### 3.1 分块策略

```
CHUNK_CONFIG = {
    "chunk_size": 512,          # tokens，不是字符数
    "chunk_overlap": 64,        # 相邻块的重叠 tokens
    "separators": [             # 优先按以下分隔符切分
        "\n\n",                 # 段落间
        "\n",                   # 行间
        "。",                   # 中文句号
        ".",                    # 英文句号
        " ",                    # 空格
    ],
    "add_start_index": True,    # 记录块在原文中的位置
}
```

### 3.2 特殊内容处理

| 内容类型 | 处理方式 |

| --- | --- |

| 表格 | 转换为 `Markdown` 表格格式后整块存储，不拆分 |

| 代码块 | 保持完整，不按 token 数强制截断 |

| 图片（PDF 内嵌） | 提取为独立图片文件，生成独立 `Embedding` |

| 页眉 / 页脚 | 正则过滤，不进入分块 |

| 超长段落（> 1024 tokens） | 强制按 512 token 滑动窗口拆分 |

### 3.3 Chunk 元数据结构

```
{
  "chunk_id": "doc_001_chunk_003",
  "file_id": "doc_001",
  "content": "...",
  "start_index": 1024,
  "end_index": 1536,
  "chunk_type": "text",
  "page_number": 2,
  "metadata": {
    "source_file": "architecture_spec.pdf",
    "section_title": "系统架构设计",
    "has_table": false,
    "has_code": true
  }
}
```

---

## 四、Embedding 规范

### 4.1 文本 `Embedding`（`BGE-M3`）

```
TEXT_EMBEDDING_CONFIG = {
    "model": "BAAI/bge-m3",
    "device": "cuda",           # GPU 推理，CPU 备选
    "batch_size": 32,
    "max_length": 512,
    "output": {
        "dense_vector": True,   # 1024 维稠密向量
        "sparse_vector": True,  # BM25 稀疏权重
        "colbert_vector": False # 多粒度向量（可选，资源较重）
    }
}
```

### 4.2 图片 `Embedding`（`OpenCLIP`）

```
IMAGE_EMBEDDING_CONFIG = {
    "model": "ViT-B-32",
    "pretrained": "openai",
    "device": "cuda",
    "output_dim": 512,
    "normalize": True
}
```

### 4.3 向量存储 Collection 设计

```
# 文本 Collection
TEXT_COLLECTION = {
    "name": "text_chunks",
    "vectors": {
        "dense": {"size": 1024, "distance": "Cosine"},
        "sparse": {"index": {"type": "sparse"}}
    }
}

# 图片 Collection
IMAGE_COLLECTION = {
    "name": "image_chunks",
    "vectors": {
        "image": {"size": 512, "distance": "Cosine"}
    }
}
```

---

## 五、知识图谱抽取规范

### 5.1 `LangExtract` 配置

```
KG_EXTRACTION_CONFIG = {
    "llm": "deepseek-chat",
    "entity_types": [
        "PERSON",       # 人名
        "ORG",          # 组织机构
        "TECH",         # 技术术语
        "CONCEPT",      # 抽象概念
        "PRODUCT",      # 产品名称
    ],
    "relation_types": [
        "USES",         # A 使用 B
        "PART_OF",      # A 属于 B
        "RELATED_TO",   # A 与 B 相关
        "IMPLEMENTS",   # A 实现 B
        "DEPENDS_ON",   # A 依赖 B
    ],
    "max_entities_per_chunk": 10,
    "min_confidence": 0.7
}
```

### 5.2 节点数据结构

```
{
  "node_id": "entity_tech_qdrant_001",
  "label": "TECH",
  "name": "Qdrant",
  "properties": {
    "description": "向量数据库，支持多向量存储和元数据过滤",
    "source_chunk_ids": ["doc_001_chunk_003", "doc_001_chunk_007"],
    "confidence": 0.95
  }
}
```

### 5.3 边数据结构

```
{
  "edge_id": "rel_001",
  "source_node": "entity_tech_langchain_001",
  "target_node": "entity_tech_qdrant_001",
  "relation": "USES",
  "properties": {
    "context": "LangChain 通过 QdrantVectorStore 接入 Qdrant",
    "source_chunk_id": "doc_001_chunk_005",
    "confidence": 0.88
  }
}
```

---

## 六、状态机设计

`Pipeline` 每次处理任务都绑定一个状态机：

```
PENDING
    │
    ▼ (任务开始)
PARSING          ← ManaU 解析中
    │
    ▼ (解析完成)
CHUNKING         ← 文本分块中
    │
    ▼ (分块完成)
EMBEDDING        ← 向量化中
    │
    ▼ (向量化完成)
KG_EXTRACTING    ← 知识图谱抽取中
    │
    ▼ (抽取完成)
INDEXING         ← 写入存储中
    │
    ▼ (写入完成)
COMPLETED        ← 处理成功
    │
    ▼ (任意步骤报错)
FAILED           ← 处理失败，记录错误信息
```

**幂等性保证**：同一个文件（相同 `MD5`）重复上传不重新处理，直接返回已有结果。

---

## 七、错误处理

| 错误类型 | 处理策略 |

| --- | --- |

| 解析失败（PDF 损坏、格式不支持） | 标记 `FAILED`，返回具体错误原因 |

| `Embedding` 超时 | 自动重试 3 次，指数退避，超过后标记 `FAILED` |

| `KG` 抽取失败 | 降级处理：跳过 `KG` 抽取，仅保存 `Embedding`，标记为 `PARTIAL` |

| 存储写入失败 | 事务回滚，清理已写入数据，标记 `FAILED` |

| 文件过大 | 在接入层拒绝，返回 413 错误，不进入处理流程 |

---

## 八、与 `AgenticRAG` 规范的接口

`Bridge Pipeline` 处理完成后，`AgenticRAG` 通过以下接口读取数据：

```
# 文本检索接口（由 AgenticRAG 调用）
def search_text(query: str, top_k: int = 10, filters: dict = None) -> List[Chunk]

# 图片检索接口
def search_image(query_image: bytes, top_k: int = 5) -> List[ImageChunk]

# 图谱查询接口
def query_graph(entity_name: str, depth: int = 2) -> SubGraph

# 文件信息接口
def get_document_status(file_id: str) -> DocumentStatus
```

**约定**：`AgenticRAG` 只通过上述接口访问数据，不直接操作存储层。
