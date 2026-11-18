
# PyTorch Conf talks backup

> 来源：[https://learning.feishu.cn/docx/PWOQdLh2Rok1QlxXCrocKY7unRg](https://learning.feishu.cn/docx/PWOQdLh2Rok1QlxXCrocKY7unRg)

# Python & Torch Compiler

https://www.youtube.com/watch?v=98enl3C3fps Part 2

PyTorch 2.1 Technical Deep Dive

Mario Lezcano, Mark Saroufim, Mergen Nachin, Joe Spisak, Peng Wu, Will Constable, Yanan Cao

Oct 25, 2023

## natively understand NumPy

natively 支持 numpy code 编译，带来新的加速空间。

![](img/ae1b8e3c1075127d2b22e5134689807f.png)

![](img/6dfcd5a7868284ab5ba4b203250e3db6.png)

![](img/65da741a26e87dae3aaa8679fa6e351e.png)

![](img/5b47c22960fc8153ffaf885acfbf3044.png)

## SciPy & scikit-learn

![](img/39ffd0281667823b42dfb9e79407f038.png)

# Distributed Training with XLA SPMD and Dynamo

https://www.youtube.com/watch?v=tWH2MAHzVVc

Large-Scale Distributed Training with Dynamo and PyTorch/XLA SPMD

PyTorch / XLA Dynamo & Triton

Yeounoh Chung & Jiewen Tan, google

cover `Torch.Dynamo` in relation with PyTorch/XLA distributed API.

we discuss the new PyTorch/XLA SPMD API for automatic parallelization and our latest LLaMA2 training results. PyTorch/XLA SPMD makes it simple for PyTorch developers to distribute their ML workloads (e.g., training & inference with Dynamo) with easy-to-use API, and uses XLA GSPMD, high-performance automatic parallelization system. Under the hood, it transforms the user single-device program into a partitioned one. We will share how we enabled advanced 2D sharding strategies for LLaMA2 using PyTorch/XLA SPMD.

XLA 感觉要起死回生了，解决 PyTorch 里面的分布式需求。

没太听懂，但感觉基本就是我们想要的通信算子入图的思路。

![](img/6799a48440cffdd4304b9b3b90d70483.png)

![](img/0e2995b2621d2eca6caa8864e5ff769d.png)

![](img/72de73582d38d14a6fda75261d587b22.png)

![](img/5d06006d0e744dae67a2209ff76164ea.png)

![](img/59654b7c2a5fe1575ec71bba500f82dd.png)

![](img/d772827ebb110f7416aa86dfab6b29a7.png)

![](img/a946c5deee2074133a05d396d90db9fa.png)

![](img/d6cd20945e2be1b680195d9b1a50e395.png)

![](img/6f369cbba48219998fcfa39051169b34.png)

![](img/e4216be734f6b6c50beccbabf5ebe7e4.png)

# Exploration of CUTLASS & OpenAI Triton

https://www.youtube.com/watch?v=yCyZEJrlrfY

Harnessing NVIDIA Tensor Cores: An Exploration of CUTLASS & OpenAI Triton

Matthew Nicely US, NVIDIA

Oct 25, 2023

## CUTLASS

![](img/28385ed35e194a64fdb1564b0571aaa7.png)

![](img/838732f536fa7bcbf31eb99d48803ae4.png)

### TTM: Time To Market

![](img/44490e16c7fe46679048d20ec0da8570.png)

![](img/cef19de6191ebfb81fe780cf85c03ebd.png)

![](img/114fd3bb9fd80ae0197f4e732b80cf3a.png)

![](img/44e682ba2c3de0f07e694e3efc1f6585.png)

![](img/1a9d2cc33f0d7c29bd480c74f2d871b2.png)

![](img/0962524a0dbd297ef9375d63650399f3.png)

## OpenAI Triton

已经支持了 Hopper 架构

![](img/7df5566d77f9656670e81432fe7e2b7f.png)

control knob: 控制旋钮：一种旋转设备，用于由人类操作员握住并旋转，以提供手动输入调整到机械/电气系统中，使不同程度的旋钮旋转对应不同的所需输入。

![](img/cb4f08935cbbf51ebe1d442e12dd5523.png)

# 八卦 - Pytorch Conf 2023

![](img/3d5dbe795b41300f9a3cad486a86c027.png)

## PyTorch foundation

2022.10 建立的 foundation

不做技术 governance

但做 business governance。即，办 conference，做培训和证书，搞社区。

![](img/3b199938d1ed6ff4aa1a59f639bd85d6.png)

![](img/34390c8efd47c0d5412f0192327b1717.png)

![](img/9c068b045ae9ab82019749b362df46448.png)

## PyTorch History | Major Moments

![](img/715851c3b047a321cb125046f53f1eff.png)

![](img/3b825888e922003a21ffad241f98473e.png)

### PyTorch 2.0 is all about compiler

![](img/bfd156a9cfbd7a63c38b0b48930ce50e.png)

![](img/7bb83f7a6f20cefe4220c5c800d453f3.png)

![](img/a83721ba554c1e782c073c3dfc4ed41c.png)

## PyTorch everywhere

![](img/45ef8cc09788906858dac87baec3928a.png)

![](img/ccf113c3421596c849b680a9aedbe522.png)
