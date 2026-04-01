# 奇绩大模型日报（5月 22日）

> 来源：[https://miracleplus.feishu.cn/docx/PiWDdHD6mo6ZAax8g16cDpGAnNf](https://miracleplus.feishu.cn/docx/PiWDdHD6mo6ZAax8g16cDpGAnNf)

# 推特

### Codium AI Cover Agent: 第一个改变自动测试生成的论文的开源实现

https://x.com/svpino/status/1792897013920538944

第一个改变自动测试生成的论文的开源实现现在可用！

今年二月，Meta发布了一篇论文，介绍了一种工具可以自动增加测试覆盖率，并保证对现有代码库进行改进。

这是一件大事，但Meta并没有发布代码。

幸运的是，我们现在有了Cover-Agent，一个你可以安装的开源工具，它实现了Meta的论文内容，可以自动生成单元测试：

https://github.com/Codium-ai/cover-agent

我录制了一个简短的视频，展示了Cover-Agent的实际应用。我想提两点：

1.  自动生成单元测试并不是新鲜事，但要做好却很难。如果你让ChatGPT来做，你会得到重复的、不可运行的和无意义的测试，这些测试不会改进你的代码。而Meta的解决方案只生成唯一的、可运行的测试，并且增加代码覆盖率。

1.  对于在写代码之前编写测试（TDD）的人来说，这可能不太有用。但这没关系，并不是每个人都做TDD，但我们都需要提高测试覆盖率。

AI有很多好与坏的应用，但这是一个我期待能融入我生活的应用。

![](img/965ee534294c914fbcbfaaf29e157fc2.png)

[fkg-Zt0QUvBfIkJn.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CbwLbzzhpoY1iWxYK1VcNMzynya)

### Anthropic最新研究论文：扩展单义性, 首次对现代生产级大型语言模型进行详细研究

https://x.com/AnthropicAI/status/1792935506587656625

Anthropic最新研究论文：扩展单义性。

首次对领先的大型语言模型进行了详细的内部研究。

在此阅读博客文章：http://anthropic.com/research/mapping-mind-language-model

今天，我们报告了在理解AI模型内部运作方面的重要进展。我们已经识别出数百万个概念在Claude Sonnet（我们部署的大型语言模型之一）内部的表示方式。这是首次对现代生产级大型语言模型进行详细研究。这一可解释性的发现将来可能有助于使AI模型更加安全。

我们通常将AI模型视为一个黑箱：输入某些内容，然后输出一个响应，但不清楚为什么模型给出了那个特定的响应而不是另一个。这使得我们很难相信这些模型是安全的：如果我们不知道它们如何工作，我们怎么知道它们不会给出有害的、偏见的、不真实的或其他危险的响应？我们如何能相信它们会安全可靠？

![](img/ad356efc20985053a910a11af56918e0.png)

### Shawn Wang: AnthropicAI非常接近在可商品化的解释模型方面取得突破， 不要忽视他们的论文

https://x.com/swyx/status/1793064538650325472

在我看来，AnthropicAI非常接近在可商品化的可解释性方面取得突破。

在过去的四年里，我们用来真正控制大型语言模型（LLM）的工具主要是温度/Top_p和logit偏差。我们最近得到了seed和约束结构化输出，并且interactive=false也在实现中。

但现在Claude Sonnet可以让人们调节多达3400万个特征，这些特征涵盖了从著名人物和名词类别到抽象概念（例如保密能力、代码错误和自我表现）等各个方面。这是“大规模的表示工程”，并且可能很容易为你关心的任何特征集重新训练特征自动编码器（SAEs）。

不要忽视他们的论文。这是一系列突破性工作的成果。

![](img/4f75d80bdbfd2a2d922fa38eff123d08.png)

### Deeplearning AI《设备端 AI 简介》：将 AI 模型部署到边缘设备

https://x.com/AndrewYNg/status/1792919935691214899

学习如何在我们的新短期课程《设备端 AI 简介》中将 AI 模型部署到边缘设备上，该课程由我们与 Qualcomm 合作创建，并由工程高级总监 Krishna Suresh 教授。

我认为设备端（边缘）AI 是一项重要的技术趋势，它支持新的低延迟、隐私保护应用。在本课程中，您将部署一个实时图像分割模型，并通过此过程学习设备端部署的关键步骤：神经网络图捕获、设备端编译、硬件加速以及设备端数值正确性的验证。您还将了解量化如何使您的模型速度提高至 4 倍、尺寸缩小至 4 倍，从而提升其在资源受限的边缘设备上的性能。

所涵盖的技术用于将模型部署到包括智能手机、无人机和机器人在内的多种设备类型上，从而实现许多新的、富有创意的应用。

请在此注册：https://deeplearning.ai/short-courses/introduction-to-on-device-ai/

![](img/909cb774a3cabf5493f947c518ea2d7a.png)

[4xp-LWEqFIvgX6dZ.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Z8Kab3yUBofvoqxkuA1cSus9njb)

### Phi-3 小型和中型模型现在已在 MIT 许可下发布

https://x.com/_philschmid/status/1792934321407369532

Phi-3 小型和中型模型现在已在 MIT 许可下发布！🚀 微软刚刚推出了 Phi-3 小型 (7B) 和中型 (14B) 模型 🤯。Phi-3 小型模型声称优于 Meta 的 Llama 3 和 MistralAI，而 Phi-3 中型模型则优于 GPT-3.5 和 Cohere Command R+ 🤔。

总结：

🧮 Phi-3 小型 7B 和 Phi-3 中型 14B 指导版本，支持最高 128k 上下文

🏆 Phi-3 小型 (7B): 在 MMLU 上得分 75.5；在 AGI 评估中得分 43.9（优于 Mistral 7B 或 Llama 3 8B）

🥇 Phi-3 中型 (14B): 在 MMLU 上得分 78.0；在 AGI 评估中得分 50.2（优于 Cohere Command R+ 或 GPT-3.5-Turbo）

🧠 训练数据包含 4.8 万亿个标记，包括合成和过滤后的公共数据集，并支持多语言（训练数据的 10%）

⚖️ 通过 SFT 和 DPO 微调

🔡 新的分词器，词汇量为 100,352

🔓 所有模型都在 MIT 许可下发布

🤗 在 Huggingface、Azure AI 和 ONNX 上可用

❌ 没有发布基础模型

❌ 没有关于数据集混合的详细信息（合成数据和网络数据的比例）

免责声明：模型训练可能涉及与基准数据类似的合成数据。我们需要进一步的评估或训练数据的详细信息。

![](img/36b7b4a790502813c3b4b859af3944d6.png)

###  Cover-Agent：开源工具，用于自动增强测试组

https://x.com/itamar_mar/status/1792671154538152126

🚀 介绍 Cover-Agent 🧪

这是一款开源工具，包括对 Meta 的 TestGen-LLM 的重新实现，用于自动增强测试套件。

经理：“我们必须改进旧的测试组以提高代码覆盖率。你能处理吗？”

我：“当然，这是我最喜欢的任务……（并不是！）🤷♂️”

Meta 的团队想用 LLMs 来增强测试组——听起来很棒，对吧？但他们没有发布他们的代码。

我们发布了。

在我们改进这个基准的过程中，期待您的反馈。

在这段视频中，我将回顾 TestGen-LLM，分享见解，并向您介绍 Cover-Agent。

![](img/a3952454d1a293711c5878b4137918b5.png)

[GjvysL5Ouz2gGkJq.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BQH6bRsf5oiVe2xKAP7cN5A9nDf)

# 资讯

### 微软颠覆生产力：Copilot推自定义版，AI PC原生支持PyTorch，奥特曼预告新模型

https://mp.weixin.qq.com/s/Rm2MYC8dv6_Hnt68ktUV-w

AI 生产力的未来会是什么样子？全世界都在等待微软的答案。5 月 22 日凌晨，微软 Build 2024 开发者大会在美国西雅图召开，今天的发布有关 AI 技术，更有关 AI 带来的新工具。「三十多年来，微软对于计算机一直有两个梦想 —— 首先是让计算机理解我们，而不是我们去理解计算机；其次，在信息不断增加的世界中，让计算机帮助我们根据信息有效地进行推理、计划和行动。人工智能浪潮已经为我们的梦想找到了答案，」微软 CEO 萨蒂亚・纳德拉说道。今天 Build 大会的内容主要最新的 Copilot，适用于生成式 AI 的新形态硬件，以及使用新 AI 能力的工具堆栈。

![](img/90f617a9e469fa9994b423a76ca5b367.png)

### Hinton万字访谈：用更大模型「预测下一个词」值得全力以赴

https://mp.weixin.qq.com/s/OydltjpVwsQ7hNBH6hq_Og

「这份访谈的每一秒都是精华。」最近，图灵奖得主 Geoffrey Hinton 的一个访谈视频得到了网友的高度评价。在访谈中，Hinton 谈到了很多话题，包括当前大模型的技术路线、多模态学习的重要性、 数字计算与共享知识、智能系统的意识与情感以及他的合作者和优秀学生……Hinton 认为，大型语言模型通过寻找不同领域的共同结构来进行编码，这种能力使它们能够压缩信息并形成深层次的理解，发现现实世界中人类尚未发现的万事万物的联系，这是创造力的来源。他还提到，通过预测下一个符号，模型实际上必须执行一定程度的推理，而不是像很多人所说的大模型并不具备推理能力。随着模型规模的增加，这种推理能力也将变得越来越强。这是一个值得全力以赴的方向。

![](img/72d8383bfef8f6848e650ad5863473d4.png)

### 报道称可穿戴智能设备 Al Pin 的制造商 Humane 正在寻找买家出售

https://www.theverge.com/2024/5/21/24162185/humane-seeking-acquisition-rumor-ai-pin

Humane 是可穿戴智能硬件 AlPin 背后的初创公司，该公司由苹果前长期员工 lmran Chaudhri 和 BethanyBongiorno 领导。据彭博社报道它已经在为其业务寻找潜在买家，寻求 7.5 亿至 10 亿美元的估值，而这距离发布Al Pin 才一个多月。

![](img/fbc791b4e5e59b081bb2bff3f082dfda.png)

### 三维设计巨头 Autodesk 收购 AI 视效创企 Wonder Dynamics

https://www.ithome.com/0/769/807.htm

Autodesk 宣布已收购 **Wonder Dynamics**，两家公司曾一直有密切合作。据悉 Wonde**r Dynamics 作为一家初创公司，旗下的 Wonder **Studio 是一款融合云技术与人工智能（AI）的 3D 动画和视觉特效创作工具，并与 Autodesk Maya 等其他主流的 3D 设计工具高度兼容，帮助艺术家更轻松地在场景中制作 3D 角色动画、灯光和构图。

![](img/ca73712bbc8bbc727bb258e4c12787d7.png)

### 从Claude 3中提取数百万特征，首次详细理解大模型的「思维」

https://mp.weixin.qq.com/s/cZhmvAva6NDLG84kD819Ww

刚刚，Anthropic宣布在理解人工智能模型内部运作机制方面取得重大进展。Anthropic 已经确定了如何在 Claude Sonnet 中表征数百万个概念。这是对现代生产级大型语言模型的首次详细理解。这种可解释性将帮助我们提高人工智能模型的安全性，具有里程碑意义。

![](img/b81cff6851d8a7f01422ca93405a5201.png)

### 简单通用：视觉基础网络最高3倍无损训练加速，清华EfficientTrain++入选TPAMI 2024

https://mp.weixin.qq.com/s/GsFXuNpZAF98bc7uMvOBCA

近年来，「**scaling」**是计算机视觉研究的主角之一。随着模型尺寸和训练数据规模的增大、学习算法的进步以及正则化和数据增强等技术的广泛应用，通过大规模训练得到的视觉基础网络（如 ImageNet1K/22K 上训得的 Vision Transformer、MAE、DINOv2 等）已在视觉识别、目标检测、语义分割等诸多重要视觉任务上取得了令人惊艳的性能。然而，「scaling」往往会带来令人望而却步的高昂模型训练开销，显著阻碍了视觉基础模型的进一步发展和工业应用。面向解决这一问题，清华大学的研究团队提出了一种广义课程学习（generalized curriculum learning）算法：EfficientTrain++。其核心思想在于，将「筛选和使用由易到难的数据、逐步训练模型」的传统课程学习范式推广至「不进行数据维度的筛选，一直使用全部训练数据，但在训练过程中逐步揭示每个数据样本的由易到难的特征或模式（pattern）」。

![](img/494f554f041016afafebc9fd8a2ba2e2.png)

# 论文

### PyramidInfer: 金字塔KV缓存压缩，用于高吞吐量LLM推断

贡献人：

链接：http://arxiv.org/abs/2405.12532v1

大语言模型（LLMs）展示了出色的理解能力，但在推断过程中面临GPU内存使用挑战，限制了它们在像聊天机器人这样的实时应用中的可扩展性。为了加速推断过程，我们在GPU内存中存储计算的键和值（KV缓存）。现有方法研究了KV缓存压缩以通过修剪预先计算的KV缓存来减少内存。然而，它们忽视了层间依赖性以及预先计算中的巨大内存消耗。为了探索这些缺陷，我们发现关键和值的数量逐层递减，并可以通过注意力权重的一致性来提取它们。基于这些发现，我们提出了PyramidInfer，一种通过逐层保留关键上下文来压缩KV缓存的方法。PyramidInfer通过计算更少的键和值而节省显著内存，而不会影响性能。实验结果显示，PyramidInfer比Accelerate提高了2.2倍的吞吐量，并在KV缓存中减少了超过54%的GPU内存。

![](img/b20f4adc3f2d8f389f74093222215c3d.png)

### 稀疏自动编码器在语言模型中实现可扩展和可靠的电路识别

贡献人：

链接：http://arxiv.org/abs/2405.12522v1

本文介绍了一种在大型语言模型中发现可解释电路的高效而稳健方法，使用离散稀疏自动编码器。我们的方法解决了现有技术的关键局限，即计算复杂性和对超参数的敏感性。我们提议在精心设计的正面和负面例子上训练稀疏自动编码器，其中模型只能为正面例子正确预测下一个token。我们假设学习到的注意力头输出的表示将会在头部参与特定计算时发出信号。通过将学习到的表示离散化为整数编码，并测量对于每个头部独特于正面例子的编码之间的重叠，我们能够直接识别参与电路的注意力头部，而无需昂贵的消融或架构修改。在三个广为人知的任务 - 间接对象识别，大于比较和docstring完成 - 所提出的方法在恢复基准电路的准确性和召回率方面比现有技术基准更高，同时将运行时间从几个小时缩短到几秒。值得注意的是，我们仅需要每个任务5-10个文本例子来学习稳健的表示。我们的研究结果突显了离散稀疏自动编码器在可扩展和高效的机械可解释性方面的潜力，为分析大型语言模型的内在工作提供了新方向。

![](img/e6146a9aa73604cd7b19c26e74040bcb.png)

### SirLLM：流式无限保留的LLM

贡献人：

链接：http://arxiv.org/abs/2405.12528v1

随着大语言模型（LLMs）在各个领域变得日益普及，它们处理任意长度的输入和保持一定记忆的能力变得至关重要。然而，一次性输入过长的文本受限，研究表明，当输入长度超过LLMs的预训练文本长度时，文本生成能力会急剧下降。最近的研究采用了流式输入来减轻过长文本输入的压力，但这种方法可能会显著损害模型的长期记忆能力。为了应对这一挑战，我们介绍了Streaming Infinite Retentive LLM（SirLLM），允许LLMs在无限长度的对话中保持更长的记忆而无需进行微调。SirLLM利用Token Entropy指标和记忆衰减机制来筛选关键短语，赋予LLMs持久灵活的记忆。我们设计了三个不同任务并构建了三个数据集，以从不同角度衡量SirLLM的有效性：（1）DailyDialog；（2）购物；（3）石头剪刀布。我们的实验证明，SirLLM在不同LLMs和任务中能够稳定显著地提升效果，充分证明了其有效性。在对话中，“一个先生会忘记自己”，但SirLLM永远不会！我们的代码公开在https://github.com/Zoeyyao27/SirLLM。

![](img/0e4e4587769589d7f959b90890500db5.png)

### Transformer实际上是线性的

贡献人：

链接：http://arxiv.org/abs/2405.12250v1

本文揭示了Transformer解码器独有的一种线性特征，包括GPT、LLaMA、OPT、BLOOM等模型。我们分析了顺序层之间的嵌入变换，发现了一个近乎完美的线性关系（Procrustes相似度得分达到0.99）。然而，当去除残差部分时，由于Transformer层的输出范数始终较低，线性度会降低。我们的实验表明，去除或线性逼近一些最线性的Transformer模块并不会显著影响损失或模型性能。此外，在我们对较小模型的预训练实验中，我们引入了基于余弦相似性的正则化，旨在减少层的线性度。这种正则化提升了像Tiny Stories和SuperGLUE这样的基准测试的性能指标，并成功降低了模型的线性度。这项研究挑战了对Transformer架构的现有理解，表明它们的运作可能比先前假设的更线性化。

![](img/5e3634f9e93307e5081accd811cefb1c.png)

### 使用跨层注意力减小Transformer键-值缓存大小

贡献人：

链接：http://arxiv.org/abs/2405.12981v1

摘要：键-值（KV）缓存在加速解码transformer-based autoregressive大语言模型（LLMs）中扮演着至关重要的角色。然而，在长序列长度和大批量大小下存储KV缓存所需的内存量可能变得难以承受。自Transformer的发明以来，为减小KV缓存大小发现的两个最有效的干预措施是Multi-Query Attention（MQA）及其泛化版本Grouped-Query Attention（GQA）。MQA和GQA都修改了注意力块的设计，使多个查询头可以共享单个键/值头，大幅减少不同键/值头的数量，而仅且稍微降低准确性。本文展示了通过在相邻层之间还共享键和值头，可以进一步推进Multi-Query Attention，得到一种我们称之为Cross-Layer Attention（CLA）的新注意力设计。通过CLA，我们发现可以将KV缓存大小再减少2倍，同时保持与未修改的MQA几乎相同的准确性。在从头开始训练1B和3B参数模型的实验中，我们证明CLA提供了传统MQA可能的内存/准确性权衡的帕累托改进，使推断可以使用比传统上可能的更长的序列长度和更大的批量大小。

![](img/d41987eadeb69c17c1c4a653a2233d73.png)

# 产品

### Atlas.org

贡献人：

https://atlas.org/

Atlas.org 是一款专门为学生设计的智能 AI 助手，旨在通过多方位的学习支持来帮助学生提高成绩，减轻学习压力。它能够针对各种学科领域提供作业辅导、学习指导、写作帮助、内容总结等功能，并基于学生的实际课程材料提供个性化的AI辅助。

![](img/7948cba3f59d752cea84a8efcf4c41e8.png)

### Ragobble

贡献人：

https://ragobble.com/

Ragobble 是一款利用先进的人工智能算法来增强搜索功能的创新工具，它可以帮助用户更快速、准确地搜索到所需的视频、文件和网页链接等内容。该工具通过深度挖掘数据，提供更广泛的搜索范围，让用户能够发现传统搜索手段难以找到的信息。同时，Ragobble 也致力于优化搜索流程，给予用户更加友好、高效的在线导航体验。

![](img/f34245761198c1437f3e6fab439d7010.png)

# HuggingFace&Github

### Shell-ask

贡献人：

https://github.com/egoist/shell-ask

Shell Ask 由 ChatKit 赞助，ChatKit 是一款适用于 ChatGPT 和许多其他型号的免费聊天应用程序。

![](img/d9cad7b0c93f05e2f7d38c78caa63797.png)

### 100 days_AI——初学者进阶计划

贡献人：

https://github.com/h9-tect/100days_AI

这是一个循序渐进的 100 天人工智能学习计划，从基础知识到高级概念一一涵盖，适合AI初学者跟随学习。该路线图首先介绍了人工智能的历史和应用，并指出了学习所需的先决条件，包括基本编程知识、数学基础和学习实验的意愿。接下来的每一天都有具体的任务、资源和练习，涵盖了 Python 编程、NumPy、Pandas和Matplotlib等重要工具，使学习者能够循序渐进地掌握人工智能的核心知识。整个计划旨在帮助初学者在短时间内快速提升人工智能方面的能力，为后续的深入学习奠定坚实的基础。

### Fisher AI

贡献人：

https://github.com/fisherdaddy/FisherAI

FisherAI是一款专为提高学习效率而设计的Chrome浏览器插件。它集成了自动摘要、网页及视频翻译、多轮对话等多种实用的AI功能，并支持多种大语言模型，为用户提供了灵活强大的AI工具。通过一键操作，FisherAI可以帮助用户更高效地处理信息，提升学习体验。

![](img/685a0b7cdb884473faacf5e58dab164c.png)

# 投融资

### 数据标注初创公司Scale AI融资10亿美元

贡献人：

https://scale.com/blog/scale-ai-series-f

Scale AI为希望训练机器学习模型的公司提供数据标注服务，近日宣布完成了10亿美元的F轮融资，投资方包括亚马逊和Meta等知名机构和公司。此次融资是主次结合，是AI领域大量巨型风险投资的最新案例之一。在此之前，Scale AI已经历了约8年的发展，此前共筹集了约6亿美元，包括2021年的3.25亿美元E轮融资，估值约为70亿美元。三年后，尽管去年裁员20%，但Scale AI的估值已经达到138亿美元。Accel领投了本轮融资，而Scale AI的现有投资者包括Nvidia、Y Combinator等也纷纷回归。该公司的客户包括微软、丰田、通用汽车、Meta和美国国防部等。Scale AI表示将利用新资金加速“铺设通向人工智能通用智能的道路的前沿数据的丰富性”。

公司官网：https://scale.com/

![](img/4fdbc47022b3937f95a37555b7b6fd4c.png)

### 法国AI初创公司H完成2.2亿美元种子轮融资

贡献人：

https://techcrunch.com/2024/05/21/french-ai-startup-h-raises-220-million-seed-round/

法国AI初创公司H（原名Holistic AI）在成立几个月后宣布完成了2.2亿美元的种子轮融资。该公司的创始团队包括前DeepMind的顶级研究人员，吸引了众多投资者。投资者名单包括亿万富翁Eric Schmidt、Xavier Niel和Bernard Arnault的Aglaé Ventures等，以及知名风投Accel、Bpifrance、Creandum等。H的目标是开发能够提高工作效率的前沿行动模型。融资中的40%为传统股权投资，其余部分将转为未来股权。

公司官网：https://www.hcompany.ai/

![](img/a6d94c50c68994b1f54512cab71cf473.png)

### AI音乐公司Suno完成新一轮融资

贡献人：

https://www.billboard.com/business/tech/ai-music-company-suno-raises-new-funding-round-1235688773/

AI音乐公司Suno最新获得了1.25亿美元的投资。该公司的模型可以生成声音、歌词和音乐。投资者名单包括Lightspeed Venture Partners以及Nat Friedman和Daniel Gross。Suno是少数几家可以同时生成声音、歌词和乐器的初创公司之一，旨在推动“任何人都可以创作音乐”的未来。该公司的投资者还包括Matrix和Founder Collective等。Suno的用户可以通过简单的文本提示创建完整的歌曲，而免费用户每月最多可以生成10首歌曲，而Pro计划和Premier计划的用户每月分别可以生成最多500首和2000首歌曲。

公司官网：https://suno.com/

![](img/de26b6ffc92adb0b0f7f981aa83309ad.png)

### Coactive.ai宣布完成B轮融资

贡献人：

https://coactive.ai/blog/series-b

Coactive.ai宣布获得3000万美元的B轮融资，加速向无元数据未来迈进。Cherryrock Capital与Emerson Collective共同领投，Greycroft以及之前的投资者Andreessen Horowitz、Bessemer Venture Partners和Exceptional Capital也参与了本轮融资。该公司致力于解锁图像和视频的潜力，为智能搜索和视频分析等应用提供服务，无需元数据或标签。 Coactive的平台使企业能够直接加载和索引原始图像和视频，使它们可搜索，不需要元数据或标签。

公司官网：https://coactive.ai/

![](img/60e5fc570f64fdb5029a0e3d90e727ae.png)

### Expedera完成2千万美元B轮融资

贡献人：

https://www.prnewswire.com/news-releases/expedera-raises-20m-series-b-funding-round-led-by-indie-semiconductor-302150985.html

Expedera宣布由indie Semiconductor领投的B轮融资，融资金额为2000万美元。这笔投资将用于推进公司的Origin™设备AI处理器IP产品和服务，并扩大其全球业务版图。Expedera是领先的边缘推断人工智能（AI）半导体知识产权（IP）许可商，通过将原始图像和视频加载并进行搜索，无需元数据或标签即可解锁图像和视频的潜力。该公司已经签署了多家智能手机OEM和众多汽车、AR/VR/MR以及消费设备芯片组制造商的生产许可合同，其Origin IP已部署在超过1000万台设备上。

公司官网：https://www.expedera.com/

![](img/c8d2abb70c30e24952597338427c1736.png)

### WitnessAI获得2750万美元融资

贡献人：

https://www.prnewswire.com/news-releases/witnessai-raises-27-5-million-to-enable-safe-use-of-ai-302151702.html

WitnessAI宣布完成了2750万美元的A轮融资，由GV和Ballistic Ventures共同领投。该公司开发了首个安全AI使用启用平台，旨在为企业提供可见性、控制和保护。WitnessAI Secure AI Enablement Platform包括AI使用的可观察性模块、AI使用的政策执行和治理模块，以及针对员工、客户和企业数据的AI使用保护模块。该平台将于2024年6月开始早期部署，并已与20多个设计合作伙伴合作。融资将用于加速开发新的AI防护措施，并建立全球销售和支持组织。

公司官网：https://witness.ai/

![](img/caa61e173c08c1b28bf6386506364321.png)

### 澳大利亚初创公司SpatialAI融资340万美元

贡献人：

https://siliconangle.com/2024/05/20/australian-startup-spatialai-raises-3-4m-advance-ai-architecture-construction/

澳大利亚新兴的生成人工智能初创公司SpatialGPT.AI Pty. Ltd.宣布完成了510万澳元（340万美元）的种子轮融资，用于重新定义建筑、建设、教育、医疗保健和零售等行业的空间环境。该轮融资由InnovateGPT Pty. Ltd.领投，创始人为George Stavrakakis和Rupert Walsh，均为微软澳大利亚的资深员工。SpatialGPT的创始首席执行官Robert Marolda也是微软和VMware的前资深员工，在澳大利亚推出了微软的E5安全解决方案。SpatialGPT专注于利用Microsoft Azure OpenAI Service和Microsoft Copilot等技术构建AI解决方案，旨在帮助组织转变空间的建造、管理和使用方式。公司的技术旨在提高操作效率，增强精度，缩短延迟并优化成本。SpatialGPT首先将其AI技术应用于建筑行业的十亿美元项目，并计划逐步扩展到教育、医疗保健和零售等其他行业，重新构想这些行业的空间环境的使用方式。

公司官网：https://spatialgpt.ai/

![](img/59bc918dccaf4dd679a14da99b9435b3.png)

# 学习

### GPT-4o 与 LlamaParse 融合，推动文档处理新境界

贡献人：

https://ai.gopubby.com/unleashing-multimodal-power-gpt-4o-integration-with-llamaparse-31c1e5e9da3a

GPT-4o，作为一款尖端的多模态大型语言模型，在文档理解和解析方面展现了卓越的能力。与此同时，LlamaParse 作为行业领先的文档解析平台，能够有效地从各种文件格式中提取信息，为高级文档检索增强生成系统提供支持。两者的集成，不仅彰显了技术上的完美结合，还极大地提升了信息检索的精确度和效率。GPT-4o 的多模态理解能力，使得它能够更准确地解读文档内容，而 LlamaParse 的强大解析能力，确保了从复杂文件中高效提取信息的能力。这一集成，为人工智能在文档处理领域的应用打开了新的可能性，预示着未来在信息提取和整理方面的重大进步。

![](img/dfc5baf887e58ba9589b054bd2aa90ca.png)

### ModelScope-Agent框架新增一键配置多人聊天，配套开源多智能体数据集和训练

贡献人：

https://mp.weixin.qq.com/s/T9k9s4R0FP3cUH5JCLRsqg

ModelScope-Agent 框架引入了多智能体聊天室功能，通过 AgentEnvMixin 类和 Agent Registry Center 优化了 multi-agent 通信和管理逻辑，同时 Task Center 支持 agent 之间的消息订阅和发布。该框架结合 SWIFT 微调框架，利用开源的多智能体数据集，通过 Lora 等技术对模型进行微调，提升了模型的角色扮演能力。微调后的模型可通过 swift deploy 命令部署，并支持本地模型配置 Modelscope-Agent 聊天室。这一系列升级不仅提高了 AI Agent 应用的交互性和实用性，也为开发者提供了一个灵活、高效的多智能体交互研究和应用开发平台。

![](img/359ecf364767bf23bb64bc2ad4e8af30.png)

### 大型语言模型拒绝行为的单一方向控制机制

贡献人：

https://www.alignmentforum.org/posts/jGuXSZgv6qfdhMCuJ/refusal-in-llms-is-mediated-by-a-single-direction

在最新的研究中，对大型语言模型（LLMs）的拒绝行为进行了深入的分析，发现其拒绝机制是由单一的方向控制的。通过对比有害和无害指令的平均激活差异，研究人员提取了 “拒绝方向”，并证实了在多个开源模型家族和不同规模的模型上，通过改变这个方向可以有效地控制模型的拒绝行为。具体来说，通过在模型的残差流中消除这个方向，可以实现模型拒绝请求的绕过；而在无害指令上添加这个方向，则能诱导模型产生拒绝行为。这一发现不仅展示了对齐性和安全性微调的脆弱性，也提供了一种简单的方法来修改模型权重，实现模型的 “越狱”，而不需要额外的微调或推理时的干预。此外，研究还涉及到特征消除、子空间视觉化和模型权重直接修改等技术细节，这些发现对于理解和改进大型语言模型的安全性和对齐性具有重要意义。尽管这种技术可能会引入新的风险，但研究小组认为，披露这一方法并不会增加新的风险，反而有助于激发更加强大和安全的模型微调方法的研究和开发。

![](img/cc798799f8b7df27db066310b7754e93.png)

### Llama 3 微调，最先进的奖励模型，人类提示数据集...

贡献人：

https://www.interconnects.ai/p/f1b83a34-18cd-4507-b4b0-560902eb3275

Nathan Lambert 详细介绍了最新的开源大型语言模型（LLMs）和数据集。其中，lmsys-arena-human-preference-55k 数据集提供了价值丰富的人类提示，对于开放社区的模型训练具有重要意义。allenai 推出的 WildChat-1M 数据集，包含了 100 万个 ChatGPT 对话，为开放模型的训练提供了宝贵的资源。此外，H-D-T 团队的 Buzz 数据集凸显了其对话数据集的规模，近 300 万个对话实例为研究提供了丰富的样本。

文章还提到了 Meta 公司的 Chameleon 模型，其性能接近 GPT-4o，并在论文中详细探讨了模型的微调和安全性测试过程。这些技术细节不仅展示了 AI 领域的最新进展，也为研究人员提供了实质性的参考和资源。作者预告了即将发布的 DeepSeek v2 lite 模型，期待其在 AI 领域的应用和影响。

![](img/8e1f04990318778c4072dcec3a25d265.png)

### 函数拟合的艺术：Kolmogorov-Arnold Networks ( KAN) 源码实践

贡献人：

https://zhuanlan.zhihu.com/p/698511539?utm_psn=1775355093074141184

该文章通过具体的源码实践，展示了 Kolmogorov-Arnold Networks (KAN) 在函数拟合领域的潜力。首先，作者使用 Python 创建了一个数据集，用于训练和测试 KAN 模型。然后，通过调整熵正则化参数，实现了模型的训练和剪枝，以优化网络结构。接着，文章详细介绍了如何自动或手动设置每个神经元的符号函数，以及如何通过进一步的训练来提高拟合精度。最终，KAN 模型成功地拟合出了原始函数，证明了其在符号回归方面的强大能力。

通过这一系列的实践步骤，文章突出了 KAN 在处理复杂函数逼近问题时的高效性和可解释性。此外，作者还强调了 KAN 在实际应用中的潜在优势，如在不知道真实公式的情况下，通过多次实验可能会发现全新的物理公式。这篇文章为理解和应用 KAN 提供了宝贵的技术细节和实践经验。

![](img/b92a54578f6bcf4ab0e9b0e079bbdaf8.png)

### 详看Langchain框架中的RAG多阶段优化策略：从问题转换到查询路由再到生成优化

贡献人：

https://mp.weixin.qq.com/s/pK2BRLrWpEKKIPFhUtGvcg

Langchain 框架中的 RAG 多阶段优化策略，通过精细化的问题转换、查询路由和生成优化，提升了问题答题系统的性能。在问题转换方面，通过 Multi Query、RAG-Fusion、Decomposition、Step Back 和 HyDE 等策略，重写和修改问题以优化检索。查询路由优化则通过 Logical and Semantic routing 实现问题意图分类。在生成优化阶段，Self-RAG 通过自我反思机制改善答案的准确性和质量。此外，文章还强调了 Query Construction 在将自然语言转换为数据库 DSL 方面的重要性，以及 Indexing 和 Retrieval 在提高检索效率和准确性中的作用。这些技术细节展示了 RAG 如何通过多方面的优化，实现高效、准确的问答处理。

![](img/17ac5c8faff04ef789e4331afe159a38.png)

### MLP NN tag

贡献人:

https://gwern.net/doc/ai/nn/fully-connected/index

本网页是一个关于多层感知器（MLP）的综述性文档，它整理了大量关于 MLP 的研究论文，涵盖了从 2023 年回溯到 1986 年的技术发展。MLP 作为一种全连接神经网络，在机器学习领域中表现出了令人惊讶的潜力，尤其是在图像识别、语言处理和其他数据密集型任务中。文献中详细探讨了 MLP 的结构特点，如全连接层的堆叠、激活函数的选择以及网络优化的策略。此外，研究者们提出了多种改进 MLP 性能的技术，包括引入注意力机制、利用傅里叶变换提取特征、以及通过条件计算实现稀疏性。网页还对 MLP 与其他神经网络架构（如 CNN、RNN 和 Transformer）的比较进行了总结，提出了 MLP 在特定场景下的优势和局限性。特别值得注意的是，文献中提到了一些创新性的研究，如利用 MLP 进行生物学数据分析、药物设计以及在非传统领域如音乐和艺术创作中的应用。总体来看，这些研究不仅展示了 MLP 在深度学习中的广泛应用，也为未来的研究方向提供了丰富的思路和技术细节。

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

![](img/cc990f1ce111ed38ddd43047d57fd847.png)