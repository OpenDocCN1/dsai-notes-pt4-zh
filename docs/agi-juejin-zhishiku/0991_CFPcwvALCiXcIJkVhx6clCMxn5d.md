# RAG系统的“七宗罪”--一周出demo，半年用不好

> 来源：[https://agijuejin.feishu.cn/docx/J6PMdjehoom7ATxrfFTcWLjFnBh](https://agijuejin.feishu.cn/docx/J6PMdjehoom7ATxrfFTcWLjFnBh)

> 作者：[西红柿牛腩](https://www.zhihu.com/people/chenhong007)

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/677691070)hihu.com/p/677691...

R[AG（R](https://www.zhihu.com/search?q=LLM&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)et[rieva](https://www.zhihu.com/search?q=%E8%BF%AA%E8%82%AF%E5%A4%A7%E5%AD%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)l-Augmented [Gene](https://www.zhihu.com/search?q=%E4%B8%83%E9%87%8D%E7%BD%AA&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)ration）之所以被关注，很大一部分原因是，大多数人没有GPU集群搞 LLM 的预训练，退而求其次，不如在RAG方向做一些优化。RAG主要通过检索语义匹配的文档，然后将文档知识传递给大模型，基于大模型推理获取争取的答案。RAG可以减少LLM幻觉问题，消除文档标注。但是，来自澳大利亚的 迪肯大学 的学者，提出RAG的“ 七重罪 ”，并揭示RAG的一些研究方向：

1、RAG系统的验证只能在运行过程中进行。

2、RAG系统的稳健性是演进而非一开始就设计好的。

3、提出关于RAG系统的潜在研究方向清单。

![](img/c080d01a23498d7c7a5a3e8e773084ed.png)

用检索的方法配合大模型进行任务处理，可以很好[的使用企业](https://www.zhihu.com/search?q=%E5%BC%80%E6%BA%90%E6%A1%86%E6%9E%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)数据进行知识问答。但是RAG 开源框架 众多，现实很骨感。一周出demo，半年用不好。

![](img/6c9137f05a59d4f05c783ed11f6e11c4.png)

## RAG的“七宗罪”

如下图所示：RAG主要包括建索引Index和查询Query两个过程。索引过程通常在开发时完成，而查询在运行时完成。

![](img/de3828c5b8b9781ec6d3f7f0b057af7b.png)

论文使用了 [BioASQ ](https://www.zhihu.com/search?q=BioASQ&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)数[据集进行实验](https://www.zhihu.com/search?q=GPT-4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)，共涉及15,000篇文档和[1000个问题及答案对。我们对](https://www.zhihu.com/search?q=OpenAI%20evals%201&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)所有文档进行了索引，然后运行查询并使用 GPT-4 存储生成的响应。然后，使用 OpenAI evals 1 验证了所有问题和答案对。通过人工检查（所有差异、所有被标记为错误的以及一部分正确标签的样本），总结了RAG的七宗罪。

**第一罪(FP1) **：内容缺失（Missing Content）。提问的问题，无法在被检索文档库中找到，最准确的答案是缺失的。理想情[况下，RAG系统](https://www.zhihu.com/search?q=respone&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)回应应该是“抱歉，我不知道答案”。然而，对于检索内容相关但没有相关答案的问题，系统可能被误导，给出一个 respone 。

第二宗[罪(FP](https://www.zhihu.com/search?q=%E5%8F%AC%E5%9B%9E%E7%8E%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)[2)：检索的T](https://www.zhihu.com/search?q=LLM%E5%A4%A7%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)opK内容缺失(Missed the Top Ranked Documents)。问题的答案在文档库中，但排名得分不够高，无法返回给用户。理论上，检索过程中所有文档都会被排名得分。然而，在实际操作中，会返回排名前K个文档，为了提高 召回率 ，K不可能设置的无限大，必须基于 LLM大模型 的能力，折中选择的一个值。

[第三宗罪 ](https://www.zhihu.com/search?q=%E7%AC%AC%E4%B8%89%E5%AE%97%E7%BD%AA&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)(FP3)：未在上下文中（ **Not in Context **） - 整合策略局限性。从数据库中检索到了包含答案的文档，但在生成答案的过程中，这些文档并未被纳入上下文。当数据库返回许多文档时，会进行整合过程以获取答案，此时会发生这种情况。

第四宗罪(FP4)：未提取（Not Extracted）答案存在于上下文中，但大型语言模型未能提取出正确的答案。通常，这是因为上下文中存在太多噪声或矛盾信息。简而言之，Retrival命名是对的，但是LLM根据Retrival回答问题出错。睁眼说瞎话的概率明显大于用户可以接受的概率（用户一般只能接收0.1%的错误概率）

第五宗罪(FP5)：错误格式(Wrong Format)。问题涉及以某种格式（如表格或列表）提取信息，而大型语言模型忽略了这一指示。

第六宗罪(FP6)：错误的特异性(Incorrect Specificity)。返回的答案包含在响应中，但不够具体或过于具体，无法满足用户需求。这种情况发生在RAG系统设计者对某个问题有期望的结果，例如教师对学生。在这种情况下，应该提供具体的教育内容和答案，而不仅仅是答案。当用户不确定如何提问并过于笼统时，也会出现特异性错误。

[第七宗罪 ](https://www.zhihu.com/search?q=%E7%AC%AC%E4%B8%83%E5%AE%97%E7%BD%AA&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)(FP7)：不完整（ **Incomplete **）。不完整的答案并非错误，但缺少一些信息，尽管这些信息存在于上下文中并可供提取。

## 2 RAG [七宗罪 ](https://www.zhihu.com/search?q=%E4%B8%83%E5%AE%97%E7%BD%AA&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)的改善

| FP | 研究方向 | 具体描述 |
| --- | --- | --- |
| FP4 | 更多的上下文信息 | 大模型的窗口从4K增加到8K或者更大，LLM可以使用更多的上下文信息 |
| FP1 | [语义缓存 ](https://www.zhihu.com/search?q=%E8%AF%AD%E4%B9%89%E7%BC%93%E5%AD%98&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)降低了成本和延迟 | 由于速率限制和LLM的成本，RAG系统在应对并发用户方面存在困难。使用常见问题的预检索能力，可以缓解内容缺失现象。 |
| FP5~FP7 | RAG“越狱” | LLM [大模型fine-tuning ](https://www.zhihu.com/search?q=%E5%A4%A7%E6%A8%A1%E5%9E%8Bfine-tuning&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)，增加模型的基础能力 |
| FP2，FP4 | 增加元信息 | 将文件名和块编号添加到检索到的上下文中有助于读者提取所需信息。这对聊天对话很有用。 |
| FP2 FP4~7 | [开源嵌入模型 ](https://www.zhihu.com/search?q=%E5%BC%80%E6%BA%90%E5%B5%8C%E5%85%A5%E6%A8%A1%E5%9E%8B&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)在处理小型文本方面表现更优。 | 在处理小型文本方面，开源句子嵌入模型的表现与闭源替代品相当。 |
| FP2~7 | RAG系统需要持续校准。 | RAG系统在运行时接收未知输入，需要不断监控。 |
| FP1 FP2 | 实现一个RAG配置流水线 | [一个RAG](https://www.zhihu.com/search?q=%E5%B5%8C%E5%85%A5%E7%AD%96%E7%95%A5&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)系统需要校准块大小、 嵌入策略 、分块策略、检索策略、整合策略、上下文大小和提示。 |
| FP2,FP4 | 通过组装定制解决方案创建的RAG管道是次优的。 | [端到端训练模型 ](https://www.zhihu.com/search?q=%E7%AB%AF%E5%88%B0%E7%AB%AF%E8%AE%AD%E7%BB%83%E6%A8%A1%E5%9E%8B&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)，增强RAG的领域实用性 |
| FP2~FP4 | 只有在运行时才能测试性能特征。 | [离线评估技术 ](https://www.zhihu.com/search?q=%E7%A6%BB%E7%BA%BF%E8%AF%84%E4%BC%B0%E6%8A%80%E6%9C%AF&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)，如 [G-Evals ](https://www.zhihu.com/search?q=G-Evals&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A677691070%7D)看起来很有前景，但前提是能够获得标记过的问题和答案对。 |

Seven Failure Points When Engineering a Retrieval Augmented Generation System

[Seven Failure Points When Engineering](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2401.05856) a Retrieval Augmented Generation System