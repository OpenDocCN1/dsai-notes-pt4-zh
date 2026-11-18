
# 奇绩大模型日报（10月 15日）

> 来源：[https://miracleplus.feishu.cn/docx/G1XLdvUZdoM3dGxZBhwcEjPrnre](https://miracleplus.feishu.cn/docx/G1XLdvUZdoM3dGxZBhwcEjPrnre)

## 🔉潜空间活动报名

### 潜空间系列活动重磅回归！

> 本期活动将在**10月20日** **10:00**开始，我们邀请到的嘉宾是**袁进辉**，硅基流动（SiliconFlow）创始人兼CEO，曾任微软亚洲研究院主管研究员，获得微软亚洲研究院院长特别奖。硅基流动目前专注于 AI Infra 领域，解决大模型训练与推理的效率问题，降低 AI 应用开发的门槛与成本。在本次分享中袁进辉将带来《AI Infra 创业10年得与失》的分享。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 GenAI 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/1b5cf77644a60054fb2328933eece87f.png)

## 资讯

### Google与Kairos Power签署合作协议，利用微型核反应堆为数据中心供电

https://techcrunch.com/2024/10/14/google-signed-a-deal-to-power-data-centers-with-nuclear-micro-reactors-from-kairos-but-the-2030-timeline-is-very-optimistic/

谷歌近日宣布与核能初创公司Kairos Power达成合作协议，计划建设7座小型反应堆，为其数据中心提供约500兆瓦的无碳电力。这些核电站预计将在2030年之前上线，以应对数据中心和人工智能对电力需求的急剧增长。虽然具体是直接为谷歌的数据中心供电，还是通过电网供应尚不明确，但这一合作标志着谷歌加入了微软和亚马逊等科技巨头，开始转向核能以满足其日益增长的电力需求。

尽管Kairos的目标是2030年投入运营，但核电项目建设通常面临巨大的挑战。Kairos计划使用小型模块化反应堆（SMR）技术，该技术通过模块化制造和快速施工降低成本和时间。Kairos的反应堆采用熔盐冷却技术，而非传统的水冷方式，已获得美国核监管委员会的批准进行35兆瓦示范反应堆建设。

然而，尽管Kairos取得了监管进展，但面临的挑战依然巨大。SMR的经济性尚未得到商业验证，此外，公众对核能的支持也不稳定，尤其是在选址问题上仍存在较大争议。相比核能，公众对风能和太阳能的支持率更高，这也为核能项目的推进增添了不确定性。

![](img/781f6a9c562257e74415a212c3e5fe88.png)

### OpenAI挖走微软生成式AI研究主管

https://techcrunch.com/2024/10/14/openai-snatches-up-microsoft-generative-ai-research-lead/

OpenAI从微软挖来了生成式AI研究的副总裁塞巴斯蒂安·布贝克（Sebastian Bubeck），这一变动首先由《The Information》报道，并得到路透社的确认。布贝克是微软`Phi`模型系列的主要作者之一，这些小型语言和视觉模型旨在推动AI应用进入边缘设备。这种技术正变得越来越重要，尤其是在某些市场中，离线、快速处理的设备端模型逐渐取代了大型集中式模型，如OpenAI的`GPT-4`。

布贝克的新角色尚未公开，但很可能他将在OpenAI致力于提高AI模型的效率及开发小型模型——这是目前OpenAI尚不占据主导地位的AI领域。

### 迟分是什么，不是什么【下篇】

https://mp.weixin.qq.com/s/3eDgCZrjHTOiNzoHJclhpg

文章主要深入探讨了迟分策略（Late Chunking）与其相关的概念和技术细节。作者建议先阅读上一篇文章《长文本 Embedding 模型中的“迟分”策略》以及研究论文，以便更好地理解迟分在处理长文档时的重要性。

核心问题包括两个方面：

1. 边界断点如何确定：可以使用固定长度的`Token`、固定数量的句子，或者使用语义分割模型，如`Jina`的`Segmenter`。

2. 上下文信息的丢失：在分块向量化的过程中，容易丢失全局上下文信息。

传统解决方案多集中在边界检测上，如语义分块，通过确保相似度高的句子在一起，最大程度上保留语义完整性。然而，文章认为第二个问题——上下文信息丢失更为关键。

迟分策略的主要目的是解决上下文信息丢失问题。与其他分块方法不同，迟分采取了“先向量化后分块”的方式。具体而言，它先对整个文档进行编码，然后再基于边界线索进行均值池化操作。这样可以确保全局上下文信息不会在向量化过程中丢失，且对分块边界的精确性要求也不高。

实验表明，迟分策略对不同大小的向量模型（如`jina-embeddings-v2-small`、`nomic-v1`、`jina-embeddings-v3`）在不同数据集上都有稳定的性能提升。尽管迟分有效，但向量模型的质量依然是决定最终性能的关键因素。通过相对基线模型的比较，迟分策略在边界线索的使用上表现出对模型性能的显著改进。

另一个常见的误解是认为迟分仅考虑了前向的上下文，实际上，迟分是双向依赖的，因为它利用了向量模型的全局注意力机制，能够同时考虑上下文窗口内的所有信息，增强了全局语义的捕捉能力。

