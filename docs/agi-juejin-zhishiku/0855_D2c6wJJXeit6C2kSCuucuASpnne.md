# 生成式AI设计模式：综合指南

> 来源：[https://agijuejin.feishu.cn/docx/XjiTdIjELov7GsxzrLmcYL5bnmg](https://agijuejin.feishu.cn/docx/XjiTdIjELov7GsxzrLmcYL5bnmg)

> 作者：lucas大叔

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/688708886)hihu.com/p/688708...

在构[建新任务时，](https://www.zhihu.com/search?q=%E7%94%9F%E6%88%90%E5%BC%8FAI&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)我们会诉诸过往经验，使用一些已经尝试和测试过的方法和模式。对于软件工程领域的人来说，这是一种非常正确的手段和解决问题的方式。但对于生成式AI和AI本身来说，情况可能并非如此。随着 生成式AI 等新兴技术的出现，我们缺乏完善的模式来支撑相应的解决方案。

基于对生产中LLM的工程实现的评估，本文分享一些生成式AI的方法和模式。这些模式的目标是帮助缓解和客服生成式AI实现的一些挑战，例如成本、延迟和幻觉。

## 1. 用于Fine-Tuning的分层缓存策略

![](img/80a4ed1a04e91ce88555adbc22ead536.png)

当向LLM引入 [缓存策略 ](https://www.zhihu.com/search?q=%E7%BC%93%E5%AD%98%E7%AD%96%E7%95%A5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)和服务时，旨在解决成本、冗余和训练数据等因素的组合问题。

通过缓存这些初始结果，系统可以在后续查询中更快地提供答案，从而提高效率。一旦我们有了足够的数据，就可以用这些早期交互的反馈来微调完善更专业的小模型。

专业小模型不仅可以提升流程效率，还能将AI的专家经验注入到特定任务中，使其在精度和适应性要求较高的场景更加高效，例如客户服务或个性化内容创建。

[基于一些预](https://www.zhihu.com/search?q=%E5%BF%AB%E9%80%9F%E5%90%AF%E5%8A%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)构建的服务（例如GPTC[ache](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)）可以 快速启动 ，也可以使用常见的缓存 数据库 （例如Redis、Apache Cassandra、Memcached）打造自己的服务。在向pipeline增加其他的同时，请关注和测量延迟情况。

## 2. [多路复用 ](https://www.zhihu.com/search?q=%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)AI Agent

![](img/dba0378583e33e5fce20ded033f4c329.png)

设想在一个生态系统中有多个面向特定任务的生成式AI模型（即“agent”），每个模型都是其所在领域的专家，大家并行工作解决query。这种多路复用的策略得到了一组多样性的响应，将这些响应集成整合可以提供全面的答案。

对于问题的不同方面需要不同的专业知识才能解决的复杂问题场景，这不失为一种理想的解决方案。类似于一个专家团队，每个人处理更大问题的一个方面。

## 3. 为多个任务微调LLM

![](img/af6225878003d1480049e6b35fe18772.png)

同时[对多个任务](https://www.zhihu.com/search?q=%E7%A0%94%E7%A9%B6%E5%B7%A5%E5%85%B7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)而不是单个任务微调LLM，这是一种促进知[识和技能跨不同领域迁](https://link.zhihu.com/?target=https%3A//github.com/microsoft/DeepSpeed)移的方法，增强了模型的多功能性。这种多任务学习对于需要高度胜任各种任务的平台尤其有用，例如虚拟助手或AI赋能的 研究工具 ，会简化复杂领域的训练和测试工作流。训练LLM的资源和packages请阅读： DeepSpeed 和 Hugging Face’s Transformer library .

## 4. 基于规则和生成式的融合

![](img/6282d87dc95f4e60008ef916ceaf3a5b.png)

许多现有的业务系统和 [企业应用 ](https://www.zhihu.com/search?q=%E4%BC%81%E4%B8%9A%E5%BA%94%E7%94%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)在某种程度上仍然是基于规则的。将生成式与基于规则逻辑的结构化精准输出融合，可产生既有创造性又符合要求的解决方案。对于输出必须遵守严格标准或法规的行业来说，这是一项很有效的策略，确保AI保持在要求参数的范围内，同时仍能生成富有创意和吸引力的内容。一个很好的例子是为电话IVR系统或基于规则的传统（非LLM）聊天机器人生成意图和消息流。

## 5. 利用知识图谱赋能LLM

![](img/161ba3e456bcf4779e683fbe83c88dc7.png)

将 [知识图谱 ](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%9B%BE%E8%B0%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)与生成式AI模型相结合，赋予它们以事实为导向的超强能力，使其输出不仅感知上下文内容，而且更符合事实。这种方法对于真实性和准确性不容臆造的应用至关重要，例如在教育内容创作、医疗建议或任何错误信息可能产生严重后果的领域。

知识图谱和图谱本体允许将复杂的主题或组织结构的问题分解为[结构化格式，](https://www.zhihu.com/search?q=%E5%A4%A7%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)以帮助建立具有深度上下文的 大语言模型 。此外还可以使用语言模型生成JSON或RDF等格式的本体。

可以用于知识图谱的服务包括 [图数据库 ](https://www.zhihu.com/search?q=%E5%9B%BE%E6%95%B0%E6%8D%AE%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)服务，如ArangoDB、Amazon Neptune、Azure Cosmos DB和Neo4j。还有更广泛的数据集和服务用于访问更广泛的知识图谱，包括Google Enterprise知识图谱API、PyKEEN数据集和Wikidata。

## 6. AI Agent [集群](https://www.zhihu.com/search?q=%E9%9B%86%E7%BE%A4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)

![](img/11c3bafb7594e02f89835cadcc6f2a39.png)

该模型从自然蜂群中汲取灵感，采用大量AI Agent共同解决一个问题，每个agent都有独特的视角。得到的综合输出反映了集体智慧，超过了任何单个agent所能实现的。这种模式在需要广泛的创造性解决方案或者处理复杂数据集时有优势。

## 7. 可组合的模块化LLM方法

![](img/c961e022072d9d0e1fe95e305067c772.png)

这种设计倡导适应性，它的特点是模块化AI系统可以动态重新配置自身，以实现最佳任务性能。这类似于拥有一把瑞士军刀，每个模块都可以根据需要进行选择和激活，使其对需要针对不同客户交互或产品需求量身定制解决方案的企业非常有效。

可以部署使用各种自主agent框架和架构来开发每个agent及其工具。常用的agent框架包括CrewAI、Langchain、Microsoft Autogen和SuperAGI。

## 8. LLM的记忆认知方法

![](img/cbfcb038c66e8fbcb9d49b86d173c27e.png)

这种方法将类人记忆的元素引入AI，允许模型回忆并在以前的交互基础上创建，以获得更细微的响应。

它对正在进行的对话或学习场景特别有用，因为随着时间的推移，AI会发展出更深刻的理解，就像专用的个人助理或自适应学习平台一样。记忆认知方法可以通过总结关键事件和讨论并将其存储到向量数据库中来逐渐发展演化。

为了使摘要的计算量较低，当需要处理大量内容时，可以通过较小的NLP库（如spaCy或B[ART语言](https://www.zhihu.com/search?q=%E7%9F%AD%E6%9C%9F%E8%AE%B0%E5%BF%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)模型）来生成摘要。使用向量数据库时，在[prompt阶](https://link.zhihu.com/?target=https%3A//memgpt.readme.io/docs/index)段利用相似度搜索 短期记忆 来定位关键的“事实”。对解决方案感兴趣的同学，可以阅读类似模式的开源解决方案 MemGPT 。

## 9. 红蓝团队双模式评估

![](img/e75d5c330f88671df6bb511cc2840b4c.png)

在红蓝团队 [评估模型 ](https://www.zhihu.com/search?q=%E8%AF%84%E4%BC%B0%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)中，一个AI生成内容，而另一个则对其进行批判性评估，类似于严格的同行评审过程。这种双模型设置非常适合质量控制，使其高度适用于可信度和准确性至关重要的内容生成平台，如新闻聚合或教育素材制作。

这种方法可以用微调模型代替复杂任务的部分人工反馈，以模拟人工审查过程并更新结果，从而评估复杂的语言场景和输出。

## 写在最后

本文 [浅尝辄止 ](https://www.zhihu.com/search?q=%E6%B5%85%E5%B0%9D%E8%BE%84%E6%AD%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)地探讨了生成式AI的九种 [设计模式 ](https://www.zhihu.com/search?q=%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A688708886%7D)，[为LLM落](https://link.zhihu.com/?target=https%3A//medium.com/towards-data-science/generative-ai-design-patterns-a-comprehensive-guide-41425a40d7d0)地实践提供了方法上的指导，具有一定的借鉴意义。由于能力有限，难免有理解错误或不到位的地方，感兴趣的同学可 阅读原文 或者在评论区指正、讨论！