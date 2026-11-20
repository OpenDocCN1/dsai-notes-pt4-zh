
# 谷歌爆改Transformer，“无限注意力”让1B小模型读完10部小说

> 来源：[https://agijuejin.feishu.cn/docx/SQkSd8Ielo30cixAQKmckDtqngg](https://agijuejin.feishu.cn/docx/SQkSd8Ielo30cixAQKmckDtqngg)

> 明敏 发自 凹非寺

> 量子位 | 👸号 QbitAI

谷歌大改 `Transformer`，“无限”长度上下文来了。

现在，1B 大模型上下文长度可扩展到 1M（100 万 token，大约相当于 10 部小说），并能完成 `Passkey` 检索任务。

8B 大模型在 `500K 上下文长度` 的书籍摘要任务中，拿下最新 SOTA。

这就是谷歌最新提出的 `Infini-attention` 机制（无限注意力）。

![](img/50b702da3ecbc41cd06d82ba55d96050.png)

它能让 `Transformer` 架构大模型在有限的计算资源里处理无限长的输入，在内存大小上实现 114 倍压缩比。

什么概念？

就是在内存大小不变的情况下，放进去 114 倍多的信息。好比一个存放 100 本书的图书馆，通过新技术能存储 11400 本书了。

这项最新成果立马引发学术圈关注，大佬纷纷围观。

![](img/76df3c45afca23a65335ec3642876b26.png)

加之最近 DeepMind 也改进了 `Transformer` 架构，使其可以动态分配计算资源，以此提高训练效率。

有人感慨，基于最近几个新进展，感觉大模型越来越像一个包含高度可替换、商品化组件的软件栈了。

![](img/7c820e4c3bd336e3f0ff598426748ef1.png)

## 核心机制：Infini-attention

该论文核心提出了一种新机制 `Infini-attention`。

它通过将压缩记忆（compressive memory）整合到线性注意力机制中，用来处理无限长上下文。

压缩记忆允许模型在处理新输入时保留和重用之前的上下文信息。它通过固定数量的参数来存储和回忆信息，而不是随着输入序列长度的增加而增加参数量，能减少内存占用和计算成本。

线性注意力机制不同于传统 `Transformer` 中的二次方复杂度注意力机制，它能通过更小的计算开销来检索和更新长期记忆。

在 `Infini-attention` 中，旧的 KV 状态（`{KV}s-1`）被存储在压缩记忆中，而不是被丢弃。

通过将查询与压缩记忆中存储的键值进行匹配，模型就可以检索到相关的值。

`PE` 表示位置嵌入，用于给模型提供序列中元素的位置信息。

![](img/a8bd6778212f6c4d5729f887c48c2cbd.png)

对比来看 `Transformer-XL`，它只缓存最后一段 KV 状态，在处理新的序列段时就会丢弃旧的键值对，所以它只能保留最近一段的上下文信息。

![](img/f46e995789ae693f3fb99a89381cd56f.png)

对比几种不同 `Transformer` 模型可处理上下文的长度和内存占用情况。

`Infini-attention` 能在内存占用低的情况下，有效处理非常长的序列。

![](img/d907063496ad7303f3b4e9395329a220.png)

`Infini-attention` 在训练后，分化出了两种不同类型的注意力头，它们协同处理长期和短期上下文信息。

+ 专门化的头（Specialized heads）：这些头在训练过程中学习到了特定的功能，它们的门控得分（`gating score`）接近 0 或 1。这意味着它们要么通过局部注意力机制处理当前的上下文信息，要么从压缩记忆中检索信息。

+ 混合头（Mixer heads）：这些头的门控得分（`gating score`）接近 0.5，它们的作用是将当前的上下文信息和长期记忆内容聚合到单一的输出中。

![](img/53c0ac604bda27d7d9419c4eb02deef9.png)

## 实验与性能对比

研究团队将训练长度增加到 100K，在 `Arxiv-math` 数据集上进行训练。

在长下文语言建模任务中，`Infini-attention` 在保持低内存占用的同时，困惑度更低。

对比来看，同样情况下 `Memorizing Transformer` 存储参数所需的内存是 `Infini-attention` 的 114 倍。

消融实验比较了“线性”和“线性 + 增量”记忆两种模式，结果显示性能相当。

![](img/97e6cd532a3ead1b2ed147df7b2d2d70.png)

实验结果显示，即使在输入只有 5K 进行微调的情况下，`Infini-Transformer` 可成功搞定 1M 长度（100 万）的 `Passkey` 检索任务。

![](img/a5690332f0668ef032be058131c8eeda.png)

在处理长达 500K 长度的书籍摘要任务时，`Infini-Transformer` 达到最新 SOTA。

![](img/f0514038d4753b8aced8fe286bbbd5d9.png)

## 研究团队与相关工作

该研究由谷歌团队带来。

其中一位作者（Manaal Faruqui）在 Bard 团队，领导研究 Bard 的模型质量、指令遵循等问题。

![](img/bcb387977a6ab5afff98949263d0ea22.png)

最近，DeepMind 的一项工作也关注到了高效处理长序列数据上。他们提出了两个新的 `RNN` 模型，在高效处理长序列时还实现了和 `Transformer` 模型相当的性能和效率。

![](img/22eab026d0e74f1f0bccb3eb1f64f3d1.png)

感觉到谷歌最近的研究重点之一就是长文本，论文在陆续公布。

网友觉得，很难了解哪些是真正开始推行使用的，哪些只是一些研究员心血来潮的成果。

> 不过想象一下，如果有一些初创公司专门做内存数据库，但是模型能已经能实现无限内存了，这可真是太有趣了。

![](img/2d9b78a89829a11b0d118bfe0ab3424c.png)

## 论文与参考链接

论文地址：

`arxiv.org/abs/2404.07…`

参考链接：

[1] `twitter.com/Joby_Fi/sta…`

[2] `twitter.com/omarsar0/st…`

[3] `twitter.com/swyx/status…`
