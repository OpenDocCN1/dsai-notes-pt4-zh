
# 多模态RAG智能问答演示

## 直接问答

回答：这个户型有三个卧室，分别是主卧、次卧和儿童房。

信息来源：文档 1、文档 2、文档 3。

```
result2 = qa_system.ask("找一下有没有3室2厅的户型？")

print(f"\n问题：找一下有没有3室2厅的户型？")

print(f"回答：{result2['message']}")
```

## LLM智能检索模式

   查询: 找一下有没有3室2厅的户型？

============================================================

执行向量检索: 找一下有没有3室2厅的户型？...

Number of requested results 10 is greater than number of elements in index 3, updating n_results = 3

✓ 找到 3 个相关结果

正在生成智能检索结果...

✓ 找到 3 个相关户型

============================================================

问题：找一下有没有3室2厅的户型？

回答：感谢您的查询！根据您的需求，我们找到了三个符合条件的3室2厅的户型。以下是每个户型的简要介绍：

1. **户型一**

   - **面积**: 总面积为119平方米。

   - **特点**: 该户型布局紧凑，动线合理，包含客厅、主卧、次卧、儿童房、厨房、主卫、次卫和阳台等功能区。主卧带有独立卫生间，客厅与餐厅相连，采光良好。

2. **户型二**

   - **面积**: 总面积为119平方米。

   - **特点**: 该户型设计为分隔式布局，包含客厅、主卧、次卧、儿童房、厨房、主卫、次卫、餐厅和入户玄关。主卧同样带有独立卫生间，客餐厅一体化设计，动线流畅。

3. **户型三**

   - **面积**: 总面积为119平方米。

   - **特点**: 该户型为开放式布局，客厅和餐厅连通，动线流畅。包含客厅、主卧、次卧、儿童房、厨房、主卫、次卫和入户玄关。主卧带有独立卫生间，采光良好。

**推荐意见**:

根据您的需求，这三个户型都符合3室2厅的标准，并且每个户型都有其独特的设计特点。若您偏好动线流畅且采光良好的设计，户型三可能是一个不错的选择。若您更注重客餐厅一体化设计，户型二则可能更适合您。希望这些信息能帮助您做出更好的选择！

如需进一步了解或预约看房，请随时与我们联系。我们很乐意为您提供更多帮助！

## 应用场景与问答模式

根据实际的应用场景，我们实现了两种互补的问答模式：

+   **直接问答**：适合需要快速获取精确信息的场景（如查询材料、尺寸等）

+   **图像检索**：适合需要浏览和对比多个图纸的场景

# 六、企业项目实战：多模态RAG项目本地部署

![](img/de882ff1d78cb5fe84ac6f66c49ea3d8.png)

本节内容，我们将详细介绍如何部署和运行这个基于VLM的多模态RAG智能问答系统。该系统支持CAD图纸、平面图、架构图、PDF文档等多种格式的智能分析和问答。

## 6.1 项目结构详解

项目采用模块化设计，核心结构如下：

```
pc_multimodal_rag/                    # 项目根目录
├── 📁 backend/                       # 后端服务层
│   ├── main_service.py              # FastAPI主服务 - 多模态RAG API
│   ├── simple_vlm_analyzer.py       # VLM图像分析器 (支持CAD/平面图/架构图)
│   ├── qwen_embeddings.py           # 通义千问Embedding模型封装
│   ├── simple_logger.py             # 日志记录模块
│   ├── 📁 unified/                   # 统一PDF处理模块
│   │   └── unified_pdf_extraction_service.py  # PDF解析服务
│   ├── 📁 Information-Extraction/    # 信息提取模块
│   ├── 📁 image_analysis/            # 图像分析模块
│   └── 📁 chroma_db/                 # ChromaDB向量数据库存储
│
├── 📁 frontend/                      # 前端界面 (可选)
├── 📁 uploads/                       # 上传文件存储目录
├── 📁 previews/                      # 文件预览/缩略图存储
├── 📁 test_data/                     # 测试数据
└── .env                              # 环境配置文件
```

核心组件功能说明

| 层级 | 技术栈 | 主要功能 | 关键文件 |
| --- | --- | --- | --- |
| **API服务层** | FastAPI + Pydantic | RESTful API、文件上传、智能问答 | `main_service.py` |
| **VLM分析层** | 多模态大模型 + 自定义提示词 | 多模态理解、图像分析、结构化提取 | `simple_vlm_analyzer.py` |
| **向量检索层** | ChromaDB + Qwen/HuggingFace Embeddings | 语义检索、相似度计算 | `qwen_embeddings.py` + ChromaDB |
| **文档处理层** | PyMuPDF + PIL + 自定义解析器 | PDF解析、图像预处理、格式转换 | `unified_pdf_extraction_service.py` |
| **数据存储层** | 文件系统 + 向量数据库 | 原文件存储、向量索引、元数据管理 | `uploads/` + `chroma_db/` |

