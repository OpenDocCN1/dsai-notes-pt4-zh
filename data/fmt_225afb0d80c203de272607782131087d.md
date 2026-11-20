
# Transformer中的三种注意力机制

> 来源：[https://v11enp9ok1h.feishu.cn/docx/RRq9dJibvo4Jgbxpf78cogHqnwf](https://v11enp9ok1h.feishu.cn/docx/RRq9dJibvo4Jgbxpf78cogHqnwf)

注意力机制在我们模型中的应用Transformer使用多头注意力机制的三种不同方式如下：

+   在“编码器-解码器注意力”层中，查询来自前一层解码器，而记忆键和值则来自编码器的输出。这使得解码器中的每个位置都能关注输入序列中的所有位置。这模仿了序列到序列模型中典型的编码器-解码器注意力机制。
+   编码器包含自注意力层。在自注意力层中，所有的键、值和查询都来自同一个地方，即编码器前一层的输出。编码器中的每个位置都可以关注编码器前一层中的所有位置。
+   同样，解码器中的自注意力层允许解码器中的每个位置关注解码器中包括该位置在内的所有位置。我们需要防止解码器中的信息向左流动，以保持自回归属性。我们通过将softmax输入中对应非法连接的所有值掩盖掉（设为-∞）来实现这一点。

相信大家跟我一样，刚开始看到这段话一脸懵逼，每个字都认识，但组合起来却感觉难以理解，甚至有点“不明觉厉”的感觉。

那今天我们来说人话、敲黑板、划重点，希望通过一系列问答，让大家真正一文搞懂Transformer中的三种注意力机制。

本文将从`Self Attention`、`Cross Attention`、`Causal Attention`三个方面，带您一文搞懂 Transformer中的三种注意力机制。（每篇文章的标准开头不能少，生活再苦再累，仪式感不能缺）

![](img/be6df22810df3488587bd31fa7b438be.png)

## 一、`Self Attention`

疑问一：图中编码器明明写的是`Multi-Head Attention`，怎么说是`Self Attention`？

![](img/541be1bcd39aa6b56c3fb602b7c4cc5d.png)

先了解三个概念：`Scaled Dot-Product Attention`、`Self Attention`、`Multi-Head Attention`

`Scaled Dot-Product Attention`（缩放点积注意力）：输入包括维度为`dk`的查询（queries）和键（keys），以及维度为`dv`的值（values）。我们计算查询与所有键的点积，每个点积结果都除以`√dk`，然后应用`softmax`函数，以得到注意力分数。

体现如何计算注意力分数，关注`Q`、`K`、`V`计算公式。

![](img/09beb355b98a2c82a58f9db0d85e6929.png)

`Self Attention`（自注意力）：对同一个序列，通过缩放点积注意力计算注意力分数，最终对值向量进行加权求和，从而得到输入序列中每个位置的加权表示。

表达的是一种注意力机制，如何使用缩放点积注意力对同一个序列计算注意力分数，从而得到同一序列中每个位置的注意力权重。

![](img/09507efcdb884730537af6064877b7ec.png)

`Multi-Head Attention`（多头注意力）：多个注意力头并行运行，每个头都会独立地计算注意力权重和输出，然后将所有头的输出拼接起来得到最终的输出。

强调的是一种实操方法，实际操作中我们并不会使用单个维度来执行单一的注意力函数，而是通过`h=8`个头分别计算，然后加权平均。这样为了避免单个计算的误差。

![](img/4adca1639541e39c4c470e5f40087dfc.png)

疑问解答：`Scaled Dot-Product Attention`、`Self Attention`、`Multi-Head Attention`实际上说的是同一件事，从不同维度解答如何获取同一个序列中每个位置的注意力权重。图上标注`Multi-Head Attention`强调需要多个头计算注意力权重。

Transformer第一个注意力（`Self Attention`）更严谨的描述应该为：编码器输入序列通过`Multi-Head Self Attention`（多头自注意力）计算注意力权重。

## 二、`Cross Attention`

疑问二：图中编码器明明写的也是`Multi-Head Attention`，怎么说是`Cross Attention`？

![](img/ecb9cb838a70827245e8910536283ddd.png)

先了解一个概念：`Cross Attention`

`Cross Attention`（交叉注意力）：输入来自两个不同的序列，一个序列用作查询（Q），另一个序列提供键（K）和值（V），实现跨序列的交互。

![](img/5c92b156e92517b15ac1d08061be0984.png)

`Cross Attention`与`Self Attention`的区别：

+   输入来源：
    +   `Cross Attention`：来自两个不同的序列，一个来自编码器，一个来自解码器
    +   `Self Attention`：来自编码器的同一序列
+   实现目标：
    +   `Cross Attention`：解码器序列用作查询（Q），编码器序列提供键（K）和值（V），用于在编码器-解码器两个不同序列之间进行注意力转移。
    +   `Self Attention`：查询（Q）、键（K）和值（V）均来自编码器同一序列，实现编码器序列内部的注意力计算。

疑问解答：`Cross Attention`、`Multi-Head Attention`实际上说的是也同一件事，从不同维度解答两个不同序列之间如何进行注意力转移。图上标注`Multi-Head Attention`强调需要多个头进行注意力转移计算。

Transformer第二个注意力（`Cross Attention`）更严谨的描述应该为：编码器-解码器两个序列通过`Multi-Head Cross Attention`（多头交叉注意力）进行注意力转移。

## 三、`Causal Attention`

疑问三：图中编码器明明写的也是`Masked Multi-Head Attention`，怎么说是`Causal Attention`？

![](img/4f4a3902372d98bf2615c26d0b299ba5.png)

先了解四个概念：`Predict The Next Word`、`Masked Language Model`、`Autoregressive`、`Causal Attention`

`Predict The Next Word`（预测下一个词）：模型通常需要基于已经生成的词来预测下一个词。这种特性要求模型在预测时不能“看到”未来的信息，以避免预测受到未来信息的影响。

![](img/0fdfd0bf2fcbe7f7f902f10c59e58ab9.png)

`Masked Language Model`（掩码语言模型）：遮盖一些词语来让模型学习预测被遮盖的词语，从而帮助模型学习语言规律。

![](img/875d28b6273e6a4668e291f4a0fe8654.png)

`Autoregressive`（自回归）：在生成序列的某个词时，解码器会考虑已经生成的所有词，包括当前正在生成的这个词本身。为了保持自回归属性，即模型在生成序列时只能基于已经生成的信息进行预测，我们需要防止解码器中的信息向左流动。换句话说，当解码器在生成第`t`个词时，它不应该看到未来（即第`t+1`, `t+2`, ...等位置）的信息。

![](img/7499885862f639e8cbca759f7088b048.png)

`Causal Attention`（因果注意力）：为了确保模型在生成序列时，只依赖于之前的输入信息，而不会受到未来信息的影响。`Causal Attention`通过掩盖（mask）未来的位置来实现这一点，使得模型在预测某个位置的输出时，只能看到该位置及其之前的输入。

![](img/7ec2002cfc46a4a4396c8bb5ba40e34a.png)

疑问解答：`Causal Attention`、`Mask Multi-Head Attention`实际上说的是也同一件事，解码器中`Self Attention`如何结合`Causal Attention`来保持自回归属性。

`Mask Multi-Head Attention`强调使用了多个独立的注意力头，每个头都可以学习不同的注意力权重，从而增强模型的表示能力。而`Causal Attention`则强调了模型在预测时只能依赖于已经生成的信息，不能看到未来的信息。

Transformer第三个注意力（`Causal Attention`）更严谨的描述应该为：解码器的单个序列通过`Multi-Head Causal Self Attention`（多头因果自注意力）进行注意力计算。
