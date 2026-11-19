
# 微软 A100 单卡技术实现大模型推理速度提升 10 倍

> 来源：[https://agijuejin.feishu.cn/docx/Q6DFd1VVioUoOnxrsqAcKKcQn7c](https://agijuejin.feishu.cn/docx/Q6DFd1VVioUoOnxrsqAcKKcQn7c)

> 机器之心｜[阅读原文](https://www.jiqizhixin.com/articles/2024-07-08-17)

> 转载请联系原作者取得授权

> 微软的这项研究让开发者可以在单卡机器上以 10 倍的速度处理超过 1M 的输入文本。

大型 语言模型 (LLM) 已进入长上下文处理时代，其支持的上下文窗口从先前的 128K 猛增到 10M token 级别。

然而，由于 注意力机制 的二次复杂度，模型处理输入提示（即预填充阶段）并开始产生第一个 token 可能需要几分钟时间。导致首个 token 生成的时间过长，从而严重影响了用户体验，这也极大地限制了长上下文 LLM 的广泛应用。

举例来说（如图 2a 所示），在单台装有 A100 的机器上为 LLaMA-3-8B 提供服务时，如果提示有 30 万个 token，模型需要 6 分钟才能完成预填充（ pre-filling）阶段，如果提示增加到 100 万个 token，这个数字将增加到 30 分钟。

![](img/3888d5dfec655ae258479126a7d9e6d5.png)

自注意力 计算的开销占到了总预填充延迟的 90% 以上，这使其成为 LLM 处理长上下文时的主要瓶颈。现有的加速预填充方法在应用于长上下文 LLM 时通常无法保持可接受的准确性或效率。

为了解决上述问题，来自微软、萨里大学的研究者提出了一种旨在加速长序列处理预填充的稀疏计算方法：MInference（ Milliontokens Inference ）。

![](img/5596e906de28e1353f5d7c2ee2089939.png)

+   论文地址：https://arxiv.org/pdf/2407.02490

+   论文主页：https://hqjiang.com/minference.html

+   论文标题：MInference 1.0: Accelerating Pre-filling for Long-Context LLMs via Dynamic Sparse Attention

MInference 可以直接应用于现有 LLM，无需对预训练设置进行修改或额外的微调。

通过对各种下游任务（包括 InfiniteBench、RULER、PG-19 和 Needle In A Haystack）以及模型（包括 LLaMA-3-1M、Yi-200K、GLM-4-1M、Phi-3-128K 和 Qwen2-128K）进行评估，实验证明 MInference 可有效将 A100 上的预填充推理延迟降低多达 10 倍，同时保持准确性。

![](img/05bf2a099f2b84e4c049bb0ff53c7974.png)

![](img/70a209b9fa14c5eb24cfcb37f6e710d8.png)

使用 MInference 1.0 ，长上下文 LLM（如 LLaMA-3-8B-1M、GLM-4-1M）在单个 A100 上的推理速度实现了 10 倍提升，并且准确度更高。

## 方法介绍

作者提出了 MInference，这个名字反映了他们希望在一台 A100 机器上实现百万（million）token 推理的雄心。

MInference 是一种无需训练的高效方法，用于基于动态稀疏注意力的长上下文 LLM 的预填充阶段。

研究者认为注意力，特别是在长上下文中，是稀疏和动态的，即在不同的输入中，稀疏模式有很大的不同。这种动态稀疏性呈现出三种适用于所有输入的独特空间聚合模式：A 形（A-shape）、垂直 - 斜线（Vertical-Slash）和块状 - 稀疏（Block-Sparse）。

![](img/849bfc1f7099463cc8c7fc8fc036b3a6.png)

MInference 首先使用内核 感知 稀疏模式搜索算法为每个头部离线确定最佳动态稀疏模式，如算法 1 所示。在推理过程中，它会根据头部的模式动态逼近动态稀疏指数，如算法 2、3 所示。最后，作者使用优化后的 GPU 内核执行高效的动态稀疏注意力计算，大大减少了长上下文 LLM 的预填充阶段延迟。

![](img/f68abbfdf39265962fed81101ea3b2d1.png)

例如，对于「垂直 - 斜线」模式，作者首先利用最后一个 `Q` 和 `K` 之间的注意力计算来估计垂直线和斜线的最佳指数。然后，他们利用动态稀疏编译器 PIT 和 Triton 构建垂直 - 斜线 FlashAttention 内核，加速注意力计算。对于 A 形、垂直 - 斜线和块状 - 稀疏模式，作者首先在注意力计算中使用 `Q` 和 `K` 的均值池。利用均值池和 `MatMul` 的交换属性，可以估算出块状 - 稀疏指数。然后，他们使用 Triton 构建块稀疏 FlashAttention 内核，加速注意力计算。有关内核的详细实现，请参阅附录 C.4 和代码。

## 在长上下文 基准 中的评估结果

作者在一系列场景中测试了 MInference，包括 QA、编码、基于检索的任务、multi-hop QA、总结和数学任务。RULER 基准 包括几个复杂的 multi-hop 或 multi-needle 任务，有效地反映了 LLM 的实际上下文窗口大小。如表 1 所示，MInference 有效地保留了 LLM 的实际上下文窗口处理能力，甚至将实际上下文窗口大小略微扩展到 32K。

![](img/b88c3cfc2cdea6b581b0dccdcf7352cd.png)

作者还使用平均 token 长度为 214K 的 InfiniteBench 在更广泛的任务中测试了 MInference，如表 2 所示。与 SoTA 基线相比，MInference 在所有任务中都始终保持了良好的性能。值得注意的是，在更具挑战性的检索任务（如 KV 检索任务）中，所有基线都无法做出准确预测， 准确率 低于 1.2%。但是，MInference 成功地保留了处理动态 KV 对检索的能力。

![](img/d0a4751865440f58471450416f842977.png)

为了进一步评估不同上下文长度和关键信息在提示中不同位置时的性能，作者使用「大海捞针」任务测试了各种模型和方法。如图 1 所示，MInference 在不同的模型、上下文窗口和提示信息位置下都表现良好，与原始模型相比，其性能保持不变甚至略有提高。在 LLaMA-3-8B 和 GLM-4-9B-1M 的情况下，MInference 在高达 1M 的上下文窗口中实现了完全绿色的性能。相比之下，即使在 70K 上下文窗口中，StreamingLLM 和 InfLLM 在提示的中间段性能也会下降到 20% 以下。

![](img/b6f9eae72324f9120452ebf0d54a6f40.png)

作者还使用 PG-19 在 语言模型 任务中测试了 MInference，其中包括多达 100k 的 token。如图 2 所示，MInference 有效地保持了 LLaMA-3-8B 和 Yi-9B-200K 的困惑度，而所有基线都出现了不同程度的困惑度下降。此外，与标准的 StreamingLLM 相比，使用膨胀和步长配置的 StreamingLLM 更好地保持了困惑度性能。

![](img/5296379ee3ae3720096a4cedbacb7c9b.png)

## 延迟和内核中的稀疏模式

图 3 展示了本文提出的三种注意力模式以及 FlashAttention 的微 基准 测试结果。可以看出，Vertical-Slash 是三种模式中最慢的，但在 1M 上下文窗口下，相比 FlashAttention 仍然实现了 13 倍的加速。

![](img/a2944c4e0d1fc07d1d08ddc29061837f.png)

图 4 展示了 Vertical-Slash 头部内核中的稀疏索引。垂直线通过 PIT FlashAttention 使用 1x64 块计算，而斜线通过块级 FlashAttention 使用 64x64 块计算。

![](img/81d173da87e328857803f7db0c6f748e.png)

> 参考链接：https://hqjiang.com/minference.html
