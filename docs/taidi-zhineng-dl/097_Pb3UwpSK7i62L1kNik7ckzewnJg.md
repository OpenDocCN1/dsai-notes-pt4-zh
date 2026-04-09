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

本文是 SoC 101 课程 Lecture 1 的笔记，讲 xxx 阶段。

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

![](img/1bdae9d985a306c7b9b45b39abd06d77.png)

## MPU, MCU, SoC

option 2 就是 SoC

![](img/4c2e16472519201ce7af8008762851f1.png)

intel 做 4004 的时候，还是个 DRAM company。

tape out: 最早就是 AT&T 大楼里画线。

map 也就是 CPU。

![](img/fc4cbc03736f2efa90d53de55260adb1.png)

mpu 是 1 颗 cpu。

MCU 是 1 个 computer。包括 CPU + memory + I/O 外设。

![](img/cfa571841f528269870c552ea6c05db8.png)

![](img/b455b4bd5d1575898f108b18fc953cbe.png)

![](img/a55e5726113650b42f738f5e2466e5e7.png)

![](img/56e907209c12bec554731b4f171d5b4b.png)

![](img/9f2dc0b0c720055ac35d98f0654d97f0.png)

用 chiplet 部分解决 SoC 的 limitations。

## Embedded Systems

![](img/0bbfb9bed394a58f1b0f0a12f030effc.png)

![](img/d3b848557883c1e6e203c2d84ca756d6.png)

![](img/d1943c6f108f531fa18169c017936182.png)

![](img/8747b198c0beed1ea72c64461ed8df7d.png)

![](img/9e01cba3f3fec2f268108671eae86274.png)

![](img/99e2ac15b7cef38bc6a7731ccf9e4d2c.png)

![](img/6869bf9af321d69edcceab3d4f86d9c9.png)

![](img/70b006d60da70c75c604a17fa7e6f310.png)

![](img/f0e4a19ca96863449452e445a75640f1.png)

![](img/1dc89c840362f7cae9c00f5660e3168e.png)

## Components

![](img/c25d59de7099ea312fe7affb9cc30510.png)

![](img/6113dda2e25cc875c7c9a14a4d2786bd.png)

![](img/7198686361ee2956108cb3e0568d1847.png)

![](img/3c8670b492f4aa484c8aa148981cf7ca.png)

![](img/6d92e4a2ca289a7a23b0f0eb50676a8d.png)

![](img/09954e4cc468df738cf16f29e47c8b09.png)

## Summary

![](img/6131e5c54073500039d5afe2f98bc049.png)

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

注意区分 stack pointer 和 frame pointer。都需要独占 register。

1.  running 的主要 4 个阶段：compiling, assembling, Linking, loading.

1.  object file 的格式，常用 ELF or COFF.

分层，常被忽略，但应该学习的 3 层：

1.  runtime (OS / VM)

1.  Architecture (ISA)

1.  MicroArchitecture

![](img/8164ffc139a854d171142a816b8f8989.png)

## Introduction and History

### 分类 & 基本概念

![](img/3d06bc47efadb632819c7614c9b44936.png)

![](img/f0458dfd0d7ac3198aab0b1987b1995a.png)

![](img/4f86b0dfdf4b22b1d3ae42a4e2a8ed7c.png)

### Turing Machine

图灵机，计算所有可计算的问题。

能 simulate 图灵机的，就满足图灵完备性。

![](img/002e76a5ae668f9a9eeb7d9842be352d.png)

第一个电子的 computer。

冯诺伊曼架构。stored-program。难怪，指令和数据分开的，就叫非冯架构。

![](img/2f7b36a7dd982e694e369da5966786fe.png)

### Stored-Program Computers

PC - program counter 是个重要的概念。

![](img/1f73f203c53f196f32879f9c234c8aae.png)

微指令，是微架构的一部分。对 programmer 不可见。

缺点，不能跟进微架构的创新，比如，没法通过 pipeline 做指令级并行。与 compiler 的工作重复。不能重新 programming。

![](img/806214d684690b79d0269cc5ddca9178.png)

![](img/24f448cd31aa7d1efab77e14e5ec8a0c.png)

### 哈佛架构

![](img/cb4518becaf9fffa5e19e749b0ea58c8.png)

### ISA 指令集

![](img/7734b2288c61d7f6bad623ec0220f440.png)

