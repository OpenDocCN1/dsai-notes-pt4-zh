# Deploy006 Norm

> 来源：[https://aicarrier.feishu.cn/docx/YbKzdeK8to9N35xlEd0cPo3LnBs](https://aicarrier.feishu.cn/docx/YbKzdeK8to9N35xlEd0cPo3LnBs)

从 传统的CNN卷积神经网络 到 基于Transformer的视觉大模型VIT，再到目前多模态大语言模型MLLM的不断发展，激活函数、归一化函数以及模型初始化，始终作为网络结构中的比较容易忽视的一部分，但是其与模型训练收敛和最终性能息息相关，之前一直比较忽视这部分，以 “浅谈Transformer的初始化、参数化与标准化”为思路 (主要是针对每层结构输出的均值和方差)，

结合之前的文章[Deploy008 激活函数](https://aicarrier.feishu.cn/wiki/PjoUwaGZtiNeHPkHiifcHptPnme)，简单总结一下 归一化层、模型初始化以及激活函数对于以下常见网络训练的影响。

| 组件 | ResNet | ViT | LLaMA |
| --- | --- | --- | --- |
| 归一化 | Batch Normalization：在每个卷积层后进行BN，减小内部协变量偏移，输出均值接近0，方差接近1。 | Layer Normalization：在每个自注意力层和前馈层后进行LN，保持输出均值和方差稳定，适合处理序列数据。 | **RMSNorm**：使用均方根归一化，确保每层输出均值和方差在合理范围内，增强模型稳定性。 |
| 权重初始化 | **He初始化**：适用于ReLU激活函数，保持激活值的均值和方差在合理范围，促进收敛。 | **均匀分布或正态分布初始化**：确保每层输出均值和方差适中，保持稳定学习过程。 | **均匀分布或正态分布初始化**：同样确保输出均值和方差适中，稳定训练。 |
| 激活函数 | **ReLU及其变种**：简单高效，能减轻梯度消失问题，但输出均值偏向正值，方差较大。 | GELU（Gaussian Error Linear Unit）：平滑非线性激活，输出均值和方差稳定，适合复杂模型。 | **SwiG**LU（Swish-Gated Linear Unit）：结合Swish和门控机制，增强非线性表达能力，保持输出均值和方差稳定。 |

大语言模型的背景下提炼一下关于激活、归一化函数的观点:

1.  模型初始化以及激活函数适配性对于模型收敛性影响:

衡量网络是否“消失”或者“爆炸”的指标，输出0均值、1方差是非必要的，这里我们用二阶（原点）矩来代替，它可以看成是L2模长的变体，跟方差的作用类似，都可以用来衡量指标是否“消失”或者“爆炸”，但它相对来说更普适和简单。包括Xavier初始化针对sigmoid、tanh的适配、“微调”后的Elu函数等。

| 模型 | 激活函数 | 对收敛性的影响 | 优点 | 缺点 |
| --- | --- | --- | --- | --- |
| ResNet | ReLU | 稳定训练并支持更深的网络结构，减少梯度消失问题。 | 减少梯度消失；支持残差连接，允许更深的网络。 | 可能会出现“死神经元”现象，即某些神经元不再激活。 |
| Vision Transformer | GELU | 提高收敛速度，提供更平滑的线性与非线性区域过渡，改善训练过程中的梯度流动。 | 减少梯度消失，保持平滑性，适合处理复杂模式。 | 对超参数的调优依赖性较强，可能需要更多的实验。 |
| Llama3 | SwiGLU | 通过非单调特性改善优化过程，使得收敛更加高效，适应复杂的任务。 | 结合了激活函数和线性单元的优点，适合大规模模型。 | 由于其复杂性，可能导致解释性较差，难以理解其内部机制。 |

1.  通过层归一化稳定方差对于模型收敛性影响

相比激活函数这种简单的“微调”，更直接的处理方法是各种Normalization方法，如Batch Normalization、Instance Normalization、Layer Normalization等，这类方法直接计算当前数据的均值方差来将输出结果标准化。去掉center后的Layer Normalization (Root Mean Square Normalization, RMSNorm) 在Google的T5、以及LLama3中均被使用 替换掉了LayerNorm，实验结论上还有提升，性能上也有优势(无需计算Mean)。

1.  残差对于模型收敛性的影响

Post Norm结构，它也是原版Transformer和BERT所使用的设计。然而，这种做法虽然稳定了前向传播的方差，但事实上已经严重削弱了残差的恒等分支，所以反而失去了残差“易于训练”的优点。

![](img/6dae85a464be64a1019779a3ac1f3691.png)

![](img/c0f8d79d1864035a3b6170ad4e32e1e3.png)

Post Norm 保证了主干方差恒定，每层对   都可能有较大影响。代价则是模型结构中没有从头到尾的恒等路径，梯度难以控制。通常认为会更难收敛，但训练出来的效果更好。

Post Norm 和 Pre Norm 相反，它每 Norm一次就削弱一次恒等项   的权重，变相更突出残差项   的（但是梯度还是递减的，只不过总体看更加偏向于残差项）。

所以其实 Post-N 和传统的深度网络都一样有梯度消失的问题，而 Pre Norm 避免了这种问题，让训练变简单，但是带了新的问题，即网络退化问题。

## 归一化函数

### 1. Batch Normalization（批量归一化）

- 目的：BN 通过计算每个批次的均值和方差来标准化输入，使输出具有零均值和单位方差。这有助于加速训练并稳定模型。

- 计算：对每个mini-batch计算均值和方差，然后进行标准化。(Per channel across mini-batch)

其中   是批量大小， 是一个很小的数字，用于数值稳定性。  和   是可学习的缩放和偏移参数

https://pytorch.org/docs/stable/generated/torch.nn.BatchNorm2d.html

```
import torch
import torch.nn as nn

class BatchNorm(nn.Module):
    def __init__(self, num_features, eps=1e-5, momentum=0.1):
        super(BatchNorm, self).__init__()
        self.eps = eps
        self.momentum = momentum
        # 可学习的缩放和偏移参数
        self.gamma = nn.Parameter(torch.ones(num_features))
        self.beta = nn.Parameter(torch.zeros(num_features))
        self.running_mean = torch.zeros(num_features)
        self.running_var = torch.ones(num_features)

    def forward(self, x):
        if self.training:
            batch_mean = x.mean(dim=0)
            batch_var = x.var(dim=0, unbiased=False)
            x_norm = (x - batch_mean) / torch.sqrt(batch_var + self.eps)
            # 记录移动平均的 running_mean、running_var 
            self.running_mean = self.momentum * batch_mean + (1 - self.momentum) * self.running_mean
            self.running_var = self.momentum * batch_var + (1 - self.momentum) * self.running_var
        else:
            x_norm = (x - self.running_mean) / torch.sqrt(self.running_var + self.eps)
        
        return self.gamma * x_norm + self.beta
```

### 2. Layer Normalization（层归一化）

Layer Normalization

- 目的：在每个样本的特征维度上进行归一化，适用于RNN等序列模型。

**   - 特点：对每个样**本的所有特征进行归一化，不依赖于batch的大小。

其中，  是特征维度。

```
class Naive_LayerNorm(torch.nn.Module):
    def __init__(self, num_features, eps=1e-5):
        super(Naive_LayerNorm, self).__init__()
        self.eps = eps
        self.gamma = torch.nn.Parameter(torch.ones(num_features))
        self.beta = torch.nn.Parameter(torch.zeros(num_features))

    def forward(self, x):
        mean = x.mean(dim=-1, keepdim=True)
        var = x.var(dim=-1, keepdim=True, unbiased=False)
        x_norm = (x - mean) / torch.sqrt(var + self.eps)
        return self.gamma * x_norm + self.beta
```

https://pytorch.org/docs/stable/generated/torch.nn.LayerNorm.html

```
import torch

# NLP Example
batch, sentence_length, embedding_dim = 20, 5, 10
embedding = torch.randn(batch, sentence_length, embedding_dim)
layer_norm = torch.nn.LayerNorm(embedding_dim)
# Activate module
nlp_output = layer_norm(embedding)
print(nlp_output.shape)

# Image Example
N, C, H, W = 20, 5, 10, 10
input = torch.randn(N, C, H, W)
# Normalize over the last three dimensions (i.e. the channel and spatial dimensions)
# as shown in the image below
layer_norm = torch.nn.LayerNorm([C, H, W])
output = layer_norm(input)
print(output.shape)
```

1.  重新居中：它使模型对输入和权重上的偏移噪声不敏感。

1.  重新缩放：当输入和权重都随机缩放时，它保持输出表示不变。

### 3. Instance Normalization（实例归一化）

[I](https://arxiv.org/abs/1607.08022)nstance Normalization: The Missing Ingredient for Fast Stylization

**   - 目的：在**图像生成和风格迁移中常用，对每个样本的每个通道计算均值和方差进行归一化。

- 特点：通常用于风格迁移任务，能够保持样本之间的风格差异。

其中，  和   分别是高度和宽度。

```
class InstanceNorm(nn.Module):
    def __init__(self, num_features, eps=1e-5):
        super(InstanceNorm, self).__init__()
        self.eps = eps
        self.gamma = nn.Parameter(torch.ones(num_features))
        self.beta = nn.Parameter(torch.zeros(num_features))

    def forward(self, x):
        mean = x.mean(dim=(2, 3), keepdim=True)
        var = x.var(dim=(2, 3), keepdim=True, unbiased=False)
        x_norm = (x - mean) / torch.sqrt(var + self.eps)
        return self.gamma * x_norm + self.beta
```

### 4. Group Normalization（组归一化）

- 目的：将特征通道分成若干组，在组内计算均值和方差进行归一化，适用于小batch size的情况。

- 特点：在图像分类等任务中表现良好，尤其是在小batch情况下。

其中，  是批量大小，  是通道数，  是组数。

```
class GroupNorm(nn.Module):
    def __init__(self, num_groups, num_channels, eps=1e-5):
        super(GroupNorm, self).__init__()
        self.num_groups = num_groups
        self.eps = eps
        self.gamma = nn.Parameter(torch.ones(num_channels))
        self.beta = nn.Parameter(torch.zeros(num_channels))

    def forward(self, x):
        N, C, H, W = x.size()
        x = x.view(N, self.num_groups, C // self.num_groups, H, W)
        mean = x.mean(dim=(2, 3, 4), keepdim=True)
        var = x.var(dim=(2, 3, 4), keepdim=True, unbiased=False)
        x_norm = (x - mean) / torch.sqrt(var + self.eps)
        x_norm = x_norm.view(N, C, H, W)
        return self.gamma * x_norm + self.beta
```

#### 直观理解

![](img/de2d552f6b60d5605d0884a70633df1a.png)

+   B**atchNorm：n**×c×w×h -> nwh×c，那么我们的意思是 nwh。对于每个通道 （c），我们减去其相应的平均值，该平均值是在整个批次和所有像素上计算得出的。N本书c页，将每本书第c页所有文字(w*h)取均值和方差。insight:对于不同的样本来说，某个通道学习的信息都是某个维度、颜色的信息，学习一个固定的feature，batch一般很大。

+   L**ayerNorm**： n×c×w×h -> cwh×n，那么我们的意思是 cwh。对于每个示**例 （n），我们用相应的平均**值减去它，该平均值是在所有通道和像素上计算的。对于embedding层:  n×token×embedding_dims -> embedding_dims× (n×token),该平均值是在embedding_dims计算的。N本书c页，将第n本书所有页所有文字(w*h)取均值和方差。相应地 就是每个token的embedding_dims(4096个值)做均值和方差。 batchsize不会很大，数据不定长，不同句子 之间的关系不紧密。

+   InstanceNorm： n×c×w×h -> wh×cn，那么我们的意思是 wh。对于每个通道 （c） 和每个示例 （n），我们用相应的平均值减去它。N本书c页，将第n本书第n页所有文字(w*h)取均值和方差。

+   GroupNorm：n×c×w×h -> swh×gn，其中 c = sg。这将几个通道分成同一组，我们在 swh 上做平均值。然后，我们减去每个组 （g） 和每个样本 （n） 及其相应的平均值。N本书c页，将c页书分为s组，将第s组所有文字(w*h)取均值和方差。

可以自行验证Norm之间的等价性关系，其中差别仅在于归一化的范围。

### 5. RMS (Root Mean Square) Normalization

作用位置:  对每个Transformer block 的输入进行归一化，而不是对输出进行归一化。(pre-normalization方式)，RMSNorm 是层归一化 （LayerNorm） 的扩展。使用 RMSNorm 的原因是 LayerNorm 中的计算开销。这使得改进变得缓慢且昂贵。RMSNorm 实现了与 LayerNorm 相当的性能，但运行时间减少了 7%∼64%。RMSNorm 通过完全删除 LayerNorm 中的(Mean)平均统计量来简化 LayerNorm。

```
class TransformerBlock(nn.Module):
    &quot;&quot;&quot; Transformer block: communication followed by computation
    &quot;&quot;&quot;
    def __init__(self, layer_id: int, args: ModelArgs):
        super().__init__()
        self.n_heads = args.n_heads
        self.dim = args.dim # 4096
        self.head_dim = args.dim // args.n_heads # 4096 // 32

        # ATTN
        self.attention = Attention(args)
        
        # FFN
        self.feed_forward = FeedForward(
            dim=args.dim,
            hidden_dim=4 * args.dim, # 16384
            multiple_of=args.multiple_of, # 1024
            ffn_dim_multiplier=args.ffn_dim_multiplier, # 1.3
        )
        self.layer_id = layer_id
        self.attention_norm = RMSNorm(args.dim, eps=args.norm_eps)
        self.ffn_norm = RMSNorm(args.dim, eps=args.norm_eps)

    def forward(
        self,
        x: torch.Tensor,
        freqs_cos: torch.Tensor,
        freqs_sin: torch.Tensor,
    ):
        # (B, seq_len, dim) + (B, seq_len, dim) --&gt; (B, seq_len, dim)  (torch.Size([1, 17, 4096]), torch.Size([17, 64]), torch.Size([17, 64]))
        h = x + self.attention(self.attention_norm(x), freqs_cos, freqs_sin)
        out = h + self.feed_forward(self.ffn_norm(h))
        return out
```

- 目的：一种替代Layer Normalization的方法，RMS Norm 通过计算特征的均方根来标准化输入。它减少了对均值的依赖，适用于不同的输入分布。

- 特点：计算效率高，适用于大规模模型。

其中，均方根（RMS）定义为：

参数及其意义

1.   ：表示神经元的输入总和，通常是经过加权的模型参数的输出。

2.  ：均方根值，作为输入的标准化因子，提供输入幅度的度量。它帮助在不同的输入和权重缩放情况下保持输出的稳定性。

3.  ：归一化后的输出，经过RMS标准化的神经元激活值。

4.  ：增益参数，允许模型对归一化后的输出进行重新缩放。通常初始化为1，但在训练过程中可以更新。

5.  ：默认 1e-6，是一个很小的数字，加在分母用于数值稳定性。

RMSNorm的设计目的是提高计算效率，同时保持归一化的优点，如稳定训练过程和增强模型收敛性。与传统的LayerNorm不同，RMSNorm不执行均值归一化，从而简化了计算，减少了开销，特别适用于大型模型或需要快速迭代的任务。

```
class RMSNorm(torch.nn.Module):
    &quot;&quot;&quot; Root Mean Square Layer Normalization
    &quot;&quot;&quot;
    def __init__(self, dim: int, eps: float = 1e-6):
        super().__init__()
        self.eps = eps
        self.weight = nn.Parameter(torch.ones(dim))

    def _norm(self, x):
        # (B, seq_len, dim)
        return x * torch.rsqrt(x.pow(2).mean(-1, keepdim=True) + self.eps)

    def forward(self, x):
        output = self._norm(x.float()).type_as(x)
        # (dim) * (B, seq_len, dim) --&gt; (B, seq_len, dim)
        return output * self.weight
```