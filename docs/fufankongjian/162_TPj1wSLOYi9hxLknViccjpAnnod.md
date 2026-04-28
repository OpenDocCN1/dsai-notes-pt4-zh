# RAG（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/BEsKd90EXoJDdLxNXtXcQW3ZnNe](https://kq4b3vgg5b.feishu.cn/docx/BEsKd90EXoJDdLxNXtXcQW3ZnNe)

# 技术介绍

检索增强生成（RAG）是一种结合检索和生成能力的AI模型架构，旨在提升自然语言处理任务的准确性和上下文相关性。它由两部分组成：检索器从外部知识库中检索相关文档，生成器基于这些文档和用户查询生成回答。研究表明，RAG通过减少生成模型的“幻觉”（hallucination，即生成不准确内容）问题，特别适合需要大量领域知识的任务，如问答系统、知识库对话和文档生成。以下是本地搭建和企业级项目对比表：

| **方面** | **本地搭建** | **企业级项目** |
| --- | --- | --- |
| **硬件需求** | 16GB RAM，建议GPU | 高性能服务器，分布式计算 |
| **知识库规模** | 小规模，数百至千份文档 | 大规模，数万至百万份文档 |
| **部署方式** | 本地运行，单机环境 | Docker+Kubernetes，分布式部署 |
| **安全性** | 基本，无特殊认证 | OAuth2，数据加密，访问控制 |
| **性能优化** | 简单调整，FAISS索引 | HNSW算法，缓存机制，负载均衡 |

根据[N](https://blogs.nvidia.com/blog/what-is-retrieval-augmented-generation/)VIDIA Blogs: What I[s Retrieval Augmented Generat](https://aws.amazon.com/what-is/retrieval-augmented-generation/)ion (RAG)，RAG通过从特定数据源获取事实信息，增强生成AI模型的准确性和可靠性。同样，AWS: What is Retrieval Augmented Generation (RAG)指出，RAG无需重新训练模型即可扩展LLM到特定领域或组织内部知识库，成本效益高。

RAG技术通过检索与生成的协同工作，为开发者提供了构建智能、知识驱动型应用的强大工具。本地搭建流程简单高效，适合快速原型开发；而企业级项目则需关注架构设计、性能优化和安全性。研究显示，随着向量搜索和生成模型的进步，RAG在未来的应用潜力将进一步扩大。

> **关键引用：**

+   [N](https://blogs.nvidia.com/blog/what-is-retrieval-augmented-generation/)VIDIA Blogs: What Is Retrieval Augmented Generation (RAG)

+   [AWS: What is Retrieval Augmen](https://aws.amazon.com/what-is/retrieval-augmented-generation/)ted Generation (RAG)

+   [I](https://research.ibm.com/blog/retrieval-augmented-generation-RAG)BM Research: What is retrieval-augmented generation (RAG)?

+   [Wikipedia: Retrieval-](https://en.wikipedia.org/wiki/Retrieval-augmented_generation)augmented generation

+   [Cohesity: What is retrieval a](https://www.cohesity.com/glossary/retrieval-augmented-generation-rag/)ugmented generation?

+   [G](https://www.geeksforgeeks.org/what-is-retrieval-augmented-generation-rag/)eeksforGeeks: What is Retrieval Augmented Generation (RAG)?

+   [Pinecone: Retrieval Augmen](https://www.pinecone.io/learn/retrieval-augmented-generation/)ted Generation (RAG)

+   [E](https://www.elastic.co/what-is/retrieval-augmented-generation)lastic: What is Retrieval Augmented Generation (RAG)?

+   [D](https://www.datacamp.com/blog/what-is-retrieval-augmented-generation-rag)ataCamp: What is Retrieval Augmented Generation (RAG)?

+   [P](https://www.promptingguide.ai/techniques/rag)rompt Engineering Guide: Retrieval Augmented Generation (RAG)

# 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️** 入门介绍**

+   大模型RAG技术快速入门介绍（视频）

+   LangChain AI 完整开源生态速览（视频）

### **☑️ **进阶之路

**1、本地RAG搭建流程**

⭐️从零到一搭建一个小型RAG系统（课件）

+   OpenAI Embedding模型快速入门实践（视频）

+   从零到一手动搭建RAG引擎（视频）

2、企业级RAG项目（持续更新中）

⭐️Part 1. 快速理解RAG整体框架，掌握高效学习路径

+   企业级RAG项目整体介绍（视频）

+   [大模型问答功能链路设计（视频）](https://www.bilibili.com/video/BV1LhzmYvEmy/?p=3)

+   接入GLM模型构建问答功能（视频）

⭐️ Part 2.1小时速通LangChain（课件）

---

# ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/8bba36cabd832a42ed3cb10f6cef41a4.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流氛围浓厚，**不定期开设硬**核干货&前沿技术公开课噢**~

---