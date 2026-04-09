# CS143 编译原理笔记 1 - Overview & Lexical

> 来源：[https://learning.feishu.cn/docx/CuUgd48BPoQmdjxN8DicLmMPnJh](https://learning.feishu.cn/docx/CuUgd48BPoQmdjxN8DicLmMPnJh)

# 总结

本文是 Stanford CS143 Compilers 课程 Week 1 & Week 2 的笔记，入门介绍 & Lexical 阶段。

| weeks | topic | 视频时长 | 用时/二刷 |
| --- | --- | --- | --- |
| Week 1 | Overview | 1.5h | 2h |
| week 2 | Lexical Analysis | 2h | 5h/3h |

提纲：

1.  Compiler 的 5 个阶段：Lexical analysis，parsing，Semantic analysis，Optimization，codegen

1.  Lexical：把源码切成 tokens，并标注 token 的 classes（类别）。

1.  语言分 spec 和 implementation。spec，用正则。实现，用有限状态机。

1.  Spec 的设计目标之一，减少 lookahead。

1.  实现的总体流程：正则 -> NFA(非确定）-> DFA -> table based 实现。

1.  Epsilon 和 Epsilon move 是很重要的概念。

其他一些 highlights：

1.  当 Software cost > hardware cost 时，大家开始做 compiler。

1.  演进老的语言，成本一般高于设计新语言。因为，programmer training 是主要成本。

1.  正则语言可以定义 grammar，也就是，描述所有合法 expression 的规则。

1.  Formal language，为了完备性而引入的定义。从一个有限的字符表里面任意搞点 strings 出来，都算。

1.  Finite automata (状态机) 实现的语言，就是这个 Finite automata 的所有 accepted string。

延申理解：从 grammar spec 理解正则表达式是如何设计出来的。

所谓的 grammar，就是所有合法 expression 的规则汇总。

正则表达式，就是为了描述这些 expression 的 pattern 而设计的。

因为，grammar 是正则定义的，所以，直接用正则解析词法（输出 token），自然很方便。

我们平时写的正则，可以理解为：

1.  针对试图解析的内容，设计了一个领域特定的语言（DSL）。比如，描述 Email address 的 DSL。

1.  我们写的正则，就是这个 DSL 的 grammar。

正则的局限性。

在 parsing 阶段会遇到，很多 grammar 无法用正则描述。

比如，nested 的表达式，状态数无限，或者有限但超过正则的有限状态机。

解决方案是，引入了 context free grammar。

Compiler 的 5 个阶段如下

| 编译阶段 | input | output | 备注 |
| --- | --- | --- | --- |
| Lexical analysis | 源码 text | tokens | 把源码切成 tokens，并标注 token 的 classes（类别）。 |
| parsing | sequence of tokens from lexer | parse tree of the program (AST) | 用 tree 表示 sentence structure |
| Semantic analysis | AST from parser | annotated syntax tree | 太难了，只能做有限的分析: 发现 inconsistent &amp; 报错。 |
| Optimization | IR | IR | 比如，常量折叠、死代码删除等。 |
| codegen | IR | 一般是 assembly code | 也可以输出其他高级语言、人类语言等。 |

# Week 1 - Overview

## Compiler History

起源：1954 年，IBM 704 机器.

背景：Software cost > hardware cost

（—— 跟现在 AI 芯片落地的场景相似。）

1953 年，开发了 interpreter，叫 speedCoding。运行效率下降 10x。

于是，开发 Fortran I，计划一年，时间花了 3 年（1954-1957年）。

目的：处理科学计算的 formula translation。解决 interpreter 速度慢的问题。

影响：

1.  1958 年，50% 的 code 用 Fortran I 写的。

1.  现代 compiler 基本都保留了 Fortran I 的 outline。

## Structure of A Compiler

![](img/5eaa443d636784fce53a01d4c42606b7.png)

Semantic analysis 很困难的例子：

下面 even worse 的例子中，最多可能有有几个 Jack？

答：3 个。Jack，Jack，his。

这是 compiler 语义分析 for 循环内的 variable binding 时的常见难题，同名变量最多可能指代多少个不同的值。

![](img/5a12fc7ab1df122473232e45d10121d2.png)

## Economy of Programming Language

3 个问题：

1.  为什么有如此多的编程语言 -- 不同领域，需求不同，甚至 conflict。

1.  为什么有新的语言诞生 -- programmer training 是主要成本。演进老的语言，成本高。

1.  什么是好的编程语言 -- 没有标准

-- 但如果是演进 compiler，相对就容易很多？

## COOL - 为课程设计的语言

全称: classroom object oriented language

目标：编译成 MIPS assembly language。

课程 optional：optimization。

![](img/50d176943f92471e9439fd9e7d883453.png)

# Week 2 - Lexical Analysis

## 简介

目标：

1.  把源码切成 tokens，

1.  标注 token 的 classes（类别）。

Token 的类别，比如：

+   Identifier（变量名），

+   integer，number，

+   keyword（语言的保留词），

+   operators，

+   whitespace，

实现上：

1.  left-to-right 扫描源码 text

1.  需要 lookahead，但，尽可能少的 lookahead。

## Lookahead 的例子

设计 lexical 规则时，很重要的 goal 之一就是，尽可能少的 lookahead。

典型的例子：

1.  正面：变量名不能以数字开头，减少了 lookahead。

1.  反面：== 和 =，必须 lookahead 才能区分。不是一个很好的 grammar 设计。

来 fortran 的奇葩例子，需要很长的 lookahead。

背景：Fortran 里面，空格是没有意义的。即，可以删了所有空格再分析代码。

1.  代码上，2 行的区别是 1, 25 和 1. 25。语义上，完全不一样。

1.  前者，是一个 loop，从 1 循环倒 25.

1.  后者，声明了一个变量 do5I，并赋值 1.25

![](img/155f6609820d42d71a5d8b1af05e3ed1.png)

来自 c++ 的反面教材。很长时间里，c++ 的 compiler 一直有 bug，需要手动加个空格才能区分 >>。

![](img/eb66d9ce96283e806687b5c46ece555a.png)

## Regular Languages（正则语言）

### 正则语言的定义

基本理解：

1.  Lexical structure 等价于 token classes

1.  用 regular languages 描述每个 token classes 包含哪些 set of strings。

正则语言包括 5 个基本表达式：

+   2 个基础 case：single character，epsilon

+   3 个组合表达式：union，concatenation，iteration

![](img/3616e3dbfe19b6ecb3ec2e8752344369.png)

![](img/69055211ac2653e811c148fdfb913811.png)

把以上 5 种情况组合起来，组成了 grammar 的最小定义。

其中,  是希腊字母 Sigma，不是求和。表示语言 alphabet，即语言可用的所有字符。

![](img/8557a56b10cd658ea599e8fa9425e164.png)

一个只包含 0 和 1 的语言 & 正则的例子

![](img/b06eaa0f41a46aaaf3f658fa9d05275a.png)

### 正则的限制

内容来自 week3 introduction to parser: https://www.bilibili.com/video/BV1NE411376V?p=20

很多语言不是 regular。比如：nested 的表达式

![](img/b496d8b5157f96e4ad227359649d05e7.png)

Regular expr 可以表达什么？

1.  有限状态机的状态数为 k，则最多可以表达 mod k 层 nested。

1.  如果提前测试程序并 trace 循环次数，则可以表达 fixed 次数的循环。

1.  如果循环次数任意的，因为没法记录循环次数，所以无法表达。

## **Formal language**

### 定义

![](img/57d6d0f4ea5a6cc757c8a27c18b091d1.png)

注： 是希腊字母 Sigma，不是求和。

（个人感觉，定义了个废话。从一个有限的字符表里面任意搞点 strings 出来，都可以叫 formal language）

formal language 包含很广：

1.  正则表达式，是 formal language。

1.  英语和 c program 也是 formal language。

![](img/b52d0541eba971f629f5b446ad8e22ef.png)

### Mapping function

Mapping function 完成 syntax 到 semantics 的映射。

+   Syntax: expressions

+   Semantics: meanings。也就是，精确的指出 expression 包含哪些 strings 组合。

用 mapping function 描述正则的 5 种基础表达式，如下：

![](img/73276429ee16f95e48a219ffea0463fd.png)

为什么要用 meaning function 做 mapping？

1.  区分 syntax (expression) 和 semantics (meaning) 2 个概念。

1.  把 notion 可以作为一个单独的问题处理。notion 很重要，比如：罗马数字 vs 阿拉伯数字，大部分人用前者做数学运算很困难。

1.  Expresions 和 meanings 不是 1-1 的关系。而是，many-to-one。

many-to-one （多对一）展开讨论：

如果 一对多，会导致二义性的 syntax (expression)。

Many-to-one 是做 optimization 的基础。

即，存在很多种不同的 expressions，他们的 meaning 完全相同。所以，可以切换到 better expression。

## 语言 spec - Lexical specification

### 4 个 spec 例子

用 regular expression 描述语法（lexical spec）的 4 个例子

1.  Keyword 的正则

![](img/8d4b7f078e0b4bae3be6dd2c53cd46fb.png)

1.  Integer 的正则

![](img/5ae6b918ccd63de0afd4b041ab5eed8f.png)

1.  Identifier 正则

![](img/94ba7a1405e9784695bb36df2457f82b.png)

1.  Whitespace 正则

![](img/cd0b505f4488e4f06f2f33da7625edff.png)

### 为什么需要 epsilon

看下面的 opt_fraction 定义，+ epsilon 的数学语义是 or。即，fraction 部分是 optional 的。

![](img/aab5fb35c9ba4c667df0187392c22f4d.png)

### 正则表达式的基本语法

![](img/6231a502aa5e66a584dfe22c9d35cb69.png)

### 正则匹配算法

除了回答：是否满足 regular expression 的集合，

更要回答：匹配的是哪一个 regular expression（即，词法解析）。

匹配算法：

![](img/88d10175fa8acd664fb5613a39dbc7eb.png)

![](img/c21399015fecec8aa5b948afe59fc927.png)

![](img/0df2b2e9a1567a592738391194e87aa2.png)

几个问题：

多个 pattern 都满足时，选哪个?

1.  maximal munch 规则。即，选最长的。比如，== 和 =

1.  按优先级选。

1.  比如，keyword 和 identifier 同时满足。一般 keyword 定义在前面，优先匹配。

1.  注意，不同语言的优先级定义，可能不一样。

如果没有 rule matched，怎么办？

1.  Compiler 大多直接 crash、报错。

1.  尽量不在 lexical 阶段报错，因此，定义一个兜底所有 Error 的 regular expression。

1.  Error 规则的定义，优先级最低。

### 小结

一句话总结：正则做 lexical analysis，非常好用，准确、够用、高效。

![](img/e9b1cbd08dc735a0b33ec9e6a1e70ddf.png)

## 语言实现 - **Finite ****automat****a****(****状态机****)**

### 基本概念

有限状态机，是语言 spec 的 implementation。

包括 5 个组成部分，如下

![](img/a1699b466a3ddad94f41857fe194d50f.png)

常用的概念，Accepting 和 rejected state

1.  Accepting states，也就是有限状态机可以结束的 state。

1.  一个 finite automata 实现的语言，就是这个 FA 的所有 accepted string。

1.  accepted 的 2 个条件：input 已读完，且，state 是 accepting state。否则，rejected。

常用的 notation，重点是 accepting state 的符号。

![](img/149c02f5cf8b5866c409d22012b80800.png)

一个例子：

![](img/91dd11701d31a6327ff285c3071eaf2f.png)

### epsilon-move

epsilon-move 是精髓的设计：不用读入 input，也能跳转 state。

![](img/2c7aeb58f31aa792bc2e61df80ab72e3.png)

### DFA & NFA 有限状态机

1.  DFA，确定性有限自动机。跑得快。

1.  NFA，非确定性有限自动机。图的 size 小，甚至指数级下降。

（本以为，NFA 又是一个为了定义完备而引入的无用概念，实际很有用）

核心区别：NFA 有 epsilon move。

备注：NFA 的每一个 transition 前，都可以加 epsilon move，变成 single transition per input per state。

![](img/fab88c84e10a0eda555f203ccff5738e.png)

更清晰的理解区别：NFA 读了一个 input，可以同时有多个 path 可选。

![](img/855b795285bb8e411319567beab86484.png)

NFA 可以进入 multiple states，只要最终有一个 state accept，就 accept。

![](img/1834606dc230b846ce7c23cec76e291a.png)

### 总体流程

![](img/033416bada8ce0877660b5de235de714.png)

### RegExpr to NFA

构造 NFA 的图，非常简单

对于每一类 reg expr，都定义一个对应的 NFA。

用 epsilon 把基础的 regular expression 自动状态机都连起来即可。

只有 1 个 final state。

规则如下

![](img/ce6f3ee2ed65d993335c6eb996a2f221.png)

![](img/2fd2481061ff2d0cfc97f9aa02e224cc.png)

![](img/392b0c633d2d2a0e73320c87520825f5.png)

一个具体的例子

![](img/0b870735ab9bad8e852769b76237bef1.png)

### **NFA**** to DFA**

难点和精髓，就是 Epsilon-closure of a state。

Epsilon-closure of a state：只用 epsilon 就可以到达的 state

![](img/0b4edf184645e26506e78b1da3131a52.png)

NFA 转 DFA

1.  理论基础：State 数量虽然多，但是有限。如果 NFA 有 N 个 states，DFA 最多 2^N - 1

1.  DFA 的 final state，只要有一个 NFA 的 final state 即可。

1.  DFA 的 transition func 里，输出 Y 是 states 的 subset 且唯一。即，保证符合确定性。

![](img/4bdc3f515fea1db91f4d008b98650700.png)

一个例子（不看原视频，基本绕不清画了啥）

![](img/d5bc1033a2ce4a13d1eb9876f47ee77d.png)

### Table based DFA implementation

简单来说，DFA 可以用一个 2 维的转移矩阵实现。

![](img/3993a5033934e3187726bb8bdf787f33.png)

一个例子

![](img/0c7c53a3ebb3e9659450a85b3db99e8c.png)

压缩转移矩阵空间占用的方法

![](img/9d85f56a2e6d84063438a42edab792bc.png)

也可以直接对 NFA 用 table based 实现

因为 state 数量少，所以，table 的空间占用也很少。但是，table 中的每个 cell，都是一组 state，但不是单个 state。

![](img/e81e40b390e17deabda73b8e5849fd88.png)

### 小结

总体流程如下，核心是 NFA -> DFA。

![](img/2e5445620cb06190ea63e3625e55538a.png)

DFA 和 NFA 做 tradeoff

1.  DFA：faster，less compact（速度快，空间大）

1.  NFA：slow，concise（速度慢，空间小）