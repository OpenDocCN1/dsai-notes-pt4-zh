# 语义引导的生成式图像增广方法

> 来源：[https://agijuejin.feishu.cn/docx/DiI9dWaF6oGicuxAop5cUlpRn8f](https://agijuejin.feishu.cn/docx/DiI9dWaF6oGicuxAop5cUlpRn8f)

> 哈工大SCIR｜[阅读原文](https://mp.weixin.qq.com/s/8ibE0WGDLJZBhHqi3d9lnQ)

> 转载请联系原作者取得授权

> 论文： 语义引导的生成式图像增广方法

> 作者：李博涵，徐啸，王兴昊，侯宇泰，冯云龙，王峰，张玄靓，朱庆福， 车万翔

> 链接： https://arxiv.org/abs/2302.02070

> 转载须标注出处：哈工大SCIR

##  **背景 **

现有的图**像增**广方法分为两类：基于扰动的方法**和生**成方法。基于扰动的方**法应****用预**定义的扰动来增广原始图像，但大多局限于局部变化，因此缺乏图像多样性。相比之下，生成式方法在增广的图像中带来了更多的图像多样性，但可能无法保持语义一致性，因此可能错误地改变原始图像的基本语义。为了在增广图像中平衡图像多样性和语义一致性，我们提出了SGID( S emantic-guided G enerative I mage augmentation method with D iffusion models for image classification)，一种语义引导的生成式图像增广方法。

![](img/a331b505460646fac71f697d9b4a94ba.png)

图1:四种基线方法与SGID在四个数据集上基于ViT（ImageNet-21k）的表现。SGID在语义一致性和图像多样性之间取得了平衡，从而带来了最高的性能提升。

##  **SGID框架 **

在本文中，我们提出了SGID，一种语义引导的生成式图像增广方法。我们的目标不是在各种数据集和backbone上超越现有的图像增广基线，而是探索一种平衡图像多样性和语义一致性的方法，即保留原始图像的基本语义的同时带来良好的图像多样性。此外，SGID可以自然地与其他数据增广（DA）基线结合，并进一步提高它们的性能。SGID包括两个基本步骤，如图2所示：

![](img/f3e620d77129e3473079d953a4d39623.png)

图2:本文方法SGID的示意图。

1.  我们首先构建每张图像的文本标签，然后使用BLIP[1]生成caption，再使用CLIP[2]计算选定caption和原始图像之间的相似性。标签和caption都提供了原始图像的基本语义。

1.  我们根据每张图像的标签和caption构建提示。然后将提示连同原始图像一起输入到Stable Diffusion中。提示中包含的语义引导可以帮助生成多样化且语义一致的增广图像。

###  **标签获取和标题生成 **

在第一步中，我们从原始图像构建提示，作为后续第二步的语义引导。提示由原始图像的文本标签和caption组成。具体来说，对于每张图像 x，我们首先使用其真实图像标签来构建相应的句子作为文本标签 l：

![](img/57b51caa987f51b88fb604fbdfc4c0dc.png)

然后我们通过BLIP模型为图像生成caption：

![](img/f0c27d95961927f5769868627cdae621.png)

采样策略包括 beam search[3]和nucleus sampling[4]。对于每张图像，我们获得相应的caption集合，并从中随机选择一个caption。caption可以提供图像标签以外的语义信息，例如背景和颜色的描述。将caption与真实但通常过短的图像标签结合起来，可以为增广图像的生成提供有效的语义引导。此外，为了增加提示的语义一致性，我们还通过将CLIP作为一个可选过滤器来探索潜在更高质量的caption。给定生成的caption集合，我们使用CLIP计算原始图像与每个caption之间的相似性，并选择相似性最高的caption。

###  **图像到图像生成 **

在第二步中，我们采用文本标签和caption作为图像到图像生成的语义引导。我们首先将它们连接起来构建文本提示。例如，在图2中，“A picture of a [Chevrolet Silverado 1500 Extended Cab 2012], a 2009 chevrolet silverado in a desert.”文本提示不仅携带了来自图像标签的准确但简洁的图像语义，还包含了来自图像caption的整体图像描述。为了提供图像多样性，我们在Stable Diffusion中应用高斯噪声，噪声率为 n ，基于上述语义约束基于原始图像进行微小修改。

考虑到图像标签和图像caption的不同贡献，我们采用提示加权策略。具体来说，我们分别为标签和caption应用不同的权重，将权重与两者的向量表示分别相乘。此外，为了控制语义对图像生成的指导程度，我们采用指导映射策略，提供适当的指导比例 g。指导比例 g 表示图像生成过程中，在多大程度上遵循文本提示 p的引导。最后，Stable Diffusion根据上述元素生成增广图像:

![](img/0a4f3fd3aabd628b20969baebaadb234.png)

##  **实验 **

###  **实验设置 **

####  **数据集 **

我们在七个常用数据集上评估了我们提出方法的有效性，包括三个粗粒度分类数据集：CIFAR-10、CIFAR-100、Caltech101（Cal101），以及四个细粒度分类数据集：Stanford Cars（Cars）、Flowers102（Flowers）、OxfordPets（Pets）和纹理分类数据集 DTD。

####  **Backbones **

我们在三个backbones上进行实验，包括一个基础模型：ResNet-50 (from scratch) [5]，以及两个预训练模型：ViT（ImageNet-21k）[6]，CLIP-ViT（LAION-2B）[7]。

####  **基线方法 **

我们应用各种数据增广（DA）方法作为基线，包括四种基于扰动的方法：Random Erasing（RE）、CutMix、MoEx和RandAugment（RA），以及三种生成式方法：Text2Img、SGID+DiverseCaption（SGID+DC）和SGID+InstructPix2Pix（SGID+IP）。所有生成式方法都使用相同的图像生成模型SD。

###  **主实验结果 **

在本文中，我们在三个backbones上使用七个数据增广（DA）基线对七个数据集进行了实验（图3）。

![](img/d58599fa73eb84f9024d23ae7a2688e4.png)

图3:在七个数据集、三个backbone上，SGID与七种基线模型的实验结果，以及SGID与基线模型相结合的实验结果。

(1) 总体而言，SGID显示出正面效果，并在所有七个数据集和三个backbone上取得平均最高性能。具体来说，我们的方法在ResNet-50 (from scratch)上带来了10.39％的准确率提升，在ViT（ImageNet-21k）上为2.08%，在CLIP-ViT（LAION-2B）上为0.85%。这表明通过向扩散模型中加入语义引导的图像增广可以使不同的backbones受益。值得注意的是，SGID在CLIP-ViT（LAION-2B）上仍有效果，其预训练数据几乎与SD相同。这证明了SGID范式的有效性：在保留原始图像的语义一致性的同时带来良好的图像多样性。

(2) 在语义引导下，SGID的表现与扰动式或生成式的最佳基线相当或更好。具体来说，SGID在ResNet-50 (from scratch)上的平均表现以4.89%和1.72%优于RandAugment和SGID+DiverseCaption，在ViT（ImageNet-21k）上分别提升1.26%和0.33%，在CLIP-ViT（LAION-2B）上分别提升0.19%和0.14%。明显的性能提升显示了SGID在语义引导下平衡了多样性和语义一致性。

(3) SGID可以与基于扰动和生成的基线方法结合以进一步提高性能。我们分别探索基于SGID的RandAugment方法，和基于Text2Img的SGID方法。我们发现上述结合在三个backbone上均实现了进一步的提升。例如，作为“基于扰动 & 基于生成”的结合，“SGID & RA”既超过了RA，又超过了SGID。持续和显著的性能提升进一步证明了SGID不仅保留了原始图像的基本语义，同时带来良好的多样性，而且与基于扰动的方法互有助益。有趣的是，尽管Text2Img和SGID两种生成式增广方法的图像生成模型是相同的，但Text2Img & SGID的性能超过Text2Img，却低于SGID。我们将前者的性能提升归因于SGID引入的语义引导，而后者的性能下降则由于Text2Img可能错误地改变了原始图像的基本语义。

###  **图像多样性与语义一致性 **

在本节中，我们旨在从三个角度讨论SGID和其他基线方法的图像多样性和语义一致性：(1) 人类评估；(2) 自动相似性评估；(3) 案例研究。我们尝试分析SGID为何比现有扰动式和生成式的基线方法表现更好。

####  **人类评估 **

我们在一个粗粒度分类数据集（Caltech101）、一个细粒度分类数据集（OxfordPets）以及一个纹理分类数据集（DTD）上应用人类评估，如图4。对于每个数据集，我们随机选择10个标签及其对应的10张原始图像。我们将SGID与最佳的扰动式和生成式基线（RandAugment、Text2Img和SGID+DiverseCaption）进行比较。我们根据原始图像对这四种方法的增广图像进行评估。每张增广图像分别根据图像多样性和语义一致性在1~5的范围内进行评分。

![](img/0de9cf5047f7f16fd1a78b6641583f43.png)

图4:四种 DA 方法在三个数据集上在语义一致性（Con.）和多样性（Div.）角度的人类评估结果。

我们发现SGID在语义一致性方面与RandAugment相似，但图像多样性相对更高，而在语义一致性方面比Text2Img和SGID+DiverseCaption更高，但图像多样性相对较低。当考虑图像分类性能和人类评估结果时，我们的SGID通过语义引导的生成式图像增广实现了最佳性能，平衡了图像多样性和语义一致性。

####  **自动相似性评估 **

我们基于CIFAR-10进行自动相似性评估，并分别使用SGID和六种数据增广基线为每张原始图像生成五张增广图像。对于每种数据增广方法，我们计算原始图像及其五张增广图像间的平均余弦相似性。我们在所有原始图像上重复此过程，并计算每个标签的平均值作为多样性的衡量指标。增广图像与原始图像之间的平均相似性越低，对应的数据增广方法带来的语义一致性越低，但多样性越显著。结果如图5所示。

![](img/a87c058e71fb979d05fc66557d43b967.png)

图 5:CIFAR-10上每个类别的增强图像和原始图像之间的平均余弦相似度。

总体而言，与基于扰动式数据增广方法相比，SGID的相似度较低（0.8548），而与生成式基线相比，相似度较高。我们的SGID的平均相似性位于两类数据增广方法之间，但SGID在图像分类任务中表现最佳。这进一步证明了平衡图像多样性和语义一致性的重要性。

####  **案例研究 **

图6比较了SGID和其他六种基线方法生成的增广图像。三种基于扰动式基线方法通过变换带来多样性。然而，这些预定义的变换无法为增广图像提供足够的多样性。生成式基线方法比扰动式基线方法带来了更多样和生动的图像，但它们难以保持原始图像的语义一致性。相比之下，SGID保留了原始图像的语义一致性，并提供了良好的图像多样性，这也在下游任务中带来了性能提升。

![](img/d2698d7a8a9f475a79bea848138ef41c.png)

图6:7种增广方法的样例分析。

##  **结论 **

在本文中，我们介绍了SGID，一种语义引导的生成式图像增广方法。所提出的方法平衡了图像多样性和语义一致性。具体来说，SGID通过使用图像标签和caption作为语义引导来构建提示，生成既保留原始图像中基本语义又带来良好图像多样性的增广图像。我们通过在七个不同数据集上使用三个backbones与七个强大的图像增广基线进行实验来证明SGID的有效性。SGID在所有数据集上均优于backbone，并取得了最佳或与所有基线相当的性能。此外，SGID可以与其他图像增广基线结合，进一步提高整体性能。我们还通过定量的人类和自动评估以及定性的案例研究，评估了SGID在语义一致性和图像多样性方面的表现。

总体而言，SGID展示了在图像增广中平衡图像多样性与语义一致性的重要性。通过将图像标签和caption结合作为语义引导，SGID不仅在保持原始图像基本语义的同时增加了图像多样性，而且在不同的数据集和backbone上都显示出卓越的性能。这一方法的成功证明了在图像增广领域中，对语义的细致考量是提高模型性能的关键因素。SGID的灵活性也表现在它能与其他增广方法结合使用，进一步提高性能，这为未来的图像增广研究提供了新的视角和策略。

# **参考文献**

[1] Blip: Bootstrapping language-image pre-training for unified vision-language understanding and generation, arXiv:2201.12086.

[2] Learning transferable visual models from natural language supervision,Proc. of ICML, 2021.

[3] Style transfer from non-parallel text by cross-alignment, Advances in neural information processing systems, 2017 volume 30.

[4] The Curious Case of Neural Text Degeneration, arXiv:1904.09751.

[5] Deep residual learning for image recognition, CVPR 2016.

[6] An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale, ArXiv 2020

[7] Reproducible scaling laws for contrastive language-image learning, ArXiv: 2212.07143

本期责任编辑：车万翔

本期编辑：李宝航