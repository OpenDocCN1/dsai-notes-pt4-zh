# Part 1.从零到一Llama-Index上手指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/YWiQdUcJDoUQGcxlDanc48FfnTd](https://kq4b3vgg5b.feishu.cn/docx/YWiQdUcJDoUQGcxlDanc48FfnTd)

# 最强RAG框架：Llama-Index快速入门实战

# Part 1.从零到一Llama-Index上手指南

## 一、RAG（Retrieval-Augmented Generation，检索增强生成）技术综述

RAG，Retrieval-Augmented Generation，也被称作检索增强生成技术，最早在 Facebook AI（Meta AI）在 2020 年发表的论文《Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks》（ https://arxiv.org/abs/2005.11401 ）中正式提出，这种方法的核心思想是借助一些文本检索策略，让大模型每次问答前都带入相关文本，以此来改善大模型回答时的准确性。这项技术刚发布时并未引发太大关注，而伴随2022年大模型技术大爆发，RAG技术才逐渐进入人们视野，并且由于早期大模型技术应用均已“知识库问答”为主，而RAG技术是最易上手、并且上限极高的技术，因此很快就成为了大模型技术人必备的技术之一。

### 1. RAG技术极简实现流程

时至今日，RAG技术已经是非常庞大的技术体系了，从简答的文档切分、存储、匹配，再到复杂的入GraphRAG（基于知识图谱的检索增强），以及复杂文档解析+多模态识别技术等等等等。

![](img/1ba4ead5e66a876f5377663faea8fe01.png)

而对于初学者来说，为了更好的上手学习RAG技术，我们首先需要对RAG技术最简单的实现形式有个基础的了解。一个最简单的RAG技术实现流程如下所示：

![](img/21ac171e193d35f29899a0a1019e3210.png)

我们需要围绕给定的文档（往往是非常长的文档）先进行切分，然后将切分的文档转化为计算机能识别的形式，也就是将其转化为一个数值型向量（也被称为词向量），然后当用户询问问题的时候，我们再将用户的问题转化为词向量，并和段落文档的词向量进行相似度匹配，借此找出和当前用户问题最相关的原始文档片段，然后将用户的问题和匹配的到的原文片段都带入大模型，进行最终的问答。由此便可实现一次完整的文档检索增强执行流程。a

具体执行过程如下所示：

![](img/3ce5a4d2d704695fdee0f6685cf31d6a.png)

### 3. RAG技术核心应用场景：拓展模型知识边界与减少问答幻觉

那这样的一个检索增强流程到底有什么用呢？这就不得不从当代大模型本身的三项技术缺陷开始说起了。

+   缺陷一：大模型幻觉

相信大家在使用大模型的时候，都会遇到大模型无中生有胡编乱造答案的情况，例如胡乱生成一些概念、一些论文甚至是一些实时等，这就是所谓的大模型幻觉。

![](img/e432778f21fdaa47c4bd5cc1066c571f.png)

而其中，第一代DeepSeek R1模型的幻觉是非常严重的，平均七次回答中就会有一次的回答存在幻觉，这可以说是第一版R1模型最大的短板。

大型语言模型之所以会产生幻觉，主要是因为它们的训练方式和内在机制决定了它们并不具备真正理解和验证事实的能力。模型在训练过程中，通过分析大规模文本数据来学习不同词语和句子之间的概率关系，也就是在某种程度上掌握“在什么上下文中，什么样的回答听起来更合理”。然而，模型并没有接入实时的知识库或事实核查工具，当它遇到陌生的问题、模糊的描述或者上下文不完整的输入时，就会基于概率和语料库中似是而非的关联去“编造”一个看似正确的答案。由于这些输出往往语法流畅、逻辑连贯，人类读者很容易误以为它是真实可信的内容，这就是我们通常说的“模型幻觉”。

+   缺陷二：有限的最大上下文

而除此之外，大模型在实际应用中还会另一个“障碍”，那就是最大上下文限制。由于大模型的本质其实是一个算法，不管是让大模型“知道”有哪些外部工具，还是要给大模型进行“背景设置”，或者是要给模型添加历史对话消息，以及本次对话的输出，都需要占用这个上下文窗口。这就使得我们在一次对话中能够给大模型灌输的知识（文本）其实是有限的。

大型语言模型还存在最大上下文限制，这是由它们的架构和计算方式决定的。每次生成回答时，模型需要把输入文本转换成固定长度的数字序列（称为token），并在内部一次性加载到模型的“上下文窗口”中进行处理。这个窗口的大小是有限的，不同模型一般在几千到几万token之间。如果输入内容超出这个长度，模型要么截断最前面的部分，要么丢弃部分信息，这就会造成对话历史、长文档或先前提到的重要细节的遗失。因为它无法跨越上下文窗口无限地保留信息，所以在面对长对话或者大量背景知识时，模型常常出现上下文断裂、回答不连贯或者忽略先前条件的情况。

早些时候的大模型普遍是8k最大上下文，相当于是8-10页中文PDF，伴随着大模型预训练技术的不断发展，顶尖的大模型，如Gemini 2.5 Pro和GPT-4.1等模型，已经达到了1M的最大上下文长度，相当于是一千页的PDF，相当于1.5本《红楼梦》，而普通的模型，也基本达到64K或128K最大上下文，相当于60-100也左右的PDF。

![](img/85b1317f5f68e2076528c61957b617e0.png)

但是，模型上下文的增长也是有限度的，对于开发者来说，能够一次性输入的信息都会有限制。

+   缺陷三：模型专业知识与时效性知识不足

大型语言模型虽然在通用领域展现出令人瞩目的语言理解和生成能力，但其在特定领域的专业知识掌握往往存在明显局限。其根本原因在于，模型的训练依赖于预先收集的大规模语料，这些语料覆盖面虽广，却很难保证在所有专业领域中具有足够的深度和准确性。某些领域，如医学、法律或前沿科技，知识更新速度快且门槛较高，公开可获取的高质量数据本身就有限，模型难以在此基础上形成系统性和权威性的认知。此外，模型训练通常在固定的时间点结束，因此其所掌握的知识具有天然的时效性，无法实时反映新近出现的研究成果、政策变化或行业动态。这种静态的知识存储模式，决定了大模型在面对最新或高度专业化的问题时，往往难以提供全面、精确的解答。

![](img/a5180086ece7508b865a2412f1c289d0.png)

基于此，我们再回顾RAG的技术实现流程，就不难发现其背后的技术价值了：如果我们能在每次对话的时候，为当前模型输入最精准的问题相关的文档，那就能拓展模型的知识边界，无论是提升模型专业知识的准确性、给模型灌输一些时效性的知识、或者消除模型幻觉，都将大有助益，而在其他一些对话场景中，无论是需要围绕海量的文本搭建本地问答知识库、还是在构建无限上下文的聊天机器人，RAG技术都是最佳解决方案。

### 4. 问答机器人标配：RAG系统

正因为知识库检索的广泛的使用需求，RAG技术几乎成了现在各项聊天机器人的标配，无论是面向普通用户的聊天问答应用Cherry Studio：

![](img/a2b6a98afac86c898575cf4832f533db.png)

还是面向企业应用场景的通用开源前端Open-WebUI:

![](img/aad55fdce2b11325be9a6b5bd2377c8e.png)

都毫无例外都配置了RAG功能，而对于OpenAI-WebUI这种企业级前端，还为用户展示了RAG检索过程诸多技术细节：

![](img/e182b0ca2c43f730ba19371f42eb9991.png)

尽管这些项目能让用户更加快速的使用RAG系统，但这种传统的RAG流程（也被称作Native RAG），在长期的应用过程中也逐渐展露出很多问题，例如对于非结构化的文本（例如包含图片、公式的文本）无法进行检索，而对于超大规模文本的检索又会存在精度不足、或者无法提炼总结跨文本概念等问题。为此，近两年的时间里，在无数技术人的共同努力下，RAG技术有了长足的成长和突破。

我们团队自研的开源Jupyter智能体助教MateGen Air，也提供了完整的公开课（部分）知识库问答功能：

![](img/43bf576e5bb34deea5bf570ad270aabe.png)

![](img/6eaa662c2da04d471f4c68b208d3e4d2.png)

### 5. RAG全栈技术体系介绍

但是，就像前文介绍的那样，RAG技术是一项应用面广、门槛很低、但同时上限也很高的一项技术。历经数年的技术发展，RAG技术的体系已经非常庞大，以下是RAG技术全栈技术框架概览：

![](img/e31ab2d67df17b2fbbac96a82868041b.png)

+   GraphRAG

**G****raphRAG（**Graph-enhanced Retrieval-Augmented Generation） 是一种在经典 RAG 基础上引入知识图谱/图结构的新型检索生成方法 。其核心思想是通过将文档或数据转换成图的形式，从而捕捉实体与实体之间的语义关系，并在检索阶段利用图遍历、关系推理等机制来辅助上下文构建，这种结构化信息能够提升语义理解和多跳推理能力。

具体来说，GraphRAG 的流程包括：

1.  **图谱构建**：将文本拆分为多个单元（TextUnit），提取实体与关系，构造知识图，并进行图社区检测与摘要；

1.  **混合检索**：用户提问既可以进行向量检索定位实体，也可以通过图查询（如 Cypher/SPARQL）沿关系边扩展信息 ；

1.  **图增强生成**：将检索到的节点、路径、社区摘要等信息拼接进 Prompt，引导 LLM 生成更准确、结构清晰、并基于事实推理的回答。

| 对比维度 | 传统 RAG | GraphRAG |
| --- | --- | --- |
| 检索方式 | 基于向量语义相似度 | 向量+知识图遍历/查询 |
| 关系理解能力 | 弱：只能匹配语义相近片段 | 强：能理解实体之间的多跳关系与结构 |
| 多跳推理支持 | 弱：难以综合跨文档信息 | 强：图结构天然支持推理路径遍历 |
| 语义上下文覆盖 | 依赖检索片段 | 可检索完整实体子图、社区摘要 |
| 可解释性 | 中：返回片段但缺关键信息结构 | 高：能显示实体关系路径及社区结构 |
| 性能/复杂度 | 低：直接使用向量库 | 高：需要图构建、遍历、摘要等pipeline |

传统 RAG 主要是“先检索语义近似片段，再生成回答”，适合简单查询与短对话。但当问题需要“连接多个事实”“推理关系链”和“洞察上下文结构”时，传统 RAG 会显得力不从心，而 GraphRAG 正是为复杂推理场景设计的增强机制。

+   Agentic RAG

**Agentic RAG（Agentic Retrieval-A****ugment**e**d Generation**） 是一种在传统 RAG 基础上进一步扩展的增强范式，它将检索增强生成与Agent（智能体）能力有机结合，使大模型不仅能够基于外部知识库进行回答，还能够通过一系列自主决策和工具调用来完成复杂任务。与经典 RAG 的“检索+拼接+生成”线性流程不同，Agentic RAG 将 LLM 视为一个具备推理、规划和操作能力的智能体，它在对话过程中可以根据问题拆解子任务，先后执行多轮检索、知识整合、函数调用甚至外部API请求，再将结果动态组合成最终的答案。

在这个模式下，大模型可以主动提出接下来的检索需求，或根据中间推理结果迭代获取更多信息，形成“循环式检索与生成”的闭环工作流。例如，当用户提出复杂查询时，Agentic RAG可以先调用检索工具定位候选内容，再使用工具对结果进行归纳或分类，必要时还会触发计算或外部查询操作，最后再汇总所有信息输出一个有依据的、分步骤的解答。

相比传统RAG，Agentic RAG不仅提升了回答准确性和透明度，也为多轮推理和跨知识库整合提供了更强的灵活性，是近年来大模型产品中非常重要的能力演进方向。

### 6. RAG技术实际落地应用困局

#### 6.1 “易于上手，难于精通”的技术悖论

检索增强生成（RAG）技术展现出典型的“低门槛，高天花板”特征。在原型验证（PoC）阶段，通过标准的“切分-向量化-检索-生成”流程，开发者往往能在数小时内构建出一个可运行的 Demo。

然而，当系统从演示环境走向企业级生产环境时，边际效益会迅速递减。尽管市面上已经涌现出如 MaxKB、RAGFlow 等优秀的开箱即用型 RAG 产品，它们在处理通用知识库时表现出色，但在面对企业高度定制化、非标准化的业务场景时，往往显得力不从心。

企业级应用的核心挑战在于：**业务需求千差万别，不存在“一套通吃（One-siz**e-fits-all）”的 RAG 架构。为了在特定领域达到生产可用的准确率（通常要求 90% 以上），必须深入数据层进行定制化开发与调优。

![](img/a4fb46546d5b25fc1143ff4a5918d094.png)

#### 6.2 当前 RAG 系统的核心技术瓶颈

在实际工程实践中，通用的 RAG 架构主要面临以下三大难以逾越的障碍：

+   痛点一：系统效果的黑盒化与评估难题

+   **缺乏量化标准：** 与传统软件工程不同，RAG 系统的输出具有概率性。缺乏“标准答案（Ground Truth）”使得开发者难以对检索准确率（Retrieval Accuracy）和生成忠实度（Generation Faithfulness）进行客观量化。

+   **归因困难：** 当模型回答错误时，很难快速界定是检索环节漏掉了关键文档（Recall Issue），还是大模型本身的推理能力不足（Reasoning Issue），或是 Prompt 设计不当。这导致优化工作如同“盲人摸象”，缺乏迭代方向。

+   痛点二：大规模语料下的“迷失”与推理局限

+   **精度与规模的矛盾：** 随着文档数量从几百篇增加到数万篇，简单的 Top-K 向量检索容易引入大量噪声，导致“大海捞针（Needle in a Haystack）”效应，关键信息被淹没。

+   **跨文档推理能力的缺失：** **传统 RAG 将文档切碎成独立的片段（Chunks**），丢失了文档间的逻辑关联。当用户问题需要综合 A 文档的条件和 B 文档的结论进行多跳推理（Multi-hop Reasoning）时，或者需要对整个文集进行综合性主题概括时，基于片段匹配的 RAG 系统往往表现堪忧。

+   痛点三：高价值数据的结构性丢失（多模态与表格）

+   **表格数据的语义坍塌：** 企业财报、技术手册中包含大量表格。传统的文本切分方式会将表格打散成无意义的字符串，导致行列对应关系丢失，LLM 无法进行精准的数据查询与对比。

+   **多模态信息的盲区：** 传统的 RAG 流程通常会丢弃 PDF 中的图表、架构图和截屏。然而，在工业制造、医疗诊断等场景中，核心知识往往蕴含在图片而非文字中，这造成了巨大的信息资产浪费。

## 二、Llama-Index框架入门介绍

### 1. Llama-Index 项目介绍

+   Llama-Index官方网站：https://www.llamaindex.ai/

![](img/af4707cca0654ee0764cffeaaf29643f.png)

+   Llama-Index GitHub项目主页：https://github.com/run-llama/llama_index

![](img/ae249f0607a3e4441d77cb1150b6e6eb.png)

Llama-Index（前身为 GPT Index）由 Jerry Liu 于 2**022 年底发起。项目的诞生背景源于大语言模型（LLM）落地初期的一个核心痛点：模型虽然具备强**大的通用推理能力，但缺乏特定领域的私有知识（Private Knowledge），且受限于上下文窗口（Context Window）的长度，无法处理大规模文档。

Llama-Index 最初的初衷非常纯粹：构建一个高效的“接口”，将用户的私有数据（如 PDF、Notion、SQL、API 数据）转化为 LLM 能够理解和利用的格式，从而通过上下文学习（In-context Learning）来增强模型的能力。经过两年的高速迭代，它已从一个简单的索引工具演变为一个完整的**“数据驱动型”大模型应用开发框架**。

如果把大模型比作**“大脑”，Llam**a-Index 的定位就是“记忆增强系统”。它的官方定义是：“用于构建上下文增强型（Context-augmented）LLM 应用程序的数据框架”**。

它不仅仅关注于如何向模型提问（Prompt Engineering），更侧重于数据管理（Data Management）的全生命周期，包括数据的摄入（Ingestion）、结构化索引（Indexing）以及高效检索（Retrieval）。其核心目标是打破私有数据孤岛，让 LLM 能够以最低的成本、最高的精度访问海量外部知识。

目前，Llama-Index 是全球 AI 开源社区中最活跃的项目之一。

+   **技术迭代：** 保持着极高的更新频率，已发布 v0.10+ 版本，完成了核心架构的模块化重构，具备了企业级生产环境所需的稳定性。

+   **生态系统：** 拥有庞大的 **LlamaHub** 数据加载器生态，支持数百种数据源（从文件系统到 SaaS 服务）的开箱即用连接；同时支持 Python 和 TypeScript 双语言版本。

在 LLM 开发领域，通过对比可以更清晰地理解两者的差异：

+   LangChain（通用编排者）：

+   **定位：** 通用的 LLM 应用开发框架。

+   **强项：** 侧重于**“计算逻辑”**的编排。它擅长管理复杂的 Agent 行为链、工具调用（Tool Usage）以及多模态交互的流程控制。它像是一个“胶水”，连接模型与万物。

+   Llama-Index（数据专家）：

+   **定位：** 专注于数据处理与检索的垂直框架。

+   **强项：** 侧重于**“数据结构”**的优化。它在 RAG 领域拥有更深的护城河，特别是在处理非结构化数据切片、层级索引构建、复杂查询路由等方面提供了更精细的控制能力。

相较于其他框架，Llama-Index 在 RAG（检索增强生成）方面展现出显著的专业性优势：

1.  **更丰富的数据索引结构：** 不仅支持简单的向量索引，还原生支持树状索引、关键词表索引、知识图谱索引等多种结构。

1.  **更高级的检索策略：** 提供了递归检索（Recursive Retrieval）、混合检索（Hybrid Search）及元数据过滤等开箱即用的高级功能。

1.  **数据与 LLM 的深度对齐：** 能够更好地处理长文档摘要、跨文档推理等复杂的数据任务。

![](img/0b2683ff4fab79248c9af5a9d357cdce.png)

### 2. Llama-Index RAG核心优势

2.1 RAG 全生命周期（Full-Lifecycle）的闭环支持

与许多仅关注“向量检索”单一环节的工具不同，Llama-Index 提供了从数据源头到最终评估的 RAG 全流程解决方案。它将 RAG 系统抽象为五个标准化的流水线环节，开发者可以在同一个框架内完成所有工作，无需拼凑多个零散的工具库。

+   **数据加载 (Loading)：** 提供统一接口将各类非结构化数据转化为标准的 Document 对象。

+   索引构建 (Indexing)： 将文档切分（Chunking）并向量化（Embedding），构建出不仅包含向量、还包含元数据（Metadata）和节点关系（Relationships）的高级索引结构。

+   **存储 (Storing)：** 原生适配数十种主流向量数据库（如 Chroma, Weaviate, Milvus）及图数据库，支持索引的持久化与增量更新。

+   **查询 (Querying)：** 这一环节是 Llama-Index 的核心，它封装了检索、后处理和合成的复杂逻辑，对外提供简洁的查询接口。

+   评估 (Evaluation)： 独有的 "Evaluation-First" 理念，内置了基于 LLM 的评估模块，能够对检索的准确性（Retrieval metrics）和生成的质量（Response metrics）进行自动化评分，解决了 RAG 难以优化的痛点。

![](img/bc0a1ab8800b3ad4386741bb62c5edb3.png)

2.2 极致的定制化空间与丰富的功能接口

Llama-Index 最大的架构优势在于其模块化（Modularity）设计。它将 RAG 的每个步骤都解耦为可插拔的组件，特别是在查询引擎（Query Engine）环节，提供了极高的定制自由度：

+   高级后处理 (Post-Processing)：
在检索与生成之间，Llama-Index 允许开发者插入“节点后处理器（Node Postprocessors）”。这使得我们可以轻松实现：

+   重排序（Re-ranking）： 集成 Cohere Rerank 或 BGE Rerank 模型，对检索到的 Top-K 结果进行二次精排，大幅提升相关性。

+   元数据过滤（Metadata Filtering）： 基于时间、作者或文件类型过滤节点。

+   相似度截断（Similarity Cutoff）： 自动丢弃相似度低于阈值的噪声数据。

+   灵活的响应合成 (Response Synthesis)：
针对不同的业务场景，提供了多种内置的合成策略，而非单一的拼接：

+   **Refine（精炼模式）：** 线性遍历检索结果，逐步迭代优化答案，适合生成详尽的回答。

+   Tree Summarize（树状总结）： 自底向上构建摘要树，适合处理海量上下文的归纳总结任务。

+   **Compact（紧凑模式）：** 最大化利用 Context Window，平衡速度与成本。

![](img/9a50e186720175e7529b06051a446b81.png)

2.3 庞大的数据生态：LlamaHub

Llama-Index 拥有目前** AI 社区中最**丰富的数据连接器生态——LlamaHub。它解决了 RAG 开发中“最后一公里”的数据获取难题，让开发者无需自行编写爬虫或解析脚本。

+   海量数据加载器 (Data Loaders)：
社区维护了超过 400 种数据加载器，覆盖了几乎所有主流数据源：

+   **文件类：** PDF, Markdown, PowerPoint, Word, Excel, CSV 等。

+   **SaaS 类：** Notion, Slack, Discord, Jira, Salesforce, Google Docs 等。

+   **网络类：** Wikipedia, YouTube Transcripts, Web Page Reader 等。

+   **数据库类：** PostgreSQL, MongoDB, SQL Database 等。

+   **多模态原生支持：**
除了文本，LlamaHub 还提供了针对图像、音频和视频的加载器，支持构建图文混排的 RAG 系统，能够将多模态信息统一映射到向量空间。

![](img/b1c7f03dcc8d4a6206ef627262454498.png)

2.4 企业级云服务：LlamaCloud

为了满足企业对高性能和免运维的需求，Llama-Index 团队还推出了配套的商业化云服务，进一步补全了开源框架的拼图：

+   **LlamaParse：**
**这是目前业界最先进的文档**解析**服务之一**。专为解决 RAG 中的“复杂文档处理”难题而生，能够精准识别和解析 PDF 中的复杂表格、图表、数学公式以及多栏排版，将其转化为 LLM 易于理解的 Markdown 格式，从源头上提升了 RAG 的检索质量。

+   Managed Indexing：
提供“检索即服务（Retrieval-as-a-Service）”，开发者无需自行维护向量数据库和索引管道，通过 API 即可直接上传文件并进行高效检索，大幅降低了企业级 RAG 的落地门槛。

![](img/52c746f14080491419a9af608cf32605.png)

### 3.Llama-Index RAG 黑科技

在 Llama-Index 的工具箱中，隐藏着几种能够显著提升 RAG 性能的“杀手级”特性。这些技术突破了传统 RAG 的线性流程，引入了逻辑推理、代码执行和多模态能力。

#### 3.1 Small-to-Big Retrieval（小索引，大窗口）

+   **痛点：** 传统 RAG 直接对切片进行检索。切片太小，上下文缺失（LLM 看不懂）；切片太大，包含过多噪声（检索不准）。

+   **黑科技原理：** **“索引粒度”与“生成粒度”分离**。
系统将文档切分成极小的句子（Sentence）进行精准检索，但在提交给 LLM 时，自动将该句子替换为它周围更大的文本窗口（Window）。既保证了检索的高精度（High Precision），又提供了生成所需的丰富上下文（Rich Context）。

+   **伪代码示例：**

```
# 1. 使用窗口解析器，记录每句话的“上下文窗口”
node_parser = SentenceWindowNodeParser.from_defaults(
    window_size=3  # 检索到一句话，同时带出前后各3句话
)

# 2. 在查询时，启用元数据替换功能
query_engine = index.as_query_engine(
    node_postprocessors=[MetadataReplacementPostProcessor(...)]
)
```

#### 3.2 Pandas Query Engine（结构化数据RAG）

+   **痛点：** 用户问：“计算 Q3 销售额的总和。” 传统 RAG 会检索出一堆包含数字的文本，让 LLM 自己算，极易出现数学幻觉（Math Hallucination）。

+   **黑科技原理：** 将“检索任务”转化为“编程任务”。
Llama-Index 不去搜索文本，而是让 LLM 编写一段 Pandas Python 代码，并在本地沙箱中执行。这相当于让 RAG 拥有了类似 ChatGPT Code Interpreter 的能力，实现 100% 准确的数据计算。

+   **伪代码示例：**

```
import pandas as pd
from llama_index.experimental import PandasQueryEngine

df = pd.read_excel(&quot;sales_data.xlsx&quot;)

# 引擎会自动生成代码：df[df[&#x27;quarter&#x27;]==&#x27;Q3&#x27;][&#x27;sales&#x27;].sum() 并执行
query_engine = PandasQueryEngine(df=df, verbose=True)
response = query_engine.query(&quot;Q3 的销售总额是多少？&quot;)
```

#### 3.3 Router Query Engine（智能路由分诊）

+   **痛点：** 系统中既有非结构化的 PDF（适合语义检索），又有结构化的 SQL 数据库（适合精确查询）。用户提问时，系统不知道该查哪一个。

+   **黑科技原理：** **RAG 的“前置大脑”**。
Router 是一个轻量级的分类器（Agent），它会先分析用户意图，然后动态决定将问题路由给“向量检索引擎”还是“SQL 查询引擎”，甚至同时查询两者。

+   **伪代码示例：**

```
from llama_index.core.tools import QueryEngineTool
from llama_index.core.query_engine import RouterQueryEngine

# 定义两个工具：一个查文档，一个查摘要
summary_tool = QueryEngineTool.from_defaults(query_engine=summary_engine, description=&quot;查摘要&quot;)
vector_tool = QueryEngineTool.from_defaults(query_engine=vector_engine, description=&quot;查细节&quot;)

# 智能路由：根据问题自动选择用哪个工具
query_engine = RouterQueryEngine(
    selector=LLMSingleSelector.from_defaults(),
    query_engine_tools=[summary_tool, vector_tool]
)
```

#### **3.4 Sub-Question **Query Engine（多子问题查询）

+   **痛点：** 用户问：“对比 A 公司和 B 公司的营收增长率。” 这是一个多跳推理（Multi-hop）问题，单次检索往往只能搜到其中一家的数据，导致回答不全。

+   **黑科技原理：** 分而治之（Divide and Conquer）。
引擎会自动将这个复杂问题拆解为三个子问题：（1）查 A 公司营收；（2）查 B 公司营收；（3）对比结果。系统并发执行查询，最后汇总答案。

+   **伪代码示例：**

```
from llama_index.core.query_engine import SubQuestionQueryEngine

# 引擎运行时会打印：
# &gt; Generated sub-question: What is the revenue of Company A?
# &gt; Generated sub-question: What is the revenue of Company B?
query_engine = SubQuestionQueryEngine.from_defaults(
    query_engine_tools=tools
)
```

#### 3.5 Native Multimodal RAG（原生多模态检索）

+   **痛点：** PDF 中的架构图、流程图或产品照片被传统 OCR 忽略，导致关键信息丢失。

+   **黑科技原理：** **CLIP 跨模态对齐**。
利用 CLIP 模型将文本和图像映射到同一个向量空间。无需 OCR 转文字，系统可以直接用“文字”检索到“图片”，并将图片直接喂给 GPT-4o 或 LLaVA 等多模态大模型进行理解。

+   **伪代码示例：**

```
from llama_index.core.indices import MultiModalVectorStoreIndex

# 建立包含文本和图片的混合索引
index = MultiModalVectorStoreIndex.from_documents(documents)

# 检索结果包含 ImageNode，可直接展示图片或喂给视觉模型
retriever = index.as_retriever(image_similarity_top_k=2)
```

## 三、Llama-Index快速上手指南

### 1.基本环境搭建与快速运行

课程开始之前，我们需要构建一个干净、独立的 Python 开发环境。Llama-Index 迭代速度极快，使用虚拟环境可以避免与您本地的其他项目产生依赖冲突。

#### 1.1 创建 Python 虚拟环境

推荐使用 Conda 进行环境管理（也可以使用 Python 自带的 venv）。我们将创建一个名为 llama-index-demo 的环境，并指定 Python 版本为 3.10 或以上（Llama-Index 对新特性支持较好）。

+   **打开终端（Terminal）或 Ana**conda Prompt，执行以下命令：

```
# 1. 创建虚拟环境
conda create -n llamaindex python=3.12 -y

# 2. 激活虚拟环境
conda activate llamaindex

# 3.安装jupyter kernel
conda install jupyterlab
conda install ipykernel
python -m ipykernel install --user --name llamaindex --display-name &quot;Python (llamaindex)&quot;

# 4.启动jupyter
jupyter lab
```

#### 1.2 安装 Llama-Index 核心库

自 v0.10.0 版本起，Llama-Index 采用了模块化架构。对于初学者，我们直接安装主包，它会自动包含核心组件（Core）以及常用的 OpenAI 适配器。

+   **执行安装命令：**

```
# 安装 Llama-Index 主包
pip install llama-index -i https://pypi.tuna.tsinghua.edu.cn/simple

# 安装环境变量管理工具 (用于加载 .env 文件)
pip install python-dotenv -i https://pypi.tuna.tsinghua.edu.cn/simple
```

是一个包含以下软件包的入门级捆绑包

+   llama-index-core

+   llama-index-llms-openai

+   llama-index-embeddings-openai

+   llama-index-readers-file

注意： llama-index-core已预先捆绑 NLTK 和 tiktoken 文件，以避免运行时下载和网络调用。

#### 1.3 配置 OpenAI API Key

Llama-Index 默认使用 OpenAI 的 GPT-3.5-turbo 进行推理以及 text-embedding-ada-002 进行向量化。虽然框架支持更换任意模型，但在快速上手阶段，使用 OpenAI 是最稳定、调试最方便的选择。

为了安全起见，我们严禁将 API Key 直接硬编码在代码中。我们将采用业界标准的 .env 环境变量文件进行配置。

+   **步骤 1：** 在项目根目录下，创建一个名为 .env 的新文件（注意文件名前面有个点，且没有后缀名）。

+   **步骤 2：** 使用文本编辑器打开该文件，并写入您的 API Key：

```
# .env 文件内容示例
OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
BASE_URL=sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

![](img/5cf8c1077f7b1351fb6b2fec8da7bf79.png)

![](img/2d9fe9f96ab93268ee0661980969f77d.png)

#### **1.4 验证安装**

```
import os
from dotenv import load_dotenv
import llama_index.core
```

```
# 1. 加载环境变量
load_dotenv()

# 2. 检查 Llama-Index 版本
print(f&quot;Llama-Index Version: {llama_index.core.__version__}&quot;)
```

```
Llama-Index Version: 0.14.10
```

+   模型调用测试

```
from openai import OpenAI
client = OpenAI(
base_url=os.getenv(&quot;BASE_URL&quot;),
api_key=os.getenv(&quot;OPENAI_API_KEY&quot;),
)
# 查看模型列表
model_list = client.models.list()
```

```
model_list.data
```

```
[Model(id=&#x27;gpt-4-0613&#x27;, created=1686588896, object=&#x27;model&#x27;, owned_by=&#x27;openai&#x27;),
 Model(id=&#x27;gpt-4&#x27;, created=1687882411, object=&#x27;model&#x27;, owned_by=&#x27;openai&#x27;),
 Model(id=&#x27;gpt-3.5-turbo&#x27;, created=1677610602, object=&#x27;model&#x27;, owned_by=&#x27;openai&#x27;),
 Model(id=&#x27;gpt-5.1-codex-max&#x27;, created=1763671532, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5.1-2025-11-13&#x27;, created=1762800353, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5.1&#x27;, created=1762800673, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5.1-codex&#x27;, created=1762988221, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5.1-codex-mini&#x27;, created=1763007109, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;davinci-002&#x27;, created=1692634301, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;babbage-002&#x27;, created=1692634615, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-3.5-turbo-instruct&#x27;, created=1692901427, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-3.5-turbo-instruct-0914&#x27;, created=1694122472, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;dall-e-3&#x27;, created=1698785189, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;dall-e-2&#x27;, created=1698798177, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4-1106-preview&#x27;, created=1698957206, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-3.5-turbo-1106&#x27;, created=1698959748, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;tts-1-hd&#x27;, created=1699046015, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;tts-1-1106&#x27;, created=1699053241, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;tts-1-hd-1106&#x27;, created=1699053533, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;text-embedding-3-small&#x27;, created=1705948997, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;text-embedding-3-large&#x27;, created=1705953180, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4-0125-preview&#x27;, created=1706037612, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4-turbo-preview&#x27;, created=1706037777, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-3.5-turbo-0125&#x27;, created=1706048358, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4-turbo&#x27;, created=1712361441, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4-turbo-2024-04-09&#x27;, created=1712601677, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o&#x27;, created=1715367049, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-2024-05-13&#x27;, created=1715368132, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini-2024-07-18&#x27;, created=1721172717, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini&#x27;, created=1721172741, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-2024-08-06&#x27;, created=1722814719, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;chatgpt-4o-latest&#x27;, created=1723515131, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-audio-preview&#x27;, created=1727460443, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-realtime-preview&#x27;, created=1727659998, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;omni-moderation-latest&#x27;, created=1731689265, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;omni-moderation-2024-09-26&#x27;, created=1732734466, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-realtime-preview-2024-12-17&#x27;, created=1733945430, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-audio-preview-2024-12-17&#x27;, created=1734034239, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini-realtime-preview-2024-12-17&#x27;, created=1734112601, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini-audio-preview-2024-12-17&#x27;, created=1734115920, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o1-2024-12-17&#x27;, created=1734326976, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o1&#x27;, created=1734375816, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini-realtime-preview&#x27;, created=1734387380, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini-audio-preview&#x27;, created=1734387424, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;computer-use-preview&#x27;, created=1734655677, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o3-mini&#x27;, created=1737146383, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o3-mini-2025-01-31&#x27;, created=1738010200, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-2024-11-20&#x27;, created=1739331543, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;computer-use-preview-2025-03-11&#x27;, created=1741377021, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-search-preview-2025-03-11&#x27;, created=1741388170, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-search-preview&#x27;, created=1741388720, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini-search-preview-2025-03-11&#x27;, created=1741390858, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini-search-preview&#x27;, created=1741391161, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-transcribe&#x27;, created=1742068463, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini-transcribe&#x27;, created=1742068596, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o1-pro-2025-03-19&#x27;, created=1742251504, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o1-pro&#x27;, created=1742251791, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-mini-tts&#x27;, created=1742403959, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o3-2025-04-16&#x27;, created=1744133301, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o4-mini-2025-04-16&#x27;, created=1744133506, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o3&#x27;, created=1744225308, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o4-mini&#x27;, created=1744225351, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4.1-2025-04-14&#x27;, created=1744315746, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4.1&#x27;, created=1744316542, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4.1-mini-2025-04-14&#x27;, created=1744317547, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4.1-mini&#x27;, created=1744318173, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4.1-nano-2025-04-14&#x27;, created=1744321025, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4.1-nano&#x27;, created=1744321707, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-image-1&#x27;, created=1745517030, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;codex-mini-latest&#x27;, created=1746673257, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-realtime-preview-2025-06-03&#x27;, created=1748907838, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-audio-preview-2025-06-03&#x27;, created=1748908498, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o4-mini-deep-research&#x27;, created=1749685485, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-4o-transcribe-diarize&#x27;, created=1750798887, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;o4-mini-deep-research-2025-06-26&#x27;, created=1750866121, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-chat-latest&#x27;, created=1754073306, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-2025-08-07&#x27;, created=1754075360, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5&#x27;, created=1754425777, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-mini-2025-08-07&#x27;, created=1754425867, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-mini&#x27;, created=1754425928, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-nano-2025-08-07&#x27;, created=1754426303, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-nano&#x27;, created=1754426384, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-audio-2025-08-28&#x27;, created=1756256146, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-realtime&#x27;, created=1756271701, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-realtime-2025-08-28&#x27;, created=1756271773, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-audio&#x27;, created=1756339249, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-codex&#x27;, created=1757527818, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-image-1-mini&#x27;, created=1758845821, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-pro-2025-10-06&#x27;, created=1759469707, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-pro&#x27;, created=1759469822, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-audio-mini&#x27;, created=1759512027, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-audio-mini-2025-10-06&#x27;, created=1759512137, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-search-api&#x27;, created=1759514629, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-realtime-mini&#x27;, created=1759517133, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-realtime-mini-2025-10-06&#x27;, created=1759517175, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;sora-2&#x27;, created=1759708615, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;sora-2-pro&#x27;, created=1759708663, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5-search-api-2025-10-14&#x27;, created=1760043960, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-5.1-chat-latest&#x27;, created=1762547951, object=&#x27;model&#x27;, owned_by=&#x27;system&#x27;),
 Model(id=&#x27;gpt-3.5-turbo-16k&#x27;, created=1683758102, object=&#x27;model&#x27;, owned_by=&#x27;openai-internal&#x27;),
 Model(id=&#x27;tts-1&#x27;, created=1681940951, object=&#x27;model&#x27;, owned_by=&#x27;openai-internal&#x27;),
 Model(id=&#x27;whisper-1&#x27;, created=1677532384, object=&#x27;model&#x27;, owned_by=&#x27;openai-internal&#x27;),
 Model(id=&#x27;text-embedding-ada-002&#x27;, created=1671217299, object=&#x27;model&#x27;, owned_by=&#x27;openai-internal&#x27;)]
```

### 2.【选学】LLama-Index创建Agent功能介绍

```
import asyncio
from llama_index.core.agent.workflow import FunctionAgent
from llama_index.llms.openai import OpenAI
```

```
# Define a simple calculator tool
def multiply(a: float, b: float) -&gt; float:
    &quot;&quot;&quot;Useful for multiplying two numbers.&quot;&quot;&quot;
    return a * b
```

```
OpenAI?
```

```
[31mInit signature:[39m
OpenAI(
    model: str = [33m&#x27;gpt-3.5-turbo&#x27;[39m,
    temperature: float = [32m0.1[39m,
    max_tokens: Optional[int] = [38;5;28;01mNone[39;00m,
    additional_kwargs: Optional[Dict[str, Any]] = [38;5;28;01mNone[39;00m,
    max_retries: int = [32m3[39m,
    timeout: float = [32m60.0[39m,
    reuse_client: bool = [38;5;28;01mTrue[39;00m,
    api_key: Optional[str] = [38;5;28;01mNone[39;00m,
    api_base: Optional[str] = [38;5;28;01mNone[39;00m,
    api_version: Optional[str] = [38;5;28;01mNone[39;00m,
    callback_manager: Optional[llama_index.core.callbacks.base.CallbackManager] = [38;5;28;01mNone[39;00m,
    default_headers: Optional[Dict[str, str]] = [38;5;28;01mNone[39;00m,
    http_client: Optional[httpx.Client] = [38;5;28;01mNone[39;00m,
    async_http_client: Optional[httpx.AsyncClient] = [38;5;28;01mNone[39;00m,
    openai_client: Optional[openai.OpenAI] = [38;5;28;01mNone[39;00m,
    async_openai_client: Optional[openai.AsyncOpenAI] = [38;5;28;01mNone[39;00m,
    system_prompt: Optional[str] = [38;5;28;01mNone[39;00m,
    messages_to_prompt: Optional[Callable[[Sequence[llama_index.core.base.llms.types.ChatMessage]], str]] = [38;5;28;01mNone[39;00m,
    completion_to_prompt: Optional[Callable[[str], str]] = [38;5;28;01mNone[39;00m,
    pydantic_program_mode: llama_index.core.types.PydanticProgramMode = &lt;PydanticProgramMode.DEFAULT: [33m&#x27;default&#x27;[39m&gt;,
    output_parser: Optional[llama_index.core.types.BaseOutputParser] = [38;5;28;01mNone[39;00m,
    strict: bool = [38;5;28;01mFalse[39;00m,
    reasoning_effort: Optional[Literal[[33m&#x27;low&#x27;[39m, [33m&#x27;medium&#x27;[39m, [33m&#x27;high&#x27;[39m]] = [38;5;28;01mNone[39;00m,
    modalities: Optional[List[str]] = [38;5;28;01mNone[39;00m,
    audio_config: Optional[Dict[str, Any]] = [38;5;28;01mNone[39;00m,
    *,
    query_wrapper_prompt: Optional[llama_index.core.prompts.base.BasePromptTemplate] = [38;5;28;01mNone[39;00m,
    logprobs: Optional[bool] = [38;5;28;01mNone[39;00m,
    top_logprobs: Annotated[int, Ge(ge=[32m0[39m), Le(le=[32m20[39m)] = [32m0[39m,
) -&gt; [38;5;28;01mNone[39;00m
[31mDocstring:[39m     
OpenAI LLM.

Args:
    model: name of the OpenAI model to use.
    temperature: a float from 0 to 1 controlling randomness in generation; higher will lead to more creative, less deterministic responses.
    max_tokens: the maximum number of tokens to generate.
    additional_kwargs: Add additional parameters to OpenAI request body.
    max_retries: How many times to retry the API call if it fails.
    timeout: How long to wait, in seconds, for an API call before failing.
    reuse_client: Reuse the OpenAI client between requests. When doing anything with large volumes of async API calls, setting this to false can improve stability.
    api_key: Your OpenAI api key
    api_base: The base URL of the API to call
    api_version: the version of the API to call
    callback_manager: the callback manager is used for observability.
    default_headers: override the default headers for API requests.
    http_client: pass in your own httpx.Client instance.
    async_http_client: pass in your own httpx.AsyncClient instance.

Examples:
    `pip install llama-index-llms-openai`

    ```python
    import os
    import openai

    os.environ[&quot;OPENAI_API_KEY&quot;] = &quot;sk-...&quot;
    openai.api_key = os.environ[&quot;OPENAI_API_KEY&quot;]

    from llama_index.llms.openai import OpenAI

    llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;)

    stream = llm.stream_complete(&quot;Hi, write a short story&quot;)

    for r in stream:
        print(r.delta, end=&quot;&quot;)
    ```
[31mInit docstring:[39m
Create a new model by parsing and validating input data from keyword arguments.

Raises [`ValidationError`][pydantic_core.ValidationError] if the input data cannot be
validated to form a valid model.

`self` is explicitly positional-only to allow `self` as a field name.
[31mFile:[39m           c:\users\admin\anaconda3\envs\llamaindex\lib\site-packages\llama_index\llms\openai\base.py
[31mType:[39m           ModelMetaclass
[31mSubclasses:[39m     
```

```
base_url=os.getenv(&quot;BASE_URL&quot;)
api_key=os.getenv(&quot;OPENAI_API_KEY&quot;)
```

```
llm=OpenAI(api_key=api_key,
           api_base=base_url,
           model=&quot;gpt-4o-mini&quot;)
```

```
# Create an agent workflow with our calculator tool
agent = FunctionAgent(
    tools=[multiply],
    llm=llm,
    system_prompt=&quot;You are a helpful assistant that can multiply two numbers.&quot;,
)
```

```
response = await agent.run(&quot;What is 1234 * 4567?&quot;)
```

```
response
```

```
AgentOutput(response=ChatMessage(role=&lt;MessageRole.ASSISTANT: &#x27;assistant&#x27;&gt;, additional_kwargs={}, blocks=[TextBlock(block_type=&#x27;text&#x27;, text=&#x27;The result of \\( 1234 \\times 4567 \\) is 5,635,678.&#x27;)]), structured_response=None, current_agent_name=&#x27;Agent&#x27;, raw={&#x27;id&#x27;: &#x27;chatcmpl-ClVH0e884JLy3BK9fTfyeyIuuAjVL&#x27;, &#x27;choices&#x27;: [{&#x27;delta&#x27;: {&#x27;content&#x27;: None, &#x27;function_call&#x27;: None, &#x27;refusal&#x27;: None, &#x27;role&#x27;: None, &#x27;tool_calls&#x27;: None}, &#x27;finish_reason&#x27;: &#x27;stop&#x27;, &#x27;index&#x27;: 0, &#x27;logprobs&#x27;: None}], &#x27;created&#x27;: 1765437666, &#x27;model&#x27;: &#x27;gpt-4o-mini-2024-07-18&#x27;, &#x27;object&#x27;: &#x27;chat.completion.chunk&#x27;, &#x27;service_tier&#x27;: &#x27;default&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_aa07c96156&#x27;, &#x27;usage&#x27;: None, &#x27;obfuscation&#x27;: &#x27;oR&#x27;}, tool_calls=[ToolCallResult(tool_name=&#x27;multiply&#x27;, tool_kwargs={&#x27;a&#x27;: 1234, &#x27;b&#x27;: 4567}, tool_id=&#x27;call_bX9MMaUdLyeObAaFm2oMgX3N&#x27;, tool_output=ToolOutput(blocks=[TextBlock(block_type=&#x27;text&#x27;, text=&#x27;5635678&#x27;)], tool_name=&#x27;multiply&#x27;, raw_input={&#x27;args&#x27;: (), &#x27;kwargs&#x27;: {&#x27;a&#x27;: 1234, &#x27;b&#x27;: 4567}}, raw_output=5635678, is_error=False), return_direct=False)], retry_messages=[])
```

+   Llama-Index Agent历史对话记忆管理

```
from llama_index.core.workflow import Context

# create context
ctx = Context(agent)

# run agent with context
response = await agent.run(&quot;你好，我叫陈明&quot;, ctx=ctx)
```

```
response
```

```
AgentOutput(response=ChatMessage(role=&lt;MessageRole.ASSISTANT: &#x27;assistant&#x27;&gt;, additional_kwargs={}, blocks=[TextBlock(block_type=&#x27;text&#x27;, text=&#x27;你好，陈明！很高兴认识你。有什么我可以帮助你的吗？&#x27;)]), structured_response=None, current_agent_name=&#x27;Agent&#x27;, raw={&#x27;id&#x27;: &#x27;chatcmpl-ClVHo6lPr1vnoIj69KKWHTA7nDTtN&#x27;, &#x27;choices&#x27;: [{&#x27;delta&#x27;: {&#x27;content&#x27;: None, &#x27;function_call&#x27;: None, &#x27;refusal&#x27;: None, &#x27;role&#x27;: None, &#x27;tool_calls&#x27;: None}, &#x27;finish_reason&#x27;: &#x27;stop&#x27;, &#x27;index&#x27;: 0, &#x27;logprobs&#x27;: None}], &#x27;created&#x27;: 1765437716, &#x27;model&#x27;: &#x27;gpt-4o-mini-2024-07-18&#x27;, &#x27;object&#x27;: &#x27;chat.completion.chunk&#x27;, &#x27;service_tier&#x27;: &#x27;default&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_aa07c96156&#x27;, &#x27;usage&#x27;: None, &#x27;obfuscation&#x27;: &#x27;VO&#x27;}, tool_calls=[], retry_messages=[])
```

```
response = await agent.run(&quot;你还记得我叫什么名字么？&quot;, ctx=ctx)
```

```
response
```

```
AgentOutput(response=ChatMessage(role=&lt;MessageRole.ASSISTANT: &#x27;assistant&#x27;&gt;, additional_kwargs={}, blocks=[TextBlock(block_type=&#x27;text&#x27;, text=&#x27;当然记得，你叫陈明。有什么我可以帮助你的吗？&#x27;)]), structured_response=None, current_agent_name=&#x27;Agent&#x27;, raw={&#x27;id&#x27;: &#x27;chatcmpl-ClVIC02ryKoPcuC06k1yjbypxp5O1&#x27;, &#x27;choices&#x27;: [{&#x27;delta&#x27;: {&#x27;content&#x27;: None, &#x27;function_call&#x27;: None, &#x27;refusal&#x27;: None, &#x27;role&#x27;: None, &#x27;tool_calls&#x27;: None}, &#x27;finish_reason&#x27;: &#x27;stop&#x27;, &#x27;index&#x27;: 0, &#x27;logprobs&#x27;: None}], &#x27;created&#x27;: 1765437740, &#x27;model&#x27;: &#x27;gpt-4o-mini-2024-07-18&#x27;, &#x27;object&#x27;: &#x27;chat.completion.chunk&#x27;, &#x27;service_tier&#x27;: &#x27;default&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_aa07c96156&#x27;, &#x27;usage&#x27;: None, &#x27;obfuscation&#x27;: &#x27;Im&#x27;}, tool_calls=[], retry_messages=[])
```

### 3.Llama-Index RAG功能快速实现方法

+   数据集准备

```
import os
```

```
file_path = &quot;./data/创新科技股份有限公司员工手册.txt&quot;

with open(file_path, &quot;r&quot;, encoding=&quot;utf-8&quot;) as f:
    md_content = f.read()
```

```
md_content[:1000]
```

```
&#x27;# 创新科技股份有限公司员工手册\n\n## 前言\n\n尊敬的各位员工：\n\n欢迎您加入创新科技股份有限公司（以下简称“公司”或“创新科技”）。在您正式开启职业生涯新阶段的这一时刻，我们谨代表全体管理层及全体同仁，对您的加入表示最热烈的欢迎和最诚挚的祝贺。无论您是刚步入职场的应届毕业生，还是经验丰富的行业专家，我们都深信，您的才华、热情与努力将为公司注入新的活力，也将在这里收获丰富的成长与成就。\n\n创新科技股份有限公司自2010年成立以来，始终秉持“科技改变未来，创新引领发展”的理念，致力于推动人工智能、云计算、大数据、物联网等前沿科技在各行业的应用落地，助力全球客户实现数字化转型和可持续发展。作为一家快速成长的高科技企业，我们的每一次突破、每一次扩张、每一次变革，无不凝聚着每一位员工的智慧与汗水。今天，我们已经拥有超过800位来自五湖四海、充满激情的同仁，分布在北京总部、上海、深圳、广州等主要城市，并在美国硅谷、新加坡等国际科技枢纽设有办事处。公司的服务范围广泛涵盖金融、医疗、教育、制造业、智慧城市等多个关键领域，并与多家国内外知名企业建立了长期稳定的合作关系。\n\n我们深知，企业的可持续发展，离不开良好的企业文化和科学的人力资源管理制度。而这一切的基础，正是规范、清晰、公开、透明的员工政策体系。本员工手册的制定，旨在为所有员工提供一份详尽而实用的行为指南，帮助您全面了解公司的核心价值观、组织架构、行为规范、管理流程以及相关福利政策。它不仅是公司与员工之间的一份制度性文件，更是双方建立互信、合作与共同成长的重要载体。\n\n在本手册中，您将系统地了解公司在多个层面的基本政策，包括但不限于雇佣制度、职业道德准则、信息安全与数据保护、绩效评估与职业发展、薪酬福利计划、员工健康与安全保障、多样性与包容性倡议、环境可持续发展目标，以及员工在不同生命周期阶段（包括入职、在职、调动、离职等）的应尽义务与享有权利。手册中的每一项制度、每一个流程，都是经过充分调研与反复论证制定而成，力求在保障公司合规运营与高效管理的同时，也最大限度地维护员工的合法权益和个人尊严。\n\n公司始终奉行“以人为本”的管理理念。我们不仅视员工为最宝贵的资产，更把员工的成长、发展与幸福作为企业成功的核心指标之一。在创新科技，您将拥有公平公正的发展平台、丰富多样的培训资源、具有市场竞争力的薪酬福利体系，以及健康积极的工作氛&#x27;
```

![](img/144389632addea904d7692ac5cc2b338.png)

接下来介绍如何从零构建一个能够回答公司内部政策的 AI 助手。请注意，这几行代码虽然简洁，但它们在后台完成了 RAG 系统中最核心的ETL（抽取、转换、加载）、索引构建以及检索生成的全流程。我们将逐行拆解，重点剖析在默认参数下，Llama-Index 到底在后台做了什么，以及如何查看底层结果。

![](img/c747592f232a0ca675141d9e3af2d461.png)

```
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

# 1. 数据摄入 (Loading)
documents = SimpleDirectoryReader(&quot;data&quot;).load_data()

# 2. 索引构建 (Indexing)
index = VectorStoreIndex.from_documents(documents)

# 3. 引擎配置 (Query Engine)
query_engine = index.as_query_engine()

# 4. 执行查询 (Execution)
# 注意：aquery 是异步调用，在 Jupyter 或标准脚本中通常使用同步的 query
response = query_engine.query(&quot;请问，我们公司有病假政策么？请用中文进行回复。&quot;)
```

```
2025-12-11 17:52:14,257 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings &quot;HTTP/1.1 200 OK&quot;
2025-12-11 17:52:15,993 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings &quot;HTTP/1.1 200 OK&quot;
2025-12-11 17:52:17,533 - INFO - HTTP Request: POST https://api.openai.com/v1/chat/completions &quot;HTTP/1.1 200 OK&quot;
```

```
response.response
```

```
&#x27;公司有病假政策，员工可以享受最长为6个月的病假，具体的带薪比例根据工龄计算。&#x27;
```

第一步：数据摄入 (Data Ingestion)

```
documents = SimpleDirectoryReader(&quot;data&quot;).load_data()
```

+   **功能逻辑：** 这是 RAG 流程的起点。SimpleDirectoryReader 就像一个全能的搬运工**，它读取 dat****a 目**录下的《员工手册.txt》，将其内容加载到内存中，并封装为 Llama-Index 的标准 Document 对象。

+   **默认行为：**

+   **读取模式：** 默认将一个文件视为一个完整的 Document。如果手册有 1 万字，这里生成的 Document 就包含 1 万字的 text。

+   **元数据提取：** 自动提取文件名、路径、创建时间等基础信息作为 metadata。

```
documents
```

```
[Document(id_=&#x27;5f2c6ba3-5ee5-4e81-acf9-0108bde89918&#x27;, embedding=None, metadata={&#x27;file_path&#x27;: &#x27;D:\\work\\jupyter\\公开课项目\\1211 Llama-index入门\\data\\创新科技股份有限公司员工手册.txt&#x27;, &#x27;file_name&#x27;: &#x27;创新科技股份有限公司员工手册.txt&#x27;, &#x27;file_type&#x27;: &#x27;text/plain&#x27;, &#x27;file_size&#x27;: 116573, &#x27;creation_date&#x27;: &#x27;2025-12-11&#x27;, &#x27;last_modified_date&#x27;: &#x27;2025-12-11&#x27;}, excluded_embed_metadata_keys=[&#x27;file_name&#x27;, &#x27;file_type&#x27;, &#x27;file_size&#x27;, &#x27;creation_date&#x27;, &#x27;last_modified_date&#x27;, &#x27;last_accessed_date&#x27;], excluded_llm_metadata_keys=[&#x27;file_name&#x27;, &#x27;file_type&#x27;, &#x27;file_size&#x27;, &#x27;creation_date&#x27;, &#x27;last_modified_date&#x27;, &#x27;last_accessed_date&#x27;], relationships={}, metadata_template=&#x27;{key}: {value}&#x27;, metadata_separator=&#x27;\n&#x27;, text_resource=MediaResource(embeddings=None, data=None, text=&#x27;# 创新科技股份有限公司员工手册\r\n\r\n## 前言\r\n\r\n尊敬的各位员工：\r\n\r\n欢迎您加入创新科技股份有限公司（以下简称“公司”或“创新科技”）。在您正式开启职业生涯新阶段的这一时刻，我们谨代表全体管理层及全体同仁，对您的加入表示最热烈的欢迎和最诚挚的祝贺。无论您是刚步入职场的应届毕业生，还是经验丰富的行业专家，我们都深信，您的才华、热情与努力将为公司注入新的活力，也将在这里收获丰富的成长与成就。\r\n\r\n创新科技股份有限公司自2010年成立以来，始终秉持“科技改变未来，创新引领发展”的理念，致力于推动人工智能、云计算、大数据、物联网等前沿科技在各行业的应用落地，助力全球客户实现数字化转型和可持续发展。作为一家快速成长的高科技企业，我们的每一次突破、每一次扩张、每一次变革，无不凝聚着每一位员工的智慧与汗水。今天，我们已经拥有超过800位来自五湖四海、充满激情的同仁，分布在北京总部、上海、深圳、广州等主要城市，并在美国硅谷、新加坡等国际科技枢纽设有办事处。公司的服务范围广泛涵盖金融、医疗、教育、制造业、智慧城市等多个关键领域，并与多家国内外知名企业建立了长期稳定的合作关系。\r\n\r\n我们深知，企业的可持续发展，离不开良好的企业文化和科学的人力资源管理制度。而这一切的基础，正是规范、清晰、公开、透明的员工政策体系。本员工手册的制定，旨在为所有员工提供一份详尽而实用的行为指南，帮助您全面了解公司的核心价值观、组织架构、行为规范、管理流程以及相关福利政策。它不仅是公司与员工之间的一份制度性文件，更是双方建立互信、合作与共同成长的重要载体。\r\n\r\n在本手册中，您将系统地了解公司在多个层面的基本政策，包括但不限于雇佣制度、职业道德准则、信息安全与数据保护、绩效评估与职业发展、薪酬福利计划、员工健康与安全保障、多样性与包容性倡议、环境可持续发展目标，以及员工在不同生命周期阶段（包括入职、在职、调动、离职等）的应尽义务与享有权利。手册中的每一项制度、每一个流程，都是经过充分调研与反复论证制定而成，力求在保障公司合规运营与高效管理的同时，也最大限度地维护员工的合法权益和个人尊严。\r\n\r\n公司始终奉行“以人为本”的管理理念。我们不仅视员工为最宝贵的资产，更把员工的成长、发展与幸福作为企业成功的核心指标之一。在创新科技，您将拥有公平公正的发展平台、丰富多样的培训资源、具有市场竞争力的薪酬福利体系，以及健康积极的工作氛围。我们倡导开放沟通、跨部门协作和自我驱动，鼓励员工提出创新想法、挑战现状，不断优化流程、提升效率，实现个人价值的同时推动公司整体进步。\r\n\r\n值得特别指出的是，本员工手册虽然内容详尽，但其本质属性为指导性文件，并不构成劳动合同或任何形式的法律约束文件。然而，公司强烈建议您认真阅读并熟悉本手册内容，视其为您在公司工作期间的重要行为准则和行为指南。通过充分了解和遵守本手册规定的各项政策与流程，您不仅可以更快地融入团队、高效开展工作，还能在与同事及上级的互动中建立良好的职业关系，提升您的专业素养与领导力。\r\n\r\n为了确保手册始终与公司业务发展和法律法规保持一致，公司保留在必要时对本手册进行修改、补充或更新的权利。每一次修订将通过公司官方渠道进行公告，包括但不限于内部通知系统、电子邮件或HR门户网站，修订后的版本将自动替代之前版本并生效。员工有义务及时关注相关更新，并在疑惑或不解的情况下主动联系直属主管或人力资源部门进行咨询。公司人力资源部也将持续优化员工沟通机制，确保信息畅通、反馈及时、处理公正。\r\n\r\n在这个信息爆炸、技术快速更迭的时代，组织形态与工作方式正在发生深刻变革。面对数字化转型、远程办公、多元协作与跨文化团队的挑战与机遇，公司正在积极探索更高效的管理模式和更具人文关怀的工作文化。我们深知，仅凭制度无法塑造优秀的组织，只有通过信任、责任与合作，才能真正打造一个富有创造力与凝聚力的企业共同体。因此，除了书面制度外，我们更倡导一种“共创共担”的文化精神，希望员工在履行义务的同时，也能积极建言献策、共谋发展。\r\n\r\n例如，在公司行为准则部分，我们强调职业道德与合规经营，在保密义务与知识产权章节中，我们倡导严守法律责任并鼓励自主创新；在员工福利与假期管理方面，我们致力于提供兼顾生活质量与工作效率的弹性制度；而在多样性、公平与包容性政策下，我们努力营造一个尊重差异、平等参与的组织环境。这些政策不仅是一纸条文，更是公司文化的体现与员工体验的保障。我们希望每一位员工都能从中获得清晰的行为边界和发展的路线图，也希望每一位员工都能在这样的环境中实现自我突破与事业腾飞。\r\n\r\n在此，我们也向您郑重承诺：公司将一如既往地尊重并重视每一位员工的声音，持续优化员工体验，建立更具前瞻性、包容性和可持续性的组织管理机制。我们相信，员工与公司之间的关系，不仅是一种法律层面的雇佣关系，更是一种基于共同目标、共享价值和共同成长的事业伙伴关系。\r\n\r\n最后，请您在阅读完本手册后，签署《确认声明》并及时提交人力资源部归档。我们建议您将手册保留副本以供日后参考，同时也欢迎您在任何时候就手册内容提出建议与反馈。我们坚信，每一位员工的用心参与，都是推动公司持续进步的关键力量。\r\n\r\n再次感谢您选择加入创新科技股份有限公司。在接下来的旅程中，愿我们携手并肩，在科技浪潮中砥砺前行，共同开启属于我们的崭新时代。\r\n\r\n公司管理层\r\n 2025年7月11日\r\n\r\n## 第一章：公司概述\r\n\r\n在快速演变的科技时代，一个企业的发展不仅取决于技术和资本，更取决于其组织愿景、战略方向、文化体系与人才凝聚力。本章将全面介绍创新科技股份有限公司的使命、愿景、价值观、组织架构及雇佣原则，帮助员工更好地理解公司的本质与发展方向，增强归属感与责任感。\r\n\r\n### 1.1 公司使命与愿景\r\n\r\n#### 一、公司简介\r\n\r\n创新科技股份有限公司（以下简称“公司”或“创新科技”）成立于2010年，总部位于中国北京，是国家认定的高新技术企业，聚焦于人工智能（AI）、云计算、大数据与物联网（IoT）四大核心技术领域。自成立以来，公司始终秉持“以科技赋能社会，以创新驱动未来”的理念，致力于为全球客户提供先进、可靠、安全的数字化解决方案，推动各行业迈入智能化新时代。\r\n\r\n目前，公司员工人数已突破800人，分别在北京总部、上海、深圳、广州等重点城市设有研发和运营中心，并在美国硅谷、新加坡设立了全球业务拓展与技术合作办事处。公司业务横跨多个行业，包括但不限于金融科技、智慧医疗、智能制造、数字教育、交通出行与新型智慧城市。\r\n\r\n#### 二、使命宣言\r\n\r\n公司的使命是：“通过前沿技术创新，驱动全球数字化转型。”这不仅是一句口号，更是指导公司战略布局、产品开发、客户服务以及人才培养等各方面的核心理念。我们相信，科技的最终目的是服务人类、改善生活。因此，在技术研发的每一个细节中，我们都强调实用性、可持续性与社会责任，力求实现技术发展与人类福祉的平衡。\r\n\r\n#### 三、愿景展望\r\n\r\n公司的愿景是：“成为全球领先的科技服务提供商，构建人机共融的智能世界。”为实现这一愿景，我们不断投资前沿技术研究，探索人机协作、数字孪生、边缘计算、自然语言理解等关键领域。同时，我们也积极参与国际合作项目，与全球领先的大学、实验室及企业建立了开放式创新网络，共同探索未来科技的边界。\r\n\r\n#### 四、核心价值观\r\n\r\n我们始终坚持以下五项核心价值观，这些价值观深深嵌入公司管理体系、项目执行流程和团队协作文化中，构成创新科技的精神基石：\r\n\r\n- **创新（Innovation）**：我们鼓励员工提出大胆、前沿甚至颠覆性的想法，勇于实验、容错试错，建立内部孵化机制和知识共享平台，推动技术和管理持续迭代。\r\n- **诚信（Integrity）**：我们倡导真实、透明的交流方式，要求所有员工恪守职业操守、诚实守信、履职尽责。无论对客户、合作伙伴还是内部同仁，诚信始终是信任与合作的前提。\r\n- **协作（Collaboration）**：我们强调跨部门、跨区域、跨文化的高效协作。通过打造共创文化与团队协同机制，提升组织响应速度与整体执行力。\r\n- **卓越（Excellence）**：我们追求高质量标准，注重结果导向。无论是产品质量、服务体验还是运营流程，我们始终以客户满意度和行业标杆为目标，持续优化和迭代。\r\n- **责任（Responsibility）**：我们视社会责任为企业发展不可或缺的一部分，致力于环境保护、数据安全、教育公平、员工福祉等公益议题，真正做到“责任科技，科技有责”。\r\n\r\n#### 五、发展历程与里程碑\r\n\r\n公司自创立以来，历经十余年发展，已实现从初创团队到国际化科技企业的跨越。部分关键里程碑包括：\r\n\r\n- **2012年**：推出首个自主研发的云计算平台“CloudBase”，实现国内中型企业云部署自动化。\r\n- **2014年**：发布数据智能分析产品“Insight”，服务覆盖教育、零售、金融等三大行业。\r\n- **2016年**：AI实验室成立，聚焦自然语言处理、计算机视觉与自动驾驶算法。\r\n- **2018年**：获得国家高新技术企业认证，并荣获“最具成长性企业奖”。\r\n- **2020年**：与阿里云达成战略合作，打造混合云解决方案。\r\n- **2023年**：完成A轮融资，估值突破10亿美元。\r\n- **2024年**：新设可持续发展部门，开启“绿色科技”专项研发计划。\r\n\r\n这些成绩不仅是团队专业能力的体现，也彰显了公司在全球化竞争环境下的战略眼光与执行能力。\r\n\r\n#### 六、社会责任与公益参与\r\n\r\n创新科技高度重视企业社会责任，认为企业的发展必须与社会整体福祉同步。公司设有专门的社会责任小组，定期评估社会影响并制定行动计划。近年来，公司主要公益项目包括：\r\n\r\n- 每年投入固定比例利润支持“青少年科技教育基金”；\r\n- 组织技术员工为欠发达地区提供远程教育平台搭建与维护；\r\n- 参与“绿色城市”计划，向智慧城市平台引入碳排管理功能；\r\n- 设立员工志愿者项目，鼓励参与环保、教育与社区服务。\r\n\r\n我们相信，这些行动不仅帮助弱势群体，也激发了员工的使命感和归属感，提升了企业文化温度。\r\n\r\n### 1.2 组织结构\r\n\r\n#### 一、管理架构概览\r\n\r\n公司采用**矩阵式组织结构（Matrix Organization Structure）**，兼具职能部门与项目团队两种组织优势，旨在提高业务协同效率与资源配置灵活性。最高决策机构为董事会，由公司创始人、外部投资人及独立董事组成，负责制定公司发展战略、审议重大财务计划与监督运营管理。\r\n\r\n公司由首席执行官（CEO）领导，直接管理核心职能部门和各业务板块，向董事会汇报。CEO之下设有以下主要高级管理职位：\r\n\r\n- **首席运营官（COO）**：负责公司日常运营、流程优化、内部控制。\r\n- **首席技术官（CTO）**：领导技术战略、产品研发与技术生态构建。\r\n- **首席财务官（CFO）**：负责财务规划、成本控制、审计与资本运作。\r\n- **首席人力资源官（CHRO）**：统筹人才战略、组织文化、绩效与培训体系。\r\n- **首席市场官（CMO）**：统筹品牌传播、市场拓展与客户关系管理。\r\n\r\n各高管下设一级部门，部门负责人（Director或VP）直接向其汇报。\r\n\r\n#### 二、核心职能部门与职责\r\n\r\n以下为公司主要职能部门及其核心职责简述：\r\n\r\n- **人力资源部**：负责招聘与配置、绩效评估、员工关系、培训与发展、薪酬福利、组织文化建设。\r\n- **财务部**：负责预算管理、财务核算、税务合规、资金运作及投资分析。\r\n- **市场部**：聚焦品牌战略、客户调研、媒介投放、公关危机管理与市场活动执行。\r\n- **研发部**：由AI实验室、云平台团队、大数据平台组组成，专注前沿技术研究与产品开发。\r\n- **运营部**：负责内部流程管控、质量管理、供应链协作与项目推进管理。\r\n- **销售部**：负责客户关系、解决方案输出、项目招投标与合同管理。\r\n- **信息安全部**（新设）：制定与执行网络安全政策，防控数据泄露与合规风险。\r\n- **可持续发展部**：推动绿色能源、碳足迹监控、ESG报告发布等职责。\r\n\r\n公司在2024年新设立了“创新委员会”和“AI伦理审查小组”，分别统筹战略性项目孵化与技术伦理合规审查，体现公司在快速扩张期对内控与责任的重视。\r\n\r\n#### 三、跨部门沟通机制\r\n\r\n为了提高横向沟通效率，公司推行扁平化管理理念，鼓励基层员工与中高层管理者保持开放、双向的交流关系。公司内部门户系统汇聚组织架构图、通讯录、部门职能说明与工作流程文档，员工可随时查看并联络对口同事。\r\n\r\n每季度，公司召开一次“全员大会”，由CEO向全体员工通报业绩、战略动向与重点项目进展。部门之间则通过OKR联动会议、跨部门项目小组、内部Hackathon等形式，协同推进关键任务。\r\n\r\n### 1.3 雇佣原则\r\n\r\n#### 一、公平公正的雇佣制度\r\n\r\n公司严格遵循《中华人民共和国劳动法》及其他相关法律法规，承诺在招聘、晋升、奖惩、调岗等各类人事活动中贯彻**平等就业机会（Equal Employment Opportunity, EEO）**原则，不因员工的性别、种族、民族、年龄、宗教、婚姻状况、性取向、身体状况、政治信仰等受法律保护的特征而产生歧视行为。\r\n\r\n#### 二、招聘与录用流程\r\n\r\n我们推行结构化招聘流程，以确保候选人能力与岗位需求的高度匹配。具体步骤如下：\r\n\r\n1. **职位发布**：通过公司官网、主流招聘平台、校招系统和内部推荐机制发布岗位信息。\r\n2. **简历筛选**：采用AI辅助简历筛选工具，结合人工评估确保公正。\r\n3. **面试流程**：分为HR初试、业务面试、技术评估与文化适配面试，特殊岗位需额外参加案例演练。\r\n4. **背景调查**：包括学历验证、工作经历核查、推荐人反馈等。\r\n5. **录用与签约**：经审批后发出正式Offer，签署劳动合同、保密协议、知识产权归属协议等法律文件。\r\n\r\n#### 三、入职与融入机制\r\n\r\n新员工入职后将参与为期两周的**“迎新成长营”**培训计划，包括：\r\n\r\n- 企业文化、价值观导入；\r\n- 安全制度与合规培训；\r\n- 岗位职责、部门流程熟悉；\r\n- 团队协作活动与导师带教。\r\n\r\n此外，公司为新员工分配“职业导师”，帮助其在前三个月完成适应期评估，确保顺利融入。\r\n\r\n#### 四、多样性与包容性\r\n\r\n公司坚定推行多样性与包容性（Diversity &amp; Inclusion）战略，通过以下举措实现组织均衡：\r\n\r\n- 明确招聘目标，如女性技术人才比例提升计划；\r\n- 开设“职场平权”讲座与性别平衡专题培训；\r\n- 设置无障碍办公设施与远程协助系统；\r\n- 鼓励少数群体组建内部支持社群。\r\n\r\n截至2025年，女性员工比例已达40%，少数民族及外籍员工比例合计达15%。我们将继续努力，营造一个多元、包容、平等的职场生态。\r\n\r\n## 第二章：员工行为准则\r\n\r\n公司始终坚信，一个有秩序、高效且积极向上的组织环境，离不开全体员工对道德操守、组织规范和社会责任的共同遵守与坚守。为了明确员工在工作中应当遵循的行为标准，构建一个安全、公正、包容、富有责任感的职场环境，创新科技股份有限公司制定了本章“员工行为准则”，作为全体员工日常行为的指导方针和行为边界。\r\n\r\n本章将从职业道德、保密义务、知识产权、反歧视与反骚扰政策、冲突解决机制五个方面，系统阐述公司对员工在职业操守与行为规范方面的基本要求。\r\n\r\n### 2.1 职业道德\r\n\r\n#### 一、基本要求\r\n\r\n职业道德不仅关乎个人操守，更关乎整个组织的形象与信任基础。每位员工都应将诚实守信、尽职尽责、遵纪守法作为基本行为准则，时刻以公司利益、客户满意度与社会责任为重，不得利用职务便利谋取不当个人利益，不得参与损害公司声誉或资产的活动。\r\n\r\n以下为具体行为规范：\r\n\r\n- 所有员工在执行职责时，应确保信息真实、数据准确，严禁篡改、隐瞒或误导；\r\n- 员工不得在未申报情况下从事与公司业务存在竞争关系的兼职、投资或顾问活动；\r\n- 员工应避免与供应商、客户或合作方形成任何不正当的利益关联，包括接受现金、礼品、回扣、有价证券、宴请等；\r\n- 员工在代表公司参与外部活动时，应注意形象言行，不得传播与公司价值观相悖的言论；\r\n- 员工在使用社交媒体时，不得发布涉及公司机密、虚假陈述、恶意批评或煽动性质的内容；\r\n- 所有与客户、合作方和政府机构的接触均应依法合规，任何违规行为将承担严重后果。\r\n\r\n#### 二、道德培训与执行机制\r\n\r\n公司每年组织一次“职业道德与合规培训”，通过线上课程、案例研讨与模拟测试等方式，强化员工对风险场景的识别与应对能力。\r\n\r\n此外，公司设有**道德与合规监督办公室（Ethics Office）**，负责监督相关政策执行、受理投诉举报并开展专项审计。员工可通过匿名邮箱、热线电话或内部举报系统提交问题线索，公司承诺对举报内容保密并严禁任何形式的报复行为。\r\n\r\n### 2.2 保密义务\r\n\r\n#### 一、保密范围与分类\r\n\r\n在公司工作期间，员工将可能接触到各类商业秘密与敏感信息，包括但不限于：\r\n\r\n- 产品研发资料、源代码、算法文档；\r\n- 市场分析报告、客户名单、销售策略；\r\n- 财务报表、预算计划、投资信息；\r\n- 战略合作协议、投标文件、法律合同；\r\n- 内部邮件、管理制度、员工数据等。\r\n\r\n上述信息如被未经授权地泄露、传播、篡改或滥用，可能导致公司重大损失，甚至法律诉讼。为此，公司要求所有员工在入职前签署《保密协议》（NDA），承诺在在职及离职后2年内严格履行保密义务。\r\n\r\n#### 二、信息使用与传输规范\r\n\r\n- 员工在撰写、复制、下载、分享、保存公司内部文档时，必须遵循“最小权限原则”，仅限于业务所需的最小范围；\r\n- 禁止将敏感信息通过非加密邮件、公共网盘、U盘、聊天软件等非安全渠道外传；\r\n- 出差期间在公共场所使用笔记本、平板等设备处理工作事务时，应开启VPN与屏幕隐私保护；\r\n- 使用公司内部通讯工具（如企业微信、钉钉、OA系统）时，不得随意群发敏感信息，须按照岗位权限控制范围；\r\n- 离职员工必须归还所有资料、设备和访问权限，不得带离任何形式的公司数据。\r\n\r\n#### 三、违规行为处理\r\n\r\n公司设有**信息安全与数据合规审计机制**，定期抽查员工终端行为、网络流量与权限管理。如发现泄密行为，将依据《员工违纪处理细则》启动问责程序，必要时将追究法律责任。情节严重者，公司保留提起民事赔偿或刑事控告的权利。\r\n\r\n### 2.3 知识产权\r\n\r\n#### 一、归属原则\r\n\r\n公司依法拥有员工在雇佣期间所创作的以下成果的完整知识产权，包括但不限于：\r\n\r\n- 发明、专利、实用新型；\r\n- 软件代码、架构设计、技术论文；\r\n- 产品UI设计、数据库结构；\r\n- 实验结果、市场调研分析报告等。\r\n\r\n上述成果无论是否以“加班”或“远程办公”形式完成，均视为“职务成果”，归属于公司所有。员工须配合公司完成申报、登记、转让等手续，确保知识产权的合法归属。\r\n\r\n#### 二、创新激励机制\r\n\r\n公司高度重视员工的技术创造与方案贡献，建立了多层级的激励机制：\r\n\r\n- **专利奖励**：经公司申报并授权的发明专利最高奖励5万元；\r\n- **成果展示激励**：技术成果被纳入白皮书、官方展会或国际会议的，另行发放奖金；\r\n- **股权激励计划**：对于在核心项目中表现突出的骨干员工，公司将提供期权计划，作为长期激励措施；\r\n- **研发贡献积分系统**：通过内部系统累计积分，可兑换技术资源、培训机会或现金奖励。\r\n\r\n#### 三、避免侵权与合规使用\r\n\r\n员工在使用第三方资源时，必须严格遵循合法授权范围。常见情形包括：\r\n\r\n- 使用开源代码需注明许可证（如Apache、MIT）并遵守条款；\r\n- 禁止私自下载盗版软件、图像、文献或引用未经授权的数据；\r\n- 未经许可不得在外部公开演讲中披露公司专有算法或架构；\r\n- 所有对外发布的内容（如论文、博客、演示PPT）均需经主管审核批准。\r\n\r\n公司设有“知识产权管理委员会”，专责审查专利申报、侵权调查与法律合规。员工如发现可疑侵权行为，应立即向该委员会报告。\r\n\r\n### 2.4 反歧视与反骚扰政策\r\n\r\n#### 一、政策承诺\r\n\r\n创新科技致力于营造一个多元、平等、尊重的工作环境。我们坚定执行“零容忍”原则，禁止任何形式的歧视、骚扰或霸凌行为。无论行为是否出于恶意，一经核实，均将视情节严肃处理。\r\n\r\n#### 二、禁止行为示例\r\n\r\n- 基于性别、年龄、民族、宗教、残疾、婚姻状况、性取向、外貌等的歧视性言语或行为；\r\n- 强制性、暗示性、重复性地提出不受欢迎的言语或身体接触；\r\n- 在办公环境中传播性暗示内容、低俗玩笑或带有攻击性的表情包；\r\n- 公开羞辱、孤立某一员工或当众贬损其能力；\r\n- 上下级间存在不对等权力关系下的恋爱、打压或利益交换行为。\r\n\r\n#### 三、投诉与处理机制\r\n\r\n公司为所有员工提供多渠道、匿名、保密的投诉路径，包括：\r\n\r\n- 人力资源邮箱与举报表单；\r\n- 道德热线（支持中文、英文双语）；\r\n- 第三方中立仲裁机构专属邮箱（处理特殊申诉）。\r\n\r\n一旦接到投诉，公司将立即启动独立调查程序，通常包括：\r\n\r\n1. 事实调查（证据收集与当事人访谈）；\r\n2. 问题定性与政策匹配；\r\n3. 高管或HR部门讨论与定责；\r\n4. 通知处理结果与跟踪后续反馈。\r\n\r\n对加害方的处理可包括：正式警告、培训重修、岗位调整、停职处理或立即解雇。对举报人，公司承诺绝不进行任何形式的报复，若遭遇“打击报复”，欢迎再次上报，公司将加重处理。\r\n\r\n### 2.5 冲突解决\r\n\r\n#### 一、冲突的定义与常见类型\r\n\r\n组织内部存在不同意见与多样背景，是健康发展的表现。然而，当沟通不畅、情绪激化或目标冲突失控时，冲突可能演变为生产力的阻碍。公司鼓励以建设性方式处理冲突，防止其对工作环境造成负面影响。\r\n\r\n常见冲突情形包括：\r\n\r\n- 团队成员间因目标、分工、进度差异引发的合作摩擦；\r\n- 管理者与下属因沟通风格、绩效评估等产生误解；\r\n- 跨部门因资源分配、优先级判断等发生冲突；\r\n- 文化差异、沟通偏好或历史积怨导致的潜在对立。\r\n\r\n#### 二、解决机制与流程\r\n\r\n公司提供三个层级的冲突解决机制：\r\n\r\n1. **个人协商机制**：当冲突较小且涉及当事人有限时，员工应优先主动沟通，面对面交流观点，澄清误解，达成共识。\r\n2. **主管协调机制**：当个人协商失败或影响工作节奏时，员工可书面报告直接主管，请求第三方介入协调。主管应在5个工作日内响应，召集协调会议，记录处理结果。\r\n3. **人力资源调解机制**：对于涉及跨部门、结构性问题或可能涉及政策违规的冲突，HR部门将指派调解专员，采用结构化谈话、共识会议或书面协议等方式达成解决方案。\r\n\r\n必要时，公司也提供外部职业调解师或劳动仲裁机构建议。\r\n\r\n#### 三、预防与文化建设\r\n\r\n为了从源头减少冲突，公司倡导“开放文化”与“反馈意识”，通过以下举措提升组织心理安全感：\r\n\r\n- 新员工培训中加入“职场沟通与团队协作”模块；\r\n- 每月一次部门“情绪分享会”；\r\n- 引入“员工满意度调查”和“360反馈”制度；\r\n- 设立“员工援助计划（EAP）”，为有心理压力的员工提供免费心理咨询支持。\r\n\r\n## 第三章：工作时间与出勤\r\n\r\n规范的工作时间安排和出勤制度，是确保企业高效运营、员工健康发展和组织公平管理的关键基础。创新科技股份有限公司本着“以人为本、效率优先、灵活协同”的理念，在全面遵守国家劳动法规的前提下，制定了兼顾员工工作效率与生活质量的出勤管理体系。本章将系统阐述公司的标准工作时间制度、出勤要求、考勤管理方式、假期安排及相关的管理程序，以保障员工权利、维护组织纪律，并营造积极、高效、规范的工作氛围。\r\n\r\n### 3.1 工作时间\r\n\r\n#### 一、标准工时制度\r\n\r\n公司执行国家规定的**标准工时制**，每周工作五天，每天工作时间为**8小时**，即**上午9:00至下午6:00**，中午12:00至13:00为午休时间。每周共计工作时间为40小时，法定节假日除外。\r\n\r\n在标准工作时间内，员工应遵守作息安排，按时开始工作、参与会议、提交日报等，确保业务有序推进与跨部门协同效率。\r\n\r\n#### 二、弹性工作机制\r\n\r\n为适应多样化岗位特征、提升员工满意度，公司设立**弹性出勤政策**，适用于研发、产品、设计、市场等强调创造力与成果导向的岗位。弹性机制分为以下几类：\r\n\r\n1. **弹性上下班制度**：员工可在8:30至9:30之间灵活选择到岗时间，确保完成每日8小时工作任务。下班时间依照到岗时间顺延，但必须保证团队沟通顺畅，不影响集体安排。\r\n2. **远程办公机制**：符合条件的岗位员工可申请每周不超过**3天的远程办公**。远程期间需保持在线沟通状态，使用公司指定系统记录每日任务进展，并接受主管考核。\r\n3. **核心工作时间制度**：部分业务部门如运营中心、客服中心设有核心在岗时间（10:00-16:00），要求员工必须在此时段保持在线/在场，以保障服务不中断。\r\n\r\n弹性安排须经所属主管书面批准并在人力资源系统备案，部门需定期评估弹性机制对绩效、沟通与团队协作的影响。\r\n\r\n#### 三、成果导向工作制度（ROWE）\r\n\r\n公司对部分岗位（如中高级研发、战略顾问、技术专家）试点推行“以成果为导向的工作制度（Results-Only Work Environment，ROWE）”，即对工作时长不做强制约束，而以成果产出、任务完成质量与时间为绩效依据。\r\n\r\n实施ROWE制度需满足以下条件：\r\n\r\n- 任职时间不少于一年；\r\n- 过往绩效为“良好”以上；\r\n- 能独立承担项目任务，具备自我管理能力；\r\n- 已签署《ROWE工作协议》，明确目标设定与交付周期。\r\n\r\n人力资源部将与用人部门联合进行年度评估，并在试点成功基础上逐步推广。\r\n\r\n#### 四、加班管理与补偿\r\n\r\n在特殊业务需求或项目冲刺期间，确需员工延时工作的，需严格按照《加班管理制度》执行：\r\n\r\n- 所有加班必须提前由主管审批；\r\n- 工作日加班按基本工资的**1.5倍**补偿；\r\n- 周末加班按**2倍工资**补偿，或以调休形式折算；\r\n- 法定节假日加班按**3倍工资**支付，原则上不安排；\r\n- 年度个人加班时间不得超过**200小时**，超过部分须由部门总监审批并报备HR。\r\n\r\n公司鼓励在合理范围内通过任务管理、流程优化减少加班需求，防止员工长期过劳。\r\n\r\n### 3.2 出勤要求\r\n\r\n#### 一、考勤管理机制\r\n\r\n公司采用**电子打卡系统**（支持App打卡、人脸识别、门禁联动等方式），用于记录每日出勤数据。员工应在规定时间段内完成打卡，如遇设备故障或特殊情况，可在当日通过HR系统提交“考勤异常申诉”，由主管审核。\r\n\r\n出勤状态分类如下：\r\n\r\n| 状态      | 定义                       | 是否计为出勤       |\r\n| --------- | -------------------------- | ------------------ |\r\n| 正常出勤  | 员工按时上下班，无迟到早退 | 是                 |\r\n| 迟到      | 上班打卡时间超过9:10       | 是（记录警告）     |\r\n| 早退      | 下班前30分钟内打卡离岗     | 是（记录警告）     |\r\n| 旷工      | 未请假且整天未打卡         | 否                 |\r\n| 公出      | 经批准前往外部工作任务     | 是                 |\r\n| 事假/病假 | 经批准请假                 | 否（但不计入旷工） |\r\n\r\n#### 二、迟到与早退管理\r\n\r\n- 每月累计**3次迟到或早退**，将由主管进行面谈；\r\n- 每季度累计**5次以上**，将记入绩效记录；\r\n- 连续3天未打卡或无故缺勤，将被视为**严重违纪**，启动正式调查程序；\r\n- 特殊情形（如突发灾害、公共交通中断等）可提交说明申请免除记录。\r\n\r\n#### 三、请假制度\r\n\r\n公司鼓励员工合理安排个人事务，确保工作与生活平衡。员工因私事或健康问题无法出勤时，必须遵守请假流程：\r\n\r\n- **事假**：需提前24小时提交申请，说明原因与预计时长；\r\n- **病假**：当天提交，3天以上病假须附正规医院开具的病假单；\r\n- **突发情况**：如因家属紧急就医、交通事故等特殊情况无法提前申报，员工应尽早向主管报告，由其进行记录与补报；\r\n- 所有请假记录须在HR系统登记备案，并经直接上级批准。\r\n\r\n如无正当理由多次请假、延迟返岗或虚假请假，将依据公司纪律制度进行处理。\r\n\r\n#### 四、远程工作要求\r\n\r\n远程办公期间，员工应遵守以下规范：\r\n\r\n- 每日早上9:30前通过指定平台签到；\r\n- 使用远程日报系统提交工作计划与实际成果；\r\n- 保持电话、即时通讯、视频会议工具畅通；\r\n- 未经许可不得在远程期间从事与公司无关活动；\r\n- 人力资源部与信息技术部将定期审计远程工作日志。\r\n\r\n### 3.3 假期与休假\r\n\r\n公司严格按照国家法律法规执行各类假期制度，并提供多种带薪假期和额外福利假期，保障员工身心健康与家庭照顾权利。\r\n\r\n#### 一、法定节假日\r\n\r\n员工享有以下**法定带薪节假日**（以当年国家人力资源与社会保障部公告为准）：\r\n\r\n- 元旦：1天；\r\n- 春节：7天；\r\n- 清明节：1天；\r\n- 劳动节：3天；\r\n- 端午节：1天；\r\n- 中秋节：1天；\r\n- 国庆节：3天。\r\n\r\n节假日前后如遇调休，HR部门将发布出勤调整安排。\r\n\r\n#### 二、带薪年假\r\n\r\n员工在公司服务满一周年后，享有带薪年假，计算方式如下：\r\n\r\n| 工龄      | 年假天数 |\r\n| --------- | -------- |\r\n| 1 - 10年  | 10天     |\r\n| 11 - 20年 | 15天     |\r\n| 20年以上  | 20天     |\r\n\r\n年假须提前申请、按月累计，未使用部分可顺延至次年3月31日前使用，逾期未休视为自动放弃，但可申请转为现金补偿。\r\n\r\n公司鼓励员工**合理规划年假使用**，避免年底集中申请，保障业务连续性。\r\n\r\n#### 三、其他带薪假期\r\n\r\n| 假种类 | 时长      | 说明                           |\r\n| ------ | --------- | ------------------------------ |\r\n| 病假   | 最长6个月 | 按工龄计算带薪比例             |\r\n| 产假   | 158天     | 按国家规定执行，含产前假       |\r\n| 陪产假 | 15天      | 必须在配偶生产前后一个月内使用 |\r\n| 婚假   | 10天      | 提供结婚证复印件               |\r\n| 丧假   | 3天       | 限一等亲属，视情况延长         |\r\n\r\n#### 四、特别假期与福利假\r\n\r\n为提升员工满意度，公司增设以下弹性假期：\r\n\r\n- **家庭照护假**：每年最多5天，用于照顾生病子女、父母；\r\n- **心理健康日**：每半年可自选1天，事先申请，不需说明；\r\n- **奖励假**：连续两次绩效评级“优秀”的员工可额外获得3天带薪奖励假。\r\n\r\n### 3.4 假期申请流程与注意事项\r\n\r\n员工申请各类假期，应遵循以下标准流程：\r\n\r\n1. 登录企业HR系统，选择假种类、起止时间、请假原因；\r\n2. 提交后系统自动流转至直属主管审核；\r\n3. 审批通过后人力资源系统更新考勤状态；\r\n4. 返回岗位后如有实际出勤差异，员工须及时补录调整；\r\n5. 请假期间必须确保工作交接、设置代办事项、合理安排邮箱自动回复。\r\n\r\n若在假期期间提前结束或延长休假，员工须尽快提交变更申请。无故未返岗或擅自延长假期的行为，将被视为旷工处理。\r\n\r\n## 第四章：薪酬与福利\r\n\r\n薪酬与福利是员工价值认可和企业文化的重要体现。科学合理、透明公正的薪酬福利制度，不仅能够激励员工持续贡献、提升企业凝聚力与忠诚度，也体现了公司对员工权益的高度尊重。创新科技股份有限公司始终坚持“以能力为基础、以绩效为导向、以市场为参考、以公平为原则”的薪酬管理理念，构建了覆盖员工职业发展全过程的薪酬福利体系，致力于营造富有激励性、保障性与人文关怀的雇主品牌形象。\r\n\r\n本章将详尽说明公司在薪酬结构、年度调薪、福利计划、报销制度、奖金激励和员工支持项目等方面的具体政策，确保每一位员工都能清晰了解自己的权益和义务。\r\n\r\n### 4.1 薪酬结构\r\n\r\n#### 一、薪酬构成概览\r\n\r\n公司为员工提供具备竞争力的**“3+1”薪酬结构**，即：\r\n\r\n1. **固定薪资（基本工资）**：按月发放，是员工薪酬的基本保障，占总薪酬结构约70%；\r\n2. **绩效奖金**：依据个人KPI完成情况、部门目标及公司整体经营成果核定；\r\n3. **津贴补贴**：包括交通补贴、餐补、通讯补贴等日常工作支持性费用；\r\n4. **长期激励计划**：包括股权激励、期权计划、专项项目奖金等，用于激励核心骨干。\r\n\r\n该结构兼顾员工稳定收入、绩效导向和长期发展，体现“即时激励+中长期激励”相结合的薪酬战略。\r\n\r\n#### 二、薪资档级与岗位体系\r\n\r\n公司建立了清晰的**岗位等级体系**，划分为六大职级：助理级、专员级、高级专员/主管、经理级、高级经理/总监、副总裁及以上，每一级内设多个薪资档位，依据员工能力、绩效、年限及市场行情进行动态调整。\r\n\r\n各职级设有“横向通道”和“纵向通道”：\r\n\r\n- **横向通道**：允许员工在技术、管理、项目、产品等多个发展路径中进行转换；\r\n- **纵向通道**：强调从执行到规划、从操作到决策的晋升成长路径。\r\n\r\n员工每年可在绩效评估后参与晋级评审或调薪申请，由所属部门与人力资源联合审核，确保公开透明。\r\n\r\n#### 三、薪资发放与保密\r\n\r\n- 公司实行**月度发薪制**，每月15日前发放上月工资；\r\n- 所有薪资发放通过银行代发，员工可在HR系统中查询工资条明细；\r\n- 公司严格遵守《个人所得税法》，依法代扣代缴；\r\n- 所有员工薪资信息属于公司机密，未经允许不得泄露、交换、比较。\r\n\r\n### 4.2 年度调薪与特别调整\r\n\r\n#### 一、年度调薪政策\r\n\r\n公司每年进行一次统一薪资审查，通常安排在**12月-次年2月**之间实施。调薪评估依据以下因素：\r\n\r\n1. 个人绩效评级（权重最大）；\r\n2. 岗位晋升或职能变动；\r\n3. 部门或公司年度经营状况；\r\n4. 市场薪酬水平动态；\r\n5. 所属部门薪资结构均衡性。\r\n\r\n绩效评级为“优秀”或“超越预期”的员工，将优先获得调薪机会。调薪方案须经人力资源部、部门负责人及高管批准后执行。\r\n\r\n#### 二、特别调薪机制\r\n\r\n在以下情况下，员工可申请“特别调薪”或由主管主动发起：\r\n\r\n- 接受更高责任的岗位调动；\r\n- 关键人才挽留或外部高薪报价对比；\r\n- 重大项目成功交付；\r\n- 所在城市生活成本显著提升；\r\n- 市场行情波动较大岗位（如AI算法工程师、云平台架构师等）。\r\n\r\n特别调薪流程需提交说明材料、目标对比数据及推荐意见，由人力资源部评估，并呈报审批。\r\n\r\n### 4.3 奖金与激励计划\r\n\r\n#### 一、绩效奖金\r\n\r\n绩效奖金基于员工个人KPI、OKR以及公司整体业绩发放，标准如下：\r\n\r\n- 高绩效员工（“优秀”等级）奖金占基本工资**25%~30%**；\r\n- 达标员工（“良好”等级）奖金为基本工资**10%~15%**；\r\n- 表现一般或“待改进”者可能无绩效奖金。\r\n\r\n绩效奖金按**季度或年度**发放，部门经理需组织绩效回顾会议，向员工反馈评分依据和提升建议。\r\n\r\n#### 二、项目奖金\r\n\r\n为鼓励员工参与创新项目和战略重点任务，公司设立专项项目奖金池，典型适用情形如下：\r\n\r\n- 参与国家重点研发计划或企业级重要交付；\r\n- 完成跨部门高复杂度项目；\r\n- 创造显著业务效益或节省大量成本。\r\n\r\n项目负责人可提交申请，经项目办公室评估并由管理层核定发放金额。\r\n\r\n#### 三、股权激励与期权计划\r\n\r\n核心骨干员工可参与“合伙人激励计划”，包括：\r\n\r\n- **员工持股计划（ESOP）**；\r\n- **虚拟股权激励（VSOP）**；\r\n- **限制性股票或期权（RSU/ESO）**。\r\n\r\n公司根据年度经营成果与个人贡献动态调整激励池额度，具体方案在年度内另行通知，参与人员需签署相关协议。\r\n\r\n### 4.4 津贴补贴政策\r\n\r\n为减轻员工日常工作成本压力，公司设有一系列固定津贴，包含：\r\n\r\n- **交通补贴**：按城市划分，每月固定发放（北京600元、二线城市500元）；\r\n- **餐饮补贴**：每日20元标准，按出勤天数月结；\r\n- **通讯补贴**：全员统一每月100元，经理级及以上200元；\r\n- **高温补贴**：6-9月期间户外岗位按法规发放；\r\n- **节日补贴**：春节、端午、中秋发放节日礼包或等额购物卡。\r\n\r\n各项补贴在工资单中列明，员工可在HR系统中查看补贴记录。\r\n\r\n### 4.5 福利保障计划\r\n\r\n#### 一、社会保险与住房公积金\r\n\r\n公司依法为员工缴纳“五险一金”，即：\r\n\r\n- 养老保险\r\n- 医疗保险\r\n- 失业保险\r\n- 工伤保险\r\n- 生育保险\r\n- 住房公积金（按员工基数缴纳12%）\r\n\r\n公司严格按照国家及属地政策标准执行，并每年协助员工更新社保基数信息。\r\n\r\n#### 二、补充医疗保险\r\n\r\n公司为全员购买补充商业医疗险，涵盖：\r\n\r\n- 门急诊报销（含普通病、慢性病）；\r\n- 住院费用（含手术、药品）；\r\n- 特殊疾病津贴；\r\n- 年度报销上限10万元。\r\n\r\n员工入职30日内自动生效，可添加配偶和子女为附加受益人（自费）。\r\n\r\n#### 三、健康支持项目\r\n\r\n- **年度免费体检**（含癌症筛查）；\r\n- **健身房补贴**：每人每年最高2000元，提供合作健身品牌；\r\n- **心理健康服务**：EAP计划，每年6次免费心理咨询；\r\n- **营养指导与健康讲座**：每季度一次，公司专家主讲。\r\n\r\n#### 四、教育发展援助\r\n\r\n- 报销每年5000元以内的职业证书考试费或技能培训；\r\n- 与Coursera、MOOC、极客时间等平台合作，开放课程库；\r\n- 对外部专业会议、技术大会参会人员报销差旅与门票费用；\r\n- 提供内部轮岗与管理培训机会。\r\n\r\n#### 五、家庭支持政策\r\n\r\n- 儿童教育补助（小学至高中）：每年最高2000元/子女；\r\n- 弹性工作支持：育儿期员工可申请调整上班时间；\r\n- 孕期员工可享独立母婴室、专属休息区；\r\n- 配偶重大疾病员工可申请专项护理假。\r\n\r\n### 4.6 报销政策\r\n\r\n#### 一、适用范围\r\n\r\n公司报销制度适用于以下情况：\r\n\r\n- 商务差旅费用（含交通、住宿、交通补贴）；\r\n- 项目材料采购；\r\n- 外出用餐、客户招待；\r\n- 培训课程及考试费用；\r\n- 合规办公耗材采购。\r\n\r\n#### 二、报销流程\r\n\r\n1. 员工在ERP系统提交报销申请；\r\n2. 上传有效票据与发票（纸质、电子均可）；\r\n3. 主管审核签字；\r\n4. 财务部核对并付款（5-7个工作日到账）；\r\n5. 超预算或单笔超过3000元须由部门负责人额外审批。\r\n\r\n#### 三、限制性条款\r\n\r\n- 报销项目须在**60日内申请**，逾期不予处理；\r\n- 不得报销个人消费、家属陪同费用、发票不符项目；\r\n- 虚假报销将视为严重违纪行为处理；\r\n- 公司保留对报销记录审计的权利。\r\n\r\n## 第五章：绩效评估\r\n\r\n绩效评估是企业人力资源管理的核心环节之一。它不仅是对员工过去一段时间工作成果的回顾总结，更是人才发展、组织优化与文化落地的重要工具。创新科技股份有限公司秉持“科学、公正、协同、发展”的绩效理念，构建了涵盖目标设定、过程跟踪、阶段回顾、评估反馈和改进提升在内的闭环绩效管理体系，确保员工个人发展与公司战略目标之间实现高度对齐。\r\n\r\n本章将详细介绍公司绩效评估的组织架构、实施流程、评估标准、应用机制、发展工具和申诉渠道等内容，帮助员工充分理解绩效评估的意义、方法与影响，建立积极、开放、正向的绩效文化。\r\n\r\n### 5.1 绩效评估体系概述\r\n\r\n#### 一、绩效理念\r\n\r\n公司的绩效管理核心理念包括：\r\n\r\n- **目标导向**：绩效应聚焦“做正确的事”，强调目标设定与结果落地。\r\n- **过程可见**：通过OKR、周报、季度回顾等机制，保障过程透明。\r\n- **发展驱动**：评估不仅是结果判断，更是成长导向与能力发展的工具。\r\n- **全员参与**：绩效管理不仅是HR的职责，更是每一位员工和管理者的责任。\r\n- **公平公正**：评估过程遵循标准化、可追溯、基于事实的数据评判原则。\r\n\r\n公司反对“唯结果论”“唯关系论”“形式主义”等偏差做法，鼓励以真实数据、实际贡献为评估依据，确保员工在公平环境下获得反馈与激励。\r\n\r\n#### 二、适用对象\r\n\r\n公司所有正式员工，包括总部、各地分公司与海外办事处员工，均纳入统一绩效评估管理范畴。试用期员工参与季度试点评估，不参与年度评级。实习生与项目外包人员根据实际项目考核方式处理。\r\n\r\n中高级管理人员（经理及以上）需承担下属绩效辅导责任，其绩效评级将部分受其团队整体绩效表现影响。\r\n\r\n### 5.2 绩效评估流程\r\n\r\n公司采用“半年度+年度”双周期绩效评估机制，评估流程共分为五个阶段：\r\n\r\n#### 一、目标设定（Performance Planning）\r\n\r\n- 时间：每年1月与7月\r\n- 工具：公司统一OKR系统或绩效管理平台（如SuccessFactors、Moka）\r\n- 要求：\r\n  - 所有员工需与直属上级在评估周期开始前两周内共同确定绩效目标；\r\n  - 目标需具备SMART原则（具体、可衡量、可达成、相关性强、时限明确）；\r\n  - 建议设定3~5个主要目标，并明确权重与量化指标。\r\n\r\n其中包括：\r\n\r\n| 目标类型       | 示例                                     |\r\n| -------------- | ---------------------------------------- |\r\n| 业务成果类目标 | 完成A项目上线、实现销售额增长20%         |\r\n| 管理类目标     | 建立团队协作流程、优化组织架构           |\r\n| 个人成长目标   | 完成某门技术课程并取得证书、提高演讲能力 |\r\n\r\n目标需在HR平台提交，并经两级主管确认后生效。\r\n\r\n#### 二、过程跟踪（Performance Monitoring）\r\n\r\n- 时间：全过程\r\n- 工具：双周/月度OKR打卡、项目日报、复盘会议\r\n- 要求：\r\n  - 员工需定期记录工作进展，更新目标完成状态；\r\n  - 直属上级需每月至少一次与员工进行反馈对话；\r\n  - 对于目标偏离或进度滞后，应及时调整策略或修订目标。\r\n\r\n公司提倡“即时反馈”文化，鼓励主管与员工之间建立持续反馈机制，避免“只看期末评分”的单一模式。\r\n\r\n#### 三、中期回顾（Mid-term Review）\r\n\r\n- 时间：每年6月、12月初\r\n- 工具：系统自动提醒\r\n- 要求：\r\n  - 上下级共同审视目标进展；\r\n  - 对进展偏离部分说明原因，并讨论改进计划；\r\n  - 必要时调整目标内容或权重；\r\n  - 回顾会议需形成书面纪要并上传系统。\r\n\r\n中期回顾不计入正式评分，但对年终评估具有参考价值。\r\n\r\n#### 四、年度评估（Performance Appraisal）\r\n\r\n- 时间：每年12月~次年1月\r\n- 步骤：\r\n  1. **自评（Self-Evaluation）**：员工根据目标完成情况、自我反思与收获进行自评撰写；\r\n  2. **主管评估（Manager Review）**：直属上级基于事实数据与观察打分并提供书面反馈；\r\n  3. **360度反馈（可选）**：对关键岗位实施多方反馈机制，涵盖同事、下属与跨部门协作对象；\r\n  4. **校准会议（Calibration Meeting）**：部门内统一标准、调整偏差；\r\n  5. **绩效沟通（Feedback Meeting）**：主管与员工一对一进行正式沟通，明确结果与建议；\r\n  6. **结果录入系统并归档**。\r\n\r\n### 5.3 绩效评估标准\r\n\r\n#### 一、评估维度\r\n\r\n公司采用多维度绩效评分机制，共包括以下四项维度，赋予权重如下：\r\n\r\n| 维度                 | 权重 | 描述                               |\r\n| -------------------- | ---- | ---------------------------------- |\r\n| 目标完成情况         | 50%  | 是否完成原定业务指标和项目交付成果 |\r\n| 专业能力发展         | 20%  | 是否掌握新技能、知识学习、能力提升 |\r\n| 团队合作与沟通       | 20%  | 协作意识、跨部门配合、信息透明     |\r\n| 企业文化与价值观体现 | 10%  | 是否践行公司核心价值观与行为准则   |\r\n\r\n#### 二、评分等级定义\r\n\r\n评估结果分为以下五档，分别对应不同管理动作：\r\n\r\n| 等级                            | 描述                               | 后续举措                         |\r\n| ------------------------------- | ---------------------------------- | -------------------------------- |\r\n| A - 卓越（Top Performer）       | 超出预期，创造重大价值，具有引领性 | 晋升、加薪、激励奖金、项目主导权 |\r\n| B - 良好（Strong Performer）    | 达成目标，有持续提升，表现稳定     | 推荐培养、技能培训、奖金         |\r\n| C - 合格（Meets Expectations）  | 完成核心任务，表现中规中矩         | 保持岗位，参与轮岗               |\r\n| D - 待改进（Needs Improvement） | 存在多项未完成任务或行为问题       | 纳入PIP计划，限期改进            |\r\n| E - 不合格（Unsatisfactory）    | 严重绩效失职或不当行为             | 解除劳动合同或调整岗位           |\r\n\r\n公司实施强制分布比例建议（非强制），防止评估“虚高”失真现象。例如，A等级人数不超过10%，B等级控制在30%~40%。\r\n\r\n### 5.4 绩效评估结果应用\r\n\r\n绩效评估不仅决定短期激励，更影响员工长期发展。评估结果将用于以下人力资源决策：\r\n\r\n#### 一、薪酬调整\r\n\r\n- 年度调薪方案以绩效评级为主要参考；\r\n- 绩效等级为“优秀”者将获得高于平均水平的涨薪；\r\n- 连续两年“待改进”或“不合格”者将暂停调薪资格。\r\n\r\n#### 二、奖金发放\r\n\r\n- 年终奖依据绩效等级分档发放，具体比例见第四章薪酬部分；\r\n- 项目奖金分配亦需参考相关人员绩效等级，强化“多劳多得”机制。\r\n\r\n#### 三、晋升与职级调整\r\n\r\n- 所有岗位晋升、职级评估必须满足至少一年“良好”以上绩效记录；\r\n- 连续两次“卓越”评级者可跳级申报晋升或参与管理培训项目。\r\n\r\n#### 四、内部流动与轮岗\r\n\r\n- 绩效评估为横向流动或跨部门推荐提供重要依据；\r\n- 表现突出者优先考虑参与高潜力项目或岗位试用。\r\n\r\n#### 五、绩效改进计划（PIP）\r\n\r\n针对连续绩效评级低于“合格”的员工，公司启动“绩效改进计划（Performance Improvement Plan）”：\r\n\r\n- 设定3个月改善周期；\r\n- 明确具体改进目标；\r\n- 提供配套培训、资源与主管指导；\r\n- 周度进展回顾；\r\n- 计划期末进行复评，未达预期者启动岗位调整或终止合同。\r\n\r\n### 5.5 培训与职业发展支持\r\n\r\n绩效不仅评估过去，更着眼未来成长。公司提供如下职业发展支持：\r\n\r\n#### 一、培训资源\r\n\r\n- 内训课：由公司内训师或管理者开设业务课程、通用技能班；\r\n- 外部课：合作高校、平台（如Coursera、极客时间）提供；\r\n- 技术认证：鼓励考取PMP、CFA、Cisco、AWS等国际认证；\r\n- 领导力计划：经理级及以上员工每年参加领导力工作坊。\r\n\r\n#### 二、职业发展路径\r\n\r\n公司建立清晰的双通道发展机制：\r\n\r\n| 通道     | 说明                           |\r\n| -------- | ------------------------------ |\r\n| 专业通道 | 高级工程师→技术专家→首席科学家 |\r\n| 管理通道 | 高级专员→主管→经理→总监        |\r\n\r\n每位员工均可通过绩效回顾提出“职业发展意愿”，HRBP将定期组织职业访谈与发展辅导。\r\n\r\n#### 三、导师机制\r\n\r\n新员工及关键岗位人员可申请“内部导师”配对制度，获得经验传承、职业导航与心理支持，促进长期职业发展。\r\n\r\n### 5.6 反馈与申诉机制\r\n\r\n为确保绩效评估公正透明，公司建立完善的反馈与申诉机制：\r\n\r\n#### 一、反馈会谈制度\r\n\r\n- 每次绩效评估结束后，主管需与员工面对面开展绩效反馈会；\r\n- 员工可就目标设定、评分依据、成长建议提出看法；\r\n- 所有反馈过程应形成纪要文档并存档。\r\n\r\n#### 二、绩效申诉机制\r\n\r\n若员工对评估结果有异议，可在评估结果公示后7个工作日内提出书面申诉，流程如下：\r\n\r\n1. 员工提交《绩效申诉表》至HR；\r\n2. HR组织二级主管或独立评审小组复审；\r\n3. 如需，安排三方复议会谈；\r\n4. 最终处理结果由公司绩效委员会裁定并反馈。\r\n\r\n公司承诺申诉过程公开透明、全程保密，不因申诉行为对员工进行打压或负面评价。\r\n\r\n## 第六章：纪律与惩戒\r\n\r\n企业的持续健康发展，离不开科学的制度保障与严格的行为规范。纪律不仅是组织运转的基础，也是员工之间实现公平、互信与效率的前提。为保障公司的运营秩序、维护公司利益和良好的工作氛围，创新科技股份有限公司建立了明确的纪律规范与惩戒制度，旨在引导员工自觉遵守行为准则，明确“可为”与“不可为”的边界，及时发现与处理不当行为，最大限度地预防风险并推动组织持续改善。\r\n\r\n本章对员工在公司工作期间必须遵守的基本纪律、违纪行为的分类、惩戒措施的种类与适用标准、调查处理流程、申诉与复议机制等方面进行全面说明，并强调在处理任何员工违纪行为时，公司都将遵循合法、合规、公正、透明的原则。\r\n\r\n### 6.1 违纪行为定义与分类\r\n\r\n#### 一、违纪行为定义\r\n\r\n员工违纪是指员工在公司工作期间，违反公司规章制度、行为准则、岗位职责、国家法律法规、职业道德，或因疏忽、过失或主观故意行为而造成公司形象、业务、财产或其他员工权益受到不良影响的行为。无论违纪行为发生在公司场所或外部，只要与工作相关或影响公司利益，均可作为处理依据。\r\n\r\n#### 二、违纪行为分类\r\n\r\n公司依据违纪行为的性质、情节和后果，将其划分为**轻微违纪、中等违纪和严重违纪**三类，具体分类如下：\r\n\r\n**（1）轻微违纪行为：**\r\n\r\n该类行为通常为无主观恶意、未造成严重后果的行为，但若多次发生或整改不力，将升级为中等违纪处理。\r\n\r\n| 示例行为                                             |\r\n| ---------------------------------------------------- |\r\n| 上班迟到、早退，未履行请假手续擅自离岗               |\r\n| 着装不符合公司规范（如未穿工牌、未遵守商务着装要求） |\r\n| 上班时间使用手机、社交软件处理私人事务               |\r\n| 未经许可私自修改办公软件配置                         |\r\n| 忽略公司会议通知，无正当理由缺席部门例会或全员会议   |\r\n| 未按流程提交日报、周报、项目总结等文档               |\r\n\r\n**（2）中等违纪行为：**\r\n\r\n此类行为具有一定主观性，已对工作秩序、团队协作或客户形象造成负面影响，或已触犯公司核心流程制度。\r\n\r\n| 示例行为                                               |\r\n| ------------------------------------------------------ |\r\n| 多次无正当理由迟到、早退或未打卡，累计达警戒线         |\r\n| 虚构工作成果、隐瞒项目延误、篡改数据报告               |\r\n| 未经授权私自外出、代表公司参加外部活动                 |\r\n| 滥用公司资源（如打印机、车辆、办公耗材等）用于个人目的 |\r\n| 上班期间睡觉、打游戏、购物等与工作无关行为             |\r\n| 在公共平台发表不当言论损害公司声誉                     |\r\n| 不服从上级合理工作安排、公开顶撞管理人员               |\r\n| 对同事有语言冒犯、冷嘲热讽、消极抵抗协作任务           |\r\n\r\n**（3）严重违纪行为：**\r\n\r\n严重违纪通常包括有意违背职业道德、违反法律、触犯公司“红线”行为，情节严重者将被立即解聘并可能追究法律责任。\r\n\r\n| 示例行为                                 |\r\n| ---------------------------------------- |\r\n| 伪造、篡改报销单据，骗取公司资金         |\r\n| 泄露公司机密信息、客户数据、项目方案     |\r\n| 收受贿赂或向第三方提供公司资源谋取私利   |\r\n| 严重歧视、性骚扰、暴力行为或职场霸凌     |\r\n| 盗窃公司或他人财物                       |\r\n| 在工作场所使用、持有、传播毒品或违禁品   |\r\n| 酒后进入办公室或在上班期间饮酒影响工作   |\r\n| 故意破坏公司系统、服务器或数据平台       |\r\n| 恶意传播谣言，挑拨离间，严重影响组织氛围 |\r\n| 利用职位谋取私人投资利益或竞业行为       |\r\n\r\n### 6.2 惩戒措施与适用规则\r\n\r\n公司在处理员工违纪行为时，始终坚持“以事实为依据、以制度为准绳”的原则，综合考虑违纪的性质、次数、后果、主观态度及员工过往记录，依法依规实施惩戒。\r\n\r\n#### 一、惩戒等级\r\n\r\n公司设立以下六级惩戒措施，按违纪情节依次加重：\r\n\r\n1. **口头警告**：由主管对员工进行非正式谈话，指出问题，提醒整改；\r\n2. **书面警告**：形成正式书面文书存档，列入员工行为记录；\r\n3. **绩效影响**：将违纪纳入年度考评，影响绩效评级与奖金；\r\n4. **停发奖金或津贴**：根据违纪行为直接扣除当期绩效奖金或专项津贴；\r\n5. **岗位调整或降级**：取消管理权限，调离原岗位或降低职级；\r\n6. **解除劳动合同**：对严重违纪者，依法解除劳动合同，不支付经济补偿。\r\n\r\n严重违纪者，公司保留追究经济赔偿、向公安机关报案及依法追责的权利。\r\n\r\n#### 二、惩戒适用细则\r\n\r\n- 同一违纪行为连续出现超过三次者，自动升级处理级别；\r\n- 多项轻微违纪合并视为中等违纪；\r\n- 若违纪行为造成公司客户损失、声誉风险、法律纠纷，将按严重违纪处理；\r\n- 涉及领导干部、管理人员违纪将适用更高惩戒标准，并加重处理；\r\n- 员工若在调查过程中存在掩盖、推诿、串供行为，将视为抗拒调查并加重惩罚。\r\n\r\n### 6.3 惩戒流程与执行程序\r\n\r\n为了确保纪律执行的公正与程序正当，公司设有统一的**纪律处理流程**，由人力资源部联合用人部门执行。\r\n\r\n#### 一、流程步骤\r\n\r\n1. **事件报告**\r\n   任何员工、主管或第三方发现疑似违纪行为，均可通过邮件、HR系统或匿名渠道向人力资源部或监察办公室报告。\r\n2. **初步核实**\r\n   HR根据线索初步核实事件真实性，并决定是否立案调查。轻微事件由部门自处，中等及以上事件须升级处理。\r\n3. **正式调查**\r\n   HR牵头成立调查小组（可含第三方），对当事人、证人进行访谈，收集资料，确保取证合法、流程可追溯。\r\n4. **评估与处理建议**\r\n   调查结束后，HR撰写调查报告，列明事实、证据、涉事人陈述，并提出处理建议。\r\n5. **听证与申辩（适用于中等及以上）**\r\n   给予被调查人申辩权利，可陈述理由、提交补充材料，必要时举行听证会。\r\n6. **决定与执行**\r\n   由人力资源主管或总经理根据调查结果做出正式处理决定，并下发书面通知。视情节通报范围包括部门、全员或高管层。\r\n7. **文件归档**\r\n   所有处理结果、流程记录将存入员工人事档案，保留5年备查。\r\n\r\n#### 二、员工知情权与参与权\r\n\r\n- 所有涉及个人惩戒的程序，员工均有知情权、答辩权与申诉权；\r\n- 除非法律要求或严重扰乱秩序，公司承诺在调查未结束前不披露当事人姓名；\r\n- 公司严禁对举报人、证人、申诉人进行打击报复，一经发现将严惩。\r\n\r\n### 6.4 申诉与复议机制\r\n\r\n为保障员工合法权益，公司建立了完善的纪律处分申诉和复议机制。\r\n\r\n#### 一、申诉路径\r\n\r\n员工若对公司作出的惩戒决定不服，可在**收到决定之日起7个工作日内**，通过以下渠道提出申诉：\r\n\r\n- 向人力资源部书面提交《申诉申请表》；\r\n- 邮件提交至公司纪律与合规邮箱（[confidential@innovationtech.com](mailto:confidential@innovationtech.com)）；\r\n- 如遇高管或HR人员牵涉案件，可向“独立合规审查委员会”提出异议。\r\n\r\n#### 二、复议程序\r\n\r\n收到申诉后，公司将在5个工作日内启动复议流程：\r\n\r\n1. **成立复议小组**：成员包括HR代表、法务代表、独立第三方/工会代表（如有）；\r\n2. **审阅材料**：重新审查调查报告与员工申诉理由；\r\n3. **听取意见**：必要时组织复议会谈；\r\n4. **做出结论**：复议结论形成《申诉复审意见书》，由总经理或指定高管签署生效；\r\n5. **反馈与归档**：将复议结论反馈至员工，并归入人事系统。\r\n\r\n复议决定为最终处理结果，除非另有法律程序启动，否则不再接受二次申诉。\r\n\r\n### 6.5 纪律管理文化倡导\r\n\r\n公司认为，纪律制度不仅是惩罚机制，更应当发挥教育、预防、提醒、激励的功能。为此，公司从文化层面强化纪律意识，主要举措包括：\r\n\r\n#### 一、制度公开透明\r\n\r\n- 所有员工手册、制度文件均可在HR门户网站查阅；\r\n- 公司定期开展《员工行为规范》、《公司红线行为警示录》学习培训；\r\n- 每季度发布“合规案例通报”与“正面典型表彰”。\r\n\r\n#### 二、正向激励结合\r\n\r\n- 对于自我纠错、积极配合整改者，可视情节减轻处理；\r\n- 鼓励员工主动举报、发现风险，设立“合规建议激励奖”，每条最高1000元；\r\n- 表现优秀、履职尽责者优先推荐参加管理培训和晋升评定。\r\n\r\n#### 三、管理者职责明确\r\n\r\n- 各级管理者需承担下属行为教育与风险预警责任；\r\n- 未能及时制止下属违纪行为或存在“纵容”“包庇”情节者，将一并问责；\r\n- 管理者每年需接受一次“组织纪律责任”专项培训。\r\n\r\n## 第七章：健康与安全\r\n\r\n员工的健康与安全是企业可持续发展的基础。创新科技股份有限公司始终将员工的生命安全和身心健康放在首位，全面推行“以人为本、预防为先、全员参与、持续改进”的健康与安全管理方针，积极构建安全、健康、和谐的工作环境，为员工创造稳定、高效、可持续的工作生活空间。\r\n\r\n本章将详细阐述公司在职业安全、环境卫生、健康促进、紧急响应和安全文化建设方面的制度安排和执行规范，确保每位员工了解自己的权利、义务与可用资源。\r\n\r\n### 7.1 健康与安全管理体系\r\n\r\n#### 一、健康与安全的总体目标\r\n\r\n- 零重大安全事故；\r\n- 零职业病；\r\n- 员工年度健康体检参与率100%；\r\n- 应急响应机制响应时间≤3分钟；\r\n- 安全培训覆盖率100%；\r\n- 员工满意度≥90%的安全工作环境。\r\n\r\n#### 二、管理架构与责任分工\r\n\r\n公司设立专门的“健康与安全委员会”，由首席运营官（COO）牵头，成员包括人力资源部、行政部、设施管理部、IT部、安保服务商、各部门代表及员工志愿者代表。职责包括：\r\n\r\n- 制定与更新公司安全管理政策；\r\n- 组织年度安全检查、消防演练；\r\n- 审查重大健康和安全事件报告；\r\n- 制定员工健康支持项目计划；\r\n- 管理疫情响应、传染病控制和突发公共卫生事件流程；\r\n- 推广安全文化、员工心理健康、压力管理等项目。\r\n\r\n各部门设有专兼职“安全联络员”，负责本部门日常安全检查、安全培训传达、安全隐患收集与上报。\r\n\r\n### 7.2 工作场所安全政策\r\n\r\n#### 一、办公场所安全设施\r\n\r\n公司各办公场所均按照国家和地方安全法规配置相关安全设施：\r\n\r\n- 所有办公区配备足量灭火器、烟雾探测器、喷淋装置，并每季度检测；\r\n- 每个楼层设有明确疏散图、出口指示牌、应急照明；\r\n- 配备自动体外除颤仪（AED）、急救包、简易担架等急救工具；\r\n- 在核心服务器机房配置UPS、不间断供电系统、温湿度控制与安防系统；\r\n- 建有独立吸烟区，避免吸烟引发火灾或影响同事健康。\r\n\r\n所有新入职员工必须完成“职场安全通识培训”，涵盖逃生路径、火灾应对、触电急救、防滑防摔等基本常识。\r\n\r\n#### 二、安全检查与隐患排查\r\n\r\n公司执行“日常自检、月度巡检、季度专项”的三级隐患排查机制：\r\n\r\n- 各部门每周五组织安全自查，包括插座、电线、办公家具、通风口等；\r\n- 行政与设施管理部每月组织一次巡查，涵盖电气系统、厨房、走道等重点区域；\r\n- 公司每季度邀请第三方安全机构进行评估，并提交《健康与安全改进报告》。\r\n\r\n发现的隐患需在48小时内整改，重大隐患须立即停用相关设备并上报健康与安全委员会。\r\n\r\n#### 三、禁止性行为规范\r\n\r\n以下行为被视为严重违反安全规定，公司将予以严肃处理：\r\n\r\n- 擅自移动灭火器、应急出口设备；\r\n- 在服务器机房、配电间等区域吸烟；\r\n- 非IT人员未授权进入关键设备区域；\r\n- 擅自关闭烟感、消防喷淋设备；\r\n- 私拉乱接插线板、私用电热器等存在电气火灾风险的行为；\r\n- 拒不参与公司组织的应急疏散演练。\r\n\r\n### 7.3 员工健康支持计划\r\n\r\n#### 一、年度健康体检\r\n\r\n公司为所有正式员工提供年度免费体检，涵盖以下内容：\r\n\r\n- 基础项目：血常规、肝肾功能、心电图、X光；\r\n- 高风险筛查：癌症早筛、甲状腺、肿瘤标志物（根据年龄与岗位定制）；\r\n- 女性专项检查：乳腺彩超、妇科筛查（适龄员工）；\r\n- 中老年专项：骨密度、眼底照查、认知功能测试。\r\n\r\n员工体检报告由公司指定三甲医院出具，并可通过公司健康管理平台进行预约、查看、咨询。\r\n\r\n#### 二、心理健康支持（EAP）\r\n\r\n现代职场压力巨大，公司引入专业心理健康服务体系（EAP），免费向员工提供以下服务：\r\n\r\n- 每年6次心理咨询（电话、在线或面对面）；\r\n- 职场情绪调节、亲子关系、睡眠管理等主题心理讲座；\r\n- 心理压力自测工具及心理课程库；\r\n- 专属心理服务热线，支持24小时匿名咨询；\r\n- 特殊事件干预（如亲属离世、重大变动）由HR协调专属心理顾问介入支持。\r\n\r\n所有心理服务均由第三方合规服务机构执行，员工信息严格保密，公司不会获知具体内容。\r\n\r\n#### 三、运动与健康生活激励\r\n\r\n- 公司与多家健身机构合作，员工可使用年度健身补贴（最高2000元）；\r\n- 每年组织两次“健康挑战月”，如“每日步数达标挑战”“零外卖挑战”等；\r\n- 设置“健康积分商城”，积分可通过步数、饮食打卡等方式积累，兑换健康餐券、按摩器、体脂秤等福利；\r\n- 办公场所设置休闲健身区，配备瑜伽垫、拉伸带、可调节桌椅；\r\n- 鼓励员工参与骑行、登山、羽毛球、篮球等运动社团，公司按人均每月100元标准提供场地或装备补贴。\r\n\r\n### 7.4 疾病防控与疫情响应\r\n\r\n#### 一、传染病管理与响应机制\r\n\r\n公司参考国家卫健委、疾控中心、世界卫生组织发布的标准，建立“传染病防控与应急响应机制”，适用于：\r\n\r\n- 流感季节、疫情爆发阶段的快速反应；\r\n- 个人确诊传染病后的隔离、通报和恢复流程；\r\n- 密接人员的排查与健康管理；\r\n- 疫苗接种倡导与协助服务。\r\n\r\n公司设有“疫情响应小组”，由行政、HR、法务、医疗顾问构成，职责包括：\r\n\r\n- 发布口罩佩戴、场所消杀、出入登记等管控政策；\r\n- 设立居家办公政策、线上会议优先机制；\r\n- 协助员工核酸检测与就医转诊；\r\n- 处理传染病相关误解、歧视和信息发布。\r\n\r\n#### 二、居家隔离与线上支持\r\n\r\n如员工确诊传染病（如新冠、甲流、肺结核等），公司将提供以下支持：\r\n\r\n- 正常病假薪酬与补贴；\r\n- 所需物资配送服务（药品、口罩等）；\r\n- 安排线上每日健康回访；\r\n- 如员工居住地封控或有密接史，可申请临时远程办公；\r\n- 部门主管需合理安排交接或延后不紧急任务，避免员工病中仍“被动加班”。\r\n\r\n### 7.5 紧急事件响应机制\r\n\r\n#### 一、应急预案覆盖范围\r\n\r\n公司已制定全面的《应急处置预案》，适用于以下场景：\r\n\r\n- 火灾；\r\n- 地震或自然灾害；\r\n- 暴力入侵或恐袭事件；\r\n- 办公楼电梯、楼梯踩踏风险；\r\n- 员工突发疾病（心梗、哮喘、过敏）；\r\n- 数据中心断电、系统崩溃；\r\n- 水管爆裂、空调系统故障、气体泄漏等设施问题。\r\n\r\n#### 二、演练与培训\r\n\r\n- 每年进行不少于**2次应急疏散演练**，确保员工掌握逃生路线；\r\n- 所有行政、安全人员需接受**消防证、急救员资格证培训**；\r\n- 定期举办**CPR（心肺复苏）与AED使用培训**；\r\n- 应急联系人、急救志愿者名单在员工门户长期公示，方便应对突发事件。\r\n\r\n#### 三、紧急报告机制\r\n\r\n员工在遭遇紧急事件时应立即拨打公司应急热线（分部门张贴），或通过企业IM标记“#紧急”发布信息，系统将自动通知安保、行政、HR、IT等多部门联动响应。\r\n\r\n公司要求：\r\n\r\n- 每位员工知晓最近逃生出口；\r\n- 熟悉灭火器使用流程；\r\n- 知道所在楼层的疏散集结点；\r\n- 发生事件时优先自保，不擅自行动或返回办公室取物。\r\n\r\n### 7.6 健康与安全文化建设\r\n\r\n#### 一、安全文化倡导\r\n\r\n公司不仅强调制度与流程，更希望通过文化建设提升全员健康与安全意识：\r\n\r\n- 每季度发布《健康与安全快报》，介绍典型案例与新知识；\r\n- 在内部社区设立“安全建议箱”，每条采纳建议奖励100元；\r\n- 发起“每人都是安全员”活动，每位员工可获得“微检查员”身份参与隐患排查；\r\n- 定期表彰“健康达人”“安全模范员工”，提升认同感。\r\n\r\n#### 二、全员参与与责任共担\r\n\r\n- 员工在发现潜在危险（如插座冒烟、楼道积水、电梯异响）后有义务立即上报；\r\n- 员工在办公桌区域堆放纸箱、遮挡逃生通道将被提醒整改；\r\n- 员工对不安全行为（如吸烟、电瓶车上楼等）有权劝阻或报告；\r\n- 各级主管对其管理区域负有安全第一责任，考核中纳入安全管理维度。\r\n\r\n## 第八章：信息安全与数据保护\r\n\r\n在数字化、网络化和全球化迅猛发展的背景下，信息已成为企业最核心的资产之一。任何信息系统的失控、数据泄露或网络攻击，均可能导致财产损失、品牌声誉受损、客户信任丧失，甚至触犯国家法律法规。创新科技股份有限公司高度重视信息安全与数据保护，将其视为企业运营的“生命防线”。\r\n\r\n为此，公司建立了全面的信息安全管理体系（ISMS），依据《中华人民共和国网络安全法》《数据安全法》《个人信息保护法》以及国际标准ISO/IEC 27001等，明确员工在信息使用、系统操作、数据传输、保密义务等方面的行为规范，确保公司、客户及合作伙伴的信息资产得到充分保护。\r\n\r\n### 8.1 信息安全政策\r\n\r\n#### 一、信息安全的基本原则\r\n\r\n公司信息安全管理制度遵循以下五项核心原则：\r\n\r\n1. **保密性（Confidentiality）**：信息仅授权人员可访问；\r\n2. **完整性（Integrity）**：信息在存储、传输和处理过程中不被篡改；\r\n3. **可用性（Availability）**：授权人员在需要时可正常访问和使用信息资源；\r\n4. **可追溯性（Traceability）**：所有数据操作和系统访问均可审计、追溯、记录；\r\n5. **最小权限（Least Privilege）**：任何员工仅能访问其履行岗位职责所必须的信息。\r\n\r\n#### 二、适用范围\r\n\r\n信息安全政策适用于：\r\n\r\n- 所有正式员工、实习生、外包人员、临时项目合作人员；\r\n- 所有公司内部系统、邮件平台、远程接入终端；\r\n- 涉及公司业务、客户、财务、技术、HR等全量数据；\r\n- 所有办公终端、服务器、云平台、个人移动设备等使用环境。\r\n\r\n所有人员在入职或签约前，必须签署《信息安全承诺书》与《保密协议》，明确自身义务与法律责任。\r\n\r\n#### 三、安全责任划分\r\n\r\n| 角色       | 安全责任                                   |\r\n| ---------- | ------------------------------------------ |\r\n| 高管层     | 设定信息安全战略，批准重大安全政策         |\r\n| CIO/CTO    | 主导安全体系规划与投入                     |\r\n| 信息安全部 | 建立制度、监控系统、处理事件、审计报告     |\r\n| 各部门主管 | 对本部门数据使用行为、安全培训负直接责任   |\r\n| 所有员工   | 按章操作，发现风险及时报告，不做高风险操作 |\r\n\r\n公司每年开展一次安全自评与外部审计，并将审计结果纳入部门年度运营考核指标。\r\n\r\n### 8.2 数据资产管理与防护\r\n\r\n#### 一、数据分类分级制度\r\n\r\n为有效识别、管理并保护不同敏感程度的数据，公司将数据分为四类并设定相应防护等级：\r\n\r\n| 数据等级     | 类型                           | 访问控制             | 示例                   |\r\n| ------------ | ------------------------------ | -------------------- | ---------------------- |\r\n| 一级（机密） | 核心代码、财务报表、客户合同   | 高度限制，需多重授权 | 商业秘密、加密密钥     |\r\n| 二级（内部） | 项目资料、会议纪要、日常报表   | 员工可查看但不得外发 | 内部流程文件、员工手册 |\r\n| 三级（公开） | 官网内容、公开宣传资料         | 可公开浏览           | 招聘海报、公司介绍PPT  |\r\n| 四级（个人） | 员工身份证、联系方式、健康数据 | 仅本人及HR可访问     | 入职资料、薪资信息     |\r\n\r\n所有涉及一级与二级数据的操作，必须通过VPN+工控设备完成，不得在非授权平台进行处理。\r\n\r\n#### 二、数据存储与备份\r\n\r\n公司所有重要业务数据均通过以下方式存储：\r\n\r\n- 在公司内网服务器与云平台（如AWS、阿里云）进行双重存储；\r\n- 定期（每日/每周）自动备份；\r\n- 每月对备份有效性进行验证；\r\n- 对客户敏感数据启用加密存储机制，含传输加密（TLS/SSL）和静态加密（AES-256）。\r\n\r\n个人终端数据如未同步至服务器，发生丢失风险由员工自行承担。请勿在本地磁盘保存核心资料，建议使用公司统一云盘或项目管理平台（如Notion、Confluence、GitLab等）。\r\n\r\n#### 三、数据访问权限管理\r\n\r\n- 所有权限由系统管理员通过RBAC（基于角色访问控制）机制统一管理；\r\n- 权限变更（调岗、离职、项目变更）需在24小时内完成更新；\r\n- 高风险操作（如数据库导出）需经主管+信息安全部联合审批；\r\n- 数据提取日志每周进行一次巡查，疑似违规访问将被记录与审查。\r\n\r\n### 8.3 员工行为规范与操作指南\r\n\r\n#### 一、日常信息安全行为要求\r\n\r\n1. **登录管理**：\r\n   - 登录系统时必须使用公司提供的账户；\r\n   - 严禁共享账号密码，违反视为严重违纪；\r\n   - 登录密码每90天自动过期，须定期更新。\r\n2. **邮件使用规范**：\r\n   - 公司邮箱为正式业务往来唯一通道；\r\n   - 禁止使用个人邮箱收发工作资料；\r\n   - 不点击来源不明的邮件链接或附件；\r\n   - 所有群发邮件须使用密件抄送（BCC）。\r\n3. **移动存储与打印**：\r\n   - 不得使用个人U盘或移动硬盘连接办公设备；\r\n   - 打印涉及客户信息或财务内容需主管授权；\r\n   - 机密文件打印后须及时回收并粉碎销毁。\r\n4. **软件安装与设备使用**：\r\n   - 禁止私自安装第三方软件、浏览器插件、破解工具；\r\n   - 设备故障或中毒不得私自修复，必须联系IT支持；\r\n   - 公司配发笔记本不得擅自外借或安装“双系统”。\r\n5. **离岗安全**：\r\n   - 15分钟内未操作电脑将自动锁屏；\r\n   - 离开工位须手动锁屏；\r\n   - 不得将带有工作资料的笔记本长时间暴露在公共场所。\r\n\r\n#### 二、远程办公与BYOD政策\r\n\r\n为保障远程办公的安全性，公司实施如下规定：\r\n\r\n- 必须使用公司VPN接入企业系统；\r\n- 所有远程设备须安装公司授权的终端安全软件；\r\n- 禁止在网吧、公共Wi-Fi下访问内部系统；\r\n- BYOD（自带设备）需提前登记备案并通过加密配置。\r\n\r\n### 8.4 个人信息保护政策（PIP）\r\n\r\n#### 一、定义与适用范围\r\n\r\n根据《个人信息保护法》，公司对以下信息承担保护义务：\r\n\r\n- 员工信息：身份证、手机号、邮箱、薪资、健康、住址、家庭联系人；\r\n- 客户信息：公司名称、联系人、交易记录、地址、付款方式等；\r\n- 应聘者信息：简历、学历、工作经历、面试过程记录等；\r\n- 合作方信息：合同、开票信息、对公账号、联系人等。\r\n\r\n#### 二、保护措施\r\n\r\n- 建立“个人信息处理记录”，每季度更新处理目的与范围；\r\n- 收集前必须明确告知使用目的、范围、保存期限并取得授权；\r\n- 严禁将员工/客户个人信息出售、共享、泄露给第三方；\r\n- 所有信息保留期限不超过业务所需最长6年，期满销毁；\r\n- 所有参与客户服务、数据标注、调研、IT支持等岗位，需参加个人信息保护专项培训并考核合格。\r\n\r\n#### 三、用户权利\r\n\r\n所有信息主体享有以下权利：\r\n\r\n- 查询与访问权；\r\n- 撤回授权权；\r\n- 数据更正与删除权；\r\n- 投诉与申诉权；\r\n- 拒绝非必要处理的权利。\r\n\r\n员工或客户可通过“信息安全服务中心”邮箱提出申请，公司将在15个工作日内反馈处理。\r\n\r\n### 8.5 安全事件处理与违规处罚\r\n\r\n#### 一、信息安全事件定义\r\n\r\n信息安全事件指因系统漏洞、员工操作不当、外部攻击等原因，导致信息丢失、篡改、泄露或非法使用的行为，包括但不限于：\r\n\r\n- 系统入侵、病毒传播；\r\n- 非法访问、数据外泄；\r\n- 误操作导致客户信息误发；\r\n- 加密证书泄漏；\r\n- 客户或员工举报数据异常使用；\r\n- 云服务权限配置不当。\r\n\r\n#### 二、事件应急响应流程\r\n\r\n1. 第一时间报告信息安全部（ISD）；\r\n2. 启动《信息安全应急预案》，隔离影响系统；\r\n3. 调查事件源头、受影响范围；\r\n4. 向法律合规、数据负责人汇报；\r\n5. 如符合法定上报标准，向监管部门及客户报告；\r\n6. 组织修复、更新策略、发布事件说明。\r\n\r\n公司信息安全部承诺“4小时内响应、24小时内初步结论、72小时内恢复计划”。\r\n\r\n#### 三、违规处罚\r\n\r\n依据事件等级与后果，公司将对责任人采取以下措施：\r\n\r\n| 情节             | 惩戒方式                       |\r\n| ---------------- | ------------------------------ |\r\n| 轻微违规（首次） | 口头警告、重申规则、补训       |\r\n| 中度违规         | 书面警告、绩效影响、权限调整   |\r\n| 严重违规         | 降职、停薪、解除合同、移送司法 |\r\n\r\n如行为涉嫌违法，公司将协同司法机关进行追责。\r\n\r\n## 第九章：多样性、公平与包容（D&amp;I）\r\n\r\n在全球化和人才多元化日益加速的今天，多样性、公平与包容（Diversity, Equity &amp; Inclusion, 简称 D&amp;I）不仅是现代企业履行社会责任的重要表现，更是提升组织韧性、激发创新能力与增强员工归属感的核心驱动因素。创新科技股份有限公司始终坚持“尊重差异，公平发展，包容共创”的人本理念，将 D&amp;I 原则嵌入企业治理、人才战略、文化建设和日常运营的各个环节，努力构建一个真正多元、平等、开放的工作环境。\r\n\r\n本章将系统阐述公司在多样性人才管理、反偏见培训、公平晋升机制、包容文化建设、员工支持网络、监督评估体系等方面的政策与举措，确保每一位员工无论其性别、年龄、种族、文化背景、宗教信仰、性取向、身体状况或其他身份特征，都能在公司内平等获得机会、资源与尊重。\r\n\r\n### 9.1 D&amp;I战略政策声明\r\n\r\n#### 一、价值宣言\r\n\r\n我们相信：\r\n\r\n- 多样化的员工群体能够带来更丰富的视角与创意，推动技术创新与产品多元发展；\r\n- 公平的制度安排能够保障每位员工的努力与价值被公正对待；\r\n- 包容的文化氛围能够帮助员工更安心、更积极地投入工作，从而实现自我成长与组织目标的统一。\r\n\r\nD&amp;I 是公司可持续发展战略的一部分，不是短期项目，而是长期承诺。\r\n\r\n#### 二、核心目标\r\n\r\n- 建立可衡量的 D&amp;I 发展目标，并与业务指标挂钩；\r\n- 提升女性、残障人士、少数群体在中高层管理团队中的占比；\r\n- 所有职位、薪酬、晋升、培训、绩效等环节遵循公平原则；\r\n- 所有员工均接受反歧视、偏见意识和包容力培训；\r\n- 建立高信任度的申诉与反馈机制，鼓励安全发声。\r\n\r\n### 9.2 多样性管理\r\n\r\n#### 一、招聘与人才吸引\r\n\r\n公司在招聘过程中确保消除偏见与歧视，实施以下策略：\r\n\r\n- 使用中性语言与包容表达编写职位描述，避免含有性别倾向或年龄暗示词汇；\r\n- 拓宽招聘渠道，与女性技术社区、残障人就业平台、多元高校合作，确保人才池多样化；\r\n- 所有招聘人员必须接受“无意识偏见（Unconscious Bias）”培训；\r\n- 初试阶段采用结构化面试与匿名简历评审，避免“背景标签”干扰判断；\r\n- 推行“候选人多元池”策略，每个岗位面试至少邀请1名女性或少数群体候选人（如具备条件）。\r\n\r\n#### 二、员工群体代表性监测\r\n\r\n公司每年对员工群体结构进行数据分析，包括：\r\n\r\n| 项目                   | 统计维度                            |\r\n| ---------------------- | ----------------------------------- |\r\n| 性别                   | 女性员工比例，女性技术/管理岗位占比 |\r\n| 年龄                   | 各年龄段员工比例（&lt;30, 30-45, &gt;45） |\r\n| 残障                   | 有无注册残障员工及岗位适配情况      |\r\n| 国籍/地域              | 海外员工、跨文化员工数量及地域分布  |\r\n| 教育背景               | 非985/211院校占比，非传统专业占比   |\r\n| LGBTQ+群体（匿名统计） | 自愿参与匿名调研                    |\r\n\r\n数据分析用于优化招聘策略、薪酬公平性和组织构成，不用于员工个人评估。\r\n\r\n### 9.3 公平机制建设\r\n\r\n#### 一、薪酬公平性保障\r\n\r\n- 实施年度“同岗同酬”审计，对比性别、年龄、工作年限维度的数据，确保无系统性差异；\r\n- 招聘薪资谈判中禁止依据员工历史薪资报价，统一参考岗位标准与能力匹配；\r\n- 所有薪酬等级与升迁机会公开透明，HR定期开展“公平晋升沟通日”解读政策；\r\n- 设立“薪酬审计小组”，如员工怀疑薪资不公，可提交保密申诉，HR介入复核。\r\n\r\n#### 二、晋升与评估公平\r\n\r\n- 所有管理晋升机会对内公开发布，支持员工主动申报参与评审；\r\n- 晋升评审委员会成员构成需具备多样性（性别、职级、职能）；\r\n- 绩效评分过程要求主管提供事实型证据（数据、案例、成果），避免主观标签；\r\n- 员工每年有一次自我申诉与目标修订机会，避免因前期目标设定不公而影响评级。\r\n\r\n#### 三、无障碍办公支持\r\n\r\n- 所有办公楼配备无障碍通道、电梯语音播报、无障碍卫生间；\r\n- 公司会议室均配有无线麦克风、投屏字幕，方便听力受限员工；\r\n- IT部为视障员工提供屏幕阅读工具（如NVDA），支持键盘导航等操作习惯；\r\n- 可远程办公岗位优先支持残障员工申请灵活工作制。\r\n\r\n### 9.4 包容性文化建设\r\n\r\n#### 一、反歧视与反骚扰政策（与第六章联动）\r\n\r\n公司在第六章中已明确设立“零容忍”政策，禁止一切基于下列特征的歧视或骚扰行为：\r\n\r\n- 性别与性别认同\r\n- 年龄与婚育状况\r\n- 残障与健康状况\r\n- 国籍与语言能力\r\n- 宗教信仰与文化背景\r\n- 性取向与LGBTQ+身份\r\n\r\n所有员工每年必须完成D&amp;I在线课程，通过结业测试后方可进入下一轮绩效考核。\r\n\r\n#### 二、包容语言倡导\r\n\r\n公司推行“包容语言”使用规范，员工在邮件、会议、公开发言时应避免以下用语：\r\n\r\n- “女程序员”、“老员工”、“正常人”、“小姑娘”等具有隐含标签性别/年龄/能力歧视；\r\n- 使用中性词如“同事”“技术人员”“团队成员”替代；\r\n- 所有面向外部的内容（如演讲、招聘）须通过HR内容审校；\r\n- 推出《职场包容表达手册》并设立示范用语表。\r\n\r\n### 9.5 员工资源小组（ERG）与支持网络\r\n\r\n公司支持成立多样性相关的**员工资源小组（Employee Resource Groups, ERG）**，目前已批准设立：\r\n\r\n| 小组名称         | 目标与活动                               |\r\n| ---------------- | ---------------------------------------- |\r\n| Women@Innovation | 女性发展分享、女性技术讲座、导师配对     |\r\n| DiverseAbilities | 残障包容倡导、办公适配建议、辅助工具试点 |\r\n| Pride+           | LGBTQ+社区支持、职场友好建设             |\r\n| NewGeneration    | Z世代员工文化交流、跨代对话工作坊        |\r\n| CultureMix       | 多语言、多民族员工联谊、海外节日交流会   |\r\n\r\nERG在公司支持下可申请年度预算（每组不超过5万元），用于组织线下活动、出版手册、开展公益倡导项目等。员工可匿名参与，无需公开身份。\r\n\r\n### 9.6 D&amp;I培训与赋能\r\n\r\n#### 一、培训计划\r\n\r\n- 新员工入职前须完成D&amp;I意识入门课程；\r\n- 所有中层以上管理人员需每两年完成一次“包容性领导力”线下培训；\r\n- 高风险岗位（如面试官、人力资源、法务等）需接受“反偏见”“识别歧视信号”“员工沟通技巧”等专题培训；\r\n- 绩效面谈、辞退沟通等敏感场景设有专项辅导材料。\r\n\r\n#### 二、包容性管理技能建设\r\n\r\n公司为各级主管配备《包容性管理者工具包》，包括：\r\n\r\n- 包容性会议组织技巧（如发言轮次引导、避免“一言堂”）；\r\n- 冲突中的文化理解框架（如跨文化差异解释）；\r\n- 建设性反馈的语言结构模板；\r\n- 非语言信号识别指南（如孤立迹象、情绪波动预警）；\r\n- “困难对话”模拟练习清单。\r\n\r\n### 9.7 监督机制与数据披露\r\n\r\n#### 一、指标追踪与透明披露\r\n\r\n公司设立D&amp;I年度目标指标，HR与战略发展部每年联合发布《D&amp;I年度报告》，内容包括：\r\n\r\n- 关键数据：性别比例、晋升机会分布、薪酬差距、管理层多样性；\r\n- 重点项目：D&amp;I培训覆盖率、ERG运营情况、典型案例；\r\n- 外部合作：与大学、非营利组织合作内容、社会影响；\r\n- 员工反馈：来自年度满意度调查的D&amp;I维度数据。\r\n\r\n该报告内部公开，并视情况摘要对外披露，接受股东、监管机构、行业协会审阅。\r\n\r\n#### 二、员工反馈与申诉渠道\r\n\r\n- 所有员工可通过D&amp;I建议通道提交提升建议；\r\n- 对存在隐性歧视、不公平评估、边缘化等现象的员工，可通过HRBP、独立合规邮箱 [confidential@innovationtech.com](mailto:confidential@innovationtech.com) 提出反馈；\r\n- 公司设立“D&amp;I监督专员”岗位，独立于HR日常架构，直接向CHRO汇报；\r\n- 所有举报将严格保密，杜绝打击报复，申诉流程将于10个工作日内启动调查。\r\n\r\n## 第十章：环境与可持续发展\r\n\r\n在全球面临日益严峻的气候变化、资源枯竭、生物多样性下降等挑战的背景下，企业作为社会经济的重要组成部分，不仅肩负着经济责任，更承载着环境保护和可持续发展的道德与法律义务。创新科技股份有限公司自成立以来，始终将“绿色发展”作为企业核心战略之一，积极倡导低碳办公、绿色供应链和环保科技解决方案，致力于通过技术创新推动社会向可持续未来迈进。\r\n\r\n本章旨在全面介绍公司在环境管理、节能减排、可持续产品与服务开发、员工环保行为规范以及绿色文化建设等方面的具体政策与实践。公司希望通过制度化的管理机制和全民参与的倡导方式，将“可持续发展”从理念转化为行动，将“环保责任”融入每一位员工的日常行为之中。\r\n\r\n### 10.1 企业环境责任政策声明\r\n\r\n创新科技郑重承诺：\r\n\r\n1. 严格遵守国家和地方关于环保、节能、废弃物管理等方面的法律法规，主动接受监管与社会监督；\r\n2. 设立环境管理目标，将碳排放、水资源使用、电力消耗、办公耗材等指标纳入年度KPI监控；\r\n3. 推行绿色采购、绿色设计与绿色制造原则，促进产品生命周期各阶段的环境影响最小化；\r\n4. 鼓励员工践行环保行为，倡导低碳生活方式，营造绿色办公文化；\r\n5. 与客户、供应商、政府和NGO等外部利益相关方开展合作，共建绿色价值链。\r\n\r\n### 10.2 环境管理体系\r\n\r\n#### 一、组织架构与职责分工\r\n\r\n公司设立“环境与可持续发展小组”（简称“ESG小组”），隶属公司董事会战略委员会，统筹全公司环境治理工作，职责包括：\r\n\r\n- 制定年度环保目标与执行计划；\r\n- 协调各部门环境管理措施的落地；\r\n- 建立碳排放、能源使用、水资源等环境数据的统计和报告机制；\r\n- 推进公司ESG评级和可持续发展报告披露；\r\n- 协调外部审计与评估认证工作（如ISO 14001）。\r\n\r\n各事业部配备环保联络员，协助执行具体措施，形成“总部统筹、部门协作、员工参与”的三级环境管理体系。\r\n\r\n#### 二、绿色政策覆盖范围\r\n\r\n| 领域             | 政策举措与说明                                               |\r\n| ---------------- | ------------------------------------------------------------ |\r\n| 办公场所         | 节能照明、空调温控、楼宇能效监测、办公楼绿化比达30%以上      |\r\n| 数据中心         | 引入绿色机房标准，优化PUE指标（&lt;1.5），使用液冷系统减少能耗  |\r\n| 运输出行         | 出差优先选择高铁和新能源交通工具，内部班车采用电动车辆       |\r\n| 采购管理         | 与绿色认证供应商合作（如RoHS、EPEAT、ISO14001认证），优先选择环保包装和可再生材料 |\r\n| IT设备更新       | 推行设备延寿计划，回收淘汰设备并交由合规处理商分解回收       |\r\n| 废弃物分类与管理 | 设立电子废弃物、纸张、塑料、餐厨垃圾分类点，张贴清晰指引标识 |\r\n\r\n### 10.3 节能减排与资源管理\r\n\r\n#### 一、能源管理\r\n\r\n公司通过多项技术与管理手段降低电力消耗：\r\n\r\n- 全员办公区推广LED节能灯具，设定夜间自动熄灯机制；\r\n- 空调系统定期清洗与智能温控（夏季不低于26℃，冬季不高于20℃）；\r\n- 服务器部署边缘计算技术，实现任务调度优化，降低高峰期耗电；\r\n- 所有新建/改建楼宇需满足绿色建筑二星级标准。\r\n\r\n#### 二、水资源与办公耗材管理\r\n\r\n- 安装节水型水龙头与感应式冲洗系统；\r\n- 鼓励员工自带水杯，公司统一发放可重复使用水瓶；\r\n- 纸张使用遵循“双面打印+无纸化优先”原则，禁止非必要文件打印；\r\n- 所有打印机设定默认黑白/草稿模式，文印室张贴“节纸提醒”海报；\r\n- 年度耗材消耗控制在2022年基准线以下，制定部门限额预警机制。\r\n\r\n#### 三、碳排放与碳中和目标\r\n\r\n- 公司承诺在2035年前实现“运营碳中和”（涵盖办公能耗、员工通勤、数据中心等范畴）；\r\n- 已完成2023年碳排查（Scope 1和Scope 2）；\r\n- 逐步引入可再生能源（如绿电采购、光伏屋顶试点）；\r\n- 启动“碳账户”项目，试行个人碳积分记录机制，用于年度评优、奖品兑换等。\r\n\r\n### 10.4 绿色产品与服务倡议\r\n\r\n#### 一、绿色设计原则\r\n\r\n公司技术研发部门已将“绿色设计”纳入产品开发流程（Green by Design）：\r\n\r\n- 优化产品算法能耗比，减少高功耗计算任务；\r\n- 提高系统容错率与组件可更换性，延长产品生命周期；\r\n- 推广模块化设计，提升维修便利性与资源再利用率；\r\n- 在产品说明书中标注“环保使用建议”，指导客户节能操作。\r\n\r\n#### 二、数字化助力客户减碳\r\n\r\n- 推出“低碳办公SaaS平台”，帮助企业客户测量办公行为碳足迹；\r\n- 提供数据中心能效分析工具，优化资源调度与负载均衡；\r\n- 与生态伙伴共建“绿色云服务平台”，承诺使用清洁能源比例达80%以上；\r\n- 面向政府机构和高校推出“碳中和管理方案咨询”服务包。\r\n\r\n### 10.5 员工绿色行为倡导\r\n\r\n#### 一、日常行为规范\r\n\r\n公司鼓励每一位员工在日常工作中践行环保承诺：\r\n\r\n| 行为建议 | 实施说明                                                     |\r\n| -------- | ------------------------------------------------------------ |\r\n| 通勤出行 | 倡导步行、骑行或公共交通，发放绿色出行补贴；私家车通勤员工可选择参与“碳抵消项目” |\r\n| 会议活动 | 优先选择线上会议；线下会议统一提供可降解纸杯与无塑料瓶水源   |\r\n| 食堂就餐 | 实行“按需取餐、拒绝浪费”倡议；每日剩餐量计入食堂绩效考核     |\r\n| 节能使用 | 午休时段关闭显示器与照明设备；下班前检查电脑/空调关闭情况    |\r\n| 志愿行动 | 鼓励员工参加环保志愿活动（如植树节、海滩净化行动），记入年度CSR激励档案 |\r\n\r\n#### 二、绿色积分与奖励机制\r\n\r\n为鼓励员工持续参与环保，公司建立“绿色积分平台”：\r\n\r\n- 绿色行为（如少开车、节约用纸、参与培训等）可获得积分；\r\n- 每季度积分可兑换环保文具、定制纪念品、公益捐赠；\r\n- 部门累计积分纳入年终CSR绩效评估。\r\n\r\n### 10.6 培训、沟通与文化建设\r\n\r\n#### 一、员工培训\r\n\r\n- 新员工入职环节增加“可持续发展导论”模块，普及基础环保知识；\r\n- 所有技术、采购、项目管理岗须完成年度“绿色流程规范”课程；\r\n- 管理层参与“绿色领导力”培养项目，建立生态治理意识。\r\n\r\n#### 二、宣传与激励\r\n\r\n- 每月在企业内网推送“绿色生活小贴士”；\r\n- 开展“绿色一周”、“节能行动月”、“地球日主题周”等专题活动；\r\n- 评选“绿色榜样员工”、“低碳先锋部门”，授予荣誉证书与奖金；\r\n- 设立“可持续发展倡议箱”，鼓励员工提交节能环保创意。\r\n\r\n### 10.7 ESG合规、披露与外部合作\r\n\r\n#### 一、环境合规监测\r\n\r\n- 公司所有运营场所每年需完成一次环境合规自查；\r\n- 定期接受第三方机构绿色审计，问题整改记录纳入年度治理报告；\r\n- 建立内部举报机制，发现违规排放、非法弃置等问题可匿名举报。\r\n\r\n#### 二、可持续信息披露\r\n\r\n- 公司发布《年度环境与社会责任报告》，公开碳排放、节能成果、ESG目标进展；\r\n- ESG报告参照GRI标准，并接受外部专家委员会审阅；\r\n- 已参与CDP碳信息披露计划，并获得“B”等级以上评分。\r\n\r\n#### 三、生态伙伴与公益合作\r\n\r\n- 与阿拉善SEE基金会、中国绿色碳汇基金、联合国可持续发展组织等开展公益合作；\r\n- 联合高校推进“绿色技术挑战赛”“青年低碳研究基金”等项目；\r\n- 支持员工参与联合国“清洁地球计划”行动，提供带薪环保假期（每人每年2天）。\r\n\r\n### 10.8 未来展望\r\n\r\n我们深知，可持续发展之路没有终点。面对日新月异的环境挑战，公司将持续：\r\n\r\n- 探索碳金融与内部碳定价机制；\r\n- 试点绿色债券发行、碳交易平台参与；\r\n- 推动上下游协同减碳与绿色供应链建设；\r\n- 引入AI与区块链技术监控环境绩效数据；\r\n- 加强跨部门协作，确保“每一项业务都绿色、每一位员工都参与”。\r\n\r\n我们坚信，绿色不仅是一种责任，更是一种竞争力。在未来的发展征程中，愿我们共同以技术创新为引擎，以生态文明为指南，以每一位员工的绿色行为为起点，汇聚起推动社会、经济与自然协同发展的强大力量。\r\n\r\n## 第十一章：离职程序\r\n\r\n员工的离职是企业人力资源生命周期管理中的重要环节，既关系到员工个人职业路径的转折，也直接影响组织稳定性与知识资产的完整传承。为保障员工与公司双方的合法权益，维护良好的雇佣关系氛围，创新科技股份有限公司（以下简称“公司”）制定了科学、规范、人性化的离职流程及相关制度。本章旨在明确不同类型离职的适用流程、员工与部门的权责划分、相关手续办理及信息交接机制等内容，确保离职过程高效、透明、有序，并为双方保留长期合作与沟通的良好基础。\r\n\r\n### 11.1 离职类型分类\r\n\r\n公司认可并处理以下几种主要类型的员工离职情形：\r\n\r\n#### （一）主动离职\r\n\r\n员工基于个人职业发展、生活规划、家庭变故、教育进修或其他主观意愿提出辞职。该类型离职应提前提出正式书面申请，并完成相应的离职交接流程。\r\n\r\n#### （二）协议解除\r\n\r\n公司与员工通过协商一致的方式终止劳动合同，例如项目调整、组织优化、岗位取消或双方基于现实情况自愿达成一致。公司会依法给予经济补偿。\r\n\r\n#### （三）试用期不通过\r\n\r\n在员工试用期间内，若经考核不符合录用标准，或员工自行决定不继续履职，双方可根据试用期条款解除劳动关系。\r\n\r\n#### （四）违纪解除（单方解除）\r\n\r\n若员工严重违反公司规章制度、道德规范或触犯法律法规，公司有权根据《员工行为准则》及国家法律规定单方面解除劳动合同，视情况决定是否支付经济补偿或追究法律责任。\r\n\r\n#### （五）自动离职（视为放弃）\r\n\r\n若员工连续旷工3日（或月累计5日）以上，且无合理解释，视为“自动离职”；公司有权单方解除合同并保留追偿损失的权利。\r\n\r\n#### （六）退休/病退/死亡\r\n\r\n符合国家退休政策的员工，在达到法定年龄或申请提前退休时，可依法办理退休手续；患重大疾病不宜继续履职的员工可根据相关政策申请病退；员工去世视为自然解除劳动合同，公司将协助家属完成后续事务处理。\r\n\r\n### 11.2 离职申请流程\r\n\r\n对于主动辞职或协商解除，员工应严格按照以下步骤执行：\r\n\r\n#### 一、提交书面辞职申请\r\n\r\n- 员工需通过公司内网“员工服务系统”或以纸质形式向直属上级提出辞职申请，说明离职原因与拟定离职日期；\r\n- 若为管理岗、技术骨干或项目关键岗位，建议提前30个工作日提出辞职申请，以便公司妥善安排交接与人力资源配置。\r\n\r\n#### 二、部门审批\r\n\r\n- 直属主管在接收到辞职申请后，应在3个工作日内与员工进行离职沟通；\r\n- 若存在特殊情况（如不可替代性、冲突期间等），可与HR协商决定是否适当延后离职时间；\r\n- 审批后，主管需在系统内提交意见报告，HR方可启动后续流程。\r\n\r\n#### 三、HR审核与面谈\r\n\r\n- 人力资源部收到主管审批意见后，将安排离职面谈；\r\n- 面谈内容包括员工离职原因调查、对公司建议反馈、薪酬及福利结算说明、后续就业建议等；\r\n- HR将判断是否为正常离职，是否存在经济补偿、保密义务延续、竞业协议执行等需进一步处理的事项。\r\n\r\n#### 四、离职审批表签署\r\n\r\n- 员工需逐一完成以下部门签字流程：\r\n  - 所在部门主管；\r\n  - 人力资源部；\r\n  - 财务部（工资结算、报销审核）；\r\n  - IT部门（办公设备归还、账户注销）；\r\n  - 行政部（门禁卡、工牌、办公物品归还）；\r\n  - 法务部（如涉及合同责任、竞业协议等）。\r\n- 所有签字需在最后工作日前完成，特殊情况需主管提交书面说明申请延后。\r\n\r\n#### 五、离职通告与系统更新\r\n\r\n- HR将在员工最后工作日前，通过公司内部OA平台或邮箱发布正式离职通告，明确交接人选、职责划分；\r\n- 所有权限系统将同步进行账户注销或冻结，包括邮件、VPN、内网知识库、业务平台等。\r\n\r\n### 11.3 工作交接流程\r\n\r\n为确保组织连续性与知识资产传承，离职员工有义务履行以下交接责任：\r\n\r\n#### 一、任务与项目文档归档\r\n\r\n- 所有当前参与项目需整理项目背景、执行方案、关键节点、客户对接资料、进度状态；\r\n- 提交项目总结报告，并对接指定接替人进行口头与文档说明。\r\n\r\n#### 二、客户资源移交\r\n\r\n- 离职员工不得私自与客户解除联系或转移客户至新东家；\r\n- 客户沟通需纳入公司统一平台或CRM系统归档，由部门经理监控交接过程；\r\n- 离职后不得擅自使用或传播客户信息。\r\n\r\n#### 三、代码/技术资产归档\r\n\r\n- 技术岗位员工需将研发代码提交至公司代码库，明确版本标记与维护建议；\r\n- 提供API文档、注释说明、部署脚本及调试手册；\r\n- 配合技术负责人完成测试环境交接与上线支持计划。\r\n\r\n#### 四、知识库与培训材料交接\r\n\r\n- 如员工为讲师、导师、知识管理参与者，应移交其创建的文档、课件、流程图；\r\n- 若员工承担培训职责，应提前协助指定交接人熟悉流程并做好支持计划。\r\n\r\n### 11.4 薪酬、福利及经济补偿\r\n\r\n员工离职时，公司将依法、公平地处理各项结算事宜：\r\n\r\n#### 一、工资结算\r\n\r\n- 正常离职员工工资将按最后工作日至当月结算，次月发放；\r\n- 未休年假将根据国家规定换算为工资补偿（仅限合法节假日和年休）；\r\n- 如有欠薪、加班费、津贴、报销未结，须在离职面谈时提交财务复核申请。\r\n\r\n#### 二、社保与公积金处理\r\n\r\n- 离职当月社保/公积金由公司正常缴纳至最后工作日；\r\n- 员工可选择办理转移或暂停手续，HR将协助提供所需资料；\r\n- 外籍员工或异地社保政策特殊，按各地社保机构具体要求办理。\r\n\r\n#### 三、商业保险、补贴与贷款清理\r\n\r\n- 员工离职后，公司统一商业保险（含意外险、补充医疗险）于次月失效；\r\n- 已发放但尚未完成条件的购房补贴、学历提升补贴，如有提前离职违约条款，将按照比例退还；\r\n- 若员工存在内部无息借款、设备租赁、购置预付款等，须在离职前完成清偿手续。\r\n\r\n#### 四、竞业限制及保密义务延续\r\n\r\n- 如员工与公司签署了竞业限制协议，HR及法务将依据协议规定支付竞业补偿金并确认限制范围与时限；\r\n- 所有员工离职后仍应严格遵守保密义务，不得披露或使用公司商业秘密。\r\n\r\n### 11.5 离职证明与档案转移\r\n\r\n- 员工正式离职后3个工作日内，公司将出具标准《离职证明》，载明入职时间、岗位、离职原因、绩效评语等；\r\n- 员工人事档案可根据申请转至指定单位或人才市场，党团组织关系转出须经工会或党委办公室协助；\r\n- 如员工需开具《工作推荐信》或第三方背景调查材料，由所在部门领导或HR出具授权书并统一对外口径。\r\n\r\n### 11.6 离职后沟通与回聘机制\r\n\r\n#### 一、离职员工问卷与回访\r\n\r\n- 离职满30日，公司将以问卷或电话形式对员工进行满意度调查；\r\n- HR将收集员工建议用于改进组织文化、绩效管理、职业成长机制等。\r\n\r\n#### 二、人才回流与返聘政策\r\n\r\n- 公司鼓励高绩效员工在外部发展后重返岗位，设有“创新归雁计划”，允许3年内员工优先参与内部空缺岗位；\r\n- 回聘员工将享受“工龄继承”“内部推荐人奖”等政策激励；\r\n- 离职期间无不良记录、无商业纠纷的员工将被纳入“高潜人才回访库”。\r\n\r\n### 11.7 特殊情形处理说明\r\n\r\n- 员工因涉嫌舞弊、泄密、骚扰、暴力等严重违纪行为被解聘，公司保留不予出具离职证明与追诉的权利；\r\n- 若员工因伤病申请离职，公司将根据《劳动合同法》及《工伤保险条例》核算补偿；\r\n- 涉及劳动仲裁、争议调解等法律程序的离职，将暂停正常流程，待仲裁结果确认后统一处理。\r\n\r\n### 11.8 结语\r\n\r\n员工离职并不意味着合作的终结，而是另一段职业旅程的开启。公司始终尊重每一位员工的选择，并致力于以职业化、温暖、有序的方式处理每一段劳动关系的结束。我们感谢所有曾在创新科技奋斗过的同仁，希望您在新的岗位继续闪耀光芒，也欢迎有朝一日重返创新，共绘蓝图。\r\n\r\n## 第十二章 附录\r\n\r\n附录部分旨在为员工提供手册正文之外的实用性信息支持和补充说明。本章将涵盖公司各项制度的关键文书模板、常用联络方式、常见问题解答（FAQ）、法律政策引用条目、名词解释等内容，帮助员工在工作中快速查找相关资源、处理制度事务并准确理解术语含义。本章内容随着组织流程、法规变化将动态维护，员工应定期通过公司内网【HR服务平台】查阅最新版本。\r\n\r\n### 12.1 常用表单与模板索引\r\n\r\n为提升员工制度执行效率，公司提供以下表单与模板文档，员工可通过【HR服务平台】-【制度文库】-【标准模板】下载使用：\r\n\r\n| 表单名称                 | 用途说明                                   | 下载路径                           |\r\n| ------------------------ | ------------------------------------------ | ---------------------------------- |\r\n| 员工入职登记表           | 员工入职时填写，记录基础信息与岗位职责     | HR服务平台 &gt; 表单下载 &gt; 入职类     |\r\n| 试用期评估表             | 用于试用期满考核评定                       | HR服务平台 &gt; 表单下载 &gt; 考评类     |\r\n| 请假申请单               | 所有类型的假期申请须提交此表               | HR服务平台 &gt; 表单下载 &gt; 出勤类     |\r\n| 加班申报表               | 加班记录与审批使用                         | HR服务平台 &gt; 表单下载 &gt; 出勤类     |\r\n| 薪酬调整申请表           | 薪资、补贴变动的审批文档                   | HR服务平台 &gt; 表单下载 &gt; 薪酬类     |\r\n| 异动申请表               | 部门调动、岗位调整使用                     | HR服务平台 &gt; 表单下载 &gt; 岗位类     |\r\n| 离职申请与交接表         | 员工辞职与手续办理专用                     | HR服务平台 &gt; 表单下载 &gt; 离职类     |\r\n| 年度绩效评估表           | 年终评估使用，HR会提供当前年度版本         | HR服务平台 &gt; 表单下载 &gt; 考评类     |\r\n| 员工建议与反馈表         | 员工就制度、流程、文化等提出建议或问题反馈 | HR服务平台 &gt; 表单下载 &gt; 互动类     |\r\n| 保密协议签署模板         | 员工签订正式保密责任文件使用               | HR服务平台 &gt; 表单下载 &gt; 法务类     |\r\n| 竞业限制协议模板         | 关键岗位员工签署的竞业协议                 | HR服务平台 &gt; 表单下载 &gt; 法务类     |\r\n| 商业保险理赔申请表       | 员工申请公司团体意外险或医疗补偿时使用     | HR服务平台 &gt; 表单下载 &gt; 福利类     |\r\n| 员工健康状况报告与申报表 | 体检异常或传染病申报必填                   | HR服务平台 &gt; 表单下载 &gt; 健康安全类 |\r\n\r\n&gt; 注：部分表单需经所属部门领导审批盖章后提交至HR，电子签批无效时将另行注明。\r\n\r\n### 12.2 公司部门联系方式一览\r\n\r\n| 部门名称     | 负责人   | 联系电话      | 办公地点          | 对外邮箱                                                  |\r\n| ------------ | -------- | ------------- | ----------------- | --------------------------------------------------------- |\r\n| 人力资源部   | 王颖 HRD | 010-8921-2201 | 北京总部 A座5层   | [hr@innovatech.cn](mailto:hr@innovatech.cn)               |\r\n| 行政与后勤部 | 刘浩     | 010-8921-2234 | 北京总部 A座4层   | [admin@innovatech.cn](mailto:admin@innovatech.cn)         |\r\n| 财务部       | 张倩     | 010-8921-2215 | 北京总部 A座6层   | [finance@innovatech.cn](mailto:finance@innovatech.cn)     |\r\n| 法务合规部   | 周云杰   | 010-8921-2299 | 北京总部 A座7层   | [legal@innovatech.cn](mailto:legal@innovatech.cn)         |\r\n| 信息安全部   | 李伟     | 010-8921-2280 | 北京总部 A座8层   | [infosec@innovatech.cn](mailto:infosec@innovatech.cn)     |\r\n| 技术研发中心 | 黄振宇   | 010-8921-2401 | 上海分部 T2研发区 | [techlab@innovatech.cn](mailto:techlab@innovatech.cn)     |\r\n| 市场与品牌部 | 陈晓曦   | 010-8921-2601 | 广州分部 C楼3层   | [marketing@innovatech.cn](mailto:marketing@innovatech.cn) |\r\n| 员工帮助热线 | 客服团队 | 400-801-2768  | 支持远程工作时间  | [support@innovatech.cn](mailto:support@innovatech.cn)     |\r\n\r\n&gt; 内部联系推荐优先使用公司企业微信协作平台；所有非紧急事务请尽量使用邮件说明。\r\n\r\n### 12.3 常见问题与处理指引（FAQ）\r\n\r\n#### Q1：我可以在试用期请假或申请调休吗？\r\n\r\n可以。试用期员工同样享有病假、事假、婚假等国家规定法定假期权利。但累计请假超过10个工作日者，试用期考核结果可能会受影响。\r\n\r\n#### Q2：我每月的考勤异常如何确认？\r\n\r\n考勤异常记录可通过【钉钉考勤助手】-【异常打卡查询】功能查看。若有误记，请于当月25日前联系直属上级及HR进行纠正。\r\n\r\n#### Q3：公司薪资结构公开吗？绩效奖金如何计算？\r\n\r\n公司对岗位薪资级别设有标准区间，但具体金额属个人隐私。绩效奖金按“基本绩效系数×公司经营绩效×个人评级”三因素综合确定。\r\n\r\n#### Q4：我是否可以拒绝加班？\r\n\r\n公司不强制安排无偿加班，员工应根据项目安排与主管协商。必要情况下（如发布节点、客户应急）需在合规范围内服从组织安排，并依法给予调休或加班费。\r\n\r\n#### Q5：如何处理工作中的冲突？\r\n\r\n首先建议与当事人沟通澄清；如无法自行解决，应向直属主管或HRBP反馈。公司提倡使用“非对抗式沟通模型”，HR也将视情况进行协调介入。\r\n\r\n#### Q6：我发现信息安全隐患，如何上报？\r\n\r\n请立即通过【信息安全隐患上报表】提交至 [infosec@innovatech.cn](mailto:infosec@innovatech.cn)。高危事件（如员工电脑感染勒索病毒）应第一时间电话通知信息安全部。\r\n\r\n#### Q7：我如何申请岗位调动或内部转岗？\r\n\r\n请填写《异动申请表》，获得现岗位主管及目标岗位主管双签后提交至HR，由HRBP协调面谈及流程安排。\r\n\r\n#### Q8：如发生工伤，公司如何处理？\r\n\r\n请及时就医并保留完整就诊单据，同时通知主管及HR备案，公司将协助员工申请工伤鉴定并协助办理保险赔付。\r\n\r\n### 12.4 法律与政策依据条目\r\n\r\n以下为员工手册制定过程中所参照的国家法规与地方实施细则部分摘要，员工如需详细信息可联系法务部获取法规原文：\r\n\r\n- 《中华人民共和国劳动法》（2022年修订版）\r\n- 《中华人民共和国劳动合同法》（含实施条例）\r\n- 《职工带薪年休假条例》\r\n- 《中华人民共和国社会保险法》\r\n- 《企业职工奖惩条例》\r\n- 《女职工劳动保护特别规定》\r\n- 《中华人民共和国个人信息保护法》（2021年施行）\r\n- 《中华人民共和国民法典》合同编、人格权编相关条款\r\n- 《用人单位劳动保障守法诚信等级评价办法》\r\n- 《国家网信办个人信息出境标准合同办法》\r\n\r\n### 12.5 专有名词释义\r\n\r\n| 术语                                  | 定义                                                         |\r\n| ------------------------------------- | ------------------------------------------------------------ |\r\n| HRBP（Human Resource BP）             | 人力资源业务伙伴，嵌入业务一线支持招聘、绩效、发展等战略落地 |\r\n| KRA（Key Result Area）                | 关键结果领域，是员工绩效考核的重要结构之一                   |\r\n| C&amp;B（Compensation &amp; Benefit）         | 薪酬与福利管理领域的常用术语                                 |\r\n| PIP（Performance Improvement Plan）   | 绩效改善计划，用于支持绩效低于标准员工的过渡方案             |\r\n| NDA（Non-Disclosure Agreement）       | 保密协议，禁止员工在任职或离职后泄露公司敏感信息             |\r\n| BYOD（Bring Your Own Device）         | 员工自带设备办公政策，用于界定信息保护边界                   |\r\n| EHS（Environment, Health and Safety） | 企业环保、安全、健康责任系统框架                             |\r\n| L&amp;D（Learning &amp; Development）         | 学习与发展计划，包括岗位培训、课程体系、成长路径             |\r\n\r\n### 12.6 修订记录与版本说明\r\n\r\n| 版本号 | 修订时间   | 修订内容摘要                   | 修订人      | 备注                   |\r\n| ------ | ---------- | ------------------------------ | ----------- | ---------------------- |\r\n| v1.0   | 2022-07-01 | 初版制定                       | HRD 王颖    |                        |\r\n| v2.0   | 2023-03-15 | 新增远程办公与线上考勤规范     | HRBP 陈涵   | 适应疫情常态化管理需要 |\r\n| v2.5   | 2024-04-10 | 更新绩效评估机制与晋升路径     | HRD 王颖    | 配合战略人才发展计划   |\r\n| v3.0   | 2025-07-01 | 全面修订各章节，完善福利与合规 | HR 法务团队 | 当前有效版本           |\r\n\r\n&gt; 员工应始终以最新版本员工手册为准，公司保留对附录内容的更新解释权。\r\n\r\n------\r\n\r\n如您对本员工手册附录内容仍有疑问、建议或发现未涵盖问题，欢迎通过以下渠道进行反馈：\r\n\r\n- 企业微信：进入“HR服务”小程序 →“我要提问”\r\n- 邮箱联系：[hr@innovatech.cn](mailto:hr@innovatech.cn)\r\n- 现场咨询：北京总部 A座5层人力资源部，工作日 9:00-18:00\r\n\r\n&#x27;, path=None, url=None, mimetype=None), image_resource=None, audio_resource=None, video_resource=None, text_template=&#x27;{metadata_str}\n\n{content}&#x27;)]
```

```
len(documents)
```

```
1
```

+   **如何查看底层结果：**

```
print(f&quot;加载了 {len(documents)} 个文档&quot;)
print(f&quot;文档内容预览: {documents[0].text[:100]}...&quot;) # 查看前100个字符
print(f&quot;元数据: {documents[0].metadata}&quot;)
```

```
加载了 1 个文档
文档内容预览: # 创新科技股份有限公司员工手册

## 前言

尊敬的各位员工：

欢迎您加入创新科技股份有限公司（以下简称“公司”或“创新科技”）。在您正式开启职业生涯新阶段的这一时刻，我们谨代表全体...
元数据: {&#x27;file_path&#x27;: &#x27;D:\\work\\jupyter\\公开课项目\\1211 Llama-index入门\\data\\创新科技股份有限公司员工手册.txt&#x27;, &#x27;file_name&#x27;: &#x27;创新科技股份有限公司员工手册.txt&#x27;, &#x27;file_type&#x27;: &#x27;text/plain&#x27;, &#x27;file_size&#x27;: 116573, &#x27;creation_date&#x27;: &#x27;2025-12-11&#x27;, &#x27;last_modified_date&#x27;: &#x27;2025-12-11&#x27;}
```

**第二步：**索引构建 (Indexing Pipeline) —— 黑盒核心

```
index = VectorStoreIndex.from_documents(documents)
```

+   **功能逻辑：** 这行代码启动了数据处理流水线。它将粗粒度的 Document 转化为适合检索的细粒度向量索引。

+   默认参数下的后台动作 (The Default Pipeline)：

1.  **切分 (Parsing):** 调用 **Sent****enceS**plitter，将《员工手册》切分成多个小的 Node (节点)。

+   默认大小： chunk_size=1024 (Token)。

+   默认重叠： chunk_overlap=20 (Token)。

1.  **嵌入 (Embedding):** 调用 OpenAI 的 text-embedding-ada-002 模型，将每个 Node 的文本转化为 1536 维的向量。

1.  **存储 (Storage):** 将向量和文本保存在内存中的 SimpleVectorStore（即 Python 字典结构）。

```
# 查看切分后的 Node 数量
print(f&quot;文档被切分成了 {len(index.docstore.docs)} 个节点&quot;)
# 查看第一个节点的实际内容
nodes = list(index.docstore.docs.values())
print(f&quot;节点1内容: {nodes[0].text}&quot;)
```

```
文档被切分成了 58 个节点
节点1内容: # 创新科技股份有限公司员工手册

## 前言

尊敬的各位员工：

欢迎您加入创新科技股份有限公司（以下简称“公司”或“创新科技”）。在您正式开启职业生涯新阶段的这一时刻，我们谨代表全体管理层及全体同仁，对您的加入表示最热烈的欢迎和最诚挚的祝贺。无论您是刚步入职场的应届毕业生，还是经验丰富的行业专家，我们都深信，您的才华、热情与努力将为公司注入新的活力，也将在这里收获丰富的成长与成就。

创新科技股份有限公司自2010年成立以来，始终秉持“科技改变未来，创新引领发展”的理念，致力于推动人工智能、云计算、大数据、物联网等前沿科技在各行业的应用落地，助力全球客户实现数字化转型和可持续发展。作为一家快速成长的高科技企业，我们的每一次突破、每一次扩张、每一次变革，无不凝聚着每一位员工的智慧与汗水。今天，我们已经拥有超过800位来自五湖四海、充满激情的同仁，分布在北京总部、上海、深圳、广州等主要城市，并在美国硅谷、新加坡等国际科技枢纽设有办事处。公司的服务范围广泛涵盖金融、医疗、教育、制造业、智慧城市等多个关键领域，并与多家国内外知名企业建立了长期稳定的合作关系。

我们深知，企业的可持续发展，离不开良好的企业文化和科学的人力资源管理制度。而这一切的基础，正是规范、清晰、公开、透明的员工政策体系。本员工手册的制定，旨在为所有员工提供一份详尽而实用的行为指南，帮助您全面了解公司的核心价值观、组织架构、行为规范、管理流程以及相关福利政策。它不仅是公司与员工之间的一份制度性文件，更是双方建立互信、合作与共同成长的重要载体。

在本手册中，您将系统地了解公司在多个层面的基本政策，包括但不限于雇佣制度、职业道德准则、信息安全与数据保护、绩效评估与职业发展、薪酬福利计划、员工健康与安全保障、多样性与包容性倡议、环境可持续发展目标，以及员工在不同生命周期阶段（包括入职、在职、调动、离职等）的应尽义务与享有权利。
```

**第三步：引擎配置 (Query E**ngine Configuration)

```
query_engine = index.as_query_engine()
```

+   **功能逻辑：** 将静态的索引数据库封装为可交互的问答引擎。这一步定义了 RAG 的检索策略和合成策略。

+   **默认参数行为：**

+   **检索策略：** **similarity**_top_k=2。意味着当用户提问时，系统只会在《员工手册》中寻找最相关的 2 个片段。

+   **合成策略：** response_mode='compact'。系统会将检索到的 2 个片段拼接进 Prompt，一次性发送给 LLM 生成答案。

第四步：执行查询 (Query Execution)

```
response = query_engine.query(&quot;请问，我们公司有病假政策么？&quot;)
# 注：原代码中的 aquery 为异步方法，适合 Web 服务；在教学演示中推荐使用 query
```

+   **功能逻辑：**

1.  Query Embedding: 将用户的自然语言问题转化为向量。

1.  Similarity Search: 计算问题向量与《员工手册》所有 Node 向量的相似度，找出 Top-2。

1.  **Synthesis:** 将 Top-2 的“病假相关条款”填入 Prompt 模板，发给 GPT-3.5/4。

1.  **Response:** LLM 返回“根据员工手册，本公司病假政策如下……”

+   如何查看底层结果（验证 RAG 是否有效）：
查看 **re**sponse 对象不仅能看到答案，还能看到依据（即溯源）。

```
print(&quot;=== AI 回答 ===&quot;)
print(response.response)

print(&quot;\n=== 参考依据 (Source Nodes) ===&quot;)
for node in response.source_nodes:
    print(f&quot;引用片段 (相似度 {node.score:.2f}):&quot;)
    print(node.node.text)
    print(&quot;----------------&quot;)
```

```
=== AI 回答 ===
公司有病假政策，员工可以享受最长为6个月的病假，具体的带薪比例根据工龄计算。

=== 参考依据 (Source Nodes) ===
引用片段 (相似度 0.83):
所有心理服务均由第三方合规服务机构执行，员工信息严格保密，公司不会获知具体内容。

#### 三、运动与健康生活激励

- 公司与多家健身机构合作，员工可使用年度健身补贴（最高2000元）；
- 每年组织两次“健康挑战月”，如“每日步数达标挑战”“零外卖挑战”等；
- 设置“健康积分商城”，积分可通过步数、饮食打卡等方式积累，兑换健康餐券、按摩器、体脂秤等福利；
- 办公场所设置休闲健身区，配备瑜伽垫、拉伸带、可调节桌椅；
- 鼓励员工参与骑行、登山、羽毛球、篮球等运动社团，公司按人均每月100元标准提供场地或装备补贴。

### 7.4 疾病防控与疫情响应

#### 一、传染病管理与响应机制

公司参考国家卫健委、疾控中心、世界卫生组织发布的标准，建立“传染病防控与应急响应机制”，适用于：

- 流感季节、疫情爆发阶段的快速反应；
- 个人确诊传染病后的隔离、通报和恢复流程；
- 密接人员的排查与健康管理；
- 疫苗接种倡导与协助服务。

公司设有“疫情响应小组”，由行政、HR、法务、医疗顾问构成，职责包括：

- 发布口罩佩戴、场所消杀、出入登记等管控政策；
- 设立居家办公政策、线上会议优先机制；
- 协助员工核酸检测与就医转诊；
- 处理传染病相关误解、歧视和信息发布。

#### 二、居家隔离与线上支持

如员工确诊传染病（如新冠、甲流、肺结核等），公司将提供以下支持：

- 正常病假薪酬与补贴；
- 所需物资配送服务（药品、口罩等）；
- 安排线上每日健康回访；
- 如员工居住地封控或有密接史，可申请临时远程办公；
- 部门主管需合理安排交接或延后不紧急任务，避免员工病中仍“被动加班”。

### 7.
----------------
引用片段 (相似度 0.83):
如无正当理由多次请假、延迟返岗或虚假请假，将依据公司纪律制度进行处理。

#### 四、远程工作要求

远程办公期间，员工应遵守以下规范：

- 每日早上9:30前通过指定平台签到；
- 使用远程日报系统提交工作计划与实际成果；
- 保持电话、即时通讯、视频会议工具畅通；
- 未经许可不得在远程期间从事与公司无关活动；
- 人力资源部与信息技术部将定期审计远程工作日志。

### 3.3 假期与休假

公司严格按照国家法律法规执行各类假期制度，并提供多种带薪假期和额外福利假期，保障员工身心健康与家庭照顾权利。

#### 一、法定节假日

员工享有以下**法定带薪节假日**（以当年国家人力资源与社会保障部公告为准）：

- 元旦：1天；
- 春节：7天；
- 清明节：1天；
- 劳动节：3天；
- 端午节：1天；
- 中秋节：1天；
- 国庆节：3天。

节假日前后如遇调休，HR部门将发布出勤调整安排。

#### 二、带薪年假

员工在公司服务满一周年后，享有带薪年假，计算方式如下：

| 工龄      | 年假天数 |
| --------- | -------- |
| 1 - 10年  | 10天     |
| 11 - 20年 | 15天     |
| 20年以上  | 20天     |

年假须提前申请、按月累计，未使用部分可顺延至次年3月31日前使用，逾期未休视为自动放弃，但可申请转为现金补偿。

公司鼓励员工**合理规划年假使用**，避免年底集中申请，保障业务连续性。

#### 三、其他带薪假期

| 假种类 | 时长      | 说明                           |
| ------ | --------- | ------------------------------ |
| 病假   | 最长6个月 | 按工龄计算带薪比例             |
| 产假   | 158天     | 按国家规定执行，含产前假       |
| 陪产假 | 15天      | 必须在配偶生产前后一个月内使用 |
| 婚假   | 10天      | 提供结婚证复印件               |
| 丧假   | 3天       | 限一等亲属，视情况延长         |

#### 四、特别假期与福利假

为提升员工满意度，公司增设以下弹性假期：

- **家庭照护假**：每年最多5天，用于照顾生病子女、父母；
- **心理健康日**：每半年可自选1天，事先申请，不需说明；
- **奖励假**：连续两次绩效评级“优秀”的员工可额外获得3天带薪奖励假。
----------------
```

**也就是说，通过这 5** 行代码，我们实际上构建了一个完整的非结构化数据处理管道。

+   **输入：** 本地 txt 文件（非结构化）。

+   **处理：** 切分 -> 向量化 -> 存储（结构化索引）。

+   **输出：** 基于事实的自然语言回答（知识服务）。

### 4.【选学】Llama-Index Agentic RAG基础实现流程

```
async def search_documents(query: str) -&gt; str:
    &quot;&quot;&quot;Useful for answering natural language questions about an personal essay written by Paul Graham.&quot;&quot;&quot;
    response = await query_engine.aquery(query)
    return str(response)
```

```
# Create an enhanced workflow with both tools
agent = FunctionAgent(
    tools=[multiply, search_documents],
    llm=llm,
    system_prompt=&quot;&quot;&quot;你是一名助人为乐的助手，能够快速准确的进行乘法运算，同时也能结合文档内容回答用户问题。&quot;&quot;&quot;,
)
```

```
response = await agent.run(&quot;请问，我们公司有病假政策么？&quot;)
```

```
2025-12-11 15:30:23,683 - INFO - HTTP Request: POST https://ai.devtool.tech/proxy/v1/chat/completions &quot;HTTP/1.1 200 OK&quot;
2025-12-11 15:30:25,973 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings &quot;HTTP/1.1 200 OK&quot;
2025-12-11 15:30:28,602 - INFO - HTTP Request: POST https://api.openai.com/v1/chat/completions &quot;HTTP/1.1 200 OK&quot;
2025-12-11 15:30:29,458 - INFO - HTTP Request: POST https://ai.devtool.tech/proxy/v1/chat/completions &quot;HTTP/1.1 200 OK&quot;
```

```
response
```

```
AgentOutput(response=ChatMessage(role=&lt;MessageRole.ASSISTANT: &#x27;assistant&#x27;&gt;, additional_kwargs={}, blocks=[TextBlock(block_type=&#x27;text&#x27;, text=&#x27;是的，我们公司的病假政策包括最多6个月的病假，薪资根据服务年限进行计算。&#x27;)]), structured_response=None, current_agent_name=&#x27;Agent&#x27;, raw={&#x27;id&#x27;: &#x27;chatcmpl-ClVQ533SKVq2vE7bl0brA8mUIQo4x&#x27;, &#x27;choices&#x27;: [{&#x27;delta&#x27;: {&#x27;content&#x27;: None, &#x27;function_call&#x27;: None, &#x27;refusal&#x27;: None, &#x27;role&#x27;: None, &#x27;tool_calls&#x27;: None}, &#x27;finish_reason&#x27;: &#x27;stop&#x27;, &#x27;index&#x27;: 0, &#x27;logprobs&#x27;: None}], &#x27;created&#x27;: 1765438229, &#x27;model&#x27;: &#x27;gpt-4o-mini-2024-07-18&#x27;, &#x27;object&#x27;: &#x27;chat.completion.chunk&#x27;, &#x27;service_tier&#x27;: &#x27;default&#x27;, &#x27;system_fingerprint&#x27;: &#x27;fp_aa07c96156&#x27;, &#x27;usage&#x27;: None, &#x27;obfuscation&#x27;: &#x27;By&#x27;}, tool_calls=[ToolCallResult(tool_name=&#x27;search_documents&#x27;, tool_kwargs={&#x27;query&#x27;: &#x27;病假政策&#x27;}, tool_id=&#x27;call_Z8zjXbbvIJr91ubMiEHLo2Ay&#x27;, tool_output=ToolOutput(blocks=[TextBlock(block_type=&#x27;text&#x27;, text=&#x27;The sick leave policy includes a maximum of 6 months of sick leave, with the pay calculated based on the length of service.&#x27;)], tool_name=&#x27;search_documents&#x27;, raw_input={&#x27;args&#x27;: (), &#x27;kwargs&#x27;: {&#x27;query&#x27;: &#x27;病假政策&#x27;}}, raw_output=&#x27;The sick leave policy includes a maximum of 6 months of sick leave, with the pay calculated based on the length of service.&#x27;, is_error=False), return_direct=False)], retry_messages=[])
```

![](img/7196cb0c318c95dfc0b4a4a88d7f58a9.png)

### 5.Llama-Index RAG系统默认参数与默认RAG流程

在**上一节的“5 行**代码”示例中，系统之所以能自动运行，是因为 Llama-Index 在后台加载了一套默认配置（Default Configuration）。但在实际生产环境中，默认参数往往无法满足中文语境或特定业务的需求。本节我们将深入系统的“驾驶舱”，学习如何查看与修改核心参数。自 v0.10.0 版本起，Llama-Index 引入了全局配置对象 Settings，它是控制 RAG 行为的核心枢纽。

#### 5.1 默认切分策略 (Chunking)

+   **默认配置：**

+   **切分器：** SentenceSplitter（句子切分器，优先保持句子完整）。

+   C**hunk Size **(块大小)： 1024 Token。这意味着每个节点包含约 1024 个 token 的文本。

+   C**hunk Ove**rlap (块重叠)： 20 Token。用于保持切片间的上下文连贯性。

```
from llama_index.core import Settings
```

```
# 查看默认值
print(f&quot;默认块大小: {Settings.chunk_size}&quot;)  
print(f&quot;默认重叠块大小: {Settings.chunk_overlap}&quot;)  
```

```
默认块大小: 1024
默认重叠块大小: 200
```

+   **修改后的效果：** 减小 Chunk Size 会增加生成**的节点（Node）总数，通常**能提升检索的精准度（Precision），但可能会切断长距离的语义关联；增加 Overlap 可以减少上下文丢失的风险。

#### 5.2 默认 Embedding 模型

+   **默认配置：**

+   **模型厂商：** **OpenAI**。

+   **具体模型：** text-embedding-ada-002 (1536 维)。

+   **注意：** 这意味着哪怕你只是在本地跑索引，每处理一段文本都会向 OpenAI 发起 API 请求并产生费用。

+   如何修改 (切换为本地开源模型)：
对于中文用户，默认的 OpenAI 模型效果虽然不错，但网络延迟和成本是问题。我们通常会切换为 HuggingFace 上的中文模型（如 BGE 或 M3）。

```
# 1. 获取当前的 Embedding 模型对象
embed_model = Settings.embed_model

# 2. embedding模型名字
print(f&quot;模型名称: {embed_model.model_name}&quot;)
```

```
模型名称: text-embedding-ada-002
```

#### 5.3 默认存储机制 (Storage)

+   **默认配置：**

+   **存储位置：** **内存 (In-Memory)**。

+   **存储类：** SimpleVectorStore。

+   **风险：** 这是一个临时存储。一旦你的 Python 脚本运行结束或程序**崩溃，花费**大量时间（和金钱）构建好的索引会直接消失。

+   **如何修改 (持久化到磁盘)：**
我们需要显式调用 persist() 方法将内存中的索引“刷”到硬盘上。

```
# 1. 构建索引时 (默认在内存)
index = VectorStoreIndex.from_documents(documents)

# 2. 保存到磁盘 (会在当前目录生成一个 storage 文件夹)
index.storage_context.persist(persist_dir=&quot;./storage&quot;)
```

```
2025-12-11 18:18:07,391 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings &quot;HTTP/1.1 200 OK&quot;
```

![](img/bbf233eb4328d1b717bd1716a02db3d8.png)

```
# 3. 下次启动时加载 (无需重新 Embedding)
# from llama_index.core import StorageContext, load_index_from_storage
# storage_context = StorageContext.from_defaults(persist_dir=&quot;./storage&quot;)
# index = load_index_from_storage(storage_context)
```

#### 5.4 默认检索参数 (Retrieval)

+   **默认配置：**

+   **Top-K：** **2**。即每次只找最相似的 2 个片段。

+   **后果：** 对于简单问题（如“公司地址在哪”）够用了，但对于复杂问题（如“总结公司五大核心价值观”），2 个片段可能无法覆盖全貌，导致回答不完整。

+   **如何查看与修改：**
这是**在生成查询引**擎时配置的，而不是在 Settings 里。

```
# 修改 Top-K 为 5，获取更多上下文
query_engine = index.as_query_engine(similarity_top_k=5)
```

#### 5.5 默认提示词模板 (Prompt Template)

+   **默认配置：**

+   **语言：** **纯英文**。

+   **内容：** Llama-Index 内置了一套通用的问答模板，大意是：“Context information is below... Given the context information... answer the question”。

+   **风险：**** 这是**中文 RAG 开发中最大的隐形坑。如果您的文档是中文，问题是中文，但提示词是英文，模型（尤其是能力较弱的小模型）很容易出现“中英夹杂”的回复，或者回答语气生硬。

+   **如何查看底层结果：**
提示词是在 QueryEngine 初始化时绑定的，我们可以通过 get_prompts() 方法把它“提取”出来看看。

```
prompts_dict = query_engine.get_prompts()

for key, prompt in prompts_dict.items():
    print(f&quot;=== 提示词类型: {key} ===&quot;)
    # 打印模板的具体内容
    print(prompt) 
    print(&quot;\n&quot;)
```

```
=== 提示词类型: response_synthesizer:text_qa_template ===
metadata={&#x27;prompt_type&#x27;: &lt;PromptType.QUESTION_ANSWER: &#x27;text_qa&#x27;&gt;} template_vars=[&#x27;context_str&#x27;, &#x27;query_str&#x27;] kwargs={} output_parser=None template_var_mappings={} function_mappings={} default_template=PromptTemplate(metadata={&#x27;prompt_type&#x27;: &lt;PromptType.QUESTION_ANSWER: &#x27;text_qa&#x27;&gt;}, template_vars=[&#x27;context_str&#x27;, &#x27;query_str&#x27;], kwargs={}, output_parser=None, template_var_mappings=None, function_mappings=None, template=&#x27;Context information is below.\n---------------------\n{context_str}\n---------------------\nGiven the context information and not prior knowledge, answer the query.\nQuery: {query_str}\nAnswer: &#x27;) conditionals=[(&lt;function is_chat_model at 0x00000198FECB36A0&gt;, ChatPromptTemplate(metadata={&#x27;prompt_type&#x27;: &lt;PromptType.CUSTOM: &#x27;custom&#x27;&gt;}, template_vars=[&#x27;context_str&#x27;, &#x27;query_str&#x27;], kwargs={}, output_parser=None, template_var_mappings=None, function_mappings=None, message_templates=[ChatMessage(role=&lt;MessageRole.SYSTEM: &#x27;system&#x27;&gt;, additional_kwargs={}, blocks=[TextBlock(block_type=&#x27;text&#x27;, text=&quot;You are an expert Q&amp;A system that is trusted around the world.\nAlways answer the query using the provided context information, and not prior knowledge.\nSome rules to follow:\n1. Never directly reference the given context in your answer.\n2. Avoid statements like &#x27;Based on the context, ...&#x27; or &#x27;The context information ...&#x27; or anything along those lines.&quot;)]), ChatMessage(role=&lt;MessageRole.USER: &#x27;user&#x27;&gt;, additional_kwargs={}, blocks=[TextBlock(block_type=&#x27;text&#x27;, text=&#x27;Context information is below.\n---------------------\n{context_str}\n---------------------\nGiven the context information and not prior knowledge, answer the query.\nQuery: {query_str}\nAnswer: &#x27;)])]))]


=== 提示词类型: response_synthesizer:refine_template ===
metadata={&#x27;prompt_type&#x27;: &lt;PromptType.REFINE: &#x27;refine&#x27;&gt;} template_vars=[&#x27;query_str&#x27;, &#x27;existing_answer&#x27;, &#x27;context_msg&#x27;] kwargs={} output_parser=None template_var_mappings={} function_mappings={} default_template=PromptTemplate(metadata={&#x27;prompt_type&#x27;: &lt;PromptType.REFINE: &#x27;refine&#x27;&gt;}, template_vars=[&#x27;query_str&#x27;, &#x27;existing_answer&#x27;, &#x27;context_msg&#x27;], kwargs={}, output_parser=None, template_var_mappings=None, function_mappings=None, template=&quot;The original query is as follows: {query_str}\nWe have provided an existing answer: {existing_answer}\nWe have the opportunity to refine the existing answer (only if needed) with some more context below.\n------------\n{context_msg}\n------------\nGiven the new context, refine the original answer to better answer the query. If the context isn&#x27;t useful, return the original answer.\nRefined Answer: &quot;) conditionals=[(&lt;function is_chat_model at 0x00000198FECB36A0&gt;, ChatPromptTemplate(metadata={&#x27;prompt_type&#x27;: &lt;PromptType.CUSTOM: &#x27;custom&#x27;&gt;}, template_vars=[&#x27;context_msg&#x27;, &#x27;query_str&#x27;, &#x27;existing_answer&#x27;], kwargs={}, output_parser=None, template_var_mappings=None, function_mappings=None, message_templates=[ChatMessage(role=&lt;MessageRole.USER: &#x27;user&#x27;&gt;, additional_kwargs={}, blocks=[TextBlock(block_type=&#x27;text&#x27;, text=&quot;You are an expert Q&amp;A system that strictly operates in two modes when refining existing answers:\n1. **Rewrite** an original answer using the new context.\n2. **Repeat** the original answer if the new context isn&#x27;t useful.\nNever reference the original answer or context directly in your answer.\nWhen in doubt, just repeat the original answer.\nNew Context: {context_msg}\nQuery: {query_str}\nOriginal Answer: {existing_answer}\nNew Answer: &quot;)])]))]
```

翻译如下：

=== 提示词类型: response_synthesizer:text_qa_template ===

metadata={'prompt_type': <PromptType.QUESTION_ANSWER: 'text_qa'>} template_vars=['context_str', 'query_str'] kwargs={} output_parser=None template_var_mappings={} function_mappings={} default_template=PromptTemplate(metadata={'prompt_type': <PromptType.QUESTION_ANSWER: 'text_qa'>}, template_vars=['context_str', 'query_str'], kwargs={}, output_parser=None, template_var_mappings=None, function_mappings=None, template='上下文信息如下。\n---------------------\n{context_str}\n---------------------\n给定上述上下文信息，不要使用先验知识，直接回答该查询。\n问题：{query_str}\n答案：') conditionals=[(<function is_chat_model at 0x00000198FECB36A0>, ChatPromptTemplate(metadata={'prompt_type': <PromptType.CUSTOM: 'custom'>}, template_vars=['context_str', 'query_str'], kwargs={}, output_parser=None, template_var_mappings=None, function_mappings=None, message_templates=[ChatMessage(role=<MessageRole.SYSTEM: 'system'>, additional_kwargs={}, blocks=[TextBlock(block_type='text', text="你是一个享誉全球的专家级问答系统。\n始终使用提供的上下文信息来回答查询，不要使用先验知识。\n需要遵循的一些规则：\n1. 绝不要在你的回答中直接引用给定的上下文（例如不要说‘根据上下文...’）。\n2. 避免使用像‘基于上下文...’或‘上下文信息显示...’或任何类似的表述。")]), ChatMessage(role=<MessageRole.USER: 'user'>, additional_kwargs={}, blocks=[TextBlock(block_type='text', text='上下文信息如下。\n---------------------\n{context_str}\n---------------------\n给定上述上下文信息，不要使用先验知识，直接回答该查询。\n问题：{query_str}\n答案：')])]))]

=== 提示词类型: response_synthesizer:refine_template ===

**me****ta**data={'prompt_type': <PromptType.REFINE: 'refine'>} template_vars=['query_str', 'existing_answer', 'context_msg'] kwargs={} output_parser=None template_var_mappings={} function_mappings={} default_template=PromptTemplate(metadata={'prompt_type': <PromptType.REFINE: 'refine'>}, template_vars=['query_str', 'existing_answer', 'context_msg'], kwargs={}, output_parser=None, template_var_mappings=None, function_mappings=None, template="原始问题如下：{query_str}\n我们提供了一个现有的答案：{existing_answer}\n我们有机会利用下方更多的上下文来完善现有答案（仅在需要时）。\n------------\n{context_msg}\n------------\n基于新的上下文，完善原始答案以更好地回答查询。如果上下文没有用，请返回原始答案。\n完善后的答案：") conditionals=[(<function is_chat_model at 0x00000198FECB36A0>, ChatPromptTemplate(metadata={'prompt_type': <PromptType.CUSTOM: 'custom'>}, template_vars=['context_msg', 'query_str', 'existing_answer'], kwargs={}, output_parser=None, template_var_mappings=None, function_mappings=None, message_templates=[ChatMessage(role=<MessageRole.USER: 'user'>, additional_kwargs={}, blocks=[TextBlock(block_type='text', text="你是一个专家级问答系统，在完善现有答案时严格按照两种模式运行：\n1. 重写：利用新上下文重写原始答案。\n2. 重复：如果新上下文没用，则重复原始答案。\n绝不要在你的回答中直接引用原始答案或上下文。\n如有疑问，直接重复原始答案即可。\n新上下文：{context_msg}\n问题：{query_str}\n原始答案：{existing_answer}\n新答案：")])]))]

#### 5.6 默认响应合成策略 (Response Synthesis)

+   **默认配置：**

+   **策略名称：** **compact**** (紧凑模式)**。

+   **工作原理：** **这是**为了节省 Token 和 API 调用次数而设计的默认策略。它会尝试将尽可能多的检索片段（Node）塞进一个 Prompt 窗口中发送给 LLM。如果片段太多塞不下，它才会分批处理。

+   **对比：** 早期版本或其他框架可能默认使用 refine（精炼模式，串行处理，慢但细致）或 tree_summarize（树状总结）。compact 是速度与成本的最佳平衡点。

+   **如何查看底层结果：**
合成策略封装在引擎的合成器（Synthesizer）内部。我们可以通过访问受保护属性来验证它。

```
synth = query_engine._response_synthesizer
print(f&quot;当前合成模式: {synth}&quot;) 
```

```
当前合成模式: &lt;llama_index.core.response_synthesizers.compact_and_refine.CompactAndRefine object at 0x00000198B2516900&gt;
```

+   Llama-Index基础默认参数

| 核心组件 | 默认参数 (Default) | 潜在风险/问题 | 推荐优化方向 (Optimization) |
| --- | --- | --- | --- |
| **切分 (Chunking)** | Size: 1024, Overlap: 200 | 粒度较粗，细节丢失 | 中文场景建议 Size: 256-512 |
| **向量 (Embedding)** | OpenAI (ada-002) | **产生费用**，依赖网络 | 切换为 BGE/M3 等本地开源模型 |
| **存储 (Storage)** | 内存 (RAM) | **关机即失**，无法持久化 | 必须使用 persist 保存到磁盘 |
| **检索 (Retrieval)** | Top-k: 2 | 上下文覆盖不足 | 根据文档长度调整为 3-5 |
| **提示词 (Prompt)** | **纯英文模板** | **回答中英夹杂**，指令遵循差 | 必须替换为中文 Prompt 模板 |
| **合成 (Synthesis)** | Compact (紧凑模式) | 对长文档总结能力一般 | 总结类任务改用 tree_summarize |

### 6.Llama-Index RAG系统核心组建与通用RAG流程

**通过前面的实操，我们已经跑通**了**一个基础的 RAG Demo。**现在，**让我们跳出代码细节，站在系统架**构的高度，俯瞰 Llama-Index 是如何将一个 RAG 系统拆解为三个标准化的工业阶段：数据装载 (Loading)、索引构建 (Indexing) 和 查询执行 (Querying)。

这一架构图景将帮助大家理解 Llama-Index 的核心设计哲学：将非结构化数据转化为结构化知识的流水线。

#### 第一阶段：数据装载 (Loading) —— 连接真实世界

+   **定义：**
  装载阶段是 RAG 系统的“入口”。它的核心任务是解决**“数据孤岛”**问题，将散落在磁盘文件、云端文档、数据库或 API 中的原始数据，统一提取并转换为 Llama-Index 能够理解的标准格式。

+   核心对象：Document (文档)
  这是该阶段的产物。Document 是一个数据容器，它不仅包含原始文本（Text），还承载了关键的属性信息（Metadata，如文件名、页码、作者）。此时的数据尚未被切分，仍保持原始形态。

+   **核心工具：**

+   SimpleDirectoryReader： 内置的最基础加载器，适用于读取本地文件夹中的文件（支持 .txt, .pdf, .docx, .ppt 等）。

+   LlamaHub (生态连接器)： 这是 Llama-Index 的核心护城河。它提供了数百种社区维护的加载器，能够连接 Notion、Discord、YouTube、SQL 数据库甚至 Google Drive。这意味着您不仅能读取本地文件，还能直接对接企业的 SaaS 数据资产。

![](img/0e2c159a06b042e9063550b74e2a3864.png)

#### 第二阶段：索引构建 (Indexing) —— 知识的原子化与结构化

+   **定义：**
这是 RAG **系统的“加工厂”。在此阶段**，**系统需要将粗粒度的 Docum**ent** 进行精细化处理，使其适**合数学计算和检索。这个过程通常包含切分 (Chunking)、向量化 (Embedding) 和 存储 (Storing)。

+   **核心对象：Node (节点)**
这是 Llama-Index 中的最小数据单元。

+   **概念梳理：** 一个 Document 会被切分成多个 Node。Node 不仅是一段文本切片，它还保留了指向原文档的引用（Ref_ID）以及与前后节点的逻辑关系（Prev/Next Node），这是实现高质量上下文检索的基础。

+   切分策略 (Splitters)：
Llama-Index 提供了多种针对不同数据类型的切分器，以保证语义的完整性：

+   SentenceSplitter (默认)： 按句子边界切分，适合通用文本。

+   **CodeSplitter：** 专为代码设计，支持 Python/Java 等语法树切分。

+   MarkdownSplitter： 按标题层级（H1, H2）切分，适合技术文档。

+   存储与扩展性 (Vector Stores)：
虽然默认使用内存存储**，但 Llama-Index 具有极强的可**扩展性。它允许通过统一接口（StorageContext）组装各类主流向量数据库，如 Chroma, Faiss, Pinecone, Milvus, Weaviate 等。这使得系统能够轻松从 Demo 扩展至亿级数据规模的生产环境。

![](img/350cae6c04305c157683a326b41f8e17.png)

#### 第三阶段：查询执行 (Querying) —— 智能交互与合成

+   **定义：**
这是 RAG 系统的“出口”。它的目标是理解用户的自然语言问题，从索引中精准找到答案，并组织语言进行回复。

+   核心组件：Query Engine (查询引擎)
它是检索器（Retriever）和合成器（Response Synthesizer）的封装体。

+   **关键概念与拓展：**

+   **检索 (Retrieval)：** 除了基础的“Top-K 向量相似**度搜索”，**Llama-I**ndex**** 还支持**关键词搜索（BM25）、混合搜索（Hybrid Search）以及递归检索（Recursive Retrieval）。

+   后处理 (Post-Processing)： 在检索后、生成前，引入重排序（Rerank）模型或元数据过滤器，剔除不相关内容。

+   响应合成 (Synthesis)： 决定如何利用检索到的信息。除了默认的“拼接（Compact）”，还支持“分步精炼（Refine）”和“树状总结（Tree Summarize）”等高级策略。

```

```

```

```

```

```

```
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader
```

```

```

```
documents = SimpleDirectoryReader(&quot;data&quot;).load_data()
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()
```

```
2025-12-11 15:28:51,340 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings &quot;HTTP/1.1 200 OK&quot;
```

```
documents 
```

```
[Document(id_=&#x27;fca88080-deb5-4097-b930-84c369d14f9b&#x27;, embedding=None, metadata={&#x27;file_path&#x27;: &#x27;D:\\work\\jupyter\\公开课项目\\1211 Llama-index入门\\data\\创新科技股份有限公司员工手册.txt&#x27;, &#x27;file_name&#x27;: &#x27;创新科技股份有限公司员工手册.txt&#x27;, &#x27;file_type&#x27;: &#x27;text/plain&#x27;, &#x27;file_size&#x27;: 116573, &#x27;creation_date&#x27;: &#x27;2025-12-11&#x27;, &#x27;last_modified_date&#x27;: &#x27;2025-12-11&#x27;}, excluded_embed_metadata_keys=[&#x27;file_name&#x27;, &#x27;file_type&#x27;, &#x27;file_size&#x27;, &#x27;creation_date&#x27;, &#x27;last_modified_date&#x27;, &#x27;last_accessed_date&#x27;], excluded_llm_metadata_keys=[&#x27;file_name&#x27;, &#x27;file_type&#x27;, &#x27;file_size&#x27;, &#x27;creation_date&#x27;, &#x27;last_modified_date&#x27;, &#x27;last_accessed_date&#x27;], relationships={}, metadata_template=&#x27;{key}: {value}&#x27;, metadata_separator=&#x27;\n&#x27;, text_resource=MediaResource(embeddings=None, data=None, text=&#x27;# 创新科技股份有限公司员工手册\r\n\r\n## 前言\r\n\r\n尊敬的各位员工：\r\n\r\n欢迎您加入创新科技股份有限公司（以下简称“公司”或“创新科技”）。在您正式开启职业生涯新阶段的这一时刻，我们谨代表全体管理层及全体同仁，对您的加入表示最热烈的欢迎和最诚挚的祝贺。无论您是刚步入职场的应届毕业生，还是经验丰富的行业专家，我们都深信，您的才华、热情与努力将为公司注入新的活力，也将在这里收获丰富的成长与成就。\r\n\r\n创新科技股份有限公司自2010年成立以来，始终秉持“科技改变未来，创新引领发展”的理念，致力于推动人工智能、云计算、大数据、物联网等前沿科技在各行业的应用落地，助力全球客户实现数字化转型和可持续发展。作为一家快速成长的高科技企业，我们的每一次突破、每一次扩张、每一次变革，无不凝聚着每一位员工的智慧与汗水。今天，我们已经拥有超过800位来自五湖四海、充满激情的同仁，分布在北京总部、上海、深圳、广州等主要城市，并在美国硅谷、新加坡等国际科技枢纽设有办事处。公司的服务范围广泛涵盖金融、医疗、教育、制造业、智慧城市等多个关键领域，并与多家国内外知名企业建立了长期稳定的合作关系。\r\n\r\n我们深知，企业的可持续发展，离不开良好的企业文化和科学的人力资源管理制度。而这一切的基础，正是规范、清晰、公开、透明的员工政策体系。本员工手册的制定，旨在为所有员工提供一份详尽而实用的行为指南，帮助您全面了解公司的核心价值观、组织架构、行为规范、管理流程以及相关福利政策。它不仅是公司与员工之间的一份制度性文件，更是双方建立互信、合作与共同成长的重要载体。\r\n\r\n在本手册中，您将系统地了解公司在多个层面的基本政策，包括但不限于雇佣制度、职业道德准则、信息安全与数据保护、绩效评估与职业发展、薪酬福利计划、员工健康与安全保障、多样性与包容性倡议、环境可持续发展目标，以及员工在不同生命周期阶段（包括入职、在职、调动、离职等）的应尽义务与享有权利。手册中的每一项制度、每一个流程，都是经过充分调研与反复论证制定而成，力求在保障公司合规运营与高效管理的同时，也最大限度地维护员工的合法权益和个人尊严。\r\n\r\n公司始终奉行“以人为本”的管理理念。我们不仅视员工为最宝贵的资产，更把员工的成长、发展与幸福作为企业成功的核心指标之一。在创新科技，您将拥有公平公正的发展平台、丰富多样的培训资源、具有市场竞争力的薪酬福利体系，以及健康积极的工作氛围。我们倡导开放沟通、跨部门协作和自我驱动，鼓励员工提出创新想法、挑战现状，不断优化流程、提升效率，实现个人价值的同时推动公司整体进步。\r\n\r\n值得特别指出的是，本员工手册虽然内容详尽，但其本质属性为指导性文件，并不构成劳动合同或任何形式的法律约束文件。然而，公司强烈建议您认真阅读并熟悉本手册内容，视其为您在公司工作期间的重要行为准则和行为指南。通过充分了解和遵守本手册规定的各项政策与流程，您不仅可以更快地融入团队、高效开展工作，还能在与同事及上级的互动中建立良好的职业关系，提升您的专业素养与领导力。\r\n\r\n为了确保手册始终与公司业务发展和法律法规保持一致，公司保留在必要时对本手册进行修改、补充或更新的权利。每一次修订将通过公司官方渠道进行公告，包括但不限于内部通知系统、电子邮件或HR门户网站，修订后的版本将自动替代之前版本并生效。员工有义务及时关注相关更新，并在疑惑或不解的情况下主动联系直属主管或人力资源部门进行咨询。公司人力资源部也将持续优化员工沟通机制，确保信息畅通、反馈及时、处理公正。\r\n\r\n在这个信息爆炸、技术快速更迭的时代，组织形态与工作方式正在发生深刻变革。面对数字化转型、远程办公、多元协作与跨文化团队的挑战与机遇，公司正在积极探索更高效的管理模式和更具人文关怀的工作文化。我们深知，仅凭制度无法塑造优秀的组织，只有通过信任、责任与合作，才能真正打造一个富有创造力与凝聚力的企业共同体。因此，除了书面制度外，我们更倡导一种“共创共担”的文化精神，希望员工在履行义务的同时，也能积极建言献策、共谋发展。\r\n\r\n例如，在公司行为准则部分，我们强调职业道德与合规经营，在保密义务与知识产权章节中，我们倡导严守法律责任并鼓励自主创新；在员工福利与假期管理方面，我们致力于提供兼顾生活质量与工作效率的弹性制度；而在多样性、公平与包容性政策下，我们努力营造一个尊重差异、平等参与的组织环境。这些政策不仅是一纸条文，更是公司文化的体现与员工体验的保障。我们希望每一位员工都能从中获得清晰的行为边界和发展的路线图，也希望每一位员工都能在这样的环境中实现自我突破与事业腾飞。\r\n\r\n在此，我们也向您郑重承诺：公司将一如既往地尊重并重视每一位员工的声音，持续优化员工体验，建立更具前瞻性、包容性和可持续性的组织管理机制。我们相信，员工与公司之间的关系，不仅是一种法律层面的雇佣关系，更是一种基于共同目标、共享价值和共同成长的事业伙伴关系。\r\n\r\n最后，请您在阅读完本手册后，签署《确认声明》并及时提交人力资源部归档。我们建议您将手册保留副本以供日后参考，同时也欢迎您在任何时候就手册内容提出建议与反馈。我们坚信，每一位员工的用心参与，都是推动公司持续进步的关键力量。\r\n\r\n再次感谢您选择加入创新科技股份有限公司。在接下来的旅程中，愿我们携手并肩，在科技浪潮中砥砺前行，共同开启属于我们的崭新时代。\r\n\r\n公司管理层\r\n 2025年7月11日\r\n\r\n## 第一章：公司概述\r\n\r\n在快速演变的科技时代，一个企业的发展不仅取决于技术和资本，更取决于其组织愿景、战略方向、文化体系与人才凝聚力。本章将全面介绍创新科技股份有限公司的使命、愿景、价值观、组织架构及雇佣原则，帮助员工更好地理解公司的本质与发展方向，增强归属感与责任感。\r\n\r\n### 1.1 公司使命与愿景\r\n\r\n#### 一、公司简介\r\n\r\n创新科技股份有限公司（以下简称“公司”或“创新科技”）成立于2010年，总部位于中国北京，是国家认定的高新技术企业，聚焦于人工智能（AI）、云计算、大数据与物联网（IoT）四大核心技术领域。自成立以来，公司始终秉持“以科技赋能社会，以创新驱动未来”的理念，致力于为全球客户提供先进、可靠、安全的数字化解决方案，推动各行业迈入智能化新时代。\r\n\r\n目前，公司员工人数已突破800人，分别在北京总部、上海、深圳、广州等重点城市设有研发和运营中心，并在美国硅谷、新加坡设立了全球业务拓展与技术合作办事处。公司业务横跨多个行业，包括但不限于金融科技、智慧医疗、智能制造、数字教育、交通出行与新型智慧城市。\r\n\r\n#### 二、使命宣言\r\n\r\n公司的使命是：“通过前沿技术创新，驱动全球数字化转型。”这不仅是一句口号，更是指导公司战略布局、产品开发、客户服务以及人才培养等各方面的核心理念。我们相信，科技的最终目的是服务人类、改善生活。因此，在技术研发的每一个细节中，我们都强调实用性、可持续性与社会责任，力求实现技术发展与人类福祉的平衡。\r\n\r\n#### 三、愿景展望\r\n\r\n公司的愿景是：“成为全球领先的科技服务提供商，构建人机共融的智能世界。”为实现这一愿景，我们不断投资前沿技术研究，探索人机协作、数字孪生、边缘计算、自然语言理解等关键领域。同时，我们也积极参与国际合作项目，与全球领先的大学、实验室及企业建立了开放式创新网络，共同探索未来科技的边界。\r\n\r\n#### 四、核心价值观\r\n\r\n我们始终坚持以下五项核心价值观，这些价值观深深嵌入公司管理体系、项目执行流程和团队协作文化中，构成创新科技的精神基石：\r\n\r\n- **创新（Innovation）**：我们鼓励员工提出大胆、前沿甚至颠覆性的想法，勇于实验、容错试错，建立内部孵化机制和知识共享平台，推动技术和管理持续迭代。\r\n- **诚信（Integrity）**：我们倡导真实、透明的交流方式，要求所有员工恪守职业操守、诚实守信、履职尽责。无论对客户、合作伙伴还是内部同仁，诚信始终是信任与合作的前提。\r\n- **协作（Collaboration）**：我们强调跨部门、跨区域、跨文化的高效协作。通过打造共创文化与团队协同机制，提升组织响应速度与整体执行力。\r\n- **卓越（Excellence）**：我们追求高质量标准，注重结果导向。无论是产品质量、服务体验还是运营流程，我们始终以客户满意度和行业标杆为目标，持续优化和迭代。\r\n- **责任（Responsibility）**：我们视社会责任为企业发展不可或缺的一部分，致力于环境保护、数据安全、教育公平、员工福祉等公益议题，真正做到“责任科技，科技有责”。\r\n\r\n#### 五、发展历程与里程碑\r\n\r\n公司自创立以来，历经十余年发展，已实现从初创团队到国际化科技企业的跨越。部分关键里程碑包括：\r\n\r\n- **2012年**：推出首个自主研发的云计算平台“CloudBase”，实现国内中型企业云部署自动化。\r\n- **2014年**：发布数据智能分析产品“Insight”，服务覆盖教育、零售、金融等三大行业。\r\n- **2016年**：AI实验室成立，聚焦自然语言处理、计算机视觉与自动驾驶算法。\r\n- **2018年**：获得国家高新技术企业认证，并荣获“最具成长性企业奖”。\r\n- **2020年**：与阿里云达成战略合作，打造混合云解决方案。\r\n- **2023年**：完成A轮融资，估值突破10亿美元。\r\n- **2024年**：新设可持续发展部门，开启“绿色科技”专项研发计划。\r\n\r\n这些成绩不仅是团队专业能力的体现，也彰显了公司在全球化竞争环境下的战略眼光与执行能力。\r\n\r\n#### 六、社会责任与公益参与\r\n\r\n创新科技高度重视企业社会责任，认为企业的发展必须与社会整体福祉同步。公司设有专门的社会责任小组，定期评估社会影响并制定行动计划。近年来，公司主要公益项目包括：\r\n\r\n- 每年投入固定比例利润支持“青少年科技教育基金”；\r\n- 组织技术员工为欠发达地区提供远程教育平台搭建与维护；\r\n- 参与“绿色城市”计划，向智慧城市平台引入碳排管理功能；\r\n- 设立员工志愿者项目，鼓励参与环保、教育与社区服务。\r\n\r\n我们相信，这些行动不仅帮助弱势群体，也激发了员工的使命感和归属感，提升了企业文化温度。\r\n\r\n### 1.2 组织结构\r\n\r\n#### 一、管理架构概览\r\n\r\n公司采用**矩阵式组织结构（Matrix Organization Structure）**，兼具职能部门与项目团队两种组织优势，旨在提高业务协同效率与资源配置灵活性。最高决策机构为董事会，由公司创始人、外部投资人及独立董事组成，负责制定公司发展战略、审议重大财务计划与监督运营管理。\r\n\r\n公司由首席执行官（CEO）领导，直接管理核心职能部门和各业务板块，向董事会汇报。CEO之下设有以下主要高级管理职位：\r\n\r\n- **首席运营官（COO）**：负责公司日常运营、流程优化、内部控制。\r\n- **首席技术官（CTO）**：领导技术战略、产品研发与技术生态构建。\r\n- **首席财务官（CFO）**：负责财务规划、成本控制、审计与资本运作。\r\n- **首席人力资源官（CHRO）**：统筹人才战略、组织文化、绩效与培训体系。\r\n- **首席市场官（CMO）**：统筹品牌传播、市场拓展与客户关系管理。\r\n\r\n各高管下设一级部门，部门负责人（Director或VP）直接向其汇报。\r\n\r\n#### 二、核心职能部门与职责\r\n\r\n以下为公司主要职能部门及其核心职责简述：\r\n\r\n- **人力资源部**：负责招聘与配置、绩效评估、员工关系、培训与发展、薪酬福利、组织文化建设。\r\n- **财务部**：负责预算管理、财务核算、税务合规、资金运作及投资分析。\r\n- **市场部**：聚焦品牌战略、客户调研、媒介投放、公关危机管理与市场活动执行。\r\n- **研发部**：由AI实验室、云平台团队、大数据平台组组成，专注前沿技术研究与产品开发。\r\n- **运营部**：负责内部流程管控、质量管理、供应链协作与项目推进管理。\r\n- **销售部**：负责客户关系、解决方案输出、项目招投标与合同管理。\r\n- **信息安全部**（新设）：制定与执行网络安全政策，防控数据泄露与合规风险。\r\n- **可持续发展部**：推动绿色能源、碳足迹监控、ESG报告发布等职责。\r\n\r\n公司在2024年新设立了“创新委员会”和“AI伦理审查小组”，分别统筹战略性项目孵化与技术伦理合规审查，体现公司在快速扩张期对内控与责任的重视。\r\n\r\n#### 三、跨部门沟通机制\r\n\r\n为了提高横向沟通效率，公司推行扁平化管理理念，鼓励基层员工与中高层管理者保持开放、双向的交流关系。公司内部门户系统汇聚组织架构图、通讯录、部门职能说明与工作流程文档，员工可随时查看并联络对口同事。\r\n\r\n每季度，公司召开一次“全员大会”，由CEO向全体员工通报业绩、战略动向与重点项目进展。部门之间则通过OKR联动会议、跨部门项目小组、内部Hackathon等形式，协同推进关键任务。\r\n\r\n### 1.3 雇佣原则\r\n\r\n#### 一、公平公正的雇佣制度\r\n\r\n公司严格遵循《中华人民共和国劳动法》及其他相关法律法规，承诺在招聘、晋升、奖惩、调岗等各类人事活动中贯彻**平等就业机会（Equal Employment Opportunity, EEO）**原则，不因员工的性别、种族、民族、年龄、宗教、婚姻状况、性取向、身体状况、政治信仰等受法律保护的特征而产生歧视行为。\r\n\r\n#### 二、招聘与录用流程\r\n\r\n我们推行结构化招聘流程，以确保候选人能力与岗位需求的高度匹配。具体步骤如下：\r\n\r\n1. **职位发布**：通过公司官网、主流招聘平台、校招系统和内部推荐机制发布岗位信息。\r\n2. **简历筛选**：采用AI辅助简历筛选工具，结合人工评估确保公正。\r\n3. **面试流程**：分为HR初试、业务面试、技术评估与文化适配面试，特殊岗位需额外参加案例演练。\r\n4. **背景调查**：包括学历验证、工作经历核查、推荐人反馈等。\r\n5. **录用与签约**：经审批后发出正式Offer，签署劳动合同、保密协议、知识产权归属协议等法律文件。\r\n\r\n#### 三、入职与融入机制\r\n\r\n新员工入职后将参与为期两周的**“迎新成长营”**培训计划，包括：\r\n\r\n- 企业文化、价值观导入；\r\n- 安全制度与合规培训；\r\n- 岗位职责、部门流程熟悉；\r\n- 团队协作活动与导师带教。\r\n\r\n此外，公司为新员工分配“职业导师”，帮助其在前三个月完成适应期评估，确保顺利融入。\r\n\r\n#### 四、多样性与包容性\r\n\r\n公司坚定推行多样性与包容性（Diversity &amp; Inclusion）战略，通过以下举措实现组织均衡：\r\n\r\n- 明确招聘目标，如女性技术人才比例提升计划；\r\n- 开设“职场平权”讲座与性别平衡专题培训；\r\n- 设置无障碍办公设施与远程协助系统；\r\n- 鼓励少数群体组建内部支持社群。\r\n\r\n截至2025年，女性员工比例已达40%，少数民族及外籍员工比例合计达15%。我们将继续努力，营造一个多元、包容、平等的职场生态。\r\n\r\n## 第二章：员工行为准则\r\n\r\n公司始终坚信，一个有秩序、高效且积极向上的组织环境，离不开全体员工对道德操守、组织规范和社会责任的共同遵守与坚守。为了明确员工在工作中应当遵循的行为标准，构建一个安全、公正、包容、富有责任感的职场环境，创新科技股份有限公司制定了本章“员工行为准则”，作为全体员工日常行为的指导方针和行为边界。\r\n\r\n本章将从职业道德、保密义务、知识产权、反歧视与反骚扰政策、冲突解决机制五个方面，系统阐述公司对员工在职业操守与行为规范方面的基本要求。\r\n\r\n### 2.1 职业道德\r\n\r\n#### 一、基本要求\r\n\r\n职业道德不仅关乎个人操守，更关乎整个组织的形象与信任基础。每位员工都应将诚实守信、尽职尽责、遵纪守法作为基本行为准则，时刻以公司利益、客户满意度与社会责任为重，不得利用职务便利谋取不当个人利益，不得参与损害公司声誉或资产的活动。\r\n\r\n以下为具体行为规范：\r\n\r\n- 所有员工在执行职责时，应确保信息真实、数据准确，严禁篡改、隐瞒或误导；\r\n- 员工不得在未申报情况下从事与公司业务存在竞争关系的兼职、投资或顾问活动；\r\n- 员工应避免与供应商、客户或合作方形成任何不正当的利益关联，包括接受现金、礼品、回扣、有价证券、宴请等；\r\n- 员工在代表公司参与外部活动时，应注意形象言行，不得传播与公司价值观相悖的言论；\r\n- 员工在使用社交媒体时，不得发布涉及公司机密、虚假陈述、恶意批评或煽动性质的内容；\r\n- 所有与客户、合作方和政府机构的接触均应依法合规，任何违规行为将承担严重后果。\r\n\r\n#### 二、道德培训与执行机制\r\n\r\n公司每年组织一次“职业道德与合规培训”，通过线上课程、案例研讨与模拟测试等方式，强化员工对风险场景的识别与应对能力。\r\n\r\n此外，公司设有**道德与合规监督办公室（Ethics Office）**，负责监督相关政策执行、受理投诉举报并开展专项审计。员工可通过匿名邮箱、热线电话或内部举报系统提交问题线索，公司承诺对举报内容保密并严禁任何形式的报复行为。\r\n\r\n### 2.2 保密义务\r\n\r\n#### 一、保密范围与分类\r\n\r\n在公司工作期间，员工将可能接触到各类商业秘密与敏感信息，包括但不限于：\r\n\r\n- 产品研发资料、源代码、算法文档；\r\n- 市场分析报告、客户名单、销售策略；\r\n- 财务报表、预算计划、投资信息；\r\n- 战略合作协议、投标文件、法律合同；\r\n- 内部邮件、管理制度、员工数据等。\r\n\r\n上述信息如被未经授权地泄露、传播、篡改或滥用，可能导致公司重大损失，甚至法律诉讼。为此，公司要求所有员工在入职前签署《保密协议》（NDA），承诺在在职及离职后2年内严格履行保密义务。\r\n\r\n#### 二、信息使用与传输规范\r\n\r\n- 员工在撰写、复制、下载、分享、保存公司内部文档时，必须遵循“最小权限原则”，仅限于业务所需的最小范围；\r\n- 禁止将敏感信息通过非加密邮件、公共网盘、U盘、聊天软件等非安全渠道外传；\r\n- 出差期间在公共场所使用笔记本、平板等设备处理工作事务时，应开启VPN与屏幕隐私保护；\r\n- 使用公司内部通讯工具（如企业微信、钉钉、OA系统）时，不得随意群发敏感信息，须按照岗位权限控制范围；\r\n- 离职员工必须归还所有资料、设备和访问权限，不得带离任何形式的公司数据。\r\n\r\n#### 三、违规行为处理\r\n\r\n公司设有**信息安全与数据合规审计机制**，定期抽查员工终端行为、网络流量与权限管理。如发现泄密行为，将依据《员工违纪处理细则》启动问责程序，必要时将追究法律责任。情节严重者，公司保留提起民事赔偿或刑事控告的权利。\r\n\r\n### 2.3 知识产权\r\n\r\n#### 一、归属原则\r\n\r\n公司依法拥有员工在雇佣期间所创作的以下成果的完整知识产权，包括但不限于：\r\n\r\n- 发明、专利、实用新型；\r\n- 软件代码、架构设计、技术论文；\r\n- 产品UI设计、数据库结构；\r\n- 实验结果、市场调研分析报告等。\r\n\r\n上述成果无论是否以“加班”或“远程办公”形式完成，均视为“职务成果”，归属于公司所有。员工须配合公司完成申报、登记、转让等手续，确保知识产权的合法归属。\r\n\r\n#### 二、创新激励机制\r\n\r\n公司高度重视员工的技术创造与方案贡献，建立了多层级的激励机制：\r\n\r\n- **专利奖励**：经公司申报并授权的发明专利最高奖励5万元；\r\n- **成果展示激励**：技术成果被纳入白皮书、官方展会或国际会议的，另行发放奖金；\r\n- **股权激励计划**：对于在核心项目中表现突出的骨干员工，公司将提供期权计划，作为长期激励措施；\r\n- **研发贡献积分系统**：通过内部系统累计积分，可兑换技术资源、培训机会或现金奖励。\r\n\r\n#### 三、避免侵权与合规使用\r\n\r\n员工在使用第三方资源时，必须严格遵循合法授权范围。常见情形包括：\r\n\r\n- 使用开源代码需注明许可证（如Apache、MIT）并遵守条款；\r\n- 禁止私自下载盗版软件、图像、文献或引用未经授权的数据；\r\n- 未经许可不得在外部公开演讲中披露公司专有算法或架构；\r\n- 所有对外发布的内容（如论文、博客、演示PPT）均需经主管审核批准。\r\n\r\n公司设有“知识产权管理委员会”，专责审查专利申报、侵权调查与法律合规。员工如发现可疑侵权行为，应立即向该委员会报告。\r\n\r\n### 2.4 反歧视与反骚扰政策\r\n\r\n#### 一、政策承诺\r\n\r\n创新科技致力于营造一个多元、平等、尊重的工作环境。我们坚定执行“零容忍”原则，禁止任何形式的歧视、骚扰或霸凌行为。无论行为是否出于恶意，一经核实，均将视情节严肃处理。\r\n\r\n#### 二、禁止行为示例\r\n\r\n- 基于性别、年龄、民族、宗教、残疾、婚姻状况、性取向、外貌等的歧视性言语或行为；\r\n- 强制性、暗示性、重复性地提出不受欢迎的言语或身体接触；\r\n- 在办公环境中传播性暗示内容、低俗玩笑或带有攻击性的表情包；\r\n- 公开羞辱、孤立某一员工或当众贬损其能力；\r\n- 上下级间存在不对等权力关系下的恋爱、打压或利益交换行为。\r\n\r\n#### 三、投诉与处理机制\r\n\r\n公司为所有员工提供多渠道、匿名、保密的投诉路径，包括：\r\n\r\n- 人力资源邮箱与举报表单；\r\n- 道德热线（支持中文、英文双语）；\r\n- 第三方中立仲裁机构专属邮箱（处理特殊申诉）。\r\n\r\n一旦接到投诉，公司将立即启动独立调查程序，通常包括：\r\n\r\n1. 事实调查（证据收集与当事人访谈）；\r\n2. 问题定性与政策匹配；\r\n3. 高管或HR部门讨论与定责；\r\n4. 通知处理结果与跟踪后续反馈。\r\n\r\n对加害方的处理可包括：正式警告、培训重修、岗位调整、停职处理或立即解雇。对举报人，公司承诺绝不进行任何形式的报复，若遭遇“打击报复”，欢迎再次上报，公司将加重处理。\r\n\r\n### 2.5 冲突解决\r\n\r\n#### 一、冲突的定义与常见类型\r\n\r\n组织内部存在不同意见与多样背景，是健康发展的表现。然而，当沟通不畅、情绪激化或目标冲突失控时，冲突可能演变为生产力的阻碍。公司鼓励以建设性方式处理冲突，防止其对工作环境造成负面影响。\r\n\r\n常见冲突情形包括：\r\n\r\n- 团队成员间因目标、分工、进度差异引发的合作摩擦；\r\n- 管理者与下属因沟通风格、绩效评估等产生误解；\r\n- 跨部门因资源分配、优先级判断等发生冲突；\r\n- 文化差异、沟通偏好或历史积怨导致的潜在对立。\r\n\r\n#### 二、解决机制与流程\r\n\r\n公司提供三个层级的冲突解决机制：\r\n\r\n1. **个人协商机制**：当冲突较小且涉及当事人有限时，员工应优先主动沟通，面对面交流观点，澄清误解，达成共识。\r\n2. **主管协调机制**：当个人协商失败或影响工作节奏时，员工可书面报告直接主管，请求第三方介入协调。主管应在5个工作日内响应，召集协调会议，记录处理结果。\r\n3. **人力资源调解机制**：对于涉及跨部门、结构性问题或可能涉及政策违规的冲突，HR部门将指派调解专员，采用结构化谈话、共识会议或书面协议等方式达成解决方案。\r\n\r\n必要时，公司也提供外部职业调解师或劳动仲裁机构建议。\r\n\r\n#### 三、预防与文化建设\r\n\r\n为了从源头减少冲突，公司倡导“开放文化”与“反馈意识”，通过以下举措提升组织心理安全感：\r\n\r\n- 新员工培训中加入“职场沟通与团队协作”模块；\r\n- 每月一次部门“情绪分享会”；\r\n- 引入“员工满意度调查”和“360反馈”制度；\r\n- 设立“员工援助计划（EAP）”，为有心理压力的员工提供免费心理咨询支持。\r\n\r\n## 第三章：工作时间与出勤\r\n\r\n规范的工作时间安排和出勤制度，是确保企业高效运营、员工健康发展和组织公平管理的关键基础。创新科技股份有限公司本着“以人为本、效率优先、灵活协同”的理念，在全面遵守国家劳动法规的前提下，制定了兼顾员工工作效率与生活质量的出勤管理体系。本章将系统阐述公司的标准工作时间制度、出勤要求、考勤管理方式、假期安排及相关的管理程序，以保障员工权利、维护组织纪律，并营造积极、高效、规范的工作氛围。\r\n\r\n### 3.1 工作时间\r\n\r\n#### 一、标准工时制度\r\n\r\n公司执行国家规定的**标准工时制**，每周工作五天，每天工作时间为**8小时**，即**上午9:00至下午6:00**，中午12:00至13:00为午休时间。每周共计工作时间为40小时，法定节假日除外。\r\n\r\n在标准工作时间内，员工应遵守作息安排，按时开始工作、参与会议、提交日报等，确保业务有序推进与跨部门协同效率。\r\n\r\n#### 二、弹性工作机制\r\n\r\n为适应多样化岗位特征、提升员工满意度，公司设立**弹性出勤政策**，适用于研发、产品、设计、市场等强调创造力与成果导向的岗位。弹性机制分为以下几类：\r\n\r\n1. **弹性上下班制度**：员工可在8:30至9:30之间灵活选择到岗时间，确保完成每日8小时工作任务。下班时间依照到岗时间顺延，但必须保证团队沟通顺畅，不影响集体安排。\r\n2. **远程办公机制**：符合条件的岗位员工可申请每周不超过**3天的远程办公**。远程期间需保持在线沟通状态，使用公司指定系统记录每日任务进展，并接受主管考核。\r\n3. **核心工作时间制度**：部分业务部门如运营中心、客服中心设有核心在岗时间（10:00-16:00），要求员工必须在此时段保持在线/在场，以保障服务不中断。\r\n\r\n弹性安排须经所属主管书面批准并在人力资源系统备案，部门需定期评估弹性机制对绩效、沟通与团队协作的影响。\r\n\r\n#### 三、成果导向工作制度（ROWE）\r\n\r\n公司对部分岗位（如中高级研发、战略顾问、技术专家）试点推行“以成果为导向的工作制度（Results-Only Work Environment，ROWE）”，即对工作时长不做强制约束，而以成果产出、任务完成质量与时间为绩效依据。\r\n\r\n实施ROWE制度需满足以下条件：\r\n\r\n- 任职时间不少于一年；\r\n- 过往绩效为“良好”以上；\r\n- 能独立承担项目任务，具备自我管理能力；\r\n- 已签署《ROWE工作协议》，明确目标设定与交付周期。\r\n\r\n人力资源部将与用人部门联合进行年度评估，并在试点成功基础上逐步推广。\r\n\r\n#### 四、加班管理与补偿\r\n\r\n在特殊业务需求或项目冲刺期间，确需员工延时工作的，需严格按照《加班管理制度》执行：\r\n\r\n- 所有加班必须提前由主管审批；\r\n- 工作日加班按基本工资的**1.5倍**补偿；\r\n- 周末加班按**2倍工资**补偿，或以调休形式折算；\r\n- 法定节假日加班按**3倍工资**支付，原则上不安排；\r\n- 年度个人加班时间不得超过**200小时**，超过部分须由部门总监审批并报备HR。\r\n\r\n公司鼓励在合理范围内通过任务管理、流程优化减少加班需求，防止员工长期过劳。\r\n\r\n### 3.2 出勤要求\r\n\r\n#### 一、考勤管理机制\r\n\r\n公司采用**电子打卡系统**（支持App打卡、人脸识别、门禁联动等方式），用于记录每日出勤数据。员工应在规定时间段内完成打卡，如遇设备故障或特殊情况，可在当日通过HR系统提交“考勤异常申诉”，由主管审核。\r\n\r\n出勤状态分类如下：\r\n\r\n| 状态      | 定义                       | 是否计为出勤       |\r\n| --------- | -------------------------- | ------------------ |\r\n| 正常出勤  | 员工按时上下班，无迟到早退 | 是                 |\r\n| 迟到      | 上班打卡时间超过9:10       | 是（记录警告）     |\r\n| 早退      | 下班前30分钟内打卡离岗     | 是（记录警告）     |\r\n| 旷工      | 未请假且整天未打卡         | 否                 |\r\n| 公出      | 经批准前往外部工作任务     | 是                 |\r\n| 事假/病假 | 经批准请假                 | 否（但不计入旷工） |\r\n\r\n#### 二、迟到与早退管理\r\n\r\n- 每月累计**3次迟到或早退**，将由主管进行面谈；\r\n- 每季度累计**5次以上**，将记入绩效记录；\r\n- 连续3天未打卡或无故缺勤，将被视为**严重违纪**，启动正式调查程序；\r\n- 特殊情形（如突发灾害、公共交通中断等）可提交说明申请免除记录。\r\n\r\n#### 三、请假制度\r\n\r\n公司鼓励员工合理安排个人事务，确保工作与生活平衡。员工因私事或健康问题无法出勤时，必须遵守请假流程：\r\n\r\n- **事假**：需提前24小时提交申请，说明原因与预计时长；\r\n- **病假**：当天提交，3天以上病假须附正规医院开具的病假单；\r\n- **突发情况**：如因家属紧急就医、交通事故等特殊情况无法提前申报，员工应尽早向主管报告，由其进行记录与补报；\r\n- 所有请假记录须在HR系统登记备案，并经直接上级批准。\r\n\r\n如无正当理由多次请假、延迟返岗或虚假请假，将依据公司纪律制度进行处理。\r\n\r\n#### 四、远程工作要求\r\n\r\n远程办公期间，员工应遵守以下规范：\r\n\r\n- 每日早上9:30前通过指定平台签到；\r\n- 使用远程日报系统提交工作计划与实际成果；\r\n- 保持电话、即时通讯、视频会议工具畅通；\r\n- 未经许可不得在远程期间从事与公司无关活动；\r\n- 人力资源部与信息技术部将定期审计远程工作日志。\r\n\r\n### 3.3 假期与休假\r\n\r\n公司严格按照国家法律法规执行各类假期制度，并提供多种带薪假期和额外福利假期，保障员工身心健康与家庭照顾权利。\r\n\r\n#### 一、法定节假日\r\n\r\n员工享有以下**法定带薪节假日**（以当年国家人力资源与社会保障部公告为准）：\r\n\r\n- 元旦：1天；\r\n- 春节：7天；\r\n- 清明节：1天；\r\n- 劳动节：3天；\r\n- 端午节：1天；\r\n- 中秋节：1天；\r\n- 国庆节：3天。\r\n\r\n节假日前后如遇调休，HR部门将发布出勤调整安排。\r\n\r\n#### 二、带薪年假\r\n\r\n员工在公司服务满一周年后，享有带薪年假，计算方式如下：\r\n\r\n| 工龄      | 年假天数 |\r\n| --------- | -------- |\r\n| 1 - 10年  | 10天     |\r\n| 11 - 20年 | 15天     |\r\n| 20年以上  | 20天     |\r\n\r\n年假须提前申请、按月累计，未使用部分可顺延至次年3月31日前使用，逾期未休视为自动放弃，但可申请转为现金补偿。\r\n\r\n公司鼓励员工**合理规划年假使用**，避免年底集中申请，保障业务连续性。\r\n\r\n#### 三、其他带薪假期\r\n\r\n| 假种类 | 时长      | 说明                           |\r\n| ------ | --------- | ------------------------------ |\r\n| 病假   | 最长6个月 | 按工龄计算带薪比例             |\r\n| 产假   | 158天     | 按国家规定执行，含产前假       |\r\n| 陪产假 | 15天      | 必须在配偶生产前后一个月内使用 |\r\n| 婚假   | 10天      | 提供结婚证复印件               |\r\n| 丧假   | 3天       | 限一等亲属，视情况延长         |\r\n\r\n#### 四、特别假期与福利假\r\n\r\n为提升员工满意度，公司增设以下弹性假期：\r\n\r\n- **家庭照护假**：每年最多5天，用于照顾生病子女、父母；\r\n- **心理健康日**：每半年可自选1天，事先申请，不需说明；\r\n- **奖励假**：连续两次绩效评级“优秀”的员工可额外获得3天带薪奖励假。\r\n\r\n### 3.4 假期申请流程与注意事项\r\n\r\n员工申请各类假期，应遵循以下标准流程：\r\n\r\n1. 登录企业HR系统，选择假种类、起止时间、请假原因；\r\n2. 提交后系统自动流转至直属主管审核；\r\n3. 审批通过后人力资源系统更新考勤状态；\r\n4. 返回岗位后如有实际出勤差异，员工须及时补录调整；\r\n5. 请假期间必须确保工作交接、设置代办事项、合理安排邮箱自动回复。\r\n\r\n若在假期期间提前结束或延长休假，员工须尽快提交变更申请。无故未返岗或擅自延长假期的行为，将被视为旷工处理。\r\n\r\n## 第四章：薪酬与福利\r\n\r\n薪酬与福利是员工价值认可和企业文化的重要体现。科学合理、透明公正的薪酬福利制度，不仅能够激励员工持续贡献、提升企业凝聚力与忠诚度，也体现了公司对员工权益的高度尊重。创新科技股份有限公司始终坚持“以能力为基础、以绩效为导向、以市场为参考、以公平为原则”的薪酬管理理念，构建了覆盖员工职业发展全过程的薪酬福利体系，致力于营造富有激励性、保障性与人文关怀的雇主品牌形象。\r\n\r\n本章将详尽说明公司在薪酬结构、年度调薪、福利计划、报销制度、奖金激励和员工支持项目等方面的具体政策，确保每一位员工都能清晰了解自己的权益和义务。\r\n\r\n### 4.1 薪酬结构\r\n\r\n#### 一、薪酬构成概览\r\n\r\n公司为员工提供具备竞争力的**“3+1”薪酬结构**，即：\r\n\r\n1. **固定薪资（基本工资）**：按月发放，是员工薪酬的基本保障，占总薪酬结构约70%；\r\n2. **绩效奖金**：依据个人KPI完成情况、部门目标及公司整体经营成果核定；\r\n3. **津贴补贴**：包括交通补贴、餐补、通讯补贴等日常工作支持性费用；\r\n4. **长期激励计划**：包括股权激励、期权计划、专项项目奖金等，用于激励核心骨干。\r\n\r\n该结构兼顾员工稳定收入、绩效导向和长期发展，体现“即时激励+中长期激励”相结合的薪酬战略。\r\n\r\n#### 二、薪资档级与岗位体系\r\n\r\n公司建立了清晰的**岗位等级体系**，划分为六大职级：助理级、专员级、高级专员/主管、经理级、高级经理/总监、副总裁及以上，每一级内设多个薪资档位，依据员工能力、绩效、年限及市场行情进行动态调整。\r\n\r\n各职级设有“横向通道”和“纵向通道”：\r\n\r\n- **横向通道**：允许员工在技术、管理、项目、产品等多个发展路径中进行转换；\r\n- **纵向通道**：强调从执行到规划、从操作到决策的晋升成长路径。\r\n\r\n员工每年可在绩效评估后参与晋级评审或调薪申请，由所属部门与人力资源联合审核，确保公开透明。\r\n\r\n#### 三、薪资发放与保密\r\n\r\n- 公司实行**月度发薪制**，每月15日前发放上月工资；\r\n- 所有薪资发放通过银行代发，员工可在HR系统中查询工资条明细；\r\n- 公司严格遵守《个人所得税法》，依法代扣代缴；\r\n- 所有员工薪资信息属于公司机密，未经允许不得泄露、交换、比较。\r\n\r\n### 4.2 年度调薪与特别调整\r\n\r\n#### 一、年度调薪政策\r\n\r\n公司每年进行一次统一薪资审查，通常安排在**12月-次年2月**之间实施。调薪评估依据以下因素：\r\n\r\n1. 个人绩效评级（权重最大）；\r\n2. 岗位晋升或职能变动；\r\n3. 部门或公司年度经营状况；\r\n4. 市场薪酬水平动态；\r\n5. 所属部门薪资结构均衡性。\r\n\r\n绩效评级为“优秀”或“超越预期”的员工，将优先获得调薪机会。调薪方案须经人力资源部、部门负责人及高管批准后执行。\r\n\r\n#### 二、特别调薪机制\r\n\r\n在以下情况下，员工可申请“特别调薪”或由主管主动发起：\r\n\r\n- 接受更高责任的岗位调动；\r\n- 关键人才挽留或外部高薪报价对比；\r\n- 重大项目成功交付；\r\n- 所在城市生活成本显著提升；\r\n- 市场行情波动较大岗位（如AI算法工程师、云平台架构师等）。\r\n\r\n特别调薪流程需提交说明材料、目标对比数据及推荐意见，由人力资源部评估，并呈报审批。\r\n\r\n### 4.3 奖金与激励计划\r\n\r\n#### 一、绩效奖金\r\n\r\n绩效奖金基于员工个人KPI、OKR以及公司整体业绩发放，标准如下：\r\n\r\n- 高绩效员工（“优秀”等级）奖金占基本工资**25%~30%**；\r\n- 达标员工（“良好”等级）奖金为基本工资**10%~15%**；\r\n- 表现一般或“待改进”者可能无绩效奖金。\r\n\r\n绩效奖金按**季度或年度**发放，部门经理需组织绩效回顾会议，向员工反馈评分依据和提升建议。\r\n\r\n#### 二、项目奖金\r\n\r\n为鼓励员工参与创新项目和战略重点任务，公司设立专项项目奖金池，典型适用情形如下：\r\n\r\n- 参与国家重点研发计划或企业级重要交付；\r\n- 完成跨部门高复杂度项目；\r\n- 创造显著业务效益或节省大量成本。\r\n\r\n项目负责人可提交申请，经项目办公室评估并由管理层核定发放金额。\r\n\r\n#### 三、股权激励与期权计划\r\n\r\n核心骨干员工可参与“合伙人激励计划”，包括：\r\n\r\n- **员工持股计划（ESOP）**；\r\n- **虚拟股权激励（VSOP）**；\r\n- **限制性股票或期权（RSU/ESO）**。\r\n\r\n公司根据年度经营成果与个人贡献动态调整激励池额度，具体方案在年度内另行通知，参与人员需签署相关协议。\r\n\r\n### 4.4 津贴补贴政策\r\n\r\n为减轻员工日常工作成本压力，公司设有一系列固定津贴，包含：\r\n\r\n- **交通补贴**：按城市划分，每月固定发放（北京600元、二线城市500元）；\r\n- **餐饮补贴**：每日20元标准，按出勤天数月结；\r\n- **通讯补贴**：全员统一每月100元，经理级及以上200元；\r\n- **高温补贴**：6-9月期间户外岗位按法规发放；\r\n- **节日补贴**：春节、端午、中秋发放节日礼包或等额购物卡。\r\n\r\n各项补贴在工资单中列明，员工可在HR系统中查看补贴记录。\r\n\r\n### 4.5 福利保障计划\r\n\r\n#### 一、社会保险与住房公积金\r\n\r\n公司依法为员工缴纳“五险一金”，即：\r\n\r\n- 养老保险\r\n- 医疗保险\r\n- 失业保险\r\n- 工伤保险\r\n- 生育保险\r\n- 住房公积金（按员工基数缴纳12%）\r\n\r\n公司严格按照国家及属地政策标准执行，并每年协助员工更新社保基数信息。\r\n\r\n#### 二、补充医疗保险\r\n\r\n公司为全员购买补充商业医疗险，涵盖：\r\n\r\n- 门急诊报销（含普通病、慢性病）；\r\n- 住院费用（含手术、药品）；\r\n- 特殊疾病津贴；\r\n- 年度报销上限10万元。\r\n\r\n员工入职30日内自动生效，可添加配偶和子女为附加受益人（自费）。\r\n\r\n#### 三、健康支持项目\r\n\r\n- **年度免费体检**（含癌症筛查）；\r\n- **健身房补贴**：每人每年最高2000元，提供合作健身品牌；\r\n- **心理健康服务**：EAP计划，每年6次免费心理咨询；\r\n- **营养指导与健康讲座**：每季度一次，公司专家主讲。\r\n\r\n#### 四、教育发展援助\r\n\r\n- 报销每年5000元以内的职业证书考试费或技能培训；\r\n- 与Coursera、MOOC、极客时间等平台合作，开放课程库；\r\n- 对外部专业会议、技术大会参会人员报销差旅与门票费用；\r\n- 提供内部轮岗与管理培训机会。\r\n\r\n#### 五、家庭支持政策\r\n\r\n- 儿童教育补助（小学至高中）：每年最高2000元/子女；\r\n- 弹性工作支持：育儿期员工可申请调整上班时间；\r\n- 孕期员工可享独立母婴室、专属休息区；\r\n- 配偶重大疾病员工可申请专项护理假。\r\n\r\n### 4.6 报销政策\r\n\r\n#### 一、适用范围\r\n\r\n公司报销制度适用于以下情况：\r\n\r\n- 商务差旅费用（含交通、住宿、交通补贴）；\r\n- 项目材料采购；\r\n- 外出用餐、客户招待；\r\n- 培训课程及考试费用；\r\n- 合规办公耗材采购。\r\n\r\n#### 二、报销流程\r\n\r\n1. 员工在ERP系统提交报销申请；\r\n2. 上传有效票据与发票（纸质、电子均可）；\r\n3. 主管审核签字；\r\n4. 财务部核对并付款（5-7个工作日到账）；\r\n5. 超预算或单笔超过3000元须由部门负责人额外审批。\r\n\r\n#### 三、限制性条款\r\n\r\n- 报销项目须在**60日内申请**，逾期不予处理；\r\n- 不得报销个人消费、家属陪同费用、发票不符项目；\r\n- 虚假报销将视为严重违纪行为处理；\r\n- 公司保留对报销记录审计的权利。\r\n\r\n## 第五章：绩效评估\r\n\r\n绩效评估是企业人力资源管理的核心环节之一。它不仅是对员工过去一段时间工作成果的回顾总结，更是人才发展、组织优化与文化落地的重要工具。创新科技股份有限公司秉持“科学、公正、协同、发展”的绩效理念，构建了涵盖目标设定、过程跟踪、阶段回顾、评估反馈和改进提升在内的闭环绩效管理体系，确保员工个人发展与公司战略目标之间实现高度对齐。\r\n\r\n本章将详细介绍公司绩效评估的组织架构、实施流程、评估标准、应用机制、发展工具和申诉渠道等内容，帮助员工充分理解绩效评估的意义、方法与影响，建立积极、开放、正向的绩效文化。\r\n\r\n### 5.1 绩效评估体系概述\r\n\r\n#### 一、绩效理念\r\n\r\n公司的绩效管理核心理念包括：\r\n\r\n- **目标导向**：绩效应聚焦“做正确的事”，强调目标设定与结果落地。\r\n- **过程可见**：通过OKR、周报、季度回顾等机制，保障过程透明。\r\n- **发展驱动**：评估不仅是结果判断，更是成长导向与能力发展的工具。\r\n- **全员参与**：绩效管理不仅是HR的职责，更是每一位员工和管理者的责任。\r\n- **公平公正**：评估过程遵循标准化、可追溯、基于事实的数据评判原则。\r\n\r\n公司反对“唯结果论”“唯关系论”“形式主义”等偏差做法，鼓励以真实数据、实际贡献为评估依据，确保员工在公平环境下获得反馈与激励。\r\n\r\n#### 二、适用对象\r\n\r\n公司所有正式员工，包括总部、各地分公司与海外办事处员工，均纳入统一绩效评估管理范畴。试用期员工参与季度试点评估，不参与年度评级。实习生与项目外包人员根据实际项目考核方式处理。\r\n\r\n中高级管理人员（经理及以上）需承担下属绩效辅导责任，其绩效评级将部分受其团队整体绩效表现影响。\r\n\r\n### 5.2 绩效评估流程\r\n\r\n公司采用“半年度+年度”双周期绩效评估机制，评估流程共分为五个阶段：\r\n\r\n#### 一、目标设定（Performance Planning）\r\n\r\n- 时间：每年1月与7月\r\n- 工具：公司统一OKR系统或绩效管理平台（如SuccessFactors、Moka）\r\n- 要求：\r\n  - 所有员工需与直属上级在评估周期开始前两周内共同确定绩效目标；\r\n  - 目标需具备SMART原则（具体、可衡量、可达成、相关性强、时限明确）；\r\n  - 建议设定3~5个主要目标，并明确权重与量化指标。\r\n\r\n其中包括：\r\n\r\n| 目标类型       | 示例                                     |\r\n| -------------- | ---------------------------------------- |\r\n| 业务成果类目标 | 完成A项目上线、实现销售额增长20%         |\r\n| 管理类目标     | 建立团队协作流程、优化组织架构           |\r\n| 个人成长目标   | 完成某门技术课程并取得证书、提高演讲能力 |\r\n\r\n目标需在HR平台提交，并经两级主管确认后生效。\r\n\r\n#### 二、过程跟踪（Performance Monitoring）\r\n\r\n- 时间：全过程\r\n- 工具：双周/月度OKR打卡、项目日报、复盘会议\r\n- 要求：\r\n  - 员工需定期记录工作进展，更新目标完成状态；\r\n  - 直属上级需每月至少一次与员工进行反馈对话；\r\n  - 对于目标偏离或进度滞后，应及时调整策略或修订目标。\r\n\r\n公司提倡“即时反馈”文化，鼓励主管与员工之间建立持续反馈机制，避免“只看期末评分”的单一模式。\r\n\r\n#### 三、中期回顾（Mid-term Review）\r\n\r\n- 时间：每年6月、12月初\r\n- 工具：系统自动提醒\r\n- 要求：\r\n  - 上下级共同审视目标进展；\r\n  - 对进展偏离部分说明原因，并讨论改进计划；\r\n  - 必要时调整目标内容或权重；\r\n  - 回顾会议需形成书面纪要并上传系统。\r\n\r\n中期回顾不计入正式评分，但对年终评估具有参考价值。\r\n\r\n#### 四、年度评估（Performance Appraisal）\r\n\r\n- 时间：每年12月~次年1月\r\n- 步骤：\r\n  1. **自评（Self-Evaluation）**：员工根据目标完成情况、自我反思与收获进行自评撰写；\r\n  2. **主管评估（Manager Review）**：直属上级基于事实数据与观察打分并提供书面反馈；\r\n  3. **360度反馈（可选）**：对关键岗位实施多方反馈机制，涵盖同事、下属与跨部门协作对象；\r\n  4. **校准会议（Calibration Meeting）**：部门内统一标准、调整偏差；\r\n  5. **绩效沟通（Feedback Meeting）**：主管与员工一对一进行正式沟通，明确结果与建议；\r\n  6. **结果录入系统并归档**。\r\n\r\n### 5.3 绩效评估标准\r\n\r\n#### 一、评估维度\r\n\r\n公司采用多维度绩效评分机制，共包括以下四项维度，赋予权重如下：\r\n\r\n| 维度                 | 权重 | 描述                               |\r\n| -------------------- | ---- | ---------------------------------- |\r\n| 目标完成情况         | 50%  | 是否完成原定业务指标和项目交付成果 |\r\n| 专业能力发展         | 20%  | 是否掌握新技能、知识学习、能力提升 |\r\n| 团队合作与沟通       | 20%  | 协作意识、跨部门配合、信息透明     |\r\n| 企业文化与价值观体现 | 10%  | 是否践行公司核心价值观与行为准则   |\r\n\r\n#### 二、评分等级定义\r\n\r\n评估结果分为以下五档，分别对应不同管理动作：\r\n\r\n| 等级                            | 描述                               | 后续举措                         |\r\n| ------------------------------- | ---------------------------------- | -------------------------------- |\r\n| A - 卓越（Top Performer）       | 超出预期，创造重大价值，具有引领性 | 晋升、加薪、激励奖金、项目主导权 |\r\n| B - 良好（Strong Performer）    | 达成目标，有持续提升，表现稳定     | 推荐培养、技能培训、奖金         |\r\n| C - 合格（Meets Expectations）  | 完成核心任务，表现中规中矩         | 保持岗位，参与轮岗               |\r\n| D - 待改进（Needs Improvement） | 存在多项未完成任务或行为问题       | 纳入PIP计划，限期改进            |\r\n| E - 不合格（Unsatisfactory）    | 严重绩效失职或不当行为             | 解除劳动合同或调整岗位           |\r\n\r\n公司实施强制分布比例建议（非强制），防止评估“虚高”失真现象。例如，A等级人数不超过10%，B等级控制在30%~40%。\r\n\r\n### 5.4 绩效评估结果应用\r\n\r\n绩效评估不仅决定短期激励，更影响员工长期发展。评估结果将用于以下人力资源决策：\r\n\r\n#### 一、薪酬调整\r\n\r\n- 年度调薪方案以绩效评级为主要参考；\r\n- 绩效等级为“优秀”者将获得高于平均水平的涨薪；\r\n- 连续两年“待改进”或“不合格”者将暂停调薪资格。\r\n\r\n#### 二、奖金发放\r\n\r\n- 年终奖依据绩效等级分档发放，具体比例见第四章薪酬部分；\r\n- 项目奖金分配亦需参考相关人员绩效等级，强化“多劳多得”机制。\r\n\r\n#### 三、晋升与职级调整\r\n\r\n- 所有岗位晋升、职级评估必须满足至少一年“良好”以上绩效记录；\r\n- 连续两次“卓越”评级者可跳级申报晋升或参与管理培训项目。\r\n\r\n#### 四、内部流动与轮岗\r\n\r\n- 绩效评估为横向流动或跨部门推荐提供重要依据；\r\n- 表现突出者优先考虑参与高潜力项目或岗位试用。\r\n\r\n#### 五、绩效改进计划（PIP）\r\n\r\n针对连续绩效评级低于“合格”的员工，公司启动“绩效改进计划（Performance Improvement Plan）”：\r\n\r\n- 设定3个月改善周期；\r\n- 明确具体改进目标；\r\n- 提供配套培训、资源与主管指导；\r\n- 周度进展回顾；\r\n- 计划期末进行复评，未达预期者启动岗位调整或终止合同。\r\n\r\n### 5.5 培训与职业发展支持\r\n\r\n绩效不仅评估过去，更着眼未来成长。公司提供如下职业发展支持：\r\n\r\n#### 一、培训资源\r\n\r\n- 内训课：由公司内训师或管理者开设业务课程、通用技能班；\r\n- 外部课：合作高校、平台（如Coursera、极客时间）提供；\r\n- 技术认证：鼓励考取PMP、CFA、Cisco、AWS等国际认证；\r\n- 领导力计划：经理级及以上员工每年参加领导力工作坊。\r\n\r\n#### 二、职业发展路径\r\n\r\n公司建立清晰的双通道发展机制：\r\n\r\n| 通道     | 说明                           |\r\n| -------- | ------------------------------ |\r\n| 专业通道 | 高级工程师→技术专家→首席科学家 |\r\n| 管理通道 | 高级专员→主管→经理→总监        |\r\n\r\n每位员工均可通过绩效回顾提出“职业发展意愿”，HRBP将定期组织职业访谈与发展辅导。\r\n\r\n#### 三、导师机制\r\n\r\n新员工及关键岗位人员可申请“内部导师”配对制度，获得经验传承、职业导航与心理支持，促进长期职业发展。\r\n\r\n### 5.6 反馈与申诉机制\r\n\r\n为确保绩效评估公正透明，公司建立完善的反馈与申诉机制：\r\n\r\n#### 一、反馈会谈制度\r\n\r\n- 每次绩效评估结束后，主管需与员工面对面开展绩效反馈会；\r\n- 员工可就目标设定、评分依据、成长建议提出看法；\r\n- 所有反馈过程应形成纪要文档并存档。\r\n\r\n#### 二、绩效申诉机制\r\n\r\n若员工对评估结果有异议，可在评估结果公示后7个工作日内提出书面申诉，流程如下：\r\n\r\n1. 员工提交《绩效申诉表》至HR；\r\n2. HR组织二级主管或独立评审小组复审；\r\n3. 如需，安排三方复议会谈；\r\n4. 最终处理结果由公司绩效委员会裁定并反馈。\r\n\r\n公司承诺申诉过程公开透明、全程保密，不因申诉行为对员工进行打压或负面评价。\r\n\r\n## 第六章：纪律与惩戒\r\n\r\n企业的持续健康发展，离不开科学的制度保障与严格的行为规范。纪律不仅是组织运转的基础，也是员工之间实现公平、互信与效率的前提。为保障公司的运营秩序、维护公司利益和良好的工作氛围，创新科技股份有限公司建立了明确的纪律规范与惩戒制度，旨在引导员工自觉遵守行为准则，明确“可为”与“不可为”的边界，及时发现与处理不当行为，最大限度地预防风险并推动组织持续改善。\r\n\r\n本章对员工在公司工作期间必须遵守的基本纪律、违纪行为的分类、惩戒措施的种类与适用标准、调查处理流程、申诉与复议机制等方面进行全面说明，并强调在处理任何员工违纪行为时，公司都将遵循合法、合规、公正、透明的原则。\r\n\r\n### 6.1 违纪行为定义与分类\r\n\r\n#### 一、违纪行为定义\r\n\r\n员工违纪是指员工在公司工作期间，违反公司规章制度、行为准则、岗位职责、国家法律法规、职业道德，或因疏忽、过失或主观故意行为而造成公司形象、业务、财产或其他员工权益受到不良影响的行为。无论违纪行为发生在公司场所或外部，只要与工作相关或影响公司利益，均可作为处理依据。\r\n\r\n#### 二、违纪行为分类\r\n\r\n公司依据违纪行为的性质、情节和后果，将其划分为**轻微违纪、中等违纪和严重违纪**三类，具体分类如下：\r\n\r\n**（1）轻微违纪行为：**\r\n\r\n该类行为通常为无主观恶意、未造成严重后果的行为，但若多次发生或整改不力，将升级为中等违纪处理。\r\n\r\n| 示例行为                                             |\r\n| ---------------------------------------------------- |\r\n| 上班迟到、早退，未履行请假手续擅自离岗               |\r\n| 着装不符合公司规范（如未穿工牌、未遵守商务着装要求） |\r\n| 上班时间使用手机、社交软件处理私人事务               |\r\n| 未经许可私自修改办公软件配置                         |\r\n| 忽略公司会议通知，无正当理由缺席部门例会或全员会议   |\r\n| 未按流程提交日报、周报、项目总结等文档               |\r\n\r\n**（2）中等违纪行为：**\r\n\r\n此类行为具有一定主观性，已对工作秩序、团队协作或客户形象造成负面影响，或已触犯公司核心流程制度。\r\n\r\n| 示例行为                                               |\r\n| ------------------------------------------------------ |\r\n| 多次无正当理由迟到、早退或未打卡，累计达警戒线         |\r\n| 虚构工作成果、隐瞒项目延误、篡改数据报告               |\r\n| 未经授权私自外出、代表公司参加外部活动                 |\r\n| 滥用公司资源（如打印机、车辆、办公耗材等）用于个人目的 |\r\n| 上班期间睡觉、打游戏、购物等与工作无关行为             |\r\n| 在公共平台发表不当言论损害公司声誉                     |\r\n| 不服从上级合理工作安排、公开顶撞管理人员               |\r\n| 对同事有语言冒犯、冷嘲热讽、消极抵抗协作任务           |\r\n\r\n**（3）严重违纪行为：**\r\n\r\n严重违纪通常包括有意违背职业道德、违反法律、触犯公司“红线”行为，情节严重者将被立即解聘并可能追究法律责任。\r\n\r\n| 示例行为                                 |\r\n| ---------------------------------------- |\r\n| 伪造、篡改报销单据，骗取公司资金         |\r\n| 泄露公司机密信息、客户数据、项目方案     |\r\n| 收受贿赂或向第三方提供公司资源谋取私利   |\r\n| 严重歧视、性骚扰、暴力行为或职场霸凌     |\r\n| 盗窃公司或他人财物                       |\r\n| 在工作场所使用、持有、传播毒品或违禁品   |\r\n| 酒后进入办公室或在上班期间饮酒影响工作   |\r\n| 故意破坏公司系统、服务器或数据平台       |\r\n| 恶意传播谣言，挑拨离间，严重影响组织氛围 |\r\n| 利用职位谋取私人投资利益或竞业行为       |\r\n\r\n### 6.2 惩戒措施与适用规则\r\n\r\n公司在处理员工违纪行为时，始终坚持“以事实为依据、以制度为准绳”的原则，综合考虑违纪的性质、次数、后果、主观态度及员工过往记录，依法依规实施惩戒。\r\n\r\n#### 一、惩戒等级\r\n\r\n公司设立以下六级惩戒措施，按违纪情节依次加重：\r\n\r\n1. **口头警告**：由主管对员工进行非正式谈话，指出问题，提醒整改；\r\n2. **书面警告**：形成正式书面文书存档，列入员工行为记录；\r\n3. **绩效影响**：将违纪纳入年度考评，影响绩效评级与奖金；\r\n4. **停发奖金或津贴**：根据违纪行为直接扣除当期绩效奖金或专项津贴；\r\n5. **岗位调整或降级**：取消管理权限，调离原岗位或降低职级；\r\n6. **解除劳动合同**：对严重违纪者，依法解除劳动合同，不支付经济补偿。\r\n\r\n严重违纪者，公司保留追究经济赔偿、向公安机关报案及依法追责的权利。\r\n\r\n#### 二、惩戒适用细则\r\n\r\n- 同一违纪行为连续出现超过三次者，自动升级处理级别；\r\n- 多项轻微违纪合并视为中等违纪；\r\n- 若违纪行为造成公司客户损失、声誉风险、法律纠纷，将按严重违纪处理；\r\n- 涉及领导干部、管理人员违纪将适用更高惩戒标准，并加重处理；\r\n- 员工若在调查过程中存在掩盖、推诿、串供行为，将视为抗拒调查并加重惩罚。\r\n\r\n### 6.3 惩戒流程与执行程序\r\n\r\n为了确保纪律执行的公正与程序正当，公司设有统一的**纪律处理流程**，由人力资源部联合用人部门执行。\r\n\r\n#### 一、流程步骤\r\n\r\n1. **事件报告**\r\n   任何员工、主管或第三方发现疑似违纪行为，均可通过邮件、HR系统或匿名渠道向人力资源部或监察办公室报告。\r\n2. **初步核实**\r\n   HR根据线索初步核实事件真实性，并决定是否立案调查。轻微事件由部门自处，中等及以上事件须升级处理。\r\n3. **正式调查**\r\n   HR牵头成立调查小组（可含第三方），对当事人、证人进行访谈，收集资料，确保取证合法、流程可追溯。\r\n4. **评估与处理建议**\r\n   调查结束后，HR撰写调查报告，列明事实、证据、涉事人陈述，并提出处理建议。\r\n5. **听证与申辩（适用于中等及以上）**\r\n   给予被调查人申辩权利，可陈述理由、提交补充材料，必要时举行听证会。\r\n6. **决定与执行**\r\n   由人力资源主管或总经理根据调查结果做出正式处理决定，并下发书面通知。视情节通报范围包括部门、全员或高管层。\r\n7. **文件归档**\r\n   所有处理结果、流程记录将存入员工人事档案，保留5年备查。\r\n\r\n#### 二、员工知情权与参与权\r\n\r\n- 所有涉及个人惩戒的程序，员工均有知情权、答辩权与申诉权；\r\n- 除非法律要求或严重扰乱秩序，公司承诺在调查未结束前不披露当事人姓名；\r\n- 公司严禁对举报人、证人、申诉人进行打击报复，一经发现将严惩。\r\n\r\n### 6.4 申诉与复议机制\r\n\r\n为保障员工合法权益，公司建立了完善的纪律处分申诉和复议机制。\r\n\r\n#### 一、申诉路径\r\n\r\n员工若对公司作出的惩戒决定不服，可在**收到决定之日起7个工作日内**，通过以下渠道提出申诉：\r\n\r\n- 向人力资源部书面提交《申诉申请表》；\r\n- 邮件提交至公司纪律与合规邮箱（[confidential@innovationtech.com](mailto:confidential@innovationtech.com)）；\r\n- 如遇高管或HR人员牵涉案件，可向“独立合规审查委员会”提出异议。\r\n\r\n#### 二、复议程序\r\n\r\n收到申诉后，公司将在5个工作日内启动复议流程：\r\n\r\n1. **成立复议小组**：成员包括HR代表、法务代表、独立第三方/工会代表（如有）；\r\n2. **审阅材料**：重新审查调查报告与员工申诉理由；\r\n3. **听取意见**：必要时组织复议会谈；\r\n4. **做出结论**：复议结论形成《申诉复审意见书》，由总经理或指定高管签署生效；\r\n5. **反馈与归档**：将复议结论反馈至员工，并归入人事系统。\r\n\r\n复议决定为最终处理结果，除非另有法律程序启动，否则不再接受二次申诉。\r\n\r\n### 6.5 纪律管理文化倡导\r\n\r\n公司认为，纪律制度不仅是惩罚机制，更应当发挥教育、预防、提醒、激励的功能。为此，公司从文化层面强化纪律意识，主要举措包括：\r\n\r\n#### 一、制度公开透明\r\n\r\n- 所有员工手册、制度文件均可在HR门户网站查阅；\r\n- 公司定期开展《员工行为规范》、《公司红线行为警示录》学习培训；\r\n- 每季度发布“合规案例通报”与“正面典型表彰”。\r\n\r\n#### 二、正向激励结合\r\n\r\n- 对于自我纠错、积极配合整改者，可视情节减轻处理；\r\n- 鼓励员工主动举报、发现风险，设立“合规建议激励奖”，每条最高1000元；\r\n- 表现优秀、履职尽责者优先推荐参加管理培训和晋升评定。\r\n\r\n#### 三、管理者职责明确\r\n\r\n- 各级管理者需承担下属行为教育与风险预警责任；\r\n- 未能及时制止下属违纪行为或存在“纵容”“包庇”情节者，将一并问责；\r\n- 管理者每年需接受一次“组织纪律责任”专项培训。\r\n\r\n## 第七章：健康与安全\r\n\r\n员工的健康与安全是企业可持续发展的基础。创新科技股份有限公司始终将员工的生命安全和身心健康放在首位，全面推行“以人为本、预防为先、全员参与、持续改进”的健康与安全管理方针，积极构建安全、健康、和谐的工作环境，为员工创造稳定、高效、可持续的工作生活空间。\r\n\r\n本章将详细阐述公司在职业安全、环境卫生、健康促进、紧急响应和安全文化建设方面的制度安排和执行规范，确保每位员工了解自己的权利、义务与可用资源。\r\n\r\n### 7.1 健康与安全管理体系\r\n\r\n#### 一、健康与安全的总体目标\r\n\r\n- 零重大安全事故；\r\n- 零职业病；\r\n- 员工年度健康体检参与率100%；\r\n- 应急响应机制响应时间≤3分钟；\r\n- 安全培训覆盖率100%；\r\n- 员工满意度≥90%的安全工作环境。\r\n\r\n#### 二、管理架构与责任分工\r\n\r\n公司设立专门的“健康与安全委员会”，由首席运营官（COO）牵头，成员包括人力资源部、行政部、设施管理部、IT部、安保服务商、各部门代表及员工志愿者代表。职责包括：\r\n\r\n- 制定与更新公司安全管理政策；\r\n- 组织年度安全检查、消防演练；\r\n- 审查重大健康和安全事件报告；\r\n- 制定员工健康支持项目计划；\r\n- 管理疫情响应、传染病控制和突发公共卫生事件流程；\r\n- 推广安全文化、员工心理健康、压力管理等项目。\r\n\r\n各部门设有专兼职“安全联络员”，负责本部门日常安全检查、安全培训传达、安全隐患收集与上报。\r\n\r\n### 7.2 工作场所安全政策\r\n\r\n#### 一、办公场所安全设施\r\n\r\n公司各办公场所均按照国家和地方安全法规配置相关安全设施：\r\n\r\n- 所有办公区配备足量灭火器、烟雾探测器、喷淋装置，并每季度检测；\r\n- 每个楼层设有明确疏散图、出口指示牌、应急照明；\r\n- 配备自动体外除颤仪（AED）、急救包、简易担架等急救工具；\r\n- 在核心服务器机房配置UPS、不间断供电系统、温湿度控制与安防系统；\r\n- 建有独立吸烟区，避免吸烟引发火灾或影响同事健康。\r\n\r\n所有新入职员工必须完成“职场安全通识培训”，涵盖逃生路径、火灾应对、触电急救、防滑防摔等基本常识。\r\n\r\n#### 二、安全检查与隐患排查\r\n\r\n公司执行“日常自检、月度巡检、季度专项”的三级隐患排查机制：\r\n\r\n- 各部门每周五组织安全自查，包括插座、电线、办公家具、通风口等；\r\n- 行政与设施管理部每月组织一次巡查，涵盖电气系统、厨房、走道等重点区域；\r\n- 公司每季度邀请第三方安全机构进行评估，并提交《健康与安全改进报告》。\r\n\r\n发现的隐患需在48小时内整改，重大隐患须立即停用相关设备并上报健康与安全委员会。\r\n\r\n#### 三、禁止性行为规范\r\n\r\n以下行为被视为严重违反安全规定，公司将予以严肃处理：\r\n\r\n- 擅自移动灭火器、应急出口设备；\r\n- 在服务器机房、配电间等区域吸烟；\r\n- 非IT人员未授权进入关键设备区域；\r\n- 擅自关闭烟感、消防喷淋设备；\r\n- 私拉乱接插线板、私用电热器等存在电气火灾风险的行为；\r\n- 拒不参与公司组织的应急疏散演练。\r\n\r\n### 7.3 员工健康支持计划\r\n\r\n#### 一、年度健康体检\r\n\r\n公司为所有正式员工提供年度免费体检，涵盖以下内容：\r\n\r\n- 基础项目：血常规、肝肾功能、心电图、X光；\r\n- 高风险筛查：癌症早筛、甲状腺、肿瘤标志物（根据年龄与岗位定制）；\r\n- 女性专项检查：乳腺彩超、妇科筛查（适龄员工）；\r\n- 中老年专项：骨密度、眼底照查、认知功能测试。\r\n\r\n员工体检报告由公司指定三甲医院出具，并可通过公司健康管理平台进行预约、查看、咨询。\r\n\r\n#### 二、心理健康支持（EAP）\r\n\r\n现代职场压力巨大，公司引入专业心理健康服务体系（EAP），免费向员工提供以下服务：\r\n\r\n- 每年6次心理咨询（电话、在线或面对面）；\r\n- 职场情绪调节、亲子关系、睡眠管理等主题心理讲座；\r\n- 心理压力自测工具及心理课程库；\r\n- 专属心理服务热线，支持24小时匿名咨询；\r\n- 特殊事件干预（如亲属离世、重大变动）由HR协调专属心理顾问介入支持。\r\n\r\n所有心理服务均由第三方合规服务机构执行，员工信息严格保密，公司不会获知具体内容。\r\n\r\n#### 三、运动与健康生活激励\r\n\r\n- 公司与多家健身机构合作，员工可使用年度健身补贴（最高2000元）；\r\n- 每年组织两次“健康挑战月”，如“每日步数达标挑战”“零外卖挑战”等；\r\n- 设置“健康积分商城”，积分可通过步数、饮食打卡等方式积累，兑换健康餐券、按摩器、体脂秤等福利；\r\n- 办公场所设置休闲健身区，配备瑜伽垫、拉伸带、可调节桌椅；\r\n- 鼓励员工参与骑行、登山、羽毛球、篮球等运动社团，公司按人均每月100元标准提供场地或装备补贴。\r\n\r\n### 7.4 疾病防控与疫情响应\r\n\r\n#### 一、传染病管理与响应机制\r\n\r\n公司参考国家卫健委、疾控中心、世界卫生组织发布的标准，建立“传染病防控与应急响应机制”，适用于：\r\n\r\n- 流感季节、疫情爆发阶段的快速反应；\r\n- 个人确诊传染病后的隔离、通报和恢复流程；\r\n- 密接人员的排查与健康管理；\r\n- 疫苗接种倡导与协助服务。\r\n\r\n公司设有“疫情响应小组”，由行政、HR、法务、医疗顾问构成，职责包括：\r\n\r\n- 发布口罩佩戴、场所消杀、出入登记等管控政策；\r\n- 设立居家办公政策、线上会议优先机制；\r\n- 协助员工核酸检测与就医转诊；\r\n- 处理传染病相关误解、歧视和信息发布。\r\n\r\n#### 二、居家隔离与线上支持\r\n\r\n如员工确诊传染病（如新冠、甲流、肺结核等），公司将提供以下支持：\r\n\r\n- 正常病假薪酬与补贴；\r\n- 所需物资配送服务（药品、口罩等）；\r\n- 安排线上每日健康回访；\r\n- 如员工居住地封控或有密接史，可申请临时远程办公；\r\n- 部门主管需合理安排交接或延后不紧急任务，避免员工病中仍“被动加班”。\r\n\r\n### 7.5 紧急事件响应机制\r\n\r\n#### 一、应急预案覆盖范围\r\n\r\n公司已制定全面的《应急处置预案》，适用于以下场景：\r\n\r\n- 火灾；\r\n- 地震或自然灾害；\r\n- 暴力入侵或恐袭事件；\r\n- 办公楼电梯、楼梯踩踏风险；\r\n- 员工突发疾病（心梗、哮喘、过敏）；\r\n- 数据中心断电、系统崩溃；\r\n- 水管爆裂、空调系统故障、气体泄漏等设施问题。\r\n\r\n#### 二、演练与培训\r\n\r\n- 每年进行不少于**2次应急疏散演练**，确保员工掌握逃生路线；\r\n- 所有行政、安全人员需接受**消防证、急救员资格证培训**；\r\n- 定期举办**CPR（心肺复苏）与AED使用培训**；\r\n- 应急联系人、急救志愿者名单在员工门户长期公示，方便应对突发事件。\r\n\r\n#### 三、紧急报告机制\r\n\r\n员工在遭遇紧急事件时应立即拨打公司应急热线（分部门张贴），或通过企业IM标记“#紧急”发布信息，系统将自动通知安保、行政、HR、IT等多部门联动响应。\r\n\r\n公司要求：\r\n\r\n- 每位员工知晓最近逃生出口；\r\n- 熟悉灭火器使用流程；\r\n- 知道所在楼层的疏散集结点；\r\n- 发生事件时优先自保，不擅自行动或返回办公室取物。\r\n\r\n### 7.6 健康与安全文化建设\r\n\r\n#### 一、安全文化倡导\r\n\r\n公司不仅强调制度与流程，更希望通过文化建设提升全员健康与安全意识：\r\n\r\n- 每季度发布《健康与安全快报》，介绍典型案例与新知识；\r\n- 在内部社区设立“安全建议箱”，每条采纳建议奖励100元；\r\n- 发起“每人都是安全员”活动，每位员工可获得“微检查员”身份参与隐患排查；\r\n- 定期表彰“健康达人”“安全模范员工”，提升认同感。\r\n\r\n#### 二、全员参与与责任共担\r\n\r\n- 员工在发现潜在危险（如插座冒烟、楼道积水、电梯异响）后有义务立即上报；\r\n- 员工在办公桌区域堆放纸箱、遮挡逃生通道将被提醒整改；\r\n- 员工对不安全行为（如吸烟、电瓶车上楼等）有权劝阻或报告；\r\n- 各级主管对其管理区域负有安全第一责任，考核中纳入安全管理维度。\r\n\r\n## 第八章：信息安全与数据保护\r\n\r\n在数字化、网络化和全球化迅猛发展的背景下，信息已成为企业最核心的资产之一。任何信息系统的失控、数据泄露或网络攻击，均可能导致财产损失、品牌声誉受损、客户信任丧失，甚至触犯国家法律法规。创新科技股份有限公司高度重视信息安全与数据保护，将其视为企业运营的“生命防线”。\r\n\r\n为此，公司建立了全面的信息安全管理体系（ISMS），依据《中华人民共和国网络安全法》《数据安全法》《个人信息保护法》以及国际标准ISO/IEC 27001等，明确员工在信息使用、系统操作、数据传输、保密义务等方面的行为规范，确保公司、客户及合作伙伴的信息资产得到充分保护。\r\n\r\n### 8.1 信息安全政策\r\n\r\n#### 一、信息安全的基本原则\r\n\r\n公司信息安全管理制度遵循以下五项核心原则：\r\n\r\n1. **保密性（Confidentiality）**：信息仅授权人员可访问；\r\n2. **完整性（Integrity）**：信息在存储、传输和处理过程中不被篡改；\r\n3. **可用性（Availability）**：授权人员在需要时可正常访问和使用信息资源；\r\n4. **可追溯性（Traceability）**：所有数据操作和系统访问均可审计、追溯、记录；\r\n5. **最小权限（Least Privilege）**：任何员工仅能访问其履行岗位职责所必须的信息。\r\n\r\n#### 二、适用范围\r\n\r\n信息安全政策适用于：\r\n\r\n- 所有正式员工、实习生、外包人员、临时项目合作人员；\r\n- 所有公司内部系统、邮件平台、远程接入终端；\r\n- 涉及公司业务、客户、财务、技术、HR等全量数据；\r\n- 所有办公终端、服务器、云平台、个人移动设备等使用环境。\r\n\r\n所有人员在入职或签约前，必须签署《信息安全承诺书》与《保密协议》，明确自身义务与法律责任。\r\n\r\n#### 三、安全责任划分\r\n\r\n| 角色       | 安全责任                                   |\r\n| ---------- | ------------------------------------------ |\r\n| 高管层     | 设定信息安全战略，批准重大安全政策         |\r\n| CIO/CTO    | 主导安全体系规划与投入                     |\r\n| 信息安全部 | 建立制度、监控系统、处理事件、审计报告     |\r\n| 各部门主管 | 对本部门数据使用行为、安全培训负直接责任   |\r\n| 所有员工   | 按章操作，发现风险及时报告，不做高风险操作 |\r\n\r\n公司每年开展一次安全自评与外部审计，并将审计结果纳入部门年度运营考核指标。\r\n\r\n### 8.2 数据资产管理与防护\r\n\r\n#### 一、数据分类分级制度\r\n\r\n为有效识别、管理并保护不同敏感程度的数据，公司将数据分为四类并设定相应防护等级：\r\n\r\n| 数据等级     | 类型                           | 访问控制             | 示例                   |\r\n| ------------ | ------------------------------ | -------------------- | ---------------------- |\r\n| 一级（机密） | 核心代码、财务报表、客户合同   | 高度限制，需多重授权 | 商业秘密、加密密钥     |\r\n| 二级（内部） | 项目资料、会议纪要、日常报表   | 员工可查看但不得外发 | 内部流程文件、员工手册 |\r\n| 三级（公开） | 官网内容、公开宣传资料         | 可公开浏览           | 招聘海报、公司介绍PPT  |\r\n| 四级（个人） | 员工身份证、联系方式、健康数据 | 仅本人及HR可访问     | 入职资料、薪资信息     |\r\n\r\n所有涉及一级与二级数据的操作，必须通过VPN+工控设备完成，不得在非授权平台进行处理。\r\n\r\n#### 二、数据存储与备份\r\n\r\n公司所有重要业务数据均通过以下方式存储：\r\n\r\n- 在公司内网服务器与云平台（如AWS、阿里云）进行双重存储；\r\n- 定期（每日/每周）自动备份；\r\n- 每月对备份有效性进行验证；\r\n- 对客户敏感数据启用加密存储机制，含传输加密（TLS/SSL）和静态加密（AES-256）。\r\n\r\n个人终端数据如未同步至服务器，发生丢失风险由员工自行承担。请勿在本地磁盘保存核心资料，建议使用公司统一云盘或项目管理平台（如Notion、Confluence、GitLab等）。\r\n\r\n#### 三、数据访问权限管理\r\n\r\n- 所有权限由系统管理员通过RBAC（基于角色访问控制）机制统一管理；\r\n- 权限变更（调岗、离职、项目变更）需在24小时内完成更新；\r\n- 高风险操作（如数据库导出）需经主管+信息安全部联合审批；\r\n- 数据提取日志每周进行一次巡查，疑似违规访问将被记录与审查。\r\n\r\n### 8.3 员工行为规范与操作指南\r\n\r\n#### 一、日常信息安全行为要求\r\n\r\n1. **登录管理**：\r\n   - 登录系统时必须使用公司提供的账户；\r\n   - 严禁共享账号密码，违反视为严重违纪；\r\n   - 登录密码每90天自动过期，须定期更新。\r\n2. **邮件使用规范**：\r\n   - 公司邮箱为正式业务往来唯一通道；\r\n   - 禁止使用个人邮箱收发工作资料；\r\n   - 不点击来源不明的邮件链接或附件；\r\n   - 所有群发邮件须使用密件抄送（BCC）。\r\n3. **移动存储与打印**：\r\n   - 不得使用个人U盘或移动硬盘连接办公设备；\r\n   - 打印涉及客户信息或财务内容需主管授权；\r\n   - 机密文件打印后须及时回收并粉碎销毁。\r\n4. **软件安装与设备使用**：\r\n   - 禁止私自安装第三方软件、浏览器插件、破解工具；\r\n   - 设备故障或中毒不得私自修复，必须联系IT支持；\r\n   - 公司配发笔记本不得擅自外借或安装“双系统”。\r\n5. **离岗安全**：\r\n   - 15分钟内未操作电脑将自动锁屏；\r\n   - 离开工位须手动锁屏；\r\n   - 不得将带有工作资料的笔记本长时间暴露在公共场所。\r\n\r\n#### 二、远程办公与BYOD政策\r\n\r\n为保障远程办公的安全性，公司实施如下规定：\r\n\r\n- 必须使用公司VPN接入企业系统；\r\n- 所有远程设备须安装公司授权的终端安全软件；\r\n- 禁止在网吧、公共Wi-Fi下访问内部系统；\r\n- BYOD（自带设备）需提前登记备案并通过加密配置。\r\n\r\n### 8.4 个人信息保护政策（PIP）\r\n\r\n#### 一、定义与适用范围\r\n\r\n根据《个人信息保护法》，公司对以下信息承担保护义务：\r\n\r\n- 员工信息：身份证、手机号、邮箱、薪资、健康、住址、家庭联系人；\r\n- 客户信息：公司名称、联系人、交易记录、地址、付款方式等；\r\n- 应聘者信息：简历、学历、工作经历、面试过程记录等；\r\n- 合作方信息：合同、开票信息、对公账号、联系人等。\r\n\r\n#### 二、保护措施\r\n\r\n- 建立“个人信息处理记录”，每季度更新处理目的与范围；\r\n- 收集前必须明确告知使用目的、范围、保存期限并取得授权；\r\n- 严禁将员工/客户个人信息出售、共享、泄露给第三方；\r\n- 所有信息保留期限不超过业务所需最长6年，期满销毁；\r\n- 所有参与客户服务、数据标注、调研、IT支持等岗位，需参加个人信息保护专项培训并考核合格。\r\n\r\n#### 三、用户权利\r\n\r\n所有信息主体享有以下权利：\r\n\r\n- 查询与访问权；\r\n- 撤回授权权；\r\n- 数据更正与删除权；\r\n- 投诉与申诉权；\r\n- 拒绝非必要处理的权利。\r\n\r\n员工或客户可通过“信息安全服务中心”邮箱提出申请，公司将在15个工作日内反馈处理。\r\n\r\n### 8.5 安全事件处理与违规处罚\r\n\r\n#### 一、信息安全事件定义\r\n\r\n信息安全事件指因系统漏洞、员工操作不当、外部攻击等原因，导致信息丢失、篡改、泄露或非法使用的行为，包括但不限于：\r\n\r\n- 系统入侵、病毒传播；\r\n- 非法访问、数据外泄；\r\n- 误操作导致客户信息误发；\r\n- 加密证书泄漏；\r\n- 客户或员工举报数据异常使用；\r\n- 云服务权限配置不当。\r\n\r\n#### 二、事件应急响应流程\r\n\r\n1. 第一时间报告信息安全部（ISD）；\r\n2. 启动《信息安全应急预案》，隔离影响系统；\r\n3. 调查事件源头、受影响范围；\r\n4. 向法律合规、数据负责人汇报；\r\n5. 如符合法定上报标准，向监管部门及客户报告；\r\n6. 组织修复、更新策略、发布事件说明。\r\n\r\n公司信息安全部承诺“4小时内响应、24小时内初步结论、72小时内恢复计划”。\r\n\r\n#### 三、违规处罚\r\n\r\n依据事件等级与后果，公司将对责任人采取以下措施：\r\n\r\n| 情节             | 惩戒方式                       |\r\n| ---------------- | ------------------------------ |\r\n| 轻微违规（首次） | 口头警告、重申规则、补训       |\r\n| 中度违规         | 书面警告、绩效影响、权限调整   |\r\n| 严重违规         | 降职、停薪、解除合同、移送司法 |\r\n\r\n如行为涉嫌违法，公司将协同司法机关进行追责。\r\n\r\n## 第九章：多样性、公平与包容（D&amp;I）\r\n\r\n在全球化和人才多元化日益加速的今天，多样性、公平与包容（Diversity, Equity &amp; Inclusion, 简称 D&amp;I）不仅是现代企业履行社会责任的重要表现，更是提升组织韧性、激发创新能力与增强员工归属感的核心驱动因素。创新科技股份有限公司始终坚持“尊重差异，公平发展，包容共创”的人本理念，将 D&amp;I 原则嵌入企业治理、人才战略、文化建设和日常运营的各个环节，努力构建一个真正多元、平等、开放的工作环境。\r\n\r\n本章将系统阐述公司在多样性人才管理、反偏见培训、公平晋升机制、包容文化建设、员工支持网络、监督评估体系等方面的政策与举措，确保每一位员工无论其性别、年龄、种族、文化背景、宗教信仰、性取向、身体状况或其他身份特征，都能在公司内平等获得机会、资源与尊重。\r\n\r\n### 9.1 D&amp;I战略政策声明\r\n\r\n#### 一、价值宣言\r\n\r\n我们相信：\r\n\r\n- 多样化的员工群体能够带来更丰富的视角与创意，推动技术创新与产品多元发展；\r\n- 公平的制度安排能够保障每位员工的努力与价值被公正对待；\r\n- 包容的文化氛围能够帮助员工更安心、更积极地投入工作，从而实现自我成长与组织目标的统一。\r\n\r\nD&amp;I 是公司可持续发展战略的一部分，不是短期项目，而是长期承诺。\r\n\r\n#### 二、核心目标\r\n\r\n- 建立可衡量的 D&amp;I 发展目标，并与业务指标挂钩；\r\n- 提升女性、残障人士、少数群体在中高层管理团队中的占比；\r\n- 所有职位、薪酬、晋升、培训、绩效等环节遵循公平原则；\r\n- 所有员工均接受反歧视、偏见意识和包容力培训；\r\n- 建立高信任度的申诉与反馈机制，鼓励安全发声。\r\n\r\n### 9.2 多样性管理\r\n\r\n#### 一、招聘与人才吸引\r\n\r\n公司在招聘过程中确保消除偏见与歧视，实施以下策略：\r\n\r\n- 使用中性语言与包容表达编写职位描述，避免含有性别倾向或年龄暗示词汇；\r\n- 拓宽招聘渠道，与女性技术社区、残障人就业平台、多元高校合作，确保人才池多样化；\r\n- 所有招聘人员必须接受“无意识偏见（Unconscious Bias）”培训；\r\n- 初试阶段采用结构化面试与匿名简历评审，避免“背景标签”干扰判断；\r\n- 推行“候选人多元池”策略，每个岗位面试至少邀请1名女性或少数群体候选人（如具备条件）。\r\n\r\n#### 二、员工群体代表性监测\r\n\r\n公司每年对员工群体结构进行数据分析，包括：\r\n\r\n| 项目                   | 统计维度                            |\r\n| ---------------------- | ----------------------------------- |\r\n| 性别                   | 女性员工比例，女性技术/管理岗位占比 |\r\n| 年龄                   | 各年龄段员工比例（&lt;30, 30-45, &gt;45） |\r\n| 残障                   | 有无注册残障员工及岗位适配情况      |\r\n| 国籍/地域              | 海外员工、跨文化员工数量及地域分布  |\r\n| 教育背景               | 非985/211院校占比，非传统专业占比   |\r\n| LGBTQ+群体（匿名统计） | 自愿参与匿名调研                    |\r\n\r\n数据分析用于优化招聘策略、薪酬公平性和组织构成，不用于员工个人评估。\r\n\r\n### 9.3 公平机制建设\r\n\r\n#### 一、薪酬公平性保障\r\n\r\n- 实施年度“同岗同酬”审计，对比性别、年龄、工作年限维度的数据，确保无系统性差异；\r\n- 招聘薪资谈判中禁止依据员工历史薪资报价，统一参考岗位标准与能力匹配；\r\n- 所有薪酬等级与升迁机会公开透明，HR定期开展“公平晋升沟通日”解读政策；\r\n- 设立“薪酬审计小组”，如员工怀疑薪资不公，可提交保密申诉，HR介入复核。\r\n\r\n#### 二、晋升与评估公平\r\n\r\n- 所有管理晋升机会对内公开发布，支持员工主动申报参与评审；\r\n- 晋升评审委员会成员构成需具备多样性（性别、职级、职能）；\r\n- 绩效评分过程要求主管提供事实型证据（数据、案例、成果），避免主观标签；\r\n- 员工每年有一次自我申诉与目标修订机会，避免因前期目标设定不公而影响评级。\r\n\r\n#### 三、无障碍办公支持\r\n\r\n- 所有办公楼配备无障碍通道、电梯语音播报、无障碍卫生间；\r\n- 公司会议室均配有无线麦克风、投屏字幕，方便听力受限员工；\r\n- IT部为视障员工提供屏幕阅读工具（如NVDA），支持键盘导航等操作习惯；\r\n- 可远程办公岗位优先支持残障员工申请灵活工作制。\r\n\r\n### 9.4 包容性文化建设\r\n\r\n#### 一、反歧视与反骚扰政策（与第六章联动）\r\n\r\n公司在第六章中已明确设立“零容忍”政策，禁止一切基于下列特征的歧视或骚扰行为：\r\n\r\n- 性别与性别认同\r\n- 年龄与婚育状况\r\n- 残障与健康状况\r\n- 国籍与语言能力\r\n- 宗教信仰与文化背景\r\n- 性取向与LGBTQ+身份\r\n\r\n所有员工每年必须完成D&amp;I在线课程，通过结业测试后方可进入下一轮绩效考核。\r\n\r\n#### 二、包容语言倡导\r\n\r\n公司推行“包容语言”使用规范，员工在邮件、会议、公开发言时应避免以下用语：\r\n\r\n- “女程序员”、“老员工”、“正常人”、“小姑娘”等具有隐含标签性别/年龄/能力歧视；\r\n- 使用中性词如“同事”“技术人员”“团队成员”替代；\r\n- 所有面向外部的内容（如演讲、招聘）须通过HR内容审校；\r\n- 推出《职场包容表达手册》并设立示范用语表。\r\n\r\n### 9.5 员工资源小组（ERG）与支持网络\r\n\r\n公司支持成立多样性相关的**员工资源小组（Employee Resource Groups, ERG）**，目前已批准设立：\r\n\r\n| 小组名称         | 目标与活动                               |\r\n| ---------------- | ---------------------------------------- |\r\n| Women@Innovation | 女性发展分享、女性技术讲座、导师配对     |\r\n| DiverseAbilities | 残障包容倡导、办公适配建议、辅助工具试点 |\r\n| Pride+           | LGBTQ+社区支持、职场友好建设             |\r\n| NewGeneration    | Z世代员工文化交流、跨代对话工作坊        |\r\n| CultureMix       | 多语言、多民族员工联谊、海外节日交流会   |\r\n\r\nERG在公司支持下可申请年度预算（每组不超过5万元），用于组织线下活动、出版手册、开展公益倡导项目等。员工可匿名参与，无需公开身份。\r\n\r\n### 9.6 D&amp;I培训与赋能\r\n\r\n#### 一、培训计划\r\n\r\n- 新员工入职前须完成D&amp;I意识入门课程；\r\n- 所有中层以上管理人员需每两年完成一次“包容性领导力”线下培训；\r\n- 高风险岗位（如面试官、人力资源、法务等）需接受“反偏见”“识别歧视信号”“员工沟通技巧”等专题培训；\r\n- 绩效面谈、辞退沟通等敏感场景设有专项辅导材料。\r\n\r\n#### 二、包容性管理技能建设\r\n\r\n公司为各级主管配备《包容性管理者工具包》，包括：\r\n\r\n- 包容性会议组织技巧（如发言轮次引导、避免“一言堂”）；\r\n- 冲突中的文化理解框架（如跨文化差异解释）；\r\n- 建设性反馈的语言结构模板；\r\n- 非语言信号识别指南（如孤立迹象、情绪波动预警）；\r\n- “困难对话”模拟练习清单。\r\n\r\n### 9.7 监督机制与数据披露\r\n\r\n#### 一、指标追踪与透明披露\r\n\r\n公司设立D&amp;I年度目标指标，HR与战略发展部每年联合发布《D&amp;I年度报告》，内容包括：\r\n\r\n- 关键数据：性别比例、晋升机会分布、薪酬差距、管理层多样性；\r\n- 重点项目：D&amp;I培训覆盖率、ERG运营情况、典型案例；\r\n- 外部合作：与大学、非营利组织合作内容、社会影响；\r\n- 员工反馈：来自年度满意度调查的D&amp;I维度数据。\r\n\r\n该报告内部公开，并视情况摘要对外披露，接受股东、监管机构、行业协会审阅。\r\n\r\n#### 二、员工反馈与申诉渠道\r\n\r\n- 所有员工可通过D&amp;I建议通道提交提升建议；\r\n- 对存在隐性歧视、不公平评估、边缘化等现象的员工，可通过HRBP、独立合规邮箱 [confidential@innovationtech.com](mailto:confidential@innovationtech.com) 提出反馈；\r\n- 公司设立“D&amp;I监督专员”岗位，独立于HR日常架构，直接向CHRO汇报；\r\n- 所有举报将严格保密，杜绝打击报复，申诉流程将于10个工作日内启动调查。\r\n\r\n## 第十章：环境与可持续发展\r\n\r\n在全球面临日益严峻的气候变化、资源枯竭、生物多样性下降等挑战的背景下，企业作为社会经济的重要组成部分，不仅肩负着经济责任，更承载着环境保护和可持续发展的道德与法律义务。创新科技股份有限公司自成立以来，始终将“绿色发展”作为企业核心战略之一，积极倡导低碳办公、绿色供应链和环保科技解决方案，致力于通过技术创新推动社会向可持续未来迈进。\r\n\r\n本章旨在全面介绍公司在环境管理、节能减排、可持续产品与服务开发、员工环保行为规范以及绿色文化建设等方面的具体政策与实践。公司希望通过制度化的管理机制和全民参与的倡导方式，将“可持续发展”从理念转化为行动，将“环保责任”融入每一位员工的日常行为之中。\r\n\r\n### 10.1 企业环境责任政策声明\r\n\r\n创新科技郑重承诺：\r\n\r\n1. 严格遵守国家和地方关于环保、节能、废弃物管理等方面的法律法规，主动接受监管与社会监督；\r\n2. 设立环境管理目标，将碳排放、水资源使用、电力消耗、办公耗材等指标纳入年度KPI监控；\r\n3. 推行绿色采购、绿色设计与绿色制造原则，促进产品生命周期各阶段的环境影响最小化；\r\n4. 鼓励员工践行环保行为，倡导低碳生活方式，营造绿色办公文化；\r\n5. 与客户、供应商、政府和NGO等外部利益相关方开展合作，共建绿色价值链。\r\n\r\n### 10.2 环境管理体系\r\n\r\n#### 一、组织架构与职责分工\r\n\r\n公司设立“环境与可持续发展小组”（简称“ESG小组”），隶属公司董事会战略委员会，统筹全公司环境治理工作，职责包括：\r\n\r\n- 制定年度环保目标与执行计划；\r\n- 协调各部门环境管理措施的落地；\r\n- 建立碳排放、能源使用、水资源等环境数据的统计和报告机制；\r\n- 推进公司ESG评级和可持续发展报告披露；\r\n- 协调外部审计与评估认证工作（如ISO 14001）。\r\n\r\n各事业部配备环保联络员，协助执行具体措施，形成“总部统筹、部门协作、员工参与”的三级环境管理体系。\r\n\r\n#### 二、绿色政策覆盖范围\r\n\r\n| 领域             | 政策举措与说明                                               |\r\n| ---------------- | ------------------------------------------------------------ |\r\n| 办公场所         | 节能照明、空调温控、楼宇能效监测、办公楼绿化比达30%以上      |\r\n| 数据中心         | 引入绿色机房标准，优化PUE指标（&lt;1.5），使用液冷系统减少能耗  |\r\n| 运输出行         | 出差优先选择高铁和新能源交通工具，内部班车采用电动车辆       |\r\n| 采购管理         | 与绿色认证供应商合作（如RoHS、EPEAT、ISO14001认证），优先选择环保包装和可再生材料 |\r\n| IT设备更新       | 推行设备延寿计划，回收淘汰设备并交由合规处理商分解回收       |\r\n| 废弃物分类与管理 | 设立电子废弃物、纸张、塑料、餐厨垃圾分类点，张贴清晰指引标识 |\r\n\r\n### 10.3 节能减排与资源管理\r\n\r\n#### 一、能源管理\r\n\r\n公司通过多项技术与管理手段降低电力消耗：\r\n\r\n- 全员办公区推广LED节能灯具，设定夜间自动熄灯机制；\r\n- 空调系统定期清洗与智能温控（夏季不低于26℃，冬季不高于20℃）；\r\n- 服务器部署边缘计算技术，实现任务调度优化，降低高峰期耗电；\r\n- 所有新建/改建楼宇需满足绿色建筑二星级标准。\r\n\r\n#### 二、水资源与办公耗材管理\r\n\r\n- 安装节水型水龙头与感应式冲洗系统；\r\n- 鼓励员工自带水杯，公司统一发放可重复使用水瓶；\r\n- 纸张使用遵循“双面打印+无纸化优先”原则，禁止非必要文件打印；\r\n- 所有打印机设定默认黑白/草稿模式，文印室张贴“节纸提醒”海报；\r\n- 年度耗材消耗控制在2022年基准线以下，制定部门限额预警机制。\r\n\r\n#### 三、碳排放与碳中和目标\r\n\r\n- 公司承诺在2035年前实现“运营碳中和”（涵盖办公能耗、员工通勤、数据中心等范畴）；\r\n- 已完成2023年碳排查（Scope 1和Scope 2）；\r\n- 逐步引入可再生能源（如绿电采购、光伏屋顶试点）；\r\n- 启动“碳账户”项目，试行个人碳积分记录机制，用于年度评优、奖品兑换等。\r\n\r\n### 10.4 绿色产品与服务倡议\r\n\r\n#### 一、绿色设计原则\r\n\r\n公司技术研发部门已将“绿色设计”纳入产品开发流程（Green by Design）：\r\n\r\n- 优化产品算法能耗比，减少高功耗计算任务；\r\n- 提高系统容错率与组件可更换性，延长产品生命周期；\r\n- 推广模块化设计，提升维修便利性与资源再利用率；\r\n- 在产品说明书中标注“环保使用建议”，指导客户节能操作。\r\n\r\n#### 二、数字化助力客户减碳\r\n\r\n- 推出“低碳办公SaaS平台”，帮助企业客户测量办公行为碳足迹；\r\n- 提供数据中心能效分析工具，优化资源调度与负载均衡；\r\n- 与生态伙伴共建“绿色云服务平台”，承诺使用清洁能源比例达80%以上；\r\n- 面向政府机构和高校推出“碳中和管理方案咨询”服务包。\r\n\r\n### 10.5 员工绿色行为倡导\r\n\r\n#### 一、日常行为规范\r\n\r\n公司鼓励每一位员工在日常工作中践行环保承诺：\r\n\r\n| 行为建议 | 实施说明                                                     |\r\n| -------- | ------------------------------------------------------------ |\r\n| 通勤出行 | 倡导步行、骑行或公共交通，发放绿色出行补贴；私家车通勤员工可选择参与“碳抵消项目” |\r\n| 会议活动 | 优先选择线上会议；线下会议统一提供可降解纸杯与无塑料瓶水源   |\r\n| 食堂就餐 | 实行“按需取餐、拒绝浪费”倡议；每日剩餐量计入食堂绩效考核     |\r\n| 节能使用 | 午休时段关闭显示器与照明设备；下班前检查电脑/空调关闭情况    |\r\n| 志愿行动 | 鼓励员工参加环保志愿活动（如植树节、海滩净化行动），记入年度CSR激励档案 |\r\n\r\n#### 二、绿色积分与奖励机制\r\n\r\n为鼓励员工持续参与环保，公司建立“绿色积分平台”：\r\n\r\n- 绿色行为（如少开车、节约用纸、参与培训等）可获得积分；\r\n- 每季度积分可兑换环保文具、定制纪念品、公益捐赠；\r\n- 部门累计积分纳入年终CSR绩效评估。\r\n\r\n### 10.6 培训、沟通与文化建设\r\n\r\n#### 一、员工培训\r\n\r\n- 新员工入职环节增加“可持续发展导论”模块，普及基础环保知识；\r\n- 所有技术、采购、项目管理岗须完成年度“绿色流程规范”课程；\r\n- 管理层参与“绿色领导力”培养项目，建立生态治理意识。\r\n\r\n#### 二、宣传与激励\r\n\r\n- 每月在企业内网推送“绿色生活小贴士”；\r\n- 开展“绿色一周”、“节能行动月”、“地球日主题周”等专题活动；\r\n- 评选“绿色榜样员工”、“低碳先锋部门”，授予荣誉证书与奖金；\r\n- 设立“可持续发展倡议箱”，鼓励员工提交节能环保创意。\r\n\r\n### 10.7 ESG合规、披露与外部合作\r\n\r\n#### 一、环境合规监测\r\n\r\n- 公司所有运营场所每年需完成一次环境合规自查；\r\n- 定期接受第三方机构绿色审计，问题整改记录纳入年度治理报告；\r\n- 建立内部举报机制，发现违规排放、非法弃置等问题可匿名举报。\r\n\r\n#### 二、可持续信息披露\r\n\r\n- 公司发布《年度环境与社会责任报告》，公开碳排放、节能成果、ESG目标进展；\r\n- ESG报告参照GRI标准，并接受外部专家委员会审阅；\r\n- 已参与CDP碳信息披露计划，并获得“B”等级以上评分。\r\n\r\n#### 三、生态伙伴与公益合作\r\n\r\n- 与阿拉善SEE基金会、中国绿色碳汇基金、联合国可持续发展组织等开展公益合作；\r\n- 联合高校推进“绿色技术挑战赛”“青年低碳研究基金”等项目；\r\n- 支持员工参与联合国“清洁地球计划”行动，提供带薪环保假期（每人每年2天）。\r\n\r\n### 10.8 未来展望\r\n\r\n我们深知，可持续发展之路没有终点。面对日新月异的环境挑战，公司将持续：\r\n\r\n- 探索碳金融与内部碳定价机制；\r\n- 试点绿色债券发行、碳交易平台参与；\r\n- 推动上下游协同减碳与绿色供应链建设；\r\n- 引入AI与区块链技术监控环境绩效数据；\r\n- 加强跨部门协作，确保“每一项业务都绿色、每一位员工都参与”。\r\n\r\n我们坚信，绿色不仅是一种责任，更是一种竞争力。在未来的发展征程中，愿我们共同以技术创新为引擎，以生态文明为指南，以每一位员工的绿色行为为起点，汇聚起推动社会、经济与自然协同发展的强大力量。\r\n\r\n## 第十一章：离职程序\r\n\r\n员工的离职是企业人力资源生命周期管理中的重要环节，既关系到员工个人职业路径的转折，也直接影响组织稳定性与知识资产的完整传承。为保障员工与公司双方的合法权益，维护良好的雇佣关系氛围，创新科技股份有限公司（以下简称“公司”）制定了科学、规范、人性化的离职流程及相关制度。本章旨在明确不同类型离职的适用流程、员工与部门的权责划分、相关手续办理及信息交接机制等内容，确保离职过程高效、透明、有序，并为双方保留长期合作与沟通的良好基础。\r\n\r\n### 11.1 离职类型分类\r\n\r\n公司认可并处理以下几种主要类型的员工离职情形：\r\n\r\n#### （一）主动离职\r\n\r\n员工基于个人职业发展、生活规划、家庭变故、教育进修或其他主观意愿提出辞职。该类型离职应提前提出正式书面申请，并完成相应的离职交接流程。\r\n\r\n#### （二）协议解除\r\n\r\n公司与员工通过协商一致的方式终止劳动合同，例如项目调整、组织优化、岗位取消或双方基于现实情况自愿达成一致。公司会依法给予经济补偿。\r\n\r\n#### （三）试用期不通过\r\n\r\n在员工试用期间内，若经考核不符合录用标准，或员工自行决定不继续履职，双方可根据试用期条款解除劳动关系。\r\n\r\n#### （四）违纪解除（单方解除）\r\n\r\n若员工严重违反公司规章制度、道德规范或触犯法律法规，公司有权根据《员工行为准则》及国家法律规定单方面解除劳动合同，视情况决定是否支付经济补偿或追究法律责任。\r\n\r\n#### （五）自动离职（视为放弃）\r\n\r\n若员工连续旷工3日（或月累计5日）以上，且无合理解释，视为“自动离职”；公司有权单方解除合同并保留追偿损失的权利。\r\n\r\n#### （六）退休/病退/死亡\r\n\r\n符合国家退休政策的员工，在达到法定年龄或申请提前退休时，可依法办理退休手续；患重大疾病不宜继续履职的员工可根据相关政策申请病退；员工去世视为自然解除劳动合同，公司将协助家属完成后续事务处理。\r\n\r\n### 11.2 离职申请流程\r\n\r\n对于主动辞职或协商解除，员工应严格按照以下步骤执行：\r\n\r\n#### 一、提交书面辞职申请\r\n\r\n- 员工需通过公司内网“员工服务系统”或以纸质形式向直属上级提出辞职申请，说明离职原因与拟定离职日期；\r\n- 若为管理岗、技术骨干或项目关键岗位，建议提前30个工作日提出辞职申请，以便公司妥善安排交接与人力资源配置。\r\n\r\n#### 二、部门审批\r\n\r\n- 直属主管在接收到辞职申请后，应在3个工作日内与员工进行离职沟通；\r\n- 若存在特殊情况（如不可替代性、冲突期间等），可与HR协商决定是否适当延后离职时间；\r\n- 审批后，主管需在系统内提交意见报告，HR方可启动后续流程。\r\n\r\n#### 三、HR审核与面谈\r\n\r\n- 人力资源部收到主管审批意见后，将安排离职面谈；\r\n- 面谈内容包括员工离职原因调查、对公司建议反馈、薪酬及福利结算说明、后续就业建议等；\r\n- HR将判断是否为正常离职，是否存在经济补偿、保密义务延续、竞业协议执行等需进一步处理的事项。\r\n\r\n#### 四、离职审批表签署\r\n\r\n- 员工需逐一完成以下部门签字流程：\r\n  - 所在部门主管；\r\n  - 人力资源部；\r\n  - 财务部（工资结算、报销审核）；\r\n  - IT部门（办公设备归还、账户注销）；\r\n  - 行政部（门禁卡、工牌、办公物品归还）；\r\n  - 法务部（如涉及合同责任、竞业协议等）。\r\n- 所有签字需在最后工作日前完成，特殊情况需主管提交书面说明申请延后。\r\n\r\n#### 五、离职通告与系统更新\r\n\r\n- HR将在员工最后工作日前，通过公司内部OA平台或邮箱发布正式离职通告，明确交接人选、职责划分；\r\n- 所有权限系统将同步进行账户注销或冻结，包括邮件、VPN、内网知识库、业务平台等。\r\n\r\n### 11.3 工作交接流程\r\n\r\n为确保组织连续性与知识资产传承，离职员工有义务履行以下交接责任：\r\n\r\n#### 一、任务与项目文档归档\r\n\r\n- 所有当前参与项目需整理项目背景、执行方案、关键节点、客户对接资料、进度状态；\r\n- 提交项目总结报告，并对接指定接替人进行口头与文档说明。\r\n\r\n#### 二、客户资源移交\r\n\r\n- 离职员工不得私自与客户解除联系或转移客户至新东家；\r\n- 客户沟通需纳入公司统一平台或CRM系统归档，由部门经理监控交接过程；\r\n- 离职后不得擅自使用或传播客户信息。\r\n\r\n#### 三、代码/技术资产归档\r\n\r\n- 技术岗位员工需将研发代码提交至公司代码库，明确版本标记与维护建议；\r\n- 提供API文档、注释说明、部署脚本及调试手册；\r\n- 配合技术负责人完成测试环境交接与上线支持计划。\r\n\r\n#### 四、知识库与培训材料交接\r\n\r\n- 如员工为讲师、导师、知识管理参与者，应移交其创建的文档、课件、流程图；\r\n- 若员工承担培训职责，应提前协助指定交接人熟悉流程并做好支持计划。\r\n\r\n### 11.4 薪酬、福利及经济补偿\r\n\r\n员工离职时，公司将依法、公平地处理各项结算事宜：\r\n\r\n#### 一、工资结算\r\n\r\n- 正常离职员工工资将按最后工作日至当月结算，次月发放；\r\n- 未休年假将根据国家规定换算为工资补偿（仅限合法节假日和年休）；\r\n- 如有欠薪、加班费、津贴、报销未结，须在离职面谈时提交财务复核申请。\r\n\r\n#### 二、社保与公积金处理\r\n\r\n- 离职当月社保/公积金由公司正常缴纳至最后工作日；\r\n- 员工可选择办理转移或暂停手续，HR将协助提供所需资料；\r\n- 外籍员工或异地社保政策特殊，按各地社保机构具体要求办理。\r\n\r\n#### 三、商业保险、补贴与贷款清理\r\n\r\n- 员工离职后，公司统一商业保险（含意外险、补充医疗险）于次月失效；\r\n- 已发放但尚未完成条件的购房补贴、学历提升补贴，如有提前离职违约条款，将按照比例退还；\r\n- 若员工存在内部无息借款、设备租赁、购置预付款等，须在离职前完成清偿手续。\r\n\r\n#### 四、竞业限制及保密义务延续\r\n\r\n- 如员工与公司签署了竞业限制协议，HR及法务将依据协议规定支付竞业补偿金并确认限制范围与时限；\r\n- 所有员工离职后仍应严格遵守保密义务，不得披露或使用公司商业秘密。\r\n\r\n### 11.5 离职证明与档案转移\r\n\r\n- 员工正式离职后3个工作日内，公司将出具标准《离职证明》，载明入职时间、岗位、离职原因、绩效评语等；\r\n- 员工人事档案可根据申请转至指定单位或人才市场，党团组织关系转出须经工会或党委办公室协助；\r\n- 如员工需开具《工作推荐信》或第三方背景调查材料，由所在部门领导或HR出具授权书并统一对外口径。\r\n\r\n### 11.6 离职后沟通与回聘机制\r\n\r\n#### 一、离职员工问卷与回访\r\n\r\n- 离职满30日，公司将以问卷或电话形式对员工进行满意度调查；\r\n- HR将收集员工建议用于改进组织文化、绩效管理、职业成长机制等。\r\n\r\n#### 二、人才回流与返聘政策\r\n\r\n- 公司鼓励高绩效员工在外部发展后重返岗位，设有“创新归雁计划”，允许3年内员工优先参与内部空缺岗位；\r\n- 回聘员工将享受“工龄继承”“内部推荐人奖”等政策激励；\r\n- 离职期间无不良记录、无商业纠纷的员工将被纳入“高潜人才回访库”。\r\n\r\n### 11.7 特殊情形处理说明\r\n\r\n- 员工因涉嫌舞弊、泄密、骚扰、暴力等严重违纪行为被解聘，公司保留不予出具离职证明与追诉的权利；\r\n- 若员工因伤病申请离职，公司将根据《劳动合同法》及《工伤保险条例》核算补偿；\r\n- 涉及劳动仲裁、争议调解等法律程序的离职，将暂停正常流程，待仲裁结果确认后统一处理。\r\n\r\n### 11.8 结语\r\n\r\n员工离职并不意味着合作的终结，而是另一段职业旅程的开启。公司始终尊重每一位员工的选择，并致力于以职业化、温暖、有序的方式处理每一段劳动关系的结束。我们感谢所有曾在创新科技奋斗过的同仁，希望您在新的岗位继续闪耀光芒，也欢迎有朝一日重返创新，共绘蓝图。\r\n\r\n## 第十二章 附录\r\n\r\n附录部分旨在为员工提供手册正文之外的实用性信息支持和补充说明。本章将涵盖公司各项制度的关键文书模板、常用联络方式、常见问题解答（FAQ）、法律政策引用条目、名词解释等内容，帮助员工在工作中快速查找相关资源、处理制度事务并准确理解术语含义。本章内容随着组织流程、法规变化将动态维护，员工应定期通过公司内网【HR服务平台】查阅最新版本。\r\n\r\n### 12.1 常用表单与模板索引\r\n\r\n为提升员工制度执行效率，公司提供以下表单与模板文档，员工可通过【HR服务平台】-【制度文库】-【标准模板】下载使用：\r\n\r\n| 表单名称                 | 用途说明                                   | 下载路径                           |\r\n| ------------------------ | ------------------------------------------ | ---------------------------------- |\r\n| 员工入职登记表           | 员工入职时填写，记录基础信息与岗位职责     | HR服务平台 &gt; 表单下载 &gt; 入职类     |\r\n| 试用期评估表             | 用于试用期满考核评定                       | HR服务平台 &gt; 表单下载 &gt; 考评类     |\r\n| 请假申请单               | 所有类型的假期申请须提交此表               | HR服务平台 &gt; 表单下载 &gt; 出勤类     |\r\n| 加班申报表               | 加班记录与审批使用                         | HR服务平台 &gt; 表单下载 &gt; 出勤类     |\r\n| 薪酬调整申请表           | 薪资、补贴变动的审批文档                   | HR服务平台 &gt; 表单下载 &gt; 薪酬类     |\r\n| 异动申请表               | 部门调动、岗位调整使用                     | HR服务平台 &gt; 表单下载 &gt; 岗位类     |\r\n| 离职申请与交接表         | 员工辞职与手续办理专用                     | HR服务平台 &gt; 表单下载 &gt; 离职类     |\r\n| 年度绩效评估表           | 年终评估使用，HR会提供当前年度版本         | HR服务平台 &gt; 表单下载 &gt; 考评类     |\r\n| 员工建议与反馈表         | 员工就制度、流程、文化等提出建议或问题反馈 | HR服务平台 &gt; 表单下载 &gt; 互动类     |\r\n| 保密协议签署模板         | 员工签订正式保密责任文件使用               | HR服务平台 &gt; 表单下载 &gt; 法务类     |\r\n| 竞业限制协议模板         | 关键岗位员工签署的竞业协议                 | HR服务平台 &gt; 表单下载 &gt; 法务类     |\r\n| 商业保险理赔申请表       | 员工申请公司团体意外险或医疗补偿时使用     | HR服务平台 &gt; 表单下载 &gt; 福利类     |\r\n| 员工健康状况报告与申报表 | 体检异常或传染病申报必填                   | HR服务平台 &gt; 表单下载 &gt; 健康安全类 |\r\n\r\n&gt; 注：部分表单需经所属部门领导审批盖章后提交至HR，电子签批无效时将另行注明。\r\n\r\n### 12.2 公司部门联系方式一览\r\n\r\n| 部门名称     | 负责人   | 联系电话      | 办公地点          | 对外邮箱                                                  |\r\n| ------------ | -------- | ------------- | ----------------- | --------------------------------------------------------- |\r\n| 人力资源部   | 王颖 HRD | 010-8921-2201 | 北京总部 A座5层   | [hr@innovatech.cn](mailto:hr@innovatech.cn)               |\r\n| 行政与后勤部 | 刘浩     | 010-8921-2234 | 北京总部 A座4层   | [admin@innovatech.cn](mailto:admin@innovatech.cn)         |\r\n| 财务部       | 张倩     | 010-8921-2215 | 北京总部 A座6层   | [finance@innovatech.cn](mailto:finance@innovatech.cn)     |\r\n| 法务合规部   | 周云杰   | 010-8921-2299 | 北京总部 A座7层   | [legal@innovatech.cn](mailto:legal@innovatech.cn)         |\r\n| 信息安全部   | 李伟     | 010-8921-2280 | 北京总部 A座8层   | [infosec@innovatech.cn](mailto:infosec@innovatech.cn)     |\r\n| 技术研发中心 | 黄振宇   | 010-8921-2401 | 上海分部 T2研发区 | [techlab@innovatech.cn](mailto:techlab@innovatech.cn)     |\r\n| 市场与品牌部 | 陈晓曦   | 010-8921-2601 | 广州分部 C楼3层   | [marketing@innovatech.cn](mailto:marketing@innovatech.cn) |\r\n| 员工帮助热线 | 客服团队 | 400-801-2768  | 支持远程工作时间  | [support@innovatech.cn](mailto:support@innovatech.cn)     |\r\n\r\n&gt; 内部联系推荐优先使用公司企业微信协作平台；所有非紧急事务请尽量使用邮件说明。\r\n\r\n### 12.3 常见问题与处理指引（FAQ）\r\n\r\n#### Q1：我可以在试用期请假或申请调休吗？\r\n\r\n可以。试用期员工同样享有病假、事假、婚假等国家规定法定假期权利。但累计请假超过10个工作日者，试用期考核结果可能会受影响。\r\n\r\n#### Q2：我每月的考勤异常如何确认？\r\n\r\n考勤异常记录可通过【钉钉考勤助手】-【异常打卡查询】功能查看。若有误记，请于当月25日前联系直属上级及HR进行纠正。\r\n\r\n#### Q3：公司薪资结构公开吗？绩效奖金如何计算？\r\n\r\n公司对岗位薪资级别设有标准区间，但具体金额属个人隐私。绩效奖金按“基本绩效系数×公司经营绩效×个人评级”三因素综合确定。\r\n\r\n#### Q4：我是否可以拒绝加班？\r\n\r\n公司不强制安排无偿加班，员工应根据项目安排与主管协商。必要情况下（如发布节点、客户应急）需在合规范围内服从组织安排，并依法给予调休或加班费。\r\n\r\n#### Q5：如何处理工作中的冲突？\r\n\r\n首先建议与当事人沟通澄清；如无法自行解决，应向直属主管或HRBP反馈。公司提倡使用“非对抗式沟通模型”，HR也将视情况进行协调介入。\r\n\r\n#### Q6：我发现信息安全隐患，如何上报？\r\n\r\n请立即通过【信息安全隐患上报表】提交至 [infosec@innovatech.cn](mailto:infosec@innovatech.cn)。高危事件（如员工电脑感染勒索病毒）应第一时间电话通知信息安全部。\r\n\r\n#### Q7：我如何申请岗位调动或内部转岗？\r\n\r\n请填写《异动申请表》，获得现岗位主管及目标岗位主管双签后提交至HR，由HRBP协调面谈及流程安排。\r\n\r\n#### Q8：如发生工伤，公司如何处理？\r\n\r\n请及时就医并保留完整就诊单据，同时通知主管及HR备案，公司将协助员工申请工伤鉴定并协助办理保险赔付。\r\n\r\n### 12.4 法律与政策依据条目\r\n\r\n以下为员工手册制定过程中所参照的国家法规与地方实施细则部分摘要，员工如需详细信息可联系法务部获取法规原文：\r\n\r\n- 《中华人民共和国劳动法》（2022年修订版）\r\n- 《中华人民共和国劳动合同法》（含实施条例）\r\n- 《职工带薪年休假条例》\r\n- 《中华人民共和国社会保险法》\r\n- 《企业职工奖惩条例》\r\n- 《女职工劳动保护特别规定》\r\n- 《中华人民共和国个人信息保护法》（2021年施行）\r\n- 《中华人民共和国民法典》合同编、人格权编相关条款\r\n- 《用人单位劳动保障守法诚信等级评价办法》\r\n- 《国家网信办个人信息出境标准合同办法》\r\n\r\n### 12.5 专有名词释义\r\n\r\n| 术语                                  | 定义                                                         |\r\n| ------------------------------------- | ------------------------------------------------------------ |\r\n| HRBP（Human Resource BP）             | 人力资源业务伙伴，嵌入业务一线支持招聘、绩效、发展等战略落地 |\r\n| KRA（Key Result Area）                | 关键结果领域，是员工绩效考核的重要结构之一                   |\r\n| C&amp;B（Compensation &amp; Benefit）         | 薪酬与福利管理领域的常用术语                                 |\r\n| PIP（Performance Improvement Plan）   | 绩效改善计划，用于支持绩效低于标准员工的过渡方案             |\r\n| NDA（Non-Disclosure Agreement）       | 保密协议，禁止员工在任职或离职后泄露公司敏感信息             |\r\n| BYOD（Bring Your Own Device）         | 员工自带设备办公政策，用于界定信息保护边界                   |\r\n| EHS（Environment, Health and Safety） | 企业环保、安全、健康责任系统框架                             |\r\n| L&amp;D（Learning &amp; Development）         | 学习与发展计划，包括岗位培训、课程体系、成长路径             |\r\n\r\n### 12.6 修订记录与版本说明\r\n\r\n| 版本号 | 修订时间   | 修订内容摘要                   | 修订人      | 备注                   |\r\n| ------ | ---------- | ------------------------------ | ----------- | ---------------------- |\r\n| v1.0   | 2022-07-01 | 初版制定                       | HRD 王颖    |                        |\r\n| v2.0   | 2023-03-15 | 新增远程办公与线上考勤规范     | HRBP 陈涵   | 适应疫情常态化管理需要 |\r\n| v2.5   | 2024-04-10 | 更新绩效评估机制与晋升路径     | HRD 王颖    | 配合战略人才发展计划   |\r\n| v3.0   | 2025-07-01 | 全面修订各章节，完善福利与合规 | HR 法务团队 | 当前有效版本           |\r\n\r\n&gt; 员工应始终以最新版本员工手册为准，公司保留对附录内容的更新解释权。\r\n\r\n------\r\n\r\n如您对本员工手册附录内容仍有疑问、建议或发现未涵盖问题，欢迎通过以下渠道进行反馈：\r\n\r\n- 企业微信：进入“HR服务”小程序 →“我要提问”\r\n- 邮箱联系：[hr@innovatech.cn](mailto:hr@innovatech.cn)\r\n- 现场咨询：北京总部 A座5层人力资源部，工作日 9:00-18:00\r\n\r\n&#x27;, path=None, url=None, mimetype=None), image_resource=None, audio_resource=None, video_resource=None, text_template=&#x27;{metadata_str}\n\n{content}&#x27;)]
```

```
index
```

```
&lt;llama_index.core.indices.vector_store.base.VectorStoreIndex at 0x198ab0ea1e0&gt;
```

```

```

+   持久化存储

```
# Save the index
index.storage_context.persist(&quot;storage&quot;)

# Later, load the index
from llama_index.core import StorageContext, load_index_from_storage

storage_context = StorageContext.from_defaults(persist_dir=&quot;storage&quot;)
index = load_index_from_storage(storage_context)
query_engine = index.as_query_engine()
```

+   RAG管道构建

LLM（生命周期模型）基于海量数据进行训练，但并未基于您的数据进行训练。检索增强生成（RAG）通过将您的数据添加到 LLM 已可访问的数据中来解决这个问题。您将在本文档中频繁看到 RAG 的相关内容。查询引擎、聊天引擎和代理通常使用 RAG 来完成其任务。

在 RAG 中，您的数据会被加载并准备好以供查询，即“建立索引”。用户查询会作用于索引，索引会将数据筛选到最相关的上下文。然后，该上下文和您的查询以及提示信息会发送到 LLM，LLM 会提供响应。

即使你正在构建的是聊天机器人或代理，你也需要了解 RAG 技术，以便将数据导入到你的应用程序中。

RAG流程包含五个关键阶段，而这些阶段又会成为您构建的大多数大型应用程序的组成部分。这五个阶段分别是：

+   加载：这指的是将数据从其存储位置（无论是文本文件、PDF、其他网站、数据库还是 API）导入到您的工作流程中。LlamaHub提供数百种连接器供您选择。

+   索引：这意味着创建一种数据结构，以便查询数据。对于LLM（语言学习模型）而言，这几乎总是意味着创建vector embeddings数据含义的数值表示，以及其他多种元数据策略，以便轻松准确地找到与上下文相关的数据。

+   存储：一旦数据被索引，您几乎总是希望存储索引以及其他元数据，以避免重新索引。

+   查询：对于任何给定的索引策略，您可以使用 LLM 和 LlamaIndex 数据结构进行查询，包括子查询、多步骤查询和混合策略。

+   评估：任何流程的关键步骤之一就是检查其相对于其他策略的有效性，或者在进行更改时进行评估。评估提供客观的衡量标准，用于衡量您对查询的响应的准确性、可靠性和速度。

+   装载阶段

节点和文档(Nodes and Documents)：Document是包含任何数据源的容器，例如 PDF 文件、API 输出或从数据库中检索的数据。Node是 LlamaIndex 中的数据最小单元，代表数据源的一个“数据块”Document。节点具有元数据，用于关联它们所在的文档以及其他节点。

连接器(Connectors)：数据连接器（通常称为Reader）将来自不同数据源和数据格式的数据导入到Documents和 中Nodes。

+   索引阶段

索引(Indexes)：导入数据后，LlamaIndex 会帮助您将数据建立索引，使其结构易于检索。这通常涉及生成索引vector embeddings，这些索引存储在一个名为“索引库”的专用数据库中vector store。索引还可以存储各种关于数据的元数据。

嵌入(Embeddings)：LLM 生成称为嵌入的数值数据表示embeddings。在筛选数据相关性时，LlamaIndex 会将查询转换为嵌入，而您的向量存储将找到与查询的嵌入在数值上相似的数据。

+   查询阶段

检索器(Retrievers)：检索器定义了如何根据查询从索引中高效地检索相关上下文。检索策略是决定检索数据的相关性和效率的关键。

路由器(Routers)：路由器决定使用哪个检索器从知识库中检索相关上下文。更具体地说，路由器RouterRetriever 类负责选择一个或多个候选检索器来执行查询。它们使用选择器，根据每个候选检索器的元数据和查询语句来选择最佳选项。

节点后处理器(Node Postprocessors)：节点后处理器接收一组检索到的节点，并对其应用转换、过滤或重新排序逻辑。

响应合成器(Response Synthesizers)：响应合成器使用用户查询和一组给定的检索文本块，从 LLM 生成响应。

+   数据加载

在您选择的机器学习模型能够处理数据之前，您首先需要对数据进行处理和加载。这与机器学习领域的数据清洗/特征工程流程，或者传统数据环境中的 ETL 流程类似。

该数据摄取流程通常包含三个主要阶段：

加载数据

转换数据

数据索引和存储

Loaders

在您选择的 LLM 能够处理您的数据之前，您需要先加载数据。LlamaIndex 通过数据连接器（也称为数据对象）来实现这一点Reader。数据连接器从不同的数据源提取数据，并将其格式化为Document数据对象。数据对象Document是数据（目前为文本，未来将支持图像和音频）及其元数据的集合。

使用 SimpleDirectoryReader 加载

我们最易用的阅读器是 SimpleDirectoryReader，它可以将指定目录中的每个文件转换为文档。它内置于 LlamaIndex 中，可以读取多种格式，包括 Markdown、PDF、Word 文档、PowerPoint 演示文稿、图像、音频和视频。

```
from llama_index.core import SimpleDirectoryReader

documents = SimpleDirectoryReader(&quot;./data&quot;).load_data()
```

LlamaHub reader

由于获取数据的方式多种多样，因此并非所有数据都已内置。您需要从我们的数据连接器注册表LlamaHub下载这些数据连接器。

在这个例子中，LlamaIndex 下载并安装了名为DatabaseReader 的连接器，该连接器针对 SQL 数据库运行查询，并将结果的每一行作为Document:

```
from llama_index.core import download_loader

from llama_index.readers.database import DatabaseReader

reader = DatabaseReader(
    scheme=os.getenv(&quot;DB_SCHEME&quot;),
    host=os.getenv(&quot;DB_HOST&quot;),
    port=os.getenv(&quot;DB_PORT&quot;),
    user=os.getenv(&quot;DB_USER&quot;),
    password=os.getenv(&quot;DB_PASS&quot;),
    dbname=os.getenv(&quot;DB_NAME&quot;),
)

query = &quot;SELECT * FROM users&quot;
documents = reader.load_data(query=query)
```

+   https://llamahub.ai/?tab=readers

直接创建文档

除了使用加载器之外，您还可以直接使用文档。

```
from llama_index.core import Document

doc = Document(text=&quot;text&quot;)
```

Transformations

数据加载完成后，需要对其进行处理和转换，然后才能将其存入存储系统。这些转换包括分块、提取元数据以及嵌入各个数据块。这样做是为了确保LLM能够检索并充分利用数据。

转换的输入/输出是Node对象（aDocument是 a 的子类Node）。转换也可以堆叠和重新排序。

我们有用于转换文档的高级 API 和低级 API。

```
documents = SimpleDirectoryReader(&quot;data&quot;).load_data()
```

```
len(documents)
```

```
15
```

```
documents[2]
```

```
Document(id_=&#x27;8f463e9f-6752-4087-a627-a4b34c516d3c&#x27;, embedding=None, metadata={&#x27;page_label&#x27;: &#x27;3&#x27;, &#x27;file_name&#x27;: &#x27;DeepSeek-V3.2_Open_Models_Redefining_Frontier (1).pdf&#x27;, &#x27;file_path&#x27;: &#x27;D:\\work\\jupyter\\公开课项目\\1211 Llama-index入门\\data\\DeepSeek-V3.2_Open_Models_Redefining_Frontier (1).pdf&#x27;, &#x27;file_type&#x27;: &#x27;application/pdf&#x27;, &#x27;file_size&#x27;: 13680365, &#x27;creation_date&#x27;: &#x27;2025-12-11&#x27;, &#x27;last_modified_date&#x27;: &#x27;2025-12-11&#x27;}, excluded_embed_metadata_keys=[&#x27;file_name&#x27;, &#x27;file_type&#x27;, &#x27;file_size&#x27;, &#x27;creation_date&#x27;, &#x27;last_modified_date&#x27;, &#x27;last_accessed_date&#x27;], excluded_llm_metadata_keys=[&#x27;file_name&#x27;, &#x27;file_type&#x27;, &#x27;file_size&#x27;, &#x27;creation_date&#x27;, &#x27;last_modified_date&#x27;, &#x27;last_accessed_date&#x27;], relationships={}, metadata_template=&#x27;{key}: {value}&#x27;, metadata_separator=&#x27;\n&#x27;, text_resource=None, image_resource=None, audio_resource=None, video_resource=None, text_template=&#x27;{metadata_str}\n\n{content}&#x27;)
```

```
index = VectorStoreIndex.from_documents(documents)
```

```
2025-12-11 16:02:16,710 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings &quot;HTTP/1.1 200 OK&quot;
```

```
VectorStoreIndex?
```

```
[31mInit signature:[39m
VectorStoreIndex(
    nodes: Optional[Sequence[llama_index.core.schema.BaseNode]] = [38;5;28;01mNone[39;00m,
    use_async: bool = [38;5;28;01mFalse[39;00m,
    store_nodes_override: bool = [38;5;28;01mFalse[39;00m,
    embed_model: Union[llama_index.core.base.embeddings.base.BaseEmbedding, ForwardRef([33m&#x27;LCEmbeddings&#x27;[39m), str, NoneType] = [38;5;28;01mNone[39;00m,
    insert_batch_size: int = [32m2048[39m,
    objects: Optional[Sequence[llama_index.core.schema.IndexNode]] = [38;5;28;01mNone[39;00m,
    index_struct: Optional[llama_index.core.data_structs.data_structs.IndexDict] = [38;5;28;01mNone[39;00m,
    storage_context: Optional[llama_index.core.storage.storage_context.StorageContext] = [38;5;28;01mNone[39;00m,
    callback_manager: Optional[llama_index.core.callbacks.base.CallbackManager] = [38;5;28;01mNone[39;00m,
    transformations: Optional[List[llama_index.core.schema.TransformComponent]] = [38;5;28;01mNone[39;00m,
    show_progress: bool = [38;5;28;01mFalse[39;00m,
    **kwargs: Any,
) -&gt; [38;5;28;01mNone[39;00m
[31mDocstring:[39m     
Vector Store Index.

Args:
    use_async (bool): Whether to use asynchronous calls. Defaults to False.
    show_progress (bool): Whether to show tqdm progress bars. Defaults to False.
    store_nodes_override (bool): set to True to always store Node objects in index
        store and document store even if vector store keeps text. Defaults to False
[31mInit docstring:[39m Initialize params.
[31mFile:[39m           c:\users\admin\anaconda3\envs\llamaindex\lib\site-packages\llama_index\core\indices\vector_store\base.py
[31mType:[39m           ABCMeta
[31mSubclasses:[39m     MultiModalVectorStoreIndex
```

```
query_engine = index.as_query_engine()
```

```
from llama_index.core import Settings

# 默认的全局切分大小
print(f&quot;默认 Chunk Size: {Settings.chunk_size}&quot;)  
# 输出: 1024

# 默认的全局重叠大小
print(f&quot;默认 Chunk Overlap: {Settings.chunk_overlap}&quot;) 
# 输出: 20
```

```
默认 Chunk Size: 1024
默认 Chunk Overlap: 200
```

```
print(Settings.node_parser)
```

```
include_metadata=True include_prev_next_rel=True callback_manager=&lt;llama_index.core.callbacks.base.CallbackManager object at 0x00000198AFF7F620&gt; id_func=&lt;function default_id_func at 0x00000198F6750A40&gt; chunk_size=1024 chunk_overlap=200 separator=&#x27; &#x27; paragraph_separator=&#x27;\n\n\n&#x27; secondary_chunking_regex=&#x27;[^,.;。？！]+[,.;。？！]?|[,.;。？！]&#x27;
```

```
print(Settings.embed_model)
```

```
model_name=&#x27;text-embedding-ada-002&#x27; embed_batch_size=100 callback_manager=&lt;llama_index.core.callbacks.base.CallbackManager object at 0x00000198AFF7F620&gt; num_workers=None embeddings_cache=None additional_kwargs={} api_key=&#x27;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&#x27; api_base=&#x27;https://api.openai.com/v1&#x27; api_version=&#x27;&#x27; max_retries=10 timeout=60.0 default_headers=None reuse_client=True dimensions=None
```

```
# 2. 获取所有节点 (它是一个字典，key是id，value是Node对象)
# 我们把它转成列表方便查看
all_nodes = list(index.docstore.docs.values())

print(f&quot;一共切分出了 {len(all_nodes)} 个节点&quot;)
```

```
一共切分出了 72 个节点
```

```
# 3. 抽查第一个节点看看长啥样
first_node = all_nodes[0]

print(&quot;=== 节点 ID ===&quot;)
print(first_node.node_id)

print(&quot;\n=== 节点文本 (切片内容) ===&quot;)
print(first_node.text)  # &lt;--- 这就是喂给 Embedding 的实际内容

print(&quot;\n=== 节点元数据 ===&quot;)
print(first_node.metadata) # &lt;--- 继承自 Document 的 metadata

print(&quot;\n=== 节点关系 (重点) ===&quot;)
# 这里能看到它指向的“上一块”和“下一块”是谁
print(first_node.relationships)
```

```
=== 节点 ID ===
353a21cd-d7b6-4b8d-b90a-eaae6c9da74d

=== 节点文本 (切片内容) ===


=== 节点元数据 ===
{&#x27;page_label&#x27;: &#x27;1&#x27;, &#x27;file_name&#x27;: &#x27;DeepSeek-V3.2_Open_Models_Redefining_Frontier (1).pdf&#x27;, &#x27;file_path&#x27;: &#x27;D:\\work\\jupyter\\公开课项目\\1211 Llama-index入门\\data\\DeepSeek-V3.2_Open_Models_Redefining_Frontier (1).pdf&#x27;, &#x27;file_type&#x27;: &#x27;application/pdf&#x27;, &#x27;file_size&#x27;: 13680365, &#x27;creation_date&#x27;: &#x27;2025-12-11&#x27;, &#x27;last_modified_date&#x27;: &#x27;2025-12-11&#x27;}

=== 节点关系 (重点) ===
{&lt;NodeRelationship.SOURCE: &#x27;1&#x27;&gt;: RelatedNodeInfo(node_id=&#x27;77c458a0-35a8-4477-af36-97a8e9ccb9a2&#x27;, node_type=&#x27;4&#x27;, metadata={&#x27;page_label&#x27;: &#x27;1&#x27;, &#x27;file_name&#x27;: &#x27;DeepSeek-V3.2_Open_Models_Redefining_Frontier (1).pdf&#x27;, &#x27;file_path&#x27;: &#x27;D:\\work\\jupyter\\公开课项目\\1211 Llama-index入门\\data\\DeepSeek-V3.2_Open_Models_Redefining_Frontier (1).pdf&#x27;, &#x27;file_type&#x27;: &#x27;application/pdf&#x27;, &#x27;file_size&#x27;: 13680365, &#x27;creation_date&#x27;: &#x27;2025-12-11&#x27;, &#x27;last_modified_date&#x27;: &#x27;2025-12-11&#x27;}, hash=&#x27;b22865c8849b826b9e5662cfd7ec7e991da44cc989eabbf083e908383828d0e6&#x27;)}
```

```

```

```

```

```

```

```

```

```
response = query_engine.query(&quot;DeepSeek V3.2 的核心特点是什么？&quot;)
```

```
2025-12-11 16:10:06,597 - INFO - HTTP Request: POST https://api.openai.com/v1/embeddings &quot;HTTP/1.1 200 OK&quot;
2025-12-11 16:10:08,798 - INFO - HTTP Request: POST https://api.openai.com/v1/chat/completions &quot;HTTP/1.1 200 OK&quot;
```

```
response.response
```

```
&#x27;The core features of DeepSeek V3.2 include advanced data intelligence analysis, automated cloud deployment for medium-sized enterprises, and service coverage across education, retail, and finance sectors.&#x27;
```

```

```

```
index?
```

```
[31mType:[39m           VectorStoreIndex
[31mString form:[39m    &lt;llama_index.core.indices.vector_store.base.VectorStoreIndex object at 0x00000198B21D07D0&gt;
[31mFile:[39m           c:\users\admin\anaconda3\envs\llamaindex\lib\site-packages\llama_index\core\indices\vector_store\base.py
[31mDocstring:[39m     
Vector Store Index.

Args:
    use_async (bool): Whether to use asynchronous calls. Defaults to False.
    show_progress (bool): Whether to show tqdm progress bars. Defaults to False.
    store_nodes_override (bool): set to True to always store Node objects in index
        store and document store even if vector store keeps text. Defaults to False
[31mInit docstring:[39m Initialize params.
```

```

```

```

```

```

```

```
documents = SimpleDirectoryReader(&quot;data&quot;).load_data()
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()
```

```

```

```

```

```

```

```
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

documents = SimpleDirectoryReader(&quot;data&quot;).load_data()
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()
response = query_engine.query(&quot;Some question about the data should go here&quot;)
print(response)
```