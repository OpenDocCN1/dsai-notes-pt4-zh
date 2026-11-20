
# 5. 聊天逻辑

在构建一个能够对单个搜索查询反复有效工作的高效 RAG 系统中，下一个关键进展是聊天逻辑的开发。这种逻辑考虑到对话上下文，其原理类似于LLM时代之前的经典聊天机器人。

这种逻辑的开发是为了支持用户提出后续问题、处理上下文中的指代或与先前对话上下文相关的任意命令。为了解决这一问题，研究者们采用了一种查询压缩技术，这种技术在处理用户的查询时同时考虑了聊天的上下文。

关于上下文的压缩，有几种不同的方法 —

+ 一种受欢迎且相对简单的方法是使用 `ContextChatEngine`[28]。这种引擎首先检索与用户查询相关的上下文，然后将其连同聊天历史从内存缓冲区一起发送给 LLM，确保 LLM 在生成下一条回答时能够理解之前的上下文。

+ 更为复杂的一个例子是 `CondensePlusContextMode`[29]。在这种模式下，每次互动中的聊天历史和最后一条消息会被压缩成一个新的查询。然后，这个查询被发送到索引系统，检索到的上下文连同原始用户消息一起回传。

值得注意的是，LlamaIndex 还支持基于 OpenAI 智能体的聊天引擎[30]，提供了更灵活的聊天模式，Langchain 也支持[31] OpenAI 功能性 API。

![](img/f4634fbfcd7cbf4a7df48ac7dd91a5b9.png)

还有其他聊天引擎类型，如`ReAct Agent`[32]，本部分内容会在第7节中详细介绍。

# 6. 查询路由

查询路由是一种基于LLM的决策步骤，用于确定针对用户的查询接下来应采取的行动。通常的选项包括概括回答、针对某个数据索引执行搜索，或尝试多条不同的途径，然后将它们的结果综合成一个答案。

查询路由器还用于选择合适的索引或更广泛的数据存储位置来处理用户查询。这可能涉及多个数据来源，比如传统的向量存储、图数据库或关系型数据库，或者是一个索引层级结构。在多文档存储情况下，一个常见的设置是一个概要索引和另一个文档块向量的索引。

设置查询路由器涉及确定它能做出的选择。路由选项的选择是通过 LLM 的调用来实现的，返回的结果按照预定义的格式来引导查询到指定的索引。或者，如果涉及多智能体行为，它可以引导到子链或其他智能体，如下面的多文档智能体方案所示。

LlamaIndex[33] 和 LangChain[34] 都支持查询路由器。

# 7. RAG 中的智能体

智能体（Langchain[35] 和 LlamaIndex[36] 都支持）自从第一个 LLM API 发布以来就已存在。其核心理念是为具备推理能力的 LLM 提供一套工具和一个待完成的任务。这些工具可能包括确定性函数（如代码功能或外部 API）或其他智能体。正是这种 LLM 链接的思想促成了 LangChain 的命名。

智能体本身是一个非常广泛的领域，在 RAG 概述中无法深入探讨，因此我将直接继续讨论基于智能体的多文档检索案例，并简要介绍 OpenAI 助手。OpenAI 助手是在最近的 OpenAI 开发者大会上作为 GPTs 提出的相对较新的概念[37]，并在下面描述的 RAG 系统中发挥作用。

OpenAI Assistants[38]集成了许多围绕LLM必需的工具，这些工具我们以前在开源项目中已经见过 —— 包括聊天记录管理、知识库存储、文档上传界面，以及最关键的，功能调用 API[39]。这个 API 的重要功能是能够将自然语言请求转换为外部工具或数据库查询的 API 调用。

在 LlamaIndex 中，`OpenAIAgent`[40] 类融合了这些高级功能，并与 `ChatEngine` 和 `QueryEngine` 类结合，提供了基于知识的、具有上下文感知能力的聊天体验。此外，它还能在一次对话交互中调用多个 OpenAI 功能，从而真正实现智能代理行为。

