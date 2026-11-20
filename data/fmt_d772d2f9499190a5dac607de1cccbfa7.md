
# 总线 & PCIe 总线

> 来源：[https://learning.feishu.cn/docx/J0e1dpuUko2QtjxbF73cXh3Xn5c](https://learning.feishu.cn/docx/J0e1dpuUko2QtjxbF73cXh3Xn5c)

## Summary

`总线带宽 (bit/s) = 频率 * 位宽`

PCIe 是高速串行总线，差分信号。

采用端到端的连接方式，一条链路只能接 2 个设备。

一条 PCIe 链路，可以包含多个 lane。1, 2, 4, 8, 12, 16, 32。

一般来说，当系统上电后，host会自动查询PCIE设备枚举以获取总线拓扑结构。除一些特殊系统外，普通的系统只会在开机阶段进行设备的扫描，启动成功后，即枚举过程结束后，即使插入一个PCIE设备，系统也不会再去识别它。

PCIe在现代x86体系中除了传统的连接外部设备（真的有硬连线）的作用，更多的是充当IP粘合剂的作用，并不存在符合PCIe总线标准的硬连线。

Chip 内的 IP 之间，需要相互暴露接口。本质就是一组寄存器，准确的说是 CSR（Control and Status Register）。接下来唯一的问题是这些CSR如何暴露给带内内核了。在 **ARM传统体系中**，这些CSR往往作为MMIO映射到地址空间中，某个IP CSR MMIO起始地址在SOC设计地址规划阶段就需要定下来，定好后一般后期不能改变。在 **x86体系中**，传统上，南桥的设备映射到PCIe地址空间中，而北桥和CPU内部的设备映射到MMIO中。但后期随着北桥的功能被移入CPU，和CPU中加入越来越多的IP，传统做法让MMIO地址冲突越来越严重。更重要的是，MMIO的映射关系不是PnP的，也就是不可以枚举的，而是在ACPI中列举的，这在系统整合和SKU越来越多的时候，造成很大麻烦。为什么不和南桥设备一样，统一映射到PCIe地址空间呢？

Intel的架构师也有一样的想法，如果我们仔细跟踪Intel CPU中设备CSR的映射关系，就会发现有越来越多的设备IP从MMIO空间移入了PCIe空间（配置寄存器空间或者BAR上），有了自己的Device和Func号。在BIOS启动阶段设备甚至比在操作系统阶段可以看到的设备多得多，很多不需要终端用户关心的设备在启动最后阶段被隐藏了。那么这种迁移是如何做到的呢？是不是通过在芯片内部连接了无数PCIe总线呢？

并不是。这要归功于IOSF（Intel On-Chip System Fabric）总线，它提供Die内backbone Fabric（可以跨DMI），将各个IP整合在一起。IOSF总线包括两种接口（Interface）：Primary Interface和Sideband Interface。主接口提供PCIe抽象，方便各个IP CSR映射到PCIe空间中；而简化的旁路接口是传统的index/value接口，提供轻量级的访问，方便一些简单的IP的CSR映射到带外IO空间中，缺点是速度慢。

借助于IOSF总线，大量的集成IP通过Primary Interface将自己的CSR暴露给带内CPU，从而在独立和互操作上取得平衡。它们在软件逻辑上，看起来和PCIe没有什么不同，其实原理大不相同。 **正因为如此，我们今天，才能在x86 PC和服务器中，看到如此多的PCIe设备，它们不仅仅是传统的外部PCIe设备，还有很多是抽象的内部IP。**

PCI-to-PCI桥规范是理解PCI体系结构的基础

### 常见总线

抛开PCIe不谈，如果我们设计一种板间通信的总线协议，我们关注的点至少有：

+   总线带宽

+   成本开销

+   信号完整性

+   可扩展性

从系统模块的角度，总线系统主要由 2 大部分组成：

1.  CPU 总线，又称为 FSB（Front Side Bus，前端总线）。

1.  PCI/PCIe 总线。

从功能实现上，可以分为 3 种总线类型：

1.  **数据总线**：用于在 CPU 与 Main Memory 或 I/O 设备之间传输数据。

1.  **地址总线**：用于在 CPU 与 Main Memory 或 I/O 设备之间传输地址信息。

1.  **控制总线**：用于在 CPU 与 Main Memory 或 I/O 设备之间传输控制信号。

![](img/f20cbcbbfcbb82ba20872cd38be3347e.png)

| Bus | Year |  |  |

| --- | --- | --- | --- |

| ISA | 1981 | 8位的ISA提供了4.77MB/s的带宽（或传输率） | 第一代 IBM PC XT机型上（1981） |

| EISA | 1988 |  | IBM 的 MCA总线，迫使其他几家PC兼容机厂商联合起来捣鼓出来EISA。因为两者都期待兼容ISA，导致速度没有多大提升。 |

| MCA |  |  | IBM PS/2 产品线 |

|  |  |  |  |

| VLB | 1992 | 绑定自己的频率到了当时486 CPU内部总线频率：33MHz。而到了奔腾时代，内部总线提高到了66MHz，给VLB带来了严重的兼容问题，造成致命一击。 | 真正的高速总线 |

|  |  |  |  |

| I2C or I^2 C |  |  |  |

| SPI |  |  | peripheral 外围的 |

| USB |  |  |  |

| SATA |  |  |  |

| PCI | 1992 | 133MB/s的带宽(33MHz时钟，每时钟传送32bit) |  |

| AGP | 1996 |  |  |

| PCI-X | 1999 |  | PCI-X和AGP走了两条略有不同的路径，PCI-x不断提高时钟频率，而AGP通过在一个时钟周期内传输多次数据来提速。 |

| PCIe / PCI Express | 2004 |  |  |

### Bus History

1990-1991 年 80386 的 motherboard。分 8bit 和 16 bit 2 代 ISA slot。

几乎用于一切外设接到 motherboard 上，缺点是速度慢，不满足最新 graphics card 等设备的需求。

![](img/60ac77f5c0c19f0f9563f604893c7339.png)

1997 年的 motherboard，

1.  4 个 PCI slot。白色。

1.  2 个 ISA slot。白色下面 2 个长条。

1.  1 个 AGP slot。白色的上面。缺点是，版本多，相互不兼容。插口的物理外形都不一样。

![](img/06c50c7324be66c4ca7d5065ed92af67.png)

另一个 motherboard，5 个 PCI slot + 1 AGP Pro slot。

![](img/88003ae32d2177d86a52ef29afa0576b.png)

2004 年，诞生 PCIe。

下面的 motherboard，有 3 个 PCIe x1， 1 个 PCIe x16

![](img/5af1b77055ff4db8faa6fc1991d6cd91.png)

slot 数量

1.  3 个 PCI slot

1.  4 个 PCIe slot。2 个 x1，1 个 x4，1 个 x16。

![](img/52515945e24d7fce015a25e3f0750e77.png)

如果放大了仔细看，PCIe x4 的 slot，只有前 1/4 有 electrical pins。

![](img/0124ef34184f8d0c6b6a5cdc2e0cde80.png)

第一代总线

第二代 IO 总线包括 PCI、AGP 和 PCI-X

第三代高性能IO总线PCIExpress(下称PCIe)

![](img/ecc72e8456a9e0f7ebcfc6817b8598b7.png)

![](img/52f5426e800be5f7c5b0c58d97dfc512.png)

PCI 总线与处理器总线（FSB）以及存储总线（DDR）三者是相互分离的。

PCI总线作为处理器系统的局部总线（Local Bus），是处理器系统的一个组成部分。

FSB（Front Side Bus）两个PCI总线域的PCI设备不能直接进行数据交换，需通过FSB（Front Side Bus）总线进行数据交换（需要CPU参与）

PCI与I2C或SPI的区别是什么？

前者是由物理层，链路层和事务层三层组成。后两者则只有物理层和链路层，所以在先了解I2C SPI协议的前提下，没能意识到这种区别就会发现很难理解。

PCI是与x86强捆绑的，所以他的IO事务类型和内存事务类型的区分对于ARM背景的人就显得很奇怪。IO事务类型是完全为了x86的`in` `out`两个指令服务的，而ARM没有这两个指令。

PCI总线相比于EISA和MCA总线相比有很多突出的优点：

1.  PCI总线空间与处理器空间隔离，即PCI设备具有独立的地址空间，如上图所示该空间通过HOST主桥隔离，而且HOST主桥包含有缓存，使得处理器和PCI总线工作在各自时钟频率中，互不干扰；

——任何访问PCI设备的操作必须经过HOST主桥， PCI设备可以通过HOST主桥与处理器共享主存储器资源；

——HOST主桥的一个重要作用就是将处理器访问的存储器地址转换为PCI总线地址；而PCI设备的地址属于PCI总线域的，与存储器地址空间不同。

1.  PCI总线具有很好的可扩展性，HOST主桥管理的第一条PCI总线，可以通过PCI桥扩展出一系列PCI总线，并以HOST主桥为根节点，形成一颗PCI总线树；

——PCI总线树上最多能挂256个PCI设备，这其中包括了PCI桥；而PCI桥是PCI总线能够扩展的主要原因，每一个PCI桥下相当于一个PCI总线的子树，可以通过PCI桥继续扩展；

——PCI桥可以管理这颗PCI总线子树，其配置空间中含有管理PCI总线子树的配置寄存器。

1.  PCI设备地址可以根据需要由软件动态分配，解决了设备间的地址冲突问题，实现了PCI板卡即插即用的功能；

——PCI设备的地址在BAR空间中，系统软件可以动态配置这个基地址（也可以配置固定地址），保证了每个PCI设备使用的物理地址不相同；PCI桥的配置空间含有其下挂PCI子树所能使用的地址范围。

1.  总线带宽高，32位/33MHz的PCI总线可提供132MB/s的峰值带宽，相比于EISA和MCA要大的多；

1.  共享总线机制，PCI设备通过仲裁后获得总线使用权，然后才能进行数据传输，PCI总线上的数据传输不需要处理器干预；

——PCI总线仲裁器不在PCI总线规范定义中，并不一定是HOST主桥和PCI桥的一部分（一般来说是由它们来实现）。

1.  [中断机制](https://www.zhihu.com/search?q=%E4%B8%AD%E6%96%AD%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3121947087%7D)，PCI总线上的设备可以通过4个中断信号INTA~D#向处理器提交中断请求；PCI总线上的设备可以共享中断请求信号，不同的PCI设备可以“线与”后连接到中断请求管脚上。

### 历代 PCIe 性能

![](img/07c190e8c9bbf3fd59cb50e4ea839d87.png)

![](img/0de4ea027bbb5904250e95adc56df6d6.png)

## 总线简介

### PCIe 历史

Intel在1992年提出PCI（Peripheral Component Interconnect）总线协议，并召集其它的小伙伴组成了名为 PCI-SIG (PCI Special Interest Group)（PCI 特殊兴趣组J）的企业联盟。从那以后这个组织就负责PCI和其继承者们（PCI-X和PCIe的标准制定和推广。

![](img/4649db03ba95f4d1f2e8b83047565e34.png)

1.  在 VESA 和 PCI 的竞争中，PCI 胜出。

1.  PCI 是 单端并行 信号传输数据。易受外部系统干扰，总线频率很难进一步提高。

1.  高速差分信号，可以用更高的时钟频率，更少的信号线。

1.  2021 年，Intel 的 IAL（Intel Architecture Lab）发布取代 PCI 的第 3 代 IO 技术，称为 3GIO，后改名 PCI Express。

1.  当前最新的是 PCIe 6.0。常用的是 PCIe 3.0 和 PCIe 4.0。

1.  从 driver 的角度，PCIe 全面兼容 PCI。

Intel 的 IAL（Intel Architecture Lab）提出了：USB，AGP（Accelerated Graphics Port，替代了VESA总线），PCIe总线规范；甚至连X86的南北桥结构也是由它提出的。

PCI 淘汰了EISA（Extended Industry Standard Architecture）、VESA（Video Electronics Standards Association）等32位的总线

PCIe VS PCI 的 2 个核心改变：

1.  由并行总线转变为串行总线，

1.  点到点的互连。

PCIe 6.0标准变化很大，它将总线上的可用带宽提升了一倍，也就是64GT/s，这意味着PCIe 6.0 x16的带宽可以达到128GB/s，PCIe的信号也由原来的二进制NRZ信号变成了四态的PAM4信号，并结合固定数据包FLIT接口实现传输，以避免速率翻倍之后不提升频率的情况。

顺带一提，在NVIDIA Ampere架构GeForce RTX 3080和3090高性能GPU的推广后，GDDR6X中所使用的PAM4信号在消费领域也已经获得了不少的认知。

![](img/06652175df224080f91e5eb141a74066.png)

### M.2 协议

PCIe有两种存在形式M.2接口通道形式和PCIe标准插槽。

M.2接口通道也是一种PCIe接口，主要插支持M.2的固态硬盘。

为什么有的PCIe插口和M.2的插口不能共存？

两种插口使用的是同一PCIe带宽通道。当使用其中一个的时候，由于被占用，另一个就失效了。

![](img/ebc8b8e2345d6a9a2153857dd1e07ed6.png)

## PCIe 体系

### 通俗介绍

1.  CPU 芯片内，集成了 Root Complex。

1.  下图有 3 个端口，可以连 3 个设备。

1.  通过 PCIe 专用 switch，可以连更多 PCIe 设备。

![](img/5be127c0ca95e7a6dabdbb6066b01e9e.png)

![](img/dfe95ba2f528850c2fd0698a70c6db61.png)

1.  2 个域 (domain)，2 种地址。PCIe 控制器翻译地址。

1.  实际上，为了简化代码，OS 统一编址，可以不用翻译地址。

1.  这 2 个地址，统称为物理地址。

### 精确术语

![](img/f0ef68c2add24d516bb49c2cb39e0e37.png)

![](img/1777f8171c3615b76d47e95bfd41688b.png)

PCI 总线相关的模块包括了：HOST主桥，PCI总线，PCI桥以及PCI设备。

1.  我们已经知道了，HOST主桥的功能是隔离处理器系统的存储器域和PCI总线域，并管理PCI总线域，完成处理器与PCI设备之间的数据交换；而处理器与PCI设备之间的数据交换主要由：1，处理器访问PCI设备的地址空间；2，PCI设备使用DMA机制访问主存储器；这两部分组成；

——HOST主桥下的PCI设备的配置空间都由HOST主桥通过配置读写（Config Read/Write）访问。

1.  PCI总线，有PCI总线和PCI总线树两个概念；一颗PCI总线树中可能有多条PCI总线，而具有血缘关系的PCI总线组成一颗PCI总线树，一般直接与HOST主桥连接的PCI总线被命名为总线0；

——PCI总线由HOST主桥或PCI桥来管理，用来连接各类PCI设备；如果有多个HOST主桥那么就有多个PCI总线树。

1.  PCI设备，PCI总线中有三类设备：PCI主设备，PCI从设备和桥设备；

1，PCI从设备只能被动接收来自HOST主桥或其它PCI设备的读写请求；

2，PCI主设备可以通过总线仲裁获得PCI使用权，主动向其它PCI设备/主存储器发起读写请求；

3，桥设备的主要作用是管理下游PCI总线，并转发上下游总线之间的事务；

——PCI桥的两端分别接了两条PCI总线：上游总线和下游总线；其中距离HOST主桥较近的总线称为上游总线（Primary Bus），相反较远的那条称为下游总线（Secondary Bus）；但是不可否认的是它们都属于同一个总线域。

——存在两种PCI桥：透明桥和非透明桥。

——PCI桥的存在是PCI总线具有强大扩展性的重要原因，使大规模系统互连成为可能。

4，HOST主桥是一个特殊的PCI设备，可以获取PCI总线的控制权和访问PCI设备，也可以被PCI设备所访问，但是HOST主桥并非PCI设备，在PCI规范中没有对HOST主桥进行定义；

1.  HOST处理器，PCI总线规范规定在同一时刻，在一颗PCI总线树上有且只有一个HOST处理器；该HOST处理器可以通过HOST主桥发起PCI总线的配置请求总线事务，并对PCI总线上的设备和桥片进行配置。

——对于多CPU的处理器，这些CPU都可以通过HOST主桥访问其下的PCI总线树，作为HOST处理器；但上面已经说了：PCI总线树的实际管理者是HOST主桥，而非HOST处理器。

1.  处理器与PCI设备之间的数据通信，需要先经过HOST主桥进行地址转换（翻译）；

1.  处理器系统可能含有多个HOST主桥，那么不同HOST主桥下的PCI设备通信需要经过两个HOST主桥；

1.  HOST主桥与主存储器控制器在同一级总线上，因此PCI设备可以直接通过HOST主桥访问主存储器（DMA：Direct Memory Access）；
