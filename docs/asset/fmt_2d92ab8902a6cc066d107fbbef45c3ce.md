
[OpenAI 助手](https://link.zhihu.com/?target=https%3A//platform.openai.com/docs/assistants/overview)基本上整合了开源 LLM 周边工具——聊天记录、知识存储、文档上传界面。最重要的是 函数调用 API ，其提供了将自然语言转换为对外部工具或数据库查询的 API 调用的功能。

[在 LlamaIndex 中](https://link.zhihu.com/?target=https%3A//docs.llamaindex.ai/en/stable/examples/agent/openai_agent.html)，有一个 `OpenAIAgent` 类 将这种高级逻辑与 `ChatEngine` 和 `QueryEngine` 类结合在一起，提供基于知识和上下文感知的聊天，以及在一个对话轮次中调用多个 OpenAI 函数的能力，这真正实现了智能代理行为。

让我们来看一下 [多文档](https://link.zhihu.com/?target=https%3A//docs.llamaindex.ai/en/stable/examples/agent/multi_document_agents.html)智能体的方案 —— 这是一个非常复杂的配置，涉及到在每个文档上初始化一个 `Agent`（ `OpenAIAgent` ），该智能体能进行文档摘要制作和传统问答机制的操作，还有一个顶层智能体，负责将查询分配到各个文档智能体，并综合形成最终的答案。

每个文档智能体都有两个工具：向量存储索引和摘要索引，它根据路由查询决定使用哪一个。对于顶级智能体来说，所有文档智能体都是其工具。

该方案展示了一种高级 RAG 架构，其中每个智能体都做路由许多决策。这种方法的好处是能够比较不同的解决方案或实体在不同的文档及其摘要中描述，以及经典的单个文档摘要和 QA 机制——这基本上涵盖了最常见的与文档集合聊天的用例。

![](img/b5bee247959d265639a64eec42199704.png)

这种复杂配置的缺点可以通过图片发现 —— 由于需要在智能体内部的大语言模型之间进行多次往返迭代，其运行速度较慢。顺便一提，LLM 调用通常是 RAG 管道中耗时最长的操作，而搜索则是出于设计考虑而优化了速度。因此，对于大型的多文档存储，我建议考虑对此方案进行简化，以便实现扩展。

### 8. 响应合成

这是任何 RAG 管道的最后一步——根据我们检索的所有上下文和初始用户查询生成答案。

最简单的方法是将所有获取的上下文（高于某个相关性阈值）与查询一起连接并提供给 LLM。但是，与往常一样，还有其他更复杂的选项，涉及多个 LLM 调用，以优化检索到的上下文并生成更好的答案。

响应合成的主要方法有：

+ 通过将检索到的上下文逐块发送到 LLM 来优化答案

+ 概括检索到的上下文，以适应提示

+ 根据不同的上下文块生成多个答案，然后将它们连接或概括起来。

有关更多详细信息，请查看 [响应合成器模块文档](https://link.zhihu.com/?target=https%3A//docs.llamaindex.ai/en/stable/module_guides/querying/response_synthesizers/root.html)。

## RAG 融合

和其他软件世界的架构决策一样，RAG融合也有权衡取舍，你需要清楚它们，以便为你的具体情境做出最好的决定。不过首先，我们先列一下RAG融合的优缺点。

### 优点：

+ 提供多样化的上下文：因为你可以从不同的角度查看用户的查询，所以顶级结果里会出现更多样化的内容片段。与专注于单一视角的内容段落相比，你更有可能看到能够涵盖话题多个方面的内容作为顶级结果出现。

+ 额外的控制层面：像其他机器学习解决方案一样，RAG融合提供了额外的控制手柄，让你可以微调你的应用，并让它更加符合你的期望目标。

+ 自动校正：通过使用LLM作为用户在文本框中输入内容与实际在数据库中搜索内容之间的中间人，你可以纠正拼写错误，添加与用户查询相关的上下文信息（关于用户的信息、时间、他们的账户状态等），以及/或潜在地筛选特定类型的内容。

+ 成本：这是一个有些争议的问题，因为成本既是RAG融合的优点也是缺点，让我来解释一下。你大概知道，向量搜索比LLMs要便宜得多，那么用于RAG融合的额外LLM调用是不是应该会增加应用的整体成本呢？不过……让我们再来看看LLM的成本。你大概也知道，你可能使用的大多数主流LLMs都采用基于令牌的计费模式。即使是自己托管LLM，你的成本也会与处理的令牌数量大致成正比。我们基本上在这儿向LLM发送两次请求，一次大概有100个令牌用来生成相似的查询，另一次则会有数千个令牌，我们提供相关文本块并希望从LLM那里得到适当的回应。所以基本上我们第一次对LLM的调用要比第二次便宜10倍到100倍。所以即使RAG融合每10次查询节省一次后续问题，你在成本上还是能领先的。

### 缺点：

+ 延迟:正如你现在可能知道的，LLMs需要大量的计算资源，因此它们的速度相对较慢(相对于我们应用程序中的其他部分)。根据你的应用程序，向LLM发送一次额外的请求可能会让用户的体验变得糟糕，因为他们可能需要等待几百毫秒的时间。

+ 自动纠错:是的，这是一个优点，但是当它不起作用时，也可能是一个缺点。这通常发生在你的内容包含内部术语或行话，而这些术语或行话没有出现在LLM学习过的数据中。在这种情况下，LLM可能会出现困惑并生成完全无关的查询，从而影响到最后的结果。

+ 成本:正如我们之前讨论的，如果RAG融合对你应用程序的整体效益贡献不大，你最终可能会花费更多的费用，但收益却很有限。

有了以上的介绍，让我们来讨论一下在什么情况下最有可能通过实现RAG融合得到好的效果。如果你的应用程序的内容主要基于常见概念，那么你更有可能从使用RAG融合中获益良多。

![](img/e87d9b40f1d502da6bb9bb45c455d659.png)

### RAG融合n不适用场景

如果您拥有的内容包含大量内部行话或与知名品牌重复的词语，则您可能需要调整RAG融合提示以获得良好的效果，或者最好避免使用。以下是一个例子来说明这一点。如今，所有的知名LLMs都是基于“注意力就是你所需要”的论文中首次引入的transformer架构，这是一种根据语境中其他单词对生成下一个token的重要性的度量方法。现在，如果我在该论文的基础上构建一个RAG应用并添加RAG融合功能，其工作方式可能是这样的：(绿色文字表示对RAG融合有贡献的LLM生成的查询)

![](img/62b4c4df5b1f97eb206bf65da9fd2abb.png)

由于在这个语境下理解“注意力”的含义依赖于上下文，LLM对这个含义产生了误解，并生成了一些与之完全无关的搜索查询。这可能导致您的应用得出糟糕的结果。现在，我们来看一下通过更改系统提示，将其改为“您是一个有用的助手，可以根据单一输入查询为试图解释transformer架构概念的应用生成多个搜索查询”，是如何在这个特定例子中解决这个问题的。

![](img/2bdda0763a8092d2fcd30de641eeea79.png)

根据您的具体情况，微调提示可能有用也可能没用。您也可以试试以下技巧，然后才放弃：

+ 使用语义搜索找到相关的查询：这个选项适合较为成熟的使用场景，但是如果你有很多用户基础，你可以尝试搜索一个相似的查询数据库，利用它们来寻找相关的内容。

+ 通过少量示例学习实现上下文理解：有时候，在给出提示前先给LLM展示几个例子也许能帮助提升效果。

+ 微调小型LLM：如果上述方法对你特定的使用场景都没有用（而测试这些方法相对较快），那么你可以考虑微调自己的LLM。这样可能会有效果，因为即使是很小的LLM（即使只有几百万个参数），也有可能足够好到能在特定模板下生成几个相似的查询。请注意，这种方法相比于前面提到的方法，会花费更多的成本和时间，但可能会给你带来最好的效果（代价是增加了复杂性）。

正如您所见，在这种情况下(像HyDe、TF-IDF、BM25或混合搜索等许多其他方法一样)，不清楚这种方法是否会胜过针对您具体用例的基本语义搜索功能。但是，就像人们常说的那样，“如果你不衡量，你就无法改进”。因此，我的建议是：一旦构建了基本的RAG应用程序，就要立即创建一个评估过程。在这一过程中，你会有大量的想法去优化你的提示或搜索功能，而每一种改变所带来的附带影响是不清楚的。有时候，提升某一组查询的效果会以牺牲另一组查询为代价。在这种情况下，最好的办法就是把它当作另一种机器学习问题，看看数据告诉你什么。

## 编码器和 LLM 微调

这种方法主要是对 Transformer 编码器 和 LLM 进行微调。其中，编码器影响嵌入质量，从而影响上下文检索质量。LLM 负责最好地使用提供的上下文来回答用户查询。

如今的一大优势是可以使用像 GPT-4 这样的高端 LLM 来生成高质量的数据集。但是必须清楚，使用小型合成数据集进微调基础模型，可能会降低基础模型的通用能力。

### 编码器微调

作者进行了一项测试，对 `bge-large-en-v1.5` 编码器进行微调，发现对于检索效果提升影响有限。因为针对搜索优化的最新 Transformer 编码器已经非常高效。

### 排序器微调

如果不完全信任基础编码器，可以使用交叉编码器对检索到的结果进行重排。这个过程是这样的：你把查询和每个前 `k` 个检索到的文本块一起[送入交叉编码器](https://link.zhihu.com/?target=https%3A//docs.llamaindex.ai/en/latest/examples/finetuning/cross_encoder_finetuning/cross_encoder_finetuning.html%23)，中间用 `SEP` (分隔符) Token 分隔，并对它进行微调，使其对相关的文本块输出 1，对不相关的输出 0。一个这种微调过程的成功案例可以在 这里 找到，结果显示通过交叉编码器微调，成对比较得分提高了 4%。

### LLM 微调

[最近，OpenAI 开始提供 LLM 微调 API](https://link.zhihu.com/?target=https%3A//platform.openai.com/docs/guides/fine-tuning)，LlamaIndex 有一个关于在 RAG 设置中微调 GPT-3.5-turbo 的教程。RAG 管道评估的 ragas 框架显示，忠实度指标增加了 5%，这意味着微调后的 GPT 3.5-turbo 模型比原始模型更好地利用了提供的上下文来生成答案。

[Meta AI Research](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2310.01352.pdf)最近的论文 RA-DIT： Retrieval Augmented Dual Instruction Tuning 展示了一种更复杂的方法，提出了一种同时调整 LLM 和 `Retriever` 的技术（原始论文中的双编码器）关于查询、上下文和答案的三元组。该技术被用于通过微调 API 微调 OpenAI LLM。也被用于微调了Llama2 开源模型（在原始论文中），结果与带有 RAG 的 Llama2 65B 相比，知识密集型任务指标增加 ~5%和常识推理任务增加几个百分点。

## 评估

RAG 系统性能评估的多个框架，都包含了几项独立的指标，例如总体答案相关性、答案基础性、忠实度和检索到的上下文相关性。

在之前章节提到的 [Ragas](https://link.zhihu.com/?target=https%3A//docs.ragas.io/en/latest/index.html)，使用 [真实性](https://link.zhihu.com/?target=https%3A//docs.ragas.io/en/latest/concepts/metrics/faithfulness.html)和 [答案相关性](https://link.zhihu.com/?target=https%3A//docs.ragas.io/en/latest/concepts/metrics/answer_relevance.html)来评价生成答案的质量，并使用经典的上下文 精准度 和 召回率 来评估 RAG 方案的检索性能。

最近推出的课程 [构建和评估高级 RAG](https://link.zhihu.com/?target=https%3A//learn.deeplearning.ai/building-evaluating-advanced-rag/)中，以及 [LlamaIndex](https://link.zhihu.com/?target=https%3A//github.com/truera/trulens/tree/main) 和评估框架 Truelens ，他们提出了 RAG 三元组 评估模式 — 分别是对问题的 检索内容相关性 、答案的 基于性（即大语言模型的答案在多大程度上被提供的上下文的支持）和答案对问题的 相关性 。

最关键且可控的指标是 检索内容的相关性 — 实际上是上述高级 RAG 管道的前 1-7 部分加上编码器和排名器的微调部分，这些都是为了提高这个指标。而第 8 部分和大语言模型的微调则专注于提高答案的相关性和基于性。

[一个简](https://link.zhihu.com/?target=https%3A//github.com/run-llama/finetune-embedding/blob/main/evaluate.ipynb)单有效的[检索器评估](https://link.zhihu.com/?target=https%3A//github.com/openai/openai-cookbook/blob/main/examples/evaluation/Evaluate_RAG_with_LlamaIndex.ipynb)管道的例子可以在 这里 找到，它已被应用于编码器的微调部分。一个更高级的方法不仅考虑 命中率 ，还包括了常用的搜索引擎评估指标 平均倒数排名 (Mean Reciprocal Rank) ，以及生成答案的质量指标，如真实性和相关性，这在 OpenAI 的 实用指南 中有所展示。

[LangChain](https://link.zhihu.com/?target=https%3A//docs.smith.langchain.com/)提供了一个颇为先进的评估框架 LangSmith 。在这个框架中，你不仅可以实现自定义的评估器，还能监控 RAG 管道内的运行，进而增强系统的透明度。

如果你正在使用 LlamaIndex 进行构建，可以尝试 `rag_evaluator` llama pack 。

## 总结

本文概述 RAG 的核心算法，并举例说明其中的一些方法。

RAG融合是一个强大的功能，能够提高RAG应用的语义搜索效率。通过使用语言模型生成多个查询并对搜索结果进行重新排序，RAG融合可以呈现更丰富多样的内容，并提供了一个额外的层次，用于调整应用。此外，RAG融合还可以实现自动纠正、节省成本以及增加内容多样性。但是，需要注意一些权衡，比如潜在的延迟问题、自动纠正的挑战以及成本影响。对于依赖常见概念但可能出现内部行话或重叠词汇的应用来说，RAG融合尤其有用。重要的是要评估RAG融合的表现并测量其影响，以确定它是否适合特定应用场景。

还有很多其他的事情需要考虑，比如基于网络搜索的 RAG（LlamaIndex 的 RAG、webLangChain 等），更深入地研究智能体架构以及关于 LLM 长期记忆的一些想法。

除了答案相关性和忠实度之外，RAG 系统的主要生产挑战是速度。ChatGPT 和大多数其他助手使用的这种流式特性不是随机的赛博朋克风格，而只是一种缩短感知答案生成时间的方法。

这就是为什么我认为小参数规模的 LLM 有一个非常光明的未来，最近发布的 Mixtral 和 Phi-2 正在引领我们朝着这个方向前进。

## 参考资料：

[爱吃牛油果的璐璐：借助知识图谱和 Llama-Index实现基于大模型的RAG](https://zhuanlan.zhihu.com/p/675131151)

[https://pub.towardsai.net/advanced-rag-techniques-an-illustrated-overview-04d193d8fec6?gi=77e3202eaa34](https://link.zhihu.com/?target=https%3A//pub.towardsai.net/advanced-rag-techniques-an-illustrated-overview-04d193d8fec6%3Fgi%3D77e3202eaa34)

果壳PAI：一文搞懂大模型RAG应用（附实践案例）

iyacontrol：图解高级RAG技术

[ketchum：构建基于RAG的聊天机器人（四）：RAG融合(RAG Fusion)](https://zhuanlan.zhihu.com/p/674032507)
