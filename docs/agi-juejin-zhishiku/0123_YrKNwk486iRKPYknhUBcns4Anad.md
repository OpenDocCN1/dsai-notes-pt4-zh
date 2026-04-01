# 新PyTorch API：几行代码实现不同注意力变体，兼具FlashAttention性能和PyTorch灵活性

> 来源：[https://agijuejin.feishu.cn/docx/TGEmdtRDdo0XO1x8YePcSKSmnog](https://agijuejin.feishu.cn/docx/TGEmdtRDdo0XO1x8YePcSKSmnog)

> 机器之心｜[阅读原文](https://www.jiqizhixin.com/articles/2024-08-11-3)

> 转载请联系原作者取得授权

> 用 FlexAttention 尝试一种新的注意力模式。

理论上，注意力机制就是你所需要的一切。然而在实际操作中，我们还需要优化像 FlashAttention 这样的注意力机制的实现。

尽管这些融合的注意力机制大大提高了性能，且支持长上下文，但这种效率的提升也伴随着灵活性的丧失。对于机器学习研究人员来说，这就像是一种「软件彩票」—— 如果你的注意力变体不适合现有的优化内核，你将面临运行缓慢和 CUDA 内存不足的困境。

一些注意力变体包括因果注意力、相对位置嵌入、Alibi、滑动窗口注意力、PrefixLM、文档掩码、不规则张量、PagedAttention 等。更糟糕的是，人们通常希望将这些变体组合在一起！比如滑动窗口注意力 + 文档掩码 + 因果注意力 + 上下文并行，又比如 PagedAttention + 滑动窗口的组合。

下图左侧代表了当今的现状 —— 一些掩码 + 偏置 + 设置的组合已经有现成的内核实现。然而，各种选项的添加会导致设置呈指数级增长。更糟糕的是，这种方式不会支持新的注意力变体。

![](img/e3c5b7cea87e8e287d3bbf31979a4906.png)

为了彻底地解决这个超立方体问题，PyTorch 团队引入了 FlexAttention，一个新的 PyTorch API。

1.  FlexAttention 是一个灵活的 API，允许用户使用几行惯用的 PyTorch 代码就能实现多个注意力变体。

1.  团队人员通过 torch.compile 将其降低到一个融合的 FlashAttention 内核中 ，生成了一个不会占用额外内存且性能可与手写内核相媲美的 FlashAttention 内核。

1.  利用 PyTorch 的自动求导机制自动生成反向传播。

1.  最后，PyTorch 团队还可以利用注意力掩码中的稀疏性，从而显著改善标准注意力实现。

![](img/f5302482ec4af7c0daa2f9c542053a6e.png)

FlashAttention 1-3 版本的参与者 Tri Dao 对这项研究进行了转发并评论：这项研究使得很多技术都融合在一起了。

![](img/9ae1a3e2b46cd4c215660c15bdb5e54a.png)

**FlexAttention**

经典的注意力方程式如下：

![](img/f51ec69430bbe351cc8eefea56d966ed.png)

代码形式：

![](img/554971d7000caa68fcf10e0923b26c5b.png)

FlexAttention 形式如下，其通过接受用户定义的函数 score_mod 来解决上述问题。

![](img/5e79bbcd055cf2e4016b79e6bcd90c44.png)

代码形式：

![](img/7bd8fb7a80849e1a182b98752c0d4597.png)

此函数允许用户在 softmax 之前修改注意力分数。研究人员发现，该函数最终足以满足大多数用户对注意力变体的需求。

具体而言，score_mod 如下：

![](img/8643a68a3897760252ce69b9ce149719.png)

要应用此函数，可以将其实现为：

```
for b in range (batch_size):
    for h in range (num_heads):
        for q_idx in range (sequence_length):
            for kv_idx in range (sequence_length):
                modified_scores [b, h, q_idx, kv_idx] = score_mod (scores [b, h, q_idx, kv_idx], b, h, q_idx, kv_idx)
```

最终的 API 具有令人惊讶的表达能力。

**Score Mod 示例**

**全注意力**

在这种情况下，score_mod 无操作，它接受分数作为输入，然后原样返回它们。

![](img/cb5076bb07a6d2c1fb02a9236ed96ece.png)

然后端到端的使用。

![](img/9a8419c294368f4fe76a7646d5bf88c7.png)

**相对位置编码**

一种常见的注意力变体是相对位置编码。相对位置编码不是对查询和键中的绝对距离进行编码，而是根据查询和键之间的距离调整分数。

![](img/382496026b7b378528e3c2f1989e5459.png)

需要注意的是，与典型实现不同，这不需要具体化 SxS 张量。相反，FlexAttention 会在内核中动态计算偏差值，从而显著提高内存和性能。

![](img/74335c7b8680511690512b5c3e15181a.png)

**Soft-capping**

Soft-capping 是 Gemma 2 和 Grok-1 使用的一种技术，在 FlexAttention 中，它的形式是这样的：

![](img/f6dfaaf7266a7158a826c722802fb4ad.png)

**Causal Mask**

尽管双向注意力很简单，但在论文《Attention is All You Need》，以及其他的 LLM 中，它们的设置都是仅解码器的注意力，其中每个 token 只能关注它之前的 token。如果用户使用 score_mod API ，可以将其表示为：

![](img/e991acbd5d47891553db58a319594a63.png)

Sliding Window + Causal

![](img/0ccd23a85865c84672e5dd481e453133.png)

图源：https://arxiv.org/abs/2310.06825

Mistral 一直在推广滑动窗口注意力（也称为局部注意力），它允许查询 token 仅关注最近的 1024 个 token，通常与因果注意力一起使用。

![](img/a683cd243c3166f65d684fe26c67f6a4.png)

研究者对带有滑动窗口掩码的 F.scaled_dot_product_attention 以及带有因果掩码的 FA2 进行基准测试。结果表明，FlexAttention 不仅明显快于 F.scaled_dot_product_attention，也明显快于带有因果掩码的 FA2。

![](img/20c344c035f5dac830d725b99049ed3f.png)

**性能**

总体而言，FlexAttention 的性能几乎与手写的 Triton 内核一样好。然而，由于 FlexAttention 具有通用性，因此会遭受轻微的性能损失。例如，用户必须承受一些额外的延迟。

FlexAttention 在前向传播中实现了 FlashAttention2 性能的 90%，在反向传播中实现了 85%。FlexAttention 目前正在使用一种确定性算法，该算法比 FAv2 重新计算了更多的中间体，研究者计划改进 FlexAttention 的反向算法，来缩小这一差距！

![](img/68686dd87d3eec061a04b2515a706e81.png)

![](img/d0b790e6548510ea94e2d7596c5e80f2.png)

参考链接：https://pytorch.org/blog/flexattention/