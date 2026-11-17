
# LLM（大语言模型）部署加速方法

> 来源：https://agijuejin.feishu.cn/docx/MyQXdet85opziBxlqU9chQ25nld

> 作者：归来仍是少年

> 原文：https://zhuanlan.zhihu.com/p/644001250

## 一、前言

现在火热的大模型，国内各家开源了很多种 `6b`、`7b` 这样的大模型，但就算 `6b` 和 `7b` 推理一条长的 `instruct` 指令也是很耗时的，`chatgpt` 的 API 调用也是极其缓慢且很容易崩，在网页版上大家没感觉出来推理缓慢的原因，也就是得益于流式输出，除了商业应用方向外满足业务调用响应需求，所以线上加速也是很有必要的。

## 二、vLLM

`vLLM` 用于大模型并行推理加速，其中核心改进是 `PagedAttention` 算法，在 `vLLM` 中，我们发现 LLM 服务的性能受到内存的瓶颈。在自回归解码过程中，LLM 的所有输入标记都会生成其 `key` 和 `value` 张量，并且这些张量保存在 GPU 内存中以生成下一个 `token`。这些缓存的 `key` 和 `value` 张量通常称为 KV 缓存。KV 缓存是：

+   占用大：`LLaMA-13B` 中的单个序列最多占用 `1.7GB`。
+   动态变化：其大小取决于序列长度，序列长度变化很大且不可预测。因此，有效管理 KV 缓存提出了重大挑战。我们发现现有系统由于碎片和过度预留而浪费了 `60%` - `80%` 的内存。

为了解决这个问题，`vllm` 引入了 `PagedAttention`，这是一种受操作系统中 虚拟内存 和分页的经典思想启发的注意力算法。与传统的注意力算法不同，`PagedAttention` 允许在不连续的内存空间中存储连续的 `key` 和 `value`。具体来说，`PagedAttention` 将每个序列的 KV 缓存划分为块，每个块包含固定数量 `token` 的 `key` 和 `value`。在注意力计算过程中，`PagedAttention` 内核有效地识别并获取这些块。

![](img/34f3360350c25e9c7fd8ce8dd36c43d8.png)

因为块在内存中不需要是连续的，所以我们可以像在操作系统的虚拟内存中一样以更灵活的方式管理 `key` 和 `value`：可以将块视为页面，将 `token` 视为字节，将序列视为进程。序列的连续 逻辑块 通过块表映射到非连续 物理块。当新代币生成时，物理块会按需分配。

在 `PagedAttention` 中，内存浪费仅发生在序列的最后一个块中。实际上，这会导致内存使用接近最佳，浪费率低于 `4%`。事实证明，内存效率的提高非常有益：它允许系统将更多序列一起批处理，提高 GPU 利用率，从而显著提高吞吐量，如上面的性能结果所示。

`PagedAttention` 还有另一个关键优势：高效的内存共享。例如，在并行采样中，从同一提示生成多个输出序列。在这种情况下，提示的计算和内存可以在输出序列之间共享。

![](img/abd75c59224237982500cbc15f0bfa53.png)

`PagedAttention` 自然可以通过其块表实现内存共享。与进程共享物理页的方式类似，`PagedAttention` 中的不同序列可以通过将其逻辑块映射到同一物理块来共享块。为了确保安全共享，`PagedAttention` 跟踪物理块的 引用计数 并实现 `Copy-on-Write` 机制。

![](img/4dc5c6fa6c53886aedc79f3f2580b348.png)

`PageAttention` 的内存共享极大地降低了复杂采样算法的内存开销，例如并行采样和波束搜索，将其内存占用降低高达 `55%`。这可以将吞吐量提高高达 `2.2` 倍。

`PagedAttention` 是 `vLLM` 背后的核心技术，`vLLM` 是 LLM 推理和服务引擎，支持各种具有高性能和易于使用的界面的模型。

从 `vllm` 的源码中我们可以看出来，`vllm` 是怎么样对于 `huggingface models` 上的模型进行推理优化的。

![](img/d3ddb20b22b271901b1e9d6f7ee0064e.png)

