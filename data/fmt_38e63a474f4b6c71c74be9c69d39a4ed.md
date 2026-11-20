

# LLM在Pretrain时如何做好拼接

> 来源：`https://agijuejin.feishu.cn/docx/UQEQdryrtoYFYGxzzYpcbSehngQ`

> 作者：咸鱼王

> 原文链接：`https://zhuanlan.zhihu.com/p/676647785`

为了提高`pretrain`效率、拓展`LLM`最大长度，随机将若干条短文本进行拼接是`pretrain`阶段常见手段，但这种"强扭的瓜"究竟是否合理？

![](img/cc8341bc247e1f3de3bf79ddc9f39141.png)

论文链接：

[IN-CONTEXT PRETRAINING: LANGUAGE MODELING BEYOND DOCUMENT BOUNDARIES](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2310.10638.pdf%25EF%25BC%2589)

## 01 背景

由于`pretrain`过高的实验成本，延用成功案例的训练策略无可厚非，但是这些策略是否最优仍未可知。该篇论文从`pretrain`阶段短文本拼接方案为切入点，尝试给出一种更加合理的拼接策略。同时，笔者尝试以本篇论文为主干，结合相关文章和笔者的个人见解，对这些拼接策略做进一步分析。

## 02 一些常见的拼接方案

### 2.1 Random Concatenate

随机将短文本 `{examples_{i \in D}}` 拼接成 `{Examples_{k \in D}}` 以打满 `maxLen` 是 `pretrain` 的常见手段，该方法不仅能够降低 `padding` 占比、提高训练效率，还能使 `LLM` 具备更好的长文本处理能力。

但笔者认为，绝大多数情况下构成 `Example Example` 的多个 `examples examples` 彼此互不相关，无法提供有效的上下文信息，`LLM`自然也无法从拓宽的窗口中获得反馈。甚至，在语料较少、分布比较集中时，`LLM`很有可能从多次、偶然的（因拼接导致的）噪音共现中拟合到错误的特征。当然，如果语料足够多、分布足够广，`LLM`仍能通过足够的 `contrastive`，逐渐聚焦于 `example example` 本身而非其他无关 `examples examples`。此外，也有一些使用 `specialToken` 对 `examples examples` 进行软隔离的方案，但没有额外的正则手段时，使用 `specialToken` 进行隔离或许只是鸡生蛋、蛋生鸡的死循环。

### 2.2 Random Concatenate + NoiseMask

为缓解2.1所述的无关 `examples` 间的噪音共现问题，笔者尝试过添加自定义 `attentionMask`，使 `LLM` 在 `pretrain` 时仅 focus on 当前 `example`，经笔者测试，该方法在 `ICL few-shot` 上相比2.1（也即常规 `pretrain` 方法）有1.6%左右的提升。

```
def segment_causal_mask(input_ids, device, val=float(&quot;-inf&quot;)):
    bsz, tgt_len = input_ids.shape
    cum_lens = torch.arange(1, tgt_len+1, device=device).unsqueeze(0) * \
               torch.eq(input_ids, EosTokenId).int().to(device)

    mask = torch.zeros([bsz, tgt_len, tgt_len]).to(device)
    for i, _cum_lens in enumerate(cum_lens):
        for v in _cum_lens:
            mask[i,v:,:v] = val

    return mask
```

但这种方式仍存在一个问题，相对位置编码（如 `ALIBI`、`ROPE`）的 token-wise 相对位置信息会在 `attentionScore` 矩阵对应位置有所体现，如果施加了 `attentionMask`，这部分相对位置信息经过 `softmax` 会被完全掩盖/误杀，也即 `LLM` 无法在 `BP` 过程中，从跨 `examples` 间获得反馈（不论是相对位置的反馈还是语义信息的反馈）。因此在不考虑外推性的前提下，这种 `pretrain` 方法仍是在短文本窗口内进行训练，没有真正意义上实现 `maxLen` 级别的长文本训练，只能起到提高训练效率的作用。

另外，尽管2.1中没有利用 `attentionMask`，`LLM` 是否能从无关 `examples` 构成的窗口中获取对（更远）相对位置的正向反馈仍然存疑（如果数据构成表现为远的都不相关，即便没有 mask，`LLM` 也倾向于忽略更远的 `tokens`），或许这也是多数 `LLM` 在拓宽 `maxLen` 之后，长文本支持效果仍然差强人意的原因之一。

### 2.3 Random Concatenate + Cluster

鉴于2.2存在的问题，能否既不施加 `attentionMask`，也能让 `LLM` 不受跨 `examples` 干扰甚至还能获益的方法呢？一个直观的想法就是 以实体、语义等维度对 `{examples_{i \in D}}` 进行聚类，使构成同一个 `Example` 的 `examples` 存在 真实可靠的信息共现前提 ，从而 `LLM` 更加不容易从噪音共现中学偏，也能从 `pretrain` 中适应更加广泛全局的 `attention`，从而具备更好的长文本处理能力。

笔者曾经尝试沿实体维度进行聚类，但发现了一个比较棘手的问题：信息重复，以及经过关键词、语义去重后仍难以避免的 信息泄露（或者说二者本质相同，只是程度的分别）。在此情况下 `LLM` 有 从 `memorize` 变成了 `copy` 的风险，或许这就是后来实验结论没有显著的原因。

本文作者提出的 `ICLM` 实际上也是一种类似的方法，即基于语义对 `examples` 进行聚合、拼接，因此笔者开始时也十分好奇作者如何妥善处理泄露问题。

