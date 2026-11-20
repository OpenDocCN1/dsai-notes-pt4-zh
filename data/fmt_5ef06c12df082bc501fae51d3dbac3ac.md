
# Triton Conference 2023 - talk notes

> 来源：[`https://learning.feishu.cn/docx/CiMUdyaO4oTfKqxIzXhcc9ARn7I`](https://learning.feishu.cn/docx/CiMUdyaO4oTfKqxIzXhcc9ARn7I)

TODO：

`https://www.youtube.com/playlist?list=PLc_vA1r0qoiRZfUC3o4_yjj0FtWvodKAz`

Triton Conference 2023

10 videos

First Triton Developer Conference

加上几个 PyTorch Conference 上的 triton 相关 talk。

# Summary

几个普遍关心的关键词

1. 在 nv hooper 上的性能表现

2. block pointer [`https://triton-lang.org/main/getting-started/tutorials/08-experimental-block-pointer.html`](https://triton-lang.org/main/getting-started/tutorials/08-experimental-block-pointer.html)

![](img/c3fa06a7020a20dc43acafd224ef0948.png)

![](img/aa35d3af8cef1dbac4e8311b51a71ed6.png)

![](img/41aa06d92c5869a27f510d4f160f12ec.png)

Microsoft - triton-shared

Introducing a middle layer: Linalg + HW vendor Dialects

![](img/f96a1c49bc06a2e0094cbc536f58aa25.png)

点评 @ 2025.03

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

`https://www.youtube.com/watch?v=AtbnRIzpwho`

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

`https://www.youtube.com/watch?v=p13HpZv2S3Q`

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

`https://www.youtube.com/watch?v=G951lCm_qnk`

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

`https://www.youtube.com/watch?v=ZGU0Yw7mORE`

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

`https://www.youtube.com/watch?v=KMRl-SBbTDk`

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

`https://www.youtube.com/watch?v=y2V3ucS1pfQ`

Triton for All: Triton as a device-independent language

Ian Bearman (Microsoft)

Sep 27, 2023

## Middle Layer

![](img/1d06569143d9724e88fec0cb114a19d9.png)

思路：

1. 用 linalg 代替 tritonGPU IR，llvm 再往下，又是可以共享的

2. triton IR is really like modeling a single block of computation。试图 raise to the entire grid？No，依旧是 single kernel or single instance of a kernel。-- 需要把 grid-block-thread 的 Modeling 搞清楚，并定义好，每一层的调度、IR 分别要解决什么问题，需要什么样的 IR 来存储信息。

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

1. affine_map

2. interator_types

3. bb0: 计算的 body

![](img/b947c41110c14b3bfc88c6fe99cd4fc8.png)

## Getting Involved

![](img/6fa9cf9c18fdbf1e3645bf32c556f544.png)

## frequently asked questions

![](img/6e794b2bef51ad7c05bbea91a82542fd.png)

any plan or thoughts on coupling Triton to TOSA?

Q&A first question, we see a pretty good 1:1 mapping for most of the triton instructions like the TT or TL instructions to linalg operations.

如果用了 TOSA，则提升了 IR 的 level。更适合描述 graph，grid level，or multiple kernels together 的计算。
