
# Dataflow Execution Model

> 来源：[https://learning.feishu.cn/docx/IxWQdAW01oxSl4x7x8fcw1wlnoe](https://learning.feishu.cn/docx/IxWQdAW01oxSl4x7x8fcw1wlnoe)

Lecture 11 https://www.youtube.com/watch?v=A5Uq5COaEG4

principles & single-cycle uarch

Lecture 12 https://www.youtube.com/watch?v=oRXK2OlIswE

multi-cycle Microarchitecture

Onur Mutlu, ETH Zürich

Spring 2022

## Dataflow Model Introduction

sequential 的最大好处，是 debugable。

Dataflow ISA representation

dataflow node example

下图，在做的是阶乘。输出是 N 的阶乘。

## ISA-Level Tradeoff

DL accelerator 应该是 Dataflow Model 比较好。

tradeoff 上，一般认为：

1. sequential model 的 hardware 复杂度更低。
2. dataflow model 的 performance 更高，但 hardware 复杂度也高。复杂度也来源于极高的并行度。
3. 能耗比上，dataflow 的能耗一般更低。因为，sequential model 需要反复维护 `PC` 的递增。

## Micro Arch - Level Tradeoff

Micro Arch 可以用 dataflow 乱序执行，只要给 programmer 可感知的与 sequential ISA 一样就可以了。

也就是，micro-arch 层面可以乱序执行，基本是，现在的 HPC 机器都是这么设计的。

## More Reference

1970s 和 1990s 是 dataflow 的 2 个 golden time

![](img/9c0f8e4839b4cfef418abaebe28e5921.png)
