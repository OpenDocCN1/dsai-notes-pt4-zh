
# LLVM & 编译原理 Readings

> 来源：[https://learning.feishu.cn/docx/Gu7Ldvqtgoz7Vexf6p1crFCmnOd](https://learning.feishu.cn/docx/Gu7Ldvqtgoz7Vexf6p1crFCmnOd)

## 入门材料

### 学习路线图

新手入门，主要是 2 个材料：

1.  `LLVM Essentials-2015.pdf`
1.  2019 EuroLLVM Developers’ Meeting - LLVM IR Tutorial

预置条件：

[`CS143 编译原理笔记 - 提纲`](https://deep-learning.feishu.cn/wiki/YzyCwQzcUi8R0LkG243cCPRvnwg?fromScene=spaceOverview)解简单的编译原理，效果会更好。入门推荐

要点提纲

1.  掌握 LLVM common Tools
1.  掌握 LLVM IR 的 API。看的懂手册、会用 `llvm::IRBuilder`。
1.  Pass Infra
1.  Codegen

不覆盖的内容：

1.  intrinsic
1.  exception
1.  metadata & attribute
1.  vector instruction
1.  debug information & DWARF

### LLVM Essentials - 2015

`LLVM Essentials-2015.pdf`: sourceforge 下载链接

非常适合入门 LLVM，只是 2015 年的书，API 有点老了。

我用 chatGPT 重新写了一下 tutorial code，并用 `Clang19` 跑通了。

快的话，一个周末就可以刷完。

书 200 多页。精髓，都在 code 里面，每个 chapter ~100 行代码。

特别是 Building LLVM IR - scalar add，做完就感觉会写 LLVM 代码了。

看懂代码，可以跳过书中的大部分文字解释。

### LLVM IR Tutorial - 2019

2019 EuroLLVM Developers’ Meeting - LLVM IR Tutorial

https://www.youtube.com/watch?v=m8G_S5LwlTo

slides: https://llvm.org/devmtg/2019-04/slides/Tutorial-Bridgers-LLVM_IR_tutorial.pdf

对 GEP 的讲解，非常深入，非常精髓。

## 个人理解

通常，大家都会推荐这些书：

1.  编译原理相关书籍
1.  [`CS143`](https://web.stanford.edu/class/cs143/)， [`Bilibili`](https://www.bilibili.com/video/BV1NE411376V/)
1.  鲸书：高级编译器设计与实现
1.  龙书：编译原理
1.  `Engineering a Compiler, Second Edition.pdf`
1.  `The SSA Design Book.pdf`
1.  体系结构相关
1.  Computer Architecture - A Quantitative Approach
1.  `Computer Organization and Design RISC-V edition.pdf`
1.  编译相关经典论文
1.  `The Compiler Design Handbook, 2nd Edition (Dec 2007).pdf`
1.  PLDI 论文
1.  ISCA 论文
1.  CGO 论文
1.  LLVM 相关学习书籍。low-level IR, optimization, and code-generation
1.  `Getting Started with LLVM Core Libraries.pdf`
1.  `LLVM Cookbook.pdf`
1.  `LLVM Essentials.pdf`
1.  看源码的 `examples` 目录

传统的建议，一般包括：

1.  学习编译原理，刷书。比如，龙书，鲸书，`Engineering a Compiler`。
1.  LLVM 看书 & 做练习。比如，`LLVM essentials`，`LLVM Cookbook`。
1.  读 LLVM 源码。特别是，源码的 `examples` 目录。
1.  Start with the LLVM Developer Meeting tutorials and presentations. They're on YouTube.

个人看法，这种入门，造就了 2 种群体：

1.  熟悉 LLVM API 但不懂编译原理和算法。
1.  懂编译原理，但不会 LLVM。基本写不了代码。

传统学习编译器，是为了创造新语言，所以，重点是 frontend。

现在学习编译器，是 hardware 适配 & 性能优化。即，后端 `codegen` 和 中端 `opt`。

另外，deep learning model 本身就是 DAG，也不需要词法解析构建 AST 了。

LLVM 最新的 tutorial，实现 Kaleidoscope 语言，仍有 10 个 chapter 中的 2 个在写 Lexer 和 parser。

如果能基于 LLVM 提供的 compiler infra 来学习编译原理，效果可能会更好。

CS 6120: Advanced Compilers: The Self-Guided Online Course

https://www.cs.cornell.edu/courses/cs6120/2020fa/self-guided/

https://www.cs.cmu.edu/afs/cs/academic/class/15745-s19/www/syllabus.html

Compilers: Principles, Techniques, and Tools (2nd Edition)

## 阅读材料

官方文档，主要看 `UserGuides` 系列 https://llvm.org/docs/UserGuides.html

重点包括：

+   `Optimizations` （终端）。
+   Writing an LLVM Backend: https://llvm.org/docs/WritingAnLLVMBackend.html
+   The LLVM Target-Independent Code Generator: https://llvm.org/docs/CodeGenerator.html
+   `TableGen`: https://llvm.org/docs/TableGen/index.htmls
+   Code Generation （后端）。比如：Writing an LLVM Backend

`Clang` （前端）相关的，在独立的 https://clang.llvm.org。

比较重要的内容：

+   https://clang.llvm.org/docs/InternalsManual.html
+   https://clang.llvm.org/docs/Tooling.html

LLVM 官方的 `GettingStarted` 系列

+   https://llvm.org/docs/GettingStarted.html
+   https://llvm.org/docs/ProgrammersManual.html

LLVM 后端 - `CodeGen`

+   [Build a Modern Computer from First Principles: From Nand to Tetris](https://www.coursera.org/learn/build-a-computer). Taught by Shimon Schocken and Noam Nisan. This course starts backward: first, you build the logic gates (and, or, xor, etc.), then use the logic gates to construct Arithmetic-Logic Unit (ALU), and then use the ALU to build the CPU. Then you learn how to control the CPU with zeros and ones (machine code), and eventually, you develop your assembler to convert the human-readable assembly into the machine code.
+   Parsing `Mach-O` files(🔌). This is a short article written by me. It shows how to parse object files on macOS (`Mach-O`). If you are on Linux or Windows, search for similar articles on `elf` and `PE/COFF` files, respectively.
+   Performance Analysis and Tuning on Modern CPUs. The book by Denis Bakhvalov. While it is about performance, it gives an excellent introduction to how CPUs work.

Clang & LLVM 前端

+   Understanding the Clang AST. This article is written by Jonas Devlieghere. It goes into detail and touches implementation details of Clang’s AST. It also has a lot of excellent links to dive deeper into the subject.
+   [`clang-tutor`](https://github.com/banach-space/clang-tutor/). This repository maintained by Andrzej Warzyński. It contains several Clang plugins covering various topics, from simple AST traversals to more involved subjects such as automatic refactoring and obfuscation.

### 优质 Paper

https://llvm.org/pubs/

| Title | year | Author | Notes |
| --- | --- | --- | --- |
| LLVM: A Compilation Framework for Lifelong Program Analysis & Transformation | 2004 | Chris Lattner | |
| The LLVM Compiler Framework and Infrastructure | | Gennady Pekhimenko | |
| | | | |
| Making Graphs Reducible with Controlled Node Splitting | | | |
| Partial Redundancy Elimination in SSA Form | | | |
| MLIR: A Compiler Infrastructure for the End of Moore’s Law | 2020 | Chris Lattner | |
| Compiling ONNX Neural Network Models Using MLIR | 2020 | IBM | |
| | | | |

![](img/360ff015ea0290930cffaacc223be2ec.png)

[`2004-LLVM.pdf`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Jvf1b9AP1osn9Ix90Lnct8Zgn0d)

![](img/c28633ee9a98c755a66fc0b012817ccd.png)

[`2020-mlir.pdf`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KyM4bgS4woqQ0bxi8yJccUdPnsd)

![](img/841a18ffe1a53f2d93789aae09aaa004.png)

[`2020-onnx-mlir.pdf`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Pdh6bxzi8ofiJExM7C5c6kRDnVC)

`llvm`

#### Big Picture

[1] Jeanne Ferrante, Karl J. Ottenstein, Joe D. Warren. The program dependence graph and its use in optimization. ACM Transactions on Programming Languages and Systems, Vol.9, No.3, July 1987, Pages 319-349.
[2] Stanislav Manilov, Christos Vasiladiotis, Björn Franke. Generalized Profile-Guided Iterator Recognition. The University of Edinburgh. CC’18, February 24–25, 2018, Vienna, Austria.

Compiler-Based Code Improvement Techniques, an [unpublished paper](https://link.zhihu.com/?target=https%3A//www.clear.rice.edu/comp512/Lectures/Papers/survey.pdf) by K.D. Cooper, K.S. McKinley, and L. Torczon (local copy)

#### Value Numbering

P. [Briggs](https://link.zhihu.com/?target=http%3A//dx.doi.org/10.1002/%28SICI%291097-024X%28199706%2927%3A6%253C701%3A%3AAID-SPE104%253E3.3.CO%3B2-S), K.D. Cooper, and L.T. Simpson, "Value Numbering," Software-Practice and Experience, 27(6), July 1997. (DOI)

#### Global Common Subexpression Elimination

J. Cocke, "Global Common Subexpression Elimination," [Proceedings](https://link.zhihu.com/?target=http%3A//doi.acm.org/10.1145/800028.808480) of a Symposium on Compiler Construction, Sigplan Notices 5(7), 1970, pages 20--24. (DOI)

#### Global and Interprocedural

K. Pettis and R.C. Hansen, "Profile Guided Code [Positioning](https://link.zhihu.com/?target=http%3A//doi.acm.org/10.1145/93542.93550)," Proceedings of the ACM SIGPLAN '90 Conference on Programming Language Design and Implementation (PLDI), White Plains, NY, July 1990, pages 16-27. (Also SIGPLAN Notices 25(6).) (DOI)

K.D. Cooper, M.W. Hall, and L. Torczon, "An Experiment with [Inline Substitution](https://link.zhihu.com/?target=http%3A//dx.doi.org/10.1002/spe.4380210604)", Software--Practice and Experience 21(6), June 1991, pages 581-601. (DOI)
