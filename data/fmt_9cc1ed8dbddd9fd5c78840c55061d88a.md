
# FPGA 和 AI 处理器：DNN 与 CNN 之争

Koniku 的网站正在倒计时，现在还有 20 几天。我已经等不及了。他们没有融到多少钱，而看过它们在福布斯上的这个视频之后（`https://goo.gl/VA1PJx`），你很可能也不得不相信他们，但你也无法预料究竟会如何。利用生物细胞肯定是不一样的。听起来就像是一个科研项目，但他们这样说：

> 「我们是一个企业。我们不是一个科研项目。」下周将在维也纳的 Pioneers Festival 上发表演讲的 Agabi 这样说，「今天有些需求是硅所不能满足的，而我们可以通过我们的系统提供。」

> Koniku 提供的核心是所谓的神经元壳（neuron-shell），这家创业公司称其内部可以控制神经元彼此通信的方式，加上一种正在申请专利的电极，就可以在神经元上读写信息。所有这些都可以装在一个 iPad 大小的设备里，他们还希望能在 2018 年之前将其减小到一枚五美分硬币大小。

## Adapteva

Adapteva 是我最喜欢的一家小技术公司，正如你在之前文章《Adapteva tapes out Epiphany-V：一款 1024 核 64 位 RISC 处理器》：`https://goo.gl/6ZH7JP`。去年年底时 Andreas Olofsson 拿出了他的 1024 核芯片，我们都等着看它的性能表现。Epiphany-V 有用于深度学习的新指令，我们必须要看看这种带有 64MB 片上内存的更少内存控制器的设计是否具有合适的扩展能力。Andrea 的设计和构建的出色效率可能能让我们真正负担得起这种芯片，所以让我们希望它有良好的表现吧。

## Knowm

Known 研究的是 Anti-Hebbian and Hebbian（AHaH）可塑性和忆阻器。这里有篇覆盖这一主题的论文《AHaH 计算：从 Metastable Switches 到 Attractors 到机器学》：`https://doi.org/10.1371/journal.pone.0085175`。这对我来说有点太高级了。简单看了看，我看不出这项技术和胡言乱语的区别，但看起来确实科学味道十足。我需要亲眼看到才能相信。神经忆阻式处理器（neuromemristive processor）的思想是很有趣的。我早上确实需要一个好的流行术语。

## Mythic

Mythic 的一款电池驱动的神经芯片具有低 50 倍的功耗。目前还看不到太多真正的细节。这款芯片大约纽扣大小，但大多数芯片不都这样吗？

> 「Mythic 的平台能在纽扣大小的芯片上提供桌面 GPU 的性能。」

也许这又是一款适合无人机和手机的芯片，很可能被用在手机中，也可能被排除在外。

## 高通

手机显然是机器学习硬件的一大用武之地。我们希望能够识别狗的品种、花朵、树叶、癌症痣、翻译标识、理解口语等等。我们口袋里的超级计算机愿意用上它能得到的所有帮助，以便能迈入感知时代。高通一直以来都在鼓捣机器学习，推出了 Zeroth SDK 和 Snapdragon 神经处理引擎（NPE）。这种 NPE 显然在高通所用的 Hexagon DSP 上效果良好。

Hexagon DSP 已经远远不止是一个非常广泛的并行平台了，Yann LeCun 已经证实高通和 Facebook 正在合作开发一种更好的方法，参见 Wired 的文章《业界 | 谷歌 TPU 之后还有高通，人工智能芯片竞赛已经展开》：

> 「最近，高通已经开始制造执行神经网络的专用芯片，这条消息来自 LeCun，因为 Facebook 正帮助高通开发机器学习相关技术，所以他对高通的计划很了解；高通技术副总裁 Jeff Gehlhaar 证实了这个项目，他说：『在原型设计和开发方面，我们还有很长的路要走。』」

也许我们很快就会看到 Kryo CPU、Adreno GPU、Hexagon DSP 和 Hexagon Vector Extensions 之外的其它东西。对于这一领域的创业公司来说，和高通的机器学习竞争将会艰难无比。

## Pezy-SC 和 Pezy-SC2

这两者是 Pezy 开发的 1024 核和 2048 核处理器。Pezy-SC 1024 核芯片可以驱动 2015 年 Green500 超级计算机榜单的前三名的系统。Pezy-SC2 是现在已经开始提供的后续芯片，我在 6 月份也做过关于它的演讲，但相关细节还很少，不过仍然很吸引人：

> 「PEZY-SC2 HPC Brick：单个机箱中有 32 个带有 64GB DDR4 DIMM（2.1 PetaFLOPS（DP））的 Pezy-SC2 模块卡，速度可达 6.4 Tb/s.」

不知道 2048 个 MIMD MIPS Warrior 64 位核的机器能做到什么？在 6 月份的 2017 年 Green500 榜单中，一个英伟达 P100 系统拿到了头把交椅，而排名第 7 的是一个 Pezy-SC2 系统。所以看起来这款芯片还活着，但相关细节却很少。Motoaki Saito（齊藤元章）当然值得一看。

## Kalray

