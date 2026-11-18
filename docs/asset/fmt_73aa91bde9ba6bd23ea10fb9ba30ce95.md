
## （三）BGE

### （2）模型地址

+   `https://huggingface.co/BAAI/bge-small-en`
+   `https://huggingface.co/BAAI/bge-small-zh`
+   `https://huggingface.co/BAAI/bge-base-en`
+   `https://huggingface.co/BAAI/bge-base-zh`
+   `https://huggingface.co/BAAI/bge-large-en`
+   `https://huggingface.co/BAAI/bge-large-en-v1.5`
+   `https://huggingface.co/BAAI/bge-large-zh`
+   `https://huggingface.co/BAAI/bge-large-zh-v1.5`

代码地址：

+   `https://github.com/FlagOpen/FlagEmbedding`

### （3）模型使用

依赖库：

```
pip install -U FlagEmbedding
或
git clone https://github.com/FlagOpen/FlagEmbedding.git
cd FlagEmbedding
pip install -e .
```

样例代码：

```
from FlagEmbedding import FlagModel
sentences_1 = ["样例数据-1", "样例数据-2"]
sentences_2 = ["样例数据-3", "样例数据-4"]
model = FlagModel('BAAI/bge-large-zh-v1.5', 
                  query_instruction_for_retrieval="为这个句子生成表示以用于检索相关文章：",
                  use_fp16=True) # Setting use_fp16 to True speeds up computation with a slight performance degradation
embeddings_1 = model.encode(sentences_1)
embeddings_2 = model.encode(sentences_2)
similarity = embeddings_1 @ embeddings_2.T
print('similarity:')
print(similarity)

# for s2p(short query to long passage) retrieval task, suggest to use encode_queries() which will automatically add the instruction to each query
# corpus in retrieval task can still use encode() or encode_corpus(), since they don't need instruction
queries = ['query_1', 'query_2']
passages = ["样例文档-1", "样例文档-2"]
q_embeddings = model.encode_queries(queries)
p_embeddings = model.encode(passages)
scores = q_embeddings @ p_embeddings.T

print('scores:')
print(scores)
```

![](img/8a1144a55928d4dddd823e4f2215edba.png)

## （四）BCEmbedding：bce-embedding-base_v1

### （1）模型概述

`BCEmbedding`（Bilingual and Crosslingual Embedding）是网易有道2024年1月3日发布的两阶段检索算法库。

`BCEmbedding`以其出色的双语和跨语种能力而著称，在语义检索中消除中英语言之间的差异。`BCEmbedding`是一个二阶段检索算法模型库，由召回模块和精排模块组成，核心是`EmbeddingModel`(`bce-embedding-base_v1`)和`RerankerModel`(`bce-reranker-base_v1`)两款模型。

`bce-embedding-base_v1`模型是一款基于`XLMRoberta`的句子向量编码模型，拥有中英双语种编码能力。用于离线提取知识库语料提取语义向量，并由向量数据库保存。用户提问时，该模型实时地提取用户问题的语义向量，用于之后结合向量数据库进行一阶段检索。

`bce-reranker-base_v1`模型是一款基于`XLMRoberta`的句子向量编码模型，拥有中英日韩四个语种跨语种语义精排能力。一阶段检索后，该模型依次评估各文档与用户问题的相关性，输出有意义的语义相关分数。之后根据相关分数进行过滤和排序，实现对文档的选优。

二阶段检索算法结合召回和精排二者的优势，召回阶段可以快速找到用户问题相关文本片段，精排阶段可以将正确相关片段尽可能排在靠前位置，并过滤掉低质量的片段。二阶段检索可以很好地权衡检索效果和效率，具有巨大应用价值。

### （2）模型地址

模型地址：

+   `https://huggingface.co/maidalun1020/bce-embedding-base_v1`（HuggingFace）

+   `https://modelscope.cn/models/maidalun/bce-embedding-base_v1`（魔搭社区）

