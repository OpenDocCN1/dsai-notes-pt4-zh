
# 专为五大科学领域定制，NASA 与 IBM 合作开大语言模型 INDUS

> 来源：[https://agijuejin.feishu.cn/docx/QUGCdTeFboiBNdxrFsWcD9YhnGd](https://agijuejin.feishu.cn/docx/QUGCdTeFboiBNdxrFsWcD9YhnGd)

> 机器之心 ScienceAI｜[阅读原文](https://www.jiqizhixin.com/articles/2024-06-27-3)

> 转载请联系原作者取得授权

![](img/d79b1d8fff1b057246898a39a9b1dd3f.png)

INDUS 以南天星座命名，是一套全面的大型语言模型，支持五个科学领域。（来源：NASA）

编辑 | KX

在大量数据上训练的大型语言模型 (LLM) 在自然语言理解和生成任务上表现出色。大多数流行的 LLM 使用 Wikipedia 等通用语料库进行训练，但词汇的分布变化导致特定领域的性能不佳。

受此启发，NASA 与 IBM 合作开发了 INDUS，这是一套全面的 LLM，专为地球科学、生物学、物理学、太阳物理学、行星科学和天体物理学领域量身定制，并使用从不同数据源的精选科学语料库进行训练。

INDUS 包含两类模型：编码器和句子 `Transformer`。编码器将自然语言文本转换为 LLM 可以处理的数字编码。INDUS 编码器在包含天体物理学、行星科学、地球科学、太阳物理学、生物和物理科学数据的 600 亿个 tokens 的语料库上进行训练。

相关研究以「 INDUS: Effective and Efficient Language Models for Scientific Applications 」为题，发布在 arXiv 预印平台。

论文链接：https://arxiv.org/abs/2405.10725

在通用领域语料库上训练的 LLM 在自然语言处理 (NLP) 任务上表现出色。然而，先前的研究表明，使用特定领域语料库训练的 LLM 在专门任务上表现更好。

比如，有研究者已经开发了几个特定领域的 LLM，例如 `SCIBERT`、`BIOBERT`、`MATBERT`、`BATTERYBERT` 和 `SCHOLARBERT`，目的是提高领域内 NLP 任务的准确性。

## INDUS：一套全面的 LLM

在该研究中，研究人员特别关注与地球、天体、太阳和太阳系内的行星相关的跨学科领域，例如物理学、地球科学、天体物理学、太阳物理学、行星科学和生物学。

![](img/eebf1bd630f37efd5f3375a9fb98d836.png)

图示：INDUS 模型概览。（来源：论文）

INDUS 是一组基于编码器的 LLM，专注于这些感兴趣的领域，用不同来源的精心策划的语料库进行训练。INDUS 中包含的 50,000 个词汇中有超过一半是用于训练的特定科学领域所独有的。INDUS 编码器模型对大约 2.68 亿个文本对（包括标题/摘要和问题/答案）上的句子 `Transformer` 模型进行微调。

具体而言：

1. 利用字节对编码算法，从精选的科学语料库中构建了定制的标记器 `INDUSBPE`。

2. 使用精选的科学语料库和 `INDUSBPE` 标记器预训练了多个仅编码器的 LLM。进一步通过使用对比学习目标对仅编码器模型进行微调来创建句子嵌入模型，以学习「通用」句子嵌入。使用知识提炼技术训练了这些模型的更小、更高效的版本。

3. 创建了三个新的科学基准数据集，`CLIMATE-CHANGE NER`（实体识别任务）、`NASA-QA`（提取问答任务）和 `NASA-IR`（检索任务），以进一步加速这一多学科领域的研究。

4. 通过实验结果，展示了模型在这些基准任务以及现有领域特定基准上的出色表现，超越了 `RoBERTa` 等通用模型以及 `SCIBERT` 等科学领域编码器。

## 比非领域特定 LLM 表现更好

通过为 INDUS 提供领域特定词汇，研究团队在生物医学任务基准、科学问答基准和地球科学实体识别测试中，比开放的、非领域特定 LLM 表现更好。

将 INDUS 模型与类似大小的开源模型 `RoBERTaBASE`、`SCIBERT`、`MINILM` 和 `TINYBERT` 进行了比较。

在自然语言理解任务上，在基础模型中，`INDUSBASE` 在微观/宏观平均值上明显优于通用 `RoBERTa` 模型，同时在生物领域特定的对应模型 `SCIBERT` 中也取得了竞争性的表现。

表：`BLURB` 的评估结果。（来源：论文）

![](img/c1cfa20e4f95f0cc7e8b83492036d25a.png)

`BLURB` 在气候变化 `NER` 任务上明显优于相应的基线模型，表明了对大型特定领域数据进行训练的有效性。

表：气候变化 `NER` 基准结果。（来源：论文）

![](img/7299ade9be5752451ee9e053c9df8edb.png)

在 `NASA-QA`（提取问答任务）中，使用相关的 `SQuAD` 对扩充训练集进行微调。所有模型都经过 15 epochs 的微调，结果观察到 `INDUSBASE` 的表现优于所有类似规模的模型，而 `INDUSSMALL` 的表现相对较强。

表：`NASA-QA` 基准结果。（来源：论文）

![](img/4e0fba79e403e5177dac1c5d33f4e0e2.png)

在检索任务中，在 `NASA-IR` 数据集和 `BEIR` 基准上评估了 INDUS 模型，该基准由 12 个涵盖各种领域的检索任务组成。

如下表所示，两个句子嵌入模型在 `NASA-IR` 任务上的表现都明显优于基线，同时在几个 `BEIR` 任务上仍保持良好的性能。

表：`NASA-IR` 和 `BEIR` 的评估结果。（来源：论文）

![](img/5cc1b9d9d67a7369765f9e8511f825d5.png)

研究人员还在单个 A100 GPU 上测量了 BEIR 自然问题集的 4,202 个测试查询中每个查询的平均检索时间。这个时间包括编码查询、语料库的时间以及检索相关文档的时间。值得注意的是，`INDUS-RETRIEVERSMALL` 在 `NASA-IR` 和 `BEIR` 上的表现都优于 `INDUS-RETRIEVERBASE`，同时速度快了约 4.6 倍。

IBM 研究员 Bishwaranjan Bhattacharjee 对整体方法进行了评论：「我们不仅拥有自定义词汇表，还拥有用于训练编码器模型的大型专业语料库和良好的训练策略，从而实现了卓越的性能。对于较小、较快的版本，我们使用神经架构搜索来获得模型架构，并使用知识提炼来对其进行训练，同时监督较大的模型。」

NASA 生物和物理科学 (BPS) 部门 Sylvain Costes 博士讨论了整合 INDUS 的好处：「将 INDUS 与开放科学数据存储库 (OSDR) 应用程序编程接口 (API) 集成使我们能够开发和试用 聊天机器人，为浏览单个数据集提供更直观的搜索功能。我们目前正在探索改进 OSDR 内部策展数据系统的方法，利用 INDUS 来提高策展团队的工作效率并减少每天所需的手动工作量。」

> 参考内容：https://techxplore.com/news/2024-06-nasa-ibm-collaboration-indus-large.html
