
# RISC-V Chips for Deep Learning

> 来源：`https://learning.feishu.cn/docx/C22zdzsrdoLCr0xdwOMctB0ynCg`

## 目标

1.  理解 `rv` 生态在 `deep learning` 时代的前景、进展。

1.  准备一些可以放入公司 `PR` 材料里的 `RISCV` 相关卖点。包云岗 的材料，不错。

## Introduction

2023年，用 `RISCV` 做 `AI` 加速器一般是三种路径：

1.  高性能 `CPU`。用 `RISCV Vector ISA` 来做 `CPU`。把 `vector core`/`vector engine`/`matrix` 这些东西做在 `core` 内，或者当成协处理器挂在 `core` 外。参考玄铁 `c908` 等支持 `rvv` 的 `cpu core`

1.  `GPGPU`。用 `RISCV Vector ISA` 来做 `gpgpu`。

1.  `NoC` 搞集群。比如 `Celerity` 这种大怪兽。5 个大 `core` + 16x31 个小 `core` + 二值网络

### riscv for GPGPU

以 `Nvidia GPU` 为例，它的硬件架构分成几个层面：

+   多核并行（一个 `GPU` 由多个 `SM core` 组成）

+   多内存层级 （`Global`/`Shared`/`Register` 以及 `cache`）

+   一个 `SM core` 内部：

+   多个 `cuda core`，负责通用计算

+   `DSA` 扩展，如 `TensorCore`、`TMA`，负责矩阵计算

真正和 `RISC-V` 有关系的，其实就是 `GPU` 的通用计算部分，也就是 `cuda core` + `warp scheduler` 部分，这部分其实和通用 `CPU` 处理器的原理差不太多，`RISC-V` 也是一个面向通用处理器架构的指令集（当然 `Nvidia GPU` 经过几十年的发展肯定内部做了大量的优化）。所以理论上可以把 `GPU` 的通用计算部分平替成 `RISC-V` 处理器。

具体到 `GPU SIMT` 计算架构，可以用 `RISC-V vector` 扩展来支持 `GPU` 的 `warp threads`，每个 `vector lane` 对应 `warp` 中的一个 `thread`。用 `RISC-V` 的 `hart scheduler` 来代替 `GPU` 的 `warp scheduler`。

![](img/45384f11c3b58f1d1529f2e289bc7fb7.png)

当然，通过 `RISC-V` 支持了通用计算只是 `GPU` 的一小部分，如果想做一个能用在数据中心的 `AI` 计算 `GPU`，还需要支持多核架构、多内存层级、多 `GPU` 互联、以及一堆针对具体应用（如 `AI`）的 `DSA`，比如 `tensorcore`、`TMA` 等。

### riscv for HPC CPU

最近这几年，已经出现了一批 `RISC-V` 高性能处理器实现，代表企业有硅谷的 `Sifive`、`Ventana`，在技术上有一定优势，国内还有 `香山处理器`，以及 `上海赛昉`。

![](img/ecbc7e6203df0c720da5dd0eaa991005.png)

![](img/4642311d66466a6157510fe8e69a35c4.png)

`SiFive` 有两个高性能计算上相对重要的客户：`谷歌` 和 `NASA`

最新（2023）的 `P670` 是个 13 级流水线、4 发射的乱序架构，在面积效益上对标的是 `Arm Cortex-A78`。

`MIPS` 公司的 `eVocore P8700` 核心，从 `8-wide` 取指、`7-wide` 执行来看，这是个超宽大核心；每个 `cluster` 最多 8 核方案，可扩展至 64 `cluster`、512 核心；应用方向是汽车、数据中心、`HPC` 等。

![](img/7901afd767a24de0dad8c83792e6f03c.png)

高性能 `CPU` 设计为什么很难？

`https://zhuanlan.zhihu.com/p/651765552`

1.  完全从头重写高性能 `CPU` 需要的周期很长，举几个例子感受下：

（1）高通之前的 1000 人 `QDT` 研发团队，在第二代 `core` 还不一定能够到 15 分，然而第一代就经过了两年多的研发（约 10 分/GHz），第二代研发（内测性能提升 30%+）进入尾声又过去了两年，后来因为董事会不同意继续撒钱，项目彻底砍掉不了了之。

（2）`Apple M1` 中的 `Firetail core`（大约 22 分/GHz），是从 2010 年的 `Apple A4`（基于 `ARM Cortex A8`）一直迭代到现在的产物，相关研发人员具体我不清楚，但几百人肯定是有的。

（3）`龙芯` 上一代 `3A5000` 是 10.x 分/GHz，这一代 `3A6000` 是 15 分/GHz，最新一代微结构性能提升了 50%，这个性能已经达到了全球范围内的第一梯队，至少垫垫脚能看到前面的 `AMD`、`Intel`、`Apple` 了。走到这一步 `龙芯` 坚持了 21 年，值得让人尊敬。很多人看不起 `龙芯`，全国范围内所有 `CPU` 设计公司，注意是所有，有哪家设计的 `CPU` 单线程性能超过了 `龙芯`？

（4）`鲲鹏 920`，大概 10 分/GHz，其中的 `图灵核` 是从 2010 年开始组建团队研发。`图灵核` 团队还在继续自研 `core`，我相信性能肯定会进一步提升，但是由于制裁导致产品不能流片，这里不做推测和介绍。

曾经跟 `IBM` 和 `Qualcomm` 的架构师交流，如果把他们架构师核心团队原班人马拉出来，重新组织团队设计新的高性能 `CPU`，没有两代（4-5 年）也不可能达到目前 `AMD` 和 `Intel` 的最高水平。

有人说，买了最新 `IP` 消化吸收再创新行不行？依然很难，`中晟宏芯`、`华芯通`、`海光`，还有几家买 `ARM N2`，这几家都是这种思路，哪家已经做到了修改代码随心所欲不逾矩？`RTL` 与应用软件的区别就在此。

1.  国内在高性能 `CPU` 微结构设计方面的积累和人才不够。

我国从 2010 年左右才开始涉足高性能 `CPU` 的设计，之前自研的 `CPU` 都偏中低端。

1.  国内的高性能研发无法形成良性循环。

“`信创`”推动国产替代的一个重要原因：必须扶持本地企业做正向研发，才有可能（注意只是有可能）在未来拜托国外在高性能 `CPU` 方面的垄断。

1.  基础设施建设不足。

问题 3 间接导致的。比如，仿真实验的 `CPU` 模拟器。

1.  高性能 `CPU` 设计没法弯道超车，没有后发优势。

`CPU` 微结构的探索过程、`PPA` 的优化过程绝对是内部极度保密的资源，不会轻易让你看到、学到。

`IBM` 为了开发性能探索用的模拟器，甚至先开发一门 `T语言`，这不可能公开。当年中晟宏芯买来的 `IBM` 的 `M1` 模拟器就是 `T语言` 编写，但是只给你加密后的二进制，付费也不给源码，不仅如此，`core` 的微结构文档大幅删减，想读懂他们 `VHDL` 写的源码需要花费大量时间；`华芯通` 买到的高通代码不仅没有微结构文档，甚至连断言都给你删掉；更不用说美国政府限制的高性能浮点模块不允许出口，这些花多少钱也买不到。

