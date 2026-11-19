
# 序列到序列模型与机器翻译

> 来源：[https://aicarrier.feishu.cn/docx/S6DndnETSoe96wxwwhIc3yMBnGb](https://aicarrier.feishu.cn/docx/S6DndnETSoe96wxwwhIc3yMBnGb)

[[幻灯片](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture06-fancy-rnn.pdf)] [[笔记（讲座 5 和 6）](https://web.stanford.edu/class/cs224n/readings/cs224n-2019-notes05-LM_RNN.pdf)]

## 建议阅读

1. 统计机器翻译幻灯片，CS224n 2015（讲座 2/3/4）

1. [统计机器翻译](https://www.cambridge.org/core/books/statistical-machine-translation/94EADF9F680558E13BE759997553CDE5)（Philipp Koehn 著）

1. [BLEU](https://www.aclweb.org/anthology/P02-1040.pdf)（原文）

1. [使用神经网络进行序列到序列学习](https://arxiv.org/pdf/1409.3215.pdf)（原始 seq2seq NMT 论文）

1. [使用循环神经网络进行序列传导](https://arxiv.org/pdf/1211.3711.pdf)（早期的 seq2seq 语音识别论文）

1. 通过联合学习对齐和翻译进行神经机器翻译（原始 seq2seq+attention 论文）

1. [注意力机制和增强循环神经网络](https://distill.pub/2016/augmented-rnns/)（博客文章概述）

1. [神经机器翻译架构的大规模探索](https://arxiv.org/pdf/1703.03906.pdf)（超参数选择的实用建议）

1. 利用混合词字模型实现开放词汇神经机器翻译

1. 重新审视基于字符的神经机器翻译的容量和压缩

## 作业3

[代码](https://web.stanford.edu/class/cs224n/assignments/a3_spr24_student_code.zip)

[讲义](https://web.stanford.edu/class/cs224n/assignments/a3_spr24_student_handout.pdf)

[LaTeX 模板](https://web.stanford.edu/class/cs224n/assignments/a3_spr24_student_latex.zip)

[Overleaf 链接](https://www.overleaf.com/read/cmyyxpbfzqyr#c58d0d)