接下来让我们来了解一下多文档代理方案[41] —— 这是一个相当复杂的设计，涉及到在每个文档上初始化一个代理（`OpenAIAgent`），这个代理不仅能进行文档摘要处理，还能执行经典的问答流程。还有一个顶级代理，负责将查询任务分配给各个文档代理，并合成最终答案。

每个文档代理配备了两种工具 —— 向量存储索引和摘要索引，它会根据接收到的查询决定使用哪个工具。对于顶级代理来说，所有文档代理都是其工具，可供其调度使用。

这个方案展示了一个高级的 RAG 架构，涉及每个代理做出的复杂路由决策。这种架构的优势在于它可以比较不同文档中描述的不同解决方案或实体，以及它们的摘要，同时也支持经典的单文档摘要处理和问答流程 —— 这实际上覆盖了大多数与文档集合交互的常见用例。

![](img/83bf410ce50b3ea74106e5ada87e4a05.png)

这是一个展示多文档代理工作方式的方案，其中包括了查询的路由处理和代理的智能行为模式。

这个复杂方案的一个缺点可以通过图像来理解 —— 由于需要在代理内部与大语言模型（LLM）进行多轮迭代，因此处理速度较慢。需要注意的是，在 RAG 架构中，调用 LLM 总是最耗时的步骤，而搜索则是出于设计考虑而优化了速度。因此，对于涉及大量文档的存储系统，我建议对这一方案进行简化，以提高其扩展性。

# 8. 响应合成器

这是 RAG 架构中的最后一步 —— 基于我们精心收集的所有上下文和用户的初步查询来生成答案。

最简单的方法是直接将所有相关度较高的上下文和查询串联起来，一次性输入到LLM中。

然而，还有一些更复杂的方法，这些方法涉及多次使用LLM来细化检索到的上下文，并据此生成更加准确的答案。响应合成的几种主要方法包括：

1. 将检索到的上下文分块后逐次发送给大语言模型（LLM），以此迭代地精炼答案。

1. 总结检索到的上下文，使其适应输入提示。

1. 基于不同上下文块生成多个答案，然后将这些答案连接或总结起来。

更多细节请参考响应合成器模块的文档[42]。

# 9. 编码器和 LLM 微调

这个方法涉及对我们 RAG 架构中的两种深度学习模型之一进行精细调整。这可以是负责生成高质量嵌入从而影响上下文检索效果的 Transformer 编码器，也可以是用于最佳地利用所提供上下文以回答用户问题的LLM — 幸运的是，后者非常擅长少样本学习。

目前一个重要的优势是我们能够使用像 GPT-4 这样的高端大语言模型来创造高质量的合成数据集。

但同时，我们也必须认识到，对一个由专业团队在大型、经过精心筛选和清洗的数据集上训练的开源模型进行快速而简单的调整（尤其是使用小型合成数据集时），可能会限制模型的整体性能和能力。

## 编码器的微调

我曾对编码器的微调持保留态度，因为最新的为搜索优化的 Transformer 编码器已经非常高效了。

所以，我在 LlamaIndex notebook[43]环境中测试了 `bge-large-en-v1.5`[44]（在撰写本文时，该模型在 MTEB 排行榜上排名前四）的微调效果。测试结果表明，检索质量提高了 2%。虽然这个提升不是非常显著，但了解这一微调选项还是有益的，特别是在为特定领域的数据集构建 RAG 时。

## `Ranker` 的微调

另一个传统的选择是，如果你对基础编码器的效果不完全满意，可以使用一个交叉编码器来重新评估你检索到的结果。

这个方法的工作原理是这样的 —— 你需要将查询语句和检索到的前 k 个文本块一起传递给交叉编码器，每部分之间用 `SEP token` 分隔，并对其进行微调，以便对相关的文本块输出 1，对不相关的输出 0。 这种调整过程的一个很好的例子可以在这里[45]找到，结果显示通过交叉编码器的微调，成对得分提升了 4%。

