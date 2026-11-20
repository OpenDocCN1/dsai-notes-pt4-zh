
# DB-GPT: 用私有化LLM技术定义数据库下一代交互方式

> 来源：[https://xuqiwei1986.feishu.cn/docx/Fr2fdzE7DomVcYxwtNIcOluWnlc](https://xuqiwei1986.feishu.cn/docx/Fr2fdzE7DomVcYxwtNIcOluWnlc)

> 🔗 原文链接： h[ttps://blog.csdn.n](https://blog.csdn.net/sinat_37574187/article/details/131735236?ops_request_misc=&request_id=&biz_id=102&utm_term=LLM%E7%A7%81%E6%9C%89%E5%8C%96&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-131735236.142^v92^insert_down1&spm=1018.2226.3001.4187)et/sinat_3757418...

## DB-GPT 是什么？

随着大模型的发布迭代，大模型变得越来越智能，在使用大模型的过程中，遇到极大的数据安全与隐私挑战。在利用大模型能力的过程中我们的私密数据跟环境需要掌握自己的手里，完全可控，避免任何的数据隐私泄露以及安全风险。基于此，我们发起了DB-GPT项目，为所有以数据库为基础的场景，构建一套完整的私有大模型解决方案。此方案因为支持本地部署，所以不仅仅可以应用于独立私有环境，而且还可以根据业务模块独立部署隔离，让大模型的能力绝对私有、安全、可控。我们的愿景是让围绕数据库构建大模型应用更简单，更方便。

DB-GPT 是一个开源的以数据库为基础的GPT实验项目，使用本地化的GPT大模型与您的数据和环境进行交互，无数据泄露风险，100% 私密

[DB-GPT视频介绍](https://www.bilibili.com/video/BV1SM4y1a7Nj/?buvid=551b023900b290f9497610b2155a2668&is_story_h5=false&mid=%2BVyE%2Fwau5woPcUKieCWS0A%3D%3D&p=1&plat_id=116&share_from=ugc&share_medium=iphone&share_plat=ios&share_session_id=5D08B533-82A4-4D40-9615-7826065B4574&share_source=GENERIC&share_tag=s_i&timestamp=1686307943&unique_k=bhO3lgQ&up_id=31375446)

## 效果演示

示例通过 RTX 4090 GPU 演示

`dbgpt_demo.mp4`

### 根据自然语言对话生成分析图表

![](img/7a14f363f3d5b740fc2b4283d1807110.png)

### 根据自然语言对话生成SQL

![](img/edf02d860da67520d6356d07a646b6e3.png)

### 与数据库元数据信息进行对话，生成准确SQL语句

![](img/371734bee1b4a367401fd6e03cb5a330.png)

### 与数据对话，直接查看执行结果

![](img/f450b656617c5f1bcde53985823d727a.png)

### 知识库管理

![](img/92cd8bde51d51b7cf4b042277707a86f.png)

### 根据知识库对话，比如`pdf`、`csv`、`txt`、`words`等等。

![](img/b24cb924fe8c9f2ee5327944f176b8d7.png)

## 特性一览

目前我们已经发布了多种关键的特性，这里一一列举展示一下当前发布的能力。

+ `SQL` 语言能力
+ `SQL`生成
+ `SQL`诊断
+ 私域问答与数据处理
+ 知识库管理(目前支持 `txt`, `pdf`, `md`, `html`, `doc`, `ppt`, and `url`)
+ 数据库知识问答
+ 数据处理
+ 插件模型
+ 支持自定义插件执行任务，原生支持`Auto-GPT`插件。如:
+ `SQL`自动执行，获取查询结果
+ 自动爬取学习知识
+ 知识库统一向量存储/索引
+ 非结构化数据支持包括`PDF`、`MarkDown`、`CSV`、`WebURL`
+ 多模型支持
+ 支持多种大语言模型, 当前已支持`Vicuna(7b,13b)`, `ChatGLM-6b(int4, int8)`, `guanaco(7b,13b,33b)`, `Gorilla(7b,13b)`
+ TODO: `codet5p`, `codegen2`

## 架构方案

DB-GPT基于`FastChat`构建大模型运行环境，并提供`vicuna`作为基础的大语言模型。此外，我们通过`LangChain`提供私域知识库问答能力。同时我们支持插件模式，在设计上原生支持`Auto-GPT`插件。我们的愿景是让围绕数据库和LLM构建应用程序更加简便和便捷。

整个DB-GPT的架构，如下图所示

![](img/0f50b48ac14847467909ac863fe7dd5d.png)

核心能力主要有以下几个部分。

1. 知识库能力：支持私域知识库问答能力
1. 大模型管理能力：基于`FastChat`提供一个大模型的运营环境。
1. 统一的数据向量化存储与索引：提供一种统一的方式来存储和索引各种数据类型。
1. 连接模块：用于连接不同的模块和数据源，实现数据的流转和交互。
1. Agent与插件：提供`Agent`和插件机制，使得用户可以自定义并增强系统的行为。
1. Prompt自动生成与优化：自动化生成高质量的`Prompt`，并进行优化，提高系统的响应效率。
1. 多端产品界面：支持多种不同的客户端产品，例如Web、移动应用和桌面应用等。

## 安装

[快速开始](https://db-gpt.readthedocs.io/projects/db-gpt-docs-zh-cn/zh_CN/latest/getting_started/getting_started.html)

### 多语言切换

在`.env` 配置文件当中，修改 `LANGUAGE` 参数 来切换使用不同的语言，默认是英文(中文`zh`, 英文`en`, 其他语言待补充)

## 使用说明

### 多模型使用

[使用指南](https://db-gpt.readthedocs.io/projects/db-gpt-docs-zh-cn/zh_CN/latest/modules/llms.html)

如果在使用知识库时遇到与`nltk`相关的错误，您需要[安装`nltk`工具包](https://www.nltk.org/data.html)。更多详情，请参见：`nltk`文档 Run the Python interpreter and type the commands:

```
>>> import nltk
>>> nltk.download()
```

我们提供了全新的的用户界面，可以通过我们的用户界面使用DB-GPT， 同时关于我们项目相关的一些代码跟原理介绍，我们也准备了以下几篇参考文章。

1. [大模型实战系列(1) —— 强强联合`Langchain`-`Vicuna`应用实战](https://zhuanlan.zhihu.com/p/628750042)
1. 大模型实战系列(2) —— DB-GPT 阿里云部署指南
1. 大模型实战系列(3) —— DB-GPT插件模型原理与使用

## 感谢

项目取得的成果，需要感谢技术社区，尤其以下项目。

+ `FastChat` 提供 chat 服务
+ `vicuna-13b` 作为基础模型
+ `langchain` 工具链
+ `Auto-GPT` 通用的插件模版
+ `Hugging Face` 大模型管理
+ `Chroma` 向量存储
+ `Milvus` 分布式向量存储
+ `ChatGLM` 基础模型
+ `llama-index` 基于现有知识库进行 `In-Context Learning` 来对其进行数据库相关知识的增强。

## 贡献

+ 提交代码前请先执行 `black`。

这是一个用于数据库的复杂且创新的工具，我们的项目也在紧急的开发当中，会陆续发布一些新的`feature`。如在使用当中有任何具体问题，优先在项目下提`issue`，如有需要，请联系如下🛰️，我会尽力提供帮助，同时也非常欢迎大家参与到项目建设中。

## 路线图

![](img/ee727a125da77e2e04c94d679097ee11.png)
