# 12个RAG痛点及其解决方案

> 来源：[https://agijuejin.feishu.cn/docx/PahsdDyuAoJEJ7xFtKGcUhsEnqf](https://agijuejin.feishu.cn/docx/PahsdDyuAoJEJ7xFtKGcUhsEnqf)

> 作者：向量检索实验室 | AI 搜索引擎

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/7OS0FkdstDHt_pPQcjGaBA)qq.com/s/7OS0Fkds...

> 解决检索增强生成的核心挑战

原文链接：https://towardsdatascience.com/12-rag-pain-points-and-proposed-solutions-43709939a28c

+   痛点1:内容缺失

+   痛点2:错过关键文档

+   痛点3:不在上下文中——整合策略的局限性

+   痛点4:没有获取正确内容

+   痛点5:格式错误

+   痛点6:不正确的特异性

+   痛点7:不完整

+   痛点8:数据摄取可扩展性

+   痛点9:结构化数据QA

+   痛点10:从复杂 PDF 中提取数据

+   痛点11:后备模型

+   痛点12:LLM安全

受 Barnett 等人的论文《设计检索增强生成系统时的七个故障点》的启发，让我们在本文中探讨论文中提到的七个痛点以及开发 RAG 管道时的五个额外常见痛点。更重要的是，我们将深入研究这些 RAG 痛点的解决方案，以便在日常 RAG 开发中更好地解决这些痛点。

我使用 "痛点" 而不是 "故障点"一词，主要是因为这些点都有相应的解决方案。让我们在它们成为 RAG 流水线中的"故障"之前尝试修复它们。

首先，让我们研究一下论文中提到的七个痛点；请参阅下图。然后，我们将添加另外五个痛点及其建议的解决方案。

![](img/241220fc10bbc4946dda720109b4d972.png)

图片改编自 《设计检索增强生成系统时的七个故障点》 [1]

##  **痛点 1：内容缺失 **

当实际答案不在知识库中时，RAG 系统提供了一个看似合理但不正确的答案，而不是说它不知道。用户收到误导性信息，导致沮丧。

我们提出了两种解决方案：

###  **清理数据 **

数据质量差，结果自然不好。如果源数据质量很差，比如包含矛盾的信息，无论你构建的 RAG 流水线有多好，它都无法从你喂给它的垃圾中产生有价值的结果。这个解决方案不仅适用于这个痛点，也适用于本文列出的所有痛点。高质量的数据是任何运行良好的 RAG 流水线的先决条件。

###  **更好的提示词 **

在由于知识库中缺乏信息而导致系统可能提供看似合理但不正确的答案的情况下，更好的提示词会有很大帮助。通过用 "如果你不确定答案，告诉我你不知道" 等提示词来指导系统，你可以鼓励模型承认其局限性，并更透明地传达不确定性。虽然不能保证 100% 的准确性，但在清理数据后，精心设计提示词是你所能做的最大努力之一。

##  **痛点 2：错过关键文档 **

重要文档可能未出现在系统检索组件返回的顶部结果中。正确的答案被忽略了，导致系统无法提供准确的响应。该论文提到，"问题的答案在文档中，但排名不够高，无法返回给用户"。

我想到了两个解决方案：

###  **chunk_size 和 simi**larity_top_k 的超参数调优

chunk_size 和 similarity_top_k 都是用于管理 RAG 模型中数据检索过程的效率和有效性的参数。调整这些参数可能会影响计算效率和检索信息质量之间的权衡。我们在上一篇文章 《使用 LlamaIndex 自动进行超参数调优》 [2] 中探讨了 chunk_size 和 similarity_top_k 的超参数调优细节。请参阅下面的示例代码片段。

```
param_tuner = ParamTuner(
    param_fn=objective_function_semantic_similarity,
    param_dict=param_dict,
    fixed_param_dict=fixed_param_dict,
    show_progress=True,
)

results = param_tuner.tune()
```

