# 奇绩大模型日报（7月 12日）

> 来源：[https://miracleplus.feishu.cn/docx/OY0UdmJL3oZ9QRx4JeuctiUVnvT](https://miracleplus.feishu.cn/docx/OY0UdmJL3oZ9QRx4JeuctiUVnvT)

# 推特

### GPT训练成本大幅下降：llm.c项目的五年成果与未来展望

https://x.com/karpathy/status/1811467135279104217

2019年，OpenAI发布了GPT-2，详见此帖：

https://openai.com/index/better-language-models/

今天（大约5年后），你可以花费大约$672，使用一台8XH100 GPU节点运行24小时，训练出自己的模型。我们最新的llm.c帖子详细介绍了具体步骤：

https://github.com/karpathy/llm.c/discussions/677

令人难以置信的是，由于计算硬件（H100 GPU）、软件（CUDA、cuBLAS、cuDNN、FlashAttention）和数据质量（例如FineWeb-Edu数据集）的改进，过去五年成本大幅下降。为了这次实验，算法保持不变，遵循GPT-2/3的论文。

因为llm.c是GPT训练在C/CUDA中的直接实现，要求很少——无需conda环境、Python解释器、pip安装等。你只需启动一个云GPU节点（例如在Lambda上），可选地安装NVIDIA cuDNN、NCCL/MPI，下载.bin数据分片，编译并运行，几分钟内即可开始。然后等待24小时，就可以享受关于安第斯山脉的会讲英语的独角兽的样本了。

对我来说，这是一个非常不错的阶段性成果，因为整个llm.c项目的起点是我想为一个教育视频重现GPT-2，结果被一些PyTorch问题卡住了，然后一怒之下决定从头用C/CUDA编写整个程序。这让我走上了一条比预期更长的旅程，但也很有趣，我学到了更多CUDA知识，结交了朋友，llm.c现在真的很棒。它大约有5000行代码，编译和运行速度非常快，所以几乎没有等待时间，它具有恒定的内存占用，采用混合精度训练，在多节点分布式上运行，具有位级确定性，并保持在约50%的MFU。所以它非常可爱。

llm.c能够达到现在的水平，离不开一群从互联网汇集而来的优秀开发者，特别是ademeure、ngc92、@gordic_aleksa 和 rosslwheeler的帮助。还要感谢@LambdaAPI提供的GPU周期支持。

还有很多工作要做。我对目前的运行还不是100%满意——评估应该更好，尤其是对于更大模型尺寸的长时间运行，训练应该更稳定。还有很多有趣的新方向：fp8（即将到来！）、推理、微调、多模态（VQVAE等）、更现代的架构（Llama/Gemma）。llm.c的目标仍然是拥有一个简单、最小、干净的全功能LLM代理训练栈，直接在C/CUDA中实现，并配以伴随的教育材料，让更多人能够进入这个令人惊叹的领域。

眼睛的甜点：我更长的400B token GPT-2运行（从33B tokens提高），一直到330B（达到61% HellaSwag，远高于同尺寸的GPT-2和GPT-3）都很好，之后在这个图表后不久崩溃了，我正在研究这个问题 :)

![](img/a7c5372c2ba3e6b99d42c5c130f8e96a.png)

### 吴恩达再谈SB 1047：对开源和AI创新的威胁

https://x.com/AndrewYNg/status/1811425437048070328

我对加州拟议的SB 1047法规的进展感到震惊。这项法律错误地将AI技术作为监管对象，而不是AI应用，从而无法有效提高AI的安全性。以下是SB 1047对开源的具体危害。

有些监管措施确实有助于提高安全性。例如，我支持禁止未经同意的深度伪造色情内容，标准化水印和指纹技术以识别生成内容，以及加大对红队和其他安全研究的投资。但不幸的是，这项拟议法案选择了一条更有害的道路。

SB 1047旨在确保AI模型的安全性。它对微调模型或开发训练成本超过1亿美元的模型的开发者设立了复杂的报告要求。这是一部含糊不清的法律，对违规行为施加重罚，导致开发者无法确定如何避免违法，这将使许多团队陷入瘫痪。

你可以在线阅读法律草案，我仔细阅读后发现其模糊且难以理解。

开发者在试图遵守复杂的法律要求时面临巨大个人风险。法律要求开发者在遵守法律的证明文件上签字，一旦违反可能面临伪证罪的处罚。但当法律要求复杂、难以理解且可能随非选举产生的机构的意愿而变化时，如何确保我们遵守呢？

