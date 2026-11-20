
# 07-08 聚类算法-模糊 C 均值

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/JBNjdcVcBoTTppxS5hhcVHW8nwe](https://qqs7y1hozd1.feishu.cn/docx/JBNjdcVcBoTTppxS5hhcVHW8nwe)

## 什么是模糊 C 均值（Fuzzy C-Means）？

我们一般做聚类时，像 K 均值（K-Means）这样的算法，是硬分类：

一个东西只能属于一类，比如这个水果是 100% 是苹果，不可能又是苹果又是香蕉。

而模糊 C 均值（FCM）是软分类：它允许一个东西同时属于多类，只是“程度”不同。

比如：

+ 一个小青苹果，有 70% 的可能性是“苹果类”，有 30% 是“梨子类”。

+ 它不是非黑即白的判断，而是“模糊”的归类。

### FCM 的工作流程（大致流程）

1. 设定要分几类（比如分 3 类）

1. 随机给每个样本一个“属于每一类的程度”（这叫“隶属度”）

1. 计算每一类的中心点（叫“类中心”）

1. 根据每个样本到这些中心点的距离，更新它的“隶属度”

1. 重复第 3 和第 4 步，直到中心点不怎么变化为止

换句话说，就是：

> 猜一下归属 → 算一下中心 → 看离中心多远 → 再重新猜 → 重复直到稳了。

模糊 C 均值就是：一种允许“一个东西部分属于多个类别”的聚类方法，用数学方法算出每个点属于每类的“可能性”。

## 原理详解

### 基本符号与目标函数

+ 有  个样本点：

+ 要划分成  个簇（类），簇心（中心）标为

+ 隶属度矩阵  ，其中

+ “模糊参数”（或“加权指数”） ，控制隶属度的“模糊程度”。

#### 目标函数

FCM 通过最小化下式来找最优簇心和隶属度：

+  是样本  与第  个簇心的欧氏距离平方。

+  对距离加权，  越大，隶属度越平均，聚类结果越模糊。

同时需满足每行隶属度之和为 1：

### 公式推导

为了同时优化  和  ，引入拉格朗日乘子  ，构造带约束的拉氏函数：

#### 簇心求偏导并令其为 0

**簇心更新公式**：

#### 隶属度求偏导并令其为 0

对  中  项：

因为  与  无关，对两个簇  比值可得：

再利用约束  ，可推出：

**隶属度更新公式：**

### 算法流程

1. **初始化**

+ 任选  个初始簇心  ，或随机初始化隶属度矩阵  （并归一化每行之和为 1）。

1. **迭代更新**（直到收敛）

1. **更新簇心**

1. **更新隶属度**

1. **检查收敛**

+ 若  ，或  ，则停止；

+ 否则回到 2.1。

1. **输出**

+ 最终簇心

+ 隶属度矩阵  ，可用于软分配或硬划分（取最大隶属度的簇）。

### 参数与注意事项

+ **模糊指数** ：常取  。

时趋近“硬聚类”（K-Means）。

越大，簇边界越模糊。

+ **收敛速度**：依赖于  、初始值和阈值  。

+ **距离度量**：通常用欧式距，但也可替换成其它度量。

+ **局部极小**：算法可能陷入局部最优，多次随机初始化可缓解。

总结一下，三个点，目标、核心公式和流程：

1. **目标**：在隶属度和簇心间不断迭代，最小化加权平方距离和。

1. **核心公式**：

1. **流程**：初始化 → 更新簇心 → 更新隶属度 → 收敛检测 → 输出。

## 完整案例

在许多实际场景中，我们并不总能明确地将数据划分为互斥的类别。

例如，在市场营销中，一位顾客可能同时对多类商品有兴趣；在医学图像分割中，一个像素点也可能介于不同组织边界之间。硬聚类（如 K-Means）强制每个样本“非此即彼”，而模糊 C 均值（Fuzzy C-Means, FCM）则允许“软”归属：每个样本对各簇有一组隶属度，表示它“多大程度上属于”每个簇。

这里，咱们采用经典的“红酒（Wine）”数据集来演示 FCM 在多维特征上的应用，并通过 PCA 将结果可视化到二维空间，分析每个样本的隶属度分布以及目标函数收敛过程，最后讨论算法优化策略。

### 数据集

“红酒”数据集包含 178 个样本、13 个化学成分特征，以及 3 种不同品种的红酒标签。为了模拟无监督情境，我们不使用真实标签，只用作结果对比。

#### 标准化

不同化学指标量纲差异较大，需对特征矩阵  做 Z-Score 标准化：

#### PCA 降维

为了在二维平面中直观展示聚类效果，对标准化后的数据进行 PCA（主成分分析），保留前 2 个主成分，得到  。

```
import numpy as np
from sklearn.datasets import load_wine
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

# 1. Fuzzy C-Means 实现
def fuzzy_c_means(X, c, m=2.0, max_iter=100, error=1e-5):
    """
    X: 数据矩阵 (N_samples, N_features)
    c: 聚类数
    m: 模糊指数 (fuzzifier)
    max_iter: 最大迭代次数
    error: 收敛阈值
    返回: centers 簇心, U 隶属度矩阵, J_history 目标函数值历史
    """
    N = X.shape[0]
    # 随机初始化隶属度矩阵 U
    U = np.random.rand(N, c)
    U = U / np.sum(U, axis=1, keepdims=True)
    
    J_history = []
    for iteration in range(max_iter):
        U_old = U.copy()
        
        # 1) 更新簇心 c_j
        um = U ** m
        centers = (um.T @ X) / np.sum(um.T, axis=1, keepdims=True)
        
        # 2) 计算距离矩阵 D[i,j] = ||x_i - c_j||
        D = np.linalg.norm(X[:, np.newaxis] - centers, axis=2)
        D = np.fmax(D, 1e-12)  # 防止除零
        
        # 3) 更新隶属度矩阵 U
        exponent = 2.0 / (m - 1)
        U = 1.0 / np.sum((D[:, :, np.newaxis] / D[:, np.newaxis, :]) ** exponent, axis=2)
        
        # 4) 计算目标函数值 J_m
        J = np.sum((U ** m) * (D ** 2))
        J_history.append(J)
        
        # 收敛检测
        if np.linalg.norm(U - U_old) < error:
            break
    
    return centers, U, J_history

# 2. 加载并预处理数据
data = load_wine()
X_raw = data.data
y_true = data.target  # 真实标签，仅用于后续对比

# 标准化
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X_raw)

# PCA降维到2D，便于可视化
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)

# 3. 运行 FCM
n_clusters = 3
m = 2.0
centers, U, J_history = fuzzy_c_means(X_pca, n_clusters, m=m, max_iter=200, error=1e-6)
labels = np.argmax(U, axis=1)

# 4. 可视化结果
colors_bright = ['red', 'lime', 'cyan']

# 4.1 硬划分散点图
plt.figure(figsize=(8, 6))
for j in range(n_clusters):
    idx = labels == j
    plt.scatter(X_pca[idx, 0], X_pca[idx, 1],
                s=50, c=colors_bright[j], label=f'Cluster {j+1}')
plt.scatter(centers[:, 0], centers[:, 1], s=200, c='yellow', marker='*', label='Centers')
plt.title('FCM Clustering on Wine Dataset')
plt.xlabel('PC1')
plt.ylabel('PC2')
plt.legend()
plt.show()

# 4.2 隶属度随样本索引曲线
plt.figure(figsize=(10, 4))
for j in range(n_clusters):
    plt.plot(U[:, j], label=f'Membership C{j+1}', linewidth=1.5)
plt.title('Membership Degrees for Each Cluster')
plt.xlabel('Sample Index')
plt.ylabel('Membership Value')
plt.legend()
plt.show()

# 4.3 目标函数收敛曲线
plt.figure(figsize=(6, 4))
plt.plot(J_history, marker='o', linewidth=2)
plt.title('Objective Function Convergence')
plt.xlabel('Iteration')
plt.ylabel('J_m Value')
plt.tight_layout()
plt.show()
```

### 1. 硬划分散点图（Cluster Assignments）

+ 三个簇中心用黄色星号标出，其余点用红、绿、青分别表示 Cluster 1~3。

+ 由于使用 PCA 展示二维空间，实际在高维时簇形状可能更复杂，但从图中可以看出三类基本分离。

+ 某些点位于簇边界处，后续将在“隶属度”曲线上体现其“模糊性”。

![](img/9457b31fcaf0c1e9b08dad95c17768d0.png)

### 2. 隶属度曲线（Membership Degrees）

+ 横轴为样本索引（0~177），纵轴为隶属度值。

+ 样本对不同簇的隶属度并不全为 0/1，边界样本出现多条曲线交叉，代表它们既不完全属于某一簇，也未完全排除于他簇。

![](img/fbb0684486f7402e1fe79354c1c5d3ed.png)

### 3. 目标函数收敛

+ 初始时目标值较高，经过约 5 次迭代迅速下降，并在第 10 次左右达到平稳，表明算法收敛较快。

+ 对比不同模糊指数  的收敛曲线，可发现  越接近 1（更“硬”），收敛速度更快；  越大（更“模糊”），对隶属度的平滑处理使收敛稍慢，但能获得更稳健的簇划分。

![](img/0e386fb3383bd0d2e2eedf89eba95ba4.png)

### 算法优化策略

1. **多次初始化与最优选择**：FCM 对初始隶属度敏感，易陷入局部最优。可多次随机初始化  ，记录每次最终的  ，选择最小  对应的划分。

1. **加速向量化**：本实现已充分利用 `NumPy` 广播加速距离计算与隶属度更新，如对大规模数据仍可借助 GPU 库（如 `CuPy`）进一步提速。

1. **动态调整模糊指数**：让  随迭代动态变化，从较小值（逼近 K-Means）过渡到较大值（平滑划分），兼顾速度与鲁棒性。

1. **剪枝策略**：对于距离非常远的点可提前赋低隶属度，减少内循环计算量。

1. **并行化**：在多核 CPU 上并行计算距离矩阵  和隶属度矩阵更新，提升性能。

FCM 在处理边界模糊、不确定性高的数据时，比硬聚类更具优势。

这里的案例咱们结合 PCA，将 Wine 数据集映射到二维，完整演示了 FCM 的实现、可视化及深入分析。

通过多次初始化、向量化、动态  调整等优化策略，可进一步提升算法性能和稳定性。

真实项目中，可将此框架扩展到更高维度、多模态数据，或结合监督信号做半监督聚类。

## 模型分析

### 模糊 C 均值（FCM）优缺点

#### 优点

1. **软归属**：每个样本对各簇都有一个隶属度（0–1），能准确反映边界样本的模糊性，而非“非此即彼”。

1. **灵活性高**：通过调整模糊指数  可以控制聚类结果的“软硬”程度，适应不同应用场景。

1. **目标函数收敛**：借助明确的优化目标  并用交替最优化，通常能在较少迭代内收敛。

1. **解释性好**：隶属度矩阵  本身就可以作为后续分析（如权重加权、模糊决策）的输入，无需额外转换。

#### 缺点

1. **需预设簇数** ：与 K-Means 类似，必须事先指定类别数，若选错会严重影响结果。

1. **对初值敏感**：随机初始化隶属度或簇心可能导致陷入不同的局部极小，需多次重启取最优。

1. **计算量较大**：每次迭代都要计算完整的距离矩阵和隶属度更新，数据量大时开销明显高于硬聚类。

1. **对异常值不够鲁棒**：和 K-Means 一样，欧氏距离度量会被离群点拖偏，需额外策略（如预先去噪）才能缓解。

### 与相似算法的对比

| 算法 | 隶属／归属方式 | 模型假设 | 计算复杂度 | 对噪声／离群值鲁棒性 | 典型适用场景 |

| --- | --- | --- | --- | --- | --- |

| **FCM** | 软（隶属度） | 质心＋模糊隶属，欧氏距离 |  | 中等 | 模糊边界、高不确定性数据；图像分割等 |

| **K-Means** | 硬（唯一归属） | 各簇等方差高斯、最小平方误差 |  | 较差 | 大规模快速聚类；对划分硬性要求高的数据 |

| **GMM** | 软（概率隶属） | 混合高斯分布，含协方差矩阵 |  | 较好 | 需要概率解释或估计数据分布的场景 |

+ **隶属／归属方式**：FCM、GMM 都是“软”聚类，样本可同时部分属于多类；K-Means 强制硬分类。

+ **模型假设**：K-Means 隐含簇内方差相同，GMM 则允许每簇有自己协方差，FCM 只关注距离加权，无概率解释。

+ **计算复杂度**：GMM 因需要求逆协方差矩阵，复杂度最高；FCM 和 K-Means 相近，但 FCM 的隶属度更新略复杂。

+ **鲁棒性**：GMM 可通过协方差反映簇大小和方向，对噪声略好；FCM 和 K-Means 都对离群点敏感。

### 何时优选 FCM，何时考虑其它算法

#### 优选 FCM 的场景

+ **边界模糊**：样本确实存在交叠区域，需要「程度」而非「标签」。

+ **软分类需求**：后续需利用隶属度进行加权预测、模糊决策或组合分析。

+ **中小规模数据**：数据量不至于让隶属度迭代开销过大；同时需要比硬聚类更丰富的信息。

#### 可考虑其它算法的场景

1. **大规模、实时性高**：选择 K-Means 或 Mini-Batch K-Means，计算更快、内存更少。

1. **需要概率建模或不等方差簇**：用 GMM 可获得更灵活的簇形状及样本后验概率。

1. **聚类数未知或形状任意**：层次聚类（无需预设簇数）或 DBSCAN（可发现任意形状、自动识别噪声），更灵活。

1. **高维稀疏数据**：可尝试谱聚类（Spectral Clustering）或基于子空间聚类的方法。

总之，FCM 最适合**需要软分配**、**簇数已知**且**规模适中**的场景；若对速度、可扩展性、概率解释或形状灵活性有更高要求，则应优先考虑 K-Means、GMM、DBSCAN、谱聚类等替代方案。
