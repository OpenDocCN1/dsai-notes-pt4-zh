# 使用 LlamaIndex 构建代理式RAG(2)-Tool Calling

> 来源：[https://agijuejin.feishu.cn/docx/VFtLdF7J2ou1crxKheIcoXwsnab](https://agijuejin.feishu.cn/docx/VFtLdF7J2ou1crxKheIcoXwsnab)

> 作者：lucas大叔

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/698776599)hihu.com/p/698776...

上一节展示了如何利用LLM选取不同的pipeline，这是tool calling的一种简化形式。本节将展示如何使用LLM不仅选择要执行的函数，还要推断传递给函数的参数。

在基础RAG pipeline中，LLM仅用于信息合成。工具调用使LLM能够通过动态接口与外部环境进行交互，在工具调用中不仅有助于选择适当的工具，还能推断执行所需的参数。工具调用在RAG管道的基础上增加了一层查询理解，使用户能够提出复杂的查询并获得更精确的结果。

## 配置

导入Open API KEY

```
from helper import get_openai_api_key

OPENAI_API_KEY = get_openai_api_key()
```

导入nest_asyncio模块

```
import nest_asyncio

nest_asyncio.apply()
```

## 定义简单工具

FunctionTool封装提供的任何给定的Python函数。add和mystery两个函数对于 x 和 y 变量都注释了类型，同时也提供了docstring。这么做并不仅仅是出于风格上的考虑，而是非常重要的一步，因为这些内容将被用作LLM的prompt。

LlamaIndex的函数工具与包括OpenAI在内的许多LLM模型的函数调用能力进行了原生集成。

```
from llama_index.core.tools import FunctionTool

def add(x: int, y: int) -&gt; int:
    &quot;&quot;&quot;Adds two integers together.&quot;&quot;&quot;
    return x + y

def mystery(x: int, y: int) -&gt; int: 
    &quot;&quot;&quot;Mystery function that operates on top of two numbers.&quot;&quot;&quot;
    return (x + y) * (x + y)


add_tool = FunctionTool.from_defaults(fn=add)
mystery_tool = FunctionTool.from_defaults(fn=mystery)
```

要将工具传递给LLM，需要导入LLM模块，然后调用 predict_and_call 。

这个简单的例子实际上是一个扩展版本的router。LLM不仅正确调用mystery工具，还推断出x和y参数的值。

```
from llama_index.llms.openai import OpenAI

llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;)
response = llm.predict_and_call(
    [add_tool, mystery_tool], 
    &quot;Tell me the output of the mystery function on 2 and 9&quot;, 
    verbose=True
)
print(str(response))
```

> === Calling Function ===

> Calling function: mystery with args: {"x": 2, "y": 9}

> === Function Output ===

> 121

> 121

## 定义自检索工具

下面在向量搜索之上定义一个更复杂的代理层。LLM不仅可以选择向量搜索，还可以让它推断元数据过滤器，这是一种结构化的标签列表，有助于返回更精确的搜索结果集。

### 加载数据

此处仍使用MetaGPT论文。

```
from llama_index.core import SimpleDirectoryReader
# load documents
documents = SimpleDirectoryReader(input_files=[&quot;metagpt.pdf&quot;]).load_data()
```

切分文档

```
from llama_index.core.node_parser import SentenceSplitter
splitter = SentenceSplitter(chunk_size=1024)
nodes = splitter.get_nodes_from_documents(documents)
```

打印nodes[0]查看文本内容及关联的元数据信息。

```
print(nodes[0].get_content(metadata_mode=&quot;all&quot;))
```

