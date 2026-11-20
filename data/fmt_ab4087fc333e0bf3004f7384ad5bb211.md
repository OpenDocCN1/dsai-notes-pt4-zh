
# 01-01回归算法-K近邻回归

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/YwKKdrS1VoRE0NxCIJicq6G2nGb](https://qqs7y1hozd1.feishu.cn/docx/YwKKdrS1VoRE0NxCIJicq6G2nGb)

K近邻回归（K-Nearest Neighbors Regression, KNN回归），简单又直观的机器学习方法，它用来预测数值型的数据（比如房价、气温等）。

基本原理：

1. 找邻居：当我们想预测一个数据点的数值时，先在数据集中找到离它最近的 `K` 个点（邻居）。

1. 求平均值：然后，把这 `K` 个邻居的数值（目标值）取平均，作为这个点的预测值。

举个例子：假设你想预测一个新房子的房价，而你手上有一堆已经卖出去的房子数据（包括它们的价格和地理位置）。K近邻回归的做法是：

+   找到 `K` 个离这个新房子最近的已售房子；

+   取这 `K` 个房子的平均价格，作为新房子的预测价格。

K值的影响：

+   `K`太小（比如`K=1`）：预测结果会受到单个邻居的影响，可能不稳定。

+   `K`太大（比如`K=100`）：虽然更稳定，但可能会包含太多远离目标的数据点，导致预测不够精准。

+   一般`K`值的选择：需要通过实验（比如交叉验证）来找到一个合适的`K`。

## 原理详解

KNN 回归不依赖于训练数据的显式建模，而是通过找到新样本在训练数据中的 `K` 个最近邻，然后利用这些邻居的目标值来进行预测。

KNN回归的本质是局部加权平均法，是一种非参数估计方法。

### 1. 基本思想

给定一个训练数据集：

其中：

是输入特征向量

是对应的目标值（输出）

是训练数据集的大小

对于一个新的测试样本，KNN回归的基本思想是：

1.  在训练数据集中找到离它最近的 `K` 个邻居（按照某种距离度量）。

1.  根据这些邻居的目标值计算它的预测值。

### 2. 距离度量

通常，KNN回归采用欧氏距离来度量样本之间的相似性：

也可以使用其他距离度量，如：

+   曼哈顿距离：

+   闵可夫斯基距离：

当  时即为欧氏距离，当  时即为曼哈顿距离。

### 3. 预测值计算

找到最近的 `K` 个邻居后，KNN 回归的预测值可以通过加权平均的方法计算，常见的方法包括：

#### 1) 直接均值

其中， 是 `K` 个最近邻的目标值。

#### 2) 距离加权均值

为了让更接近它的样本具有更高的影响力，可以采用加权平均：

权重  通常与距离的倒数成正比，例如：

其中  是一个小的正数，防止分母为零。

#### 3) 高斯核加权

可以使用核函数（如高斯核）来赋予权重：

这样，离测试点  越近的点权重越大，影响力越强。

## 公式推理

### 1. KNN 回归的期望误差分析

KNN回归属于非参数回归方法，其误差可以从偏差-方差分解（Bias-Variance Decomposition）的角度分析。假设数据服从分布：

其中  是真实函数， 是噪声。

KNN 回归的预测值：

对期望值取均值：

假设 `K` 个最近邻均匀分布在  附近：

因此，KNN回归的偏差为：

方差计算：

总误差：

因此，当 `K` 过小时，方差较大（易过拟合）；当 `K` 过大时，偏差较大（易欠拟合）。

## 算法流程

输入：

+   训练数据集

+   需要预测的样本

+   选择邻居数

+   选择距离度量方式（如欧氏距离）

+   选择预测策略（均值、加权均值等）

步骤：

1.  计算距离：计算测试样本  与训练集中所有样本的距离：

1.  选择最近邻：选取距离最近的 `K` 个样本。

1.  计算预测值：

+   若采用均值方法：

+   若采用加权方法：

1.  输出预测结果。

## 完整案例

在这个案例中，我们先加载数据，然后用 KNN 进行回归预测，并利用 `GridSearchCV` 优化超参数（邻居数 `k`），最后通过多种可视化展示模型效果、预测值与真实值的对比、残差分布以及超参数调优过程的热力图。

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import fetch_california_housing
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.neighbors import KNeighborsRegressor
from sklearn.metrics import mean_squared_error, r2_score


# 加载 California 房价数据集
housing = fetch_california_housing(as_frame=True)
data = housing.frame
# 打印数据基本信息
print("数据集维度：", data.shape)
print("数据集特征：\n", data.columns)

# 选择特征与目标变量
X = data.drop("MedHouseVal", axis=1)  # 房屋中位数作为目标变量
y = data["MedHouseVal"]

# 将数据集拆分为训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 初始化 KNN 回归器
knn = KNeighborsRegressor()

# 设置超参数搜索空间：邻居数从 1 到 30
param_grid = {'n_neighbors': np.arange(1, 31)}

# 使用 GridSearchCV 进行超参数优化，采用 5 折交叉验证
grid_search = GridSearchCV(knn, param_grid, cv=5, scoring='neg_mean_squared_error', n_jobs=-1)
grid_search.fit(X_train, y_train)

# 输出最优参数与对应的得分
best_k = grid_search.best_params_['n_neighbors']
best_score = -grid_search.best_score_
print("最优邻居数 k =", best_k)
print("最佳均方误差 (MSE) =", best_score)

# 使用最优参数训练模型
best_knn = grid_search.best_estimator_
best_knn.fit(X_train, y_train)

# 在测试集上进行预测
y_pred = best_knn.predict(X_test)

# 计算测试集上的 MSE 和 R2 分数
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
print("测试集均方误差 (MSE) =", mse)
print("测试集 R2 score =", r2)

# 可视化：真实值 vs 预测值
plt.figure(figsize=(10, 6))
plt.scatter(y_test, y_pred, alpha=0.6, color='mediumspringgreen', edgecolor='k')
plt.plot([y_test.min(), y_test.max()], [y_test.min(), y_test.max()], 'r--', lw=2)
plt.xlabel("True Values", fontsize=14)
plt.ylabel("Predicted Values", fontsize=14)
plt.title("True vs Predicted Values", fontsize=16)
plt.tight_layout()
plt.show()

# 可视化：残差分布
residuals = y_test - y_pred
plt.figure(figsize=(10, 6))
sns.histplot(residuals, bins=50, kde=True, color='hotpink')
plt.xlabel("Residuals", fontsize=14)
plt.ylabel("Frequency", fontsize=14)
plt.title("Residual Distribution", fontsize=16)
plt.tight_layout()
plt.show()

# 可视化：GridSearchCV 中不同 k 值的均方误差热力图
# 提取所有搜索结果
results = pd.DataFrame(grid_search.cv_results_)
results['mean_test_mse'] = -results['mean_test_score']

plt.figure(figsize=(12, 6))
plt.plot(results['param_n_neighbors'].data, results['mean_test_mse'], marker='o', linestyle='-', color='dodgerblue')
plt.xlabel("Number of Neighbors (k)", fontsize=14)
plt.ylabel("Mean Squared Error (MSE)", fontsize=14)
plt.title("Grid Search MSE vs k", fontsize=16)
plt.xticks(results['param_n_neighbors'].data)
plt.grid(True)
plt.tight_layout()
plt.show()
```

+   数据加载与预处理：从 `fetch_california_housing` 加载数据，并将数据拆分为训练集与测试集，确保模型评估的客观性。

+   模型训练与超参数优化：使用 `GridSearchCV` 对 KNN 回归器的邻居数进行调优，选取使均方误差最低的参数。

+   模型评估：利用 MSE 与 R2 分数对模型进行评价，并通过散点图对比真实值与预测值，以及绘制残差分布图。

+   超参数调优可视化：通过折线图展示不同邻居数对应的均方误差，直观展示模型的参数选择过程。

![](img/fc1affe5d88d1dd0fae9da8bd57deea7.png)

算法优化说明：

1.  采用 `GridSearchCV` 自动搜索最优的 `k` 值，并利用 5 折交叉验证提高模型泛化能力。

1.  对数据进行训练集和测试集的划分，确保模型在未知数据上的评估准确性。

1.  利用可视化手段直观展示模型效果及超参数选择结果，为进一步调参提供依据。

## 模型分析

### KNN回归 优缺点

优点：

1.  简单易懂，直观性强：KNN 只需要计算距离，不涉及复杂的数学建模，非常容易理解和实现。

1.  非参数方法：KNN 不需要对数据分布做假设，适用于各种数据类型，尤其是非线性数据。

1.  适用于小数据集：在小规模数据上表现较好，不需要大量训练时间，适用于数据量较少但维度较高的场景。

1.  适用于局部模式学习：对于局部模式明显的数据，KNN 可以很好地拟合，例如地理位置相关的数据预测。

缺点：

1.  计算量大，预测速度慢：KNN 需要存储所有训练数据，并在预测时计算每个样本到所有训练样本的距离，计算开销较大，特别是在大数据集上表现较差。

1.  受维度灾难影响：高维数据会导致欧几里得距离的效果变差，样本点之间的距离趋于相等，使得 KNN 变得无效。

1.  对噪声敏感：KNN 依赖于距离度量，如果数据中存在异常值或噪声点，可能会显著影响预测结果。

1.  无法输出概率：不像一些统计学习方法（如贝叶斯回归）能给出置信度，KNN 仅能提供一个具体的预测值，缺乏不确定性度量。

### KNN 回归与其他相似算法的对比

| 算法 | 适用场景 | 优点 | 缺点 |

| --- | --- | --- | --- |

| KNN 回归 | 适用于小数据集、非线性数据、局部模式学习 | 易理解、非参数、适应性强 | 计算量大、对高维数据效果差、对噪声敏感 |

| 线性回归 | 适用于线性关系的数据 | 计算效率高、可解释性强 | 仅适用于线性数据，对非线性数据拟合较差 |

| 决策树回归 | 适用于数据集较大、有明显分段特征的数据 | 能处理非线性数据、不需要标准化 | 易过拟合、对噪声敏感 |

| 随机森林回归 | 适用于大规模数据、有较多特征的数据 | 处理非线性关系能力强、不易过拟合 | 计算复杂度较高 |

| 支持向量回归 (`SVR`) | 适用于小规模、高维数据集 | 适用于高维数据、支持核方法 | 计算复杂度较高、对超参数敏感 |

| 神经网络回归 | 适用于大数据、高度非线性关系的数据 | 适用于复杂任务、强大的表达能力 | 训练时间长、需要大量数据、难以解释 |

### 在什么情况下优选 KNN，什么时候应该考虑其他算法？

适合使用 KNN 的情况：

+   数据集较小，计算量可接受（如 <10,000 条样本）。

+   需要处理非线性数据，且数据分布未知时，KNN 可作为基准模型。

+   数据具有局部模式，例如地理位置预测、推荐系统等。

+   需要快速实现一个基准模型进行对比。

应考虑其他算法的情况：

+   数据量大（> 10 万条）：KNN 计算所有样本距离的过程过慢，推荐使用决策树回归或随机森林。

+   数据是高维（> 50 维）：KNN 受维度灾难影响较大，推荐支持向量回归 (`SVR`) 或 神经网络。

+   数据有线性趋势：如果数据关系接近线性，使用线性回归的计算效率更高。

+   需要解释性和可解释的决策规则：KNN 不易解释，推荐使用决策树回归或线性回归。

+   数据包含较多噪声：KNN 对噪声敏感，推荐使用随机森林回归，因其鲁棒性较强。

### 总结

KNN 回归是一种直观、易于实现的算法，适用于小规模、非线性数据集。但在大数据、高维场景或计算效率要求高的应用中，KNN 可能不是最佳选择，此时可以考虑决策树、随机森林、支持向量机等替代方案。
