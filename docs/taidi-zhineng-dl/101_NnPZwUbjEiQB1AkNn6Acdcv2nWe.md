# [lec11] Multicycle uArch Design

> 来源：[https://learning.feishu.cn/docx/W6R6dyXz6oKsU6x6vhVcRBcRn6e](https://learning.feishu.cn/docx/W6R6dyXz6oKsU6x6vhVcRBcRn6e)

https://www.bilibili.com/video/BV1e6421G7mz/?p=14

Lecture 11: Multi-Cycle Microarchitecture Design

Onur Mutlu, ETH Zürich

Spring 2023

视频中间有 10min 的 break 休息，从 1:04 分开始

# Summary

设计一个 uArch，就是 2 步：

1.  datapath

1.  Control logic

Control logic 决定了 hardware 运行时的所有状态空间，

1.  用 uISA 来描述，是一个 35bit 的 word。

1.  存在 Control Store 里。

control store 的抽象，让 hardware designer 可以做 uProgramming。

Single-Cycle uArch 一般是 bad idea，因为不同指令的 latency 可能相差很大。也就是长尾问题。

现代处理器，memory 访问非常慢。

导致，single-cycle uArch 的指令长尾问题，更加严重。

如果再有访问 hard disk 的指令呢？

![](img/fae11848d369562428e7a576c2461ebf.png)

single-cycle 需要 2 个 memory (或 port)，3 个加法器 （ALU，PC，Branch address）。

![](img/c7f221c8fa08448f81d9632d18697932.png)

1.  新增了 4 个 register 保存 stage 的中间结果。红色框出。

1.  1 个 memory，1 个 ALU 加法器。绿色框出。

![](img/8f91672bdc2b7d9cd1313fc6bf5e4232.png)

![](img/fcec810d73b52098d76358f232a87565.png)

![](img/fe2d30cc4d2524f556842e8afb28a72e.png)

# Eval Single-Cycle uArch

Matrics:

1.  a good idea / design?

1.  when is this a good design?

1.  when is this a bad design?

1.  how can we design a better uArch

1.  multi-cycle uArch

1.  pipelining

1.  out-of-order execution

好不好，主要取决于不同指令的 latency。

实践上，一般是 bad idea，因为不同指令的 latency 可能相差很大。

也就是长尾问题。

# Performance Analysis

## Modeling

![](img/cf88ffe50e9e0d45823143875b27b578.png)

single cycle uArch：

1.  CPI (cycles per instruction) = 1。

1.  clock cycle time 取决于最慢的 instruction。

6 个 phases，5 级流水。

![](img/79d02acb9867a09dcdb940c48d5f5a10.png)

## 指令 Latency 分析

LW 指令最慢。因为，IF 和 MEM 阶段都访问了 memory，各需要 200ps

![](img/d4d2e7ad1b78649107d38b54b77b28b5.png)

![](img/5b12bbc7d67802d0326bbe89512f6802.png)

![](img/b3a563de89dbb11d8c5b6e94f0057bd2.png)

![](img/dde47bc56d5a3c931e8b22cf9af13ba8.png)

![](img/6c42185d78b31a568d75f7add9b6dfb4.png)

![](img/10182796eb0ed7bc55966f5019f9284f.png)

## Control Logic

First question: can control logic be on the critical path？

single-cycle 设计中，control logic 在 critical path 上，必须先生成 control signal，然后才是 datapath。

## Single-Cycle uArch 分析

现代处理器，memory 访问非常慢。

导致，single-cycle uArch 的指令长尾问题，更加严重。

如果再有访问 hard disk 的指令呢？

因为，一个 cycle 内做完了所有工作，所以，不能重复使用硬件，也没法复用资源。

如果是多 multi-cycle 的，cycle 间就能复用一些硬件和资源。

![](img/d72b2928f55f9b0a6dc1b2299f34237b.png)

# Design Principles

设计原则，

single-cycle uArch 违反了所有的原则。

![](img/286ba975ca99d60df5ed931cbf46f8a3.png)

![](img/83b67199c48cb3af6eecd99c05570eb0.png)

![](img/251c0e07475ca369bc5514d8aea1f2d2.png)

# Multi-Cycle uArch

![](img/fefce5379ded00b1c8c0672d882c23fc.png)

![](img/03db48eb64f053b0c05e6c05537df91c.png)

![](img/78250c40fa55f6858161e9fb79cb1890.png)

![](img/212d15d3167b0c797090bd62257ccd49.png)

# 优缺点

![](img/bc95485663a1579c0303e42b2e45d065.png)

![](img/088106bc43de41e2e60b711858712df8.png)

![](img/c8e0f348334a99149d18c6c60b7bd29e.png)

![](img/7557d5034dbc70dd3992622fc9054532.png)

# Closer Look

what does best mean？

![](img/440c55700ba277b971d5abec36581a80.png)

依旧是有限状态机。

![](img/ab601351a33092fd653e37b4a571395d.png)

![](img/cb56f71c52d718fb0930c043734577d7.png)

![](img/cadfabda1f136375229af85c5f5855d3.png)

single-cycle 需要 2 个 memory (或 port)，3 个加法器 （ALU，PC，Branch address）。

![](img/06c9c1684ea67f245b90baaa02bc8f88.png)

# Datapath

## lw instruction

lw 指令的执行过程

![](img/57a0ce823d3f332cf03aa1dbf3d4e881.png)

fetch 是所有指令都一样的。

![](img/7108db9acee27856261ca120a3c35b74.png)

![](img/e3329ba84e4b4a4f51bfa68b4a5f22a3.png)

![](img/aeee714c97c90afd98a41b58cba27d9b.png)

![](img/7263625d70d85ce7ab502bfe669b23b9.png)

只用 1 个 memory（port）就可以了。single-cycle 需要 2 个。

![](img/e82d44aeff1ad8605d016b56548df34b.png)

![](img/e894b25e69bc52b4e3c345e86a58b44a.png)

只用 1 个 ALU 就行了。single-cycle 需要 3 个。

![](img/2cb6eddf91ba73b1d9073333a1e232ff.png)

## Other instruction

![](img/f6f6e3df92a999e18423de3547683d6f.png)

![](img/e5c018970581b8456d5678870692418e.png)

![](img/9b0eabd44e4e180336eee3b32ffde20e.png)

## Complete Multi-Cycle Processor

1.  新增了 4 个 register 保存 stage 的中间结果。红色框出。

1.  1 个 memory，1 个 ALU 加法器。绿色框出。

![](img/664754e7657121bd2658630e95299dae.png)

# Control Logic

## Control Unit

需要一个 control unit，是 FSM（有限状态机）。

与 single-cycle 的基本一样，增加了少部分 signal。

![](img/20fa9967fb72920ad2f46229111b4828.png)

## 指令 FSM (有限状态机)

![](img/690d554ad7d1460c5aaf91f293eba5c1.png)

![](img/24668ce0e9fdf5cfa23348d10b2efcfa.png)

![](img/2e4c795561dd33262490abb0183cca6c.png)

![](img/88f7c0a419e22a571374f48e81b09e8c.png)

![](img/206e3baf6d9f10725ed00b0010824b10.png)

![](img/55b273fa0d34af8a0e957651b749d421.png)

![](img/81f9ac21cfd6289f4bdf30c63a030351.png)

![](img/687410acdccd0fc0af99d5537ebe9106.png)

![](img/485b0ba1ec1fc71338eb66f80ea42304.png)

## 完整的 multi-cycle FSM

![](img/8130c468972c6a80d9dd8fd86dc90812.png)

![](img/3ef2f92af5b1011600a215b1c9a91515.png)

## 指令扩展

按需增加 control signal 和 datapath 元素即可。

addi 只需要增加 control signal，不用加 datapath。

![](img/d87f94e0cd3bc3e5de55851c7db001ed.png)

![](img/32ff0ad2c1f95732f47b007b8719e33a.png)

![](img/5ce62fac3e795ca355f3089bb8867199.png)

![](img/1bc2b1fcc53325ffc369cc4e55562fd0.png)

# Summary - uArch Review

![](img/87c5f3dcf4d2647c75a80b11d0294be2.png)

微架构

![](img/0fa8ecd8b33254a926f2ff2b93080bc0.png)

MIPS 有限状态机。与 single-cycle 的区别相对大一些。

缺点：

1.  假定了每个 stage 需要 1 个 cycle。但，memory access 可能超过 1 cycle。解决方案，FSM 中，让 state 持续处于 memory state，直到数据 ready。

![](img/5fb25b1b1d143ee8eaa6350a56432579.png)

![](img/bd5a2f2a759f5161dcd3952acebea9a7.png)

memory access 持续 loop 等待，直到 ready。

![](img/f60b951d50f7a3598454cd2c5d790206.png)

# Another multi-cycle Example

what does best mean?

作者：设计非常的 clean。

对现代处理器的设计，很有参考价值。

![](img/964e0d7c89c965adfd28aed174b3efb7.png)

## 形式化建模

在 ISA 之下，给 uArch 定义 Microinstruction。

1.  control signal 控制 machine 状态，即，Microinstruction。

1.  microsquencing：用 Microinstructions 控制有限状态机的状态序列转移。

1.  Control Store，存储所有 machine 的所有 possible state。可以像存储代码一样，把 FSM 的所有 Microinstruction 指令都存在 memory 里面。

1.  microsequencer 就像 PC 一样，决定下个 clock cycle 执行哪条 Microinstruction

对标于 ISA 和 uArch 的概念：

1.  ISA -> Microinstruction。实际是 ISA 的所有 control signal

1.  实现：

1.  有限状态机：用 microsquencing 描述 state sequence。

1.  所有 possible state 在 Control Store 里。

1.  microsequencer  决定下一个 state 去哪里。

![](img/64baa8ac91e9d3ef35e4b7d0a800c08f.png)

## Control Store

一个 control structure 的例子

Control store 公式说明：

1.  pipeline 共 6 个 stage（状态），每个状态  0/1 共 2 个选项。

1.  每个 microinstruction 位宽 35 bit。

![](img/f0fe8a1425bd761ca75e963381c400a5.png)

![](img/c945f0db2576c9ad923626d7da21ece5.png)

control signal 是 microinstruction。

将 microinstruction 应用于 datapath 和 control logic。

在统一 cycle 内同时执行，从而确定：

1.  当前 cycle 的 datapath 应该做什么。（也就是 ISA 的定义？）

1.  下一个 cycle 的 state 是什么（microinstruction）

![](img/3864f629f8a116f6fb30c9c0e2b2719b.png)

control structure，有 3 个东西，比前面的 4 个 少了 microsequencing（这个不算 control 的东西吧）

![](img/15053d4c943bb1ea22432003ec6b65fb.png)

![](img/53f62ff5c48f9cc94b925597ec02fc57.png)

![](img/46297d0ce6097f40049a0b4742779014.png)

![](img/8a4b91bccb0c3b908693457861ee07aa.png)

![](img/abba607f6836f710b30b200ce677ab1b.png)

![](img/041acf0569003a4fc402a8b44c2d749d.png)

## the Power of Abstraction

control store 的抽象，让 hardware designer 可以做 uProgramming

![](img/f2cd5fe10d263ab79277d8e086efd10e.png)

## uProgramming & ISA translator

![](img/ce161b1231204c3252bb7b044bc64e51.png)

![](img/fc37b906ac13b36037c3e415fb35d148.png)

![](img/f108c494ae39ff23ee6b52a0cd551472.png)

![](img/66a786f61aa18850a8d465934c540c74.png)

![](img/1df11e581eb65cde41f56b3d9bb05540.png)

![](img/0cd4bfd7af397edd23a3f942f3b90b15.png)

# Backup Slides

![](img/1be9a042f7c873791f173d14f3b1ca92.png)

![](img/1755a5899a20e995aa09aeca77c37ccf.png)

![](img/75439d1bbc9ae65bfd376a8a6cc99921.png)

![](img/6264596c84abc5da2cad31e4c59e79b8.png)

![](img/286240d11bbe9c96deb5c498209cd390.png)

![](img/88c133087abba8f074ad2f14a6b512b3.png)

![](img/1dfa116936672c75114baa9eb9881ec8.png)

![](img/03b30238b2eacdceda5a10d054737986.png)

![](img/1f4e51a53ed71f207dc4444968b54a8c.png)

![](img/de572ab07d80425b01b47c1b47efd6b6.png)

![](img/038f6fb4564a8e9a6f7de1139e22df7f.png)

![](img/303ee687200e250656a2fcae7a0dd177.png)

![](img/3e42d8588b1c4df47cb6e4430913484e.png)

![](img/5fc1e13d28c39b00baeaebe48d1383d5.png)

![](img/c47039b8a72553b20dd1be22b31d6520.png)

![](img/a4daccf3f8caf9124d28d57d2f351c51.png)

![](img/44ae17036cb9beb2c0655d2b1fd06349.png)

![](img/c9303fddb40b4b0446c2ccea8e3568ef.png)

![](img/3324810c17e20378176182ccf5dcd763.png)

![](img/da5e3ddefce18dc979dbc039ac8e3f2c.png)

![](img/e02beda8e1b7ade0f7695960595e289c.png)

![](img/f110c3c71a985dc426d3e83ff3d8d919.png)

![](img/ffb5027b236961a7c5a842769dc7b011.png)

![](img/d0c91ad73c86aa6bffed66f0db46cec2.png)

![](img/2a2c8abf7983c4dbec5d9f3b57aaeaeb.png)

![](img/e663487a6abe19d3507d6e830e00c6f2.png)

![](img/4c97cd4c7fbbf52148a1a0ed496e6935.png)

![](img/eb0d7ce7b896f39085b109574d337f1c.png)

![](img/fbbe7b4ccaaaf57f7e1fcb05b61d88a2.png)

![](img/a23fb04b3871e4c53fcbe4f44c4e9374.png)

![](img/bcd7f1c723f6b2fb940c4b49488a1c19.png)

![](img/cd39738b6ec41c0e3fbabebb2c188847.png)

![](img/be285f86b88fd8a5f77e5e7ff8d76300.png)

![](img/70179bd155541bfd045be9b7abe86185.png)

![](img/03360b63897e2fe0f24e36926ca07e37.png)

![](img/8b515dd1a0fe743d9ccc23c90cd2d380.png)

![](img/11a392fcaf8961108b8eaa6ac827a29c.png)

![](img/3bab74196b0144016809768ac671c11a.png)

![](img/967b475cd7561dfb4bd16ccf65e78275.png)

![](img/ab7d66f935e9e007d5e0df97ae14fc51.png)

![](img/6f976738502664d17f53bb7427e58f39.png)

![](img/3a348f41c749d5d479a470e82f096977.png)

![](img/5438fe658502208043ae60f71da05cc6.png)

![](img/3bb58b2d57116ef1bf3d5202ba5295a1.png)

![](img/399bf2b65096de38ec3f2aeb2797efcc.png)

![](img/db264dbca89f4e1dbfab0528ca9ae644.png)

![](img/d611a5d55fd46fd5293af8fc45aa7380.png)

![](img/c0808d22dc3402b109ec0a251f9807c3.png)

![](img/1d156338033a82789e3f0597fc7a8ef9.png)

![](img/ea529a3c92fbc1a6ab52462327c08efb.png)

![](img/4ff0b49575ba28b9c507e641c2f192c4.png)

![](img/9e27c2e574a61a748da48acb4cd51c9f.png)

![](img/0109b590b37017ff4657701b1ea8e9d8.png)

![](img/6b1bd2faa87ce786a30e52be7ed86efd.png)

![](img/e4c82c239acaca9fc6d5ddcdd43147f6.png)

![](img/80ccb932ffcfd4c8c5e0d89b487953d3.png)

![](img/62ecbc5e8240f962ca309a706363beca.png)

![](img/721a59bf0d3d732f8ac2cf2267667dd5.png)

![](img/3597c8328e3c80801b4de1a608d10831.png)

![](img/bef90ca40f5b605a969d3fc26202feda.png)

![](img/a255b150eedd1a8fe4796c8f87e5313f.png)

![](img/0ab4dc4ab0e8aba4e992946c63c7e96a.png)

![](img/e30190f1cc62b0cb7fd3b090e0df8239.png)

![](img/ffc5c5f9b433d219d3d810fca7278860.png)

![](img/e852e767971f32d61db904ea615fccd5.png)

![](img/0395640bdd836658d6903294168927cd.png)

![](img/a3ae73c3b298419077ae4345a12dd012.png)

![](img/7f08fc465c3cde9701a02cb0ea18dd25.png)

![](img/6b575107efc35ebafe17419a4e14d914.png)

![](img/c43683285fc965123b0b04d406427337.png)

![](img/d89ef66287ccbb323d505ec58680199b.png)

![](img/882072b87b531a2241a6fa04457f9085.png)

![](img/64e6380ee7286f280ae1579b60607f3a.png)

![](img/30620b73ad566767110f3e5f515fa823.png)

![](img/901cc5384c3e3d1e7833a2a597e39a63.png)

![](img/dfe9b1aaa31815d1321c493af143a629.png)

![](img/ae7a97dba3c204727484afc8e08e9737.png)

![](img/fee8f59e663ec300ea94cc151b28c158.png)

![](img/32ae23ab5d7598ffc65fb39c30b9d5f1.png)

![](img/24a74599a5a0983199eb94db400ce975.png)

![](img/842df976c7b0db47467d76525ad8762f.png)

![](img/87f62265930c12fed13bd00be6bcf01a.png)

![](img/f62304bbd7a4e517b4109cadcb6ce384.png)

![](img/989b54190331313f50d60473de76746f.png)