
# 12个RAG痛点及其解决方案

> 来源：[https://agijuejin.feishu.cn/docx/PahsdDyuAoJEJ7xFtKGcUhsEnqf](https://agijuejin.feishu.cn/docx/PahsdDyuAoJEJ7xFtKGcUhsEnqf)

> 作者：向量检索实验室 | AI 搜索引擎

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/7OS0FkdstDHt_pPQcjGaBA)qq.com/s/7OS0Fkds...

原文链接：`https://towardsdatascience.com/12-rag-pain-points-and-proposed-solutions-43709939a28c`

+ 痛点1:内容缺失
+ 痛点2:错过关键文档
+ 痛点3:不在上下文中——整合策略的局限性
+ 痛点4:没有获取正确内容
+ 痛点5:格式错误
+ 痛点6:不正确的特异性
+ 痛点7:不完整
+ 痛点8:数据摄取可扩展性
+ 痛点9:结构化数据QA
+ 痛点10:从复杂 PDF 中提取数据
+ 痛点11:后备模型
+ 痛点12:LLM安全

受 Barnett 等人的论文《设计检索增强生成系统时的七个故障点》的启发，让我们在本文中探讨论文中提到的七个痛点以及开发 RAG 管道时的五个额外常见痛点。更重要的是，我们将深入研究这些 RAG 痛点的解决方案，以便在日常 RAG 开发中更好地解决这些痛点。

我使用 "痛点" 而不是 "故障点"一词，主要是因为这些点都有相应的解决方案。让我们在它们成为 RAG 流水线中的"故障"之前尝试修复它们。

首先，让我们研究一下论文中提到的七个痛点；请参阅下图。然后，我们将添加另外五个痛点及其建议的解决方案。

![](img/241220fc10bbc4946dda720109b4d972.png)

图片改编自 《设计检索增强生成系统时的七个故障点》 [1]

## 痛点 1：内容缺失

当实际答案不在知识库中时，RAG 系统提供了一个看似合理但不正确的答案，而不是说它不知道。用户收到误导性信息，导致沮丧。

我们提出了两种解决方案：

### 清理数据

数据质量差，结果自然不好。如果源数据质量很差，比如包含矛盾的信息，无论你构建的 RAG 流水线有多好，它都无法从你喂给它的垃圾中产生有价值的结果。这个解决方案不仅适用于这个痛点，也适用于本文列出的所有痛点。高质量的数据是任何运行良好的 RAG 流水线的先决条件。

### 更好的提示词

在由于知识库中缺乏信息而导致系统可能提供看似合理但不正确的答案的情况下，更好的提示词会有很大帮助。通过用 "如果你不确定答案，告诉我你不知道" 等提示词来指导系统，你可以鼓励模型承认其局限性，并更透明地传达不确定性。虽然不能保证 100% 的准确性，但在清理数据后，精心设计提示词是你所能做的最大努力之一。

## 痛点 2：错过关键文档

重要文档可能未出现在系统检索组件返回的顶部结果中。正确的答案被忽略了，导致系统无法提供准确的响应。该论文提到，"问题的答案在文档中，但排名不够高，无法返回给用户"。

我想到了两个解决方案：

### chunk_size 和 similarity_top_k 的超参数调优

`chunk_size` 和 `similarity_top_k` 都是用于管理 RAG 模型中数据检索过程的效率和有效性的参数。调整这些参数可能会影响计算效率和检索信息质量之间的权衡。我们在上一篇文章 《使用 LlamaIndex 自动进行超参数调优》 [2] 中探讨了 `chunk_size` 和 `similarity_top_k` 的超参数调优细节。请参阅下面的示例代码片段。

```
param_tuner = ParamTuner(
    param_fn=objective_function_semantic_similarity,
    param_dict=param_dict,
    fixed_param_dict=fixed_param_dict,
    show_progress=True,
)

results = param_tuner.tune()
```

该函数 `objective_function_semantic_similarity` 定义如下，`param_dict` 包含参数 `chunk_size` 和 `top_k` ，以及它们对应的建议值：

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

### 重新排序

