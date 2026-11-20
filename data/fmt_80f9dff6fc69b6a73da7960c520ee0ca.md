
因此，让我们看看可视化两个连续型数值属性的方法。散点图和联合分布图（joint plot）是检查模式、关系以及属性分布的特别好的方法。

```py
# Scatter Plot
plt.scatter(wines['sulphates'], wines['alcohol'],
            alpha=0.4, edgecolors='w')

plt.xlabel('Sulphates')
plt.ylabel('Alcohol')
plt.title('Wine Sulphates - Alcohol Content',y=1.05)

# Joint Plot
jp = sns.jointplot(x='sulphates', y='alcohol', data=wines,
                   kind='reg', space=0, size=5, ratio=4) 
```

![](img/7f25d024d9c7a8b01c5f8816579449e7-fs8.png)

*使用散点图和联合分布图可视化 2 维连续型数值数据*

散点图在上图左侧，联合分布图在右侧。就像我们提到的那样，你可以查看联合分布图中的相关性、关系以及分布。

如何可视化两个连续型数值属性？一种方法是为分类维度画单独的图（子图）或分面（facet）。

```py
# Using subplots or facets along with Bar Plots
fig = plt.figure(figsize = (10, 4))
title = fig.suptitle("Wine Type - Quality", fontsize=14)
fig.subplots_adjust(top=0.85, wspace=0.3)
# red wine - wine quality
ax1 = fig.add_subplot(1,2, 1)
ax1.set_title("Red Wine")
ax1.set_xlabel("Quality")
ax1.set_ylabel("Frequency") 
rw_q = red_wine['quality'].value_counts()
rw_q = (list(rw_q.index), list(rw_q.values))
ax1.set_ylim([0, 2500])
ax1.tick_params(axis='both', which='major', labelsize=8.5)
bar1 = ax1.bar(rw_q[0], rw_q[1], color='red', 
               edgecolor='black', linewidth=1)

# white wine - wine quality
ax2 = fig.add_subplot(1,2, 2)
ax2.set_title("White Wine")
ax2.set_xlabel("Quality")
ax2.set_ylabel("Frequency") 
ww_q = white_wine['quality'].value_counts()
ww_q = (list(ww_q.index), list(ww_q.values))
ax2.set_ylim([0, 2500])
ax2.tick_params(axis='both', which='major', labelsize=8.5)
bar2 = ax2.bar(ww_q[0], ww_q[1], color='white', 
               edgecolor='black', linewidth=1) 
```

![](img/91f091266e608bfe15e29b1ab9f6a92a-fs8.png)

*使用条形图和子图可视化 2 维离散型分类数据*

虽然这是一种可视化分类数据的好方法，但正如所见，利用 matplotlib 需要编写大量的代码。另一个好方法是在单个图中为不同的属性画堆积条形图或多个条形图。可以很容易地利用 seaborn 做到。

```py
# Multi-bar Plot
cp = sns.countplot(x="quality", hue="wine_type", data=wines, 
                   palette={"red": "#FF9999", "white": "#FFE888"}) 
```

![](img/3f8d9849964997f68f1271b94e7e18c0-fs8.png)

*在一个条形图中可视化 2 维离散型分类数据*

这看起来更清晰，你也可以有效地从单个图中比较不同的类别。

让我们看看可视化 2 维混合属性（大多数兼有数值和分类）。一种方法是使用分图\子图与直方图或核密度图。