1.  设计高性能 `CPU` know-how 很难

四大顶会的论文质量、数据、结论等，参差不齐。

很多论文中很漂亮的 `idea`，评估效果也不错，但是真正落到芯片中代价太大，例如面积太大、时序紧张等，但是尝试把新 `feature` 落到产品的周期都要以年为单位计算。

### riscv for DSA

倪光南：`riscv` 自定义扩展指令，是实现 `DSA` 的有效途径

![](img/220c2ce7822ef5222d81e88f9634e6da.png)

`Energy costs for a 90 nm process to fetch instructions or access a data cache compared to the energy cost of arithmetic operations`

![](img/7a6afad2b28192d54a1b39cfe4e8bfa2.png)

![](img/36afc8f3cd37db88e96b8c360bdd91e3.png)

`DSA` 设计的五个原则

1.  使用专用存储器以最小化数据移动距离。通用微处理器中的多级缓存使用大量区域和能量来尝试为程序最优地移动数据。例如，双路集合关联缓存消耗的能量是一个等效的软件控制的暂存存储器的 2.5 倍。根据定义，`DSA` 的编译器编写者和程序员理解他们的领域，因此没有必要让硬件为他们移动数据。相反，通过专为特定领域内特定功能定制的软件控制存储器来减少数据移动。

1.  将放弃高级微架构优化所节省的资源投入到更多的算术单元或更大的存储器中。架构师将摩尔定律的收益转化为 `CPU` 和 `GPU` 的资源密集型优化（乱序执行、多线程、多处理、预取、地址合并等）。鉴于对这些更狭窄领域内程序执行的更深入了解，这些资源最好用于更多的处理单元或更大的片上存储器。

1.  使用与领域相匹配的最简单形式的并行性。`DSA` 的目标领域几乎总是具有固有的并行性。`DSA` 的关键决策是如何利用这种并行性以及如何将其暴露给软件。围绕领域的自然并行粒度设计 `DSA`，并在编程模型中简单地暴露这种并行性。例如，关于数据级并行性，如果 `SIMD` 在领域内有效，对程序员和编译器编写者来说肯定比

1.  将数据大小和类型简化为领域所需的最简形式。正如我们将看到的，许多领域的应用程序通常是内存受限的，因此通过使用更窄的数据类型，您可以增加有效内存带宽和片上存储器利用率。更窄和更简单的数据还允许您在相同的芯片区域内封装更多的算术单元。

1.  使用特定领域的编程语言将代码移植到 `DSA`。 `DSA` 的一个经典挑战是让应用程序在您的新型架构上运行。一个长期存在的谬误是假设您的新计算机如此吸引人，以至于程序员会仅仅为了您的硬件重写他们的代码。幸运的是，即使在架构师被迫将注意力转向 `DSA` 之前，特定领域的编程语言就已经变得流行。

### RISC-V in Nvidia

英伟达 `RISC-V`，鲜为人知

`https://mp.weixin.qq.com/s/hSZpamUaFOga6QVayEo4Yw`

早在 2015 年，英伟达就将 `RISC-V` 选定为其专有 `Falcon MCU` 的继任架构。`Falcon` 一直是英伟达专有的控制处理器，负责在 `GPU` 芯片中完成关键的控制任务。然而，随着技术的快速演进，新兴用例对 `Falcon` 的功能和性能提出了更高要求，现有的 `CPU` 架构和指令集（`ISA`）都难以满足。通过比较多种指令集，英伟达认为 `RISC-V` 是下一代 `ISA` 的正确方向，并且称 “`RISC-V` 是唯一符合我们所有标准的架构。”

各种指令集的综合比较

（来源：`RISC-V in NVIDIA`，2017）

![](img/da99ab41a4df9808f52d42ae5789038e.png)

2016 年，英伟达将基于 `RISC-V` 指令集架构的新型 `GPU` 系统处理器 (`GSP`) 解决方案取代其代号为 `Falcon` 的快速逻辑控制器处理器。这款新型 `RISC-V` 处理器的代号为 `NV-RISCV`，被用作 `GPU` 的控制器核心，协调大量 `GPU` 核心中的所有内容。

`NVIDIA RISC-V Story`，2016

（来源：英伟达）

![](img/e4bbee04c0185dd9a3e5c65eaa3ab67c.png)

如今，几乎所有的 `MCU` 内核都基于 `RISC-V` 架构。

英伟达已经推出了三类 `RISC-V` 控制器核心，分别是 `NV-RISCV32`、`NV-RISCV64` 和 `NV-RVV`，每一类核心都针对特定应用进行了优化，展现了极高的灵活性和可扩展性，逐渐取代了英伟达专有的 `Falcon MCU`：

+   `NV-RISCV32`：支持 `RV32I-MU` 指令集，包括乘法、压缩和浮点运算功能。它们使用顺序执行架构，单发射，性能达到 1.8 CM/MHz，主频为 1.8 GHz。

+   `NV-RISCV64`：支持 `RV64I-MSU` 指令集，增加了位操作和原子操作功能。其使用乱序执行架构，双发射，性能提升至 5 CM/MHz，主频为 2 GHz，并支持对称多处理（`SMP`）。

+   `NV-RVV`：在 `NV-RISCV32` 的基础上增加了向量扩展（1024 位），以支持高性能向量运算。

英伟达还在 `RISC-V` 核心上增加了超过 20 项自定义扩展功能，主要包括三大类：（一）功能扩展：支持 64 位物理地址/虚拟地址扩展，提供 2KB 页面大小的支持；（二）安全性：集成 `ICD`（`In-Circuit Debug`）的安全调试功能，支持 `ROM` 内存保护；（三）性能优化：提供缓存操作扩展以提升性能。

英伟达 `RISC-V` 核心的模块化设计还包括多个关键组件，如中断控制器、`Trace Buffer`、在电路调试模块以及引导和控制寄存器等。这些模块与 `NV MPU`（内存保护单元）、`PMP`（物理内存保护）和 `PMA`（物理内存属性）相结合，确保了高效的任务执行和数据处理能力。

从硬件结构来看，`GSP` 位于主机处理器与 `GPU` 的中间层，通过 `PCIe` 接口与主机通信，并直接控制 `GPU` 的硬件单元和视频内存。`GSP` 基于模块化设计，结合了多个嵌入式 `RISC-V` 核心及协作组件：包含多个 `RV64` 核心，一致性互连（`Coherent Fabric`）、统一 `TCM`/缓存，还有一些外围模块。内存接口连接到 `GPU` 的视频内存。`IOPMP`（输入输出内存保护）提供内存访问的安全保障。

![](img/f377a4b47e874508fee87f1cb27764dc.png)

`GSP` 具有三大重要的功能：（一）内核驱动卸载：`GSP` 接管了许多传统上由主机内核驱动程序完成的功能，减轻了主机处理器的负担，提高了系统整体效率。（二）减少 `GPU` 对 `CPU` 的暴露：`GSP` 通过中间抽象层的方式，将 `GPU` 的低级硬件细节封装起来，降低了 `GPU` 在系统中直接暴露给 `CPU` 的复杂度。（三）封装 `GPU` 低级细节：`GSP` 的软件模块负责对 `GPU` 底层功能的封装，为上层软件提供了简化的接口，同时增强了系统的安全性和可靠性。