尽管做了很多承诺，但 Kalray 的芯片还没有超过 256 核，我在 2015 年的一篇文章就谈到：`https://goo.gl/pxqn7Z`。Kalray 宣传自己的产品说是适合嵌入式自动驾驶汽车应用，但我觉得其目前产品架构的形式并不是一种完美的 CNN 平台。Kalray 有一个 Kalray Neural Network（KaNN）软件包并且宣称有比 GPU 更好的效率，在芯片上能实现高达 1 TFlop/s 的速度。

随着即将到来的产品更新，Kalray 的神经网络财富可能还会提升，就在这个月 Kalray 完成了新一轮 2600 万美元的融资。他们新的 Coolidge 处理器预计将在 2018 年年中上市，它会带有 80 或 160 个核，另外还有 80 或 160 个为视觉和深度学习优化的协处理器。

这在他们的多于 1000 核的方法上有了很大的改变，而我认为这是最明智的。

## IBM TrueNorth

TrueNorth 是 IBM 的神经形态 CMOS ASIC，是与 DARPA 的 StNAPSE 项目一起开发的。

这是一种单个芯片设计上的多核处理器网络，具有 4096 个核，每个核模拟 256 个可编程的硅「神经元」，总共就超过了 100 万个神经元。另外，每个神经元还有 256 个可编程的「突触」，信号可以在这些突触之间传递。因此，可编程突触的总是超过了 2.68 亿（2²⁸）。在基本构建模块方面，它的晶体管数量为 54 亿。因为存储、计算和通信都是在这 4096 个神经突触核中各自处理的，所以 TrueNorth 避开了冯·诺依曼架构的瓶颈，而且能效非常高，功耗为 70 mW，大约是传统微处理器的功率密度的万分之一（来自维基百科）。

IBM 之前还在批评脉冲神经网络（spiking neural network）无法适配深度学习，现在 IBM 开发了一种在 TureNorth 上运行 CNN 的新算法。

这些神经元不会在每个周期内都放电，脉冲神经网络中的神经元必须逐渐积累它们的电势，然后才能放电……深度学习专家普遍认为脉冲神经网络在深度学习上没有效率——至少和卷积神经网络比起来是这样。Facebook 人工智能研究院主任和深度学习先驱 Yann LeCun 曾经批评过 IBM 的 TureNorth 芯片，因为它主要支持脉冲神经网络……

> ……这种神经形态芯片不会太激动人心，因为它们注重的脉冲神经网络在深度学习领域并不受欢迎。

为了让 TrueNorth 芯片很好地契合深度学习，IBM 不得不开发了一种新算法，让卷积神经网络可以在这款神经形态计算硬件上很好地运行。这种组合方法实现了 IBM 所谓的「接近当前最佳」的分类准确度，实验涉及了视觉和语音挑战赛方面的 8 个数据集。在最好的情况下，它们的准确度达到了 65% 到 97%。

当只使用一块 TrueNorth 芯片时，它只在这 8 个数据集中的 1 个数据集上超越了当前最佳的准确度。但如果使用多达 8 块芯片，IBM 的研究者可以极大提升这款硬件在深度学习挑战上的准确度。这使 TureNorth 可以在其中 3 个数据集上媲美或超越当前最佳的准确度。

这个 TureNorth 测试也做到了每秒处理 1200 到 2600 视频帧。这意味着单个 TureNorth 芯片就能实时检测来自多达 100 个同时工作的相机的数据的模式……（来自 IEEE Spectrum）

TrueNorth 的功率效率非常出色，所以非常值得考虑。

## Brainchip 的脉冲神经元自适应处理器（SNAP：Spiking Neuron Adaptive Processor）

SNAP 不能做深度学习，这只是一个源于好奇心的项目，还没有实际落地成 CNN 工程解决方案，至少现在还没有。如果你想探索这条路，IBM 的随机相变神经元（stochastic phase-change neurons）似乎更有意思。

## 苹果的神经引擎（Neural Engine）

到底会不会有？彭博社报道称这会是一款次级处理器，但没有什么细节信息。对苹果来说，这不仅是一个重要领域，而且也有助于与高通的竞争。

## 其它

1. 寒武纪（Cambricon）——中国科学院在芯片上投入的 140 万美元。它是一个用于神经网络的指令集架构，具有数据级的并行、自定义向量/矩阵指令、片上 scratchpad memory。宣称速度是 x86 CPU 的 91 倍，是 K40M 的 3 倍，且峰值功率仅有 1%，即 1.695W。参阅这两篇论文：

   - Cambricon：一个用于神经网络的指令集架构：`http://dl.acm.org/citation.cfm?id=3001179`

2. 前谷歌员工创立的 Groq Inc.，也许是另一种 TPU？

3. Aimotive：`https://aimotive.com/`

4. Deep Vision 正在开发用于深度学习的低功耗芯片，也许这两篇创始人的论文能提供一点线索：

   - Convolution Engine: Balancing Efficiency and Flexibility in Specialized Computing [2015]：`http://csl.stanford.edu/~christos/publications/2015.convolution_engine.cacm.pdf`

