
| Max Registers / Block | 65536 | 65536 | 65536 |
| Max Registers / Thread | 255 | 255 | 255 |
| Max Thread Block Size | 1024 | 1024 | 1024 |
| FP32 Cores / SM | 64 | 64 | 64 |
| Ratio of SM Registers to FP32 Cores | 1024 | 1024 | 1024 |
| Shared Memory Size / SM | 64 KB | Configurable up to 96 KB | Configurable up to 164 KB |

## A100

[https://devblogs.nvidia.com/nvidia-ampere-architecture-in-depth/](https://devblogs.nvidia.com/nvidia-ampere-architecture-in-depth/)

[https://www.nvidia.com/en-us/data-center/a100/](https://www.nvidia.com/en-us/data-center/a100/)

[https://www.techpowerup.com/gpu-specs/tesla-a100-sxm4.c3506](https://www.techpowerup.com/gpu-specs/tesla-a100-sxm4.c3506)

1. 支持SM的数据直接从global memory中load，并不会占用 register file bandwidth
1. SM的访存是异步的，并不会block SM的使用
1. Asynchronous barrier
1. 新的同步机制，可以与异步拷贝一起使用，达到Producer-Consumer的效果
1. 粒度可控，不在局限于warp或者block
1. A100
1. L1+SharedMemory共192KB（SharedMemory最大支持164KB）
1. 40GB HBM2 内存，理论带宽1.6 TB/s（73% speedup compared with V100）
1. 40MB L2 cache（所有SM共享），size接近V100的7倍，速度接近V100的2.3倍

完整的GA100 应该包含：

+ 每个A100有128个SMs：每个GPU有8个GPCs，每个GPC有8个TPCs，每个TPCs有2个SMs
+ 每个A100有8192个FP32 CUDA Core（每个SM有64个FP32 CUDA Core）
+ 每个A100有512个 第三代TensorCore（每个SM有4个第三代TensorCore）
+ 6个HBM2 stacks，12个512bit 内存控制器

A100上的GA100实际包含：（封掉了部分硬件）

+ 每个A100有108个SMs：每个GPU有7个GPCs，每个GPC有7-8个TPCs，每个TPCs有2个SMs
+ 每个A100有6912个FP32 CUDA Core（每个SM有64个FP32 CUDA Core）
+ 每个A100有432 第三代TensorCore（每个SM有4个第三代TensorCore）
+ 5个HBM2 stacks，10个512bit 内存控制器

A100 SM架构

+ A100上的TensorCore是第三代
+ 每个TensorCore在1个时钟周期完成256次FMA（`M*K*N = 4*8*8`，V100是64次FMA）
+ 每个SM有4个TensorCore，相当于1个是时钟周期完成1024次FMA（V100是每个SM有8个TensorCore，共完成512次FMA）
+ 加速所有的类型，包括FP16、BF16、TF32、FP64、Int8、Int4、Binary
+ 引入Sparsity Struct
+ TF32加速常规的FP32计算
+ 192KB的L1+SharedMemory
+ Asynchronous SM Copy
+ Asynchronous SM Barrier
+ 针对L2 cache管理提供更多指令

## L2 Cache

To optimize capacity utilization, the NVIDIA Ampere architecture provides L2 cache residency controls for you to manage data to keep or evict from the cache. You can set aside a portion of L2 cache for persistent data accesses.

For example, for DL inferencing workloads, ping-pong buffers can be persistently cached in the L2 for faster data access, while also avoiding writebacks to DRAM. For producer-consumer chains, such as those found in DL training, L2 cache controls can optimize caching across the write-to-read data dependencies. In LSTM networks, recurrent weights can be preferentially cached and reused in L2.

# Example GPU

![](img/fb945a7ce9e26d3bb5f5b69b0a5a0d4a.png)

## GTX 285

![](img/5f07d358eb654ab3d24be5b47f01613f.png)

![](img/9d1d6e227ed56bf82926d17305bc4653.png)

1. 每个 group 32 threads。-> warp。SIMD 模式运行，share 一个 instruction stream。
1. up to 32 warps 以 FGPT 方式 interleaved 运行。
1. 最多 store 1024 个 thread context。
1. ensure every pipeline stage is occupied by a different thread instruction。

![](img/b75177d161863314c831187a68eae42b.png)

![](img/975d539877653155ae98df3fa2acda6d.png)

## Fermi arch

![](img/28bdf2ff27dccaf7c3737499ced6c1b7.png)

SM单元缓存图：

![](img/3b56618e7147788c31ac15824fcc22ae.png)

## Pascal架构

![](img/173c47f25eaa777816ed27924086ba47.png)

## Turing架构

![](img/fe1164180e39649abb49a7566912f392.png)

## Turing arch

![](img/945f8ff6fdf2ebe8cc20024d9b9322ec.png)

## Volta架构

在SM单元内部缓存方面，Turing与Volta采用了同样的结构，不同点主要在于数值的变动。而两者与Pascal的SM缓存结构相比，主要有以下几个差异点：

+ 融合了SM单元的共享 L1 data cache 与 shared memory，采用灵活分配策略（以Turing T4，总容量为96kb的共享缓存为例，两者的比例可在 64+32 KiB与32+64KiB间动态调配），在处理数据交换需求少的任务时，实际可用的L1数据缓存提高了两到三倍。
+ 指令缓存更加靠近计算单元，由原本的PB块外部共享转变为PB块内部独占
+ 每个SM块寄存器文件的总量没有变化，但每个CUDA核实际分配的寄存器容量增加了

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

NVLink的技术细节简要来说可以分为NVHS，Sub-link，Link，Gang四个层次。一条NVHS的link提供单向20Gb/s的传输带宽带宽，8根NVHS构成一个Sub-link，两条Sub-links组成一条用于双向连接的Link，P100架构下的单机八卡配置，不同GPU之间会由四个Link组成一条Gang，所以Gang的双向汇总带宽是`20Gb * 8 * 2 * 4 = 160GB/s`，单向汇总带宽80GB/s，是PCIe提供带宽的5x。

# Volta 时代的思考

为什么选择了先支持FP16，而不是Google提及的BF16？为什么考虑选择了4x4尺寸的TensorCore，而不是更大的尺寸？如何figure out出来和FP16配套的Loss scaling的训练策略？TensorCore的编程API如何对外暴露来尽量避免和现有CUDA体系形成撕裂？怎样让NV之外的开发者也具备在TensorCore上开发程序的能力？这里涉及到了大量的技术、非技术因素的综合权衡。一些结论决策，当时看是合理的，但现在来看，已经被推翻或迭代更新了。比如从最早支持FP16，到支持BF16，及至引入TF32。从4x4尺寸，到更大的8x8。CUDA层面暴露的API粒度也在发生演进变化。如此等等。这里重要的不是结论，而是探讨这些结论产生的过程。因为这些过程，是可以迁移到下一拨workload，下一拨架构创新机会上的，具体的结论则很可能未必。

`Tensor Core`对数据存取的需求和FP32 CUDA core存在明显的差异，如何将`Tensor Core`计算能力对CUDA软件开发人员暴露成为一个蛮考究的问题。从Volta架构开始，`PTX`指令中引入了Warp level的矩阵计算及存取指令，并不断扩展其灵活性，从只能使用`wmma.load`/`wmma.store`/`wmma.mma`按ISA约束的数据组织方式来进行存取，到使用`mma`指令由软件开发人员根据需要进行显式的数据组织排布（灵活性提升的同时也意味着编程复杂性的增加）。以及为了简化NV之外CUDA开发人员使用`TensorCore`的负担所推出的`CUTLASS`软件库。`TensorCore`编程复杂性的根源在于

+ 从原先相对符合朴素直觉的SIMT过渡到了Warp-level的编程范式，同一个warp内的32个thread需要按照`TensorCore`的数量分成8个分组(grouping),每个group内的四个thread需要协同完成shared memory到寄存器的数据加载，group与group之间也需要考虑协同。不同处理任务之间的依赖关系，增加了思考的复杂度度。
+ 考虑到性能，shared memory里的数据组织方式需要注意规避bank conflict,这就引入入了另一个反朴素直觉的维度。
+ `Tensor Core`的粗计算粒度，使得程序员需要显式地将计算[过程](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1804.06826.pdf)进[行细](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1811.08309.pdf)致的拆解。有兴趣的同学可以参考这里和这里的两份材料，可以看到CUDA层面为16x16x16规模矩阵计算暴露的`wmma::mma_sync` API映射为64（FP32累加）或32条`HMMA`指令(FP16累加)的细节拆解过程。如果想直接使用`PTX`的`mma`指令进行编程来获得更好的灵活性，就需要自行完成相应的拆解。

在V100时代，随着`Tensor Core`对Conv/GEMM这类计算密集算子带来显著加速，访存密集算子以及kernel launch开销对端到端性能的影响变得也越来越大，也是从V100时代开始，自动算子融合技术开始受到更多关注。

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

+ [https://developer.nvidia.com/cuda-gpus](https://developer.nvidia.com/cuda-gpus)
+ [https://www.techpowerup.com/gpu-specs/](https://www.techpowerup.com/gpu-specs/)
+ [https://en.wikipedia.org/wiki/List_of_Nvidia_graphics_processing_units#Quadro_Kxxx_series](https://en.wikipedia.org/wiki/List_of_Nvidia_graphics_processing_units#Quadro_Kxxx_series)

科普

+ 关于p2p：[https://www.pugetsystems.com/labs/hpc/P2P-peer-to-peer-on-NVIDIA-RTX-2080Ti-vs-GTX-1080Ti-GPUs-1331/](https://www.pugetsystems.com/labs/hpc/P2P-peer-to-peer-on-NVIDIA-RTX-2080Ti-vs-GTX-1080Ti-GPUs-1331/)
+ 关于pcie board设计：[https://blog.exxactcorp.com/exploring-the-complexities-of-pcie-connectivity-and-peer-to-peer-communication/](https://blog.exxactcorp.com/exploring-the-complexities-of-pcie-connectivity-and-peer-to-peer-communication/)