迟分策略不需要额外训练向量模型，任何使用均值池化的长文本向量模型都可以应用。文章还比较了其他上下文增强方法，如 Anthropic 的上下文检索，但强调迟分在效率和资源消耗上有更大优势。

结论是，迟分策略是基于对`Transformer`机制深刻理解的高效方法，能够在长文本处理中提供更加自然和精准的结果。

![](img/ebc32c7f5d6010224cd661f6b9947301.png)

### 特斯拉FSD深度：架构详解、优势及不足、入华进度及相关公司深度梳理

https://mp.weixin.qq.com/s/kZjukuLMJLqfHsmrG0P0fA

特斯拉在全球智能驾驶领域处于领先地位，其完全自动驾驶（FSD）系统是一套集感知、规划、执行等全链路的自动驾驶软硬件架构，具有高度集成的算法、算力和数据支持。FSD系统功能包括自动变道、自动泊车、智能召唤、交通灯识别等，其技术优势主要体现在数据积累、强大算力以及自研硬件的适配性。特斯拉通过海量的行驶数据不断迭代优化其FSD技术，最新版本`V12`实现了端到端神经网络架构，进一步提升了系统的智能化水平。

近年来，由于电动汽车业务的增速放缓，特斯拉将FSD视为新的利润增长点，推出订阅制以增加经常性收入。同时，特斯拉积极推动FSD在全球，尤其是在中国这一全球最大新能源汽车市场的应用。中国市场对FSD的潜在需求巨大，但FSD进入中国市场面临两大挑战：其一是数据安全问题，特斯拉已经通过本地数据存储等举措提升合规性；其二是自动驾驶技术的分级，中国要求`L3`及以上等级的自动驾驶才能被定义为自动驾驶，特斯拉的`L2`级技术需进一步提升。

特斯拉的FSD有望推动中国智能驾驶技术的发展，激发国内市场需求，带来“鲶鱼效应”。特斯拉的FSD和`Robotaxi`将通过技术突破和市场扩展对国内智能驾驶产业链产生积极影响。相关公司如经纬恒润、德赛西威、旭升集团等也将从中受益，共同推动智能驾驶的快速发展。

![](img/55c134df5a244ac69bd7b527e7e941b9.png)

## 推特

### 最甜甜甜的小草莓：🍓 `Ichigo-llama3.1`，本地实时语音AI

贡献人：

https://x.com/reach_vb/status/1845749495927062861

🍓 `Ichigo-llama3.1`: 本地实时语音AI

我们为 `Ichigo` 带来了两项重要改进：

• 它可以回应（是的！）

• 它能够识别何时无法理解输入

现在，您可以在自己的设备上运行这个小草莓了！单张 `@nvidia` `3090` GPU 的演示。

![](img/fde51f954b8b3e6e40a8a27aa33a5593.png)

[ssstwitter.com_1728978954583.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KPZkbyyxjocWgJxRYaqcjUtNnQy)

### `AgentStack`开源：开始下一个 Agent 项目最简单的方式

贡献人：

https://x.com/braelyn_ai/status/1845938983727255673

正式开源了 `AgentStack`！这是开始您的下一个 `Agent` 项目最简单的方式。

可以吐槽我的代码或者贡献代码：

`AgentStack` 适用于 `Python` 3.10+，许可证：MIT

---

从命令行创建 AI Agent 项目。

创建 Agent 项目 – 如何创建一个新的 Agent 项目。

用户指南 – 如何使用 `AgentStack` 开发启动的 Agent 项目。

`AgentStack` 兼容 `macOS`、`Windows` 和 `Linux`。

![](img/281ef25e5445f1f6222567921def18b5.png)

### `Model2Vec`：通过将词汇表传递给 `Sentence Transformer` 来提取一个快速模型

贡献人：

https://x.com/tomaarsen/status/1845875524297806143

`Model2Vec` 通过将词汇表传递给 `Sentence Transformer` 来提取一个快速模型，通过 `PCA` 降低嵌入维度并应用 `Zipf` 加权。生成的静态嵌入推理速度极快，例如每秒可处理 10k 条文本：

https://huggingface.co/blog/Pringled/model2vec

![](img/1ce352ae9798ede3c3d8c65dcfa57d14.png)

### Adobe推出`Project Concept`：在 AI 时代的情绪板和概念设计，专为每个创意旅程的早期阶段设计的 AI 优先产品

贡献人：

https://x.com/scottbelsky/status/1845849779856662583

很高兴分享一个新产品（也是 Adobe 的新类别）：`Project Concept`，在 AI 时代的情绪板和概念设计。

`Project Concept` 是一款专为每个创意旅程的早期阶段设计的 AI 优先产品，旨在减少我们在生产中枯燥重复的部分上花费的时间，更多地探索可能性的全方位并找到更好的想法。它结合了 Adobe 最新的 `Firefly` 生成式 AI 模型功能，帮助你快速探索潜在的艺术方向，混合图像，转换资源的区域，并重新混合风格、背景和其他组成部分。

你可以使用自己的资源或其他灵感来源来启动项目。然后，`Project Concept` 使用 AI 和协作工具来帮助进行发散思维，揭示任何创意项目的广泛可能性，同时进行聚合思维，将想法结合在一起，应用控制，最终选择要采用的最终方向。