5. DeepScale

6. Reduced Energy Microsystems 正在开发用于 CNN 推理的低功耗异步芯片。据 TechCrunch 报道，REM 是 Y Combinator 第一个 ASIC 领域的风险投资。

7. Leapmind 也很忙。

## FPGA

微软已经站队 FPGA 了。Wired 这篇文章说得很好：《深度 |《连线》长文揭秘微软 Project Catapult：人工智能时代押注 FPGA》。

> 「Bing 占据着世界上 20% 的桌面搜索市场和 6% 的移动手机市场，在 Bing 上，这个芯片能帮助 Bing 适应新品种的人工智能：深度神经网络。」

我对这种方法也有些兴趣。赛灵思和英特尔（收购了 Altera）的 FPGA 是很强大的引擎。赛灵思自然宣称他们的 FPGA 是对 INT8 最好的，他们的一份白皮书里面包含了下面的幻灯片：

![](img/23ce1e6a3d300881afc0b1e9d353a03c.jpg)

这两家供应商都很支持使用他们的 FPGA 做机器学习：

- Intel FPGA OpenCL（`https://goo.gl/S62fMA`）和 Solutions（`https://goo.gl/zkYyXB`）

尽管 FPGA 单位功耗的性能是很出色的，但这些供应商的更大型的芯片的售价却长时间以来都高得吓人。赛灵思的 VU9P 在 Avnet 上的售价超过 5 万美元。

寻找价格和能力之间的平衡点是 FPGA 的主要难题。

FPGA 方法的一大优势是可以用来制定一些非常出色的架构决策。比如如果因为 HBM，你想在板上 DRAM 压缩并且实时地解压，然后希望借此改善你的内存的浮点数据流，如果你足够努力，你能找到解决方案。参阅《用于基于 FGPA 的高性能计算的浮点数值数据流的带宽压缩（Bandwidth Compression of Floating-Point Numerical Data Streams for FPGA-Based High-Performance Computing）》：`http://dl.acm.org/citation.cfm?id=3053688`。

![](img/06229dae0d775ebe52531ff1946a4687.jpg)

这种动态的架构敏捷性（architectural agility）很困难，几乎无法用其它任何方法实现。

架构选择太多可能也是一个问题，但我自己还是挺喜欢这个问题的。这篇论文很不错《使用 TILT 减少 Soft Scalar CPU 和定制硬件之间的性能差距（Reducing the Performance Gap between Soft Scalar CPUs and Custom Hardware with TILT）》：`http://dl.acm.org/citation.cfm?id=3079757`，其中研究了定制硬件和 FPGA 处理器（带有基于 FPGA 的水平微编码的计算引擎）的性能差距，这让人想起了古老的 DISC 和很多个月前的离散指令集计算机（discrete instruction set computer）。

![](img/35b088b680f9d5017910bce659be83ef.jpg)

![](img/8caa12b724fc146b6ddcc85196571520.jpg)

## 谁是赢家？

在这样的竞争中，预测谁是赢家是傻瓜的举动。高通凭借其手机市场的主导地位，可以轻松进入赢家榜单。苹果不管做什么都会成功的。英伟达的 V100 有 Tensor 单元，也基本上赢了。我不确定我能否看到谷歌的 TPU 在无休止的长期硅谷竞赛中幸存下来，尽管目前它的性能很出色。我很喜欢 FPGA 方法，但我也不禁想他们应该以远远更低的价格发布 DNN 版本，这样才不会被大众漠视。英特尔和 AMD 会做自己的协处理器。因为所有主要玩家都参战了，所以其中很多都会支持标准的工具包，比如 TensorFlow，这样我们就不必太在意规格了，关心基准就可以了。

在更小型的玩家中，我很喜欢而且支持的是 Adapteva 方法，我认为他们的内存架构可能并不适合 DNN。我希望我错了。

Wave Computing 可能是我继 FPGA 之后最喜欢的方法。他们的整个异步数据流方法是非常棒的。REM 似乎也在做类似的事情；但我认为他们可能太迟了。Wave Computing 能否在面对所有竞争对手时继续保持领先吗？也许只要他们的异步 CGRA 有一个本质优势，就可以。尽管我不确定他们是否只要 DNN 上的成功，因为他们的技术有更加广泛的应用能力。

神经形态脉冲处理器也许现在暂时可以忽略，但也可以保持关注，因为它们有很大的功耗优势。量子计算反正会让所有这些都稍逊风骚。IBM 的 TrueNorth 可能会是个例外，因为它不仅能执行脉冲网络，还能有效地运行 DNN。

![](img/772b30b2a3f8d55dd21a3f1d22cb6891.jpg)

原文链接：`https://meanderful.blogspot.jp/2017/06/fpgas-and-ai-processors-dnn-and-cnn-for.html`

**本文为机器之心编译，转载请联系本👸号获得授权。**

✄------------------------------------------------

**加入机器之心（全职记者/实习生）：hr@jiqizhixin.com**

**投稿或寻求报道：content@jiqizhixin.com**

**广告&商务合作：bd@jiqizhixin.com**
