# Transformer 模型原理通俗解释（DeepLearning.AI+EasyAI）

> 来源：[https://agijuejin.feishu.cn/docx/SyGBdd4BlozZ32xoqwwcXFz5nff](https://agijuejin.feishu.cn/docx/SyGBdd4BlozZ32xoqwwcXFz5nff)

> 作者：王几行XING

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/645687139)hihu.com/p/645687...

## 1 Attention Is All You Need

![](img/32e8356a8b877be7f78c5436517b5546.png)

+   《 Att[entio](https://www.zhihu.com/search?q=%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)n is All [You Ne](https://www.zhihu.com/search?q=%E6%B3%A8%E6%84%8F%E5%8A%9B%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)[ed 》是由G](https://www.zhihu.com/search?q=%E5%8D%B7%E7%A7%AF%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)oogle的研究人员在2017年发表的一篇研究论文，它引入了Transformer模型，这是一种革命性的架构，彻底改变了自然语言处理（NLP）领域，并成为我们现在熟知的语言模型（LLMs）的基础，比如GPT、PaLM等等。该论文提出了一种 神经网络 架构，用完全基于 注意力机制 的方式取代了传统的循环神经网络（RNNs）和 卷积神经网络 （CNNs）。

+   [Transfo](https://www.zhihu.com/search?q=%E8%87%AA%E6%B3%A8%E6%84%8F%E5%8A%9B%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)[rmer](https://www.zhihu.com/search?q=%E5%B9%B6%E8%A1%8C%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)模型使用 自注意力机制 来计算输入序列的表示，从而允许它有效地捕捉长期依赖关系并 并行化 计算。作者证明了他们的模型在几个机器翻译任务上取得了最先进的性能，并且胜过了依赖于RNNs或CNNs的之前的模型。

+   [Tran](https://www.zhihu.com/search?q=%E8%A7%A3%E7%A0%81%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)sformer架构由一个编码器和一个 [解码器 组成，](https://www.zhihu.com/search?q=%E5%89%8D%E9%A6%88%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)[每个部分都](https://www.zhihu.com/search?q=%E5%89%8D%E9%A6%88%E7%BD%91%E7%BB%9C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)由多个层组成。每个层包含两个[子层：一个](https://www.zhihu.com/search?q=%E5%85%A8%E8%BF%9E%E6%8E%A5%E5%B1%82&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)多头自注意力机制和一个 前馈神经网络 。多头自注意力机制允许模型关注输入序列的不同部分，而 前馈网络 对每个位置分别应用点对点的 全连接层 。

+   [Trans](https://www.zhihu.com/search?q=%E6%AE%8B%E5%B7%AE%E8%BF%9E%E6%8E%A5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)[forme](https://www.zhihu.com/search?q=%E4%BD%8D%E7%BD%AE%E7%BC%96%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)r模型还使用 残差连接 和层归一化来促进训练并防止过拟合。此外，作者引入了一种 位置编码 方案，用于编码输入序列中每个标记的位置，使模型能够在不需要循环或卷积操作的情况下捕捉序列的顺序。

## [2 En](https://www.zhihu.com/search?q=%E6%A8%A1%E5%9E%8B%E5%88%86%E7%B1%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)coder/Decoder/Encoder+Decoder 模型分类

![](img/5732d42d82e81dbd47bbc6a3b4b7fa12.png)

## 3 Encoder/Decoder/Seq2seq 补充解释

参考： [一文看懂 NLP 里的模型框架 Encod](https://link.zhihu.com/?target=https%3A//easyai.tech/ai-definition/encoder-decoder-seq2seq/%23Encoder-Decoder)er-Decoder 和 Seq2Seq

![](img/f5f8e997afe7f4af8f4d9a4591488dbb.png)

![](img/34f1a9d2f1a42e147f39d7221df9b813.png)

![](img/8f25292eb5858e31230dd7d07f3e2e43.png)

上图中，d0~d3，可能由 e0~e6 一个或者多个的加权之和推出。

## 4 [Attention 机制](https://www.zhihu.com/search?q=Attention%20%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)

[最初 Att](https://www.zhihu.com/search?q=%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)ention 机制最早被[应用在 计算机](https://www.zhihu.com/search?q=%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E5%A4%84%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)[视觉 领域。随后，它也逐](https://www.zhihu.com/search?q=Attention%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)**渐在 自然**语言**处理 （**NLP）领域中得到应用。然而，真正让 Attention机制 大放异彩的是在NLP领域。这得益于在2018年， BERT 和 GPT 这两种模型在NLP任务上取得了惊人的效果，引起了广泛的关注。由于它们的成功，Transformer架构和Attention机制这些核心技术也开始受到大家的密切关注。

想象一下你在学习时的注意力：当你在阅读一本书的时候，你可能会集中注意力在某个特定的句子或段落上，因为它们对你理解整个故事或概念非常重要。这种集中注意力的过程可以被看作是一种“注意力机制”。在机器学习中，"Attention 机制"也类似于这个概念，它允许模型在处理信息时专注于重要的部分，从而更好地解决问题。

在自然语言处理（NLP）和计算机视觉等领域，Attention 机制的应用十分广泛。让我们以 NLP 为例来简单地理解 Attention 机制：假设有一个机器学习模型要完成一个翻译任务，将英文句子翻译成法文句子。传统的模型可能会简单地将整个输入英文句子作为一个整体来处理。但是，这样的方法可能会忽略句子中不同部分的重要信息。而 Attention 机制就像是一种更智能的“注意力”，它会告诉模型在处理每个输出（法文）单词时，应该注意输入（英文）句子的哪些部分。

具体来说，Attention 机制允许模型在输出每个法文单词时，动态地将不同权重的“注意力”集中在输入英文句子的不同位置。在翻[译的例子中](https://www.zhihu.com/search?q=%E8%B0%93%E8%AF%AD%E5%8A%A8%E8%AF%8D&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)，模型可能在翻译法文句子的第一个单词时，会更关注英文句子的主语部分；而在翻译第二个单词时，会更关注英文句子的 谓语动词 部分。这样，模型能够根据输入和输出之间的关联性，有选择地获取和利用输入信息，提高翻译质量。

总结一下，Attention 机制就是模拟人类注意力的分配方式，让机器学习模型能够根据任务的需要，集中关注输入中最相关的部分。通过这种方式，模型可以更好地处理复杂的任务，并且在自然语言处理、计算机视觉等领域取得更好的效果。

## [5 Tr](https://www.zhihu.com/search?q=%E5%9B%BE%E7%89%87%E8%AF%86%E5%88%AB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)ansformer 与CV/ 图片识别

T[ransf](https://www.zhihu.com/search?q=%E5%B9%B6%E8%A1%8C%E8%AE%A1%E7%AE%97&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)[ormer最初是为了自然语言](https://www.zhihu.com/search?q=Transformer%E6%9E%B6%E6%9E%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)[处理（NL](https://www.zhihu.com/search?q=%E5%9B%BE%E7%89%87%E5%88%86%E7%B1%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)P）任务而提出的，特别是在机器翻译任务中取得了巨大成功。然而，由于其出色的 并行计算 能力和能够捕捉长距离依赖关系的优点， Transformer架构 逐渐在计算机视觉领域的图片分类任务中得到了广泛应用。 图片分类 是计算机视觉中的基础任务之一，旨在将输入图像分为预定义的类别。下面是Transformer在图片分类任务中的应用综述：

### Self-Attention机制 在图片分类中的应用

Transformer中的Self-Attention机制允许模型对输入的不同位置进行关注，从而更好地捕捉图像中不同区域的重要性。在图片分类任务中，这种机制可以使模型集中关注图像中的关键区域，以便更准确地进行分类。

### Vision Transformer (ViT)

Vision Transformer是将Transformer架构应用于图片分类任务的一个重要实现。它将图像拆分为一系列固定大小的图[像块，然后](https://www.zhihu.com/search?q=%E5%9B%BE%E5%83%8F%E5%88%86%E7%B1%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A645687139%7D)将这些图像块转换为序列，并使用Transformer的Self-Attention机制来处理这些序列数据。Vision Transformer在许多 图像分类 基准数据集上取得了令人瞩目的性能，展现出了Transformer在计算机视觉中的潜力。

### 位置编码

由于Transformer是基于序列的架构，它对输入数据的位置信息非常敏感。在NLP任务中，位置编码通常通过嵌入技术来添加到输入序列中，但在图片分类中，图像是二维数据。因此，位置编码需要适当修改，以适应图片的结构。

### 预训练与微调

与NLP中的BERT类似，Vision Transformer模型也可以通过预训练阶段进行学习，然后在特定的图片分类任务上进行微调。预训练使得模型能够在大规模数据上学习图像特征，从而更好地适应特定的图片分类任务。

### 性能与可扩展性

尽管Transformer在图片分类任务上表现出色，但由于计算和内存需求较高，特别是对于大规模高分辨率图像，其在一些特定场景下可能会受到限制。因此，研究人员一直在努力改进Transformer架构，以提高其性能和可扩展性。