
# langchain全景图

> 来源：[https://bluefocus.feishu.cn/docx/MYiidFsnfouuZixKeo7cYJHOnWh](https://bluefocus.feishu.cn/docx/MYiidFsnfouuZixKeo7cYJHOnWh)

LangChain是一个开源框架，专注于简化和加速在应用程序中集成及利用大型语言模型（LLMs）的过程。它为开发者提供了一套丰富的工具、组件和接口，使得与复杂语言模型的交互变得更为直接和高效。LangChain的核心特点和功能包括：

1. 通用的LLM接口：支持多种主流的大型语言模型，如GPT-3、GPT-4、LaMDA等，通过统一的接口进行访问和操作，降低了模型切换的成本。
2. 提示管理与定制：允许用户根据应用场景自定义提示模板，优化语言模型的输入，以获得更精准的输出结果。
3. 链式结构设计：通过“链”这一概念，可以将不同的处理步骤和组件串联起来，形成复杂的逻辑流程，便于构建高级应用，如多轮对话系统、复杂决策支持等。
4. 外部数据与工具集成：支持集成外部API、数据库和其他数据源，使得语言模型能够访问和利用更多上下文信息，扩展其功能和应用范围。
5. LangChain表达语言（LCEL）：这是一种声明式语言，用户可以用它来描述处理流程，从简单的模型调用到复杂的逻辑链，便于非程序员也能快速构建应用原型，且支持从原型到生产环境的平滑过渡。
6. 支持多语言：LangChain框架目前支持Python和JavaScript两种编程语言，便于不同背景的开发者使用。
7. 开源与社区驱动：LangChain遵循MIT许可，拥有活跃的开发者社区，不断更新和增加新的功能模块，保持框架的前沿性和实用性。

总之，LangChain作为一个高度模块化、灵活且强大的工具集，旨在帮助开发者快速将先进的语言模型技术融入到各类应用程序中，无论是创建聊天机器人、问答系统、内容生成还是数据分析等，都能通过LangChain实现更高效、低门槛的开发。

**langchain项目目标**

[通过LLM的组合创建应用程序万](https://link.zhihu.com/?target=https%3A//github.com/hwchase17/langchain)花筒，目前GitHub Star超过2万，

## 项目架构

下图中每个部件其实都是一类集合，我们以汽车（人）来类比一个基于LangChain的Agent，核心组件包括：

+ 系统发动机（心脏）：开源Models以及其封装的LLMS
+ 汽油（血液）：Prompt
+ 方向盘（大脑）：Memory
+ 前进（创造新应用）：LLMChain
+ 听流行音乐（链接千行万业）：Tools
+ 播放珍藏光碟（链接垂直私有领域）：Index私有数据
+ 开车旅行（开放生态）：Agent智能

下面是一张简图：

![](img/95a05829285c5a8f7b4bbecced3b5184.png)

草图

## 项目全景图

整体分为：基础层、能力层、应用层三部分（如果不清晰，建议点击github浏览）

![](img/023c53a48513f072641ac6cd640b0fa3.png)

## 基础层

包括：Models、LLM、Index三层：

### Models层：

吐血整理最近3年左右的模型、详细文档。可以看出各家大模型的演进历史，其中Google、OpenAI、Meta、DeepMind领先优势非常大，国内大厂唯独腾讯缺席。有几个点：

+ 目前最主要的几个大模型为： Google的LaMDA、Meta的LLaMa、OpenAI的GPT-4、DeepMind的：Chinchilla
+ 另外目前投入应用的智能终端基本都是基于模型的Fine-Tuning或者RLHF技术，并且需要大量的相关行业训练集，典型比如code领域、财经领域BloombergGPT、学术论文编写等，其中在作者最关注的Code细分领域，目前的模型有：
  + OpenAI的Codex
  + DeepMind的AlphaCode
  + Saleforce的CodeGen

Models层的输出包括：

+ 文本自动生成
+ embeding向量化输出
+ 多模态输出

### LLMS层：

这一层主要强调对models层能力的封装以及服务化输出能力，主要有：

+ 各类LLM模型管理平台：强调的模型的种类丰富度以及易用性
+ 一体化服务能力产品：强调开箱即用
+ 差异化能力：比如聚焦于Promp管理、基于共享资源的模型运行模式等等

### Index层：

对用户私域文本、图片、PDF等各类文档进行存储和检索，这里有两个方案：

+ Vector方案：即对文件先切分为Chunks，在按Chunks分别编码存储并检索
+ KG方案：这部分利用LLM抽取文件中的三元组，将其存储为KG供后续检索

## 能力层

如果基础层提供了最核心的能力，能力层则给这些能力安装上手、脚、脑，让其具有记忆和触发万物的能力，包括：Chains、Memory、Tool三部分

### Chains层：

按照不同的需求抽象并定制化不同的执行逻辑，Chain可以相互嵌套并串行执行，通过这一层，让LLM的能力链接到各行各业比如面向私域数据的`load_qa_with_sources_chain`; 比如面向SQL数据源的`SQLDatabaseChain`；再比如能自动生成代码并执行的`LLMMathChain`等等，这里面比较让人眼前一亮的点有：

+ `ConstitutionalChain`：对最终结果进行偏见、合规问题处理的逻辑，保证最终的结果符合价值观
+ `LLMCheckerChain`：能让LLM自动检测自己的输出是否有没有问题的逻辑

### Memory层：

这层主要有两个核心点：

+ 对Chains的执行过程中的输入、输出进行记忆并结构化存储，为下一步的交互提供上下文，这部分简单存储在Redis即可
+ 根据交互历史构建知识图谱，根据关联信息给出准确结果

### Tools层：

其实Chains层可以根据LLM + Prompt执行一些特定的逻辑，但是如果要用Chain实现所有的逻辑不现实，可以通过Tools层也可以实现，Tools层理解为技能比较合理。典型的比如搜索、维基百科、天气预报、chatGPT服务等等。

## 应用层

有了基础层和能力层，我们可以构建各种各样好玩的，有价值的服务，这里就是Agent

### Agent层：

Agent层可以根据Tool和Chain组合出特定的服务来，最终实现以更自然的文本交互形态完成用户特定需求的目标，比如用自然语言的方式实现sql的操作等等。

> 原文链接:https://zhuanlan.zhihu.com/p/620315617
