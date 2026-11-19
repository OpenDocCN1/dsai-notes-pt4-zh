
# CES 2024 行业图谱

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BxnSdk3oSoFyGdxT0N0cNcIOndY](https://v11enp9ok1h.feishu.cn/docx/BxnSdk3oSoFyGdxT0N0cNcIOndY)

<https://v11enp9ok1h.feishu.cn/sync/PaIIdslbxsIJUIbkTfPcsMCFnph>

## 下载阅读

英文版

[`CES_2024_Industry_Mapping.xlsx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Q2wfbk2qaoTta3xmh8ccSTt4nYd)

翻译版

![](img/f973426d8fd722a5d2dc7f6b910d5468.png)

[`CES2024行业图谱.xlsx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XsUqbcQ3GoOgcSxPPgocv86Enkd)

源代码

https://github.com/garmanwu/CES2024

在线版（外国开发者）

![](img/dfb949d84741df927a2c21fff78a430f.png)

## 项目背景

这是一个实验，探索AI能否根据信息框架归纳资讯，提高阅读效率。

CES行业图谱覆盖：

+ CES 4300家参展商产品信息，47个行业最新动态。

+ 每间公司搜集10个相关新闻报道，700M文本数据。

+ 用ChatGPT总结，汇总成一个行业图谱，便于研究。

项目难点：

+ 20%企业是startup，没有网络信息，容易造成幻觉，需多信息源交叉验证，幻觉控制在5%。

### 总体情况

+ 本届主题是AI for All。

+ 大语言模型提高I/O效率，甚至成为I/O系统雏形。

+ 大语言模型给硬件应用场景、form factor、新交互方式提供机会。家庭、汽车是目前的主战场，面向现实场景的个人硬件层出不穷，例如Rabbit R1。

![](img/a6002934603ca82731b5cd6d38a0f82c.png)

## 研发实现

### Pipeline

整个技术管线分为爬虫、归纳、修复、输出四个环节。创新点：

+ 级联设计：任务1爬虫和任务2归纳同时执行，提高效率。

+ 多模型配合：

+ 90%由ChatGPT 3.5执行归纳，控制成本。

+ 10%由ChatGPT 4.0执行归纳，修复幻觉。

### 资讯收集

首先，我们需要一个搜索引擎，搜索相关信息在什么地方。与爬虫软件不同，爬虫是针对单个网页（如amazon）或指定几个网址收集信息。但假如网页在哪里都不知道，就需要搜索引擎找到才能爬虫。这里推荐Tavily API接口，集成了搜索、爬虫等功能，免费。

| 资讯渠道 | 优点 | 缺点 |

| --- | --- | --- |

| GPTs Bing Search | quick search为主，信息不一定全面，但整体效果不错。 | 无法提供API访问接口，不符合项目需求 |

| Azure Bing Search | 可API访问，每个月1000次免费 | 只返回前5条信息，而且只有url和网页摘要，没有网页原文。 |

| Tavily API[推荐] | 提供广泛网站搜索库 | 因为没有商业化，服务不稳定。 |

Python执行过程

1. 从CES官方收集4000+参展商名单，作为Tavily搜索爬虫的任务列表（`Query_list.xlsx`）。

1. Tavily根据这份名单爬虫，每一家都以`json`文件方式保存到`Crawler`目录。

1. 源代码查看`crawler.py`

### 总结归纳

1. `Tavily`收集回来原始数据需要清洗，减少幻觉现象。`filter`包括：

+ 原始数据需包含参展商公司名字

1. 选择一个大语言模型基座进行信息归纳。考虑因素如下：

+ `Context windows`越大，可从越多原始数据中找到产品相关信息。

+ 价格成本。原本没做限制，100k tokens原始文档直接给到GPT4，一次请求就1美金。后来限制单次请求限制10k tokens，而且转为GPT 3.5模型，成本才得到控制。

+ CES是海外活动，英语语料为主，不考虑中文基座模型。

| 模型 | 优点 | 缺点 |

| --- | --- | --- |

| Google Gemini | 32k context windows | prompt指令语义理解非常差，需设置多条明确指令才懂执行。 |

| ChatGPT 4.0 | 128k context windows | 太贵了。全部跑这个接口需要4000美元。 |

| ChatGPT 3.5 | 16k context windows | 信息归纳效果勉强可以，价格可以接受。 |

Python执行过程

+ 对`crawler`目录爬虫数据进行逐一归纳，另存到`Summary`目录下。

+ 源代码查看`summary.py`

### 幻觉修复

ChatGPT 3.5回应中有15%的幻觉，更换ChatGPT 4修复。

原因如下

+ 数据质量低，没有厂商的信息，Tavily只找到CES overall的信息。

+ ChatGPT 3.5无法区别品牌与CES之间关系，会让CES热点信息代替品牌信息。

解决方案

+ 更换信息源，换成Azure Bing Search，增加信息覆盖范围。因请求量较大，请使用付费接口，避免免费额度不足导致中断。

+ 更换大语言模型，换成ChatGPT 4.0。

Python代码执行

+ 在`Summary`目录检索一些幻觉关键字，如“transparent”、“rabbit r1”等，这些都是CES 2014 热门词汇。

+ 当`json`文件出现这些词汇，`python`会提取该文件的品牌名称，在Bing再搜索一次让ChatGPT4归纳修复。

+ 源代码`check.py`

### 翻译输出

经过多方测试，使用ChatGPT 3.5 API翻译原`summary`目录下`json`文件（源代码`translation.py`），然后再Merge合并输出`xlsx`文件（源代码`output.py`）。

|  | 优点 | 缺点 |

| --- | --- | --- |

| Google Translation | 支持`xls`直接翻译 | 翻译后格式丢失，阅读困难 |

| Azure Translation | 每个月免费100万字翻译 | 免费接口非常不稳定 |

| DeepL | 翻译专业 | 贵 |

| ChatGPT3.5 | 接口稳定，价格便宜。 | 需要写一个Python脚本 |
