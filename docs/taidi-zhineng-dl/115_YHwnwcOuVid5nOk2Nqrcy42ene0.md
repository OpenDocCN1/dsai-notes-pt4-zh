
# CS450: Structure of Higher Level Languages

> 来源：[https://learning.feishu.cn/docx/ES6xdvMaNoq2ahxrIsVczYQ1nWe](https://learning.feishu.cn/docx/ES6xdvMaNoq2ahxrIsVczYQ1nWe)

`https://cogumbreiro.github.io/teaching/cs450/f22/`

Tiago Cogumbreiro

UMass Boston 马萨诸塞大学波士顿分校。Software Verification Laboratory

![](img/68bf423e788a42a786416ce3e219839c.png)

一共 25 个视频，每个 1h 15min。

课程使用 `Racket`：`https://racket-lang.org/`

一些心得

看到了 `06`，数据结构也是 function，以及 function 的柯里化，都有了不错的新的更深刻的理解。

可能也跟，上个月刚刚刷完 compilers 有关，之前看到 AST 的表示有点懵，现在很适应。

几个印象比较深刻的新理解：

1. `Function` 可以表示一切，包括 data structure。

1. 所有语言都等价于 lambda calculus without numbers。差异只是 implementation 的性能不一样。

1. Everything is a list. Racket 源码就是 a bunch of nested lists。Lists are linked-lists of pairs。

1. 一切都是 list，跟 function 可以表示一切，是否冲突。应该不冲突。

1. 针对 dl，是不是有必要设计一个 tensor 作为基本 number 类型的抽象语言，然后分析如何做出一个高效的工程化 implementation。

![](img/d58211207ae8446e56bd18c471ee20c8.png)

## Lec 1-3 Basic Functional Programming

lec1 从 24 min 开始看。前面都在讲 course info，比如，如何布置作业之类。

lec2 也先用了 20min 讲课程相关。演示了完成一个 assignment & 在内部 autograder 网站上 submit。

### 要点总结

1. 理论上，所有语言都等价于 lambda calculus without numbers。

1. 语言没有快与慢之分，但，语言的实现，有。

1. FP is a test bed for PL design. Closer to math formalism.

1. Everything is a list. Racket 源码就是 a bunch of nested lists.

1. Lists are linked-lists of pairs

### 语言都一样

要点：

1. 理论上，所有语言都等价于 lambda calculus without numbers。

1. 语言没有快与慢之分，但，语言的实现，有。

`https://en.wikipedia.org/wiki/Church%E2%80%93Turing_thesis` 图灵机理论？

C 为什么快？“无他，唯手熟尔”。paper `https://queue.acm.org/detail.cfm?id=3212479`

1. `compiler` 好，

1. `optimizater` 好，

1. 用户好 - 善于写 optimizer-ready 的 `c` code。

`https://wiki.python.org/moin/GlobalInterpreterLock`

1. `Cpython` 的 `GIL` 导致的。Java 版的 `Jython` 可能没有这个限制。

1. 多核并行可用，但，shared memory 非常慢，还是不解决 compute-bound 的问题。

结论：compute-bound 的并行计算，别用 `python`。

语言的差异

![](img/a6a1c2ade5176189b9d1578c8f9a59b1.png)

### 使用 FP

FP is a test bed for PL design.

Closer to math formalism.

![](img/97636bfe270dbc1380b40a4e0bfc7de2.png)

### Racket 入门 - numbers

![](img/3f0a9bf0e40dc85fb6a0a217e282da81.png)

![](img/3c40b379f4eaa0c3c55f54459c4d557c.png)

![](img/14b0106e301fc13cfcf3ceb07758d89a.png)

![](img/e0e3d866e7181eda909419a375b463f4.png)

![](img/b9e342206eef912e71fe4f18ab7a35bb.png)

### Boolean and logic

![](img/c96827424c4b6840e74bd14961ee52f3.png)

![](img/b5d72547a8cd1cc3bc96ed5e86a2130a.png)

### Branching

![](img/3a973c5a7d11bc02255e34b4bb9ac54a.png)

### Variable definition

![](img/4484a51ee9589c167d3e20ed7657cc1e.png)

![](img/16008cb518c6e8c4aef99b366bb9bf9b.png)

![](img/6e015e1701448b1f4b7e2d133f3db944.png)

![](img/531901560d82ba308e3eda04ee5e9041.png)

例子

![](img/82647ac8cd9ef7e8dca5ecec740c9a51.png)

![](img/7aaa41b430949292a76496730909b8dc.png)

### Data Structure

Pairs, lists, user structures, and quoting

![](img/74acb8461a3fbbbc30ff9dae54daf41f.png)

Pairs

![](img/009a1135b6be5d7b603a6aa579f978d6.png)

![](img/eb6338ed6280ac53d5c61c85b2011840.png)

![](img/4f089115101af1bae7087b478bdab8aa.png)

![](img/4eece09e9d697118cdb9ce97ff49dc61.png)

![](img/e910ecc98ca483b656c4c6b1852aa959.png)

![](img/662c39d2f488458afcade9e30938b7b2.png)

![](img/ac7fcc11dd634d355faec80f31027d7e.png)

![](img/37ae838aa0155eb68b89f0b6a5ca59c5.png)

### Quotation and serializers

Everything is a list.

Racket 源码就是 a bunch of nested lists。

```
(point? #f) ; length: 2
(point? 10) ; length: 2

(define x 10) ; length: 3
```

![](img/2c3ffe4a23b51be7377e563a5a3a6f2b.png)

![](img/7ef30237bfde05318af4d7df43e6bc11.png)

![](img/7e9f32162b3d0119501a6689d623dfb4.png)

## Lec 4-8 Advanced Functional Programming

从 41:41 开始看。前面都是关于 list 的练习，10min 看一下 slides 就够了。

### List 的练习题

![](img/1d1049bb021815c3b3c492a8ed512b57.png)

![](img/911d3a3fd96201ab3dfe7e92293f94c8.png)

使用 `define` 的写法

![](img/77bb917a7c049a87af44a951813fbbbc.png)

![](img/9abcb519d9dff1019dd7057c59116458.png)

![](img/0d8c90e3b2a9632b752879714cc85913.png)

使用 `define`，逻辑清晰了很多。

![](img/870efc60627b50b9bfd12c69bd5c36a4.png)

### Nested definition

其实就是闭包的概念。

`count down` 是很容易的，见 list 练习 2.

但 `count up` 就不一样了。

![](img/da9f2240d6eaa8884f6469f7463fa5a3.png)

![](img/e6b7c8e86125fffe8b24ebbda61e35ff.png)

![](img/74fa726c3e6680ad8485cc4b38d42e0f.png)

`Count` 的第二个参数 `to`，始终是 `x`，不需要额外传入。简化后的代码

![](img/35a18faddb1b32f94c0843019d2d3f37.png)

![](img/e2205984d5cf6c4dd2fc39730c71899f.png)

### Scope 作用域

1. Local var 不能 `redefine`

1. 但是，可以 `shadow` global var。

![](img/7a5fd4a90eccabc0a712205786cc5cba.png)

![](img/565386fb107cde8cdc3026d834e1e48d.png)

![](img/4244a966e3c897bc9eba02f96893bf20.png)

### measuring performance

List length 每增加 1，耗时翻倍。

因为，每一次执行，都做了 2 次 `max (rest xs)` 

方法 1，加个 `cache`

![](img/3d990e80be2b8fc3a925ec5c69ba9067.png)

### Tail-call 优化

尾递归

![](img/dcb62b4a8e2dce0e25e6c16018c80976.png)

![](img/096cb876108df13883c482b36823e18c.png)

性能继续翻倍，分析：

1. `Cache` 版本里，每次 `loop` 都会计算一次所有元素都 `max`。总的运算次数，是一个累加的关系。

1. `Tail-call` 版本，跟普通 `loop` 一样，全局只算了一次 `max`。每次 `loop` 只比较 2 个元素，不用计算一次 `max`。

![](img/000370831aefb4bb31bd73f6a61f8d1a.png)

### Modules

![](img/b4d0d9c9b25c7b01fb6e175c10ca15b7.png)

![](img/dc38404e3ee14e177a8b9f1cf64a32c6.png)

### Tail-call 优化

非 `fp` 的语言里，递归的 `call-stack` 可能过长，从而报错。

一般的基本概念，没啥新知识。

![](img/43d824823942278569d65020029f0c2c.png)

`Tail-call` 的严谨的定义

![](img/d5a4420fc233f39db827705e24fa468c.png)

`Tail call` 不需要 `push` 新 `activation frame`，可以 `reuse current frame`。

![](img/325b9f336c413f6ae2c04c3bd456a192.png)

### struct - Revisit user-defined structure

`struct` 关键词的收益：

1. 改进封装，constructor 和 accessor 配对。

1. ensure `type-safety`

不配对的问题，例子如下

![](img/fe37ff3d75b253d56e817f49f52b6437.png)

`#:transparent` 的作用，声明新变量时会 `print`。

![](img/65f08e135acba3fea54e74eb0e1a4a55.png)

一些例子，注意，variable 全是 immutable 的。所以，叫 `binding` 更合适。

![](img/5263bf1c951b4d384623b25296e70755.png)

### Ractet AST

![](img/d140766fa11d21750c3bb66be7862458.png)

![](img/bc95e7b5494f87aa040fd31f9b281483.png)

![](img/1017cd81890092f8c4150b4400eb6cb5.png)

![](img/e879b7f1fe97547a0849cbb0c4f592d5.png)

![](img/50b2a091b0220e091f48b5c283722050.png)

### What is FP

![](img/f469b55b2096c954ffc9d4f0cd9336bc.png)

![](img/b7f25fcf43c806853b9a3319eb04f8cc.png)

1. Functions as parameters

1. Functions as data structures

monotonic（单调的）

![](img/105e35de9bd0b98c1fe0187b22045f51.png)

![](img/f9dfc9c7ef3925a77dffe60373951cde.png)

![](img/112f5803c870916d78d014da6633ac1a.png)

最后一个 `lambda () 10` 有点学问。下图中，注释掉的 2 个版本，是错误的。

第二个不会报错，但也不对。注意，`func-dec` 中，最后一项是 `term+`，至少一个 `term`，所以，这是一个 list，即使只有一个 `term` 时，也要用 list 表示。

![](img/6c35a7273b9c94c829f17495f3a95b3c.png)

![](img/7d36e55a8e33d85a8357dd16e7a367cb.png)

![](img/85d0aed698df1cf5770735cc2b22dbff.png)

![](img/685cd4c5f484558ee90a0a732e2e8919.png)

![](img/33d254646b2172bfd21a9028be00c749.png)

![](img/039383772bc26fa38837ba5e1b147196.png)

此处，`g` 就像一个 `tuple` 的 data structure, holding 2 things。

`function` 可以表示一切，data structure，constant 等。

-- 这个确实有助于理解，`function` can represent everything

=> 从 `lambda calculus without numbers` 的角度，一切语言都一样。

![](img/d532dbbc91d53d4bfe1fedb94a29bf80.png)

Store `function` in data structure

```
#lang racket

(struct frozen (func arg1) #:transparent)

(define f (frozen + 10))
(frozen-func f)
(frozen-arg1 f)

(define (call f arg2)
    (define func (frozen-func f))
    (define arg1 (frozen-arg1 f))
    (func arg1 arg2))

(call f 30)
```

注意 `define f (frozen + 10)`

定义了一个 `procedure`(`struct`) `f`，用的 `struct` 是 `frozen`，初始化参数是 `+` 和 `10`

可以看到， 用 `struct` 保存了 `function` 的一部分片段，且可以任意去扩展这个片段。

经典的例子，`function` 的 `pipeline`

![](img/f86a87fe5c7f2b360a336730bc6d84d3.png)

### Currying 柯里化

有用的抽象，但实现和使用都比较简单。

![](img/61417cc0b3ebaa9af35c45b3b28fabb0.png)

### Functional patterns

1. Does it exists。对 list 做递归处理的一般 pattern。（类似于 `any`，没有构造一个新的 list）



## List operations: map, filter, reduction

1. `map`。Updating elements。（building a new list with updated elements，`length` 不变）

1. Scanning / `filter`。Remove 0 from a list。（building a new list。`length` 可能改变，但 element 一般不变。）

1. `reduction`。`concatenate` 2 lists。

Does it exists

![](img/0a1b3b77214045d56e64b03f7bc81cc8.png)
![](img/2b2b9ae68aad207b9ea10a392531a468.png)
![](img/bda3cf222259a1bdf7d53d72ae5811a9.png)
![](img/f8b2ddd1d38d91680b0277dd73b5c853.png)

Updating elements 。`map`

例子，把 `list` 中的每一个 element 转成 `float`。

![](img/96dcc7375ecc4589dbf2bae1a62271fe.png)

Tail-recusive optimization pattern

把 `map` 改成 `tail-recursion`

![](img/54edf1348a6bbc0f236c1480d7c90d3c.png)
![](img/e0e680039ecc796151b0844d49b93301.png)
![](img/94db813298127aef31ddb70c0e849873.png)
![](img/6d169e1804b036e618e26e1848e68920.png)

Scanning / `filter`

Remove 0 from a list

![](img/e28937135910b7285b0c53d51d73825e.png)
![](img/9c04112c6d82552bfc739384af76d01a.png)
![](img/65df7601afc2c95ed383c926d0cea25e.png)

`reduction`。`concatenate` 2 lists。

![](img/23daeced1f13e49b318144504e6c1205.png)

### `Fold right` - 一般化的 `reduction` pattern

一般化后的通用 `list` 递归 patten

![](img/86afb235842f6cbaca2ce4afc2fbdca6.png)
![](img/c6a0b8eabffe4dcf0ab7928a15b1a1e1.png)
![](img/1d4db78e30abd0660a55a2f1d26dbcfc.png)
![](img/8483c33cb17c5ca2a2ee9994cfb0c1df.png)
![](img/cd752968234c92347eca465aefc985a2.png)
![](img/4343bb5f067a29e3a99a6030ab81ce89.png)

### `foldl`

跟 parser 的 top-down 算法类似，right recursion，left recursion 2 种算法，各有适用场景。

Example 1: Reverse a list

![](img/3e86f52fa4303f49b8fa56b06e83f613.png)

Example 2: join strings

![](img/6386f36337aaef2ca9a7d5e3a58184de.png)
![](img/e8e9ca18d9e091453257927222a377cb.png)
![](img/cec9659879bea5f942a0c3e1fd8e138d.png)
![](img/3bfccfa29b26672790cdd30cb5989ae1.png)

### `optimize fold-right`

![](img/67c6ec4c6e05e5173cea187c26b4a32c.png)
![](img/b97eaaa22ff83d946dd11c2880a1733b.png)
![](img/d4e798e9e7899f12bd3d0e270a710a20.png)

结论，不快。慢了 0.7

![](img/55c641a7a8073823600f2fbdbfa75b76.png)
![](img/6d8d8ec5a396c7aee2245aa8ca244dc5.png)

# Lec 9-10 Lazy evaluation & streams

Lazy eval 不仅仅是性能优化层面的需求，还可以实现 stream / 无限递归 类问题的数学化表达。

![](img/706785ae031f70a6f9768ef90483f20f.png)

## `delay` `eval` & `thunk`

![](img/df1756fbd588f8e27f897cd525c82657.png)
![](img/e4cec82fb84b146514b8b5c36c49879c.png)

如下图，把 `if` 换成 `foo`，问题就很清晰了。会报错 divide by 0.

If (`foo`) 就是一个普通的 function，`then-branch` 是 function 的一个入参数，执行 `func` 之前，需要先 `eval` 所有的入参。

![](img/3802cb3300e89255bd4b3ecaa8477fa3.png)

解法，用 `lambda` 来 `delay` `eval` 的时间点。

匿名函数的意义。

![](img/bf953ef5a125d378338bbd3c13b44094.png)

`Thunk` 形实转换程序

![](img/fee43352a27a88b38f95a5e7f46515d5.png)
![](img/a9509fa19b58f6bd8ee7e52e2e79f803.png)

## Promises

![](img/ca7612bf969f5f15ffd82ab26f99a2c1.png)

也就是 future 和 promise

![](img/8d8aca72960f34ff250a05c240290646.png)

2 种方案的例子，右侧的，只会运行一次 `thunk` 的东西。

![](img/9ef221ee49d7ae5d5645c54bccd78ba4.png)

运行结果

![](img/a386cbf86965fb57fcdb6caaa70e1abc.png)

练习题：47 行的行为和输出

![](img/a3b2dd3c8b3bf76d034b8af6a66f9e40.png)
![](img/f196b4fb1aaff4054dcb121ca023bef2.png)
![](img/9d8a95f34fe37e9c1648a505c18a94c3.png)

## Streams

A `stream` is an **infinite** sequence of values.

本质是，用有限的计算机 memory，如何存储无限的数据？

计算机科学，就是 时间 time 与空间 space 的 tradeoff。

要节约 space，必然需要牺牲时间。

Stream 抽象，就是用代码计算 `state` 信息，使得有限的空间可以存储无限的数据。

1.  How to generate data. How to combine streams of data.

1.  How to access data.

![](img/7b877d92b768314f760a51ee33c11dd4.png)

Example code

```
#lang racket

(define (power-of-two)
    (define (power-of-two-iter x)
        (cons x (thunk (power-of-two-iter (* x 2)))))
    (power-of-two-iter 1))

(power-of-two)

(define (stream-get s) (car s))
(define (stream-next s) ((cdr s)))

(stream-get (power-of-two))  ; 1

(define 2.. (stream-next (power-of-two)))
(stream-get 2..)  ; 2

(stream-get (stream-next (power-of-two)))  ; 2
```

Implement functions on streams

特殊之处，没有 base case，因为，总是在 recursion。

![](img/4746581c8ecdcb9b98b8e739221c917d.png)
![](img/67b458a3b886e216ed34b07e691ea7d7.png)

针对 有限 `list` 的 `map`，`filter`，`reduction` 等方法的 `stream` 版本的实现。

体力活，没啥坑。

`Foldl` 和 `foldr` 的区别是

1.  `Foldl` 先算了最右边，然后不断的递归左边，

1.  （等所有的递归完，从左边开始构造输出）？

1.  （在 `accumulate` 的左边插入数据）？

1.  `Foldr` 先用最左边做 `accumulate`，然后不断的递归右边，构成一个尾递归。

如果是 `stream`，`foldl` 可能不断递归下去，永远不会开始输出结果。

## Evaluating expressions

递归的 `eval` 给定的 `expr`。

![](img/8c506d96645a9007dae81182b863025a.png)
![](img/03c01c5633d69ad2a5f5c972411aadff.png)

Evaluate `function-call` 的例子

![](img/774d955dc6805da6dbdc4538500f169b.png)
![](img/794b0e65d9fe3206d4c9af9ba397876a.png)
![](img/04f3e23bdd92a586c1ff1734480c7248.png)

实现 `eval-exp`

先定 `spec`，然后写实现

![](img/631f9508a2736c4ad304d6611a45d9ad.png)

`Eval-exp` 的实现如下。

备注：

1.  假定 `func` 只有 2 个 `args`

1.  `r:apply?` `exp` 的分支，后面执行的是一个 function call。

![](img/2595c5e923a8e16c030f2563eb8e5de3.png)
![](img/d414286ae7a6090944c74048e9c37d28.png)

Handling functions with an arbitrary number of parameters

![](img/c152398b474cb6f14d527eab01db824e.png)
![](img/fe52ab05f86be4735c98e69a0794264d.png)
![](img/4373bd47496ea6d8d765840b2f2f90a8.png)

# Lec 11-13 Lambda calculus

## Lambda 函数的形式化定义

![](img/13ce326a5400f9680c3c47623ff31201.png)
![](img/3aa3e9f7a97df9bbf048825210a25b3b.png)

learn how to implement an interpreter

实际上，大部分 interpreter 都会先 compile 一下再运行。也有的是 JIT 方法动态 compiler 一个代码片段。

![](img/a56740f4da9879e85d9f72b14624b0fc.png)

syntax

注意，

1.  所有的 function 都只有 1 个参数，使得 syntax 简化了很多。后面会讲，如果把 function 的 0 或多个参数处理成一个参数。

![](img/346d93649e79ed071e0b3cc47546b4a4.png)
![](img/f43ff97f968c4ac9a9399bc93c3dc99f.png)

semantic

如果学过 compiler 里的 semantic 逻辑表达，下面的式子很好理解。

1.  `e_f` 是 function

1.  `e_a` 是 function 的 `argument`

![](img/248534adf3692c9e6de9fe9fa6d0b3ca.png)

换个更熟悉的 `notation`，即，`code notation`

![](img/c8090b1431ae4037a7357563e8fa549a.png)

总结，2 种 `notation`

![](img/6a61d5dbaa4e6a9384ed4e36073bf11d.png)
![](img/dca8bce540cbc4d892495730501b39e4.png)
![](img/18227173ef43d02f74cbb354cb324bf9.png)

备注：

1.  `n` 是 number

1.  `x` 和 `y` 是 variable，且不一样

1.  第 4 个式子，变量作用域问题，`lambda` 函数 body 内的 `x` shadow 了 `x=v`，所以，不做任何替换。

1.  一共 6 个 branch，其中最后 2 个是 recursive substitution 的 branch。

几个例子

![](img/85f0fcc5d7167dba98ae5f6602486b9c.png)

更复杂一点的例子

![](img/e8302184894cfbfcceb87cf7bed449f9.png)
![](img/cadb907aa49902b01519ff7acaae99e8.png)
![](img/15a922d65c3420d05de0b97503bd2c90.png)

后续的推导过程

![](img/b2bc1361325162bc81b85505659832bd.png)
![](img/c16ff0c63a29ab44d19e22bcc459b9ab.png)

## scoping and closures

Lexical scoping is important for using functions-as-values

![](img/f481a65fdd1fe8660495c861fc501c95.png)
![](img/6e9a377bdb51376f321ee3e316ea4d01.png)
![](img/415ecc4c8e95ee9406b46e4f53f9735e.png)
![](img/8978229c4720221e68681c7ce2f84ae2.png)
![](img/2d0719b0f3e81a3f47fc4177143a404f.png)
![](img/dc0e33e6d511e41345a781707ae1c06b.png)

2 个问题

1.  How to compute the variables in a closure

1.  When to set the values of each variable in a closure

`Free variable` & `captured variable` 的定义

与 `free variable` 相对的，应该是 `bounded variable`。也就是被绑定了的。

![](img/5b8dccb774f87c9dd925c9e825f3e8d6.png)
![](img/ebad6b6d390994166eb0be570c7b8ca8.png)

`PL` 里面的上下文管理。

相比于软件工程里的做法，此处明确定义了 `create` 和 `update` 场景。即，形式上枚举了所有可能性。

![](img/def2b77e5f4b3b8b27f9637051cd2dec.png)

## 时间空间复杂度分析

因为 `substitution`，`Runtime` 耗时与 `expression` size 是 `linear` 的，非常慢。

![](img/fab6f3831456b6aca6d6d76edce679c7.png)

优化思路，本质是 `space` 换 `time`

![](img/ad0139dfdf72a5839ad152f5194e8045.png)
![](img/156f5ab95c575d984c9742f60e6af388.png)

`Recall` 不带 `env` 的表达式

![](img/16a9e4eb0372ff453adc0e255047df87.png)

带 `env` 的，主要改了 2 个东西

1.  `Syntax`，新增了紫色的规则

1.  `Semantic` 多了 2 个规则

![](img/a173ba32f15d23348d5b1aa79f8a77b8.png)

`V2` 就是 `function closure`，也是 `function value`

语法上，就完全不同了。写的是 `lambda`。运行时，内部是 `closure`。

在 `closure` 里面，为了 `state` `(x: 1)` 改变时也能 `eval` 出正确结果，`lambda` 部分一般不做提前的 `substitution/change`。在 `evaluate` 时（`runtime`）直接 `lookup` `environment` 拿结果。

![](img/0120d57676ca637f84429861cbf5cdff.png)
![](img/48b38028ba07bf6cb6db1c0296fe6695.png)

## Chuch's encoding

![](img/40ef0d7fcf566dec96fb597dddf8d357.png)

`Lambda` 只有 functions，能表达一切。



Church encoding 回答了，如何用 lambda expression 表达 numbers。从 Boolean 开始。

![](img/77882414a8a0e164b3a0446bf559ef86.png)

## Implementing the new AST

![](img/d2a0c2c23b73786e9479460891fd1565.png)

![](img/3aba0525e9b8cd3fdc291ea4c5d42230.png)

### Hash tables: represent environments in Racket

![](img/e5bd336b55d4ea1a45be9642ee0a80d6.png)

![](img/9729b0df56664a8a80017ba263eb1869.png)

![](img/8d4b6636aebfd11951ae91e178470d54.png)

![](img/243d971d2cbf5ee7bddd9a756953daa6.png)

## Implementing Inductive definition

![](img/a9f444b42946cdf4fad7501c0afbc44a.png)

![](img/0bac0e05271f6b261bae420ac082dfd3.png)

只是 notation 不一样，实现的 code 完全一样。

![](img/b2a7ba413e6c1671d1f14d5276d7a700.png)

![](img/51f949f986a5838491bc7d54efa83efe.png)

快排 quick sort 的算法描述。

说明：第二个规则，分子是在定义 2 个新 list，不是删选条件。

![](img/68845d68111fbfb66c0d85b571a463c4.png)

![](img/6fadf65c886da2ec3076845153fa085d.png)

## Support for definitions

1.  介绍一个 `define` 的 naive 实现，然后分析为什么不够用，以及在 racket 里实现起来很困难。

![](img/546eee36daad83516d2f71e876abd263.png)

2.  新定义了一个名词，`term`。对应第一条 规则，`t`。可以是 表达式 `e`，一组 `term` （`t;t)`，或 `define`。

3.  `Evaluate` `definition` 会返回 `void`。所以，`v` 的定义里，增加了一个分支。

![](img/672eb15ca743a19ca97a6abf098680a7.png)

新增了 `void` 实现

![](img/fa7c141f429d4efe8f1f56a02c7e37f6.png)

与之前一样

![](img/0da8aa9329473d93daa8de7a7180967b.png)

### 新增的概念

![](img/ba97fe5cb6e384a992aac14278a93bb1.png)

### Semantics

### 不正确的实现

几个备注：

1.  符合下标是 `environment`，后面是 `return`

2.  `E-seq` 规则里，一直在 copy `environment`。先生成了新的 `E_2`, `t_1` 为了兼容 `define`，可能就是 `define`，或者一个 `terms`，其中有 `define`。然后是 `E_3`，此处 `t_2` 一般是 expression

3.  最后一个规则，最终返回的 `environment` 是 `E_2`

![](img/c042ef9fbdb1b7bb3f81e12b64be2c42.png)

### 不正确的实现 - 续

![](img/db8ae504e0ddf704e65b51c781310e65.png)

作为对比，复习

![](img/3cb9e95d2907333ff44bec73a5ecb4ef.png)

为什么 不正确？`incorrect`

![](img/72a8281d4b108df5c547079bbaf1f3ee.png)

![](img/79ca659245fca3c9f9b7609f798884da.png)

![](img/7cbf52a75b3d4ac4ee1dc4db1a189b40.png)

作为对比，`example 1` 的 `step3`

![](img/9184d407efbf21cfd223ca8d24f148b1.png)

`step2`，`recursive` 的 `eval` `b` 时，可以 `lookup` `environment`，此时，`a` 刚好在 `environment` 里，所以，`b` 的 `closure` 包含了 `a` 的 `define`。

![](img/ed470ac6f6c30dfcb714f046ea37383b.png)

# Lec 14-16 Mutable lambda calculus

Lec 14 的课程 video link 错了。

根据 https://cogumbreiro.github.io/teaching/cs450/s21/ 和 YouTube playlist https://www.youtube.com/@tiagocogumbreiro/playlists?view=1

找到 CS450 - S21 - Lecture 21

![](img/d83c017114ad173fc6806613f1d030c9.png)

有 `side effect` 的定义。`mutable environment`

![](img/808a79d3b0cafd9555c59690ab04d68b.png)

因为 `side effect`，所以，`sub expression` `eval` 的顺序就很重要了。-- 所以，不喜欢 `side effect`。

![](img/30641c554049fc8a6f6cca99e2d03381.png)

![](img/ac5044cd0f34e1d2b843266d137b1102.png)

How to implement `mutation` without `mutable` constructs?

有点类似 JavaScript 里的 `prototype` 机制。

![](img/304178228bfb872e57e029acf718c3c6.png)

![](img/dca4858d89077f8283902f52c99b5cef.png)

数据结构：`heap`

![](img/684384bd66d2125a99892dce3f845baa.png)

`Heap` 的用法分析

![](img/c548ca0ea8955f27622d90baa455b443.png)

给内存、数据结构做 versioning

![](img/a6b90b10f11ec29124e5002485ab7727.png)

`Heap` 的简单实现

![](img/f5b9255d47ce579a812dccd436e0903e.png)

![](img/1c72fc80eded259b3034ea3a1cd6a455.png)

## Contract

没太看懂

![](img/9b29c4d324c0effc539810d967befd9b.png)

![](img/67426be85007d2a075bb4f02cc56e5d3.png)

![](img/a9ea14117d6d514f7972058af3efd210.png)

## Visualizing the environment

`Environment` 是 sequence of `frames`，`variable` `lookup` 是按照顺序进行的。

有点类似 JavaScript 里的 `prototype` 机制。

![](img/ea70d0a7c942128c87e2349389b13e26.png)

## Implementing mutable environments

`implement` `frames`

`TODO`

![](img/7c68b7cf64839c8e86bd849a7167b5eb.png)

![](img/e0fb0ba65618223e9d91f75e9073ae58.png)

![](img/085f162db025afd40258d9630b331f7b.png)

![](img/129193d0bbd57275a621bb2f1c31a6e8.png)

![](img/77b9df32b3baad903361a60b8dcb3109.png)

### Lec 16: Evaluating expressions, variable arguments

Slides 很敷衍。好像没有关键内容，跳过。

# Lec 17-20 Monads and side effects

## memory management Introduction

从数学理论上，`function` 的信息存在 `frames` 里，回收的是 `frames` 对应的存储空间。

并不限制 `frames` 存在内存、寄存器甚至硬盘上。

与硬件实现，还是无关的。

![](img/3c4fa979b6c6b26d7082f8357ce31b43.png)

![](img/db3b9ad391a1038f8b6291a575a868b2.png)

![](img/2d0c688ac7edfef8c6c90e3324a9a9a2.png)

![](img/e4ac8116dcc2c8b32da9cbf24b0e773f.png)

![](img/18e7299dbc69a0bdd53dc04a1a615ee7.png)

## GC - garbage collection

GDB/c/c++ 不能像 python 一样用 `dir` 看变量的所有 attribute/method 也是一样的道理。

![](img/cb18e0d2c2ddd39b29fe7086a59d4023.png)

![](img/ffa2015f184cccc0ceb4e78d55ab0393.png)

Java 和 Javascript 都是 reachability 的方案，不是 reference counting。特别是 Java 的 `jvm` 机制。

问题：如果有 1T 的内存，这可达性分析，一圈扫描下来，快不到哪里去吧？

回答：不会。`heap` 分成很多个块，新生代、旧生代。做分代的回收，不发生 `full gc` 的时候，不会有暂停的。

![](img/ba75a53471db9bd058d309b4759cacba.png)

![](img/8e3b7bfef3305da606f1ae00017042c0.png)

## Reference counting GC

![](img/a517f8cfc8ccb5b448ddb781eba3d584.png)

![](img/aa92cb4e0bd1acb5a580637dc414d16e.png)

![](img/12db85b8249bc967d7a161f6aa3dc4fa.png)

![](img/df969cacc87f37dfcc1c7a438958f5da.png)

![](img/7233b43cc652b54ad5212678637130cc.png)

## Mark-and-sweep GC

![](img/774b2ac45a086015ea5f1c5552827a7d.png)

![](img/61433594eba2c470a1afa52cbf0698a9.png)

![](img/0e4e01a53f8e2156b622489c2444026b.png)

![](img/f67929899924b6a0ed6a334f148f6554.png)

![](img/b1c1f1508726c8f1e3d93592e37380ae.png)

## GC and our implementation of `Heap`

![](img/c297fb6ff7252f7700f535f7347cbf15.png)

![](img/0d7e14805aae2110d9e55ed6ce6b75e1.png)

![](img/69b8df7dfb40d454361c2ea3d6e1bf8c.png)

## Abstracting the assignment

Introduce a functional pattern monads

Introduce state monads

Introduce stack machines

## Functional pattern: Mutable state

![](img/40da46f1ba3e2438da99c53c50dcc992.png)

![](img/73a278c1ebad43545675e8cf0182649a.png)

![](img/efbf6342f0a58f8214a42635d7c55402.png)

![](img/fa060c454aab8b6cc4188862786af6c7.png)

![](img/43ed85956b0be5d57c7def38529237da.png)

## Arithmetic on the `heap`
