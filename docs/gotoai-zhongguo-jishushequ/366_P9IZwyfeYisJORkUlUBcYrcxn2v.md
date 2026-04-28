# 第三节 Milvus索引构建

> 来源：[https://gotoaiworld.feishu.cn/docx/VF3GdvOIBoK2fRxXXDvcmoM3n1Y](https://gotoaiworld.feishu.cn/docx/VF3GdvOIBoK2fRxXXDvcmoM3n1Y)

在图RAG系统中，索引构建是连接图数据和向量检索的关键环节。本节介绍如何将图数据转换为可检索的向量索引。

在第三章中，我们已经详细介绍了Milvus的基本概念、部署方式和基础操作。本节将在此基础上，专门针对图RAG场景进行深度应用。如果你对Milvus还不熟悉，建议先阅读Milvus介绍及多模态检索实践。

> [本节完整代码](https://github.com/datawhalechina/all-in-rag/blob/main/code/C9/rag_modules/milvus_index_construction.py)

## [一、索引构建概述](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=%e4%b8%80%e3%80%81%e7%b4%a2%e5%bc%95%e6%9e%84%e5%bb%ba%e6%a6%82%e8%bf%b0)

### [1.1 索引构建流程](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=_11-%e7%b4%a2%e5%bc%95%e6%9e%84%e5%bb%ba%e6%b5%81%e7%a8%8b)

图RAG的索引构建需要将从图数据库构建的结构化文档转换为向量表示，并存储到向量数据库中：

### [1.2 核心组件](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=_12-%e6%a0%b8%e5%bf%83%e7%bb%84%e4%bb%b6)

+   文档构建器：从图数据构建结构化文档

+   分块处理器：智能分块策略

+   向量化模型：文本转向量

+   Milvus索引：高性能向量存储和检索

## [二、Milvus索引构建实现](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=%e4%ba%8c%e3%80%81milvus%e7%b4%a2%e5%bc%95%e6%9e%84%e5%bb%ba%e5%ae%9e%e7%8e%b0)

### [2.1 索引构建器核心架构](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=_21-%e7%b4%a2%e5%bc%95%e6%9e%84%e5%bb%ba%e5%99%a8%e6%a0%b8%e5%bf%83%e6%9e%b6%e6%9e%84)

```
class MilvusIndexConstructionModule:&quot;&quot;&quot;Milvus索引构建模块 - 负责向量化和Milvus索引构建&quot;&quot;&quot;def __init__(self,
                 host: str = &quot;localhost&quot;,
                 port: int = 19530,
                 collection_name: str = &quot;cooking_knowledge&quot;,
                 dimension: int = 512,
                 model_name: str = &quot;BAAI/bge-small-zh-v1.5&quot;):
        self.host = host
        self.port = port
        self.collection_name = collection_name
        self.dimension = dimension
        self.model_name = model_name

        self.client = None
        self.embeddings = None
        self.collection_created = False

        self._setup_client()
        self._setup_embeddings()Copy to clipboardErrorCopied
```

代码解读：

+   模块化设计：将Milvus操作封装为独立模块，便于复用和维护

+   配置灵活性：支持自定义Milvus连接参数和嵌入模型

+   中文优化：默认使用BAAI/bge-small-zh-v1.5，专门针对中文文本优化

+   延迟初始化：在构造函数中设置连接，避免启动时的阻塞

### [2.2 向量化处理](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=_22-%e5%90%91%e9%87%8f%e5%8c%96%e5%a4%84%e7%90%86)

```
def _vectorize_documents(self, documents: List[Document]) -&gt; Tuple[List[List[float]], List[Dict]]:&quot;&quot;&quot;文档向量化处理&quot;&quot;&quot;
    vectors = []
    metadatas = []for i, doc in enumerate(documents):try:# 向量化文档内容
            vector = self.embedding_model.embed_query(doc.page_content)
            vectors.append(vector)# 准备元数据
            metadata = {&quot;id&quot;: i,&quot;content&quot;: doc.page_content,&quot;source&quot;: doc.metadata.get(&quot;source&quot;, &quot;&quot;),&quot;chunk_id&quot;: doc.metadata.get(&quot;chunk_id&quot;, &quot;&quot;),&quot;parent_id&quot;: doc.metadata.get(&quot;parent_id&quot;, &quot;&quot;),# ... 其他元数据}
            metadatas.append(metadata)except Exception as e:
            logger.error(f&quot;文档 {i} 向量化失败: {e}&quot;)continuereturn vectors, metadatasCopy to clipboardErrorCopied
```

### 2.3 图RAG专用集合Schema设计

```
def _create_collection_schema(self):&quot;&quot;&quot;创建集合schema&quot;&quot;&quot;
    fields = [
        FieldSchema(name=&quot;id&quot;, dtype=DataType.VARCHAR, max_length=150, is_primary=True),
        FieldSchema(name=&quot;vector&quot;, dtype=DataType.FLOAT_VECTOR, dim=self.dimension),
        FieldSchema(name=&quot;text&quot;, dtype=DataType.VARCHAR, max_length=15000),
        FieldSchema(name=&quot;node_id&quot;, dtype=DataType.VARCHAR, max_length=100),
        FieldSchema(name=&quot;recipe_name&quot;, dtype=DataType.VARCHAR, max_length=300),
        FieldSchema(name=&quot;node_type&quot;, dtype=DataType.VARCHAR, max_length=100),
        FieldSchema(name=&quot;category&quot;, dtype=DataType.VARCHAR, max_length=100),
        FieldSchema(name=&quot;cuisine_type&quot;, dtype=DataType.VARCHAR, max_length=200),
        FieldSchema(name=&quot;difficulty&quot;, dtype=DataType.INT64),
        FieldSchema(name=&quot;doc_type&quot;, dtype=DataType.VARCHAR, max_length=50),
        FieldSchema(name=&quot;chunk_id&quot;, dtype=DataType.VARCHAR, max_length=150),
        FieldSchema(name=&quot;parent_id&quot;, dtype=DataType.VARCHAR, max_length=100)]

    schema = CollectionSchema(
        fields=fields,
        description=&quot;中式烹饪知识图谱向量集合&quot;)return schemaCopy to clipboardErrorCopied
```

Schema设计亮点：

+   图数据特化：专门为烹饪知识图谱设计的字段结构

+   丰富元数据：包含菜谱名称、节点类型、菜系、难度等图谱特有信息

+   长度优化：根据实际数据特点设置合理的字段长度限制

+   检索友好：所有关键字段都可用于过滤和检索条件

## [三、索引优化策略](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=%e4%b8%89%e3%80%81%e7%b4%a2%e5%bc%95%e4%bc%98%e5%8c%96%e7%ad%96%e7%95%a5)

### [3.1 批量插入优化](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=_31-%e6%89%b9%e9%87%8f%e6%8f%92%e5%85%a5%e4%bc%98%e5%8c%96)

```
def _batch_insert(self, vectors: List[List[float]], metadatas: List[Dict]):&quot;&quot;&quot;批量插入优化&quot;&quot;&quot;
    batch_size = self.config.batch_size
    collection_name = self.config.milvus_collection_name
    
    for i in range(0, len(vectors), batch_size):
        batch_vectors = vectors[i:i + batch_size]
        batch_metadatas = metadatas[i:i + batch_size]# 准备插入数据
        insert_data = [[meta[&quot;id&quot;] for meta in batch_metadatas],           # id
            batch_vectors,                                       # vector[meta[&quot;content&quot;] for meta in batch_metadatas],      # content[meta[&quot;source&quot;] for meta in batch_metadatas],       # source[meta[&quot;chunk_id&quot;] for meta in batch_metadatas],     # chunk_id[meta[&quot;parent_id&quot;] for meta in batch_metadatas],    # parent_id]# 执行插入
        self.milvus_client.insert(collection_name, insert_data)
        logger.info(f&quot;批次 {i//batch_size + 1} 插入完成，数量: {len(batch_vectors)}&quot;)Copy to clipboardErrorCopied
```

### [3.2 索引创建](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=_32-%e7%b4%a2%e5%bc%95%e5%88%9b%e5%bb%ba)

```
def _create_index(self):&quot;&quot;&quot;创建向量索引&quot;&quot;&quot;
    collection_name = self.config.milvus_collection_name
    
    # 索引参数
    index_params = {&quot;metric_type&quot;: &quot;COSINE&quot;,    # 余弦相似度&quot;index_type&quot;: &quot;IVF_FLAT&quot;,   # 索引类型&quot;params&quot;: {&quot;nlist&quot;: 1024}   # 索引参数}# 创建索引
    self.milvus_client.create_index(
        collection_name=collection_name,
        field_name=&quot;vector&quot;,
        index_params=index_params
    )# 加载集合到内存
    self.milvus_client.load_collection(collection_name)
    
    logger.info(&quot;向量索引创建完成&quot;)Copy to clipboardErrorCopied
```

## [四、索引构建流程](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=%e5%9b%9b%e3%80%81%e7%b4%a2%e5%bc%95%e6%9e%84%e5%bb%ba%e6%b5%81%e7%a8%8b)

### [4.1 核心向量构建流程](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=_41-%e6%a0%b8%e5%bf%83%e5%90%91%e9%87%8f%e6%9e%84%e5%bb%ba%e6%b5%81%e7%a8%8b)

```
def build_vector_index(self, chunks: List[Document]) -&gt; bool:&quot;&quot;&quot;构建向量索引&quot;&quot;&quot;
    logger.info(f&quot;正在构建Milvus向量索引，文档数量: {len(chunks)}...&quot;)try:# 1. 创建集合（如果schema不兼容则强制重新创建）if not self.create_collection(force_recreate=True):return False# 2. 准备数据
        logger.info(&quot;正在生成向量embeddings...&quot;)
        texts = [chunk.page_content for chunk in chunks]
        vectors = self.embeddings.embed_documents(texts)# 3. 准备插入数据
        entities = []for i, (chunk, vector) in enumerate(zip(chunks, vectors)):
            entity = {&quot;id&quot;: self._safe_truncate(chunk.metadata.get(&quot;chunk_id&quot;, f&quot;chunk_{i}&quot;), 150),&quot;vector&quot;: vector,&quot;text&quot;: self._safe_truncate(chunk.page_content, 15000),&quot;node_id&quot;: self._safe_truncate(chunk.metadata.get(&quot;node_id&quot;, &quot;&quot;), 100),&quot;recipe_name&quot;: self._safe_truncate(chunk.metadata.get(&quot;recipe_name&quot;, &quot;&quot;), 300),# ... 更多字段}
            entities.append(entity)# 4. 批量插入数据
        batch_size = 100for i in range(0, len(entities), batch_size):
            batch = entities[i:i + batch_size]
            self.client.insert(collection_name=self.collection_name, data=batch)Copy to clipboardErrorCopied
```

关键技术点解读：

1.  强制重建策略：force_recreate=True确保Schema一致性，避免字段不匹配错误

1.  批量向量化：一次性处理所有文档的向量化，提高效率

```
texts = [chunk.page_content for chunk in chunks]
vectors = self.embeddings.embed_documents(texts)  # 批量处理Copy to clipboardErrorCopied
```

1.  安全截断机制：_safe_truncate方法防止字段长度超限

```
def _safe_truncate(self, text: str, max_length: int) -&gt; str:if text is None:return &quot;&quot;return str(text)[:max_length]Copy to clipboardErrorCopied
```

1.  图数据元数据保留：完整保留图谱中的结构化信息，支持后续的复合检索

### [4.2 索引验证](https://datawhalechina.github.io/all-in-rag/#/chapter9/03_index_construction?id=_42-%e7%b4%a2%e5%bc%95%e9%aa%8c%e8%af%81)

```
def verify_index(self) -&gt; bool:&quot;&quot;&quot;验证索引构建结果&quot;&quot;&quot;try:
        collection_name = self.config.milvus_collection_name
        
        # 检查集合状态
        collection_info = self.milvus_client.describe_collection(collection_name)
        logger.info(f&quot;集合信息: {collection_info}&quot;)# 检查数据量
        count = self.milvus_client.query(
            collection_name=collection_name,
            expr=&quot;&quot;,
            output_fields=[&quot;count(*)&quot;])
        logger.info(f&quot;索引中文档数量: {count}&quot;)# 简单检索测试
        test_results = self.milvus_client.search(
            collection_name=collection_name,
            data=[[0.1] * self.config.embedding_dim],  # 测试向量
            anns_field=&quot;vector&quot;,
            param={&quot;metric_type&quot;: &quot;COSINE&quot;, &quot;params&quot;: {&quot;nprobe&quot;: 10}},
            limit=1)
        
        logger.info(&quot;索引验证通过&quot;)return Trueexcept Exception as e:
        logger.error(f&quot;索引验证失败: {e}&quot;)return FalseCopy to clipboardErrorCopied
```

## 五、为什么从FAISS切换到Milvus？

在第八章中，使用的是FAISS作为向量存储方案。虽然FAISS在研究和原型开发中表现出色，但在生产环境和复杂应用场景下，Milvus提供了更多优势：

FAISS的局限性：

+   纯库模式：FAISS是一个向量搜索库，缺乏数据库的完整功能

+   无持久化：需要手动管理数据持久化和备份

+   单机限制：难以实现分布式部署和水平扩展

+   元数据支持有限：无法高效存储和查询复杂的结构化元数据

+   并发性能：在高并发场景下性能受限

Milvus的优势：

+   完整数据库功能：提供CRUD操作、事务支持、数据一致性保证

+   云原生架构：支持分布式部署、自动扩缩容、高可用性

+   丰富的元数据支持：支持复杂Schema设计，适合图RAG的多维度数据

+   生产级特性：监控、日志、备份恢复等企业级功能

[ 上一章节](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling)

[图数据建模与准备](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling)