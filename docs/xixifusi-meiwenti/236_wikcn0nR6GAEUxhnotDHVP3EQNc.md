# 编程语言原理

> 来源：[https://docs.feishu.cn/docx/X2wodKuXKojRZJxZDIWcH4sqnzd](https://docs.feishu.cn/docx/X2wodKuXKojRZJxZDIWcH4sqnzd)

> RyanFcr

# 0 About Class

## **0.1 考核方式**

+   课堂讨论与测试 20%

+   单元作业 40%（书面作业和编程作业）

+   期末考试 40%

![](img/a249ebda5e2ff4096ea4dac98406c82e.png)

![](img/4ce5a4c07e2e4df70cdec94edbe6f804.png)

# 1 基础

## 1.1  calculus

![](img/ce7d13c0420c76e8c64e21abae02ae50.png)

[1 Lambda.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnm3WpJOduGH4JoKag51A8oe)

![](img/7136f9edb250d3fb2ff8232ca96333d7.png)

[4-lambda_calculus.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcn9GhzFgQX36vxuvIYR0eHdb)

![](img/4717d7e97a0e262bbd05e394e5aae4b0.png)

![](img/578e633483a53856b3e6ff0f7fcb7c4c.png)

> 应用：t作用于s，t是函数，s是参数，把s带入到t中

项分为变量、抽象、应用。定义的定义存在递归定义

对于抽象： 相当于def，x是变量，t是结果

大多是**单字母**表示

BNF：表达编程语言的一种表达范式

![](img/c5d08663e264622b1ba80d73ec41ee3c.png)

尽可能尽量延伸，直到**括号或下一个**** **

**(x+5)是一个整体**

![](img/4cd7ae861b6ad4483444e7cd95678c72.png)

> 一个 函数输入输出都可以是函数

> 应用必须要括号

> I 表示单位矩阵，恒等 **Eye**

![](img/38b0e9851f06affa10afd2580c825e52.png)

> x先作用到y，再把结果作用到z，再计算整个

Inductive Definition

![](img/58472837e4f7bde481f2755e78629232.png)

+   **Base** case

+   **Inductive** case

![](img/3481d5953d9aca0fde282c35b4efe635.png)

+   If x是一个变量，则x是一个term

+   如果x，var，M是一个term，则 (x.M)也是一个term

+   如果M1和M2是一个term，则把M1应用到M2这个ap也是一个term

对应之前到变量、抽象、应用

如果线上的是对的，线下的也是对的

![](img/fb030c3592ac4a3645bc43cc752e2f17.png)

+   只要进行一次运用，size+1

+   size是一个抽象的映射

![](img/03792a7cf7cb006795a2070eadf784bf.png)

+   出现在表达式里的是绑定变量，可以任意改名

+   没有出现的是自由变量

![](img/59a59f27c7858b8cd97f59dc4dbfd05f.png)

![](img/898c95d03c9c0ba01f1c2f8d4d87591a.png)

![](img/9704e87124b4723be7dcfae8e382d819.png)

> 空集减任何还是空集

![](img/3264e03faf1aee9c0e8d755b9f6b2d1e.png)

![](img/7aebc1af32346625a7be00d6b3930d63.png)

![](img/762b8d86365aef2c8e57c8e07aebf07c.png)

![](img/93088c421b93a33c12ac3fadbc7baaad.png)

![](img/6c6b9c884827355f5d1b8933e7d259ef.png)

+   定义等价

+   语义等价

+

+

+

![](img/29028f98b2a554cd0f6cd21e4c2f6630.png)

在定义上代换

> 只能替换自由变量

> 不能有捕获

![](img/c40a2f241942d6e5910dd732424df7f2.png)

必须要有**封闭**，相当于把t2带入到t1`里

把t1`的x用t2代掉

在应用里代换

> 相当于把t1`中的x用t2代掉

+   Reduction: 相当于运算，t2代入函数，封闭意味着t2不是自由变量

+   相当于先introduce然后use的化简

![](img/b292ba187fbd7bbb45eb6c5f7f1b8498.png)

![](img/f79ae470276c5be60413f813be9455d9.png)

> 严格是没有

> 应该是

> 先use再introduce就会变成原来的东西：

![](img/ef93f92d49eb8ee3919a45e0d9ad52b0.png)

> 加一撇 和 字母大写 表示不是变量，可能是一个 项

![](img/ed4b1cfed3168519f3408fd0f2c4aec3.png)

![](img/61f037a60f3ad36a83b827ad99defdbf.png)

![](img/6d49f1a2f9e9e002dd87949d4eb82bc7.png)

> 把y带入到

因为两个y不是一个y，没有封闭，需要改名

![](img/dd786ada2829bc9f7ac73e3aee9dcd92.png)

所有函数在lambda 中是prefix并且只需要一个参数

但是如果想多个参数，我们需要currying

比如，将一个需要两个参数的函数，喂一个参数，生成一个需要一个参数的函数

![](img/af3fc0ef016967f3ec6a486d5280fea2.png)

计算

![](img/1ee06038f1f35a6bb6cba88d469ca218.png)

![](img/d3f5293d993a12c50f150250335dd414.png)

不封闭需要先替换

![](img/e1186572c0ea97fd0ee4547bcde2c4af.png)

![](img/c1e50c18ad9396041aa907099788c186.png)

![](img/aa37c5e14437ebacd954113a18f2dba5.png)

![](img/a5bf1aa3305c14929ffa73d71372f7b6.png)

e1e2表示ap

![](img/9cb6c92d20be481f92dcdef68c8721d6.png)

表示布尔量

![](img/1a64b3ccde5f54d7e40f35b0c274af2d.png)

![](img/a2d415e325074893ef99cd30c69b7880.png)

逻辑其实只是一个**选择**的过程

True A B = A

False A B = B

> 除了这种写法还有很多种写法

![](img/b36785258229c4191f6e8e067b7e35ad.png)

not怎么实现？

![](img/9c8680866d698bcbbfd6d4811fceb349.png)

TRUE := λx y.x

FALSE := λx y.y

AND := λp q.p q FALSE

OR := λp q.p TRUE q

NOT := λp.p FALSE TRUE

IFTHENELSE := λp x y.p x y

isZero = lambda n. n (lambda x. false) true

多位布尔值——>笛卡尔积

![](img/ee0ea136ef301b215952a1f5ca50ea14.png)

p1和p2是投影函数，就相当于是true or false的选择函数

![](img/65184922a53100a78492d68955486324.png)

表达自然数

![](img/bfbbe966fa8d78d2c453b22a65391171.png)

0做任何计算还是0

1是0的下一个

n是n次s的调用，用调用次数来表示自然数

s是下一个方程

![](img/be1c65514e7f41e8d1e0ccb9a816c2bd.png)

> succ相当于 的那个s n times变成n s

![](img/cc3f8b5daead1f08e6d580c56ab291bd.png)

> 第一行应该是 s z 反了

> 然后用

![](img/c03f7c5252a4f60a35d564d3e4f3d686.png)

> 这个来推

![](img/0d6a6b6a3b3d45e9215ef4064bb5dff9.png)

> 幂的话相当于是把+1这个东西替换成做m次什么

![](img/e64fbe8645edfecce938db81b6815baf.png)

![](img/33d06d5a91a0869816bb5c617bb86bcc.png)

## 1.2 Abstract Syntax 抽象语法

![](img/fe7c889593cdad3ac7846bc6ca5948ff.png)

[2 ast.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnP0HSCsA5dfCVWarDC1UzWf)

+   语言的语法表明各种短语（**表达式、命令、声明**等等）是如何组合起来形成程序的

+   表面语法关心短语如何输入和显示，以某种字母表来表达

+   抽象语法（结构）关心如何用短语组合出其他短语

![](img/b0338f55b95152b23e1020de93db2dfe.png)

引入**新的标志符**

ABT是AST的扩充

### 1.2.1 AST 抽象语法树

![](img/56a890bc9ecf218ffb8ccbcfdf4e8fb1.png)

> 修正：叶节点是变量或者没有参数的运算符

> 运算符是连接两个参数的运算符，不是狭义的运算符

> 变量是语法片段，也不是狭义的语法片段

![](img/789799974639520de2c45ef3b396c5c2.png)

![](img/4a963150649952e361cbe668c291ba0c.png)

右边有两个def，三个assignment

![](img/f5b7b0550d2b017a2f7cd1366bcfb8b5.png)

while

> 是静态的，只反应结构，不表达执行

> 计算图表示执行

![](img/9ceaba6a10436aeb4b66a640f7bd6673.png)

![](img/f376d0d3ce5a63e21eebf8329922c8f5.png)

常用 **表达式 命令** 分

![](img/6d6ca5eef0c9acfc2c85c5e391cc7183.png)

![](img/40589b291e850c1058b9ce7f1e7a6e80.png)

![](img/e720735fa8c75699c0f6ce8c3766d844.png)

![](img/cd7df191c7d03908e758a673d7871949.png)

![](img/fef26c65ca9563630df0e162efa08819.png)

![](img/7e12e431e3af2e400bc4309e37b2436e.png)

![](img/ff3c009cfba9a9a1c50c49ed453cc655.png)

![](img/1f2813d78e9a0a9fa2ffeadb8c9847d1.png)

总结：

![](img/169cb9c21ebbf8bea728d1c4cdc8e143.png)

![](img/33841007b7328cd73c634d65cd80d433.png)

![](img/43ae648a92f987bb1b766a838926d92d.png)

![](img/ff6acd46ea8c06aa41e0741460821a15.png)

如果每一个都是在族里面的，那么运算后的结果也在族里面

把a里所有的x替换后，仍属于

![](img/bcc0da3f7bfd9d775f1cf8dde4ea8160.png)

![](img/a6667c9bfc0828cfedeeae9dce7fbae6.png)

![](img/eb857a7036c2b7b3fde38e41d8f7549d.png)

唯一是怎么来的？

+   若a = x

+   若a = y != x

+   若a = o(a1;……;an)

### 1.2.2 ABT 抽象绑定树

![](img/5fcea151d094d2c1ec5ca58c90366542.png)

![](img/f1ac7de5addf6c711c2d806667d53abb.png)

> 可以直接括号

![](img/6bae85e3c4b770ed4b6767e9b94b6e54.png)

![](img/eefef5ed43b88596b7efff2e9434af08.png)

![](img/4403e6b5561dfb7826dec28d667cd234.png)

![](img/ce335ae74a52250f7cf854328a4c8bfa.png)

X是形参

![](img/3a249c17cd0ebf18acd8ca71599023eb.png)

但是定义没有正确解释约束变量的重命名，解决方法是使用新的重命名（fresh renaming），其实就是重命名

![](img/033e0ef12507d11ed172eb7bc2e7e8e3.png)

> -等价  双向的

![](img/15fd30cc09b542acb92830fe5527ad4a.png)

标识约定

抽象绑定树总是根据 a**-等价**决定是否相同，a-等价的abt被视作相同的

## 1.3 Inductive Definition 归纳定义

![](img/887be530187eb3ce2dabb9b4b98d55b9.png)

[3 Inductive Definition.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcn3TvEG4CPxz7QBAqCzzBWqm)

![](img/cb570e675b51abfd8319ebe43cd3a581.png)

基于某个规则，某个判断成立了

![](img/9646148e6c64eccafdf340eeeba7b55a.png)

> nat：natural

> Int type：int是一个类型

![](img/5adf9f4edd7cafc800f54e17c67ef3fd.png)

> 一个是性质，多个是关系

### 1.3.1 推理规则

![](img/e5acf300dde80d44e738a29e8311cbd2.png)

![](img/f0368ee6fddd90a08572c43f5ba062fb.png)

![](img/ba796d3ca4ff00e0f8ad03319a3feed0.png)

### 1.3.2 推导

![](img/636a483b76d5baa7531c1459990de12f.png)

forward = bottom-up 从base case往前走

Backward = top-down 从某个点朝base case走

![](img/55fc67c17f7f3882ae832394ade23d25.png)

把自然数放进list里

![](img/e518d617fb3ddd1d7748eb6b0a13c6d7.png)

![](img/2edfc087a28d5a4ccf1f7dad715978f6.png)

![](img/2b39ee7e3837c31f630a2afea7103f3c.png)

之前我们看见过的tree、list都是迭代

联立：奇数、偶数，互相定义

![](img/500d1fee1c8c5d5244311b6a76fd68d6.png)

+   存在性

+   唯一性

## 1.4 假言判断于一般性判断

**Hypo**thesis 假言判断 ：一个或多个假设与一个结论之间的蕴含关系

两个蕴含概念：

+   可推导 derivability

+   扩展新规则情况下，稳定

+   可以推导出来的

+   可纳性 admissibility

+   不稳定

+   逻辑上具有蕴含关系

+   函数的类型

一般性判断：判断的普遍性或通用性

+   泛型：一个判断中各变量的所有可替代实例的一般性

+   参数化：与符号重命名有关的一般性

### 1.4.1 假言判断

#### 1.4.1.1 可导性

可导性判断

可导性在扩展新规则时是稳定的

三个性质

+   自反性：如果J成立，那么能够推出J

![](img/3bd92bc053e65003db80a1ccddfae480.png)

+   弱化

![](img/e5046b5b71043cdafd3925a66e44aae7.png)

+   传递性

![](img/69b483ffc0e470bccd7fcd95a4632725.png)

#### 1.4.1.2 可纳性

较弱的假言判断形势

在对规则扩展时并不稳定

可导性比可纳性更强

定理3.2

逆命题是错误的

+   自反

+   弱化

+   传递性

### 1.4.2 假言归纳定义

### 1.4.3 一般性判断

+   泛型判断

+   参数化判断

### 1.4.4 泛型归纳定义

泛型可导性：

![](img/77351bfc588b401d00202281703f717c.png)

J包含x，y；我们把y提出来，前提是x和y不交；y或大gamma可以推出

y中的每一个小项也是可以推出J的

+   增殖

+   重命名

+   y`对于X，Y同类型，但是不在里面

+   把Y里的小y都换成y`，对应的J里的小y也都换成y`

![](img/13838fcf24c3ff5b10069cf079cc1ed6.png)

+   代换

![](img/75d6d15dcb8ffd639aac524070f610f7.png)

把y代换成a

> 替换是降低复杂度的，换名只是形式上的替换

> C++ template就是泛型，java是参数化，会把T类型变成object公共父类，类里有一个隐藏的成员变量记录类型；对于Java来说，类是对象，解释性的语言，一切都是字符串，类型可以作为变量

> python（解释语言）做编译执行的难度  需要一个runtime；不能简单的一比一对应到汇编语言

+   python中的pyc扩展名文件，类似于Java的Class字节码文件，是由编译器编译得来的，供解释器使用；当我们部署程序时，不想暴露源码，可以使用pyc文件；

> c（编译语言）做解释的难度

# 2 静态语义和动态语义


[5 静态和动态语义.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnsvdFoymO67iz0uHtE7rgTh)

## 2.1 静态语义

## 2.2 动态语义

## 2.3 类型安全

# 3 函数和高阶递归系统T

## 3.1 函数定义和值

![](img/c3020fe005c72d5714cd32ae0df1545b.png)

[6 Function and Recuisive.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcn5zOujiUuSWiY6eINy3AVLb)

![](img/4d5c604e82499df5f13c5c846f9ab073.png)

+   One step reduction 一步

+   Deterministic strategy

+   Nondeterministic strategy

= \displaystyle \to

Transitive closure 传递闭包

![](img/f9a497c7077dfe746071c616ae44c162.png)

是最小的传递关系

Reflexive closure 自反传递

![](img/f8fdb2e3f2259acd6a0a30df4b17e36f.png)

是包含R的最小的自反关系

reduction是在数学上把一个term转变成定义更小的一个term

## 3.2 高阶递归的系统T

# 4 有限数据类型

## 4.1  积类型

![](img/64b6d4da2a32c2cd2eb934c64c24c341.png)

[7 Product and Sum Types.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcn6xwh6XV5wizQAt37PE63Tc)

![](img/425dd807ca3f63aac9d36d91a128804d.png)

![](img/d85ab22100b96dd4f62db3d07b1b527c.png)

保持对新名词的敬畏，同时利用已经学过的东西迁移学习

# 5 无限数据类型

![](img/c19f82ac09ff4e34eb297e470b2abc1f.png)

[8 Inifite Data Type.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnPDhLfdzHQCP4mfgbbGCrbt)

![](img/e27c44bbab5cca300701e77a574463ce.png)

![](img/3553fae30950b6205ff0db7713a59bdb.png)

![](img/c0569c735e56896079aebd41d2a06ee9.png)

![](img/622b427dc6f5a28b5309579994114dd2.png)

一头有终点，计算往有终点的那一端靠近

![](img/4e363dbe150507959da46b2f88f68461.png)

![](img/56bac3a2dc3ac18d7b448fda97aebd49.png)

tail的n次相当于取第几个元素

![](img/ea8683d3d68eeeff357c7dbe68d7e6aa.png)

> 互模拟：两个玩意，相同的输入有相同的输出

> 互模拟关系，头是一样的，去掉头之后，新的头也是一样的

Even = odd   tail

的意思是even可以由odd和tail结合表示，而没有解释具体怎么实现

最后公式错了，应该是

1.

![](img/a11b8b4763c64f9ec67b9263cd32be84.png)

> 具体推导

![](img/8ffa4ee529cba2504275232810517235.png)

> 作业题：利用归纳和等式演算，证明

![](img/c3a7bacd46ccf4767d75a1f47d8a4f81.png)

![](img/6ffeb40a65b9091daea6a5e655f2ed68.png)

归纳和余归纳是不动点的最小点和最大点

![](img/0ad65af3fa647d3696a4ca7bd98cd5c2.png)

![](img/c7b423a0916ae84f19951d7926f858f9.png)

![](img/47368138f7ed908b7c8096c8ab586af7.png)

![](img/01fbf1e2a7fa5ebb8543471d49960c86.png)

fold是引入，引入和消去是一对逆过程

引入静态：判断代码是否正确

消去动态：如何得到某个值

归纳类型

![](img/7ca54beab5618e5a9cf78745ee17e8cd.png)

余归纳类型

![](img/c203096808bbe38b1f0d17fef6c17d92.png)

Load, store是瓶颈，如何减少load store

hd，tl元运算，可以推导其他运算，但是没有其他运算符可以推导出他们

# 6 多态类型的系统

![](img/4dfbce94a85df0b39aa9c3f57b3eb7ef.png)

[9 Variable Types.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnFx3CUhzAwxSjNdRxLuvYvc)

## 6.1 变量类型

![](img/df488dc7d44ae6dd4c6898f4f7b4b56b.png)

+   Static polymorphism: binding at compile-time

+   Parametric polymorphism参数化多态

+   polymorphism(FPL), templates, generics

+   Ad-hoc

+   Overload：操作符overload

+   Coercion：类型转换

+   Int i = 6.5

+   Dynamic polymorphism: binding at run-time

+   Subtyping polymorphism

+   Inheritance, virtual function

+   继承，虚函数

![](img/3d65a4c4982260bc0a1b3ae0621f1bc5.png)

[9 Variable Types(1).pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnfN4O6PEBY8o1q4fYzlJYPb)

数据类型是一个集合

+   变量的值都是在集合里的

+   有一些操作可以应用在这些值上

![](img/6729f6b3d0bd36d083274c410f9c188d.png)

一开始尽可能的include所有type，后来有了user-designed type

抽象数据的使用和实现的隔离分离的

![](img/30de7a97daff56cd9ba95e291dcd0844.png)

![](img/09aec1dda4b5b9bb3a0fa10cb1cbb97f.png)

+   怎么确定一个type

+   什么时候绑定

+   静态：编译

+   显式声明

+   隐式声明

语言没有解释编译之分

编译语言变成解释语言很简单：写个解释器很方便

编译变成编译比较难：有重型的库

+   解释语言，也可以在前面之前做一遍类型检查；加速，通过了静态检查之后不需要做动态检查

+   动态

+   运行时

![](img/a6849fb2aefe7f405a579ad07d966b9b.png)

+   python不是没有类型的语言，而是不需要声明的语言

![](img/424b69238eeaa3ebea5ebea6d9b2be8d.png)

![](img/c1f706f07943070f352187d58e7b3e0c.png)

> Coercion自动类型转换 ：（int）B——>（real）B，结果再从real变成int

> 工程上不太care整数和浮点

+   绑定静态——》几乎都可以静态类型检查

+   指针没有办法检查

+   绑定动态——》动态类型检查

![](img/e03755d4c65c64b3cf2520af87ac8b27.png)

强类型：总是类型检查

![](img/5fd8972907f7b1c017ee478498788e6b.png)

+   数据、结构、用户自定义就比较麻烦

+   对于结构体（下面的方式是我们设计PL需要去思考的问题）——如何表达类型相等？

+   结构一样，名字不一样

+   结构一样，名字一样

+   结构不一样，名字一样

![](img/c690c9bd25ca3f4ba6b61ce1ab4c4693.png)

+   等价

+   要不相同一个声明里

+   要不声明用了相同的type

+   某个类型的子集和该类型不等价

+   形参和实参需要一样

![](img/40b1103f5c34fc3cc77add6915d3b3e7.png)

> 简单的通过名字，名字相同，就认为是同样类型

> 保证一个名字只编译一次；c不一样，有分离的头文件；现在不依赖头文件，所以一定是保持一致，不会出现我代码和include的头文件里的东西不一样

![](img/d0064343c0014e2b781108ee74393d6e.png)

![](img/674b51a9ec12d8fe5e09e1324ab9f301.png)

![](img/294152a010b70584bf32b12303636b97.png)

![](img/4c42e62a51cf1702493ebb8634023557.png)

![](img/697321edbc7306357881b5ebadac6a0a.png)

> 断言是逻辑定理，计算是证明

![](img/4c2d859df40cbde19bf5bfb6f5f555ac.png)

boolean可以得到string——boolean蕴含string

![](img/e94e237b6d657d8f7f5d015542763869.png)

不可变性

![](img/98679cbe5147c5c2e9930c22561b7a14.png)

是变量，但是只绑一次

![](img/1f489cc6e5e91707327d15b8f9f91cf4.png)

> let声明不变类——只被赋一次值

> var声明可以变化的

+   我们的成长路径，思维惯性是变量就是可以变的，特别的用const

+   而swift是反着，先let（不变），再var（变化）

+   我们会发现很多变量确实是**不会变化的**

+   设计很巧妙

+   一个变量被反复赋值，越有可能出现更多问题

![](img/bf8b43116b050f1eb5cd4ee28ee7f0ae.png)

![](img/6b1cdee956f0df6e8041b41a90b3338d.png)

加入操作，如果一个操作是正交的，那么里面所有的类型都能做所有的操作

![](img/00cb45e2dce3c8331d7be782789c2fca.png)

## 6.2 递归函数的系统PCF

![](img/84c01e6222726622bf0ceb639c7e190b.png)

[10 递归系统PCF(1).pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnBG40tP7pnR25402N0BEkYo)

![](img/08cc41a2b70547ac6fe3d63d45a3e160.png)

![](img/5cae1e34da46fdee40bdded8ae1cedc1.png)

表达式

函数

![](img/7aba465719c21545fb41c8b230bf95e1.png)

![](img/399821ec98259185bef15491a7b61717.png)

x+1是一起的

![](img/4dbbcc169258feeffcbbde5982692b29.png)

最里面是x，中间是g，外面是f

![](img/f284c48dce26ce06e2f5b69f71de2c30.png)

+

+

![](img/8a06ebc246604d772d1271ccb286ecb8.png)

![](img/40f77b1e03552eb4c45455dcc67abfdb.png)

![](img/631f79fa95a2337f6f3be42f81f7af9d.png)

并非语法严格定义，是语法糖

![](img/9380e4be0c360fd4bc970f93fdfb228e.png)

答案12

![](img/d28a73150cdda3cf520a0f45fda4d03b.png)

![](img/a2285dd646c4a2cde1198024721b0dbd.png)

![](img/1a9543990949c42c0fe53b905c59f04a.png)

![](img/0c4079554c8622f1139739b4dee0396e.png)

![](img/9587e380b6227c30d82a893488d5b794.png)

![](img/a6f4398d8722ed9cdb8c3743d3780f11.png)

![](img/6207cf960ed4b3fc3326a85a628afcf7.png)

![](img/a7bebfc6cf28581040a7fa0d6f2ff1a1.png)

![](img/8975ca3edcea2d3d285ae4b0cd62eba5.png)

![](img/c7ec4e1089e93b0b3e2326c994a95e1a.png)

![](img/c20838e575190540d6be247278ae0721.png)

![](img/f078feab2330417497563649534b1b88.png)

![](img/70c46f1c812cbd69e4c957ed581b3607.png)

> 最后一行是fix，打错了

![](img/e2c6f1d8601cd459a84fb67e3711ee0b.png)

![](img/971c00fff91c8ef3778e53df228f50ee.png)

![](img/ba4c451c4814bd0b14d39f1b62f125ea.png)

![](img/2fd8746f04a773323b393d320a2529c0.png)

![](img/65fb4d6ccc138f95b2ba6d50d8140829.png)

![](img/089e70f06bc8647288c57e07c7073e2c.png)

![](img/a61a58e98b70e21b2e757fb41f4ce69e.png)

![](img/8686588b6988bfe3e8c9563a356f6d80.png)

+   3+4->7 是规约

+   7-> 3+4不是规约

+   5+2多步可以规约到5+2，但是不能单步到5+2

![](img/0c70dd120b459685e14ce2aaa9b0dffa.png)

![](img/2bcfffa5fbdb3bf2d0f6d1577d2394c7.png)

![](img/e56323237bea2d169af380feb28060a7.png)

大多数语言都是不确定计算顺序的

并行

然而很多语言存在副作用

+   a++ + ++a

![](img/8126dadbcd81642cad36519be06cb48d.png)

+   没有停机条件

+   最外面是常量，可以停机，不需要做内部计算