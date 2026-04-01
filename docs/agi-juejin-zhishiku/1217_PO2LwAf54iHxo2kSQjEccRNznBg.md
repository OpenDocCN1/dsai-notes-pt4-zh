# DSPy RAG与LlamaIndex——超越prompt的编程LLM

> 来源：[https://agijuejin.feishu.cn/docx/OEu6dxzkco6Q6Px8ZeOczAEFnid](https://agijuejin.feishu.cn/docx/OEu6dxzkco6Q6Px8ZeOczAEFnid)

> 作者：lucas大叔

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/700496881)hihu.com/p/700496...

在构建LLM应用时，Langchain和LlamaIn[dex是非](https://www.zhihu.com/search?q=%E7%94%9F%E6%80%81%E9%9B%86%E6%88%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700496881%7D)常不错的选择，它们有着丰富的 生态集成 ，可以帮助用户快速构建针对通用性问题的智能系统。但当你面对更具有针对性、适应性强的LLM应用开发时，DSPy比Langchain和LlamaIndex更合适。它提供了强大的通用模块，可以动态调整pipeline中的prompt或微调模型；无与伦比的模块化和自动优化，非常适合以最小的努力达到高质量的结果。

[DSPy与](https://www.zhihu.com/search?q=%E4%BA%92%E6%96%A5%E5%85%B3%E7%B3%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700496881%7D)Lan[gch](https://link.zhihu.com/?target=https%3A//github.com/Ronoh4/A-DSPy-based-RAG-with-LlamaIndex/blob/main/DSPyRAG.py)ain和LlamaIndex并非是 互斥关系 ，可以在应用中同时使用DSPy和Langchain或LlamaIndex。下面是DSPy和LlamaIndex共存的一个示例，展示DSPy如何在标准的DNN监督学习设置中使用训练数据集、优化器和bootstrapping优化RAG系统的LM输出。完整的代码见 这里 。

## 第一步：设置模块和 [环境变量](https://www.zhihu.com/search?q=%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700496881%7D)

结[合Llama](https://www.zhihu.com/search?q=%E7%B3%BB%E7%BB%9F%E7%9A%84%E5%8A%9F%E8%83%BD&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700496881%7D)Index和DSPy可以充分利用LlamaIndex的文档处理和DSPy的LM优化，从而增强RAG 系统的功能 。选择DSPy中的默认 BootstrapFewShot 提示器可确保以最小的设置实现高效的程序编译，使其成为启动简单RAG工作流程的理想选择。

![](img/039e7867afeb6656a46269e2bf2d7c03.png)

## 第二步：以查询引擎为索引构建简单的RAG

在DSPy系统中，索引简化 [数据检索 ](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E6%A3%80%E7%B4%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700496881%7D)[，提升L](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700496881%7D)M和RM的性能。DSPy支持流行的向量 数据库 ，如Pinecone、Chroma和Weaviate，确保了多功能集成。而LlamaParse作为首选的PDF解析工具，丰富了系统的文档处理能力。

![](img/b472a689ba900fa18f3c944cba1ae418.png)

## 第三步：定义签名、模块和预测器

在DSPy中，定义签名至关重要，因为每个LM调用都需要一个签名。签名简洁地描述了任务、输入和输出字段，有助于清晰地划分任务。

DSPy中的声明性定义模块促进了模块化的方法，重点关注pipeline信息流。DSPy会根据每个pipeline自动优化LM的使用，增强了效率。

[在DSP](https://www.zhihu.com/search?q=%E9%A2%84%E6%B5%8B%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700496881%7D)y中定义预测器是必不可少的。 预测器 封装了对于指定签名的LM使用，学习适应任务的行为。

![](img/d2cd377ea9f1b9e1f1461fa99145e6ef.png)

## 第四步：设置验证逻辑并筛选训练数据集

验证逻辑 [通过验证 ](https://www.zhihu.com/search?q=%E9%80%9A%E8%BF%87%E9%AA%8C%E8%AF%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700496881%7D)答案的正确性和上下文相关性，确保准确的预测结果。在DSPy RAG[中，这种逻](https://www.zhihu.com/search?q=%E6%B3%9B%E5%8C%96%E8%83%BD%E5%8A%9B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700496881%7D)辑对于保持模型的保真度至关重要。多样化的训练数据集确保了稳健的学习和 泛化能力 ，更大的数据集提高了模型的熟练度，涵盖了各种情景，并提供了跨任务的性能。

![](img/9824a27acf0b7495b3d2b44dc70451bf.png)

## 第五步：选择提示器，编译并查询系统

DSPy的提示器，比如 BootstrapFewShot，在优化程序方面表现出色，通过学习为模块选择有效的提示，促进了LM的高效使用。通过更多的自举跟踪，系统获得了更深入的洞察力，增强了在各种不同任务中的适应性并提高了性能，展示了DSPy的多功能性和健壮性。

![](img/452b2b91ea0da0712960a4a07385b023.png)

## 总结

总的来说，DSPy编程优先于prompt的哲学为Python语法提供了可组合和声明式的模块，它的自动编译器基于程序步骤为LM制作高质量的提示，通过统一的技术和简约的操作增强了RAG系统的准确性和多功能性。

## 参考文档

[ht](https://link.zhihu.com/?target=https%3A//medium.com/%40leighphil4/dspy-rag-with-llamaindex-programming-llms-over-prompting-1b12d12cbc43)tps:// medium.com/@leighphil4/ dspy-rag-with-llamaindex-programming-llms-over-prompting-1b12d12cbc43

[https:// github.com/Ronoh4/A-DSP y-based-R](https://link.zhihu.com/?target=https%3A//github.com/Ronoh4/A-DSPy-based-RAG-with-LlamaIndex/blob/main/DSPyRAG.py)AG-with-LlamaIndex/blob/main/DSPyRAG.py