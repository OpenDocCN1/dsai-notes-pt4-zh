
# 法律检索（项目）本地知识库问答模型搭建

> 来源：[https://v11enp9ok1h.feishu.cn/docx/IfgxdwDeToXjcQxWMQVcrQYCn4e](https://v11enp9ok1h.feishu.cn/docx/IfgxdwDeToXjcQxWMQVcrQYCn4e)

## 一、常见本地知识库问答流程

### 整体框架

![](img/fe41e332f8eaa9772041a2eaba23dffc.png)

**改进的点（方向）：**

- 改LLM模型
- `embedding`模型
- 文本分割方式
- 多卡加速模型部署
- 提升`top-k`检索召回的质量
- 基于数据隐私和私有化部署，可以方便的使用`Langchain`+大模型进行推理

### 文本切分

`Langchain`源码：[源码](https://v11enp9ok1h.feishu.cn/wiki/ZK5awPdPtiZFc4k5j5Gc44r9nre)

`Langchain`的[内置文本](https://so.csdn.net/so/search?q=%E5%86%85%E7%BD%AE%E6%96%87%E6%9C%AC&spm=1001.2101.3001.7020)拆分模块的常见参数：

- `chunk_size`：文本块的大小，即文本块的最大尺寸；
- `chunk_overlap`：表示两个切分文本之间的重合度，文本块之间的最大重叠量，保留一些重叠可以保持文本块之间的连续性，可以使用滑动窗口进行构造，这个比较重要。
- `length_function`：用于计算文本块长度的方法，默认为简单的计算字符数；

**其他文本分割器：**

> 我们直接举个栗子，比如对中文文本切分，继承`langchain`中类`CharacterTextSplitter`的`ChineseTextSplitter`类。正则表达式`sent_sep_pattern`来匹配中文句子的分隔符（如句号，感叹号，问好，分号等）：

```
# 中文文本切分类
class ChineseTextSplitter(CharacterTextSplitter):
    def __init__(self, pdf: bool = False, **kwargs):
        super().__init__(**kwargs)
        self.pdf = pdf

    def split_text(self, text: str) -> List[str]:
        if self.pdf:
            text = re.sub(r"\n{3,}", "\n", text)
            text = re.sub(r'\s', ' ', text)
            text = text.replace("\n\n", "")
        sent_sep_pattern = re.compile('([﹒﹔﹖﹗．。！？]["’”」』]{0,2}|(?=["‘“「『]{1,2}|$))')  # del ：；
        sent_list = []
        for ele in sent_sep_pattern.split(text):
            if sent_sep_pattern.match(ele) and sent_list:
                sent_list[-1] += ele
            elif ele:
                sent_list.append(ele)
        return sent_list
```

### 图解流程

![](img/7eef17ee0f51439dcc25103609888d55.png)

## 二、相关RAG综述/工作

![](img/0a0354a56a08765ba81d501ac9566038.png)

### Retrieval Source知识来源

- 训练语料：有标注的训练数据直接作为外部知识；
- 外部数据：支持提供训练数据之外的外部知识作为检索来源，比如于任务相关的领域数据，实现模型的快速适应；
- 无监督数据：前两种知识源都需要一定的人工标注来完善“检索依据-输出”的对齐工作，无监督知识源可以直接支持无标注/对齐的知识作为检索来源；

### Retrieval Metrics召回的度量标准

- 浅层语义检索：针对稀疏向量场景的度量方法，比如TF-IDF, BM25等；
- 深层语义检索： 针对稠密向量的度量方法，比如文本相似度；
- 特定任务检索：在通用的度量场景下，度量得分高并不能代表召回知识准确，因此有学者提出基于特定任务优化的召回度量方法，提高度量的准确率；

### Integration集成的方法

- 数据增强：直接拼接用户输入文本和知识文本，然后输入文本生成模型；
- 注意力机制：引入额外的Encoder，对用户输入文本和知识文本进行注意力编码后输入文本生成模型；
- 架抽取：前两种方法都是通过文本向量化的隐式方法完成知识重点片段的抽取，skeleton extraction方法可以显式地完成类似工作；

该工作针对后续优化方向提出了一些观点，认为：

RATG效果强依赖于召回知识的准确率，这个问题在`query`和目标知识片段的文本相似度较低的情况尤其为突出；

- 需要做好存储的容量和召回效率之间的平衡；
- 逻辑上，召回度量模块和文本生成模块的联合训练会有更好的效果；但是在这样的框架下，其训练和预测之间会存在较大的Gap：训练时，模型只针对少量标注的知识源完成优化（Local）；而预测时，模型是针对大量的知识源完成度量（Global）；
- 此外，针对目前都基于通用的文本语义完成知识召回，后续可以考虑关注基于任务层级的召回方法的多样化研究，以及不同召回方法的调用的灵活性和可控制性，针对多模态知识源召回方法的优化；

## 三、HIT的RAG+知识融合综述

### Retrieval-Augmented-LLM

检索增强LLM：给LLM提供外部数据库，对于用户问题 ( Query )，通过一些信息检索 ( Information Retrieval, IR ) 的技术，先从外部数据库中检索出和用户问题相关的信息，然后让 LLM 结合这些相关信息来生成结果。

![](img/dafb4225352592bc538bcfaafd290e05.png)

## 四、基于检索的大语言模型

### 架构：What & How & When

![](img/a450bd1f0c0eca05bf243b54c9e4e089.png)

### 如何训练

现有工作中对于检索增强的语言模型被证实的比较work的训练方式可划分为以下四类：

- Independent training：语言模型与检索器均是独立训练的。
- Sequential training：独立训练单个组件后将其固定，另一个组件根据此组件任务目标进行训练。
- Joint training w/ asynchronous index update：允许索引是“过时”的，即每隔T步才重新更新检索索引。
- Joint training w/ in-batch approximation：使用“批内索引”，而不是完整知识库中的全部索引。

### 应用

RAG常用在知识密集型任务上。

![](img/893fe310abf3a4c5cabd747bf4a2bdb9.png)

## 五、相关RAG项目

### chatlaw项目

1. `query`抽取为key word

![](img/5fdf41f9b567f6ec86682fafb1ae59df.png)

**参考：**

[ 北大团队发布大模型 ChatLaw](https://v11enp9ok1h.feishu.cn/wiki/ExnxwEMfhix8GNk7mAkcJPO3nyd)

![](img/d1d7143195d1afb924830e1c91a384b6.png)

1. 项目中的模型

- `ChatLaw-13B`，此版本为学术demo版，基于姜子牙`Ziya-LLaMA-13B-v1`训练而来，中文各项表现很好，但是逻辑复杂的法律问答效果不佳，需要用更大参数的模型
- `ChatLaw-33B`，此版本为学术demo版，基于`Anima-33B`训练而来，逻辑推理能力大幅提升，但是因为Anima的中文语料过少，导致问答时常会出现英文数据。
- `ChatLaw-Text2Vec`，使用93w条判决案例做成的数据集基于`BERT`训练了一个相似度匹配模型，可将用户提问信息和对应的法条相匹配，
- 例如：

```
“请问如果借款没还怎么办。”
“合同法(1999-03-15): 第二百零六条 借款人应当按照约定的期限返还借款。对借款期限没有约定或者约定不明确，依照本法第六十一条的规定仍不能确定的，借款人可以随时返还；贷款人可以催告借款人在合理期限内返还。”
两段文本的相似度计算为0.9960
```

1. 调度模型

将文件、音频、文字整合在一起，同时支持法律援助、法律文书、思维导图等多样化输出。

![](img/23ea254d3ebeb0194908dc4a23327841.png)

## 六、利用`LongContextRetriver`克服RAG中的中间丢失现象

### 【1】检索增强生成

当我们不想微调模型，但仍想从自定义知识库中生成答案时，RAG特别有用。想象一下，我们有多个音频和视频，那么我们如何使用一种机制来帮助从这些来源检索信息，并根据用户查询生成见解。

**RAG由检索和生成组件组成：**

- 对于检索，我们使用一种称为语义搜索的技术；
- 对于Generation，我们使用大型语言模型来查看检索到的块，并生成类似人类的响应。

> 无论我们使用什么样的大模型，当检索到10多个文档时，都会出现显著的性能下降。简言之：当模型必须在长上下文中访问相关信息时，它们往往会忽略所提供的文档。

### 【2】实现更好的RAG系统以避免LIM（中间丢失）的技术

- 为每个文档创建一个知识库是不可取的，这样基于语义相似性的检索模型难以正确检索到相关的上下文集合；
- 为了在一定程度上避免LIM问题，我们将创建不同的`VectorStore`，并使用`MergerRetriever`将其合并在一起；
- 然后，我们将使用`LongContextReorder`重新排序结果以避免性能下降

### 【3】合并检索器

`MergerRetriever`以列表的形式输入多个检索器，并将通过`get_relevant_documents()`方法获取的结果合并到一个列表中。合并后的结果将是与查询相关的文档列表，这些文档已由不同的检索器进行了排序。

`MergerRetriever`类可以通过多种方式用于提高文档检索的准确性：

- 首先，它可以组合多个检索器的结果，这有助于降低结果中出现偏差的风险；
- 其次，它可以对不同检索器的结果进行排序，这有助于确保首先返回最相关的文档。

### 【4】准备工作

`Chromadb`：用于存储文本嵌入的矢量存储。

三种不同的嵌入模型促进高级语义分析，模型如下：

```
BAAI/bge-small-en-v1.5
Sentence-transformers/all-MiniLM-L6-v2 
OpenAI Embeddigs
```

> `Langchain`：使用大型语言模型（LLM）进行应用程序开发的框架。

> `Zephyr-7B Beta`：作为应用程序核心组件的大型语言模型。

> 上下文压缩：通过基于查询上下文压缩和过滤文档来解决检索中的挑战。在检索过程中涉及一个基本检索器和一个文档压缩器。

### 【5】代码实现

#### 5.1 安装所需的包

```
%capture
!pip install -qU langchain chromadb huggingface_hub sentence-transformers pypdf cohere openai tiktoken
```

#### 5.2 导入所需的包

```
import os
from langchain.document_transformers import (    
    EmbeddingsClusteringFilter,   
    EmbeddingsRedundantFilter,
    )
from langchain.embeddings import HuggingFaceEmbeddings, OpenAIEmbeddings, HuggngFaceBgeEmbeddings
from langchain.retrievers import ContextualCompressionRetriever
from langchain.retrievers.document_compressors import DocumentCompressorPipeline
from langchain.document_transformers import LongContextReorder
from langchain.retrievers.merger_retriever import MergerRetriever
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.vectorstores import FAISS, Chroma
from langchain.document_loaders import PyPDFLoader
```

#### 5.3 设置OpenAI Key

```
import os
from getpass import getpass
import openai
#
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
if not OPENAI_API_KEY:
    OPENAI_API_KEY = getpass("Enter your OpenAI API key: ")
openai.api_key = OPENAI_API_KEY
```

#### 5.4 设置Embedding模型

```
hf_embeddings = HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2",                                      model_kwargs={"device":"cpu"},                                      encode_kwargs = {'normalize_embeddings': False})hf_bge_embeddings = HuggingFaceBgeEmbeddings(model_name="BAAI/bge-large-en",                                             model_kwargs={"device":"cpu"},                                             encode_kwargs = {'normalize_embeddings': False})openai_embeddings = OpenAIEmbeddings(openai_api_key=OPENAI_API_KEY)
```

#### 5.5 数据处理

```
loader_mh  = PyPDFLoader("/content/mental_health_Document.pdf")documnet_mh = loader_mh.load()print(len(documnet_mh))loader_esops = PyPDFLoader("/content/Employee-Stock-Option-Plans-ESOP-Best-Practices-2.pdf")documnet_esops = loader_esops.load()print(len(documnet_esops))
#####OUTPUT############1344
```

<https://v11enp9ok1h.feishu.cn/sync/Ipw1d27gNsv3OAbo3N7cks4ZnJf>
