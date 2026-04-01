# 如何使用 MongoDB、LlamaIndex 和 OpenAI 构建一个RAG系统

> 来源：[https://agijuejin.feishu.cn/docx/S3Uid42Czof3tVxOsTjcPm8EnZg](https://agijuejin.feishu.cn/docx/S3Uid42Czof3tVxOsTjcPm8EnZg)

> 作者：向量检索实验室 | AI 搜索引擎

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/dqVoZISNkw2EMxFOvIMS6w?poc_token=HIgryGajhMlVVkxksx5tdctkM1vPaGyuuX0OUtux)qq.com/s/dqVoZISN...

原文链接：https://www.mongodb.com/developer/products/atlas/rag-with-polm-stack-llamaindex-openai-mongodb/

![](img/3219bc1800b5087c642c3500585e5708.png)

#  **简介 **

大语言模型(LLM)为商业应用带来了巨大的好处,尤其是在提高生产力方面。LLM 及其应用无疑是有利的,但仅依靠 LLM 的参数化知识来响应用户的输入和提示对于私有数据或依赖实时数据的查询来说是不够的。这就是为什么需要一个非参数化的安全知识源来保存敏感数据并可以定期更新,以增强具有当前和相关信息的用户输入到 LLM 中。

检索增强生成(RAG)是一种系统设计模式,它利用信息检索技术和生成式 AI 模型,通过检索语义相关数据来增强用户查询,并将其作为额外的上下文与用户查询相结合,作为 LLM 的输入,从而提供准确且相关的响应。

![](img/2fa4658766f62f102b89508359bd8cea.png)

**关键要点 **:

+   RAG概述

+   了解 AI 技术栈

+   如何使用 MongoDB、LlamaIndex 和 OpenAI 构建一个端到端的 RAG 系统

#  **什么是AI技术栈 **

本教程将使用 **OLM ****(Open**A**I**、LlamaIndex 和 MongoDB)或 POLM (Python、OpenAI、LlamaIndex、MongoDB)AI 技术栈实现一个端到端的 RAG 系统。 AI 技术栈或 GenAI 技术栈是指用于构建和开发具有生成式 AI 能力的现代应用程序的模型、数据库、库和业务编排框架的组合。

一个典型的 AI 技术栈实现了一个基础设施,利用来自 LLM 的参数化知识和来自数据的非参数化知识来增强用户查询。AI 技术栈的组件包括:模型、编排器或集成器、操作型和向量数据库。在本教程中,MongoDB 将同时充当操作型和向量数据库。

| AI stack component  AI堆栈组件 | OLM/POLM stack  OLM/POLM 堆栈 |
| --- | --- |
| Language  (语言) | Python |
| Model provider  (模型提供者) | OpenAI (GPT-3.5, GPT-4) |
| LLM orchestrator  (LLM 协调器) | LlamaIndex |
| Operational and vector  database  (操作型和向量数据库) | MongoDB Atlas |

![](img/3a6e94d975dd76c10430d471e850f57e.png)

本文的实现步骤可以在 **github仓库 **[1]中找到。以下步骤的内容详细解释了用于实现 RAG 系统目标的库类、方法和过程。

#  **步骤 1:安装库 **

下面的代码片段安装了各种库,提供了访问 LLM、重新排序模型、数据库和集合方法的功能,将与广泛编码相关的复杂性抽象为几行代码和方法调用。

+   LlamaIndex:提供将数据源(文件、PDF、网站或数据源)连接到封闭式(OpenAI、Cohere)和开源(Llama)大语言模型的功能的数据框架;LlamaIndex 框架抽象了与数据摄取、RAG 管道实现和 LLM 应用程序开发(聊天机器人、代理等)相关的复杂性。

+   LlamaIndex (MongoDB):LlamaIndex 扩展库,导入所有必要的方法来连接并操作 MongoDB Atlas 数据库。

+   LlamaIndex (OpenAI):LlamaIndex 扩展库,导入所有必要的方法来访问 OpenAI 嵌入模型。

+   PyMongo:用于与 MongoDB 交互的 Python 库,提供连接到集群并查询存储在集合和文档中的数据的功能。

