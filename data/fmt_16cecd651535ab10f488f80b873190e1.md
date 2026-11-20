
# RAG：在`LangChain`中使用本地向量`embedding`模型

> 来源：[https://agijuejin.feishu.cn/docx/Lq3yd1MGeoBYuUx35Shca2S5nub](https://agijuejin.feishu.cn/docx/Lq3yd1MGeoBYuUx35Shca2S5nub)

> 作者：哎呀AIYA

> 原文：https://mp.weixin.qq.com/s/JcIr1VLNAEzDALXkeYaPpA

向量模型是`RAG`系统中实现有效信息检索和生成的关键技术之一，它们使得系统能够处理复杂的语言理解任务，并生成更加准确和相关的输出。

向量模型将文本转换为向量形式，便于在高维空间中进行快速的相似性检索，这是`RAG`系统中检索相关信息的基石。通过向量化，模型能够评估不同文本之间的语义相似度，即使在 词汇不完全匹配 的情况下也能找到 语义相关 的文档。向量模型帮助系统捕捉输入查询的 上下文信息 ，这对于理解用户意图并检索最相关的信息至关重要。

本篇文章将为大家介绍在`langchain`中使用自己向量模型的方法，帮助大家扫清障碍快速搭建`RAG`和`Agent`流程。

# 环境依赖

### 本示例用到的安装包如下：

```
pip install torch langchain sentence_transformers 
```

# 模型选择

### 向量模型可以去`MTEB`榜单上找，发掘一个适合自己业务的模型。目前榜单如下：

![](img/0d2564626b58c0c9603be3f8db6733c2.png)

```
# 榜单地址
https://huggingface.co/spaces/mteb/leaderboard
```

本文采用`bge-m3`模型作为例子，其是向量维数为1024维，支持的最大长度为8192，是一个支持多语言的模型，目前效果还算比较好。后面会专门写一篇文章介绍向量模型如何选择和评测。以下是`bge-m3`的一些信息：

![](img/5016307cac74ac2d8faa40652b7b7b48.png)

# 示例代码

直接上代码，可以直接用在自己的项目中：

```
import torch
from typing import Any, List
from pydantic import Extra
from langchain.embeddings.base import Embeddings
from sentence_transformers import SentenceTransformer

device = 'cpu'

class CustomEmbedding(Embeddings):

    client: Any  #: :meta private:
    tokenizer: Any
    context_sequence_length: int = 512
    query_sequence_length: int = 512
    model_name: str = ''
    """Model name to use."""

    def __init__(self, **kwargs: Any):
        """Initialize the sentence_transformer."""
        # super().__init__(**kwargs)
        self.client = SentenceTransformer(
            'BAAI/bge-m3',
            device=device,
            trust_remote_code=True)
        self.context_sequence_length = 512
        self.query_sequence_length = 512

    class Config:
        extra = Extra.forbid

    @staticmethod
    def mean_pooling(model_output, attention_mask):
        # First element of model_output contains all token embeddings
        token_embeddings = model_output[0]
        input_mask_expanded = attention_mask.unsqueeze(
            -1).expand(token_embeddings.size()).float()
        return torch.sum(token_embeddings * input_mask_expanded, 1) / \
            torch.clamp(input_mask_expanded.sum(1), min=1e-9)

    def embed_documents(self, texts: List[str]) -> List[List[float]]:

        with torch.no_grad():
            embeddings = self.client.encode(texts)
        embeddings = embeddings.astype('float32')
        return embeddings.tolist()

    def embed_query(self, text: str) -> List[float]:
        return self.embed_documents([text])[0]
```

正确运行后，输出的结果是1024，即代表`query`被向量化后的维数为1024维；可以用这个模型替换上篇文章中的`OpenAIEmbeddings`。搭建`RAG`系统就这么简单：`LangChain`|`RAG`是什么？

如果对内容有什么疑问和建议可以私信和留言，也可以添加我加入大模型交流群，一起讨论大模型在创作、`RAG`和`agent`中的应用。

往期推荐

+   `RAG`：`Langchain`中使用自己的`LLM`大模型

+   详解`GPT-4o`的收费设计，为什么输出`token`贵？

+   搭建`RAG`系统就这么简单：`LangChain`|`RAG`是什么？

+   一文读懂`token`到底是什么鬼，消耗那么多钱？
