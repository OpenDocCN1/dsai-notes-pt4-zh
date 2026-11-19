
# TVM 的 Unity & Relax

> 来源：[https://learning.feishu.cn/docx/TctYdSiZAoeZ3Sxhcn9cGea8nqc](https://learning.feishu.cn/docx/TctYdSiZAoeZ3Sxhcn9cGea8nqc)

## 参考 link

| 文章 | 作者 | Notes |
| --- | --- | --- |
| Relax: TVM 的下一代图层级 IR | 金雨辰 | 介绍了 3 个设计目标，3 个设计原则。 |
| 新一代深度学习编译技术变革和展望 | 陈天奇 | TVMCon 2021 |
| Relax: Co-designing High Level Abstraction Towards TVM Unity - TVMCon 2021 | Yuchen Jin, Altan Haan, Ziheng Jiang | 21min 视频。 |
|  |  |  |
| Relax Architecture Overview | TVM team |  |
| Relax Shape Computation Design |  |  |

1. 引入了 2 个 intrinsics：`call_tir` 和 `call_packed`.
2. 陈天奇的 MLC 课程 chap 4 讲了这两个 API

## 要点总结

1. `Relax` 是 `Relay` next，TVM 的下一代图层 IR。

![](img/dcd15060dbb3917e1e36c69c7c4b9919.png)

## Motivations - 3 个

![](img/6d0f4db8a7be0cabe35f90b5ae88f25e.png)

![](img/4ec8009aaea45411c6189fbb02ada122.png)

![](img/c2e6804c3d2a2f159f03100e713a7af1.png)

![](img/8088a12b3dd5318559c313565db44a5d.png)

## Design & Architecture

![](img/5a7ca2ef582fae7ce3294e3f898a2b66.png)

![](img/279baca8c5c234b8340bdc6c63ae9e2e.png)

![](img/9a5e17dfaa9271fa4bd157db7e3f4343.png)

![](img/97788a2818e9577d77ec06e03a7c6d46.png)

![](img/7cdfd9bf891e7d37092ab296b939ac02.png)

![](img/91b10815810bbe5af0a0021c436ee323.png)
