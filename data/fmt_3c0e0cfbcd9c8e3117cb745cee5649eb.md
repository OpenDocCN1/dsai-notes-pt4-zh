
1. It expands the model’s ability to focus on different positions. Yes, in the example above, `z1` contains a little bit of every other encoding, but it could be dominated by the actual word itself. If we’re translating a sentence like “The animal didn’t cross the street because it was too tired”, it would be useful to know which word “it” refers to.

2. It gives the attention layer multiple “representation subspaces”. As we’ll see next, with multi-headed attention we have not only one, but multiple sets of `Query`/`Key`/`Value` weight matrices (the Transformer uses eight attention heads, so we end up with eight sets for each encoder/decoder). Each of these sets is randomly initialized. Then, after training, each set is used to project the input embeddings (or vectors from lower encoders/decoders) into a different representation subspace.

我对上面这段引用自The Illustrated Transformer的话的理解：首先，在翻译一个特定单词的时候，模型的注意力肯定主要关注这个词自身。我们希望翻译这个词的时候模型多去关注上下文的其他词，但上下文联系其实也有很多种，有it这样的指代、are这样的单复数、were这样的时态，或者说基于不同的上下文，一个单词自身也会有不同的意义（想起ELMo还有Stick了吗？）。如果我们多训练几个不同的权重矩阵 、 、 的组合，让他们各司其职，有的负责指代任务，有的负责时态任务，有的负责基于全局语境搞清楚多义词（即去探索多个不同的表征子空间），那是不是模型就可以取得更好的效果？

因此，简单来说，多头注意力就是我们多训练几个权重矩阵 、 、 。

Attention is All You Need原文中，Transformer用了8个Attention Heads。我们先用下图展示两头注意力：

![](img/35d945c8c7d8983e35889f7131bf76a8.png)

对于同一个输入Thinking Machine，我们有8个权重矩阵组，因此最终会产生8个`Z`矩阵：

![](img/f6446b6af79d15df6b04c3d7a91eb702.png)

我们将这些矩阵连接起来，形成了一个2*512维的矩阵；然后将其乘以一个产生最终输出的权重矩阵 。

下面的例子中， 是一个512*512的矩阵，最后我们得到了一个2*512的`Z`矩阵，他的形状和最开始的输入一样。

![](img/96745f8ffc1f6d98bfeb4fb8755fa71f.png)

## 总结：自注意力层中发生了什么？

+ 输入：基于one-hot，假设字典大小是30,000,那么一个词的输入就是一个30K维的向量，对应该词汇的元素会是1，其他的全部都是0。
+ Embedding层：把输入的one-hot向量乘以一个矩阵后，投影到512维的词向量里，再加上一个位置嵌入。
+ 自注意力层：对输入的词嵌入，乘三个权重矩阵，投影出`Q`，`K`，`V`。输入每一个词`Qk`会和输入的全部词的`K1`、`K2`、`K3`….`Kn`向量做点积（向量点积得到的是标量），得到一个`n`维的新向量（`n`就是输入的句子的长度）。对向量的元素除以8（根号64），再做一个`softmax`，得到一个Attention Distribution，这个分布反映了当前输入的词对句子中每一个词的相关性。而后我们对`V`向量以Attention Distribution为权重做一个加权，得到向量`Z`。向量`Z`里就蕴含了当前timestep下来自整个句子的各种信息。
+ 多头注意力机制：多训练几个不同的权重矩阵组合，把他们最后得到的结果拼接到一起，提高信息收集维度的数量和鲁棒性。

![](img/111d90549beeeda6ceb2b4bcddc67440.png)

自注意力层最终输出的一个`N*512`的矩阵会输入给前馈神经网络。嗯，所以它的形状和最开始的时候输入的矩阵是一样的。即：一个多头自注意力层并不改变输入的东西的形状。

FFNN就是一个平平无奇的全连接层，和2.1.4中介绍的东西没有什么区别，所以就不再专门做介绍了。

它也不改变输入的东西的形状。总之，输入一个（`bacthsize`, `seq_length`, `embed_dim`）的张量到一个Encoder Block里去，输出的也是一个同维的张量。

## 解码器介绍

解码器的结构在10.1.2里展示过，这里不再重复。

与seq2seq一样，Transformer中的解码器本质上也是一个语言模型，它做的事情就是综合考虑当前时间步的信息和Encoder给来的信息，进行一个词一个词的自回归的输出。

既然同样是一个自回归语言模型，那么7.2.2提到的Teaching Force和计划采样、7.2.3中提到的Beam Search在Decoder中都是同样适用的。这里也不重复介绍了。

![](img/8f06d7c3e19191d27ff1024c36c02c3e.png)

Decoder模型中的一些细节可能需要更多的输入才能讲清楚，所以现在我们试图去翻译I have a cat为中文。

### 解码器的第一层：带掩码的多头自注意力层

解码器中的自注意力层和编码器中的不太一样：在解码器中，自注意力层只允许关注已输出位置的信息。所以我们称之为“带掩码的自注意力层”。

