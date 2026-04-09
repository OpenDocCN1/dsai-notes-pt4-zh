# 书生·浦语知识图谱+RAG实践

> 来源：[https://aicarrier.feishu.cn/docx/SGMhdJwpioRyuJxUxUFc2rP9nre](https://aicarrier.feishu.cn/docx/SGMhdJwpioRyuJxUxUFc2rP9nre)

![](img/21e1109299946da5b64a4ff2e590526b.png)

> 文档尚处于草稿阶段，完善中

欢迎来到 知识图谱+RAG 系列教程！在这个任务中，你将体验如何使用 LightRAG（轻量级的图结构检索增强生成框架）在书生·浦语的 InternLM3-8b 模型上来构建知识图谱，并进行有效的文本检索。通过完成以下任务，你将了解如何利用 LightRAG 的不同检索模式来获取 InternLM3 模型生成的不同层次的信息，同时掌握框架的配置与使用方法。

**闯关任务：**

1.  使用 LightRAG 框架在书生·浦语 InternLM3 模型上进行图检索增强生成，对比不同检索模式对大模型输出的影响。(本地实践 + API 实践)

1.  进行知识图谱可视化，理解 知识图谱+RAG 加强大模型检索与推理过程的原理。

# 什么是 知识图谱+RAG（图检索增强生成）

知识图谱+RAG是结合了知识图谱和检索增强生成（RAG，Retrieval-Augmented Generation）的方法，用于提升自然语言处理（NLP）任务的效果。知识图谱是通过节点（实体）和边（关系）构建的图结构，能够表达复杂的知识和实体间的关系。而RAG框架通过结合外部文档检索与生成模型，将检索到的相关信息作为上下文提供给生成模型，从而提高生成内容的准确性和丰富度。

在本次任务中我们使用 LightRAG 框架来完成基于书生·浦语 InternLM3 模型的实践。

LightRAG是一个轻量级的检索增强生成 (Retrieval-Augmented Generation, RAG) 框架，通过结合图结构和文本索引，克服了现有 RAG 系统的局限性。它利用双层检索机制和向量表示，提高了信息检索能力、加快响应速度并维持上下文相关性。此外，通过增量更新算法支持数据的实时整合。实验显示，LightRAG 在准确性和效率上超越了现有方法。

仓库地址：https://github.com/HKUDS/LightRAG.git

# 配置环境

## 创建虚拟环境

```
conda create -n lightrag python=3.10
conda activate lightrag
```

## 下载 LightRAG 仓库，并安装相关包

```
git clone https://github.com/HKUDS/LightRAG.git  
cd LightRAG
pip install -e .
pip install httpx
pip install einops
pip install sentencepiece
```

# 下载 RAG 内容（zhuyuanzhang.txt 文本）

```
curl https://raw.githubusercontent.com/gusye1234/nano-graphrag/main/tests/zhuyuanzhang.txt &gt; ./book.txt
```

# LightRAG+本地模型 internlm3-8b 实践

## 运行 LightRAG 提供的 naive 检索模式

因为 naive 检索模式不使用图谱，所以我们运行 naive 代码会首先将 RAG 文本信息录入 ./dickens 文件夹，并输出实体切分情况

在 LighRAG 目录下新建 local_naive.py 文件，写入以下代码：

```
import os

from lightrag import LightRAG, QueryParam
from lightrag.llm.hf import hf_model_complete, hf_embed
from lightrag.utils import EmbeddingFunc
from transformers import AutoModel, AutoTokenizer

WORKING_DIR = &quot;./dickens&quot;

if not os.path.exists(WORKING_DIR):
    os.mkdir(WORKING_DIR)

# 使用 internlm3-8b-instruct 本地模型
internlm_model_path = &quot;/root/share/new_models/internlm3/internlm3-8b-instruct&quot;
embed_model_path = &quot;/root/llamaindex_demo/model/sentence-transformer&quot;

# 加载本地生成模型
llm_model = AutoModel.from_pretrained(internlm_model_path, trust_remote_code=True)
tokenizer = AutoTokenizer.from_pretrained(internlm_model_path, trust_remote_code=True)

# 加载本地嵌入模型
embed_model = AutoModel.from_pretrained(embed_model_path)
embed_tokenizer = AutoTokenizer.from_pretrained(embed_model_path)

# 创建 LightRAG 实例
rag = LightRAG(
    working_dir=WORKING_DIR,
    llm_model_func=hf_model_complete,
    llm_model_name=internlm_model_path,
    embedding_func=EmbeddingFunc(
        embedding_dim=384,
        max_token_size=5000,
        func=lambda texts: hf_embed(
            texts,
            tokenizer=embed_tokenizer,
            embed_model=embed_model,
        ),
    ),
)

# 插入文本到 RAG 中
with open(&quot;./book.txt&quot;, &quot;r&quot;, encoding=&quot;utf-8&quot;) as f:
    rag.insert(f.read())

# 执行 naive 查询模式
print(
    rag.query(&quot;这个故事的主题是什么？&quot;, param=QueryParam(mode=&quot;naive&quot;))
)
```

终端运行上述代码：

```
python local_naive.py
```

输出截图：

![](img/2b75cacc9dcfca780ce14edbb7beecc1.png)

通过输出我们可以看到 LightRAG 通过读取 RAG 文本并构建实体间关联的基本流程：

1、获得文档分段

2、插入文档分段向量表

3、大模型提取点和边

## 使用 local 和 global 检索模式并进行效果对比

### Local Search 局部检索 和 Global Search 全局检索

Local Search 和 Global Search 都会查出相关联的实体点，在根据实体点关联的边和邻居实体点，进行相结合出来的文档，提交给大模型进行回答问题。

他们的区别在于，local search 根据 low 关键词进行检索，查询到实体点后利用 low_level_keywords，对关系边 vdb_relationships.json 向量库进行检索；

而 global 是根据 high 关键词进行检索，检索出实体点，global search 利用 high_level_keywords，对关系边 vdb_relationships.json 向量库进行检索。

### 调用

在 LighRAG 目录下新建 local_global.py 文件，写入以下代码：

```
import os

from lightrag import LightRAG, QueryParam
from lightrag.llm.hf import hf_model_complete, hf_embed
from lightrag.utils import EmbeddingFunc
from transformers import AutoModel, AutoTokenizer

WORKING_DIR = &quot;./dickens&quot;

if not os.path.exists(WORKING_DIR):
    os.mkdir(WORKING_DIR)

# 使用 internlm3-8b-instruct 本地模型
internlm_model_path = &quot;/root/share/new_models/internlm3/internlm3-8b-instruct&quot;
embed_model_path = &quot;/root/llamaindex_demo/model/sentence-transformer&quot;

# 加载本地生成模型
llm_model = AutoModel.from_pretrained(internlm_model_path, trust_remote_code=True)
tokenizer = AutoTokenizer.from_pretrained(internlm_model_path, trust_remote_code=True)

# 加载本地嵌入模型
embed_model = AutoModel.from_pretrained(embed_model_path)
embed_tokenizer = AutoTokenizer.from_pretrained(embed_model_path)

rag = LightRAG(
    working_dir=WORKING_DIR,
    llm_model_func=hf_model_complete,
    llm_model_name=internlm_model_path,
    embedding_func=EmbeddingFunc(
        embedding_dim=384,
        max_token_size=5000,
        func=lambda texts: hf_embed(
            texts,
            tokenizer=embed_tokenizer,
            embed_model=embed_model,
        ),
    ),
)

# 插入文本到 RAG 中
with open(&quot;./book.txt&quot;, &quot;r&quot;, encoding=&quot;utf-8&quot;) as f:
    rag.insert(f.read())

# 执行 local 查询模式
print(&quot;\n\n*****local局部检索*****\n&quot;)
print(
    rag.query(&quot;这个故事的主题是什么？请用中文回答&quot;, param=QueryParam(mode=&quot;local&quot;))
)

# 执行 global 查询模式
print(&quot;\n\n*****global全局检索*****\n&quot;)
print(
    rag.query(&quot;这个故事的主题是什么？&quot;, param=QueryParam(mode=&quot;global&quot;))
)
```

终端运行上述代码：

```
python local_global.py
```

查看输出效果：

**local 检索效果：**

![](img/e6751f5bb7a15b96c73ebd5253efa332.png)

**global 检索效果：**

![](img/0d5f0259b3ef6f8d89370f4f97488892.png)

### local 和 global 检索模式效果对比

+   Naive：传统的RAG框架

+   Local：检索 low level key word，关注低粒度的、具体的细节信息，通常针对非常具体的问题或事件

+   Global：检索 high level key word，关注高粒度的、概念性的信息，通常适用于更为抽象或广泛的问题

+   Naive 简单检索 虽然简洁直观，但缺少对故事背后深刻主题的挖掘。

+   Local 局部上下文检索 则在细节上更为充实，增加了对人物内心的多层次解读。

+   Global 全局上下文检索 侧重于深入探讨社会问题，尤其是阶级差异和道德觉醒。

# LightRAG+InternLM2.5 的 API 实践

## 申请 API（生成模型 InternLM2.5 和 嵌入向量模型 bge-m3）

首先，分别在浦语官网和硅基流动申请 api-key，可以访问以下两个网址了解两个 API 的使用方法和 Key。

浦语官方 API：[https://internlm.intern-ai](https://internlm.intern-ai.org.cn/api/document).org.cn/api/document  API 用于生成模型（InternLM2.5-latest）

硅基流动：https://siliconflow.cn/zh-cn/models  API 用于嵌入向量模型（BAAI/bge-m3）

## 调用

新建 api.py 写入以下代码：

```
import os
import asyncio
from lightrag import LightRAG, QueryParam
from lightrag.llm.openai import openai_complete_if_cache, openai_embed
from lightrag.utils import EmbeddingFunc
import numpy as np

WORKING_DIR = &quot;./dickens&quot;

if not os.path.exists(WORKING_DIR):
    os.mkdir(WORKING_DIR)

semaphore = asyncio.Semaphore(1)

#生成模型函数
async def llm_model_func(prompt, system_prompt=None, history_messages=[], **kwargs):
    async with semaphore:
        result = await openai_complete_if_cache(
            &quot;internlm2.5-latest&quot;,
            prompt,
            system_prompt=system_prompt,
            history_messages=history_messages,
            api_key=&quot;your_api_key&quot;,
            base_url=&quot;https://chat.intern-ai.org.cn/api/v1/&quot;,
            **kwargs,
        )
        await asyncio.sleep(1)
        return result

#嵌入向量模型函数
async def embedding_func(texts: list[str]) -&gt; np.ndarray:
    return await openai_embed(
        texts,
        model=&quot;BAAI/bge-m3&quot;,
        api_key=&quot;sk-your_api_key&quot;,
        base_url=&quot;https://api.siliconflow.cn/v1&quot;,
    )

rag = LightRAG(
    working_dir=WORKING_DIR,
    llm_model_func=llm_model_func,
    embedding_func=EmbeddingFunc(
        embedding_dim=1024,
        max_token_size=8192,
        func=embedding_func,
    ),
)

with open(&quot;./book.txt&quot;, &quot;r&quot;, encoding=&quot;utf-8&quot;) as f:
    rag.insert(f.read())

print(rag.query(&quot;这个故事有那些主题？&quot;, param=QueryParam(mode=&quot;naive&quot;)))
print(rag.query(&quot;这个故事有那些主题？&quot;, param=QueryParam(mode=&quot;local&quot;)))
print(rag.query(&quot;这个故事有那些主题？&quot;, param=QueryParam(mode=&quot;global&quot;)))
print(rag.query(&quot;这个故事有那些主题？&quot;, param=QueryParam(mode=&quot;hybrid&quot;)))
```

最后我们让 Internlm2.5 模型结合知识图谱，分别使用 LightRAG 提供的四种检索方式，生成对故事主题的分析。

naive：简单搜索

local：局部关键词搜索（low level key word）

global：全局关键词搜索（high level key word）

hybrid：混合关键词搜索（low+high level key word）

## 查看输出流程

首先删除 LightRAG 目录下的 dickens 文件，清空之前本地模型对图谱的构建记录。

接下来我们运行 api.py 代码：

```
python api.py
```

**输出流程：**

### 重新构建图谱

![](img/d821835af7d4c3a1dc2c008df5e9cb36.png)

接着，输出四种检索方式的模型回应：

### naive（简单检索）

![](img/25141a847bc6ce62bbc4b53d7088b3bf.png)

### Local （ low level key word 检索）

![](img/d4a5965967be5eccd121cda1c0c302e0.png)

### global（ high level key word 检索）

![](img/51f27ecaf654f6210c75ce1c6cbc8128.png)

### hybrid（ low+high level key word 检索）

![](img/77a5017007befa2281401f75059b0eae.png)

### KG References

![](img/1ebee976ee2cb3da5e307b69c9086d04.png)

# 知识图谱可视化

首先，我们需要把 LightRAG/examples/graph_visual_with_html.py 文件复制到 examples 文件夹同级目录下。

接下来在终端运行 graph_visual_with_html.py：

```
cd
cd LightRAG
python graph_visual_with_html.py
```

运行完成后，我们可以看到生成了 knowledge_graph.html 文件。

右键点击选择 Open in New Browser Tab

![](img/76881fcaff72ab719dcb7e37bc878d6e.png)

打开网页后可以查看构建的知识图谱：

![](img/9b83fac33e2209dec8bd60c04176d43e.png)

通过知识图谱可视化，我们可以查看模型对 RAG 文本切分后，每一个实体节点的具体信息，以及实体间关联情况。