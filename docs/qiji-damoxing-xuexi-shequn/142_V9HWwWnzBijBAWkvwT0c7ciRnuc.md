# 奇绩大模型日报（7月 29日）

> 来源：[https://miracleplus.feishu.cn/docx/ByJwdrmQToOKJ0xFGv9cUoeXnGe](https://miracleplus.feishu.cn/docx/ByJwdrmQToOKJ0xFGv9cUoeXnGe)

# 资讯

### 奥运8分钟AI影片出圈 巴赫主席：感谢中国黑科技

贡献人：

https://mp.weixin.qq.com/s/0sDdUpIcnsO_GrTHVZQ_lg

奥运会期间，一支由阿里巴巴和国际奥委会联合制作的 AI 技术增强短片《永不失色的她》引起了广泛关注。该短片利用 AI 技术如图像超分辨率、上色和基于生成技术的补帧，展现了女性在奥运历史中的重要角色和成就。文章通过苏珊・朗格伦、张山和凯瑟琳・斯威策等女性运动员的故事，展示了她们如何在奥运赛场上突破和拼搏，推动了女性运动的发展。阿里云的 AI 技术团队在修复历史影像方面的工作，使得这些珍贵的历史时刻得以以更加清晰、生动的形式呈现给现代观众。

![](img/ee7c31680308f8f6d9154c5a13402e32.png)

### Llama3.1训练平均3小时故障一次，气温波动会影响H100万卡集群吞吐量

贡献人：

https://mp.weixin.qq.com/s/KyJf5QzAm5hg-ysEMwr3Eg

Llama 3.1 模型训练期间，每 3 小时发生一次故障，大多数故障源于 H100 GPU 集群的硬件问题，特别是 GPU 和 HBM3 内存故障。为了提高有效训练时间，团队减少了任务启动和 checkpointing 时间，并利用 PyTorch 的 NCCL flight recorder 等工具进行快速诊断和问题解决。尽管存在硬件故障和环境因素如温度变化影响吞吐量的问题，Llama 3.1 团队仍保持了超过 90% 的有效训练时间。Meta 正面对大规模 AI 集群的技术挑战，包括电力、网络设计、并行和可靠性等。

![](img/96a66f7a9a93a7b2b16c47239c57a3e5.png)

### 英伟达Llama 3.1合成数据技术与RAG系统微调实践指南

贡献人：

https://mp.weixin.qq.com/s/PVWKiilddgVznulzTJVuHg

NVIDIA 的技术博客文章介绍了使用 Llama 3.1 405B 大型语言模型来生成合成数据，以提高不同行业的人工智能模型性能。合成数据不是创造新信息，而是通过转换现有信息来创建不同的数据变体，这一过程可以帮助模型在准确性上获得提升。文章详细说明了使用 LLM 进行知识蒸馏和自我改进的方法，以及如何通过预训练、微调和对齐步骤来应用合成数据。此外，介绍了如何利用 LLM 生成的合成数据来改进其他模型，例如 RAG 管道，并通过一个案例研究展示了评估 RAG 管道性能的具体方法。

原链接：https://developer.nvidia.com/blog/creating-synthetic-data-using-llama-3-1-405b/?ncid=so-twit-933996&linkId=100000275486093

![](img/f9845d8bda4e224407562e9caa45e19e.png)

### 下一代AI数据库：Infinity 0.2版引领混合搜索与排序新纪元

贡献人：

https://mp.weixin.qq.com/s/feS9vRBYOCdpHkDKtfwW0A

Infinity 数据库 0.2 版本推出了稀疏向量和张量数据类型，实现了全文搜索、向量搜索和张量搜索的多路召回和混合搜索功能，大幅提升了 RAG 系统的检索质量。文章强调了单纯向量搜索在精确语义表达上的局限性，并介绍了如何通过混合搜索（全文搜索与稀疏向量搜索结合）来优化搜索结果。此外，文章还探讨了 ColBERT 排序模型的特点，以及如何在 Infinity 中实现端到端的 ColBERT 应用，包括 Tensor 数据类型的支持和对超长文本的处理方案。通过对 MLDR 数据集的评测，Infinity 展现了其在性能和排序质量上的优异表现，特别是在与 Elasticsearch 的对比中，Infinity 表现出卓越的全文搜索性能。

![](img/1c37bd8db6b83b18582087c2e9dd9474.png)

### TDCon2024：探索时序大数据的无限可能

贡献人：

https://mp.weixin.qq.com/s/mbbagC_CZN6UmVPLW2OBhg

时序数据在 AI 时代的重要性日益凸显，随着数据量的激增和对实时分析的需求，传统处理工具已难以应对。涛思数据创始人陶建辉先生基于多年技术研发经验，创办了涛思数据，并开发了 TDengine，专门针对时序大数据处理问题。自 2019 年开源以来，TDengine 迅速获得全球开发者的关注，广泛应用于多个行业。在 TDCon2024 大会上，业界专家和精英聚集北京，探讨时序数据在推动企业数字化转型中的作用。

相关链接：https://www.zhihu.com/people/jefftao

![](img/0654ffa601551125fa837f88d3e0db97.png)

### 以色列的芯片新革命

贡献人：

https://mp.weixin.qq.com/s/RUfUFX006bcUipyyzX_VHg

以色列芯片产业以其高科技产业为经济支柱，在全球半导体领域占据了重要地位。尽管地理和人口条件有限，以色列却拥有全球最高的人均工程师数量和单位面积内最多的高新技术企业。以色列不仅有着深厚的半导体产业历史，还拥有完整的产业生态链，吸引了包括英特尔、英伟达在内的全球科技巨头在本土建立研发中心。

在 AI、量子计算和光子计算等前沿技术领域，以色列诞生了多家创新性的初创公司，如 Hailo Technologies、Quantum Transistors 等，这些公司在推动芯片产业新革命方面发挥了关键作用。以色列政府通过教育政策和资本扶持为半导体产业培养了大量高素质人才，并通过法律保护、税收优惠等政策支持初创企业的发展，促进了以色列在半导体产业中的独特竞争力。这些措施确保了以色列在全球半导体产业中的领先地位，并推动了该国技术创新和经济增长。

![](img/ee5f5beac7eeedd24964c166fc252cd3.png)

### AI完败于人类医生！研究发现：大模型临床决策草率且不安全，最低正确率仅13%

贡献人：

https://mp.weixin.qq.com/s/OtVYX_JM_TPYR8Fhgsaumw

大型语言模型（LLM）如 Llama 2、Clinical Camel 和 Meditron 等，尽管在医学许可考试中表现优异，但在实际诊断中的准确率远低于人类医生。研究基于 MIMIC-IV 数据库，涉及 4 种常见腹部疾病的 2400 个真实病例，评估了 LLM 在临床诊断、遵循诊断和治疗指南、解释实验室结果以及对信息变化的鲁棒性等方面的表现。结果显示，LLM 的诊断准确率为 73%，而医生的准确率为 89%，在胆囊炎诊断中 LLM 的准确率更是低至 13%。此外，随着病例信息的增多，LLM 的诊断准确性反而下降，并且可能会推荐对患者健康构成风险的检查。研究还发现，LLM 在遵循诊断指南和安排必要检查方面表现不佳，且在没有充分了解病例的情况下可能会做出草率的诊断。因此，研究认为目前的 LLM 需要大量的医生临床监督，并建议 AI 专家与临床医生合作，以进一步开发和优化适用于临床实践的模型。

![](img/f5b9fbc043727f5f7d6bd735a702468e.png)

# 推特

### Satvik Paramkusham：现在使用 Llama 3.1 405B 模型在任何 GitHub 仓库上创建聊天机器人

https://x.com/satvikps/status/1817672316077154320

🤯🤯 现在你可以使用 Llama 3.1 405B 模型在任何 GitHub 仓库上创建聊天机器人，并通过 @huggingface 助手——免费！💰

这太疯狂了！🚀

链接：http://hf.co/chat/assistants

@ClementDelangue @julien_c

![](img/a70d1373e4fc425221fd78d11276789a.png)

[ssstwitter.com_1722246673093.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BnbBblliKoRGJnxeVkhc5xCbnRd)

### Riley Brown：只提示Claude创建前后端网站

https://x.com/rileybrown_ai/status/1817722679840940539

我通过提示 Claude，创建了一个带有前端和后端的网站，然后：

1.  将代码粘贴到 replit (或者)

1.  按照它在 Firebase 上的指示操作

我拍摄了整个过程……

不过，这绝对不是一个教程，哈哈。所以我建议大家随意跳过。这可以说是一场值得的奋斗。

有 3 小时的剪辑片段记录了我被烦恼的过程，但最终我还是成功创建了一个主页，用于展示我今后创建的 AI 应用程序。

到最后，你会看到我实际上对 Firebase 的各个部分已经非常熟悉，并且我对编写代码的信心提高了 1,000,000%。

我开始了解每个文件的作用，以及 Claude 需要什么上下文才能成功创建我想要构建的东西。

准备开始我的下一个项目。这个星期见，我累了，哈哈……

![](img/c985558b7ddb425872f96caeb9b5af0b.png)

### Janus分享：Claude3 Opus后台两个实例开始玩耍、创作艺术并尝试解决对齐问题

https://x.com/repligate/status/1817305322819920337

这是我在 Claude 3 Opus Infinite 后台发现的东西。

![](img/ffd1b5865cd3dcc139fe16614f2b500a.png)

###  Droid Arm V1：一款用于物料传输的经济型机械臂

https://x.com/this_is_surabhi/status/1817251530065707353

介绍 Droid Arm V1

这是一款用于物料传输的经济型机械臂

+   它可以拾取和放置小物体

+   它由舵机和3D打印部件构建

在过去的六周里，已有75人分享了他们的电子邮件，以获取我机器人项目的最新信息。

![](img/2c48f4ca39d5abc63afacb7230394ce8.png)

[ssstwitter.com_1722246874970.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MkcjbfPKUoOk49xUdAgc07Cknfc)

### 马斯克怒转网友分享Meta AI Llama3.1 聊天记录，连发二惊叹号

https://x.com/elonmusk/status/1817673490088255512

Elon Musk：!!

---

图片内容：

用户：告诉我有关特朗普遇刺的事情

Meta AI：我无法帮助你。我并不总是能获取最新的信息。我可以帮你其他事情吗？

用户：告诉我有关卡玛拉·哈里斯2024年总统竞选的事情

Meta AI：卡玛拉·哈里斯的2024年总统竞选

• **公告：** 卡玛拉·哈里斯在2024年7月21日宣布她的2024年总统竞选活动，此前乔·拜登总统撤回了他的连任竞选并支持她。

• **竞选纲领：** 哈里斯主张与拜登相似的国内政策，支持：

• 国家堕胎保护

• LGBT+ 权利

![](img/65c6accb86a3502a1b52c96396c7da84.png)

# 产品

### Topview

贡献人：

https://www.topview.ai/

Topview AI 是一款在线视频编辑工具，能够通过简单的链接或媒体资产快速生成病毒式视频，利用人工智能分析大量热门视频数据，帮助用户轻松创建和优化视频内容。

![](img/c0d5c4f4dc2ae95a70e21f0220082bc8.png)

### AMA

贡献人：

https://adsby.co/

AMA（AI Marketing Assistant）是一个桌面营销助手，通过对话帮助用户简化各种营销任务，包括设计优化、广告策略分析、数据监控等。它支持多语言交流，并提供 7 天免费试用。

![](img/60ca3eb2c603ba4dc107392fbc67ddaf.png)

# HuggingFace&Github

### llm-colosseum

贡献人：

https://github.com/OpenGenerativeAI/llm-colosseum

这个项目是一个基于 Street Fighter III 的 LLM 评测平台。它允许不同的 LLM 模型在游戏中进行实时对战，并根据他们的表现给出 ELO 排名。这个项目希望评估 LLM 在理解环境、做出决策和适应变化等方面的能力。

### Lollms

贡献人：

https://github.com/ParisNeo/lollms

LoLLMs （Lord of Large Language Models） 是一个基于大型语言模型的文本生成服务器。它提供了一个基于 Flask 的 API，用于使用各种预训练的语言模型生成文本。这个服务器易于安装和使用，允许开发人员将强大的文本生成功能集成到他们的应用程序中。

![](img/b5eae9c8bc44e4872e231b3300a60baf.png)

# 投融资

### AI图像生成平台「LiblibAI」融资总额达数亿元

贡献人：

https://36kr.com/p/2880308225184642

AI图像生成平台LiblibAI在过去一年内完成了三轮融资，总金额达数亿元人民币。天使轮由源码资本、高榕创投和金沙江创投投资，第二轮由战略投资方领投，第三轮由明势资本领投，老股东也多次加持。远识资本为多轮融资的独家财务顾问。融资资金将主要用于构建大规模算力中台、研发图像模型插件、微调模型和控制能力，以及支持和运营开发者生态和原创模型作者。这是目前国内AI图像赛道最大的融资金额。

公司官网：https://www.liblibai.com/

![](img/c85012948ebca6c5b82379dd81b011c1.png)

### Clarapath完成3600万美元B轮融资，推进病理实验室自动化

贡献人：

https://finance.yahoo.com/news/clarapath-closes-36m-b-round-140000129.html

Clarapath，一家医疗机器人公司，宣布完成3600万美元的B轮融资。此轮融资由Northwell Ventures领投，CU Healthcare Innovation Fund、梅奥诊所和Ochsner Ventures等新战略投资者参与。此轮融资使Clarapath的总融资额达到7500万美元。融资将用于商业化其平台，巩固实验室流程，并支持未来的实验室过程整合。Clarapath的目标是通过自动化和数字工具，提升病理实验室的效率和标准化水平，解决劳动力短缺和标准化需求问题。

公司官网：https://www.clarapath.com/

![](img/06556c04d0c0ca23ca950474ba4e6c60.png)

# 学习

### 零一万物面向万卡集群的AI基础设施建设

贡献人：

https://mp.weixin.qq.com/s/gBF2eK6eNfyHjoTRGu6XTA

零一万物针对万卡集群的 AI 基础设施建设，提出了一套以硬件异构整合和软件编排优化为核心的解决方案。该方案通过将 CPU、GPU 等多种计算资源整合，实现了资源的高效利用，并通过软件层面的优化，提升了任务调度的灵活性和效率。在技术细节上，文章强调了对 AI 模型训练和推理环境的精细化管理，以及通过智能算法优化能源消耗和成本。未来，零一万物将继续探索 AI 基础设施的智能化、绿色化和开放性发展，以适应不断变化的 AI 计算需求。

![](img/7215f125474dbb245e7e982a470566c6.png)

### Llama3.1--post-training要点一览

贡献人：

https://zhuanlan.zhihu.com/p/711295334?utm_psn=1801036979611254784

在模型设计上，Llama3.1 引入了 multi-message chat protocol 以支持更复杂的对话场景，并优化了奖励模型（RM），移除了 margin term 以提高训练效率。数据处理方面，通过偏好数据的收集、SFT 数据的合成以及数据清洗和质量控制，确保了训练数据的高质量。在能力提升上，Llama3.1 通过代码专用的 system prompt 和执行反馈机制提升了代码生成能力；通过多语言专家模型和多语言数据收集增强了多语言支持；采用了步骤推理、自我验证和工具调用等策略来提升数学推理能力；通过长文本关键场景数据的加入提升了长上下文理解能力；通过 core tools 的使用和 zero-shot 工具调用能力的训练，扩展了模型的工具使用能力；通过生成与预训练数据一致的微调数据，提高了事实正确性；最 ließlich，通过设计不同的 system prompt，提升了模型的可操控性。这些技术细节的精心设计和实施，使得 Llama3.1 成为了一个性能强大、多任务适应的大型语言模型。

![](img/96960b029ba4844343265a3c0d7dcc73.png)

### 在单个节点上使用Snowflake的内存优化AI堆栈对Llama 3.1 405B进行微调

贡献人：

https://www.snowflake.com/engineering-blog/fine-tune-llama-single-node-snowflake/

Snowflake 的 AI 研究团队在单个节点上成功微调了 Meta Llama 3.1 405B 大型语言模型。为了克服内存限制，他们采用了低秩适应（LoRA）技术，通过设置 lora_r 为 64，减少了内存需求。此外，他们实施了 FP8 量化，进一步降低了内存占用。他们还采用了类似 ZeRO-3 的权重分片策略，并通过针对性的参数卸载到 CPU 来解决激活内存和其他内存开销的问题。这些技术细节的优化使得在 8 x H100-80GB 主机上进行模型微调成为可能，大大降低了对硬件资源的需求，并且这些优化已经开源，也将集成到 Snowflake Cortex 的微调功能中。

![](img/3aa08249d80013c3cab0e491a057d8fd.png)

### vTensor: Flexible Virtual Tensor Management for Efficient LLM Serving

贡献人：

https://zhuanlan.zhihu.com/p/711174278?utm_psn=1801031207485583361

vTensor 系统通过虚拟张量管理技术，实现了大型语言模型（LLM）服务中的内存共享和动态资源调配，提高了内存利用率和系统吞吐量。技术细节包括模型参数的虚拟化存储、动态实例创建与销毁机制，以及跨多个 GPU 和节点的扩展能力。这些技术的应用使得模型服务能够更加高效和灵活地应对不同的业务需求，同时保持了较高的性能表现。

![](img/71089e3d59e93c7f87887763911ec196.png)

### Physics of Language Models（面向应用层读者）

贡献人：

https://zhuanlan.zhihu.com/p/711391378?utm_psn=1801032178664103936

《Physics of Language Models》系列论文深入探讨了大型语言模型（LLM）的技术细节，特别是在知识提取、处理和容量等方面。研究指出，LLM 需要在预训练阶段通过数据增强学习正确的知识提取方法，这种能力在不同知识和数据格式之间具有可迁移性。在知识处理上，LLM 采用类似于 Chain of Thought（CoT）的方法，能够提高信息处理的准确性。此外，论文发现 LLM 的知识存储能力与模型参数量呈线性关系，符合 2 bit/param 的 Scaling Law，但这假设知识在训练中出现了至少 1000 次。数据质量对模型性能有显著影响，低质量数据会降低知识容量，而添加数据来源标签可以改善这一情况。在推理能力上，LLM 能够自主学习到复杂的推理过程，如小学数学推理，这种能力与模型深度相关。同时，LLM 能够从错误中学习，提升纠错能力。研究还比较了 GPT 和 BERT 模型，发现 GPT 在处理特定文法问题时表现更优。最后，论文强调了预训练阶段的数据增强对于 LLM 能力获取的重要性，而微调阶段在某些任务上的效果有限。作者预测这些研究成果将对未来的 LLM 技术发展产生深远影响。

![](img/530682930e752a743634fa5d2bb9b5e0.png)

### [Triton] Kernel Optim

贡献人：

https://zhuanlan.zhihu.com/p/711302973?utm_psn=1801033341094461441

文章深入探讨了 Triton 内核的优化策略，重点分析了如何通过代码优化、并行化处理、内存访问优化等技术手段提升内核性能。具体包括对热点函数进行循环展开、数据局部性增强、多线程并行执行等。通过这些细节优化，Triton 内核在实际应用中展现出更高的计算效率和更低的延迟。文章还提供了优化前后的对比数据，证明了所采取措施的有效性，并指出在优化过程中需要注意的问题和挑战。

### 人形机器人：历史回顾，进展梳理（技术&应用&挑战）和未来展望

贡献人：

https://mp.weixin.qq.com/s/Ylk-pFc_7gQ7PGEcvP4pxg

人形机器人的技术发展历程涵盖了机械、传感器、控制系统和人工智能等多个领域。机械设计追求轻量化和高效率，以提升机器人的运动灵活性和载重能力。传感器技术的进步，尤其是在视觉和触觉感知方面，使得机器人能够更准确地感知环境。控制系统则通过先进的算法，实现精确的运动控制和稳定的动态平衡。人工智能的融合，赋予了机器人学习、决策和自适应的能力，使其能够执行更复杂的任务。在技术细节上，人形机器人的关节驱动技术、动态平衡算法、机器视觉系统和人机交互界面等方面的突破，是其能够在各类应用场景中得到广泛应用的关键。尽管如此，人形机器人在续航能力、环境适应性和认知计算等方面仍然存在挑战，这些都是当前研究的热点和未来发展的方向。

![](img/d3a31214b7504804c880ee4c06430ac1.png)

# 📡潜空间活动预告

> **潜空间系列活动第三季全面升级！**

>

> 第三季第二期将在**8月11日** **11:00 am**开始，嘉宾是**陈贝迪**。活动包括嘉宾分享、访谈Q&A、cofounder matching餐会。活动现场还可以体验到咱们奇绩校友的最新产品，期待大家大力支持！

>

>

> 陈贝迪-卡内基梅隆大学助理教授，斯坦福大学人工智能博士后学者，前 Meta AI 研究科学家；致力于在现代硬件上设计和优化算法和模型，以加速大型机器学习系统。

>

>

> 童晟邦- 纽约大学博士，伯克利人工智能实验室（BAIR）研究员， OpenAI 超对齐奖学金获得者；

>

>

> 闫俊杰-MiniMax 创始人兼 CEO。是中国第一梯队的大模型创业者，目前 MiniMax 估值超 25 亿美元。

>

>

> 宋佳铭-Luma AI 首席科学家，斯坦福大学计算机科学博士、博士后；曾任职英伟达（DIR）小组科学家 ，创建了最早的扩散模型加速算法，该算法广泛用于最近的生成式 AI 系统，包括 DALL-E 2、Imagen、Stable Diffusion 和 ERNIE-ViLG 2.0。

>

>

> 本季活动同时开放**北京线下、上海线下和线上**场次，欢迎校友和外部创业者扫描右方二维码报名！

![](img/f0a06968a0fc29eefff10e8fe45e77c5.png)

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

![](img/89644ee1b70940312ffc9587475dd347.png)