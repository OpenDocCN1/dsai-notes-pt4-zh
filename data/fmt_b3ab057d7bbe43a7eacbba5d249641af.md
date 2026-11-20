
ZML：一个高性能的AI推理框架，能够并行化并在多种硬件上运行深度学习系统。它已经不再保密，非常出色，并且是开源的。

https://github.com/zml/zml

![](img/5b53e19724970e695330eb5d04e95118.png)

## 帅Otto！让你可以在表格中使用 AI 代理，在几分钟内自动化处理数小时的手动研究工作

https://x.com/SullyOmarr/status/1836059834543734892

终于激动地推出 Otto！

Otto 让你可以在表格中使用 AI 代理，在几分钟内自动化处理数小时的手动研究工作。

这里是我对如何使用它的简要介绍，以及一些真实的应用场景。

![](img/b3ce0148638fbe647b3b1bc4115071b1.png)

[ssstwitter.com_1726650373416.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WAHebfG5Boy98cxVhUgcomp2n74)

## 通过使用自生成数据改进大型语言模型的推理：强化学习和验证器

https://x.com/agarwl_/status/1836119825216602548

Rishabh Agarwal：今天我在麦吉尔大学的一门关于大型语言模型的研究生课程中进行了我的第一次客座讲座，作为一名即将成为兼职教授的人。我把幻灯片放在这里，或许对一些人有用 ;)

https://drive.google.com/file/d/1komQ7s9kPPvDx_8AxTh9A6tlfJA0j6dR/view?usp=sharing

![](img/4f4a0dbe2ccb668054e49c9c666b9d95.png)

## Pixtral 12B：由 MistralAI 推出的首款多模态模型

https://x.com/dchaplot/status/1836086836789436810

非常激动地宣布 `Pixtral 12B` —— 由 @MistralAI 推出的首款多模态模型！

+   在文本和多模态基准测试中表现强劲
+   全新的 400M 视觉编码器
+   支持可变的图像尺寸和多图像输入
+   128 序列长度
+   Apache 2.0 许可

🔗 https://mistral.ai/news/pixtral-12b/

![](img/47b29302512ab50c8cba053bc55e030c.png)

## YC创始人Friedman：垂直大型语言模型agent是新的垂直SaaS

https://x.com/snowmaker/status/1835877593024999695

垂直大型语言模型`agent`是新的垂直`SaaS`，这是生成十亿美元公司创意的最简单方式。

![](img/e21708bcd12f144a36428be062939a3b.png)

# 论文

## 自注意力机制限制基于Transformer模型的工作记忆容量

贡献人：

链接：https://arxiv.org/abs/2409.10715

近期关于基于`Transformer`的大型语言模型（`Large Language Models`, `LLMs`）的研究揭示了其工作记忆容量的显著限制，这与人类行为研究中发现的现象相似。具体来说，这些模型在执行`N-back`任务时，随着`N`的增加，其性能显著下降。然而，关于这一现象产生的机制性解释仍然缺乏。受行为科学中执行注意力理论的启发，我们假设`Transformer`模型中的自注意力机制可能是其工作记忆容量限制的原因。为了验证这一假设，我们训练了基础的仅解码器（`decoder-only`）`Transformer`模型执行`N-back`任务，并发现注意力分数在训练过程中逐渐聚集到`N-back`位置，这表明模型通过学习一种策略，即关注当前位点与`N-back`位点之间的关系，从而掌握了任务。关键的是，我们发现随着`N`的增加，注意力分数矩阵的总熵也在增加，这表明注意力分数的分散可能是导致`N-back`任务中观察到的容量限制的原因。

![](img/702bc2e3f90c7afddea9e464b8955951.png)

![](img/8653e3904d30d70ea14844540fffd6c1.png)

## 模型自我指引注意力方向：忠实性与自动注意力引导的结合

贡献人：

链接：https://arxiv.org/abs/2409.10790

