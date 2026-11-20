
# Book-Crossing 数据集

这个数据集是网上的 Book-Crossing 图书社区的 278858 个用户对 271379 本书进行的评分，包括显式和隐式的评分。这些用户的年龄等人口统计学属性(`demographic feature`)都以匿名的形式保存并供分析。这个数据集是由 Cai-Nicolas Ziegler 使用爬虫程序在 2004 年从 Book-Crossing 图书社区上采集的。

# 推荐论文

经典必读论文整理，包括综述文章、传统经典推荐文章、社会化推荐文章、基于深度学习的推荐系统文章、专门用于解决冷启动的文章、`POI` 相关的论文、利用哈希技术来加速推荐的文章以及推荐系统中经典的探索与利用问题的相关文章等。

项目地址：

`https://github.com/hongleizhang/RSPapers`

# 推荐项目

1. 今日头条推荐系统机制介绍，面向内容创作者。分享人：项亮，今日头条推荐算法架构师：

   `https://v.qq.com/x/page/f0800qavik7.html?`

2. 3 分钟了解今日头条推荐系统原理

   `https://v.qq.com/x/page/g05349lb80j.html?`

3. facebook 是如何为十亿人推荐好友的

   `https://code.facebook.com/posts/861999383875667/recommending-items-to-more-than-a-billion-people/`

4. Netflix 的个性化和推荐系统架构

   `http://techblog.netflix.com/2013/03/system-architectures-for.html`

# 风控模型（评分卡模型）

## 评分卡模型简介

评分卡模型时在银行、互金等公司与借贷相关业务中最常见也是最重要的模型之一。简而言之它的作用就是对客户进行打分，来对客户是否优质进行评判。

根据评分卡模型应用的业务阶段不用，评分卡模型主要分为三大类：`A` 卡（`Application score card`）申请评分卡、`B` 卡（`Behavior score card`）行为评分卡、`C` 卡（`Collection score card`）催收评分卡。其中申请评分卡用于贷前，行为评分卡用于贷中，催收评分卡用于贷后，这三种评分卡在我们的信贷业务的整个生命周期都至关重要。

## 推荐书籍

《信用风险评分卡研究——基于 `SAS` 的开发与实施》

## 评分卡模型建模过程

### 样本选取

确定训练样本、测试样本的观察窗（特征的时间跨度）与表现窗（标签的时间跨度），且样本的标签定义是什么？一般情况下风险评分卡的标签都是考虑客户某一段时间内的延滞情况。

### 特征准备

原始特征、衍生变量

### 数据清洗

根据业务需求对缺失值或异常值等进行处理

### 特征筛选

根据特征的 `IV` 值（特征对模型的贡献度）、`PSI`（特征的稳定性）来进行特征筛选，`IV` 值越大越好（但是一个特征的 `IV` 值超过一定阈值可能要考虑是否用到未来数据），`PSI` 越小越好（一般建模时取特征的 `PSI` 小于等于 0.01）

### 对特征进行 `WOE` 转换

即对特征进行分箱操作，注意在进行 `WOE` 转换时要注重特征的可解释性

### 建立模型

在建立模型过程中可根据模型和变量的统计量判断模型中包含和不包含每个变量时的模型质量来进行变量的二次筛选。

### 评分指标

评分卡模型一般关注的指标是 `KS` 值（衡量的是好坏样本累计分部之间的差值）、模型的 `PSI`（即模型整体的稳定性）、`AUC` 值等。

# 知识图谱

## 知识图谱是什么

知识图谱是一种结构化数据的处理方法，它涉及知识的提取、表示、存储、检索等一系列技术。从渊源上讲，它是知识表示与推理、数据库、信息检索、自然语言处理等多种技术发展的融合。

## 推荐资料

为什么需要知识图谱？什么是知识图谱？——`KG` 的前世今生

`https://zhuanlan.zhihu.com/p/31726910`

什么是知识图谱？

`https://zhuanlan.zhihu.com/p/34393554`

智能搜索时代：知识图谱有何价值？

`https://zhuanlan.zhihu.com/p/35982177?from=1084395010&wm=9848_0009&weiboauthoruid=5249689143`

百度王海峰：知识图谱是 AI 的基石

`http://www.infoq.com/cn/news/2017/11/Knowledge-map-cornerstone-AI#0-tsina-1-5001-397232819ff9a47a7b7e80a40613cfe1`

译文|从知识抽取到 `RDF` 知识图谱可视化

`http://rdc.hundsun.com/portal/article/907.html?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io`

## 主要内容

### 3.1 知识提取

构建 `kg` 首先需要解决的是数据，知识提取是要解决结构化数据生成的问题。我们可以用自然语言处理的方法，也可以利用规则。

#### 使用规则

##### 正则表达式

正则表达式（`Regular Expression`，`regex`）是字符串处理的基本功。数据爬取、数据清洗、实体提取、关系提取，都离不开 `regex`。

推荐资料入门：

精通正则表达式

`regexper` 可视化：例 `[a-z]*(\d{4}(\D+))`

`pythex` 在线测试正则表达式：

`http://pythex.org/`

推荐资料进阶：

`re2`：

Python wrapper for Google's `RE2` using `Cython`

`https://pypi.python.org/pypi/re2/`

`Parsley`：更人性化的正则表达语法

`http://parsley.readthedocs.io/en/latest/tutorial.html`

