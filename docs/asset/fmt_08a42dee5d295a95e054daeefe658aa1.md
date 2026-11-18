
## GPU 深度学习性能的三驾马车：Tensor Core、内存带宽与内存层次结构

> https://zhuanlan.zhihu.com/p/669987669?utm_medium=social&utm_oi=56635854684160&utm_psn=1736330574372118528&utm_source=wechat_timeline

本篇文章讨论了 GPU 在深度学习性能中的三个关键因素：Tensor Core、内存带宽与内存层次结构。Tensor Core 对于矩阵乘法运算至关重要，可以大幅提升性能。由于矩阵乘法是深度神经网络中计算量最大的部分，因此这种计算核心显得格外重要。内存带宽的大小直接影响 Tensor Core 的数据获取速度，进而影响整体性能；而内存层次结构，尤其是二级缓存的大小，决定着训练过程中数据加载的次数。了解这些性能指标有助于选用合适 GPU、优化深度学习应用的运行效率。

## DeepLearning Al：新短期课程“使用向量数据库构建应用程序”

> https://x.com/DeepLearningAI/status/1752730997936771398?s=20

DeepLearning.Al： 查看与 @Pinecone 合作创建的新短期课程：使用向量数据库构建应用程序。

学习如何构建六个使用向量数据库的应用程序，包括 RAG、面部相似性和异常检测。

立即注册：https://hubs.la/Q02jhPPgO

## 论文阅读：RWKV Architecture & Linear Transformer & Attention Free Transformer

> https://zhuanlan.zhihu.com/p/680322418?utm_medium=social&utm_oi=56635854684160&utm_psn=1735334404703043584&utm_source=wechat_timeline

知乎专栏文章介绍了几种线性复杂度的 Transformer 架构：RWKV（Rethinking Weight Key Value）、Linear Transformer 和 Attention Free Transformer（AFT）。RWKV 结构采用了相对位置编码，通过 token shift 机制实现序列间的信息传递，优化了模型的运算效率。Linear Transformer 使用 kerne|函数简化传统注意力机制，从而将计算复杂度从平方级降至线性。而 AFT 摒弃了 Attention 机制，利用门控函数和位置信息的加权和改进了模型结构，同时简化了计算。这些架构在推理效率和存储空间上均有显著改进。

## 写在跨年之前：聊聊 LLM Agents 的现状，问题与未来

> https://zhuanlan.zhihu.com/p/679177488

作者在知乎专栏文章中讨论了 LLM Agents 的当前状况、问题和未来展望。首先给出了三种理解 LLM Agents 的视角，包括面向用户的实用定义、直观定义以及生产者视角的学术定义。接着深入讲解了构建 LLM Agents 时会使用到的一些技术，如 RAG、COT、多模态等，并探讨了各技术的细节、应用场景和潜在问题。此外，还涉及了如意图识别与执行、数据通路与行动框架等概念。最后，作者分享了对 LLM Agents 可能会遇到的问题的反思，以及未来的愿景，特别是在游戏领域中的应用。整篇文章围绕大型语言模型的智能体展开深入的分析和展望。

## 随机 Transformer

> https://mp.weixin.qq.com/s/4WtoHGegZY6o4Jaa3bz66Q

Transformer 模型数学原理解释

📄 文本嵌入 将输入 token 转换为向量

1️⃣ 位置编码 表示单词在句子中位置的信息

↔️ 自注意力 模型控制输入不同部分的重要程度
