
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
