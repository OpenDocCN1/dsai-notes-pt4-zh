
# TVM meetup Highlights - 2023.12

> 来源：[https://learning.feishu.cn/docx/LaOGdIvibopwL9x0C0ec1Ko4nif](https://learning.feishu.cn/docx/LaOGdIvibopwL9x0C0ec1Ko4nif)

## Insights from the talks

`TVM Unity` 的 `frontend`

1.  不再支持 `Tensorflow` 了。大模型时代，没人用了。

1.  `PyTorch`，直接或用 `Dynamo` 转成 `Torch FX`。

1.  进入图编译 (`Relax`) 之前，针对各个 `frontend` 先做了一次 `shape deduction`

一次 `lowering` 搞不定的，就 `loop` 反复 `lowering` 多次。

以前 `lowering` 前后的 `IR` 目标清晰，相互解耦。现在，撤回设计，只要啥都能存，就能支持反复 `loop` `lowering`。

-- 我们的设计就是没有设计。大道无形。

`TVM Unity` 把 `KVCache` 和 `distributed` 放在了 `runtime` 里面来支持。

这感觉历史包袱还是太重了，非常不面向未来的 `llm`。

`OctoML` 的 `project`

把 `KV Cache` 和 `IO` 作为 `Effect` 单独抽象。是个思路，但又感觉有点僵硬，就像没抽象一样。

`TVM` 的一贯做法 - `model` 和 `weight` 分离。

在大模型时代，可能确实是个好主意。省了很多事。

量化主要就是怼着 `weight` 搞。

字节 `ByteIR`，`torch` 的 `demo`，思路跟我们的接近。

他们验证过了，还能做 `training`。

先抄完再说，有问题再优化。即使错了，也是字节 `AML` 水平的错误。

`Forward` 和 `backward` 的图分离，获得额外的加速空间。

高性能算子，用了 `AI Template`。看起来比 `torch compiler` 原生的快一些。

`Compiler` 直接以 `backend` 接入 `PyTorch compiler`，效果不错。

`ByteIR` 的 `compile`，`inference` & `training` demos，`API` 设计的感觉不错

`compile`

`inference`

训练 + `inference` 的 `demo`

![](img/e616f73a211a83c7506a27bbc12e5fe9.png)

## Talk 1 - `TVM Unity`

### 编译挑战: Steep Lowering Curve

![](img/0f15932c8a6fddf2b37253700633b1e3.png)

### TVM Unity 方案: Partial Lowering

### Compilation workflow

## Talk 2 - `OctoML` 的一些 projects

### Torchy

`KVCache`

`IOEffect`

### AutoLLM: 统一的量化框架

其实就是个胶水代码，所以才能统一各种算法。

价值：整理的各种量化算法的通用 `pattern` 和 `workflow`。

可能是 1 个人不到 3 个月的工程工作。

## Talk 3 - 美团 `TVM` 的实践

一个老生常谈的话题了，讲了很多年了。没变过。

## Talk 4 - 字节 `ByteIR`

### `ByteIR` 架构

### `PyTorch` 的例子

### `FW` / `BW` 的 `graph` 分离

效果很好，特别是针对 `backward` 图，优化空间变多了。

### 接 `AI-template` 算子

### `Linalg` 上的 `tiling` 和 `fusion` 优化

### `Pytorch Example`

编译

`inference`

训练 + `inference` 的 `demo`

![](img/d9abe8fb6d42002d1405e3324237c0d6.png)
