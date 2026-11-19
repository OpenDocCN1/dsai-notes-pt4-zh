
# 奇绩大模型日报（7月 11日）

> 来源：[https://miracleplus.feishu.cn/docx/N2vpdjToeoVyIjxkzWJcuNR6nKg](https://miracleplus.feishu.cn/docx/N2vpdjToeoVyIjxkzWJcuNR6nKg)

## 推特

### OpenAI和洛斯阿拉莫斯国家实验室宣布生物科学研究合作伙伴关系

https://x.com/OpenAI/status/1811048047704690859

OpenAI和洛斯阿拉莫斯国家实验室正在开发评估，以了解多模态AI模型如何能够在实验室环境中被科学家安全使用。

![](img/b98f2674f1f457b972cd5047a42e353b.png)

### 机器学习LeetCode上线！第一个挑战：在不使用任何张量操作的情况下计算矩阵-向量积

https://x.com/jxmnop/status/1811120776789250328

终于有人为机器学习做了LeetCode，它完全符合我们的期望

刚刚解决了第一个练习：在不使用任何张量操作的情况下计算矩阵-向量积（仅允许使用Python列表）

http://deep-ml.com

![](img/f40840bba9314a408e6326084c3cd1bf.png)

### Karpathy分享优质项目：一个值得信赖的、免费的、支持Linux的、自主托管的64位RISC-V计算机

https://x.com/karpathy/status/1811097021539045582

一个让我早些时候感到惊叹并且我至今仍时常思考的项目：

一个值得信赖的、免费的（自由的）、支持Linux的、自主托管的64位RISC-V计算机

https://contrib.andrew.cmu.edu/~somlo/BTCP/

这是一个尝试构建一个完全开源的计算机系统的项目，包括软件和硬件。通常，即使你使用的是开源软件，你也会依赖于实际运行的硬件芯片，包括其（通常是不透明的）设计、指令集架构（ISA）等。

由于制造芯片的成本很高，这里的方法是使用FPGA，它可以重新配置以实现任何定制的数字电路。而且它们已经足够强大，可以（显然）容纳整个计算机。

这为整个硬件+软件栈提供了前所未有的灵活性。你可以随意更改或扩展计算机的指令集本身（在这里，RISC-V作为默认选择是一个显然优秀的选择）。或者更改CPU的流水线深度。或者更改内存层次结构，添加/更改缓存级别。添加定制的硬件加速器。当然，也可以任意更改操作系统：定制调度程序、内存管理系统或其他任何组件。

该系统也是自主托管的，因此它完全自包含并且没有外部依赖，可以编译自己的编译器和整个软件环境。

在安全性/隐私/可信性方面，你最终会得到一个完全可审计的系统，硬件和软件都是如此。此外，与ASIC相比，FPGA硬件本身对于攻击者来说要难以攻破得多，因为他们无法提前知道你将如何运行它，如何表示你的数据等。

当然，FPGA的运行速度不如实际的芯片，但你在性能上损失的部分可以在开放性和完全控制权上得到补偿。

总之，这是一个引人入胜的项目，如果计算可能在根本层面上发生变化，它可能会变得非常相关。

![](img/10cd3cce564bc19d4c3ca20593fafc6d.png)

![](img/31e762ef2653a658d3e71c58ef67c683.png)

### 《提示压缩和查询优化》：优化RAG的成本和性能

https://x.com/AndrewYNg/status/1811065347841348052

学习如何在我们与@MongoDB合作并由@richmondalake教授的新短期课程《提示压缩和查询优化》中优化RAG的成本和性能。

本课程教你如何结合传统数据库功能与MongoDB用于RAG的向量搜索。你将学习以下技术：

+ 向量搜索：用于用户查询的语义匹配
+ 使用元数据进行过滤：预过滤和后过滤以缩小搜索结果范围
+ 投影：选择仅必要的字段以最小化返回的数据
+ 提升：重新排序结果以提高相关性
+ 提示压缩：使用小型LLM压缩上下文，显著减少令牌数量和处理成本

这些方法解决了大规模RAG应用中的扩展、性能和安全性挑战。

你可以在这里注册：https://deeplearning.ai/short-courses/prompt-compression-and-query-optimization/

![](img/e864c2a080f08f2f728d049a37d1b1b9.png)

[ssstwitter.com_1720701457951.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JqlLbfX58ogKV2xXnqDcUwSKnXd)

### Shawn Wang分享：完整的高清编辑版OpenAI主题演讲

https://x.com/swyx/status/1811106907815563503

完整的高清编辑版@openai主题演讲，由不可替代的@romainhuet，现在已经在YouTube上线了！

在这里特别感谢他：我还没机会好好感谢他拯救了我们——在几次日程变动后，我们急需一个好的演示主题演讲。我只是让Romain重新演示他以前做过的Sora + Voice Engine演示，但他不仅仅是重做，而是超越了我们的预期，给我们带来了全新的GPT4o语音+ ChatGPT桌面版公开演示。

你在舞台上看不到的是他为了这个演示花费了大量时间和我们一起准备和排练，考虑到语音演示的独特限制——其主要新颖特性是在非常大的舞台上实时被打断，同时自身的语音被非常大声地播放——因为他在乎与AI工程师的交流。他当然不必做这些；我们中99%的人无论如何都是从OpenAI开始我们的AI工程师之旅的（其中50%是在ChatGPT之前！）

感谢@eugeneyan和@youngphlo_提供的紧张准备阶段的照片。

然后是在非常充实的一天结束时进行高调的临时演讲，并且在这里看到他与@SarahChieng进行多次采访拍摄。

对AI工程师的绝对奉献和热情。很荣幸他能加入我们参加WF 2024。

![](img/dc8d60e6ebb7fe46cb3edd6e74e6996e.png)

[ssstwitter.com_1720701576633.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/V0lmbEoi7oWbddxyCWhcbzGEnjh)

### Anthropic支持在Amazon Bedrock中微调Claude 3 Haiku

https://x.com/AnthropicAI/status/1811084348692517323

你现在可以在Amazon Bedrock中微调Claude 3 Haiku——我们最快且最具成本效益的模型。

---

你现在可以微调Claude模型了！

目前只能在Amazon Bedrock上微调Claude 3 Haiku，但我们正在努力将其扩展到更多模型和服务。

![](img/edaed0549615596730d4650073b19421.png)

## 论文

### PaliGemma：用于迁移的多功能3B VLM

贡献人：

链接：http://arxiv.org/abs/2407.07726v1

![](img/5cf5025b0a9b2b3920c3689c8b1e04d5.png)

PaliGemma是一个基于SigLIP-So400m视觉编码器和Gemma-2B语言模型的开放式视觉语言模型（VLM）。它被训练成一个多功能且广泛知识的基础模型，非常适合传输。它在各种开放世界任务中表现出色。我们在近40个不同任务上评估了PaliGemma，包括标准VLM基准测试，以及更专门的任务，如遥感和分割。

### 生成图像作为动作模型

贡献人：

链接：http://arxiv.org/abs/2407.07875v1

摘要：图像生成扩散模型已经进行了微调，以解锁诸如图像编辑和新颖视角合成等新能力。我们能否类似地为图像生成模型解锁视觉-运动控制？我们提出了GENIMA，一个行为克隆智能体，通过微调稳定扩散在RGB图像上“绘制联合动作”作为目标。这些图像被输入到一个控制器中，将视觉目标映射到一系列联合位置。我们在25个RLBench和9个现实世界的操纵任务中研究了GENIMA。我们发现，通过将动作转移到图像空间，互联网预训练的扩散模型可以生成优于最先进的视觉-运动方法的策略，特别是在对场景扰动的鲁棒性和泛化到新对象方面。尽管我们缺少深度、关键点或运动计划器等先验知识，我们的方法也与3D智能体竞争力强。

![](img/99926cbbca0f7896fe293ac5aaa1b11d.png)

### LLaVA-NeXT-Interleave：在大型多模态模型中处理多图像、视频和3D

贡献人：

链接：http://arxiv.org/abs/2407.07895v1

摘要：视觉指导微调在增强大型多模态模型（LMMs）的能力方面取得了显著进展。然而，现有的开放式LMMs主要关注单幅图像任务，对于多图像情境的应用仍未深入探讨。此外，先前的LMM研究分别解决不同情境，使得难以将新涌现的能力泛化到跨情境中。因此，我们引入了LLaVA-NeXT-Interleave，同时解决LMM中的多图像、多帧（视频）、多视角（3D）和多块（单幅图像）情境。为了实现这些能力，我们将交错格式的数据视为通用模板，并编制了包含1,177.6k样本的M4-Instruct数据集，涵盖了4个主要领域、14个任务和41个数据集。我们还精心策划了LLaVA-Interleave Bench，全面评估LMM的多图像性能。通过广泛实验，LLaVA-NeXT-Interleave在多图像、视频和3D基准测试中取得了领先成绩，同时保持了单幅图像任务的性能。此外，我们的模型还展现了若干新兴能力，例如在不同设置和模态之间传递任务。代码可在https://github.com/LLaVA-VL/LLaVA-NeXT找到。

![](img/a5afb59a39c2270bb429910c0451f0aa.png)

### 桶预训练就足够

贡献人：

链接：http://arxiv.org/abs/2407.07495v1

大语言模型（LLMs）在各种自然语言处理任务中表现出色。然而，传统的固定长度的数据组成策略，涉及连接和分割文档，可能引入噪音并限制模型捕获长距离依赖性的能力。为了解决这个问题，我们首先引入了三个评估数据组成质量的指标：填充比、截断比和连接比。我们进一步提出了一种多桶数据组成方法，超越了固定长度的范例，提供了一种更灵活有效的预训练方法。大量实验证明，我们提出的方法显著提高了LLMs预训练的效率和效果。我们的方法不仅减少了噪音并保存了上下文，还加速了训练，是LLMs预训练的一个有希望的解决方案。

![](img/caef763a8c5c8a1d8449850e1695b4c3.png)

### 属性或放弃：大语言模型作为长文档助手

贡献人：

链接：http://arxiv.org/abs/2407.07799v1

摘要：LLM可以帮助人类处理长文档，但容易出现幻觉。归因可以增加对LLM响应的信任：LLM提供支持其响应的证据，增强可验证性。现有的归因方法仅在RAG设置下进行评估，初始检索会使LLM的表现混淆。这与长文档设置有根本区别，后者不需要检索，但可能有所帮助。因此，长文档特定归因的评估缺失。为填补这一空白，我们提出LAB，一个包含6个不同长文档任务的归因基准，并尝试在4个不同大小的LLM上使用不同的归因方法，包括提示和微调。我们发现引文，即一步生成响应和提取证据的方法，大部分效果最佳。我们调查了归因是否存在“中间迷失”现象，但未发现。我们还发现证据质量可以预测简单响应数据集上的响应质量，但对于复杂响应数据集，模型很难为复杂主张提供证据。我们发布代码和数据以供进一步研究。

![](img/c75969d61da86f026d2525954de1068f.png)

## 产品

### Odyssey——即将发布

贡献人：

https://odyssey.systems/

Odyssey 公司正在开发一种"Hollywood 级别的视觉人工智能"技术，为专业讲故事者提供强大的创作工具。这种技术采用多模型的方式，可以生成和精确控制场景、角色、光照和动作等各种视觉元素，使创作者能够完全掌控创作过程。Odyssey 的目标是让人类讲故事者重拾主导地位，而不是被 AI 取代。该公司由一支经验丰富的团队开发，已得到知名投资方的支持。

![](img/3fd0d69a1d0cda560947a661afa37edf.png)

### PIECES

贡献人：

https://pieces.app/

Pieces Copilot+是一款实时 AI 助手工具，能够捕捉和处理用户工作流中的各种上下文信息，提供相关的总结、建议和资源，帮助开发者提高效率，无缝连接各种开发工具，同时注重数据隐私和安全性。

![](img/2da5cbbda42cad61ffadbd740ce69e31.png)

## HuggingFace&Github

### WebCanvas

贡献人：@Jack Jin

https://github.com/iMeanAI/WebCanvas

WebCanvas 是一个用于开发和评估网络代理的平台。它提供了一个通用的代理框架,包括规划、观察、记忆和奖励等关键模块，可以在真实的在线网络环境中有效地执行复杂任务。该项目还提供了一套工具包，用于扩展和维护网络代理数据，支持在动态的网络环境中进行评估。

项目网站：https://www.imean.ai/web-canvas

数据标注和协同编辑工具：https://builder.imean.ai/

论文链接：arxiv.org/abs/2406.12373

数据集链接：https://huggingface.co/datasets/iMeanAI/Mind2Web-Live

![](img/c7a8285cc3358383abf51d45579cdead.png)

### Shire

贡献人：

https://github.com/phodal/shire

Shire 提供了一种简单的 AI 编码代理语言，支持自动编程 LLM 和控制 IDE 之间的通信。Shire 的概念起源于 AutoDev，它是 UnitMesh 的一个子项目。在 AutoDev 中，团队为开发人员设计了一个 AI 驱动的 IDE，其中包括 DevIns，即 Shire 的前身。

![](img/de4df9629221309804dd70e7acadf046.png)

![](img/1cd65c6673688085b0eb65c100e2dce9.png)

### NuminaMath-7B-TIR

贡献人：

https://huggingface.co/AI-MO/NuminaMath-7B-TIR

NuminaMath 7B TIR 是一个 7B 参数的数学语言模型，通过两个阶段的监督微调训练而成。它在"AI 数学奥林匹克"比赛中取得了优异成绩，获得了第一个进步奖。该模型擅长解决竞争级数学问题，但在更复杂的几何问题和高难度数学题上表现较弱。它主要针对数学问题领域，不适用于一般聊天应用。在训练过程中，模型采用了多 GPU 分布式训练、Adam 优化器和余弦学习率调度等技术。

![](img/8730e458796801595fca3c70321de5de.png)

## 投融资

### Skild AI 完成 3 亿美元 A 轮融资

贡献人：

https://www.wtae.com/article/pittsburgh-skild-ai-funding/61549600

Pittsburgh 的人工智能和机器人公司 Skild AI 宣布完成了 3 亿美元的 A 轮融资，此次融资包括亚马逊 CEO 杰夫·贝佐斯的投资。该公司专注于构建通用机器人模型，旨在适应各种类型的机器人。Skild AI 表示，这笔资金将用于扩展公司的模型和训练数据集，以便未来商业部署。此次融资使 Skild AI 的估值达到 15 亿美元。参与投资的还有软银集团和卡内基梅隆大学等。

公司官网：https://www.skild.ai/

![](img/db858d6b31975078900ef1cc8c3c06af.png)

### Beeble AI获475万美元融资推出虚拟制作平台

贡献人：

https://techcrunch.com/2024/07/10/beeble-ai-raises-4-75m-to-launch-a-virtual-production-platform-for-indie-filmmakers/

Beeble AI, 一家韩国VFX初创公司，成功筹集了475万美元的种子资金，由Basis Set Ventures领投，Fika Ventures参与，估值2500万美元。该公司计划利用这笔资金提升其基础AI模型，进一步开发产品，并扩充员工团队。目前，Beeble AI团队由七名成员组成，目标是为独立电影制作人和内容创作者提供经济实惠的虚拟制作解决方案。

公司官网：https://www.beeble.ai/

![](img/d1d0095c14ba4062f07174eb13e1a997.png)

### Heymax.ai 宣布完成 260 万美元种子轮融资

贡献人：

https://words.heymax.ai/p/heymaxai-announces-us26m-seed-funding

Heymax.ai 宣布完成 260 万美元（350 万新加坡元）种子轮融资，由 January Capital 领投，Tenity、Ascend Angels、XA Network 等战略投资者参投。Heymax.ai 是由四名前 Meta 工程师于 2023 年创立的个人理财和购物平台，用户可通过平台从 500 多家商户赚取 Max Miles，并将其兑换成任何航班或转移到 25 家航空公司和酒店合作伙伴的积分。Heymax.ai 致力于优化消费者的奖励机制，为用户提供每年一次的免费假期。



公司官网：https://heymax.ai/

![](img/259c0fe4a174aa84f14570fbc5b42d76.png)

# 盘点2024上半年完成融资的64个"Web3+AI”项目，超半数聚焦基础设施和游戏娱乐

贡献人：

https://mp.weixin.qq.com/s/6Sv0Mwr0gbzl5fEXuhg8uw

2024 年上半年，随着人工智能技术的发展，美国股市表现热烈，标普 500 指数创下了新高。AI 芯片制造商 Nvidia 的股价飙升，成为 3 万亿美元市值俱乐部的成员。在这样的市场背景下，科技初创公司几乎都在强调自己的 AI 技术应用，而 Web3 与 AI 的交叉应用被视为一个值得探索的新方向。文章接着提到，PANews 对于在此期间获得融资的 64 个 Web3+AI 项目进行了统计，这些项目在不同程度上应用了 AI 技术，涉及基础设施、数据与隐私、DeFi、游戏娱乐以及内容创作等领域。

在基础设施类项目中，Sentient 等项目致力于构建开源 AI 模型，而 io.net 和 Allora Labs 等则专注于为 AI 提供 GPU 资源和去中心化网络。此外，还有一些项目如 Ritual、Ora 和 AINN Layer2 等，它们在不同层面上推动 AI 技术与区块链的融合。

数据类项目则更多关注隐私安全，如 OpenLedger、FLock 和 Hyperline 等，它们提供了数据基础设施、隐私保护和数据湖等解决方案。DeFi 类项目如 Aperture Finance 和 onaji 等，则尝试通过 AI 技术简化和优化金融操作。

游戏娱乐类项目如 MetaCene 和 Raiinmaker 等，则将 AI 技术应用于游戏和娱乐领域，推动了链游结合 AI 的趋势。应用产品类项目如 MediConCen 和 MyShell 等，关注创作者经济，提供了 AI 驱动的社交平台和内容创作工具。

# 学习

## 生成式AI之父Jürgen Schmidhuber：机器学习编年史与宇宙未来丨智源独家

贡献人：

https://mp.weixin.qq.com/s/Rxju29D0BL-_V5AIALunMA

Jürgen Schmidhuber 在 2024 年智源大会上回顾了他在 1990-91 年期间的研究成果，包括引入了生成对抗神经网络、非归一化线性 Transformer 和自监督预训练技术。这些技术为现代深度学习奠定了基础，特别是 LSTM 和 Transformer 在自然语言处理和机器学习领域得到了广泛应用。Schmidhuber 还探讨了 AI 技术的未来发展，强调了 AI 对宇宙探索的潜在影响，并预测了 AI 发展的关键时刻 —— 欧米伽点，即 AI 将在 2040 年左右达到一个新的高度，这将是人类文明历史上的一个重大转折点。他的研究不仅推动了 AI 技术的进步，也为人类社会的发展和宇宙探索提供了新的视角。

![](img/1ccc381268c182a0494144c8a9d2e6d7.png)

## Do We Really Need Packing in LLM SFT?——理论析

贡献人：

https://zhuanlan.zhihu.com/p/706231162

本文深入探讨了在LLM微调（SFT）过程中，是否需要使用打包（Packing）技术。打包技术主要用于提高训练效率，通过将多个短序列合并成更长的序列，减少显存占用和加速训练。然而，文章指出打包可能导致短文本信息的损失，影响模型对短文本的处理能力。在多轮对话场景中，打包可能会引入错误的注意力机制，影响模型的准确性。此外，打包策略可能会影响梯度的准确计算，尤其是在梯度稀疏的情况下。文章建议，在使用打包技术时，应注意选择正确的框架，如 `axolotl`，以确保实现正确，并且在处理多轮对话数据时，应使用特殊标记（如 `EOT Token`）来区分不同轮次的结束。最后，文章强调，打包是否必要应根据具体任务和数据集的特点来决定，以平衡训练效率和模型性能之间的关系。

![](img/780d6705e1d5ab3e668ffd75a80864a1.png)

## Sparse Tensor：内存之蜜糖，XPU之砒霜？

贡献人：

https://zhuanlan.zhihu.com/p/667959507

稀疏张量是一种高效的数据存储格式，专门用于存储大量零元素的张量数据。它通过只保存非零元素及其索引来减少内存占用，特别适合于深度学习模型中的稀疏数据处理。然而，稀疏张量在计算上面临随机访问内存的延迟问题，尤其是在非 `element-wise` 计算和分布式并行计算中，其动态 `shape` 特性给现有的计算框架带来了挑战。为了克服这些难题，`ExTensor` 等专用硬件加速器被设计出来，它们通过优化的数据布局和协同遍历算法，显著提升了稀疏张量计算的性能。尽管如此，稀疏张量在分布式计算中的数据拆分与合并问题仍然未解，需要进一步的研究和技术创新。

![](img/de41e4361957ef27501639d3351f4691.png)

## 图解序列并行云台28将（下篇）

贡献人：

https://zhuanlan.zhihu.com/p/707499928

本文深入探讨了序列并行云台 28 将的技术，主要分析了 `TeraPipe`、`Seq1F1B` 和 `HPipe` 等序列流水并行方法。`TeraPipe` 通过动态规划优化负载均衡，`Seq1F1B` 支持多种策略如 `1F1B`、`1F1B-I`、`1F1B-ZERO-Bubble`，以及 batch 和序列维度的切分。`HPipe` 则针对异构设备进行了优化。这些技术在推理场景中尤其重要，能够有效地提高大模型的处理效率。文章还提到了 `RingAttention` 和 `Ulysess` 等本土技术，它们在不同场景下展现出色的性能和负载均衡能力。混合序列并行 USP 在长序列处理方面表现出优势，可与其他并行模式如 TP 或 FSDP 结合。`AllGatherKV` 虽然在特定场景有应用，但由于关注度和开源实现的缺乏，尚未被广泛采用。总体来看，序列并行技术在提升大模型训练和推理效率方面具有重要意义。

![](img/e41572bdb0d69bc02f18e596d4f5e4e3.png)

## 一窥廉价LLM时代的Agent策略框架设计

贡献人：

https://zhuanlan.zhihu.com/p/708143331

文章深入探讨了在廉价大型语言模型（LLM）时代如何通过策略框架设计来优化 Agent 的性能。作者指出，尽管 LLM 技术不断进步，但单次调用的能力提升有限，且 workflow 的构建和调优成本较高。为了降低成本并提高效率，提出了减少 prompt 数量并提升每个 prompt 的智力工作量的策略。以 `GraphRAG` 为例，通过内部 workflow 构建知识图谱，改善了文本检索增强生成（RAG）的过程，并且在中间环节的结果质量上做出了妥协，以此降低整体成本。文章强调，在廉价 LLM 时代，策略框架设计方法论的重要性，并呼吁进一步的实践和探索。

## 从零开始-深入理解存储引擎

贡献人：

https://zhuanlan.zhihu.com/p/707710109

文章详细探讨了存储引擎的原理与实践，从单机到分布式系统的设计。首先，介绍了如何通过增加内存索引和文件分段压缩来提高单机存储引擎的性能。然后，详细阐述了 `SSTable` 的构建与格式，以及 `LSM Tree` 的工作原理，并与 `B + 树` 进行了对比，指出了它们各自适用的场景。接着，文章讨论了数据复制的技术，包括主从复制、多主复制和无主复制，并指出了如何处理复制滞后和冲突问题。最后，针对数据分片，介绍了分片策略、再平衡和请求路由的方法，确保分布式数据库的高可用性和扩展性。

![](img/570d787aa1aeecce590711fd8320c32c.png)

## GPU并行通讯方式科普

贡献人：

https://zhuanlan.zhihu.com/p/707699141?utm_psn=1794633611699433472

GPU并行通信方式包括多对1或1对多通信（如 `BroadCast`、`Scatter`、`Gather`、`Reduce`）和多对多通信（如 `All Reduce`、`Ring All Reduce`）。多对1或1对多通信涉及数据的广播、分发、收集和汇总操作，优化时间复杂度。多对多通信则包括 `All Reduce` 和 `Ring All Reduce`，通过减少节点负担和异步操作提高效率。`GPU Direct` 通信技术如 `GPUDirect Storage`、`RDMA`、`Video` 和 `P2P`，通过直接内存访问和数据传输，提高数据传输速度，减少系统瓶颈。常用载体包括 `NVLink`、`NVSwitch`、`InfiniBand`、`RoCE`、`iWARP` 和 `IPOIB`。

相关链接：https://linjixin.notion.site/LLM-ebf2e9065f7c4602808c349197c24224

![](img/916687fb5c2bfefbce8f12ebda446e7c.png)

## 华泰 | 科技年度报告：墨西哥篇 - 有望受益于全球产业链重构

贡献人：

https://mp.weixin.qq.com/s/84JMG_U3gcTHsE6iJQfLwQ

墨西哥作为全球供应链重组的关键受益者，其在汽车电子和 AI 产业的战略地位被进一步巩固。在汽车电子领域，墨西哥不仅是世界第七大乘用车生产国，还拥有成熟的汽车零部件制造业，尤其是在瓜达拉哈拉市等地的产业集群。在 AI 服务器组装方面，墨西哥华雷斯市正成为全球最大的服务器组装基地，中国台湾的鸿海、纬创等企业在此扩产，这将有助于满足北美市场对 AI 硬件的快速增长需求。此外，特斯拉在墨西哥建设超级工厂，预计将成为全球产能最大的汽车工厂，这一举措将进一步吸引中国汽车电子企业进驻墨西哥，加速墨西哥成为全球产业链重 `STRUCT` 中心的过程。

![](img/89b04d7ff91f029f68e339b965982143.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报知识库】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/9ec1711d0edd16935ea61d8fa157df04.png)
