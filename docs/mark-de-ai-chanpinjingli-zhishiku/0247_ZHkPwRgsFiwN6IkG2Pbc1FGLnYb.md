# 12-09机器学习模型评估方法-准确率

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LsN0dGxowoYitDxL4lWcfJRJnag](https://qqs7y1hozd1.feishu.cn/docx/LsN0dGxowoYitDxL4lWcfJRJnag)

在机器学习中，我们经常训练一个模型去「分类」，比如判断邮件是不是垃圾邮件、判断照片里是不是猫、预测某人是否会买产品等。

训练完模型之后，我们就要问自己一个问题：

**“这个模型到底准不准？”**

这时就需要一个「评价指标」来衡量模型的表现，「准确率」就是最简单也最常用的一个。

> 准确率 = 模型预测对的次数 / 总的预测次数

打个比方：

你用模型预测了100封邮件，最后发现其中有90封判断得对（比如判断成垃圾邮件的确是垃圾邮件，判断成正常邮件的也是正常邮件），那你的准确率就是：

> 准确率 = 90 / 100 = 90%

这就是准确率的核心思想。

## 核心逻辑

在二分类问题中，我们常用一个混淆矩阵（Confusion Matrix）来整理预测的结果：

| 实际 \ 预测 | 预测为正（Positive） | 预测为负（Negative） |
| --- | --- | --- |
| 实际为正 | TP（真正例） | FN（假负例） |
| 实际为负 | FP（假正例） | TN（真负例） |

解释一下每个术语：

+   TP（True Positive）：模型预测是正的，实际上也是正的（判断成“猫”，的确是“猫”）

+   TN（True Negative）：模型预测是负的，实际上也是负的（判断成“不是猫”，真的“不是猫”）

+   FP（False Positive）：模型预测是正的，实际上是负的（误判）

+   FN（False Negative）：模型预测是负的，实际上是正的（漏判）

**准确率的数学公式：**

也就是：

> 准确率 = 所有预测对的数量 / 总预测样本数

这个公式说明：只要预测对了，不管是正还是负，我们都加到分子上；而总数永远是所有预测的总量。

## 准确率的计算流程

以下是一个准确率计算的标准流程，适用于二分类或多分类问题：

**步骤一 准备数据**：

+   有一个测试集（包含真实标签）

+   模型已经训练好，准备对测试集做预测

**步骤二 模型预测**：

+   对测试集中的每个样本，使用模型预测一个类别

**步骤三 比较预测值与真实值**：

+   统计预测结果：

+   有多少个预测对了（预测值 = 真实值）

+   有多少个预测错了

**步骤四 计算准确率**

**举例流程：**

假设我们有10个测试样本，真实标签是：

| 样本编号 | 真实标签 | 预测标签 |
| --- | --- | --- |
| 1 | 正 | 正 |
| 2 | 正 | 正 |
| 3 | 负 | 正 |
| 4 | 负 | 负 |
| 5 | 正 | 正 |
| 6 | 负 | 负 |
| 7 | 正 | 负 |
| 8 | 负 | 负 |
| 9 | 负 | 正 |
| 10 | 正 | 正 |

我们可以得到：

+   TP = 4（预测成正且真实为正的是样本 1、2、5、10）

+   TN = 3（预测成负且真实为负的是样本 4、6、8）

+   FP = 2（预测成正但真实为负的是样本 3、9）

+   FN = 1（预测成负但真实为正的是样本 7）

套入公式：

## 完整案例

**在这里，咱们使用一个经典的数据集：乳腺癌**肿瘤预测（Breast Cancer Wisconsin Dataset），构建一个机器学习分类模型用于预测肿瘤的良恶性，并使用准确率对模型的性能进行深入评估。

同时，我们使用图像可视化方式辅助分析，并在最后系统性地讨论准确率的优缺点与适用情境。

### 数据集

数据来自 sklearn.datasets 中的乳腺癌数据集，具体如下：

+   样本数：569 条肿瘤样本

+   特征数：30 个（肿瘤大小、对称性、光滑度等数值型变量）

+   目标变量：

+   0：恶性（Malignant）

+   1：良性（Benign）

这是一个标准的二分类问题，数据具有轻度类别不平衡。

### 步骤

**依次完成以下步骤：**

1.  加载与探索数据

1.  数据预处理：标准化与训练测试集划分

1.  模型训练（逻辑回归）

1.  使用准确率进行模型评估

1.  可视化分析模型预测效果

1.  分析准确率优缺点与适用情境

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, confusion_matrix, ConfusionMatrixDisplay

# 加载数据
data = load_breast_cancer()
X = pd.DataFrame(data.data, columns=data.feature_names)
y = pd.Series(data.target)  # 0=恶性，1=良性

print(f&quot;数据维度: {X.shape}&quot;)
print(&quot;类别分布：&quot;)
print(y.value_counts())

# 数据标准化与划分
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42, stratify=y)

