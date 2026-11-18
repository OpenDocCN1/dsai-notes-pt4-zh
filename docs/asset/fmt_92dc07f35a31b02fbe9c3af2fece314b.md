
# GPGPU uArch, SIMT & FGMT

> 来源：[https://learning.feishu.cn/docx/MdtxdipQno3TPuxVoW2ccqhynmf](https://learning.feishu.cn/docx/MdtxdipQno3TPuxVoW2ccqhynmf)

| Links |
| --- |
| https://www.techspot.com/article/1874-amd-navi-vs-nvidia-turing-architecture/ |
| NVIDIA Tesla: A unified graphics and computing architecture |
| https://pages.cs.wisc.edu/~sinclair/courses/cs758/fall2019/handouts/lecture/cs758-fall19-gpu_uarch.pdf |

本文只涉及 `CUDA Core` 部分的 `SIMT` 架构。

TensorCore 见：[Nvidia TensorCore](https://dwpl6xgouw.feishu.cn/wiki/PnYmw4KcdiPBphkJBFdcqdDxnke)

## SIMT and SIMD

![](img/9437460fb92dd46667237e7c68eb945b.png)

### SPMD on SIMT machine

2 个关键词：

1. `programming model` 是 `SPMD`（single program，multiple data）
1. `execution model` 是 `SIMT`（single instruction，multiple thread）

关键的硬件概念：

| 关键概念 | Notes |
| --- | --- |
| `thread` | 1. `programmer` 控制 `SIMT` 中的一个 `scalar thread`。 |
| `warp` | 1. `hardware` 的概念，执行一组相同 `instruction` 的 `thread`。 |

coding tips (`CUDA` / `OpenAI Triton`):

代码只会写，`number of block` & `number of thread per block`。

`software` 只会控制到 `thread blocks`。`hardware` 会 decompose 成尽可能多的 `warps`。

`memory access` 用 `thread id` 找对应的 `data elements`。在 `triton` 上，`GPU` 里就是 `program id`。

![](img/3696be470351b80e5b928a71175ab892.png)

### SIMT vs SIMD

`SIMD` 的弊端：

1. 每个 `cycle` 并行处理的数据，必须连续。
1. 需要程序员计算数据的 `address`，感知并适配 `SIMD` 指令的 `vector length`。
1. `ISA` 中需要设计单独的 `vector` 指令。`SIMT` 只需要 `scalar` 指令。

`SIMT` vs `SIMD` 的弊端（挑战）：

1. 寄存器利用率低。`block` 内的每个 `thread`，都有独立的 `register`。实际 `active` 的 `thread` 只有一两个 `warp`。且，各个 `thread` 可能使用相同的数据，重复占用 `register`。
1. `memory coalescing`。每个 `thread` 的每个 `instruction`，多个 `operands` 可能都要访存，且离散。需额外优化。
1. `branch divergence`。存在 `branch` 时，`SIMT` 的机制，使得 `branch divergence` 的概率更高一些，需额外优化。

`GPU` 的底层是 `SIMD engines`。

1. `instruction pipeline` 类似 `SIMD pipeline`。
1. `Key Difference`: `programmer` 控制的是 `Thread`，不是 `SIMD instructions`。
1. `warp` 是一个 `hardware` 的概念，执行一组相同 `instruction` 的 `thread`。

`SIMT` 的每一个 `thread`，都是 `scalar` 操作。`hardware` 会 `dynamically group` 成 `warps`。

把 `thread group` 成 `warp` 是 `hardware` 动态去做的。可以 `maximize benefit`。

![](img/25e5f4fa67d068f47df6344056adadd9.png)

### SIMT on SIMD (RVV)

所以，如果用 `rvv` 实现 `GPGPU`。

`rvv lanes` (`rvv length` / size of element type) 应该等价于 `warp size`，也就是 `num threads per warp`。

那么，一个 `block` 的 `thread` 数量，如何界定呢？底层 `llvm` 又如何把 `block_size` 翻译成 `num of warps`。

思路，`rvv` 最近的 `SRAM`，假设叫 `DLM`，当作 `vGPR` 使用。那么，就可以根据这个 `DLM` size 来计算最大允许的 `block size`。实际指定的 `block size`，自然希望是 `rvv lanes` 的倍数。

这样，`triton` 一次把一个 `block` 的数据 `load` 进 `DLM`。`llvm` 再根据 `rvv length` 做 `software pipelining`，用多个 `cycle` 完成一个 `block` 的计算。

![](img/60197832be00721683cf3d336690b2a7.png)

![](img/dd2421237a4feb6b4006e16aa754f83c.png)

`SIMT Core` (`CUDA Core`)

![](img/389ec784d6711e2c96c05607ac4b8699.png)

`SIMD width` 甚至可以小于 `warp size`。`Fermi 16`，`Volta 32`。

这种设定，可能有助于 `mitigate control flow divergence`。

有点意思。

![](img/8c61b8134dc2a558162042c895911dd7.png)

![](img/54631ba63f572fbf135d015a3c4b8501.png)

![](img/5eb2fdded6ffce5ac2bd848670bb65dc.png)

## GPU 基本概念 - FGMT

`GPU` 不仅是 `SIMD`，还是 `FGMT processor`。

这是一个不太被提及，但很重要的 `hardware design` 的概念。

`FGMT` 全称：`Fine-Gained Multithreading`。

每个 `thread` 有独立 `hardware` 资源保存 `context` 信息。包括 `PC`（`program counter`）和 `register` 等。

`hardware` 成本增加，但复杂度降低，不用 `branch prediction` 等。

`pipeline` 更好，`throughput` 更高。会牺牲单个 `thread` 的 `latency`。

### Basic Idea

处理 `data dependency` 的一种方法。

idea: Do something else, don't even bother to detect `data dependency`, since you need to do something else anyway。

![](img/6b66611d21576f442a01fddb10b4c8f8.png)

右下图，不同 `color` 是不同 `threads`。每个 `cycle` 从不同的 `thread` 做 `fetch`。

代价是

1. `hardware context` 比较大。每个 `thread` 都有自己的 `PC+registers`。
1. `single thread` 的 `latency` 高。
1. 如果 `thread` 数量不足，就会有 `throughput loss`。

-- 重新认识 `GPU` 是高 `throughput`，高 `latency` 的 feature。

![](img/5727c2370b135275fccadf98b0278406.png)

![](img/beff1ea53405d36864083366a84f03f6.png)

不同的颜色，是不同的 `thread`。有自己的 `PC`（`Program counter`）和 `register`。

`thread select` 选一个 `PC`，然后选出对应的 `GPR`（`general purpose register`）

### Hardware Example

![](img/949190fc3a2e333987bebd01bcab3463.png)

一个真实的 `machine`，

`thread` 相关的很多资源，都是 `thread` 数相关（`x4`），

比如，`PC`，`instructions buffer`，`register file`，`store buffer` 等。

`hardware` 硬件成本，是增加的。

省掉了 `hardware` 复杂度，不用 `branch prediction` 等。

![](img/b0dd2d318dd4772b9ee162381422e253.png)

### Pros and Cons

![](img/58f68868c344188966afdc0c968f1944.png)

资源竞争，指的是 `thread` 之间争抢 `memory access` 等。

## GPGPU uArch

### Warp-Level FGMT

此处是 `multithreading of warps`，不是 `multithreading of individual threads`。

![](img/e6bbe01630b2cc2060e4255636c62dc5.png)

`warp-level FGMT` 的特点

![](img/c63b2765bbf61858fdb555af0d453d51.png)

`shader core` 应该就是 `nv` 的 `sm`（`streaming processor`）

![](img/abcea055cbc7df8ccf98b53e0db607ec.png)

![](img/994e5c28a372e5b62f205604bd3cc581.png)

`warp-level` 跟 `thread` 一样，也有 `time-space` 并行。

1. 多个 `warp` 本身是 `space` 纬度的并行。
1. 用了 `FGMT` or `interleave execution` 以后，在 `time` 上也并行了。

![](img/ff488ef4d428b429b52c91d8bac761cd.png)

![](img/23eb4afbbb70df9c9286417739a24e30.png)

`8 lanes` -> `8 functional unit`

每个 `warp` 有 `32` 个 `thread`。

每个 `cycle` 可 `issue 1 warp`，完成 `24` 个 `operation`。

执行策略，可以是：

1. 每个 `cycle` `issue 8` 个 `thread`。`4` 个 `cycle` 完成 `1` 个 `warp` 的 `issue`。
1. 每个 `thread` 的执行，可以分多个 `stage`，在对应的 `lane` 上 `pipeline` 起来。

![](img/80ce0a37ef5f19a1b9ecc4e116afe6eb.png)

### SIMT Programming

`memory access` 用 `thread id` 找对应的 `data elements`。

![](img/9a892c51022a915448fdea999fd72c77.png)

`programmer` 不管 `warp`，而是控制到 `thread blocks`。

`thread blocks` 是 `software` 的概念。

`hardware` 会 decompose 成尽可能多的 `warps`。

代码只会写，`number of block` & `number of thread per block`。

左边是顺序执行的 `code`，右边 `cpu + gpu` 的 `thread` 情况示意图。

![](img/da326b4540ac039d674ffe57ca55c645.png)

![](img/d827f1ae6919e953c981dfbd8251bafd.png)

![](img/5afd2f7a2cb3fec6308a7f5f2e3dfeab.png)

`block` 是 `software` 概念，`programmer` 控制 `number of threads`

`warp` 是 `hardware` 概念，`hardware` 控制 `number of threads`。

运行时，`hardware` 会自动把 `block` 中的 `threads` decompose 到多个 `warp` 里。

![](img/6d45cbc2c43beeee71f84a5ea7f22f81.png)

### Warp-based SIMD

传统 `SIMD`：只有 `1 thread`，`1 cycle` 处理一批数据，`sequential instruction execution` (`lock-step`)。`ISA` 显式包含 `simd` 指令。

`warp-based SIMD`: 以 `simd` 的 manner 执行。由多个 `scalar thread` 处理数据，每个 `cycle` 的效果，类似 `simd` 指令。但，因为 `hardware register` 和 `thread` 很多，不要求 `lock-step`。

传统 `SIMD`，`sw` 必须知道并显式指定 `vector length`。`warp-based SIMD`，不需要，`sw` 不知道 `vector length`。

`warp-based SIMD`，就是 `SIMD hardware` 上的一种 `SPMD programming model`。

所以，如果用 `rvv` 实现 `GPGPU`。

`rvv length` 应该等价于 `warp size`，也就是 `thread/warp`。

那么，一个 `block` 的 `thread` 数量，如何界定呢？底层 `llvm` 又如何把 `block_size` 翻译成 `num of warps`。

思路，`rvv` 最近的 `SRAM`，假设叫 `DLM`，当作 `vGPR` 使用。那么，就可以根据这个 `DLM` size 来计算最大允许的 `block size`。实际指定的 `block size`，自然希望是 `rvv length` 的倍数。

这样，`triton` 一次把一个 `block` 的数据 `load` 进 `DLM`。`llvm` 再根据 `rvv length` 做 `software pipelining`，用多个 `cycle` 完成一个 `block` 的计算。

![](img/41dec05d616bdf8dd7bf7f40b69a0a8e.png)

![](img/4f8e5cf7f21490ac572fb60730c62352.png)

`warp` 内，不同 `thread` 可以走不同的 `control flow`。

![](img/da68af0613c847053de3e0bacd89b457.png)

原理是，通过 `mask`，自动选出满足 `branch condition` 的 `thread` 组 `warp`。

执行效果如右图。有些 `IDLE` 的 `thread`，效率有所降低，但，至少能用。

这种 `IDLE` 是 `GPU programming` 中，需要 `programmer` 去尽力避免的。

![](img/3f4ac92fe088b523827a6b827cc8049b.png)

### perf & benchmark

关键指标之一：`IPC`

![](img/f38ac185439f0cde9957f5ea9ee9aa10.png)

## Key Challenges

### memory coalescing

[由于每个 `thread` 有](https://www.zhihu.com/search?q=Sharememory%20&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3374307130%7D)自己的存储，所以可以并行存取[，相对一个访存入口竞争的设](https://www.zhihu.com/search?q=share%20memory%20&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3374307130%7D)计，性能可以提高。

优势比较明显，劣势在于考虑 `GPU` 存储的多级性，`thread` 间独立并行访问，提高了设计难度。

`GPU` 的存储层次一般至少包括以下（离 `core` 由近至远）：`share memory`、`L1`、`L2`、`DRAM`。最近的 `share memory` 在 `SM` 内，由 `SM` 内 `core` 共享（如 `regfile`），最远的 `DRAM` 则在外部存储器中。

提高 `DRAM` 的访存效率的途径就是合并访问，从而尽可能减少向 `dram` 访存的次数，由于 `thread` 的并行化，这一块请求合并的动态性要求更高。

`Sharememory` 由多个单独的 `bank` 构成，`share memory` 需要处理多 `core` 随机访问造成的 `bank` 冲突问题。如果一个 `bank` 被两个地址访问，每个 `thread` 中其访存带宽会下降至 `1/2`。

### branch divergence

#### Introduction

`warp` 内，不同 `thread` 可以走不同的 `control flow`。`branch divergence` 导致 `GPU` 利用率降低，`2` 个解决算法：`dynamic warp merging`，`large warp` and `2-level warp scheduling`

![](img/54a975e68ab4daa13eed9fb6722f59fd.png)
