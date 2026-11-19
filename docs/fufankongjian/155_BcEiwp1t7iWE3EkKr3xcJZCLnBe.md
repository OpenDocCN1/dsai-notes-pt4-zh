
# GraphRAG（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Klo2d2vaqoLmhExDRf5cyprvn2g](https://kq4b3vgg5b.feishu.cn/docx/Klo2d2vaqoLmhExDRf5cyprvn2g)

## 框架介绍

GraphRAG 是一种基于图的检索增强生成（RAG）框架，由 Microsoft 于 2024 年 10 月 15 日首次公开，旨在通过知识图谱提升大型语言模型（LLM）的生成能力。截至 2025 年 2 月 27 日，GraphRAG 被广泛认为是 RAG 领域的创新，特别适合处理复杂关系查询和长文档。

其核心理念是将传统文档转换为图结构，利用图查询和 LLM 结合生成更准确、上下文相关的回答。GraphRAG 适用于需要深度关系分析的领域，如金融（风险评估）、医疗（疾病与治疗关系）、法律（法规关联性分析）。它也支持长文档处理，特别适合企业知识管理。GraphRAG 与传统 RAG 比较：

| 特性 | GraphRAG | 传统 RAG |
| --- | --- | --- |
| 数据结构 | 知识图谱（节点、边） | 向量数据库（嵌入向量） |
| 检索方式 | 图查询（遍历、社区检测） | 相似性搜索（余弦相似度） |
| 适合场景 | 复杂关系查询、长文档 | 简单关键词匹配、短文档 |
| 多模态支持 | 是（文本、图像、表格） | 有限（主要文本） |
| 计算成本 | 高（图构建和查询） | 低（嵌入生成和搜索） |

GraphRAG 是一种创新的 RAG 框架，通过知识图谱提升生成内容的准确性和相关性，特别适合复杂关系查询和长文档处理。其多模态支持和开源性为开发者提供了广阔的定制空间，尽管计算成本较高，未来有望在多种工业领域发挥更大作用。

> **关键引用：**

+ [GraphRAG GitHub](https://github.com/microsoft/graphrag)

+ [GraphRAG: Democratizing Data-Driven Decision Making with Knowledge Graphs](https://arxiv.org/abs/2410.11820)

+ [Microsoft AI Blog: Introducing GraphRAG](https://blogs.microsoft.com/ai/graphrag-announcement/)

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️ 入门介绍

+ 零基础手撕GraphRAG基本原理（视频）

### ☑️ 部署调用

+ ⭐️微软GraphRAG快速部署与调用方法详解（课件）

+ 零门槛GraphRAG部署与调用方法详解（视频）

+ ⭐️ollama+GraphRAG本地部署（课件）

+ GraphRAG+Ollama本地大模型运行方案详解（视频）

### ☑️ 进阶之路

1. GraphRAG搭建本地知识库

+ GraphRAG的核心原理详解，与传统RAG的区别（视频）

+ [使用GraphRAG在个人电脑上搭建高效本地知识库问答系统全流程（视频）](https://www.bilibili.com/video/BV1wvB4YEEAa/?)

---

## ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/fa641a3aac6c428ba0d6d6be777766ff.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流氛围浓厚，**不定期开设硬**核干货&前沿技术公开课噢**~

---
