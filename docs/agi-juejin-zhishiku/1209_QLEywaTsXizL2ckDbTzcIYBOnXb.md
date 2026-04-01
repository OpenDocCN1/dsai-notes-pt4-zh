# LLM（廿八）：解读 Mamba 2，兼谈 LLM 的统一理论

> 来源：[https://agijuejin.feishu.cn/docx/Ly9LdXmdLoeSu0xQQ6VcfHUNnrb](https://agijuejin.feishu.cn/docx/Ly9LdXmdLoeSu0xQQ6VcfHUNnrb)

> 作者：紫气东来

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/701860743)hihu.com/p/701860...

在笔者之前的文章中，已经比较详细介绍了 Mamba 模型的发展历程及基本原[理。在半年的进步](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2402.19427)和发展中，出现了众多基于 Mamba 的细节优化与场景应用，如 Griffin 和 Jamba，但总体仍然延续了之前的理论框架。

[紫](https://zhuanlan.zhihu.com/p/680833040?)气东来：LLM（廿五）：从控制系统到语言模型 —— Mamba 的前世今生 196 赞同 · 12 评论 文章

而 Mamba 的原作者所提出的 Mamba 2, 则向前迈出了一大步。除了模型结构的调整外，在我看来，更重要的是作者试图建立一个统一的理论体系，从更高的视角来看待 Transformer, RetNet, Mamba 等模型。这种理论的抽象将不仅仅有利于大家理解这些模型本身，更将有利于生成式人工智能的核心内涵，这也是笔者长期以来所思考的问题。

## 一、Mamba 2 的改进

Mamba-2 最重要的变化是提出了结构化状态空间对偶性（ structured state space duality， SSD），其主要有以下要点：

+   SSD 是一个 DNN 模型层，可以用来组合以构建模型；

+   SSD 提供了一个分析框架，可以分析各种生成式模型特点；

+   SSD 比 SSM 有更高的计算效率，更适合 GPU 计算

下面我们便来一探究竟。

### 1.1 从 SSM 到 SSD

1.1.1 回顾 SSM 的核心内涵

要理解 SSD ，必须首先理解 SSM，关于 SSM 的详细原理推荐去复习一下 Mamba 那一篇，在此仅回顾一下其核心部分，即状态转移方程

其本质即实现了一个映射 ，将 视作一个标量，中间隐藏状态 视作 维向量。参数 可以是固定权重，也可以随输入变化(S6)。

进一步地，可以将上述过程表达为更一般的形式 其中 的结构会影响其参数形状

+    适用于常规（非结构化）SSM

+    适用于对角线 SSM

+    用于标量 SSM（即 SSD）

另外， 指头数，在 Mamba 1 中，可认为该值为 1 ，在 Mamba-2 中，可取 P=64 或 P=128。

**1.1.2 SSM 的卷积形式**

在 SSM 中，为了高效计算，可以通过矩阵乘法来实现，即卷积形式，其原理概括如下：

由 ，且

则

写成矩阵形式，即

其中   M M 是下三角矩阵，即

时， ，否则

展开起来，即

这种类型的矩阵实际上有一个名字：它被称为（三角形）半可分离矩阵 ( semiseparable matrix )， 半可分离矩阵的另一种表征是它们的结构秩属性，它表示包含在下三角形部分的每个子矩阵都是低秩的。

![](img/f98ef4a5b531f2c7f46563f3a968dbaf.png)

### 1.2 SSD：更加高效的 SSM 计算

**1.2.1 矩阵运算及其分解**

在 Mamba 1 中，由于 和输入相关，作者放弃了 SSM 的卷积形式，而采用了并行扫描技术。尽管相较于 RNN 类型的模型，性能明显提升，但是与 Transformer 的矩阵计算相比，计算效率仍然较低。

因此 Mamba-2 的主要目标之一是利用 GPU 张量核心来加速 SSM，同时利用 Transformer 的并行优化技术。优化 SSM 矩阵计算的一个重要方法是进行矩阵分解。

![](img/b4d3c8a7c39640df1a0ca18110c93b61.png)

首先将 SSM（半可分离）矩阵划分为大小 × 为 的块，然后使用半可分离矩阵的属性来分解每个非对角线块，这是低秩的。 非对角线块 的分解如下：

其中 #x27; 。

由此得到了 4 种类型的矩阵块：

+   橙色：每个对角线块都是一个较小的半可分离矩阵，可以比较方便进行矩阵计算；

+   绿色：只有 / 完全不同的绿色块，因为它们中的许多是共享的。这些可以使用 matmul 进行计算；

+   黄色：黄色项本身形成一个 1-SS 矩阵（详见2.1节），此步骤等同于SSM扫描；

+   蓝色：与绿色类似，这些可以使用 matmul 进行计算

**1.2.2 计算过程的代码说明**

关于SSD 的实现及以上各分块的计算方法，原作者提供了一个最小版本的代码，如下所示。为了便[于](https://link.zhihu.com/?target=https%3A//github.com/ifromeast/AI_analysis/blob/main/mamba2_ssd.ipynb)理解计算过程的细节，笔者展示出了其中各个步骤的计算结果，感兴趣的可以加一参考 AI_analysis/mamba2_ssd.ipynb at main · ifromeast/AI_analysis

```
def segsum(x):
    &quot;&quot;&quot;Naive segment sum calculation. exp(segsum(A)) produces a 1-SS matrix,
       which is equivalent to a scalar SSM.&quot;&quot;&quot;
    T = x.size(-1)
    x_cumsum = torch.cumsum(x, dim=-1)
    x_segsum = x_cumsum[..., :, None] - x_cumsum[..., None, :]
    mask = torch.tril(torch.ones(T, T, device=x.device, dtype=bool), diagonal=0)
    x_segsum = x_segsum.masked_fill(~mask, -torch.inf)
    return x_segsum

def ssd(X, A, B, C, block_len=64, initial_states=None):
    &quot;&quot;&quot;
    Arguments:
        X: (batch, length, n_heads, d_head)
        A: (batch, length, n_heads)
        B: (batch, length, n_heads, d_state)
        C: (batch, length, n_heads, d_state)
    Return:
        Y: (batch, length, n_heads, d_head)
    &quot;&quot;&quot;
    assert X.dtype == A.dtype == B.dtype == C.dtype
    assert X.shape[1] % block_len == 0

    # Rearrange into blocks/chunks
    X, A, B, C = [rearrange(x, &quot;b (c l) ... -&gt; b c l ...&quot;, l=block_len) for x in (X, A, B, C)]

    A = rearrange(A, &quot;b c l h -&gt; b h c l&quot;)
    A_cumsum = torch.cumsum(A, dim=-1)

    # 1. Compute the output for each intra-chunk (diagonal blocks)
    L = torch.exp(segsum(A))
    Y_diag  = torch.einsum(&quot;bclhn,bcshn,bhcls,bcshp-&gt;bclhp&quot;, C, B, L, X)

    # 2. Compute the state for each intra-chunk
    # (right term of low-rank factorization of off-diagonal blocks; B terms)
    decay_states = torch.exp((A_cumsum[:, :, :, -1:] - A_cumsum))
    states = torch.einsum(&quot;bclhn,bhcl,bclhp-&gt;bchpn&quot;, B, decay_states, X)

    # 3. Compute the inter-chunk SSM recurrence; produces correct SSM states at chunk boundaries
    # (middle term of factorization of off-diag blocks; A terms)
    if initial_states is None:
        initial_states = torch.zeros_like(states[:, :1])
    states = torch.cat([initial_states, states], dim=1)
    decay_chunk = torch.exp(segsum(F.pad(A_cumsum[:, :, :, -1], (1, 0))))
    new_states = torch.einsum(&quot;bhzc,bchpn-&gt;bzhpn&quot;, decay_chunk, states)
    states, final_state = new_states[:, :-1], new_states[:, -1]

    # 4. Compute state -&gt; output conversion per chunk
    # (left term of low-rank factorization of off-diagonal blocks; C terms)
    state_decay_out = torch.exp(A_cumsum)
    Y_off = torch.einsum(&#x27;bclhn,bchpn,bhcl-&gt;bclhp&#x27;, C, states, state_decay_out)

    # Add output of intra-chunk and inter-chunk terms (diagonal and off-diagonal blocks)
    Y = rearrange(Y_diag+Y_off, &quot;b c l h p -&gt; b (c l) h p&quot;)
    return Y, final_state
```

### 1.3 使用 SSD 搭建起 Mamba 2

Mamba-2 的核心贡献是 SSD 层及其理论，相较于 Attention，SSD 降低了计算复杂度，可以更加高效进行训练和推理；相较于 SSM，SSD实现了并行计算及矩阵运算，这样就可以充分利用 GPU 的算力。

![](img/45aa7da4449166a9ea46ee15a171050a.png)

除此之外 SSD 层的贡献外，Mamba 2 的结构也稍有改动。主要变化是与 输入并行生成SSM 参数 ( , , ) ，而不是按顺序生成。这在一定程度上是出于与注意力的联系，同时也使其更适合于TP等并行化技术。

![](img/4bee6a1af57f19fdaa4c265bfdfbf02f.png)

使用张量并行 （TP） 对 Mamba 1 进行大规模训练的一个困难是，它每层需要 2 个all-reduce，而 Transformer 中每个 attention 或 MLP 层只需要 1 个all-reduce。这是因为某些 SSM 参数是内部激活的函数，而不是层输入的函数。

在 Mamba 2 中，使用“平行投影”结构，所有 SSM 参数都是层输入的函数，我们可以很容易地将 TP 应用于输入投影：将输入投影和输出投影矩阵拆分为 2、4、8 个分片，具体取决于 TP 度。我们使用一个分组范数，其组数可被 TP 度整除，因此每个 GPU 单独进行归一化。这些更改会导致每层 1 个 all-reduce，而不是 2 个。

![](img/68a3352f0e54680667514f13171e8e3f.png)

当在非常长的序列长度上进行训练时，我们可能需要沿着序列长度进行拆分，并将不同的部分分配给不同的设备。序列并行性 （sequence parallelism, SP） 有两种主要形式： 对于残差和归一化运算：将 TP 中的 all-reduce 替换为 reduce-scattter、残差 + 归一化，然后是 all-gather。

由于 Mamba-2 使用与 Transformer 相同的残差和归一化结构，因此这种形式的 SP 无需修改即可直接应用。用于attention 或 SSM 操作，也称为上下文并行（context parallelism, CP）。对于 attention ，可以使用环形注意力沿序列维度将其拆分。对于 Mamba-2，SSD 框架再次为我们提供了帮助：使用相同的块分解，我们可以让每个 GPU 计算其本地输出和最终状态，然后在 GPU 之间传递状态（使用发送/接收通信原语），然后再更新每个 GPU 的最终输出。

![](img/cfbea26a3fe3c161b630c645b86f2c09.png)

除此之外，Mamba 2 还非常适合可变长度。为了进行微调和推理，在同一批次中，我们通常具有不同长度的序列。对于 Transformer，人们通常会填充所有序列具有相同的长度（浪费计算），或者通过仔细的负载平衡专门针对可变长度序列实现 attention 。使用 SSM，可以简单地将整个 batch 视为一个长“序列”，并通过在每个序列末尾将标记的状态转换 设置为 0 来避免在 batch 中的不同序列之间传递状态。

## 二、LLM 统一理论

在生成式模型大热之后，也出现了许多试图替代 Transformer 的结构，如 RWKV系列、RetNet系列、Mamba系列等。笔者在之前的文章中也尝试进行一些整理和统合的工作，但都不够系统和深刻，在此借助 Mamba 2 的工作，重新认识和讨论这一问题。

[紫](https://zhuanlan.zhihu.com/p/673781418)气东来：LLM（廿四）：Transformer 的结构改进与替代方案 339 赞同 · 9 评论 文章

### 2.1 SSM 的矩阵形式与 Transformer 的注意力机制

2.1.1 Attention 机制再讨论

首先回顾一下 Transformer 的 Attention 机制，其本质是一个映射操作，即

(Q(T,N),K(T,N),V(T,P))↦Y(T,P)

具体来说就是

其中 可以被改写为 ，现在进行一个重定义 ，此时我们可以把 看作核函数，此时的注意力机制即核注意力

当序列长度 增长并且特征维度 较小时，那么注意力成本可以从二次降低 到线性。这是通过简单地以不同的顺序计算矩阵乘法得出的

这是最常见及简单的线性化注意力的方法。

接下来讨论生成式模型的因果掩码（Causal Mask）。线性注意力的掩码表示可以表示如下：

其中 表示element-wise 乘，    为 mask 矩阵，即

对于这种掩码操作，有如下等价操作    2.1.2 Duality: SSM 的注意力表征

考虑如下循环表示

这种关系可以使用矩阵形式表示 ，其中 为

将这种形式的矩阵称为 1-SS (1-semiseparable) 矩阵。可见当  时，   就是正常的掩码矩阵。

接下来我们需要回到 SSM 的 M 矩阵，当 是标量时，有

此时 M 矩阵可以改写为 这样将一维输入映射到一维输出的序列变换 表示为 。这样当我们做一个转换 ，该形式就和线性注意力机制完全等价！二者之间的关系如下所示：

![](img/df8b01469c45c722f995fc76422af50f.png)

这样我们就可以把 SSD 写成抽象的映射表示，即

### 2.2 统一的视角看各类模型

在前文中我们讨论的一个关键就是 Mask，实际上无论是在 attention 中还是其他改造的结构中，其承担的作用主要在两方面：

1.  因果关系。即对于序列来说，只能是后置单位观测前置单位，而不能相反，这是事物随时间发展的规律确定的。满足这种特点的 Mask 具有相同的特性，即是一个下三角矩阵。

1.  远程衰减。这个功能并不是所有 Mask 都有的，一部分远程衰减的功能是由位置编码实现的， 如RoPE。在这里最典型的两个 Mask 是：

1.  Linear Attention 的 Causal Mask 下三角值均为1，完全没有远程衰减的功能；

1.  RetNet 的 Decay Mask 则认为规定的指数衰减（当然在后续的YOCO中已经修改了这一点）

![](img/0ffc2c65e765eb4a17128ea70a30d504.png)

在笔者看来，最重要的贡献是构建了一个框架，使用统一的理论将 SSM 和 Attention 联系了起来，其交集的部分即 SSD 模型，在该理论下 Linear Attention 和 RetNet 只是其一种特殊形式。

![](img/c8f75d1d3bed13c2586f29f767f1fbc4.png)

接下来，尝试回答几个问题。

问题1: SSM 和 Attention 是如何被联系起来的？

这个问题是本文所讨论的核心问题，在前文中用了比较复杂的理论进行推导。简单来说就是，SSM 的卷积形式可以写成矩阵相乘的形式，当   A A 为标量时，可以进一步改写为和 Linear Attention 相同的形式，不同之处在于SSM掩码矩阵是一个 1-SS 矩阵，因果掩码只是其一种特殊形式。

问题2: 为什么要放弃 softmax，其收益与代价?

去掉了 softmax，attention 部分就可以进行自由的矩阵变换了，这也就是 Linear Attention 和 RetNet 放弃 softmax 的根本原因，其代价就是需要 Mask 来进行因果掩码及部分远程衰减的功能，同时要考虑增加非线性单元。如果保留 softmax，则无法与卷积形式的 SSM 构成等价关系。

线性的变换固然可以提高计算效率（包括训练效率和推理效率），但问题在于 Casual Mask 过于简单缺少泛化性，Decay Mask 在距离较远时又会使注意力衰减过大，1-SS 矩阵介于二者之间，但是面对大数据量及长文本是否可以 scale up 仍然需要时间的检验。

## 参考资料

[1] https:// arxiv.org/pdf/2405.2106 0

[2] [State Space Duality (Mamba-2](https://link.zhihu.com/?target=https%3A//tridao.me/blog/2024/mamba2-part1-model/)) Part I - The Model

[3] [State Space Duality (Mamba-2) ](https://link.zhihu.com/?target=https%3A//tridao.me/blog/2024/mamba2-part2-theory/)Part II - The Theory

[4] [S](https://link.zhihu.com/?target=https%3A//tridao.me/blog/2024/mamba2-part3-algorithm/)tate Space Duality (Mamba-2) Part III - The Algorithm

[5] [h](https://link.zhihu.com/?target=https%3A//tridao.me/blog/2024/mamba2-part4-systems/)ttps:// tridao.me/blog/2024/mam ba2-part4-systems/

[6] [重温SSM（一）：线性系统和HiPPO矩阵 - 科学](https://link.zhihu.com/?target=https%3A//spaces.ac.cn/archives/10114)空间|Scientific Spaces

[7] [重温SSM（二）：HiPPO的一些遗留问题 - 科学](https://link.zhihu.com/?target=https%3A//spaces.ac.cn/archives/10137)空间|Scientific Spaces

> 卷地风来忽吹散，望湖楼下水如天。 —— 苏轼《望湖楼醉书》