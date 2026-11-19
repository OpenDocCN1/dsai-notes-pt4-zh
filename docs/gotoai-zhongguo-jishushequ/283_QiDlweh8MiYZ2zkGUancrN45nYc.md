
# Transformer动画讲解 - 数据处理的四个阶段

> 来源：[https://gotoaiworld.feishu.cn/docx/MvcidmxTYoqF6Gx6gpCcnEPYnAQ](https://gotoaiworld.feishu.cn/docx/MvcidmxTYoqF6Gx6gpCcnEPYnAQ)

`https://mp.weixin.qq.com/s?__biz=MzkzMTEzMzI5Ng==&mid=2247488783&idx=1&sn=8731571cfbc950448fdc9d8eb3f7f5fc&chksm=c26ef710f5197e064a19aa5dec3c519e033192e754e45a8d1909221fb5bf718d68bdf22d5ef8&cur_album_id=3476803651475865601&scene=190#rd`

## Transformer数据处理的四个阶段

`Transformer`模型是一种基于自注意力机制的神经网络架构，广泛应用于自然语言处理任务，如机器翻译、文本摘要等。

![](img/7838c8895fe1352830cbab1122d7ecb7.png)

`Transformer`

神经网络算法 - 一文搞懂`Transformer`

从端到端的角度来看，数据在`Transformer`中的流转可以概括为四个阶段：`Embedding`（嵌入）、`Attention`（注意力机制）、`MLPs`（多层感知机）和`Unembedding`（从模型表示到最终输出）。

![](img/38e37d18a467c414fde9234fb96a91e8.png)

`Embedding -> Attention -> MLPs -> Unembedding`下面是对这四个阶段的简要介绍：

### Embedding（嵌入）

- 模型的输入通常是离散的词或符号（例如，在英文文本中，每个单词或标点符号都是一个符号）。
- `Embedding`层将这些离散的符号转换为连续的、固定大小的向量（即词嵌入）。这些向量捕捉了符号的语义和上下文信息。
- 对于`Transformer`的编码器（`Encoder`）和解码器（`Decoder`），都会有一个`Embedding`层。
- 在解码器中，还有一个额外的位置嵌入（`Positional Embedding`）层，用于捕捉序列中单词的位置信息，因为`Transformer`不依赖于`RNN`或`CNN`等结构来捕获顺序信息。

神经网络算法 - 一文搞懂`Embedding`（嵌入）

### Attention（注意力机制）

- 注意力机制是`Transformer`的核心。它允许模型在生成当前输出时关注输入序列中的不同部分。
- `Transformer`使用了多种注意力机制，包括自注意力（`Self-Attention`）、编码器-解码器注意力（`Encoder-Decoder Attention`）、掩码多头注意力（`Mask Multi-Head Attention`）等。
- 自注意力允许模型在同一序列的不同位置之间建立依赖关系，从而捕获序列的内部结构。
- 编码器-解码器注意力允许模型在生成输出时关注输入序列的特定部分。
- 在注意力计算过程中，会生成一个注意力权重矩阵，该矩阵描述了输入序列中每个位置对当前位置的重要性。

神经网络算法 - 一文搞懂`Transformer`中的三种注意力机制

### MLPs（多层感知机，也称为`FFNN`前馈神经网络）

- 经过注意力机制处理后，模型会通过一个或多个全连接层（也称为前馈网络或`MLPs`）进行进一步的变换和特征提取。
- 这些全连接层可以捕捉输入数据中的非线性关系，并帮助模型学习更复杂的模式。
- 在`Transformer`中，`MLPs`通常位于自注意力层和归一化层之间，形成所谓的“编码器块”或“解码器块”。

神经网络算法 - 一文搞懂`FFNN`（前馈神经网络）

### Unembedding（从模型表示到最终输出）

- 可以将其理解为从模型内部表示转换到最终输出格式的过程。
- 对于文本生成任务，如机器翻译，解码器的输出会经过一个线性层和一个`softmax`函数，以生成一个概率分布，该分布描述了下一个可能输出的词的概率。
- 对于其他任务（如文本分类），解码器的输出可能会直接用于计算损失函数（如交叉熵损失），或者通过其他方式转换为最终的预测结果。

神经网络算法 - 一文搞懂`Softmax`函数
