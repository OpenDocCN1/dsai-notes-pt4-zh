# LlamaIndex技术报告：知识助手的尽头是Multi-Agents！

> 来源：[https://agijuejin.feishu.cn/docx/GihydCnFPoKXsyxBhRKcKTEun3e](https://agijuejin.feishu.cn/docx/GihydCnFPoKXsyxBhRKcKTEun3e)

> 作者：PaperAgent

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/uSHGFKpPzdrJjDL3BZVDWw)qq.com/s/uSHGFKpP...

LlamaIndex CEO于 2024.6.26在 AI Engineer上的主题演讲：

+   报告人： Jerry Liu

+   报告人信息： LlamaIndex CEO

+   报告主题：“ 知识助手的尽头是Multi-Agents！ ”

+   主题原文：“ The Future of Knowledge Assistants ”

+   报告概要内容如下：

知识助手（ Knowledge Assistants ）的终极形态是多智能体系统，它通过RAG技术起步，逐渐发展出高级Agent能力，最终实现Multi-Agent协同工作，以提供更高效、智能的服务。 一些关键点：

1.  **知识助手的演进 **：为了超越简单的搜索和问答，知识助手需要发展更高级的数据和检索模块、单智能体查询流程，以及多智能体任务解决者。

1.  **数据的重要性 **：高质量的数据是构建有效知识助手的关键。数据解析、分块和索引等数据处理步骤对于减少错误和提高性能至关重要。

1.  **基础RAG的局限性 **：检索增强生成（RAG）作为知识助手的起点，提供了基础的数据索引和简单的问答功能。然而，它在理解复杂查询、规划任务和使用工具方面存在局限。

1.  **单智能体的高级流程 **：通过引入工具使用、动态规划与执行、对话记忆等元素，单智能体可以处理更复杂的任务，但可能伴随着更高的成本和延迟。

1.  **Agentic RAG **：将数据接口视为工具，利用智能体的推理循环来解决复杂任务，构建能够处理复杂问题的个性化问答系统。

1.  Multi-Agents的优势 ：多智能体系统通过专业化、并行化处理任务，优化成本和延迟，但同时也带来了服务架构和协调的挑战。

1.  Llama Agents的架构 ：将每个智能体视为独立的服务，通过消息队列进行通信，并通过控制平面进行编排，实现了封装性、模块性和标准化API接口。

1.  Llama Agents的推出 ：Llama Agents作为微服务的智能体，提供了易于部署、可扩展性和资源管理的优势，并且已经以Alpha预览模式发布。

1.  **LlamaCloud的开放 **：LlamaCloud作为一个提供高级非结构化数据处理的平台，为LLMs提供了更强大的支持，并且已经开放了等待名单。

1.  **未来展望 **：知识助手的未来将朝着更智能、更灵活、更高效的方向发展，通过单智能体和多智能体的协同工作，能够解决更广泛的任务和问题。

**演讲报告详情：**

![](img/e0a43a664bb345df71918768d904a5d9.png)

![](img/36d97ced924d055fe8d55d6b680c9e83.png)

![](img/735c1641d59ab4b75d75afca4e0ae3f2.png)

![](img/134f644c0f0a9497f6ba2e9f45135259.png)

![](img/234d9cab7db3f0ba36c57fd43e925841.png)

![](img/817e53446e61b1a0b25857c11e274113.png)

![](img/5abff8ce7986fb263792e0f58a0b15db.png)

![](img/f225d0d074e70add82e0df7360c3a9d1.png)

![](img/a87f3ff12c7e4a1b9ae269bb2bca979d.png)

![](img/4ebbabc6623aadef586d09d98519920f.png)

![](img/c6c4346a52647b1ced2fb746549df6ed.png)

![](img/12b1bf39aed42912dffe353f5aefc0fa.png)

![](img/2a1d4ca417500354a68efea32874bbbd.png)

![](img/b0c3b993c7664fb66b0b37cf4fe4d665.png)

![](img/c5b87df4bd73215bd6e863f2c7ef4f53.png)

![](img/82433b020210ed5fe2e6e97e8f3c40c8.png)

![](img/a0b7a2ca9af5762d0ca9b64e2adc75e9.png)

![](img/a4a0fe398634d3ad129650c50d597fca.png)

![](img/6aa0fb85e13abd5f537c001cba54960e.png)

![](img/e7681809a31abc4e88e7a944f34c4a92.png)

![](img/8dd1f6bc5376d12262e896e3aea2cb7c.png)

![](img/4db56eac5c9e5d8b7770a0202ccd7e43.png)

![](img/fec73b0d4417f64cb4b90a87529f29fa.png)

![](img/bf22459eb18a81b7dbbe1b3f5e622feb.png)

![](img/3c017c44a2a5ab8a359e549e53b76b4d.png)

![](img/b02d11aa1d757609fb37acaecb822190.png)

![](img/872d5f984f8793ec8a0c69497a6468f6.png)

**推荐阅读**

+   • 对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO

+   • 2024：ToB、Agent、多模态

+   • [RAG全景图：从RAG启蒙到高级RAG之3](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)6技，再到终章Agentic RAG！

+   • [A](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)gent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）

---

欢迎关注我的公众号“ **PaperAgent **”， 每天一篇大模型（LLM）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。