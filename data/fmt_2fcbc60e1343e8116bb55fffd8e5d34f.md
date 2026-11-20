
# 03-07集成算法-极端随机森林

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/WPHrdrFiWoSMCsxE5f4cHg9knHC](https://qqs7y1hozd1.feishu.cn/docx/WPHrdrFiWoSMCsxE5f4cHg9knHC)

`极端随机森林`，全名叫 `Extremely Randomized Trees`，简称 `Extra Trees`，它是一个机器学习中的集成算法，用来做分类（比如判断邮件是不是垃圾邮件）或者回归（比如预测房价）的问题。

它跟`随机森林`很像，只不过它更「随机」，决策更「极端」一点。

当你要做一个重要决定，比如选哪部电影看。

你可能会：

+ 问问几个朋友的意见
+ 看看网上评分
+ 再考虑自己的喜好

这就像是你用`多个意见综合判断`，而不是`靠一个人说了算`。机器学习中的集成算法，就是这个`思路` - 把很多个模型的结果合在一起，来得出更准确的预测。

`极端随机森林`是由很多棵决策树组成的一片「森林」，这些树一起投票、做决定。

跟`随机森林`的区别是：

#### 1. 切分方式更随机

在建每棵树时，`随机森林`会找一个最好的切法来分数据，比如在哪个年龄点把人群分成两组最合理。

而`极端随机森林`会直接随机选一个点切一刀，不去计算哪一刀最优。

这种「随便切」的方式虽然看起来粗暴，但反而降低了过拟合（就是在训练集上表现很好，但在新数据上很差），让模型更有泛化能力。

#### 2. 多树多数决

每一棵树会给出一个自己的预测结果（比如“这封邮件是垃圾邮件”），最后所有树投票，得出最终答案。

就像一群人投票决定看哪部电影，大多数人说《疯狂星期五》，那我们就看这部。

跟随机森林有啥不同？

| 项目 | 随机森林 | 极端随机森林 |
| --- | --- | --- |
| 特征选择 | 随机选一部分特征，然后找最优切分点 | 随机选特征+随机选切分点 |
| 偏差 | 稍微低一点 | 稍微高一点 |
| 方差 | 稍微高一点 | 更低，稳定性更好 |
| 训练速度 | 慢一些 | 更快，因为不找“最佳”切分点 |

一句话总结：`极端随机森林`就是一群"随机切菜"的决策树，虽然它们每一棵树都不是最聪明的，但靠数量多、方向广，投票起来还挺靠谱。

## 原理详解

### 整体思想

用多个完全随机构建的决策树组成一个模型，每棵树的预测都不太靠谱，但整体平均（或投票）后会非常强大。

相比`随机森林`，它进一步增加了切分点选择上的随机性，从而进一步减少过拟合，降低模型方差。

设我们有一个数据集：

+ 输入特征： ，其中
+ 标签：
+ 样本数：
+ 特征维度：

我们要学得一个模型   来逼近真实函数  ，使得在新样本上的期望误差最小。

### Extra Trees 的数学建模与推理

#### 1. 集成学习框架

`Extra Trees` 属于并行集成方法（不像`Boosting`是串行的），假设我们训练了   棵树，每棵树的输出为  ，则最终模型为：

+ 对于回归问题（平均输出）：
+ 对于分类问题（多数投票）：

其中   是指示函数，表示是否投给类别  。

#### 2. 单棵决策树的构建

与传统`CART`树（`Classification And Regression Tree`）不同，`Extra Trees`在构造每个内部节点时采用以下两个极端随机化策略：

#### 策略一：随机选择特征子集

+ 对每个节点，随机选出   个特征，而不是使用所有特征。
+ 通常  （分类） 或  （回归）

#### 策略二：对每个特征，随机选取切分点

+ 对于选出的每一个特征  ：
+ 不再去寻找使目标函数（如`Gini`、`信息增益`、`MSE`）最优的切分点。
+ 而是从该特征在当前节点的数据中的取值范围   上均匀随机采样一个切分点
+ 然后计算这些随机切分点的划分质量，选出效果最好的那个特征 + 切分点组合。

#### 3. 划分质量评估

虽然切分点是随机生成的，但最终我们还是会从这些随机切分中选出一个划分效果最好的。

常见划分标准：

对于分类问题：使用`基尼指数`（`Gini Index`）

+ 当前节点 `impurity`:
+ 其中   是当前节点中类别   的样本比例。
+ 划分后的 `impurity gain`:
+ 选择使   最大的切分。

对于回归问题：使用`均方误差（MSE）`

+ 当前节点 `MSE`:
+ 划分后的误差下降量：

最终在随机采样的切分点中，选择让`impurity reduction`（如`Gini`或`MSE`）最大的组合。

### Extra Trees 的完整算法流程

1. 输入数据集，设定树的数量，每个节点随机选取特征数，以及最小样本数、最大深度等参数。
2. 对于每棵树：
   a. 不进行`bootstrap`采样（默认使用整个训练集）
   b. 从根节点开始递归构建树：
   + 对当前节点，若样本数 < 最小阈值或`impurity`很小，则停止切分，设为叶子节点。
   + 随机选取   个特征
   + 对每个特征  ：
   + 从特征取值范围中随机采样一个切分点
   + 计算所有候选切分点对应的`impurity reduction`，选出最佳切分特征和切分点。
   + 用这个切分点将当前样本划分为左右子节点
   + 对左右子节点递归进行以上步骤
3. 所有棵树构建完成，组成一个`Extra Trees`模型
4. 预测阶段：
   + 对于输入  ，将其输入所有   棵树，得到每棵树的预测
   + 对于分类：多数投票
   + 对于回归：取平均值

### 偏差-方差

`Extra Trees`通过更强的随机性，来达到降低模型方差的目标。

| 项 | `Extra Trees` | `Random Forest` |
| --- | --- | --- |
| 偏差 | 稍高 | 稍低 |
| 方差 | 更低（更稳定） | 稍高（更容易过拟合） |

这是因为：`Extra Trees`不进行`bootstrap`，更加稳定，切分点完全随机，降低模型复杂度和过拟合风险。

## 完整案例

我们选用`sklearn`自带的乳腺癌（`Breast Cancer`）数据集，进行数据探索、预处理、模型训练、模型评估、特征重要性分析以及超参数优化。

### 数据导入与探索性数据分析

加载乳腺癌数据集。数据集包含 569 个样本和 30 个特征，每个样本对应一个二分类标签（良性或恶性）。

我们将对数据集进行初步统计分析，如分布、相关性热力图等。

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split, GridSearchCV, learning_curve, cross_val_score
from sklearn.ensemble import ExtraTreesClassifier
from sklearn.metrics import confusion_matrix, classification_report, roc_curve, auc, accuracy_score
import warnings
warnings.filterwarnings("ignore")


# 加载数据集
data = load_breast_cancer()
X = pd.DataFrame(data.data, columns=data.feature_names)
y = pd.Series(data.target, name='target')

# 数据集基本信息展示
print("数据集样本数：", X.shape[0])
print("数据集特征数：", X.shape[1])
print("目标变量类别：", np.unique(y))
print("\n数据描述：\n", X.describe())
```

+ `数据维度`：本数据集包含 569 个样本，每个样本 30 个特征，目标变量为二分类（0：恶性，1：良性）。
+ `统计描述`：通过`describe()`方法，我们能查看各特征的均值、标准差、最小值、最大值等统计信息。
+ `相关性分析`：后续将通过热力图分析各特征间的相关性，寻找高相关性特征，为特征选择或降维提供依据。

接下来，绘制部分特征的分布图以及特征相关性热力图，便于直观理解数据分布情况和变量之间的相互关系。

```
# 绘制部分特征的分布情况
plt.figure(figsize=(12, 8))
features_to_plot = ['mean radius', 'mean texture', 'mean perimeter', 'mean area']
for feature in features_to_plot:
    sns.kdeplot(X[feature], shade=True, label=feature)
plt.title("Distribution of Selected Features", fontsize=16)
plt.xlabel("Feature Values", fontsize=14)
plt.ylabel("Density", fontsize=14)
plt.legend()
plt.show()

# 绘制特征相关性热力图
plt.figure(figsize=(14, 10))
corr = X.corr()
sns.heatmap(corr, annot=False, cmap='coolwarm', linewidths=0.5)
plt.title("Feature Correlation Heatmap", fontsize=18)
plt.show()
```

![](img/2d41ee2a6f197ba532554091429c6db9.png)

1. `分布图`：显示了选定特征的概率密度分布，帮助我们判断数据是否存在偏态、异常值或多峰分布现象。
1. `相关性热力图`：展示了不同特征之间的线性相关性。相关性高的特征可能存在冗余信息，对模型构建时可考虑降维或特征选择，减少过拟合风险。

![](img/72a6d4e7a7f1d0a7b54ccac35273cae8.png)

### 数据预处理与划分

在进行模型训练前，需要对数据集进行划分，将数据分为训练集和测试集。这里我们使用 70% 的数据作为训练集，30% 的数据作为测试集。

此外，对于某些模型（如基于距离度量的模型）有必要对数据进行标准化，但在树模型中不强制要求标准化处理。

```
# 数据划分：70% 训练集，30% 测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42, stratify=y)
print("训练集样本数：", X_train.shape[0])
print("测试集样本数：", X_test.shape[0])
```

+ `数据划分`：将数据划分为训练集和测试集可以有效评估模型的泛化能力，防止模型在训练数据上过拟合。
+ `stratify`参数：保持类别分布一致，有助于平衡各类别比例，确保模型训练和测试数据分布相似。

### 模型训练 - Extra Trees Classifier

接下来，我们构造极端随机森林分类器，并训练模型。`Extra Trees`在构建过程中对特征选择和划分阈值进行随机采样，因此比传统决策树具有更高的鲁棒性和泛化能力。

这里我们先采用默认参数训练模型，并对其进行初步评估。

```
# 构建并训练 Extra Trees 分类器
etc = ExtraTreesClassifier(random_state=42)
etc.fit(X_train, y_train)

# 在测试集上进行预测
y_pred = etc.predict(X_test)
```

#### 模型初步评估

我们使用混淆矩阵、分类报告以及准确率等指标来评估模型性能。

```
# 输出分类报告和准确率
print("Classification Report:\n", classification_report(y_test, y_pred))
print("Accuracy:", accuracy_score(y_test, y_pred))

# 绘制混淆矩阵热力图
conf_mat = confusion_matrix(y_test, y_pred)
plt.figure(figsize=(8, 6))
sns.heatmap(conf_mat, annot=True, fmt='d', cmap='viridis', cbar=True)
plt.xlabel("Predicted Label", fontsize=14)
plt.ylabel("True Label", fontsize=14)
plt.title("Confusion Matrix", fontsize=16)
plt.show()
```

![](img/2f89c4efd52073ec83d0e8699e5a6bcd.png)

+ `分类报告`：详细展示了每个类别的`precision`、`recall`和`F1`分数，帮助我们了解模型在不同类别上的表现。
+ `混淆矩阵`：通过混淆矩阵，我们能直观了解模型的预测错误分布，判断是否存在偏向某一类别的情况。
+ `准确率`：整体反映模型在测试集上的预测准确性，是评估分类模型的重要指标之一。

### ROC 曲线与 AUC 分析

为了进一步评估模型在不同阈值下的分类性能，我们绘制`ROC`曲线并计算`AUC`（曲线下面积）。

`ROC`曲线展示了在不同分类阈值下模型的真阳性率（`TPR`）和假阳性率（`FPR`），`AUC`值则衡量了模型总体的判别能力。

```
# 计算预测概率
y_proba = etc.predict_proba(X_test)[:, 1]
fpr, tpr, thresholds = roc_curve(y_test, y_proba)
roc_auc = auc(fpr, tpr)

# 绘制 ROC 曲线
plt.figure(figsize=(10, 8))
plt.plot(fpr, tpr, color='darkorange', lw=2, label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
plt.xlabel('False Positive Rate', fontsize=14)
plt.ylabel('True Positive Rate', fontsize=14)
plt.title('Receiver Operating Characteristic (ROC)', fontsize=16)
plt.legend(loc="lower right")
plt.show()
```

+ `ROC 曲线`：通过绘制`ROC`曲线，我们可以了解模型在不同阈值下的敏感性和特异性。
+ `AUC 值`：`AUC`值越接近 1，模型的判别能力越强；本例中若`AUC`值较高，则说明模型对乳腺癌良恶性分类较为准确。

### 学习曲线分析

学习曲线展示了模型在训练集和交叉验证集上的性能随训练样本数变化的情况，有助于判断模型是否存在欠拟合或过拟合现象，以及是否需要增加数据量。
