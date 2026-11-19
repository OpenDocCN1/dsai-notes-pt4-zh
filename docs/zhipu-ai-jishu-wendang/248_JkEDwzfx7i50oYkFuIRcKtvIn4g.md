
# 测评 · Relay Diffusion：级联扩散模型，无需从白噪声开始

> 来源：[https://ai.feishu.cn/docx/Ft1edqSh3oqtmfxgvMcc2JlKnBf](https://ai.feishu.cn/docx/Ft1edqSh3oqtmfxgvMcc2JlKnBf)

![](img/96dd5d4c0cbf3efb06fead791e386a67.png)

我们提出一个新型的级联模型 Relay Diffusion Model，可以从任意给定分辨率的图像快速生成，而无需从白噪声生成。

论文链接：

`https://arxiv.org/abs/2309.03350`

GitHub地址：

`https://github.com/THUDM/RelayDiffusion`

近些年来，扩散模型（Diffusion）在图像合成方面取得了巨大的成功，显著提升了图片合成的质量。然而，扩散模型在合成高分辨率图片时仍面临较大挑战，

+ 一，低分辨率的噪声调度很难直接用于高分辨率，研究者们需要为高分辨的场景谨慎地调节噪声调度表，且仍难以获得良好的结果；

+ 二，高分辨的训练过程需要大量资源，计算成本较高。

目前，一种普遍采用的解决方案是 latent (stable) diffusion 提出的在隐空间内训练，再映射回像素空间，但这种方法不可避免地会受到底层伪影（low-level artifacts）的影响。

另一种方案则是训练一系列不同分辨率的超分扩散模型构成级联，现有的级联方法是有效的，但它需要在每个阶段从噪音开始完整采样，效率较低，且效果严重依赖于条件增强等训练技巧。

为了更好地解决上述问题，我们提出的级联模型 Relay Diffusion Model，在具备原有级联方法优点的同时，借助模糊扩散过程（blurring diffusion）和块状噪音（block noise），可以在任意不同分辨率间无缝衔接，就像“接力赛”一样，能够极大减少训练和采样的成本。

![](img/be56db057ec0ec37039678a02e8d94e0.png)

具体来说，我们通过离散余弦变换频谱分析发现，相同噪声强度在更高的分辨率下对应于频率空间的信噪比（SNR）在低频部分更高，这意味着自然图像的低频信息没有被很好地破坏掉。

为此，我们提出了一种像素点间具有相关性的块状噪音——block noise，它在高分辨率下对应的 SNR 在低频部分和高斯噪音在低分辨率下的 SNR 相当。

![](img/ae629493ddbd44bb62d4fe9adc1672eb.png)

以 64×64 和 256×256 为例，Relay Diffusion 的整体流程为：先通过标准扩散过程生成低分辨率图片，再将其上采样为每个 4×4 网格具有相同像素值的模糊高分辨率图片，之后对每个 4×4 的网格独立进行模糊扩散过程（blurring diffusion）。

这样使得前向过程的终态和上采样的模糊图片对齐，因此 Relay Diffusion 的第二阶段可以直接以模糊图片为起始点，而不是现有级联方法中的纯高斯噪音。

![](img/6853e5766eff1ece322169689a45eb6c.png)

实验结果显示，相比传统的级联扩散模型，Relay Diffusion 在生成高分辨率图片时，省去了生成低频信息的部分，极大地节约了计算成本，同时更加简单，不需要以低分辨率图片为条件和各种条件增强技巧，而且不需要重新设计或调节噪声调度表。

![](img/f1c52abb7e71969f77bfd2e9cfabd214.png)

另外，Relay Diffusion 在节省成本的同时，还可以更快地达到更好的生成性能，在无条件数据集 CelebA-HQ-256 上达到了 SoTA 的 FID，在条件数据集 ImageNet-256 上达到了 SoTA 的 sFID 以及具有竞争力的 FID，大幅超过了 ADM、LDM、DiT 等模型。当不使用无分类器指导（CFG）时，Relay Diffusion 也显示出强大的性能优势。

![](img/d94ea86c70766dab8d57c5079d8cfe11.png)

## 研究心路

Relay Diffusion（中继扩散）想法源自在一次实验中突然意识到不同分辨率图像和视频对噪音的敏感程度不同的原因在于其各像素分布完全不能看成独立的，而之前的扩散模型信噪比分析都没怎么考虑。最终在现有的pipeline之间权衡后，选择了从优化层次生成的角度入手，终于狠狠刷了一把纯图像生成模型的SOTA。
