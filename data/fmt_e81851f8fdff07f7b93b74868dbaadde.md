
This document is organized into several sections and chapters, as listed below and in the sidebar. You are encouraged to jump around to whichever parts seem most useful for your personal learning goals. Overall, I’d recommend first skimming many of the linked resources rather than reading (or watching) word-for-word. This should hopefully at least give you a sense of where your knowledge gaps are in terms of dependencies for any particular learning goals, which will help guide a more focused second pass.

本文档分为几个部分和章节，如下和侧栏中所列。我们鼓励您跳到对您的个人学习目标最有用的部分。总的来说，我建议首先浏览许多链接的资源，而不是逐字阅读（或观看）。希望这至少能让您了解在任何特定学习目标的依赖性方面您的知识差距在哪里，这将有助于指导更有针对性的第二遍。

# Section I: Foundations of Sequential Prediction 第一节：序列预测的基础

> **Goal:** Recap machine learning basics + survey (non-DL) methods for tasks under the umbrella of “sequential prediction”.
>
> 目标：回顾“序列预测”下任务的机器学习基础知识 + 调查（非深度学习）方法。
>
> Our focus in this section will be on quickly overviewing classical topics in statistical prediction and reinforcement learning, which we’ll make direct reference to in later sections, as well as highlighting some topics that I think are very useful as conceptual models for understanding LLMs, yet which are often omitted from deep learning crash courses – notably time-series analysis, regret minimization, and Markov models.
>
> 本节的重点将是快速概述统计预测和强化学习中的经典主题，我们将在后面的部分中直接参考这些主题，并强调一些我认为作为理解概念模型非常有用的主题，但深度学习速成课程中经常忽略这些内容，特别是时间序列分析、遗憾最小化和马尔可夫模型。

## Chapter 1: Preliminaries 第一章：预备知识

### Math 数学

Calculus and linear algebra are pretty much unavoidable if you want to understand modern deep learning, which is largely driven by matrix multiplication and backpropagation of gradients. Many technical people end their formal math educations around multivariable calculus or introductory linear algebra, and it seems common to be left with a sour taste in your mouth from having to memorize a suite of unintuitive identities or manually invert matrices, which can be discouraging towards the prospect of going deeper in one’s math education. Fortunately, we don’t need to do these calculations ourselves — programming libraries will handle them for us — and it’ll instead be more important to have a working knowledge of concepts such as:

如果你想了解现代深度学习，微积分和线性代数几乎是不可避免的，现代深度学习很大程度上是由矩阵乘法和梯度反向传播驱动的。许多技术人员围绕多元微积分或入门线性代数结束了他们的正式数学教育，并且由于必须记住一套不直观的恒等式或手动求逆矩阵而感到口酸，这似乎很常见，这可能会阻碍你的学习。进一步深入数学教育的前景。幸运的是，我们不需要自己进行这些计算——编程库将为我们处理它们——相反，掌握以下概念的实用知识会更重要：

+   Gradients and their relation to local minima/maxima

    梯度及其与局部最小值/最大值的关系

+   The chain rule for differentiation

    微分的链式法则

+   Matrices as linear transformations for vectors

    矩阵作为向量的线性变换

+   Notions of basis/rank/span/independence/etc.

    基础/等级/跨度/独立性/等等的概念。

