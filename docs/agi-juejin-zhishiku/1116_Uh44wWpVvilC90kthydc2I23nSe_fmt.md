
# 万字长文说明白`Transformer`

> 来源：[https://agijuejin.feishu.cn/docx/TCYgdNef9otqQ1x1ksycoJmrn6c](https://agijuejin.feishu.cn/docx/TCYgdNef9otqQ1x1ksycoJmrn6c)

> 作者：爱吃牛油果的璐璐

> 原文：https://zhuanlan.zhihu.com/p/657456977

在全面解析`Transformer`网络结构之前，我们先来回顾一下之前介绍的几篇文章：

- 爱吃牛油果的璐璐：一文带你学会 `Attention`
- 爱吃牛油果的璐璐：如何训练你的`BERT`
- 爱吃牛油果的璐璐：`Transformer` 的结构

## 一句话简介

`Transformer` 是第一个完全依赖 自注意力（`self-attention`）来计算输入和输出的表示，而不使用序列对齐的递归神经网络或卷积神经网络的转换模型。

## 输入编码(`BPE`,`PE`)剖析

[爱吃牛油果的璐璐：`Transformer`结构-输入编码(`BPE`,`PE`)剖析](https://zhuanlan.zhihu.com/p/667635031)

## `Transformer`网络结构

![](img/778745b56c29b497664f69b8725fcdff.png)

`Transformer` 是大模型，除了一些特例（如 `DistilBERT`）外，实现更好性能的一般策略是增加模型的大小以及预训练的数据量。其中，`GPT-2` 是使用「`Transformer` 解码器模块」构建的，而 `BERT` 则是通过「`Transformer` 编码器」模块构建的。

### 嵌入表示层以及`Python`实现

对于输入文本序列，首先通过输入嵌入层（`Input Embedding`）将每个单词转换为其相对应的向量表示。通常直接对每个单词创建一个向量表示。由于`Transformer`模型不再使用基于循环的方式建模文本输入，序列中不再有任何信息能够提示模型单词之间的相对位置关系。在送入编码器端建模其上下文语义之前，一个非常重要的操作是在词嵌入中加入位置编码（`Positional Encoding`）这一特征。具体来说，序列中每一个单词所在的位置都对应一个向量。这一向量会与单词表示对应相加并送入到后续模块中做进一步处理。在训练的过程当中，模型会自动地学习到如何利用这部分位置信息。

为了得到不同位置对应的编码，`Transformer`模型使用不同频率的正余弦函数如下所示：

![](img/7be311e84fbd92ae5b10c9e110c0ba98.png)

通过上面这种方式计算位置编码有这样几个好处：首先，正余弦函数的范围是在[-1,+1]，导出的位置编码与原词嵌入相加不会使得结果偏离过远而破坏原有单词的语义信息。其次，依据三角函数的基本性质，可以得知第`pos+k` 个位置的编码是第`pos`个位置的编码的线性组合，这就意味着位置编码中蕴含着单词之间的距离信息。

```
class PositionalEncoder(nn.Module):

    def __init__(self, d_model, max_seq_len=80):
        super().__init__()
        self.d_model = d_model
        # 根据 pos 和 i 创建一个常量 PE 矩阵
        pe = torch.zeros(max_seq_len, d_model)
        for pos in range(max_seq_len):
            for i in range(0, d_model, 2):
                pe[pos, i] = math.sin(pos / (10000**((2 * i) / d_model)))
                pe[pos,
                   i + 1] = math.cos(pos / (10000**((2 * (i + 1)) / d_model)))
        pe = pe.unsqueeze(0)
        self.register_buffer('pe', pe)

    def forward(self, x):
        # 使得单词嵌入表示相对大一些
        x = x * math.sqrt(self.d_model)
        # 增加位置常量到单词嵌入表示中
        seq_len = x.size(1)
        x = x + Variable(self.pe[:, :seq_len], requires_grad=False).cuda()
        return x
```

### 注意力层及实现

自注意力（`Self-Attention`）操作是基于`Transformer`的机器翻译模型的基本操作，在源语言的编码和目标语言的生成中频繁地被使用以建模源语言、目标语言任意两个单词之间的依赖关系。给定由单词语义嵌入及其位置编码叠加得到的输入表示，为了实现对上下文语义依赖的建模，进一步引入在自注意力机制中涉及到的三个元素：查询（`Query`），键（`Key`），值（`Value`）。在编码输入序列中每一个单词的表示的过程中，这三个元素用于计算上下文单词所对应的权重得分。直观地说，这些权重反映了在编码当前单词的表示时，对于上下文不同部分所需要的关注程度。

```
class MultiHeadAttention(nn.Module):

    def __init__(self, heads, d_model, dropout=0.1):
        super().__init__()
        self.d_model = d_model
        self.d_k = d_model // heads
        self.h = heads
        self.q_linear = nn.Linear(d_model, d_model)
        self.v_linear = nn.Linear(d_model, d_model)
        self.k_linear = nn.Linear(d_model, d_model)
        self.dropout = nn.Dropout(dropout)
        self.out = nn.Linear(d_model, d_model)

    def attention(q, k, v, d_k, mask=None, dropout=None):
        scores = torch.matmul(q, k.transpose(-2, -1)) / math.sqrt(d_k)
        # 掩盖掉那些为了填补长度增加的单元，使其通过 softmax 计算后为 0
        if mask is not None:
            mask = mask.unsqueeze(1)
            scores = scores.masked_fill(mask == 0, -1e9)
        scores = F.softmax(scores, dim=-1)
        if dropout is not None:
            scores = dropout(scores)
        output = torch.matmul(scores, v)
        return output

    def forward(self, q, k, v, mask=None):
        bs = q.size(0)
        # 进行线性操作划分为成 h 个头
        k = self.k_linear(k).view(bs, -1, self.h, self.d_k)
        q = self.q_linear(q).view(bs, -1, self.h, self.d_k)
        v = self.v_linear(v).view(bs, -1, self.h, self.d_k)
        # 矩阵转置
        k = k.transpose(1, 2)
        q = q.transpose(1, 2)
        v = v.transpose(1, 2)
        # 计算 attention
        scores = attention(q, k, v, self.d_k, mask, self.dropout)
        # 连接多个头并输入到最后的线性层
        concat = scores.transpose(1, 2).contiguous().view(bs, -1, self.d_model)
        output = self.out(concat)
        return output
```

### 前馈层及实现

前馈层接受自注意力子层的输出作为输入，并通过一个带有 `Relu` 激活函数的两层全连接网络对输入进行更加复杂的非线性变换。实验证明，这一非线性变换会对模型最终的性能产生十分重要的影响。

实验结果表明，增大前馈子层隐状态的维度有利于提升最终翻译结果的质量，因此，前馈子层隐状态的维度一般比自注意力子层要大。

```
class FeedForward(nn.Module):

    def __init__(self, d_model, d_ff=2048, dropout=0.1):
        super().__init__()
        # d_ff 默认设置为 2048
        self.linear_1 = nn.Linear(d_model, d_ff)
        self.dropout = nn.Dropout(dropout)
        self.linear_2 = nn.Linear(d_ff, d_model)

    def forward(self, x):
        x = self.dropout(F.relu(self.linear_1(x)))
        x = self.linear_2(x)
```

### 残差连接与层归一化实现

由 `Transformer` 结构组成的网络结构通常都是非常庞大。编码器和解码器均由很多层基本的`Transformer`块组成，每一层当中都包含复杂的非线性映射，这就导致模型的训练比较困难。因此，研究者们在 `Transformer` 块中进一步引入了残差连接与层归一化技术以进一步提升训练的稳定性。具体来说，残差连接主要是指使用一条直连通道直接将对应子层的输入连接到输出上去，从而避免由于网络过深在优化过程中潜在的梯度消失问题。

```
class NormLayer(nn.Module):

    def __init__(self, d_model, eps=1e-6):
        super().__init__()
        self.size = d_model
        # 层归一化包含两个可以学习的参数
        self.alpha = nn.Parameter(torch.ones(self.size))
        self.bias = nn.Parameter(torch.zeros(self.size))
        self.eps = eps

    def forward(self, x):
        norm = self.alpha * (x - x.mean(dim=-1, keepdim=True)) \
        / (x.std(dim=-1, keepdim=True) + self.eps) + self.bias
        return norm
```

### 编码器和解码器结构及实现

基于上述模块，根据`Transformer`的网络架构，编码器端可以较为容易实现，只需要牢牢掌握 自注意力机制 和 多头自注意力机制 就可以了，编码器结构如下：

![](img/8eb78aabbb86f7b69feee7759ec74f6c.png)

相比于编码器端，解码器端要更复杂一些。具体来说，解码器的每个`Transformer`块的第一个自注意力子层额外增加了 注意力掩码 ，对应图中的 `Masked Multi-Head Attention` 部分，解码器结构如下：

![](img/935ebbe7f348b9607595862447ef95bc.png)

这主要是因为在翻译的过程中，编码器端主要用于编码源语言序列的信息，而这个序列是完全已知的，因而编码器仅需要考虑如何融合上下文语义信息即可。

而解码端则负责生成目标语言序列，这一生成过程是自回归的，即对于每一个单词的生成过程，仅有当前单词之前的目标语言序列是可以被观测的，因此这一额外增加的掩码是用来掩盖后续的文本信息，以防模型在训练阶段直接看到后续的文本序列进而无法得到有效地训练。

换句话说，在`Transformer`推理时，是一个词一个词的输出，但在训练时这样做效率太低了，所以我们会将`target`一次性给到`Transformer`，通过对`target`进行掩码，防止其看到后面的信息，效果等同于一个一个词给解码器。

此外，解码器端还额外增加了一个多头注意力（`Multi-Head Attention`）模块，使用交叉注意力（`Cross-attention`）方法，同时接收来自编码器端的输出以及当前`Transformer`块的前一个掩码注意力层的输出。查询是通过解码器前一层的输出进行投影的，而键和值是使用编码器的输出进行投影的。它的作用是在翻译的过程当中，为了生成合理的目标语言序列需要观测待翻译的源语言序列是什么。基于上述的编码器和解码器结构，待翻译的源语言文本，首先经过编码器端的每个`Transformer`块对其上下文语义的层层抽象，最终输出每一个源语言单词上下文相关的表示。解码器端以自回归的方式生成目标语言文本，即在每个时间步 `t`，根据编码器端输出的源语言文本表示，以及前 `t − 1` 个时刻生成的目标语言文本，生成当前时刻的目标语言单词。



```
class EncoderLayer(nn.Module):

    def __init__(self, d_model, heads, dropout=0.1):
        super().__init__()
        self.norm_1 = Norm(d_model)
        self.norm_2 = Norm(d_model)
        self.attn = MultiHeadAttention(heads, d_model, dropout=dropout)
        self.ff = FeedForward(d_model, dropout=dropout)
        self.dropout_1 = nn.Dropout(dropout)
        self.dropout_2 = nn.Dropout(dropout)

    def forward(self, x, mask):
        x2 = self.norm_1(x)
        x = x + self.dropout_1(self.attn(x2, x2, x2, mask))
        x2 = self.norm_2(x)
        x = x + self.dropout_2(self.ff(x2))
        return x


class Encoder(nn.Module):

    def __init__(self, vocab_size, d_model, N, heads, dropout):
        super().__init__()
        self.N = N
        self.embed = Embedder(vocab_size, d_model)
        self.pe = PositionalEncoder(d_model, dropout=dropout)
        self.layers = get_clones(EncoderLayer(d_model, heads, dropout), N)
        self.norm = Norm(d_model)

    def forward(self, src, mask):
        x = self.embed(src)
        x = self.pe(x)
        for i in range(self.N):
            x = self.layers[i](x, mask)
        return self.norm(x)


class DecoderLayer(nn.Module):

    def __init__(self, d_model, heads, dropout=0.1):
        super().__init__()
        self.norm_1 = Norm(d_model)
        self.norm_2 = Norm(d_model)
        self.norm_3 = Norm(d_model)
        self.dropout_1 = nn.Dropout(dropout)
        self.dropout_2 = nn.Dropout(dropout)
        self.dropout_3 = nn.Dropout(dropout)
        self.attn_1 = MultiHeadAttention(heads, d_model, dropout=dropout)
        self.attn_2 = MultiHeadAttention(heads, d_model, dropout=dropout)
        self.ff = FeedForward(d_model, dropout=dropout)

    def forward(self, x, e_outputs, src_mask, trg_mask):
        x2 = self.norm_1(x)
        x = x + self.dropout_1(self.attn_1(x2, x2, x2, trg_mask))
        x2 = self.norm_2(x)
        x = x + self.dropout_2(self.attn_2(x2, e_outputs, e_outputs, \
         src_mask))
        x2 = self.norm_3(x)
        x = x + self.dropout_3(self.ff(x2))
        return x


class Decoder(nn.Module):

    def __init__(self, vocab_size, d_model, N, heads, dropout):
        super().__init__()
        self.N = N
        self.embed = Embedder(vocab_size, d_model)
        self.pe = PositionalEncoder(d_model, dropout=dropout)
        self.layers = get_clones(DecoderLayer(d_model, heads, dropout), N)
        self.norm = Norm(d_model)

    def forward(self, trg, e_outputs, src_mask, trg_mask):
        x = self.embed(trg)
        x = self.pe(x)
        for i in range(self.N):
            x = self.layers[i](x, e_outputs, src_mask, trg_mask)
        return self.norm(x)
```

## 整体实现及测试训练

基于上面的代码，整体实现如下：

```
class Transformer(nn.Module):

    def __init__(self, src_vocab, trg_vocab, d_model, N, heads, dropout):
        super().__init__()
        self.encoder = Encoder(src_vocab, d_model, N, heads, dropout)
        self.decoder = Decoder(trg_vocab, d_model, N, heads, dropout)
        self.out = nn.Linear(d_model, trg_vocab)

    def forward(self, src, trg, src_mask, trg_mask):
        e_outputs = self.encoder(src, src_mask)
        d_output = self.decoder(trg, e_outputs, src_mask, trg_mask)
        output = self.out(d_output)
        return output
```

训练过程如下：

```
# 模型参数定义
d_model = 512
heads = 8
N = 6
src_vocab = len(EN_TEXT.vocab)
trg_vocab = len(FR_TEXT.vocab)
model = Transformer(src_vocab, trg_vocab, d_model, N, heads)
for p in model.parameters():
        if p.dim() > 1:
                nn.init.xavier_uniform_(p)
optim = torch.optim.Adam(model.parameters(), lr=0.0001, betas=(0.9, 0.98), eps=1e-9)
# 模型训练
def train_model(epochs, print_every=100):
        model.train()
        start = time.time()
        temp = start
        total_loss = 0
        for epoch in range(epochs):
                for i, batch in enumerate(train_iter):
                        src = batch.English.transpose(0,1)
            trg = batch.French.transpose(0,1)
            # the French sentence we input has all words except
                        # the last, as it is using each word to predict the next
                        trg_input = trg[:, :-1]
                        # the words we are trying to predict
                        targets = trg[:, 1:].contiguous().view(-1)
                        # create function to make masks using mask code above
                        src_mask, trg_mask = create_masks(src, trg_input)
                        preds = model(src, trg_input, src_mask, trg_mask)
                        optim.zero_grad()
                        loss = F.cross_entropy(preds.view(-1, preds.size(-1)),results, ignore_index=target_pad)
                        loss.backward()
                        optim.step()
                        total_loss += loss.data[0]
                        if (i + 1) % print_every == 0:
                      loss_avg = total_loss / print_every
                                print("time = %dm, epoch %d, iter = %d, loss = %.3f,%ds per %d iters" % ((time.time() - start) // 60,epoch + 1, i + 1, loss_avg, time.time() - temp,print_every))
                                total_loss = 0
                                temp = time.time()
                
# 模型测试
def translate(model, src, max_len = 80, custom_string=False):
        model.eval()
        if custom_sentence == True:
                src = tokenize_en(src)
                sentence=Variable(torch.LongTensor([[EN_TEXT.vocab.stoi[tok] for tok in sentence]])).cuda()
        src_mask = (src != input_pad).unsqueeze(-2)
        e_outputs = model.encoder(src, src_mask)
        outputs = torch.zeros(max_len).type_as(src.data)
        outputs[0] = torch.LongTensor([FR_TEXT.vocab.stoi['<sos>']])
        for i in range(1, max_len):
                trg_mask = np.triu(np.ones((1, i, i),k=1).astype('uint8')
                trg_mask= Variable(torch.from_numpy(trg_mask) == 0).cuda()
                out = model.out(model.decoder(outputs[:i].unsqueeze(0),e_outputs, src_mask, trg_mask))
        out = F.softmax(out, dim=-1)
                val, ix = out[:, -1].data.topk(1)
                outputs[i] = ix[0][0]
                if ix[0][0] == FR_TEXT.vocab.stoi['<eos>']:
                        break
        return ' '.join([FR_TEXT.vocab.itos[ix] for ix in outputs[:i]])
```

## Multi-Head Attention 结构

`Encoder` 和 `Decoder` 结构中公共的 `layer` 之一是 `Multi-Head Attention` ，其是由多个 `Self-Attention` 并行组成的。`Encoder` block 只包含一个 `Multi-Head Attention`，而 `Decoder` block 包含两个 `Multi-Head Attention` (其中有一个用到 `Masked`)。

![](img/68a11b24d33bb00d102712bd4848e410.png)

`Multi-Head Attention (MHA)` 是基于 `Self-Attention (SA)` 的一种变体。`MHA` 在 `SA` 的基础上引入了“ 多头 ”机制 ，将输入拆分为多个子空间， 每个子空间分别执行 `SA` ，最后将多个子空间的输出拼接在一起并进行线性变换，从而得到最终的输出。

对于 `MHA` ，之所以需要对 `Q`、`K`、`V` 进行多头（`head` ）划分，其目的是为了增强模型对不同信息的关注。具体来说，多组 `Q`、`K`、`V` 分别计算 `Self-Attention`，每个头自然就会有独立的 `Q`、`K`、`V` 参数，从而让模型同时关注多个不同的信息，这有些类似 `CNN` 架构模型的 多通道机制 。

注意的是：`MultiHead`的`head`不管有几个，参数量都是一样的。并不是`head`多，参数就多。记住上面那句话， 将输入拆分为多个子空间， 如果输入的维度已经固定了，拆成多少个头，参数都是一样的。`MultiHead`按照“词向量维度”这个方向，将`Q`、`K`、`V`拆成了多个头，如下图所示（两张图对比去看）：

![](img/012a87308b1c87892a06adf651b7b335.png)

`MultiHead Attention`在带入公式前做了一件事情，就是 拆，`head`数并不是越多越好。

![](img/4a7a59ab42afe742db7b713490e7b12b.png)

![](img/9e156a793b25ee2e61f2da5d6184377b.png)

## Attention的细节

在 `Self-Attention` 中，`Q`、`K`、`V` 是在同一个输入（比如序列中的一个单词）上计算得到的三个向量。具体来说，我们可以通过对原始输入词的 `embedding` 进行线性变换（比如使用一个全连接层），来得到 `Q`、`K`、`V`。这三个向量的维度通常都是一样的，取决于模型设计时的决策。

`attention`可以有很多种计算方式：加性 `attention`、点积 `attention`，还有带参数的计算方式。

![](img/76093a0cb4d09ff161592025e1ea9d8e.png)

在计算 `Self-Attention` 时，`Q`、`K`、`V` 被用来计算注意力分数 ，即用于表示 当前位置和其他位置之间的关系 。注意力分数可以通过 `Q` 和 `K` 的点积来计算，然后将分数除以 `8` ，再经过一个 `softmax` 归一化处理，得到每个位置的权重。然后用这些权重来加权计算 `V` 的加权和，即得到当前位置的输出。在论文中，输入给 `Self-Attention` 层的 `Q`、`K`、`V` 的向量维度是 `64`， `Embedding Vector` 和 `Encoder-Decoder` 模块输入输出的维度都是 `512`。

> 将分数除以 `8` 的操作，对应图中的 `Scale` 层，这个参数 `8` 是 `K` 向量维度 `64` 的平方根结果。

### 为什么有缩放因子  1/\sqrt{d_k}

先一句话回答这个问题: 缩放因子的作用是 「归一化」 。



假设 `Q`，`K` 里的元素的均值为 0，方差为 1，那么 `A^T = Q^T K` 中元素的均值为 0，方差为 `d`。当 `d` 变得很大时，`A` 中的元素的方差也会变得很大，如果 `A` 中的元素方差很大，那么 `softmax(A)` 的分布会趋于陡峭（分布的方差大，分布集中在绝对值大的区域）。总结一下就是 `softmax(A)` 的分布会和 `d` 有关。因此 `A` 中每一个元素乘上 `1/\sqrt{d_k}` 后，方差又变为 1。这使得 `softmax(A)` 的分布“陡峭”程度与 `d` 解耦，从而使得训练过程中梯度值保持稳定。
