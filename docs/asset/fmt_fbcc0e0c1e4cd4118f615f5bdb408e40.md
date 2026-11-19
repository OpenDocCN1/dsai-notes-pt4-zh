
## Transformer Encoder block

红色部分是 `Transformer` 的 `Encoder block` 结构，可以看到是由 `Multi-Head Attention`、`Add & Norm`、`Feed Forward`、`Add & Norm` 组成的。刚刚已经了解了 `Multi-Head Attention` 的计算过程，现在了解一下 `Add & Norm` 和 `Feed Forward` 部分。

![](img/3f68ca737e6efa11a6954aa9efe2782a.png)

Transformer Encoder block

### Add & Norm

`Add & Norm` 层由 `Add` 和 `Norm` 两部分组成，其计算公式如下：

![](img/b63c679a31ec49d550dd4c5edd1f3e2d.png)

`Add & Norm` 公式

其中 `X` 表示 `Multi-Head Attention` 或者 `Feed Forward` 的输入，`MultiHeadAttention(X)` 和 `FeedForward(X)` 表示输出（输出与输入 `X` 维度是一样的，所以可以相加）

`Add` 指 `X+MultiHeadAttention(X)`，是一种残差连接，通常用于解决多层网络训练的问题，可以让网络只关注当前差异的部分，在 `ResNet` 中经常用到：

![](img/39224ebd86e8d85f052851cd0fb49025.png)

残差连接

`Norm` 指 `Layer Normalization`，通常用于 `RNN` 结构，`Layer Normalization` 会将每一层神经元的输入都转成均值方差都一样的，这样可以加快收敛。

### Feed Forward

`Feed Forward` 层比较简单，是一个两层的全连接层，第一层的激活函数为 `Relu`，第二层不使用激活函数，对应的公式如下。

![](img/5cdb55b40a662752d170d3b824172806.png)

Feed Forward

`X` 是输入，`Feed Forward` 最终得到的输出矩阵的维度与 `X` 一致。

### 组成 Encoder

> 通过上面描述的 `Multi-Head Attention`、`Feed Forward`、`Add & Norm` 就可以构造出一个 `Encoder block`，`Encoder block` 接收输入矩阵，并输出一个矩阵。通过多个 `Encoder block` 叠加就可以组成 `Encoder`。

+ 第一个 `Encoder block` 的输入为句子单词的表示向量矩阵，
+ 后续 `Encoder block` 的输入是前一个 `Encoder block` 的输出
+ 最后一个 `Encoder block` 输出的矩阵就是编码信息矩阵 `C`，这一矩阵后续会用到 `Decoder` 中。

![](img/8dd934fc5ad56d94975746bce5d53350.png)

## Decoder 结构

红色部分为 `Transformer` 的 `Decoder block` 结构，与 `Encoder block` 相似，但是存在一些区别：

+ 包含两个 `Multi-Head Attention` 层。
+ 第一个 `Multi-Head Attention` 层采用了 `Masked` 操作。
+ 第二个 `Multi-Head Attention` 层的 `K`, `V` 矩阵使用 `Encoder` 的编码信息矩阵 `C` 进行计算，而 `Q` 使用上一个 `Decoder block` 的输出计算。
+ 最后有一个 `Softmax` 层计算下一个翻译单词的概率。

![](img/e274a9427ec62be8d138e2995734decb.png)

Transformer Decoder block

### 第一个 Multi-Head Attention

> `Decoder block` 的第一个 `Multi-Head Attention` 采用了 `Masked` 操作，因为在翻译的过程中是顺序翻译的，即翻译完第 `i` 个单词，才可以翻译第 `i+1` 个单词。通过 `Masked` 操作可以防止第 `i` 个单词知道 `i+1` 个单词之后的信息。下面以 "我有一只猫" 翻译成 "I have a cat" 为例，了解一下 `Masked` 操作。

+ 在 `Decoder` 的时候，是需要根据之前的翻译，求解当前最有可能的翻译，如下图所示。首先根据输入 `<Begin>` 预测出第一个单词为 "I"，然后根据输入 `<Begin> I` 预测下一个单词 "have"。

![](img/7e213b154ed9ba9035f1aee15d593b33.png)

Decoder 预测