+   Hugging Face datasets:Hugging Face 库包含音频、视觉和文本数据集。

+   Pandas:使用 Python 提供数据结构进行高效的数据处理和分析。

```
!pip install llama-index

!pip install llama-index-vector-stores-mongodb

!pip install llama-index-embeddings-openai

!pip install pymongo

!pip install datasets

!pip install pandas

```

#  步骤 2:数据获取和 OpenAI 密钥设置

下面的命令将 OpenAI API 密钥分配给环境变量 OPENAI_API_KEY。这确保 LlamaIndex 使用提供的 OpenAI API 密钥创建一个 OpenAI 客户端,以访问 LLM 模型(GPT-3、GPT-3.5-turbo 和 GPT-4)和嵌入模型(text-embedding-ada-002、text-embedding-3-small 和 text- embeddings-3-large)等功能。

```
%env OPENAI\_API\_KEY=openai\_key\_here 
```

本教程中使用的数据来自 Hugging Face 数据集,特别是 AIatMongoDB/embedded_movies 数据集 [2]。电影数据集中的一个数据点包含与特定电影相对应的信息;为每个数据点捕获情节、流派、演员阵容、运行时间等。将数据集加载到开发环境后,将其转换为 Pandas 数据帧对象,可以相对轻松地进行数据结构操作和分析。

```
from datasets import load_dataset

import pandas as pd

# https://huggingface.co/datasets/AIatMongoDB/embedded\_movies

dataset=load_dataset(&quot;AIatMongoDB/embedded\_movies&quot;)

# Convert the dataset to a pandas dataframe

dataset_df=pd.DataFrame(dataset[&#x27;train&#x27;])

dataset_df.head(5)

```

#  步骤 3:数据清理、准备和加载

此步骤中的操作侧重于强制执行数据完整性和质量。第一个过程确保每个数据点的 plot 属性不为空,因为这是我们在嵌入过程中主要利用的数据。此步骤还确保我们从所有数据点中删除 plot_embedding 属性,因为这将被使用不同模型 text-embedding-3-small 创建的新嵌入所替换。

```
# Remove data point where plot column is missing

dataset_df=dataset_df.dropna(subset=[&#x27;plot&#x27;])

print(&quot;\nNumber of missing values in each column after removal:&quot;)

print(dataset_df.isnull().sum())

# Remove the plot_embedding from each data point in the dataset as we are going to create new embeddings with the new OpenAI embedding Model &quot;text-embedding-3-small&quot;

dataset_df=dataset_df.drop(columns=[&#x27;plot_embedding&#x27;])

dataset_df.head(5)

```

从 llama_index.embeddings 模块的 OpenAIEmbedding 模型初始化一个嵌入对象。具体来说, OpenAIEmbedding 模型接受两个参数:嵌入模型名称(对于本教程为 text-embedding-3-small )和向量嵌入的维度。

下面的代码片段配置了整个开发环境中使用的嵌入模型和 LLM。用于响应用户查询的 LLM 是通过 LlamaIndex 启用的默认 OpenAI 模型,使用 OpenAI() 类初始化。为了确保所有 LLM 使用者及其配置的一致性,LlamaIndex 提供了"Settings"模块,可以在环境中全局配置使用的 LLM 和嵌入模型。

```
from llama_index.core.settings import Settings

from llama_index.llms.openai import OpenAI

from llama_index.embeddings.openai import OpenAIEmbedding

embed_model=OpenAIEmbedding(model=&quot;text-embedding-3-small&quot;,dimensions=256)

llm=OpenAI()

Settings.llm=llm

Settings.embed_model=embed_model

```

接下来,为了将数据集及其内容适当地格式化以供 MongoDB 摄取,至关重要。在接下来的步骤中,我们将把当前数据集的结构 dataset_df (目前是一个 DataFrame 对象)转换为 JSON 字符串。这个数据集转换是在代码行 documents = dataset_df.to_json(orient='records') 中完成的,它将 JSON 格式分配给文档。

通过指定 orient='records',DataFrame 的每一行都转换为一个单独的 JSON 对象。

