
# 深度学习12条调参经验

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/BBVCdHDFWoTsRux9RbQcpunPn3e](https://qqs7y1hozd1.feishu.cn/docx/BBVCdHDFWoTsRux9RbQcpunPn3e)

## 12条调参经验分享

1. 调参一定要做好笔记，每次只调 1 个参数。

2. 在 `Validation` 筛选模型参数时，可以除 `loss` 函数“外，设置某种规则引导模型向某个想要的方向去更新参数。

3. 关于 `Activation function`:
(1) `ReLu`、`Sigmoid`、`Softmax`、`Tanh` 是最常用的 4 个激活函数
(2) 对于输出层，常用 `sigmoid` 和 `softMax` 激活函数，中间层常用 `ReLu` 激活函数，`RNN` 常用 `Tanh` 激活函数。

4. 关于 `Weights initialization`:
预训练参数是最好的参数初始化方式，其次是 `Xavir`。

5. 关于 `Regularization`:
`Dropout` 虽然思想很简单，但效果出奇的好，首选 0.5。

6. `Batch normalization` 很好用。

7. 如果在 `train` 模型时，`loss` 可以降到非常低那么模型的学习能力是够的; 如果 `train` 能达到，`validation` 达不到，则说明模型泛化能力不强。

8. 关于调参:
有哪些参数可以调呢?
`Loss function`、`Learning rate`、`Batch size`、`Epoch number`、`Optimizer`、`Activationfunction`、`Weights initialization`、使用 `Regularization`、`Validation`、使用的 GPU 个数。

9. 关于 `Loss function`:
`Loss function` 是 `Model` 和数据之外，第三重要的参数。具体使用 `MSE`、`Cross entropy`、`Focal` 还是其他自定义，需要具体问题具体分析。

10. 关于 `Epoch number` 和 `early stopping`
(1) `Epochnumber` 和 `Early stopping` 是息息相关的，需要输出 `loss` 看一下，到底是什么 `epoch` 时效果最好，及时 `early stopping`。
(2) `Epoch` 越大，会浪费计算资源; `epoch` 太小，则训练模型提取特征没到极致。
(3) 此外，也要明白 `Epoch`、`lteration`、`batchsize` 的关系。

11. 关于 `Optimizer`:
(1) `Adam` 和 `SGDM` 是最常用的两个，前者能快速收敛，后者收敛慢但最终精度更高。现在大家会先使用 `Adam` 快速收敛，后面再用 `SGDM` 提升精度。
(2) 如果必须二选一的话，我会推荐 `Adam`。

12. 关于 `Learning rate` 和 `batch size`:
(1) `Learningrate` 和 `batch size` 是两个重要的参数，而且二者也是相互影响的，在反向传播时直接影响梯度。一般情况下，先调 `batchsize`，再调 `learning rate`
(2) `batchsize` 不能太大，也不能太小; 太小会浪费计算资源，太大则会浪费内存; 一般设置为 16 的倍数。
(3) 如果使用微调，则 `learningrate` 设置为 0.0001 较好。`learningrate` 设置上有很多 `trick`, 包括 `cosing learning rate` 等。
