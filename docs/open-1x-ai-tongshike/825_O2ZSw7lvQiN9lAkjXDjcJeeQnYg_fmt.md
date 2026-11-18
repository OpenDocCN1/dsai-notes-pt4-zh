
# Chapter 1 线性回归

> 来源：[https://datawhaler.feishu.cn/docx/PeErdstv4oUZIUxmo8LcX8EynOh](https://datawhaler.feishu.cn/docx/PeErdstv4oUZIUxmo8LcX8EynOh)

学习网址：`https://acgpp.github.io/simple-ml-code/`

## 代码块

```
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
```

### 逐行解释

```
import numpy as np
```

`import` 是用于调用库的一个指令，`numpy` 是一个用于科学计算的库，特别是在处理数组和矩阵运算时非常有用，`as np` 即给 `numpy` 起一个简称方便我们在后面的程序中调用。就像是我们给好朋友起昵称一样，方便快速叫到他！

```
import matplotlib.pyplot as plt
```

`matplotlib` 是一个用于绘制图表和数据可视化的库，`pyplot` 是 `matplotlib` 的一个模块，通常用于绘制各种类型的图表，在这里我们用于绘制线图。可以把它想象成我们的画笔和画板，帮助我们把数据变成直观的图像。

```
from sklearn.linear_model import LinearRegression
```

`sklearn` 是一个用于机器学习的库，包含了许多常用的机器学习算法，`linear_model` 是 `sklearn` 中的一个模块，专门用于线性模型的实现。

`LinearRegression` 用于执行线性回归分析，它是一种最基础的回归算法，用于通过拟合一条直线来预测数据。

## 定义真实函数

```
def true_fun(X): # 这是我们设定的真实函数，即ground truth的模型
    return 1.5 * X + 0.2
```

### 逐行解释

`def true_fun(X)`:

用`def`定义了一个名为`true_fun`的函数，`X`是该函数的输入参数，即自变量。`true_fun`代表的是我们假设的真实关系函数。

在机器学习中，通常我们有一个真实的目标函数`y = f(X)`，但我们只有带噪声的观测数据，模型的目标是通过拟合数据来接近真实函数。

`return 1.5*X+0.2`

这行代码是`true_fun`函数的实现，代表这个函数输出的值（为`1.5*X+0.2`）。

这是一个线性函数，斜率为1.5，截距为0.2。也就是说，真实的函数是一个直线，斜率为1.5，y轴与直线的交点为0.2。

## 生成随机数据

```
np.random.seed(0) # 设置随机种子

n_samples = 30 # 设置采样数据点的个数
```

### 逐行解释

`np.random.seed(0)`

这里调用了`np`库里的`random`方法设置了随机数生成器的种子。

计算机中的随机数通常是伪随机的，它们是通过某些算法生成的。为了确保每次程序运行时得到相同的随机数，我们可以使用`seed`来设置"种子"。

这意味着每次运行程序时生成的随机数序列是一样的。0是种子的值，通常选择任意整数。

设置种子可以确保程序可重复性，尤其是在做调试或者需要对比结果时非常有用。就像是播种一样，同样的种子会长出同样的植物！

`n_samples = 30`

设置了训练数据中的样本数量，也就是我们将生成多少个数据点，这里设定的是30个样本点。

`n_samples`是我们设置的变量，变量是一个个小盒子，它的名字和装载的内容由我们规定，但有一定的限制。

变量名只能由字母，数字和下划线组成，不能以数字开头，大小写不同代表不同变量，如`Big`和`big`是两个变量；不能使用`python`的保留关键字作为变量名，如`if`，`else`等。

## 生成训练数据

```
X_train = np.sort(np.random.rand(n_samples))

y_train = (true_fun(X_train) + np.random.randn(n_samples) * 0.05).reshape(n_samples, 1)
```

### 逐行解释

`X_train = np.sort(np.random.rand(n_samples))`

`np.random.rand()`用于生成均匀分布的随机数，这里可以把`n_samples`等价看做30。

`np.sort()`将这些随机数从小到大排序，使得训练数据`X_train`是一个从0到1排序的数组。即这里将一个含有三十个随机数字的从0-1排序的数组赋值给`X_train`。

`y_train = (true_fun(X_train) + np.random.randn(n_samples) * 0.05).reshape(n_samples, 1)`

`true_fun(X_train)`计算每个样本点`X_train`对应的真实函数值，即`y = 1.5*X + 0.2`。

`np.random.randn(n_samples)`生成符合标准正态分布（均值为0，方差为1）的随机数，用于模拟噪声。

`np.random.randn(n_samples) * 0.05`将噪声的幅度缩小到原来的0.05倍，模拟一些小的随机误差，确保数据不完全符合真实的线性关系，这样更符合实际情况。

`.reshape(n_samples, 1)`将`y_train`转换为一个列向量，使得它的形状为(30, 1)，这是机器学习库`sklearn`中常用的数据格式。

注意，这里我们是模拟数据，噪声是为了使我们模拟的数据样本更贴合现实，在现实中，不会有那么标准的线性关系。数据总会有一些"杂音"，就像我们听音乐时可能会听到一些背景噪音一样。这些噪声让我们的模型更加健壮，能够适应真实世界的不完美数据。

## 训练模型

```
model = LinearRegression() # 定义模型

model.fit(X_train[:, np.newaxis], y_train) # 训练模型

print("输出参数w：", model.coef_) # 输出模型参数w

print("输出参数b：", model.intercept_) # 输出参数b
```

### 逐行解释

`model = LinearRegression()`

创建了一个`LinearRegression`类的实例，并将其赋值给变量`model`。

线性回归模型的目标是找到一个最合适的直线，以使得真实数据点和拟合直线之间的误差最小化。我们可以使用`LinearRegression()`类来构建该模型。这就像是我们在找一条最佳路径，让所有的点到这条路径的距离总和最小。

`model.fit(X_train[:, np.newaxis], y_train)`

`.fit()`方法用于训练模型，即根据训练数据拟合线性回归模型的参数（权重`w`和截距`b`）。

`X_train[:, np.newaxis]`这个操作将`X_train`的形状从(30,)转换为(30, 1)。

它通过`np.newaxis`将原来一维的数组变成二维数组。原来的一维数组是(30,)，经过`[:, np.newaxis]`后变成了(30, 1)。

这是因为`sklearn`的`LinearRegression`要求输入的特征数据必须是二维数组，即每个样本点都是一个行向量。

`y_train`是目标值，形状已经是(30, 1)，所以可以直接作为输入。

## 输出模型参数

```
print("输出参数w：", model.coef_) # 输出模型参数w

print("输出参数b：", model.intercept_) # 输出参数b
```

### 逐行解释

`print("输出参数w:", model.coef_)`

`model.coef_`是线性回归模型的属性，表示模型学习到的回归系数（即斜率`w`）。

在简单线性回归中，回归系数表示输入特征`X`对目标变量`y`的影响。在本例中，`w`是斜率。

`print("输出参数w:", model.coef_)`输出模型训练后学到的回归系数（斜率）。

`print("输出参数b:", model.intercept_)`

`model.intercept_`是线性回归模型的另一个属性，表示模型学习到的截距`b`。

截距是回归直线与y轴的交点，也就是说，它表示当`X = 0`时，`y`的值。

在我们的例子中，真实的截距是0.2，因此我们期望`model.intercept_`的值接近0.2。

`print("输出参数b:", model.intercept_)`，输出模型训练后学到的截距。

## 绘制图形

```
X_test = np.linspace(0, 1, 100)

plt.plot(X_test, model.predict(X_test[:, np.newaxis]), label="Model")

plt.plot(X_test, true_fun(X_test), label="True function")

plt.scatter(X_train, y_train) # 画出训练集的点

plt.legend(loc="best")

plt.show() # 这一部分建议运行代码后根据显示出的图形来一一对应
```

### 逐行解释

`X_test = np.linspace(0, 1, 100)`

`np.linspace(0, 1, 100)`生成了一个从0到1的等间距的100个点。

`np.linspace()`是`numpy`中的一个函数，用来生成指定范围内的均匀分布的数字。

在这里，我们使用`X_test`来生成100个在[0, 1]区间内的测试点。我们将用这些测试点来预测模型的输出，进而绘制拟合曲线。

`plt.plot(X_test, model.predict(X_test[:, np.newaxis]), label="Model")`

`model.predict(X_test[:, np.newaxis])`通过训练好的模型来预测`X_test`上的输出值。

`X_test[:, np.newaxis]`将`X_test`从一维数组（长度为100）转换为二维数组（形状为(100, 1)），以便与模型的输入格式匹配。

`plt.plot()`函数将预测的结果与`X_test`绘制成一条曲线，并用"Model"作为该曲线的标签（label）。

这条曲线表示模型对测试数据的预测结果。

`plt.plot(X_test, true_fun(X_test), label="True function")`

`true_fun(X_test)`计算出真实函数`y = 1.5 * X + 0.2`在`X_test`上的值。

这条曲线表示真实的函数关系，它是我们的数据生成函数，用于与模型的预测结果进行比较。

`plt.plot()`将真实函数的结果绘制成一条曲线，并用"True function"作为该曲线的标签。

`plt.scatter(X_train, y_train)`

`plt.scatter(X_train, y_train)`绘制出训练数据点的散点图。每个点表示一个训练样本的(`X_train[i]`, `y_train[i]`).

散点图显示了训练数据的分布情况，让我们可以直观地看到数据是如何分布的。

`plt.legend(loc="best")`

`plt.legend(loc="best")`为图形添加一个图例，并将其位置设置为`best`，即`matplotlib`会自动选择最佳位置放置图例。

图例将显示不同曲线的标签，例如"Model"和"True function"。

`plt.show()`

`plt.show()`显示绘制好的图形。

这会弹出一个窗口，显示图形。在该窗口中，你可以看到模型拟合的结果与真实函数的比较，以及训练数据点的位置。

当程序执行后你可能会发现，你所得到的数据类似：输出参数`w: [[1.4474774]]`输出参数`b: [0.22557542]`。

这与我们标准的`y = 1.5*X + 0.2`有所偏差，这是正常的，线性回归的过程就是一个不断拟合的过程，基于我们的样本数量的有限，我们只能无限趋近于准确的值。就像是射箭，我们可能无法每次都正中靶心，但通过不断练习，我们的箭会越来越接近靶心！