![](img/c92951e2c6671255a98c9d3f298446c5.png)

![](img/144d943fcd652b0850fa04c7cc225a29.png)

## ISA

### ISA Introduction

![](img/9bd92a730b944e7eef70485b608971e3.png)

![](img/e1e5e4fc20dc7253b5b7780bfe30771e.png)

读写 SRAM 和 register 都是 1 个 cycle 的 latency。实际是用了 single clock edge。

但，SRAM 是独占。而 register 可以与 compute 共享一个 cycle。

一个 cycle 可以读写多个 register，但 SRAM 必须是相同地址。（可以用多个 bank 提高 bandwidth）

![](img/995e5abde11228d7f7cca47773b07957.png)

ABI 是比 ISA 高一级的 abstract。

![](img/8b424a474970204a67b7ea52593a2b60.png)

### Data Transfer Instructions

RISC 里，必须用 load/store 才能访问 memory。在 CISC 里，有 instruction 可以直接访问 SRAM。

![](img/a80edfe7df6d76c00a17db4f59d41b9a.png)

RISC-V 只支持 4 种 mode 里的第 3 种：Displacement Addressing。

![](img/6d22a1924c3cbf273513a4542c1e2f8d.png)

### Flow-Control Instructions

需要 branch prediction 的地方。

![](img/1bf7acef4cc050113c31e45a08bf02bd.png)

### Arithmetic-Logic Instructions

![](img/5554e3772be479d121827b55064fba2c.png)

### Big Endian vs. Little Endian

大端法：特朗普吃披萨。

![](img/6aa096603ac619e14ae9623ad4616bfd.png)

![](img/d352e45a9fc44b32d4b6b0ada4973b23.png)

## Calling a Procedure

主要就是讲 calling convention，是 ABI 的一部分，一般在 compiler 里实现。

注意区分 stack pointer 和 frame pointer。都需要独占 register。

讲的比较浅，其他没啥重要的东西。

分 6 步

![](img/bd24947b41ff280689d9a0ca3d2c2eed.png)

注意区分 stack pointer 和 frame pointer。都需要独占 register。

![](img/770d221c6bbe5b1bb0af042e7935049e.png)

![](img/e2447a7a0782e1e234e876c5e1e7aeab.png)

## Running  a Program - CALL

compiler 课程讲的更深入一些。

1.  主要 4 个阶段：compiling, assembling, Linking, loading.

1.  object file 的格式，常用 ELF or COFF.

![](img/a452bba4bd2d4d60a36f601f585b8670.png)

compilation and assembling 过程

![](img/edb99c15df13fce606600bc634105ff4.png)

stack 和 heap 的 memory space。

![](img/8138df1f61969f0e67c66cd16a67e5c1.png)

linking 和 relocate 过程

![](img/3f8455bf6f05984aeafb3a3409fb49eb.png)

![](img/77b2492241e923262ebb6c3b507ad58b.png)

## Measuring Performance

skipped

![](img/3caa6cf779172525c8b5f29c93fb330c.png)

![](img/c9095c8e241ae3a3edb029a5ed882e6b.png)

![](img/a25e612d9976c79b6b6279a2dbd82abd.png)

![](img/c47e4d3f9436d8b4e81869dff7b5471b.png)

![](img/5e6146fef224c63bf7aa156eb9d33695.png)

![](img/4fbf3a7404ac636cc8cbbd2e31d4d557.png)

## Reference

![](img/210406010dedc078bf027555dd0c53a6.png)

# Compilation - From C to Assembly

ISA 详细定义 & hardware 实现。

## Motivation - ISA 的趋势 & RISC-V

![](img/0d776200956cc5f20dea79655c43fe70.png)

![](img/fe29ff38c792c7f6df1db9d1431b6351.png)

![](img/588fd2bb0a5244dc9be87c763a715954.png)

好多 standard 的 mapping 关系。

![](img/30dae98fd772116ce5ffde181c25b7d2.png)

![](img/93dfc265f5b029b427ebded53dab69e1.png)

![](img/390f9cd611082048e60fc1e858f31510.png)

![](img/d84cfa698cb6144c3266393ee3066475.png)

![](img/3e67bdfa077e0210496439405b64edd5.png)

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

![](img/f39602c0e95f168bfb2cd154422c2712.png)

PC 的累加，也需要一个 ALU。输入 old address，输出 new address。

因为 RV 有 32 个 register，所以 rs 和 rd 都需要 5bits。

