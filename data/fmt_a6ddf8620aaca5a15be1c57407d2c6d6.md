
# 从 1 维到 6 维，一文读懂多维数据可视化策略

选自 towardsdatascience

作者：Dipanjan Sarkar

机器之心编译

参与：Jane W、乾树、黄小天

> 数据聚合、汇总和可视化是支撑数据分析领域的三大支柱。长久以来，数据可视化都是一个强有力的工具，被业界广泛使用，却受限于 2 维。在本文中，作者将探索一些有效的多维数据可视化策略（范围从 1 维到 6 维）。

## 介绍

描述性分析（descriptive analytics）是任何分析生命周期的数据科学项目或特定研究的核心组成部分之一。数据聚合（aggregation）、汇总（summarization）和可视化（visualization）是支撑数据分析领域的主要支柱。从传统商业智能（Business Intelligence）开始，甚至到如今人工智能时代，数据可视化都是一个强有力的工具；由于其能有效抽取正确的信息，同时清楚容易地理解和解释结果，可视化被业界组织广泛使用。然而，处理多维数据集（通常具有 2 个以上属性）开始引起问题，因为我们的数据分析和通信的媒介通常限于 2 个维度。在本文中，我们将探索一些有效的多维数据可视化策略（范围从 1 维到 6 维）。

## 动机

> 「一图胜千言」

这是一句我们熟悉的非常流行的英语习语，可以充当将数据可视化作为分析的有效工具的灵感和动力。永远记住：「有效的数据可视化既是一门艺术，也是一门科学。」在开始之前，我还要提及下面一句非常相关的引言，它强调了数据可视化的必要性。

> 「一张图片的最大价值在于，它迫使我们注意到我们从未期望看到的东西。」

> ——John Tukey

## 快速回顾可视化

本文假设一般读者知道用于绘图和可视化数据的基本图表类型，因此这里不再赘述，但在本文随后的实践中，我们将会涉及大部分图表类型。著名的可视化先驱和统计学家 Edward Tufte 说过，数据可视化应该在数据的基础上，以清晰、精确和高效的方式传达数据模式和洞察信息。

结构化数据通常包括由行和特征表征的数据观测值或由列表征的数据属性。每列也可以被称为数据集的某特定维度。最常见的数据类型包括连续型数值数据和离散型分类数据。因此，任何数据可视化将基本上以散点图、直方图、箱线图等简单易懂的形式描述一个或多个数据属性。本文将涵盖单变量（1 维）和多变量（多维）数据可视化策略。这里将使用 Python 机器学习生态系统，我们建议先检查用于数据分析和可视化的框架，包括 `pandas`、`matplotlib`、`seaborn`、`plotly` 和 `bokeh`。除此之外，如果你有兴趣用数据制作精美而有意义的可视化文件，那么了解 `D3.js`（`https://d3js.org/`）也是必须的。有兴趣的读者可以阅读 Edward Tufte 的「The Visual Display of Quantitative Information」。

## 闲话至此，让我们来看看可视化（和代码）吧！

别在这儿谈论理论和概念了，让我们开始进入正题吧。我们将使用 UCI 机器学习库（`https://archive.ics.uci.edu/ml/index.php`）中的 Wine Quality Data Set。这些数据实际上是由两个数据集组成的，这两个数据集描述了葡萄牙「Vinho Verde」葡萄酒中红色和白色酒的各种成分。本文中的所有分析都在我的 `GitHub` 存储库中，你可以用 `Jupyter Notebook` 中的代码来尝试一下！

我们将首先加载以下必要的依赖包进行分析。

```py
import pandas as pd
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import matplotlib as mpl
import numpy as np
import seaborn as sns
%matplotlib inline
```

我们将主要使用 `matplotlib` 和 `seaborn` 作为我们的可视化框架，但你可以自由选择并尝试任何其它框架。首先进行基本的数据预处理步骤。