# 模型训练
model = LogisticRegression(max_iter=10000)
model.fit(X_train, y_train)

# 预测与准确率评估
y_pred = model.predict(X_test)
acc = accuracy_score(y_test, y_pred)
print(f&quot;\n模型准确率为：{acc:.4f}&quot;)

# 混淆矩阵可视化
cm = confusion_matrix(y_test, y_pred)
disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=data.target_names)
disp.plot(cmap=&quot;plasma&quot;)
plt.title(&quot;预测结果混淆矩阵&quot;)
plt.show()

# 可视化预测正确与否分布
correct = y_test == y_pred
result_df = pd.DataFrame({
    &#x27;真实标签&#x27;: y_test.values,
    &#x27;预测标签&#x27;: y_pred,
    &#x27;预测是否正确&#x27;: correct
})

sns.countplot(data=result_df, x=&#x27;预测是否正确&#x27;, palette=&#x27;Set2&#x27;)
plt.title(&quot;预测是否正确的样本数量&quot;)
plt.xlabel(&quot;是否正确&quot;)
plt.ylabel(&quot;样本数量&quot;)
plt.show()
```

#### 第一步：数据加载与初步探索

使用 load_breast_cancer() 加载乳腺癌数据集，获取特征矩阵 X 和标签向量 y。

可以看到：

+   类别 1（良性）：357 条

+   类别 0（恶性）：212 条

虽然不是完全平衡，但大致均衡，可使用准确率作为评估指标的基础。

#### 第二步：数据标准化与划分

使用 StandardScaler() 将所有特征标准化，使其符合标准正态分布（均值为 0，标准差为 1）。

然后使用 train_test_split() 将数据划分为训练集（80%）和测试集（20%）。

关键参数：

+   stratify=y：确保训练集和测试集中，正负样本比例一致。

+   random_state=42：确保实验可复现。

#### 第三步：逻辑回归模型训练

逻辑回归是二分类中最基础、最易理解的模型，其输出是每个类别的概率值，默认 0.5 作为判断阈值。

我们设置 max_iter=10000，确保模型能够充分收敛。

#### 第四步：准确率评估

准确率计算公式如下：

对应代码：

```
acc = accuracy_score(y_test, y_pred)
```

假设模型预测结果为 96.49%，说明在 114 个测试样本中有大约 110 个预测正确。

#### 第五步：可视化分析

##### 混淆矩阵可视化

![](img/e4707e75006d5be8350a77d3fa009276.png)

混淆矩阵告诉我们：

+   有多少个恶性肿瘤被正确预测为恶性（True Negative）

+   有多少个良性被错判为恶性（False Positive）

##### 正确与错误预测样本分布图

用 Seaborn 画出模型预测正确与否的样本数：

```
sns.countplot(data=result_df, x=&#x27;预测是否正确&#x27;, palette=&#x27;Set2&#x27;)
```

![](img/d7f65dc4950e7a100577fc1635c742cf.png)

直观地看到模型是大部分预测正确，少数错误。

### 准确率的优缺点分析

---

**优势**：

1.  **简单直观**：易于理解与计算，尤其对非技术人员友好。

1.  **适用于样本均衡问题**：若正负样本比例相近，准确率非常有效。

1.  **快速模型评估**：可作为第一层筛选标准，衡量模型大体效果。

**局限性**：

1.  **对类别不平衡极度不敏感**：假如数据中 95% 是良性肿瘤，那么即使模型全预测为良性，也有 95% 的准确率，却没有任何实质性判断能力。

1.  **无法区分错误类型**：假阳性（将良性错判为恶性） vs 假阴性（将恶性漏判为良性）。在医疗诊断中，假阴性比假阳性更危险，但准确率不会反映这种代价。

1.  **不能衡量模型泛化能力**：一个训练集准确率高但测试集准确率低的模型，可能过拟合了，而准确率不会直接揭示这一问题。

### 在什么情况下优先使用准确率？

**推荐使用准确率的场景：**

+   正负类别比例大致均衡（如情感分析、一般性分类问题）

+   错误分类不会造成严重后果（如电影推荐系统）

+   多模型对比时的初步筛选指标

**不推荐单独使用准确率的场景：**

+   类别极度不平衡（如欺诈检测、罕见疾病诊断）

+   错误类型代价严重不对等（如癌症漏诊的假阴性）

+   模型需要精细调优和风险控制的场合（如信用评分、风控）

**准确率虽然简单，但在合适的场景下是快速且有效的评估方法**。要始终应牢记：准确率不能孤立使用，特别是在高风险与不平衡场景下，必须配合更细致的指标综合判断。