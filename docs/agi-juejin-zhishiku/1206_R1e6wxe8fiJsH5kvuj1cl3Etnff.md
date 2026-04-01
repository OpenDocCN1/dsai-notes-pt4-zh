# LLM（廿五）：从控制系统到语言模型 —— Mamba 的前世今生

> 来源：[https://agijuejin.feishu.cn/docx/XOEQd3ZfMomjxUx9rc0c6eUrn9g](https://agijuejin.feishu.cn/docx/XOEQd3ZfMomjxUx9rc0c6eUrn9g)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/680833040)hihu.com/p/680833...

在当前 Transformer 之外的模型，势头最盛的恐怕就是 Mamba 了，而要学习和研究 Mamba 就离不开其前导模型—— 状态空间模型(State Space Models, SSM) ，而 SSM 则又是脱胎于动态控制系统。本文将从最初的起点出发，通过数学和实现细节上的分析和讨论，抽丝剥茧，以期对 Mamba 有一个根本而深刻的认识。

该篇在内容上属于前一篇的一部分。

[紫](https://zhuanlan.zhihu.com/p/673781418?)气东来：NLP（廿四）：Transformer 的结构改进与替代方案 339 赞同 · 9 评论 文章

由于前文篇幅所限，且该篇内容自成体系，还由于该篇内容的底层原理正是来源于笔者的大学专业内容，仅以此文来纪念青葱岁月与奋斗年华。

## 一、控制系统与状态空间模型

考虑控制系统的连续状态的时不变系统，如下：

![](img/06e67caa93ed0374391a459610943efd.png)

改图的状态方程可以表示为：

其中 x′为状态变量 的导数，实际上控制系统中这种关系非常普遍，如电容： ，电感： ； 表示输入， 表示输出。

均为可学习的矩阵。

其中 被称为 skip connection, 可以理解为经过变换的残差连接。因此通常把不包括 在内的部分特指为 SSM，其方程如下：

![](img/9ca56e225657453af6052a3b2aae6d3c.png)

需要说明的还有两点：

+   该系统是一个时不变( time-invariant )系统（这对后边 mamba 的改造非常关键）

+   该系统是一个连续( continuous )系统（在实际处理中需要离散化处理）

### 1.1 离散化及其三种解法

由于以上的状态是连续的，而在实际的系统中状态通常是通过采样来获取的，因此需要进行离散化处理。其中的关键又是微分方程 的求解和处理，对该方程的离散化主要有 3 种方法，下面分别加以推导。

**1.1.1 方法一：微分极限法**

首先，由导数的定义

![](img/1aaeecde80c4c531b4cefc37db367ae0.png)

则

![](img/1c661f67a89769aadf4d38db960cc971.png)

对上式稍加变换，有

代入状态方程则有

因此有 ，尽管该方法非常简单，但不够稳定和精确，因此有以下 2 种方法。

1.1.2 方法二：双线性变换（Tustin’s method）

在方法一中，我们利用了微分的定义，那么现在我们将尝试使用积分的思想。对于函数    ，其在 的区域面积可以表示为 ，同理，则有

继续代入状态方程，则有

在很小的时间间隔 内，可认为 ，因此则有

这也是 SSM 论文中提供的结果，在此补充完整推理计算过程。

**1.1.3 方法三：零序保持（ze**ro- order hold, ZOH）

之前两种方法都是通过微积分的方法变换得到，实际上，可以通过解常微分方程的方式求解。对于微分方程 的通解为

令 并且 则有

则有

这也是 Mamba 论文中提供的结果，在此补充完整推理计算过程。

除了 之外，还有 ，这样就通过三种方式实现了状态方程的离散化，经过离散化后就实现了线性的循环模式。当然，这些还仅仅是第一步，让我们继续吧。

我们可以简单计算几步体会其过程

![](img/5043f0f3816dc03edcf7aa98091ae670.png)

这种离散后的计算方式，与典型的 RNN 别无二致，需要说明的是其中 是状态转移的桥梁

![](img/344346587d45def0e86817f4a76f5551.png)

这样就会遇到 RNN 的困境，即无法实现计算的并行化。

### 1.2 卷积形式与训练并行化

**通过前文，我们已**经认**识了两种形式： **原始的连续形式 和 序列的循环形式 ，如下图的前两部分。连续形式在实际中不易实现，循环形式可以实现推理的线性，但是无法并行训练，这就引出了第三种形式，即卷积形式。

![](img/096396db3ea46eabb1a8896a2d9d5cca.png)

首先写出离散形式下的状态方程

由此可以写出前几步的状态变量

即状态 可以表示成输入  的函数。

同理可以写出前几步的输出变量

令卷积核 则状态方程可简写为

![](img/fbee209f4a49a6d0850dca81b9a9d28b.png)

为了计算 ，关键在于计算 ，最理想的状态是特征值对角化，即

除此之外还有一种 **HiPPO **( High-Order Polynomial Projection Operator ) 矩阵作为转移矩阵，即

通过以上的卷积形式，即可实现训练的并行性，再结合适用于推理的线性复杂度的循环形式，即构成了完整的 SSM 结构。

![](img/2a4939841137a9d583108df0cd3362d7.png)

以上我们详细推导了三种形式，现在有必要总结一下每种形式的特点：

+   **连续形式**

+   自动处理连续数据（如音频信号、时间序列）。这在处理具有不规则或时移采样的数据时具有巨大的优势。

+   可以进行数学上的分析，例如通过计算精确的轨迹或构建记忆系统 。

+   训练和推理都非常慢

+   **循环形式**

+   对于序列数据的自然归纳，原则上无上下文限制

+   高效推理（恒定时间状态更新）

+   学习缓慢（缺乏并行性）

+   训练过长序列时梯度消失或爆炸

+   **卷积形式**

+   本地的、可解释的特征

+   高效（可并行化）训练

+   在线或自回归上下文中的速度较慢（必须重新计算每个新数据点的整个输入）

+   固定上下文大小

以上内容已经全面介绍了 SSM 的诸多细节，但对于文本生成的场景，便可以比较清楚发现其问题所在。在 Transformer 架构中，前文的信息存储在了 KV cache 中，而 SSM 中不存在类似模块，因此其循环形式并不擅长处理上下文学习相关问题。那么这就引出了 Mamba 的优化和改进。

![](img/19f28e926d386fb8d9811eb06b0fdc35.png)

## 二、Mamba 的突围

概括而言，SSM 模型的问题主要可以总结为以下两个方面：

1.  **选择性复制 **：在语言模型的上下文中，选择性复制是指从给定输入中辨别相关信息并将其适当地再现或合并到生成的输出中的能力。它涉及模型从输入数据中识别和重现特定短语、实体或模式的能力，从而增强生成文本的相关性和连贯性。

1.  **归纳头 **：语言模型中的归纳头属于专门组件，可促进模型从输入数据中推断和概括知识的能力。与人类根据观察到的模式得出结论和进行推论的方式类似，归纳头使模型能够推断信息、理解潜在的关系，并应用学到的概念来生成更细致、更适合上下文的响应。

### 2.1 Mamba 的选择机制

普通 SSM 无法学习选择性复制任务的原因是它们是时不变的（内核是固定的）。这意味着可学习参数随着每个新 token 的传入而保持固定。

![](img/999da0c73748ccf4177312de17e2aaa4.png)

Mamba 的第一个改进就是 选择性扫描(Selective Scan) 操作，即放弃卷积和递归的双重属性，仅依赖于循环。具体来说就是时变参数化。矩阵 A（HiPPO 矩阵）保持不变，但 Δ、B 和 C 现在成为输入的函数。下图展示了二者选择机制的差异。

![](img/9bcfb07efe5b8f12c36353a8070d9bb8.png)

![](img/8b0255375c32ea45b1e465b9af8d956a.png)

以下为 SSM+Selection 的一个简单实现：

```
    def ssm(self, x):
        &quot;&quot;&quot;Runs the SSM. See:
            - Algorithm 2 in Section 3.2 in the Mamba paper [1]
            - run_SSM(A, B, C, u) in The Annotated S4 [2]

        Args: x: shape (b, l, d_in)    (See Glossary at top for definitions of b, l, d_in, n...)
    
        Returns: output: shape (b, l, d_in)

        Official Implementation:
            mamba_inner_ref(), https://github.com/state-spaces/mamba/blob/main/mamba_ssm/ops/selective_scan_interface.py#L311
            
        &quot;&quot;&quot;
        (d_in, n) = self.A_log.shape

        # Compute ∆ A B C D, the state space parameters.
        #     A, D are input independent (see Mamba paper [1] Section 3.5.2 &quot;Interpretation of A&quot; for why A isn&#x27;t selective)
        #     ∆, B, C are input-dependent (this is a key difference between Mamba and the linear time invariant S4,
        #                                  and is why Mamba is called **selective** state spaces)
        
        A = -torch.exp(self.A_log.float())  # shape (d_in, n)
        D = self.D.float()

        x_dbl = self.x_proj(x)  # (b, l, dt_rank + 2*n)
        (delta, B, C) = x_dbl.split(split_size=[self.args.dt_rank, n, n], dim=-1)  # delta: (b, l, dt_rank). B, C: (b, l, n)
        delta = F.softplus(self.dt_proj(delta))  # (b, l, d_in)
        y = self.selective_scan(x, delta, A, B, C, D)  # This is similar to run_SSM(A, B, C, u) in The Annotated S4 [2]
        
        return y
```

### 2.2 训练加速 —— 并行扫描

由于 Mamba 的上述操作，使其仅具有循环形式，而不具有卷积形式，那么其训练就无法实现并行化。这就需要研究其他方法来加速训练。每个新状态都是当前输入和先前状态的总和，后者是迄今为止所有先前状态的递归总和。

![](img/22338b70200b8b01fbc179ffc187cca5.png)

这种对数组的前缀和运算也称为扫描操作。因此，该算法的一个简单解决方案就是简单地循环遍历数组并跟踪前缀和，每个新的和将是前缀和+当前输入。这样就有   O(N) \mathcal{O}(N) 的时间复杂度，因此不可并行化。该算法的名称称为并行扫描，下图为其工作原理：

![](img/8f9551a7e35ccbf59efeaeff7e0b44be.png)

Mamba 不可并行化（因为它是时变的），因此需要依赖循环操作。 Mamba 的作者采用三种经典技术来提高循环操作速度：

+   并行扫描算法 (Parallel Scan)

+   核融合 (Kernel Fusion)

+   激活重计算 (Activation Recomputation)

### 2.3 Mamba 结构与实现

Mamba 模型是由多层 Mamba 层连接而成，与 Transformer 模型的层非常相似。 Mamba 区块的架构很大程度上受到Transformer 和 Hungry Hungry Hippo (H3) 架构的启发。

![](img/f17b4d302248aa0f1d94025458e9bba7.png)

Mamba 层是 H3 和门控 MLP 操作的组合。它首先将输入投影到隐藏状态维度，然后在投影维度上进行非线性卷积（ SILU 激活函数）。然后计算 SSM 操作。接下来我们进行跳跃连接操作。最后，用另一个线性投影缩小张量的大小就完成了。

![](img/5a4e3ba6e53239ed44e981e991c380b0.png)

下面看一下核心部分的官方实现，如下：

```
class MixerModel(nn.Module):
    def __init__(
        self,
        d_model: int,
        n_layer: int,
        vocab_size: int,
        ssm_cfg=None,
        norm_epsilon: float = 1e-5,
        rms_norm: bool = False,
        initializer_cfg=None,
        fused_add_norm=False,
        residual_in_fp32=False,
        device=None,
        dtype=None,
    ) -&gt; None:
        factory_kwargs = {&quot;device&quot;: device, &quot;dtype&quot;: dtype}
        super().__init__()
        self.residual_in_fp32 = residual_in_fp32

        self.embedding = nn.Embedding(vocab_size, d_model, **factory_kwargs)

        # We change the order of residual and layer norm:
        # Instead of LN -&gt; Attn / MLP -&gt; Add, we do:
        # Add -&gt; LN -&gt; Attn / MLP / Mixer, returning both the residual branch (output of Add) and
        # the main branch (output of MLP / Mixer). The model definition is unchanged.
        # This is for performance reason: we can fuse add + layer_norm.
        self.fused_add_norm = fused_add_norm
        if self.fused_add_norm:
            if layer_norm_fn is None or rms_norm_fn is None:
                raise ImportError(&quot;Failed to import Triton LayerNorm / RMSNorm kernels&quot;)

        self.layers = nn.ModuleList(
            [
                create_block(
                    d_model,
                    ssm_cfg=ssm_cfg,
                    norm_epsilon=norm_epsilon,
                    rms_norm=rms_norm,
                    residual_in_fp32=residual_in_fp32,
                    fused_add_norm=fused_add_norm,
                    layer_idx=i,
                    **factory_kwargs,
                )
                for i in range(n_layer)
            ]
        )

        self.norm_f = (nn.LayerNorm if not rms_norm else RMSNorm)(
            d_model, eps=norm_epsilon, **factory_kwargs
        )

        self.apply(
            partial(
                _init_weights,
                n_layer=n_layer,
                **(initializer_cfg if initializer_cfg is not None else {}),
            ))
```

官方也提供了多个版本的 checkpoint，下面以 mamba-2.8b 为例，实现其端到端的推理过程

```
import time
import json
import torch
import torch.nn.functional as F

from einops import rearrange
from transformers import AutoTokenizer, AutoModelForCausalLM
from mamba_ssm.models.mixer_seq_simple import MambaLMHeadModel

model_name = &quot;state-spaces/mamba-2.8b&quot;
prompt = &quot;I have a dream and I&quot;
promptlen = 100
genlen = 100
temperature = 1.0
topk = 1
topp = 1.0
minp = 0.0
repetition_penalty = 1.1
batch =1

repeats = 3
device = &quot;cuda:0&quot;
dtype = torch.float16

print(f&quot;Loading model {model_name}&quot;)
tokenizer = AutoTokenizer.from_pretrained(&quot;EleutherAI/gpt-neox-20b&quot;)
model = MambaLMHeadModel.from_pretrained(model_name, device=device, dtype=dtype)
model.eval()
print(f&quot;Number of parameters: {sum(p.numel() for p in model.parameters() if p.requires_grad)}&quot;)

torch.random.manual_seed(0)
tokens = tokenizer(prompt, return_tensors=&quot;pt&quot;)
input_ids = tokens.input_ids.to(device=device)
attn_mask = tokens.attention_mask.to(device=device)
max_length = input_ids.shape[1] + genlen

fn = lambda: model.generate(
        input_ids=input_ids,
        max_length=max_length,
        cg=True,
        return_dict_in_generate=True,
        output_scores=True,
        enable_timing=False,
        temperature=temperature,
        top_k=topk,
        top_p=topp,
        min_p=minp,
        repetition_penalty=repetition_penalty,
    )
out = fn()
 
print(tokenizer.batch_decode(out.sequences.tolist()))
torch.cuda.synchronize()
start = time.time()
for _ in range(repeats):
    fn()
torch.cuda.synchronize()
print(f&quot;Prompt length: {len(input_ids[0])}, generation length: {len(out.sequences[0]) - len(input_ids[0])}&quot;)
print(f&quot;{model_name} prompt processing + decoding time: {(time.time() - start) / repeats * 1000:.0f}ms&quot;)
```

结果如下所示：

```
Loading model state-spaces/mamba-2.8b
Number of parameters: 2768345600
[&#x27;I have a dream and I want to make it come true.\n\n&quot;The first thing is that we need more people in the community who are willing to help us out, because there\&#x27;s not enough of them,&quot; he said. &quot;We\&#x27;re going through this process now with our new building.&quot;\n\nHe added: &quot;It would be great if they could get some volunteers from the local business owners or maybe even residents themselves so that when we do go into construction mode on the project... then at least you know somebody will&#x27;]
Prompt length: 6, generation length: 100
state-spaces/mamba-2.8b prompt processing + decoding time: 569ms
```

## 三、语言模型之外的 Mamba

除了在语言模型上的应用外，Mamba 还在图像和时间序列模型上有所应用，下面分别介绍相关工作。

### 3.1 图像中的 Mamba

处理图像本质上比处理文本要复杂得多。因为图像不仅仅是像素的序列;它们还包含复杂的模式，变化的空间关系，以及理解整体环境的需要。这种复杂性使得视觉数据的有效处理成为一项具有挑战性的任务，特别是在规模和高分辨率下。

Mamba块是Vim的一个关键特性，通过使用位置嵌入标记图像序列，并使用双向状态空间模型压缩视觉表示，Vision Mamba可以有效地捕获图像的全局上下文。这种方法解决了可视数据固有的位置敏感性，这是传统Transformer模型经常遇到的一个关键问题，特别是在更高分辨率下。

![](img/91dc4baae72eb0906420fffeb750d641.png)

Vim模型首先将输入图像划分为小块，然后将小块投影到 token 中。这些 token 随后被输入到 Vim 编码器中。对于像 ImageNet 分类这样的任务，在 token 序列中添加了一个额外的可学习分类标记，与用于文本序列建模的Mamba 模型不同，Vim编码器在正向和反向两个方向上处理 token 序列。

ViM 的突出特点之一是其双向处理能力，类似于 LSTM 的工作原理。与许多以单向方式处理数据的模型不同，ViM 的编码器可以向前和向后处理 token。双向模型可以更丰富地理解图像上下文，这是准确图像分类和分割的关键因素。

一旦 token 被卷积并激活，算法就会执行额外的线性变换并应用 softplus 函数，以确保输出值保持正值。这些转换为 SSM 序列建模功能准备 token。

在 SSM 操作之后，该算法应用门控机制，通过 SSM 输出与 SiLU 激活的前向和后向序列的元素相乘来调制信息流。这种门控机制可能旨在控制每个方向处理的贡献。

![](img/e0ad96cb83c85f3268747b13686f6745.png)

最后一步是残差连接，将原始输入序列添加到门控输出中，这有助于保留前几层的信息并解决梯度消失问题。整个过程的输出是一个新的标记序列，它可能经过了复杂的转换，捕捉到了序列两个方向上的复杂依赖关系。最后，算法会返回这个经过转换的 token 序列。

在 ImageNet 分类、COCO 物体检测和 ADE20K 语义分割等基准测试中，ViM 不仅表现更好，而且效率更高。例如，在处理高分辨率图像（1248 × 1248）时，ViM 比 DEIT 快 2.8 倍，使用的 GPU 内存减少了 86%。考虑到处理高分辨率图像时通常面临的内存限制，这无疑是一个重大改进。

### 3.2 时间序列中的 Mamba

通过开始阶段的研究和讨论，可以看到，SSM 本来就是用来处理时间序列信号的，因此 Mamba 也可以直接用来处理时间序列的任务，以下是其简单的实现：

```
class TSModel(nn.Module):
def __init__(self, d_model, d_state, d_conv, expand, forecast, lookback, dropout=0.5, device=&quot;cpu&quot;):
    super(TSModel,self).__init__()
    self.device=device
    self.mamba = Mamba(d_model=d_model, d_state=d_state,d_conv=d_conv, expand=expand).to(device)
    self.d1_nn = nn.Dropout(p=dropout).to(device)
    self.fc1=nn.Linear(in_features=lookback*d_model, out_features=forecast).to(device)

def forward(self, input):
    bs=input.shape[0]
    h_out = self.mamba(input)
    h_out = rearrange(h_out, &#x27;b l c -&gt; b (l c)&#x27;)
    h_out = self.d1_nn(h_out)

    out = self.fc1(h_out)
    return out

def predict(self,input):
    with torch.no_grad():
        predictions=self.forward(input)
    return predictions
```

最后，比较一下 Transformer, RNN, Mamba 的区别：

![](img/3f613f9c7733644b24274a52eaad6f03.png)

## 参考资料

[1] [Introduction to Stat](https://link.zhihu.com/?target=https%3A//huggingface.co/blog/lbourdois/get-on-the-ssm-train)e Space Models (SSM)

[2] [Modeling sequences with str](https://link.zhihu.com/?target=https%3A//searchworks.stanford.edu/view/14784021)uctured state spaces

[3] [Mamba Simplified - P](https://link.zhihu.com/?target=https%3A//blog.premai.io/s4-and-mamba/)art 2 - S4 and Mamba

[4] zero-order hold – studywolf

[5] [http:// arrow.utias.utoronto.](https://link.zhihu.com/?target=http%3A//arrow.utias.utoronto.ca/~damaren/s9.pdf)ca /~damaren/s9.pdf

[6] [h](https://link.zhihu.com/?target=https%3A//arxiv.org/ftp/arxiv/papers/2312/2312.00752.pdf)ttps:// arxiv.org/ftp/arxiv/pap ers/2312/2312.00752.pdf

[7] [https:// medium.com/@oxenai/mamb a-linear-time-sequence-mo](https://link.zhihu.com/?target=https%3A//medium.com/%40oxenai/mamba-linear-time-sequence-modeling-with-selective-state-spaces-arxiv-dives-cf96518d7ec4)deling-with-selective-state-spaces-arxiv-dives-cf96518d7ec4

[8] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2401.09417.pdf)pdf/2401.0941 7.pdf

[9] [h](https://link.zhihu.com/?target=https%3A//medium.com/%40jelkhoury880/vision-mamba-2584a340f7db)ttps:// medium.com/@jelkhoury88 0/vision-mamba-2584a340f7db

[10] [ht](https://link.zhihu.com/?target=https%3A//www.reddit.com/r/MachineLearning/comments/1989o55/d_mamba_models_on_time_series_data/)tps://www. reddit.com/r/MachineLea rning/comments/1989o55/d_mamba_models_on_time_series_data/

[11] [A Visual Guide to Mamba an](https://link.zhihu.com/?target=https%3A//www.maartengrootendorst.com/blog/mamba/)d State Space Models

> 江雨霏霏江草齐，六朝如梦鸟空啼。 —— 韦庄《台城》