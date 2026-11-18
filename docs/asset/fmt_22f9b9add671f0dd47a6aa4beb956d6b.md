
# 03-09集成算法-梯度提升

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/M87ldnncLoKLm1x90v7cEdVmnSd](https://qqs7y1hozd1.feishu.cn/docx/M87ldnncLoKLm1x90v7cEdVmnSd)

当你在做选择题，不确定答案，你就去问一群同学，然后“听大多数人的意见”来选答案。这就是集成算法。它的核心思路就是：多个模型联合起来投票、协作，做出更好的决策。

梯度提升就像你一个人做题做得不太好，但你不放弃，每次找出错在哪，然后请“更聪明的小助手”专门来帮你改正错误，下一次做得更好。你一次次改进，最后整体的结果就越来越棒了！

## 梯度提升的工作方式

1. 先训练一个很简单的模型（比如一个弱小的决策树），它尝试去预测结果。
2. 这个模型肯定不完美，所以我们就看它“错在哪了”（就是它预测值和实际值的差距）。
3. 然后训练第二个模型，目标是“专门去修补前一个模型的错误”。
4. 不断重复这个过程：每次都让新的模型去“补前面所有模型犯的错”。
5. 最终，把所有这些模型的预测“加起来”，就能得到一个超强预测结果！

这就好像你每次考试都反思错题，然后请不同老师来专门教你不会的部分。

经过很多轮，你的整体成绩就突飞猛进了。

## 原理详解

### 数学原理

#### 问题设定（监督学习）

我们希望通过一个模型去拟合输入与输出之间的关系。

目标是最小化损失函数（Loss Function）：

在实际中我们是给定样本，变成经验风险最小化：

### 模型形式

我们使用加法模型（Additive Model）：

：最终的模型

：第 `i` 个“弱学习器”模型（通常是决策树）

：每个学习器的权重（学习率）

：总的迭代轮数（学习器数量）

模型是“逐步构建的”，每一步往模型里加一个新的学习器。

### 逐步优化思想

我们一步一步地构建模型，每一步都选择一个新的弱学习器和对应系数，使得损失尽可能下降：

第 `m` 步优化目标：

解释：

+ 是前  个弱学习器构成的模型
+ 新的  是为了进一步降低总损失

但这个优化不容易直接做，我们引入梯度下降的思想！

### 引入梯度下降

我们对损失函数进行一阶泰勒展开（也就是用梯度近似）：

设：

那么我们就可以近似成：最小化负梯度方向的误差

> 梯度提升的核心思想：每一步都用一个新模型去拟合损失函数在当前模型下的负梯度！

### 梯度提升算法流程（细化步骤）

我们以回归问题为例（损失函数是平方误差）：

#### 初始化

对平方误差来说，就是：

#### 对于每一轮  到  ：

**第一步：计算残差（负梯度）**

对于平方误差，有：

**第二步：拟合残差**

用一个弱学习器去拟合这些残差：

也可以理解为“向残差方向修正”。

**第三步：确定步长**

对于平方误差来说，这就是最小二乘问题。

**第四步：更新模型**

其中 `learning_rate` 是学习率（通常很小，如 0.1 或 0.01），用于控制每一步更新的幅度，防止过拟合。

#### 举例：平方误差下的推导过程

平方误差：

梯度是：

+ 残差就是当前模型与真实值的差值
+ 每轮就是“拟合残差 + 加入模型”

#### 扩展：分类任务中的梯度提升

对于二分类，损失函数可以是对数损失（Log Loss）：

这时每一轮我们依然计算负梯度：

后续的流程相同，只是损失函数和梯度不同。

## 完整案例

咱们这里使用`Scikit-learn`内置的“Breast Cancer Wisconsin”数据集，该数据集包含了乳腺癌患者的各项特征数据，通过分类任务预测患者是否患有恶性肿瘤。

我们利用梯度提升算法构建模型，并通过`GridSearchCV`进行超参数调优，最终使用多种指标对模型进行评估和可视化。

### 数据预处理

首先，我们导入所需的库，并对数据集进行加载、预处理和初步探索。数据预处理包括数据标准化、数据集拆分等。探索性数据分析（EDA）能够帮助我们了解数据分布、特征相关性，为后续模型调参提供依据。

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import confusion_matrix, classification_report, roc_curve, auc

import warnings
warnings.filterwarnings('ignore')

# 加载数据
data = load_breast_cancer()
df = pd.DataFrame(data.data, columns=data.feature_names)
df['target'] = data.target

# 输出数据集的基本信息
print("数据集样本数：", df.shape[0])
print("特征数量：", df.shape[1] - 1)
print("类别分布：\n", df['target'].value_counts())

# 分析各特征的分布情况
plt.figure(figsize=(12, 6))
sns.histplot(df['mean radius'], kde=True, color='darkorange')
plt.xlabel('mean radius', fontsize=12)
plt.title('Distribution of Mean Radius', fontsize=14)
plt.show()

# 分析特征间的相关性，绘制热力图
plt.figure(figsize=(14, 10))
corr_matrix = df.drop('target', axis=1).corr()
sns.heatmap(corr_matrix, cmap='viridis', annot=False)
plt.xlabel('Features', fontsize=12)
plt.ylabel('Features', fontsize=12)
plt.title('Feature Correlation Matrix', fontsize=14)
plt.show()
```

1. 数据加载后可见共有569个样本，30个特征以及1个目标变量。目标变量0代表恶性肿瘤，1代表良性肿瘤（数据集的具体含义请参考数据说明）。

2. 绘制“mean radius”的分布图有助于观察该特征是否呈现正态分布，便于判断是否需要进行数据转换或标准化。

![](img/16d20c5f7ed3127b4788292fa8dadd2a.png)

3. 相关性热力图展示了各个特征之间的相关关系，能够帮助发现高度相关的特征，以便于后续特征选择或降维。

![](img/be41934209cbd7448301a8e89cb7df39.png)

### 数据集拆分与标准化

为了保证模型能够泛化，我们将数据集划分为训练集和测试集，并进行标准化处理，使得各特征处于相同的尺度上，从而提高梯度提升算法的收敛速度和性能。

```python
# 特征和标签分离
X = df.drop('target', axis=1)
y = df['target']

# 拆分数据集为训练集和测试集，测试集占比30%
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42, stratify=y)

