# 2023-10-08向量检索构建智能知识库：实战与进阶

> 来源：[https://xuqiwei1986.feishu.cn/docx/AZ2tdRXYMomhpFxbiFocYy67n3e](https://xuqiwei1986.feishu.cn/docx/AZ2tdRXYMomhpFxbiFocYy67n3e)

> 🔗 原文链接： h[ttps://articles.zs](https://articles.zsxq.com/id_44ewtcmg799z.html)xq.com/id_44ewtc...

>

大家好，我是鹤涵。我正在参加小林老师的第四期 AI 之心写作营。

最近一直在给一些企业做 AI 知识库的落地的工作，在期间发现了一些不足和改进点，做一个整理复盘。

# 一、大模型的不足

以 ChatGPT 为首的大模型的，让我们看到了生产力再一次飞跃。

ChatGPT 可以在很多工作场景中提高效率：

**辅助编程： **使用 AI 来提高编程效率，帮助编写代码。

**辅助文章输出 **：可以通过翻译、总结、头脑风暴等方式生成深度文章，帮助快速产出内容。

**智能客服 **：可以用来支持客服工作，回答常见问题或提供相关建议。

**自媒体 **：可用于撰写短视频脚本、文案创作、内容批量生成等，助力自媒体创作。

**爆文写作 **：使用 ChatGPT 来撰写引人入胜的文章，吸引更多读者关注。

看起来能用到AI的地方有很多，但是**每个垂直方向在实际操作落地的时**候会感觉 「十八般武艺，样样稀松 用」 。

**好像它什么都会，**但是产出的内容直接用在复杂的生产环境上， 总好像差点意思 。

## 一）局限性

### 1、领域信息不足

虽然 LLM 仅基于公开数据集进行训练，但这也意味着它缺乏领域特定或专有等非公开的信息。这可能导致在某些情况下， LLM 无法提供更准确或更全面的回答。

### 2、有可能产生误导

尽管 LLM 尽力根据已有的数据提供信息和答案，但在超出其范围的情况下，它可能会给出不正确或是虚构的信息。这是因为 LLM 无法直接获取新的信息或与实时数据同步。

### 3、无法获取实时信息

由于 LLM 的训练成本非常高，它无法实时更新其知识库。因此，LLM 可能无法提供最新的信息或跟上快速变化的情况。

### 4、预训练数据不可更改

LLM 使用的预训练数据可能包含错误或过时的信息，并且无法进行更正或删除。这意味着 LLM 可能会基于不准确或过时的数据进行回答。

### 5、缺乏长期记忆

LLM 设计的目标是根据输入的数据给出准确的回答，但它没有真正的长期记忆能力。这意味着 LLM 可能在处理复杂的问题或需要上下文理解的情况下表现不佳。

## 二） **改进点**

### 1、领域信息不足

1） **嵌入(Embedding) **+向量数据库建立知识库，拓展认知边界

把私有数据进行切分，Embedding 转成向量，存入向量库中。这样就可以通过向量搜索给大模型加上私有数据了。

2） 微调(Fine-tuning) 针对专业领域进行特定训练

Fine-tuning 就是在大模型的基础上投喂自己的私有数据，从而生成一个新的模型。然后使用新的模型就可以使用到投喂的私有数据。

它确实可以解决领域信息不足的问题，但是他的缺点也非常多。

**需要大量标注数据： **Fine-tuning 需要大量标注的数据才能在特定任务上取得好的效果。如果标注数据有限，可能会导致性能下降。

**准确性难以保证 **：Transformer 本质上是基于“单词接龙”，无法基于领域私有数据进行准确输出。

**修改困难： **如果想对 LLM 中的数据是不能直接修改的，只能重新训练。就像一个人已经形成习惯了再去修改是很难的。

### 2、有可能产生误导

1）使用提示词工程 Prompt-Engineering 规定限制

比如使用如下提示词，让 LLM 只从我们提供的信息中查找，不会自己随意发挥。

"""

已知信息：

{context}

根据上述已知信息，简洁和专业的来回答用户的问题。如果无法从中得到答案，请说 “根据已知信息无法回答该问题” 或 “没有提供足够的相关信息”，不允许在答案中添加编造成分，答案请使用中文。

问题是：{question}

"""

2）增加更多的上下文信息

通过增加向量检索结果的 context 条数以降低误导的风险

### 3、无法获取实时信息

1）利用向量数据库为大模型建立记忆，及时更新

导入到向量库中的数据是可以随意增删改的，相比微调灵活性高狠多。

2）结合实时数据源，确保信息的时效性

可以定期的导入增量数据，从而提高数据时效性。

当然也可以接入联网搜索API（Google， Bing 等等）,那这就是另一个话题了。

### 4、不变的预训练数据

1）RLHF(Reinforcement learning from human feedback) 人工纠正再次微调

2）从向量库中剔除无效信息

### 5、缺乏长期记忆

利用向量数据库建立知识库，存储超长上下文。

由于 LLM 有最大 Token 数量的限制，只能使用 few-shot 的形式进行简单的数据投喂。

使用向量库就可以支持超大上下文（一本书，文献，课程等）的导入和读取。

# 二、向量库改进大模型

![](img/0d145717810a6136f4e772bd4cc87e77.png)

**Embedding 和 Fi**ne-tuning 对比下来还是使用 Embedding+向量库 的方式更适合做知识库。

![](img/1c6ac3e706649fc4fdd7fdfae14591bd.png)

## 一）什么是 Embedding？

Embedding 度量了文本字符串之间的相关性。Embedding 通常用于：

**搜索 **（根据与查询字符串的相关性对结果进行排序）

**聚类 **（将文本字符串按相似性分组）

**推荐 **（推荐具有相关文本字符串的项目）

**异常检测 **（识别与相关性较低的异常值）

**多样性测量 **（分析相似性分布）

**分类 **（根据最相似的标签对文本字符串进行分类）

**E**mbedding 是一个浮点数向量（列表）。两个向量之间的距离测量它们的相关性。较小的距离表示高相关性，较大的距离表示低相关性。

调用 OpenAI 的 text-embedding-ada-002 看下向量到底是什么玩意？

请求：

curl https://key.wenwen-ai.com/v1/embeddings \

-H "Authorization: Bearer sk-NYsoG3VBKDiTuvdtC969F95aFc4f45379aD3854a93602327" \

-H "Content-Type: application/json" \

-d '{

"input": "The food was delicious and the waiter...",

"model": "text-embedding-ada-002"

}'

响应：

{

"object": "list",

"data": [

{

"object": "embedding",

"embedding": [

0.0023064255,

-0.009327292,

.... ( 1536 floats total for ada -002 )

-0.0028842222,

],

"index": 0

}

],

"model": "text-embedding-ada-002",

"usage": {

"prompt_tokens": 8,

"total_tokens": 8

}

}

通过使用 Embedding 接口，我们将输入的文字转换成了一个浮点数数组的多维向量表示。这个多维向量表示了输入文字的语义信息，并且可以用于各种自然语言处理任务，如文本分类、信息检索和机器翻译等。使用 Embedding 接口可以帮助我们更好地理解和处理文字数据，提高我们的文本处理效果。

## 二）什么是向量库？

向量库是一个用于 **存储向量数据的数据库 **。它可以将文本、图像或其他类型的数据转换成向量形式，并将这些**向量存储在数据**库中。向量库的主要目的是 支持向量检索 ，即根据查询向量的相似性，在数据库中找到最相似的向量。

**简单来说就是存储向量的，把我们 Embeddi**ng 的结果存入向量库，从而构建知识库。

## 三）向量检索好在哪？

当搜索一个词或短语时，通过计算你的搜索词与数据库里每个项目的向量相似度，返回最相似的结果。

与传统的分词搜索相比，向量检索有以下优点：

**更好的理解语义 **：向量检索能够更好地理解词语或句子的意思，因为它不仅仅是基于关键词匹配。

**处理同义词 **：它可以识别不同词语之间的相似性，比如“手机”和“移动电话”。

**跨语言搜索 **：向量检索可以更容易地进行不同语言之间的搜索。

简单来说，向量检索可以通过 **语义 **进行搜索，不像传统搜索需要关键词 完全匹配，更像跟一个真实的人在对话。

# 三、CVP 技术栈落地

## 一）CVP 详解

**C **hatGPT /LLM: 大模型是大脑，用于总调度。

**V **ector database： 向量库是记忆，存储超大上下文

**P **rompt-as-code：Prompt 是逻辑能力，用来调整问答质量。

## 二）核心流程

![](img/d1eae65b7e8574ddad54fc368ab6fccd.png)

1、把私有数据进行切分后，使用模型进行向量化，然后存储到向量库中。

2、用户提问时先在向量库中进行向量搜索，匹配相关联的n条数据。

3、把用户的问题和向量库匹配结果格式化成一个 Prompt，发送给大模型，得到最终结果。

## 三）FastGPT 技术栈落地

**强烈推荐一个 CVP** 的开源技术方案 「FastGPT」 ，我也作为 FastGPT 的 Contributor 贡献过一些代码。

除了支持 CVP 的能力，还支持“高级编排”，可以自定义很多复杂的应用。

项目技术栈: NextJs + TS + ChakraUI + Mongo + Postgres（Vector 插件）

## 四）系统搭建

使用 docker-compose 一键启动

### 1、安装Docker

Docker 安装： https://www.docker.com/

[Docker-compose 安装： h](https://docs.docker.com/compose/install/)ttps://docs.docker.com/compose/install/

### 2、创建配置文件

[在 fastgpt 目录](http://config.json/)下创建 config.json 文件

{

"FeConfig": {

"show_emptyChat": false,

"show_contact": false,

"show_git": false,

"show_doc": true,

"systemTitle": "问问知识库",

"authorText": "Made by 问问AI Team.",

"limit": {

"exportLimitMinutes": 0

},

"scripts": []

},

"SystemParams": {

"vectorMaxProcess": 15,

"qaMaxProcess": 15,

"pgIvfflatProbe": 20

},

"ChatModels": [

{

"model": "gpt-3.5-turbo",

"name": "GPT35-4k",

"contextMaxToken": 4000,

"quoteMaxToken": 2000,

"maxTemperature": 1.2,

"price": 0,

"defaultSystem": ""

},

{

"model": "gpt-3.5-turbo-16k",

"name": "GPT35-16k",

"contextMaxToken": 16000,

"quoteMaxToken": 8000,

"maxTemperature": 1.2,

"price": 0,

"defaultSystem": ""

},

{

"model": "gpt-4",

"name": "GPT4-8k",

"contextMaxToken": 8000,

"quoteMaxToken": 4000,

"maxTemperature": 1.2,

"price": 0,

"defaultSystem": ""

}

],

"VectorModels": [

{

"model": "text-embedding-ada-002",

"name": "Embedding-2",

"price": 0,

"defaultToken": 500,

"maxToken": 3000

}

],

"QAModel": {

"model": "gpt-3.5-turbo-16k",

"name": "GPT35-16k",

"maxToken": 16000,

"price": 0

},

"ExtractModel": {

"model": "gpt-3.5-turbo-16k",

"functionCall": true,

"name": "GPT35-16k",

"maxToken": 16000,

"price": 0,

"prompt": ""

},

"CQModel": {

"model": "gpt-3.5-turbo-16k",

"functionCall": true,

"name": "GPT35-16k",

"maxToken": 16000,

"price": 0,

"prompt": ""

},

"QGModel": {

"model": "gpt-3.5-turbo",

"name": "GPT35-4k",

"maxToken": 4000,

"price": 0,

"prompt": "",

"functionCall": false

}

}

在 fastgpt 目录下创建 docker-compose.yml 文件

# 非 host 版本, 不使用本机代理

version: '3.3'

services:

pg:

# image: ankane/pgvector:v0.4.2 # dockerhub

image: registry.cn-hangzhou.aliyuncs.com/fastgpt/pgvector:v0.4.2 # 阿里云

container_name: pg

restart: always

ports: # 生产环境建议不要暴露

- 5432:5432

networks:

- fastgpt

environment:

# 这里的配置只有首次运行生效。修改后，重启镜像是不会生效的。需要把持久化数据删除再重启，才有效果

- POSTGRES_USER=username

- POSTGRES_PASSWORD=password

- POSTGRES_DB=fastgpt

volumes:

# 刚创建的文件

- ./pg/init.sql:/docker-entrypoint-initdb.d/init.sh

- ./pg/data:/var/lib/postgresql/data

mongo:

#image: mongo:4.0.1

image: registry.cn-hangzhou.aliyuncs.com/fastgpt/mongo:5.0.18 # 阿里云

container_name: mongo

restart: always

ports: # 生产环境建议不要暴露

- 27017:27017

networks:

- fastgpt

environment:

# 这里的配置只有首次运行生效。修改后，重启镜像是不会生效的。需要把持久化数据删除再重启，才有效果

- MONGO_INITDB_ROOT_USERNAME=username

- MONGO_INITDB_ROOT_PASSWORD=password

volumes:

- ./mongo/data:/data/db

- ./mongo/logs:/var/log/mongodb

fastgpt:

container_name: fastgpt

# image: c121914yu/fast-gpt:latest # docker hub

#image: registry.cn-hangzhou.aliyuncs.com/fastgpt/fastgpt:latest # 阿里云

# image: ghcr.io/hehan-wang/fastgpt:latest

image: registry.cn-hangzhou.aliyuncs.com/david_wang/fastgpt:latest

# network_mode: host #

ports:

- 3000:3000

networks:

- fastgpt

depends_on:

- mongo

- pg

restart: always

environment: # 可选的变量，不需要的话需要去掉

# root 密码，用户名为: root

- DEFAULT_ROOT_PSW=wenwenai

# 中转地址，如果是用官方号，不需要管

- OPENAI_BASE_URL=https://key.wenwen-ai.com/v1

- CHAT_API_KEY=sk-xxxx

- DB_MAX_LINK=5 # database max link

- TOKEN_KEY=wenwenai

- ROOT_KEY=wenwenai

- FILE_TOKEN_KEY=filetoken

# mongo 配置，不需要改. 如果连不上，可能需要去掉 ?authSource=admin

- MONGODB_URI=mongodb://username:password@mongo:27017/fastgpt?authSource=admin

# pg配置. 不需要改

- PG_URL=postgresql://username:password@pg:5432/fastgpt

volumes:

- ./config.json:/app/data/config.json

networks:

fastgpt:

修改上面配置文件中的 OPENAI_BASE_URL,CHAT_API_KEY 替换成自己的 key。

### 3、启动脚本

docker-compose pull

docker-compose up -d

### 4、访问网站

目前可以通过 ip:3000 直接访问(注意防火墙)。登录用户名为 root，密码为 docker-compose.yml 环境变量里设置的 DEFAULT_ROOT_PSW。

![](img/defd134fdf028d768e9222e3d2582ebb.png)

## 五）知识库搭建

### 1、创建知识库

默认使用 OpenAI 的 Embedding 模型把文本转成向量（不懂什么是向量也不影响使用哈）。

还可以设置标签，方便知识库进行分类

![](img/0bdc9e9e65cde1e6f724d0831082e46a.png)

### 2、导入私有数据

支持丰富的导入方式。

可以手动输入 QA，也可以从网络/本地文件中提取文本的方式导入

![](img/de7762a1a6d5c6f51635a885c5f4c9a8.png)

### 3、测试向量搜索

**可以输入文本测**试向量库的搜索，下面 0.8884 就是文本和知识库片段的相似度。

![](img/5562af54fcb6b9d52f2ce9841def8db5.png)

## 六）应用搭建

### 1、创建应用

我们选 **知识库+对话引导 **这个最经典运用到 CVP 技术的场景

![](img/47b3c379314b21d23c40cd93702e20ce.png)

### 2、关联知识库

关联刚创建的知识库，填写自定义 Prompt 控制输出。

![](img/48fa950522596e52c5057bdb8576955d.png)

### 3、测试知识库问答

这里就跟我们正常使用 GPT 一样，通过文本的方式对话，也支持聊天上下文。

同时还会显示向量搜索的结果，上下文情况， Token 消耗，完整响应日志，帮助我们优化应用。

![](img/16011800d78f2db90d43e068992659e1.png)

虽然可以完成基本的知识库能力，但是效果并不是那么好。会经常出现想要的内容搜不到，不想要的内容却可以搜出来的情况。

这就会导致在对 **准确度要求严格 **的场景无法使用。所以要进行召回效果优化。

# 四、召回效果优化

在互联网业务中常见的推荐、广告、搜索等场景下，召回是指从大规模数据集中快速检索出一些候选项，以便进一步进行排序和筛选。召回阶段的目标是尽可能多地捕捉到用户可能感兴趣的内容，以提供更准确和相关的推荐、广告或搜索结果。

在知识库场景下的召回就是如何找到跟问题最相关的文档片段。

其中重点可优化的点一个是在 **索引阶段（文档导入） **，另一个是在 **召回阶段（文档搜索） **。

## 一）索引阶段

### 1、优化文档分段模式

文档切分最准确的方式肯定是按照固定的字数（比如600字）切开，但是可能会把语义连贯的文字切分到两个片段造成部分数据缺失。

1）上下文理解

在文档切分的时候使用提示词工程（Prompt Engineering）进行上下文理解，保证连贯语义的上下文不切割。

2）文件格式清洗

**由于LL**M是文本语言模型，最终得把文件的格式清洗成 纯文本 再进行投喂。比如 PDF ， Word ，网址，都必须通过技术手段最终转化成文本（OCR技术和爬虫技术已经比较成熟了）。

而 **结构化文本 Markdown **，当然是语料的首选，由于本身就是纯LLM可以很容易的识别出来。

但是如果一个标题下面内容太长了怎么办呢?

比如下面这个Markdown我们可以把标题识别出来放在下面多个文本段前面

before:

# 标题1

## 标题2

...

2000字

...

# 标题2

...

after：

# 标题1

## 标题2

0-600字

# 标题1

## 标题2

600-1200字

...

这样我们就可以通过标题1，标题2检索到全量内容了。这个代码不难，当然可以直接让 GPT 帮我们去写~

### 2、切换 Embedding Model

根据文档的特性（语言、内容），选择 Embedding 模型。

不同语言，不同细分领域使用同一个Embedding模型，可能会对某些领域知识不了解，导致转化成的向量不是很精准。而向量生成是第一步，会影响后续的召回。

我们不仅仅可以使用 OpenAI 的 text-embedding-ada-002，还可以选择 m3e 等等上百种 Embedding Model。

### 3、自动生成QA

将文档转化为问题，并使用问题召回方法

使用提示词工程（Prompt Engineering）把分段后的文本自动切分 Question/Awser ，使用 Question 进行召回。

我会给你一段文本，它们可能包含多个主题内

容，学习它们，并整理学习成果，要求为：

1.提出最多25个问题。

2.给出每个问题的答案。

3.答案要详细完整，答案可以包含普通文字、

链接、代码、表格、公示、媒体链接等

markdown元素。

4.按格式返回多个问题和答案：

Q1:问题。

A1:答案。

Q2:

A2:

....

我的文本：""{text}""

## 二）召回阶段

### 1、参数调整

不断调整 相似度，单次搜索上限,温度值,回复上限 来测试最佳效果

相似度越高，搜索结果越准确。

单次搜索上限越高，私有数越月丰富，Token消耗越快。

![](img/1016bc19e52a83e1442353ea1a28fc14.png)

### 2、Prompt 调整

可以通过 Prompt 调整，让知识库只回复私有数据中有的内容，不自己「胡编乱造」

你的背景知识:

"""

{{quote}}

"""

对话要求：

1. 回答前，请先判断背景知识是否足够回答问题，如果无法回答，请直接回复：“对不起，我无法回答你的问题~”。

2. 背景知识是最新的，其中 instruction 是相关介绍，output 是预期回答或补充。

3. 使用背景知识回答问题。

我的问题是:"{{question}}"

### 3、用户问题改写

用户的问题可能很不规范，导致召回效果差。同样使用提示词工程对用户问题进行改写，然后再去向量库召回。

### 4、多路召回

由于向量召回是属于模 **糊匹配 **，依赖于 Embedding 是否准确，容易产生幻觉。

所以引入“多路召回”，在使用向量召回的同**时，也使用**原始的分词召回（如ES）进行 精准匹配 。

把多路召回的结果合并后继续走后续的流程。

![](img/4c9ccc2732cf0564a5a44fd44d8a5a8a.png)

## 三）召回后排序

当召回出的文档片段数量比较多的时候，只对召回结果通过相似度取 topK 就不够了。

需要对召回结果进一步做粗排，精排，重排，把大的候选集优中选优得到 topK。

![](img/ef87de3b6b0637a88e94961c2b8c6e55.png)

# 五、总结

## 一）文档的清洗非常重要

整理清洗工作也可以利用人工智能来更快更好地完成。

**上面这些优化甚至可以封装**成一个智能体 FileAgent 的 单独项目来对文档进行清洗，再导入知识库。

## 二）知识库在企应用非常广

知识库的在企业中的应用场景非常多

智能客服，企业培训，Copilot 助**手，数**字分身这些场景都需要知识库来提供 记忆 。

随着时间推移，承接业务越来越复杂，会持续涌现出各种各样的优化。

希望大家可以多多讨论交流，共同进步。

# 六、最后

感谢 **洋哥 **搭建破局这个国内最大的 AI 交流平台。其他大大小小 AI 社群都没有内容输出的情况下，咱们破局还能不断输出精华贴内容，这些精华帖随随便便拿出去都能碾压市面上1000块以上的课程。洋哥也真的是用心在经营这个社群，举办了线下峰会，航海，快闪活动等。

也感谢 **小林老师 **举办了这个 AI 之心的活动，帮我们把控文章的质量。

最后感谢 **小林老师 **和 **心图老师 **在十一假期期间，用自己的休息时间帮忙改稿，衷心感谢，辛苦了~！