
# NLP（五）：Transformer及其attention机制

> 来源：[https://agijuejin.feishu.cn/docx/KnGpdXvSCoqIDQxsMp2cCMmRnIh](https://agijuejin.feishu.cn/docx/KnGpdXvSCoqIDQxsMp2cCMmRnIh)

> 作者：紫气东来

> 原文：[https://zhuanlan.zhihu.com/p/476585349](https://zhuanlan.zhihu.com/p/476585349)

Transformer 模型自 2017 年 6 月在论文《Attention Is All You Need》被提出以来，已经成为 NLP 领域中的首选模型。Transformer 抛弃了 RNN 的顺序结构，采用了 Self-Attention 机制，使得模型可以并行化训练，而且能够充分利用训练资料的全局信息，加入 Transformer 的 Seq2seq 模型在 NLP 的各个任务上都有了显著的提升。本文将试图从多角度更加清晰地讲解 Transformer 的运行原理。

Transformer 由且仅由 self-Attention 和 Feed Forward Neural Network 组成。Transformer中包括了编码器和解码器各 6 层，总共 12 层的 Encoder-Decoder。Transformer 中的核心机制就是 Self-Attention。Self-Attention 机制的本质来自于人类视觉注意力机制。

## attention详解

输入是一个序列，`x_1, x_2, ..., x_n`，进行`embedding`之后形成 `a_1, a_2, ..., a_n`；然后每个 `x_i` 都会经过三个矩阵处理（`WQ`、`WK` 和 `WV`），得到三个向量 `Q`、`V` 和 `K`。

`Q: query (to match others)`, `Q^i = W^Q a^i`
`K: key (to be matched)`, `K^i = W^K a^i`
`V: value (information to be extracted)`, `V^i = W^V a^i`

Attention最核心的公式如下，下面将逐步剖析这一公式的含义

`Attention(Q, K, V) = softmax(QK^T / sqrt(d_k)) V`

![](img/2070d2d639357f10b6f87343f6ee5ed2.png)

### QK^T 的含义

我们都知道，对于向量 `x_i` 和 `x_j`，其内积 `<x_i, x_j>` 表征一个向量在另一个向量上的投影，即两个向量的相似关系。那么对于矩阵 `X`，`XX^T` 是一个方阵，以行向量的角度理解，里面保存了每个向量与自己和其他向量进行内积运算的结果，即与其他向量的相似程度。使用`query`和每个`key`进行计算，得到就是相应的`attention`。

### \sqrt{d_k} 与 softmax

`d_k` 是 `q` 和 `k` 的维度，主要是因为随着维度增加，点乘的结果也会变大，结果太大也会对`softmax`的结果产生影响，所以需要除以一个系数。

下面简要予以证明：

假设向量 `q` 和 `k` 的各个分量是互相独立的随机变量，维度为 `d_k`，均值是 0，方差是 1，即

```
\begin{aligned} 
&\mathrm{E}\left[q_{i}\right]=\mathrm{E}\left[k_{i}\right]=0 \\ 
&\operatorname{var}\left[q_{i}\right]=\operatorname{var}\left[k_{i}\right]=1 
\end{aligned} 
```

其中 `i ∈ [0, d_k]`

则有

```
\begin{aligned} 
\mathrm{E}[q \cdot k] &=\mathrm{E}\left[\sum_{i=1}^{d_{k}} q_{i} k_{i}\right] \\ 
&=\sum_{i=1}^{d_{k}} \mathrm{E}\left[q_{i} k_{i}\right] \\ 
&=\sum_{i=1}^{d_{k}} \mathrm{E}\left[q_{i}\right] \mathrm{E}\left[k_{i}\right] \\ 
&=0 
\end{aligned} 
```

```
\begin{aligned} 
\operatorname{var}[q \cdot k] &=\operatorname{var}\left[\sum_{i=1}^{d_{k}} q_{i} k_{i}\right] \\ 
&=\sum_{i=1}^{d_{k}} \operatorname{var}\left[q_{i} k_{i}\right] \\ 
&=\sum_{i=1}^{d_{k}} \operatorname{var}\left[q_{i}\right] \operatorname{var}\left[k_{i}\right] \\ 
&=\sum_{i=1}^{d_{k}} 1 \\ 
&=d_{k} 
\end{aligned} 
```

则 `var(q · k / sqrt(d_k)) = d_k / (sqrt(d_k))^2 = 1`

将方差控制为 1，也就有效地控制了`softmax`反向的梯度消失的问题。

接着把这些分数经过一个`Softmax`函数，`Softmax`可以将分数归一化，这样使得分数都是正数并且加起来等于 1，这些分数决定了当前词向量，对其他所有位置的词向量分别有多少的注意力。

```
Softmax(z_i) = e^{z_i} / \sum_{c=1}^{C} e^{z_c}
```

### 加权求和

得到每个词向量的分数后，将分数分别与对应的`Value`向量相乘。这种做法背后的直觉理解就是：对于分数高的位置，相乘后的值就越大，我们把更多的注意力放到了它们身上；对于分数低的位置，相乘后的值就越小，这些位置的词可能是相关性不大的。

## 多头模型

由于不同的 `Attention` 的权重侧重点不一样，所以将这个任务交给不同的 `Attention` 一起做，最后取综合结果会更好，有点像 `CNN` 中的 `Kernel`。在Transformer的论文中指出，将 `Q`、`K`、`V` 通过一个线性映射后，分成 `h` 份，对每份进行 `Scaled Dot-Product Attention` 效果更好， 再把这几个部分 `Concat` 起来，过一个线性层的效果更好， 可以综合不同位置的不同表征子空间的信息 。

![](img/619c6a1262cd9a375c6d48f39c21dd9b.png)

即

```
O = MultiHead(Q, K, V) = W^O · Concat([softmax(Q^1(K^1)^T / sqrt(d_m)) V^1; softmax(Q^2(K^2)^T / sqrt(d_m)) V^2])
```

多头`attention`模型的实现：

```
class MultiHead(nn.Module):
    def __init__(self, n_head, model_dim, drop_rate):
        super().__init__()
        self.head_dim = model_dim // n_head
        self.n_head = n_head
        self.model_dim = model_dim
        self.wq = nn.Linear(model_dim, n_head * self.head_dim)
        self.wk = nn.Linear(model_dim, n_head * self.head_dim)
        self.wv = nn.Linear(model_dim, n_head * self.head_dim)

        self.o_dense = nn.Linear(model_dim, model_dim)
        self.o_drop = nn.Dropout(drop_rate)
        self.layer_norm = nn.LayerNorm(model_dim)
        self.attention = None

    def forward(self, q, k, v, mask, training):
        # residual connect
        residual = q

        # linear projection
        key = self.wk(k)  # [n, step, num_heads * head_dim]
        value = self.wv(v)  # [n, step, num_heads * head_dim]
        query = self.wq(q)  # [n, step, num_heads * head_dim]

        # split by head
        query = self.split_heads(query)  # [n, n_head, q_step, h_dim]
        key = self.split_heads(key)
        value = self.split_heads(value)  # [n, h, step, h_dim]
        context = self.scaled_dot_product_attention(query, key, value, mask)  # [n, q_step, h*dv]
        o = self.o_dense(context)  # [n, step, dim]
        o = self.o_drop(o)

        o = self.layer_norm(residual + o)
        return o

    def split_heads(self, x):
        x = torch.reshape(x, (x.shape[0], x.shape[1], self.n_head, self.head_dim))
        return x.permute(0, 2, 1, 3)

    def scaled_dot_product_attention(self, q, k, v, mask=None):
        dk = torch.tensor(k.shape[-1]).type(torch.float)
        score = torch.matmul(q, k.permute(0, 1, 3, 2)) / (torch.sqrt(dk) + 1e-8)  # [n, n_head, step, step]
        if mask is not None:
            # change the value at masked position to negative infinity,
            # so the attention score at these positions after softmax will close to 0.
            score = score.masked_fill_(mask, -np.inf)
        self.attention = softmax(score, dim=-1)
        context = torch.matmul(self.attention, v)  # [n, num_head, step, head_dim]
        context = context.permute(0, 2, 1, 3)  # [n, step, num_head, head_dim]
        context = context.reshape((context.shape[0], context.shape[1], -1))
        return context  # [n, step, model_dim]
```

## 残差连接

编码器的每个子层（`Self Attention`层和`FFN`）都有一个残差连接和层标准化（`layer-normalization`），如下图所示。

![](img/69ae39919da6e0aa782cd95577aa4bb0.png)

```
class PositionWiseFFN(nn.Module):
    def __init__(self, model_dim, dropout=0.0):
        super().__init__()
        dff = model_dim * 4
        self.l = nn.Linear(model_dim, dff)
        self.o = nn.Linear(dff, model_dim)
        self.dropout = nn.Dropout(dropout)
        self.layer_norm = nn.LayerNorm(model_dim)

    def forward(self, x):
        o = relu(self.l(x))
        o = self.o(o)
        o = self.dropout(o)

        o = self.layer_norm(x + o)
        return o  # [n, step, dim]
```

## 编码器(encoder)与解码器(decoder)

`Encoder` 的输入由两部分组成分别是词编码矩阵 `I ∈ R^{n × l × d}` 和位置编码矩阵 `P ∈ R^{n × l × d}` ，其中 `n` 表示句子数目，`l` 表示一句话的`sequence length`，`d` 表示的是词向量的维度。

位置编码矩阵 `P` 表示的是每个单词在一句里的所有位置信息，因为 `Self-Attention` 计算注意力分布的时候只能给出输出向量和输入向量之间的权重关系，但是不能给出词在一句话里的位置信息，所以需要在输入里引入位置编码矩阵。位置编码向量生成方法有很多，常见是利用三角函数对位置进行编码。

输入矩阵 `I + P` 通过线性变换生成矩阵 `Q`、`K`、`V` 。在实际编程中是将输入 `I + P` 直接赋值给 `Q`、`K`、`V` 。如果输入单词长度小于最大长度并来填充 0 的时候，还要相应引入 `Mask` 矩阵。

```
class EncoderLayer(nn.Module):

    def __init__(self, n_head, emb_dim, drop_rate):
        super().__init__()
        self.mh = MultiHead(n_head, emb_dim, drop_rate)
        self.ffn = PositionWiseFFN(emb_dim, drop_rate)

    def forward(self, xz, training, mask):
        # xz: [n, step, emb_dim]
        context = self.mh(xz, xz, xz, mask, training)  # [n, step, emb_dim]
        o = self.ffn(context)
        return o
```

`Decoder` 的输入也由两部分组成分别是词编码矩阵 `O ∈ R^{n_1 × l_1 × d}` 和位置编码矩阵 `P^O ∈ R^{n_1 × l_1 × d}` 。因为 `Decoder` 的输入是具有时顺序关系的（即上一步的输出为当前步输入）所以还需要输入 `Mask` 矩阵 `M` 以便计算注意力分布。

解码（`decoding`）阶段的每一个时间步都输出一个翻译后的单词（这里的例子是英语翻译），解码器当前时间步的输出又重新作为输入 `Q` 和编码器的输出 `K`、`V` 共同作为下一个时间步解码器的输入。然后重复这个过程，直到输出一个结束符。



# Transformer 解码器

```
class DecoderLayer(nn.Module):
    def __init__(self, n_head, model_dim, drop_rate):
        super().__init__()
        self.mh = nn.ModuleList([MultiHead(n_head, model_dim, drop_rate) for _ in range(2)])
        self.ffn = PositionWiseFFN(model_dim, drop_rate)

    def forward(self, yz, xz, training, yz_look_ahead_mask, xz_pad_mask):
        dec_output = self.mh[0](yz, yz, yz, yz_look_ahead_mask, training)  # [n, step, model_dim]
        dec_output = self.mh[1](dec_output, xz, xz, xz_pad_mask, training)  # [n, step, model_dim]
        dec_output = self.ffn(dec_output)  # [n, step, model_dim]

        return dec_output
```

![](img/ef0a07362d5ae3614580a9beff3f256b.png)

## 解码器中的 Self Attention 层

解码器中的 `Self Attention` 层，和编码器中的 `Self Attention` 层的区别：

1. 在解码器里，`Self Attention` 层只允许关注到输出序列中早于当前位置之前的单词。具体做法是：在 `Self Attention` 分数经过 `Softmax` 层之前，屏蔽当前位置之后的那些位置（将 `attention score` 设置成 `-inf`）。

2. 解码器 `Attention` 层是使用前一层的输出来构造 `Query` 矩阵，而 `Key` 矩阵和 `Value` 矩阵来自于编码器最终的输出。

## 线性层和 `softmax`

`Decoder` 最终的输出是一个向量，其中每个元素是浮点数。我们怎么把这个向量转换为单词呢？这是线性层和 `softmax` 完成的。线性层就是一个普通的全连接神经网络，可以把解码器输出的向量，映射到一个更大的向量，这个向称为 `logits` 向量：假设我们的模型有 10000 个英语单词（模型的输出词汇表），此 `logits` 向量便会有 10000 个数字，每个数表示一个单词的分数。然后，`Softmax` 层会把这些分数转换为概率（把所有的分数转换为正数，并且加起来等于 1）。然后选择最高概率的那个数字对应的词，就是这个时间步的输出单词。

![](img/ca15d8eded23809b76cd57233049ad92.png)

## 损失函数

`Transformer` 训练的时候，需要将解码器的输出和 `label` 一同送入损失函数，以获得 `loss`，最终模型根据 `loss` 进行方向传播。我们用一个简单的例子来说明训练过程的 `loss` 计算：把“merci”翻译为“thanks”。

我们希望模型解码器最终输出的概率分布，会指向单词 ”thanks“（在“thanks”这个词的概率最高）。但是，一开始模型还没训练好，它输出的概率分布可能和我们希望的概率分布相差甚远，如下图所示，正确的概率分布应该是“thanks”单词的概率最大。但是，由于模型的参数都是随机初始化的，所示一开始模型预测所有词的概率几乎都是随机的。

![](img/199db110c3a4046049aabef54da7445e.png)

只要 `Transformer` 解码器预测了组概率，我们就可以把这组概率和正确的输出概率做对比，然后使用反向传播来调整模型的权重，使得输出的概率分布更加接近整数输出。

那我们要怎么比较两个概率分布呢？：我们可以简单的用两组概率向量的的空间距离作为 `loss`（向量相减，然后求平方和，再开方），当然也可以使用交叉熵(`cross-entropy`)和 KL 散度(`Kullback–Leibler divergence`)。

![](img/ab8d3a0fabe60982aed332d7e20d9ab0.png)

我们用例子中的句子训练模型，希望产生图中所示的概率分布我们的模型在一个足够大的数据集上，经过足够长时间的训练后，希望输出的概率分布如下图所示：

![](img/8e5ca3d38d12d5581c28f69fc3e91035.png)

我们希望模型经过训练之后可以输出的概率分布也就对应了正确的翻译。当然，如果你要翻译的句子是训练集中的一部分，那输出的结果并不能说明什么。我们希望模型在没见过的句子上也能够准确翻译。

额外提一下 `greedy decoding` 和 `beam search` 的概念：

+ Greedy decoding：由于模型每个时间步只产生一个输出，我们这样看待：模型是从概率分布中选择概率最大的词，并且丢弃其他词。这种方法叫做贪婪解码（greedy decoding）。

+ Beam search：每个时间步保留 `k` 个最高概率的输出词，然后在下一个时间步，根据上一个时间步保留的 `k` 个词来确定当前应该保留哪 `k` 个词。假设 `k=2`，第一个位置概率最高的两个输出的词是”I“和”a“，这两个词都保留，然后根据第一个词计算第2个位置的词的概率分布，再取出第2个位置上2个概率最高的词。对于第3个位置和第4个位置，我们也重复这个过程。这种方法称为集束搜索（beam search）。

## 参考资料

[1] Attention Is All You Need

[2] [自然语言处理6：Transfor](https://zhuanlan.zhihu.com/p/137615798)mer - 知乎 (zhihu.com)

[3] [T](https://link.zhihu.com/?target=https%3A//mofanpy.com/tutorials/machine-learning/nlp/transformer/)ransformer 将注意力发挥到极致 - 自然语言处理 | 莫烦Python (mofanpy.com)

[4] 矩阵视角下的Transformer详解（附代码）

[5] 超详细图解Self-Attention的那些事儿

> 至人之用心若镜，不将不逆，应而不藏，故能胜物而不伤。 —— 《庄子·应帝王》