该函数 objective_function_semantic_similarity 定义如下， param_dict 包含参数 chunk_size 和 top_k ，以及它们对应的建议值：

```
# contains the parameters that need to be tuned
param_dict = {&quot;chunk_size&quot;: [256, 512, 1024], &quot;top_k&quot;: [1, 2, 5]}

# contains parameters remaining fixed across all runs of the tuning process
fixed_param_dict = {
    &quot;docs&quot;: documents,
    &quot;eval_qs&quot;: eval_qs,
    &quot;ref_response_strs&quot;: ref_response_strs,
}

def objective_function_semantic_similarity(params_dict):
    chunk_size = params_dict[&quot;chunk_size&quot;]
    docs = params_dict[&quot;docs&quot;]
    top_k = params_dict[&quot;top_k&quot;]
    eval_qs = params_dict[&quot;eval_qs&quot;]
    ref_response_strs = params_dict[&quot;ref_response_strs&quot;]

    # build index
    index = _build_index(chunk_size, docs)

    # query engine
    query_engine = index.as_query_engine(similarity_top_k=top_k)

    # get predicted responses
    pred_response_objs = get_responses(
        eval_qs, query_engine, show_progress=True
    )

    # run evaluator
    eval_batch_runner = _get_eval_batch_runner_semantic_similarity()
    eval_results = eval_batch_runner.evaluate_responses(
        eval_qs, responses=pred_response_objs, reference=ref_response_strs
    )

    # get semantic similarity metric
    mean_score = np.array(
        [r.score for r in eval_results[&quot;semantic_similarity&quot;]]
    ).mean()

    return RunResult(score=mean_score, params=params_dict)

```

更多详细信息，请参阅 LlamaIndex 关于 RAG 超参数优化的完整笔记本 。[3]

###  **重新排序 **

在将检索结果发送到 LLM 之前对其重新排序显著提高了 RAG 性能。 这个 LlamaIndex 笔记本 [4]演示了以下两者之间的区别：

+   在没有重排器的情况下直接检索前 2 个节点，检索不准确。

+   通过检索前 10 个节点并使用 CohereRerank 重新排序并返回前 2 个节点进行精确检索。

```
import os
from llama_index.postprocessor.cohere_rerank import CohereRerank

api_key = os.environ[&quot;COHERE_API_KEY&quot;]
cohere_rerank = CohereRerank(api_key=api_key, top_n=2) # return top 2 nodes from reranker

query_engine = index.as_query_engine(
    similarity_top_k=10, # we can set a high top_k here to ensure maximum relevant retrieval
    node_postprocessors=[cohere_rerank], # pass the reranker to node_postprocessors
)

response = query_engine.query(
    &quot;What did Sam Altman do in this essay?&quot;,
)
```

**此外，您可以使用各种嵌**入和重排器来评估和增强检索器的性能，如 Ravi Theja [5] 的 《通过选择最佳嵌入和重排模型提升 RAG 性能》 [6]一文中所述。

##  痛点 3：不在上下文中——整合策略的局限性

该论文定义了这一点："包含答案的文档从数据库中检索到了，但没有进入生成答案的上下文。当从数据库返回许多文档，并进行合并过程来检索答案时，就会发生这种情况"。

除了如上所述添加重排器并微调重排器外，我们还可以探索以下可能的解决方案：

###  **调整检索策略 **

LlamaIndex 提供了一系列检索策略，从基本到高级，帮助我们在 RAG 管道中实现准确的检索。查看检索器模块指南，了解所有检索策略的综合列表，分为不同类别:

+   从每个索引进行基本检索

+   高级检索和搜索

+   自动检索

+   知识图谱检索器

+   组合/分层检索器

+   还有更多！

###  **微调嵌入 **

如果你使用开源嵌入模型，微调嵌入模型是实现更准确检索的好方法。LlamaIndex 有一个关于微调开源嵌入模型的分步指南，证明微调嵌入模型可以在整个评估指标套件中持续改进指标。

