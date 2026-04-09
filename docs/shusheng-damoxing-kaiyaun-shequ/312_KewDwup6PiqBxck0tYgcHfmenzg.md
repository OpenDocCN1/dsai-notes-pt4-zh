# 1.3 大模型与NLP

> 来源：[https://aicarrier.feishu.cn/docx/UnygdjORloAuB3xmWGkcdIEWncf](https://aicarrier.feishu.cn/docx/UnygdjORloAuB3xmWGkcdIEWncf)

大语言模型（large language models, LLM）是指参数量巨大的语言模型。广义上讲，1.1节提到的encoder-only架构的BERT，encoder-decoder架构的T5和decoder-only架构的GPT，它们都是大模型，例如BERT-base有1.1亿参数；狭义上指的是“生成式大模型”要求至少含有decoder，则不包括BERT族。

### 大模型与传统NLP的区别

大模型与传统NLP的区别在于大模型的泛化性和通用性更强，更加接近通用人工智能（artificial general intelligence, AGI）。我们不需要为不同的NLP任务选择不同的模型，一个大模型可以做许多事情。正如1.1节提到的，BERT可以用作许多自然语言理解（NLU）任务的训练；而decoder-only架构的GPT的泛化性的表现则更加直接：如OpenAI的论文所述，“大语言模型是少样本学习器（LLMs are few shot learners）”，用户只需要在提示词（prompt）中给出少量相关例子，大模型就能“现学现卖”，回答新的问题。

以BERT为例。在上一节的最后，通过介绍词表示的进化历史，我们最后提到了BERT，这是第一款预训练的（pre-trained）大模型产品。BERT诞生可以被视作NLP的革命性时刻。利用这个强大的词表示模型，作为encoder-only架构的大模型，BERT可以在1.1节自然语言理解（NLU）部分提到的下游任务（downstream）上进行进一步训练，这被称作微调（fine-tune）。例如NLU的文本分类任务，过去，我们使用TF-IDF等方法嵌入文本，再使用机器学习分类器，例如朴素贝叶斯、GBDT等；现在，我们使用BERT学习到的词表示作为词嵌入，再对输出进行二分类预测（例如使用一个浅层的神经网络作为分类器），在文本分类数据集上进一步进行训练，使得分类器的参数能充分学习到文本分类任务即可。

### 大模型的微调

同样地，对于自然语言转换（NLT）任务，我们可以选择相应的encoder-decoder架构大模型，如T5，来进行相应任务的微调；对于自然语言生成（NLG）任务，我们可以选择相应的decoder-only架构大模型，如GPT-2，来进行相应任务的微调。

直觉来讲，参数量越大的模型，下游任务微调起来的计算成本越高。例如decoder-only的大模型，在预训练阶段之后还需要进行有监督微调（supervised fine tuning, SFT），来增强它的指令遵循和agent能力；随后还需要进行基于人类反馈的强化学习（reinforcement learning with human feedback, RLHF），来使得大模型和人类伦理对齐。每一步都要消耗大量的人力物力。

我们容易想到的一个问题是：需要调整全部参数吗？固定作为预训练权重部分，只调整为下游任务而新增的额外参数层可以吗？当然可以，这被称作参数高效微调（parameter efficient fine-tuning, PEFT）。例如LoRA（low rank adaptation）就是固定预训练权重而训练了额外的LoRA参数层，只需调整大模型不到0.5%参数量的规模，一些情况下实现了不差甚至更好的表现。