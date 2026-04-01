# LLM（二十）：漫谈 KV Cache 优化方法，深度理解 StreamingLLM

> 来源：[https://agijuejin.feishu.cn/docx/RAR0dhgfMoGKv2xA2rRcfEMjnZc](https://agijuejin.feishu.cn/docx/RAR0dhgfMoGKv2xA2rRcfEMjnZc)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/659770503)hihu.com/p/659770...

在 Transformer 的 Encoder-base 的模型（如 BERT系列）中，推理和训练过程保持了高度的统一性（差异仅仅在于是否存在反向过程）。而在 Decoder-base 的生成式模型（如 GPT系列）中，推理和训练存在相当大的差异性，主要体现在推理过程具有以下3点特征：

+   **自回归**

+   **两阶段 **（第一阶段输入 prompt，第二阶段输入上一个生成的token）

+   **KV cache**

**以上三点**实际上也是相辅相成、不可**分割的，**其中 自回归 的生成**模式是根本原因， **两阶段 是外在的体现形式， KV cache 是优化手段。

下面将通过梳理整个推理过程，来理解 KV cache 的作用及优化方法。

## 一、KV cache 的由来与基本矛盾

以典型的 GPT2 结构为例。假设第   i i 层 Decoder 的权重矩阵为   WQi,WKi,WVi,WOi,Wupi,Wdowni W^i_Q,W^i_K,W^i_V,W^i_O,W^i_{up},W^i_{down} 。其中，self-attention块的4个权重矩阵   WQi,WKi,WVi,WOi∈Rh×h W^i_Q,W^i_K,W^i_V,W^i_O\in \mathbb{R}^{h\times h} ，并且MLP块的2个权重矩阵   Wupi∈Rh×4h,Wdowni∈R4h×h W^i_{up}\in \mathbb{R}^{h\times 4h},W^i_{down}\in \mathbb{R}^{4h\times h} 。

### 第一阶段（prompt 输入）：

假设第   i i 层的输入为   xi∈Rb×s×h x^i \in \mathbb{R}^{b\times s\times h} ，self-attention块的key、value、query和output表示为   xKi,xVi,xQi∈Rb×s×h x^i_K,x^i_V,x^i_Q \in \mathbb{R}^{b\times s\times h} 。

K cache和V cache的计算过程为：

xKi=xi⋅WKi∈Rb×s×h x^{i}_{K} = x^{i} \cdot W^{i}_{K} \in \mathbb{R}^{b\times s\times h} \\   xVi=xi⋅WVi∈Rb×s×h x^{i}_{V} = x^{i} \cdot W^{i}_{V} \in \mathbb{R}^{b\times s\times h} \\

第  i 层 attention 和 mlp 的计算过程为：

x^{i}_{Q} = x^{i} \cdot W^{i}_{Q} \in \mathbb{R}^{b\times s\times h}\\  x^{i}_{out} = softmax(\frac{x^{i}_{Q} {x^{i}_{K}}^{T}}{\sqrt{h}}) \cdot x^{i}_{V} \cdot W^{i}_{O} + x^{i} \in \mathbb{R}^{b\times s\times h}\\  x^{i+1} = f_{gelu}(x^{i}_{out}\cdot W_{up}) \cdot W_{down} + x^{i}_{out} \in \mathbb{R}^{b\times s\times h} \\

![](img/9b11c39b5d87a72870ff96b7c24b9bb1.png)

### 第二阶段（token by token）：

给定当前生成词在第  i 层的向量表示为  t^{i}\in \mathbb{R}^{b\times 1\times h} 。推断计算分两部分：更新KV cache和计算第  i 层的输出。

更新K cache和V cache的计算过程如下：

x^{i}_{K} \leftarrow Concat(x^{i}_{K}, t^{i}\cdot W^{i}_{K})\in \mathbb{R}^{b\times (s+1)\times h}\\

x^{i}_{V} \leftarrow Concat(x^{i}_{V}, t^{i}\cdot W^{i}_{V})\in \mathbb{R}^{b\times (s+1)\times h}\\

第  i 层剩余的计算过程为：

t^{i}_{Q} = t_{i} \cdot W^{i}_{Q}\in \mathbb{R}^{b\times 1\times h}\\

t^{i}_{out} = softmax(\frac{{t^{i}_{Q}x^{i}_{K}}^{T}}{\sqrt{h}}) \cdot x^{i}_{V} \cdot W^{i}_{O} + t^{i}\in \mathbb{R}^{b\times 1\times h}\\  t^{i+1} = f_{gelu}(t^{i}_{out}\cdot W_{up}) \cdot W_{down} + t^{i}_{out}\in \mathbb{R}^{b\times 1\times h}\\

![](img/538578bdf3f4b9fc90f5650d8d936b1a.png)

### **KV cache的显存占用分析**

假设输入序列的长度为  s ，输出序列的长度为  n ，以 FP16 来保存KV cache，那么 KV cache的峰值显存占用大小为  b(s+n)h * l * 2 * 2=4blh(s+n) 。这里第一个2表示K/V cache，第二个2表示 FP16 占2个bytes。

以 GPT3 (175B) 为例，对比 KV cache 与模型参数占用显存的大小。GPT3 模型weight占用显存大小为350GB (FP16)，层数  l 为96，维度  h 为12888。

| batch size | s+n | KV cache(GB) | KV cache/weight |
| --- | --- | --- | --- |
| 4 | 4096 | 75.5 | 0.22 |
| 16 | 4096 | 302 | 0.86 |
| 64 | 4096 | 1208 | 3.45 |

可见随着 batch size 和 长度的增大，KV cache 占用的显存开销快速增大，甚至会超过模型本身。

**而 LLM 的窗口长度也在不断增大**，因此就出现一组主要矛盾，即： 对不断增长的 LLM 的窗口长度的需要与有限的 GPU 显存之间的矛盾。 因此优化 KV cache 就显得非常必要。

## 二、KV cache 优化的典型方法

### 2.1 共用 KV cache：MQA，GQA

MQA (Multi Query Attention，多查询注意力) 是多头注意力的一种变体。其主要区别在于，在 MQA 中不同的注意力头共享一个K和V的集合，每个头只单独保留了一份查询参数。因此K和V的矩阵仅有一份，这大幅度减少了显存占用，使其更高效。由于MQA改变了注意力机制的结构，因此模型通常需要从训练开始就支持 MQA 。也可以通过对已经训练好的模型进行微调来添加多查询注意力支持，仅需要约 5% 的原始训练数据量 就可以达到不错的效果。包括 Falcon、SantaCoder、StarCoder 等在内很多模型都采用了 MQA 机制。

```
# Multi Head Attention
self.Wqkv = nn.Linear(     # Multi-Head Attention 的创建方法
    self.d_model,
    3 * self.d_model,     # Q、K和V 3 个矩阵, 所以是 3 * d_model
    device=device
)
query, key, value = qkv.chunk(3, dim=2)      # 每个 tensor 都是 (1, 512, 768)

# Multi Query Attention
self.Wqkv = nn.Linear(       # Multi-Query Attention 的创建方法
    d_model,
    d_model + 2 * self.head_dim,    # 只创建Q的头向量，所以是 1* d_model, 而K和V不再具备单独的头向量, 所以是 2 * self.head_dim
    device=device,
)
query, key, value = qkv.split(
    [self.d_model, self.head_dim, self.head_dim],    # query -&gt; (1, 512, 768), key   -&gt; (1, 512, 96), value -&gt; (1, 512, 96)
    dim=2
)
```

![](img/762bb637cfc998840bf811bb842d905a.png)

GQA（Grouped Query Attention，分组查询注意力）是一种介于多头注意力和 MQA 之间的折中方案。它将查询头（Query Heads）分组，并在每组中共享一个键头（Key Head）和一个值头（Value Head）。表达能力与推理速度：GQA既保留了多头注意力的一定表达能力，又通过减少内存访问压力来加速推理速度。

![](img/2e1330dce6ec693c726b0bb12eca9704.png)

### 2.2 窗口优化

KV cache 的作用是计算 attention，当推理时的文本长度  T 大于训练时的最大长度  L 时，一个自然的想法就是滑动窗口。这里又有两种方式：

[1）固定窗口长度（图 ](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2004.05150v2.pdf)b），典型代表即 Longformer ，该方法实现简单，而且空间复杂度只有  \mathcal{O}(TL) ，但是精度下降比较大；

2）KV 重计算（图 c），该方法需要每次计算都重新计算长度为  L 的 KV cache，由于重计算的存在，其精度可以保证，但是性能损失比较大；

![](img/32072afc067412fde480d53e98c8ec57.png)

[3）箭型 attent](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2308.16137.pdf)[ion 窗口，在 LM-I](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2309.17453.pdf)nfinit 中就已经被提出了，其基本原理和 StreamingLLM 是一致的。

![](img/673ce72075846cffd4038002f1782871.png)

### 2.3 量化与稀疏

该类方法是基于压缩的思想，通过量化与稀疏压缩 KV cache 的 显存消耗。

当前主流推理框架都在逐步支持 K[V cache 量](https://link.zhihu.com/?target=https%3A//github.com/InternLM/lmdeploy)化，一个典型的案例[是 lmdeploy](https://link.zhihu.com/?target=https%3A//github.com/InternLM/lmdeploy%23turbomind) ，下图展示了其在 TurboMind 框架下 KV INT8 的支持情况。

![](img/6d8ba42fa380ec316221156753044468.png)

稀疏的方法也比较简单，其做法无外乎以下几种方式：

![](img/2c076b5b5f58da7c72853e546d1971e2.png)

这里最值得一提的是 [H2O ](https://link.zhihu.com/?target=https%3A//browse.arxiv.org/pdf/2306.14048.pdf)。简单来说就是通过动态的评价方式来判断需要保留和废弃的KV值，其评估的算法如下所示：

![](img/a2a12f1ae2fe6a0e4fed179c0ad314d4.png)

结果显示，在 KV cache 稀疏到只有原来的 20% 时仍然可以保持很高的精度。

![](img/4eac518a4222690f49e0da83b3b9955f.png)

### 2.4 存储与计算优化

该方法的典型代表即 [vLLM ](https://link.zhihu.com/?target=https%3A//github.com/vllm-project/vllm)的 PagedAttention，简单来说就是允许在非连续的内存空间中存储连续的 K 和 V。详情可参考笔者之前的文章，在此不予赘述

[  ](https://zhuanlan.zhihu.com/p/638468472)

FlashDecoding 是在 FlashAttention 的基础上针对 inference 的优化主要分为三步：

1.  长文本下将KV分成更小且方便并行的chunk

1.  对每个chunk的KV，Q和他们进行之前一样的FlashAttention获取这个chunk的结果

1.  对每个chunk的结果进行reduce

![](img/7b64a530ebdd5055dbbf60a165c33b24.png)

## 三、StreamingLLM：简洁高效的“无限长度”

StreamingLLM 的基本思想同样是来源于上述的窗口思想，其最大的创新在于提出了识别并保存模型固有的「注意力池」（attention sinks）锚定其推理的初始 token。下面将详细讨论其工作的原理。

### 3.1 精度是如何保证的？

核心的发现： Lost in the Middle 。

多个研究都发现，self-attention 的注意力比较集中于头部和尾部，对文本中段的注意力相对较弱，如下图所示：

![](img/57042ec464a8c7ff75f6b3f041cd3fd7.png)

绘制出 self-attention 的热力图也能看到这一点，由此当文本长度超过额定长度时，头部的 token 就会被遗弃掉，这就会在 softmax 阶段产生很大的问题。

![](img/89b86d4bf4b7ba9cfe603a2456724892.png)

具体来说就是在  x_1 较大的情况下，如果删除  e^{x_1} 会很大程度影响 softmax 输出结果的分布。

\operatorname{SoftMax}(x)_i=\frac{e^{x_i}}{e^{x_1}+\sum_{j=2}^N e^{x**_j}},** \quad x_1 \gg x_j, j \in 2, \ldots, N \\  e^{x_1} 会很大的原因并非是 token 决定的，而是其 起始位置 决定的，这也是论文中即使使用 4"\n" 精度仍然较高的原因。

实际上这一发现也在 Attention Is Off By One 中被详细讨论过，并提出要用 softmax1 替代 softmax 。

### 3.2 “无限长度”是如何做到的？

该问实际上可以换种表述为： 如何在文本长度不断增加的情况下，保证GPU显存不会溢出 。由于该方案主要应用于多轮对话的场景，那么有必要回顾一下当前多轮对话生成的主流做法，概括起来就以下几点：

+   将用户输入与模型输出拼接，中间做必要分割；

+   多个轮次之间倒序排列，并拼接；

+   如果前边所有轮次长度之和超过最大长度，则截断到最大长度；

上述过程可以用代码描述如下：

```
    history = [&quot;\n[|Human|]{}\n[|AI|]{}&quot;.format(x[0], x[1]) for x in history]
    history.append(&quot;\n[|Human|]{}\n[|AI|]&quot;.format(text))
    history_text = &quot;&quot;
    flag = False
    for x in history[::-1]:
        if tokenizer(prompt + history_text + x, return_tensors=&quot;pt&quot;)[&quot;input_ids&quot;].size(-1) &lt;= max_length:
            history_text = x + history_text
            flag = True
        else:
            break
    if flag:
        inputs = tokenizer(prompt + history_text, return_tensors=&quot;pt&quot;)
        input_ids = inputs[&quot;input_ids&quot;][:, -max_length:].to(device)
        torch.cuda.empty_cache()
        return input_ids, text
    else:
        return None
```

实际上这就是典型的 **滑动窗口 **的做法，滑窗  l 的存在保证了 GPU 的显存不会溢出，但是由于上节的讨论，会存在精度损失。

![](img/e56a5cc4f5602a28d1c0e8acd532f6bb.png)

而 StreamingLLM 的做法可以理解为将滑窗  l 分为固定部分加上滑动部分，即：  [4, l-4] ，那么需要 GPU 的显存的上限就是确定的，即可保证长度的“无限”增加。

## 参考资料

[1] [E](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2309.17453.pdf)FFICIENT STREAMING LANGUAGE MODELS WITH ATTENTION SINKS

[2] [GQA: Training Generalized Multi-Query Transf](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.13245.pdf)ormer Models from Multi-Head Checkpoints

[3] [使](https://link.zhihu.com/?target=https%3A//developer.nvidia.com/zh-cn/blog/accelerated-inference-for-large-transformer-models-using-nvidia-fastertransformer-and-nvidia-triton-inference-server/)用 FasterTransformer 和 Triton 推理服务器加速大型 Transformer 模型的推理

[4] [H2O: Heavy-Hitter Oracle for Efficient Gener](https://link.zhihu.com/?target=https%3A//browse.arxiv.org/pdf/2306.14048.pdf)ative Inference of Large Language Models

[5] [https:// browse.arxiv.org/](https://link.zhihu.com/?target=https%3A//browse.arxiv.org/pdf/2306.06000.pdf)pdf/23 06.06000.pdf

[6] [分](https://zhuanlan.zhihu.com/p/624740065)析transformer模型的参数量、计算量、中间激活、KV cache - 知乎 (zhihu.com)

[7] [https:// browse.arxiv.org/](https://link.zhihu.com/?target=https%3A//browse.arxiv.org/pdf/2307.03172.pdf)pdf/23 07.03172.pdf

[8] [https:// arxiv.org/pd](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2004.05150v2.pdf)f/2004.0515 0v2.pdf

[9] Attention Is Off By One

> 荷尽已无擎雨盖，菊残犹有傲霜枝。—— 苏轼《赠刘景文》