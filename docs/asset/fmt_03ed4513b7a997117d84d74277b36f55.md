
# 突破传统缺陷检测的界限，"Defect Spectrum"首次实现超高精度丰富语义的工业缺陷检测

> 来源：[https://agijuejin.feishu.cn/docx/FfU4deDftoDvayxWZlycQsC2ngS](https://agijuejin.feishu.cn/docx/FfU4deDftoDvayxWZlycQsC2ngS)

> 机器之心｜[阅读原文](https://www.jiqizhixin.com/articles/2024-07-26-9)

> 转载请联系原作者取得授权

在现代制造业中，精准的缺陷检测不仅是保证产品质量的关键，更是提升生产效率的核心。然而，现有的缺陷检测数据集常常缺乏实际应用所需的精确度和语义丰富性，导致模型无法识别具体的缺陷类别或位置。

为了解决这一难题，由香港科技大学广州和思谋科技组成的顶尖研究团队，创新性地开发出了“Defect Spectrum”数据集，为工业缺陷提供了详尽、语义丰富的大规模标注。如表一所示，相比其他工业数据集，“Defect Spectrum”数据集提供了最多的缺陷标注（5438张缺陷样本），最细致的缺陷分类（125种缺陷类别），并为不同种类的缺陷都提供了像素级的细致标签。此外，该数据集还为每一个缺陷样本提供了精细的语言描述。具体的标注对比如图一所示。

![](img/43ebe96841ba48a3f96fb3ae96bc7e2a.png)

图一：相比其他工业数据集，Defect Spectrum精准度更高，标注更丰富

![](img/f777419045f1f6bd9e93189c19d11fad.png)

表一：Defect Spectrum与其他现有数据集的数量，性质对比

“Defect Spectrum”基于最先进的扩散模型，推出了一种革命性的方法——“DefectGen”。通过利用极少量的工业缺陷数据生成图像与像素级缺陷标签，该方法显著提升了工业缺陷检测模型的性能，在多个行业标准数据集上（如MVTec AD、VISION、DAGM2007及Cotton-Fabric）实现了前所未有的性能突破。

这一突破性的研究不仅极大地提高了缺陷检测的准确性，更为AI在复杂工业环境中的应用开辟了新的可能性。该项目的代码和模型已经全面开源。

![](img/382813cc41dfbcfe5d0a1faa700b9b13.png)

+ 论文：Defect Spectrum: A Granular Look of Large-Scale Defect Datasets with Rich Semantics

+ 论文链接： https://arxiv.org/abs/2310.17316

+ 项目链接: https://envision-research.github.io/Defect_Spectrum/

+ Github Repo: https://github.com/EnVision-Research/Defect_Spectrum

+ Dataset Repo: https://huggingface.co/datasets/DefectSpectrum/Defect_Spectrum

## 突破传统的缺陷检测限制，更加贴近落地生产

![](img/4c88d9e8474a615d700fc1e8fbb36336.png)

图二：实际工业生产，缺陷检测以及分析的闭环

在实际的工业生产中，我们对缺陷检测的要求更加细致，工厂需要在控制缺陷件的同时保证收益率，如图二所示。然而，现有的缺陷检测数据集常常缺乏实际应用所需的精确度和语义丰富性，例如一块金属板表面如果有较大面积的油漆剥落，尽管缺陷面积很大，但对金属板的功能影响可能微乎其微。然而，如果金属板内部有一条细小的裂缝，这条裂缝虽小如发丝，却可能在承受压力时导致金属板瞬间断裂，显著影响其性能，甚至引发严重的安全隐患。

更好比说，假设一件衣服的拉链齿出现了错位，这种缺陷虽然看起来尺寸不大，甚至不容易被发现，但却严重影响了衣物的功能，导致拉链无法正常使用，消费者不得不将其退回工厂进行修复。然而，如果缺陷发生在衣物的面料上，比如轻微的钩丝或颜色略有差异，这时就需要仔细权衡其尺寸和影响。小规模的面料缺陷可以被归类在可接受的范围内，允许这些产品通过不同的分销策略销售，比如以打折价格进行销售，从而保持产品流通而不影响整体质量标准。

在这一切背后，"Defect Spectrum"数据集就像一个全能的侦探，洞察一切。它不仅覆盖了广泛的工业缺陷类型，还为每一种缺陷提供了详尽而丰富的描述。通过这个强大的工具，缺陷检测系统能够更加精确地识别和分类各种缺陷，不放过任何一个细节。

想象一下，在实际的生产线上，通过“Defect Spectrum”数据集的帮助，检测系统能够迅速识别出这个至关重要的缺陷，立即标记并返回工厂进行修复。同时，对于那些面料上轻微的缺陷或颜色差异，系统可以根据缺陷的详细标注，判断其是否在可接受范围内，决定是否以打折价格销售。这种灵活的处理方式，不仅提高了产品的质量，还保证了生产的高效和成本的控制。

传统数据集如MVTEC和AeBAD尽管提供了像素级的标注，但常常局限于二元遮罩，无法细致区分缺陷类型和位置。《Defect Spectrum》数据集通过与工业界四大基准的合作，重新评估并精细化已有的缺陷标注。例如，对细微的划痕和凹坑进行了更精确的轮廓绘制，且通过专家的辅助填补了遗漏的缺陷，确保了标注的全面性和精确性。

## 创新的缺陷生成模型“Defect-Gen”

![](img/079e24425d0e1ae458f36eb0f33c769d.png)

图三：Defect-Gen两阶段生成流程示意图

面对当前数据集中缺陷样本不足的问题，我们提出了“Defect-Gen”，一个两阶段的扩散式生成器。这个生成器在样本数量有限的情况下，通过两个关键方法提高了图像的多样性和质量：第一，使用Patch级建模；第二，限制感受野。

传统的扩散模型在训练样本少时，容易过拟合，生成的结果缺乏多样性，往往只是记住了训练样本。而我们的模型通过降低数据维度和增加样本量，有效地减少了这种过拟合现象。

为了弥补Patch级建模在表达整个图像结构上的不足，我们提出了两阶段的扩散过程。首先，在早期步骤中使用大感受野模型捕捉几何结构，然后在后续步骤中切换到小感受野模型生成局部Patch。这样做在保持图像质量的同时，显著提升了生成的多样性。通过调整两个模型的接入点和感受野，我们的模型在保真度和多样性之间实现了良好的平衡。

通过“Defect-Gen”，我们为工业缺陷检测提供了更丰富和多样的训练样本，推动了自动化检测技术的发展

## 全面评估与未来的研究方向

![](img/9a9a6d56e575281f52724ec63e55643a.png)

表二：部分缺陷检测网络在Defect Spectrum数据集上的测评结果

![](img/f56893e108d910f18d0245534e334ffe.png)

表三：Defect Spectrum数据集上的实际评估标准

![](img/743c9602ef43724329878f5ac6354bd1.png)

表四：Defect Spectrum在实际评估中的优异表现

我们对Defect Spectrum数据集进行了全面的评估，标注如表三所示。这个实验验证了Defect Spectrum在各种工业缺陷检测挑战中的适用性和优越性。表四表明，比起原有的数据集，在我们数据集上训练的模型提升了10.74%的召回率(recall)，降低了33.1%的过杀率(False Positive Rate)。此外，数据集的构建和评估过程不仅提供了一个坚实的研究基础，也为工业界和学术界的研究人员提供了一个评估和开发针对工业缺陷检测复杂需求的先进模型的平台。

Defect Spectrum数据集的引入，犹如为工业生产注入了一剂强心针。它让缺陷检测系统更加贴近实际生产需求，实现了高效、精准的缺陷管理。同时，它也为未来的预测性维护提供了宝贵的数据支持，通过记录每个缺陷的类别和位置，工厂可以不断优化生产流程，改进产品修复方法，最终实现更高的生产效益和产品质量。

## 总结

我们发布了Defect Spectrum数据集以及DefectGen缺陷生成器，提供了实际工业检测中所需的高精确度和丰富缺陷语义，解决了模型无法识别缺陷类别或位置的问题。

我们对Defect Spectrum数据集进行了全面的评估，验证了其在各种工业缺陷检测挑战中的适用性和优越性，比起原有的数据集，在我们数据集上训练的模型提升了10.74%的召回率(recall)，降低了33.1%的过杀率(False Positive Rate)。

## 参考资料：

1. Bai, H., Mou, S., Likhomanenko, T., Cinbis, R.G., Tuzel, O., Huang, P., Shan, J., Shi, J., Cao, M.: Vision datasets: A benchmark for vision-based industrial inspec- tion. arXiv preprint arXiv:2306.07890 (2023)

2. Silvestre-Blanes, J., Albero-Albero, T., Miralles, I., Pérez-Llorens, R., Moreno, J.: A public fabric database for defect detection methods and results. Autex Research Journal19(4), 363–374 (2019). https://doi.org/doi:10.2478/aut-2019-0035,https://doi.org/10.2478/aut-2019-0035

3. Zhang, Z., Zhao, Z., Zhang, X., Sun, C., Chen, X.: Industrial anomaly detection with domain shift: A real-world dataset and masked multi-scale reconstruction. arXiv preprint arXiv:2304.02216 (2023)

4. Mishra, P., Verk, R., Fornasier, D., Piciarelli, C., Foresti, G.L.: VT-ADL: A vision transformer network for image anomaly detection and localization. In: 30th IEEE/IES International Symposium on Industrial Electronics (ISIE) (June 2021)

5. Incorporated, C.: Standard fabric defect glossary (2023), uRL: https : / / www . cottoninc . com / quality - products / textile - resources / fabric - defect - glossary

6. Wieler, M., Hahn, T.: Weakly supervised learning for industrial optical inspection. In: DAGM symposium in. vol. 6 (2007)

7. Tabernik, D., Šela, S., Skvarč, J., Skočaj, D.: Segmentation-based deep-learning approach for surface-defect detection. Journal of Intelligent Manufacturing31(3), 759–776 (2020)

8. Bergmann, P., Fauser, M., Sattlegger, D., Steger, C.: Mvtec ad–a comprehen- sive real-world dataset for unsupervised anomaly detection. In: Proceedings of the IEEE/CVF conference on computer vision and pattern recognition. pp. 9592–9600 (2019)

9. Zou, Y., Jeong, J., Pemula, L., Zhang, D., Dabeer, O.: Spot-the-difference self- supervised pre-training for anomaly detection and segmentation (2022)
