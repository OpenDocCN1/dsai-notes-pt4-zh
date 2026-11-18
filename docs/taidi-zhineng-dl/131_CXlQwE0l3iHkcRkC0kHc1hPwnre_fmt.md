
# TVM v.s. MLIR

> 来源：[https://learning.feishu.cn/docx/DqYFdMNG1oS0nxxUgtJcO5SZnlh](https://learning.feishu.cn/docx/DqYFdMNG1oS0nxxUgtJcO5SZnlh)

## Overview

|  | TVM | MLIR | 点评 |

| --- | --- | --- | --- |

| 社区 | 有统一的社区 &amp; leader | 没有统一的社区，想要贡献 `ai-compiler` 的代码，都缺少社区。 | MLIR 缺个 dl compiler 的社区领头羊。 |

| 基因 | 不是传统的 compiler 开发者。 | 老牌 compiler 开发者做的 meta compiler。 | MLIR 的代码更干净。 |

| 代码量 | 一共 50 万行代码， | 30 万 `c++` | MLIR 想要达到 `tvm` 的成熟度，即使前端只支持 `onnx` 这一个框架，拍脑袋，还差 30 万行代码。 |

`TVM` 缺少 global scope 的 low-level 优化机制。

`TVM` `Relay` 和 `TIR` 有明显的分界线。

`Relay` 是 high-level IR，最小粒度是算子，无法进行 `split` / `tensorize` 等 low-level 优化。`tiling` 应该也做不了。

`TIR` 是 low-level IR，但只能以算子为单位 (local scope) 描述 `compute` 和 `schedule`。无法做 global 优化。

![](img/763a8c2693bb83c7dea6a6031282e4a2.png)

很多优化策略，都需要跨多个算子的 analysis 和 `schedule` 能力：

1.  TP 和 PP 等模型切分策略的选择。即，`input` 和 `weight` 的切分策略。

2.  SRAM / shared memory 的分配和复用机制。避免算子间的 `input`/`output` 都要经过 DDR。

3.  overlap `compute` and IO。单算子内，优化空间有限，比如 `all-reduce`，`matmul` 等算子。

基于 `TVM` 实现上述优化，需要把整个图融合成一个算子。基本没有可行性。

`TVM` 也在尝试解决，比如，`Relax` / `Unity` 等新设计，但，个人并不看好这些设计。

`TVM` 预定义了一些代码变换规则（`tile`/`split`/`compute_at`/...），可以减少 `CodeGen` 实现的工作量，但同时也限制了编译器在代码变换上的灵活性。比如，`conv input` 等非等分 `split`，就很难支持。在 dbnet / deeplabV3 等 CV 模型上，这个 feature 很重要。

`MLIR` 解决上述问题的显著特性是：progressive lowering 的多层 IR 设计。

`MLIR` 中的每一层 IR 都是全局的，可以支持在 high-level 和 low-level 上进行图程序级的全局优化。

## 分裂的 MLIR 社区

| 项目 | Notes |

| --- | --- |

| `ONNX-MLIR` | `https://github.com/onnx/onnx-mlir` |

| 字节 `ByteIR` | `https://github.com/bytedance/byteir` |

| 阿里 | `https://github.com/alibaba/BladeDISC` |

| 算能 `TPU-MLIR` | `https://github.com/sophgo/tpu-mlir` |

字节的 `MLIR` paper

![](img/55f139a9fe327c4cafa5b11c82b2ee9d.png)
