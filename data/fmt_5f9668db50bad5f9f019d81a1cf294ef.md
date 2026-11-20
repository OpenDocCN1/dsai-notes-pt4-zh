
# 基于流形的提示学习标签空间重嵌入方法

> 来源：[https://agijuejin.feishu.cn/docx/Sjw4di7Ikobo4xxyITbcyUKrn6c](https://agijuejin.feishu.cn/docx/Sjw4di7Ikobo4xxyITbcyUKrn6c)

> 哈工大SCIR｜[阅读原文](https://mp.weixin.qq.com/s/CorKmLzFrCFpe8lR2QMpOQ)

> 转载请联系原作者取得授权

> 论文名称：Manifold-Based Verbalizer Space Re-embedding for Tuning-Free Prompt-Based Classiﬁcation

> 论文作者：王昊淳，赵森栋，刘驰，席奴瓦，蔡沐祯，秦兵，刘挺

> 原创作者：王昊淳

> 论文链接：https://arxiv.org/pdf/2309.04174.pdf

> 转载须标注出处：哈工大SCIR

## 摘要

基于提示学习的微调通过将自然语言处理任务转换为完型填空任务，由此缓解预训练任务和微调阶段下游任务的差距，上述过程将任务输入、提示模板（prompt template）和`[MASK]`串联在一起，期待预训练模型在`[MASK]`位填入合适的回答，一般来说，需要预先在模型词汇表（vocabulary list）中选择一些token作为提示学习的标签（verbalizer），并通过标签将token映射到分类任务的类别，然而，当前针对提示学习标签的研究都需要对预训练模型本体或额外的可训练参数进行参数更新。同时，先前的最优方法采用欧式距离进行距离度量，而由于表示空间中潜在的非线性流形，上述方法存在一定局限性。因此，我们提出了一种名为类内邻域约束的局部线性嵌入（Locally Linear Embedding with Intra-class Neighborhood Constraint，`LLE-INC`）的无需参数更新的流形空间重嵌入方法，用于提示学习标签空间的重嵌入。实验结果表明，即使在不训练任何参数的情况下，`LLE-INC`也能与自动化提示学习标签相媲美，同时，基于`LLaMA-7B`、`LLaMA-13B`和`LLaMA-65B`的实验表明，`LLE-INC`对于无参数更新的大语言模型分类任务而言同样有效。

## 背景

![](img/5b37c60ffa606cf82556429eb41523af.png)

![](img/0de16851395a78875a6f0f2760976458.png)

图1. 空间重构示意图

![](img/f19339eac8e687a3535a2a3cb5c9ecdb.png)

## 方法设计

### 连续型标签嵌入表示

上一节介绍了基于提示学习的微调过程中的标签设计，其通常需要花费大量的精力来手动定义标签，受前人工作的启发，我们采用`[MASK]`符号串的嵌入表示作为输入的表示，并通过向量距离的度量对输入样例进行分类。

![](img/e0db4a2144fad3c8a6e3c21db5244af9.png)

由于向量间的欧氏距离并没有考虑到嵌入空间中的潜在流形，我们采用了一种基于流形的标签空间重嵌入方法来重构`[MASK]`的表征。

### 基于流形的空间重嵌入

![](img/03549c9fb611d0c3630bd4eb26f4dc99.png)

图2. 基于流行的提示学习标签空间重嵌入

我们基于流形的空间重嵌入方法如图2所示，我们从`[MASK]`符号串在预训练模型原始空间中的输出表示开始，通过以下步骤获得新的嵌入表示。步骤一：输入训练集中所有类别的训练样本，基于提示学习得到`[MASK]`符号串的向量表示；步骤二：通过同类邻居约束的局部线性嵌入方法拟合流形学习模型，并将步骤一中得到的`[MASK]`表示重新嵌入到新空间中；步骤三：用拟合得到的模型将测试样例中的`[MASK]`的向量表示嵌入新空间；步骤四：将测试样例的`[MASK]`重嵌入表示与训练集中的重嵌入表示用`k`-近邻算法完成分类。

![](img/b19cadd95da257b0ce4e00eff4ed5577.png)

![](img/363b76b20fbda4afcaa6f7864d17fd47.png)

![](img/3107c3b3b59086ee1033afe241f51f23.png)

![](img/6da7eb6bc54793710b3f7969f72ab86d.png)

### 参数更新

至此，在上述整个提示学习标签空间重嵌入的过程中，没有任何新参数，甚至是预训练模型中的参数更新，因而避免了计算资源的消耗以及微调过后的预训练模型的存储。此时的预训练模型被看作是一个知识库，同时，由于预训练模型并没有充分地获得与任务相关的信息，模型的输入表示只依赖于预训练过程，我们同时额外引入对比学习来更新预训练模型，通过将同类别样本构建为的正例样本对，将不同类样本构建为负例样本对，类似于[6]中的方法，作为标签空间重嵌入方法之前的插件模块，为预训练模型提供任务相关的知识。

![](img/9545d5c5dd851985a5d4c2160c7284a0.png)

## 实验

### 数据集

我们在十个多分类数据集上进行了实验，以证明我们的方法的有效性，数据集来源于中英文的通用域和生物医学领域。

### 实验结果

我们报告了基于不同随机种子的五个采样的少样本数据集中每个任务的平均准确率结果，以及相对应的标准差。下表显示了有参数更新和无参数更新的实验结果。

![](img/bde9862a0374e8f6976ece725b7598fa.png)

表1.主实验结果

#### 无参数更新

我们冻结了预训练模型中的参数，同时不训练任何额外的参数，在这种条件下，只有基线中的`ManualVerb`可以进行预测。无参数更新的`ManualVerb`和`LLE-INC`分别表示为`ManualVerb†`和`LLE-INC†`。表1显示，未经预训练模型微调的`ManualVerb†`性能较差，这意味着模型无法理解任务和手动设计的提示标签的含义。由于预训练参数被冻结，`ManualVerb†`在测试集上的性能在不同的随机种子下保持不变。另一方面，`LLE-INC†`比`ManualVerb†`更准确地预测标签，甚至指标与都需要参数更新`GenVerb`和`ProtoVerb`相当，因为`LLE-INC`的空间重构是基于类内邻居的约束关系。上述发现表明，来自冻结的预训练模型的输出向量中包含大量的隐含信息，基于流形的重新嵌入是一种利用这些信息的可行的方法。

#### 有参数更新

由于预训练模型可能不理解任务信息，我们可以首先使用对比学习来训练模型。然后，我们利用`LLE-INC`重新嵌入更新后的PTMs的输出表示，如图2所示。表1表明，`LLE-INC`在性能上一直优于`Fine-tuning`、`GenVerb`、`ProtoVerb`甚至`ManualVerb`。实验结果显示，通过基于流形的嵌入空间重建，预训练模型的潜力可以得到发挥，而`LLE-INC`的性能进一步表明，对预训练模型中的参数进行对比学习调整与空间重新嵌入更好地整合在一起。对于更难的多类任务，`LLE-INC`在没有人工重新嵌入过程的情况下表现出了优越性，可以为数据提供更好的表示。

#### 大语言模型的实验

![](img/4fdb939f213b709b2c658617db95d606.png)

表2.大语言模型实验

大语言模型的出现显著改变了自然语言处理领域，然而，由于参数规模的原因，大语言模型的微调是一个很大的挑战。为了应对这一挑战，我们将`LLE-INC`应用于`LLaMA`模型（`LLaMA-7B`、`LLaMA-13B`和`LLaMA-65B`）的无微调输出，以探索`LLE-INC`的性能。由于`LLaMA`主要是在英语语料库上训练的，因此我们只考虑英语语言任务。我们在表2中呈现的结果显示，没有任何参数调优的`LLaMA-LLE-INC†`能够比使用模型更新的`RoBERTa-large`以及未采用空间重嵌入的`LLaMA`模型获得更好的性能。这表明`LLE-INC`可以作为一种无参数微调的方法应用于使用大型语言模型的分类任务。

## 结论

基于提示学习的微调在少样本下已经取得了很不错的效果，最近，一些研究着眼于以向量形式的标签取代耗时费力的手动定义标签的做法，然而，高维表示空间内的流形有可能欺骗标签向量间的欧氏距离度量。在这项研究中，我们提出了带有类内邻居约束的局部线性嵌入方法（`LLE-INC`）来重嵌入提示学习标签的表示空间。根据实验结果，`LLE-INC`可在不更新任何参数的情况下得到良好的效果，并且可以通过使用基于对比学习更新过的预训练模型进一步提升性能表现，此外，流形学习的有效性也为更大规模模型的直接应用提供了新的可能性。

## 参考文献

[1] Schick, T.; and Sch¨utze, H. 2021a. Exploiting ClozeQuestions for Few-Shot Text Classiﬁcation and Natural Language Inference. In Proceedings of the 16th Conference of the European Chapter of the Association for Computational Linguistics: Main Volume, 255–269.

[2] Shin, T.; Razeghi, Y.; Logan IV, R. L.; Wallace, E.; and Singh, S. 2020. Eliciting Knowledge from Language Models Using Automatically Generated Prompts. In Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing (EMNLP), 4222–4235.

[3] Gao, T.; Fisch, A.; and Chen, D. 2021. Making Pre-trained Language Models Better Few-shot Learners. In Proceedings of the 59th Annual Meeting of the Association for Computational Linguistics and the 11th International Joint Conference on Natural Language Processing (Volume 1: Long Papers), 3816–3830. Online: Association for Computational Linguistics.

[4] Zhang, N.; Li, L.; Chen, X.; Deng, S.; Bi, Z.; Tan, C.; Huang, F.; and Chen, H. 2022b. Differentiable Prompt Makes Pretrained Language Models Better Few-shot Learners. In International Conference on Learning Representations.

[5] Hambardzumyan, K.; Khachatrian, H.; and May, J. 2021. WARP: Word-level Adversarial ReProgramming. In Proceedings of the 59th Annual Meeting of the Association for Computational Linguistics and the 11th International Joint Conference on Natural Language Processing (Volume 1: Long Papers), 4921–4933.

[6] Cui, G.; Hu, S.; Ding, N.; Huang, L.; and Liu, Z. 2022. Prototypical Verbalizer for Prompt-based Few-shot Tuning. In Proceedings of the 60th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), 70147024.

[7] Lee, J. 2010. Introduction to topological manifolds, volume 202. Springer Science & Business Media.

[8] Roweis, S. T.; and Saul, L. K. 2000. Nonlinear dimensionality reduction by locally linear embedding. science, 290(5500): 2323–2326.

[9] Saul, L. K.; and Roweis, S. T. 2000. An introduction to locally linear embedding. unpublished. Available at: http://www. cs. toronto. edu/˜roweis/lle/publications. html.

本期责任编辑：赵森栋

本期编辑：童彦澎
