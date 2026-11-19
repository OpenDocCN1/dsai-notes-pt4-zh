
# 2023-05-29 LLM之高性能向量检索库

> 来源：https://xuqiwei1986.feishu.cn/docx/OdRxdq03xovV4mxNnvtctxtpnze

> 🔗 原文链接：https://blog.csdn.net/qq128252/article/details/130930164?spm=1001.2014.3001.5502

## LLM之高性能向量检索库

### LLM向量数据库

- 高性能向量检索库
- milvus
  - 简介
  - 安装
  - 调用
- faiss
  - 简介
  - 安装
  - 调用

## 高性能向量检索库

### milvus

#### 简介

> [Milvus](https://so.csdn.net/so/search?q=%E6%95%B0%E6%8D%AE%E5%BA%93%E5%BC%95%E6%93%8E&spm=1001.2101.3001.7020) 是一个开源的向量数据库引擎，旨在提供高效的向量存储、检索和分析能力。它被设计用于处理大规模的高维向量数据，常用于机器学习、计算机视觉、自然语言处理和推荐系统等领域。

> `Milvus` 提供了多种功能和特性，使其成为处理向量数据的理想选择。以下是一些 `Milvus` 的主要特点：

1. 高性能：`Milvus` 使用了高度优化的数据结构和索引算法，以实现快速的向量检索。它支持多种索引类型，如平面索引、倒排索引和 HNSW（Hierarchical Navigable Small World）等，这些索引能够加速向量的相似度搜索。

2. 可扩展性：`Milvus` 具备良好的可扩展性，可以轻松地扩展到大规模的向量数据集。它支持分布式部署，可以在多个节点上进行数据存储和查询操作，实现高吞吐量和低延迟。

3. 多样化的向量类型：`Milvus` 支持多种向量类型，包括浮点型向量、二进制向量和文本向量等。这使得它可以适应不同领域和应用中的向量数据需求。

4. 多语言支持：`Milvus` 提供了多种编程语言的 SDK（软件开发工具包），包括 Python、Java、Go 和 C++ 等，使开发者可以方便地集成 `Milvus` 到他们的应用程序中。

5. 可视化管理界面：`Milvus` 提供了一个易于使用的 Web 界面，用于管理和监控向量数据库。开发者可以通过该界面进行数据导入、索引构建和查询优化等操作，同时还能够查看系统状态和性能指标。

> 总之，`Milvus` 是一个功能强大的向量数据库引擎，通过其高性能、可扩展性和多样化的特性，能够有效地存储和检索大规模的高维向量数据。它在许多领域的应用中发挥着重要作用，帮助开发者加速向量相关任务的开发和部署。

#### 安装

- `docker` 安装镜像：`docker pull milvusdb/milvus:cpu-latest`
- 创建工作目录：
  - `mkdir milvus`
  - `cd milvus`
  - `mkdir congf`
  - `mkdir db`
  - `mkdir logs`
  - `mkdir wal`

我的目录结构是：

```
milvus
│
├─conf //配置文件目录
│      server_config.yaml  //配置文件 搜索引擎配置都在这里修改
│
├─db //数据库存储目录 你的索引与向量存储的位置
│
└─logs //日志存储目录 
│
└─wal // 预写式日志相关配置
```

`server_config.yaml`

> `docker run -it milvusdb/milvus:cpu-latest bash`

> `docker cp 74c20a680091:/var/lib/milvus/conf/server_config.yaml milvus/conf/`

- 启动容器

```
docker run -td --name mymilvus -e "TZ=Asia/Shanghai" -p 19530:19530 -p 19121:19121 -v /work/lnn_workspace/chatgpt/search/milvus/conf:/var/lib/milvus/conf -v /work/lnn_workspace/chatgpt/search/milvus/db:/var/lib/milvus/db -v /work/lnn_workspace/chatgpt/search/milvus/logs:/var/lib/milvus/logs   milvusdb/milvus:cpu-latest
```

- `docker ps | grep mymilvus`

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-orhH8dlK-1685346967888)(C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230519105321396.png)]

- 安装 `pymilvus`

```
pip install pymilvus==1.1.2
ps:注意这里安装最新版本可以会无法正常调用，1.1.2经过测试可正常使用
```

#### 调用

