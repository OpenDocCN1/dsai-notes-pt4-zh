
# 用llama3打造光速响应的RAG Chatbot

> 来源：[https://agijuejin.feishu.cn/docx/XgfidQ4dfoiA2Mx5wknc5Rsjnme](https://agijuejin.feishu.cn/docx/XgfidQ4dfoiA2Mx5wknc5Rsjnme)

> 作者：lucas大叔

> 原文：[https://zhuanlan.zhihu.com/p/700525800](https://zhuanlan.zhihu.com/p/700525800)

本示例不仅展示如何构建高级的RAG 索引/查询 pipeline，而且还传授如何将其转化为具有快速响应的 全栈 应用。

1. 通过句子拆分、标题提取和Gemini embedding定义索引 pipeline
2. 定义以Groq为LLM和聊天引擎查询的 pipeline 维护对话历史
3. 使用`chainlit`构建整洁的全栈应用

---

## 准备工作

![](img/17111eea46981042f21e1290f667bff4.png)

现在我们开始研究如何基于LlamaIndex实现RAG，首先安装 `llama-index`：

```bash
pip install llama-index
```

安装完成后导入必要的包

```python
from llama_index.core import (
    VectorStoreIndex,
    SimpleDirectoryReader,
    StorageContext,
    ServiceContext,
    load_index_from_storage
)
from llama_index.core.node_parser import SemanticSplitterNodeParser
from llama_index.embeddings.gemini import GeminiEmbedding
from llama_index.llms.groq import Groq
```

## 数据摄入

构建RAG pipeline的第一步是加载数据，可以是PDF、文本、代码或Markdown文件，SQL数据库，或者我们希望加载的任何其他格式。LlamaIndex提供了一系列的数据加载器，无论是本地文件存储还是Google Drive或MongoDB等外部存储。

```python
reader = SimpleDirectoryReader(input_dir="path/to/directory")
documents = reader.load_data()
```

`SimpleDirectoryReader` 模块允许从本地系统加载跨格式的文件/文件夹。

参数：

- `input_dir` — 定义包含待加载文件的本地目录路径。如果要从子目录中读取文件，请设置 `recursive=True`。
- `input_files` — 定义待加载的文件列表。

```python
SimpleDirectoryReader(input_files=["path/to/file1", "path/to/file2"])
```

- `exclude` — 定义从目录中排除的文件列表。

```python
SimpleDirectoryReader(
    input_dir="path/to/directory", exclude=["path/to/file1", "path/to/file2"]
)
```

- `required_exts` — 定义指定目录中包含的文件格式扩展名列表。

```python
SimpleDirectoryReader(
    input_dir="path/to/directory", required_exts=[".pdf", ".docx"]
)
```

`show_progress=True` 显示在运行序列中正在处理的文件数量。

对于多进程：

```python
documents = reader.load_data(num_workers=4)
```

## 向量嵌入

嵌入是将原始数据转换为向量表示的数学映射，向量捕捉了数据的基本特征和关系。向量表示，也称为 embedding 向量，是低维空间中紧凑且富有表现力的复杂数据的表示方式。

词嵌入：将词语表示为高维空间中的向量，捕捉它们的语义和关系。例如，Word2Vec和GloVe。

![](img/169d7a402d8e86e620bb1fff82c69512.png)

Embedding的特性：

- 分布式表示：Embedding将数据表示为高维空间的密集向量，从而实现高效的计算和比较。
- 富含语义：Embedding捕捉数据点之间的语义和关系，使得诸如聚类、分类和检索等任务成为可能。
- 低维表示：Embedding减少了数据的维度，使其更容易处理和分析。
- 对噪声的鲁棒性：Embedding对数据中的噪声和变化具有鲁棒性，从而实现更准确的预测和决策。

LlamaIndex提供了一系列的开源和闭源embedding模型可供选择。

```bash
pip install llama-index-embeddings-huggingface llama-index-embeddings-gemini
```

使用`HuggingFaceEmbedding`

```python
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
embed_model = HuggingFaceEmbedding(
    model_name="BAAI/bge-small-en-v1.5"
)
```

使用`GeminiEmbedding`

```bash
pip install google-generativeai>=0.3.0 llama-index-embeddings-gemini
```

```python
from llama_index.embeddings.gemini import GeminiEmbedding
embed_model = GeminiEmbedding(model_name="models/embedding-001")
```

对于本示例的RAG pipeline，使用Google Gemini embedding，维度为768。`model_name`默认为 'models/embedding-001'

## 分块

分块是涉及将信息分解为更小、更易处理单元（称为“块”）的认知过程，分块促进了学习、记忆和检索。这个过程对于高效的信息处理至关重要，因为它有助于减少认知负荷并提高理解能力。

![](img/720ba0aa85c982e7f00a686c334bb40e.png)

分块的类型：

- 字符分割 — 简单的静态字符数据块
- 递归字符文本分割 — 基于分隔符列表的递归分块
- 特定文档分割 — 不同文档类型（PDF、Python、Markdown）的各种分块方法
- 语义分割 — 基于嵌入行走的分块

```python
splitter = SemanticSplitterNodeParser(
              buffer_size=1,
              breakpoint_percentile_threshold=95,
              embed_model=embed_model
           )

nodes = splitter.get_nodes_from_documents(documents, show_progress=True)
```

与固定块大小文本分块不同，语义分割器通过embedding相似度自适应地选择句子之间的断点。这确保“块”包含了在语义上相关的句子。

## 转换与摄入pipeline

数据加载完成后，需要对数据进行处理和转换，然后放入到存储系统中。转换包括分块、提取元数据以及对每个块进行embedding。这么做可以确保数据被LLM最优地检索和使用。

转换的输入/输出是`Node`对象（文档是`Node`的子类），转换也可以被堆叠和重新排序。

```python
from llama_index.core import Document
from llama_index.embeddings.gemini import GeminiEmbedding
from llama_index.core.node_parser import SentenceSplitter
from llama_index.core.extractors import TitleExtractor
from llama_index.core.ingestion import IngestionPipeline, IngestionCache

# create the pipeline with transformations
pipeline = IngestionPipeline(
    transformations=[
        SentenceSplitter(chunk_size=25, chunk_overlap=0),
        TitleExtractor(),
        GeminiEmbedding(),
    ]
)

# run the pipeline
nodes = pipeline.run(documents=[Document.example()])
```

摄入pipeline 使用转换的概念，这些转换被应用于输入数据，生成的节点要么被返回，要么被插入到向量数据库中（如果指定）。每个节点+转换对都被缓存，以便使用相同的节点+转换组合进行的后续运行（如果缓存被持久化）可以使用缓存结果，从而节省时间。

## 设置

LlamaIndex的设置模块帮助配置可在整个pipeline中使用的全局设置。之前可以通过 `ServiceContext` 来实现相同的功能。

```python
from llama_index.llms.groq import Groq
from llama_index.embeddings.gemini import GeminiEmbedding
from llama_index.core.node_parser import SentenceSplitter
from llama_index.core import Settings

Settings.llm = Groq(model="llama3-70b-8192")
Settings.embed_model = GeminiEmbedding(model_name="models/embedding-001")
Settings.text_splitter = SentenceSplitter(chunk_size=1024)
Settings.chunk_size = 512
Settings.chunk_overlap = 20
Settings.transformations = [SentenceSplitter(chunk_size=1024)]
# maximum input size to the LLM
Settings.context_window = 4096

# number of tokens reserved for text generation.
Settings.num_output = 256
```

## 向量存储索引

在数据加载、分块和embedding之后，就可以把数据存储到向量数据库了。在本示例中，我们将使用LlamaIndex内存数据库中的向量存储索引。当使用 `StorageContext` 模块从主存储到二级存储持久化，将创建一堆包含向量embedding块的JSON文件。

![](img/e410df0f99042607e006bc2124c998e3.png)

向量存储接受`Node`对象列表，并从这些`Node`对象构建索引。

```python
vector_index = VectorStoreIndex.from_documents(
                  documents, show_progress=True,
                  service_context=service_context,
                  node_parser=nodes
               )

vector_index.storage_context.persist(persist_dir="./storage")

storage_context = StorageContext.from_defaults(persist_dir="./storage")

index = load_index_from_storage(
            storage_context,
            service_context=service_context
        )
```

在创建存储和持久化后，我们可以在未来的调用中使用它，使用`load_index_from_storage()`方法，该方法接受service context对象和storage context对象。

## 使用Groq的开源LLM

Groq LPU（Language Processing Engine，语言处理引擎）正在颠覆开源LLM领域。它具有确定性、单核流式架构，为GenAI推理速度树立了标准，并且对于任何给定的工作负载都具有可预测和可重复的性能。

```bash
pip install llama-index-llms-groq
llm = Groq(model="llama3-70b-8192", api_key=GROQ_API_KEY)
```



# GroqCloud API 与 Llama3 RAG 聊天机器人构建

GroqCloud API 使得任何开发者都能轻松接入 LLM，无需在本地进行托管，直接获得迄今为止记录的最低延迟率的强大功能！

登录 GroqCloud 账户并开始创建 API 密钥，请访问 [https://console.groq.com/keys](https://console.groq.com/keys)。

Meta 开发并发布了 Meta Llama 3 系列的 LLM，这是一套预训练和指令微调的生成式文本模型，包括 8B 和 70B 两种尺寸。Llama 3 指令微调的模型针对对话使用场景进行了优化，在常见的行业基准测试中表现优异，超过了许多可用的开源聊天模型。

## 查询/聊天引擎

现在可以引用上述创建的索引变量 `index` 来查询用户问题，并使用 `as_query_engine()` 方法构建问答系统。同样，可以使用相同的参数将其转换为具有上下文和会话聊天历史记录的聊天机器人，方法是使用 `as_chat_engine()` 函数创建 `chat_engine`，并且查询方式变为 `chat_engine.chat(query)`。我们将在下一部分中看到如何构建最终的聊天机器人。

```
query_engine = index.as_query_engine(
                  service_context=service_context,
                  similarity_top_k=10,
                )

query = "What is difference between double top and double bottom pattern?"
resp = query_engine.query(query)

print(resp.response)
# A double top pattern is a bearish technical reversal pattern that forms after an asset reaches a high price twice with a moderate decline between the two highs, and is confirmed when the asset's price falls below a support level equal to the low between the two prior highs. On the other hand, a double bottom pattern is a bullish reversal pattern that occurs at the bottom of a downtrend, signaling that the sellers are losing momentum and resembles the letter "W" due to the two-touched low and a change in the trend direction from a downtrend to an uptrend. In summary, the key difference lies in the direction of the trend change and the shape of the pattern.
```

上述答案来自于文档 `All Chart Patterns.pdf` 文件，该文件具有与股市相关的不同 K 线图的上下文展示。

## Chainlit

![](img/4ffc92060f17c49aa31241d89d1e188f.png)

现在 [RAG pipeline](https://www.zhihu.com/search?q=Chainlit%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A700525800%7D) 已经构建完成，可以以聊天机器人的方式为最终用户提供 SaaS 模型服务。为了构建 SaaS 模型，我们将使用 `Chainlit` 库，该库是基于 `Streamlit` 构建的。`Chainlit` 的用户界面类似于 ChatGPT。

```
from llama_index.core import StorageContext, ServiceContext, load_index_from_storage
from llama_index.core.callbacks.base import CallbackManager
from llama_index.embeddings.gemini import GeminiEmbedding
from llama_index.llms.groq import Groq
import os
from dotenv import load_dotenv
load_dotenv()
import chainlit as cl

GOOGLE_API_KEY = os.getenv("GEMINI_API_KEY")
GROQ_API_KEY = os.getenv("GROQ_API_KEY")

@cl.on_chat_start
async def factory():
    storage_context = StorageContext.from_defaults(persist_dir="./storage")

    embed_model = GeminiEmbedding(
        model_name="models/embedding-001", api_key=GOOGLE_API_KEY
    ) 

    llm = Groq(model="llama3-70b-8192", api_key=GROQ_API_KEY)

    service_context = ServiceContext.from_defaults(
                        embed_model=embed_model, llm=llm,
                        callback_manager=
                        CallbackManager([cl.LlamaIndexCallbackHandler()]),
    )

    index = load_index_from_storage(
                storage_context, 
                service_context=service_context
    )

    chat_engine = index.as_chat_engine(
        service_context=service_context,
        similarity_top_k=10
    )

    cl.user_session.set("chat_engine", chat_engine)

@cl.on_message
async def main(message: cl.Message):
    chat_engine = cl.user_session.get("chat_engine")  
    response = await cl.make_async(chat_engine.query)(message.content)

    response_message = cl.Message(content="")

    for token in response.response:
        await response_message.stream_token(token=token)

    await response_message.send()
```

运行 `pip install chainlit` 创建上述文件。应用中附带了默认的 `config.toml` 文件（位于 `.chainlit` 文件夹下），可以配置自定义的 HTML、CSS 和 JS 代码，更改应用的外观，以及其他与应用相关的设置。

为了从终端运行上述代码，请将其保存到 `app.py` 文件中，并运行命令 `chainlit run app.py`。

Tips：完整代码参阅 [这里](https://link.zhihu.com/?target=https%3A//github.com/jayita13/GenerativeAI/blob/main/llamaindex/rag.ipynb)。

## 参考文档

[构建基于 LlamaIndex、Groq 与 Llama3 的 RAG 聊天机器人（含 Chainlit）](https://link.zhihu.com/?target=https%3A//blog.gopenai.com/building-a-rag-chatbot-using-llamaindex-groq-with-llama3-chainlit-b1709f770f55)
