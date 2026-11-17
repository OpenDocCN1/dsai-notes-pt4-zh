
## Step 2. 创建检索项目文件夹

```
cd /root/autodl-tmp/graphrag-ollama
mkdir -p ./ml-ollama/input
```

![](img/f616428d139830baf0b52c714d9009a8.png)

## Step 3. 上传数据集

![](img/ad01a46ac7bc0703f11d92b8a26974a1.png)

![](img/50cbef2563d3e55cb20868df102e756f.png)

> 相关数据集已同步至本节公开课的课件包，扫码添加老师助理即可领取：

![](img/d0a863d61b9fd60a35e9eac5c2b497be.png)

## Step 4. 初始化项目文件

```
graphrag init --root /root/autodl-tmp/graphrag-ollama/ml-ollama
```

## Step 5. 修改项目配置

+ 修改`.env`文件：

![](img/09cf40bec1db8655359e23fee0ca08dc.png)

将`API-KEY`设置为`ollama`：`GRAPHRAG_API_KEY=ollama`

![](img/8c6b1b4de2f80c18ba9d85ba45950c21.png)

+ 修改`setting.yaml`文件

model: qwen2.5-7b-instruct

api_base: http://localhost:11434/v1

![](img/319c05be875db617609f4570905e46ae.png)

model:nomic-embed-text

api_base: http://localhost:11434/api

![](img/a33d6cdda95db6b824bad93bc1ebe0dc.png)

## Step 6. 修改项目源码

首先，找到`pip`安装库的地址：`/root/miniconda3/envs/graphrag-ollama/lib/python3.10/site-packages/graphrag`

![](img/0654fb406afa2de0fd7db272f85423e8.png)

+ 修改一：`llm/openai/openai_embeddings_llm.py`

![](img/62e360e54d65d966ccf69038892ae81d.png)

修改为如下内容：

```
# Copyright (c) 2024 Microsoft Corporation.
# Licensed under the MIT License

"""The EmbeddingsLLM class."""

from typing_extensions import Unpack

from graphrag.llm.base import BaseLLM
from graphrag.llm.types import (
    EmbeddingInput,
    EmbeddingOutput,
    LLMInput,
)

from .openai_configuration import OpenAIConfiguration
from .types import OpenAIClientTypes

import ollama


class OpenAIEmbeddingsLLM(BaseLLM[EmbeddingInput, EmbeddingOutput]):
    """A text-embedding generator LLM."""

    _client: OpenAIClientTypes
    _configuration: OpenAIConfiguration

    def __init__(self, client: OpenAIClientTypes, configuration: OpenAIConfiguration):
        self.client = client
        self.configuration = configuration

    async def _execute_llm(
        self, input: EmbeddingInput, **kwargs: Unpack[LLMInput]
    ) -> EmbeddingOutput | None:
        args = {
            "model": self.configuration.model,
            **(kwargs.get("model_parameters") or {}),
        }
        # embedding = await self.client.embeddings.create(
        #     input=input,
        #     **args,
        # )
        embedding_list = []
        for inp in input:
            embedding = ollama.embeddings(model='nomic-embed-text', prompt=inp)
            embedding_list.append(embedding["embedding"])
        return embedding_list
```

+ 修改二：`query/llm/oai/embedding.py`

修改后内容如下：