```
# -*- coding: utf-8 -*-  

# 导入相应的包  
import numpy as np  
from milvus import Milvus, MetricType  

# 初始化一个 `Milvus` 类，以后所有的操作都是通过 `milvus` 来的  
milvus = Milvus(host='localhost', port='19530')  

# 向量个数  
num_vec = 5000  
# 向量维度  
vec_dim = 768  
# name  
collection_name = "test_collection"  
# 创建 collection，可理解为 mongo 的 collection  
collection_param = {  
    'collection_name': collection_name,  
    'dimension': vec_dim,  
    'index_file_size': 32,  
    'metric_type': MetricType.IP  # 使用内积作为度量值  
}  
milvus.create_collection(collection_param)  

# 随机生成一批向量数据  
# 支持 ndarray，也支持 list  
vectors_array = np.random.rand(num_vec, vec_dim)  

# 把向量添加到刚才建立的 collection 中  
status, ids = milvus.insert(collection_name=collection_name, records=vectors_array)  # 返回 状态和这一组向量的 ID  
milvus.flush([collection_name])  

# 输出统计信息  
print(milvus.get_collection_stats(collection_name))  

# 创建查询向量  
query_vec_array = np.random.rand(1, vec_dim)  
# 进行查询,  
status, results = milvus.search(collection_name=collection_name, query_records=query_vec_array, top_k=5)  
print(status)  
print(results)  

# 如果不用可以删掉  
status = milvus.drop_collection(collection_name)  

# 断开、关闭连接  
milvus.close()
```

```
collection_name = "test_collection";
- 定义集合名称为 `test_collection`
collection_param = {
    'collection_name': collection_name,
    'dimension': vec_dim,
    'index_file_size': 32,
    'metric_type': MetricType.IP 
}
milvus.create_collection(collection_param)  



- `collection_name` 指定集合名称为 `test_collection`
- `dimension` 表示集合中向量的维度，由 `vec_dim` 变量赋值
- `index_file_size` 设置索引文件大小为 32MB
- `metric_type` 设置度量类型为 `IP`，表示使用向量内积作为相似度度量
所以，这段代码定义了集合名称和相关参数，用于在 `Milvus` 服务上创建一个新的集合。
在 `Milvus` 中，集合相当于关系数据库的表，是存储向量的基本单元。在创建集合时，我们需要指定：
1. 集合名称：唯一标识一个集合
2. 向量维度：集合中向量的特征数量
3. 度量类型：测量向量之间相似度的算法，如 `IP` (内积)、`L2` (欧氏距离) 等
4. 索引文件大小：用于提高搜索性能，索引文件会存储向量的索引信息
```

### faiss

#### 简介

`Faiss` 是 Facebook 开源的一个向量检索库，用于大规模向量集合的索引和搜索。主要功能包括：

1. 支持多种索引结构：`IVF`, `IVFFlat`, `HNSW` 等。这些索引结构可以实现高精度和高召回的向量搜索。

2. 支持多种度量方式：内积、欧氏距离、`cosine` 相似度等。可选择合适的度量方式对向量集合建立索引。

3. 快速的索引构建与搜索：`Faiss` 使用 `GPU` 加速，可以实现亿量级向量的索引构建和搜索。

4. 降维与聚类：`Faiss` 提供 `PCA`, `IVFFlat` 等算法进行向量降维，并支持 `Kmeans` 算法进行向量聚类。

5. 高级特性：`Faiss` 支持在线学习、异构向量检索、索引压缩等高级特性。

`Faiss` 的典型应用有：

1. 图像检索：在大规模图片数据库中找到与输入图片最相似的图片。

2. 文本匹配：快速找到与输入文本最相近的文本内容。

3. 推荐系统：根据用户兴趣对大量商品进行快速检索和推荐。

4. 声纹识别：在海量语音数据中实现语音识别和检索。

`Faiss` 提供 `C++`, `Python` 和 `Java` 语言接口，可以轻松构建向量检索系统。如果需要管理和搜索海量高维向量，`Faiss` 是一个非常好的选择。

#### 安装

```
install faiss-cpu
```

#### 调用

```
# 导入库  
import numpy as np  
import faiss  

# 向量个数  
num_vec = 5000  
# 向量维度  
vec_dim = 768  
# 搜索 topk  
topk = 10  

# 随机生成一批向量数据  
vectors = np.random.rand(num_vec, vec_dim)  

# 创建索引  
faiss_index = faiss.IndexFlatL2(vec_dim)  # 使用欧式距离作为度量  
# 添加数据  
faiss_index.add(vectors)  

# 查询向量 假设有 5 个  
query_vectors = np.random.rand(5, vec_dim)  
# 搜索结果  
# 分别是 每条记录对应 `topk` 的距离和索引  
# `ndarray` 类型 。shape：`len(query_vectors)` * `topk`  
res_distance, res_index = faiss_index.search(query_vectors, topk)  
print(res_index)  
print(res_distance)
```
