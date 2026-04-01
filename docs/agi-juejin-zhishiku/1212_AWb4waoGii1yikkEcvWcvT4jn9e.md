# Transformer 基本原理：逐步拆解和剖析

> 来源：[https://agijuejin.feishu.cn/docx/Uk1cdMVu6osGqsxQ5TvcSRzQnQh](https://agijuejin.feishu.cn/docx/Uk1cdMVu6osGqsxQ5TvcSRzQnQh)

> 作者：王几行XING

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/677202804)hihu.com/p/677202...

（本文是以下博文的学习笔记： [h](https://link.zhihu.com/?target=https%3A//jalammar.github.io/illustrated-transformer/)ttps:// jalammar.github.io/illu strated-transformer/  ）

学完发现，大神有几个问题没有说清楚：

+   [绝对位置信息 ](https://www.zhihu.com/search?q=%E7%BB%9D%E5%AF%B9%E4%BD%8D%E7%BD%AE%E4%BF%A1%E6%81%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)如何计算，即生成位置向量过程的细节。（代入公式求出）

+   [位置编码 ](https://www.zhihu.com/search?q=%E4%BD%8D%E7%BD%AE%E7%BC%96%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)[，如何纳入](https://www.zhihu.com/search?q=%E5%92%8C%E5%B7%AE%E5%8C%96%E7%A7%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)相对位置信息。（其实靠的是三角函数的 和差化积 ，位置之和，比如可以拆开成 sin*cos+cos*sin）

+   第一个 decoder 的第一个input 来自哪里？

+   Encoders 向 Decoders 输送的 K 和 V 矩阵 如何生成？

## 模型的整体功能

![](img/142593d7c2978b475208f2bec7a2e8d0.png)

首先，对于 Transformer，我们可以把它当成一个具有翻译功能的黑箱（Transformer的发明初衷是机器翻译，MT），左边一句法语进去，右边一句英文出来。

![](img/0f4b5ba5c2ba7c965bf0cda9deb91c90.png)

## 模型的两大模块

具体一点来看，这个 [黑箱 ](https://www.zhihu.com/search?q=%E9%BB%91%E7%AE%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)由两大模块组成：

+   Encoder

+   Decoder

```
提前透露一下目前主流 LLMs 的架构：

- 单独由 Encoder 构成的LLMs 代表是 BERT
- 单独由 Decoder 构成的LLMs 代表是 GPT 系列，此结构目前也是大模型的主流
- 其它 LLMs 则由 Encoder + Decoder 组合而成，比如 T5
```

进一步地，Encoder 和 Decoder 分别由 6 个一模一样的encoder 或 decoder 模块重复堆叠而成。这个数字 6，是《 Attention is All You Need 》中的安排，现实中换成其它数字的实践也很常见。

![](img/466b66ed8b4c22d86b95822e46265310.png)

每个 Encoder 模块的结构是一模一样的，虽然它们之间不共享参数（参数不同）。并且，每个 Encoder 可以分成两个子模块：

+   Multi-head Self-Attention，多头自注意力

+   [Feed F](https://www.zhihu.com/search?q=%E5%89%8D%E9%A6%88%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)orward Network，FFN， 前馈神经网络

![](img/ae76c6407244fda1b63c3d57fa7101c1.png)

### **Encoder 编码器的输入**

编码器的输入，首先流经一个 [自注意力层 ](https://www.zhihu.com/search?q=%E8%87%AA%E6%B3%A8%E6%84%8F%E5%8A%9B%E5%B1%82&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)——这个 **自注意力层帮助编码器在编码一个特定单词时，考**[**虑输入句子**](https://www.zhihu.com/search?q=%E8%87%AA%E6%B3%A8%E6%84%8F%E5%8A%9B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)**中的**其他单词（这就是所谓的“ 自注意力 ” ）。

[自注意力层](https://www.zhihu.com/search?q=%E5%89%8D%E9%A6%88%E7%BD%91%E7%BB%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)的输出被送入一个前馈神经网络。完全相同的 前馈网络 ，独立地应用于每一个位置。

![](img/f886a12d91ce96739f9360106ab7ea8d.png)

### **Decoder 解码器**

[**解码器也拥有**](https://www.zhihu.com/search?q=%E4%BA%A4%E5%8F%89%E6%B3%A8%E6%84%8F%E5%8A%9B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)自注意力层+FFN，但在它们之间有一个注意力层（ Encoder-Decoder Attention，也叫 交叉注意力 ），帮助解码器专注于输入句子的相关部分

[这种工作模式](https://www.zhihu.com/search?q=%E6%B3%A8%E6%84%8F%E5%8A%9B%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)，和 seq2seq 中的 注意力机制 类似（Transformer 并不是第一个使用注意力机制的模型）。

![](img/4b2a333a35f4db266184a27f6795e2e0.png)

## 词向量

单词作为模型的输入，在输入之前，我们需要通过 embedding 技术，将每个单词，转换成为一个向量。比如下图中，Je 转换成了向量 x1，一共有4维。现实中，不同的模型使用不同的维度。

![](img/e313b4bfbef7b192f057500f205fa318.png)

想象一下，如果使用one-hot，每个单词可能需要表现为上万维的维度，而 em[beddi](https://www.zhihu.com/search?q=512%E7%BB%B4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)[ng 技](https://www.zhihu.com/search?q=%E8%AF%8D%E5%90%91%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)术或者神经网络，可以将这个高维的向量压缩，压缩至几百维，比如 512维 。下图中，我们将 4 纬的原始表示，转换为三维的 词向量 ，且两者的转换可逆。

![](img/8b856357113866e7291b1cf75c87c8bf.png)

## Encoder 与词嵌

嵌入仅发生在最底层的编码器中。

所有编码器的共同抽象是， **它们接收一个向量列表 **[，每个向](https://www.zhihu.com/search?q=%E8%AF%8D%E5%B5%8C%E5%85%A5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)[**量的大小**](https://www.zhihu.com/search?q=%E8%B6%85%E5%8F%82%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)为512——在最底层的编码器中，这**些**将是 词嵌入 ，但在其他编码器中，这将是直接在其下方的编码器的输出。 这个列表的大小是我们可以设置的 超参数 ——基本上，它会是我们训练数据集中最长句子的长度（ context的长度？我们先不要考虑GPT生成模型，这里只探讨翻译模型） 。

![](img/865c0b26348332b488ab9e69dc0ec9d3.png)

在我们的输入序列中嵌入单词之后，每个单词都会[依次流经](https://www.zhihu.com/search?q=%E5%89%8D%E9%A6%88%E5%B1%82&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)编码器的两个层：自注意力层，和 前馈层 。

在这[**里我们开始**](https://www.zhihu.com/search?q=%E5%B9%B6%E8%A1%8C%E6%89%A7%E8%A1%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)看到 Transformer 的一个关键特性，即每个位置上的单词在编码器中通过其自己的路径流动。在自注意力层中，这些路径之间存在依赖关系。然而，前馈层并没有这些依赖关系。 因此，在流经前馈层时，不同的路径可以 并行执行 。

接下来，我们将例子换成一个更短的句子，观察在编码器的每个子层中发生了什么。

## Encoding [自编码](https://www.zhihu.com/search?q=%E8%87%AA%E7%BC%96%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)

编码器接收一系列向量作为输入，并传递到一个“自注意力”层，然后传递到一个前馈神经网络，然后将输出向上发送到下一个编码器，从而处理这个句子。

在这个过程中， **自注意力层是核心 **，它允许模型在处理每个输入的同时，考虑序**列中的其他所**有输入，这样可以更好地理解 上**下文**关系 （可见，自注意力=上下文关系 ） 。接着，前馈神经网络进一步处理这些信息，为下一个编码层提供加工后的输入。

这种结构允许编码器捕捉到 输入序列中的复杂模式和依赖关系 ，这对于理解和生成自然语言至关重要。

![](img/6ee99743b1293014deaa56036c87339c.png)

## [自注意力机制 ](https://www.zhihu.com/search?q=%E8%87%AA%E6%B3%A8%E6%84%8F%E5%8A%9B%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)：概览

自注意力一词，是在《 Attention is all you need 》中首次被提出。正如我们前面所提醒的那这样：自注意力=上下文关系。

假设下面这句话是我们想要翻译的输入句子：

”The animal didn't cross the street because it was too tired”

这句话中的“it”指的是什么？它[是指街道还是](https://www.zhihu.com/search?q=%E7%AE%80%E5%8D%95%E7%9A%84%E9%97%AE%E9%A2%98&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)动物？对于人类来说这是一个 简单的问题 ，但对算法来说就没有那么简单了。

**当模型处理单词“it”时，自注意力允许**它将“it”与“animal”联系起来 。

当[模型处理每个单](https://www.zhihu.com/search?q=%E5%BE%AA%E7%8E%AF%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)词（输入序列中的每个位置）时，自注意力允许它查看输入序列中的其他位置，寻找线索，这些线索可以帮助更好地编码这个单词。在 循环神经网络 RNN 中，隐藏层将之前处理过的单词/向量，与当前正在处理的那个结合起来。

自注意力是 Transformer 用来 将对其他相关单词的“理解”，融入到我们当前正在处理的那个单词中的方法。

![](img/cf8f66333883d5c0c3a42f1bf1617b1c.png)

## 自注意力机制：细节

我们先了解如何 **使用向量计算自注意力 **，然后再看看它是如何使用矩阵实现。

计算自注意力的第一步是从编码器的每个输入向**量（在这个案例**中，是每个单词的嵌入）中 创建三个向量 ：

+   查询（Query）向量

+   一个键（Key）向量

+   一个值（Value）向量

+   这些向量是通过将嵌入向量，乘以我们在训练过程中训练的三个矩阵来创建的

注意，这些新向量的维度比 [嵌入向量 ](https://www.zhihu.com/search?q=%E5%B5%8C%E5%85%A5%E5%90%91%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)小。它们的维度是 **64 ****，而嵌入**和编码器输入/输出向量的维度是 512 。它们不必非得更小，这是一个架构选择，为了使多头注意力的计算（大多数情况下）保持恒定。

![](img/d31143920d7ce05d953be4cc077e95e4.png)

[上图中，我](https://www.zhihu.com/search?q=%E6%9D%83%E9%87%8D%E7%9F%A9%E9%98%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)们将 x1 乘以 WQ 权重矩阵 产生 q1， 即与该单词相关联的“查询”向量 。

我们最终为输入句子中的每个 **单词 **，创建了一个 **“查询”、“键”和“值”投影 **。

## 什么是 q、k、v 向量？

它们是用于计算和思考注意力的 [抽象概念 ](https://www.zhihu.com/search?q=%E6%8A%BD%E8%B1%A1%E6%A6%82%E5%BF%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)。

计**算自注意力的**第二步是计算一个分数。比如我们正在为这个例子中的第一个单词“Thinking”计算自注意力。我们需要对输入句子中的 每个单词与这个单词进行打分（可以理解为关系的大小得分） 。分数决定了在我们编码特定位置的一个单词时，应该将多少注意力（ 注意力得分 ）放在输入句子的其他部分。

注意力得分，是通过取 [**查询向量 **](https://www.zhihu.com/search?q=%E6%9F%A5%E8%AF%A2%E5%90%91%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)与我们正在打分的相应单词的 **键向量的 **[**点积 **](https://www.zhihu.com/search?q=%E7%82%B9%E7%A7%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)来计算的。所以，如果我们正在处理位置＃1的单词的自注意力，第一个分数将是 q1 和 k1 的点积。第二个分数将是 q1 和 k2 的点积。

![](img/39e4c51fc49607359d2b192e1097d401.png)

**计算注意力得分第三和第四步， **是将上面的分数除以8，这是论文中使用的键向量维度的平方根——64。这导致[**梯度更加稳**](https://www.zhihu.com/search?q=%E5%8E%BB%E9%87%8F%E7%BA%B2%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)定，这里也可能有其他的可能值）。这个除以 8 的操作，类似于常见的“ 去量纲化 ”操作。

然后将结果通过一个softmax操作。Softmax将分数标准化，使它们都为正数并且加起来等于1。这个softmax分数决定了每个单词在这个位置将被表达的程度。显然，这个位置的单词将拥有最高的softmax分数，但有时关注与当前单词相关的另一个单词是有用的。

**第五步 ****是将每个值向量 v 乘以 softmax 分数。这里的直觉是 **保持我们想要关注的单词的值不变，并淹没掉不相关的单词（例如，通过将它们乘以像0.001这样的微小数字 ）。

这个就是 All You Need 论文中最经典的公式：

![](img/86e81898aad6dd4527ac508d1f2434a0.png)

![](img/e6f7fa4ea49422f5b89851759e155e8f.png)

**第六步 **是加总 [加权 ](https://www.zhihu.com/search?q=%E5%8A%A0%E6%9D%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)**后的值向量（得到 **z1），这产生了这个位置的 自注意力层的输出 。即对于第一个单词而言，得到 z1。

至此，自注意力计算完成。得到的向量z，是我们可以发送到前馈神经网络的向量。然而，在实际实现中，为了更快的处理速度，这个计算是以矩阵形式进行的。所以现在我们已经看到了单词层面上计算的直觉，接下来让我们看看矩阵运算。

## 注意力机制：矩阵运算

**第一步，通过输入矩阵 X 与权重矩阵 Wq/W**k/Wv 相乘，来计算 Q、K、V 矩阵

![](img/1f1488670f688ef195e8673d3a88b19b.png)

接下来，通过自注意力公式，完成其他运算：

![](img/2d14fb694be253eb8c2a058e97948ed4.png)

## 多头注意力

该论文进一步改进了自注意力层，增加了一种称为“多头”注意力的机制。这以两种方式提高了注意力层的性能：

+   **扩展了模型关注不同位置的能力 **。在上面的例子中， **z1 **包含了一点其他每个编码的内容， 但它可能被实际单词本身所主导。 如果我们在翻译像“这只动物没有过马路，因为它太累了”这样的句子时，知道“它”指的是哪个词会很有用。

+   **为注意力层提供了多个“**表示子空间” 。有了多头注意力，我们不仅仅有一组 查询/键/值权重矩阵 ，而是有多组（变换器使用八个注意力头，所以我们最终为每个编码器/解码器得到八组）。每一组都是随机初始化的。然后，在训练之后，每组都用于将输入的词向量（或来自较低编码器/解码器的向量）投射到不同的表示子空间中。

![](img/f4c1aa0852372478bb2d6e0daa36c5a2.png)

通过8个头，我们最终得到8批不同的 Wq/k/v 矩阵，以及8个不同 Z 矩阵 （Z 表示基于注意力得分，实现不同位置的关注情况 ）。

## 重点：Z 表示基于注意力得分，实现不同位置的关注情况。

![](img/935763f76a0ee693bb3ff708ae5fa137.png)

但是，前馈层无法接收八个矩阵。它期望接收的是，一个单一的矩阵（每个单词一个向量）。所以我们需要一种方法将这八个矩阵压缩成一个单一的矩阵（单个Z。X是2行3列，Z也是2行3列）。

做法其实很简单。我们将这些 [矩阵拼接 ](https://www.zhihu.com/search?q=%E7%9F%A9%E9%98%B5%E6%8B%BC%E6%8E%A5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)起来，然后乘上权重矩阵 Wo。

![](img/15a34b38acfb4be13a3df6f983e2f216.png)

这个就是所谓的多头-自注意力机制了。下图进行了一个简单的汇总。

![](img/1f5aed35a7a61fbc1af0e5d6e901a8cc.png)

+   第一步，两个单词转换为两个 [行向量 ](https://www.zhihu.com/search?q=%E8%A1%8C%E5%90%91%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)，构成了输入矩阵 X

+   第二步，八个头，每个头都在构造各自的Q/K/V 矩阵

+   第三步，每个头计算各自的 Z

+   第四步，把 Z0、Z1...Z7 一共8个 Z横向拼接起来，乘以 Wo，得到最终的 Z

![](img/15362a57e153f9178b1dd57b5da74c89.png)

上面，用不同颜色表示的8个头，都有各自不同的注意的单词。比如绿色的头，更注意的是单词 tire和d_。

## 位置编码与位置向量

一个句子作为一个序列，单词的前后顺序信息，本身包含着非常重要的信息。但是，目前的模型都没有考虑到。也就是说，即使顺序打乱，注意力的计算结果也是类似的。

为了解决这个问题，Transformer **为每个输入**嵌入添加了一个向量，也就是“ 位置向量 ”。这些向量遵循模型学习**到的特定模式，帮助模型确定每个单**词的位置，或者序列中不同单词之间的距离。这里的直觉是，将这些值添加到词嵌向量中，然后在 将嵌入向量投射到查询/键/值（Q/K/V）向量、以及在点积注意力的运算 的过程中，提供了词向量之间有意义的距离（绝对位置和相对位置的信息）。

![](img/b35acdaa5d671c2ff1f032d0a7f5f2af.png)

举例来说，加入我们的词向量是4维，那么我们得到的位置向量也是4维。通过位置向量的和输入词向量的叠加之后，我们的输入就拥有了位置的信息。

在论文中，Positional Encoding 通过下面的公式进行计算：

![](img/56af6bc26cb25444b18d4250dc3cfa52.png)

下图中，展示了10个单词（维度=65）的位置编码信息。颜色表示取值，从0-1（sin 和 cos 函数的取值）。

![](img/8754428cdbedac194ffe72d7478e4dd3.png)

## 残差连接 + normalization

在每个自注意力层和前馈层之后，我们都会加上一个残差层（来自ResNet 的类似操作），以及层归一化（应该来说应该叫层标准化）。

![](img/7fd08a858e63cfbea8b0be60f06f9696.png)

下图展示了更多的“残差连接”的细节：所谓残差连接，就是当前层的输入（X）也加回到输出层，变成（X+Z），然后再标准化。

加入我们只有两个 Encoder 和两个 Decoder 的情况下：

![](img/ab766e0ca589003d5dd5c98bb907a61c.png)

## 重点：Z 表示基于注意力得分，实现不同位置的关注情况。

## Decoder 解码器

## 第一步，最后一个 Encoder 向 Decoder 的传递 K 和 V

**编码器首先处理**输入序列。顶层编码器的 输出（Z？） ，随后被转换成一组注意力向量K和V。这些将被每个解码器在其“编码器-解码器注意力（Encoder-Decoder Attention）”层中使用，这有助于解码器专注于输入序列中的适当位置：

![](img/21414c7fda8efce0b252d37ca4e663a8.png)

## 第二步，Decoder 有了新的输入：第一个 output "I"。其它不变

接下来的步骤重复该过程，直到达到一个终止符号，表明变压器解码器已经完成了其输出。

每一步的输出，都被送到下一个时间步骤中的底层解码器（拼接在一起当成输入），并且解码器像编码器一样逐步产生其解码结果。就像我们对编码器输入所做的那样，我们也对这些解码器输入进行嵌入并添加位置编码，以指示每个单词的位置。

此时的由最后一个 Encoder 传递过来的 K 和 V，依然重复在每一个 Decoder 中被使用。

## 第三步，及以后的步骤，都在重复第二步

这里关键的地方在于，一旦有一个新的输出单词，就会拼接到 Decoder 的Input。

## 解码器中的注意力层

解码器中的自注意力层与编码器中的自注意力层略有不同：

+   在解码器中，自注意力层只允许关注输出序列中较早的位置（因为后面的单词还没有被生成出来）。这是通过在自注意力计算的softmax步骤之前 对未来位置进行掩码（Mask，将它们设为无穷小）来实现 的。

+   “编码器-解码器注意力”层的工作方式与多头自注意力相似，不同之处在于它从其下方的层创建查询矩阵，并从最后一个 Encoder 的输出中获取 K 和 V 矩阵。

## 最终的 Linear 和 Softmax 层

解码器堆栈输出一个 [浮点数 ](https://www.zhihu.com/search?q=%E6%B5%AE%E7%82%B9%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)向量**。我们怎样将其转换成一**个词呢？这是最后一个线性层的工作，其后是一个Softmax层。这里的线性层是一个简单的全连接神经网络，它将由解码器堆栈产生的向量投影到一个更大的向量上，称为 logits 向量（ 本质就是概率得分向量 ）。

假设**我们的模型**知道 10,000 个独特的英语单词（Vocabulary），这是它从训练数据集中学到的。这将使 logits 向量宽达10,000个单元——每个单元对应一个独特词汇的分数（ 概率得分 ）。

**然后s**oftmax层将这些分数转换为 概率 （真正的意义上的概率，总和为 1）。选择概率最高的 cell，并将与之关联的词作为这个时间步骤的输出。

![](img/e4c9891afdace61fd587bbf31ab762a5.png)

## 训练过程

在模型训练期间，未经训练的模型将经历完全相同的前向传递。但由于我们正在使用带标签的训练数据集对其进行训练，我们可以将其输出与实际正确的输出进行比较。

为了形象化这一点，假设我们的输出词汇表只包含六个单词：

（“a”、“am”、“i”、“thanks”、“student”和“<eos>”（End of Sentence 的缩写））

![](img/c33d354307dd98dd197c2526043b2956.png)

一旦我们定义了输出词汇表，我们就可以使用同样宽度的向量来[表示我们词](https://www.zhihu.com/search?q=%E7%8B%AC%E7%83%AD%E7%BC%96%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)汇表中的每个单词。这也被称为 独热编码 （one-hot encoding）。例如，我们可以使用以下向量来表示单词“am”：

![](img/93516d935a83d66c078ea5676bae4283.png)

## [损失函数](https://www.zhihu.com/search?q=%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)

假设这是训练阶段的第一步，我们正在用一个简单的例子来训练——把“merci”翻译成“thanks”。

[这意味着我](https://www.zhihu.com/search?q=%E6%A6%82%E7%8E%87%E5%88%86%E5%B8%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)们希望输出是一个指示单词“thanks”的 概率分布 。但由于这个模型还没有训练好，所以output 作为 prediction与真实结果往往相差甚远。

![](img/908e6014b28ad26e880440ef972d8381.png)

如何比较两个概率分布？我们只需将一个概[率分布从](https://www.zhihu.com/search?q=%E4%BA%A4%E5%8F%89%E7%86%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)另一个中减去。更多细节可以查看 交叉熵 和Kullback–Leibler散度。

但请注意，这是一个过于简化的例子。更现实地，我们会使用比一个单词更长的句子。例如 - 输入：“je suis étudiant”，期望输出：“i am a student”。这实际上意味着我们希望我们的模型依次输出概率分布，其中：

+   每个概率分布由一个宽度为vocab_size（在我们的玩具示例中为6，但在现实中可能是30,000或50,000）的向量表示。

+   第一个概率分布在与单词“i”相关联的单元格中有最高的概率。

+   第二个概率分布在与单词“am”相关联的单元格中有最高的概率。

+   此类推，直到第五个输出分布表示‘<end of sentence>’符号，它在10,000元素词汇表中也有一个相关联的单元格。

![](img/d9f84b341a9e37802dd1f164de1a5ad8.png)

经过多轮训练以后，可能会得到：

![](img/b15a5fdb226f403b33de97be74479ade.png)

现在，因为模型一次产生一个输出，我们可以假设模型正在从概率分布中选择概率最高的单词，并丢弃其余的。这是一种做法（称为贪婪解码，greedy decoding）。

另一种方法是 **保留比如前两个单词 **（比如“I”和“a”），然后在下一步中，运行模型两次：一次假设第一个输出位置是单词“我”，另一次假设第一个输出位置是单词“一个**”，并且保留在考虑第1和第2位置时产生较少错**误的那个版本。我们重复这个过程，对第2和第3位置[...等等](https://www.zhihu.com/search?q=%E6%9C%AA%E5%AE%8C%E6%88%90%E7%9A%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677202804%7D)。这种方法被称为 “束搜索”（beam search，相对而言，贪婪算法每次只找出一个最优的单词）。 在我们的例子中，束大小（beam_size）=2。这也意味着在所有时候，两个部分假设（ 未完成的 翻译）被保留在内存中。而最优束（top_beams），也是两个。而这个 beam_size，也只是模型的一个超参数。

## 参考资料

+   [https://www.youtube.com/wa](https://link.zhihu.com/?target=https%3A//www.youtube.com/watch%3Fv%3D-QH8fRhqFHM%26t%3D268s)tch?v=-QH8fRhqFHM&t (视频讲得很简洁，但是和博文不重复，是个很好的补充)

+   [h](https://link.zhihu.com/?target=https%3A//jalammar.github.io/illustrated-transformer/)ttps:// jalammar.github.io/illu strated-transformer/

+   Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017). Attention is all you need. Advances in neural information processing systems, 30.