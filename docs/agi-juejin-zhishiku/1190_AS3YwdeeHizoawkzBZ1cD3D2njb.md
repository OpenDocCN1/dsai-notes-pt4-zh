# LLM（八）：大语言模型的稀疏化技术

> 来源：[https://agijuejin.feishu.cn/docx/KtL5dnkbWodr5oxYH8lcQ4VQnie](https://agijuejin.feishu.cn/docx/KtL5dnkbWodr5oxYH8lcQ4VQnie)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/615399255)hihu.com/p/615399...

随着深度学习模型，特别是语言模型的规模的快速增大，对计算资源和时延要求越来越高。由此，以稀疏化为代表的模型压缩技术应运而生，这些方法为模型压缩和加速提供了一种可能的方向。本文将通过对主流稀疏技术的梳理和实践，来理解其对大规模语言模型的影响。

## 一、从数据无关到数据驱动的方法

### 1.1 Magnitude pruning (MaP) —— 数据无关的稀疏算法

MaP 是一种典型的数据无关的方法，此类方法基于一个思想：将权重低于某个阈值的全部被剪枝，之后进行fine-tuned直到准确率达到预期。

考虑一个典型的 MLP+ReLU 的场景，即   Y=max(0,XW) Y=max(0, XW) ，由于 ReLU 函数的存在，会导致结果出现大量的0值，那么就可以考虑直接将权重   W W 中的绝对值较小的值直接赋0处理，即

Mij=Topv⁡(S)ij={1ifSijin topv%0otherwise \mathbf{M}_{ij} = \operatorname{Top}_v(\mathbf{S})_{i j}=\left\{\begin{array}{lr} 1 & \text { if } S_{i j} \text { in top } v \% \\ 0 & \text { otherwise } \end{array}\right.

其中   S=(|Wij|)1≤j,j≤n \mathbf{S}=\left(\left|W_{i j}\right|\right)_{1 \leq j, j \leq n} 表示重要性分数。

那么结果即变为   Y=max(0,X(W⊙M)) Y=max(0, X(W \odot M) ) ，其中   ⊙ \odot 表示 Hadamard 积。其过程可用下图形象表达：

![](img/a146cbe3654db8e86d071e765b51c748.png)

下面通过一个小例子来说明操作的过程，在下面这段代码中，先取出权重，然后进行从小到大排列。基于稀疏百分比（sparsity_percentage=0.7），把权重中的从小到大排列的前百分之七十的权重设置为0。

```
import numpy as np

weights = np.array([[-0.4711, 0.4448, 0.065], [0.4070, 0.4301, -0.7560]])
weights_ = np.linalg.norm(weights, axis=0)  # [0.62256262 0.61873504 0.75878917]
ind = np.argsort(np.linalg.norm(weights, axis=0))  # [1 0 2]
sparsity_percentage = 0.7
cutoff = int(len(ind) * sparsity_percentage)  # 2
sparse_cutoff_inds = ind[0:cutoff]  # [1 0]
weights[:, sparse_cutoff_inds] = 0  # [[ 0.     0.     0.065], [ 0.     0.    -0.756]]
```

剪枝后需要重新训练网络，以补偿在网络performance上的损失。在重新训练的时候需要注意，剪枝后的那些权重在重新训练的过程中不能被更新。

下图展示了ResNet 模型的 Dense 层在稀疏前(a)，稀疏后(b)，以及重训后(c)的weight 值的分布情况。

![](img/ff0adee6e3db9ab2006c23cf39024fa5.png)

### 1.2 Movement Pruning (MvP) —— 数据驱动的稀疏算法

MaP 实际上只利用了模型权重的0阶信息（即绝对值），而 MvP 还利用了权重的一阶信息，即MvP 中的权重  W 和重要性分  S 都会在微调的过程中更新。

设  a_i=\sum_{k=1}^n W_{i, k} M_{i, k} x_k ，损失为  \mathcal{L} ，那么  S 的梯度如下：

\frac{\partial \mathcal{L}}{\partial S_{i, j}}=\frac{\partial \mathcal{L}}{\partial a_i} \frac{\partial a_i}{\partial S_{i, j}}=\frac{\partial \mathcal{L}}{\partial a_i} W_{i, j} x_j = \frac{\partial \mathcal{L}}{\partial W_{i,j}} W_{i, j} = \frac{\partial \mathcal{L}}{\partial W_{i,j}} \odot W_{i, j}

这说明即使在前向过程中  W 被mask了，其重要性分  S 也能被更新，  S 按如下方式更新：

S^{(t)}=\left|W^{(t)}\right|-\alpha \sum_{i=1}^t\left(\frac{\delta \mathcal{L}}{\delta W}\right)^{(i)} \odot W^{(i)}

其中超参  \alpha 用来平衡0阶和1阶的重要性分。

更新后再计算  \mathbf{M}= \operatorname{Top}_v(\mathbf{S}) ，即参数有远离0点趋势的权重重要性高，有趋向0点的权重重要性低。

![](img/37f1776c2df8484471a20f27d4e95075.png)

实际上该方法利用了  \mathcal{L} 的一阶泰勒展开，如果更加精细化还可以考虑二阶项

\delta \mathcal{L}=\mathcal{L}(\mathbf{W}+\delta \mathbf{W})-\mathcal{L}(\mathbf{W}) \approx \nabla_{\mathbf{w}} \mathcal{L} \delta \mathbf{W}+\frac{1}{2} \delta \mathbf{W}^{\top} \mathbf{H} \delta \mathbf{W}

其具体的训练代码可参考 [这里 ](https://link.zhihu.com/?target=https%3A//github.com/huggingface/transformers/blob/main/examples/research_projects/movement-pruning/masked_run_squad.py)，核心代码如下：

```
    optimizer_grouped_parameters = [
        {
            &quot;params&quot;: [p for n, p in model.named_parameters() if &quot;mask_score&quot; in n and p.requires_grad],
            &quot;lr&quot;: args.mask_scores_learning_rate,
        },
        {
            &quot;params&quot;: [
                p
                for n, p in model.named_parameters()
                if &quot;mask_score&quot; not in n and p.requires_grad and not any(nd in n for nd in no_decay)
            ],
            &quot;lr&quot;: args.learning_rate,
            &quot;weight_decay&quot;: args.weight_decay,
        },
        {
            &quot;params&quot;: [
                p
                for n, p in model.named_parameters()
                if &quot;mask_score&quot; not in n and p.requires_grad and any(nd in n for nd in no_decay)
            ],
            &quot;lr&quot;: args.learning_rate,
            &quot;weight_decay&quot;: 0.0,
        },
    ]

    optimizer = AdamW(optimizer_grouped_parameters, lr=args.learning_rate, eps=args.adam_epsilon)
    scheduler = get_linear_schedule_with_warmup(
        optimizer, num_warmup_steps=args.warmup_steps, num_training_steps=t_total
    )
```

训练的结果如下所示：

![](img/63606a6cc81d717f2f9d6e6e7435fad6.png)

下图灰色部分即被稀疏的部分，可以看出，MaP 保留的是原理0点的值，而MvP保留的是趋于远离0点的值。

![](img/3a5755f2555c28fcf6dc68884760c870.png)

## 二、PST —— 小参数训练的稀疏算法

Parameter-Efficient Sparsity Training (PST) 在MvP的基础上引入了两组小矩阵来计算权重的重要性，相比于原本需要与权重一样大的矩阵来保存和更新重要性指标，稀疏训练需要更新的参数量大大减少。该原理主要来源于一下特点：

低秩性（Low Rankness）

根据矩阵理论  rank(W \odot \frac{\partial \mathcal{L}}{\partial W}) \leq rank(W) * rank( \frac{\partial \mathcal{L}}{\partial W}) ，

由于  \frac{\partial \mathcal{L}}{\partial W} 是低秩的，  W 在训练过程中也接近低秩，由此可考虑使用两个低秩矩阵  A \in \mathbb{R}^{n\times r_1}, B \in \mathbb{R}^{r_1 \times k} 来表示一阶分量，即  \Delta S = AB 。

结构化（Structureness）

根据下图的统计结果，不同结构行和列在稀疏化的过程被稀疏的程度是不同的，基于这种发现，可以采用两个矩阵  R \in \mathbb{R}^{n\times 1}, C \in \mathbb{R}^{1 \times k} 来分别表示行和列的重要性分数。由此1阶分量可更新为：

\Delta S^{(t)}=\alpha_1 A^{(t)} B^{(t)}+\alpha_2\left(R^{(t)}+C^{(t)}\right)

其中  \alpha _1, \alpha _2 用来平衡二者的相对大小。

![](img/bf9fadd7ea0190c9e5d560e34c464f0b.png)

另外，还可以对  W 做  UV 分解，即  W^{(t)}=W^{(0)}+\beta U^{(t)} V^{(t)}

由此重要性分可完整表述为：

S^{(t)}=\left|W^{(0)}+\beta U^{(t)} V^{(t)}\right|+\alpha_1 A^{(t)} B^{(t)}+\alpha_2\left(R^{(t)}+C^{(t)}\right)

完整的输出可表示为

Y=X[(W^{(0)}+\beta U^{(t)} V^{(t)}) \odot f(\left|W^{(0)}+\beta U^{(t)} V^{(t)}\right|+\alpha_1 A^{(t)} B^{(t)}+\alpha_2\left(R^{(t)}+C^{(t)}\right),v)]

那么需要训练的参数包括  U, V, A, B, R, C ，参考下图所示：

![](img/34a3e54ca688f6ea74c42c9b14f8b418.png)

## 三、PLATON —— 考虑置信区间上界

在MaP算法中，使用0阶分量进行稀疏，实际上在复杂的模型结构中，较小的权重可能也会在输出中发挥很大作用。为更好分析权重对结果的影响，定义敏感度  I=|W \frac{\partial \mathcal{L}}{\partial W}| ，为避免不同batch的影响，进一步对敏感度进行平滑处理，即

\bar{I}^{(t)}=\beta_1 \bar{I}^{(t-1)}+\left(1-\beta_1\right) I^{(t)}

其中超参  \beta_1 \in (0,1) ，并且  \bar{I}^{(0)}=0 。

在得到平滑的敏感度后，还需要量化其不确定性，定义敏感度的不确定性为  U^{(t)}=|I^{(t)}-\bar I^{(t)}| ，同样对其进行平滑处理

\bar{U}^{(t)}=\beta_2 \bar{U}^{(t-1)}+\left(1-\beta_2\right) U^{(t)}

其中超参  \beta_2 \in (0,1) ，并且  \bar{U}^{(0)}=0 ，那么  \bar{U}^{(t)} 可认为是  \bar{I}^{(t)} 的置信上界 (upper confidence bound, UCB )。

那么重新定义重要性分为

S^{(t)} = \bar I^{(t)} \odot \bar U^{(t)}

考虑到  \bar I^{(t)} , \bar U^{(t)} 值较小（接近0），进一步进行指对数处理

S^{(t)} = e^{ log(\bar I^{(t)}) +c \cdot log(\bar U^{(t)}) }

其中  c 为平衡超参。

官方代码可参考 [这里 ](https://link.zhihu.com/?target=https%3A//github.com/QingruZhang/PLATON)，其中核心部分  \bar I^{(t)} , \bar U^{(t)} 计算方法如下：

```
local_step = global_step % self.deltaT
update_step = global_step // self.deltaT
if local_step == 0:
    self.exp_avg_ipt[n] = self.beta1 * self.exp_avg_ipt[n] + (1 - self.beta1) * self.ipt[n]
    if self.beta2 &gt; 0 and self.beta2 &lt; 1:
        self.exp_avg_unc[n] = self.beta2 * self.exp_avg_unc[n] + (1 - self.beta2) * (self.ipt[n] - self.exp_avg_ipt[n]).abs()
    elif self.beta2 == 2.:
        self.exp_avg_unc[n] = (update_step * self.exp_avg_unc[n] + (self.ipt[n] - self.exp_avg_ipt[n]) ** 2) / (update_step + 1)
    self.ipt[n] = (p * p.grad).abs().detach()
else:
    self.ipt[n] = (self.ipt[n] * local_step + (p * p.grad).abs().detach()) / (local_step + 1)
```

S^{(t)} 的计算方法如下：

```
if self.beta2 &gt; 0 and self.beta2 &lt; 1:
    is_dict[n] = self.exp_avg_ipt[n] * self.exp_avg_unc[n]
elif self.beta2 == 1.:
    is_dict[n] = self.exp_avg_ipt[n]
elif self.beta2 == 2.:
    is_dict[n] = self.exp_avg_ipt[n] * self.exp_avg_unc.sqrt()
else:
    # Handling the uncepted beta2 to default setting 
    is_dict[n] = self.exp_avg_ipt[n] * (self.ipt[n] - self.exp_avg_ipt[n]).abs()
```

下图是BERT模型在SQuAD任务上重训的结果，稀疏化率(1-prune_target) 逐渐从0上升到90%，在这个过程中loss在波动中下降，稀疏化的精度(eval_f1, eval_exect) 在稀疏率低时呈上升趋势, 随着稀疏率的提高开始下降且逐渐稳定。

![](img/4a5c0854384042193fc5630a95ecafae.png)

## 参考资料

[1] [Movement Pruning: Adaptive Spa](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2005.07683.pdf)rsity by Fine-Tuning

[2] [PL](https://link.zhihu.com/?target=https%3A//proceedings.mlr.press/v162/zhang22ao/zhang22ao.pdf)ATON: Pruning Large Transformer Models with Upper Confidence Bound of Weight Importance

[3] [P](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2205.11005.pdf)arameter-Efficient Sparsity for Large Language Models Fine-Tuning

[4] [tr](https://link.zhihu.com/?target=https%3A//github.com/huggingface/transformers/tree/main/examples/research_projects/movement-pruning)ansformers/examples/research_projects/movement-pruning at main · huggingface/transformers

[5] [Sparse is Enough in ](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2111.12763.pdf)Scaling Transformers

[6] 1506.02626.pdf (arxiv.org)

[7] [W](https://link.zhihu.com/?target=https%3A//lewtun.github.io/blog/weeknotes/nlp/huggingface/transformers/compression/few-shot/2021/01/24/wknotes-pruning-transformers.html)eeknotes: Fine-pruning transformers, universal data augmentation

[8] [https:// lilianweng.github.io/po](https://link.zhihu.com/?target=https%3A//lilianweng.github.io/posts/2023-01-10-inference-optimization/) sts/2023-01-10-inference-optimization/

> 浮天水送无穷树，带雨云埋一半山。 —— 辛弃疾《鹧鸪天·送人》