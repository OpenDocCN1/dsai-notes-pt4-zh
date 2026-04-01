# LLM（廿七）：KL 散度与 LLM 知识蒸馏

> 来源：[https://agijuejin.feishu.cn/docx/Ge9Ld4EnZoXFBux5HCecoJb0nNf](https://agijuejin.feishu.cn/docx/Ge9Ld4EnZoXFBux5HCecoJb0nNf)

> 作者：紫气东来

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/692268639)hihu.com/p/692268...

在 LLM 时代，由于 Transformer 模型的“不可能三角”的存在，我们面临这样一种困境：

+   在训练时希望模型尽可能大以获得更好的效果

+   在推理时又希望模型尽可能小以更好的时延和吞吐

在当前处理这种矛盾的典型做法是模型压缩，包括稀疏、量化、蒸馏，在之前的文章中专门讨论过稀疏和量化技术，本篇则主要关注知识蒸馏。

要实现知识蒸馏，关键问题是度量两种分布的差异，而度量方法最常用的则是 KL 散度(Kullback-Leibler Divergence) 。由于 KL 散度的不对称性，Farward KL(FKL) 和 Reverse KL(RKL) 表现出不同的特点，从而引发较多的研究和讨论。本文将从分布的度量出发，深度研究 KL 散度的特性，并分析对模型蒸馏的影响。

## 一、剖析 KL 散度

### 1.1 KL及其基本特性

按照 KL 散度的定义，P 代表真实分布，Q 代表预测分布，则

则可以得到如下的离散形式的 KL 散度的表达式

**（1）非负性**

不等式部分使用了 **J**ensen's inequality .

**（2）非对称性**

可知当且仅当 P(x)=Q(x) P(x)=Q(x) 时，   DKL(P‖Q)=DKL(Q‖P) D_{K L}(P \| Q)=D_{K L}(Q \| P) 。

**（3）凹性**

其中，不等式部分用到了 **l**og sum inequality .

### 1.2 优化理论中的 FKL 与 RKL

首先假设目标分布 P 如下图所示

![](img/03f2d46c37e20ba4257f2dff8fc6ef46.png)

在通常情况下 Q 通常由参数   \theta 表征，则

+    : P基于Q的KL，或从Q到P的KL，此处称为正向KL (FKL)

+    : Q基于P的KL，或从P到Q的KL，此处称为反向KL (RKL)

**最小化 FKL 目标函数**

可见在此情况下和交叉熵优化是等价的，即通过进行 最大似然估计。也就是说，数据 由 产生，基于这些数据，选取 让 平均在 上的似然函数最大。直观的理解则是：

**平均概率高的地方，**** ****概率也要高**

**如果 Q 满足高斯分布，则**最终 FKL 得到的是如下图所示的 mean-seeking 的结果

![](img/15f1247b2d300afaa7d58b6314fba665.png)

**最小化 RKL 目标函数**

此时，需要选取参数 ，让平均在 上的 似然函数最大; 同时，让Shannon熵 也比较大，即约束不要过于集中，可以理解为 起到了一个正则化（regularization）的效果。总的来看，即：

**平均概率高的地方，**** ****概率也要高，但**** ****不能过于集中**

**如果 Q 满足高斯分布，则**最终 RKL 得到的是如下图所示的 mode-seeking 的结果

![](img/2a43ee859de67f2f16a70550150cfa2e.png)

### 1.3 mean-seeking 与 mode-seeking 的论证

