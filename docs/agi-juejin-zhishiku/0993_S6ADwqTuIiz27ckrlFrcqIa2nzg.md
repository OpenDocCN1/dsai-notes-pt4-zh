# RAG优化: 输入信息分类和二次优化

> 来源：[https://agijuejin.feishu.cn/docx/ThrydoaA0oov9ZxLBhdcSjTvnvd](https://agijuejin.feishu.cn/docx/ThrydoaA0oov9ZxLBhdcSjTvnvd)

> 作者：王大锤

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/714614507)hihu.com/p/714614...

## 一、背景

### 1.1 问题定义

在真实的RAG应用场景中，不同的对话请求，可能会需要RAG系统采取不同的算法逻辑进行回复。如:

> 场景1: 从相关文档中找到相关[资料，生成回](https://www.zhihu.com/search?q=%E7%BA%B3%E7%A8%8E%E6%80%BB%E9%87%91%E9%A2%9D&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)复答案。

> 如: 如何查看个人养老保[险?

> 场景](https://www.zhihu.com/search?q=%E6%99%BA%E8%83%BD%E7%BB%98%E5%9B%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)2: 从文档中找到相关资料，并进行数学计算返回正确的答案。

> 如: 2023年社保缴纳人数对比2022年增加了多少?

> 场景3: 从文档找到相关资料，并以指定的形式返回结构化的数据。

> 如: 以json格式抽取今年 纳税总金额 最高的市区名称和纳税总金额，以市区名称为主键，纳税总金额为键值。

> 场景4: 从文档中抽取相关的信息资料，然后推理生成答案，最后根据数据结构 智能绘图 。

> 如: 查看2023年杭州市各种企业类型纳税总金额的分析，并以饼图展示其结果。

如何才能缓解上述问题，提高RAG系统的理解能力和回答质量呢?

### 1.2 方案简介

本文主要介绍基于问题的分类和查询优化两种方案，并对作者在这些技术方案的理解和思路做相关说明。

## 二、分类优化(Adaptive-RAG)

### 2.1 算法流程

Adaptive-RAG 提出了一种创新的 adaptive framework （译者注：该系统可以根据 query 的复杂程度动态选择最合适的信息检索和生成策略。）。如图所示，该系统可根据 query 的复杂度，动态选择最合适的 LLMs 使用策略（包含从最简单（the simplest）到最复杂（the most complex）的多种策略）。

![](img/c960660b3690b49f629b37986688a944.png)

A描绘的是一种 [**单步方法 **](https://www.zhihu.com/search?q=%E5%8D%95%E6%AD%A5%E6%96%B9%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)[（single](https://www.zhihu.com/search?q=%E5%A4%9A%E7%BA%A7%E9%80%BB%E8%BE%91%E6%8E%A8%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)-step approach） ，这种方法会先检索出相关文档，然后生成答案。但这一方法对于那些需要 多级逻辑推理 （multi-step reasoning）的复杂 query 而言，可能精度不足。

B是一种 分多个步骤进行处理（multi-st[ep pr](https://www.zhihu.com/search?q=%E4%B8%AD%E9%97%B4%E7%AD%94%E6%A1%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)ocess） 的方法，包括迭代进行文档检索（document retrieval）及生成 中间答案 （generation of intermediate responses）等步骤。虽然这种方法效果比较好，但由于需多次调用大语言模型（LLMs）和检索器（retrievers），处理简单 queries 时效率不太高。

C是一种 **adaptive **（译者注：可根据具体情况选择具体的策略。）的方法，通过精心设计的分[类器（cl](https://www.zhihu.com/search?q=%E8%BF%AD%E4%BB%A3%E6%A3%80%E7%B4%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)[assif](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E6%96%B9%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)iers），我们能够更精准地判断、选择最佳检索策略（是选择 迭代检索 （iterative）、一次性检索（single），还是不使用 检索方法 （no retrieval methods））。

为了帮助大家更直观地理解 Adaptive-RAG 的工作流程，本文会结合具体代码来加以说明。目前，该技术存在四个不同版本的代码实现，它们分别是官方版本（official version）[2]、Langchain** 版本[3]、LlamaIndex 版本[4]以及 Cohere 版本[5]。

![](img/d3ffaef0e1ae3415fb6024970699755f.png)

代码的运行方式会根据 query 的复杂程度而产生变化，并相应地调用不同的工具：

+   **面对复杂的 queries **：需要多个工具协同工作，这些工具会从多份文档中提取信息。

+   **针对简单的 queries **：仅需单个工具从单个文档中获取所需上下文。

+   处理直接明了的 queries ：直接调用 LLMs 给出答案。

我们可以通过分类器（classifier**）来挑选合适的工具。但与官方版本不同， 此处**使用的分类器并未针对该应用场景进行过针对性地训练，而是直接应用现成的 LLMs ，这一做法有明确描述。

![](img/c98c4eaadaa6746c30f4fe5891765afc.png)

### 2.2 构建分类器

虽然 LlamaIndex 版本的代码实现并没有分类器的构建这一环节，但深入了解分类器的构建过程，对于我们的后续开发工作有着至关重要的作用。

### 2.3 构建数据集

在该技术的实现过程中面临一个重大挑战，我们缺乏带有 query-complexity pairs（译者注：query 与其相应的复杂度（complexity）的配对数据。）的训练数据集。那么，我们该如何应对这一问题呢？Adaptive-RAG 采用了两种策略，以自动化的方式创建所需的训练数据集（training dataset）。

根据 Adaptive-RAG 提供的数据集，我们可以看到，其对分类器训练集的数据标注工作，是依托于那些已经公开并带有标签 的问答数据集完成的。

存在两种处理策略：

**对于用户上传的 q**[**uery ，**](https://www.zhihu.com/search?q=%E9%9D%9E%E5%9F%BA%E4%BA%8E%E6%A3%80%E7%B4%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)** 若使用最简易的、 非基****于检索 **的方法能够得出正确答案 ，**那**么对应**的** query 就会被打上 ‘A’ 的等级标签。同样的逻辑， [通过单步](https://www.zhihu.com/search?q=%E5%A4%9A%E6%AD%A5%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)方法（single-step approach）能够得到正确答案的 query 会标记为 ‘B’ 等级 ，而 通过分多个步骤进行处理（multi-step process）的方法正确解答的 query 则会被标记为 ‘C’ 等级 。不过有一点需要在此强调，较为简单的模型优先级更高。也就是说，当单步法（single-step）和 多步法 （multi-step）均能给出正确答案，但非基于检索的方法无法做到时，就会给该 query 打上 ‘B’ 的等级标签。

```
{
    &quot;answer&quot;: &quot;B&quot;,
    &quot;dataset_name&quot;: &quot;musique&quot;,
    &quot;id&quot;: &quot;sdsada&quot;,
    &quot;question&quot;: &quot;when did the headquarters city of the Fireball Cinnamon Whiskey manufacturer electits first black mayor?&quot;,
    &quot;total_answer&quot;: [
        &quot;multiple&quot;,
        &quot;one&quot;
    ]
}
```

倘若上述三种方法均未能生成正确答案，则说明有些问题仍未[被标注分类。](https://www.zhihu.com/search?q=%E5%85%AC%E5%BC%80%E6%95%B0%E6%8D%AE%E9%9B%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)这种情况下，我们会直接根据 公开数据集 进行分类。具体而言，单步数据集（single-hop dataset）中的 query 会被标注为 ‘B’ 等级，而多步数据集（ multi-hop dataset ）中的 query 则会被标注为 ‘C’ 等级。

### 2.4 训练和推理

训练方法采用 [交叉熵损失函数 ](https://www.zhihu.com/search?q=%E4%BA%A4%E5%8F%89%E7%86%B5%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)（cross-entropy loss），基于这些自动收集的 query-complexity pairs （译者注：query 与其相应的复杂度（complexity）的配对数据。）来训练分类器。

然后，在推理过程中，我们将 query 输入至分类器，即可判定 query 的复杂度等级，该等级标签可为 'A'、'B' 或 'C' 中的任意一种：o = Classifier(q)。

### 2.5 模型选择

![](img/d7825138a8db3cbd32021b129815468a.png)

可以看出， **无论 **[**分类器模型 **](https://www.zhihu.com/search?q=%E5%88%86%E7%B1%BB%E5%99%A8%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)的 size 如何，其性能表现并无明显差异。 即便是小型模型也能维持相同水平的性能，有利于提高资源利用效率。

## 三、查询优化(RQ-RAG)

[针对上述挑](https://www.zhihu.com/search?q=%E4%BC%98%E5%8C%96%E6%96%B9%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)战，RQ-RAG 提出了三项 优化方法 。

+   对于 日常问候（daily greetings）等这类简单 query ，加入额外的上下文反而可能降低大模型的回复质量。对于这种情况，大语言模型应当直接做出响应，而非添加不必要的上下文信息，以避免造成大模型的回答质量下降。换句话说，如图 7 左上方所示，模型应具备按需应答（respond on demand）的能力。

+   **面对复杂的 query **[，RQ-RAG 会](https://www.zhihu.com/search?q=subquery&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)将其细分为若干个更易于解答的 subquery 。逐一检索 subquery 的相关信息，从而形成对原始复杂 query 的完整响应。

+   遇到含义模糊、可能有多重解释的 query 时，仅使用原始的 query 文本进行检索是远远不够的。大语言模型必须掌握 query 文本的具体细节、理解用户的真实意图并制定出针对性的检索方案。

这种方法确保了检索到的信息既全面又精准，从而更加有效地回答问题。

RQ-RAG 通过端到端（end-to-end）的方式 training （译者注：Llama2 是一个预训练模型，此处的 training 应当是指微调。）一个 Llama2 7B 模型。使得该模型能够动态地通过重写（rewriting）、分解（decomposing）和消除 query 中的歧义来增强 query 的检索效果。

![](img/87a2c5d0a9c6c718a88bb46fa0754bb4.png)

### 3.1 构建数据集

如上图展示了整个数据准备流程：

1.  首先，将搜集到的任务池按前文所述的三种类别进行分类。这一步比较简单，因为每个数据集都对应特定的数据类型。

1.  接着，我们针对每种数据集类型，使用C[hatGP](https://www.zhihu.com/search?q=%E6%9F%A5%E8%AF%A2%E8%AF%AD%E5%8F%A5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)T和预设的提示模板生成精炼的 查询语句 。利用这些查询语句，我们从外部数据源中检索信息，通常以DuckDuckGo为主，把检索过程当作一个不透明的黑盒子处理。

1.  之后，我们引导ChatGPT根据这些精炼的查询和相关上下文，生成新的响应。通过这样的反复过程，我们累积了大约4万个案例。

[注：Duc](https://www.zhihu.com/search?q=%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)kDuckGo是一款互联网[ 搜索引擎 ](https://www.zhihu.com/search?q=%E4%B8%AA%E6%80%A7%E5%8C%96%E6%A3%80%E7%B4%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)，其注重用户隐私，及避免 个性化检索 所致的过滤气泡。它与其他搜索引擎不同的地方在于其不会分析自己的用户、对所有使用同一组关键词的用户显示同样的结果。它也强调返回最佳结果，而不是最多网站链接之结果。

### 3.2 模型训练

![](img/eb3229c599ac70857517cc3b94098189.png)

### 3.3 答案选择

每次迭代时，该模型都会针对特定需求生成多种用于检索的 query 语句，比如对 query 进行重写、分解或消除其歧义。这些 query 反过来又能获得不同的上下文，有助于模型更全面、更灵活地处理复杂任务（leading to the diversification of expansion paths）。

![](img/90120190bc27598e116975332dfc8a4a.png)

如图所**示，R**[**Q-RA**](https://www.zhihu.com/search?q=%E7%BD%AE%E4%BF%A1%E5%BA%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)G 研发了一套树形解码策略（tree decoding strategy），并使用了三种选择机制[13]： 基于困惑度（PPL）的选择方法 、 基于 置信度 （Confidence）的选择方法 以及 基于集成学习（Ensemble）的选择方法 。

在基于 [困惑度 ](https://www.zhihu.com/search?q=%E5%9B%B0%E6%83%91%E5%BA%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)（P[PL）的选择方法](https://www.zhihu.com/search?q=%E7%B4%AF%E7%A7%AF%E7%BD%AE%E4%BF%A1%E5%BA%A6%E5%88%86%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)中，模型会选择所有输出中困惑度（PPL）最低的答案。基于置信度（Confidence）的选择方法则是选择所有置信度最高的结果。而基于集成学习的选择方法，则倾向于选取 累积置信度分数 （confidence score）最高的最终结果。

## 四、相关总结

## 4.1 这些技术与 Self-RAG 和 CRAG 进行对比

不同于 Adaptive-RAG 和 RQ-RAG 在检索前对原始 query[ 进行优化的做法，S](https://www.zhihu.com/search?q=retrieval&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)elf-RAG和 CRAG的关注重点在于判断何时执行检索（ retrieval ）操作以及如何优化检索操作之后的信息处理效率。特别值得一提的是，CRAG 通过重写用于网络检索的 query 语句，提升了检索结果的信息质量。

RQ-RAG 和 Self-RAG 均通过训练小型语言模型的方式来替代原有的大模型（LLMs）。相比之下，Adaptive-RAG 和 CRAG 保留了原有模型，仅是新增了对 query 进行分类或评估的两个功能层。

后起之秀 Adaptive-RAG 和 RQ-RAG 都声称自己的性能优于 Self-RAG，在它们的论文中都有对应的实验报告。

从[生成流程（](https://www.zhihu.com/search?q=%E6%A0%91%E5%BD%A2%E8%A7%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A714614507%7D)generation process）的角度考量，Self-RAG、CRAG 及 Adaptive-RAG 因未采用复杂的 树形解码 策略（tree decoding），显得更为简洁明快。

## 4.2 技术实践过程中发现的一些问题

当 query 转化为多轮对话的情况时，利用大语言模型处理冗长的提示词数据可能会造成响应延时。根据我目前的理解，采用并行处理技术（parallelization）或许能有效解决这一问题。

此外，无论是 Adaptive-RAG 还是 RQ-RAG 技术，它们都对 query 进行了分类。但这些分类方式是否真正达到了最优状态？它们是否能完美适用于特定的生产场景？有没有可能采用其他分类策略能取得更好的效果？需要通过一系列对比实验（comparative experiments）来验证这些观点。

## **4.3 小模型优势**

RQ-RAG 的实践过程表明，即使是一个 7B 参数量的模型，只要数据集构建得当、生成流程精细， 7B 参数量的模型也能创造卓越的性能表现。

盲目追求模型规模的庞大并不一定等同于更高的性价比。对于那些资源有限的团队而言，专注于优化数据集与精进算法或许是更为明智的选择。