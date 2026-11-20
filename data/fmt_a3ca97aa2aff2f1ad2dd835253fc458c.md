
## 性能测试代码

```python
# 配置字体
import matplotlib.pyplot as plt
import numpy as np

plt.rcParams['font.sans-serif'] = ['SimHei']  # 设置字体为黑体
plt.rcParams['axes.unicode_minus'] = False  # 显示负号

# 测试不同数据规模
data_sizes = [100, 500, 1000, 10000]
insert_times_with_extension = []
insert_times_without_extension = []
query_times_with_extension = []
query_times_without_extension = []

# 随机查询向量
num_queries = 5
print(f"为每种测试情况生成{num_queries}个随机查询向量...")
query_embeddings = [np.random.rand(embedding_dim).astype(np.float32) for _ in range(num_queries)]
query_embeddings = [vec / np.linalg.norm(vec) for vec in query_embeddings]

# 重置两个数据库的表
cursor.execute("DROP TABLE IF EXISTS vec_documents;")
cursor.execute(f"""
CREATE VIRTUAL TABLE IF NOT EXISTS vec_documents USING vec0(
    document_id INTEGER PRIMARY KEY,
    content_embedding FLOAT[{embedding_dim}] DISTANCE_METRIC=cosine,
    category TEXT,
    +original_content TEXT
);
""")

no_vec0_cursor.execute("DROP TABLE IF EXISTS vec_documents;")
no_vec0_cursor.execute(f"""
CREATE TABLE IF NOT EXISTS vec_documents(
    document_id INTEGER PRIMARY KEY,
    content_embedding BLOB,
    category TEXT,
    original_content TEXT
);
""")

print("表创建成功，开始性能测试...")

# 测试不同数据规模
for size in data_sizes:
    print(f"\n测试数据规模: {size}")
    # 生成测试数据
    import random
    test_data = generate_test_data(size)
    
    # 测试使用扩展的插入性能
    print("\n使用扩展测试:")
    insert_time = batch_insert_documents(conn, cursor, test_data, use_extension=True)
    insert_times_with_extension.append(insert_time)
    
    # 测试使用扩展的查询性能
    query_time = test_query_performance(conn, cursor, query_embeddings[:3], k=5, use_extension=True)
    query_times_with_extension.append(query_time)
    
    # 测试不使用扩展的插入性能
    print("\n不使用扩展测试:")
    insert_time = batch_insert_documents(no_vec0_conn, no_vec0_cursor, test_data, use_extension=False)
    insert_times_without_extension.append(insert_time)
    
    # 测试不使用扩展的查询性能
    query_time = test_query_performance(no_vec0_conn, no_vec0_cursor, query_embeddings[:3], k=5, use_extension=False)
    query_times_without_extension.append(query_time)

# 可视化结果
plt.figure(figsize=(14, 8))

# 创建插入时间对比图
plt.subplot(1, 2, 1)
plt.plot(data_sizes, insert_times_with_extension, 'o-', label='使用扩展')
plt.plot(data_sizes, insert_times_without_extension, 's-', label='不使用扩展')
plt.title('向量数据插入性能对比')
plt.xlabel('数据量（记录数）')
plt.ylabel('插入时间（秒）')
plt.grid(True)
plt.legend()

# 创建查询时间对比图
plt.subplot(1, 2, 2)
plt.plot(data_sizes, query_times_with_extension, 'o-', label='使用扩展')
plt.plot(data_sizes, query_times_without_extension, 's-', label='不使用扩展')
plt.title('向量查询性能对比')
plt.xlabel('数据量（记录数）')
plt.ylabel('平均查询时间（秒）')
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.savefig('sqlite_vec_performance.png')
plt.show()

# 计算性能提升比例
for i, size in enumerate(data_sizes):
    if (insert_times_with_extension[i] > 0 and 
        insert_times_without_extension[i] > 0):
        insert_ratio = insert_times_without_extension[i] / insert_times_with_extension[i]
        print(f"数据量 {size}: 插入速度比例 {insert_ratio:.2f}x")
    
    if (query_times_with_extension[i] > 0 and 
        query_times_without_extension[i] > 0):
        query_ratio = query_times_without_extension[i] / query_times_with_extension[i]
        print(f"数据量 {size}: 查询速度提升 {query_ratio:.2f}x")
```

