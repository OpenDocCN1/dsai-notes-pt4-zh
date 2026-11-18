
# RAG实践、提升RAG效果方法梳理

> 来源：`https://qqs7y1hozd1.feishu.cn/docx/PY81d4AWTogwrCxtCqlccOzqnJc`

## 一、RAG实践

### （一）背景

当前大模型的应用还是处于比较初期的阶段，以物流领域常见场景为例，可以看到一些行业常见要求和当前大模型能力有较多冲突。

在这些业务生产环境应用之前，还是希望先在容错性较高的场景进行一些实践。因此团队尝试基于大模型来优化答疑机器人，为后续更多的深度应用积累实践经验。（下面的示例图片由大模型生成，仅供参考）

![](img/e7ce9f25224f6c12f1dc4d1a28cb89d7.png)

### （二）传统答疑机器人的痛点

在大模型崛起之前，传统答疑机器人一般有两种模式：

基于多级目录分类，让用户慢慢翻到想了解的知识。

基于传统关键词检索方式，根据用户提问，在知识库中找到和用户提问相关的知识。

核心痛点是，不能快速准确找到用户想寻找的答案。

由于答疑机器人受众不是 C 端普通用户，而是内部作业人员，因此机器人给出的回答在语气或者回答方式上不需要过多润色。

但团队希望它具备以下一些特点：

可以准确理解提问人员自然语言提问的语义给出标准答案；

希望可以比较快速给出答案（比如 5s 之内）；

不希望答非所问，提供不属于业务范围内的回答，如果提问的问题确实没有答案，希望拒绝回答并引导到人工。

### （三）迭代过程

团队结合业内经验和自身诉求，进行下五个迭代，下面详细展开。

#### 阶段一：向量搜索

嵌入（Embedding）是一种向量化方法，把文本，图像，视频等数据转换为向量，同时特别强调保留数据之间的语义关系。比如“苹果”和“橘子”要比“苹果”和“太阳”的向量值更为接近（语义相似）。

Embedding 模型服务：提供具体的 Embedding 能力的在线服务，可以直接调用 OpenAI，阿里云等远程服务，也可以本地部署。低成本试用可以尝试阿里云灵积。

向量数据库是用于存储和检索高维向量数据的数据库系统，特别擅长相似性搜索。低成本试用可以使用阿里云的 Hologres 数据库。

向量搜索可以类比传统的关系型数据库检索，只不过向量搜索是按照语义来进行相似度匹配搜索。

可以看到一端文本转为向量存在向量数据库中的字段展示，根据不同的 Embedding 模型维度也有不同，通常达到上千维。

##### 1、向量搜索流程

下图是一个简化版的向量搜索流程图，分为数据准备和在线推理两个部分：

数据准备部分，需要把历史沉底的知识库数据清洗为多个 QA 问答对的格式，把问题，答案，问题对应的向量，存储在向量数据库。

在线推理部分，先把用户提出的问题通过向量服务转为向量，在向量数据库中找到与该问题最为相似的一条数据，然后通过相似度阈值来判断用户提问的问题和数据存储的标准问题是否足够相似，来决定是否把对应的答案返回给用户。

![](img/8f958432b3f66b724b6248ebc9f20259.png)

##### 2、效果分析

假设有这样一个标准问答对：

Q：遇到不可抗力因素无法配送，如何操作？

A：1. 系统进行提报，详细步骤 XXX

![](img/dc129d154ea6faa654734683a6c23023.png)

尝试下来，纯靠向量搜索的优势在于：对于原始问题或者细微修改的问题，可以比较快速、准确找到答案。

不足之处：

+ 问题较长且多个问题语义相近时，准确率下降

+ 向量距离的阈值不好把控，对于无关问题的拒绝回答机制不好处理。

#### 阶段二：RAG

RAG 是知识问答领域业内落地最多的实践，同时 RAG 也是应用很广且很容易上手的一种大模型应用方式。