对此非常期待。`Project Concept` 今天进入私人测试版，现在回去继续工作了……#AdobeMAX

![](img/29e19d5b8dfd727cea39fa98b41060d4.png)

[ssstwitter.com_1728979240593.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FW0sb0grpothHJxecufcUYyNnnb)

### xAI黑客马拉松第一名： Chrome 扩展程序，将自己的算法应用到任何网站上，使用 `grok` 来过滤信息流

贡献人：

https://x.com/SohilAthare/status/1845662874275926412

我们昨天在 `@xAI` 黑客马拉松中获得了第一名！感谢 `@xAI` 团队的举办——非常有趣。

我们构建了一个 Chrome 扩展程序，允许你将自己的算法应用到任何网站上，使用 `@grok` 来过滤你的信息流，从 X 开始。

![](img/41c3063ff1844797bd5868d07ee7d8e6.png)

[ssstwitter.com_1728979446282.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F4ijbHdSzoiqZXx2zqCcvVgznvd)

## 信号

### Toward General Instruction-Following Alignment for Retrieval-Augmented Generation

贡献人:

原文链接：https://arxiv.org/pdf/2410.09584

信号源：https://huggingface.co/papers

研究节点：开发体系-RAG

这篇文章介绍了`VIF-RAG`，这是一个自动化、可扩展且可验证的合成流程，用于在检索增强生成（RAG）系统中提高指令遵循（IF）的一致性。研究者们首先手动创建了一组最小的原子指令，并开发了组合规则来合成和验证复杂指令。然后，他们使用监督模型来重写指令，并通过`Python`执行器自动生成代码以验证指令质量。此外，他们还创建了一个高质量的`VIF-RAG-QA`数据集，包含超过100k的样本，并通过自动化流程扩展。为了进一步缩小RAG系统在指令遵循自动评估方面的差距，研究者们引入了`FollowRAG`基准测试，包含约3K的测试样本，涵盖22种通用指令约束类别和四个知识密集型问答数据集。`VIF-RAG`显著提高了LLM在各种通用指令约束下的性能，并在RAG场景中有效利用了其能力。文章还提供了实现RAG系统中IF对齐的实际见解。研究者们发布了代码和数据集。

![](img/a860426e7c694ccb796aa00bf352b488.png)

### MMIE: Massive Multimodal Interleaved Comprehension Benchmark for Large Vision-Language Models

贡献人:

原文链接：https://arxiv.org/pdf/2410.10139

信号源：https://huggingface.co/papers

研究节点：模型体系-评估体系

这篇文章介绍了一个名为`MMIE`的大规模基准测试，用于评估大型视觉-语言模型（LVLMs）在交错多模态理解和生成方面的能力。`MMIE`包含20,000个精心策划的多模态查询，覆盖了3个类别、12个领域和102个子领域，支持交错输入和输出，并提供多种选择题和开放式问题格式来评估不同的能力。此外，文章还提出了一种可靠的自动化评估指标，该指标基于经过人类标注数据微调的评分模型和系统化的评估标准，旨在减少偏见并提高评估准确性。实验结果表明，`MMIE`能够全面评估交错LVLMs的表现，同时也揭示了现有最佳模型仍有很大的改进空间。

![](img/1ccce9250dfcd9f54222cc1025472bdb.png)

### Omni-MATH: A Universal Olympiad Level Mathematic Benchmark For Large Language Models

贡献人:

原文链接：https://arxiv.org/pdf/2410.07985

信号源：https://huggingface.co/papers

研究节点：模型体系-具身智能

这篇文章提出了一个新的基准测试，旨在评估大型语言模型（LLMs）在奥林匹克级别的数学推理能力。现有的基准测试，如`GSM8K`或`MATH`，已经被高级模型以高准确率解决，因此需要一个新的挑战性基准来测试这些模型的能力。新提出的基准测试专注于数学领域，包含4428个竞赛级别的问题，这些问题经过了严格的人工标注，并被细致地分类到超过33个子领域，涵盖了10个不同的难度级别。实验结果显示，即使是最先进的模型，如OpenAI `o1-mini`和OpenAI `o1-preview`，在面对高度挑战性的奥林匹克级别的问题时，准确率也分别只有60.54%和52.55%，这突显了在奥林匹克级别数学推理方面的重大挑战。

![](img/39d05db4d2b6757daf42a06c53a3cf63.png)

### LOKI: A Comprehensive Synthetic Data Detection Benchmark using Large Multimodal Models

贡献人:

原文链接：https://huggingface.co/papers/2410.09732

信号源：https://x.com/eric_srchen/status/1845875700978655589

研究节点：应用体系-多模态模型

这篇文章介绍了一个名为`LOKI`的新基准，用于评估大型多模态模型（LMMs）在检测合成数据方面的能力。随着AI生成内容的快速发展，区分真实与合成数据变得越来越重要。`LOKI`涵盖视频、图像、3D、文本和音频等多种模态，包含18,000个精心策划的问题，覆盖26个子类别，并有明确的难度等级。该基准测试包括粗粒度判断和选择题，以及细粒度异常选择和解释任务，能够全面分析LMMs的表现。文章对22个开源LMMs和6个闭源模型进行了`LOKI`上的评估，展示了这些模型作为合成数据检测器的潜力，同时也揭示了它们在能力发展上的一些局限性。