##### 中文分词和词性标注

分词也是后续所有处理的基础，词性（`Part of Speech`，`POS`）就是中学大家学过的动词、名词、形容词等等的词的分类。一般的分词工具都会有词性标注的选项。

推荐资料入门：

`jieba` 中文分词包

`https://github.com/fxsjy/jieba`

中文词性标记集

`https://github.com/memect/kg-beijing/wiki/`

推荐资料进阶：

`genius` 采用 `CRF` 条件随机场算法

`https://github.com/duanhongyi/genius`

`Stanford CoreNLP` 分词

`https://blog.csdn.net/guolindonggld/article/details/72795022`

##### 命名实体识别

命名实体识别（`NER`）是信息提取应用领域的重要基础工具，一般来说，命名实体识别的任务就是识别出待处理文本中三大类（实体类、时间类和数字类）、七小类（人名、机构名、地名、时间、日期、货币和百分比）命名实体。

推荐资料：

`Stanford CoreNLP` 进行中文命名实体识别

`https://blog.csdn.net/guolindonggld/article/details/72795022`

### 3.1.2 使用深度学习

使用自然语言处理的方法，一般是给定 `schema`，从非结构化数据中抽取特定领域的三元组（`spo`），如最近百度举办的比赛就是使用 `DL` 模型进行信息抽取。

#### 序列标注

使用序列生出模型，主要是标记出三元组中 `subject` 及 `object` 的起始位置，从而抽取信息。

推荐资料：

序列标注问题

`https://www.cnblogs.com/jiangxinyang/p/9368482.html`

#### `seq2seq`

使用 `seq2seq` 端到端的模型，主要借鉴文本摘要的思想，将三元组看成是非结构化文本的摘要，从而进行抽取，其中还涉及 `Attention` 机制。

推荐资料：

`seq2seq` 详解

`https://blog.csdn.net/irving_zhang/article/details/78889364`

详解从 `Seq2Seq` 模型到 `Attention` 模型

`https://caicai.science/2018/10/06/attention%E6%80%BB%E8%A7%88/`

### 3.2 知识表示

知识表示（`Knowledge Representation`，`KR`，也译为知识表现）是研究如何将结构化数据组织，以便于机器处理和人的理解的方法。

需要熟悉下面内容：

`JSON` 和 `YAML`

`json` 库：

`https://docs.python.org/2/library/json.html`

`PyYAML`: 是 Python 里的 `Yaml` 处理库

`http://pyyaml.org/wiki/PyYAML`

`RDF` 和 `OWL` 语义：

`http://blog.memect.cn/?p=871`

`JSON-LD`

主页：`http://json-ld.org/`

### 3.3 知识存储

需要熟悉常见的图数据库

a. 知识链接的方式：字符串、外键、`URI`

b. `PostgreSQL` 及其 `JSON` 扩展

`Psycopg` 包操作 `PostgreSQL`

`http://initd.org/psycopg/docs/`

c. 图数据库 `Neo4j` 和 `OrientDB`

1. `Neo4j` 的 `Python` 接口 `https://neo4j.com/developer/python/`

2. `OrientDB`：`http://orientdb.com/orientdb/`

d. `RDF` 数据库 `Stardog`

`Stardog` 官网：`http://stardog.com/`

### 3.4 知识检索

需要熟悉常见的检索技术

`ElasticSearch` 教程：

`http://joelabrahamsson.com/elasticsearch-101/`

## 相关术语及技术路线

本体：

`https://www.zhihu.com/question/19558514`

`RDF`：

`https://www.w3.org/RDF/`

`Apache Jena`：

`https://jena.apache.org/`

`D2RQ`：

`http://d2rq.org/getting-started`

### 4.1 `Protege` 构建本体系列

`protege`：

`https://protege.stanford.edu/`

`protege` 使用：

`https://zhuanlan.zhihu.com/p/32389370`

### 4.2 开发语言

`python` 或 `java`

### 4.3 图数据库技术

`Neo4j`：`https://neo4j.com/`

`AllegroGraph`：`https://franz.com/agraph/allegrograph/`

### 4.4 可视化技术

`d3.js`：`https://d3js.org/`

`Cytoscape.js`：`http://js.cytoscape.org/`

### 4.5 分词技术

`jieba`：`https://github.com/fxsjy/jieba`

`hanlp`：`https://github.com/hankcs/HanLP`

## 项目实战

基于知识图谱的问答：

`https://github.com/kangzhun/KnowledgeGraph-QA-Service`

`Agriculture_KnowledgeGraph`：

`https://github.com/qq547276542/Agriculture_KnowledgeGraph`

# 贡献平台

由知名开源平台，AI 技术平台以及领域专家：`ApacheCN`，`Datawhale`，`AI` 有道和黄海广博士联合整理贡献：

1. `ApacheCN`：片刻，李翔宇，飞龙，王翔

2. `Datawhale`：范晶晶，马晶敏，李碧涵，李福，光城，居居，康兵兵，郑家豪

3. `AI` 有道：红色石头

4. 黄海广博士

# 转载声明

本文由机器之心转载，转载请联系原👸号获得授权。

# 联系我们

## 加入机器之心（全职记者 / 实习生）

`hr@jiqizhixin.com`

## 投稿或寻求报道

`content@jiqizhixin.com`

## 广告 & 商务合作

`bd@jiqizhixin.com`
