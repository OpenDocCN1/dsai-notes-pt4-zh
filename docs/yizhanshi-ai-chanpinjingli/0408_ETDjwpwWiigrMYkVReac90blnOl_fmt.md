
# PyTorch torch.nn 参考手册

> 来源：[https://v11enp9ok1h.feishu.cn/docx/YsemdPQ4QolIBXxwdCUclAbOnre](https://v11enp9ok1h.feishu.cn/docx/YsemdPQ4QolIBXxwdCUclAbOnre)

PyTorch 的 `torch.nn` 模块是构建和训练神经网络的核心模块，它提供了丰富的类和函数来定义和操作神经网络。

以下是 `torch.nn` 模块的一些关键组成部分及其功能：

## `nn.Module` 类

`nn.Module` 是所有自定义神经网络模型的基类。用户通常会从这个类派生自己的模型类，并在其中定义网络层结构以及前向传播函数（forward pass）。

## 预定义层（Modules）

包括各种类型的层组件，例如卷积层（`nn.Conv1d`、`nn.Conv2d`、`nn.Conv3d`）、全连接层（`nn.Linear`）、激活函数（`nn.ReLU`、`nn.Sigmoid`、`nn.Tanh`）等。

## 容器类

`nn.Sequential`：允许将多个层按顺序组合起来，形成简单的线性堆叠网络。

`nn.ModuleList` 和 `nn.ModuleDict`：可以动态地存储和访问子模块，支持可变长度或命名的模块集合。

## 损失函数（Loss Functions）

`torch.nn` 包含了一系列用于衡量模型预测与真实标签之间差异的损失函数，例如均方误差损失（`nn.MSELoss`）、交叉熵损失（`nn.CrossEntropyLoss`）等。

## 实用函数接口（Functional Interface）

`nn.functional`（通常简写为 `F`），包含了许多可以直接作用于张量上的函数，它们实现了与层对象相同的功能，但不具有参数保存和更新的能力。例如，可以使用 `F.relu()` 直接进行 ReLU 操作，或者 `F.conv2d()` 进行卷积操作。

## 初始化方法

`torch.nn.init` 提供了一些常用的权重初始化策略，比如 Xavier 初始化（`nn.init.xavier_uniform_()`）和 Kaiming 初始化（`nn.init.kaiming_uniform_()`），这些对于成功训练神经网络至关重要。

## PyTorch torch.nn 模块参考手册

### 神经网络容器

### 线性层

### 卷积层

### 池化层

### 激活函数

### 损失函数

### 归一化层

### 循环神经网络层

### 嵌入层

### Dropout 层

### 实用函数

### 实例

```
import torch
import torch.nn as nn

# 定义一个简单的神经网络
class SimpleNet(nn.Module):
    def __init__(self):
        super(SimpleNet, self).__init__()
        self.fc1 = nn.Linear(10, 20)
        self.relu = nn.ReLU()
        self.fc2 = nn.Linear(20, 1)

    def forward(self, x):
        x = self.fc1(x)
        x = self.relu(x)
        x = self.fc2(x)
        return x

# 创建模型和输入
model = SimpleNet()
input = torch.randn(5, 10)
output = model(input)
print(output)
```
