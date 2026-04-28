# 01-12回归算法-随机森林回归

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/HAVCdXYY5oKSwbxgc0HcyzSDnAd](https://qqs7y1hozd1.feishu.cn/docx/HAVCdXYY5oKSwbxgc0HcyzSDnAd)

**随**机森林回归（Random Forest Regression）是一种用来做预测的机器学习方法，它本质上是一个由很多棵决策树组成的“森林”。

可以把它想象成一个**「集体投票」的过程**，每棵树都会给出自己的预测结果，最后让这些树的平均值作为最终答案。

比如**你想预测一个房子的价格**，可以找很多个房产专家来估价，每个专家根据自己的经验和数据做出预测，最后我们取所有专家给出的价格的平均值，这样得到的结果通常比单个专家预测的更准确、更稳定。这就是随机森林回归的基本思想——用多个决策树来减少单个模型的误差和偏差，让结果更加可靠！

#### 核心特点

1.  **由很多棵决策树组成**：不是单靠一棵树，而是多个模型一起工作。

1.  **每棵树都是随机训练的**：

+   训练数据是**随机抽样**的（每棵树可能看到的数据都不一样）。

+   **选择特征**（决定如何分裂树的条件）时也是随机挑选的。

1.  最终结果是所有树的预测值的平均值，这样能减少单个模型的过拟合问题，提高泛化能力。

#### 它为什么好用？

+   **稳定**：单棵树容易受到数据的影响，而多个树取平均值后，噪声和异常数据的影响会减小。

+   **防止过拟合**：如果只有一棵决策树，可能会对训练数据过于敏感（即过拟合），但随机森林可以有效缓解这个问题。

+   **适用于复杂关系**：即使数据的关系很复杂，随机森林也能很好地找到模式。

随机森林回归就是用**多个决策树**一起预测一个数值结果，通过**投票+平均值**的方式提高预测的准确性和稳定性，是一种非常好用的机器学习方法~

## 原理详解

随机森林回归通过构建多个回归树并取平均预测值来提升准确性和泛化能力。

### 单棵回归决策树

随机森林是由多棵**回归决策树（Regression Tree）**组成，因此先回顾决策树回归的数学原理。

**回归树的目标**

给定一个训练数据集：

其中， 是输入特征， 是对应的目标变量。

我们希望构建一个决策**树来**将输入特征空间划分成多个区域  ，并在每个区域内用均值作为预测值：

其中：

是决策树分割出来的区域。

是该区域内所有样本  的均值：

是指示函数，表示样本  是否属于区域  。

**决策树的划分标准**

构造决策树时，我们需要在某个特征  上选择一个最佳分割点  ，使得目标函数最优（最小化方差）。

对于一个分割点  ，数据被划分为左右两个区域：

我们希望选择一个  使得目标函数（均方误差，MSE）最小：

其中：

最终，我们使用递归的方法，逐步划分数据，直到满足停止条件（如最小叶子节点数、最大树深度等）。

### 随机森林回归的数学推导

**随机森林的基本思想**

随机森林由  棵独立的回归树  组成，每棵树通过不同的样本和特征训练得到，最终的预测结果是所有树的平均值：

其中：

是树的数量。

是第  棵树对输入  的预测。

**误差分析**

假设每棵回归树的预测值可以表示为：

其中：

是真实的目标函数。

是随机误差，满足均值为 0，方差为  。

随机森林的最终预测值为：

假设误差  是独立同分布的，则随机森林的方差：

当  增大时，方差降低，说明随机森林比单棵决策树更加稳定。

### 随机森林回归的算法流程

造训练集（Bootstrap 采样）

从原始数据集中**随机有放回**地抽取  个样本，构建一个子数据集  （与原数据集大小相同，但可能包含重复样本）。

**建决策树**

对于每个子数据集  ，训练一棵决策树，过程中：

1.  在每个节点随机选择  个特征（ ），然后选择最优特征进行分裂。

1.  递归划分数据，直到达到停止条件。

**复构建 ** **棵树**

重复步骤 1 和 2，共训练  棵不同的回归树。

对于新输入  ，每棵树  进行预测，最终预测值是所有树的**均值**：

随机森林回归通过**集成多棵随机采样的决策树**，减少单个模型的过拟合，提高预测准确性。

它的数学基础主要包括**决策树的方差最小化**和Bootstrap 采样的集成学习，最终通过取均值得到稳定的预测值。

## 完整案例

这里，咱们使用 **加利福尼亚房价数据集** (Cali**fornia** Housing dataset) 进行随机森林回归建模，完整流程如下：

1.  数据加载与预处理

1.  数据可视化（特征分布、相关性分析）

1.  构建随机森林回归模型

1.  超参数优化（GridSearchCV）

1.  模型评估（MSE、R²、残差分析）

1.  结果可视化（实际 vs 预测）

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import fetch_california_housing
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error, r2_score

# 1. 加载加利福尼亚房价数据集
data = fetch_california_housing()
df = pd.DataFrame(data.data, columns=data.feature_names)
df[&quot;Target&quot;] = data.target * 100000  # 转换单位（单位：美元）

# 2. 数据可视化
plt.figure(figsize=(12, 6))
sns.heatmap(df.corr(), annot=True, cmap=&quot;coolwarm&quot;, fmt=&quot;.2f&quot;)
plt.title(&quot;Feature Correlation Heatmap&quot;)
plt.show()

# 3. 数据拆分
X = df.drop(columns=[&quot;Target&quot;])
y = df[&quot;Target&quot;]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 4. 训练随机森林回归模型
rf = RandomForestRegressor(n_estimators=100, random_state=42)
rf.fit(X_train, y_train)
y_pred = rf.predict(X_test)

# 5. 评估模型
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
print(f&quot;Mean Squared Error: {mse:.2f}&quot;)
print(f&quot;R² Score: {r2:.2f}&quot;)

# 6. 结果可视化
plt.figure(figsize=(10, 5))
plt.scatter(y_test, y_pred, alpha=0.7, color=&#x27;b&#x27;)
plt.plot([min(y_test), max(y_test)], [min(y_test), max(y_test)], &#x27;--r&#x27;)
plt.xlabel(&quot;True Prices ($)&quot;)
plt.ylabel(&quot;Predicted Prices ($)&quot;)
plt.title(&quot;Actual vs. Predicted Prices&quot;)
plt.show()

# 7. 超参数优化
param_grid = {
    &quot;n_estimators&quot;: [50, 100, 200],
    &quot;max_depth&quot;: [None, 10, 20],
    &quot;min_samples_split&quot;: [2, 5, 10]
}
grid_search = GridSearchCV(RandomForestRegressor(random_state=42), param_grid, cv=5, scoring=&quot;r2&quot;, n_jobs=-1)
grid_search.fit(X_train, y_train)

# 8. 使用最佳参数重新训练
best_rf = grid_search.best_estimator_
best_rf.fit(X_train, y_train)
y_pred_best = best_rf.predict(X_test)

# 9. 评估优化后模型
mse_best = mean_squared_error(y_test, y_pred_best)
r2_best = r2_score(y_test, y_pred_best)
print(f&quot;Optimized MSE: {mse_best:.2f}&quot;)
print(f&quot;Optimized R²: {r2_best:.2f}&quot;)
```

**数据可视化**：通过 **相关性热图** 发现哪些特征对房价影响较大（例如 MedInc 对 Target 影响较大）。

![](img/c2f7669bdc5a4ab37cd39a4a8192c844.png)

**模型评估**：初始 R² 和 MSE 评估基本性能。

**优化模型**：通过 **超参数调优** (n_estimators, max_depth, min_samples_split) 提升预测性能。

**优化后结果**： R² 提升，MSE 降低，说明优化后模型更准确。

![](img/120d6d850bd095b3115fab239e83c1e0.png)

## 模型分析

### 随机森林回归优缺点

**优点：**

1.  **强大的非线性建模能力**：相比线性回归，随机森林可以捕捉复杂的非线性关系，非常适用于房价预测等涉及多种因素的任务。

1.  **降低过拟合风险**：单棵决策树容易过拟合，但随机森林通过集成学习（多个决策树投票），使得模型更稳健。

1.  **特征选择的重要性评估**：可以通过 feature_importances_ 查看各个特征的重要程度，方便特征工程优化。

1.  **鲁棒性强，对异常值不敏感**：由于是多个模型的平均值，单个异常数据不会极端影响预测结果。

1.  **无需特征缩放**：不像 SVM 或线性回归需要归一化，随机森林不受特征尺度影响。

**缺点：**

1.  **计算成本高**：相比单个决策树，随机森林需要训练多个树，计算开销大，尤其在超参数优化时会消耗大量资源。

1.  **难以解释**：不像决策树可以直观地理解每个分裂条件，随机森林的多个树组合在一起，使得解释性较差。

1.  **可能对高维稀疏数据不友好**：在数据维度非常高、稀疏性强（如 NLP 任务）时，随机森林可能不如线性模型或梯度提升树（GBDT）效果好。

### 随机森林回归 vs. 其他相似算法（对比表）

| **算法** | **适用场景** | **优点** | **缺点** |
| --- | --- | --- | --- |
| **随机森林回归** | 适用于特征多、数据量大且有非线性关系的任务，如房价预测、股票价格预测 | 抗过拟合、适用于非线性关系、能评估特征重要性 | 计算复杂度高，难以解释 |
| **决策树回归** | 适用于简单的回归任务，或者数据量较小的情况 | 易解释、计算速度快 | 容易过拟合，单棵树泛化能力差 |
| 梯度提升回归（GBDT，如 XGBoost, LightGBM） | 适用于数据规模大、需要高准确度的场景，如金融风控、广告点击率预测 | 精度高，能自动处理缺失值，支持特征重要性评估 | 训练时间较长，参数调优复杂 |
| **线性回归** | 适用于特征之间关系近似线性，且数据分布良好的情况 | 计算快，解释性强 | 无法处理复杂的非线性关系，易受异常值影响 |
| **支持向量回归** | 适用于小数据集、高维数据，且需要高精度预测 | 适用于小样本，能找到最优超平面 | 计算复杂度高，不适合大数据 |

### 什么时候优选 随机森林回归，什么时候可以考虑其他算法？

**优选随机森林的情况：**

+   当数据的特征较多，并且这些特征之间可能存在非线性关系（如房价预测）。

+   当你希望降低过拟合，但仍然想保持较强的预测能力。

+   当你想要了解特征对预测的贡献度（feature_importances_ 可用）。

+   当你不需要实时预测，而更关注预测的稳定性和准确度。

**考虑其他算法的情况：**

+   **数据规模特别大时**，如数百万以上的样本，梯度提升树（如 XGBoost、LightGBM）可能比随机森林更快、更高效。

+   数据是高维稀疏数据（如 NLP 任务），随机森林不适用，而 Lasso 回归或神经网络可能更合适。

+   **需要结果易解释的情况**，如医疗诊断、金融决策，可考虑决策树回归或线性回归，而不是随机森林。

+   **需要实时预测的情况**，如推荐系统、在线广告点击率预测，梯度提升树（如 LightGBM）或简单的线性模型通常更快。

### 总结

本案例中，房价预测涉及多个因素，并且变量之间可能存在非线性关系，因此随机森林是一个较好的选择。但如果数据规模进一步增大，或者需要更高的预测精度，可能需要尝试 XGBoost 或 LightGBM 等更高效的集成学习方法。