## LLM 的微调

OpenAI 最近推出了大语言模型微调 API[46]，LlamaIndex 也提供了一个教程[47]，介绍如何在 RAG 环境下微调 GPT-3.5-turbo，从而“吸收”GPT-4 的部分知识。这个过程包括使用一份文档，利用 GPT-3.5-turbo 生成问题，然后用 GPT-4 基于该文档内容回答这些问题（即构建一个由 GPT-4 驱动的 RAG 架构），接着对 GPT-3.5-turbo 进行微调，训练它处理这些问题与答案对。通过这种方式，使用 `ragas`[48] 框架进行的 RAG 架构评估显示，信实度指标提高了 5%，意味着经过微调的 GPT 3.5-turbo 在利用提供的上下文生成答案方面比原模型表现得更好。

Meta AI Research 最近的一篇论文《RA-DIT: Retrieval Augmented Dual Instruction Tuning》展示了一种更为复杂的方法，它提出了一种同时对LLM和检索器（原论文中的双编码器）进行微调的技术，这种微调是基于查询、上下文和答案的三元组进行的。具体实施细节请参考相关指南[49]。这种技术被用于通过微调 API 对 OpenAI 大语言模型进行微调，同时也用于开源模型 Llama2（如原论文所述），在知识密集型任务的指标上实现了约 5% 的提升（与使用 RAG 的 Llama2 65B 相比），并在常识推理任务上也有一定的提升。

# 10. 评估

RAG 系统的性能评估通常涉及多个独立的指标，包括整体答案的相关性、答案的根据性、信实度以及检索到的上下文的相关性等。

之前提到的 `Ragas` 主要利用信实度[50]和答案相关性[51]作为评估生成答案质量的指标，并使用传统的上下文精确度[52]和召回率[53]来评估 RAG 架构的检索效果。

Andrew NG 最近推出的精彩短课程《构建和评估高级 RAG》[54]中，结合 LlamaIndex 和评估框架 `Truelens`，提出了 RAG 的三大评估维度：检索到的上下文与查询的相关性、答案的根据性（大语言模型的答案在多大程度上得到提供的上下文的支持）以及答案对查询的相关性。

最关键也是最容易控制的指标是检索到的上下文相关性。实际上，高级 RAG 架构的前 1-7 部分以及编码器和排名器的微调部分都是为了提高这一指标，而第 8 部分和大语言模型的微调则侧重于提高答案的相关性和根据性。

关于简单的检索器评估流程的一个很好的例子可以在这里找到[55]，它在编码器微调部分得到了应用。一个更高级的方法考虑到了不仅仅是命中率，还有平均倒数排名（一种搜索引擎常用的指标）以及生成答案的信实度和相关性指标，这在 OpenAI cookbook[56]中有展示。

LangChain 提供了一个先进的评估框架 `LangSmith`[57]，不仅可以实现自定义的评估方法，还可以监控在 RAG 架构内部运行的流程，从而增加系统的透明度。

如果你正在使用 LlamaIndex 进行构建，那么有一个 `rag_evaluator llama`[58] 包，它提供了一个快捷的工具，可以利用公共数据集来评估你的架构。

# 11. 结论

我努力概述了 RAG 的核心算法方法，并通过一些示例展示它们，希望这能激发你在自己的 RAG 架构中尝试一些新思路，或者对今年推出的众多技术有更系统的理解 —— 对我而言，2023 年是迄今为止机器学习领域最令人兴奋的一年。

还有很多其他方面需要探索，如基于网络搜索的 RAG（例如 LlamaIndex、`webLangChain`[59] 的 RAGs[60]）、更深入地研究智能体架构[61]以及关于大语言模型长期记忆功能的一些构想[62]。

