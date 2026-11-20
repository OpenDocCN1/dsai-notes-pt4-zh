
# Transformer

> 来源：[https://v11enp9ok1h.feishu.cn/docx/WirIdUM2NouZNPxNUuicXtyKn4d](https://v11enp9ok1h.feishu.cn/docx/WirIdUM2NouZNPxNUuicXtyKn4d)

## 1. Transformer 整体结构

首先介绍 `Transformer` 的整体结构，下图是 `Transformer` 用于中英文翻译的整体结构：

![](img/db8accb919f8a50fc126c723bd3364ba.png)

`Transformer` 的整体结构，左图 `Encoder` 和右图 `Decoder`

> 可以看到 `Transformer` 由 `Encoder` 和 `Decoder` 两个部分组成，`Encoder` 和 `Decoder` 都包含 6 个 `block`。

- `Transformer` 的工作流程大体如下：

- 第一步：获取输入句子的每一个单词的表示向量 `X`，`X` 由单词的 `Embedding`（`Embedding` 就是从原始数据提取出来的 `Feature`）和单词位置的 `Embedding` 相加得到。

![](img/6c9a16e9f1e8a68f3aa8ced53a103073.png)

`Transformer` 的输入表示

- 第二步：将得到的单词表示向量矩阵（如上图所示，每一行是一个单词的表示 `x`）传入 `Encoder` 中，经过 6 个 `Encoder` `block` 后可以得到句子所有单词的编码信息矩阵 `C`，

  如下图。单词向量矩阵用 `X` 表示，`n` 是句子中单词个数，`d` 是表示向量的维度（`d=512`）。每一个 `Encoder` `block` 输出的矩阵维度与输入完全一致。

![](img/d0312fc1f9049c698c5035473addf23a.png)

- 第三步：将 `Encoder` 输出的编码信息矩阵 `C` 传递到 `Decoder` 中，`Decoder` 依次会根据当前翻译过的单词 `1~i` 翻译下一个单词 `i+1`，如下图所示。在使用过程中，翻译到单词 `i+1` 的时候需要通过 `Mask` (掩盖) 操作遮盖住 `i+1` 之后的单词。

![](img/cc5da5a3cda9af8a5b31c0a1a4720836.png)

`Transformer` Decoder 预测

> 上图 `Decoder` 接收了 `Encoder` 的编码矩阵 `C`，然后首先输入一个翻译开始符 `"<Begin>"`，预测第一个单词 `"I"`；然后输入翻译开始符 `"<Begin>"` 和单词 `"I"`，预测单词 `"have"`，以此类推。这是 `Transformer` 使用时的大致流程，接下来是里面各个部分的细节。

## Transformer 的输入

> `Transformer` 中单词的输入表示 `x` 由单词 `Embedding` 和位置 `Embedding`（`Positional Encoding`）相加得到。

![](img/3cc62394cb0fb3fc4f79aeb188e65bcc.png)

`Transformer` 的输入表示

### 单词 Embedding

单词的 `Embedding` 有很多种方式可以获取，例如可以采用 `Word2Vec`、`Glove` 等算法预训练得到，也可以在 `Transformer` 中训练得到。

### 位置 Embedding

> `Transformer` 中除了单词的 `Embedding`，还需要使用位置 `Embedding` 表示单词出现在句子中的位置。因为 `Transformer` 不采用 `RNN` 的结构，而是使用全局信息，不能利用单词的顺序信息，而这部分信息对于 `NLP` 来说非常重要。所以 `Transformer` 中使用位置 `Embedding` 保存单词在序列中的相对或绝对位置。

位置 `Embedding` 用 `PE` 表示，`PE` 的维度与单词 `Embedding` 是一样的。`PE` 可以通过训练得到，也可以使用某种公式计算得到。在 `Transformer` 中采用了后者。

**计算公式如下：**

![](img/7d7ebc5fc03ddab75746b615696dae2a.png)

其中，`pos` 表示单词在句子中的位置，`d` 表示 `PE` 的维度（与词 `Embedding` 一样），`2i` 表示偶数的维度，`2i+1` 表示奇数维度（即 `2i≤d, 2i+1≤d`）。使用这种公式计算 `PE` 有以下的好处：

- 使 `PE` 能够适应比训练集里面所有句子更长的句子，假设训练集里面最长的句子是有 20 个单词，突然来了一个长度为 21 的句子，则使用公式计算的方法可以计算出第 21 位的 `Embedding`。

- 可以让模型容易地计算出相对位置，对于固定长度的间距 `k`，`PE(pos+k)` 可以用 `PE(pos)` 计算得到。

- 因为 `Sin(A+B) = Sin(A)Cos(B) + Cos(A)Sin(B)`，`Cos(A+B) = Cos(A)Cos(B) - Sin(A)Sin(B)`。

## Self-Attention（自注意力机制）

`Transformer` 的内部结构图

- 左侧为 `Encoder` `block`

- 右侧为 `Decoder` `block`

- 红色圈中的部分为 `Multi-Head Attention`，是由多个 `Self-Attention` 组成的

1. 可以看到 `Encoder` `block` 包含一个 `Multi-Head Attention`

1. 而 `Decoder` `block` 包含两个 `Multi-Head Attention`（其中有一个用到 `Masked`）。

1. `Multi-Head Attention` 上方还包括一个 `Add & Norm` 层

1. `Add` 表示残差连接（Residual Connection）用于防止网络退化，

1. `Norm` 表示 `Layer Normalization`，用于对每一层的激活值进行归一化。

![](img/3d325a87d9fbf56c62e8750bc7cccb71.png)

因为 `Self-Attention` 是 `Transformer` 的重点，所以我们重点关注 `Multi-Head Attention` 以及 `Self-Attention`，首先详细了解一下 `Self-Attention` 的内部逻辑。

### Self-Attention 结构

![](img/bd7535f6174c6f2c175fa93e136a29b7.png)

Self-Attention 结构

上图是 `Self-Attention` 的结构，在计算时需要用到矩阵 `Q`（查询）、`K`（键值）、`V`（值）。在实际中，`Self-Attention` 接收的是输入（单词的表示向量 `x` 组成的矩阵 `X`）或者上一个 `Encoder` `block` 的输出。而 `Q`、`K`、`V` 正是通过 `Self-Attention` 的输入进行线性变换得到的。

### Q、K、V 的计算

> `Self-Attention` 的输入用矩阵 `X` 进行表示，则可以使用线性变换矩阵 `WQ`、`WK`、`WV` 计算得到 `Q`、`K`、`V`。计算如下图所示，注意 `X`、`Q`、`K`、`V` 的每一行都表示一个单词。

![](img/c5159bb59a1d25295a8b8ea5af5f2132.png)

Q、K、V 的计算

### 实现

```python
# self-attention 实现
import numpy as np
from math import sqrt
import torch
from torch import nn


class Self_Attention(nn.Module):
    # input : batch_size * seq_len * input_dim
    # q : batch_size * input_dim * dim_k
    # k : batch_size * input_dim * dim_k
    # v : batch_size * input_dim * dim_v
    def __init__(self, input_dim, dim_k, dim_v):
        super(Self_Attention, self).__init__()
        self.q = nn.Linear(input_dim, dim_k)
        self.k = nn.Linear(input_dim, dim_k)
        self.v = nn.Linear(input_dim, dim_v)
        self._norm_fact = 1 / sqrt(dim_k)

    def forward(self, x):
        Q = self.q(x)  # Q: batch_size * seq_len * dim_k
        K = self.k(x)  # K: batch_size * seq_len * dim_k
        V = self.v(x)  # V: batch_size * seq_len * dim_v

        # Q * K.T() # batch_size * seq_len * seq_len
        atten = nn.Softmax(
            dim=-1)(torch.bmm(Q, K.permute(0, 2, 1))) * self._norm_fact

        # Q * K.T() * V # batch_size * seq_len * dim_v
        output = torch.bmm(atten, V)

        return output


X = torch.randn(4, 3, 2)
print(X)
self_atten = Self_Attention(2, 4, 5)  # input_dim:2, k_dim:4, v_dim:5
res = self_atten(X)
print(res.shape)  # [4,3,5]
```

### Self-Attention 的输出

得到矩阵 `Q`、`K`、`V` 之后就可以计算出 `Self-Attention` 的输出了，计算的公式如下：

![](img/5eaa1f12474c52a54888b74c323d2081.png)

Self-Attention 的输出

> 公式中计算矩阵 `Q` 和 `K` 每一行向量的内积，为了防止内积过大，因此除以 `sqrt(d_k)` 的平方根。`Q` 乘以 `K` 的转置后，得到的矩阵行列数都为 `n`，`n` 为句子单词数，这个矩阵可以表示单词之间的 `attention` 强度。下图为 `Q` 乘以 `K` 的转置，`1234` 表示的是句子中的单词。

![](img/b0af5013d42e6ce994a46a328b7322c2.png)

`Q` 乘以 `K` 的转置的计算

得到 `QK^T` 之后，使用 `Softmax` 计算每一个单词对于其他单词的 `attention` 系数，公式中的 `Softmax` 是对矩阵的每一行进行 `Softmax`，即每一行的和都变为 1。

![](img/0bed1ee05e0ca81579575f5527cff502.png)

对矩阵的每一行进行 `Softmax`

得到 `Softmax` 矩阵之后可以和 `V` 相乘，得到最终的输出 `Z`。

![](img/d0722a0f781fd4bbc57fbc4d3475e146.png)

`Self-Attention` 输出

上图中 `Softmax` 矩阵的第 1 行表示单词 1 与其他所有单词的 `attention` 系数，最终单词 1 的输出 `Z_1` 等于所有单词 `i` 的值 `V_i` 根据 `attention` 系数的比例加在一起得到，如下图所示：

![](img/995074578cb8135477c2e19b82a63d41.png)

`Z_i` 的计算方法

### Multi-Head Attention

在上一步，我们已经知道怎么通过 `Self-Attention` 计算得到输出矩阵 `Z`，而 `Multi-Head Attention` 是由多个 `Self-Attention` 组合形成的，下图是论文中 `Multi-Head Attention` 的结构图。

![](img/63ef6d80ebb3b3c59d7f74917d619bfc.png)

`Multi-Head Attention`

从上图可以看到 `Multi-Head Attention` 包含多个 `Self-Attention` 层，首先将输入 `X` 分别传递到 `h` 个不同的 `Self-Attention` 中，计算得到 `h` 个输出矩阵 `Z`。下图是 `h=8` 时的情况，此时会得到 8 个输出矩阵 `Z`。

### 实现

```python
#%%
# Muti-head Attention 机制的实现
from math import sqrt
import torch
import torch.nn as nn


class Self_Attention_Muti_Head(nn.Module):
    # input : batch_size * seq_len * input_dim
    # q : batch_size * input_dim * dim_k
    # k : batch_size * input_dim * dim_k
    # v : batch_size * input_dim * dim_v
    def __init__(self, input_dim, dim_k, dim_v, nums_head):
        super(Self_Attention_Muti_Head, self).__init__()
        assert dim_k % nums_head == 0
        assert dim_v % nums_head == 0
        self.q = nn.Linear(input_dim, dim_k)
        self.k = nn.Linear(input_dim, dim_k)
        self.v = nn.Linear(input_dim, dim_v)

        self.nums_head = nums_head
        self.dim_k = dim_k
        self.dim_v = dim_v
        self._norm_fact = 1 / sqrt(dim_k)

    def forward(self, x):
        Q = self.q(x).reshape(-1, x.shape[0], x.shape[1], self.dim_k // self.nums_head)
        K = self.k(x).reshape(-1, x.shape[0], x.shape[1], self.dim_k // self.nums_head)
        V = self.v(x).reshape(-1, x.shape[0], x.shape[1], self.dim_v // self.nums_head)
        print(x.shape)
        print(Q.size())

        atten = nn.Softmax(dim=-1)(torch.matmul(Q, K.permute(0, 1, 3, 2)))  # Q * K.T() # batch_size * seq_len * seq_len

        output = torch.matmul(atten, V).reshape(x.shape[0], x.shape[1], -1)  # Q * K.T() * V # batch_size * seq_len * dim_v

        return output


x = torch.rand(1, 3, 4)
print(x)
# %%
atten = Self_Attention_Muti_Head(4, 4, 4, 2)
y = atten(x)
print(y.shape)

# %%
```

![](img/e11f987c2687ed658f590c984aabf956.png)

多个 `Self-Attention`

得到 `h` 个输出矩阵 `Z` 之后，`Multi-Head Attention` 将它们拼接在一起（`Concat`），然后传入一个 `Linear` 层，得到 `Multi-Head Attention` 最终的输出 `Z`。

![](img/e1292c1fe6f761c2b0c3945d7cd82151.png)

`Multi-Head Attention` 的输出

可以看到 `Multi-Head Attention` 输出的矩阵 `Z` 与其输入的矩阵 `X` 的维度是一样的。

## Encoder 结构
