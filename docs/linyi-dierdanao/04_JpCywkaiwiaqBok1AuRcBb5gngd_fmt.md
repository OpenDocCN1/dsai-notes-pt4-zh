
# 大佬 Ilya Sutskever 的神级论文清单

> 来源：[https://docs.feishu.cn/docx/EtpRdiri4o0zAPxfGYVcRwhVnzd](https://docs.feishu.cn/docx/EtpRdiri4o0zAPxfGYVcRwhVnzd)

Ilya Sutskever，作为深度学习领域的领航者，不仅在学术界树立了里程碑，更通过在谷歌大脑、OpenAI及DeepMind的领导角色，引领着人工智能技术向更加智能、安全的方向迈进。

Sutskever 相信，如果 Carmack 能够深入理解并吸收这些论文的精髓，他将掌握当今科技界（人工智能领域）最为关键的90%的知识。

从研究主题上看，Ilya Sutskever 重点关注 Transformer 架构、循环神经网络（RNN）、长短期记忆网络（LSTM）、神经网络的复杂度、计算机视觉等。

![](img/b0bb864940d1e5c25e4d3c24527a5804.png)

推荐清单地址：https://arc.net/folder/D0472A20-9C20-4D3F-B145-D2865C0A9FEE

我们一睹为快。

## 01 Transformer 架构

在 Transformer 架构方面，Ilya 推荐谷歌在 2017 年发表的经典论文《Attention Is All You Need》，这是 Transformer 架构的问世之作。Transformer 架构今天已经成为人工智能领域的主流基础架构，特别是它是生成式 AI 模型的核心架构。

![](img/6b920216f01522e910aedad2c17eb08f.png)

论文地址：https://arxiv.org/abs/1706.03762

Ilya 还推荐了一篇由康奈尔大学副教授 Alexander Rush 等研究者在 2018 年撰写的博客文章 ——《The Annotated Transformer。

作为 Transformer 新手入门教程，这个教程从最基础的理论开始，手把手教你按照最简单的python代码实现Transformer，一经推出就广受好评。2022年，这个入门教程有了新的版本。

![](img/1b1c8fcfc02a33daff15bf32deca8bda.png)

博客地址：http://nlp.seas.harvard.edu/annotated-transformer/

GitHub地址：https://github.com/harvardnlp/annotated-transformer/

论文地址：https://nlp.seas.harvard.edu/annotated-transformer/

![](img/3701cdd8eadcd72ad5ba60bfbdfba9f6.png)

图注：Transformer模型架构

## 02 循环神经网络（RNN）

在 RNN 方面，Ilya 首先推荐阅读 AI 大牛 Andrej Karpathy于 2015 年撰写的一篇博客，强调「RNN 惊人的有效性」。

文章主要探讨了循环神经网络（Recurrent Neural Networks, RNNs）这一深度学习模型在处理序列数据任务上的强大能力与广泛应用潜力。

![](img/8aabb82d01b4e543ebf3ccefbfe04226.png)

论文地址：https://karpathy.github.io/2015/05/21/rnn-effectiveness/

Ilya 还推荐了由纽约大学 Wojciech Zaremba（OpenAI创始团队成员）和 Ilya Sutskever 本人 2015 年发表的论文《Recurrent Neural Network Regularization。当时，Ilya 还是谷歌大脑的研究科学家。

这篇论文为 RNN 提出了一种简单的正则化技术，阐述了如何正确地将 dropout 应用于 LSTM，大大减少了各种任务的过拟合，包括语言建模、语音识别、图像字幕生成、机器翻译等等。

![](img/ecfb9caaf86de5f429ec7e4402c6c5aa.png)

论文地址：https://arxiv.org/pdf/1409.2329

此外，Ilya 还推荐了 DeepMind、伦敦大学学院COMPLEX 2018 年联合发表的论文《Relational recurrent neural networks。

论文提出一种关系循环神经网络，该网络利用一种新型记忆模块 RMC 解决标准记忆架构难以执行关系推理任务的问题。该方法在强化学习领域（如 Mini PacMan）、程序评估和语言建模上获得了很大进步，在 WikiText-103、Project Gutenberg和 GigaWord 数据集上获得了当前最优的结果。

论文链接：https://arxiv.org/abs/1806.01822

另一篇，文章《ORDER MATTERS: SEQUENCE TO SEQUENCE FOR SETS由Oriol Vinyals、Samy Bengio和Manjunath Kudlur（来自Google Brain团队）共同撰写，并在ICLR 2016会议上发表。该文探讨了在监督学习中，序列因其能够利用循环神经网络（RNN）高效处理而成为重要对象。

![](img/b651ab46b1d7d8ae2fc0b0a6b08164ac.png)

论文地址：https://arxiv.org/pdf/1511.06391

## 03 长短期记忆网络（LSTM）

在 LSTM 方面，Ilya 推荐了 Anthropic 联合创始人、前 OpenAI 可解释性团队技术负责人 Christopher Olah 2015 年撰写的博客文章《Understanding LSTM Networks，这篇文章全面细致地讲解了 LSTM 的基本知识，并阐明 RNN 取得的显著成果本质上是依靠 LSTM 实现的。

![](img/144b93d4b0dc05e0978b31b5d5e3eb5a.png)

论文地址：https://colah.github.io/posts/2015-08-Understanding-LSTMs/

总的来说，《Understanding LSTM Networks》是一篇面向广泛读者群体的科普文章，旨在降低理解LSTM这一复杂深度学习模型的门槛，无论你是初学者还是有一定基础的研究人员，都能从中获益，加深对LSTM及其背后原理的认识。

## 04 神经网络的复杂度

在「复杂度」方面，Ilya 重点推荐了《Kolmogorov Complexity and Algorithmic Randomness一书中讲解「算法统计」的部分。

![](img/010b4b8dd69d91dbb7a298940d6dfa8d.png)

文章探讨了物理系统随时间增加而复杂性先增后减的现象，与熵的单增性质形成对比。作者尝试利用柯尔莫哥洛夫复杂性理论为这一现象提供一个可能的解释，并提出了“复杂性”的计算方法，旨在更深入地理解系统在不同时间点的“有趣性”。

![](img/945e28700fcb30996f06c29c1298c7df.png)

论文地址：https://scottaaronson.blog/?p=762

## 05 计算机视觉

在计算机视觉方面，Ilya 推荐了 2012 年 ImageNet 图像识别大赛中图灵奖得主 Geoffrey Hinton 组的论文《ImageNet Classification with Deep Convolutional Neural Networks，这篇论文提出了 `AlexNet()`，引入了全新的深层结构和 dropout 方法，颠覆了图像识别领域，甚至被认为开启了深度学习革命。Ilya 也是这篇论文的三位作者之一。

![](img/876a26a2566976691345d90dd0831931.png)

论文地址：https://proceedings.neurips.cc/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf

此外，Ilya 还推荐了神经网络应用于基础科学（化学）的研究论文、扩展定律相关文章等等，并推荐了斯坦福大学计算机科学课程 CS231n：用于视觉识别的卷积神经网络。

![](img/24d8a9eac4fbc277383a8f1e7821641f.png)

课程地址：https://cs231n.github.io/

在这份推荐清单中，我们还看到了很多著名 AI 学者的经典论文，比如一些神经网络架构 Pointer Networks（Ptr-Nets）、神经图灵机（NTM），还有神经网络训练优化相关的《GPipe: Easy Scaling with Micro-Batch Pipeline Parallelism》。

![](img/34c437d6be7d6a429821e4825f188e0a.png)

论文地址：https://arxiv.org/pdf/1811.06965

感兴趣的小伙伴可以查看原推荐清单，了解更多内容。
