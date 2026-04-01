# 使用FastChat快速部署LLM服务

> 来源：[https://agijuejin.feishu.cn/docx/MX9MdinwXo9EsIx8ZEDcISJ7nVb](https://agijuejin.feishu.cn/docx/MX9MdinwXo9EsIx8ZEDcISJ7nVb)

> 作者：lucas大叔

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/705915093)hihu.com/p/705915...

FastChat 是一个用于训练、服务和评估基于LLM的聊天机器人的开放平台，它的核心功能包括：

+   最先进模型（例如 Vicuna、MT-Bench）的训练和评估代码。

+   具有 Web UI 和与 OpenAI 兼容的 RESTful API 的分布式多模型服务系统。

## 准备工作

### 创建虚拟环境

为了避免潜在的依赖冲突，创建单独的虚拟环境用于FastChat部署。

```
# initiate virtual environment
python -m venv .venv
# activate virtual environemnt
source .venv/bin/activate
# upgrade `pip`
pip install -U pip
```

### 安装FastChat

FastChat有pip安装和源码安装两种方式，这里采用pip方式安装：

```
pip install &quot;fschat[model_worker,webui]&quot; -i https://pypi.tuna.tsinghua.edu.cn/simple
```

> 如运行时报缺少accelerate的错误，请通过pip install accelerator 安装。

### 安装vLLM（可选）

为了获得更好的推理性能，推荐使用vLLM作为后端来加速推理。

```
pip install vllm -i https://pypi.tuna.tsinghua.edu.cn/simple
```

### 下载LLM

从modelscope下载本次演示的模型glm-4-9b-chat，下载命令如下：

```
git lfs install

git clone https://www.modelscope.cn/ZhipuAI/glm-4-9b-chat.git
```

## 使用FastChat部署LLM

为了能够通过 Web 和 SDK 访问 LLM 服务，需要在服务器上部署以下 FastChat 的组件：

+   Controller

+   Worker

+   Gradio Web Server

+   OpenAI API Server

![](img/b27eb42032954bde5de7595803bdba56.png)

### 启动 Controller

```
python -m fastchat.serve.controller --host 0.0.0.0
```

默认端口为 21001 ，可通过 --port 指定。 --host 参数是设置服务的主机地址，这里设置为 0.0.0.0 ，表示可以通过任何地址访问。

![](img/0d1235da0041c3775a13bee9004c35db.png)

### 启动vLLM Worker

```
python -m fastchat.serve.vllm_worker --model-path /path/to/local_model --host 0.0.0.0
```

默认端口为 21002 ，可通过 --port 指定。FastChat 的 Worker 会向 Controller 注册自身，并通过心跳机制保持连接。若没有安装vLLM后端，可通过下面的命令直接启动model_work:

```
python -m fastchat.serve.model_worker --model-path /path/to/local_model --host 0.0.0.0
```

![](img/250b7af298fd50c2c975b01abe696554.png)

> 默认使用单GPU时，memory溢出。修改启动命令： python -m fastchat.serve.vllm_worker --model-path /path/to/glm-4-9b-chat --host 0.0.0.0 --num-gpus 4 指定4块GPU。

### 启动 OpenAI API Server

```
python -m fastchat.serve.openai_api_server --host 0.0.0.0
```

服务启动后，默认端口是 8000，可以通过 --port 参数来修改端口。

![](img/6e5daa973d7d523b76af26a4f915612e.png)

## 使用OpenAI SDK

使用 FastChat OpenAI API Server 的端点初始化

```
import os
from langchain.chat_models import ChatOpenAI

os.environ[&quot;OPENAI_API_BASE&quot;] = &#x27;http://localhost:8000/v1&#x27;
os.environ[&quot;OPENAI_API_KEY&quot;] = &#x27;na&#x27;

llm = ChatOpenAI(model=&#x27;glm-4-9b-chat&#x27;, temperature=0)
```

测试简单问答

```
result = llm.invoke(&quot;介绍下你自己&quot;)
print(result.content)
```

![](img/0d884eb84c0d9a193bf2ccf598b7ca25.png)

在RAG实现中应用

