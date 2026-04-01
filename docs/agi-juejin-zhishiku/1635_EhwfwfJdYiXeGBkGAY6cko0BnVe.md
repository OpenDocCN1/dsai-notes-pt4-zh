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

![](img/6488cb8b157d09fdb070f253e21aeaf1.png)

![](img/b0f52bc7b0d278a02928316d4684ab7c.png)

![](img/8718f5784133e3583d05f16382760fe6.png)

![](img/137444dbedbd672079621b1773ae8eda.png)

![](img/a7ed1243ad5d3f6e15ca1ed089739141.png)

![](img/de150fd4418042fa1cbb941888d2ba17.png)

![](img/3174695fb3cd9d1e0248d766ec4f1929.png)

![](img/77b9da3e4190ecf3314f4ffe9eac1f91.png)

![](img/f9d81e637163f5f8a11aadce3a04b19c.png)

![](img/c35507f41dcc6681be9be5d94195f439.png)

![](img/ca10797dd1942b3130621c8744bf55fe.png)

![](img/80f572b053da84bb82ea6aab9e5fdd51.png)

![](img/23cc168d6036f71fda4e9eaa537c0f5a.png)

![](img/1f2cb8bd458cbb03116373a810621240.png)

![](img/56ef0725082308dfbdaced9fbe87ee0a.png)

![](img/a1e237943f361d663fc5049c4b1f5657.png)

![](img/81b3a9e755264b145f767f34ea392259.png)

![](img/21059792977c2942526409944ddf4994.png)

![](img/658a6000da6930f91214f86367b2f8ef.png)

![](img/3b83b4516680758f700f3f1ff3e9edbd.png)

![](img/d5faf11bb914e37192490a52b304328c.png)

![](img/fba5f4213d8c1faa137fe3d2607a87e0.png)

![](img/0781aa5ce2e1768765486d505f2c2c8c.png)

![](img/dea9bee57423288a17e6cbacd550af88.png)

![](img/79fc7f04bd45a1405f1c6587c18c6f69.png)

![](img/a95073a103add0941b131565b1e63eed.png)

![](img/3028599774fb25419161c656aa234c75.png)

![](img/f89f22097e7e239a83053e0c54ff4ab2.png)

![](img/e19ff73276d77a90f98898a0a4778ee8.png)

![](img/0fe9b4e5585ff4e71995b66b0200e91e.png)

![](img/788104be997a46f27baa870555a045c3.png)

![](img/138b3f9b68109f823bd86a307e138138.png)

![](img/b59c0aa8966c8850bbfcded6447169f7.png)

![](img/265845642288d18573ad09bbd54c991f.png)

![](img/602d0016f90767965b7f47ff59bfd96b.png)

![](img/102cad70f1df94c6d17e4e73d3cc4c06.png)

![](img/515eeeba3ed79fece1991ede3659f3b1.png)

![](img/2a17b6ea5f1b10fb7e41e0a9b3f4dbe9.png)

![](img/0c86844130c8f999b239c00f21b4cc63.png)

![](img/ab9022956d18a4ec81e647be84d1db46.png)

**推荐阅读**

+   • 对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO

+   • 2024：ToB、Agent、多模态

+   • [RAG全景图：从RAG启蒙到高级RAG之3](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)6技，再到终章Agentic RAG！

+   • [A](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)gent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）