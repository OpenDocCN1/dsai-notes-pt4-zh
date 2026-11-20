
# LLM为什么Decoder only架构

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Kd51dMw9Copr22xsfWdca9QonAc](https://qqs7y1hozd1.feishu.cn/docx/Kd51dMw9Copr22xsfWdca9QonAc)

LLM 是 “Large Language Model” 的简写，目前一般指百亿参数以上的语言模型，主要面向文本生成任务。跟小尺度模型（10亿或以内量级）的“百花齐放”不同，目前LLM的一个现状是`Decoder-only`架构的研究居多，像OpenAI一直坚持`Decoder-only`的`GPT`系列就不说了，即便是Google这样的并非全部押注在`Decoder-only`的公司，也确实投入了不少的精力去研究`Decoder-only`的模型，如`PaLM`就是其中之一。那么，为什么`Decoder-only`架构会成为LLM的主流选择呢？

`Transformer` 模型一开始是用来做 `seq2seq` 任务的，所以它包含 `Encoder` 和 `Decoder` 两个部分；他们两者的区别主要是，`Encoder` 在抽取序列中某一个词的特征时能够看到整个序列中所有的信息，即上文和下文同时看到；而 `Decoder` 中因为有 `mask` 机制的存在，使得它在编码某一个词的特征时只能看到自身和它之前的文本信息。

首先概述几种主要的架构:

+   以`BERT`为代表的`encoder-only`
+   以`T5`和`BART`为代表的`encoder-decoder`
+   以`GPT`为代表的`decoder-only`,
+   以`UNILM9`为代表的`PrefixLM`(相比于`GPT`只改了`attention mask`，前缀部分是双向，后面要生成的部分是单向的`causal mask`%)

然后说明要比较的对象: 首先淘汰掉`BERT`这种`encoder-only`，因为它用`masked language modeling`预训练，不擅长做生成任务，做`NLUQ`一般也需要有监督的下游数据微调: 相比之下`decoder-only`的模型用`next token prediction`%预训练，兼顾理解和生成，在各种下游任务上的`zero-shot`和`few-shot`泛化性能·都很好。我们需要讨论的是，为啥引入了一部分双向`attention`的`encoder-decoder`和`Prefix-LM`没有被大部分大模型工作采用? (它们也能兼顾理解和生成，泛化性能也不错)

## 1. `Encoder`的低秩问题

LLM之所以主要都用`Decoder-only`架构，除了训练效率和工程实现上的优势外，在理论上是因为`Encoder`的双向注意力会存在低秩问题，这可能会削弱模型表达能力，就生成任务而言，引入双向注意力并无实质好处。而`Encoder-Decoder`架构之所以能够在某些场景下表现更好，大概只是因为它多了一倍参数。所以，在同等参数量、同等推理成本下，`Decoder-only`架构就是最优选择了。（参考：为什么现在的LLM都是`Decoder-only`的架构？）

## 2. 更好的`Zero-Shot`性能、更适合于大语料自监督学习

首先，对 `encoder-decoder` 与 `decoder-only` 的比较早已有之。先把目光放放到模型参数动辄100B之前的时代，看看小一点的模型参数量下、两个架构各有什么优势——Google Brain 和 HuggingFace联合发表的 What Language Model Architecture and Pretraining Objective Work Best for Zero-Shot Generalization? 曾经在5B的参数量级下对比了两者性能。

论文最主要的一个结论是：`decoder-only` 模型在没有任何 `tuning` 数据的情况下、`zero-shot` 表现最好，而 `encoder-decoder` 则需要在一定量的标注数据上做 `multitask finetuning` 才能激发最佳性能。 而目前的`Large LM`的训练范式还是在大规模语料上做自监督学习，很明显，`Zero-Shot`性能更好的`decoder-only`架构才能更好地利用这些无标注数据。此外，`Instruct GPT`在自监督学习外还引入了`RLHF`作辅助学习。`RLHF`本身也不需要人工提供任务特定的标注数据，仅需要在`LLM`生成的结果上作排序。虽然目前没有太多有关`RLHF + encoder-decoder`的相关实验，直觉上`RLHF`带来的提升可能还是不如`multitask finetuning`，毕竟前者本质只是`ranking`、引入监督信号没有后者强。

## 3. 效率问题

`decoder-only`支持一直复用`KV-Cache`，对多轮对话更友好，因为每个`Token`的表示之和它之前的输入有关，而`encoder-decoder`和`PrefixLM`就难以做到。
