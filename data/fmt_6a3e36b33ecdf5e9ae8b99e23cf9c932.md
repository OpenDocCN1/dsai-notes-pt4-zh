
# 第一节 图RAG系统架构与环境配置

> 来源：`https://gotoaiworld.feishu.cn/docx/WDLids6S5oxnaLxh4uAc7hPMn5f`

> 在前面章节的基础上，接下来构建一个更先进的图RAG系统。通过引入Neo4j图数据库和智能查询路由机制，实现真正的知识图谱增强检索，解决传统RAG在复杂查询和关系推理方面的局限性。

![](img/e9c37b84897634d949ea22ecd0604929.png)

## 一、项目背景与目标

### 1.1 从传统RAG到图RAG的演进

上一章中，我们构建了基于向量检索的传统RAG系统，采用了父子文本块的分块策略，能够有效回答简单的菜谱查询。但在处理复杂的关系推理和多跳查询时仍存在明显局限：

+ 关系理解缺失：虽然父子分块保持了文档结构，但无法显式建模食材、菜谱、烹饪方法之间的语义关系

+ 跨文档关联困难：难以发现不同菜谱之间的相似性、替代关系等隐含联系

+ 推理能力有限：缺乏基于知识图谱的多跳推理能力，难以回答需要复杂逻辑推理的问题

### 1.2 图RAG系统的核心优势

通过引入知识图谱，我们的新系统将具备：

+ 结构化知识表达：以图的形式显式编码实体间的语义关系

+ 增强推理能力：支持多跳推理和复杂关系查询

+ 智能查询路由：根据查询复杂度自动选择最适合的检索策略

+ 事实性与可解释性：基于图结构的推理路径提供可追溯的答案

## 二、环境配置

> 若需要进行外部访问，需更换本地或服务器环境

### 2.1 创建虚拟环境

```
# 使用conda创建环境
conda create -n graph-rag python=3.12.7
conda activate graph-rag
```

### 2.2 安装核心依赖

```
cd code/C9
pip install -r requirements.txt
```

### 2.3 Neo4j数据库配置

使用Docker Compose方式安装Neo4j，配置文件位于 `data/C9/docker-compose.yml`：

#### 2.3.1 启动Neo4j服务

```
# 进入docker-compose.yml所在目录
cd data/C9

# 启动Neo4j服务
docker-compose up -d

# 检查服务状态
docker-compose ps
```

#### 2.3.2 访问Neo4j Web界面

启动成功后，可以通过以下方式访问：

+ Web界面：`http://localhost:7474`

+ 用户名：`neo4j`

+ 密码：`all-in-rag`

> 当前网址为本地访问，如果你是部署在远程服务器上，需要将 `localhost` 修改为你的服务器IP地址。

#### 2.3.3 数据导入

Docker Compose配置中包含了自动数据导入功能。启动服务时会自动执行以下步骤：

1. 等待Neo4j服务就绪：通过健康检查确保数据库可用

1. 执行导入脚本：自动运行 `data/C9/cypher/neo4j_import.cypher`

1. 导入菜谱数据：包括菜谱、食材、烹饪步骤等节点和关系

导入的数据包括：

+ 菜谱节点：包含菜名、难度、烹饪时间、菜系等信息

+ 食材节点：包含食材名称、分类、营养信息等

+ 烹饪步骤节点：包含步骤描述、烹饪方法、所需工具等

+ 关系网络：菜谱与食材、步骤之间的复杂关系

如果需要手动重新导入数据：

```
# 进入容器执行导入脚本
docker exec -it neo4j-db cypher-shell -u neo4j -p all-in-rag -f /import/cypher/neo4j_import.cypher
```

### 2.4 Milvus向量数据库配置

#### 2.4.1 使用Docker安装Milvus

> 如果前面已经安装过了可以跳过此步，通过 `docker-compose ps` 确认Milvus服务正在运行即可。