例如，证明文件必须包括许多不同部分，其中之一是对“模型可能合理造成或启用的关键危害的性质和程度”的分析。但即使是顶尖的AI研究人员也不确定模型可能造成或启用哪些危害，那么开发团队如何确定这一点并在伪证罪的威胁下声明他们符合要求呢？

此外，某些开发者将被要求实施“保护措施，以防止对模型及其衍生模型的误用或不安全的后期修改，适合于模型相关风险，包括高级持续威胁或其他高级攻击者。”即使是顶尖的AI研究人员也不同意如何最好地“保护”AI模型免受这些风险，或什么是“适当的”。那么开发者如何确定如何遵守这一要求？

这为开发者创造了一个可怕的局面。伪证可能导致罚款甚至监禁。一些开发者将不得不雇佣昂贵的律师或顾问来指导他们如何遵守这些要求。其他人则会避免发布尖端AI产品。

如果法律通过，对陪审团审判的恐惧将非常真实——陪审团的判决可能非常不可预测，一旦被定罪，惩罚可能非常严厉。如果有人今天在采取他们认为合理的保障措施后发布模型，但几年后，当对AI技术的看法可能发生变化时，一些积极的检察官成功说服陪审团，他们当时的做法在事后看来并不“合理”怎么办？

“合理性”是模糊的，其法律解释可能依赖于判例法、陪审团指示和共同事实等因素。这使得很难确保开发者今天的行为在未来的陪审团看来是合理的。

加剧这一模糊性，法案将创建一个五人组成的边境模型司（FMD），拥有对开发者制定标准的权力。这个小委员会将成为游说和监管俘获的绝佳目标。未选举产生的FMD可以对开发者征收费用以支付其成本。它可以任意改变微调模型的计算门槛，从而要求甚至小团队也需要雇佣审计员检查其是否符合模糊的安全标准。

这些条款并不能确保AI的安全。它们创造了监管不确定性，并为希望扼杀开源的既得利益集团提供了更多游说机会，以推动要求变化，提高合规成本。这将锁定许多没有收入来源的团队——特别是许多开源贡献者——使他们无法支付游说者、审计员和律师费用，以确保他们遵守这些模糊和不合理的要求。

开源是一个美好的力量，正在为许多人带来知识和工具，是AI创新的关键支柱。我对针对开源的有组织攻击感到沮丧。毫无疑问，加州正在为开源的未来健康进行斗争。我致力于尽我所能保护开源，但我并不认为支持开源的一方会一定获胜。希望你能和我一起反对SB 1047和其他威胁开源的法律。