```py
white_wine = pd.read_csv('winequality-white.csv', sep=';')
red_wine = pd.read_csv('winequality-red.csv', sep=';')

# store wine type as an attribute
red_wine['wine_type'] = 'red'
white_wine['wine_type'] = 'white'

# bucket wine quality scores into qualitative quality labels
red_wine['quality_label'] = red_wine['quality'].apply(lambda value: 'low'
    if value <= 5 else 'medium'
    if value <= 7 else 'high')
red_wine['quality_label'] = pd.Categorical(red_wine['quality_label'],
    categories=['low', 'medium', 'high'])
white_wine['quality_label'] = white_wine['quality'].apply(lambda value: 'low'
    if value <= 5 else 'medium'
    if value <= 7 else 'high')
white_wine['quality_label'] = pd.Categorical(white_wine['quality_label'],
    categories=['low', 'medium', 'high'])

# merge red and white wine datasets
wines = pd.concat([red_wine, white_wine])

# re-shuffle records just to randomize data points
wines = wines.sample(frac=1, random_state=42).reset_index(drop=True)
```

我们通过合并有关红、白葡萄酒样本的数据集来创建单个葡萄酒数据框架。我们还根据葡萄酒样品的质量属性创建一个新的分类变量 `quality_label`。现在我们来看看数据前几行。

```py
wines.head()
```

![](img/c965d8e3d5c7e4e852ba19b06da6472d-fs8.png)

*葡萄酒质量数据集*

很明显，我们有几个葡萄酒样本的数值和分类属性。每个观测样本属于红葡萄酒或白葡萄酒样品，属性是从物理化学测试中测量和获得的特定属性或性质。如果你想了解每个属性（属性对应的变量名称一目了然）详细的解释，你可以查看 `Jupyter Notebook`。让我们快速对这些感兴趣的属性进行基本的描述性概括统计。

```py
subset_attributes = ['residual sugar', 'total sulfur dioxide', 'sulphates',
    'alcohol', 'volatile acidity', 'quality']
rs = round(red_wine[subset_attributes].describe(), 2)
ws = round(white_wine[subset_attributes].describe(), 2)

pd.concat([rs, ws], axis=1, keys=['Red Wine Statistics', 'White Wine Statistics'])
```

![](img/ef69389937260b886605f7672b5b702f-fs8.png)

*葡萄酒类型的基本描述性统计*

比较这些不同类型的葡萄酒样本的统计方法相当容易。注意一些属性的明显差异。稍后我们将在一些可视化中强调这些内容。

## 单变量分析

单变量分析基本上是数据分析或可视化的最简单形式，因为只关心分析一个数据属性或变量并将其可视化（1 维）。

### 可视化 1 维数据（1-D）

使所有数值数据及其分布可视化的最快、最有效的方法之一是利用 `pandas` 画直方图（histogram）。

```py
wines.hist(bins=15, color='steelblue', edgecolor='black', linewidth=1.0,
    xlabelsize=8, ylabelsize=8, grid=False)
plt.tight_layout(rect=(0, 0, 1.2, 1.2))
```

![](img/ca2151be425d9d5bc5d654b77c026b1d-fs8.png)

*将属性作为 1 维数据可视化*

上图给出了可视化任何属性的基本数据分布的一个好主意。

让我们进一步可视化其中一个连续型数值属性。直方图或核密度图能够很好地帮助理解该属性数据的分布。

```py
# Histogram
fig = plt.figure(figsize=(6, 4))
title = fig.suptitle("Sulphates Content in Wine", fontsize=14)
fig.subplots_adjust(top=0.85, wspace=0.3)

ax = fig.add_subplot(1, 1, 1)
ax.set_xlabel("Sulphates")
ax.set_ylabel("Frequency")
ax.text(1.2, 800, r'$\mu$=' + str(round(wines['sulphates'].mean(), 2)),
    fontsize=12)
freq, bins, patches = ax.hist(wines['sulphates'], color='steelblue', bins=15,
    edgecolor='black', linewidth=1)

# Density Plot
fig = plt.figure(figsize=(6, 4))
title = fig.suptitle("Sulphates Content in Wine", fontsize=14)
fig.subplots_adjust(top=0.85, wspace=0.3)

ax1 = fig.add_subplot(1, 1, 1)
ax1.set_xlabel("Sulphates")
ax1.set_ylabel("Frequency")
sns.kdeplot(wines['sulphates'], ax=ax1, shade=True, color='steelblue')
```

![](img/e0f3fb14fe20bbe7ba336ae47a4227cc-fs8.png)

*可视化 1 维连续型数值数据*

从上面的图表中可以看出，葡萄酒中硫酸盐的分布存在明显的右偏（right skew）。

可视化一个离散分类型数据属性稍有不同，条形图是（bar plot）最有效的方法之一。你也可以使用饼图（pie-chart），但一般来说要尽量避免，尤其是当不同类别的数量超过 3 个时。

