
# 系统盘点StarkNet技术优越性

> 来源：https://docs.feishu.cn/docx/DOHmdOc9RoKi9zxADmbcjKaFnGd

> 🔗 原文链接：https://mirror.xyz/zhunianpan.eth/GQjhODNKUtWEM5aHuuWUbmm0iehaoRTbbcZDxpQ4UL0...

## TL;DR

1. 🛒ZKR 设计：汇总经济学使得交易越多，Gas 越低；ZK 证明-验证的方法取代了节点执行全部交易以验证正确性的方法，大大降低链上计算与存储数据量，提高执行效率；工作流优越性需要等待其他 zkEVM/zkVM 确定设计后再对比优越性；原生账户抽象可以“消灭”助记词，已经实现手机硬钱包，并为更加易用、安全、低成本的链上可编程奠定基础。

2. 🏕EVM 兼容：Solidity-Cairo 转译器 Warp 使得 StarkNet 语言级兼容 EVM；用 Cairo 编写的 zkEVM Kakarot 使得 StarkNet 字节码兼容 EVM。

3. 🧬STARK 证明系统：纯粹依赖哈希和信息论，不依赖可信设置，抗量子攻击，比 SNARK 更安全；证明生成速度具备线性扩展性，验证时间和证明大小具备对数扩展性；缺点在于生成的证明尺寸更大；递归证明可将多个证明验证后合并，理论合并量上限达到 6000 万，也为 L3 奠定技术基础；STARK 友好的哈希函数如 `Rescue hash function` 等相比以太坊 `keccak256` 有更高执行效率。

4. 🐺Cairo 虚拟机与语言：Cairo VM 是一个 STARK 友好的、图灵完备的 CPU 架构；CPU 架构灵活性高，可通过软件编程无限接近 ASIC 性能，理论上可以复刻任意虚拟机；Cairo 语言基于 Cairo 汇编和 AIR，执行效率非常高，但开发体验类似 Rust 和 C 语言，指针+内存大套餐🥙使得学习难度中上；开发者可使用证明者一侧的“提示”(`hints`)来创建快捷方式并启用某些计算，降低了开发难度；Cairo 支持通过字节码哈希加载程序，大大提高链上扩展性和隐私性，成为隐私方案的强力替代。

## 一、🛒ZK-Rollup优越性

在以太坊上，每提交一笔交易都需要所有节点检查、验证并执行交易来验证计算正确性，并将计算后的状态变化在网络中广播。

![](img/c8eb3ad31781fb77bcb4f7bc77fb75d7.png)

https://ethereum.org/zh/developers/docs/evm/

StarkNet 仅在链下执行计算并生成一个 ZK 证明，然后在链上验证该证明的正确性，最后 把多个 L2 交易打包为以太坊上的一笔交易。因此，StarkNet 上发生的交易成本可以被同一打包批次的其他交易所均摊，就像拼车（🚌拼多多）一样，交易越多，成本越低。

除此之外，相比以太坊让每个节点完整执行交易的方法，StarkNet 为交易生成 ZK 证明的方法可以大大提高网络运行速度、减少链上通信量、增加网络吞吐，因此 StarkNet 相比以太坊具有更高 TPS 和更低 Gas。

简而言之，将验证计算正确性比喻为老师需要检查同学们是不是掌握了知识。 以太坊的方法是检查每个同学是否能背诵整本教科书，而 StarkNet 的方法是让同学们做卷子。后者的效率更高，成本更低，但仍然保证安全。

### ⚙StarkNet工作流程

与大部分 ZKR/ZK 项目一样，StarkNet 存在一类称之为证明者的角色来生成证明。验证者作为 L1（以太坊） 上的合约来验证证明，验证者合约代码都是相同的。

具体而言，StarkNet 有五个组成部分，分别是在 StarkNet 上的 Prover（证明者）、Sequencer（排序器）和全节点，以及部署在以太坊上的验证者（Verifier）和核心状态合约（`StarkNet Core`）。

系统架构如下图：

![](img/98d1c45daf722dca8bcf113111f65898.png)

https://david-barreto.com/starknets-architecture-review/#more-4602

StarkNet 的工作流程如下：

1. 当我们在 StarkNet 上发起一个交易，一个链下服务器——排序器将会接收、排序、验证，并将它们打包到区块。执行交易，然后状态转换发送给 `StarkNet Core` 合约；

2. 证明者将为交易生成证明，并发送给以太坊的验证者合约；