`GSP` 的架构设计允许 `PC` 或 `GPU` 在云端被多个远程用户共享使用，特别适合虚拟化和多租户环境。

英伟达的 `GPU` 系统处理器（`GSP`）不仅在硬件层面展现了强大的性能优化能力，其软件架构同样体现了高度灵活性和安全性。具体而言，`GSP` 的软件架构支持多分区模式，允许 `GPU` 资源在多个虚拟机（`Guest VM`）之间共享。`GSP` 内部的资源管理器负责为每个虚拟 `GPU`（`vGPU`）分配计算时间、内存等资源，确保各分区能够高效运行而互不干扰。每个 `Guest VM` 的驱动程序仅与其对应的 `vGPU` 运行时分区进行通信，确保虚拟机之间的隔离性和数据安全性。通过卸载内核驱动任务，`GSP` 支持机密计算功能，为高安全性应用提供支持。

这一切强大的功能，都是 `RISC-V` 架构所提供的基础支持，包括安全性和隔离能力以及为 `GPU` 集成量身定制的扩展功能。

英伟达的深度学习加速器（`Deep Learning Accelerator`，`DLA`）利用 `RISC-V` 架构的模块化设计和 `RVV` 向量扩展，为深度学习任务提供了高效的硬件加速能力。

硬件层面，`DLA` 采用的是包括 `NV-RISCV32` (`RV32IMC`) 和 `NV-RVV`（`RISC-V` 向量扩展），分别用于标量和向量计算。模块化设计中包括卷积核心、`SDP`（缩放与动态精度处理）、矩阵变换和优化模块 `RUBIK`、`CDP`（压缩与解压模块）、`Cfg DMA`（可配置 `DMA`）。内存接口集成了 `ITCM`、`DTCM` 和 `VTCM` 等紧耦合存储器，用于高效数据访问。另外，卷积缓冲器和内存接口优化了数据流传输和并行计算。

在编译器上，`DLA` 的编译器集成了 `RVV` 编译器和 `RISC-V` 通用编译器，编译器可以将多个节点（`nodes`）整合到一个 `RVV` 内核中，优化硬件计算效率。`DLA` 硬件模块直接通过运行时和编译器与高层应用交互。`DLA` 特别适用于深度学习训练、推理优化等应用场景。

在应用上，`RISC-V` 已经在其产品中实现了从功能级别控制到芯片与系统级别控制再到数据处理的全面覆盖。具体来看，在功能级别，`RISC-V` 被用于处理视频编解码、显示、摄像头控制、内存控制器（主要应用于训练任务）等核心功能。同时，它也在芯片间接口和上下文切换等领域扮演关键角色。在系统级别，`RISC-V` 负责资源管理、电源管理以及安全控制，进一步提升了芯片的整体运行效率和可靠性。此外，`RISC-V` 还被应用于数据处理任务，例如网络中的数据包路由以及深度学习加速器（`DLA`）中的非 `GPU` 任务处理，展现出其在高性能计算中的潜力。

从时间线来看，英伟达对 `RISC-V` 的应用始于 2018 年的 `Turing` 架构，当时主要聚焦于硬件与软件的基础设计。到 2020 年的 `Ampere` 架构阶段，`RISC-V` 的应用逐步扩展至 `IP` 集成。在 2022 年的 `Ada` 和 `Hopper` 架构中，英伟达进一步强化了 `RISC-V` 在安全领域的能力。而展望 2024 年即将推出的 `Blackwell` 架构，英伟达表示将继续聚焦 `RISC-V` 在性能和扩展性上的突破。

## RISCV pros

`RISC-V`，与 `x86`、`Arm` 一起，被业界并称为「三大 `CPU` 主流架构」。

因开源开放、灵活、低功耗等特性，`RISC-V` 具备没有历史包袱、没有高昂授权费用、开发成本更低、无需担忧 “断供” 风险等优势，被视为发展自主可控本土芯片生态的关键路径之一。

## why riscv - David Patterson

开发 `RISC-V` 只花了两三个月，它仅有约 50 条指令，非常轻巧。

![](img/a7e97071d75c5d7e2d637098b9f828cf.png)

从 `David Patterson` 的书 (`Computer Organization and Design RISC-V Edition`) 看，

为什么 `Risc-V` 比 `ARM` 和 `X86` 都更好？

1.  从 `General` 的层面，对比 `Risc-V` 和 `X86` 的性能

![](img/3234b078d07d38aa867f01d6de7afc0c.png)

1.  从具体的指令集设计层面

1.  指令格式的优化：`Risc-V` 的指令只有 6 种格式，`ARM` 有很多，所以 `arm` 的解码不见在低端中偏昂贵，高端有性能挑战。`Risc-V` 的寄存器操作数更多，会减少 `Move` 指令的数量；`RIsc-V` 可以在解码之前先访问寄存器，逻辑上更简单，并且，立即数符号扩展也可以在指令解码前进行。

1.  寄存器的设计：为 `0` 单独分配了一个寄存器，否则，程序计数器作为单独寄存器的方式会导致分支跳转变得很复杂。

1.  整数计算的设计：`Risc-V` 没有字节或者半字节宽度的计算操作，全部都是完整的寄存器宽度，也不包含乘法和除法。前者节约了内存访问的能量，后者有时候可以缩小嵌入式芯片的面积。

1.  `Load` 和 `Store`：`Risc-V` 省略了复杂寻址，但可以模仿 `X86` 寻址模式；并且 `Risc-V` 不需要对内存里的数据的自然大小做边界对齐，这使得其通用性和简洁性都有很大的提升。

1.  条件分支：丢掉了 `MIPS` 和 `Sparc` 里广受诟病的延迟分支特性，也没有使用条件码，这使得一切都变得更简单。

## RISC-V 发展态势与趋势 - 包云岗

`RISC-V` 发展态势与趋势



video: https://www.zhihu.com/zvideo/1591586718976774144

https://www.zhihu.com/question/357557401/answer/2828629042

2022-12-29

在北京微电子国际研讨会暨IC WORLD大会上的报告

## 一、RISC-V的定位

1）RISC-V是一种指令集，不是处理器实现

2）什么是处理器芯片设计的核心能力？

3）指令集重要，还是不重要？

4）关于RISC-V的五个谬误

5）高铁模式/北斗模式/5G模式

## 二、RISC-V发展态势

6）RISC-V国际基金会情况

7）态势一：高性能RISC-V

8）态势二：软件生态

9）态势三：企业投入

10）态势四：各国投入

11）中国发展情况

## 三、实践

11）香山近况

12）开芯院与香山

## 四、展望

13）开源硬件成功案例：IBM PC

14）透过处理器发展历史看未来

RISC-V是一种指令集，并不是一种处理器的实现

有了指令集标准规范，最重要的就是芯片设计。根据指令集来去完成微架构的设计，形成文档，然后通过工程开发形成源代码，之后再用EDA软件形成芯片版图，交给台积电或中芯国际这些代工厂去流片。

![](img/cb74d27f58aa778ee859f426d4c80ca6.png)

![](img/bea40bc8bd722bb5fcf7ab6797b7dcd0.png)

![](img/5b75e171769fed3999cb35c314ff43f7.png)