### 2.4 IN-CONTEXT PRETRAINING

作者在文中提出的 `pretrain` 方法，基本思想是在拼接时，利用语义相似度，优先将最相似的 `{examples_{i \in D}}` 进行拼接，从而构成语义更加连贯流畅的上下文，基本流程如下图：

1. 将 `{examples_{i \in D}}` embedding化（作者使用了 `contriever`）；
1. 基于余弦距离进行数据去重；
1. 基于旅行商思想，不断串联最相关的 `example`（每个 `example` 用完即扔，不会 `repeat`）；
1. 基于拼接后的 `{Examples_{k \in D}}` 进行 `pretrain`；

![](img/dd49d5e58989c1c43e476f1996ae5110.png)

作者在文中多次强调了数据去重的重要性，并经过消融实验验证了去重对 `ICLM` 的正向增益。相比实体，沿语义聚合的 `{examples_{i \in D}}` 分布更加平缓，受泄露影响的风险更低；此外，分布更广泛的数据、更妥善的去重操作，或许也是 `ICLM` 能够有效的重要原因。

## 03 实验

### 3.1 实验设置

基于 `LLAMA` + `CommonCrawl`（306B tokens）进行预训练，`maxLen` 为 8192，`batchSize` 为 400 万 tokens。对比方法：

1. `Standard`：常规 `pretrain` 拼接方法，随机选取若干个 `examples` 进行拼接；
1. `kNN`：对于每个 `example`，选取 top-K 个 `examples` 进行拼接，可能会有重复（注意这种 `repeat` 不同于一般的过采样或多轮训练，一般是 不均匀 的，更易导致过拟合）；
1. `ICLM`：即本文提出的方法。

### 3.2 实验结果

作者首先评测了 3 种 `pretrain` model 在 `wiki`、`arxiv`、`Books` 这三种高质量 `corpus` 下的 `PPL` ，如下图所示。结论可以说是非常明了，在不同 `corpus`、在不同参量下，`ICLM` 方法始终优于 `Standard` 始终优于 `kNN`。

![](img/a26282a8cd92be3780968f9f33e3fd4b.png)

作者又基于 `32-shot`，测试了不同 model 的情感分类、主题分类、仇恨言论检测能力，如下表所示，结论与 PPL 实验基本一致，`ICLM` 的 `In-context learning` 分类效果 显著优于 `Standard` 方法（+8%），`kNN` 仍是最差。

![](img/f9fe990a7e4c1da51a19fd0b188442ab.png)

阅读理解上的实验结论也基本相同，如下表，`ICLM` 在 阅读理解 的 `2-shot` 测试上和 `Standard` 拉开了更大的差距（+14%），可以看出 `In-context pretraining` 对 `LLM` 的理解和推理能力有不小的正向增益。

![](img/95c85baa75da7f7ef9aaf8061608065a.png)

作者又基于 `NQ` 和 `TQA` 分别进行了 `close/open` 测试，前者主要评测 `LLM` 的 世界知识，后者则侧重 `RAG` 能力。在 `closed` 测试中，`ICLM` 整体略低于 `Standard` 方法，这似乎也可以解释：即便作者利用余弦距离对 `examples` 进行去重，仍无法避免邻近 `examples` 中存在部分 `repeat` 或信息泄露，因此在 `pretrain` 阶段，`LLM` 存在 `copy` 而非 `memory` 的风险，进而导致 `ICLM` 的知识记忆效果相对较差。在 `open` 也即 `RAG` 测试中，`ICLM` 则再次表现出了明显优势（+9%）。另外，尽管 `kNN` 的 `pretrain` 方法形似 `RAG`，仍未取得更好的结果，原因大概还是过拟合。

![](img/c8ed23bbc5039e59ca3cf3b1b24e2c64.png)

另外，为测试 `LLM` 遵循指令 & `reference` 的能力（尤其在 `RAG` 下很有意义），作者在 `NQ-Swap` 和 `MemoTrap` 进行了测试（该测试集中 `instruction` / `reference` 基本都与 `LLM` 知识存在冲突），具体结果如下表，`ICLM` 展现了更大的优势，也即 更倾向于依赖 `context` 进行生成（更遵循 `instruction`、更忠实 `reference`）。

![](img/b9f6e59d68bf8ff44da681635a339cd6.png)

最后也是最值得关心的一点，作者基于 `SCROLL` 进行了长文本推理测试，以对比几种方法的 长文本支持效果，具体如下表，`ICLM` 相比 `Standard` 方式在不同数据集上都普遍存在增益（+5%）。

![](img/ce232e69396811b4b5835dc2b29dbbd8.png)

## 04 结论

该篇文章给出了沿语义维度进行短文本拼接的 `pretrain` 策略，从论文作者给出的实验来看，可以说实验维度非常全面，实验结果大获全胜。

笔者认为，该方法 从真正意义上拓宽了 `LLM` 的 `maxLen`、`attentionWindow`：相比普通 `pretrain` 方法，`ICLM` 能从长文本窗口获得更高受益，（尤其对更远的 `token`）更加 `concentrated`，也具备更好的长文本理解能力、`instruction following` 和 `reference dependent` 能力。

但是 `ICLM` 也存在 `copy` 而非 `memorize` 的风险，或许在知识记忆上有一定劣势，尤其在没有妥善处理好信息重复或泄露问题时会更加严重；此外对于 `continue pretrain` 的场景，如果语料较少、分布比较集中，拼接时会以更高概率出现泄露问题，需要更加妥善处理。

