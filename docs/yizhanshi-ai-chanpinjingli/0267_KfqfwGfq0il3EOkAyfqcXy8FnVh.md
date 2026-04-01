# 动手学深度学习

> 来源：[https://v11enp9ok1h.feishu.cn/docx/WJ49d3KoLozAKrxBswWc8CD2nGE](https://v11enp9ok1h.feishu.cn/docx/WJ49d3KoLozAKrxBswWc8CD2nGE)

<https://v11enp9ok1h.feishu.cn/sync/PaIIdslbxsIJUIbkTfPcsMCFnph>

## 斯坦福 NLP CS224N NLP 深度学习

官方英文网站：https://web.stanford.edu/class/cs224n/

GIthub仓库：https://github.com/SmartFlowAI/CS224N-CN/blob/main/README.md

1.  注意事项：千万不能抄袭，图片copy需要标注引用来源～

1.  **可认领暂无负责人的任务。点击对应任务的“领**取地址”并完成问卷填写即表示完成任务领取

自然语言处理 (NLP) 是人工智能 (AI) 的重要组成部分，它模拟了人们如何共享信息。近年来，深度学习方法在许多 NLP 任务上取得了非常高的性能。在本课程中，学生将全面了解用于 NLP 的尖端神经网络。

## **内容**

### **这门课程是关于什么的？**

自然语言处理 ([NL](https://pytorch.org/)P) 或计算语言学是信息时代最重要的技术之一。NLP 的应用无处不在，因为人们几乎用语言交流所有事物：网络搜索、广告、电子邮件、客户服务、语言翻译、虚拟代理、医疗报告、政治等。在 2010 年代，深度学习（或神经网络）方法在许多不同的 NLP 任务中获得了非常高的性能，使用不需要传统任务特定特征工程的单一端到端神经模型。在 2020 年代，通过扩展大型语言模型（例如 ChatGPT），取得了惊人的进一步进展。在本课程中，学生将全面了解 NLP 深度学习的基础知识以及大型语言模型 (LLM) 的最新前沿研究。通过讲座、作业和最终项目，学生将学习使用 Pytorch 框架设计、实施和理解自己的神经网络模型的必要技能。

> “拿去吧。CS221 教我算法。CS229 教我数学。CS224N 教我如何编写机器学习模型。” ——Carta 上的一名 CS224N 学生

### **先决条件**

+   **精通 Python**

+   [所有课程作](https://numpy.org/)业[都将以 Pyt](https://pytorch.org/)h[o](https://web.stanford.edu/class/cs224n/#schedule)n 形式完成（使用NumPy和PyTorch）。如果您需要重温 Python，或者对 NumPy 不太熟悉，您可以参加第一周的 Python 复习课程（在时间表中列出）。如果您有丰富的编程经验，但使用不同的语言（例如 C/C++/Matlab/Java/Javascript），那么您可能会没问题。

+   **大学微积分、线性代数**（例如 MATH 51、CME 100）

+   您应该能够轻松地获取（多变量）导数并理解矩阵/向量符号和运算。

+   **基础概率与统计学**（如 CS 109 或同等课程）

+   您应该了解概率、高斯分布、平均值、标准差等的基础知识。

+   **机器学习基础**（例如 CS221、CS229、CS230 或 CS124）

+   我们将[制定成本函数，](http://ciml.info/)求导数并使用梯度下降进行优化。如果您已经具备基本的机器学习和/或深度学习知识，那么本课程将更容易；但是，即使没有这些知识，也可以参加 CS224n。有很多关于 ML 的介绍，包括网页、书籍和视频形式。一个平易近人的介绍是 Hal Daumé 正在编写的《机器学习课程》。阅读该书的前 5 章将是一个很好的背景知识。了解前 7 章就更好了！

### **参考文献**

以下文章很有用，但都不是必需的。它们都可以在线免费阅读。

+   Dan Jurafsky 和 James H. Martin。语音和语言处理（2024 年预发行）

+   [Jacob ](https://github.com/jacobeisenstein/gt-nlp-class/blob/master/notes/eisenstein-nlp-notes.pdf)Eisenstein。自然语言处理

+   Yoav Goldberg。[自然语言处理神经网络模型入门](http://u.cs.biu.ac.il/~yogo/nnlp.pdf)

+   Ian Goodfellow、Yoshua Bengi[o 和 ](http://www.deeplearningbook.org/)Aaron Courville。深度学习

+   Delip Rao 和 Brian McMahan。使用 PyTorch 进行自然语言处理（需要斯坦福登录）。

+   Lewis Tunstall、Leandro von Werra 和 Thomas Wolf。使用 Transformers 进行自然语言处理

如果你没有神经网络背景，但仍想参加该课程，你可能会发现以下其中一本书可以为你提供更多背景知识：

+   [Michael A](http://neuralnetworksanddeeplearning.com/). Nielsen。神经网络和深度学习

+   [Eugene](https://mitpress.mit.edu/books/introduction-deep-learning) Charniak。深度学习简介

### 日程

| 日期 | 任务编号 | 描述 | 课程材料 | 活动 | 领取问卷 | 负责人 |
| --- | --- | --- | --- | --- | --- | --- |
| 第一周 | CS224N-101 | 词[向量  ](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture01-wordvecs1.pdf)[幻灯[片]](https://web.stanford.edu/class/cs224n/readings/cs224n_winter2023_lecture1_notes_draft.pdf) [笔记] | 建议阅读： | 作[业 ](https://web.stanford.edu/class/cs224n/assignments/a1.zip)1[  [](https://web.stanford.edu/class/cs224n/assignments/a1_preview/exploring_word_vectors.html)代码]  [预览] |  |  |
|  | CS224N-102 | 词[向量和](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture02-wordvecs2.pdf)语言模[型  ](https://web.stanford.edu/class/cs224n/readings/cs224n-2019-notes02-wordvecs2.pdf)[幻灯[片]](https://web.stanford.edu/class/cs224n/materials/gensim_2024.zip) [笔记] [代码] | 建议阅读： |  |  |  |
|  | CS224N-103 | P[yth](https://web.stanford.edu/class/cs224n/readings/cs224n-python-review-2023.pdf)on 回[顾课程  [](https://colab.research.google.com/drive/1hxWtr98jXqRDs_rZLZcEmX_hUcpDLq6e?usp=sharing)幻灯片] [ colab ] |  |  |  |  |
| 第二周 | CS224N-201 | 反[向传播](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture03-neuralnets.pdf)和神经[网络](https://web.stanford.edu/class/cs224n/readings/cs224n-2019-notes03-neuralnets.pdf)基础  [幻灯片] [笔记] | 建议阅读： | 作[业 ](https://web.stanford.edu/class/cs224n/assignments/a2.zip)2[  [](https://web.stanford.edu/class/cs224n/assignments/a2.pdf)代码[]  [讲义]  [](https://web.stanford.edu/class/cs224n/assignments/a2_latex_template.zip) latex 模板] |  |  |
|  | CS224N-202 | 依[存句法](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture04-dep-parsing.pdf)分析  [[幻](https://web.stanford.edu/class/cs224n/readings/cs224n-2019-notes04-dependencyparsing.pdf)灯片] [注释] | 建议阅读： |  |  |  |
|  | CS224N-203 | Py[Torch](https://colab.research.google.com/drive/1Pz8b_h-W9zIBk1p2e6v-YFYThG1NkYeS?usp=sharing) 教程课程  [ colab ] |  |  |  |  |
| 第三周 | CS224N-301 | 循[环神经](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture05-rnnlm.pdf)网络  [[幻灯片] [笔记（第 5 ](https://web.stanford.edu/class/cs224n/readings/cs224n-2019-notes05-LM_RNN.pdf)和第 6 讲）] | 建议阅读： |  |  |  |
|  | CS224N-302 | 序[列到序](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture06-fancy-rnn.pdf)列模型[和机器翻译  [幻灯片] ](https://web.stanford.edu/class/cs224n/readings/cs224n-2019-notes05-LM_RNN.pdf)[笔记（讲座 5 和 6）] | 建议阅读： | 作[业 ](https://web.stanford.edu/class/cs224n/assignments/a3_spr24_student_code.zip)3[  [](https://web.stanford.edu/class/cs224n/assignments/a3_spr24_student_handout.pdf)代码[]  [讲义]  [](https://web.stanford.edu/class/cs224n/assignments/a3_spr24_student_latex.zip) l[atex 模板]  [ ](https://www.overleaf.com/read/cmyyxpbfzqyr#c58d0d)overleaf 链接] |  |  |
| 第四周 | CS224N-401 | 最[终项目](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture07-final-project.pdf)和 LLM 简介  [幻灯片] | 建议阅读： | 项目提案**出炉**  [[讲](https://web.stanford.edu/class/cs224n/project/project-proposal-instructions-spr2024-updated.pdf)义]  默认**最终**项[目出](https://web.stanford.edu/class/cs224n/project/default-final-project-handout-minbert-spr2024-updated.pdf)炉  [讲义] |  |  |
|  | CS224N-402 | Transformer[s  (](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture08-transformers.pdf)作者：[安娜](https://web.stanford.edu/class/cs224n/readings/cs224n-self-attention-transformers-2023_draft.pdf)·戈尔迪)  [幻灯片] [笔记] | 建议阅读： |  |  |  |
| 第五周 | CS224N-501 | 预[训练  ](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture09-pretraining-updated.pdf)[幻灯片] | 建议阅读： | 作[业 ](https://web.stanford.edu/class/cs224n/assignments/a4_spr24_student_code.zip)4[  [](https://web.stanford.edu/class/cs224n/assignments/a4_spr24_student_handout.pdf)代码[]  [讲义]  [](https://www.overleaf.com/read/srrtnkygvdpj#fc4729) o[verleaf ]  ](https://colab.research.google.com/drive/193j033YPWyzooOK_C4tSBpnaguoewN7X?usp=sharing)[ colab 运行脚本] |  |  |
|  | CS224N-502 | Po[st-](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture10-prompting-rlhf.pdf)Training（RLHF、SFT、DPO）  （由 Archit Sharma 提供）  [幻灯片] | 建议阅读： |  |  |  |
|  | CS224N-503 | Hu[gging](https://colab.research.google.com/drive/13r94i6Fh4oYf-eJRSi7S_y_cen5NYkBm#scrollTo=OTsW-Wwi-X81) Face Transformers 教程课程  [ colab ] |  |  |  |  |
| 第六周 | CS224N-601 | 基准测试与评估  (作者：Yann[ Du](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture11-evaluation-yann.pdf)bois )  [幻灯片] | 建议阅读： |  |  |  |
|  | CS224N-602 | 高效[的神经](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture12-training-shikhar.pdf)网络训练  (作者： Shikhar Murty )  [幻灯片] | 建议阅读： | 项[目里](https://web.stanford.edu/class/cs224n/project/project-milestone-instructions-spr2024.pdf)程碑  [讲义] |  |  |
| 第七周 | CS224N-701 | 语音脑机接口  (作者：Chaof[ei ](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture13-speech-bci.pdf)Fan)  [幻灯片] | 建议阅读： |  |  |  |
|  | CS224N-702 | 推理[与代理](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture14-agents-shikhar-updated.pdf)  (作者： Shikhar Murty )  [幻灯片] | 建议阅读： |  |  |  |
| 第八周 | CS224N-801 | DP[O 之](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture15-life-after-dpo-lambert.pdf)后的生活  (作者：Nathan Lambert )  [幻灯片] | 建议阅读： |  |  |  |
|  | CS224N-802 | C[onv](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture16-CNN-TreeRNN.pdf)Nets、树递归神经网络和成分分析  [幻灯片] | 建议阅读（暂定）： | 最[终项](https://web.stanford.edu/class/cs224n/project/CS224n_Spring_2024_Final_Project_Report_Instructions.pdf)目报告  [说明] |  |  |
| 第九周 | CS224N-901 | 负责任的 NLP 简介  （作者：Adina Williams） | 建议阅读： |  |  |  |
|  | CS224N-902 | N[LP、](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture18-nlp-linguistics-philosophy.pdf)语言学和哲学  [幻灯片] |  |  |  |  |
| 第十周 | 最终项目海报展示会 |  |  |  |  |  |