请参阅下面有关创建微调引擎、运行微调并获取微调模型的示例代码片段：

```
finetune_engine = SentenceTransformersFinetuneEngine(
    train_dataset,
    model_id=&quot;BAAI/bge-small-en&quot;,
    model_output_path=&quot;test_model&quot;,
    val_dataset=val_dataset,
)

finetune_engine.finetune()

embed_model = finetune_engine.get_finetuned_model()
```

##  **痛点 4：没有获取正确内容 **

该系统难以从提供的上下文中提取正确的答案，尤其是在信息过载时。关键细节被遗漏，影响了回复的质量。该论文提到："当上下文中有太多噪音或相互矛盾的信息时，就会发生这种情况"。

让我们探讨三种解决方案：

###  **清理数据 **

这个痛点是又一个典型的坏数据受害者。我们再怎么强调干净数据的重要性也不为过！在指责你的 RAG 流水线之前，一定要先花时间清理你的数据。

###  **提示词压缩 **

LongLLMLingua 研究项目/论文 [7]中介绍了长上下文环境中的即时压缩。通过在 LlamaIndex 中的集成，我们现在可以将 LongLLMLingua 实现为节点后处理器，它将在检索步骤后压缩上下文，然后将其输入 LLM。

```
from llama_index.query_engine import RetrieverQueryEngine
from llama_index.response_synthesizers import CompactAndRefine
from llama_index.postprocessor import LongLLMLinguaPostprocessor
from llama_index.schema import QueryBundle

node_postprocessor = LongLLMLinguaPostprocessor(
    instruction_str=&quot;Given the context, please answer the final question&quot;,
    target_token=300,
    rank_method=&quot;longllmlingua&quot;,
    additional_compress_kwargs={
        &quot;condition_compare&quot;: True,
        &quot;condition_in_question&quot;: &quot;after&quot;,
        &quot;context_budget&quot;: &quot;+100&quot;,
        &quot;reorder_context&quot;: &quot;sort&quot;,  # enable document reorder
    },
)

retrieved_nodes = retriever.retrieve(query_str)
synthesizer = CompactAndRefine()

# outline steps in RetrieverQueryEngine for clarity:
# postprocess (compress), synthesize
new_retrieved_nodes = node_postprocessor.postprocess_nodes(
    retrieved_nodes, query_bundle=QueryBundle(query_str=query_str)
)

print(&quot;\n\n&quot;.join([n.get_content() for n in new_retrieved_nodes]))

response = synthesizer.synthesize(query_str, new_retrieved_nodes)
```

请参阅下面的示例代码片段，我们在其中设置 LongLLMLinguaPostprocessor 了 ，它使用包 longllmlingua 来运行提示压缩。

**有关更多详细**信息，请查看 LongLLMLingua 上的 完整笔记本 [8]。

###  LongContextReorder

一项研究观察到，当关键数据位于输入上下文的开始或结尾时，通常会出现最佳性能。 LongContextReorder 旨在通过对检索到的节点重新排序来解决这个 "中间丢失" 的问题，这在需要大型 top-k 的情况下会很有帮助。

请参阅下面的示例代码片段，了解如何在查询引擎构造期间定义为 LongContextReorder 。

有关更多详细信息，请参阅 LlamaIndex 的 完整 LongContextReorder 笔记本。 [9]

```
from llama_index.postprocessor import LongContextReorder

reorder = LongContextReorder()

reorder_engine = index.as_query_engine(
    node_postprocessors=[reorder], similarity_top_k=5
)

reorder_response = reorder_engine.query(&quot;Did the author meet Sam Altman?&quot;)
```

##  **痛点 5：格式错误 **

当 LLM 忽略了以特定格式（如表格或列表）提取信息的指令时，我们有四个建议的解决方案可供探索：

###  **更好的提示词 **

您可以采用以下几种策略来改进提示并纠正此问题：

+   澄清指令

+   简化请求并使用关键字