![](img/a854b587c906f7a57413d899186f17e9.png)

### Rethinking Data Selection at Scale: Random Selection is Almost All You Need

贡献人:

原文链接：https://huggingface.co/papers/2410.09335



# 研究前沿

信号源：https://huggingface.co/papers

研究节点：模型体系-后训练

这篇文章探讨了在大规模数据集中对大型语言模型（`LLM`）进行监督式微调（`SFT`）时的数据选择问题。文章的主要目标是在`SFT`中从更大的数据池中选择一个小型但具有代表性的子集，以便使用这个子集进行微调能够达到与使用整个数据集相当或更好的结果。研究发现，大多数现有的数据选择技术都是为小规模数据池设计的，无法满足现实世界中`SFT`场景的需求。文章复制了几种自评分方法，这些方法不依赖于外部模型的帮助，并且在大规模数据集上进行测试，发现几乎所有方法在处理大规模数据池时的表现都难以显著优于随机选择。此外，文章还分析了几种当前方法的局限性，并解释了为什么它们在大规模数据集上表现不佳，以及为什么它们不适合这样的环境。最后，文章发现通过令牌长度过滤数据提供了一种稳定且高效的方法来提高结果，特别是当训练长文本数据时，这种方法对相对较弱的基础模型（如`Llama3`）非常有益。

![](img/4fa33337091f24745cd3ec9a4575a85c.png)

### Language model developers should report train-test overlap

贡献人:

原文链接：https://arxiv.org/abs/2410.08385

信号源：https://x.com/RishiBommasani/status/1845645918747676931

研究节点：模型体系-评估体系

这篇文章指出了语言模型评估中一个关键但常被忽视的问题：训练-测试重叠（train-test overlap），即模型在测试时所用的数据与训练数据的重合程度。当前公众缺乏关于训练-测试重叠的充分信息，因为大多数模型没有公开相关的统计数据，第三方也无法直接测量这一重叠，因为他们无法访问训练数据。文章调查了30个模型的实践情况，发现只有9个模型报告了训练-测试重叠信息，其中4个模型开放了训练数据，5个模型发布了它们的训练-测试重叠方法和统计数据。通过与语言模型开发者合作，作者为另外三个模型提供了新的训练-测试重叠信息。文章主张语言模型开发者应当在报告公共测试集上的评估结果时，发布训练-测试重叠统计数据或训练数据，以增加评估透明度和社区对模型评估的信任。

![](img/204284a688e4fad6719b218cdac65f4f.png)

### Zero-latency SQLite storage in every Durable Object

贡献人:

原文链接：https://x.com/swyx/status/1845714328260878535

信号源：https://simonwillison.net/2024/Oct/13/zero-latency-sqlite-storage-in-every-durable-object/

研究节点：算力体系-基础软件/模型部署

这篇文章介绍了Cloudflare的`Durable Object`平台的最新迭代，它将一个基于`SQLite`的完整关系系统整合到了`Durable Object`中，实现了零延迟的`SQLite`存储。文章强调了`Durable Object`的设计理念，即将应用逻辑与数据存储在同一物理主机上，以实现快速的读写性能。此外，还讨论了如何在大规模应用中实现这一点，包括通过创建更多的对象来处理更多的流量，以及如何通过Cloudflare的网络路由请求到特定的`Durable Object`。技术细节包括每个`Durable Object`不断向对象存储流式传输`WAL`日志条目，以及如何确保在10秒窗口内的数据持久性。

![](img/ed5bffe3acc6b6b0f10dd5a91b26e0dc.png)

### DROP: Dexterous Reorientation via Online Planning

贡献人:

原文链接：https://arxiv.org/abs/2409.14562

信号源：https://x.com/albert_h_li/status/1844408066407792847

研究节点：模型体系-具身智能

这篇文章探讨了在接触丰富的操作任务中使用在线规划方法的可行性，特别是针对著名的手内立方体重定向任务。作者提出了一种简单的架构，该架构结合了基于采样的预测控制器和基于视觉的姿态估计器，以在线搜索接触丰富的控制动作。通过广泛的实验，文章评估了该方法在现实世界中的性能、架构设计选择以及关键因素，并展示了这种基于简单采样的方法能够达到与先前基于强化学习（`RL`）的方法相媲美的性能。

![](img/afe88d176f73c1770d5034ee3a8871c9.png)

### Unintentional Unalignment: Likelihood Displacement in Direct Preference Optimization

贡献人:

原文链接：https://arxiv.org/abs/2410.08847

信号源：https://x.com/noamrazin/status/1845859648114450929

研究节点：模型体系-后训练体系

这篇文章探讨了直接偏好优化（`DPO`）及其变体在使语言模型与人类偏好对齐时出现的一个反直觉现象——即在训练过程中，优选响应的似然性反而下降，这一现象被称为“似然位移”。文章揭示了这种现象的原因和影响，指出它可能导致概率质量从优选响应转移到相反意义的响应上。例如，训练一个模型更偏好`“No”`而不是`“Never”`，可能会意外地增加`“Yes”`的概率。此外，当试图让模型拒绝不安全的提示时，这种位移可能无意中导致模型不再对齐，从而增加有害响应的概率。文章通过理论分析发现，这种似然位移是由诱导相似嵌入的偏好驱动的，并提出了一个中心化隐藏嵌入相似性（`CHES`）评分来量化这一点。实验证明，过滤掉那些`CHES`评分高的样本可以有效减轻无意中的不对齐问题。