![](img/0b8434bf097a4c064ab0f19e6ed84ffc.png)

硬件不区分 resister，ABI 约定了几种 register 语义。

硬件只约定了 1 个 register，x0 硬编码为 0.

![](img/3de8616e6906de5cca4c9afb71ee5788.png)

![](img/c76d50db1926cfa7f6fdd028f8f99047.png)

![](img/d45e6f7f50e07a84cfe74a93ac39e528.png)

![](img/631f1fc415d5f2b231198215e8cd2f1c.png)

## Variables and Memory Access

介绍 C 和 RISC-V 的 memory model，以及寻址方式。

挑战在于，RISC-V 只有 displacement addressing，如何满足业务需求。特别是，访问 32bit 的绝对地址。

![](img/c05cfe4ce39492fca8d89f7eff76ca8b.png)

volatile 关键词，阻止 compiler 的 dead code elimination。

比如，有的变量是输出，在局部代码内并没有被用到。

![](img/f7d36465936b9a09d48d0f1061e25c27.png)

![](img/d13d0b5523349059a3a8efa8c11216c8.png)

![](img/37b3443b3bbff6f654d10df32b9cf6d1.png)

后面几页，都是 c 指针的具体设计与实现。经典的那套理论。

![](img/f62ec5d3ce514ffea5b621ab3ca4b3c6.png)

![](img/f2bb5965a3468fee35e02dab76beebb5.png)

![](img/bb24905d0aaa8fba7f902ce704e08e7c.png)

![](img/91fda4fa1883ea4d66f0b7ba1183dbc9.png)

## Control Flow

ISA 层面，比较简单。没啥新知识。

![](img/1f2af156999459ef87497113308e9c96.png)

![](img/cfbe21b15d86af37aa50325e242b1280.png)

## Procedure Calls

![](img/68e2ba20889d3f29f635d5351e946e8c.png)

2 种 jump 指令，当 dst 距离最新 PC 很远（Imm 很大）的时候，适合指令 2（jump and link register）

![](img/8469edf7b8c9e5c25d143425db997469.png)

![](img/b38e115f25b5fb43f70cd4f324bc5855.png)

state register，3 个：x2, x3, x4.

stack pointer, global pointer, thread pointer.

saved register，3 组：x8, x9, x18-27

frame pointer/saved reg, saved register, saved registers

temporary registers, 2 组：x5-7, x12-17

这些为什么是 volatile？

caller 自己保存有用的 value 到 memory，因为 callee 可以覆盖 register。

additional volatile register, 3 组，x1, x10-11, x12-17

return address, arguments/return address, arguments.

同上，callee 可以覆盖 register。

![](img/a6ac93972cfdcdd97e731c621afc9f7a.png)

序言和尾声

应该都是 compiler codegen 里讲过的东西，会更详细一些。

![](img/8af6bf4da8161042d0452c1e581936f7.png)

![](img/5be818ec3e778e76057f20b381db8629.png)

## VIP - RISC-V Features and Extensions

### base arch with control flow

![](img/54d3f696c6266d4207c6f54bc520a904.png)

![](img/94d0f386c30b86cabd861809e832f90f.png)

![](img/8c277d29b29e6decd9ad2d6d49661ca3.png)

TODO 需要详细理解一下

1.  什么是 branch delay slots

1.  overflow checks 很有意思。

![](img/046efafb4062a910d837edd4315c9598.png)

### RV Extensions

给 RISC-V 增加一个 存算 chip 的 custom extension

![](img/92c5e2c49f17c3ac7e0ded57ccbeed0e.png)

### compressed instructions

![](img/d09aa0f89c61a627ac9c79749a4ba262.png)

### integer multiplication & divide

n bit * n bit = 2n bit

低 n bits 都一样，与计算类型无关

![](img/b563ca44b677305a31a477ff6b64427a.png)

除法是很慢的，可能需要很多个时钟周期。

![](img/ff96d239030d59d3ac60cfd9ac68dbfa.png)

### Float Point

下面这个图，第一次清晰简洁的理解了 float point 实现细节。

![](img/da7ef21aa6cd980ad9bd9f496fc28de5.png)

fp 的加法和乘法实现

![](img/5b78129478bd5d0648248aed300dfabe.png)

![](img/5ee86aef3d8cad914bdb2b88bc98063b.png)

