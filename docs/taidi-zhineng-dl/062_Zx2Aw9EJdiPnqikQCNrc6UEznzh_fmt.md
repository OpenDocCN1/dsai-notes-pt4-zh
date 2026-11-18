
# 开源 GPGPU

> 来源：`https://learning.feishu.cn/docx/ChlSd8UwuoSpIsx6dJLcnhIBnae`

相关笔记：`https://dwpl6xgouw.feishu.cn/wiki/ZzYvwIjsuie4CLkcvTOcxVgVneE`

工业界大部分gpgpu都是以 simt+tensor core 来实现的

有些协处理器(Coprocessor, GPU通常也被认为是Coprocessor),采用了双层指令集的设置：协处理器接收CISC,从而降低instruction通信的压力；然后在coprocessor上通过硬解码，将CISC转成RISC再进行控制与计算。

一些计算芯片的初创公司的软硬件工程师比例甚至能达到10:1 (以Habana为例，注：这是听来的，我也没有证据)。

## 重要开源

### 佐治亚理工 - Vortex

Vortex: OpenCL Compatible RISC-V GPGPU

`https://arxiv.org/abs/2002.12151`

2020.02

关键贡献：

1. 定制 5 条指令 + `RV32IM` (RISC-V 32 bit integer and multiply extensions)，就可以启用 SIMT execution。

1. 能跑 OpenCL 程序，需要简单的适配。

1. 开源了 RTL 代码。

项目情况：

1. 支持 OpenCL 并在 FPGA 上运行，

1. 有完整的开源编译器、驱动程序和运行时软件栈，可用于 GPU 架构研究。

`![](img/38bf1045f80ade25f9ba2fc79dc4eabf.png)`

`![](img/5088025cdced3168dceb56bff5b74a42.png)`

`![](img/eeb8a1e4ad1a15ec421f040237bec669.png)`

### AMD GCN 1.0 - Miaow

`https://github.com/VerticalResearchGroup/miaow`

基于 AMD GCN 1.0(Southern Islands ISA) 子集的实现。

威斯康星麦迪逊大学Vertical Research Group

2014 年开源，从 2017 年开始，七八年未更新了。

MIAOW 是 Many-core Integrated Accelerator Of the Waterdeep的缩写

设计文件中包含了GPU架构细节、RTL代码、FPGA实现介绍、仿真TB、测试用例等

Enabling GPGPU Low-Level Hardware Explorations with MIAOW: An Open-Source RTL Implementation of a GPGPU

`https://dl.acm.org/doi/10.1145/2764908`

MIAOW Architecture Whitepaper (PDF)

AMD Southern Islands ISA (PDF)

MIAOW: An Open Source GPGPU `https://old.hotchips.org/wp-content/uploads/hc_archives/hc27/HC27.25-Tuesday-Epub/HC27.25.50-GPU-Epub/HC27.25.512-MIAOW-Balasubramaniam-UWisc-v1.2.pdf`

poster

`![](img/16a67bae2d69b1fda8ef08d8354d5743.png)`

Architecture Overview

`https://github.com/VerticalResearchGroup/miaow/wiki/Architecture-Overview`

`![](img/f7e34540535d0de9b190d881a91264d9.png)`

cuda core 部分的 SIMT，看起来也是用 SIMD 做的。

`![](img/18d798ebc97fa365010b147c96c8d485.png)`

### 清华 - 乘影

`https://github.com/THU-DSP-LAB/ventus-gpgpu`

GPGPU processor supporting RISCV-V extension, developed with Chisel HDL

2024年1月26日正式启动。

何虎 团队。集成电路学院

亮点：

1. RVV 扩展 + Chisel 实现（硬件代码）

1. ISA conversion proposal from vector processor to GPGPU

1. software toolchain from OpenCL to our RVV-based GPGPU ISA

架构文档：`https://github.com/THU-DSP-LAB/ventus-gpgpu/blob/master/docs/%E4%B9%98%E5%BD%B1GPGPU%E6%9E%B6%E6%9E%84%E6%96%87%E6%A1%A3%E6%89%8B%E5%86%8Cv2.01.pdf`

承影也是simt，但多了vector unit做cu。