```
# ----------------------------------------------------------- #
import torch
from langchain.document_loaders import PyPDFLoader
from langchain import PromptTemplate
from langchain import HuggingFacePipeline
from langchain.vectorstores import Chroma
from langchain.retrievers import BM25Retriever, EnsembleRetriever
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.schema.runnable import RunnablePassthrough
from langchain.schema.output_parser import StrOutputParser
from transformers import AutoModelForCausalLM, AutoTokenizer, pipeline

import os
from langchain.chat_models import ChatOpenAI
# ----------------------------------------------------------- #
os.environ[&quot;OPENAI_API_BASE&quot;] = &#x27;http://localhost:8000/v1&#x27;
os.environ[&quot;OPENAI_API_KEY&quot;] = &#x27;na&#x27;
llm = ChatOpenAI(model=&#x27;glm-4-9b-chat&#x27;, temperature=0)
# ----------------------------------------------------------- #
embed_path = &quot;/path/to/bge-large-zh-v1.5&quot;
embeddings = HuggingFaceEmbeddings(
    model_name=embed_path,
    model_kwargs={&quot;device&quot;: &quot;cuda&quot;},
    encode_kwargs={&quot;normalize_embeddings&quot;: True},
)
# ----------------------------------------------------------- #
loader = PyPDFLoader(&quot;/path/to/data/中华人民共和国证券法(2019修订).pdf&quot;)
documents = loader.load_and_split()

text_splitter = RecursiveCharacterTextSplitter(separators=[&#x27;。&#x27;], chunk_size=512, chunk_overlap=32)
texts_chunks = text_splitter.split_documents(documents)
texts_list = [text.page_content for text in texts_chunks]
# ----------------------------------------------------------- #
bm25_retriever = BM25Retriever.from_texts(texts_list)
bm25_retriever.k = 2

vectorstore = Chroma.from_documents(texts_chunks, embeddings, persist_directory=&quot;db&quot;)
chroma_retriever = vectorstore.as_retriever(search_kwargs={&quot;k&quot;: 2})
ensemble_retriever = EnsembleRetriever(retrievers=[bm25_retriever, chroma_retriever], weights=[0.5, 0.5])
# ----------------------------------------------------------- #
template = &quot;&quot;&quot;你是一名智能助手，可以根据上下文回答用户的问题。

已知内容：
{context}

问题：
{question}
&quot;&quot;&quot;
prompt = PromptTemplate.from_template(template)
chain = (
        {&quot;question&quot;: RunnablePassthrough(), &quot;context&quot;: ensemble_retriever}
        | prompt
        | llm
        | StrOutputParser()
)
# ----------------------------------------------------------- #
query = &quot;公司首次公开发行新股，应当符合哪些条件？&quot;
result = chain.invoke(query)
print(result)
```

![](img/e03f5c6792c9b9593a605a794dc3fbf5.png)

## 关闭服务

关闭所有服务

```
python -m fastchat.serve.shutdown_serve --down all
```

关闭 Controller

```
python -m fastchat.serve.shutdown_serve --down controller
```

关闭 Model Worker

```
python -m fastchat.serve.shutdown_serve --down model_worker
```

关闭 OpenAI API Server

```
python -m fastchat.serve.shutdown_serve --down openai_api_server
```

杀死所有服务

```
kill -9 $(pgrep -f fastchat)
```

## 参考文档

[GitHub - lm-sys/FastChat: An open platform for training, serving, ](https://link.zhihu.com/?target=https%3A//github.com/lm-sys/FastChat)and evaluating large language models. Release repo for Vicuna and Chatbot Arena.

[h](https://link.zhihu.com/?target=https%3A//rudeigerc.dev/posts/llm-inference-with-fastchat/)ttps:// rudeigerc.dev/posts/llm -inference-with-fastchat/

[h](https://link.zhihu.com/?target=https%3A//zhaozhiming.github.io/2023/08/22/use-fastchat-deploy-llm/)ttps:// zhaozhiming.github.io/2 023/08/22/use-fastchat-deploy-llm/

[ht](https://link.zhihu.com/?target=https%3A//wangjunjian.com/fastchat/vllm/2024/01/16/Using-FastChat-to-Deploy-LLM-on-CUDA.html)tps:// wangjunjian.com/fastcha t/vllm/2024/01/16/Using-FastChat-to-Deploy-LLM-on-CUDA.html