```py
# facets with histograms
fig = plt.figure(figsize = (10,4))
title = fig.suptitle("Sulphates Content in Wine", fontsize=14)
fig.subplots_adjust(top=0.85, wspace=0.3)

ax1 = fig.add_subplot(1,2, 1)
ax1.set_title("Red Wine")
ax1.set_xlabel("Sulphates")
ax1.set_ylabel("Frequency") 
ax1.set_ylim([0, 1200])
ax1.text(1.2, 800, r'$\mu$='+str(round(red_wine['sulphates'].mean(),2)), 
         fontsize=12)
r_freq, r_bins, r_patches = ax1.hist(red_wine['sulphates'], color='red', bins=15,
                                   edgecolor='black', linewidth=1)

ax2 = fig.add_subplot(1,2, 2)
ax2.set_title("White Wine")
ax2.set_xlabel("Sulphates")
ax2.set_ylabel("Frequency")
ax2.set_ylim([0, 1200])
ax2.text(0.8, 800, r'$\mu$='+str(round(white_wine['sulphates'].mean(),2)), 
         fontsize=12)
w_freq, w_bins, w_patches = ax2.hist(white_wine['sulphates'], color='white', bins=15,
                                   edgecolor='black', linewidth=1)

# facets with density plots
fig = plt.figure(figsize = (10, 4))
title = fig.suptitle("Sulphates Content in Wine", fontsize=14)
fig.subplots_adjust(top=0.85, wspace=0.3)

ax1 = fig.add_subplot(1,2, 1)
ax1.set_title("Red Wine")
ax1.set_xlabel("Sulphates")
ax1.set_ylabel("Density") 
sns.kdeplot(red_wine['sulphates'], ax=ax1, shade=True, color='r')

ax2 = fig.add_subplot(1,2, 2)
ax2.set_title("White Wine")
ax2.set_xlabel("Sulphates")
ax2.set_ylabel("Density") 
sns.kdeplot(white_wine['sulphates'], ax=ax2, shade=True, color='y') 
```

![](img/b19df0eb778445b774e287fcf30807e6-fs8.png)

*利用分面和直方图\核密度图可视化 2 维混合属性*

虽然这很好，但是我们再一次编写了大量代码，我们可以通过利用 seaborn 避免这些，在单个图表中画出这些图。

```py
# Using multiple Histograms 
fig = plt.figure(figsize = (6, 4))
title = fig.suptitle("Sulphates Content in Wine", fontsize=14)
fig.subplots_adjust(top=0.85, wspace=0.3)
ax = fig.add_subplot(1,1, 1)
ax.set_xlabel("Sulphates")
ax.set_ylabel("Frequency") 

g = sns.FacetGrid(wines, hue='wine_type', palette={"red": "r", "white": "y"})
g.map(sns.distplot, 'sulphates', kde=False, bins=15, ax=ax)
ax.legend(title='Wine Type')
plt.close(2) 
```

![](img/6212b8743e7b24f2dae71b90d0696f8a-fs8.png)

*利用多维直方图可视化 2 维混合属性*

可以看到上面生成的图形清晰简洁，我们可以轻松地比较各种分布。除此之外，箱线图（box plot）是根据分类属性中的不同数值有效描述数值数据组的另一种方法。箱线图是了解数据中四分位数值以及潜在异常值的好方法。

```py
# Box Plots
f, (ax) = plt.subplots(1, 1, figsize=(12, 4))
f.suptitle('Wine Quality - Alcohol Content', fontsize=14)

sns.boxplot(x="quality", y="alcohol", data=wines,  ax=ax)
ax.set_xlabel("Wine Quality",size = 12,alpha=0.8)
ax.set_ylabel("Wine Alcohol %",size = 12,alpha=0.8) 
```

![](img/859cad0c13908b25304827bd75ad4dcb-fs8.png)

*2 维混合属性的有效可视化方法——箱线图*

另一个类似的可视化是小提琴图，这是使用核密度图显示分组数值数据的另一种有效方法（描绘了数据在不同值下的概率密度）。

```py
# Violin Plots
f, (ax) = plt.subplots(1, 1, figsize=(12, 4))
f.suptitle('Wine Quality - Sulphates Content', fontsize=14)

sns.violinplot(x="quality", y="sulphates", data=wines,  ax=ax)
ax.set_xlabel("Wine Quality",size = 12,alpha=0.8)
ax.set_ylabel("Wine Sulphates",size = 12,alpha=0.8) 
```

![](img/b55c899ee2a9b5660b5ff0d0608fed9f-fs8.png)

*2 维混合属性的有效可视化方法——小提琴图*

你可以清楚看到上面的不同酒品质类别的葡萄酒硫酸盐的密度图。

将 2 维数据可视化非常简单直接，但是随着维数（属性）数量的增加，数据开始变得复杂。原因是因为我们受到显示媒介和环境的双重约束。

对于 3 维数据，可以通过在图表中采用 z 轴或利用子图和分面来引入深度的虚拟坐标。

