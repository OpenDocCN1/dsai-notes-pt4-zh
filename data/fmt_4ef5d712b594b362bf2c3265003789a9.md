
# BridgeTower: 在视觉语言表示学习中建立编码器间的桥梁

> 来源：[https://agijuejin.feishu.cn/docx/SIFadGPy2o0wxtxee9cct4k9nPe](https://agijuejin.feishu.cn/docx/SIFadGPy2o0wxtxee9cct4k9nPe)

> 哈工大SCIR｜[阅读原文](https://mp.weixin.qq.com/s/igz2uvPWu3R031GaZuC2VA)

> 转载请联系原作者取得授权

> 论文名称：BridgeTower: Building Bridges Between Encoders in Vision-Language Representation Learning

> 论文作者：徐啸， 车万翔*等

> 原创作者：徐啸

> 论文链接：`https://arxiv.org/abs/2206.08657`

> 转载须标注出处：哈工大 SCIR

## 0. Take-away messages

+ 提出了一个简单有效的视觉语言模型架构，`BridgeTower`，通过在顶层单模态层和每个跨模态层之间建立桥梁，成功地引入了不同语义层次的视觉和文本表示，从而提高了跨模态编码器中注意力头的多样性，并在各种任务上实现了突出的性能改进。

+ 在公平的评估设置下，与`Two-Tower`架构的`METER`模型相比，`BridgeTower`显著地提高了模型的多模态表示能力。

+ 仅使用400万张图片进行视觉语言预训练，`BridgeTower`在各种视觉语言下游任务上取得了十分强大的性能，击败了许多用更多数据和参数进行预训练的强大模型。

+ `BridgeTower`可以适用于不同的视觉、文本或跨模态编码器。

## 1. 背景与动机

![](img/3c13332ae5a551b624ebadc723d06df9.png)

视觉语言任务示例

> 图源：12-in-1: Multi-Task Vision and Language Representation Learning

视觉语言研究的目标，是训练一个能够理解图像和文本的智能AI系统。上图展示了一些流行的视觉语言任务。视觉问答是其中最著名的任务之一，它需要根据输入图像来回答和图片相关的问题。

![](img/61733d38f7e854f137fc134419099373.png)

[0bc3naadoaaaueagj7jka5sfa2gdg5uaanya.f10002.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Fk0nbRom6oPSKWxtPkhc2UtDnub)

各类视觉语言模型

自2019年以来，在大规模图像-文本对的自监督预训练的帮助下，基于Transformer的视觉语言模型取得了显著的进展。其中，具有双塔结构的视觉语言 (VL) 模型在视觉语言表示学习中占主导地位。基于不同的文本和视觉编码器，人们提出了各种模型架构和预训练目标。从模型架构的角度来看，近期大多数的VL工作，可以看作是由三个模块组成的双塔架构，即文本编码器、视觉编码器，以及在它们之上的跨模态融合模块。不同的VL模型在这三个模块的设计上有所不同。

![](img/259b16f07caa62f78b7f1d64c0baaac4.png)

视觉语言模型架构简述

> 图(a)-(d)是目前的四类视觉语言模型。图(e)简要说明了`BridgeTower`的模型结构。`VE`、`TE`和`CE`分别是视觉编码器、文本编码器和跨模态编码器的简称。每个矩形的高度代表其相对计算成本。本图受到了`ViLT: Vision-and-Language Transformer Without Convolution or Region Supervision`的启发。

目前的VL模型要么使用轻量级的单模态编码器，并学习在深度跨模态编码器中同时提取、对齐和融合两种模态，要么将预训练的深层单模态编码器的最后一层单模态表示，送入顶部的跨模态编码器中。这两种方法都有可能限制视觉-语言表示的学习，并进一步限制模型的性能。

![](img/417e18f299ac2c029aad8260849e947f.png)

动机

如果我们深入双塔结构的单模态塔 (编码器) ，例如`METER`模型。我们可以发现他们只将最后一层的单模态特征直接送入顶部的跨模态融合模块，忽略了深层单模态塔的不同层的语义信息。我们自然地想到，能否在不同层的预训练单模态塔和跨模态融合模块之间建立起桥梁，以充分利用多层单模态特征？

![](img/3fe42401a82851088b8843e01369dc9a.png)

架构对比

因此，我们提出了`BridgeTower`架构。与双塔架构不同，`BridgeTower`在跨模态融合模块和单模态编码器之间建立起了多座桥梁。二者的主要区别在于，双塔结构只融合最后一层的特征，而`BridgeTower`则逐渐融合单模态编码器顶部的多层特征。

## 2. 模型架构

![](img/66a5b2ff225646010366573019581bc6.png)

模型架构

这里我们展示了`BridgeTower`的详细架构图。具体而言，我们采用12层的`RoBERTa-base`和12层的`CLIP-ViT-B`作为单模态编码器。跨模态编码器为6层，每一层都添加了`BridgeLayer`来与单模态编码器的顶部6层建立连接。

这使得预训练单模态编码器中的不同语义层次的视觉和文本表示，通过`BridgeLayer`与跨模态表示进行融合，从而促进了跨模态编码器中，高效的，自下而上的跨模态对齐与融合。需要注意的是，`BridgeTower`架构适用于不同的视觉、文本或跨模态编码器。

## 3. 设计选择

我们对`BridgeTower`的不同设计选择进行了广泛的实验。

### 3.1 BridgeLayer的定义

![](img/698201aabaa3e5aadf17c02290e3dfb8.png)

bridge-layer

首先是`BridgeLayer`的定义，也就是单模态信息与跨模态信息如何在`BridgeLayer`中融合。

![](img/c8095ae875c07efa92a93e0ea76bbca9.png)

上表展示了不同定义的`BridgeLayer`的参数量和其在`VQAv2`和`Flickr30K`数据集上的性能。`RSUM`表示图文检索任务的召回度量之和。  表示前一层输出的跨模态表示。   表示相应的单模态表示。我们省略了每一行中使用的  。有些出乎意料但又合乎情理的是，第一行中的  使用  最小 的参数量得到了  最好 的结果。

### 3.2 Cross-Modal Layer的数量

![](img/cac6f943657b14e7be3fc16c44736d3a.png)

cross-modal-layer

接着我们基于12层的文本和视觉编码器，研究不同数量的跨模态层对性能的影响。

![](img/37c786ee137f8669ec23de09ad78b011.png)

表示跨模态层的数量，并且`BridgeTower`使用Top-  的单模态表示作为跨模态层的输入。我们在两个数据集上比较不同  下，`METER`和`BridgeTower`的性能情况，我们发现更多的跨模态层并不能不断提高性能。这可能是由于

1. 更多的跨模态层需要更多的训练数据。

1. 顶层的单模态表示有利于跨模态对齐和融合，而底层的单模态表示可能不利于，甚至是有害于跨模态表示的学习。虽然`METER`和`BridgeTower`之间唯一的区别是`BridgeLayers`，但`BridgeTower`在不同数量的跨模态层中始终获得了一致的性能提升。

### 3.3 BridgeLayer的数量

![](img/35846f0019f454eda317c3719e02b25b.png)

internal-external

![](img/cec4bf4d88cf30fa1a77e118fb5aab77.png)

为了充分比较`BridgeTower`和双塔结构的`METER`模型，我们试图建立一个从`BridgeTower`到`Two-Tower`逐渐变化的情景。为了进行公平的比较，我们使用共计6个跨模态层，并将它们分为外部 (`External`) 跨模态层和内部 (`Internal`) 跨模态层。二者的区别在于内部跨模态层具有`BridgeLayer`，而外部跨模态层没有。

第一行显示了6个跨模态层均为内部层的`BridgeTower`的结果。然后，我们逐渐增加外部层，减少内部层。我们发现在两个数据集上的性能都出现了稳定的下降。

最后一行显示了双塔结构的`METER`模型的性能。这表明`BridgeTower`通过`BridgeLayers`，将单模态编码器的顶层与跨模态编码器的每一层连接起来，可以显著提高性能。

### 3.4 单模态编码器

![](img/6bcc61d14708da6915977a7f643ec061.png)

最后我们尝试了不同的视觉和文本编码器作为`BridgeTower`的预训练单模态编码器，并直接对下游任务进行微调，以进一步研究`BridgeLayers`带来的影响。我们发现，对于不同的预训练视觉和文本编码器，`BridgeTower`的性能都持续且显著地优于`METER`的性能。

## 4. 实验效果

![](img/4fdab27e7947db889a42b46d79828f9e.png)

我们基于公共图文对数据集对`BridgeTower`进行预训练，如上表所示，大约共计400万张独立图片，900万对图文对。我们使用通用的掩码语言建模 (`Masked Language Modeling`, `MLM`) 和图文匹配 (`Image-Text Matching`, `ITM`) 任务作为预训练任务。所有的预训练设置与预训练参数都与`METER`一致，以提供`METER`和`BridgeTower`之间的公平比较。

![](img/fff9a770222eba83eeced05157e5f22a.png)

上图展示了`BridgeTower`模型在视觉问答 (`Visual Question Answering`) 的`VQAv2`数据集上的`Base`和`Large`两种Size的模型性能。在视觉-语言预训练中，我们的`Base`模型只使用了400万张图片进行预训练，就在`VQAv2`基准上取得了令人印象深刻的表现。

而且，`METER`和`BridgeTower`使用相同的文本编码器、视觉编码器和跨模态融合机制。只需将`METER`模型的`Two-Tower`架构改为`BridgeTower`架构，在相同的预训练数据和几乎可以忽略不计的额外参数和计算成本下，`VQAv2`数据集的`Test-Standard`性能就可以轻松提高 1.09 。`BridgeTower`的`Large`模型在`VQAv2`数据集上更是取得 81.15 的`Test-Standard`性能。

值得注意的是，`BridgeTower`超过了许多使用 10倍 甚至 100倍 的图像进行VL预训练的`Base`模型与`Large`模型，击败了许多用更多数据和参数进行预训练的强大模型。

![](img/2c9f5ead1395f6a21a6952718e2b1c3c.png)

类似的趋势也出现在视觉蕴含 (`Visual Entailment`) 和图像-文本检索 (`Image-Text Retrieval`) 任务中。特别是在`Flickr30K`数据集上，`BridgeTower`的`Base`模型带来了 5.9 点收益。

## 5. 可视化结果

为了进一步研究性能提高的原因，我们通过分析每个跨模态层中，不同注意力头的注意力权重分布之间的`KL`散度，来比较双塔架构的`METER`模型和我们的`BridgeTower`架构。

`KL`散度可以被看作是注意力头的多样性。较高或较低的`KL`散度表示不同的注意力头之间，关注的`token`更加不同或更加相似。

![](img/b636ef896da38b7dafeae743b73a5dca.png)

> 图中的小点代表不同注意力头的注意力分布间的`KL`散度，大点表示同层`KL`散度的均值。上图对比了`METER`和`BridgeTower`模型的跨模态编码器中，视觉/文本部分的自我/交叉注意力层之间的区别。

上图展示了两个模型的跨模态编码器的视觉和文本部分的自注意力以及交叉注意力的注意力头的多样性。图中存在两个明显的趋势：

1. 对于`BridgeTower`来说，注意力头的多样性随着层的深入而逐渐变小，但对于`METER`来说，注意力头的多样性随着层的深入而逐渐变大，然后变小。

1. `BridgeTower`每层的注意力头的多样性明显大于`METER`，尤其是第1层至第5层。

因此，对于跨模态编码器的视觉和文本部分的自注意力以及交叉注意力的不同注意力头，与`METER`相比，`BridgeTower`能够关注到更多不同的标记 (`token`)。

我们将此归功于我们提出的`BridgeLayers`，它将单模态编码器的顶层与跨模态编码器的每一层连接起来。不同语义层次的视觉和文本表示通过`BridgeLayer`与跨模态表示进行融合，从而促进了跨模态编码器每一层的更有效和更丰富的跨模态对齐和融合。

## 6. 结论

在本文中，我们提出了`BridgeTower`，它引入了多个`BridgeLayer`，在单模态编码器的顶层和跨模态编码器的每一层之间建立连接。这使得预训练单模态编码器中的不同语义层次的视觉和文本表示，通过`BridgeLayer`与跨模态表示进行融合，从而促进了跨模态编码器中，高效的，自下而上的跨模态对齐与融合。

仅使用400万张图像进行视觉语言预训练，`BridgeTower`在各种下游的视觉-语言任务中取得了非常强大的性能。特别是在`VQAv2`数据集上，`BridgeTower`达到了 78.73% 的准确率，在相同的预训练数据和几乎可以忽略不计的额外参数和计算成本下，比`Two-Tower`架构的`METER`模型高出了 1.09% 的准确率。注意的是，当进一步扩展该模型时，`BridgeTower`达到了 81.15% 的准确率，甚至超过了一些在更大数量级的数据集上使用更多参数进行预训练的强大模型。

## 7. 附录

我们在附录中给出了更加丰富的实验结果与分析，包括：

+ `BridgeTower`和`METER`模型在单模态任务 (`CIFAR-10`, `CIFAR-100`, `GLUE`) 的性能与分析

+ `BridgeTower`和`METER`模型的参数量、计算量、推理时间和性能的详细比较与分析

+ `BridgeTower`在其他下游任务 (`Visual Reasoning` - `NLVR`, `Image-Text Retrieval` - `COCO`) 的性能

+ `BridgeTower`和`METER`模型在`VQAv2`上的详细性能比较，以及特定案例的分析

+ `BridgeTower`的预训练和下游任务微调的详细参数配置

欢迎感兴趣的同学阅读我们的论文，尤其是关于单模态任务的实验结果与分析。文中阐明了对于单模态实验结果的思考与可能的解决方案，该问题对多模态学习的研究是非常有价值的。

本期责任编辑：徐 啸

本期编辑：童彦澎