通过 `vllm` 实现的一些算子，替换原模型中对应的算子之后，也能使用 `vllm` 进行加载，目前 `vllm` 对于模型适配有限，尤其是国内的开源大模型支持不是很好，需要自己动手利用算子去改模型，其实难度也不大，有空我可能也去试试适配下国内开源的 `baichuan`。

![](img/87ef0526f97b782e60525565b21b1820.png)

![](img/c603c43170713e6ca8995491e8c25481.png)

## 三、Faster Transformer

`Fast Transformer` 是老黄的 `nv` 公司开发针对于 `Transformer` 结构进行加速优化。

`NVIDIA FasterTransformer` (`FT`) 是一个库，用于实现基于 `Transformer` 的神经网络推理的加速引擎，特别强调大型模型，以分布式方式跨越许多 GPU 和节点。

`FasterTransformer` 包含 `Transformer` 块的高度优化版本的实现，其中包含编码器和 解码器 部分。

使用此模块，您可以运行完整的编码器-解码器架构（如 `T5`）以及仅编码器模型（如 `BERT`）或仅解码器模型（如 `GPT`）的推理。它是用 `C++/CUDA` 编写的，依赖于高度优化的 `cuBLAS`、`cuBLASLt` 和 `cuSPARSELt` 库。这使您可以在 GPU 上构建最快的 `Transformer` 推理流程。

![](img/ab04e360dc408e8fad499e3c6b886e13.png)

`Faster Transformer` 支持完整的编码器-解码器架构（如 `T5`）以及仅编码器模型（如 `BERT`）或仅解码器模型（如 `GPT`）的推理。它是用 `C++/CUDA` 编写的，依赖于高度优化的 `cuBLAS`、`cuBLASLt` 和 `cuSPARSELt` 库。

![](img/c562eebca223763f7aba89dbc5942408.png)

跟之前的 `TensorRT` 的加速方法对比，`Faster Transformer` 可以利用多 `GPU` 加载 `Transformer` 不同的块，推理时更好的利用了 `GPU` 运算。

当每个张量被分成多个块时，就会发生张量 并行性，并且张量的每个块都可以放置在单独的 GPU 上。在计算过程中，每个块在不同的 GPU 上单独并行处理，并且可以通过组合来自多个 GPU 的结果来计算结果（最终张量）。

当模型被深度拆分并将不同的完整层放置到不同的 `GPU`/节点上时，就会发生流水线并行。

在底层，启用节点间/节点内通信依赖于 `MPI` 和 `NVIDIA NCCL`。使用此软件堆栈，您可以在多个 GPU 上以张量并行模式运行大型 `Transformer`，以减少计算延迟。

同时，`TP` 和 `PP` 可以结合在一起，在多 GPU 和多节点环境中运行具有数十亿和数万亿个参数（相当于 TB 级权重）的大型 `Transformer` 模型。

除了 `C` 中的 源代码，`FasterTransformer` 还提供 `TensorFlow` 集成（使用 `TensorFlow` 操作）、`PyTorch` 集成（使用 `PyTorch` 操作）和 `Triton` 集成作为后端。

目前，`TensorFlow op` 仅支持单 `GPU`，而 `PyTorch op` 和 `Triton` 后端都支持多 `GPU` 和多节点。

为了避免为模型并行性而拆分模型的额外工作，`FasterTransformer` 还提供了一个工具，用于将模型从不同格式拆分和转换为 `FasterTransformer` 二进制文件格式。然后 `FasterTransformer` 可以直接以二进制格式加载模型。

![](img/835a66fd612334c27e3e886a5ac0796a.png)

### 1. 推理缓存优化

因为自回归在推理的时候，像上面 `vllm` 的时候讲的会产生非常多的 `key` 和 `value` 的值，每次都需要重复计算，所以最好对这些产生的缓存分块存储，避免重复计算。

![](img/a4d1214491d08fdc82921f3dcedf84d5.png)

### 2. 内存优化

大模型往往带来极大参数量，就算量化到 `int4` 也是个不小的内存占用，`GPT-3 175b` 使用半精度存储也需要 `350 GB`，`Faster Transformer` 会缓存激活值和输出，在进行新 `sentence` 推理时可以重新利用缓存激活值和输出，避免多层反复计算和保存激活值和输出信息，例如 `GPT-3` 中的层数为 `96`，因此我们只需要 `1/96` 的内存量用于激活。

