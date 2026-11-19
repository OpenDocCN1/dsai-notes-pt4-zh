
# 【RAG实践】Rerank，让大模型 RAG 更近一步

> 来源：[https://v11enp9ok1h.feishu.cn/docx/ML7RdmA1OoT8m3x725mcP83bnpc](https://v11enp9ok1h.feishu.cn/docx/ML7RdmA1OoT8m3x725mcP83bnpc)

## RAG+Rerank原理

上一篇[【RAG实践】基于`LlamaIndex`和`Qwen1.5`搭建基于本地知识库的问答机器人](https://v11enp9ok1h.feishu.cn/wiki/UCt8waVUjiEclSkWgIocpUf5ndv?from=from_copylink)

> 我们介绍了什么是RAG，以及如何基于`LLaMaIndex`和`Qwen1.5`搭建基于本地知识库的问答机器人，原理图和步骤如下：

![](img/403f92bf3b67992773e00fd70b402357.png)

这里面主要包括包括三个基本步骤：

1. 索引 — 将文档库分割成较短的`Chunk`，并通过编码器构建向量索引。

1. 检索 — 根据问题和`chunks`的相似度检索相关文档片段。

1. 生成 — 以检索到的上下文为条件，生成问题的回答。

更加完善的RAG技术还包含了很多点，如下是RAG技术的大图

![](img/31d8c8d1c6fe2ec809dbc844035a896f.png)

本文主要关注在Rerank，本文中，Rerank可以在不牺牲准确性的情况下加速LLM的查询（实际上可能提高准确率），Rerank通过从上下文中删除不相关的节点，重新排序相关节点来实现这一点。

### 本文Rerank+RAG原理图

![](img/79f8dc853fb87905472dd6db842b1270.png)

## 技术交流&资料

技术要学会分享、交流，不建议闭门造车。一个人可以走的很快、一堆人可以走的更远。

成立了大模型面试和技术交流群，相关资料、技术交流&答疑，均可加我们的交流群获取，方便找到志同道合的朋友。

`Rerank`模型：`bge-reranker-v2-m3`

与`embedding`模型不同，`Reranker`使用问题和文档作为输入，直接输出相似度而不是`embedding`。本文通过向`Reranker`输入查询和段落来获得相关性分数。`Reranker`是基于交叉熵损失进行优化的，因此相关性得分不受特定范围的限制。`bge-reranker-v2-m3`适用于中英文双语`Rerank`场景。

`LLM`模型：通义千问`1.5`

`Qwen1.5`版本年前开源了包括`0.5B`、`1.8B`、`4B`、`7B`、`14B`、`32B`、`72B`和`A2.7B-MoE`在内的8种大小的基础和聊天模型，同时，也开源了量化模型。不仅提供了`Int4`和`Int8`的`GPTQ`模型，还有`AWQ`模型，以及`GGUF`量化模型。为了提升开发者体验，`Qwen1.5`的代码合并到`Hugging Face Transformers`中，开发者现在可以直接使用`transformers>=4.37.0`而无需`trust_remote_code`。

`RAG`框架：`LLaMaIndex`

`LlamaIndex` 是一个基于 `LLM` 的应用程序的数据框架，受益于上下文增强。 这种`LLM`系统被称为`RAG`系统，代表“检索增强生成”。`LlamaIndex` 提供了必要的抽象，可以更轻松地摄取、构建和访问私有或特定领域的数据，以便将这些数据安全可靠地注入 `LLM` 中，以实现更准确的文本生成。

![](img/11ce40478c273aec0a3fbdf92bc64171.png)

`Embedding`模型：`GTE`文本向量

文本表示是自然语言处理(`NLP`)领域的核心问题, 其在很多`NLP`、信息检索的下游任务中发挥着非常重要的作用。近几年, 随着深度学习的发展，尤其是预训练语言模型的出现极大的推动了文本表示技术的效果, 基于预训练语言模型的文本表示模型在学术研究数据、工业实际应用中都明显优于传统的基于统计模型或者浅层神经网络的文本表示模型。`GTE`主要关注基于预训练语言模型的文本表示。

![](img/e7b348588c60a48f018e52767f2e9e29.png)

`GTE-zh`模型使用`retromae`初始化训练模型，之后利用两阶段训练方法训练模型：第一阶段利用大规模弱弱监督文本对数据训练模型，第二阶段利用高质量精标文本对数据以及挖掘的难负样本数据训练模型。

## 环境配置与安装

1. `python` 3.10及以上版本

1. `pytorch` 1.12及以上版本，推荐`2.0`及以上版本

1. 建议使用`CUDA` 11.4及以上

### 安装依赖库

```
!pip install transformers -U
!pip install llama-index llama-index-llms-huggingface ipywidgets
!pip install sentence-transformers
```

```
import logging
import sys
from abc import ABC
from typing import Any, List

import pandas as pd
import torch
from IPython.display import display, HTML
from llama_index.core import QueryBundle
from llama_index.core import (
    SimpleDirectoryReader,
    VectorStoreIndex,
    Settings,
    ServiceContext,
    set_global_service_context,
)
from llama_index.core.base.embeddings.base import BaseEmbedding, Embedding
from llama_index.core.postprocessor import SentenceTransformerRerank
from llama_index.core.prompts import PromptTemplate
from llama_index.core.retrievers import VectorIndexRetriever
from llama_index.llms.huggingface import HuggingFaceLLM
from modelscope import snapshot_download
from transformers import AutoModelForSequenceClassification, AutoTokenizer

logging.basicConfig(stream=sys.stdout, level=logging.INFO)
logging.getLogger().addHandler(logging.StreamHandler(stream=sys.stdout))

```

### 加载大语言模型

因为`Qwen`本次支持了`Transformers`，使用`HuggingFaceLLM`加载模型，模型为（`Qwen1.5-4B-Chat`）

```
# Model names 
qwen2_4B_CHAT = "qwen/Qwen1.5-4B-Chat"

selected_model = snapshot_download(qwen2_4B_CHAT)

SYSTEM_PROMPT = """You are a helpful AI assistant.
"""

query_wrapper_prompt = PromptTemplate(
    " \n <<SYS>>\n" + SYSTEM_PROMPT + "<</SYS>>\n\n{query_str} \n </SYS>> "
)

llm = HuggingFaceLLM(
    context_window=4096,
    max_new_tokens=2048,
    generate_kwargs={"temperature": 0.0, "do_sample": False},
    query_wrapper_prompt=query_wrapper_prompt,
    tokenizer_name=selected_model,
    model_name=selected_model,
    device_map="auto",
    # change these settings below depending on your GPU
    model_kwargs={"torch_dtype": torch.float16},
)

```

### 下载数据并加载文档：

```
!wget https://modelscope.oss-cn-beijing.aliyuncs.com/resource/rag/xianjiaoda.md
!mkdir -p /mnt/workspace/custom_data
!mv /mnt/workspace/xianjiaoda.md /mnt/workspace/custom_data
documents = SimpleDirectoryReader("/mnt/workspace/data/xianjiaoda/").load_data()
documents

```

### 构建embedding类：

加载`GTE`模型，使用`GTE`模型构造`Embedding`类

```
embedding_model = "iic/nlp_gte_sentence-embedding_chinese-base"
class ModelScopeEmbeddings4LlamaIndex(BaseEmbedding, ABC):
    embed: Any = None
    model_id: str = "iic/nlp_gte_sentence-embedding_chinese-base"

    def __init__(
            self,
            model_id: str,
            **kwargs: Any,
    ) -> None:
        super().__init__(**kwargs)
        try:
            from modelscope.models import Model
            from modelscope.pipelines import pipeline
            from modelscope.utils.constant import Tasks
            # 使用modelscope的embedding模型（包含下载）
            self.embed = pipeline(Tasks.sentence_embedding, model=self.model_id)

        except ImportError as e:
            raise ValueError(
                "Could not import some python packages." "Please install it with `pip install modelscope`."
            ) from e

    def _get_query_embedding(self, query: str) -> List[float]:
        text = query.replace("\n", " ")
        inputs = {"source_sentence": [text]}
        return self.embed(input=inputs)["text_embedding"][0].tolist()

    def _get_text_embedding(self, text: str) -> List[float]:
        text = text.replace("\n", " ")
        inputs = {"source_sentence": [text]}
        return self.embed(input=inputs)["text_embedding"][0].tolist()

    def _get_text_embeddings(self, texts: List[str]) -> List[List[float]]:
        texts = list(map(lambda x: x.replace("\n", " "), texts))
        inputs = {"source_sentence": texts}
        return self.embed(input=inputs)["text_embedding"].tolist()

    async def _aget_query_embedding(self, query: str) -> List[float]:
        return self._get_query_embedding(query)

```

### 建设索引：

加载数据后，基于文档对象列表（或节点列表），建设他们的`index`，就可以方便的检索他们。

```
embeddings = ModelScopeEmbeddings4LlamaIndex(model_id=embedding_model)
service_context = ServiceContext.from_defaults(embed_model=embeddings, llm=llm)
set_global_service_context(service_context)

index = VectorStoreIndex.from_documents(documents)

```

### 加载Rerank模型：

```
rerank_llm_name = "AI-ModelScope/bge-reranker-v2-m3"
downloaded_rerank_model = snapshot_download(rerank_llm_name)
rerank_llm = SentenceTransformerRerank(model=downloaded_rerank_model, top_n=3)

```

首先，我们尝试使用`Rerank`，同时对查询+问答进行计时，检索召回`top_k=10`，`Rerank`重排`top_n=3`看看处理检索到的上下文的输出需要多长时间。

```
from time import time

query_engine = index.as_query_engine(similarity_top_k=10, node_postprocessors=[rerank_llm])

now = time()
response = query_engine.query("西安交大由哪几个学校合并的")
print(response)
print(f"Elapsed: {round(time() - now, 2)}s")
```

<https://v11enp9ok1h.feishu.cn/sync/Ipw1d27gNsv3OAbo3N7cks4ZnJf>