`https://github.com/THU-DSP-LAB/ventus-gpgpu/blob/master/docs/ICCD2024_Ventus_Presentation.pdf`

`![](img/0c3de02f752895f19855dfaca38ab053.png)`

“乘影”的自定义 ISA：

+ 分支、同步、线程束控制：主要是为了在SIMT架构下运行程序所必需的控制指令。解决多线程锁步执行时，不同的线程要走不同分支的问题。还有核函数结束指令。

+ 寄存器/立即数扩展：主要是是为了解决RISC-V指令集寄存器编码空间和立即数位宽不足的问题。扩展以后的指令集可以支持64个标量寄存器，256个向量寄存器，11位立即数。与主流GPGPU相同。

+ 寄存器对拼接：是为了解决32位寄存器处理64位地址和数据的问题。乘影没有简单的采用`RV64`扩展寄存器位宽的方式来处理64位地址和数据。因为在GPGPU应用中，64位数据使用非常稀少，64位寄存器将对硬件设计带来浪费。乘影用两个相邻32位寄存器构成64位寄存器的方式来解决这个问题。

+ 自定义访存指令：实现OpenCL定义的访存操作。

+ 张量计算和指数函数：支持Transformer类型的神经网加速设计的

`![](img/ee66d61035ce232aa89434e187c95d15.png)`

`![](img/978c37a60d75795bb81753a604dcbab0.png)`

`![](img/d1de8c55e921bde654ee7d3c8cb8b95f.png)`

每个流多处理器（Streaming Multiprocessor，SM）主体结构接近于RISC-V。后端执行则直接使用处理器中常用ALU、FPU、LSU。为了让SM能够完成GPGPU工作，乘影里添加了`SIMT-stack`和SFU这类GPU中的常见功能模块。

`![](img/7435cc75906f4e9a8645b1f43cd70ead.png)`

`![](img/3e1560b9f6d2b051affc59638ab74793.png)`

`![](img/ef9101147e451d3334cb5f44bac5c5b5.png)`

`![](img/ef55c5f8362715d0aae879a1b7d4ca03.png)`

## 其他开源 List

`![](img/08826f5905d9f5e59897d6683a184cd0.png)`

### Celerity

`http://opencelerity.org/`

陈巍：RISC-V AI芯片Celerity史上最详细解读

`https://zhuanlan.zhihu.com/p/447836924`

5个大core+16x31个小core+二值网络

Celerity currently holds the world record for RISC-V performance; 500B RISC-V instructions per second, beating prior records by 100X.

设计展现了两个特点：

1） 设计了基于RISC-V核心的多级（Tier）异构AI加速结构，兼顾计算中灵活性和能耗的要求，在硬件层面直接支持AI计算，实现了更好的功能和更高的能效。

2） 采用了HLS+Chipsel+开源IP的敏捷设计方法提升芯片的设计速度，明显缩短芯片的研发周期到几个月。

该芯片由Michigan大学、 Cornell 大学、和 Bespoke Silicon Group（目前属于Washington大学）共同完成。并且该项目受到了DARPA（美国国防高级研究计划局）的资助。Celerity的设计源码已经可以下载。

Celerity多级结构组成。（图1）它们分别为通用级（General-Purpose Tier）、众核级（Manycore Tier）和专用级（ Specialization Tier）。三级之间两两互连

`![](img/f840444fc5858f2da139ad560c65a9f9.png)`

Manycore Tier与NoC

`![](img/b8eaaaef3e3b3c898f6ba267a2bcf16e.png)`

众核级内部核心通过网格网络（Mesh Network，图7）互联。网格网络的每个节点都是缓冲路由（Buffered Router）,按照所在位置确定`X/Y`坐标编号，并通过前向包与反向包进行全双工连接。缓冲路由间通过特定的连接协议进行通信与数据传输。其中前向连接为80bit位宽，反向连接为10bit位宽。

`![](img/e59ffd06678a07291e116fd98f91d2a9.png)`

Accelerating Binarized Convolutional Neural Networks with Software-Programmable FPGAs

`https://dl.acm.org/doi/10.1145/3020078.3021741`

22 February 2017

