
# 英伟达全产品体系：从全新 Blackwell 架构，到能容纳 32000 块 GPU 的超级 AI 工厂

> 来源：`https://aigcreative.feishu.cn/docx/Ha7NdKOtOo04bdxrQC0cwRtXnGd`

> 先复习一下基础知识：

浮点运算次数（Floating Point Operations Per Second，简称 FLOPS）是用来衡量计算设备执行浮点运算能力的指标。这个指标通常用来描述处理器（CPU）、图形处理器（GPU）或其他计算设备在一秒钟内能够执行多少次浮点运算。浮点运算是指能够处理带有小数点的数学运算，这对于科学计算、工程模拟、图形渲染等领域尤为重要。

浮点运算次数的单位按照大小顺序如下：

1. **KFLOPS**（Kilo FLOPS）：每秒千次浮点运算。
   - 1 KFLOPS = 1,000 FLOPS
1. **MFLOPS**（Mega FLOPS）：每秒百万次浮点运算。
   - 1 MFLOPS = 1,000,000 FLOPS
   - 也就是每秒一百万次浮点运算。
1. **GFLOPS**（Giga FLOPS）：每秒十亿次浮点运算。
   - 1 GFLOPS = 1,000,000,000 FLOPS
   - 也就是每秒十亿次浮点运算。
1. **TFLOPS**（Tera FLOPS）：每秒万亿次浮点运算。
   - 1 TFLOPS = 1,000,000,000,000 FLOPS
   - 也就是每秒一万亿次浮点运算。
1. **PFLOPS**（Peta FLOPS）：每秒千万亿次浮点运算。
   - 1 PFLOPS = 1,000,000,000,000,000 FLOPS
   - 也就是每秒千万亿次浮点运算。
1. **EFLOPS**（Exa FLOPS）：每秒百亿亿次浮点运算。
   - 1 EFLOPS = 1,000,000,000,000,000,000 FLOPS
   - 也就是每秒百亿亿次浮点运算。

这些单位通常用于高性能计算（HPC）领域，以及在描述超级计算机的性能时。随着技术的发展，现代的超级计算机已经达到了 EFLOPS 级别的计算速度，例如，用于气候模拟、生物信息学、物理模拟等复杂计算任务。在评估计算设备的性能时，FLOPS 是一个重要的参考指标，但它并不是唯一的衡量标准，内存带宽、存储速度、软件优化等因素也会影响实际的计算性能。

## 2024 GTC 大会开胃菜

![](img/1867620c8d8a976f8f27866c4279ebfe.png)

![](img/5f0ccc33dff4d5acdaff5cbaa0072567.png)

![](img/050ebcab167e9f0b071120c766cc8d0c.png)

### 开始

![](img/b2afe8db13c38dfc2bac9eeb756d2224.png)

## Blackwell

物理上可能存在的最大芯片

1040 亿个晶体管

合积电 4NP 工艺

10TB/S NVIDIA 高宽带接口

![](img/ab504f8e1e1f7f4be3ecd07bb0e4982d.png)

### Blackwell GPU

两个裸片集成为一个 GPU

2080 亿个晶体管

全缓存一致性

![](img/f6da7d01f2eea4c5d73ec113a3c2a708.png)

### Blackwell Vs Hopper

尺寸是 Hopper 的两倍

5 倍的 AI 性能

4 倍的 no-die memory

### Blackwell：新工业革命的引擎

AI 性能 20 petaFLOPS

192GB 的 HBM3e

8TB/s 的内存带宽

全栈，启用 CUDA

![](img/5507f868f900c62a2803294ada5b9660.png)

### 两个 Blackwell GPU 和一个 Grace CPU

GB200 超级芯片的组成部分

384GB 的 HBM3e

72 Arm Neoverse V2 CPU cores

900GB/s 的 NVlink-C2C 带宽

![](img/e484d74cbacda8215b16fb9014ece47b.png)

### GB200 Grace Blackwell 超级芯片

万亿参数级人工智能生成处理器

40 petaFLOPS 的 AI 性能

864GB 高速内存

![](img/01c5116c97dffbde5d30c913932dd51f.png)

### Blackwell Compute Node

有史以来最强大的计算节点

两个 Grace CPU 和四个 Blackwell GPU

80 petaFLOPS 的 AI 性能

1.7TB 的 HMB3e

32TB/s 的内存带宽

水冷式 MGX 设计

![](img/5604d512e2f5c7665620c8f1f219c93f.png)

### ConnectX-800G InfiniBand SuperNIC

业界最先进的 GPU RDMA

自适应路由和可编程拥塞控制

针对 AI 处理进行了优化

### BlueField-3 DPU

强大的基础架构处理器

网络、存储和网络安全的线速处理

网络内计算

每秒 8oCB 的内存带宽

![](img/a69b0b1ab58412e7d47ddc07c61aeae7.png)

### GB200 NVL72

为数据中心设计

一个机架上有 18 个计算托盘

36 个 Grace CPU 和 72 个 Blackwell GPU

![](img/36a59ba09a268c92b57d70f775915c67.png)

### NVLINK Switch

多 Blackwell GPU 架构

两个 NVLINK 交换芯片

八个端口，速度为 1.8TB/S

14.4TB/S 的总带宽

NVLINK 交换机系统

Blackwell 多 GPU 架构

18 个 NVLINK 交换机芯片

72 个端口，1.8TB/秒的速度

以 130TB/S 的总带宽进行全对全通信

![](img/16a5b111fadc89e094a288af0826a633.png)

### GB200 计算节点 InvLink 交换机和 SPINE

用于生成式人工智能的天型 GPU

72 个 Blackwell GPU 由 NMLINK 完全连接

铜缆布线，成本降低 6 倍

盲插连接器便于安装和维护

![](img/ae572d510461a64cf41715312f2dc4f6.png)

### CB200NVL72

计算万亿参数规模的生成式人工智能

1.4 exaFLOPS 的 AI 性能

30TB HBM3e

一个巨大的 CUDA GPU

![](img/0a4538ba7a4bcc6e6480db738b04eb07.png)

### Quantum InfiniBand Switch

无与伦比的数据吞吐量和密度

每个端口 800Gb/s 的吞吐量

每秒 230.4TB 的聚合双向吞吐量

![](img/3581cb25f48716e56a6041a4e2972900.png)

### Quantum-X800 InfiniBand 或 Spectrum-X800 以太网交换机

最高性能的网络

800Gbs 连接

带宽容量提高 5 倍

14.4 teraFLOPs 的网内计算能力

![](img/d2bad25669f85a62f5fc897999a1469a.png)

### GB200 NVL72 计算机架

液体冷却，实现最佳能效

八个 GB200 NVL72

288 个 Grace CPU 和 576 个 Blackwell GPU

机架冷却功率降低 2 倍

![](img/25937143727928eea1e9e4753b04b830.png)

### 配备 32,000 个 GPU 的完整数据中心

新工业革命的 AI 工厂

645 exaFLOPS 的 AI 性能

13PB 快速内存

58PB 后的 NVLINK 总带宽

16.4 百万亿次网络内计算

`https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg`