RAG（Retrieval-Augmented Generation 检索增强生成）：因为大模型本身没有垂域的知识，RAG 其实就是把“查资料”和“写答案”这两个步骤结合在一起。先用检索系统找到一些跟问题相关的资料，然后再用大模型（类似于 GPT）来编写一个详细的答案，来提高AI回答垂直领域问题的准确性。

RAG 就像让一个大学理科生（知识储备丰富的大模型）拿着初中生的历史课本做开卷考试（相似度检索），虽然他本来不会，但靠查资料准确率还可以（效果不错）。就算书上没有，他也可以凭借自己的本事写满试卷（存在幻觉）。

##### 1、RAG 流程

下图是简化版本的通过 RAG 实现知识问答的流程图，可以看到，整体和第一阶段的向量搜索是比较类似的：

在数据准备阶段，这里的原始数据可能是大量的文档资料，需要尽量清洗为统一格式的文档，每个文档都按照一定的规则进行分段。这里的分段规则和大小需要多次尝试，来保证最终的检索速度和总结效果。然后把分段后的知识向量化，存入向量数据库。

在线推理阶段，首先要做的事情是把用户提问的问题从数据库中检索出 `N` 条相似的分段，作为下一步大模型总结的参考资料，然后大模型拿着这些资料按照要求，总结出一个答案给到提问者。

![](img/c74232fcc2c30c7534e7c688d4988354.png)

##### 2、RAG 上手

目前业内有很多现成的产品可以快速体验 RAG，整体可配置的交互和原理大同小异。

平台对应的文档中心有详细的实践教程，在准备好基础文档的前提下，基本五分钟就可以搭出来一个智能体应用。

##### 3、效果分析

优势：

【正确率高】正确率基本可以达到 90% 以上。

【话术控制】回复话术可以根据 `prompt` 较为灵活的控制。包括期望的语气，回复格式等。

【兜底返回】可以控制知识库没有的内容拒绝回答，只是返回固定文案。

不足：

耗时较长，经常一次完整回复需要 10s 以上

按调用量产生调用费用，QPS 较高时，费用也会较高

因为允许大模型发挥，因此会存在幻觉的情况，也就是会有偶发的答非所问的 case。

#### 阶段三：SFT

上面两种方案各有优劣，之后团队继续尝试了垂域模型微调技术，看看能不能训练出一个无所不知的垂直领域小模型。

先贴一些相关的概念：

PT（Pre-Training 预训练），使用大量机器和未标记数据，如 GPT-4、Qwen2-7B 等；

SFT（Supervised Fine-Tuning 监督式微调），使用少量标注好的领域数据来训练基座大模型，来获得可处理专属领域任务的专有模型过程；

FFT（全参数微调），模型整个参数结构和权重会变，需要资源多，训练彻底，但容易过拟合和灾难遗忘；

lora 微调（一种常用的部分参数微调方式），通过矩阵运算原理，只微调小部分参数，达到和全参数微调类似的不错效果；

Instruction-Tuning（指令微调），旨在提高对多种自然语言指令的多任务处理能力，训练数据要求特定格式。

SFT 的过程就像一个家长教五岁（一个懂不少东西的小模型）的小孩背 20 篇古诗，背了三天发现还是不会（欠拟合），背了三个月发现挺好的（到位），可以倒背如流，背了三年，还是只会这几首（过拟合），不仅不会自己写诗（并没有理解诗的本质），而且 1 + 1 等于几都不知道了。(灾难性遗忘)

##### 1、SFT 流程

一个 SFT 的简化版流程图如下所示，一般分为三块内容：

构建数据，高质量的数据非常重要，数据量的大小和质量会极大影响最终模型的效果。

模型微调和评测部分，是 SFT 最核心的部分，除了选择合适的大模型基座外，重点需要根据评测效果来多次调整训练所需的超参数，直到大模型表现达到自己的预期。

