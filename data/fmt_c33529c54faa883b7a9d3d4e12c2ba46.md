
# 多模态 RAG 后端系统架构 v1.0

> 文档编号：04-后端系统架构-v1.0 | 版本：v1.0 | 更新日期：2026-04  
> 来源：赋范大模型技术社区 · 多模态 RAG 项目实战课  
> 原文链接：`https://kq4b3vgg5b.feishu.cn/docx/WNbIdgf4No3FFpx5ueycQjwWnhh`  
>
> 适用场景：企业内部知识库 / 客服智能问答 / 技术文档检索 / 行业资料分析  
> 项目阶段：后端架构设计阶段（前端尚未接入）

---

## 一、架构设计原则

这套架构是用 Plan 模式 + Claude Code 输出的。设计时遵循以下原则：

1. **MVP 先行**：存储层用纯文件系统 JSON 验证链路，不一开始就上 PostgreSQL + Qdrant  
1. **四层职责分离**：接入层不做业务逻辑，执行层不做参数校验，职责边界清晰  
1. **文档驱动**：每个层的输入输出在这份文档里单独定义，多会话 AI 协作时直接引用，不重复解释  
1. **状态机统一**：所有异步任务通过同一个状态机管理，不为每个接口写独立的状态逻辑  

---

## 二、四层架构总览

```
┌──────────────────────────────────────────────────┐
│                    接入层 (API Layer)              │
│  FastAPI  │  参数校验 (Pydantic)  │  统一错误处理  │
│  JWT 认证  │  请求限流  │  OpenAPI 文档自动生成    │
└────────────────────────┬─────────────────────────┘
                         │ 校验后的请求
┌────────────────────────▼─────────────────────────┐
│                   服务层 (Service Layer)            │
│  文件管理服务  │  Pipeline 调度服务  │  QA 问答服务  │
│  图谱管理服务  │  任务状态服务  │  历史会话服务     │
└──────────┬──────────────┬────────────────────────┘
           │              │
┌──────────▼──────┐  ┌───▼────────────────────────┐
│  执行层          │  │  执行层（Agent 侧）          │
│  (Pipeline 侧)  │  │                             │
│  ManaU 解析     │  │  LangChain Agent            │
│  文本分块        │  │  Tool: vector_search_text   │
│  BGE-M3 Embed  │  │  Tool: graph_search         │
│  OpenCLIP Embd │  │  Tool: hybrid_rerank        │
│  LangExtract KG │  │  DeepSeek-V3 生成           │
└──────────┬──────┘  └───┬────────────────────────┘
           │              │
┌──────────▼──────────────▼─────────────────────────┐
│                  存储层 (Storage Layer)              │
│  文件存储（本地 FS / MinIO）                         │
│  向量存储（JSON File / Qdrant）                      │
│  图谱存储（networkX / Neo4j）                        │
│  元数据存储（JSON File / PostgreSQL）                │
│  状态机（内存 Dict / Redis）                         │
└───────────────────────────────────────────────────┘
```

---

## 三、接入层规范

### 3.1 职责边界

**接入层做**：

+ 路由注册（`FastAPI` `Router`）  
+ 请求参数校验（`Pydantic` `BaseModel`）  
+ 认证鉴权（JWT Bearer Token）  
+ 请求日志记录  
+ 统一错误格式转换  

**接入层不做**：

+ 业务逻辑  
+ 数据库操作  
+ 调用外部服务  

### 3.2 统一返回格式

所有接口返回统一格式：

```
class APIResponse(BaseModel):
    code: int           # 0 = 成功，非 0 = 错误
    message: str        # 人类可读的状态描述
    data: Optional[Any] # 返回数据，错误时为 None
    request_id: str     # 请求 ID，用于日志追踪

# 成功示例
{
    "code": 0,
    "message": "success",
    "data": {...},
    "request_id": "req_1714567890_abc123"
}

# 错误示例
{
    "code": 4001,
    "message": "文件格式不支持，仅支持 .pdf / .jpg / .png / .txt",
    "data": null,
    "request_id": "req_1714567890_def456"
}
```

### 3.3 错误码定义

| 错误码 | 含义 |
| --- | --- |
| 0 | 成功 |
| 4001 | 参数校验失败 |
| 4002 | 文件格式不支持 |
| 4003 | 文件过大 |
| 4004 | 资源不存在 |
| 4005 | 重复文件（MD5 已存在） |
| 5001 | Pipeline 处理失败 |
| 5002 | Agent 推理失败 |
| 5003 | 存储层写入失败 |
| 5004 | 外部服务不可用 |

---

## 四、服务层规范

### 4.1 服务模块划分

```
services/
├── file_service.py        # 文件上传、存储、查询、删除
├── pipeline_service.py    # Bridge Pipeline 调度、任务管理
├── qa_service.py          # QA 问答、历史会话管理
└── graph_service.py       # 知识图谱查询、导出
```

### 4.2 服务间依赖规则

+ 服务之间不直接调用彼此的函数，通过共享存储层交换状态  
+ 服务不直接实例化执行层组件，通过依赖注入获取  
+ 每个服务对应一个 `FastAPI` `Router`，一对一关系  

### 4.3 Pipeline 调度服务状态机