下一步创建一个 Python 字典列表 documents_list ,每个字典表示原始 DataFrame 中的一个单独记录。此过程的最后一步是将每个字典转换为手动构造的文档,这些文档是保存从数据源提取的信息的一级公民。LlamaIndex 中的文档保存信息,例如在 RAG 管道的下游处理和摄取阶段中使用的元数据。

需要注意的一点是,在手动创建 LlamaIndex 文档时,可以配置文档的属性,这些属性在作为嵌入模型和 LLM 的输入传递时使用。document 类构造函数上的 excluded_llm_metadata_keys 和 excluded_embed_metadata_keys 参数接受要在生成 RAG 管道内下游过程的输入时忽略的属性列表。这样做的原因是限制在嵌入模型中使用的上下文以获得更相关的检索,并且在 LLM 的情况下,这用于控制与用户查询结合的元数据信息。如果没有配置这两个参数中的任何一个,默认情况下,文档会在其元数据中使用所有内容作为嵌入和 LLM 输入。

在此步骤结束时,Python 列表包含与预处理数据集中每个数据点相对应的几个文档。

```
import json
from llama_index.core import Document
from llama_index.core.schema import MetadataMode

# Convert the DataFrame to a JSON string representation
documents_json = dataset_df.to_json(orient=&#x27;records&#x27;)
# Load the JSON string into a Python list of dictionaries
documents_list = json.loads(documents_json)

llama_documents = []

for document in documents_list:

  # Value for metadata must be one of (str, int, float, None)
  document[&quot;writers&quot;] = json.dumps(document[&quot;writers&quot;])
  document[&quot;languages&quot;] = json.dumps(document[&quot;languages&quot;])
  document[&quot;genres&quot;] = json.dumps(document[&quot;genres&quot;])
  document[&quot;cast&quot;] = json.dumps(document[&quot;cast&quot;])
  document[&quot;directors&quot;] = json.dumps(document[&quot;directors&quot;])
  document[&quot;countries&quot;] = json.dumps(document[&quot;countries&quot;])
  document[&quot;imdb&quot;] = json.dumps(document[&quot;imdb&quot;])
  document[&quot;awards&quot;] = json.dumps(document[&quot;awards&quot;])


  # Create a Document object with the text and excluded metadata for llm and embedding models
  llama_document = Document(
      text=document[&quot;fullplot&quot;],
      metadata=document,
      excluded_llm_metadata_keys=[&quot;fullplot&quot;, &quot;metacritic&quot;],
      excluded_embed_metadata_keys=[&quot;fullplot&quot;, &quot;metacritic&quot;, &quot;poster&quot;, &quot;num_mflix_comments&quot;, &quot;runtime&quot;, &quot;rated&quot;],
      metadata_template=&quot;{key}=&gt;{value}&quot;,
      text_template=&quot;Metadata: {metadata_str}\n-----\nContent: {content}&quot;,
      )

  llama_documents.append(llama_document)

# Observing an example of what the LLM and Embedding model receive as input
print(
    &quot;\nThe LLM sees this: \n&quot;,
    llama_documents[0].get_content(metadata_mode=MetadataMode.LLM),
)
print(
    &quot;\nThe Embedding model sees this: \n&quot;,
    llama_documents[0].get_content(metadata_mode=MetadataMode.EMBED),
)
```

处理数据之前的最后一步是将 LlamaIndex 文档列表转换为另一个一级公民数据结构,称为节点。一旦我们从文档中生成了节点,下一步就是使用文本和元数据属性中的内容为每个节点生成嵌入数据。

```
from llama_index.core.node_parser import SentenceSplitter

parser = SentenceSplitter()
nodes = parser.get_nodes_from_documents(llama_documents)

for node in nodes:
    node_embedding = embed_model.get_text_embedding(
        node.get_content(metadata_mode=&quot;all&quot;)
    )
    node.embedding = node_embedding
     
```

#  **步骤 4:数据库设置和连接 **

在继续之前,请确保满足以下先决条件:

+   在 MongoDB Atlas 上设置数据库集群

+   获取集群的 URI

**有关数据库集群设置和获取** URI 的帮助，请参阅**我们的指南 设置**MongoDB集群 [3]，以及我们的指南 获取你的连接字 [4]

