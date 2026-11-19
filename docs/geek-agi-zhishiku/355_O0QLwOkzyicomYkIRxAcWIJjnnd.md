
# Transformer动画讲解 - 多层感知机

> 来源：[https://geekbang.feishu.cn/docx/HASAdPzgFob51MxvUUPcU0Dcn1c](https://geekbang.feishu.cn/docx/HASAdPzgFob51MxvUUPcU0Dcn1c)

> 👨🏻 作者：AllenTang

> 📃 作者介绍：分享人工智能，让所有人玩转AI

> 🛖 👸号：架构师带你玩转AI

> ⏰ 收录时间：2024-05-23 11:28:42

> 🔗 原文链接：https://mp.weixin.qq.com/s/v3c7dVEAH49A69w3zjeVlg

![](img/e3e2d91a592169fbc571e6c4c41568a7.png)

关注作者👸号，获取更多AI知识

**动画地址：查看原文，可看动画**

## 从端到端的数据流转

从端到端的角度来看，数据在`Transformer`中的流转可以概括为四个阶段：`Embedding`（嵌入）、`Attention`（注意力机制）、`MLPs`（多层感知机）和`Unembedding`（从模型表示到最终输出）。

![](img/15c48bb2158f019981555f4bad51434b.png)

`Embedding` -> `Attention` -> `MLPs` -> `Unembedding`

下面对第三个阶段`MLPs`（多层感知机或前馈网络）进行详细介绍：

## MLPs在Transformer中的位置

### Transformer的编码器和解码器结构

（1）`Transformer`的编码器由多个相同的层堆叠而成，每个层包含两个主要的子层：一个`Multi-Head Self-Attention`机制和一个全连接的前馈神经网络（`MLP`）。

（2）`Transformer`的解码器也由多个相同的层堆叠而成，但每个层包含三个主要的子层：一个`Masked Multi-Head Self-Attention`机制（用于编码器的输出），一个`Multi-Head Encoder-Decoder Attention`机制（用于结合编码器的输出和解码器的当前位置信息），以及一个全连接的前馈神经网络（`MLP`）。

![](img/41c1d42ae988ecc4171b7ab95691df84.png)

`Transformer`的编码器和解码器结构

### MLP在Transformer中的位置

`MLP`位于`Self-Attention`（或`Masked Self-Attention`）和`Multi-Head Attention`层之后。

![](img/ed6903ef1264db4435d8ab0495716a2d.png)

MLP在Transformer中的位置

## MLPs的模型架构

### MLPs模型架构

（1）通常包含两个线性变换层，即一个输入层、一个隐藏层和一个输出层。

（2）在每个线性变换层之间，会应用一个激活函数（如`ReLU`），以增强模型的非线性表示能力。

（3）数学表达式为，`FFN(x) = max(0, xW1 + b1)W2 + b2`。

![](img/a253faa3c86c8b4148b951fa4f965290.png)

**MLP模型架构**

### 神经网络算法 - 一文搞懂`FFNN`（前馈神经网络）

## MLPs在Transformer中的作用

### MLP在Transformer中的作用

（1）非线性变换：`MLPs`通过引入激活函数（如`ReLU`）提供非线性变换，这有助于模型捕获输入数据中的复杂模式。

（2）特征提取与整合：`MLPs`进一步处理和转换注意力机制提取的特征，提取和整合更多有用的信息，使其能够学习更加复杂的函数关系。

### MLP的两个线性变换层的作用

（1）第一个线性变换层：增加了输入的维度，以便网络能够学习更复杂的表示。

（2）第二个线性变换层：将维度还原到与输入相同的大小，通过残差连接和层归一化，将`MLP`的输出与原始输入相加并进行归一化，以确保稳定的训练过程。

![](img/19fa779ff268c2590be7bdb01bc2226d.png)

MLP在Transformer中的作用

## Transformer无限循环操作`Attention` + `MLP`

### `Transformer`操作的本质

`Transformer`操作的本质：海量的矩阵乘法。大模型比拼的就是算力，背后大佬就是万卡集群（`GPU`集群）。

![](img/aae47ad0cc17e72f0a727d1b671642d4.png)

Transformer操作的本质

### 无限循环`Attention` + `MLP`

`Attention`模块和多层感知机（`MLP`）模块之间不断切换，直到输出完全融入到序列的最后一个向量。

![](img/f5fedc4e4d017f6530eeb376888fdf0d.png)

1. 无限循环`Attention` + `MLP`

## 参考资料

+ https://www.3blue1brown.com/lessons/gpt