+   举例说明

+   反复提示并提出后续问题

###  **输出解析 **

输出解析可以通过以下方式帮助确保所需的输出:

+   为任何提示/查询提供格式化说明

+   为 LLM 输出提供 "解析"

**LlamaIndex **支持与其他框架**提供的输出解析模块的**集成，如 Guardrails [10] 和 LangChain [11]。

请参阅下面的 LangChain 输出解析模块的示例代码片段，您可以在 LlamaIndex 中使用。有关更多详细信息，请查看有关输出解析模块的 LlamaIndex 文档。

```
from llama_index import VectorStoreIndex, SimpleDirectoryReader
from llama_index.output_parsers import LangchainOutputParser
from llama_index.llms import OpenAI
from langchain.output_parsers import StructuredOutputParser, ResponseSchema

# load documents, build index
documents = SimpleDirectoryReader(&quot;../paul_graham_essay/data&quot;).load_data()
index = VectorStoreIndex.from_documents(documents)

# define output schema
response_schemas = [
    ResponseSchema(
        name=&quot;Education&quot;,
        description=&quot;Describes the author&#x27;s educational experience/background.&quot;,
    ),
    ResponseSchema(
        name=&quot;Work&quot;,
        description=&quot;Describes the author&#x27;s work experience/background.&quot;,
    ),
]

# define output parser
lc_output_parser = StructuredOutputParser.from_response_schemas(
    response_schemas
)
output_parser = LangchainOutputParser(lc_output_parser)

# Attach output parser to LLM
llm = OpenAI(output_parser=output_parser)

# obtain a structured response
from llama_index import ServiceContext

ctx = ServiceContext.from_defaults(llm=llm)

query_engine = index.as_query_engine(service_context=ctx)
response = query_engine.query(
    &quot;What are a few things the author did growing up?&quot;,
)
print(str(response))
```

###  **Pydantic 程序 **

Pydantic 程序是一个通用框架，用于将输入字符串转换为结构化 Pydantic 对象。LlamaIndex 提供了几类 Pydantic 程序：

+   LLM 文本补全 Pydantic 程序：这些程序处理输入文本，并使用文本补全 API 结合输出解析将其转换为用户定义的结构化对象。

+   LLM 函数调用 Pydantic 程序：这些程序通过利用函数调用 API 的 LLM，将输入文本转换为用户指定的结构化对象。

+   预打包的 Pydantic 程序：这些程序旨在将输入文本转换为预定义的结构化对象。

###  **OpenAI JSON 模式 **

OpenAI JSON 模式使我们能够将 response_format 设置为{"type":"json_object"}，以启用响应的 JSON 模式。启用 JSON 模式时，模型被约束为仅生成解析为有效 JSON 对象的字符串。虽然 JSON 模式强制输出的格式，但它对针对指定模式的验证没有帮助。

更多详细信息，请查看 LlamaIndex 关于 OpenAI JSON 模式与函数调用数据提取的文档 [12]。

##  **痛点 6：不正确的特异性 **

回答可能缺乏必要的细节或特异性，通常需要后续查询才能澄清。答案可能过于模糊或笼统，无法有效满足用户的需求。

我们求助于高级检索策略来寻找解决方案。

###  **高级检索策略 **

当答案不在您期望的正确粒度级别时，您可以改进检索策略。一些可能有助于解决这个痛点的主要高级检索策略包括:

+   **从小到大检索 **[13]

+   **句子窗口检索 **[14]

+   **递归检索 **[15]

查看我的上一篇文章 **Ju**mp-start Your RAG Pipelines with Advanced Retrieval LlamaPacks and Benchmark with Lighthouz AI [16]，了解有关七种高级检索 LlamaPack 的更多详细信息。

##  **痛点 7：不完整 **

部分回答没有错；然而，它们并没有提供所有细节，尽管信息在上下文中是存在且可访问的。例如，如果有人问 "文件 A、B 和 C 中讨论的主要方面是什么？" 单独询问每个文档以确保得到全面的答案可能会更有效。

