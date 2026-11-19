
# [Lec10] uArch Fundamentals & Design

> 来源：[https://learning.feishu.cn/docx/WAITdtTByoim9PxOG9VcWdy4noh](https://learning.feishu.cn/docx/WAITdtTByoim9PxOG9VcWdy4noh)

https://www.bilibili.com/video/BV1e6421G7mz?p=13

Lecture 10: Microarchitecture Fundamentals and Design

Onur Mutlu, ETH Zürich

Spring 2023

视频中间有 10min 的 break 休息，从 1:06 分开始。

# Summary

![](img/7c2777594b3053dfd2de1e2a3ea84796.png)

1. Architecture = ISA + uArch。

1. ISA 定义了一个有限状态机。

1. ISA 的上面是 ABI，一种 calling convention。约定了 `register` 的用途等，比如，`stack` / `frame pointer`。

ISA 和 uArch 与 ABI 之间的一层 API 抽象。相对更稳定。演进慢。

uArch 是 ISA 的 implementation。一般比 ISA 变化更快。

![](img/3a02a95f6f424848861cc2d495239ae6.png)

ISA 还都是冯诺依曼架构，

但底层的 micro arch 实现就很丰富了。

Micro Arch 可以用 dataflow 乱序执行，只要给 programmer 可感知的与 sequential ISA 一样就可以了。

也就是，micro-arch 层面可以乱序执行，基本是，现在的 HPC 机器都是这么设计的。

![](img/1caeaf3c64b29af9a2e344591f44b059.png)

# ISA v.s. uArch

![](img/4ca32c0bbacb4a2b3d708f9ecae51738.png)

uArch 具体定义些什么？

![](img/9142fd88430b2cfeae6670ebcb8efa89.png)

练习题

![](img/bee6013eeb12f499a6ede3ab20ce23d7.png)

答案（programmer 需要知道的，就是 ISA，否则就是 uArch）：

1. ISA

1. uArch

1. ISA

1. uArch

1. uArch

1. uArch （pipeline instructions，虽然程序员知道，但根本没有 ISA instruction 去控制）

1. ISA。有 ISA 在操作 `PC`，所以，是 ISA 的一部分。实际上，每个 instruction 都在操作（递增） `PC`。

# Design Point - tradeoffs

![](img/eb14e6cc99d5bd9ec133b133a6fc6262.png)

![](img/bf5592b6fced1acc0aeab174924dafd1.png)

未来不可知，会有新需求

![](img/533dd37449b3f875646f6fb3e13f81a6.png)

未来的需求不是固定的

![](img/e86e39804024bd849f2ff9843dd296aa.png)

# uArch Basics

## 数学化

uArch

=> 实现一个 ISA？

=> 实现 1 个 满足 ISA interface 的 system

=> system 可以是 纯 hardware， 或 hardware + software。

此处，指，纯 hardware 实现 的 ISA。

![](img/5cb763ee15e41cc9a91874c17d6e37df.png)

process an instruction，就是在 instruction space 做状态转移的状态机器。

![](img/4c537ccb841c5902e51759707c404c56.png)

## Von Neumann Arch

Von Neumann 架构的 2 个印记：

1. stored program: instructions stored in a linear memory array.

1. sequential instruction processing

ISA 还都是冯诺依曼架构，

但底层的 micro arch 实现就很丰富了。

![](img/8fcd372aef2b310373a39c16280eef20.png)

![](img/e8b487e22cbcc574ccb5a747bef209fa.png)

![](img/18a37c87576f9410969a0a5e91f078b2.png)

## 反思 Von Neumann Arch

Von Neumann 架构虽不是唯一，但是当前的绝对主流。

![](img/7fe1d8645c8198183283a23abe45028a.png)

unified memory 的主要问题：非常依赖稀碎的 control signal。

需要频繁的从 fetched instruction 中 decode 出来。

![](img/f8b58b521769ef2c0c8cec562766f48f.png)

## Process Instruction Step

就是一个有限状态机

1. ISA 定义了状态 & 有限状态机

1. uArch 定义了状态转移函数。转移的速度，有快有慢（speed of instruction execution）

![](img/cd783a8f4e2391f4050f364ff6bf001e.png)

## single/multi-cycle machine

![](img/066f3c1613fc610c6df8a6a6b3eb4c41.png)

![](img/a80002a6c7817cb1f3a8227bf2fd69bf.png)

1. clock cycle time 是 combinational logic 的 critical path

1. critical path 就是 longest instruction to execute。

![](img/a0f7b2fb91d933529beab113b67bb37f.png)

6 个 stage 的 instruction processing

![](img/4258e2315e2ee1d320b74211a9732602.png)

![](img/2f8f60bffe0ca91a21e574c6bd80e5dc.png)

## View: datapath + control

datapath 是重要概念，除了 control logic 之外的 compute 部分，都是 datapath。

可以理解为，signal 的传输路径。

![](img/461c487aa20d8fb6e9113ed20357fd6a.png)

空心箭头是 control logic。

实心箭头是 datapath。

![](img/9c154fd271fbadd0b9f34389b156e3e8.png)

single cycle machine, control 和 data signal 都在同一个 cycle 内串行，无法 overlap。

multi-cycle machine，control signal 可以在前一个 cycle 内执行，可以被 overlap。

![](img/38f07b1ccc5dcdb6a5f24140ef5fe448.png)

常见的实现思路

![](img/5a94f23a2be939f304b77c432b0d4d06.png)

## Performance Analysis

![](img/94e7e77e8a2bd043dbae404dbf76ba31.png)

# Single-Cycle uArch

from the ground up

![](img/89fc3695940b2f74997f7eec2f107f99.png)

## State Elements

黄色的 control signal。

其他的是 datapath。

![](img/6f193129079f51480947a0ed2802bfef.png)

![](img/57bd4f3e852e747cd85342b641b76dde.png)

fast memory & sync write

![](img/694ee4d1d5d0c7a30461f9ce80487974.png)

## datapath - Instruction Processing

这是 datapath，MIPS 的 5 个 stage

![](img/cc0be612317a9ab2c5d6ceffa813ae71.png)

## Control Logic

![](img/a5468294e0b76b5b7cc3a39871250f39.png)

![](img/1a9031a86717933abc82fbfd56533b8a.png)

# ISA 设计 & 电路实现

说到 ISA 实现，可以认为，这也是 uArch 的内容。

## Arithmetic & logic instructions

数学和逻辑运算，也就是 ALU 做的。

### `R-Type` ALU instructions

`register = register + register`

![](img/3465b9ba1bb921ee7b399d378ee8bb79.png)

黄色的 control signal。此处主要还是 datapath。

![](img/c73744991c4b9c45339fa614f335f104.png)

![](img/282c88f15c0e763ace5524506a6a6159.png)

### `I-Type` ALU instructions

`register = register + immediate`

![](img/4674f1a332c2837941949fb6958c6633.png)

### combine `R-` & `I-Type`

加了 2 个 multiplexer，图中标注 `ALUSrc` 的部分。

![](img/88eabc714fc068aa05e528b674ea9de9.png)

![](img/484412db9edf4dbc03816cc06781c23f.png)

## Data Movement Instructions

### Load Instructions

与 I-Type ALU instruction 非常像。dataflow 也很像。

![](img/9ddd99a6b6ae658cee86ef5cbc57c4e3.png)

multiplexer

![](img/389e54cd08af4bfff18526a76cbd3132.png)

### Store Instructions

![](img/e39d1feaf428d7c5918acbbc5ac3efc1.png)

![](img/d5cc18d7b61cb1d0039fd567e5310995.png)

### Load-Store Datapath

用 multiplexer 把 load/store datapath 统一起来。

![](img/65ff24bc97ba131403616a87ce8e4107.png)

## Datapath for Non-Control-Flow Insts.

与 load-store 相比，最下面多了一个 multiplexer，已用红色圈出。

![](img/895b8cecff19108c5132f62977e559b2.png)

## Control Flow Instruction

### Jump Instructions

jump 指令的立即数，一般是相对 `PC` 的 address offset。

需要一点计算，做 `(PC+4) + immediate`。但非常简单，基本就是 bit slice。

设计的首要原则：与 jump 无关的 datapath，不要去破坏。所以，无关的控制信号，输入都是 0.

现代处理器，为了节能，可以关掉一部分电路，就不需要维持时钟甚至不用供电。

这是另一种 control signal，此处不研究。

注意，`concat` 的另一个 `operand` 是 `PC+4`，不是 `PC`。所以，要用红色的线。

![](img/ccd3cf8b8262f30a3d6fabe7a93e15d1.png)

### Other Jumps in MIPS

![](img/6855c37fa3ff3d9c14a38303fbf9959a.png)

![](img/688d6c1289b8e3ada632bb4055fdcfd5.png)

### Conditonal Branch Instructions

![](img/52c658664893437667ae5044dcc6e258.png)

![](img/c0d143e02c764b97f541d10e24c0db63.png)

## Putting It All Together

![](img/88238859c221c8485b1614c6ab492fb4.png)

# Control Logic

## Single-bit Controls Signals

主要讨论 logic to generate control signals。

就是 instruction bytes 的 combinational function。

`MEM[PC]` 表示 instruction bytes。

![](img/10a4003987e639c419b8478de0478089.png)

![](img/b6627c1639d7e3f65d6330ead25b866e.png)

![](img/64be8248fb808080441a57831307283a.png)

![](img/62ed698ac84f9b390d7af5b376819294.png)

## 指令 ALU

![](img/11b43a773fca5c5772cd50dc99dfb65d.png)

![](img/8b4cff0198914cb487ba385d645b73af.png)

![](img/5401fc9d6fbd3da73483aa9bb56494b8.png)

![](img/b4546400227c51a27fea285c43e31877.png)

![](img/f4e11d0b9168182e1fb1791366509880.png)

![](img/8219d6933cdb3f7a96ef4fbd1ff06d30.png)

![](img/0164fc3601746fac7d516bcad977461c.png)

## Control Box

software 实现，就是一个 lookup table。

![](img/9a52cd01c5d5ebacdb801d1c528b6405.png)

![](img/e6bd2a95b0b0243923ab3a14dc9d8a7a.png)

# Another Single-Cycle MIPS Processor

![](img/d0c9aa54e00f7ec4d8048c600ed56e5c.png)

![](img/f4b04df51ec68e3cf26dc6ce4f62bfdc.png)

![](img/8434e86ac313d313e0aec9b1f8475962.png)

![](img/42c0465c9ef639b6fcfd8c481727f053.png)

![](img/b9e8d05e5556c2ed8a9579e0dfe313e6.png)

![](img/df75dcf6d793282f59faa220d8f48bc2.png)

![](img/014b210b365ef3ab95ab7234b575b40d.png)

![](img/0700b30afd40085b308c7623e7c12d66.png)

![](img/869e766861e08d01954154e0289c3b88.png)

![](img/1d63c4e8c1a7d2181fc7c99dc745f518.png)

![](img/b9d374a637fbd862d32a634031599c4d.png)

![](img/3f6901ff6aeab7de6b44e925c34e8d48.png)

# Backup Slides

![](img/c9b6902faa1c4e81e9cf0b321734ead9.png)

![](img/5f7f244e7dee46d7f3e2dae4848511cd.png)

![](img/b24ec18ff2d4fd7b8134a64757f9bf0e.png)

![](img/42e77d400079a4e8a1fb730eb4a39919.png)

![](img/719ceff0d0949232db9792d2843b4f1a.png)

![](img/f625247e1f80e84afb9949446897cbf5.png)

![](img/f22de449d85ed0ef3c9dc25d89e33c75.png)

![](img/938db54be46189f6aecbd9dbfe5287f3.png)

![](img/ef8ed80c6a5e2d4c73cc118ad368f3f9.png)

![](img/2ba16320c248b74ae9c629b25dc73e92.png)

![](img/437cd8655d0141936f059473fb4c4867.png)



![](img/a079ba4afc99bc6eba924aadf21f9e77.png)
![](img/7542928e0290dc42f8b219cc0faa0970.png)
![](img/52afc8a7647eeb96d383230b387ea690.png)
![](img/d83131cda3e668255bff58493d27985c.png)
![](img/92c03a43728943cf86eca0eaf8177a4e.png)
![](img/0129a38a6df2fe56d868fbfad81febcb.png)
![](img/6e4a9518522c8c857cf176177b468fba.png)
![](img/102970c03471ef1fd769fd1bfbac340f.png)
![](img/794323a192c99ac15d66092cc57fb380.png)
![](img/24728a2ec38f13ffe992d0be03ce19cf.png)
![](img/907ed68a04b989e4ae6ea783a78bbc87.png)
![](img/b77ed916eea0d9b17ff5015ba0e9d2a5.png)
![](img/083b94df06cbcc2c3568df235e512f3f.png)
![](img/cf2de3eaaf40503d9d668e499f4142ba.png)
![](img/1160435d2994ff8bf7c4203b02235bc9.png)
![](img/dde906c21c9194612d0f1e3815f7622c.png)
![](img/f27792343c2e4489bfccc08b456620c2.png)
![](img/323bf0414a8f570b230e8af71b51fcf0.png)
![](img/2526bbe71ae329d76452375acd3a61d6.png)
![](img/c0bbc127af91749677dfa8ac120171eb.png)
![](img/7bfd785ddefe473aadd9685b6644e023.png)
![](img/683baa99eca16b1d587abb86a6b907d8.png)
![](img/3f1fc7d9dca85bd2e5814d3084d494e4.png)
![](img/3fd1121f73da49b6707c2a3ad54b2211.png)
![](img/111ae74ee5ae28982fccaad432f52d01.png)
![](img/c16521891caac68e427367333e55ca7d.png)
![](img/8147a0bf553684367db636cde1e3e839.png)
![](img/e470374f43650d7f2a110a1bea4d3688.png)
![](img/fc17f871648a697cfd293a19ea8dde1c.png)
![](img/69001d9c27d07a3574581c751db62ddc.png)
![](img/5fb4aec57a1f9564da1316e86fcf04e7.png)
![](img/8f2465c9bfc7cb72a99b8c7a619a6c99.png)
![](img/9d42a7a0afbb64fe0fb39977be8e5df2.png)
![](img/426d9c83629d562668b48dbf287d35eb.png)
![](img/4738d0e102ddc164d7d113725acbb5eb.png)
![](img/cccd037fc623cbd3a0b43c4420457fdb.png)
![](img/cd95d43079016f504303f3a1afba4eb4.png)
![](img/612165b666c1c085e2f7b946b22f9b36.png)
![](img/326d479be20a0c4c9353b2e148baa70c.png)
![](img/a2c8b3710b321eae0d8340137bf04a55.png)
![](img/34b14635acf9cefdbb80c8c55ca23488.png)
![](img/e97e91eb99c2dac62da9b6932eb0628a.png)
![](img/f51f2248432d319b1556b39879799517.png)
![](img/f4e25aafffa6a540a8cd199363f00119.png)
![](img/79e614fcf74f7b48c86a600436fa3374.png)
![](img/5d0032170876fa087abee7fd3b9c6672.png)
![](img/24715d93111460b9b8014d4cde84379a.png)
![](img/ba832c16f4ab0011713e2dae13c52dff.png)
![](img/f0c4dbdb6683b59cc92ec8206bdcb3c3.png)
![](img/991a25a9f4148f2f0f3c8fe3cf27a869.png)
![](img/2095e91bd5e1f6d0cc3504f064759fad.png)
![](img/55953f4b59c94656ce5ad2fcaabc5a0c.png)
![](img/dafb65283a5d80c7276c7da9074e0617.png)
![](img/48dd7852758a8071f7d88aad354abc98.png)
![](img/1d8bc0f0524d82c59d6af55a2464d023.png)
![](img/65ee656ca946cc94bcee653ae5a599e6.png)
![](img/42640210b4dee0cd140b4541ba633d60.png)
![](img/77ef5c161290a3bcef334a144917eb78.png)
![](img/3a63a48cd4f5e276ffed5240c7d58c17.png)
![](img/43fe9243e836d8e9a36bfc7a57f7a1d8.png)
![](img/8c59aaf568ee3428d33092159b5bfda3.png)
![](img/f652de809d34cac1ffa972b96a8523cf.png)
![](img/329967c86a37058cc54cae39ea63c82c.png)
![](img/536574ad640c12b97e2deec2089d21c9.png)
![](img/75a4558a18226d552e65f0bafcba93b6.png)
![](img/1078b74d9d23278c89e4358991b05dbd.png)
![](img/e1aa3a1d888890296573789f84df6f0b.png)
![](img/b7935a550bf1139643c60452e10a946c.png)
![](img/f4ce89fa9755e0daf9c2cff998bebbbf.png)
![](img/da4f1af42bbb0daf8e3721fe0c17ab24.png)
![](img/fd46776c0e08c716b72d5bff559f4fa8.png)
![](img/35e09875f51be76f63c608ca4283fa43.png)
![](img/3cf62464f1b5db6113e715dbc16eaadd.png)
![](img/03e73621186a7b185ce1ad74c1c944c8.png)
![](img/a53366f8d85acbbe7c84000b2a900110.png)
![](img/9cd14dafa13609ad5771244d716d95d9.png)
![](img/396b6c6faab3b8f72390b707987b6b0f.png)
![](img/3b2d51da38edd5188b827f11ec30fb6c.png)
![](img/56f15d8b3e255f652e17960cf5e66c38.png)
![](img/d0f6064229b4645c188b8ef667f6dc48.png)
![](img/20982b55be85fc417208194195f28984.png)
![](img/0c93294a850eefb91774f2d1981403d6.png)
![](img/f7b856ed814232bd90a728b3fa63134b.png)
![](img/8c3fa87f400ec4af75978d6eaab6f9bb.png)
![](img/55d2d987bf50285c539ad5d50e257268.png)
![](img/8f7638c0f99125d8d416709d74775cdc.png)
![](img/f29bf34be943d3ef39559a80155a6b43.png)
![](img/11a6e3da1bb004595de4655b5a5aa854.png)
![](img/460270ed5864255ea04406d7b4dfb6e1.png)
![](img/a59e6ea5e5f0c3a61712c64a2b646e79.png)
![](img/8bd6e7cad967c2d82e35c0dcabaf1de1.png)
![](img/fd895c3ddfae0fa272abda4f7b510152.png)
![](img/e63aeb2106691151bdaed86db7762538.png)
![](img/8bfb38f4d80bf7be9a5e6cad35658227.png)
![](img/b5aceeb35ea1500bbe1fca93ebaad2d5.png)
![](img/bb7165ac05dae840e5942915b4034f34.png)
![](img/41801483ada51ed8c54f9f77d4fc2ba4.png)
![](img/42fec567585ffdd252a9c7f14c5f0cca.png)
![](img/94aba5709cb655d5c38a7687f9e6aada.png)
![](img/a9b55bc433c8aabfd52fcc6c2c80dc4a.png)
![](img/5c806d109b7c80e31fd4076bb34ca322.png)
![](img/3e3adf0eccc7c8ba99ea9e6a9cf925cc.png)
![](img/80cbc4ee9f16c276a541695a155a6e95.png)
