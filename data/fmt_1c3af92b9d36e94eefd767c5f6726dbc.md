
# 第四节 Milvus介绍及多模态检索实践

> 来源：[https://gotoaiworld.feishu.cn/docx/PdCadyXcYoxjHMxr2m3cHFCjndf](https://gotoaiworld.feishu.cn/docx/PdCadyXcYoxjHMxr2m3cHFCjndf)

## 一、简介

Milvus 是一个开源的、专为大规模向量相似性搜索和分析而设计的向量数据库。它诞生于 Zilliz 公司，并已成为 LF AI & Data 基金会的顶级项目，在AI领域拥有广泛的应用。

与 FAISS、ChromaDB 等轻量级本地存储方案不同，Milvus 从设计之初就瞄准了生产环境。其采用云原生架构，具备高可用、高性能、易扩展的特性，能够处理十亿、百亿甚至更大规模的向量数据。

官网地址: `https://milvus.io/`

GitHub: `https://github.com/milvus-io/milvus`

## 二、部署安装

Milvus 提供了多种部署方式，这里以 Milvus Standalone (单机版) 为例。

### 1. 环境准备

+ 安装 `Docker` 与 `Docker Compose`：确保系统中已安装并正在运行 `Docker` 和 `Docker Compose`。如果你对 `Docker` 不熟悉，可以参考这篇详细的教程：Docker 万字教程：从入门到掌握。

> codespace 环境自带 `Docker Compose` 无需安装

### 2. 下载并启动 Milvus

在你选定的工作目录下，打开终端（Terminal）或命令行工具（PowerShell），执行以下步骤：

第一步：下载配置文件

使用以下命令下载官方的 `docker-compose.yml` 文件。这个文件定义了 Milvus Standalone 及其运行所需的两个核心依赖服务：`etcd` 用于存储元数据，`MinIO` 用于对象存储（更多架构细节请参考官方文档）。

```
# macOS / Linux (使用 wget)
wget https://github.com/milvus-io/milvus/releases/download/v2.5.14/milvus-standalone-docker-compose.yml -O docker-compose.yml
```

```
# Windows (使用 PowerShell)
Invoke-WebRequest -Uri "https://github.com/milvus-io/milvus/releases/download/v2.5.14/milvus-standalone-docker-compose.yml" -OutFile "docker-compose.yml"
```

第二步：启动 Milvus 服务

在 `docker-compose.yml` 文件所在的目录中，运行以下命令以后台模式启动 Milvus：

```
docker compose up -d
```

`Docker` 将会自动拉取所需的镜像并启动三个容器：`milvus-standalone`, `milvus-minio`, 和 `milvus-etcd`。这个过程可能需要几分钟，具体取决于你的网络状况。

### 3. 验证安装

可以通过以下方式验证 Milvus 是否成功启动：

+ 查看 `Docker` 容器：打开 `Docker Desktop` 的仪表盘 (Windows/macOS) 或在终端运行 `docker ps` 命令 (Linux)，确认三个 Milvus 相关容器（`milvus-standalone`, `milvus-minio`, `milvus-etcd`）都处于 `running` 或 `up` 状态。

+ 检查服务端口：`Milvus Standalone` 默认通过 `19530` 端口提供服务，这是后续代码连接时需要用到的地址。

### 4. 常用管理命令

+ 停止服务：

```
docker compose down
```

此命令会停止并移除容器，但保留存储的数据卷。

+ 彻底清理（停止并删除数据）：如果想彻底删除所有数据（包括向量、元数据等），可以执行以下命令：

```
docker compose down -v
```

## 三、核心组件

### 3.1 Collection (集合)

可以用一个图书馆的比喻来理解 `Collection`：

+ `Collection` (集合)：相当于一个图书馆，是所有数据的顶层容器。一个 `Collection` 可以包含多个 `Partition`，每个 `Partition` 可以包含多个 `Entity`。

+ `Partition` (分区)：相当于图书馆里的不同区域（如“小说区”、“科技区”），将数据物理隔离，让检索更高效。

+ `Schema` (模式)：相当于图书馆的图书卡片规则，定义了每本书（数据）必须登记哪些信息（字段）。

+ `Entity` (实体)：相当于一本具体的书，是数据本身。

+ `Alias` (别名)：相当于一个动态的推荐书单（如“本周精选”），它可以指向某个具体的 `Collection`，方便应用层调用，实现数据更新时的无缝切换。

`Collection` 是 Milvus 中最基本的数据组织单位，类似于关系型数据库中的一张表 (`Table`)。是我们存储、管理和查询向量及相关元数据的容器。所有的数据操作，如插入、删除、查询等，都是围绕 `Collection` 展开的。

一个 `Collection` 由其 `Schema` 定义，并包含以下重要的子概念和特性：

#### 3.1.1 Schema

在创建 `Collection` 之前，必须先定义它的 `Schema`。 `Schema` 规定了 `Collection` 的数据结构，定义了其中包含的所有字段 (`Field`) 及其属性。一个设计良好的 `Schema` 是能够保证数据一致性并提升查询性能。

`Schema` 通常包含以下几类字段：

+ 主键字段 (Primary Key Field)：每个 `Collection` 必须有且仅有一个主键字段，用于唯一标识每一条数据（实体）。它的值必须是唯一的，通常是整数或字符串类型。

+ 向量字段 (Vector Field)：用于存储核心的向量数据。一个 `Collection` 可以有一个或多个向量字段，以满足多模态等复杂场景的需求。

+ 标量字段 (Scalar Field)：用于存储除向量之外的元数据，如字符串、数字、布尔值、`JSON` 等。这些字段可以用于过滤查询，实现更精确的检索。

![](img/41b357e0292001904a2d7ac226bd7dc0.png)

上图以一篇新闻文章为例，展示了一个典型的多模态、混合向量 `Schema` 设计。它将一篇文章拆解为：唯一的 `Article (ID)`、文本元数据（如 `Title`、`Author Info`）、图像信息（`Image URL`），并为图像和摘要内容分别生成了密集向量（`Image Embedding`, `Summary Embedding`）和稀疏向量（`Summary Sparse Embedding`）。

#### 3.1.2 Partition (分区)

`Partition` 是 `Collection` 内部的一个逻辑划分。每个 `Collection` 在创建时都会有一个名为 `_default` 的默认分区。我们可以根据业务需求创建更多的分区，将数据按特定规则（如类别、日期等）存入不同分区。

为什么使用分区？

+ 提升查询性能：在查询时，可以指定只在一个或几个分区内进行搜索，从而大幅减少需要扫描的数据量，显著提升检索速度。

+ 数据管理：便于对部分数据进行批量操作，如加载/卸载特定分区到内存，或者删除整个分区的数据。

一个 `Collection` 最多可以有 1024 个分区。合理利用分区是 Milvus 性能优化的重要手段之一。

#### 3.1.3 Alias (别名)

`Alias` (别名) 是为 `Collection` 提供的一个“昵称”。通过为一个 `Collection` 设置别名，我们可以在应用程序中使用这个别名来执行所有操作，而不是直接使用真实的 `Collection` 名称。

为什么使用别名？

+ 安全地更新数据：想象一下，你需要对一个在线服务的 `Collection` 进行大规模的数据更新或重建索引。直接在原 `Collection` 上操作风险很高。正确的做法是：

1. 创建一个新的 `Collection` (`collection_v2`) 并导入、索引好所有新数据。

1. 将指向旧 `Collection` (`collection_v1`) 的别名（例如 `my_app_collection`）原子性地切换到新 `Collection` (`collection_v2`) 上。

+ 代码解耦：整个切换过程对上层应用完全透明，无需修改任何代码或重启服务，实现了数据的平滑无缝升级。

### 3.2 索引 (Index)

如果说 `Collection` 是 Milvus 的骨架，那么索引 (`Index`) 就是其加速检索的神经系统。从宏观上看，索引本身就是一种为了加速查询而设计的复杂数据结构。对向量数据创建索引后，Milvus 可以极大地提升向量相似性搜索的速度，代价是会占用额外的存储和内存资源。

![](img/925e3224c61a9c4ad41d515cb910a2f2.png)

上图清晰地展示了 Milvus 向量索引的内部组件及其工作流程：

+ 数据结构：这是索引的骨架，定义了向量的组织方式（如 `HNSW` 中的图结构）。

+ 量化 (可选)：数据压缩技术，通过降低向量精度来减少内存占用和加速计算。

+ 结果精炼 (可选)：在找到初步候选集后，进行更精确的计算以优化最终结果。

Milvus 支持对标量字段和向量字段分别创建索引。

+ 标量字段索引：主要用于加速元数据过滤，常用的有 `INVERTED`、`BITMAP` 等。通常使用推荐的索引类型即可。

+ 向量字段索引：这是 Milvus 的核心。选择合适的向量索引是在查询性能、召回率和内存占用之间做出权衡的艺术。

#### 3.2.1 主要向量索引类型

Milvus 提供了多种向量索引算法，以适应不同的应用场景。以下是几种最核心的类型：

+ `FLAT` (精确查找)

  原理：暴力搜索 (`Brute-force Search`)。它会计算查询向量与集合中所有向量之间的实际距离，返回最精确的结果。

  优点：100% 的召回率，结果最准确。

  缺点：速度慢，内存占用大，不适合海量数据。

  适用场景：对精度要求极高，且数据规模较小（百万级以内）的场景。

+ `IVF` 系列 (倒排文件索引)

  原理：类似于书籍的目录。它首先通过聚类将所有向量分成多个“桶”(`nlist`)，查询时，先找到最相似的几个“桶”，然后只在这几个桶内进行精确搜索。`IVF_FLAT`、`IVF_SQ8`、`IVF_PQ` 是其不同变体，主要区别在于是否对桶内向量进行了压缩（量化）。

  优点：通过缩小搜索范围，极大地提升了检索速度，是性能和效果之间很好的平衡。

  缺点：召回率不是 100%，因为相关向量可能被分到了未被搜索的桶中。

  适用场景：通用场景，尤其适合需要高吞吐量的大规模数据集。

+ `HNSW` (基于图的索引)

  原理：构建一个多层的邻近图。查询时从最上层的稀疏图开始，快速定位到目标区域，然后在下层的密集图中进行精确搜索。

  优点：检索速度极快，召回率高，尤其擅长处理高维数据和低延迟查询。

  缺点：内存占用非常大，构建索引的时间也较长。

  适用场景：对查询延迟有严格要求（如实时推荐、在线搜索）的场景。

+ `DiskANN` (基于磁盘的索引)

  原理：一种为在 SSD 等高速磁盘上运行而优化的图索引。

  优点：支持远超内存容量的海量数据集（十亿级甚至更多），同时保持较低的查询延迟。

  缺点：相比纯内存索引，延迟稍高。

  适用场景：数据规模巨大，无法全部加载到内存的场景。

#### 3.2.2 如何选择索引？

选择索引没有唯一的“最佳答案”，需要根据业务场景在数据规模、内存限制、查询性能和召回率之间进行权衡。

在实际应用中，通常需要通过测试来找到最适合自己数据和查询模式的索引类型及其参数。

### 3.3 检索

拥有了数据容器 (`Collection`) 和检索引擎 (`Index`) 后，最后一步就是从海量数据中高效地检索信息。

#### 3.3.1 基础向量检索 (ANN Search)

这是 Milvus 的核心功能之一，近似最近邻 (`Approximate Nearest Neighbor`, `ANN`) 检索。与需要计算全部数据的暴力检索 (`Brute-force Search`) 不同，`ANN` 检索利用预先构建好的索引，能够极速地从海量数据中找到与查询向量最相似的 `Top-K` 个结果。这是一种在速度和精度之间取得极致平衡的策略。

+ 主要参数：

  + `anns_field`：指定要在哪个向量字段上进行检索。

  + `data`：传入一个或多个查询向量。

  + `limit` (或 `top_k`)：指定需要返回的最相似结果的数量。

  + `search_params`：指定检索时使用的参数，例如距离计算方式 (`metric_type`) 和索引相关的查询参数。

#### 3.3.2 增强检索

在基础的 `ANN` 检索之上，Milvus 提供了多种增强检索功能，以满足更复杂的业务需求。

过滤检索 (`Filtered Search`)

在实际应用中，我们很少只进行单纯的向量检索。更常见的需求是“在满足特定条件的向量中，查找最相似的结果”，这就是过滤检索。它将向量相似性检索与标量字段过滤结合在一起。

+ 工作原理：先根据提供的过滤表达式 (`filter`) 筛选出符合条件的实体，然后仅在这个子集内执行 `ANN` 检索。这极大地提高了查询的精准度。

+ 应用示例：

  + 电商：“检索与这件红色连衣裙最相似的商品，但只看价格低于 500 元且有库存的。”

  + 知识库：“查找与‘人工智能’相关的文档，但只从‘技术’分类下、且发布于 2023 年之后的文章中寻找。”

范围检索 (`Range Search`)

有时我们关心的不是最相似的 `Top-K` 个结果，而是“所有与查询向量的相似度在特定范围内的结果”。