![](img/e4e60aba50464c192117471ad2f96c1a.png)

### Uncovering Latent Memories: Assessing Data Leakage and Memorization Patterns in Frontier AI Models

贡献人:

原文链接：https://arxiv.org/abs/2406.14549

信号源：https://x.com/RylanSchaeffer/status/1845922206205071467

研究节点：AGI综述-AI基础知识

这份报告包含了加州大学伯克利分校机器学习入门课程的讲义，涵盖了多种分类和回归方法，以及一些聚类和降维技术。它内容精炼，仅包含在一个学期内可以讲授的内容，并且选择了特别有用和流行的算法进行讲解。

![](img/68ea69fd63dcaf5cc866b13e75e7bd30.png)

### ARCap: Collecting High-quality Human Demonstrations for Robot Learning with Augmented Reality Feedback

贡献人:

原文链接：https://arxiv.org/abs/2410.08464

信号源：https://x.com/eric_srchen/status/1845875700978655589

研究节点：模型体系-具身智能

这篇文章介绍了一种名为`ARCap`的便携式数据收集系统，它通过增强现实（`AR`）提供视觉反馈和触觉警告来指导用户收集高质量的演示数据。`ARCap`旨在解决使用便携设备进行数据收集时缺乏机器人实时反馈的问题，使得即使是新手用户也能收集到符合机器人运动学要求且避免场景碰撞的数据。实验表明，使用`ARCap`收集的数据可以帮助机器人执行复杂的任务，如在杂乱环境中操作物体以及跨不同机器人形态的长时间操作。`ARCap`是完全开源的，并且易于校准，所有组件都是由现成产品构建而成。

![](img/6001d3402def8dcf879e696db9cc234e.png)

### ACDC: Automated Creation of Digital Cousins for Robust Policy Learning

贡献人:

原文链接：https://arxiv.org/abs/2410.07408

信号源：https://x.com/RogerDai1217/status/1844411408374693941

研究节点：应用体系-智能硬件

这篇文章提出了一种名为“数字表亲”（digital cousins）的概念，它是一种虚拟资产或场景，虽然不像数字孪生那样精确复制现实世界中的对应物，但仍然保留了相似的几何和语义属性。通过这种方式，数字表亲可以降低创建类似虚拟环境的成本，并且通过提供一系列相似的训练场景来提高从仿真到现实世界的迁移鲁棒性。文章还介绍了一种自动创建数字表亲的方法`ACDC`（Automatic Creation of Digital Cousins），以及一个全自动的真实-仿真-真实流水线，用于生成完全交互式的场景并训练机器人策略，这些策略可以在原始场景中零样本部署。实验表明，使用`ACDC`训练的策略在零样本仿真到现实世界的迁移中表现优于使用数字孪生训练的策略。

![](img/f1e9905a3d30a9f3bc88934484d80fc0.png)

# 产品

### LLMWare

贡献人：

https://llmware.ai/

`LLMWare` 是一个专注于小型语言模型的开发工具，希望为企业提供安全、私密的人工智能应用。它支持在本地或私有云中运行，确保敏感数据的安全，特别适合金融服务和合规行业。通过丰富的开源示例和视频教程，帮助开发者快速上手，自动化日常工作任务。

![](img/78fc38440ca72434f75f506393a8ced2.png)

### PicLooks

贡献人：

https://www.piclooks.com/landing/avatars

`PicLooks` 是一个提供免费 AI 生成头像的平台，希望为设计师提供真实感强、种类多样的个人资料图片，帮助他们节省寻找库存照片的时间。所有头像均可用于商业用途，且看起来更像真实的人，而非模特。

![](img/b3d385557452c48365ad0c124a870093.png)

# HuggingFace&Github

### SWARM

贡献人：

https://github.com/openai/swarm

`Swarm` 是一个知识性内容，探索多智能体编排的人性化和轻量级方法。它提供了两个基本内容：智能体和交接。智能体包含指令和工具，可以随时选择将对话交给另一个智能体。这些足以表达工具和智能体网络之间的丰富动态，使用户能够构建可扩展的实际解决方案，同时避免过于复杂的学习。

![](img/d6447814f845b3f37314c755e9b8538f.png)

### Genaiscript

贡献人：

https://github.com/microsoft/genaiscript

`GenAIScript` 是一个功能强大的脚本环境，专门为构建和管理大型语言模型（`LLM`） 提示而设计。无论是开发人员、数据科学家还是研究人员，`GenAIScript` 都提供了相对应的工具来高效地创建、调试和共享脚本。

![](img/d67c584d2fb12693ef267dafc9e8dab5.png)

### LLM PLSE paper

贡献人：

https://github.com/wcphkust/LLM-PLSE-paper

这个 GitHub 项目是一个关于使用大语言模型（`LLM`）进行代码分析和软件工程应用的研究合集。该项目涵盖了多个方向，包括漏洞检测、代码生成、代码推理、测试和调试等。