3. 验证者将验证结果发送到以太坊上的 `StarkNet Core` 合约，并从 `StarkNet Core` 合约触发一组新的以太坊交易，以更新链上的全局状态以进行记录保存。状态事务作为 `calldata` （`EIP-4844` 后为 `Blob`）来发送，以节省 L1 事务 gas。这些 `metadata` 可被 StarkNet 全节点解密。

全节点基本发挥存储功能。全节点存储状态改变、元数据、证明以及记录在 StarkNet 中的已被执行的所有事务，并跟踪系统的当前全局状态。在有必要的时候，全节点将解密 `metadata` 来重构 StarkNet 的历史。

工作流程方面的优越性结论可能需要等待其他 ZKR/Validity Rollup 工作流基本确定后，通过对比得出。

### 原生账户抽象

不同于以太坊 EOA+CA 的双账户设计，StarkNet 实现了原生账户抽象，只有一种账户设计。

这对合约钱包以及更加低成本、安全、易开发的链上可编程打开了大门，这可以推动 ECDSA 以外的签名验证方式、多签管理、社会恢复、聚合签名、交易限制、隐私保护、gas 优化、gas 代付、异币 gas、应用聚合、自动收益等各个方面的发展。

下图为 StarkNet 交易流程图（仍然借鉴了 `4337` 的设计思路）：

![](img/e6b85bebfb5c09d8a56a7aa5ee5a55f6.png)

https://community.starknet.io/t/starknet-account-abstraction-model-part-1/781

而且 StarkNet 已经实现了“手机硬钱包”。

