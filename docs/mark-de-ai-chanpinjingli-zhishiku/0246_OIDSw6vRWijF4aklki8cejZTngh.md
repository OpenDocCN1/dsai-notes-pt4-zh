# 12-08机器学习模型评估方法-召回率

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Kxw6dyvcqo2LcCxofMAcAXoUnLd](https://qqs7y1hozd1.feishu.cn/docx/Kxw6dyvcqo2LcCxofMAcAXoUnLd)

### 召回率是干嘛的？

召回率就是用来衡量模型有没有漏掉重要目标的比例。

假**如你在**找丢了的**小猫（**正类），你找了100只猫，实际有20只是你家猫，模型帮你找回来其中15只。那么你就找回来 15 / 20 = 75%，这就是召回率！

### 举个简单的例子：

| 猫编号 | 实际是你家的猫吗？ | 模型说是你家的猫吗？ |
| --- | --- | --- |
| 1 | 是 | 是 |
| 2 | 是 | 是 |
| 3 | 是 | 否 ❌ |
| 4 | 是 | 是 |
| 5 | 是 | 否 ❌ |
| ... | ... | ... |

假设总共有 10 只你家猫（正类），你模型只找对了其中 6 只（预测对的正类），那召回率就是：

👉 召回率 = 6 / 10 = 60%

你找回来了 60% 的丢猫，虽然不一定找得很准，但找回来的猫尽量多。

**召回率（Recall）是指在所有真实为正**类的样本中，被模型正确识别为正类的比例。

也就是说，它衡量的是你有没有把该找的目标都找出来了。

## 数学原理

### 四种分类结果（混淆矩阵）

在分类任务中，预测结果和真实标签可能组合成以下 4 种情况：

|  | **真实为正类** | **真实为负类** |
| --- | --- | --- |
| **预测为正类** | TP（真阳性） | FP（假阳性） |
| **预测为负类** | FN（假阴性） | TN（真阴性） |

+   TP（True Positive）：模型预测正类，实际也为正类

+   **FN（Fal**se Negative）：模型预测负类，但实际是正类 → 漏掉了目标！

+   **FP（Fal**se Positive）：模型预测正类，实际是负类 → 错抓了无关的

+   TN（True Negative）：模型预测负类，实际也是负类

### 召回率公式

即：

意思就是，实际中应该被发现的正类样本中，模型到底发现了多少个。

### 举例推导公式

假设你在做肿瘤检测（正类 = 有肿瘤），你有以下检测结果：

+   检测正确地找出肿瘤的病人（TP）：80人

+   **漏掉了实际有肿瘤的人（FN）**：20人

+   **所以，总共有肿瘤的病人**：100人（80 + 20）

召回率就是：

说明你成功识别出了 80% 的有肿瘤病人。

## 召回率的作用和价值

召回率特别重要的场景是：**不能漏掉任何重要目标！**

### 常见场景

+   **医学诊断（肿瘤检测）**：宁可误判一些没病的，也不能漏掉一个有病的。

+   **垃圾邮件检测**：宁可多拦几个正常邮件，也不能放过一封垃圾邮件。

+   **安检识别武器**：宁可误报普通物品，也不能漏掉危险品。

这种时候，**召回率越高越好**，宁可牺牲一点准确性，也要尽量“全找回来”。

## 召回率的评估流程

**步骤如下：**

1.  **模型预测结果输出**：得到所有样本的预测标签（比如预测有没有肿瘤）。

1.  **与真实标签对比，构造混淆矩阵**：统计出 TP、FN（召回率只用这两个）。

1.  **计算公式**

1.  **多分类时怎么办？**

+   可以对每一类都算召回率，叫每类召回率（per-class recall）

+   再求个平均，叫宏平均召回率（Macro Recall）

## 完整案例

假设我们正在做**医疗疾病诊断**任务，预测某种罕见疾病的患者是否患病。数据集比较复杂，正负样本比例不平衡，正**类（患**病）样本较少。我们用机器学习模型预测患者是否患病，评估模型效果时，重点关注召回率，确保尽可能多地发现患病患者。

我们使用一个模拟数据集：

+   1000个样本

+   20个特征（数值型）

+   正类比例约5%（即50个患病患者，950个健康人）

### 实现思路

+   使用scikit-learn生成模拟数据

+   用RandomForestClassifier模型进行分类

+   评估指标包括准确率、精确率、召回率、F1分数

+   重点展示召回率计算及影响

+   绘制混淆矩阵和PR曲线（Precision-Recall曲线）直观表现

+   讲解每一步代码及分析结果

```
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import make_classification
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import (confusion_matrix, classification_report, 
                             precision_recall_curve, average_precision_score)
from sklearn.model_selection import train_test_split

# Step 1: 生成不平衡的模拟数据
X, y = make_classification(n_samples=2000, n_features=20, n_informative=5, 
                           n_redundant=2, n_clusters_per_class=1,
                           weights=[0.95, 0.05], flip_y=0, random_state=42)

print(f&quot;正类样本数（患病患者）：{sum(y==1)}&quot;)
print(f&quot;负类样本数（健康人）：{sum(y==0)}&quot;)

# Step 2: 划分训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42, stratify=y)

# Step 3: 训练随机森林模型
clf = RandomForestClassifier(n_estimators=100, random_state=42)
clf.fit(X_train, y_train)

# Step 4: 预测测试集结果
y_pred = clf.predict(X_test)
y_proba = clf.predict_proba(X_test)[:, 1]  # 预测为正类的概率，用于绘制PR曲线

# Step 5: 计算各种评估指标
cm = confusion_matrix(y_test, y_pred)
report = classification_report(y_test, y_pred, digits=4)

# 计算召回率
TP = cm[1, 1]
FN = cm[1, 0]
recall = TP / (TP + FN)

print(&quot;\n混淆矩阵&quot;)
print(cm)

print(&quot;\n 分类报告&quot;)
print(report)

print(f&quot;手动计算召回率: {recall:.4f}&quot;)

# Step 6: 绘制混淆矩阵可视化
plt.figure(figsize=(6, 5))
plt.imshow(cm, interpolation=&#x27;nearest&#x27;, cmap=plt.cm.Oranges)
plt.title(&#x27;Confusion Matrix&#x27;)
plt.colorbar()
tick_marks = np.arange(2)
plt.xticks(tick_marks, [&#x27;Healthy(0)&#x27;, &#x27;Diseased(1)&#x27;])
plt.yticks(tick_marks, [&#x27;Healthy(0)&#x27;, &#x27;Diseased(1)&#x27;])

# 标注数字
thresh = cm.max() / 2
for i in range(2):
    for j in range(2):
        plt.text(j, i, format(cm[i, j], &#x27;d&#x27;),
                 horizontalalignment=&quot;center&quot;,
                 color=&quot;white&quot; if cm[i, j] &gt; thresh else &quot;black&quot;,
                 fontsize=16)

plt.ylabel(&#x27;True label&#x27;)
plt.xlabel(&#x27;Predicted label&#x27;)
plt.tight_layout()

# Step 7: 绘制Precision-Recall曲线
precision, recall_vals, thresholds = precision_recall_curve(y_test, y_proba)
average_precision = average_precision_score(y_test, y_proba)

plt.figure(figsize=(8, 6))
plt.plot(recall_vals, precision, color=&#x27;darkorange&#x27;, lw=2,
         label=f&#x27;Precision-Recall curve (AP = {average_precision:.2f})&#x27;)
plt.fill_between(recall_vals, precision, alpha=0.2, color=&#x27;orange&#x27;)
plt.xlabel(&#x27;Recall&#x27;, fontsize=14)
plt.ylabel(&#x27;Precision&#x27;, fontsize=14)
plt.title(&#x27;Precision-Recall Curve&#x27;, fontsize=16)
plt.legend(loc=&quot;lower left&quot;)
plt.grid(True)
plt.show()
```

**步骤 1: 生成不平衡数据集**：使用make_classification创建一个带标签的二分类数据集，设置weights=[0.95, 0.05]来制造严重类别不平衡（5%的正类），这模拟了现实中疾病样本往往少于健康样本的情况。

**步骤 2: 划分训练和测试集**：使用train_test_split将数据分成70%训练，30%测试，参数stratify=y确保训练集和测试集中的正负样本比例保持一致。

**步骤 3: 训练随机森林模型**：使用RandomForestClassifier，较为稳健且适合处理复杂非线性数据，训练模型，拟合训练集数据。

**步骤 4: 预测结果**：预测测试集标签：y_pred，获取正类概率：y_proba，后续绘制PR曲线用。

**步骤 5: 计算指标和混淆矩阵**：混淆矩阵展示四类情况，重点看TP和FN，使用classification_report打印详细指标，包括召回率、精确率、F1等，手动计算召回率，验证一致性。

**步骤 6: 混淆矩阵可视化**：在每个格子中显示数字，方便直观理解分类正确/错误的数量。

![](img/fff2268e93fad9e8fbf7e5b086d6512a.png)

步骤 7: Precision-Recall曲线绘制：PR曲线适合不平衡数据，展示阈值变化时精确率和召回率的关系，曲线下面积（AP）衡量整体表现，越接近1越好，从图中可以直观发现，召回率提升时，精确率如何变化。

![](img/7605156b152fa855d7c409e4b0fc72d9.png)

## 分析召回率的优缺点

**优点：**

+   强调“漏掉率”，尤其适合“漏一例可能后果严重”的应用场景

+   简单直观，能快速告诉你模型有没有把该发现的正类样本找出来

+   与精确率结合使用，可以更全面评估模型性能（F1分数）

+   在不平衡数据集中，召回率比准确率更有指导意义，因为准确率可能因为负类数量太多而失真

**缺点：**

+   只关注是否找回所有正样本，不关心“找出来的有多少是错的”，忽略了假阳性（FP）

+   如果只追求召回率，模型容易“放水”，大量预测为正，导致假阳性暴增，降低精确率

+   不适合对精度要求极高的场景，误报代价较大时单独看召回率有风险

+   无法反映模型对负类样本的分类能力，易导致评估片面

## 召回率何时是优选指标？

召回率作为评估指标，在以下场景中尤其重要：

+   **医疗诊断中漏诊不可接受**：如癌症筛查，宁可多检查几次也不能漏掉患者

+   **安检和安全检测**：不能漏掉潜在危险品，误报可容忍

+   **欺诈检测**：尽可能找出所有欺诈交易，虽然会带来一些误报，但保障安全

+   **信息检索和推荐系统**：确保用户获得尽可能多的相关结果，减少遗漏

总的来说，当“漏检成本远高于误检成本”时，召回率是首选评估标准。

## 总结

召回率是机器学习模型评估中不可或缺的指标，尤其在高风险漏检场景中具有无可替代的价值。然而，必须结合其他指标（如精确率、F1分数）综合考量。