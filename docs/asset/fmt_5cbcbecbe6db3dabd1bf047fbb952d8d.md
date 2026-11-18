
# CS143 编译原理笔记 3 - Semantic

> 来源：[https://learning.feishu.cn/docx/NiwSdbnEooCi8VxSMAicsI9wnVb](https://learning.feishu.cn/docx/NiwSdbnEooCi8VxSMAicsI9wnVb)

## 总结

本文是 Stanford CS143 Compilers 课程 Week 5 & 6 的笔记，讲 Semantic Analysis 阶段。

| weeks | topic | 视频时长 | 用时/二刷 |
| --- | --- | --- | --- |
| week 5 | typing system | 1.5h | 2h |
| Week 6-1 | self-type | 0.5h | 1h |

注：

1. `Week 6` 后半段是讲 `runtime`，与后面的 `codegen` 关联度更高。
1. 为了整理这份笔记，又花了 1.5 天的时间。（笔记整整齐齐的代价）

这一门课的 `Semantic` 部分，主要是基于 `type system` 的 `error` check。

1. 如果不打算动手写 `semantic`，花 30min 看到 `type-checking` 处，剩下的可以跳过。
1. 基本全是讲 `type system` 的形式化定义。后面的 `codegen` 和 `optimization` 都用不上。
1. 比 `parsing` 的实现算法更考验记忆力 & 抗瞌睡能力。
1. 深入理解 `typing system`，可能有助于理解 `lambda` 计算和闭包等概念。

顺着这个课程深入理解 `type system` 和 `type theory` 是不错的切入点。

经典教材：

![](img/27c06d1b953c8d36ec57e7e0e95718aa.png)

[`Types-and-Programming-Languages-2002.pdf`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BQR0bXYfYoP5GBxyZ2Mce25unte)

![](img/28bfd14c58d76659988851bbd742bccf.png)

提纲：

1. `Semantic analysis Overview`
1. 核心是基于 `type system` 兜底所有 `error` 检查。
1. 实现上，遍历 `AST`，`symbol table` 保存上下文。
1. 保存 `context` 的难点：
1. 存在 `free variable`，多 `branch` 结构等。
1. `function` 和 `class` 不遵循 `most-closely nested rule`。
1. `context` 的结构，包括 3 个 `mapping` 函数：`O` / `M` / `C`。
1. `O`: 解决大部分问题，比如：`free variable`, `subtype` 等。
1. `M`: 针对 `function` 的设计。使得，`function` 和 `class` 可以同名。
1. `C`: 即，当前的 `class`。
1. `Subtype`。解决 `class` 多继承，多 `branch` 问题。用集合的子集、并集等概念建模。
1. `self_type`。解决 `class method` 在继承时的 `return type` 问题。
1. 仍处于 `research` 状态，不成熟。主流语言基本都没支持。
1. 加入 `self_type` 规则后，需要把所有的 `rules` 重新 `update` & 推导一遍。

其他一些 `highlight`

1. `Type` 就是 `value` + set of `operations`。
1. `compiler` 需要实现 `type checking` 和 `type inference`。也可以根据 `type` 计算准确的内存分配需求。
1. 有些问题，`single pass` 解不了，需要 `multiple pass`。比如，`class name` 可以先使用后定义。
1. `Type checking`，主要是大量的 `logic` 表达，描述各种情况的 `if xxx`，`then yyy`。
1. `Error recovery`，就是如何 `display` & 不 `crash`。不要与 `correction` 混淆。

## Semantic Overview

### 功能

`Semantic analysis` 是 `Compiler frontend` 的最后一步。

从 `detect error` 角度，3 个阶段的职责如下

1. `Lexical analysis`：`illegal tokens`
1. `parsing`：`ill-formed parse tree`
1. `Semantic analysis`：`all remaining errors`

`Parsing` 不能处理所有 `error`，原因：

1. 捕获不到。
1. 有些 `language construct` 不是 `context free` 的。

一些 `error` 的例子：

![](img/f267713ab3a762d06c3648aa180adadb.png)

### Scope - 变量作用域

`Scope`，就是变量作用域。验证变量的声明和使用是 `match` 的。

复杂度，主要来自于 `dynamic scope`，以及 `nested scope`。

`Scope` 的分类：`Static scope`，`dynamic scope`

![](img/4c3ab134bd5b324259c04907e9d041ea.png)

（`dynamic scope`，有点像闭包？现在很多语言都有闭包了。）

![](img/98e03532b420807885ecd0b845d1630f.png)

来自 `function` 和 `class` 的复杂度，不遵循 `most-closely nested rule`。

![](img/880a93d20f4addc7960d5193fe557e60.png)

![](img/7b98080ca8faf1f2edde72f122dab7d.png)

### Symbol tables

大部分 `Semantic analysis`，可以用递归的 `AST` 下降来实现。

流程有点特殊：

1. `process part of the node`
1. `process children notes`
1. `process remaining part of the node`, `return`

![](img/a4fb80843a7d7dc45a121ff6f175fb49.png)

一个例子，`Parse` 如下

![](img/87ffb34d2cf8c915c80b6929f334ff2e.png)

执行过程如下

![](img/a3fcf8ff852118698e3da2efba2b1821.png)

`Symbol table` 的定义 & `operations`。是一个 `stack` 数据结构。

![](img/43f65f7b1f69a192e550dcd0ed16cba2.png)

为了解决 `function scope` 的支持，`refine` 后的版本

![](img/97c9433b4e16aaee430b7aa11608f8ec.png)

### Multiple passes

`Class names` 的检查，用了 `symbol table` 也没法在 1 个 `pass` 里解决。

思路：多整几个 `pass`。

这跟 `Latex` 编译时解决 `reference` 索引问题的原理是一样的。

![](img/74813634b64bb279c5328899dc8b1efb.png)

## Types 基本概念

`Type` 就是 `value` + set of `operations`。

`Class` 是 `type` 的一种现代化 `notion`。

`Type checking` 的目标，保证每一个 `operation` 只会用在正确的 `type` 上。

不同的编程语言，对 `types` 的定义有差异。

`Assembly language` 是没有 `type` 的。

按照 `type` 类型，分为 3 种语言：

![](img/402370be0c1aa400630e1436b91a952f.png)

优劣对比

![](img/fa9d8b1f60d3d3e7ee3325901ff053ff.png)

`static` 与 `dynamic type`，已经相互拥抱对方。现状是否合理，有待商榷。

![](img/8049caffdd1917a79d06b6889b98c526.png)

`Cool` 的 `type` 系统设计。其中，`SELF_TYPE` 是保留类型，后面（`Week 6`）单独讲。

![](img/d429ea8c0ed3da327ce97dea645c895f.png)

`Type checking` & `type inference`

（`type inference`，有点像 `ai compiler` 里面的 `shape inference`）

![](img/bf72ebb048415c4a91548e1f45f92943.png)

## Type checking

### 符号约定

`Type checking` 的形式化（`formalism`）是 `logical rules of inference`。

也就是 `if xxx`, `then xxx` 的形式(`reasoning`)。

`Lexer` 是 `regular expressions`。`parsing` 是 `context-free grammars`。

`Inference Rules` 的形式。

![](img/4ae4cfbbdcae430d3476e4743d5b5ab4.png)

符号约定

![](img/96663e8befebe02adbdb143bccc4d6d0.png)

使用符号约定的一个例子

![](img/200f67992d197fa16f1a5eddbce05731.png)

通用的 `inference rule` 形式：

![](img/51bd64fd5a56f06b4b0ad2ff21700044.png)

`Cool` 语言 `inference rule` 的例子。级联了。

![](img/1ecd74080fb6afc9d875097a0027b567.png)

`Sound rules`。一般翻译为 健全的规则。即，用可靠且有信息量的 `rules` 做 `type inference`。

![](img/f5496cc31fb2dae8716239966264f9b2.png)

`Type checking` 小结：

（没有太多的新信息，就是一些形式化术语的总结）

1. `Type checking` 在 `AST` 上进行。一个 `ast node` 一条 `type rule`。
1. 所以，所有 `type checking` 结构的形状，与 `ast` 相同。

![](img/f5e37f3c9a6b0928936e8c6b9183bd8f.png)

### Type Environments

就是一个 `mapping function`。可以实现从 `objectIdentifiers` 到 `types` 的转换。

使用场景是，给定某个 `mapping function`，一个 `expression` 的 `type` 就能精确推导了。

也可以粗暴的理解为，这就是一张表，可以查到 `free variable` 的 `type` 信息。

`Cool` 语言设计，`loop` 返回 `object`。

如果 `loop` 返回 `type T`，则，当 `e1` 为 `false`，没有 执行 `e2` 时，就无法返回有效的 `type T`。这种不一致，容易导致问题。

`Rust` 里的 `loop` 也有 `type`。

![](img/6580894655ca97def95f3bc67092b5dd.png)

`Variable reference` 的 `type`

难点

![](img/342277996851a50212abffd7404e2518.png)

`solution`，这种变量叫 `free variable`。

`Type environment` 包含了确定 `free variable typelub` 所需的信息。从 `objectIdentifiers` 到 `types` 的转换。

![](img/1d977ad7d345c3b6f2b026f811f6b2c9.png)

![](img/8bde28cd792655df590fb611cb8d2968.png)

2 个例子

![](img/249445b7b799864da24b9f9dcd1770aa.png)

又一个例子，稍微有点绕的 `notation`。

![](img/def38216028bbb5e278f9ddcecef24a7.png)

![](img/5ef0f6ef6a5ec420e54e5efa11ad2966.png)

小结

就是在 `AST node` 之间携带 `free variable type` 推导信息的一个 `function`。

![](img/882361dc76ed80fb64888bf29a8daa07.png)

### Subtyping

主要是解决 `class` 继承和 多 `branch` （`if...else`, `case` 等）的 `type checking` 问题。

引子：不带 `subtyping` 的例子。

![](img/f5ec721e5334f7d7805be042759d35e2.png)

`Subtyping` 的 `notation`

![](img/be7995974688b752144a1a78e6489541.png)

另一个 `notation`

![](img/c3b80e5c7e1f3c6a59ed3318206e576d.png)

![](img/7dffc161e1fc4c91e2d3c19466503a4a.png)

`if...else` 的 `type checking`

![](img/fcbe1ae3c744e8f28d6e2259e8f36686.png)

![](img/6eed7f3f867762cc3144b396a2af5f2d.png)

![](img/e4690a564f1676698ff279e6a316f881.png)

同样的，`case` 表达式，就是 `lub` 所有 `branches`。

![](img/9498b5b430b7aefeb0b8519a13bb9892.png)

### Typing Methods

![](img/cc011cc45e35c5981186cb7d6653b12d.png)

![](img/ca2b1adc6060ed260c54dec7b7c77474.png)

![](img/61d7359f655d2baee709a9544eeb2be2.png)

![](img/8d798bd8b96037c56eef06009a0762b6.png)

![](img/a244f5073b50200c64d3d884e6909eda.png)

![](img/3f5fe6e105155b93a1a4f2b7c3eb4956.png)

![](img/0338b611459cf244fbaf7feb72cbc973.png)

### Implement type checking

![](img/1f5a4cd411a5a93886c78b40057f65a7.png)

代码实现

![](img/12f4895fffeeb8acbce1dec7671f8f01.png)

一个复杂一点的代码例子

![](img/3c87d84cfbbdc51bad16cbba73d204f2.png)

## Static vs dynamic typing

![](img/752e587720e47dd43d077a1eb2b51c49.png)

![](img/282e5a6e1153abde78e8f1584525bd57.png)

![](img/4f6ef03a10d66b2f747ea7c5c1c4db43.png)

![](img/6f6dbfabdc87af26f509972bded38024.png)

`Cool` 的稍微复杂一点

![](img/4b7e2be92a677e8d0a9ca06ec28c1ca5.png)

## Self-type

### 介绍

用 `keyword SELF_TYPE` 表示 `return value` 的 `type`

![](img/193f78f2ad57b6f9395d5bf66364bbf8.png)

![](img/882698194992d226edae1073e8e1d3ed.png)

![](img/a4f7956fa84f02e551bf76dd02b61016.png)

### self-type operations

`Key idea`。

![](img/5069ba1b6b4e26e69793507511dd1e49.png)

![](img/f42c0a3e34eb19145ba2174398166bb4.png)

`Notation`

![](img/141af58bec69fbe2d39c65fcf75e36f5.png)

![](img/b8041ca89d0adaeec0770eeba456e8cd.png)

![](img/dc7aec5ea1f593e4f3b7eaecc3f9b379.png)

### Self-type usage

![](img/e8961f9a911efb47af1b6b4972cae374.png)

![](img/a2fd559fb6f9d85b85bfc2c4f17abc55.png)

![](img/19a9cf9f001d4c88a3255b7bf6f93e6e.png)

### Self-type checking

`Assign rule`

![](img/f5e1453016e14d0a647116c781b1d651.png)

![](img/d9b8f547c33d4122b379c184ba7c6fbc.png)

![](img/6b697c6d2a185cdbe1d3ec22aebdf831.png)
