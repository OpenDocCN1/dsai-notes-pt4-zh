
# Part 4.多模态RAG项目开发实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Vg6ndBDyBoTZ5CxG5c4ci6ZrnVc](https://kq4b3vgg5b.feishu.cn/docx/Vg6ndBDyBoTZ5CxG5c4ci6ZrnVc)

**课程说明：**

+ 体验课内容节选自《2025大模型Agent智能体开发实战》(秋招冲刺班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(秋招冲刺班)

![](img/b80c9eb350fde747bbba79308df5c1d8.png)

![](img/34077f968985e95b919e9d2e4bb53f71.png)

公开课全套学习资料，已上传至网盘（https://pan.baidu.com/s/1vV-8ew5tAfZOfwG3c4WAFw 提取码: i345）需要更系统深入学习大模型vx可扫码⬆️添加助教咨询喔～

---

+ 本期公开课四大模块内容

![](img/aa8844511ad7e04af2b74c888e3a205d.png)

+ 【演示】实操项目一：从零到一快速搭建多模态RAG系统

+ 【演示】实操项目二：企业级多模态RAG系统开发实战

## 阶段一：多模态RAG项目需求描述、技术栈规划与接口设计

### 1. 项目背景与需求

本项目是一个 RAG（Retrieval-Augmented Generation，检索增强生成）系统，目标是帮助学习者理解 RAG 系统的完整开发流程。

我们希望实现以下功能：

1. **PDF 文档处理**

+ 用户上传 PDF 文件；
+ 后端完成 OCR、版面解析、Markdown 转换；
+ 提供文档解析状态查询与预览。

1. **索引构建**

+ 将解析得到的 Markdown 文档进行切分（`chunking`）；
+ 使用 Embedding 模型（`OpenAI Embeddings`）将片段向量化；
+ 保存至 `FAISS` 向量数据库，用于后续检索。

1. **对话问答（RAG）**

+ 用户输入问题，系统先在向量数据库中检索相关片段；
+ 将检索结果与用户问题一起交给 LLM 生成答案；
+ 答案中附带引用（`citations`），方便追溯来源。
+ 支持**流式输出**（`SSE`），提升交互体验。
+ 支持**历史对话记忆**（基于 `InMemorySaver`），并提供清空功能。

1. **健壮性需求**

+ 系统在没有上传文档时也能正常回答（走模型自带知识）；
+ 所有接口有清晰的 API 约定，方便前端对接。
+ 效果展示

![](img/ffdf00f450879b27ee80e3a1e50ff4fe.png)

+ 完整源码领取：

![](img/8649d3eb91d40b8ffcc14db6d2df0d7b.png)

### 2. 技术栈规划

#### 2.1 后端技术

+ `FastAPI`：高性能 Python Web 框架，自动生成 `Swagger UI`。
+ `Uvicorn`：ASGI 服务器，支持异步处理。
+ `LangChain` / `LangGraph`：RAG 框架，管理对话状态与检索逻辑。
+ `Unstructured` / `fitz` (`PyMuPDF`)：PDF 解析、OCR、图片处理。
+ `FAISS`：Facebook 开源的向量数据库，用于相似检索。

#### 2.2 前端技术

+ `Figma`：快速完成 UI 原型设计；
+ `React` / `Next.js`：实现流式 SSE 接口调用、前端展示。

#### 2.3 AI 模型与 API

+ `OpenAI Embeddings`：用于向量化（`text-embedding-3-small` / `large`）；
+ 对话模型（LLM）：支持通用对话（如 `DeepSeek-Chat` / `OpenAI GPT-4`）。

#### 2.4 环境与依赖

+ Python >= 3.9
+ 主要依赖：`fastapi`、`uvicorn`、`python-multipart`、`langchain`、`faiss-cpu`、`unstructured`、`pymupdf`、`paddleocr`

### 3. 接口规划

我们采用 RESTful API 风格，分为 4 大模块：

1. **健康检查（Health）**

+ `/health`：确认服务正常运行。

1. PDF 处理（PDF Service）

+ `/pdf/upload`：上传 PDF 文件；
+ `/pdf/parse`：触发解析任务；
+ `/pdf/status`：查询解析进度；
+ `/pdf/page`：获取 PDF 页图（原始/解析）；
+ `/pdf/chunk`：根据 `citationId` 获取片段。

1. 索引构建（Index Service）

+ `/index/build`：构建向量索引；
+ `/index/search`：检索相似片段。

1. 对话（Chat Service）

+ `/chat`：RAG 聊天（SSE 流式输出，包含 `citations`）；
+ `/chat/clear`：清空当前会话历史。

### 4. OpenAPI 文件编写

基于以上规划，我们使用 OpenAPI 3.1.0 来定义后端接口。

> OpenAPI 的作用：

+ 是前后端沟通的“契约”；
+ 能被 `FastAPI` 自动识别并生成交互式文档；
+ 可生成前端 SDK / TS 类型，避免手写错误。

下面是一份完整的 `openapi.yaml`（已简化描述，完整版本可参考项目仓库）：

```
openapi: 3.1.0
info:
  title: RAG Demo API
  version: 1.0.0
servers:
  - url: http://localhost:8001/api/v1

paths:
  /health:
    get:
      summary: 健康检查
      responses:
        "200":
          description: OK

  /pdf/upload:
    post:
      summary: 上传 PDF
      requestBody:
        required: true
        content:
          multipart/form-data:
            schema:
              type: object
              required: [file]
              properties:
                file: { type: string, format: binary }
      responses:
        "200":
          description: 上传成功

  /pdf/parse:
    post:
      summary: 触发解析
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                fileId: { type: string }
      responses:
        "202": { description: 已接受 }

  /pdf/status:
    get:
      summary: 查询解析状态
      parameters:
        - in: query
          name: fileId
          required: true
          schema: { type: string }
      responses:
        "200": { description: 状态返回 }

  /index/build:
    post:
      summary: 构建索引
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                fileId: { type: string }
      responses:
        "200": { description: 构建完成 }

  /chat:
    post:
      summary: 聊天接口（SSE）
      description: 返回流式回答 + 引用
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                message: { type: string }
                fileId: { type: string }
      responses:
        "200": { description: text/event-stream }

  /chat/clear:
    post:
      summary: 清空会话
      responses:
        "200": { description: 会话已清空 }
```

![](img/c69784c67d1a346df89d1ce5b9787366.png)

在开发的第一部分，我们完成了：

1. 明确了系统需求：PDF → 索引 → RAG 问答；
1. 规划了技术栈：`FastAPI` + `LangChain` + `FAISS` + 前端 `React`；
1. 梳理了接口模块：PDF、索引、聊天；
1. 编写了 `openapi.yaml`，形成前后端开发的“契约”。

**下一步（Part 2）**：我们将基于 OpenAPI 文档，逐步实现 后端服务（`FastAPI` + `Mock` + 真正逻辑），并在 `Swagger UI` 中进行测试。

## 阶段二：后端功能思路规划与`Mock`功能实现

### 1. 项目结构与职责边界

**目标**：前后端“用接口说话”；后端内部“用服务分层说话”。

```
backend/
├─ app.py                        # 入口与路由（FastAPI）
├─ services/
│  ├─ pdf_service.py             # 上传/解析/页图/可视化
│  ├─ index_service.py           # 切分/向量化/索引/检索
│  └─ rag_service.py             # RAG 检索+生成流（SSE）与会话历史
├─ data/                         # 解析与索引产物（按 `fileId` 分目录）
│  └─ f_xxx/
│     ├─ original.pdf
│     ├─ output.md
│     ├─ pages/{original|parsed}/page-0001.png
│     └─ index_faiss/{index.faiss,index.pkl}
├─ .env
└─ requirements.txt
```

**为什么这么拆？**

+ `app.py` 专注“路由+协议”（HTTP/SSE），不掺业务细节。
+ `services/*` 可单独测试、复用、替换（比如换别的向量库/LLM）。
+ `data/<fileId>/…` 是 **教学友好** 的本地可观测产物（便于课堂讲解与排错）。

### 2. 从 `Mock` 到真实现：增量替换的策略

**先 `Mock`**：接口通、前端能调、边界清晰；

再替换：把 `Mock` 的“假数据/睡眠”替换成真实调用与产物。

> 这能最大化减少“前后端互相等待”的时间，也让你每步都有可验证成果。

#### 2.1 PDF 管线（`/pdf/*`）

**`Mock` 版思路**

+ `/pdf/upload` 返回一个临时 `fileId`（`UUID`），把文件暂存；
+ `/pdf/parse` 启动一个**后台任务**（不要阻塞 HTTP 响应），假装解析，写个“进度文件”；
+ `/pdf/status` 根据进度文件返回 parsing/ready；
+ `/pdf/page` 返回占位图/原图。

**逐步替换为真实现**

+ 解析任务里接入 `Unstructured` + `PaddleOCR` + `PyMuPDF`；
+ 导出 `output.md`、每页 `original.png`、`parsed.png`、抽取图片 `images/`；
+ 把 **页号、`bbox`、类别**等结构写入中间文件（便于 `/pdf/chunk`/可视化叠框）；
+ 进度更新：**按阶段写入**（20/50/80/100），让 `/status` 有“在动”的感觉；
+ 页图加载：`/pdf/page` 用 `FileResponse` 或 `StreamingResponse` 回图。

**关键示意**：

```
# app.py
@app.post("/api/v1/pdf/parse")
def start_parse(req: ParseBody, bg: BackgroundTasks):
    bg.add_task(run_full_parse_pipeline, req.fileId)
    return {"jobId": f"j_{shortid()}"}

# services/pdf_service.py
def run_full_parse_pipeline(file_id: str):
    write_status(file_id, "parsing", 10)
    # 1) OCR/结构解析 -> elements
    # 2) 导出 output.md、images/
    # 3) 渲染 original/parsed/page-*.png
    write_status(file_id, "ready", 100)
```

#### 2.2 索引管线（`/index/*`）

**`Mock` 版思路**

+ `/index/build` 直接 `time.sleep(1)` 返回 `"chunks": 42`；
+ `/index/search` 返回固定三条占位片段。

**真实现替换**

+ 构建阶段读取 `data/<fileId>/output.md`，做“按标题切分（Header1/2）→ 清洗 → 向量化（`OpenAI Embeddings`）→ `FAISS`”；
+ 检索阶段加载本地 `FAISS`，`similarity_search_with_score(query, k)`。

**关键示意**：

```
# services/index_service.py
def build_faiss_index(file_id: str):
    md = load_markdown(file_id)
    docs = header_split(md)               # MarkdownHeaderTextSplitter
    vs = FAISS.from_documents(docs, embedding=load_embeddings())
    vs.save_local(index_dir(file_id))
    return {"ok": True, "chunks": len(docs)}

def search_faiss(file_id: str, query: str, k=5):
    vs = FAISS.load_local(index_dir(file_id), load_embeddings(), allow_dangerous_deserialization=True)
    return vs.similarity_search_with_score(query, k=k)
```

**为什么先做“标题切分”？**

+ 讲解性强（学生容易理解“结构化”切分）；
+ 对课程 PPT/讲义类文档命中率高；
+ 之后再引入“递归字符切分”作对照，讨论召回/粒度权衡。

#### 2.3 RAG 聊天（`/chat` SSE）

**`Mock` 版思路**

+ 原来 `/chat` 返回“伪流”：`sleep` + 逐句 `token` 事件；
+ 先不发 `citation`，只发 `token`/`done`。

**真实现替换**

+ 收到 `message` + `pdfFileId` + `sessionId`；
+ 如果有 `pdfFileId` 且索引存在：先 `retrieve` → 生成 `citations`；
+ **先发若干 `event: citation`**（前端角标立刻出现）；
+ 组装历史 + 上下文 → 调 LLM 流式生成 → `event: token` 连发；
+ 结尾 `event: done` 带 `{"used_retrieval": true|false}`。
