# 后GraphRAG时代，利用KG提升LLM领域问答可解释性的系统Fact Finder！

> 来源：[https://agijuejin.feishu.cn/docx/KBfqdnw9voAoxjxlyh2cwZDOnse](https://agijuejin.feishu.cn/docx/KBfqdnw9voAoxjxlyh2cwZDOnse)

> 作者：PaperAgent

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/M-ED30cFMGMNEWWMF8mJdQ)qq.com/s/M-ED30cF...

“后GraphRAG时代 ” ： 探索解决GraphRAG落地场景、工程化以及降**成本**等问题。PaperAgent团**队经过一段****时**间探索，已经初步走通一条覆盖场景、效果、成本、部署的路，后续会分享 ， GraphRAG的中间产物有 知识图谱 ，其结合LLM可以提升在 特定领域知 识问答可靠性与可解释性 。

今天介绍了一个名为 **FactF**in**der的开源混合系**统 ，通过结合特定领域的知识图谱（KGs）来增强 LLMs 的 领域专业知识，  特别是在生命科学等需要最新和事实信息的领域，获取正确和完整的答案至关重要。

使用大型语言模型和知识图谱回答科学问题的FactFinder流水线概述

![](img/e0dca40ae9c2af1032e5ac217cf21edc.png)

**FactFinder系统 **包括以下步骤：

+   **预处理 **：对知识图谱进行预处理，包括映射名称到首选术语和转换所有条目为小写。

+   **Cypher查询生成 **：使用LLMs根据问题和图谱模式生成Cypher查询。

原始的 Cypher 查询通常只返回回答问题所需的节点，而忽略了连接问题实体与答案节点的边。为了解决这个问题，需要生成一个新的 Cypher 查询，它能够同时获取答案节点和连接边。

![](img/18ea41029f9ee64a0eba2eea4e36434a.png)

+   **Cypher查询处理 **：对生成的Cypher查询进行预处理，以提高系统的鲁棒性。

+   **知识图谱检索 **：执行预处理后的Cypher查询，从知识图谱中检索数据。

+   **增强型LLM响应生成 **：将问题和图谱结果整合到提示模板中，并发送给LLM，以生成自然语言答案。

带有生成的Cypher查询和作为证据的图谱响应的示例

![](img/bdeaeccfeadb3aa37e10105fe4a8e312.png)

**此外，系统**还提**供了可解释性**功能，包括** 中间结果 和 **子图可视化 ，以增强 透明度和解释性 。

**针对癫痫患**者不应使用哪些抗癫痫药物的 证据子图 示例

![](img/efd3ddf068e461e7cb9ef682ab845ab9.png)

带有独立大型语言模型和基于图谱的混合系统问题与答案的用户界面

![](img/148665b804a0728813e9932cd013076b.png)

探索用于治疗癫痫的药物的答案

![](img/b1a168ef4c51206acb39638b18242826.png)

探索被安巴比妥靶向但不是拉莫三嗪靶向的基因的答案

![](img/b747b3ebd88069a270be808aaa409d84.png)

**FactFinder**系统通过结合知识图谱和LLMs，提供了一个 用户友好的聊天系统 ，用于回答科学问题，同时最小化了“幻觉”（错误的陈述）。研究表明，LLMs能够生成有效的 Cypher查询来检索知识图谱中的相关数据 （ 实现了78%的精度在检索正确的KG节点上 ） ，从而提供准确 的答案。

Table1： 展示了 FactFinder 系统在图检索评估中的结果，使用了与真实文本到 Cypher 查询数据集的比较来量化图查询返回的节点。评估使用了交并比（IoU）、精确度和召回率三个指标

Table2：评估了 FactFinder 系统处理不正确或不完整图响应的能力。测试通过禁用 Cypher 查询生成并为每个问题提供错误的 Cypher 查询来进行，导致图结果不正确。结果显示

![](img/f108b10ec65ebe372c24b5510a1415ec.png)

# **附录**

**Fact Finder的用****户界面，针对问题“ **哪些药物用于治疗眼高压？ ”。独立大型语言模型和基于图谱的混合系统的答案被比较并输出。此外， 相关的子图作为证据 与生成的Cypher查询和图谱中的答案一起展示。

![](img/58498d0c20c68b3ce4a25a34783eaf3a.png)

Fact Finder的用户界面，针对问题“哪些抗癫痫药物不应由高血压患者使用？ ”。

![](img/afb21d10720742d20a23b25002dd918a.png)

```
https://arxiv.org/pdf/2408.03010
Fact Finder - Enhancing Domain Expertise of Large Language Models by Incorporating Knowledge Graphs
https://github.com/chrschy/fact-finder
```

**推荐阅读**

+   对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO

+   一篇搭建AI大模型应用平台架构的全面指南

+   [RAG全景图：从RAG启蒙到高级RAG之](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)36技，再到终章Agentic RAG！

+   [A](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)gent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）

---

欢迎关注我的公众号“ **PaperAgent **”， 每天一篇大模型（LLM）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。