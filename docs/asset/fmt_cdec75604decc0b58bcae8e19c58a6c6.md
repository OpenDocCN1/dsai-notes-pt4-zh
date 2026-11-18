
# T5 模型架构与预训练探索

作者探索了3种模型结构：`Encoder-Decoder`、`Decoder-only` 和 `Prefix LM`。我们对前两种模型结构已经比较熟悉了，而`Prefix LM`的基本思路是结合双向和causal的注意力机制，输入开始的部分token是双向的注意力（我们把这部分叫做前缀），之后的token则使用了causal注意力。在`T5`中，模型将输入中不需要预测的部分（如任务描述和任务的输入）视作前缀，使用双向注意力，而需要模型预测的部分使用causal注意力。在`Prefix LM`预训练时，注意力的掩码情况如下图所示：

![](img/eb77972eb9a6e580402824f972b306b8.png)

实验结果表明，在同等量级的模型中，`encoder-decoder`架构的模型的表现最好。

## 预训练

在探索`T5`的预训练任务时，作者可谓是把预训练数据掰开了揉碎了利用。探索了最佳的训练目标和文本破坏策略。

### 训练目标

作者探索了几种不同的训练目标，具体如下表所示：

> 假设我们的训练数据集中有一组数据"人闲桂花落，夜静春山空"

| 训练目标 | 输入 | 预期输出 |
| --- | --- | --- |
| `GPT-style`(`Prefix-style LM`) | 人闲桂花 | 落，夜静春山空 |
| `BERT-style`(`Masked LM`) | 人闲`[MASK]`[`[MASK]`]落，夜深春山空 | 人闲桂花落，夜静春山空 |
| 还原打乱的文本顺序(`De-shuffling`) | 花桂人，落闲山空春静夜 | 人闲桂花落，夜静春山空 |

论文中多组实验表明`BERT-style`的训练目标能够让语言模型达到最好的结果。

### 文本破坏策略

在确定了`BERT-style`训练目标的基础上，作者又探索了几种不同的文本破坏（掩码）策略，具体如下表所示

> 假设我们的训练数据集中有一组数据"人闲桂花落，夜静春山空"

| 破坏策略 | 输入 | 预期输出 |
| --- | --- | --- |
| `BERT-style` | 同上`BERT-style` | 同上`BERT-style` |
| 简化的`BERT-style`(`MASS-style`) | 同上 | 同上 |
| 替换几小段文本(`Replace`) | 人闲`[X]`落，`[Y]`静春山空 | `[X]`桂花`[Y]`夜`[Z]` |
| 删除几小段文本(`Drop`) | 桂花落，夜静山空 | 人闲春 |

> *: 该方法只挑选15% token替换为`[MASK]`，删除了`BERT`中替换随机token和token保持不变的策略

> **: `[Z]`表示输出结束

论文中多组实验表明`Replace`的破坏策略能够让语言模型达到最好的结果。

之后，作者还探索了较好的文本破坏比例以及替换段落的较好长度等超参，此处不再赘述其结果了，感兴趣的读者可以去看看原论文。

## 微调

作者比较了几种当时提出的非全量微调策略（`adapter` 和 `gradual unfreezing`）和全量微调策略，发现全量微调表现最好，这个结果并不令人意外。

> FYI:

1. `Adapter`策略是提出比较早的`PEFT`(Parameter Efficient Fine-Tuning, 参数高效微调)策略，该方法的思想是在每个`transformer-block`的`FFN`层之后加入一系列`Linear-ReLU-Linear`层，在微调时仅仅更新加入的参数而保持原始模型参数不变。

1. `Gradual unfreezing`策略的思想是在进行首轮微调时仅更新部分模型参数，之后的轮次逐渐增加参数更新量。

作者还在其他方面（如训练数据配比，模型规模大小等）做了探索，限于篇幅，此处就不再赘述了。

总之，虽然以`T5`为代表的`Encoder-Decoder`模型没能成为`LLM`的主流，但其大量的探索为后续的研究者们提供了宝贵的经验和最佳实践。

> 这篇文章仅仅介绍了预训练语言模型的冰山一角，现在流行的开源模型基座如`LLaMA`、`Qwen`、`InternLM`、`GLM`等系列模型在本文皆未涉及。感兴趣的读者可以阅读其源码和相关技术报告。读者如发现本文有任何错误或疏漏，或有任何想与笔者交流的想法，欢迎发邮件至1030587461@qq.com

## 参考文献

1. `https://arxiv.org/pdf/1706.03762` Attention Is All You Need

1. `https://arxiv.org/pdf/2001.08361` Scaling Laws for Neural Language Models

1. `https://arxiv.org/pdf/1810.04805` BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding

1. `https://zhuanlan.zhihu.com/p/622688415` BERT及其变体

1. `https://arxiv.org/pdf/1910.01108` DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter

1. `https://arxiv.org/pdf/1909.11942` ALBERT: A Lite BERT for Self-supervised Learning of Language Representations

1. `https://arxiv.org/pdf/1907.11692` RoBERTa: A Robustly Optimized BERT Pretraining Approach

1. `https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf` Improving Language Understanding by Generative Pre-Training

1. `https://d4mucfpksywv.cloudfront.net/better-language-models/language-models.pdf` Language Models are Unsupervised Multitask Learners

1. `https://arxiv.org/pdf/2005.14165` Language Models are Few-Shot Learners

1. `https://arxiv.org/abs/2203.02155` Training language models to follow instructions with human feedback

1. `https://arxiv.org/abs/2303.08774` GPT-4 Technical Report

1. `https://arxiv.org/abs/1910.10683` Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer

1. `https://zhuanlan.zhihu.com/p/88438851` T5 模型：NLP Text-to-Text 预训练模型超大规模探索

## 相关链接

1. `transformers`版BERT: `https://github.com/huggingface/transformers/blob/main/src/transformers/models/bert/modeling_bert.py`

1. `transformers`版GPT-2: `https://github.com/huggingface/transformers/blob/main/src/transformers/models/gpt2/modeling_gpt2.py`

1. `transformers`版T5: `https://github.com/huggingface/transformers/blob/main/src/transformers/models/t5/modeling_t5.py`
