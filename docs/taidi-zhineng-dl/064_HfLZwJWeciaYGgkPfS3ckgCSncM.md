
# Hot Chips

> 来源：[https://learning.feishu.cn/docx/Xspad9h8LooBU7xQtklcBBrInLh](https://learning.feishu.cn/docx/Xspad9h8LooBU7xQtklcBBrInLh)

分布式 `SRAM`

`core` 外的互联 `topo`

编译器规划 `dataflow`，`core` 内的可编程性高。

核 & 加 `core`。

`nvlink`。

加 `SRAM`。`GDDR xxx`

`3d-DRAM -> HBM`

`dojo`。编译器调度的难易度。

三星的存储。

![](img/780254bce951b25399bbb74369e18ceb.png)

# Groq

`DRAM` 是一个中间点。

分布式的。

![](img/a7f58309450234fc9bd57e4cc220550a.png)

1. 去除了所有非必需的控制逻辑，将所有控制交由软件编译器完成，从而优化芯片面积分配，实现更高的单位面积算力。

1. 抛弃 `DRAM`、`HBM` 及 `CoWoS` 封装工艺，直接使用 `SRAM` 高性能存储，保持较高的内存带宽同时让级联扩展更容易，能够支持更大密度的集群配置

1. 静态和动态分区、以及优化的 `SIMD` 功能单元布局，提供了一种高度定制的计算环境，使得编译器能够在编译时完全控制硬件，实现了对执行流程的精细管理和优化。这种方法不仅增强了计算的确定性，而且通过减少运行时的不确定性，提高了效率和性能。

1. 320元素长度的 `SIMD` 单元、矩阵单元 `MXM`、用于逐点运算的 `vector` 单元、用于数据重塑的 `SXM` 单元以及处理片上存储的存储器单元，这些单元被简化成易于控制的构建块。

### 为什么级联更容易了？

![](img/55ec8021ff61e6298532d284bfa3b453.png)

Groq 采用空间方向排列，功能单元相邻布置，通过彼此之间传递操作数和结果来协作，形成一种称为“链条化”的高效运作方式，使得一个功能单元的输出可以直接连接到相邻下游功能单元的输入。其核心是大型 `MXM` 模块，包含 409,600 个乘加器，利用芯片上的数据并行处理能力，提供每平方毫米超过 1 TeraOps 的计算密度。

![](img/e534ccce9b7be651008c0edc8dab05da.png)

![](img/e5640ecd91bb7f52bc34e9cde16340d7.png)

`SRAM` 的速度，是 `HBM` 的 20 倍。

Groq 的 `LPU` 采用的时序指令集计算机（Temporal Instruction Set Computer, `TISC`）架构，与依赖 `HBM` 进行高速数据传输的 `GPU` 在工作原理上有本质的不同。

`TISC` 架构的一个关键优势在于其减少了从内存中加载数据的频率。

1. `distributed` 趋势，从 一维到 2 维。为什么不是 `hierarchy`？

1. `ring`，`mesh`，`torus`，`hierarchy` 为什么是递进的关系，理论基础。

### `mesh` 和 `torus` 的区别

`torus` 就是在 `mesh` 上多加了个环，而且是单向的。

`mesh` 是双向的。

`mesh` 不就是 `crossbar`？

4 个 `memory model` 的名字。

搜索最佳调度的成本，急剧下降。

软件 2 级分配。

`tt` 的架构。核的阵列。

`IMT` - interleave multiple thread

`SMT` & `IMT` 就是 `GPGPU` 的概念。

多核，`SMT`

单核超标量

单核单线程标量

多核改变 `memory` 结构。

![](img/8f4255b4666dfedd326f7fe715f3b24d.png)

1. 给定 64 个 `tiles/node`，有哪些拓扑结构可选？

1. 每个 `tile` 内部如何构建微架构？

1. 拓扑结构 与 `tile/node` 微架构如何平衡？

1. 是否能定义一种 `tiles/compiler` 有好的编程模型（`programming model`）？

1. `tile/node` 之间的 `packet` 如何定义？什么结构？

## 拓扑结构

1. 模型都是大力出奇迹。某个结构的反复重复。

1. 所以，拓扑也希望是某个结构的反复重复。

1. 结构的每个 `node`，内部适配模型的 `building block`。比如，`resnet` / `decoder`

1. `node` 内部，要做张量并行。

### 3 层的话

1. 第 1 和 3 层，用 `ring` 或 单向 `torus`。

```
graph: dataflow (DAG) -> node: op

op-level

instruction-level (LLVM intrinsic)

graph: conv -> bn -> relu

op-level: TVM / torch compiler / linalg - Loop Level IR

for - loop

for - loop

compute-block (add/mul/exp/sin/cos) 初等代数运算

LLVM intrinsic

add/mul -> a list of hardware instruction
```

# 周永权 - 从 `GPGPU` 到 `DSA`

周永权

跟着 NV Hopper 学异构：从 `GPGPU` 到 `DSA` https://zhuanlan.zhihu.com/p/487076260

![](img/782d188ac7e74483593f3069318fa00e.png)