代码地址：

+   `https://github.com/netease-youdao/BCEmbedding`

### （3）模型使用

依赖库：

```
pip install BCEmbedding==0.1.5
或
git clone git@github.com:netease-youdao/BCEmbedding.git
cd BCEmbedding
pip install -v -e .
```

样例代码：

```
from BCEmbedding import EmbeddingModel

# list of sentences
sentences = ['sentence_0', 'sentence_1']

# init embedding model
model = EmbeddingModel(model_name_or_path="maidalun1020/bce-embedding-base_v1")

# extract embeddings
embeddings = model.encode(sentences)
```

![](img/90554624cac1bf829420632a5b39b8af.png)

## （五）BGE-M3：bge-m3

### （1）模型概述

`BGE-M3`是北京智源人工智能研究院（BAAI）2024年1月30日推出的模型。论文地址：`https://arxiv.org/abs/2402.03216`。论文提出了一种新的embedding模型，称为`M3-Embedding`，它在多语言性（Multi-Linguality）、多功能性（Multi-Functionality）和多粒度性（Multi-Granularity）方面表现出色。`M3-Embedding`支持超过100种工作语言，支持8192长度的输入文本，同时支持密集检索（Dense Retrieval）、多向量检索（Multi-Vector Retrieval）和稀疏检索（Sparse Retrieval），为现实世界中的信息检索（IR）应用提供了统一的模型基础，通过这几种检索方式的组合，取得了良好的混合召回效果。

`bge-m3`是结合了`BGE`和`M3E`特点的模型。

`BGE-M3`模型的特点如下：

+   多语言（Multi-Linguality），训练集包含100+种以上语言， 具备领先的多语言、跨语言检索能力。既包含每种语言内部的语义匹配任务（Language X to Language X），又包含不同语言之间的语义匹配任务（Language X to Language Y），丰富且优质的训练数据帮助`BGE-M3`建立了出色的多语言检索（Multi-Lingual Retrieval）与跨语言检索能力（Cross-Lingual Retrieval）。

+   多功能（Multi-Functionality），支持稠密检索（Dense Retrieval），还支持稀疏检索（Sparse Retrieval）与多向量检索（Multi-vector Retrieval）。`BGE-M3`既可以借助特殊token `[CLS]`的输出向量用以完成稠密检索（Dense Retrieval）任务，又可以利用其他一般性token的输出向量用以支持稀疏检索（Sparse Retrieval）与多向量检索（Multi-vector Retrieval）。三种检索功能的高度集成使得`BGE-M3`可以一站式服务不同的现实场景，如语义搜索、关键字搜索、重排序同时，无需使用多个模型进行多个推理，`BGE-M3`一次推理就可以得到多个不同模式的输出，无需额外开销，并能高效支持混合检索，联合三种检索模式可获得更加精准的检索结果。

+   多粒度（Multi-Granularity），`BGE-M3`目前可以处理最大长度为8192 的输入文本，支持“句子”、“段落”、“篇章”、“文档”等不同粒度的输入文本。在训练`BGE-M3`时，在现有长文本检索数据集的基础之上，通过模型合成的方式获取了大量文本长度分布多样化的训练数据；`BGE-M3`通过改进分批（batch）与缓存（cache）策略，使得训练过程具备足够高的吞吐量与负样本规模，从而确保了训练结果的质量。基于数据与算法双层面的优化处理，`BGE-M3`得以高质量的支持“句子”、“段落”、“篇章”、“文档”等不同粒度的输入文本。

### （2）模型地址

模型地址：

+   `https://huggingface.co/BAAI/bge-m3`

代码地址：

+   `https://github.com/FlagOpen/FlagEmbedding`

### （3）模型使用

依赖库：

```
pip install -U FlagEmbedding
或
git clone https://github.com/FlagOpen/FlagEmbedding.git
cd FlagEmbedding
pip install -e .
```