### 3. 使用 MPI 和 NCCL 通信优化

张量并行性：

`FasterTransformer` 遵循了 `Megatron` 的思想。对于自注意力块和前馈网络块，`FT` 按行拆分第一个矩阵的权重，并按列拆分第二个矩阵的权重。通过优化，`FT` 可以将每个 `Transformer` 块的归约操作减少到两倍。

流水线并行性：

`FasterTransformer` 将整批请求拆分为多个微批，隐藏了通信的泡沫。`FasterTransformer` 会针对不同情况自动调整微批量大小。

### 4. MatMul 内核自动调整（GEMM 自动调整）

矩阵乘法 是基于 `Transformer` 的神经网络中主要和最繁重的操作。`FT` 使用来自 `CuBLAS` 和 `CuTLASS` 库的功能来执行这些类型的操作。重要的是要知道 `MatMul` 操作可以在“硬件”级别使用不同的低级算法以数十种不同的方式执行。

`GemmBatchedEx` 函数实现 `MatMul` 操作，并以 `cublasGemmAlgo_t` 作为输入参数。使用此参数，您可以选择不同的底层算法进行操作。

`FasterTransformer` 库使用此参数对所有底层算法进行实时 基准测试，并为模型的参数和您的输入数据（注意层的大小、注意头的数量、隐藏层的大小）选择最佳的一个。此外，`FT` 对网络的某些部分使用硬件加速的底层函数，例如 `__expf`、`__shfl_xor_sync`。

### 5. 量化推理

`FT` 的内核支持使用 `fp16` 和 `int8` 中的低精度输入数据进行推理。由于较少的数据传输量和所需的内存，这两种机制都允许加速。同时，`int8` 和 `fp16` 计算可以在特殊硬件上执行，例如 `Tensor Core`（适用于从 `Volta` 开始的所有 `GPU` 架构），以及即将推出的 `Hopper` `GPU` 中的 `Transformer` 引擎。

## 四、LLM Accelerator（使用参考文本无损加速大语言模型推理）

`LLM Accelerator` 是微软亚研院提出来的大模型加速方法，在大模型典型的应用场景中可以取得两到三倍的加速。

![](img/20c44d256991fe80be19c5c08d46c640.png)

很多场景下，大模型生成的文字重复度是比较高的，这样重复生成会极其损耗资源和拖慢推理速度。

微软亚研院想到提高并行加速器利用率，加速大语言模型推理，进而提出了一种利用输出与参考文本的重复性来实现一步输出多个词的方法 `LLM Accelerator`。

![](img/4781152ebf762e2070a6c901f84f4cae.png)

在每一个 `token` 解码时，让模型先匹配已有的输出结果与参考文本，如果发现某个参考文本与已有的输出相符，那么模型很可能顺延已有的参考文本继续输出。因此，研究员们将参考文本的后续词也作为输入加入到模型中，从而使得一个解码步骤可以输出多个词。

在匹配标签文本时会进行 `token` 校验，去掉不正确的 `token`，保证生成的正确性，从而得到无损加速，一次输出多个词。

![](img/675f88ca069c0603f15ca77a685e56a2.png)

![](img/00096c0b1c1a076fbb617fb6df9424ae.png)

## 五、总结

大模型应用对于企业来说才是最关键的，推理速度决定着是否能够落地应用，现在也有不同的大厂提出新的推理方法来推动大模型的应用，目前的加速方法很多是对于批量推理加速明显，但是短的单条加速效果一般，但是单条推理是线上最常见的情况，希望也能针对这方面来优化下。

## 六、参考文献

1、https://github.com/vllm-project/vllm/tree/main

2、`vLLM`: Easy, Fast, and Cheap LLM Serving with `PagedAttention`

3、使用 `FasterTransformer` 和 `Triton` 推理服务器加速大型 `Transformer` 模型的推理

4、`LLM Accelerator`: 使用参考文本无损加速大语言模型推理 - MSRA
