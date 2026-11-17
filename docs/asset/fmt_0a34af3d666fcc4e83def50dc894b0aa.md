
# 令人信服的决策!统一的两阶段框架:自我归因与决策

> 来源：`https://agijuejin.feishu.cn/docx/ZwFbd1A9dobubcxVMBHcVkLAnIg`

> 哈工大SCIR｜[阅读原文](https://mp.weixin.qq.com/s/IDIMMrOt2CdiOVsUD2aeuQ)

> 转载请联系原作者取得授权

> 论文：Make Your Decision Convincing! A Unified Two-Stage Framework: Self-Attribution and Decision-Making

> 作者：杜晏睿，赵森栋等

> 链接：`https://arxiv.org/pdf/2310.13610.pdf`

> 转载须标注出处：哈工大SCIR

## 1. 背景

通过自然语言解释黑箱模型行为在各种自然语言处理（NLP）任务中取得了令人印象深刻的成果。其中一种形式是，如图1所示，利用输入文本的子序列作为证据，为用户提供支持模型决策的合理解释。

![](img/3eca6d5c5ba2be3676025545c513caaa.png)

图1 将Claim和Passage作为模型输入，模型的输出为决策和支撑其决策的证据。

尽管现有框架在生成高质量的证据和实现高任务性能方面表现出色，但它们忽略了所提供的证据和决策之间的不可靠联系。简单地说，如图1所示，模型可能在提供错误的证据时做出正确的决策，或者在提供正确的证据时做出错误的决策。为了缓解这一问题，我们提出了一个统一的两阶段框架，称为`Self-Attribution and Decision-Making` (`SADM`)。

## 2. SADM框架

### 2.1 总体架构

人类推理理论(Evans, 1984)认为人类推理有两个过程:启发式过程和分析过程。在启发式过程中，个体收集与任务相关的信息，而分析过程则涉及对收集到的信息进行操纵和加工以做出判断。从这种认知思维中获得灵感，我们提出了`SADM`框架，如图2所示。首先，我们使用`Trationale`模板来提示模型生成与任务相关的证据，这个过程称为`Self-Attribution`。随后，我们使用`Tanswer`模板来提示模型根据生成的证据做出其决策，该过程称为`Decision-Making`。关于提示模板的选择，我们利用面向人类理解的自然语言。以事实验证任务（`FEVER`）为例。对于`Trationale`模板，我们将其设计如下：`Extract the rationale from the passage to assess the claim`。对于`Tanswer`模板，我们将其设计为：`Refer to the following information to judge the claim`。我们使用了离散提示模板，并不会引入额外的参数或训练成本。

此外，为了处理冗长文本输入的问题，我们引入了基于`seq2seq`模型的`FID`架构。`FID`的结构描述如下：首先将长文本分成多个片段，分别表示为`{seg1, seg2，…, segn}`。接下来，将问题`q`与每个片段`segi`结合，在编码器模块中单独编码，生成多个向量表示`{e1, e2，…, en}`。最后，所有的向量表示都连接为`e1⊕e2…⊕en`，转发给解码器模块进行进一步处理。

![](img/a91363525c41ac2a8d710f8101a5807c.png)

图2 `SADM`框架的示例图

### 2.2 训练和推理

#### a) 训练

训练阶段旨在实现两个目标:

+   `Orationale`：训练模型在`Trationale`模板的提示下生成与任务相关的证据。

+   `Oanswer`：训练模型在`Tanswer`提示下根据提供的证据做出决策。

如图3所示，对于目标`Orationale`，`Trationale`、问题`q`和文本`p`作为模型的输入，使用人工注释的证据`r`作为学习目标。对于目标`Oanswer`，`Tanswer`、问题`q`和人工注释的证据`r`作为输入，将决策`y`作为学习目标。训练过程中，我们采用联合训练策略，即将两个目标的损失相加。

此外，为了增强模型在两个阶段的能力，我们分别采用了`Sentence Mark` (`SM`)策略和`Reasoning Augment Learning` (`RAL`)策略。

+   `SM`策略：最近的研究强调了生成式模型的创造性。因此，我们有一个担忧，即生成模型可能会产生随机和不相关的自然语言作为证据。为了缓解这个问题，我们采用了`SM`策略，该策略在文本`p`中的每个句子之前添加索引号。如一段文本由`n`个句子组成`(s1，…，sn)`。添加索引后，段落的形式为`(S1: s1，…，Sn: sn)`，其中大写字符为句子索引。若应用`SM`策略，在训练过程中优化目标`Orationale`时，需要将人工标注的句子索引作为学习目标，而不是自然语言形式的证据。

+   `RAL`策略：考虑到人类无论面对细粒度信息(如人类注释的证据)还是粗粒度信息(如整段文本)，都有能力做出合理的决策。因此，我们同样希望赋予模型感知不同粒度信息的能力，从而增强其推理能力。为了实现这一点，我们利用了监督数据中可用的丰富信息。如图3所示，对于目标`Oanswer`，我们增加了两种训练样本格式。分别将`Tanswer`、问题`q`、文本`p`作为输入和将`Tanswer`、问题`q`、人工注释的证据`r`、文本`p`作为输入，同时以决策`y`作为学习目标，计算出的损失添加到联合训练的损失中。

![](img/8aa450885244c7cad8174c2583ae2632.png)

图3 `RAL`策略

#### b) 推理

在推理阶段采用`pipeline`的策略。首先使用`Trationale`模板提示模型，模型会根据问题`q`和文本`p`生成证据。如果使用`SM`策略，模型先生成句子索引，然后使用句子索引从文本`p`中定位到相应的证据。随后使用`Tanswer`模板提示模型根据生成的证据做出决策。此外，我们还考虑在决策阶段让模型同时基于生成的证据和文本做出决策。

## 3 实验分析

### 3.1 数据集

![](img/68a0ff4f851b3337747240f681b875dd.png)

表1 数据集统计

实验中使用的数据集统计结果如表1所示。`FEVER` 数据集旨在判断给定段落是否支持或反驳该说法。`MultiRC` 数据集旨在根据给定的段落为与答案相关的问题分配`True`或`False`。`BoolQ`数据集旨在根据给定的段落使用`True`或`False`标签回答问题。`Evi inf`数据集将(干预、结果、比较者)三元组连接到问题中，目的是根据给定的段落判断干预是否显著增加、减少或对结果没有影响。`Mov Rev`数据集旨在分析给定段落的情感为正面或负面标签，其中问题被统一设置为`这篇评论的情感是什么?`。此外，`Mov Rev`数据集提供短语粒度的注释证据，而其他数据集提供句子级别的注释证据。为保持一致，我们将短语粒度的注释转换为句子粒度的注释。

### 3.2 评价指标

我们使用`Accuracy`评估任务性能，并使用`Intersection-Over-Union F1`分数(`IOU F1`)，`Token F1` (`TF1`)和证据精度(`R-Acc`)评估生成的证据的质量。此外，我们引入了一个新的度量，推理成功商(`RSQ`)，以衡量生成的证据和决策之间可靠联系的程度。具体来说，我们将测试样本分为四类:

+   `rcdc`: 生成的证据和模型决策都正确的样本。

+   `rwdw`: 生成的证据和模型决策都错误的样本。

+   `rcdw`: 生成的证据是正确的，但模型决策是错误的。

+   `rwdc`: 生成的证据是错误的，但模型决策是正确的。

`RSQ`度量的计算方法如下。

![](img/c660ce5387268ada5b166dce6ed29a84.png)

基于`RSQ`度量，我们还提出了`RSQ-W`和`RSQ-C`度量来指导更详细的分析。`RSQ-W`衡量当模型提供正确的证据时，模型做出错误决策的比例。`RSQ-W`度量如下:

![](img/4de0a70fd1e1d21371570b398a31f902.png)

`RSQ-C`度量当模型提供错误的证据时，模型所做正确决策的比例。`RSQ-C`度量如下:

![](img/5ccc0a18f68c4840cce11767db396cb9.png)

### 3.3 主实验结果

在主实验中，我们将场景分为全监督场景和半监督场景。在全监督场景中，我们为每条训练样本提供人工注释的证据。在半监督场景中，我们和先前工作一致，仅为25%的训练样本提供人工注释的证据。

![](img/0f8827628a28cf68c0ce6ad4e2866324.png)

表2 全监督场景下的实验结果

**全监督场景。**如表2所示，实验结果显示了`SADM`框架的良好潜力。对于任务性能(`Perf.`)和证据质量(`IOU F1`, `TF1`和`R-Acc`)，我们的框架在五个数据集上显示了不同程度的改进。值得注意的是，我们的框架在`RSQ`度量中表现出了更显著的改进，这表明在生成的证据和决策之间存在更可靠的联系。

![](img/d6da1d2411b19e070e884b3ea0ec0a25.png)

表3 半监督场景下的实验结果

**半监督场景。**考虑到证据注释的昂贵成本，半监督的场景更有可能应用于现实世界。我们只使用25%的带有注释证据的训练数据。如表3所示，在`Mov Rev`数据集上，我们的`SADM`框架在任务性能和证据质量方面的性能低于`FID-Ex`框架，但在`RSQ`指标方面仍优于前者。在其他四个数据集上，我们观察到任务性能平均提高3.7分，`IOU F1`提高1.3分，`TF1`提高0.9分，`RSQ`指标提高4.2分。整体上看，我们的框架显示出更显著的优势。

### 3.4 消融实验结果

![](img/9195a8141795b018412b50426c019aee.png)

表4 消融实验结果

在消融实验中，我们评估了没有`SM`策略和`RAL`策略的`SADM`框架的性能。如表4所示，实验结果表明，无论去掉`SM`策略还是去掉`RAL`策略，`SADM`框架的性能均有一定程度的下降，这说明`SM`策略和`RAL`策略都起到了正向的作用。值得注意的是，我们特别验证了`RAL`策略对模型推理能力的影响。我们提出了以证据为中心的精度(`RCP`)度量，它关注为模型在决策阶段提供人工注释的证据时，模型可以做出正确决策的比例。实验结果表明，当去除`RAL`策略时，五个数据集的`RCP`度量平均下降1.3个点。这种现象凸显了`RAL`策略在提高模型决策阶段推理能力方面的重要意义。

## 4 讨论

### 4.1 先前研究（`Measuring Association Between Labels and Free-Text Rationales`）表明相比于`pipeline`框架，`parallel`框架（让模型同时生成证据和决策）的综合性能更好，这与我们工作的结论不符。

为了阐述该问题，我们首先说明和先前工作最大的区别在于实验场景的不同：自由文本证据场景vs提取证据场景：

对于自由文本证据场景，目标是让模型根据候选选项对问题做出正确的选择，并生成连贯的自由文本段落作为支持其决策的证据。对于提取证据场景，目标是让模型根据给定的文章提供问题的答案，同时从文章中提取句子作为证据以支持其决策。

先前的工作揭示了在自由文本证据场景中，`pipeline`框架面临以下挑战:

+   由于在`self-attribution`阶段产生的质量差的证据导致的级联错误。

+   `Pipeline`往往将参数的数量增加一倍，以达到与端到端(`I→OR`)框架相当的性能，但通常性能仍然更差。

然而，这些挑战在提取证据场景中并不存在。关于挑战1，如表2所示，我们的`SADM`框架确保了在`Self-Attribution`阶段生成证据的质量。我们将这种能力归因于在基于`T5`的模型上使用信息提取(`IE`)任务的可行性。然而，在自由文本证据场景中，使用`T5`生成自由文本仍然存在挑战。对于挑战2，这在我们提出的框架中得到了解决。如图2所示，我们的`pipeline`框架在训练的两个阶段之间共享参数。在不增加额外参数的情况下，我们的框架可以获得更好的性能。

因此，在不同的场景下，我们可以考虑基座模型的能力从而选择不同的架构。

### 4.2 ChatGPT在该任务上的性能如何？

在`rebuttal`阶段，我们补充了关于对`ChatGPT`的分析(`https://openreview.net/forum?id=xJ3O94DnMZ&noteId=LyQNj6XpjJ`)。实验结果表明，我们的模型在各种其他数据集和指标上优于`ChatGPT`。我们将此归因于两个关键因素:

+   我们的数据主要由冗长的文本组成，充满了复杂和嘈杂的信息。处理复杂的信息对任何语言模型都是一个挑战，包括`ChatGPT`，特别是没有针对我们的特定场景进行微调的语言模型。

+   我们的任务包括从文章中提取句子作为证据，类似于信息提取(`Information Extraction`, `IE`)任务。值得注意的是，有大量的工作[3][4]表明，`ChatGPT`在`IE`任务上的性能明显弱于精细调整的较小模型。因此，与`ChatGPT`相比，我们的模型的优越性能并不奇怪。

即便如此，我们也并不能认为`ChatGPT`的能力较弱。正如4.1讨论的，我们需要考虑基座模型本身的能力。`ChatGPT`的强大之处在于其生成能力而不是抽取，如果我们不严格控制`ChatGPT`从文本中提取证据，而是鼓励`ChatGPT`生成一段自由文本的证据，也许会展现一个不一样的结果。

## 参考文献

[1] Jay DeYoung, Sarthak Jain, Nazneen Fatema Rajani, Eric Lehman, Caiming Xiong, Richard Socher, and Byron C Wallace. 2019. Eraser: A benchmark to evaluate rationalized nlp models. arXiv preprint arXiv:1911.03429.

[2] Kushal Lakhotia, Bhargavi Paranjape, Asish Ghoshal, Wen-tau Yih, Yashar Mehdad, and Srinivasan Iyer. 2020. Fid-ex: Improving sequence-to-sequence models for extractive rationale generation. arXiv preprint arXiv:2012.15482.

[3] Jonathan St BT Evans. 1984. Heuristic and analytic processes in reasoning. British Journal of Psychology, 75(4):451–468.

[4] Gautier Izacard and Edouard Grave. 2020. Leveraging passage retrieval with generative models for open domain question answering. arXiv preprint arXiv:2007.01282.

[5] Sarah Wiegreffe, Ana Marasovic, and Noah A Smith. ´ 2020. Measuring association between labels and freetext rationales. arXiv preprint arXiv:2010.12762.

[6] Bhargavi Paranjape, Mandar Joshi, John Thickstun, Hannaneh Hajishirzi, and Luke Zettlemoyer. 2020. An information bottleneck approach for controlling conciseness in rationale extraction. arXiv preprint arXiv:2005.00652.

[7] Asish Ghoshal, Srinivasan Iyer, Bhargavi Paranjape, Kushal Lakhotia, Scott Wen-tau Yih, and Yashar Mehdad. 2022. Quaser: Question answering with scalable extractive rationalization. In Proceedings of the 45th International ACM SIGIR Conference on Research and Development in Information Retrieval, pages 1208–1218.

[8] James Thorne, Andreas Vlachos, Christos Christodoulopoulos, and Arpit Mittal. 2018. Fever: a large-scale dataset for fact extraction and verification. arXiv preprint arXiv:1803.05355.

[9] Daniel Khashabi, Snigdha Chaturvedi, Michael Roth, Shyam Upadhyay, and Dan Roth. 2018. Looking beyond the surface: A challenge set for reading comprehension over multiple sentences. In Proceedings of the 2018 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies, Volume 1 (Long Papers), pages 252–262.
