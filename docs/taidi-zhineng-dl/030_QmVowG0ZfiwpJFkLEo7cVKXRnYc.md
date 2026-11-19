
# Introduction to LLVM - 巴西 UFMG 课程笔记

> 来源：[https://learning.feishu.cn/docx/WVtldJCKDomM4txpNm7cm2apndf](https://learning.feishu.cn/docx/WVtldJCKDomM4txpNm7cm2apndf)

## 课程信息

| slides | https://homepages.dcc.ufmg.br/~fernando/classes/dcc888/ementa/slides/YouTubeLLVM/ |
| --- | --- |
| YouTube | https://www.youtube.com/playlist?list=PLDSTpI7ZVmVnvqtebWnnI8YeB8bJoGOyv |
| b 站 | https://www.bilibili.com/video/BV1Xr4y1k7Bj/ |
| LLVM 信息 | Code: `https://github.com/llvm/llvm-project.git` |
| 时长 | 共约 2 小时。 |
| 点评 | 1. 不错的新手入门材料。划出了重点 & 脉络结构，明确了学习目标。 |

## 要点总结

1. 一共 3 个东西：
   - 前端 `clang`：词法和语法分析，生成 `AST`（抽象语法树）
   - 后端 `llc`：`codegen`
   - 中端 `opt`：`IR` 优化
2. 重点是 `opt`。分 3 个 topic
3. `Visualization` 方便 debug
4. `Pass` 分 2 种：analysis pass，transform pass
5. 一般会写成一个 tool，封装依赖的多个 pass。
6. 高级概念包括：`Loop` 的处理
7. 重要的基本功
8. `Region analysis`：掌握 2 件事
9. 4 个 motivation: 并行性好，`ssa` 构建，control dependency，side-channel detection
10. `visit regions` 的 example code
11. `metadata`：
    - 可以理解为 `IR` 之外的补丁，存储不能放在 `IR` 里的其他信息。
    - Debug 信息放在这里，还是很方便的。比如，原始 source code 片段的信息。
    - Pass 内使用 `metadat` 的 `api` & example code
    - 使用 `metadata` 在 `IR` 之间跟踪 `variable` 的变化
    - `TBAA metadata`，为程序中指针和内存操作提供别名信息，以便优化器能够更好地推断和优化代码。
12. `Clang tools`，讲了 3 节课，但感觉不太重要。
13. `AST Matchers`。问题：`ast` 是不是 `clang` 里的概念，`opt` 里还有 `ast` 吗？
14. 后端 `llc`，没涉及。

备注：

课程本身上 `Clang 10`。

Mac 上的 `Clang 17`、`Clang 19`，复现本文的 code，情况较好。

Ubuntu 上的 `Clang 15`，复现不好。

## Quick introduction to LLVM

名字缩写：`Low Level Virtual Machine`

看起来，类似 java 的 virtual machine。实际上，不严谨。

`LLVM` 更像是 `Compilation Infrastructure`。体现在 2 点：

1. 基于 `LLVM` 可以更方便的开发 `compiler`。典型代表，如下表。
2. 作为一个 infrastructure 的意思是，
   - 提供了一些的 tools。包括，前端、后端、middle ends 的 tools
   - `Intermediate Representation` 机制。

基于 `LLVM` 的 Compiler Frontend

| frontend | Tool name |
| --- | --- |
| c/cpp | `clang` |
| rust | `rustc` |
| julia | `julia` |
| swift | `swift` |

## LLVM Tools

重点是 3 个工具

| Tool | Input | Output | 功能 |
| --- | --- | --- | --- |
| `clang` | `file.c` | `file.ll` | c/cpp 转 `LLVM IR` |
| `opt` | `file.ll` | `file.ll` | 优化器 |
| `llc` | `file.ll` | `file.o` | 指令发射 |

### clang

```
# -S: 生成 assembly 文件，此处是 x86 架构的
clang -S file.c -o file.ll

# -emit-llvm: 生成 LLVM IR 格式的 assembly 文件
clang -S -emit-llvm file.c -o file.ll
```

LLVM IR 的例子

![](img/95e1fd5ce2bdf1bdea1b60e1e8cf95b8.png)

### opt

```
# 把 LLVM IR 转 dot 文件，方便用 graphviz 的 dot 工具可视化
opt -dot-cfg file.ll -disable-output
```

`dot` 可视化的例子如下

![](img/f07e4762dedff8ea2e07c0f60e2a4ff9.png)

```
opt -opaque-pointers -dot-cfg -debug-pass-manager file.ll -disable-output
```

```
Skipping pass CFGPrinterPass on prefix_sum due to optnone attribute
Skipping pass: CFGPrinterPass on prefix_sum
Running pass: VerifierPass on [module]
```

### llc

```
# 生成 arm 文件
llc file.ll -march=arm -o file.arm
```

可以生成很多种硬件架构的 binary

![](img/528fc0bcbc981e4be8015ca43fc0e796.png)

arm warning

```
'x86-64' is not a recognized processor for this target (ignoring processor)
'+cx8' is not a recognized feature for this target (ignoring feature)
'+fxsr' is not a recognized feature for this target (ignoring feature)
'+mmx' is not a recognized feature for this target (ignoring feature)
'+sse' is not a recognized feature for this target (ignoring feature)
'+sse2' is not a recognized feature for this target (ignoring feature)
'+x87' is not a recognized feature for this target (ignoring feature)
'x86-64' is not a recognized processor for this target (ignoring processor)
'x86-64' is not a recognized processor for this target (ignoring processor)
'+cx8' is not a recognized feature for this target (ignoring feature)
'+fxsr' is not a recognized feature for this target (ignoring feature)
'+mmx' is not a recognized feature for this target (ignoring feature)
'+sse' is not a recognized feature for this target (ignoring feature)
'+sse2' is not a recognized feature for this target (ignoring feature)
'+x87' is not a recognized feature for this target (ignoring feature)
'x86-64' is not a recognized processor for this target (ignoring processor)
'x86-64' is not a recognized processor for this target (ignoring processor)
'x86-64' is not a recognized processor for this target (ignoring processor)
```

riscv warning

```
'x86-64' is not a recognized processor for this target (ignoring processor)
'generic' is not a recognized processor for this target (ignoring processor)
'+cx8' is not a recognized feature for this target (ignoring feature)
'+fxsr' is not a recognized feature for this target (ignoring feature)
'+mmx' is not a recognized feature for this target (ignoring feature)
'+sse' is not a recognized feature for this target (ignoring feature)
'+sse2' is not a recognized feature for this target (ignoring feature)
'+x87' is not a recognized feature for this target (ignoring feature)
'generic' is not a recognized processor for this target (ignoring processor)
'x86-64' is not a recognized processor for this target (ignoring processor)
'generic' is not a recognized processor for this target (ignoring processor)
'+cx8' is not a recognized feature for this target (ignoring feature)
'+fxsr' is not a recognized feature for this target (ignoring feature)
'+mmx' is not a recognized feature for this target (ignoring feature)
'+sse' is not a recognized feature for this target (ignoring feature)
'+sse2' is not a recognized feature for this target (ignoring feature)
'+x87' is not a recognized feature for this target (ignoring feature)
'generic' is not a recognized processor for this target (ignoring processor)
```

## LLVM IR

[LLVM IR](https://deep-learning.feishu.cn/wiki/AnVVwMFRwiDZBpkjs8hck3v9ngc?fromScene=spaceOverview)

如前面已介绍的命令，从 `cc` 生成 `LLVM IR` 的命令如下

```
clang -S -emit-llvm callee.c -o callee.ll
```

![](img/f8f81bc20476169e1d2e402b02e33f5a.png)

语法高亮后的版本

![](img/c06ac138c947856e39efc7062610ae29.png)

几个要点解读：

1. `Low Level`。硬件指令上，用了：`alloca`，`store`，`load`，`add`，`ret` 等。
2. 强类型的。此用了 `i32`，`i32*`，`i32**` 等

可以被 `pass` 自动优化。比如，`-mem2reg`

```
# compile without optimization
clang -Xclang -disable-O0-optnone -S -emit-llvm f.c -o f.ll

# apply mem2reg optimization
opt -S -mem2reg f.ll -o f_opt.ll
# new version, changed to
opt -S -passes=mem2reg f.ll -o f_opt.ll
```

`mem2reg` 介绍：把分配到 stack 上的变量 move 到 register 上面。

| 优化前 | 优化后 |
| --- | --- |
| `allocate` 和 `store` 指令，是操作 stack 变量的 | |

![](img/9a1ab865fbdfb7fbd3297888facbc4b9.png)

## OPT

1. `visualization`
2. `Analysis`
3. `Transformation`
4. `Instrumentation`
5. `Optimization`

### Program Visualization

工具 `DOT`: Graph Description Language

命令

```
clang -S -emit-llvm diag.c -o diag.ll

# 生成 .identity.dot
opt -dot-cfg diag.ll
# cfg only
opt -dot-cfg-only diag.ll
# regions only
opt -dot-regions-only diag.ll
# Dominator Tree
opt -dot-dom-only diag.ll
# The Call Graphs
opt -dot-callgraph fact.ll

# more
opt --help | grep dot

# 转 pdf
dot -Tpdf identity.dot -o identity.pdf

# 使用 graphviz 可视化
sudo apt install graphviz
```

画了一个 `CFG` 图

![](img/01ef05319399bf8346349f07cad7649a.png)

![](img/e5e1179d4db006e46ed5b56f76a2a880.png)

可以看到，一共 4 个 region。

Region 是单进单出的。

![](img/b2e112b6c6c905f21512608092d5a470.png)

在 `loop optimization` 时用的概念。

详细介绍 https://homepages.dcc.ufmg.br/~fernando/classes/dcc888/ementa/slides/LoopOptimizations.pdf

![](img/46267eefe5e39ecc6f229d2568bedd83.png)

![](img/c60dd0d9a50a7dd47dde34634ad9fbf9.png)

### Passes

![](img/6c1b444de729b375f6ed4bb2986bb631.png)

1. `Transform pass`
2. `Dead code elimination`
3. `Constant propagation`
4. `Loop invariant code motion`
5. `Analysization pass`。辅助其他 `pass`，提供信息。
6. `Range analysis`
7. `Scalar evolution`
8. `Dominator tree`

其他类型的 `pass`

1. `Loop pass`
2. `Function pass`
3. `Module pass`
4. `Call graph SCC Pass`
5. `Region pass`
6. `Machine function pass`



# Write an LLVM Analysis Pass

## References

`https://llvm.org/docs/WritingAnLLVMNewPMPass.html`

`https://llvm.org/docs/Passes.html`

2019 LLVM Developers’ Meeting: A. Warzynski “Writing an LLVM Pass: 101”

## Goal

```
%a = add i32 1, 2
%b = add i32 3, 4
%c = add i32 a, b
```

![](img/6ef9b71340b18a8141a8d6dca96632fd.png)

## Transform Pass

继续完成了前面的 `code`。

更多的是一个过于简单的 `coding reading`。

## Write a LLVM-based Tool

写完这个以后，运行的时候，就不用手动指定特定的 `pass` 了，tool 本身已经精准注册了。

## LLVM Loop - TODO

## Region Analysis

输入 `code`，输出 `regions`

![](img/40da6e817b67e1ddb82b3ceb9e6e0023.png)

LLVM 提供了一个数据结构，把 `regions` 组织为一个 `tree`: `The Program Structure Tree`

Richard Johnson, David Pearson, Keshav Pingali: The Program Structure Tree: Computing Control Regions in Linear Time. PLDI 1994: 171-185

例子如下

![](img/39d3fc7c54a0f3c6413936e81c03de8f.png)

![](img/d3c811e3efcb3acd064346d721f086db.png)

![](img/32f5e86600891007779fe2e93aa30f0b.png)

![](img/0229fdbb1ed636bb83c8727d0729405f.png)

![](img/747cc8a86f05e7716733634007ff81c6.png)

![](img/27532e13aabced3fdc9a42de7e88b6e5.png)

输出的样例

![](img/2487b3bd77462d7e9f8d4a868d815cbf.png)

## LLVM Metadata

![](img/688a4c962983ae2c3090899b252db7f0.png)

```
clang -c -emit-llvm -g program.c -o program.bc
```

Example

![](img/506cf037fd1c89a1dffaca5bea2c5b5b.png)

![](img/54c0e3e82a67ca253d59afb417fc745e.png)

![](img/290ea5377276547e3355d016ff411c93.png)

![](img/59ac569e573b735000ad45a2057e1c54.png)

![](img/81625418fd73777972dec2b0ad564c7c.png)

![](img/9b0a318ccc30162128113d4050c955d6.png)

使用 `metadata` 的例子

获取 `source code` 里的代码行号。

![](img/7bc2e8b867b5226e6369b8b71fd56061.png)

# PGO - Profile Guided Optimization

别名很多

![](img/cf6a14cf6538ebdeb88a9a4a06b9b685.png)

![](img/64563f43f2e6e227d89746172c7e00e6.png)

![](img/2136b78d7eefc792f4e2ff4f178ad7f8.png)

![](img/fb01c6a2491e6b9b4487a83017fd83dc.png)

![](img/e407345b3e0bd5d103a097163361eee7.png)

# Clang Tools

## Clang AST

```
clang -c -Xclang -ast-dump simple.c
```

![](img/15af759835e3ef5b718ef9d0c7060ce2.png)

![](img/0ca2deec06518fa4fe17bb21de2c56d5.png)

`https://clang.llvm.org/docs/Tooling.html`

## AST Matcher

感觉比较重要，文档认真看一下 `https://clang.llvm.org/docs/LibASTMatchersReference.html`

![](img/206a0886e4bc16a859f92041c525101a.png)

![](img/a865f55983127ac428e7e8d43f451463.png)

![](img/e90e0b973ed91d4da3423c3f89b99826.png)
