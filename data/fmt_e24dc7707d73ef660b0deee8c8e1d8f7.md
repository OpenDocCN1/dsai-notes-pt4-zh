
# LLM参数高效微调综述和详解-从`Adapter`、`PrefixTuning`到`LoRA`

> 来源：[https://agijuejin.feishu.cn/docx/NP29d90mYo69wbxOz5fc8VcQnze](https://agijuejin.feishu.cn/docx/NP29d90mYo69wbxOz5fc8VcQnze)

> 作者：[LeonYi](https://www.zhihu.com/people/lei-yi-43-4)

> 原文：https://zhuanlan.zhihu.com/p/696057719

## 一、背景

### 1.1 Transformer结构

![](img/5313b269f1225363456f2ace924b0a3e.png)

![](img/7d63e14af9eb99cd27f4b57e0ad3a57b.png)

### 1.2 指令微调

在大量通用数据上进行预训练语言模型训练，然后再针对特定下游任务进行微调，达到领域适应（迁移学习）的目的。是`NLP`目前的主流范式。

![](img/951569978cfe47eab5f9f9344b493b7e.png)

指令微调在预训练语言模型微调的基础进行优化，其目的是尽量让下游任务的形式尽量接近预训练任务。

> 从而减少下游任务和预训练任务之间的`Gap`, 实现预训练语言模型适应下游任务，而非下游任务去适应模型

![](img/563412e7d44550431469948ac267098d.png)

指令微调的效果要优于基于`Zero/Few-shot`的提示词工程的上下文学习。

但随着预训练语言模型进入`LLM`时代，其参数量愈发庞大。全量微调模型所有参数所需的显存早已水涨船高。

例如：

> 全参微调`Qwen2-7B`预估要2张80GB的`A800`，160GB显存

> 全参微调`Qwen2-72B`预估要20张80GB的`A800`，至少1600GB显存

而且，通常不同的下游任务还需要`LLM`的全量参数，对于算法服务部署来说简直是个灾难（当然，一种折衷做法就是全量微调后把增量参数进行`SVD`分解保存，推理时再合并参数）。

![](img/eecc370b8d2ef23c78327de3f9b8b6ea.png)

### 1.3 PEFT综述

为了寻求一个不更新`LLM`全部参数的廉价微调方案，之前一些预训练语言模型的高效微调(`Parameter Efficient Fine-Tuning`, `PEFT`)工作，要么插入一些参数或学习外部模块来适应新的下游任务。

![](img/f32abe2f4cac356300c793315d892997.png)

现有的微调方法旨在解决大型语言模型微调的效率问题。以下是一些相关研究的概述：

1. `Adapters`：通过在`Transformer`模型的子层后引入小型全连接网络来实现参数高效的微调。
2. `BitFit`：通过仅微调网络的偏置项来实现参数效率。
3. `LoRa` (`Low-Rank Adaptation`) ：使用低秩矩阵分解来微调权重矩阵，从而减少可训练参数的数量。
4. `Compacter`：结合了Kronecker积和低秩矩阵来生成适配器权重，同时在不同层之间共享参数以提高效率。
5. `Soft Prompts`：通过在模型输入嵌入中添加可训练的张量（软提示）来优化模型行为，将离散空间中的提示寻找问题转化为连续优化问题。
6. `Prefix-Tuning`：与软提示类似，但在所有层的隐藏状态中添加可训练参数。
7. `Intrinsic Prompt Tuning` (`IPT`) ：通过预训练软提示并使用自编码器来压缩其维度，从而减少每个新任务所需的参数数量。
8. `Ladder-Side Tuning` (`LST`) ：通过在预训练网络旁边训练一个小的`Transformer`网络，即`side network`，利用预训练模型作为特征提取器，减少训练时的内存和计算需求。`side network`用门控网络来组合了预训练`backbone network`和自身的隐状态.`side network`只用了预训练模型作为特征提取器，反向传播计算只在`side network`.
9. `(IA)3` ：通过学习新的参数来重新缩放关键、值和隐藏`FFN`激活，实现高参数效率。
10. `SparseAdapter`：通过在初始化时稀疏化适配器层并进行修剪，提高参数效率。
11. `UniPELT` ：结合了`LoRA`、`Prefix-Tuning`和`Adapters`的方法，通过门控机制实现参数高效的微调。
12. `S4` ：通过自动化算法搜索，结合所有`PEFT`类别以在额外0.5%的参数计数下最大化准确性。

**方法分类**：

![](img/5322ac625ae894150cdb90d5a43615c0.png)

Additive methods : Augmenting the existing pre-trained model with extra parameters or layers and training only the newly added parameters.

`Adapters`: A type of additive `PEFT` method that involves introducing small fullyconnected networks after Transformer sub-layers.

`Soft Prompts`: A part of the model’s input embeddings is fine-tuned via gradient descent. Soft prompts can be trained for the input layer only or for all layers.

Selective methods: 早期的Selective PEFT仅仅微调网络的最后几层, 即freeze这种经典的迁移学习套路。Modern approaches are usually based on the type of the layer or the internal structure, such as tuning only model biases or only particular rows. An extreme version of selective methods is sparse update methods which can completely ignore the structure of the model, and select parameters individually 。

Reparametrization-based methods: Reparametrization-based `PEFT` methods leverage low-rank representations to minimize the number of trainable parameters . 支撑就是empirical and theoretical analysis of dl证实neural networks有low dimensional representations。例如，`LoRA` employs a simple low-rank matrix decomposition to parametrize the weight update.

![](img/8a3c041a67a2957de683f053ddbbd73a.png)

![](img/080ebbc420aee2c289d30947810bed6a.png)

这一段主要基于：Scaling Down to Scale Up: A Guide to Parameter-Efficient Fine-Tuning

尽管提出了这么多方法，复杂的方法很少用，而实际使用的最多的可能就是`LoRA`，因为架构简单、效果不错，便于实现和推理。不然就是全参数微调了。毕竟宁愿加卡，也不愿去折腾奇奇怪怪的算法。

接下来将介绍如下4个`PEFT`方法 （重点是主流的`LoRA`）：

+ `Adapter Tuning`
+ `Prompt Tuning`
+ `Prefix Tuning`
+ `LoRA`

![](img/2a7da7710897cd4fc707f4683ad166af.png)

## 二、参数高效微调

### 2.1 Adapter Tuning

`Adapter Tuning`试图在`Transformer Layer`的`Self-Attetion`+`FFN`之后，插入一个先降维再升维的`MLP`（以及一层残差和`LayerNormalization`）来学习模型微调的知识。

![](img/48e30e13a67b12419dd3b1e39106b245.png)

`Adapter`即插入的`FF up` + `FF Down`。

![](img/c0bb4af37aeb36bc180589accf70c709.png)

在微调时，`Transformer Layer`原有的所有参数冻结，反向传播后仅更新`Adapter`参数。

![](img/219f1a2e11ab94c6bed60a5670c1aa3b.png)

缺点：需要修改原有模型结构，同时还会增加模型参数量。

### 2.2 Promot Tuning

`Promot Tuning`设计了一种`prefix prompt`方法，即在模型输入的`token`序列前添加前缀`prompt token`，而这个前缀`prompt token`的`embedding`是由网络学到。

`Promot Tuning`可以看做`token`已经确定，但是`embedding`是可以学的。

> 它相当于仅用`prompt token`的`embedding`去适应下游任务，相比手工设计或挑选`prompt`，它是一种`Soft`的`prompt`(软提示)

给定 `n` 个`token`组成的输入序列 `x1, x2, …, xn`，其对应`token embedding`矩阵为 `Xe ∈ R^{n×d}`，`d` 代表嵌入维度。

`Soft-prompts`对应参数 `Pe ∈ R^{p×d}`，`p` 代表`prompt`的长度。

然后，将`prompt`拼接到输入前面，就能得到完整的模型输入 `[Pe;Xe] ∈ R^{(p+n)×d}`。

这个新的输入将会送入模型 `f([P;X];Θ,Θp)` ，以最大化交叉熵损失来最大化条件概率 `Pr_{Θ,Θp}(Y|[P;X])` ，以拟合其标签`token`序列 `Y`。

> 在针对下游任务微调时，`Prompt Tuning`将冻结原始`LLM`的参数， 只学习独立的`prompt token`参数 （参数化的`prompt token`加上输入的`token`送入模型进行前向传播，反向传播只更新`prompt token embedding`的参数）。

在针对不同的下游任务微调时，就可以分别学习不同的`Task Specifical`的`Prompt Token`参数。

+ `Soft Prompt Tuning`在模型增大时可以接近`Model Tuning`(`fine-tuning`)的效果
+ 离散`Prompt Tuning`(`Prompt Design`)基本不能达到`Model Tuning`的效果

`Promot Tuning`方法的参数成本是 `ed` ，其中`e`是提示长度，`d` 是`token`嵌入维度。

> 提示越短，必须调整的新参数就越少，那么调参的目标是就是找到表现仍然良好的最小`prefix prompt`长度。

`Promot Tuning`在输入序列前缀添加连续可微的软提示作为可训练参数，其缺点在于：

> 由于模型可接受的最大输入长度有限，随着软提示的参数量增多，实际输入序列的最大长度也会相应减小，影响模型性能。

### 2.3 Prefix-Tuning

为了避免人为的离散`Prompt`选取优化，`Prefix-Tuning`提出可学习的`Prefix Prompt`。

> learns a sequence of prefixes that are prepended at every transformer layer

`Prefix tuning`为 `l` 层的`Transformer Layer`的每层多头注意力的键和值都配置了可学习的`prefix vectors`.

Two sets of prefix vectors `Pk` , `Pv ∈ R^{l×d}` are concatenated with the original key `K` and value `V`.

> 看到这里可以知道，`Prefix-Tuning`可以算是`Promot Tuning`的一个特例（`Promot Tuning`只在输入侧加入可学习的`Prefix Prompt Token`，`Prefix-Tuning`推广到`Transformer Layer`每一层的`K`、`V`上）

### 2.4 LoRA

`LoRA`（`Low-Rank Adaptation of LLMs`），即`LLMs`的低秩适应，被提出用于高效参数微调。

`LoRA`的核心思想，是假设 `LLM`在下游任务上微调得到的增量参数矩阵 `ΔW` 是低秩的 ，即是存在冗余参数的高维矩阵，但实际有效矩阵是更低维度的。

> 相关论文表明训练学到的过度参数化的模型实际上存在于一个较低的内在维度上。类似于机器学习中的降维算法，假设高维数据实际是在低维的流形上一样。

`LoRA`架构

全参数微调会优化`LLM`的所有参数。这相当于在`LLM`的原始权重 `W0 ∈ R^{d×d}` 上，加上一个了微调增量参数 `ΔW ∈ R^{d×d}` （相当于冻结原始权重，插入增量参数并仅对做优化）。

对于全参数微调后的`LLM`权重，给定输入 `x` ，其输出为下式：

`h = (W0 + ΔW)x = W0x + ΔWx`

既然，对增量参数矩阵 `ΔW` 有低秩假设。

那么在微调`LLM`时，完全可以对每一层的参数，加入参数 `B` 和 `A` 对 增量参数 `ΔW` 进行来低秩近似，同时只训练参数 `B` 和 `A` 。这就是上图右侧的`LoRA`微调。

这样一来，在微调过程中，可训练的参数量大大减少 （使得微调参数量从 `d × d` 降低至 `2rd` , 有 `r << d` ）

具体来说，`LoRA`将 `ΔW = BA ∈ R^{d×d}` 用两个更小的参数矩阵进行低秩近似，其中 `r` 是`LoRA`的需要近似 `ΔW` 的秩的维度，`B ∈ R^{d×r}` 和 `A ∈ R^{r×d}` 。
