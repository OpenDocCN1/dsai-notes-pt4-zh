
# Transformer模型的基础演算

> 来源：[https://agijuejin.feishu.cn/docx/URVTd7XyJolK7ixgXHCcCaMxnzF](https://agijuejin.feishu.cn/docx/URVTd7XyJolK7ixgXHCcCaMxnzF)

> 作者：Quentin Anthony、Stella Biderman、Hailey Schoelkopf

> 翻译：贾川、徐佳渝、杨婷 | OneFlow

> 原文：https://mp.weixin.qq.com/s/0Er0UOk6Wdky-0gzeQxK0g

![](img/9fbb94da823d9831540cc6104458d7ad.png)

## 引言

Transformer语言模型的许多基本重要信息可以通过简单计算得出。不幸的是，这些计算公式在自然语言处理（`NLP`）社区中并不广为人知。AI非营利研究组织EleutherAI收集整理这些公式，并介绍这些公式的来源和重要性。

注：本文主要关注显存（`VRAM`）主导的训练成本。有关推理成本和时延方面的类似讨论，请参见此前发布的《 大型语言模型的推理演算 》。

（本文经授权后由OneFlow编译发布，译文转载请联系OneFlow获得授权。原文：https://blog.eleuther.ai/transformer -math/ ）

## 计算需求

Transformer模型的训练成本可通过以下基本公式计算得出：

![](img/6db2ae17a8caac959f25881c68d9c275.png)

这些公式由OpenAI的论文《Scaling Laws for Neural Language Models》（https://arxiv.org/abs/2001.08361）和DeepMind的论文《Training Compute-Optimal Large Language Models》（https://arxiv.org/abs/2203.15556）提出并经实验证实。

值得一提的是`C`的单位问题。`C`是对总计算量的度量，可以用多种单位来表示。例如：

![](img/ce1ba462c871e73a7eeae49c46f44da8.png)

`Actual FLOPs`是一个值得注意的有用概念。GPU加速器白皮书通常会宣传它们的理论FLOPs，但在实际情况下这些值往往无法实现（特别是在分布式设置下）。下面的“计算成本”部分指出了在分布式训练设置中 `Actual FLOPs` 的常见报告值。

备注：我们使用了吞吐量-时间版本（`throughput-time version`）的成本公式，这个公式已在一篇关于LLM训练成本的文章中（https://medium.com/@dzmitrybahdanau/the-flops-calculus-of-language-model-training-3b19c1f025e4）使用。

### 参数与数据集权衡（Tradeoffs）

严格来说，你可以使用任意数量的`token`训练Transformer模型。然而，训练的`token`数量会对计算成本和最终模型的性能产生很大影响，因此找到一个合适的平衡点非常重要。

让我们从关键问题 `计算最优（compute optimal）` 语言模型入手。关于参数数量的理论“`Chinchilla scaling laws`”指出，一个计算最优的语言模型拥有的参数数量和数据集大小需满足一个近似等式：`D=20P`。

这在特定意义上是最优的：假设使用1000个GPU运行1小时的成本与使用1个GPU运行1000小时的成本相同，如果你的目标是在最小化GPU训练成本的同时最大化模型性能，你就应该使用上面提到的公式。

我们不建议训练小于200B个`token`的大型语言模型。虽然对于许多模型来说这样才能达到“`Chinchilla`最优”，但最终的模型效果通常非常差。针对所有应用程序，我们建议要确定用例的可接受推理成本，然后尽可能地训练最大的模型，以使得尽可能多的`token`的训练保持在这一推理成本之下。

### 计算成本的工程要点

Transformer模型通常用GPU-hours或FLOP-seconds来表示计算成本。

+ `GPT-NeoX`在`Normal Attention`下实现了150 TFLOP/s/A100，而在`Flash Attention`下实现了180 FLOP/s/A100。这与其他高度优化的库在规模上是相符的，例如`Megatron-DS`的报告是在137至163 TFLOP/s/A100之间。

+ 就经验来看，我们应该始终能够达到120 FLOP/s/A100的计算性能。如果计算性能低于115 FLOP/s/A100，那么很可能是模型或硬件配置出了问题。

+ 使用高质量的互连技术（例如 `InfiniBand`），我们可以在数据并行维度上实现线性或亚线性扩展（`sublinear scaling`，即增加数据并行度应该近乎线性地增加总吞吐量）。下图展示了在Oak Ridge National Lab的Summit超级计算机上测试`GPT-NeoX`库的结果。注意：x 轴上是 V100，但文章中大部分数值示例针对的都是A100。

![](img/358dad52481a0977702d78c0e0dda937.png)

## 内存需求

通常，Transformer模型的规模是以其参数数量为指标的。然而，在确定一个模型是否可以匹配特定计算资源时，我们需要知道模型将占用多少字节空间。这样才能弄清楚多大的模型适合在本地GPU上进行推理，或者在一定的总加速器内存下可以训练多大的模型。

### 推理

#### 模型权重

![](img/7148ba7264ba1a07ddc4129d052255ca.png)

大多数Transformer都是以混合精度训练的，不是`fp16 + fp32`就是 `bf16 + fp32`。这减少了训练模型所需的内存量，以及运行推理所需的内存量。我们可以将语言模型从`fp32`转换为`fp16`甚至`int8`，而不会产生实质性的性能损失。这些数字指的是单个参数所需的位（bits）大小。由于一个字节（Byte）有8位，我们把这个数除以8，看看每个参数需要多少字节

![](img/65ec4ea2e5af68746922e16c8bcb91ff.png)

这里还有少量的额外开销，但通常与确定GPU所匹配的最大模型无关。根据经验来看，此额外开销≤20%。

#### 总推理内存

除了存储模型权重所需的内存外，在实际前向传递过程中还会有少量额外开销。根据经验来看，此额外开销≤20%，并且通常与确定GPU所匹配的最大模型无关。

总的来说，可通过以下公式来“判断一个模型是否适合进行推理”：

![](img/1855d128a74390005049d6781772d185.png)

本文不会探讨这种额外开销的来源，这部分内容将会在其他文章中详细介绍。接下来将继续谈谈模型训练的内存。如果想了解更多关于推理所需的计算，请查看此前发布的《 大型语言模型的推理演算 》。

### 训练

除了模型参数之外，训练还需要在设备内存中存储优化器状态和梯度。这就是为什么当问及“我需要多少内存来拟合模型X”时，会立即得到“这取决于训练或推理”的答案。通常情况下，训练需要的内存比推理更多。

#### 模型参数

首先，可以在纯`fp32`或`fp16`中训练模型：

![](img/c36fb5f87ab3e3ed7db8965d8ce057f3.png)

除了推理中讨论的常见模型权重数据类型外，训练还引入了混合精度训练，例如`AMP`。该技术旨在保持收敛性的同时最大化GPU张量核心的吞吐量。现代深度学习训练领域经常使用混合精度训练，因为：1) `fp32`训练稳定，但内存额外开销高且不能利用NVIDIA GPU张量核心；2) `fp16`训练稳定但难以收敛。注意：混合精度需要在内存中存储模型的`fp16`/`bf16`和`fp32`版本，因此需要 ：