模型部署阶段，可以根据评测结果，选择表现最佳的模型快照进行部署，来为后续提供服务。

![](img/2245add488f85c1e3656f3f90dd31d05.png)

##### 2、SFT 上手实践

目前模型微调平台也有很多，这里使用阿里云百炼平台简单展示下核心步骤。（以下截图来源阿里云百炼平台）

一些比较核心的配置：

循环次数，表示模型看几遍训练资料，根据我们的目的来，测试下来，这里循环次数越多，模型泛化能力越差，但也更加稳定。

学习率：模型开始更新参数时的幅度，可以决定是否学习到数据特征。

批次大小：根据训练轮数选择合适的步数，保证一次完整的训练可以保存若干个快照。

等待训练完成观察日志，是否有 `loss`（损失函数）跳跃过大，未收敛等异常情况。

![](img/c7e99ddf64f57780443e4d407f606d07.png)

模型评测：选择刚才训练好的模型进行数据推理，可以人工评测，也可以利用更强大的大模型来进行评测。

对推理数据打分，可以根据团队的要求来选择评测和打分方式，也可以借助平台能力完成打分。

+ 比如可以写个脚本直接比标准答案完全一致

+ 也可以用评测大模型给评测结果打分

测评通过后，将模型部署到平台并进行测试，运维，服务提供等。

##### 3、一些尝试下来的有效手段

数据构造：

通过人工 + 通义千问 72B 等大模型同义改写问题来扩充训练数据数量以及让模型更好学习特征，进行了十倍扩充。

大模型改写：遇到不可抗力因素无法配送，如何操作？-> 遇到不可抗力因素无法配送，该怎么办？

人工改写：遇到不可抗力因素无法配送，如何操作？-> 遇到大雪等特殊情况，不能作业了！

1.打乱训练数据中相似数据的顺序，防止学到顺序相关的错误的特征。

是否有必要放大量无关的数据集？

这里还是和我们训练的期望效果来决定，如果我们希望模型回答稳定，且只希望它处理和训练数据相关的知识，不需要它的通用能力，那这里不需要混合数据训练。

微调：

通过观察 `loss` 收敛曲线和测评效果；调整初始学习率和训练轮数；我们的场景下，一般会把最终 `loss` 收敛在 0.1 以下。

![](img/986565cad673efa7627417c581661bb0.png)

2.尝试当 `loss` 曲线不再收敛时对应的快照作为最终的产出模型，避免过拟合。

评测——构建丰富的评测数据集：

【是否欠拟合】先使用训练集数据进行推理评测，然后再试用改写后的相同语义的测试集进行推理评测，保证两者都有较高分数。

【是否过拟合】对和训练数据语义无关的问题进行评测，看是否答案是训练数据集内的。

##### 4、效果分析

实践下来发现的一些不一定对的认知：

【过拟合】经过 sft，大模型的一些通用能力的遗忘和一些过拟合行为是经常会发生的，不需要完全避免，还是要根据我们的目标来。

【prompt】sft 的小模型，使用常见的提示词工程技巧 构造 `prompt` 提升作用不大，基本保证提问时和训练时的 `prompt` 保持一致即可。

【推理能力】可能和我们较少的原始训练数据（万条）和训练目标（保证准确率）有关，当前微调后的小模型比较难有举一反三的能力，所以构造多样化的训练集和评测集很重要。

优势：

由于使用的 7B 的小模型，因此响应快，一般全量结果 3s 左右可以返回。

准确率高：经过了完整的测评流程产出，只要提问和训练数据语义相关，就可以有较高的准确率。

缺点：

有一定的微调和部署成本。

对于未训练过的问题因为模型本身无法判断，因此会有乱回答的情况。

#### 阶段四：多种技术结合使用

##### 1、SFT + 向量数据库

希望微调后的大模型，对于未训练过的问题直接拒绝回答：