成功创建集群后,通过单击"+ 创建数据库"在 MongoDB Atlas 集群中创建数据库和集合。数据库将被命名为 movies ,集合将被命名为 movies_records 。

![](img/b3162b1a8ad7d37a9e78e531634f9908.png)

在设置数据库并获得 Atlas 集群连接 URI 后,在您的开发环境中安全地存储 URI。

本指南使用 Google Colab,它提供了一个功能,可以安全地存储环境机密。然后可以在开发环境中访问这些机密。具体来说,代码 mongo_uri = userdata.get('MONGO_URI') 从安全存储中检索 URI。

下面的代码片段还使用 PyMongo 创建一个 MongoDB 客户端对象,表示与集群的连接并允许访问其数据库和集合。

```

import pymongo
from google.colab import userdata

def get_mongo_client(mongo_uri):
  &quot;&quot;&quot;Establish connection to the MongoDB.&quot;&quot;&quot;
  try:
    client = pymongo.MongoClient(mongo_uri)
    print(&quot;Connection to MongoDB successful&quot;)
    return client
  except pymongo.errors.ConnectionFailure as e:
    print(f&quot;Connection failed: {e}&quot;)
    return None

mongo_uri = userdata.get(&#x27;MONGO_URI_2&#x27;)
if not mongo_uri:
  print(&quot;MONGO_URI not set in environment variables&quot;)

mongo_client = get_mongo_client(mongo_uri)

DB_NAME=&quot;movies&quot;
COLLECTION_NAME=&quot;movies_records&quot;

db = mongo_client[DB_NAME]
collection = db[COLLECTION_NAME]
```

以下代码通过对集合执行 delete_many() 操作来确保当前数据库集合为空。

```
# To ensure we are working with a fresh collection 
# delete any existing records in the collection

collection.delete_many({})

```

#  **步骤 5:向量搜索索引创建 **

在 movies_records 集合中创建向量搜索索引对于从 MongoDB 有效地检索文档到我们的开发环境至关重要。要实现这一点,请参考关于创建向量搜索索引的官方指南。

在使用 MongoDB Atlas 上的 JSON 编辑器创建向量搜索索引时,请确保您的向量搜索索引名为 vector_index ,并且向量搜索索引定义如下:

```
{
 &quot;fields&quot;: [
   {
     &quot;numDimensions&quot;: 256,
     &quot;path&quot;: &quot;embedding&quot;,
     &quot;similarity&quot;: &quot;cosine&quot;,
     &quot;type&quot;: &quot;vector&quot;
   }
 ]
}
```

设置好向量搜索索引后,就可以高效地摄取和检索数据了。利用 LlamaIndex,数据摄取是一个简单的过程,只需不到三行代码即可实现。

#  步骤 6:将数据摄取到向量数据库

到目前为止,我们已经成功完成了以下工作:

+   加载了来自 Hugging Face 的数据源

+   使用 OpenAI 嵌入模型为每个数据点提供了嵌入

+   设置了一个旨在存储向量嵌入的 MongoDB 数据库

+   从我们的开发环境建立了与该数据库的连接

+   定义了一个向量搜索索引,用于高效查询向量嵌入

下面的代码片段还通过 LlamaIndex 构造函数 MongoDBAtlasVectorSearch 初始化了一个 MongoDB Atlas 向量存储对象。需要注意的是,在此步骤中,我们引用了之前通过 MongoDB Cloud Atlas 界面创建的向量搜索索引的名称。对于这个特定的用例,索引名称是 vector_index 。

将节点摄取到指定向量存储的关键方法是 LlamaIndex MongoDB 实例的 .add() 方法。

```
from llama_index.vector_stores.mongodb import MongoDBAtlasVectorSearch

vector_store = MongoDBAtlasVectorSearch(mongo_client, db_name=DB_NAME, collection_name=COLLECTION_NAME, index_name=&quot;vector_index&quot;)
vector_store.add(nodes)

```

