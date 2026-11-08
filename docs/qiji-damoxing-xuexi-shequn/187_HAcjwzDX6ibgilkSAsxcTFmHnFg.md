# 奇绩大模型日报（6月 4日）

> 来源：[https://miracleplus.feishu.cn/docx/N9VbdfwKVoEWKNxW9qBcVinDn2g](https://miracleplus.feishu.cn/docx/N9VbdfwKVoEWKNxW9qBcVinDn2g)

# 资讯

### 再战Transformer！原作者带队的Mamba 2来了，新架构训练效率大幅提升

https://mp.weixin.qq.com/s/31t6pJqcXrZDjT6XiJZC_g

自 2017 年被提出以来，Transformer 已经成为 AI 大模型的主流架构，一直稳居语言建模方面 C 位。但随着模型规模的扩展和需要处理的序列不断变长，Transformer 的局限性也逐渐凸显。一个很明显的缺陷是：Transformer 模型中自注意力机制的计算量会随着上下文长度的增加呈平方级增长。几个月前，Mamba的出现打破了这一局面，它可以随上下文长度的增加实现线性扩展。随着 Mamba 的发布，这些状态空间模型 (SSM) 在中小型规模上已经实现了与 Transformers 匹敌，甚至超越 Transformers。Mamba 的作者只有两位，一位是卡内基梅隆大学机器学习系助理教授 Albert Gu，另一位是 Together.AI 首席科学家、普林斯顿大学计算机科学助理教授 Tri Dao。Mamba 面世之后的这段时间里，社区反应热烈。可惜的是，Mamba 的论文却惨遭 ICLR 拒稿，让一众研究者颇感意外。仅仅六个月后，原作者带队，更强大的 Mamba 2 正式发布了。

![](img/d1f15e8d85ee591fc5296a38ec00512c.png)

### 多模态模型学会打扑克：表现超越GPT-4v，全新强化学习框架是关键

https://mp.weixin.qq.com/s/bAf-5NzOD3fdTwYzdKsELw

只用强化学习来微调，无需人类反馈，就能让多模态大模型学会做决策！这种方法得到的模型，已经学会了看图玩扑克、算“12点”等任务，表现甚至超越了GPT-4v。这是来自UC伯克利等高校最新提出的微调方法，研究阵容也是相当豪华：

+   图灵奖三巨头之一、Meta首席AI科学家、纽约大学教授LeCun

+   UC伯克利大牛、ALOHA团队成员Sergry Levine

+   ResNeXt()一作、Sora基础技术DiT作者谢赛宁

+   香港大学数据科学学院院长、UC伯克利教授马毅

![](img/0b852225751ada27910ad955f9ad60e1.png)

### Oculus创始人帕尔默·拉奇宣称自己正在研发一款全新的头显设备

https://news.nweon.com/121452

AWE USA 2024大会将于6月18日至6月20日在美国加州洛杉矶举行，而Oculus创始人帕尔默·拉奇日前宣布，他计划在本届活动正式宣布自己在研究的VR头显设备。

![](img/f1a52e35ce73fa020b1156258103be95.png)

### Karpathy点赞，这份报告教你如何用 LLaMa 3创建高质量网络数据集

https://mp.weixin.qq.com/s/luZGMG1RRUT4X_ckt8hsCQ

众所周知，对于 Llama3、GPT-4 或 Mixtral 等高性能大语言模型来说，构建高质量的网络规模数据集是非常重要的。然而，即使是最先进的开源 LLM 的预训练数据集也不公开，人们对其创建过程知之甚少。最近，AI 大牛 Andrej Karpathy推荐了一项名为 FineWeb-Edu 的工作。

![](img/c0541b4085bf1c88a9cee87ff43b956e.png)

### 单个4090可推理，2000亿稀疏大模型「天工MoE」开源

https://mp.weixin.qq.com/s/h5bxuWca65t3LsQwqGq-Og

在大模型浪潮中，训练和部署最先进的密集 LLM 在计算需求和相关成本上带来了巨大挑战，尤其是在数百亿或数千亿参数的规模上。为了应对这些挑战，稀疏模型，如专家混合模型（MoE），已经变得越来越重要。这些模型通过将计算分配给各种专门的子模型或「专家」，提供了一种经济上更可行的替代方案，有可能以极低的资源需求达到甚至超过密集型模型的性能。6 月 3 日，开源大模型领域又传来重要消息：昆仑万维宣布开源 2 千亿稀疏大模型 Skywork-MoE，在保持性能强劲的同时，大幅降低了推理成本。Skywork-MoE 基于此前昆仑万维开源的 Skywork-13B 模型中间 checkpoint 扩展而来，是首个完整将 MoE Upcycling 技术应用并落地的开源千亿 MoE 大模型，也是首个支持用单台 4090 服务器推理的开源千亿 MoE 大模型。让大模型社区更为关注的是，Skywork-MoE 的模型权重、技术报告完全开源，免费商用，无需申请。

![](img/2d2d3a110c73af6f22ca55d931b7e475.png)

### AI训练数据的版权保护:公地的悲剧还是合作的繁荣?

https://mp.weixin.qq.com/s/5D4lpqucMwWQF6VQpSe-iw

GPT-4o内置声音模仿「寡姐」一案闹的沸沸扬扬，虽然以OpenAI发布声明暂停使用疑似寡姐声音的「SKY」的语音、否认曾侵权声音为阶段性结束。但是，一时间「即便是AI，也得保护人类版权」这一话题甚嚣尘上，更刺激起了人们本来就对AI是否可控这一现代迷思的焦虑。近日，普林斯顿大学、哥伦比亚大学、哈佛大学和宾夕法尼亚大学共同推出了一项关于生成式AI版权保护的新方案，题为《An Economic Solution to Copyright Challenges of Generative AI》。

![](img/bf1d50f7ab1b8f4b86d420fc4250527f.png)

### 华尔街盘点全球芯片战！市场规模将达 1 万亿美元

https://mp.weixin.qq.com/s/cfUb4mysQIwO-K3qvaN3aQ

根据《华尔街日报》，随着全球芯片之争日益升级，到本世纪末，芯片行业的规模预计将翻一番，达到 1 万亿美元。各国政府加大力度，推动国内芯片生产，这些芯片为汽车、电子产品和人工智能等各行各业提供动力。全球各地的公司都在竞相加入这股热潮。据芯片行业咨询公司 International Business Strategies预测，到本世纪末，全球半导体收入预计将超过 1 万亿美元。 国内生产能力增强可能会使高度专业化的半导体供应链多样化，在某些地区，半导体供应链在某些工艺领域具有优势，而在其他领域则存在弱点。例如，美国公司在芯片设计的许多领域处于领先地位，而台湾、韩国和中国大陆的公司则在后期的生产和组装阶段占据主导地位。

![](img/7d6e4836e98d7ef7587666ee9833a10e.png)

# 推特

### 用任何声音制作歌曲：哼唱一首曲调，但要赋予它节奏布鲁斯。即将推出 VOL-3，

https://x.com/suno_ai_/status/1797635285368680826

用任何声音制作歌曲。即将推出 VOL-3:哼唱一首曲调,但要赋予它节奏布鲁斯(R&B)的感觉,由 Rebecca (Suno 产品经理)演唱。

![](img/9df4d5ea2498f78fcb5d2cda93bf16e2.png)

[ssstwitter.com_1717427541788.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IQEFbq2EVoCbrmxQBsLcZywGnHe)

### Tri Dao分享状态空间对偶理论框架：证明了许多线性注意力变体和状态空间模型是等价的

https://x.com/tri_dao/status/1797650443218436165

Tri Dao 和 Albert Gu 合作构建了一个丰富的状态空间对偶(State-Space Duality, SSD)理论框架,证明了许多线性注意力(linear attention)变体和状态空间模型(State-Space Models, SSMs)是等价的。由此产生的模型 Mamba-2 比 Mamba-1 更好、更快,并且在语言建模任务上与强大的 Transformer 架构相匹配。

SSD 理论通过与矩阵分解的联系,让我们可以为 SSM 推导出更快的算法,利用矩阵乘法作为基本运算。大多数 GPU 的浮点运算都是矩阵乘法,因此充分利用张量核心(tensor cores)对于提高运算速度至关重要。

SSD 还启发了 Mamba-2 模型的设计,引入了并行投影、头部结构,并绑定参数以创建"多输入 SSM",类似于"多值注意力"或"分组值注意力"。

此外,SSD 使得将许多最初为 Transformer 开发的系统优化更容易应用到 SSM 中,例如:

+   张量并行和序列并行以扩展到更大的模型和更长的序列

+   可变序列长度以加速微调和推理

Mamba-2 的 SSD 层比 Mamba-1 中的关联扫描(associative scan)要快得多,使得增加状态维度和改善模型质量成为可能。在 3B 参数规模下训练 300B tokens 后,Mamba-2 超过了同等规模的 Mamba-1 和旧的 Transformers。

最近的模型如 Jamba 和 Zamba 通过添加一些注意力层来提高模型质量。研究发现,将 4-6 个注意力层与 Mamba-2 层混合,可以超过 Transformer++ 和纯 Mamba-2。这表明注意力机制和 SSM 是互补的。

未来还有许多值得探索的方向,例如:

+   在混合模型中,注意力层如何提高质量?

+   如何在新硬件(如 H100)上优化 SSM 训练?

+   如果不再使用键值缓存(KV cache),推理过程会有什么变化?

总之,这项研究揭示了注意力机制和 SSM 之间的深层联系,为构建更快、更好的语言模型提供了新的思路。

![](img/ad3df46dfa8ff82d65f59e6d6133053c.png)

### Bilawal Sidhu分享：深入解析"将神经辐射场视频烘焙成沉浸式分层深度图像"

https://x.com/bilawalsidhu/status/1797807189400104985

当苹果公司表示他们将推出 "空间媒体" 时,我真正期望的是这样的效果——具有六个自由度的 3D 视频。相反,Vision Pro 用户只得到了装在新瓶子里的旧酒(苹果的 MV-HEVC 格式中的 VR180 视频)。不过很巧的是,这里有一个基于 NeRF 的开源体积视频引擎,可以通过 WebXR 流式传输到 Vision Pro 和 Quest。哦,它也支持 Mac。这一切都来自一家名不见经传的公司 Lifecast(YC '21)。

让我们来深入了解他们的新论文 "将神经辐射场视频烘焙成沉浸式分层深度图像",该论文旨在证明在当今的硬件上,体积视频捕捉、编辑和回放是完全可能的。

中心方法:多视图视频 → NeRFs → LDIs

你也可以使用一个简单的 VR180 相机(例如佳能 R5C)或一个由 GoPro 组成的大球形相机来捕捉场景的 3D 视频,制作一些 NeRF,然后将其烘焙成基于分层深度图像(LDI)的视频格式。这有点像拿一个精心制作的雕塑(NeRF),然后创建一个简化的 3D 打印版本(LDI),它捕捉了原始作品的本质,但更易于分发和展示。根据论文,它是一个 3 层 LDI——这不是超高层数,但与平面立体相比仍然是一个相当大的飞跃。然而,作者坚持只使用 3 层的原因非常实用。

为什么 3 层是体积视频的最佳选择

事实证明,你可以轻松地将 1920x1920 分辨率的 3x3 网格层打包成一个 5760x5760 的图像——这是大多数带有移动芯片的 VR 头显可以解码的最大分辨率视频。此外,论文使用等角(相对于等矩形)投影,这使得中心区域的像素密度与 "8K" 分辨率的 180° VR 视频相当。在我看来,这是一个相当不错的选择。

事情进展很快!然而,旧的想法常常找到新的归宿

我对看到 "LDIs" 和 "等角投影" 被用来烘焙这些 4D 辐射场感到低调兴奋。大约在 2017-2020 年,我在谷歌的团队(@broxtronix 等人)使用完全相同的方法来实现光场视频和 YouTubeVR 上的高分辨率视频流。

4 年后的这篇论文与 Deep View Video 论文相比,速度提高了 238 倍,而且该流水线可以在单个 GPU 工作站上运行,而不是在谷歌数据中心运行,这一点非常疯狂。

结束语

在我看来,对于复杂的主题,视觉保真度还不够——例如,3 层只能让你达到一定程度。但这篇论文/流水线向我证明,在移动硬件上进行高分辨率体积视频流式传输绝对是可能的。像这样一个更简单的 3 层格式也可以使体积视频在现有工具如 Premiere/Resolve 中的编辑比我们预期的要容易得多。

![](img/ef7adc2d8065bdd1947443f9a90a362d.png)

[ssstwitter.com_1717472601980.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZD39bzr0AosrnfxvrGdcYOAYnDf)

### 对话Near Protocol，Opentensor和SaharalabsAI：AI和Web3的交集

https://x.com/PanteraCapital/status/1797657047162622306

与我们一起探索 #AI 和 #Web3 的交集,参与一场由该领域的三位先驱者的对话。

时间:美国东部时间 6 月 4 日中午 12 点

演讲者:

@NEARProtocol | @ilblackdragon

@opentensor | @const_reborn

@SaharaLabsAI | @xiangrenNLP

在此注册:https://panteracapital.com/future-conference-calls/

![](img/a04d56cb94cfbd991ac583439465c7fe.png)

### ElevenLabs分享：将 Jensen Huang 精彩的 Computex 主题演讲的部分片段用 AI 配音成普通话

https://x.com/elevenlabsio/status/1797655062350565433

我们与 @nvidia 合作,通过将 Jensen Huang 精彩的 Computex 主题演讲的部分片段用 AI 配音成普通话,帮助他扩大了演讲的影响力。此外,我们很高兴能够合作,通过将我们高质量的 AI 语音引入 NVIDIA ACE NIM 微服务,为视频游戏创建动态的非玩家角色(NPC)。

阅读更多:https://elevenlabs.io/blog/nvidia-ace-at-computex/

![](img/fd67c5b4bba4694e14d91a11fbfd0ea3.png)

[ssstwitter.com_1717488634262.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QmTubaE7UovBZmxDTDGcYbsPnVh)

### Awni Hannun分享MLX教程：基础知识、神经网络、微调等

https://x.com/awnihannun/status/1797750024639787045

以下是一些指导性的演练,帮助学习 MLX:

+   基础知识,使用 MLX 核心进行线性回归:

https://ml-explore.github.io/mlx/build/html/examples/linear_regression.html

+   神经网络,在 MNIST 上训练多层感知机(MLP):

https://ml-explore.github.io/mlx/build/html/examples/mlp.html

+   大语言模型(LLM)推理:

https://ml-explore.github.io/mlx/build/html/examples/llama-inference.html

+   使用 MLX LM 进行 LoRA 微调:

https://gist.github.com/awni/773e2a12079da40a1cbc566686c84c8f

MLX 教程中还缺少什么?如果你有关于演练、教程或示例的想法,请告诉我。

![](img/ef1df4cc6c9040a00ca50ea13d6edf45.png)

# 论文

### D-CPT法则：面向领域的大语言模型持续预训练缩放定律

贡献人：

链接：http://arxiv.org/abs/2406.01375v1

持续预训练（CPT）在大语言模型（LLMs）上被广泛应用于扩展模型对特定下游领域（如数学和代码）的基本理解。对于特定领域LLMs的CPT，一个重要问题是如何选择通用语料库（例如Dolma，Slim-pajama）和特定领域语料库之间的最佳混合比例。现有方法通常通过在一组混合比例上进行网格搜索来采用费时费力的人力工作，这需要高GPU训练消耗成本。此外，我们无法保证所选比例对特定领域是最佳的。为解决现有方法的局限性，受性能预测的Scaling Law启发，我们提出了调查特定领域持续预训练（D-CPT Law）的Scaling Law来确定不同大小的LLMs的可接受训练成本的最佳混合比例。通过拟合D-CPT Law，我们可以轻松预测任意混合比例、模型大小和数据集大小的通用和下游性能，使用有限实验中的小规模训练成本。此外，我们还将标准的D-CPT Law扩展到跨领域设置，并提出了交叉域D-CPT Law来预测目标领域的D-CPT Law，在目标领域只需要非常小的训练成本（约为正常训练成本的1%）。对六个下游领域的全面实验结果展示了我们提出的D-CPT Law和交叉域D-CPT Law的有效性和泛化性。

![](img/ad3a8b94338e256baadbd99d13acc43e.png)

### MMLU-Pro：更强大且具挑战性的多任务语言理解基准

贡献人：

链接：http://arxiv.org/abs/2406.01574v1

在大规模语言模型时代，类似于Massive Multitask Language Understanding（MMLU）的基准测试在推动AI在各领域语言理解和推理方面的能力所能达到的极限方面起到了关键作用。然而，随着模型性能的持续提升，它们在这些基准测试上的表现已经开始趋于稳定，这使得越来越难以区分模型能力上的差异。本文引入了MMLU-Pro，一个增强的数据集，旨在通过整合更具挑战性、专注于推理的问题，并将选项选择从四个扩展到十个，扩展主要基于知识的MMLU基准测试。此外，MMLU-Pro消除了MMLU中的琐碎和嘈杂问题。我们的实验结果表明，与MMLU相比，MMLU-Pro不仅提高了挑战，导致准确率下降了16%至33%，而且在不同提示下展现出更大的稳定性。在测试了24种不同提示风格的情况下，模型得分对提示变化的敏感度从MMLU中的4-5%降至MMLU-Pro中的仅为2%。此外，我们发现，利用Chain of Thought (CoT)推理的模型在MMLU-Pro上的表现优于直接回答问题，这与原始MMLU上的发现形成鲜明对比，表明MMLU-Pro包含了更复杂的推理问题。我们的评估结果证实，MMLU-Pro是一个更具区分性的基准测试，有助于更好地跟踪该领域的进展。

![](img/768f554308b3a509ca13ce453b1392b9.png)

### **R2C2-Coder：增强和对比代码大语**言模型在真实世界代码库级别的代码补全能力

贡献人：

链接：http://arxiv.org/abs/2406.01359v1

摘要：最近几年，代码补全模型取得了显著进展。最近，存储库级别的代码补全在现代软件开发中引起了更多关注，并提出了几种基线方法和基准。然而，现有的存储库级别代码补全方法通常未能充分利用项目存储库的广泛上下文，如相关文件和类层次结构的复杂性。此外，现有的基准通常集中在有限的代码补全场景，无法很好地反映现有方法的存储库级别代码补全能力。为了解决这些局限性，我们提出了R2C2-Coder来增强和评估代码大语言模型在实际存储库级别上的代码补全能力，其中R2C2-Coder包括代码提示构建方法R2C2-Enhance和精心设计的基准R2C2-Bench。具体来说，在R2C2-Enhance中，我们首先构建候选检索池，然后通过从检索池中检索来为每个补全光标位置组装完成提示。其次，基于R2C2-Enhance，我们可以构建一个更具挑战性和多样性的R2C2-Bench，并提出上下文扰动策略来很好地模拟现实世界的存储库级别代码补全。对多个基准的广泛结果证明了我们的R2C2-Coder的有效性。

![](img/3314e5d572f2cf0a72a97868563f41b9.png)

### LongSkywork: 一个高效拓展大语言模型上下文长度的训练方法

贡献人：

链接：http://arxiv.org/abs/2406.00605v1

我们引入了LongSkywork，一个能够处理高达200,000个token的长文本大语言模型（LLM）。我们提供了一个培训配方，可以有效地扩展LLM的上下文长度。我们确定增强长上下文处理能力的关键元素是在标准的SFT阶段后加入一个长上下文SFT阶段。仅仅200次迭代就能将标准SFT模型转换为长上下文模型。为了减少为长上下文语言建模收集和注释数据的工作量，我们开发了两种新方法来创建合成数据。这些方法应用在持续预训练阶段以及监督微调（SFT）阶段，极大地提高了我们长上下文LLM的训练效率。我们的研究发现，合成的长上下文SFT数据在一定程度上可以超越人类策划的数据性能。LongSkywork在各种长上下文基准测试中表现出色。在Needle测试中，一个用于长上下文信息检索的基准测试中，我们的模型在多个上下文范围上实现了完美的准确性。此外，在实际应用场景中，LongSkywork-13B表现与领先的长上下文模型Claude2.1相当，突出了我们提出的方法的有效性。

![](img/a1a542e28ab49e8f105b8324438c3309.png)

### **通用上下文提示全循环模型的逼近**

贡献人：

链接：http://arxiv.org/abs/2406.01424v1

零样例学习和上下文学习使得在没有模型微调的情况下解决任务成为可能，这对于发展生成模型解决方案至关重要。因此，理解预训练模型是否可以被提示以逼近任何函数是至关重要的，即它是否是一个通用的上下文逼近器。我们展示了RNNs、LSTMs、GRUs、线性RNNs以及线性门控结构（如Mamba和Hawk/Griffin）也可以作为通用的上下文逼近器。我们介绍了一种名为LSRL的编程语言，它可以编译到这些完全循环的架构中。LSRL对于进一步研究完全循环模型可能具有独立的兴趣。我们还研究了乘法门控的作用，并观察到包含这种门控的架构（如LSTMs、GRUs、Hawk/Griffin）可以更加稳定地实现某些操作，使它们成为实际上下文通用逼近的更有竞争力的选择。

![](img/b7ef2a56f112bf4f9add82794ea6b65d.png)

### **如何理解整个软件库？**

贡献人：

链接：http://arxiv.org/abs/2406.01422v1

最近，基于大语言模型（LLM）的智能体推动了自动软件工程（ASE）的重要发展。尽管已经验证了有效性，现有方法的设计主要集中在代码的局部信息，例如问题、类和函数，导致捕捉软件系统中的全局背景和相互依赖性存在局限性。根据人类软件工程开发者的实践经验，我们认为对整个存储库的全面理解将是ASE的关键路径。然而，理解整个存储库会带来各种挑战，例如极长的代码输入、杂乱的代码信息、复杂的依赖关系等。为此，我们开发了一种名为RepoUnderstander的新型ASE方法，通过指导智能体全面理解整个存储库。具体来说，我们首先将整个存储库的关键信息压缩成存储库知识图，以自顶向下的方式减少存储库的复杂性。随后，我们通过提出基于蒙特卡洛树搜索的存储库探索策略，赋予了智能体全面理解整个存储库的能力。此外，为了更好地利用存储库级别的知识，我们指导智能体进行总结、分析和规划。然后，他们可以操纵工具动态获取信息并生成补丁以解决真实世界中的GitHub问题。大量实验证明了提出的RepoUnderstander的优越性和有效性。与SWE-agent相比，在SWE-bench Lite基准上取得了18.5%的相对改进。

![](img/497fafbe2d10a6a23da8af92678b8538.png)

### **用扩散模型解读甲骨文语言**

贡献人：

链接：http://arxiv.org/abs/2406.00684v1

自大约3000年前中国的商代起源，甲骨文是语言史上的重要里程碑之一，早于许多已建立的书写系统。尽管已发现成千上万的铭文，但仍有大量的甲骨文尚未破译，给这种古老语言笼罩了一层神秘的面纱。现代人工智能技术的出现为甲骨文破译提供了新的前沿，挑战了传统依赖大量文本语料库的自然语言处理方法，这是历史语言所不具备的奢侈条件。本文通过采用图像生成技术，特别是通过开发甲骨文解密器（OBSD）的做法，介绍了一种新颖的方法。利用条件扩散策略，OBSD为破译提供了重要线索，开辟了AI辅助分析古代语言的新途径。为验证其有效性，对甲骨文数据集进行了大量实验，定量结果证明了OBSD的有效性。代码和破译结果将在https://github.com/guanhaisu/OBSD提供。

![](img/3066527bf6b072e2969043ca05553036.png)

# 产品

### **photes.io**

贡献人：

https://photes.io/

Photes.io 是一款基于 AI 技术的应用程序，能够将各种形式的照片(如课堂笔记、会议记录、白板截图等)快速转换为可编辑、组织有序的数字笔记。它不仅提供标签分类、跨设备同步等便捷功能，还注重数据安全和隐私保护，并支持自定义模板，为用户带来更个性化的数字笔记体验。

![](img/191a1fec010e95e34c507e2cb0b189df.png)

### Linkz.AI

贡献人：

https://linkz.ai/

Linkz.ai 是一个为网站和博客提供实时链接预览功能的工具。它可以在访客悬停在链接上时弹出预览框，展示链接内容的摘要，并直接在网页上显示视频、文章等内容，无需访客跳转离开网站。Linkz.ai 还提供链接预览的分析数据，以及可自定义的主题样式。通过这些功能，帮助网站提高访客的停留时间和交互，提升网站的整体用户体验。

![](img/07604d6f249155b1b2ab11206f723442.png)

# HuggingFace&Github

### rejax

贡献人：@Jack Jin

https://github.com/keraJLi/rejax

rejax 是一个基于 Jax 的强化学习算法库，它专注于提高训练过程的速度和可扩展性。它允许用户利用 Jax 的各种功能，如 jit、vmap 和 pmap，来加速整个训练流程。它实现了多种流行的强化学习算法，如 PPO、SAC 和 DQN 等，并具有可修改性，使用户可轻松扩展或定制算法。此外，它还提供了灵活的回调机制，方便用户进行日志记录和监控。

![](img/3b950d6364dfe3ee02ddf6c77c80d081.png)

### **Animate Anyone**

贡献人：

https://github.com/novitalabs/AnimateAnyone

Animate Anyone 是一个非官方的视频生成模型，可以根据输入的图像或视频生成动画人物。这个开源项目提供了预训练权重和推理代码，受到MooreThreads/Moore-AnimateAnyone 项目的启发，做了一些改进。项目提供了快速开始指南，包括环境搭建、权重下载和推理运行等步骤。同时还支持将视频转换为关键点序列，并提供在线演示平台。

![](img/8b95ccedd98ff256ef1e2792365761ad.png)

### CLIPPyX

贡献人：

https://github.com/0ssamaak0/CLIPPyX

CLIPPyX 是一个强大的系统范围内的图像搜索和管理工具，集内容、文本和视觉相似性搜索于一体，无论图像位置或文件名，都可以轻松定位所需图像;它利用 CLIP、OCR 和文本嵌入模型实现语义层面的图像检索，并提供 CLIPPyX 服务器接收查询、返回结果，支持高效的替代模型，是一款功能丰富、易用的图像管理神器。

![](img/15a16318a7af360d7fe32a4cc30db937.png)

# 投融资

### 11thEstate 获得200万美元种子轮融资

贡献人：

https://www.thesaasnews.com/news/11thestate-raises-2-million-in-seed-round

11thEstate是一家总部位于纽约的平台，专注于帮助投资者在证券欺诈案件中获得赔偿。公司最近完成了一轮200万美元的种子融资。本轮融资由Social Leverage领投，其他参与者包括Outside Ventures、Blank Ventures以及天使投资人Evan Rapoport和Chris Camillo。Social Leverage专注于软件、消费品和金融科技领域的早期和种子期投资。随着此次投资，Social Leverage的Matt Ober将加入11thEstate的董事会。11thEstate计划利用这笔资金扩大运营规模，并在投资回收领域建立新的标准。

公司官网：https://11thestate.com/

![](img/cf9b37eaa533e71cb28dc1759ee3e971.png)

### Caju AI 完成300万美元种子轮融资

贡献人：

https://www.thesaasnews.com/news/caju-ai-closes-3-million-in-seed-round

Caju AI是一家位于弗吉尼亚州夏洛茨维尔的公司，提供生成式AI驱动的客户互动解决方案。公司近日完成了300万美元的种子轮融资。本轮融资由Grotech Ventures、Felton Group及夏洛茨维尔对冲基金经理Jaffray Woodriff的家族办公室领投，另有天使投资人参与。Caju AI计划利用这笔资金进一步推动公司的增长和创新。公司专注于生成式AI客户互动解决方案，能够安全捕获并分析客户在各种消息渠道中的交流，提供高级分析和可操作的商业智能。

公司官网：https://www.caju.ai/

![](img/fc7e666fe9768ce5e137f2778a0e29a9.png)

### Headout 收购 Dabble 打造AI体验

贡献人：

https://www.phocuswire.com/tour-activities-platform-headout-acquires-dabble-ai-experiences

旅游活动平台Headout收购了Dabble，这是一家由Y Combinator支持的初创公司，致力于将数字世界与物理世界连接。Headout计划利用Dabble的技术能力，为用户打造新一代的AI驱动旅游体验。Dabble专注于计算机视觉、人工智能和空间计算，在游戏、室内导航和培训等领域开发应用。此次收购的条款未披露。Headout成立于2015年，提供全球125个城市的旅游体验，并已筹集超过6000万美元资金。

公司官网：https://www.dabble.so/

![](img/ecbef3fb62585aa0846d0dad5a760ae3.png)

### Lear通过战略收购WiP工业自动化增强自动化和人工智能能力

贡献人：

https://www.lear.com/newsroom/lear-to-enhance-automation-and-artificial-intelligence-capabilities-through-strategic-acquisition-of-wip-industrial-automation

Lear公司宣布已达成最终协议，将收购西班牙的私营系统集成商WiP工业自动化公司。WiP专注于为工业应用开发高级自动化解决方案，拥有25年自动化经验。此次收购预计将于2024年第三季度完成。通过这次收购，Lear将增强其在机器人和基于AI的计算机视觉方面的能力，进一步提升其全球运营效率。Lear计划利用WiP的技术，为未来的自动化技术发展提供创新支持，推动公司在自动化和数字化方面的全球领导地位。

公司官网：https://www.wip.es/en/

![](img/236ebe2d9503865096183ee3ea3a65df.png)

### Neural Concept 完成2700万美元融资，加速电动汽车设计

贡献人：

https://techcrunch.com/2024/06/03/swiss-startup-neural-concept-raises-27m-to-cut-ev-design-time-to-18-months/

瑞士初创公司Neural Concept完成2700万美元B轮融资，由Forestay Capital领投，新投资者D.E. Shaw集团及现有投资者Alven、Constantia New Business、HTGF和Aster Group参与。本轮融资将用于招聘和在欧洲、亚太地区及美国的扩展。Neural Concept利用深度学习技术，帮助设计师在制造前模拟组件性能，将电动汽车的设计时间从四年缩短到18个月。公司的客户包括Airbus、Bosch、通用电气、Mubea、Subaru及四支一级方程式车队。

公司官网：https://www.neuralconcept.com/

![](img/7c4809d549a086f537d4523da8a4dda2.png)

# 学习

### I-LLM：首次实现了LLM全整形量化，精度逼近浮点，超过Smooth/Omini/AffineQuant

贡献人:

https://zhuanlan.zhihu.com/p/701393483

I-LLM 项目开发了一种全新的 integer-only 量化方法，专为大型语言模型（LLM）设计。该方法通过 Fully-Smooth Block-Reconstruction（FSBR）技术平滑了模型中的激活值和权重之间的变化，显著提高了量化后的模型准确性。FSBR 通过学习平滑系数，对所有激活值和权重进行平滑处理，考虑了 Serial norm-linear、Parallel linear-linear、Serial Linear-Linear、Nonlinear Act-Smooth 四种范式。此外，I-LLM 采用了 Dynamic Integer-only MatMul（DI-MatMul）技术，通过 dyadic number 近似浮点乘法，实现了全整形的矩阵乘法，并通过位移操作来实现指数函数和除法操作，从而解决了 LLMs 中 GEMM 量化推理中的动态量化问题。I-LLM 还提出了 DI-ClippedSoftmax、DI-Exp 和 DI-Normalization 等非线性算子的全整形动态量化方案，确保了在整数运算的基础上保持模型精度。实验结果证明，I-LLM 在 LLaMA 和 OPT 系列模型上的性能优于现有的量化方法，尤其是在 W6A6 和 W4A4 的配置下，其精度接近或超过了浮点模型，为 LLM 在边缘设备上的高效部署和推理提供了有效的解决方案。

![](img/e42ee02c40c5db4b5c82fefdfa3690b4.png)

### 2024年了，Transformer在哪些领域被RWKV/Mamba全面碾压

贡献人：

https://zhuanlan.zhihu.com/p/700331629

文章探讨了 Transformer 模型在自然语言处理和人工智能领域的应用，特别是它与动态规划之间的联系。作者通过详细的数学推导，展示了 Transformer 中的注意力机制如何与动态规划的状态转移方程相对应。文章进一步分析了 Transformer 在计算生物学、物理学、几何学和图论等领域中的动态规划应用。此外，文章从微观复杂度的角度探讨了 Mamba 和 RWKV 等结构如何在特定任务中替代 Transformer，并提出了 “Transformer-RWKV 假设”，指出对于某些复杂度较高的问题，标准的 Transformer 可能更为适用。同时，文章指出在时间序列和 RNA/DNA 问题上，高效 Transformer 和状态空间模型可能更优。最后，文章质疑了 next token prediction 是否能有效学习动态规划，并探讨了可能的解决方案，如结合探索、链式思考和强化学习。

![](img/94ce804bf079557c8a1440a4048c1a1e.png)

### 揭秘C++：虚假的零成本抽象

贡献人：

https://zhuanlan.zhihu.com/p/700751030

文章深入探讨了 C++ 中所谓的 “零成本抽象” 特性，揭示了其在实际应用中可能存在的额外运行时开销。具体分析了std::unique_ptr、std::forward_list和std::tuple这三个例子，指出它们虽然设计初衷是为了提供便捷的抽象而不增加性能负担，但实际上由于自定义的拷贝或移动构造函数，以及 ABI 的要求，它们在函数传递等场景下会导致性能损失。文章强调，尽管这些特性带来的额外成本在一般情况下可以忽略不计，但在追求极致性能的场景中，开发者应该注意这些潜在的开销，并在必要时寻找替代的实现方式以达到真正的零成本抽象。

![](img/609285ff3fa7102699feb37e9f057532.png)

### [ICML 2024] 高效架构SLAB: Attention简化，无需LayerNorm，视觉/语言任务精度无损效率提升

贡献人：

https://zhuanlan.zhihu.com/p/700170686

本文介绍了 SLAB，一种高效的 Transformer 架构，它通过简化线性注意力机制和采用渐进式重参数化 BatchNorm 策略，优化了模型性能。在技术细节上，SLAB 使用了 ReLU 算子作为相似度函数，并融入了深度可分离卷积来增强局部特征提取。渐进式重参数化 BatchNorm（PRepBN）策略通过引入可学习参数 η 和超参数 γ，实现了从 LayerNorm 到 BN 的平滑过渡，提高了推理速度，同时保持了模型精度。实验结果显示，SLAB 在 ImageNet1k、COCO 和 Wikitext-103 等数据集上的应用，能够在精度无损甚至提升的情况下，显著降低模型推理的时延。

![](img/bbf33e7db101aee93cb4be075a901054.png)

### 原理&图解vLLM Automatic Prefix Cache(RadixAttention): 首Token时延优化

贡献人：

https://zhuanlan.zhihu.com/p/693556044

文章深入探讨了 vLLM 自动前缀缓存机制（RadixAttention），旨在优化大型语言模型（LLM）中首个 Token 的处理时延。该机制通过构建基数树（Radix Tree）来自动识别和缓存序列中的共同前缀，实现对 KV Cache 的重用。文章详细阐述了 Radix Tree 的动态变化过程，以及如何利用哈希码作为物理 KV Block 的唯一标识，以简化工程实现。在处理多轮对话时，vLLM 能够缓存 Prefix 和 Generated KV Cache，从而在新的对话轮次中减少重复计算，提升性能。此外，文章还讨论了在边界情况下的缓存策略，以及如何通过 LRU 算法在显存不足时进行缓存块的逐出。最终，通过实际应用实践和代码分析，验证了 vLLM 自动前缀缓存机制在提升 LLM 推理效率方面的实用性和效果。

![](img/ee15572f2188efa0ce3a8cc77c82e177.png)

### 创建、设计和部署机器人应用程序：使用新的NVIDIA Isaac基础模型和工作流程

贡献人:

https://developer.nvidia.com/blog/create-design-and-deploy-robotics-applications-using-new-nvidia-isaac-foundation-models-and-workflows/

NVIDIA 推出了新的 NVIDIA Isaac 基础模型和工作流程，以促进机器人应用的创建、设计和部署。其中包括：

+   Isaac Perceptor：为自主移动机器人（AMR）和自动导引车辆（AGV）提供了一个新的参考工作流程，支持多相机 AI 深度感知、多相机视觉惯性里程计（VIO）、实时多相机体素网格映射等功能，以帮助机器人在复杂环境中进行导航和障碍物避让。

+   Isaac Manipulator：提供了加速库和 AI 模型，用于赋予机器人臂 AI 加速能力，包括：

+   cuMotion：GPU 加速的运动规划器，减少周期时间。

+   FoundationPose：用于单次 6D 姿态估计和跟踪的统一基础模型。

+   SyntheticaDETR：基于 DETR 的模型，用于单次图像空间中的物体检测，可在 NVIDIA Omniverse 生成的合成数据上进行训练。

+   JetPack 6.0：支持 Isaac ROS 3.0，加速 AI 和机器人应用的开发。

+   Isaac Sim 4.0：提供了一个轻量级的参考应用程序 Isaac Lab，用于机器人基础模型的训练，支持强化学习、模仿学习和迁移学习，并增加了对多款机器人的支持。

![](img/a5977d035b1b481e242c822660aca4ac.png)

### 银河通用王鹤：让具身智能机器人“言出法随”，需攻克两大局限性丨GenAICon 2024

贡献人：

https://mp.weixin.qq.com/s/EK2KheSholTnr7DEqU6SoQ

银河通用的王鹤项目旨在开发具身智能机器人，使其能够理解并执行复杂指令。该项目面临两大技术挑战：认知局限性和行动局限性。认知局限性体现在机器人难以准确解析人类语言和上下文信息，限制了其理解和适应复杂场景的能力。行动局限性则涉及机器人在实际环境中执行任务的精确度和适应性，特别是在动态变化的环境中进行精确控制和操作的能力。为了克服这些挑战，GenAICon 2024 将提供一个平台，汇聚专家共同探讨如何通过先进的自然语言处理、机器学习、传感器融合和机械设计等技术，提升机器人的认知和行动能力，推动具身智能机器人技术的进步。

![](img/dd3761738a0a8a65681fc20e0d821d73.png)

### 借着triton inference server聊一下各种batching方法

贡献人：

https://mp.weixin.qq.com/s/Y-3ZMvn_gZK1u5I6_woq_g

Triton Inference Server 支持多种批处理方法，优化推理过程。同步批处理要求客户端等待形成批次，有助于提升 GPU 资源利用率，但可能增加延迟。异步批处理允许客户端立即发送请求，减少延迟，但可能不利于 GPU 资源的最大化利用。动态批处理结合了前两者的优点，自动调整批处理大小，兼顾延迟和资源利用。Triton 还提供了自适应批处理大小调整、请求优先级处理和 Instance Group 等高级特性，以适应不同的推理场景，实现性能最优化。这些技术细节体现了 Triton 在提升推理效率和灵活性方面的优势。

![](img/de3b8c4a2de7290682e671fa8519ae25.png)

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

![](img/3d49a79ae684efcfbfe4f1d2c81a2d9b.png)