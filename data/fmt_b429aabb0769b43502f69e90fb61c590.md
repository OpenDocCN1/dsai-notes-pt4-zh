
# RAGChecker为你的RAG系统提供全方位诊断

> 来源：[https://agijuejin.feishu.cn/docx/K6BSdRxUFowefhxMHQic0T61n6f](https://agijuejin.feishu.cn/docx/K6BSdRxUFowefhxMHQic0T61n6f)
>
> 作者：PaperAgent
>
> 原文：[https://mp.weixin.qq.com/s/66Rz9-Rlxv0Dg3i80YjVJw](https://mp.weixin.qq.com/s/66Rz9-Rlxv0Dg3i80YjVJw)

由于RAG系统的模块化特性、对长文本响应的评估需求以及现有评估指标的可靠性不足，对RAG系统进行全面评估存在挑战。

亚马逊AWS AI开源了`RAGChecker`，一个基于声明级别蕴含性检查的细粒度评估框架，涉及从响应和真实答案中提取声明并与其他文本对照。

`RAGCHECKER`中提出的指标的说明。上面的维恩图展示了模型响应与真实答案之间的比较，显示了可能的正确（O）、错误（X）和缺失的声明（V）。检索到的块根据它们包含的声明类型被分类为两类。下面，定义了整体、检索器和生成器的指标，说明了如何评估RAG系统的每个组件的性能。

![](img/01a7af09eb22c46d682a3dca6cf518c2.png)

`RAGChecker`使开发者和研究人员能够精确深入地全面评估、诊断和增强他们的RAG系统：

+   **全面评估**：`RAGChecker`提供整体指标，用于评估整个RAG流程。
+   **诊断指标**：用于分析检索组件的诊断检索器指标。用于评估生成组件的诊断生成器指标。这些指标为针对性改进提供了有价值的见解。
+   **细粒度评估**：利用声明级别的蕴含操作进行细粒度评估。
+   **基准数据集**：一个包含4000个问题、涵盖10个领域的全面的RAG基准数据集（即将推出）。
+   **元评估**：一个用于评估`RAGChecker`结果与人类判断相关性的人工标注偏好数据集。

通过在10个领域的公共数据集上对8个最先进的RAG系统进行综合实验，`RAGCHECKER`显示出与人类评估者有更强的相关性，并提供了关于RAG系统组件行为和设计中固有权衡的深刻见解。

**RAG基准统计信息**。此基准测试是从涵盖10个领域的公共数据集中重新调整用途构建的，包含4,162个问题。对于财务、生活方式、娱乐、科技、科学和小说到领域的数据，简短的答案已使用`GPT-4`扩展为长篇答案。

![](img/d2a63bf7436ebddd8a75a8001f25ecaa.png)

与人类评估的正确性、完整性和整体评估的相关性结果。展示了每个基线框架（`TruLens`、`RAGAS`、`ARES`、`CRUD-RAG`）相关指标。

![](img/354aa0112632d35d6016126644f2bb5e.png)

`RAGCHECKER`的指标可以帮助研究人员和实践者开发更有效的RAG系统，并通过调整RAG系统的设置（如检索器的数量、块大小、块重叠比例和生成提示）来提供改进建议。

不同RAG系统在10个数据集上的平均评估结果。使用精确度（`Prec.`）、召回率（`Rec.`）和`F1`分数来量化RAG系统的整体性能。检索器组件基于声明召回率（`CR`）和上下文精确度（`CP`）进行评估，而生成器组件则通过上下文利用度（`CU`）、相关噪声敏感性（`NS(I)`）、不相关噪声敏感性（`NS(II)`）、幻觉（`Hallu.`）、自我知识（`SK`）和忠实度（`Faith.`）进行诊断。此外，还提供了每个RAG系统的平均响应声明数量。

![](img/0f075d43e498fd2fc3b1baa503dc0ae2.png)

## 附录

`RAGCHECKER`和`RAGAS`答案相似性预测分数分布的比较。图中的每个点代表元评估数据集中的一个实例，其中`x`轴是相应方面的人类偏好标签，`y`轴是`RAGCHECKER`和`RAGAS`答案相似性的预测分数。预测分数的分布由彩色区域表示，虚线是平均线。

![](img/bed53eb2bc39d0c0c660aa27d685e8fe.png)

![](img/a9416946c50472a3675e164983b12cf4.png)

![](img/1d05b9f3fa327f8b19af55254e180ef4.png)

更多上下文增强了忠实度诊断示例分析

`Top-k`选择和片段大小都平衡了呈现给生成器的噪声量和有用信息量，但方式不同。相应的结果在下面两张图中展示。

+   增加`k`会增加可能不太相关的更多上下文，而增加片段大小则提供了更多相关事实的周围上下文。因此，随着`k`的增大，上下文精确度会降低，但随着片段大小的增大而提高。尽管如此，它们都会导致检索中更好的声明召回率。
+   生成器在提供更多上下文时往往更加忠实，尽管这一趋势对于`Llama3`来说不太明显，因为它已经表现出很高的忠实度。由于噪声的增加，上下文利用度通常随着更多上下文而恶化，导致更高的相关噪声敏感性。
+   端到端的RAG性能在更多上下文的情况下略好，主要是由于召回率的提高。建议适度增加这两个参数以实现更忠实的生成，同时要注意在高值时会出现饱和，因为有用信息的数量是有限的。在上下文长度有限的情况下，更倾向于选择较大的片段大小和较小的`k`，特别是对于较容易的数据集（金融、写作）。这在比较片段大小为150且`k=20`与片段大小为300且`k=10`时尤为明显。

`Top-k`选择的诊断

![](img/b988eae57021407e8eb45ddcdc9382fa.png)

片段大小的诊断

![](img/13c17c0658b8f6a94985086859f4fcf8.png)

```
https://arxiv.org/pdf/2408.08067
RAGCHECKER: A Fine-grained Framework for Diagnosing Retrieval-Augmented Generation
https://github.com/amazon-science/RAGChecker
```

## 推荐阅读

+   对齐LLM偏好的直接偏好优化方法：`DPO`、`IPO`、`KTO`
+   一篇搭建AI大模型应用平台架构的全面指南
+   [RAG全景图：从RAG启蒙到高级RAG之36技，再到终章`Agentic RAG`！](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)
+   [Agent到多模态Agent再到多模态`Multi-Agents`系统的发展与案例讲解（1.2万字，20+文献，27张图）](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)

---

欢迎关注我的👸号“`PaperAgent`”，每天一篇大模型（LLM）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。