# 投融资

### Beyond Presence创始人融资310万美元，打造超逼真虚拟化身

https://techcrunch.com/2024/10/14/after-selling-his-last-ai-startup-to-meta-beyond-presences-founder-nabs-3-1m-to-build-realistic-avatars/

德国慕尼黑的初创公司Beyond Presence正在利用人工智能和计算机视觉技术开发超逼真的虚拟化身。该公司最近完成了首轮融资，获得了310万美元的预种子轮投资。领投方是德国的HV Capital，参投者包括10x Founders、Alba VC以及来自Meta、DeepMind和Zalando的个人投资者。

这笔资金将用于继续开发公司核心技术模型，并计划在本月末推出公测版。该产品预计将在客户服务、招聘、销售和电子学习等领域应用，帮助企业扩大用户互动规模。

Beyond Presence的联合创始人兼CEO Awais Shafique曾创办Presize，一家可以通过智能手机摄像头录制视频测量用户尺寸的计算机视觉公司。Presize在2022年被Meta收购，为其创始团队带来了可观的退出收益。这次成功的退出也为Shafique的最新创业项目奠定了良好的资本基础，吸引了多家投资者在产品尚未正式发布时提供融资支持。

公司官网：https://www.beyondpresence.ai/

![](img/a665080f74b578e4957ab6476884c5f9.png)

# 学习

### 浅谈ROPE位置编码模式下，`q`、`k`的分布（均值与方差）对注意力远程衰减的影响

贡献人：

https://zhuanlan.zhihu.com/p/975380493

这篇论文《ROUND AND ROUND WE GO! WHAT MAKES ROTARY POSITIONAL ENCODINGS USEFUL?》对`RoPE`（旋转式位置编码）的远程衰减机制进行了详细的实验探讨。传统观点认为，`RoPE` 的主要作用是增强随距离增加的依赖衰减，然而作者通过实验挑战了这一认知，指出远程衰减性并非必然存在，尤其在`Q`、`K`矩阵的初始分布为均值为0的高斯分布时，这种衰减效应消失。

实验部分探讨了不同的初始化条件下`RoPE`的表现，尤其针对`Q`、`K`矩阵的均值和方差的影响。主要发现如下：

1.  均值影响：当`Q`、`K`的均值同向且绝对值较大时，注意力远程衰减效应明显；反之，均值接近0或异向时，衰减效应减弱甚至消失。此外，均值异向（一个大于0，一个小于0）时，甚至观察到远程增加的注意力性质。

1.  方差影响：方差越大，远程衰减越弱，方差越小，衰减越显著。这暗示在模型初始化时应控制方差值以获得更好的远程衰减效果，如`GPT-2`系列中对参数方差的设置较小（如0.02）。

这些实验表明，`RoPE`下的注意力远程衰减性质与`Q`、`K`矩阵的分布密切相关。不同的初始化均值和方差可以导致不同层和注意力头对局部或全局信息的关注程度。这可能解释了在大模型中，不同层的`RoPE`编码具有不同的远程衰减特性。

![](img/f81744d8800450ce477b1af9e9feb426.png)

### 大模型`LLM`知识蒸馏代码讲解与训练

https://zhuanlan.zhihu.com/p/1064724364

知识蒸馏是一种通过大规模模型（`teacher model`）指导小规模模型（`student model`）训练的方法，其目的是通过交互两个模型的`logits`来优化`student`模型，而不是简单地使用答案。重要的是，`teacher`和`student`模型的`vocab size`必须一致。本文讨论了几种不同的知识蒸馏方式，特别是在论文《On-Policy Distillation of Language Models: Learning from Self-Generated Mistakes》中对`on-policy`和`off-policy`两类蒸馏方法的分类。

知识蒸馏的主要方法包括：

1.  `Supervised KD`：使用训练数据的`output`作为答案，将`prompt`和`output`拼接后输入模型。`teacher`和`student`模型在相同维度上生成`logits`，并计算两者之间的散度损失，常用的是`Jensen-Shannon Divergence (JSD)`。

1.  `SeqKD`：仅使用`prompt`，`teacher`模型生成`output`后进行训练。`student`模型使用`teacher`模型生成的`output`进行训练，其过程类似于`Supervised KD`。

1.  `GKD` (Generalized Knowledge Distillation)：`student`模型通过生成自己的`output`进行训练，这是一种`on-policy`方法，模型每次更新后生成新的`output`，并将其反馈回训练过程中。

在实现方面，`GKDTrainer`继承自`SFTTrainer`，而`SFTTrainer`又继承自`Trainer`。以下是各自的实现细节：

1.  `Trainer`：标准的`Trainer`要求用户手动处理数据，将`prompt`与`output`拼接，并定义输入数据的格式为`["input_ids", "labels", "attention_mask"]`。

1.  `SFTTrainer`：`SFTTrainer`对`Trainer`进行了封装，自动处理数据拼接，简化了用户操作，返回的数据格式与`Trainer`一致。



### GKDTrainer

`GKDTrainer`的输入要求更加复杂，包括一个`messages`列表，通常包含`role`和`content`字段，用于模拟对话形式的数据输入。