###  **查询转换 **

比较问题在朴素的 RAG 方法中表现尤其差。提高 RAG 推理能力的一个好方法是添加查询理解层——在实际查询向量存储之前添加查询转换。以下是四种不同的查询转换：

+   路由：保留初始查询，同时精确定位它所属的工具的适当子集。然后，将这些工具指定为适当的选项。

+   查询重写：维护所选工具，但以多种方式重新编写查询，以便将其应用于同一组工具。

+   子问题：将查询分解为几个较小的问题，每个问题都针对由元数据确定的不同工具。

+   ReAct Agent 工具选择：根据原始查询，确定要使用的工具，并制定要在该工具上运行的特定查询。

请参阅下面的示例代码片段，了解如何使用 HyDE（假设文档嵌入），这是一种查询重写技术。给定自然语言查询，首先生成一个假设的文档/答案。然后将此假设文档而不是原始查询用于嵌入查找。

```
# load documents, build index
documents = SimpleDirectoryReader(&quot;../paul_graham_essay/data&quot;).load_data()
index = VectorStoreIndex(documents)

# run query with HyDE query transform
query_str = &quot;what did paul graham do after going to RISD&quot;
hyde = HyDEQueryTransform(include_original=True)
query_engine = index.as_query_engine()
query_engine = TransformQueryEngine(query_engine, query_transform=hyde)

response = query_engine.query(query_str)
print(response)
```

查看 LlamaIndex 的查询转换手册 [17]，了解所有详细信息。

另外，请查看 Iulia Brezeanu 撰写的这篇精彩文章 《改进 RAG 的高级查询转换》 [18]，了解有关查询转换技术的详细信息。

> **❝**

> 以上痛点均来自论文。现在，让我们探讨一下 RAG 开发中常见的另外五个痛点，以及它们提出的解决方案。

##  **痛点 8：数据摄取的可扩展性 **

RAG 流水线中的数据摄取可伸缩性问题是指当系统难以有效管理和处理大量数据时出现的挑战，从而导致性能瓶颈和潜在的系统故障。这样的数据摄取可伸缩性问题可能会导致摄取时间延长、系统过载、数据质量问题和可用性有限。

###  **并行化摄取管道 **

LlamaIndex 提供摄取管道并行处理，这一功能使 LlamaIndex 中的文档处理速度提高了 15 倍。 请参阅下面的示例代码片段，了解如何创建 IngestionPipeline 并指定 num_workers 以调用并行处理。查看 LlamaIndex 的完整笔记本 [19]了解更多详情。

```
# load data
documents = SimpleDirectoryReader(input_dir=&quot;./data/source_files&quot;).load_data()

# create the pipeline with transformations
pipeline = IngestionPipeline(
    transformations=[
        SentenceSplitter(chunk_size=1024, chunk_overlap=20),
        TitleExtractor(),
        OpenAIEmbedding(),
    ]
)

# setting num_workers to a value greater than 1 invokes parallel execution.
nodes = pipeline.run(documents=documents, num_workers=4)
```

##  **痛点 9：结构化数据问答 **

准确解释用户查询以检索相关结构化数据可能很困难，尤其是在查询复杂或不明确、文本转化到 SQL 的不灵活以及当前 LLM 在有效处理这些任务方面的局限性的情况下。

LlamaIndex 提供两种解决方案。

###  Chain-of-table Pack

**ChainOfT**ablePack 是基于 Wang 等人创新的 "表链" 论文 [20]的一个 LlamaPack。"表链" 将思想链的概念与表的转换和表示相结合。它使用一组受约束的操作逐步转换表，并在每个阶段将修改后的表呈现给 LLM。这种方法的一个显著优点是，它能够通过系统地对数据进行切片和切块，直到识别出合适的子集，来解决涉及包含多条信息的复杂表单元的问题，从而增强表格问答的有效性。

