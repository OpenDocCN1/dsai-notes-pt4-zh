
注意力头维度`dmodel`/`nhead``dmodel`/`nhead`，即每个注意力头（`Head`）的大小。无论你把头的大小设为 64 还是 128，只要总参数量不变，对最终效果几乎没有影响。

### 批量大小 (Batch Size)

noise scale：在 `batch` 内随机采样时，你所预期的梯度噪声

![](img/16b94572410defb6fd5c50620f7ad07a.png)

图9.22 批量大小与临界值

当批量大小小于临界值时，增大批量大小能有效降低梯度噪声，训练速度近似线性提升（Perfect Scaling）。当批量大小超过临界值时，收益迅速递减（Ineffective Scaling）。

![](https://raw.githubusercontent.com/datawhalechina/diy-llm/main/docs/chapter9/images/9-23-%E4%B8%B4%E7%95%8C%E6%89%B9%E9%87%8F%E5%A4%A7%E5%B0%8F%E4%B8%8E%E6%A8%A1%E5%9E%8B%E6%80%A7%E8%83%BD.png)

图9.23 临界批量大小与模型性能

当你尝试降低损失，也就是图形从左向右移动（横坐标是 `10->6->4->3`），critical batch size 会变大，相应的，整体的 batch size 就会更大。

所以，目标损失越小，可以使用的整体batch size 就越大。

![](https://raw.githubusercontent.com/datawhalechina/diy-llm/main/docs/chapter9/images/9-24-%E9%80%89%E6%8B%A9%E6%9C%80%E4%BC%98%E7%9A%84%E6%89%B9%E9%87%8F%E5%A4%A7%E5%B0%8F.png)

图9.24 选择最优的批量大小

随着计算量和模型规模的增加，我们应该如何扩展训练规模？

+   大 batch size，步数不变
+   固定 batch size，更多步数

### 学习率 (Learning Rate)

当我们把模型做大时，学习率应该如何调整？通常如下左图所示，我们最优学习率依赖于模型规模。模型越大，最优学习率通常越小。

`μP` (Maximal Update Parametrization) 这项工作（右图），通过一种特殊的参数化和初始化方案（尺度感知初始化），可以使得最优学习率在不同模型规模下保持稳定。这意味着你可以在小模型上找到最优学习率，然后直接将其用于训练万亿参数的大模型，无需重新调整。

![](https://raw.githubusercontent.com/datawhalechina/diy-llm/main/docs/chapter9/images/9-25-%E6%A0%87%E5%87%86%E5%81%9A%E6%B3%95%E4%B8%8E%CE%BCP%E7%9A%84%E6%94%B9%E8%BF%9B.png)

图9.25 标准做法与`μP`的改进

下面的[表格](https://arxiv.org/pdf/2304.06875)展示了如何实现 `μP`。核心思想是根据模型的宽度变化比例 `rr` 来调整初始化和学习率。

![](img/81fd5c419c8cfcf39948e2259ef20eec.png)

图9.26 `μP`的不同实现

假设我们要把模型 `MM` 放大到 `M′M′`，宽度扩大了 `rr` 倍：

+   `AdamW` Learning Rate (matrix-like): 对于矩阵类的参数（如 `Transformer` 中的权重矩阵），学习率需要除以 `rr` (`l/rl/r`)。这是最关键的一步，通常意味着大模型的学习率要比小模型小。
+   Initialization Variance (matrix-like): 初始化的方差也需要除以 `rr` (`σ/rσ/r`)。这意味着大模型的初始权重应该更接近 0，以防止信号在深层网络中爆炸。
+   Others: 对于向量类参数（如 `Bias`, `LayerNorm`），通常保持不变。

总而言之，如果我们只是简单粗暴地把模型做大（`Naive scale up`），最佳学习率会变，导致训练困难。我们需要一种 `Scaling Aware` 的初始化和学习率设置策略（即 `μμP`），让超参数在不同规模的模型间保持稳定。

### 9.3.4 `scaling` 在不同的下游任务上表现不同

![](https://raw.githubusercontent.com/datawhalechina/diy-llm/main/docs/chapter9/images/9-27-scaling%20%E5%9C%A8%E4%B8%8D%E5%90%8C%E7%9A%84%E4%B8%8B%E6%B8%B8%E4%BB%BB%E5%8A%A1%E4%B8%8A%E8%A1%A8%E7%8E%B0%E4%B8%8D%E5%90%8C.png)

图9.27 `scaling` 在不同的下游任务上表现不同

左图，横坐标是计算量，纵坐标是困惑度，采用的都是`log`对数的形式。呈现出很好的相关性。

右图，纵坐标是 `superGLUE` 准确率，就没有线性关系了。有的模型明显比其他模型更好。

### 9.3.5 实践建议

在训练前可以有效选择优化器、模型深度、模型架构。先训练小模型，然后将结果外推，预测大模型的表现。

步骤：

+   在小模型上训练
+   建立某种扩展定律
+   设置最优超参数

## 9.4 联合缩放：模型、数据与计算 (`Joint Scaling`)

### 9.4.1 联合缩放定律

#### 模型大小 (`N`) 和数据量 (`D`)的联合缩放

在固定的计算预算下，我们应该训练一个更大的模型，还是用更多的数据训练一个较小的模型？

为了科学地解决这个问题，我们需要一个公式，能同时把“数据量 (`nn`)"和“模型大小 (`mm`)"都考虑进去。这就是联合缩放定律。

Rosenfeld et al. (2020) 提出 `Error=n−α+m−β+C`，这个公式直观地把误差拆成了三部分：

+   数据带来的误差 (`n−α`)：数据越少，这部分误差越大。
+   模型带来的误差 (`m−β`)：模型越小，这部分误差越大。
+   不可约误差 (`C`)：任务本身的难度底线。

这意味着，如果你的模型太小（`mm` 很小），中间那一项就会很大，无论你怎么增加数据（`nn`），总误差都降不下来。

Kaplan et al. (2020) 提出 `Error=[m−α+n−1]β``Error=[m−α+n−1]β`，这是 `OpenAI` 提出的另一种形式，同样描述了模型大小和数据量之间的耦合关系，但考虑的是可约误差项，所以没有常数项`C`

下图展示的是，在绿色的小数据和小模型上训练，再扩展到红色的大数据和大模型上训练：

![](https://raw.githubusercontent.com/datawhalechina/diy-llm/main/docs/chapter9/images/9-28-%E5%8F%82%E6%95%B0_%E8%AE%A1%E7%AE%97_%E6%95%B0%E6%8D%AE%E7%9A%84%E8%81%94%E5%90%88%E7%BC%A9%E6%94%BE.png)

图9.28 参数_计算_数据的联合缩放

横轴是参数量，颜色代表计算量，数据量是第三个轴。

#### Compute vs performance 上的权衡

这张图（来自 Kaplan et al. 2020/2021）展示了在不同算力预算下，模型大小与性能的关系。

![](img/98a37d094879d3b170f1cf282986d1d4.png)

图9.29 不同算力预算下模型大小与性能的关系

左图每一条颜色的线代表一个固定的算力预算。对于每一个固定的算力预算，都存在一个唯一的最优模型大小。如果你钱多，就应该造大模型；如果你钱少，造大模型反而效果不如小模型。

右图小模型一开始 `Loss` 下降很快（起步快），但很快就变平了。大模型一开始 `Loss` 比较高（起步慢，因为参数多难训练），但随着算力投入增加，它会反超小模型，并且 `Loss` 能持续下降到更低的位置。

### 9.4.2 chinchilla

DeepMind 的 `Chinchilla` 论文 (Hoffman et al., 2022) 通过大规模实验，精确地拟合了这个联合缩放定律，并给出了一个惊人的结论：

> 对于给定的计算预算，模型大小和数据量应该按比例增加。

之前的模型（如 `GPT-3`）普遍是“模型过大，数据不足”（Over-trained on too little data）。`Chinchilla` 发现，要达到计算最优（`Compute-optimal`），数据量（tokens）和模型参数量的比例大约应为 20:1。

这意味着，一个 `70B` 的模型（`Chinchilla-70B`），应该用大约 `70B×20=1.4T``70B×20=1.4T` tokens 来训练，其性能可以超过一个用更少数据训练的 `175B` 模型（`GPT-3`）。

`Chinchilla` 使用了三种方法来寻找在固定计算预算（`FLOPs`）下，模型大小（`N`）和数据量（`D`）的最优组合：

+   最小值包络法（Minimum over runs）：取所有训练曲线的下包络线。
+   等计算量分析法（`IsoFLOPs`）：在固定`FLOPs`下，扫描不同 `N` 和 `D` 的组合，找到性能最优的点。
+   联合拟合法（`Joint fits`）：在 `N-D` 网格上训练模型，直接拟合一个联合缩放函数。

这三种方法都指向了同一个结论：最优的 `D` 与 `N` 的缩放指数几乎都是 0.5，这意味着最优的 `D/N` 比例是常数。`Chinchilla` 得出的具体比例是 约 20 tokens per parameter。这意味着，与其训练一个巨大的模型，不如用相同的计算预算训练一个更小但数据量更充足的模型，后者性能更优。

### 9.4.3 从训练最优到推理最优

`Chinchilla` 定律是训练最优 (`Train-optimal`) 的，它的目标是在固定的训练计算预算下，获得性能最好的模型。但在实际部署中，推理 (`Inference`) 成本占据了模型生命周期总成本的大头。一个推理成本更低的小模型，即使训练成本稍高，也可能更具经济效益。

因此，业界趋势是“过度训练” (`Over-training`) 小模型，即用远超 `Chinchilla` 比例的数据来训练模型，以换取更强的推理能力。

+   `GPT3` – 2 tokens / param
+   `Chinchilla` – 20 tokens / param
+   `LLaMA65B` – 22 tokens / param
+   `Llama 2 70B` – 29 tokens / param
+   `Mistral 7B` – 110 tokens / param
+   `Llama 3 70B` – 215 tokens / param

这种趋势表明，为了降低推理时的延迟和成本，业界愿意在训练阶段投入更多的计算资源，以获得一个在给定能力下参数量更小、更高效的模型。

## 9.5 扩散模型的缩放法则

在此之前，`Scaling Laws` 主要是在自回归模型（[`Autoregressive Models`](https://arxiv.org/pdf/2305.18619)，即像 `GPT` 这样的大语言模型）上被广泛研究。`Likelihood-Based Diffusion Language Models` 研究了缩放定律（`Scaling Laws`）在扩散模型（`Diffusion Models`）上的验证结果,主要发现是扩散模型也遵循同样的缩放法则

![](img/81b35874374fe4759d1090cf881e79af.png)

图9.30 不同算力预算下模型大小与性能的关系

`Iso` 前缀意味着“相等”，`IsoFLOPS` 是在固定总计算预算的前提下，去寻找模型大小（参数量）和训练数据量的最佳平衡点。左图是自回归模型的 `IsoFLOP` 曲线。中图是扩散模型的 `IsoFLOP` 曲线。右图把左图和中图里所有的“星星”（最佳点）连了起来，在双对数坐标下，这些最佳点连成了一条直线，这意味着扩散模型也严格遵循幂律（`Power Law`）。只要我们增加算力，我们就能精准地预测出扩散模型能达到多好的效果。

## 9.6 SCALING 的案例研究及实现细节

随着大语言模型进入产品化时代，业界对核心缩放策略（如数据-模型权衡、超参选择）的披露变得愈发谨慎。因此，我们只能通过少数公开、详实的研究案例来窥探其内部逻辑。本节将深入剖析三个典范：`Cerebras-GPT`、`MiniCPM` 和 `DeepSeek`，使读者了解现代顶尖语言模型（如 `DeepSeek`, `MiniCPM`）的开发者究竟是如何运用缩放定律来设计和优化其模型的。

### 9.6.1 Cerebras-GPT

[`Cerebras-GPT`](https://arxiv.org/pdf/2304.03208) 遵循`Chinchilla`扩展法则，每个参数对应20个token，训练了从 `0.1B` 到 `13B` 的 7 个 `GPT-3` 风格模型。并首次公开验证了 `muP` (Maximal Update Parametrization) 在大模型扩展中的有效性。

+   采用 `muP` 的模型在扩展时表现出 更稳定的（`stable`） 规模化扩展

![](img/07211c68b4741e534345cae002169361.png)

图9.31 使用`mμP`展现了更稳定的扩展规律

+   使用 `mμP` 带来 更可预测（`predictable`） 的规模化扩展

研究人员首先使用 `mμP` 缩放定律训练了一个小型模型（`10M` 参数），然后将这些超参数迁移到更大的 `Cerebras-GPT` 模型上。

![](img/a337d97dbdabfb5e663333e1cacfff71.png)

图9.32 使用`mμP`展现了更可预测的扩展规律
