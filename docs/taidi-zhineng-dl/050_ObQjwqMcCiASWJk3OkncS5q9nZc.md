# GPGPU uArch, SIMT & FGMT

> 来源：[https://learning.feishu.cn/docx/MdtxdipQno3TPuxVoW2ccqhynmf](https://learning.feishu.cn/docx/MdtxdipQno3TPuxVoW2ccqhynmf)

| Links |
| --- |
| https://www.techspot.com/article/1874-amd-navi-vs-nvidia-turing-architecture/ |
| NVIDIA Tesla: A unified graphics and computing architecture |
| https://pages.cs.wisc.edu/~sinclair/courses/cs758/fall2019/handouts/lecture/cs758-fall19-gpu_uarch.pdf |

本文只涉及 CUDA Core 部分的 SIMT 架构。

TensorCore 见：[Nvidia TensorCore](https://dwpl6xgouw.feishu.cn/wiki/PnYmw4KcdiPBphkJBFdcqdDxnke)

# SIMT and SIMD

![](img/9437460fb92dd46667237e7c68eb945b.png)

## SPMD on SIMT machine

2 个关键词：

1.  programming model 是 SPMD（single program，multiple data）

1.  execution model 是 SIMT（single instruction，multiple thread）

关键的硬件概念：

| 关键概念 | Notes |
| --- | --- |
| thread | 1.  programmer 控制 SIMT 中的一个 scalar thread。 |
| warp | 1.  hardware 的概念，执行一组相同 instruction 的 thread。 |

coding tips (CUDA / OpenAI Triton)：

代码只会写，number of block & number of thread per block。

software 只会控制到 thread blocks。hardware 会 decompose 成尽可能多的 warps。

memory access 用 thread id 找对应的 data elements。在 triton 上，GPU 里就是 program id。

![](img/3696be470351b80e5b928a71175ab892.png)

## SIMT vs SIMD

SIMD 的弊端：

1.  每个 cycle 并行处理的数据，必须连续。

1.  需要程序员计算数据的 address，感知并适配 SIMD 指令的 vector length。

1.  ISA 中需要设计单独的 vector 指令。SIMT 只需要 scalar 指令。

SIMT vs SIMD 的弊端（挑战）：

1.  寄存器利用率低。block 内的每个 thread，都有独立的 register。实际 active 的 thread 只有一两个 warp。且，各个 thread 可能使用相同的数据，重复占用 register。

1.  memory coalescing。每个 thread 的每个 instruction，多个 operands 可能都要访存，且离散。需额外优化。

1.  branch divergence。存在 branch 时，SIMT 的机制，使得 branch divergence 的概率更高一些，需额外优化。

GPU 的底层是 SIMD engines。

1.  instruction pipeline 类似 SIMD pipeline。

1.  Key Difference: programmer 控制的是 Thread，不是 SIMD instructions。

1.  warp 是一个 hardware 的概念，执行一组相同 instruction 的 thread。

SIMT 的每一个 thread，都是 scalar 操作。hardware 会 dynamically group 成 warps。

把 thread group 成 warp 是 hardware 动态去做的。可以 maximize benefit。

![](img/25e5f4fa67d068f47df6344056adadd9.png)

## SIMT on SIMD (RVV)

所以，如果用 rvv 实现 GPGPU。

rvv lanes (rvv length / size of element type) 应该等价于 warp size，也就是 num threads per warp。

那么，一个 block 的 thread 数量，如何界定呢？底层 llvm 又如何把 block_size 翻译成 num of warps。

思路，rvv 最近的 SRAM，假设叫 DLM，当作 vGPR 使用。那么，就可以根据这个 DLM size 来计算最大允许的 block size。实际指定的 block size，自然希望是 rvv lanes 的倍数。

这样，triton 一次把一个 block 的数据 load 进 DLM。llvm 再根据 rvv length 做 software pipelining，用多个 cycle 完成一个 block 的计算。

![](img/60197832be00721683cf3d336690b2a7.png)

![](img/dd2421237a4feb6b4006e16aa754f83c.png)

SIMT Core (CUDA Core)

![](img/389ec784d6711e2c96c05607ac4b8699.png)

SIMD width 甚至可以小于 warp size。Fermi 16，Volta 32。

这种设定，可能有助于 mitigate control flow divergence。

有点意思。

![](img/8c61b8134dc2a558162042c895911dd7.png)

![](img/54631ba63f572fbf135d015a3c4b8501.png)

![](img/5eb2fdded6ffce5ac2bd848670bb65dc.png)

# GPU 基本概念 - FGMT

GPU 不仅是 SIMD，还是 FGMT processor。

这是一个不太被提及，但很重要的 hardware design 的概念。

FGMT 全称：Fine-Gained Multithreading。

每个 thread 有独立 hardware 资源保存 context 信息。包括 PC（program counter） 和 register 等。

hardware 成本增加，但复杂度降低，不用 branch prediction 等。

pipeline 更好，throughput 更高。会牺牲单个 thread 的 latency。

## Basic Idea

处理 data dependency 的一种方法。

idea: Do something else, don't even bother to detect data dependency, since you need to do something else anyway。

![](img/6b66611d21576f442a01fddb10b4c8f8.png)

右下图，不同 color 是不同 threads。每个 cycle 从不同的 thread 做 fetch。

代价是

1.  hardware context 比较大。每个 thread 都有自己的 PC+registers。

1.  single thread 的 latency 高。

1.  如果 thread 数量不足，就会有 throughput loss。

-- 重新认识 GPU 是高 throughput，高 latency 的 feature。

![](img/5727c2370b135275fccadf98b0278406.png)

![](img/beff1ea53405d36864083366a84f03f6.png)

不同的颜色，是不同的 thread。有自己的 PC（Program counter）和 register。

thread select 选一个 PC，然后选出对应的 GPR （general purpose register）

## Hardware Example

![](img/949190fc3a2e333987bebd01bcab3463.png)

一个真实的 machine，

thread 相关的很多资源，都是 thread 数相关（x4），

比如，PC，instructions buffer，register file，store buffer 等。

hardware 硬件成本，是增加的。

省掉了 hardware 复杂度，不用 branch prediction 等。

![](img/b0dd2d318dd4772b9ee162381422e253.png)

## Pros and Cons

![](img/58f68868c344188966afdc0c968f1944.png)

资源竞争，指的是 thread 之间争抢 memory access 等。

# GPGPU uArch

## Warp-Level FGMT

此处是 multithreading of warps，不是 multithreading of individual threads。

![](img/e6bbe01630b2cc2060e4255636c62dc5.png)

warp-level FGMT 的特点

![](img/c63b2765bbf61858fdb555af0d453d51.png)

shader core 应该就是 nv 的 sm (streaming processor)

![](img/abcea055cbc7df8ccf98b53e0db607ec.png)

![](img/994e5c28a372e5b62f205604bd3cc581.png)

warp-level 跟 thread 一样，也有 time-space 并行。

1.  多个 warp 本身是 space 纬度的并行。

1.  用了 FGMT or interleave execution 以后，在 time 上也并行了。

![](img/ff488ef4d428b429b52c91d8bac761cd.png)

![](img/23eb4afbbb70df9c9286417739a24e30.png)

8 lanes -> 8 functional unit

每个 warp 有 32 个 thread。

每个 cycle 可 issue 1 warp，完成 24 个 operation。

执行策略，可以是：

1.  每个 cycle issue 8 个 thread。4 个 cycle 完成 1 个 warp 的 issue。

1.  每个 thread 的执行，可以分多个 stage，在对应的 lane 上 pipeline 起来。

![](img/80ce0a37ef5f19a1b9ecc4e116afe6eb.png)

## SIMT Programming

memory access 用 thread id 找对应的 data elements。

![](img/9a892c51022a915448fdea999fd72c77.png)

programmer 不管 warp，而是控制到 thread blocks。

thread blocks 是 software 的概念。

hardware 会 decompose 成尽可能多的 warps。

代码只会写，number of block & number of thread per block。

左边是顺序执行的 code，右边 cpu + gpu 的 thread 情况示意图。

![](img/da326b4540ac039d674ffe57ca55c645.png)

![](img/d827f1ae6919e953c981dfbd8251bafd.png)

![](img/5afd2f7a2cb3fec6308a7f5f2e3dfeab.png)

block 是 software 概念，programmer 控制 number of threads

warp 是 hardware 概念，hardware 控制 number of threads。

运行时，hardware 会自动把 block 中的 threads decompose 到多个 warp 里。

![](img/6d45cbc2c43beeee71f84a5ea7f22f81.png)

## Warp-based SIMD

传统 SIMD：只有 1 thread，1 cycle 处理一批数据，sequential instruction execution (lock-step)。ISA 显式包含 simd 指令。

warp-based SIMD: 以 simd 的 manner 执行。由多个 scalar thread 处理数据，每个 cycle 的效果，类似 simd 指令。但，因为 hardware register 和 thread 很多，不要求 lock-step。

传统 SIMD，sw 必须知道并显式指定 vector length。warp-based SIMD，不需要，sw 不知道 vector length。

warp-based SIMD，就是 SIMD hardware 上的一种 SPMD programming model。

所以，如果用 rvv 实现 GPGPU。

rvv length 应该等价于 warp size，也就是 thread/warp。

那么，一个 block 的 thread 数量，如何界定呢？底层 llvm 又如何把 block_size 翻译成 num of warps。

思路，rvv 最近的 SRAM，假设叫 DLM，当作 vGPR 使用。那么，就可以根据这个 DLM size 来计算最大允许的 block size。实际指定的 block size，自然希望是 rvv length 的倍数。

这样，triton 一次把一个 block 的数据 load 进 DLM。llvm 再根据 rvv length 做 software pipelining，用多个 cycle 完成一个 block 的计算。

![](img/41dec05d616bdf8dd7bf7f40b69a0a8e.png)

![](img/4f8e5cf7f21490ac572fb60730c62352.png)

warp 内，不同 thread 可以走不同的 control flow。

![](img/da68af0613c847053de3e0bacd89b457.png)

原理是，通过 mask，自动选出满足 branch condition 的 thread 组 warp。

执行效果如右图。有些 IDLE 的 thread，效率有所降低，但，至少能用。

这种 IDLE 是 GPU programming 中，需要 programmer 去尽力避免的。

![](img/3f4ac92fe088b523827a6b827cc8049b.png)

## perf & benchmark

关键指标之一：IPC

![](img/f38ac185439f0cde9957f5ea9ee9aa10.png)

# Key Challenges

## memory coalescing

[由于每个thread 有](https://www.zhihu.com/search?q=Sharememory%20&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3374307130%7D)自己的存储，所以可以并行存取[，相对一个访存入口竞争的设](https://www.zhihu.com/search?q=share%20memory%20&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3374307130%7D)计，性能可以提高。

优势比较明显，劣势在于考虑GPU存储的多级性，thread 间独立并行访问，提高了设计难度。

GPU的存储层次一般至少包括以下（离core 由近至远）：share memory、L1、L2、DRAM。最近的share memory在SM 内，由SM 内core 共享（如regfile），最远的DRAM则在外部存储器中。

提高DRAM的访存效率的途径就是合并访问，从而尽可能减少向dram 访存的次数，由于thread的并行化，这一块请求合并的动态性要求更高。

Sharememory 由多个单独的bank 构成，share memory 需要处理多core随机访问造成的bank 冲突问题。如果一个bank 被两个地址访问，每个thread中其访存带宽会下降至1/2。

## branch divergence

### Introduction

warp 内，不同 thread 可以走不同的 control flow。branch divergence 导致 GPU 利用率降低，2 个解决算法：dynamic warp merging，large warp and 2-level warp scheduling

![](img/54a975e68ab4daa13eed9fb6722f59fd.png)

由于SIMT 的部分，执行的是同一指令，所以如果指令执行中发现有的thread 执行if 有的执行else，else的线程必须等待if的执行完，才能开始，而else thread执行时，if thread处于idle。

![](img/6d7e4a97a37fe2ede32b9c0a82eae3a6.png)

![](img/96e05529bc6a373be6a4ed949bfa99b4.png)

![](img/269fe8a0faa00c487968f79060f843b3.png)

![](img/fa011f8cc65f1dece414904fcd5ad845.png)

![](img/bd6aa8eb0082edec1d0307e13d20656d.png)

![](img/5ea6b4c96c68052a530b555aab78c13a.png)

![](img/2e8c0353d0560115781eb17b6c7f9cd6.png)

![](img/c36b463fb4b03fce99c1165d22ebf803.png)

Recent work on warp divergence • Intel [MICRO 2011]: Thread Frontiers – early reconvergence for unstructured control flow. • UT-Austin/NVIDIA [MICRO 2011]: Large Warps – similar to TBC except decouple size of thread stack from thread block size. • NVIDIA [ISCA 2012]: Simultaneous branch and warp interweaving. Enable SIMD to execute two paths at once. • NVIDIA: Temporal SIMT [described briefly in IEEE Micro article and in more detail in CGO 2013 paper] • UT Austin [HPCA 2013]: The dual-path execution model for efficient GPU control flow • NVIDIA [ISCA 2015]: Variable Warp-Size Architecture – merge small warps (4 threads) into “gangs”.

![](img/1edd476097b2fa3d1f3e1344fa50b17c.png)

### Dynamic Warp Merging

![](img/2eeef7cf6c21205380fb12fb0dd3b4fd.png)

![](img/189487079b3339753780aa9cefd0937e.png)

![](img/a136cbb9cbac643bc86a7a7860f88738.png)

![](img/f4da5fadbeb75f53da1b7e4eacfc3c77.png)

warp merge 时，能否跨 lane 去做？

可以肯定是可以，只是软硬件的代价问题。

实践上，好像基本划不来。

![](img/c681d9e15ab4a0fca7a39869ccef9f15.png)

### Large Warp

![](img/6eeb9d4691eda2fcc8a389d2ebbd5aa1.png)

解决 branch divergence 导致的 GPU 利用率问题。

思路：搞一个超大的 warp。

![](img/49d290945f15dc50b555c799b7195673.png)

解决 long latency operation 问题，比如，memory access。

思路：2 级 round robin。

分析：如果 16 warp 同时 trigger，那么，大家会高概率的在相同时刻 block 在相同的 long latency op 上。

错峰 trigger warp 解决这个问题，8 个一组，分 2 组 trigger。

![](img/017752e53c7008c2e87bcd1bc951cccb.png)

![](img/770422d50941d828c00dd9cd0e357e70.png)

### SIMT Assembly is Hard

control flow 的 分支分化。在 LLM 中，应该比较少出现。-- 待分析代码验证。

注意，此处指的是 Assembly code 难写，不是 CUDA programming hard。

https://www.youtube.com/watch?v=uJEDFEJ_L2s

https://keasigmadelta.com/blog/why-writing-simt-gpu-assembly-is-hard

Yea**rs ag**o someone asked me if I would add the ability to write shader assembly to the graphics drivers I was writing. My reply was: you really don't want to do that. I've been working on adding switch support to Warp3D Nova's drivers, and decided to explain why writing GPU code is hard:

+   Modern GPUs have Single-Instruction Multiple Thread (SIMT) architectures

+   This means that multiple threads execute the same instruction in lockstep (typically in groups of 32/64 threads)

+   Threads may need to execute different code** pat**hs (e.g., if/else), but all threads must execute the same code

+   **S**o, you need to execute all paths, enabling/disabling threads using the EXEC mask

+   For loops need to keep track of which threads have exited the for-loop (using a loop active thread mask)

+   Functions need to keep track of which threads have exited (using a function active thread mask)

+   Pixel/fragment shaders may kill/discard pixels, so you need to keep track of which threads have permanently exited (using yet another active thread mask)

+   Even if you can handle that, you can have if/else nested inside for-loops, switch statemants, and function calls, which in turn are nested inside something else

+   Control-flow instructions (if/else, etc.) end up distributed through the code

+   All branches, returns or other control-flow need to be analysed, so you can insert conditional

+   **Y**ou need to "think in parallel" when writing SIMT code

The end result is that it's very easy to get it wrong, and hard to debug because you end up with masses of code, and it isn't doing what the code looks like it's doing (e.g., a conditional branch is not implemented as a conditional branch). The code looks like it's single-threaded, but you're executing multiple threads at once, in lock-step.

CPU 上，control flow 汇编，很容易写。

![](img/92bd2da89791bc3c5dec36a1f00a83b9.png)

![](img/4c8b4f42831a6ff747531f205218e3ce.png)

![](img/20859af267c3b3d0b2beef4f45ac9966.png)

![](img/cd1b9607ab7697ebfd350d761a0c33c5.png)

![](img/dd273d3d1bb7d3a1041416d0465ccbdb.png)

# AMD Navi vs Nvidia Turing

https://www.techspot.com/article/1874-amd-navi-vs-nvidia-turing-architecture/

December 19, 2019

AMD's 7nm Latest vs. Nvidia's RTX Tech

比较的重点：

1.  任务的调度与分发

1.  计算单元间的差异 - CU vs SM

1.  内存层次结构

## Arch Compare

2019 年的 GPU arch

|  | Chip Design | Graphics Architecture | Die Image |
| --- | --- | --- | --- |
| AMD | Navi / Navi 10 | RDNA | fabricated by TSMC。晶体管 Navi: Turing = 10.3: 18.6 billion |
| Nvidia | Turing | TU102 |  |

下图不严格准确，但旋转 90 度以后，跟 Die Image 基本能对上。特别是 central strip。

![](img/41618a21977736042a3822ecb5e84a38.png)

层级结构

| Navi |  | Turing |  |
| --- | --- | --- | --- |
| Shader Engines (SEs) | 2 | Graphics Processing Clusters (GPCs) | 6 |
| Asynchronous Compute Engines (ACEs) | 2 |  |  |
| titled Workgroup Processors (WGPs) | 5 | Texture Processing Clusters (TPCs) | 6 |
| Compute Units (CUs) | 2 | 2 Streaming Multiprocessor (SM) | 2 |
|  |  |  |  |
| 2 SE x 2 ACE x 5 WGP x 2 CU | = 40 CU | 6 GPC x 6 TPC x 2 SM | = 72 SM |

实际使用中，Nvidia 卡可能会有一个GPC或几个TPC处于休眠状态。也就是实际可用于计算的SM块会减少4~12个，视不同型号显卡而定

Each CU contains two sets of:

+   32 SPs (IEE754 FP32 and INT32 vector ALUs)

+   1 SFU

+   1 INT32 scalar ALU

+   1 scheduling and dispatch unit

|  | AMD | Nvidia | 描述 |
| --- | --- | --- | --- |
| 控制单元 | Graphics Command Processor | GigaThread Engine | 把相同 instruction 的 thread 组 wave/warp。 |
| thread group | wave。 | warp。 |  |

Nvidia's execution units (CUDA cores) are scalar in nature – that means one unit carries out one math operation on one data component;

AMD's units (Stream Processors) work on vectors – one operation on multiple data components. For scalar operations, they have a single dedicated unit.

a program that generates lots of threads performing FP32 vector calculations and little else would seem to favor Navi, whereas a program with a variety of integer, float, scalar and vector calculations would favor the flexibility of Turing

## Nvidia Archs

Roadmap: From left to right: Fermi, Kepler, Maxwell, Volta

自 2006 以来，NV 卡的架构演进，持续性很好。从下面 4 代的架构图就能看出来。

![](img/d15af42275fbe7a98ed7fecd8b17ea56.png)

cuda core 的每个 thread，处理的都是 scalar。warp 组合多个 thread，表现为处理 vector。

Kepler 架构，5 GPCs，3 SM blocks。

Pascal，GPC 被 split 成 TPC。每个 tpc 2 个 sm。类似 navi 架构。

Turing Arch

![](img/16b4260894e90b94f0b3a5a11fc4f290.png)

One SM contains 4 processing blocks, with each containing:

+   1 instruction scheduling and dispatch unit

+   16 IEE754 FP32 scalar ALUs

+   16 INT32 scalar ALUs

+   2 Tensor cores

+   4 SFUs

+   4 Load/Store units (which handle cache read/writes)

每个 sm 还有 2 个 FP64 units。图里没画。

RT core 是做 ray tracing 的，与我们关系不大。

FP32 和 INT32 的 ALU 可以同时并行工作。

TensorCore 专门做 4*4 的 matmul。支持 fp16/int8/int4。

等价于，每 cycle 做 64 个 FMA 指令。

## Memory Hierarchy Compare

navi memory

![](img/1eb7ed76e1bd42e4afa29469e3d1f1f0.png)

![](img/fd912b44cf2725b335c201f9e136a999.png)

Turing memory

![](img/5a210ab514e114b8fbba75a2b274c507.png)

## Compute Compare

Navi10 没有 Tensor Core，在通用计算能力上没有明显差别。

| ** ** | Radeon RX 5700 XT | GeForce RTX 2070 Super |
| --- | --- | --- |
| GPU | Architecture | Navi 10 | RDNA | TU104 | Turing |
| **Process** | 7 nm TSMC | 12 nm TSMC |
| **Die area (mm2)** | 251 | 545 |
| Transistors (billions) | 10.3 | 13.6 |
| **Base clock** | 1605 MHz | 1605 MHz |
| **Boost clock** | 1905 MHz | 1770 MHz |
| **Block profile** | 2 SE | 4 ACE | 40 CU | 5 GPC | 20 TPC | 40 SM |
| Unified shader cores | 2560 SP | 2560 CUDA |
| Peak FP64 TFLOPS | .55 | .28 |
| Peak FP32 TFLOPS | 9.75 | 9.06 |
| Peak Tensor FP16 TFLOPS | - | 73 |
| **Memory** | 8GB 256-bit GDDR6 | 8GB 256-bit GDDR6 |
| Memory bandwidth | 448 GBps | 448 GBps |
| Thermal Design Power (TDP) | 225 W | 215 W |
| **Price** | 399$ | 499$ |

## Why AMD performance 差

Older Radeon graphics cards had lots of potential performance, but tapping into that was notoriously difficult. Now, AMD has taken a large step forward in reducing the latency in processing instructions and also retained features to allow for backwards compatibility for all the programs designed for the GCN architecture.

instruction latency 太长了，tuning 起来非常 specific，学习和解决的时间成本，太高了。

# 附录

get faults and want to recover faults

1.  detect faults。

1.  recover from faults

1.  end version execution

![](img/96740a305d148bf0e6730c2b8fae5ca6.png)

![](img/33b06c2141efa75ba1b1bff0a98ce595.png)

![](img/57b7fe7807def23f71d97c642f05c773.png)

## Programming v.s. Execution Model

讨论 SIMD 和 GPU 时，

注意区分：

+   programming model （software）: SPMD

+    Execution Model （hardware）: SIMT

这个概念，第一次听到的时候，还是很有启发的。

特别是，GPU 内，layer 很多，听的很晕。

![](img/015e4adcb5e54429701b5dff8e244798.png)

举个例子：sequential ISA 的 Instruction 级并行，3 种 Programming Model

+   SISD

+   SIMD

+   MIMD / SPMD

![](img/a7bbeeb54ed913e717d9f1c9bd2fc458.png)

SISD 的多种 execution model

pipelined processor，out-of-order，superscalar(VLIW) 基本都可以。很丰富。

![](img/1933bab206b136f3422cb8cfe62c636d.png)

SIMD 的多种 execution model

最好用 SIMD processor (Vector, Array Processor)，选项就没 SISD 那么多了。

也可以用 SISD 的 processor，需要 compiler / processor 把 SIMD code 转成 scalar 的 sequential code。

![](img/0d082336297c12f2318b6ec8b48108f5.png)

类似 MIMD 的 model，叫 SPMD 更准确。

如果理解为 MIMD，用 MIMD processor 最佳。

如果理解为 SPMD，用 SMID 或 SMIT processor 则更好。

![](img/42d5f16d92ca0c87e22a0e3bc9ce1d70.png)

## FGMT History

![](img/31eb1cc79cebf3d1d0babf36803df5b5.png)

![](img/4404139771b23ad83c2fdbc4db8cc43a.png)

## roofline model

![](img/f16a4ca5ad51043c08709bdf371de2e2.png)

![](img/752d090d6f395229a9d1e8e15df4444d.png)

左图为三款芯片 FP32计算模式的Roof-Line曲线，右图为T4与V100 FP16 Tensor计算模式的Roof-Line曲线

从数据上看，V100处理任务时更易达到计算瓶颈，能更好的处理高带宽需求任务；T4与P4则主要受带宽限制，更适合处理高计算强度的任务。在实际工作中，可以根据不同模型（计算强度高的和低的）、不同需求（训练还是推理）来合理分配任务。