
# CVPR 24｜ETH Zurich等团队：重新定义小样本3D分割任务，新基准开启广阔提升潜力！

> 来源：`https://agijuejin.feishu.cn/docx/XzTTdq36koRQkLxNHAYcKA66n7c`
> 机器之心｜[阅读原文](https://www.jiqizhixin.com/articles/2024-06-24-8)
> 转载请联系原作者取得授权

该文章的第一作者安照崇，目前在哥本哈根大学攻读博士学位，导师为Serge Belongie。他硕士毕业于苏黎世联邦理工学院（ETH Zurich），在硕士期间，他在导师 Luc Van Gool 的实验室中参与了多个研究项目。他的主要研究方向包括场景理解、小样本学习以及多模态学习。

3D场景理解让人形机器人「看得见」周身场景，使汽车自动驾驶功能能够实时感知行驶过程中可能出现的情形，从而做出更加智能化的行为和反应。而这一切需要大量3D场景的详细标注，从而急剧提升时间成本和资源投入。

最近，ETH Zurich等团队提出了一种Few-shot学习方法，大大改善了这一局限性，并重新审视了目前的`FS-PCS`任务，在3D场景感知领域引入全新的benchmark，为未来的模型设计与开发开创了新局面。

![](img/ed24be4430b8f5ea5580960421ab9dda.png)

+ 论文链接: `https://arxiv.org/abs/2403.00592`
+ 代码链接: `https://github.com/ZhaochongAn/COSeg`

![](img/6585a205484d7903f42492e457f1f8ff.png)

3D Few-shot分割结果示例

## 技术背景

3D场景理解在自动驾驶、智能机器人等领域扮演着至关重要的角色，它使设备能够感知并理解周围的三维世界。尽管传统的全监督学习模型在特定类别的识别上表现出色，但这些模型通常只限于识别这些预定义的类别。这就意味着，每当需要识别新的对象类别时，就必须收集大量的3D场景数据并进行详细标注，这一过程不仅耗时耗力，还极大限制了全监督模型在真实世界中的应用广度和灵活性。

然而，借助Few-shot学习方法，这一局面得到了显著改善。Few-shot学习是一种需要极少标注样本就能迅速适应新类别的技术。这意味着模型可以通过少量的示例迅速学习和适应新的环境，大大降低了数据收集和处理的成本。这种快速、灵活的学习方式，使得3D场景理解技术更加适应快速变化的现实世界，为各种应用场景如自动驾驶和高级机器人系统打开了新的可能性。因此，研究Few-shot 3D模型能有效推动很多重要任务在更广阔世界的实际应用。

特别的，对于`FS-PCS`（Few-shot 3D point cloud semantic segmentation）任务，模型的输入包括support point cloud以及关于新类别的标注（support mask）和query point cloud。模型需要通过利用support point cloud和support mask获得关于新类别的知识并应用于分割query point cloud，预测出这些新类别的标签。在模型训练和测试时使用的目标类别无重合，以保证测试时使用的类均为新类，未被模型在训练时见过。

## 任务的重新审视与改正

![](img/b8b6299af020cb6e9357de70574df320.png)

图1. 两个场景的可视化（前景类分别为door和board）

![](img/614119415a1bf0ea741fe47c1fe316bd.png)

表1. 存在(w/FG)和不存在前景泄露(w/o FG)时过往模型的性能比较

该文章重新审视了当前`FS-PCS`任务。发现当前的任务setting具有两个显著的问题：

+ 第一个问题是前景泄漏：3D任务通常将场景点云中的密集点均匀采样后作为模型的输入。然而`FS-PCS`采用的采样方法并非均匀采样，而是会对目标类别（前景区域）采样更多的点，对非目标区域（背景区域）采样更少的点，这样得到的输入点云会在前景有更密集的点分布，导致了前景泄露问题。如图1所示，第四和第六列的输入点云来自于当前的有偏采样，在前景区域（door或board）展示出比背景更密集的点分布，而第三和第五列的输入使用改正后的一致性采样，展示出了均匀的点密度分布。该问题使得新类的信息被点云的密度分布所泄漏，从而允许模型简单的利用输入点云中的密度差异，预测更密集的区域为前景就可以实现良好的few-shot性能，而非依赖于学习从support到query的知识转移能力。因此当前的评价benchmark无法反映过往模型的真实性能。如表1所示，将当前setting中的前景泄露改正后，过往模型展示出了大的性能下降，表明了过往模型极大的依赖于密度的差异来实现看似优越的few-shot性能。

+ 第二个问题是稀疏点分布：当前的setting仅从场景中采样2048个点作为模型在训练和测试时的输入，这样稀疏的点分布严重限制了输入场景的语义清晰度。如图1所示，在第一行第五列中，人类肉眼都难以区分出区域中的语义类别door和周围的类别wall。对第二行也同样很困难来区分目标区域为board类或是其他的类别如window。这些稀疏的输入点云有非常受限的语义信息，引入了显著的歧义性，限制了模型有效挖掘场景中语义的能力。

因此，为了改正这些问题，作者提出了一个新的setting来标准化`FS-PCS`任务，采用均匀采样并增加采样点数10倍到20480点。如图1中第三列所示，新setting下的输入有一致性的点的分布和更清晰的语义信息，使得该任务更加贴近于真实的应用场景。

## 新的模型`COSeg`

在新改正的setting下，作者引入了一个新的模型叫做`Correlation Optimization Segmentation`（`COSeg`）。过往的方法都基于特征优化范式，侧重于优化support或者query的特征，并将改进后的特征输入到无参的预测模块获得预测结果，可看作隐式的建模support和query间的correlations。相反，没有注重于优化特征，文中提出了correlation优化范式，直接将support和query间的correlations输入到有参的模块中，显式的优化correlations，允许模型直接塑造query和support间的关系，增强了模型的泛化能力。

![](img/8fbf415cf856f1603351db2d7c65da9f.png)

图2. `COSeg`架构

在`COSeg`中，首先对每个query点计算与support prototypes间的Class-specific Multi-prototypical Correlation简称为`CMC`，表示每个点和所有类别prototypes之间的关系。随后将`CMC`输入到后续的`HCA`模块。

`HCA`模块利用两个潜在的关系来优化correlations。第一，query点都是互相关联的，因此他们对于类别prototypes间的correlations也是互相关联的，由此可得到点和点间的关系，相对应于`HCA`的前半部分对correlations在点维度做attention。第二，将一个query点分为前景或者背景类依赖于该点对于前景和背景prototypes之间的相对correlations，由此可得到前景和背景间的关系，相对应于`HCA`的后半部分对correlations在类别维度做attention。

此外，由于few-shot模型在base类别上做训练，在novel类别上做测试。这些模型会容易被测试场景中存在的熟悉的base类别干扰，影响对于novel类别的分割。为了解决该问题，文中提出对于base类别学习无参的prototypes（称为base prototypes）。当分割新类时，属于base类的query点应该被预测为背景。因此，利用base prototypes，作者在`HCA`层内部引入`BPC`模块来调整点和背景类别间的correlations，从而缓解base类带来的干扰。

## 实验结果

![](img/614771ed609165e7d40c2dc4440ef12e.png)

表2. 新的`FS-PCS` benchmark

![](img/def232e0d18ae8c6bf8009044f6fe8e8.png)

图3. `COSeg`和过往最佳方法的可视化比较

文中的实验首先在改正后的标准setting下评测了之前的方法，创立了标准的benchmark，并且证明了`COSeg`方法的优越性能，在各个few-shot任务中都实现了最佳的结果。可视化也清楚表明了`COSeg`实现了更好分割结果。此外，在文中作者也提供了广泛的消融实验证明了设计的有效性和correlation优化范式的优越性。

## 总结

该文章的研究在`FS-PCS`领域的贡献如下。

首先，作者确定了当前`FS-PCS` setting中的两个关键问题（前景泄露和稀疏点分布），这两个问题降低了对过往方法的评价基准的准确性。为了解决过往setting中的问题，文中引入了一个全新的标准化的setting以及评价benchmark。

此外，在标准化`FS-PCS` setting下，作者提出一个新的correlation优化范式，显著提高了模型在few-shot任务上的泛化性能。文中的模型`COSeg`融合了`HCA`来挖掘有效的点云关联信息和`BPC`来进行背景预测的调整，在所有few-shot任务上实现了最佳的性能。

文中改正的标准化setting开放了更多在Few-shot 3D分割任务上提升的可能性，同时提出的新correlation优化范式也为未来的模型设计与开发提供了新的方向。这项工作作为`FS-PCS`领域的一个新基准，有望激励更多研究者探索和拓展小样本3D场景理解的边界。

作为参考，以下几点可以作为潜在的研究方向，以进一步推动该领域的发展：

+ 在文中的新setting下，虽然`COSeg`实现了最佳性能，但仍然有很大的进步空间，可以改进模型以实现更优的few-shot泛化：如改进prototype的抽取方式[1,2]，改进correlation优化模块[3]，对每个few-shot任务做针对性的训练[4]。

+ 解决Base类别干扰问题也是影响Few-shot性能的关键因素，可以从训练或模型设计角度进行优化[5,6]，更好的减少Base类别的干扰。

+ 提高模型的训练和推理效率[7]，特别是在部署到实际应用时，模型的效率也是一个关键考量。

总结来说，这一领域的前景十分广阔，而且目前尚处于新兴起步阶段，对于广大的研究者而言，无疑是一个充满希望和机遇的研究领域。

## 参考链接

[1] Lang, Chunbo, et al. "Progressive parsing and commonality distillation for few-shot remote sensing segmentation." IEEE Transactions on Geoscience and Remote Sensing (2023).

[2] Liu, Yuanwei, et al. "Intermediate prototype mining transformer for few-shot semantic segmentation." Advances in Neural Information Processing Systems 35 (2022): 38020-38031.

[3] Zhang, Canyu, et al. "Few-shot 3d point cloud semantic segmentation via stratified class-specific attention based transformer network." Proceedings of the AAAI Conference on Artificial Intelligence. Vol. 37. No. 3. 2023.

[4] Boudiaf, Malik, et al. "Few-shot segmentation without meta-learning: A good transductive inference is all you need?." Proceedings of the IEEE/CVF conference on computer vision and pattern recognition. 2021.

[5] Wang, Jiahui, et al. "Few-shot point cloud semantic segmentation via contrastive self-supervision and multi-resolution attention." 2023 IEEE International Conference on Robotics and Automation (ICRA). IEEE, 2023.

[6] Lang, Chunbo, et al. "Learning what not to segment: A new perspective on few-shot segmentation." Proceedings of the IEEE/CVF conference on computer vision and pattern recognition. 2022.

[7] Sun, Yanpeng, et al. "Singular value fine-tuning: Few-shot segmentation requires few-parameters fine-tuning." Advances in Neural Information Processing Systems 35 (2022): 37484-37496.