```py
# Histogram
fig = plt.figure(figsize=(6, 4))
title = fig.suptitle("Sulphates Content in Wine", fontsize=14)
fig.subplots_adjust(top=0.85, wspace=0.3)

ax = fig.add_subplot(1, 1, 1)
ax.set_xlabel("Sulphates")
ax.set_ylabel("Frequency")
ax.text(1.2, 800, r'$\mu$=' + str(round(wines['sulphates'].mean(), 2)),
    fontsize=12)
freq, bins, patches = ax.hist(wines['sulphates'], color='steelblue', bins=15,
    edgecolor='black', linewidth=1)

# Density Plot
fig = plt.figure(figsize=(6, 4))
title = fig.suptitle("Sulphates Content in Wine", fontsize=14)
fig.subplots_adjust(top=0.85, wspace=0.3)

ax1 = fig.add_subplot(1, 1, 1)
ax1.set_xlabel("Sulphates")
ax1.set_ylabel("Frequency")
sns.kdeplot(wines['sulphates'], ax=ax1, shade=True, color='steelblue')
```

![](img/1f8f292850300a22f0f1d49efdcf60bd-fs8.png)

*可视化 1 维离散分类型数据*

现在我们继续分析更高维的数据。

## 多变量分析

多元分析才是真正有意思并且有复杂性的领域。这里我们分析多个数据维度或属性（2 个或更多）。多变量分析不仅包括检查分布，还包括这些属性之间的潜在关系、模式和相关性。你也可以根据需要解决的问题，利用推断统计（inferential statistics）和假设检验，检查不同属性、群体等的统计显著性（significance）。

### 可视化 2 维数据（2-D）

检查不同数据属性之间的潜在关系或相关性的最佳方法之一是利用配对相关性矩阵（pair-wise correlation matrix）并将其可视化为热力图。

```py
# Correlation Matrix Heatmap
f, ax = plt.subplots(figsize=(10, 6))
corr = wines.corr()
hm = sns.heatmap(round(corr, 2), annot=True, ax=ax, cmap="coolwarm", fmt='.2f',
    linewidths=.05)
f.subplots_adjust(top=0.93)
t = f.suptitle('Wine Attributes Correlation Heatmap', fontsize=14)
```

![](img/554edb686012df9736a4f6047a71cfc4-fs8.png)

*用相关性热力图可视化 2 维数据*

热力图中的梯度根据相关性的强度而变化，你可以很容易发现彼此之间具有强相关性的潜在属性。另一种可视化的方法是在感兴趣的属性之间使用配对散点图。

```py
# Correlation Matrix Heatmap
f, ax = plt.subplots(figsize=(10, 6))
corr = wines.corr()
hm = sns.heatmap(round(corr, 2), annot=True, ax=ax, cmap="coolwarm", fmt='.2f',
    linewidths=.05)
f.subplots_adjust(top=0.93)
t = f.suptitle('Wine Attributes Correlation Heatmap', fontsize=14)
```

![](img/640026ae0170c56114abab0f0bf73486-fs8.png)

*用配对散点图可视化 2 维数据*

根据上图，可以看到散点图也是观察数据属性的 2 维潜在关系或模式的有效方式。另一种将多元数据可视化为多个属性的方法是使用平行坐标图。

```py
# Correlation Matrix Heatmap
f, ax = plt.subplots(figsize=(10, 6))
corr = wines.corr()
hm = sns.heatmap(round(corr, 2), annot=True, ax=ax, cmap="coolwarm", fmt='.2f',
    linewidths=.05)
f.subplots_adjust(top=0.93)
t = f.suptitle('Wine Attributes Correlation Heatmap', fontsize=14)
```

![](img/2ba7dd624bfde20cfc6e6a71b8418ad5-fs8.png)

*用平行坐标图可视化多维数据*

基本上，在如上所述的可视化中，点被表征为连接的线段。每条垂直线代表一个数据属性。所有属性中的一组完整的连接线段表征一个数据点。因此，趋于同一类的点将会更加接近。仅仅通过观察就可以清楚看到，与白葡萄酒相比，红葡萄酒的密度略高。与红葡萄酒相比，白葡萄酒的残糖和二氧化硫总量也较高，红葡萄酒的固定酸度高于白葡萄酒。查一下我们之前得到的统计表中的统计数据，看看能否验证这个假设！