思路：如果 SFT 大模型返回的结果和向量搜索返回的前几条结果相似度不高，认为是未训练过的数据，直接拒绝回答。

##### 2、RAG + SFT

将 SFT 后的模型作为 RAG 里的总结推理模型？

不可行，因为 RAG 部分的总结推理模型，用的是大模型的通用的总结能力，而不是 QA 问答能力，因此这里不适合使用经过 QA 训练数据微调后的模型。

命中 SFT 未训练过的问题时，跳转到 RAG 流程回答

综合考虑 RAG 强大的兜底能力和费用问题，可以根据我们的诉求选择性开启

于是有了让向量搜索，RAG，SFT 各司其职来实现更好问答效果的实践，以下是一次问答的简化版本的流程图：

![](img/6d58d6df0aac3922bc2407263db37390.png)

#### 阶段五：工程优化

【体验优化】丰富点赞点踩，图片和视频回答，展示相似语义问题列表等体验能力。

【自我迭代】数据清洗，模型训练部署等环节通过工程自动化实现模型的自我学习能力（大模型在快速发展，我们需要有快速微调模型并切换的能力，而且未来成本一定会降低）。

【数据沉淀】一线反馈数据，高质量人工答疑数据，统一沉淀为文档，作为后续数据资产。

![](img/2ac2e95c61d989bccb32e1e47cfd3405.png)

## 二、有效提升RAG效果方法的梳理

在大模型实际落地的时候，存在一些问题，主要集中在以下方面：

缺少垂直领域知识：虽然大模型压缩了大量的人类知识，但在垂直场景上明显存在短板，需要专业化的服务去解决特定问题。

存在幻觉、应用有一定门槛：在大模型使用上有一些幻觉、合规问题，没有办法很好地落地，配套工作不足，缺乏现成的方案来管理非结构化文本、进行测试、运营和管理等。

存在重复建设：各业务孤立摸索，资产无法沉淀，存在低水平重复建设，对公司来说ROI低，不够高效。

站在应用的角度，需要一种能够有效解决大模型在垂直领域知识短板、降低应用门槛、提高效率并发挥规模优势的技术方案。当前业内RAG是一种相对有效的解决上面问题的方案(平台化能力、开箱即用垂直私域数据)。

实现一个基本的不难，但要真正优化RAG的效果，还需要投入大量的精力。这里梳理总结一些常见的RAG优化方法，以便日后在实践RAG时提供参考

### （一）RAG介绍

![](img/5586618f9a765a91a58a69abf8fa90dc.png)

RAG 是 "Retrieval-Augmented Generation"（检索增强生成）的缩写，它通过结合检索系统和生成模型来提高语言生成的准确性和相关性。

RAG 的优势在于它能够在生成响应时引入外部知识，这使得生成的内容更加准确和信息丰富，对于处理需要专业知识或大量背景信息的问题尤其有效。随着大型语言模型（LLMs）的发展，RAG 技术也在不断进化，以适应更长的上下文和更复杂的查询。

+ 目前，大部分公司倾向于使用 RAG 方法进行信息检索，因为相比长文本的使用成本，使用向量数据库的成本更低。

+ 而在 RAG 应用过程中，一些公司会使用微调的 Embedding Model，以增强 RAG 的检索能力；另一些些公司会选择使用知识图谱或者 ES 等非向量数据库的 RAG 方法。

+ 大多数第三方个人和企业开发者会使用集成好的 RAG 框架（例如 `llamaindex`、`langchain`、`etcs`），或者直接使用 LLMOps 里内建的 RAG 工具。

#### Dense X Retrieval

探讨在密集检索任务中，选择适当的检索单元粒度（如文档、段落或句子）的重要性，检索单元“命题”（proposition），并通过实验证明，在密集检索和下游任务（如开放域问答）中，基于命题的检索显著优于传统的段落或句子检索方法。

