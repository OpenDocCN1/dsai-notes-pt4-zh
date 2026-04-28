# 【LLM-RAG】知识库问答 | 检索 | embedding

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Swr6dhB3BoL8xxxpl86ceE8tn8b](https://qqs7y1hozd1.feishu.cn/docx/Swr6dhB3BoL8xxxpl86ceE8tn8b)

## 一、常见本地知识库问答流程

### 1 整体框架

> 项目过程包括**加载文件****、****读取文本****、****文本分割****、****文本向量化****、****问句向量化**

> **在文本向量**中匹配出与问句向量相似的top k 个、匹配出的文本作为上下文和问题一起添加到prompt中、提交给LLM生成回答。

![](img/7de7f31b63184c7f23abe9d58e4e2d5f.png)

**改进的点（方向）：**

+   embedding模型

+   **多卡加速模型部署**

+   提升top-k检索召回的质量

**基于数据隐私和私有化部署，可以方便的**使用Langchain+大模型进行推理。

### 文本切分

Langchain的内置文本拆分模块的常见参数：

+   **chunk_size：**文本块的大小，即文本块的最大尺寸；

+   **chunk_overlap：**表示两个切分文本之间的重合度，文本块之间的最大重叠量，保留一些重叠可以保持文本块之间的连续性，可以使用滑动窗口进行构造，这个比较重要。

+   length_function：用于计算文本块长度的方法，默认为简单的计算字符数；

**其他文本分割器：**

![](img/d0a6ebe79eaab58bdc52c67b490e9381.png)

**举个例子：**

比如对中文文本切分，继承langchain中类CharacterTextSplitter的ChineseTextSplitter类。

正则表达式sent_sep_pattern来匹配中文句子的分隔符（如句号，感叹号，问号，分号等）：

```
# 中文文本切分类
class ChineseTextSplitter(CharacterTextSplitter):
    def __init__(self, pdf: bool = False, **kwargs):
        super().__init__(**kwargs)
        self.pdf = pdf

    def split_text(self, text: str) -&gt; List[str]:
        if self.pdf:
            text = re.sub(r&quot;\n{3,}&quot;, &quot;\n&quot;, text)
            text = re.sub(&#x27;\s&#x27;, &#x27; &#x27;, text)
            text = text.replace(&quot;\n\n&quot;, &quot;&quot;)
        sent_sep_pattern = re.compile(&#x27;([﹒﹔﹖﹗．。！？][&quot;’”」』]{0,2}|(?=[&quot;‘“「『]{1,2}|$))&#x27;)  # del ：；
        sent_list = []
        for ele in sent_sep_pattern.split(text):
            if sent_sep_pattern.match(ele) and sent_list:
                sent_list[-1] += ele
            elif ele:
                sent_list.append(ele)
        return sent_list
```

### 图解流程

![](img/3ccb4144d957679236b31e51fd22235f.png)

## 二、相关RAG综述/工作

### 一篇RAG综述

**《A Survey on Retrieval-Augmen**ted Text Generation》

该工作旨在对检索增强文本生成进行研究。主要核心的点如下：

![](img/7d7edfcc47a1c0de20b6f15e2c42e944.png)

### 1 Retrieval Source知识来源

> **训练语料：**有标注的训练数据直接作为外部知识；

> **外部数据：**支持提供训练数据之外的外部知识作为检索来源，比如于任务相关的领域数据，实现模型的快速适应；

> **无监督数据：**前两种知识源都需要一定的人工标注来完善“检索依据-输出”的对齐工作，无监督知识源可以直接支持无标注/对齐的知识作为检索来源;

### 2 Retrieval Metrics召回的度量标准

> **浅层语义检索：**针对稀疏向量场景的度量方法，比如TF-IDF, BM25等；

> **深层语义检索：** 针对稠密向量的度量方法，比如文本相似度；

> **特定任务检索：**在通用的度量场景下，度量得分高并不能代表召回知识准确，因此有学者提出基于特定任务优化的召回度量方法，提高度量的准确率；

### 3 Integration集成的方法

> **数据增强：**直接拼接用户输入文本和知识文本，然后输入文本生成模型；

> **注意力机制：**引入额外的Encoder，对用户输入文本和知识文本进行注意力编码后输入文本生成模型；

> **架抽取：**前两种方法都是通过文本向量化的隐式方法完成知识重点片段的抽取，skeleton extraction方法可以显式地完成类似工作；

该工作针对后续优化方向提出了一些观点，认为：

> RATG效果强依赖于召回知识的准确率，这个问题在query和目标知识片段的文本相似度较低的情况尤为突出；

> 需要做好存储的容量和召回效率之间的平衡；

> **逻辑上，召回度量模块和文本生成模块的联合**训练会有更好的效果；但是在这样的框架下，其训练和预测之间会存在较大的Gap：训练时，模型只针对少量标注的知识源完成优化（Local）；而预测时，模型是针对大量的知识源完成度量（Global）；

> 此外，针对目前都基于通用的文本语义完成知识召回，后续可以考虑关注基于任务层级的召回方法的多样化研究，以及不同召回方法的调用的灵活性和可控制性，针对多模态知识源召回方法的优化；

### HIT的RAG+知识融合综述

**《Tr**ends in Integration of Knowledge and Large Language Models: A Survey and Taxonomy of Methods, Benchmarks, and Applications》

HIT解读：赛尔笔记 | 大语言模型与知识融合综述：方法、基准和应用

### Retrieval-Augmented-LLM

> **检索增强LLM：**给 LLM 提供外部数据库，对于用户问题 ( Query )，通过一些信息检索 ( Information Retrieval, IR ) 的技术，先从外部数据库中检索出和用户问题相关的信息，然后让 LLM 结合这些相关信息来生成结果。

![](img/677f66c9774058b16089dbeeb2416841.png)

### 1 架构：What & How & When

![](img/a0a5afdcf10e7e0eda2306c3cb0e1125.png)

### 2 如何训练

**现有工作中对于检索增强的语言模型被证实的**比较work的训练方式可划分为以下四类：

> Independent training：语言模型与检索器均是独立训练的。

> Sequential training：独立训练单个组件后将其固定，另一个组件根据此组件任务目标进行训练。

> **Joint training w/ asynch**ronous index update：允许索引是“过时”的，即每隔T步才重新更新检索索引。

> **Joint training w/ in-**batch approximation：使用“批内索引”，而不是完整知识库中的全部索引。

### 3 应用

**RAG常用在知识密集型任务上。**

![](img/d5ee69f7fa5bcecbb0b1ddbcfd9bdc2e.png)

### 解决RAG的中间丢失问题

> **来**自斯坦福大学、加州大学伯克利分校和Samaya AI的研究人员在论文《Lost in the Middle: How Language Models Use Long Contexts》中发现了一种类似于人类首要/近因效应的现象。这意味着，像人类一样，LLM更有可能回忆起位于输入开始或结束的信息，并倾向于忽略在中间的内容。

## 三、相关RAG项目

### chatlaw项目

#### 1 query抽取为key word

![](img/e9f2ac3726ce065fe2bc76b5f194343e.png)

![](img/441aaf391a94c40b330919f650e8d0d0.png)

#### 2 项目中的模型

> **ChatLaw-13B，**此版本为学术demo版，基于姜子牙Ziya-LLaMA-13B-v1训练而来，中文各项表现很好，但是逻辑复杂的法律问答效果不佳，需要用更大参数的模型来解决。

> **ChatLaw-33B，**此版本为学术demo版，基于Anima-33B训练而来，逻辑推理能力大幅提升，但是因为Anima的中文语料过少，导致问答时常会出现英文数据。

> ChatLaw-Text2Vec，使用93w条判决案例做成的数据集基于BERT训练了一个相似度匹配模型，可将用户提问信息和对应的法条相匹配。

**例如：**

> “请问如果借款没还怎么办。”

> “合同法(1999-03-15): 第二百零六条 借款人应当按照约定的期限返还借款。对借款期限没有约定或者约定不明确，依照本法第六十一条的规定仍不能确定的，借款人可以随时返还；贷款人可以催告借款人在合理期限内返还。”

两段文本的相似度计算为0.9960

#### 3 调度模型

**将文件、音频、文字整合在一起，同时支持法**律援助、法律文书、思维导图等多样化输出。

## 四、embedding模型相关

**FlagEmbedding**

![](img/d14abe8a9ed167fca672dca466b61f97.png)

![](img/d30932a1dc92417cde812c70996010dc.png)

### 七个emb模型总结

#### 1 BGE

> BGE，即BAAI General Embedding，是由智源研究院（BAAI）团队开发的一款文本Embedding模型。该模型可以将任何文本映射到低维密集向量，这些向量可用于检索、分类、聚类或语义搜索等任务。此外，它还可以用于LLMs的向量数据库。

BGE模型在2023年有多次更新，包括发布论文和数据集、发布新的reranker模型以及更新Embedding模型。BGE模型已经集成到Langchain中，用户可以方便地使用它。此外，BGE模型在MTEB和C-MTEB基准测试中都取得了第一名的成绩。

**BGE模型的主要特点如下：**

> **多语言支持：**BGE模型支持中英文。

> **多版本：**BGE模型有多个版本，包括bge-large-en、bge-base-en、bge-small-en等，以满足不同的需求。

> **高效的reranker：**BGE提供了reranker模型，该模型比Embedding模型更准确，但比Embedding模型更耗时。因此，它可以用于重新排名Embedding模型返回的前k个文档。

> **开源和许可：**BGE模型是开源的，并在MIT许可下发布。这意味着用户可以免费用于商业目的。

> **丰富集成：**用户可以使用FlagEmbedding、Sentence-Transformers、Langchain或Huggingface Transformers等工具来使用BGE模型。

总的来说，BGE是一个强大的文本Embedding模型，适用于多种NLP任务。它的多语言支持、多版本选择和高效的reranker使其成为文本Embedding领域的领先模型。

#### 2 GTE

> GTE模型，也称为General Text Embeddings，是阿里巴巴达摩院推出的文本Embedding技术。它基于BERT框架构建，并分为三个版本：GTE-large、GTE-base和GTE-small。

该模型在大规模的多领域文本对语料库上进行训练，确保其广泛适用于各种场景。因此，GTE可以应用于信息检索、语义文本相似性、文本重新排序等任务。

尽管GTE模型的参数规模为110M，但其性能卓越。它不仅超越了OpenAI的Embedding API，在大型文本Embedding基准测试中，其表现甚至超过了参数规模是其10倍的其他模型。更值得一提的是，GTE模型可以直接处理代码，无需为每种编程语言单独微调，从而实现优越的代码检索效果。

#### 3 E5 Embedding

> E5-embedding是由intfloat团队**研发的一款先进的Embedding模型。E5**的设计初衷是为各种需要单一向量表示的任务提供高效且即用的文本Embedding，与其他Embedding模型相比，E5在需要高质量、多功能和高效的文本Embedding的场景中表现尤为出色。

以下是E5-embedding的一些特点：

> **新的训练方法：**E5采用了“EmbEddings from bidirEctional Encoder rEpresentations”这一创新方法进行训练，这意味着它不仅仅依赖传统的有标记数据，也不依赖低质量的合成文本对。

> **高质量的文本表示：**E5能为文本提供高质量的向量表示，这使得它在多种任务上都能表现出色，尤其是在需要句子或段落级别表示的任务中。

> **多场景：**无论是在Zero-shot场景还是微调应用中，E5都能提供强大的现成文本Embedding，这使得它在多种NLP任务中都有很好的应用前景。

#### 4 Jina Embedding

j**i**na-embedding-s-en-v1是Jina AI的Finetuner团队精心打造的文本Embedding模型。它基于Jina AI的Linnaeus-Clean数据集进行训练，这是一个包含了3.8亿对句子的大型数据集，涵盖了查询与文档之间的配对。这些句子对涉及多个领域，并已经经过严格的筛选和清洗。值得注意的是，Linnaeus-Clean数据集是从更大的Linnaeus-Full数据集中提炼而来，后者包含了高达16亿的句子对。

Jina Embedding有这些亮点：

> **广泛应用：**jina-embedding-s-en-v1适合多种场景，如信息检索、语义文本相似性判断和文本重新排序等。

> **卓越性能：**虽然该模型参数量仅为35M，但其性能出众，而且能够快速进行推理。

> **多样化版本：**除了标准版本，用户还可以根据需求选择其他大小的模型，包括14M、110M、330M。

#### 5 Instructor

Instructor是由香港大学自然语言处理实验室团队推出的一**种**指导微调的文本Embedding模型。该模型可以生成针对任何任务（例如分类、检索、聚类、文本评估等）和领域（例如科学、金融等）的文本Embedding，只需提供任务指导，无需任何微调。Instructor在70个不同的Embedding任务（MTEB排行榜）上都达到了最先进的性能。该模型可以轻松地与定制的sentence-transformer库一起使用。

Instructor 模型主要特点如下：

> **多任务适应性：**只需提供任务指导，即可生成针对任何任务的文本Embedding。

> **高性能：**在MTEB排行榜上的70个不同的Embedding任务上都达到了最先进的性能。

> **易于使用：**与定制的sentence-transformer库结合使用，使得模型的使用变得非常简单。

此外，模型还提供了其他使用案例，如计算句子相似性、信息检索和聚类等。

#### 6 XLM-Roberta

X**LM-Roberta（简称XLM-R）是Faceboo**k AI推出的一种多语言版本的Roberta模型。它是在大量的多语言数据上进行预训练的，目的是为了提供一个能够处理多种语言的强大的文本表示模型。XLM-Roberta模型在多种跨语言自然语言处理任务上都表现出色，包括机器翻译、文本分类和命名实体识别等。

**模型主要特点如下：**

> **多语言支持：**XLM-Roberta支持多种语言，可以处理来自不同语言的文本数据。

> **高性能：**在多种跨语言自然语言处理任务上，XLM-Roberta都表现出了最先进的性能。

> **预训练模型：**XLM-Roberta是在大量的多语言数据上进行预训练的，这使得它能够捕获跨语言的文本表示。

#### 7 text-embedding-ada-002

**text-embedding-ada-002是一个由Xenov**a团队开发的文本Embedding模型。

> **该**模型提供了一个与Hugging Face库兼容的版本的text-embedding-ada-002分词器，该分词器是从openai/tiktoken适应而来的。这意味着它可以与Hugging Face的各种库一起使用，包括Transformers、Tokenizers和Transformers.js。

**模型主要特点如下：**

> **兼容性：**该模型与Hugging Face的各种库兼容，包括Transformers、Tokenizers和Transformers.js。

> 基于openai/tiktoken：该模型的分词器是从openai/tiktoken适应而来的。

## 五、RAG评测相关

### 相关综述

**HIT**综述《Trends in Integration of Knowledge and Large Language Models: A Survey and Taxonomy of Methods, Benchmarks, and Applications》

![](img/01a75ef987fc73db0caf9d7a19b5cdc8.png)

### ragas指标评估

![](img/c6c3d645a311b743b565cbc280a23442.png)

## 六、RAG相关比赛

**经验：**第一名很早之前就使用了RAG+微调，一直遥遥领先。

**其具体的关键点为：**

> **1.RAG+**维基百科文档（上TB原始cirrussearch维基百科数据，夸张）

> **2.e5向量模型，**具体是e5-base-v2,e5-large-v2,gte-base,gte-large和bge-large

> 3.自己定制pytorch的相似度模型，无需faiss，可以解决GPU显存不够的问题。

> **4.大多数模型是7B和融合，**又一个13B，单更大的模型没起作用。

> 5.想让deberta work，但是大模型太强了，甚至融合都没用。

> **6.所有的LLM都是二分类头**+微调的。

> **7.用了两种典型**的分类头结构

## 七、常见问题

**知识库问题：**扫码版图文表图区精度不高，超长文本存储和检索精度低

改进（除了第三点，其他都能粗暴地用prompt实现）：

> **1 把问答域细化，**给检索文本分类，打标签处理，以缩小召回目标域，提升相关性。

> **2 增加问答逻辑。**如问题与上下文是否相关，上下文是否可以回答用户问题的判定逻辑，拒答逻辑。

> **3 不同种类问答的分流逻辑。**打个比方，问百科，问医药，问金融，走不同的回答逻辑。

> **4 使用多重召回逻辑。**基于向量，基于领域向量，基于es，基于编辑距离等，走投票策略。

> 5 增加生成前判定，生成后判定逻辑。前者判定适合是否该回答，是否该拒答，后者判定是否对自己回答有置信。

### 注意事项

向量库只是个数据库，并不能优化什么决定性的东西【除了检索速度】。

**重心要放在：**

> **1）对相关性内容的召回上，**把不对的踢掉【分类，拒答】，把对的提高（加权，投票）

> **2）不受控生成的缓解**【听懂指令，不跳出给定上下文，这靠SFT】。