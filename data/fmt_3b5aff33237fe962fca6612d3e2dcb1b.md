
# RAG技术评估与生态实践

上下文相关性和噪声鲁棒性是评估检索质量的重要指标，而答案忠实度、答案相关性、负面拒绝、信息整合和反事实鲁棒性则是评估生成质量的关键。这些评估方面的具体度量标准在文献中进行了总结，但目前这些度量还不是成熟或标准化的评估方法。尽管如此，一些研究也已经开发出针对RAG模型特性的定制度量指标。

![](img/da935020707d03406e19d151c9a3728e.png)

## 评估的Benchmarks和工具

这一部分介绍了RAG模型的评估框架，该框架包含基准测试和自动评估工具。这些工具提供用于衡量RAG模型性能的定量指标，并且帮助更好地理解模型在各个评估方面的能力。知名的基准测试如RGB和RECALL专注于评价RAG模型的关键能力，而最新的自动化工具如RAGAS、ARES和TruLens则利用大型语言模型来评定质量得分。这些工具和基准测试共同形成了一个为RAG模型提供系统评估的坚实框架，相关细节在下表中有所总结。

![](img/86a50454e6db884c6b204949e0e95969.png)

## 展望与挑战

### RAG面临的挑战

尽管RAG技术已经取得了重大进展，但仍有若干挑战需要深入研究。其中包括如何处理LLMs的上下文窗口大小限制、提升RAG的鲁棒性、探索结合RAG和微调（RAG+FT）的混合方法、扩展LLMs在RAG框架中的角色、研究规模法则在RAG中的适用性，以及实现生产就绪的RAG。特别地，需要在RAG模型中找到平衡上下文长度的方法，提高对抗性或反事实输入的抵抗力，并确定RAG与微调的最佳整合方式。同时，需要确保RAG在生产环境中的实用性和数据安全，解决检索效率和文档召回率的问题。这些挑战的探索和解决将推动RAG技术向前发展。

### RAG的模态扩展

RAG技术已经发展到不仅限于文本问答，而是包含图像、音频、视频和代码等多种数据模态。这一扩展催生了在各个领域整合RAG概念的创新多模态模型。例如，RA-CM3作为一个多模态模型，能够检索和生成文本与图像；BLIP-2利用图像编码器和LLMs进行视觉语言预训练，实现图像到文本的转换；而"Visualize Before You Write"方法则展示了在开放式文本生成任务中的潜力。音频和视频方面的GSS方法和UEOP实现了数据的音频翻译和自动语音识别，而Vid2Seq通过引入时间标记帮助语言模型预测事件边界和文本描述。在代码领域，RBPS通过检索与开发者目标一致的代码示例擅长处理小规模学习任务，而CoK方法则通过整合知识图谱中的事实来提高问答任务的性能。这些进展表明，RAG技术在多模态数据处理和应用方面具有巨大的潜力和研究价值。

## RAG的生态

### 下游任务和评估

RAG技术在丰富语言模型处理复杂查询和生成详尽回答方面表现出极大潜力，它已经在开放式问题回答和事实验证等多种下游任务中展现了优异的性能。RAG不但提升了回答的精准度和关联性，还增强了回答的多样性和深度。特别在医学、法律和教育等专业领域，RAG可能会减少培训成本，提升与传统微调方法相比的性能。为了最大化RAG在各种任务中的效用，完善其评估框架至关重要，包括开发更加细致的评估指标和工具。同时，增强RAG模型的可解释性是一个关键目标，以便用户能更好理解模型生成回答的逻辑，促进RAG应用的信任度和透明度。

### 技术栈

RAG生态系统的发展显著受到其技术栈进化的影响。随着ChatGPT的兴起，LangChain和LLamaIndex等关键工具因其提供的丰富RAG相关API而快速流行，成为LLMs领域的核心工具。即便新兴技术栈在功能上不如它们，也通过专业化的服务来突显差异化，例如Flowise AI通过低代码途径使用户能够轻松部署AI应用。同样，HayStack、Meltano和Cohere Coral等技术因其独到的贡献而备受瞩目。

传统软件和云服务提供商也在拓展服务以提供RAG为中心的解决方案，如Weaviate的Verba和亚马逊的Kendra。RAG技术的演变呈现出不同的专业化方向，包括定制化、简化和专业化，以更好地适应生产环境。

RAG模型及其技术栈的共同成长表现在技术进步为基础设施设定了新的标准，技术栈的增强又推动了RAG能力的进一步演化。RAG工具包正在成为企业应用的基础技术栈，但一个完全集成的综合平台仍需要进一步创新和发展。

## 实践

### LlamaIndex实践

