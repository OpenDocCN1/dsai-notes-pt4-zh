# RAG模块

> 来源：[https://deepwisdom.feishu.cn/docx/RM4ld86G7o3upvxraIGcYWq8nAe](https://deepwisdom.feishu.cn/docx/RM4ld86G7o3upvxraIGcYWq8nAe)

## 定义

RAG（Retrieval-Augmented Generation）通过引用外部权威知识库来优化大型语言模型（LLM）的输出，增强其生成响应的能力。这种方法允许LLM在不重新训练的情况下，访问特定领域的知识，提高输出的相关性、准确性和实用性。

本文主要介绍当前MetaGPT所提供的RAG功能：

1.  数据输入，支持多种格式文件（包括pdf/docx/md/csv/txt/ppt）、python对象

1.  检索功能，支持faiss/bm25/chromadb/es，并支持混合检索

1.  检索后处理，支持LLM Rerank/ColbertRerank，对上面检索出来的内容进行重排以得到更准确的数据

1.  数据更新，增加文本与python对象

1.  数据保存及恢复，不用每次都进行向量化

更多的例子请查看 [rag_pipeline](https://github.com/geekan/MetaGPT/blob/main/examples/rag/rag_pipeline.py) 和 [rag_search](https://github.com/geekan/MetaGPT/blob/main/examples/rag/rag_search.py)

## 前置准备

+   安装RAG模块

```
# 从pypi安装
pip install metagpt[rag]
```

```
# 从源码安装
pip install -e .[rag]
```

> 注意点：

1.  有些模块比较大，采用延迟加载，需要自行安装，比如要使用ColbertRerank，需安装llama-index-postprocessor-colbert-rerank。

+   配置embedding

```
# openai
embedding:
  api_type: &quot;openai&quot;
  base_url: &quot;YOU_BASE_URL&quot;
  api_key: &quot;YOU_API_KEY&quot;

# azure
embedding:
  api_type: &quot;azure&quot;
  base_url: &quot;YOU_BASE_URL&quot;
  api_key: &quot;YOU_API_KEY&quot;
  api_version: &quot;YOU_API_VERSION&quot;

# gemini
embedding:
  api_type: &quot;gemini&quot;
  api_key: &quot;YOU_API_KEY&quot;

# ollama
embedding:
  api_type: &quot;ollama&quot;
  base_url: &quot;YOU_BASE_URL&quot;
  model: &quot;YOU_MODEL&quot;
```

> 注意点：

1.  为了向后兼容，如果config不设置embedding，并且llm的api_type类型是openai或azure，那么会使用llm的配置进行embedding。

1.  如果llm是ollama，可能会出现"context size was not non-negative"报错，这时需要在llm里配置max_token，比如2048。

## 数据输入

### 示例 1.1: 文件或目录

```
import asyncio

from metagpt.rag.engines import SimpleEngine
from metagpt.const import EXAMPLE_DATA_PATH

DOC_PATH = EXAMPLE_DATA_PATH / &quot;rag/travel.txt&quot;

async def main():
    engine = SimpleEngine.from_docs(input_files=[DOC_PATH])
    answer = await engine.aquery(&quot;What does Bob like?&quot;)
    print(answer)

if __name__ == &quot;__main__&quot;:
    asyncio.run(main())
```

在这个示例中，我们使用最简配置，输入文件，接收一个问题，并打印出查询结果。

### 示例 1.2: 自定义对象

```
import asyncio

from pydantic import BaseModel

from metagpt.rag.engines import SimpleEngine


class Player(BaseModel):
    name: str
    goal: str

    def rag_key(self):
        return f&quot;{self.name}&#x27;s goal is {self.goal}.&quot;


async def main():
    objs = [Player(name=&quot;Jeff&quot;, goal=&quot;Top One&quot;), Player(name=&quot;Mike&quot;, goal=&quot;Top Three&quot;)]
    engine = SimpleEngine.from_objs(objs=objs)
    answer = await engine.aquery(&quot;What is Jeff&#x27;s goal?&quot;)
    print(answer)

if __name__ == &quot;__main__&quot;:
    asyncio.run(main())
```

在这个示例中，我们使用最简配置，定义Player对象，其中[最重要的是自定义对](https://github.com/geekan/MetaGPT/blob/main/metagpt/rag/interface.py)象需满足接口（具体查看RAGObject）。

## 检索功能

### 示例 2.1: faiss检索

```
import asyncio

from metagpt.rag.engines import SimpleEngine
from metagpt.rag.schema import FAISSRetrieverConfig
from metagpt.const import EXAMPLE_DATA_PATH

DOC_PATH = EXAMPLE_DATA_PATH / &quot;rag/travel.txt&quot;


async def main():
    engine = SimpleEngine.from_docs(input_files=[DOC_PATH], retriever_configs=[FAISSRetrieverConfig()])
    answer = await engine.aquery(&quot;What does Bob like?&quot;)
    print(answer)

if __name__ == &quot;__main__&quot;:
    asyncio.run(main())
```

在这个示例中，我们使用faiss进行检索，其中更多参数可查看FAISSRetrieverConfig。

### 示例 2.2: faiss和bm25混合检索

```
import asyncio

from metagpt.rag.engines import SimpleEngine
from metagpt.rag.schema import FAISSRetrieverConfig, BM25RetrieverConfig
from metagpt.const import EXAMPLE_DATA_PATH

DOC_PATH = EXAMPLE_DATA_PATH / &quot;rag/travel.txt&quot;


async def main():
    engine = SimpleEngine.from_docs(input_files=[DOC_PATH], retriever_configs=[FAISSRetrieverConfig(), BM25RetrieverConfig()])
    answer = await engine.aquery(&quot;What does Bob like?&quot;)
    print(answer)

if __name__ == &quot;__main__&quot;:
    asyncio.run(main())
```

在这个示例中，我们使用faiss和bm25进行混合检索，把两种检索出来的结果去重结合。

## 检索后处理

### 示例 3.1: LLM重排

```
import asyncio

from metagpt.rag.engines import SimpleEngine
from metagpt.rag.schema import FAISSRetrieverConfig, LLMRankerConfig
from metagpt.const import EXAMPLE_DATA_PATH

DOC_PATH = EXAMPLE_DATA_PATH / &quot;rag/travel.txt&quot;


async def main():
    engine = SimpleEngine.from_docs(input_files=[DOC_PATH], retriever_configs=[FAISSRetrieverConfig()], ranker_configs=[LLMRankerConfig()])
    answer = await engine.aquery(&quot;What does Bob like?&quot;)
    print(answer)

if __name__ == &quot;__main__&quot;:
    asyncio.run(main())
```

在这个示例中，我们先使用faiss进行检索，然后对检索出来的结果再用LLMRanker进行重排，得到最后检索的结果。

## 数据更新

### 示例 4.1: 增加文本与python对象

```
import asyncio

from pydantic import BaseModel
from metagpt.rag.engines import SimpleEngine
from metagpt.rag.schema import FAISSRetrieverConfig
from metagpt.const import EXAMPLE_DATA_PATH

DOC_PATH = EXAMPLE_DATA_PATH / &quot;rag/travel.txt&quot;


class Player(BaseModel):
    name: str
    goal: str

    def rag_key(self):
        return f&quot;{self.name}&#x27;s goal is {self.goal}&quot;


async def main():
    engine = SimpleEngine.from_objs(retriever_configs=[FAISSRetrieverConfig()])

    engine.add_docs([DOC_PATH])
    answer = await engine.aquery(&quot;What does Bob like?&quot;)
    print(answer)

    engine.add_objs([Player(name=&quot;Jeff&quot;, goal=&quot;Top One&quot;)])
    answer = await engine.aquery(&quot;What is Jeff&#x27;s goal?&quot;)
    print(answer)


if __name__ == &quot;__main__&quot;:
    asyncio.run(main())
```

在这个示例中，我们创建engine后，可以添加文档或者对象，最重要的是，如果自定义retriever需要实现接口ModifiableRAGRetriever。

## 数据保存及恢复

### 示例 5.1

```
import asyncio

from metagpt.rag.engines import SimpleEngine
from metagpt.rag.schema import FAISSRetrieverConfig, FAISSIndexConfig
from metagpt.const import EXAMPLE_DATA_PATH

DOC_PATH = EXAMPLE_DATA_PATH / &quot;rag/travel.txt&quot;


async def main():
    persist_dir = &quot;./tmp_storage&quot;
    retriever_configs = [FAISSRetrieverConfig()]

    # 1. save index
    SimpleEngine.from_docs(input_files=[DOC_PATH], retriever_configs=retriever_configs).persist(persist_dir)

    # 2. load index
    engine = SimpleEngine.from_index(index_config=FAISSIndexConfig(persist_path=persist_dir), retriever_configs=retriever_configs)

    # 3. query
    answer = await engine.aquery(&quot;What does Bob like?&quot;)
    print(answer)

if __name__ == &quot;__main__&quot;:
    asyncio.run(main())
```

在这个示例中，我们先把向量化相关数据保存在persist_dir，然后从persist_dir进行恢复后查询。