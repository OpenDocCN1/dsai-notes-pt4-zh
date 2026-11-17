# LangChain技术报告：如何构建可靠智能体Agent？

> 来源：[https://agijuejin.feishu.cn/docx/HluBdd0Ero9QxExKbeLcvzjBnhd](https://agijuejin.feishu.cn/docx/HluBdd0Ero9QxExKbeLcvzjBnhd)

> 作者：PaperAgent

> 原文：https://mp.weixin.qq.com/s/rW4q2wBAVmU9yOaIE4NbeA

LangChain 团队技术报告：

+   报告人： Lance Martin

+   报告人信息：  LangChain的软件工程师

+   报告主题：“ 构建与测试可靠智能体 ”

+   主题原文：“ **Architecting + test**ing reliable agents ”

+   报告概要内容如下：

构建一个可靠的Agent是一个复杂的过程，涉及到多个方面，包括控制流的设定、工具调用、Agent的灵活性与可靠性，以及测试和部署，一些关键点：

1.  **理解控制流 **：Agent应用程序遵循控制流，这是程序执行的顺序。理解这一点对于设计Agent至关重要。

1.  **设定控制流 **：开发者可以通过设定控制流来创建链（Chain），这是通过开发者设定的一系列步骤。

1.  利用LLM（Large Language Model） ：Agent可以由LLM设定控制流，这增加了Agent的灵活性。

1.  **工具调用 **：Agent通常使用工具（函数）调用来执行步骤，这是实现Agent功能的一种流行方法。

1.  **ReAct Agent **：ReAct是一个灵活的Agent框架，它允许多种可能的控制流。

1.  **LangGraph **：LangGraph是一种表达控制流的图形化方法，可以用来构建LangGraph Agent。

1.  **结合开发者和LLM **：在构建Agent时，可以结合开发者设定的可靠控制流部分和LLM提供的灵活性。

1.  **测试Agent性能 **：使用LangSmith等工具来测试Agent的性能，确保在部署前能够捕捉到错误。

1.  **部署与反馈 **：部署Agent后，建立系统以在部署过程中捕捉错误，并利用反馈循环来加速开发。

1.  **持续迭代 **：Agent的构建不是一次性的，而是一个持续迭代的过程，需要不断地测试、评估和改进。








































![](img/ab9022956d18a4ec81e647be84d1db46.png)

**推荐阅读**

+   • 对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO

+   • 2024：ToB、Agent、多模态

+   • [RAG全景图：从RAG启蒙到高级RAG之3](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)6技，再到终章Agentic RAG！

+   • [A](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)gent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）