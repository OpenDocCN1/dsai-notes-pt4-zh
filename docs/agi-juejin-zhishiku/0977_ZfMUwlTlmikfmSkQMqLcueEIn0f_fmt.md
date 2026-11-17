
# 高级RAG技巧

> 来源：[https://agijuejin.feishu.cn/docx/SMhYdfAl5oHPyKx0V0pcf6L5niJ](https://agijuejin.feishu.cn/docx/SMhYdfAl5oHPyKx0V0pcf6L5niJ)

> 作者：lucas大叔

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/711988265)hihu.com/p/711988...

本文以构建聊天机器人模拟或协助人类医生为例，探讨RAG流程每个部分的实用技巧。

医生经过十余年的学习和临床实践，拥有了庞大、复杂的知识库。在诊断患者时，他们会从这些丰富的经验中获取相关信息，提供准确且有针对性的建议。要使用RAG创建模拟该过程的聊天机器人，需要复刻人类医生的几个关键能力。

[首先，建](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)立相当于医生数十年学习和患者互动的 知识库 。该知识库支持高效搜索，根据患者当前的问题仅检索最相关的信息。

其次，必须能够对检索到的信息进行推理，就像医生辨别特定病例的相关细节一样，诊断结果要可解释。

另外，整个流程必须是实时的。聊天机器人处理患者的问询，检索和推理相关知识，并及时提供可行的建议或诊断结果。