本文[中](https://link.zhihu.com/?target=https%3A//blog.llamaindex.ai/a-cheat-sheet-and-some-recipes-for-building-advanced-rag-803a9d94c41b)所提到的很多RAG的优化方案，都可以在LlamaIndex中找到对应的实现，LlamaIndex官方也出了一份官方的指南，详细介绍了一些模块的最佳实践经验。更多详情可以参考 A Cheat Sheet and Some Recipes For Building Advanced RAG 。

下图列举了一些RAG技术在llamaindex中对应的代码模块，可以参考llamaindex文档进一步尝试。

![](img/90137559f77c627e582cf139aeef6e35.png)

![](img/6faa84e5a7085d4c75f78a21b8ab616c.png)

## 业界实践

### 百川智能的RAG方案

百川智能的RAG方案流程包括以下几个关键步骤：

1. 提升上下文窗口容量：百川智能推出了Baichuan2-192K模型，它具有业界领先的192K超长上下文窗口，能一次处理高达35万汉字的文本。
2. 搜索增强知识库：将向量数据库升级为搜索增强知识库，使得大模型在响应用户查询时能够访问到互联网实时信息和企业的完整知识库。
3. 用户意图理解和搜索查询优化：在用户提出的Prompt（查询）基础上，使用自研大模型进行微调，将连续多轮、口语化的用户查询转换为搜索引擎更容易理解的关键词或语义结构。
4. 复杂Prompt拆分与并行检索：借鉴Meta的CoVe技术，将复杂的Prompt拆分为多个可以并行检索的查询，使大模型能针对每个查询进行定向知识库搜索。
5. 进一步理解用户意图：使用TSF（Think Step-Further）技术来推断用户输入背后的更深层问题，从而更全面地理解用户意图并引导模型提供更有价值的答案。

![](img/90392e72cc16c55a330b120122fbaac0.png)

6. 向量检索、稀疏检索、Rerank的结合：为了提高知识获取效率和准确性，百川智能结合使用了向量检索与稀疏检索，形成了一种混合检索方式，以提高目标文档的召回率。
7. 大模型自省技术：在通用RAG基础上，百川智能创新性地提出了Self-Critique技术，让大模型能够根据Prompt，对搜索回来的内容进行自省和筛选，以确保提供与用户查询最匹配、最优质的答案。

![](img/867b2233c483cd55c8f6922d063a6de1.png)

8. 模型与搜索的深度融合：通过这些步骤，百川智能实现了大模型与搜索的紧密结合，为用户提供定制化解决方案，有效降低成本、提升性能，并持续增值企业专有知识库。

百川智能的RAG方案显著地改善了大模型在行业垂直场景中的应用，通过提供一种更低成本、更高效的定制化大模型解决方案，提升了大模型技术的落地潜力，并有望引领大模型产业走向一个全新的阶段。

### OpenAI案例

[applying-openai-rag](https://link.zhihu.com/?target=https%3A//blog.langchain.dev/applying-openai-rag/)

[OpenAI RAG视频](https://link.zhihu.com/?target=https%3A//www.youtube.com/watch%3Fv%3DahnGLM-RC1Y)

![](img/3e56790b59159ec7d1e9177c0458e812.png)

![](img/ef7260d217756b586927b087e195d743.png)

OpenAI展示了一个使用检索增强生成（RAG）技术来优化问题解答系统的案例。起初，系统仅仅通过基于余弦相似度的检索方案达到45%的准确率。为了提高性能，尝试了多种策略，如HyDE检索，它通过生成虚拟答案并用其检索相关段落，以及微调嵌入模型来调整嵌入空间。虽然这些方法提高了准确性，但由于成本和速度的问题，最终并未被采用。通过调整数据分片和嵌入，准确率提升至65%；进一步通过Rerank和分类不同类型的问题，准确率提升至85%。最后，通过prompt工程、引入工具使用和查询扩展等方法，将准确率提高到了98%。在整个过程中，他们并没有进行大模型的微调，并强调了解决问题的关键在于检索系统能够提供正确的上下文信息。

## 总结

![](img/3c50c9dfd98abf9715f50b370360065e.png)

RAG技术通过结合语言模型中的参数化知识和外部知识库中的非参数化数据，显著提升了大型语言模型（LLMs）的能力，特别是在处理复杂查询和生成详细响应方面。RAG技术经历了从初级到高级再到模块化的演进，其中高级RAG通过引入查询重写和块重新排序等复杂架构元素，提升了性能和可解释性。RAG与微调和强化学习等其他AI方法的整合，进一步扩展了其功能。在内容检索方面，采用结构化和非结构化数据源的混合方法正成为趋势。RAG的应用范围正在扩展到多模态数据，如图像、视频和代码，突出了其在AI部署方面的实际意义。

RAG生态系统的增长表现在以RAG为中心的AI应用的增加和支持工具的发展。随着RAG应用领域的扩张，提炼评估方法以跟上其进化变得迫切必要，确保性能评估的准确性和代表性对于充分捕捉RAG在AI研究和开发中的贡献至关重要。

## 参考资料

1. [Gao Y, Xiong Y, Gao X, et al. Retrieval-augmented generation for large language models: A survey[J]. arXiv preprint arXiv:2312.10997, 2023.](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2312.10997)

2. [Advanced RAG Techniques: an Illustrated Overview](https://link.zhihu.com/?target=https%3A//pub.towardsai.net/advanced-rag-techniques-an-illustrated-overview-04d193d8fec6)

3. [A Cheat Sheet and Some Recipes For Building Advanced RAG](https://link.zhihu.com/?target=https%3A//blog.llamaindex.ai/a-cheat-sheet-and-some-recipes-for-building-advanced-rag-803a9d94c41b)

4. [10 Ways to Improve the Performance of Retrieval Augmented Generation Systems](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/10-ways-to-improve-the-performance-of-retrieval-augmented-generation-systems-5fa2cee7cd5c)

5. [大模型+搜索构建完整技术栈，百川智能用搜索增强给企业定制化下了一剂「猛药」](https://link.zhihu.com/?target=https%3A//www.jiqizhixin.com/articles/2023-12-20-10)
