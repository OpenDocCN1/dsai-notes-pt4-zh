# 超全拆解AlphaFold 3，上海交大钟博子韬：极致利用数据，以原子精度预测所有生物分子结构，但并不完美

> 来源：[https://agijuejin.feishu.cn/docx/EBzld0CP3oHRWQxjlxbcvW52nge](https://agijuejin.feishu.cn/docx/EBzld0CP3oHRWQxjlxbcvW52nge)

![](img/5c074faf1e3b3b8a55d8ce90e1448267.png)

> 作者：钟博子韬    [ 阅读原文](https://hyper.ai/news/33870)

能够**以「**原子精度」预测出所有生物分子结构和相互作用的 AlphaFold 3，一经面世便引起了业界的广泛讨论。8 月 13 日，在上海交通大学 AI for Bioengineering 暑期学校活动中，钟博子韬博士以「AlphaFold 3：原理，应用与展望」为题，系统性地梳理了他的学习心得，并广泛整理了来自科研界的众多相关研究成果，向大家分享了他对于 AlphaFold 3 的深刻洞察，HyperAI超神经在不违原意的前提下，整理了演讲的核心内容，以下为演讲实录。

![](img/dc278a108f2e1d11c86a3cb85f57e2d3.png)

钟博子韬博士分享现场

聚焦蛋白质结构预测，今天我们来谈谈 AlphaFold 3，作为当前顶尖的蛋白质、乃至更广泛的生物分子结构预测工具，AlphaFold 3 的地位已不言而喻。

蛋白质合成始于 DNA 转录，后将遗传信息传递给 RNA，再翻译成蛋白质，进一步折叠成二级结构、三级结构、四级结构。大多数蛋白质会折叠成独特的构象，而结构所需的信息都编码在氨基酸序列中，也就是我们常说的：序列决定结构，结构决定功能，蛋白质结构预测对于了解生物功能至关重要。

## AlphaFold 3 突破：革新模型架构，提高数据利用率

### 对比 AlphaFold 3 与 AlphaFold 2 模型架构

**过去，AlphaFold 2 在**蛋白质结构预测方面直接「血虐」其他算法。其核心架构可归结为 3 个关键部分，如下图所示：第一部分，蓝色框内的 MSA & Template 模块，其功能在于搜集并整合多序列比对 (MSA) 及模版结构信息作为模型的输入数据。第二部分，绿色框内的 Evoformer 模块，其功能在于理解多序列组织中的共进化信息，通过提炼并处理收集到的信息，将其传递给第三部分紫色框内的 Structure Module 模块。

![](img/7968fc2a0f396f10023db42ea98b8c58.png)

以**深**度学习视角来看，Evoformer 扮演了编码器的角色，而 Structure Module 则相当于解码器，从这方面来说，AlphaFold 2 之所以备受赞誉，很大程度上归功于其端到端的优化能力，即直接从序列输入映射到结构输出。

外界普遍认为 AlphaFold 3 的模型架构变化并没有想象中那么大，其模型框架也是由 3 个关键部分组成，每部分和 AlphaFold 2 的对比如下：

**第一部分：保持高度相似**

如下图所示，比较 AlphaFold 3 和 AlphaFold 2 的架构图可得，AlphaFold 3 的第一部分（蓝色框内）依然包括 MSA & Template，并额外引入了Conformer generation（构象生成）环节。

![](img/c6e280f666ab7f2d0c0a05a24fdc8635.png)

第二部分：减少对 MSA 序列的依赖

AlphaFold 3 第二部分（绿色框内）命名为 Pairformer，其结构实质上与 Evoformer 高度相似，**但 MSA Module 数量减少到** 4 个。如下图所示，绿色箭头是两个模块相同的内容，黄色箭头标代表有区别，可以看到，AlphaFold 3 更重视目标蛋白质序列，而减少对 MSA 序列的依赖。

![](img/a45e80751d5025ec2e0cffee48297f2c.png)

**进**一步地，我们认为，AlphaFold 3 之所以能在多项任务中表现出强大性能，可能就是因为它降低了对多序列比对 (MSA) 的依赖。如下图所示，右侧展示了 MSA 对 AlphaFold 2 性能的影响：随着 MSA 数量的增加，当超过某一阈值（粉色线）后，AlphaFold 2 性能的提升趋于平缓。据下图中间部分可得，与 AlphaFold 2 相比，MSA 对 AlphaFold 3 的影响减弱（曲线浮动幅度很小）。

![](img/bd4526b83385dab80e492021253959ac.png)

另外，抗体的成熟经常需要通过体内超突变的过程， MSA 信息对其结构预测的帮助较为有限，蛋白质及其复合物也很难找到成对的 MSA 信息，从这一点来看，AlphaFold 3 应用范围的拓展或许就是其对 MSA 的依赖性减弱。

第三部分：全原子结构生成+去除立体旋转不变性

AlphaFold 3 第三部分（紫色框内）采用 Diffusion model（扩散模型），它也属于结构模块范畴，区别在于，Diffusion model 将 Structure Module 中的重复迭代优化替换为一种称为扩散模型的新机制。

*Diffusion model：给模型加噪（前向），让模型去噪（反向），学习反向过程，生成类似的数据分布。

如下图所示，在第三部分，AlphaFold 3 实现了全原子级别的结构生成。原子作为分子的基本组成单元，可能含有更丰富的物理信息，这意味着 AlphaFold 3 在预测蛋白质结构时，或许会捕捉更深层次的物理规律。此外，AlphaFold 3 放弃了AlphaFold 2 中强调的立体旋转不变性，删除 AlphaFold 2 中该特性的额外架构后，研究人员发现，模型 (Diffusion Module) 的设计变得更加自由。

![](img/30344d5c0372413af76724ca547f1276.png)

AlphaFold 3 提高对数据的利用率

**蛋白质数据资源有限，但 AlphaFold **3 不仅数据集变大，还提高了数据利用率。具体而言，相比于 AlphaFold 2 的百万级数据集，AlphaFold 3 直接逼近亿级，训练集增大。此外，其训练集除了包含 PDB 中的数据外，还融入了大量其他数据，比如，选取 AlphaFold 2 预测较准的结构数据作为训练集的扩充。具体训练集如下图所示：

![](img/e3d9605eb22ed354b20f7bc4e7284ce4.png)

## AlphaFold 3 在应用范围上实现高度飞跃

AlphaFold 3 的最大改变是它在应用范围上实现了质的飞跃。过去，AlphaFold 2 更多的是预测氨基酸结构，而 AlphaFold 3 能直接预测原子级结构，其功能拓展具体体现在以下 4 个方面：

+   能够准确预测配体 (Ligand)，即预测小分子在蛋白质中的结合位点；

+   能够预测蛋白质复合物 (Protein complex) 结构；

+   能预测蛋白质及核酸上的翻译后修饰 (Post-translational modification) 结构；

+   能够预测 DNA 和 RNA 的结构，以及 DNA/RNA 和蛋白质的复合物结构。

### AlphaFold 3 改变 Ligand Docking 领域

**其**中，AlphaFold 3 对科学领域影响最大的是对 Ligand Docking（即配体对接）任务的改善。如下图所示，在基准测试 PostBusters Benchmark 下评估不同深度学习算法在 4 个不同 Ligand Docking 任务下的成功率，可以得到，AlphaFold 3 在未知口袋和结构先验知识的前提下，能够达到最高的成功率，即 76.4%。

![](img/f645fa5153c1054b5a9d4529c3cfe29e.png)

PostBusters Benchmark 选择 428 个来自于 2021 年之后的 PDB 数据

任务成功的标准：预测的小分子对接位置与真实对接位置之间的偏差小于 2Å

如上图所示，在第一类 Blind docking 任务中，未知口袋位置、已知蛋白质结构 (No pocket, Holo structure)，DiffDock 能达到最高的成功率 37.9%。

在第二类 Co-folding 任务中（小分子和蛋白质结构都折叠），未知口袋位置、未知蛋白质结构 (No** **pocket, No structure)，AlphaFold 2+DiffDock 结合预测的成功率降到了 18%，另外，AlphaFold 3 达到了最高的 76.4% 成功率，这说明，AlphaFold 3 不仅预测准确，还无需依赖口袋和结构的先验知识。

在第三类 Traditional docking 任务中，已知小分子的口袋位置、已知蛋白质结构 (With pocket, Holo structure)，即口袋处于暴露状态，Gold 达到 51.2% 的成功率，Vina 表现出 52.3% 的成功率，Glide 则提升至 55%，其他深度学习算法也能达到相对较好的水平，说明成功率受到口袋的影响。

在第四类 Guided co-folding 任务中，已知口袋位置、未知蛋白质结构 (With pocket, No structure)**，**模型的成功率都显著提升，AlphaFold 3 从 76.4% 提升到 90.2%，说明已知口袋信息可以提高任务成功率。但是，由于目前口袋的定义存在一些争议，所以想要知道 AlphaFold 3 对 Ligand docking 任务的具体改善，可以只考虑第二类任务下的成功率，该结果相对来说更稳定。

如下图所示，不同模型的口袋定义存在显著差异。Gold 的口袋是一个 25Å 的球体（图左上方蓝色部分），而 Vina 模型则采用 25Å 的正方体作为口袋表示，DeepDock 口袋尺寸为 10Å，Uni-Mol 口袋尺寸为 8Å。

![](img/71c013dcd324a89524e68e7b278c651b.png)

如上图右侧所示，当 Gold 模型的口袋大小从 25Å 逐步缩小 6Å 时，其 PoseBusters 基准测试成功率相对稳定，这得益于 Gold 基于物理算法的特性。相反，深度学习算法 Uni-Mol 逐步缩小口袋至 6Å，成功率升高至 68%，25Å 下则降为零，体现了一部分深度学习对接算法对于口袋的依赖性。

同**样地，前面说到，AlphaFold 3 在引入口袋信息后，对接成功率显**著提升，从 76.4% 提升至 90.2%，综上，口袋信息对于提高模型预测成功率有关键作用。但理想情况下，无需口袋或结构信息即可达到高准确性的模型是我们的最优选择，比如 AlphaFold 3。

### AlphaFold 3 实现抗体、抗原结构预测

AlphaFold 3 的另外一个应用就是抗体、抗原的结构预测。下图左侧是 AlphaFold 3 对于抗体、抗原结构预测的性能评估。在较低评估标准下 (DockQ>0.23)，只运行 1 次，AlphaFold 3 的成功率低于 40%（浅蓝色线），但经过 1,000 次尝试后，预测成功率能提升至 60%。

![](img/a80614e102f3d97bcde25b0898e424d7.png)

+   左图：抗体结构预测，每个数据点表示在 1,200 个种子中随机抽取 1,000 个种子的的平均得分

+   右图：当评估指标 DockQ 大于 0.23 时，可视为结构准确性尚待验证；当 DockQ 超过 0.8 时，结构预测高度精确

进一步地，若以更为严格的标准衡量 (DockQ>0.8)，单次运行的成功**率**可能低至 10%，通过增加运行次数至 1,000 次，成功率可提升至 30%。这表明，我们可以通过增加 AlphaFold 3 运行次数 (seeds per target)，来提升抗体抗原结构预测成功率。

然而，如上图右侧表示，AlphaFold 3 仅在预测复合物 protein-protein 的结构时，能够通过增加运行次数提升成功率，这说明，对于其他类型的复合物结构预测，AlphaFold 3 的适用性也需进一步优化。

### AlphaFold 3 实现共价修饰预测

**如下图所示，在修饰预测方面，AlphaFol**d 3 同样展现出了出色的结构预测能力，成功率可达到约 80%、60%、40% 的水平。对于从事 Covalent Modifications（共价修饰）的研究人员而言，AlphaFold 3 无疑是一个强有力的工具。

![](img/0a38c28edd6247c1c5d672960077d7fd.png)

### AlphaFold 3 在 RNA 结构预测的局限

目前，RNA 结构预测仍然很难，如下图所示，与 RoseTTAFoId2NA 模型相比，AlphaFold 3 预测性能有了较高提升。但在预测 CASP15 RNA 结构时，AlphaFold 3 的准确率低于 Alchemy_RNA2 (has human input) 模型。

![](img/569a74f6a014cf5f487b4deb9690ef08.png)

### 对比 AlphaFold 3 在不同任务上的优劣

**通**过分析 AlphaFold 3 的训练曲线，可以明确模型在不同任务上的表现优劣，LDDT 指标越高越好。如下图所示，模型在预测 intra ligand（配体内部）结构时表现最佳；在预测 intra protein（蛋白质内部）时也展现出较高的准确性；对于 intra dna（DNA 内部）预测，模型同样表现良好，这得益于 DNA 稳定的双螺旋结构；相比之下，模型在 intra rna（RNA 内部）上的表现较差。

![](img/0516ab420040161c6afb1013f7512dd7.png)

转向复合物预测领域，模型在 protein-ligand 复合物结构预测上表现最优，其次是 protein-protein 复合物预测，在 protein-dna 复合物预测上，模型表现有所下滑，protein-rna 复合物预测表现最差。该结果也反映了 RNA 结构预测的难度，RNA 结构数据稀缺，结构动态、灵活，是当前结构生物学领域中面临的难题之一。

此外，研究人员在使用 AlphaFold 3 进行结构预测时，还可以通过 PAE 表来评估预测结果的可靠性。

![](img/3c0805c5d8fb685edb505e1b84a988cb.png)

## AlphaFold 3 并不完美

AlphaFold 3 并不完美，比如，它会找错手性，运行过程中如果出现异常情况，建议多次运行以验证结果的稳定性。其次，AlphaFold 3 在蛋白质 Dynamic 预测上也存在局限，这可能是因为结构数据匮乏，无法掌握蛋白质的多维构象信息。

*如果某物体与其镜像不同，则其被称为「手性的」，其镜像不能与原物体重合，就如同左手和右手互为镜像而无法叠合

**另外，AlphaFold 3 还有一个生成模型普遍存在的问题，**即幻觉 (Hallucination)。如下图的蛋白质结构预测结果，左侧的蛋白质结构仅灰色区域可解析，其余部分因电子密度不足，可能处于未折叠状态。中间的图是 AlphaFold 2 预测该**蛋白质的结果，蓝色区域被认为是折叠状态，其他「彩带」部分被**认为未折叠，预测的结构相对合理。右侧是 AlphaFold 3 的预测结果，它倾向于将所有可能折叠的区域均进行折叠处理，该结构看似合理，但从实际情况出发，上述大部分区域其实并未折叠。因此，AlphaFold 3 的幻觉倾向于将蛋白质预测为折叠状态，而非保留其可能存在的未折叠状态。

![](img/80965e99c0024468eb48ebfa51991267.png)

为应对 AlphaFold 3 的幻觉问题，研究人员选择了一个直接而有效的方法：既然 AlphaFold 2 预测的结果相对合理，那就将 AlphaFold 2 预测的结果纳入 AlphaFold 3 的训练数据集中，以增强模型的训练效果。然而，该方法存在一个局限：若 AlphaFold 2 的预测本身存在错误，则可能影响到 AlphaFold 3 的预测质量，除非能引入其他的数据源来进一步优化模型。

另**外，将 256x OLA **作为输入提交给 AlphaFold 3 时，如下图所示，其预测结果呈现出了一个类似双分子层的结构形态，该结构并非预期或典型形态。

![](img/fb1472d1da3b4c2e4b5729c8aa975c7f.png)

**此外，AlphaFold 3 预测** RNA 与 DNA 的结构也并不精确，如下图所示，预测 RNA 结构时甚至还出现了匪夷所思的互补配对，如 G:G、G:A 等。

![](img/6a5d9ef6810d0abbd9d29fae9c6100bd.png)

## AlphaFold 3 的使用限制

在满足数据非高度保密性的前提下，大家可以通过谷歌提供的网站访问 AlphaFold 3。但该平台也存在一些使用限制，如下图所示，在蛋白质修饰方面，AlphaFold 3 目前仅支持特定 3 个位置上的有限种类修饰，共计 23 种，DNA 修饰仅支持 9 种，RNA 修饰仅支持 15 种，金属离子仅支持 10 种不同金属，而配体仅限于 14 种小分子。

![](img/950ce9816bcefbf8d65167b37689b0c9.png)

因此，在上述特定限制的情况下，AlphaFold 3 可能无法处理大多数研究和反应，或许还要等待其真正开源后才能实现。

综上所述，AlphaFold 3 在扩展其预测范围方面取得了显著成就，超越了**现有的 AI 模型，但它在特定任务上的表现仍有待提升，尤**其是在精细结构预测方面。因此，尽管 AlphaFold 3 已取得重大进展，但完全解决某些复杂问题仍需持续的研究与努力。

## **关于钟博子韬**

![](img/1ddf13302db1c82366234bc2709aebcb.png)

钟博子韬现为上海交通大学人工智能方向博士生，主要研究方向包括高通量蛋白质结构和功能预测，蛋白质构象生成等。2019 年至今发表 20 余篇论文，在 Nature Communications 上发表了高通量 AlphaFold 结构预测解析深海蛋白质组与代谢通路关联的成果，曾 3 度获得国际遗传工程机器大赛 (iGEM) 金奖，并多次出任该竞赛评委。

**Google Scholar:**

https://scholar.google.com/citations?hl=zh-CN&user=6k_LhSoAAAAJ&view_op=list_works&sortby=pubdate