最简单的方法是将患者信息输入语言模型，然后该模型根据预先训练的[医疗数据生](https://www.zhihu.com/search?q=%E5%8C%BB%E5%AD%A6%E6%96%87%E7%8C%AE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)成响应。增强功能包括根据专业 [医学文献 对模](https://www.zhihu.com/search?q=%E6%8F%90%E7%A4%BA%E5%B7%A5%E7%A8%8B%E6%8A%80%E6%9C%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)型进行微调，并采用先进的 提示工程技术 来提高性能。

[然而，更复](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E7%B3%BB%E7%BB%9F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)杂的 RAG 方法涉及将综合知识库（例如病史和文献）集成到 检索系统 中。通过索引和查询庞大的数据库，我们可以为语言模型提供精确、相关的信息，以增强其生成能力。

![](img/2f93eed987245760060454c93483170a.png)

以下是简单的 RAG 框架，为了方便理解，将其分解为三个步骤：索引、检索和生成。

![](img/f3936a075a2b61308f974cf01ba4a5bb.png)

本文将探索 RAG [堆栈 ](https://www.zhihu.com/search?q=%E5%A0%86%E6%A0%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)的每个组件，并讨论如何使用开源库和资源来改善和增强它们。无论是开发用于医疗保健还是其他应用的聊天机器人，这些技术都是普遍适用的，并且将有助于改善大多数 RAG 设置！

## 索引

有[效的索引对](https://www.zhihu.com/search?q=%E5%88%86%E5%9D%97%E6%96%87%E6%9C%AC&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)于任何RAG系统都至关重要。第一步涉及如何摄取、分块和存储数据。让我们探索将数据索引到数据库中的技术，重点关注 分块文本 和利用元数据的各种方法。

### [文本分块](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E5%88%86%E5%9D%97&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)

1.  简单分块

+   概述：将 [文档 ](https://www.zhihu.com/search?q=%E6%96%87%E6%A1%A3&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)分解为具有一些重叠的固定大小的块。这确保了每个块都保留来自前一个块的上下文。

+   实现：可以指定块大小（例如，35个字符）和重叠大小，以维护跨块的上下文。这种方法适用于简单的文档，但可能不适合更复杂的文本，并且根本没有利用文档结构。

+   优缺点：实现简单，但可能会破坏语义单元，导致块不连贯，检索性能较差。

![](img/c66342143f7333fa36cf0c9c9558170a.png)

2. [语义分块](https://www.zhihu.com/search?q=%E8%AF%AD%E4%B9%89%E5%88%86%E5%9D%97&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)

+   概述：根据含义而不是固定大小划分文本。这种技术确保每个块代表一条连贯的信息。

+   实现：计算句子嵌入之间的 [余弦距离 ](https://www.zhihu.com/search?q=%E4%BD%99%E5%BC%A6%E8%B7%9D%E7%A6%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)。如果两个句子在语义上相似（低于某个阈值），它们属于同一个块。这会根据内容的语义结构创建可变长度的块。

+   优缺点：产生更连贯和上下文有意义的块，提高检索准确性，但需要前向通过基于`BERT`的编码器模型，该模型比简单分块计算量更大。

![](img/b70717df8254115f837717cd1da6c41c.png)

3. 基于语言模型的分块

+   概述：用语言模型从文本中创建自包含的命题，确保每个块在语义上是完整的。

+   实现：语言模型（例如，70亿个参数模型）处理文本，将其分解为独立有意义的命题。然后，模型将这些命题组合成块，平衡语义完整性和上下文。这种方法是计算密集型的，但提供了最高的准确性。

+   优缺点：适应文本细微差别，并产生高质量的块，但计算密集，可能需要针对特定应用进行微调。

![](img/8e646caedf390bca7f795a39fb7ae934.png)

### 元数据使用

1. 基于元数据的添加和过滤：

+   概述：元数据提供了额外的上下文，可以显著提高检索准确性。通过合并日期、患者年龄和先决条件等元数据，可以在搜索期间过滤不相关的信息。过滤有助于缩小搜索空间，使检索过程更加高效和相关。

+   实现：索引数据时，将元数据与向量化文本一起存储。例如，如果索引患者记录，则包括年龄、就诊日期和特定条件等元数据。使用此元数据过滤搜索结果，确保系统只检索相关信息。

例如，如果查询与儿科患者有关，则在执行语义搜索之前过滤掉18岁以上患者的记录。这减少了检索结果中的噪声并增强了输出的相关性。

![](img/9cb81ca0ef6d851453fa343e3e86306c.png)

2. 用`GLiNER`为块 [生成元 ](https://www.zhihu.com/search?q=%E7%94%9F%E6%88%90%E5%85%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)数据

并不总是有大量的元数据来增强块，但是用`GLiNER`（Generalized Linear Named Entity Recognizer）模型可以即时从块中生成元数据！`GLiNER`是强大的基于`BERT`的模型，可用于在摄取阶段标记块，即时创建元数据，显著改善检索过程。

+   使用`GLiNER`自动打标签

**实现**：将每个块与要在块中识别的标签一起提供给`GLiNER`。如果在块[文本中找](https://www.zhihu.com/search?q=%E7%BD%AE%E4%BF%A1%E5%BA%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)到标签，将识别它们，如果没有高 置信度 匹配，则不会生成标签，从而减少误报！

**优缺点**：对于一般用途非常有效，但可能需要对特定领域的数据集和应用进行微调。

+   [查询解析 ](https://www.zhihu.com/search?q=%E6%9F%A5%E8%AF%A2%E8%A7%A3%E6%9E%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)和过滤

**实现**：`GLiNER`的另一个用途是解析传入查询，将其与标记的元数据匹配过滤结果。

**优缺点**：通过确保上下文相关性来提高检索准确性，但增加了额外的处理步骤。

![](img/6d56f1770ec9de597e3c966dc19f47af.png)

这些技术为强大的RAG系统奠定了基础，能够从大数据集中进行高效检索。分块方法的选择和元数据的使用将取决于数据集的具体要求和特征。

## 检索

如何从 [向量数据库 ](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E6%95%B0%E6%8D%AE%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)中增强检索召回？这个过程要检索尽可能多的与查询相关的文档，从而显著提高LLM产生高质量结果的机会。以下是实现这一点的几种技术：

### 混合搜索

[向量搜索 ](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E6%90%9C%E7%B4%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)捕获查询的语义含义，关键字搜索识别特定术语的精确匹配。混合搜索结合了向量搜索和关键字搜索的优势，以提高检索准确性。在医学等领域，许多术语和概念在语义上无法理解，而是特定的关键字，如药物名称、解剖学术语、疾病名称和诊断。

纯向量搜索可能会错过这些关键字，而基于关键字的搜索可确保匹配特定的重要术语。通过集成这两种方法，混合搜索可以实现更全面的检索过程。

向量[和关键字](https://www.zhihu.com/search?q=%E5%8F%AC%E5%9B%9E%E7%8E%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)检索并行工作，然后根据加权系统合并和排名结果。例如，使用`Weaviate`，可以调整`alpha`参数平衡向量和关键字搜索结果的重要性，创建一个组合的排序文档列表。这么做平衡了准确率和 召回率 ，提高了整体检索质量，但需要仔细调整权重参数。

![](img/814eee1957a1566fdd17eaa2fa4a5243.png)

### 查询重写

[查询重写 ](https://www.zhihu.com/search?q=%E6%9F%A5%E8%AF%A2%E9%87%8D%E5%86%99&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)认为人类查询可能不会针对向量数据库或语言模型进行优化。通过利用语言模型重写查询，可以显著改进检索过程。

+   向量数据库的查询重写：患者的初始查询被转换为对数据库更友好[的格式。例](https://www.zhihu.com/search?q=%E9%A3%9F%E6%8C%87%E9%BA%BB%E6%9C%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)如，“我的左肩疼，我的拇指和 食指麻木 ”可能会被重写为“左肩疼痛。麻木。食指麻木。”

+   语言模型的提示重写：这涉及自动和交互式自动构建提示，以优化与语言模型的交互，增强生成结果的质量和准确性。像`DSPy`这样的框架有助于这个过程。

这些重写的查询和提示确保搜索过程更好地理解和检索相关文档，并以最佳方式提示语言模型。查询重写可以如下所示实现。

![](img/c6bbb75c4911c4ffbeb6f89ea89f7f27.png)

### 微调嵌入模型

虽然预训练的嵌入模型提供了一个很好的起点，但在特定数据集上微调这些模型可以显著提高检索性能。这包括：

+   选[择适当的模](https://www.zhihu.com/search?q=%E7%B3%BB%E5%88%97%E6%9F%A5%E8%AF%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A711988265%7D)型：对于医学等专业领域，选择在相关数据上预训练的模型。对于这个特定的示例，选择使用`MedCPT` 系列查询 和文档编码器，这些编码器已经用`PubMed`搜索日志中255M查询-文章对进行了预训练。

+   使用正负对象对进行微调：通过收集自己的数据并创建相似（正）和不相似（负）示例对来进一步提高这些模型的性能，并微调模型以更好地理解这些区别。这有助于模型学习特定领域的关系和语义学，提高检索性能，但需要精心设计的训练数据。

当这些技术组合在一起时，可以创建一个健壮的检索系统，该系统增强了对象的整体相关性，当最终将其提供给LLM时，可以提高生成质量。

## 生成

最后，让我们讨论一下如何能够提高并控制LLM的生成质量。关键还是要确保提供给语言模型的上下文与提示尽可能相关，不相关的数据可能会引发幻觉。以下是一些实现更好生成结果的提示：

### 自动删除不相关信息

`AutoCut`是一种过滤掉从数据库中检索到的不相关信息的方法，否则可能会误导LLM并导致幻觉。它的工作原理如下：



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