Celerity专用级（图1最右侧）集成了二值化神经网络（BNN）加速器。

`![](img/00b6085e10e4268320d93d9b7a351b95.png)`

图4 二值化神经网络计算流示意

这一神经网络由1层定点卷积层、6层二值化卷积层和2层全连接层组成。根据文献，这一结构的神经网络在CIFAR-10数据集上可以达到89.8%的精度。可支持13.4M个权值。

`![](img/b1e90b28ff2161bc4061d69091d9547e.png)`

图5 BNN架构与二值化卷积模块 | （a）BNN核心的架构 (b) 二值化卷积模块

在二值化卷积的计算过程中，每两个特征图（feature map）分别与3卷积核进行卷积计算，生成累加后的3组下一级特征图。

BNN结构，既可以看作是Celerity芯片的亮点，也可以看作是Celerity不足之处。

### 芯瞳半导体 - RVGPU

2024年2月，开源。

RVGPU采用 SIMT 技术，现已成功实现对CUDA（Compute Unified Device Architecture）的兼容支持，包括CUDA编译器和运行时环境。除此以外，RVGPU的架构指令`CModel`仿真模型允许开发者在模拟环境中模拟GPU的各种指令和行为。

基于RISC-V指令集的GPU处理器模型RVGPU，这款模型采用SIMT（单指令多线程）技术，能够为GPU设计带来新的灵活性和扩展性。现已成功实现对CUDA的兼容支持，包括CUDA编译器和运行时环境。该公司表示，将进一步强化对CUDA的支持，并积极探索RISC-V在GPU领域的更多可能性。

RVGPU软件栈

`![](img/f3cff369e04027aea5edae2fd84f466d.png)`

### 上交大 - 青花瓷

类似 佐治亚理工 的工作。

上交 ACALAB (先进计算机体系结构实验室)

`https://gpgpuarch.org/`

### 美国公司 X-Silicon

成立于2022年

发布了开放标准、低功耗的 C-GPU 架构

C-GPU架构将打造成为开源、统一的RISC-V 矢量 CPU-with-GPU ISA 架构，C-GPU采用NanoTile架构，X-Silicon将这种CPU/GPU混合处理器称为“万能处理器”。其处理器专为动态内容最佳管理和渲染而设计，并超越传统GPU限制。

翻译翻译就是把基于RISC-V架构的CPU和GPU放在同一个核内。

`![](img/66ab097f1e4a591c197f9ccc180bdd63.png)`

### VeriGPU

`https://github.com/hughperkins/VeriGPU`

OpenSource GPU, in Verilog, loosely based on RISC-V ISA

个人 project，比较玩具水平。已经 2 年不迭代了。

Big Picture:

`![](img/f508dc5e6267b2962c3cc4d5d90a2f77.png)`

GPU Die Architecture:

`![](img/39ec38a2f01b8a7b467adb1bdc9dfa55.png)`

Single Core:

`![](img/d8580eeeb78439a68c94d4a593c67f7d.png)`

## 关键模块

### GPU 指令集

英伟达以指令集为基石的高可编程性，也就是CUDA，成为了其在相关领域取得市场份额和话语权的关键因素。

GPU行业中，大部分商业公司选择将指令集架构作为闭源技术标准。

不过英伟达和AMD都选择开放部分指令集架构标准，初衷主要是为了让应用开发者进行更深入的性能优化。

问题 list：

1. 当前普遍选择了 RISCV，为什么是RISC-V？

1. RVV (SIMD) 和 GPGPU (SIMT) 有区别。需要深度修改以在 GPGPU 上实现 Vector 指令，怎么改？

### SIMT Stack

`https://www.zhihu.com/question/610068003/answer/3101628510`

`https://www.zhihu.com/question/609674268/answer/3097932895`

使得，SIMD 可以实现 SIMT 中 branch divergence 的行为。

通过Lane Mask屏蔽不需要被SIMD pipeline执行的`func lane`，串行地依次执行每一个分支的基本块代码，最终仍然可以给程序员提供每个线程独立执行的假象，同时根据SIMT条目中记录的基本块的`IPDOM`指示分化线程最早可能的reconvergence point，尽力提升程序的执行效率。

