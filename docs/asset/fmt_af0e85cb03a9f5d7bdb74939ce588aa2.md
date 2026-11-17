
# 学界 | 2010-2016 年被引用次数最多的深度学习论文（附论文下载）

选自 Github

**作者：Terry T. Um**

**机器之心编译**

**参与：阿儁是个 nerd，亚洲，黄清纬，微胖**

我相信世上存在值得阅读的经典的深度学习论文，不论它们的应用领域是什么。比起推荐大家长长一大串论文，我更倾向于推荐大家一个某些深度学习领域的必读论文精选合集。

## 精选合集标准

- 2016 : +30 引用 「+50」
- 2015 : +100 引用 「 +200」
- 2014 : +200 引用 「+400」
- 2013 : +300 引用 「 +600」
- 2012 : +400 引用 「 +800」
- 2011 : +500 引用 「 +1000」
- 2010 : +600 引用 「 +1200」

## 目录

- 调查 / 回顾
- 理论 / 未来
- 优化 / 正则化
- 网络模型
- 图像
- 图片说明
- 视频 / 人类行为
- 词嵌入
- 机器翻译 / 问答
- 语音 / 及其他
- 强化学习 / 机器人
- 无监督
- 硬件 / 软件
- 推荐论文

总共有 85 篇论文，不包括「硬件 / 软件」论文以及「推荐论文」。

## 一、调查 / 回顾

### 1. 论文：深度学习

Deep learning

作者：Ian.Goodfellow et al.

### 2. 论文：深度学习

Deep learning (2015)

作者：Y. LeCun, Y. Bengio 以及 G. Hinton

