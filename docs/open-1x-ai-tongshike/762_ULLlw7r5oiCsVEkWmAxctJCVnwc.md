
# RAG系统召回率低？BGE-M3 多语言多功能重塑文本检索

> 来源：[https://datawhaler.feishu.cn/docx/IT75d1j5XoFaUGxCPm5cPn5MnJd](https://datawhaler.feishu.cn/docx/IT75d1j5XoFaUGxCPm5cPn5MnJd)

## 🎯 文章目标

本文专为 `RAG` 爱好者打造，旨在帮助你：

+ 深入掌握 `BGE-M3` 的多功能性、多语言性和多粒度性，理解其如何重塑文本检索。

+ 通过实验与代码，探索 `BGE-M3` 在密集、稀疏和多向量检索中的性能与实现。

+ 结合理论与实践，发掘 `BGE-M3` 在多语言和长文档场景的应用潜力。

> 💡 小提示
>
> 本文是 `RAG` 进阶系列，包含丰富代码与实验结果。建议边读边动手运行，亲测 `BGE-M3` 的强大能力，效果更佳！

## 📄 主题

**核心主题**：从密集向量到混合检索：了解下 `BGE-M3` 多语言多功能重塑文本检索

**📚 通过本文你将收获**

+ 系统性理解 `BGE-M3` 及其家族的演进脉络。

+ 掌握 `FlagEmbedding`、`SentenceTransformer` 等工具调用 `BGE-M3` 的实战技巧。

+ 获得密集、稀疏、多向量及混合检索的实验经验与技术洞见。

## 🚁 前言

在大家做基于 `RAG` 问答系统的时候，嵌入模型的性能直接决定了系统的效率和准确性，当你的检索管道检索的命中率低导致问答系统效果不好怎么办？今天我们要深入探讨 `BGE-M3`，它一款由北京智源人工智能研究院（BAAI）开发的文本嵌入模型。它支持密集、稀疏和多向量检索，覆盖 100+ 种语言，最长处理 8192 个 token。如果你的检索管道只有稠密向量检索或者稀疏向量检索的同学，我强烈建议看看这篇文章。

---

✨ 你好，我是筱可，欢迎来到「筱可 AI 研习社」！

🚀 **标签关键词：**| AI 实战派开发者 | 技术成长陪伴者 | `RAG` 前沿探索者 | 文档处理先锋 |

## 一、`BGE-M3` 模型简介

`BGE-M3`（Beijing General Embedding M3，由北京智源人工智能研究院 BAAI 开发，2024 年发布）是一个多功能的嵌入模型，以其 多语言性（Multi-Linguality）、多功能性（Multi-Functionality）、多粒度性（Multi-Granularity） 而著称。它基于 `XLM-RoBERTa` 架构进行了优化，训练数据规模高达 2.5TB，覆盖 100 多种语言，支持从短句到长达 8192 个 token 的文档处理。相比 `RoBERTa` 和 `XLM-RoBERTa`，`BGE-M3` 的独特之处在于它不仅保留了 `MLM`（掩码语言模型）任务，还引入了多种检索功能：

+ **多功能性**：`BGE-M3` 能同时支持三种检索方式：

1. 稠密检索（Dense Retrieval）：生成固定维度的密集向量，用于语义相似性匹配。

1. 稀疏检索（Sparse Retrieval）：生成高维稀疏向量，类似于 `BM25`，强调词的精确匹配。

1. 多向量检索（Multi-Vector Retrieval）：用多个向量表示文本（如 `ColBERT`），捕捉更细粒度的上下文信息。

这种多功能设计让它可以灵活应对不同场景，比如结合稠密和稀疏检索的混合检索（Hybrid Retrieval），提升准确性和泛化能力。

+ **多语言性**：它在 100 多种语言上表现优异，支持多语言和跨语言检索任务，无需额外语言标识即可自动识别并处理多种语言文本。

+ **多粒度性**：从短句子到超长文档（最大 8192 token），`BGE-M3` 都能有效嵌入，特别适合需要处理长文本的场景。

`BGE-M3` 在训练中采用了创新的 自知识蒸馏（Self-Knowledge Distillation） 方法，以提升嵌入质量。简单来说，自知识蒸馏是一种特殊的知识蒸馏技术，通常知识蒸馏是指用一个强大的“教师模型”指导一个较小的“学生模型”学习，而自知识蒸馏则是让模型自己充当“教师”和“学生”。具体到 `BGE-M3`，它将自身在不同检索任务（稠密、稀疏、多向量检索）中的输出得分进行融合，生成一个综合的“教师信号”，然后用这个信号来指导模型自身的训练。这种方法不需要额外的外部模型，而是利用模型自身的多功能性挖掘内在知识，从而提高嵌入的语义表达能力和一致性。

+ **工作原理**：在训练中，`BGE-M3` 会同时优化多种检索目标（比如稠密向量的语义匹配和稀疏向量的词级匹配）。通过融合这些目标的预测结果，模型得到一个更全面的“伪标签”或“软目标”，然后用这个目标反过来约束自己的学习过程。这种自监督的方式让模型能够更好地平衡不同任务的需求。

+ **优势**：相比传统训练，自知识蒸馏使 `BGE-M3` 在多语言和多任务场景下表现更稳定，尤其是在处理复杂语义和长文本时。它避免了外部教师模型可能带来的偏差，同时充分利用了大规模数据的潜力。

目前，开源且支持中文的嵌入模型中，`BGE` 系列和 `Jina` 系列是最主流的选择。那为什么我们这篇文章主讲 `BGE` 呢？一方面，它的多功能性让人印象深刻，尤其是多向量检索（`ColBERT`）这种方式，我个人觉得非常出色。像 `RAGFlow` 这样的工具也在主推这类模型，可见它的潜力。

具体有哪些特点？别急，后面的介绍会让你一探究竟。我们先从 `BGE` 家族的成员说起，包括初代、二代（v1.5）和三代（`BGE-M3`）。另外，他们还有一些基于大语言模型的变体，比如基于 `Gemma2-9B` 和 `Mistral-7B` 构建的嵌入模型。不过这些模型参数量太大（动辄几十亿），这次我们只简单提一下，不会展开太多。你准备好了解 `BGE` 的进化历程了吗？

## 二、`BGE` 系列模型解析

> 从这部分的介绍，大家可以发现 `BGE` 系列第一、二、三代模型基本都是从 `bert` 系列发展出来的。

### 2.1 `BGE` 家族概览

#### 2.1.1 初代 `BGE`

初代针对英语和中文提供 6 个模型，基于 `BERT`：

| 模型 | 语言 | 参数数量 | 模型大小 | 描述 | 基础模型 |

| --- | --- | --- | --- | --- | --- |

| `BAAI/bge-large-en` | 英语 | 500M | 1.34 GB | 高性能嵌入模型 | `BERT` |

| `BAAI/bge-small-zh` | 中文 | 24M | 95.8 MB | 小型但竞争力强 | `BERT` |

#### 2.1.2 `BGE` v1.5

`BGE` v1.5 通过优化相似度分布和提升无指令检索能力，改进了前代模型的不足。

| 模型 | 语言 | 参数数量 | 模型大小 | 描述 | 基础模型 |

| --- | --- | --- | --- | --- | --- |

| `BAAI/bge-large-en-v1.5` | 英语 | 335M | 1.34 GB | 相似度分布更合理 | `BERT` |

#### `BGE` v1.5 的优化背景

`BGE` v1（2023 年 8 月发布）在 `MTEB` 和 `C-MTEB` 等基准测试中表现优异，但存在两方面问题：

1. **相似度分布不合理**：采用对比学习（Contrastive Learning）时，温度参数（temperature）设为 0.01，导致相似度分数集中于 [0.6, 1] 区间。即使语义不相关的句子，分数也可能超过 0.5，难以直观区分相关性。

1. **指令依赖性强**：在短查询匹配长文档的检索任务中，`BGE` v1 需为查询添加特定指令（如“Represent this sentence for searching relevant passages:”），否则性能下降明显。

为解决上述问题，`BGE` v1.5（2023 年 9 月发布）进行了针对性优化。

#### 优化相似度分布的实现

`BGE` v1.5 通过改进训练策略和损失函数，使相似度分布更合理：

+ **改进对比学习**：沿用对比学习框架，但调整了温度参数或优化损失函数（如 `InfoNCE` 变体），使分数分布更均匀。`BGE` v1 的分数集中源于温度过低，v1.5 通过平衡正样本（语义相似的句子对）和负样本（不相关的句子对），让分数更准确反映语义差异。

+ 硬负样本挖掘（Hard Negative Mining）：引入高质量硬负样本（如表面相似但语义不同的句子，例如查询“什么是 AI”对应的硬负样本“什么是空调”），增强模型对细微语义差异的区分能力，避免无关句子的分数过高。

+ **效果**：优化后，相似度分布更自然，用户可根据需求设置阈值（如 0.8 或 0.9）筛选相似句子，不再受限于 v1 的 [0.6, 1] 集中区间。

#### 提升无指令检索能力的实现

`BGE` v1.5 增强上下文理解，减少对指令的依赖：

+ **预训练优化**：基于 `RetroMAE` 等方法，在更大规模、多样化的语料上预训练。通过双向掩码预测任务，模型无需指令也能捕捉查询的检索意图。

+ **指令无关微调**：微调时减少指令比例，混合有指令和无指令数据进行对比学习，使模型自适应两种场景，提升无指令下的性能。

+ **短查询处理改进**：针对短查询匹配长文档场景，优化嵌入生成（如调整注意力机制或池化策略，例如 `CLS` 池化或平均池化），确保短文本语义表示更鲁棒，无需指令也能提取充分信息。

+ **效果**：无指令场景下的检索性能接近有指令场景，下降幅度极小，用户可直接使用原始查询生成嵌入，简化操作流程。

#### 与 `BGE-M3` 的关联

`BGE-M3`（2024 年发布）在 v1.5 基础上进一步发展，消除指令依赖，并通过支持稠密、稀疏和多向量检索提升性能。`BGE` v1.5 的改进为其后续版本奠定了关键基础。

#### 2.1.3 `BGE-M3`

`BGE-M3` 引入多功能性，基于 `XLM-RoBERTa`，[参数](https://arxiv.org/pdf/2402.03216) 568M，大小 2.27 GB，见论文。

| 模型 | 语言 | 参数数量 | 模型大小 | 描述 | 基础模型 |

| --- | --- | --- | --- | --- | --- |

| [`BAAI/bge-m3`](https://huggingface.co/BAAI/bge-m3) | 多语言 | 568M | 2.27 GB | 多功能性(密集检索、稀疏检索、多向量(`colbert`))、多语言性和多粒度(8192标记) | `XLM-RoBERTa` |

#### 2.1.4 `BGE` Multilingual `Gemma2`

`BGE` Multilingual `Gemma2` 是一个基于 `LLM` 的多语言嵌入模型。

| 模型 | 语言 | 参数数量 | 模型大小 | 描述 | 基础模型 |

| --- | --- | --- | --- | --- | --- |

| `BAAI/bge-multilingual-gemma2` | 多语言 | 9.24B | 37 GB | 基于 `LLM` 的多语言嵌入模型，在多语言基准测试中取得了 `SOTA` 结果 | `Gemma2-9B` |

#### 2.1.4 `BGE ICL`

`BGE ICL` 代表上下文学习（in-context learning）。通过在查询中提供少样本示例，它可以显著增强模型处理新任务的能力。

| 模型 | 语言 | 参数数量 | 模型大小 | 描述 | 基础模型 |

| --- | --- | --- | --- | --- | --- |

| [`BAAI/bge-en-icl`](https://huggingface.co/BAAI/bge-en-icl) | 英语 | 7.11B | 28.5 GB | 基于 `LLM` 的英语嵌入模型，具有出色的上下文学习能力 | `Mistral-7B` |

### 2.2 `RoBERTa`

我们在上一篇文章中介绍过 `BERT` 模型。由于 `BGE-M3` 是基于 `XLM-RoBERTa` 模型开发的，而它基于 `RoBERTa`，因此本文也会简要介绍 `RoBERTa` 相较于 `BERT` 的主要区别。

`RoBERTa`（Robustly optimized BERT approach，2019 年发布）是在 `BERT` 基础上优化而来，去除了 `NSP`（Next Sentence Prediction，下文预测任务），仅保留 `MLM`（Masked Language Model，掩码语言模型），并引入动态掩码机制。它使用 160GB 大规模数据进行训练，参数规模从 1.25 亿增至 3.55 亿，在 `GLUE` 等基准测试中性能超越 `BERT`。

+ **下文预测任务（`NSP`，Next Sentence Prediction）**：这是 `BERT` 预训练中的一项任务，旨在判断两段文本是否在原文中是连续的。具体来说，`BERT` 会随机抽取两句话，50% 的情况下这两句话是连续的，50% 的情况下则不是，模型需要预测它们之间的关系。这一任务的目的是增强模型对句子间逻辑关系的理解，但 `RoBERTa` 研究发现其效果有限，因此将其移除。

+ 动态掩码机制（Dynamic Masking）：在 `BERT` 中，掩码（即将部分词替换为 `[MASK]` 以供模型预测）是在数据预处理阶段静态生成的，每次训练时掩码位置固定。而 `RoBERTa` 改为在每次训练时动态生成掩码，即每次输入模型时随机选择不同的词进行掩盖。这样可以增加训练的多样性，让模型更鲁棒地学习词语之间的上下文关系。

### 2.3 `XLM-RoBERTa`

`XLM-RoBERTa`（Cross-lingual Language Model - `RoBERTa`，2019 年发布）是在 `RoBERTa` 基础上进一步优化而来的多语言模型。它继承了 `RoBERTa` 的改进特性（如去除 `NSP`，仅保留 `MLM`，采用动态掩码机制），并通过使用 2.5TB 的多语言数据（涵盖 100 种语言，来自过滤后的 CommonCrawl 数据）进行训练，使其在跨语言任务中表现出色。相比 `RoBERTa`，`XLM-RoBERTa` 的参数规模也有所增加，提供 `base`（2.7 亿参数）和 `large`（5.5 亿参数）两个版本。它在跨语言基准测试（如 `XNLI`）中显著超越了多语言 `BERT`（`mBERT`），展现了强大的语言迁移能力。

## 三、`BGE-M3` 实验与应用

`FlagEmbedding` 是由北京智源人工智能研究院（BAAI）开发的一个开源项目，专注于语义嵌入模型的研究与应用。你可能已经听说过，它的目标是为检索增强生成（`RAG`）、语义搜索等场景提供强大的技术支持。而 `BGE-M3` 正是其中的一个模型，接下来我们就通过实验看看它的表现如何。

### 3.1 实验环境

要开始实验，第一步自然是搭建环境。你有没有想过直接用云服务来跑模型？像硅基流动这样的服务确实方便，但目前它们提供的向量模型还不支持生成稀疏向量和多向量。为了完整展示 `BGE-M3` 的能力，我们选择在本地加载模型。

我的本地模型路径如下，如果你要做这个实验，路径需要换成你自己的模型路径，从 huggingface 上下载，大概是 10G 大小的存储空间。

```
model_path = r"C:\Users\k\Desktop\BaiduSyncdisk\baidu_sync_documents\hf_models\bge-m3";
```

实验依赖 `FlagEmbedding` 库，这是官方提供的工具，支持 `BGE-M3` 全系列及其他嵌入模型的调用。安装很简单：

```
%pip install -U FlagEmbedding
```

如果你还想尝试云服务（比如硅基流动的 API），可以配置相关环境变量。不过这次我们主要用本地模型，这部分是可选的：

```
import os, numpy as np, dotenv
dotenv.load_dotenv()
os.environ["OPENAI_API_KEY"] = os.getenv("API_KEY")
os.environ["OPENAI_BASE_URL"] = os.getenv("BASE_URL")
```

环境就绪后，我们就可以进入正题了。准备好探索 `BGE-M3` 的能力了吗？

### 3.2 密集检索实验

下面是分别使用 `FlagEmbedding`、`SentenceTransformer`、`OpenAI API` 进行调用示例。

其中 `SentenceTransformer` 是一个强大且易用的自然语言处理库，通过预训练模型能将各种语言的文本句子高效地转换为低维向量，使语义相似的句子在向量空间中距离相近，具有多种预训练模型可供选择，适用于文本分类、信息检索、语义相似度计算、文本聚类等多种自然语言处理场景。

测试句子：

```
sentences = ["那是一只快乐的狗", "那是一个非常快乐的人", "今天是个阳光明媚的日子"]
```

#### `FlagEmbedding` 调用

使用 `BGE M3` 进行密集嵌入的步骤与 `BGE` 或 `BGE` 1.5 模型类似。

使用特殊标记 `[CLS]` 的标准化隐藏状态作为嵌入：

然后计算查询和段落之间的相关性分数：

其中  分别是段落和查询的嵌入向量。

是计算两个嵌入相似度的评分函数（如内积和 `L2` 距离）。

下面这种调用方式是 `bge-m3` 的调用方式，因为初代和第二代的 `bge` 模型都是需要加指令优化的。

```
from FlagEmbedding import FlagModel
model = FlagModel(model_path)
embeddings = model.encode(sentences)
print(f"嵌入向量：\n{embeddings.shape}")
print(f"相似度分数：\n{embeddings @ embeddings.T}")
```

输出：

```
嵌入向量：(3, 1024)
相似度分数：
[[1.         0.77477485 0.5957686 ]
 [0.77477485 0.99999976 0.64835584]
 [0.5957686  0.64835584 1.0000001 ]]
```

#### `SentenceTransformer` 调用



因为`FlagEmbedding`并不能够支持所有的`embedding`模型，所以这里给大家看看如何使用`SentenceTransformer`调用`BGE-M3`模型

```
from sentence_transformers import SentenceTransformer
model = SentenceTransformer(model_path)
embeddings = model.encode(sentences, normalize_embeddings=True)
print(f"嵌入向量：\n{embeddings.shape}")
print(f"相似度分数：\n{embeddings @ embeddings.T}")
```

输出：

```
嵌入向量：
(3, 1024)
相似度分数：
[[1.         0.7747749  0.5957686 ]
 [0.7747749  0.9999999  0.64835596]
 [0.5957686  0.64835596 1.0000001 ]]
```

输出几乎一致，验证了两者推理的`bge-m3`模型效果是一致的。

### OpenAI API 兼容调用

这个部分主要是给大家看看如何使用别人的`embedding`云服务生成文本`embedding`。

```
from openai import OpenAI
client = OpenAI(api_key=os.getenv("API_KEY"), base_url=os.getenv("BASE_URL"))
response = client.embeddings.create(input=sentences, model="BAAI/bge-m3")
embeddings = np.asarray([response.data[i].embedding for i in range(len(sentences))])
print(f"相似度分数：\n{embeddings @ embeddings.T}")
```

输出：

```
[[0.99999997 0.77488069 0.59606279]
 [0.77488069 1.00000009 0.6488824 ]
 [0.59606279 0.6488824  0.99999999]]
```

#### 分析

三种方式结果接近，句 1 和句 2 高相似度（~0.77）反映“快乐”的语义一致性。

## 3.3 查询与语料编码

在检索任务中，如何让查询和语料的嵌入表示更契合呢？这里我们介绍一种方法：通过指令提示优化查询的表示。`FlagModel` 是一个嵌入模型，在初始化时可以通过参数 `query_instruction_for_retrieval` 设置提示，比如 `Represent this sentence for searching relevant passages:`。这个提示的作用是什么？它指导模型生成查询嵌入时，聚焦于与语料库中相关段落的语义匹配，而不是仅仅输出通用的句子表示。

不过需要注意，我这里使用的是 `BGE-M3` 模型，而代码示例是基于初代或第二代 BGE 模型的调用方式。`BGE-M3` 不需要指令也能很好地完成检索任务，这是它相较于早期模型的一个改进点。但如果你追求极致的性能，尤其是在特定领域或复杂查询场景下，可以尝试添加指令并测试效果。建议在你的实际应用中对比两种方式，选择最优方案。来看看代码：

```
model = FlagModel(model_path, query_instruction_for_retrieval="Represent this sentence for searching relevant passages:")
queries = ["query 1", "query 2"]
corpus = ["passage 1", "passage 2"]
q_embeddings = model.encode_queries(queries)
p_embeddings = model.encode_corpus(corpus)
print(q_embeddings @ p_embeddings.T)
```

最后一行 `q_embeddings @ p_embeddings.T` 是什么意思呢？它计算了查询和语料嵌入之间的相似度矩阵。我们一步步拆解它的数学原理。

### 数学原理

假设：

是   的查询嵌入矩阵，每行   表示一个查询的嵌入向量。

是   的语料嵌入矩阵，每行   表示一个段落的嵌入向量。

是   的转置，尺寸为  。

计算过程为：

结果   是一个   的矩阵，其中每个元素   表示第   个查询与第   个段落的相似度，公式为：

如果嵌入向量已归一化（即  ， ），则   等于余弦相似度：

这里的   是两个向量之间的夹角。

### 一个简单例子

假设我们有 2 个查询和 2 个段落，嵌入维度  ：

计算  ：

结果矩阵：

这表示 `query 1` 与 `passage 2` 的相似度 (0.68) 高于 `passage 1` (0.50)，而 `query 2` 与 `passage 2` 的相似度 (1.67) 最高。

## 3.4 多粒度多功能实验

`BGE-M3` 的一个亮点是支持多粒度表示，包括密集向量、稀疏向量和 `ColBERT` 多向量。那这些表示有什么不同呢？我们通过一个实验来展示它的灵活性。

先看一个简单的编码示例：

```
from FlagEmbedding import BGEM3FlagModel
model = BGEM3FlagModel(model_path, use_fp16=True)
sentences = ["What is BGE M3?", "Definition of BM25"]
embeddings = model.encode(sentences, batch_size=12, max_length=8192, return_dense=True, return_sparse=False, return_colbert_vecs=False)
print(embeddings)
```

输出结果：

```
{
  'dense_vecs': array([[-0.03411703, -0.0470783, ..., 0.04828535, -0.02961658],
                       [-0.01041743, -0.04479258, ..., 0.01503996, 0.011138]], dtype=float32),
  'lexical_weights': None,
  'colbert_vecs': None
}
```

这里我们只返回了密集向量（`dense_vecs`），每个句子对应一个固定维度的嵌入表示。`max_length=8192` 设置了最大输入长度，但如果你的句子很短，这个值可以调小以加快编码速度，比如调整 `max_length=10`：

```
embeddings = model.encode(sentences, max_length=10, return_dense=True, return_sparse=True, return_colbert_vecs=True)
print(embeddings)
```

输出结果：

```
{
  'dense_vecs': array([[-0.03411703, -0.0470783, ..., 0.04828535, -0.02961658],
                       [-0.01041743, -0.04479258, ..., 0.01503996, 0.011138]], dtype=float32),
  'lexical_weights': [
    {'What': 0.08362088, 'is': 0.08146952, 'B': 0.12964639, 'GE': 0.25186998, 'M': 0.1700173, '3': 0.2695788, '?': 0.040755093},
    {'De': 0.050144292, 'fin': 0.1368939, 'ation': 0.04513465, 'of': 0.06342196, 'BM': 0.251676, '25': 0.3335321}
  ],
  'colbert_vecs': [
    array([[-0.00867264, -0.04892197, ..., 0.04389409]], dtype=float32),
    array([[0.01715664, 0.03835307, ..., 0.00310116]], dtype=float32)
  ]
}
```

### 结果解读

+ 密集向量：和之前一样，每个句子一个固定向量，适合整体语义匹配。

+ 稀疏向量：`lexical_weights` 显示每个 token 的权重，比如 “What is BGE M3?” 中 “M” 的权重是 0.1700173，而 “3” 是 0.2695788，反映了词汇在上下文中的重要性。

+ ColBERT 向量：`colbert_vecs` 为每个句子生成 token 级别的向量列表，适合细粒度匹配。

## 3.5 稀疏检索与词汇匹配

将 `return_sparse` 设置为 `true`，使模型返回稀疏向量。如果一个词标记在句子中多次出现，它只保留其最大权重。

`BGE-M3`通过在隐藏状态后添加一个线性层和一个`ReLU`激活函数来生成稀疏嵌入：

其中  表示线性层的权重， 是第  个标记的编码器输出。

基于查询和段落中标记的权重，它们之间的相关性分数是通过查询和段落中共存词项的联合重要性计算的：

其中  分别是查询和段落中每个共存词项  的重要性权重。

### BGE-M3 稀疏向量的特点与优势

你有没有想过，传统的词汇匹配和现代语义理解能否结合在一起？`BGE-M3` 就做到了这一点。作为一种深度学习嵌入模型，它生成的稀疏向量不仅记录词汇出现，还通过语义建模为每个词汇分配动态权重。这种方式融合了词汇匹配的直观性和语义理解的深度。比如，在代码示例中，`lexical_weights` 会输出一个词典，每个词（如 `What`、`is`、`B`）对应一个浮点权重，这些权重由模型根据上下文和语义重要性动态计算得出。

相比传统的词频统计，`BGE-M3` 的优势在哪里呢？它能更好地处理同义词、语义相近的表达以及上下文关联性。例如，“`BGE M3 is an embedding model...`” 和 “`What is BGE M3?`” 的匹配分数达到 0.1955，这个分数不仅反映词汇重叠，还捕捉到了语义上的相关性。更妙的是，`BGE-M3` 还支持跨语言任务，生成的匹配分数非常适合现代检索系统，能显著提升召回率和精确度。

### 传统词频表示的特点与局限

再来看看传统方法。传统词频表示，比如 `TF-IDF`，依靠统计手段，通过词频（`TF`）和逆文档频率（`IDF`）来评估词汇的重要性。这种方法简单高效，在纯词汇匹配任务中表现不错。但问题来了：它真的能理解语义吗？显然不能。像 “`dog`” 和 “`puppy`” 这样的同义词，它无法识别语义相似性；而且，它对上下文也不敏感，容易被停用词（如 “`is`”、“`the`”）或高频无关词干扰。在复杂查询或语义检索场景中，这种表面统计的局限性让它比不上深度学习模型。

### 代码示例与效果

想看看 `BGE-M3` 的实际表现吗？下面这段代码展示了它的词权重生成和匹配分数计算：

```
sentences_1 = ["What is BGE M3?", "Definition of BM25"]
sentences_2 = ["BGE M3 is an embedding model...", "BM25 is a bag-of-words..."]
output_1 = model.encode(sentences_1, return_sparse=True)
output_2 = model.encode(sentences_2, return_sparse=True)
print(model.convert_id_to_token(output_1['lexical_weights']))
print(model.compute_lexical_matching_score(output_1['lexical_weights'][0], output_2['lexical_weights'][0]))
```

输出结果如下：

```
[{'What': 0.08362088, 'is': 0.08146952, 'B': 0.12964639, ...}, {...}]
0.19554441515356302
```

从这里可以看出，`BGE-M3` 的稀疏向量不仅捕捉到了词汇，还通过权重反映了语义重要性，表达力明显强于传统词频。

### 上下文如何影响词汇权重？

我们再深入一点。你有没有好奇同一个词在不同句子中的权重会有什么变化？来看这个实验：

```
sentences = [
    "The cat is cute",
    "Is this a test?",
    "The sky is blue and vast"
]
output = model.encode(sentences, return_dense=False, return_sparse=True, return_colbert_vecs=False)
weights = model.convert_id_to_token(output['lexical_weights'])
for i, sentence in enumerate(sentences):
    print(f"Sentence {i+1}: '{sentence}'")
    print(f"Weights: {weights[i]}")
    if 'is' in weights[i]:
        print(f"'is' in Sentence {i+1}: {weights[i]['is']}")
    else:
        print(f"'is' not found in Sentence {i+1}")
```

输出结果如下：

```
Sentence 1: 'The cat is cute'
Weights: {'The': 0.19849324, 'cat': 0.336685, 'is': 0.2108174, 'cute': 0.29809025}
'is' in Sentence 1: 0.2108173966407776

Sentence 2: 'Is this a test?'
Weights: {'Is': 0.15789574, 'this': 0.23845498, 'a': 0.15951744, 'test': 0.34180185, '?': 0.08654297}
'is' not found in Sentence 2

Sentence 3: 'The sky is blue and vast'
Weights: {'The': 0.16425258, 'sky': 0.27996635, 'is': 0.18102533, 'blue': 0.28520262, 'and': 0.16542327, 'vast': 0.2636472}
'is' in Sentence 3: 0.18102532625198364
```

分析一下：同一词 “`is`” 的权重在不同句子中有所变化（0.2108 vs. 0.1810），而且当它出现在句首大写 “`Is`” 时，被视为不同的 `token`。这说明 `BGE-M3` 对上下文和词形非常敏感。

---

## 3.6 多向量检索实验

接下来，我们聊聊 `ColBERT`——一种通过多向量表示提升检索精度和效率的方法。你可能听说过传统的单向量模型，比如 `BERT`，它把文本压缩成一个向量。但如果让你把一篇长文浓缩成一个点，会不会觉得有些细节被忽略了？`ColBERT` 正是为了解决这个问题而设计的。

### 多向量是什么？

与单向量不同，`ColBERT` 为文本中的每个 token 生成一个独立的向量表示。这样，一段查询或文档就变成了一个向量列表，而不是单一向量。这种方式保留了 token 级别的上下文信息，让匹配更加细腻。

多向量方法使用整个输出嵌入来表示查询  和段落 。

其中  是可学习的投影矩阵。

按照 `ColBERT` 的方法，我们使用后期交互计算细粒度相关性分数：

其中  分别是查询和段落的整个输出嵌入。

这是  中每个  与  中向量的最大相似性的平均总和。

### 多向量有什么好处？



## 3.6 多向量检索（ColBERT）

### 核心优势

+   **更精准的语义匹配**

每个 token 都有自己的向量，`ColBERT` 能在查询和文档间进行 token 级别的比较。比如“苹果手机的价格”，它会分别关注“苹果”、“手机”和“价格”的匹配情况，而不是只看整体相似度。

+   **避免信息瓶颈**

单向量容易丢失细节，而多向量保留了更多信息，尤其适合复杂查询或长文档。

+   **高效的后期交互**

`ColBERT` 先独立编码查询和文档，然后通过 `MaxSim` 计算相似性，既精准又高效。

### 实际应用场景

这种方法在哪些地方发光发热呢？比如检索增强生成（RAG）、搜索引擎和问答系统。多向量能更准确地定位相关内容，提升结果质量。

### 效率与存储的平衡

当然，多向量也有挑战，比如存储需求更高。但 `ColBERTv2` 通过残差压缩技术优化了这一点，减少空间占用，同时保持精度。

### 实验效果

我们用 `ColBERT` 计算多向量相似度：

```
output_1 = model.encode(sentences_1, return_dense=True, return_sparse=True, return_colbert_vecs=True)
output_2 = model.encode(sentences_2, return_dense=True, return_sparse=True, return_colbert_vecs=True)
print(model.colbert_score(output_1['colbert_vecs'][0], output_2['colbert_vecs'][0]))
print(model.colbert_score(output_1['colbert_vecs'][0], output_2['colbert_vecs'][1]))
```

输出结果：

```
tensor(0.7797)
tensor(0.4621)
```

分析一下：“What is BGE M3?” 与 “BGE M3 is an embedding model...” 的分数高达 0.7797，而与 “BM25 is a bag-of-words...” 只有 0.4621。这表明多向量检索能有效捕捉细粒度语义相关性。

## 3.7 混合检索实验

到这里，你可能在想：密集向量、稀疏向量和多向量各有优势，那能不能把它们结合起来，发挥更大的作用呢？答案是可以的。我们通过一个实验，测试了 `BGE-M3` 模型在密集（dense）、稀疏（sparse）和多向量（`ColBERT`）模式下的加权混合评分，看看这种组合会带来怎样的效果。

### 实验设计与代码

我们选了两组句子进行对比，一组是查询，另一组是对应的描述：

```
from FlagEmbedding import BGEM3FlagModel

model = BGEM3FlagModel(model_path, use_fp16=True) 

sentences_1 = ["What is BGE M3?", "Definition of BM25"]
sentences_2 = [
    "BGE M3 is an embedding model supporting dense retrieval, lexical matching and multi-vector interaction.",
    "BM25 is a bag-of-words retrieval function that ranks a set of documents based on the query terms appearing in each document"
]

sentence_pairs = [[i, j] for i in sentences_1 for j in sentences_2]

datas = model.compute_score(
    sentence_pairs, 
    max_passage_length=128,  # 缩短最大长度以降低延迟
    weights_for_different_modes=[0.4, 0.2, 0.4]  # 加权融合：0.4*dense + 0.2*sparse + 0.4*colbert
)
print(datas)
```

这里，`weights_for_different_modes=[0.4, 0.2, 0.4]` 定义了三种模式的权重：密集和多向量各占 40%，稀疏占 20%。为什么要这样分配？密集和多向量擅长捕捉语义，而稀疏更聚焦词汇匹配，这种组合旨在平衡语义和字面上的相关性。不过，上面的权重是我随便调的，建议可以自己根据实际场景进行调优。

### 输出结果

运行代码后，我们得到了以下评分：

```
{
  'colbert': [0.7797, 0.4621, 0.4524, 0.7899],
  'sparse': [0.1955, 0.0088, 0.0, 0.1804],
  'dense': [0.6259, 0.3475, 0.3499, 0.6782],
  'sparse+dense': [0.4825, 0.2346, 0.2332, 0.5123],
  'colbert+sparse+dense': [0.6013, 0.3256, 0.3209, 0.6233]
}
```

这些数字分别对应四组句子对的相似度：

1.  “What is BGE M3?” vs. “BGE M3 is an embedding...”

2.  “What is BGE M3?” vs. “BM25 is a bag-of-words...”

3.  “Definition of BM25” vs. “BGE M3 is an embedding model...”

4.  “Definition of BM25” vs. “BM25 is a bag-of-words...”

### 结果分析

让我们来解读一下这些分数：

+   `ColBERT` 分数：多向量模式在第 1 组 (0.7797) 和第 4 组 (0.7899) 表现最佳，这说明它在语义匹配细腻的场景下非常强大，尤其当查询和描述高度相关时。

+   `Sparse` 分数：稀疏模式的得分普遍较低，比如第 2 组 (0.0088) 和第 3 组 (0.0)，反映了它对词汇重叠的依赖。如果两句话词汇差异大，即使语义相关，分数也会偏低。

+   `Dense` 分数：密集模式得分较为稳定，比如第 1 组 (0.6259) 和第 4 组 (0.6782)，表明它在捕捉整体语义上表现均衡。

+   混合评分：最终的加权融合分数（`colbert+sparse+dense`）在第 1 组 (0.6013) 和第 4 组 (0.6233) 依然较高，同时避免了单一模式的极端波动。这种组合似乎在语义和词汇匹配之间找到了一个平衡点。

### 这个实验说明了什么？

通过加权混合，我们可以看到不同模式各司其职：`ColBERT` 提供细粒度语义，密集向量保证整体相关性，稀疏向量补充词汇层面的匹配。调整权重可以根据具体任务优化结果——比如，如果你更看重语义，可以提高密集和多向量的比重；如果更关注关键词匹配，就增加稀疏的权重。你觉得这种灵活性在实际应用中会有多大价值呢？

**建议**：具体的策略，比如权重设置等等，还需要大家自己根据实际情况进行调整！

🙋♂️ 入群交流

1.  **入群方式** (二选一)

+   👸号菜单点击「社群」，扫码直接入群

+   回复关键词「入群」，添加作者🛰️人工邀请

## 📒 四、总结与展望

### 🚀 4.1 技术全景图

+   密集检索：捕捉语义整体，适合快速语义匹配。

+   稀疏检索：强调词汇精确性，弥补语义模型的盲点。

+   多向量检索：细粒度匹配，解锁复杂场景的潜力。

+   混合检索：三者结合，灵活应对多样化需求。

### 📓 4.2 学习汇总

+   理论基础：基于 `XLM-RoBERTa` 和自知识蒸馏，`BGE-M3` 在多任务训练中实现了性能飞跃。

+   实战技巧：通过 `FlagEmbedding` 和 `SentenceTransformer`，我们掌握了密集、稀疏和多向量编码的具体实现。

+   实验洞见：从相似度计算到混合检索，实验结果展示了 `BGE-M3` 在不同多种向量检索方式的适用性。

### 🔥 4.3 动手挑战

1.  多语言检索实验：用 `BGE-M3` 对接一个包含中、英、日三种语言的语料库，测试它在跨语言检索中的表现，并对比密集和稀疏模式的召回率。

2.  长文档处理：找一篇超过 5000 token 的文档（比如一篇论文），用 `BGE-M3` 编码后检索关键段落，看看它在超长文本中的稳定性如何。

3.  混合检索优化：基于本文的混合检索实验，调整 `weights_for_different_modes` 的比例（比如 `[0.5, 0.3, 0.2]`），找到适合你数据集的最佳组合，并记录性能提升。

### ♻️ 4.4 互动问题

1.  你觉得 `BGE-M3` 的多向量检索（`ColBERT`）在哪些实际场景中最有价值？为什么？

2.  在你的 RAG 项目中，密集、稀疏和多向量检索，你会优先选择哪一种？或者你会如何组合它们？

3.  如果让你改进 `BGE-M3`，你会从哪些方面入手（比如训练数据、模型架构或应用场景）？

**欢迎在留言区分享你的实验结果或见解，每条留言我都会认真阅读！你的反馈是我创作的最大动力 ❤️**

---

### 💗 立即行动

1.  **点赞、收藏、关注**，顺手分享给可能感兴趣的朋友。

2.  为账号加个星标或特别关注，获取更多精彩内容。

3.  以本文为灵感，整理一份属于自己的笔记。

---

### 🚗 行动召唤

📢 "与其等着 AI 改变世界，不如自己参与变革！在这里，让 AI 成为你弯道超车的秘密武器。"

---

### 📒附录

（往期文章）

+   RAG去重小助手`SimHash`算法：轻松解决文本相似度检测与查重难题

+   从模式匹配到语义理解：文本嵌入技术的演变与 RAG 系统优化

+   别再瞎猜了！用 `RAGAS` 精准定位 RAG 系统短板

+   PDF提取神器！将双栏学术论文转换为单栏Markdown实战（上）

+   从零打造RAG检索系统：`BM25`让检索快到飞起

+   打造RAG应用必知：`BM25`算法实战解析，让你不落人后

+   手把手教程：用`MinerU`构建自动化文档转换流水线

+   [飞桨版面检测分析的命令看不懂？恭喜！您已获得 arg](http://mp.weixin.qq.com/s/5b8PqqViSpvgMjpfJqOxCQ?token=1077873472&lang=zh_CN)parse 入门手册一本，快快学起来吧！

+   pdf提取逆天神器，基于飞浆平台的版面检测与版面识别(1)

+   [大模型应用极简开发快速入门](http://mp.weixin.qq.com/s/kHeVK6hManVUaci2p-42GQ)

+   [基于 `Streamlit` 和 `DeepSeek` 的智能文档助手开发实战指南](https://mp.weixin.qq.com/s/38BcsiGF1WhKx4DaA-8l0A)

+   告别低效沟通：提示词框架助你玩转`deepseek`