RAG 系统在生产中的主要挑战，除了答案的相关性和信实度之外，还包括速度，尤其是在采用更灵活的基于智能体的方案时更是如此，但这需要另外的讨论。ChatGPT 和许多其他助手所采用的流式特性，并不是随机的赛博朋克风格，而是为了缩短用户感知的答案生成时间。这也是我认为小型大语言模型，以及近期发布的 Mixtral 和 Phi-2 等产品，在未来将大有可为的原因。

有关更多的主要参考资料，可以在我的知识库中找到，还有一个与这些文档相关的co-pilot聊天工具：https://app.iki.ai/playlist/236。

## 相关链接

[1] https://app.iki.ai/playlist/236

[2] https://faiss.ai/

[3] https://docs.llamaindex.ai/en/latest/examples/prompts/prompts_rag.html

[4] https://platform.openai.com/docs/guides/prompt-engineering/strategy-write-clear-instructions

[5] https://www.pinecone.io/learn/chunking-strategies/

[6] https://docs.llamaindex.ai/en/stable/api_reference/service_context/node_parser.html

[7] https://huggingface.co/BAAI/bge-large-en-v1.5

[8] https://huggingface.co/intfloat/multilingual-e5-large

[9] https://huggingface.co/spaces/mteb/leaderboard

[10] https://docs.llamaindex.ai/en/latest/module_guides/loading/ingestion_pipeline/root.html#

[11] https://boston.lti.cs.cmu.edu/luyug/HyDE/HyDE.pdf

[12] https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/MetadataReplacementDemo.html

[13] https://python.langchain.com/docs/modules/data_connection/retrievers/parent_document_retriever

[14] https://docs.llamaindex.ai/en/stable/examples/retrievers/recursive_retriever_nodes.html

[15] https://plg.uwaterloo.ca/~gvcormac/cormacksigir09-rrf.pdf

[16] https://python.langchain.com/docs/modules/data_connection/retrievers/ensemble

[17] https://docs.llamaindex.ai/en/stable/examples/retrievers/reciprocal_rerank_fusion.html

[18] https://www.sbert.net/examples/applications/cross-encoder/README.html

[19] https://txt.cohere.com/rerank/

[20] https://python.langchain.com/docs/modules/data_connection/retrievers/MultiQueryRetriever?ref=blog.langchain.dev

[21] https://docs.llamaindex.ai/en/stable/examples/query_engine/sub_question_query_engine.html

[22] https://arxiv.org/pdf/2310.06117.pdf?ref=blog.langchain.dev

[23] https://github.com/langchain-ai/langchain/blob/master/cookbook/stepback-qa.ipynb?ref=blog.langchain.dev

[24] https://github.com/langchain-ai/langchain/blob/master/cookbook/rewrite.ipynb?ref=blog.langchain.dev

[25] https://llamahub.ai/l/llama_packs-fusion_retriever-query_rewrite

[26] https://github.com/run-llama/llama-hub/tree/main/llama_hub/llama_packs/fuzzy_citation

[27] https://github.com/run-llama/llama-hub/tree/main/llama_hub/llama_packs/fuzzy_citation

[28] https://docs.llamaindex.ai/en/stable/examples/chat_engine/chat_engine_context.html

[29] https://docs.llamaindex.ai/en/stable/examples/chat_engine/chat_engine_condense_plus_context.html

[30] https://docs.llamaindex.ai/en/stable/examples/chat_engine/chat_engine_openai.html

[31] https://python.langchain.com/docs/modules/agents/agent_types/openai_multi_functions_agent

[32] https://docs.llamaindex.ai/en/stable/examples/chat_engine/chat_engine_react.html

[33] https://docs.llamaindex.ai/en/stable/module_guides/querying/router/root.html

[34] https://docs.llamaindex.ai/en/stable/module_guides/querying/router/root.html

[35] https://python.langchain.com/docs/modules/agents/

[36] https://docs.llamaindex.ai/en/latest/use_cases/agents.html#
