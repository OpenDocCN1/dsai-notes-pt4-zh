# OpenAI是如何将RAG的准确率优化至98%的

> 来源：[https://agijuejin.feishu.cn/docx/ZNFudFoMJoYJzmxlcAHcA4r5nfb](https://agijuejin.feishu.cn/docx/ZNFudFoMJoYJzmxlcAHcA4r5nfb)

> 作者：王大锤

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/704291371)hihu.com/p/704291...

## 一、前言

这篇文章介绍一下 [OpenAI ](https://www.zhihu.com/search?q=OpenAI&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)近期公布的其团队在RAG领域的优化工作，探索了OpenAI在RAG领域的一些优化实践。

> 需求: 用户有大量的数据文档(这个数据量一般比较大，比如超过10万份)，用户期望大模型只基于这些文档进行知识检索。

**解决方案 **:

1.  直接将PDF和docx文件进行嵌入，RAG整体的准确率为45%。

1.  经过20次调优迭代，解决细节问题和修复少量bug，准确率提升至65%。

1.  [基于规则进](https://www.zhihu.com/search?q=%E9%A2%86%E5%9F%9F%E8%AF%86%E5%88%AB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)行优化，比如将业务进行 领域识别 ，然后基于该领域的知识库进行检索回答，准确率提升至85%。

1.  发现数据里有一些是 [结构化数据 ](https://www.zhihu.com/search?q=%E7%BB%93%E6%9E%84%E5%8C%96%E6%95%B0%E6%8D%AE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)(如表格)，为此片段定制提取解决，准确率提升到98%。

![](img/373105d1a4ab2706ea8a6fda650e8f02.png)

---

## 二、相关工作

### 2.1 优化 [LLM ](https://www.zhihu.com/search?q=LLM&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)性能的挑战

Colin细致讨论了优化大型语言模型的挑战，包括：

+   [**信号与噪声 **](https://www.zhihu.com/search?q=%E4%BF%A1%E5%8F%B7%E4%B8%8E%E5%99%AA%E5%A3%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)**的分离 **：在大量数据中识别有效信号，滤除干扰噪声。

+   **性能衡量的复杂性 **：确立合理的性能评估标准，确保模型优化方向的正确性。

+   **解决问题的策略选择 **：面对多种可能的解决方案，如何做出最合适的选择。

### 2.2 微调技术的进展

[John Allard ](https://www.zhihu.com/search?q=John%20Allard&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)深入介绍了OpenAI在微调领域的最新进展，包括：

+   **函数调用数据的微调 **：通过特定数据集的微调，优化模型对特定函数调用的理解。

+   **持续微调 **：提供了一种机制，允许模型随着时间的推移不断适应新数据。

+   **平台内 **[**全UI微调 **](https://www.zhihu.com/search?q=%E5%85%A8UI%E5%BE%AE%E8%B0%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)：通过用户界面的直接操作，简化了微调过程，提高了效率。

### 2.3 RAG技术的应用案例分析

[RAG技术通](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E6%95%B0%E6%8D%AE%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)过结合Embeddings技术和 向量数据库 ，显著提升了信息检索的效率和准确性。案例分析包括：

**客户需求1 **:

![](img/3dd32d81363a40e014c9ea90fcbf5e52.png)

+   **问题描述 **：基于大量文档的知识检索需求，要求模型仅从这些文档中检索信息。

+   **解决方案 **：

+   直接将PDF和docx文件嵌入，准确率是45%。

+   2. 经过20次调优迭代，解决细节小Bug - 准确率到65%

+   3. 基于规则进行优化，譬如先判断问题属于什么领域（退一步思考），然后再回答，效果提升到85%

+   4. 发现数据里有一些是结构化数据（如表格），为此定制提取解决，准确率提升到98%

+   **总结： **通过直接嵌入文件、迭代调优、规则优化和结构化数据的定制提取，逐步提升检索准确率。

**客户需求2:**

![](img/a73161e4f97b5b69a2488246218e1e89.png)

+   **解决方案 **:

+   [首先，我们用](https://www.zhihu.com/search?q=%E4%BD%99%E5%BC%A6%E7%9B%B8%E4%BC%BC%E6%80%A7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)最简单的方法，比如 余弦相似性 ，寻找类似问题的SQL查询，进行问题相似性搜索。我们测试了不同的嵌入和提示工程，但结果不太好。后来，我们意识到不同的数据库模式对问题的答案可能完全不同。

+   所以针对一个问题进行相似性搜索意义不大，但用预设答案进行搜索可能效果更好。我们使用预设的文档嵌入，生成一个假设的SQL查询进行相似性搜索，性能大大提升。对于这个特定问题，我们尝试了上下文检索通过简单过滤将问题按难度排名，只带回同等难度的例子，这带来了改进。

+   [我们还尝试](https://www.zhihu.com/search?q=%E9%93%BE%E5%BC%8F%E6%8E%A8%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)了一些先进技术，比如 链式推理 ，让系统识别列、表，最后构建查询。但我们决定采用更简单的方法进行自我一致性检查，让系统构建查询并运行，出错时给出提示，再次尝试。

+   我们从69%开始，然后添加了一些示例，得到了几个改进点，这表明RAG可以进一步改进。我们尝试这个问题，看到性能提升了3%，然后使用答案，假设的文档嵌入使我们进一步提升了5%。

+   我[们决定试试微](https://www.zhihu.com/search?q=GPT-4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)调，看能否更进一步。先建立了我们的基线，与上图里的69%基线一样，使用简单的提示工程技术。然后，用简单的提示工程对 GPT-4 进行微调，结果模型准确率提升到接近82%。

+   再稍微调整模型使用um rag技术，把一些示例动态注入上下文窗口，准确率达到83.5%。这些都很简单，没有复杂的数据预处理或后处理。结果显示简单的微调和提示工程也能接近最先进的技术。

![](img/5b9e0b6c3aec84e0a2e1cdece6c3f804.png)

**总结： **采用问题相似性搜索、预设答案搜索、上下文检索和链式推理等方法，有效提升查询生成的性能。

### **2.4 评估和优化RAG **[**系统的方法**](https://www.zhihu.com/search?q=%E7%B3%BB%E7%BB%9F%E7%9A%84%E6%96%B9%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)

John和Colin评估和优化RAG系统的方法，强调了数据质量的重要性，提出了评估RAG系统的多个维度：

+   **模型测试分数 **：评估模型在特定任务上的表现。

+   [**检索相关性 **](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E7%9B%B8%E5%85%B3%E6%80%A7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)：衡量检索到的内容与问题的相关程度。

+   **生成内容的准确性 **：评估生成答案的事实准确性。

+   **检索内容的信号与噪声比 **：评估检索到的信息的有用性。

![](img/e6d5add34df2b9693b601b69591a3617.png)

### 2.5 实战指导参考流程

[优化LLM](https://www.zhihu.com/search?q=%E6%8F%90%E7%A4%BA%E5%B7%A5%E7%A8%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704291371%7D)性能并不总是线性的，有时需要结合使用 提示工程 、RAG和微调。当你想提高语言模型（LM）的性能时，可以参考如下顺序调优。

+   提示工程技术：作为开始的好地方，技术投资小，能够快速迭代，验证解决问题的可行性。

+   RAG技术：私有化、数据安全考虑时使用，增加允许模型访问特定领域的内容，提升专业知识检索能力。

+   微调技术：还需进一步提升系统准确率，强化模型遵循一致指令的能力，提升特定领域的专业性使用。

+   预训练：成本最高，如果不是特殊要求一般不建议采用。

## **总结：**

这次的分享不仅展示了提高大型语言模型性能的多种技术手段，也强调了在实际应用中根据业务场景灵活选择和组合技术的重要性。通过深入理解和应用这些策略，我们可以更有效地优化AI模型，推动AI技术的发展和应用。