
# Diffusion Model 的数学原理 - Song Yang 笔记

> 来源：[https://learning.feishu.cn/docx/KUmSdFJulor7oFx8pWjcYZbQnjg](https://learning.feishu.cn/docx/KUmSdFJulor7oFx8pWjcYZbQnjg)

NeurIPS 2022 Workshop-Diffusion and Score-Based Generative Models

https://www.youtube.com/watch?v=wMmqCMwuM2Q

把 diffusion model 数学原理讲的最清楚的男人

## 其他求解方法

### 主流的求解思路

Diffusion model 之外的主要流派：

| 求解思路 | 典型模型 | 弊端 |
| --- | --- | --- |
| Approximating the normalizing constant | Energy-Based Models (EBMs) | Inaccurate probability evaluation |
| Using Restricted Neural Network Models | 1. Autoregressive model | Restricted model family |
| Modeling the Generation Process Only | GAN | Cannot evaluate probabilities |

个人观点：GAN 的问题，根因就在这里。数学不行，没有去建模 data distribution。

### 评判方法的维度

![](img/df3c6d45985578e3c88b6d563a7d3d08.png)

个人认为，漏了一个关键维度：fast during inference，或者叫 computational efficiency。

## 训练 - score function

数学科普：score function，是一个统计学术语。指 log-likelihood function 的梯度函数。

### 数学基础

1. density function 和 score function 可以相互无损的转化（求导 / 求积分），不丢失信息。

1. 收益：不用计算 normalizing constant 了。计算复杂度更低，问题解决。

1. 注意：Score function 里有 `log` 项，因为，score function 的定义，是 log-likelihood function 的梯度。

| Score Functions Bypass the normalizing constant | |
| --- | --- |
| | ![](img/37f43f860bb4f25e91fcfbbd378c81f4.png) |
| ![](img/6a5fb04e1d8e357ed671fb85a3e34e22.png) | ![](img/8ad48132fcba02d4a5532713326d25a6.png) |

关键细节：

1. 分子求导之后，有 2 项，只需要第 1 项。因为，定义上 `Z` 是 constant，所以，第二项是 0。

1. 更灵活的模型选择空间，因为，不要求输出的 function 积分面积为 1 了。

### 求解过程

要点：

1. 如果假设 groud truth（真实分布的梯度） 和 prediction 结果已知，可以用 fisher divergence 计算 loss。

1. 实际上，ground truth 未知。数学上，用 score matching 来解决。( TODO 没看懂)

详细解释如下

数学模型

![](img/0694ab24f89d7020d1e03845d5803d8a.png)

红色是 groud truth，蓝绿色是 prediction，计算 loss 的思路

| 1. 计算向量差，得知一组新 vector | 1. 计算新 vector 的 average length 作为 loss |
| --- | --- |
| ![](img/a84c17bbc52d93169ad64862b4e2c50b.png) | ![](img/8d385212724e3199e1a3549c97df8d17.png) |

数学上，这就是 fisher divergence。当，ground truth 未知时，用 score matching 方法求解

![](img/7e25f499f216765f39bd38861dd1fedc.png)

### 降低 score matching 的计算复杂度

困难：计算的规模没法 scale（高维问题的计算复杂度太高）

![](img/76f8c1ec9d8e2dc5c177349f1ad19ddb.png)

思路 1：投影成 1 维的做计算。

![](img/b49cf7fe3ed5c8d90bb4d09400a92154.png)

思路 2：denoising convention

增加 additional noise。

1. 调整，计算 noisy `x` 的 score function，而非 original `x` 的 score function。

1. 难点：无法 estimate scores of noise-free distribution。

![](img/2a3c15125dd39122856b938bc8d40095.png)

## 推理 - Langevin Dynamics

1. 加了 noise，好处理。

1. 最右边的，就是 langevin dynamics 方法。

![](img/1245a1e0cf1931aa1d2125e5c06f78b1.png)

在物理学中，朗之万动力学是一种对分子系统动力学进行数学建模的方法。它最初是由法国物理学家保罗·朗之万（Paul Langevin）开发的。该方法的特点是使用简化模型，同时通过使用随机微分方程来说明省略的自由度。Langevin 动力学模拟是一种蒙特卡洛模拟。

![](img/845164f46cf80f53b204bf391a57b9aa.png)

![](img/f909ad0a092e7b8825152b2e879d72fe.png)

![](img/de99652304c31fb82381ee88d8f8cab1.png)

Noise 在 low data density region 增加了梯度信息。约等于用 noise 解决梯度消失问题。

所以，添加了 noise 的数据更容易学习。

与人的直觉是等价的，即，一点一点的学习。被 noise 遮挡的区域，可以先不学习。

![](img/ef10a62f7698e701b948b032f235d334.png)

![](img/eb315d8f30e2d52103c7741494a20b61.png)

![](img/5e6925c260365b10d1485d5e25a075d9.png)

energy-based model

![](img/2607d528090a80479e9dcb1081e7df97.png)

只要分子的，就是 energy-based model。或者叫 unnormalized probabilistic model

从上面的推导看出，与 score based model 是等价的。
