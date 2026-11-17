
# 从0开始学RAG之Feedback + self-reflection

> 来源：[https://agijuejin.feishu.cn/docx/FJIRd8DkfoVUBOxTRVqcY3lTnYe](https://agijuejin.feishu.cn/docx/FJIRd8DkfoVUBOxTRVqcY3lTnYe)

> 作者：lucas大叔

> 原文：https://zhuanlan.zhihu.com/p/691080401

作为《从0开始学RAG》系列的最后一篇，本文探讨如何利用自我反思(self-reflection)和反馈(feedback)改善RAG性能。

存在问题：RAG系统可能会受到低质量检索（例如，用户问题不在索引的域内）和生成幻觉的困扰。原始的检索-生成管道无法检测或自我纠正这些错误。

解决思路：通过单元测试反复构建代码问题的答案，以检查并自我更正错误。一些工作已经应用了这种RAG，例如 Self-RAG 和 Corrective RAG。这两种情况都会在RAG 回答流 中检查文档相关性、幻觉或者回答质量。使用`LangGraph` 编排 检查和反馈，从而实现检索内容的评估和修正。

---

## Self-RAG

常规的RAG流程将检索到的文档作为LLM回答用户问题的背景知识，但并没有考虑检索是否是必要的，检索到的文档是否与问题相关，因此可能会生成低质量的答案。Self-RAG通过检索-->自我反思提高LM的质量和真实性，该框架训练单个任意LM自适应按需检索段落，对检索到的段落和用反思token 生成的内容进行生成和反思。生成反思token使得LM在推理阶段可控，使其能够调整行为适应不同的任务需求。

![](img/8aed28252b1e1f9b1a70d8ca9073af10.png)

如上左图所示，RAG用检索到的相关段落作为LLM的增强输入，以减少知识密集型任务中的事实错误。然而，常规的RAG方法可能会阻碍LLM生成的多样性。另外由于没有考虑检索到的段落事实上是否对回答问题有用，引入非必要或偏离主题的段落会导致低质量的生成。此外，模型没有经过明确的训练去利用和遵循所提供段落中的事实，不能保证输出与检索到的段落一致。

Self-RAG通过按需检索(on-demand retrieval)和 自我反思 (self-reflection)，在不损害生成多样性的同时保证事实的准确性，从而改善LLM的生成质量。以端到端的方式训练任意LM学习反思给定任务输入的自我生成过程，生成任务输出和特殊的token(即反思token)。反思token分为检索token(retrieval token)和评判token(critique token)，分别表示是否需要检索和生成质量。如右上图，给定个输入提示和之前的生成内容，Self-RAG首先确定用检索到的段落增强接下来的生成是否有帮助。如果有帮助，它将输出检索token按需调用检索器模型(step 1)。接下来，Self-RAG同时处理多个检索段落，评估它们的相关性，然后生成相应的任务输出(step 2)。然后生成评判token来评估自己的输出，根据真实性和总体质量选择最佳输出(step 3)。Self-RAG的处理流程与常规RAG不同，常规RAG返回固定数量的文档用于生成，并不考虑检索的必要性(例如上图底部的例子并不需要事实性的知识)。它也不会二次评估生成质量。另外，Self-RAG通过 自我评估 段落是否支持生成结果为每个段落片段提供引证，从而更容易进行事实验证。

我们利用`langgraph`实现self-RAG的思想，整体流程如下图所示：

![](img/027d36470a45774977d51ab99191a971.png)

基于ChatGPT的实现代码详见 [这里](https://link.zhihu.com/?target=https%3A//github.com/langchain-ai/langgraph/blob/main/examples/rag/langgraph_self_rag.ipynb)。

基于Ollama和开源模型Mistral的实现代码详见 [这里](https://link.zhihu.com/?target=https%3A//github.com/langchain-ai/langgraph/blob/main/examples/rag/langgraph_self_rag_local.ipynb)。

---

## CRAG

CRAG聚焦在改善生成过程的 鲁棒性，具体来说，它设计了一个轻量级的检索评估器，评估检索到的文档对查询的总体质量，然后返回置信度，基于 置信度 触发不同的知识检索动作。由于从静态的和有限的 语料库 中检索，因此只能得到次优的文档。大规模 网络搜索 可以作为检索结果增强的扩展。此外，CRAG还设计了先分解-后重组的 检索方法 ，选择性地关注检索到文档的关键信息，过滤掉不相关的信息。CRAG可与任何基于RAG的方法无缝耦合，即插即用。

![](img/02be5e6487d1ba47aa35bb8a2400614c.png)

从上图可以看到，在CRAG框架中，给定输入`query`和任意一种 `retriever` 检索到的文档，构建轻量级的 检索评估器 评估检索到的文档与输入`query`之间的相关性。通过设置两个门限，将相关性得分量化为三个置信度，然后触发相应的动作：{`Correct`, `Incorrect`, `Ambiguous`}。

+ `Correct`：当检索到的文档中至少有一个文档置信度得分高于上门限时，则认为检索是`correct`的，在检索结果中存在相关文档。但即使找到了相关文档，也不可避免掺杂一些噪声知识。为了提取文档中的重要知识点，还需要“知识提纯”( `knowledge refinement` )。"知识提纯"就是把每篇检索到的文档根据启发式规则切割成更 细粒度 的知识片段，然后用检索评估器计算每个知识片段的得分，基于得分把不相关的知识过滤掉，相关的知识再按顺序拼接在一起。

+ `Incorrect`：如果所有检索到的文档置信度得分都低于下门限，则认为检索时`incorrect`的，表明所有检索到的文档都认为是不相关的，对生成没有帮助。因此就要寻求新的知识源来修正，此处引入网页搜索从互联网搜索相关知识，避免了生成过程没有可信知识参考的尴尬。这里网页搜索利用ChatGPT把输入`query`重写为关键字的形式，模拟 搜索引擎 的日常使用状态。

+ `Ambiguous`：除了以上两种情况，其余的情形都归于 `ambiguous` 。由于检索评估器对它自己的判断并不确信，那就联合 `correct` 和`incorrect`两种情况处理，从而达到互补的目的。即一方面把检索到的文档走`Correct`处理流程，进行知识提纯；另一方面走网页搜索检索互联网上的相关知识，然后把两种方式得到的知识一起用于生成。

同样的，利用`langgraph`实现CRAG，整体流程如下图所示：

![](img/9439722cee032f015f791395665d12db.png)

基于ChatGPT的实现代码详见 [这里](https://link.zhihu.com/?target=https%3A//github.com/langchain-ai/langgraph/blob/main/examples/rag/langgraph_crag.ipynb)。

基于Tavily Search和开源模型Mistral的实现代码详见 [这里](https://link.zhihu.com/?target=https%3A//github.com/langchain-ai/langgraph/blob/main/examples/rag/langgraph_crag_local.ipynb)。

## 参考资料

`https://arxiv.org/abs/2310.11511`

`https://arxiv.org/pdf/2401.15884.pdf`
