
# Chip Arch & uArch

> 来源：[https://learning.feishu.cn/docx/Ro4GdO0YMoW7IjxsEhccxfXKn3b](https://learning.feishu.cn/docx/Ro4GdO0YMoW7IjxsEhccxfXKn3b)

![](img/b2e1674cceb08f1f2419615ab1ea0661.png)

1.  `Architecture = ISA + uArch`。

1.  `ISA` 定义了一个有限状态机。是 software 与 hardware 的分界线。

1.  `ISA` 的上面是 `ABI`，一种 calling convention。约定了 `register` 的用途等，比如，`stack` / `frame pointer`。

1.  `ISA` 下面是微架构。可以 `sequential`，`dataflow`，`out-of-order execution`, `VLIW` 等。

# x86 & RISCV

`X86` 从 1978 年的 80 条指令，增长到 2015 年的 3600 条。

`RISC-V` 基础部分（`RV32I`）只有 47 条指令，并已冻结。

# 商业工程

1.  `Computer Architecture` 的另一个名字是 `Trade-off`。看着整个体系结构的发展历程中种种新技术的使用，无不体现了 `Trade-off` 的设计思想。

1.  市场是没有耐心的，完美设计却延迟交付，导致夭折的项目，好几个。

训练 `LLM` 最常需要的是 `H100s` with `3.2Tb/s` `InfiniBand`。

对于训练 `LLM`，大家更关注内存带宽、`FLOPS`、缓存和缓存延迟、`FP8` 计算等附加功能、计算性能（与 `cuda` 核心数量相关）、互连速度（例如 `InfiniBand`）。`H100` 比 `A100` 更受青睐，部分原因是缓存延迟较低和 `FP8` 计算等。

`H100` 比 `A100` 快多少？`16-bit` 推理速度提高约 3.5 倍，`16-bit` 训练速度提高约 2.3 倍。

# 新架构 Overview

+   冯诺伊曼架构

+   近存计算：`HBM`

+   存内计算：`Memristor`

| 架构 | paper |

| --- | --- |

| 光电计算 | https://doi.org/10.1038/s41586-023-06558-8 (2023). |

|  |  |

# 存内计算

![](img/66b97f5372bbb1ed496e9fa2af976d0f.png)

其他公司

https://mp.weixin.qq.com/s/Sbl9VcaZp7zy4WSl1razIg 迈特芯黄瀚韬：自研立方脉动架构，已开发三款 `X-Edge` 芯片并实现大模型高效能并行存算

![](img/6d862cecd0434fe50e9e728d56e85a15.png)

# Learning Notes

## 2023.12.31

调休 2 天 + 元旦 3 天假，入门 2 个未曾深入的 topic：chip 间互联，忆阻器 `memristor`。

https://www.youtube.com/watch?v=P_57FZSnrO4

讲体系结构的 `Interconnects`。唯一记住的就是 `Crossbar`，简单粗暴到荒唐（贵）的思路还能被写到书上，那必然是不一般。

https://www.bilibili.com/video/BV1k44y1474h/

6 分钟的忆阻器科普。思路：给 `crossbar` 的任意导线之间加个类似二极管 `PN` 节的开关。

忆阻器能做到 nm 级，导电靠氧化物的正价空穴和负价阳离子。原理跟 `PN` 节基本一样。

https://www.youtube.com/watch?v=9U-5jshTuwg

清华 高斌。介绍忆阻器与类脑计算，讲的很好。

最后，又想起两位老爷子的论断：A New Golden Age for Computer Architecture

More notes：

1.  英伟达 2 代架构之间，都做不到计算精度严格对齐，忆阻器为什么要追求与 `GPU` 严格精度对齐呢，没必要。理论上，应该没有 `finetune` 解决不了的精度问题。

1.  为了解决忆阻器模拟计算的精度问题，搞个误差模型，再 `finetune` 一下原始 `weight` 适配忆阻器的误差特点。用魔法打败魔法。

1.  `Crossbar` 果然不一般。但，我们的课程和日常工作中，好像都不太强调这个概念的意义。

## 2024.03.04

刷 Onur Mutlu 的课，入门了微架构，`SIMD` 和 `GPGPU` 的原理。

`arch = ISA + uArch`。醍醐灌顶。

## 2024.03.08

深入理解 `Nvidia` `GPU` `arch` 的演进历史。

感觉，大部分架构没啥深入学习的价值。