[good visualizations](https://www.youtube.com/@3blue1brown/playlists) can really help these ideas sink in, and I don’t think there’s a better source for this than these two YouTube series from 3Blue1Brown:

良好的可视化确实可以帮助这些想法深入人心，我认为没有比 3Blue1Brown 的这两个 YouTube 系列更好的资源了：

+   Essence of calculus  微积分的本质

+   [Essence of linear algebra](https://www.youtube.com/watch?v=fNk_zzaMoSs&list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab&pp=iAQB) 线性代数本质

If [your math is rusty](https://www.youtube.com/watch?v=aircAruvnKk&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi), I’d certainly encourage (re)watching these before diving in deeper. To test your understanding, or as a preview of where we’re headed, the shorter Neural networks video series on the same channel is excellent as well, and the latest couple videos in the series give a great overview of Transformer networks for language modeling.

如果你的数学很生疏，我当然鼓励你在深入研究之前（重新）观看这些内容。为了测试您的理解程度，或者作为我们前进方向的预览，同一频道上的较短的神经网络视频系列也非常好，该系列中的最新几个视频对用于语言建模的 Transformer 网络进行了很好的概述。

These [lecture notes](https://links.uwaterloo.ca/math227docs/set4.pdf) from Waterloo give some useful coverage of multivariable calculus as it relates to optimization, and “Linear Algebra Done Right” by Sheldon Axler is a nice reference text for linear algebra. “Convex Optimization” by Boyd and Vandenberghe shows how these topics lay the foundations for the kinds of optimization problems faced in machine learning, but note that it does get fairly technical, and may not be essential if you’re mostly interested in applications.

滑铁卢的这些讲义对与优化相关的多变量微积分进行了一些有用的介绍，谢尔顿·阿克斯勒 (Sheldon Axler) 的《线性代数正确》是线性代数的一本很好的参考书。Boyd 和 Vandenberghe 的“凸优化”展示了这些主题如何为机器学习中面临的各种优化问题奠定基础，但请注意，它确实具有相当的技术性，如果您最感兴趣的是应用程序，则可能不是必需的。

Linear programming is certainly worth understanding, and is basically the simplest kind of [high-dimensional optimization problem](https://www.youtube.com/watch?v=E72DWgKP_1Y) you’ll encounter [(but still quite practical)](https://genai-handbook.github.io/#generative-adversarial-nets); this illustrated video should give you most of the core ideas, and Ryan O’Donnell’s videos (17a-19c in this series, depending on how deep you want to go) are excellent if you want to go deeper into the math. These lectures (#10, #11) from Tim Roughgarden also show some fascinating connections between linear programming and the “online learning” methods we’ll look at later, which will form the conceptual basis for GANs (among many other things).

线性规划当然值得理解，并且基本上是您将遇到的最简单的高维优化问题（但仍然非常实用）；这个插图视频应该为您提供大部分核心思想，如果您想更深入地了解数学，Ryan O’Donnell 的视频（本系列中的 17a-19c，取决于您想要深入的程度）非常好。Tim Roughgarden 的这些讲座（#10、#11）还展示了线性编程和我们稍后将讨论的“在线学习”方法之间的一些有趣的联系，这将构成 GAN（以及许多其他事物）的概念基础。

### Programming 编程

Most machine learning code is written in Python nowadays, and some of the references here will include Python examples for illustrating the discussed topics. If you’re unfamiliar with Python, or programming in general, I’ve heard good things about Replit’s 100 Days of Python course for getting started. Some systems-level topics will also touch on implementations in C++ or CUDA — I’m admittedly not much of an expert in either of these, and will focus more on higher-level abstractions which can be accessed through Python libraries, but I’ll include potentially useful references for these languages in the relevant sections nonetheless.

如今，大多数机器学习代码都是用 Python 编写的，这里的一些参考文献将包括用于说明所讨论主题的 Python 示例。如果您不熟悉 Python 或一般编程，我听说过 Replit 的 100 天 Python 入门课程。一些系统级主题还将涉及 C++ 或 CUDA 中的实现 - 诚然，我在这方面都不是专家，并且将更多地关注可以通过 Python 库访问的更高级别的抽象，但我会尽管如此，在相关部分中仍包含这些语言的潜在有用参考。

## Chapter 2: Statistical Prediction and Supervised Learning 第 2 章：统计预测和监督学习

Before getting to deep learning and large language models, it’ll be useful to have a solid grasp on some foundational concepts in probability theory and machine learning. In particular, it helps to understand:

在进入深度学习和大型语言模型之前，扎实掌握概率论和机器学习中的一些基本概念将很有用。特别是，它有助于理解：

+   Random variables, expectations, and variance

    随机变量、期望和方差

+   Supervised vs. unsupervised learning

    监督学习与无监督学习

+   Regression vs. classification

    回归与分类

+   Linear models and regularization

    线性模型和正则化

+   Empirical risk minimization

    经验风险最小化

+   Hypothesis classes and bias-variance tradeoffs

    假设类和偏差-方差权衡
