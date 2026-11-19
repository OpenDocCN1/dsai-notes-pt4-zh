
# 测评 · LongBench：衡量模型的「长」

> 来源：`https://ai.feishu.cn/docx/L2KndzlxnoH82MxB27FcabR2ngh`

![](img/271d626665f69f81c832723bcf25c8f4.png)

上下文窗口大小是影响模型解决更广泛问题的重要维度之一。近期包括 ChatGLM2 等在内的多个模型都在努力尝试在保证性能的同时，将模型上下文长度尽可能地拓展，达到千/万 tokens 级别。

然而，现有的模型评测集长度多数比较短，仅在百/千 tokens 量级，并不能很好地评测模型的长文本理解能力，尤其是中文的长文本理解能力。

为了解决这一问题，GLM 技术团队基于内部长期的探索，开发了专门针对模型长文本理解能力的评测数据集 LongBench。

该数据集包含了 13 个英文任务、5个中文任务和 2 个代码任务。多数任务的平均长度在5k-15k之间，共包含约4500条测试数据。

从主要任务分类上，LongBench包含单文档QA、多文档QA、摘要、Few-shot学习、代码补全和合成任务等六大类任务 20 个不同子任务。

具体来说，LongBench 有以下特点：

### 双语

LongBench 能够针对中、英双语的长文本进行更全面的评估。

### 多任务

LongBench由六大类、二十个不同的任务组成，覆盖了单文档QA、多文档QA、摘要、Few-shot学习、代码补全和合成任务等关键的长文本应用场景。

### 自动评测

我们深知模型评测过程中可能产生的高昂成本，尤其是长文本场景下（如人工标注成本或API调用成本）。因此，我们采用了一种全自动的评测方式，旨在以最低的成本，最有效地衡量和评估模型的长文本理解能力。

利用该评测数据集，我们分别对 GPT-3.5-Turbo-16k、Llama2-7B-chat-4k、LongChat-7B-16k、XGen-7B-8k、InternLM-7B-8k、ChatGLM2-6B、ChatGLM2-6B-32k 等 7 个支持长文本的模型的性能。

### 注

ChatGLM2-6B-32K在ChatGLM2-6B的基础上进一步强化了对于长文本的理解能力，能够更好的处理最多32K长度的上下文。具体地，我们基于位置插值（Positional Interpolation）的方法对位置编码进行了更新，并在对话阶段使用 32K 的上下文长度训练。

### 下载使用

- Github：`https://github.com/THUDM/ChatGLM2-6B`
- HF：`https://huggingface.co/THUDM/chatglm2-6b-32k`

如下所示：

![](img/0743887d49446c18d0e4e735e11b5d83.png)

![](img/681f0c7b59f4a93c77bdfae6e0c33042.png)

## 不同长度文本下的能力变化

为了更有针对性地分析模型在不同文本长度下的相对表现，下图展示了模型在不同文本长度区间上，所有任务上的平均相对分数。

![](img/1ce21633e3ced1086df8ae978e495d3b.png)

我们希望，该数据集能够帮助评估模型在长文本理解方面的表现，促进模型的进一步发展和改进。

更多细节，请参考：`https://github.com/THUDM/LongBench`

## LongBench 使用

### 载入数据

通过`Hugging Face` `datasets`来下载并载入LongBench的数据：

```
from datasets import load_dataset
datasets = ["hotpotqa", "2wikimqa", "musique", "dureader", "narrativeqa", "qasper", "multifieldqa_en", "multifieldqa_zh", "gov_report", "qmsum", "vcsum", "trec", "nq", "triviaqa", "lsht", "passage_count", "passage_retrieval_en", "passage_retrieval_zh", "lcc", "repobench-p"]
for dataset in datasets:
    data = load_dataset("THUDM/LongBench", dataset, split="test")
```

### 数据格式

LongBench中所有数据都统一为以下格式：

```
{
    "input": "任务的输入/指令，通常较短，比如QA中的问题、Few-shot任务中的提问等",
    "context": "任务所需的长语境文本，比如文档、跨文件代码、Few-shot任务中的few-shot样本",
    "answers": "由所有标准答案组成的列表",
    "length": "前三项文本的总长度（中、英文分别用字、词数统计）",
    "dataset": "本条数据所属数据集名称",
    "language": "本条数据的语言",
    "all_classes": "分类任务中的所有类别，非分类任务则为null",
    "_id": "每条数据的随机id"
}
```

### 评测

我们以`ChatGLM2-6B`为例提供了一份评测代码。首先，运行仓库下的`pred.py`

```
CUDA_VISIBLE_DEVICES=0 python pred.py
```

可以在`pred/`文件夹下得到模型在所有数据集下的输出，此后运行`eval.py`的评测代码：

```
python eval.py
```

可以在`result.json`中得到在各数据集上的评测结果。请注意，我们在`config/`下提供了我们总结出来的在各数据集上适合的输入格式和最大输出长度限制，在评测的时候可以进行修改以更好地适用你要评测的模型，修改后在`pred.py`评测时会自动按照新的格式去整理数据并得到对应的模型输出。

### 下载使用

- Github：`https://github.com/THUDM/LongBench`
- HF：`https://huggingface.co/datasets/THUDM/LongBench`
