
# Loop Optimizations by Compilers

> 来源：[https://learning.feishu.cn/docx/DaxJdGjAyoPNVIxgdNScxLH8nPb](https://learning.feishu.cn/docx/DaxJdGjAyoPNVIxgdNScxLH8nPb)

## 参考资料与相关链接

[Loop Tiling](https://dwpl6xgouw.feishu.cn/wiki/JfX0wpTfliwXyHkCNLHcuLAtnpd?fromScene=spaceOverview)

详细介绍 https://homepages.dcc.ufmg.br/~fernando/classes/dcc888/ementa/slides/LoopOptimizations.pdf

https://www.youtube.com/watch?v=UVZPtBGV8kQ

Revisiting Loop Fusion, and its place in the loop transformation framework. - Kit Barton

2018 LLVM Developers’ Meeting

https://zhuanlan.zhihu.com/p/315041435

循环优化之循环合并（loop fusion）

https://zhuanlan.zhihu.com/p/337756824

循环优化之向量化并行（vectorization）

看一下 `prefetch` 对 loop 优化的影响

http://www.cs.cmu.edu/afs/cs/academic/class/15745-s19/www/lectures/L20-Global-Scheduling.pdf

https://zhuanlan.zhihu.com/p/588617980

loop optimization 详细介绍

https://homepages.dcc.ufmg.br/~fernando/classes/dcc888/ementa/slides/LoopOptimizations.pdf

## 循环表示与控制流分析

A CFG is the control flow graph of the program that gives a look into how the program may be executed through the various basic blocks.

By dominance information, we get to know about the relation between the various basic blocks in the CFG.

dominator tree

start by looking at how loops are represented in LLVM, use the concept of dominance and CFG to optimize loops.

We will use Loop Simplification (`LoopSimplify`) and Loop Invariant Code Motion optimizations for loop processing

## 编译优化教材与分类

编译优化，教科书主要是：

+ 龙书

+ Allen & Kennedy 的 Parallelizing Compiler for Modern Architecture

+ engineering a compiler（作者是Kennedy的早期学生），将 scalar 优化多一些。

Kennedy 是并行计算、并行编译的鼻祖之一。

龙书的作者之一 Monica S Lam 也是一个鼻祖。

编译优化主要分为：

+ scalar 优化。分析复杂的控制流，以数据流分析为基本框架。

+ vector 优化。偏复杂的数据依赖分析，主要考虑局部性和并行性。

![](img/375c8fdf1461606a9895accd95501333.png)
