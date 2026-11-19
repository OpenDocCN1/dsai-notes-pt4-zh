
# 12-02机器学习模型评估方法-F1 分数

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/XrIadgBjtolLZAxCG5mcx8K3niV](https://qqs7y1hozd1.feishu.cn/docx/XrIadgBjtolLZAxCG5mcx8K3niV)

在机器学习中，尤其是分类问题（比如判断一封邮件是不是垃圾邮件），我们训练了一个模型后，得知道它表现好不好。

`F1 分数`就是一个非常重要的评估标准，它综合考虑了两个重要因素：

+   `精确率（Precision）`：你说是的里面，真的是的有多少？

+   `召回率（Recall）`：真正的是里，你说出来了多少？

`F1 分数`就是一个平衡这两方面的综合评分。

比如说在医院，如果一个模型精确率特别高但召回率特别低，那说明你很少误诊，但很多真正的病人你没发现。

而 `F1 分数` 能帮你综合这两个指标，判断整体好不好。

## F1 分数的数学原理和公式推理

### 1. 四个基础概念：混淆矩阵四项

在理解 `F1 分数` 前，我们得先知道以下四个术语，它们来自模型预测结果和真实值的对比：

| | 实际为正（Positive） | 实际为负（Negative） |
| --- | --- | --- |
| **预测为正** | `TP`（真正） | `FP`（假正） |
| **预测为负** | `FN`（假负） | `TN`（真负） |

+   `TP`（True Positive）：模型预测是正类，实际也是正类。

+   `FP`（False Positive）：模型预测是正类，但实际是负类（冤枉了）。

+   `FN`（False Negative）：模型预测是负类，但实际是正类（漏掉了）。

+   `TN`（True Negative）：模型预测是负类，实际也是负类。

### 2. 精确率（Precision）的公式

表示模型说是的里面，有多少是真的。

如果你预测了 10 个癌症患者，其中 7 个是真的（`TP`），3 个是误诊（`FP`），那你的精确率是：

### 3. 召回率（Recall）的公式

表示真正的是里面，有多少被你说出来了。

如果现实中有 12 个癌症患者，而你只找出了 7 个（`TP`），漏掉了 5 个（`FN`），你的召回率是：

## F1 分数的核心公式

### 为什么要有 F1 分数？

如果一个模型只看精确率，不顾召回率，或者只顾召回率，忽视精确率，那评估不全面。我们需要一个平衡这两个指标的指标，这就是 `F1 分数`。

### `F1 分数` 公式：

这是`调和平均数`（不是普通平均数！）。

#### 举个例子：

如果精确率是 0.8，召回率是 0.6，那 `F1` 是：

注意：调和平均强调「两个数都得高」，如果一个很低，`F1 分数` 也会很低。

## 数学公式推导

### 从精确率和召回率出发：

+   `Precision = TP / (TP + FP)`

+   `Recall = TP / (TP + FN)`

我们将这两个代入 `F1` 的公式：

这个公式看起来复杂，但它本质是：

这个是最简化的形式，直接体现了 `TP`、`FP` 和 `FN` 三者的关系。

## F1 分数的算法评估流程

### 步骤 1：获得预测结果

你有一组真实的标签（例如：是否是垃圾邮件），还有一组模型预测的标签。

### 步骤 2：统计 `TP`、`FP`、`FN`、`TN`

构建混淆矩阵，数出：

+   有多少真正（`TP`）

+   有多少假正（`FP`）

+   有多少假负（`FN`）

### 步骤 3：计算精确率和召回率

用上面两个公式：

+   `Precision = TP / (TP + FP)`

+   `Recall = TP / (TP + FN)`

### 步骤 4：计算 `F1 分数`

或者简化形式：

### 步骤 5：分析结果

+   如果 `F1 分数` 接近 1，说明模型表现很好。

+   如果 `F1 分数` 很低（比如 < 0.5），说明模型在某方面严重失衡。

## 完整案例

我们使用一个经典的文本分类问题作为实际案例：垃圾邮件识别。

这个问题属于二分类问题：判断一封邮件是“垃圾邮件（`spam`）”还是“正常邮件（`ham`）”。

> 数据集在👸号：深夜努力写`Python`，后台回复「数据集」既可获取~

这个场景很贴切 `F1 分数` 的应用，因为：

+   垃圾邮件通常是少数类（正类），而正常邮件是多数类（负类）；

+   如果你把所有邮件都预测成正常的，准确率可能还挺高，但没啥意义；

+   所以需要综合考虑「预测准确性」和「漏掉垃圾邮件的严重性」，这正是 `F1 分数` 能解决的！

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import f1_score, classification_report, confusion_matrix


# 1. 加载数据（UCI Spam）
url = "sms.tsv"
df = pd.read_csv(url, sep='\t', header=None, names=['label', 'message'])

# 2. 数据预处理
df['label'] = df['label'].map({'ham': 0, 'spam': 1})  # ham=0，spam=1

# 3. 文本向量化（TF-IDF）
vectorizer = TfidfVectorizer(stop_words='english', max_features=3000)
X = vectorizer.fit_transform(df['message'])
y = df['label']

# 4. 划分训练集与测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 5. 建立模型并训练（使用朴素贝叶斯）
model = MultinomialNB()
model.fit(X_train, y_train)

# 6. 模型预测与评估
y_pred = model.predict(X_test)

# 7. 打印 F1 分数
f1 = f1_score(y_test, y_pred)
print(f"F1 分数：{f1:.4f}")

# 8. 分类报告
print("\n分类报告（Classification Report）:")
print(classification_report(y_test, y_pred))

# 9. 混淆矩阵可视化
cm = confusion_matrix(y_test, y_pred)
sns.heatmap(cm, annot=True, fmt='d', cmap="YlGnBu", xticklabels=["Ham", "Spam"], yticklabels=["Ham", "Spam"])
plt.title("混淆矩阵 (Confusion Matrix)")
plt.xlabel("预测")
plt.ylabel("实际")
plt.show()
```

### 步骤 1：导入数据集

数据源是一个公开的短信数据集，每行一条短信，包括标签和内容：

```
df = pd.read_csv(url, sep='\t', header=None, names=['label', 'message'])
```

其中：

+   `label` 是目标变量，有两个值：`ham`（正常）和 `spam`（垃圾）；

+   `message` 是短信的内容。

### 步骤 2：标签处理

我们把标签转换成数字格式，便于机器学习模型处理：

```
df['label'] = df['label'].map({'ham': 0, 'spam': 1})
```

+   0 表示正常短信（负类）

+   1 表示垃圾短信（正类）

### 步骤 3：文本向量化（TF-IDF）

自然语言中的词是文字，不能直接输入模型。我们用 `TF-IDF` 方法把每条短信转成数值向量。

```
vectorizer = TfidfVectorizer(stop_words='english', max_features=3000)
```

+   去除英文停用词（如 `"the"`, `"is"`）

+   限制最大特征数为 3000，防止维度过高

`TF-IDF` 本质：词频 × 逆文档频率，强调有代表性的词。

### 步骤 4：划分数据集

我们把数据按 80/20 划分成训练集和测试集：

```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
```

训练集用于学习，测试集用于检验。

### 步骤 5：选择模型（朴素贝叶斯）

文本分类问题，朴素贝叶斯是常见首选，尤其适合处理词频型特征：

```
model = MultinomialNB()
model.fit(X_train, y_train)
```

模型学习词之间的联合概率，建立垃圾与非垃圾的概率模型。

### 步骤 6：模型预测与评估

```
y_pred = model.predict(X_test)
```

用模型预测测试集短信是否为垃圾。

### 步骤 7：`F1 分数` 评估

```
f1 = f1_score(y_test, y_pred)
```

`F1 分数` 越高，说明模型在“找准”与“找全”两个方面表现越好。

### 步骤 8：分类报告

```
print(classification_report(y_test, y_pred))
```

包含精确率、召回率、`F1 分数`、支持度（样本数）。

输出：

```
              precision    recall  f1-score   support

           0       0.98      1.00      0.99       966
           1       1.00      0.90      0.95       149

    accuracy                           0.99      1115
   macro avg       0.99      0.95      0.97      1115
weighted avg       0.99      0.99      0.99      1115
```

可见：`spam` 类的 `F1 分数` 为 0.90，说明分类表现良好，但还有提升空间。

### 步骤 9：混淆矩阵可视化

```
sns.heatmap(cm, annot=True, fmt='d', cmap="YlGnBu")
```

可以快速看到：

![](img/5be3017a73daad9e234b011e1ba98e82.png)

+   预测错的有多少？

+   哪一类误判更多？

结果：

| | 预测 `ham` | 预测 `spam` |
| --- | --- | --- |
| 实际 `ham` | 966 | 0 |
| 实际 `spam` | 15 | 134 |

我们看到，垃圾邮件漏掉了 18 封（`FN`），误判了 8 封正常邮件为垃圾（`FP`），这些对 `F1 分数` 有重要影响。

## F1 分数的优缺点

**优点：**

1.  **平衡评估两个维度**：`F1 分数` 考虑了误判和漏判，不会偏向任何一方，在任务中更“公平”。

1.  **适用于不平衡分类问题**：在 `spam` 分类、欺诈识别、疾病检测等场景下，正负样本分布极不均衡时，`F1 分数` 仍能正确评估模型好坏。

1.  **对极端错误敏感**：如果你的模型有严重的漏判或误判，`F1 分数` 会迅速降低，警示你优化模型。

1.  **更贴近实际场景关注点**：现实业务往往不只追求准确率，而是希望“尽量不漏掉关键类”，`F1 分数` 正适用于此。

**缺点：**

1.  **忽略真负类（`TN`）**：`F1` 不考虑真正类（即正常预测正确的“非垃圾邮件”），所以它不能反映总体准确度。

1.  无法单独分析 `Precision` 和 `Recall` 的失衡原因：`F1 分数` 是综合值，如果低了，我们不知道是精确率低，还是召回率低，必须单独分析。

1.  **对业务优先级无法调整**：某些场景下我们可能更关注 `Recall`（不要漏判），但 `F1` 默认两者同等重要，无法权衡业务优先级。

1.  **不适用于多类分类的直接比较**：`F1 分数` 在多分类时需要加权平均，计算方式变复杂，解释性也下降。

## F1 分数适用场景讨论：什么时候是首选？

**适用情况：**

1.  **类别不平衡**的分类任务（比如 `spam` 检测、欺诈检测、癌症识别）；

1.  **错误代价不均衡**时（如漏判一个癌症患者比误诊更严重）；

1.  **对模型鲁棒性要求高**，需要防止模型只关注准确率却忽略实际应用价值；

1.  任务目标是综合考察模型对正类检测能力，而不仅仅是整体准确。

**不适用情况：**

+   当样本非常均衡、误判和漏判成本相当时，`accuracy`（准确率）或 `ROC-AUC` 可能更简单直观；

+   当你希望显式地优先优化 `Precision` 或 `Recall` 时，`F1 分数` 可能会掩盖细节；

+   当你有丰富负类分析需求时，`F1` 会忽略真负类信息。

通过实际输出的混淆矩阵、分类报告，我们可以看到，`F1 分数`能很好反映模型的分类准确性与全面性；

能警惕我们对小类漏判问题的忽视，在不平衡分类问题中，远比“准确率”更可靠。
