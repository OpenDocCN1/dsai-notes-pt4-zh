
# 大厂LLM面试必考题

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/OnUBdreIeofMdVxfvr2cLSp0neg](https://qqs7y1hozd1.feishu.cn/docx/OnUBdreIeofMdVxfvr2cLSp0neg)

## `Transformer`的优缺点

### 答：

+ 优点：位置关联操作不受限，建模能力强，通用性和可扩展性强，能更好地进行并行运算
+ 缺点：局部信息获取不强，位置信息编码问题，顶层梯度消失

## `Encoder`端和`Decoder`端是如何进行交互的？

### 答：

+ `Cross Self-attention`、`Decoder`提供`Q`、`Encoder`提供`K`、`V`

## 为什么需要`multi-head attention`？

### 答：

+ 将模型分为多个头，形成多个子空间，可以关注不同方面的信息，类似于`CNN`的多个卷积核
+ 为了解决模型在对当前位置的信息进行编码时，会过度的将注意力集中于自身位置的问题
+ 一定程度上`h`越大，整个模型的表达能力越强，越能提高模型对于注意力权重的合理分配

## `QK`为什么要除以`dk`来进行`Scale`？

### 答：

+ 对于较大的`dk`来说，`QK`会得到极大的点积值，将整个`softmax`推向梯度平缓区，收敛困难，不利于网络的训练
+ 假设`Q`和`K`的均值为0，方差为1。它们的矩阵乘积将有均值为0，方差为`dk`，因此用`dk`的平方根被用于缩放，因为，`Q`和`K`的矩阵乘积的均值本应该为0，方差本应该为1，这样可以获得更平缓的`softmax`。当维度很大时，点积结果会很大，会导致`softmax`的梯度很小；为了减轻这个影响，对点积进行缩放

## 为什么使用不同的`K`和`Q`，为什么不能使用同一个值？

### 答：

+ `K`和`Q`的点乘是为了得到一个`attention score`矩阵，用来对`V`进行提纯
+ `K`和`Q`使用了不同的，是在不同空间上的投影，增强了表达能力，提高泛化能力
+ 如果不用`Q`，直接拿`K`和`K`点乘的话，`attention score`是一个对称矩阵。因为同一个矩阵都投影到了同样一个空间，所以泛化能力很差

## 在计算`attention score`的时候如何对`padding`做`mask`操作？

### 答：

+ `padding mask`：所有`scaled dot-product attention`都用到，因为一个`batch`输入中，所有序列的长度不同。为了符合模型的输入方式，会用`padding`的方式来填充（比如填0），使所有序列的长度一致。但填充部分是没有意义的，所以在计算注意力的时候，不需要也不应该有注意力分配到这些填充的值上面。`padding`位置置为负无穷（一般来说`-1000`就可以）
+ `sequence mask`：在`Decoder`的`self-attention`里面用到。在`Decoder`部分，因为不能见到下文信息（防止泄漏），所以用`mask`的方式掩盖掉当前时刻`t`及之后的下文信息。具体，可产生一个对角线为0的上三角矩阵，将其作用到每个`Decoder`的输入列上

## `self-attention`是什么？

### 答：

+ `self-attention`的特点在于无视`token`之间的距离直接计算依赖关系，从而学习序列关系

## 关于 `self-attention` 为什么它能发挥如此大的作用？

### 答：

+ 强大的特征抽取能力，更好的`representation`来表达自身，并行计算

## `Transformer` 中的`Add & Norm` 模块，具体是怎么做的？

### 答：

`Layer Normalization`，公式其实是常见的归一化方式：`(x-mu)/sigma`

## 简单介绍一下`Transformer`的位置编码？有什么意义和优缺点？

### 答：

+ 因为`self-attention`是位置无关的，无论句子的顺序是什么样的，通过`self-attention`计算的`token`的`hidden embedding`都是一样的，这显然不符合人类的思维
+ 因此要有一个办法能够在模型中表达出一个`token`的位置信息，`Transformer`使用了固定的`positional encoding`来表示`token`在句子中的绝对位置信息

## 为什么`Transformer`块使用`LayerNorm`而不是`BatchNorm`？

### 答：

+ 正则化的本质是通过把一部分不重要的复杂信息损失掉，以此来降低拟合难度以及过拟合的风险，`Normalization`的目的是降低各维度数据的方差
+ CV用`BN`是认为不同卷积核`feature map`（`channel`维）之间的差异性很重要，`LN`会损失`channel`的差异性，对于`batch`内的不同样本，同一卷积核提取特征的目的性是一致的，所以使用`BN`仅是为了进一步保证同一个卷积核在不同样本上提取特征的稳定性。
+ NLP用`LN`是认为`batch`内不同样本同一位置`token`之间的差异性更重要，而`embedding`维，网络对于不同`token`提取的特征目的性是一致的，使用`LN`是为了进一步保证在不同`token`上提取的稳定性。NLP每个序列的长度是不一致的，`BN`不适用

## `Decoder`阶段的多头自注意力和`encoder`的多头自注意力有什么区别？

### 答：

+ `Decoder`有两层`mha`，`encoder`有一层`mha`，`Decoder`的第二层`mha`是为了转化输入与输出句长，`Decoder`的请求`q`与键`k`和数值`v`的倒数第二个维度可以不一样，但是`encoder`的`qkv`维度一样

## `Transformer attention`的注意力矩阵的计算为什么用乘法而不是加法？

### 答：

+ 为了计算更快。加法形式是先加、后`tanh`、再和`V`矩阵相乘，相当于一个完整的隐层。在计算复杂度上，乘法和加法理论上的复杂度相似，但在实践中，乘法可以利用高度优化的矩阵乘法代码（有成熟的加速实现）使得点乘速度更快，空间利用率更高
+ 在`dk`较小的时候，加法和乘法形式效果相近。但是随着`dk`增大，加法开始显著优于乘法。作者认为，`dk`增大导致乘法性能不佳的原因，是极大的点乘值将整个`softmax`推向梯度平缓区，使得收敛困难。于是选择`scale`

## `Transformer`的残差结构及意义

### 答：

+ 解决梯度消失，防止过拟合

## `Transformer`的并行化体现在哪里，`Decoder`可以做并行化嘛？

### 答：

+ `Encoder`的模块是串行的，子模块`Multi-Head`和`FFN`是并行，因为单词之间没有依赖关系
+ `Decoder`引入`sequence mask`就是为了并行化训练，推理过程不并行
