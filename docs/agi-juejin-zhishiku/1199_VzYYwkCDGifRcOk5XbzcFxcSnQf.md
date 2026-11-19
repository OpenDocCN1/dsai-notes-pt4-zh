
# LLM（十八）：LLM 的推理优化技术纵览

> 来源：[https://agijuejin.feishu.cn/docx/IgPOdYSNLoqBxCx3jPycA9n9nkd](https://agijuejin.feishu.cn/docx/IgPOdYSNLoqBxCx3jPycA9n9nkd)

> 作者：紫气东来

> 原文链接：[https://zhuanlan.zhihu.com/p/642412124](https://zhuanlan.zhihu.com/p/642412124)

推理是 `LLM` 应用的重要一环，在部署服务环节影响重大，本文将讨论主流的 `LLM` 的推理优化技术。

## 一、子图融合（subgraph fusion）

图融合技术即通过将多个 `OP`（算子）合并成一个 `OP`（算子），来减少 `Kernel` 的调用。因为每一个基本 `OP` 都会对应一次 `GPU kernel` 的调用，和多次显存读写，这些都会增加大量额外的开销。

### 1.1 `FasterTransformer` by `NVIDIA`

`FasterTransformer` (`FT`) 是一个用于实现基于 `Transformer` 的神经网络推理的加速引擎。`FT` 框架是用 `C++`/`CUDA` 编写的，依赖于高度优化的 `cuBLAS`、`cuBLASLt` 和 `cuSPARSELt` 库，与 `NVIDIA TensorRT` 等其他编译器相比，`FT` 的特点是它支持 以分布式方式推理 `Transformer` 大模型 。

图融合是 `FT` 的一个重要特征，将多层神经网络组合成一个单一的神经网络，将使用一个单一的内核进行计算。 这种技术减少了数据传输并增加了数学密度，从而加速了推理阶段的计算。 例如，`multi-head attention` 块中的所有操作都可以合并到一个内核中。

![](img/4fb73e8ed2a2e0e12341b53574762b66.png)

除此之外，`FT` 还对部分大模型分别支持：

+ `INT8` 低精度量化推理

+ `Ampere` 架构的 `GPU` 硬件部分支持稀疏化

+ `Hopper` 架构支持 `FP8` 推理

+ `Tensor` 并行

+ `Pipeline` 并行

### 1.2 `DeepSpeed Inference` by `Microsoft`

对于 `Transformer layer`，可分为以下4个主要部分：

1. `Input Layer-Norm plus Query, Key, and Value GeMMs and their bias adds.`

1. `Transform plus Attention.`

1. `Intermediate FF, Layer-Norm, Bias-add, Residual, and Gaussian Error Linear Unit (GELU).`

1. `Bias-add plus Residual.`

如图所示，每一部分可分别进行融合，与未融合相比，以上几个部分的加速比可分别达到 1.5x, 2.9x, 3x, 1.2x 。

![](img/792a349555a20cfc7023a20df99ac985.png)

除此之外，`DeepSpeed Inference` 的优化点还有以下几点：

+ 多 `GPU` 的并行优化

+ `INT8` 模型量化

+ 推理的 `pipeline` 方案

更多详细介绍及实践可参考笔者之前的文章：

[紫气东来：NLP（十二）：`DeepSpeed Inference` 在 `LLM` 推理上的优化探究](https://zhuanlan.zhihu.com/p/629085568?) 231 赞同 · 18 评论 文章

![](img/6fd330afa9b02b6d9625b5a9602f3dc2.png)

### 1.3 [`MLC LLM`](https://link.zhihu.com/?target=https%3A//github.com/mlc-ai/mlc-llm) by `TVM`

之前介绍的推理方案主要是基于 `GPU` 的优化，而 `MLC LLM` 提供了可应用于移动端（例如 iPhone）、消费级电脑端（例如 Mac）和 Web 浏览器的轻设备解决方案。

`MLC LLM` 的主要工作流基于 `Apache TVM Unity`，通过扩展 `TVM` 后端使模型编译更加透明和高效。其中以编译代码转换、融合、内存规划和库卸载（library offloading）为代表的可组合的 ML 编译优化是其中重要的优化特性。

![](img/fa413464cf457626e0f93c4ca4f73e64.png)

除此之外，`MLC LLM` 还具有以下特性：

+ `Dynamic shape`：避免了对最大输入长度进行额外填充的需要，并减少了计算量和内存使用量。

+ 量化：`MLC LLM` 利用低位量化来压缩模型权重，并利用 `TVM` 的 `loop-level TensorIR` 为不同的压缩编码方案快速定制代码生成。

+ 运行时（`Runtime`）：`TVM` 编译生成的库能够通过 `TVM runtime` 在设备的原生环境中运行，`TVM runtime` 支持 `CUDA`/`Vulkan`/`Metal` 等主流 `GPU` 驱动以及 `C`、`JavaScript` 等语言的绑定。

除了上述3种方案外，其他也支持图融合的方案还包括 `NVIDIA TensorRT` ， `Tencent TurboTransformers` 等。

## 二、模型压缩（Model Compression）

模型压缩的基本动机在于当前的模型是冗余的，可以在精度损失很小的情况下实现模型小型化，主要包括3类方法：稀疏(`Sparsity`)、量化(`Quantization`)、蒸馏(`Distillation`)。

### 2.1 稀疏(`Sparsity`)

实现稀疏(`Sparsity`)的一个重要方法是剪枝(`Pruning`)。剪枝是在保留模型容量的情况下，通过修剪不重要的模型权重或连接来减小模型大小。 它可能需要也可能不需要重新培训。 修剪可以是非结构化的或结构化的。

+ 非结构化剪枝允许删除任何权重或连接，因此它不保留原始网络架构。 非结构化剪枝通常不适用于现代硬件，并且不会带来实际的推理加速。

+ 结构化剪枝旨在维持某些元素为零的密集矩阵乘法形式。 他们可能需要遵循某些模式限制才能使用硬件内核支持的内容。 当前的主流方法关注结构化剪枝，以实现 `Transformer` 模型的高稀疏性。

关于剪枝稀疏的基本原理，可参考笔者之前的文章：

[紫气东来：NLP（八）：大语言模型的稀疏化技术](https://zhuanlan.zhihu.com/p/615399255) 56 赞同 · 7 评论 文章

![](img/090cce68b21ae78b5b81ed1cf388528d.png)

除了上文介绍的稀疏方法外，还有其他的稀疏化方法，包括但不限于：

+ [`SparseGPT`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2301.00774.pdf)：该方法的工作原理是将剪枝问题简化为大规模的稀疏回归实例。它基于新的近似稀疏回归求解器，用于解决分层压缩问题，其效率足以在几个小时内使用单个 `GPU` 在最大的 `GPT` 模型（175B 参数）上执行。同时，`SparseGPT` 准确率足够高，不需要任何微调，剪枝后所损耗的准确率也可以忽略不计。

+ [`LLM-Pruner`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.11627.pdf)：遵循经典的“重要性估计-剪枝-微调”的策略，能够在有限资源下完成大语言模型的压缩，结果表明即使剪枝 20％ 的参数，压缩后的模型保留了 93.6％ 的性能。

+ [`Wanda`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2306.11695.pdf): 该方法由两个简单但必不可少的组件构成——剪枝度量和剪枝粒度。剪枝度量用来评估权重的重要性，然后按照剪枝粒度进行裁剪。该方法在 65B 的模型上只需要 5.6 秒就可以完成剪枝，同时达到 `SparseGPT` 相近的效果。

以上主要实现了稀疏的方法，那么对于稀疏后的模型如何加速呢？`NVIDIA Ampere` 架构对与结构化稀疏做了专门的 稀疏加速单元 ，下图展示了结构化稀疏的物理表示：

![](img/5915226f1aca452e8a2a3679827d17b8.png)

[下图展示了稀疏单元 `GEMM` 计算与标准 `GEMM` 计算的区别](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2104.08378.pdf) （详细解释参见 https://arxiv.org/pdf/2104.08378.pdf ）

![](img/2da54f7c72f8b00a58471104ab43169c.png)

### 2.2 量化(`Quantization`)

常见量化有两种常见方法：

+ 训练后量化（`Post-Training Quantization`，`PTQ`）：模型首先经过训练以达到收敛，然后我们将其权重转换为较低的精度，而无需进行更多训练。 与训练相比，实施起来通常相当便宜。

+ 量化感知训练（`Quantization-Aware Training`，`QAT`）：在预训练或进一步微调期间应用量化。 `QAT` 能够获得更好的性能，但需要额外的计算资源和对代表性训练数据的访问。

实际上，由于 `GPU` 内核缺乏对某些类型的矩阵乘法（例如 `INT4 x FP16`）的支持，理论最优量化策略与硬件内核支持之间的差距，并非以下所有方法都能加速实际推理。

关于量化的基本原理和实现细节，可参考笔者之前的文章：

[ ](https://zhuanlan.zhihu.com/p/627436535)

许多关于 `Transformer` 模型量化的研究都有相同的观察结果：简单的低精度（例如 8 bit）训练后量化会导致性能显著下降，这主要是由于动态的 `activation` 和静态的 `weight` 量化策略无法保持一致。

![](img/0f08bf8be0d42ac2fded56aa443a27a7.png)

为了不损失精度而提高性能，可以考虑 `WeightOnly` 量化技术，即只把 `Weight` 量化成 `int8` 格式，以降低访存压力。到实际 `Kernel` 内部再 `Dequantize` 回 `fp16`，进行矩阵乘计算。这种方法在 `BS` 较小是比较有效(因为此时的瓶颈在`IO`)，`BS` 较大时(因为此时的瓶颈在计算)效果变差。

![](img/7d74ceae39c410d6beefe1321ebe8851.png)

[`WeightOnly` 量化的典型案例是 `AWQ`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2306.00978.pdf)：`Activation-aware Weight Quantization` ，即只对 `weight` 进行量化以实现压缩和加速的效果。

### 2.3 蒸馏(`Distillation`)

[知识蒸馏](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2006.05525)是一种构建更小、更便宜的模型（“`student` 模型”）的直接方法，通过从预先训练的昂贵模型中转移技能来加速推理（“`teacher` 模型”）融入 `student`。 除了与 `teacher` 匹配的输出空间以构建适当的学习目标之外，对于如何构建 `student` 架构没有太多限制。

![](img/e89626a904eb6228c92abc8e12cea9e2.png)

给定数据集，训练 `student` 模型通过蒸馏损失来模仿 `teacher` 的输出。 通常神经网络有一个`softmax`层； 例如，`LLM` 输出 `token` 的概率分布。 将 `softmax` 之前的 `logits` 层表示为 `\mathbf{z}_t` 和 `\mathbf{z}_s` , 分别表示 `teacher` 和 `student` 模型。 蒸馏损失最小化两个 `softmax` 输出之间的差异（温度 `T`）。 当标签 `y` 已知，可以将其与`student` 的 `logits` 之间计算交叉熵，最后将两个损失相加，如下：

```
\mathcal{L}_{\mathrm{KD}}=\mathcal{L}_{\text {distll }}\left(\operatorname{softmax}\left(\mathbf{z}_t, T\right), \operatorname{softmax}\left(\mathbf{z}_s, T\right)\right)+\lambda \mathcal{L}_{\mathrm{CE}}\left(\mathbf{y}, \mathbf{z}_s\right)
```

[在 `Transformer` 中](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1910.01108) 一个典型案例是 `DistilBERT` ，模型参数减少 40%，速度提升71%。在大模型时代，蒸馏可以与量化、剪枝或稀疏化技术相结合，其中 `teacher` 模型是原始的全精度密集模型，而 `student` 模型则经过量化、剪枝或修剪以具有更高的稀疏级别，以实现模型的小型化。

### 2.4 `MoE`

随着模型越来越大，训练和推理成本会平方级增长，为了平衡成本和规模，即希望在较小成本下实现超大模型的效果，通常会采用 `MOE`（`Mixture-of-Experts`）技术。

`MoE` 层即将大模型拆分成多个小模型(专家，`expert`)， 每轮迭代根据样本决定激活一部分专家用于计算，达到了节省计算资源的效果；并引入可训练并确保稀疏性的门( `gate` )机制，以保证计算能力的优化。

与密集模型不同，`MoE` 将模型的某一层扩展为多个具有相同结构的专家网络( `expert` )，并由门( `gate` )网络决定激活哪些 `expert` 用于计算，从而实现超大规模稀疏模型的训练。`MoE` 细节如下图所示，中间层具有 `n` 个 `expert` 的 `MoE` 结构，并引入 `Gating network` 和 `Top_k` 机制。计算过程如下公式：

```
\begin{gathered} MoE(x)=\sum_{i=1}^n\left(G(x)_i E_i(x)\right) \\ G(x)=\operatorname{TopK}\left(\operatorname{softmax}\left(W_g(x)+\epsilon\right)\right) \end{gathered}
```

上述第 1 个公式表示了包含 `n` 个专家的 `MoE` 层的计算过程。具体来讲，首先对样本 `x` 进行门控计算， `W` 表示权重矩阵；然后，由 `Softmax` 处理后获得样本 `x` 被分配到各个 `expert` 的权重；然后，只取前 `k` (通常取 1 或者 2）个最大权重；最终，整个 `MoE Layer` 的计算结果就是选中的 `k` 个专家网络输出的加权和。

![](img/beedbf73d73259ab6e5880687faa7d30.png)

典型的 `MoE` 的方案包括 `GShard`, `Switch-Transformer`, `GLaM`, `FastMoE`, `DeepSpeed-MoE` 等方案。

`FastMoE` 支持在同一个 `worker` 上运行多个 `experts`，从而减少模型研究者在探索更多 `experts` 数量时所需的硬件资源。当 `experts` 数量较多时，`FastMoE` 针对传统的两层 `MLP` 全连接网络（即 `Transformer` 中的 `FFN` 网络）使用了更精细的并行策略，从而使得 `Transformer` 模型中 `MLP` 部分的运算速度相比朴素的实现较大的加速。

![](img/3a1f0fc749de287ed80b66d93b404047.png)

## 三、并行化（Parallelism）

当前的推理的并行化技术主要体现在3个维度上，即 `3D Parallelism`:

+ `Data Parallelism`(`DP`)

+ `Tensor Parallelism`(`TP`)

+ `Pipeline Parallelism`(`PP`)

![](img/c07cbda24515f2e2fd48039be1378448.png)

### 3.1 数据并行 (`Data Parallelism`, `DP`)

在推理中，`DP` 主要是增加设备数来增加系统整体 `Throughput`，其中最经典的即 `DeepSpeed`的`Zero`系列

![](img/7e7f6692db7ad18a1d92bceb614e562e.png)

另外 `FSDP` 也比较高效和易用

![](img/ee70b3a7dc22a2cf2d79e552fce5682b.png)

### 3.2 张量并行 (`Tensor Parallelism`, `TP`)

在推理中，`TP` 主要是横向增加设备数通过并行计算来减少 `latency`，其实现原理及细节可参考笔者之前的文章

[ ](https://zhuanlan.zhihu.com/p/603908668)

当前也有一些方便易用的 `TP` 方案，如 `BlackSamorez/tensor_parallel` ，使用起来非常简单：



```
import transformers
import tensor_parallel as tp
tokenizer = transformers.AutoTokenizer.from_pretrained("facebook/opt-13b")
model = transformers.AutoModelForCausalLM.from_pretrained("facebook/opt-13b")  # use opt-125m for testing

model = tp.tensor_parallel(model, ["cuda:0", "cuda:1"])  # <- each GPU has half the weights

inputs = tokenizer("A cat sat", return_tensors="pt")["input_ids"].to("cuda:0")
outputs = model.generate(inputs, num_beams=5)
print(tokenizer.decode(outputs[0])) # A cat sat on my lap for a few minutes ...

model(input_ids=inputs, labels=inputs).loss.backward()  # training works as usual
```

当前主流的推理框架都支持 TP 的方式，包括但不限于：

+   [`Megatron-LM`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1909.08053.pdf)
+   FasterTransformer
+   DeepSpeed Inference
+   [`vLLM`](https://link.zhihu.com/?target=https%3A//github.com/vllm-project/vllm)
+   Text Generation Inference
+   [`ParallelFormers`](https://link.zhihu.com/?target=https%3A//github.com/tunib-ai/parallelformers)
+   [`ColossalAI`](https://link.zhihu.com/?target=https%3A//github.com/hpcaitech/ColossalAI)
+   [`FlexFlow`](https://link.zhihu.com/?target=https%3A//github.com/flexflow/FlexFlow)
+   [`LiBai`](https://link.zhihu.com/?target=https%3A//github.com/Oneflow-Inc/libai)
+   [`AlpaServe`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2302.11665.pdf)

### 3.3 流水线并行(Pipeline Parallelism, PP)

在推理中，PP 主要是纵向增加设备数通过并行计算来支持更大模型，同时提高设备利用率。

![](img/0e01f1ffaf80e3191083f32649119826.png)

通常来说，PP 需要与 TP 结合以支持更大模型，并实现最佳效果

![](img/a9f4beb0b112eda148c2cb0e9af27a4d.png)

## 四、Transformer 结构优化

该类方法主要通过优化 Transformer 的结构以实现推理性能的提升。

### 4.1 FlashAttention

该部分的实现细节可参考笔者之前的文章，在此不予赘述

[  ](https://zhuanlan.zhihu.com/p/638468472)

`FlashAttention-v2` 在原基础上做了改进，使其在算法、并行化和工作分区等方面都有了显著改进，对大模型的适用性也更强。在A100 上性能数据如下：

![](img/ac8ed28c4d1c78d01aef7a4d293eab89.png)

`FlashDecoding` 是在 `FlashAttention` 的基础上针对 inference 的优化主要分为三步：

1.  长文本下将`KV`分成更小且方便并行的`chunk`

1.  对每个`chunk`的`KV`，`Q`和他们进行之前一样的`FlashAttention`获取这个`chunk`的结果

1.  对每个`chunk`的结果进行`reduce`

![](img/9d6feb9971ac4d0955eca079bb536177.png)

### 4.2 PagedAttention

可参考

[  ](https://zhuanlan.zhihu.com/p/638468472)

### 4.3 [`FLAT Attention`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2107.06419.pdf)

`FLAT-Attention` 与 `FlashAttention` 采取不同的路线来解决同一问题。 提出的解决方案有所不同，但关键思想是相同的（`tiling` 和 `scheudling`）。下面主要讨论二者不同之处：

![](img/ba649ba5e3bc767d0957eda1ab8c2f88.png)

### 4.3.1 Tiling 策略比较

`FlashAttention` 使用块平铺和权重固定。 `FLAT-Attention` 使用行平铺（行粒度）和输出固定。

![](img/906b4062f140746c480269e7cddb40b3.png)

### 4.3.2 Scheduling 策略(数据流)比较

`FlashAttention` 的 `Scheduling` 过程

![](img/690a94a22053d6aef89aad67850f2d1f.png)

![](img/061cafcaa2f4e31cc1dcbed45b0112e3.png)

`FLAT-Attention` 的 `Scheduling` 过程

![](img/409e58b551567c511bcc5cf357621b6a.png)

![](img/7381aa24503b7aaec50c527d75825b87.png)

## 五、动态批处理（Dynamic Batch, Continuous batch ）

该类方法主要是针对多 `Batch` 的场景，通过对 `Batch` 的时序优化，以达到去除 `padding`、提高吞吐和设备利用率。传统的 `Batch` 处理方法是静态的，因为`Batch size` 的大小在推理完成之前保持不变。

如下图所示，使用静态 `Batch` 完成四个序列。 在第一次迭代（左）中，每个序列从`prompt`（黄色）生成一个`token`（蓝色）。 经过几次迭代（右）后，每个完成的序列都有不同的大小，因为每个序列在不同的迭代中发出其序列结束标记（红色）。 可见序列 3 在两次迭代后就已经结束，但仍然需要等待 `Batch` 中的最后一个序列完成生成（在本例中，序列 2 在六次迭代后）才能统一输出，这意味着 GPU 未被充分利用。

![](img/062a03335be86084b0e048839d24a9fe.png)

下面我们来研究 `Dynamic Batch` 是如何优化这一过程的。

### 5.1 [`ORCA`](https://link.zhihu.com/?target=https%3A//www.usenix.org/system/files/osdi22-yu.pdf)

Orca 不是等到 `Batch` 中的所有序列完成生成，而是实现 `iteration` 级调度，其中`Batch size`由每次迭代确定。 结果是，一旦 `Batch` 中的序列完成生成，就可以在其位置插入新序列，从而比静态 `Batch` 产生更高的 GPU 利用率。

![](img/742b65a22a0bc294580071420f5faf48.png)

下图展示了使用 `Dynamic Batch` 完成七个序列的过程。 左侧显示单次迭代后的批次，右侧显示多次迭代后的 `Batch` 。 一旦序列发出序列结束标记，就在其位置插入一个新序列（即序列 `S5`、`S6` 和 `S7`）。 这样可以实现更高的 GPU 利用率，因为 GPU 不会等待所有序列完成才开始新的序列。

![](img/e6c0dc841f4a4ff38238674adc4011a1.png)

结果显示在延时不变的情况下，其相对于FasterTransformer 可获得 36.9 倍的吞吐提升。

![](img/7f64883a1d9c34f1b39a4bbf0699d07a.png)

### 5.2 [`FastServe`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.05920.pdf)

`ORCA` 使用first-come-first-served (FCFS) 处理推理作业, 计划任务持续运行直至完成。 由于 GPU 内存容量有限以及推理对延时敏感，无法通过任意数量的传入函数来增加处理，由此可能会导致队列阻塞。

`FastServe` 使用 preemptive scheduling，通过新颖的跳跃连接 Multi-Level Feedback Queue 程序来最小化延时。 基于 LLM 推理的长度无法确定，调度程序利用输入长度信息来分配适当的初始值每个到达作业要加入的队列。 较高优先级队列跳过加入的队列以减少降级。 设计高效的GPU内存管理机制主动下载和上传 GPU 内存和主机内存之间的中间状态，以进行 LLM 推理。

![](img/f202ff44ebb205ae5614abe9c8a356ea.png)

实验表明，该方法比`ORCA`有明显的性能提升

![](img/8739efedce0e27ca0ba6ef1a246660f5.png)

### 5.3 [`vLLM`](https://link.zhihu.com/?target=https%3A//vllm.ai/)

`vLLM` 的核心是 `PagedAttention`，其灵感来自传统操作系统概念，例如分页和虚拟内存。 它们通过在固定大小的“页面”或块中分配内存，允许 `KV` 缓存变得不连续。 然后可以重写 `attention` 机制以对块对齐的输入进行操作，从而允许在非连续的内存范围上执行 `attention` 。

这意味着 `cache` 分配可以 just-in-time，而不是 ahead-of-time：当启动一个新的生成任务时，框架不需要分配大小为 `Maximum_context_length` 的连续 `cache`。 每次迭代，调度程序都可以决定特定生成任务是否需要更多空间，并动态分配，而不会降低 `PagedAttention` 的性能。 这并不能保证内存的完美利用（浪费现在限制在 4% 以下，仅在最后一个块中），但它明显改善了当今业界广泛使用的提前分配方案的浪费 。

总而言之，`PagedAttention` + `vLLM` 可节省大量内存，因为大多数序列不会消耗整个上下文窗口。 这些内存节省直接转化为更高的 `Batch` 大小，这意味着更高的吞吐量和更便宜的服务。

实验表明，该方法相比于静态 `Batch` 与其他动态 `Batch` 的方法吞吐性能提升明显。

![](img/292c86731892b2dcaec93cf79b685dd4.png)

### 5.4 Text Generation Inference

`TGI` 是 `HuggingFace` 开发的基于 `Rust`, `Python` 和 `gRPC` 的推理服务工具，其基本框架如下：

![](img/110a6782a55421a370fe20598121bfa0.png)

关于 `TGI` 的用法，可参考笔者的文章，同时对比了和 `vLLM` 和 `FasterTransformer` 的性能。

[  ](https://zhuanlan.zhihu.com/p/646772063)

### 5.5 [`LMDeploy`](https://link.zhihu.com/?target=https%3A//github.com/InternLM/lmdeploy)

`LMDeploy` 是由 `MMRazor` 和 `MMDeploy` 团队开发的用于压缩、部署 LLM 服务的工具包。 它具有以下核心特点：

+   `TurboMind`：基于`FasterTransformer` 的高效推理引擎。

+   交互推理：通过缓存多轮对话过程中的 `k/v`，记住对话历史，以避免对历史会话的重复处理。

+   多GPU模型部署和量化

+   `Dynamic Batch`

## 六、KV cache 优化

关于 `KV cache` 的典型优化方法及原理，可参考笔者的文章

[  ](https://zhuanlan.zhihu.com/p/659770503)

## 七、解码优化

### 7.1 推测解码 ( Speculative Decoding )

`LLM`推理主要是受内存带宽限制的（`memory-bandwidth bound`）即 `LLM` 每个解码步所用的推理时间大部分并不是用于模型的前向计算，而是消耗在了将`LLM`巨量的参数从GPU显存（High-Bandwidth Memory，`HBM`）迁移到高速缓存（`cache`）上（以进行运算操作）。这个问题随着`LLM`规模的增大愈发严重。

推测解码方法的关键是使用了一大一小两个模型(分别记作`Mp`和`Mq)`，推测解码方法的核心思想是：输入一个`prefix`，在用`LLM`(目标模型)做推理的同时，并行的让一个小得多的近似模型(approximation model)基于输入`prefix`以自回归的方式(autoregressively)来运行 `\gamma` 次，生成得到 `\gamma` 个`token`(被称作`guesses`或`draft tokens`)，然后并行的(parallel run)让`LLM`对这些推测出来的`token`做评估，通过随机阈值 `r_{i}` 与概率分布比 `p_{i}(x)/q_{i}(x)` 的对比，从中挑选出`n`个`token`(都是由近似模型生成)，随后让目标模型在`n`的基础上生成第`n+1`个`token`，最后把这`n+1`个`token`添加到原`prefix`之后作为新的`prefix`，然后重复整个过程直到满足停止条件(达到`max length`或生成了`<EOS>`等)。

![](img/667f963485f8ae387a69b51ef4a37375.png)

### 7.2 并行解码 —— Medusa

`Medusa` 主要思想是在正常的`LLM`的基础上，增加几个解码头，并且每个头预测的偏移量是不同的，比如原始的头预测第`i`个`token`，而新增的`medusa heads`分别为预测第`i+1`，`i+2`...个`token`。如上图，并且每个头可以指定`topk`个结果，这样可以将所有的`topk`组装成一个一个的候选结果，最后选择最优的结果。

![](img/9fb5c23706f94b21a96bf1846156e142.png)

计算每个头组装之后的候选的最优解，其实这时候完全可以每个候选都走一次模型，算出概率，但是很显然不可能这样做，因为本来方案是为了加速，作者设计了一种`tree attention`的机制，可以做到只走一次模型来达到目的，如示例所示，第一个`medusa heads`的 `top-2` 预测和第二个`medusa heads`的 `top-3` 预测产生 2*3=6 个候选。假设原始的`LLM`输出是`[0]`，第一个头是`[1,2]`，第二个头是`[3,4,5]`。期望直接能把`[0,1,2,3,4,5]`，输入模型就能得到一些概率的信息，但是不同的头对应的`token`的父节点是不同的，所以需要冗余一些`token`，方便添加`mask`变成`[0,1,2,3,4,5,3,4,5]`，对应到右上的`mask`矩阵，每个节点只有父节点以及当前节点是1，这样其实就能得到一个树的路径关系。

第一次用美杜莎头解码的时候，是看不到前面`i`个`token`的，而再次输入模型可以看到完整的上文，得到完整的概率之后，可以通过头计算得到树的路径信息，比如示例对应的路径`index`是`[0,1,3]` , `[0,1,4]`, `[0,1,5]`, `[0,2,6]`，然后基于后验概率得到最优的候选片段。

## 八、硬件升级

以上主要介绍了在算法和模型层面的优化方法，除此之外，升级硬件系统可以进一步提升整体性能，下面将介绍几种可用于(和潜在的)推理加速的硬件产品。

### 8.1 NVIDIA H100 PCIe

`NVIDIA H100` 核心架构与 `Ampere` 相似，数学运算部分布置在144组`CUDA`上，最高可拥有18432个`FP32`(单精度)、9216个`FP64`(双精度)`CUDA`核心，辅以576个第四代`Tensor`核心。`H100`核心采用台积电的`N4`工艺制造，内建800亿个晶体管，核心面积仅有814m㎡。其与`A100` 主要参数对比如下：

![](img/bab0c4c31a276d1273a058daeda8af96.png)

在性能方面，`H100` 较 `A100` 也有明显提升，其部分数据如下所示。

![](img/6ded6b1b75ecf702a2cf39ceefd2ad38.png)

### 8.2 AMD MI300

`AMD MI300` 处理器集成了24个`Zen 4`架构`CPU`核心，以及`CDNA 3`架构`GPU`核心，周围还有着8颗`HBM3`高速缓存，容量高达128GB，总计拥有1460亿个晶体管。与上一代 `MI250`相比，`MI300`进行`AI`运算的速度将提高至8倍，能效方面也将提升5倍。

目前未找到公开的在 `LLM` 方面的推理性能数据。

### 8.3 [`Apple M2 Ultra`](https://link.zhihu.com/?target=https%3A//www.apple.com/newsroom/2023/06/apple-introduces-m2-ultra/)



M2 Ultra 采用第二代 5 纳米工艺制造，并使用 Apple 突破性的 UltraFusion 技术连接两个 M2 Max 芯片的芯片，使性能提高一倍。 M2 Ultra 由 1340 亿个晶体管组成，比 M1 Ultra 多了 200 亿个。 其统一内存架构支持突破性的192GB内存容量，比M1 Ultra多出50%，并具有800GB/s的内存带宽，是M2 Max的两倍。 M2 Ultra 配备更强大的 CPU（比 M1 Ultra 快 20%）、更大的 GPU（快 30%）以及神经引擎（快 40%）。

目前未找到公开的在 LLM 方面的推理性能数据。

## 参考资料

[1] [Large Transformer Model Inference Optimization](https://link.zhihu.com/?target=https%3A//lilianweng.github.io/posts/2023-01-10-inference-optimization/)

[2] Efficient Inference on a Single GPU

[3] [How continuous batching enables 23x throughput in LLM inference while reducing p50 latency | Anyscale](https://link.zhihu.com/?target=https%3A//www.anyscale.com/blog/continuous-batching-llm-inference)

[4] [https://www.coreweave.com/blog/serving-inference-for-llms-nvidia-triton-inference-server-eleuther-ai](https://link.zhihu.com/?target=https%3A//www.coreweave.com/blog/serving-inference-for-llms-nvidia-triton-inference-server-eleuther-ai)

[5] [https://medium.com/mobius-labs/accelerating-large-language-models-strategies-for-enhancing-your-ai-inference-speed](https://link.zhihu.com/?target=https%3A//medium.com/mobius-labs/accelerating-large-language-models-strategies-for-enhancing-your-ai-inference-speed-a77dddc33bb2%23id_token%3DeyJhbGciOiJSUzI1NiIsImtpZCI6IjkzNDFkZWRlZWUyZDE4NjliNjU3ZmE5MzAzMDAwODJmZTI2YjNkOTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJuYmYiOjE2ODg4NzQzMzMsImF1ZCI6IjIxNjI5NjAzNTgzNC1rMWs2cWUwNjBzMnRwMmEyamFtNGxqZGNtczAwc3R0Zy5hcHBzLmdvb2dsZXVzZXJjb250ZW50LmNvbSIsInN1YiI6IjExNjAzMzg2MDcwOTE0MzI0NjA1OCIsImVtYWlsIjoiaWZyb21lYXN0QGdtYWlsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJhenAiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJuYW1lIjoiemlkb25nIHpoYW5nIiwicGljdHVyZSI6Imh0dHBzOi8vbGgzLmdvb2dsZXVzZXJjb250ZW50LmNvbS9hL0FBY0hUdGROVWZZT2FaNEJtQm1nQ2x2X3NxQjBFazlEcVRPSEpTTjhGbFpWb242Nz1zOTYtYyIsImdpdmVuX25hbWUiOiJ6aWRvbmciLCJmYW1pbHlfbmFtZSI6InpoYW5nIiwiaWF0IjoxNjg4ODc0NjMzLCJleHAiOjE2ODg4NzgyMzMsImp0aSI6IjE2ZGZhODZlODM5NjY1MzgzOWZjNDc3MGUwODFiNDdjZmQ4NGU4NzkifQ.SzEtUp26_6ZvGPS0UkxS1lkVl_w-f4Q9VcPBUdlxbgVmXBopMiHi1nBD61umoLMZ40RCna1bQmPtQzTIHQCeJ8cUSaubEnepuR3MilA37THqPeOCXm86O7VvI7O4HNaOe2tDENZMIw1n7ZuZ_P4bmbSG9WZQgb41Ka8kDTTi3MXCRxgo8JdMD9t1JGmzkUs5Ocq_Bvb2ojVltA86rab6QVWVsvUEzg7YmqZbHSaxK_WuFYTG3E09w5YVXenIUXcerwlVkqpdAcJWi2XSkLR8aI4inpxCYLuiPMTSI1zNG0Su_cqpXMd8Lfi-KZn5vndpV63mVjN-GbteNfth4KITdA)

[6] [LM-Pruner: On the Structural Pruning of Large Language Models](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.11627.pdf)

[7] [https://jonathan-hui.medium.com/ai-chips-a100-gpu-with-nvidia-ampere-architecture-3034ed685e6e](https://link.zhihu.com/?target=https%3A//jonathan-hui.medium.com/ai-chips-a100-gpu-with-nvidia-ampere-architecture-3034ed685e6e)

[8] `arxiv.org/pdf/2302.14017.pdf`

[9] [7 ways to speed up inference of your hosted LLMs. «In the future, every 1% speedup on LLM… | by Sergei Savvov | Jun, 2023 | Medium | Medium](https://link.zhihu.com/?target=https%3A//slgero.medium.com/speed-up-llm-inference-83653aa24c47)

[10] [https://arxiv.org/pdf/2104.08378.pdf](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2104.08378.pdf)

[11] [Introduction to Weight Quantization | Towards Data Science](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/introduction-to-weight-quantization-2494701b9c0c)

[12] [Accelerating Large Language Models via Low-Bit Quantization | NVIDIA On-Demand](https://link.zhihu.com/?target=https%3A//www.nvidia.com/en-us/on-demand/session/gtcspring23-s51226/)

[13] 认真读读WeightOnly - 知乎 (zhihu.com)

[14] [https://hackmd.io/@felixkao/HkZaooPD3](https://link.zhihu.com/?target=https%3A//hackmd.io/%40felixkao/HkZaooPD3)

[15] [SpecInfer: Accelerating Generative Large Language Model Serving with Speculative Inference and Token Tree Verification](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.09781.pdf)

[16] [Fast Inference from Transformers via Speculative Decoding](https://link.zhihu.com/?target=https%3A//proceedings.mlr.press/v202/leviathan23a/leviathan23a.pdf)

[17] [Accelerating large language model decoding with speculative sampling](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.01318)

> 钟鼎山林都是梦，人间宠辱休惊。 —— 辛弃疾 《临江仙》
