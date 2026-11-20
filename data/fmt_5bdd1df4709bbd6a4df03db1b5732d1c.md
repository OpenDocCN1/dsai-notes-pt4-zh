
# 【LLM 加速技巧】Muti Query Attention 和 Attention with Linear Bias（附源码） - 知乎

> 来源：[https://agijuejin.feishu.cn/docx/LiLJd1Lh6osfpqxO0eocZ9M4nxh](https://agijuejin.feishu.cn/docx/LiLJd1Lh6osfpqxO0eocZ9M4nxh)
>
> 作者：何枝
>
> 原文：https://zhuanlan.zhihu.com/p/634236135

Large Language Model（LLM）效果着实令人震惊，但其训练和推理过程都非常消耗资源。为此，不少研究者都在想办法来加速大模型的训练/推理速度，例如：最近非常火热的 LoRA 和 PTuning。但 LoRA 和 PTuning 大多只用在微调（Finetune）阶段，今天我们来看看另外一种加速的方法，不仅使用于微调，也同样适用于预训练（Pretrain）阶段，以及一种能在短句子上训练，长句子上推理的方法。

## 1. Muti Query Attention (MQA)

> 论文地址：https://arxiv.org/pdf/1911.02150.pdf

MQA 是 19 年提出的一种新的 Attention 机制，其能够在保证模型效果的同时加快 decoder 生成 token 的速度。

那到底能提升多少的速度呢，我们来看论文中给出的结果图：

![](img/224e3976c3c3b5a620a8628483207b93.png)

从上图表中可以看到，MQA 在 encoder 上的提速没有非常明显，但在 decoder 上的提速是很显著，

那 MQA 究竟是怎么做的，能够使得 decoder 有着如此大的速度提升呢？

---

从字面上看，Multi Query Attention（MQA）和 Multi Head Attention（MHA）只差了一个单词，就是从「Head」变成了「Query」。

我们知道，在 transformer 中是包含若干个注意力头（head）组成的，

而每个 head 又是由：`query`（Q），`key`（K），`value`（V）3 个矩阵共同实现的。

既然这里把 Head 换成了 Query，那是不是就意味着只保留了 Query 矩阵呢？

保持这个猜想，我们来看看论文中是怎么解释的：

![](img/82f6a704b65a37ed88c4559ec0629a7e.png)

> 顺带吐槽一句，这篇论文写的着实不是很友好，例子里面很多地方变量没有给出定义和解释，不是很容易看懂 =-=

从论文的解释中可以看到，`MQA` 让所有的头之间 共享 同一份 `key` 和 `value` 矩阵，每个头只单独保留了一份 `query` 参数，从而大大减少 `key` 和 `value` 矩阵的参数量。

> 「参数共享」并不是一个很新奇的思路，在 Albert 里也有通过使用跨层共享参数（Cross-layer parameter sharing）的方式来大大减少 bert 的参数量，具体做法可以参考这里： 何枝：基于BERT的几种改进模型

现在，

我们知道了 `MQA` 实际上是将 head 中的 `key` 和 `value` 矩阵抽出来单独存为一份共享参数，

而 `query` 则是依旧保留在原来的 head 中，每个 head 有一份自己独有的 `query` 参数。

---

这个做法具体是怎么实现的呢？

我们选用 `llm-foundry` 的代码作为示例，一起来看看究竟是怎么个「参数共享」法：

```
# 为了方便阅读，我们只保留了 llm-foundry 中关键部分的代码，完整代码请参照源码。

class MultiheadAttention(nn.Module):

    def __init__(
            self,
            d_model: int,
            n_heads: int,
            device: str
        ):
        """
        Multi Head init func.
        Args:
            d_model (int): hidden state size, e.g. 768
            n_heads (int): 设定的注意力头数, e.g. 8
            device (str): _description_
        """
        super().__init__()

        self.d_model = d_model
        self.n_heads = n_heads
    
        self.Wqkv = nn.Linear(
            self.d_model, 
            3 * self.d_model,                        # 有 query, key, value 3 个矩阵, 所以是 3 * d_model
            device=device
        )                                            # (d_model, 3 * d_model)
        self.attn_fn = scaled_multihead_dot_product_attention
        self.out_proj = nn.Linear(
            self.d_model, 
            self.d_model, 
            device=device
        )

    def forward(
        self,
        x
    ):
        """
        forward func.
        Args:
            x (tensor): (batch, hidden_state, d_model) e.g. -> (1, 768, 512)
        Returns:
            _type_: _description_
        """
        qkv = self.Wqkv(x)                            # (1, 768, 3 * 768)

        query, key, value = qkv.chunk(                # 每个 tensor 都是 (1, 512, 768)
            3, 
            dim=2
        )     

        context, attn_weights, past_key_value = self.attn_fn(
            query,
            key,
            value,
            self.n_heads
        )                                             # (1, 512, 768)

        return self.out_proj(context), attn_weights, past_key_value


class MultiQueryAttention(nn.Module):
    """Multi-Query self attention.
    Using torch or triton attention implemetation enables user to also use
    additive bias.
    """

    def __init__(
        self,
        d_model: int,
        n_heads: int,
        device: Optional[str] = None,
    ):
        super().__init__()

        self.d_model = d_model
        self.n_heads = n_heads
        self.head_dim = d_model // n_heads

        self.Wqkv = nn.Linear(                           # 【关键】Multi-Query Attention 的创建方法
            d_model,
            d_model + 2 * self.head_dim,                 # 只创建 query 的 head 向量，所以只有 1 个 d_model
            device=device,                               # 而 key 和 value 则只共享各自的一个 head_dim 的向量
        )

        self.attn_fn = scaled_multihead_dot_product_attention
        self.out_proj = nn.Linear(
            self.d_model, 
            self.d_model, 
            device=device
        )
        self.out_proj._is_residual = True  # type: ignore

    def forward(
        self,
        x,
    ):
        qkv = self.Wqkv(x)                                           # (1, 512, 960)

        query, key, value = qkv.split(                               # query -> (1, 512, 768)
            [self.d_model, self.head_dim, self.head_dim],            # key   -> (1, 512, 96)
            dim=2                                                    # value -> (1, 512, 96)
        )

        context, attn_weights, past_key_value = self.attn_fn(
            query,
            key,
            value,
            self.n_heads,
            multiquery=True,
        )

        return self.out_proj(context), attn_weights, past_key_value
```

> 完整源码：`github/mosaicml/llm-foundry`

从上面的代码中可以看到，`MHA` 和 `MQA` 之间的区别只在于建立 `Wqkv` Layer 上：

```
# Multi Head Attention
self.Wqkv = nn.Linear(                        # 【关键】Multi-Head Attention 的创建方法
    self.d_model, 
    3 * self.d_model,                         # 有 query, key, value 3 个矩阵, 所以是 3 * d_model
    device=device
)

query, key, value = qkv.chunk(                # 【关键】每个 tensor 都是 (1, 512, 768)
    3, 
    dim=2
)


# Multi Query Attention
self.Wqkv = nn.Linear(                                # 【关键】Multi-Query Attention 的创建方法
    d_model,
    d_model + 2 * self.head_dim,                      # 只创建 query 的 head 向量，所以只有 1 个 d_model
    device=device,                                    # 而 key 和 value 不再具备单独的头向量
)

query, key, value = qkv.split(                        # query -> (1, 512, 768)
    [self.d_model, self.head_dim, self.head_dim],     # key   -> (1, 512, 96)
    dim=2                                             # value -> (1, 512, 96)
)
```

在 `MHA` 中，`query`, `key`, `value` 每个向量均有 768 维度，

而在 `MQA` 中，只有 `query` 是 768 维，而 `key` 和 `value` 均只剩下 96 维了，恰好是 1 个 `head_dim` 的维度。

因此，我们更可以确认：在 `MQA` 中，除了 `query` 向量还保存着 8 个头，`key` 和 `value` 向量都只剩 1 个「公共头」了。

这也正好印证了论文中所说的「所有 head 之间共享一份 `key` 和 `value` 的参数」。

剩下的问题就是如何将这 1 份参数同时让 8 个头都使用，

代码里使用矩阵乘法 `matmul` 来广播，使得每个头都乘以这同一个 tensor，以此来实现参数共享：

```
def scaled_multihead_dot_product_attention(
        query,
        key,
        value,
        n_heads,
        multiquery=False,
    ):
    q = rearrange(query, 'b s (h d) -> b h s d', h=n_heads)         # (1, 512, 768) -> (1, 8, 512, 96)
    kv_n_heads = 1 if multiquery else n_heads
    k = rearrange(key, 'b s (h d) -> b h d s', h=kv_n_heads)        # (1, 512, 768) -> (1, 8, 96, 512) if not multiquery 
                                                                    # (1, 512, 96) -> (1, 1, 96, 512)  if multiquery
    v = rearrange(value, 'b s (h d) -> b h s d', h=kv_n_heads)      # (1, 512, 768) -> (1, 8, 512, 96) if not multiquery 
                                                                    # (1, 512, 96) -> (1, 1, 512, 96)  if multiquery
    
    attn_weight = q.matmul(k) * softmax_scale                       # (1, 8, 512, 512)
    attn_weight = torch.softmax(attn_weight, dim=-1)                # (1, 8, 512, 512)

    out = attn_weight.matmul(v)                                     # (1, 8, 512, 512) * (1, 1, 512, 96) = (1, 8, 512, 96)
    out = rearrange(out, 'b h s d -> b s (h d)')                    # (1, 512, 768)

    return out, attn_weight, past_key_value
```

非常感谢评论区的指正，之前对 `d_model` 的理解有误，导致注释中的维度有问题，容易产生误导，现已修正。还是要亲自跑一遍代码才知道自己理解的到底对不对，这里将源码修改为可以单测的形式提供出来，建议大家可以走一遍 `debug` 看看具体的维度变化。
