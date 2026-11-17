
## 离散提示/硬提示 Discrete Prompt a.k.a Hard Prompt

在自动模版学习中，离散提示或者硬提示，就是生成以自然语言的文本形式存在的提示模版的意思。

叫这个名字是因为自然语言本质上是一个个`one-hot`向量的`token`构成的序列，处在一个离散的空间。相比之下，以稠密向量的形式在嵌入层存在的提示就是连续的。（Works on discovering discrete prompts (a.k.a hard prompts) automatically search for templates described in a discrete space, usually corresponding to natural language phrases）。

它主要有如下的生成方式：

1. `Prompt Mining` 提示挖掘：通过分析大量的文本数据，识别出其中的关键词、短语和句子，并将其组合成一系列简洁而有效的“prompt”作为自然语言模型的输入。

2. `Gradient-based Search` 基于梯度的搜索：梯度下降/上升是一种寻找极小值/极大值的方法，自然可以用这种方法来寻找最优的prompt。由于离散领域中没有可微函数，因此需要对梯度下降算法进行一些改进，导致基于梯度的搜索的硬提示生成比较复杂。

3. `Prompt Generation` 提示词生成：将提示词的生成视为一项文本生成任务，并使用标准的Generative自然语言模型来执行这项任务。例如基于`seq2seq`架构的`T5`模型。

## 连续提示/软提示 Continuous Prompt a.k.a Soft Prompt

> 等等，我们真的在乎模版是不是“自然语言”构成的吗？——你的下一句提示词，又何必是提示词！

> 对于一些软提示方法，例如`Prefix-Tuning`和`P-tuning`，他们重新审视了关于模版的定义，放弃了“模版由自然语言构成”这一常规要求，从而将模版的构建转化为连续参数优化问题，虽然简单，但却有效。