仍然假设 P 满足高斯混合分布 (Gaussian Mixture Models, GMM), Q 满足高斯分布，现在要求其[ FKL 和 RKL 最小时 Q 的均值和方差。由于高斯混合分布无法求的其K](https://link.zhihu.com/?target=https%3A//ieeexplore.ieee.org/document/4218101)L散度解析解，只能估计其近似值，具体可参考 APPROXIMATING THE KULLBACK LEIBLER DIVERGENCE BETWEEN GAUSSIAN MIXTURE MODELS ，那么本节将采用实验的方式来论证。

首先论证 mean-seeking ，论证步骤如下：

+   构造出分布 P（如下图小图中的绿色概率分布曲线）

+   首先假设高斯分布Q的 std=1, 计算 FKL 与 Q 的均值的关系（下图左），并标注最小值点

+   以上一步最小值点的值作为 Q 的均值， 计算 FKL 与 Q 的 std 的关系（下图右）

通过以上分析及曲线，可以得到如下结论：

+   FKL-mean 曲线中有唯一的极小值点，该点的均值非常接近于 P分布的两个均值点的平均值，这也是 mean-seeking 的根本原因

+   Q 的 std 对 FKL 值几乎无影响，及 FKL 只对均值敏感，而对 std 不敏感

![](img/94ddd9454ae8004f2c6771b3d8148f37.png)

接下来论证 mode-seeking ，论证步骤如下：

+   构造出分布 P（如下图小图中的绿色概率分布曲线）

+   首先假设高斯分布Q的 std=1, 计算 RKL 与 Q 的均值的关系（下图左），并标注最小值点

+   以上一步最小值点的值作为 Q 的均值， 计算 RKL 与 Q 的 std 的关系（下图右）

通过以上分析及曲线，可以得到如下结论：

+   RKL-mean 曲线中有两个极小值点，分别位于 P 分布的两个均值点附近，其中最小值点位于P的主均值附近，这也是 mode-seeking 的根本原因

+   Q 的 std 对 RKL 的影响比较特殊，std 在一定范围内有极小值，其中极小值点附近特征如下图右小图所示。超过一定范围呈递减趋势

![](img/19f343ecb9481e17c48ef5bac652d002.png)

综合以上分析，可以进一步比较一下两种方式：

1.  FKL 的优化过程关注于样本整体的均值，这一过程类似于监督学习的优化过程，即

包括分类模型采用的交叉熵损失，以及回归模型的均方误差，本质都是最小化模型经验风险。

1.  RKL 的优化过程即关注于均值，又关注于方差，这一过程类似于强化学习中对于累计回报的优化过程，即

这也是 RL 算法(如TRPO，PPO)中非常关注于减小方差的原因。

以上分析的代码我开源在了 [h](https://link.zhihu.com/?target=https%3A//github.com/ifromeast/AI_analysis/blob/main/KL.ipynb)ttps:// github.com/ifromeast/AI _analysis/blob/main/KL.ipynb  。

## 二、LLM 的知识蒸馏

在上文关于 KL 的分析和讨论中，有两个非常强的假设，即：

+   P 满足 GMM 分布

+   Q 满足高斯分布

实际上 LLM 输出的 logits 经过 softmax 后，既不满足高斯分布，又不满足 GMM 分布。一些研究也发现，单纯的 FKL 和 RKL 损失对模型最终效果的影响并不明显，而二者的调和反而有更好的效果，如下：

![](img/7cfc30c91c70b7f70f23f3964d3504c2.png)

### 2.1 LLM 蒸馏的实现与工程化

相较于之前的非大模型，如 BERT，LLM 的知识蒸馏遵循着相同的流程与逻辑，即如下图所示。其中的关键是计算 Distillation loss, 而 Distillation loss 的计算依赖于词表，因此 LLM 的蒸馏需要保证词表的一致性，异源模型无法进行直接蒸馏。

![](img/3733c45f6e921d3c46abc34b65405db3.png)

以下为参考 miniLLM 实现的 RKL 的代码：

```
class KDLoss(nn.Module):
    &quot;&quot;&quot;
    Language Model Knowledge Distillation Loss
    &quot;&quot;&quot;

    def __init__(self):
        super().__init__()
        self.IGNORE_INDEX = -100

    def forward(self, logits: torch.Tensor, teacher_logits: torch.Tensor, label: torch.Tensor) -&gt; torch.Tensor:
        teacher_probs = F.softmax(teacher_logits, dim=-1, dtype=torch.float32)
        inf_mask = torch.isinf(logits)
        logprobs = F.log_softmax(logits, dim=-1, dtype=torch.float32)
        prod_probs = torch.masked_fill(teacher_probs * logprobs, inf_mask, 0)
        x = torch.sum(prod_probs, dim=-1).view(-1)
        mask = (label != self.IGNORE_INDEX).int()
        distil_loss = -torch.sum(x * mask.view(-1), dim=0) / torch.sum(mask.view(-1), dim=0)

        return distil_loss
```

训练的过程类似于 DPO，KTO 的过程，即一个模型推[理，](https://link.zhihu.com/?target=https%3A//github.com/OpenLLMAI/OpenRLHF/blob/main/examples/scripts/train_knowledge_distillation.sh)一个模型训练，具体执行过程可以参考 OpenRLHF/examples/scripts/train_knowledge_distillation.sh at main · OpenLLMAI/OpenRLHF (github.com) 。

## 参考资料

[1] KL Divergence for Machine Learning

[2] [Va](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2106.15980.pdf)riational Refinement for Importance Sampling Using the Forward Kullback-Leibler Divergence

[3] [https:// towardsdatascience.com/ forward](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/forward-and-reverse-kl-divergence-906625f1df06)-and-reverse-kl-divergence-906625f1df06

[4] $KL(q | p)$ is mode-seeking

[5] [Divergence measures](https://link.zhihu.com/?target=https%3A//groups.seas.harvard.edu/courses/cs281/papers/minka-divergence.pdf) and message passing

[6] [BE](https://link.zhihu.com/?target=https%3A//openreview.net/pdf%3Fid%3D2cRzmWXK9N)YOND REVERSE KL: GENERALIZING DIRECT PREFERENCE OPTIMIZATION WITH DIVERSE DIVERGENCE CONSTRAINTS

[7] [Re](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2404.02657.pdf)thinking Kullback-Leibler Divergence in Knowledge Distillation for Large Language Models

[8] [KL散度(Kullback-Leibler ](https://link.zhihu.com/?target=https%3A//hsinjhao.github.io/2019/05/22/KL-DivergenceIntroduction/)Divergence)介绍及详细公式推导

[9] [墨滴社区](https://link.zhihu.com/?target=https%3A//mdnice.com/writing/4333157f1a544436be287d4b3e4601aa)

[10] [APPROXIMATING THE KULLBACK LEIBLER DI](https://link.zhihu.com/?target=https%3A//ieeexplore.ieee.org/document/4218101)VERGENCE BETWEEN GAUSSIAN MIXTURE MODELS

> 伫倚危楼风细细，望极春愁，黯黯生天际。 —— 柳永《蝶恋花》