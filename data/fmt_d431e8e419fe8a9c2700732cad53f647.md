
# PyTorch 基础

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HXakd8HlDoUwCNxQdekcCFV5n0c](https://v11enp9ok1h.feishu.cn/docx/HXakd8HlDoUwCNxQdekcCFV5n0c)

## PyTorch 架构总览

`PyTorch` 采用模块化设计，由多个相互协作的核心组件构成。理解这些组件的作用和相互关系，是掌握 `PyTorch` 的关键。

### PyTorch 架构图

```
┌─────────────────────────────────────────────────────────────┐
│                    PyTorch 生态系统                          │
├─────────────────────────────────────────────────────────────┤
│  torchvision  │  torchtext  │  torchaudio  │  其他专业库     │
├─────────────────────────────────────────────────────────────┤
│                     PyTorch 核心                            │
├───────────────┬─────────────────┬───────────────────────────┤
│   torch.nn    │   torch.optim   │      torch.utils          │
│   (神经网络)   │   (优化器)      │      (工具函数)           │
├───────────────┼─────────────────┼───────────────────────────┤
│               │                 │   torch.utils.data        │
│  torch 核心   │  autograd       │   (数据加载)              │
│  (张量计算)   │  (自动微分)     │                           │
└───────────────┴─────────────────┴───────────────────────────┘
```

`PyTorch` 采用**分层架构**设计，从上层到底层依次为：

### 1、Python API（顶层）

+ `torch`：核心张量计算（类似`NumPy`，支持GPU）。

+ `torch.nn`：神经网络层、损失函数等。

+ `torch.autograd`：自动微分（反向传播）。

+ 开发者直接调用的接口，简单易用。

### 2、C++核心（中层）

+ `ATen`：张量运算核心库（400+操作）。

+ `JIT`：即时编译优化模型。

+ `Autograd` 引擎：自动微分的底层实现。

+ 高性能计算，连接`Python`与底层硬件。

### 3、基础库（底层）

+ `TH/THNN`：`C`语言实现的基础张量和神经网络操作。

+ `THC/THCUNN`：对应的`CUDA`（GPU）版本。

+ 直接操作硬件（`CPU`/`GPU`），极致优化速度。

**执行流程**：

`Python代码` → `C++核心计算` → 底层`CUDA`/`C`库加速 → 返回结果。

既保持易用性，又确保高性能。

![](img/89ffc45ff47966ebfbfae97e43a23d7c.png)

## 张量（`Tensor`）

张量（`Tensor`）是 `PyTorch` 中的核心数据结构，用于存储和操作多维数组。

张量可以视为一个多维数组，支持加速计算的操作。

在 `PyTorch` 中，张量的概念类似于 `NumPy` 中的数组，但是 `PyTorch` 的张量可以运行在不同的设备上，比如 `CPU` 和 `GPU`，这使得它们非常适合于进行大规模并行计算，特别是在深度学习领域。

+ 维度（`Dimensionality`）：张量的维度指的是数据的多维数组结构。例如，一个标量（0维张量）是一个单独的数字，一个向量（1维张量）是一个一维数组，一个矩阵（2维张量）是一个二维数组，以此类推。

+ 形状（`Shape`）：张量的形状是指每个维度上的大小。例如，一个形状为(3, 4)的张量意味着它有3行4列。

+ 数据类型（`Dtype`）：张量中的数据类型定义了存储每个元素所需的内存大小和解释方式。`PyTorch`支持多种数据类型，包括整数型（如`torch.int8`、`torch.int32`）、浮点型（如`torch.float32`、`torch.float64`）和布尔型（`torch.bool`）。

**张量创建：**

```
import torch

# 创建一个 2x3 的全 0 张量
a = torch.zeros(2, 3)
print(a)

# 创建一个 2x3 的全 1 张量
b = torch.ones(2, 3)
print(b)

# 创建一个 2x3 的随机数张量
c = torch.randn(2, 3)
print(c)

# 从 NumPy 数组创建张量
import numpy as np
numpy_array = np.array([[1, 2], [3, 4]])
tensor_from_numpy = torch.from_numpy(numpy_array)
print(tensor_from_numpy)

# 在指定设备（CPU/GPU）上创建张量
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
d = torch.randn(2, 3, device=device)
print(d)
```

输出结果类似如下：

```
tensor([[0., 0., 0.],
        [0., 0., 0.]])
tensor([[1., 1., 1.],
        [1., 1., 1.]])
tensor([[ 1.0189, -0.5718, -1.2814],
        [-0.5865,  1.0855,  1.1727]])
tensor([[1, 2],
        [3, 4]])
tensor([[-0.3360,  0.2203,  1.3463],
        [-0.5982, -0.2704,  0.5429]])
```

**常用张量操作：**

### 实例

```
# 张量相加
e = torch.randn(2, 3)
f = torch.randn(2, 3)
print(e + f)

# 逐元素乘法
print(e * f)

# 张量的转置
g = torch.randn(3, 2)
print(g.t())  # 或者 g.transpose(0, 1)

# 张量的形状
print(g.shape)  # 返回形状
```

### 张量与设备

`PyTorch` 张量可以存在于不同的设备上，包括`CPU`和`GPU`，你可以将张量移动到 `GPU` 上以加速计算：

```
if torch.cuda.is_available():
    tensor_gpu = tensor_from_list.to('cuda')  # 将张量移动到GPU
```

### 梯度和自动微分

`PyTorch`的张量支持自动微分，这是深度学习中的关键特性。当你创建一个需要梯度的张量时，`PyTorch`可以自动计算其梯度：

### 实例

```
# 创建一个需要梯度的张量
tensor_requires_grad = torch.tensor([1.0], requires_grad=True)

# 进行一些操作
tensor_result = tensor_requires_grad * 2

# 计算梯度
tensor_result.backward()
print(tensor_requires_grad.grad)  # 输出梯度
```

### 内存和性能

`PyTorch` 张量还提供了一些内存管理功能，比如`.clone()`、`.detach()` 和 `.to()` 方法，它们可以帮助你优化内存使用和提高性能。

---

## 自动求导（`Autograd`）

自动求导（Automatic Differentiation，简称`Autograd`）是深度学习框架中的一个核心特性，它允许计算机自动计算数学函数的导数。

在深度学习中，自动求导主要用于两个方面：一是在训练神经网络时计算梯度，二是进行反向传播算法的实现。

自动求导基于链式法则（`Chain Rule`），这是一个用于计算复杂函数导数的数学法则。链式法则表明，复合函数的导数是其各个组成部分导数的乘积。在深度学习中，模型通常是由许多层组成的复杂函数，自动求导能够高效地计算这些层的梯度。

### 动态图与静态图：

+ 动态图（`Dynamic Graph`）：在动态图中，计算图在运行时动态构建。每次执行操作时，计算图都会更新，这使得调试和修改模型变得更加容易。`PyTorch`使用的是动态图。

+ 静态图（`Static Graph`）：在静态图中，计算图在开始执行之前构建完成，并且不会改变。`TensorFlow`最初使用的是静态图，但后来也支持动态图。

`PyTorch` 提供了自动求导功能，通过 `autograd` 模块来自动计算梯度。

`torch.Tensor` 对象有一个 `requires_grad` 属性，用于指示是否需要计算该张量的梯度。

当你创建一个 `requires_grad=True` 的张量时，`PyTorch` 会自动跟踪所有对它的操作，以便在之后计算梯度。

### 创建需要梯度的张量

### 实例

```
# 创建一个需要计算梯度的张量
x = torch.randn(2, 2, requires_grad=True)
print(x)

# 执行某些操作
y = x + 2
z = y * y * 3
out = z.mean()

print(out)
```

输出结果类似如下：

```
tensor([[0., 0., 0.],
        [0., 0., 0.]])
tensor([[1., 1., 1.],
        [1., 1., 1.]])
tensor([[ 1.0189, -0.5718, -1.2814],
        [-0.5865,  1.0855,  1.1727]])
tensor([[1, 2],
        [3, 4]])
tensor([[-0.3360,  0.2203,  1.3463],
        [-0.5982, -0.2704,  0.5429]])
tianqixin@Mac-mini runoob-test % python3 test.py
tensor([[-0.1908,  0.2811],
        [ 0.8068,  0.8002]], requires_grad=True)
tensor(18.1469, grad_fn=<MeanBackward0>)
```

### 反向传播（`Backpropagation`）

一旦定义了计算图，可以通过 `.backward()` 方法来计算梯度。

### 实例

```
# 反向传播，计算梯度
out.backward()

# 查看 x 的梯度
print(x.grad)
```

在神经网络训练中，自动求导主要用于实现反向传播算法。

反向传播是一种通过计算损失函数关于网络参数的梯度来训练神经网络的方法。在每次迭代中，网络的前向传播会计算输出和损失，然后反向传播会计算损失关于每个参数的梯度，并使用这些梯度来更新参数。

### 停止梯度计算

如果你不希望某些张量的梯度被计算（例如，当你不需要反向传播时），可以使用 `torch.no_grad()` 或设置 `requires_grad=False`。

```
# 使用 torch.no_grad() 禁用梯度计算
with torch.no_grad():
    y = x * 2
```
