# Nvidia GPGPU uArch

> 来源：[https://learning.feishu.cn/docx/To92dPCUOorU20x3LZUceJYOnzd](https://learning.feishu.cn/docx/To92dPCUOorU20x3LZUceJYOnzd)

NVIDIA Ampere Architecture In-Depth

https://developer.nvidia.com/blog/nvidia-ampere-architecture-in-depth/

https://developer.nvidia.com/blog/nvidia-hopper-architecture-in-depth/

+   FP8 format和Transformer Engine：Hybrid 8-bit Floating Point是2019年的论文，Tesla Dojo上也产品化了Tesla Configurable Float8 (CFloat8)。结合Transformer的动态精度（adaptive format）加速引擎，基本可以将Transformer类模型性能x2。

+   In-switch computing（In-fabric multicast and reductions）：这一代终于加上去了，减少了一半的带宽需求。

老黄鸡汤：

+   我一直倾向于直接和团队沟通，这样做更透明，所有人都能获取到第一手信息和知识，而不是我的想法被转述、重复和解释。

+   虚拟世界将会有更多的创新、设计和创造力，而不是在物理世界中进行所有的创新和设计。

+   在未来10年里，不一定是在晶体管层面，在计算层面我们绝对会看到更大的进步。

+   如果人工智能要进入下一个阶段，就必须学习物理定律。

![](img/607757af56e87cbe890b5d99dee14e12.png)

![](img/694f2be2d50282d2a78282e53232a690.png)

# 反推 NV GPU 的 uArch

这个细分领域叫 uArch detection, 或者gpu micro benchmark，或者gpu micro architecture

Benchmarking and Dissecting the Nvidia Hopper GPU Architecture https://arxiv.org/html/2402.13499v1

Dissecting the Ampere GPU Architecture through Microbenchmarking

Dissecting the NVidia Volta GPU architecture via mircobenchmark

Dissecting the NVidia Turing T4 GPU via Microbenchmarking

# Grid - block - thread

在H100之前的架构中，线程的控制相对有限，

主要基于 Grid 和 Block 的划分，分别对应硬件 Device 和 SM。

且局部数据只能通过 shared memory 限制在SM内部，不能跨SM。

Hopper 在硬件层面引入交叉互联网络，数据得以在4个SM之间进行拓展和共享，GPC内SM可以高效访问彼此的共享内存。这一创新使得SM之间能够实现高效的通信，从而打破了之前架构中SM间的数据隔阂。

A100 通过全局内存的数据交互，不仅增加了数据传输的时延，还可能导致寄存器的过度使用

![](img/677a386427b2182f3e98a4fa90b268e5.png)

在H100架构中，如上图右边所示，通过引入SM的Cluster或Block的Cluster，实现了硬件层面的分布式共享内存。这意味着SM与SM之间的数据可以直接进行互联，无需再通过全局内存进行中转。这种机制极大地减少了数据传输的时延，提高了数据交互的效率。同时，由于数据可以在SM之间直接共享，寄存器的使用也得到了更加合理的分配，减少了不必要的浪费。

通过TMA将SM组织成一个更大的计算和存储单元，从而实现了数据从全局内存（global memory）到共享内存（shared memory）的异步加载，以及数据到寄存器的计算和矩阵乘法的流水线处理，最后通过硬件实现了矩阵乘法的流水线。硬件实现的矩阵乘法流水线确保了计算过程的连续性和高效性，使得GPU能够更快地处理大规模矩阵运算。

![](img/0c44a3a2f9ab764cc07bfab138e97331.png)

要点总结

1.  大面上，越来越多的 core，越来越大的显存。

1.  GPU 和 GPU 直连，从 Kepler 2012 开始，Pascal 2016 有了 nvlink，然后 nvlink 越来越快。

1.  近几年开始增加行业相关的优化。比如，deep learning 的 TensorCore，游戏和仿真的 Ray Tracing Core

心得：

1.  初步了解一下就是，大部分架构没啥深入学习的价值。

1.  Tesla 火是因为第一代。

1.  Turing 也很火。但，就是个改进了光追踪的推理卡架构，只出了 2080ti 和 T4。估计是天下文章一大抄导致的。

the FLOPS per $ doubles every ~2.5 years

For top GPUs, doubles every 2.95 years

游戏 GPU，用户能够接受的高端 GPU 价格长期以来就是在 $599～$799 之间。

不看尾货，最新 GEN 的 HBM 版本大约都需要 $20/GB，放 8 GB 就 $160 了。

# Arch Brief

| No. | Date | Chip | 经典型号 | 要点 | Specs |
| --- | --- | --- | --- | --- | --- |
| 1 | 2008/6/16 | Tesla | G80 | 1.  引入 SIMT execution model |  |
| 2 | 2010/3/26 | Fermi | GTX 480 | 1.  更快的 Atomic Operations。读-修改-写 | 1.  每个 SM 有 32 个 CUDA 内核。 |
| 3 | 2012/11/12 | Kepler | GTX 680, | 1.  没有后续改进架构的一代，讨论的似乎也不多。 | 1.  每个SM单元的CUDA内核数由Fermi架构的32个激增至192个 |
| 4 | 2015/3/17 | Maxwell | GTX 750/980, | 1.  基于 2010 的 Fermi 架构，而不是 2012 年的 Kepler 架构。 | 不太重要。 |
| 5 | 2016/4/5 | Pascal | P100, P4 | 1.  引入了 NVLink。带宽达到了160GB/s, 大约5倍于PCIe 3 x 16 | 单机 8 卡 P100拓扑 |
| 6 | 2017/6/21 | Volta | V100, | 1.  引入了 TensorCore。从 SIMT 走到了 SIMT+DSA 的混合。 | ![](img/3130a6e85891b43780494e324bc00c1f.png) |
| 7 | 2018/8/13 | Turing | 2080ti | 1.  给游戏或者仿真用的。加了 RT Core，全名是 Ray Tracing Core |  |
| 8 | 2020/5/14 | Ampere | A100 | 1.  最核心的升级就是 Tensor Core。加入 TF32, BF16, FP64。历史是，在 Volta 中的 FP16 以及在 Turing 中的 INT8/INT4/Binary。 | ![](img/7a87005a121f5ef250918c583b3279d9.png) |
| 9 | 2022/3/22 | Hopper |  |  |  |

1.  GP100：3840个CUDA核心，60组SM单元；

1.  GP102：3584个CUDA核心，28组SM单元；

其他参数对比

| No. | Date | Chip | Launch | Fab(nm) | Transistors | Die Size | Density |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 2008/6/16 | Tesla | GT200 | 65 | 1.4 | 576 | 2.4 | ![](img/e05278b8baf21245357bb396c92c6047.png) |
| 2 | 2010/3/26 | Fermi | GF100 | 40 | 3.1 | 529 | 5.9 | ![](img/e95965dbaa4278417e6f5a4b78b08592.png) |
| 3 | 2012/11/12 | Kepler | GK110 | 28 | 7.08 | 561 | 12.6 | ![](img/fa1589d3734649084942ee2fce3b5bd3.png) |
| 4 | 2015/3/17 | Maxwell | GM200 | 28 | 8 | 601 | 13.3 | ![](img/be60fde1baa64214ebf90fc553fcc500.png) |
| 5 | 2016/4/5 | Pascal | GP100 | 16 | 15.3 | 610 | 25.1 |  |
| 6 | 2017/6/21 | Volta | GV100 | 12 | 21.1 | 815 | 25.9 | ![](img/239cb1795280ba705bc6561ecf2f478d.png) |
| 7 | 2018/8/13 | Turing | TU102 | 12 | 18.6 | 754 | 24.7 |  |
| 8 | 2020/5/14 | Ampere | GA100 | 7 | 54.2 | 826 | 65.6 |  |
| 9 | 2022/3/22 | Hopper | GH100 | 4 | 80 | 814 | 98.3 |  |

# 训练卡详细指标

## 基础指标

| **Data Center GPU** | NVIDIA Tesla P100 | NVIDIA Tesla V100 | **NVIDIA A100** |
| --- | --- | --- | --- |
| **GPU Codename** | GP100 | GV100 | GA100 |
| GPU Architecture | NVIDIA Pascal | NVIDIA Volta | NVIDIA Ampere |
| GPU Board Form Factor | SXM | SXM2 | SXM4 |
| **SMs** | 56 | 80 | 108 |
| **TPCs** | 28 | 40 | 54 |
| **FP32 Cores / SM** | 64 | 64 | 64 |
| FP32 Cores / GPU | 3584 | 5120 | 6912 |
| **FP64 Cores / SM** | 32 | 32 | 32 |
| FP64 Cores / GPU | 1792 | 2560 | 3456 |
| INT32 Cores / SM | NA | 64 | 64 |
| INT32 Cores / GPU | NA | 5120 | 6912 |
| Tensor Cores / SM | NA | 8 | 4 |
| Tensor Cores / GPU | NA | 640 | 432 |
| **GPU Boost Clock** | 1480 MHz | 1530 MHz | 1410 MHz |
| **Peak FP16 Tensor TFLOPS **with FP16 Accumulate | NA | 125 | 312/624 |
| **Peak FP16 Tensor TFLOPS **with FP32 Accumulate | NA | 125 | 312/624 |
| **Peak BF16 Tensor TFLOPS **with FP32 Accumulate | NA | NA | 312/624 |
| Peak TF32 Tensor TFLOPS | NA | NA | 156/312 |
| Peak FP64 Tensor TFLOPS | NA | NA | 19.5 |
| Peak INT8 Tensor TOPS | NA | NA | 624/1248 |
| Peak INT4 Tensor TOPS | NA | NA | 1248/2496 |
| Peak FP16 TFLOPS | 21.2 | 31.4 | 78 |
| Peak BF16 TFLOPS | NA | NA | 39 |
| Peak FP32 TFLOPS | 10.6 | 15.7 | 19.5 |
| Peak FP64 TFLOPS | 5.3 | 7.8 | 9.7 |
| **Peak INT32 TOPS** | NA | 15.7 | 19.5 |
| **Texture Units** | 224 | 320 | 432 |
| Memory Interface | 4096-bit HBM2 | 4096-bit HBM2 | 5120-bit HBM2 |
| **Memory Size** | 16 GB | 32 GB / 16 GB | 40 GB |
| Memory Data Rate | 703 MHz DDR | 877.5 MHz DDR | 1215 MHz DDR |
| Memory Bandwidth | 720 GB/sec | 900 GB/sec | 1.6 TB/sec |
| **L2 Cache Size** | 4096 KB | 6144 KB | 40960 KB |
| Shared Memory Size / SM | 64 KB | Configurable up to 96 KB | Configurable up to 164  KB |
| Register File Size / SM | 256 KB | 256 KB | 256 KB |
| Register File Size / GPU | 14336 KB | 20480 KB | 27648 KB |
| **TDP** | 300 Watts | 300 Watts | 400 Watts |
| **Transistors** | 15.3 billion | 21.1 billion | 54.2 billion |
| **GPU Die Size** | 610 mm² | 815 mm² | 826 mm² |
| TSMC Manufacturing Process | 16 nm FinFET+ | 12 nm FFN | 7 nm N7 |

|  | **Pascal-P100** | **Volta-V100** | **Pascal-P4** | **Turing-T4** |
| --- | --- | --- | --- | --- |
| **晶体管数** | 153亿 | 210亿 | 72亿 | 136亿 |
| **制程** | 16nm | 12nm | 16nm | 12nm |
| **SMs** | 56 | 80 | 20 | 40 |
| **GPU Boost Clock** | 1480MHz | 1530MHz | 1063MHz | 1590MHz |
| **CUDA cores/GPU** | 3584 | 5120 | 2560 | 2560 |
| Tensor cores/GPU | - | 640 | - | 320 |
| Peak FP64 TFLOPS | 5.3 | 7.8 | .17 | .25 |
| Peak FP32 TFLOPS | 10.6 | 15.7 | 5.7 | 8.1 |
| Peak FP16 TFLOPS | 21 | 31 | .087 | 16 |
| Peak Tensor FP16 TFLOPS | - | 125 | - | 65 |
| **Training ResNet-**50 V1.5 (TensorFlow) | - | 6353 img/s | - | 2151 img/s |
| Inference ResNet-50 V1.5 | - | 7223 img/s | - | 5013 img/s |
| Memory Bandwidth(GB/sec) | 732 | 900 | 192 | 320 |
| **Power** | 300w | 300w | 75w | 75w |
| **Price** | 5480$ | 8100$ | 1895$ | 2080$ |

## capacity

| **Data center GPU** | NVIDIA Tesla P100 | NVIDIA Tesla V100 | **NVIDIA A100** |
| --- | --- | --- | --- |
| **GPU Codename** | GP100 | GV100 | GA100 |
| GPU Architecture | NVIDIA Pascal | NVIDIA Volta | NVIDIA Ampere |
| Compute Capability | 6.0 | 7.0 | 8.0 |
| **Threads / Warp** | 32 | 32 | 32 |
| **Max Warps / SM** | 64 | 64 | 64 |
| Max Threads / SM | 2048 | 2048 | 2048 |
| Max Thread Blocks / SM | 32 | 32 | 32 |
| Max 32-bit Registers / SM | 65536 | 65536 | 65536 |
| Max Registers / Block | 65536 | 65536 | 65536 |
| Max Registers / Thread | 255 | 255 | 255 |
| Max Thread Block Size | 1024 | 1024 | 1024 |
| **FP32 Cores / SM** | 64 | 64 | 64 |
| Ratio of SM Registers to FP32 Cores | 1024 | 1024 | 1024 |
| Shared Memory Size / SM | 64 KB | Configurable up to 96 KB | Configurable up to 164 KB |

## A100

[h](https://devblogs.nvidia.com/nvidia-ampere-architecture-in-depth/)ttps://devblogs.nvidia.com/nvidia-ampere-architecture-in-depth/

[https://www.nvidia.com/en-](https://www.nvidia.com/en-us/data-center/a100/)us/data-center/a100/

[h](https://www.techpowerup.com/gpu-specs/tesla-a100-sxm4.c3506)ttps://www.techpowerup.com/gpu-specs/tesla-a100-sxm4.c3506

1.  支持SM的数据直接从global memory中load，并不会占用 register file bandwidth

1.  SM的访存是异步的，并不会block SM的使用

1.  Asynchronous barrier

1.  新的同步机制，可以与异步拷贝一起使用，达到Producer-Consumer的效果

1.  粒度可控，不在局限于warp或者block

1.  A100

1.  L1+SharedMemory共192KB（SharedMemory最大支持164KB）

1.  40GB HBM2 内存，理论带宽1.6 TB/s（73% speedup compared with V100）

1.  40MB L2 cache（所有SM共享），size接近V100的7倍，速度接近V100的2.3倍

完整的GA100 应该包含：

+   每个A100有128个SMs：每个GPU有8个GPCs，每个GPC有8个TPCs，每个TPCs有2个SMs

+   每个A100有8192个FP32 CUDA Core（每个SM有64个FP32 CUDA Core）

+   每个A100有512个 第三代TensorCore（每个SM有4个第三代TensorCore）

+   6个HBM2 stacks，12个512bit 内存控制器

A100上的GA100实际包含：（封掉了部分硬件）

+   每个A100有108个SMs：每个GPU有7个GPCs，每个GPC有7-8个TPCs，每个TPCs有2个SMs

+   每个A100有6912个FP32 CUDA Core（每个SM有64个FP32 CUDA Core）

+   每个A100有432 第三代TensorCore（每个SM有4个第三代TensorCore）

+   5个HBM2 stacks，10个512bit 内存控制器

A100 SM架构

+   A100上的TensorCore是第三代

+   每个TensorCore在1个时钟周期完成256次FMA（256应该是M*K*N = 4*8*8，V100是64次FMA）

+   每个SM有4个TensorCore，相当于1个是时钟周期完成1024次FMA（V100是每个SM有8个TensorCore，共完成512次FMA）

+   加速所有的类型，包括FP16、BF16、TF32、FP64、Int8、Int4、Binary

+   引入Sparsity Struct

+   TF32加速常规的FP32计算

+   192KB的L1+SharedMemory

+   Asynchronous SM Copy

+   Asynchronous SM Barrier

+   针对L2 cache管理提供更多指令

【L2 Cache】

To optimize capacity utilization, the NVIDIA Ampere architecture provides L2 cache residency controls for you to manage data to keep or evict from the cache. You can set aside a portion of L2 cache for persistent data accesses.

For example, for DL inferencing workloads, ping-pong buffers can be persistently cached in the L2 for faster data access, while also avoiding writebacks to DRAM. For producer-consumer chains, such as those found in DL training, L2 cache controls can optimize caching across the write-to-read data dependencies. In LSTM networks, recurrent weights can be preferentially cached and reused in L2.

# Example GPU

![](img/fb945a7ce9e26d3bb5f5b69b0a5a0d4a.png)

## GTX 285

![](img/5f07d358eb654ab3d24be5b47f01613f.png)

![](img/9d1d6e227ed56bf82926d17305bc4653.png)

1.  每个 group 32 threads。-> warp。SIMD 模式运行，share 一个 instruction stream。

1.  up to 32 warps 以 FGPT 方式 interleaved 运行。

1.  最多 store 1024 个 thread context。

1.  ensure every pipeline stage is occupied by a different thread instruction。

![](img/b75177d161863314c831187a68eae42b.png)

![](img/975d539877653155ae98df3fa2acda6d.png)

## Fermi arch

![](img/28bdf2ff27dccaf7c3737499ced6c1b7.png)

SM单元缓存图：

![](img/3b56618e7147788c31ac15824fcc22ae.png)

Pascal架构

![](img/173c47f25eaa777816ed27924086ba47.png)

Turing架构

![](img/fe1164180e39649abb49a7566912f392.png)

## Turing arch

![](img/945f8ff6fdf2ebe8cc20024d9b9322ec.png)

Volta架构

在SM单元内部缓存方面，Turing与Volta采用了同样的结构，不同点主要在于数值的变动。而两者与Pascal的SM缓存结构相比，主要有以下几个差异点：

+   融合了SM单元的共享 L1 data cache 与 shared memory，采用灵活分配策略（以Turing T4，总容量为96kb的共享缓存为例，两者的比例可在 64+32 KiB与32+64KiB间动态调配），在处理数据交换需求少的任务时，实际可用的L1数据缓存提高了两到三倍。

+   指令缓存更加靠近计算单元，由原本的PB块外部共享转变为PB块内部独占

+   每个SM块寄存器文件的总量没有变化，但每个CUDA核实际分配的寄存器容量增加了

## V100

![](img/c4936d5efc8010c7c7b97b35f9a3c94a.png)

![](img/0c85a6298e23ddbb84e84b7acdeb3403.png)

![](img/f72cedb016ca2de87eff94e0f019887a.png)

![](img/0ea28459899cc8387b32f97805123491.png)

## A100

![](img/987191e53178672bee0e0938f0bf0d80.png)

![](img/27ec00fee7a65e24c5e58dba39ab8706.png)

![](img/63b1a1675e183db146da379f67bf6ffd.png)

内存结构，简化模型

![](img/f754b12e7247f4c836fa8ad9a84e7dc6.png)

## GTX 2080

![](img/0e936ab52cb4874b3aebd31edd1979e5.png)

![](img/69b9db7b883488710f4d7bbbd69d0555.png)

# Edge TPU

![](img/15574fec6c0eaefd7d822edc42b25639.png)

![](img/0a93ed13a0effcc641f5ad3df3c331ae.png)

![](img/5b47b36acb3982e98ff8f7bff14e1478.png)

![](img/ede218bbd5a88abc226dac06779d5b00.png)

# NVlink

NVLink的技术细节简要来说可以分为NVHS，Sub-link，Link，Gang四个层次。一条NVHS的link提供单向20Gb/s的传输带宽带宽，8根NVHS构成一个Sub-link，两条Sub-links组成一条用于双向连接的Link，P100架构下的单机八卡配置，不同GPU之间会由四个Link组成一条Gang，所以Gang的双向汇总带宽是20Gb * 8 * 2 * 4 = 160GB/s，单向汇总带宽80GB/s，是PCIe提供带宽的5x。

# Volta 时代的思考

为什么选择了先支持FP16，而不是Google提及的BF16？为什么考虑选择了4x4尺寸的TensorCore，而不是更大的尺寸？如何figure out出来和FP16配套的Loss scaling的训练策略？TensorCore的编程API如何对外暴露来尽量避免和现有CUDA体系形成撕裂？怎样让NV之外的开发者也具备在TensorCore上开发程序的能力？这里涉及到了大量的技术、非技术因素的综合权衡。一些结论决策，当时看是合理的，但现在来看，已经被推翻或迭代更新了。比如从最早支持FP16，到支持BF16，及至引入TF32。从4x4尺寸，到更大的8x8。CUDA层面暴露的API粒度也在发生演进变化。如此等等。这里重要的不是结论，而是探讨这些结论产生的过程。因为这些过程，是可以迁移到下一拨workload，下一拨架构创新机会上的，具体的结论则很可能未必。

Tensor Core对数据存取的需求和FP32 CUDA core存在明显的差异，如何将Tensor Core计算能力对CUDA软件开发人员暴露成为一个蛮考究的问题。从Volta架构开始，PTX指令中引入了Warp level的矩阵计算及存取指令，并不断扩展其灵活性，从只能使用wmma.load/wmma.store/wmma.mma按ISA约束的数据组织方式来进行存取，到使用mma指令由软件开发人员根据需要进行显式的数据组织排布（灵活性提升的同时也意味着编程复杂性的增加）。以及为了简化NV之外CUDA开发人员使用TensorCore的负担所推出的CUTLASS软件库。TensorCore编程复杂性的根源在于

+   从原先相对符合朴素直觉的SIMT过渡到了Warp-level的编程范式，同一个warp内的32个thread需要按照TensorCore的数量分成8个分组(grouping),每个group内的四个thread需要协同完成shared memory到寄存器的数据加载，group与group之间也需要考虑协同。不同处理任务之间的依赖关系，增加了思考的复杂度度。

+   考虑到性能，shared memory里的数据组织方式需要注意规避bank conflict,这就引入入了另一个反朴素直觉的维度。

+   Tensor Core的粗计算粒度，使得程序员需要显式地将计算[过程](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1804.06826.pdf)进[行细](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1811.08309.pdf)致的拆解。有兴趣的同学可以参考这里和这里的两份材料，可以看到CUDA层面为16x16x16规模矩阵计算暴露的wmma::mma_sync API映射为64（FP32累加）或32条HMMA指令(FP16累加)的细节拆解过程。如果想直接使用PTX的mma指令进行编程来获得更好的灵活性，就需要自行完成相应的拆解。

在V100时代，随着Tensor Core对Conv/GEMM这类计算密集算子带来显著加速，访存密集算子以及kernel launch开销对端到端性能的影响变得也越来越大，也是从V100时代开始，自动算子融合技术开始受到更多关注。

# Links

https://images.nvidia.com/aem-dam/en-zz/Solutions/geforce/ampere/pdf/NVIDIA-ampere-GA102-GPU-Architecture-Whitepaper-V1.pdf

https://images.nvidia.com/aem-dam/en-zz/Solutions/design-visualization/technologies/turing-architecture/NVIDIA-Turing-Architecture-Whitepaper.pdf

https://images.nvidia.com/content/volta-architecture/pdf/volta-architecture-whitepaper.pdf

https://images.nvidia.com/content/pdf/tesla/whitepaper/pascal-architecture-whitepaper.pdf

https://www.microway.com/download/whitepaper/NVIDIA_Maxwell_GM204_Architecture_Whitepaper.pdf

https://www.nvidia.com/content/PDF/product-specifications/GeForce_GTX_680_Whitepaper_FINAL.pdf

https://www.hardwarebg.com/b4k/files/nvidia_gf100_whitepaper.pdf

在[大模型训练上](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2006.16668)，除了GPT-3，B[ERT类模型以外，MoE模](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2201.05596.pdf)型也受到了一定关注，相应地也催生了一系列工作，包括Google TPU之上的Gshard系列工作，GPU上的DeepSpeed-MoE工作等

https://jcf94.com/2020/05/24/2020-05-24-nvidia-arch/

https://jcf94.com/2023/02/26/2023-02-26-nvidia-arch-2/

NV 卡的数据查询

+   [https://developer.](https://developer.nvidia.com/cuda-gpus)nvidia.com/cuda-gpus

+   [https://www.techpo](https://www.techpowerup.com/gpu-specs/)werup.com/gpu-specs/

+   [ht](https://en.wikipedia.org/wiki/List_of_Nvidia_graphics_processing_units#Quadro_Kxxx_series)tps://en.wikipedia.org/wiki/List_of_Nvidia_graphics_processing_units#Quadro_Kxxx_series

科普

+   关于p2p：[ht](https://www.pugetsystems.com/labs/hpc/P2P-peer-to-peer-on-NVIDIA-RTX-2080Ti-vs-GTX-1080Ti-GPUs-1331/)tps://www.pugetsystems.com/labs/hpc/P2P-peer-to-peer-on-NVIDIA-RTX-2080Ti-vs-GTX-1080Ti-GPUs-1331/

+   关于pcie board设计：[ht](https://blog.exxactcorp.com/exploring-the-complexities-of-pcie-connectivity-and-peer-to-peer-communication/)tps://blog.exxactcorp.com/exploring-the-complexities-of-pcie-connectivity-and-peer-to-peer-communication/

+   [关于nvlink/ nvswitch:https://w](https://www.nvidia.com/en-us/data-center/nvlink/)ww.nvidia.com/en-us/data-center/nvlink/

+   [n](https://images.nvidia.com/content/pdf/nvswitch-technical-overview.pdf)vswitch white paper:https://images.nvidia.com/content/pdf/nvswitch-technical-overview.pdf

+   关于turing 架构：[https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualiz](https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualization/technologies/turing-architecture/NVIDIA-Turing-Architecture-Whitepaper.pdf)ation/technologies/turing-architecture/NVIDIA-Turing-Architecture-Whitepaper.pdf

+   [关于](https://images.nvidia.com/content/pdf/tesla/whitepaper/pascal-architecture-whitepaper.pdf)pascal(p100)架构:https://images.nvidia.com/content/pdf/tesla/whitepaper/pascal-architecture-whitepaper.pdf

+   关于volta架构：[ht](https://images.nvidia.com/content/volta-architecture/pdf/volta-architecture-whitepaper.pdf)tps://images.nvidia.com/content/volta-architecture/pdf/volta-architecture-whitepaper.pdf

+   关于1080：[http://international.download.nvidia.com/geforce-](http://international.download.nvidia.com/geforce-com/international/pdfs/GeForce_GTX_1080_Whitepaper_FINAL.pdf)com/international/pdfs/GeForce_GTX_1080_Whitepaper_FINAL.pdf

+   关于dgx-2:

+   关于hgx：[https://www.nvidia.com/en](https://www.nvidia.com/en-us/data-center/hgx/)-us/data-center/hgx/

+   [m](https://www.nvidia.com/en-us/data-center/machine-learning-benchmarks/)achine-learning-benchmarks:https://www.nvidia.com/en-us/data-center/machine-learning-benchmarks/

+   关于>8卡1080ti下p2p会出问题的一些线索：

+   [https://devtalk.nvidia.com/default/topic/970010/cuda-programming-and-perfor](https://devtalk.nvidia.com/default/topic/970010/cuda-programming-and-performance/cuda-peer-resources-error-when-running-on-more-than-8-k80s-aws-p2-16xlarge)mance/cuda-peer-resources-error-when-running-on-more-than-8-k80s-aws-p2-16xlarge

+   [https://github.com/N](https://github.com/NVIDIA/nccl/issues/51)VIDIA/nccl/issues/51