### Additional Standard Extensions

![](img/cf620a6c97d864f3958b5bc980759d10.png)

## The Build Process

![](img/b58b536ffdfff10c6fbadd2fc1c7f154.png)

### Preprocessing

![](img/0bccdb9d34050ff968eb514a002ded90.png)

### Compilation

2 个重点：

1.  生成的 assembly files 遵循了 ABI，体现在 register 的使用上。

1.  section 的划分，是依据 memory model 做的。

pseudo- instructions 里的 li 是 load immediate 的缩写

![](img/3a034ddfdba8ee0ee343a2c74f789604.png)

### Assembler

2-passes

![](img/52d984a3f5a772e2d41b50cfb8f49194.png)

### Linker

![](img/9657c173ebc6d1efa58e93ee8c99fae9.png)

### Startup code for c

![](img/741a5ca25b1c0e5cb3cc0d44abca5006.png)

### Locator

server 上，一般是 runtime 的 loader 完成 locator 的工作。

![](img/7dd6a395038f4e47620900a0bde4fa35.png)

### Loader

执行过程，非常接近前面讲的 startup code for c。

![](img/2db8db576fd8ef35b762f52019045fc6.png)

## Reference

![](img/1fc4dd6566d54b782c9601a843127b13.png)

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

![](img/daa6038683deacd0456a7db0a5b2166d.png)

heterogeneity 异质性，非均匀性

简单一句话：communication 越来越复杂，对标设计一个小 cpu。

![](img/cedb15f51a7d76505ea5380e1e37396c.png)

跟 memory bandwidth 一样，都是 IO bounded 的一部分。

通病：scale 速度明显落后于 compute 部分。

![](img/40ebee1bf91581ebdc3892c4be315313.png)

communication 是 SoC 设计最关键的部分，但，很多人没有这个意识。

![](img/e8ca34557318f691201e471631c50ed5.png)

3 topics to discus

![](img/fb0c379166a8f3eacc61026a0af299e5.png)

## 外设通信

### Memory

2 种 memory 模式：普林斯顿架构 & 哈佛架构。

先搞清楚 memory 本身，再分析不同架构下的 interconnect。

![](img/5a3a167e92621e6aee2d9d97b151a39f.png)

实际上是高速 Cache memory，不是 DDR 内存。

红色，是一个 cycle，根据 clk 有上升沿、下降沿。

Q 是 data out。

CEN Chip enable。high: not using the chip.

WEN write enable。控制 read/write mode。

CLK 时钟。

![](img/d2b39c7da1730da44e6b98aa13834f4d.png)

### System Bus

![](img/51c43c95a54c23af3efae6bbf5f16d7e.png)

一般的，CPU 是 master node。负责发起 comm。

系统总线包括 3 种 bus + clock。

![](img/a1740bbf82f5dc953f65c20e6c6a475a.png)

### Memory Mapping

![](img/6c42fcf0341cce41e3a38d234a714a3d.png)

![](img/bc7c321c307c770260860cf3ee3d1476.png)

![](img/7e297598c871c4c5fbc496309fac3c9d.png)

![](img/f0c0322b9b76b0ee4bb2c20cd2bcd850.png)

## Simple Bus Operation

### bus communication

![](img/b2ce0e3f353cac7012303def416d1598.png)

![](img/2fc13954aaa38eb8c3f83698e0868be0.png)

![](img/101aa0a1f4135bd0a47482b919a650c2.png)

![](img/210029f84cc3a18e914c568bb1d20e4a.png)

### APB bus

![](img/112f83e0080293145de46eda98bf7d5d.png)

![](img/50ed3977dc877f20332ec4f214ea78d6.png)

![](img/83ffe65c48fb92c2ad7db0bc3b164f60.png)

![](img/7228837f1ca50b9a2536cdfcadcbab3f.png)

## Higher Performance Buses

![](img/193ff71396a6fd2c4e4f8f1aa13df332.png)

### APB

![](img/72fde92d9333c66c27e8a0f71c0e2149.png)

![](img/f66af2e26dca129a305310cdf6a6c658.png)

![](img/364358b30caf0e95c1dd2ba8acd02078.png)

![](img/06716589182adee062ebe2970859bfd4.png)

![](img/50fcb73e6d7720effa2dc2e7b0a5ecc2.png)

### AXI

![](img/7e22335895c56cbb43471e998053b946.png)

