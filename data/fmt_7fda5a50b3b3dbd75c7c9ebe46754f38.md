
# 神经网络算法 - 一文搞懂BERT（基于Transformer的双向编码器）

> 来源：https://geekbang.feishu.cn/docx/FzlPdUAeWogWJ9xN2LacWQhXnCf
>
> 👨🏻 作者：AllenTang
>
> 📃 作者介绍：分享人工智能，让所有人玩转AI
>
> 🛖 👸号：架构师带你玩转AI
>
> ⏰ 收录时间：2024-05-23 11:28:42
>
> 🔗 原文链接：https://mp.weixin.qq.com/s?__biz=MzkzMTEzMzI5Ng==&mid=2247485865&idx=1&sn=140f4953bc9c137127f546121785db76&chksm=c26eebb6f51962a0d76fa518ed367f88ca84bba0395a867d07a252e9709a4e864fe32863f8d6&scene=21#wechat_redirect

![](img/a520095a7b7666dc5df6320da264d099.png)

关注作者👸号，获取更多AI知识

本文将从 `BERT`的本质、`BERT`的原理、`BERT`的应用 三个方面，带您一文搞懂 `Bidirectional Encoder Representations from Transformers | BERT`。

![](img/9180924499d253569abdf39083319388.png)

Google `BERT`

## 一、`BERT` 的本质

