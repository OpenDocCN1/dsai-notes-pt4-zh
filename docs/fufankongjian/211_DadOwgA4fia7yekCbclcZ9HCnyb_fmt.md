
# Dify（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/NvCrdNC84oG0DCxg4dwcXPpYn0X](https://kq4b3vgg5b.feishu.cn/docx/NvCrdNC84oG0DCxg4dwcXPpYn0X)

> 从专业 LLM 开发者的角度来看，Dify 是一个基础平台，标准化了 AI 应用开发，提供可扩展、安全和高效的解决方案。其开源性质和不断增长的生态系统，使其成为构建下一代 AI 应用的理想工具。

## 框架介绍

Dify 是一个开源的 AI 应用开发平台，核心目标是解决 LLM 应用开发的复杂性问题。该平台于 2023 年初发布，已迅速成为 GitHub 上热门项目之一（截至 2025 年，支持超过 180,000 名开发者）。它支持 Python 和其他语言的集成，适用于构建聊天机器人、AI 代理、工作流和知识管理系统。

Dify 采用模块化、开源架构，构建于 Docker 和 Kubernetes 等容器化技术之上，支持云端和自托管部署，用户可以通过拖拽界面构建应用。其核心是一个可视化低代码平台，集成 LLM、RAG 和代理框架。

+ 其关键组件包括：

+ **应用构建界面**：拖拽式编辑器，用于创建 AI 应用。支持提示模板（Prompt Templates）、变量管理和少样本学习（few-shot learning）。

+ **RAG 引擎**：高品质的检索增强生成系统，支持文档加载、嵌入模型（如 Hugging Face）和向量存储（如 Milvus、Pinecone）。它处理知识管道，将杂乱数据转换为 LLM 上下文。

+ **AI 代理框架**：灵活的代理设计，支持工具集成（如 API 调用、数据库查询）和多代理协作。代理可以执行复杂任务，如 RPA（机器人流程自动化）。

+ **工作流引擎**：低代码工作流，支持条件分支、循环和并行执行。基于 LangGraph 风格的图形化设计，允许自定义节点。

+ **API 和集成**：RESTful API 用于将提示与业务逻辑分离，支持与现有应用集成。提供 SDK for Python 和 JavaScript。

+ **监控与管理**：通过 LangSmith-like 工具监控调用、成本和使用情况。支持数据加密和访问控制。

> Dify 的架构强调可扩展性，支持水平扩展和多租户模式。底层使用 PostgreSQL 或 MySQL 作为数据库，Redis 作为缓存，并集成主流 LLM 提供商。

> **关键引用：**

+ [Dify官方文档](https://docs.dify.ai/)

+ [Dify Github仓库](https://github.com/langgenius/dify)

+ [Dify功能与入门指南](https://www.cometapi.com/what-is-deify-features-start-with-dify/)

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### Dify快速入门

⭐️ Dify快速入门与聊天助手搭建流程（课件）

### Dify快速实战

⭐️ Dify文本生成应用与Agent开发指南（课件）

### Dify企业级实战

#### 1、ChatFlow开发

⭐️ [Dify工作流与ChatFlow开发指南（课件）](https://kq4b3vgg5b.feishu.cn/docx/TVVXdN3XmoV1PQxPuUwc6O6Tnaf?from=from_copylink)

+ 0代码开发工业级Agent，Dify快速入门与开发实战！（视频）

#### 2、长文本生成

⭐️ Dify_2.0_智能Agent长文本生成实战【课件】

+ [Dify 工作流实现企业级长文本生成！基于 DeepSeek / Qwen 构建 Agentic Writing Engine，报告、公文、小说多领域一键生成！【视频】](https://www.bilibili.com/video/BV1kpsgzyEja)

#### 3、DeepResearch Agent

⭐️ [（Dify + Tavily）基于DeepResearch Agent构建企业级自动化调研系统【课件】](https://kq4b3vgg5b.feishu.cn/wiki/YhJswHLdHiQi44kY7jLcnAlVn0f?from=from_copylink)

+ [Dify + Tavily 构建 DeepResearch Agent](https://www.bilibili.com/video/BV1tjs2zsEYq)！企业级自动化调研系统全流程实战！多源检索、溯源脚注、可信报告一键生成！【视频】

---

## ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/499ec01ac9a048c0fc0bc34dae05376d.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流氛围浓厚，**不定期开设硬**核干货&前沿技术公开课噢**~

---