大型语言模型（`Large Language Models`, `LLMs`）在各种现实任务中展示了卓越的性能。然而，它们常常难以充分理解和有效利用输入上下文，导致生成的回答不够忠实或出现幻觉（`hallucinated`）。这种困难在处理长上下文或包含干扰信息的情况下更加明显，这些信息可能会使`LLMs`无法完全捕捉到关键证据。为了解决这一问题，许多研究通过提示（`prompting`）方法帮助`LLMs`更忠实地利用上下文信息。例如，迭代提示通过两步突出关键信息，首先要求`LLM`识别重要的上下文片段，然后据此推导答案。然而，提示方法仅限于在令牌空间中隐式地突出关键信息，这通常不足以完全引导模型的注意力。为了更可靠地提升模型的忠实性，我们提出了`AutoPASTA`，一种自动识别关键信息并通过引导`LLM`的注意力分数显式突出这些信息的方法。与提示方法类似，`AutoPASTA`在推理时应用，无需更改任何模型参数。我们在开放书籍问答（`open-book QA`）上的实验表明，`AutoPASTA`能够有效地帮助模型抓住关键的上下文信息，从而显著提升模型的忠实性和性能，例如，`LLAMA3-70B-Instruct`的平均性能提升了7.95%。`AutoPASTA`的代码将公开提供，访问链接为https://github.com/QingruZhang/AutoPASTA

![](img/732ea466013ed7549e172b556c7fddfd.png)

![](img/d6a64c09ead9cae9916647fc0265b6f1.png)

## 调查大型语言模型中的上下文忠实性：记忆强度与证据风格的作用

贡献人：

链接：https://arxiv.org/abs/2409.10955

检索增强生成（`Retrieval-augmented generation`, `RAG`）通过将外部信息纳入响应生成过程，提升了大型语言模型（`Large Language Models`, `LLMs`）的性能。然而，`LLMs`在多大程度上能够忠实地利用上下文信息，以及哪些因素影响`LLMs`的上下文忠实性，仍然在很大程度上未被探讨。在本研究中，我们调查了记忆强度和证据呈现方式对`LLMs`接受外部证据的影响。我们引入了一种方法，通过测量`LLMs`对同一问题不同释义的响应差异，量化`LLMs`的记忆强度，这一点在之前的研究中未被考虑。我们还生成了多种风格的证据，以评估不同风格的证据对`LLMs`影响的效果。我们使用了两个数据集进行评估：包含热门问题的自然问题（`Natural Questions`, `NQ`）和涵盖长尾问题的`popQA`。结果显示，对于记忆强度高的问题，`LLMs`更倾向于依赖内部记忆，尤其是像`GPT-4`这样的大型`LLMs`。另一方面，与简单重复或添加细节相比，呈现释义后的证据显著提高了`LLMs`对外部证据的接受度。

![](img/0b86728ad847be3b17bfa8d436804c85.png)

![](img/a92153502b431f39a6cfdcc40a458ef6.png)

## Less is More: 一种简单而有效的令牌减少方法以提升多模态大型语言模型的效率

贡献人：

链接：https://arxiv.org/abs/2409.10994

随着多模态大型语言模型（`Multimodal Large Language Models`, `MLLMs`）的快速发展，这些模型在各个领域中展现了卓越的性能。然而，这一进步伴随着模型资源消耗的显著增加。针对这一紧迫问题，我们提出了一种新方法——基于`CLIP`度量的令牌减少（`Token Reduction using CLIP Metric`, `TRIM`），旨在提高`MLLMs`的效率，同时不牺牲其性能。受到人类在视觉问答（`Visual Question Answering`, `VQA`）任务中注意力模式的启发，`TRIM`为图像令牌的选择和减少提供了新的视角。我们在12个数据集上广泛测试了`TRIM`方法，结果显示在保持一致性能水平的同时，显著降低了计算开销。这项研究标志着高效`MLLM`开发的重要一步，促进了高性能模型的更广泛可访问性和可持续性。

![](img/52a04e807cd37bab9d21ec196da004ae.png)

## 对量化指令调优大型语言模型的全面评估：405B规模的实验分析

贡献人：

链接：https://arxiv.org/abs/2409.11055