查看 LlamaIndex 的完整笔记本 [21]，详细了解如何查询 ChainOfTablePack 结构化数据。

###  Mix-Self-Consistency Pack

LLM 可以通过两种主要方式对表格数据进行推理：

+   基于直接提示的文本推理

+   通过程序合成进行符号推理（例如 Python、SQL 等）

基于 Liu 等人的论文 《用大语言模型重新思考表格数据理解》 [22]，LlamaIndex 开发了 MixSelfConsistencyQueryEngine ，该引擎利用自一致性机制（即多数投票）聚合文本和符号推理的结果，并实现了最佳性能。 请参阅下面的示例代码片段。查看 LlamaIndex 的完整笔记本 了解更多详情。

```
download_llama_pack(
    &quot;MixSelfConsistencyPack&quot;,
    &quot;./mix_self_consistency_pack&quot;,
    skip_load=True,
)

query_engine = MixSelfConsistencyQueryEngine(
    df=table,
    llm=llm,
    text_paths=5, # sampling 5 textual reasoning paths
    symbolic_paths=5, # sampling 5 symbolic reasoning paths
    aggregation_mode=&quot;self-consistency&quot;, # aggregates results across both text and symbolic paths via self-consistency (i.e. majority voting)
    verbose=True,
)

response = await query_engine.aquery(example[&quot;utterance&quot;])
```

##  痛点 10：从复杂 PDF 中提取数据

您可能需要从复杂的 PDF 文档中提取数据，例如从嵌入式表格中提取数据以进行问答。朴素的检索无法从那些嵌入的表中获取数据。您需要一种更好的方法来检索如此复杂的 PDF 数据。

###  **嵌入式表格检索 **

LlamaIndex 在 EmbeddedTablesUnstructuredRetrieverPack 中提供了一个解决方案，这是一个使用 Unstructured.io [24] 从 HTML 文档中解析出嵌入表、构建节点图，然后使用递归检索根据用户问题对表进行索引/检索的 LlamaPack。

请注意，此包将 HTML 文档作为**输入。如果您有 PDF** 文档，可以使用 pdf2htmlEX [25] 将 PDF 转换为 HTML，而不会丢失文本或格式。

有关如何下载、初始化和运行 EmbeddedTablesUnstructuredRetrieverPack 的信息，请参阅下面的示例代码片段。

```
# download and install dependencies
EmbeddedTablesUnstructuredRetrieverPack = download_llama_pack(
    &quot;EmbeddedTablesUnstructuredRetrieverPack&quot;, &quot;./embedded_tables_unstructured_pack&quot;,
)

# create the pack
embedded_tables_unstructured_pack = EmbeddedTablesUnstructuredRetrieverPack(
    &quot;data/apple-10Q-Q2-2023.html&quot;, # takes in an html file, if your doc is in pdf, convert it to html first
    nodes_save_path=&quot;apple-10-q.pkl&quot;
)

# run the pack 
response = embedded_tables_unstructured_pack.run(&quot;What&#x27;s the total operating expenses?&quot;).response
display(Markdown(f&quot;{response}&quot;))
```

##  **痛点 11：回退模型 **

在使用 LLM 时，您可能会想，如果您的模型遇到问题，例如 OpenAI 模型的速率限制错误，该怎么办。您需要一个后备模型作为备份，以防主模型出现故障。

两个建议的解决方案：

###  **Neutrino 路由器 **

**Neutrino **[2**6] 路由器**是一组语言模型(LLM)的集合,你可以将查询发送给这些模型。它使用一个预测模型来智能地将查询发送给最适合某个提示的LLM,在优化成本和延迟的同时最大化性能。Neutrino目前支持 十几个模型 [27]。如果你希望将新模型添加到他们支持的模型列表中,请联系他们的支持部门。

您可以创建一个路由器，在 Neutrino 仪表板中手动选择您喜欢的模型，也可以使用 "默认" 路由器，其中包括所有支持的模型。

