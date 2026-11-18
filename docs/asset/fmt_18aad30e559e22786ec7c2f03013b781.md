
# 02-02分类算法-ID3算法

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Ri8odWtdfodeIkxnzYMcGOAbnJf](https://qqs7y1hozd1.feishu.cn/docx/Ri8odWtdfodeIkxnzYMcGOAbnJf)

在学习决策树的时候，大家一定遇到了其中的ID3算法。

ID3算法（Iterative Dichotomiser 3）本身是一种用于分类的决策树算法，由Ross Quinlan在1986年提出。

它基于信息论中的**信息增益**概念，递归地选择具有最高信息增益的属性作为节点，将数据划分为多个子集，直到满足停止条件（如所有样本属于同一类别或没有剩余属性）为止。

## ID3核心步骤

1. 计算每个特征的信息增益。
2. 选择信息增益最高的特征作为当前节点的分裂标准。
3. 对分裂后的子数据集递归重复步骤1和2，构建决策树。
4. 停止条件为：所有数据属于同一类别，或数据集不可分，或特征用尽。

简单举例来说，假设你是个侦探，想通过观察一些线索来推断一个案件的可能结果，比如「是事故还是犯罪」。

你有以下线索：

+ 天气
+ 地点
+ 目击者数量
+ 时间段

ID3算法就像一个逻辑筛选器：

1. 挑线索：先挑出最关键的线索，比如「目击者数量」可能是影响结果的最重要因素，因为它能最清晰地区分案件类型。
2. 用线索分类：根据「目击者数量」分类，比如：

   + 如果目击者是0，很可能是事故。
   + 如果目击者很多，可能是犯罪。

3. 继续挑下一个线索：在目击者数量的基础上，再挑选其他次重要的线索，比如「时间段」。
4. 层层筛选：每次都用最能帮你区分的线索，直到你有明确答案。

最终，你会构建出一个逻辑决策的树形结构，像「如果目击者=0，结果=事故；如果目击者很多且时间是夜晚，结果=犯罪」。

简单说，ID3算法就是一套「不断挑最重要线索、用线索分类」的方法，帮助你从复杂数据中得出最清晰的判断。

## 理论基础

ID3算法的核心数学基础是信息论中的信息熵（Entropy）和信息增益（Information Gain）。

### 信息熵（Entropy）

信息熵衡量的是系统的混乱程度或不确定性。假设数据集包含个样本，分类结果有个类别，其中第类样本的概率为，则信息熵的定义为：

：数据集的信息熵。

：属于第类的样本比例，计算方式为，其中是第类的样本数量，是样本总数。

+ 信息熵越高，数据的不确定性越大；信息熵为0时，数据完全确定（即所有样本属于同一类别）。

### 条件熵（Conditional Entropy）

条件熵表示在给定某个特征的条件下，数据集的不确定性。假设特征有个可能的取值，将数据集根据特征的取值划分为个子集，则条件熵定义为：

：子集的样本数量。

：子集的信息熵。

条件熵衡量的是特征对数据集的分类效果。条件熵越小，说明的分类效果越好。

### 信息增益（Information Gain）

信息增益衡量的是由于特征的引入，数据集的不确定性减少的程度。信息增益定义为：

：特征的信息增益。

：数据集的信息熵。

：在给定特征的条件下，数据集的条件熵。

信息增益越大，说明特征的分类效果越好。

### 特征选择

在构建决策树时，ID3算法选择信息增益最大的特征作为当前节点的分裂标准。

### ID3算法的流程

**1. 输入**：数据集、特征集合、类别标签集合。

**2. 输出**：决策树。

**3. 步骤**：

Step 1: 计算根节点的信息熵

+ 根据类别标签计算数据集的信息熵。

Step 2: 计算每个特征的信息增益

+ 对于每个特征：

  1. 根据特征的可能取值，将数据集划分为多个子集。
  2. 计算特征的条件熵。
  3. 计算特征的信息增益。

Step 3: 选择信息增益最大的特征

+ 找到具有最大信息增益的特征。
+ 如果所有特征的信息增益都很小（低于设定阈值）或样本无法继续分裂，则停止分裂。

Step 4: 构建决策树节点

+ 使用特征创建一个决策树节点。
+ 根据特征的取值划分数据集，生成对应的分支。

Step 5: 对子集递归构建决策树

+ 对于每个子集：

  1. 如果子集中的样本全属于同一类别，则将当前分支标记为叶子节点，类别为该类别。
  2. 如果子集中没有剩余特征，则将当前分支标记为叶子节点，类别为该子集样本中占比最大的类别。
  3. 否则，对子集的剩余特征集合递归执行上述步骤。

Step 6: 终止条件

+ 所有样本属于同一类别。
+ 没有剩余特征可以分裂。
+ 样本无法继续分裂（即样本数量小于设定阈值）。

**输出决策树**：返回构建好的决策树。

ID3算法的核心在于使用信息增益选择最优特征，通过递归地分裂数据集构建决策树。它优点在于简单直观，适合处理离散特征的数据集，但由于偏向选择取值较多的特征，可能导致过拟合问题。

## 应用场景

### ID3算法适用问题类型

1. 分类问题：ID3算法用于解决具有**离散特征**的数据分类任务。
2. 目标变量是离散的：目标变量必须是类别（如“是/否”或多类别）。
3. 适合较小规模数据集：ID3适合中小规模数据集，大规模数据可能导致性能下降。

**优点**：

1. 易于理解和实现：决策树结构直观，算法简单。
2. 高效处理离散数据：天然适合离散特征。
3. 无需数据预处理：不需要对特征进行归一化或缩放。
4. 结果可解释性强：构造出的树形结构便于人类理解分类规则。

**缺点**：

1. 易于过拟合：ID3倾向于选择取值较多的特征（偏向分裂次数多的特征），可能导致过拟合。
2. 处理连续特征能力差：需要对连续特征进行离散化。
3. 对噪声敏感：噪声数据可能显著影响结果。
4. 偏向信息增益高的特征：可能导致重要特征被忽略。
5. 不适合动态变化的特征集：算法需要从头训练决策树。

### 运用时的前提条件

1. 特征是离散的：ID3不直接处理连续特征，需预处理为离散数据。
2. 数据无严重噪声：数据质量较高的情况下效果较好。
3. 特征独立性假设：算法假设特征之间相互独立。
4. 类别明确：目标变量的类别已知且分布较平衡。

## 完整案例

一个完整的 **ID3 算法案例**，包括 Python 实现、数据集、生成决策树以及绘制数据分析图表。

整个案例展示了如何用 ID3 算法分类数据，图表包括特征选择的信息增益分析和决策树的可视化。

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics import accuracy_score

# 1. 生成虚拟数据集
data = {
    &#x27;Weather&#x27;: [&#x27;Sunny&#x27;, &#x27;Sunny&#x27;, &#x27;Overcast&#x27;, &#x27;Rainy&#x27;, &#x27;Rainy&#x27;, &#x27;Rainy&#x27;, &#x27;Overcast&#x27;, &#x27;Sunny&#x27;, 
                &#x27;Sunny&#x27;, &#x27;Rainy&#x27;, &#x27;Sunny&#x27;, &#x27;Overcast&#x27;, &#x27;Overcast&#x27;, &#x27;Rainy&#x27;],
    &#x27;Temperature&#x27;: [&#x27;Hot&#x27;, &#x27;Hot&#x27;, &#x27;Hot&#x27;, &#x27;Mild&#x27;, &#x27;Cool&#x27;, &#x27;Cool&#x27;, &#x27;Cool&#x27;, &#x27;Mild&#x27;, 
                    &#x27;Cool&#x27;, &#x27;Mild&#x27;, &#x27;Mild&#x27;, &#x27;Mild&#x27;, &#x27;Hot&#x27;, &#x27;Mild&#x27;],
    &#x27;Humidity&#x27;: [&#x27;High&#x27;, &#x27;High&#x27;, &#x27;High&#x27;, &#x27;High&#x27;, &#x27;Normal&#x27;, &#x27;Normal&#x27;, &#x27;Normal&#x27;, &#x27;High&#x27;, 
                 &#x27;Normal&#x27;, &#x27;Normal&#x27;, &#x27;Normal&#x27;, &#x27;High&#x27;, &#x27;Normal&#x27;, &#x27;High&#x27;],
    &#x27;Windy&#x27;: [&#x27;False&#x27;, &#x27;True&#x27;, &#x27;False&#x27;, &#x27;False&#x27;, &#x27;False&#x27;, &#x27;True&#x27;, &#x27;True&#x27;, &#x27;False&#x27;, 
              &#x27;False&#x27;, &#x27;False&#x27;, &#x27;True&#x27;, &#x27;True&#x27;, &#x27;False&#x27;, &#x27;True&#x27;],
    &#x27;PlayTennis&#x27;: [&#x27;No&#x27;, &#x27;No&#x27;, &#x27;Yes&#x27;, &#x27;Yes&#x27;, &#x27;Yes&#x27;, &#x27;No&#x27;, &#x27;Yes&#x27;, &#x27;No&#x27;, 
                   &#x27;Yes&#x27;, &#x27;Yes&#x27;, &#x27;Yes&#x27;, &#x27;Yes&#x27;, &#x27;Yes&#x27;, &#x27;No&#x27;]
}
df = pd.DataFrame(data)

# 2. 数据预处理
# 将分类变量转换为数值
le = LabelEncoder()
for column in df.columns:
    df[column] = le.fit_transform(df[column])

# 特征和目标变量
X = df[[&#x27;Weather&#x27;, &#x27;Temperature&#x27;, &#x27;Humidity&#x27;, &#x27;Windy&#x27;]]
y = df[&#x27;PlayTennis&#x27;]

# 3. 构建 ID3 决策树
model = DecisionTreeClassifier(criterion=&#x27;entropy&#x27;, max_depth=3, random_state=42)
model.fit(X, y)

# 4. 输出信息增益分析
features = [&#x27;Weather&#x27;, &#x27;Temperature&#x27;, &#x27;Humidity&#x27;, &#x27;Windy&#x27;]
info_gain = []
H_S = -np.sum((np.bincount(y) / len(y)) * np.log2(np.bincount(y) / len(y)))  # 数据集熵
for i, feature in enumerate(features):
    unique_values = np.unique(X.iloc[:, i])
    H_S_A = 0
    for value in unique_values:
        subset = y[X.iloc[:, i] == value]
        p = len(subset) / len(y)
        H_S_A += p * (-np.sum((np.bincount(subset) / len(subset)) * np.log2(np.bincount(subset) / len(subset))))
    info_gain.append(H_S - H_S_A)

# 可视化信息增益
plt.figure(figsize=(8, 6))
plt.bar(features, info_gain, color=[&#x27;red&#x27;, &#x27;blue&#x27;, &#x27;green&#x27;, &#x27;purple&#x27;], alpha=0.8)
plt.title(&quot;Information Gain for Each Feature&quot;, fontsize=16)
plt.xlabel(&quot;Features&quot;, fontsize=14)
plt.ylabel(&quot;Information Gain&quot;, fontsize=14)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.grid(axis=&#x27;y&#x27;, linestyle=&#x27;--&#x27;, alpha=0.7)
plt.show()

# 5. 决策树可视化
plt.figure(figsize=(12, 8))
plot_tree(model, feature_names=features, class_names=[&#x27;No&#x27;, &#x27;Yes&#x27;], filled=True, rounded=True, fontsize=10)
plt.title(&quot;Decision Tree Visualization&quot;, fontsize=16)
plt.show()

# 6. 测试与优化
# 模拟新数据
test_data = pd.DataFrame({
    &#x27;Weather&#x27;: [&#x27;Sunny&#x27;, &#x27;Rainy&#x27;, &#x27;Overcast&#x27;],
    &#x27;Temperature&#x27;: [&#x27;Hot&#x27;, &#x27;Mild&#x27;, &#x27;Cool&#x27;],
    &#x27;Humidity&#x27;: [&#x27;Normal&#x27;, &#x27;High&#x27;, &#x27;High&#x27;],
    &#x27;Windy&#x27;: [&#x27;False&#x27;, &#x27;True&#x27;, &#x27;False&#x27;]
})

# 数据转换
for column in test_data.columns:
    test_data[column] = le.fit_transform(test_data[column])

# 预测
predictions = model.predict(test_data)
print(&quot;Predictions for test data:&quot;, predictions)

# 优化策略（可选）：调整树的深度，选择特征重要性
optimized_model = DecisionTreeClassifier(criterion=&#x27;entropy&#x27;, max_depth=2, random_state=42)
optimized_model.fit(X, y)
optimized_predictions = optimized_model.predict(test_data)
print(&quot;Optimized Predictions:&quot;, optimized_predictions)

# 输出准确率（仅供模拟）
y_pred = model.predict(X)
accuracy = accuracy_score(y, y_pred)
print(&quot;Model Accuracy on Training Data:&quot;, accuracy)
```

**信息增益柱状图**：柱状图显示了每个特征的信息增益，帮助理解哪些特征对分类贡献最大。

![](img/31f11164ccc982c4f04c28f0835cd596.png)

**决策树可视化**：决策树直观展示了分类规则，从根节点到叶子节点的路径即分类逻辑。

![](img/ca16f36d19bdfe3ba0a304efa5cf3ad1.png)

**优化策略**：通过限制树的最大深度，减少过拟合风险。预测新数据并评估模型的表现。

### 改进优化方法

1. 剪枝策略：使用后剪枝技术，移除冗余节点。
2. 特征选择：结合特征重要性，去除信息增益低的特征。
3. 交叉验证：使用交叉验证评估模型性能，避免单一数据集的偏差。
4. 处理连续特征：对连续特征进行分段处理（如分箱或聚类）。
