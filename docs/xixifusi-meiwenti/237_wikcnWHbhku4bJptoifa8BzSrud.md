
# 计算理论

> 来源：[https://docs.feishu.cn/docx/Tj08dsiiWoqAlFxVOj2c3TFvnDc](https://docs.feishu.cn/docx/Tj08dsiiWoqAlFxVOj2c3TFvnDc)

> 计算理论是计算机的理论，是高于计算机的

> 科学

> 从不可计算去思考

## 0 About Class

### 0.1 grade

+ 10% Homeworks
+ 20% Quiz
+ 60% Final exam
+ 10% Writing projects
+ 写关于计算机理论的问题，思考

### 0.2 Basic goals

> 计算的极限在哪里

+ 计算模型解决计算问题
+ 是否可计算，是否可确定
+ 可计算的话，复杂度如何？难易程度如何？`P` or `NP`？
+ 数据
+ 形式化语言

![](img/649b32a4f63e73e5fce321190c1a2a68.png)

+ 形式化带来的简洁之美
+ 语言学与计算模型研究的殊途同归
+ Godel不完备定理的完美证明及其深远的影响

### 0.3 Course Overview

+ Sets, logic, infinities, notation, proof types
+ Formal languages I grammars | machines
+ Computability | undecidability
+ `NP` - completeness, complexity

> 自然语言比形式语言远远复杂

> `P`和`NP`区别在于是不是确定型模型

## 1 集合、关系和语言

![](img/af1d39f4f94523daa0f81d5e439554ef.png)

Language

![](img/563fe65b490974c01c5f33ae224f7f8d.png)

![](img/b1a0f3f8c96f94a788197ab8cafe5409.png)

任一字母表上的字符串集合称作语言。

+ 有穷
+ 无穷

语言运算

> 集合并交差

![](img/d4abcb8f11add4b8984fc36638434de7.png)

+ 连接：`concatenation`
+ 幂：`exponentiation`
+ 逆：`reversal`
+ Kleene星号：表示连接`L`中0个或多个字符串得到的所有字符串的集合
+ `+`

![](img/d3dbb477a0e4806af6f076a83f788f63.png)

`L+`是包括`L`且在连接运算下封闭的最小语言

> `Language` 和 判定问题一一对应

![](img/0705bd8dc07b64c12a3ba017809d351d.png)

### 1.1 语言的有穷表达

正则表达式

![](img/6853e34e14a2821fd45542d6042b1803.png)

接下来，我们探索多种计算模型，由简到繁，看看他们各自的极限在哪里

## 2 有穷自动机 finite automata

### 2.1 State diagram

![](img/1e541386d7be15d63354bbb5e478bbf2.png)

![](img/669a07e9ade058e379cb515df5fab269.png)

> 初始只有一个，末态有多个

### 2.2 Deterministic finite automata

> `Deterministic finite automata` 确定型有穷自动机

![](img/b9b5d6cf5a567b38ef7f38e201176fbe.png)

转移函数用一张表来表示

#### 2.2.1 Configuration

![](img/75372b52f9eb8ce470e2add313ed8fff.png)

> `(q,W)`意味着从`q`出发，读一个`W`字符串

> `W = a W‘` 相当于`a` 链接 `W’`

> `aw’`是`a`和`w’`的`concatenation`。 `w=aw’`的意思是，`a`是`w`的第一个`symbol`，`w’`是剩下的`symbols`

走一步（yield a step）

走很多步（yield many steps）

![](img/c2d82bea26d17c49a0abc3b2224cc190.png)

accept的条件：

![](img/5c8e83604eabdb27c93ca0c1e361e68c.png)

`M`这台机子接受字符串`w`表示接受字符串走了几步之后可以停在`final state`

> `M`是`automata`自动机，`w`是字符串

![](img/88c9c9bd68445651415005f21c4d391a.png)

两个`accept`不太一样，第一个是`accept`某条路径，第二个是`accept`一个集合，所有可行路径的集合

可被`M`接受的`w`都在`L(M)`里，`L(M)`是字符串的集合

这个集合是唯一的

1. 只有初始，没有末态
1. 出身就是终点

![](img/f4ee419199ea0d731050833cf35c1e0a.png)

#### 2.2.2 Regular

定义：A language is regular if it is accepted by some `DFA`

是language 的一个特性

要证明是regular，就要构造一台`DFA`

> Deterministic finite autamata = `DFA`

Eg

![](img/b2e7f8c3d8e2eeb7c7ef4feffcc7a676.png)

Regular operations

![](img/99bfaf5d453c93d3c36d5043761f8e9a.png)

The class of regular languages is closed under `union`

![](img/c7879c5aeed78cc133cbadb35d72c833.png)

并行的去跑`M1`，`M2`，至少一个接受即可

字母表不一样怎么办？

![](img/e94ce4992d0629e74db384d497890675.png)

额外添加一个non final state

一旦读到`a`，`b`就跳到那里，之后无论怎么读，都一直在这里

——>可以任意扩充input集合，并且保证accept的language不变

所以可以不失一般性地表示`M1`和`M2` 的 alphabet一样，所以alphabet是一个很松的条件

![](img/6b20c7d4c6651bcd2bb3f1c3896e90c2.png)

> `or`有歧义，可以同时成立

用1和2的符号把3表示出来

`K3 = K1 ✖️ K2` 笛卡尔乘积

转移方程是分开来转移

![](img/af6d788da3302491db296f070c38da6e.png)

正则语言连接，交，补，`Star`同样成立

![](img/9dc3fc9b196ab1e1f75b9390c7f6ff92.png)

> 不知道切在哪里？`DFA`也没有提供直接工具告诉我们切哪里——>需要`NFA`

### 2.3 Non-deterministic finite automata

![](img/275f61ca66fce9837630bf07f97c1adf.png)

1. 对于下一个state有多个选择
1. 接受某个input，只要存在某个分支可以完成读取
1. E-transition：不读也可以走到下一步

和`DFA`不一样的是transition relation，可以是1个，也可以是0个也可以是n个；确定型是函数，只能是1个

![](img/f988b2cac91ce8a52f2db5f696510ed6.png)

现在状态 × 输入（也可以是`e`） × 下一个状态

![](img/c09a18f1b0ab5cc98dcde2ad813d8ca9.png)

#### 2.3.1 configuration

> 现实中很难找到这样的模型，主要是为了理论证明

> 但可以从几个角度来理解

1. 并行计算（但是不是真的并行）
1. 面对分叉口，很多选择，只要存在正确的选择，他总能猜对

![](img/101978a24b11bc8432b8ca8d6d896974.png)

`configuration`唯一不同就是一步可能有多种情况

`accept`是只要有一条能到就行

![](img/4f8fc91c96204770c98982464e843243.png)

> 只需要保证两点

1. 存在`b`是倒数第二个点
1. 不存在`a`是倒数第二个点

> 想象他有猜对的能力

定理：任何`NFA` 都可以 有一个对应 的`DFA`，with `L(M’)=L(M)`

思路：让一个串行的`DFA`去模拟树形并行的`NFA`

![](img/ed8ea774c554c22184367dc1f44342e3.png)

思路：把树的每一层的状态看作一个整体，当做`DFA`的一种状态

`DFA`的状态就是上图的右边

1. `DFA`的状态`K'`对应了`NFA`的`K`的一个子集，`K`的不同子集组成的集合= ，因为对于某个状态来说，可能在，也可能不在，所以总共的状态是
1. `S' = E(S)`，即 `S`+`S`不读symbol情况下可以走到的状态；`NFA`可能会不走，所以他对于`DFA`的初始状态就有多
1. 定义`E(q)`，`q`在不读symbol情况下可以走到的状态（自然包括`q`）
1.
1. `F'` 是一个`K`的子集，并且和`F`的交不是空集，就是必须保证`F'`有`F`的元素，除此之外还可以有其他元素
1. 当前状态是`Q`，读了一个`a`。之后`Q`中各自元素读取`a`之后的并 + 从读取`a`之后的这些状态出发，在不读取symbol情况下走到的状态（也就是`E`（这些状态））
1. 读取之后的并 + `E`

![](img/84f89f156188f682d39d535c1a3be28b.png)

![](img/9702b0b7442dfec2f0776bc9f8476a84.png)

![](img/8f70a3cff6e4f4d9bb0be669415bb5e6.png)

例子：

![](img/f8300b8bceae1682025ecb98b76177e0.png)

状态有{空集},{`q0`},{`q1`},{`q0,q1`}，先画四个圈

发现{`q0,q1`}`是E{`q0`}`，是初始

final只需要包括`q1`即可，也就是{`q0,q1`}和{`q1`}都是final state

transition，我们先看{`q0,q1`}，发现读个`b`，`q0`到`q0`，并且`q0`读`e`会变成`q1`，而`q1`读个`b`就寄了，所以读个`b`是到{`q0,q1`}，后面同理

都画完之后我们发现其实初始状态不会走到其他三个状态，这样子也是可以的；当然我们把这三个状态删掉也可以

严谨证明：

我们要证明`L(M) = L(M')`

claim：

![](img/eb0b27885786f9b27605d9981b9c51f2.png)

其实很直观claim是成立，当`NFA`有一条`pq`的路，在`DFA`里面从`E(p)`出发，就可以到`Q`，`Q`包含`q`；严格的证明，就是归纳

在claim成立的前提下

`M` accepts `w` 等同于

对于一个`NFA` （第二行）

等同于对于一个`DFA`（第三行），同时 不是空集， `Q`属于`F'`

等同于`M' accept w`

![](img/6b73fa9ef902a2bee938ebeb1dc7a906.png)

既然`NFA`和`DFA`相同，则regular也可以被`NFA`定义

> A language is regular iff it is accepted by some `NFA`

但是`NFA`更加灵活

我们回来证明

![](img/c6ffad5660cfb0716fc8081406528a74.png)

当有input string，先让`N1`去跑input，如果在某个时刻，`N1`到达final state，就意味着当前读的都是`A`的，我们把`N1`的final用读`e`切换到`N2`的initial，当`N1`到达final的时候，我们可以切换到`N2`，然后`N2`继续，如果`N2`读到final state，就说明当前的string是可以的

当到切分点的时候，`NFA`总能猜对的

![](img/23364619faef6311c59979d641e0cd69.png)

正则语言的星也是regular的

star的定义

![](img/c3af533f1dd3d573f4d2650ff1c4c052.png)

相当于是切无数刀，每到一个final state就切分一刀，重启，回到初始点，继续让`NFA`去跑字符串，直到input读完

但是没有考虑空串（`e`）的情况

> 思路一

> 把initial state变成final state

> ——>把一些不属于`A*`的也加进来了

> 思路二

> 新加一个initial state，也是final state，然后不需要走就可以到原来的initial state，这样就可以接受`e`了，再加上原来的final可以到原来的initial

![](img/dc9c07834f73ba45e1264cd8afea5fd6.png)

![](img/9f2dec7fce90c7f502c0341cd4027c17.png)

用`NFA`来证明两个regular的language的union也是regular

![](img/0e501f9e02183cdb3547bbebb66a602f.png)

让他自己去猜，如果是`A`的就走`A`，如果是`B`的就走`B`，至少有一条路可以跑通

新增一个initial state 跳转到`A`和`B`的initial state

### 2.4 正则表达式 REX

> 我们已经学了finite automata，但是他更多的只是language recognizer，被动地判定一个string是不是在language里；主动地去分析language有什么特性的叫language generator，这种最常见的就是grammar 。regular language对应的就有regular grammar，但是我们不常用，我们常用的是regular expression

> 将我们之前的公式用符号来代替

`R` =

语言：

> {}其实代表的是一个语言，`a`的语言和`b`的语言的union，再star一下，再和`a`的语言做个concatenation；其实就是以`a`结尾的语言

> 每个symbol的小language通过union，star，concatenation组成的

> `R` describes `L(R)`

数学定义：

![](img/057fa6c677de7a35ed90221fd7e9365c.png)

1. 空
1. 单元素
1. Union
1. Concatenation
1. Star

优先级

![](img/bc7f385204052da75264ea12c690e61f.png)

E.G.

1. `e`就是空集的星
1. 以`a`开始，以`b`结尾，中间有若干个`a`或者`b`
1. 两个`0`，他们的前中后都有若干个`0`

![](img/abfcf6b9b33593ce306e9411b35f5fe3.png)

接下来，我们要证明REX所能描述的language正好是regular language，也就是`NFA`可以和REX相互转换，给定一个正则表达式`R`，可以构造一台`NFA` `M`，使得`L(M) = L(R)`，反过来也一样。

![](img/aa03de4789b4348c3a53c5d2cce8f486.png)

![](img/e29fb91ed83ec04f79128d635ec48d4f.png)

1. REX —— > `NFA`

+ 先把base情况的构造`NFA`
+ 空
+ 单元素

![](img/0b70a64f2be5c97fdb3a2a90ed915ed9.png)

+ 利用基础的`NFA`，来构造在star，union，concatenation下的`NFA`（利用我们刚刚证明的star、union、concatenation的构造）

e.g

![](img/9f4fe859a508e6fc7537cd6213325238.png)

star需要设置重启和新的initial state

1. `NFA` —— > REX

> 工程角度

> 理论角度

工程角度：

1. Simplify `NFA`
1. 需要满足没有箭头指向initial state
1. 如果不满足解决方法：搞个新的initial state，指向老的initial state
1. 需要满足只有一个final state，并且没有箭头离开final state
1. 如果不满足解决方法：搞个新的final state，老的final state指向他
1. Eliminate states
1. 不断删除`NFA`的状态得到REX
1. 不断地用下面3种case简化

![](img/67895028396e99ae45063e0eef216203.png)

> 删除三个点中间的点

Eg

![](img/0efe38944526e2841a8c486c47ab961c.png)

理论角度：

> 本质是动态规划

假设已经做好第一步的简化

![](img/ef3572dbcc1d98f3059226d6742e4407.png)

+ 初始没有进入
+ 没有箭头离开final

对于动态规划，最重要的是定义子问题

定义语言：

![](img/1348afb1e56d42eb58d210bd5c6e2055.png)

`qi`读`w`这个string存在一条路从 到 ，没有中间状态的index超过`k`的

所有字符串的集合称为

当`k=0`时说明没有中间状态

对应的REX是

![](img/1490ef784d24896931f3edf47aee97ca.png)

> 例子

目标：`NFA` accept的那个语言对应的REX



从初始状态n-1到final state `n`的路，为什么是`n-2`呢，其实是随便的，不受限制，因为保证initial state和final state不会成为中间状态，不会有`n-1`和`n`，所以`n-2`即可

`REX`：

**Base case:**

![](img/dc7413450a6615c3336798f7ae66e249.png)

Recurrence:

![](img/63a1617c1666685ab983875100e2d0c9.png)

![](img/907d1d63e04bbbc68ef6959532ee018d.png)

+   一直都小于`k`

+   经过`k`

+   可以撞很多次

![](img/3f945ec3b54bf54185cfb45b218adb67.png)

时间复杂度是

有`n^3`个问题

其实工程的角度本质就是这样的，只是少了一些冗余的计算

> 相当于把`k`这个点去掉

怎么证明一个语言regular？

+   定义，`DFA`，`NFA` regular expression

+   Closure property：union，intersection，star，complement，concatenation

### 2.5 非正则语言

> 不是所有的语言都是regular的

> 那么怎么证明呢？

> Pumping theorem是一个监测必要条件，regular语言的必要条件

#### 2.5.1 Pumping theorem

![](img/52cb37fc2cad2ab6d643dc66ed00e12d.png)

1.  中间`y`可以重复任意多次

1.  `y`非空

1.  `xy`至多为`p`

![](img/12129fcb75c6e8f2a64c2634deaf4c0f.png)

1.  如果`L`是**有限**的，则`p`就是最长的`string`+1，条件就出问题

![](img/89d0f78006161da49bfa0038e0afb38d.png)

1.  如果`L`不是有限的

![](img/56c8e7bba9e69d94a7c34feec05fd610.png)

抽屉原理，一定存在`qi`=`qj`

![](img/f519fb2731792a397855061f05740ad0.png)

![](img/e43ed739c43f694a3d7b0a2a38b1962a.png)

**只是必要条件，而不是充分条件**

但是满足pumping theory而不是regular的情况很少

Eg

![](img/27c3780b511db2e6fc1dd244fff038e3.png)

23推出`y`包含若干个`0`，推出 是矛盾的

反证 是比较套路的，**关键是寻找`w`**，多训练

证明非正则 利用**小的定理**

![](img/09f5a897d6a8133e4b14aa21f8dad7b5.png)

### 3 上下文无关语言 context free language

> 比regular language更广泛的语言

![](img/f0c4d8b304a871d0864d2ac779ad7654.png)

两种描述`CFL`的方法

+   `Grammar`：context-free grammar

+   `automata`：pushdown automata

他们两个是互相转换的，但是两个都有各自更好的使用场景

同时也会有pumping theorem

#### 3.1 CFG

Context-free grammar

![](img/a516e26b7aa2520d36f01a5f0559428a.png)

左边可以转换成右边

左边：`non-terminal`

剩下的：`terminal`

开始是`S`，最终转换成只有`terminal`的

> 需要加一个`e`

严谨**定义**：

![](img/ac19f1d8942dbca888f0bb01d3181533.png)

##### 3.1.1 如何运算

+   一步

![](img/144dd6386ca190b6ba6cd5d4fa3fd58f.png)

+   若干步

![](img/172eb66c7220d3941dfd9be7e59d70d6.png)

![](img/6068a95f85ae8d22316918abee8b7305.png)

Grammar `G`生成了只有`terminal`组成的`w`，从`S`开始生成`w`，所有生成的`w`就被称为the language of `G`

![](img/4e80379aefe547619cfa534f5478feb7.png)

![](img/fbaeb2dcf82537f0ab1a079f42046ce4.png)

![](img/3b4cf4f9842998412dfcd43172a38e0a.png)

![](img/be4da8a83235be451521a77a665d24e1.png)

![](img/e11d8509b24c7fcefe4082c0a91416cc.png)

`left`：总是替换最左边

`right`：总是替换最右边

![](img/7f7dd9e8e9d183a5aa5585b64088519b.png)

Parse tree发现`left`和`right`一样

（）（）是`yield` of the tree

但是Parse tree不一定总是一样的

![](img/c0d9a1821bdb83fd3cf1e48e0447d133.png)

有歧义ambiguous

但是可以通过grammar来消除歧义

![](img/8f0a45b972b429786674ce0d4ecc293d.png)

但是有时候歧义来自于language本身

![](img/fe255d00a42f3287feeb95119cf290dc.png)

不知道从`i`到`k`还是`k`到`i`

##### 3.1.2 Norm-form CNF

`CNF`：Chomsky norm form 每条`rule`都必须是三种之一

![](img/bb6a52079722d86c6f9c2d3cfc495220.png)

1.  `S->e`

1.  要么两个非`terminal`

1.  要么一个`terminal`

好处：`derivation`长度是可以控制的

![](img/0338238f0087b37ec954c0bcc0d54029.png)

第二条用`n-1`次，因为每次使用增加长度1，一开始长度为1，所以用`n-1`次，然后剩下长度为`n`，都转成`terminal`，所以再用`n`次

任意`CFG`都可以转换成`CNF`

证明：

五种违反的情况

![](img/d22f24181b30f7ad8abd609006513cdc.png)

![](img/3ef10a8985d512020dddead9f76ab319.png)

> 举个例子

1.  第一种，增加一个新的`S`，指向老的`S`

1.  第二种，`B->e`删掉，会受影响，需要补偿，加一条`A-e`

![](img/8c34f9356bc7af88fbedb3c0d2344f69.png)

1.  第三种，直接去掉，补偿

![](img/bbef4838a41ebcfbc70f6e3f9931f21c.png)

1.  第四种，多步的拆成多个两步的

![](img/a59625298b2db1358d897a3483c78132.png)

1.  第五种，新加一个`non-terminal`，然后这个`non-terminal`转换成被替换的`terminal`

![](img/c28006731e11b4ecfdff8e999116ad3c.png)

#### 3.2 PDA

Pushdown Automata

![](img/e7aa91fdb348e26b265c50a8a39bd452.png)

![](img/23a37492895860597da9c21b3f32e05c.png)

第一个 是`pop`，后面一个是`push`；要对栈做判断

![](img/d2c9885ad7f753094c87ed58e3a82871.png)

123指`stack`里面有123，按顺序

如果 是`e`表示，

1.  栈里有什么都可以执行

1.  不做`pop`

![](img/8fe144454051afe0c7391640bc48192b.png)

![](img/ee85a66eb5099853c16a37ffb5f676c8.png)

![](img/de73b26cd1fcc3179b139e21e0fb9344.png)

+   走若干步

![](img/7cfd7a1e55ed226e1ae0f26b8b1d4855.png)

`PDA` `Accept` input要求

需要满足两个条件

Language of `P`是所有能够接受的字符串的集合

![](img/0074087c2f1c954fffc57fe040282cc1.png)

![](img/a2328f3a81a224dc6480996af0c05861.png)

> Reading 1相反

0和1都放入栈里，让他们数量相等相销

+   读了一个0

+   如果栈是空或者说`0`，要么就先`pop`再`push`，要么就相当于把read的这个`0`，代表一下，`push`进栈里计数

+   如果栈是`1`，那就相消

+   读了一个1

+   相反

但是如何检测栈里是空的？

1.  `NFA`去猜，总能猜对

![](img/e6017a175437694630e4e3063b387b69.png)

1.  用特殊符号来标记栈顶，用`$`标记栈顶

![](img/f417642b3e552c17891522808a6d50f9.png)

![](img/2cda7a6c3244d22737f192dc45d2e5f9.png)

![](img/b4552af06e1f8c6d657e6c59030395f4.png)

左半边，放到栈里面，右半边，跟栈顶作比较，消掉；中点怎么判断，用`NFA`

![](img/cefffbaf319b679b9f5cd6fa88b8de3f.png)

非确定性是必要的，也就是说对于`PDA`来说非确定性>确定性

##### 3.2.2 CFG和PDA等价

![](img/4e06843c56468689d24a63d94ee37654.png)

###### CFG——>PDA

![](img/26dd3d41ed13550b2286b52390e05033.png)

边换边消，直到一样

![](img/189058b0f1977ae728a88dc706078993.png)

+   push `S`，把start symbol放进去

+   如果栈顶是`non-terminal`，用规则去做替换

+   如果栈顶是`terminal`，就做匹配，相同的消掉

![](img/a6a6a5ca9c56f33169b4f1f370484cb7.png)

> 证明就是归纳法，不需要了解

###### PDA——>CFG

![](img/fac8eb82be0f3fa31b3ee41e609028b4.png)

引入simple `PDA`

+   只有一个**final state**

+   每个`transition`要么只做`push`一次，要么只做`pop`一次

`PDA`——>simple `PDA`(考试不考）

![](img/92574b38ac5b737894c0473ef59e9aa9.png)

+   `Final` 大于1，就创建新的`final`

+   其他`push` `pop`有问题的

![](img/9516e8bfc28523718773a2ae94797437.png)

+   2.1 拆，拆成只做`pop`或者`push`的

![](img/c6d162f29e02a8df0797b5fca2166ae9.png)

+   2.2/2.3 拆成`k`个步骤

![](img/38e4262c73c89709d7bc71aff79bd027.png)

+   2.4 新建状态 `r`，增加一个`pop` `b`的和一个`push` `b`的

![](img/0074e610f63e4156e2eb6083610613c4.png)

simple `PDA`——>CFG

![](img/7c5bab322e5f62195b078bb68a1eb858.png)

`terminal`是`symbols`

`non-terminal`：对任意的状态`p`，`q`建立一个`non-terminal`叫`Apq`

从`Apq`出发，生成的`string`正好是满足`PDA`在`p`时，栈为空，读完`u`后能够走到`q`，栈为空

+   `S=Asf`（从初始到结束，并且栈都是空）

+   `Pq`

+   `pq`之间栈也空过

![](img/f7a896f4cfe2fb81051b79e6c955c01b.png)

`Apq->AprArq`

![](img/762cbc096fb575249815cd2fb6e88134.png)

唯一能保证的是一开始的`push`和最后的`pop`是一样的

![](img/8e05d89ef436e54c1c13e83fd212cae7.png)

`Apq——>aAp'q'b`

![](img/0a6d227460786c80464276eea9eeeb9f.png)

![](img/e0759c171fe4a69953350a604ecb7e7d.png)

##### 3.2.3 任何regular语言都是context-free的

![](img/062c743f5fda0cd18144db59b92d6abb.png)

![](img/e6c85564ced340e6e676739678eb9085.png)

##### 3.2.4 CFL的闭合规则

+   交、`connect`、`star`上都是闭合的

+   并和补集都不是闭合的

![](img/983768b76b7f179813dfe9abc865d48e.png)

证明：新加一个`S`，start symbol，让其生成`Sa`，`Sb`

![](img/0e1b563edc874c77fa211a863851fc1b.png)

也是新加一个Start symbol

![](img/cac01c51a7010c978d254df8df6fc5e5.png)

新加一个start symbol，不要忘记生成`e`

![](img/dfd8a09ae8ddb59be6fd58837c0cb6e3.png)

怎么证明？用pumping theorem

##### 3.2.5 pumping theorem

![](img/151633c9fcf2c54067b2e55d61f7d471.png)

1.  第二部分和第四部分重复任意多次

1.  至少有一个非空

1.  中间小于`p`

![](img/4b1493317c32f429c5fbf623b79f3944.png)

证明：

![](img/05cb72c426fa84cfd1ed0a7da0b35ec6.png)

假设某个`non-terminal`出现了两次，我们可以把叶子分类

![](img/336512cc3229d881a6667ba9fbea1f79.png)

![](img/8cfa387abc7ed98f0ff5817810e25093.png)

如何证明百分之百会有某个`non-terminal`出现了两次，当长度到达一定？

![](img/14b783cfa0007a92b6c605745b9a586a.png)

`b`为`non-terminal`能生成的最长的`string`

所以说Parse tree里面内部的节点最多只有`b`个孩子

我们称`fanout`小于等于`b`

我们有一个事实：高度大于等于

保证了当`n`足够大的时候，高度也很大

我们取`p`等于

![](img/694bc18545250a6ee144e0c78269f1bc.png)

`T`为节点数量最小的那棵

那么高度满足

![](img/28a4e776bd4c7b4a606dbc50950ef1e6.png)

边的数量大于高度

`节点数量=边+1`

除了叶子其他都是`non-terminal`，所以`non-terminal`就是

但是总共有 ，所以一定有一个至少出现两次

![](img/410750a676b4d8d4d54e521e27025584.png)

2：如果同时为空，则有更小的parse tree；矛盾

3：证明高度小于等于

🐧`A`是最长的子树

![](img/8d6f5a51df6fd60b919cd40c20edddd8.png)

有个结论，任何非`terminal`出现至最多一次（因为我们选择`Q`的时候是选择重复的，并且最低的那一位，意味着其他都是至多出现一次的）

所以`🐧A`的长度小于

所以得证

![](img/2393b3dc4decf5c08949cbda9ad8f734.png)

![](img/548e5c86d9304aaacdf561a22967e15c.png)

`a`和`c`至少有一个没有出现在`v`和`y`里

当我们把`v`和`y`去掉的时候，`a`和`c`和`b`不可能数量相等

例子2：

定义`surA`是`A`里面后缀的集合

![](img/84a40fbc22dad56eec2a7c2d4c7e487b.png)

![](img/488015f0141a194ee59499606ac93106.png)

重要的是，`u`读完的时候`A`处于什么状态

先猜`u`

构造`P'`

转移方程唯一的区别是不需要读`a`，读`e`就行；其实`u`就是这些`a`

走到某一步，觉得`u`是正确的；就开始读`v`了，需要用栈记录，需要用正常的`PDA`去做，需要跳过去，从`p'`跳到`p` ，读`v`

### 4 图灵机



## 4.1 确定型图灵机`DTM`/`STM`定义

![](img/d8eef3ec1a583a966682c52b0ccba32e.png)

- 无限的纸带：右端无限延长，可以写一些东西
- 最左端用三角形标记
- 空格用`|_|`标记
- 读写头：可以左右移动；可以读可以写
- 写比`stack`灵活，写更多其他的内容
- `State control`

![](img/a2d32ab7216c225bbfa794d4602c4baa.png)

![](img/ac22f602d8b6c9d87e644cfbaf8c3eea.png)

`H`停机

满足

1. 到了最左边一定向右走
1. 不能自己写左端标记

### 4.1.1 运算

![](img/95ebc7ca0b4ad7a2e008ca1876ca8825.png)

当前状态，读写头左边的`string`（包含读写头所在位置），读写头右边的`string`

右边要么是什么都没有（`e`），要么是非`blank`

三种动作：写，走左边，走右边

![](img/ace1cc8481467fc0692acb9d4651a004.png)

![](img/2ce2be5e3428c374c9939f95489ffdbe.png)

![](img/8c3861ae31381565715f609a3aad31c6.png)

![](img/d01267ea5b1cca30cf949c2668a80ce2.png)

走若干步

![](img/42be51f19994801cf5289f98d5be411c.png)

练习：

![](img/6d685812a0803d82b85758c408035e7f.png)

功能：不管什么状态，都写`a`，除非在最左端，先右移再停机

就只有开始和`halt`

![](img/6b00f944cbdca43e48839ca6b4ac5ac3.png)

功能：不管什么状态，都往左（右）移动一格，除非在一开始

![](img/66313cb18448aedcdf7ddf8c565e62ed.png)

功能：对于任意的不包含初始，空的`string`，让`w`整体向左移动一格

左移`machine`：`delta`写起来很麻烦

![](img/f642e95ea043a38fc2c6fa136c27744c.png)

我们利用`basic machines`拼接；举个拼接的例子

我们接下来利用`basic machines`证明

1. 从左往右看，先右移一下，判断是否是一个字符（包含左端和`blank`），再移一次；无论如何就是移动两次

![](img/d127cb294cd1d427ef318b764a2f5456.png)

1. 先右移一次，如果右端不是空格，记作`a`，再右移一次，写`a`；如果是空格，就停机

![](img/6a6bf0e1428046456e33ee8f935040c0.png)

1. 一直右移直到找到当前位置的第一个`blank symbol`为止（先右移，再判断）

![](img/06decfaca5c98defc3bab0e445e4f731.png)

记作

![](img/ed16df5591d7092721a837d423791db6.png)

1. 左移

![](img/0829842900a5b742d51e621f02212487.png)

特殊情况：如果都没有`blank`，最后会在最左边两个横跳

1. 一直往右，直到找到一个`non-blank symbol`

![](img/db37ecd0580029a526d8791f328a1531.png)

![](img/9731a571a4672a066896c0098305148e.png)

![](img/e682443b67d9154dbc24f79805fa033b.png)

现在利用这些`basic machines`推导左移的`machine`

![](img/877120a3a405591da30fae80f956528b.png)

> 两个空格之间所有`string`左移一个

最后直到遇到`blank`，就向左停机

![](img/20492512fe045ecbacfa2f2209419cc6.png)

先右到第一个`0`，然后把`0`写成空，往右两个空格，写`0`，然后左两个空格，把`0`写回去；再重新开始循环

技巧：做标记构建循环

![](img/37557406cf4b04d7c20e24f0a28062e9.png)

## 4.2 `Recoganize` languages

![](img/d9f202fc87d0ca752d38d6cda73708e7.png)

有时候会把`input`写出来：不考虑左端标记和空格

初始状态，读写头是空一格在`blank`里；`s`是初始点

定义`L(M)`语言

![](img/1fa23ad116cad43aaaac287787773544.png)

### 半判定：

![](img/7f9b0355826ed9ef242abcce61606c3b.png)

这样的`M`是递归可枚举的 `RE`语言

![](img/a136667aaec768c56207543346b696c9.png)

例子：

![](img/e0e55a1d330400d4924d45875822a1cf.png)

> 半判定：停机的时候能判断，不停的时候没有办法判定

### 判定图灵机：

![](img/980e701c1067e44ee923dece352919ec.png)

首先保证肯定能停机，对于属于语言的`w`，会停在`y`；否则停在`n`

![](img/3961f58b5f0bdf4a3afee1b84deb2d7c.png)

![](img/dca59666966fc6ce3d3c8fff5b5df3c5.png)

![](img/2ef1d6e969e95f95764b7e004fa1c3f7.png)

例子：

![](img/6488c1a0bafd33e0d3decefae748b5af.png)

![](img/05cfe052a128f17cd97e3de865fe8c6e.png)

> 第一轮`aabbcc`，第一个`a`，`b`，`c`都×掉，`def`就是把原来的涂掉

递归语言和递归可枚举语言关系：

![](img/0d5ec6ff347379c39a891bb91365f292.png)

相当于我有一个可判定的图灵机，我把他改成半判定的图灵机，只需要我改到一定程度`n`上就循环

判定一定半判定，反之不成立

### 计算

![](img/bfc79472eb27d3c9323cc95a3ae117dc.png)

![](img/95a558969add77b43431a48ffb7bf138.png)

当停机的时候满足以上条件

![](img/f249cf0849c3e9b9af1c7c57918ad8f8.png)

![](img/0ed69cd4a9627c94a0dd14805af11bbb.png)

这样的函数，我们称之为递归函数/可计算函数

`C`是`copy manchine`

![](img/b367ea7a57dfb69b7f9a7d6c4180b3dd.png)

二进制

![](img/639597ac18d4a521c2a42b1f80dc4a04.png)

## 4.3 拓展

![](img/c8580f091156eafd83541bce6b6bf26c.png)

### 多带图灵机

有`k`个纸带，可以并行做运算

怎么用单带来模拟多带

![](img/bebe5810e2286249aad3accf4bfe8bb7.png)

![](img/426622eba654dc2b0e923be7e0c77011.png)

![](img/45b64cd912d7ee35a15df9447a5ba914.png)

### 双向图灵机

![](img/4b58900c15cc64a59cdabc380d11ff32.png)

先转换成二带

### 多头图灵机

![](img/63ac31a195aa59f9178b5e1fda1ea4d9.png)

把多头看作一个整体

### 二维

![](img/ee4995cc4b63c9d78249a2ad62da38ac.png)

可以看作一个单带

### 随机图灵机

> 可以跳跃

![](img/8cebb6b1a10c511f64062239c78fed14.png)

跳跃拆成若干步，看作一个整体就可以

### 非确定性图灵机`NTM`

![](img/1190721372e1a8c576ff6fa32ebeb8ee.png)

> 更像是关系而不是函数

`transition`是一个`finite subset`

每一步有多个选择（不唯一）

![](img/66ea892467c40d6d61ac300bd9a704d6.png)

![](img/4499e831d263bf0b950d2391fd6abb62.png)

格局是由（状态，纸带）决定的

和之前是一样的

![](img/10cb99a04d47a914a77c750721184bd6.png)

#### 半判定

![](img/eb96eee4b3d61ec2a06b5f55ea88c883.png)

![](img/82c1888a128f24b439c7d3afa85bca5c.png)

只要有一个停机存在就行

#### 判定

![](img/9d211edc15ad492db5ac9a27ab42eb73.png)

首先保证：所有分支都能`halt`

![](img/dd20a804bbfce40063fc9c97c548cbc2.png)

`w`属于`L`当且仅当有一个分支停在`y`

`w`不属于`L`，所有的分支都拒绝了`w`，都停在了`n`

例子：

![](img/23e255790a4d1090ca9d996382e8432d.png)

非确定性地去猜`P`和`Q`

`C`是所有合数的二进制编码的集合

我们去猜这个`P`和`Q`，将`P`和`Q`相乘，如果相等，则`halt` with `y`；否则就不停（semi decide）

如果`input`是一个合数，会以一个合数接受；如果不是合数，则都被拒绝

## 4.4 用`DTM`/`STM`模拟`NTM`-二者等价

![](img/d63602a634e831ac761ec5f167ba1887.png)

`NTM`可以半判定`DTM`，对于`w`存在某个`branch`是`halt`；

我们用`DMT`就是去找这个`branch`

- `DFS`
- 如果一个不停机，就寄了
- `BFS`
- 一层一层找

![](img/cb7eea3decd70e5b7778609bf0f07f7a.png)

![](img/54d6b225ffd22f56caaaee1c9f0e8d0a.png)

用三条纸带来模拟`N`

1. 存`input`
1. 模拟`NTM`
1. 枚举指令（树节点编号）

- 第一步写什么，走什么，比如说`1`
- 如果停机，就停机，否则
- 重置，选`2`；循环
- 如果都没有，就下一层
- 选`1`,`1`；不是再重置
- 如果存在`halting state`，一定会停机
- 如果是无限的，一直往下找，不会停机

所以当且仅当有停机的（半判定`L`），才可以

判定的证明是类似的，就是找`y`

![](img/5cddbf96abfbff6fcc1c40cda0c9b861.png)

![](img/a46a98e08e34e0f90e737e53e882664d.png)

## 4.5 算法就是图灵机

算法就是一台总能停机的图灵机

![](img/baeb8cbd109a9de351165557f51ee457.png)

图灵机在判定语言就等价于算法在`solve problem`

图灵机描述可以分为三层

- 正式定义
- `Diagram`
- 伪代码

> 有个问题是对于输入，需要进行编码

![](img/75b747680f14e432086b1e29af3ed25b.png)

任意有限集合都可以编码

![](img/4d07fb548f947a3127babe24b3daaaeb.png)

先把`ABCD`做编码，再拼起来

![](img/23f42b7650128248fb2ff73f53f0df00.png)

图也可以编码，图灵机本身也可以编码

![](img/ffff793c495085871785abc828452a5b.png)

用图灵机来描述算法

![](img/d24a42cd534920113fb5a63372c9cd81.png)

## 4.6 递归

![](img/55d27d17c593149836e3a256f25406b7.png)

![](img/86d7bbfa313e696fc02986c59b908d2b.png)

`ADFA`是一个`language`

![](img/2ffcf207f995fac4526835d8dbb2a5ab.png)

把`NFA`转换成一台等价的`DFA`；跑`R1`就可以得出

![](img/95612c86e6200c4e568ef60e521c7d37.png)

把`R2`的`input`转换成`R1`的`input`，`N`接受`w`当且仅当`D`能接受`w`

保证转换前后是一样的

规约：

![](img/8548497703ecd86f503605f68564aa2d.png)

![](img/526622efb48045db51cb0e0b98a7e3fe.png)

![](img/8fbc20e08583973966ca6ac2f7b0beea.png)

![](img/d0e50bff4ad21f8c024e8808283af44f.png)

![](img/b3eb7636dd0e435f066a5afa7b887b7f.png)

## 4.7 `Unrestricted grammar`语法（最大的语法）

![](img/2d0adad116f62b3b5bb7fdcb7596a0bc.png)

> 任意文法规则只需要左边至少有一个非终结符

> 可以看到`R`的左边不止是非终结符，而上下文无关是只有非终结符；我们扩展了文法的定义，显然上下文无关文法属于任意文法

规则

![](img/3c11702a8c5e73b9cb2ba29f757862a7.png)

![](img/4821ef3d35cf9b8c2c05890c0e9a5ca0.png)

![](img/f5175801498a54b17fed01bc4340146a.png)

为什么不直接`abc`，因为左边不能是`terminal`

所以引入了`ABC`

![](img/370c2a2eefdecbb92e18deb359b80a82.png)

> 语法生成的语言也是递归可枚举的

![](img/dec03e61fac70f361260326414af47f2.png)

![](img/780874a3e0534e55783851db0a2707e1.png)

`S`是`grammar`里的`initial symbol`

先有图灵机

图灵机证明可停机，就是把`w`放进来，然后看停不停机

![](img/4d9e881f3c8ff73c80f5d15e7989b2da.png)

右端标记——使有一个有限串，否则全是空格

停下来，停到同一格局，左边标记 空格 右端标记

现在我们反过来，我们先有这个格局 左边标记 空格 右端标记

用文法写反向推导 反过来做 最后变成`w`就行了

![](img/4c91132f683b2aef03dd0d49f448b2f9.png)

本身`q`只是状态，无关，这里强行让他有关

改一下格局，把`q`插进来，加一个右端标记

![](img/69ab981aa341cad63d75f61a3355bf8f.png)

我们找到了最终的格局，然后倒推

重新定义替换，右移，左移

图灵机向下做，文法向上做

![](img/256bd93d91feafbed344bc47aa4749f2.png)

## 4.8 `Numerical Functions`

![](img/d51aeb65867bf4ae2c9072611e50dda3.png)

![](img/d8551da268678cdc0dff99d5d1fb9202.png)

![](img/149773cd56a14f270a9f4109088a3ec0.png)

![](img/93239a4ca3b598998e2c78ff3a2dc51c.png)

![](img/724d9702863ff0163efeb42e08099dd8.png)

![](img/a182378d217c0a6ed22127d059550a60.png)

![](img/da6e78e95363ab4e1cfc4f035edb0772.png)

![](img/35ad7f19a3ceef142712b745dfee1671.png)

![](img/b43a7337082879a6fd0ca59d20302453.png)

![](img/ebb83f1bef4262313755fc188998bb71.png)

进制和奇偶

![](img/a8b1f86d4477161ce4c575d2e8db9bfc.png)

![](img/036631ca9b237daabc02e9e8fde60290.png)

全称量词和存在量词

![](img/047322d9a92d9ff43f68375f566c61ff.png)

![](img/3334103f0c77794b5d08990765480067.png)

![](img/7ca2679dae1bfdad4558102721629e2e.png)

原始递归函数是递归函数的真子集

原始递归函数一定是可枚举的

![](img/43a95ff07b9dcdc8deb696267ac01c86.png)

原始递归不是递归，是递归的真子集，他没有极小化，加了极小化之后成为`u`递归，就是递归

![](img/df4ad6aa9bccb695bddb90677b7a224c.png)

`um`就是使里面等式成立的最小的`m`



![](img/ed1e80814f7af6c610b89b20048fa156.png)

![](img/7f5b6c2e884cd0f8582190f7fd8bfe21.png)

只能枚举

如果一定是可以终止的，我们称这个函数是可极小化的

![](img/7a7917cf3026429a1435cf4326b6c2d4.png)

`up` 就是让 的最小`p`

![](img/9de23b0bad5d2e65573657887711f1a3.png)

图灵可计算 = 图灵可递归 = 可判定 = 字典序下的图灵可枚举 = `u`递归

图灵可枚举 = 递归可枚举

![](img/52570af35a10fd6b0eac7e2994713bce.png)

![](img/881153c629ddc5098c075d3663676203.png)

![](img/794154dfe0da5f25a4df9579552876e8.png)

![](img/b8c919029dda337843ed7a8478d247f2.png)

![](img/5c34bb65b1b6ab47beb77568e1b0a521.png)

`comp` 对 `n` 做计算，`last` 得到最后的格局，`output` 输出 `b` 进制结果，`num` 将 `b` 进制转成 `2` 进制

# 5 不可判定性

![](img/24a6c20bd95dfaa601db059de24ce59e.png)

## 5.1 广义图灵机 `UTM`

![](img/0dc42931b67c8ff2c9811709e8f0fada.png)

![](img/8d8547be13988937e5bc60a2af55e6f8.png)

![](img/e09c4ec95f2a5de39826ac0391a302e4.png)

![](img/001bf8821827b3760202cd31d0b6d244.png)

![](img/a2ce0366751e43f7a5870bd699c2a08b.png)

![](img/e6b8d490d6319641d03403403890f0da.png)

![](img/6f7f8f7a23a9a33cf7dd9187ea48e34f.png)

![](img/389ac5bcdec26dec87ee0ccecde2378a.png)

+   `W`

+   `M`

+   `M`的状态

`M`跑`w`在编码的情况下

## 5.2 停机问题

![](img/ca40de97eaec371c61370dc3a3075ca8.png)

> `H`就是停机问题

假设存在`M*`，则`M*`的编码属于`H2`，那么`M`不停机，但是是半判定的，也就是如果串属于`H2`，就会停机；反过来也是矛盾的，所以`H`不是递归可枚举语言

![](img/de1d850bf1dc96dea2ea577df8026621.png)

## 5.3 规约与里斯定理

![](img/026b2e93c6e25e8a58565a79cbeb3508.png)

![](img/db54b8691e090191a325996e52848ad4.png)

![](img/44ecf362813eaec6e294fc74a2cd6c5a.png)

![](img/563474693befe0eb690d518a080e0387.png)

![](img/61ec15d54ab22fb562d3cc4b6634f309.png)

![](img/5c81629bd19367ea2fc52ad6af1f4bae.png)

开始枚举`L`，周期性的经过`q`状态，生成的所有串

![](img/0ad8f548328c845361cebcf44dc6591b.png)

![](img/2aa2594a89b2067b6a3d2386482281e8.png)

![](img/af4a7850850a6268c114c39f04ef7fa3.png)

![](img/035685cca1de971e7a699a1954b5cbaf.png)

![](img/4d52969933522f5ca955a23a672fb929.png)

所有非平凡的性质都是不可判定的

> 空集和全集是平凡情况

> 平凡（trivial）通常指的是非常简单、显然的情况，通常不需要特别的思考或技巧即可得出结论。

## 5.4 自动机的判定性问题

![](img/069798905ccc72596d3adb88874868af.png)

![](img/f548e6eadd0bce77a54c5454d56a1e79.png)

# 6 结论

+   `L+`是包括`L`且在连接运算下封闭的最小语言

+   能用正则表达式表达的语言是正则语言

+   A language is `regular` if it is accepted by some `DFA`

+   The class of regular languages is closed under `union`、`交`、`连接`、`补`、`star`

+   任给一个 `DFA`，都存在与其等价的 `NFA`，反之亦然．

+   A language is regular iff it is accepted by some `NFA`

![](img/f3ad0ee7a8c62ed96346e7e90d4bcab2.png)

+   匹配括号不是正则语言

+   `pumping`

+   所有的正则语言都是上下文无关语言．

+   上下文无关语言与正则语言的`交`/`并`还是上下文无关语言.

+   自己的并不是闭合的

+   上下文无关语言

+   `交`、`connect`、`star`上都是闭合的

+   并和补集都不是闭合的

+   递归语言在`并`，`交`，`补`下都是封闭的．递归语言一定是递归可枚举语言．

+   无限制语法生成的语言也是递归可枚举的`re`

+   原始递归函数是递归函数的真子集

+   原始递归函数一定是可枚举的

+   可识别 = 半判定

+   图灵可计算 = 图灵可递归 = 可判定 = 字典序下的图灵可枚举 = `u`递归

+   图灵可枚举 = 递归可枚举`re`

+   图灵可枚举：一种计算模型（即图灵机）可以枚举或列举出可计算函数中的所有元素

+   `H`的补不是`re`

+   递归语言是递归可枚举语言的真子集

![](img/ff35853a7d6e151848d13a58f4a6e0a9.png)

![](img/c2f43bf8c1450cb325e3a41ba9496b67.png)

![](img/4fcb7ce29c37f40f6502f69e85bbaeaf.png)

![](img/bdf9988ad84840d6de320861c3d1ff01.png)

![](img/7e4655b426ce2372c21e3b33b31ddbdb.png)

+   由于`L1`是`re`，`L1`交`L2`是`r`，`L1`并`L2`是`r`，所以非`L2`是`re`，又因为`L2`是`re`，所以`L2`是`r`，同理得`L1`是`r`

+   第二题：因为`A`是`re`，`B`是`re`，所以`A`并`B`是`re`，又因为非（`A`并`B`）是`re`，所以`A`并`B`是`r`，又因为`A`交`B`是空集也是`r`，所以由1得`A`和`B`都是`r`

+   停机问题在递归语言外面，在递归可枚举语言里面

![](img/cd63db24a6f430c59f4f25ea31d27310.png)

![](img/7c85277167afdf74c0934620b2d0f876.png)

![](img/18090e930fd84c832580143b7e06514f.png)

![](img/18bccae7b1fb43c6f1f646d756fddb4d.png)

![](img/5fb0907b52cca39a4274634284c30e4f.png)

错

![](img/45ee9b791206948163112ddb3ebf07ce.png)

错

![](img/f344d5c711f82d9997de000ab407d488.png)

![](img/658c353aa583a079b99dabbf4812518c.png)

正则对于减法闭合

# 题目

## Chapter 1

![](img/a0fb3c7217ad336bc71b523a56cb94c5.png)

## Chapter 2

![](img/7a7f6f7f3ea77164052abf431b96f699.png)

![](img/e5c0616f2e406603f09f659579c39ac7.png)

![](img/3389d4362bfdef2f72a805d749172a6b.png)

`g`举了一种例子，已经是`sigma` 星号了，肯定包含

## Quiz 1

![](img/7a2d59a04394d84f78983d158b286f8e.png)

![](img/afe760e7b27074b2e7462e2f9249191f.png)

1.  是，和自然数一一映射；有限字符的有限长度

1.  对，`L={e}`

1.  `a*`，`L`是`a*`的子集，`a*`是countable；`2`的`a*`次不是countable

1.  是正则，`{a,b}`七个连接 再`*`；长度关系

1.  不正则，`pumping`

1.  正则，有限元

1.  正则，同余的就是可穷举的，可以列出所有的情况，然后再链接什么的

1.  正则，回文加里面一个任意内容

1.  不是正则，用`pumping`，当做

1.  不是， 假设`A`是空集，`C`是

## Chapter 3

## Chapter 4

## 其他

### 1 rice定理类似问题

![](img/a7ef642989d3c64e5aedc5a746082339.png)

`a`是不存在这样的语言，所以`L5`是空集，不是`L`是空集

![](img/7630f09e6e6213b7de506c0bd280db75.png)

![](img/5cbca647c61d171cef75dda2c26595a5.png)

两个otherwise后，应该是does'nt

![](img/2aed82ac6f946ddf3006a8d80a6304b9.png)

![](img/2f66766c9c1cbf272a6500433a1e6058.png)

![](img/e35b7bc3e8538c5fb263fd52d20f611d.png)

### 2 证明`regular`

怎么证明一个语言`regular`？

+   定义，`DFA`，`NFA` `regular expression`

+   Closure property：`union`，`intersection`，`star`，`complement`，`concatenation`

![](img/1a134171cd436140f038ecf8375fd22c.png)

### 3 判断

+   `A` `FA` accepts a language iff this `FA` accepts every string in this language AND `REJECTS` EVERY STRING NOT IN THIS LANGUAGE

![](img/666eb12e65f713174c3140ef3389181f.png)

No

+

### 4 `NFA`到`DFA`

先构建`NFA`，之后从`E（S）`开始，挨个输入，再根据`E（S）`到的地方，再`E`他们，

### 5 `pumping`

![](img/17301d3a123b19aadd6f9d767808d636.png)

第一个是取补
