
# 奇绩大模型日报（6月 13日）

> 来源：[https://miracleplus.feishu.cn/docx/TPmRdK4kpohyc4xgsQccJLHDnPb](https://miracleplus.feishu.cn/docx/TPmRdK4kpohyc4xgsQccJLHDnPb)

## 资讯

### IDC最新报告，7大维度11家大模型厂商比拼，唯一全优是谁？

https://mp.weixin.qq.com/s/P2Y7mT7XMYiq6ATA0FP8cA

如果考试题太简单，学渣也能拿一百昏。在 AI 圈，我们应该拿怎样的「试卷」来检验一直处于流量 C 位的大模型的真实水平？是高考题吗？当然不是！也有些人认为，在各种 Benchmark 榜单上，谁排第一谁最强。其实并非如此，有时候，越「权威」的榜单就越容易被策略性刷榜。因此，模型的「强」不能只是在某个 Benchmark 上排名第一，而是要在多个维度上都很能打。近日，全球领先的国际数据公司（IDC） 最新发布的大模型实测报告《中国大模型市场主流产品评估，2024》从基础能力到应用能力 7 大维度对 11 家大模型厂商的 16 款市场主流产品进行实测。报告显示，百度文心大模型整体竞争力位于领先水平，产品能力处于第一梯队，是唯一一家在 7 大维度上均为优势厂商的企业。文心一言、文心一格在问答理解类、推理类、创作表达类、数学类、代码类等基础能力，toC 通用场景类、toB 特定行业类等应用能力等 7 大维度均具备领先优势。其他评测厂商中，阿里获 6 项优势维度，OpenAI GPT-4 和商汤分获 5 项。

![](img/4a845af598c041847832649580006dfe.png)

### Stability AI开源Stable Diffusion 3 Medium文生图模型

https://huggingface.co/stabilityai/stable-diffusion-3-medium

6月12日晚间消息，人工智能初创公司Stability AI宣布正式开源发布其最新的文本到图像生成模型Stable Diffusion 3 Medium (SD3 Medium)。Stable Diffusion 3 Medium 包含 20 亿个参数，是 StabilityAI迄今为止最先进的文本到图像开放模型，更小的 VRAM 占用空间旨在使其更适合在消费级 GPU 以及企业级 GPU 上运行。

![](img/86acc56a68f98fffa4ef3d5224614ce5.png)

### 又一Sora级选手来炸街！

https://mp.weixin.qq.com/s/ADMuhRMeCiKYHvyTXkmgyA

Sora 再不开放使用，真的要被偷家了！今日，旧金山初创公司 Luma AI(打出一手王牌，推出新一代 AI 视频生成模型 Dream Machine。人人免费可用。

![](img/1338c7f34593b1903ba130cff516e781.png)

### 万字综述大模型高效推理：无问芯穹与清华、上交最新联合研究全面解析大模型推理优化

https://mp.weixin.qq.com/s/7LKfamTnCyFih6_grf9m3A

近年来，大语言模型（Large Language Models, LLMs）受到学术界和工业界的广泛关注，得益于其在各种语言生成任务上的出色表现，大语言模型推动了各种人工智能应用（例如ChatGPT、Copilot等）的发展。然而，大语言模型的落地应用受到其较大的推理开销的限制，对部署资源、用户体验、经济成本都带来了巨大挑战。例如，将包含700亿参数量的`LLaMA-2-70B`模型进行部署推理，至少需要6张RTX 3090Ti显卡或2张NVIDIA A100显卡，以部署在A100显卡上为例，该模型生成512长度的词块（`token`）序列需要耗时超过50秒。许多研究工作致力于设计优化大语言模型推理开销的技术，优化模型的推理延迟、吞吐、功耗和存储等指标，成为许多研究的重要目标。为了对这些优化技术有更全面、更系统的认知，为大语言模型的部署实践和未来研究提供建议和指南，来自清华大学电子工程系、无问芯穹和上海交通大学的研究团队对大语言模型的高效推理技术进行了一次全面的调研和整理，在《A Survey on Efficient Inference for Large Language Models》（简称`LLM Eff-Inference`）这篇万字长文综述将领域相关工作划分归类为三个优化层次（即数据层、模型层和系统层），并逐个层次地介绍和总结相关技术工作。此外，该工作还对造成大语言模型推理不高效的根本原因进行分析，并基于对当前已有工作的综述，深入探讨高效推理领域未来应关注的场景、挑战和路线，为研究者提供可行的未来研究方向。

![](img/3b1d43e9853bcc0204a2b3f671592756.png)

### 「AI+物理先验知识」，浙大、中国科学院通用蛋白质-配体相互作用评分方法登Nature子刊

https://mp.weixin.qq.com/s/mclq_etKI7chQp7QY46tig

蛋白质就像是身体中的精密锁具，而药物分子则是钥匙，只有完美契合的钥匙才能解锁治疗之门。科学家们一直在寻找高效的方法来预测这些「钥匙」和「锁」之间的匹配度，即蛋白质-配体相互作用。然而，传统的数据驱动方法往往容易陷入「死记硬背」，记住配体和蛋白质训练数据，而不是真正学习它们之间的相互作用。近日，浙江大学和中国科学院研究团队，提出了一种名为 `EquiScore` 的新型评分方法，利用异构图神经网络整合物理先验知识，并在等变几何空间中表征蛋白质-配体相互作用。`EquiScore` 基于一个新数据集进行训练，该数据集采用多种数据增强策略和严格的冗余消除方案构建。在两个大型外部测试集上，与其他 21 种方法相比，`EquiScore` 始终名列前茅。当 `EquiScore` 与不同的对接方法一起使用时，它可以有效增强这些对接方法的筛选能力。`EquiScore` 在一系列结构类似物的活性排序任务中也表现出色，表明其具有指导先导化合物优化的潜力。最后，研究了 `EquiScore` 的不同可解释性水平，这可能为基于结构的药物设计提供更多见解。

![](img/47abf05414de79b0e6bf4e315049b833.png)

## 推特

### SkalskiP分享：`supervision`超精准识别Demo，已在GitHub上突破了15,000颗星

https://x.com/skalskip92/status/1801025914647478532?s=46&t=GRStLXDcUNuun8J5Noyw4Q

`supervision`——我创建的一个计算机视觉库——刚刚在GitHub上突破了15,000颗星！

BBBRRRRRR！

链接：[https://github.com/roboflow/supervision](https://github.com/roboflow/supervision)

![](img/b945a5d221f4feded3411b53f0dd0282.png)

[ssstwitter.com_1718233117114.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QqbebW7zpo7GH8xZQrHcXjE2nhh)

### Mixture-of-Agents (MoA)模型：选择具有不同优势的多个`LLM`，创建多层架构，使用前一层的输出作为辅助信息

https://x.com/_philschmid/status/1801160312910974993?s=46&t=GRStLXDcUNuun8J5Noyw4Q

迭代增强的`LLM`输出在`AlpacaEval 2.0`、`MT-Bench`和`FLASK`上超越了`@OpenAI` 的`GPT-4 Omni`！🤯 `Mixture-of-Agents`（`MoA`）使用多层架构中的多个`LLM`来迭代增强生成质量。

`Mixture-of-Agents` (`MoA`)

1️⃣ 选择具有不同优势的多个`LLM`。

2️⃣ 创建一个多层架构，其中每一层都由多个`LLM`组成。

3️⃣ 每一层都使用前一层的输出作为辅助信息来生成其响应。

洞察

🌟 `MoA`模型在`AlpacaEval 2.0`上获得了65.1%的高分，远超`GPT-4 Omni`的57.5%。

🤖 当`LLM`面对其他模型的输出时，往往会生成更好的响应。

📊 来自多个`LLM`的输出的分层聚合会带来更好的响应。

🔍 每层`MoA`所选的`LLM`对性能和多样性至关重要。

🚀 `MoA-Lite`变体以两倍的成本效益比超越了`GPT-4 Turbo` 4%。

⏳ 模型响应的聚合可能会导致较高的首次令牌时间（`TTFT`）。

🧪 这可能对合成数据生成和评估很有趣。

论文：[https://huggingface.co/papers/2406.04692](https://huggingface.co/papers/2406.04692)

代码：[https://github.com/togethercomputer/moa](https://github.com/togethercomputer/moa)

来自`@togethercompute`的出色工作！

![](img/f527815cfdebd7f9b52ac55056a464a7.png)

### Jim Fan分享视频：人类在“跨形态迁移”方面的非凡能力和具身智能

https://x.com/drjimfan/status/1800905021405729245?s=46&t=GRStLXDcUNuun8J5Noyw4Q

一个引人注目的例子展示了人类在“跨形态迁移”方面的非凡能力：一个人通过将跟随他的无人机摄像头传输到他的VR头显中，从第三人称“NPC视角”来操控自己。我们天生处理自我中心视角，但却能毫无困难地在没有训练的情况下转变到星光层面。

在`AI`代理的文献中，“具身”通常指的是（观察空间，动作空间）的元组。第一人称到第三人称是观察空间的重大转变，尤其是如果代理没有在新场景中进行广泛训练。没有已知的代理算法能在开箱即用的情况下克服如此巨大的分布不匹配。

另一个人类跨形态迁移的例子是特斯拉`Optimus`操作员。他们通过`VR`设备和动作捕捉手套远程操作类人机器人。动作空间中存在显著的差距：从生物关节到机械关节的不完美映射、延迟以及缺乏精细的触觉反馈。然而，人类几乎在没有训练（或最多只有一点训练）的情况下就能适应新身体。

视频和创意来源：`@kylebrussell`

![](img/e5b8177d60f2d4e84931b9aeddbe7fd4.png)

[ssstwitter.com_1718286552029.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Q1ilbc2Q4oCmexxrEaIcfDOsnwc)

### George Hotz ：13个月前，我们说过要让`AMD`进入`MLPerf`，今天，比`AMD`提前做到了

https://x.com/realgeorgehotz/status/1800932122569343043?s=46&t=GRStLXDcUNuun8J5Noyw4Q

13个月前，我们说过要让`AMD`进入`MLPerf`。今天，在`MLPerf Training 4.0`中，我们`@__tinygrad__`比`@AMD`提前做到了。

![](img/26a4b095d9734ab118bf6865c3543005.png)

### Anthropic AI分享：AI系统红队测试中的挑战

https://x.com/AnthropicAI/status/1800943123343745221

在这篇文章中，我们详细介绍了我们用于测试`AI`系统的红队测试方法的见解。通过这种实践，我们开始收集关于在特定情况下使用适当工具的实证数据，以及每种方法的相关优势和挑战。我们希望这篇文章能对其他试图进行`AI`系统红队测试的公司、对红队测试实际操作感兴趣的政策制定者以及想要对`AI`技术进行红队测试的组织有所帮助。

![](img/5ccb48831b93ec488774a49a80157afb.png)

## 论文

### 如果我们用`LLaMA-3`重新添加数十亿张网络图片的描述会怎样？

贡献人：

链接：http://arxiv.org/abs/2406.08478v1

摘要：网络爬取的图像文本对存在固有的噪音。先前的研究表明，语义对齐和丰富这些对的文本描述可以显著增强各种视觉语言任务的模型训练，特别是文本到图像生成。然而，这一领域的大规模调查仍然主要是闭源的。我们的论文旨在搭建这一社区努力，利用强大且开源的 `LLaMA-3`，一种`GPT-4`级`LLM`。我们的重新标题流程很简单：首先，我们微调一个由`LLaMA-3-8B`驱动的`LLaVA-1.5`，然后利用它重新标题`DataComp-1B`数据集中的13亿个图像。我们的实证结果证实，这种增强的数据集`Recap-DataComp-1B`在训练先进的视觉语言模型方面带来了实质性的好处。对于像`CLIP`这样的辨别模型，我们观察到在跨模态检索任务中的零-shot性能有所提升。对于像文本到图像`Diffusion Transformers`这样的生成模型，生成的图像在与用户文本指令的对齐方面显示出明显改善，特别是在遵循复杂查询时。我们的项目页面是https://www.haqtu.me/Recap-Datacomp-1B/

![](img/22af3734201776fc857fc67c96760190.png)

### 一项基于 `Mamba` 的语言模型的实证研究

贡献人：

链接：http://arxiv.org/abs/2406.07887v1

选择性状态空间模型（`SSM`）如`Mamba`克服了`Transformer`的一些缺点，例如随着序列长度呈二次计算复杂性和查询时间需要大量内存的无法容忍。此外，最近的研究表明，`SSM`可以匹配或超过`Transformer`的语言建模能力，使其成为一个有吸引力的替代方案。然而，迄今为止的研究只是在受控设置下（例如，相同的数据集）展示了小规模实验，比较了`SSM`和`Transformer`。为了了解这些架构在更大规模上的优势和劣势，我们在相同的数据集上直接比较了`8B`参数的`Mamba`、`Mamba-2`和`Transformer`模型，训练的`token`数量多达`3.5T`。我们还将这些模型与一个由`43%` `Mamba-2`、`7%`注意力和`50%` `MLP`层组成的混合架构（`Mamba-2-Hybrid`）进行了比较。通过使用各种任务，我们回答了`Mamba`模型能否在更大的训练预算下匹配`Transformer`的问题。我们的结果显示，尽管纯`SSM`在许多任务上与`Transformer`匹配或超过，但是在需要强大的复制或上下文学习能力（例如，`5-shot` `MMLU`、电话簿）或长上下文推理的任务上，它们落后于`Transformer`。相反，我们发现`8B`的`Mamba-2-Hybrid`在所有`12`项标准任务上超越了`8B`的`Transformer`（平均增加`2.65`个点），在生成`token`时推理速度预计可高达`8`倍。为验证长上下文能力，我们进行了额外的实验，评估了针对支持`16K`、`32K`和`128K`序列的`Mamba-2-Hybrid`和`Transformer`的变体。在额外的`23`个长上下文任务中，混合模型在平均水平上继续接近或超过`Transformer`。为了进一步研究，我们将检查点和用于训练模型的代码作为`NVIDIA`的`Megatron-LM`项目的一部分发布。

![](img/e454f590149faa76b6ed168147a2f4dc.png)

### 下一代数据库界面：基于`LLM`的文本到`SQL`的调查

贡献人：

链接：http://arxiv.org/abs/2406.08426v1

生成准确的`SQL`是一个长期存在的问题，因为对自然语言问题进行处理、数据库架构理解和`SQL`生成都是具有挑战性的。传统的`text-to-SQL`系统包括人工工程和深度神经网络。随后，预训练语言模型（`PLMs`）已经被开发并用于`text-to-SQL`任务，取得了令人期待的表现。随着现代数据库变得更加复杂以及相应的用户问题变得更具挑战性，具有有限理解能力的`PLMs`可能会导致不正确的`SQL`生成。最近，大型语言模型（`LLMs`）在自然语言理解方面展现了显著的能力。因此，将`LLM-based`实施集成到`text-to-SQL`中可以为研究带来独特的机会、挑战和解决方案。在这项调查中，我们对基于`LLM`的`text-to-SQL`进行了全面的审查。具体来说，我们提出了当前挑战的简要概述和`text-to-SQL`的演进过程。然后，我们详细介绍了设计用于评估`text-to-SQL`系统的数据集和度量标准。之后，我们对基于`LLM`的`text-to-SQL`的最新进展进行了系统分析。最后，我们讨论了这一领域仍然存在的挑战，并提出了未来发展的期望。

![](img/f4ded970a17dddbe1df5677c50908bfa.png)

### 线性回归的缩放定律：计算、参数和数据

贡献人：

链接：http://arxiv.org/abs/2406.08466v1

大规模深度学习模型经验上通常满足神经规模定律：训练模型的测试错误随着模型规模和数据规模的增长呈多项式改进。然而，传统智慧认为测试错误包括逼近、偏差和方差误差，其中方差误差随着模型规模增加而增加。这与神经规模定律的一般形式不符，后者预测增大模型规模会单调地改善性能。

我们研究在无限维线性回归设置中的定律理论。具体而言，我们将具有`M`参数的模型视为对简化协变量的线性函数。该模型通过一次随机梯度下降（`SGD`）使用`N`数据进行训练。假设最优参数满足高斯先验，并且数据协方差矩阵具有幂律谱，度数为`a>1`，我们表明测试错误的可降部分为`$\Theta(M^{-(a-1)} + N^{-(a-1)/a} )$`。随着`M`增加的方差误差被由于`SGD`的隐式规整化而被主导的其他误差所掩盖，因此从这个界限中消失。我们的理论与经验神经规模定律一致，并经数值模拟验证。

![](img/d437a89842617552cb0af00a2005e2bb.png)

### 短长卷积有助于硬件高效的线性注意力集中处理长序列

贡献人：

链接：http://arxiv.org/abs/2406.08128v1



# 引言

为了缓解处理长序列中的自注意力机制的计算复杂性，线性注意力利用计算技巧实现了线性复杂度，同时状态空间模型（SSMs）普遍采用一种良好的做法，即使用非数据依赖的记忆模式，即强调近处而忽略远处，来处理序列。最近的研究表明了通过将它们结合为一个整体可以获得优先级。然而，线性注意力的效率仍然仅停留在理论级别上，在因果设置中，而SSMs需要各种设计良好的约束条件才能有效地处理特定数据。因此，为了揭示混合设计真正的威力，需要解决以下两个问题：（1）线性注意力的硬件高效实现和（2）SSMs的稳定性。为了实现这一目标，我们利用平铺和层次分层的思想提出了CHELA（短长卷积与硬件高效线性注意力），它用短长卷积替换了SSMs，并以分治方式实现线性注意力。这种方法享有来自稳定SSM和线性注意力的全局抽象和数据依赖选择，同时保持真正的线性复杂度。我们在Long Range Arena基准和语言建模任务上进行了全面实验，证明了所提出方法的有效性。

![](img/8268ad05a4e0ed902654c493ba98833e.png)

# 项目推荐

### UICoder：通过自动反馈微调大语言模型以生成用户界面代码

贡献人：

链接：`http://arxiv.org/abs/2406.07739v1`

大语言模型(LLMs)在生成符合视觉设计要求且可编译的UI代码时存在困难。现有方法依赖昂贵的人工反馈或提炼专有模型来改进生成过程。本文提出使用自动化反馈(编译器和多模态模型)引导LLMs生成高质量UI代码。我们的方法从现有LLM开始，通过自动生成大规模合成数据集，利用自动化工具对数据进行严格过滤、评分和去重，生成更优质的数据集，然后通过在此数据集上微调原LLM来改进模型。我们将这种方法应用到几个开源LLMs上，并通过自动化指标和人类偏好与基准模型进行比较。评估结果显示，生成的模型胜过所有其他可下载的基线模型，并接近较大的专有模型性能。

![](img/e2807be33bba82321f9678b79971ec46.png)

# 产品

### Namify AI

贡献人：

`https://namify.tech/?`

Namify AI 是一款免费的企业名称生成器,它不仅使用 AI 提供最适合用户产品或初创公司的品牌名称建议,还可以进行商标、域名和社交媒体用户名检查,让用户自定义名称以匹配品牌风格,并支持多种语言,还免费提供品牌徽标设计,帮助用户快速找到可用且有吸引力的品牌名称,是一款专为企业主和创业者设计的全面性名称生成和检查工具。

![](img/0a727a9ac406bd83eefbdd5076edf867.png)

### Autodesigner 2.0

贡献人：

`https://uizard.io/?ref=producthunt`

Autodesigner 2.0 是 Uizard 公司推出的一款 AI 驱动的 UI 设计生成工具,将 ChatGPT 的对话交互和 Uizard 的 UI 设计功能相结合,帮助用户快速将创意转化为数字产品概念。Autodesigner 2.0 通过持续的技术迭代升级,已经成为 Uizard 平台上超过 80% 新 UI 的生成引擎。

![](img/a4b81e2231624cac9adc56de6dd07f7d.png)

# HuggingFace&Github

### L3-8B-Stheno-v3.2

贡献人：

`https://huggingface.co/Sao10K/L3-8B-Stheno-v3.2`

L3-8B-Stheno-v3.2 在训练过程中融合了多种数据集,包括短篇故事生成、指令式对话以及聊天日志数据。与上一版本相比,该模型在处理 SFW 和 NSFW 内容、故事写作、助手任务等方面都有所改进,同时在多轮对话连贯性和提示遵循度上也有所提升,尽管创造力略有下降。作者推荐使用特定的采样设置来充分发挥该模型的性能。

![](img/664d70ac4b5f970fed5ca2e23d211c86.png)

### Aidapal

贡献人：

`https://github.com/atredispartners/aidapal`

aidapal 是一个集成了大型语言模型的 IDA Pro 插件,利用人工智能技术来辅助逆向工程师分析二进制代码。它使用了微调过的 LLM 模型来生成 Hex-Rays 伪代码,帮助提高代码分析的效率和准确性。aidapal 的源代码、模型权重和训练数据都可以从 Hugging Face 上下载,开发者可以自行部署和使用。这个插件结合了机器学习和逆向工程领域的前沿技术,为二进制代码分析带来了新的解决方案。

![](img/f622d1e7ec12a469b0a28b6e56d3d1a5.png)

# 投融资

### Black Semiconductor 融资2.74亿美元以推动欧洲芯片发展

贡献人：

`https://www.cnbc.com/2024/06/12/black-semiconductor-raises-274-million-in-funding-in-europe-chip-push.html`

Black Semiconductor是一家致力于开发下一代芯片技术的初创公司，宣布筹集了2.544亿欧元（约2.74亿美元）。其中，德国经济事务部和北莱茵-威斯特法伦州政府提供了2.287亿欧元（约2.48亿美元）的公共资金，剩余的2570万欧元（约2800万美元）由保时捷创投和风险投资公司Project A领投。资金将用于在德国亚琛建立试生产设施，并计划在2031年前实现大规模生产。此外，公司计划在2026年前将员工人数从30人增加到120人。此举显示了欧洲在半导体制造领域追赶美国的雄心。

公司官网：`https://blacksemi.com/`

![](img/c6ad3502015a710e0aa2ceee992f9d35.png)

### BlinqIO 融资500万美元以利用生成式AI自动化软件测试

贡献人：

`https://siliconangle.com/2024/06/12/blinqio-raises-5m-automate-software-testing-using-generative-ai/`

BlinqIO是一家生成式AI软件测试初创公司，宣布完成了500万美元的融资，主要投资者为Flint Capital和Inovia Capital的Precede Fund I。BlinqIO开发了业内首个AI测试自动化工程师，能够自主创建和维护测试自动化代码，无需人工监督。该技术利用专有的大型语言模型，支持多达50种语言，旨在提高软件测试的效率。融资将用于进一步发展其自动化测试工具，帮助企业更频繁地推出新软件产品和服务。

公司官网：`https://blinq.io/`

![](img/3287d65e39a32010b3bf878682fcb579.png)

### Pyte 融资500万美元以推动企业私密和安全的数据利用与协作

贡献人：

`https://www.prnewswire.com/news-releases/pyte-announces-5m-in-funding-to-advance-private-and-secure-data-utilization-and-collaboration-for-every-enterprise-302170244.html`

Pyte宣布获得500万美元的新一轮融资，由Myriad Venture Partners领投，Innovation Endeavors、Liberty Mutual Strategic Ventures和Pillar VC等参投，使总融资金额超过1200万美元。Pyte利用其专有的安全多方计算（SMPC）技术，允许企业在数据加密状态下进行复杂的AI和机器学习计算。此次融资将用于加速其技术在金融服务和医疗等高度监管行业的商业化应用，帮助企业在保证数据隐私和安全的前提下实现数据协作。

公司官网：`https://www.pyte.ai/`

![](img/a92667f960be8b06d644c8fd0842880f.png)

### InScope 融资430万美元以推动自动化财务报告

贡献人：

`https://techcrunch.com/2024/06/12/sheel-mohnot-inscope-financial-reporting/`

InScope是一家自动化财务报告的金融科技初创公司，近日宣布完成了由Lightspeed Venture Partners领投的430万美元种子轮融资。InScope利用机器学习和大型语言模型为中型市场和企业提供财务报告和审计服务。公司将利用这笔资金扩展产品、扩大团队并增强AI能力，目标是在年底前将客户数量增加到50家。参与投资的还有Together AI的创始人兼CEO Vipul Ved Prakash和Casetext的创始人兼CEO Jake Heller等个人投资者。

公司官网：`https://www.inscopehq.com/`

![](img/be06090263eea5bb926c07443eb08b13.png)

# 学习

### 潞晨训推一体机，助力算力高效利用

贡献人：

`https://mp.weixin.qq.com/s/kVF2fOR8_i7EVoUrt3vBIg`

潞晨科技推出的训推一体机，采用英伟达 H20 GPU，每台配备 8 张卡，每卡 96GB 显存，支持 32B Qwen1.5 模型全参数微调和 34B Yi1.5 模型推理。该机器利用 Colossal-AI 加速系统，在 8 卡规模下，平均每卡算力提升 21%，实现快速模型微调和推理。集成了多种开源模型，如 LLaMA3、Mixtral、Qwen 等，并提供了 Colossal LLM Studio 软件，实现了低代码、可视化的模型训练和微调。此外，还提供了 Colossal Reader 和 Open-Sora 等应用，分别针对文档理解和视频生成进行了深度优化。这款一体机的推出，旨在解决企业在 AI 落地过程中的算力利用问题，降低技术门槛，促进 AI 技术的商业化应用。

![](img/79d349f2441b4bbe37f56ac4ebe23b0f.png)

### 如何理解Nvidia英伟达的Multi-GPU多卡通信框架NCCL？

贡献人：

`https://www.zhihu.com/question/63219175?utm_psn=1784234882622324736`

NCCL 库：NVIDIA NCCL 库是一个针对多 GPU 并行通信优化的库，支持深度学习框架中的 collective communications，如 `all-reduce`, `broadcast`, `reduce` 等。

性能优化：NCCL 在不同硬件环境下进行了优化，特别是在 NVIDIA 的 GPU 上，通过利用 NVLink 和 InfiniBand 技术，显著提高了通信性能。

Ring-Allreduce 算法：在单机多卡场景下，NCCL 采用 Ring-Allreduce 算法，通过数据切片和环形通信结构，实现高效的数据传输和聚合。

多机通信：对于多机多卡的环境，NCCL 使用了 Double Binary Tree 和 CollNet 技术，后者基于 SHArP 协议，可在网络交换机上进行数据聚合，减少通信延迟和带宽压力。

硬件拓扑 sensing：NCCL 能够自动探测系统的链路拓扑，根据探测结果创建最优的通信环路，从而实现最佳的通信性能。

与深度学习框架集成：NCCL 与主流深度学习框架兼容，允许开发者在这些框架中无缝集成，以便进行高效的并行计算。

实际性能测试：文章中包含了在不同的 GPU 配置（如 DGX-1 V100、DGX A100）下进行的 NCCL 性能测试，实际案例显示 NCCL 能够提供高达 130GB/s 甚至更高的通信带宽。

![](img/04839271bf7a647870b303314ed99fc1.png)

### 当"狂飙"的大模型撞上推荐系统

贡献人：

`https://mp.weixin.qq.com/s/hQ5MYFxdKA4dgXetFEc_Bg`

大型人工智能模型如 GPT-3 和 BERT，利用深度学习技术，能够高效处理自然语言数据，显著提升文本理解和生成的能力。这些模型通过大量数据训练，掌握了语言的复杂模式和结构，从而在搜索引擎和推荐系统中提供更加精准的服务。技术细节上，GPT-3 采用 1750 亿参数的转换器模型，通过无监督学习方式从互联网数据中学习语言知识。BERT 则通过双向训练策略，能够更好地理解搜索语句的上下文含义。结合推荐系统时，这些模型能够根据用户行为和偏好，提供个性化推荐，同时面临着解释性、计算成本和多样性平衡等技术挑战。未来发展将聚焦于提高模型的透明度和可解释性，同时确保推荐系统的公平性和用户隐私保护。

![](img/985d80c907203e50afbd766b0a54efb0.png)

### AI时代进击的CPU们

贡献人：

`https://zhuanlan.zhihu.com/p/702957407?utm_psn=1784491896913133569`

AI 时代下，CPU 面临着性能提升的巨大挑战，主要集中在增加核心数量、提高每个核心的性能、优化指令集以及提升内存访问速度上。多核处理器成为提升并行处理能力的关键，通过提高时钟频率和微架构优化，每个核心的计算效率得到显著提升。为了适应 AI 计算，指令集优化被视为提高处理速度的重要手段，支持更多 AI 指令，降低计算复杂度。此外，内存系统的优化也不容忽视，高速缓存和内存接口的改进对提升 CPU 性能至关重要。未来的 CPU 将更加注重能效比，通过异构计算架构，与 GPU、TPU 等处理器协同工作，以满足 AI 时代的计算需求。

![](img/eeffb119b0ee37ca82aefdf82e0f41bb.png)

### Jina CLIP v1：多模态向量模型，为文本和图像而生！

贡献人：

`https://mp.weixin.qq.com/s/9cIAP9PJRHE4-N6AKhn_SQ`

Jina CLIP v1 是一个基于 OpenAI CLIP 的多模态向量模型，专注于处理文本和图像数据。该模型通过大规模中文数据集进行训练，提升了对中文内容的理解能力。技术上，Jina CLIP v1 实现了文本和图像到共享向量空间的映射，使得跨模态的相似性搜索成为可能。此外，模型支持微调，可以针对特定应用进行优化。用户可以通过 Jina 框架轻松地调用 API 进行文本和图像编码，并进行相似性搜索。该模型适用于多模态内容理解和检索，为开发者提供了一个强大的工具，以实现更智能的搜索和内容分析应用。

![](img/d0b158c7e64907159a1234d85bcd4f52.png)

### 再谈苹果AI

贡献人：

`https://mp.weixin.qq.com/s/pZeoVk-ezf7jHtftWWm3cA`

文章详细介绍了苹果发布的Apple Intelligence技术，其核心由端侧大模型、云端大模型和ChatGPT三部分组成。苹果端侧大模型拥有约30亿参数，每秒生成30个token，初始响应延迟约为0.6毫秒；苹果云端大模型的处理能力相当于GPT-3.5。新Siri在Apple Intelligence加持下，实现了跨应用信息整合、连续对话和屏幕内容理解的能力。这些技术进步依赖于苹果系统、芯片和模型的高度集成，使其在AI体验上具有独特优势。文章还分析了苹果产品的创新周期及其对未来出货量的影响，并提出了相关的投资建议。

![](img/7572f538f9a0b2ef82a099656bf7d676.png)

### 巴黎开源AI开发者见面会

贡献人：

`https://lu.ma/docker-paris`

Docker 将在 2023 年 6 月 20 日星期四，时间为 18:00 - 21:00 GMT+2，在巴黎的 STATION F 举办一个本地和开源的 AI 开发者聚会。聚会需要主办方批准注册，并且参与者需要使用钱包验证代币所有权。活动包括开门迎宾、Koyeb、LlamaIndex、Giskard、Red Hat 和 Docker 等公司的闪电演示，以及后续的网络交流环节。参与者可以在活动中拾取由合作伙伴 Ollama 提供的 Ollama 钥匙扣。

![](img/f82dd2abe4fb96ee8d17ab10900b67b7.png)

> 潜空间系列活动第二季启动！

> 第2季第1期将在6.15 10:00开始，嘉宾是姚顺雨。活动包括嘉宾分享、访谈Q&A、cofounder matching餐会。

> 姚顺雨-普林斯顿计算机系博士，ReAct、思维树ToT、SWE框架提出者；

> Jesse Lyu吕骋-最热🔥🔥🔥AI硬件Rabbit- R1创始人；

> 尤洋-新加坡国立大学校长青年教授，曾创造Imagenet、Bert等多项训练速度世界纪录；

> 蔡天乐-普林斯顿“小天才”、美杜莎推理优化论文作者。

本季活动同时开放北京线下、上海线下和线上场次，欢迎校友和外部创业者扫描右方二维码报名！

![](img/aeb13d6bd0756bcb4754b4e2ed8686a3.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报知识库】**这里是近期日报内容的汇总,欢迎大家查阅

> `https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh`

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/05a37475a0388faf13fd34157aea7bed.png)