> pag[e_label: 1

> file_n](https://link.zhihu.com/?target=https%3A//github.com/geekan/MetaGPT.)ame: metagpt.pdf

> file_path: metagpt.pdf

> file_type: application/pdf

> file_size: 16911937

> creation_date: 2024-05-20

> last_modified_date: 2024-04-23

> 

> Preprint

> METAGPT: M ETA PROGRAMMING FOR A

> MULTI -AGENT COLLABORATIVE FRAMEWORK

> Sirui Hong1∗, Mingchen Zhuge2∗, Jonathan Chen1, Xiawu Zheng3, Yuheng Cheng4,

> Ceyao Zhang4,Jinlin Wang1,Zili Wang ,Steven Ka Shing Yau5,Zijuan Lin4,

> Liyang Zhou6,Chenyu Ran1,Lingfeng Xiao1,7,Chenglin Wu1†,J¨urgen Schmidhuber2,8

> 1DeepWisdom,2AI Initiative, King Abdullah University of Science and Technology,

> 3Xiamen University,4The Chinese University of Hong Kong, Shenzhen,

> 5Nanjing University,6University of Pennsylvania,

> 7University of California, Berkeley,8The Swiss AI Lab IDSIA/USI/SUPSI

> ABSTRACT

> Remarkable progress has been made on automated problem solving through so-

> cieties of agents based on large language models (LLMs). Existing LLM-based

> multi-agent systems can already solve simple dialogue tasks. Solutions to more

> complex tasks, however, are complicated through logic inconsistencies due to

> cascading hallucinations caused by naively chaining LLMs. Here we introduce

> MetaGPT, an innovative meta-programming framework incorporating efficient

> human workflows into LLM-based multi-agent collaborations. MetaGPT en-

> codes Standardized Operating Procedures (SOPs) into prompt sequences for more

> streamlined workflows, thus allowing agents with human-like domain expertise

> to verify intermediate results and reduce errors. MetaGPT utilizes an assembly

> line paradigm to assign diverse roles to various agents, efficiently breaking down

> complex tasks into subtasks involving many agents working together. On col-

> laborative software engineering benchmarks, MetaGPT generates more coherent

> solutions than previous chat-based multi-agent systems. Our project can be found

> at https:// github.com/geekan/MetaG PT.

> 1 I NTRODUCTION

> Autonomous agents utilizing Large Language Models (LLMs) offer promising opportunities to en-

> hance and replicate human workflows. In real-world applications, however, existing systems (Park

> et al., 2023; Zhuge et al., 2023; Cai et al., 2023; Wang et al., 2023c; Li et al., 2023; Du et al., 2023;

> Liang et al., 2023; Hao et al., 2023) tend to oversimplify the complexities. They struggle to achieve

> effective, coherent, and accurate problem-solving processes, particularly when there is a need for

> meaningful collaborative interaction (Chen et al., 2024; Zhang et al., 2023; Dong et al., 2023; Zhou

> et al., 2023; Qian et al., 2023).

> Through extensive collaborative practice, humans have developed widely accepted Standardized

> Operating Procedures (SOPs) across various domains (Belbin, 2012; Manifesto, 2001; DeMarco &

> Lister, 2013). These SOPs play a critical role in supporting task decomposition and effective coor-

> dination. Furthermore, SOPs outline the responsibilities of each team member, while establishing

> standards for intermediate outputs. Well-defined SOPs improve the consistent and accurate exe-

> cution of tasks that align with defined roles and quality standards (Belbin, 2012; Manifesto, 2001;

> DeMarco & Lister, 2013; Wooldridge & Jennings, 1998). For instance, in a software company,

> Product Managers analyze competition and user needs to create Product Requirements Documents

> (PRDs) using a standardized structure, to guide the developmental process.

> Inspired by such ideas, we design a promising GPT -based Meta -Programming framework called

> MetaGPT that significantly benefits from SOPs. Unlike other works (Li et al., 2023; Qian et al.,

> 2023), MetaGPT requires agents to generate structured outputs, such as high-quality requirements

> ∗These authors contributed equally to this work.

> †Chenglin Wu (alexanderwu@fuzhi.ai) is the corresponding author, affiliated with DeepWisdom.

> 1

定义向量存储，在节点上创建RAG indexing pipeline。

```
from llama_index.core import VectorStoreIndex

vector_index = VectorStoreIndex(nodes)
query_engine = vector_index.as_query_engine(similarity_top_k=2)
```

与上节不同，这里我们尝试用元数据过滤器查询RAG pipeline。

```
from llama_index.core.vector_stores import MetadataFilters

query_engine = vector_index.as_query_engine(
    similarity_top_k=2,
    filters=MetadataFilters.from_dicts(
        [
            {&quot;key&quot;: &quot;page_label&quot;, &quot;value&quot;: &quot;2&quot;}
        ]
    )
)

response = query_engine.query(
    &quot;What are some high-level results of MetaGPT?&quot;, 
)

print(str(response))
```

> Some high-level results of MetaGPT include achieving a new state-of-the-art in code generation benchmarks with 85.9% and 87.7% in Pass@1, outperforming other popular frameworks in creating complex software projects. Additionally, MetaGPT excels in handling higher levels of software complexity, offering extensive functionality, and demonstrating robustness and efficiency with a 100% task completion rate in experimental evaluations.

打印响应的源节点信息，可以看到MetadataFilters正确过滤了页码，将搜索范围限定在page 2。

```
for n in response.source_nodes:
    print(n.metadata)
```

> {'page_label': '2', 'file_name': 'metagpt.pdf', 'file_path': 'metagpt.pdf', 'file_type': 'application/pdf', 'file_size': 16911937, 'creation_date': '2024-05-20', 'last_modified_date': '2024-04-23'}

### 定义自检索工具

上节我们将检索工具封装在函数内，函数接受query字符串和页码作为过滤器。实际上LLM可以推断页码过滤用户查询，而不是让用户手动指定元数据过滤器。元数据不只限定为页码，也可以是章节ID、header、footer，或者可以通过LlamaIndex抽象的任何字段。

下面定义Python函数接收query和page_numbers，以指定页码为元数据过滤器在index上执行向量搜索。

```
from typing import List
from llama_index.core.vector_stores import FilterCondition


def vector_query(
    query: str, 
    page_numbers: List[str]
) -&gt; str:
    &quot;&quot;&quot;Perform a vector search over an index.
    
    query (str): the string query to be embedded.
    page_numbers (List[str]): Filter by set of pages. Leave BLANK if we want to perform a vector search
        over all pages. Otherwise, filter by the set of specified pages.
    
    &quot;&quot;&quot;

    metadata_dicts = [
        {&quot;key&quot;: &quot;page_label&quot;, &quot;value&quot;: p} for p in page_numbers
    ]
    
    query_engine = vector_index.as_query_engine(
        similarity_top_k=2,
        filters=MetadataFilters.from_dicts(
            metadata_dicts,
            condition=FilterCondition.OR
        )
    )
    response = query_engine.query(query)
    return response
    

vector_query_tool = FunctionTool.from_defaults(
    name=&quot;vector_tool&quot;,
    fn=vector_query
)
```

测试问题

```
llm = OpenAI(model=&quot;gpt-3.5-turbo&quot;, temperature=0)
response = llm.predict_and_call(
    [vector_query_tool], 
    &quot;What are the high-level results of MetaGPT as described on page 2?&quot;, 
    verbose=True
)
```

> === Calling Function ===

> Calling function: vector_tool with args: {"query": "high-level results of MetaGPT", "page_numbers": ["2"]}

> === Function Output ===

> MetaGPT achieves a new state-of-the-art (SoTA) in code generation benchmarks with 85.9% and 87.7% in Pass@1. It stands out in handling higher levels of software complexity and offering extensive functionality. In experimental evaluations, MetaGPT achieves a 100% task completion rate, demonstrating robustness and efficiency in terms of time and token costs.

查看源节点信息，与预期一致，检索内容限定在page 2。

```
for n in response.source_nodes:
    print(n.metadata)
```

> {'page_label': '2', 'file_name': 'metagpt.pdf', 'file_path': 'metagpt.pdf', 'file_type': 'application/pdf', 'file_size': 16911937, 'creation_date': '2024-05-20', 'last_modified_date': '2024-04-23'}

## 增加其他工具

接下来，我们引入router示例中的摘要工具，向量工具结合起来，创建整体的工具选择系统。

```
from llama_index.core import SummaryIndex
from llama_index.core.tools import QueryEngineTool

summary_index = SummaryIndex(nodes)
summary_query_engine = summary_index.as_query_engine(
    response_mode=&quot;tree_summarize&quot;,
    use_async=True,
)
summary_tool = QueryEngineTool.from_defaults(
    name=&quot;summary_tool&quot;,
    query_engine=summary_query_engine,
    description=(
        &quot;Useful if you want to get a summary of MetaGPT&quot;
    ),
)
```

再试一次工具调用，此时LLM的任务稍微困难一些，它不仅需要推断函数参数，还需要选择正确的工具。

```
response = llm.predict_and_call(
    [vector_query_tool, summary_tool], 
    &quot;What are the MetaGPT comparisons with ChatDev described on page 8?&quot;, 
    verbose=True
)
```

> === Calling Function ===

> Calling function: vector_tool with args: {"query": "MetaGPT comparisons with ChatDev", "page_numbers": ["8"]}

> === Function Output ===

> MetaGPT outperforms ChatDev on the SoftwareDev dataset in various metrics such as executability, running times, token usage, code statistic, productivity, and human revision cost. MetaGPT achieves a higher score in executability, takes less time to generate code, uses more tokens but requires fewer tokens to generate one line of code compared to ChatDev. Additionally, MetaGPT shows improved productivity and reduced human revision cost when compared to ChatDev.

查看源节点信息

```
for n in response.source_nodes:
    print(n.metadata)
```

> {'page_label': '8', 'file_name': 'metagpt.pdf', 'file_path': 'metagpt.pdf', 'file_type': 'application/pdf', 'file_size': 16911937, 'creation_date': '2024-05-20', 'last_modified_date': '2024-04-23'}

最后测试摘要问题，返回正确的响应。

```
response = llm.predict_and_call(
    [vector_query_tool, summary_tool], 
    &quot;What is a summary of the paper?&quot;, 
    verbose=True
)
```

> === Calling Function ===

> Calling function: summary_tool with args: {"input": "The paper discusses the impact of climate change on global food security and explores potential solutions to address this issue."}

> === Function Output ===

> The paper does not discuss the impact of climate change on global food security or explore potential solutions to address this issue.