## 测试输出示例

```text
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

关于插入性能，在数据量比较小（100-1000条记录）的情况下，不使用扩展的方式插入速度反而更快。这可能是因为`sqlite-vec`在插入时需要将JSON格式的向量转换为内部格式，而`pickle`序列化相对简单一些。不过，随着数据量增加到10000条以上，这种差距变得不太明显，因为IO操作和事务提交的开销占比增加了。

但在查询性能上，`sqlite-vec`的优势非常明显，而且随着数据规模增加，这种优势越来越大。对于100条记录的情况，`sqlite-vec`的查询速度大约是手动方案的5.64倍。当数据量达到1000条时，这个差距扩大到了61倍。在10000条记录的测试中，`sqlite-vec`的查询速度甚至达到了手动方案的88倍！

从扩展性角度来看，无扩展方案的查询时间随着数据量的增加呈接近线性增长（接近`O(n)`复杂度），这是因为每次查询都需要扫描所有记录并计算相似度。而使用`sqlite-vec`的查询时间增长速度则明显较慢，显示出它内部实现的优化价值。

这些结果表明，虽然在小数据量的简单插入操作上不使用扩展可能更快一些，但在真正重要的查询性能上，`sqlite-vec`提供了显著的优势。随着数据规模增长，这种优势会变得越来越明显，使它成为中小型RAG应用的理想选择。

手动方案最大的问题显然是查询性能。当数据量增大时，全表扫描和应用层计算的开销会线性增长，很快就会达到无法接受的程度。而且，由于`SQLite`数据库本身无法理解`BLOB`中向量的语义，所以无法利用任何索引或其他数据库层面的优化手段来加速向量检索。

相比之下，`sqlite-vec`的优势非常明显：它大大简化了开发过程，向量的存储、索引和检索逻辑被封装在扩展内部，开发者只需编写简洁的`SQL`语句；它的查询语法（如`MATCH`）清晰易懂，容易使用；最重要的是，它在查询方面提供了显著的性能提升，这种优势随着数据量增加而变得更加明显；此外，它还能充分利用`SQLite`的能力，将向量检索功能无缝集成到`SQL`查询引擎中，方便地与其他`SQL`功能结合使用。

通过这个对比，我们可以看到，`sqlite-vec`通过提供原生的`SQLite`扩展，将复杂的向量检索操作从应用层下沉到数据库扩展层，不仅大幅简化了开发工作，还带来了随数据规模增加而越发明显的性能提升。

## 💡 sqlite-vec在RAG架构中的应用价值

`sqlite-vec`的出现，为那些资源有限或追求简洁技术栈的RAG应用提供了一个很有吸引力的选择。它的价值主要体现在以下几个方面：

首先，它能大幅简化RAG的技术栈。在很多RAG项目中，特别是中小型项目或原型验证阶段，引入一个独立的、功能完备的向量数据库服务（如`Elasticsearch`配合向量插件、`Weaviate`或`Milvus`、`Postgresql`等）可能会带来很多额外工作，包括部署、配置、监控和维护。而`sqlite-vec`则允许开发者在现有的或新建的`SQLite`数据库基础上直接添加向量检索功能。这样整个RAG系统就可以依赖更少的外部组件，技术栈更加精简，从而降低系统的复杂度和维护压力。对于个人开发者或小型团队来说，这一点特别重要。

其次，它提供了更好的数据一致性和管理便捷性。RAG应用通常需要同时处理结构化或半结构化的元数据（如文档来源、创建时间、类别标签等）以及非结构化文本内容及其向量嵌入。当这些数据都存储在同一个`SQLite`数据库中时，数据管理会变得更加方便。例如，对元数据和向量数据的更新操作可以更容易地纳入`SQLite`的事务管理，确保数据的一致性；整个知识库可以作为一个单一的`SQLite`数据库文件进行备份、迁移或共享，操作非常简单；虽然`vec0`是虚拟表，但它的设计允许与`SQLite`的其他功能一起工作，所以理论上可以更容易地在检索出的向量结果基础上，用`SQL`进行更复杂的元数据关联查询或聚合分析。这里也解释了我为什么不用`chromadb`做向量数据库的原因，因为我需要`SQLite`这类关系型数据同时支持我对向量进行对应的元数据筛选管理，虽然可以维护一个映射关系来实现，但终归是让系统变得更复杂了……

此外，`sqlite-vec`的资源消耗相对较低，性价比高。它本身是一个轻量级的`C`扩展，对系统资源的占用远小于很多专用的向量数据库服务。它不需要独立的服务器进程，内存消耗也相对可控，尤其是与需要将大型索引完全载入内存的`Faiss`相比。这使得`sqlite-vec`非常适合部署在资源有限的环境中，比如边缘计算设备（如树莓派、工业计算机等）、个人桌面应用（如个人知识管理工具、本地文档搜索引擎）以及小型Web服务或微服务。对于并发量不高、数据规模适中的RAG服务，特别是适用个人使用场景的软件，`sqlite-vec`可以提供一个很好的解决方案，避免了过度使用资源。

---

> 🙋♂️ 入群交流
> 
> +   👸号菜单点击「社群」，扫码直接入群
> 
> +   回复关键词「入群」，添加作者🛰️人工邀请，注意备注：入群

## 📒 总结与展望

核心要点回顾

+   RAG架构需要高效轻量的向量检索组件，尤其在资源有限场景。
+   `sqlite-vss`依赖`Faiss`导致部署和资源问题。
+   `sqlite-vec`无依赖设计为`SQLite`提供原生向量检索能力。
+   性能测试显示`sqlite-vec`查询速度最高可达传统方法的88倍。
+   `sqlite-vec`插入性能在小数据量时反而更慢，但随数据增加差距变小，查询优势明显。

### 参考资料

1.  `sqlite-vec` [GitHub Repository](https://github.com/asg017/sqlite-vec): https://github.com/asg017/sqlite-vec (项目官方源码、文档和最新动态)

1.  `sqlite-vss` [GitHub Repository](https://github.com/asg017/sqlite-vss): https://github.com/asg017/sqlite-vss (了解其设计与局限性，作为对比参考)

1.  `Faiss` [GitHub Repository](https://github.com/facebookresearch/faiss): https://github.com/facebookresearch/faiss (了解`sqlite-vss`所依赖的核心向量搜索库)

1.  Alex Garcia's Blog [Post on `sqlite-vec`](https://alexgarcia.xyz/blog/2024/building-new-vector-search-sqlite/index.html): https://alexgarcia.xyz/blog/2024/building-new-vector-search-sqlite/index.html (`sqlite-vec`作者之一关于构建该工具的 insightful 文章)

1.  `SentenceTransformers` Documentation: https://www.sbert.net/ (了解文中所用文本嵌入模型的背景和使用方法)

> 欢迎在留言区分享你的想法，每条留言我都会认真阅读！
> 你的反馈是我创作的最大动力 ❤️
> **💗 立即行动**
> 
> 1.  点赞、收藏、关注，分享给朋友。
> 
> 1.  为账号加星标，获取更多内容。
> 
> 1.  以本文为灵感，整理一份属于自己的笔记。
> 
> **🚗 行动召唤**
> 
> "与其等着 AI 改变世界，不如自己参与变革！在这里，让 AI 成为你弯道超车的秘密武器。"

## 📚 往期精选

+   [摆脱`Faiss`束缚：`sqlite-vec`如何在RAG中实现`SQLite`的原生向量检索轻量化](https://mp.weixin.qq.com/s/-Fvs-3Twi5YB5hUc9aBqtg)
+   [轻量化数据库向量检索`DuckDB VSS` 入门指南：从安装到 `HNSW` 索引优化，看这篇就够了](https://mp.weixin.qq.com/s/-wc805MKj34GlWEtnYZ6cA)
+   [训练一个`tokenizer`？理解 `GPT`/`BERT` 的文本处理，`BPE` 分词器的工作机制（2）](https://mp.weixin.qq.com/s/3OF8BtIJAfTE3-nJMZY8ww)
