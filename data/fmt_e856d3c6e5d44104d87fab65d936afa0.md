
# [lec11] Multicycle uArch Design

> 来源：https://learning.feishu.cn/docx/W6R6dyXz6oKsU6x6vhVcRBcRn6e

https://www.bilibili.com/video/BV1e6421G7mz/?p=14

Lecture 11: Multi-Cycle Microarchitecture Design

Onur Mutlu, ETH Zürich

Spring 2023

视频中间有 10min 的 break 休息，从 1:04 分开始

## Summary

设计一个 `uArch`，就是 2 步：

1. `datapath`
1. `Control logic`

`Control logic` 决定了 hardware 运行时的所有状态空间，

1. 用 `uISA` 来描述，是一个 35bit 的 word。
1. 存在 `Control Store` 里。

`control store` 的抽象，让 hardware designer 可以做 `uProgramming`。

`Single-Cycle uArch` 一般是 bad idea，因为不同指令的 latency 可能相差很大。也就是长尾问题。

现代处理器，memory 访问非常慢。

导致，`single-cycle uArch` 的指令长尾问题，更加严重。

如果再有访问 hard disk 的指令呢？

`single-cycle` 需要 2 个 memory (或 port)，3 个加法器 （`ALU`，`PC`，`Branch address`）。

1. 新增了 4 个 register 保存 stage 的中间结果。红色框出。

1. 1 个 memory，1 个 `ALU` 加法器。绿色框出。

## Eval Single-Cycle uArch

Metrics:

1. a good idea / design?
1. when is this a good design?
1. when is this a bad design?
1. how can we design a better uArch
1. multi-cycle uArch
1. pipelining
1. out-of-order execution

好不好，主要取决于不同指令的 latency。

实践上，一般是 bad idea，因为不同指令的 latency 可能相差很大。

也就是长尾问题。

## Performance Analysis

### Modeling

`single cycle uArch`：

1. `CPI` (cycles per instruction) = 1。
1. `clock cycle time` 取决于最慢的 instruction。

6 个 phases，5 级流水。

### 指令 Latency 分析

`LW` 指令最慢。因为，`IF` 和 `MEM` 阶段都访问了 memory，各需要 200ps

### Control Logic

First question: can control logic be on the critical path？

`single-cycle` 设计中，control logic 在 critical path 上，必须先生成 control signal，然后才是 datapath。

### Single-Cycle uArch 分析

现代处理器，memory 访问非常慢。

导致，`single-cycle uArch` 的指令长尾问题，更加严重。

如果再有访问 hard disk 的指令呢？

因为，一个 cycle 内做完了所有工作，所以，不能重复使用硬件，也没法复用资源。

如果是多 multi-cycle 的，cycle 间就能复用一些硬件和资源。

## Design Principles

设计原则，

`single-cycle uArch` 违反了所有的原则。

## Multi-Cycle uArch

## 优缺点

## Closer Look

what does best mean？

依旧是有限状态机。

`single-cycle` 需要 2 个 memory (或 port)，3 个加法器 （`ALU`，`PC`，`Branch address`）。

## Datapath

### lw instruction

`lw` 指令的执行过程

fetch 是所有指令都一样的。

只用 1 个 memory（port）就可以了。single-cycle 需要 2 个。

只用 1 个 `ALU` 就行了。single-cycle 需要 3 个。

### Other instruction

### Complete Multi-Cycle Processor

1. 新增了 4 个 register 保存 stage 的中间结果。红色框出。

1. 1 个 memory，1 个 `ALU` 加法器。绿色框出。

## Control Logic

### Control Unit

需要一个 control unit，是 `FSM`（有限状态机）。

与 single-cycle 的基本一样，增加了少部分 signal。

### 指令 FSM (有限状态机)

### 完整的 multi-cycle FSM

### 指令扩展

按需增加 control signal 和 datapath 元素即可。

`addi` 只需要增加 control signal，不用加 datapath。

## Summary - uArch Review

微架构

`MIPS` 有限状态机。与 single-cycle 的区别相对大一些。

缺点：

1. 假定了每个 stage 需要 1 个 cycle。但，memory access 可能超过 1 cycle。解决方案，FSM 中，让 state 持续处于 memory state，直到数据 ready。

memory access 持续 loop 等待，直到 ready。

## Another multi-cycle Example

what does best mean?

作者：设计非常的 clean。

对现代处理器的设计，很有参考价值。

### 形式化建模

在 `ISA` 之下，给 `uArch` 定义 `Microinstruction`。

1. control signal 控制 machine 状态，即，`Microinstruction`。

1. `microsquencing`：用 `Microinstructions` 控制有限状态机的状态序列转移。

1. `Control Store`，存储所有 machine 的所有 possible state。可以像存储代码一样，把 `FSM` 的所有 `Microinstruction` 指令都存在 memory 里面。

1. `microsequencer` 就像 `PC` 一样，决定下个 clock cycle 执行哪条 `Microinstruction`

对标于 `ISA` 和 `uArch` 的概念：

1. `ISA` -> `Microinstruction`。实际是 `ISA` 的所有 control signal

1. 实现：

1. 有限状态机：用 `microsquencing` 描述 state sequence。

1. 所有 possible state 在 `Control Store` 里。

1. `microsequencer` 决定下一个 state 去哪里。

### Control Store

一个 control structure 的例子

Control store 公式说明：

1. `pipeline` 共 6 个 stage（状态），每个状态 0/1 共 2 个选项。

1. 每个 `microinstruction` 位宽 35 bit。

control signal 是 microinstruction。

将 microinstruction 应用于 datapath 和 control logic。

在统一 cycle 内同时执行，从而确定：

1. 当前 cycle 的 datapath 应该做什么。（也就是 ISA 的定义？）

1. 下一个 cycle 的 state 是什么（microinstruction）

control structure，有 3 个东西，比前面的 4 个 少了 microsequencing（这个不算 control 的东西吧）

### the Power of Abstraction

control store 的抽象，让 hardware designer 可以做 uProgramming

### uProgramming & ISA translator

## Backup Slides

![](img/989b54190331313f50d60473de76746f.png)
