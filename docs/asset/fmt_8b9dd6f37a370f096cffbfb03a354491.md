
# `XGBoost`模型详解

> 来源：[https://v11enp9ok1h.feishu.cn/docx/CHe9dAMMCoLPzmxbkXJcpbkmnus](https://v11enp9ok1h.feishu.cn/docx/CHe9dAMMCoLPzmxbkXJcpbkmnus)

## 1.算法原理可视化解读

[关于`XGBoost`](https://web.njit.edu/~usman/courses/cs675_spring20/BoostedTree.pdf) 的原理，其作者陈天奇本人有一个非常详尽的Slides做了系统性的介绍，我们在这里借助于这个资料给大家做展开讲解。

### 1）监督学习中的一些重要概念

在开始介绍 `Boosted Tree` 之前，我们先来回顾一下机器学习中的一些重要的概念。

#### （1）监督学习核心要素

![](img/c85da80d646128d43ce41ee64bf976b2.png)

线性模型（`Linear Model`）\hat{y}_i=\Sigma_jw_jx_{ij}（包括线性回归和逻辑回归），预测值根据不同的任务有不同的解释：

+ 线性回归（`Linear Regression`）：表示预测的分数。
+ 逻辑回归（`Logistic Regression`）：预测了实例为正的概率。
+ 其他：例如在排名任务中可以是排名分数。

![](img/2ec24558107e1202b65aa9b0b81e3227.png)

参数（`Parameters`）：需要从数据中学习的东西。

+ `Linear Model`：

目标函数（`Objective function`）

+ 代表训练损失函数（`Training Loss`），表示模型多好的拟合了训练数据。
+ 为正则化项（`Regularization`）衡量了模型的复杂程度。

训练数据损失函数（`Training Loss`）

+ 平方损失（`Square Loss`）：
+ 逻辑损失（`Logistic Loss`）：

正则化项（`Regularization`）：描述了模型的复杂程度。

`L1 Norm`（`lasso`）：

`L2 Norm`：

#### （2）监督学习进阶知识

![](img/ab84751f86b167dfd7b8a90dad6d4312.png)

`Ridge`回归：

+ `Lasso`是`Linear Model`，用的是`Square Loss`，正则化项是`L1 Norm`。

逻辑回归（`Logistic Regression`）：

+ 逻辑回归是`Linear Model`，用的是`Logistic Loss`，正则化项是`L2 Norm`。

#### （3）目标函数及偏差方差权衡

回顾一下目标函数，为什么目标函数需要两部分组成呢？

![](img/c67d43de861afbdfa08f4065beca6549.png)

+ 优化训练损失函数（`Training Loss`）有助于建立预测模型，很好地拟合训练数据至少能让你更接近潜在的训练数据的分布。
+ 优化正则化项（`Regularization`）有助于建立简单的模型：模型越简单，未来预测的方差越小，预测越稳定。