![](img/c704bd43744442c9dc5c46af4f416ec0.png)

![](img/ce5a719281bd6c57892d7803790a6b1d.png)

## 6.2 环境要求与依赖安装

系统基于Python 3.11+开发，需要确保环境满足以下要求：

环境要求

| 组件 | 版本要求 | 安装方式 | 验证命令 |
| --- | --- | --- | --- |
| **Python** | ≥ 3.10 | 官网下载或conda | `python --version` |
| **pip** | 最新版 | 随Python安装 | `pip --version` |

首先需要创建Python虚拟环境

```
# 使用conda创建环境（推荐）

conda create -n multimodal_rag python=3.11

conda activate multimodal_rag

# 或使用venv创建环境

python -m venv multimodal_rag

source multimodal_rag/bin/activate  # Linux/Mac

# multimodal_rag\Scripts\activate     # Windows
```

接下来一键安装核心依赖

```
# 进入项目目录

cd pc_multimodal_rag/backend

# 安装核心依赖包

pip install -r requirements_service.txt
```

![](img/3a35cdd80208d82ab43d4c828e8dad3e.png)

## 6.3 后端服务配置与启动

完成依赖安装后，需要配置API密钥和启动后端服务。

创建 `.env` 文件，配置必要的API密钥：

```
# 在项目根目录创建 .env 文件

touch .env
```

在 `.env` 文件中添加以下配置：

```
# 多模态 RAG 服务配置

# VLM 模型配置

VLM_MODEL_URL=https:/

VLM_API_KEY=sk-Y4o8DF6Iq2l8nFT

VLM_MODEL_NAME=gpt-4o

# 服务配置

SERVICE_HOST=0.0.0.0

SERVICE_PORT=8000

# 存储配置

UPLOAD_DIR=./uploads

PREVIEW_DIR=./previews

VECTOR_DB_DIR=./chroma_db

# Embedding 模型配置

EMBEDDING_TYPE=qwen  # qwen 或 huggingface

EMBEDDING_MODEL=text-embedding-v4

EMBEDDING_DIMENSIONS=1024

DASHSCOPE_API_KEY=sk-bdccf7277a5

DASHSCOPE_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1

# 文本分割配置

CHUNK_SIZE=800

CHUNK_OVERLAP=100
```

重要提示：

+   `OPENAI_API_KEY`：必须配置，用于调用gpt-4o模型进行图像分析

+   `DASHSCOPE_API_KEY`：必须配置，用于调用Qwen embedding模型

+   如果使用HuggingFace模型，可以不配置`DASHSCOPE_API_KEY`

![](img/14fc215e75ad37f7acb5b192bdae600a.png)

核心组件功能说明

| 接口路径 | 方法 | 功能 | 说明 |
| --- | --- | --- | --- |
| `/upload/` | POST | 文件上传 | 支持图片、PDF等多种格式 |
| `/search/` | POST | 智能搜索 | 基于向量检索的语义搜索 |
| `/intelligent_qa/` | POST | 智能问答 | 多模态问答，支持直接回答和图像检索 |
| `/files/` | GET | 文件列表 | 获取已上传的文件列表 |

接下来启动后端服务

```
# 启动FastAPI后端服务

python backend/main_service.py
```

启动成功后，终端会显示如下信息：

```
INFO:     Started server process [12345]  

INFO:     Waiting for application startup.

INFO:     Application startup complete.

INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

![](img/2f87c2c2f32e942ddc1efa7f4f35dffc.png)

## 6.4 前端服务配置与启动

最后，启动前端服务，进入前端目录，安装Node.js依赖，启动开发服务器：

```
cd frontend

npm install
```

![](img/d760c88f87dd80509339594eb825a2fb.png)

```
npm run dev
```

![](img/f808cc7840744277d30ea650fa4c1681.png)

打开浏览器访问 http://localhost:5173，就可以看到"赋范空间公开体验课"的界面。

![](img/c431f790826800a8bca616cd3172fe80.png)

至此，多模态RAG系统部署完成！系统支持CAD图纸、平面图、PDF文档的智能分析和问答，可根据实际需求进行功能扩展和定制开发。

我们下期公开课，再见！
