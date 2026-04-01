# 基于知识桥接交互网络的因果情绪蕴含

> 来源：[https://agijuejin.feishu.cn/docx/DXmUdjWJbozjLixPucKcnlTUnVe](https://agijuejin.feishu.cn/docx/DXmUdjWJbozjLixPucKcnlTUnVe)

> 哈工大SCIR｜[阅读原文](https://mp.weixin.qq.com/s/BNceYMd_gpFlPLrvYT9dGg)

> 转载请联系原作者取得授权

> 论文名称：Knowledge-Bridged Causal interaction Network for Causal Emotion Entailment

> 论文作者：赵伟翔，赵妍妍，李卓君，秦兵

> 原创作者：赵伟翔

> 论文链接：https://arxiv.org/pdf/2212.02995.pdf

> 转载须标注出处：哈工大SCIR

# **1. 动机介绍**

对话中的情感分析已经成为自然语言处理（Natural Language Processing, NLP）界的一个新兴话题。大多数现有的工作主要集中在对话情绪识别上（Emotion Recognition in Conversations, ERC），其目的是预测对话中每个话语的情绪标签[1,2,3]。然而，情感推理任务，如识别对话中情绪背后的原因，还没有被充分研究。最近，Poria等人[4]认为，在对话中识别情绪原因（RECCON）有利于提高情绪分析模型的可解释性和性能。同时，它在一些领域有潜在的应用，如情绪支持系统[5]和共情对话系统[6]。因此，Poria等人[4]引入了一个名为RECCON的新任务，该任务有一个标注情绪原因的数据集。它包括两个不同的子任务：原因跨度抽取（Causal Span Extraction, CSE）和情绪原因蕴含（Causal Emotion Entailment, CEE）。在本文中，我们重点关注CEE子任务，其目标是预测对话历史中哪些特定的语句会引发目标语句中的非中性情绪。

在CEE任务中，有两个主要的挑战。首先，为了捕捉对话者之间相互交织的情感动态变化，有必要通过有效的语境模型来理解语境中的深层语义关联。其次，要准确地将候选语句推理到目标情感上可能很困难，因为因果线索并不总是在语境中明确提及，而是应该通过基于推理来暗示，这就导致了候选语句和目标语句之间存在推理空缺。然而，Poria等人[4]简单地将CEE表述为一个语句对分类问题，这是缺乏足够的对话语境模型和有效的情感原因推理的。因此，为了应对这样的两个挑战，我们将常识性知识（Commonsense Knowledge, CSK）[7]引入CEE。

![](img/8d11bc2621e95cd92f5eb1f150f0cc48.png)

图1: 数据集RECCON-DD示例

一方面，以事件为中心的CSK，对语句中提到的事件前后可能发生的事情进行揭示，可以被视为语义层面的桥梁（S-bridge），连接对话的发展，加强相关语句之间的语义依赖，从而深入理解对话的语境信息。如图1左侧所示，语句#7中PersonA想单独离开的事件发生在PersonA告诉PersonB离开的事件之后，这与语句#1和#5相关。

另一方面，根据Moors等人[8],人类的感觉和行动倾向是情绪的两个重要组成部分，并在很大程度上为目标情绪的产生提供了潜在的因果线索。为此，社会交互CSK被用作情绪层面的桥梁（E-bridge）和行动层面的桥梁（A-bridge），根据对话者的感觉和行动倾向所传达的因果线索，将候选语词与目标语词连接起来。在图1中，话语#4中PersonB的狡辩和批评使PersonA感到愤怒，这与目标语句#7所持有的情绪是一致的。此外，语句#5的内容暗示了PersonA的行动倾向是独处，它直接导致了她在目标语句#7中表达的内容。

在本文中，我们提出了 **知识桥接因果交互网络 **(Knowledge Bridged Causal Interaction Network, KBCIN)，以有效地进行语境建模和情绪原因推理。具体来说，我们将一个对话抽象为一个对话图，以建模对话中的语句间依赖关系。然后，我们引入了以事件为中心的CSKs，包括两种类型isAfter和isBefore，并设计了知识增强的图注意力模块（CSK-Enhanced Graph Attention），将CSKs作为S-bridge在图上进行消息传递。此外，为了填补候选语句和目标语句之间的推理空缺，我们利用社会交互CSKs，x(o)Want和x(o)React作为A-bridge和E-bridge。我们设计了情绪交互模块和行动交互模块，借助这两个bridge所传达的明确的因果线索，准确推理出目标情绪的原因。而上述三个模块构成了知识桥接的因果交互（KBCI）模块，它作为多个注意力头的并列，充分地建模了对话语句之间的相互依赖关系，并将目标情绪与候选语篇精确地联系起来。

为了评估所提出的模型的性能，我们在基准数据集[4]上进行了广泛的实验。我们与CEE、情绪原因抽取（ECE）和情感-原因对提取（ECPE）任务上的基线模型相比，取得了最先进的性能。

这项工作的主要贡献总结如下：

+   我们将常识性知识引入到因果情感实体化任务中，以填补候选语句和目标语句之间的推理空白。

+   我们提出了一个新的模型KBCIN，以常识性知识为桥梁，进行全面的对话语境建模和准确的情感原因推理。

+   在基准数据集上对比大多数强基线的实验结果证明了我们模型的优越性。

# **2. 模型方法**

![](img/bf559bb95b063e45d8f8a94187c76172.png)

图3: 整体模型架构图

## **2.1 特征提取**

![](img/1d0f040e3f0317cdedc1281053e73db9.png)

**知识获取 **。在这项工作中，我们使用ATOMIC- 2020[7]作为我们的常识性知识（CSK）基础。它是一个常识知识图，涵盖日常推理知识的社会、物理和时间相关方面。

![](img/a555084a56a2f17aab6a8b574824512c.png)

图2: 常识知识图事件中心CSK和社会交互CSK示例

为了充分理解对话中各语句之间的语义依赖关系，并填补候选语句与目标之间的推理空白，我们将CSK作为三座桥梁，分别命名为语义级桥梁（S-bridge）、情绪级桥梁（E-bridge）和动作级桥梁（A-bridge）。更具体地说，我们从ATOMIC-2020中探索了六种CSK，它们被归类为以事件为中心的CSK和社会交互CSK。图2中显示了CSK的例子。一方面，根据事件中心CSK isAfter和isBefore所体现的对话发展的脉络和因果关系，语句之间的深层语义依赖将由此来建立。因此，S-bridge的构建是为了对对话语境进行全面的了解。另一方面，另外两座桥，E-bridge和A-bridge是由社会交互CSK xReact, oReact, xWant和oWant构建的。而x(o)Want是对自身（他人）在事件发生后可能想做什么的描述，而x(o)React体现了自身（他人）在事件发生后的情绪感受。它们从人的感觉和行动倾向的角度出发，填补了候选语词和目标语词之间的推理空白。

![](img/28b0406c4fde7ca87473f3df6a3c629a.png)

## **2.2 并行的知识桥接因果互动**

受多头注意力机制的启发[11]，我们提出了并行的知识桥接因果交互块，其目的是为了充分理解对话语境，准确推理出目标语气中的非中性情绪的原因。对于每个模块，它由三个部分组成：CSK增强的图形注意模块，情感互动模块和行为互动模块。

CSK-增强的图注意力模块。我们没有把CEE定义为一个没有明确的语境交互建模的语句对分类问题，而是把对话中的语句抽象为一个对话图，其中当前的语句只与对话历史中的过去的语句相联系。通过这种方式，我们确保语句的互动符合因果关系的性质，即原因只能从过去推理出来。每个节点的表示都是由相应的语句级特征  初始化的。此外，我们计算目标语句和候选语句之间的相对距离，并利用相对位置信息来丰富语句的表示。由于每个语句的情绪标签被证明在CEE中起着重要作用[4]，我们也考虑到了这一点。因此，每个节点的最终表示是通过以下方式获得的：

![](img/687edad60159def5fda395d786b2e998.png)

![](img/d3e8f07a04ae8cec2b18b1cb6296f9c6.png)

![](img/bb796616c292c459cf70cc32f760abaa.png)

## **2.3 因果语句预测**

![](img/d93cb11893b0b10b4330416bea7f1b42.png)

# **3. 实验**

## **3.1 数据集**

![](img/dab8442e796beed244b9c55859ef2313.png)

表1: 经处理的RECCON-DD的统计数据

我们在基准数据集RECCON-DD上进行了实验。它是在数据集DailyDialog[13]的基础上，标注了情绪原因标签。我们只考虑对话历史中的原因，重复的因果对被删除。表1显示了经过处理的RECCON-DD的统计数据。

## **3.2 主实验结果及分析**

![](img/05f7fc14507a387c380c77e5862d5f01.png)

表2: 主实验结果

如表2所示，我们提出的模型在REECON-DD数据集上取得了最好的结果。由于RoBERTa-Base/Large的结果和ECPE的方法是在与我们相同的数据集规模下实现的，我们直接参考了Poria等人[4]的结果，我们在相同的环境下重新实现了KEC和ECE任务下的方法。受益于通过S-bridge进行的有效对话语境建模和通过E-bridge和A-bridge进行的准确情绪原因推理，KBCIN取得了最先进的Pos. F1和macro F1分数，分别为68.59和79.12。

## **3.3 消融实验**

![](img/c894b6bcd483dc68df515ea5f4843a9d.png)

表3: 消融实验结果

我们进行了消融实验，以验证我们模型中提出的不同模块的有效性。从表3的结果可以看出，三个bridge均对情绪原因的推理起到正向的增强作用。

## **3.4 情绪信息的影响**

![](img/ad610f483355afa4edb9d6e42389fc98.png)

表4: 情绪信息影响分析

为了进一步研究对话历史中每条语句的情绪信息的影响，我们要么删除情绪信息，要么用情绪识别模型预测的标签替换真实情感标签。结果展示在表4中。我们之所以用预测的标签来测试KBCIN的性能，是因为在实际应用的情况下，情绪识别是情绪原因提取的前置过程，这意味着在实际应用中的情绪原因抽取系统中，对话历史中语句的这种真实情感标签可能无法获得。使用预测的情绪标签导致的结果下降，提醒我们要尝试在对话中联合进行情绪识别和情绪原因提取，这样可以在两个任务之间共享相关的情绪信息，缓解两阶段使用情绪信息带来的错误级联传播问题。

## **3.5 知识桥接块的数量**

![](img/2d47cd4af57595d11617b5c71f604b80.png)

图4: 知识桥接块数量影响分析

由于KBCI是我们模型的创新和关键部分，用于有效的对话语境建模和准确的情绪原因推理，我们调整了不同数量的KBCI块来深入分析性能。结果显示在图4中。随着KBCI块的数量从1到5的增加，配有2个KBCI块的模型取得了最好的性能。

# **4. 结论**

在本文中，我们提出了知识桥接的因果交互网络（KBCIN），用于情绪原因的推理。常识知识（CSK）被用作三个桥梁来进行有效的对话语境建模和准确的情绪原因推理。具体来说，我们将对话抽象为一个对话图，并利用以事件为中心的CSK作为语义层面的桥梁（S-bridge），通过CSK增强的图注意力模块在图上进行消息传递，来增强深层次的语义间的依赖性。而社会交互CSK作为情绪级桥梁（E-bridge）和行动级桥梁（A-bridge），从人的感觉和行动倾向的角度为情绪交互模块和行动交互模块提供显示的因果线索，填补了候选语句与目标语句之间的推理空白。基准数据集的实验结果证明了我们提出的KBCIN的有效性。

# **参考文献**

[1] Dialoguernn: An attentive rnn for emotion detection in conversations. In Proceedings of the AAAI conference on artificial intelligence, volume 33, 6818–6825.

[2] COSMIC: COmmonSense knowledge for eMotion Identification in Conversations. In Findings of the Association for Computational Linguistics: EMNLP 2020, 2470–2481. Online: Association for Computational Linguistics.

[3] Directed Acyclic Graph Network for Conversational Emotion Recognition. In Proceedings of the 59th Annual Meeting of the As- sociation for Computational Linguistics and the 11th Inter- national Joint Conference on Natural Language Processing (Volume 1: Long Papers), 1551–1560. Online: Association for Computational Linguistics.

[4] Recognizing Emotion Cause in Conversations. Cogn. Comput., 13(5): 1317–1332.

[5] Control Globally, Understand Locally: A Global-to-Local Hierarchical Graph Network for Emotional Support Conversation.

[6] Improving Empathetic Response Generation by Recognizing Emotion Cause in Conversations. In Findings of the Association for Computational Linguistics: EMNLP 2021, 807–819. Punta Cana, Dominican Republic: Association for Computational Linguistics.

[7] Atomic 2020: On Symbolic and Neural Commonsense Knowledge Graphs. In Thirty-Fifth AAAI Conference on Artificial Intelligence, AAAI 2021, Thirty-Third Conference on Innovative Applications of Artificial Intelligence, IAAI 2021, The Eleventh Symposium on Educational Advances in Artificial Intelligence, EAAI 2021, Virtual Event, February 2-9, 2021, 6384–6392. AAAI Press.

[8] On the causal role of appraisal in emotion. Emotion Review Journal of the International Society for Research on Emotions, 5(2): 132–140.

[9] COMET: Commonsense Trans- formers for Automatic Knowledge Graph Construction. In Korhonen, A.; Traum, D. R.; and Ma`rquez, L., eds., Proceedings of the 57th Conference of the Association for Computational Linguistics, ACL 2019, Florence, Italy, July 28- August 2, 2019, Volume 1: Long Papers, 4762–4779. Association for Computational Linguistics.

[10] BART: Denoising Sequence-to-Sequence Pre-training for Natural Language Generation, Translation, and Comprehension. In Jurafsky, D.; Chai, J.; Schluter, N.; and Tetreault, J. R., eds., Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics, ACL 2020, On- line, July 5-10, 2020, 7871–7880. Association for Computational Linguistics.

[11] Attention is All you Need. In Guyon, I.; von Luxburg, U.; Bengio, S.; Wallach, H. M.; Fergus, R.; Vishwanathan, S. V. N.; and Garnett, R., eds., Advances in Neural Information Processing Systems 30: Annual Conference on Neural Information Processing Systems 2017, December 4-9, 2017, Long Beach, CA, USA, 5998–6008.

[12] Graph Attention Networks. In 6th International Conference on Learning Representations, ICLR 2018, Vancouver, BC, Canada, April 30 - May 3, 2018, Conference Track Proceedings. OpenReview.net.

[13] DailyDialog: A Manually Labelled Multi-turn Dialogue Dataset. In Kondrak, G.; and Watanabe, T., eds., Proceedings of the Eighth International Joint Conference on Natural Language Processing, IJCNLP 2017, Taipei, Tai- wan, November 27 - December 1, 2017 - Volume 1: Long Pa- pers, 986–995. Asian Federation of Natural Language Processing.

本期责任编辑：赵妍妍

本期编辑：杨 昕