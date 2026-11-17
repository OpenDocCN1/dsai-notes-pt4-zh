
```
from llama_index import VectorStoreIndex, SimpleDirectoryReader
from llama_index.output_parsers import LangchainOutputParser
from llama_index.llms import OpenAI
from langchain.output_parsers import StructuredOutputParser, ResponseSchema

# load documents, build index
documents = SimpleDirectoryReader("../paul_graham_essay/data").load_data()
index = VectorStoreIndex.from_documents(documents)

# define output schema
response_schemas = [
    ResponseSchema(
        name="Education",
        description="Describes the author's educational experience/background.",
    ),
    ResponseSchema(
        name="Work",
        description="Describes the author's work experience/background.",
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
    "What are a few things the author did growing up?",
)
print(str(response))
```

## Pydantic 程序

Pydantic 程序是一个通用框架，用于将输入字符串转换为结构化 Pydantic 对象。LlamaIndex 提供了几类 Pydantic 程序：

+ LLM 文本补全 Pydantic 程序：这些程序处理输入文本，并使用文本补全 API 结合输出解析将其转换为用户定义的结构化对象。

+ LLM 函数调用 Pydantic 程序：这些程序通过利用函数调用 API 的 LLM，将输入文本转换为用户指定的结构化对象。

+ 预打包的 Pydantic 程序：这些程序旨在将输入文本转换为预定义的结构化对象。

## OpenAI JSON 模式

OpenAI JSON 模式使我们能够将 `response_format` 设置为 `{"type":"json_object"}`，以启用响应的 JSON 模式。启用 JSON 模式时，模型被约束为仅生成解析为有效 JSON 对象的字符串。虽然 JSON 模式强制输出的格式，但它对针对指定模式的验证没有帮助。

更多详细信息，请查看 LlamaIndex 关于 OpenAI JSON 模式与函数调用数据提取的文档 [12]。

## 痛点 6：不正确的特异性

回答可能缺乏必要的细节或特异性，通常需要后续查询才能澄清。答案可能过于模糊或笼统，无法有效满足用户的需求。

我们求助于高级检索策略来寻找解决方案。

### 高级检索策略

当答案不在您期望的正确粒度级别时，您可以改进检索策略。一些可能有助于解决这个痛点的主要高级检索策略包括:

+ 从小到大检索 [13]

+ 句子窗口检索 [14]

+ 递归检索 [15]

查看我的上一篇文章 Jump-start Your RAG Pipelines with Advanced Retrieval LlamaPacks and Benchmark with Lighthouz AI [16]，了解有关七种高级检索 LlamaPack 的更多详细信息。

## 痛点 7：不完整

部分回答没有错；然而，它们并没有提供所有细节，尽管信息在上下文中是存在且可访问的。例如，如果有人问 "文件 A、B 和 C 中讨论的主要方面是什么？" 单独询问每个文档以确保得到全面的答案可能会更有效。

### 查询转换

比较问题在朴素的 RAG 方法中表现尤其差。提高 RAG 推理能力的一个好方法是添加查询理解层——在实际查询向量存储之前添加查询转换。以下是四种不同的查询转换：

+ 路由：保留初始查询，同时精确定位它所属的工具的适当子集。然后，将这些工具指定为适当的选项。

+ 查询重写：维护所选工具，但以多种方式重新编写查询，以便将其应用于同一组工具。

+ 子问题：将查询分解为几个较小的问题，每个问题都针对由元数据确定的不同工具。

+ ReAct Agent 工具选择：根据原始查询，确定要使用的工具，并制定要在该工具上运行的特定查询。

请参阅下面的示例代码片段，了解如何使用 HyDE（假设文档嵌入），这是一种查询重写技术。给定自然语言查询，首先生成一个假设的文档/答案。然后将此假设文档而不是原始查询用于嵌入查找。

```
# load documents, build index
documents = SimpleDirectoryReader("../paul_graham_essay/data").load_data()
index = VectorStoreIndex(documents)

# run query with HyDE query transform
query_str = "what did paul graham do after going to RISD"
hyde = HyDEQueryTransform(include_original=True)
query_engine = index.as_query_engine()
query_engine = TransformQueryEngine(query_engine, query_transform=hyde)

response = query_engine.query(query_str)
print(response)
```

查看 LlamaIndex 的查询转换手册 [17]，了解所有详细信息。

另外，请查看 Iulia Brezeanu 撰写的这篇精彩文章《改进 RAG 的高级查询转换》[18]，了解有关查询转换技术的详细信息。

> ❝
>
> 以上痛点均来自论文。现在，让我们探讨一下 RAG 开发中常见的另外五个痛点，以及它们提出的解决方案。

## 痛点 8：数据摄取的可扩展性

RAG 流水线中的数据摄取可伸缩性问题是指当系统难以有效管理和处理大量数据时出现的挑战，从而导致性能瓶颈和潜在的系统故障。这样的数据摄取可伸缩性问题可能会导致摄取时间延长、系统过载、数据质量问题和可用性有限。

### 并行化摄取管道

LlamaIndex 提供摄取管道并行处理，这一功能使 LlamaIndex 中的文档处理速度提高了 15 倍。请参阅下面的示例代码片段，了解如何创建 `IngestionPipeline` 并指定 `num_workers` 以调用并行处理。查看 LlamaIndex 的完整笔记本 [19]了解更多详情。

