
# DeepMind“反向”搭建Transformer方法火了：由可解释程序设计AI，项目已开源

> 来源：[https://agijuejin.feishu.cn/docx/IdmSdjqh5oOjPcxuxd9cQeftn0d](https://agijuejin.feishu.cn/docx/IdmSdjqh5oOjPcxuxd9cQeftn0d)

> 萧箫 发自 凹非寺

> 量子位 | 👸号 QbitAI

先搭个模型，效果好就试着解释它，是我们常见的AI设计套路。

但，有人已经受够了这样“莫名其妙”的流程——

**既然人类总是**试图解释AI模型，那为什么不能反过来，直接用看得懂的程序来设计AI模型权重呢？

这样做出来的模型，既提升了它的**可解释性**，又降低了设计架构的复杂度。

![](img/96f259c1c840591123915d29d211982d.png)

这是DeepMind针对Transformer模型设计的最新工具，被po到网上后爆火：

![](img/c166133a8ace5e406578b685dc11b27b.png)

有网友惊讶表示：

> 这真有意思。它或许是第一个能将代码（软件1.0）变模型（软件2.0）的系统。

![](img/25cf1701b45ed1cbd41b40cd67f140d5.png)

所以它究竟是如何实现的？

## 将可读代码转变成AI权重的“编译器”

**这个将可读**性较高的代码“翻译”成Transformer模型的工具，名叫Tracr。

具体来说，它有点像是一个“**编译器**”，懂得将一种名叫RASP（Restricted Access Sequence Processing Language）的编程语言转换成Transformer模型权重。

RASP，是此前在ICLR 2021一篇论文中提出的“编程语言”，专门根据Transformer架构（编解码器-注意力机制等）设计。

Tracr则在此基础上更进一步，不仅简单优化了原有的RASP语言，而且能将基于RASP编写的程序很好地转换为Transformer模型的权重。

具体来说，Tracr会先将“编程语言”RASP转换成“汇编语言”craft，后者表示向量空间以及其间的对应操作，再进一步地，将craft转变成“机器码”模型权重。

![](img/31d66dfdadaee11f015e2042a3e41c69.png)

具体实现过程如下，一共分为5步：

![](img/7c8f31360e84f87218c3828efee97a7a.png)

基于它，作者们创建了一系列人工编写程序的基准Transformer，包括计算token频率、排序以及Dyck-n括号识别等。

![](img/5ff29d752355bf030b037fa73f560123.png)

据一作介绍，Tracr最大的亮点在于，可以“从0开始搭建你自己的Transformer模型”。

## 还提出了配套压缩方法

不过，基于Tracr打造的模型可能**效率**很低，因此作者们又提出了一种配套的“压缩”方法。

基于案例来看，这种方法不仅能让Tracr模型更高效，而且允许将`D`维特征压缩到小于`D`维的特征空间中。

例如这是基于RASP语言编写的程序示例：

![](img/fe50324b8739a37bf7b37c511e9d60a6.png)

在对模型进行压缩后，同样能很好地完成原来的任务，且效果相差不大：

![](img/2e91f541b235b9940d061e636652e058.png)

同时作者们还观察到，在对模型进行压缩后，它甚至保留了更重要的特征，学会自动筛掉一些不重要的特征：

![](img/08c9fc6128305ec6c98c28b95ff1ba09.png)

与此同时也能保证一个不错的精度：

![](img/cb496ec76ea9d4a2b2f741afa6187bea.png)

目前对应的Tracr模型已经开源，感兴趣的小伙伴们可以去查看一波~

## 作者介绍

论文作者们分别来自DeepMind和苏黎世联邦理工学院。

![](img/6fa73ee1defc508cd303ee7d935e7d50.png)

一作David Lindner，苏黎世联邦理工学院在读博士生，这份工作是他在DeepMind实习期间完成，目前感兴趣的研究方向是AI鲁棒性、强化学习等。

![](img/764c5e25c98da5798a0536b8edbc19bf.png)

你想好用它设计一个怎样的Transformer模型了吗？

论文地址：

arxiv.org/abs/2301.05…

项目地址：

github.com/deepmind/tr…
