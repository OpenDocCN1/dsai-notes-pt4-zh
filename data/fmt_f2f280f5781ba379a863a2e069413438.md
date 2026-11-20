
# System-on-Chip 101

> 来源：[https://learning.feishu.cn/docx/QKYidtyomorCZ1xePIAcnfhDneh](https://learning.feishu.cn/docx/QKYidtyomorCZ1xePIAcnfhDneh)

# Summary

https://www.youtube.com/playlist?list=PLZU5hLL_713ygweO3b_9KiZUJuEI7I5yK

System-on-Chip 101

Everything you wanted to know about a computer but were afraid to ask

course at Bar-Ilan University

Prof. Adam Teman

slides https://enicslabs.com/academic-courses/soc-101/

![](img/2bc7558088f3e7d6703dd865cd5d9705.png)

本文是 SoC 101 课程 Lecture 1 的笔记，讲 `xxx` 阶段。

motivation for the course and an introduction to MPUs, MCUs, SoCs and basic embedded system components

| No. | topic | 视频时长 | 用时/二刷 |  |
| --- | --- | --- | --- | --- |
| 01 | introduction | 40min | 1h |  |
| 02 | Microprocessor / CPU | 1.5h | 1h |  |
| 03 | From C to Assembly | 2h | 3h | basic computer，risc-v 和 the build process 3 部分有收益。 |
| 04 | on-chip interconnect | 1h | 2h | 非常 hardware，知识盲区比较多。 |
| 05 | 外设通信 | 1.5h | 1.5h | UART, APB, AHB, AXI, DRAM, DMA |
| 06 | memory | 2h |  |  |
| 07 | OS | 2h |  |  |
| 08 | debug | 45min |  |  |
|  |  |  |  |  |
| total |  | 11.5h / 4h |  |  |

![](img/6a17061ef85eb3f96b9b77766be23350.png)

非常推荐该老师的数字集成电路和数字VLSI课程（尤其是RTL to GDS那门课），该老师应该是做定制电路出身的，对存储器、先进工艺等有很深入的了解，其还参与了大名鼎鼎的RISC-V开源项目PULP

AMBA 学习

https://www.zhihu.com/column/c_1663245806869291008

# Lec01 - Introduction

## Motivation

Welcome to hardware engineering

## MPU, MCU, SoC

option 2 就是 SoC

![](img/4c2e16472519201ce7af8008762851f1.png)

intel 做 4004 的时候，还是个 DRAM company。

tape out: 最早就是 AT&T 大楼里画线。

map 也就是 CPU。

![](img/fc4cbc03736f2efa90d53de55260adb1.png)

mpu 是 1 颗 cpu。

MCU 是 1 个 computer。包括 CPU + memory + I/O 外设。

用 chiplet 部分解决 SoC 的 limitations。

## Embedded Systems

## Components

## Summary

![](img/8a6fdc33db9d19f44ed2751249cc995d.png)

# Lec02 - Microprocessor (CPU)

## Summary

前 2 个 section 可以。后 3 个 section 讲的比较浅。

1.  CPU = Datapath + Control

1.  图灵机：计算所有可计算的问题。图灵完备性。

1.  冯诺伊曼架构：stored-program computer。

1.  RISC-V 只支持 4 种 mode 里的第 3 种：Displacement Addressing。

1.  指令可以分 3 类：data transfer, flow-control, compute

calling convention (约定)，是 ABI 的一部分，一般在 compiler 里实现。

注意区分 `stack pointer` 和 `frame pointer`。都需要独占 register。

1.  running 的主要 4 个阶段：compiling, assembling, Linking, loading.

1.  object file 的格式，常用 ELF or COFF.

分层，常被忽略，但应该学习的 3 层：

1.  runtime (OS / VM)

1.  Architecture (ISA)

1.  MicroArchitecture

![](img/8164ffc139a854d171142a816b8f8989.png)

## Introduction and History

### 分类 & 基本概念

### Turing Machine

图灵机，计算所有可计算的问题。

能 simulate 图灵机的，就满足图灵完备性。

第一个电子的 computer。

冯诺伊曼架构。stored-program。难怪，指令和数据分开的，就叫非冯架构。

### Stored-Program Computers

`PC` - program counter 是个重要的概念。

微指令，是微架构的一部分。对 programmer 不可见。

缺点，不能跟进微架构的创新，比如，没法通过 pipeline 做指令级并行。与 compiler 的工作重复。不能重新 programming。

### 哈佛架构

### ISA 指令集

## ISA

### ISA Introduction

读写 SRAM 和 register 都是 1 个 cycle 的 latency。实际是用了 single clock edge。

但，SRAM 是独占。而 register 可以与 compute 共享一个 cycle。

一个 cycle 可以读写多个 register，但 SRAM 必须是相同地址。（可以用多个 bank 提高 bandwidth）

ABI 是比 ISA 高一级的 abstract。

### Data Transfer Instructions

RISC 里，必须用 `load`/`store` 才能访问 memory。在 CISC 里，有 instruction 可以直接访问 SRAM。

RISC-V 只支持 4 种 mode 里的第 3 种：Displacement Addressing。

### Flow-Control Instructions

需要 branch prediction 的地方。

### Arithmetic-Logic Instructions

### Big Endian vs. Little Endian

大端法：特朗普吃披萨。

## Calling a Procedure

主要就是讲 calling convention，是 ABI 的一部分，一般在 compiler 里实现。

注意区分 `stack pointer` 和 `frame pointer`。都需要独占 register。

讲的比较浅，其他没啥重要的东西。

分 6 步

注意区分 `stack pointer` 和 `frame pointer`。都需要独占 register。

## Running  a Program - CALL

compiler 课程讲的更深入一些。

1.  主要 4 个阶段：compiling, assembling, Linking, loading.

1.  object file 的格式，常用 ELF or COFF.

compilation and assembling 过程

stack 和 heap 的 memory space。

linking 和 relocate 过程

## Measuring Performance

skipped

## Reference

![](img/210406010dedc078bf027555dd0c53a6.png)

# Compilation - From C to Assembly

ISA 详细定义 & hardware 实现。

## Motivation - ISA 的趋势 & RISC-V

好多 standard 的 mapping 关系。

## Basic Operations

3 个：

1.  instruction fetch

1.  register access

1.  execution

组件说明：

+   Control and Datapath: 就是 Control + ALU 部分。

+   IMEM: instruction memory

+   DMEM: data memory

load-store architecture，就是 RISC 架构 (RISC-V)，即，只能直接读 SRAM。

`PC` 的累加，也需要一个 ALU。输入 `old address`，输出 `new address`。

因为 `RV` 有 32 个 register，所以 `rs` 和 `rd` 都需要 5bits。

硬件不区分 resister，ABI 约定了几种 register 语义。

硬件只约定了 1 个 register，`x0` 硬编码为 0.

## Variables and Memory Access

介绍 C 和 RISC-V 的 memory model，以及寻址方式。

挑战在于，RISC-V 只有 displacement addressing，如何满足业务需求。特别是，访问 32bit 的绝对地址。

`volatile` 关键词，阻止 compiler 的 dead code elimination。

比如，有的变量是输出，在局部代码内并没有被用到。

后面几页，都是 c 指针的具体设计与实现。经典的那套理论。

## Control Flow

ISA 层面，比较简单。没啥新知识。

## Procedure Calls

2 种 jump 指令，当 `dst` 距离最新 `PC` 很远（`Imm` 很大）的时候，适合指令 2（`jump and link register`）

state register，3 个：`x2`, `x3`, `x4`.

`stack pointer`, `global pointer`, `thread pointer`.

saved register，3 组：`x8`, `x9`, `x18-27`

`frame pointer`/`saved reg`, `saved register`, `saved registers`

temporary registers, 2 组：`x5-7`, `x12-17`

这些为什么是 `volatile`？

caller 自己保存有用的 `value` 到 memory，因为 callee 可以覆盖 register。

additional `volatile` register, 3 组，`x1`, `x10-11`, `x12-17`

return address, arguments/return address, arguments.

同上，callee 可以覆盖 register。

序言和尾声

应该都是 compiler codegen 里讲过的东西，会更详细一些。

## VIP - RISC-V Features and Extensions

### base arch with control flow

TODO 需要详细理解一下

1.  什么是 branch delay slots

1.  overflow checks 很有意思。

### RV Extensions

给 RISC-V 增加一个 存算 chip 的 custom extension

### compressed instructions

### integer multiplication & divide

`n bit * n bit = 2n bit`

低 `n` bits 都一样，与计算类型无关

除法是很慢的，可能需要很多个时钟周期。

### Float Point

下面这个图，第一次清晰简洁的理解了 `fp` 实现细节。

`fp` 的加法和乘法实现

### Additional Standard Extensions

## The Build Process

### Preprocessing

### Compilation

2 个重点：

1.  生成的 assembly files 遵循了 ABI，体现在 register 的使用上。

1.  section 的划分，是依据 memory model 做的。

pseudo- instructions 里的 `li` 是 `load immediate` 的缩写

### Assembler

2-passes

### Linker

### Startup code for c

### Locator

server 上，一般是 runtime 的 `loader` 完成 `locator` 的工作。

### Loader

执行过程，非常接近前面讲的 startup code for c。

## Reference

# On-Chip Interconnect

![](img/a79806e73c0ac124e7384802dee54eb9.png)

## On-Chip Communication

+   off-chip: 红色框出

+   on-chip: 其他部分

![](img/358934cf7bc30a49e90a9805cb7bbdf6.png)

软件主要关心 3 点：

1.  bandwidth

1.  latency

1.  master-slave，一般是底层 runtime or 通信库关心即可。

heterogeneity 异质性，非均匀性

简单一句话：communication 越来越复杂，对标设计一个小 cpu。

跟 memory bandwidth 一样，都是 IO bounded 的一部分。

通病：scale 速度明显落后于 compute 部分。

communication 是 SoC 设计最关键的部分，但，很多人没有这个意识。

3 topics to discus

## 外设通信

### Memory

2 种 memory 模式：普林斯顿架构 & 哈佛架构。

先搞清楚 memory 本身，再分析不同架构下的 interconnect。

实际上是高速 Cache memory，不是 DDR 内存。

红色，是一个 cycle，根据 `clk` 有上升沿、下降沿。

`Q` 是 data out。

`CEN` Chip enable。`high`: not using the chip.

`WEN` write enable。控制 `read/write` mode。

`CLK` 时钟。

### System Bus

一般的，CPU 是 master node。负责发起 comm。

系统总线包括 3 种 bus + clock。

### Memory Mapping

## Simple Bus Operation

### bus communication

### APB bus

## Higher Performance Buses

### APB

### AXI

### Multi- Level Bus and Bridge

## References

# Communicating with Peripherals

or How to build a router

## Comm with the outside world

### GPIO

### Off-Chip - UART

## Offload the CPU

### UART Controller

### busy poll & interrupts

better than busy polling

## Faster Interfaces

### Ethernet

![](img/de7c751414bc45b3dd5662bdade32c66.png)

![](img/524cf4729bfe01f253f798970cc66f52.png)

### APB for Ethernet

### AHB for Ethernet

## More Off-Loading - DMA & ASIP

ASIP: Application-specific instruction set processor

ASIP: Application-specific instruction set processor

## DDR - DRAM

![](img/2adf8da796a62b4fb40af9ae756abc3e.png)

## AXI and Boot

### AXI and Cache

![](img/62c2ae4e1e4fdef598b28e37636153ac.png)

AHB 不能乱序执行，AXI 可以。

![](img/88f3ef77460ff37695927d13c28e6df2.png)

![](img/2bc2834a37850aee317c5a4f3bfc1d91.png)

### Boot

## Conclusion

## References

![](img/c3bfd1498b3503511f3f635040ae331b.png)
