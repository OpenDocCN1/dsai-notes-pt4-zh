
+ [检索和评分相似性](https://www.zhihu.com/search?q=%E7%9B%B8%E4%BC%BC%E6%80%A7%E5%88%86%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)：进行查询时，将检索多个对象及其相似性分数。
+ 识别和切断：使用相似性分数，确定分数显著下降的截止点。超过此点的对象被认为相关性较低，并自动排除。

假设检索到六个对象，相似性得分可能在第四个对象之后急剧下降。通过检查相似性得分的变化率，可以确定要排除哪些对象，确保仅将最相关的信息提供给LLM。

![](img/f02a273f77a8d0f27fd6e556e158ce60.png)

## 重排序检索到的对象

重排序使用辅助的、更复杂的模型，根据最初检索到的对象与查询的相关性对其进行重新评估和重排序。这个过程增强了最终检索到的集合的质量。以下是它的工作原理：

+ 过度召回：最初检索的对象比需要的多。
+ 应用`ranker`模型：使用高延迟、重量级模型，通常是交叉编码器，重新评估每个检索到的对象的相关性。该模型成对考虑查询和每个检索到的对象，以重新评估它们的相似性。
+ 重排序结果：基于新的评估，重排序对象，以在顶部反映最相关的结果。这种方法确保最相关的文档被优先排序，提高了提供给LLM的数据的整体质量。

![](img/5b0c1d620df8eaceb779e01dfe75980f.png)

## 微调LLM

对特定领域数据微调LLM可以显著提高其在该领域内的性能。例如，使用像`Meditron70B`这样的模型，它是`LLaMA2 70b`专门针对医学数据的微调版本，使用：

+ 无监督微调：在大量特定领域文本（例如PubMed文献）上继续预训练模型。
+ [监督微调](https://www.zhihu.com/search?q=%E7%9B%91%E7%9D%A3%E5%BE%AE%E8%B0%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)：在特定领域任务（例如医学选择题）上使用监督学习进一步完善模型。[这种专门的训练有](https://www.zhihu.com/search?q=GPT-3.5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)助于模型在目标领域中表现异常出色，在特定任务中优于其基本模型和`GPT-3.5`等更大、不太专业的模型。

## 总结

[改进检索-增强](https://www.zhihu.com/search?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E5%BA%94%E7%94%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)生成（RAG）系统中的检索和生成对于提高 人工智能应用 的性能和可靠性至关重要。上面讨论的技术范围从低投资、高回报的方法（如查询重写和重排序）到更密集的过程（如嵌入和LLM微调）。技术的选择取决于应用的具体要求和约束。

## 参考文档

[Advanced RAG Techniques | Weaviate - Vector Database](https://link.zhihu.com/?target=https%3A//weaviate.io/blog/advanced-rag)
