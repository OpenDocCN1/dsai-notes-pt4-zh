# Transformer动画讲解 - 数据处理的四个阶段

> 来源：[https://geekbang.feishu.cn/docx/XzjFd3oWKoU1YwxGZ5ocx4cznih](https://geekbang.feishu.cn/docx/XzjFd3oWKoU1YwxGZ5ocx4cznih)

> 👨🏻 作者：AllenTang

> 📃 作者介绍：分享人工智能，让所有人玩转AI

> 🛖 公众号：架构师带你玩转AI

> ⏰ 收录时间：2024-05-23 11:28:42

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/bFeXsWpWEwUBTRzz1_IbxQ)q.com/s/bFeXsWpW...

![](img/dcfb3e75a2165e7597b16b670e3dcf34.png)

关注作者公众号，获取更多AI知识

**动画地址：查看原文，可看动画**

**T**ransformer模型是一种基于自注意力机制的神经网络架构，广泛应用于自然语言处理任务，如机器翻译、文本摘要等。

![](img/09de4f61a9ff5ed2bb5d924ddeca05f7.png)

**Transformer**

神经网络算法 - 一文搞懂Transformer

**从端**到端的角度来看，数据在**Tr**ansformer中的流转可以概括为四个阶段：Embedding（嵌入）、Attention（注意力机制）、MLPs（多层感知机）和Unembedding（ 从模型表示到最终输出 ）。

![](img/464a9e9e7d0d7e44d59fe7a894104b7e.png)

**Embedding -> Attention ->** MLPs -> Unembedding

**下面是对这四个阶段的简要介绍：**

**Embedding（嵌入）：**

+   **模型的输入通常是离散的词或符号（例如，在英文文**本中，每个单词或标点符号都是一个符号）。

+   **E**mbedding层将这些离散的符号转换为连续的、固定大小的向量（即词嵌入）。这些向量捕捉了符号的语义和上下文信息。

+   **对**于Transformer的编码器（Encoder）和解码器（Decoder），都会有一个Embedding层。

+   **在解**码器中，还有一个额外的位置嵌入（Positional Embedding）层，用于捕捉序列中单词的位置信息，因为Transformer不依赖于RNN或CNN等结构来捕获顺序信息。

神经网络算法 - 一文搞懂Embedding（嵌入）

Attention（注意力机制）：

+   **注意力机制是Transformer的核心。它允许模型在**生成当前输出时关注输入序列中的不同部分。

+   **Transformer使用了多种注意力机制，包括自注意力（Self-Attention）、编码器-解码器注意力（Encod**er-Decoder Attention）、掩码多头注意力（ Mask Multi-Head Attention） 等。

+   **自注意力允许模型在同一序列的不同位置之**间建立依赖关系，从而捕获序列的内部结构。

+   编码器-解码器注意力允许模型在生成输出时关注输入序列的特定部分。

+   ** 在注意力计算过程中，会生成一个注意力权重矩阵，该矩阵描述了输**入序列中每个位置对当前位置的重要性。

神经网络算法 - 一文搞懂Transformer中的三种注意力机制

MLPs（多层感知机，也称为FFNN前馈神经网络）：

+   **经**过注意力机制处理后，模型会通过一个或多个全连接层（也称为前馈网络或MLPs）进行进一步的变换和特征提取。

+   **这些全连接层可以捕捉输入数据中的**非线性关系，并帮助模型学习更复杂的模式。

+   **在**Transformer中，MLPs通常位于自注意力层和归一化层之间，形成所谓的“编码器块”或“解码器块”。

神经网络算法 - 一文搞懂FFNN（前馈神经网络）

Unembedding（从模型表示到最终输出）：

+   可以将其理解为从模型内部表示转换到最终输出格式的过程。

+   **对**于文本生成任务，如机器翻译，解码器的输出会经过一个线性层和一个softmax函数，以生成一个概率分布，该分布描述了下一个可能输出的词的概率。

+   **对**于其他任务（如文本分类），解码器的输出可能会直接用于计算损失函数（如交叉熵损失），或者通过其他方式转换为最终的预测结果。

神经网络算法 - 一文搞懂 Softmax 函数

**参考资料 **

+   https://www.3blue1brown.com/lessons/gpt