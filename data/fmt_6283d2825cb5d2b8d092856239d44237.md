
# 04-12概率分布-正态分布

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/MDxGdgi6LoJb1YxMlCwceEJCn3d](https://qqs7y1hozd1.feishu.cn/docx/MDxGdgi6LoJb1YxMlCwceEJCn3d)

正态分布，也称为高斯分布，是统计学中最常见和最重要的概率分布之一。广泛应用于自然科学、社会科学、经济学、工程学等多个领域。

## 直观解释

正态分布可以被看作是许多随机变量的平均值的分布。

举一个非常简单的例子：假设我们测量一大群人的身高，虽然每个人的身高不同，但如果我们画出所有身高的分布图，通常会发现大部分人的身高集中在一个平均值附近，越接近这个平均值的人越多，而身高偏离平均值越远的人越少。

这种分布呈现出一个对称的钟形曲线，这就是正态分布。

正态分布的本质是描述大量独立随机变量的和（或平均值）的分布特征，它具有以下几个特点：

1. 对称性：正态分布关于其均值对称。
1. 均值、中位数、众数相等：对于正态分布，均值（`mean`）、中位数（`median`）和众数（`mode`）是相同的。
1. 钟形曲线：分布图呈钟形，曲线的形状由均值和标准差决定。
1. 68-95-99.7规则：在正态分布中，大约68%的数据位于均值的一个标准差范围内，95%的数据位于两个标准差范围内，99.7%的数据位于三个标准差范围内。

正态分布的形状由均值（μ）和标准差（σ）决定，使用Python生成并绘制正态分布的图形。

```
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# 生成正态分布数据
mu, sigma = 0, 1  # 均值为0，标准差为1
data = np.random.normal(mu, sigma, 1000)

# 绘制正态分布图形
sns.set(style="whitegrid")
plt.figure(figsize=(8, 6))
sns.histplot(data, kde=True, color="blue", bins=30)
plt.title("Normal Distribution (mean=0, std=1)")
plt.xlabel("Value")
plt.ylabel("Frequency")
plt.show()
```

上面的代码生成了一个均值为0，标准差为1的正态分布数据，并绘制了它的分布图。图形中，曲线展示了数据的密度分布，呈现出经典的钟形。

## 公式推导

正态分布的概率密度函数（PDF）由以下公式给出：

其中：

是均值，决定分布的中心位置。

是标准差，决定分布的宽度。

### 推导细节

1. 正态分布的公式来源于对独立同分布的随机变量的和的极限分布的分析（即中央极限定理）。
1. 在推导过程中，通过极限分析及特定函数的傅里叶变换，最终得出具有指数形式的概率密度函数。
1. 公式中的分母部分确保了函数的标准化，即整个概率密度函数的积分为1。

## Python实现

### 生成正态分布数据

```
import numpy as np

# 设置均值和标准差
mu, sigma = 10, 2

# 生成1000个正态分布的数据
data = np.random.normal(mu, sigma, 1000)

# 输出前10个数据
print(data[:10])
```

### 可视化正态分布

```
import matplotlib.pyplot as plt
import seaborn as sns

# 绘制数据的分布
sns.set(style="whitegrid")
plt.figure(figsize=(8, 6))
sns.histplot(data, kde=True, color="green", bins=30)
plt.title("Normal Distribution (mean=10, std=2)")
plt.xlabel("Value")
plt.ylabel("Frequency")
plt.show()
```

![](img/2ecd088b58b79085590032b5540c177b.png)

### 机器学习中的应用

在机器学习中，正态分布被广泛应用于数据建模和特征工程。例如，在朴素贝叶斯分类器中，假设特征符合正态分布，可以使用正态分布的概率密度函数来计算后验概率。

```
from sklearn.datasets import make_classification
from sklearn.naive_bayes import GaussianNB
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# 生成一个简单的二分类数据集
X, y = make_classification(n_samples=1000, n_features=2, n_informative=2, n_redundant=0, random_state=42)

# 分割数据为训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# 使用高斯朴素贝叶斯模型
model = GaussianNB()
model.fit(X_train, y_train)

# 预测
y_pred = model.predict(X_test)

# 输出准确率
accuracy = accuracy_score(y_test, y_pred)
print(f"Model Accuracy: {accuracy * 100:.2f}%")
```

这个代码演示了如何使用Python和scikit-learn库应用高斯朴素贝叶斯模型。这个模型假设数据的各个特征符合正态分布，在实际的分类任务中表现良好。

最后和大家总结一句：正态分布是一个非常重要的概率分布，一定要理解其中的核心逻辑和意义。
