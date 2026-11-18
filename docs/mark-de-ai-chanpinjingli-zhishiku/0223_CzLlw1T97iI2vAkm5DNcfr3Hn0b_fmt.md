
# 09-12数据预处理方法-特征选择

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/CmyodlnF3oEPjexYMvpcVMQCnGd](https://qqs7y1hozd1.feishu.cn/docx/CmyodlnF3oEPjexYMvpcVMQCnGd)

在开始说特征选择前，咱们先搞清楚“特征”是啥。

打个比方，如果我们要训练一个模型来判断某个人是否会买一双运动鞋，我们可能会收集一些跟这个人有关的信息，比如：

+ 年龄  
+ 性别  
+ 喜欢运动吗  
+ 平时穿什么鞋  
+ 收入多少  
+ 是否经常网购  

这些信息就是“特征”，也可以叫“变量”或“属性”。

简单说，就是用来描述某个对象（在这个例子中是“人”）的一组数据。

## 什么是特征选择？

**特征选择**，就是从一堆特征中**挑出对结果最有用的那几个**。

比如我们收集了100个特征，但其实可能只有5~10个是对我们要解决的问题真正有帮助的。其余的特征不仅没用，有时候还会干扰模型判断，让结果更差。

特征选择的目的就像是：

> 我们在菜市场挑菜，要做一道菜，不需要所有的菜，只挑对的、好的、能用上的菜。

## 为什么要做特征选择？

原因很简单，有几个关键点：

1. **减少数据维度**：特征太多会让模型变慢、变复杂，选出关键特征后，模型更快也更好理解。  
2. **提高模型性能**：去掉“干扰项”，模型能更专注于真正重要的因素，预测结果可能更准。  
3. **防止过拟合**：特征太多可能会让模型“记住”训练数据而不是“学会”规律，选少一些能防止这个问题。  
4. **节省资源**：比如数据采集和存储的成本也会降低。

## 特征选择方法简介

不用展开太复杂，这里简单提几个常见的方法：

+ **看相关性**：比如统计每个特征和结果之间的关系，关系弱的就去掉。  
+ **用模型评估**：让模型自己“评分”哪些特征更有用，选得分高的。  
+ **一步一步试**：每次加入或去掉一个特征，看模型效果变好还是变差。

总结一句话，特征选择就像是在做减法，我们从一堆数据中挑出对结果最有帮助的那几项，让模型更聪明、更高效、更靠谱。

## 原理详解

特征选择（`Feature Selection`）本质是寻找与目标变量（`label`）关联性强、冗余性低的特征子集。

其目标可归结为一个优化问题：

其中：

+ 是所有特征的集合，假设有 个特征：  
+ 是从 中选择的子集（特征子集）  
+ 是衡量特征子集“好坏”的评分函数，通常基于互信息、相关系数、模型性能等  

特征选择要解决的问题有两个关键点：

1. **相关性（`Relevance`）**：特征是否对目标变量有信息量  
2. **冗余性（`Redundancy`）**：特征之间是否高度相关、重复  

## 数学指标

### 1. 互信息 `Mutual Information`（`MI`）

衡量特征与目标之间的信息量：

解释：

+ 如果 和 完全独立，则  
+ 越高表示  提供关于  越多的信息，越“相关”

在特征选择中，我们希望选择互信息较大的特征。

### 2. 方差选择法（`Variance Threshold`）

如果一个特征的方差接近于0，说明这个特征在所有样本中几乎是常量，对分类或回归没有贡献：

我们可以设定一个阈值 ，选择满足  的特征。

### 3. 皮尔逊相关系数（`Pearson Correlation`）

衡量特征与目标  的线性相关性：

适合数值型变量，绝对值越大，相关性越强。

### 4. 最小冗余最大相关（`mRMR`）

这是一个综合相关性与冗余性的方法，目标函数：

其中：

+ 第一项是特征 和目标 的互信息（代表相关性）  
+ 第二项是 和已选特征集合 的平均互信息（代表冗余性）

我们希望选择得分最大的特征。

### 5. 正则化方法：`L1` 正则（`Lasso`）

`Lasso` 是一种模型内嵌特征选择方法，它在损失函数中加入  范数项：

`Lasso` 的一个性质是会将部分权重 ，即对应特征被自动“剔除”，从而实现特征选择。

## 特征选择的算法流程（分类）

### 方法分类

1. **`Filter`（过滤法）**

+ 不依赖模型，单独评估特征  
+ 如：互信息、相关系数、卡方检验、方差过滤  

2. **`Wrapper`（包装法）**

+ 使用模型训练+验证去评估特征子集的性能  
+ 如：递归特征消除（`RFE`）  

3. **`Embedded`（嵌入法）**

+ 模型训练中自动完成选择  
+ 如：`Lasso`、决策树的特征重要性  

### `Filter` 方法流程

以互信息为例：

1. 输入：数据集 、目标变量  
1. 对每个特征 ，计算  
1. 选择前  个互信息最大的特征，或设阈值筛选  
1. 输出特征子集  

### `Wrapper` 方法流程（如递归特征消除 `RFE`）：

1. 输入：数据集 、标签 、学习器模型（如`SVM`）  
1. 用所有特征训练模型  
1. 根据模型权重排序特征重要性  
1. 删除最不重要的特征（一个或多个）  
1. 重复 2~4 步，直到达到预期特征数  
1. 输出特征子集  

### `Embedded` 方法流程（以 `Lasso` 为例）：

1. 输入：数据集 、标签 、`L1`正则系数  
1. 用 `Lasso` 回归拟合数据，最小化损失函数  
1. 得到权重向量 ，统计哪些特征  
1. 输出这些非零权重对应的特征子集  

## 完整案例

咱们这里的案例包括：

1. 数据加载与预处理  
1. 多种特征选择方法应用（`Filter`、`Wrapper`、`Embedded`）  
1. 可视化分析  
1. 模型性能评估与优化  

数据方面，我们以经典的UCI乳腺癌数据集（Breast Cancer Wisconsin Dataset）为例，进行实验~

### 导入库并加载数据

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression, LassoCV
from sklearn.feature_selection import SelectKBest, mutual_info_classif, RFE
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, roc_auc_score, classification_report, confusion_matrix

sns.set(style="whitegrid")
plt.rcParams["figure.figsize"] = (10, 6)
plt.rcParams["axes.titlesize"] = 16
plt.rcParams["axes.labelsize"] = 12
```

### 数据预览与探索性分析

```
# 读取数据集
data = load_breast_cancer()
df = pd.DataFrame(data.data, columns=data.feature_names)
df['target'] = data.target

# 基本信息
print(df.info())
print(df.describe())

# 类别分布
sns.countplot(x='target', data=df, palette='Set2')
plt.title("Target Class Distribution")
plt.xlabel("Class")
plt.ylabel("Count")
plt.show()
```

这是一个二分类问题（0=恶性，1=良性），类分布不平衡不明显，适合用于模型训练。

![](img/d0672aa501290c1d7d049017a3cb3237.png)

### 特征标准化

```
X = df.drop('target', axis=1)
y = df['target']

# 标准化
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

部分模型（如逻辑回归、`Lasso`）对特征量纲敏感，因此需要标准化。

### `Filter` 法：`SelectKBest` + 互信息

```
# 选择前10个互信息得分最高的特征
selector_mi = SelectKBest(score_func=mutual_info_classif, k=10)
X_mi = selector_mi.fit_transform(X_scaled, y)
selected_features_mi = X.columns[selector_mi.get_support()]

# 可视化互信息得分
mi_scores = selector_mi.scores_
mi_df = pd.DataFrame({'Feature': X.columns, 'MI Score': mi_scores})
mi_df = mi_df.sort_values(by='MI Score', ascending=False)

sns.barplot(data=mi_df.head(15), x='MI Score', y='Feature', palette='plasma')
plt.title("Top 15 Mutual Information Scores")
plt.xlabel("Score")
plt.ylabel("Feature")
plt.show()
```

互信息反映特征与目标变量之间的非线性关系，分数越高，说明该特征包含的“信息量”越大。

![](img/b9681e172d16eeaeb88cc662a11c6a78.png)

### `Wrapper` 法：递归特征消除 `RFE`

```
# 用逻辑回归做基模型，递归删除特征
model = LogisticRegression(max_iter=5000)
rfe = RFE(estimator=model, n_features_to_select=10)
X_rfe = rfe.fit_transform(X_scaled, y)

selected_features_rfe = X.columns[rfe.support_]

# 可视化选择结果
rfe_result = pd.DataFrame({
    'Feature': X.columns,
    'Selected': rfe.support_
})
sns.barplot(data=rfe_result, x='Selected', y='Feature', palette='coolwarm')
plt.title("RFE Feature Selection Result")
plt.xlabel("Selected (1=True)")
plt.ylabel("Feature")
plt.show()
```

`Wrapper` 方法通过“反复试验”直接评估特征对模型性能的影响，适合找组合最优特征子集。

![](img/a871f183ad248bc1736706446931a03e.png)

### `Embedded` 法：`Lasso` 回归自动选择

```
lasso = LassoCV(cv=5, random_state=0)
lasso.fit(X_scaled, y)

# `Lasso` 回归系数
lasso_coef = pd.Series(lasso.coef_, index=X.columns)
selected_features_lasso = lasso_coef[lasso_coef != 0].index

# 可视化
lasso_coef_sorted = lasso_coef.sort_values(key=abs, ascending=False).head(15)
lasso_coef_sorted.plot(kind='barh', color='darkorange')
plt.title("Top Lasso Coefficients")
plt.xlabel("Coefficient Value")
plt.ylabel("Feature")
plt.grid(True)
plt.show()
```

`Lasso` 通过惩罚项自动将一些不重要的特征系数压缩为 0，实现了模型训练与特征选择同步进行。

![](img/7883c7c578948d06f89771c40acee8f0.png)

### 比较不同方法选择的特征

```
methods = ['Mutual Info', 'RFE', 'Lasso']
sets = [set(selected_features_mi), set(selected_features_rfe), set(selected_features_lasso)]

# 构建特征选择结果的对比图
all_features = list(X.columns)
results_df = pd.DataFrame({'Feature': all_features})

for method, selected in zip(methods, sets):
    results_df[method] = results_df['Feature'].apply(lambda f: 1 if f in selected else 0)

# 可视化为热力图
sns.heatmap(results_df.set_index('Feature').T, cmap='YlGnBu', cbar=True, linewidths=1, linecolor='gray')
plt.title("Feature Selection Comparison (Heatmap)")
plt.xlabel("Feature")
plt.ylabel("Method")
plt.show()
```

直观对比不同方法所选的特征，发现有些特征是多方法共同选中的，可信度更高。

![](img/cd6b5d3f0dd73ad4c2d4e2e8ec578da3.png)

### 模型性能评估（使用随机森林）

```
# 使用不同特征集合训练模型
feature_sets = {
    'All Features': X.columns,
    'Mutual Info': selected_features_mi,
    'RFE': selected_features_rfe,
    'Lasso': selected_features_lasso
}

for name, features in feature_sets.items():
    X_train, X_test, y_train, y_test = train_test_split(
        X_scaled[:, [X.columns.get_loc(f) for f in features]], y, test_size=0.3, random_state=42)
    
    model = RandomForestClassifier(random_state=42)
    model.fit(X_train, y_train)
    y_pred = model.predict(X_test)
    acc = accuracy_score(y_test, y_pred)
    auc = roc_auc_score(y_test, model.predict_proba(X_test)[:, 1])
    
    print(f"【{name}】")
    print(f"Accuracy: {acc:.4f}, AUC: {auc:.4f}")
    print(classification_report(y_test, y_pred))
    print("-" * 60)
```

在减少特征数量的同时，我们观察模型的准确率和 `AUC` 指标是否下降，如果保持稳定或更好，说明特征选择有效。

### 算法优化建议

+ 使用集成选择策略，比如只选择多个方法共同提取的特征，提高鲁棒性  
+ 对特征选择过程加上交叉验证，避免过拟合  
+ 尝试增加更多非线性方法（如 `XGBoost`）进行嵌入式特征选择  
+ 应用 `PCA`（主成分分析）进行降维后比较性能  

## 模型分析

### 各个特征选择方法的优缺点



# 特征选择方法分析

我们在案例中使用了三种常见的特征选择方法：

+ `Filter`（过滤法）：以互信息为代表
+ `Wrapper`（包装法）：以递归特征消除 `RFE` 为代表
+ `Embedded`（嵌入法）：以 `Lasso` 回归为代表

下面从多个角度分析这三种方法在实际案例中的综合优缺点：

## 优点分析

+ 通用性强：三种方法基本覆盖了大多数特征选择需求，适用于分类问题中的各种模型结构（线性/非线性均可）。

+ 解释性好：每种方法都有明确的数学解释，例如互信息反映变量之间的非线性信息量、`Lasso`通过系数稀疏性体现特征重要性，方便特征意义理解。

+ 降低维度风险：通过删除低信息量或冗余的特征，不仅减少了模型训练的计算负担，同时降低了模型的过拟合风险。

+ 可组合优化：在案例中我们展示了多方法交叉选择特征的手段，这种“集成特征选择”思路在实战中非常实用，稳定性好。

## 缺点分析

+ 方法局限性明显：

+ `Filter`法没有考虑特征之间的交互关系，可能会保留冗余特征。

+ `Wrapper`法计算成本高，尤其在特征维度很大时效率低下。

+ `Embedded`方法（如`Lasso`）适用于线性模型，对于复杂非线性关系（如图像、NLP任务）效果会下降。

+ 对数据类型敏感：如互信息对于连续型变量需要离散化处理，否则评分不准；`RFE`对模型初选非常敏感；`Lasso`只适用于数值型特征。

+ 特征子集不稳定：在数据轻微变化时，`Wrapper` 和 `Lasso` 的选中特征可能不同，影响模型一致性。

+ 依赖模型假设：`Lasso` 假设特征与目标之间存在线性关系；如果真实关系是非线性的，则选择结果偏差较大。

## 与相似算法的对比

| 方法类别 | 算法 | 是否考虑特征间关系 | 是否依赖模型 | 优点 | 缺点 | 适用场景 |

| --- | --- | --- | --- | --- | --- | --- |

| `Filter` | 互信息（`MI`） | 否 | 否 | 计算快，不依赖模型，可解释性好 | 忽略特征间冗余性 | 快速预筛选，特征数较大时 |

| `Filter` | 卡方检验 / F检验 | 否 | 否 | 适用于离散变量，解释性强 | 不支持连续目标，变量需离散化 | 分类任务中特征初选 |

| `Wrapper` | `RFE` | 是 | 是 | 考虑特征组合效果，模型驱动 | 训练耗时长，过拟合风险高 | 特征少、关注性能优先时 |

| `Embedded` | `Lasso` | 否（部分） | 是 | 自动降维，嵌入模型中，效率高 | 线性模型假设，不适用于非线性任务 | 高维稀疏数据，线性回归场景 |

| `Embedded` | 决策树/随机森林重要性 | 是 | 是 | 适用于非线性，解释力强 | 可能对噪声敏感，重要性易变 | 非线性任务、结构化数据分析 |

| 降维方法（替代） | `PCA`（主成分分析） | 是（转换维度） | 否 | 无监督降维，消除多重共线性 | 不保留原始语义，解释性差 | 高维连续数据、可视化探索性分析 |

| 特征学习方法 | `AutoEncoder` / 深度网络 | 是（非线性） | 是 | 能处理复杂特征交互，高表达力 | 训练复杂，可解释性差 | 图像、语音、文本类非结构化任务 |

## 在什么情况下优选该类特征选择算法

### 应优先使用该类算法的场景

1. 结构化数据：如医疗、金融、电商等传统结构化数据集，有清晰的变量意义。

1. 中小特征集：特征在几十到几百维时，使用上述方法效率高、效果好。

1. 模型可解释性要求高：比如医疗诊断、信用评估等，需要解释“为什么选择这些变量”。

1. 需要模型精简部署：在嵌入式或边缘设备部署场景下，需要减少特征数量。

1. 早期探索性数据分析：用于快速筛选可疑变量，指导后续建模流程。

## 考虑其他算法的场景

应考虑替代算法的情况包括：

1. 特征维度极高（>10,000）：如文本TF-IDF特征，此时`Lasso`会收敛困难，`RFE`计算不可接受，建议用`PCA`或稀疏模型。

1. 特征间存在复杂非线性关系：如图像或语音识别任务，此时应考虑用深度学习（如`AutoEncoder`）进行端到端特征学习。

1. 特征含义不明确或冗余很强：如基因表达数据，应考虑用聚类或降维技术先压缩信息量。

1. 训练时间极为敏感：如大数据平台上线任务，应选择快速的`Filter`方法或预训练方式。

1. 模型不具备稀疏性优势：如基于集成的`XGBoost`等模型本身有特征筛选机制，不需要再手动选择。

## 总结

特征选择作为数据与处理方法中极为重要的一环，在模型性能提升、解释性增强、计算资源优化等方面都有不可替代的作用。本文通过具体案例演示了互信息、`RFE` 和 `Lasso` 三种方法的实际应用，辅以强视觉化手段，清晰展示了模型对特征的依赖程度。

然而，正如表格所述，每种方法都有局限性，在具体项目中应结合数据类型、模型结构、任务目标、部署环境等多重因素综合选择。没有“万能”的特征选择方法，只有最适合场景的策略。