样例代码：

```
from FlagEmbedding import BGEM3FlagModel

model = BGEM3FlagModel('BAAI/bge-m3',  
                       use_fp16=True) # Setting use_fp16 to True speeds up computation with a slight performance degradation

sentences_1 = ["What is BGE M3?", "Defination of BM25"]
sentences_2 = ["BGE M3 is an embedding model supporting dense retrieval, lexical matching and multi-vector interaction.", 
               "BM25 is a bag-of-words retrieval function that ranks a set of documents based on the query terms appearing in each document"]

embeddings_1 = model.encode(sentences_1, 
                            batch_size=12, 
                            max_length=8192, # If you don't need such a long length, you can set a smaller value to speed up the encoding process.
                            )['dense_vecs']
embeddings_2 = model.encode(sentences_2)['dense_vecs']
similarity = embeddings_1 @ embeddings_2.T
print(similarity)
# [[0.6265, 0.3477], [0.3499, 0.678 ]]
```

![](img/0b500ba085ea332bec75abf0427410ec.png)

## （六）OpenAI：text-embedding-ada-002

### （1）模型概述

`text-embedding-ada-002`是OpenAI于2022年12月发布的一个基于`transformer`的embedding模型，需付费使用。它通过在大规模语料库上进行预训练，学习到了丰富的语义信息。它能够捕获词序信息，因此在处理复杂的NLP任务时表现出色。然而，由于其模型结构的复杂性，它需要消耗较多的计算资源。

模型特点如下：

+   统一能力：OpenAI通过将五个独立的模型（文本相似性、文本搜索-查询、文本搜索-文档、代码搜索-文本和代码搜索-代码）合并为一个新的模型

+   在一系列不同的文本搜索、句子相似性和代码搜索基准中，比以前的嵌入模型表现得更好

+   上下文：上下文长度为8192，处理长文档更方便

+   嵌入尺寸：只有1536个维度，是`davinci-001`嵌入尺寸的八分之一，使新的嵌入在处理矢量数据库时更具成本效益

### （2）模型地址

模型地址：

+   未开源

API地址：

+   `https://openai.xiniushu.com/docs/guides/embeddings`

## （七）Google：gemini-embedding-exp-03-07/text-embedding-004/embedding-001

### （1）模型概述

`Gemini API` 是`Google`推出的Embedding模型，可为字词、短语、代码和句子生成最先进的嵌入，可用于语义搜索、文本分类、聚类等任务。

`Gemini API` 提供了三个用于生成文本嵌入的模型：

+   `gemini-embedding-exp-03-07`

+   `text-embedding-004`

+   [`embedding-001`](https://ai.google.dev/gemini-api/docs/models/gemini?hl=zh-cn#embedding)

### （2）模型地址

模型地址：

+   未开源

API地址：

+   `https://ai.google.dev/gemini-api/docs/embeddings?hl=zh-cn`

### （3）模型使用

依赖库：

```
pip install -U -q google-generativeai
```

样例代码：

```
from google import genai
client = genai.Client(api_key="GEMINI_API_KEY")
result = client.models.embed_content(
    model="gemini-embedding-exp-03-07",
    contents="How does alphafold work?",
)

print(result.embeddings)
```

# 四、如何选型

`Text Embedding`模型如何选择是小伙伴们常见的问题。

一般来说，每家模型提供商在发布模型的时候，其实自己也有推荐，但是大多希望在自己发布的模型中的进行选择。在前面的介绍中略有相关的场景描述。

那对于不同模型提供商的模型该如何选择呢？这里提供两个思路：一是看模型发布时间，往往发布较迟的模型会比早期的模型效果好一些。像`text2vec`发布时间较早，它的问题也比较多。二是看`MTEB`或`C-MTEB`榜单，一般排名靠前的效果会好些。（这就像豆瓣电影评分，打分低一般就能说明电影确实拍的不好看。）
