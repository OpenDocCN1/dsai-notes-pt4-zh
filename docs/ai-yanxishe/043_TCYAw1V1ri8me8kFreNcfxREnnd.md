# 基于RAG构建投喂大模型，实战与进阶

> 来源：[https://aiyanxishe.feishu.cn/docx/ExTtd0LnGo00RdxRZLXcaqwmn2e](https://aiyanxishe.feishu.cn/docx/ExTtd0LnGo00RdxRZLXcaqwmn2e)

## 程序员AI破局简介

> [《程序员AI破局指南》](https://t.zsxq.com/19EOflzkQ)由字节、百度、阿里等互联网大厂的AI业务相关高管共创，分享开箱即用、面向市场的AI编程知识和工具。

> 指南包括六个专栏:**AI编程实战、AI编程工具、AI高效办公、A**I副业赚钱、AI提示词工程、AI私人助手

> [程序员AI破局指南](https://t.zsxq.com/19EOflzkQ) 原价 599 元，限时特惠 199 元/年，5月1日恢复原价。

> 加入我们：https://t.zsxq.com/19EOflzkQ

> 购买后，添加群主🛰️加入 AI研习社陪伴社群，限时领取超值福利。

# 一、大模型的不足

以 ChatGPT 为首的大模型的，让我们看到了生产力再一次飞跃。

ChatGPT 可以在很多工作场景中提高效率：

**辅助编程： **使用 AI 来提高编程效率，帮助编写代码。

**辅助文章输出 **：可以通过翻译、总结、头脑风暴等方式生成深度文章，帮助快速产出内容。

**智能客服 **：可以用来支持客服工作，回答常见问题或提供相关建议。

**自媒体 **：可用于撰写短视频脚本、文案创作、内容批量生成等，助力自媒体创作。

**爆文写作 **：使用 ChatGPT 来撰写引人入胜的文章，吸引更多读者关注。

看起来能用到AI的地方有很多，但是每个垂直方向在实际操作落地的时候会感觉**「十八般武艺，样样稀松」**。

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

把私有数据进行切分，Embedding转成向量，存入向量库中。这样就可以通过向量搜索给大模型加上私有数据了。

2）**微调(Fine-tuning) **针对专业领域进行特定训练

Fine-tuning 就是在大模型的基础上投喂自己的私有数据，从而生成一个新的模型。然后使用新的模型就可以使用到投喂的私有数据。

它确实可以解决领域信息不足的问题，但是他的缺点也非常多。

**需要大量标注数据： **Fine-tuning 需要大量标注的数据才能在特定任务上取得好的效果。如果标注数据有限，可能会导致性能下降。

**准确性难以保证 **：Transformer 本质上是基于“单词接龙”，无法基于领域私有数据进行准确输出。

**修改困难： **如果想对 LLM 中的数据是不能直接修改的，只能重新训练。就像一个人已经形成习惯了再去修改是很难的。

### 2、有可能产生误导

1）使用提示词工程 Prompt-Engineering 规定限制

2）增加更多的上下文信息

通过增加向量检索结果的 context 条数以降低误导的风险

### 3、无法获取实时信息

1）利用向量数据库为大模型建立记忆，及时更新

导入到向量库中的数据是可以随意增删改的，相比微调灵活性高狠多。

2）结合实时数据源，确保信息的时效性

可以定期的导入增量数据，从而提高数据时效性。

当然也可以接入联网搜索API（google， bing 等等）,那这就是另一个话题了。

### 4、不变的预训练数据

1）RLHF(Reinforcement learning from human feedback) 人工纠正再次微调

2）从向量库中剔除无效信息

### 5、缺乏长期记忆

利用向量数据库建立知识库，存储超长上下文。

由于 LLM 有最大 Token 数量的限制，只能使用 few-shot 的形式进行简单的数据投喂。

使用向量库就可以支持超大上下文（一本书，文献，课程等）的导入和读取。

# 二、向量库改进大模型

![](img/605a63b3faf27441d98a0f2238d19178.png)

**Embedding 和 Fi**ne-tuning 对比下来还是使用 Embedding+向量库 的方式更适合做知识库。

Embedding

Fine-tunine

易于管理

适用于较小的知识库

更精确和具有上下文的答案

没有知识的访问控制

高度适应性和灵活性

高前期成本，高维护成本

更好的模糊搜索能力

知识不是最新的

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

```
curl https://key.wenwen-ai.com/v1/embeddings \
  -H &quot;Authorization: Bearer sk-NYsoG3VBKDiTuvdtC969F95aFc4f45379aD3854a93602327&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#x27;{
    &quot;input&quot;: &quot;The food was delicious and the waiter...&quot;,
    &quot;model&quot;: &quot;text-embedding-ada-002&quot;
  }&#x27;
```

响应：

```
{
  &quot;object&quot;: &quot;list&quot;,
  &quot;data&quot;: [
    {
      &quot;object&quot;: &quot;embedding&quot;,
      &quot;embedding&quot;: [
        0.0023064255,
        -0.009327292,
        .... (1536 floats total for ada-002)
        -0.0028842222,
      ],
      &quot;index&quot;: 0
    }
  ],
  &quot;model&quot;: &quot;text-embedding-ada-002&quot;,
  &quot;usage&quot;: {
    &quot;prompt_tokens&quot;: 8,
    &quot;total_tokens&quot;: 8
  }
}
```

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

**C ****ha****tG**PT /LLM: 大模型是大脑，用于总调度。 V ector database： 向量库是记忆，存储超大上下文 P rompt-as-code：Prompt 是逻辑能力，用来调整问答质量。

## 二）核心流程

![](img/454f8b3cb24c0b98202a61a7ba7df87b.png)

1、把私有数据进行切分后，使用模型进行向量化，然后存储到向量库中。

2、用户提问时先在向量库中进行向量搜索，匹配相关联的n条数据。

3、把用户的问题和向量库匹配结果格式化成一个 Prompt，发送给大模型，得到最终结果。

## 三）FastGPT 技术栈落地

强烈推荐一个 CVP 的开源技术方案**「FastGPT」**，我也作为 FastGPT 的 Contributor 贡献过一些代码。

除了支持 CVP 的能力，还支持“高级编排”，可以自定义很多复杂的应用。

项目技术栈: NextJs + TS + ChakraUI + Mongo + Postgres（Vector 插件）

## 四）系统搭建

使用 docker-compose 一键启动

### 1、安装Docker

Docker 安装： https://www.docker.com/

[Docker-compose 安装： h](https://docs.docker.com/compose/install/)ttps://docs.docker.com/compose/install/

### 2、创建配置文件

```
mkdir fastgpt
cd fastgpt
```

在 fastgpt 目录下创建 config.json 文件

```
{
  &quot;FeConfig&quot;: {
    &quot;show_emptyChat&quot;: false,
    &quot;show_contact&quot;: false,
    &quot;show_git&quot;: false,
    &quot;show_doc&quot;: true,
    &quot;systemTitle&quot;: &quot;问问知识库&quot;,
    &quot;authorText&quot;: &quot;Made by 问问AI Team.&quot;,
    &quot;limit&quot;: {
      &quot;exportLimitMinutes&quot;: 0
    },
    &quot;scripts&quot;: []
  },
  &quot;SystemParams&quot;: {
    &quot;vectorMaxProcess&quot;: 15,
    &quot;qaMaxProcess&quot;: 15,
    &quot;pgIvfflatProbe&quot;: 20
  },
  &quot;ChatModels&quot;: [
    {
      &quot;model&quot;: &quot;gpt-3.5-turbo&quot;,
      &quot;name&quot;: &quot;GPT35-4k&quot;,
      &quot;contextMaxToken&quot;: 4000,
      &quot;quoteMaxToken&quot;: 2000,
      &quot;maxTemperature&quot;: 1.2,
      &quot;price&quot;: 0,
      &quot;defaultSystem&quot;: &quot;&quot;
    },
    {
      &quot;model&quot;: &quot;gpt-3.5-turbo-16k&quot;,
      &quot;name&quot;: &quot;GPT35-16k&quot;,
      &quot;contextMaxToken&quot;: 16000,
      &quot;quoteMaxToken&quot;: 8000,
      &quot;maxTemperature&quot;: 1.2,
      &quot;price&quot;: 0,
      &quot;defaultSystem&quot;: &quot;&quot;
    },
    {
      &quot;model&quot;: &quot;gpt-4&quot;,
      &quot;name&quot;: &quot;GPT4-8k&quot;,
      &quot;contextMaxToken&quot;: 8000,
      &quot;quoteMaxToken&quot;: 4000,
      &quot;maxTemperature&quot;: 1.2,
      &quot;price&quot;: 0,
      &quot;defaultSystem&quot;: &quot;&quot;
    }
  ],
  &quot;VectorModels&quot;: [
    {
      &quot;model&quot;: &quot;text-embedding-ada-002&quot;,
      &quot;name&quot;: &quot;Embedding-2&quot;,
      &quot;price&quot;: 0,
      &quot;defaultToken&quot;: 500,
      &quot;maxToken&quot;: 3000
    }
  ],
  &quot;QAModel&quot;: {
    &quot;model&quot;: &quot;gpt-3.5-turbo-16k&quot;,
    &quot;name&quot;: &quot;GPT35-16k&quot;,
    &quot;maxToken&quot;: 16000,
    &quot;price&quot;: 0
  },
  &quot;ExtractModel&quot;: {
    &quot;model&quot;: &quot;gpt-3.5-turbo-16k&quot;,
    &quot;functionCall&quot;: true,
    &quot;name&quot;: &quot;GPT35-16k&quot;,
    &quot;maxToken&quot;: 16000,
    &quot;price&quot;: 0,
    &quot;prompt&quot;: &quot;&quot;
  },
  &quot;CQModel&quot;: {
    &quot;model&quot;: &quot;gpt-3.5-turbo-16k&quot;,
    &quot;functionCall&quot;: true,
    &quot;name&quot;: &quot;GPT35-16k&quot;,
    &quot;maxToken&quot;: 16000,
    &quot;price&quot;: 0,
    &quot;prompt&quot;: &quot;&quot;
  },
  &quot;QGModel&quot;: {
    &quot;model&quot;: &quot;gpt-3.5-turbo&quot;,
    &quot;name&quot;: &quot;GPT35-4k&quot;,
    &quot;maxToken&quot;: 4000,
    &quot;price&quot;: 0,
    &quot;prompt&quot;: &quot;&quot;,
    &quot;functionCall&quot;: false
  }
}
```

在 fastgpt 目录下创建 docker-compose.yml 文件

```
# 非 host 版本, 不使用本机代理
version: &#x27;3.3&#x27;
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
```

修改上面配置文件中的 OPENAI_BASE_URL,CHAT_API_KEY 替换成自己的 key。

### 3、启动脚本

```
# 在 docker-compose.yml 同级目录下执行
docker-compose pull
docker-compose up -d
```

### 4、访问网站

目前可以通过 ip:3000 直接访问(注意防火墙)。登录用户名为 root，密码为 docker-compose.yml环境变量里设置的 DEFAULT_ROOT_PSW。

![](img/c3ff4fa4d7221d4e51385ac80fc9cff0.png)

## 五）知识库搭建

### 1、创建知识库

默认使用 OpenAI 的 Embedding 模型把文本转成向量（不懂什么是向量也不影响使用哈）。

还可以设置标签，方便知识库进行分类

![](img/97dbe7c1a693a7417c14296e6ba9361d.png)

### 2、导入私有数据

支持丰富的导入方式。 可以手动输入 QA，也可以从网络/本地文件中提取文本的方式导入

![](img/35230756efe4d99db0a23f511383a1c0.png)

### 3、测试向量搜索

**可以输入文本测**试向量库的搜索，下面 0.8884 就是文本和知识库片段的相似度。

![](img/2e51a9ad44d7122d26f0ad6f731c32fb.png)

## 六）应用搭建

### 1、创建应用

我们选 **知识库+对话引导 **这个最经典运用到 CVP 技术的场景

![](img/ade83a23c29c24bb09561a89910cc255.png)

### 2、关联知识库

关联刚创建的知识库，填写自定义 Prompt 控制输出。

![](img/34115deda8bee2947cafccef433a2560.png)

### 3、测试知识库问答

这里就跟我们正常使用 GPT 一样，通过文本的方式对话，也支持聊天上下文。

同时还会显示向量搜索的结果，上下文情况， Token 消耗，完整响应日志，帮助我们优化应用。

![](img/1ef0632a82cea0ed5c145b01f591d3a8.png)

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

而**结构化文本 Markdown **，当然是语料的首选，由于本身就是纯LLM可以很容易的识别出来。

但是如果一个标题下面内容太长了怎么办呢?

比如下面这个Markdown我们可以把标题识别出来放在下面多个文本段前面

### 2、切换 Embedding Model

根据文档的特性（语言、内容），选择 Embedding 模型。

不同语言，不同细分领域使用同一个Embedding模型，可能会对某些领域知识不了解，导致转化成的向量不是很精准。而向量生成是第一步，会影响后续的召回。

我们不仅仅可以使用 OpenAI 的 text-embedding-ada-002，还可以选择 m3e 等等上百种 Embedding Model。

### 3、自动生成QA

将文档转化为问题，并使用问题召回方法

使用提示词工程（Prompt Engineering）把分段后的文本自动切分 Question/Awser ，使用 Question 进行召回。

```
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
我的文本：&quot;&quot;{text}&quot;&quot;
```

## 二）召回阶段

### 1、参数调整

不断调整 相似度，单次搜索上限,温度值,回复上限 来测试最佳效果

相似度越高，搜索结果越准确。

单次搜索上限越高，私有数越月丰富，Token消耗越快。

![](img/79154ad79e6d5ddaaa85ebb336de0d5c.png)

### 2、Prompt调整

可以通过 Prompt 调整，让知识库只回复私有数据中有的内容，不自己”胡编乱造“

```
你的背景知识:
&quot;&quot;&quot;
{{quote}}
&quot;&quot;&quot;
对话要求：
1. 回答前，请先判断背景知识是否足够回答问题，如果无法回答，请直接回复：“对不起，我无法回答你的问题~”。
2. 背景知识是最新的，其中 instruction 是相关介绍，output 是预期回答或补充。
3. 使用背景知识回答问题。
我的问题是:&quot;{{question}}&quot;
```

### 3、用户问题改写

用户的问题可能很不规范，导致召回效果差。同样使用提示词工程对用户问题进行改写，然后再去向量库召回。

### 4、多路召回

由于向量召回是属于模 **糊匹配 **，依赖于 Embedding 是否准确，容易产生幻觉。

所以引入“多路召回”，在使用向量召回的同**时，也使用**原始的分词召回（如ES）进行 精准匹配 。

把多路召回的结果合并后继续走后续的流程。

![](img/5b022f74129d046132195a759b180f56.png)

## 三）召回后排序

当召回出的文档片段数量比较多的时候，只对召回结果通过相似度取 topK 就不够了。

需要对召回结果进一步做粗排，精排，重排，把大的候选集优中选优得到 topK。

![](img/ffe4da2eb8606608405bf76a911eaf5f.png)

# 五、总结

## 一）文档的清洗非常重要

整理清洗工作也可以利用人工智能来更快更好地完成。

**上面这些优化甚至可以封装**成一个智能体 FileAgent 的 单独项目来对文档进行清洗，再导入知识库。

## 二）知识库在企应用非常广

知识库的在企业中的应用场景非常多

智能客服，企业培训，Copilot 助**手，数**字分身这些场景都需要知识库来提供 记忆 。

随着时间推移，承接业务越来越复杂，会持续涌现出各种各样的优化。

更多内容可以点击目录： [手册内容汇总](https://aiyanxishe.feishu.cn/wiki/LBXcwPv6UiZgdfkp14Xc1Wo1n3b)

✨ 如果觉得有收获，请点一点下方的“有启发” ✨

🌱 我也会及时优化迭代内容，给大家更好的阅读体验 🌱

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>