在GKD的训练过程中，最关键的部分是计算`student`和`teacher`模型`logits`之间的散度损失。常见的损失函数包括KL散度和JSD散度：

+ KL散度 (KLD)：用于衡量两个分布之间的差异，计算两个模型的`logits`差异。

+ JSD散度 (Jensen-Shannon Divergence)：基于KL散度的改进版本，具有更平滑和对称的特点，广泛用于知识蒸馏的损失计算中。

JSD的计算涉及温度缩放、log softmax转换等步骤，并通过`torch.kl_div`函数来计算KL散度，进而得到JSD。

训练过程中的关键参数包括：

+ `lambda`：在Supervised KD与GKD之间的权重平衡，范围为[0,1]。

+ `beta`：控制KLD与JSD的权重，范围为[0,1]。

+ `seq_kd`：是否启用SeqKD方法。

本文还展示了一个基于`trl`的代码实现，并结合`deepspeed`进行训练。实验以deepseek模型为例，使用deepseek 7b作为`student`模型，deepseek 33b作为`teacher`模型，通过调整`lambda`和`beta`进行混合蒸馏训练。实验结果显示，学生模型的性能高度依赖于教师模型的质量。未经微调的教师模型导致学生模型效果不佳，但当教师模型经过SFT微调后，学生模型的性能显著提升。

![](img/2c35d5061f4ed23ca531919e2a5e28a8.png)

### reasoning能力微调实战篇——微调qwen基座模型具备思考推理能力（类似o1）

https://zhuanlan.zhihu.com/p/1065570360

在这篇文章中，作者分享了基于OpenAI最新发布的O1模型的推理能力微调实战经验，背后的理论基础可以参考论文《Let’s Verify Step by Step》。具体实战方案主要围绕以下几个技术细节展开。

首先，数据集和基座模型选择了`reasoning-base-20k`数据集和`Qwen2.5-1.5B`模型。微调方法采用全参微调（SFT），但与标准SFT微调有所不同。主要差异点包括：增加了`reasoning`的special token，并同步调整了模型的embedding层；调整聊天模版，使其适配新的`reasoning`部分；在计算损失时，结合`reasoning`和`assistant`的部分统一考虑。

作者特别指出几个技术关键点：首先是增加special token，格式为`<|reasoning|>`，并通过调整模型的词嵌入层确保模型可以接受新的token。其次是调整聊天模版，使其能够处理`reasoning`和`assistant`部分的交互输入。在损失计算方面，作者使用了掩码机制，确保仅对推理和回答部分计算损失，避免无关部分影响模型学习。此外，作者使用了一张Google Colab上的A100显卡进行训练，方便模型快速收敛。

在模型的推理能力测试中，作者使用了一种两步法：首先生成推理的`reasoning`部分，然后将问题和推理的结果作为输入，生成最终的`assistant`回答。通过这种方式，模型能够详细解释复杂问题，例如在回答“找出0-10内的所有质数”时，模型提供了完整的推理过程，并且推理链条非常长，展现了强大的推理能力。

在实验过程中，作者提到由于训练数据较为单一，模型容易出现过拟合现象，尽管损失下降很快，但多样化的数据集可能有助于模型更好地泛化。

![](img/b3ff726f36efb1a9cbc552a867ecc972.png)

### 深度学习编译器 TVM 代码串讲

https://zhuanlan.zhihu.com/p/446976730

随着深度学习的广泛应用，如何高效地在不同设备上运行深度学习模型成为一个关键问题。传统方法依赖专家手动优化特定设备的算子，比如Nvidia的TensorRT，但在面对用户自定义算子或特殊网络结构时，这些方法存在局限性。深度学习编译器TVM提供了一种新的思路，通过对整个计算图进行优化并自动生成适配不同设备的代码。本文详细解读了TVM的工作机制，包括编译流程、代码结构以及运行时的实现细节。

TVM接收来自TensorFlow、PyTorch等框架的高层次模型，转换为Graph IR，并对其进行图层级别的优化。每个算子使用TVM定义的DSL“Tensor Expression”来描述，这种方式源于Halide的计算与调度分离思想。TVM通过机器学习优化器自动寻找最优的调度方式，最终生成低层次的LLVM IR，之后利用传统编译器完成编译。

运行时方面，TVM通过简化的C API管理模块和函数，允许通过`PackedFunc`调用编译后的算子。TVM采用了一种称为FFI的机制，使得跨语言的函数调用成为可能。此外，TVM的数据结构通过继承`Object`类实现序列化、格式化和Python绑定。

TVM的编译流程涵盖从高层次的Relay IR到低层次的TIR的转换，经过图优化、调度生成、内存规划等步骤，最终生成适用于不同硬件设备的代码。TVM还提供了丰富的模式匹配和重写机制，便于开发者编写`Pass`来优化图结构。

TVM的核心特性之一是其支持多种硬件设备的调度与代码生成，允许通过`AutoTVM`和`Ansor`等自动调优工具找到最优的执行方式，进一步提升了模型的性能表现。

![](img/90a0802ba15e80afac517822bd179735.png)

### 高阶体系结构自学大纲

https://zhuanlan.zhihu.com/p/1053997997