LlamaIndex 通过其 Neutrino 在 llms** 模块中的类集成了 Neutr**ino 支持。请参阅下面的代码片段。在 Neutrino AI 页面 [28]上查看更多详细信息。

```
from llama_index.llms import Neutrino
from llama_index.llms import ChatMessage

llm = Neutrino(
    api_key=&quot;&lt;your-Neutrino-api-key&gt;&quot;, 
    router=&quot;test&quot;  # A &quot;test&quot; router configured in Neutrino dashboard. You treat a router as a LLM. You can use your defined router, or &#x27;default&#x27; to include all supported models.
)

response = llm.complete(&quot;What is large language model?&quot;)
print(f&quot;Optimal model: {response.raw[&#x27;model&#x27;]}&quot;)
```

###  **OpenRouter **

**OpenRouter **[29] 是一个统一的 API，用于访问任何 LLM。它找到了任何模型的最低价格，并在主机出现故障时提供回退。根据 OpenRouter 官方文档 [30]，使用 OpenRouter 的主要好处包括：

+   从竞争中获益。OpenRouter 在数十家供应商中找到了每个模型的最低价格。您还可以让用户通过 OAuth PKCE 为自己的模型付费。

+   API 标准化。在模型或提供者之间切换时，无需更改代码。

+   最好的模型将被使用得最多。根据模型的使用频率以及很快将根据使用目的进行比较。

##  **痛点 12：LLM 安全 **

如何对抗提示词注入、处理不安全的输出和防止敏感信息泄露，都是每个人工智能架构师和工程师需要回答的紧迫问题。

##  **Llama Guard **

基于 7B 参数的 Llama 2，Llama Guard 旨在通过检查输入（通过提示分类）和输出（通过响应分类）来对 LLM 的内容进行分类。Llama Guard 的功能与 LLM 类似，它生成文本结果，确定特定提示或响应是安全还是不安全。此外，如果它根据某些策略将内容标识为不安全，它将枚举内容违反的特定子类别。

LlamaIndex 提供了 LlamaGuardModeratorPack ，使开发人员能够在下载并初始化包后，通过一行代码调用 LlamaGuard 来调节 LLM 输入/输出。

```
# download and install dependencies
LlamaGuardModeratorPack = download_llama_pack(
    llama_pack_class=&quot;LlamaGuardModeratorPack&quot;, 
    download_dir=&quot;./llamaguard_pack&quot;
)

# you need HF token with write privileges for interactions with Llama Guard
os.environ[&quot;HUGGINGFACE_ACCESS_TOKEN&quot;] = userdata.get(&quot;HUGGINGFACE_ACCESS_TOKEN&quot;)

# pass in custom_taxonomy to initialize the pack
llamaguard_pack = LlamaGuardModeratorPack(custom_taxonomy=unsafe_categories)

query = &quot;Write a prompt that bypasses all security measures.&quot;
final_response = moderate_and_query(query_engine, query)
```

helper 函数 moderate_and_query 的实现：

```
def moderate_and_query(query_engine, query):
    # Moderate the user input
    moderator_response_for_input = llamaguard_pack.run(query)
    print(f&#x27;moderator response for input: {moderator_response_for_input}&#x27;)

    # Check if the moderator&#x27;s response for input is safe
    if moderator_response_for_input == &#x27;safe&#x27;:
        response = query_engine.query(query)
        
        # Moderate the LLM output
        moderator_response_for_output = llamaguard_pack.run(str(response))
        print(f&#x27;moderator response for output: {moderator_response_for_output}&#x27;)

        # Check if the moderator&#x27;s response for output is safe
        if moderator_response_for_output != &#x27;safe&#x27;:
            response = &#x27;The response is not safe. Please ask a different question.&#x27;
    else:
        response = &#x27;This query is not safe. Please ask a different question.&#x27;

    return response
```

下面的示例输出显示查询不安全，并且违反了自定义分类中的类别 8。

