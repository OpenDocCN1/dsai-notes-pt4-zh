
# RAG之常用向量相似度对比

> 来源：[https://datawhaler.feishu.cn/docx/OsqBdAqoCotiCxxXvnUcytL5nhO](https://datawhaler.feishu.cn/docx/OsqBdAqoCotiCxxXvnUcytL5nhO)

## 🎯 文章目标

本文面向对 NLP、大语言模型、RAG、数据挖掘等领域感兴趣的开发者、学习者，旨在帮助大家：

+ 系统了解 L1、L2、余弦、内积、汉明、Jaccard 等常用向量相似度与距离计算方法。

+ 掌握各种度量方法的原理、公式、优缺点及适用场景。

+ 学会根据具体任务和数据特性选择合适的度量方法，并了解相关的 Python 实现及库。

> 💡 小提示
>
> 在比较向量相似度时，思考你的数据特性是关键！数据是稠密还是稀疏？向量的“长度”是否有意义？方向是否比位置更重要？这些问题的答案将指引你选择最合适的度量方法。

## 📄 主题

**本次主题**：常用向量相似度与距离计算方法深度解析与对比

## 📚 摘要

+ **核心度量介绍**：深入解析了 L1 (曼哈顿)、L2 (欧几里得)、余弦相似度/距离、内积的计算原理、特性与应用场景。

+ **特定场景度量**：探讨了汉明距离（适用于等长离散序列）和 Jaccard 相似度/距离（适用于集合数据）的特点和用途。

+ **选择与实践**：提供了选择度量方法的指导原则（考虑数据类型、特性、任务目标），强调了数据预处理（归一化/标准化）的重要性，并介绍了 SciPy 和 Scikit-learn 等常用库。

---

## 🚁 前言

在自然语言处理 (NLP)、机器学习、信息检索和推荐系统等众多领域，衡量数据点（通常表示为向量）之间的相似性或差异性是一个基础且核心的任务。向量是表示文本、图像、用户偏好等对象的常用方式。选择合适的度量方法对于模型性能和任务效果至关重要。距离 (Distance) 通常衡量向量间的差异程度（值越小越相似），而相似度 (Similarity) 则衡量它们的接近程度（值越大越相似）。两者通常可以相互转换。本文将深入探讨几种常用的向量相似度/距离计算方法，分析其原理、优缺点、适用场景，并提供 Python 实现，方便后续在 RAG 实战的环节大家能够知道什么时候选什么向量相似度计算方法。

---

✨ 你好，我是筱可，欢迎来到「筱可 AI 研习社」！

🚀 标签关键词：| AI 实战派开发者 | 技术成长陪伴者 | RAG 前沿探索者 | 文档处理先锋 |

## 🐱 一、核心向量距离与相似度

本部分将介绍最常用且基础的几种向量距离和相似度计算方法。

### 🐷 1.1 L1 距离（Manhattan Distance / 曼哈顿距离）

**描述：**

也称为曼哈顿距离、城市街区距离 (City Block Distance) 或出租车距离 (Taxicab Geometry)。它计算的是向量空间中两个点沿着坐标轴方向移动的距离总和，就像在规则的城市街区（如曼哈顿）中从一个点走到另一个点所必须经过的街区数。它衡量的是各维度绝对差异的总和。

**公式：**

其中是向量的维度，和分别是向量和在第维上的分量。

**实现细节：**

+ 计算非常简单直接，只涉及减法、取绝对值和加法。

+ 计算复杂度为 O(n)，对于高维向量也非常高效。

**NLP 及其他应用：**

+ **特征差异比较：** 特别适用于衡量稀疏向量（如 TF-IDF 或 Bag-of-Words 表示，其中大部分值为零）之间的差异，因为它不会像 L2 距离那样放大个别维度上的巨大差异。

+ **鲁棒统计：** 在存在异常值的数据中，L1 距离比 L2 距离更鲁棒，因为差异是线性累加的，而不是平方累加，单个大的差异对总距离的影响相对较小。

**优点：**

+ 计算效率高。

+ 对数据中的异常值或噪声不敏感（鲁棒性强）。

+ 几何意义直观（城市街区路径）。

**缺点：**

+ **不考虑方向：** L1 距离只关心各维度差值的绝对大小，完全忽略了向量的方向信息。两个方向相反但各维度差值相同的向量，其 L1 距离可能很小。

+ **坐标轴依赖：** 距离的计算依赖于坐标系的选择。如果旋转坐标系，点之间的 L1 距离通常会改变。

+ 在高维空间中，所有点对之间的 L1 距离可能趋于相近，效果可能不如其他度量。

**实际场景：**

+ 需要快速计算且对异常值不敏感的场景。

+ 特征维度含义明确且不适合平方放大差异的场景。

+ 拼写纠错中衡量编辑距离的一种简化方式（只考虑替换，不考虑插入删除）。

**Python 实现：**

```
import math
from typing import List, Union

def manhattan_distance(x: List[Union[int, float]], y: List[Union[int, float]]) -> float:
    """
    计算两个向量之间的曼哈顿距离（L1 距离）

    参数:
        x: 第一个向量，列表或数组类型
        y: 第二个向量，列表或数组类型

    返回:
        两个向量间的曼哈顿距离，即各维度差值绝对值之和

    异常:
        当两个向量长度不同时抛出 ValueError 异常
    """
    if len(x) != len(y):
        raise ValueError("Vectors must have the same length")
    return sum(abs(xi - yi) for xi, yi in zip(x, y))

# 示例
vector1 = [1, 2, 3]
vector2 = [4, 5, 6]
print(f">>> manhattan_distance({vector1}, {vector2})")
print(manhattan_distance(vector1, vector2))  # 输出: 9.0
```

### 🐰 1.2 L2 距离（Euclidean Distance / 欧几里得距离）

**描述：**

最常用的距离度量之一，即向量空间中两点之间的直线距离。它是 L2 范数（向量的模）的具体应用，符合我们在二维或三维空间中对距离的直观理解（勾股定理的推广）。

**公式：**

**实现细节：**

+ 计算涉及减法、平方、加法和开方。

+ **优化：** 在比较距离大小时（例如，寻找最近邻），可以比较距离的平方来避免计算成本相对较高的开方运算，因为平方函数是单调递增的。

+ 计算复杂度主要也是 O(n)，但平方和开方运算比 L1 的绝对值和加法稍慢。

**NLP 及其他应用：**

+ **词嵌入距离：** 衡量 Word2Vec, GloVe, FastText 等生成的词向量在语义空间中的距离。距离近通常表示语义相似。

+ **聚类算法：** K-Means、DBSCAN 等多种聚类算法默认使用欧几里得距离来衡量数据点之间的远近。

+ **推荐系统：** 计算用户或物品向量之间的相似性（通常先转换为相似度）。

**优点：**

+ **直观性：** 最符合物理世界中的距离概念。

+ **旋转不变性：** 欧几里得距离不随坐标系的旋转而改变。

+ **广泛应用：** 是许多标准机器学习算法的基础。

**缺点：**

+ **对量纲敏感：** 如果不同维度的取值范围（量纲）差异很大，取值范围大的维度会对距离计算产生不成比例的影响。通常需要进行数据标准化（如 Z-score 标准化）或归一化（如 Min-Max 归一化）预处理。

+ **对异常值敏感：** 平方项会放大单个维度上的巨大差异。

+ 维度诅咒 (Curse of Dimensionality)：在非常高的维度下，所有点对之间的欧几里得距离趋向于变得非常接近，区分度下降。点倾向于分布在高维空间的“壳”上。

+ **忽略方向：** 与 L1 类似，主要关注点的位置差异，对向量方向不敏感。

**实际场景：**

+ 低维到中等维度的稠密向量比较。

+ 需要几何直观距离的场景。

+ 作为许多传统 ML 算法的默认距离度量。

**Python 实现：**

```
import math
from typing import List, Union

def euclidean_distance(x: List[Union[int, float]], y: List[Union[int, float]]) -> float:
    """
    计算两个向量之间的欧几里得距离（L2 距离）

    参数:
        x: 第一个向量，列表或数组类型
        y: 第二个向量，列表或数组类型

    返回:
        两个向量间的欧几里得距离

    异常:
        当两个向量长度不同时抛出 ValueError 异常
    """
    if len(x) != len(y):
        raise ValueError("Vectors must have the same length")
    return math.sqrt(sum((xi - yi) ** 2 for xi, yi in zip(x, y)))

# 示例
vector1 = [1, 2, 3]
vector2 = [4, 5, 6]
print(f">>> euclidean_distance({vector1}, {vector2})")
print(euclidean_distance(vector1, vector2))  # 输出: 5.196152422706632
```

### 🦊 1.3 余弦相似度 (Cosine Similarity) 与 余弦距离 (Cosine Distance)

**描述：**

衡量两个非零向量之间夹角的余弦值。它关注向量的方向而不是大小（模长）。余弦相似度的取值范围是 [-1, 1]，值越接近 1 表示方向越一致（越相似），越接近 -1 表示方向越相反，接近 0 表示方向接近正交（不相关）。对于非负向量（如 TF-IDF），范围是 [0, 1]。

余弦距离 通常定义为 1 - 余弦相似度，取值范围是 [0, 2]（或对于非负向量是 [0, 1]）。值越小表示距离越近（方向越相似）。

**公式：**

**余弦相似度：**

**余弦距离：**

**实现细节：**

+ 计算涉及点积、各向量的 L2 范数（模长）和除法。

+ **预处理：** 如果需要频繁计算某个向量与其他向量的余弦相似度，可以预先计算并存储该向量的 L2 范数。如果向量被 L2 归一化（即 ||x|| = 1），则余弦相似度简化为两个归一化向量的点积。

+ 计算复杂度为 O(n)。

+ 需要处理零向量的情况（分母为零）。

**NLP 及其他应用：**

+ **文档相似性：** 比较 TF-IDF 向量或文档嵌入 (Doc2Vec) 的标准方法。文档长度（向量模长）通常不被认为是衡量主题相似性的关键因素。

+ **词汇语义相似性：** 比较词嵌入 (Word2Vec, GloVe) 以评估词语之间的语义关系。

+ **信息检索：** 根据查询向量和文档向量的余弦相似度对搜索结果进行排序。

+ **推荐系统：** 计算用户-用户或物品-物品向量之间的相似度。

+ **人脸识别：** 比较人脸特征向量。

**优点：**

+ **对长度不敏感：** 只关心向量方向，不受向量长度（模长）的影响。这对于像文档这样长度可能差异很大的数据尤其有用。

+ **高维稀疏数据友好：** 在高维空间（如文本数据）中表现良好，受“维度诅咒”影响较小。对于稀疏向量，计算主要集中在非零元素上。

+ **归一化效果：** 相当于自动进行了 L2 归一化后的点积/欧氏距离计算（有数学关联）。

**缺点：**

+ **忽略大小信息：** 完全忽略向量的模长可能在某些场景下丢失重要信息（例如，一个词的出现频率可能很重要）。

+ **计算成本：** 相比 L1/L2，计算涉及除法和开方（计算范数），可能稍慢，但通常可通过库优化。

**实际场景：**

+ 文本分析、信息检索、主题建模。

+ 比较方向比大小更重要的向量表示（如词嵌入、用户偏好方向）。

+ 高维、稀疏的数据集。

**Python 实现：**

```
import math
from typing import List, Union

def cosine_similarity(x: List[Union[int, float]], y: List[Union[int, float]]) -> float:
    """计算两个向量之间的余弦相似度"""
    if len(x) != len(y):
        raise ValueError("Vectors must have the same length")
    dot_product = sum(xi * yi for xi, yi in zip(x, y))
    norm_x = math.sqrt(sum(xi ** 2 for xi in x))
    norm_y = math.sqrt(sum(yi ** 2 for yi in y))
    if norm_x == 0 or norm_y == 0:
        # 如果任一向量为零向量，理论上夹角未定义。
        # 实践中，若认为零向量与任何向量都不相似，返回 0；
        # 若认为它们无法比较或根据场景需要，可能返回 0 或抛出错误。
        # 这里返回 0 表示不相似。
        return 0.0
    return dot_product / (norm_x * norm_y)

def cosine_distance(x: List[Union[int, float]], y: List[Union[int, float]]) -> float:
    """
    计算两个向量之间的余弦距离 (1 - Cosine Similarity)

    参数:
        x: 第一个向量
        y: 第二个向量

    返回:
        两个向量间的余弦距离

    异常:
        当两个向量长度不同时抛出 ValueError 异常
    """
    # 注意：cosine_similarity 函数已处理长度和零向量问题
    return 1.0 - cosine_similarity(x, y)

# 示例
vector1 = [1, 2, 3]
vector2 = [4, 5, 6]
print(f">>> cosine_similarity({vector1}, {vector2})")
print(cosine_similarity(vector1, vector2)) # 输出: ~0.9746318461970762
print(f">>> cosine_distance({vector1}, {vector2})")
print(cosine_distance(vector1, vector2))  # 输出: ~0.02536815380292381
```

### 🐻 1.4 内积（Inner Product / Dot Product）

**描述：**

计算两个向量对应元素乘积的和。它反映了两个向量在方向上的对齐程度以及它们各自的大小（模长）。内积是未归一化的相似性度量。

**公式：**

**实现细节：**

+ 计算极其简单高效，只涉及乘法和加法。

+ 与余弦相似度的关系：如果向量已经 L2 归一化，则内积等于余弦相似度。

+ 计算复杂度为 O(n)。

**NLP 及其他应用：**

+ **注意力机制：** 在 Transformer 等模型中，查询 (Query) 和键 (Key) 向量的内积（通常是缩放点积）用于计算注意力权重，衡量它们之间的匹配度。

+ **神经网络层：** 全连接层本质上是输入向量与权重矩阵（每行是一个向量）的内积运算。

+ **推荐系统：** 可用于计算用户向量和物品向量的内积，作为预测评分或匹配度的一种方式，此时向量模长可能代表用户活跃度或物品流行度等信息。

+ **信息检索（特定场景）：** 如果向量模长本身有意义（例如，表示某种强度或置信度），内积可以直接用作相关性评分。

**优点：**

+ **计算极其高效：** 是最基本的向量运算之一。

+ **保留大小信息：** 与余弦相似度不同，内积同时考虑了向量的方向和大小。值的大小直接受向量模长的影响。

**缺点：**

+ **对向量长度极其敏感：** 模长较大的向量之间的内积会显著大于模长较小的向量，即使后者方向更一致。

+ **无界性：** 内积的值域可以是整个实数域 (-∞, +∞)，不像余弦相似度那样有固定的 [-1, 1] 或 [0, 1] 范围，使得直接比较不同向量对的内积结果有时意义不明确。

+ **解释性：** 如果没有归一化，内积的绝对值大小不直接对应于角度的接近程度。

**实际场景：**

+ 当向量的模长和方向都需要考虑，并且计算效率是首要考虑因素时。



## 一、内积（点积）

+ 作为更复杂模型（如神经网络、注意力机制）的基础计算单元。
+ 在某些推荐或匹配场景下，模长信息被认为是有用的。

### Python 实现

```python
from typing import List, Union

def inner_product(x: List[Union[int, float]], y: List[Union[int, float]]) -> float:
    """
    计算两个向量的内积（点积）

    参数:
        x: 第一个向量
        y: 第二个向量

    返回:
        两个向量的内积

    异常:
        当两个向量长度不同时抛出ValueError异常
    """
    if len(x) != len(y):
        raise ValueError("Vectors must have the same length")
    return sum(xi * yi for xi, yi in zip(x, y))

# 示例
vector1 = [1, 2, 3]
vector2 = [4, 5, 6]
print(f">>> inner_product({vector1}, {vector2})")
print(inner_product(vector1, vector2))  # 输出: 32.0
```

## 二、特定场景度量方法

除了上述核心方法外，还有一些针对特定数据类型和场景的度量方法。

### 2.1 汉明距离（Hamming Distance）

#### 描述

衡量两个等长字符串或序列在相同位置上不同字符的数量。它主要用于比较离散的、特别是二进制序列。

#### 公式

其中指示函数，当条件为真时取值为 1，否则为 0。或者简单地说，就是对应位置上符号不同的计数。

#### 实现细节

对于二进制向量，可以通过 `XOR` 操作然后计算结果中 `1` 的数量（population count 或 `popcount`）来高效实现。`hamming_distance = popcount(x XOR y)`。

计算复杂度为 。

#### NLP 及其他应用

拼写校正/编辑距离：是编辑距离的一种特例，只考虑替换操作，不考虑插入和删除。

信息论/编码理论：用于衡量码字之间的差异，评估信道的错误检测和纠正能力。

生物信息学：比较 DNA 或蛋白质序列的差异（当序列等长且只关心替换时）。

局部敏感哈希（LSH）：基于汉明距离的 `LSH` 用于在高维空间中进行近似最近邻搜索，尤其适用于二进制编码。（记得上次写的`Simhash`算法的文章吗！）

#### 优点

概念简单，计算非常快速，特别是对于二进制数据。

在需要精确匹配或计数差异位的场景中非常有用。

#### 缺点

仅适用于等长序列：无法直接比较长度不同的序列。

仅适用于离散数据：不适用于连续数值型向量。

不考虑差异程度：只关心位置上的元素是否相同，不关心差异有多大（例如，数字 `1` 和 `2` 的距离与 `1` 和 `9` 的距离相同，都是 `1`）。

对位置敏感：序列 `[1, 2, 3]` 和 `[3, 1, 2]` 的汉明距离是 `3`（完全不同），即使它们包含相同的元素。

#### 实际场景

比较基因序列。

校验数据传输错误。

数据库中查找相似记录（基于某些编码字段）。

#### Python 实现

```python
from typing import Sequence, Any

def hamming_distance(x: Sequence[Any], y: Sequence[Any]) -> int:
    """
    计算两个等长序列之间的汉明距离

    参数:
        x: 第一个序列 (如字符串, 列表)
        y: 第二个序列 (同类型)

    返回:
        两个序列对应位置元素不同的数量

    异常:
        当两个序列长度不同时抛出ValueError异常
    """
    if len(x) != len(y):
        raise ValueError("Sequences must have the same length")
    return sum(xi != yi for xi, yi in zip(x, y))

# 示例
seq1 = "karolin"
seq2 = "kathrin"
print(f">>> hamming_distance(\"{seq1}\", \"{seq2}\")")
print(hamming_distance(seq1, seq2))  # 输出: 3

# 二进制示例
bin1 = [1, 0, 1, 0, 1, 1, 0]
bin2 = [1, 1, 1, 0, 0, 1, 0]
print(f">>> hamming_distance({bin1}, {bin2})")
print(hamming_distance(bin1, bin2)) # 输出: 2
```

### 2.2 Jaccard 相似度（Jaccard Index）与 Jaccard 距离（Jaccard Distance）

#### 描述

用于比较有限样本集之间的相似性。`Jaccard` 相似系数定义为两个集合交集的大小除以它们并集的大小。它衡量的是两个集合的重叠程度。

`Jaccard` 距离定义为 `1 - Jaccard` 相似系数，衡量集合之间的不相似度。

#### 公式

Jaccard 相似度（Jaccard Index）：

`Jaccard` 距离：

其中对称差，即

#### 实现细节

输入通常是集合（sets）或者可以转换为集合表示的数据（如二进制向量，其中 `1` 表示元素存在）。

计算涉及集合的交集、并集和大小计数。

对于二进制向量 `x` 和 `y`，可以等价地计算：

`M11`: 两个向量在相同位置都为 `1` 的数量

`M01`: `x` 为 `0`, `y` 为 `1` 的数量

`M10`: `x` 为 `1`, `y` 为 `0` 的数量

`Jaccard Similarity = M11 / (M01 + M10 + M11)`

计算复杂度取决于集合操作的效率，通常与集合大小相关，大致为 。

#### NLP 及其他应用

文本相似性（词袋模型）：比较两段文本共有的词汇占总词汇的比例（忽略词频和顺序）。

文档去重/近似复制检测：快速判断两篇文档是否内容高度相似。常与 `MinHash` 等技术结合使用。

推荐系统：比较用户喜欢的物品集合，发现相似用户。

#### 优点

概念和计算相对简单。

直观地衡量集合的重叠程度。

在处理存在/缺失（二元）类型的数据时非常有效。

#### 缺点

忽略数值信息：只关心元素是否存在于集合中，不考虑元素的频率、数值大小或顺序。

受集合大小影响：对于大小差异悬殊的集合，即使小集合完全被大集合包含，相似度也可能不高（因为并集很大）。

不适用于负值或非集合数据：设计上用于处理集合或可转换为集合的数据。

#### 实际场景

检测抄袭或内容复用。

比较用户的购物篮、浏览历史等行为数据。

社交网络中衡量用户共同好友/兴趣的比例。

评估聚类结果（将簇视为元素集合）。

#### Python 实现

```python
from typing import Set, Any

def jaccard_similarity(set1: Set[Any], set2: Set[Any]) -> float:
    """计算两个集合之间的Jaccard相似系数"""
    intersection = len(set1.intersection(set2))
    union = len(set1.union(set2))
    if union == 0:
        # 如果两个集合都是空的，可以认为它们完全相似（相似度为1）
        # 或者根据场景定义为0或抛出错误。这里定义为空集与空集相似度为1。
        return 1.0
    return intersection / union

def jaccard_distance(set1: Set[Any], set2: Set[Any]) -> float:
    """
    计算两个集合之间的Jaccard距离（1 - Jaccard Similarity）

    参数:
        set1: 第一个集合
        set2: 第二个集合

    返回:
        两个集合的Jaccard距离

    异常:
        无（内部已处理空集情况）
    """
    # 注意：jaccard_similarity 函数已处理并集为空的情况
    return 1.0 - jaccard_similarity(set1, set2)

# 示例
set1 = {"cat", "dog", "fish"}
set2 = {"dog", "fish", "bird"}
print(f">>> jaccard_similarity({set1}, {set2})")
print(jaccard_similarity(set1, set2)) # 输出: 0.5 (交集{dog, fish}大小2, 并集{cat, dog, fish, bird}大小4)
print(f">>> jaccard_distance({set1}, {set2})")
print(jaccard_distance(set1, set2))  # 输出: 0.5
```

## 三、深入理解与实践

理解了基础概念后，我们来探讨一些更深入的问题和实践建议。

### 3.1 相似度 vs. 距离

#### 距离（Distance）

满足度量空间的性质（非负性、同一性、对称性、三角不等式）。

值越小，表示对象越相似。

典型例子：`L1` 距离、`L2` 距离、汉明距离、`Jaccard` 距离。

#### 相似度（Similarity）

通常值越大，表示对象越相似。

不一定满足所有距离度量性质（例如，余弦相似度不满足三角不等式）。

典型例子：余弦相似度、内积、`Jaccard` 相似系数。

#### 转换

常用的转换方式有 `Distance = 1 - Similarity`（当 `Similarity` 在 [0, 1] 或 [-1, 1] 且关注正相关时）或 `Distance = sqrt(1 - Similarity^2)`（用于将余弦相似度转为某种角度距离）。选择哪种转换取决于后续应用的需求。

### 3.2 如何选择合适的度量方法？

选择哪种度量方法取决于：

#### 数据类型

稠密、连续向量：`L2`（欧氏距离）是常用基准，尤其在低维空间。余弦相似度/距离适合忽略大小只关心方向的情况。

稀疏向量（如文本 `TF-IDF`）：余弦相似度/距离通常是首选，因为它不受文档长度影响且对高维稀疏数据友好。`L1` 距离有时也用作鲁棒替代。

二进制向量/序列：汉明距离（等长）、`Jaccard` 相似度/距离（不等长集合表示）是常用选择。

类别数据：汉明距离或自定义的距离函数。

#### 数据特性

是否需要考虑大小/模长：如果需要，内积或 `L1`/`L2` 距离可能更合适；如果不需要，余弦相似度/距离是好选择。

维度和稀疏性：高维稀疏数据通常用余弦相似度。高维稠密数据要警惕 `L1`/`L2` 的“维度诅咒”。

异常值敏感性：`L1` 对异常值比 `L2` 更鲁棒。

#### 任务目标

聚类：`L2` 常用于 `K-Means`，但余弦距离也常用于文本聚类。

信息检索/推荐：余弦相似度、内积常用于排序。`Jaccard` 用于集合重叠度。

近似最近邻搜索：`LSH` 技术常基于 `L2`、余弦、汉明或 `Jaccard` 距离。

计算效率要求：内积、`L1`、汉明距离通常计算最快。

### 3.3 数据预处理的重要性：归一化与标准化

归一化（Normalization）：通常指将数据缩放到 [0, 1] 或 [-1, 1] 区间，或使向量长度为 1（`L2` 归一化）。

`L2` 归一化（`x / ||x||_2`）对于余弦相似度计算至关重要（或者说，余弦相似度隐式地做了 `L2` 归一化）。它消除了向量长度的影响。如果先对所有向量进行 `L2` 归一化，则它们的余弦相似度就等于它们的内积，计算更快。

标准化（Standardization）：通常指将数据转换为均值为 0，标准差为 1（`Z-score`）。

对于 `L1` 和 `L2` 距离，如果特征的量纲（尺度）差异很大，标准化通常是必要的预处理步骤，以防止某些维度主导距离计算。

### 3.4 提及相关库

在实践中，通常不需要手动实现这些函数。强大的科学计算库提供了优化过的实现：

#### SciPy

`scipy.spatial.distance` 模块包含了 `euclidean`、`cityblock`（`L1`）、`cosine`、`hamming`、`jaccard` 等多种距离函数，可以直接作用于 `NumPy` 数组。

```python
from scipy.spatial import distance
import numpy as np

vec1 = np.array([1, 2, 3])
vec2 = np.array([4, 5, 6])

print("L1 (cityblock):", distance.cityblock(vec1, vec2))
print("L2 (euclidean):", distance.euclidean(vec1, vec2))
print("Cosine Distance:", distance.cosine(vec1, vec2)) # 注意：scipy.spatial.distance.cosine 返回的是余弦距离 (1 - sim)
print("Cosine Similarity:", 1 - distance.cosine(vec1, vec2))

set1 = {1, 2, 3}
set2 = {2, 3, 4}
# Jaccard 需要布尔向量或使用特定的函数
# 对于集合，可以直接用 len(s1.intersection(s2)) / len(s1.union(s2))
# 或者将集合转为布尔向量
u = list(set1.union(set2))
bool_vec1 = np.array([item in set1 for item in u], dtype=bool)
bool_vec2 = np.array([item in set2 for item in u], dtype=bool)
print("Jaccard Distance:", distance.jaccard(bool_vec1, bool_vec2))
```

输出：

```
L1 (cityblock): 9
L2 (euclidean): 5.196152422706632
Cosine Distance: 0.025368153802923787
Cosine Similarity: 0.9746318461970762
Jaccard Distance: 0.5
```

#### Scikit-learn

`sklearn.metrics.pairwise` 模块提供了 `euclidean_distances`、`manhattan_distances`、`cosine_similarity`、`cosine_distances` 等函数，可以高效计算一个矩阵内部或两个矩阵之间的所有成对距离/相似度。



```python
from sklearn.metrics.pairwise import euclidean_distances, manhattan_distances, cosine_similarity, cosine_distances
import numpy as np

X = np.array([[1, 2, 3], [4, 5, 6]])
Y = np.array([[1, 2, 3], [7, 8, 9]])

print("Pairwise Euclidean Distances:\n", euclidean_distances(X, Y))
print("Pairwise Manhattan Distances:\n", manhattan_distances(X, Y))
print("Pairwise Cosine Similarity:\n", cosine_similarity(X, Y))
print("Pairwise Cosine Distances:\n", cosine_distances(X, Y)) # 注意：sklearn 返回的是余弦距离
```

输出：

```
Pairwise Euclidean Distances:
 [[ 0.         10.39230485]
 [ 5.19615242  5.19615242]]
Pairwise Manhattan Distances:
 [[ 0. 18.]
 [9.  9.]]
Pairwise Cosine Similarity:
 [[1.         0.95941195]
 [0.97463185 0.99819089]]
Pairwise Cosine Distances:
 [[0.         0.04058805]
 [0.02536815 0.00180911]]
```

> 🙋♂️ 入群交流

+ 👸号菜单点击「社群」，扫码直接入群
+ 回复关键词「入群」，添加作者🛰️人工邀请

## 📒 总结与展望

### 🚀 技术全景图

本文系统梳理了常用的向量相似度与距离度量方法，构成了向量空间分析的基础工具箱：

+ `基于坐标差值:` `L1` (曼哈顿), `L2` (欧几里得) - 关注位置差异，受量纲影响，`L1`更鲁棒，`L2`更直观。

+ `基于方向:` 余弦相似度/距离 - 忽略长度，关注方向，适合高维稀疏数据如文本。

+ `基于方向与长度:` 内积 - 计算高效，同时考虑方向和长度，但对长度敏感且无界。

+ `基于离散/集合元素:` 汉明距离 (等长序列差异计数), Jaccard 相似度/距离 (集合重叠度)。

### 📓 学习汇总

+ 理解不同度量方法的数学定义、几何意义及其优缺点是关键。

+ 选择合适的度量方法需紧密结合数据类型（连续/离散/二元）、数据特性（维度/稀疏性/量纲/异常值）和具体任务需求。

+ 数据预处理（标准化/归一化）对于依赖数值大小的距离（`L1`/`L2`）通常是必要的。`L2`归一化是使用余弦相似度的常见步骤（或内含步骤）。

+ 熟悉并利用 SciPy 和 Scikit-learn 等库可以极大提高开发效率和计算性能。

### 🔥 动手挑战

1. **实现与对比:** 选择两种你认为最容易混淆的度量方法（例如 `L2` 和余弦距离），用相同的两个简单向量（如 `[1, 1]` 和 `[1, 5]`）手动计算它们的值，并解释结果差异的原因。

2. **文本相似度实验:** 找两段简短的文本，使用 `TF-IDF` 将它们转换为向量，然后分别计算它们之间的余弦相似度和欧几里得距离。思考哪个结果更能反映文本的主题相似性，为什么？（提示：可以使用 `sklearn.feature_extraction.text.TfidfVectorizer`）

3. **库函数应用:** 使用 `scipy.spatial.distance` 或 `sklearn.metrics.pairwise` 计算本文示例中 `vector1 = [1, 2, 3]` 和 `vector2 = [4, 5, 6]` 之间的 `L1`, `L2` 和余弦距离，验证结果与手动实现是否一致。

### ♻️ 互动问题

1. 在什么场景下，你会倾向于使用内积而不是余弦相似度来衡量向量间的相似性？请举例说明。

2. “维度诅咒”对 `L1` 和 `L2` 距离有什么具体影响？为什么余弦相似度在高维空间中通常表现更好？

3. 假设你在做一个商品推荐系统，用户行为被表示为他们购买过的商品集合。你会选择哪种度量方法（`Jaccard`, `Cosine`, `Euclidean`等）来计算用户之间的相似度？为什么？

> **名言**：
>
> 理解数据的内在“距离”，是让算法洞察模式、预测未来的第一步。
>
> ——筱可

**欢迎在留言区分享你的想法，每条留言我都会认真阅读！你的反馈是我创作的最大动力 ❤️**

### 💗 立即行动

1. **点赞、收藏、关注**，顺手分享给可能感兴趣的朋友。
2. 为账号加个星标或特别关注，获取更多精彩内容。
3. 以本文为灵感，整理一份属于自己的向量相似度/距离度量选择指南笔记。

### 🚗 行动召唤

📢 "与其等着 AI 改变世界，不如自己参与变革！在这里，让 AI 成为你弯道超车的秘密武器。"

## 📒 附录

（往期文章）

+ RAG系统召回率低？ BGE-M3 多语言多功能重塑文本检索
+ RAG去重小助手SimHash算法：轻松解决文本相似度检测与查重难题
+ 从模式匹配到语义理解：文本嵌入技术的演变与 RAG 系统优化
+ 别再瞎猜了！用 RAGAS 精准定位 RAG 系统短板
+ PDF提取神器！将双栏学术论文转换为单栏Markdown实战（上）
+ 从零打造RAG检索系统：BM25让检索快到飞起
+ 打造RAG应用必知：BM25算法实战解析，让你不落人后
+ 手把手教程：用MinerU构建自动化文档转换流水线
+ [飞桨版面检测分析的命令看不懂？恭喜！您已获得 arg](http://mp.weixin.qq.com/s/5b8PqqViSpvgMjpfJqOxCQ?token=1077873472&lang=zh_CN)parse 入门手册一本，快快学起来吧！
+ pdf提取逆天神器，基于飞浆平台的版面检测与版面识别(1)
+ [大模型应用极简开发快速入门](http://mp.weixin.qq.com/s/kHeVK6hManVUaci2p-42GQ)
+ [基于 Streamlit 和 De](https://mp.weixin.qq.com/s/38BcsiGF1WhKx4DaA-8l0A)epSeek 的智能文档助手开发实战指南
+ 告别低效沟通：提示词框架助你玩转deepseek