先前的研究工作使用有限的指标（如困惑度）或少数基础知识任务以及较旧的数据集对量化大型语言模型（`Large Language Models`, `LLMs`）进行了评估。此外，诸如`Llama 3.1`等最近的大规模模型（最高可达405B）尚未得到充分的检验。本文评估了指令调优`LLMs`在各种量化方法（`GPTQ`、`AWQ`、`SmoothQuant`和`FP8`）下，从7B到405B不同规模模型的性能。通过13个基准测试，我们在六种任务类型上评估了性能，包括常识问答、知识与语言理解、指令遵循、幻觉检测、数学和对话。我们的主要发现如下：1.大规模`LLM`的量化性能优于小规模`FP16` `LLM`：将较大的`LLM`量化到与较小的`FP16` `LLM`相似的规模，通常在大多数基准测试中表现更好，除了在幻觉检测和指令遵循任务上除外。2.量化方法、模型规模和位宽对性能影响显著：不同的量化方法、模型规模和位宽对性能有显著影响，其中仅权重量化方法在较大模型上往往表现更佳。3.任务难度对量化引起的准确性下降影响不显著：任务的难度并未显著影响由于量化导致的准确性下降。4.`MT-Bench`评估方法对近期高性能`LLMs`的区分能力有限。

![](img/0082bb5cf7efe71fd365689bf0fd281a.png)

## Unveiling Induction Heads：Transformer中的可证训练动态与特征学习

贡献人：

链接：https://arxiv.org/abs/2409.10559

上下文学习（`In-context Learning`, `ICL`）是大型语言模型（`Large Language Models`, `LLMs`）功能的基石，然而，由于`Transformer`架构的复杂性，其理论基础仍然难以捉摸。特别是，大多数现有研究仅在特定数据模型下理论性地解释了注意力机制如何促进`ICL`，但尚不清楚`Transformer`的其他构建模块如何贡献于`ICL`。为了解决这一问题，我们研究了一个双注意力层`Transformer`在`n-gram`马尔可夫链数据上训练以执行`ICL`的过程，其中马尔可夫链中的每个令牌在统计上依赖于前`n`个令牌。我们分析了一个复杂的`Transformer`模型，该模型具有相对位置嵌入、多头软最大注意力机制以及带有归一化的前馈层。我们证明了，在交叉熵`ICL`损失下的梯度流将收敛到一个极限模型，该模型执行一种归纳头机制的广义版本，并通过所有构建模块的协调贡献生成一个学习到的特征。在极限模型中，第一注意力层充当复制器，将给定窗口内的过去令牌复制到每个位置，带有归一化的前馈网络充当选择器，仅通过查看窗口内的信息相关父节点生成特征向量。最后，第二注意力层作为分类器，将这些特征与输出位置的特征进行比较。

![](img/2d97a2b332c5e0d1ea212fb963e8da8f.png)

![](img/6887d0d4266cacb40fc5a97a5dd992c7.png)

## Eureka：评估与理解大型基础模型

贡献人：

链接：https://arxiv.org/abs/2409.10566

严格和可重复的评估对于评估人工智能的最先进水平和指导科学进步至关重要。然而，实际中的评估由于多种原因而具有挑战性，包括基准饱和、用于测量的方法缺乏透明性、在为生成任务提取测量指标方面的开发挑战，以及更普遍的，需要广泛能力以实现模型间全面比较等。我们提出了三项贡献，以缓解上述挑战。首先，我们提出了`Eureka`，一个开源框架，用于标准化大型基础模型的评估，超越单一分数报告和排名。其次，我们引入了`Eureka-Bench`，作为一个可扩展的基准集合，用于测试以下能力：(i) 对最先进模型仍然具有挑战性，以及(ii) 代表基本但被忽视的语言和多模态能力。非饱和基准中的固有改进空间使我们能够在能力层面上发现模型之间的有意义差异。第三，利用`Eureka`，我们对12个最先进的模型进行了分析，提供了对失败理解和模型比较的深入见解，这些见解可以用于规划有针对性的改进。与近期报告和排行榜显示绝对排名并声称某一模型为最佳的趋势不同，我们的分析表明不存在这样的最佳模型。不同模型具有不同的优势，但在某些能力方面，有些模型比其他模型更常被视为最佳表现者。尽管最近有所改进，当前模型仍在若干基本能力上存在困难，包括详细的图像理解、在可用时从多模态输入中获益而不是完全依赖语言、信息检索的事实性和基础性，以及过度拒绝。

