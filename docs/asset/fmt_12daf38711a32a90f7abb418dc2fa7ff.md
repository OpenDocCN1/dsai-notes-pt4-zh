
# CUDA 程序与虚拟地址一致性

一个 CUDA 程序，有一部分是由 CPU 完成的（比如初始化和返回结果，下图蓝色部分所示），有一部分是由 GPU 完成的（比如计算和渲染，下图绿色部分所示），里面的变量 `x`, `y` 通过 CPU/GPU 各自的虚拟地址（`VA`）访问，如果这两个 `VA` 能保持一致，将带来编程的方便。`single-pointer-to-data`，也是 UMA 的一个重要组成部分。

这就要求 GPU 的虚拟地址空间和 CPU 匹配，x86-64 的 CPU 一般是 48 位吧，Pascal 架构的 GPU 是 49 位，完全没问题。

`VA` 相同，但 `PA` 可能不同。CPU 的用户态程序虽然和 GPU 使用相同的 address space，但有各自独立的 per-process 页表。

## Weakly-Ordered Memory Model

TODO

## distributed shared memory

MMU 很重

`sm` - work group - cluster 8M

![](img/114c94a0b3a0dab267d5b597a586f233.png)

# Compiler 优化需求

## 为什么重视内存访问

+ 必须清楚 `desirable`（`getter`）和 `undesirable`（`scatter`）的内存访问方式。`gather` 和 `scatter`

+ `getter`:memory 到 `thread` 有多个输入，只有一个输出（图像模糊算法）

+ `scatter`:thread 到 memory 有一个输入，多个输出（每个 `thread` 将结果 `scatter` 到各个 memory，将附近值 +1）

## 内存模型对 GPU 编程中优化方向

+ `global memory`

+ 带宽、内存合并和内存对齐

+ `shared memory`

+ bank conflict、数据分布、conner-turning、async

+ `const cache`

+ broadcast、serialization

+ `L2 cache`

+ persisting

+ `local memory`

+ 数据重用、数据对齐、同步原语

+ `register`

+ 数据重用、循环展开

## 与锁有关的优化方向

+ `global memory` -> `shared memory`

+ `global memory` -> `texture memory`

+ `global memory` -> `warp`/`block`

## 解决数据竞争与数据一致性的锁与内存屏障

+ 数据竞争 -> 锁

+ 数据一致性 -> `memory barrier` / `fence`

# CPU & Linux Memory

https://mp.weixin.qq.com/s/vc9xEkG5FFKkqTJfCb7kzA

Linux 里面用户态和内核态共享数据的两种方式：`read`/`write` 和 `mmap`。可能很多人都觉得 `mmap` 不用拷贝，性能应该更好，但：随着硬件的发展，内存拷贝消耗的时间已大大降低，有些情况下，`mmap` 的性能反倒比不过 `read`/`write`。

Understanding the Linux® Virtual Memory Manager

https://pdos.csail.mit.edu/~sbw/links/gorman_book.pdf
