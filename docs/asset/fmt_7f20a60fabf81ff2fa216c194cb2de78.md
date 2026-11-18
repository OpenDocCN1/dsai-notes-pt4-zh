
# 沐曦论文分类打榜赛

> 来源：[https://aicarrier.feishu.cn/docx/SR6KdeGY9ox5W5xpqywcBsSknqg](https://aicarrier.feishu.cn/docx/SR6KdeGY9ox5W5xpqywcBsSknqg)

![](img/c5b88aac63c20093cd08df780baf55a3.png)

书生大模型实战营（沐曦魔乐专场）—— 论文分类打榜赛

**在参赛前请先查看：**[闯关手册——书生大模型实战营（沐曦+魔乐）专场](https://aicarrier.feishu.cn/wiki/ARcxwLXfpiugmlkAxFOcjiEHnBh)

**温馨提示：一定要记得关开发机，一定要记得关开发机**，一定要记得关开发机，不然钱会用完！！！

**温馨提示：**

本赛事必须使用 `InternLM 1.8B` 模型

**本赛事飞书提交的权重必须由沐曦算力训练而来（`d.run` 和 `OpenI` 等平台都可）**

## 背景

在人工智能大语言模型蓬勃发展的时代背景下，书生·浦语（`InternLM`）凭借其卓越的知识理解、文本分析、数学推理和多语言处理能力，在各类评测任务中表现优异。该模型在综合性考试中成绩突出，在多项中文基准测试中超越了同期主流模型的表现。随着版本的持续迭代，`InternLM`在数理计算、代码生成、对话交互、内容创作等领域均实现了显著提升，整体性能位居同参数量级开源模型的前列。

本次书生·浦语端侧小模型`1.8B`论文分类微调竞赛旨在深入探索`InternLM`在垂直领域应用的巨大潜力。竞赛依托魔乐社区（`Modelers`）平台举办，魔乐社区是天翼云与华为联合推出的AI开发者社区，提供`TDMA`（工具链、数据集、模型、应用）的托管展示服务和支撑系统。魔乐社区汇聚AI产业链资源，依托理事会成员单位，吸引开发者共同推动AI发展，解决行业难题，促进生态繁荣。社区提供免费算力，方便用户体验AI模型和应用效果。魔乐社区的核心功能涵盖模型托管与管理、数据集托管、体验空间、`Git`仓库服务，以及`openMind Library`和`openMind Hub Client`等工具套件集成，为模型开发和管理提供了完善的技术支撑。

在算力支持方面，本次竞赛得到了沐曦的强力支撑。沐曦致力于为异构计算提供全栈`GPU`芯片及解决方案，可广泛应用于智算、智慧城市、云计算、自动驾驶、数字孪生、元宇宙等前沿领域，为数字经济发展提供强大的算力支撑。沐曦先进的`GPU`计算架构为参赛者提供了高效稳定的训练环境，确保模型微调过程的顺利进行。

竞赛采用学术资源丰富的`arXiv`数据集作为训练基础，该数据集汇聚了物理学、数学、计算机科学、定量生物学、定量金融等多个前沿学科的海量论文资源。`arXiv`作为全球领先的预印本论文发布平台，以其快速的论文更新频率和前沿的研究内容，为模型训练提供了高质量的学术数据支撑。同时，竞赛还整合了额外采集的学术论文数据，这些数据来源多样化，与原始`arXiv`数据集形成有效互补，进一步增强了训练数据的丰富性和覆盖面，有助于提升模型对不同研究风格和主题论文的精准分类能力。

竞赛设置了26个细分学科类别，具体映射关系如下：

`quant-ph`（量子物理）→ `A`，`physics.chem-ph`（化学物理）→ `B`，`physics.atom-ph`（原子物理）→ `C`，`cond-mat.soft`（软凝聚态物理）→ `D`，`cs.RO`（机器人学）→ `E`，`cs.CL`（计算语言学）→ `F`，`cs.SE`（软件工程）→ `G`，`cs.IR`（信息检索）→ `H`，`hep-th`（高能理论物理）→ `I`，`hep-ph`（高能现象学）→ `J`，`physics.optics`（光学）→ `K`，`cs.AI`（人工智能）→ `L`，`cs.CV`（计算机视觉）→ `M`，`nucl-th`（核理论）→ `N`，`astro-ph`（天体物理）→ `O`，`math.PR`（概率论）→ `P`，`cs.OS`（操作系统）→ `Q`，`eess.SP`（信号处理）→ `R`，`math.OC`（最优化与控制）→ `S`，`math.DS`（动力系统）→ `T`，`math.DG`（微分几何）→ `U`，`math.MP`（数学物理）→ `V`，`cs.MM`（多媒体）→ `W`，`stat.ME`（统计方法）→ `X`，`math.CO`（组合数学）→ `Y`，`cs.NE`（神经与进化计算）→ `Z`。

为确保竞赛的科学性和模型泛化能力的真实性，避免参赛者过度拟合训练数据而影响模型的实际应用价值，本次竞赛的测试集采用了20种不同的问题模板进行评估。现公开其中一种标准模板格式：

```
&quot;Based on the title &#x27;{title}&#x27;, authors &#x27;{authors}&#x27;, and abstract &#x27;{summary}&#x27;, please determine the scientific category of this paper.&quot;
```

在技术实施层面，所有参赛者必须基于`InternLM 1.8B`模型进行微调训练。参赛者需要根据竞赛提供的数据集特征，结合自身的技术储备和对模型架构的深度理解，并严格遵循相关的技术标准和学术伦理规范，确保最终模型在论文分类任务中具备良好的稳定性、准确性和可靠性。

## 数据集介绍

https://www.kaggle.com/datasets/Cornell-University/arxiv

本比赛选取arXiv数据集作为核心数据来源，通过系统性数据分析确定了26个重要学术类别，包括`quant-ph`、`physics.chem-ph`、`physics.atom-ph`、`cond-mat.soft`、`cs.RO`、`cs.CL`、`cs.SE`、`cs.IR`、`hep-th`、`hep-ph`、`physics.optics`、`cs.AI`、`cs.CV`、`nucl-th`、`astro-ph`、`math.PR`、`cs.OS`、`eess.SP`、`math.OC`、`math.DS`、`math.DG`、`math.MP`、`cs.MM`、`stat.ME`、`math.CO`、`cs.NE`。在此分类框架基础上，进一步追踪采集各对应类别的最新学术论文，通过科学的数据整合方法构建高质量的混合训练数据集。

所构建的数据集涵盖预训练数据和监督微调（`SFT`）数据两个层次，为`InternLM`模型的论文分类能力训练提供多维度的数据支撑。同时，为确保评估的科学性和可信度，验证集和测试集均采用20种不同问题模板进行构建，既保持了与训练数据的同源性，又通过模板多样化有效防止了过拟合现象，从而为参赛者的模型优化和竞赛评估奠定了坚实可靠的数据基础。

### Arxiv dataset样例

```
{&quot;id&quot;: &quot;0711.3691&quot;,
 &quot;submitter&quot;: &quot;Eric Laporte&quot;,
 &quot;authors&quot;: &quot;Olivier Blanc (IGM-LabInfo), Matthieu Constant (IGM-LabInfo), Eric Laporte (IGM-LabInfo)&quot;,
 &quot;title&quot;: &quot;Outilex, plate-forme logicielle de traitement de textes \&#x27;ecrits&quot;,
 &quot;comments&quot;: null,
 &quot;journal-ref&quot;: &quot;Dans Verbum ex machina. Proceedings of TALN - Outilex, plate-forme logicielle de traitement de textes \&#x27;ecrits, Louvain : Belgique (2006)&quot;,
 &quot;doi&quot;: null,
 &quot;report-no&quot;: null,
 &quot;categories&quot;: &quot;cs.CL&quot;,
 &quot;license&quot;: null,
 &quot;abstract&quot;: &quot;The Outilex software platform, which will be made available to research, development and industry, comprises software components implementing all the fundamental operations of written text processing: processing without lexicons, exploitation of lexicons and grammars, language resource management. All data are structured in XML formats, and also in more compact formats, either readable or binary, whenever necessary; the required format converters are included in the platform; the grammar formats allow for combining statistical approaches with resource-based approaches. Manually constructed lexicons for French and English, originating from the LADL, and of substantial coverage, will be distributed with the platform under LGPL-LR license.&quot;,
 &quot;versions&quot;: [
 {&quot;version&quot;: &quot;v1&quot;, &quot;created&quot;: &quot;Fri, 23 Nov 2007 09:45:13 GMT&quot;},
 {&quot;version&quot;: &quot;v2&quot;, &quot;created&quot;: &quot;Tue, 27 Nov 2007 10:22:24 GMT&quot;}
 ],
 &quot;update_date&quot;: &quot;2007-11-27&quot;,
 &quot;authors_parsed&quot;: [
 [&quot;Blanc&quot;, &quot;Olivier&quot;, &quot;&quot;, &quot;IGM-LabInfo&quot;],
 [&quot;Constant&quot;, &quot;Matthieu&quot;, &quot;&quot;, &quot;IGM-LabInfo&quot;],
 [&quot;Laporte&quot;, &quot;Eric&quot;, &quot;&quot;, &quot;IGM-LabInfo&quot;]
 ]}
```

### 抽取数据集样例

```
{&quot;system&quot;: &quot;你是个优秀的论文分类师&quot;, 
&quot;conversation&quot;: 
[{&quot;human&quot;: &quot;Based on the title &#x27;Theory of linear sweep voltammetry with diffuse charge: unsupported   electrolytes, thin films, and leaky membranes&#x27;, authors &#x27;David Yan, Martin Z. Bazant, P. M. Biesheuvel, Mary C. Pugh, and\n  Francis P. Dawson&#x27;, and abstract &#x27;Linear sweep and cyclic voltammetry techniques are important tools for electrochemists and have a variety of applications in engineering. Voltammetry has classically been treated with the Randles-Sevcik equation, which assumes an electroneutral supported electrolyte. In this paper, we provide a comprehensive mathematical theory of voltammetry in electrochemical cells with unsupported electrolytes and for other situations where diffuse charge effects play a role, and present analytical and simulated solutions of the time-dependent Poisson-Nernst-Planck (PNP) equations with generalized Frumkin-Butler-Volmer (FBV) boundary conditions for a 1:1 electrolyte and a simple reaction. Using these solutions, we construct theoretical and simulated currentvoltage curves for liquid and solid thin films, membranes with fixed background charge, and cells with blocking electrodes. The full range of dimensionless parameters is considered, including the dimensionless Debye screening length (scaled to the electrode separation), Damkohler number (ratio of characteristic diffusion and reaction times) and dimensionless sweep rate (scaled to the thermal voltage per diffusion time). The analysis focuses on the coupling of Faradaic reactions and diffuse charge dynamics, although capacitive charging of the electrical double layers (EDL) is also studied, for early time transients at reactive electrodes and for non-reactive blocking electrodes. Our work highlights cases where diffuse charge effects are important in the context of voltammetry, and illustrates which regimes can be approximated using simple analytical expressions and which require more careful consideration.&#x27;, please determine the scientific category of this paper.\n\nA. quant-ph\nB. physics.chem-ph\nC. physics.atom-ph\nD. cond-mat.soft\nE. cs.RO\nF. cs.CL\nG. cs.SE\nH. cs.IR\nI. hep-th\nJ. hep-ph\nK. physics.optics\nL. cs.AI\nM. cs.CV\nN. nucl-th\nO. astro-ph\nP. math.PR\nQ. cs.OS\nR. eess.SP\nS. math.OC\nT. math.DS\nU. math.DG\nV. math.MP\nW. cs.MM\nX. stat.ME\nY. math.CO\nZ. cs.NE&quot;, 
&quot;assistant&quot;: &quot;B&quot;}]}
```

本次比赛将不限微调框架和微调数据，因此需要参赛选手自行对数据进行处理和优化，同时也应自行就行模型训练的调参等工作，后续的介绍中将使用`xtuner`和`ms-swift`框架讲解如何微调。

### 评测集介绍

使用`opencompass`进行测评，测评集为单选，如论文存在多类别的情况下，能保证除对应选项外，其他类别均不在选项内。

![](img/ed8e65e00d91627736f980ccf447700f.png)

![](img/2d6762833375a512425909cd4dd9bf0a.png)

## 比赛结果提交

**模型上传之后千万不要换和删除**

**新模型上传到新的仓库**

否则复查时出现问题当成绩无效处理

| **注意**：飞书表格的评测提交时间一定要晚于「模型仓库的最后更新时间」，否则该次提交成绩无效。 |  |
| --- | --- |
