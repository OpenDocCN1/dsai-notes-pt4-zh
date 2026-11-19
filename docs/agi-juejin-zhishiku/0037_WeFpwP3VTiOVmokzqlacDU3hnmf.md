
# ICML 2024| 大语言模型助力基于 CLIP 的分布外检测任务

> 来源：[https://agijuejin.feishu.cn/docx/OmKsd3YFKonIhLxlo8YcTGRKnOf](https://agijuejin.feishu.cn/docx/OmKsd3YFKonIhLxlo8YcTGRKnOf)

> 机器之心 新闻助手｜[阅读原文](https://www.jiqizhixin.com/articles/2024-07-01-16)

> 转载请联系原作者取得授权

当训练数据集和测试数据集的分布相同时，机器学习模型可以表现出优越的性能。然而在开放世界环境中，模型经常会遇到分布外（Out-of-Distribution, OOD，下称“OOD”）样本，OOD样本可能会导致模型做出不可预测的行为，而错误的后果可能是致命的，尤其是在自动驾驶等高风险场景中 [1, 2]。因此OOD检测对于保障机器学习模型在实际部署中的可靠性至关重要。

大多数OOD检测方法 [1, 3] 可以基于训练有素的分布内 (In-Distribution, ID) 分类器有效地检测 OOD 样本。然而，对于不同的ID数据集，它们需要重新训练分类器才能进行OOD检测。此外，这些方法仅依赖于视觉模式，而忽略了视觉图像与文本标签之间的联系。随着大规模视觉语言模型（Vision-Manguage Models , VLMs，例如CLIP [4]）的出现，使得零样本 OOD 检测成为了可能[5]。通过构建仅具有 ID 类别标签的文本分类器，能够实现在无需重新训练分类器的情况下跨不同的 ID 数据集检测 OOD 样本。

尽管现有的基于CLIP的分布外检测方法展现出了令人印象深刻的性能，但是它们在遇到较难检测的分布外样本时常常会检测失败，我们认为现有仅依赖ID 类别标签构建文本分类器的方法很大程度上限制了 CLIP 识别来自开放标签空间样本的固有能力。如图1 (a) 所示，仅依赖ID 类别标签构建文本分类器的方法难以区分较难检测的OOD样本（ID数据集：CUB-200-2011，OOD数据集：Places）。

![](img/37e819a01b6e09d2ffaa0c84bb660301.png)

图1. 研究动机示意图：(a) 仅依赖ID 类别标签构建文本分类器, (b) 使用真实OOD标签, (c) 使用LLM想象潜在的异常值暴露

在这项工作中，我们提出了一种名为Envisioning Outlier Exposure (EOE) 的分布外检测方法，该方法利用通过利用大型语言模型 (LLM) 的专家知识和推理能力来想象潜在的异常值暴露，从而提升VLMs的OOD检测性能 (如图1 (c) 所示)，同时无需访问任何实际的 OOD 数据。我们设计了 (1) 基于视觉相似性的 LLM 提示，以生成专门用于 OOD 检测的潜在异常值类标签，以及 (2) 基于潜在异常值惩罚的新评分函数，以有效区分难以识别的 OOD 样本。实验表明，EOE 在不同的 OOD 任务中实现了优越的性能，并且可以有效地扩展到 ImageNet-1K 数据集。

![](img/1d9a29c9d9fdd72f23ee6a75b0484776.png)

Ø论文链接：`https://arxiv.org/pdf/2406.00806`

Ø代码链接：`https://github.com/tmlr-group/EOE`

接下来将简要地向大家分享我们近期发表在 ICML 2024 上的分布外检测方向的研究结果。

## 预备知识

![](img/6ccd875c428b003516c8670e46cd3a1e.png)

## 方法介绍

EOE旨在通过利用 LLM 生成潜在的异常值类别标签来提高零样本 OOD 检测性能。然而，由于模型部署时遇到的OOD类别是未知的，那么，我们应该如何引导 LLM 生成所需的异常值类别标签？在获取异常值类别标签后，我们如何才能更好地区分 ID 和 OOD 样本？为了解决这些问题，我们提出了基于视觉相似性原则设计的专门针对 OOD 检测的 LLM 提示，并引入了一种新颖的评分函数来更好地区分ID/OOD样本。我们方法的总体框架如图 2所示。

![](img/dc166df87d449ba96b529f2a435563c9.png)

图2. EOE总体框架图

![](img/695183fce4352984af23f292c6ca3155.png)

Fine-grained OOD 检测也称为开放集识别，在Fine-grained OOD 检测中，ID 和 OOD 样本都属于同一主要类别(例如“鸟”类），并且子类之间存在内在的视觉相似性 (例如“麻雀”和“燕子”)。因此，指示 LLM 直接提供同一主要类别内的不同子类更为合适。

以上三种OOD 检测的 LLM 提示如图3所示

![](img/84d54618a22892f06a4056d66244459a.png)

图3. 基于视觉相似性原则设计的三类LLM提示

![](img/89628109551e601bf12cee35fab48e9b.png)

![](img/742bee5023463ca98874633d0de6fcf0.png)

图4. EOE 伪代码

## 我们的方法优点总结

EOE 不依赖于未知 OOD 数据的先验知识，因此特别适合开放世界场景。

零样本：同一个预训练模型可有效地应用于各种特定于任务的 ID 数据集，而无需对每个特定 ID 数据集进行单独训练。EOE 仅通过了解 ID 类标签即可实现卓越的 OOD 检测性能。

可扩展性和通用性：与同样生成潜在OOD 类标签的现有零样本 OOD 检测方法 [6]相比，EOE 可以轻松应用于 ImageNet-1K 等大规模数据集。此外，EOE 在不同任务中表现出通用性，包括Far, Near和Fine-grainedOOD 检测。

## 实验结果

我们在不同OOD任务的多个数据集上进行了实验。表1展示了在ImageNet-1K 上进行Far OOD 检测实验结果，其中，Ground Truth表示使用真实OOD标签时的性能，在实际部署中是不可获取的。结果表明，EOE 与微调方法相当，并超越了 MCM [5]。

表1. Far OOD实验结果

![](img/a3103f288032338269864f6a2e8c3091.png)

我们也报告了在Near OOD和Fine-grained OOD任务上的实验结果，如表2和表3所示，我们的方法均实现了最佳的检测性能。

表2. Near OOD实验结果

![](img/2a0cc1b12095aef141ce4a273b700f41.png)

表3. Fine-grained OOD实验结果

![](img/246727a5b3c5a713ac1c92ffdfd25937.png)

我们对EOE的各个模型进行了消融实验，包括不同的得分函数，LLM提示：不同的LLM和不同长度的潜在OOD 类标签数量。实验表明，我们设计的得分函数和基于视觉相似性原则设计的LLM prompt获得了最优的性能，并且我们的方法在不同的LLM和不同长度的潜在OOD 类标签数量上都获得了优异的表现。同时，我们也对视觉语言模型的结构进行了消融实验，详细的实验结果请大家参考原文。

![](img/ba8a0ed2b69eec1a0a7739c8148f031f.png)

图5. 消融实验 –不同的得分函数、LLM提示和不同的LLM

![](img/58568373a20cda3534f6a8dcdb5bc380.png)

图5. 消融实验 –生成潜在OOD 类标签数量

我们对EOE的有效性进行了分析，实际上，生成的异常类标签不太可能有很高的概率命中真实值 OOD 类。这是因为在模型的实际部署中遇到的 OOD 数据是多种多样，且不可预测的。然而，通过视觉相似性规则引导，即使没有命中真实的OOD类的情况下，EOE生成的潜在异常类标签仍然可以提高模型在 OOD 检测中的表现。

为了说明以上论点，我们展示了通过 T-SNE从标签匹配分数的 `softmax` 输出中得出的可视化效果。我们的 EOE 与对比方法 MCM 之间的可视化结果如图6所示。基于 ImageNet-10 的 ID 类标签，LLM 生成基于视觉相似性规则的潜在异常标签“潜艇”。当遇到 OOD 类“蒸汽机车”（ImageNet-20 中的类）时，“蒸汽机车”在和中与“潜艇”的相似度最高。因此，EOE 会将其聚类为“潜艇”，从而将其检测为 OOD 类。然而，如果没有潜在的异常类标签，我们可以发现 MCM 倾向于将所有 OOD 类标签聚类在一起。这可能导致将难以识别的 OOD 样本识别为 ID 类。总之，在我们的 EOE 框架中，1) 属于同一类的 OOD 样本倾向于被聚集在一起，2) 同一组的样本被归类到与它们在视觉上相似的设想的异常值类中（“蒸汽机车” vs “潜艇”）。这些观察结果表明，我们的 EOE 可以在不触及实际 OOD 类别的情况下增强 OOD 检测，并且在语义上也更易于解释。我们希望这项工作能为 OOD 检测领域的未来研究提供一个新的思路。

![](img/46279c5cd9e19645492f42abdccc8c48.png)

图6. 可视化结果

> 参考文献

> [1] Hendrycks, D. and Gimpel, K. A baseline for detecting misclassified and out-of-distribution examples in neural networks. In ICLR, 2017.

> [2] Yang, J., Zhou, K., Li, Y., and Liu, Z. Generalized out-of-distribution detection: A survey. arXiv preprint arXiv:2110.11334, 2021.

> [3] Liu, W., Wang, X., Owens, J., and Li, Y. Energy-based out-of-distribution detection. In NeurIPS, 2020.

> [4] Radford, A., Kim, J. W., Hallacy, C., Ramesh, A., Goh, G., Agarwal, S., Sastry, G., Askell, A., Mishkin, P., Clark, J., et al. Learning transferable visual models from natural language supervision. In ICML, 2021.

> [5] Ming, Y., Cai, Z., Gu, J., Sun, Y., Li, W., and Li, Y. Delving into out-of-distribution detection with vision-language representations. In NeurIPS, 2022.

> [6] Esmaeilpour, S., Liu, B., Robertson, E., and Shu, L. Zeroshot out-of-distribution detection based on the pre-trained model clip. In AAAI, 2022.

## 课题组介绍

香港浸会大学可信机器学习和推理课题组 (TMLR Group) 由多名青年教授、博士后研究员、博士生、访问博士生和研究助理共同组成，课题组隶属于理学院计算机系。课题组专攻可信表征学习、基于因果推理的可信学习、可信基础模型等相关的算法，理论和系统设计以及在自然科学上的应用，具体研究方向和相关成果详见本组Github (`https://github.com/tmlr-group`)。课题组由政府科研基金以及工业界科研基金资助，如香港研究资助局杰出青年学者计划，国家自然科学基金面上项目和青年项目，以及微软、英伟达、百度、阿里、腾讯等企业的科研基金。青年教授和资深研究员手把手带，GPU计算资源充足，长期招收多名博士后研究员、博士生、研究助理和研究实习生。此外，本组也欢迎自费的访问博士后研究员、博士生和研究助理申请，访问至少3-6个月，支持远程访问。有兴趣的同学请发送个人简历和初步研究计划到邮箱 (`bhanml@comp.hkbu.edu.hk`)。
