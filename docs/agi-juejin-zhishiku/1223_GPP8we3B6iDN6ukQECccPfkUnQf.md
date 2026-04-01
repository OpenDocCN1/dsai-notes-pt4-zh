# LlamaIndex重大发布：让你在任何LLM-Powered的 ETL、RAG 、agent管道中获得结构化抽取能力

> 来源：[https://agijuejin.feishu.cn/docx/XnmDd3J9aoRGaYxRttHccagQn5e](https://agijuejin.feishu.cn/docx/XnmDd3J9aoRGaYxRttHccagQn5e)

> 作者：lucas大叔

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/710855495)hihu.com/p/710855...

只需定义 Pydantic 对象，将其[附加到 L](https://www.zhihu.com/search?q=%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710855495%7D)LM（ as_structured_llm(...) ），就能使你通过块级 RAG 或文档级 RAG 进行结构化抽取，支持异步和 流式输出 。

下面让我们一起来探索llamaindex这项全新能力。

## 环境设置

这里演示使用的是gpt，也可以通过HuggingFaceLLM加载本地模型。

```
import nest_asyncio

nest_asyncio.apply()

from llama_index.llms.openai import OpenAI
from llama_index.embeddings.openai import OpenAIEmbedding
from llama_index.core import Settings

llm = OpenAI(model=&quot;gpt-4o&quot;)
embed_model = OpenAIEmbedding(model=&quot;text-embedding-3-small&quot;)
Settings.llm = llm
Settings.embed_model = embed_model
```

## 简单结构化抽取

你可以通过 as_structured_[llm ](https://www.zhihu.com/search?q=%E7%BB%93%E6%9E%84%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710855495%7D)将任何 LLM 转换为“ 结构化 LLM”，方法是为其附加一个输出类。

本示例中，传递一个简单的 Album 类，其中包含歌曲列表。然后，我们可以使用像 chat/complete 这样的常规 LLM 端点。

```
from typing import List
from pydantic.v1 import BaseModel, Field


class Song(BaseModel):
    &quot;&quot;&quot;Data model for a song.&quot;&quot;&quot;

    title: str
    length_seconds: int


class Album(BaseModel):
    &quot;&quot;&quot;Data model for an album.&quot;&quot;&quot;

    name: str
    artist: str
    songs: List[Song]


from llama_index.core.llms import ChatMessage

sllm = llm.as_structured_llm(output_cls=Album)
input_msg = ChatMessage.from_str(&quot;Generate an example album from The Shining&quot;)
```

### sync

```
output = sllm.chat([input_msg])
# get actual object
output_obj = output.raw

print(str(output))
```

> assistant: {"name": "The Shining: Original Soundtrack", "artist": "Various Artists", "songs": [{"title": "Main Title", "length_seconds": 180}, {"title": "Rocky Mountains", "length_seconds": 210}, {"title": "Lontano", "length_seconds": 720}, {"title": "Music for Strings, Percussion and Celesta", "length_seconds": 540}, {"title": "Utrenja (Excerpt)", "length_seconds": 300}, {"title": "The Awakening of Jacob", "length_seconds": 480}, {"title": "De Natura Sonoris No. 2", "length_seconds": 540}, {"title": "Home", "length_seconds": 180}, {"title": "Midnight, the Stars and You", "length_seconds": 180}, {"title": "It's All Forgotten Now", "length_seconds": 150}, {"title": "Masquerade", "length_seconds": 180}]}

```
print(output_obj)
```

> name='The Shining: Original Soundtrack' artist='Various Artists' songs=[Song[(title='Main](https://www.zhihu.com/search?q=seconds%3D210&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710855495%7D) Title', length_seconds=180), Song(title='Rocky Mountains', length_ seconds=210 ), Song(title='Lontano', length_seconds=720 ), Song(title='Music for Strings, Percussion and Celesta', length_seconds=540 ), Song(title='Utrenja (Excerpt)', length_seconds=300 ), Song(title='The Awakening of Jacob', length_seconds=480 ), Song(title='De Natura Sonoris No. 2', length_seconds=540), Song(title='Home', length_seconds=180 ), Song(title='Midnight, the Stars and You', length_seconds=180), Song(title="It's All Forgotten Now", length_seconds=150), Song(title='Masquerade', length_seconds=180)]

### async

```
output = await sllm.achat([input_msg])
# get actual object
output_obj = output.raw
print(str(output))
```

> assistant: {"name": "The Shining: Original Soundtrack", "artist": "Various Artists", "songs": [{"title": "Main Title (The Shining)", "length_seconds": 180}, {"title": "Rocky Mountains", "length_seconds": 210}, {"title": "Lontano", "length_seconds": 240}, {"title": "Music for Strings, Percussion and Celesta", "length_seconds": 300}, {"title": "Utrenja (Excerpt)", "length_seconds": 180}, {"title": "The Awakening of Jacob", "length_seconds": 150}, {"title": "De Natura Sonoris No. 2", "length_seconds": 270}, {"title": "Home", "length_seconds": 200}, {"title": "Heartbeats and Worry", "length_seconds": 160}, {"title": "The Overlook", "length_seconds": 220}]}

### streaming

```
from IPython.display import clear_output
from pprint import pprint

stream_output = sllm.stream_chat([input_msg])
for partial_output in stream_output:
    clear_output(wait=True)
    pprint(partial_output.raw.dict())

output_obj = partial_output.raw
print(str(output))
```

> {'artist': 'Various Artists',

> 'name': 'The Shining: Original Soundtrack',

> 'songs': [{'length_seconds': 180, 'title': 'Main Title'},

> {'length_seconds': 210, 'title': 'Rocky Mountains'},

> {'length_seconds': 240, 'title': 'Lontano'},

> {'length_seconds': 540,

> 'title': 'Music for Strings, Percussion and Celesta'},

> {'length_seconds': 300, 'title': 'Utrenja (Excerpt)'},

> {'length_seconds': 360, 'title': 'The Awakening of Jacob'},

> {'length_seconds': 420, 'title': 'De Natura Sonoris No. 2'},

> {'length_seconds': 180, 'title': 'Home'},

> {'length_seconds': 180, 'title': 'Midnight, the Stars and You'},

> {'length_seconds': 150, 'title': "It's All Forgotten Now"},

> {'length_seconds': 120, 'title': 'Masquerade'}]}

> assistant: {"name": "The Shining: Original Soundtrack", "artist": "Various Artists", "songs": [{"title": "Main Title (The Shining)", "length_seconds": 180}, {"title": "Rocky Mountains", "length_seconds": 210}, {"title": "Lontano", "length_seconds": 240}, {"title": "Music for Strings, Percussion and Celesta", "length_seconds": 300}, {"title": "Utrenja (Excerpt)", "length_seconds": 180}, {"title": "The Awakening of Jacob", "length_seconds": 150}, {"title": "De Natura Sonoris No. 2", "length_seconds": 270}, {"title": "Home", "length_seconds": 200}, {"title": "Heartbeats and Worry", "length_seconds": 160}, {"title": "The Overlook", "length_seconds": 220}]}

### async streaming

```
from IPython.display import clear_output
from pprint import pprint

stream_output = await sllm.astream_chat([input_msg])
async for partial_output in stream_output:
    clear_output(wait=True)
    pprint(partial_output.raw.dict())
```

> {'artist': 'Various Artists',

> 'name': 'The Shining: Original Soundtrack',

> 'songs': [{'length_seconds': 180, 'title': 'Main Title'},

> {'length_seconds': 210, 'title': 'Rocky Mountains'},

> {'length_seconds': 720, 'title': 'Lontano'},

> {'length_seconds': 540,

> 'title': 'Music for Strings, Percussion and Celesta'},

> {'length_seconds': 300, 'title': 'Utrenja (Excerpt)'},

> {'length_seconds': 480, 'title': 'The Awakening of Jacob'},

> {'length_seconds': 540, 'title': 'De Natura Sonoris No. 2'},

> {'length_seconds': 180, 'title': 'Home'},

> {'length_seconds': 180, 'title': 'Midnight, the Stars and You'},

> {'length_seconds': 180, 'title': "It's All Forgotten Now"},

> {'length_seconds': 180, 'title': 'Masquerade'}]}

### 使用查询管道的示例

你可以在查询管道中插入结构化的LLM - 输出将直接是结构化对象。

```
# use query pipelines
from llama_index.core.prompts import ChatPromptTemplate
from llama_index.core.query_pipeline import QueryPipeline as QP
from llama_index.core.llms import ChatMessage

chat_prompt_tmpl = ChatPromptTemplate(
    message_templates=[
        ChatMessage.from_str(
            &quot;Generate an example album from {movie_name}&quot;, role=&quot;user&quot;
        )
    ]
)

qp = QP(chain=[chat_prompt_tmpl, sllm])
response = qp.run(movie_name=&quot;Inside Out&quot;)
response
```

> Album(name='Inside Out Soundtrack', artist='Various Artists', songs=[Song(title='Bundle of Joy', length_seconds=150), Song(title='Team Building', length_seconds=120 ), Song(title='Nomanisone Island/National Movers', length_seconds=180), Song(title='Overcoming Sadness', length_seconds=210), Song(title='Free Skating', length_seconds=160), Song(title='First Day of School', length_seconds=140), Song(title='Riled Up', length_seconds=130), Song(title='Goofball No Longer', length_seconds=170), Song(title='Memory Lanes', length_seconds=200), Song(title='The Forgetters', length_seconds=110)])

### 使用 structured_predict 函数

与其明确地执行 llm.as_structured_llm(...) ，每个 LLM 类都有 structured_predict 函数，该函数允许你更方便地使用提示模板 + 模板变量调用 LLM，以便在一行代码中返回结构化输出。

```
# use query pipelines
from llama_index.core.prompts import ChatPromptTemplate
from llama_index.core.llms import ChatMessage
from llama_index.llms.openai import OpenAI

chat_prompt_tmpl = ChatPromptTemplate(
    message_templates=[
        ChatMessage.from_str(
            &quot;Generate an example album from {movie_name}&quot;, role=&quot;user&quot;
        )
    ]
)

llm = OpenAI(model=&quot;gpt-4o&quot;)
album = llm.structured_predict(
    Album, chat_prompt_tmpl, movie_name=&quot;Lord of the Rings&quot;
)
album
```

> Album(name='Songs of Middle-earth', artist='Various Artists', songs=[Song(title='The Shire', length_seconds=180), Song(title='The Fellowship', length_seconds=240), Song(title="Gollum's Theme", length_seconds=200 ), Song(title="Rohan's Call", length_seconds=220), Song(title="The Battle of Helm's Deep", length_seconds=300), Song(title='Lothlórien', length_seconds=210), Song(title='The Return of the King', length_seconds=250), Song(title='Into the West', length_seconds=260)])

## 接入 RAG 管道

你也可以将其插入到 RAG 管道中。下面我们展示从Apple 10K 报告中进行结构化抽取。

```
!mkdir data
!wget &quot;https://s2.q4cdn.com/470004039/files/doc_financials/2021/q4/_10-K-2021-(As-Filed).pdf&quot; -O data/apple_2021_10k.pdf
```

### 选项 1：使用 LlamaParse

你需要在 https:// cloud.llamaindex.ai/  拥有账户以及 API 密钥才能使用 LlamaParse，这是我们用于 10K 文件的文档解析器。

```
from llama_parse import LlamaParse

# os.environ[&quot;LLAMA_CLOUD_API_KEY&quot;] = &quot;llx-...&quot;
orig_docs = LlamaParse(result_type=&quot;text&quot;).load_data(
    &quot;./data/apple_2021_10k.pdf&quot;
)

from copy import deepcopy
from llama_index.core.schema import TextNode


def get_page_nodes(docs, separator=&quot;\n---\n&quot;):
    &quot;&quot;&quot;Split each document into page node, by separator.&quot;&quot;&quot;
    nodes = []
    for doc in docs:
        doc_chunks = doc.text.split(separator)
        for doc_chunk in doc_chunks:
            node = TextNode(
                text=doc_chunk,
                metadata=deepcopy(doc.metadata),
            )
            nodes.append(node)

    return nodes


docs = get_page_nodes(orig_docs)
print(docs[0].get_content())
```

### 选项 2：使用 SimpleDirectoryReader

你也可以选择使用捆绑到SimpleDirectoryReader的免费PDF解析器。

```
# # OPTION 2: Use SimpleDirectoryReader
# from llama_index.core import SimpleDirectoryReader

# reader = SimpleDirectoryReader(input_files=[&quot;apple_2021_10k.pdf&quot;])
# docs = reader.load_data()
```

### 构建 RAG 管道，定义结构化输出schema

我们使用可靠的 VectorStoreIndex 和 reranker 模块构建了 RAG 管道。然后，将输出定义为 Pydantic 模型。这使我们能够创建带有附加输出类的结构化 LLM。

```
from llama_index.core import VectorStoreIndex

# skip chunking since we&#x27;re doing page-level chunking
index = VectorStoreIndex(docs)
```

加载reranker

```
from llama_index.postprocessor.flag_embedding_reranker import (
    FlagEmbeddingReranker,
)

reranker = FlagEmbeddingReranker(
    top_n=5,
    model=&quot;BAAI/bge-reranker-large&quot;,
)
```

定义输出schema，附加到LLM

```
fro[m pydantic](https://www.zhihu.com/search?q=irrelevant&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710855495%7D).v1 import BaseModel, Field
from typing import List


class Output(BaseModel):
    &quot;&quot;&quot;Output containing the response, page numbers, and confidence.&quot;&quot;&quot;

    response: str = Field(..., description=&quot;The answer to the question.&quot;)
    page_numbers: List[int] = Field(
        ...,
        description=&quot;The page numbers of the sources used to answer this question. Do not include a page number if the context is irrelevant.&quot;,
    )
    confidence: float = Field(
        ...,
        description=&quot;Confidence value between 0-1 of the correctness of the result.&quot;,
    )
    confidence_explanation: str = Field(
        ..., description=&quot;Explanation for the confidence score&quot;
    )


sllm = llm.as_structured_llm(output_cls=Output)
```

### 运行查询

```
[query_engine](https://www.zhihu.com/search?q=query_engine&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710855495%7D)[ = index.as_qu](https://www.zhihu.com/search?q=tree_summarize&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710855495%7D)ery_engine(
    similarity_top_k=5,
    node_postprocessors=[reranker],
    llm=sllm,
    response_mode=&quot;tree_summarize&quot;,  # you can also select other modes like `compact`, `refine`
)

response = query_engine.query(&quot;Net sales for each product category in 2021&quot;)
print(str(response))
```

> {"response": "In 2021, the net sales for each product category were as follows: iPhone: $191,973 million, Mac: $35,190 million, iPad: $31,862 million, Wearables, Home and Accessories: $38,367 million, and Services: $68,425 million.", "page_numbers": [21], "confidence": 1.0, "confidence_explanation": "The figures are directly taken from the provided data, ensuring high accuracy."}

```
response.response.dict()
```

> {'r[esponse'](https://www.zhihu.com/search?q=million&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710855495%7D)[: 'In 20](https://www.zhihu.com/search?q=figures&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710855495%7D)[21, the n](https://www.zhihu.com/search?q=provided&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A710855495%7D)et sales for each product category were as follows: iPhone: $191,973 million, Mac: $35,190 million, iPad: $31,862 million , Wearables, Home and Accessories: $38,367 million, and Services: $68,425 million.',

> 'page_numbers': [21],

> 'confidence': 1.0,

> 'confidence_explanation': 'The figures are directly taken from the provided data, ensuring high accuracy.'}

## 参考文档

[https:// github.com/run-llama/ll ama_index/blob/main/doc](https://link.zhihu.com/?target=https%3A//github.com/run-llama/llama_index/blob/main/docs/docs/examples/structured_outputs/structured_outputs.ipynb)s/docs/examples/structured_outputs/structured_outputs.ipynb