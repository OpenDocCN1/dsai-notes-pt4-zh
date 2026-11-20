
## LLM 量化

随着领先LLM参数数量的快速增加以及获取GPU来运行模型的困难（成本和可用性方面），人们对量化LLM权重越来越感兴趣每个位数更少，通常可以产生相当的输出质量，同时所需内存减少 50-75%（或更多）。通常这不应该天真地完成； Tim Dettmers 是多种现代量化方法（`LLM.int8()`、`QLoRA`、`bitsandbytes`）的先驱之一，他发表了一篇很棒的博客文章，介绍了量化原理以及混合精度量化的必要性大型模型训练中的新兴特征。其他流行的方法和格式有 `GGUF`（对于 `llama.cpp`）、`AWQ`、`H🐧` 和 `GPTQ`；请参阅 TensorOps 的这篇文章来了解概述，以及 Maarten Grootendorst 的这篇文章来讨论它们的权衡。

In addition to enabling inference on smaller machines, quantization is also popular for parameter-efficient [training](https://www.youtube.com/watch?v=fQirE9N5q_Y) in [`QLoRA`](https://huggingface.co/blog/4bit-transformers-bitsandbytes), most weights are [`quantized`](https://www.answer.ai/posts/2024-03-14-fsdp-qlora-deep-dive.html) to 4-bit precision and frozen, while active LoRA adapters are trained in 16-bit precision. See this talk from Tim Dettmers, or this blog from Hugging Face for overviews. This blog post from Answer.AI also shows how to combine `QLoRA` with `FSDP` for efficient finetuning of 70B+ parameter models on consumer GPUs.

除了能够在较小的机器上进行推理之外，量化对于参数高效的训练也很流行；在 `QLoRA` 中，大多数权重被量化为 4 位精度并冻结，而主动 LoRA 适配器则以 16 位精度进行训练。请参阅 Tim Dettmers 的演讲或 Hugging Face 的博客了解概述。 Answer.AI 的这篇博文还展示了如何将 `QLoRA` 与 `FSDP` 结合起来，以便在消费类 GPU 上高效微调 70B+ 参数模型。

## Chapter 36: Speculative Decoding 第36章：推测解码

The basic idea behind speculative decoding is to speed up inference from a larger model by primarily sampling tokens from a much smaller model and occasionally applying corrections (e.g. every `N` tokens) from the larger model whenever the output distributions diverge. These batched consistency checks tend to be much faster than sampling `N` tokens directly, and so there can be large overall speedups if the token sequences from smaller models only diverge periodically.

推测性解码背后的基本思想是通过主要从较小模型中采样标记，并在输出分布发散时偶尔应用较大模型的校正（例如，每 `N` 个标记）来加速较大模型的推理。这些批量一致性检查往往比直接采样 `N` 个令牌要快得多，因此如果来自较小模型的令牌序列仅周期性发散，则可能会出现很大的整体加速。

See this [blog post](https://jaykmody.com/blog/speculative-sampling/) from Jay Mody for a walkthrough [of the original paper](https://pytorch.org/blog/hitchhikers-guide-speculative-decoding/), and this [PyTorch article](https://www.youtube.com/watch?v=hm7VEgxhOvk) for some evaluation results. There’s a nice video overview from Trelis Research as well.

请参阅 Jay Mody 的这篇博客文章了解原始论文的演练，以及这篇 PyTorch 文章了解一些评估结果。 Trelis Research 也提供了一个很好的视频概述。

## Chapter 37: FlashAttention 第37章：FlashAttention

Computing [attention matrices](https://genai-handbook.github.io/#s7) tends to be a primary bottleneck in inference and training for Transformers, and `FlashAttention` has become one of the most widely-used techniques for speeding it up. In contrast to some of the techniques we’ll see in Section 7 which approximate attention with a more concise representation (occurring some representation error as a result), `FlashAttention` is an exact representation whose speedup comes from hardware-aware implementation. It applies a few tricks — namely, `tiling` and `recomputation` — to decompose the expression of attention matrices, enabling significantly reduced memory I/O and faster wall-clock performance (even with slightly increasing the required FLOPS).

计算注意力矩阵往往是 Transformers 推理和训练的主要瓶颈，而 `FlashAttention` 已成为最广泛使用的加速技术之一。与我们将在第 7 节中看到的一些技术相比，这些技术用更简洁的表示来近似注意力（因此会出现一些表示错误），`FlashAttention` 是一种精确的表示，其加速来自于硬件感知的实现。它应用了一些技巧（即平铺和重新计算）来分解注意力矩阵的表达，从而显著减少内存 I/O 和更快的挂钟性能（即使稍微增加所需的 FLOPS）。

Resources: 资源：

+   [Talk](https://www.youtube.com/watch?v=gMOAud7hZg4) by Tri Dao (author of `FlashAttention`)

Tri Dao 的演讲（`FlashAttention` 的作者）

+   ELI5: `FlashAttention` by Aleksa Gordić

ELI5：Aleksa Gordić 的 `FlashAttention`

## Chapter 38: Key-Value Caching and Paged Attention 第 38 章：键值缓存和分页注意力

[As noted in the NVIDIA blog referenced above](https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/), key-value caching is fairly standard in Transformer implementation matrices to avoid redundant recomputation of attention. This enables a tradeoff between speed and resource utilization, as these matrices are kept in GPU VRAM. While managing this is fairly straightforward for a single “thread” of inference, a number of complexities arise when considering parallel inference or multiple users for a single hosted model instance. How can you avoid recomputing values for system prompts and few-shot examples? When should you evict cache elements for a user who may or may not want to continue a chat session? `PagedAttention` and its popular implementation `vLLM` addresses this by leveraging ideas from classical paging in operating systems, and has become a standard for self-hosted multi-user inference servers.

正如上面引用的 NVIDIA 博客中所指出的，键值缓存在 Transformer 实现矩阵中是相当标准的，以避免冗余的注意力重新计算。这可以在速度和资源利用率之间进行权衡，因为这些矩阵保存在 GPU VRAM 中。虽然管理这一点对于单个推理“线程”来说相当简单，但在考虑单个托管模型实例的并行推理或多个用户时，会出现许多复杂性。如何避免重新计算系统提示和少数示例的值？对于可能想要或不想继续聊天会话的用户，什么时候应该逐出缓存元素？ `PagedAttention` 及其流行的实现 `vLLM` 通过利用操作系统中经典分页的思想解决了这个问题，并已成为自托管多用户推理服务器的标准。

Resources: 资源：

+   [The KV Cache: Memory Usage in Transformers](https://www.youtube.com/watch?v=80bIUggRJf4) (video, Efficient NLP)

KV 缓存：Transformers 中的内存使用情况（视频、Efficient NLP）

+   [Fast LLM Serving with `vLLM` and `PagedAttention`](https://www.youtube.com/watch?v=5ZlavKF_98U) (video, Anyscale)

使用 `vLLM` 和 `PagedAttention` 快速 LLM 提供服务（视频、Anyscale）

+   `vLLM` [blog post](https://blog.vllm.ai/2023/06/20/vllm.html) `vLLM` 博客文章

## Chapter 39: CPU Offloading 第39章：CPU卸载

[The primary method](https://www.datacamp.com/tutorial/llama-cpp-tutorial) used for running LLMs either partially or entirely on CPU (vs. [`GPU`](https://justine.lol/matmul/)) is `llama.cpp`. See here for a high-level overview; `llama.cpp` serves as the backend for a number of popular self-hosted LLM tools/frameworks like LMStudio and Ollama. Here’s a blog post with some technical details about CPU performance improvements.

用于部分或全部在 CPU（相对于 GPU）上运行 LLMs 的主要方法是 `llama.cpp`。请参阅此处了解高级概述； `llama.cpp` 充当许多流行的自托管 LLM 工具/框架（例如 LMStudio 和 Ollama）的后端。这是一篇博客文章，其中包含有关 CPU 性能改进的一些技术细节。

# Section VII: Sub-Quadratic Context Scaling 第七节：第二次上下文缩放

> **Goal:** Survey approaches for avoiding the "quadratic scaling problem" faced by `self-attention` in Transformers.

> 目标：避免 Transformers 中自注意力所面临的“二次缩放问题”的调查方法。

>

> A major bottleneck in scaling both the size and context length of Transformers is the quadratic nature of attention, in which all pairs of token interactions are considered. Here we’ll look at a number of approaches for circumventing this, ranging from those which are currently widely used to those which are more exploratory (but promising) research directions.

> 缩放 Transformer 的大小和上下文长度的主要瓶颈是注意力的二次性质，其中考虑了所有标记交互对。在这里，我们将研究一些规避这一问题的方法，从目前广泛使用的方法到更具探索性（但有希望）的研究方向。

## Chapter 40: Sliding Window Attention 第40章：滑动窗口注意力

[Introduced](https://arxiv.org/abs/2004.05150) in the “`Longformer`” paper, `sliding window attention` acts as a sub-quadratic drop-in replacement for standard attention which allows attending only to a sliding window (shocking, right?) of recent tokens/states rather than the entire context window, under the pretense that vectors for these states have already attended to earlier ones and thus have sufficient representational power to encode relevant pieces of early context. Due to its simplicity, it’s become one of the more widely adopted approaches towards sub-quadratic scaling, and is used in Mistral’s popular `Mixtral-8x7B` model (among others).

在“`Longformer`”论文中介绍，`滑动窗口注意力` 充当标准注意力的次二次替代品，它允许仅关注最近标记/状态的滑动窗口（令人震惊，对吧？）而不是整个上下文窗口，假装这些状态的向量已经关注了早期的状态，因此具有足够的代表性能力来编码早期上下文的相关部分。由于其简单性，它已成为更广泛采用的次二次缩放方法之一，并用于 Mistral 流行的 `Mixtral-8x7B` 模型（等等）。

Resources: 资源：

+   “What is Sliding Window Attention?” (blog post by Stephen M. Walker)

“什么是滑动窗口注意力？” （斯蒂芬·M·沃克的博客文章）

+   “Sliding Window Attention” (blog post by Manoj Kumal)

“滑动窗口注意力”（Manoj Kumal 的博客文章）

+   [“Longformer: The Long-Document Transformer”](https://www.youtube.com/watch?v=_8KNb5iqblE) (video by Yannic Kilcher)

“`Longformer`：长文档 `Transformer`”（视频由 Yannic Kilcher 拍摄）

## Chapter 41: Ring Attention 第41章：请注意
