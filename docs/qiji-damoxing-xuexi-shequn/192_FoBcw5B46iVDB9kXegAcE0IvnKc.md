# 奇绩大模型日报（5月 31日）

> 来源：[https://miracleplus.feishu.cn/docx/UoY3du0slo7Ma3xN6gscvByZnfh](https://miracleplus.feishu.cn/docx/UoY3du0slo7Ma3xN6gscvByZnfh)

# 推特

### Karpathy谈评估：从SEAL排名到训练集的泄露

https://x.com/karpathy/status/1795873666481402010

很好,一个与 @lmsysorg 在评估大语言模型方面的有力竞争者加入了对话。大语言模型的评估正在改进,但不久前它们的状态还非常黯淡,定性体验与定量排名往往不一致。这是因为建立良好的评估方法非常困难 - 在特斯拉,我可能有1/3的时间花在数据上,1/3花在评估上,1/3花在其他所有事情上。评估必须全面、有代表性、高质量,并衡量梯度信号(即不能太容易,也不能太难),在定性和定量评估一致之前,有很多细节需要考虑和处理。我推荐的一些有趣的细节可能是Open LLM Leaderboard MMLU的说明:https://github.com/huggingface/blog/blob/main/open-llm-leaderboard-mmlu.md... 另一个不太明显的部分是,任何开放(非私有)的测试数据集不可避免地会泄露到训练集中。这是人们强烈直觉怀疑的事情,也是最近这个GSM1k轰动一时的原因 https://arxiv.org/html/2405.00332 即使大语言模型开发人员尽了最大努力,防止测试集渗透到训练集中(并记住答案)也是困难的。当然,你可以尽最大努力过滤掉完全匹配的内容。你还可以用 n-gram 重叠或类似方法过滤掉近似匹配。但是你如何过滤掉合成数据重写,或与数据相关的在线讨论?一旦我们开始常规训练多模态模型,你如何过滤掉数据的图像/截图?你如何防止开发人员例如对测试集进行向量嵌入,并专门针对在嵌入空间中与测试集高度一致的数据进行训练?最后一个组成部分是,并非所有我们关心的大语言模型任务都可以自动评估(例如总结等),此时你希望让人类参与进来。当你这样做时,你如何控制所涉及的所有变量,例如人们对实际答案、长度、风格的关注程度,以及如何处理拒绝等。总之,良好的评估出人意料地困难,需要大量的工作,但相当重要,所以我很高兴看到更多的组织加入努力,做好这项工作。

![](img/3a807066f78678fb094a131be664ce67.png)

### Perplexity Pages：将您的研究转化为视觉吸引人的文章

https://x.com/perplexity_ai/status/1796203494401040846

我们很兴奋地推出了 Perplexity Pages,这是一种将您的研究转化为视觉吸引人的文章的简单方法。通过格式化的图像和章节,Pages 让您可以分享关于任何主题的深入知识。现在 Pro 用户可以使用,并将很快向所有用户推广!

![](img/a2b4a53935250e30335ed8824115f8a0.png)

[ssstwitter.com_1717090452129.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OzCVbnriko2ZF5xt5nMch0ypn0g)

### Daniel Han分享英伟达研究报告笔记：4nm的研究推理芯片、指数尾数等

https://x.com/danielhanchen/status/1796253349932843214

我从NVIDIA研究报告中的笔记:

1.  NVIDIA有一个4nm的研究推理芯片,每瓦特可达96 int4 TOPs,而Blackwell的芯片每瓦特可达20 TOPs。

1.  B200的float4是指数(exponent)为2,尾数(mantissa)为2?也许我听错了?我以为指数+尾数=3。

1.  加速不是来自摩尔定律(提升3倍),而是来自更小的数值表示。从fp32到f4可提升32倍。但最近的LLMs物理学论文表明,int4的效果差2倍,这限制了加速的上限。1.58bit不会有帮助。

1.  Tensor Cores / 复杂指令HMMA的速度提高了13倍,能耗更低。

1.  NVIDIA正在研究从2:4稀疏性到2:8稀疏性?

演讲链接:https://youtu.be/gofI47kfD28?si=41UIMkpMCyb_qWqA

LLMs物理学论文:https://arxiv.org/abs/2404.05405

![](img/c59e2f8e69af190fa95684b0c8f23711.png)

### 非常好模型，使我的大脑旋转：通过Era3D创造3D肖像

https://x.com/xxlong0/status/1796229904843030836

只需一张图片,使用我们强大的Era3D模型创建您的3D肖像。@ylecun

Era3D是一个新的高分辨率跨域扩散模型,可以生成512x512的彩色图像和法线贴图(Wonder3D和Syncdreamer只支持256x256分辨率)。有了这样的2D表示,就可以创建如此详细的3D重建。非常感谢@pengli_hkust的努力。

重建部分无法集成到Huggingface演示中。访问我们的Github仓库以获得完整体验。

Github仓库:https://github.com/pengHTYX/Era3D

网页:https://penghtyx.github.io/Era3D/

用于MV颜色和法线预测的Huggingface演示:https://huggingface.co/spaces/pengHTYX/Era3D_MV_demo

![](img/21bc2d11e18b2feca44f78bdd1357d49.png)

[ssstwitter.com_1717138543284.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H1h1b3FYGoVeBYx2ClycADSPnQg)

### Suno v3.5上线，每个人都可以一次生成完整歌曲；LeCun：我超爱

https://x.com/suno_ai_/status/1796273804991156326

v3.5现已向所有用户开放!🥳

现在每个人都可以:

•制作4分钟的歌曲。您现在可以一次生成完整的歌曲了!

•创建最长2分钟的歌曲延伸部分

•体验改进的歌曲结构和人声流畅度

您的反馈对我们很重要,有助于我们随时间改进Suno。所以请继续给我们反馈,对你喜欢的歌曲点赞👍,对你不喜欢的歌曲点踩👎。

❤️ Suno团队

---

LeCun：我能说我超爱Suno吗?我最喜欢的一些作品:

狗狗狗狗狗狗狗狗汪汪 https://suno.com/song/1783c864-18fb-440f-bc51-15701a19e4b5

化学元素 https://suno.com/song/5f324463-08a7-490e-b9c5-f8e2d399baa9

train_gpt2.c 头文件(谁做的哈哈) https://suno.com/song/2a210337-62fc-49f8-8850-9af12e06e6e0

Suno教程(用Suno写的!):

https://suno.com/song/d960e84a-1b03-46a2-999e-2a896a56bd57

还有很多其他的。太棒了。还有人有最爱的作品吗?

![](img/e3e2bbc1b98fb7fc64056d54936257df.png)

# 资讯

### AI内容创作开卷，为什么百度文库成为超强玩家？

https://mp.weixin.qq.com/s/zsJvXcrjxVU6T2kc9NK0_Q

大语言模型发展至今，对各行各业的赋能程度越来越深，一些代表性产品正在改变甚至颠覆着整个行业。 简单回顾便能发现，ChatGPT引领了一场文本创作革命、Stable Diffusion 等文生图大模型降低了绘画门槛、视频大模型 Sora 令影视从业者感到恐慌、音乐大模型 Suno引发了一场音乐圈地震…… 我们能够感受到 AI 大模型重塑内容创作行业尤其是跨模态生成的巨大潜力。如今，AI 大模型能力的释放，不仅可以将用户需求转化为成熟产品，也能让经典产品展现出前所未有的活力。在 5 月 30 日举行的 2024 百度移动生态万象大会上，我们从百度搜索、百度文库、百度 APP 等产品基于文心大模型发布的各项行业领先新功能中，看到了全新的产品形态。本届万象大会还是国内首个智能体生态大会，百度希望更多用户、创作者和生态伙伴加入并共同构建低门槛、多场景、高价值的智能体生态圈，探索智能体赋能产品、场景和行业的新思路。

![](img/b82b45a231a4cc4e37d7770ba174f6fa.png)

### Suno3.5版本模型已向所有人开放 可制作4分钟歌曲

https://www.pingwest.com/w/295428

据 Suno 官方消息，:Suno的最新3.5版本模型已经向所有人开放，这个版本的改进包括:可以制作4分钟的歌曲，创建最长2分钟的歌曲扩展，以及显著改进的歌曲结构。

![](img/bd9a447d67f4a963fe04c0378e2cf6a9.png)

### 爆火ChatTTS突破开源语音天花板，3天斩获9k的Star量

https://mp.weixin.qq.com/s/O6a5LdjH-2LGR5tep3uUeg

近日，一个名为 ChatTTS 文本转语音项目爆火出圈，引来大家极大的关注。短短三天时间，在 GitHub 上已经斩获了 9.2 k 的 Star 量。作者本人也在 x 上表示，ChatTTS 突破了开源天花板。不过，目前开源的只是底模，没有经过 SFT 监督微调。

![](img/6dd8df57ee52f79d161690a75b774f1c.png)

### Claude 上线 ToolUse功能，可与外部数据和工具交互

https://www.anthropic.com/news/tool-use-ga

人工智能公司 Anthropic 宣布AI助手 Claude 的 Tool Use 功能已全面上线。该功能使 Claude 能够自主与外部数据源、API 和工具进行交互，以改变企业利用人工智能实现任务自动化、个性化推荐和简化数据分析的方式。对于使用 Anthropic Messages APl、Amazon Bedrock 和 Google Vertex Al的开发人员来说，Tool Use 现已在整个Claude3 模型系列中可用。

![](img/5c95788e3cf6f706c52696a5b7868762.png)

### 解决Transformer根本缺陷，CoPE论文爆火：所有大模型都能获得巨大改进

https://mp.weixin.qq.com/s/JxB6JU6MxO3709mkg7penw

LeCun 宣传的这篇论文题目为《 Contextual Position Encoding: Learning to Count What’s Important 》，来自 Meta 的 FAIR。这篇论文的重要性不言而喻。短短 24 小时之内就成为了 AI 领域最热门的论文之一。它有望解决如今大模型（LLM）最让人头疼的问题。总的来说，该研究提出了一种新的用于 transformer 的位置编码方法 CoPE（全称 Contextual Position Encoding），解决了标准 transformer 无法解决的计数和复制任务。传统的位置编码方法通常基于 token 位置，而 CoPE 允许模型根据内容和上下文来选择性地编码位置。CoPE 使得模型能更好地处理需要对输入数据结构和语义内容进行精细理解的任务。文章通过多个实验展示了 CoPE 在处理选择性复制、计数任务以及语言和编码任务中相对于传统方法的优越性，尤其是在处理分布外数据和需要高泛化能力的任务上表现出更强的性能。CoPE 为大型语言模型提供了一种更为高效和灵活的位置编码方式，拓宽了模型在自然语言处理领域的应用范围。有网友表示，CoPE 的出现改变了在 LLM 中进行位置编码的游戏规则，此后，研究者能够在一个句子中精确定位特定的单词、名词或句子，这一研究非常令人兴奋。

![](img/cf029634fb26d997b4b907320cdd8994.png)

### SOTA性能，多尺度学习，中山大学提出蛋白质-药物相互作用AI框架

https://mp.weixin.qq.com/s/wmE9wr-IMDK96IujuHgWPA

蛋白质、药物和其他生物分子之间的相互作用，在各种生物过程中发挥着至关重要的作用。了解这些相互作用对于破译生物学过程背后的分子机制和开发新的治疗策略至关重要。当前的多尺度计算方法，常常过于依赖于单一尺度，而对其他尺度的拟合不足，这可能与多尺度学习的不平多尺度衡性和固有的贪婪性有关。为了缓解优化不平衡，中山大学和上海交通大学的研究人员提出了一种基于变量期望最大化的多尺度表示学习框架 MUSE，它可以有效地整合多尺度信息进行学习。该策略通过相互监督和迭代优化，有效融合原子结构和分子网络尺度之间的多尺度信息。MUSE 不仅在分子相互作用（蛋白质-蛋白质、药物-蛋白质和药物-药物）任务方面优于当前最先进的模型，而且在原子结构尺度的蛋白质界面预测方面也优于当前最先进的模型。更重要的是，多尺度学习框架可扩展到其他尺度的计算药物发现。

![](img/1ccd99816003b03d5d1d18a9575123e3.png)

### 抗衡英伟达 NVLink！微软、谷歌、Meta 以及 AMD 等硅谷巨头联手成立 UALink 制定统一标准

https://mp.weixin.qq.com/s/nx0xzqowbeSR-VT06x7dzw

许多人认为 Nvidia 的 AI 系统之所以占据主导地位，只是因为 GPU 或 CUDA 软件，其实 Nvidia 还拥有一系列技术，可用于在多个 GPU 和系统上扩展工作负载，其中包括其片上和封装互连、用于服务器或 POD 中 GPU 到 GPU 通信的 NVLink、用于扩展POD 之外的 Infiniband以及用于连接到更广泛基础设施的以太网。如今，微软、谷歌以及 Meta 等硅谷科技巨头正在成立新的行业组织 UALink（Ultra Accelerator Link）小组以推动数据中心 AI 加速器芯片的组件开发，通过开放标准对 Nvidia 进行反击，其他成员还包括 AMD、英特尔、惠普企业（HPE）、博通以及思科。

![](img/968d12fc62c87928788e29974ef22d4b.png)

# 产品

### oh！a potato

贡献人：

https://ohapotato.app/

OH，a potato! 是一款 AI 驱动的零浪费餐单规划应用程序，由 Radu Lupu 和 Dalma Szabo 开发。该应用旨在帮助用户减少家庭食物浪费，主要功能包括：自动保存和提取各种来源的食谱、根据现有食材生成食谱建议、制定和共享餐单、生成购物清单等。这款应用力图解决人们在制定和执行餐单计划时容易造成食物浪费的问题，为用户提供一站式的零浪费餐单管理解决方案。

![](img/ba1e6f42396557ad2a3d4fcaaa341c30.png)

### AI Notebook

贡献人：

https://ainotebook.app/

AI Notebook App是一款智能笔记应用，基于人工智能技术，可以无缝地组织保存用户在手机上的各种内容，包括文本、图像、音频和视频，并提供实时转录、内容摘要、问答等功能，帮助用户更有效地管理信息，增强移动学习和工作的效率。该应用还计划后续增加语音操作和离线访问等新功能，为用户打造一个智能"第二大脑"。

![](img/b7d3466bbd82bf6fd3c30fddffe245f9.png)

# HuggingFace&Github

### V-Express

贡献人：

https://github.com/tencent-ailab/V-Express

V-Express 是一种创新的肖像视频生成方法，通过渐进式的 dropout 操作来平衡不同强度的控制信号(如音频、姿态和原始图像)，解决了弱信号被强信号所掩盖的问题，并提供了多种生成策略，如无翻转、固定人脸和偏移翻转等，可根据需求调整不同输入条件的权重，生成逼真自然的说话视频，为视频编辑、虚拟主播等应用提供了帮助，同时发布了可直接使用的代码和预训练模型。

![](img/b3476cf4182352a014e33f059795ad1c.png)

### **MusePose**

贡献人：

https://github.com/TMElyralab/MusePose

MusePose 是一个基于姿势的虚拟人类视频生成框架，可以根据给定的姿势序列，生成目标人物在参考图像中的舞蹈视频。它采用了扩散模型技术，生成效果超越了当前大多数开源模型。

![](img/fe18648c50775c1cd68f83f12fca196a.png)

[334049449-bb52ca3e-8a5c-405a-8575-7ab42abca248.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Q8yxbNsrjozFJbxHtmwcKnVpnmf)

### FinRobot

贡献人：

https://github.com/AI4Finance-Foundation/FinRobot

FinRobot 是一个开源的、综合性的 AI 代理平台，专门针对金融领域设计，旨在利用大型语言模型作为"大脑"搭建智能 AI 代理系统，通过多层次的架构实现金融数据分析、市场预测、交易决策等功能，并配备智能调度器优化代理的性能与集成，为金融行业提供全面、灵活的AI解决方案。

![](img/210aa667945ebbc6dffe79f8a5fee6d5.png)

# 投融资

### 光轮智能获数千万Pre-A轮融资，加速合成数据商业化

贡献人：

https://news.pedaily.cn/202405/534797.shtml

光轮智能近日宣布获得数千万人民币Pre-A轮融资，由经纬创投领投，奇绩创坛和辰韬资本跟投。此轮融资将用于加速合成数据产品研发、扩大高端人才队伍，并支持公司业务扩展。光轮智能成立于2023年，专注于为自动驾驶和具身智能等领域提供高质量合成数据解决方案。创始团队在生成式AI与仿真技术领域具有丰富经验，已与多家国内外主机厂和自动驾驶公司签约合作。

![](img/ddc1ad7372a387bfadea5060ab1fe1fc.png)

### 瑞士EthonAI融资1650万美元，推动AI制造技术发展

贡献人：

https://techcrunch.com/2024/05/30/ai-manufacturing-startup-funding-is-on-a-tear-as-switzerlands-ethonai-raises-16-5m/

瑞士AI制造初创公司EthonAI宣布获得1650万美元A轮融资，由Index Ventures领投，General Catalyst、Earlybird和Founderful参与。EthonAI成立于2021年，专注于通过AI模型识别制造过程中的产品缺陷，并整合公司制造数据，提供性能优化解决方案。其客户包括西门子和瑞士巧克力制造商Lindt。融资将用于加速产品研发和市场扩展，助力制造企业利用AI提升运营效率。

公司官网：https://ethon.ai/

![](img/eb58c9120d2dd4b64583f23d5c69fa3a.png)

### 投资Maven AGI：用AI代理重塑客户支持

贡献人：

https://www.luxcapital.com/news/our-investment-in-maven-agi-reimagining-customer-support-with-ai-agents

Lux Capital宣布对Maven AGI投资2800万美元，投资方包括M13、E14、Mentors Fund及来自OpenAI、Google、HubSpot和Stripe的高管。Maven AGI旨在通过AI代理技术，彻底改造客户支持体验。其平台能够处理多种数据形式并理解用户意图，已在TripAdvisor等企业取得显著成果，支持工单解决率超90%，成本降低81%。Maven AGI团队由经验丰富的二次创业者组成，包括HubSpot前全球客户成功与策略副总裁Jonathan Corbin、Stripe前应用机器学习负责人Eugene Mann以及资深工程领导者Sami Shalabi。

公司官网：https://www.mavenagi.com/

![](img/b4e9afac0c5ea04673c05d55b877fea5.png)

### Exactly.ai获430万美元融资，助力艺术家通过AI扩大创作规模

贡献人：

https://techcrunch.com/2024/05/30/exactly-ai-secures-4m-to-help-artists-use-ai-to-scale-up-their-output/

伦敦初创公司Exactly.ai宣布获得430万美元种子轮融资，由Speedinvest领投，InReach Ventures、Cornerstone VC、GuruDev Capital及多名天使投资人参投。Exactly.ai成立于2022年，致力于利用生成式AI帮助艺术家保留艺术作品的法律所有权，并快速大规模复制其设计。该平台已有4万名注册用户，并声称能够通过订阅费用让艺术家的收入增长四倍。融资将用于进一步发展其技术和扩大市场份额。

公司官网：https://exactly.ai/

![](img/81595baee94e6fbd96d0fe10db60b824.png)

### 有鹿机器人获超1亿元融资，引领具身智能商业化

贡献人：

https://mp.weixin.qq.com/s/kg_UvigzocGF15TnXDcm9Q

有鹿机器人近日披露获得创新工场、元璟资本、百度风投等超1亿元融资，并签订超千万元订单用于智能化升级清洁设备。公司推出的具身智能大模型LPLM-10B和Master 2000通用具身大脑，通过软硬件结合，实现高效跨模态、跨场景智能化。此次融资和订单标志着具身智能从实验室走向商业化，预计在万亿级市场中前景广阔。

公司官网：https://www.udeercloud.com/ai130

![](img/20ea3f824fb351546cf0e09a61fb989b.png)

### 合成数据服务商「卓印智能」获千万级天使轮融资

贡献人：

https://mp.weixin.qq.com/s/c3Fd9P0izJK9_DlYmhysbQ

合成数据服务商「卓印智能」宣布获得天使湾创投千万级天使轮融资。卓印智能成立于2022年，专注于通过端到端生成高质量合成数据，已在自动驾驶和机器人领域获得多个头部客户订单。其自研AI训练数据生成模型“Simulaix”帮助企业降低80%-90%的数据成本，并提升模型效果。融资后，卓印智能将继续提升交付能力并推进世界模型“Terra-1”的商业落地验证，目标通过技术创新推动AI行业发展。

![](img/001c291415cf61f503a276e00a641477.png)

# 学习

### 高能力全透明双语大语言模型MAP-Neo完全开源

贡献人：

https://mp.weixin.qq.com/s/hKdufVyzAhxFKFIScT9YQA

1.  数据处理pipeline：提供了预训练数据（包括OCR处理）、去重和基于Spark的分布式数据处理支持。

1.  训练数据：使用4.5T高质量tokens进行训练，提供了详细的预训练语料库和训练代码。

1.  模型架构和训练：包括分词器、基础模型和对齐模型的训练代码，解决了Megatron-LM框架的问题，支持大规模预训练。

1.  模型Checkpoint：发布了中间Checkpoint，确保重现性。

1.  评估和优化：提供了详细的评估代码和优化技巧。

MAP-Neo的开源为LLM研究提供了重要参考，特别是对非英语地区的研究者。

![](img/e978f3a1f0480a89992ea5dd867994de.png)

### 如何高效实现矩阵乘？

贡献人：

https://mp.weixin.qq.com/s/3WgIpmZarlNIWj0rsEH17w

文章主要介绍了如何从 CUDA 初学者的角度出发，高效实现大规模正方形 FP32 矩阵乘的优化过程。首先，明确了目标是实现比 cublas 更快的矩阵乘，并通过理论分析确定合理的矩阵分块与排布方法。性能测试显示，手写的矩阵乘能够达到硬件 95% 的峰值性能。在代码实现方面，详细讨论了朴素的 GEMM 实现、矩阵分块（Tiling）、Thread、Warp 和 Block 级优化。具体到技术细节，提到了使用 shared memory 来减少全局内存交互，以及如何通过向量外积和 double buffer 技术来优化线程级计算。此外，还探讨了如何通过 Warp Tiling 和向量化访存来提高并行计算效率，并避免 bank conflict。在 Block 级优化中，关注了 L2 cache 的命中率和波形（Wave）的影响。最后，作者指出了未来优化方向，包括 split K、长尾问题和分块细调等。整个优化过程强调了理论与实践相结合，并且利用 Nsight Compute 等性能分析工具对优化效果进行了验证。

![](img/1e4ff73f9aac94d186edfe250439a8ca.png)

### 大模型训练优化：论反向传播中序列并行和张量并行的通信计算重叠方法

贡献人：

https://zhuanlan.zhihu.com/p/700639611?utm_psn=1779569304334897152

本文深入探讨了大模型训练中的并行技术，特别是在张量并行和序列并行中实现通信计算重叠的方法。在张量并行中，通过优化 FFN 和 Attention 层的通信计算顺序，减少了内存占用和提高了计算效率。序列并行方面，文章提出了一种适用于 DeepSpeed-Ulysses 的通信计算重叠策略，显著降低了通信开销。此外，文章还讨论了序列并行实践中的挑战，如 Ring-Attention 的优化实现、大规模 Ring-Attention 的通信优化以及 USP+ZeRO3+Offload 的实现问题。作者强调通信计算重叠对于提升大模型训练效率的重要性，并呼吁行业内对序列并行技术进行更多探索和实践。

![](img/0f6407d5809abae06ccfb2e34a3d8eb5.png)

### 我们的AI语音代理论题

贡献人：

https://a16z.com/ai-voice-agents/

网页内容揭示了 AI 语音助手技术的最新发展及其在商业和消费者领域的应用趋势。AI 语音助手利用自然语言处理（NLP）、大型语言模型（LLM）和文本到语音（TTS）技术，能够模拟人类对话，为用户提供服务。随着多模态模型的兴起，技术栈可能会整合，降低延迟和成本，提升用户体验。在 B2B 领域，AI 语音助手正逐步替代传统电话树系统，提供更高效、准确的客户服务解决方案，尤其在特定行业中，通过深度定制化服务，满足复杂的通话需求。而在 B2C 市场，AI 语音助手正开始提供之前需要人类提供的服务，如心理咨询等，并有潜力创造全新的用户互动方式。文章强调，成功的 AI 语音助手产品需要专注于技术细节，如对话流程的精细化处理、情感识别能力的提升，以及针对特定用户场景的定制化开发。这些技术细节的打磨，将是 AI 语音助手在未来市场中取得成功的关键。

![](img/1c66123adaf75421580a00570d4a9761.png)

### Pytorch 显存管理机制与显存占用分析方法

贡献人：

https://zhuanlan.zhihu.com/p/699254132

PyTorch 显存管理机制采用动态申请与二次分配的策略，通过 cudaMalloc 从 GPU 申请显存块（Segment），并在其中划分出子块（Block）进行使用。显存管理器根据 tensor 的大小，将 Block 分配到 large pool 或 small pool，以优化显存使用。PyTorch 提供了 API 如 torch.cuda.memory_allocated 和 torch.cuda.memory_reserved 来监控显存占用。Snapshot 工具可以记录显存消耗的历史和时间线，帮助开发者分析和优化显存使用。nvidia-smi 虽然可以显示 GPU 显存占用，但刷新频率低，不适合实时监控。torch.cuda.mem_get_info 可以查询指定 GPU 的可用显存量。文章最后通过一个全连接网络的训练示例，展示了如何在实际操作中应用这些工具和方法，以及如何通过断言来确保显存使用的正确性和效率。

![](img/59cbb7df241895901a4c01b6c73611f1.png)

### NVIDIA AI 芯片演进解读与推演

贡献人：

https://mp.weixin.qq.com/s/PQL1popiYarfXpiALlhHcw

NVIDIA 的 GB200 超级芯片采用双 Die 设计，具备 20 PFLOPS 的 FP4 算力，与前代 H100 相比，晶体管数量增加了一倍，算力提升了 5 倍。GB200 通过 NVLink、InfiniBand 和 Ethernet 三种网络技术实现互联，其中 NVLink 作为专有技术，演进速度最快，适用于 GPU 直连互联。GB200 NVL72 系统采用背板电缆互联，实现了高效的算力扩展，但面临高功耗和散热挑战。未来 AI 芯片发展可能面临的挑战包括算力提升受限于工艺演进、存储技术的发展与 GPU 演进同步问题，以及机框间互联技术成本和功耗的挑战。SuperPOD 超级节点的商业化需要降低 Rack-to-Rack 互联成本。NVLink、InfiniBand 和 Ethernet 三种网络技术在未来不太可能融合，各自在特定应用场景中具有不同的优势和发展方向。

![](img/1e678a3203fe4ac5a02fb05d0cb4f98a.png)

### 半导体真空零部件

贡献人：

https://mp.weixin.qq.com/s/LaoXBt-RFNJZcAvlWS9MrQ

半导体真空零部件在芯片制造中起着关键作用，它们包括真空泵、真空阀门和真空测量仪器等，确保了芯片生产过程中的高度纯净和精确控制。目前，全球市场规模呈现增长趋势，但技术门槛高，尤其是在芯片制造的关键环节，如极 ultraviolet lithography（EUV）技术中的超高真空要求。国内企业虽然在技术积累和产业链建设方面取得了一定进展，但与国际先进水平仍存在差距。中金公司通过投资支持，助力国内半导体真空零部件企业提升技术实力，推动产业升级和市场布局。未来，随着新材料、新技术的不断涌现和产业链的完善，预计半导体真空零部件领域将迎来新一轮发展机遇。

![](img/cc0b00ff4ca3e259e39dd26fed5baf5c.png)

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

![](img/a7ea9ef0418e649d1334ea4ac0ca11a3.png)