
# 利用Ollama和Weaviate构建隐私安全的本地RAG系统

> 来源：https://agijuejin.feishu.cn/docx/CJRBdfEW6osqK6xGodWciasnnpe

> 作者：lucas大叔

> 原文：https://zhuanlan.zhihu.com/p/699369177

构建 [LLM赋能](https://www.zhihu.com/search?q=LLM%E8%B5%8B%E8%83%BD&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A699369177%7D)的原型应用非常容易，但当希望投入生产使用时，将会面临各种各样的挑战，例如减轻幻觉或者数据隐私保护。虽然RAG在减轻幻觉方面已经证明很有效，但本地部署是隐私保护的一种主流方法。

本文将展示如何不需要任何外部依赖，在本地环境中使用 Python 实现基于本地 RAG 的聊天机器人。使用的本地组件有：

+ 利用 Ollama 使用本地 LLM 和 embedding 模型

+ 利用 Docker 运行的本地 Weaviate 向量数据库实例

所有的组件都是本地的、开源的，不需要任何 API 密钥！

## 使用 Ollama 设置本地语言模型

### 第一步：下载和安装 Ollama

从 [官方网站](https://link.zhihu.com/?target=https%3A//ollama.com/download) 下载适用于你的操作系统的 Ollama 版本，这里我们选择 Linux 系统。

![](img/9d4c085d8b9057061a1818c7efa4fcc8.png)

复制下载页面的命令，在 Linux 终端执行

![](img/5febe388d122428c6cc7b7c7c88ba1de.png)

下载完成安装

![](img/df2ef2544fdaff5d1874e218e262b9d8.png)

启动 Ollama

```
sudo systemctl start ollama
```

### 第二步：拉取模型

打开终端拉取你选择的 LLM 和 embedding 模型。本示例使用 Meta 提供的 `llama2` 作为 LLM，`all-minilm` 作为 embedding 模型。

```
ollama pull llama2
ollama pull all-minilm
```

![](img/aaba906f7753f3473e92d92a51964731.png)

其他可用的 embedding 模型包括 `mxbai-embed-large`（334M 参数）和 `nomic-embed-text`（137M 参数）。Ollama 支持模型的完整列表请参阅 [这里](https://link.zhihu.com/?target=https%3A//ollama.com/library)。

### 第三步：安装 Ollama Python 库

由于将用 Python 实现 RAG pipeline，因此需要使用 `pip` 安装 Python 库。

```
pip install ollama
```

另外，还可以使用 [REST API](https://link.zhihu.com/?target=https%3A//github.com/ollama/ollama/blob/main/docs/api.md%23generate-embeddings) 或 [JavaScript](https://link.zhihu.com/?target=https%3A//github.com/ollama/ollama-js) 库。

## 使用 Docker 设置本地向量数据库实例

在本地 RAG pipeline 中，也希望将 [向量数据库](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E6%95%B0%E6%8D%AE%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A699369177%7D) 本地托管。本示例展示如何使用 Docker 在本地托管一个开源的 Weaviate 向量数据库实例。

### 第一步：下载和安装 Docker

安装 `docker`（版本 17.09.0 或更高）和 `docker-compose`（Docker Compose V2）CLI 工具。

### 第二步：使用 Weaviate 实例启动 Docker 容器

在终端上运行以下命令，从默认的 Docker 镜像启动 Weaviate 实例。

```
docker run -p 8080:8080 -p 50051:50051 cr.weaviate.io/semitechnologies/weaviate:1.24.8
```

### 第三步：安装 Weaviate Python 客户端

由于将用 Python 实现 RAG pipeline，因此需要使用 `pip` 安装 Python 库。

> 特别提示：本示例代码基于 `weaviate` 的 Python (Client v4) 实现，使用 Python (Client v3) 会报错！

```
pip install -U weaviate-client
```

## 创建本地 RAG pipeline

当在本地拉取了相关模型并准备好通过 Ollama 提供服务，以及向量数据库已通过 Docker 自行托管时，就可以开始实现 RAG pipeline。

![](img/a689fa95ba7bc560a1a585cea6e5e28f.png)

### 准备工作：将数据导入向量数据库

构建 RAG pipeline 的第一步是将数据导入向量数据库。为此，需要生成数据的 embedding。

下面是一些样本数据。

```
documents = [
  "Llamas are members of the camelid family meaning they're pretty closely related to vicuñas and camels",
  "Llamas were first domesticated and used as pack animals 4,000 to 5,000 years ago in the Peruvian highlands",
  "Llamas can grow as much as 6 feet tall though the average llama between 5 feet 6 inches and 5 feet 9 inches tall",
  "Llamas weigh between 280 and 450 pounds and can carry 25 to 30 percent of their body weight",
  "Llamas are vegetarians and have very efficient digestive systems",
  "Llamas live to be about 20 years old, though some only live for 15 years and others live to be 30 years old",
]
```

接下来连接本地运行的向量数据库实例

```
import weaviate

client = weaviate.connect_to_local()
```

启动时向量数据库是空的，需要填充数据。首先定义存储数据的结构（在本示例中称为 `docs` 的集合）。

由于示例数据只是简单的字符串列表，可以定义名为 `text` 的单属性，并使用 `DataType.TEXT` 作为数据类型。

```
import weaviate.classes as wvc
from weaviate.classes.config import Property, DataType

# Create a new data collection
collection = client.collections.create(
    name = "docs", # Name of the data collection
    properties=[
        Property(name="text", data_type=DataType.TEXT), # Name and data type of the property
    ],
)
```

现在可以将数据加载到预定义的结构中。遍历文档，并使用 Ollama 的 `embeddings()` 方法为每个数据对象生成 embedding。然后，文本及其 embedding 将一起存储在向量数据库中。

```
import ollama

# Store each document in a vector embedding database
with collection.batch.dynamic() as batch:
  for i, d in enumerate(documents):
    # Generate embeddings
    response = ollama.embeddings(model = "all-minilm",
                                 prompt = d)

    # Add data object with text and embedding
    batch.add_object(
        properties = {"text" : d},
        vector = response["embedding"],
    )
```

### 第一步：检索上下文

在推理阶段，希望为 `prompt` 检索额外的上下文。为此，需要对 `prompt` 进行简单的相似性搜索（例如，“What animals are llamas related to？”）。

对于相似性搜索，首先对搜索查询（这里是 `prompt`）生成向量嵌入，类似于摄入阶段的 `embeddings()` 方法。然后，将生成的 `embedding` 传递给 Weaviate 的 `near_vector()` 方法，并指定只检索最接近的结果（`limit = 1`）。

```
# An example prompt
prompt = "What animals are llamas related to?"

# Generate an embedding for the prompt and retrieve the most relevant doc
response = ollama.embeddings(
  model = "all-minilm",
  prompt = prompt,
)

results = collection.query.near_vector(near_vector = response["embedding"],
                                       limit = 1)

data = results.objects[0].properties['text']
```

> Llamas are members of the camelid family meaning they're pretty closely related to vicuñas and camels

### 第二步：扩充 prompt

接下来使用原始 `prompt` 和检索到的上下文来扩充提示模板：

```
prompt_template = f"Using this data: {data}. Respond to this prompt: {prompt}"
```

### 第三步：生成响应

最后，用 Ollama 的 `generate()` 方法从扩充的提示模板中生成响应。

```
# Generate a response combining the prompt and data we retrieved in step 2
output = ollama.generate(
  model = "llama2",
  prompt = prompt_template,
)

print(output['response'])
```

> Llamas are members of the camelid family, which means they are closely related to other animals in the same family, including:
>
> 1. Vicuñas: Vicuñas are small, wild relatives of llamas and alpacas. They are found in the Andean region and are known for their soft, woolly coats.
>
> 2. Camels: Camels are large, even-toed ungulates that are closely related to llamas and vicuñas. They are found in hot, dry climates around the world and are known for their ability to go without water for long periods of time.



> 3. Guanacos: Guanacos are large, wild animals that are related to llamas and vicuñas. They are found in the Andean region and are known for their distinctive long necks and legs.

> 4. Llama-like creatures: There are also other animals that are sometimes referred to as "llamas," such as the lama-like creatures found in China, which are actually a different species altogether. These creatures are not closely related to vicuñas or camels, but are sometimes referred to as "llamas" due to their physical similarities.

> In summary, llamas are related to vicuñas, camels, guanacos, and other animals that are sometimes referred to as "llamas."

## 总结

本文引导你通过一个非常简单的 RAG pipeline 示例，突出说明如何使用本地组件（通过 Ollama 提供语言模型、通过 Docker 自行托管的 Weaviate 向量数据库）构建本地 RAG 系统，以实现隐私保护。

值得注意的是，本示例未使用任何编排框架，例如 LangChain 或 LlamaIndex。然而，LangChain 和 LlamaIndex 都与 Ollama 和 Weaviate 集成，因此你可以轻松地将上述示例修改为你选择的编排框架。

本文的完整实现代码请参阅 [GitHub 仓库](https://link.zhihu.com/?target=https%3A//github.com/weaviate/recipes/blob/main/integrations/ollama/local_rag.ipynb)。

## 参考文档

[Building a Local RAG System for Privacy Preservation with Ollama and Weaviate | Weaviate - Vector Database](https://weaviate.io/blog/local-rag-with-ollama-and-weaviate)
