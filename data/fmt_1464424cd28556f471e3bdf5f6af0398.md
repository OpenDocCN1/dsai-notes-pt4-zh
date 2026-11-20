
# 01-02回归算法-Lasso回归

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Is2Jd9LIUoIhmyxy7lpcpi0vn7c](https://qqs7y1hozd1.feishu.cn/docx/Is2Jd9LIUoIhmyxy7lpcpi0vn7c)

`Lasso` 回归也是主要用于回归问题。

它的名字来自于“Least Absolute Shrinkage and Selection Operator”的缩写。

`Lasso` 回归是一种线性回归方法，它不仅能找到最佳的预测模型，还能自动选择出最重要的特征。它通过在损失函数中加入一个“惩罚项”来实现这一点。这个惩罚项是所有回归系数的绝对值之和。

那我们为什么要用 `Lasso` 回归？

就是在处理数据时，我们经常会遇到很多特征（变量），但不是所有特征都对结果有重要影响。`Lasso` 回归能帮助我们自动筛选出最有用的特征，忽略那些不重要的，这样模型更简单、预测效果更好。

## 理论基础

`Lasso` 回归就是在普通最小二乘法的基础上增加了一个惩罚项，用于约束模型中的系数，使某些系数变为零，从而实现特征选择。

### 数学公式

在普通的线性回归中，我们的目标是找到参数向量  使得损失函数（即残差平方和）最小化：

其中， 是第  个样本的特征向量， 是对应的目标值， 是回归系数向量。

在 `Lasso` 回归中，我们在损失函数中添加一个  正则化项，这个项是所有系数的绝对值之和。`Lasso` 回归的优化目标变为：

其中， 是正则化参数，用于控制惩罚项的权重。

### 公式推理

为了理解 `Lasso` 回归的公式推理，我们需要从优化问题的角度来看。

普通最小二乘法（OLS）：

最小化残差平方和：

`Lasso` 回归：

在 `OLS` 的基础上，加上一个  正则化项：

其中， 是正则化项，它通过增加非零系数的数量来惩罚模型的复杂性。

梯度下降法：

为了最小化这个目标函数，我们可以使用梯度下降法。梯度下降的更新规则如下：

其中， 是学习率， 是目标函数的梯度。

计算梯度时，普通最小二乘项的梯度是：

正则化项的梯度稍微复杂一些，因为  范数的导数在零点不连续。其导数形式是：

其中， 表示符号函数。

综合以上两部分，我们得到 `Lasso` 回归的梯度：

注意：梯度下降法仅是解决这个优化问题的一种方法，也有其他方法如坐标下降法。

### 算法流程

下面是 `Lasso` 回归的算法流程，假设使用梯度下降法来优化：

1. 初始化参数：
   - 设置学习率和正则化参数。
   - 初始化回归系数为零或小的随机值。

2. 迭代更新：
   - 计算预测值。
   - 计算误差。
   - 计算梯度。
   - 更新参数。
   - 重复上述步骤直到收敛（即参数变化很小或达到最大迭代次数）。

3. 输出结果：
   - 最终的回归系数。

通过上述流程，我们可以得到一个包含最重要特征的稀疏模型。

### 几何解释

![](img/a8e6114646f4ad4fa9226d50c2e0dff3.png)

假设我们有两个特征  和  ，`Lasso` 回归的惩罚项  的几何形状是一个菱形。而 `OLS` 的等值线是椭圆形。当我们在优化过程中缩小误差平方和的同时约束  （即菱形内的区域），最终会导致优化解在菱形的顶点处。这些顶点对应于一些系数（如  或  ）为零，从而实现特征选择。

总之，通过 `Lasso` 回归，我们不仅能得到一个适合数据的模型，还能自动筛选出对结果有重要影响的特征，使得模型更加简洁和解释性更强。

## 完整案例

我们使用加利福尼亚房价数据集，该数据集包含了1990年美国加州各个区的房屋价格以及相关的地理和人口统计数据。

这个数据集可以通过 `Scikit-learn` 库获取，大家不需要额外去寻找~

### 主要步骤

1. 加载和探索数据
2. 数据预处理
3. 特征工程
4. 建立和优化 `Lasso` 模型
5. 模型评估
6. 结果可视化

下面，咱们一步一步来实现，细节的地方大家可以看注释部分。

### 1. 加载和探索数据

首先，我们加载数据并进行初步探索。

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.linear_model import Lasso
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import GridSearchCV

# 加载数据
from sklearn.datasets import fetch_california_housing
data = fetch_california_housing()
df = pd.DataFrame(data.data, columns=data.feature_names)
df['MedHouseVal'] = data.target

# 初步查看数据
print(df.head())
print(df.describe())
```

### 2. 数据预处理

处理数据中的缺失值，并进行特征缩放。

```python
# 检查缺失值
print(df.isnull().sum())

# 分离特征和目标变量
X = df.drop('MedHouseVal', axis=1)
y = df['MedHouseVal']

# 数据分割
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 特征缩放
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### 3. 特征工程

可视化特征之间的关系以帮助理解数据。

```python
# 特征相关性
plt.figure(figsize=(10, 8))
sns.heatmap(df.corr(), annot=True, cmap='coolwarm', linewidths=0.5)
plt.title('Feature Correlation Heatmap')
plt.show()
```

![](img/da9174da7489619ea0797793504a6965.png)

### 4. 建立和优化 Lasso 模型

使用交叉验证和网格搜索优化 `Lasso` 模型。

```python
# 建立 Lasso 模型
lasso = Lasso()

# 定义超参数网格
param_grid = {'alpha': np.logspace(-4, 4, 50)}

# 网格搜索
grid_search = GridSearchCV(lasso, param_grid, cv=5, scoring='neg_mean_squared_error')
grid_search.fit(X_train_scaled, y_train)

# 最佳超参数
best_alpha = grid_search.best_params_['alpha']
print(f'Best alpha: {best_alpha}')

# 训练最终模型
lasso_opt = Lasso(alpha=best_alpha)
lasso_opt.fit(X_train_scaled, y_train)
```

### 5. 模型评估

评估模型性能，并计算各项指标。

```python
# 预测
y_pred_train = lasso_opt.predict(X_train_scaled)
y_pred_test = lasso_opt.predict(X_test_scaled)

# 评估
mse_train = mean_squared_error(y_train, y_pred_train)
mse_test = mean_squared_error(y_test, y_pred_test)
r2_train = r2_score(y_train, y_pred_train)
r2_test = r2_score(y_test, y_pred_test)

print(f'MSE (Train): {mse_train}')
print(f'MSE (Test): {mse_test}')
print(f'R^2 (Train): {r2_train}')
print(f'R^2 (Test): {r2_test}')
```

### 6. 结果可视化

可视化实际值与预测值之间的关系。

```python
# 可视化
plt.figure(figsize=(10, 6))
plt.scatter(y_test, y_pred_test, alpha=0.6, color='b')
plt.plot([0, 5], [0, 5], 'r--')
plt.xlabel('Actual')
plt.ylabel('Predicted')
plt.title('Actual vs Predicted House Prices')
plt.show()
```

![](img/c67166f09e37836e1629e6de46c5d3cf.png)

其中，有几个点，需要大家注意下~

1. 数据预处理：确保所有特征进行标准化，避免特征值差异过大对模型的影响。
2. 参数调整：使用网格搜索找到最佳的正则化参数 `alpha`。
3. 模型评估：计算 `MSE` 和 `R²` 来评估模型性能，并通过可视化检查预测效果。
4. 特征重要性：可以查看哪些特征对模型的贡献最大。`Lasso` 回归会将不重要的特征系数压缩为零，这也可以帮助我们理解特征的重要性。

通过上述整个的过程，大家基本可以全面了解如何使用 `Lasso` 回归进行房价预测，并且理解原理和代码的实现。最后，并通过优化提升模型性能。

## 模型分析

先来聊聊 `Lasso` 回归的优缺点~

### 优点

1. 特征选择：`Lasso` 回归会将一些不重要的特征的系数缩减为零，从而实现特征选择。这使得模型更加简洁和易于解释。
2. 减少过拟合：通过引入正则化项，`Lasso` 回归可以有效地减少过拟合，提高模型的泛化能力。
3. 简单高效：`Lasso` 回归相对简单，计算效率高，适用于处理高维数据。

### 缺点

1. 多重共线性问题：当特征之间存在多重共线性时，`Lasso` 回归可能无法正确选择特征，导致模型性能下降。
2. 计算复杂度：在大规模数据集上，`Lasso` 回归的计算复杂度可能较高，尤其是当使用网格搜索优化超参数时。
3. 模型解释性：虽然 `Lasso` 回归能进行特征选择，但对结果的解释性仍可能受到数据特性的影响，有时难以完全理解模型的行为。

### 与相似算法的对比

| 对比项 | `Ridge` 回归（岭回归） | `Elastic Net` 回归 |
| --- | --- | --- |
| 正则化方式 | 使用  范数（系数的平方和） | 结合了  和  范数的组合 |
| 特征选择 | 不进行特征选择，系数不会被缩减为零 | 能够进行特征选择，同时处理多重共线性问题 |
| 优点 | - 适用于多重共线性问题  - 不会将系数缩减为零，保留所有特征信息 | - 结合了`Lasso`和`Ridge`的优点  - 既能进行特征选择，又能处理多重共线性问题  - 更加灵活，适应性强 |
| 缺点 | - 无法进行特征选择，模型可能较复杂  - 不会丢弃不重要的特征 | - 需要调节两个正则化参数，模型复杂度增加 |
| 适用场景 | - 特征之间存在多重共线性  - 需要保留所有特征信息时 | - 需要同时处理多重共线性和特征选择问题  - 数据具有稀疏性时 |
| 调参 | 只需要调整一个正则化参数（正则化强度  ） | 需要调整两个正则化参数（  和  ） |
| 模型简洁性 | 由于不进行特征选择，模型可能较为复杂，包含所有特征 | 可以进行特征选择，模型更加简洁，保留重要特征 |
| 计算复杂度 | 计算复杂度相对较低，仅需调整一个参数 | 计算复杂度较高，需要调节两个正则化参数 |

### 使用场景

#### 适用场景

+ 特征数量多但重要特征少：`Lasso` 回归非常适合特征数量多，但其中只有少数特征真正重要的情况，因为它可以自动进行特征选择。
+ 减少模型复杂度：在希望模型更加简洁易于解释的场景中，`Lasso` 回归是一个好的选择。
+ 防止过拟合：在数据较少但特征较多的情况下，`Lasso` 回归通过正则化防止过拟合，提高模型的泛化能力。

#### 其他算法优选场景

+ 多重共线性：当特征之间存在强烈的多重共线性时，`Ridge` 回归或 `Elastic Net` 回归可能更合适，因为 `Lasso` 回归在这种情况下可能无法正确选择特征。
+ 所有特征都重要：如果所有特征都对模型预测有贡献，并且不希望丢弃任何特征，可以考虑使用 `Ridge` 回归。
+ 需要更强的正则化效果：在特征数量非常庞大的情况下，`Elastic Net` 回归可能提供更好的正则化效果，兼顾特征选择和多重共线性处理。

### 最后

`Lasso` 回归，其实特别适用于高维数据和需要特征选择的场景。然而，在多重共线性严重或需要保留所有特征的情况下，`Ridge` 回归或 `Elastic Net` 回归可能是更好的选择。
