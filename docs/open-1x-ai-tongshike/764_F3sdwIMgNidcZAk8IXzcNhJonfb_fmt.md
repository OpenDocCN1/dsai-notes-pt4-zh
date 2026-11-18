
# RAG背后的数学：向量点积与余弦的秘密

> 来源：[https://datawhaler.feishu.cn/docx/RJWDdzjcKovM74xEM9fcXDJbnZg](https://datawhaler.feishu.cn/docx/RJWDdzjcKovM74xEM9fcXDJbnZg)

## 🎯 文章目标

本文面向对线性代数和AI应用感兴趣的读者，旨在帮助大家：

+ 🌟 理解向量点积的含义、计算方法及其几何意义
+ 📊 掌握点积与余弦的关系及其在实际中的应用
+ 🚀 通过`Python`代码实践，加深对点积的直观认识

> 💡 小提示
> 本文包含丰富的代码示例，建议动手运行代码，亲自体验点积的魅力哦！

## 📄 主题

本次主题：向量点积的含义、计算公式及其与余弦关系 🌈

### 📚 摘要

本文深入探讨了向量点积的含义、计算方法及其与余弦的关系 🌟。我们从代数和几何两个角度揭示了点积如何连接向量的分量与方向，展现其独特魅力 📐。文中详细介绍了点积的计算方式，并附上`Python`代码实现，让理论落地 🛠️。此外，还有点积在物理（比如计算功）、机器学习（衡量相似性）和自然语言处理（注意力机制、`RAG`）中的应用。

## 🚁 前言

点积（`dot product`），也叫标量积或内积，是线性代数里一个简单却很重要的运算。它把两个向量变成一个标量，在数学、物理和计算机科学中用处很大。我们将从点积的定义讲起，逐步看看它的计算方法和几何意义，再深入分析它跟余弦（ ）的关系，配上例子和推导来帮你理解。✨

✨ 你好，我是筱可，欢迎来到「筱可 AI 研习社」！

🚀 标签关键词：`AI 实战派开发者` | `技术成长陪伴者` | `RAG 前沿探索者` | `文档处理先锋`

## 📐 一、点积是什么

点积是两个向量的运算，结果是个标量。假设有两个   维向量   和  ，点积的定义是：

简单说，就是对应分量相乘然后加起来。比如：

+ 🌟  ， ，

。

从几何上看，点积还有另一种表达：

这里：

是向量   的长度，

是   的长度，

是两向量间的夹角（0° 到 180°），

是夹角的余弦值。

这两种方式其实是一回事，几何定义能让我们更直观地看到点积跟方向和长度的关系。🌍

## 🛠️ 二、怎么算点积

1. **代数方法**

直接用分量计算，适合任何维度。比如：

+ 二维向量  ， ：

+ 三维向量  ， ：

1. **几何方法**

用长度和夹角来算，需要先求模。比如  ， ：

，

，

+ 点积  ，

+ 用几何公式： ，

+ 夹角  。

代数方法方便计算，几何方法则更能解释方向。✨

### Python实现点积计算

下面是用`Python`计算向量点积的几种方法：

```
import numpy as np
import math

# 方法1：手动计算点积
def dot_product(a, b):
    if len(a) != len(b):
        raise ValueError("向量维度必须相同")
    return sum(a_i * b_i for a_i, b_i in zip(a, b))

# 方法2：使用NumPy计算点积
def dot_product_numpy(a, b):
    return np.dot(a, b)

# 方法3：使用几何方法计算点积和夹角
def dot_product_geometric(a, b):
    a_np = np.array(a)
    b_np = np.array(b)
    dot = np.dot(a_np, b_np)
    norm_a = np.linalg.norm(a_np)
    norm_b = np.linalg.norm(b_np)
    cos_theta = dot / (norm_a * norm_b)
    angle = np.arccos(np.clip(cos_theta, -1.0, 1.0))
    return {
        "dot_product": dot,
        "magnitude_a": norm_a,
        "magnitude_b": norm_b,
        "cos_theta": cos_theta,
        "angle_radians": angle,
        "angle_degrees": angle * 180 / math.pi
    }

# 示例
a = [1, 2, 3]
b = [4, 5, 6]

print(f"手动计算点积: {dot_product(a, b)}")
print(f"NumPy计算点积: {dot_product_numpy(a, b)}")
print(f"几何方法结果:")
result = dot_product_geometric(a, b)
for key, value in result.items():
    print(f"  {key}: {value:.4f}")
```

输出:

```
手动计算点积: 32
NumPy计算点积: 32
几何方法结果:
  dot_product: 32.0000
  magnitude_a: 3.7417
  magnitude_b: 8.7750
  cos_theta: 0.9746
  angle_radians: 0.2257
  angle_degrees: 12.9332
```

## 🌍 三、点积有什么用

点积在实际中很实用，我们来看几个例子：

1. **看方向关系**

点积的正负能告诉你向量怎么摆：

：夹角小于 90°，方向靠得近。比如  ， ，点积是 4，夹角 0°。

：夹角 90°，互相垂直。比如  ， ，点积是 0。

：夹角大于 90°，方向相反。比如  ， ，点积是 -1。

1. **算投影**

想知道   在   上的投影长度，用：

比如  ， ：

，

，

+ 投影长度 =  。

1. **物理里的功**

功   是力和位移的点积。比如力  ，位移  ：

焦耳。

1. **机器学习中的相似性**

在机器学习中，特别是自然语言处理和推荐系统中，点积是衡量向量相似度的关键指标。

1. **词向量相似度**：当文本被表示为向量后，点积可以反映语义接近程度。例如：

， ，点积为

+ 归一化后的点积（余弦相似度）为

1. **应用场景**：

+ 文本检索：计算查询和文档向量的相似度
+ 推荐系统：衡量用户向量和物品向量的匹配程度
+ 聚类算法：根据点积决定数据点归属
+ 神经网络：在注意力机制中计算注意力分数

1. **标准化点积**：实践中常将向量标准化后再计算点积，消除长度影响，仅考虑方向相似性。

## 📊 四、点积和余弦的关系

点积跟余弦的联系是它几何意义的关键。公式是：

点积的大小跟向量长度和   挂钩。反过来：

### Python实现余弦相似度计算

```
import numpy as np
from sklearn.metrics.pairwise import cosine_similarity

# 计算两个向量的余弦相似度
def cosine_sim(a, b):
    a_np = np.array(a)
    b_np = np.array(b)
    return np.dot(a_np, b_np) / (np.linalg.norm(a_np) * np.linalg.norm(b_np))

# 使用sklearn计算余弦相似度
def cosine_sim_sklearn(a, b):
    a_np = np.array(a).reshape(1, -1)
    b_np = np.array(b).reshape(1, -1)
    return cosine_similarity(a_np, b_np)[0][0]

# 示例
a = [1, 0]
b = [1, 1]
print(f"向量 {a} 和 {b} 的余弦相似度: {cosine_sim(a, b):.4f}")
print(f"验证夹角为: {np.arccos(cosine_sim(a, b)) * 180 / np.pi:.2f} 度")

# 反向向量示例
c = [1, 0]
d = [-1, 0]
print(f"反向向量 {c} 和 {d} 的余弦相似度: {cosine_sim(c, d):.4f}")
print(f"验证夹角为: {np.arccos(np.clip(cosine_sim(c, d), -1.0, 1.0)) * 180 / np.pi:.2f} 度")

# 垂直向量示例
e = [1, 0]
f = [0, 1]
print(f"垂直向量 {e} 和 {f} 的余弦相似度: {cosine_sim(e, f):.4f}")
print(f"验证夹角为: {np.arccos(cosine_sim(e, f)) * 180 / np.pi:.2f} 度")
```

输出:

```
向量 [1, 0] 和 [1, 1] 的余弦相似度: 0.7071
验证夹角为: 45.00 度
反向向量 [1, 0] 和 [-1, 0] 的余弦相似度: -1.0000
验证夹角为: 180.00 度
垂直向量 [1, 0] 和 [0, 1] 的余弦相似度: 0.0000
验证夹角为: 90.00 度
```

### 特点和意义

+ **范围**：  在 -1 到 1 之间：

：夹角 0°，同向。

：夹角 90°，垂直。

：夹角 180°，反向。

+ **投影角度**：  正，投影是正方向；负则是反方向。

+ **余弦相似度**：机器学习里常用   看方向相似，不关心长度。

## 📝 五、向量点积在文本处理中的应用

在自然语言处理(`NLP`)和信息检索领域，向量点积扮演着核心角色。文本数据本身是非结构化的，通过向量化处理后，才能应用数学运算进行分析和比较。

### 文本如何表示为向量

文本向量化有多种方法：

1. 词袋模型(`Bag of Words`)

将文本表示为词频向量。例如，对于词汇表 `["苹果", "香蕉", "橙子"]`：

+ 句子 `"我喜欢苹果和香蕉"` → `[1, 1, 0]`

+ 句子 `"香蕉和橙子很甜"` → `[0, 1, 1]`

1. **`TF-IDF`向量**(`Term Frequency-Inverse Document Frequency`)

在词袋基础上考虑词的重要性，结合词频和逆文档频率：

1. 其中：

是词   在文档   中的词频：

是词   的逆文档频率，用于降低常见词的权重：

1. （这里加1是为了避免分母为0的情况，有些实现还会在分子上也加1）

1. 例如，对于句子：

+ 文档1：`"机器学习很有趣"` → `[0.3, 0.5, 0.0, 0.7]`

+ 文档2：`"深度学习是机器学习的分支"` → `[0.1, 0.4, 0.2, 0.0]`

1. 词嵌入(`Word Embeddings`)

如`Word2Vec`、`GloVe`等方法，将词映射到密集的低维空间：

+ `"北京"` → `[0.2, -0.5, 0.8, ...]`

+ `"上海"` → `[0.3, -0.3, 0.7, ...]`

这些向量能捕捉语义关系，如`"北京"`和`"上海"`的向量很接近，因为都是城市。

1. **文档嵌入**

如`Doc2Vec`，将整个文档映射为单个向量，保留文档级语义。

### 用`Python`实现文本向量相似度计算

```
import numpy as np
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import jieba  # 添加中文分词库

# 示例文本
documents = [
    "机器学习是人工智能的一个分支",
    "深度学习是机器学习的一种方法",
    "自然语言处理是人工智能的重要应用",
    "向量点积在机器学习中有重要应用"
]

# 使用jieba进行中文分词
def chinese_tokenizer(text):
    return list(jieba.cut(text))

# 1. 使用词袋模型向量化
print("1. 词袋模型向量化:")
count_vec = CountVectorizer(tokenizer=chinese_tokenizer)
X_count = count_vec.fit_transform(documents)
print(f"词汇表: {count_vec.get_feature_names_out()}")
print(f"文档向量形状: {X_count.shape}")
print("文档向量:")
print(X_count.toarray())

# 计算文档1和文档2的余弦相似度
doc1_vec = X_count.toarray()[0]
doc2_vec = X_count.toarray()[1]
sim = np.dot(doc1_vec, doc2_vec) / (np.linalg.norm(doc1_vec) * np.linalg.norm(doc2_vec))
print(f"文档1和文档2的余弦相似度: {sim:.4f}")

# 2. 使用TF-IDF向量化
print("\n2. TF-IDF向量化:")
tfidf_vec = TfidfVectorizer(tokenizer=chinese_tokenizer)
X_tfidf = tfidf_vec.fit_transform(documents)
print(f"词汇表: {tfidf_vec.get_feature_names_out()}")
print(f"文档TF-IDF向量形状: {X_tfidf.shape}")
print("文档TF-IDF向量:")
print(np.round(X_tfidf.toarray(), 4))

# 计算文档1和文档2的余弦相似度
doc1_tfidf = X_tfidf.toarray()[0]
doc2_tfidf = X_tfidf.toarray()[1]
tfidf_sim = np.dot(doc1_tfidf, doc2_tfidf) / (np.linalg.norm(doc1_tfidf) * np.linalg.norm(doc2_tfidf))
print(f"文档1和文档2的余弦相似度(手动计算): {tfidf_sim:.4f}")

# 使用sklearn计算所有文档对之间的余弦相似度
cosine_sim = cosine_similarity(X_tfidf, X_tfidf)
print("余弦相似度矩阵:")
print(np.round(cosine_sim, 4))

# 3. 查找最相似的文档
print("\n3. 相似文档查询:")
query = "机器学习和人工智能"
query_vec = tfidf_vec.transform([query])
print(f"查询向量: {np.round(query_vec.toarray(), 4)}")
similarities = cosine_similarity(query_vec, X_tfidf)[0]
print(f"查询与各文档的相似度: {np.round(similarities, 4)}")

# 为每个文档打印详细相似度信息
for i, (doc, sim) in enumerate(zip(documents, similarities)):
    print(f"文档 {i+1}: {sim:.4f} - {doc}")

# 查找最相似的文档
most_similar_idx = np.argmax(similarities)
print(f"\n最相似的文档: {documents[most_similar_idx]}")
print(f"相似度得分: {similarities[most_similar_idx]:.4f}")

# 可视化分词结果
print("\n4. 分词效果展示:")
for i, doc in enumerate(documents):
    print(f"文档 {i+1} 分词结果: {' '.join(chinese_tokenizer(doc))}")
print(f"查询分词结果: {' '.join(chinese_tokenizer(query))}")
```

输出示例:



# 文本向量化与相似度计算示例

```
词汇表: ['一个' '一种' '中有' '人工智能' '分支' '向量' '在' '处理' '学习' '应用' '方法' '是' '机器' '深度'
 '点积' '的' '自然语言' '重要']
文档向量形状: (4, 18)
文档向量:
[[1 0 0 1 1 0 0 0 1 0 0 1 1 0 0 1 0 0]
 [0 1 0 0 0 0 0 0 2 0 1 1 1 1 0 1 0 0]
 [0 0 0 1 0 0 0 1 0 1 0 1 0 0 0 1 1 1]
 [0 0 1 0 0 1 1 0 1 1 0 0 1 0 1 0 0 1]]
文档1和文档2的余弦相似度: 0.5976

2. TF-IDF向量化:
词汇表: ['一个' '一种' '中有' '人工智能' '分支' '向量' '在' '处理' '学习' '应用' '方法' '是' '机器' '深度'
 '点积' '的' '自然语言' '重要']
文档TF-IDF向量形状: (4, 18)
文档TF-IDF向量:
[[0.485  0.     0.     0.3824 0.485  0.     0.     0.     0.3096 0.
  0.     0.3096 0.3096 0.     0.     0.3096 0.     0.    ]
 [0.     0.4134 0.     0.     0.     0.     0.     0.     0.5277 0.
  0.4134 0.2639 0.2639 0.4134 0.     0.2639 0.     0.    ]
 [0.     0.     0.     0.3645 0.     0.     0.     0.4623 0.     0.3645
  0.     0.2951 0.     0.     0.     0.2951 0.4623 0.3645]
 [0.     0.     0.4063 0.     0.     0.4063 0.4063 0.     0.2593 0.3203
  0.     0.     0.2593 0.     0.4063 0.     0.     0.3203]]
文档1和文档2的余弦相似度(手动计算): 0.4084
余弦相似度矩阵:
[[1.     0.4084 0.322  0.1606]
 [0.4084 1.     0.1557 0.2053]
 [0.322  0.1557 1.     0.2335]
 [0.1606 0.2053 0.2335 1.    ]]

3. 相似文档查询:
查询向量: [[0.     0.     0.     0.6578 0.     0.     0.     0.     0.5326 0.
  0.     0.     0.5326 0.     0.     0.     0.     0.    ]]
查询与各文档的相似度: [0.5813 0.4216 0.2398 0.2762]
文档 1: 0.5813 - 机器学习是人工智能的一个分支
文档 2: 0.4216 - 深度学习是机器学习的一种方法
文档 3: 0.2398 - 自然语言处理是人工智能的重要应用
文档 4: 0.2762 - 向量点积在机器学习中有重要应用

最相似的文档: 机器学习是人工智能的一个分支
相似度得分: 0.5813

4. 分词效果展示:
文档 1 分词结果: 机器 学习 是 人工智能 的 一个 分支
文档 2 分词结果: 深度 学习 是 机器 学习 的 一种 方法
文档 3 分词结果: 自然语言 处理 是 人工智能 的 重要 应用
文档 4 分词结果: 向量 点积 在 机器 学习 中有 重要 应用
查询分词结果: 机器 学习 和 人工智能
```

# 大语言模型中的注意力机制

在Transformer架构的大语言模型（如`BERT`、`GPT`）中，向量点积是实现注意力机制的关键。注意力机制的核心公式如下：

其中：

：查询矩阵（Query）

：键矩阵（Key）

：键矩阵的转置

：值矩阵（Value）

：键向量的维度

：查询和键的点积，表示相关性得分

：缩放点积，防止梯度消失

以下是一个简化的实现：

```
import numpy as np
import torch
import torch.nn as nn

# 简化版自注意力机制实现
class SimplifiedAttention:
    def __init__(self, d_model, scale=True):
        self.d_model = d_model
        self.scale = scale
        
    def forward(self, Q, K, V):
        # 计算点积注意力
        scores = np.matmul(Q, K.T)  
        
        # 缩放（可选）
        if self.scale:
            scores = scores / np.sqrt(self.d_model)
        
        # 应用softmax获取注意力权重
        attention_weights = self._softmax(scores)
        
        # 加权和得到输出
        output = np.matmul(attention_weights, V)
        return output, attention_weights
    
    def _softmax(self, x):
        exp_x = np.exp(x - np.max(x, axis=1, keepdims=True))
        return exp_x / np.sum(exp_x, axis=1, keepdims=True)

# 示例：分析一个简单句子的注意力
def analyze_attention():
    # 假设我们有一个简单的句子："苹果公司发布了新iPhone"
    # 用简化的向量表示每个词
    word_vectors = {
        "苹果": np.array([0.8, 0.2, 0.7, 0.1]),
        "公司": np.array([0.7, 0.3, 0.6, 0.2]),
        "发布": np.array([0.3, 0.8, 0.2, 0.6]),
        "了": np.array([0.1, 0.1, 0.1, 0.1]),
        "新": np.array([0.5, 0.6, 0.3, 0.4]),
        "iPhone": np.array([0.9, 0.3, 0.8, 0.2])
    }
    
    # 构建QKV矩阵
    words = ["苹果", "公司", "发布", "了", "新", "iPhone"]
    embeddings = np.array([word_vectors[w] for w in words])
    
    # 简单线性投影（在实际模型中是通过学习的权重矩阵投影）
    Q = embeddings  # 查询向量
    K = embeddings  # 键向量
    V = embeddings  # 值向量
    
    # 应用注意力机制
    attention = SimplifiedAttention(d_model=4, scale=True)
    output, weights = attention.forward(Q, K, V)
    
    # 打印注意力权重矩阵
    print("注意力权重矩阵:")
    for i, word in enumerate(words):
        print(f"{word} 关注到: ", end="")
        for j, target in enumerate(words):
            print(f"{target}: {weights[i, j]:.3f}  ", end="")
        print()

# 运行注意力分析
analyze_attention()
```

输出示例：

```
注意力权重矩阵:
苹果 关注到: 苹果: 0.192  公司: 0.181  发布: 0.144  了: 0.117  新: 0.157  iPhone: 0.210  
公司 关注到: 苹果: 0.185  公司: 0.178  发布: 0.154  了: 0.119  新: 0.162  iPhone: 0.202
发布 关注到: 苹果: 0.155  公司: 0.162  发布: 0.202  了: 0.126  新: 0.183  iPhone: 0.171
了 关注到: 苹果: 0.168  公司: 0.168  发布: 0.169  了: 0.157  新: 0.168  iPhone: 0.171
新 关注到: 苹果: 0.167  公司: 0.169  发布: 0.181  了: 0.125  新: 0.175  iPhone: 0.183
iPhone 关注到: 苹果: 0.193  公司: 0.181  发布: 0.145  了: 0.109  新: 0.157  iPhone: 0.215
```

## 🔍 六、RAG系统中的相似度判断

在检索增强生成(RAG)系统中，相似度计算是核心环节，决定了检索结果的质量。向量点积和余弦相似度在这一过程中扮演着关键角色。

### 6.1 RAG系统中的向量检索流程

RAG系统的基本工作原理可概括为以下步骤：

1. **文档向量化**：将知识库中的所有文档转换为高维向量表示

1. **查询向量化**：将用户查询同样转换为向量

1. **相似度计算**：计算查询向量与文档向量间的相似度

1. **检索与排序**：根据相似度分数返回最相关的文档

1. **生成回答**：将检索到的内容与原始问题一起输入大模型生成回答

其中，第3步的相似度计算通常采用向量点积或余弦相似度。

### 6.2 点积与余弦在RAG中的应用

#### 为什么RAG系统偏爱余弦相似度？

在实际的RAG系统中，余弦相似度比原始点积更受青睐，原因包括：

+ **长度无关性**：余弦只关注方向，不受文档长度影响

+ **标准化范围**：结果被限制在[-1,1]区间，便于设定阈值和比较

+ **语义相关性**：方向相似往往意味着语义相关，而不必考虑向量模长

#### RAG系统中的实际应用

1. **稠密检索**：使用`BERT`、`BGE`等模型编码文档和查询，通过余弦相似度找到最匹配的文档

1. **混合检索**：结合`BM25`等词频模型与余弦相似度的结果，提高召回质量

1. **重排序**：对初步检索结果使用更精细的余弦相似度计算进行重新排序

### 6.3 使用BGE-M3模型计算向量相似度示例

以下代码展示了如何使用`BGE-M3`模型生成向量并计算余弦相似度：

```
from FlagEmbedding import BGEM3FlagModel
import numpy as np

# 初始化模型
model_path = r"C:\Users\k\Desktop\BaiduSyncdisk\baidu_sync_documents\hf_models\bge-m3"
model = BGEM3FlagModel(model_path, use_fp16=True)

# 输入数据
sentences = ["What is BGE M3?", "Defination of BM25"]
output = model.encode(
    sentences=sentences, 
    batch_size=12, 
    max_length=8192, 
    return_dense=True, 
    return_sparse=False, 
    return_colbert_vecs=False
)

# 提取向量
dense_vecs = output['dense_vecs'] 
print(f"Dense vectors shape: {dense_vecs.shape}")  # 形状: (2, 1024)

dense_vecs1 = output['dense_vecs'][0]  # 第一个句子的稠密向量
dense_vecs2 = output['dense_vecs'][1]  # 第二个句子的稠密向量
print(f"Dense vectors 1: {dense_vecs1.shape}")
print(f"Dense vectors 2: {dense_vecs2.shape}")

# 计算稠密向量之间的余弦相似度
cosine_similarity = np.dot(dense_vecs1, dense_vecs2) / (np.linalg.norm(dense_vecs1) * np.linalg.norm(dense_vecs2))
print(f"Cosine similarity between dense_vecs1 and dense_vecs2: {cosine_similarity}")
```

输出：

```
Dense vectors shape: (2, 1024)
Dense vectors 1: (1024,)
Dense vectors 2: (1024,)
Cosine similarity between dense_vecs1 and dense_vecs2: 0.41029053926467896
```

### 6.4 自定义余弦相似度计算函数

为了更好地理解余弦相似度的计算原理，我们可以实现自己的余弦相似度函数：

```
def cosine_similarity(vec1, vec2):
    """
    计算两个向量之间的余弦相似度
    
    参数:
    vec1, vec2: numpy数组表示的向量
    
    返回:
    float: 余弦相似度值，范围在[-1, 1]之间
    """
    # 计算点积
    dot_product = np.dot(vec1, vec2)
    
    # 计算向量模长
    norm_vec1 = np.linalg.norm(vec1)
    norm_vec2 = np.linalg.norm(vec2)
    
    # 避免除零错误
    if norm_vec1 == 0 or norm_vec2 == 0:
        return 0
    
    # 计算余弦相似度
    similarity = dot_product / (norm_vec1 * norm_vec2)
    return similarity

# 使用示例
similarity = cosine_similarity(dense_vecs1, dense_vecs2)
print(f"自定义函数计算的余弦相似度: {similarity}")
```

输出：

```
自定义函数计算的余弦相似度: 0.41029053926467896
```

### 6.5 点积在RAG中的优化

在大规模RAG系统中，向量点积的计算往往是性能瓶颈。常见的优化方法包括：

1. **向量量化**：将32位浮点向量压缩为8位或更低位，减少内存占用和计算量

1. **索引结构**：使用`HNSW`、`FAISS`等近似最近邻算法，加速相似度搜索

1. **GPU加速**：利用GPU的并行计算能力，加速矩阵点积运算

1. **分布式计算**：在多机多卡环境下分布式计算点积，处理超大规模向量库

通过这些优化，现代RAG系统能够在毫秒级别内从数百万文档中检索出最相关的内容，为大模型提供精准的上下文信息。

> 🙋♂️ 入群交流

+ 👸号菜单点击「社群」，扫码直接入群

+ 回复关键词「入群」，添加作者🛰️人工邀请

## 📒 六、总结与展望

### 🚀 6.1 技术全景图

+ 🌟 点积的代数与几何定义，连接数学与实际应用

+ 📊 余弦相似度在方向判断中的核心作用

+ 🌍 点积在物理、`NLP`、`AI`模型中的广泛应用

### 📓 6.2 学习汇总

+ 🛠️ 学会用代数和几何方法计算点积，并用`Python`实现

+ 📐 理解点积与   的关系及其几何意义

+ ✨ 掌握点积在文本处理和注意力机制中的应用

---

### 🔥 6.3 动手挑战

1. 💻 用`Python`计算两个自定义向量的点积和夹角



1. 📝 尝试用 `TF-IDF` 向量化一组文档，计算它们的余弦相似度

1. 🚀 实现一个简化的注意力机制，分析一个短句的词间关系

## ♻️ 6.4 互动问题

1. 🌟 你觉得点积在哪些场景下最实用？

1. 📊 在机器学习中，余弦相似度和欧氏距离有什么不同？

1. ✨ 如果让你设计一个基于点积的推荐系统，你会怎么做？

> **名言**：

> “数学是宇宙的语言，点积则是向量间的对话。”

> ——筱可

**欢迎在留言区分享你的想法，每条留言我都会认真阅读！** 你的反馈是我创作的最大动力 ❤️

## 💗 立即行动

1. **点赞、收藏、关注**，顺手分享给可能感兴趣的朋友。

1. 为账号加个星标或特别关注，获取更多精彩内容。

1. 以本文为灵感，整理一份属于自己的笔记。

## 🚗 行动召唤

📢 "与其等着 AI 改变世界，不如自己参与变革！在这里，让 AI 成为你弯道超车的秘密武器。"

## 📒 附录

（往期文章）

+ RAG系统召回率低？ `BGE-M3` 多语言多功能重塑文本检索

+ RAG去重小助手 `SimHash` 算法：轻松解决文本相似度检测与查重难题

+ 从模式匹配到语义理解：文本嵌入技术的演变与 RAG 系统优化

+ 别再瞎猜了！用 `RAGAS` 精准定位 RAG 系统短板

+ `PDF` 提取神器！将双栏学术论文转换为单栏 `Markdown` 实战（上）

+ 从零打造 RAG 检索系统：`BM25` 让检索快到飞起

+ 打造 RAG 应用必知：`BM25` 算法实战解析，让你不落人后

+ 手把手教程：用 `MinerU` 构建自动化文档转换流水线

+ [飞桨版面检测分析的命令看不懂？恭喜！您已获得 arg](http://mp.weixin.qq.com/s/5b8PqqViSpvgMjpfJqOxCQ?token=1077873472&lang=zh_CN) `parse` 入门手册一本，快快学起来吧！

+ `pdf` 提取逆天神器，基于飞浆平台的版面检测与版面识别(1)

+ [大模型应用极简开发快速入门](http://mp.weixin.qq.com/s/kHeVK6hManVUaci2p-42GQ)

+ [基于 Streamlit 和 De](https://mp.weixin.qq.com/s/38BcsiGF1WhKx4DaA-8l0A) `epSeek` 的智能文档助手开发实战指南

+ 告别低效沟通：提示词框架助你玩转 `deepseek`
