# Triton Conference 2023 - talk notes

> 来源：[https://learning.feishu.cn/docx/CiMUdyaO4oTfKqxIzXhcc9ARn7I](https://learning.feishu.cn/docx/CiMUdyaO4oTfKqxIzXhcc9ARn7I)

TODO：

https://www.youtube.com/playlist?list=PLc_vA1r0qoiRZfUC3o4_yjj0FtWvodKAz

Triton Conference 2023

10 videos

First Triton Developer Conference

加上几个 PyTorch Conference 上的 triton 相关 talk。

# Summary

几个普遍关心的关键词

1.  在 nv hooper 上的性能表现

1.  block pointer https://triton-lang.org/main/getting-started/tutorials/08-experimental-block-pointer.html

![](img/c3fa06a7020a20dc43acafd224ef0948.png)

![](img/aa35d3af8cef1dbac4e8311b51a71ed6.png)

![](img/41aa06d92c5869a27f510d4f160f12ec.png)

Microsoft - triton-shared

Introducing a middle layer: Linalg + HW vendor Dialects

![](img/f96a1c49bc06a2e0094cbc536f58aa25.png)

点评 @  2025.03

这个出发点，还是太不工程了。

从某一层开始，往下都是 hw vendor dialects。这个设计等于没有设计。

triton 作为一个框架，要解决的核心问题是什么，要保留的核心设计是什么。要基于这个理解做新的设计。

triton GPU IR

![](img/02f79ca33173d7196f597249b33407f1.png)

算子覆盖范围

![](img/ba404bca726f01b08727f2168b728a60.png)

Q: how do you generate nested for-loops

A: Take multiple loops and flatten them into a single for-loop. ends up as indexing math.

Q: do you see any value in having a sort or topk kernel written by Triton?

A: hard to implement a sort in Triton. maybe sort will be coming soon?

如果 topk 的 k 很小，可以用 ArgMax。topk 可能更早一点可以提供，但 sort 更难一些。

# Triton Compiler - 2023

https://www.youtube.com/watch?v=AtbnRIzpwho

Triton Compiler

Thomas Raoux, OpenAI

Oct 25, 2023

## Introduction

细读 triton 的介绍，一点感悟。

很多人把重点理解为：allows no GPU programming experience to write code。

No。重点是 high performance GPU code。

前面的 no GPU programming experience，重点也不是 python or cuda 语法差异的 experience。

这是三流的问题，高水平团队是不屑于解决的。更不要说 OpenAI 的团队。

GPU 的 experience，关键还是 high performance programming 的 experience。

需要理解 hardware arch，理解 h2d/d2h 细节，精通 GPU 显存管理，精通 SIMT 调度。很难的。

这是一流的问题，也是 Triton 自我介绍的 key point。

![](img/a09df4535edc18debc08607767e1dd67.png)

## Design Principles

![](img/966a7b9f4f36c641e91142888cc4d360.png)

TODO: 需要整理一个类似的表格，在 RISC-V CPU 上，哪些交给 compiler，哪些留给 user control。

![](img/46e616fb5de5a5182caa13599bf4a056.png)

TODO: 需要贴一个更清晰的 code 看一下

![](img/8eb96e9343fd4de007113c214b666c3a.png)

## Compiler Architecture

![](img/74f89f53127116301102ef1c9f6232d4.png)

![](img/c8d2180cb3c18b8197244d09160ef99b.png)

triton GPU IR

![](img/7046326caa1bf15238eb61dc74818b87.png)

![](img/eb0cf35729b36cfbf94130a8786fd1dd.png)

# Deeper Dive of Inductor with TritonIR

https://www.youtube.com/watch?v=p13HpZv2S3Q

PyTorch 2.0 and TorchInductor

Horace He (Meta)

Sep 27, 2023

从 11~12min 开始看，前面讲 torch inductor，与 triton 关系不大。

## Softmax Example

支持 dynamic shape

![](img/d700fad05d7b81939589869e54d8bc2a.png)

![](img/2dadf25d42cb149b389038a530e45d8d.png)

自动决定 reduction 是 persistent manner 和 non-persistent manner。

![](img/9ccb0f02c81672400ce46bc46b468ede.png)

不同的 softmax 变体，都会 lower 到同一个 triton kernel，并用 first principle 的 kernel fusion。

![](img/98adeef917d2814c3c1ed6fb443b2b2c.png)

## Matmul and Templates

![](img/6cc6f44cee135451103f1a46c1b991bc.png)

triton 里面的一些 tricks

L2 swizzling

re-materialing

![](img/a78e1e24aa25ab5fc7fabc424001f30a.png)

## For-Each Kernels

![](img/2e142d3130e50b5d63d8973ce26a22fd.png)

![](img/674e1ca825340a5495e81c65e1d21c0a.png)

![](img/1251ac27987f13d35f1fa7c33ae8ab55.png)

## Q&A

Q: how do you generate nested for-loops

A: Take multiple loops and flatten them into a single for-loop. ends up as indexing math.

Q: do you see any value in having a sort or topk kernel written by Triton?

A: hard to implement a sort in Triton. maybe sort will be coming soon?

如果 topk 的 k 很小，可以用 ArgMax。topk 可能更早一点可以提供，但 sort 更难一些。

# The Triton Language - 2021

https://www.youtube.com/watch?v=G951lCm_qnk

The Triton Language

PHILIPPE TILLET

Dec 16, 2021

## Introduction

要点：用户定义的 Tensor 在 SRAM 中。

![](img/4260b0217a327ad6f6656dc956ee5066.png)

project history

![](img/50f32d6c56795fa7e184d6625ab52d6e.png)

## Code Example

![](img/478716679a202c5f22c65297f5a7d62c.png)

![](img/8e5d96b67c503247a70ceae2912be4cd.png)

![](img/715a18d67cbc598937d93b6727d96cb0.png)

## Performance

![](img/5628f1269f08404d8a22156066b3fde1.png)

![](img/16695610856f49bb8e283620bc90a6e5.png)

![](img/efe5ebf830cea41bdb2ca4a027cc1362.png)

![](img/c9a7c5efd4144d306d6822684bd78dcd.png)

# Qualcomm: Vectorization of Triton Kernels

https://www.youtube.com/watch?v=ZGU0Yw7mORE

Qualcomm: Vectorization of Triton Kernels

Javed Absar (Qualcomm)

Sep 26, 2023

![](img/9c130fdc8223c76b5d47253345eb786d.png)

## Triton at Qualcomm

![](img/d693442f85f9f14bd2d61681467d34b7.png)

![](img/0b45dda5fca326af8cf22cfdf48fdd57.png)

## Scalar Code Gen

![](img/7391e1ba5982f8ab7cfcff5cd97b4dea.png)

![](img/5dd73aa2ed92bdd7cec29c41341de2c1.png)

![](img/efe03be8facfcf63e34f372692e2b25f.png)

![](img/23f5771f00e79aefdeba9c2c2f387d69.png)

## Vector Code Gen

![](img/59d32a13bc1ff4c8e0ad97c61c6db75c.png)

![](img/267e68ed9c6659c2b5dbd7003bff0fbb.png)

![](img/b2ebce11925c0d9e8412a9858419bee7.png)

## Future Plans

![](img/8313beee2f2821ff873ddb566350377f.png)

# Nvidia: Hopper support in Triton

https://www.youtube.com/watch?v=KMRl-SBbTDk

Hopper support in Triton

Jun (Nvidia)

Sep 26, 2023

## Recap

![](img/bae9fc0886567c33e1c3e1a642fd74ac.png)

![](img/68665f1225ffb336f194acf6229087a1.png)

![](img/adad7e80f643dd6414711cddb0105f66.png)

## Motivation

![](img/b41f1cb07c4cf9407569cffcba8ba324.png)

![](img/90ed65ee1c6a3f7213d033dd29188220.png)

## New Features of Hopper

![](img/8b9426b6166fdcb2d7a1ea1471307f1f.png)

![](img/312f83f0824bda057328d1b3a6159635.png)

![](img/4f931ff25c6c1336b0b4226c3c6a4173.png)

![](img/7ddfa62bc49645b2642650b5db4c5395.png)

![](img/d59eed421ddda1f1fe3b2735a9d17a85.png)

![](img/9430b5d17a8d028e48fddaf6b703b703.png)

![](img/7a541a6364d6f58527c1999638a21c86.png)

## Dialects and Passes

![](img/184e5717fffd83feed36896315c1e7f2.png)

![](img/9d696f23250c76bfc0b2f1e4b4ed445a.png)

![](img/89cc36d2feea9b7bfaf6c1a51815ec7c.png)

![](img/3c243011b26a99c729e3aebf77d7ca2b.png)

![](img/b2087ac1ee53c7315fa9e6945350d6a0.png)

![](img/023c1e984b313549baf35c1f5843e9a5.png)

## Performance

![](img/0a62efcd2e842d1cb16848f4317de956.png)

![](img/81f55baba6caa1f123fd8e32c655d9b5.png)

![](img/17c9b629f2ae64ee2f0b852a7f1eef3c.png)

![](img/32f22d5bea57a6b5cb58393e96105b47.png)

# Microsoft: Triton for All

https://www.youtube.com/watch?v=y2V3ucS1pfQ

Triton for All: Triton as a device-independent language

Ian Bearman (Microsoft)

Sep 27, 2023

## Middle Layer

![](img/1d06569143d9724e88fec0cb114a19d9.png)

思路：

1.  用 linalg 代替 tritonGPU IR，llvm 再往下，又是可以共享的

1.  triton IR is really like modeling a single block of computation。试图 raise to the entire grid？No，依旧是 single kernel or single instance of a kernel。-- 需要把 grid-block-thread 的 Modeling 搞清楚，并定义好，每一层的调度、IR 分别要解决什么问题，需要什么样的 IR 来存储信息。

![](img/0f5f9b0580a3c03acfc754eb8c9ba4fd.png)

![](img/3ef211e73e5a32bb4be2fe2996abfc12.png)

## Linalg

![](img/f86c1bda0a29ec6f0038bfb4a14c105e.png)

![](img/38098580360df6db9c73c1ff41f8f29f.png)

load

![](img/041445e2ab64d4fa7d7fe2f007af4ecf.png)

compute

这是整个 program 中，唯一推荐 convert to linalg 计算的部分。

that is the part that's already operating on values.

this is a mixture with the load/store operation.

可能更好的 design: make a block pointer of the main program model and map it to memory.

block pointer 可以把这些红框变得 much much smaller。

![](img/87dd8d48f93a126e73abc129d9e12b82.png)

store

在下面，没截图。

1.  affine_map

1.  interator_types

1.  bb0: 计算的 body

![](img/b947c41110c14b3bfc88c6fe99cd4fc8.png)

## Getting Involved

![](img/6fa9cf9c18fdbf1e3645bf32c556f544.png)

## frequently asked questions

![](img/6e794b2bef51ad7c05bbea91a82542fd.png)

any plan or thoughts on coupling Triton to TOSA?

Q&A first question, we see a pretty good 1:1 mapping for most of the triton instructions like the TT or TL instructions to linalg operations.

如果用了 TOSA，则提升了 IR 的 level。更适合描述 graph，grid level，or multiple kernels together 的计算。

for the first one like arithmetically mapping，

the first programming model is basically a tensor of pointers。

# AMD - Triton to AMD GPUs

https://www.youtube.com/watch?v=8o7Jhbv8xek

Bringing Triton to AMD GPUs

Jason Furmanek, Lixun Zhang (AMD)

Sep 26, 2023

## Project Introduction

![](img/ea7d79ea36393c0cb907b3ca59d866d4.png)

![](img/976285c4e5eaaed706de67ef56b0c63b.png)

![](img/176fab22f53b8d752305c059fe62c31a.png)

![](img/3b68077a203ff6ab055219ea1c6dedab.png)

## Optimizations

optimizations on AMD instinct GPUs

![](img/cd2e908ecfdc0f31a4d58f11762ae141.png)

![](img/6c25b7852ac539dc7ac3a3ea5caeb543.png)

![](img/51cb6483f50f87b9cb9a9baf10197add.png)

![](img/fdf4bbc0b86396f844a4749c65f38205.png)

![](img/6712fe1ab9c2e2882b284e134a8db4f3.png)

## Performance

small m1k 很慢的原因分析：

workload 太小了，并行度低，内部的 warp 调度没有针对性的优化。

![](img/8c8502ba4fa8368302233a777eb93fa4.png)

![](img/e4d3b4c7f5548d57abdc31768f185be2.png)

## Q&A

Q: for all the flash attention optimizations,

do they require any changes to the tutorial or triton op or they are just compiler optimizations?

A: 1. the pointer arithmetic stuff requires changes in the Python Level.

1.  everything else is the compiler stuff.

# Intel GPU - Triton

https://www.youtube.com/watch?v=KwSbLAZ-xg4

Adding Backends for TorchInductor: Case Study with Intel GPU

Eikan Wang, Intel

Staff AI framework Engineer

Oct 25, 2023

作者 focus on both intel GPU & CPU

this session talks about GPU only。

一句话总结要点：intel GPU backend for indicator on top of Triton.

用的 Triton GPU IR。

从 PyTorch 2.0 开始，支持在 Dynamo level 增加 backend。直接处理 fx capture 的 graph。

但，Intel GPU backend 选择在 inductor level 增加 backend。

动机是，尽可能复用 inductor 的功能和 optimization passes。比如：

1.  inductor Loop IR

1.  loop of fusion optimization

1.  the inline optimization

![](img/e5b962c5316593a895e2e7886ce1edbb.png)

BaseScheduling: produce the device code.

we just need to enumerate the schedule node and produce the device code.

WrapperCodeGen: glue the kernel code and the infra? code

inductor 当前有不同的 backend 支持不同的 GPU，比如 NV，AMD。

intel 尝试提供一个统一的 runtime 支持不同 GPU。

![](img/8efef59807abb747fb396b5d79c5027b.png)

![](img/b25c63ac2b7aa8b32a8f7049a9f67eb8.png)

![](img/a0504adb6c7689d53767f407ed3af463.png)

outlier model 的数量，比 training 好了很多。大概有 10% 的 outlier model。

![](img/fab90cc2293edade5f317f3457366bd9.png)

performance analysis, 2 major reasons:

1.  inductor 的 configuration 对 Intel GPU 不够优化。比如，number of warps

1.  intel GPU backend 不够好。基于 triton 接入的，需要优化 triton 侧。比如，triton 的 load / store align with 优化过的版本。

![](img/6e1b79415ec6e35c8a1b45fa301894d4.png)

break down 看一下加速的来源

1.  fused kernel 基本能得到 impressive 的加速空间。

1.  triton 的 matmul 和 batch matmul 会 fallback 到 aten 算子上，所以，基本没有加速。

![](img/251fa33c6c83f5d26e0d7334f1d2832a.png)

in terms hardware behavior

1.  line 3, GPU Memory Access, aten 算子全程都有，triton 只在算子 launch 时有一点。

1.  倒数第 2 行，L3 cache 的 bandwidth。triton 也好非常多。

![](img/7e48c74ff23b5282946e7ab1ef15d364.png)

进一步的优化空间

1.  仍然有 ~31% 的 stalled by GPU barrier. 在 Intel GPU 上，计划通过 named GPU barrier 消除。

1.  编排 barrier arrive and wait and other no data dependency instructions to mitigate the stalled overhead.

![](img/94b52bd3b2184d4e0b03f578f2ff517b.png)

![](img/53ab8060bda7b53a85c3a9933bc6d832.png)

![](img/dd8ddec906d90876d7c4b85846d85529.png)

![](img/67533da47dc0109597beb677edfd5a7f.png)

# Intel - XPU Backend

https://www.youtube.com/watch?v=KtC1MBb2cy8

Intel XPU Backend for Triton

Eikan Wang (Intel)

Sep 26, 2023

XPU means: Both GPU & CPU

![](img/886ace653a2a11a5911ecafc9bf7fcae.png)

## Software Stack

![](img/05d9476a5f3ff00d8bf835ed5c0e087a.png)

## Performance

![](img/beaae713b2bf4bedcab5dfe226fd2ffa.png)

性能不好的 2 个原因：

1.  torch inductor side: 默认的 number of warps & number of cta，不适合 Intel GPU。需要调整一下，增加并行度。

1.  triton / intel GPU backend side: vectorization 做的不好，不充分。

![](img/9484ba0af0687e3fd1316f8b9f4f5486.png)

![](img/c04e47edd6dd4076b9f0fc52b506e974.png)

fusion 的效果最明显。

![](img/cc7b6eb219d78febfad2f84b09749a54.png)

在 Intel 的其他 talk 里，讲过。

![](img/ffd0dfd9821cb3b9e74cf098db8e75e1.png)

![](img/a4d9f2cc26e8908d0ac79336ce6982ab.png)

## Future & Conclusions

要点，功能上，强调了 dot 和 block pointer。

![](img/fd6c157fc1176444ff10c7a28b7d5cb5.png)

![](img/e6a143eb7aebcda3b4230d7aa746d8d6.png)

# MTIA Meta

https://www.youtube.com/watch?v=ZtiiMHSWhw8

Triton for MTIA

Roman Levenstein et al, (Meta)

Sep 26, 2023

MTIA 是 META 的 ML accelerator

MTIA: First Generation Silicon Targeting Meta's Recommendation Systems

https://dl.acm.org/doi/10.1145/3579371.3589348

http://mail.shadesofsue.com/publications/3579371.3589348.pdf

MTIA v1: Meta’s first-generation AI inference accelerator

https://ai.meta.com/blog/meta-training-inference-accelerator-AI-MTIA/

![](img/5f37d3683f939fffb2e5efc1a8493ad6.png)

## What is MTIA

![](img/a272a809471044d612adf68e836d6fd2.png)

里面也有个 fabric interface + Command Processor

![](img/56df6fc64f5f9cfdd035fd4cfd9f1ee0.png)

![](img/0c7fa9887822c716102266ef66b7881b.png)

## Why Triton for MTIA

![](img/a2a6ed8ee31db784a97b214d5049f9af.png)

![](img/3e90b6031538c9cae5f4a22ef746e586.png)

## Software Design

a year ago, there was no Microsoft linalg staff.

左侧 / 红色，是 GPU version

中间 / 蓝色，是作者认为正确的 triton implementation

右侧 / 橙色，是 MTIA 选择的路线。

写了 5k - 10k 行 python code 来生成 c++ code。然后在 MTIA hardware 上运行。

![](img/f372f3930ddde1d66e963fd4e61e73b7.png)

![](img/2ae5531bf309a143dcf8ed42265004b1.png)

![](img/a79b886e7b34fa0c52ecf6754c08266b.png)

## Next

![](img/a1ca9d1bfe2bf512f7a804de090fd955.png)

accelerator 对标量 load/store 效率比较低。此处需要优化。

什么是 block pointer？

![](img/995efc0dfcb50f481187b047a8765d90.png)

在 triton 的语言 level 支持 async 的 computation。

![](img/6e60cc377218baedebb8c9672c83d4d4.png)

Communication 通信

![](img/ec881de0087418e83fe233fdf26b2085.png)

## Conclusion

![](img/25ad69576703e9455e5bac9ec1936b4a.png)

# Google - fusions in XLA

https://www.youtube.com/watch?v=UIvMOeNdtrQ

Using Triton IR for high-performance fusions in XLA

George Karpenkov (Google)

Sep 26, 2023

## OpenXLA Compiler

![](img/0ba82f7dd5052163ad640d9b0557d079.png)

![](img/e9f130b6b16c3abd480aebbc1c7c8ea7.png)

![](img/a720dc575ff6a21ca68ceab85e218427.png)

## XLA Fusion

![](img/d27b7c76df8b036775aacced7c0dc5ae.png)

![](img/666f00c361193db532e0b05592e04b1c.png)

![](img/28fa114b6897bb96088a25e5a22fc085.png)

![](img/bccb61e953df3c9e5e2adfbfdafcd2d1.png)

![](img/726d3d5abc990329e9df4a4053e66a0b.png)

![](img/87bf5cbae18550daddd1db0d1d15040b.png)

![](img/aa3447941e69a8fdd4fd7ab82e43b7ec.png)

![](img/15abf09be97a7f67b1be8420f7949017.png)

## Running Example

![](img/9a3e6613e13c9a912aa51373c0896be7.png)

![](img/14c933beedae44d135b01c7d01d7cb7f.png)

## Performance

![](img/211f2ecaa425c18ada8a9ac7f60d1926.png)

## Future work

![](img/400116d011e49dc183617377c366e572.png)

## Q & A

在 hooper 上，有什么有趣的变化或者困难吗？

1.  在 hooper 上，获得了不错的加速。但主要来自于 matmul。

1.  相比于 cutlass，主要是 matmul 需要优化，其他的差距并不大。

1.  又提到了 block pointer。

block pointer provides a much better abstraction level than arbitrary loads and stores.

fusion 和 tiling 的 cost model

what factors to take into account

这实际上是 2 个问题

1.  Is there a cost model we should apply during Fusion so when we do Fusion we do not concretize the tiling.
we just check if the dimensions are compatible. during fusion, we mostly check whether you run into compute bound yet or was it you can use it for free

1.  tiling. what you tried is to do a decision first and then try to predict the best tiling for a matmul using a decision for it.

# Nvidia - Grouped GEMM in Triton

https://www.youtube.com/watch?v=_rrhYbvNIx0

Writing Grouped GEMMs in Triton

Vinod Grover (Nvidia)

Sep 27, 2023

## Overview

![](img/1318582df536c07bedb26b2463373bcc.png)

![](img/f0c2afae1eefde8bf80c96f29325156e.png)

## Motivation

![](img/cd3bceb380fb73b10b1d1c7f142fa1e8.png)

![](img/8f6896ebccf26ff11f278c36e470dbd0.png)

![](img/c5f2a316227fc0889274f7b1235bd68f.png)

lot of IDLE

![](img/884eb650469d4100e33c4b46c04102bc.png)

## Grouped GEMM

![](img/a565a222996ff13f66c9c1430f33ba62.png)

![](img/416d0032170570da9812349fb8aa36cd.png)

![](img/df8675d2b20971f25b93087aea54c90a.png)

![](img/9fa075876da8a1d634d30814d1739905.png)

## Triton Implementation

![](img/cb8e386716d5f50ee855567e9191a3ad.png)

![](img/63566ae21226cd210d9c0b71edaaab3a.png)

## Performance

![](img/f107929d41b402addcef67722b1814e9.png)

## Conclusion

![](img/246f6559647db4e2147e32c1f0ee6458.png)

# Pallas: A JAX Kernel Language

https://www.youtube.com/watch?v=OR8NZyTz-yo

Pallas: A JAX Kernel Language

Sharad Vikram (Google)

Sep 26, 2023

![](img/696515bb0e8723d65cebab643ae57fa2.png)

![](img/eae5a8843703f670e8acc7ad3f785a63.png)

![](img/67586f6fdf6b31b6306a27f82722f64a.png)

![](img/c71a2a16cb9a562419a62582768068fc.png)

![](img/30502049f4659075c2d46d77dd5fed01.png)

![](img/f8a26582150887bb612b1b7a610d14ca.png)

![](img/eabf3da7411c014f89374d419ba65872.png)

![](img/5c9f4f8eec0d2eb70dccee28c017edfc.png)

![](img/4f43f964d83f8c285c75af7afa5d00a0.png)