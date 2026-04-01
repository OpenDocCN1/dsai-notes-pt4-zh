# 取代昂贵量子方法，南科大AI方法实现蛋白质-药物系统多尺度量子「精炼」

> 来源：[https://agijuejin.feishu.cn/docx/ZXU1dvvrFo7Q6TxSZvYcIgSnnmg](https://agijuejin.feishu.cn/docx/ZXU1dvvrFo7Q6TxSZvYcIgSnnmg)

> 机器之心 ScienceAI｜[阅读原文](https://www.jiqizhixin.com/articles/2024-06-11-21)

> 转载请联系原作者取得授权

![](img/8f792ba3b57d680c5c2b09215e1c4760.png)

编辑 | 萝卜皮

生物大分子结构对于药物开发和生物催化至关重要。量子「精炼」（Quantum refinement，QR）方法采用可靠的量子力学（QM）方法进行晶体细化，在提高结构质量甚至纠正生物大分子的结构方面显示出前景。

然而，巨大的计算成本和复杂的量子力学/分子力学（QM/MM）设置限制了 QR 的应用。

在这里，南方科技大学的研究团队将稳健的机器学习势（Machine Learning Potentials，MLP）融入多尺度 ONIOM（QM:MM）方案中，来描述核心部分（例如药物/抑制剂），取代昂贵的 QM 方法。

首次组合了两个级别的 MLP，从而克服 MLP 的限制。

该团队独特的基于 MLP+ONIOM 的 QR 方法，实现了 QM 级精度，并且效率显著提高。

此外，该研究提供了计算证据，证明美国食品和药物管理局（FDA）批准的药物 nirmatrelvir 在一种 SARS-CoV-2 主要蛋白酶结构中存在键合和非键合形式。

这项研究强调，强大的 MLP 可以加速可靠的蛋白质-药物复合物的 QR，促进更广泛的 QR 应用，并为药物开发提供更多原子见解。

该研究以「 Accelerating reliable multiscale quantum refinement of protein–drug systems enabled by machine learning 」为题，于 2024 年 5 月 16 日发布在《 Nature Communications 》。

![](img/8a5ae69d711e0bac581c089de046fd2e.png)

在药物开发和生物催化领域，生物大分子结构至关重要，它影响着分子性质预测、配体结合位点识别及催化机制的理解。

X 射线衍射是解析这些结构的强有力工具，而量子细化方法通过引入可靠的量子力学手段改进晶体结构细化过程，展现出提高结构质量和修正错误结构的潜力。

尽管如此，高昂的计算成本和复杂的量子力学/分子力学（QM/MM）设置限制了其广泛应用。

南方科技大学的研究团队提出了一种创新策略，利用稳健的机器学习势（MLPs）集成到多尺度ONIOM（QM：MM）模型中，以高效且准确地描述药物/抑制剂等核心部分，取代昂贵的量子力学计算。

通过结合两种级别的 MLPs，该方法不仅加速了蛋白质-药物复合物的量子细化过程，还为药物研发提供了原子层面的新见解，并促进了更广泛的量子细化应用。

![](img/d7c171c9eed8e270b8c3ef1a7da5b470.png)

图示：工作流程示意图和选定的蛋白质-药物/抑制剂系统。（来源：论文）

在这项研究中，提出并利用机器学习势（MLP）来代替可靠但昂贵的 QM 方法，并首次加速多尺度 QR 过程。

为了克服一些 MLP 中的元素限制，两个不同级别的 MLP 还通过外推 ONIOM 方法组合，然后作为前所未有的 ONIOM3（MLP1:MLP2:MM）和 ONIOM4（MLP1:MLP2:SE:MM）方案应用于 QR。

![](img/476ca23e6b1adb3eb787dccbd6aff7c0.png)

图示：该团队 N 层集成分子轨道和分子力学 (ONIOM) 方案，具有机器学习势 (MLP) 和电子密度图。（来源：论文）

这里的 QR 结果表明，MLP（尤其是达到耦合簇精度的高水平 AIQM1 方法）可以在 50 种不同的蛋白质-药物/抑制剂系统中实现高度准确的药物/抑制剂结构，与可靠的 DFT 方法的结果相当，计算成本降低大约两个数量级。

![](img/abeaabbea495d5531975d652c73772b9.png)

图示：不同 QR 方案的准确性。（来源：论文）

此外，他们的 QR 结果提供了计算证据，表明 FDA 批准的药物 nirmatrelvir 的键合形式和非键合形式共存于 SARS-CoV-2 MPro 的一种晶体结构中，这将为治疗 SARS-CoV-2 的药物设计提供新的结构见解。

![](img/226816c0ed80f27f67a2cb561ef02735.png)

图示：SARS-CoV-2 MPro 的量子改进。（来源：论文）

这里的概念验证研究表明，强大的 MLP 可以高精度地加速蛋白质-药物/抑制剂复合物的 QR，这应该会促进更多 QR 应用，并为分子识别、催化和药物开发提供新的原子学见解。此外，除了 X 射线晶体学之外，研究人员相信 MLP 可能有助于生物大分子的现代结构测定方法（例如 Cryo-EM、MicroED）。

此外，还建立了计算基准数据集（PB20-QM、PB20-QM-8 k 和 PB20-QM-3 k）来评估由 DFT、MLP 或计算得出的 3-19 k 药物/抑制剂分子的结构可靠性。

SE 方法，希望有助于未来开发更好的药物/抑制剂分子的 DFT、MLP 和/或 SE 方法。研究人员也乐观地认为，随着不同团队开发的更高水平和通用 MLP 的进步，未来各种生物系统的快速可靠的 QR 将在任何普通台式计算机上常规执行。

论文链接： https://www.nature.com/articles/s41467-024-48453-4