`BERT`架构：一种基于多层 [`Transformer`](http://mp.weixin.qq.com/s?__biz=MzkzMTEzMzI5Ng==&mid=2247485629&idx=1&sn=71fdf59802e2025d9322548270c545c4&chksm=c26eeaa2f51963b49ecbd01ccf11e6fab7b7558338edc84beb1cb3193053f4cf6a089df1ecd6&scene=21#wechat_redirect) 编码器的预训练语言模型，通过结合 `Tokenization`、多种 `Embeddings` 和特定任务的输出层，能够捕捉文本的双向上下文信息，并在各种自然语言处理任务中表现出色。

![](img/8a99f7e73859e4830979057eede22b25.png)

`BERT`架构

### 输入层 (Input)

`BERT`的输入是一个原始的文本序列，它可以是单个句子，也可以是两个句子（例如，问答任务中的问题和答案）。在输入到模型之前，这些文本需要经过特定的预处理步骤。

### `Tokenization` 和 `Embeddings`

`Tokenization`：输入文本首先通过分词器（`Tokenizer`）被分割成 `Token`。这一步通常包括将文本转换为小写、去除标点符号、分词等。`BERT` 使用 `WordPiece` 分词方法，将单词进一步拆分成子词（subwords），以优化词汇表的大小和模型的泛化能力。

`Token Embeddings`：分词后的 `Token` 被映射到一个高维空间，形成 `Token Embeddings`。这是通过查找一个预训练的嵌入矩阵来实现的，该矩阵为每个 `Token` 提供一个固定大小的向量表示。

`Segment Embeddings`：由于 `BERT` 能够处理两个句子作为输入（例如，在句子对分类任务中），因此需要一种方法来区分两个句子。`Segment Embeddings` 用于此目的，为每个 `Token` 添加一个额外的嵌入，以指示它属于哪个句子（通常是 `"A"` 或 `"B"`）。

`Position Embeddings`：由于 `Transformer` 模型本身不具有处理序列中 `Token` 位置信息的能力，因此需要位置嵌入来提供这一信息。每个位置都有一个独特的嵌入向量，这些向量在训练过程中学习得到。

`Token Embeddings`、`Segment Embeddings` 和 `Position Embeddings` 三者相加，得到每个 `Token` 的最终输入嵌入。

### `BERT` 的网络结构 (Network Structure of BERT)

`BERT` 的核心是由多个 [`Transformer`](http://mp.weixin.qq.com/s?__biz=MzkzMTEzMzI5Ng==&mid=2247485629&idx=1&sn=71fdf59802e2025d9322548270c545c4&chksm=c26eeaa2f51963b49ecbd01ccf11e6fab7b7558338edc84beb1cb3193053f4cf6a089df1ecd6&scene=21#wechat_redirect) 编码器层堆叠而成的。每个编码器层都包含自注意力机制和前馈神经网络，允许模型捕捉输入序列中的复杂依赖关系。

自注意力机制：允许模型在处理序列时关注不同位置的 `Token`，并计算 `Token` 之间的注意力权重，从而捕捉输入序列中的依赖关系。

前馈神经网络：对自注意力机制的输出进行进一步转换，以提取更高级别的特征。

残差连接和层归一化：用于提高模型的训练稳定性和效果，有助于缓解梯度消失和梯度爆炸问题。

### 输出层 (Output)

`BERT` 的输出取决于特定的任务。在预训练阶段，`BERT` 采用了两种任务：`Masked Language Model`（`MLM`）和 `Next Sentence Prediction`（`NSP`）。

`MLM`：在这种任务中，`BERT` 预测输入序列中被随机遮盖的 `Token`。模型的输出是每个被遮盖 `Token` 的概率分布，通过 `Softmax` 层得到。

`NSP`：这种任务要求 `BERT` 预测两个句子是否是连续的。模型的输出是一个二分类问题的概率分布。

## `BERT` 模型的输入

通过结合 `Token Embeddings`、`Segment Embeddings` 和 `Position Embeddings` 三种嵌入方式，`BERT` 等模型能够全面捕获文本的语义和上下文信息，为各类自然语言处理任务提供强大的基础表示能力。

### `Token Embeddings`

在 `BERT` 中，输入文本首先被分割成 `Token` 序列（单词、子词等），每个 `Token` 都会被映射到一个高维向量空间，形成 `Token Embeddings`。这些嵌入捕获了 `Token` 的语义信息，是模型理解文本的基础。

`Token Embeddings` 是通过在大规模语料库上进行无监督预训练得到的，这使得 `BERT` 能够理解和处理各种复杂的语言现象和语义关系。

![](img/e0b1e98d92b6731371129e5b158b5dd5.png)

`Token Embeddings`

### `Segment Embeddings`

为了处理像问答这样的任务，`BERT` 引入了 `Segment Embeddings` 来区分两个不同但相关的句子（例如问题和答案）。这些嵌入帮助模型理解句子间的关系和边界。

在文本分类任务中，`Segment Embeddings` 的作用可能不那么明显，因为输入通常是一个连续的文本段落。然而，它们仍然可以用于区分不同部分的文本，特别是在处理长文档或多个句子时。

![](img/64b65cda86d8addb0cf7c096577f095a.png)

`Segment Embeddings`

### `Position Embeddings`

由于 `Transformer` 结构本身不具备处理序列顺序的能力，`BERT` 引入了 `Position Embeddings` 来捕获文本中 `Token` 的位置信息。

这些嵌入确保模型能够区分不同位置的相同 `Token`，例如区分 `"hello，world"` 和 `"world，hello"` 中的 `"hello"` 和 `"world"`。

![](img/c4b79026201f1f5d77c6ddc1341c3267.png)

`Position Embeddings`

`Position Embeddings` 与 `Token Embeddings` 和 `Segment Embeddings` 相加，形成最终的输入嵌入，这些嵌入随后被送入 `Transformer` 编码器进行处理。

## 二、`BERT` 的原理

`BERT` 的工作原理是通过在大规模未标注数据上执行预训练任务（如 `Masked Language Model` 来捕获文本中词汇的双向上下文关系，以及 `Next Sentence Prediction` 来理解句子间的逻辑关系），再将预训练的模型针对特定任务进行 `Fine-tuning`，从而在各种自然语言处理任务中实现高性能。

![](img/f591faefb8b251dd8101edacc68f6c9e.png)

`BERT` 工作原理

模型的预训练任务：在大量未标注数据上进行自我学习的过程，通过这些任务，模型能够学习到语言的内在规律和模式，从而为其在后续的具体任务（如文本分类、问答等）中提供有力的支持。以下是两个核心的预训练任务：

![](img/8f2e10c28c69004b079e3f3f6e976658.png)

无监督预训练

### `Masked Language Model` (`MLM`)

任务描述：在输入的文本中，随机地遮盖或替换一部分词汇，并要求模型预测这些被遮盖或替换的词汇的原始内容。

目的：使模型能够利用双向的上下文信息来预测被遮盖的词汇，从而学习到更深层次的语义表示。

实现方式：在预训练阶段，`BERT` 随机选择文本中 15% 的 `Token` 进行遮盖，其中 80% 的时间用 `[MASK]` 标记替换，10% 的时间用随机词汇替换，剩下的 10% 保持不变。这种遮盖策略被称为动态遮盖，因为它在每次输入时都会随机改变遮盖的位置和词汇。

![](img/13500f47f6d58cfbafdcaf58caf8b1a3.png)

`Masked Language Model` (`MLM`)

### `Next Sentence Prediction` (`NSP`)

任务描述：给定一对句子，判断第二个句子是否是第一个句子的后续句子。

目的：使模型能够理解句子间的逻辑关系，如连贯性、因果关系等，从而提高其在处理长文档或复杂文本时的能力。

实现方式：在预训练阶段，`BERT` 构造了一个二分类任务，其中 50% 的时间 `B` 是 `A` 的真正后续句子（标签为 `"IsNext"`），另外 50% 的时间 `B` 是从语料库中随机选择的句子（标签为 `"NotNext"`）。模型通过最后一层 `Transformer` 输出的 `[CLS]` 标记的嵌入来进行预测。

![](img/bae792c4c1e1c0de57ae02596f952798.png)

`Next Sentence Prediction` (`NSP`)

模型 `Fine-tuning`：`BERT` 的 `fine-tuning` 过程是针对特定任务对预训练模型进行调整的过程，使其能够更好地适应和解决具体任务。根据任务类型的不同，对 `BERT` 模型的修改也会有所不同，但通常这些修改都相对简单，往往只需要在模型的输出部分加上一层或多层神经网络。

![](img/94c52e200fe58b575f7775e4d565c3ec.png)

模型 `Fine-tuning`

### 任务类型

根据具体任务的不同，`BERT` 的 `fine-tuning` 可以分为以下几种类型：

- 句子对分类任务（`Sentence Pair Classification`）：这种任务需要判断两个句子之间的关系，如文本蕴含、问答匹配等。在 `fine-tuning` 时，将两个句子一起输入模型，并取第一个 `token`（`[CLS]`）的输出表示作为整个句子对的表示，然后将其输入到一个额外的 `softmax` 层进行分类。
- 单句分类任务（`Single Sentence Classification`）：这种任务需要对单个句子进行分类，如情感分析、文本分类等。在 `fine-tuning` 时，将单个句子输入模型，并同样取第一个 `token`（`[CLS]`）的输出表示进行分类。
- 问答任务（`Question Answering`）：这种任务需要模型从给定的文本中找出问题的答案。在 `fine-tuning` 时，将问题和答案一起输入模型，并取答案部分在模型输出中的起始和结束位置作为答案的预测。
- 序列标注任务（`Sequence Tagging`，如命名实体识别 `NER`）：这种任务需要对输入序列中的每个 `token` 进行分类，如识别文本中的实体、词性标注等。在 `fine-tuning` 时，取所有 `token` 在最后一层 `Transformer` 的输出，然后将其输入到一个额外的 `softmax` 层进行逐 `token` 的分类。

### 模型修改

在进行 `fine-tuning` 时，对 `BERT` 模型的修改通常包括以下几个方面：

- 输入处理：根据任务类型的不同，对输入数据进行相应的处理，如将句子对拼接在一起、添加特殊标记等。
- 输出层：在 `BERT` 模型的输出部分添加一层或多层神经网络，用于将模型的输出转换为任务所需的格式。对于分类任务，通常添加一个 `softmax` 层进行概率分布的计算；对于序列标注任务，则逐 `token` 进行分类。
- 损失函数：根据任务类型选择合适的损失函数，如交叉熵损失函数用于分类任务、平方差损失函数用于回归任务等。

## 三、`BERT` 的应用

### `Question Answer` (`QA`，问答系统)

`BERT` 在问答系统（`QA`）中的应用通常涉及两个阶段：检索阶段和问答判断阶段。

#### 一、检索阶段

文档处理：

- 切割：将长文档切割成较短的段落或句子（`Passage`），这些片段更容易处理和索引。
- 建立索引：利用倒排索引技术，为每个切割后的片段（`Passage`）建立索引，以便快速查询。

检索模型：

- `BM25` 模型：使用 `BM25` 或类似的检索函数（如 `BM25+RM3`）计算问句与每个候选段落或句子的相关性得分。
- 候选选择：根据得分选择 Top K 个最相关的候选段落或句子。

#### 二、问答判断阶段

模型准备：

- `Fine-tuning` 数据：选择适当的问答数据集（如 `SQuAD`）或任务数据进行 `BERT` 模型的 `fine-tuning`。
- 模型结构：在 `BERT` 模型的基础上，添加必要的输出层以适应问答任务，如分类层或起始/终止位置预测层。

问答处理：

- 输入构建：将用户问句和每个候选段落或句子组合成 `BERT` 模型的输入格式。
- 模型预测：使用 `fine-tuned` `BERT` 模型对每个输入进行预测，判断候选段落或句子是否包含正确答案，或者预测答案的精确位置。
- 答案选择：
  - 评分机制：根据 `BERT` 模型的预测结果，为每个候选段落或句子分配得分。
  - 最终答案：选择得分最高的候选段落或句子作为最终答案。

### 聊天机器人

`BERT` 在聊天机器人中的应用主要涉及两个方面：用户意图分类和槽位填充（对于单轮对话），以及多轮对话中的上下文信息利用。

![](img/1f98d108421fbc64951617add10b7984.png)

#### 一、`BERT` 在单轮对话中的应用

##### 用户意图分类

- 输入：将用户的话语作为 `BERT` 模型的输入。
- 模型结构：在 `BERT` 模型的基础上添加分类层，用于将用户意图分类到不同的服务类型中。
- 训练：使用带有意图标签的用户话语数据集进行 `fine-tuning`，使模型能够准确识别用户意图。
- 任务描述：从用户的话语中解析出用户的意图，如订餐、点歌等。

##### 槽位填充

- 输入：将用户的话语以及预定义的槽位作为 `BERT` 模型的输入。
- 模型结构：采用序列标注的方式，对每个输入 `token` 进行槽位标签的预测。
- 训练：使用带有槽位标签的用户话语数据集进行 `fine-tuning`，使模型能够准确填充槽位信息。
- 任务描述：根据用户意图抽取关键元素，如订机票时的出发地、目的地等。

#### 二、`BERT` 在多轮对话中的应用

##### 上下文信息利用

- 输入：将当前用户话语以及历史对话内容作为 `BERT` 模型的输入。
