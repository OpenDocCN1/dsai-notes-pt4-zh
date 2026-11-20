
# Softmax 算子加速

> 来源：[https://learning.feishu.cn/docx/JLeXdItWxoH9acxv9bLcOVZgntb](https://learning.feishu.cn/docx/JLeXdItWxoH9acxv9bLcOVZgntb)

## 数学推导

精度上，都是 `safe softmax`。

性能上，普遍使用 `online softmax`。

| 算法 | Scan Pass | IO/Element | 备注 |

| --- | --- | --- | --- |

| Naive | 1.  计算分母的 `sum` | 共 3 次 |  |

| Safe | 1.  计算 `max` | 4 | 3 轮 IO，成为模型的性能瓶颈 |

| Online | 1.  计算 `max` & `sum` | 3 |  |

### Naive Softmax

![](img/e2452880ab33c89d528cd2d2c8719ceb.png)

`softmax` 的数学语义：将输入向量转换为概率分布。

直接计算 `x_i / sum(x_i)` 无法处理 `x_i` < 0 的情况。

`exp(x)` 的值域是[0, +∞)，且单调连续，问题解决。

### Safe Softmax

`exp(x)` 的值域是[0, +∞)，超出 `float` 表示范围时会 `overflow`。

的值域是[0, 1]，不会发生 `underflow` 或 `overflow`，提供了很好的数值稳定性。

![](img/cc30b2cf6eb675bb8c70d480fde7b445.png)

### Online Softmax

核心思想的数学等价性:

`softmax(x) = softmax(x - constant)`

用 1 轮 IO 同时完成 `max` & `sum` 的计算，也减少了一轮内存读写。

注意，理论的加速比是 1.3。因为，每个元素的读写次数，从 `3 load + 1 store` 将至 `2 load + 1 store`。

代价是，每个 `element` 增加 2 个计算：`1 mul + 1 exp`。新增 `cost` 不高。

但，要小心 `SFU` 算力瓶颈的风险。

![](img/e842e38c6d34fda7863e7f1d38eeea15.png)

### exp 加速 - 底数替换

数学上，

代码上，

```
exp(x) = exp2(x * 1.4426950408889634)  # 精确的数学变换
```

注：`exp` 的底数是 `e`，`exp2` 的底数是 `2`。

在某些专用硬件上，`exp2` 可能比 `exp` 有更好的性能表现：

+   `NPU`/`TPU` 可能有专门的 `2^x` 硬件单元

+   `exp2` 的实现可能更简单，电路面积更小，功耗更低。

虽数学上精确等价，实际测试中可能观察到：

+   微小数值差异 (`1e-7` 到 `1e-6` 级别)。特别是 `input` 为 `fp16` 或更小时。

+   在梯度反向传播中可能累积误差。`exp(x)` 的导数是 `exp(x)`。而 `exp2(x * k)` 的导数是 `k * ln(2) * exp2(x * k)`

推理一般是可以用的，也是 `triton` `attention` 的推荐做法。训练慎用。

```
def exp_softmax(x, dim=-1):
    x_max = torch.amax(x, dim=dim, keepdim=True)
    x_stable = x - x_max

    exp2_x = torch.exp(x_stable)
    sum_exp2 = torch.sum(exp2_x, dim=dim, keepdim=True)
    return exp2_x / sum_exp2


def exp2_softmax(x, dim=-1):
    x_max = torch.amax(x, dim=dim, keepdim=True)
    x_stable = x - x_max

    q_scale = 1.4426950408889634  # = 1.0 / ln(2), 1.0 / math.log(2)
    exp2_x = torch.exp2(x_stable * q_scale)
    sum_exp2 = torch.sum(exp2_x, dim=dim, keepdim=True)
    return exp2_x / sum_exp2
```

## GPU 理论性能推演 - Online Softmax

Key operators:

1.  `ReduceMax`

1.  `ReduceSum`

1.  `Exp` (element wise)

1.  `Div` (element wise)

draft - 以下经验数字尚未实测证实

`Softmax` 的典型输入 `Shape` 是 `:(Batch, SeqLen)`。

`Batch` 之间相互独立。每个 `(1, SeqLen)` 上做 `reduce_max` 和 `reduce_sum`。

`SeqLen` 从小到大，存在 `warp_reduce`, `block_reduce` 等不同高性能需求，实现方案不同。

实现策略（基于 `SeqLen`）:

+   `Warp-Level`：`SeqLen` ∈ [1, 256]（32 `Threads` * `elemPerThreads`）

+   `elemPerThreads` 一般最大 8。

+   `Reduce` 技术：`__shfl_xor_sync`, `__shfl_down_sync` 等 `warp shuffle` 指令。

+   `Block-Level`: `SeqLen` ∈ [128, 8192]（32 * `elemPerThreads` * `warpsPerBlock`）

+   `warpsPerBlock` 一般取 4。受限于 `Shared Memory` 容量和 `Block` 线程数限制等。

+   `Reduce` 技术：`warp` 间 `reduce` 用 `Shared Memory`, 考虑 `double buffer`。

+   `Tiled/Online`: `SeqLen` ∈ [2048, ∞]

+   `Reduce` 技术：循环加载、迭代归约、共享内存缓存。

+   核心：减少 `Global Memory`，`Shared Memory` 反复读写。避免 `register spill`。

注意：

1.  `SeqLen` 下限暂时随便写的。主要想表达，实际的最佳分界线一般是有交叠的。

1.  一般不要发生 `block` 间 `reduce`，需通过 `global memory` 交换数据。如果是 `triton` `kernel`，还要实现第 2 个 `reduce kernel`。慢且复杂

1.  如果 `Batch` 小且 `SeqLen` 很大 (比如 `1M`)，并行的收益超过 `block` 间通信，则 `block` 间 `reduce` 则有收益。即，开启 `SeqLen` 并行。

### Warp-Level 性能模型

计算复杂度：

```
总操作数 = Batch × SeqLen × (2 load + 1 store + 3 arithmetic)
带宽需求 = Batch × SeqLen × 4 bytes × 3 (读+写+临时)
```

瓶颈分析：

+   小 `SeqLen`：指令吞吐是瓶颈

+   接近 256：内存带宽开始成为瓶颈

`SeqLen` ∈ [1, 256]（32 `Threads` * `elemPerThreads`）

关键是 `elemPerThreads` 最大值的选取。

硬约束是 `Max Register per Thread`。（`Nvidia PTX` 约定了 255 个 32bit `reg`）

实践上，当 `elemPerThreads` 超过 8 时，还要考虑 `Occupancy`，`warp` 指令调度效率，`128 bytes cache line` 对齐等因素。

> `Nvidia PTX` 约定了 `Max 32 bit Register per Thread` 是 255 个。

> （硬件上，可能是 256 个 `reg`，保留了 1 个给系统或编译器）。

TODO - `PTX` 实际编译一下，看看指令数量和寄存器需求。

### Block-Level 性能模型

资源约束方程：

```
共享内存需求 = warpsPerBlock × (2 × 4 bytes)  # 存储各warp的max/sum
           = 4 × 8 = 32 bytes

寄存器需求 = threadsPerBlock × registersPerThread
         = 128 × ~20 = ~2.5KB
```

性能特征：

+   计算与通信重叠良好

+   `Shared Memory` `Bank Conflicts` 可能成为瓶颈

### Tiled/Online Softmax 性能模型 - TODO

## GPU Benchmarks

### V100 - from Softmax Paper

`batch = 4000`

`online softmax` 做到了与 `naive` 基本相同的速度。

at that point `L1` and `L2` `cache thrashing` starts to make all three algorithms limited by the `DRAM` bandwidth

![](img/95cb71191342274d9da1a062a605549d.png)

`batch = 10`

`vector size` 达到 `10k` 时，才开始体现优势。

![](img/e3ab3c7bd61d0e9ff11d1465bfee76c1.png)

### RTX 3090 - OneFlow

2020 年

测试环境是 `GeForce RTX™ 3090 GPU`，数据类型是 `half`，`Softmax` 的 `Shape` = `(49152, num_cols)`，其中 `49152 = 32 * 12 * 128`，是 `BERT-base` 网络中的 `attention` `Tensor` 的前三维，我们固定了前三维，将最后一维动态变化，测试了从 `32` 到 `32768` 不同大小的 `Softmax` 前向 `Kernel` 和反向 `Kernel` 的有效显存带宽。从上面两张图中可以看出 `OneFlow` 在多数情况下都可以逼近理论带宽（`800GB/s` 左右，与 `cudaMemcpy` 的访存带宽相当。官方公布的理论带宽为 `936GB/S`）。并且在所有情况下，`OneFlow` 的 `CUDA Kernel` 的有效访存带宽都优于 `cuDNN` 的实现。

`warp level` 允许的最大 `SeqLen=1024`，反推出 `elemPerThreads=32`。

来源：https://github.com/Oneflow-Inc/oneflow/pull/4058

| cols | size(GB) | t warp(us) | t smem(us) | T uncached(us) | T cudnn(us) | bw warp | bw smem | bw uncached | bw cudnn |

| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

| 32 | 0.00585938 | 18 | 103 | 1230 | 406 | 325.5208 | 56.88714 | 4.76372 | 14.43196 |

| 64 | 0.01171875 | 22 | 103 | 1210 | 413 | 532.6705 | 113.7743 | 9.684917 | 28.3747 |

| 128 | 0.0234375 | 32 | 112 | 1210 | 427 | 732.4219 | 209.2634 | 19.36983 | 54.88876 |

| 256 | 0.046875 | 62 | 133 | 1240 | 454 | 756.0484 | 352.4436 | 37.80242 | 103.2489 |

| 512 | 0.09375 | 122 | 201 | 1260 | 506 | 768.4426 | 466.4179 | 74.40476 | 185.2767 |

| 1024 | 0.1875 | 244 | 269 | 1360 | 613 | 768.4426 | 697.026 | 137.8676 | 305.8728 |

| 2048 | 0.375 |  | 484 | 1660 | 828 |  | 774.7934 | 225.9036 | 452.8986 |

| 4096 | 0.75 |  | 964 | 2520 | 1350 |  | 778.0083 | 297.619 | 555.5556 |

| 8192 | 1.5 |  | 2230 | 2920 | 4210 |  | 672.6457 | 513.6986 | 356.2945 |

| 16384 | 3 |  |  | 4720 | 10690 |  |  | 635.5932 | 280.6361 |

| 32768 | 6 |  |  | 11950 | 20940 |  |  | 502.0921 | 286.533 |

![](img/796887533ecd12eb3f0096e73e30f923.png)

## CPU Softmax

### AVX 指令的 Softmax 实现

Try to enable `AVX512` instruction

`softmax` 的函数方程并不复杂，实现的关键点在于如何实现 `exp` ，下面的实现中参考了

https://stackoverflow.com/questions/48863719/fastest-implementation-of-exponential-function-using-avx
