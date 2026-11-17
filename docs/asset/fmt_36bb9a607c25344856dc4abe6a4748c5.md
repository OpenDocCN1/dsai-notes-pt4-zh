
# RAG or Long Context Window?

> 来源：<https://agijuejin.feishu.cn/docx/D1cMd12UFoXp7Txx9qTcSlIunie>
> 作者：Meta
> 原文：<https://zhuanlan.zhihu.com/p/678407209>

为了将LLM落地在特定的领域场景，通常有这样一些技术路线可供选择：

+ 检索增强生成（RAG），首先利用检索系统从海量领域文档中召回与用户`query`相关的少量文档，将这些文档整合后输入LLM进行结果生成。
+ 直接利用LLM的长文本（Long Context Window）处理能力，将所有相关文档全部输入给LLM，直接生成结果。
+ 微调，将领域知识以参数化的形式直接嵌入LLM。

对于普通用户而言，微调模型所需要耗费的时间和经济成本都是比较高的，所以通常会在RAG和Long Context两种方案中间做选择。那么究竟哪种方案更胜一筹呢？

Atai Barkai 做了一个详细的对比实验，从准确率、成本、时延这三个角度全面比较了RAG和Long Context这两种方式的优劣，以下是他的一些实验结果。

## 准确率

**一**句话结论：调优的RAG方案准确率优于直接用Long Context，RAG要想达到较好的效果需要做很多优化工作。

这里采用了 [Greg Kamradt](https://link.zhihu.com/?target=https%3A//twitter.com/GregKamradt) 提出的“大海捞针”测试来评测RAG和Long Context的能力，其中LLM用的是GPT-4-Turbo，RAG方案用了两种实现方式（一种是OpenAI提供的assistants API RAG，另一种方案是LlamaIndex的基础RAG方案）。

实验的结果如下图所示：

![](img/7eae754e70d806f002bb8a388fed8ad4.png)

从下图可以看到 GPT-4-Turbo+assistants API RAG的RAG方案明显取得了比GPT-4-Turbo更优秀的结果。

同时也注意到采用LlamaIndex的RAG方案取得了相对较差的效果，很有可能是采用的检索方案比较简单，未能在检索阶段将相关文档召回完整。这点可以从OpenAI关于RAG的技术分享中得到佐证，采用最初级的RAG是无法得到很好的效果的，但是对各个模块进行深度调优就能取得很惊艳的效果。

![](img/a1b3f7eda3e14febca8392dd14b51fc9.png)

## 成本

一句话总结：RAG方案的成本仅需Long Context方案的4%

这里Atai Barkai对几种方案所需的成本进行了详细的分析，从最后折算下来的成本来看，在128K上下文的情况下，assistants API RAG方案的成本仅需Long Context 方案的4%。在5K上下文的情况下，直接用LLM比较划算。

![](img/94b27f13cab3d8cefe3a9a2bd5edb321.png)

## 时延

**一**句话总结：RAG方案耗时略长于Long Context Window方案，主要原因在于多了显式的向量嵌入和检索过程。

从响应时间来看，

LlamaIndex RAG方案响应最快，平均为12.9s；GPT-4-Turbo平均响应时间为21.6s，不过方差比较大，分布在7-36s之间；而assistant API RAG方案平均响应时间最长，为24.8s。

RAG方案之所以比直接采用Long Context Window更加耗时一些，主要是多了显式的文本分块、嵌入、检索等链路的繁琐流程，随着后续各种工程手段的优化，相信这一差距会逐步减小。

![](img/fa2541a8166e401c1e72f0e2643cf1e9.png)

## 结论

根据以上分析，可以得出结论：在当前阶段，通过采用RAG方案，我们能够在成本仅为4%的条件下，取得比直接使用LLM的Long Context方案更出色的结果。RAG方案具有巨大的潜力！

## 参考

1. [RAG vs. Context-Window in GPT-4: accuracy, cost, & latency](https://link.zhihu.com/?target=https%3A//ai88.substack.com/p/rag-vs-context-window-in-gpt4-accuracy-cost)
2. [Pressure Testing GPT-4-128K With Long Context Recall](https://link.zhihu.com/?target=https%3A//twitter.com/GregKamradt/status/1722386725635580292)
3. [Claude 2.1 (200K Tokens) - Pressure Testing Long Context Recall](https://link.zhihu.com/?target=https%3A//twitter.com/GregKamradt/status/1727018183608193393)
4. [LLMTest_NeedleInAHaystack](https://github.com/gkamradt/LLMTest_NeedleInAHaystack)
5. [YouTube: ahnGLM-RC1Y](https://www.youtube.com/watch?v=ahnGLM-RC1Y)
