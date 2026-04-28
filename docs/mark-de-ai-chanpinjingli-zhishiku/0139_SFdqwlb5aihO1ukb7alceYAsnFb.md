# 02-07分类算法-高斯混合模型

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/MO04dkoZhoJ53px3Wghcs4jRnhe](https://qqs7y1hozd1.feishu.cn/docx/MO04dkoZhoJ53px3Wghcs4jRnhe)

高斯混合模型是一种用于进行数据聚类和概率密度估计的统计模型。

咱们用一个简单的例子让大家先有一个清晰的概念：

**当你**有一堆数据点的时候，这些点可能是一些人的身高、体重或者其他特征。你希望找到这些数据点的潜在模式或类别，比如说你想知道这堆数据可以分成几组。高斯混合模型就是帮助你做这件事的一种方法。

**高斯分布**

在介绍高斯混合模型之前，先来说说高斯分布，也叫正态分布。高斯分布是一个钟形曲线，比如说我们常说的“身高的正态分布”。在这个分布中，大部分人的身高集中在平均值附近，越接近平均值的人越多，越偏离平均值的人越少。

**混合模型**

现在，假设你有一群人的身高数据，但这群人其实由不同的子群体组成，比如男性和女性。男性和女性的身高各自遵循不同的高斯分布，但你不知道哪些数据点属于男性，哪些属于女性。高斯混合模型就是用来处理这种情况的，它认为你的数据是由多个高斯分布“混合”而成的。

**高斯混合模型的组成**

1.  **多个高斯分布**：每个子群体对应一个高斯分布，这些高斯分布有各自的平均值和标准差。

1.  **权重**：每个高斯分布还有一个权重，表示这个分布在整体数据中占的比例。

**高斯混合模型怎么工作？**

1.  **初始化**：假设有几个高斯分布，开始时我们不知道这些分布的参数（平均值、标准差和权重），所以随机初始化这些参数。

1.  **期望步骤（E步）**：根据当前的参数，计算每个数据点属于每个高斯分布的概率。换句话说，估计每个数据点是男性还是女性的可能性。

1.  **最大化步骤（M步）**：利用E步的结果，重新计算高斯分布的参数，使得这些分布能更好地解释数据。

1.  **重复**：不断重复E步和M步，直到模型参数收敛，也就是说变化很小，可以认为模型已经找到了最佳的参数。

**高斯混合模型的用途**

+   **聚类**：可以用于将数据分成不同的组，比如市场上的不同客户群体。

+   **概率密度估计**：可以用来估计数据的分布情况，帮助我们理解数据的结构。

总结来说，高斯混合模型是一种假设你的数据由多个高斯分布混合而成的统计模型。通过不断调整这些分布的参数，它可以帮助你发现数据中的潜在模式或类别。对于初学者来说，可以把它看作是一种高级的、自动化的“分组”工具。

## 理论基础

#### 1. 高斯混合模型的定义

GMM 假设数据集   是由   个高斯分布的加权组合生成的。每个数据点   属于   维向量。

高斯混合模型的概率密度函数   表示为：

其中：

是第   个高斯分布的权重，满足   且  。

是第   个高斯分布的概率密度函数。

高斯分布的概率密度函数定义为：

#### 2. 对数似然函数

GMM 的目标是最大化数据集的对数似然函数。

对数似然函数   定义为：

这里，  是需要估计的参数集合。

#### 3. EM算法推导

EM算法由期望步骤（E步）和最大化步骤（M步）组成。我们首先初始化参数，然后在每次迭代中执行E步和M步，直到收敛。

##### 期望步骤（E步）

在E步中，我们计算每个数据点   属于第   个高斯分布的后验概率，即“责任”  ：

其中：

是数据点   的隐变量，表示数据点属于哪个高斯分布。

是数据点   属于第   个高斯分布的概率。

##### 最大化步骤（M步）

在M步中，我们利用E步计算得到的后验概率   来重新估计模型参数，使对数似然函数最大化。

**1. 更新权重 ** ：

其中，  是第   个高斯分布的有效样本数：

**2. 更新均值 ** ：

**3. 更新协方差矩阵 ** ：

### EM算法流程详细步骤

**1. 初始化**：

+   随机初始化参数  。

+   初始化时，可以使用K-means算法得到初始的均值  ，然后计算初始的协方差矩阵   和权重  。

**2. 迭代**：

**期望步骤（E步）**：

+   对每个数据点   和每个高斯分布  ，计算后验概率  ：

**最大化步骤（M步）**：

+   更新权重  ：

+   更新均值  ：

+   更新协方差矩阵  ：

**3. 收敛判断**：

+   计算对数似然函数   的值：

+   如果对数似然函数的增量小于预设阈值，则认为收敛，停止迭代；否则，返回第2步继续迭代。

高斯混合模型通过假设数据由多个高斯分布混合而成，使用EM算法迭代优化参数，找到最佳的高斯分布组合来解释数据。其核心在于不断计算数据点属于每个高斯分布的概率（E步）并更新模型参数（M步），直到模型收敛。这一过程通过最大化数据的对数似然函数来实现最佳参数估计。

## Python案例

我们使用make_blobs函数来生成一个包含多个簇的虚拟数据集，这些簇具有不同的均值和协方差。

接着，我们将应用GMM对生成的数据进行聚类分析。

```
import numpy as np
import pandas as pd
from sklearn.mixture import GaussianMixture
from sklearn.datasets import make_blobs
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

# Seaborn风格
sns.set(style=&quot;white&quot;, palette=&quot;muted&quot;, color_codes=True)

# 生成虚拟数据集
n_samples = 5000
n_features = 4
n_clusters = 4

# 使用make_blobs生成数据
X, y = make_blobs(n_samples=n_samples, n_features=n_features, centers=n_clusters, cluster_std=0.60, random_state=42)

# 数据标准化
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# 将数据转换为DataFrame
df = pd.DataFrame(X_scaled, columns=[f&#x27;Feature_{i+1}&#x27; for i in range(n_features)])
df[&#x27;true_label&#x27;] = y

# 使用GMM进行聚类
gmm = GaussianMixture(n_components=n_clusters, covariance_type=&#x27;full&#x27;, random_state=42)
gmm.fit(X_scaled)
labels = gmm.predict(X_scaled)

# 将聚类结果添加到DataFrame中
df[&#x27;GMM_cluster&#x27;] = labels

# PCA降维到2D
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)
df[&#x27;PCA1&#x27;] = X_pca[:, 0]
df[&#x27;PCA2&#x27;] = X_pca[:, 1]

# 可视化聚类结果
plt.figure(figsize=(12, 6))

# 原始标签
plt.subplot(1, 2, 1)
sns.scatterplot(x=&#x27;PCA1&#x27;, y=&#x27;PCA2&#x27;, hue=&#x27;true_label&#x27;, data=df, palette=&#x27;viridis&#x27;, legend=&#x27;full&#x27;)
plt.title(&#x27;True Labels&#x27;)

# GMM聚类结果
plt.subplot(1, 2, 2)
sns.scatterplot(x=&#x27;PCA1&#x27;, y=&#x27;PCA2&#x27;, hue=&#x27;GMM_cluster&#x27;, data=df, palette=&#x27;viridis&#x27;, legend=&#x27;full&#x27;)
plt.title(&#x27;GMM Clustering Result&#x27;)

plt.show()

# 评估不同组件数量的AIC和BIC
n_components_range = range(1, 10)
aic = []
bic = []

for n in n_components_range:
    gmm = GaussianMixture(n_components=n, covariance_type=&#x27;full&#x27;, random_state=42)
    gmm.fit(X_scaled)
    aic.append(gmm.aic(X_scaled))
    bic.append(gmm.bic(X_scaled))

# 可视化AIC和BIC
plt.figure(figsize=(12, 6))
plt.plot(n_components_range, aic, label=&#x27;AIC&#x27;, marker=&#x27;o&#x27;)
plt.plot(n_components_range, bic, label=&#x27;BIC&#x27;, marker=&#x27;o&#x27;)
plt.xlabel(&#x27;Number of Components&#x27;)
plt.ylabel(&#x27;AIC/BIC&#x27;)
plt.legend()
plt.title(&#x27;Model Selection using AIC and BIC&#x27;)
plt.show()

# 选择最佳组件数量
best_n_components = n_components_range[np.argmin(bic)]
print(f&#x27;Best number of components: {best_n_components}&#x27;)

# 重新进行GMM聚类
gmm = GaussianMixture(n_components=best_n_components, covariance_type=&#x27;full&#x27;, random_state=42)
gmm.fit(X_scaled)
labels_optimized = gmm.predict(X_scaled)

# 更新DataFrame中的聚类结果
df[&#x27;GMM_cluster_optimized&#x27;] = labels_optimized

# 可视化优化后的聚类结果
plt.figure(figsize=(12, 6))
sns.scatterplot(x=&#x27;PCA1&#x27;, y=&#x27;PCA2&#x27;, hue=&#x27;GMM_cluster_optimized&#x27;, data=df, palette=&#x27;viridis&#x27;, legend=&#x27;full&#x27;)
plt.title(&#x27;Optimized GMM Clustering Result&#x27;)
plt.show()
```

![](img/c536e5fe4c53697fa5678e892b157eeb.png)

1.  **生成虚拟数据集**：使用make_blobs函数生成一个虚拟数据集，包含n_samples个样本，n_features个特征，和n_clusters个簇。

1.  **标准化数据**：使用StandardScaler对数据进行标准化处理，使每个特征具有零均值和单位方差。

1.  **应用GMM**：使用GaussianMixture对数据进行聚类分析，并将结果添加到DataFrame中。

1.  **PCA降维**：使用PCA将数据降维到二维，以便进行可视化。

1.  **可视化**：绘制原始标签和GMM聚类结果的散点图，以观察模型的效果。

1.  **模型优化**：评估不同组件数量的AIC和BIC，选择最佳的组件数量，并重新进行GMM聚类，绘制优化后的结果。

![](img/3e2c31011f8f60a4ef33bdacde93c865.png)

![](img/0d96b89c4542e9ecb9db870ccd73ce24.png)

通过虚拟数据集，我们给大家演示了高斯混合模型（GMM）的整体应用过程，包括数据生成、模型训练、结果可视化和模型优化。

## 模型分析

### 优缺点

**优点**：

1.  **灵活性**：GMM能够处理形状各异的簇，因为每个簇是一个高斯分布，具有各自的均值和协方差矩阵。因此，它能够捕捉到数据中的复杂分布。

1.  **软聚类**：GMM是一个软聚类方法，意味着每个数据点对所有簇都有一定的隶属度（通过责任值   表示），这比硬聚类方法（如K-means）更具信息量。

1.  **适用性**：可以处理不同形状和不同大小的簇，特别适用于高维数据集。与K-means相比，GMM对于簇的形状更不那么敏感。

1.  **概率建模**：提供了对数据分布的概率解释，使得模型更具解释性，并且可以通过计算概率密度来进行样本生成和异常检测。

**缺点**：

1.  **对初始值敏感**：GMM的性能受到初始参数选择的影响较大。需要多次运行算法以获得较好的结果，或者使用更复杂的初始化方法。

1.  **计算复杂度**：GMM的计算复杂度相对较高，特别是在数据维度较高时。EM算法的迭代可能较慢，并且可能陷入局部最优解。

1.  **模型选择困难**：选择适当的组件数量   是一个挑战。虽然可以使用AIC或BIC进行选择，但这些指标在不同的数据集上可能表现不同。

1.  **高维数据问题**：在高维数据中，协方差矩阵可能会变得不稳定（尤其是当样本数不足时），从而影响模型的性能。

### 与相似算法对比

| **对比项** | **K-means聚类** | **DBSCAN** | **层次聚类** |
| --- | --- | --- | --- |
| **模型假设** | 假设簇是球形的且大小相似，硬聚类方法，每个数据点只能属于一个簇。 | 基于密度的聚类方法，不需要预设簇数，能够发现形状不规则的簇并处理噪声。 | 通过构建树状结构（层次图）来进行聚类，不需要预设簇数。 |
| **优点** | - 计算简单且速度较快，特别适合大数据集。  - 适用于球形簇，易于理解与实现。 | - 能检测不同形状的簇。  - 对噪声数据有较好的处理能力。 | - 不需要预设簇数，自动生成层次结构，适合揭示数据层次关系。  - 可以提供更多聚类信息。 |
| **缺点** | - 不适合簇形状不规则的数据。  - 对初始簇心敏感。 | - 需要调整参数（邻域大小和最小点数）。  - 对于不同密度的簇，效果较差。 | - 计算复杂度高，特别是大数据集。  - 结果对距离度量和合并准则敏感。 |
| **适用场景** | - 簇接近球形，簇的大小差异不大时。  - 数据量较大，计算速度重要时。 | - 数据中存在噪声和离群点，簇形状不规则时。  - 密度变化大的数据集。 | - 簇数量未知时，适用于较小数据集，计算复杂度可接受时。  - 想要层次化聚类结果时。 |
| **聚类形状** | 适用于球形或规则的聚类。 | 适用于任意形状和密度的聚类。 | 适用于层次结构的数据，能够揭示数据的多层级聚类关系。 |
| **计算复杂度** | 低，适合大规模数据集。 | 中等，处理密度变化大的数据集时可能较慢。 | 高，尤其在处理大数据集时效率低。 |
| **噪声与异常值处理** | 对离群点敏感，无法有效处理噪声。 | 能有效处理噪声数据，能够识别并排除异常值。 | 对噪声点的处理较弱。 |

### 选择GMM的场景

+   **簇形状和大小不一致**：当数据簇的形状不规则或者簇的大小差别较大时，GMM通常比K-means更有效。

+   **需要软聚类**：当需要每个数据点对每个簇的隶属度（而不仅仅是硬分类）时，GMM提供了更丰富的信息。

+   **概率建模**：当需要对数据分布进行概率建模，或者需要生成样本，GMM是一个好的选择。

### 选择其他算法的场景

+   **簇形状接近球形且大小相似**：在这种情况下，K-means可能更简单且更高效。

+   **数据中有噪声和离群点**：DBSCAN能够有效处理这类问题。

+   **需要层次结构信息**：当需要获取数据簇的层次关系时，层次聚类更为合适。

## 最后

GMM因其灵活性和概率建模能力，适合于处理复杂的数据分布，但在实际应用中需要权衡其计算复杂度和初始化敏感性。