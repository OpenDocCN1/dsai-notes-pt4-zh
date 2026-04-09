# CS143 编译原理笔记 5 - Optimization

> 来源：[https://learning.feishu.cn/docx/KDXVdOXYGoFPsBxkSw1cgXAMnlc](https://learning.feishu.cn/docx/KDXVdOXYGoFPsBxkSw1cgXAMnlc)

# 总结

本文是 Stanford CS143 Compilers 课程 Week 8 的笔记，讲 Optimization 阶段。

| weeks | topic | 视频时长 | 用时/二刷 |
| --- | --- | --- | --- |
| week 8 | optimization | 2h | 4h |

Optimization 是 compiler 的核心，博大精深。这个课只是入门介绍，但很精髓。

提纲：

1.  Optimization 的关键概念：IR，basic block， control-flow graph。

1.  Optimization 的 3 个粒度：local，global，Inter-procedural。

1.  dataflow analysis 的 2 个例子：constant propagation 和 liveness analysis

其他一些 highlight:

1.  optimization 的 2 个利器：basic block，single assignment form。

1.  block 里面不含 control flow，而 cfg（control-flow graph）里可以有。

1.  block 的代码 fully predictable，优化更简单。

1.  optimization 的中文是**最**优化。有误导，实际上，只是 improvement，并无法保证 best。

1.  dataflow analysis，用一些 rules 描述信息在 statements 之间的 change，不断 apply rules，直到全部满足。

1.  rules 有优先级（ordering） 且 statements 数量有限，因此可证，有限次重复 apply rules 必然能全部满足。

# IR

1.  介于 source code 和 target code 之间的一种 language，提供 intermediate level 的抽象能力，简化 optimization 算法。

1.  拥有无限的 register，所以，IR 的 codegen 简单了很多。

1.  表达式的形式，一般采用 three-address code

Three address code

![](img/da7d6c4bd3cb37443f08457b4bb5b21c.png)

IR 的 codegen 算法

![](img/db4662b43d1fd64b46b5293874c18d92.png)

# Optimization overview

几个重要的概念：

1.  IR

1.  block。

1.  control-flow graph

IR，专门为 optimization 设计的抽象 level。

![](img/384598684762123fe766772dc93a3e08.png)

## Block

basic block，或简称 block，没有 label 和 jump 的最长的一段 statement groups。

+   核心：block 的代码是 fully predictable 的。严格按顺序逐条执行的。

+   收益：易于分析和优化 cpu pipeline。

解释：

1.  Jumps 就是 control flow。一般 if、loop、continue 等会用到。

1.  Label 是其他代码片段能直接跳入的地方。

易于优化的例子：

常量折叠、copy elimination 等。如果有 control flow，不能假定代码执行顺序，也就不好分析能不能 eliminate。

很多优化，只看 block 内是不行。

比如，下图的 (2) 是否可以删除，只看 block 内是不行的。因为，block 可能小于变量作用域，还要分析其他 相关的 block。

![](img/fa5f70960971fde83258049683b747bf.png)

## control-flow graph

一个函数的 body，可以看作是一个 control-flow graph。

此处，没要求 graph 是单进单出的，没有任何约束。

简称也是 CFG，与 context free grammar 的简称一样。

![](img/422ca8eec0007399e50c029eb10071f7.png)

## Optimization 的 3 个粒度

3 个粒度，优化的代码 scope 逐渐增大，难度增大，做的 compiler 也越少。

1.  Local optimization: basic block 内

1.  Global optimization：control-flow graph (method body) 内

1.  Inter-procedural: 跨函数 (method boundaries) 的优化

# Local Optimization

## 优化 1：代数化简

最后 2 个乘法换 << 运算符的优化 pass，在 modern machine 上可能没啥加速效果了。

但是，在传统 machine 上，很重要。

![](img/026f8dba7f165d531fda056426eb12da.png)

## 优化 2：compile time computation

也就是 constant folding。

需要 op 的所有入参都是 constant。

![](img/df1074b82ea7b94e8d36bca39711daad.png)

注：constant folding 可能导致计算误差。

Cross-compiler 场景。2 个机器的 float point rounding 算法可能不一样，产生 rounding error。

## 优化 3：死代码删除

![](img/22eb110bdf767fd06b3a97020a735df5.png)

## SSA - Single assignment form

可以简化 optimization 算法的常用 IR。

课程讲的过于简单了。推荐：

+   北大程序分析笔记（SSA和稀疏分析）https://mp.weixin.qq.com/s/-8MsB79v8T5c1CaGYiM2Gg

+   完整课程的主页: https://xiongyingfei.github.io/SA/2018/main.htm

![](img/697336767fc6f4e12daa7ee66f8fd50f.png)

用了 single assignment 以后，

Common sub-expression elimination 就比较容易做了。

## 小结

常见的优化 pattern：-- 对应于 llvm 的 Canonicalizer pass 吗？

1.  Algebra simplification

1.  Copy propagation

1.  Constant folding

1.  Common subexpression elimination

1.  Dead code elimination

为什么 patter match and rewrite 时，要反复的 repeat

![](img/d02026b042b695ff1e5600bcc207fadb.png)

# Peephole Optimization

针对 assembly language 的 optimization 算法。

![](img/7e91114b8bb479e64a80b7b41961d8ef.png)

![](img/8ebb5ebbabf28367ad7f520292a144cf.png)

与 local opt 的区别是啥？

![](img/ba1b4355491b15e94427fd3f4d7fe59e.png)

# dataflow analysis

Global Optimization 需要先做 dataflow analysis

在 cfg 里做 常量折叠，

1.  难点：需要检查 "all paths"，包括 loop 和 conditional branches

1.  检查方法：global dataflow analysis

![](img/e04b67eaad0f361ee4a8a4f4e48a64ba.png)

例如，一个满足条件的 constant folding

![](img/eb1fb12e9d11db1a879b66adf440d503.png)

特点：

![](img/3bff783c496467fd2b1214837dd64044.png)

# Constant propagation

## 问题定义

符号定义

![](img/872fb1cacca086d90db4560a4e1ff155.png)

分析的 example

![](img/e8c580ca296a3a1a6103c3c226cce553.png)

代码分析的本质：

1.  A combination of sample rules

1.  Rule 描述了 the change in information between adjacent statements

针对一个 statement，需要 2 大类 rules：

1.  根据 in 推 out

1.  根据 out 推 in

## Rules

1.  Rule 1 - rule 4: 根据 in 推 out

1.  Rule 5 - rule 8: 根据 out 推 in

![](img/ad5409e363b7e20d8050e63b6bc54a2e.png)

![](img/db58ea71556376fb7e8f1ec55447e30d.png)

![](img/ef64d7c5b74f3571ed9d3c64ff9bc108.png)

![](img/e30ade2dccbf2797d553685e135844d6.png)

（以下是根据 out 推 in）

![](img/c33f26880e41c7d4074e3ad56591cc6b.png)

这个讨论起来有些复杂的，x != bottom.

反证法，如果 x=bottom, 则，dead code elimination 已经删了这一行。

![](img/014f6d111b8fc189a9703e46ca2844fc.png)

与 rule 6 的推导，基本一样。

![](img/537884222816ba2be7ef54496732b252.png)

![](img/f4d5b22629261df676aae3298059a7fe.png)

## 完整算法

核心：

1.  先全部初始化一个默认值。

1.  不停的重复 apply 所有 rules，直到全部满足 rules。

（下一节的 Orderings 会论证，有限次的 apply，一定能满足条件）

![](img/52a6afbfc788cfbde1822356bf82ea4a.png)

例子

![](img/997c9e49dc4c5f81f382a5d0df0c2eb9.png)

Analysis of Loops，

带 loop 的例子，如下。

分析的方法相同，但，能说明为什么需要定义 bottom。否则，无法分析右下角 node 的 input

![](img/59cbb041010e555df53e563f22761232.png)

## Orderings

简单来说：

1.  Ordering 就是，当有多个 rule 可选时，优先选择哪一个 rule 的 ordering 定义。

1.  rules 有优先级（ordering） 且 statements 数量有限，因此可证，有限次重复 apply rules 必然能全部满足。

详细说明如下：

![](img/ca8c1a328e8059ef749488c3f33b82ff.png)

![](img/969be5a54e3a093f57e149263c87bfa2.png)

![](img/fae95bd44ddc700fef86926f9d00eda0.png)

# Liveness analysis

## 问题定义

Dead code elimination，即，检查 statement 的 liveness，并删除 liveness 为 false 的 statement。

核心是怎么检查 liveness。

Liveness 比 constant propagation 简单。因为，

liveness 的属性是 boolean，只有 true / false。

constant propagation 时，属性可能是任意的 constant 变量（string，int，number 等）

Liveness 的定义：

![](img/6bd37cde4e4747b696ec634c65d7e6c1.png)

## Rules

Rules 简单很多，只有 4 条。

![](img/e170c8567c5678b0c10e4a9a1ddf83e2.png)

![](img/a76d5456c448ef87837bb67cab471b3e.png)

![](img/7db04f1bd602691798d09b9a0c2e8639.png)

![](img/2102d37cbb9b93405f8788db0a27fa8d.png)

## 完整算法

类似 constant propagation：

1.  先全部初始化一个默认值 （false）。

1.  不停的重复 apply 所有 rules，直到全部满足 rules。

![](img/59680768d39b925966351874dfc9760b.png)

一个例子

![](img/0a42cc1afcda42bbaa7ac5d76dcdad89.png)

Ordering 定义：

1.  只能从 false 变成 true。不能逆向改变。

1.  因此，每个 value 之后改变一次。