在将检索结果发送到 LLM 之前对其重新排序显著提高了 RAG 性能。 这个 LlamaIndex 笔记本 [4]演示了以下两者之间的区别：

+ 在没有重排器的情况下直接检索前 2 个节点，检索不准确。

+ 通过检索前 10 个节点并使用 `CohereRerank` 重新排序并返回前 2 个节点进行精确检索。

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

此外，您可以使用各种嵌入和重排器来评估和增强检索器的性能，如 Ravi Theja [5] 的 《通过选择最佳嵌入和重排模型提升 RAG 性能》 [6]一文中所述。

## 痛点 3：不在上下文中——整合策略的局限性

该论文定义了这一点："包含答案的文档从数据库中检索到了，但没有进入生成答案的上下文。当从数据库返回许多文档，并进行合并过程来检索答案时，就会发生这种情况"。

除了如上所述添加重排器并微调重排器外，我们还可以探索以下可能的解决方案：

### 调整检索策略

LlamaIndex 提供了一系列检索策略，从基本到高级，帮助我们在 RAG 管道中实现准确的检索。查看检索器模块指南，了解所有检索策略的综合列表，分为不同类别:

+ 从每个索引进行基本检索

+ 高级检索和搜索

+ 自动检索

+ 知识图谱检索器

+ 组合/分层检索器

+ 还有更多！

### 微调嵌入

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

## 痛点 4：没有获取正确内容

该系统难以从提供的上下文中提取正确的答案，尤其是在信息过载时。关键细节被遗漏，影响了回复的质量。该论文提到："当上下文中有太多噪音或相互矛盾的信息时，就会发生这种情况"。

让我们探讨三种解决方案：

### 清理数据

这个痛点是又一个典型的坏数据受害者。我们再怎么强调干净数据的重要性也不为过！在指责你的 RAG 流水线之前，一定要先花时间清理你的数据。

### 提示词压缩

`LongLLMLingua` 研究项目/论文 [7]中介绍了长上下文环境中的即时压缩。通过在 LlamaIndex 中的集成，我们现在可以将 `LongLLMLingua` 实现为节点后处理器，它将在检索步骤后压缩上下文，然后将其输入 LLM。

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

请参阅下面的示例代码片段，我们在其中设置 `LongLLMLinguaPostprocessor` 了 ，它使用包 `longllmlingua` 来运行提示压缩。

**有关更多详细信息，请查看 `LongLLMLingua` 上的 完整笔记本 [8]。**

### LongContextReorder

一项研究观察到，当关键数据位于输入上下文的开始或结尾时，通常会出现最佳性能。`LongContextReorder` 旨在通过对检索到的节点重新排序来解决这个 "中间丢失" 的问题，这在需要大型 top-k 的情况下会很有帮助。

请参阅下面的示例代码片段，了解如何在查询引擎构造期间定义为 `LongContextReorder` 。

有关更多详细信息，请参阅 LlamaIndex 的 完整 `LongContextReorder` 笔记本。 [9]

```
from llama_index.postprocessor import LongContextReorder

reorder = LongContextReorder()

reorder_engine = index.as_query_engine(
    node_postprocessors=[reorder], similarity_top_k=5
)

reorder_response = reorder_engine.query(&quot;Did the author meet Sam Altman?&quot;)
```

## 痛点 5：格式错误

当 LLM 忽略了以特定格式（如表格或列表）提取信息的指令时，我们有四个建议的解决方案可供探索：

### 更好的提示词

您可以采用以下几种策略来改进提示并纠正此问题：

+ 澄清指令

+ 简化请求并使用关键字

+ 举例说明

+ 反复提示并提出后续问题

### 输出解析

输出解析可以通过以下方式帮助确保所需的输出:

+ 为任何提示/查询提供格式化说明

+ 为 LLM 输出提供 "解析"

`LlamaIndex` 支持与其他框架提供的输出解析模块的集成，如 `Guardrails` [10] 和 `LangChain` [11]。

请参阅下面的 `LangChain` 输出解析模块的示例代码片段，您可以在 `LlamaIndex` 中使用。有关更多详细信息，请查看有关输出解析模块的 `LlamaIndex` 文档。