![](img/c8af602aa89388264a2d19502b21d6cd.png)

![](img/15b7340fbd22c764ff8769cfb7fa22cf.png)

# 产品

## 字节跳动推出 Seed-Music

贡献人：

https://team.doubao.com/en/special/seed-music

`Seed-Music` 是一套高质量音乐生成系统，具有精细的风格控制能力。该框架结合了自回归语言建模和扩散方法，支持两种主要的音乐创作工作流程：受控音乐生成和后期制作编辑。受控生成方面，系统允许通过多模态输入（如风格描述、音频参考、乐谱和语音提示）生成声乐作品；而在后期制作方面，则提供了直接编辑生成音频中的歌词和声乐旋律的互动工具。

![](img/d10730e324021c156923553d760a710d.png)

[Seed-Music视频0914.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IpEgb3qiWoV2u0xCNU8cjnHXnFg)

## Supademo 3.0

贡献人：

https://app.supademo.com/

`Supademo` 是一个 AI 驱动的互动演示工具，允许用户快速创建精美的产品演示，支持无登录使用。新版本 `3.0` 引入了 Mac 桌面录制、应用内导览、动态变量和可追踪链接等功能，广泛应用于市场营销、销售和用户培训，帮助用户实现高效产品故事讲述。

![](img/468ad857eb9eb26b4a8affe5f15bbcef.png)

# HuggingFace&Github

## AudioBERT

贡献人：

https://github.com/HJ-Ok/AudioBERT

`AudioBERT` 是一种通过注入音频知识提升语言模型在音频任务表现的模型。它主要功能包括音频知识检测器，用于识别文本中的音频相关关键词；`CLAP` 检索，基于识别结果获取相关音频样本并嵌入模型；以及动态注入音频嵌入的 `AudioBERT` （`LoRA`），保障其他语言任务的性能。其技术栈基于 `BERT-base` 和 `AST` 编码器，并使用 `AdamW` 优化器进行训练。

![](img/246c5fda704746cefcf31ccdd2487b5f.png)

## Datagemma-rig-27b-it

贡献人：

https://huggingface.co/google/datagemma-rig-27b-it

`DataGemma` 是一个经过微调的 `Gemma 2` 模型系列，可以帮助大型语言模型访问并整合来自 `Data Commons` 的可靠公共统计数据。它通过检索交替生成的方法，自动为统计数据添加自然语言查询。用户可以通过特定代码运行该模型，获取有关特定主题（如性别、年龄、种族等）的统计趋势，并在输出中包含数据查询的注释。

# 投融资

## Workday收购AI文档平台Evisort，专注投融资信息

https://techcrunch.com/2024/09/17/workday-acquires-ai-powered-document-platform-evisort/

Workday于周二宣布收购了AI驱动的合同管理平台Evisort，交易金额未披露。此次收购将使Workday能够在其现有的财务和人力资源软件中增加一系列AI文档处理工具，提升客户对关键数据的价值利用。Evisort成立于2016年，由哈佛法学院和麻省理工学院的研究人员创立。该公司提供AI模块，帮助客户分析合同、资产协议和供应商发票等文件，并提供修改建议和关键元素的标注。

投融资信息方面，在被收购之前，Evisort已经从包括General Atlantic、TCV、Vertex Ventures和微软M12在内的投资者筹集了1.556亿美元的资金。Workday近几年加大了对AI的投资力度。2023年，Workday宣布将其Workday Ventures风投基金扩大2.5亿美元，以支持AI、机器学习和工作流程自动化的创业公司。此次收购预计将在Workday 2025财年第三季度完成，成为其今年继收购HR平台HiredScore后的第二笔收购。

被收购公司：https://www.evisort.com/

![](img/e704be648caac25f0c419bc2f6ff1c9d.png)

# 学习

## Accelerating Generative AI with PyTorch II: GPT, Fast

贡献人：

https://zhuanlan.zhihu.com/p/720215891?utm_psn=1819688146423648256
