# 01-技术选型清单

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/UPR3dFXKpo7JZsxZJm5cW8vynac](https://kq4b3vgg5b.feishu.cn/docx/UPR3dFXKpo7JZsxZJm5cW8vynac)

# 多模态 RAG · 技术选型清单

> 适用场景：企业内部知识库 / 客服智能问答 / 技术文档检索 / 行业资料分析

> 项目阶段：技术调研阶段

> 版本号：v1.0

> 更新日期：2026-04

> 来源：赋范大模型技术社区 · 多模态 RAG 项目实战课

---

## 选型方法说明

这份清单不是"看到别人在用就选"出来的。每个技术选项都经过以下流程：

1.  Deep Research（AI 深度调研）：让模型基于系统需求生成候选方案对比

1.  官方文档核验：通过 MCP 连接官方文档确认最新特性和接入方式

1.  MVP 验证：选型结论在实际项目里跑通后才写入清单

---

## 一、后端框架

**选定：FastAPI**

| 候选 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- |
| FastAPI | 自动生成 OpenAPI 文档、异步原生支持、Python 生态最成熟 | 相比 Django 生态较轻 | ✅ 选用 |
| Flask | 简单灵活 | 不支持原生异步、无自动文档 | ❌ 不选 |
| Django REST Framework | 生态完整 | 过重，ORM 绑定深 | ❌ 不选 |

**选型理由**：RAG 后端核心是 I/O 密集型（向量检索 + LLM 调用），FastAPI 的异步支持和自动 OpenAPI 文档是关键优势。LangChain Agent 接入也有现成的 FastAPI 集成方案。

---

## 二、文档解析层

**选定：ManaU（PDF 解析） +** LangExtract（知识图谱抽取）

### 2.1 PDF / 图片解析

| 候选 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- |
| ManaU | 多模态原生支持（文本 + 图片 + 表格）、保留版式结构 | 依赖较重 | ✅ 选用 |
| PyMuPDF | 轻量、速度快 | 图片处理能力弱 | 备选（纯文本场景） |
| Unstructured | 格式支持全 | 对中文 PDF 效果不稳定 | ❌ 不选 |
| VLM 直接解析 | 不需要专门解析库 | 成本高、速度慢、结构化输出不稳定 | ❌ 不选 |

### 2.2 知识图谱抽取

| 候选 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- |
| LangExtract | 与 LangChain 生态集成好、中文支持稳定 | 社区相对小 | ✅ 选用 |
| LlamaIndex KG | 功能完整 | 与本项目 LangChain 栈有冲突 | ❌ 不选 |
| 自研 NER + 关系抽取 | 完全定制 | 开发成本高 | ❌ 不选 |

---

## 三、向量存储

选定：Qdrant（生产）/ 纯文件 JSON（MVP 验证阶段）

| 候选 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- |
| Qdrant | 原生多向量支持、Python SDK 完整、元数据过滤强 | 需要独立部署 | ✅ 选用（生产） |
| Milvus | 高并发性能强 | 部署复杂、资源占用大 | 备选（高并发场景） |
| Chroma | 本地开发友好 | 不适合生产规模 | ❌ 不选 |
| Pinecone | 托管省心 | 国内访问不稳定、数据出境合规问题 | ❌ 不选 |

**MVP 阶段说明**：架构设计验证阶段使用纯文件系统 JSON 存储，跑通链路后再替换为 Qdrant，避免过早引入基础设施复杂度。

---

## 四、知识图谱存储

选定：networkX（MVP 验证）/ Neo4j（生产）

| 候选 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- |
| networkX | Python 原生、无需额外服务、适合验证 | 不支持持久化、不适合大规模 | ✅ 选用（MVP） |
| Neo4j | 专业图数据库、Cypher 查询强 | 需要独立部署、学习曲线 | ✅ 选用（生产） |
| TigerGraph | 性能强 | 企业收费、复杂 | ❌ 不选 |

---

## 五、Embedding 模型

选定：BGE-M3（文本） + OpenCLIP（图片跨模态）

### 5.1 文本 Embedding

| 候选 | 维度 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- | --- |
| BGE-M3 | 1024 | 中英双语强、支持稠密+稀疏+多粒度检索 | 模型体积大 | ✅ 选用 |
| text-embedding-3-small | 1536 | 简单易用 | 需要调 OpenAI API，成本高 | ❌ 不选 |
| bge-large-zh | 1024 | 中文强 | 不支持多模态扩展 | 备选 |

### 5.2 图片跨模态 Embedding

| 候选 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- |
| OpenCLIP | 开源、支持文字-图片对齐、模型选择多 | 对中文图片支持有限 | ✅ 选用 |
| CLIP (OpenAI) | 原版效果好 | 闭源 API | ❌ 不选 |
| Chinese-CLIP | 中文优化 | 生态不如 OpenCLIP 完整 | 备选（中文图片为主场景） |

---

## 六、Agent 框架

选定：LangChain 1.0+

| 候选 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- |
| LangChain | 生态最大、工具链最全、有 MCP Server 可接入最新规范 | 版本迭代快、历史文档混乱 | ✅ 选用 |
| LlamaIndex | 检索层能力强 | Agent 功能弱于 LangChain | 备选（纯检索场景） |
| AutoGen | 多 Agent 协作强 | 对话式场景偏重，不适合接口服务化 | ❌ 不选 |

**接入说明**：LangChain 版本更新快，不要凭记忆写代码。用 LangChain 官方 MCP Server 连接最新文档，让 AI 基于规范写接入代码。

---

## 七、大模型

选定：DeepSeek-V3（通过 ChatOpenAI 适配器接入）

| 候选 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- |
| DeepSeek-V3 | 性价比最高、中文强、API 兼容 OpenAI 格式 | 上下文窗口有限 | ✅ 选用（主力） |
| GPT-4o | 综合能力强 | 成本高、国内访问不稳定 | 备选 |
| Claude 3.5 Sonnet | 长文本 + 推理强 | 价格中等 | 备选（复杂推理场景） |
| Qwen-Max | 中文优化好 | 多模态能力弱于 DeepSeek | 备选 |

**适配说明**：DeepSeek 提供 OpenAI 兼容接口，LangChain 里用 ChatOpenAI(base_url=..., model="deepseek-chat") 直接接入，无需额外适配层。

---

## 八、基础设施

| 组件 | 选型 | 说明 |
| --- | --- | --- |
| 文件存储 | 本地文件系统（MVP）/ MinIO（生产） | MVP 阶段不引入 OSS 复杂度 |
| 元数据存储 | JSON 文件（MVP）/ PostgreSQL（生产） | 同上 |
| 任务队列 | 内存状态机（MVP）/ Celery + Redis（生产） | 异步任务管理 |
| API 文档 | FastAPI 自动生成 Swagger UI | 无需额外配置 |
| 容器化 | Docker Compose | 本地开发 + 简单部署 |

---

## 选型原则总结

1.  **MVP 阶段轻量优先**：验证链路比选最优工具更重要，先跑通再替换

1.  **用 MCP 拿最新规范**：LangChain 这类更新快的框架，不凭记忆写代码

1.  **国内可用性优先**：向量数据库选 Qdrant（自部署），LLM 选 DeepSeek

1.  **Python 生态优先**：后端全用 Python，减少跨语言调用复杂度