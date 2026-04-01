# 摆脱Faiss束缚：sqlite-vec如何在RAG中实现SQLite的原生向量检索轻量化

> 来源：[https://datawhaler.feishu.cn/docx/Q63Adr2p7oTwMCxindzcczS4nLe](https://datawhaler.feishu.cn/docx/Q63Adr2p7oTwMCxindzcczS4nLe)

# 摆脱Faiss束缚：sqlite-vec如何在RAG中实现SQLite的原生向量检索轻量化

![](img/9e1074eef5e92c49a71696d1efdccaaa.png)

## 🎯 文章目标

我写这篇文章主要是为了跟大家分享一下在RAG应用中使用轻量级向量检索方案的一些探索。如果你正在尝试构建RAG应用，或许会发现这些内容有些帮助：

+   RAG架构中为什么会需要轻量化的向量检索组件

+   SQLite在向量检索领域的发展历程，特别是从依赖Faiss的sqlite-vss到无依赖的sqlite-vec的演变

+   怎么用sqlite-vec来实现基于SQLite的轻量级向量存储与检索

+   通过实际代码和性能测试，看看sqlite-vec在不同数据规模下的表现如何

> **💡小提示

> **现在RAG（检索增强生成）技术用得越来越多，找一个既能满足功能需求又不会太重的向量检索组件变得特别重要。尤其是对于小型项目或资源有限的环境，轻量化解决方案往往会更实用。

> 本次文章代码开源地址：[ht](https://github.com/li-xiu-qi/XiaokeAILabs/blob/main/datas/test_sqlite/advance_sqlite_vec_tutorial.ipynb)tps://github.com/li-xiu-qi/XiaokeAILabs/blob/main/datas/test_sqlite/advance_sqlite_vec_tutorial.ipynb

**开源项目宣传**

> 一个基于多模态向量模型及视觉多模态模型构建的图片搜索引擎&管理系统，实现精准的以文搜文，文搜图、以图搜图多种智能检索方式。

> **地址**：[https://github.com/li-xiu](https://github.com/li-xiu-qi/SmartlmageFinder)-qi/SmartlmageFinder

> 后续会使用sqlite对项目进行改造哦，欢迎star，感谢

> (本来是想要用duckdb的，但是由于一些其他因素，换成sqlite了，这篇文章里面会提到原因)

## 📄 本次主题

这次我想和大家聊聊在RAG架构下，SQLite如何通过sqlite-vec扩展摆脱对外部库的依赖，实现更原生、更轻量的向量检索方案。我会通过一些实际代码和性能测试，来看看它在简化开发流程、降低资源消耗以及提升查询效率方面表现如何，尤其是当数据量增加时会有什么变化。

## 🚁 前言

![](img/cb99264f95636cb9d2b86d720deec0e6.png)

检索增强生成（RAG）现在已经成为提升大语言模型回答质量的重要技术。这个技术能够帮助我们解决大模型知识有限、容易"产生幻觉"等问题。RAG的核心在于从外部知识库中找到相关信息，然后把这些信息放到提示中，让大模型参考使用。在这个过程里，向量数据库或有向量检索能力的组件扮演了非常关键的角色，它们会把文本转换成向量嵌入，然后通过计算向量间的相似度来找到相关信息。

不过，传统的专用向量数据库虽然功能很强大，但往往也会带来一些额外的麻烦，比如部署复杂、需要维护、资源消耗大等问题。对于很多个人项目、中小型应用，或者想在现有SQLite数据基础上快速加入向量检索功能的场景，引入一个重量级的外部依赖可能不是最好的选择。比如，当我们构建像SmartImageFinder这样的个人图片搜索系统时，通常希望技术栈尽可能简单，部署过程轻松，资源占用小。这也就是为什么我们需要一些轻量级的、嵌入式的向量检索方案，而SQLite作为一个几乎无处不在的轻量级关系型数据库，自然成为一个很好的选择。

这篇文章里，我们会重点看看SQLite在向量检索领域的探索和发展，特别是从早期依赖Faiss库的sqlite-vss，到现在新出现的、更加轻量高效的sqlite-vec扩展。我们会了解sqlite-vec是如何摆脱外部依赖，为SQLite带来原生向量检索能力的。同时，通过一些实际代码和详细的性能测试，比较它与没有扩展的方案在不同数据规模下的表现差异，最后分析一下它在轻量级RAG应用中的价值。

---

> ✨ 你好，我是筱可，欢迎来到「筱可 AI 研习社」！

> **🚀 标签关键词**

> | AI 实战派开发者 | 技术成长陪伴者 | RAG 前沿探索者 | 文档处理先锋 |

## 📒 SQLite向量检索的早期探索：以sqlite-vss为例

在SQLite中直接处理和检索向量数据的想法其实已经存在了一段时间。早期尝试中比较有代表性的是sqlite-vss，这是一个旨在给SQLite添加向量相似度搜索能力的扩展。sqlite-vss的核心思路是集成Facebook的AI相似度搜索库Faiss。Faiss是一个功能强大且使用广泛的开源库，专门用于高效地进行稠密向量的相似度搜索和聚类，它提供了很多先进的索引结构和搜索算法。

通过sqlite-vss，开发者理论上可以在SQLite数据库内部执行向量检索操作，这无疑为SQLite的应用场景带来了新的可能性。Faiss在其中充当底层引擎的角色，负责处理向量的索引构建和相似度计算。

然而，尽管sqlite-vss的初衷是好的，但它对Faiss的强依赖也带来了一系列问题，这些问题在追求极致轻量化和广泛适用性的场景中尤为明显：

首先是编译和部署的复杂性。Faiss主要用C++编写，虽然也有Python接口。将它集成到SQLite扩展中意味着在构建sqlite-vss时需要处理Faiss的编译链和各种依赖项。在不同操作系统上可能会遇到各种编译难题和兼容性问题，尤其是在Windows上（因为Faiss的官方支持和社区资源更偏向Linux和macOS）。虽然我在Windows上使用faiss-cpu时似乎没遇到对应问题，但sqlite-vss的作者确实提到过这方面的困难。可能是faiss-gpu版本的问题，不过作者也提到sqlite-vss不支持faiss-gpu版本。也许是Faiss已经更新了，因为sqlite-vss的作者从去年五月开始就没有更新了（核心代码已经两年没动过了）。

其次是资源消耗方面的考虑。Faiss为了追求极致性能，索引通常需要完全加载到内存中。sqlite-vss的文档中也提到其底层Faiss索引的大小上限（如1GB的示例），并明确指出索引必须适合RAM，这也就意味着内存消耗会比较大。我上一篇文章提到的duckdb支持的hnsw索引类型也是需要在内存里，除非开启实验版本，所以我才重新找了另一个实现方案，也就是sqlite-vec。

还有就是功能和维护上的局限。sqlite-vss的某些功能受限于Faiss的集成方式。比如，早期版本在KNN搜索之上进行额外过滤的支持不完善，仅支持CPU Faiss索引（不支持GPU加速），不支持内存映射（mmap）索引等。同时，由于依赖一个庞大且独立的外部库，sqlite-vss的开发迭代、问题修复以及与SQLite自身事务处理的兼容性都可能受到Faiss版本的制约和影响，这也增加了维护的复杂度。用户反馈中也提到sqlite-vss存在"各种与事务相关的bug和问题"，并且"编译极其困难且耗时"。

这些因素共同构成了sqlite-vss在轻量化和普适性方面的短板，促使社区和开发者开始寻找一种更为原生、更少依赖的SQLite向量检索解决方案。

## 🌱 sqlite-vec：一个极小、"足够快"的向量搜索 SQLite 扩展

面对sqlite-vss因依赖Faiss带来的各种不便，开发者社区中开始有人呼吁打造一个更简洁、更通用且无外部依赖的SQLite向量搜索扩展。在这样的背景下，sqlite-vec应运而生，它试图为SQLite的向量处理能力开创一个新的篇章。

sqlite-vec的设计初衷很明确：解决sqlite-vss的核心痛点。它采用纯C语言编写，不依赖任何外部大型库（如Faiss），从根本上解决了编译复杂性、平台兼容性以及运行时依赖的问题。sqlite-vec希望打造一个"可在任何支持SQLite的地方运行"的向量搜索扩展，让开发者能够轻松地在各种环境中为SQLite应用添加向量处理能力。

为了实现这一目标，sqlite-vec在设计上有很多特点，让它与sqlite-vss相比有一些明显的优势：

首先，它完全没有外部依赖。这是sqlite-vec最核心的改进。通过纯C语言实现所有核心功能，它摆脱了对Faiss等第三方库的依赖。这样一来，开发者在使用sqlite-vec时，就不用担心复杂的编译环境配置和外部库的版本兼容问题，大大简化了项目的构建、集成和后续分发流程。

其次，它有很好的跨平台能力。因为是纯C实现且没有依赖，sqlite-vec在不同平台上的兼容性非常好。它可以在Linux、macOS、Windows等主流桌面操作系统上顺畅运行，甚至可以在浏览器环境（通过WebAssembly, WASM）、树莓派（Raspberry Pi）等嵌入式设备上使用。不过需要注意的是，不同平台需要下载对应的扩展文件，比如Windows上就叫做vec0.dll，有兴趣的话可以看看我们演示仓库的代码，里面就有这个文件。

第三，它的使用体验非常友好。sqlite-vec在API设计上充分考虑了开发者的使用体验。它提供了直观易懂的自定义SQL函数，比如用于将JSON格式的向量数组转换为内部格式的vec_f32()函数，以及直接计算向量间距离的vec_distance_cosine()、vec_distance_L2()等函数。更重要的是，它引入了vec0虚拟表接口，让开发者可以通过标准的SQL DDL语句（CREATE VIRTUAL TABLE ... USING vec0）来创建和管理向量表，并通过简洁的SQL DML语句（如MATCH子句）进行高效的向量检索，这比手动操作BLOB数据和在应用层计算相似度要优雅和高效得多。

尽管追求轻量，sqlite-vec在功能上也比较完善。它支持存储和查询多种类型的向量数据，包括常见的32位浮点型（float32）、8位整型（int8）以及二进制向量。同时，vec0虚拟表的设计允许开发者在存储向量的同时，定义并存储相关的元数据列（如文档ID、类别标签、原始文本内容等），并可以在查询时利用这些元数据进行过滤或辅助排序，这对于构建实用的RAG应用非常重要。

虽然sqlite-vec可能无法在超大规模数据集的检索性能上与专门的、分布式的向量数据库集群相媲美，但它的设计目标是为中小型数据集提供"足够快"的向量检索性能。对于个人项目、原型验证、中小型应用或RAG流程中的特定轻量级检索节点而言，它的性能通常能够满足需求。

## 🛠️ 实践对比：SQLite中向量检索方案的选择

为了更直观地了解sqlite-vec带来的便利性和效率提升，我们来比较一下在SQLite中实现向量检索的两种主要方案：不使用任何扩展的纯SQLite与Python手动实现，以及基于sqlite-vec扩展的实现。

### 环境准备与基础设置

首先，我们需要设置环境、加载所需库并准备嵌入模型：

```
import sqlite3
import numpy as np
import pandas as pd
from sentence_transformers import SentenceTransformer
import os
import json
import pickle
from sklearn.metrics.pairwise import cosine_similarity, euclidean_distances
import time
import matplotlib.pyplot as plt

# 加载嵌入模型
try:
    # 首先尝试加载本地模型
    model_path = r&#x27;C:\Users\k\Desktop\BaiduSyncdisk\baidu_sync_documents\hf_models\bge-m3&#x27;
    model = SentenceTransformer(model_path)
    print(f&quot;成功加载本地模型: {model_path}&quot;)
except Exception as e:
    # 如果本地模型加载失败，使用Hugging Face上的小型模型
    print(f&quot;本地模型加载失败: {e}&quot;)
    print(&quot;尝试从Hugging Face加载多语言模型...&quot;)
    model = SentenceTransformer(&#x27;sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2&#x27;)
    print(&quot;成功加载Hugging Face模型&quot;)

# 获取模型的输出维度
embedding_dim = model.get_sentence_embedding_dimension()
print(f&quot;模型输出维度: {embedding_dim}&quot;)
```

输出:

```
成功加载本地模型: C:\Users\k\Desktop\BaiduSyncdisk\baidu_sync_documents\hf_models\bge-m3
模型输出维度: 1024
```

### 方案一：无扩展的纯SQLite与Python实现（"手动挡"）

如果我们没有使用专门的向量检索扩展（比如sqlite-vec或sqlite-vss），想在SQLite中存储和检索向量数据，通常需要在应用层面（比如用Python）做很多工作。

**创建数据库连接：**

```
# 创建一个SQLite数据库连接
no_vec0_db_path = &quot;vec_no_vec1.db&quot;
no_vec0_conn = sqlite3.connect(no_vec0_db_path)
no_vec0_cursor = no_vec0_conn.cursor()
```

**数据存储**：

向量数据（通常是浮点数列表或NumPy数组）需要先被序列化成SQLite能够直接存储的格式。常见的做法是使用Python的pickle模块将其序列化为字节串，然后存储在SQLite表的BLOB（Binary Large Object）类型列中。

```
# 创建表用于存储BLOB向量
no_vec0_cursor.execute(&quot;&quot;&quot;
CREATE TABLE IF NOT EXISTS vec_documents(
    document_id INTEGER PRIMARY KEY,
    content_embedding BLOB,
    category TEXT,
    original_content TEXT
);
&quot;&quot;&quot;)

# 定义插入函数
def no_vec0_insert_documents(cursor, documents):
    &quot;&quot;&quot;插入文档&quot;&quot;&quot;
    # 清空表
    cursor.execute(&quot;DELETE FROM vec_documents&quot;)
    
    for doc in documents:
        # 标准表插入
        embedding_bytes = pickle.dumps(doc[&quot;embedding&quot;])
        cursor.execute(
            &quot;INSERT INTO vec_documents(document_id, content_embedding, category, original_content) VALUES (?, ?, ?, ?)&quot;,
            (doc[&quot;id&quot;], embedding_bytes, doc[&quot;category&quot;], doc[&quot;content&quot;])
        )
    
    no_vec0_conn.commit()
    print(f&quot;成功插入 {len(documents)} 条文档&quot;)
```

**数据检索（KNN搜索）**：

当需要进行相似度搜索时，Python程序需要从SQLite中查询出所有记录，反序列化向量数据，并在应用层逐一计算相似度：

```
def no_vec0_knn_search(cursor, query_embedding, k=3, category=None):
    &quot;&quot;&quot;手动实现KNN搜索，针对没有vec0插件的情况&quot;&quot;&quot;
    results = []
    
    # 构建查询SQL
    if category:
        query = &quot;SELECT document_id, original_content, category, content_embedding FROM vec_documents WHERE category = ?&quot;
        cursor.execute(query, (category,))
    else:
        query = &quot;SELECT document_id, original_content, category, content_embedding FROM vec_documents&quot;
        cursor.execute(query)
    
    # 处理每一行数据
    for row in cursor.fetchall():
        doc_id, content, doc_category, embedding_bytes = row
        try:
            # 使用pickle反序列化embedding
            doc_embedding = pickle.loads(embedding_bytes)
            
            # 计算余弦相似度
            similarity = cosine_similarity([query_embedding], [doc_embedding])[0][0]
            # 计算距离（1 - 相似度，使得值越小越相似）
            distance = 1 - similarity
            results.append((doc_id, content, doc_category, distance))
        except Exception as e:
            print(f&quot;Error processing embedding for doc_id {doc_id}: {e}&quot;)
            continue
    
    # 排序并返回前k个结果
    results.sort(key=lambda x: x[3])  # 按距离排序
    return results[:k]
```

### 方案二：基于sqlite-vec扩展的向量检索（"自动挡"）

与手动方案形成鲜明对比的是使用sqlite-vec扩展。sqlite-vec通过引入vec0虚拟表和相关的SQL函数，将向量的存储和检索能力深度集成到SQLite的SQL引擎中。

环境配置与sqlite-vec加载：

```
# 创建一个SQLite数据库连接
db_path = &quot;vector_demo1.db&quot;
conn = sqlite3.connect(db_path)
cursor = conn.cursor()

# 尝试加载sqlite-vec扩展
try:
    conn.enable_load_extension(True)
    # 根据操作系统加载不同的扩展文件
    conn.execute(&quot;SELECT load_extension(&#x27;./vec0.dll&#x27;)&quot;)  # Windows系统
    # 测试扩展是否成功加载
    cursor.execute(&quot;SELECT vec_version()&quot;)
    version = cursor.fetchone()[0]
    print(f&quot;成功加载sqlite-vec扩展，版本: {version}&quot;)
except Exception as e:
    print(f&quot;加载sqlite-vec扩展失败: {e}&quot;)
    print(&quot;将使用纯Python实现向量操作&quot;)
    print(&quot;如需使用sqlite-vec扩展，请从 https://github.com/asg017/sqlite-vec/releases 下载对应版本&quot;)
```

输出:

```
成功加载sqlite-vec扩展，版本: v0.1.7-alpha.2
```

**定义向量存储结构**：

```
# 删除原来的表，如果存在
cursor.execute(&quot;DROP TABLE IF EXISTS vec_documents;&quot;)

# 创建表格
cursor.execute(f&quot;&quot;&quot;
CREATE VIRTUAL TABLE IF NOT EXISTS vec_documents USING vec0(
    document_id INTEGER PRIMARY KEY,
    content_embedding FLOAT[{embedding_dim}] DISTANCE_METRIC=cosine,
    category TEXT,
    +original_content TEXT
);
&quot;&quot;&quot;)
```

**文本嵌入与数据入库**：

```
def insert_documents(cursor, documents):
    # 清空表
    cursor.execute(&quot;DELETE FROM vec_documents&quot;)
    for doc in documents:
        # 虚拟表插入（如果可用）
        embedding_json = json.dumps(doc[&quot;embedding&quot;].tolist())
        try:
            cursor.execute(
                &quot;INSERT INTO vec_documents(document_id, content_embedding, category, original_content) VALUES (?, vec_f32(?), ?, ?)&quot;,
                (doc[&quot;id&quot;], embedding_json, doc[&quot;category&quot;], doc[&quot;content&quot;])
            )
        except Exception as e:
            print(f&quot;插入虚拟表失败: {e}&quot;)
    conn.commit()
```

执行高效向量相似度搜索（KNN）：

```
def knn_search(cursor, query_embedding, k=3, category=None):
    &quot;&quot;&quot;执行KNN搜索&quot;&quot;&quot;
    # 使用vec0虚拟表
    query_json = json.dumps(query_embedding.tolist())
    if category:
        cursor.execute(&quot;&quot;&quot;
        SELECT document_id, original_content, category, distance
        FROM vec_documents 
        WHERE content_embedding MATCH ? AND k = ? AND category = ?
        &quot;&quot;&quot;, (query_json, k, category))
    else:
        cursor.execute(&quot;&quot;&quot;
        SELECT document_id, original_content, category, distance
        FROM vec_documents 
        WHERE content_embedding MATCH ? AND k = ?
        &quot;&quot;&quot;, (query_json, k))
    
    return [(row[0], row[1], row[2], row[3]) for row in cursor.fetchall()]
```

### 测试两种方案

准备测试数据并运行两种方案：

```
# 准备一些示例文本
documents = [
    {&quot;id&quot;: 1, &quot;content&quot;: &quot;机器学习是人工智能的一个子领域&quot;, &quot;category&quot;: &quot;技术&quot;},
    {&quot;id&quot;: 2, &quot;content&quot;: &quot;深度学习是机器学习的一种方法&quot;, &quot;category&quot;: &quot;技术&quot;},
    {&quot;id&quot;: 3, &quot;content&quot;: &quot;向量数据库可以高效存储和检索向量数据&quot;, &quot;category&quot;: &quot;数据库&quot;},
    {&quot;id&quot;: 4, &quot;content&quot;: &quot;SQLite是一个轻量级的关系型数据库&quot;, &quot;category&quot;: &quot;数据库&quot;},
    {&quot;id&quot;: 5, &quot;content&quot;: &quot;Python是一种流行的编程语言&quot;, &quot;category&quot;: &quot;编程&quot;},
    {&quot;id&quot;: 6, &quot;content&quot;: &quot;自然语言处理是处理人类语言的技术&quot;, &quot;category&quot;: &quot;技术&quot;},
    {&quot;id&quot;: 7, &quot;content&quot;: &quot;向量相似度搜索在推荐系统中很常用&quot;, &quot;category&quot;: &quot;技术&quot;},
    {&quot;id&quot;: 8, &quot;content&quot;: &quot;大数据分析需要高效的数据存储和处理&quot;, &quot;category&quot;: &quot;数据&quot;}
]

# 为每个文档生成embedding向量
for doc in documents:
    embedding = model.encode(doc[&quot;content&quot;])
    doc[&quot;embedding&quot;] = embedding

# 展示部分数据
for doc in documents[:2]:
    print(f&quot;ID: {doc[&#x27;id&#x27;]}, 内容: {doc[&#x27;content&#x27;]}&quot;)
    print(f&quot;向量维度: {len(doc[&#x27;embedding&#x27;])}, 向量前几个元素: {doc[&#x27;embedding&#x27;][:5]}...\n&quot;)
```

输出:

```
ID: 1, 内容: 机器学习是人工智能的一个子领域
向量维度: 1024, 向量前几个元素: [-0.03649624 -0.02476022 -0.04679906 -0.00498728  0.00767139]...

ID: 2, 内容: 深度学习是机器学习的一种方法
向量维度: 1024, 向量前几个元素: [-0.02456879 -0.06428009 -0.04240257 -0.00734466 -0.02539387]...
```

插入数据：

```
# 使用sqlite-vec插入
insert_documents(cursor, documents)

# 使用手动方式插入
no_vec0_insert_documents(no_vec0_cursor, documents)
```

执行查询：

```
# 生成查询向量
query_text = &quot;数据库技术与应用&quot;
query_embedding = model.encode(query_text)

# 使用sqlite-vec执行KNN查询
k = 3
results = knn_search(cursor, query_embedding, k)

print(f&quot;查询：&#x27;{query_text}&#x27;的最近{k}个结果 (使用sqlite-vec):&quot;)
for doc_id, content, category, distance in results:
    print(f&quot;ID: {doc_id}, 距离: {distance:.4f}, 类别: {category}, 内容: {content}&quot;)

# 使用手动方式执行KNN查询
results = no_vec0_knn_search(no_vec0_cursor, query_embedding, k)
print(f&quot;\n查询：&#x27;{query_text}&#x27;的最近{k}个结果 (手动实现):&quot;)
for doc_id, content, category, distance in results:
    print(f&quot;ID: {doc_id}, 距离: {distance:.4f}, 类别: {category}, 内容: {content}&quot;)
```

输出:

```
查询：&#x27;数据库技术与应用&#x27;的最近3个结果 (使用sqlite-vec):
ID: 3, 距离: 0.4323, 类别: 数据库, 内容: 向量数据库可以高效存储和检索向量数据
ID: 8, 距离: 0.4413, 类别: 数据, 内容: 大数据分析需要高效的数据存储和处理
ID: 4, 距离: 0.4911, 类别: 数据库, 内容: SQLite是一个轻量级的关系型数据库

查询：&#x27;数据库技术与应用&#x27;的最近3个结果 (手动实现):
ID: 3, 距离: 0.4323, 类别: 数据库, 内容: 向量数据库可以高效存储和检索向量数据
ID: 8, 距离: 0.4413, 类别: 数据, 内容: 大数据分析需要高效的数据存储和处理
ID: 4, 距离: 0.4911, 类别: 数据库, 内容: SQLite是一个轻量级的关系型数据库
```

从结果可以看出，两种方法返回的结果是一致的，但是实现的复杂度和性能却有很大差异。

### 性能对比测试

接下来，我们测试两种方案在不同数据量下的性能表现：

```
# 生成随机测试数据
def generate_test_data(num_samples, vec_dim=1024, categories=[&quot;技术&quot;, &quot;数据库&quot;, &quot;编程&quot;, &quot;数据&quot;]):
    &quot;&quot;&quot;生成随机测试数据&quot;&quot;&quot;
    print(f&quot;生成{num_samples}条随机测试数据...&quot;)
    test_data = []
    for i in range(1, num_samples + 1):
        # 生成随机向量
        vec = np.random.rand(vec_dim).astype(np.float32)
        # 归一化向量
        vec = vec / np.linalg.norm(vec)
        
        # 创建随机内容
        content = f&quot;这是第{i}个随机测试文档&quot;
        category = random.choice(categories)
        
        test_data.append({
            &quot;id&quot;: i,
            &quot;content&quot;: content,
            &quot;category&quot;: category,
            &quot;embedding&quot;: vec
        })
    
    return test_data

# 批量插入函数
def batch_insert_documents(conn, cursor, documents, use_extension=True):
    &quot;&quot;&quot;批量插入向量数据&quot;&quot;&quot;
    start_time = time.time()
    
    count = len(documents)
    print(f&quot;开始插入{count}条记录...&quot;)
    
    # 清空表
    if use_extension:
        cursor.execute(&quot;DELETE FROM vec_documents&quot;)
        # 使用虚拟表插入
        for i, doc in enumerate(documents):
            embedding_json = json.dumps(doc[&quot;embedding&quot;].tolist())
            try:
                cursor.execute(
                    &quot;INSERT INTO vec_documents(document_id, content_embedding, category, original_content) VALUES (?, vec_f32(?), ?, ?)&quot;,
                    (doc[&quot;id&quot;], embedding_json, doc[&quot;category&quot;], doc[&quot;content&quot;])
                )
                # 每插入100条记录打印一次进度
                if (i+1) % 100 == 0:
                    print(f&quot;已插入 {i+1}/{count} 条记录...&quot;)
            except Exception as e:
                print(f&quot;插入虚拟表失败: {e}&quot;)
                break
    else:
        cursor.execute(&quot;DELETE FROM vec_documents&quot;)
        # 不使用扩展插入
        for i, doc in enumerate(documents):
            embedding_bytes = pickle.dumps(doc[&quot;embedding&quot;])
            cursor.execute(
                &quot;INSERT INTO vec_documents(document_id, content_embedding, category, original_content) VALUES (?, ?, ?, ?)&quot;,
                (doc[&quot;id&quot;], embedding_bytes, doc[&quot;category&quot;], doc[&quot;content&quot;])
            )
            # 每插入100条记录打印一次进度
            if (i+1) % 100 == 0:
                print(f&quot;已插入 {i+1}/{count} 条记录...&quot;)
    
    conn.commit()
    end_time = time.time()
    print(f&quot;插入完成，共耗时 {end_time - start_time:.2f} 秒&quot;)
    return end_time - start_time

# 测试查询性能
def test_query_performance(conn, cursor, query_embeddings, k=5, use_extension=True):
    &quot;&quot;&quot;测试查询性能&quot;&quot;&quot;
    times = []
    
    count = len(query_embeddings)
    print(f&quot;开始执行{count}次查询...&quot;)
    
    for i, query_embedding in enumerate(query_embeddings):
        start_time = time.time()
        
        if use_extension:
            # 使用扩展的查询
            query_json = json.dumps(query_embedding.tolist())
            cursor.execute(&quot;&quot;&quot;
            SELECT document_id, original_content, category, distance
            FROM vec_documents 
            WHERE content_embedding MATCH ? AND k = ?
            &quot;&quot;&quot;, (query_json, k))
            _ = cursor.fetchall()
        else:
            # 不使用扩展的查询
            results = []
            cursor.execute(&quot;SELECT document_id, original_content, category, content_embedding FROM vec_documents&quot;)
            
            for row in cursor.fetchall():
                doc_id, content, doc_category, embedding_bytes = row
                try:
                    # 使用pickle反序列化embedding
                    doc_embedding = pickle.loads(embedding_bytes)
                    
                    # 计算余弦相似度
                    similarity = cosine_similarity([query_embedding], [doc_embedding])[0][0]
                    # 计算距离（1 - 相似度）
                    distance = 1 - similarity
                    results.append((doc_id, content, doc_category, distance))
                except Exception as e:
                    continue
            
            # 排序并选择前k个结果
            results.sort(key=lambda x: x[3])
            top_k_results = results[:k]
        
        end_time = time.time()
        times.append(end_time - start_time)
        
        # 每执行5次查询打印一次进度
        if (i+1) % 5 == 0:
            print(f&quot;已执行 {i+1}/{count} 次查询...&quot;)
    
    avg_time = np.mean(times)
    print(f&quot;查询完成，平均每次查询耗时 {avg_time:.4f} 秒&quot;)
    return avg_time
```

执行性能测试：

```
# 配置字体
plt.rcParams[&#x27;font.sans-serif&#x27;] = [&#x27;SimHei&#x27;]  # 设置字体为黑体
plt.rcParams[&#x27;axes.unicode_minus&#x27;] = False  # 显示负号

# 测试不同数据规模
data_sizes = [100, 500, 1000, 10000]
insert_times_with_extension = []
insert_times_without_extension = []
query_times_with_extension = []
query_times_without_extension = []

# 随机查询向量
num_queries = 5
print(f&quot;为每种测试情况生成{num_queries}个随机查询向量...&quot;)
query_embeddings = [np.random.rand(embedding_dim).astype(np.float32) for _ in range(num_queries)]
query_embeddings = [vec / np.linalg.norm(vec) for vec in query_embeddings]

# 重置两个数据库的表
cursor.execute(&quot;DROP TABLE IF EXISTS vec_documents;&quot;)
cursor.execute(f&quot;&quot;&quot;
CREATE VIRTUAL TABLE IF NOT EXISTS vec_documents USING vec0(
    document_id INTEGER PRIMARY KEY,
    content_embedding FLOAT[{embedding_dim}] DISTANCE_METRIC=cosine,
    category TEXT,
    +original_content TEXT
);
&quot;&quot;&quot;)

no_vec0_cursor.execute(&quot;DROP TABLE IF EXISTS vec_documents;&quot;)
no_vec0_cursor.execute(f&quot;&quot;&quot;
CREATE TABLE IF NOT EXISTS vec_documents(
    document_id INTEGER PRIMARY KEY,
    content_embedding BLOB,
    category TEXT,
    original_content TEXT
);
&quot;&quot;&quot;)

print(&quot;表创建成功，开始性能测试...&quot;)

# 测试不同数据规模
for size in data_sizes:
    print(f&quot;\n测试数据规模: {size}&quot;)
    # 生成测试数据
    import random
    test_data = generate_test_data(size)
    
    # 测试使用扩展的插入性能
    print(&quot;\n使用扩展测试:&quot;)
    insert_time = batch_insert_documents(conn, cursor, test_data, use_extension=True)
    insert_times_with_extension.append(insert_time)
    
    # 测试使用扩展的查询性能
    query_time = test_query_performance(conn, cursor, query_embeddings[:3], k=5, use_extension=True)
    query_times_with_extension.append(query_time)
    
    # 测试不使用扩展的插入性能
    print(&quot;\n不使用扩展测试:&quot;)
    insert_time = batch_insert_documents(no_vec0_conn, no_vec0_cursor, test_data, use_extension=False)
    insert_times_without_extension.append(insert_time)
    
    # 测试不使用扩展的查询性能
    query_time = test_query_performance(no_vec0_conn, no_vec0_cursor, query_embeddings[:3], k=5, use_extension=False)
    query_times_without_extension.append(query_time)

# 可视化结果
plt.figure(figsize=(14, 8))

# 创建插入时间对比图
plt.subplot(1, 2, 1)
plt.plot(data_sizes, insert_times_with_extension, &#x27;o-&#x27;, label=&#x27;使用扩展&#x27;)
plt.plot(data_sizes, insert_times_without_extension, &#x27;s-&#x27;, label=&#x27;不使用扩展&#x27;)
plt.title(&#x27;向量数据插入性能对比&#x27;)
plt.xlabel(&#x27;数据量（记录数）&#x27;)
plt.ylabel(&#x27;插入时间（秒）&#x27;)
plt.grid(True)
plt.legend()

# 创建查询时间对比图
plt.subplot(1, 2, 2)
plt.plot(data_sizes, query_times_with_extension, &#x27;o-&#x27;, label=&#x27;使用扩展&#x27;)
plt.plot(data_sizes, query_times_without_extension, &#x27;s-&#x27;, label=&#x27;不使用扩展&#x27;)
plt.title(&#x27;向量查询性能对比&#x27;)
plt.xlabel(&#x27;数据量（记录数）&#x27;)
plt.ylabel(&#x27;平均查询时间（秒）&#x27;)
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.savefig(&#x27;sqlite_vec_performance.png&#x27;)
plt.show()

# 计算性能提升比例
for i, size in enumerate(data_sizes):
    if (insert_times_with_extension[i] &gt; 0 and 
        insert_times_without_extension[i] &gt; 0):
        insert_ratio = insert_times_without_extension[i] / insert_times_with_extension[i]
        print(f&quot;数据量 {size}: 插入速度比例 {insert_ratio:.2f}x&quot;)
    
    if (query_times_with_extension[i] &gt; 0 and 
        query_times_without_extension[i] &gt; 0):
        query_ratio = query_times_without_extension[i] / query_times_with_extension[i]
        print(f&quot;数据量 {size}: 查询速度提升 {query_ratio:.2f}x&quot;)
```

部分输出结果:

```
测试数据规模: 100
使用扩展测试:
开始插入100条记录...
已插入 100/100 条记录...
插入完成，共耗时 0.12 秒
开始执行3次查询...
已执行 3/3 次查询...
查询完成，平均每次查询耗时 0.0077 秒

不使用扩展测试:
开始插入100条记录...
已插入 100/100 条记录...
插入完成，共耗时 0.01 秒
开始执行3次查询...
已执行 3/3 次查询...
查询完成，平均每次查询耗时 0.0433 秒

测试数据规模: 10000
使用扩展测试:
开始插入10000条记录...
已插入 100/10000 条记录...
...
已插入 10000/10000 条记录...
插入完成，共耗时 21.63 秒
开始执行3次查询...
已执行 3/3 次查询...
查询完成，平均每次查询耗时 0.0616 秒

不使用扩展测试:
开始插入10000条记录...
已插入 100/10000 条记录...
...
已插入 10000/10000 条记录...
插入完成，共耗时 0.50 秒
开始执行3次查询...
已执行 3/3 次查询...
查询完成，平均每次查询耗时 5.3996 秒

数据量 100: 插入速度比例 0.07x
数据量 100: 查询速度提升 5.64x
数据量 500: 插入速度比例 0.08x
数据量 500: 查询速度提升 20.81x
数据量 1000: 插入速度比例 0.06x
数据量 1000: 查询速度提升 61.29x
数据量 10000: 插入速度比例 0.02x
数据量 10000: 查询速度提升 87.70x
```

![](img/895fbd8a2ad6f0b8e568fcfffec3a53e.png)

![](img/4d0dbfa708d45238c5cfa9a0602523b6.png)

### 性能测试结果分析

通过这次性能测试，我们发现了一些有趣的趋势：

关于插入性能，在数据量比较小（100-1000条记录）的情况下，不使用扩展的方式插入速度反而更快。这可能是因为sqlite-vec在插入时需要将JSON格式的向量转换为内部格式，而pickle序列化相对简单一些。不过，随着数据量增加到10000条以上，这种差距变得不太明显，因为IO操作和事务提交的开销占比增加了。

但在查询性能上，sqlite-vec的优势非常明显，而且随着数据规模增加，这种优势越来越大。对于100条记录的情况，sqlite-vec的查询速度大约是手动方案的5.64倍。当数据量达到1000条时，这个差距扩大到了61倍。在10000条记录的测试中，sqlite-vec的查询速度甚至达到了手动方案的88倍！

从扩展性角度来看，无扩展方案的查询时间随着数据量的增加呈接近线性增长（接近O(n)复杂度），这是因为每次查询都需要扫描所有记录并计算相似度。而使用sqlite-vec的查询时间增长速度则明显较慢，显示出它内部实现的优化价值。

这些结果表明，虽然在小数据量的简单插入操作上不使用扩展可能更快一些，但在真正重要的查询性能上，sqlite-vec提供了显著的优势。随着数据规模增长，这种优势会变得越来越明显，使它成为中小型RAG应用的理想选择。

手动方案最大的问题显然是查询性能。当数据量增大时，全表扫描和应用层计算的开销会线性增长，很快就会达到无法接受的程度。而且，由于SQLite数据库本身无法理解BLOB中向量的语义，所以无法利用任何索引或其他数据库层面的优化手段来加速向量检索。

相比之下，sqlite-vec的优势非常明显：它大大简化了开发过程，向量的存储、索引和检索逻辑被封装在扩展内部，开发者只需编写简洁的SQL语句；它的查询语法（如MATCH）清晰易懂，容易使用；最重要的是，它在查询方面提供了显著的性能提升，这种优势随着数据量增加而变得更加明显；此外，它还能充分利用SQLite的能力，将向量检索功能无缝集成到SQL查询引擎中，方便地与其他SQL功能结合使用。

通过这个对比，我们可以看到，sqlite-vec通过提供原生的SQLite扩展，将复杂的向量检索操作从应用层下沉到数据库扩展层，不仅大幅简化了开发工作，还带来了随数据规模增加而越发明显的性能提升。

## 💡 sqlite-vec在RAG架构中的应用价值

sqlite-vec的出现，为那些资源有限或追求简洁技术栈的RAG应用提供了一个很有吸引力的选择。它的价值主要体现在以下几个方面：

首先，它能大幅简化RAG的技术栈。在很多RAG项目中，特别是中小型项目或原型验证阶段，引入一个独立的、功能完备的向量数据库服务（如Elasticsearch配合向量插件、Weaviate或Milvus、Postgresql等）可能会带来很多额外工作，包括部署、配置、监控和维护。而sqlite-vec则允许开发者在现有的或新建的SQLite数据库基础上直接添加向量检索功能。这样整个RAG系统就可以依赖更少的外部组件，技术栈更加精简，从而降低系统的复杂度和维护压力。对于个人开发者或小型团队来说，这一点特别重要。

其次，它提供了更好的数据一致性和管理便捷性。RAG应用通常需要同时处理结构化或半结构化的元数据（如文档来源、创建时间、类别标签等）以及非结构化文本内容及其向量嵌入。当这些数据都存储在同一个SQLite数据库中时，数据管理会变得更加方便。例如，对元数据和向量数据的更新操作可以更容易地纳入SQLite的事务管理，确保数据的一致性；整个知识库可以作为一个单一的SQLite数据库文件进行备份、迁移或共享，操作非常简单；虽然vec0是虚拟表，但它的设计允许与SQLite的其他功能一起工作，所以理论上可以更容易地在检索出的向量结果基础上，用SQL进行更复杂的元数据关联查询或聚合分析。这里也解释了我为什么不用chromadb做向量数据库的原因，因为我需要sqlite这类关系型数据同时支持我对向量进行对应的元数据筛选管理，虽然可以维护一个映射关系来实现，但终归是让系统变得更复杂了……

此外，sqlite-vec的资源消耗相对较低，性价比高。它本身是一个轻量级的C扩展，对系统资源的占用远小于很多专用的向量数据库服务。它不需要独立的服务器进程，内存消耗也相对可控，尤其是与需要将大型索引完全载入内存的Faiss相比。这使得sqlite-vec非常适合部署在资源有限的环境中，比如边缘计算设备（如树莓派、工业计算机等）、个人桌面应用（如个人知识管理工具、本地文档搜索引擎）以及小型Web服务或微服务。对于并发量不高、数据规模适中的RAG服务，特别是适用个人使用场景的软件，sqlite-vec可以提供一个很好的解决方案，避免了过度使用资源。

---

> **🙋♂️ 入群交流**

+   公众号菜单点击「社群」，扫码直接入群

+   回复关键词「入群」，添加作者微信人工邀请，注意备注：入群

## 📒 总结与展望

核心要点回顾

+   RAG架构需要高效轻量的向量检索组件，尤其在资源有限场景。

+   sqlite-vss依赖Faiss导致部署和资源问题。

+   sqlite-vec无依赖设计为SQLite提供原生向量检索能力。

+   性能测试显示sqlite-vec查询速度最高可达传统方法的88倍。

+   sqlite-vec插入性能在小数据量时反而更慢，但随数据增加差距变小，查询优势明显。

---

**参考资料**

1.  sqlite-vec[ GitHub Reposito](https://github.com/asg017/sqlite-vec)ry: https://github.com/asg017/sqlite-vec (项目官方源码、文档和最新动态)

1.  sqlite-vss[ GitHub Reposito](https://github.com/asg017/sqlite-vss)ry: https://github.com/asg017/sqlite-vss (了解其设计与局限性，作为对比参考)

1.  [Faiss GitHub Reposito](https://github.com/facebookresearch/faiss)ry: https://github.com/facebookresearch/faiss (了解sqlite-vss所依赖的核心向量搜索库)

1.  Alex Garcia's Blog [Post on sqlite-vec: https://alexgarci](https://alexgarcia.xyz/blog/2024/building-new-vector-search-sqlite/index.html)a.xyz/blog/2024/building-new-vector-search-sqlite/index.html (sqlite-vec作者之一关于构建该工具的 insightful 文章)

1.  SentenceTransformers Documentation: https://www.sbert.net/ (了解文中所用文本嵌入模型的背景和使用方法)

> 欢迎在留言区分享你的想法，每条留言我都会认真阅读！

> 你的反馈是我创作的最大动力 ❤️

> **💗 立即行动**

1.  点赞、收藏、关注，分享给朋友。

1.  为账号加星标，获取更多内容。

1.  以本文为灵感，整理一份属于自己的笔记。

> **🚗 行动召唤**

> "与其等着 AI 改变世界，不如自己参与变革！在这里，让 AI 成为你弯道超车的秘密武器。"

## 📚 往期精选

+   [摆脱Faiss束缚：sqlite-vec如何在RA](https://mp.weixin.qq.com/s/-Fvs-3Twi5YB5hUc9aBqtg)G中实现SQLite的原生向量检索轻量化

+   [轻量化数据库向量检索DuckDB VSS 入门指南：从](https://mp.weixin.qq.com/s/-wc805MKj34GlWEtnYZ6cA)安装到 HNSW 索引优化，看这篇就够了

+   [训练一个tokenizer？理解 GPT/BERT 的](https://mp.weixin.qq.com/s/3OF8BtIJAfTE3-nJMZY8ww)文本处理，BPE 分词器的工作机制（2）

+   [训练一个tokenizer？理解 GPT/BER](https://mp.weixin.qq.com/s/FBpbtx4vTEdpMsZMGoL5qA)T 的文本处理，BPE 分词器的工作机制

+   [（万字多图警告）不懂损失函数，别说你会NLP！从交叉熵到](https://mp.weixin.qq.com/s/DTSvfR-ADanyGXlf6NjZfg)SimCSE，一篇文章搞懂模型的底层逻辑