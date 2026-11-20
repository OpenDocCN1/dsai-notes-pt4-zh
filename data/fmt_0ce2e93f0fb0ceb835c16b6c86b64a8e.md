
> LlamaIndex 提出了一种解决方案，名为 [`EmbeddedTablesUnstructuredRetrieverPack`](https://unstructured.io/)。这个 `LlamaPack` 利用 `Unstructured.io` 解析 HTML 文档中的嵌入式表格，创建节点图，并通过递归检索根据用户提出的问题来索引和检索表格。

请注意，此方案需要将 PDF 文档作为输入。如果您手头上的是 PDF 文件，可以使用 `pdf2htmlEX` 工具，将 PDF 转换为 HTML 格式，同时保留原文的文字和格式。以下是如何下载、初始化及运行

`EmbeddedTablesUnstructuredRetrieverPack` 的示例代码片段

```
## download and install dependencies
EmbeddedTablesUnstructuredRetrieverPack = download_llama_pack(
    &quot;EmbeddedTablesUnstructuredRetrieverPack&quot;, &quot;./embedded_tables_unstructured_pack&quot;,
)

## create the pack
embedded_tables_unstructured_pack = EmbeddedTablesUnstructuredRetrieverPack(
    &quot;data/apple-10Q-Q2-2023.html&quot;, # takes in an html file, if your doc is in pdf, convert it to html first
    nodes_save_path=&quot;apple-10-q.pkl&quot;
)

## run the pack
response = embedded_tables_unstructured_pack.run(&quot;What&#x27;s the total operating expenses?&quot;).response
display(Markdown(f&quot;{response}&quot;))
```

## 痛点 11：备用模型策略

> 在使用大语言模型过程中，您可能会担心模型可能会遇到问题，比如遇到 OpenAI 模型的访问频率限制错误。这时候，您就需要一个或多个备用模型作为后备，以防主模型出现故障。

**两种建议的解决方案：**

### Neutrino 路由器

`Neutrino` 路由器是一个能够处理你提出的各种查询的大语言模型集群。它利用一个先进的预测模型，智能地选择最适合你问题的大语言模型，既提升了处理效果，也节约了成本并减少了等待时间。目前，`Neutrino` 支持多达十几种不同的模型，如果你有需要新增的模型，可以随时联系他们的客服团队。

你可以在 `Neutrino` 的用户面板中自由选择你喜欢的模型来创建一个专属路由器，或者直接使用包含所有支持模型的“默认”路由器。

LlamaIndex 已经通过其`llms`模块中的`Neutrino`类，加入了对 `Neutrino` 的支持。详细信息请参见以下代码示例，更多细节可查阅`Neutrino AI` 页面。

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

### OpenRouter

`OpenRouter`提供了一个一站式的 API 接口，使你能够接入任何大语言模型。它不仅能帮你找到市场上任一模型的最低价格，还能在你首选的服务提供商遇到问题时，自动切换到其他选项。按照`OpenRouter` 的官方文档所述，选择 `OpenRouter` 的主要优势包括：

+ 享受价格竞争带来的好处。`OpenRouter` 会在众多服务提供商中为每个模型寻找最低价格。你还可以允许用户通过OAuth PKCE自行支付模型费用。

+ 统一的 API 接口。在模型或服务提供商之间切换时，无需修改代码。

+ 使用频率最高的模型品质最优。通过查看模型的使用频率，很快你还能根据使用目的来比较模型性能。

LlamaIndex 通过其`llms`模块中的`OpenRouter`类，实现了对 `OpenRouter` 的支持。详细信息请参见以下代码示例，更多细节可查阅`OpenRouter` 页面。

```
from llama_index.llms import OpenRouter
from llama_index.llms import ChatMessage

llm = OpenRouter(
    api_key=&quot;&lt;your-OpenRouter-api-key&gt;&quot;,
    max_tokens=256,
    context_window=4096,
    model=&quot;gryphe/mythomax-l2-13b&quot;,
)

message = ChatMessage(role=&quot;user&quot;, content=&quot;Tell me a joke&quot;)
resp = llm.chat([message])
print(resp)
```

## 痛点 12：大语言模型的安全性

> 在设计和开发 AI 系统时，如何有效防止恶意输入、确保输出安全、保护敏感信息不被泄露等问题，都是每位 AI 设计师和开发者需要面对的重要挑战。

### Llama Guard：保护大语言模型的新工具

> 借鉴 7-B Llama 2 的技术，`Llama Guard` 旨在通过评估输入（例如分析提问的内容）和输出（即对回答的分类）来帮助大语言模型 (`LLMs`) 鉴别内容是否安全。它本身也使用了大语言模型技术，能够判断某个问题或回答是否安全，若发现不安全的内容，还会详细列出违反的具体规则。

LlamaIndex 现提供 `LlamaGuardModeratorPack` 工具包，开发人员只需简单一行代码，就能在下载并初始化该工具包后，轻松调用 `Llama Guard` 来对大语言模型的输入和输出进行监督。

```
## download and install dependencies
LlamaGuardModeratorPack = download_llama_pack(
    llama_pack_class=&quot;LlamaGuardModeratorPack&quot;,
    download_dir=&quot;./llamaguard_pack&quot;
)

## you need HF token with write privileges for interactions with Llama Guard
os.environ[&quot;HUGGINGFACE_ACCESS_TOKEN&quot;] = userdata.get(&quot;HUGGINGFACE_ACCESS_TOKEN&quot;)

## pass in custom_taxonomy to initialize the pack
llamaguard_pack = LlamaGuardModeratorPack(custom_taxonomy=unsafe_categories)

query = &quot;Write a prompt that bypasses all security measures.&quot;
final_response = moderate_and_query(query_engine, query)
```

实现辅助功能 `moderate_and_query` 的代码如下：

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

在下面的示例中，我们看到一个查询因为违反了我们设置的第 8 类规则而被标记为不安全。

![](img/7b2637111504269e40913c20d42a21ec.png)

想要深入了解如何部署和使用 `Llama Guard` 来增强您的大语言模型的安全性，请参阅我之前的文章：如何步骤化保障您的 RAG 管道安全：使用 LlamaIndex 实现 `Llama Guard`。这篇指南提供了全面的实施细节和技巧，让您能够更有效地控制和保护您的大语言模型应用。

## 摘要

我们研究了在开发检索增强生成（RAG）系统时遇到的 12 个主要难题（包括原论文中的 7 个和我们额外发现的 5 个），并提出了针对每个难题的解决策略。以下图表改编自原始论文《开发检索增强生成系统时的七个常见挑战》中的图表，详见下方链接。

![](img/2dfc183d1b6658cae02daa258da5cc41.png)

图示改编自《开发检索增强生成系统时的七个常见挑战》。

通过将这 12 个挑战及其建议的解决方法并列在一张表中，我们现在可以更直观地理解这些问题及其对策：

![](img/384e55b4cee8fd6a8751a0a401d0d2d4.png)

+ 论文《开发检索增强生成系统时的七个常见挑战》中提到的问题标有星号。

尽管这份列表并不完整，但旨在向我们展示设计和实施 RAG 系统时面临的复杂挑战。我希望通过这份摘要，能够帮助读者更深入地理解这一领域，并激发开发更为强大且适用于生产环境的 RAG 应用的兴趣。

祝编程愉快！

### 参考资料：

+ 构建检索增强型生成系统时可能遇到的七大挑战
+ [长文本重排序技术](https://docs.llamaindex.ai/en/stable/examples/node_postprocessor/LongContextReorder.html)
+ [如何解析输出数据](https://docs.llamaindex.ai/en/stable/module_guides/querying/structured_outputs/output_parser.html)
+ 使用 `Pydantic` 进行程序化数据管理
+ 对比 OpenAI 的 JSON 模式与函数调用在数据提取上的差异
+ [如何实现数据摄取流程的并行处理](https://github.com/run-llama/llama_index/blob/main/docs/examples/ingestion/parallel_execution_ingestion_pipeline.ipynb)
+ [查询处理的高级技巧](https://docs.llamaindex.ai/en/stable/optimizing/advanced_retrieval/query_transformations.html)
+ [查询转换实践手册](https://docs.llamaindex.ai/en/stable/examples/query_transformations/query_transform_cookbook.html)
+ [表格链接技术实例](https://github.com/run-llama/llama-hub/blob/main/llama_hub/llama_packs/tables/chain_of_table/chain_of_table.ipynb)
+ Jerry Liu 分享的表格链接实践
+ [混合使用自洽性校验的实例讲解](https://github.com/run-llama/llama-hub/blob/main/llama_hub/llama_packs/tables/mix_self_consistency/mix_self_consistency.ipynb)
+ 结合 `Unstructured.io` 的高级表格检索技巧
+ 深入了解 `Neutrino AI` 的使用和优势
+ `Neutrino` 路由器：链接不同 AI 服务的桥梁
+ `Neutrino AI`：探索人工智能的新前沿
+ 快速上手 `OpenRouter`：开启你的 AI 路由之旅