```
# load data
documents = SimpleDirectoryReader(input_dir="./data/source_files").load_data()

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

## 痛点 9：结构化数据问答

准确解释用户查询以检索相关结构化数据可能很困难，尤其是在查询复杂或不明确、文本转化到 SQL 的不灵活以及当前 LLM 在有效处理这些任务方面的局限性的情况下。

LlamaIndex 提供两种解决方案。

### Chain-of-table Pack

`ChainOfTablePack` 是基于 Wang 等人创新的 "表链" 论文 [20]的一个 LlamaPack。"表链" 将思想链的概念与表的转换和表示相结合。它使用一组受约束的操作逐步转换表，并在每个阶段将修改后的表呈现给 LLM。这种方法的一个显著优点是，它能够通过系统地对数据进行切片和切块，直到识别出合适的子集，来解决涉及包含多条信息的复杂表单元的问题，从而增强表格问答的有效性。

查看 LlamaIndex 的完整笔记本 [21]，详细了解如何查询 `ChainOfTablePack` 结构化数据。

### Mix-Self-Consistency Pack

LLM 可以通过两种主要方式对表格数据进行推理：

+ 基于直接提示的文本推理

+ 通过程序合成进行符号推理（例如 Python、SQL 等）

基于 Liu 等人的论文《用大语言模型重新思考表格数据理解》[22]，LlamaIndex 开发了 `MixSelfConsistencyQueryEngine`，该引擎利用自一致性机制（即多数投票）聚合文本和符号推理的结果，并实现了最佳性能。请参阅下面的示例代码片段。查看 LlamaIndex 的完整笔记本 了解更多详情。

```
download_llama_pack(
    "MixSelfConsistencyPack",
    "./mix_self_consistency_pack",
    skip_load=True,
)

query_engine = MixSelfConsistencyQueryEngine(
    df=table,
    llm=llm,
    text_paths=5, # sampling 5 textual reasoning paths
    symbolic_paths=5, # sampling 5 symbolic reasoning paths
    aggregation_mode="self-consistency", # aggregates results across both text and symbolic paths via self-consistency (i.e. majority voting)
    verbose=True,
)

response = await query_engine.aquery(example["utterance"])
```

## 痛点 10：从复杂 PDF 中提取数据

您可能需要从复杂的 PDF 文档中提取数据，例如从嵌入式表格中提取数据以进行问答。朴素的检索无法从那些嵌入的表中获取数据。您需要一种更好的方法来检索如此复杂的 PDF 数据。

### 嵌入式表格检索

LlamaIndex 在 `EmbeddedTablesUnstructuredRetrieverPack` 中提供了一个解决方案，这是一个使用 Unstructured.io [24] 从 HTML 文档中解析出嵌入表、构建节点图，然后使用递归检索根据用户问题对表进行索引/检索的 LlamaPack。

请注意，此包将 HTML 文档作为输入。如果您有 PDF 文档，可以使用 `pdf2htmlEX` [25] 将 PDF 转换为 HTML，而不会丢失文本或格式。

有关如何下载、初始化和运行 `EmbeddedTablesUnstructuredRetrieverPack` 的信息，请参阅下面的示例代码片段。

```
# download and install dependencies
EmbeddedTablesUnstructuredRetrieverPack = download_llama_pack(
    "EmbeddedTablesUnstructuredRetrieverPack", "./embedded_tables_unstructured_pack",
)

# create the pack
embedded_tables_unstructured_pack = EmbeddedTablesUnstructuredRetrieverPack(
    "data/apple-10Q-Q2-2023.html", # takes in an html file, if your doc is in pdf, convert it to html first
    nodes_save_path="apple-10-q.pkl"
)

# run the pack 
response = embedded_tables_unstructured_pack.run("What's the total operating expenses?").response
display(Markdown(f"{response}"))
```

## 痛点 11：回退模型

在使用 LLM 时，您可能会想，如果您的模型遇到问题，例如 OpenAI 模型的速率限制错误，该怎么办。您需要一个后备模型作为备份，以防主模型出现故障。

两个建议的解决方案：

### Neutrino 路由器

Neutrino [26] 路由器是一组语言模型(LLM)的集合,你可以将查询发送给这些模型。它使用一个预测模型来智能地将查询发送给最适合某个提示的LLM,在优化成本和延迟的同时最大化性能。Neutrino目前支持 十几个模型 [27]。如果你希望将新模型添加到他们支持的模型列表中,请联系他们的支持部门。

您可以创建一个路由器，在 Neutrino 仪表板中手动选择您喜欢的模型，也可以使用 "默认" 路由器，其中包括所有支持的模型。

LlamaIndex 通过其 `llms` 模块中的类集成了 Neutrino 支持。请参阅下面的代码片段。在 Neutrino AI 页面 [28]上查看更多详细信息。

```
from llama_index.llms import Neutrino
from llama_index.llms import ChatMessage

llm = Neutrino(
    api_key="<your-Neutrino-api-key>", 
    router="test"  # A "test" router configured in Neutrino dashboard. You treat a router as a LLM. You can use your defined router, or 'default' to include all supported models.
)

response = llm.complete("What is large language model?")
print(f"Optimal model: {response.raw['model']}")
```

### OpenRouter

OpenRouter [29] 是一个统一的 API，用于访问任何 LLM。它找到了任何模型的最低价格，并在主机出现故障时提供回退。根据 OpenRouter 官方文档 [30]，使用 OpenRouter 的主要好处包括：

+ 从竞争中获益。OpenRouter 在数十家供应商中找到了每个模型的最低价格。您还可以让用户通过 OAuth PKCE 为自己的模型付费。

+ API 标准化。在模型或提供者之间切换时，无需更改代码。

+ 最好的模型将被使用得最多。根据模型的使用频率以及很快将根据使用目的进行比较。

## 痛点 12：LLM 安全

如何对抗提示词注入、处理不安全的输出和防止敏感信息泄露，都是每个人工智能架构师和工程师需要回答的紧迫问题。

## Llama Guard