命题定义：命题被定义为文本中的原子表达，每个命题包含一个独立的事实，并以简洁、自包含的自然语言形式呈现。命题代表文本中的原子意义表达，定义如下：

a. 独立意义：每个命题应对应文本中的一个独立意义，所有命题的组合应能表示整个文本的语义。

b. 最小化：命题应是最小的，即不能进一步分割成更小的命题。

c. 上下文自包含：命题应是上下文化且自包含的。即命题应包含所有必要的上下文信息，以便独立理解其意义。

具体的方法：

![](img/e01a4e8b9f2f02b40ae9b5e7aff0d430.png)

具体的步骤：

+ 分割段落为命题（A 部分）

+ 创建 FactoidWiki（B 部分）（命题化处理器将段落分割成独立的命题，从而创建了一个新的检索语料库 FactoidWiki）

+ 比较不同粒度的检索单元（C 部分）

+ 在开放域问答任务中使用（D 部分）

不同粒度的检索单元（段落、句子、命题）进行了实证比较：

![](img/1853c299bd346317d60c2e63f29043ae.png)

![](img/5bb8b4771a97677afed439d07cca53ff.png)

（Recall@5），即在前 5 个检索结果中找到正确答案的比例。

（EM@100），即在前 100 个字中找到正确答案的比例。

+ 图表明了命题检索在提高密集检索的精确性和问答任务的准确性方面的优势。通过直接提取相关信息，命题检索能更有效地回答问题并提供更准确的检索结果。

+ 将段落和句子分解为更小的、上下文自包含的命题会产生更多的独立单元，每个单元都需要存储在索引中，会导致存储增加。

从段落到命题的提示词：（英文版）



# 从段落到命题的提示词

## 英文提示词

```
Decompose the "Content" into clear and simple propositions, ensuring they are interpretable out of context.


1. Split compound sentences into simple sentences. Maintain the original phrasing from the input whenever possible.
2. For any named entity that is accompanied by additional descriptive information, separate this information into its own distinct proposition.
3. Decontextualize the proposition by adding necessary modifiers to nouns or entire sentences and replacing pronouns (e.g., "it", "he", "she", "they", "this", "that") with the full name of the entities they refer to.
4. Present the results as a list of strings, formatted in JSON.
**Input:**
Title: Ìostre. Section: Theories and interpretations, Connection to Easter Hares.
Content:
The earliest evidence for the Easter Hare (Osterhase) was recorded in south-west Germany in 1678 by the professor of medicine Georg Franck von Franckenau, but it remained unknown in other parts of Germany until the 18th century. Scholar Richard Sermon writes that "hares were frequently seen in gardens in spring, and thus may have served as a convenient explanation for the origin of the colored eggs hidden there for children. Alternatively, there is a European tradition that hares laid eggs, since a hare’s scratch or form and a lapwing’s nest look very similar, and both occur on grassland and are first seen in the spring. In the nineteenth century the influence of Easter cards, toys, and books was to make the Easter Hare/Rabbit popular throughout Europe. German immigrants then exported the custom to Britain and America where it evolved into the Easter Bunny."
**Output:**
[
  "The earliest evidence for the Easter Hare was recorded in south-west Germany in 1678 by Georg Franck von Franckenau.",
  "Georg Franck von Franckenau was a professor of medicine.",
  "The evidence for the Easter Hare remained unknown in other parts of Germany until the 18th century.",
  "Richard Sermon was a scholar.",
  "Richard Sermon writes a hypothesis about the possible explanation for the connection between hares and the tradition during Easter.",
  "Hares were frequently seen in gardens in spring.",
  "Hares may have served as a convenient explanation for the origin of the colored eggs hidden in gardens for children.",
  "There is a European tradition that hares laid eggs.",
  "A hare’s scratch or form and a lapwing’s nest look very similar.",
  "Both hares and lapwing’s nests occur on grassland and are first seen in the spring.",
  "In the nineteenth century the influence of Easter cards, toys, and books was to make the Easter Hare/Rabbit popular throughout Europe.",
  "German immigrants exported the custom of the Easter Hare/Rabbit to Britain and America.",
  "The custom of the Easter Hare/Rabbit evolved into the Easter Bunny in Britain and America."
]
---
**Input:**
<an new passage>
**Output:**
```

