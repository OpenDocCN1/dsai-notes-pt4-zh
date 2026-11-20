
# [Lec13] Pipelined Processor Design

> 来源：[`https://learning.feishu.cn/docx/FH6Gd29T6oUEySx1Z58czLztn2g`](https://learning.feishu.cn/docx/FH6Gd29T6oUEySx1Z58czLztn2g)

2023 年，这节课是学生代讲的。所以，整合 2022 talk + 2023 slide。2022 年是 Lec14。

`https://www.youtube.com/watch?v=XaW_O9nKPe0`

Lecture 14: Pipelined Processor Design

ETH Zürich

Lecturer: Professor Onur Mutlu

Spring 2022

## Summary

重点是 Fine-Gained Multithreading，从 1:25:00 开始，一共 20 分钟。

前面都是 data dependence 和 control dependence 的处理。

lec19 SIMD，lec20 讲 GPU。

understand the principles of execution of a GPU。

也是 fine-gained multithreading （FGMT）

pipeline processor 和 out-of-order processor，

在 ISA 层面，都是冯诺依曼模型。

get faults and want to recover faults

1. detect faults。
1. recover from faults
1. end version execution

## Pipeline

## Data Dependence

read after write dependence

## Our Pipeline So Far

## Control Dependence

## Final Pipelined MIPS Processor

## Smarter Branch Prediction

## Pipeline Performance Example

## Further Readings

![](img/9719db40227562f36a403bae842710a7.png)
