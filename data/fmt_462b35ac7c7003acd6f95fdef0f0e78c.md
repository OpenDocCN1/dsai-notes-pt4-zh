
`return self.dropout(x)`

### 2.3.3 一个完整的 `Transformer`

上述所有组件，再按照下图的 Tranfromer 结构拼接起来就是一个完整的 `Transformer` 模型了，如图2.7所示：

图2.7 `Transformer` 模型结构

但需要注意的是，上图是原论文《Attention is all you need》配图，`LayerNorm` 层放在了 `Attention` 层后面，也就是“Post-Norm”结构，但在其发布的源代码中，`LayerNorm` 层是放在 `Attention` 层前面的，也就是“Pre Norm”结构。考虑到目前 LLM 一般采用“Pre-Norm”结构（可以使 `loss` 更稳定），本文在实现时采用“Pre-Norm”结构。

如图，经过 `tokenizer` 映射后的输出先经过 `Embedding` 层和 `Positional Embedding` 层编码，然后进入上一节讲过的 N 个 `Encoder` 和 N 个 `Decoder`（在 `Transformer` 原模型中，`N` 取为6），最后经过一个线性层和一个 `Softmax` 层就得到了最终输出。

基于之前所实现过的组件，我们实现完整的 `Transformer` 模型：

```python
class Transformer(nn.Module):

    '''整体模型'''
    def __init__(self, args):

        super().__init__()

        # 必须输入词表大小和 block size
        assert args.vocab_size is not None
        assert args.block_size is not None
        self.args = args
        self.transformer = nn.ModuleDict(dict(

            wte = nn.Embedding(args.vocab_size, args.n_embd),

            wpe = PositionalEncoding(args),

            drop = nn.Dropout(args.dropout),

            encoder = Encoder(args),

            decoder = Decoder(args),

        ))

        # 最后的线性层，输入是 n_embd，输出是词表大小
        self.lm_head = nn.Linear(args.n_embd, args.vocab_size, bias=False)

        # 初始化所有的权重
        self.apply(self._init_weights)

        # 查看所有参数的数量
        print("number of parameters: %.2fM" % (self.get_num_params()/1e6,))

    '''统计所有参数的数量'''
    def get_num_params(self, non_embedding=False):

        # non_embedding: 是否统计 embedding 的参数
        n_params = sum(p.numel() for p in self.parameters())

        # 如果不统计 embedding 的参数，就减去
        if non_embedding:

            n_params -= self.transformer.wpe.weight.numel()

        return n_params

    '''初始化权重'''
    def init_weights(self, module):

        # 线性层和 Embedding 层初始化为正则分布
        if isinstance(module, nn.Linear):

            torch.nn.init.normal(module.weight, mean=0.0, std=0.02)

            if module.bias is not None:

                torch.nn.init.zeros_(module.bias)

        elif isinstance(module, nn.Embedding):

            torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)

    '''前向计算函数'''
    def forward(self, idx, targets=None):

        # 输入为 idx，维度为 (batch size, sequence length, 1)；targets 为目标序列，用于计算 loss
        device = idx.device
        b, t = idx.size()

        assert t <= self.args.block_size, f"不能计算该序列，该序列长度为 {t}, 最大序列长度只有 {self.args.block_size}"

        # 通过 self.transformer
        # 首先将输入 idx 通过 Embedding 层，得到维度为 (batch size, sequence length, n_embd)
        print("idx", idx.size())

        # 通过 Embedding 层
        tok_emb = self.transformer.wte(idx)

        print("tok_emb", tok_emb.size())

        # 然后通过位置编码
        pos_emb = self.transformer.wpe(tok_emb)

        # 再进行 Dropout
        x = self.transformer.drop(pos_emb)

        # 然后通过 Encoder
        print("x after wpe:", x.size())

        enc_out = self.transformer.encoder(x)

        print("enc_out:", enc_out.size())

        # 再通过 Decoder
        x = self.transformer.decoder(x, enc_out)

        print("x after decoder:", x.size())

        if targets is not None:

            # 训练阶段，如果我们给了 targets，就计算 loss
            # 先通过最后的 Linear 层，得到维度为 (batch size, sequence length, vocab size)
            logits = self.lm_head(x)

            # 再跟 targets 计算交叉熵
            loss = F.cross_entropy(logits.view(-1, logits.size(-1)), targets.view(-1), ignore_index=-1)

        else:

            # 推理阶段，我们只需要 logits，loss 为 None
            # 取 -1 是只取序列中的最后一个作为输出
            logits = self.lm_head(x[:, [-1], :]) # note: using list [-1] to preserve the time dim
            loss = None

        return logits, loss
```

注意，上述代码除去搭建了整个 `Transformer` 结构外，我们还额外实现了三个函数：

+   `get_num_params`：用于统计模型的参数量

+   `_init_weights`：用于对模型所有参数进行随机初始化

+   `forward`：前向计算函数

另外，在前向计算函数中，我们对模型使用 `pytorch` 的交叉熵函数来计算损失，对于不同的损失函数，读者可以查阅 `Pytorch` 的官方文档，此处就不再赘述了。

经过上述步骤，我们就可以从零“手搓”一个完整的、可计算的 `Transformer` 模型。限于本书主要聚焦在 LLM，在本章，我们就不再详细讲述如何训练 `Transformer` 模型了；在后文中，我们将类似地从零“手搓”一个 LLaMA 模型，并手把手带大家训练一个属于自己的 Tiny LLaMA。

## 参考文献

[1] Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Lukasz Kaiser, Illia Polosukhin. (2023). Attention Is All You Need. arXiv preprint arXiv:1706.03762.
