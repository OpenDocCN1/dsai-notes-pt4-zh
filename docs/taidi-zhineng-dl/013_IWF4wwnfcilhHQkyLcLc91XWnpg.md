# PyTorch 2.0: Compiler - talk notes

> 来源：[https://learning.feishu.cn/docx/FObqdiy5QoDh9axrf6lc6smsnVd](https://learning.feishu.cn/docx/FObqdiy5QoDh9axrf6lc6smsnVd)

published: https://zhuanlan.zhihu.com/p/680288200

# Summary

8 个 PyTorch conference 的 talk，基本能系统的理解 PyTorch Compiler 设计。

总时长小于 120min，非常推荐。新手理解可能有难度，或许要 2-3 天时间。

章节的顺序，就是推荐的阅读顺序。

深入的重点：

1.  Dynamo: Captures PyTorch programs safely using Python Frame Evaluation Hooks

1.  Loop-Level IR

1.  Backend -> Triton / OpenMP

OPs

1.  PyTorch 有 2198 个 op，且，历史原因，有些算子拿不到 shape 等 basic information。

1.  ATen 有 750 个 op，purely functional。

1.  PrimTorch，~250 primitive operators。compilers can reclaim performance via fusion。

1.  Loop-Level IR 有 50 个 ops。implemented in Python, making it easily hackable and extensible.

3 大加速来源

![](img/69ec120c5fe077a83b0ec42329de1c62.png)

Torch Compiler 分 3 步：

1.  Graph Acquisition: Dynamo (forward) + AOTAutograd (backward)

1.  Graph Lowering: ATen / Prim IR

1.  Graph Compilation: TorchInductor

![](img/67065f3c253ed0d611f1e68e131e3756.png)

TorchInductor 分 4 层。3 key technologies：

1.  核心 IR 是 loop level IR，是 python callable。做 codegen 或者 analysis，只需要直接 execute。

1.  用 SymPy（一个符号计算库）支持动态 shape。

1.  在 CPU 上用的 OpenMP，跟 intel 一起搞的。GPU 选了 OpenAI Triton。

![](img/748e3f84a864974b24666a604a4429d1.png)

compiler stack 最终的 1 件事情，是什么？

-- graph capture。

![](img/6cda5ee708621400f6c9c61d76b76cc4.png)

Dynamo 的 2 个精髓：

1.  partial graph capture。遇到不支持的，就保留并拆分出前后的子图，分别编译。

1.  guard。解决了 trace 的经典难题 -- control flow 等导致 capture 不能用。guard 会自动报警，不会出错。

AOTInductor = torch.export (whole graph capture) + Inductor (AOT compilation)

整图导出，脱离 PyTorch 和 Python 做 deployment 用的。

实现上，复用 TorchInductor 的大部分 stack，主要 hack 了 backend 部分。

runtime 用了 AITemplate 的设计。（或许可以换成 Nvidia Triton？）

Dynamic Shapes Support，2 个要点：

1.  shape 变成 symbolic expression。用的 SymPy。

1.  control flow 是 shape 信息的重要来源。给判断条件加 guards。

Inference 的 time measurement 如下图，加速的 4 个 tips：

1.  compcompilation time 加速：用 cache。

1.  load Model 加速：使用 mmap 的 API。

1.  inference 加速：用 CUDAGraph。因为额外的 memory cost，暂时还不是默认的配置。

1.  import torch 加速: 手写一个 lazy modules load。

![](img/ede54e039647de09d7a796ea414a5dc2.png)

# PyTorch Conference 2022 Keynote

https://www.youtube.com/watch?v=U5M6jY6BWy0

PyTorch Conference 2022 Keynote

Soumith Chintala

Dec 13, 2022

PyTorch 2.0 announced here

## 目标设定 - why Torch 2.0

背景强调了 2 个 overhead: Framework, Memory bandwidth.

其中，Framework overhead 很少被提及。

从 V100 -> A100

+   memory bandwidth ->1.7x

+   compute capacity -> 5x

![](img/73f0095ee88cd091ddc27658647053a5.png)

简单来说，3 个目标，1 个约束。核心还是 30% speedup

目标：

1.  加速 30% 的 training speed。

1.  更好的支持 backend 接入

1.  更好的 distributed capability

约束：还是写 python 代码，易用性体验不下降。

![](img/9fa01e7a3c73428cd9142ec25776cd52.png)

选了 3 个 benchmark repo 来量化目标完成度：

1.  huggingface: LLM 模型

1.  TIMM: 传统 CV 模型

1.  TorchBench: 自建的 benchmark 仓库，来自 GitHub 开源模型 repo。

![](img/88dce5cb332685f9351ec87d43b5ba36.png)

目标达成情况

自动混合精度的 speedup 效果 (训练 or 推理？忘了，猜测是训练)

![](img/4b27e926bfc25bebcb4f39338c902cd3.png)

## 技术拆解

3 大加速来源：

1.  kernel fusion

1.  out-of-order execution

1.  automatic workload placement

![](img/865c02b67e9f874345fa5ec84b44ab03.png)

PyTorch 的 workflow 理解：3 个 action，4 个 state

![](img/d3f262f1737882c576c57c4c88e2832f.png)

## Graph Acquisition

当前，graph acquisition 分 2 套方案：

1.  dynamo 解决 forward。

1.  AOT Autograd 解决 backward。

如何 capture 一个 dynamic graph，很难。

PyTorch 花了 5 年时间，做了三四个 project。比如，

1.  jit trace，当 violate assumption 时，totally wrong。

1.  jit script，需要修改代码，甚至要花 2 个月修改 pytorch 代码。

1.  FX。从未正式 release。

1.  lazy tensor & dynamo 是 2 project working on。做相似的事情，但 dynamo 是更好的技术路线。

![](img/6e343231f51112e12bd93cf15a7ef86f.png)

### dynamo

dynamo 把图切成能 compile 和不能 compile。

不能 compile 的用 eager mode 运行。

能 compile 的，交给 backend 去 compile。

![](img/987c523325a0934d3132fc098f9ee341.png)

![](img/168d477d6ed40207a657baa8898963cb.png)

### AOTAutograd

用 AOTAutograd 解决 backward 的 trace 问题。

![](img/9b7453063d5b0215c257c69e5cc29f7d.png)

## Graph Lowering - loop-level IR

2000+ 个 op，backend 如何支持？

不要 reduce，而是 decompose op。

by 2023，已经 settle down 了，50 个 OP 在 loop-level IR 里。

![](img/f55e34025dba64f6753d031808dd7d2a.png)

## Graph Compilation - TorchInductor

设计原则

![](img/93ddb7b180ffd620c823c4a3812d755e.png)

个人理解：

1.  breadth-first 很重要，先能用，再提升 performance

1.  这个难度其实更大，前期就得设计好 API。提升 performance 期间，用户 API 是不能变的。

1.  实践上，很多团队以此作为挡箭牌，只听到了先跑通再优化，没意识到 API 设计的挑战大。

硬件支持情况

![](img/5f2c665876fa5397f49dd10cb8858572.png)

API 非常简单易用。

![](img/184bb82d9d25e9f721ce494b9844c2c7.png)

more like a traditional compiler。

![](img/569bb98e611d73509823d6cadbc99f2f.png)

3 种模式

![](img/ba100d9476216b83052fd8ec61a099e3.png)

distributed 也支持，performance 情况

![](img/88e4264aa843f5626245697c3aa27a23.png)

## Roadmap

compile 支持 DDP 和 FSDP。

没提到 DTensor support 情况。

![](img/0947daebffeb4c1bae6b01b2fbbea9a1.png)

# TorchDynamo

https://www.youtube.com/watch?v=JUwAuZ9Y8ek

PyTorch 2.0: TorchDynamo

Peng Wu

Dec 13, 2022

## the inflection point

compiler stack 最终的 1 件事情，是什么？

-- graph capture。

![](img/c3f7dd8e0a62053b50a9e0140ade9307.png)

花了 5 年尝试 various compiler solutions。

the inflection point: Torch Dynamo.

![](img/d1a6127061c151f75e1ad61165ff3e10.png)

## Dynamo Introduction

API example

![](img/9a1fde130ace7cae5e9284da6e44bccb.png)

前 2 点最关键：

1.  partial graph capture。遇到不支持的，就保留并拆分出前后的子图，分别编译。

1.  guard。解决了 trace 的经典难题 -- control flow 等导致 capture 不能用。guard 会自动报警，不会出错。

![](img/ca5c9df388c998437c48628486d2b7ce.png)

AOTAutoGrad, capture the backword part of the graph.

![](img/c3cdf0c3d598a140133f1cdc378eef84.png)

## 展望未来

![](img/69fda70141f7acf498f6c5d458be6dff.png)

# TorchInductor

https://www.youtube.com/watch?v=ppWKVg-VxmQ

PyTorch 2.0: TorchInductor

Jason Ansel, Meta AI

Dec 13, 2022

1.  a new PyTorch-native compiler.

1.  core -- loop level IR.

1.  contains only ~50 operators

1.  implemented in Python, making it easily hackable and extensible.

1.  pythonic define-by-run.

1.  Triton on GPUs and C++/OpenMP on CPUs.

## Design Principle

3 key principle。

没什么新东西，就是前面 PyTorch compiler 的通用 key principle。

![](img/4f5202bead26f0567853eb2283503c06.png)

TorchInductor 的 3 key technologies：

1.  核心 IR 是 loop level IR，是 python callable。做 codegen 或者 analysis，只需要直接 execute。

1.  用 SymPy（一个符号计算库）支持动态 shape。

1.  在 CPU 上用的 OpenMP，跟 intel 一起搞的。GPU 选了 OpenAI Triton。

个人看法：设计的确实聪明，都站在了最新的巨人的肩膀上。开发简单，效果出众。

![](img/6bc2bea043e6fae731a59a9f8d88639a.png)

## Compiler Stack

overview of the compiler stack

1.  AOT 和 PrimTorch 会 decompose 到 ~250 ops.

1.  PrimTorch，~250 primitive operators。compilers can reclaim performance via fusion。

1.  AOTAutograd. Gener**ating ah**ead-of-time backward traces. 然后独立 compile。

1.  字节 ByteIR 也是这么做的，还强调了是创新，看来，业界知识没及时更新。

1.  AOTAutograd 和 AOT 不一样，后者是 export 整图用的。注意区分。

1.  graph lowerings。把 ~250 ops lower to inductor's loop-level IR. 大约只包含 ~50 ops。

1.  scheduling。主要做 fusion, memory planning, tiling 等。

1.  codegen. 2 种：triton 和 OpenMP。

1.  wrapper codegen. stitch the calls to different kernels and replace the interpreter.

![](img/4a41b385fb98cb11cb599cb9d981d30f.png)

核心数据结构 (IR) - Loop-Level IR -- TODO 还没看懂。

![](img/4a96ee1c4ba799fc7a30c6e1299b163a.png)

backend1 - Triton for GPU

![](img/ac7137f746ab1ffe12dfa94521883d65.png)

backend2 - OpenMP for CPU

![](img/7fcfa555bc81428b0ada6a2968e6bada.png)

## TorchInductor Example

from 作者另一个 talk 的截图

https://www.youtube.com/watch?v=p13HpZv2S3Q

input code

![](img/23258ebcea8056ed8b022067f52c3715.png)

ATen FX graph

![](img/5b37ee99c310d028ece4f04b19de2fde.png)

define-by-run IR

this sort of loop level operator set only has 54 operators

![](img/f201b2b543650fbd00bbfacd7e52f4e6.png)

scheduling / fusion

主要是 2 个 function: can_fuse, socre_fusion

![](img/7212cd0554e65b75b83a715590c3bdb9.png)

codegen -> triton code

![](img/9fb4a575f8fcdf138e49c5296dcd6902.png)

![](img/6cafb24a998c0f8eef1fafcdbca8b7c6.png)

## Perf Result - 训练

GMEAN（几何平均值）

![](img/58b6c12b14a93901c25bfa038c502ed5.png)

## Performance 2

from 作者另一个 talk 的截图

https://www.youtube.com/watch?v=p13HpZv2S3Q

![](img/d094cf2beefd15d4993f0b3bfebf8eaf.png)

TorchInductor 加速来源分析：

1.  越往下的，加速作用越大，关闭后 performance dropdown 更明显。

1.  在 45 个 hugging face model 上做的实验。

1.  line 3: matmul template，在个别 op 上加速效果明显，但，45 个模型上，overall 的效果可以忽略。first principle 的 codegen from IR 已经很厉害了。

1.  fusion 和 inlining 最重要。

1.  如果全关了，甚至比 eager mode 还要慢。

1.  分析：前面的 op decomposition 引入了 performance dropdown。通过 fusion 可以把 op 合并回原来的 performance。

![](img/a9b1660245b30d51abe30ad2d9e0e673.png)

# AOTInductor

https://www.youtube.com/watch?v=w7d4oWzwZ0c

AOTInductor: Ahead-of-Time Compilation for PT2 Exported Models

Bin Bao, Meta

Oct 25, 2023

AOTInductor = torch.export (whole graph capture) + Inductor (AOT compilation)

整图导出，脱离 PyTorch 和 Python 做 deployment 用的。

实现上，复用 TorchInductor 的大部分 stack，主要 hack 了 backend 部分。

runtime 用了 AITemplate 的设计。

## Introduction - 概念引入

脱离 PyTorch 和 Python 做 deployment，是刚需。

![](img/1cbd9fdd623f94543ed0179b5c786675.png)

方案：torch.export (whole graph capture) + Inductor (AOT compilation)

-> AOTInductor

![](img/0f2ac379f5b536a2e0c453a29a55c8ad.png)

## Compiler Design

复用 TorchInductor 的大部分 stack，主要 hack 了 backend 部分。

![](img/e0ae57a141cd9642bdfd9093c4c81d5b.png)

design principle，跟前面描述基本一样。reuse TorchInductor as much as possible.

![](img/a60b79fa93c5b6e958d8f1852be17534.png)

## Code Example

input Code 转成的 3 种 output code example。

+   triton kernel

+   python wrapper

+   cpp wrapper

都是 GPU code，基于 triton 的。

作者说，OpenMP 和 CPU 的原理也相似。

![](img/cde222ddc3d409d26b4e1bcad59b8256.png)

![](img/4415a77a82aad56437977fd451e630bb.png)

![](img/88d5e0a49164c6e7a8d25e203cd43e71.png)

以 GPU 的 codegen 为例

![](img/e794072bffc12c9b71c2904ee119b7f0.png)

## Runtime

用了 AITemplate 的 runtime 设计。

重点忘了，没太关心这部分。

好像，有个 feature 是可以 share weight。听起来很像 Nvidia Triton 了。

![](img/42e845b332699e1567d1db83c5d721ba.png)

shapes 的定义，很有意思。对 dynamic shape 的支持很好。

猜测，这或许（已经）是 pytorch 2.x dynamic shape 的基本 feature 了。

![](img/982c7d1affb8543ae6d4a8dbbc7a583c.png)

Pytorch 2.x 对 dynamic shape 的重视确实不一样了。测个 runtime performance，都要强调是 static shape。

![](img/09b07480eff6f4afaadbb88a9abdf478.png)

# torch.export

https://www.youtube.com/watch?v=kJiv1DPK8_U

PyTorch 2.x: What’s coming up next for export-path

Michael Suo

Dec 13, 2022

## Introduction

![](img/4b370afd12ab78ec6862956ce67cfacd.png)

(来自 https://www.youtube.com/watch?v=98enl3C3fps  Part 5)

![](img/0a22f7b2c942293566c4cbf9d025e600.png)

(来自 https://www.youtube.com/watch?v=98enl3C3fps  Part 5)

很多的 control flow，都是与 tensor shape 有关。可以结合 dynamic shape 体系，更好的解决。

![](img/4866c5fcecff79956415595e0d9fc33f.png)

Export IR 也是一个 IR。可以 load/save/transform.

![](img/dd1e51b8c47b76a019f17c10717c3d19.png)

![](img/262b88a6816daeb49ca08de49d048f0f.png)

## Under The Hood

相比于 trace，重点还是 dynamo 的 guards。

![](img/ac278aa4f22672882fd4246aa2091580.png)

![](img/2515d9ba5d8a3d25b3eb210ecea49ec9.png)

## Ops levels

1.  PyTorch 有 2198 个 op，且，历史原因，有些算子拿不到 shape 等 basic information。

1.  ATen 有 750 个 op，purely functional。

1.  PrimTorch. 250 Primitive opset。compilers can reclaim performance via fusion。

![](img/8a5a8f171fa4c34caa7de04422ba36b1.png)

![](img/1fed4d74badcd75e6fef8e50fd778328.png)

# Dynamic Shapes Support

https://www.youtube.com/watch?v=rn-kJQ-7JmQ

PyTorch 2.0: Dynamic Shapes Support

Horace He

Dec 13, 2022

2 个要点：

1.  shape 变成 symbolic expression。用的 SymPy（其他 talk 讲了）

1.  control flow 是 shape 信息的重要来源。给判断条件加 guards。

## Introduction

![](img/340fa05a6a58b3cd56abb25d4c112f5c.png)

要点: shape 变成 symbolic expression。

![](img/a0f199d3940c815c78c20a7ab6877c6e.png)

resnet18 的 IR

![](img/8854f67c55f63cbd175a06198482a83c.png)

## Control Flow

2 个现象

1.  control flow 是 shape 信息的重要来源。看判断条件。

1.  specialize and guard。 只要新 shape 满足 guard 条件，就不用重新 compile。

![](img/01e1fc87c6ff96d4830d14ba817b2f6e.png)

## Result & Performance

![](img/3a39749528f2e3c3a0d325023a5ef972.png)

![](img/337064fe53031ff91eec7cc1f0d837a8.png)

## shape reasoning

![](img/1c866e6db30746b998be6cfe56a97b7c.png)

# CUDAGraph in a Partial Graph World

https://www.youtube.com/watch?v=Lg8F4F_qZxk

CUDAGraph in a Partial Graph World

Elias Ellison, Meta

Oct 25, 2023

## Takeaway

![](img/40298473f1fc21a82fe06385899a3603.png)

## CUDAGraph Background

一句话：可精确 replay 的 GPU static graph。

![](img/677a51f11c85ed50e57e9d8938961e37.png)

效果：省了 kernel 间的 CPU-GPU 来回搬运。

![](img/7da5085b22901c62a1e975008b8db4b8.png)

功能限制（by CUDAGraph itself, 与 PyTorch 集成无关）

![](img/398e824c1c69adfbc6a096001029cbda.png)

## 老版本的问题

核心：cost 是 memory。

如果无限的 recompilation，则会无限占用 memory。（真是重新定义了：speedups, at a cost）

所以，默认不能打开这个选项。

![](img/759f02c959257f8caee9e62335989066.png)

老版本的原理：

2 个独立子图，memory 不共享。

导致，额外的 memory penalty，degrade perf。

![](img/a55d895f71599b6e22f4cc549f89823d.png)

## Pytorch 2.1 新方案

所有 graph capture 共享一个 memory pool。

解决内存管理的先天残疾。

![](img/e5a7e94237c39ecf574384e4e12dd029.png)

支持 control flow 等。基本上是借鉴了 torch 2.x 验证过的新理念、新设计。

![](img/95ca0f350903ffd20ab44ad0e7510c03.png)

## Limitations

与 eager memory pool 依旧不共享。

可能遇到不支持的 CudaGraph，会跳过。

![](img/b3d86912671805b8e83b0dd6629e4a47.png)

## Use Case & Results

支持 dynamic shape

![](img/8b3124579f713250537cae67d38b8491.png)

性能提升

![](img/0cc8177217cb55212ed86eec6c7881af.png)

# PyTorch 2.0 Status

https://www.youtube.com/watch?v=98enl3C3fps  Part 1

PyTorch 2.1 Technical Deep Dive

Mario Lezcano, Mark Saroufim, Mergen Nachin, Joe Spisak, Peng Wu, Will Constable, Yanan Cao

Oct 25, 2023

## 推广情况

接了 OpenXLA 和 ONNX。

![](img/f0a0a355de08d30dc108af444bc5a1c7.png)

## Tech Summary

3 个主要加速来源，reduce graph breaks 效果最大。-- 所以，通信算子入图，可能很重要。

![](img/9be315444dec0fbdaade6ce7601ee43b.png)

部署的改进，3 点。

![](img/b86738c682f923576a3dfefddccbc349.png)

## 后续计划

![](img/fb8d9644f5a946f6cd106e4f3ea993a9.png)

![](img/c172c800a293f57179b7ce585fb7e940.png)

# Distributed: PT2-D

https://www.youtube.com/watch?v=98enl3C3fps  Part 3

PyTorch 2.1 Technical Deep Dive

Mario Lezcano, Mark Saroufim, Mergen Nachin, Joe Spisak, Peng Wu, Will Constable, Yanan Cao

Oct 25, 2023

## why PT-D hard

1.  Dynamo 没法捕获的部分，用了 graph-breaks。

1.  大部分情况，works well。但留下了不少的优化空间。

比如，右边上下 2 图，下图没有 graph-break，可以更好的 compute & io overlap。

![](img/1639f49138a6e6bc70910b0414b66a5a.png)

## Design Goal

1.  不改变 eager & compile 的 UX

1.  通过提高 Dynamo 的 coverage 解决 graph capture 问题。

![](img/a5f86042b3fac363579e2596d7b6086d.png)

具体要支持的 case

![](img/13c0c2e333bdc6968be916179429bb95.png)

可以做的 optimizations

![](img/31d46406acba1f8ff3b7946d39ecaea7.png)

![](img/26698936c2da5ed5b6b82db63eb6ce7b.png)

## Goal Summary

1.  API 不变

1.  整图 capture

![](img/460b87bafa1b7759519dac819b6eb642.png)

topic 到此结束。-- 东西还没做完，这是画饼。

# Pytorch Inference

https://www.youtube.com/watch?v=98enl3C3fps  Part 4

PyTorch 2.1 Technical Deep Dive

Mario Lezcano, Mark Saroufim, Mergen Nachin, Joe Spisak, Peng Wu, Will Constable, Yanan Cao

Oct 25, 2023

## Python 部署没问题

自己写 c++ 来部署 model，要成为一个传说故事了。

![](img/6cf2e240c9e3cc2f6e76ab84f1331afc.png)

PyTorch 的快慢，与 python 没关系。

![](img/4fec454a9b9c7a562bf83f833f063982.png)

## microbenchmark

此 topic 的精髓。

一个关于 python 影响 performance 的 microbenchmark

![](img/5d498c9c70c0b6c739b83866d70be313.png)

![](img/69ec60640a4f5cadb01db96994162dbe.png)

这应该是 compiler 里面的 csr 还是 fusion？-- 作者叫 fusion。

![](img/3e33ff5cb2f423f0559c885168b27d67.png)

![](img/9ac27e312ba6603cf4d6f5d0f9b775c3.png)

总结：speedup 来自 fusion。

![](img/3b3a27e8f23d3db948dc5732ac933fb5.png)

## Remove Graph Breaks

2 个工具 + 1 个 coding 建议。

![](img/f3620e96664acf64293b95b59596a0c1.png)

朋友们都说好

![](img/5aea682b1fec4c19ab89af2af28651c5.png)

![](img/61e2cf925dea4d04d4731d7c0329658e.png)

## Inference Speedup Tips

Time measurements 如下图。

后 3 个没法控制，前 4 个可以优化。方法在后面的 slides

![](img/3690e8b9b98d4866eaa2d8a3ebebb95c.png)

1) compcompilation time 加速：用 cache。

![](img/3c05c9eb77c0c26dd4f18575d4d98241.png)

2) load Model 加速：使用 mmap 的 API。

![](img/5f8392c7afef25052c07a029ac6d15fd.png)

3) inference 加速：用 CUDAGraph。

[PyTorch 2.0: Compiler](https://dwpl6xgouw.feishu.cn/wiki/IWF4wwnfcilhHQkyLcLc91XWnpg?fromScene=spaceOverview#VnjKdvq2HotBynx4fm5czse9nDc)为额外的 memory cost，暂时还不是默认的配置。详见

model='reduce-overhead' or 'max-autotune'

![](img/3b7316e8f6a575384305ec40430ce2a6.png)

4) import torch 加速: 手写一个 lazy modules load。

![](img/9b38fa08fc9f0e5d434338c115073589.png)

还有一个重要加速来源 -- 等 Python 移除 GIL。

![](img/f5a031a2e78b02eb4a47d4b033e53ecf.png)