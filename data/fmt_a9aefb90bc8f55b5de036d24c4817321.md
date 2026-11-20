
# 大模型厂商纷纷入局的Prompt Cache技术解析

> 来源：[https://agijuejin.feishu.cn/docx/TuUQdrV4Oo0L8rxJe7yc29gBngc](https://agijuejin.feishu.cn/docx/TuUQdrV4Oo0L8rxJe7yc29gBngc)

> 作者：PaperAgent

> 原文：[https://mp.weixin.](https://mp.weixin.qq.com/s/xXlrw7kgVyD1_0rOuYm-cQ)qq.com/s/xXlrw7kg...

Prompt Cache技术，旨在通过在大型语言模型（LLM）的推理过程中重用不同提示（prompts）之间的注意力状态来加速推理。

图1 比较大型语言模型（LLM）生成Token的方法，每种方法展示三个步骤（1至3）。每个框表示一个令牌。蓝色框代表提示。(a) 一个LLM接收一个提示（蓝色令牌）并预测下一个令牌（A）（1）。然后，它将生成的令牌（A）附加到提示上以预测下一个令牌（B）（2）。这个过程被称为自回归，会一直持续直到满足停止条件。(b) KV缓存仅在第一步（1）计算一次提示的时间注意力状态，并在随后的步骤中重复使用它们；(c) Prompt Cache在服务之间重用KV状态以绕过提示注意力计算。当加载一个模式时，Prompt Cache会填充其缓存，并为从模式派生的提示重用缓存状态（1）。图2进一步详细说明了步骤1。

![](img/328d999da4df96d7b8484afe4c14ba62.png)

## 问题识别

许多输入提示在结构上高度重叠，例如系统消息、提示模板和文档上下文。这些重叠的文本段可以预先计算并存储其注意力状态，以便在用户提示中出现时重用。

## Prompt Cache技术

通过使用称为Prompt Markup Language（PML）的模式，明确定义可重用的文本段，称为提示模块（prompt modules）。PML确保在重用注意力状态时位置的准确性，并为用户提供了一个接口来访问他们的提示中的缓存状态。

## 工作流程

当Prompt Cache接收到一个提示时，它首先处理其模式，并计算其提示模块的注意力状态。然后，这些状态被重用于提示中的提示模块，以及其他从同一模式派生的提示。

图2 Prompt Cache中的重用机制：(i) 首先，PML在模式和提示中明确了可重用的提示模块。提示模块可以有参数，如行程计划。导入模块的提示为参数（持续时间）提供值（3天）。提示可以在排除的模块和参数的位置上包括新的文本段，并在末尾添加。(ii) 其次，提示模块编码为模式中的所有模块预先计算注意力状态（1），并为将来的重用而缓存它们。(iii) 第三，当提供提示时，Prompt Cache采用缓存推理：它检索为导入的提示模块缓存的注意力状态（2），为参数（3）和新的文本段（4）计算它们，最后将它们连接起来，以产生整个提示的注意力状态（5）。这个图是对图1c中步骤1的进一步阐述。

![](img/fa226a611ea0b7c256f2a510b997cc1a.png)

## 设计和实现

Prompt Cache的设计包括了对提示结构的明确化、提示模块的编码、以及缓存推理的详细过程。实现使用了HuggingFace的`transformers`库，并在CPU和GPU上进行了评估。

使用原型实现，在多个LLM上评估了Prompt Cache。结果表明，Prompt Cache显著减少了首次生成token的时间延迟，尤其是在基于文档的问答和推荐等长提示上。GPU上的性能提升范围从8倍到60倍，CPU上则高达60倍，所有这些提升都在保持输出准确性的同时，无需修改模型参数。

## GPU延迟测量

首次令牌时间（TTFT）对于三个NVIDIA GPU上的八个LongBench数据集。

![](img/efe2d9b65f93f23da1ecec9816a82da6.png)

## CPU延迟测量

首次令牌时间（TTFT）对于两个CPU上的八个LongBench数据集。

![](img/160582eb7422c866916de9c688f10eb9.png)

![](img/2f21a9f7232c7e9a854c631ce144f28a.png)

```
https://arxiv.org/pdf/2311.04934
PROMPT CACHE: MODULAR ATTENTION REUSE FOR LOW-LATENCY INFERENCE
耶鲁大学、Google
```

## 推荐阅读

+ 对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO
+ 一篇搭建AI大模型应用平台架构的全面指南
+ [RAG全景图：从RAG启蒙到高级RAG之](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)36技，再到终章Agentic RAG！
+ [A](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)gent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）

---

欢迎关注我的👸号“PaperAgent”，每天一篇大模型（LLM）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。
