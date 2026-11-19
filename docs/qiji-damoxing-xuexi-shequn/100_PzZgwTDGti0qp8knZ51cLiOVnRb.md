
# 奇绩大模型日报（9月 13日）

> 来源：[https://miracleplus.feishu.cn/docx/DW2LdqQWvo3JEExgEJ3cMMdLn8g](https://miracleplus.feishu.cn/docx/DW2LdqQWvo3JEExgEJ3cMMdLn8g)

## 资讯

### OpenAI o1比博士还“聪明”，全球11位AI大咖怎么看？

https://mp.weixin.qq.com/s/w9chmnT1MTxMeuiTslBdxQ

**“好评”阵营：**

+   Sam Altman（OpenAI CEO）：认为OpenAI o1是迄今为止最强大的模型，尽管仍有缺陷，但其推理能力开创了AI的新范式。

+   Greg Brockman（OpenAI总裁）：强调o1是通过强化学习训练的模型，改进了推理质量和解释性，虽然技术仍处早期，但展现了巨大潜力。

+   Jason Wei（OpenAI研究员）：指出o1的思维链机制能够自适应地进行推理，在科学问题上表现出色，并重新定义了深度学习领域。

+   Jim Fan（英伟达高级科学家）：认为这是自Chinchilla缩放定律以来最重要的进展，o1突破了推理瓶颈，是AI领域的一大飞跃。

+   James Campbell（卡内基梅隆大学博士生）：展示了o1在数学比赛中的出色表现，并认为它显著超越了GPT-4。

**“中差评”阵营：**

+   Clement Delangue（HuggingFace CEO）：批评称o1并不具备真正的“思考”能力，认为OpenAI夸大了它的智能，实际上是预测系统而非人类智能。

+   Gary Marcus（纽约大学教授）：虽然认可o1的进展，但指出它距离AGI还很遥远，且没有完全取代GPT-4的表现，夸大了部分结果。

+   Ethan Mollick（沃顿商学院教授）：认为o1在规划复杂问题上表现出色，但其智能仍依赖于GPT-4，存在错误和局限性。

+   Aravind Srinivas（Perplexity CEO）：认为单纯依赖大语言模型的推理不够可靠，建议结合代码执行和知识图谱来提升模型的实用性。

![](img/3c6a68fa103e9d2ddf1d309a42504ce0.png)

### Reflection开源大模型造假风波

https://mp.weixin.qq.com/s/nFZ80XOukks7f6V-TXV7lg

质疑主要集中在测试成绩和模型来源。第三方机构Artificial Analysis测试发现，Reflection在多个基准测试（如MMLU、GPQA、MATH）中的成绩无法复现，表现甚至不如Llama 3.1-70B。官方辩解称发布的版本有误，但后续的测试成绩依旧未达到官方宣称的高度。同时，有人对Reflection的各层进行了分析，认为它是由Llama 3改造而来，而非官方声称的Llama 3.1。

更严重的指控是Reflection被认为套壳Claude。多个迹象显示，Reflection与Claude 3.5 Sonnet的输出高度相似，并且在特定情况下模型会承认自己是Claude。对此，Reflection的数据供应商Glaive AI创始人Sahil Chaudhary回应称，模型并未套壳，目前正在整理相关证据。

事件的幕后细节也被披露。Reflection发布前曾寻求Hyperbolic平台的托管支持，但测试发现该模型的成绩与宣传不符，官方后来提供了更新版本，仍未达预期。最终，托管平台决定下线Reflection的API。

![](img/293a07c02d0dd008c400a1d1fb437662.png)

### 国产GPU独角兽启动IPO

https://mp.weixin.qq.com/s/E63ZOdSJ-8IJI7bGzS6WIQ

9月11日，中国证监会公布上海壁仞科技股份有限公司的IPO辅导备案，辅导券商为国泰君安。壁仞科技是一家专注于通用计算体系研发的GPGPU独角兽，成立于2019年，注册资本3291.64万元。该公司在AI训练和推理领域致力于突破国产高端智能计算芯片，2022年推出的首款通用GPU产品“壁砺系列”已量产，并广泛应用于大模型和生成式AI等场景。

截至目前，壁仞科技已完成超过50亿元人民币的B轮融资，投资方包括珠海大横琴集团、格力创投、中国平安等知名机构。根据2024年胡润全球独角兽榜，壁仞科技估值155亿元，位列第495名。主要股东包括壁仞科技创始人张文（持股12.48%）、QM120 Limited（5.58%）等。

![](img/d4c3e2d9ce185227637d2ecf1b2a8edc.png)

### 边缘智能的新时代：端侧大模型的研究进展综述

https://mp.weixin.qq.com/s/eUw8bjgDCKeo60EQhM22ow

近年来，随着Transformer架构的发展，LLMs如OpenAI的GPT和Meta的LLaMA系列引领了自然语言处理领域的变革。然而，这些模型主要部署在云端，导致网络延迟、数据安全等问题。为此，边缘设备上的LLMs部署应运而生。

部署LLMs在边缘设备上的主要技术挑战包括计算能力和内存的限制。通过模型压缩技术如量化、剪枝和知识蒸馏，研究人员有效地降低了模型大小，提升了运行效率。此外，硬件加速技术如NVIDIA的Tensor Cores、Google的TPU，以及边缘云协同计算框架（如EdgeShard）等，使得LLMs在边缘设备上的推理速度和内存利用率显著提升。

![](img/e571f933a0edd46309cdccda10c6d1c1.png)

### 电子：关注苹果新品发布和AI功能落地节奏

https://mp.weixin.qq.com/s/isbkGGhYcrz_r5VljG9xYQ

1.  **硬件升级**：iPhone 16系列搭载了3nm工艺的A18芯片，支持AI本地化运行，内存总带宽增加17%，GPU性能提升20%，CPU性能提升15%。相机控制按钮支持视觉智能和拍摄控制，通过按压力度实现不同功能。

1.  **影像升级**：标准版iPhone 16配备48MP主摄和12MP超广角摄像头，支持空间视频拍摄；Pro版则搭载48MP主摄、12MP潜望式长焦和48MP超广角摄像头，支持120fps 4K电影感慢动作视频拍摄。

1.  Apple Intelligence：AI功能包括书写工具、Genmoji、更新版Siri等，采用“端+云”方式，强调隐私保护。首批功能包括更自然的Siri、书写工具、照片搜索、AI消除、通话摘要和智能回复。

1.  **可穿戴设备**：Series 10手表搭载S10芯片，屏幕面积增加30%，尺寸更窄，充电速度提升，支持心脏问题警示和睡眠呼吸暂停检测。AirPods 4采用H2芯片，支持头部动作交互，具备主动降噪功能。

![](img/49e28ba59cd0e611cf56dbdd7d64d38c.png)

## 推特

### Devin评估OpenAI o1模型：新系列模型在处理代码的自主系统方面有显著提升

https://x.com/cognition_labs/status/1834292718174077014

我们在过去几周与 OpenAI 紧密合作，使用 Devin 评估了 OpenAI o1 模型的推理能力。结果显示，新系列模型在处理代码的自主系统方面有显著提升。

在评估过程中，我们使用了简化版的 Devin——“`Devin-Base`”，以便专门衡量基础模型的变化对 Devin 能力的影响。与 GPT-4o 相比，o1 在反思和分析能力上表现出色，能够在得出正确答案前回溯并考虑不同选项，减少了幻想和错误的可能性。此外，使用 `o1-preview` 时，Devin 更能准确诊断问题的根本原因，而不是仅仅解决表面症状。

在提示 o1 时，我们发现与以往模型不同：

•之前常用的“链式思考”或要求模型“边想边说”在 o1 上效果不佳，反而只要求给出最终答案效果更好，因为 o1 会在回答前自行思考。

•o1 对上下文的密度要求更高，对杂乱和不必要的词语更敏感，传统的冗余指令会影响其表现。

•o1 的智能提升带来了对高度规范指令的遵循变异性增加。

我们使用内部基准`cognition-golden`对 Devin 进行评估，设计了针对经济价值任务的真实评估，环境可完全复现并具有自主反馈。在详细报告中，我们展示了例如使用用户提供的数据源部署 Grafana 仪表盘的任务，介绍了模拟用户、评估代理和评估评估者的方法。

总的来说，OpenAI 团队在推动行业进步方面取得了令人印象深刻的成果，我们期待继续合作

![](img/6790d7841af9886684026c82bcd8dd5a.png)

### Shawn Wang总结o1在各个方面表现：经济学、遗传学、物理学、编程

https://x.com/swyx/status/1834284741610405965

🎉恭喜 @OpenAI 发布 o1：

• **经济学**：

@tylercowen 基本上要求 o1 撰写一篇大学论文

•**遗传学**：

@catbrownstein 让 o1 帮助她推理“n of 1”案例 —— 没有人见过的医学案例

•**物理学**：

@mariokrenn6240 使用 o1 起草并推理复杂的量子物理方程

•**编程**：

@ren_hongyu 提示 o1 生成一个完整的贪吃蛇游戏，零样本生成，运行完美，并遵循添加障碍的指示

![](img/a26a6dc17f51474aaa6635591fb9a831.png)

[ssstwitter.com_1726164030139.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UB8IbxHeOoikuhxU8ylcQk1znmb)

### NotebookLM AI音频概览：创建两位声音讨论你上传内容的播客

https://x.com/OfficialLoganK/status/1834239568070971441

刚刚在人工智能领域经历了第三次震撼时刻……这次是通过 NotebookLM 的 AI 概览 🤯

---

看看 NotebookLM 吧！创建一个笔记本，上传一个或多个资源（例如研究论文的 PDF 文件、你最喜欢的博士论文、报纸文章等），然后点击“生成”，即可创建一个由两位声音讨论你上传内容的播客。

![](img/7c4ee3e3780b2c0cff71744e1d89189d.png)

[ssstwitter.com_1726157637679.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SmMzb6TzaoNtHSxvxQEcqaCBnTc)

### OpenAI o1和Cursor Composer结合：10分钟创建 iOS 应用

https://x.com/ammaar/status/1834348042637521031

刚刚将 @OpenAI 的 o1 和 Cursor Composer 结合起来，在不到10分钟内创建了一个 iOS 应用！

o1 mini 启动了项目（o1 思考时间太长），然后切换到 o1 完成细节。

然后砰——完整的带动画的 iOS 天气应用，耗时不到10分钟 🌤️

视频加速！

![](img/178206b7fa777b4dc753048bbabfc062.png)

[ssstwitter.com_1726218299530.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DOzqbZb8HowjwRx2NHRcdCkPnCh)

### 只需对着 Suno 唱歌，就能将您的人声转化为完整的歌曲

https://x.com/nickfloats/status/1834332468662391043

好的，

@suno_ai_ 刚刚发布了一个名为 “Covers” 的全新AI音乐功能，简直是纯粹的魔法。

它可以与您的声音协同工作。您只需对着 Suno 唱歌，提供一个提示，它就能将您的人声转化为完整的歌曲。这就像与AI进行的音乐合作。

下方有试用链接

![](img/9c2a71a189995e0c957e8bf6b781f72a.png)

[ssstwitter.com_1726176011065.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NcR8bhoNMovNL8xeG5UcjcKsnne)

## 论文

### 基于话语重写的无监督对话主题分割模型

贡献人：

链接：https://arxiv.org/abs/2409.07672

对话主题分割在各种对话建模任务中起着至关重要的作用。当前最先进的无监督对话主题分割（DTS）方法通过相邻话语匹配和伪分割，从对话数据中学习与主题相关的语篇表示，进一步挖掘未标注对话关系中的有用线索。然而，在多轮对话中，话语常常存在共指或省略，导致直接使用这些话语进行表示学习可能会对邻近话语匹配任务中的语义相似性计算产生负面影响。为了充分利用对话关系中的有用线索，本研究提出了一种新颖的无监督对话主题分割方法，该方法结合了话语重写（Utterance Rewriting, UR）技术与无监督学习算法，通过重写对话以恢复共指和省略词，有效利用未标注对话中的有用线索。与现有的无监督模型相比，所提出的话语重写主题分割模型（`UR-DTS`）显著提高了主题分割的准确性。主要发现是在DialSeg711数据集上，绝对误差评分和WD分别提高了约6%，达到11.42%和12.97%；在Doc2Dial数据集上，绝对误差评分和WD分别提高了约3%和2%，最终在绝对误差评分和WD上达到35.17%和38.49%，实现了当前最优（SOTA）的性能。这表明该模型在捕捉对话主题的细微差别方面非常有效，同时展示了利用未标注对话的有用性和挑战性。

![](img/5886e38e6769ed4a476a0c763f6956cd.png)

### 微调大型语言模型用于实体匹配

贡献人：

链接：https://arxiv.org/abs/2409.08185

生成式大型语言模型（LLMs）因其高零样本（zero-shot）性能和对未见实体的泛化能力，成为实体匹配的有前景的替代预训练语言模型的选择。现有关于使用LLMs进行实体匹配的研究主要集中在提示工程（prompt engineering）和上下文学习（in-context learning）上。本文探讨了微调LLMs在实体匹配中的潜力。我们从两个维度分析了微调方法：1）训练样本的表示，我们尝试在训练集中添加不同类型的LLM生成的解释；2）使用LLMs进行训练样本的选择和生成。除了在源数据集上的匹配性能外，我们还研究了微调如何影响模型在其他同领域数据集以及跨主题领域的泛化能力。实验结果表明，微调显著提升了较小模型的性能，而对较大模型的效果则参差不齐。微调还改善了对同领域数据集的泛化能力，但对跨领域迁移产生了负面影响。我们发现，向训练集中添加结构化解释对四个LLMs中的三个模型的性能有正面影响，而所提出的示例选择和生成方法仅提升了Llama 3.1 8B的性能，同时降低了GPT-4o Mini的性能。

![](img/2fd46f2cbac9968e23fff6624cba6058.png)

![](img/4da03a509b445cd524cd869c032a86c1.png)

### Source2Synth：基于真实数据源的合成数据生成与整理

贡献人：

链接：https://arxiv.org/abs/2409.08239

大型语言模型（LLMs）在利用结构化数据、复杂推理或工具使用的挑战性场景中仍然存在困难。本文提出了`Source2Synth`：一种无需依赖昂贵的人类注释即可用于教授LLMs新技能的新方法。`Source2Synth`以自定义数据源作为输入，生成具有基于现实世界来源的中间推理步骤的合成数据点。`Source2Synth`通过根据可回答性丢弃低质量的生成内容，从而提高数据集的质量。我们通过将该方法应用于两个具有挑战性的领域来证明其通用性：在多跳问题回答（Multi-Hop Question Answering, MHQA）中测试推理能力，以及在表格问题回答（Tabular Question Answering, TQA）中测试工具使用能力。与微调基线相比，我们的方法在WikiSQL的TQA任务上提升了25.51%，在HotPotQA的MHQA任务上提升了22.57%。

![](img/b25955bb299dec4efeedf8e765441379.png)

![](img/5fa6c2ac1d4da96b972382dcd6bd0ea5.png)

### 逆向约束强化学习综述：定义、进展与挑战

贡献人：

链接：https://arxiv.org/abs/2409.07569

逆向约束强化学习（Inverse Constrained Reinforcement Learning, ICRL）是从专家代理的示范数据中推断其遵循的隐式约束的任务。作为一个新兴的研究主题，ICRL近年来受到了相当大的关注。本文对ICRL的最新进展进行了分类综述。它作为机器学习研究人员和从业者的全面参考，同时也适合初学者理解ICRL的定义、进展和重要挑战。我们首先正式定义了该问题，并概述了在各种场景下促进约束推断的算法框架。这些场景包括确定性或随机环境、有限示范的环境以及多代理环境。对于每种情境，我们说明了关键挑战并介绍了一系列基础方法来解决这些问题。本综述涵盖了用于评估ICRL代理的离散、虚拟和现实环境。我们还深入探讨了ICRL的最相关应用，如自动驾驶、机器人控制和体育分析。为了促进持续研究，本文在结尾讨论了ICRL中的关键未解问题，这些问题能够有效地架起理论理解与实际工业应用之间的桥梁。

![](img/cdc560ec9933e39d20be9feab746e649.png)

![](img/6919b0c2704c98404a7c406fa0e6ac3a.png)

### 深度学习正则化在离线强化学习中对演员网络的作用

贡献人：



链接：https://arxiv.org/abs/2409.07606

深度学习正则化技术，如 dropout、layer normalization 或 weight decay，在构建现代人工神经网络中被广泛采用，通常能带来更稳健的训练过程和更好的泛化能力。然而，在强化学习（Reinforcement Learning, RL）领域，这些技术的应用仍然有限，通常仅应用于价值函数估计器 \citep{hiraoka2021dropout, smith2022walk}，且可能产生不利影响。这一问题在离线强化学习（offline RL）环境中更加明显，尽管离线RL与监督学习更为相似，但其研究关注度较低。近期在连续离线RL中的研究表明，尽管我们可以构建足够强大的评论家网络（critic networks），但演员网络（actor networks）的泛化能力仍然是一个瓶颈。在本研究中，我们通过实验证明，在离线RL的演员-评论家算法中对演员网络应用标准正则化技术，平均可以在两种算法和三个不同的连续D4RL领域中提升6%的性能。

![](img/dd5ac055bafa2fc4a5c8de18509d5591.png)

![](img/274091829a3e091802b954b68d6a6504.png)

# GRE²-MDCL：通过多维对比学习增强的图表示嵌入

贡献人：

链接：https://arxiv.org/abs/2409.07725

图表示学习已成为在将节点映射到向量表示时保留图拓扑结构的强大工具，支持节点分类、社区检测等各种下游任务。然而，大多数现有的图神经网络（Graph Neural Network, GNN）模型面临需要大量标注数据的挑战，这限制了它们在标注数据稀缺的实际场景中的应用。为了解决这一挑战，研究人员探索了图对比学习（Graph Contrastive Learning, GCL），利用增强的图数据和对比学习技术。尽管前景广阔，现有的GCL方法在有效捕捉局部和全局图结构以及平衡节点级和图级表示之间的权衡方面仍然存在困难。在本研究中，我们提出了通过多维对比学习增强的图表示嵌入（Graph Representation Embedding Enhanced via Multidimensional Contrastive Learning, GRE²-MDCL）。我们的模型引入了一种新颖的三网络架构，以多头注意力GNN作为核心。GRE²-MDCL首先使用奇异值分解（SVD）和局部注意力GNN（Local Attention GNN, LAGNN）技术对输入图进行全局和局部增强。然后，它构建了一个多维对比损失，结合了跨网络、跨视图和邻居对比，以优化模型。在基准数据集Cora、Citeseer和PubMed上的大量实验表明，GRE²-MDCL实现了最先进的性能，平均准确率分别达到82.5%、72.5%和81.6%。可视化结果进一步显示了更紧密的类内聚集和更清晰的类间边界，突显了我们框架在提升基线GCL模型方面的有效性。

![](img/4f7919dbd05accd6be92fe5fce01e615.png)

![](img/12d6d1966f49d67a8f2be0d12cdf3481.png)

# 产品

## Thunderbit

贡献人：

https://thunderbit.com/

Thunderbit 是一款 Chrome 扩展，通过人工智能和无代码技术帮助用户自动化网络任务。用户只需填写简单表单即可创建个性化的 AI 助手，支持功能包括网页抓取、数据剪辑和电子邮件摘要，简化自动化流程，降低使用门槛。

![](img/1ddc486d0114b38a06d3b00862ef71b9.png)

## ContentRadar

贡献人：

https://contentradar.ai/

ContentRadar 是一款自动化工具，帮助用户将写作和播客内容快速转化为 LinkedIn 和 X 平台的帖子，支持一键创建和安排内容。它利用人工智能技术，节省时间并简化协作，适合创业者、代理机构和自由职业者等用户。

![](img/cd22589c22a157bab7ba7d9511877e3e.png)

# HuggingFace&Github

## VMAs

贡献人：

https://genjib.github.io/project_page/VMAs

VMAs 是一种生成背景音乐的框架，从视频输入中学习生成音乐。与现在依赖于有限数量和多样性的符号音乐注释方法不同，VMAs 利用大量带有背景音乐的网络视频，使模型能够学习生成真实且多样化的音乐。

![](img/ee904b6c51f3c0b740ec20492a63b141.png)

[17.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XIkabfwpzoHjIgxu1ALcxrtPngg)

## MVLLaVA

贡献人：

https://jamesjg.github.io/MVLLaVA_homepage/

MVLLaVA 是一个智能代理，结合了多视角扩散模型和大型多模态模型 LLaVA，能够高效处理新视角合成任务。它支持多种输入类型，并通过语言指令生成新视角图像，展示出强大的灵活性和性能。实验结果验证了其在多样化任务中的有效性。

![](img/32435ab4855c09c602547b478691d425.png)

# 投融资

## 前小米机器人骨干创业：完成数千万天使轮融资，研发高性价比机器人关节

贡献人：

https://mp.weixin.qq.com/s/5F26kwx2UeviDMqLv1Jh1Q

机器人关节厂商「灵足时代」近日宣布完成数千万元天使轮融资，本轮融资由英诺天使基金、雅瑞智友科学家基金及一维创投共同投资。资金将主要用于机器人关节的新产品研发以及团队搭建等方面。「灵足时代」成立于2024年1月，专注于开发适用于机器人的一体化关节产品，并为其他行业提供机器人整机硬件定制服务。

### 融资亮点：

1. 投资方背景：

+ 英诺天使基金对「灵足时代」的技术积累和市场潜力表示看好，认为其产品具有极高的性价比和跨领域应用前景，未来有望对传统传动市场产生革新影响。

+ 雅瑞智友科学家基金由国内机器人领域专家王田苗创立，特别关注机器人核心零部件的创新能力，认为「灵足时代」在电机驱动和控制方面具备高性能表现，且稳定的量产能力将推动机器人行业发展。

+ 一维创投长期看好机器人赛道，认为「灵足时代」凭借核心硬件能力和务实的商业模式，具备行业领先的竞争优势。

2. 资金用途：

+ 该笔融资将用于开发新款机器人关节产品，包括即将发布的准直驱一体化关节模组“RobStride03”，这款产品在市场中性价比极具竞争力，售价仅为999元，相比同行产品降低了一半以上的成本。

+ 资金也将用于团队建设和业务扩展，特别是在海外市场上，已成功进入欧洲、美国、南非等国家，并与海外知名企业合作研发机器人整机硬件架构。

3. 商业化前景：

+ 「灵足时代」的关节产品已被国内30%左右的人形机器人厂商采用，其产品不仅局限于机器人领域，还计划扩展到智能滑板车、电动车、自动驾驶清洁车等其他应用场景。

+ 创始团队成员出身于小米机器人团队，拥有超过10年的机器人关节开发经验，为公司快速迭代和商业化进程提供了强有力的支持。

公司官网：https://www.robstride.com/

![](img/697955c3c1c1c71517544e179f8760ca.png)

## OffDeal：用AI助力小企业寻找大规模退出机会

https://techcrunch.com/2024/09/12/offdeal-wants-to-help-small-businesses-find-big-exits-with-ai-agents/

OffDeal是一家旨在帮助小企业通过AI代理找到最佳并购机会的初创公司。该公司由前投资银行家Ori Eldarov和前Meta工程师Alston Lin创立，是Y Combinator 2024年冬季批次的一部分。OffDeal专注于弥合传统并购服务的空白，通过自动化投资银行的工作，帮助小企业与机构买家对接并进行交易。

投融资方面，OffDeal近期完成了470万美元的种子轮融资，领投方为专注于AI的风险投资公司Radical Ventures。OffDeal计划利用这笔资金扩大其顾问团队并加强市场营销。该公司为其服务收取5%至10%的交易价值，与传统投资银行的收费标准相似，旨在通过AI驱动的顾问实现更多的交易。

公司官网：https://offdeal.io/

![](img/3ce83daad092ed777c1989d83a14ab42.png)

## Shopsense AI助力粉丝购买明星红毯同款

https://techcrunch.com/2024/09/12/shopsense-ai-technology-enables-music-fans-to-buy-dupes-inspired-by-red-carpet-looks-vmas/

Shopsense AI是一家利用AI技术帮助用户在观看娱乐节目时购买类似明星红毯造型的公司。该公司通过与Paramount合作，在2024年MTV音乐录影带大奖（VMAs）期间，观众能够通过该平台扫描明星造型并购买相似的服饰。

融资方面，Shopsense AI此前完成了220万美元的融资，投后估值达到2000万美元。其商业模式主要依赖广告和电商收入分成。公司通过按点击收费（CPC）和交易分成获取收入。例如，观众通过平台点击商品链接，Shopsense AI可从中获得部分收益。此外，该平台与品牌合作推出赞助的商品系列，如与Macy’s合作的VMAs在线店铺。

公司官网：https://www.shopsense.ai/

![](img/a409abdeff6b266e36e0cb705af7a5d9.png)

# 学习

## 如何看待 OpenAI 发布 o1 系列模型？将带来哪些变革？

贡献人：

https://www.zhihu.com/question/666991594?utm_psn=1817858863715725312

**Scaling Law的新维度**：除了数据量、模型大小、计算量外，推理时间的计算能力（inference time compute）也成为关键因素。这一更新引发了大模型推理速度的飞跃，尤其在处理复杂问题时表现突出。

**逐步思考训练（CoT）**：通过端到端试错的方式训练模型进行逐步思考，被证明比传统的提示方法更可靠，并能够大幅提升模型表现，特别是在处理长上下文和复杂任务时。

**推理时间和上下文长度的影响**：OpenAI的新模型o1在推理时间上有所延长，且相比4o模型，支持的上下文长度也增加了一倍。这带来更强的反思能力和复杂问题处理能力，但对提示的敏感度也更高。

**CoT和工程优化**：长期CoT被认为是最后的“低垂果实”，可以通过合成训练数据大幅提升模型性能。尤其是数学和代码任务，利用RWKV等RNN模型能够通过长期CoT提升表现。此外，推理时间的规模化也推动了工程上的改进。

![](img/0c1fb53cd8a6135bbba9250f8993600a.png)

## 一些稀疏化的工作

https://zhuanlan.zhihu.com/p/719773345?utm_psn=1817907686014066689

**DMC**：通过将 discard token 变为残差链接，实现 token 侧的稀疏。

**模型稀疏性**：

1. **SparseGPT**：针对现有剪枝方法计算成本过高的问题，SparseGPT提供了一种基于Hessian逆序列的剪枝方法。在给定剪枝掩码的情况下，通过更新权重矩阵的剩余权重来补偿剪枝带来的误差。

2. **Deja vu**：提出了一种动态剪枝 pipeline，基于上下文稀疏性预测模型中哪些神经元需要激活。对于MLP和MHA分别用不同方式来预测活跃神经元和 head，使用 layer-wise 的 token embedding 近似推理来实现加速。

3. **Sirius**：观察到稀疏性方法在推理任务（如GSM8k）中效果不佳，但通过修正少量错误 token 可以恢复正确率，这类似于 speculate decoding 的应用。

4. Adaptive methods：通过动态 head-wise 调整和分配权重，解决中间层检索不到的现象，提出了新的调整方法，如 PyramidKV 来优化 attention map。

**MInference 1.0**：使用硬件高效的模式组合 attention map，提升了计算效率，并提供了准确率的 benchmark，展现出在稀疏性优化中的潜力。

![](img/02836b8446e81e978d1ea4fc9b3d442d.png)

## xDiT加入diffusers文档

https://huggingface.co/docs/diffusers/main/en/optimization/xdit

`xDiT` 是一个专为大规模并行部署的扩散变换器（Diffusion Transformers, DiTs）设计的推理引擎，提供了多种高效的并行方法和 GPU 内核加速。`xDiT` 支持四种并行方法：统一序列并行（Unified Sequence Parallelism）、管道融合（PipeFusion）、CFG 并行和数据并行。这四种并行方法可以混合配置，以优化通信模式，适应底层网络硬件。

除了并行优化，`xDiT` 还侧重于单 GPU 的性能加速。通过引入 `torch.compile` 和 `onediff` 等编译加速技术，以及利用 Attention 优化库，进一步提升单 GPU 推理效率。

技术实现上，用户只需使用 `xFuserArgs` 从 `xDiT` 获取配置参数，并将其与 `Diffusers` 库的 `pipeline` 对象传递给 `xDiTParallel`，即可实现特定 `pipeline` 的并行化。此外，`xDiT` 支持使用 `torchrun` 命令在多节点、多 GPU 上进行推理。

例如，以下命令用于 8 GPU 的并行推理：

```
torchrun --nproc_per_node=8 ./inference.py --model models/FLUX.1-dev --data_parallel_degree 2 --ulysses_degree 2 --ring_degree 2 --prompt "A snowy mountain" "A small dog" --num_inference_steps 50
```

目前支持的模型包括 Flux.1、Stable Diffusion 3 等，性能基准可以在 `xDiT` 的 GitHub 页面上找到。

![](img/5608e520d2fffa9ebecf1a479226d24d.png)

## 使用 Imatrix 和 K-Quantization 进行 GGUF 量化以在 CPU 上运行 LLM

https://zhuanlan.zhihu.com/p/719927690?utm_psn=1817905793753833473

`GGUF` 是一种用于大型语言模型（LLM）的高效存储和快速加载的二进制文件格式，依赖 `GGML` 张量库。`GGUF` 文件封装了推理所需的所有组件，并支持多种模型格式（如 Llama 3、Phi、Qwen2）。尽管 `GGUF` 本身并不提供量化功能，但 `llama.cpp` 提供了多种量化算法，如 `K-Quantization`，用于减少模型大小并以 `GGUF` 格式存储。

`K-Quantization`（`K` 量化）通过将模型权重分成“超级块”并进一步细分为子块来提高精度。每个子块有各自的比例和最小值，用于保持模型准确性。该方法尽管较慢，但在推理过程中性能极佳。反量化过程非常简单，适合 GPU 和 SIMD 加速。

量化准确性可以通过重要性矩阵（`imatrix`）提高，该矩阵用于识别最关键的权重并避免量化这些权重，从而减少误差。生成 `imatrix` 需要模型对校准数据集进行前向传递，计算过程耗时较长，通常使用 GPU 加速。

基准测试表明，量化模型的困惑度接近于原始 `FP16` 模型，尤其是使用了重要性矩阵的模型。通过 `K-Quantization` 和重要性矩阵优化的 `GGUF` 模型在推理吞吐量和内存效率方面表现优异，特别是在 CPU 上的推理速度明显提高。

![](img/76592e07f19a4311873695254bf118ac.png)

## 大模型推理服务调度优化技术-Continuous batching

https://zhuanlan.zhihu.com/p/719610083?utm_psn=1817314936814198784

**KV Cache**：`GPT` 模型的推理分为两个阶段——预填充和解码。解码阶段中，每次推理输出一个 token，前后 token 产生冗余计算。为避免重复计算，已生成 token 的 `K` 和 `V` 值可以缓存，这就是 `KV Cache` 技术，它通过以内存换计算来提高推理速度，减少延迟。

**批处理模式**：

+ **静态批处理**：多个请求打包后一起处理，直到所有请求完成后才返回响应。这种方式由于等待较长请求完成，GPU 利用率较低。

+ **动态批处理**：允许多个请求动态打包，最大化 GPU 吞吐量，减少响应延迟。但在生成长度差异较大的情况下，性能提升有限。

+ **连续批处理**：适用于自回归大模型推理，通过在推理过程中动态加入新的请求，优化 GPU 资源利用率。该方法可提升 10-20 倍的吞吐量，是目前最先进的推理批处理技术。

**Ragged Batching**：在处理变长输入时，不需要填充到相同形状，通过设置输入允许不规则批次，实现高效批处理。`Triton` 推理框架中的 `Ragged Batching` 是此类技术的典型实现。

**ORCA 架构**：`ORCA` 提出了一种迭代级调度机制，通过分割请求并动态批处理，减少延迟并提升吞吐量。其独特之处在于选择性批处理，只对非注意力操作进行批处理，最大化 GPU 利用。




## 应用案例

多个推理框架如 `vLLM`、`DeepSpeed-FastGen`、`TensorRT-LLM`、`LMDeploy` 和 `LightLLM` 都已实现了Continuous Batching。每个框架在处理KV Cache、预填充与解码的优化上有所不同，如vLLM区分Prefill和Decode阶段，`DeepSpeed-MII`通过动态分割融合优化前向推理的大小，提升吞吐量与响应一致性。

![](img/217517a763ab366e304e1f2e1275b63e.png)

## 浅谈LLM mechanistic interpretability的几个流派

`https://zhuanlan.zhihu.com/p/719002810?utm_psn=1817598803068461056`

因果追踪（Causal Tracing）：又称路径修补或因果干预分析，主要通过改变模型某个位置的参数或向量，观察最终预测的变化来理解信息流。经典研究表明中间FFN层对知识表示重要，如在Locating and Editing Factual Associations in GPT中发现FFN层在事实知识任务中起关键作用，另有研究表明FFN早期层负责表示的丰富化，而注意力头提取知识。这一方法还可应用于其他架构，如 `Mamba` 模型。

电路分析（Circuit Analysis）：关注特定输入/输出路径上模型的重要参数，目标是以注意力头和FFN层为基本单元构建输入到输出的电路。研究发现，某些注意力头与上下文学习（in-context learning）相关，如induction head在该过程中的作用。通过分析 `GPT-2` 等模型的电路，研究者发现微调增强了已有的机制。

神经元分析（Neuron Analysis）：这一流派将FFN层的神经元作为基本单元，探讨单个神经元在预测中的角色。研究表明，FFN层可被视为多个神经元的加和，部分神经元在投影到词汇空间时具有强解释性。然而，神经元之间的叠加现象（superposition）削弱了这种可解释性，因而提出了如稀疏自编码器等方法以提高其解释能力。

## 通过打包 `Flash Attention` 来提升 Hugging Face 训练效率

`https://zhuanlan.zhihu.com/p/719693369?utm_psn=1817597530319499264`

### 背景与问题

在模型训练时，填充（padding）常用于统一输入序列长度，但这种方式会带来性能损失。打包示例能避免填充的效率低下问题。然而，之前的打包方式未处理好示例边界，导致出现不期望的跨示例注意力，影响模型的质量与收敛性。

### 解决方案

Hugging Face 引入了新的数据整理器 `DataCollatorWithFlattening`，它与 `Flash Attention 2` 兼容，能够在打包时保持对示例边界的处理。通过计算累积序列长度（`cu_seqlens`），该方法确保训练过程不会出现跨示例注意力问题，并提升训练吞吐量和内存效率。

### 吞吐量与内存改进

+ 在 FLAN 数据集上使用新的数据整理器，训练吞吐量提升了 2 倍，内存使用减少了 20%。

+ 在 OrcaMath 数据集上，吞吐量提升了 1.4 倍，内存减少了 6%。

### 收敛性保持

打包并不会影响训练收敛性。实验表明，使用打包的模型与使用填充的模型在验证损失上表现相同，保证了训练的效果不受影响。

### 支持的模型

目前已有多个模型支持这一新功能，包括 `Llama 2/3`、`Mistral`、`Granite` 等。这些模型都可以通过提供 `position_ids` 来实现与 `Flash Attention 2` 的兼容。

![](img/86f9178dd0ca02385b8821eb0d9659fb.png)

# 招聘公告

---

![](img/7cd620ce9b7570392ec0966d8cc89f3f.png)

奇绩创坛由陆奇博士于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

## 🚀F24实习岗位

##### [行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

##### 行研-GenAI研究岗位-战略分析（偏产品）

##### Web / AI 全栈开发工程师实习生

## 火热开放中🚀

##### 行研-GenAI研究岗位-前沿学术探索

##### 行研-GenAI研究岗位-战略分析（偏咨询）

##### 创新活动产品运营 （大模型系列活动产品运营）

> 点击相关岗位可查看对应岗位需求【JD】；相关岗位投递请添加🛰️💻【`l_ivresse29`】

> ‼️欢迎推荐/自荐，期待在奇绩创坛遇见更好的你‼️

---

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> 【日报知识库】这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/da38b15ab5ba1b92ec990b1e04b70966.png)