```
class TaskStatus(str, Enum):
    PENDING = "pending"
    PARSING = "parsing"
    CHUNKING = "chunking"
    EMBEDDING = "embedding"
    KG_EXTRACTING = "kg_extracting"
    INDEXING = "indexing"
    COMPLETED = "completed"
    FAILED = "failed"
    CANCELLED = "cancelled"

class Task(BaseModel):
    task_id: str
    file_id: str
    status: TaskStatus
    progress: float         # 0.0 - 1.0
    created_at: datetime
    updated_at: datetime
    error_message: Optional[str]
    result: Optional[dict]
```

---

## 五、执行层规范

### 5.1 Pipeline 执行层

```
core/pipeline/
├── file_parser.py          # ManaU 封装，输入文件路径，输出解析结果
├── text_chunker.py         # 文本分块，按规范配置（512 tokens，64 overlap）
├── text_embedder.py        # BGE-M3 封装
├── image_embedder.py       # OpenCLIP 封装
├── kg_extractor.py         # LangExtract 封装
└── pipeline_runner.py      # 编排以上组件，驱动状态机流转
```

### 5.2 Agent 执行层

```
core/agent/
├── tools/
│   ├── vector_search.py    # vector_search_text + vector_search_image
│   ├── graph_search.py     # graph_search + graph_entity_lookup
│   └── reranker.py         # hybrid_rerank
├── prompts.py              # System Prompt + User Prompt 模板
├── agent_runner.py         # createAgent 初始化 + invoke 封装
└── response_parser.py      # LLM 输出解析 → 结构化 QAResponse
```

### 5.3 执行层对外接口

执行层只暴露两个入口，服务层通过这两个入口调用：

```
# Pipeline 入口
async def run_pipeline(file_path: str, file_id: str) -> PipelineResult

# Agent 入口
async def run_agent_qa(query: str, session_id: str) -> QAResponse
```

---

## 六、存储层规范

### 6.1 MVP 阶段（文件系统）

```
storage/
├── files/                  # 原始上传文件
│   └── {file_id}/
│       ├── original.pdf    # 原始文件
│       └── metadata.json   # 文件元数据
├── chunks/                 # 文本块
│   └── {file_id}/
│       └── chunks.json     # 所有分块
├── embeddings/             # 向量（JSON 格式，生产替换为 Qdrant）
│   └── {file_id}/
│       └── embeddings.json
├── graph/                  # 知识图谱（networkX 序列化）
│   └── graph.gpickle
└── tasks/                  # 任务状态
    └── {task_id}.json
```

### 6.2 生产阶段替换路径

| MVP 存储 | 生产存储 | 替换影响范围 |
| --- | --- | --- |
| 本地文件系统 | MinIO | `file_service.py` |
| JSON 向量文件 | Qdrant | `text_embedder.py` + `image_embedder.py` |
| networkX pickle | Neo4j | `kg_extractor.py` + `graph_search.py` |
| JSON 元数据文件 | PostgreSQL | `file_service.py` + `pipeline_service.py` |
| 内存状态机 | Redis | `pipeline_service.py` |

**替换原则**：存储层替换只改对应的封装文件，不影响服务层和执行层代码。

---

## 七、目录结构

```
multimodal-rag-backend/
├── app/
│   ├── api/                    # 接入层
│   │   ├── routes/
│   │   │   ├── documents.py    # 文档管理路由
│   │   │   ├── tasks.py        # 任务管理路由
│   │   │   ├── graph.py        # 图谱管理路由
│   │   │   └── qa.py           # QA 问答路由
│   │   └── middleware.py       # 认证、日志中间件
│   ├── services/               # 服务层
│   │   ├── file_service.py
│   │   ├── pipeline_service.py
│   │   ├── qa_service.py
│   │   └── graph_service.py
│   ├── core/                   # 执行层
│   │   ├── pipeline/
│   │   └── agent/
│   ├── storage/                # 存储层抽象
│   │   ├── interfaces.py       # 存储接口定义
│   │   └── implementations/   # 具体实现（本地 / Qdrant / Neo4j）
│   ├── models/                 # 数据模型（`Pydantic`）
│   └── config.py               # 配置（从环境变量读取）
├── tests/
│   ├── unit/
│   └── integration/
├── docs/                       # 规范文档（即本包）
├── docker-compose.yml
└── requirements.txt
```

---

## 八、关键配置

```
# config.py
class Settings(BaseSettings):
    # LLM
    LLM_API_KEY: str
    LLM_BASE_URL: str = "https://api.deepseek.com"
    LLM_MODEL: str = "deepseek-chat"

    # Embedding
    TEXT_EMBEDDING_MODEL: str = "BAAI/bge-m3"
    IMAGE_EMBEDDING_MODEL: str = "ViT-B-32"

    # Storage（MVP 用本地路径，生产替换为服务地址）
    STORAGE_TYPE: str = "local"   # local / qdrant / neo4j
    STORAGE_BASE_PATH: str = "./storage"
    QDRANT_URL: Optional[str] = None
    NEO4J_URI: Optional[str] = None

    # Pipeline
    MAX_FILE_SIZE_MB: int = 50
    CHUNK_SIZE: int = 512
    CHUNK_OVERLAP: int = 64

    class Config:
        env_file = ".env"
```