![](img/ddecf975365bf2322f70ab1ca152bd21.png)

![](img/c57d02f7bc7bd3f18053e894d00e97bf.png)

![](img/441429844ae939d5099e7b03565934d2.png)

### Multi- Level Bus and Bridge

![](img/a407402969e5c568be1b61106f24c8d8.png)

![](img/3ce08765f4a47401cea5c0e04556ebe5.png)

## References

![](img/db9ef1ceb0dcef6386dc546992024141.png)

# Communicating with Peripherals

or How to build a router

![](img/707660f2271189e1fec3c577d21b88f3.png)

## Comm with the outside world

![](img/7aaa9bef87e3e6270b25ef6668b49ee2.png)

### GPIO

![](img/0bc457ef9a3d1cd7335f0b4041cbd193.png)

![](img/910cd2ad525e2113d7da258b975f33ca.png)

### Off-Chip - UART

![](img/caa3c44cd081d6327644831536afb8d5.png)

![](img/b852d31490c02e87c54cb170ad79a2e2.png)

## Offload the CPU

### UART Controller

![](img/7ca7e426c1bc68d98cfd4e40f506a5cd.png)

![](img/7ca8a4d71d1a7b8af664d696abfcd034.png)

### busy poll & interrupts

![](img/5cafa56afb7198ac38bad597ca3b84e6.png)

better than busy polling

![](img/7f75980779a8511f1bc087bc7d04254b.png)

![](img/4b9011dd8f77c45307aa78df2cc0b7c9.png)

## Faster Interfaces

![](img/a066f6ac56ee7fb13f427c3b4aba205d.png)

![](img/e85e10b3178a3d0bb409be6e981a1957.png)

### Ethernet

![](img/d334be6ebe3ac6f79808c6bab99bfb23.png)

![](img/de7c751414bc45b3dd5662bdade32c66.png)

![](img/524cf4729bfe01f253f798970cc66f52.png)

### APB for Ethernet

![](img/92636a6e1b8ec7148c867dfc70db4999.png)

![](img/aa1cc72bd3bd58b511c91fe1702f2cfa.png)

### AHB for Ethernet

![](img/cd938737ce4d9dc9c961c0bf568dbfcb.png)

![](img/27b873064fbba10cf137227ff85cd5f0.png)

![](img/ad71c2351d894d9bdb5471e8ef2428ba.png)

## More Off-Loading - DMA & ASIP

ASIP: Application-specific instruction set processor

![](img/8d7dffc296848cdda0cb5887df418fb1.png)

![](img/132a85567b3a07981f0fb117748c6ee2.png)

![](img/3518882c043473510af9dc2b3e50d306.png)

![](img/594381244013984832a0a40b8598c8ad.png)

![](img/c5d5765135545a11a12152bd8a3eaa99.png)

![](img/7c23ebe34256543c5a9a18de4c9e9e31.png)

![](img/c6e35482a77b2fdcf8b8c839da4a6117.png)

![](img/55bec69b53862c24f8e4f91a13edfc90.png)

ASIP: Application-specific instruction set processor

![](img/dfee199aaa84b20e1e2fc5a9b6cb74ff.png)

## DDR - DRAM

![](img/2d958c775df9a3c5e19c632419f461f1.png)

![](img/4712abd935ae26339ef2813ab0e3de8e.png)

![](img/8f680c372e1df471998b97b83a6cf510.png)

![](img/b5541c895b4c76c795b2d24bad4de30b.png)

![](img/7f0481ec7d15643a6b29bfc14d50e680.png)

![](img/2adf8da796a62b4fb40af9ae756abc3e.png)

## AXI and Boot

### AXI and Cache

![](img/62c2ae4e1e4fdef598b28e37636153ac.png)

AHB 不能乱序执行，AXI 可以。

![](img/88f3ef77460ff37695927d13c28e6df2.png)

![](img/2bc2834a37850aee317c5a4f3bfc1d91.png)

![](img/5e174eccd4ec90713eff3fecff70c561.png)

![](img/13ace4bd7ea473847d8c8fab4218f90f.png)

### Boot

![](img/12e45000440cbfa9638de0b229a24040.png)

![](img/868417eadbeb98800ba67315d91a5701.png)

## Conclusion

![](img/61e65c55b22f7f0bab6ad157dcce9b38.png)

## References

![](img/c3bfd1498b3503511f3f635040ae331b.png)