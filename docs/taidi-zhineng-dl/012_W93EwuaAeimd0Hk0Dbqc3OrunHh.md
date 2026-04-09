# PyTorch Parallelism - talk notes

> 来源：[https://learning.feishu.cn/docx/T90fdiLPDoA6Rsxo4C2cz3eTnTc](https://learning.feishu.cn/docx/T90fdiLPDoA6Rsxo4C2cz3eTnTc)

published: https://zhuanlan.zhihu.com/p/677808809

6 个来自 Pytorch 团队的 talk，基本能搞懂 PyTorch 分布式。

大部分 talk，时长 10 min 左右，简洁有深度。非常推荐。

notes:

2022 和 2023 的 pytorch conference，2-D parallelism 都是 Wanchao Liang 讲的。

Notes: PyTorch 的 3 种主要 Parallelism 如下:

![](img/a6dc538f8781bb69bb89798abe6e5bdf.png)

# PiPPy: Automated Pipeline Parallelism

https://www.youtube.com/watch?v=_rC49PeqrWc

PiPPy: Automated Pipeline Parallelism for PyTorch

Ke Wen

PyTorch Conference 2022

![](img/74aaeebd8c0298fd3a84340bd985cdd6.png)

![](img/4447ae22b9bae40a298ed0bc3670722b.png)

![](img/5d9c58a069832bdb751e74302850598d.png)

![](img/37ade1b0e8c4362cf238cab209685dbb.png)

![](img/579408d1f1f148d61e4a9f4ebdf049ce.png)

![](img/dd2a8a04f655b170d5f47908f89680be.png)

![](img/6bc28010331f0acb3e323fa541e0d9e4.png)

# FSDP - Fully Sharded Data Parallel

https://www.youtube.com/watch?v=By_O0k102PY

How Fully Sharded Data Parallel (FSDP) works?

Aug 28, 2023

slides: https://bit.ly/45sE4mz

explains how Distributed Data Parallel (DDP) and Fully Sharded Data Parallel (FSDP) works.

links:

+   [P](https://arxiv.org/pdf/2304.11277.pdf)yTorch FSDP: Experiences on Scaling Fully Sharded Data Parallel

+   [Z](https://arxiv.org/abs/1910.02054)eRO: Memory Optimizations Toward Training Trillion Parameter Models

+   Multi-GPU Training Strategies

+   Nvidia NCCL Operations

+   PyTorch FSDP Tutorials

+   [F](https://engineering.fb.com/2021/07/15/open-source/fsdp/)ully Sharded Data Parallel: faster AI training with fewer GPUs

## What is in the GPU memory

![](img/ee885c9e7fe37e7d8ae413c976095ef3.png)

![](img/de55703e5db77a3e04f366dd4709f5ca.png)

![](img/046e4facc8abbd7b66309e91d2276320.png)

## DDP

![](img/7b8096abca7b9551742f98630024ddf4.png)

![](img/b87aa834c2568084b2962387b2d00a7c.png)

![](img/e0cc4bc404e5a39425f5fadc37f23378.png)

## FSDP

FSDP 不是什么？

1.  Model Parallelism (已经不推荐使用了，用 DP + Pipeline Parallelism 代替)

1.  Tensor Parallelism

1.  Pipeline Parallelism

![](img/576e958af8063333b4c402bf63fd8e24.png)

![](img/9ddfdecfd95868d6b2cf5710c950fefb.png)

![](img/f463054b953bd11c7d5295a87b33a96e.png)

![](img/407f0baab5d453b5756710bd64e171c1.png)

![](img/bbadaf86bdd9a9dc5d17aaf53cc894f0.png)

![](img/a27b641dd049afc1adaa017a36d15a67.png)

![](img/f869562d394dcf5afec39aeebe81ba9d.png)

![](img/7b0e3de0308cbf98e242839a558a5b22.png)

![](img/b39184e812ed58274089179f9d73eaea.png)

![](img/75ef78cee800785289f2a58904ba6a71.png)

## Pros & Cons

![](img/bbcc6c971bd3db8cd0f3fb54c768b879.png)

![](img/81ce4390d13b1cf835f4e9c0576cb625.png)

![](img/6a6ee13d0ada368b0226c8d02ab19dfa.png)

![](img/2297c9703396114080cd25f6bc41d14b.png)

![](img/2db11d5601f771a0f195a8ede14572a3.png)

# 2D Parallelism - 2022

https://www.youtube.com/watch?v=MEx2kJPmjHo

Two Dimensional Parallelism Using Distributed Tensors

PyTorch Conference 2022

Wanchao Liang

2D 并行，就是 DP(across hosts) + TP(Tensor Parallelism, within each host)

## Motivation

![](img/4011be0fccafbdc5dd7c2c70f3a63dae.png)

## DTensor

DistributedTensor 又叫 DTensor

有 2 个重要概念：

1.  DeviceMesh

1.  DTensor placement tags: shard, replicate, partial

![](img/06a717e0d513d6c32c002b239df3abc0.png)

![](img/c37d1431d38e88eaa5c7c1a743cff788.png)

## TP and User API

![](img/c7fcee27414809d93db554fa3aedbcfa.png)

2D 并行，就是 DP(across hosts) + TP(within each host)

![](img/8dbfc5a28e8945f88a9886b9aa172fd6.png)

![](img/72315942d777a19d0e80c099e798b250.png)

# Composable Distributed PT2(D) - 2023

https://www.youtube.com/watch?v=LcDjYLJblEY

Composable Distributed PT2(D)

Wanchao Liang, Meta Platforms, Inc.

## Introduction

![](img/72cd782bf87824944f328f8f8283edf6.png)

## DeviceMesh

![](img/7af156b828500068f213edafd0012611.png)

![](img/a0139010cdef5487450fb335c2c37b7d.png)

## DTensor

![](img/878816e0f8a07317b7d94efa81c7320a.png)

## 2-D Parallel API

good, and try it.

![](img/1d1e4c8cf2933d4991afc9d2fd3a214d.png)

![](img/63a9504af64bb65a0f8155452579cab5.png)

## Distributed Checkpoint

![](img/6ccedf2814065f216d09f8e877d626b0.png)

## Compile the PT-D

![](img/b558791f26a0c066ddea2e3b763a15af.png)

# Tensor and 2D Parallelism

https://www.youtube.com/watch?v=UFbpTLOx_A0

Rodrigo Kumpera & Junjie Wang, Meta

Oct 25, 2023

## TP updates

![](img/fb4035f2c9cd1bf1332310a7fdc42ace.png)

## Sequence Parallel

![](img/8c763e8397619e43c8d767bcdb97ebb9.png)

注意：

1.  后面是 reduce_scatter，不是 all-reduce。直接可以到后面做并行的 layernorm。

## Improvements

observed that there's some CPU overhead which is critical for small models and it can not be well hidden.

did more overlapping between the collective and the computation.

![](img/b9cf0a48539b58b9c0abfccb61577db1.png)

background: scale to large thousands of ranks, crash 问题就变得很突出了。

small scale 时，几秒钟甚至几分钟的 crash，对 training 场景没啥影响。

![](img/8e226ed2d54dd8636c71d6abda28c3f5.png)

![](img/75173346c459835cdfb50aedadc254cc.png)

# PyTorch Distributed: Towards Large Scale Training

https://www.youtube.com/watch?v=bGo-2xNvNAc

PyTorch Distributed: Towards Large Scale Training

Anjali Sridhar

Dec 13, 2022

![](img/0cbf28caca20a346e7e440a0325a14b8.png)

![](img/ed699c01a5633f128742717c2a763801.png)

![](img/0dd903f197aaa290e4f2a74d3d2f276b.png)

![](img/36165da7484aaafdfc3c84030ee61bca.png)

![](img/7df94194bc96d51bff0cec6b78c36b6f.png)

![](img/242df75829fcf02dfb0150396e714f5b.png)

![](img/b2a1843b802f63e9673744814ffb23e7.png)

![](img/4294a1914b571de2da8968e800f61fed.png)

# PyTorch Distributed

https://www.youtube.com/watch?v=3XUG7cjte2U

PyTorch Distributed (DDP, RPC)

Shen Li

Mar 25, 2021

Meta Research 的一次团队间分享会议。

时间比较长，技术之外的信息量，也比较大。比如，团队关系等。

## Overview

提问和讨论 MPI 的一些细节。

talker 对 MPI 的理解看似比较少，可能是没用好 MPI 而造了 TensorPipe 的轮子。

![](img/4822faa43f2351d37f40ea973add8b66.png)

## DDP

提问展开讨论一些数据测量方法和经验值，没太细听。对 training 的经验值不太有概念。

why choose option 1? -- backword can overlap more with comm.

![](img/935554d784a7cf70afa122db02529151.png)

只测试了单机 8 卡的情况。换成多机以后，sweat point 很可能完全不一样了。

![](img/2dac738669b75a02ac4101b081d4b1fb.png)

## RPC

![](img/5599b51910d1aedcd039612421c4dc99.png)

![](img/b1edf9f244b08c481dbb825694672037.png)

![](img/9ea6f276b1e528a97fcb51863a3e99b6.png)

![](img/1f1ca910abd44f77bfa14bcd522a874a.png)

![](img/19a4ab91a83bfa37cb9834d6cee102dd.png)

## Hybrid Parallel

![](img/66f9a068b07a571891bada6ab6a86dab.png)

注：PS is Parameter Server

策略：

1.  把 embedding table 放在单独的 PS 上。可以跑在 CPU，甚至是多机 CPU 上。

1.  DDP 通过 RPC 访问 PS 上的数据。

2-3 directions:

1.  google, 微软，single program, multiple device. 每个 program 持有 a shard of the model. 不需要 RPC，只需要 collective communication

1.  AWS: PS。try to prove that，PS can be as faster as collective communication.

1.  (验证靠谱了吗？) Facebook: 预先计算好所需的数据，并不需要 RPC 获取 remote Embedding。

![](img/e9d796de3f561484a105ba821b123503.png)

![](img/6c491f96c7e98a55272e3fb868b74e62.png)

![](img/ad0e09c0a2d8da03e66fce843311b81f.png)

# torch 通信原语

https://pytorch.org/docs/stable/distributed.html

![](img/c7389c721873a0f4301ed0735e919aa2.png)