但是，对于 3 维以上的数据来说，更难以直观地表征。高于 3 维的最好方法是使用图分面、颜色、形状、大小、深度等等。你还可以使用时间作为维度，为随时间变化的属性制作一段动画（这里时间是数据中的维度）。看看 Hans Roslin 的精彩演讲就会获得相同的想法！

## 可视化 3 维数据（3-D）

这里研究有 3 个属性或维度的数据，我们可以通过考虑配对散点图并引入颜色或色调将分类维度中的值分离出来。

```py
# Scatter Plot with Hue for visualizing data in 3-D
cols = ['density', 'residual sugar', 'total sulfur dioxide', 'fixed acidity', 'wine_type']
pp = sns.pairplot(wines[cols], hue='wine_type', size=1.8, aspect=1.8, 
                  palette={"red": "#FF9999", "white": "#FFE888"},
                  plot_kws=dict(edgecolor="black", linewidth=0.5))
fig = pp.fig 
fig.subplots_adjust(top=0.93, wspace=0.3)
t = fig.suptitle('Wine Attributes Pairwise Plots', fontsize=14) 
```

![](img/720942467744681bde03ed932a5766a2-fs8.png)

*用散点图和色调（颜色）可视化 3 维数据*

上图可以查看相关性和模式，也可以比较葡萄酒组。就像我们可以清楚地看到白葡萄酒的总二氧化硫和残糖比红葡萄酒高。

让我们来看看可视化 3 个连续型数值属性的策略。一种方法是将 2 个维度表征为常规长度（x 轴）和宽度（y 轴）并且将第 3 维表征为深度（z 轴）的概念。

```py
# Visualizing 3-D numeric data with Scatter Plots
# length, breadth and depth
fig = plt.figure(figsize=(8, 6))
ax = fig.add_subplot(111, projection='3d')

xs = wines['residual sugar']
ys = wines['fixed acidity']
zs = wines['alcohol']
ax.scatter(xs, ys, zs, s=50, alpha=0.6, edgecolors='w')

ax.set_xlabel('Residual Sugar')
ax.set_ylabel('Fixed Acidity')
ax.set_zlabel('Alcohol') 
```

![](img/b0c1b770115e9e196c59cc21135570a6-fs8.png)

*通过引入深度的概念来可视化 3 维数值数据*

我们还可以利用常规的 2 维坐标轴，并将尺寸大小的概念作为第 3 维（本质上是气泡图），其中点的尺寸大小表征第 3 维的数量。

```py
# Visualizing 3-D numeric data with a bubble chart
# length, breadth and size
plt.scatter(wines['fixed acidity'], wines['alcohol'], s=wines['residual sugar']*25, 
            alpha=0.4, edgecolors='w')

plt.xlabel('Fixed Acidity')
plt.ylabel('Alcohol')
plt.title('Wine Alcohol Content - Fixed Acidity - Residual Sugar',y=1.05) 
```

![](img/d5bd1cde14714a73ac9d1942fea007bc-fs8.png)

*通过引入尺寸大小的概念来可视化 3 维数值数据*

因此，你可以看到上面的图表不是一个传统的散点图，而是点（气泡）大小基于不同残糖量的的气泡图。当然，并不总像这种情况可以发现数据明确的模式，我们看到其它两个维度的大小也不同。

为了可视化 3 个离散型分类属性，我们可以使用常规的条形图，可以利用色调的概念以及分面或子图表征额外的第 3 个维度。seaborn 框架帮助我们最大程度地减少代码，并高效地绘图。

```py
# Visualizing 3-D categorical data using bar plots
# leveraging the concepts of hue and facets
fc = sns.factorplot(x="quality", hue="wine_type", col="quality_label", 
                    data=wines, kind="count",
                    palette={"red": "#FF9999", "white": "#FFE888"}) 
```

![](img/3f3b415aba1b9e115d2e86858cf6d135-fs8.png)

*通过引入色调和分面的概念可视化 3 维分类数据*

上面的图表清楚地显示了与每个维度相关的频率，可以看到，通过图表能够容易有效地理解相关内容。

考虑到可视化 3 维混合属性，我们可以使用色调的概念来将其中一个分类属性可视化，同时使用传统的如散点图来可视化数值属性的 2 个维度。