该学习路径主要面向CPU设计，分为不同阶段，涵盖微架构、乱序执行、多核架构以及IC设计的内容。学习过程中不要求严格按顺序，可根据自身需求和兴趣进行调整，提供了广泛的资源和工具。

体系结构入门： 微架构：基于RISC-V架构的`RV64GC`核心，支持`IMACFA`指令集，搭配`MSU`特权模式和`AXI4`总线协议（带突发传输）。关键组件包括TLB、无阻塞Cache、`Tournament`分支预测单元（BPU）。 性能优化：利用硬件计数器提升`Coremark`基准测试的跑分，目标是Coremark IPC ≥ 0.6，频率达到100MHz以上。 外设支持：包括`CLINT`、`PLIC`和`UART`等外设，确保系统能够处理中断和通信。 对齐设计：从功能模型和性能模型两个角度优化设计，确保准确对齐。 软件测试：涵盖`riscv-tests`、`cpu-tests`、`coremark`、`dhrystone`、`microbench`，以及`RT-thread`和Linux等操作系统的移植与测试。测试工具包括`Verilator`、`VCS`、`Design Compiler`（`DC`）和FPGA验证。

乱序执行入门： 参考架构：以`Ridecore`为基础，介绍乱序执行的概念和架构实现。 进阶参考：乱序执行相关的中文文档详细解释乱序CPU的设计和优化。

单核与多核进阶架构： 单核架构：实现基于RISC-V超标量处理器的RTL，并结合高性能CPU架构文档进行设计反思。参考架构包括龙芯`GS464E`和其他高性能CPU文档。 多核架构：多核设计参考`Vroom`架构，深入探讨缓存一致性等问题，文献来源包括《缓存一致性学习笔记》系列。

IC设计入门： 实践与实验：提供IC设计的实践参考，包括`yatcpu`文档、`AXI`总线、`CSR`、`Pipeline`、操作系统支持等主题。涉及到诸如`Nutshell`和`Zhoushan`等超标量乱序处理器的实现。 相关设计工具：使用`CVA6`和`openla500`等工具进行设计和实验，涵盖从基本IC设计到复杂的乱序架构。

![](img/08fb6443c677fc5737f2bbb2a834f7d4.png)

### Triton kernel performance on RISC-V CPU

https://zhuanlan.zhihu.com/p/1009050689

Triton 是 OpenAI 开发的基于 Python 的开源领域专用语言（DSL），旨在简化高性能 GPU 代码的编写。其设计采用了线程块级别的并行编程模型，基于 LLVM 的中间表示和分阶段的优化流程。Triton 提供了对 GPU 的支持，最早仅限于英伟达消费级 GPU，后来扩展至工业级 GPU（如 A100 和 H100），并正逐步适配其他 AI 硬件平台，包括国内外的 GPGPU 和 DSA 加速器等。Triton 的 CPU 后端也在开发中，进一步提升了其硬件兼容性。

Triton 允许开发者灵活操作 Tile 级数据的读写、执行计算原语并定义线程块划分方式，隐藏了复杂的线程块级别以下的细节。它能自动管理共享内存、线程并行、访存合并和张量布局等，从而降低并行编程的复杂性，提升开发效率。Triton 通过编译器的多层次优化，在许多场景下能够实现媲美 CUDA 的性能，如矩阵乘法的性能表现与 `cuBLAS` 相当，展现了出色的计算能力。

在硬件支持方面，Triton 针对计算线程阵列（CTA）进行优化，其核心是将张量计算有效地并行化。它的编译器流程会自动处理线程间的协同操作和共享资源的分配。Triton 的性能与灵活性在诸如 PyTorch、Unsloth 等多个项目中得到了应用。Triton 当前还在探索对 RISC-V 和其他架构的支持，特别是通过其依赖的 MLIR 编译器技术栈，Triton 能与定制硬件进行深度适配，进一步提升硬件资源的利用效率和算法的运行性能。

Triton 的多层次中间表示（IR）机制使得它在跨架构兼容性上具有很大潜力，尤其是在 AI 算法中的并行计算任务上。它的自动优化能力和易用性有助于芯片厂商大幅降低算子库开发与维护的成本，并且在 RISC-V 生态中表现出显著的优势，成为未来高性能计算领域的重要工具。

![](img/31e2a1423964ce64559aa6eebf66939a.png)

# 招聘公告

---

![](img/3b9c2ec326ded397c3651b395024547a.png)

奇绩创坛由陆奇博士于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

## 🚀 F24 实习岗位

#### [行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

#### 行研-GenAI研究岗位-战略分析（偏产品）

#### Web / AI 全栈开发工程师实习生

## 火热开放中🚀

#### 行研-GenAI研究岗位-前沿学术探索

#### 行研-GenAI研究岗位-战略分析（偏咨询）

#### 创新活动产品运营 （大模型系列活动产品运营）

> 点击相关岗位可查看对应岗位需求【JD】；相关岗位投递请扫描二维码

![](img/4da1c8612b15d8d254d82194dbf2d78f.png)

> 欢迎推荐/自荐

> 期待在奇绩创坛遇见更好的你

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

> 欢迎大家多多帮我们分享,转发

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/f5e9c474adc4315fb8cde1e5b79d62d0.png)