# 对特征进行标准化处理
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

print("训练集样本数：", X_train.shape[0])
print("测试集样本数：", X_test.shape[0])
```

1. 采用`stratify`参数保证训练集和测试集中类别分布一致，避免因数据不平衡带来的偏差。

2. 标准化处理对大多数机器学习算法都有益处，尤其是梯度下降类算法，有助于提高算法的稳定性和收敛速度。

### 模型训练：梯度提升分类器

接下来，我们构建梯度提升分类器（`GradientBoostingClassifier`），利用默认参数进行初步训练，并对模型性能进行初步评估。

```python
# 利用默认参数构建梯度提升分类器
gbc = GradientBoostingClassifier(random_state=42)
gbc.fit(X_train_scaled, y_train)

# 在测试集上进行预测
y_pred = gbc.predict(X_test_scaled)

# 输出分类报告
print("分类报告（Default Parameters）:")
print(classification_report(y_test, y_pred))

# 绘制混淆矩阵
cm = confusion_matrix(y_test, y_pred)
plt.figure(figsize=(6, 5))
sns.heatmap(cm, annot=True, fmt='d', cmap='YlGnBu')
plt.xlabel('Predicted Label', fontsize=12)
plt.ylabel('True Label', fontsize=12)
plt.title('Confusion Matrix', fontsize=14)
plt.show()
```

1. 使用默认参数构建模型后，通过`classification_report`可以直观地看到模型在精度、召回率、F1-score等指标上的表现。

2. 混淆矩阵有助于发现模型在预测过程中是否存在偏向某一类别的情况，为后续的调参和优化提供依据。

![](img/e74d7021268b3ef7cd7ccd307ea2b6b8.png)

### 模型超参数调优

模型的性能高度依赖于超参数的设置。我们使用`GridSearchCV`来自动搜索最佳超参数组合。主要调参参数包括：

+ `n_estimators`：弱学习器（决策树）的数量
+ `learning_rate`：每个弱学习器贡献的权重
+ `max_depth`：树的最大深度，用于控制过拟合

```python
# 定义超参数网格
param_grid = {
    'n_estimators': [50, 100, 150],
    'learning_rate': [0.01, 0.1, 0.2],
    'max_depth': [3, 4, 5]
}