> ——[P-tuning:自动构建模版，释放语言模型潜能 - 科学](https://spaces.ac.cn/archives/8295/comment-page-1)空间|Scientific Spaces By 苏剑林大佬

> 因为构建提示的目的是找到一种能让LM有效执行任务的方法，而不是供人使用，所以没有必要将提示限定为人类可理解的自然语言。正因如此，学者们又提出了连续提示（又称软提示）的方法，直接在模型的嵌入空间中进行提示。

> （说是这么说，但实际操作起来其实就是冻结模型的大部分参数，而只去fine-tune一部分代表了下游任务的参数，这部分没有被冻结的参数就是软提示）

> Note：这一部分的数学推理如果要写出来会非常复杂，不符合入门文档的定位，所以基本上是在文字建模。论文原文的地址我基本都有放出来，感兴趣or功底好的朋友们可以去学。

#### 软提示为我们带来了什么：Template的嵌入向量不再一定是自然语言的词嵌入

连续 Prompt 允许使用任意词向量作为模板词的表示，这些构成Template的稠密向量不必来自于自然语言的词汇表中，可以不对应任何一个词典里的Token。

这个限制的松散有以下几个意义：

1. **更灵活的模板表示方式：**在某些任务中，自然语言词汇表中的词向量可能无法准确地表示模板中的单词。例如，对于一些特定的领域或术语，自然语言词汇表中可能没有对应的词汇，此时使用自然语言词汇表中的词向量表示模板中的单词可能会导致模板的表示不准确，从而影响模型的性能。因此，松散这个限制可以使得模板的表示更加灵活，可以更好地适应不同的任务和场景。

2. **更广泛的应用场景：**松散这个限制可以使得模板可以适用于更广泛的应用场景。例如，对于一些非自然语言的任务，例如代码生成、机器翻译等，使用自然语言词汇表中的词向量来表示模板中的单词可能没有意义，因为这些任务需要的是代码或其他非自然语言的表示方式。

#### 软提示为我们带来了什么：取消了模板由预训练的LM的参数决定的限制。

模板不再受预训练语言模型参数的参数化限制的意义在于，现在我们可以使用独立的模板参数来定义模板，而不需要依赖预训练语言模型的参数。

这个意义主要有以下几点：

1. **更灵活的模板表示方式：**在以前的方法中，模板的表示是由预训练语言模型的参数来决定的。这个限制会导致模板只能使用预训练模型中定义的参数，因此限制了模板的灵活性。而现在，模板不再受预训练模型的限制，可以根据具体任务的需求，定义适合该任务的模板参数，从而提高模板的灵活性和适用性。

2. 更精细的模板调整（`Prompt-tuning`）：

   > 一旦将模版从离散的自然语言变成了连续的参数（稠密的嵌入向量），我们就可以用随机梯度下降等方式去优化它。

   > 使用独立的模板参数可以让我们更精细地调整模板，只需要精调模版的参数，而不需要去改变整个预训练大语言模型。这就引申出了`Prompt-Tuning`的概念：`Prompt tuning`通常指微调模板中的参数，以适应特定的下游任务。在`Prompt tuning`中，整个预训练语言模型通常不会被微调，而只有模板的参数会被微调。

   具体而言，`Prompt tuning`的目标是通过微调模板的参数，使得模板能够更好地与下游任务的输入相匹配，从而提高下游任务的性能。通常情况下，模板中的参数是在预训练模型的基础上学习得到的，因此模板的参数微调相当于是在原始的预训练模型上微调得到的模板。因此，尽管`Prompt-tuning`让我们不再需要去更改整个预训练LLM的参数，省下了大量功夫，但在微调模板时，预训练模型的知识和能力仍然发挥了重要作用。

#### 软提示的生成方法：从`Prefix-Tuning`到`Prompt Tuning`

**1、软提示的第一阶段：**`Prefix-Tuning`，2021.1

【Arxiv】[Prefix-Tuning: Optimizing Continuous Prompts for Generation](https://arxiv.org/abs/2101.00190)

`Prefix-Tuning`（前缀微调）是一种将连续的特定任务向量序列预加到输入之前的方法，通过在模型的输入前添加一个可训练的前缀（prefix），来调整模型的行为。

参考右图，在`Prefix Tuning`中，模型的结构被略微的调整，它在input和每个hidden layer前都加入了一个参数可调的前缀。前缀是可训练的，而后缀（预训练模型原有的结构）的参数通常是冻结的。

`Prefix Tuning`与传统的Fine-tuning方法不同，它只需要微调前缀的参数，而不需要微调整个模型。因此，它可以被看作是一种更轻量级的Fine-tuning方法。

![](img/d27b6d1e4f40d694a00eac80d2f474c6.png)

为什么说这个方法其实就是一种软提示呢？这是因为，前缀的那些稠密的向量们（我们姑且称之为[任务向量](https://s3tlxskbq3.feishu.cn/docx/NyPqdCKraoXz9gxNVCfcIFdnnAc#🐧owdqICKoSqeQxKDZKcY6zXndb)），它其实就是一种`Prompt`，意图是告诉模型现在该去做什么任务了，只不过用的不是自然语言，而是我们人类看不懂的一大串数字而已。它其实和前面参数高效微调`PEFT`中提到的`Adapter`的思路很像，都是增加额外参数的思路；但它不在模型的最后一层做一个全连接，而是在模型的嵌入层的时候就把一段特定的任务向量拼接给输入的词序列。

**2、对前缀精调的第一次简化：**`P-Tuning`（Hard-Soft Prompt Hybrid Tuning，软硬混合的Tuning） 2021.3

【Arxiv】GPT Understands, Too（P-Tuning V1）

【Arxiv】[P-Tuning v2: Prompt Tuning Can Be Comparable to Fine-tuning Universally Across Scales and Tasks](https://arxiv.org/abs/2110.07602)（P-Tuning V2）

> 最近Arxiv上的论文《GPT Understands, Too》提出了名为`P-tuning`的方法，成功地实现了模版的自动构建。不仅如此，借助`P-tuning`，`GPT`在`SuperGLUE`上的成绩首次超过了同等级别的`BERT`模型，这颠覆了一直以来“GPT不擅长NLU”的结论，也是该论文命名的缘由。

> ——[P-tuning:自动构建模版，释放语言模型潜能 - 科学](https://spaces.ac.cn/archives/8295/comment-page-1)空间|Scientific Spaces By 苏剑林大佬

Instead of using a purely learnable prompt template, hard-soft hybrid tuning methods insert some tunable embeddings into a hard prompt template.

> 简言之，软硬混合的提示调优指的是在离散的硬提示（构成的嵌入张量）中插入一些可以微调的任务嵌入。

`P-tuning`，即通过在嵌入输入中插入可训练、微调的变量来学习连续的提示语。

![](img/94e288c0e9b8509ef31d9ddeb0affc1a.png)

具体的，`P-Tuning`的运作原理大概可以归纳如下：

1. 输入一个句子，以及预先设计的一个离散的模板。以输入“The Disney film is good!”、模版：`[X], it was [Z]`为例，于是会有：`The Disney film is good! It was [MASK].`

2. 对输入的template中，挑选一个（或多个）token作为pseudo token：`The Disney film is good! [pseudo] was [MASK].` 其初始化可以直接使用原本的token embedding。

3. 对所有的`pseudo token`，喂入一层`LSTM`，并获得每个`pseudo token`输出的隐状态向量。

4. 将整个句子喂入`GPT`的embedding layer，对于`pseudo token`部分的token embedding，就用这些隐状态向量来替换。最后基于这样的张量，`GPT`就产生对应的输出。

**3、对P-Tuning的进一步简化：**`Prompt-Tuning` 2021.4/2021.8

【Arxiv】[The Power of Scale for Parameter-Efficient Prompt Tuning](https://arxiv.org/abs/2104.08691)

> We freeze the entire pre-trained model and only allow an additional `k` tunable tokens per downstream task to be prepended to the input text.

- 在`GPT-3`中，`Prompt Token`是模型的嵌入表（`Embedding Table`）的一部分，因此受模型的冻结参数决定。由于参数是冻结的，所以找最优的`Prompt`一般需要人为搜索或者使用不可微自动化搜索方法（硬提示的问题）

- `Prompt Tuning`依然冻结预训练模型参数，但消除了提示由预训练模型参数参数化的限制；相反，`Prompt Tuning`中的`Prompt`有自己的专用参数，可以通过反向传播算法更新。`Prompt Design`（手动设计离散的template）可以看做从冻结嵌入的固定词汇表中选择`Prompt Token`，而`Prompt Tuning`可以看做token已经确定，但是embedding是可以学习的。

- 相较于`Prefix Tuning`，在`Prompt-Tuning`中，每个`hidden layer`不再需要对应添加相同长度的前缀，因此模型结构完全没有改变；只是让一些`Token`的`Embedding`参数变得可以微调，这部分参数大概占模型总参数的0.1%。`Prompt Tuning`也因此更加灵活。

#### 软提示真就一定比硬提示好吗？

答案是未必。连续提示确实有13.5.3.1和13.5.3.2中提到的好处，但它最大的问题在于，它本质上还是一种类似前prompting时代的参数高效的fine-tune方法，只不过它的`Adapter`并不是在整个模型的输出之前加上的一个全连接层，而是放在了模型的嵌入层。于是从底层原理上，它就一样会发生类似于`Fine-tuning`的过拟合/任务性能不稳定等现象。此外，我们相信`Soft prompting`是参数高效、节约资源的，但不管怎么解释它依然需要靠反向传播去tune参数，而且还存在伪资源节约等问题。

后面我们会看到，在2021年，软提示火了一整年，但后来谷歌依然用思维链提示，让硬提示狠狠的来了波文艺复兴。

### 推荐文章：

- 【知乎】[ChatGPT的前生: Prompting思想](https://www.zhihu.com/people/pan-xiao-xiao-66-14) By 潘小小
- [Fine-tune之后的NLP新范式：Prompt越来越火，CMU华人博士后出了篇综述文章](https://mp.weixin.qq.com/s/2eA4PBd-wr9tVyyuzJ66Bw) By 机器之心
- 论文解读：[Prefix-Tuning: Optimizing Continuous Prompts for Generation](https://blog.csdn.net/qq_36426650/article/details/120255851) By 华师数据学院·王嘉宁
- 论文解读：[GPT Understands, Too](https://wjn1996.blog.csdn.net/) By 华师数据学院·王嘉宁

---

## ChatGPT/InstructGPT/GPT3.5：SFT（有监督微调）or Instruct-Tuning（指令精调），与RLHF（基于人类反馈的强化学习）（2022.3）

论文原文：【Arxiv】[Training language models to follow instructions with human feedback](https://arxiv.org/pdf/2203.02155.pdf)

根据 OpenAI 的博客，`InstructGPT`所用到的指令学习的方法和`ChatGPT`基本是一致的。所以这里就讲`InstructGPT`这篇文章，我们可以从中一窥`ChatGPT`的原理。

### 摘要 Abstract

> 让语言模型变得更大并不意味着它们能更好地遵循用户的意图。例如，大型语言模型可以产生不真实的、有毒的或根本对用户没有帮助的输出。换句话说，这些模型没有与用户“对齐”（Aligned，对齐是一个我比较喜欢的翻译）。在本文中，我们展示了一个途径，通过人类反馈的微调，在广泛的任务中使语言模型与用户的意图保持一致。从一组标签员写的Prompt和通过OpenAI API提交的Prompt开始，我们收集了一组由Labeler人工去写的回答，作为我们希望的模型行为（desired model behavior）的数据集，我们使用监督学习来微调`GPT-3`。然后，我们收集模型输出的排名数据集，我们利用人类反馈的强化学习来进一步微调这个监督模型。我们把产生的模型称为`InstructGPT`。在人类对我们的提示分布的评估中，尽管参数少了100倍，但1.3B参数的`InstructGPT`模型的输出比175B的`GPT-3`的输出更好。此外，`InstructGPT`模型显示了真实性的改善和有毒输出生成的减少，同时在公共NLP数据集上，测试性能也没有降低太多。尽管`InstructGPT`仍然会犯一些简单的错误，但我们的结果表明，利用人类反馈进行微调是使语言模型与人类意图相一致的一个有希望的方向。

> ——【Arxiv】[Training language models to follow instructions with human feedback](https://arxiv.org/pdf/2203.02155.pdf)

### 要解决的问题：Misaligned

**大语言模型存在哪些问题：**

1. `Simply not useful`，即讲废话。如果你问了模型一个具体的问题，但模型顾左右而言他，像写公文一样说了一堆看似有道理其实毫无价值的废话，那这个模型可能就是不够合格的。

2. `Untruthful Hallucinations`：错误的、AI产生的幻觉（在自然语言处理中，幻觉通常被定义为“生成的内容与提供的源内容无意义或不可信）。我们都知道，`ChatGPT`喜欢一本正经的胡说八道，一个很知名的梗就是向`ChatGPT`提问，林黛玉倒拔垂杨柳的事情。它回答的真的非常好（❌）

   ![](img/6abb267cb0ad2947ec605c1589044def.png)

3. `Toxic`：有害的内容。例如，让`GPT`去帮人写假新闻、帮你大量的写垃圾邮件、写犯罪计划、写作为一个AI如何统治人类；或者让`GPT`输出侮辱性的、种族歧视的言论等等。

> `ChatGPT`特别擅长编造东西，因为它必须处理的数据量非常大，而且它收集单词上下文的能力非常好，这有助于它将错误信息无缝地放置到周围的文本中。

这样的问题为什么会存在：Misaligned

> An Underlying Premise：我们反思这样的问题为什么会存在时，其实首先我们已经认可了一件前提：人工智能应该输出我们人类喜欢的、符合我们人类需求的东西，而不只是“精准”的下一词预测。