## 中文提示词

```
将“内容”分解成清晰简洁的命题，确保它们在上下文之外也能理解。


1. 将复合句拆分为简单句。尽可能保持输入的原始措辞。
2. 对于任何伴随有额外描述信息的命名实体，将这些信息分离成独立的命题。
3. 通过为名词或整句添加必要的修饰词，并用完整的实体名称替换代词（例如“它”、“他”、“她”、“他们”、“这个”、“那个”）来使命题脱离上下文。
4. 以JSON格式将结果呈现为字符串列表。

**输入:**

标题: Ìostre. 部分: 理论和解释，与复活节兔子的联系。

内容:
关于复活节兔子（Osterhase）的最早证据是在1678年由医学教授Georg Franck von Franckenau在德国西南部记录的，但在18世纪之前在德国其他地区仍然不为人知。学者Richard Sermon写道：“兔子经常在春天的花园中出现，因此可能是对为孩子们藏在那里的彩蛋起源的一个方便解释。或者，有一种欧洲传统认为兔子下蛋，因为兔子的抓痕或形态和大鸻的巢非常相似，而且两者都出现在草地上，并在春天首次出现。19世纪，复活节卡片、玩具和书籍的影响使复活节兔子在整个欧洲流行起来。然后，德国移民将这种习俗传播到英国和美国，并在英国和美国演变成复活节兔子。”

**输出:**

[
  "关于复活节兔子的最早证据是在1678年由Georg Franck von Franckenau在德国西南部记录的。",
  "Georg Franck von Franckenau是一位医学教授。",
  "在18世纪之前，复活节兔子的证据在德国其他地区仍然不为人知。",
  "Richard Sermon是一位学者。",
  "Richard Sermon写了一个关于兔子与复活节传统之间可能联系的假设。",
  "兔子经常在春天的花园中出现。",
  "兔子可能是对为孩子们藏在花园里的彩蛋起源的一个方便解释。",
  "有一种欧洲传统认为兔子下蛋。",
  "兔子的抓痕或形态和大鸻的巢非常相似。",
  "兔子和大鸻的巢都出现在草地上，并在春天首次出现。",
  "19世纪，复活节卡片、玩具和书籍的影响使复活节兔子在整个欧洲流行起来。",
  "德国移民将复活节兔子的习俗传播到英国和美国。",
  "复活节兔子的习俗在英国和美国演变成复活节兔子。"
]

---
**输入:**
<新段落>
**输出:**
```

---

# 其他优化方法

## Chunking优化

### 主要观点

- 文本切割是优化语言模型应用性能的关键步骤。
- 切割策略应根据数据类型和语言模型任务的性质来定制。
- 传统的基于物理位置的切割方法（如字符级切割和递归字符切割）虽简单，但可能无法有效地组织语义相关的信息。
- 语义切割和基因性切割是更高级的方法，它们通过分析文本内容的语义来提高切割的精确度。
- 使用多向量索引可以提供更丰富的文本表示，从而在检索过程中提供更相关的信息。
- 工具和技术的选择应基于对数据的深入理解以及最终任务的具体需求。

### 五种文本切割的层次

1. 字符级切割：简单粗暴地按字符数量固定切割文本。
2. 递归字符切割：考虑文本的物理结构，如换行符、段落等，逐步递归切割。
3. 文档特定切割：针对不同类型的文档（如 Markdown、Python 代码、JavaScript 代码和 PDF），使用特定的分隔符进行切割。
4. 语义切割：利用语言模型的嵌入向量来分析文本的意义和上下文，以确定切割点。
5. 基因性切割：创建一个 Agent，使用 Agent 来决定如何切割文本，以更智能地组织信息。

