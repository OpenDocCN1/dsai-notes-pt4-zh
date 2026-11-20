
# RAG中Rerank重排序

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/AXkDdr2vDoxvGWx4iomcU6h6nuh](https://qqs7y1hozd1.feishu.cn/docx/AXkDdr2vDoxvGWx4iomcU6h6nuh)

## RAG & Rerank

目前大模型应用中，RAG（Retrieval Augmented Generation，检索增强生成）是一种在对话（QA）场景下最主要的应用形式，它主要解决大模型的知识存储和更新问题。

简述`RAG without Rerank`的主要过程：

![](img/78b4e79371e4f535082e185c77bb905d.png)

从向量库或其他存储召回的相关`chunk list`会按照检索时指定的距离计算公式由近及远排列，或者假设相似度得分记为`distance_score`，按`distance_score`大小从大到小排列。

![](img/af64937adc486d2fbc7a9ee4da590082.png)

>`Rerank`指的是在检索结果的排序基础之上再排一次序。

>将对生成回复真正重要的`chunk`排在前面，排除干扰项，可以类比理解为推荐算法的粗排和精排过程。

## 为什么需要Rerank？

>召回阶段使用向量库进行召回时，要求快速在大规模数据中检索到相关项。

>该过程需要度量当前`question`和库内全部向量的相似度，按指定窗口大小得到`top x`结果。也就是说，召回其实是个穷举过程，那么必然不能使用复杂度特别高的算法来计算相似度，为了召回的性能牺牲了召回的精度。

![](img/b022b559cec35cf8bce79a2802a77cf4.png)

`Rerank`的概念在大模型之前就已经出现，比如推荐算法和搜索算法的精排过程，可以理解为在粗排结果的基础之上进行`rerank`（精排）。

RAG的`Rerank`必要性体现在3个方面：

1. **精度提升**：

>基于`embedding`的向量化检索过程可以通过一定程度的语义相似度来高效检索相关性较高的文本片段。

>但由于语义本身的复杂性和多义性，以及高维向量相似度匹配可能产生的噪音，向量检索可能会召回一些相关性较低的候选项。

>因而引入`rerank`模型，希望在向量召回（可以理解为粗排）的基础上进一步优化结果，降低为生成提供的参考内容中的无效信息。

2. **语义匹配**：

>向量库检索过程仅考察了`query`向量和候选向量在向量空间的语义距离，没有考虑`query`文本和候选文本其他方面的语义关系，比如上下文信息、句法结构等。

>而`rerank`模型有机会通过衡量`query`文本和候选文本之间更丰富的语义关系实现更精细的语义匹配。

3. **场景适配**：

>通过自训练`rerank`模型来进行精排，可以按照特定需求做进一步排序，从而提升`QAG`在特定应用场景下的表现。

### 二阶段检索

>`二阶段检索`（`Two-Stage Retrieval`），即整个检索过程由原本的检索阶段和新增的精排阶段组成。

>很明显，这种组合方式能最大化利用向量库的检索速度，同时也能保证检索的效果，因而在`RAG`中广泛采用：检索过程使用基于向量的检索算法，精排过程使用`rerank`模型。

`Rerank`模型一般基于双编码器（`dual-encoder`）架构，可以同时`encode`问题和知识库语料，从而进一步度量两者之间的语义相似度。

![](img/287c3bfdbdd7f4e106783c976c874caa.png)

`QAnything`采用流程的就是两阶段检索，可以理解为`RAG with Rerank`：

![](img/ca87280a4d2b615554bbd0c1512ada98.png)

# Rerank模型

国内中文开源`rerank`模型中效果比较好的有`BAAI`的`bge`系列模型：

![](img/e768a3a258e6284875ed3ecb54264215.png)

## RAG框架中的Rerank算法评估

### 为什么需要重排序？

`混合检索`通过融合多种检索技术的优势，能够提升检索的召回效果。

![](img/0f3683121d2deb14a6528cdb7693e0e8.png)

>然而，这种方法在应用不同的`检索模式`时，必须对结果进行整合和标准化处理。

>标准化是指将数据调整到一致的标准范围或格式，以便于更有效地进行比较、分析和处理。

>在完成这些步骤后，这些数据将整合并提供给大型模型进行处理。

>为了实现这一过程，我们需要引入一个评分系统，即重排序模型（`Rerank Model`），它有助于进一步优化和精炼检索结果。

>`Rerank模型`通过对候选文档列表进行重新排序，以提高其与用户查询语义的匹配度，从而优化排序结果。

>该模型的核心在于评估用户问题与每个候选文档之间的关联程度，并基于这种相关性给文档排序，使得与用户问题更为相关的文档排在更前的位置。

>这种模型的实现通常涉及计算相关性分数，然后按照这些分数从高到低排列文档。

>市场上已有一些流行的重排序模型，例如`Cohere rerank`、`bge-reranker`等，它们在不同的应用场景中表现出了优异的性能。

### BGE-Reranker模型

>`bge-reranker`是`BAAI`（北京智源人工智能研究院）发布的系列模型之一。

>包括`Embedding`、`Rerank`系列模型等。`bge-reranker`模型在`HuggingFace`上开源，有`base`、`large`两个版本模型。

借助`FlagEmbedding`，我们以`BAAI/bge-reranker-base`模型为例，使用`FastAPI`封装成`HTTP`服务，`Python`代码如下：

```python
# !/usr/bin/env python
# encoding: utf-8
import uvicorn
from fastapi import FastAPI
from pydantic import BaseModel
from operator import itemgetter
from FlagEmbedding import FlagReranker

app = FastAPI()

reranker = FlagReranker('/data_2/models/bge-reranker-base/models--BAAI--bge-reranker-base/blobs', use_fp16=True)

class QuerySuite(BaseModel):
    query: str
    passages: list[str]
    top_k: int = 1
    
@app.post('/bge_base_rerank')
def rerank(query_suite: QuerySuite):
    scores = reranker.compute_score([[query_suite.query, passage] for passage in query_suite.passages])
    if isinstance(scores, list):
        similarity_dict = {passage: scores[i] for i, passage in enumerate(query_suite.passages)}
    else:
        similarity_dict = {passage: scores for i, passage in enumerate(query_suite.passages)}
    sorted_similarity_dict = sorted(similarity_dict.items(), key=itemgetter(1), reverse=True)
    result = {}
    for j in range(query_suite.top_k):
        result[sorted_similarity_dict[j][0]] = sorted_similarity_dict[j][1]
    return result
    
if __name__ == '__main__':
    uvicorn.run(app, host='0.0.0.0', port=50072)
```

计算`"上海天气"`与`"北京美食"`、`"上海气候"`的`Rerank`相关性分数，请求如下：

```bash
curl --location 'http://localhost:50072/bge_base_rerank' \
--header 'Content-Type: application/json' \
--data '{
    "query": "上海天气",
    "passages": ["北京美食", "上海气候"],
    "top_k": 2
}'
```

输出如下：

```json
{
    "上海气候": 6.24609375,
    "北京美食": -7.29296875
}
```

### 评估实验

`bge-reranker-base`的评估结果如下：

![](img/b8428b096ed41fb8f1b6fcfc05332a5f.png)

`bge-reranker-large`的评估结果如下：

![](img/a75808f7921e89a97810a0efa4b6a58a.png)

以`Ensemble Search`为`baseline`，分别对三种`Rerank`模型进行`Hit Rate`指标统计，柱状图如下：

![](img/1be6156dd3f49b7592168645922599d0.png)

>从上述的统计图中可以得到如下结论：

+ 在`Ensemble Search`阶段后加入`Rerank`模型会有检索效果提升

+ 就检索效果而言，`Rerank`模型的结果为：`Cohere` > `bge-rerank-large` > `bge-rerank-base`，但效果相差不大
