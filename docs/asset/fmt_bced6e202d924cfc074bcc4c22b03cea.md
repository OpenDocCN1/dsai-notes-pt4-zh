
+ 微调模型。在这里，您根据自己的数据进一步训练模型，这使得模型更加准确并且能够响应您的需求，但可能成本高昂。

![](img/a14cdbef119d723db017ff604d7ecc0e.png)

*图片来源: 企业部署 LLM 的四种方式 | Fiddler AI 博客*

## [通过上下文的提示工程](https://microsoft.github.io/generative-ai-for-beginners/#/02-exploring-and-comparing-different-llms/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e9%80%9a%e8%bf%87%e4%b8%8a%e4%b8%8b%e6%96%87%e7%9a%84%e6%8f%90%e7%a4%ba%e5%b7%a5%e7%a8%8b)

预先训练的 LLMs 在广义自然语言任务上表现得非常好，甚至可以用简短的提示来调用它们，比如要完成的句子或问题——即所谓的“零样本”学习。

然而，用户越能通过详细的请求和示例（上下文）来构建他们的查询，就越能得到最准确、最接近用户期望的答案。在这种情况下，如果提示仅包含一个示例，我们讨论“单样本”学习；如果提示包含多个示例，我们讨论“少样本学习”。

根据上下文进行提示工程设计是最具成本效益的启动方法。

## [检索增强生成 (RAG)](https://microsoft.github.io/generative-ai-for-beginners/#/02-exploring-and-comparing-different-llms/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e6%a3%80%e7%b4%a2%e5%a2%9e%e5%bc%ba%e7%94%9f%e6%88%90-rag)

[LLM](https://learn.microsoft.com/azure/search/vector-search-overview?WT.mc_id=academic-105485-koreyst) 有一个限制，即它只能使用训练期间使用过的数据来生成答案。这意味着它对训练过程后发生的事情一无所知，并且无法访问非公开信息（例如公司数据）。这可以通过 RAG 来克服，RAG 是一种考虑提示长度限制的技术，以文档块的形式使用外部数据来增强提示。矢量数据库工具（例如 Azure 向量搜索）支持此功能，可从各种预定义数据源的块中检索有用的信息，并将它们添加到提示上下文中。

当企业没有足够的数据、足够的时间或资源来微调 LLMs，但仍希望提高特定工作负载的性能并减少幻觉的风险（即对现实的神秘化或有害的风险）时，此技术非常有用。

## [微调模型](https://microsoft.github.io/generative-ai-for-beginners/#/02-exploring-and-comparing-different-llms/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e5%be%ae%e8%b0%83%e6%a8%a1%e5%9e%8b)

微调是一个利用迁移学习使模型“适应”下游任务或解决特定问题的过程。与少样本学习和 RAG 不同，它会生成一个新模型，并更新权重和偏差。它需要一组训练示例，其中包含单个输入（提示）及其关联的输出（完成）。

如果出现以下情况，这将是首选方法：

+ 使用微调模型。企业希望使用经过微调的、能力较差的模型（例如嵌入模型）而不是高性能模型，从而获得更具成本效益和快速的解决方案。
+ 考虑延迟。延迟对于特定用例很重要，因此不可能使用很长的提示，或者应该从模型中学习的示例数量不符合提示长度限制。
+ 保持最新状态。企业拥有大量高质量的数据和真实标签，以及随着时间的推移保持这些数据最新所需的资源。

## [训练垂直行业模型](https://microsoft.github.io/generative-ai-for-beginners/#/02-exploring-and-comparing-different-llms/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e8%ae%ad%e7%bb%83%e5%9e%82%e7%9b%b4%e8%a1%8c%e4%b8%9a%e6%a8%a1%e5%9e%8b)

从头开始培训 LLMs 无疑是最困难、最复杂的方法，需要大量数据、熟练资源和适当的计算能力。仅在企业具有特定领域的用例和大量以特定领域为中心的数据的情况下才应考虑此选项。

## [知识检查](https://microsoft.github.io/generative-ai-for-beginners/#/02-exploring-and-comparing-different-llms/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e7%9f%a5%e8%af%86%e6%a3%80%e6%9f%a5)

提升 LLMs 输出效率最好的方法是什么？

1. 根据背景提示进行工程设计
2. RAG
3. 模型微调

A：3，如果您有时间和资源以及高质量的数据，微调是保持最新状态的更好选择。然而，如果您正在寻求改进，但又缺乏时间，那么值得首先考虑 RAG。

## [🚀 知识拓展](https://microsoft.github.io/generative-ai-for-beginners/#/02-exploring-and-comparing-different-llms/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%f0%9f%9a%80-%e7%9f%a5%e8%af%86%e6%8b%93%e5%b1%95)

详细了解如何为您的业务使用 RAG。

## [继续您的学习旅程](https://microsoft.github.io/generative-ai-for-beginners/#/02-exploring-and-comparing-different-llms/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e7%bb%a7%e7%bb%ad%e6%82%a8%e7%9a%84%e5%ad%a6%e4%b9%a0%e6%97%85%e7%a8%8b)

想要了解更多关于不同的生成人工智能概念吗？转至进阶学习的页面，查找有关本章的其他重要资源。

前往第三章，我们将了解如何负责任地使用生成式 AI 进行应用构建！

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
