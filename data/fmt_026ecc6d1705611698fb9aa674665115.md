
# 检索增强生成（RAG）和GPT的语义搜索

> 来源：[https://vxc3hj17dym.feishu.cn/docx/ItnadPgXwozm0Tx1g0Tcma0Lnjd](https://vxc3hj17dym.feishu.cn/docx/ItnadPgXwozm0Tx1g0Tcma0Lnjd)

本文为OpenAI官方文档《Retrieval Augmented Generation (RAG) and Semantic Search for GPTs》的中文精校解读

原文地址：[Retrieval Augmented Generation (RAG) and Semantic Search for GPTs | OpenAI Help Center](https://help.openai.com/en/articles/8868588-retrieval-augmented-generation-rag-and-semantic-search-for-gpts#h_6e1ade8025...)

---

## 检索增强生成（RAG）是什么，为什么对GPT构建者很有价值？

RAG是从数据源中检索相关的上下文信息，并将该信息与用户的提示一起传递给大型语言模型的过程。通过增加模型的基础知识，利用这些信息来改进模型的输出（生成的文本或图像）。

![](img/408ceeec0763d1a8083d3c8e5cf0dfcc.png)

Basic RAG workflow 基本的RAG工作流程

RAG对于需要模型获取不包含在其学习信息中的知识的用例非常有价值。例如，假设您正在构建一个GPT来帮助支持团队回答客户的询问。GPT-4能够使用其基本知识推理客户问题，但它无法了解关于您特定产品或服务的最新事实。通过让GPT访问您的工单系统，您可以获得更好的结果，因为它可以检索与类似问题相关的过去工单，并利用该上下文生成更相关的答案。当您在GPT中使用知识检索功能时，RAG会自动为您执行。

## 什么是语义搜索？

语义搜索超越了关键词搜索（依赖于搜索输入中特定索引词的出现），根据输入字符串的概念相似性来查找相关的上下文数据。

因此，对于为GPT-4等模型提供更多上下文的选择是明智的（因为查询很可能与上下文密切相关）。语义搜索使用向量数据库，该数据库存储文本块（从某些文档中提取）及其向量（文本的数学表示）。当您查询向量数据库时，搜索输入（以向量形式）将与所有存储的向量进行比较，并返回最相似的文本块。

### 语义搜索的示例

假设你正在构建一个客户支持聊天机器人。如果你想要从知识库中填充一个向量数据库的文章，你可以：

1. 将每篇文章分成块状

- 这可以是在句子、段落或页面层面上。不同的分块策略会产生不同的结果。

2. 使用`OpenAI嵌入API`处理这些块并返回嵌入（即向量空间中表示块性质的数学表示）。

- 例如：`[-0.006929283495992422，-0.005336422007530928，... -4.547132266452536e-05，-0.024047505110502243]`

3. 将块和它们的嵌入存储在数据库中

当用户向聊天机器人提问时，会进行语义搜索

1. 用户提交了一个查询，例如“我如何使用`OpenAI API`？”
2. 使用`OpenAI Embedding API`生成查询字符串的向量表示
3. 将该向量提交到与我的向量数据库关联的搜索终端
4. 返回一个或多个与我的查询相似的文本块

聊天机器人应用程序随后将文本块与初始用户提示一起提交给`OpenAI Chat Completions API`以获取响应。

对于GPT构建者，您可以通过上传文件并在GPT中启用知识检索来利用开箱即用的语义搜索功能。