## Query重写

![](img/32c5ba99fd6f2ada41c4f316401aaa8e.png)

1. 输入问题
2. 生成子查询：输入问题被分解成多个子查询（Query 1, Query 2, Query n）。
3. 文档检索：每个子查询分别检索相关的文档，得到对应的检索结果（Query 1 documents, Query 2 documents, Query n documents）。
4. 文档合并和重新排序：将所有子查询的检索结果文档合并并重新排序（Combined Reranked Documents）。
5. 输出答案

提示词：

```
你是一名乐于助人的助手，负责生成与输入问题相关的多个子问题。
目标是将输入问题分解为一组可以单独回答的子问题。
生成与以下问题相关的多个搜索查询：{question} 
输出（3个查询）：
```

英文版：

```
You are a helpful assistant that generates multiple sub-questions related to an input question. 
The goal is to break down the input into a set of sub-problems / sub-questions that can be answers in isolation. 
Generate multiple search queries related to: {question} 
Output (3 queries):
```

上面是分解为多个子查询之后，把多个子查询召回的文档重排序后交给大模型来回答，接下来的策略对子查询生成的问题进行迭代得到新的答案。

## 其它 Query 分解的策略

### 递归回答方法

我们将问题以及先前的问答响应和为当前问题获取的上下文继续传递。保留了旧的视角，并将解决方案与新的视角同步，这种方法对于非常复杂的查询是有效的。

![](img/5efd015ac9cc09b6d97fbed3966dad05.png)

### 并行回答方法

将用户提示分解为细致入微的部分，不同之处在于我们试图并行解决它们。单独回答每个问题，然后将它们组合在一起以获得更细致的上下文，然后用于回答用户查询。对大部分场景来说是有效的方案

![](img/98f7fbcffd944ca24d2c403e6732733c.png)

### 其它

#### 分析 Query 结构（分词、NER）

- 分词：将查询分解成单独的词语或词组，便于进一步处理。
- NER（命名实体识别）：识别查询中的命名实体，如人名、地名、组织等，有助于理解查询的具体内容。

#### 纠正 Query 错误（纠错）

对查询中的拼写错误或语法错误进行自动纠正，以提高检索的准确性。

#### 联想 Query 语义（改写）

- 对查询进行语义改写，使其更具表达力和检索效果。
- HyDE：一种基于上下文的联想查询改写方法，利用现有信息生成更有效的查询。
- RAG-Fusion：结合检索和生成的技术，进一步增强查询的表达能力。

#### 扩充 Query 上下文（省略补全、指代消解）

对查询进行上下文扩展，补全省略的内容或解析指代关系，使查询更完整和明确。

![](img/d5c33968f15b673886a6ae7238d82fb2.png)

## Hybrid Retrieval（混合检索）

混合检索(Hybrid Retrieval)是一种结合了稀疏检索(Sparse Retrieval)和稠密检索(Dense Retrieval)的策略，旨在兼顾两种检索方式的优势，提高检索的效果和效率。

![](img/9dbb50b672ebee580edec593d79d1988.png)

1. 稀疏检索(Sparse Retrieval)：这种方法通常基于倒排索引(Inverted Index)，对文本进行词袋(Bag-of-Words)或TF-IDF表示，然后按照关键词的重要性对文档进行排序。稀疏检索的优点是速度快，可解释性强，但在处理同义词、词语歧义等语义问题时效果有限。

2. 稠密检索(Dense Retrieval)：这种方法利用深度神经网络，将查询和文档映射到一个低维的稠密向量空间，然后通过向量相似度(如点积、余弦相似度)来度量查询与文档的相关性。稠密检索能更好地捕捉语义信息，但构建向量索引的成本较高，检索速度也相对较慢。