[原文链接 (带链接)：https://deeplearning.ai/the-batch/issue-257/]

![](img/00418229f7ee6243ef784cd30237be9f.png)

![](img/a579317ee67d08eb4cb436a6217542fb.png)

![](img/7e7bc5d9a6d9c876412565186607bb94.png)

![](img/22cb90f080af383bc143a62fda71d915.png)

### Lynx——领先的幻觉检测模型：在幻觉任务上超越GPT-4o

https://x.com/PatronusAI/status/1811445716457316848

介绍Lynx——领先的幻觉检测模型 🚀👀

+   在幻觉任务上超越GPT-4o

+   开源、开放权重、开放数据

+   在医学和金融等实际领域表现出色

我们很高兴与第1天的集成合作伙伴一起推出Lynx：

@nvidia、@MongoDB 和 @nomic_ai🔥

![](img/deb4647983e7180f068a79f970b48dda.png)

[ssstwitter.com_1720789063922.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FbV0b6i0soVTjJx4FkiclaBGnMh)

### OpenDiLoCo：开源实现和扩展DeepMind的分布式低通信方法，支持全球分布式AI模型训练

https://x.com/samsja19/status/1811450791900901853

介绍OpenDiLoCo，这是一种开源实现和扩展DeepMind的分布式低通信（DiLoCo）方法，支持全球分布式AI模型训练。

https://primeintellect.ai/blog/opendiloco

---

非常兴奋地介绍我们的Open Diloco项目。

我们在3个国家训练了一个10亿参数的模型，带宽不到100mb/s（比Infiniband慢10,000倍），使用torch FSDP和hivemind的混合代码，实现了90%-95%的计算利用率。

我们希望打破封闭源模型在巨型集群上训练的局面，实现多个小型数据中心之间的开源联合训练模型。

OpenDiloco是我们朝着这个目标迈出的第一步，我们还有很多工作要做，从高效设计更智能的并行化技术到研究需要更少通信的新算法。

![](img/1063c648529133022b7db9180fa3ff39.png)

[ssstwitter.com_1720789161747.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TtJdb7DjIo3QrpxV8dEcE8zgnJd)

### Fireworks AI获Sequoia 领投5200万美元B轮融资

https://x.com/lqiao/status/1811500361485517153

Fireworks AI 完成了由 @sequoia 领投的5200万美元B轮融资！

此轮融资推动了我们增强推理平台并引领复合AI系统转型的使命。特别感谢我们的投资者 @nvidia、@AMD、@MongoDB、@benchmark、Sheryl Sandberg、Frank Slootman、@databricks、@alexandr_wang、@howietl、Sajith Wickramasekara 和来自天使轮、A轮和B轮的 @ClementDelangue。

Fireworks 目前提供超高速且成本高效的推理引擎，数万名开发者使用该引擎访问最新模型，以构建创意产品、新体验和生产力工具。

我们将利用这笔新资金来增强我们的推理引擎，并扩展我们的生态系统，以采用来自 Nvidia 和 AMD 的最新硬件，并与 MongoDB 等数据库进行更深入的集成。我们的平台增强包括最新的模型、先进的定制选项和改进的企业功能。这些进步使企业无需大量的机器学习工程师或科学家就能更轻松、更快速地定制模型和构建AI应用程序。

此外，我们还将加速向复合AI系统的转型。在我们的推理平台之上，我们已经建立了将复杂业务任务分解为多个步骤的系统，访问跨多种模态（文本、音频、图像等）的多个模型、检索器和外部工具。我们将继续扩展这些能力。这使得 Fireworks AI 成为开发人员和企业构建新型颠覆性产品和体验的首选解决方案。

![](img/6ef6fbaded55f043d6681e4b90539ebb.png)

![](img/9558653b551051e5eb57a59a09360b58.png)

# 论文

### FlashAttention-3: 异步和低精度快速准确的注意力

贡献人：

链接：http://arxiv.org/abs/2407.08608v1

摘要: Transformer架构中的核心层Attention是大语言模型和长上下文应用的瓶颈。FlashAttention通过最小化内存读/写来加速GPU上的Attention。然而，FlashAttention-2仅在H100 GPU上达到了35%利用率。我们开发了三种技术来加速Hopper GPU上的Attention：利用Tensor Cores和TMA的异步性来重叠计算和数据移动，交错块状矩阵乘和softmax操作，块量化和不连贯处理，以利用FP8低精度的硬件支持。我们展示了我们的方法FlashAttention-3在H100 GPU上实现了1.5-2.0倍的加速，FP16达到了740 TFLOPs/s（75%利用率），FP8达到了接近1.2 PFLOPs/s。我们验证FP8 FlashAttention-3比基线FP8 attention的数值误差降低了2.6倍。

![](img/24455daf82d15bd659a27fe139f69f8d.png)

### **Lynx：一个开源幻觉评估模型**

贡献人：

链接：http://arxiv.org/abs/2407.08488v1

摘要：检索增强生成（RAG）技术旨在减轻大型语言模型（LLM）中的幻觉。然而，LLM仍然可能产生不受支持或与检索内容相矛盾的信息。我们引入了LYNX，这是一个领先的幻觉检测LLM，能够对具有挑战性的现实世界幻觉场景进行高级推理。为了评估LYNX，我们提出了HaluBench，一个包含15k个样本的全面幻觉评估基准，样本来源于各种现实世界领域。我们的实验结果表明，LYNX在HaluBench上优于GPT-4o、Claude-3-Sonnet以及闭源和开源LLM作为评判模型。我们公开发布LYNX、HaluBench和我们的评估代码供公众访问。

![](img/21b825ffcdfc4bf30a8cc0dd65c94dc6.png)

### **Skywork-Math：大语言模型中数**学推理的数据缩放定律 -- 故事还在继续

贡献人：

链接：http://arxiv.org/abs/2407.08348v1

本文探讨了潜在增强大语言模型(LLMs)数学推理能力的基本因素。我们认为在现代LLMs中，数学推理能力的数据缩放规律远未饱和，突显了模型质量随数据量增加而提高。为了支持这一观点，我们介绍了Skywork-Math模型系列，通过我们提出的250万实例的Skywork-MathQA数据集对常见的7B LLMS进行监督微调（SFT）。Skywork-Math 7B 在竞赛级MATH基准上达到了51.2%的令人印象深刻的准确率，并且在GSM8K基准上达到了83.9%的准确率，仅使用SFT数据，在MATH基准上胜过了GPT-4的早期版本。Skywork-Math模型的卓越性能源于我们新颖的两阶段数据合成和模型SFT管道，其中包括三种不同的增强方法和一个多样化的种子问题集，确保了Skywork-MathQA数据集在不同难度级别上的数量和质量。最重要的是，我们提供了一些实用的经验教训，以增强LLMs的数学推理能力，适用于研究和工业应用。

![](img/77240fc01f4d8bad93e1d6035ea70f9a.png)

### 通过模型融合来缓解语言迁移中的灾难性遗忘

贡献人：

链接：http://arxiv.org/abs/2407.08699v1

摘要：随着开放权重的大型语言模型 (LLMs) 在英语中的各种任务中表现出越来越出色的性能，从业者们希望将这些模型调整到不同的语言中。然而，这种语言适应通常伴随着基础模型能力的灾难性遗忘，严重限制了生成模型的实用性。我们提出了一种新的适应方法 Branch-and-Merge（BaM），它基于迭代地合并在可用训练数据子集上微调的多个模型。BaM基于这样一个洞察力提出，即这样会产生更低幅度但更高质量的权重变化，降低源域遗忘，同时保持目标域的学习。我们在保加利亚语和德语上进行了广泛的实证研究，结果表明BaM能够显著减少遗忘，与标准继续预训练和指导微调相比，能够匹配甚至提高目标域性能，跨不同的模型架构。

![](img/d4465f6bc3ffd8777db2e2a66822c4ff.png)

### AutoBencher: 为语言模型创建显著、新颖、困难的数据集

贡献人：

链接：http://arxiv.org/abs/2407.08351v1

评估是评估能力、跟踪科学进展和指导模型选择至关重要的。在本文中，我们提出了智能体语言模型良好基准的三个要点：(i)显著性（例如，了解二战比了解历史上的某一天更显著），(ii)新颖性（即，该基准显示了之前基准未展示的模型排名新趋势），以及(iii)困难性（即，基准对于现有模型应具有挑战性，为未来改进留下空间）。我们对这三个要点进行具体操作，并将基准创建视为一个搜索问题，即寻找满足这三个要点的基准。为了解决这一搜索问题，我们提出了AutoBencher，利用语言模型自动搜索符合这三个要点的数据集。AutoBencher利用特权信息（例如相关文档）构建可靠数据集，并通过重新排序实现对搜索目标的优化。我们使用AutoBencher为数学、多语言和知识密集型问答创建数据集。AutoBencher的可扩展性使其能够测试微粒级类别和尾部知识，创建的数据集平均比现有基准更具新颖性和难度更大22%。对我们构建的数据集进行更详细调查显示，我们能够识别语言模型在特定领域的知识缺口，而这些缺口并未被现有基准所捕捉，例如Gemini Pro在关于二叠纪灭绝和福特主义的问题回答中表现较差，而OpenAGI-7B在关于COVID-19的问答中表现出奇的优秀。

![](img/a1760e7de0037ccecc83d6f1519d002e.png)

### Token-Mol 1.0：基于大语言模型的 Token 化药物设计

贡献人：

链接：http://arxiv.org/abs/2407.07930v1

最近，利用基于序列的大型语言模型（LLMs）进行药物设计引起了广泛关注。然而，目前大多数LLMs在药物发现中的应用缺乏理解三维结构的能力，从而限制了它们在明确涉及分子构象的任务中的效果。在本研究中，我们介绍了Token-Mol，一个仅基于标记的三维药物设计模型。该模型将所有分子信息，包括二维和三维结构，以及分子属性数据，编码为标记，将药物发现中的分类和回归任务转换为概率预测问题，从而实现通过统一范式进行学习。Token-Mol建立在Transformer解码器架构上，并使用随机因果屏蔽技术进行训练。此外，我们提出了高斯交叉熵（GCE）损失函数，以克服回归任务中的挑战，显着增强LLMs学习连续数值的能力。通过微调和强化学习（RL）的结合，Token-Mol在各种下游任务中实现了与现有基于任务的方法相媲美甚至超越的性能，包括基于口袋的分子生成、构象生成和分子属性预测。与现有分子预训练模型相比，Token-Mol在处理更广泛的与药物设计相关的下游任务方面表现出卓越能力。值得注意的是，与类似的仅标记方法相比，我们的方法将回归任务的准确性提高了约30%。Token-Mol克服了仅标记模型的精度限制，有潜力与ChatGPT等通用模型无缝集成，为发展一种能够提高专家快速高质量药物设计的通用人工智能药物设计模型铺平了道路。

![](img/9df737c280faa88efc0acb0b63f7250f.png)

# 产品

### AIEditor

贡献人：

https://aieditor.dev/

AiEditor 是一款开源的 AI 驱动富文本编辑器，解决传统编辑器在隐私、许可费用和功能定制等方面的问题。它支持开发者使用私有 API 密钥，无需担心 GPL 许可带来的限制，同时提供了包括 AI 文本润色、续写等在内的丰富功能，并可由开发者自行定制。AiEditor 基于 Web Component 技术，兼容主流前端框架，为需要富文本编辑功能的 AI CMS、知识库等场景提供了一个灵活、高效、私密的解决方案。

![](img/61b277a3c5ad7294d53186ac630c8004.png)

### AnyoneCanAI

贡献人：

https://www.anyonecanai.io/

AnyoneCanAI 是一个帮助创造者创新和设计 AI 体验的强大工具。它提供了 AI 创意室、设计思维原则和跨领域解决方案，简化 AI 产品开发的复杂过程，促进团队之间的无缝协作和创新。

![](img/1f1b9b0720f6e4d3e05b97d2de7218ba.png)

# HuggingFace&Github

### FoleyCrafter

贡献人：

https://foleycrafter.github.io/

现有方法在同时合成高质量和视频对齐的声音（语义相关和时间同步）方面存在局限性。为了克服这些限制，该项目提出了 FoleyCrafter，这是一个新的框架，利用预训练的文本到音频模型来确保高质量的音频生成。FoleyCrafter 包括两个关键组件：语义适配器用于语义对齐，时间控制器用于精确的音频-视频同步。语义适配器使用并行交叉注意力层来根据视频特征调节音频生成，产生与视觉内容语义相关的逼真音效。时间控制器结合了 onset 检测器和基于时间戳的适配器，实现了精确的音频-视频对齐。FoleyCrafter 的一个显著优势是它与文本提示兼容，可以使用文本描述来实现可控和多样的视频到音频生成，满足用户需求，并通过大量的定量和定性实验，证明了 FoleyCrafter 的有效性。

![](img/c4e2b552251e2bb123028242d6f618b3.png)

### LLMGA

贡献人：

https://github.com/dvlab-research/LLMGA

LLMGA 是一种基于多模态大型语言模型的生成助手，利用大型语言模型的知识和能力帮助用户进行图像生成和编辑。它提供了详细的语言生成提示来精确控制稳定扩散模型，增强了上下文理解，减少了噪音，生成了更精细复杂的内容，并提高了网络的可解释性。LLMGA 包括多个训练阶段，利用大量多模态数据训练 MLLM 掌握图像属性，生成详细提示，然后优化 SD 模型以符合 MLLM 的提示。此外，还提出了基于参考的修复网络减轻内外画过程中生成与保存区域的差异。LLMGA 展现出强大的生成和编辑能力，支持多种功能，包括文本到图像、修复、外绘、基于指令的编辑，以及多语言支持和灵活扩展。

![](img/a6285acee9c2d1396f1571f798b3a389.png)

# 投融资

### 软银收购英国AI芯片制造商Graphcore

贡献人：

https://techcrunch.com/2024/07/11/softbank-acquires-uk-ai-chipmaker-graphcore/

日本软银正式收购了英国芯片公司Graphcore，但交易细节未公开。Graphcore成立于2016年，开发了专为AI工作负载设计的智能处理单元（IPU）。尽管Graphcore曾获得约7亿美元融资，并拥有微软和Sequoia等大牌投资者，但由于资源密集的AI硬件业务未能达到预期高度。软银的收购预计将有助于Graphcore在AI领域的发展，且公司计划在英国增加员工。

公司官网：https://www.graphcore.ai/

![](img/d5975b8640677cdde084487a64fc6591.png)

### Medal筹集1300万美元，打造桌面AI平台

贡献人：

https://techcrunch.com/2024/07/11/medal-raises-13m-as-it-builds-out-a-new-ai-platform-for-desktop/

Medal公司宣布已筹集1300万美元，估值达3.33亿美元，投资者包括Horizons Ventures、OMERS Ventures、Peak6和Arcadia Investment Partners。公司推出了Highlight，一款跨平台桌面应用，作为用户的上下文AI助手。此应用捕捉屏幕内容，并根据上下文与大型语言模型互动。Medal计划通过应用商店模式和高级订阅进行盈利。

公司官网：https://medal.tv/zh-Hans

![](img/6719808d41129141e8036864524b1569.png)

### HerculesAI获得2600万美元B轮融资

贡献人：

https://techcrunch.com/2024/07/11/herculesai-has-been-working-with-llms-long-before-it-was-cool/

HerculesAI（前称Zero Systems）专注于自动化专业服务，自2017年以来一直构建大型语言模型。公司宣布获得2600万美元B轮融资，以继续推动其业务增长。此轮融资由Streamlined Ventures领投，Proof VC、Thomson Reuters Ventures、Alumni Ventures及多位行业天使参与。HerculesAI目前在法律、保险和金融服务领域提供智能数据提取、数据转化和数据验证等关键功能。

公司官网：https://hercules.ai/

![](img/3798b6caf8d3c4d5c5c58439cfab863c.png)

### 国防AI初创公司Helsing融资4.87亿美元C轮，计划波罗的海扩展以应对俄罗斯威胁

贡献人：

https://techcrunch.com/2024/07/11/defence-ai-startup-helsing-raises-487m-series-c-plans-baltic-expansion-to-combat-russian-threat/

国防AI初创公司Helsing宣布完成4.87亿美元C轮融资，领投方为General Catalyst。Helsing计划在未来三年内投资7000万欧元在波罗的海防御项目上。该公司总部位于德国，并在爱沙尼亚成立了新实体，旨在通过扩展其AI能力和扩大员工队伍来应对俄罗斯的威胁。Helsing开发的AI软件用于处理防御系统信息，提高无人机和战斗机的武器能力，并改进战场决策。

公司官网：https://helsing.ai/

![](img/0eca4ac01c32386240e8ad7487942832.png)

# 学习

### FlashAttention-3 发布！有什么新优化点？

贡献人：

https://www.zhihu.com/question/661395457/answer/3559517523

FA3 利用 NVIDIA Hopper 架构的新特性，如 WGMMA（Warpgroup Matrix Multiply-Accumulate）和 TMA（Tensor Memory Accelerator），以及 FP8 低精度支持，大幅提升了注意力机制的计算效率。FA3 通过在 warp 组级别和 warp 内部级别重叠 GEMM 和 Softmax 计算，有效地提高了 GPU 的利用率，实现了 H100 理论最大 FLOPS 的 75% 利用率。在精度方面，FA3 采用了不相关处理和哈达玛变换技术来减少 FP8 量化误差，这对于保持模型精度同时加速计算至关重要。这些优化不仅使得处理长序列成为可能，而且为大型语言模型的快速训练和推理提供了支持，预示着人工智能模型在处理复杂任务时的更高效率。答主们期待 FA3 的技术能够在未来被广泛应用于各种硬件架构和深度学习框架中。

![](img/779764a3dc8656719dc1becf9317792a.png)

### 了解FlashAttentionV3的优化需要先了解Hopper的主要技术（Hopper White Paper概述）

贡献人：

https://zhuanlan.zhihu.com/p/708416319

NVIDIA Hopper GPU 架构通过引入第四代 Tensor Core，提供了 6 倍的整体加速，并在单个 SM 上加倍了 MMA 计算速度。新的 DPX 指令使得动态规划算法加速了 7 倍。IEEE FP64 和 F32 分别实现了 3 倍和 2 倍的加速。Thread block clusters 特性和 Distributed shared memory 实现了多个 SMs 上的并发运行和同步，扩大了 shared memory 的使用范围。异步执行特性的 Tensor Memory Accelerator（TMA）单元优化了数据传输效率。Transformer Engine 支持 Fp16 和 Fp8 的自动切换，训练和推理性能分别提升了 9 倍和 30 倍。HBM3 内存带宽提升了 2 倍，L2 缓存扩展到 50MB。第二代 MIG 提升了每个 GPU Instance 的带宽和计算能力。第四代 NVIDIA NVLink 和第三代 NVSwitch 提供了更高的带宽和吞吐量。PCIe Gen5 支持 128GB/sec 的双向带宽。这些技术细节展现了 Hopper 架构在提升计算效率、内存带宽、以及支持更大规模并行计算方面的突破。

![](img/d5b77df797f5d5c088e414f299f303a6.png)

### FlashAttention-3: Fast and Accurate Attention with Asynchrony and Low-precision博客翻译

贡献人：

https://zhuanlan.zhihu.com/p/708409249

FlashAttention-3 是针对大型语言模型和长上下文应用中的瓶颈问题 —— 注意力机制 —— 提出的一种新的加速技术。它在 FlashAttention 和 FlashAttention-2 的基础上，进一步优化了对 H 系列架构的支持，通过异步计算和低精度 FP8 的使用，实现了 GEMM（通用矩阵乘法）和 Softmax 操作的重叠，以及减少量化误差的不一致处理技术。与 FlashAttention-2 相比，FlashAttention-3 在 FP16 下的速度提高了 1.5-2.0 倍，达到了 H100 GPU 理论最大 FLOPS 的 75% 利用率，并且在 FP8 下接近 1.2 PFLOPS 的性能，错误率仅为基准 FP8 注意力的一半。这些改进不仅提高了 GPU 的利用率，还使得在低精度下的性能更佳，并且能够处理更长的文本上下文，为大规模 AI 操作的客户带来了成本节约和效率提升的潜力。FlashAttention-3 的代码已经开源，可以在 GitHub 上获取。

![](img/601ed3699ca09cbabd43a012e7f0aac9.png)

### 可商用的DiT模型AuraFlow

贡献人：

https://zhuanlan.zhihu.com/p/708509545

AuraFlow 是一个基于 flow 的文本到图像生成模型，由 fal.ai 公司开源，采用 Apache-2.0 许可证，使其可商用。该模型参数量为 6.8B，使用了 MMDiT 架构，但在后续层次中直接采用了 DiT block。AuraFlow 的文本编码器使用的是 T5-XL，而非 CLIP，且使用了 EleutherAI 的 pile-t5-xl 进行预训练，该模型在 GenEval 上的得分为 0.64，可通过提示词增强到 0.703。AuraFlow 已经集成到 diffusers 库中，可以直接使用，并提供了 Python 代码示例。

![](img/0b65f03bc52b015482f36b047bb97184.png)

### 决策智能与强化学习（5）：神经网络的可塑性、学习动态与泛化

贡献人：

https://zhuanlan.zhihu.com/p/708244682

深度强化学习（DRL）面临的关键挑战包括神经网络可塑性的丧失、学习动态的复杂性以及泛化能力的局限性。可塑性的丧失与损失景观的曲率变化有关，影响了模型的适应性和鲁棒性。为了解决这一问题，研究提出了 plasticity injection 技术，这是一种最小化干预的方法，能够在不增加可训练参数数量的情况下提高网络的可塑性。此外，TD 学习中的干扰现象对模型的泛化能力有显著影响，需要通过优化学习动态来缓解。表征学习在提升模型性能方面也展现出重要作用。通过这些研究成果，DRL 系统的性能和鲁棒性得到了提升，为未来的研究方向提供了指导。

![](img/c2e954ed389512b61c772525a7de3d7b.png)

### OSDI 2024 论文评述 Day 1 Session 2: Low-Latency LLM Serving

贡献人：

https://zhuanlan.zhihu.com/p/707990822

OSDI 2024 论文评述涵盖了五篇关于优化大型语言模型（LLM）推理服务性能的论文。首先，Sarathi-Serve 系统通过分块预填充和无停顿批处理技术，提高了 LLM 推理服务的性能，实现了高吞吐量与低延迟的协调。其次，ServerlessLLM 项目针对 Serverless 平台上 LLM 的冷启动开销进行了优化，通过增加 checkpoint 在多层存储中的局部性和动态迁移策略，减少了模型加载时间。InfiniGen 系统则解决了 KV 缓存管理问题，通过在 CPU 上保留所有 KV 缓存条目并动态预取重要条目，提升了性能和精度。Llumnix 框架针对跨实例调度优化，通过运行时重调度机制，提高了性能隔离和内存碎片化问题的处理。最后，DistServe 框架通过分离预填充和解码阶段，优化了模型参数的划分，实现了更高的好吞吐量。这些研究成果共同推动了 LLM 推理服务的性能提升和资源利用率的提高。

![](img/4ab55c8474ed4bce90d0726d29980b62.png)

### 2024年RDMA网络与AI白皮书

贡献人：

https://650group.com/blog/rdma-networks-are-a-key-enabler-to-ai-ml-deployments/

RDMA（远程直接内存访问）技术允许两台服务器在不经过处理器、缓存或操作系统的情况下读取和/或写入对方的内存，从而提高数据传输速度并降低延迟。RDMA主要用于高性能计算（HPC）集群和AI/ML部署。RoCE（基于以太网的RDMA）在以太网上实现内存交换，提升了网络性能。AI/ML服务器市场预计到2028年将增长至600万台，RDMA交换市场将超过180亿美元。RDMA显著减少了作业完成时间（JCT），提升了AI/ML集群的性能和效率。

白皮书下载网页：https://650group.com/reports/2024-rdma-networking-and-ai-report/

![](img/32efe160941f5a37ee9e1cce1ad105b5.png)

### 扩散模型解读 (十三)：Q-DM 低比特扩散模型

贡献人：

https://zhuanlan.zhihu.com/p/687271365

本文是对扩散模型进行量化的先驱工作之一，提出了一种名为 Q-DM 的低比特量化方法。量化技术通过使用低比特精度参数和运算来显著减小内存消耗和计算消耗。Q-DM 方法通过 Timestep-aware Quantization (TaQ) 减少输出分布的振荡，并通过 Noise-estimating Mimicking (NeM) 策略来减小累积误差。作者在 CIFAR-10 和 ImageNet 上做了验证，结果表明 Q-DM 相比于 8 位 Post-Training Quantization (PTQ) 方法，性能有大幅提升，并以相当大的加速率实现了与全精度模型相当的性能。

文章详细介绍了扩散模型的前向和反向过程，以及如何将量化技术应用到扩散模型中。量化过程涉及对卷积权重和输入特征的量化，以及如何通过量化来近似浮点卷积输出。作者指出，扩散模型量化面临两大挑战：训练过程中激活值的分布振荡，以及推理时多步去噪过程产生的累积量化误差。为了解决这些问题，文章提出了 TaQ 和 NeM 两种策略。TaQ 通过考虑 time step 的量化注意力机制来减轻激活值的分布振荡，而 NeM 通过全精度模型的蒸馏来减轻量化误差的累积。实验结果显示，Q-DM 在 DDPM 和 DDIM 两个框架上的表现优于现有的量化方法，尤其在低比特量化（如 4/3/2-bit）的情况下，显著提高了模型的性能和效率。

### AI会是音乐行业的《命运交响曲》吗？｜AI&Society创造营2万字实录

贡献人：

https://mp.weixin.qq.com/s/MyWvvz7jUZLgD6aQDapRRg

在 5 月 7 日举办的主题为 “AI 是音乐行业的《命运交响曲》吗？” 的腾讯研究院线上沙龙上，四位来自音乐行业各个领域的专家讨论了人工智能技术如何在音乐领域带来创新和变革。随着生成式 AI 如 Suno 和 StableAudio 的发展，音乐创作的门槛降低，每个人都有可能成为音乐创作者，这不仅影响了创作领域，也可能对音乐服务和产业产生冲击。沙龙上的嘉宾包括音乐教育家赵易山、钢琴演奏家翁雷、DeepMusic 灵动音科技的创始人刘晓光以及 DJ 杨樾，他们从不同角度探讨了 AI 音乐的现状、挑战、机遇和对音乐教育的影响。

嘉宾们认为，AI 生成音乐可能会改变未来的娱乐体验，但它的商业机会可能包括工具变现、唱片公司模式以及与音乐教育和游戏的结合。AI 音乐的传播模式将决定其能否广为流传，而不是生产方式。在 AI 时代，音乐行业应关注如何产生更多经典作品，以触动人心并创造好的市场。AI 在音乐教育中起到重要作用，可以帮助孩子更好地理解和感受音乐，培养他们的艺术感觉。音乐对人的人格培养非常重要，现场音乐会的体验对孩子的成长有很大益处。家长应鼓励孩子多听音乐，发掘他们的兴趣，但不必强迫他们学习乐器。

嘉宾们进一步讨论了 AI 音乐的技术原理，包括音频模型的开发路线和训练方式，以及 AI 音乐对音乐服务和产业的潜在影响。他们认为，AI 音乐可能对现有音乐产业的大生态没有太大影响，但可能会改变音乐实践领域，并在教育和游戏领域提供新的商业机会。AI 音乐的生产和传播都需要新的商业模式，例如自动化的电台音乐编排和 AI 与教育产品的结合。在 AI 时代，音乐的学习和听觉体验都将得到改善，家长在培养孩子学音乐时应该更多地关注孩子的兴趣和音乐的多样性，而不是强迫孩子按照传统方式学习。

![](img/4272486485524a0a4ca69433fd5cfed2.png)

### 硅基流动开源ComfyUI节点：没有GPU也能跑可图Kolors

贡献人：

https://mp.weixin.qq.com/s/Nprfsl11lCqHjUWA4trdhg

硅基流动团队开源了 BizyAir 节点，专为 ComfyUI 设计，使得用户无需 GPU 即可运行可图 Kolors 模型。这个节点通过 OneDiff 推理引擎优化了图片生成过程，显著提升了效率。BizyAir 支持一键安装，并集成了 SiliconCloud LLM 节点，允许大型语言模型如 GLM4、DeepSeek V2 Chat 在 ComfyUI 中提供文本生成和提炼服务。可图 Kolors 模型采用 ChatGLM3 作为文本编码器，支持中英文双语，具备强大的语义理解能力。硅基流动的解决方案降低了技术门槛，使得更多用户能够轻松体验到高级 AI 技术的魅力。用户可以通过设置 API 密钥快速启动，并利用 BizyAir 提供的各种节点进行文生图和图生图创作。

![](img/5e57861c2f3066a4aced5f5badb2dc18.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

##

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报****知识库****】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/cf6d009187e8bc8ae01f0d1ef696a13b.png)