[我总结了 StarkNet 的开发倡导者 @barretodavid](https://twitter.com/barretodavid/status/1563584823884935168)，提到的在 StarkNet 上 实现手机硬钱包的思路。

1. 以太坊上的 EOA 仅支持 Secp256k1 椭圆曲线上的签名方案 ECDSA

2. 大部分的智能手机都不支持以太坊的椭圆曲线。

3. 所以移动钱包需要依靠软件签署交易，移动钱包因此是热钱包。

4. StarkNet 原生账户抽象，支持多种椭圆曲线，签名验证高度可编程，因此基于 StarkNet/Cairo 的手机钱包完全可以变成硬钱包。

![](img/873d39b572fd24d2f1a06bc61c2f3147.png)

[StarkNet](https://twitter.com/briqNFT) 上 [原子化 NFT 协议](https://twitter.com/sylvechv) [Briq 的开发者](https://hackmd.io/@s0lness/BJUb16Yo9) @sylvechv 在博客中也写到，在实现了原生账户抽象的 StarkNet（编程语言为 Cairo）上，可以授权一个 `mobile_key` 签署一定数额的交易，和/或只为某些合同，和/或只为某些功能（例如没有 `transfer()` 而只有 `play()`）。用户甚至可以在一定时间后撤销它。

在 StarkNet 中的合约钱包，授权手机签名的流程类似于

1. 在手机上生成 `pub/priv` 密钥

2. 使用“管理员密钥”在智能钱包上授权 `pubkey`

3. Cairo 已经有一个 `nistp256`（用于智能手机 `Secure Enlave`）的实现（我们一起学硬钱包叫(●'◡'●)）。

## 二、🏕EVM兼容

StarkNet 网络本身不兼容 EVM，而设计了另一套 ZK 友好的 Cairo VM。

StarkNet 没有针对以太坊操作码做 ZK 电路，而是自己做了一套更加 ZK 友好的汇编语言、AIR（代数中间表示）以及高级语言 Cairo。

![](img/630c64ca94bbbc4f9803cc1f5beb1377.png)

https://www.youtube.com/watch?v=SEp5SFaYQHY&t=6s

不兼容 EVM 的弊端在于无法继承以太坊代码和工具链，因此，以太坊应用生态没有大规模移植到 StarkNet 的基础。且 Cairo 语言对开发者存在一定学习门槛，Cairo 语言和 STARK 证明系统相关工具链和库也处于起步阶段。

但独立设计 VM 的好处在于，StarkNet 的 Cairo VM 更 ZK 友好，电路执行效率更高（代码更少），在未来将表现为 TPS 更高，Gas 更低，抛弃 EVM 设计后还能实现许多以太坊不能完成的应用创新，如原生账户抽象的合约钱包。

StarkNet 属于 Vitalik 定义的 `type 4` 级别——语言兼容的 zkEVM（StarkNet由于定制了虚拟机严格来讲属于zkVM）。

![](img/c8c2afb655a5af4ccb30a3c5c60ad3ae.png)

https://vitalik.eth.limo/general/2022/08/04/zkevm.html

尽管 StarkNet 本身不兼容 EVM，但 StarkNet 仍然可以通过其他方式兼容以太坊。

1. [Warp](https://github.com/NethermindEth/warp)：将 Solidity 转译为 Cairo 语言的转译器

![](img/f3d46456ee5ecc9d0e6585dd9c862399.png)

Warp 是一个 Solidity-Cairo 转译器，目前已经由以太坊著名基础设施团队 Nethermind 开发完成。Warp 可以把 Solidity 代码转译为 Cairo，但转译后的 Cairo 程序往往需要修改并增添 Cairo 特性（如调用内置函数，优化内存等）才能最大化执行效率。

2. [Kakarot](https://twitter.com/KakarotZkEvm)：一个用 Cairo 语言编写的 zkEVM

![](img/e3a3596900bbda8a693f164e2644aea0.png)

Kakarot 是一个用 Cairo 写的智能合约，目前部署在 Starknet（goerli 测试网）上，字节码等效 EVM。目前处于测试阶段。以太坊应用可以通过部署到 Kakarot 的方式移植到 StarkNet。

+ ✅Kakarot 可以：(a) 执行任意 EVM 字节码，(b) 按原样部署 EVM 智能合约，(c) 调用 Kakarot 部署的 EVM 智能合约的功能（视图和写入方法）。

+ ✅Kakarot 是一个 EVM 字节码解释器。

![](img/78d7c42dc960c9352474be95c8a8d2c4.png)

支持 EVM 约 90% 的操作码：https://github.com/sayajin-labs/kakarot

[此前以太坊联创 Vitalik](https://twitter.com/StarkWareLtd/status/1585194802593038337) 和 StarkWare 联创 Eli Ben-Sasson 参与了 Kakarot 举办的 Twitter Space。

## 三、🧬STARK证明系统

### 3.1 简介

目前有许多不同的证明系统（[生成和验证证明](https://eprint.iacr.org/2016/260.pdf)），如 [`Halo`](https://github.com/huyuguang/zkpblog/blob/master/groth09.md)、`PLONK`、`Groth16`、`Groth09`、`Marlin`、`Plonky2` 等，它们都属于 SNARK 证明系统。证明系统存在一个证明者生成证明，一个验证者验证证明。而不同的 ZK 项目几乎都会使用不同的证明系统，StarkNet 使用的 STARK 某种意义上属于一种特别的 SNARK。

![](img/100f749b21d830434be9245bb4370099.png)

https://medium.com/alliancedao/zkps-in-web-3-now-and-the-future-21b459348f29

SNARK 全称 Succinct Non-interactive Argument of Knowledge（零知识简洁非交互论证），STARK 全称 Scalable Transparent Argument of Knowledge（零知识可扩展透明知识论证）。

STARK 作为一种特殊而创新的 SNARK，S 的含义从简洁（Succinct）变为可扩展（Scalable），T 代表透明，取代了非交互属性。

![](img/1d4cc96d79a7b3f35ffd1f4af76079c5.png)

https://www.altoros.com/blog/zero-knowledge-proof-improving-privacy-for-a-blockchain/

STARK 相比 SNARK 有更多创新。它不需要和 SNARK 一样依赖“可信设置”。它还带有更简单的密码学假设，避免了对椭圆曲线、配对和指数知识假设的需要，纯粹依赖哈希和信息论，因此抗量子攻击。总体来讲 STARK 比 SNARK 更安全。

在扩展性方面，STARK 的扩展性更强。证明生成速度具备线性扩展性，验证时间和证明大小具备对数扩展性。但缺点在于生成的证明尺寸更大。但随着证明规模增加，验证成本将会边际递减——这意味着证明越大，总成本越低。

![](img/8c02468575719f71a6d98bbfde56039f.png)

https://research.thetie.io/zero-knowledge-starks-snarks/

### 3.2 扩展性详解

#### 3.2.1 证明时间线性扩展

证明时间线性扩展：证明人花费的时间与哈希调用的数量呈近似线性关系。

在 80 比特的安全级，STARK 每 12288 次哈希调用的证明者执行时间为 1 秒，得 12288 次/S；而每 98304 次哈希调用需要 10 秒，得 9830 次/S，因此，我们可以知道 STARK 的证明时间和哈希调用基本呈近似线性关系。如下图所示

![](img/de9819e7a6f4dfc6f6b5b50694f9f52e.png)

https://eprint.iacr.org/2021/582.pdf（左图 80 比特安全性右图 100 比特安全性，y 轴单位为秒）

#### 3.2.2 验证和证明大小对数扩展

验证和证明大小对数扩展：验证时间（与证明大小）与哈希调用呈现对数关系。

如下图所示：

![](img/012456dbdd5ce9884aa7f065e0e24c12.png)

左图可以看出，当哈希调用从 3072 增加到 49152，验证时间从 40 毫秒增加到 60 毫秒。而当哈希调用从 49152 增加到 786432，验证时间仅从 60 毫秒增加到 80 毫秒。证明大小同理。因此，我们可得知，哈希调用次数越多，平均验证时间越短，平均证明大小也会更小。

上述试验的配置统一如下

1. 操作系统：`Linux 5.3.0-51-generic x86 64`。

2. CPU：英特尔（R）酷睿（TM）`i7-7700K` @ `4.20GHz`（4 个核心，每个核心 2 个线程）。

3. 内存：`16GB DDR4`（`8GB × 2`，速度：`2667 MHz`）

4. 证明者使用多线程，验证者被限制为单线程。

#### 3.2.3 递归证明

任何通用的、简洁的知识系统的证明/论证（特别是 `STARKs`）都可以用来递增地验证计算。这意味着一个计算可以产生一个证明，以证明该计算的前一个实例的正确性，这个概念被非正式地称为“递归证明组合”或“递归 `STARKs`”。

换句话说，一个递归 STARK 证明者可为一个陈述生成一个证明，即系统的状态可以从 `a` 移到 `a+1`。因为证明者已经验证了一个证实 `a` 的计算完整性的（递归）证明，并且忠实地执行了状态 `a` 的计算，达到了新的状态 `a+1`。简而言之，你可以理解该过程将 `a` 和 `a+1` 两个证明合并为了一个证明。如下图所示：

![](img/9a5e9ea621e792cfeb4ac0e59ed11507.png)

https://medium.com/starkware/recursive-starks-78f8dd401025

这些陈述各自被平行证明。然后，每对证明都由递归验证器语句（一个验证 STARK 证明的 Cairo 程序）进行验证，并为其生成一个证明。该声明断言两个证明已被验证是正确的。

接下来，这两个证明通过递归验证程序语句再次合并。这导出为证明所有四个原始陈述的一个证明。然后可以最终在链上提交该证明，由 Solidity 验证者智能合约进行验证。

递归证明为 L3 的发展奠定技术基础。类似于 StarkNet 作为 L2 在 L1（以太坊）上工作的方式——StarkNet 将计算证明交给以太坊上用 Solidity 编写的验证者合约验证证明。L3 可以在 StarkNet 上部署一个用 Cairo 编写的验证者合约以验证 L3 中的递归证明。

关于 L3，我将在下一篇关于 StarkNet 生态盘点的文章中更详细地描述。

#### 3.2.4 递归证明历史线索

StarkWare 在 2020 年 8 月 31 日发布的文章中提到，能够将 30 万笔交易在 6 分 3 秒内聚合为一个证明。



StarkWare 联合创始人 Eli Ben-Sasson 2022 年 8 月 8 日在韩国首尔会议表示，其新的递归有效性证明理论上可以将以太坊区块链上多达 6000 万笔交易汇总为以太坊上的一笔交易。

不过我没找到相关实现细节。

## ZK 友好的哈希函数

哈希函数在以太坊（以太坊使用`keccak256`）中常用于生成区块、交易、状态哈希，是区块链基础技术。

ZK 友好的哈希函数的意义在于，EVM 本身使用的 `keccak256` ZK 不友好，变成电路执行效率非常低（很多行），使用 ZK 友好的哈希函数转为电路的代码量更少，因此可以提高证明效率。

因此，StarkNet 使用 STARK 友好的哈希函数（SFH）可以显著提高执行效率。

STARK 友好的哈希（SFH）函数，如 `Rescue hash function`（扩展性部分的Hash调用），特点包括以下三点

1. 带有证明的算术化电路，能够将10万个哈希值（3.2MB的数据）压缩到 200kB，有 80 比特的安全性。

2. 证明者在四核CPU和16GB内存的配置下能够每秒压缩100个哈希值。

3. 验证者在单核CPU和4GB内存的情况下，能够在 10ms 内对证明进行验证。

![](img/a1bf1307364f4741f0aeb7c65c595d57.png)

https://twitter.com/Ingo_zk/status/1573033153672921093

## 四、🐺Cairo虚拟机和语言

### 4.1 简介

`Cairo VM` 是一个采用冯诺依曼架构的 CPU VM，其编程语言也叫 Cairo。整个 `Cairo VM` 以及 Cairo 语言基于 STARK 设计。

Cairo 是 CPU Algebraic Intermediate Representation（代数中间表达）的首字母缩写。`Cairo VM` 包含单个 `AIR` 来验证这个「CPU」的指令集。Cairo 语言基于 Cairo 汇编，因此编译效率非常高。

Cairo 语言的编程体验偏汇编（C和Rust，经常会用到指针🎃），它具有以下特性：内存、函数调用、递归和分支条件。它还使用证明者一侧的“提示”（`hints`）来创建快捷方式并启用某些计算。

![](img/b4ae45d82e1f5014c556c6fe7d173628.png)

https://www.cairo-lang.org/docs/how_cairo_works/index.html#how-cairo-works（我捏🐎）

`Cairo VM` 和 Cairo 让我有一种 Unix 和 C 语言的感觉🤪。

![](img/d8b11b72ff4b32b5023081871ce47229.png)

https://www.cairo-lang.org/docs/hello_cairo/amm.html（AMM代码片段）

接下来将介绍 Cairo 的一些强大特性。

### 4.2 启动加载：从哈希加载程序

程序可以将另一个程序字节码写入内存，并让 `Program Counter` 指向该内存段，然后运行该程序。

一个从哈希启动加载的用例是，一个被称为启动加载器的程序计算并输出另一个程序的字节码，然后像之前一样开始执行它。这样验证者只需要知道程序的哈希而非完整字节码。

这有两个好处：

1. 可扩展性，验证时间和程序大小呈现对数关系，正如 STARK 部分提到的。

2. 隐私性，验证者可以验证程序是否正确执行而无需知道计算。

### 4.3 在同一个证明中运行不同程序

正如前文所述，启动加载器可以做一个扩展。比如执行好几个程序。输出每一个程序的字节码hash和程序的输出。而程序可以描述完全不同的计算。

因为proof的大小和验证时间都和计算的大小呈亚线性关系，因此可以使用一个加载器执行多个程序，然后生成一个证明来确保所有程序的有效性。

![](img/71b17f0bfbad031a1225310b494dff46.png)

就像滴滴打车一样

验证的成本在这些程序之间被摊销了。随着更多程序加入批处理，最终的摊销验证成本将越来越接近于0。参考 STARK 部分的“验证和证明大小对数扩展”。

### 4.4 高级优化：即时编译和字节码生成

在程序执行过程中，一些高级优化可以通过字节码的自动生成来实现。

比如，对于一个函数，不同于从内存中预取对应的值，你可以直接拷贝程序的字节码，然后在对应的指令上直接赋予对应的值；考虑一个指令“从内存中读取`x`和`c`，然后计算`x+c`”，一旦`c`的值是已知的（此时用`C`表示），我们可以用更高效的指令去替代它，“从内存中读取`x`，然后计算`x+C`”，`C`是指令的立即数。

因此，我们可以进一步提高执行效率

### 4.5 增量可验证计算：很牛的递归证明

类似 STARK 的递归证明recursive proof部分提到的。

基于从hash启动加载的概念，整个验证过程可以编码成一个hash，然后整个验证过程作为参数传递给自己（这也是生成递归证明的一个步骤）。

### 4.6 其他重要优势

#### 内置函数

开发者直接调用内置函数可以减少计算开销，而不需要代码转换。相当于 ASIC 芯片写好的电路或者数学中给定的、验证过的公式。

#### 图灵完备

支持计算（或者验证，因为`Cairo VM`本身不执行计算，仅验证计算正确性，计算在链下）一切可计算的问题。因此理论上能够复刻其他虚拟机，比如前面提到的用 Cairo 编写的 zkEVM，理论上也可以复刻其他在未来可能开源的 zkEVM/zkVM 如 Scroll、Hermerz、zkSync、Miden、Zero 等。

#### CPU 架构

更加灵活，可以通过软件编程的方式无限接近 AISC 的性能（所以`Cairo VM`会做CPU？）。

提示：关于 CPU 架构、内存、Cairo 语法等方面的更多特性我还在挖掘中（C语言没跑了）……

## 五、📒参考文献

### StarkNet部分

StarkNet 架构回顾

账户抽象模型设计

### Cairo部分

Cairo 白皮书

Cairo 白皮书中文翻译

解读 Cairo 白皮书

Starknet Cairo AIR 论文 2109.14534.pdf

### STARK部分

STARK Documentation – Version 1.1 StarkWare Team∗ January, 2021

深入理解 zk-STARK 证明系统系列

StarkWare 对递归证明的解释

[🤸♀️下面](https://mirror.xyz/zhunianpan.eth/fWR-WLw33w_m_IuYAjjJsGYhp5hBDs2nXFrgBh-rgOo)这个是基于 Arweave 开发的去中心评论插件 ECHO ，点赞/评论需要连接钱包，真不是🎣🤪hh。