1. RISC-V已经开始往高性能领域发展

![](img/242e27237601d3e52506b08aafef5434.png)

2. 关键软件生态的发展也是非常迅速

以Linux发行版Debian为例，开源社区于2019年开始支持RISC-V，在全世界开源社区的努力下，仅用3年时间就完成了2万多个软件包中93%的移植，使RISC-V成为Debian支持的一级(Tier-1)架构。

![](img/ab0c7a6f0a15cc81ce214b734b1d3e36.png)

3. RISC-V得到了越来越多企业的支持

![](img/40e00689bae30f73a0dc5f4fc3bfc69d.png)

4. 一些国家在国家层面上在积极地推动或支持RISC-V

2022.6 月，俄罗斯数字发展部宣布将大力扶持RISC-V处理器的发展。

印度也在 2022.4月份启动了“数字印度RISC-V处理器”(DIR-V)发展规划，同时，印度的电子信息部（相当于我们国内的工信部）以部的名义加入了RISC-V国际基金会，并成为了高级会员。

**欧盟在 2022.9月8号发布了《关于建立欧洲开源硬件、软件和RISC-V技术主权的建议和路线图》报告**，支持RISC-V与开源硬件，尤其给出了九大优先发展关键方向，并给出了实施路径，包括建立非盈利机构来支持研发、实施教育政策和措施等等。由此可见，全世界都在积极投入RISC-V生态建设。

![](img/7b7e123ae81c58f1a162b58105235b15.png)

![](img/bde32623dfe336d84ffbf2cd8d2f2e30.png)

在RISC-V生态建设上，中国处于第一梯队。尤其是2018年以来，我可以看到很多企业都在推出各种各样基于RISC-V的芯片产品。同时，地方政府也出台一系列政策。特别是北京政府，在RISC-V方面有很大的投入。

![](img/eff4dbecfefe41a03ce85bcc191fec2f.png)

![](img/8d47a7e894cba26006444bfe721dd6da.png)

# riscv chips