# 利用GridSearchCV进行超参数搜索，采用5折交叉验证
grid_search = GridSearchCV(GradientBoostingClassifier(random_state=42), 
                           param_grid, 
                           cv=5, 
                           n_jobs=-1,
                           scoring='accuracy')
grid_search.fit(X_train_scaled, y_train)

print("最佳参数：", grid_search.best_params_)
print("最佳交叉验证得分：", grid_search.best_score_)

# 使用最佳参数构建新的模型
best_gbc = grid_search.best_estimator_
y_pred_best = best_gbc.predict(X_test_scaled)

print("分类报告（Optimized Parameters）:")
print(classification_report(y_test, y_pred_best))

# 绘制优化后模型的混淆矩阵
cm_best = confusion_matrix(y_test, y_pred_best)
plt.figure(figsize=(6, 5))
sns.heatmap(cm_best, annot=True, fmt='d', cmap='coolwarm')
plt.xlabel('Predicted Label', fontsize=12)
plt.ylabel('True Label', fontsize=12)
plt.title('Confusion Matrix (Optimized)', fontsize=14)
plt.show()
```

1. 通过`GridSearchCV`，我们系统地遍历了超参数的组合，找到在交叉验证中表现最好的参数。调参过程不仅提高了模型的准确性，同时也能降低过拟合的风险。

2. 调参后模型的分类报告和混淆矩阵较默认参数模型有明显改善，说明超参数对模型性能具有关键影响。

3. 通过对比优化前后结果，可以验证调参过程的有效性。

![](img/da9b7a1dbc15adf2010ea9b9bebc108c.png)

### 模型评估与结果可视化

为了更全面地评估模型，我们除了查看混淆矩阵和分类报告外，还绘制了ROC曲线来分析模型在不同阈值下的分类性能。

```python
# 计算预测概率
y_proba = best_gbc.predict_proba(X_test_scaled)[:, 1]

# 计算ROC曲线
fpr, tpr, thresholds = roc_curve(y_test, y_proba)
roc_auc = auc(fpr, tpr)

plt.figure(figsize=(8, 6))
plt.plot(fpr, tpr, color='darkred', lw=2, label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
plt.xlabel('False Positive Rate', fontsize=12)
plt.ylabel('True Positive Rate', fontsize=12)
plt.title('Receiver Operating Characteristic', fontsize=14)
plt.legend(loc="lower right")
plt.show()
```

1. ROC曲线可以全面反映模型在各种分类阈值下的表现，曲线下面积（AUC）是衡量模型整体判别能力的重要指标。

2. 理想的ROC曲线应尽可能靠近左上角，AUC值越高表示模型的区分能力越强。

3. 在本例中，通过优化后的模型得到较高的AUC，证明调参优化带来的提升具有统计学意义。

![](img/cbcd395d9d83bb32142396138f971a80.png)

### 特征重要性分析

梯度提升算法在训练过程中会自动评估各特征的重要性，帮助我们理解哪些特征对最终预测贡献最大。下面我们绘制特征重要性柱状图，并对结果进行解释。

```python
# 获取各特征的重要性
feature_importances = best_gbc.feature_importances_
features = X.columns

# 构造DataFrame，方便排序和展示
importance_df = pd.DataFrame({
    'Feature': features,
    'Importance': feature_importances
}).sort_values(by='Importance', ascending=False)

plt.figure(figsize=(12, 6))
sns.barplot(x='Importance', y='Feature', data=importance_df, palette='Spectral')
plt.xlabel('Feature Importance', fontsize=12)
plt.ylabel('Feature', fontsize=12)
plt.title('Feature Importance Ranking', fontsize=14)
plt.show()
```