![](img/abd8005cb6ffe37c34cb5d1977a12e99.png)

再加上在优化器状态中计算的模型副本，大小为(4 bytes/`param`) • (#`params`)的副本。

#### 优化器状态

`Adam`很神奇，但它的内存效率非常低。除了要求拥有模型参数和梯度参数的副本外，还需要额外保留三个梯度参数副本。所以，

![](img/3a4525fcef2787cf90813b6de6d51c33.png)

#### 梯度

梯度可以以`fp32`或`fp16`存储（注意，梯度数据类型通常与模型数据类型相匹配。因此，在使用`fp16`混合精度训练时，梯度通常以`fp16`存储），因此它们对内存额外开销的贡献为：

![](img/810818f98b5a0fa89589d76d0d42b47e.png)

#### 激活（Activations）和批量大小

对于大型语言模型的训练来说，现代GPU通常受制于内存而非浮点运算。因此，激活重计算（`activation recomputation`又称`checkpointing`）是一种非常流行的方法，可以用减少的内存成本换取额外的计算成本。

激活重计算的工作原理是重新计算某些层的激活，而不是将它们存储在GPU内存中。内存的减少取决于我们在决定清除哪些层时所作的选择，但`Megatron`的选择性重计算方案如下图所示：

![](img/39e510aa4b1ab2acfdff5e506d977b36.png)

其中，红色虚线表示 A100-80GB GPU 的内存容量，“present work”表示应用选择性激活重计算后的内存需求。更多有关信息和下列公式的推导，请参阅Reducing Activation Recomputation in Large Transformer Models（https://arxiv.org/abs/2205.05198）。

存储Transformer模型激活所需内存的基本公式如下：

![](img/482043792a9ae2d9fd7f2ce635e53920.png)

此外，所需的额外重计算也取决于方法的选择，但它受限于完整的额外 前向传递。因此，前向传递的更新成本由以下公式给出：

![](img/130f9c08ecf7e5ca000cfed4e1131341.png)

#### 总训练内存

因此，有关“模型是否适合训练”的较好的启发式答案如下：

![](img/f97db698fb77388c34035599f60f5dab.png)

#### 分布式训练

##### 分片（Sharded）优化器

优化器的大量内存额外开销是`ZeRO`和`FSDP`等分片优化器的主要动机。这种分片策略将优化器额外开销降低了`No.GPUs` 倍，这就是给定模型配置可能适合大规模但`OOM`适合小规模的原因。

如果要计算使用分片优化器进行训练所需的内存额外开销，则需要包含下图中的公式。有关分片优化的示例计算，请参见ZeRO论文中的下图（注意，通常分别表示为 `ZeRO-1`、`ZeRO-2`、`ZeRO-3`。`ZeRO-0` 通常意为“ZeRO 禁用”）：

![](img/39b48e511f4bb246c6f40be99c5c4888.png)

在本博文中（假定为混合精度和`Adam`优化器）：

+ `ZeRO-1`，

![](img/c519f1f78a94c9a0d614a3093f653ca8.png)

+ `ZeRO-2`，

![](img/922ab8b15d2602665caadee2e8296663.png)

+ `ZeRO-3`，

![](img/12ca68cbc5773d2d8a29079f73a5f7bf.png)

除非应用流水并行和/或张量并行，否则（`DP Degree`）就只是(`No.GPUs`)。相关详细信息，请参阅分片优化器 + 3D并行部分（https://eleutherai.notion.site/Transformers-Math-101-d2fcfc7a25d446388fde97821ad2412a#9c476d020d7641a299fb6be6ae82e9f8）。

注意，`ZeRO-3`引入了一组实时参数，因为`ZeRO-3`引入了一组配置选项（`stage3_max_live_parameters`、`stage3_max_reuse_distance`、`stage3_prefetch_bucket_size`、`stage3_param_persistence_threshold`），用于控制一次GPU内存中的参数量（参数越大，需要的内存就越多，但需要通信会减少）。此类参数会对总GPU内存产生重大影响。

注意，`ZeRO`还可以通过`ZeRO-R`对数据并行列（ranks）上的激活进行分区。这也会使 在张量并行度 的上面。有关更多详细信息，请阅读相关ZeRO论文（https://arxiv.org/abs/1910.02054 ）和配置选项（https://www.deepspeed.ai/docs/config-json/#activation-checkpointing）（注意在`GPT-NeoX`中，这是`partition_activations`标识）。如果你正在训练巨型模型，想以一些内存额外开销换取额外的通信成本，那么激活会成为瓶颈问题。将`ZeRO-R`与`ZeRO-1`一起使用的示例如下：

![](img/0cbd609d781643356b9d7a1d6669cf46.png)

##### 3D并行

`LLM`有3种主要并行方式：

**数据并行：** 在模型副本（可能是模型并行的副本）之间划分（split）数据

**流水或张量/模型并行：** 这些并行方式将模型的参数划分到GPU之间。此类方案需要大量通信的额外开销，但它们的内存的减少大约为：

注意，此等式是近似值，原因如下：

（1）流水并行不会减少激活的内存占用；

（2）流水并行需要所有GPU存储全部传输中的`micro-batches`激活，这对于大型模型非常重要；

（3）GPU需要临时存储并行方案所需的额外通信缓冲区。

##### 分片优化器 + 3D 并行

当`ZeRO`与张量和/或流水并行结合时，生成的并行策略会形成如下所示的网格：

![](img/ef8adc4e29c271c1cdfbf6e6e5c81adf.png)

一个重要的题外话：数据并行度（`DP degree`）对于计算训练的全局批量大小至关重要。数据并行度取决于完整模型副本数量：

![](img/2d2b96a364d56c86e332aa647c4dd364.png)

虽然流水并行和张量并行与`ZeRO`的所有阶段兼容（例如，张量并行的`ZeRO-3`会先对张量切片，然后在每个张量并行单元中应用`ZeRO-3`），但只有`ZeRO-1`倾向于与张量和/或流水并行结合使用时表现良好，这是因为梯度并行策略相互冲突（流水并行和`ZeRO-2`是划分梯度）（张量并行和`ZeRO-3`是划分模型参数），从而造成大量通信的额外开销。

将所有内容放在一起，进行典型的带有激活分区的3D并行`ZeRO-1`运行：

![](img/6d1445cc32200bd7a8a89815f0a648c6.png)

## 总结

EleutherAI的工程师们经常使用上述启发式方法规划高效的模型训练以及调试分布式运行。我们希望澄清这些经常被忽视的实现细节。

其他人都在看

+ [大型语言模型的推理演算](http://mp.weixin.qq.com/s?__biz=MzU5ODY2MTk3Nw==&mid=2247491309&idx=1&sn=407fefb7ec76a2c9fdfe1ae960f7de4d&chksm=fe4190dbc93619cd0b9bfecb979e142a125fd8548d323dd9bdc2cbeb619400202e6e1affced0&scene=21#wechat_redirect)

+ 向量嵌入：AutoGPT的幻觉解法

+ John Schulman：通往TruthGPT之路

+ 复杂推理：大型语言模型的"北极星"能力

+ 为什么ChatGPT用强化学习而非监督学习

+ OneEmbedding:单卡 训练TB级推荐模型不是梦

+ GLM训练加速：性能最高提升3倍，显存节省1/3

试用OneFlow: `github.com/Oneflow-Inc/oneflow/`
