
# CS143 编译原理笔记 4 - Codegen & Runtime

> 来源：[https://learning.feishu.cn/docx/He6Hd0jQIopCmrxramPciYnBnzb](https://learning.feishu.cn/docx/He6Hd0jQIopCmrxramPciYnBnzb)

## 总结

本文是 Stanford CS143 Compilers 课程 Week 7 的笔记，讲 Codegen 阶段。

对应 龙书 的第 7 章 (Runtime) & 第 8 章（Codegen）。

| weeks | topic | 视频时长 | 用时/二刷 |
| --- | --- | --- | --- |
| week 6-2 | Runtime | 1h | 1h |
| week 7-1 | code gen | 1.5h | 3h |
| week 7-2 | semantics | 1h | - |

注：

1. Week 6 先用 1h 讲 runtime，为 codegen 铺垫背景知识。

1. Codegen 整体感觉比较简单，愉快的快速刷完。如果实际 coding，细节是很多。

1. Week 7 后半段又讲了一些 semantics，估计用处不大，跳过。

提纲：

1. Runtime 就是 OS 基础知识的科普

1. Activation 就是函数调用。

1. `AR`: Activation record，也就是 frame。保存函数运行时的 stack 信息。

1. 1-register Stack machine 的设计和运行机制。

1. Codegen 主要涉及：简单的表达式、control flow，function definition and function call

1. Codegen 的优化。把 temporary 放入 `AR` layout，减少 stack machine 栈顶的移动。

1. object codegen。重点是 layout 设计和 dynamic dispatch。

其他一些 highlight：

1. `AR` layout 和 codegen 需要同时设计好。compile-time 解决。

1. 所有函数的 `AR` 是一棵树，根节点是 `main` 函数。可以用 stack 数据结构保存这棵树。

1. Global 变量在 static data 空间里。program 手动管理的 memory，放在 heap 里。stack 里都是 `AR`，本质是 OS/compiler 自动管理的 memory。

1. Codegen 算法，整体是比较简单和机械的。temporary 和 object codegen 比较有趣，值得一看。

### 堆栈的内存结构

Stack 从上往下增长，heap 从下往上。

![](img/e6c3bc081a65936bc543dda5c5a3910c.png)

## Runtime

### Runtime organization

Codegen 之前，先要了解 what we are trying to generate。

也就是 structuring executable code 的方法，已经有成熟理论了。

要点：

1. Codegen 时，代码区分 compile time 和 run-time。

1. 需要关注 Storage organization，即，内存管理。与 disk 无关。

操作系统调用 program 的科普：

1. OS 分配 program 内存空间

1. Code 加载到已分配空间

1. OS jump 到 program entry point 处。比如 `main` 的代码地址。

### Activation record (函数 frame)

Activation 就是函数的调用。

Activation record，简称 `AR`，也就是 frame。保存函数运行时的 stack 信息。

Lifetime，就是从进入到退出函数的所有 steps。包括子函数调用与执行。

Lifetime vs scope：

1. Lifetime 是 dynamic（runtime）概念

1. Scope 是 static 概念

Lifetime 特点：

1. 天然的 Nested 结构，可以用 tree 描述。A 调用 B 调用 C，他们的 lifetime 是 nested 的。

1. 因为是 tree，可以用 stack 结构保存 currently active procedures。

1. 同一个 program，input 不同，activation tree 可能不同。

`AR` 结构的一种设计 & 具体例子

1. 刚进入函数时，`result` 是空的，只保留 stack 空间，没内容。

1. Control link 应该是指向 caller

1. (*) 和 (**) 是对应 `f` 调用的 return address

1. 只是一种 `AR` 设计。可以设计成别的结构。

![](img/46eef687e6aa42d29a59d08be12c8ef7.png)

Compiler 需要在 compile-time 就规划好 `AR` 的 layout，并生成生成正确使用 `AR` 地址的代码。

因此，`AR` layout 和 codegen 需要同时设计好。

### Globals and heaps

![](img/79f137f4596d2239865b7d39c19f4187.png)

![](img/71163e79fc69baac0042ce4a9e38ee0a.png)

![](img/ac3253bc76063250b18f03bb424a889c.png)

### Alignment

对齐内存空间，可以有效提高 program 执行效率。实际效果与 machine 的实现有关。

![](img/09eff13c950a26491a285105578fda28.png)

### stack machine

3 种 machine：

1. Stack machine，即，内存是一个 stack 结构。

1. register machine。带寄存器的。

1. 1-register Stack machine。有 1 个寄存器的 stack machine。用的就是这个。

Stack machine 的执行过程

![](img/eed310e29562461de9afb3191a69de4f.png)

特点：够用，& 比 register machine 省空间（电路）。

![](img/79db893aac87a011d136289613cf220e.png)

Stack machine 和 register machine 结合：n-register stack machine

把 stack 里 top n 个位置，放在 register 里面。

可以减少 stack 的 `push/pop`

一个 1-register stack machine 执行的例子

要点：

1. `acc` 是 register。最终结果 (top 1 location) 会写在这里。

1. 完成一个 block 的计算，前后的 stack 是相同的。

![](img/72432b58dfb83fe8f91ce140bdfd1ce0.png)

## Introduction

Codegen 的复杂性来源：既要对，又要快。

计算模式的 2 个假设 & 常见的不满足场景

1. 计算是串行的。反例：concurrency。

1. Control flow 总是能回到调用处继续执行。反例：exception，`call/cc`。

![](img/0fd1d75ee1ed6e9ee6bc4892842efda6.png)

实验环境，1-register stack machine。

用 MIPS 或 MIPS 模拟器。

![](img/a369f22208430eb4ddcaba5dc7f9f662.png)

常用的 Instructions

![](img/282461f8f8339aadc495b09ab4f06150.png)

一段 machine code 的例子

![](img/63eeda0fab912538d2d79312d732f6f4.png)

## Codgen 基础（naive）算法

### 问题建模

输入的 语言语法定义如下，（复习 cont[CS143 编译原理笔记 2 - Parsing](https://dwpl6xgouw.feishu.cn/wiki/AbuHwUnb6ixRBAkvYBcc8qocnIc#E57Gd3OLdo1Mwkx9GGcckkJcnIg)xt free grammar 语法： ）

![](img/56d5dfd45f9e1d3c9d87ef872a78c5d8.png)

CodeGen 需要满足以下定义。

![](img/790e3e4905bde3ec9d2226c36d1c44e9.png)

### sample - codegen

Example 1 and convention

![](img/79f52b822da658d057332b1172825ea9.png)

一个 code gen 的完整例子，一般，简写为左边的写法。

![](img/2a7d514a9108fc5513103029fd0f8864.png)

引入 template 的概念

![](img/f67b4d2b374a2673ef8b4cefafab9ea5.png)

### Control flow - code gen

相关机器指令

![](img/5e93a88c4e9781a851b46171d99f1e92.png)

`If` 的 codegen 例子如下

![](img/313f6f3885c02667d28e0d7bf0909ab5.png)

### Function - codegen

分 2 个问题：

+ function definition

+ function call

复述几个 runtime 处讲过的概念：

1. `AR` 是 activation record 的缩写，就是 frame。

1. `AR` layout 设计，很重要。需要与 codegen 同时完成。

`AR` layout 设计如下：

![](img/460c0ada1ae08569195e97f264d35012.png)

![](img/c96af83b3da32035d77f2c68863ec2c5.png)

Function codegen 的例子，分 caller 和 callee 2 部分。

Caller side

![](img/460e575375b178c5fb432423d68734ef.png)

Callee side

![](img/dd82904d53374f202193ba47429776ed.png)

运行时的总体情况

![](img/fb49ac20ff75d9a6e3ca8e8a567c37bf.png)

### Variable reference - codegen

![](img/49c9a2444bc6a65b73a43f4a304fd85e.png)

![](img/131f308f4cf3a14c75c9fcab2e2f34ed.png)

### 完整 Example

Very Good example for learning

![](img/997518c472c6dbe3485705e865af3ae9.png)

## Temporaries 优化指令数

背景: 函数的临时变量，引入了很多的 `push/pop` stack

思路：提前计算出最大的 temporaries 数量，并在 `AR` 里分配好固定的位置。减少 `push/pop`

例子：下面的代码，最多需要 2 个临时变量。

![](img/d40d10eb1b76c9c7fe3d54f7b2a0284a.png)

Temporary 数量分析的算法

![](img/311a51474f58f8f23b92bd352cf12b75.png)

注意，针对 第 4 行，`id` 函数调用的情况，`e_1` 的结果是保存在 callee 的 activation record 里，不在 caller 里。

此处估算的都是 caller 的 activation record 需要的 temporary 数量

![](img/4771d5e31e0764a47b2ff5a7aef4377d.png)

`AR` 里包含 temporary 后的 layout 设计：

![](img/cf8e8e157663d0e786c5e007793c7606.png)

layout 可视化如下

![](img/21ed08880f6e0c3d851786327fef7d01.png)

设计思路备注：

![](img/2ac1b2f9d3919d3c4ffdc474393d59f0.png)

对应的 codgen 对比。少了 `N` 个移动栈顶地址的指令。

| 不用 temporary | 用了 temporary |
| --- | --- |
| ![](img/2f6b85fc1f25a0c52bf35c7960f28fd2.png) | ![](img/55308c63f55caa640fdacc304f7152f3.png) |

## Object 的 codegen 算法

Object 的处理，重点是 layout 和 dynamic dispatch 的设计。

Object codegen 基本等价于经典的 c++ 八股文面试题：介绍一下虚函数是怎么实现的。

![](img/ca792ba370bebc9ef6a6a2dd22140110.png)

### Object Layout

问题分析，说明为什么 object 的 layout 很重要。

![](img/a1a7877a6264a832d6440fcab18dfa0b.png)

![](img/268c22692978fd6ebb140bd94f4a3a8a.png)

Object Layout 设计

![](img/abba7ea01d8994f808e7dfda0cb0bcb8.png)

![](img/7b7e2838cee627f09191435d9a4dd6e0.png)

![](img/6e123fcf49473f06cec42126e17059db.png)

### Dynamic dispatch

c++ 的虚函数，也是这么搞的。

![](img/5220b5565c8876f0e540d22dcfa84f40.png)

![](img/e84c7300ed10bd35b4f69aef1b731383.png)

![](img/0d18dabc82c986ab492be42a9e66a0be.png)
