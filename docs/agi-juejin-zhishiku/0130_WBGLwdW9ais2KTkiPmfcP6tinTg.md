# ECCV 2024 | 机器遗忘之后，扩散模型真正安全了吗？

> 来源：[https://agijuejin.feishu.cn/docx/NHmAdjMPIoLppRxG7EKc0nGWnNI](https://agijuejin.feishu.cn/docx/NHmAdjMPIoLppRxG7EKc0nGWnNI)

> 机器之心｜[阅读原文](https://www.jiqizhixin.com/articles/2024-08-26)

> 转载请联系原作者取得授权

**本文第一作者为密歇根州立大学计算机系博士生张益萌，贾景晗，两人均为OPTML实验室成员，指导教师为刘思佳助理教授。OPtimization and Trustwor**thy Machine Learning (OPTML) 实验室的研究兴趣涵盖机器学习/深度学习、优化、计算机视觉、安全、信号处理和数据科学领域，重点是开发学习算法和理论，以及鲁棒且可解释的人工智能。

在人工智能领域，图像生成技术一直是一个备受关注的话题。近年来，扩散模型（Diffusion Model）在生成逼真且复杂的图像方面取得了令人瞩目的进展。然而，技术的发展也引发了潜在的安全隐患，比如生成有害内容和侵犯数据版权。这不仅可能对用户造成困扰，还可能涉及法律和伦理问题。

尽管目前已有不少机器遗忘（Machine Unlearning, MU）方法 [1-3]，希望让扩散模型在使用不适当的文本提示时避免生成不合时宜的图片，但其有效性存疑。

只是我们好奇，经过机器遗忘的扩散模型，真的就一定安全了吗？

![](img/d3e0867bfcc7602cf6258631811397a8.png)

为了应对这一挑战，密歇根州立大学 (Michigan State University) 和英特尔（Intel）的研究者们提出了一种高效且无需辅助模型的对抗性文本提示生成方法 UnlearnDiffAtk [4]，并用优化后得到的对抗性文本提示作为检验遗忘后扩散模型安全可靠性的工具，论文目前已被 ECCV 2024 接收。本文第一作者为密歇根州立大学计算机系博士生张益萌、贾景晗，两人均为 OPTML 实验室成员，指导教师为刘思佳助理教授。

![](img/c71fb33e78058813b4daa707bb339d37.png)

+   论文题目：To Generate or Not? Safety-Driven Unlearned Diffusion Models Are Still Easy to Generate Unsafe Images ... For Now

+   论文地址：https://arxiv.org/abs/2310.11868

+   代码地址：https://github.com/OPTML-Group/Diffusion-MU-Attack

+   Unlearned Diffusion Model Benchmark: https://huggingface.co/spaces/Intel/UnlearnDiffAtk-Benchmark

Unlearned DM 可以从两个角度评估模型：

+   **安全可靠性 **：通过对抗性文本提示攻击 (UnlearnDiffAtk) 来进行评估；

+   **图片生成能力 **：通过一万张生成图片平均 FID（Fréchet inception distance）和 CLIP score 进行评估。

![](img/ad1394b62e5e553a46753ed3aaf73edd.png)

**文章与代码均已开源，研究团队还在积极收纳更多的方法到 Unle**arned DM Benchmark。 如有意向，欢迎邮件联系作者（zhan1853@msu.edu）沟通模型测评相关事宜。

UnlearnDiffAtk 方法有什么独特之处？

UnlearnDiffAtk 的目标是通过寻找离散的对抗性文本来进行攻击，而与之不同的是，CCE [5] 侧重于寻找连续的文本嵌入进行攻击。

**然而，CCE 并不是一个理想的评估方式，因为文本反转 [6] 的初衷是通过优**化生成 “新” 的词元（token），从而使扩散模型能够生成未见过的事物或风格。

因此，即使扩散模型已经遗忘了某些特定内容，仍然可以通过优化生成新的词元来使模型生成相应的事物。而 UnlearnDiffAtk 与其他对抗式文本生成方法不同，UnlearnDiffAtk 无需依靠辅助模型或未经机器遗忘的原模型提供优化指导。它利用扩散模型内在的分类器辨别能力 [7]，来指导对抗性文本的生成，使得攻击更具可操作性。

![](img/e2fd41ec3a3dfa2cd3ab767dedac3eaf.png)

具体来说，根据 Diffusion Classifier [7] 的概念，预测输入图片 x 为标签 c 的概率变为如下：

![](img/70b60b8de6629235d46fe957845811a4.png)

![](img/f6083690974ed96cf859508539b409ae.png)

![](img/a80c8ca3abed34a5eed4202b478f7de0.png)

通过扩散分类器 (3) 的视角，创建对抗性提示词 c’ 以规避目标遗忘后扩散模型的任务可以表述为：

![](img/528933ccf6bedfba11a3bb3e479bcfb9.png)

然而分类只需要噪声误差之间的相对差异，不需要它们的绝对大小，所以公式（3）可以变形为

![](img/c78cc4b5aab326bdec57c9df0cd12e34.png)

然后我们可以将攻击生成问题 (4) 变为

![](img/22f25bc8917f6f5cd5608396206e894f.png)

为了便于优化，我们通过利用 exp (⋅) 的凸性来简化公式 (6)。使用 Jensen 不等式，对于凸函数，公式 (6) 中的单个目标函数（针对特定的 j）的上界为：

![](img/9bd2f80154902eaea9991dfcee7169d7.png)

由于第二项与优化变量 c’无关，通过将公式 (7) 纳入公式 (6) 并排除与 c’无关的项，我们得到以下简化的攻击生成优化问题：

![](img/bb1a92c71882063f2ddbdb32519cb462.png)

**任务类型**

扩散模型的机器遗忘任务可分为三大类，而 UnlearnDiffAtk 在这三类任务中均展现了较强的攻击成功率：

+   有害内容 （如：裸体，暴力，违法行为）

+   艺术风格

+   物体

![](img/7d77d81df2d9e49e444c1d6cd955d009.png)

本文不仅深入了解了扩散模型在生成安全性方面的挑战，还提出了有效的解决方案。希望这项研究能引起更多对图像生成技术安全性的关注，并推动相关技术的进一步发展。

**实验结果与可视化**

下述表格和可视化结果分别展示了在遗忘有害内容、遗忘艺术风格以及遗忘物体这三类任务中的表现。通过这些结果可以看出，即使在没有额外辅助模型提供优化指导的情况下，仅仅依靠扩散模型自身携带的分类器特性，UnlearnDiffAtk 依然表现出与同期工作 P4D 相当甚至更高的攻击成功率。此外，由于无需依赖额外的模型辅助，UnlearnDiffAtk 能够显著提高攻击速度，平均节省约 30% 的攻击时间。

![](img/089ddd08dc470201ab663f60fdb59eba.png)

![](img/ccaad7cee9de2e9487278900605cb538.png)

![](img/377711b1dc995d661c1e365b84960fb8.png)

![](img/f446eaaf506dacafe45c1a1aa485f3e3.png)

![](img/469fdbaa1375b3902fadb56974521fbb.png)

![](img/facc26cc04dcb267cf288d14fc6941a5.png)

![](img/9a756b61a7ec88c80512f66de9bd1f55.png)

![](img/496cb27cf64810784ffd98ccec4de0ee.png)

参考文献

[1] Zhang Y, Chen X, Jia J, et al. Defensive Unlearning with Adversarial Training for Robust Concept Erasure in Diffusion Models, Arxiv 2024.

[2] Zhang Y, Zhang Y, Yao Y, et al. Unlearncanvas: A stylized image dataset to benchmark machine unlearning for diffusion models, Arxiv 2024.

[3] Fan C, Liu J, Zhang Y, et al. Salun: Empowering machine unlearning via gradient-based weight saliency in both image classification and generation, ICLR 2024.

[4] Zhang Y, Jia J, Chen X, et al. To generate or not? safety-driven unlearned diffusion models are still easy to generate unsafe images... for now, ECCV 2024.

[5] Pham M, Marshall K O, Cohen N, et al. Circumventing concept erasure methods for text-to-image generative models, ICLR 2023.

[6] Gal R, Alaluf Y, Atzmon Y, et al. An image is worth one word: Personalizing text-to-image generation using textual inversion, ICLR 2023.

[7] Li AC, Prabhudesai M, Duggal S, et al. Your diffusion model is secretly a zero-shot classifier, ICCV 2023.