`![](img/ae731dc911e566f83a017ec78cb1cb59.png)`

`https://engineering.purdue.edu/tgrogers/publication/kang-micro-2024/kang-micro-2024.pdf`

`![](img/19133ffd7cf62ece88f223988eaaf4d5.png)`

`![](img/ff5f8d865bcd904306ecab1d7a521401.png)`

## UXL 基金会

全称Unified Acceleration Foundation，创始成员包括Arm Holdings plc、富士通、谷歌、Imagination Technologies、英特尔公司、高通公司和三星。

路透社最新爆料，从英特尔开发oneAPI开始，UXL就计划构建一个开源项目，打造一套能够驱动多种AI加速器芯片的软件和工具，使计算机代码能够在任何机器上运行，无论使用何种芯片和硬件。

`![](img/c3ff267241e54ff74d3ddcdeecbe357d.png)`

有高管透露，UXL技术指导委员会正准备在今年上半年确定技术规范，工程师们计划在年底前将技术细节完善到“成熟”状态。

除谷歌英特尔高通等最初参与的公司之外，UXL还将吸引云计算公司，如亚马逊和微软Azure，以及更多的芯片制造商。UXL还接受来自热衷于开源技术的外部人士等第三方技术贡献。

最终他们的目标是支持英伟达的硬件和代码。

## 市场 & 利润

`https://mp.weixin.qq.com/s/Mmtr2emNZVa5Xqf906pyZg`

Tenstorrent今(2024)年底准备推出第二代多用途AI芯片。据公司说法，在某些领域，这款AI芯片的能源、处理效率优于英伟达AI GPU。事实上，跟英伟达DGX系列AI伺服器相比，Tenstorrent的Galaxy系统不但效率高三倍、价格也便宜33%。

这是如何办到的呢？Keller说，其中一个原因，就是Tenstorrent并未使用高频宽记忆体(HBM)。HBM是生成式AI芯片的重要元件，英伟达产品能获致成功、HBM扮演重要角色。然而，HBM同时也是导致AI芯片耗电严重、价格高昂的元凶之一。

一般来说，AI芯片组每处理一次任务，GPU就会把数据传送至记忆体。这需要HBM的高速数据传输能力。然而，Tenstorrent芯片大幅降低了数据传输次数，其特点是数百个核心中、每个核心都有一颗小型CPU，能自行判定哪些数据得优先处理、哪些非必要的任务可以放弃，进而提升整体效率。

Keller认为，全新方式可让Tenstorrent芯片在部分AI研发领域取代GPU及HBM。不只如此，该公司也会尽量提升产品成本效益，但Keller坦言想要干扰现今规模庞大的HBM产业、恐得花上数年时间。他预测，未来将有更多新兴业者抢进英伟达目前还无法服务到的AI市场，而不是出现单独一家完全取代英伟达的企业。



# AI 基础建设成本与营收缺口分析

根据Sequoia Capital分析师David Cahn日前估算，AI企业每年必须赚取约6,000亿美元，才能负担它们的AI基础建设(例如数据中心)。

Cahn的计算方式相对简单。首先，他将英伟达的营收运转率(run-rate，指按照运行速度推估未来表现)预估值乘以二，来估算AI数据中心的总成本(GPU占据一半，其余是能源、建筑及后援发电机)。然后，Cahn再将这个数字乘以二，来涵盖终端用户(例如新创商、或向AWS等云端商购买AI算力的业者)的50%毛利率。

然而，就算是乐观情境下，科技巨擘也难以创造6,000亿美元的AI营收。Cahn指出，假设Google、微软、苹果(Apple)及Meta各创造100亿美元AI相关年营收，而甲骨文(Oracle)、字节跳动(ByteDance)、阿里巴巴(Alibaba)、腾讯(Tencent)、X及特斯拉(Tesla)各创造50亿美元AI年营收，这跟AI企业每年需赚取的6,000亿美元相比，还是有多达5,000亿美元的差距。

值得注意的是，英伟达新推的B100/B200处理器，已承诺效能可望高2.5倍、成本仅增加25%。英伟达执行长黄仁勋(Jensen Huang) 2月曾表示，AI处理器的架构创新，比产量更重要。
