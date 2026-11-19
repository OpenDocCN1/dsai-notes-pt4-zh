
# micrograd Readme 翻译

> 来源：[https://v11enp9ok1h.feishu.cn/docx/GQjFd5c4DovFINxs4Hjc9GyVnCc](https://v11enp9ok1h.feishu.cn/docx/GQjFd5c4DovFINxs4Hjc9GyVnCc)

标题：300 行代码从 0 手撕 `loss.backward()`，LLM101n 硬核更新了 Mircograd 课程

引言：欢迎加入 OpenAI 联合创始人硬核推出的 LLM101n 课程的中文版共建共学计划！

---

今天 OpenAI 联合创始人更新了 LLM101n 课程中的 `micrograd` 模块，300 行代码教我们不安装第三方依赖，只使用 `math` 库就从 0 实现自动梯度引擎（automatic gradien engine）的最小实现！

https://github.com/EurekaLabsAI/micrograd

## 译 | Micrograd Is All You Need

> `Micrograd`（或翻译为“微梯度”）是训练神经网络最小实现，（神经网络中的）其他一切组件都是为了优化效率而存在。

在这个模块（`Micrograd`）中，我们构建了[一个](https://stanford.edu/~jlmcc/papers/PDP/Volume%201/Chap8_PDP86.pdf)小型的“自动梯度”引擎（英文缩写为 `autograd`），它实现了反向传播算法。这一算法在1986年由Rumelhart、Hinton和Williams在其论文《Learning Internal Representations by Error Propagation》中被广泛推广，用于训练神经网络。本仓库基于早期的 `karpathy/micrograd` 仓库，并将其修改为 LLM101n 的课程模块。

我们在此构建的代码是神经网络训练的核心——它使我们能够计算如何更新神经网络的参数，以便让它在某个任务（例如自回归语言模型中的下一个词预测）上表现得更好。所有现代深度学习库，如 `PyTorch`、`TensorFlow`、`JAX` 等，都使用了完全相同的算法，只是这些库更为优化且功能丰富。

> 这是一个非常早期的草稿，提出了我（Andrej Karpathy）心中可能的第一个版本。简单来说，这是一个针对 2D 训练数据集的三分类模型。非常直观，易于理解，有助于直觉的建立。（译者注：共建共学项目组将持续跟进课程内容的更新，👸号后台回复“101n”加入共建共学社区群，也欢迎关注中文版 repo：https://github.com/SmartFlowAI/LLM101n-CN）

涵盖内容：

+ 自动微分引擎（`micrograd`）
+ 在其上构建的一个隐藏层神经网络（多层感知器，MLP）
+ 训练循环：损失函数、反向传播、参数更新

希望这个模块能够结合此[JavaScript网页演示](https://cs.stanford.edu/~karpathy/svmjs/demo/demonn.html)，从而让学生可以交互地添加/修改数据点，并播放/暂停优化过程，看看神经网络如何响应。然而，今天它可能会是一个用 `Streamlit` 构建的漂亮应用，使用这段代码。更好的是，可以在旁边显示计算图，详细展示数据/梯度在所有节点中的情况。

本课程未来预期会更新的内容：

## 课程内容概览

![](img/3b4612b6f749b22856d1e9cf63fd604f.png)

![](img/a1e233bb587f40f1988d1a17ed32cbee.png)

![](img/4ba09ac26329a0cc619147b8cf378c43.png)

![](img/330c51dd9ae004ab2e8a8321cce9cf9e.png)

![](img/cad01d0d486040a1e90d088291e49871.png)
