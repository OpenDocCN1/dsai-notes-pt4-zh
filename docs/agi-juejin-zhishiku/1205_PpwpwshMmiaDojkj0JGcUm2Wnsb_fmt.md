
# `LLM`（廿四）：`Transformer` 的结构改进与替代方案

> 来源：[https://agijuejin.feishu.cn/docx/Yr8ldsyJDoDqmoxT7gVcsMXGndd](https://agijuejin.feishu.cn/docx/Yr8ldsyJDoDqmoxT7gVcsMXGndd)

> 作者：紫气东来

> 原文：[https://zhuanlan.zhihu.com/p/673781418](https://zhuanlan.zhihu.com/p/673781418)

自从 `Transformer` 结构被提出以来，以 `BERT` 为代表的 `Encoder` 模型，以 `GPT` 为代表的 `Decoder` 模型，以 `ViT` 为代表的 CV 模型，都烜赫一时。时至今日，几乎所有的 `LLM` 也都是 `Transformer` 结构，尽管不时也会有新的结构被提出来，但 `Transformer` 的江湖地位仍然无可撼动。那么本篇将从 `Transformer` 结构出发，将主要围绕以下问题展开思考和讨论：

+ `Transformer` 结构为什么有效，其结构中的要点和核心在哪？

+ `Transformer` 与之前的机器学习算法和模型有何联系，如何从其他角度认识 `Transformer` ？

+ `Transformer` 结构的训练和推理效率如何，如何平衡效果和效率？

+ 是否存在 `Transformer` 之外的新结构可以以更低的成本，达到同等或更高的效果？

希望通过以上问题的思考和讨论，能够帮助我们更好地使用`Transformer`、`理解Transformer`、`优化Transformer`和改进`Transformer`。

## 一、再谈 `Transformer` ，理解不可能三角

### 1.1 `Transformer` 结构再认识

本篇的主要内容是围绕 `Transformer` 的结构展开的，因此深入了解 `Transformer` 结构及其工作原理是非常必要的。实际上，在笔者之前的文章中已经分析过 `Transformer` 的结构

[紫气东来](https://zhuanlan.zhihu.com/p/476585349)：NLP（五）：`Transformer`及其`attention`机制 35 赞同 · 2 评论 文章

及一些针对性的可解释研究方法

[紫气东来：可解释人工智能 (三): 基于博弈论的可解释性研究方法](https://zhuanlan.zhihu.com/p/614990018) 23 赞同 · 0 评论 文章

为了简明起见，本篇将尽可能不与之前讨论过的内容重复，而将试图从问题的角度出发而进行相关思考与讨论。

简单而言，`Transformer` 结构及工作原理可以用下图表示，其中图左为 `Encoder` 结构，图右为 `Decoder` 结构。

![](img/4c42e7c703d0d39d6b80caf10764c60f.png)

**问题一**：`Encoder` 和 `Decoder` 结构分别适用于什么场景，为什么生成式模型普遍采用 `Decoder-only` 结构

当前，`Transformer` 结构中的 `Encoder` 和 `Decoder` 结构构成了 3 种主要的模型范式，如下表所示：

| 结构 | 特点 | 常见任务 | 典型代表 |
| --- | --- | --- | --- |
| `Encoder-only` | `auto-coding` | 序列分类，`NER`，抽取问答，`MLM` | `BERT`, `RoBERTa`, `DeBERTa` |
| `Decoder-only` | `auto-regressive` | 文本生成，`CLM` | `GPT`, `Llama`, `PaLM` |
| `Encoder-Decoder` | `sequence-to-sequence` | 翻译，总结，生成问答 | `BART`, `T5`, `Marian` |

如果从直观上来解释，`Encoder-only` 的注意力是双向的，要确定一个 `mask` 值，需要同时考虑之前和之后的文本信息，因此在问答和填空类的场景比较有效。而人类的语言生成过程具有天然的马尔科夫性，在生成过程中没有下文信息可以参考，因此刚好契合 `Decoder-only` 结构的 `token by token` 的生成方式。

除了上述解释外，还有从 [低秩角度](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/9529)来解释的，即在双向注意力中由于 `softmax` 操作会导致低秩问题，而在单向注意力中，`attention` 矩阵是满秩的下三角矩阵，因此表达能力更强。另外还有从 `scaling law` 等角度进行解释的，在次不一一展开。

问题二：`attention` 机制有什么深刻内涵？

让我们重新回顾一下，`attention` 的计算过程

这个过程可以简单概括为 3 步：

1. 衡量 和 之间的相似性，这种相似性对于向量来说就是内积，如下视频所示

1. 查找权重 `a`。这是通过 `SoftMax` 完成的。相似性像一个完全连接的层一样连接到权重。

1. 值的加权组合。将 `a` 的值和 `V` 对应的值相乘累加，最终输出是所需的结果注意力值。

通过这种方式，建立起了序列间的全局的联系。这里还有一些细节需要补充说明：

+ 来源于相同的 [`embedding`](https://www.zhihu.com/search?q=embedding&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A673781418%7D) 输入 `X` ，经过不同的权重矩阵 `W_Q`, `W_K`, `W_V` 投影到不同的空间中

+ `d_k` 作为调节因子，使得内积不至于太大，从而使梯度稳定很多

+ `SoftMax` 之后的注意力分数，其分值大小代表了相关性强弱，这种差异在计算梯度时，可以相对均匀地流入多个`token`位置

+ 由于在计算[`注意力矩阵`](https://www.zhihu.com/search?q=%E6%B3%A8%E6%84%8F%E5%8A%9B%E7%9F%A9%E9%98%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A673781418%7D)时每个 `token` 都与其他所有 `token` 交互，因此输入没有时间顺序，这就是输入需要加入 `position embedding` 的根本原因

+ 计算注意力矩阵的计算复杂度为 ，计算的过程需要大量计算资源

![](img/0b5927a91a4fa80d22e1f3dce8442664.png)

[25f76526-a166-11ee-8e07-d26cc6cc8782-v8_f2_t1_CEbm3jGT.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MA5yboyktoR5t2xkp4EczCh4nkc)

问题三：为什么需要多头，是否有深层含义？

看到多头机制，我们可能会比较自然想到 `CNN` 中的多通道，这样可以实现更好的 并行化效果 ，那么除此之外是否还存在更深层次的原因呢？

根据原文观点，将隐状态向量分成多个头，形成多个子语义空间，可以让模型去关注不同维度语义空间的信息 。简单来说就是把 `Scaled Dot-Product Attention` 过程做 `H` 次，再把输出合并起来。

多头注意力机制的公式如下：

![](img/033d9c4f2b3aac6385ecea2e715bbbd0.png)

从更加深刻的角度去认识这个问题，有相当多的研究从 秩 的角度进行分析，即认为 `attention` 操作本身就是一个低秩操作，多头机制进一步使低秩问题更加严重，但好处是多头避免了单头的低秩使某些信息丧失，即多头增强了信息关注的多样性。关于这方面的详细推导和证明可参考

+ [The Truth is in There: Improving Reasoning in Language Models with Layer-Selective Rank Reduction](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2312.13558.pdf)

+ [Mixhead: Breaking the low-rank bottleneck in multi-head attention language models](https://link.zhihu.com/?target=https%3A//www.sciencedirect.com/science/article/abs/pii/S0950705121011503)

+ [Multi-Head Attention: Collaborate Instead of Concatenate](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2006.16362.pdf)

等文章，笔者有机会专门出一期进行讨论，在此不予赘述。

**问题四**：前馈层(`feed-forward layers` or `MLP`) 的作用是什么，如何发挥作用的？

全连接网络是最基本的模型，在 `Transformer` 架构中包括：2 个线性层(升维 and 降维)，1 个激活函数，一个残差连接。本节将试图从 `memory` 的角度理解其作用，即考虑 `FFNs` 是如何转化和记忆注意力机制中的信息的。

考虑 `Memory Network` (`MN`) 的基本原理与过程：

+ `Memory Network` 包含 `d_m` 维键值对，这些键值对则被称为记忆(`memories`)，每个 `key` 由 `d` 维向量组成 ，并构成参数矩阵 , 同理

+ 对于输入向量 , 通过 `key` 计算其分布，再通过 `value` 计算其期望，即

该式可简写为 。

而 `Transformer` 中的前馈层(不考虑`bias`)可以简单表示为：

这样就完成了前馈层和记忆网络的形式上的统一。当然，这里的论证过程比较粗浅，详细严谨的论证过程可参考：

+ `End-To-End Memory Networks`

+ [Transformer Feed-Forward Layers Are Key-Value Memories](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2012.14913.pdf)

+ [MLP-Mixer: An all-MLP Architecture for Vision](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2105.01601)

### 1.2 从其他经典模型视角看 `Transformer`

#### 1.2.1 从 `SVM` 角度看 `Transformer`

该部分内容主要参考论文 [Transformers as Support Vector Machines](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2308.16898.pdf) ，在此仅讨论其核心观点，详细推理证明过程，请参考原文。

该研究证明了 `Transformer` 架构中自注意力层的优化几何学与硬间隔支持向量机（`SVM`）问题之间的形式等价性。这种联系是通过自注意力层中输入标记序列`X`的外积对数线性约束来实现的，旨在将最优输入标记与非最优标记分离，即：落在 `SVM` 决策边界错误一侧的「坏」`token` 被 `softmax` 函数抑制，而「好」`token` 是那些最终具有非零 `softmax` 概率的 `token`。还值得一提的是，这个 `SVM` 源于 `softmax` 的指数性质。

`Transformer` 中的注意力层接受输入 `X`，并通过计算 评估 `token` 之间的相关性，其中  是可训练的矩阵参数，最终有效捕获远程依赖关系。该研究在此基础上证明了以下几点：

1. **自注意力层的隐式偏差**：通过梯度下降优化带有消失正则化（`vanishing regularization`）的自注意力参数 ，会收敛到一个最小化核范数的 `SVM` 解，而不是全局最优解。直接通过核范数 参数化，会最小化`Frobenius`范数 `SVM` 目标。

1. **梯度下降的收敛性**：在适当的几何条件下，梯度下降（`GD`）迭代会收敛到一个局部最优的`SVM`解。作者们证明了在适当的初始化和线性头`h(·)`的情况下，`GD`迭代会收敛到一个局部最优解。重要的是，过度参数化通过确保 `SVM` 问题的可行性和保证没有驻点（`stationary points`）的良性优化环境来催化全局收敛。

1. **`SVM`等价性的一般性**：当使用线性头`h(·)`进行优化时，自注意力层固有地偏向于从每个序列中选择单个标记（硬注意力）。然而，非线性头需要组合多个标记，这强调了`Transformer`动态中的重要性。作者们提出了一个更一般的`SVM`等价性，准确预测了在非线性头/`MLP`情况下，通过梯度下降训练的注意力的隐式偏差。

总的来说，这些特性的证明有助于更深入地理解`Transformer`模型的内部工作机制，特别是在自注意力层如何通过优化过程来选择和组合输入序列中的 `token`，即可以将多层 `Transformer` 看作分离和选择最佳 `token` 的 `SVM` 层次结构。

#### 1.2.2 从 `GNN` 角度看 `Transformer`

`GNN` 和 `Transformer` 的关联在于二者形式上的相似性，不妨回顾一下`GNN`的基本原理：

图神经网络（`GNN`）通过邻域聚合（或消息传递）来构建图数据中节点和边的表示，其中每个节点从其邻域收集特征，以更新其周围局部图结构的表示。`GNN` 通过对节点自身特征 的非线性变换，将其添加到每个相邻节点的特征集合 中，从而在层 上更新节点 的隐藏特征 ，即：

其中  是 `GNN` 层的可学习权重矩阵，`σ` 是 `ReLU` 等非线性因素。在下图左的示例中，有 。

![](img/307cf12ec44d5f2015627c5df75598a8.png)

为了使连接更明确，将一个句子视为一个全连接图，其中每个单词都与其他每个单词连接。现在，我们可以使用 `GNN` 为图（句子）中的每个节点（单词）构建特征，然后我们可以用它来执行 `NLP` 任务。概括地说，这就是 `Transformers` 正在做的事情：它们是具有多头注意力的 `GNN`，作为邻域聚合函数。标准 `GNN` 聚合来自本地邻域节点的特征，而 `NLP` 的 `Transformers` 将整个句子视为本地邻域，聚合每个邻域的特征每层的单词。

![](img/6c8b24f5c4a80ff99f246e729b51f56b.png)

如果我们要进行邻域聚合的多个并行头，并用注意力机制（即加权和）代替对邻域的求和，我们就会得到图注意力网络（`Graph Attention Network`, `GAT`），如上图左所示。从某种程度上说，`GAT` 与 `attention` 已经完成了形式上的统一。还有一点需要注意的是，`GNN` 的图连接通常不是全连接的，而 `Transformer` 的关于稀疏性的研究也证明了非全连接同样是有效的。

#### 1.2.3 从 `RNN` 角度看 `Transformer`

上一小节构造了 `GNN` 和 `Attention` 的形式上的统一，那么能否将 `Transformer` 推理的 `token by token` 的过程和 `RNN` 构造起形式上的统一呢？答案是确定的。

在论文 `Transformers are Multi-State RNNs` 中，作者提出了一个观点：`Transformers`（特别是 `Decoder`）可以被概念化为无限多状态循环神经网络（`MSRNNs`），其中`KV`向量对应于一个动态无限增长的多状态。之所以可以这么等价，我们需要回顾一下 `RNN` 的原理

其中  表示 层 时刻的隐藏状态， 则表示对应的输入。

接下来，将 `Transformer` 的推理过程改造成这种形式，首先我们知道在推理情况下

其中  来源于 ，考虑到使用 `KV cache` 的情况，则可以进一步表示为

如果令 , 则上式可以进一步简化为

即和 `RNN` 构成了形式上的统一。如下图所示，其无限表示即保留所有的 `KV cache`，而其有限表示则是进行了 `KV cache` 的稀疏化处理。

![](img/216283bbd62b8aa97cd83600fefd22fd.png)

此外改论文还提出了：

+ 一种新的压缩策略`TOVA`（`Token Omission Via Attention`），该策略基于注意力分数选择保留哪些`token`。

+ 在多个长距离任务上进行了实验，结果表明`TOVA`在性能上优于其他基线策略，并且在某些情况下，仅使用原始缓存大小的1/8

总结一下本节的核心结论，即：

+ 通过 `SVM` 的视角证明了 `Transformer` 的优化过程，就是将「好」`token` 和「坏」`token` 实现最大化硬分隔的过程，一定程度上揭示了其训练过程的本质；



+ 通过 GNN 的视角证明了 Transformer 的 attention 机制实际上就是一种密集的关联机制，对理解其工作原理和稀疏化的有效性非常重要；

+ 通过 RNN 的视角揭示了 Transformer 文本生成过程的本质，即 `token by token` 的形式，每一个 token 的生成都依赖于之前的状态，形成了前后状态之间的深刻关联性。

### 1.3 浅谈 Transformer 的不可能三角

Transformer 的有效性很大程度上来源于 attention 机制，而 attention 机制的最大特点即在于其复杂度与多头机制的并行性。这样在文本生成这类的 `token by token` 任务上，由于每步的二次复杂度和 `KV cache` 的内存限制，在推理过程中表现出较低效率。

很多研究（如 `Linformer`）期望可以降低 attention 的复杂度，这样就可以表现出较好的推理效率，但是这样就一定程度上牺牲了模型精度和效果。

另外基于传统的 `RNN` 的模型可以达到较好的效果和高效的推理效率，但是由于其没有多头机制，无法实现高效的并行训练，因此无法扩大规模。

![](img/77ef14d224c5077d3a90fa42ff0e79a6.png)

也就是说，这些模型架构面前摆着一个“不可能三角”，三个角代表的分别是：并行训练、低成本推理和良好的扩展性能。因此如何平衡这三点，成为优化与改进 Transformer 结构的非常重要的课题。

## 二、改进 Transformer，如何平衡效率和效果

### 2.1 思路一：降低 Attention 的复杂度

由于 attention 机制的复杂度，一个最直观的想法就是尽可能降低其复杂度。该类方法在笔者之前的文章中已有多次讨论，可参考以下文章，在此不予赘述

[紫气东来：NLP（二十）：漫谈 KV Cache 优化方法，深度理解 StreamingLLM](https://zhuanlan.zhihu.com/p/659770503) 441 赞同 · 16 评论 文章

![](img/bff064d2b90d377e39451d0a2141d91b.png)

[本节将主要介绍另外两种降低 `attention` 复杂度的方法：`Linformer`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2006.04768.pdf) 和 [`FNet`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2105.03824.pdf) 。

### 2.1.1 Linformer 与 Attention 线性化

Linformer 的核心发现是 `Attention` 矩阵是低秩，即可以用低秩矩阵来近似，即对于一个 `N x N` 的 `Attention` 矩阵，可以用一个低秩矩阵（比如 `N x 128`）就已经足够存储 `attention` 的大部分信息。

![](img/9e5c230b008fa8bcb4dbedd44dd9fa70.png)

Linformer 对 `Self-Attention` 的结构进行了一些修改，使复杂度降到线性，[修改后的 `Self-Attention` 结构](https://www.zhihu.com/search?q=%E7%BA%BF%E6%80%A7%E6%98%A0%E5%B0%84%E5%B1%82&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A673781418%7D)如下图所示，主要的区别在于增加了两个线性映射层（用矩阵 `E` 和 `F` 表示）。线性映射层 `E` 和 `F` 维度是 `k×n`，原来计算得到的矩阵维度是 `n×d`，通过新增的两个线性映射层可以把其转为 `k×d` 的矩阵，因此得到的 `Attention` 矩阵 `P̄` 维度是 `n×k`。

![](img/80b330cea4b18f0b44e34f7fee6a2129.png)

尽管该工作宣传把复杂度降到了线性，稍加分析便会发现并非如此，把维度从 `512` 降到 `128` 是可行的，也并不代表可以从 `4096` 也降到 `128`，因此只能说线性降低了复杂度，而不是把复杂度降到了线性。

### 2.1.2 基于傅里叶变换的 FNet

既然 `Attention` 的本质是信息的交互，那么除了这种方式之外还有其他方式实现信息交互吗？是的，我们在复分析中常用的傅里叶变换就具有这样的能力。`FNet` 使用了傅里叶变换来实现 token 内部和外部的信息交互。

傅里叶变换的公式如下，从公式中可以看出，傅里叶变换生成的每个元素，都是原始序列中所有 token 信息的融合，这也是实现信息交互的一种方式。

每个层都由一个傅里叶混合子层和一个前馈子层组成。研究者将每个 transformer 编码器层的自注意力子层替换为傅里叶变换子层，该子层将 2D 傅里叶变换应用于其 `embedding` 输入 - 沿着序列维度和隐藏维度进行一维傅里叶变换。

应用傅里叶变换后，`embedding` 会被转换到频域。傅里叶变换的每个输出都有每个输入 `embedding` 的一个分量。这样，所有 `embedding` 的信息都能得到处理。因此，通过傅里叶变换变相实现了注意力机制。

![](img/2e9bfac9d739923dfb56c1031930785c.png)

通过这种方式，算法复杂度被降到了线性。还需要说明的是 `FNet` 中的傅里叶变换层并没有参数需要训练，因此网络中仅有前馈层的参数需要训练（当然 `layer norm` 中还有很小部分），这也大大降低训练成本。从结果来看，`FNet` 能够以 7 倍的速度达到 `BERT` 92%，也反映了其优化的效果。

当然，对于 Transformer 的结构优化是无止境的，以上仅仅是两个有趣的例子，更多的案例包括但不限于：

+ [`Longformer`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2004.05150.pdf)
+ [`Transformer-XL`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1901.02860.pdf)
+ [`Reformer`](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2001.04451)
+ `Swin Transformer`
+ `Lite Transformer`
+ `Lightning Attention-2`
+ [`MASFormer`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2310.12442.pdf)

### 2.2 思路二：结构并行化

既然 Transformer 的一大短板是推理效率较低，那么是否可以通过进一步增加并行程度来实现呢？当然！

### 2.2.1 Attention 与 FFN 的并行

我们注意到在 `Decoder layer` 中 `attention` 模块和 `FFN` 模块是串行连接的，那么就有了第一种方法。这种方法是在 `GPT-J` 模型中被首次提出的，即将 `attention` 模块和 `FFN` 模块并行连接，如下图右所示，这样很自然可以提高计算效率。但是由于我们在 1.1 节问题四中讨论过 `FFN` 是对 `attention` 输出的隐藏表示的记忆，因此这种并行化实际上解耦了这种关系，或许也正是因为这一点，该方案并没有被广泛采用。

![](img/862d3e7474e78495635e6674524a8c23.png)

在 `GPT-J` 之后，`SIMPLIFYING TRANSFORMER BLOCKS` 又进一步简化了这种并行。该方案将 `attention` 公式简化为

这样做带来了以下几点变化：

+ 线性变换：用 `X` 替换 `V`，将值变换变成线性运算，降低注意力机制的复杂度。

+ 减少参数空间：从方程中消除 `V` 减少了可训练参数的数量，从而提高训练过程的效率并可能加快收敛速度。

+ 对模型能力的影响：虽然这种简化降低了复杂性，但它也会影响模型学习复杂数据表示的能力。

+ 改变的数据流：通过单位矩阵，通过网络的数据流发生变化。注意力机制现在直接传播按注意力权重缩放的输入数据 `X`，从而改变网络内信息处理的动态过程。

![](img/85caf9d94a116ea4b828563a6d97f887.png)

同时 `MHA` 和 `MLP` 进行并行化计算，如下：

其中 、 和 是可训练参数，分别控制输入、`MLP` 和 `MHA` 块对输出的贡献。

同时该方案还删除了归一化操作和残差连接，极大程度简化了模型结构。

### 2.2.2 深入浅出理解 MoE 方案

另一种并行化的方式是只对 `FFN` 进行拆分并进行并行化，这就是当前炙手可热的 `Mixture of Expert (MoE)` 方案。`MoE` 方案的核心在于：`稀疏 MoE 层` 以及 `门控网络 (router)`。

+ `稀疏 MoE 层` 取代了传统的密集型前馈网络（`FFN`）层。`MoE` 层内部设有一定数量的“专家”，每个专家实际上是一个独立的神经网络。

+ `门控网络` 负责决定将哪些 tokens 分配给哪个专家。如何将 token 正确地路由到某个专家是在使用 `MoE` 时需要做出的重要决策之一：这个门控网络由可学习的参数构成，并且会与网络的其他部分同时进行预训练。

![](img/707ee0eb1d494a236438816764a38c1f.png)

笔者接下来以 [`DeepSeekMoE`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2401.06066.pdf) 为例做更进一步地说明。`DeepSeekMoE` 通过两种主要策略来提高专家的专业化程度：

1. 细粒度专家分割（Fine-Grained Expert Segmentation）：

+ 保持专家参数数量不变，将每个专家的中间隐藏维度分割成更小的专家（例如，将一个专家分割成多个小专家，每个小专家的参数量是原专家的 `倍`）总数为 `个`。

+ 相应地，激活更多（`个`）的细粒度专家（例如，激活 `倍` 的小专家），以保持相同的计算成本。

+ 这种细粒度分割允许更精细地分解和学习不同的知识到不同的专家中，每个专家保持更高的专业化水平。同时，激活专家的组合更加灵活，有助于更准确和针对性的知识获取。

其公式表示如下：

![](img/01e1221c785e9532c12458143219af29.png)

2. 共享专家隔离（Shared Expert Isolation）：

+ 将一部分（`个`）专家隔离出来作为共享专家，这些共享专家总是被激活，旨在捕获和整合不同上下文中的共同知识。

+ 通过将共同知识压缩到这些共享专家中，可以减轻其他路由专家之间的参数冗余，提高参数效率，并确保每个路由专家专注于其独特的方面。

增加该部分后公式表示如下：

`DeepSeekMoE` 架构的这两个策略共同工作，以训练一个参数效率高的 `MoE` 语言模型，其中每个专家都高度专业化。其实现的核心代码如下：

```
class DeepseekMoE(nn.Module):
    """
    A mixed expert module containing shared experts.
    """
    def __init__(self, config):
        super().__init__()
        self.config = config
        self.num_experts_per_tok = config.num_experts_per_tok
        self.experts = nn.ModuleList([DeepseekMLP(config, intermediate_size = config.moe_intermediate_size) for i in range(config.n_routed_experts)])
        self.gate = MoEGate(config)
        if config.n_shared_experts is not None:
            intermediate_size = config.moe_intermediate_size * config.n_shared_experts
            self.shared_experts = DeepseekMLP(config=config, intermediate_size = intermediate_size)
    
    def forward(self, hidden_states):
        identity = hidden_states
        orig_shape = hidden_states.shape
        topk_idx, topk_weight, aux_loss = self.gate(hidden_states)
        hidden_states = hidden_states.view(-1, hidden_states.shape[-1])
        flat_topk_idx = topk_idx.view(-1)
        if self.training:
            hidden_states = hidden_states.repeat_interleave(self.num_experts_per_tok, dim=0)
            y = torch.empty_like(hidden_states)
            for i, expert in enumerate(self.experts):
                y[flat_topk_idx == i] = expert(hidden_states[flat_topk_idx == i])
            y = (y.view(*topk_weight.shape, -1) * topk_weight.unsqueeze(-1)).sum(dim=1)
            y =  y.view(*orig_shape)
            y = AddAuxiliaryLoss.apply(y, aux_loss)
        else:
            y = self.moe_infer(hidden_states, flat_topk_idx, topk_weight.view(-1, 1)).view(*orig_shape)
        if self.config.n_shared_experts is not None:
            y = y + self.shared_experts(identity)
        return y
```

## 三、认识新结构，如何挑战 Transformer

### 3.1 RNN 重塑 Transformer —— RWKV

### 3.1.1 Attention 一定需要么？——从 AFT 谈起

`Attention Free Transformer (AFT)` 是Apple公司提出的一种新型的神经网络模型，它在传统的 `Transformer` 模型的基础上，通过使用像 `Residual Connection` 之类的技术来消除注意力机制，从而减少计算量和提升性能。其核心公式为

其中 表示逐元素相乘（`element-wise product`）， 为可训练参数，`b` 表示位置偏置。

![](img/60292202a0010e80d3c58625be85a58a.png)

该操作有以下几点值得一提：

+ 在 `Transformer` 中，同一个 `Value` 中不同 `dimension` 的 `weight` 是一致的，而 `AFT` 同一 `Value` 中不同 `dimension` 的 `weight` 不同

+ `attention score` 的计算也变得格外简单，用 `K` 去加一个可训练的 `bias`（位置偏置）。`Q` 的用法很像一个 `gate` 单元

+ `AFT` 在 `inference` 阶段复用前面时刻的计算结果，表现如 `RNN` 的递归形式，从而相比于 `Transformer` 变得更加高效

### 3.1.2 从 AFT 到 RWKV

首先解释一下 `RWKV` 的一些概念：

+ `R`：`Receptance`，表示过去信息的接受，用 `sigmoid` 激活函数

+ `W`：`Weight`，权重是位置权重衰减向量，是可训练的模型参数（后面还会再出来个 `U`，是对当前位置信号的补偿）

+ `K`：`Key`，类似于传统 `Attention` 中的 `K` 向量

+ `V`：`value`，类似于传统 `Attention` 中的 `V` 向量

从宏观来看，`RWKV` 架构由一系列堆叠的残差块（这个结构显然参考了 `Transformer`）组成，每个残差块由具有循环结构的时间混合和通道混合子块形成，整体上形成了 `RNN` 的结构。

在 `RWKV` 的结构中，其中的递归被表述为当前输入和前一个时间步的输入之间的线性插值（即 `time-shift mixing` 或 `token shift`）。这种结构

+ 可以针对输入嵌入的每个线性投影（例如 `timemixing` 中的 `R`、`K`、`V`，以及 `channel-mixing` 中的 `R`、`K`）进行独立调整，并作为 `WKV` 的时间相关更新

+ `WKV` 计算与 `AFT` 类似，但 `W` 现在是“通道向量”乘以“相对位置”，而不是 `AFT` 中的 `pairwise position matrix`。此外还引入了一个向量 `U` 来单独关注当前 token，以补偿 `W` 的潜在退化

![](img/962810b6e8949f6b598a94e8122ea535.png)



以上整体展示了 RWKV 的结构，下面进一步细化了关键模块的细节，其中每个时间步的主要元素之间的相互作用都是 乘法 的。当然这些还不足以是我们理解 RWKV 的设计细节，还需要下文更加细节的讨论。

![](img/82d3d26a5345c7316801ecda812aefcc.png)

### 3.1.3 RWKV的时间混合(time mix)模块与通道混合(channel mix)模块

首先来看时间混合(time mix)模块

![](img/ff362c2cdbce36cc60072911c7c2e528.png)

假设输入`sequence`是`My name is`，目前，则这里`token` 是上一个输入`token`（`My`），`token` 是这个输入`token`（`name`）。`w` 是遗忘因子，越大对上个`token`（`My`）就忘的越多，也就是对这个`token`（`name`）更专注。用作者的话说，就是递归被表述为当前输入和前一个时间步的输入之间的线性插值，也叫`token shift`。

`𝑟`,`𝑘`,`𝑣` 本质上都是 线性组合的变换，注意，这里输入的是前一个`token`的输入（`embedding`），不是输出（`hidden state`）。

`WKV`的计算，即 ， 类似于 `Transformers` 中 `Attn(Q, K, V)` 的角色，而不会产生`quadratic`成本，因为计算的都是标量。其中 ，其中 ，`d`是通道数。`w`为非负数，以确保 并且每通道权重随时间向后衰减。

RWKV能减小复杂度的关键是 `RNN` 模式，因此只需要上一时刻的`state vector`和这一时刻的输入。因此，生成的每一个`token`只要考虑常数个变量，所以复杂度为 。

直观上，随着时间`𝑡`的增加，向量 取决于较长的历史，由越来越多的项的总和表示。对于目标位置`𝑡`，RWKV在`[1, 𝑡]`的位置区间进行加权求和，然后乘以接受度`σ(𝑟)`。因此，交互作用在给定的时间步长内是乘法的，并在不同的时间步长上求和。

接下来看通道混合(`channel mix`)模块

该模块类似 `Transformer` 中的 `MLP` 结构，只是这里也考虑了前文输入。另外需要注意的是。这里采用平方 `ReLU` 激活

![](img/1705c74955e1ec44b4b1c854eecf8266.png)

在循环网络中，使用状态 `𝑡` 的输出作为状态 `𝑡+1` 的输入是很常见的。这在语言模型的自回归解码推理中尤其明显，要求每个标记在输入下一步之前进行计算，从而使得RWKV 利用其类似 `RNN` 的结构，称为`time-sequence mode`。在这种情况下，可以方便地递归地制定 RWKV 以便在推理过程中进行解码，如`附录 B` 所示，它利用了每个输出`token`仅依赖于最新状态的优点，该状态具有恒定的大小，而与序列长度无关。

然后，它充当 `RNN` 解码器，根据序列长度产生恒定的速度和内存占用，从而能够更有效地处理较长的序列。相比之下，自注意力通常需要 `KV` 缓存相对于序列长度线性增长，从而导致效率下降，并且随着序列变长而增加内存占用和时间。

![](img/f646814d536da116cae2196fc863f226.png)

### 3.2 RNN 与 Transformer 的调和 —— RetNet

RetNet 可以说是我 2023 年看过的最精巧的工作了，每次看都会有新的发现和感悟。下面让我们来尽可能逐步认识其工作的机制。

概括来说，RetNet 综合了 `Transformer` 和 `RNN` 的优点，即采用并行训练、循环推理。RetNet 使用 `Transformer` 的自注意力模块来并行化训练并实现最先进的性能，但它不会遇到推理成本和内存复杂性问题。这是由于在推理阶段，它采用 `retention` 模块+循环推理范式。RetNet 共有三种计算范式，以适应不同场景：

+ 并行表示（`Parallel representation`）：赋予训练并行性以充分利用 `GPU` 设备
+ 循环表示（`Recurrent representation`）：能在内存和计算方面实现高效的复杂度推理过程。部署成本和延迟可以大大降低。此外，无需使用键值缓存技巧，大大简化了实现过程
+ 分块循环表示（`Chunk-wise recurrent representation`）：可以执行高效的长序列建模。对每个局部块进行并行编码，以提高计算速度，同时对全局块进行递归编码，以节省 `GPU` 内存

接下来我们将分别这三种计算范式，以体会其构思的巧妙：

#### 3.2.1 用于训练的并行表示

RetNet 的训练过程采用了 `Transformer` 的多头并行的方式，以摆脱 `RNN` 的自回归序列处理的限制。如下图所示，RetNet 放弃了 `softmax` 操作，转而使用 `Hadamard 积`，引入了新引入的 `D 矩阵`，然后是 `GroupNorm` 操作。那么弄清楚为什么 `D 矩阵` + `GroupNorm` 可以替代 `softmax` 就成为了问题的关键，可以从两个方面来理解：

+ 权重的加权操作。`softmax`的重要作用就是以不同的方式对不同的时间步长进行加权，相应地，`D 矩阵`是一个因果掩码，内置了定义的预定义加权因子，以实现类似的功能。`D 矩阵`假设最近的时间步长比过去的时间步长呈指数级增长，这种固定的预定义方式（指数衰减）虽然不如 `softmax` 灵活，但好处是 时间复杂度和 的内存复杂度。
+ 引入非线性。如果没有`softmax`， 操作只是一个仿射变换。而 `GroupNorm` 操作引入了急需的非线性。

![](img/450e3dfde814dd7a03d87332455ded78.png)

由于 RetNet 既在循环范式中运行，也在并行化范式中运行，因此作者首先在循环范式中计算 RetNet 的 `retention` 块（即单独处理每 `n` 个输入元素）。然后将他们提出的循环保留块矢量化。因此，初始循环公式如下所示：

其中 `pos` 表示位置(`pos`)矩阵（该表示熟悉 `RoPE` 的同学应该非常眼熟了）。RetNet 用 `pos` 矩阵替换了原始 `Transformer` 的 `softmax`。把 吸收进 ，则上面的等式可以扩展如下：

将  作为标量值，并对第二项进行共轭表示，上式可以写作

，其中  表示共轭转置。由此我们便可以写出并行表示的公式：

整体来看，与 `Transformer` 的不同点在于 `pos` 矩阵 （及其共轭矩阵 ）和 `D 矩阵`。熟悉旋转位置编码都很清楚 表示空间的位置旋转，`D 矩阵`充当因果掩码并且指数衰减(可参考 `alibi` 方案的线性衰减)。可以通过`Hadamard 积`将位置感知和远程衰减结合起来，如下图所示：

![](img/32cf9f2b03fc9dc232ad773c039e5a74.png)

下面通过一个小例子来感受这个过程，假设有 `Q`, `K`, `V` 矩阵如下

首先计算

然后计算与 `D 矩阵`之间的 `Hadamard 积`

最后乘以 `V`

这样我们就完成了并行训练的主要步骤，在后边我们将会发现其与`循环表示`的神秘而巧妙的等价联结。

#### 3.2.2 用于推理的循环表示

RetNet 的循环范式是通过解构并行计算获得的，如下图右所示，这样递归地进行计算，且计算成本及复杂度非常低。`循环表示`的公式如下：

可以轻易发现这里的不同之处：第一步是 `K^{\top} V` 而不是 `QK^{\top}` ，最后才将更新后的状态向量与 `Q` 相乘，得到此步骤的最终输出。

![](img/16d06ce52ce05fc21781a827d1871374.png)

那么通过上一小节的例子来计算这一过程，感受其具体过程

首先计算 （`n = 1`）该计算实际上是 2 个向量之间的外积

然后获取 `S1`。由于没有 `S0`，因此 `S1` 与上一步相同

最后将 和 相乘以获得最终输出，这里参考论文中的实现

![](img/330b1d035016a463c0f76dbc2ea84643.png)

这个结果竟然跟并行模式的结果完全一样，真的是非常巧妙！让我们接着进行愉快的计算

结果仍然可以对应！这说明 RetNet 能够将并行训练计算解构为完全重复的计算，而在`token by token` 的推理过程中，这种方式非常适合且有效。

最后总结一下，RetNet 能够如此高效，其代价是什么呢？答案可能是牺牲了 `attention` 的灵活性，即采用确定的方式来计算 `attention` 以获得与 `RNN` 的兼容。

### 3.3 状态空间模型及其发展 —— Mamba

由于篇幅所限，且该部分自成体系，因此单独成章，详情参见`https://zhuanlan.zhihu.com/p/680833040`

[  ](https://zhuanlan.zhihu.com/p/680833040)

汪国真在他的著名诗篇《旅程》里写道：“ 从别人那里，我们认识了自己 ”。今天，我想通过对诸多 `Transformer` 的改进型和非 `Transformer` 的模型的讨论和分析，试图帮助大家从更多不同的角度，更加深刻认识和理解 `Transformer`，也希望能够为我们的学习、工作和研究提供些许的启发和思考。

## 参考资料

[1] Attention Is All You Need

[2] [https://medium.com/@amanatulla1606/transformer-architecture-explained-2c49e2257b4c](https://link.zhihu.com/?target=https%3A//medium.com/%40amanatulla1606/transformer-architecture-explained-2c49e2257b4c)

[3] [https://arxiv.org/pdf/2204.05832.pdf](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2204.05832.pdf)

[4] [[2005.00743] Synthesizer: Rethinking Self-Attention in Transformer Models (arxiv.org)](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2005.00743)

[5] [线性Attention的探索：Attention必须有个Softmax吗？ - 科学空间|Scientific Spaces (kexue.fm)](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/7546)

[6] [https://generativeai.pub/explainable-ai-visualizing-attention-in-transformers-4eb931a2c0f8](https://link.zhihu.com/?target=https%3A//generativeai.pub/explainable-ai-visualizing-attention-in-transformers-4eb931a2c0f8)

[7] [https://towardsdatascience.com/all-you-need-to-know-about-attention-and-transformers-in-depth-understanding-part-1-552f0b41d021#fb36](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/all-you-need-to-know-about-attention-and-transformers-in-depth-understanding-part-1-552f0b41d021%23fb36)

[8] [Why multi-head self attention works: math, intuitions and 10+1 hidden insights | AI Summer](https://link.zhihu.com/?target=https%3A//theaisummer.com/self-attention/)

[9] [https://arxiv.org/pdf/2106.09650.pdf](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2106.09650.pdf)

[10] [Performer：用随机投影将Attention的复杂度线性化 - 科学空间|Scientific Spaces (kexue.fm)](https://link.zhihu.com/?target=https%3A//kexue.fm/archives/7921)

[11] [https://github.com/pjlab-sys4nlp/llama-moe](https://link.zhihu.com/?target=https%3A//github.com/pjlab-sys4nlp/llama-moe)

[12] 听我说，`Transformer`它就是个支持向量机

[13] [`Transformers` are Graph Neural Networks | NTU Graph Deep Learning Lab](https://link.zhihu.com/?target=https%3A//graphdeeplearning.github.io/post/transformers-are-gnns/)

[14] [如何理解 `Transformers` 中 `FFNs` 的作用？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/622085869)

[15] [(4 条消息) 有哪些令你印象深刻的魔改`transformer`？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/349958732)

[16] [https://medium.com/gopenai/mixture-of-experts-moe-in-ai-models-explained-2163335eaf85](https://link.zhihu.com/?target=https%3A//medium.com/gopenai/mixture-of-experts-moe-in-ai-models-explained-2163335eaf85)

[17] DeepSeekMoE：基于细粒度专家分割、共享专家的新MOE架构模型

[18] [https://medium.com/ai-fusion-labs/retentive-networks-retnet-explained-the-much-awaited-transformers-killer-is-here-6c17e3e8add8](https://link.zhihu.com/?target=https%3A//medium.com/ai-fusion-labs/retentive-networks-retnet-explained-the-much-awaited-transformers-killer-is-here-6c17e3e8add8)

[19] [一文通透想颠覆`Transformer`的`Mamba`：从`SSM`、`S4`到`mamba`、线性`transformer`(含`RWKV`解析)_`mamba`模型-CSDN博客](https://link.zhihu.com/?target=https%3A//blog.csdn.net/v_JULY_v/article/details/134923301)

[20] [`RWKV`：`Transformer`时代的`RNN`模型 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/656136892)

> 寂寞离亭掩，江山此夜寒。 —— 王勃《江亭夜月送别》