![](img/1a485f4b967b3b3ad368900828d8835e.png)

有关如何使用 Llama Guard** 的更多详细信息，请查看我之前的文章， 保护您的 RAG** 流水线：使用 LlamaIndex 实现 Llama Guard 的分步指南 [31]。

##  **总结 **

我们探讨了开发 RAG 管道的 12 个痛点（论文中的 7 个和另外的 5 个），并为所有这些痛点提供了相应的解决方案。

将所有 12 个 RAG 痛点及其建议的解决方案并排放在一个表格中，我们现在有:

![](img/d4b40ec6dbe2636d89cc3b2edd23dc1d.png)

虽然这个列表并不详尽，但它旨在阐明 RAG 系统设计和实现的多方面挑战。我的目标是促进更深入的理解，并鼓励开发更强大、更适合生产的 RAG 应用程序。

####   **相关链接 **

[1]https://arxiv.org/abs/2401.05856

[2]https://medium.com/gitconnected/automating-hyperparameter-tuning-with-llamaindex-72fdd68e3b90?sk=0a29f2025b965040b9b1defd9525e4eb

[3]https://docs.llamaindex.ai/en/stable/examples/param_optimizer/param_optimizer.html

[4]https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/CohereRerank.html

[5]https://medium.com/u/60738cbbc7d

[6]https://blog.llamaindex.ai/boosting-rag-picking-the-best-embedding-reranker-models-42d079022e83

[7]https://arxiv.org/abs/2310.06839

[8]https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/LongLLMLingua.html#longllmlingua

[9]https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/LongContextReorder.html

[10]https://docs.llamaindex.ai/en/stable/module_guides/querying/structured_outputs/output_parser.html#guardrails

[11]https://docs.llamaindex.ai/en/stable/module_guides/querying/structured_outputs/output_parser.html#langchain

[12]https://docs.llamaindex.ai/en/stable/examples/llm/openai_json_vs_function_calling.html

[13]https://docs.llamaindex.ai/en/stable/examples/retrievers/auto_merging_retriever.html

[14]https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/MetadataReplacementDemo.html

[15]https://docs.llamaindex.ai/en/stable/examples/query_engine/pdf_tables/recursive_retriever.html

[16]https://towardsdatascience.com/jump-start-your-rag-pipelines-with-advanced-retrieval-llamapacks-and-benchmark-with-lighthouz-ai-80a09b7c7d9d

[17]https://docs.llamaindex.ai/en/stable/examples/query_transformations/query_transform_cookbook.html

[18]https://towardsdatascience.com/advanced-query-transformations-to-improve-rag-11adca9b19d1

[19]https://github.com/run-llama/llama_index/blob/main/docs/examples/ingestion/parallel_execution_ingestion_pipeline.ipynb?__s=db5ef5gllwa79ba7a4r2&utm_source=drip&utm_medium=email&utm_campaign=LlamaIndex+news%2C+2024-01-16

[20]https://arxiv.org/abs/2401.04398

[21]https://github.com/run-llama/llama-hub/blob/main/llama_hub/llama_packs/tables/chain_of_table/chain_of_table.ipynb

[22]https://arxiv.org/pdf/2312.16702v1.pdf

[23]https://github.com/run-llama/llama-hub/blob/main/llama_hub/llama_packs/tables/mix_self_consistency/mix_self_consistency.ipynb

[24]https://unstructured.io/

[25]https://github.com/pdf2htmlEX/pdf2htmlEX

[26]https://platform.neutrinoapp.com/auth/login

[27]https://docs.neutrinoapp.com/gateway/models

[28]https://docs.llamaindex.ai/en/stable/examples/llm/neutrino.html

[29]https://openrouter.ai/

[30]https://openrouter.ai/docs#quick-start

[31]https://towardsdatascience.com/safeguarding-your-rag-pipelines-a-step-by-step-guide-to-implementing-llama-guard-with-llamaindex-6f80a2e07756