[开源 GPGPU](https://dwpl6xgouw.feishu.cn/wiki/Zx2Aw9EJdiPnqikQCNrc6UEznzh)

![](img/ce81a97de9812592455638fe9df752fa.png)

## 佐治亚理工 - 支持 CUDA 的 RISC-V GPU

Supporting CUDA for an extended RISC-V GPU architecture

https://arxiv.org/abs/2109.00673

2021.09

we aim to utilize these existing CUDA codes with RISC-V devices.

More specifically, we design and implement a pipeline that can execute CUDA source code on an RISC-V GPU architecture. We have succeeded in executing CUDA kernels with several important features, like multi-thread and atomic instructions, on an RISC-V GPU architecture

![](img/8e77386002cb3cd6da791fc5767b601b.png)

![](img/87eeef67c5654e462fcc7aa6e0bb50ff.png)

## TensTorrent - Jim Keller

关于RISC-V，Jim Keller最新发声 (chip specs)

https://mp.weixin.qq.com/s/BqNp-kvbG13dQyujEKetRg

用RISC-V搭的高性能计算架构

[自己魔改RISC-V](https://zhida.zhihu.com/search?content_id=624119028&content_type=Answer&match_order=1&q=RISC-V%E6%A0%B8&zhida_source=entity)，一个小的组里面放了5个RISC-V核分别做计算和IO。

一颗大芯片可能用到几十个组，算下来用几百个RISC-V核。

![](img/3e930f8804e39aeb6b94dc28ce630e79.png)

## 中科院 - 香山

开源高性能处理器，目标就是像Linux一样。

能够被工业界广泛应用，同时又可以支持学术界去做创新。

RISC-V是一个标准规范，而香山是一个具体的实现。

https://www.zhihu.com/question/466393646/answer/1955410750

香山：开源高性能 riscv 处理器

首届 RISC-V 中国峰会 2021 年 6 月 21 日

![](img/7319aa90aa78e7617b138181e323f7b9.png)

![](img/39a67d81764133298a0a800e685207fb.png)

![](img/364aeb25327d16ddf73f760968f15ada.png)

![](img/a84bbf471095664ca833d1494e92c43b.png)

![](img/dfc5f8b407b46e4548cc7c10c9e6b2d0.png)

源代码是开源的，同时开发香山的整个平台和工具也是开放的。

香山，就是要把所有的这些内容都开源开放。用冰山做类比，芯片设计可看做是冰山水上面的部分，那冰山水下面的部分就是如何设计芯片所需要一系列的基础设施、测试及验证工具等。

![](img/335ee4a72c84dd374e10039aa922dd84.png)

![](img/78cadcdd4090af1b7165a4e9677c1833.png)

**雁栖湖”架构是一个11级流水、6发射、4个访存部件**的乱序处理器核。在发射宽度上已经可以和一些ARM高端处理器核相当，但还未进行充分优化，因此实际性能还有不小的差距。我们希望未来通过持续迭代优化（“南湖”-->"X湖"-->"Y湖"-->……），性能达到ARM A76的水平。

![](img/d486c63a3e2540c8de767f7a25be879a.png)

![](img/bc7ccda41b9ea23e1be4155307157e2b.png)

香山开发初期速度非常快：**6月11日建立代码仓库，7月6日乱序流水线便已完成，能正确运行CoreMark**，不到一个月时间；9月12日，Linux正确启动；10月22日，Debian正确启动。

接下来便是大半年的结构优化、**性能调优**、时序优化工作，香山架构几乎相当于重构了一遍。一个典型的例子，香山的第一版分支预测器（BPU）参考了BOOM的BPU，但后端评估频率只能达到800MHz（28nm）。于是负责BPU设计的勾凌睿在几位老师的指导下，不断优化BPU结构，最终将频率提升到了1.4GHz。

这期间，小伙伴们纷纷自己动手，**开发了各种各样的优化和调试工具**，大大地加速了优化和验证环节。这让我真心佩服这批90后——他们真是充满了创造力，从工作到生活，而主要驱动力之一就是“省（tou）时（lan）”。比如宁可自己写个程序自动点外卖，也懒得打开手机看菜单点。

![](img/f08fe71a2153c1a92ad2d7592da20aec.png)

香山的开发至少有两个重要的决策，第一个便是选择敏捷设计语言Chisel。很多人质疑Chisel，排斥Chisel，但是我们在充分评估后，还是决定使用Chisel。

我们团队是在2016年开始使用Chisel，一开始组里也充满质疑。2018年，我们设计了两组定量的对比实验，找了2位同学用Chisel、1位工程师用Verilog分别设计一个L2 Cache模块。通过一系列量化对比，得出了如下三个结论：

1. Chisel开发效率远高于Verilog；

2. 实现相同的功能，Chisel代码量仅为Verilog的1/5（因此香山的5万行Chisel代码相当于25万行Verilog代码）；

3. Chisel的开发质量不比Verilog差。

后来将实验结果发表在2019年1月的《计算机研究与发展》。最近去华为交流，才知道这些对比结果也推动了华为内部组建了Chisel开发团队，如今华为也是Chisel的支持者。

2020年，我们又基于Chisel完成了一款8核标签化RISC-V处理器的流片，这是基于Rocket处理器核进行了标签化体系结构改造，采用28nm工艺流片。虽然因为时间紧张，并没有进行细致的后端优化，但芯片返回后也还能正常运行在1.2GHz。这是一颗有一定复杂度的8核SoC芯片，但Chisel能应对。所以，我们相信Chisel可用来开发复杂芯片。

![](img/3384ae58fd412709267132047db42598.png)

另一个重要决策就是高度重视构建支持敏捷设计的流程与工具。

![](img/31862552b92fb124f60bc22deab07d43.png)

![](img/9d8bee09ab63789af337697062fdd27a.png)

![](img/4b833d4f108c25811c2cf82030cb26c1.png)

目前(2022 年)香山在全世界的开源芯片中性能最高，在这个基础上，开芯院进一步确定了“两核”发展目标：第一个是经典核，能够适用于中高端工业、泛工业技术平台的处理器核，对标ARM A76；第二个核是高性能核，主要应用在高性能场景，比如数据中心、算力基础设施等，对标ARM N2。

![](img/b58a8694bcfde8b0202751989673d67f.png)

![](img/f56d1fc16a9ee7fcf397c8447fa87a3d.png)

## 平头哥 - 无剑 600，玄铁 C910

2022 RISC-V中国峰会，平头哥 发布 无剑 600。

是一个 高性能RISC-V芯片设计平台，不是芯片。

默认处理器是玄铁C910，即平头哥在2019年发布的主频2.5GHz的高性能RISC-V核。

将RISC-V的商用性能，**从1GHz推向2GHz**。

无剑 600 平台设计图

![](img/d1da47e77845eac72241c36d527fd506.png)

在AI加速方面，无剑600平台中，最新版的玄铁C910集成了矢量`Vector`处理器，支持`FP16`等新型数据类型，并可提供高达4TOPs的`Int8` AI算力，方便AI加速类应用的开发。

无剑600平台还重点实现了**CPU+XPU异构架构设计**，**从运行功耗**、内存带宽和软件栈方面都进行了优化，并支持适配GPU、NPU、VPU、DSP、ASIC等多种不同功能的第三方IP。

平头哥基于无剑600平台成功“打样”——SoC原型曳影1520。

![](img/d35249fcab650d9582680e9759238ce3.png)

**曳影 1520 是一颗多模态AI处理器SoC**，内置4核玄铁C910，最高吞吐率4266MT，AI峰值算力达到4TOPS，支持全链路安全防护，具有4K视频处理能力，同时包含丰富的异构计算和高速IO资源，软硬件都已完成应用的适配。

## 芯来 - 蜂鸟 `e200` / `e203`

https://github.com/riscv-mcu/e203_hbirdv2

MCU级别的开源RISC-V处理器核，可运行一些实时操作系统。

`e200` 是国内最早的开源RISC-V核，由胡振波（芯来科技创始人）带领团队开发。可以买到相应的书《手把手教你设计CPU——RISC-V处理器篇》

![](img/1a2933878851d80b0b19e033b900b8fd.png)

## 国科大 - 果壳（`NutShell`）

https://github.com/OSCPU/NutShell

设计文档：https://github.com/OSCPU/NutShell-doc

第一期“一生一芯”计划五位本科生主导开发的一款开源RISC-V处理器核，功能比较完整，可以运行Linux/Debian。果壳有一些应用，华为的OpenEuler也成功移植到果壳处理器上。

## Ventana 公司 - Veyron

source: https://mp.weixin.qq.com/s/FyM6LCjwiAmxe4f0rH5Lvg

VT1 每核性能据说是比肩于Arm Neoverse V系列的——也就是Neoverse中更高性能定位的系列——亚马逊Graviton 3和英伟达Grace分别基于Neoverse V1和V2。

CPU可选配最高128个核心，功耗<300W。如果宣传数据属实，则其性能水平可以比肩AMD Genoa。SemiAnalysis在最近的评论文章里说，就算按照宣传数据打八折来计，则其性能仍然超过了Intel至强Ice Lake和才刚刚见到影儿的Sapphaire Rapids。

![](img/7e49e30eda539cdbf5e0d995eca829a0.png)

据说Ventana接下来要推的VT2核心效率还能做到更高，尤其面积效益表现很出色——单位面积内的性能传言很好，则成本方面的收益也就更高了。与此同时，现有方案中与VT1搭配的IO die，已经用BoW实现chip-to-chip互联，1Tbps双向带宽，PHY-to-PHY连接延迟<2ns，传输能好效率达成<0.5pj/bit。(有兴趣的同学可以去了解一下Sapphaire Rapids采用EMIB封装方案后的这几个数据;此外，Ventana也是Intel IFS Accelerator项目的合作方之一)

![](img/d1a183a42177e99264b3915848723de4.png)

Ventana面向客户主要提供的是chiplet，而且本身似乎是不提供IO die的，IO die来自第三方或合作伙伴。这种不是出售整个芯片的方案，提供了大量可能性。比如光是IO die就可以采用现成方案，或者采用授权IP来自行开发。

Ventana现阶段主要是用Open Compute Project的ODSA BOW标准来做封装，与此同时也计划在未来版本中实现对UCIe这个更具潜力标准的支持(据说今年下半年);chiplet/IO die现在都使用AMBA CHI协议。整体上还是体现出相对开放的灵活性。

## 进迭时空 DSA

https://zhuanlan.zhihu.com/p/657584867

简单设计流程如下：

在设计之前，需要达成加速的特定领域及其需要加速的倍数的技术目标。以视觉AI为例，简单说明如下：

1. 视觉AI中的特定应用，就是CPU锚定的算力级别下，需要支持的所有网络模型。通常是如ResNet，Yolo等标准模型。

2. 通过`qemu`和进迭时空自己扩展的性能分析工具，得到计算热点和计算特征。当然，视觉AI中的计算热点必定是卷积和访存，计算特征必定是矩阵乘法。

3. 基于`qemu`得到的数据，进行DSA设计。设计相关指令或者协处理器，以达到指令数的要求。

4. 基于新定义的指令或者协处理器，优化应用。

5. 通过`qemu`重新统计优化后的指令数，和原始指令数对比判断，是否达到指令数要求。

6. 若没有达到，则需要重新对DSA和应用进行迭代优化，直至达到要求。

7. 指令数达到要求以后，基于`gem5`进行微架构的优化，如计算热点中的访存。

8. 通过`gem5`的`perf`工具，统计cycle数。

9. 若cycle数没有达到加速目标，则需继续进行微架构优化，直至满足性能要求。

通过以上步骤，DSA初稿基本设计完毕，后续会和硬件同学一起探讨RTL的落地实现。

# 设计挑战

https://www.zhihu.com/question/605427422/answer/3068622311



riscv `core` 没什么好做的，做好了也没人关心，`core`就是十级流水`OoO`多发射，也不如一个`multi-port`读写带`coherency`的`pipelined L2`让人亮眼。

现在不管多少级流水`OoO`与否多发射与否的开源`core`遍地都是，`Rocket generator`输入后端你要几级的实现，前端各个部件几级的实现：`BOOM`！就出来了（pun intended）。`VRoom`还有`SMT-2`的`RV64-IMAFDCHBK(V)`高性能实现。至于常见`uncore`，`rocket`现在包含了`amba/tilelink/accelerator接口/etc`。

但你有没有发现所有这些开源实现，花里胡哨什么都有`SMT-2`都有，`L1`+`两级``TLB`打包给你，为什么唯独都缺乏`L2`实现？教材上`IEEE754`，多级`tournament BPU`还有`systolic array`什么都讲，为什么唯独略过`pipelined L2`不讲？

有个原因是`L2`设计高度捆绑`vendor`的`SRAM library`，还有个原因是很多`L2`重要方法现在都封在专利和`NDA`里。一个八九级`pipelined L2`怎么处理多`cycle`多`port`之间的`request`，怎么处理`outstanding misses`怎么打包发给`LLC/dram controller`，怎么做`pipelined coherency`。解决这一切再优化`throughput`怎么做，压低`request cycle`数怎么做。这些问题非常有趣，不亚于一个十级流水`OoO`多发射的困难和工程量，而且学到的技能还是至关重要。（你可以想象`multi-port L2`的每个`port`都是一个发射而且贯穿整个`request-response closure`，不像`core`的多发射只需要操心一两级`pipeline`的影响，之后他们就解绑了）

从商业价值来说，`L2`更是重要。`core`里面花里胡哨搞得再多，什么`ai` `dl` `bnn`加速器的，没有个耐操量大的`L2`和`dram controller`，全都得空等干烧`clk`。谷歌那篇经典`ISCA15`对全`data center`机器做的`profiling`说，至少60%的`cycle`都是后端等数据。`branch predictor`是重要，但从教材上抄个多级`tournament`就是98%左右的正确率，再做也是去小数点后扣，你能学到的技能也就是如何平衡多种多级`predictor`之间不要互相干扰。但这边后端60%的开销，能做的事和做出来的价值，自然更大。所以教材和开源资料随便讲`core`玩花，反正那些人尽皆知也无关商业价值，但一到`L2`就是专利+`NDA`绝口不提。

有失偏颇了，如果把预取器、一致性、高带宽全一起，那可以吹一下……真正做的话流水线里还是有很多东西的，不是`boom`做得简单就只有那么简单。

`L2`设计高度捆绑`vendor`的`SRAM library`。就是和代工厂提供的现成的模块很有关系

`OpenSPARC T1/T2` 的`RTL`很有参考性。但这个学习曲线可能陡了点。

`10-stage pipline` + `OoO`的工程量，非常大。虽然，创新性可能有限。

## ISCA15 - 服务器芯片的前端结构

https://dl.acm.org/doi/10.1145/2749469.2750392

![](img/3af8cc15d3684135b50c6dd515488417.png)

`ISCA15` 的工作，恰恰是在强调前端的重要性在变大。

自从那篇文章之后研究服务器芯片前端结构的文章井喷了。

## 落地的挑战

`RISC-V`核在手机或电脑`SoC`上使用，还有遥远的距离。一方面：`RISC-V`开源的是单个核，没有类似基于`ARM V8`、`V9`架构的`SoC`设计及`benchmark`评估，简单的说：没有类似 `ARM`这样一个具备强大`CPU`设计和`SoC`设计能力的专业公司。

`RISC-V`生态中，`SiFive`、`Andes`、阿里平头哥、中科院计算所等公司和实体是`RISC-V`核扩展设计`IP`的提供商，但它们都和`ARM`的设计实力相去甚远，特别是`SoC`设计能力更是没有太多有效的`benchmark`评估，再加上整个软件工具链还存在各种各样的问题。 简单的说，它们能提供单核的扩展设计及支持，更进一步的能力还在持续的积累中。

目前`RISC-V`主要还是用在弱生态领域，例如可穿戴、物联网，以及潜在的高性能计算。

`SiFive`的`RISC-VIP`核，计算能力和`ARM`只差半档，这个设计水平非常高，也因此，一定非常烧钱，但这些烧钱并没有换回来太多的商业化变现。

![](img/35de08c7a1f0f701530295ddf1c97a40.png)

![](img/2a49d33baabc52bf6ad21e63de43c006.png)

## riscv 做 GPGPU 的反对声

https://www.eeworld.com.cn/qrs/eic665414.html

用`RISC-V`做`GPU`，有人拍手叫好，也有人觉得多此一举。

觉得它好的人，认为`RISC-V`可以直接套用`RISC-V`的生态，不需要完全自建`GPU`生态，规避了与`CUDA`的专利对决。毕竟这是个开源的`GPU IP`，大家都加入到开源建设的大家庭，就能够弥补`GPU`生态不足，也不容易踩到知识产权的坑。

与此同时，每个`SM`核的独立性更好，可以执行比常规`GPU CUDA`核更复杂的计算。如果加上`MMU`，未来甚至可以在每个`SM`核上跑操作系统，非常有利于虚拟化。

觉得它不好的人，认为目前的`RISC-V GPU`产品，都是目前`NVIDIA`的`GPU`的形似物，设计没有什么太大差别，而`HBM`这样的器件八成是用不起的，其它诸如`Nvidia GPU`里面的`Texture Unit`也是缺失的。

更何况，`CUDA`这样的生态是很难跨越的鸿沟。有大佬曾言：“如果你看一下`GPU`的生态系统，就会发现工具链是由`NVIDIA`控制的，包括[`AMD`](https://www.eeworld.com.cn/zhuanti/vXHG0K)在内的其他竞争对手都试图打破垄断，但失败了。通过扩展`RISC-V`标准指令集，几乎不可能与不断更新的`NVIDIA`生态系统兼容。另一方面，也很难重新开始，因为英伟达拥有先发优势。”

所以，对于大部分`RISC-V GPU`来说，支持`CUDA`生态至关重要，我们也看到许多厂商确实也是这样做的。因为虽然通过`LLVM`可能只需要重构`compiler`后端，但性能好不好就是另外一回事了。

还有比较激进的观点，认为从`RISC-V`开始做`GPU`，不如直接从白纸开始做。他们认为，要使基本的`RISC-V`指令集适应`GPU`任务，需要大量投资来定义自定义`ISA`扩展，构建高度复杂的微架构更改，并对开源工具进行大手术，使它们几乎与原始工具相似。这样一来，使用`RISC-V`的几乎所有潜在价值，都被定制架构比下去了。

就连上海交大也曾经说过，“我们想做的是一个独立完整的`GPGPU`架构和指令设计，而不依赖于现有`RISC-V CPU`开源指令，这样才能更完备地打造和融入原生的`GPGPU`生态 ，从而与现有的各种`CPU`系统无缝衔接。”所以因此，他们做了“青花瓷”这样一个定制的开源`GPGPU`架构。

但多一条路线，总归是好事。毕竟`RISC-V CPU`发展迅速，如果`RISC-V CPU`发展好，`RISC-V GPU`就能跟着“发财致富”。这条路好不好，就全看`RISC-V`能发展成什么样子了。

https://mp.weixin.qq.com/s/eaw_DI0G7p7FxA1aTd4ivw

`Quadric`工程副总裁 `Dhanendra Jani`

`RISC-V` 为采用者带来的价值在于控制处理领域，它拥有现成的开源工具、现成的操作系统（`Linux` 或实时）以及提供的长期软件兼容性/可移植性的承诺通过 `ISA` 通用性。

要将基本 `RISC-V` 指令集调整为一种非常适合 `GPU` 任务的指令集，需要大量投资来定义自定义 `ISA` 扩展、构建高度复杂的微架构更改，并对开源工具进行重大手术，使它们与原来不同。这样做时，使用 `RISC-V` 的几乎所有固有价值都因广泛的定制而消失了，您将失去大部分优势，同时可能会受到核心 `ISA` 功能的束缚，这些功能限制了特定领域 `GPU` 环境中的有用性。简而言之，从 `RISC-V` 开始而不是从头开始有什么意义呢？

`RISC-V International` 首席技术官 `Mark Himelstein`

`RISC-V`的计划是什么？

矢量是 `SIMD` 操作，使您能够同时对多条数据进行操作，并让芯片找出从内存中取出数据的最佳方式，处理单个指令，然后将数据放回内存，或者将它们转移到下一个操作，缺少的基本内容是矩阵乘法。

我们收到了多个提案，其中之一类似于适合 32 位指令的向量扩展。这非常困难并且需要设置说明。您设置了`stride`和`masks`等内容，然后扣动扳机并执行操作。但如果您想与其他架构上更大的矩阵实现竞争，则必须采用更宽的 64 位指令。这是很多人都在谈论的话题。

`Arm`客户端业务线产品管理高级总监 `Anand Patel`

问题是有多少复杂性被暴露，有多少仍然隐藏。“`ISA` 是一个关键组件，”

然而，`GPU` 的复杂性通常由 `Vulkan` 或 `OpenCL` 等标准 `API` 抽象出来。这使得开发人员可以更轻松地跨多个供应商进行定位，同时将较低级别的优化留给 `GPU` 供应商。即使在 `GPGPU` 类型的应用中，`GPU` 的架构也在快速发展，以跟上人工智能处理等新兴用例的步伐，因此开发人员能够访问成熟的软件生态系统来跟上这些变化至关重要。标准 `API` 确保开发人员不必担心 `ISA` 更改，而是透明地看到这些底层改进的好处。

区分 宏观架构和微架构，很重要。

`RISC-V` 仅定义了宏观架构，而将所有微架构决策留给了实现者。当超越 `CPU` 时，这将成为一个更大的问题。“冯·诺依曼在某些方面有限制性，但特定实现如何与内存交互并不是由 `RISC-V` 决定的，”`RISC-V` 的 `Himelstein` 说。“大多数 `GPU` 实现都通过多级管道中的内存来优化这一点。当一些操作正在进行时，一些东西来自内存。当您开始研究 `GPU` 时，您会谈论暴露内存交互。我们确实对事情发生的顺序有一些限制，因为你想确保操作被明确定义。”

看待问题的方法有很多种。“最先进的 `GPU` 产品可以按照传统图形处理和现代人工智能加速来划分，”`Stream` 的 `Fan` 说道。“前者更像是可编程`ASIC`，而不是通用处理器，其核心能力来自于流处理器的实现，而不是`ISA`。指令集通常对程序员来说是不可见的，并且总是处于次要位置。图形处理器的设计与微架构密切相关，适合用定制指令来实现。对于我们大多数人来说，人工智能和多媒体功能的标准化更具吸引力。要实现这样的能力，复制`GPU`并不是唯一的方法。对于`RISC-V`来说，多媒体功能可以通过矢量架构来实现，`AI`能力可以通过更高效的异构架构和矩阵加速器来实现。”

如果您希望外部程序员为您的设备编写软件，则某些方面会发生变化。“数据流处理可以通过多种方式完成，”西门子的`Klein`说。“一种是使用小型通用处理器甚至专用处理器的管道，每个处理器都处理问题的一个阶段。这比单个大型 `CPU` 更快、更高效。使用可编程处理器作为计算元件保留了很大的灵活性，但确实放弃了一些性能和效率。这种方法确实可以在任何有能力的多核处理器上构建。问题是这种方法已被软件开发社区彻底拒绝，他们不愿意放弃单线程编程模型。”

这对于很多企业来说都是一个大问题。“如果您正在寻找通用处理器，根据应用要求，它可以是从单级、两级或三级微控制器一直到运行多级的多级超标量设计。`Bluespec` 的 `Hauck` 说道。“或者，你会看到人们拥有 4096 个 `RISC-V` 处理器，每个处理器都是小型、简化的 `RV32I` 类型的东西，它们被组合在特定的系统架构和互连中，使这些东西能够按照 `GPU` 的精神运行。它们由许多较小的整数单元组成，这些单元共同协作完成一项艰巨的任务。挑战在于如何为此开发软件？”

有了更大的灵活性，可能需要新的方法。“在大型 `HPC` 中，如果您运行的工作负载更加面向数据中心，则它具有一组特定的特征。但如果您的应用程序是科学应用程序，也许有一些关于加载和存储以及多种数学类型运算的功能可以扩展，”西门子 `EDA` 首席产品营销经理 `Andy Meyer` 所述。“如果人们选择这条路线，生态系统就会面临一些挑战。最大的增长领域是超大规模应用。如果你看看风险投资的金额，你就会发现他们显然正在解决一个独特的问题。”

## riscv hpc 的挑战

https://www.zhihu.com/question/557765158/answer/2762129806

`RISC-V` 后来者的优势与不足

`C[ali](https://zhida.zhihu.com/search?content_id=532292467&content_type=Answer&match_order=1&q=ISA&zhida_source=entity)sta Redmond`说：“`RISC-V`固有的持续性高性能和代码效率优势，不同于任何其他架构，比如小型的基础`ISA`，不同于其他各类专利`ISA`，我们只需要一个确认的基础`ISA`，就可以与各种开源和商业扩展件相对接，这使得使用者可以达到行业先进水平，从而在设计上取得竞争优势，这是`RISC-V`跨领域的进展。”

“此外，在其他架构中，开发者可能需要花费数年时间来获得访问代码空间的授权，而`RISC-V`拥有设计的灵活性和自由度，且还预留了代码空间，企业可以定制独特的设计并推向市场。” `Calista Redmond`补充道。

嵌入式系统分会理事长何小庆老师则认为，`RISC-V`具有三大优势：“第一，开源的生态让嵌入式系统的开发者可以触摸到芯片的架构，即使不参与芯片的设计，也能去参与设计一个扩展的指令集，从而实现用特殊的一个两条指令去完成原来占用处理器50%消耗的任务；第二，`RISC-V`只有四十几条基础指令集，却可以做出覆盖从`8051`到`ARM A系`的各种各样的嵌入式处理器，当然也可以加一些拓展，比如说做人工智能的`V`指令；第三，`RISC-V`比目前同类的处理器架构更安全，因为它的处理器模式有更多的选择性，比如`M模式`、`U模式`和`H模式`。”

如果我们拿同级别的`ARM A55`内核的处理器`[和平头哥最新](https://zhida.zhihu.com/search?content_id=532292467&content_type=Answer&match_order=1&q=%E7%8E%84%E9%93%81C908&zhida_source=entity)`发布的基于`RISC-V`架构的玄铁`C908`相比，可以发现玄铁`C908``[不仅拥有更高](https://zhida.zhihu.com/search?content_id=532292467&content_type=Answer&match_order=1&q=Vector&zhida_source=entity)`的能效，还能兼容更为灵活高效的`AI`能力，除了平头哥对数据格式和算法等方面优化的功劳外，还因为`C908`原生支持`Vector`，所以在`Vector`上肯定比`ARM`做得相对更激进、更前沿，这些都是后来者的优势。

孟建熠表示：“玄铁`C908`可以覆盖1`TOPs`以下的`AI`算力需求，不需要自己再做一个`NPU`，大大减轻了各种`NPU`与`CPU`适配的工作。此外`ARM A55`可扩展至128位，而`RISC-V`一般是从128位起，可以扩展至256位，所以性能也会更高。”

而对于`RISC-V`的不足之处，还是老生常谈，相比`ARM`来说，`RISC-V`的生态还处于相当初级的阶段，且具有行业碎片化的风险。

然而，孟建熠对`RISC-V`碎片化却有着不同角度的解读，他认为：“一定程度上上的多样化和差异化是`RISC-V`架构有生命力的体现，有利于推动技术的创新。技术能否被生态接受的最终决定性因素是市场，看下游客户的接受程度有多高，能否在生态圈里形成强大的影响力。一个不被市场接受的技术，持续演进的成本很高。”

## arm 的经验

https://zhuanlan.zhihu.com/p/580895534

2012 年，`ARM V8` 发布了，很多人理解它只仅仅理解为一套指令，是完全不对的。

`ARM`一次性完整地（注意这个完整性说法）把`64b`、`虚拟化`、`SMP`扩展、`ACPI`、`server`级`RAS`和`SECURITY`的框架发布了，其中有很多特性很不完备，是的，留白的，但`ARM`作为生态的唯一主导者做出的最大贡献，就是先把一个人该有的骨架全搭好了。

`[而RI](https://zhida.zhihu.com/search?content_id=217221754&content_type=Article&match_order=1&q=DSA&zhida_source=entity)SC-V`的主线，源自`Patterson`教授对`DSA`的预期，并无一个宏观视角下对`server`领域的全局主线。以`GC`为最小基本面，各领域随意延展，放飞自我，`RISC-V`的发展其实是如下图的样子，不过也许这本身就是`Patterson`教授对初心？

`ARM`从`v8`发布开始，就伴随了非常多的机缘巧合的事和人，这些事情，怎么看都是不可能重复发生的。

+   `ARM`在生态上的强权和领先的认识力。`ISA` 的演进几乎`R-G`一个人说了算，其余众人都只能提案

+   恰到好处的几个人，建立了恰到好处的生态组织，并且建立了各家愿意遵从的标准（可能还有`intel`的助力）。

+   各具有实力但又不够独大的诸方参与者，屈从于共同的敌人和利益目标而主动愿意相互合作。

+   相当多还不能说的国际友人，明里暗里出于对全球化技术共产主义的追求而提供的各种帮助。

+   还有那些苦`intel`旧矣的敌后地下党员……

这些要素在`RISC-V`当下，一个都没看到。也很难复现。

虚拟化是由大约一百多个细节的`feature`堆砌组成的。

很多人误以为`RISC-V`已经有虚拟化扩展这个`ISA extension`好像就支持虚拟化了，实际上完全不是。2012年`ARM v8`就支持虚拟化了，可直到今天，我们做下一代`ARM Server CPU`还在吵哪些`feature`是必须加的？哪些可以放弃？

举个浮点计算的例子，最简单的，浮点计算遵从`IEEE754`标准，我猜任意一个`RISC-V`的同学都会跳出来说，“`compatible`！”，很遗憾，我做`ARM`到现在还没能做完备，`IEEE`有`FP80`这种`FP64`的扩展浮点，并被`x86`指令集的`X87`指令提供出来，过去有相当数量高精度浮点计算会使用它（就这事被客户投诉，领导念了半年）。

你说，我可以不支持啊。但是通用处理器的通用两个字本身就是无数的细分市场聚合的，你这个缺，那个缺，那那个也放弃，最终就是个`DSA`啦（也是`Patterson`希望的吧）。

`risc-v` 因为目标碎片化原因无法在最新工艺上做好`DTCO`。

都是`RISC`指令集，同样的性能，为啥`RISC-V`能比`ARM`小？



如果RISC-V真正把指令完备性、虚拟化、安全、RAS等等细节都补齐，小不了，当今的通用CPU core设计复杂度早就不是一个OoO了，而是无数细枝末节的完备性。当你加了这么多特性之后，算算付出的设计、验证的代价，也许，还不如买一个更便宜。

上面都是第一层逻辑。

但如果真有一天，我们达成一致，一定、必须要在数据中心把RISC-V扶成主流。

就需要启动隐藏的第二层或者第三层逻辑：

放弃虚拟化、硬件的RAS、安全……

`rvv`的难点在可变向量在乱序执行下的实现

# IREE - Compilation flow

## env setup

```
pip install \
  iree-compiler \
  iree-runtime

# llvm, using tags/llvmorg-17.0.0
# llvm/build lib & bin are required
$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/<yourpath>/llvm-project/build/lib
$ export PATH=$PATH:/<yourpath>/llvm-project/build/bin
```

## torch model -> STABLEHLO (MHLO)

using `torch_mlir`

```
module = torch_mlir.compile(
    model, data,
    output_type=torch_mlir.OutputType.STABLEHLO, use_tracing=True)

with open(out_stablehlo_mlir_path, "w", encoding="utf-8") as outf:
    outf.write(str(module))
```

## StableHLO -> IREE binary

(Intermediate Representation Ecosystem for eXecution)

using `iree-compile` command-line tool

```
iree-compile \
    --iree-hal-target-backends=llvm-cpu \
    --iree-llvm-target-triple=riscv64-unknown-elf \
    --iree-llvm-target-cpu=generic-rv64 \
    --iree-llvm-target-abi=lp64d \
    --iree-llvm-target-cpu-features="+m,+a,+f,+d,+zvl1024b,+v" \
    --riscv-v-fixed-length-vector-lmul-max=1 \
    --riscv-v-vector-bits-min=-1 \
    --iree-input-type=stablehlo \
    bert_tiny_stablehlo.mlir -o bert-tiny.vmfb
    
unzip bert.vmfb
# .so file contains the RISC-V executable kernel along with some glue logic for the IREE runtime environment
```

`--iree-hal-target-backends=llvm-cpu`

LLVM backend for CPUs

`lp64d`:

ILP64 data model, where long and pointer types are 64-bit,

`d` for double-precision floating-point operations.

base integer instructions `(+m)`, atomic instructions `(+a)`, single-precision floating-point instructions `(+f)`, double-precision floating-point instructions `(+d)`, and vector extension `(+v)` with a maximum vector length of 1024 bits `(+zvl1024b)`.

`--riscv-v-fixed-length-vector-lmul-max=1` `--riscv-v-vector-bits-min=-1` flags set custom constraints for the RISC-V vector extension, such as maximum LMUL (length multiplier) and minimum vector bit width

To inspect the contents of the ELF (Executable and Linkable Format) file, we can use the following command:

```
llvm-objdump -d bert_linked_llvm_cpu_embedded_elf_riscv_64.so > bert.s

# xxxx.so:   file format elf64-littleriscv
# Disassembly of section .text:
```
