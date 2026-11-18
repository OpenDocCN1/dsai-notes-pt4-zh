
# 你的RAG检索太“笨”？用K-Means聚类来“调教”

> 来源：[https://datawhaler.feishu.cn/docx/KDk8d3mVWoRByTxSidIcftsHnOp](https://datawhaler.feishu.cn/docx/KDk8d3mVWoRByTxSidIcftsHnOp)

## 🎯 文章目标

本文面向 NLP 开发者和 RAG 爱好者，旨在帮助大家：

+ 理解 `K-means` 聚类的核心原理：从无监督学习的基础出发，掌握 `K-means` 算法的工作机制及其在文本处理中的应用潜力。

+ 学会将 `K-means` 应用于文本数据：通过结合先进的文本嵌入模型（如 `BGE-M3`），实现对文本向量的有效聚类，挖掘数据中的语义结构。

+ 提升 `RAG` 系统的检索能力：探索如何利用 `K-means` 优化检索增强生成（`RAG`）系统，使其返回更多样化、更智能的检索结果。

> 💡 小提示
>
> 本文提供的所有代码均可直接运行，只需准备好必要的模型和依赖库（如 `scikit-learn` 和 `FlagEmbedding`）。动手实践是掌握技术的关键！
>
> 本次文章配套代码开源地址：https://github.com/li-xiu-qi/XiaokeAILabs/tree/main/datas/test_k_means

## 📄 主题

本文围绕 `K-means` 聚类在 NLP 和 RAG 系统中的应用展开，核心内容包括：

+ `K-means` 算法的理论基础与实现细节。

+ 使用 `BGE-M3` 模型生成高质量文本向量，并结合 `K-means` 进行聚类实践。

+ 在 `RAG` 系统中利用 `K-means` 增强检索多样性和实现智能查询路由的实战案例。

## 📚 摘要

检索增强生成（`RAG`）是一种强大的技术，但其检索模块往往受限于传统相似度排序的单调性，导致结果缺乏多样性或无法精准匹配用户意图。本文提出了一种基于 `K-means` 聚类的解决方案，通过对文本数据进行无监督分组，优化 `RAG` 的检索过程。我们将首先介绍 `K-means` 算法的基本原理及其数学目标，然后展示如何结合 `BGE-M3` 嵌入模型对文本进行向量化与聚类分析，最后通过两个 `RAG` 实战案例（多样性增强检索和聚类感知查询路由）演示其应用效果。无论你是想提升聚类技能，还是优化 `RAG` 系统的性能，本文都将提供系统的理论支持和实操指导，助你在 NLP 领域更进一步。

> 我们快快开始吧！

---

### 🚁 前言

在自然语言处理（`NLP`）领域，检索增强生成（`RAG`, Retrieval-Augmented Generation）技术近年来迅速崛起，成为解决知识密集型任务的重要工具。`RAG` 通过结合外部知识检索和生成模型的优势，能够在回答复杂问题时提供更准确、更具上下文相关性的内容。然而，许多开发者在使用 `RAG` 时会遇到一个常见的痛点：检索结果过于单一或不够“聪明”，导致生成的内容缺乏多样性或无法全面覆盖用户需求。

为什么会出现这样的问题？传统的检索方法通常基于简单的相似度排序，例如余弦相似度或欧几里得距离，虽然简单高效，但往往倾向于返回语义高度相似的文档。这种方式在某些场景下可能导致“信息冗余”，无法捕捉数据的多样性或潜在的语义结构。而 `K-means` 聚类算法，作为一种经典的无监督学习方法，可以帮助我们“调教”检索过程，通过对数据进行分组和语义挖掘，显著提升检索的多样性和智能性。

本文将深入探讨如何利用 `K-means` 聚类优化 `RAG` 系统的检索能力。我们将从 `K-means` 的基本原理入手，逐步讲解如何将其应用于文本向量聚类，并最终展示其在 `RAG` 系统中的实战应用。无论你是 `NLP` 开发者还是 `RAG` 爱好者，本文都将为你提供实用的技术洞见和可操作的代码示例，帮助你构建更强大的智能系统。

---

> ✨ 你好，我是筱可，欢迎来到「筱可 AI 研习社」！

> 🚀 标签关键词
>
> | AI 实战派开发者 | 技术成长陪伴者 | `RAG` 前沿探索者 | 文档处理先锋 |

## 🐱一、`K-means` 聚类的基本原理

### 有监督学习 vs 无监督学习

在深入 `K-means` 算法前，让我们先理解 有监督学习 和 无监督学习 的区别：

有监督学习（Supervised Learning）：

+ 算法使用带有标签（正确答案）的数据进行训练

+ 模型学习输入数据和输出标签之间的映射关系

+ 目标是预测新数据的标签或值

+ 典型算法：分类（如决策树、支持向量机）和回归（如线性回归）

+ 应用：垃圾邮件检测、图像识别、股票价格预测

无监督学习（Unsupervised Learning）：

+ 算法处理的数据没有标签

+ 模型通过识别数据中的内在结构和模式来学习

+ 目标是发现数据中隐藏的规律或分组

+ 典型算法：聚类（如 `k-means`、`DBSCAN`）和降维（如 `PCA`）

+ 应用：客户细分、异常检测、主题发现

`k-means` 是一种无监督学习算法，目标是将数据集划分为 `k` 个簇，使得每个数据点与最近的簇中心距离最小。它的运作过程可以分为以下几个详细步骤：

### K-means 算法步骤详解

`K-means` 是一种经典的聚类算法，其目标是通过迭代优化，将数据集划分为 `k` 个簇，使得每个数据点到其所属簇中心的距离平方和（即簇内平方和，WCSS）最小。数学上，`K-means` 的优化目标可以表示为：

其中：

`J` 是目标函数，表示所有数据点到其簇中心的总距离平方和；

`k` 是预设的簇数量；

`C_j` 是第 `j` 个簇中的数据点集合；

`x` 是数据点（通常是一个向量）；

`μ_j` 是第 `j` 个簇的中心向量；

`||x - μ_j||²` 表示数据点 `x` 到簇中心 `μ_j` 的欧几里得距离平方。

算法通过以下步骤逐步逼近 `J` 的局部最优解。

#### 1️⃣ 初始化：选择初始簇中心

初始化阶段的目标是为 `k` 个簇选择初始中心 `μ`。初始中心的选择对最终聚类结果和收敛速度有显著影响。

+ **随机选择法**：从数据集中随机挑选 `k` 个点作为初始簇中心。

```
import numpy as np

# data 是数据集，形状为 (n_samples, n_features)
# k 是簇数量
initial_centers = data[np.random.choice(data.shape[0], k, replace=False)]
```

+ **K-means++ 初始化**：为避免随机初始化导致的局部最优问题，`K-means++` 通过以下步骤选择更分散的初始中心：

1. 从数据集中随机选择第一个中心点 `μ₁`；

1. 对于每个数据点 `x`，计算其到最近已有中心的最小距离 `d(x)`；

1. 以概率 `P(x)` 选择下一个中心点，距离越远的点被选中的概率越大；

1. 重复步骤 2-3，直到选出 `k` 个中心点。

`K-means++` 的优势在于初始中心分布更均匀，能有效提高算法找到全局最优解的可能性。

#### 2️⃣ 分配：将数据点分配到最近的簇

在分配阶段，每个数据点 `x_i` 被分配到距离其最近的簇中心 `μ_j` 所在的簇。距离通常使用欧几里得距离平方计算，分配规则为：

`c_i = arg min_j ||x_i - μ_j||²`

其中：

`x_i` 是第 `i` 个数据点；

`μ_j` 是第 `j` 个簇的中心；

`c_i` 是使距离最小的簇编号。

实现代码：

```
import numpy as np

def assign_to_clusters(data, centers):
    # data: 数据矩阵，形状 (n_samples, n_features)
    # centers: 簇中心矩阵，形状 (k, n_features)
    distances = np.sum((data[:, np.newaxis, :] - centers) ** 2, axis=2)  # (n_samples, k)
    cluster_labels = np.argmin(distances, axis=1)  # 每个数据点的簇编号
    return cluster_labels
```

这段代码利用广播机制高效计算所有数据点到所有中心的距离平方，并为每个数据点分配最近的簇编号。

#### 3️⃣ 更新：重新计算簇中心

更新阶段根据当前簇的分配结果，重新计算每个簇的中心 `μ_j`。新的簇中心是该簇内所有数据点的均值，公式为：

`μ_j = (1 / |C_j|) * Σ_{x_i ∈ C_j} x_i`

其中：

`|C_j|` 是第 `j` 个簇中的数据点数量；

`Σ_{x_i ∈ C_j} x_i` 是簇内所有数据点的向量和。

实现代码：

```
def update_centers(data, cluster_labels, k):
    # data: 数据矩阵，形状 (n_samples, n_features)
    # cluster_labels: 簇编号数组，形状 (n_samples,)
    # k: 簇数量
    centers = np.zeros((k, data.shape[1]))
    for j in range(k):
        points_in_cluster = data[cluster_labels == j]
        if len(points_in_cluster) > 0:  # 避免空簇
            centers[j] = np.mean(points_in_cluster, axis=0)
    return centers
```

更新后的中心 `μ_j` 是簇内数据点的质心，使得簇内距离平方和局部最小化。这一过程直接影响目标函数 `J` 的下降。

#### 4️⃣ 迭代：重复直到收敛

重复执行分配和更新步骤，直到满足以下任一条件：

+ 簇中心的变化小于某个阈值（例如 `ε`）；

+ 数据点的簇分配不再变化；

+ 达到最大迭代次数。

完整的 `K-means` 算法实现：

```
def kmeans(data, k, max_iters=300, tol=1e-4):
    # 初始化中心
    centers = data[np.random.choice(data.shape[0], k, replace=False)]
    
    for _ in range(max_iters):
        # 分配数据点到簇
        cluster_labels = assign_to_clusters(data, centers)
        # 更新簇中心
        new_centers = update_centers(data, cluster_labels, k)
        # 检查收敛
        if np.sum((new_centers - centers) ** 2) < tol:
            break
        centers = new_centers
    
    return cluster_labels, centers
```

每次迭代都使目标函数 `J` 单调递减，最终收敛到局部最优解。

#### 5️⃣ 时间复杂度

+ 单次迭代复杂度：`O(n * k * d)`，其中 `n` 是数据点数量，`k` 是簇数量，`d` 是数据维度；

+ 总复杂度：`O(n * k * d * t)`，其中 `t` 是迭代次数，通常 `t` 较小。

### `K-means` 算法参数详解

在 `scikit-learn` 的 `KMeans` 类中，可以通过以下参数调整算法行为：

+ `n_clusters`：簇数量 `k`，需根据数据特性或肘部法则选择。

+ `init`：初始化方法，默认 `'k-means++'`，可选 `'random'` 或自定义数组。

+ `n_init`：运行次数，默认 10，取 `J` 最小的结果。

+ `max_iter`：最大迭代次数，默认 300。

+ `tol`：收敛阈值，默认 1e-4。

+ `random_state`：随机种子，确保可重复性。

+ `algorithm`：算法变体，默认 `'lloyd'`，可选 `'elkan'`（适用于低维数据）。

示例代码：

```
from sklearn.cluster import KMeans

kmeans = KMeans(
    n_clusters=5,
    init='k-means++',
    n_init=10,
    max_iter=300,
    tol=1e-4,
    random_state=42,
    algorithm='lloyd'
)
```

### `K-means` 的数学目标

`K-means` 的核心是优化目标函数 `J`：

算法通过交替执行分配步骤（固定 `μ`，优化 `c`）和更新步骤（固定 `c`，优化 `μ`），逐步减小 `J`，直到收敛。多次运行并选择最优 `J` 可以缓解初始中心选择导致的局部最优问题。

### 使用示例

以下是一个使用上述 `K-means` 实现对二维数据进行聚类的示例，包含数据生成、可视化和聚类结果展示。

```
import numpy as np
import matplotlib.pyplot as plt

# 定义 K-means 函数
def assign_to_clusters(data, centers):
    distances = np.sum((data[:, np.newaxis, :] - centers) ** 2, axis=2)
    return np.argmin(distances, axis=1)

def update_centers(data, cluster_labels, k):
    centers = np.zeros((k, data.shape[1]))
    for j in range(k):
        points_in_cluster = data[cluster_labels == j]
        if len(points_in_cluster) > 0:
            centers[j] = np.mean(points_in_cluster, axis=0)
    return centers

def kmeans(data, k, max_iters=300, tol=1e-4):
    centers = data[np.random.choice(data.shape[0], k, replace=False)]
    for _ in range(max_iters):
        cluster_labels = assign_to_clusters(data, centers)
        new_centers = update_centers(data, cluster_labels, k)
        if np.sum((new_centers - centers) ** 2) < tol:
            break
        centers = new_centers
    return cluster_labels, centers

# 生成示例数据
np.random.seed(42)
data1 = np.random.normal(0, 1, (100, 2)) + [2, 2]
data2 = np.random.normal(0, 1, (100, 2)) + [-2, -2]
data3 = np.random.normal(0, 1, (100, 2)) + [2, -2]
data = np.vstack([data1, data2, data3])

# 运行 K-means
k = 3
cluster_labels, centers = kmeans(data, k)

# 可视化结果
plt.scatter(data[:, 0], data[:, 1], c=cluster_labels, cmap='viridis', s=50, alpha=0.5)
plt.scatter(centers[:, 0], centers[:, 1], c='red', s=200, marker='x', label='Centers')
plt.title('K-means Clustering Result (k=3)')
plt.xlabel('X')
plt.ylabel('Y')
plt.legend()
plt.show()
```

![](img/0aec966cef22b327aff5cd569f414932.png)

这个示例生成三组二维正态分布数据，运行 `K-means` 算法将数据分为 3 个簇，并用 Matplotlib 可视化聚类结果，簇中心以红色 "x" 标记。

## 🐶二、文本向量生成与聚类实践

在开始愉快的实践之前我们先安装好对应的依赖：

```
pip install numpy scikit-learn pandas matplotlib FlagEmbedding
```

### 🧠2.1 使用 `BGE-M3` 模型生成文本向量

`BGE-M3` 是一个强大的嵌入模型，能够为文本生成密集向量（dense vectors）和 ColBERT 向量（contextualized late-interaction vectors）。让我们通过一个具体示例来理解它的作用。

假设我们有以下输入句子：

+ "What is BGE M3?"

+ "Defination of BM25"

在本地部署 `BGE-M3` 模型后，我们可以用以下代码提取向量：
