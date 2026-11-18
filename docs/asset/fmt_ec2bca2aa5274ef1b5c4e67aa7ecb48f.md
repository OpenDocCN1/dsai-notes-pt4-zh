
# 使用LangChain搭建知识库

> 来源：[https://datawhaler.feishu.cn/docx/H1Dzd3G65oA2VExWKgPcugv0nZb](https://datawhaler.feishu.cn/docx/H1Dzd3G65oA2VExWKgPcugv0nZb)
>
> 本章节内容由[llm-universe](https://github.com/datawhalechina/llm-universe/tree/main)搬运，可以去该项目的notebook中找到前者内容，欢迎反馈
>
> 环境配置请参考`requirements.txt`

## 向量及向量知识库

### 一、词向量与向量

#### 1. 什么是词向量

![](img/380c7c793553b00ab3bd44dc08711d20.png)

在机器学习和自然语言处理（NLP）中，词向量（word embedding）是一种以单词为单位将每个单词转化为实数向量的技术。这些实数向量可以被计算机更好地理解和处理。

词向量背后的主要想法是，相似或相关的对象在向量空间中的距离应该很近。

![](img/ee7ddcc49cba72122ecdf2f9edae0af3.png)

举个例子，我们可以使用词向量来表示文本数据。在词向量中，每个单词被转换为一个向量，这个向量捕获了这个单词的语义信息。例如，`"king"` 和 `"queen"` 这两个单词在向量空间中的位置将会非常接近，因为它们的含义相似。而 `"apple"` 和 `"orange"` 也会很接近，因为它们都是水果。而 `"king"` 和 `"apple"` 这两个单词在向量空间中的距离就会比较远，因为它们的含义不同。

#### 2. 通用文本向量

词向量实际上是将单词转化为固定的静态的向量，虽然可以在一定程度上捕捉并表达文本中的语义信息，但忽略了单词在不同语境中的意思会受到影响这一现实。因此在RAG应用中使用的向量技术一般为通用文本向量（Universal text embedding），该技术可以对一定范围内任意长度的文本进行向量化，与词向量不同的是向量化的单位不再是单词而是输入的文本，输出的向量会捕捉更多的语义信息。

#### 3. 向量的优势

在RAG（Retrieval Augmented Generation，检索增强生成）方面向量的优势主要有两点：

+ 向量比文字更适合检索。当我们在数据库检索时，如果数据库存储的是文字，主要通过检索关键词（词法搜索）等方法找到相对匹配的数据，匹配的程度是取决于关键词的数量或者是否完全匹配查询句的；但是向量中包含了原文本的语义信息，可以通过计算问题与数据库中数据的点积、余弦距离、欧几里得距离等指标，直接获取问题与数据在语义层面上的相似度；

+ 向量比其它媒介的综合信息能力更强，当传统数据库存储文字、声音、图像、视频等多种媒介时，很难去将上述多种媒介构建起关联与跨模态的查询方法；但是向量却可以通过多种向量模型将多种数据映射成统一的向量形式。

#### 4. 一般构建向量的方法

在搭建 RAG 系统时，我们往往可以通过使用向量模型来构建向量，我们可以选择：

+ 使用各个公司的 `Embedding API`；
+ 在本地使用向量模型将数据构建为向量。

### 二、向量数据库

#### 1. 什么是向量数据库

向量数据库是用于高效计算和管理大量向量数据的解决方案。向量数据库是一种专门用于存储和检索向量数据（`embedding`）的数据库系统。它与传统的基于关系模型的数据库不同，它主要关注的是向量数据的特性和相似性。

在向量数据库中，数据被表示为向量形式，每个向量代表一个数据项。这些向量可以是数字、文本、图像或其他类型的数据。向量数据库使用高效的索引和查询算法来加速向量数据的存储和检索过程。

#### 2. 向量数据库的原理及核心优势

向量数据库中的数据以向量作为基本单位，对向量进行存储、处理及检索。向量数据库通过计算与目标向量的余弦距离、点积等获取与目标向量的相似度。当处理大量甚至海量的向量数据时，向量数据库索引和查询算法的效率明显高于传统数据库。

#### 3. 主流的向量数据库

+ [`Chroma`](https://www.trychroma.com/)：是一个轻量级向量数据库，拥有丰富的功能和简单的 API，具有简单、易用、轻量的优点，但功能相对简单且不支持GPU加速，适合初学者使用。
+ [`Weaviate`](https://weaviate.io/)：是一个开源向量数据库。除了支持相似度搜索和最大边际相关性（MMR，Maximal Marginal Relevance）搜索外还可以支持结合多种搜索算法（基于词法搜索、向量搜索）的混合搜索，从而搜索提高结果的相关性和准确性。
+ [`Qdrant`](https://qdrant.tech/)：Qdrant使用 Rust 语言开发，有极高的检索效率和RPS（Requests Per Second），支持本地运行、部署在本地服务器及Qdrant云三种部署模式。且可以通过为页面内容和元数据制定不同的键来复用数据。

## 使用Embedding API

注：为了方便`embedding api`调用，应将密钥填入`llm_universe`下的`.env`文件，代码将自动读取并加载环境变量。

### 一、使用OpenAI API

GPT有封装好的接口，我们简单封装即可。目前GPT embedding mode有三种，性能如下所示：

| 模型 | 每美元页数 | [`MTEB`](https://github.com/embeddings-benchmark/mteb)得分 | [`MIRACL`](https://github.com/project-miracl/miracl)得分 |
| --- | --- | --- | --- |
| `text-embedding-3-large` | 9,615 | 64.6 | 54.9 |
| `text-embedding-3-small` | 62,500 | 62.3 | 44.0 |
| `text-embedding-ada-002` | 12,500 | 61.0 | 31.4 |

+ `MTEB`得分为`embedding model`分类、聚类、配对等八个任务的平均得分。
+ `MIRACL`得分为`embedding model`在检索任务上的平均得分。

从以上三个`embedding model`我们可以看出`text-embedding-3-large`有最好的性能和最贵的价格，当我们搭建的应用需要更好的表现且成本充足的情况下可以使用；`text-embedding-3-small`有着较好的性能跟价格，当我们预算有限时可以选择该模型；而`text-embedding-ada-002`是OpenAI上一代的模型，无论在性能还是价格都不如及前两者，因此不推荐使用。

```python
import os
from openai import OpenAI
from dotenv import load_dotenv, find_dotenv


# 读取本地/项目的环境变量。
# find_dotenv()寻找并定位.env文件的路径
# load_dotenv()读取该.env文件，并将其中的环境变量加载到当前的运行环境中  
# 如果你设置的是全局的环境变量，这行代码则没有任何作用。
_ = load_dotenv(find_dotenv())

# 如果你需要通过代理端口访问，你需要如下配置
# os.environ['HTTPS_PROXY'] = 'http://127.0.0.1:7890'
# os.environ["HTTP_PROXY"] = "http://127.0.0.1:7890"

def openai_embedding(text: str, model: str = None):
    # 获取环境变量 OPENAI_API_KEY
    api_key = os.environ['OPENAI_API_KEY']
    client = OpenAI(api_key=api_key)

    # embedding model：'text-embedding-3-small', 'text-embedding-3-large', 'text-embedding-ada-002'
    if model == None:
        model = "text-embedding-3-small"

    response = client.embeddings.create(
        input=text,
        model=model
    )
    return response


response = openai_embedding(text='要生成 embedding 的输入文本，字符串形式。')
```

API返回的数据为`json`格式，除`object`向量类型外还有存放数据的`data`、`embedding model` 型号`model`以及本次`token`使用情况`usage`等数据，具体如下所示：

```json
{
  "object": "list",
  "data": [
    {
      "object": "embedding",
      "index": 0,
      "embedding": [
        -0.006929283495992422,
        ... (省略)
        -4.547132266452536e-05,
      ],
    }
  ],
  "model": "text-embedding-3-small",
  "usage": {
    "prompt_tokens": 5,
    "total_tokens": 5
  }
}
```

我们可以调用`response`的`object`来获取`embedding`的类型。

```python
print(f'返回的embedding类型为：{response.object}')
```

```
返回的embedding类型为：list
```

`embedding`存放在`data`中，我们可以查看`embedding`的长度及生成的`embedding`。

```python
print(f'embedding长度为：{len(response.data[0].embedding)}')
print(f'embedding（前10）为：{response.data[0].embedding[:10]}')
```

```
embedding长度为：1536
embedding（前10）为：[0.038827355951070786, 0.013531949371099472, -0.0025024667847901583, -0.016542360186576843, 0.02412303350865841, -0.017386866733431816, 0.042086150497198105, 0.011515072546899319, -0.0282362699508667, -0.006800748407840729]
```

我们也可以查看此次`embedding`的模型及`token`使用情况。

```python
print(f'本次embedding model为：{response.model}')
print(f'本次token使用情况为：{response.usage}')
```

```
本次embedding model为：text-embedding-3-small
本次token使用情况为：Usage(prompt_tokens=12, total_tokens=12)
```

### 二、使用文心千帆API

`Embedding-V1`是基于百度文心大模型技术的文本表示模型，`Access token`为调用接口的凭证，使用`Embedding-V1`时应先凭`API Key`、`Secret Key`获取`Access token`，再通过`Access token`调用接口来`embedding text`。同时千帆大模型平台还支持`bge-large-zh`等`embedding model`。

```python
import requests
import json


def wenxin_embedding(text: str):
    # 获取环境变量 wenxin_api_key、wenxin_secret_key
    api_key = os.environ['QIANFAN_AK']
    secret_key = os.environ['QIANFAN_SK']

    # 使用API Key、Secret Key向https://aip.baidubce.com/oauth/2.0/token 获取Access token
    url = "https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id={0}&client_secret={1}".format(api_key, secret_key)
    payload = json.dumps("")
    headers = {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
    }
    response = requests.request("POST", url, headers=headers, data=payload)
    
    # 通过获取的Access token 来embedding text
    url = "https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/embeddings/embedding-v1?access_token=" + str(response.json().get("access_token"))
    input = []
    input.append(text)
    payload = json.dumps({
        "input": input
    })
    headers = {
        'Content-Type': 'application/json'
    }

    response = requests.request("POST", url, headers=headers, data=payload)

    return json.loads(response.text)


# text应为List(string)
text = "要生成 embedding 的输入文本，字符串形式。"
response = wenxin_embedding(text=text)
```

`Embedding-V1`每次`embedding`除了有单独的`id`外，还有时间戳记录`embedding`的时间。

```python
print('本次embedding id为：{}'.format(response['id']))
print('本次embedding产生时间戳为：{}'.format(response['created']))
```

```
本次embedding id为：as-h7u5sde3ga
本次embedding产生时间戳为：1741091715
```

同样的我们也可以从`response`中获取`embedding`的类型和`embedding`。

```python
print('返回的embedding类型为:{}'.format(response['object']))
print('embedding长度为：{}'.format(len(response['data'][0]['embedding'])))
print('embedding（前10）为：{}'.format(response['data'][0]['embedding'][:10]))
```

```
返回的embedding类型为:embedding_list
embedding长度为：384
embedding（前10）为：[0.060567744076251984, 0.020958080887794495, 0.053234219551086426, 0.02243831567466259, -0.024505289271473885, -0.09820500761270523, 0.04375714063644409, -0.009092536754906178, -0.020122773945331573, 0.015808865427970886]
```

### 三、使用讯飞星火API

使用讯飞 `embedding` 时需要注意`spark_embedding_domain`参数，该参数在向量化问题时为 `"query"` 在向量化知识库时为 `"para"`。