```
# 下载Milvus standalone配置文件
wget https://github.com/milvus-io/milvus/releases/download/v2.5.11/milvus-standalone-docker-compose.yml -O docker-compose.yml

# 启动Milvus
docker-compose up -d
```

#### 2.4.2 验证安装

```
# 检查Milvus服务状态
docker-compose ps
```

### 2.5 配置连接参数

在项目根目录创建 `.env` 文件：

```
# Neo4j配置
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=all-in-rag
NEO4J_DATABASE=neo4j

# Milvus配置
MILVUS_HOST=localhost
MILVUS_PORT=19530

# LLM API配置
MOONSHOT_API_KEY=your_api_key_here
```

## 三、系统架构设计

### 3.1 整体架构

我们的图RAG系统采用模块化设计，包含以下核心组件：

### 3.2 核心模块说明

#### 图数据准备模块 (`GraphDataPreparationModule`)

+ 功能：连接Neo4j数据库，加载图数据，构建结构化菜谱文档

+ 特点：支持图数据到文档的智能转换，保持知识结构完整性

#### 向量索引模块 (`MilvusIndexConstructionModule`)

+ 功能：构建和管理Milvus向量索引，支持语义相似度检索

+ 特点：使用 `BGE-small-zh-v1.5` 模型，512维向量空间

#### 混合检索模块 (`HybridRetrievalModule`)

+ 功能：传统的混合检索策略，结合向量检索和图扩展

+ 特点：双层检索（实体级+主题级），RRF轮询融合

#### 图RAG检索模块 (`GraphRAGRetrieval`)

+ 功能：基于图结构的高级检索，支持多跳推理和子图提取

+ 特点：图查询理解、多跳遍历、知识子图提取

#### 智能查询路由 (`IntelligentQueryRouter`)

+ 功能：分析查询特征，自动选择最适合的检索策略

+ 特点：LLM驱动的查询分析，动态策略选择

#### 生成集成模块 (`GenerationIntegrationModule`)

+ 功能：基于检索结果生成最终答案，支持流式输出

+ 特点：自适应生成策略，错误处理与重试机制

### 3.3 数据流程

1. 数据准备阶段：

+ 从Neo4j加载图数据（菜谱、食材、步骤节点及其关系）

+ 构建结构化菜谱文档，保持知识完整性

+ 进行智能文档分块，支持章节和长度双重分块策略

+ 构建Milvus向量索引，支持语义检索

1. 查询处理阶段：

+ 用户输入查询

+ 智能查询路由器分析查询特征（复杂度、关系密集度、推理需求）

+ 根据分析结果选择检索策略：

+ 简单查询 → 传统混合检索

+ 复杂推理 → 图RAG检索

+ 中等复杂 → 组合检索策略

+ 执行相应的检索操作

+ 生成模块基于检索结果生成答案

1. 错误处理与降级：

+ 高级策略失败时自动降级到传统混合检索

+ 传统混合检索失败时返回系统异常

+ 支持流式输出中断时的自动重试机制

## 四、项目文件结构

```
code/C9/
├── main.py                          # 主程序入口
├── config.py                        # 配置文件
├── requirements.txt                 # 依赖包列表
└── rag_modules/                     # RAG模块包
    ├── __init__.py
    ├── graph_data_preparation.py    # 图数据准备模块
    ├── milvus_index_construction.py # Milvus索引构建模块
    ├── hybrid_retrieval.py          # 混合检索模块
    ├── graph_rag_retrieval.py       # 图RAG检索模块
    ├── intelligent_query_router.py  # 智能查询路由器
    └── generation_integration.py    # 生成集成模块
```

## 五、快速开始

### 5.1 启动系统

```
# 确保Neo4j和Milvus服务已启动
python main.py
```

### 5.2 系统初始化

首次运行时，系统会自动：

1. 检查并连接Neo4j和Milvus数据库

1. 加载图数据并构建菜谱文档

1. 创建向量索引

1. 初始化各个检索模块

1. 显示系统统计信息

### 5.3 交互式问答

系统启动后，可以进行交互式问答：
