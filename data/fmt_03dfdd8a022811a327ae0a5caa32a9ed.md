

# LLM（十七）：从 `FlashAttention` 到 `PagedAttention`, 如何进一步优化 `Attention` 性能

> 来源：[https://agijuejin.feishu.cn/docx/NjuHdKWsdow6K7xf1b1cEXSRnnd](https://agijuejin.feishu.cn/docx/NjuHdKWsdow6K7xf1b1cEXSRnnd)

> 作者：紫气东来

> 原文：[https://zhuanlan.zhihu.com/p/638468472](https://zhuanlan.zhihu.com/p/638468472)

## 一、`FlashAttention` 基本原理

### 1.1 GPU 硬件特点

由于 `FlashAttention` 计算 `self-attention` 的主要关键是有效的硬件使用，所以了解 GPU 内存和各种操作的性能特征是很有必要的。

以 A100 (40GB HBM) 为例，下面显示其内存层次结构的粗略图。SRAM 内存分布在 108 个流式多处理器 (SMs) 上，每个处理器 192KB。片上 SRAM 比 HBM 快得多，但比 HBM 小得多，在计算方面，使用 Tensor Core 的 `BFLOAT16` 的理论峰值吞吐量为 312 TFLOPS。GPU 的典型操作方式是使用大量的线程来执行一个操作，这个操作被称为内核。输入从 HBM 加载到寄存器和 SRAM，并在计算后写回 HBM。

![](img/3f4ea703e5cbe32fb70d699e2ef12e2f.png)

**算法对于内存带宽的需求**通常使用 计算强度 (arithmetic intensity) 来表示，单位是 OPs/byte。意思是在算法中平均每读入单位数据，能支持多少次运算操作。它有助于理解操作的瓶颈，即计算约束 (Compute-bound) 或带宽约束 (Bandwidth-bound, or Memory-bound)。

+   `算力` `π`：也称为计算平台的 性能上限 ，指的是一个计算平台倾尽全力每秒钟所能完成的浮点运算数。单位是 FLOPS or FLOP/s 。

+   `带宽` `β` ：也即计算平台的 带宽上限 ，指的是一个计算平台倾尽全力每秒所能完成的内存交换量。单位是 Byte/s 。

+   **计算强度上限** `Imax = π / β` ：两个指标相除即可得到计算平台的 计算强度上限 。它描述的是在这个计算平台上，单位内存交换最多用来进行多少次计算。单位是 FLOPs/Byte 。

+   **模型的理论性能** `P` ： 我们最关心的指标，即模型 在计算平台上 所能达到的每秒浮点运算次数（理论值） 。单位是 FLOPS or FLOP/s 。

如下图所示，`Roofline` 描述了 模型在一个计算平台的限制下，到底能达到多快的浮点计算速度 ，即 算力 决定“屋顶”的高度（绿色线段）， 带宽 决定“房檐”的斜率（红色线段）。

![](img/dc79e6e1ebeada2172a779fdedd94748.png)

`Roofline` 划分出的两个瓶颈区域，即

![](img/7ff9509de6345df5898a88cdf03c7e01.png)

+   **计算约束** —— 此时 HBM 访问所花费的时间相对较低，不管模型的计算强度 `I` 有多大，它的理论性能 `P` 最大只能等于计算平台的算力 `π` 。例如，具有较大内维数的矩阵乘法和具有大量通道的卷积。

+   **带宽约束** —— 当模型的计算强度 `I` 小于计算平台的计算强度上限 `Imax` 时，由于此时模型位于“房檐”区间，因此模型理论性能 `P` 的大小完全由计算平台的带宽上限 `β` （房檐的斜率）以及模型自身的计算强度 `I` 所决定。例如，`elementwise` 操作 (如 `activation`, `dropout` 等) 和 规约操作 (如 `sum`, `softmax`, `batch normalization`, `layer normalization` 等)。

在 `self-attention` 中，计算速度比内存速度快得多，因此进程 (操作) 越来越多地受到内存 (HBM) 访问的瓶颈。因此，`FlashAttention` 论文的目标是尽可能高效地使用 `SRAM` 来加快计算速度

### 1.2 `FlashAttention` 的核心思想及细节推导

首先我们回顾一下标准 `Attention` 的操作：

![](img/df537c86c88b4375dd8f11851316596d.png)

其中 `Q`, `K` （ `N` 表示序列长度， `d` 表示维度），上述公式可拆解为：

![](img/e343743b67202bd5f0fca0632e84b5c3.png)

其中 `S`, `P` （对于 decoder 来说还有 `mask`）的空间复杂度都是 `O(N^2d)` ，另外还有几个带宽约束的操作：对 `S` 的 `scale`, `mask` 和 `softmax` 操作，对 `P` 的 `dropout` 操作。下图算法展示了 HBM 与 SRAM 之间的数据传输过程。

![](img/a911c891289b771b02725b1fe2e01b94.png)

#### 1.2.1 `FlashAttention` 的优化思路

从上面的分析可以看出， `O(N^2d)` 复杂度的矩阵对 HBM 及其重复读写是一个主要瓶颈。要解决这个问题，需要做两件主要的事情：

+   在不访问整个输入的情况下计算 `softmax`

+   不为反向传播存储大的中间 `attention` 矩阵

为此 `FlashAttention` 提出了两种方法来分布解决上述问题：`tiling` 和 `recomputation` 。

+   `tiling` - 注意力计算被重新构造，将输入分割成块，并通过在输入块上进行多次传递来递增地执行 `softmax` 操作。

+   `recomputation` - 存储来自前向的 `softmax` 归一化因子，以便在反向中快速重新计算芯片上的 `attention`，这比从 HBM 读取中间矩阵的标准注意力方法更快。

由于重新计算，这确实导致 FLOPs 增加，但是由于大量减少 HBM 访问，`FlashAttention` 运行速度更快 (在 GPT-2 上高达 7.6 倍)。下面将详细推导 `FlashAttention` 的细节。

该算法背后的主要思想是分割输入，将它们从慢速 HBM 加载到快速 SRAM，然后计算这些块的 `attention` 输出。在将每个块的输出相加之前，将其按正确的归一化因子进行缩放，从而得到正确的结果。

下面将分别讨论 `tiling` 和 `recomputation` 的正确性：

#### 1.2.2 `Tiling` 与前向计算

上述算法中除了线性操作和 `elementwise` 外，分块计算注意力的关键部分是 `softmax` 的分块计算。向量的 `softmax` 可以计算为：

![](img/aceb5147566778a8215dee7c972b53e5.png)

其中 `x` 可分解为 ... 那么则有 ...

那么可以通过如下构造的方式，使得 `f(x)` 的结果与分块前保持统一：

的构造方式同理：

由此 `softmax(x)` 的结果自然与分块前保持统一：

`softmax` 的分块计算解决后，实际上其他部分的分块计算就简单多了，那么我们可以看下完整的 decoder 的 `attention` 的计算过程：

那么 `FlashAttention` 前向过程的算法可描述如下：

![](img/2e519ba803a13b2dae06666097b1aa0d.png)

上述算法并没有增加额外计算（只是将大的操作拆成多个分块逐步计算），因此其算法复杂度仍为 `O(N^2d)` ，另外由于增加了变量 `l`, `m` ，因此空间复杂度增加 `O(N)` 。

其中第 15 行需要专门来推导一下，为简洁起见，先不考虑 `mask` 和 `dropout` 操作：

其核心思想仍是使用分块的计算更新迭代以得到全局的结果。那么至此我们厘清了前向计算的整个过程，接下来我们将考虑反向计算的过程。

![](img/de85f76f02af855e0d786988e71be7c3.png)

#### 1.2.3 `Recomputation` 与 反向计算

`FlashAttention` 的向后传递需要 `Q`, `K`, `V` 矩阵来计算 `Q`, `K`, `V` 的梯度。然而由于空间复杂度是 `O(N^2d)` ，`Q`, `K` 和 `V` 没有显式存储。解决办法是使用输出 `O` 和 `softmax` 归一化统计 `l, m` ，我们可以利用 SRAM 中的 `Q`, `K`, `V` 重新计算 `Q`, `K`, `V` 矩阵。这个过程使用更多的 flop，由于减少 HBM 访问，重新计算步长也加快了反向传播的速度。

首先回顾一下标准 `Attention` 的反向的求导过程：

注意力机制的公式为 `Attention(Q, K, V)` ，反向传播时，需要根据损失函数 对模块输出的导数 `dO` (即 `∂L/∂O`)，进而求出其对输入的导数 `dQ, dK, dV` (即 `∂L/∂Q` 等)。这里，令 `P = softmax(S)`，则其求导过程如下：

由 `dP` 则 `dS` ，即 `dS` ，其余算法类似

![](img/1709bbe612137a8bef69649032e43f7b.png)

`FlashAttention` 将 `dO` 的计算拆分成若干个子部分 `dO_i` 的计算，以使得可以使用 `Tiling` 技术：

矩阵的大小是 `O(N^2d)` 量级，为了减小内存的消耗， `FlashAttention` 在反向传播时重新计算 `S` 而非在前向传播时保存：

式中 `l` 由前向传播时保存，而 `m` ...

根据之前推到同理可得到中间变量 `dS` 的导数：

而 `dS`, `dP`, `dV` ，于是：

拓展 `dS` 到 :

这里还需要求出 `dV` :

上式中， `S` 由重计算得到， `l` 则可更具体表示为：

最后是求 `dQ` :

![](img/fb2ec83f4f3c77aa64e618b6a5bd1343.png)

以上则完成反向过程的求导。

## 二、`FlashAttention` 实践与性能分析

在此采用 `xformers` 的 `memory_efficient_attention` 的实现，测试用例如下：

```
import torch
from xformers import ops as xops
import time

bs = 32
seq_len = 512
n_head = 16
head_dim = 64

query_states = torch.randn((bs, n_head, seq_len, head_dim), dtype=torch.float16).cuda()
key_states = torch.randn((bs, n_head, seq_len, head_dim), dtype=torch.float16).cuda()
value_states = torch.randn((bs, n_head, seq_len, head_dim), dtype=torch.float16).cuda()

flash_query_states = query_states.transpose(1, 2)
flash_key_states = key_states.transpose(1, 2)
flash_value_states = value_states.transpose(1, 2)

start_time = time.time()
flash_attn_output = xops.memory_efficient_attention(
    flash_query_states, flash_key_states, flash_value_states,
    attn_bias=xops.LowerTriangularMask()
)
print(f'flash attention time: {(time.time()-start_time)*1000} ms')
```

为验证其精确性，需要将其结果与标准 `attention` 对比精度，验证精度的代码如下：

```
import math
import torch.nn as nn

attention_mask = torch.tril(torch.ones((seq_len, seq_len), dtype=torch.bool)).view(1, 1, seq_len, seq_len)
attention_mask = attention_mask.to(dtype=torch.float16).cuda()  # fp16 compatibility
attention_mask = (1.0 - attention_mask) * torch.finfo(torch.float16).min

def standard_attention(query_states, key_states, value_states, attention_mask):
    attn_weights = torch.matmul(query_states, key_states.transpose(2, 3)) / math.sqrt(head_dim)
    attn_weights = attn_weights + attention_mask

    # upcast attention to fp32
    attn_weights = nn.functional.softmax(attn_weights, dim=-1, dtype=torch.float32).to(query_states.dtype)
    attn_output = torch.matmul(attn_weights, value_states)
    attn_output = attn_output.transpose(1, 2)
    return attn_output

start_time = time.time()
attn_output = standard_attention(query_states, key_states, value_states, attention_mask)
print(f'standard attention time: {(time.time()-start_time)*1000} ms')

print(torch.allclose(attn_output, flash_attn_output, rtol=2e-3, atol=2e-3))
```

下面比较二者的性能差异，即比较不同参数下二者 `Latency` 的差异 (测试设备为 1 块 A6000，`Latency` 数据为 100 次结果的平均值)：

| batch size | seq_len | n_head | head_dim | standard latency (ms) | flash latency (ms) | speedup |
| --- | --- | --- | --- | --- | --- | --- |
| 32 | 512 | 16 | 64 | 0.242 | 0.183 | 1.323x |
| 64 | 512 | 16 | 64 | 0.555 | 0.272 | 2.041x |
| 128 | 512 | 16 | 64 | 1.172 | 0.444 | 2.637x |
| 256 | 512 | 16 | 64 | 5.043 | 1.644 | 3.067x |
| 64 | 256 | 16 | 64 | 0.112 | 0.136 | 0.822x |
| 64 | 1024 | 16 | 64 | 5.414 | 1.460 | 3.707x |
| 64 | 2048 | 16 | 64 | 63.690 | 43.692 | 1.458x |
| 64 | 512 | 32 | 64 | 1.177 | 0.442 | 2.663x |
| 64 | 512 | 40 | 64 | 3.179 | 1.018 | 3.123x |
| 64 | 512 | 96 | 64 | 22.083 | 4.771 | 4.628x |
| 64 | 512 | 16 | 128 | 0.138 | 0.0959 | 1.442x |
| 64 | 512 | 16 | 256 | 0.134 | 0.0993 | 1.357x |

## 三、`PagedAttention` 原理及实践

`vLLM` 主要用于快速 LLM 推理和服务，其核心是 `PagedAttention`，这是一种新颖的注意力算法，它将在操作系统的虚拟内存中分页的经典思想引入到 LLM 服务中。在无需任何模型架构修改的情况下，可以做到比 HuggingFace Transformers 提供高达 24 倍的 `Throughput`。

### 3.1 `PagedAttention` 的基本原理

`vLLM` 具有诸多特点，其快速体现在:

+   最好的服务吞吐性能

+   使用 `PagedAttention` 优化 `KV cache` 内存管理

+   动态 `batch`

+   优化的 CUDA kernels

其易用性体现在:

+   与 HuggingFace 模型无缝集成（目前支持 GPT2, GPTNeo, LLaMA, OPT 系列）

+   高吞吐量服务与各种 decoder 算法，包括并行采样、`beam search` 等

+   张量并行 (TP) 以支持分布式推理

+   流输出

+   兼容 OpenAI 的 API 服务

`PagedAttention`: 如何解决 GPU 显存瓶颈

该研究发现，在 `vLLM` 库中 LLM 服务的性能受到内存瓶颈的影响。在自回归 `decoder` 中，所有输入到 LLM 的 `token` 会产生注意力 `key` 和 `value` 的张量，这些张量保存在 GPU 显存中以生成下一个 `token`。这些缓存 `key` 和 `value` 的张量通常被称为 `KV cache`，其具有以下特点：

+   显存占用大：在 `LLaMA-13B` 中，缓存单个序列最多需要 1.7GB 显存；

+   动态变化：`KV` 缓存的大小取决于序列长度，这是高度可变和不可预测的。因此，这对有效管理 `KV cache` 挑战较大。该研究发现，由于碎片化和过度保留，现有系统浪费了 60% - 80% 的显存。

