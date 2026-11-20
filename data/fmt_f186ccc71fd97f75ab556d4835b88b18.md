
# Chapter 4 支持向量机

> 来源：[https://datawhaler.feishu.cn/docx/TtQfdFzQtomyYSxbKxOcRRT9nQc](https://datawhaler.feishu.cn/docx/TtQfdFzQtomyYSxbKxOcRRT9nQc)

## 支持向量机

学习网址：`https://acgpp.github.io/simple-ml-code/`

## 代码块

```
import numpy as np
import matplotlib.pyplot as plt
from sklearn import svm
from sklearn.datasets import make_blobs
from sklearn.preprocessing import StandardScaler
```

## 逐行解释

`import numpy as np`

这行代码导入了`numpy`库，别名为`np`。`numpy`是一个强大的数学计算库，就像是我们手中的计算器，帮助我们进行各种数值运算。

`import matplotlib.pyplot as plt`

`matplotlib`是一个用于绘制图表和数据可视化的库，`pyplot`是`matplotlib`的一个模块，通常用于绘制各种类型的图表。这就像是我们手中的画笔，帮助我们画出漂亮的图形。

`from sklearn import svm`

从`sklearn`库中导入`svm`模块，`svm`是Support Vector Machine（支持向量机）的缩写。这是一个强大的分类算法，就像是一个聪明的分类器，能够找到最佳的分类边界。

`from sklearn.datasets import make_blobs`

从`sklearn`库的`datasets`模块导入`make_blobs`函数，这个函数可以帮助我们生成用于分类的模拟数据。这就像是我们自己创造一些数据来测试我们的模型。

`from sklearn.preprocessing import StandardScaler`

从`sklearn`库的`preprocessing`模块导入`StandardScaler`类，这个类用于数据的标准化处理。这就像是我们把不同尺度的数据统一到一个标准尺度，让模型更容易学习。

## 生成数据

### 生成随机数据

```
X, y = make_blobs(n_samples=100, centers=2, random_state=42)
```

### 逐行解释

`X, y = make_blobs(n_samples=100, centers=2, random_state=42)`

这行代码使用`make_blobs`函数生成了100个样本点，分为2个类别（`centers=2`）。`random_state=42`确保每次运行代码时生成的数据都是一样的，这就像是我们给随机数生成器设置了一个固定的种子。

## 数据标准化

### 数据标准化

```
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

### 逐行解释

`scaler = StandardScaler()`

创建了一个`StandardScaler`对象，这个对象可以帮助我们将数据标准化。标准化就是将数据转换到均值为0，标准差为1的分布。

`X_scaled = scaler.fit_transform(X)`

使用`scaler`对象对数据进行标准化处理。`fit_transform`方法会先计算数据的均值和标准差，然后将数据转换为标准正态分布。

## 训练模型

### 创建并训练SVM模型

```
clf = svm.SVC(kernel='linear')
clf.fit(X_scaled, y)
```

### 逐行解释

`clf = svm.SVC(kernel='linear')`

创建了一个SVM分类器，使用线性核函数。`kernel='linear'`表示我们使用线性核函数，这意味着我们假设数据可以用一条直线来分类。

`clf.fit(X_scaled, y)`

使用标准化后的数据训练SVM模型。这就像是让模型学习如何最好地区分不同类别的数据。

## 可视化结果

### 创建网格点

```
x_min, x_max = X_scaled[:, 0].min() - 1, X_scaled[:, 0].max() + 1
y_min, y_max = X_scaled[:, 1].min() - 1, X_scaled[:, 1].max() + 1
xx, yy = np.meshgrid(np.arange(x_min, x_max, 0.02),
np.arange(y_min, y_max, 0.02))
```

### 逐行解释

`x_min, x_max = X_scaled[:, 0].min() - 1, X_scaled[:, 0].max() + 1`
`y_min, y_max = X_scaled[:, 1].min() - 1, X_scaled[:, 1].max() + 1`

这两行代码计算了数据的范围，并稍微扩大了一些，以确保我们的可视化能够显示完整的分类边界。

`xx, yy = np.meshgrid(np.arange(x_min, x_max, 0.02), np.arange(y_min, y_max, 0.02))`

使用`np.meshgrid`创建了一个网格，这个网格将用于绘制分类边界。`0.02`是网格的步长，步长越小，边界越平滑。

### 预测网格点的类别

```
Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)
```

### 逐行解释

`Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])`

使用训练好的模型预测网格中每个点的类别。`np.c_`用于将`xx`和`yy`组合成坐标对。

`Z = Z.reshape(xx.shape)`

将预测结果重塑为与网格相同的形状，这样我们就可以绘制分类边界。

### 绘制结果

```
plt.figure(figsize=(10, 8))
plt.contourf(xx, yy, Z, alpha=0.4)
plt.scatter(X_scaled[:, 0], X_scaled[:, 1], c=y, alpha=0.8)
plt.title('SVM分类结果')
plt.show()
```

### 逐行解释

`plt.figure(figsize=(10, 8))`

创建一个新的图形，设置大小为10x8英寸。

`plt.contourf(xx, yy, Z, alpha=0.4)`

绘制分类边界，`alpha=0.4`设置透明度，使得边界看起来更柔和。

`plt.scatter(X_scaled[:, 0], X_scaled[:, 1], c=y, alpha=0.8)`

绘制数据点，`c=y`表示根据类别设置不同的颜色，`alpha=0.8`设置点的透明度。

`plt.title('SVM分类结果')`

设置图形的标题。

`plt.show()`

显示图形。

## 总结

支持向量机（SVM）是一个强大的分类算法，它通过找到一个最优的超平面来分隔不同类别的数据。在这个例子中，我们使用了线性核函数，这意味着我们假设数据可以用一条直线来分类。在实际应用中，我们还可以使用其他核函数（如`RBF`核）来处理更复杂的分类问题。

SVM的一个特点是它只关注支持向量（即最靠近分类边界的点），这使得它对噪声数据具有较强的鲁棒性。这就像是一个聪明的分类器，它不会被一些"捣乱"的数据点影响，而是专注于那些真正重要的边界点。