+ `Decoder` 可以在训练的过程中使用 `Teacher Forcing` 并且并行化训练，即将正确的单词序列 (`<Begin> I have a cat`) 和对应输出 (`I have a cat <end>`) 传递到 `Decoder`。那么在预测第 `i` 个输出时，就要将第 `i+1` 之后的单词掩盖住，

+ 注意 `Mask` 操作是在 `Self-Attention` 的 `Softmax` 之前使用的，下面用 0 1 2 3 4 5 分别表示 `<Begin> I have a cat <end>`。

**第一步**：

1. `Decoder` 的输入矩阵和 `Mask` 矩阵，输入矩阵包含 `<Begin> I have a cat` (0, 1, 2, 3, 4) 五个单词的表示向量，`Mask` 是一个 5×5 的矩阵。在 `Mask` 可以发现单词 0 只能使用单词 0 的信息，而单词 1 可以使用单词 0, 1 的信息，即只能使用之前的信息。

![](img/52739d5c63363b45954113124eb04c60.png)

输入矩阵与 Mask 矩阵

**第二步**：

1. 接下来的操作和之前的 `Self-Attention` 一样，通过输入矩阵 `X` 计算得到 `Q`、`K`、`V` 矩阵。然后计算 `Q` 和

![](img/8f92f5626b04b8de8e235f2766a28ed6.png)

Q乘以K的转置

**第三步**：

1. 在得到之后需要进行 `Softmax`，计算 attention score，我们在 `Softmax` 之前需要使用 `Mask` 矩阵遮挡住每一个单词之后的信息，遮挡操作如下：

![](img/a6b02a35e0ea4f7ad3d6693bcbe6a569.png)

Softmax 之前 Mask

得到 `Mask` 之后在 `Mask` 上进行 `Softmax`，每一行的和都为 1。但是单词 0 在单词 1, 2, 3, 4 上的 attention score 都为 0。

**第四步**：

1. 使用 `Mask` 与矩阵 `V` 相乘，得到输出 `Z`，则单词 1 的输出向量是只包含单词 1 信息的。

**第五步**：

1. 通过上述步骤就可以得到一个 `Mask Self-Attention` 的输出矩阵，然后和 `Encoder` 类似，通过 `Multi-Head Attention` 拼接多个输出，然后计算得到第一个 `Multi-Head Attention` 的输出 `Z`，`Z` 与输入 `X` 维度一样。

### 第二个 Multi-Head Attention

+ `Decoder block` 第二个 `Multi-Head Attention` 变化不大，主要的区别在于其中 `Self-Attention` 的 `K`, `V` 矩阵不是使用上一个 `Decoder block` 的输出计算的，而是使用 `Encoder` 的编码信息矩阵 `C` 计算的。

+ 根据 `Encoder` 的输出 `C` 计算得到 `K`, `V`，根据上一个 `Decoder block` 的输出 `Z` 计算 `Q`（如果是第一个 `Decoder block` 则使用输入矩阵 `X` 进行计算），后续的计算方法与之前描述的一致。

+ 这样做的好处是在 `Decoder` 的时候，每一位单词都可以利用到 `Encoder` 所有单词的信息（这些信息无需 `Mask`）。

### Softmax 预测输出单词

`Decoder block` 最后的部分是利用 `Softmax` 预测下一个单词，在之前的网络层我们可以得到一个最终的输出 `Z`，因为 `Mask` 的存在，使得单词 0 的输出 `Z0` 只包含单词 0 的信息，如下：

![](img/650be784fcf4943ad235da71cda742fb.png)

Decoder Softmax 之前的 Z

`Softmax` 根据输出矩阵的每一行预测下一个单词：

![](img/0452732c98240506550230f6dc7fbfcf.png)

Decoder Softmax 预测

这就是 `Decoder block` 的定义，与 `Encoder` 一样，`Decoder` 是由多个 `Decoder block` 组合而成。

## Transformer 总结

+ `Transformer` 与 `RNN` 不同，可以比较好地并行训练。
+ `Transformer` 本身是不能利用单词的顺序信息的，因此需要在输入中添加位置 `Embedding`，否则 `Transformer` 就是一个词袋模型了。
+ `Transformer` 的重点是 `Self-Attention` 结构，其中用到的 `Q`, `K`, `V` 矩阵通过输出进行线性变换得到。
+ `Transformer` 中 `Multi-Head Attention` 中有多个 `Self-Attention`，可以捕获单词之间多种维度上的相关系数 attention score。
