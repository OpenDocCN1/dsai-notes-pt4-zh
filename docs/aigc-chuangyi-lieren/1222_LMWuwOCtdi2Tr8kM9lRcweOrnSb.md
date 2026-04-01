# 20240320-震撼！英伟达从全新Blackwell架构到32000块GPU的超级AI工厂组建过程！

> 来源：[https://aigcreative.feishu.cn/docx/JVDOdjqMkoH2Zmx9VQ6ch3c8n3K](https://aigcreative.feishu.cn/docx/JVDOdjqMkoH2Zmx9VQ6ch3c8n3K)

> 链接：https://mp.weixin.qq.com/s/v_IGRQNROQp0kEBQ9jrQeA

**英伟达最新推出的Blackwell架构是该公司为加速计算和生成式人工智能（AI）领域设计的核心**平台。这一架构的推出旨在推动新一代计算革命，特别是在处理大规模AI工作负载方面。

**先复习一下基础知识：**浮点运算次数（Floating Point Operations Per Second，简称FLOPS）是用来衡量计算设备执行浮点运算能力的指标。这个指标通常用来描述处理器（CPU）、图形处理器（GPU）或其他计算设备在一秒钟内能够执行多少次浮点运算。浮点运算是指能够处理带有小数点的数学运算，这对于科学计算、工程模拟、图形渲染等领域尤为重要。浮点运算次数的单位按照大小顺序如下：

1.  **KFLOPS**（Kilo FLOPS）：每秒千次浮点运算。

1.  1 KFLOPS = 1,000 FLOPS

1.  **MFLOPS**（Mega FLOPS）：每秒百万次浮点运算。

1.  1 MFLOPS = 1,000,000 FLOPS

1.  也就是每秒一百万次浮点运算。

1.  **GFLOPS**（Giga FLOPS）：每秒十亿次浮点运算。

1.  1 GFLOPS = 1,000,000,000 FLOPS

1.  也就是每秒十亿次浮点运算。

1.  **TFLOPS**（Tera FLOPS）：每秒万亿次浮点运算。

1.  1 TFLOPS = 1,000,000,000,000 FLOPS

1.  也就是每秒一万亿次浮点运算。

1.  **PFLOPS**（Peta FLOPS）：每秒千万亿次浮点运算。

1.  1 PFLOPS = 1,000,000,000,000,000 FLOPS

1.  也就是每秒千万亿次浮点运算。

1.  **EFLOPS**（Exa FLOPS）：每秒百亿亿次浮点运算。

1.  1 EFLOPS = 1,000,000,000,000,000,000 FLOPS

1.  也就是每秒百亿亿次浮点运算。

这些单位通常用于高性能计算（HPC）领域，以及在描述超级计算机的性能时。随着技术的发展，现代的超级计算机已经达到了EFLOPS级别的计算速度，例如，用于气候模拟、生物信息学、物理模拟等复杂计算任务。在评估计算设备的性能时，FLOPS是一个重要的参考指标，但它并不是唯一的衡量标准，内存带宽、存储速度、软件优化等因素也会影响实际的计算性能。

**

先上2024GTC大会开胃**菜

![](img/72de2e800c25a8060d589c09d0eb0866.png)

![](img/261e46899ebf77b2fa0e1faa05e8770a.png)

![](img/df2ac5932f165fdfc62c1f325f7deffb.png)

**开始建造超级AI工厂**

![](img/a3e56af87bf5cfc4e93c268cebd075ac.png)

Blackwell

物理上可能存在的最大芯片

1040亿个晶体管

合积电4NP工艺

10TB/S NVIDIA高宽带接口

![](img/a651d6fc9cdbab6f12c9a08d4189d821.png)

Blackwell GPU

两个裸片集成为一个GPU

2080亿个晶体管

全缓存一致性

![](img/c3a52e65d8226d1276adc7c2532dab3c.png)

Blackwell Vs Hopper

尺寸是Hopper的两倍

5倍的AI性能

4倍的no-die memory

![](img/48d36cada1fc145e0628245af8b76f3d.png)

Blackwell

新工业革命的引擎AI性能

20 petaFLOPS

192GB的HBM3e

8TB/s的内存带宽

全栈，启用CUDA

![](img/433153714f6ce78e15a2968b04b1fdac.png)

两个BlackwellGPU和一个Grace CPU

GB200超级芯片的组成部分

384GB的HBM3e72

Arm Neoverse V2 CPU cores

900GB/s的NVlink-C2C带宽

![](img/85d02e8fe3b3a99c49a9216517ae14d9.png)

GB200 Grace Blackwell超级芯片

万亿参数级人工智能生成处理器

40 petaFLOPS的AI性能

864GB 高速内存

![](img/b9b6475427a168ffc2f8f9188e3a735c.png)

Blackwell Compute node

有史以来最强大的计算节点

两个Grace CPU和四个Blackwell GPU

80 petaFLOPS的AI性能

1.7TB的 HMB3e

32TB/s的内存带宽

水冷式 MGX设计

![](img/65f0bc30aa998a65b5b2d0c29c3ecc7a.png)

connectx-800G infiniband supernic

业界最先进的 GPU RDMA

自适应路由和可编程拥塞控制

针对AI处理进行了优化

![](img/724d718763550382d5f7ab1a6ef4c425.png)

BLUEFIELD-3 DPU

强大的基础架构处理器

网络、存储和网络安全的线速处理

网络内计算

每秒 8oCB 的内存带宽

![](img/dea7ec8eef95a241e291a87e9a062445.png)

GB200 NVL72

为数据中心设计

一个机架上有18个计算托盘

36个Grace CPU 和 72个Blackwell GPU

![](img/c19ad4fe4643e44664d8c4cfb393d9a0.png)

NVLINK SWITCH

多BlackwellGPU 架梅

两个NVLINK交换芯片

八个端口，速度为 1.8TB/S

14.4TB/的总带宽

![](img/272a98ec31e226320b08be8d93fb64f4.png)

NVLINK交换机系统

Blackwell多GPU架构

18个NVLINK交换机芯片

72个端口，1.8TB/秒的速度

以 130TB/的总带宽进行全对全通信

![](img/b1821f13f02c2e60675a0c1d91c46f66.png)

GB200计算节点Invlink交换机和SPINE

用于生成式人工智能的天型GPU

72个Blackwell GPU由NMLINK完全连接

铜缆布线，成本降低6倍

盲插连接器便于安装和维护

![](img/4995a49964128d12ef8881c7d3679885.png)

CB200NVL72

计算万亿参数规模的生成式人工智能

1.4exaFLOPS的AI性能

30TB HBMЗe

一个巨大的 CUDA GPU

![](img/37ff6879ad2904b9e905f9b637867b74.png)

QUANTUN INFINBAND SWITCH

无与伦比的数据吞吐量和密度

每个端口800Gb/s的吞吐量

每秒230.4TB的聚合双向吞吐量

![](img/f51ab902d460e1cce38c582874417b6f.png)

quantum-X800 infiniband

或spectrum-X800 以大网交换机

最高性能的网络

800Gbs连接

带宽容量提高5倍

14.4 teraFLOPs 的网内计算能力

![](img/8b999de20e0142b02c1fcbe73245fdca.png)

GB200 NVL72 计算机架

液体冷却，实现最佳能效

八个GB200 NVL72

288个GraceCPU和576个Blackwell GPU

机架冷却功率降低2倍

![](img/565037bee24d403a4613942176fdea8a.png)

配备32,000个GPU的完整数据中心

新工业革命的AI工厂

645 exaFLOPS的AI性能

13PB 快速内存

58PB后的NVLINK总带宽

16.4百万亿次网络内计算

Blackwell架构是英伟达在AI和加速计算领域的又一重要里程碑。它通过引入创新技术和显著提升性能，为未来的计算革命奠定了基础，特别是在AI模型的训练和推理方面。随着这一架构的进一步发展和应用，我们有望见证AI技术在各个领域的广泛应用和快速发展。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>