> Decoder block 的第一个 Multi-Head Attention 采用了 Masked 操作，因为在翻译的过程中是顺序翻译的，即翻译完第`i`个单词，才可以翻译第`i+1`个单词。通过 Masked 操作可以防止第`i`个单词知道`i+1`个单词之后的信息。

`Mask`操作主要是在`Softmax`的时候实现的。

如果要翻译I have a cat为中文，我们依然可以做 和 做矩阵乘法的操作，这样我们会得到一个4*4的矩阵。

![](img/37569cd0753ce7ef96d9d1b3d1da8685.png)

一般来说，对于我们这样得到的矩阵 ，直接`softmax`就好了，参考10.2.2.5。但是在解码器里，我们要做这样一个操作：

![](img/d3bd374b71167b51bc69be8a1c7f1444.png)

自注意力层只允许关注已输出位置的信息，所以对于已输出位置和未输出的位置的点积（上图中的黑色的位置），我们在`softmax`时会给他加上一个巨大的负值（GPT中为负一亿），好让他们在`softmax`后得到的结果为0。这样，在输出当前时间步的时候，解码器就不会去关注后面的单词的信息了。

最终我们得到了一个4*64的矩阵，它会作为`Query`矩阵直接被输入给解码器的下一层Self-Attention。矩阵的第一列（I的`z`向量，也是下一个注意力层中 I 的查询向量）就只包含了I的信息。

![](img/4663260a0a3c321a8f5ea51daa23c174.png)

![](img/6bf78ae2bf7fba5865210c796f65812c.png)

<这张图也非常直观。>

解码器中也是多头注意力机制，所以如果是8头注意力的话，最后会产生8个这样子的矩阵。他们会作为解码器的下一层要用到的8个`Query`矩阵。

### 解码器的第二层：多头编码器-解码器注意力层

参考右图，红框内的部分即为解码器。我们假定`N`为6；八头注意力。

编码器首先处理输入序列，输入序列经过了6个Encoder之后，第六个编码器会输出一个张量。该张量会被平等的送给每个Decoder的第二个注意力层，并通过一个共享参数的权重矩阵`C`转为8个`Key`矩阵和8个`Value`矩阵。

![](img/131688e256dc9a853bca2b7601642757.png)

在这一层中，解码器组件使用自己的带掩码的第一层自注意力层产生的8个`Z`矩阵作为8种`Query`矩阵，并使用编码器传过来的8个`Key`和`Value`矩阵，来做我们前面提到过的多头自注意力的运算。

![](img/5b1ce1b49fbc66058274cf820294fee9.png)

<因为在这一层中我们不仅用到了解码器上一层的输出，还用到了编码器的输出，所以就不能叫自注意力层了>

在经过两个多头注意力层后，最后会经过一个全连接层，然后输出一个向量给最后的`Linear`和`Softmax`层。

### 解码器的最后：下一词预测

> Decoder输出的是一个浮点型向量，如何把它变成一个词？这就是最后一个线性层和`softmax`要做的事情。

> 线性层就是一个简单的全连接神经网络，它将解码器生成的向量映射到`logits`向量中。

> 然后，`softmax`层将这些分数转化为概率（全部为正值，加起来等于1.0），选择其中概率最大的位置的词汇作为当前时间步的输出。

就像seq2seq一样，transformer的解码器中当前时间步的输出会作为下一个时间步的输入。

![](img/174e65c09d55cab2b8eb7fdf9018c91b.png)

## Transformer模型总结

### 模型可以去调的超参数

Transformer里可以调的参数其实并不多。

1. 词嵌入的向量有几维；
2. 编码器和解码器各自有多少层。如果没有编码器，那就是Decoder-Only（GPT）；如果没有解码器，那就是Encoder-Only（BERT）
3. 有多少个注意力头。

### 模型整体的前向传播步骤

**编码器：**

+ 输入：基于one-hot，假设字典大小是30,000,那么一个词的输入就是一个30K维的向量，对应该词汇的元素会是1，其他的全部都是0；输入一个句子的话，那就是矩阵了。
+ Word Embedding层：把输入的one-hot向量乘以一个矩阵后，投影到512维的词向量里。此外还要考虑位置编码，它是一个和词嵌入同维的、`sin`和`cos`交替的向量，两者可以直接相加作为最后的输入。
+ 自注意力层：对输入的词向量，乘三个权重矩阵，投影出`Q`，`K`，`V`。输入的第`k`个词的查询向量`Qk`会和输入的全部词的`K1`、`K2`、`K3`….`Kn`向量做点积（向量点积得到的是标量），得到一个`n`维的新向量（`n`就是输入的句子的长度）。除以8，再做一个`softmax`，得到一个Attention Distribution，这个分布反映了当前输入的词对句子中的每一个词的相关性。而后我们对`V`向量以Attention Distribution为权重做一个加权，得到向量`Z`。向量`Z`里就蕴含了当前timestep下来自整个句子的各种信息。
+ 多头注意力机制：多训练几个不同的自注意力层，最后拼接到一起，提高信息收集维度的数量和鲁棒性。
+ 自注意力层的输出会经过一个全连接层，生成一个向量，而后传给下一个编码器。最后一个编码器输出的向量会与权重矩阵`C`相乘，生成价值矩阵`V`和钥匙矩阵`K`。

