
# 04-06概率分布-二项分布

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Njn3dXj5aoWxGHxD9t7c77r5nef](https://qqs7y1hozd1.feishu.cn/docx/Njn3dXj5aoWxGHxD9t7c77r5nef)

**二项分布**（Binomial Distribution）是一种离散概率分布，描述了在   次独立的伯努利试验中成功的次数。每次试验中只有两种可能结果：成功（success）或失败（failure），成功的概率为  ，失败的概率为  。

## 二项分布的性质

### 参数

（试验次数）和  （成功的概率）。

### 概率质量函数

二项分布的概率质量函数（PMF）定义如下：

其中：

表示从   次试验中取出   次成功的组合数。

### 期望值和方差

### 应用场景

1. 医疗试验中记录某种治疗的成功次数。
1. 产品质量检测中，记录某批次产品的合格品数量。
1. A/B 测试中，记录点击广告的用户数量。

## 完整案例

使用 Python 代码来生成虚拟数据集，绘制二项分布的概率质量函数（PMF），并结合数据分析可视化解释其意义。

### 案例1：广告点击率

假设一个在线广告的点击率  ，广告被展示给   个用户。我们想分析这组数据中广告被点击的次数分布。

```
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import binom

# Parameters
n = 10  # Number of trials
p = 0.3  # Probability of success

# Generate binomial distribution
x = np.arange(0, n+1)
pmf = binom.pmf(x, n, p)

# Plotting PMF
plt.figure(figsize=(10, 6))
plt.bar(x, pmf, color=&#x27;skyblue&#x27;, edgecolor=&#x27;black&#x27;, label=&#x27;PMF&#x27;)
plt.title(&#x27;Binomial Distribution PMF (n=10, p=0.3)&#x27;, fontsize=16)
plt.xlabel(&#x27;Number of Successes (k)&#x27;, fontsize=14)
plt.ylabel(&#x27;Probability&#x27;, fontsize=14)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.legend(fontsize=12)
plt.grid(axis=&#x27;y&#x27;, linestyle=&#x27;--&#x27;, alpha=0.7)
plt.show()
```

![](img/fe7065fc9e6d3f01f88dc0cb6f776ef0.png)

图中显示了  ,   条件下二项分布的概率质量函数。横轴为点击广告的次数（成功次数），纵轴为对应概率。概率分布的高峰在  ，表明广告被点击 3 次的概率最高，符合  。

### 案例2：质量检测

假设某工厂生产的零件有 5% 的概率是次品。从 50 个零件中随机抽取一批，记录次品的数量分布。

```
# Parameters
n = 50  # Number of trials
p = 0.05  # Probability of success

# Generate binomial distribution
x = np.arange(0, n+1)
pmf = binom.pmf(x, n, p)

# Plotting PMF
plt.figure(figsize=(12, 8))
plt.bar(x, pmf, color=&#x27;salmon&#x27;, edgecolor=&#x27;black&#x27;, label=&#x27;PMF&#x27;)
plt.title(&#x27;Binomial Distribution PMF (n=50, p=0.05)&#x27;, fontsize=16)
plt.xlabel(&#x27;Number of Defective Parts (k)&#x27;, fontsize=14)
plt.ylabel(&#x27;Probability&#x27;, fontsize=14)
plt.xlim(0, 15)  # Focus on the range of interest
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.legend(fontsize=12)
plt.grid(axis=&#x27;y&#x27;, linestyle=&#x27;--&#x27;, alpha=0.7)
plt.show()
```

![](img/7bc3e798de71b22ca54a4ed698414d26.png)

图中显示了   条件下的次品数量分布。大部分概率集中在   和   附近，表明次品数量很少。这种分布适用于分析高质量生产工艺的性能。

## 数据分析图的进一步探索

我们通过模拟和数据分析，生成虚拟数据集并探索其特性。

### 虚拟数据集模拟

```
# Simulate random samples
np.random.seed(42)
samples = binom.rvs(n=50, p=0.05, size=1000)

# Plot histogram of simulated data
plt.figure(figsize=(12, 6))
plt.hist(samples, bins=np.arange(-0.5, 15.5, 1), color=&#x27;purple&#x27;, edgecolor=&#x27;black&#x27;, alpha=0.7)
plt.title(&#x27;Histogram of Simulated Binomial Data&#x27;, fontsize=16)
plt.xlabel(&#x27;Number of Defective Parts (k)&#x27;, fontsize=14)
plt.ylabel(&#x27;Frequency&#x27;, fontsize=14)
plt.grid(axis=&#x27;y&#x27;, linestyle=&#x27;--&#x27;, alpha=0.7)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.show()
```

![](img/4ca08198187278ab6525c41a9a866d40.png)

直方图展示了 1000 次模拟数据的分布情况，与理论 PMF 分布高度一致。这说明模拟数据能够真实反映二项分布的特点。

### Cumulative Distribution Function (CDF) 可视化

```
# Generate CDF
cdf = binom.cdf(x, n, p)

# Plotting CDF
plt.figure(figsize=(10, 6))
plt.step(x, cdf, where=&#x27;mid&#x27;, color=&#x27;green&#x27;, label=&#x27;CDF&#x27;)
plt.fill_between(x, cdf, step=&#x27;mid&#x27;, alpha=0.3, color=&#x27;green&#x27;)
plt.title(&#x27;Binomial Distribution CDF (n=50, p=0.05)&#x27;, fontsize=16)
plt.xlabel(&#x27;Number of Defective Parts (k)&#x2, p=0.05)&#x27;, fontsize=16)
plt.xlabel(&#x27;Number of Defective Parts (k)&#x27;, fontsize=14)
plt.ylabel(&#x27;Cumulative Probability&#x27;, fontsize=14)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.legend(fontsize=12)
plt.grid(axis=&#x27;both&#x27;, linestyle=&#x27;--&#x27;, alpha=0.7)
plt.show()
```

![](img/53a0a4eaa7cdfa2730e7a246bead8910.png)

CDF 图显示了次品数量分布的累积概率。

例如，  表示次品数量不超过 2 的概率，容易从图中读取直观数值。

## 问题与讨论

通过以上分析，我们回答了以下问题：

1. 事件成功的期望值与分布规律是什么？

通过 PMF 和模拟数据，明确了成功事件的高频区域和分布趋势。

1. **罕见事件的概率如何计算？**

CDF 提供了快速评估罕见事件（如  ）的工具。

1. 如何通过仿真验证分布模型的准确性？

使用随机生成数据对理论模型进行验证，确保分布的合理性。
