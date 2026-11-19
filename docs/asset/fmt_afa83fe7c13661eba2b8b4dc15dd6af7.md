
# RAG 系统开发中的 12 大痛点及解决方案

> 来源：[https://v11enp9ok1h.feishu.cn/docx/DgMOdDEn8ouoxNxrjiGc7y6snkg](https://v11enp9ok1h.feishu.cn/docx/DgMOdDEn8ouoxNxrjiGc7y6snkg)

![](img/486f4f88c894177595bf9ee804d6f10c.png)

图源自 Barnett 等人的研究 工程化检索增强生成系统时的七大挑战

+ [痛点 1：缺失内容](https://baoyu.io/translations/rag/12-rag-pain-points-and-proposed-solutions#pain-point1)
+ [痛点 2：关键文档被遗漏](https://baoyu.io/translations/rag/12-rag-pain-points-and-proposed-solutions#pain-point2)
+ 痛点 3：文档整合的长度限制 —— 超出上下文
+ [痛点 4：提取困难](https://baoyu.io/translations/rag/12-rag-pain-points-and-proposed-solutions#pain-point4)
+ [痛点 5：格式错误](https://baoyu.io/translations/rag/12-rag-pain-points-and-proposed-solutions#pain-point5)
+ [痛点 6：缺乏具体细节](https://baoyu.io/translations/rag/12-rag-pain-points-and-proposed-solutions#pain-point6)
+ [痛点 7：回答不全面](https://baoyu.io/translations/rag/12-rag-pain-points-and-proposed-solutions#pain-point7)
+ 痛点 8：数据摄入的可扩展性问题
+ [痛点 9：结构化数据的问答](https://baoyu.io/translations/rag/12-rag-pain-points-and-proposed-solutions#pain-point9)
+ 痛点 10：从复杂 PDF 文档提取数据
+ [痛点 11：备用模型策略](https://baoyu.io/translations/rag/12-rag-pain-points-and-proposed-solutions#pain-point11)
+ [痛点 12：大语言模型的安全性](https://baoyu.io/translations/rag/12-rag-pain-points-and-proposed-solutions#pain-point12)

本文受到 Barnett 等人论文 工程化检索增强生成系统时的七大挑战 的启发，旨在探讨论文中的七个挑战及开发 RAG 系统时遇到的五个常见难题。更关键的是，我们将深入讨论这些难题的解决策略，帮助我们在日常开发中有效应对。

这里之所以用“挑战”而不是“失败点”，是因为每个挑战都有相对应的解决方案。在它们影响我们的 RAG 系统前，让我们先行解决。

首先看看论文提及的七个挑战，如下图所示。随后，我们会补充五个额外的挑战和它们的解决方案。

![](img/5b046b9cc2dedf5247e6a0c5940c3c20.png)

图源自 工程化检索增强生成系统时的七大挑战

## 痛点 1：缺失内容

当实际答案不在知识库中时，RAG 系统可能会提供一个貌似合理但实际错误的答案，而不是直接表明自己无法给出答案。这种情况下，用户可能会因为接收到误导性信息而感到困惑和挫败。

针对此问题，我们提出两种解决策略：

### 数据清洗的重要性

俗话说，“差料出差货”。如果你的原始数据充满错误，如信息自相矛盾，那么无论你如何搭建你的 RAG 处理流程，都无法将输入的杂乱无章转化为有价值的信息。这一解决策略不仅适用于本文讨论的这一问题，也适用于所有提及的问题。高质量的数据是构建任何有效 RAG 流程的基础条件。

### 精心设计的提示有助于提高准确性

当系统因为缺少知识库中的信息而可能给出看似合理但实际错误的回答时，一个好的提示可以大有裨益。例如，通过设置提示：“如果你对答案不确定，就告诉我你不知道”，可以鼓励模型承认其局限性，并更透明地表达不确定性。虽然无法保证百分百的准确率，但在数据清洗之后，设计恰当的提示是提高回答质量的有效手段之一。

## 痛点 2：关键文档被遗漏

有时候，重要的文档可能不会出现在系统返回的最顶端结果中，导致正确的答案被忽略，系统未能提供准确的反馈。正如一篇研究所暗示的那样：“答案虽然在文档中，但因为排名不够高而未能展现给用户”。

我想到了两种可能的解决方法：

### 通过调整 `chunk_size` 和 `similarity_top_k` 参数优化检索效果

`chunk_size` 和 `similarity_top_k` 这两个参数关键影响了 RAG 模型在数据检索过程中的效率和准确性。适当调整这些参数，可以在计算效率和信息检索质量之间找到更好的平衡点。关于如何调整这些超参数，我们在先前的文章通过 `LlamaIndex` 实现超参数自动调整中有详细讨论，并提供了示例代码。

```
param_tuner = ParamTuner(
    param_fn=objective_function_semantic_similarity,
    param_dict=param_dict,
    fixed_param_dict=fixed_param_dict,
    show_progress=True,
)

results = param_tuner.tune()
```

函数 `objective_function_semantic_similarity` 的定义如下，它利用 `param_dict` 中的参数 `chunk_size` 和 `top_k` 以及它们推荐的值进行了说明：

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

[更多细节可以参考 `LlamaIndex` 提供的关于对 RAG 进行超参数优化的完整教程。](https://docs.llamaindex.ai/en/stable/examples/param_optimizer/param_optimizer.html)

### 检索结果的优化排序

在[结果送达](https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/CohereRerank.html)大语言模型 (LLM) 前对它们进行优化排序，极大地提升了 RAG 技术的效能。`LlamaIndex` 的示例笔记清晰展现了优化排序的前后差异：

+ 未采用优化排序器，直接提取前两个节点，导致的检索不精确。
+ 先提取前十个节点，再用 `CohereRerank` 进行优化排序，精选出最相关的两个节点。

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

另外，通过各种嵌入技术和排序器，我们可以对检索性能进行评估和提升，详见 提升 RAG 性能：挑选最优的嵌入技术和排序模型 由 Ravi Theja 撰写。

更进一步，定制化的排序器经过微调后能够实现更优的检索性能，具体实施方法请参阅 通过微调 Cohere 排序器与 `LlamaIndex` 提升检索效果，也是由 Ravi Theja 介绍。

## 痛点 3：文档整合限制 —— 超出上下文

论文指出：“答案所在的文档虽从数据库中检索出来，但并未包含在生成答案的上下文中。”这种情况通常发生在数据库返回众多文档，并需通过一个整合过程来选取答案的场景。

为了解决上述问题，除了增加排序器和对其进行微调外，我们还可以尝试以下建议的解决方案：

### 调整检索策略

`LlamaIndex` 提供了多种从基础到高级的检索策略，以确保我们在 RAG 流程中能够准确地检索信息。详细的检索策略列表请参见 检索器指南，其中包括：

+ 基础检索：针对每个索引执行
+ 高级检索与搜索
+ 自动检索
+ 知识图谱检索
+ 组合/层级检索
+ 等等！

这一系列的策略为我们提供了灵活性和多样性，以适应不同的检索需求和场景，从而提高检索的精确度和有效性。

### 微调嵌入技术

如果您在使用[开源嵌入模型](https://docs.llamaindex.ai/en/stable/examples/finetuning/embeddings/finetune_embedding.html)，对其进行微调是提高检索准确度的有效手段。`LlamaIndex` 提供了一份详细的微调指南（查看微调指南），展示了如何微调开源嵌入模型，并证明了这一过程能够在多个评估指标上持续提升性能。

下方是一个示例代码片段，介绍了如何创建微调引擎、执行微调过程以及获取微调后的模型：

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

## 痛点 4：提取困难

当系统面对信息过载时，往往难以准确提取出所需的答案，关键信息的遗漏降低了回答的质量。研究表明，这种情况通常发生在上下文中存在过多干扰或矛盾信息时。

以下是针对这一问题提出的三种解决策略：

### 数据清洗

数据的质量直接影响信息提取的效果，这个痛点再次凸显了优质数据的重要性。在指责你的 RAG 系统之前，确保你已经投入足够的精力去清洗数据。

### 提示信息压缩

长上下文场景下的提示信息压缩技术首次由 `LongLLMLingua` 研究项目提出，并已在 `LlamaIndex` 中得到应用。现在，我们可以将 `LongLLMLingua` 作为节点后处理器来实施，这一步骤会在检索后对上下文进行压缩，然后再送入大语言模型处理。

下面的示例代码展示了如何设置 `LongLLMLinguaPostprocessor`，利用 `longllmlingua` 包进行提示信息的压缩处理。

[更多详细信息，请参阅有关 `LongLLMLingua` 的 完整笔记本。](https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/LongLLMLingua.html#longllmlingua)

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
        &quot;reorder_context&quot;: "sort",  # enable document reorder
    },
)

retrieved_nodes = retriever.retrieve(query_str)
synthesizer = CompactAndRefine()

## outline steps in RetrieverQueryEngine for clarity:
## postprocess (compress), synthesize
new_retrieved_nodes = node_postprocessor.postprocess_nodes(
    retrieved_nodes, query_bundle=QueryBundle(query_str=query_str)
)

print(&quot;\n\n&quot;.join([n.get_content() for n in new_retrieved_nodes]))

response = synthesizer.synthesize(query_str, new_retrieved_nodes)
```

### `LongContextReorder`(长内容优先排序)