**解码器：**

+ 带掩码的多头自注意力层：Decoder的输入里，在自注意力层中，对于当前的time step之后的单词，在`softmax`之前都会加一个巨大的负数，让他们的attention distribution都会是0。最终输出的张量会作为下一层的查询矩阵。因为是8头注意力，所以最后会输出8个不同的查询矩阵，
+ 编码器-解码器注意力层：这一层中，会用来自编码器的8个`K`和来自上一层的8个`Q`做点积操作，计算出注意力分布后再和来自编码器的8个`V`矩阵做加权。
+ 输出：解码器以自回归的形式一个一个输出，当前的输出成为下一个输入。输出的部分可以greedy，也可以做一个beam search。

损失函数和反向传播这里就跳过了。损失函数依然是交叉熵函数。

### Transformer的牛逼之处

+ Transformer 与 RNN 不同，不论是自注意力层还是全连接层都是矩阵运算，可以比较好地并行训练。在做NLP的任务时，CNN、RNN和Transformer相比，并行程度CNN大于Transformer远大于RNN，信息糅合能力RNN大于Transformer远大于CNN。Transformer它没有被任何人远大于，可能这就是它为什么牛逼吧。
+ Transformer 本身是不能利用单词的顺序信息的，因此需要在输入中添加位置 Embedding，否则 Transformer 就是一个词袋模型了。
+ Transformer 的重点是 Self-Attention 结构，其中用到的 `Q`, `K`, `V`矩阵通过输出进行线性变换得到。这件事情还是挺天才的，正经人很难想到自己和自己做点积然后再和另一个自己相乘。
+ Transformer 中 Multi-Head Attention 中有多个 Self-Attention，可以捕获单词之间多种维度上的相关系数。八个注意力头在8个不同的表征子空间横扫信息，让模型学习能力非常强。

### Transformer的评价

+ 它几乎被用在所有的NLP任务上，产生了类似CNN对CV的改变。
+ 几乎用在了每一个模态的机器学习上，文本、图片、视频，万物皆可Transformer。
+ 想要让Transformer达到和CNN、RNN一样的效果，得给他喂多得多的数据量。基于Transformer的模型都很大、很贵，虽然它在刷SOTA上所向披靡，但是CV业内其实还是CNN用的更多。
+ 事实上，Attention is NOT All You Need（就像The Document is NOT All You Need一样，没有其他的参考文献还是不可能去扎实的掌握LLM的技术原理）。后续的研究中发现，Transformer中的Resnet和MLP们都起到了很大的作用，缺一不可。如果只训练多头自注意力的话，最后的效果会非常糟糕。

![](img/690c04cbfbd14410b748f2c913f4f71c.png)

<后续出现了大量的魔改Transformer，有人抛弃了Encoder只留下Decoder，有人则只用了Encoder。但不论怎样，多头自注意力机制这样天才的想法还是被得以保留>

## 推荐学习材料

### 推荐视频材料

+ 【B站】Transformer论文逐段精读【论文精读】By 李沐
+ 【Youtube】Transformer By Hung-yee Li 李宏毅

### 推荐文字材料

+ 论文地址：Attention Is All You Need  非常简短的一篇文章，一句废话都没有，甚至到了为了限制篇幅而有很多事情没有讲明白的程度。非常不建议一上来就直接啃Paper，建议的学习路径：先跟着李沐/李宏毅的视频一起把论文过一遍，而后再去看下面推荐的文字学习资料进行学习。
+ 优质学习材料：The Illustrated Transformer By Jay Alammar
+ 优质翻译：[图解transformer | The Illustrated Transformer](http://t.csdn.cn/YFjLF) By Ann's Blog
+ 讲的非常清楚的一篇知乎文章：Transformer模型详解（图解最完整版）By 知乎用户@初识CV

---

# ULM-FiT与多任务学习 Multitask Learning：NLP的ImageNet时刻（2018.5）

我还是觉得ULM-FiT是NLP的ImageNet时刻，而不是Bert。ULM-FiT的出现，让那个时代的NLP终于有了一种像CV领域一样的迁移学习方法。

论文地址：[Universal Language Model Fine-tuning for Text Classification](https://arxiv.org/abs/1801.06146)

> Inductive transfer learning has greatly impacted computer vision, but existing approaches in NLP still require task-specific modifications and training from scratch. We propose Universal Language Model Fine-tuning (ULMFiT), an effective transfer learning method that can be applied to any task in NLP, and introduce techniques that are key for fine-tuning a language model. Our method significantly outperforms the state-of-the-art on six text classification tasks, reducing the error by 18-24% on the majority of datasets. Furthermore, with only 100 labeled examples, it matches the performance of training from scratch on 100x more data. We open-source our pretrained models and code.

![](img/f3ff34a3d920732eb616499dc1317078.png)