> 注：机器之心已经翻译过本文：[Nature 重磅：Hinton、LeCun、Bengio 三巨头权威科普深度学习](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=208649889&idx=1&sn=3977c811c2fcee8d5f15fc7559ed2e2f&scene=21#wechat_redirect)

### 3. 神经网络中的深度学习：概述

Deep learning in neural networks: An overview

作者：J. Schmidhuber

### 4. 表征学习：回顾及新观点

Representation learning: A review and new perspectives

作者： Y. Bengio et al.

![](img/f8cc59619e6d5dc9c33c432417c0d363.jpg)

摘要：通常来说，机器学习算法的成功依赖于数据的表征，我们假设这是因为不同的表征方式会卷入并多少掩盖数据背后变化的不同解释因素。虽然专业领域的知识可以帮助设计表征方式，但是，通用的先验知识也可以，并且对 AI 的追求正在激发更强大的实现了这些先验知识的表征学习算法的设计。这篇论文回顾了无监督特征学习和深度学习领域最近成果，包括概率模型、自动编码器、流形学习和深度网络的进展。这也激发了人们去解决长期悬而未决的问题，也就是学习好的表征、计算表征（比如推理）的合适目标这个问题，也促进了表征学习，密度估计和流型学习之间的几何连接。索引词——深度学习，表征学习，特征学习，无监督学习，玻耳兹曼机「Boltzmann Machine」，自动编码器，神经网路。

## 二、理论 / 未来

### 5. 在神经网络中提取知识

Distilling the knowledge in a neural network

作者：G. Hinton et al.

![](img/6373e1f30d5f37ab5ef0ff79ba818c30.jpg)

摘要：一个非常简单的能改善几乎任何机器学习算法表现的方法，就是训练许多基于相同数据集的模型，并取这些模型的预测平均值。不幸的，使用全部模型来进行预测是笨拙的，而且允许大量用户部署的计算成本太昂贵，特别是当个体模型是大的神经网络的时候。Caruana 和他的合作者已经表明，有可能将一个集合中的知识压缩到一个单独模型中，部署起来也容易得多，而且我们使用了不同压缩技巧进一步扩展了这一方法。在 MNIST 上，我们取得了一些令人吃惊的成功，而且我们展示了可以显著改善一个重度使用商业系统的声学模型，方法就是将集合中的知识概括进一个单独模型。我们也介绍了一个新型集合，由一个或更多的全模型以及许多学会区分识别细粒度类别（全模型做不到）的专家模型组成。可以对这些专家模型进行快速、并行训练。

### 6. 深度神经网络很易受骗：高信度预测无法识别的图片

Deep neural networks are easily fooled: High confidence predictions for unrecognizable images

作者：A. Nguyen et al.

### 7. 深度神经网络特征的可迁移性如何？

How transferable are features in deep neural networks? (2014),

作者：J. Yosinski et al.

### 8. 细节魔鬼的回归：深挖卷积网络

Return of the Devil in the Details: Delving Deep into Convolutional Nets (2014)

作者：K. Chatfield et al.

### 9. 为什么无监督预训练对深度学习有帮助？

Why does unsupervised pre-training help deep learning (2010)

作者：D. Erhan et al. (Bengio)

### 10. 理解训练深度前馈神经网络的难点

Understanding the difficulty of training deep feedforward neural networks (2010)

作者：X. Glorot and Y. Bengio

## 三、优化 / 正则化

### 11. Batch Normalization 算法：通过减少内部协变量转化加速深度网络的训练

Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift (2015)

作者：S. Loffe and C. Szegedy (Google)

### 12. 深度探入纠正器：在 Imagenet 分类中超过人类表现

Delving deep into rectifiers: Surpassing human-level performance on imagenet classification (2015)

作者：K. He et al.(Microsoft)

### 13. Dropout：一个预防神经网络过拟合的简单方式

Dropout: A simple way to prevent neural networks from overfitting (2014)

作者：N. Srivastava et al. (Hinton)

![](img/a8196ced263dc3736c9f07d3a075f306.jpg)

摘要：带有大量参数的深度神经网络是非常有力的机器学习系统。但是，这类网络中存在一个严重的问题，过度拟合。大型网络采用缓慢，因此，很难通过测试时集合许多不同大型神经网络的预测来解决过度拟合问题。Dropout 是一个解决办法。其中关键思想是，在训练过程中，从神经网络中随机放弃单元（以及它们的连接）。这能防止单元过于适应。在训练过程中，放弃来自指数数量的不同“变薄的”神经网络的样本。测试时，简单使用一个单独的带有较小权重的变薄网络，就很容易近似平均所有这些变瘦网络预测的效果。这显著减轻了过度拟合，也改进了其他正则化方法。我们也表明，这一方法改善了神经网络在监督学习任务（视觉、语音识别、文档分类以及计算生物学）方面的表现，在许多基准数据组上的成绩目前达到最先进水平。

### 14. Adam：一个随机优化的方法

Adam: A method for stochastic optimization (2014)

作者：D. Kingma and J. Ba

### 15. 视觉识别中深度卷积网络的空间金字塔池化

Spatial pyramid pooling in deep convolutional networks for visual recognition (2014)

作者：K. He et al.

### 16. 论深度学习领域初始化和动量的重要性

On the importance of initialization and momentum in deep learning (2013)

作者：I. Sutskever et al. (Hinton)

### 17. 使用 Dropconnect 的神经网络正则化

Regularization of neural networks using dropconnect (2013)

作者：L. Wan et al. (LeCun)

### 18. 通过预防特征检测器的互相适应改善神经网络

Improving neural networks by preventing co-adaptation of feature detectors (2012)

作者：G. Hinton et al.

### 19. 超参数最优化的随机搜索

Random search for hyper-parameter optimization (2012)

作者：J. Bergstra and Y. Bengio

### 20. 图像识别中的深度残差学习

Deep residual learning for image recognition (2016)

作者：K. He et al. (Microsoft)

### 21. 用于物体精准检测和分割的基于区域的卷积网络

Region-based convolutional networks for accurate object detection and segmentation (2016)

作者：R. Girshick et al.(Microsoft)

### 22. 深入卷积网络

Going deeper with convolutions (2015)

作者：C. Szegedy et al. (Google)

### 23. 快速 R-CNN 网络

Fast R-CNN (2015)

作者： R. Girshick (Microsoft)

### 24. 用于语义分割的饱和卷积网络

Fully convolutional networks for semantic segmentation (2015)

作者：J. Long et al.

### 25. 大规模图像识别的深度卷积网络

Very deep convolutional networks for large-scale image recognition (2014)

作者：K. Simonyan and A. Zisserman

### 26. OverFeat：使用卷积网络融合识别、本地化和检测

OverFeat: Integrated recognition, localization and detection using convolutional networks (2014)

作者：P. Sermanet et al.(LeCun)

### 27. 可视化以及理解卷积网络

Visualizing and understanding convolutional networks (2014)

作者：M. Zeiler and R. Fergus

### 28. Maxout 网络

Maxout networks (2013)

作者：I. Goodfellow et al. (Bengio)

### 29. Network In Network 深度网络架构

Network in network (2013)

作者：M. Lin et al.

### 30. 使用深度卷积神经网络做 ImageNet 分类

ImageNet classification with deep convolutional neural networks (2012)

作者：A. Krizhevsky et al. (Hinton)

### 31. 大规模分布式深度网络

Large scale distributed deep networks (2012)

作者：J. Dean et al.

![](img/9162a821e5595f0806ce4c5780a4b071.jpg)

摘要：近期在无监督特征学习和深度学习上的研究表明，在大规模模型上的训练能够急剧改善性能。在此论文中，我们考虑了使用成千上万个 CPU 核训练一个带有数十亿参数的深度网络的问题。我们开发了一个名为 DistBelief 的软件框架，能够利用数千台机器的计算集群训练大规模模型。在这一框架内，我们开发了两种大规模分布训练的算法：（i）Downpour SGD，这是一个异步随机梯度下降过程，支持众多模型副本；（ii）Sandblaster，这是一个支持多样分布批处理优化程序的框架，包括一个 L-BFGS 分布式实施方案。Downpour SGC 和 Sandblaster L-BFGS 都增加了深度网络训练的规模和速度。我们已经成功的使用这一系统训练了一个比先前报告中大 30 倍的深度网络，并且在 ImageNet 挑战赛上取得了最顶尖的表现，这是一个拥有 1600 万图像和 21k 分类的视觉物体识别任务。还显示出，在商业化语音识别服务中使用同一技术能够急加速一个较小的深度网络的训练。尽管我们专注于这些方法应用于大型神经网络并报道性能，在下面的算法可应用于任何基于梯度的机器学习算法。

### 32. 深而稀疏的修正神经网络

Deep sparse rectifier neural networks (2011)

作者：X. Glorot et al. (Bengio)

### 33. 使用卷积神经网络在自然环境下阅读文本

Reading text in the wild with convolutional neural networks (2016)

作者：M. Jaderberg et al. (DeepMind)

## 四、图像

### 34. 使用卷积神经网络在自然环境下阅读文本

Reading text in the wild with convolutional neural networks (2016)

作者：M. Jaderberg et al. (DeepMind)

![](img/96a5d8e46df3dac777d60e28aa16917a.jpg)

摘要：在此论文中，我们提供了一个在自然场景图像和基于图像检索文本的发现——本地化和文本识别的端到端系统。这个系统基于一个用于检测的区域提议机制和用于识别的深度卷积神经网络。我们的途径使用了一个建议生成技术间的互补性结合，用于保证高查全率，还有一个用于改善精确率的快速后续过滤阶段。为了做到识别和建议排序，我们训练了非常大的卷积神经网络同时在全部提议区域完成单词识别，这不同于过去基于系统的字符分类器。这些网络仅仅在经由一个合成的文本生成引擎产生的数据上进行训练，不需要人类标记的数据。对我们途径阶段的分析显示，自始至终的性能表现都是最顶尖的。我们在一系列标准的端到端文本识别基准和基于文本的图像检索数据集上进行了严格的实验，结果显示要比之前的所有方法都有大幅度的改善。最终，我们展示了一个这一文本识别系统在现实中的应用，能够通过一个文本查询立即搜索到数千小时的新闻片段。

### 35. Imagenet 大规模视觉识别挑战赛

Imagenet large scale visual recognition challenge (2015)

作者：O. Russakovsky et al.

### 36. 更快速的 R-CNN 网络：使用区域建议网络的实时物体检测

Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks (2015)

作者： S. Ren et al.

### 37. DRAW：一个用于图像生成的循环神经网络

DRAW: A recurrent neural network for image generation (2015)

作者：K. Gregor et al.

### 38. 对精确的物体检测和语义切割更为丰富的特征分层

Rich feature hierarchies for accurate object detection and semantic segmentation (2014)

作者： R. Girshick et al.

### 39. 使用卷积神经网络学习和迁移中层图像表征

Learning and transferring mid-Level image representations using convolutional neural networks (2014)

作者：M. Oquab et al.

### 40. DeepFace：在面部验证任务中接近人类表现

DeepFace: Closing the Gap to Human-Level Performance in Face Verification (2014)

作者：Y. Taigman et al. (Facebook)

### 41. Decaf：一个用于通用视觉识别的深度卷积激活特征