上面代码片段中的最后一行创建了一个 LlamaIndex 索引。在 LlamaIndex 中,当文档被加载到任何索引抽象类( SummaryIndex 、 TreeIndex 、 KnowledgeGraphIndex ,尤其是 VectorStoreIndex )时,会在内存向量存储中构建一个存储原始文档表示的索引,该向量存储还存储嵌入。

但是由于本 RAG 系统中使用了 MongoDB Atlas 向量数据库来存储嵌入和我们文档的索引,因此 LlamaIndex 通过 VectorStoreIndex 类的 from_vector_store 方法实现了从 Atlas 中检索索引。

```
from llama_index.core import VectorStoreIndex, StorageContext
index = VectorStoreIndex.from_vector_store(vector_store)
```

#  **步骤 7:处理用户查询 **

下一步是创建一个 LlamaIndex 查询引擎。查询引擎实现了使用自然语言从数据索引中检索相关、上下文适当信息的功能。LlamaIndex 提供的 as_query_engine 方法抽象了 AI 工程师和开发人员编写实现代码以适当处理查询以从数据源提取信息的复杂性。

对于我们的用例,查询引擎满足了构建问答应用程序的要求。但是**,LlamaI**ndex 确实提供了使用 聊天引擎功能 [5]构建类似聊天的应用程序的能力。

```

import pprint
from llama_index.core.response.notebook_utils import display_response

query_engine = index.as_query_engine(similarity_top_k=3)
query = &quot;Recommend a romantic movie suitable for the christmas season and justify your selecton&quot;
response = query_engine.query(query)
display_response(response)
pprint.pprint(response.source_nodes)

```

#  **结论 **

在现代生成式 AI 应用程序中,结合 RAG 架构设计模式可以提高 LLM 的性能,并引入一种在构建强大 AI 基础设施时考虑成本的方法。如本文所示,使用 MongoDB 作为向量数据库和 LlamaIndex 作为 LLM 编排器等组件,构建一个具有最少代码实现的强大 RAG 系统是一个简单的过程。

特别是,本教程介绍了利用 Python、OpenAI、LlamaIndex 和 MongoDB 向量数据库(也称为 POLM AI 技术栈)的组合功能实现 RAG 系统。

需要提到的是,微调仍然是提高 LLM 能力和更新其参数化知识的可行策略。但是,对于考虑构建和维护 GenAI 应用程序的经济性的 AI 工程师来说,探索提高 LLM 能力的更具成本效益的方法值得考虑,即使它是实验性的。

数据采集、硬件加速器的获取以及微调 LLM 和基础模型所需的领域专业知识相关的成本通常需要大量投资,这使得探索更具成本效益的方法(如 RAG 系统)成为一个有吸引力的替代方案。

值得注意的是,微调和模型训练的成本影响凸显了 AI 工程师和开发人员从 AI 项目的早期阶段就采用节省成本的思维方式的必要性。今天的大多数应用程序已经或将具有某种形式的生成式 AI 能力,由 AI 基础设施支持。因此,在开发 AI 基础设施时,向利益相关者和关键决策者传达和表达探索具有成本效益的解决方案的价值成为 AI 工程师角色的一个关键方面。

#  **常见问题: **

问:什么是检索增强生成(RAG)系统? 检索增强生成(RAG)是一种设计模式,通过使用检索模型从数据库中获取语义相关信息来提高 LLM 的能力。这些额外的上下文与用户的查询相结合,以从 LLM 生成更准确和相关的响应。

问:RAG 系统中 AI 技术栈的关键组件是什么? 基本组件包括模型(如 GPT-3.5、GPT-4 或 Llama)、用于管理 LLM 和数据源之间交互的编排器或集成器,以及用于高效存储和检索数据的操作型和向量数据库。

###  **相关链接 **

[1]https://github.com/mongodb-developer/GenAI-Showcase/blob/main/notebooks/rag/naive_rag_implemenation_llamaindex.ipynb

[2]https://huggingface.co/datasets/MongoDB/embedded_movies

[3]https://www.mongodb.com/docs/guides/atlas/cluster/

[4]https://www.mongodb.com/docs/guides/atlas/connection-string/

[5]https://docs.llamaindex.ai/en/latest/module_guides/deploying/chat_engines/root.html