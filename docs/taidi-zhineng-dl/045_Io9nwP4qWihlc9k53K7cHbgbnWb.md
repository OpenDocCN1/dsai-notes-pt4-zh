
# AllReduce

> 来源：[https://learning.feishu.cn/docx/RNPfd8a2MofSMMxHYoIcU8b2nlb](https://learning.feishu.cn/docx/RNPfd8a2MofSMMxHYoIcU8b2nlb)

## Ring-allreduce vs. Tree-allreduce

`Ring-allreduce` achieves full bandwidth, but the downside of rings is that latency scales linearly with the number of GPUs, preventing scaling above hundreds of GPUs.

Double binary trees, which offer full bandwidth and a logarithmic latency even lower than 2D ring latency.

## Reference

GPU分布式训练: `NCCL`性能解析（二）多机通信——`Ring`, `Tree`, `CollNet`

`https://zhuanlan.zhihu.com/p/597081795`

`Blink`: Fast and Generic Collectives for Distributed ML

`https://arxiv.org/abs/1910.04940`

combine了 `ring` 和 `tree` 的 `allreduce`

`Sparse Allreduce`: Efficient Scalable Communication for Power-Law Data

`https://arxiv.org/abs/1312.3020`

![](img/bc8eedda1dd0225854788d4d330f60b1.png)

## 通信语义

`AllReduce` 诞生于 HPC 领域，`MPI`(`Message Passing Interface`, HPC领域的通信库)用多种方式实现了 `AllReduce` 元语，是深度学习领域参考的主要依据

![](img/fbb9d0492b7c4994b9b7f2ff8947865e.png)

## Ring AllReduce

百度在 2017 年提出 `Ring AllReduce`。

英伟达 的 `NCCL`, `Horovod` library by Uber.

Bringing HPC Techniques to Deep Learning

`https://andrew.gibiansky.com/blog/machine-learning/baidu-allreduce/`

code to Tensorflow

`https://github.com/baidu-research/tensorflow-allreduce/commit/66d5b855e90b0949e9fa5cca5599fd729a70e874`

![](img/552772cd53df39e87030cb523f4ca038.png)

![](img/44bb692acac0b75a89f3d02ad8a42e49.png)

## Having-Doubling AllReduce

阿里巴巴的通信库 `ACCL` 采用。

`https://ieeexplore.ieee.org/document/9462480`

![](img/fb78e2c45c80a0fe8fd73abd6f3c9806.png)

![](img/9fdcead8b28cd187a03e285524978e3b.png)

## 数学推演

主要计算通信量和时延。

两种算法的通信量总量其是一样的

![](img/68312a4c8bc0243318c3c49d7920b269.png)

## BytePS-AllReduce

字节的方案 `https://github.com/bytedance/byteps`

### 要点信息

核心思想：

1. 引入 `CPU` 节点，用于 `ReduceScatter` 操作。

1. `ReduceScatter` 和 `AllGather` 异步执行，从而缩短了数据传输时间开销。

利弊分析

+ `GPU` 上每一份数据在做 `Reducescatter` 或 `AllGather` 时，内部还会分段；这种 `ReduceScatter` 和 `AllGather` 的实现会因为和 `CPU Server` 的连接稍微影响性能；

+ 需要额外 `CPU Server` 节点来提供带宽，通常不如直接给 `P` 个 `worker` 增加一张网卡，这样理论上通信量也可以减半而且避免了 `partitioning` 和 `pipelining` 带来的 `overhead`；

+ `GPU` 和 `GPU` 之间的带宽通常是比较高的，譬如 `100Gbps`，而 `GPU` 节点和额外的 `CPU` 集群之间的带宽通常没有这么高，可能只有 `25Gbps`（英伟达现在应该已经优化通信了），这种情况 `byteps` 就不是比 `baidu ring-AllReduce` 快一倍，而是慢一倍了，因为在 `ReduceScatter` 和 `AllGather` 过程只能打满 `25Gbps` 的带宽，相比 `ring-AllReduce` 通信量减少一半，带宽减少变成`1/4`了，反而会慢一倍。

### 算法步骤

初始状态：

![](img/04072d72ea848e54b637dafd3b82f5e2.png)

第一次迭代操作：`CPU` 端聚合 `A0`、`B0`、`C0`

结果如下：

![](img/68910fbc30ea26dcf2b09622608b8cdb.png)

第二次迭代操作：`CPU` 端聚合 `A1`、`B1`、`C1`，同时 `GPU0`、`GPU1`、`GPU2` 分别异步拉取 `A0 + B0 + C0`

结果如下：

![](img/ae7bb355b10856cde84826522f262cee.png)

第三次迭代操作：`CPU` 端聚合 `A2`、`B2`、`C2`，同时 `GPU0`、`GPU1`、`GPU2` 分别异步拉取 `A1 + B1 + C1`

结果如下：

![](img/06d10b377d2dac471fa49070c23f2cc3.png)

第四次迭代操作：`GPU0`、`GPU1`、`GPU2` 分别拉取 `A2 + B2 + C2`

结果如下：

![](img/2f3d5987c9ca3e29cae741eef7ccaff3.png)

### 源码解读

`https://zhuanlan.zhihu.com/p/435438871`

`baidu ring-AllReduce` 和 `byteps-AllReduce` 实现原理及源码解读
