
# 第三章 MLP

> 来源：[https://v11enp9ok1h.feishu.cn/docx/VajOdHGNxofcpcx6PW4ck9sAnkf](https://v11enp9ok1h.feishu.cn/docx/VajOdHGNxofcpcx6PW4ck9sAnkf)

https://github.com/EurekaLabsAI/mlp

## MLP

大家好！今天我们要聊的是一个非常激动人心的模块：我们通过训练一个多层感知器（MLP）来构建一个n-gram语言模型。这个模型的灵感来源于2003年Bengio等人的论文《A Neural Probabilistic Language Model》。

我们有多种并行实现方式，虽然方法各异，但结果却完全一致：

+ `C` 版本，它完全阐明了如何从零构建。
+ `numpy` 版本，它添加了 `Array` 抽象。这些操作现在被分组到对数组进行操作的函数中，但是前向传播和反向传播算法仍需自己从头构建。
+ `PyTorch` 版本，增加了 `Autograd` 引擎。`PyTorch` `Tensor` 对象看起来就像 [`numpy`](https://github.com/SmartFlowAI/LLM101n-CN/blob/master/micrograd/micrograd_1.md) 中的 `Array`，但在背后，`PyTorch` 保持了与我们在`micrograd`中看到的那样的计算图。具体而言，用户自定义前向传播算法，然后当他们在损失上调用`backward()`时，`PyTorch` 将会计算梯度。

`PyTorch` 提供的主要服务因此变得清晰：

+ 它给了你一个高效的 `Array` 对象，就像 `numpy` 一样，除了 `PyTorch` 称之为 `Tensor`，且一些API（看似）有点不同。本模块未涵盖的是，`PyTorch` 张量可以分布到不同的设备（如 GPU），这极大地加快了所有的计算。
+ 它为你提供了一个 `Autograd` 引擎，可以记录张量的计算图，并为你计算梯度。
+ 它为你提供了 `nn` 库，将 `Tensor` 操作组打包到深度学习中常见的预构建层和损失函数中。

[由于我们的努力，](https://github.com/SmartFlowAI/LLM101n-CN/tree/master/ngram)我们将享受到比在 `ngram` 模块中更低的验证损失，并且参数显著更少。然而，我们在训练时也以高得多的计算成本这样做（我们实质上是将数据集压缩到模型参数中），并且在推理时在某种程度上也是如此。

### TODOs

+ 调整超参数，使其不那么糟糕，我只是随意处理了一下。（当前看到验证损失为 2.06，基于计数的 4-gram 召回率为 2.11）
+ 合并 `C` 版本并使所有三个主要版本一致（`C`，`numpy`，`PyTorch`）
+ 为此模块绘制精美的图表

训练与验证损失值：

![](img/4cbab32a6030c6efa4de0a9631b62c9e.png)

代码结构（`mlp_pytorch.py`）：

![](img/596844db7519c210f27f5ff173e08fd3.png)
