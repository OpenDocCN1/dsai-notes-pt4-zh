# PyTorch 2.0: Hardware Integration

> 来源：[https://learning.feishu.cn/docx/STDMddTOwoFoLzxl5rXcE4Fgnbg](https://learning.feishu.cn/docx/STDMddTOwoFoLzxl5rXcE4Fgnbg)

关于 硬件厂商 接入 PyTorch 的 9 个 conference talk。

总体感觉：

1.  Intel 很积极。AMD 划水。IBM 是真水。Nvidia 抱着 TensorRT 扭扭捏捏。

![](img/777e224d45f5981633c8021a65250a6d.png)

# Summary


# Intel CPU - OpenMP

https://www.youtube.com/watch?v=eN5fqBNrjOo

Accelerating Inference on CPU with Torch.Compile

Jiong Gong, Intel

Oct 25, 2023

Intel 和 Meta 一起做的 cpu 接入方案，基于 OpenMP。

个人理解，核心有 2 点：

1.  op fusion。主要针对 2 种 pattern。

1.  conv/matmul 融合 pointwise，

1.  reduction 融合 pointwise。

1.  vectorization (向量化) 做的很好。已覆盖 94% 的 kernel。

名词解释

1.  Sapphire Rapids: the 4th generation of Intel Xeon Scalable Processors

1.  Advanced Matrix Extensions (AMX) instruction set extension

SDPA: scale dot production attention fusion

![](img/bb7737af2a7127311c2b738c74610124.png)

![](img/ce8ceda82175e948e2b96cc29dc6677e.png)

Dynamo 转成 fx graph。

pattern match 发现 pointwise 计算，可以与前面的做 fusion。

![](img/478fe11656c8327812508844fab8f231.png)

提供了 2 stage optimization

1.  FX graph level。

1.  conv 和 matmul 自动融合 pointwise 的算子，mapping 到 oneDNN 中的融合算子。

1.  layout 转换。weight 变成 blocked layout。获取 better cache locality。

1.  然后，把剩下的 pointwise 和 reduction operator，用 cpp loop nest 来融合。

![](img/85a280c02b7ed5d10be73415ab6da245.png)

vectorization 是loop 优化的核心。

leverage the Aten abstraction to make sure 生成的 code 适用于不同的 CPU arch。

根据 3 个 benchmark suites (TorchBench, HuggingFace, timm)，94% 的 kernel 被向量化了。

左侧的 mask operator 用 Blendv 来 mask。

![](img/40b5c386a30db7837273090506533391.png)

https://zhuanlan.zhihu.com/p/437657452 intel AVX 系列指令基础介绍

![](img/28455e3b85b70198f324f434b5d99fb9.png)

量化的时候，undergo a torch.export quantization flow.

quat flow 在 compilation flow 之前插入 quant 算子。compilation flow 流程不变。

![](img/7af5fd280d3246941d2a6fec5860cf7c.png)

by default，inductor use python wrapper。

对 CPU 的负面作用影响更大，因为 host code and kernel code are executed sequentially on CPU without overlapping.

所以，把这些 python wrapper mapping 到 cpp counterpart to avoid these overhead.

![](img/062caf3296cc4334d05e1a3867714329.png)

cpu 侧可以自动融合了。

![](img/8d62d066013a4bd28651cd22558625a1.png)

ipex serves as a staging area for all the optimizations from intel which has not yet been upstreamed to inductor.

![](img/adfc66f9132eec35e15ab177d08d1611.png)

![](img/04ccae1f10b25579b67d2c07d0af142e.png)

# Intel OpenVINO - Compiler Backend

https://www.youtube.com/watch?v=jOIieKe3MQ8

Accelerating PyTorch Performance with OpenVINO

Yamini Nimmagadda, Devang Aggarwal & Mustafa Cavus, Intel

Oct 25, 2023

感觉比较水。

跟前面的 Intel triton GPU 和 Intel OpenMP CPU 比起来，没啥硬货。

![](img/71a59b3dc3414474b214dbe153dba8b7.png)

![](img/b091e670e9156a705624b05a61f2a5ce.png)

![](img/27c40bae4ef31cf30936509ac1f9b3c5.png)

![](img/3025f4ad60b164b332ddbaf4c8fc064e.png)

![](img/57dff12ecf409f03aa619df99fc2b0ef.png)

# NVIDIA - Torch-TensorRT

https://www.youtube.com/watch?v=eGDMJ3MY4zk

Accelerated Inference in PyTorch 2.X with Torch-TensorRT

George Stefanakis & Dheeraj Peri, NVIDIA

![](img/923e870c252652a44f375b54d5898e90.png)

## Overview

![](img/66c9a82cb8f3b7357ceca4edc7995e00.png)

![](img/80aef5384a5906a021a5b50cacde2bc4.png)

## JIT Approach

![](img/02efedc7d243a4a708d01087cf376ff4.png)

![](img/78c0aa19cb153da65065dc496fd84472.png)

![](img/839511fe414c2637e5259efc958ae390.png)

## AOT Approach

![](img/27930cab66eb3f7287ddc56cbc69ac7f.png)

![](img/a4ec5c094d5d9ba0e1fd26757acfed2b.png)

![](img/6acc9e9093fb693836554790dc41b68f.png)

![](img/d724038132ac6a2453a651fc07adb9ca.png)

![](img/5d1b1cef6378a2031a3cba3087b818bd.png)

# AMD ROCm

https://www.youtube.com/watch?v=lN-LrBqpeaA

干货不多。

![](img/bafc4e86d126818793066fccbd3218f0.png)

![](img/f1a13865c834a2976bdcee2a883955b0.png)

![](img/1a196ba376c8c415e780db210a5bdb8c.png)

![](img/f08b7000df937dd87ceee603b2168047.png)

![](img/7b2396bd2932105c6dd9342a04fdccf6.png)

![](img/5d140867e937dff7390386b7db51f970.png)

![](img/23aa5dde74f45f3b27a75993a2d3fa3f.png)

# AMD GPUs

https://www.youtube.com/watch?v=mPrfh7MNV_0

Jeff Daily, AMD

Dec 9, 2022

主要都在介绍 AMD GPUs 的 software stack。

没看到与 PyTorch 有多大的关联性。

![](img/93180786ab735ce27140c651e08bbdce.png)

![](img/4a00209f95a3448eec36e0838871a8c3.png)

![](img/192bec8f90e2d50d59064412255c605d.png)

![](img/e1738edb43892779d7f6aeb9b1979e3d.png)

![](img/9155781f8b0007cfe977b2551fc5e8d6.png)

![](img/36972a0e20c6c0a2dea9fb48f65b5a60.png)

![](img/0bcc1ca1d7cccec6157098f7778e9976.png)

![](img/13f88e272a9ca437138014b1e39ee5df.png)

# IBM's Watsonx - Lessons Learned

https://www.youtube.com/watch?v=iXSfUw7VfNw

Lessons from Using Pytorch 2.0 Compile in IBM's Watsonx.AI Inference

Antoni Viros i Martin, IBM Research

Oct 25, 2023

学生大作业水平。

![](img/ff6801d67eb6eca9d93f19f4417cf7c6.png)

![](img/3fdeec428ce1617f3b4af44cbf9b7171.png)

![](img/7aef3dbe6575003669856912e2ba2be4.png)

# Edge - Vendor Integration

https://www.youtube.com/watch?v=Zq-CW9Pey9Y

PyTorch Edge: Vendor Integration Journey for Compilers and Backends

Kimish Patel, & Chen Lai, Meta Platforms

Oct 25, 2023

integrate backend and compiler toolchain natively with PyTorch Edge IR without compromising on performance

Explore our well-defined entry points and API for integrating compiler passes, delegates, and custom kernel implementations, all without any intermediate conversions.

![](img/d30ebfc2c66960f6bfb965912b127793.png)

## Entry Points

![](img/5d5c6f84ca20b31801b359ec3c41ca0c.png)

![](img/e8c3cc1d224bda7ff3f22fa583546f8c.png)

## Example

![](img/6858701c22b87ffa74d3a19007ea7228.png)

![](img/309ddedf2f6551e9e2c85193ddaee9c3.png)

![](img/ff9168eacb9904b8efb6ea4553240ffc.png)

先用 partition API 拿出 hardware 子图。

![](img/fb3cb786d4de71439fa3d6b78bd7f57a.png)

再用 preprocess API 对 hardware 子图做优化。实际就是 compile 阶段。

此处的 preprocess 语义，是相对于 runtime 的。不是 compile 的。

![](img/29134cee83dc3d43b3907f4983f58542.png)

也在大量使用 dialect 了。

![](img/b01d58234e328382f8267ba11392abc4.png)

## Runtime

runtime，先 init

![](img/d87059399064f2d8b3d3ad8181b39808.png)

![](img/c23113d010d4ccad12c4018f02b1537f.png)

![](img/147f9e981f1229f6a9b989ff4bdffa57.png)

![](img/6be5c016e6f7570844093d56d858985e.png)

## Debuggability

![](img/f97453504f16fe115c9fc09d9829525c.png)

![](img/9dd601521e7951480f471aaacc938031.png)

![](img/8edb009b99274156b0afe788f3b5e88d.png)

## Partners 光荣榜

![](img/299e8d24ae00d60b7c247ad82ee9f2e7.png)

# Edge Solution - ExecuTorch

https://www.youtube.com/watch?v=98enl3C3fps  Part 6

PyTorch 2.1 Technical Deep Dive

Mario Lezcano, Mark Saroufim, Mergen Nachin, Joe Spisak, Peng Wu, Will Constable, Yanan Cao

Oct 25, 2023

这部分还非常早期，非常不成熟。

一年内不搞端侧的话，感觉可以跳过不看。

![](img/b4800559a76631ad911e31f4261730ef.png)

## Opportunities and Challenges

![](img/9f3bd2cfea5c9ea606970a676670386f.png)

![](img/6cf259e26ab569c6733c8c3efb0afe99.png)

![](img/3cbfb075a1d6b16bca57a1d38fbf6de3.png)

![](img/0d313340ce298f0d5d572dc94ef12789.png)

![](img/a038bec9d1628e56d953a8ba380f7410.png)

![](img/f725b1e65bf3a58ddad527c1eeca16bb.png)