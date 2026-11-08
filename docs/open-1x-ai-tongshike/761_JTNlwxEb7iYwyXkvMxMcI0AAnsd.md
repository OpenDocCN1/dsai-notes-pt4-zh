# 你的RAG检索太“笨”？用K-Means聚类来“调教”

> 来源：[https://datawhaler.feishu.cn/docx/KDk8d3mVWoRByTxSidIcftsHnOp](https://datawhaler.feishu.cn/docx/KDk8d3mVWoRByTxSidIcftsHnOp)

# 你的RAG检索太“笨”？用K-Means聚类来“调教”

## 🎯 文章目标

本文面向 NLP 开发者和 RAG 爱好者，旨在帮助大家：

+   理解 K-means 聚类的核心原理：从无监督学习的基础出发，掌握 K-means 算法的工作机制及其在文本处理中的应用潜力。

+   学会将 K-means 应用于文本数据：通过结合先进的文本嵌入模型（如 BGE-M3），实现对文本向量的有效聚类，挖掘数据中的语义结构。

+   **提升 RAG 系统的检索能力**：探索如何利用 K-means 优化检索增强生成（RAG）系统，使其返回更多样化、更智能的检索结果。

> **💡 小提示

> **本文提供的所有代码均可直接运行，只需准备好必要的模型和依赖库（如 scikit-learn 和 FlagEmbedding）。动手实践是掌握技术的关键！

> 本次文章配套代码开源地址：https://github.com/li-xiu-qi/XiaokeAILabs/tree/main/datas/test_k_means

## 📄 主题

本文围绕 K-means 聚类在 NLP 和 RAG 系统中的应用 展开，核心内容包括：

+   K-means 算法的理论基础与实现细节。

+   使用 BGE-M3 模型生成高质量文本向量，并结合 K-means 进行聚类实践。

+   在 RAG 系统中利用 K-means 增强检索多样性和实现智能查询路由的实战案例。

## 📚 摘要

检索增强生成（RAG）是一种强大的技术，但其检索模块往往受限于传统相似度排序的单调性，导致结果缺乏多样性或无法精准匹配用户意图。本文提出了一种基于 K-means 聚类的解决方案，通过对文本数据进行无监督分组，优化 RAG 的检索过程。我们将首先介绍 K-means 算法的基本原理及其数学目标，然后展示如何结合 BGE-M3 嵌入模型对文本进行向量化与聚类分析，最后通过两个 RAG 实战案例（多样性增强检索和聚类感知查询路由）演示其应用效果。无论你是想提升聚类技能，还是优化 RAG 系统的性能，本文都将提供系统的理论支持和实操指导，助你在 NLP 领域更进一步。

> 我们快快开始吧！

---

### 🚁 前言

在自然语言处理（NLP）领域，检索增强生成（RAG, Retrieval-Augmented Generation）技术近年来迅速崛起，成为解决知识密集型任务的重要工具。RAG 通过结合外部知识检索和生成模型的优势，能够在回答复杂问题时提供更准确、更具上下文相关性的内容。然而，许多开发者在使用 RAG 时会遇到一个常见的痛点：检索结果过于单一或不够“聪明”，导致生成的内容缺乏多样性或无法全面覆盖用户需求。

为什么会出现这样的问题？传统的检索方法通常基于简单的相似度排序，例如余弦相似度或欧几里得距离，虽然简单高效，但往往倾向于返回语义高度相似的文档。这种方式在某些场景下可能导致“信息冗余”，无法捕捉数据的多样性或潜在的语义结构。而 K-means 聚类算法，作为一种经典的无监督学习方法，可以帮助我们“调教”检索过程，通过对数据进行分组和语义挖掘，显著提升检索的多样性和智能性。

本文将深入探讨如何利用 K-means 聚类优化 RAG 系统的检索能力。我们将从 K-means 的基本原理入手，逐步讲解如何将其应用于文本向量聚类，并最终展示其在 RAG 系统中的实战应用。无论你是 NLP 开发者还是 RAG 爱好者，本文都将为你提供实用的技术洞见和可操作的代码示例，帮助你构建更强大的智能系统。

---

> ✨ 你好，我是筱可，欢迎来到「筱可 AI 研习社」！

> **🚀 标签关键词

> **| AI 实战派开发者 | 技术成长陪伴者 | RAG 前沿探索者 | 文档处理先锋 |

## 🐱一、K-means 聚类的基本原理

### 有监督学习 vs 无监督学习

**在深入 k**-me**ans 算**法前，让我们先理解 有监督学习 和 无监督学习 的区别：

有监督学习（Supervised Learning）：

+   算法使用带有标签（正确答案）的数据进行训练

+   模型学习输入数据和输出标签之间的映射关系

+   目标是预测新数据的标签或值

+   典型算法：分类（如决策树、支持向量机）和回归（如线性回归）

+   应用：垃圾邮件检测、图像识别、股票价格预测

无监督学习（Unsupervised Learning）：

+   算法处理的数据没有标签

+   模型通过识别数据中的内在结构和模式来学习

+   目标是发现数据中隐藏的规律或分组

+   典型算法：聚类（如 k-means、DBSCAN）和降维（如 PCA）

+   应用：客户细分、异常检测、主题发现

k-means 是一种无监督学习算法，目标是将数据集划分为   个簇，使得每个数据点与最近的簇中心距离最小。它的运作过程可以分为以下几个详细步骤：

### K-means 算法步骤详解

K-means 是一种经典的聚类算法，其目标是通过迭代优化，将数据集划分为   个簇，使得每个数据点到其所属簇中心的距离平方和（即簇内平方和，WCSS）最小。数学上，K-means 的优化目标可以表示为：

其中：

是目标函数，表示所有数据点到其簇中心的总距离平方和；

是预设的簇数量；

是第   个簇中的数据点集合；

是数据点（通常是一个向量）；

是第   个簇的中心向量；

表示数据点   到簇中心   的欧几里得距离平方。

算法通过以下步骤逐步逼近   的局部最优解。

#### 1️⃣ 初始化：选择初始簇中心

初始化阶段的目标是为   个簇选择初始中心  。初始中心的选择对最终聚类结果和收敛速度有显著影响。

+   **随机选择法**：从数据集中随机挑选   个点作为初始簇中心。

```
import numpy as np

# data 是数据集，形状为 (n_samples, n_features)
# k 是簇数量
initial_centers = data[np.random.choice(data.shape[0], k, replace=False)]
```

+   **K-means++ 初始化**：为避免随机初始化导致的局部最优问题，K-means++ 通过以下步骤选择更分散的初始中心：

1.  从数据集中随机选择第一个中心点  ；

1.  对于每个数据点  ，计算其到最近已有中心的最小距离  ；

1.  以概率   选择下一个中心点，距离越远的点被选中的概率越大；

1.  重复步骤 2-3，直到选出   个中心点。

K-means++ 的优势在于初始中心分布更均匀，能有效提高算法找到全局最优解的可能性。

#### 2️⃣ 分配：将数据点分配到最近的簇

在分配阶段，每个数据点   被分配到距离其最近的簇中心   所在的簇。距离通常使用欧几里得距离平方计算，分配规则为：

其中：

是第   个数据点；

是第   个簇的中心；

是使距离最小的簇编号。

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

更新阶段根据当前簇的分配结果，重新计算每个簇的中心  。新的簇中心是该簇内所有数据点的均值，公式为：

其中：

是第   个簇中的数据点数量；

是簇内所有数据点的向量和。

实现代码：

```
def update_centers(data, cluster_labels, k):
    # data: 数据矩阵，形状 (n_samples, n_features)
    # cluster_labels: 簇编号数组，形状 (n_samples,)
    # k: 簇数量
    centers = np.zeros((k, data.shape[1]))
    for j in range(k):
        points_in_cluster = data[cluster_labels == j]
        if len(points_in_cluster) &gt; 0:  # 避免空簇
            centers[j] = np.mean(points_in_cluster, axis=0)
    return centers
```

更新后的中心   是簇内数据点的质心，使得簇内距离平方和局部最小化。这一过程直接影响目标函数   的下降。

#### 4️⃣ 迭代：重复直到收敛

重复执行分配和更新步骤，直到满足以下任一条件：

+   簇中心的变化小于某个阈值（例如  ）；

+   数据点的簇分配不再变化；

+   达到最大迭代次数。

完整的 K-means 算法实现：

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
        if np.sum((new_centers - centers) ** 2) &lt; tol:
            break
        centers = new_centers
    
    return cluster_labels, centers
```

每次迭代都使目标函数   单调递减，最终收敛到局部最优解。

#### 5️⃣ 时间复杂度

+   单次迭代复杂度： ，其中   是数据点数量，  是簇数量，  是数据维度；

+   总复杂度： ，其中   是迭代次数，通常   较小。

### K-means 算法参数详解

在 scikit-learn 的 KMeans 类中，可以通过以下参数调整算法行为：

+   **n_clusters**：簇数量  ，需根据数据特性或肘部法则选择。

+   **init**：初始化方法，默认 'k-means++'，可选 'random' 或自定义数组。

+   **n_init**：运行次数，默认 10，取   最小的结果。

+   **max_iter**：最大迭代次数，默认 300。

+   **tol**：收敛阈值，默认 1e-4。

+   **random_state**：随机种子，确保可重复性。

+   **algorithm**：算法变体，默认 'lloyd'，可选 'elkan'（适用于低维数据）。

示例代码：

```
from sklearn.cluster import KMeans

kmeans = KMeans(
    n_clusters=5,
    init=&#x27;k-means++&#x27;,
    n_init=10,
    max_iter=300,
    tol=1e-4,
    random_state=42,
    algorithm=&#x27;lloyd&#x27;
)
```

### K-means 的数学目标

K-means 的核心是优化目标函数  ：

算法通过交替执行分配步骤（固定  ，优化  ）和更新步骤（固定  ，优化  ），逐步减小  ，直到收敛。多次运行并选择最优   可以缓解初始中心选择导致的局部最优问题。

### 使用示例

以下是一个使用上述 K-means 实现对二维数据进行聚类的示例，包含数据生成、可视化和聚类结果展示。

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
        if len(points_in_cluster) &gt; 0:
            centers[j] = np.mean(points_in_cluster, axis=0)
    return centers

def kmeans(data, k, max_iters=300, tol=1e-4):
    centers = data[np.random.choice(data.shape[0], k, replace=False)]
    for _ in range(max_iters):
        cluster_labels = assign_to_clusters(data, centers)
        new_centers = update_centers(data, cluster_labels, k)
        if np.sum((new_centers - centers) ** 2) &lt; tol:
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
plt.scatter(data[:, 0], data[:, 1], c=cluster_labels, cmap=&#x27;viridis&#x27;, s=50, alpha=0.5)
plt.scatter(centers[:, 0], centers[:, 1], c=&#x27;red&#x27;, s=200, marker=&#x27;x&#x27;, label=&#x27;Centers&#x27;)
plt.title(&#x27;K-means Clustering Result (k=3)&#x27;)
plt.xlabel(&#x27;X&#x27;)
plt.ylabel(&#x27;Y&#x27;)
plt.legend()
plt.show()
```

![](img/0aec966cef22b327aff5cd569f414932.png)

这个示例生成三组二维正态分布数据，运行 K-means 算法将数据分为 3 个簇，并用 Matplotlib 可视化聚类结果，簇中心以红色 "x" 标记。

## 🐶二、文本向量生成与聚类实践

在开始愉快的实践之前我们先安装好对应的依赖：

```
pip install numpy scikit-learn pandas matplotlib FlagEmbedding
```

### 🧠2.1 使用 BGE-M3 模型生成文本向量

BGE-M3 是一个强大的嵌入模型，能够为文本生成密集向量（dense vectors）和 ColBERT 向量（contextualized late-interaction vectors）。让我们通过一个具体示例来理解它的作用。

假设我们有以下输入句子：

+   "What is BGE M3?"

+   "Defination of BM25"

在本地部署 BGE-M3 模型后，我们可以用以下代码提取向量：

```
from FlagEmbedding import BGEM3FlagModel
import numpy as np

model_path = r&quot;C:\Users\k\Desktop\BaiduSyncdisk\baidu_sync_documents\hf_models\bge-m3&quot;
model = BGEM3FlagModel(model_path, use_fp16=True)

sentences = [&quot;What is BGE M3?&quot;, &quot;Defination of BM25&quot;]
output = model.encode(
    sentences=sentences,
    batch_size=12,
    max_length=8192,
    return_dense=True,
    return_sparse=False,
    return_colbert_vecs=False
)

dense_vecs = output[&#x27;dense_vecs&#x27;]  # 形状: (2, 1024)
print(dense_vecs.shape)  # 输出: (2, 1024)
```

运行后，输出显示：

+   dense_vecs 的形状为  ，表示两个句子各有一个 1024 维的密集向量。

+   每个句子的向量可以通过 dense_vecs[0] 和 dense_vecs[1] 提取，形状均为  。

这些向量捕捉了句子的语义信息，为 k-means 聚类提供了高质量的输入。BGE-M3 模型经过大量文本数据的训练，能够将语义相似的文本映射到向量空间中相近的位置，这对于后续的聚类任务至关重要。高质量的文本向量是聚类效果的基础，能够帮助算法更好地识别文本之间的内在联系。

### 🧩2.2 将 k-means 应用于句子向量

现在有了向量表示，我们可以将其输入 k-means 算法。假设我们希望将句子分为两类（ ），可以用 Python 的 scikit-learn 库实现：

```
from sklearn.cluster import KMeans

# 输入BGE-M3生成的密集向量
kmeans = KMeans(n_clusters=2, random_state=42)
clusters = kmeans.fit_predict(dense_vecs)

print(f&quot;聚类结果: {clusters}&quot;)  # 输出示例: 聚类结果: [0 1]
```

运行后，假设结果为 [0, 1]，表示第一个句子属于簇 0，第二个句子属于簇 1。簇中心可以通过 kmeans.cluster_centers_ 获取，形状为  。这些簇中心可以被视为每个簇的代表性向量，可以用于理解每个簇的语义含义。在实际应用中，我们可以分析每个簇的中心向量，找出与该向量最相似的原始文本，从而理解该簇所代表的主题。

### 🧩2.3 余弦相似度与 K-means 聚类

scikit-learn 的 K-means 默认使用欧几里得距离，但在处理文本向量时，余弦相似度往往更合适。我们可以通过向量归一化，让标准 K-means 等效于使用余弦相似度：

```
from sklearn.cluster import KMeans
from sklearn.preprocessing import normalize

# 对向量进行归一化(L2范数)
normalized_vecs = normalize(dense_vecs, norm=&#x27;l2&#x27;)

# 在归一化向量上应用K-means
cos_kmeans = KMeans(n_clusters=2, random_state=42)
cos_clusters = cos_kmeans.fit_predict(normalized_vecs)

print(f&quot;基于余弦相似度的聚类结果: {cos_clusters}&quot;)
```

**原理解释**：当向量归一化后，它们都位于单位超球面上。对于这种情况，欧几里得距离的平方与余弦相似度之间存在数学关系：

因此，在归一化向量上最小化欧几里得距离等同于最大化余弦相似度。这种技巧让我们能够使用标准 K-means 实现基于余弦相似度的聚类。余弦相似度衡量的是两个向量方向上的夹角，更关注文本的语义方向而不是向量的绝对大小，这在文本聚类中往往更符合我们的需求，尤其是在处理长短不一的文本时，余弦相似度能够更好地捕捉文本的主题信息。

### 📊2.4 完整案例：新闻标题聚类

让我们通过一个完整的案例来展示如何将 BGE-M3 和 K-means 结合用于实际应用。这个例子将对 20 条不同主题的新闻标题进行聚类分析，期望算法能够识别出潜在的主题分组。

```
# 作者: 筱可
# 日期: 2025 年 3 月 30 日
# 版权所有 (c) 2025 筱可 &amp; 筱可AI研习社. 保留所有权利.
# Licensed under the Apache License, Version 2.0 (the &quot;License&quot;);
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#        http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an &quot;AS IS&quot; BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

from FlagEmbedding import BGEM3FlagModel
import numpy as np
from sklearn.cluster import KMeans
from sklearn.preprocessing import normalize
from sklearn.metrics import silhouette_score
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA

# 加载模型
model_path = r&quot;C:\Users\k\Desktop\BaiduSyncdisk\baidu_sync_documents\hf_models\bge-m3&quot;
model = BGEM3FlagModel(model_path, use_fp16=True)

# 样本新闻标题集合（涵盖科技、体育、政治、娱乐和健康五个主题）
news_titles = [
    # 科技主题
    &quot;苹果发布最新iPhone 15系列，搭载A17芯片&quot;,
    &quot;谷歌推出新一代人工智能助手，支持自然语言理解&quot;,
    &quot;特斯拉自动驾驶技术获突破，事故率降低30%&quot;,
    &quot;微软宣布收购AI初创公司，强化云服务能力&quot;,

    # 体育主题
    &quot;梅西在巴黎首秀进球，球迷欢呼雀跃&quot;,
    &quot;东京奥运会闭幕，中国队金牌榜位列第二&quot;,
    &quot;NBA季后赛：湖人击败热火，夺得总冠军&quot;,
    &quot;国足世预赛不敌日本队，出线形势严峻&quot;,

    # 政治主题
    &quot;中美元首通话，就双边关系交换意见&quot;,
    &quot;欧盟通过新气候法案，承诺2050年实现碳中和&quot;,
    &quot;联合国大会召开，各国领导人讨论全球治理&quot;,
    &quot;英国宣布脱欧后新贸易政策，加强与亚洲合作&quot;,

    # 娱乐主题
    &quot;新电影《沙丘》全球热映，票房突破4亿美元&quot;,
    &quot;流行歌手泰勒·斯威夫特发布新专辑，粉丝热情高涨&quot;,
    &quot;网飞热门剧集《鱿鱼游戏》创收视纪录&quot;,
    &quot;奥斯卡颁奖典礼举行，《无依之地》获最佳影片&quot;,

    # 健康主题
    &quot;新研究发现常规锻炼可降低阿尔茨海默病风险&quot;,
    &quot;全球新冠疫苗接种突破30亿剂，发展中国家覆盖率仍低&quot;,
    &quot;医学专家建议减少超加工食品摄入，降低慢性病风险&quot;,
    &quot;心理健康问题在年轻人中上升，专家呼吁加强关注&quot;
]

# 生成文本向量
news_vectors = model.encode[news_titles](&#x27;dense_vecs&#x27;)

# 对向量进行归一化，准备基于余弦相似度的聚类
normalized_vectors = normalize(news_vectors, norm=&#x27;l2&#x27;)

# 使用归一化向量进行K-means聚类(等效于基于余弦相似度)
kmeans = KMeans(
    n_clusters=5,
    init=&#x27;k-means++&#x27;,
    n_init=10,
    random_state=42
)
clusters = kmeans.fit_predict(normalized_vectors)

# 创建结果DataFrame
results_df = pd.DataFrame({
    &#x27;title&#x27;: news_titles,
    &#x27;cluster&#x27;: clusters
})

# 打印每个簇的新闻标题
for cluster_id in range(5):
    print(f&quot;\n=== 簇 {cluster_id} ===&quot;)
    cluster_titles = results_df[results_df[&#x27;cluster&#x27;] == cluster_id][&#x27;title&#x27;]
    for title in cluster_titles:
        print(f&quot;- {title}&quot;)

# 计算聚类评估指标
silhouette_avg = silhouette_score(normalized_vectors, clusters)
print(f&quot;\n聚类轮廓系数: {silhouette_avg:.4f}&quot;)

# 可视化聚类结果
# 显示中文
plt.rcParams[&#x27;font.sans-serif&#x27;] = [&#x27;SimHei&#x27;]
plt.rcParams[&#x27;axes.unicode_minus&#x27;] = False
# 使用PCA降维以便可视化
pca = PCA(n_components=2)
reduced_vectors = pca.fit_transform(news_vectors)

# 设置绘图样式
plt.figure(figsize=(10, 8))
colors = [&#x27;#ff9999&#x27;, &#x27;#66b3ff&#x27;, &#x27;#99ff99&#x27;, &#x27;#ffcc99&#x27;, &#x27;#c2c2f0&#x27;]
markers = [&#x27;o&#x27;, &#x27;s&#x27;, &#x27;^&#x27;, &#x27;D&#x27;, &#x27;P&#x27;]

# 绘制数据点
for i in range(5):
    # 获取当前簇的点
    cluster_points = reduced_vectors[clusters == i]
    # 绘制该簇的所有点
    plt.scatter(
        cluster_points[:, 0],
        cluster_points[:, 1],
        c=colors[i],
        marker=markers[i],
        label=f&#x27;主题 {i}&#x27;,
        s=100,
        edgecolors=&#x27;black&#x27;
    )

# 绘制簇中心
centers_reduced = pca.transform(kmeans.cluster_centers_)
plt.scatter(
    centers_reduced[:, 0],
    centers_reduced[:, 1],
    c=&#x27;black&#x27;,
    marker=&#x27;X&#x27;,
    s=200,
    label=&#x27;簇中心&#x27;
)

# 添加图例和标题
plt.legend(fontsize=12)
plt.title(&#x27;新闻标题聚类结果 (PCA降至2维)&#x27;, fontsize=16)
plt.tight_layout()
plt.savefig(&#x27;news_clusters_visualization.png&#x27;, dpi=300)
plt.show()
```

输出：

```
=== 簇 0 ===
- 东京奥运会闭幕，中国队金牌榜位列第二
- 国足世预赛不敌日本队，出线形势严峻

=== 簇 1 ===
- 梅西在巴黎首秀进球，球迷欢呼雀跃
- NBA季后赛：湖人击败热火，夺得总冠军
- 新电影《沙丘》全球热映，票房突破4亿美元
- 流行歌手泰勒·斯威夫特发布新专辑，粉丝热情高涨
- 网飞热门剧集《鱿鱼游戏》创收视纪录
- 奥斯卡颁奖典礼举行，《无依之地》获最佳影片

=== 簇 2 ===
- 苹果发布最新iPhone 15系列，搭载A17芯片
- 谷歌推出新一代人工智能助手，支持自然语言理解
- 特斯拉自动驾驶技术获突破，事故率降低30%
- 微软宣布收购AI初创公司，强化云服务能力
- 欧盟通过新气候法案，承诺2050年实现碳中和
- 英国宣布脱欧后新贸易政策，加强与亚洲合作

=== 簇 3 ===
- 中美元首通话，就双边关系交换意见
- 联合国大会召开，各国领导人讨论全球治理
- 全球新冠疫苗接种突破30亿剂，发展中国家覆盖率仍低

=== 簇 4 ===
- 新研究发现常规锻炼可降低阿尔茨海默病风险
- 医学专家建议减少超加工食品摄入，降低慢性病风险
- 心理健康问题在年轻人中上升，专家呼吁加强关注

聚类轮廓系数: 0.0564
```

#### 可视化聚类结果

![](img/c9df5dbfd89963f2f69e6a29a5ba2730.png)

#### 结果分析与应用

这个例子展示了如何将文本数据转换为向量表示，并应用 k-means 进行聚类分析。从结果可以看出，尽管我们没有事先提供任何标签信息，算法成功地将新闻标题按主题分组，识别出了科技、体育、政治、娱乐和健康五个不同主题。虽然轮廓系数较低，这可能是由于新闻标题的简洁性和主题之间的潜在关联性，但从实际的聚类结果来看，算法在一定程度上捕捉到了语义上的相似性。

这种聚类方法在实际应用中有多种用途：

1.  **内容推荐系统**：根据用户阅读的内容所属簇，推荐同一簇内的其他文章。例如，如果用户正在阅读一篇关于 iPhone 15 的新闻，系统可以推荐其他关于科技公司发布新产品的文章。

1.  **自动归档与组织**：为大量文档或新闻自动生成主题分类，方便用户查找和管理信息。例如，可以将每天的新闻自动按照政治、经济、体育等主题进行归档。

1.  **热点话题发现**：通过分析哪些簇包含更多近期文章，识别热门话题。例如，在社交媒体数据分析中，可以发现当前用户讨论最多的几个热点话题。

1.  **舆情分析**：分析不同主题簇的情感倾向和数量变化，了解公众对不同话题的看法和关注度。例如，可以分析不同政治话题下的用户评论，判断舆论走向。

1.  **文本摘要**：可以从每个簇中选择最具代表性的文本作为该簇的摘要，帮助用户快速了解簇的主要内容。

1.  **构建知识图谱**：可以将每个簇视为一个概念，簇内的文本作为该概念的实例，从而辅助构建知识图谱。

在接下来的 RAG 实战部分，我们将探讨如何将这种聚类技术应用于构建更智能的检索增强生成系统。

## 🐶三、RAG 实战：k-means 的妙用

### 3.1 多样性增强的检索方法

```
# 作者: 筱可
# 日期: 2025 年 3 月 30 日
# 版权所有 (c) 2025 筱可 &amp; 筱可AI研习社. 保留所有权利.
# Licensed under the Apache License, Version 2.0 (the &quot;License&quot;);
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#        http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an &quot;AS IS&quot; BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

from FlagEmbedding import BGEM3FlagModel
import numpy as np
from sklearn.cluster import KMeans
from sklearn.metrics.pairwise import cosine_similarity

# 加载模型
# 使用BGE-M3模型进行文本向量化，该模型支持多语言文本编码
model_path = r&quot;C:\Users\k\Desktop\BaiduSyncdisk\baidu_sync_documents\hf_models\bge-m3&quot;
model = BGEM3FlagModel(model_path, use_fp16=True)  # 使用FP16加速推理

def diversity_enhanced_retrieval(query, doc_vectors, doc_texts, top_k=5, diversity_clusters=3):
    &quot;&quot;&quot;
    返回多样化的检索结果，覆盖不同语义簇。
    该方法通过K-means聚类确保结果多样性，同时保持相关性。

    Args:
        query (str): 查询文本
        doc_vectors (np.ndarray): 文档向量集合
        doc_texts (list): 文档文本列表
        top_k (int): 返回的文档数量
        diversity_clusters (int): 聚类数量，用于增加结果多样性

    Returns:
        list: 选择的文档索引列表
    &quot;&quot;&quot;
    # 编码查询并计算相似度
    # 将查询文本转换为向量表示
    query_vec = model.encode[[query]][&#x27;dense_vecs&#x27;](0)
    # 计算查询向量与所有文档向量的余弦相似度
    similarities = cosine_similarity[[query_vec], doc_vectors](0)

    # 获取候选文档
    # 选择相似度最高的top_n个文档作为候选集
    top_n = min(top_k * 3, len(doc_vectors))  # 取top_k的3倍或全部文档
    candidate_indices = np.argsort[similarities][-top_n:](::-1)  # 按相似度降序排列
    candidate_vectors = doc_vectors[candidate_indices]  # 获取候选文档向量

    # 执行K-means聚类
    # 对候选文档进行聚类，以确保语义多样性
    kmeans = KMeans(n_clusters=min(diversity_clusters, len(candidate_vectors)),
                    random_state=42, n_init=10)  # 设置聚类参数
    clusters = kmeans.fit_predict(candidate_vectors)  # 执行聚类并预测簇标签

    # 从每个簇中选择最相似文档
    selected_indices = []
    cluster_dict = {}

    # 按簇分组并记录相似度
    # 将每个文档按簇ID分组，并保存其原始索引和相似度
    for idx, cluster_id in enumerate(clusters):
        cluster_dict.setdefault(cluster_id, []).append((candidate_indices[idx], similarities[candidate_indices[idx]]))

    # 从每个簇中选最佳文档
    # 对每个簇，选择相似度最高的文档
    for cluster_id in range(min(diversity_clusters, len(cluster_dict))):
        if cluster_dict.get(cluster_id):
            best_doc = max[cluster_dict[cluster_id], key=lambda x: x[1]](0)
            selected_indices.append(best_doc)

    # 补充不足的文档
    # 如果从聚类中选出的文档数量不足top_k，从剩余候选文档中补充
    remaining = [i for i in candidate_indices if i not in selected_indices]
    if len(selected_indices) &lt; top_k and remaining:
        remaining_similarities = [similarities[i] for i in remaining]
        extra_indices = [remaining[i] for i in np.argsort[remaining_similarities](-top_k + len(selected_indices):)]
        selected_indices.extend(extra_indices)

    return selected_indices[:top_k]

def generate_sample_news_data(n_docs=20):
    &quot;&quot;&quot;
    生成模拟新闻标题及其向量表示

    Args:
        n_docs (int): 需要生成的文档数量

    Returns:
        tuple: (文档向量数组, 文档文本列表)
    &quot;&quot;&quot;
    # 中文新闻标题示例
    news_titles = [
        &quot;人工智能在医学研究中取得重大突破&quot;,
        &quot;新兴科技初创公司融资达数十亿&quot;,
        &quot;气候变化影响全球农业发展&quot;,
        &quot;2025年量子计算技术取得新进展&quot;,
        &quot;人工智能模型预测股市趋势&quot;,
        &quot;可再生能源超过传统化石燃料&quot;,
        &quot;癌症治疗新突破得益于AI技术&quot;,
        &quot;科技巨头面临新的隐私法规&quot;,
        &quot;全球数据泄露事件创历史新高&quot;,
        &quot;AI助手变得更加人性化&quot;,
        &quot;自动驾驶汽车革新交通运输&quot;,
        &quot;气候技术获得巨额投资支持&quot;,
        &quot;新AI算法解决复杂问题&quot;,
        &quot;数字化时代网络安全威胁上升&quot;,
        &quot;医疗AI减少诊断错误&quot;,
        &quot;技术创新推动经济增长&quot;,
        &quot;AI驱动的机器人进入职场&quot;,
        &quot;可持续技术解决方案受到关注&quot;,
        &quot;数据科学改变商业策略&quot;,
        &quot;量子AI研究开启新领域&quot;
    ]

    # 如果需要更多文档，重复标题列表
    news_titles = news_titles[:n_docs] if len(news_titles) &gt;= n_docs else news_titles * (n_docs // len(news_titles) + 1)
    news_titles = news_titles[:n_docs]

    # 生成向量
    # 使用模型将文本转换为向量表示
    news_vectors = model.encode[news_titles](&#x27;dense_vecs&#x27;)
    return news_vectors, news_titles

# 测试代码
if __name__ == &quot;__main__&quot;:
    # 生成测试数据
    doc_vectors, doc_texts = generate_sample_news_data(n_docs=20)

    # 测试查询
    query = &quot;人工智能研究进展&quot;
    result_indices = diversity_enhanced_retrieval(query, doc_vectors, doc_texts, top_k=5, diversity_clusters=3)

    # 打印结果
    print(&quot;查询:&quot;, query)
    print(&quot;\n检索结果:&quot;)
    # 计算查询与所有文档的相似度
    similarities = cosine_similarity([model.encode[[query]][&#x27;dense_vecs&#x27;](0)], doc_vectors)[0]

    # 按相似度降序排序结果
    sorted_results = sorted([(idx, similarities[idx]) for idx in result_indices],
                            key=lambda x: x[1], reverse=True)

    # 打印排序后的结果
    for idx, sim in sorted_results:
        print(f&quot;文档 {idx}: {doc_texts[idx]} (相似度: {sim:.4f})&quot;)
```

输出：

```

查询: 人工智能研究进展

检索结果:
文档 0: 人工智能在医学研究中取得重大突破 (相似度: 0.7393)
文档 12: 新AI算法解决复杂问题 (相似度: 0.6835)
文档 6: 癌症治疗新突破得益于AI技术 (相似度: 0.6642)
文档 10: 自动驾驶汽车革新交通运输 (相似度: 0.5270)
文档 7: 科技巨头面临新的隐私法规 (相似度: 0.5037)
```

这种多样性检索方法能够确保 RAG 系统获取不同角度的信息，避免回答过于单一，大大提高生成内容的全面性。在传统的基于相似度的检索中，返回的结果可能高度相似，导致信息冗余。而通过引入 K-means 聚类，我们可以将检索结果划分为几个不同的语义簇，并从每个簇中选择最具代表性的文档，从而保证了检索结果的多样性，使得 RAG 系统能够更全面地理解用户查询并生成更丰富的回答。这对于需要多方面信息的复杂问题尤其有效。

### 3.2 聚类感知的查询路由系统

复杂的 RAG 系统常常包含多种知识源，基于聚类的路由可以智能选择最相关的知识库：

```
# 作者: 筱可
# 日期: 2025 年 3 月 30 日
# 版权所有 (c) 2025 筱可 &amp; 筱可AI研习社. 保留所有权利.
# Licensed under the Apache License, Version 2.0 (the &quot;License&quot;);
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#        http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an &quot;AS IS&quot; BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

from FlagEmbedding import BGEM3FlagModel
import numpy as np
from sklearn.cluster import KMeans
from sklearn.metrics.pairwise import cosine_similarity

# 初始化 BGE-M3 模型
model_path = r&quot;C:\Users\k\Desktop\BaiduSyncdisk\baidu_sync_documents\hf_models\bge-m3&quot;
model = BGEM3FlagModel(model_path, use_fp16=True)

class ClusterAwareRouter:
    &quot;&quot;&quot;基于聚类的查询路由系统，将查询导向最相关的专业知识库&quot;&quot;&quot;

    def __init__(self, knowledge_bases, n_clusters=5):
        &quot;&quot;&quot;
        初始化路由系统

        参数:
            knowledge_bases: 字典 {知识库名称: {&quot;vectors&quot;: 文档向量, &quot;documents&quot;: 文档}}
            n_clusters: 每个知识库的簇数，默认值为5
        &quot;&quot;&quot;
        self.knowledge_bases = knowledge_bases
        self.kb_centers = {}
        self.n_clusters = n_clusters

        # 为每个知识库创建聚类模型
        for kb_name, kb_data in knowledge_bases.items():
            if len(kb_data[&quot;vectors&quot;]) &lt; n_clusters:
                raise ValueError(f&quot;知识库 {kb_name} 的向量数量少于指定簇数 {n_clusters}&quot;)

            kmeans = KMeans(n_clusters=n_clusters, random_state=42, n_init=10)
            kmeans.fit(kb_data[&quot;vectors&quot;])
            self.kb_centers[kb_name] = kmeans.cluster_centers_

    def route_query(self, query, top_k=1):
        &quot;&quot;&quot;
        将查询路由到最相关的知识库

        参数:
            query: 字符串，用户输入的查询
            top_k: 返回前k个最相关知识库，默认为1

        返回:
            如果 top_k=1，返回最佳知识库名称和相似度分数 (str, float)
            如果 top_k&gt;1，返回按相似度排序的知识库名称和相似度分数对列表 [(str, float), ...]
        &quot;&quot;&quot;
        # 将查询编码为向量
        query_vec = model.encode[[query], max_length=512][&#x27;dense_vecs&#x27;](0)

        # 计算查询与各知识库簇中心的最大相似度
        similarities = {}
        for kb_name, centers in self.kb_centers.items():
            sim_scores = cosine_similarity[[query_vec], centers](0)
            similarities[kb_name] = np.max(sim_scores)

        # 按相似度排序并选择top_k结果
        sorted_kbs = sorted(similarities.items(), key=lambda x: x[1], reverse=True)

        if top_k == 1:
            return sorted_kbs[0][0], sorted_kbs[0][1]  # 返回最佳知识库名称和相似度分数
        return [(kb[0], kb[1]) for kb in sorted_kbs[:top_k]]  # 返回知识库名称和相似度分数对

    def retrieve_documents(self, query, kb_name, top_k=3):
        &quot;&quot;&quot;
        在指定知识库中检索与查询最相关的文档

        参数:
            query: 用户查询
            kb_name: 知识库名称
            top_k: 返回前k个最相关文档，默认为3

        返回:
            包含文档和相似度分数的列表，格式为 [(document, similarity_score), ...]
        &quot;&quot;&quot;
        # 检查知识库是否存在
        if kb_name not in self.knowledge_bases:
            return []

        # 将查询编码为向量
        query_vec = model.encode[[query], max_length=512][&#x27;dense_vecs&#x27;](0)

        # 获取知识库中的文档向量和文档内容
        kb_vectors = self.knowledge_bases[kb_name][&quot;vectors&quot;]
        kb_documents = self.knowledge_bases[kb_name][&quot;documents&quot;]

        # 计算查询与知识库中所有文档的相似度
        similarities = cosine_similarity[[query_vec], kb_vectors](0)

        # 获取相似度排名最高的文档索引
        top_indices = np.argsort[similarities][::-1](:top_k)

        # 返回相关文档及其相似度分数
        return [(kb_documents[i], similarities[i]) for i in top_indices]

# 示例使用
if __name__ == &quot;__main__&quot;:
    # 创建示例知识库数据
    sample_docs = {
        &quot;医学&quot;: {
            &quot;documents&quot;: [
                &quot;糖尿病的治疗方法包括胰岛素治疗和饮食控制。&quot;,
                &quot;流感的常见症状包括发热、咳嗽和疲劳。&quot;,
                &quot;心脏病预防需要定期锻炼和健康饮食。&quot;
            ],
            &quot;vectors&quot;: None
        },
        &quot;技术&quot;: {
            &quot;documents&quot;: [
                &quot;Python 被广泛用于机器学习和数据分析。&quot;,
                &quot;云计算提供了可扩展的基础设施解决方案。&quot;,
                &quot;AI模型需要大量的计算资源。&quot;
            ],
            &quot;vectors&quot;: None
        }
    }

    # 生成向量表示
    for kb_name in sample_docs:
        texts = sample_docs[kb_name][&quot;documents&quot;]
        vectors = model.encode[texts, batch_size=3](&#x27;dense_vecs&#x27;)
        sample_docs[kb_name][&quot;vectors&quot;] = vectors

    # 初始化路由器
    router = ClusterAwareRouter(sample_docs, n_clusters=2)

    # 测试查询
    test_queries = [
        &quot;感冒的症状有哪些？&quot;,
        &quot;AI开发最好的编程语言是什么？&quot;
    ]

    # 执行路由
    for query in test_queries:
        best_kb, similarity = router.route_query(query)  # 获取最佳知识库及其相似度
        print(f&quot;查询: {query}&quot;)
        print(f&quot;路由到的知识库: {best_kb} (相似度: {similarity:.4f})&quot;)

        # 检索相关文档并显示
        relevant_docs = router.retrieve_documents(query, best_kb)
        print(&quot;检索结果:&quot;)
        for i, (doc, score) in enumerate(relevant_docs, 1):
            print(f&quot;{i}. 文档: {doc} (相似度: {score:.4f})&quot;)
        print(&quot;\n&quot; + &quot;-&quot;*50 + &quot;\n&quot;)
```

输出：

```

查询: 感冒的症状有哪些？
路由到的知识库: 医学 (相似度: 0.6908)
检索结果:
1. 文档: 流感的常见症状包括发热、咳嗽和疲劳。 (相似度: 0.6908)
2. 文档: 心脏病预防需要定期锻炼和健康饮食。 (相似度: 0.4443)
3. 文档: 糖尿病的治疗方法包括胰岛素治疗和饮食控制。 (相似度: 0.3520)

--------------------------------------------------

查询: AI开发最好的编程语言是什么？
路由到的知识库: 技术 (相似度: 0.6068)
检索结果:
1. 文档: AI模型需要大量的计算资源。 (相似度: 0.5529)
2. 文档: Python 被广泛用于机器学习和数据分析。 (相似度: 0.5068)
3. 文档: 云计算提供了可扩展的基础设施解决方案。 (相似度: 0.3390)
```

在包含多种专业领域文档的大型 RAG 系统中，这种路由机制可以显著提高回答的专业性，避免跨领域知识的混淆。通过对每个知识库中的文档进行聚类，我们可以提取每个知识库的代表性语义簇。当用户提出查询时，系统会将查询向量与各个知识库的簇中心进行比较，选择与查询最相关的知识库，从而将检索范围限定在最有可能包含答案的知识库中，提高了检索效率和准确性。这种方法尤其适用于拥有多个垂直领域知识库的场景，例如一个包含医学、法律和技术文档的问答系统。当然了，应该不会有人把多个领域全部投入一个系统里面吧。。。

其实我主要是想给大家一些启发，比如在多轮对话的时候可以使用聚类的方式载入历史对话会有比较好的效果。k-means咋RAG的场景下还有很多的应用，大家可以自由探索，相信你会有许多收获的。

---

> **🙋♂️ 入群交流**

+   👸号菜单点击「社群」，扫码直接入群

+   回复关键词「入群」，添加作者🛰️人工邀请，注意备注：入群

## 📒 总结与展望

🚀 技术全景图

+   无监督学习： k-means 聚类算法提供了一种无需标签即可对数据进行分组的有效方法。

+   文本嵌入模型： BGE-M3 模型能够生成高质量的文本向量，为聚类等下游任务提供强大的特征表示。

+   检索增强生成（RAG）： 通过优化检索过程，聚类技术可以显著提升 RAG 系统的性能和生成内容的多样性。

📓 学习汇总

+   掌握了 k-means 聚类算法的基本原理和实现步骤。

+   学会了使用 BGE-M3 模型生成文本的密集向量表示。

+   了解了如何在 NLP 任务中应用 k-means 进行文本聚类。

+   探索了如何利用聚类技术增强 RAG 系统的检索效果，包括提升检索结果的多样性和实现智能查询路由。

🔥 动手挑战

+   尝试修改新闻标题聚类案例中的 n_clusters 参数，观察聚类结果的变化，并分析不同 K 值对结果的影响。

+   基于提供的多样性增强检索代码，尝试使用不同的 diversity_clusters 值，分析检索结果的多样性变化。

+   扩展聚类感知的查询路由系统，添加更多的知识库，并设计更复杂的查询场景进行测试。

♻️ 互动问题

+   在你的实际项目中，你认为 k-means 聚类还可以应用在哪些 NLP 任务中？

+   除了 BGE-M3，你还了解哪些优秀的文本嵌入模型？它们各自的优缺点是什么？

+   你认为在 RAG 系统中，还有哪些方法可以提升检索结果的多样性？

> 名言：

> 与其临渊羡鱼，不如退而结网。

> ——筱可

> 欢迎在留言区分享你的想法，每条留言我都会认真阅读！

> 你的反馈是我创作的最大动力 ❤️

> **💗 立即行动**

1.  点赞、收藏、关注，分享给朋友。

1.  为账号加星标，获取更多内容。

1.  以本文为灵感，整理一份属于自己的笔记。

> **🚗 行动召唤

> **"与其等着 AI 改变世界，不如自己参与变革！在这里，让 AI 成为你弯道超车的秘密武器。"

## 📚 往期精选

+   RAG：ColBERT原理、延迟交互机制与稠密向量的对比分析

+   RAG背后的数学：向量点积与余弦的秘密

+   RAG系统召回率低？ BGE-M3 多语言多功能重塑文本检索

+   RAG去重小助手SimHash算法：轻松解决文本相似度检测与查重难题

+   从模式匹配到语义理解：文本嵌入技术的演变与 RAG 系统优化