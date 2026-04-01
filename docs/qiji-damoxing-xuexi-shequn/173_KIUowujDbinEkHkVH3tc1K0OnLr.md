# 奇绩大模型日报（6月 21日）

> 来源：[https://miracleplus.feishu.cn/docx/GgBWdsX9gozCPKxN7OjcT3zcnsd](https://miracleplus.feishu.cn/docx/GgBWdsX9gozCPKxN7OjcT3zcnsd)

[「奇绩大](https://www.feishu.cn/community/article/wiki?id=7355065047338450972)模型日报」知识库现已登陆飞书官方社区：飞书社区 点击「订阅社区精选」，即可在飞书每日收到《大模型日报》每日最新推送

# 推特

### Claude 3.5 Sonnet 上线：迄今为止最智能的模型

https://x.com/AnthropicAI/status/1803790676988920098

介绍Claude 3.5 Sonnet——我们迄今为止最智能的模型。

这是我们3.5模型系列的首次发布。

Sonnet现在在关键评估中表现优于竞争对手的模型，其速度是Claude 3 Opus的两倍，成本仅为五分之一。

免费试用：http://claude.ai

![](img/ce127f22ff93adfd14920bdd6ada5bd3.png)

### Karpathy分享Micrograd：标量值的自动求导引擎

https://x.com/karpathy/status/1803963383018066272?s=46&t=GRStLXDcUNuun8J5Noyw4Q

这些94行代码是训练神经网络所需的一切。其他的只是为了提高效率。

这是我早期的项目Micrograd。它实现了一个标量值的自动求导引擎。你从叶节点（通常是输入数据和神经网络参数）开始，用类似+和*的操作来构建一个计算图，计算图的末端是一个单一的值（损失）。然后你通过图的反向传播，应用链式法则来计算每个节点的梯度。梯度告诉你如何调整参数以减少损失（从而提高网络性能）。

有时当事情变得太复杂时，我会回到这段代码前稍作休息。不过，你还需要知道计算图应该是什么样的（例如，MLP -> Transformer），损失函数应该是什么（例如，自回归/扩散），以及如何最好地利用梯度进行参数更新（例如，SGD -> AdamW）等等。但这段代码是主要过程的核心。

1986年由Rumelhart、Hinton和Williams发表的论文推广并使用了这个算法（反向传播）来训练神经网络：

https://cs.toronto.edu/~hinton/absps/naturebp.pdf

micrograd在Github上的项目：

https://github.com/karpathy/micrograd

还有我（现在有点旧的）非常缓慢地构建和解释的YouTube视频：

https://youtube.com/watch?v=VMj-3S1tku0

![](img/b92d4b57156e8f4356fec1f8ba51a449.png)

### 吴恩达父亲节感叹：编码代理在六个月前还是新奇事物，现在已经很有趣高效

https://x.com/andrewyng/status/1803835964604977663?s=46&t=GRStLXDcUNuun8J5Noyw4Q

吴恩达感叹：在父亲节，我用OpenDevin为女儿生成算术练习题，提升了我们的学习体验。六个月前，编码代理还是新奇事物，现在它们已逐渐实用。编码代理的工作流程包括分析问题、生成代码、测试并迭代改进。多代理系统表现更好，因为不同代理分别编写代码和测试。通过HumanEval等基准测试，研究人员可以自动评估代码生成的准确性。随着工具的发展，编程变得更加有趣和高效。

![](img/7a4cef1b580f8115a762f5e18c409aca.png)

### EVI上线IOS应用

https://x.com/hume_ai/status/1803854136389308495?s=46&t=GRStLXDcUNuun8J5Noyw4Q

EVI，前沿的具有情感智能的语音AI，现在变得更加聪明——并且已经可以在iOS应用上使用了！📲

EVI拥有全新改进的AI语音Kora 💁‍♀️并在其回答中整合了Claude 3.5 Sonnet，现在准备好倾听、回答和探索→ https://apple.co/3zaFVkV

![](img/ed7a3d6e4e34388448028b5a5e30c6db.png)

[ssstwitter.com_1718974133855.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/O4iMbsoVMoCS2NxHWb7cz9lEnne)

### Keyon Vafa分享：Transformer预测纽约市出租车行程的方向

https://x.com/keyonv/status/1803838591371555252?s=46&t=GRStLXDcUNuun8J5Noyw4Q

新论文：如何判断一个transformer是否具有正确的世界模型？

我们训练了一个transformer来预测纽约市出租车行程的方向。这个模型表现很好，它能够在新的点之间找到最短路径。

但是它是否建立了纽约市的地图呢？我们重建了它的地图，发现了以下内容：

![](img/70d9dc4fa106c2c7f09f98c4aadbc833.png)

### 震惊！人们居然殴打机器人：机器人日常训练由强化学习驱动，以确保友好服务于人类

https://x.com/unitreerobotics/status/1803728664213012520?s=46&t=GRStLXDcUNuun8J5Noyw4Q

机器人日常训练由强化学习驱动

由强化学习驱动的机器人日常训练片段。

事先进行了多项测试，以确保友好服务于人类。😊

训练中包括一些极限测试，请勿模仿。

![](img/ecdca0a611ca1cec9513e4bde5d7633a.png)

[ssstwitter.com_1718974595629.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HQ6qbXOEXoJXbCxomHdcTIuhnqg)

### 谷歌Deepmind Jerry Wei加入Anthropic AI

https://x.com/jerryweiai/status/1803852311242117562?s=46&t=GRStLXDcUNuun8J5Noyw4Q

生活更新：在@Google Brain/DeepMind工作了大约2年后，我加入了@AnthropicAI！

非常感谢@quocleix和@yifenglou，在我还未完成斯坦福本科时就给了我这个机会，邀请我加入他们的团队。由于他们对我潜力的信任，我有了与许多顶尖研究人员一起研究LLM推理/事实性的独特机会。

我将带着以下经验继续前行：

1.  构建顶级模型需要可靠的评估。

1.  当结果与预期不符时，首先应该查看数据和模型输出。

1.  在突破性项目中扮演次要角色比在渐进项目中扮演主要角色更好。关注项目的影响，而不是你将获得多少荣誉。

1.  接受批评并主动寻求他人的帮助以改进你的想法，这会让你成为更优秀的研究人员。

1.  可操作的反馈更容易被接受。

1.  优化人才密度往往比优化人才总量更有效。

1.  与领域外的人交流可以让你产生一些原本可能会错过的想法。

1.  在构建复杂系统时记得编写单元测试。你可能会捕捉到导致结果噪声的错误。

1.  配对编程非常有用，尤其是在你刚开始一个新项目或在不熟悉的代码库部分工作时。

1.  主动提供帮助是我建立最难忘友谊的方式！

我迫不及待地想向Anthropic的优秀研究团队学习，在我们越来越接近AGI的过程中，继续推进AI安全研究！🤔🤖🌎

![](img/37337a354023d2baa362d846619f14a8.png)

![](img/e8d7bd5cd288e76c1ab8e2e99195f0f0.png)

# 资讯

### 华为云盘古大模型 5.0 正式发布：升级多模态能力，可提供十亿到万亿级不同参数

https://www.ithome.com/0/776/805.htm

在今日举行的华为 HDC 2024 开发者大会上，华为常务董事、华为云 CEO 张平安宣布盘古大模型 5.0 正式发布，在全系列、多模态、强思维三个方面实现升级。

![](img/8afebe3cf3d013db66c27c5eea953065.png)

### 腾讯AI Lab联合腾讯TME天琴实验室打造「琴乐大模型」，用AI助力音乐创作

https://mp.weixin.qq.com/s/J5UGb4n4h1FNwaOPJfy2gA

「琴乐大模型」模型可根据文本（默认标签或自然语言提示词），直接生成 44.1kHz 的立体声音频（wav）或最多 30 个小节、包含旋律轨、和弦轨、伴奏轨和打击乐轨等的多轨乐谱（MIDI）。对于乐谱输出，该模型支持对已生成的乐谱进行自动编辑操作，如续写、重新生成指定轨、重新生成指定小节、配器、修改乐器类型、修改节奏等。

![](img/7932708f94212b8cdd260416c42cea40.png)

### OpenAI首席技术官：预计几年内“博士级智能”的AI就会出现

https://finance.sina.com.cn/stock/usstock/c/2024-06-21/doc-inazntae1253847.shtml

OpenAI首席技术官米拉·穆拉蒂（Mira Murati）预测了人工智能能力的快速发展，预计在未来“几年”里，用于特定任务的“博士级智能”就会出现。

![](img/7621bc44d8bd8b270652639c934096da.png)

### 又一家华人创办的 AI Agent 公司悄然浮出水面！Accel 3500 万美元领投 A 轮，a16z 领投种子轮

https://mp.weixin.qq.com/s/yKkp9z-JtpSSEj7DwnkEBw

Decagon 由 CEO Jesse Zhang 以及 CTO Ashwin Sreenivas 共同创立，致力于为企业提供先进的生成式 AI 平台，以解决复杂的客户支持问题。Decagon 的解决方案不仅仅是简单的聊天机器人，而是能够与企业现有工作流程深度集成的类人 AI Agent。

![](img/86853f56f5e6df9f9048528ed0aa12b0.png)

### 可灵宣布推出全新功能图生视频和视频续写

https://www.pingwest.com/w/295957

据可灵官方消息，可灵今天宣布两个新功能现已上线，分别是图生视频和视频续写。其中图生视频功能可以根据用户提供的图片生成一段5秒的视频，用户还可以提供提示词来控制视频的运动轨迹。而视频续写功能可以让可灵为生成的视频添加后续，每次可灵可以增加4-5秒的视频时长。该功能支持多次续写，最长可生成长达3分钟的视频。用户还可以根据提示词来对续写内容进行规定。

![](img/e20d6582573831d82702354e8889db66.png)

### 阿里云推出首个 AI 程序员，具备架构师、开发 / 测试工程师等岗位技能

https://www.ithome.com/0/776/746.htm

**在今天的阿里云上海 AI 峰会**上，阿里云推出首个“AI 程序员”，具备架构师、开发工程师、测试工程师等岗位技能，能完成任务分解、代码编写、测试、问题修复、代码提交整个过程，号称最快“分钟级”完成应用开发。

![](img/ddc37b0a8081f20f9aa6288541b2a417.png)

# 论文

### 提炼精华，剔除糟粕：关于大型语言模型微调的数据选择的再思考

https://arxiv.org/abs/2406.14115

在大型语言模型（LLMs）微调的数据选择方面，目标是从给定的候选数据集中挑选出高质量的子集，以此来训练一个待微调模型（PFM），并将其转化为一个经过选择增强的模型（SEM）。这一过程不仅可以提高模型的性能，还可以加快训练过程。尽管已有一些研究调查了数据选择的相关方法，但由于各种实验设置的不同，缺乏对现有方法的全面比较。为了解决这一问题，文章首先提出了一个三阶段的数据选择方案，并且根据这个方案全面回顾了现有的工作。接着，设计了一个统一的比较方法，使用基于比率的效率指标和基于排名的可行性指标，以克服在不同实验设置下比较各种模型的困难。深入的比较分析发现，更有针对性地使用数据特定和模型特定的质量标签的方法具有更高的效率，但在设计选择算法时应避免引入额外的噪声信息。最终，文章总结了数据选择的趋势，并强调了短期和长期的挑战，以指导未来的研究。

相关领域：大型语言模型微调、数据选择、模型性能提升、训练过程加速

机构：

1.  香港中文大学（深圳）数据科学学院

1.  深圳大数据研究所

1.  中国科学技术大学

![](img/f8a0f7e01942653c221ea05c749018d3.png)

### DeciMamba: 探索Mamba的长度外推潜力

https://arxiv.org/abs/2406.14528

长序列处理对于Transformers来说是一个重大挑战，因为它们的输入长度复杂度是二次方的。一个有希望的替代方案是Mamba，它展示了高性能并且在需要大大减少计算资源的同时达到了Transformer级别的能力。在本文中，我们探索了Mamba的长度泛化能力，发现其相对有限。通过一系列的可视化和分析，我们发现限制来自于由训练过程中使用的序列长度决定的受限有效感知场。为了解决这个限制，我们引入了DeciMamba，这是专门为Mamba设计的上下文扩展方法。该机制建立在S6层内置的隐藏过滤机制之上，使训练好的模型即使没有额外训练也能很好地进行外推。实证实验表明，DeciMamba可以在不使用额外计算资源的情况下，外推到比训练时看到的上下文长度长25倍的上下文，并适用于真实世界的长范围NLP任务。

相关领域：深度学习、自然语言处理、Transformer模型、序列处理、上下文扩展方法、Mamba模型、DeciMamba

机构：特拉维夫大学、Google Research

代码和模型发布链接：[https://github.c](https://github.com/assafbk/DeciMamba)om/assafbk/DeciMamba

![](img/812db19daa95c4c3d8408bf036e061e0.png)

### 数据污染可以跨越语言障碍

https://arxiv.org/abs/2406.13236

大型语言模型（LLMs）开发过程中的不透明性引起了对预训练数据中公共基准测试数据可能受到污染的担忧。现有的污染检测方法通常基于训练和评估数据之间的文本重叠，这可能过于表面，无法反映更深层次的污染形式。在本文中，我们首先提出了一种跨语言的污染形式，这种污染通过过度拟合基准测试集的翻译版本，故意注入模型，从而在提高LLMs性能的同时避开了现有的检测方法。然后，我们提出了基于泛化的方法来揭示这种深度隐藏的污染。具体来说，我们检查修改原始基准测试后LLMs性能的变化，将错误答案选项替换为其他问题的正确选项。受污染的模型很难泛化到这种更简单的情况，在这种情况下，错误选择甚至可能不是错误，因为在它们的记忆里所有选项都是正确的。实验结果表明，跨语言污染可以轻易欺骗现有的检测方法，但无法欺骗我们的。此外，我们讨论了跨语言污染在解释LLMs工作机制以及在训练后提升LLMs多语言能力的潜在应用。我们使用的代码和数据集可以从https://github.com/ShangDataLab/Deep-Contam获得。

相关领域：大型语言模型、数据污染检测、跨语言模型、基准测试、模型泛化

机构：University of California, San Diego

![](img/b99de065ec1151e2bc0b548b0318c830.png)

### 理解长上下文大型语言模型的RoPE扩展：从注意力的角度

https://arxiv.org/abs/2406.13282

本文旨在提供一种直观深入的理解，关于如何从注意力的角度扩展旋转位置嵌入（RoPE）以处理长文本。研究表明，保持与预训练长度一致的注意力模式可以改善模型对长文本的外推能力；大的注意力不确定性会导致检索错误；使用更长的连续预训练长度可以减少注意力不确定性并显著提升外推能力。

相关领域：NLP、LLMs、注意力机制、位置编码方法以及模型的扩展和微调。

机构：

+   作者Meizhi Zhong、Kehai Chen、Min Zhang 来自哈尔滨工业大学的计算与智能研究院。

+   作者Chen Zhang 来自北京理工大学。

+   作者Yikun Lei、Xikai Liu、Yan Gao、Yao Hu 来自小红书公司。

![](img/0036de769ab7703c7abdce97ebeab24b.png)

### 贝叶斯-LoRA：使用可微分贝叶斯门控实现最优量化水平和秩值的基于LoRA的参数高效微调

https://arxiv.org/abs/2406.13046

在自然语言处理中，通常的做法是在一个通用领域上预训练单一模型，然后对其进行微调以适应下游任务。然而，对于大型语言模型来说，对整个模型进行微调可能在计算上代价高昂，导致能耗非常大。因此，最近提出了几种参数高效微调（PEFT）方法。其中最受欢迎的方法是低秩适应（LoRA），其关键思路是将预训练模型权重的更新分解为两个低秩矩阵。但是，现有方法要么在所有不同的权重矩阵上使用相同的秩值，要么不使用任何量化技术，而量化技术在模型的能耗方面是非常重要的因素之一。在这项工作中，我们提出了一种新的方法，称为贝叶斯LoRA（B-LoRA），它通过在量化水平和学习到的低秩矩阵的秩值上使用先验分布，从贝叶斯的角度来处理矩阵分解和量化。结果，B-LoRA能够在特定下游任务上微调预训练模型，为每个低秩矩阵找到最优的秩值和量化水平。我们在GLUE基准测试上验证了所提出的模型，并将其与相关基线进行了比较，展示了定性和定量结果，说明了所提出的方法如何能够学习到最优秩的量化矩阵。B-LoRA在减少约70%的总位操作量的同时，性能与基线相当或更好。

相关领域：NLP、PLMs、PEFT、LoRA、量化技术和贝叶斯方法。

机构：参与本研究的机构包括荷兰代尔夫特理工大学（Delft University of Technology）和英国的谷歌DeepMind。文中作者Cristian Meo和Ksenia Sycheva来自代尔夫特理工大学，而Anirudh Goyal和Justin Dauwels分别来自谷歌DeepMind和代尔夫特理工大学。

![](img/8e056acf1d72325730226d2099ef43e6.png)

### 长上下文语言模型是否包含了信息检索、RAG、SQL 等功能？

https://arxiv.org/abs/2406.13121

本文探讨了长上下文语言模型（Long-context Language Models, LCLMs）在处理传统上依赖于检索系统或数据库等外部工具的任务方面的潜力。利用LCLMs原生地摄取和处理整个语料库的能力，可以带来许多优势，包括提高用户友好性，减少对工具专业知识的需求，提供健壮的端到端建模以最小化复杂流程中的级联错误，并允许在整个系统中应用高级提示技术。为了评估这种范式转变，我们引入了LOFT（Long-Context Frontiers），这是一个针对需要高达数百万token上下文的真实世界任务的基准测试，旨在评估LCLMs在上下文检索和推理方面的性能。我们的发现揭示了LCLMs在从未明确训练过这些任务的情况下，仍具有与最先进检索和RAG系统相媲美的惊人能力。然而，LCLMs在SQL类任务所需的组合推理等领域仍面临挑战。值得注意的是，提示策略显著影响性能，强调了随着上下文长度增长，持续研究的必要性。总体而言，LOFT为LCLMs提供了一个严格的测试场，展示了它们取代现有范式和应对新任务的潜力。

相关领域：自然语言处理、机器学习、人工智能、语言模型、信息检索、数据库查询、多模态学习、评估基准

机构：Google DeepMind

![](img/a621f3b1ac99ab7490eeb6936a69047a.png)

# 产品

### Dot ——期待了半年的产品上线啦！

贡献人：

https://apps.apple.com/us/app/dot-living-history/id6450016041

Dot 是一款「以天为周期」的对话式产品，它具有无限长期记忆，能够随着与用户的互动而不断学习和进化，深入了解用户的偏好和习惯。

Dot 被设计成富有同理心和情商的智能伙伴，可以提供个性化的建议和指导，帮助用户在生活、工作和人际关系等方面不断成长和进步。

![](img/851d0c6b2ebd47fd5c9087c63a5df65b.png)

### Document AI

贡献人：

https://www.playmaker.so/document-ai

Document AI 是一款基于人工智能技术的文档处理解决方案，它能够自动从各种格式的文档中准确提取所需数据和信息，理解文档结构和上下文，并与企业内部系统和流程无缝集成，大幅提高文档处理的效率和准确性。

![](img/804d0243d22b3f4e68eca5ed67cdc94f.png)

### Socap.ai

贡献人：

https://socap.ai/

socap.ai 是一个由的 YC 校友创立的平台，它利用人工智能和社区力量，为初创企业提供更快、更有针对性地寻找合适投资人并建立联系的方式，从而大大提高创业者的融资效率。这个平台的目标是帮助创业者以更快捷、个性化的方式完成筹款过程。

![](img/5685019f56064b912159cd99145a45c7.png)

### TrustLoop

贡献人：

https://www.trustloop.co/

TrustLoop 是一款基于人工智能的产品反馈和声誉管理解决方案，通过提供直观的双重调查工具，帮助产品团队同时收集用户正面和负面反馈，并利用 AI 分析功能挖掘可操作的改进机会。TrustLoop 还支持自定义评论登陆页面，并根据公开评论计算产品信任评级，为产品增长提供全面支持。

![](img/52739ccd202ae6f85e808d25d3672b35.png)

# HuggingFace&Github

### Quest

贡献人：

https://github.com/mit-han-lab/Quest?tab=readme-ov-file

这个项目提出了一种名为 Quest 的查询感知型 KV 缓存选择算法，该算法可以大大提高长上下文大型语言模型（LLM）的推理速度，同时仅有微小的准确性损失。具体来说，Quest 会跟踪 KV 缓存页面中的关键字值范围，并根据查询向量估计每个页面的重要性，只加载最关键的少量页面进行自注意力计算，从而实现高达 2.23 倍的自注意力加速和 7.03 倍的推理延迟减少，这为迅速增长的长上下文 LLM 应用提供了一种有效的优化方法

![](img/b07d23f2f56e54e0c8d1f46dc627a5de.png)

### Goldfish Loss

贡献人：

https://github.com/ahans30/goldfish-loss

Goldfish Loss 是一种新的技术，减轻大型语言模型中可提取记忆的问题。它被描述为一种"非常简单"的方法，通过随机排除部分训练 Tokens 来减少语言模型对训练数据的记忆，从而降低隐私和版权风险，同时在保持模型性能的情况下显著降低了可提取的记忆量。

![](img/21679091ebebed83b0023f3de6847d23.png)

# 投融资

### 「银河通用机器人」完成7亿元天使轮融资

贡献人：

https://mp.weixin.qq.com/s/dbzafzT40OwgRwFMrypyLw

银河通用机器人近期完成了 7 亿元人民币的天使轮融资，光源资本担任独家财务顾问并参与了早期投资。此次融资吸引了包括美团点评战投、北汽产投、商汤国香基金、讯飞基金在内的顶级战略及产业投资方，以及启明创投、蓝驰创投、经纬创投、源码资本、IDG 资本等财务机构。银河通用成立于 2023 年 5 月，专注于通用可泛化的具身大模型机器人的研发和商业化应用，目前已在视觉语言图文大模型的调优和技能执行层核心技术上取得了多项突破。公司发布的银河通用 G1 机器人具备高级泛化识别抓取及 3D 视觉导航能力，预计将在今年内进入商业场景。银河通用机器人的融资成功，不仅体现了投资者对于具身智能领域的热情，也显示了对银河通用技术实力和市场潜力的认可。

![](img/76eda668dfcf83443db000662227cddd.png)

### AI芯片独角兽Cerebras申请IPO

贡献人：

https://mp.weixin.qq.com/s/0nJ3RDVJR_7nV_VO3RHprg

AI芯片独角兽Cerebras Systems已秘密申请IPO，估值约为42亿到50亿美元。Cerebras成立于2016年，总部位于美国加州，专注于研发用于训练AI模型的晶圆级芯片。该公司已累计融资7.2亿美元，并与阿布扎比科技集团等合作构建超级计算机。本次IPO牵头银行为花旗集团。Cerebras的最新旗舰芯片及其超算系统在AI训练性能和能效方面表现突出。

公司官网：https://www.cerebras.net/

![](img/3294b6540b516245980566051979b376.png)

### 前HubSpot高管筹集400万美元打造AI驱动的CRM

贡献人：

https://day.ai/sequoia-backs-ai-native-crm

前HubSpot首席产品官Christopher O'Donnell创立了Day.ai，这是一款利用生成式AI的客户关系管理（CRM）系统。Day.ai自动从用户与公司之间的对话、电子邮件和公共记录中获取信息，消除了手动数据输入的需求。该公司已完成由红杉资本领投的400万美元种子轮融资。目前，Day.ai的CRM系统通过邀请制进行测试，计划颠覆现有的CRM市场。O'Donnell表示，短期内公司不打算进一步融资。

公司官网：https://day.ai/

![](img/520d216d9da82d853bb3ed6b4cd06612.png)

### Materia利用AI提升会计效率，完成630万美元融资

贡献人：

https://techcrunch.com/2024/06/20/materia-looks-to-make-accountants-more-efficient-with-ai/

AI驱动的会计软件公司Materia通过630万美元种子轮融资走出隐形模式，融资由Spark Capital领投，参与者包括Haystack Ventures和Thomson Reuters Ventures等。Materia集成现有工作流软件，利用生成式AI平台自动处理和整理数据，减轻会计师的重复工作负担，使其能够专注于高风险领域。目前，公司已与美国排名前100的会计师事务所中的五家合作，目标是提高整个会计行业的效率和质量。

公司官网：https://www.trymateria.ai/

![](img/797d2836ca23db24f728d24753eb2b73.png)

### Julie Bornstein筹集5000万美元打造新型电商搜索引擎

贡献人：

https://techcrunch.com/2024/06/20/former-stitch-fix-coo-julie-bornstein-secures-50m-to-build-a-new-age-e-commerce-search-engine/

前Stitch Fix首席运营官Julie Bornstein创立的Daydream公司筹集了5000万美元种子资金，旨在打造一个由AI驱动的新型电商搜索引擎。该平台利用生成式AI、机器学习和计算机视觉技术，为用户提供个性化购物结果。本轮融资由Forerunner Ventures和Index Ventures领投，Google Ventures和True Ventures参与。Daydream计划今年秋季在美国推出测试版，专注于时尚领域，目前已合作超过2000个品牌。

公司官网：https://daydream.ing/welcome

![](img/f081e18f81a4e484c62bce2a98abeb28.png)

### Poolside 融资4亿美元，估值达20亿美元，用于开发超级编程助手

贡献人：

https://techcrunch.com/2024/06/20/poolside-raising-400m-at-a-2b-valuation-for-supercharged-coding-copilot/

巴黎的生成式AI公司Poolside.ai正在筹集至少4亿美元资金，投后估值达20亿美元。领投方为Bain Capital Ventures和DST，其中BCV是其早期投资者。Poolside在去年8月完成了1.26亿美元的种子轮融资。本轮融资旨在加速开发帮助开发者提高工作效率的工具，目标是成为广泛适用的AI公司。CEO为GitHub前CTO Jason Warner，CTO Eiso Kant则是Athenian的创始人。

公司官网：https://www.poolside.ai/

# 学习

### 如何评价网传 ASML「掀老底」称 3nm 芯片实际为 23nm，1nm 芯片是 18nm？

贡献人：

https://www.zhihu.com/question/659222197?utm_psn=1786850772607029248

ASML 公布的 EUV 光刻机路线图揭示了芯片工艺实际尺寸与标称 “nm” 尺寸之间的差异。例如，N3 工艺实际对应 23nm 的金属半间距，而非 3nm。这种命名方式自 1997 年起，已经不再以物理尺寸命名，而是基于晶体管密度和性能提升。芯片技术从平面型晶体管发展到 FinFET，未来可能采用 GAA FET 和 CFET 等新技术。尽管物理尺寸难以进一步显著缩小，摩尔定律仍然通过提高晶体管密度得到体现。行业内部对于这种命名方式的理解是一致的，认为它是行业标准，而非误导消费者。此外，类似于芯片工艺的 “等效” 标注现象在其他行业如手机相机焦距标注中也普遍存在。

![](img/b55647d404e94b95b20ec2e0b9d92975.png)

### 生成式AI设计模式：综合指南

贡献人：

https://towardsdatascience.com/generative-ai-design-patterns-a-comprehensive-guide-41425a40d7d0

生成式 AI 设计模式指的是一系列用于创建能够生成新数据的算法框架。这些模式包括生成对抗网络（GANs）、变分自编码器（VAEs）、自回归模型和流模型。GANs 通过生成器和鉴别器之间的对抗学习生成数据；VAEs 则通过优化变分下界来生成模糊不清的数据；自回归模型按照条件概率顺序生成数据序列；流模型则通过学习数据的连续变换来生成。这些技术在图像、音乐和文本生成等领域有广泛应用，但也面临着稳定性、效率和伦理等挑战。未来研究将致力于提高模型性能，并确保技术的负责任使用。

![](img/8f8151221e05a1882f497a22912c9c63.png)

### 使用Rotation抑制LLM中的Outlier

贡献人：

https://zhuanlan.zhihu.com/p/703687847?utm_psn=1786922512019460096

Outlier 问题会导致模型的量化精度受损，影响模型的性能。Rotation 方法通过引入随机 Hadamard 变换，对权重矩阵和激活值进行处理，减少不相干性，从而有效抑制 Outlier。具体实现包括对 FFN 和 Attention 部分进行调整，分别在离线和在线阶段应用 Hadamard 变换。在 FFN 部分，通过将 RMSNorm 和 LayerNorm 的线性部分融合到邻近的权重矩阵中，并在 W_{down} 前插入 Hadamard 变换操作，来减少激活值的相干性。在 Attention 部分，通过对多头的 W_v 和 W_{out} 以及 Key 和 Query 应用 Hadamard 变换，来抑制 Outlier。实验结果显示，Rotation 方法在低比特量化上取得了优异的效果，并且新增操作的开销不大。这一方法为 LLM 的模型部署和推理加速提供了有效的解决方案。

![](img/df7d3762324603ee9bc2f370c895e523.png)

### MoE LLM对AI芯片通信的影响

贡献人：

https://zhuanlan.zhihu.com/p/697278043?utm_psn=1787077197925736448

MoE LLM 由多个专家网络组成，通过一个门控网络来决定不同输入数据应该由哪些专家网络处理。这种架构在处理复杂任务时，可以显著提高模型的性能和泛化能力。然而，MoE LLM 对于 AI 芯片的通信需求提出了更高的要求，因为它需要在多个专家网络之间高效地传输和处理数据。文章指出，为了满足 MoE LLM 的需求，AI 芯片需要具备更强的计算能力和数据传输效率，以及更高级的内存管理技术。此外，还需要设计新的通信协议和网络拓扑结构来优化专家网络之间的数据传输。

![](img/d9ae889842a0ef4d65b00de0150479e5.png)

### NVIDIA GeForce “Blackwell” GPU系列的可能规格泄露

贡献人:

https://www.techpowerup.com/323495/possible-specs-of-nvidia-geforce-blackwell-gpu-lineup-leaked

NVIDIA 即将推出的 GeForce "Blackwell" GPU 系列的泄露信息揭示了五种芯片的详细规格。顶级芯片 GB202 预计配备于 RTX 5090，拥有 24,576 个 CUDA 核心，512 位 GDDR7 内存接口，以及 1,792 GB/s 的内存带宽。接下来的 GB203 梦想中可能成为 RTX 5080 的 heart，带有 10,752 个 CUDA 核心梦想中带有梦想中带有梦想中带有的 256 位内存接口。GB205 和 GB206 分别针对中端市场，具有 6,400 梦想中和 6,144 个 CUDA 核心，梦想中带有ゼ内ルフ 7 ユナイトで話す日本人，以及 192 位和 128 位的内存梦想中。入门级的 GB207 拥有 2,560 个 CUDA 核心和 128 位的 GDDR6 内存接口。这些新芯片预计会采用梦想中带有更大缓存和ゼネルフ 7 архитектуры梦想中带有ゼネルフ 7 Dreams 的改进，以提高ゼネルフ 7 绘图性能和人工智能加速能力。预计 NVIDIA 将在 2024 年第四季度向市场推出 "Blackwell" 系列。用户对于新 GPU 的性能提升表示期待，同时梦想中对于内存容量和价格策略也梦想中带有梦想中带有的关注。

![](img/8616571354819c60e808d0d0d6b18698.png)

### 如何通过网页抓取和RAG提升大型语言模型的能力

贡献人:

https://ai.gopubby.com/how-to-power-up-llms-with-web-scraping-and-rag-975a165587f6

网页抓取和检索增强生成（RAG）技术可以提升大型语言模型（LLMs）的性能。RAG 允许 LLMs 通过额外的数据集获得特定领域的知识，从而生成更准确、相关的输出。在实践中，可以使用 Python 以及 Scrapfly 等工具进行网页抓取，获取必要的数据，并结合 LlamaIndex 将这些数据集成到 LLMs 中。这种方法不仅提高了模型在特定领域的准确性，还避免了对模型进行全面重新训练的繁琐过程。通过这种方式，RAG 技术为 LLMs 的应用提供了灵活性和可扩展性，使其能够更好地适应实际需求。

![](img/db1dfb8a101d11d27ef1793a209dcccb.png)

### 人形机器人引爆谐波减速器市场，盘点国内外16家核心谐波减速器产能和下游应用及客户

贡献人：

https://mp.weixin.qq.com/s/NO_qDrCXtIoshmH0XEc6Rg

文章深入探讨了人形机器人技术的发展如何推动了谐波减速器市场的增长。谐波减速器作为高精度驱动系统的关键组件，在人形机器人的运动控制中扮演着至关重要的角色。文章列举了国内外 16 家主要制造商，如日本的 Mitsubishi、德国的 Siemens 等，它们通过技术创新提升了产品的性能和可靠性。这些制造商在产能上的扩张和优化，以及在精密制造、医疗设备、服务机器人等领域的广泛应用，显示了谐波减速器市场的活跃度和增长潜力。特别是在人形机器人领域，谐波减速器的高精度和高效率特性被广泛采纳，推动了该行业的技术进步。随着人形机器人技术的不断成熟，谐波减速器的市场需求预计将持续增长，尤其是在需要高精度驱动解决方案的应用场景中。

![](img/de738ccbb1242278c88f6756bf2d895b.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

##

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～微信：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报****知识库****】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/86380c107bc6fc7e1ce071d5edb8c112.png)