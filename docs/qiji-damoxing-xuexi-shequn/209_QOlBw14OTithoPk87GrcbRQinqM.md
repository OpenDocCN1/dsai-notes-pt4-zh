# 奇绩大模型日报（5月 11-12日）

> 来源：[https://miracleplus.feishu.cn/docx/R9kEdBhOBorKA3xyyKDcA2YXnde](https://miracleplus.feishu.cn/docx/R9kEdBhOBorKA3xyyKDcA2YXnde)

# 资讯

### 百万tokens低至1元！大模型越来越卷了

https://mp.weixin.qq.com/s/42hA4Ggzu_p4Iqk8yr4VAQ

在刚**刚举行的 ICLR 2**024 大会上，智谱A**I的大****模型技术团队公布了面向**激动**人心的AGI通用人工智**能前景的三大技术趋势，同时预告了G**LM的****后续升**级版本。今天，智谱大模型开放平台（bigmodel.cn）上线了新的价格体系。入门级产品 GLM-3 Turbo 模型调用价格下调80%！从5元/百万tokens降至1元/百万tokens。1元可以购买100万tokens。调整后，使用GLM-3 Turbo创作一万条小红书文案（以350字计）将仅需约1元钱，足以让更多企业和个人都能用上这款入门级产品。

![](img/244bd0f588f2392eed41ed188ff96896.png)

### 人类偏好就是尺！SPPO对齐技术让大语言模型左右互搏、自我博弈论文

https://mp.weixin.qq.com/s/ulVGoBkCtFyV_mwSBdzgQg

Rich**ard Sutton在 「The B**itter Lesson」中做过这样的评价：「从70年的人工智能研究中可以得出的最**重要教训是，那些利用计算的通用方法最终是最有效的，而且优势**巨大。」自我博弈（self play）就是这样一种同时利用搜索和学习从而充分利用和扩大计算规模的方法。今年年初，加利福尼亚大学洛杉矶分校（UCLA）的顾全全教授团队提出了一种自我博弈微调方法 (Self-Play Fine-Tuning, SPIN)，可不使用额外微调数据，仅靠自我博弈就能大幅提升 LLM 的能力。最近，顾全全教授团队和卡内基梅隆大学（CMU）Yiming Yang教授团队合作开发了一种名为「自我博弈偏好优化（Self-Play Preference Optimization, SPPO）」的对齐技术，这一新方法旨在通过自我博弈的框架来优化大语言模型的行为，使其更好地符合人类的偏好。左右互搏再显神通！

![](img/6a68d12b0df9cc47f0c10381ab7b773c.png)

### DiT架构大一统：一个框架集成图像、视频、音频和3D生成，可编辑、能试玩

https://mp.weixin.qq.com/s/NwwbaeRujh-02V6LRs5zMg

今年 2 月初，Sora 的发布让 AI 社区更加看到了基础扩散模型的潜力。连同以往出现的 Stable Diffusion、PixArt-α 和 PixArt-Σ，这些模型在生成真实图像和视频方面取得了显著的成功。这意味着开始了从经典 U-Net 架构到基于 Transformer 的扩散主干架构的范式转变。值得注意的是，通过这种改进的架构，Sora 和 S**t**able Diffusion 3 可以生成任意分辨率的样本，并表现出对 scaling 定律的严格遵守，即增加参数大小可以实现更好的结果。 不过，推出者们只对自家模型的设计选择提供有限的指导，并且缺乏详细的实现说明和公开的预训练检查点，限制了它们在社区使用和复刻方面的效用。并且，这些方法是针对特定任务（例如图像或视频生成任务）量身定制的，这阻碍了潜在的跨模态适应性。为了弥补这些差距，上海 AI Lab、港中文和英伟达的研究者联合推出了 Lumina-T2X 系列模型，通过基于流（Flow-based）的大型扩散 Transformers（Flag-DiT）打造，旨在将噪声转换为图像、视频、多视图 3D 对象和基于文本描述的音频。其中，Lumina-T2X 系列中最大的模型包括具有 70 亿参数的 Flag-DiT 和一个多模态大语言模型 SPHINX。SPHINX 是一个文本编码器，它具有 130 亿参数，能够处理 128K tokens。

![](img/0688997bb121c8745057cec844e2f14c.png)

### 从零开始手搓GPU，照着英伟达CUDA来，只用两个星期

https://mp.weixin.qq.com/s/gDWQGs4MyVWqsmONdEqvpQ

总有人说老黄的芯片供不应求，大家恨不得去手搓 GPU，现在真的有人试了。近日，美国一家 web3 开发公司的创始工程师之一 Adam Majmudar 分享了他「手搓 GPU」成功的经历，引发了网友们的一大片点赞。令人惊讶的是，他仅用两周时间就完成了这一脑力壮举。在 Twitter/X 的主题帖子中，Majmudar 进行了直播，一步步带我们回顾了整个过程。

![](img/b3ad0a1655d2559257ed8d9dc1f5babb.png)

### Flash Attention稳定吗？Meta、哈佛发现其模型权重偏差呈现数量级波动

https://mp.weixin.qq.com/s/sG3JaZR1isZApWP6ZkYe6Q

众所周知，大语言模型的训练常常需要数月的时间，使用数百乃至上千个 GPU。以 LLaMA2 70B 模型为例，其训练总共需要 1,720,320 GPU hours。由于这些工作负载的规模和复杂性，导致训练大模型存在着独特的系统性挑战。最近，许多机构在训练 SOTA生成式 AI 模型时报告了训练过程中的不稳定情况，它们通常以损失尖峰的形式出现，比如谷歌的 PaLM 模型训练过程中出现了多达 20 次的损失尖峰。数值偏差是造成这种训练不稳定性的潜在原因，由于大语言模型训练运行成本极高，如何量化数值偏差俨然成为关键问题。在最新的一项工作中，来自 Meta、哈佛大学的研究者开发了一个原则性定量方法来理解训练优化中的数值偏差，以此评估不同的最新优化技术，并确定它们在用于训练大模型时是否可能引入意外的不稳定性。

![](img/62ff0eb0748c5a95722b0d0ca7a126e8.png)

### 博弈论让 AI 更加正确、高效，LLM 与自己竞争

https://mp.weixin.qq.com/s/gMUE2eg_B_jl5wCZpQlvjw

想象一下，你有一位朋友对同一问题给出了不同的答案，具体取决于你提问的方式。「秘鲁的首都是哪里？」会得到一个答案；「利马是秘鲁的首都吗？」 会得到另一个。你可能会有点担心你朋友的智力，而且你几乎很难相信他们给出的任何答案。这正是许多大型语言模型 (LLM) 正在发生的事，这些超强大的机器学习工具为 ChatGPT和其他人工智能奇迹提供了动力。开放式的生成性问题会产生一个答案，而涉及必须在选项之间进行选择的判别性问题，通常会产生不同的答案。麻省理工学院的博士生 Athul Paul Jacob 表示：「当同一个问题的措辞不同时，就会出现脱节。」为了使语言模型的答案更加一致，并使模型整体更加可靠，Jacob 和他的同事设计了一个游戏，在这个游戏中，模型的两种模式被驱使着去寻找他们能达成一致的答案。这个简单的程序被称为共识博弈（consensus game），让 LLM 与自己竞争，使用博弈论工具来提高模型的准确性和内部一致性。

![](img/04fce19ee66d2b0c17f1f0c08e2574a9.png)

### HeyGen 创始人对话硅谷投资人 Sarah Guo：SaaS 十倍增长背后，Bootstrapping 还是拿融资

https://mp.weixin.qq.com/s/vRb4fvB08lKcPEBxQUen6g

这是 HeyGen创始人 Joshua Xu 与硅谷知名投资人 Sarah Guo和 Pilot CEO Waseem Daher 最新对谈。HeyGen 在 3 月底完成 BechMark 领投的 6000 万美元融资，最新估值 4.4 亿美元。Joshua Xu 是 HeyGen 创始人，此前是 Snapchat 担任工程负责人，在 2014 年到 2020 年从事了很多工作，包括广告技术、推荐系统和 AI 驱动的相机技术。2020 年，他创立了 HeyGen ，旨在通过先进的 AI 生成技术改变视觉叙事。Sarah Guo 是 Conviction 的创始人，专注于投资智能软件，Conviction 也是 HeyGen 的早期投资方。Sarah Guo 此前在 Greylock 担任多年合伙人，是软件、金融科技、安全、基础设施、基础研究和 AI Native应用领域 40 多家公司的早期投资者或顾问之一。

# 推特

### Anthropic控制台中现在可以生成提示语啦！

贡献人：

https://x.com/AnthropicAI/status/1788958483565732213

你现在可以在Anthropic控制台中生成可用于生产环境的提示语。

描述你想要实现的目标，Claude将使用提示工程技术，如思维链推理，来创建更有效、更精确和更可靠的提示语。

---

Ethan Mollick: 对大多数人来说，提示工程正在消失，这是众多迹象中的最新一个：像其他一些AI公司一样，Anthropic发布了一个自动为你基于意图生成好的提示的工具。它工作得相当好！

![](img/c344fd0b339add701a162cead7282a86.png)

[ssstwitter.com_1715505016033.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AM6Db1QtMoLyFaxyVgucOEsAnXe)

### Jim Fan分享思考：我们学习主流神经网络的像素到大脑会怎么样？我们能否拦截人类计算的“思维链”信号？

贡献人：

https://x.com/DrJimFan/status/1788955845096820771

神经连接最近取得的成就给了我一个奇特的新想法去训练人工智能。下面是一个人仅凭思考就能以不合理的高速度和精确度控制鼠标。信息流看起来像是屏幕像素 -> 生物神经网络 -> 由人工神经网络转换 -> 连续的鼠标动作。

我训练了许多AI代理去玩游戏和控制浏览器。这让我思考：

如果我们学习另一个神经网络去模仿流程的第一部分：像素 -> 大脑会怎样？

我们能否拦截人类计算的“思维链”信号？

如果我们使用模仿学习这些数据，我们不是能够模拟一些像意识这样难以捉摸的东西吗？

这是不是一个直接从人类内在思维学习智能的替代路径，而不是间接从我们的书面文本令牌学习？

我们能做到吗？是的，我相信现在技术上完全可行。只需要扩大规模。

我们应该做吗？

![](img/19f6361ace5a36f3d964f8ddab6e018f.png)

[ssstwitter.com_1715504870631.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KofPbyOvYojKAqxmnA8cescCn8c)

### 谷歌在Hugging Face发布TimesFM权重

贡献人：

https://x.com/GoogleAI/status/1788972685739114946

我们很高兴在Hugging Face上发布我们的时间序列基础模型（TimesFM）的权重！

要访问，请访问我们的HuggingFace（https://huggingface.co/google/timesfm-1.0-200m）和GitHub（https://github.com/google-research/timesfm）仓库。

---

TimesFM是一个预测模型，预先在一个包含1000亿实际时间点的大型时间序列语料库上进行训练，它在不同领域和细粒度的多个公共基准测试中展示了令人印象深刻的零样本表现。了解更多 → https://goo.gle/480VRlm

![](img/8f1b6065697824962ac1193e70a10af2.png)

### Yohei Nakajima: BabyAGI、人工智能代理与人工智能投资

贡献人：

https://x.com/OfficialLoganK/status/1789330831321772420

未来将充满人工智能代理，但关于如何到达那里以及那个世界将会是什么样子，仍然有许多未解之谜... 🤖

我与Yohei Nakajima坐下来讨论了这个话题，这次对话非常有启发性，希望你喜欢：

Yohei Nakajima: BabyAGI、人工智能代理与人工智能投资 | 围绕提示符 #2

![](img/cd55a923eb5ce201fe538753fc925744.png)

### Chip Huyen：人们没有花足够的时间评估他们的评估流程

贡献人：

https://x.com/chipro/status/1788972359900389475

我看到的一个关于人工智能系统的大问题是，人们没有花足够的时间评估他们的评估流程。

1.  大多数团队使用多于一个的指标（通常是3-7个指标）来评估他们的应用程序，这是一个好的做法。然而，很少有人测量这些指标之间的相关性。

如果两个指标完全相关，你可能不需要两者都有。如果两个指标彼此强烈不同意，这要么揭示了你的系统的一些重要信息，要么就是你的指标不可靠。

1.  许多人（我估计60 - 70%？）使用人工智能评估人工智能的回答，常见的标准包括简洁性、相关性、连贯性、忠实性等。我发现将人工智能作为评判者非常有前景，并期待将来看到更多这样的方法。

人工智能作为评判者的得分并不像分类F1分数或准确性那样是确定的。它们依赖于评判者的模型、评判者的提示以及使用案例。许多人工智能评判者表现出色，但也有许多表现不佳。

然而，很少有人进行实验来评估他们的人工智能评判者。好的回答是否得到了更好的评分？评分的可复制性如何——如果你问评判者两次，你会得到相同的分数吗？评判者的提示是否最佳？有些人甚至不知道他们的应用程序正在使用什么样的提示，因为他们使用的是评估工具或其他团队创建的提示。

还有一个昨天从一个（小规模）民意调查中学到的有趣事实：有些团队在评估回应上的花费比生成回应的花费还要多 🤯

![](img/04c83e43685be5fcd038315b36b6d04f.png)

### Shunyu Yao分享博士论文答辩：《语言代理：从下一个词预测到数字自动化》

贡献人：

https://x.com/ShunyuYao12/status/1789058769982550031

我已经成功答辩了我的博士论文！

《语言代理：从下一个词预测到数字自动化》

+   [演讲（WebShop、SWE-benc](https://youtube.com/watch?v=zwfE6J2BIR4)h、ReAct、ToT、CoALA以及关于代理未来的讨论）：
https://youtube.com/watch?v=zwfE6J2BIR4

+   [论](https://ysymyth.github.io/papers/Dissertation-finalized.pdf)文（内容更加全面）：
https://ysymyth.github.io/papers/Dissertation-finalized.pdf

![](img/a443c9a734116d0e437832c594c7194e.png)

### 谷歌深度学习 Chollet：深度学习模型不能解决训练分布之外的任务，“涌现学习”是不正确的

贡献人：

https://x.com/fchollet/status/1789082647375090040

即使是相对资深的机器学习从业者也常常未能理解的一点是，深度学习模型是对数据分布进行的曲线拟合。你不能期望它们解决训练分布之外的任务（这种任务需要智能）。

“涌现学习”是一个不正确的标签——如果一个模型在它未经训练的任务A上表现出性能，这仅仅意味着A与你训练的所有数据之间有显著的重叠。能力并不是凭空出现的。

![](img/c3f39349867995c16c439cee5be89161.png)

# 产品

### Wanderboat——你的私人定制旅行伙伴

贡献人：

https://wanderboat.ai/

Wanderboat 提供个性化的智能旅行服务。它可以根据用户的需求和喜好，量身定制景点、美食和活动等，并全天候提供旅行建议和支持。Wanderboat 被称为用户的 "24/7旅行伙伴" ，让每一次旅行都能如您所愿，充满独特的体验。通过这项服务，用户可以轻松规划和定制属于自己独特风格的旅行路线，快用它来策划您的下一次出游吧。

![](img/c0105aa7f5f8835243017d403191e4c7.png)

### Otto——记录生活，高速成长

贡献人：

https://www.landing.ottowrites.co/

Otto 是一款帮助用户记录，保存个人故事的产品。用户只需通过语音讲述自己的经历、教训和回忆，Otto就会自动生成文字整理成章节，在短短30天内就能完成一本个人传记。这种简单自然的交互方式，让记录生活变得轻而易举，让用户能够保存，并分享自己的故事，或了解亲友的过去经历。

![](img/e79a2b88d08ca2215ae7433b429cf0cf.png)

### FaceSwap

贡献人：

https://faceswap.so/

Face Swap Solution Online 是一款基于 AI 技术的在线人脸互换平台，为用户提供了一种创新有趣的内容创作方式。它允许用户轻松地将多个人的脸部特征合成到同一张图片或视频中，产生逼真动人的视觉效果，无论是创作幽默场景、复刻电影桥段还是构建虚构场景，都能大大增加内容的吸引力。

![](img/e6b0ac3eea66b4a92f1e3060fc43f9a3.png)

# HuggingFace&Github

### Markdowner

贡献人：

https://github.com/Dhravya/markdowner

Markdowner 是一款免费开源的工具，它能够将任何网站快速地转换为就绪的 Markdown 格式数据，为基于 LLM 的应用程序提供结构化且易于查询的内容。它具有自动抓取网页、使用 LLM 过滤无用信息、提供详细 Markdown 输出等功能，可方便开发者部署到自己的基础设施上使用，为构建 AI 应用程序提供有价值的数据支持。

![](img/0343b0fdcfe38b5a4e682eb44e90513b.png)

### **Crawl4AI**

贡献人：

https://github.com/unclecode/crawl4ai

Crawl4AI 是一个强大、免费的网页爬取服务，旨在从网页中提取有用的信息，并以 LLM 和 AI 应用程序友好的格式呈现，包括 JSON、清洁 HTML 和 Markdown。它支持高效的多 URL 爬取、媒体标签替换和灵活的配置选项，是开发基于网页数据的 AI 项目的理想工具。

### Rill Flow

贡献人：

https://github.com/weibocom/rill-flow

Rill Flow 是一种高性能、可扩展的分布式工作流编排服务，支持每天执行数千万个任务，任务执行延迟小于100毫秒。它支持异构分布式系统的编排和调度，提供可视化的流程编排和插件访问功能，支持云原生容器部署和云原生函数编排，还可以快速集成 LLM 模型服务，为用户提供灵活、高效的工作流管理能力。Rill Flow 采用开源许可，通过 Docker-Compose 可以快速在本地部署和测试，为开发者提供了一个功能丰富、易用的工作流编排解决方案。

### Agentcloud

贡献人：

https://github.com/rnadigital/agentcloud

Agentcloud 是一个开源平台，可以帮助公司构建和部署私有的 LLM 聊天应用程序，如 ChatGPT。它提供了一个强大的架构，包括一个 Python 后端应用程序 (Agent Backend)、一个基于 Next.js 的 Web 应用程序 (Webapp)以及一个基于 Rust 的矢量数据库代理 (Vector Proxy)。通过 Agentcloud 企业可以快速、安全地与内部数据交互，构建出定制化的聊天机器人和自动化应用程序。

# 学习

### 新兴的AI AgentOps景观：建设者的视角

贡献人：

https://www.prosus.com/news-insights/group-updates/2024/ai-agentops-landscape

Prosus AI 团队分享了其在构建 AI 代理系统方面的经验，强调了代理系统的核心组成部分，包括： 强大的大型语言模型（LLM） 用于理解用户意图和制定行动计划； 工具集成 用于增强 LLM 的能力，如网络搜索、文档检索、代码执行等； 记忆系统 包括长期记忆（数据库）和短期记忆（请求特定信息）；以及 反思与自我批评 能力，使代理能够识别并纠正错误，重新优先级排序。

文章指出，AI 代理系统能够执行多跳任务，如购买运动鞋、分析年报、提供穿戴式设备市场概览等，这些任务远比单轮聊天机器人更为复杂。在编码任务上，带有代理能力的 GPT3.5 甚至能够超越更强大的 GPT-4 模型。

然而，构建有效的 AI 代理系统仍然充满挑战，主要集中在技术成熟度、系统可扩展性以及工具和集成方面。为了克服这些挑战，Prosus 提出了 AgentOps 概念，旨在通过提供一套预构建的工具和能力，简化代理系统的构建和扩展过程。

通过专注于特定任务或行业的代理系统，如数据分析领域的 Toqan 代理，可以提高答案的准确率，从而为用户提供更精确的信息。未来，AI 代理的发展将继续推动技术创新，并在市场研究、数据可视化、在线购物助手等领域得到广泛应用。

![](img/fe88cbfd4e5397b658d9f4a17fcba2d1.png)

### 原理&图解vLLM Automatic Prefix Cache(RadixAttention): 首Token时延优化

贡献人：

https://zhuanlan.zhihu.com/p/693556044?utm_psn=1772536403207225344

文章深入探讨了vLLM中的Automatic Prefix Caching技术，该技术通过Hash RadixAttention算法优化了长系统提示和多轮对话场景下的首Token时延。文章详细分析了Radix Tree的动态变化、vLLM中Prefix Caching的实现、以及如何通过hash码确保缓存块的唯一性和上下文的正确性。此外，还讨论了在Generate阶段处理未知token_ids的策略、Prefix + Generated KV Caching的优势、以及在多轮对话中的应用。最后，文章提供了在vLLM中启用Prefix Caching的方法，并对比了其他相关优化论文，强调了vLLM实现的优越性。

![](img/e18c7f4be5e68d141899e5c6e217e503.png)

### Flash Attention (GPT2 implement)

贡献人;

https://zhuanlan.zhihu.com/p/685695553?utm_psn=1772229392350957568

文章讨论了如何将Flash Attention（一种注意力机制的优化方法）应用于GPT2模型以提高训练速度。作者首先介绍了Flash Attention 2的安装过程，然后详细说明了在GPT2模型的forward方法中如何修改以支持Flash Attention，包括对attention_mask处理的调整以及如何封装_flash_attention_forward方法。文章还探讨了_upad_input函数的实现，以及如何使用flash_attn_varlen_func和pad_input函数处理变长序列。最后，作者提供了一个测试Flash Attention正确性的函数，并分享了初步测试结果，指出当前Flash Attention在GPT2上可能存在问题，因为引入Flash Attention后训练loss与不使用时有较大差异。

![](img/79a9a64e4208809e1fa7f086ed746081.png)

### StarCoder2-Instruct: 完全透明和可自我对齐的代码生成

贡献人：

https://mp.weixin.qq.com/s/g2VvvKCy577XdYQKEmAXzw

StarCoder2-15B-Instruct-v0.1 是一个大型代码生成模型，它通过自我对齐和完全透明的训练流程实现指令微调，无需人类注释或专有LLMs数据。该模型在HumanEval基准测试上得分72.6，超过CodeLlama-70B-Instruct的72.0分。训练过程包括从The Stack v1中提取种子函数、生成多样化代码指令、执行自我验证生成响应，并在沙箱环境中测试。最终，模型使用通过测试的响应进行微调，展现了在自我生成数据上的有效学习能力。该模型的开源性质为代码生成领域提供了新的研究方向。

![](img/b886f51307779d46d39a32ce9ad8083e.png)

### PyTorch加速LLAMA3技术详解

贡献人：

https://deploy-preview-1596--pytorch-dot-org-preview.netlify.app/blog/accelerating-llama3/

本文介绍了如何利用优化的Triton FP8 GEMM（通用矩阵-矩阵乘法）核心TK-GEMM，通过使用SplitK并行化实现了对LLAMA3-70B推理问题规模的加速。对于小批量大小的推理，TK-GEMM在NVIDIA H100 GPU上的表现比基本Triton matmul实现提高了高达1.94倍，比cuBLAS FP8提高了1.87倍，比cuBLAS FP16提高了1.71倍。

在这篇博文中，我们将讨论如何设计一个使用Triton进行FP8推理优化的内核，并对Lama3-70B推理进行调优。我们还会涵盖FP8（8位浮点数），这是由Hopper一代GPU（SM90）支持的新数据类型，以及Triton支持的关键SM90特性，以及我们如何修改并行化以最大化内存吞吐量。

![](img/94c6fbf3b2bc97a0b7eab3fc3cf2cb5a.png)

### [CUDA 学习笔记] GEMM 优化: 双缓冲 (Prefetch) 和 Bank Conflict 解决

贡献人：

https://zhuanlan.zhihu.com/p/696844342?utm_psn=1772215247459176448

文章详细介绍了如何通过双缓冲(Prefetch)和解决Bank Conflict来优化CUDA矩阵乘法内核(GEMM)，以达到接近cuBLAS的性能。双缓冲通过读写分离，利用指令级并行掩盖访存延迟。Bank Conflict的解决涉及对共享内存(SMEM)的访问模式调整，通过warp分片和特定的线程排布避免冲突。文章还讨论了不同优化策略和实现技巧，提供了相关代码示例和性能分析。

![](img/475a679835a38a3e126eadcdafccfc29.png)

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

![](img/bd9ab54217dc422d6c22951b483e63cb.png)