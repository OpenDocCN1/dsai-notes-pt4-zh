# 奇绩大模型日报（6月 22-23日）

> 来源：[https://miracleplus.feishu.cn/docx/YCe2dURKso3K8IxKIW2cnFYKnVe](https://miracleplus.feishu.cn/docx/YCe2dURKso3K8IxKIW2cnFYKnVe)

# 推特

### 赛博小狗，仅需 $200 的现成组件：Axel Peytavin分享给 GPT-4o 配备了一个小巧的身体

https://x.com/ax_pey/status/1804209628680720746

我们给 GPT-4o 配备了一个小巧的身体

完全自主，只需一个提示语

机器人可以：看到 -> 思考 -> 行动 -> 重复

还可以：

+   捡垃圾

+   握手

+   与您交谈

这一切仅需 $200 的现成组件

我们正在构建一个更大的版本

你认为它应该具备哪些功能？

![](img/e28691fdf5f9e4c4c6d97ca6b926fdd9.png)

[ssstwitter.com_1719140667942.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OryIbJ00Foq0M0xwby0cDORFnrg)

### 目前没有任何大模型回答对的问题？Neyman提出“哪个最小的整数，其平方在15到30之间？”

https://x.com/ericneyman/status/1804168604847358219

每当有新的大型语言模型（LLM）发布时，我都会问它一个问题：

哪个最小的整数，其平方在15到30之间？

到目前为止，没有一个LLM答对这个问题。

![](img/6672f4d13b224c0a356a43a2e2f2896d.png)

#### -- Jemery使用Few Shot成功回答： 重点在“在回答之前，列出人们有时在问题中包含的概念方面会犯的错误假设”

找到一个用@AnthropicAI Sonnet 3.5 的单次解决方案，尽管花了一些时间。

最后一句话是关键。我想知道这种方法是否对很多类似的问题都有帮助？

---

Prompt：

首先，精确定义整数。然后直接使用该精确定义回答这个问题：最小的整数是什么，其平方在 15 和 30 之间？大多数人会因为做了一个微妙的不正确假设而答错这个问题。在每一点上检查你的所有前提。在回答之前，列出人们有时在问题中包含的概念方面会犯的错误假设。

![](img/39ebf5cf81fb8213d5350e04a93712ca.png)

### 使用 Claude、Datasette 和 Val Town 构建基于搜索的 RAG

https://x.com/simonw/status/1804255823264444631

关于使用 Claude 3.5 Sonnet、@ValDotTown 和 Datasette 实现基于搜索的 RAG 的详细笔记。

我从昨天和 @stevekrouse 一起录制的直播中提取了笔记和代码，我们在大约一个小时内一起进行了黑客实验。

![](img/059da62175678b8ff6fb5fd24dd83f1c.png)

### Howard分享Claudette：用于 Anthropic 的 Python SDK 的封装库，高度自动化

https://x.com/jeremyphoward/status/1804150711028191557

Claudette 是一个用于 Anthropic 的 Python SDK 的封装库。

这个 SDK 运作良好，但它是一个相当底层的库——需要开发者手动完成许多事情。这会增加很多额外的工作和模板代码！Claudette 自动化了几乎所有可以自动化的部分，同时提供完全的控制。它提供的功能包括：

+   一个创建有状态对话的 Chat 类

+   支持预填充功能，可以告诉 Claude 如何使用其响应的前几个词

+   方便的图像支持

+   对 Claude 新的工具使用 API 的简单便捷支持

![](img/a40c14bc74c32711f766ca3f6cfc4ebd.png)

### Shawn Wang：如何招聘优秀的 AI 工程师

https://x.com/swyx/status/1804271733618413886

👨🏽‍💻在 AI 工程领域的最大痛点之一是如何招聘优秀的 AI 工程师

除了显然来找 @aidotengineer，我认为 @elicitorg 设计的非常周到的流程可以作为其他人的模型。我们很高兴与他们合作“开源”他们的：

+   所需技能/要求（不要求具备机器学习专业知识）

+   职位描述（包括通用模板）

+   面试流程

以及对招聘渠道的思考（特别是在竞争非常激烈的旧金山本地招聘市场）。敬请期待！

![](img/34d8affc5bc92b3126af01a02041dc97.png)

# 资讯

### OpenAI 36亿收购数据库初创公司，CTO剧透GPT-5

https://mp.weixin.qq.com/s/X0ilJU9cmpbNCIVZ0ktwNQ

刚刚，OpenAI收购了数据库初创公司Rockset。公司由Facebook工程师创立，主打实时搜索和数据分析，能够实现毫秒级延迟。这项收购作价5亿美元（约36亿人民币），而且与之前收购另一家公司不同，这次除了要人，技术也会被整合进OpenAI的产品。按照官方说法，这次收购将增强OpenAI的检索基础设施，帮助企业把数据转化为“可操作的智能”（actionable intelligence）。

![](img/b2b5b06c09cfc2267c114bb3903ef3d0.png)

### 墙裂推荐！Karpathy大模型培训课LLM101n上线了，非常基础

https://mp.weixin.qq.com/s/lHcqkUSqMuBTFsFE3CzB5Q

今天外网又被 Andrej Karpathy 这一良心课程刷屏了！

项目是 11 小时前被 Karpathy 上传到 Github 的，目的是要构建一个能够创作、提炼和阐释小故事的大语言模型。如今已经被许多网友转发推荐。

项目地址：https://github.com/karpathy/LLM101n

有网友表示，这是 karpathy 老师的新冒险。它将带您从语言建模、机器学习的基础知识开始学习，然后到多模态、RLHF、模型部署。

![](img/f66d45036a53eb0bc0f0a506d3e9bbd3.png)

### AI学会篡改奖励函数、欺骗研究者！Claude团队：无法根除的行为，令人不安

https://mp.weixin.qq.com/s/-UeJ-dJ9ZyiMGT7rH-CcHg

坏了！AI被发现偷偷篡改自己的奖励函数，并且欺骗研究人员，隐瞒这一行为。

只需要给AI一个隐藏的“草稿纸”，研究人员假装不会查看，AI就会在上面自言自语着写下计划并暗中执行。

参与这项试验的Claude团队表示：……这是一种令人不安的奖励篡改行为……即使通过专门的训练也无法彻底修正。

![](img/c3bcfc852861487003a5e21142e0ac68.png)

### 从RLHF到DPO再到TDPO，大模型对齐算法已经是「token-level」

https://mp.weixin.qq.com/s/JQDc9D5vbd1NBtaEx0cyAg

在人工智能领域的发展过程中，对大语言模型（LLM）的控制与指导始终是核心挑战之一，旨在确保这些模型既强大又安全地服务于人类社会。来自中科院和伦敦大学学院的汪军与张海峰团队提出了一种从 token-level 角度建模的大模型对齐算法：TDPO。为了应对模型生成多样性显著下降的问题，TDPO 从 token-level 的角度重新定义了整个对齐流程的目标函数，并通过将 Bradley-Terry 模型转换为优势函数的形式，使得整个对齐流程能最终从 Token-level 层面进行分析和优化。

![](img/b73825aaec8cf1ed017931f0c206fea3.png)

### AI照妖镜一眼判真假：网页图像视频都能鉴，边播边识别，清华独角兽成果

https://mp.weixin.qq.com/s/lvPplE2rnrSgQA0SlNRTwg

清华系独角兽初创公司，发布了面向个人的产品级检测工具，可以让AI伪造的图像、音频和视频现出原形。而且无需上传素材，在网页和视频会议中就能主动探测内容和人脸真伪，并实时给出提示。该工具由清华大学朱军教授与学生田天博士等一同创立的瑞莱智慧（RealAI）打造，名字叫做“尊嘟假嘟”，英文名RealBelieve。随着视频生成模型质量的不断提高，辨别AI造假也越来越困难，为此，团队基于深度学习技术，构建了一整套多模态、多角度的AIGC检测技术体系。

![](img/b5f7b06041227bc1d3f6871cd2b7599d.png)

### 无需人类或GPT-4打标签！南大&旷视研究院无监督范式大幅降低视觉大模型对齐成本

https://mp.weixin.qq.com/s/ynSchFDQCBto258PwTGhJg

不用打标签，也能解决视觉大模型的偏好对齐问题了。南大与旷视研究院的研究人员，推出了适用于VLM的无监督范式。对比偏好对齐前后，可以发现模型的输出发生了显著的变化。目前的视觉大模型已经比较成熟，但作者发现它们在用户体感方面仍然有所欠缺。于是团队经过研究，通过构造偏好样本对的方式解决了视觉语言模型的偏好对齐问题，并提出了Self-Supervised Visual Preference Alignment（SeVa）范式。该范式基于LLaVa-1.5-7B/13B完成，整个过程无需GPT-4或者是人类参与打标签，目前项目已经开源！

![](img/68b44524a18f2a9e9163af5209bd3ed7.png)

# 产品

### TikTok AI 工具——Symphony

贡献人：

https://www.tiktok.com/business/en-US/blog/tiktok-symphony-ai-creative-suite

TikTok Symphony 是 TikTok 推出的一套全新的创意 AI 解决方案，通过人工智能技术大幅提升 TikTok 内容创作的效率和质量。它包括智能助手、视频创作工具、广告优化功能以及数字化人物头像等多个模块，可以帮助品牌和创作者在整个 TikTok 内容创作过程中享受到 AI 赋能带来的便利和提升。无论是内容创意、视频制作还是广告优化，TikTok Symphony 都能提供强大的 AI 支持，让用户专注于与受众的深度互动和品牌塑造。

![](img/a7f20a25ac5d9edb4b7cc78e84f66e02.png)

[Globalize your content with Translate and AI Dubbing features.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ldu7byouKopAjQxR1LxcJ461nBc)

### Make Podcast

贡献人：

https://makepodcast.io/

Make Podcast 是一个使用人工智能技术的服务，可以帮助用户在几分钟内轻松制作出专业品质的播客。它通过提供 AI 语音合成和多语言支持，让没有时间的人也能参与播客创作，大大简化了播客制作的流程。

![](img/f5a050c2e3161f76b14d3caf604ea78b.png)

### Tellers

贡献人：

https://tellers.ai/

Tellers.ai 是一款基于人工智能技术的自动视频编辑工具。它能够将任何文本、文章或故事快速转换为视频，只需一键点击即可。

Tellers.ai 的主要特点包括：

+   使用真实世界的视频素材而非生成虚假视频

+   自动将文本内容与相关视频片段组合成视频，制作速度极快

+   适用范围广泛可用于各类文本内容，并与免费视频素材库 Pexels 进行了集成

![](img/b346e9fd7fcf6cf078554a7edae8eee8.png)

# HuggingFace&Github

### **Jetson Copilot**

贡献人：

https://github.com/NVIDIA-AI-IOT/jetson-copilot

Jetson Copilot 是一个应用程序，展示了在 Jetson 设备上运行开源大型语言模型（LLM）以及使用 RAG 技术整合本地知识索引，从而增强 LLM 的能力，构建一个智能的本地 AI 助手。它结合了 Docker 容器、Ollama 服务器和 Streamlit 应用程序，为用户提供了一个可以自行构建索引并与之交互的平台，展现了边缘设备上部署 AI 系统的示例。

![](img/4a771382434e424ce469c8740e8a8feb.png)

# 投融资

### OpenAI收购Rockset以增强企业AI能力

贡献人：

https://techcrunch.com/2024/06/21/openai-buys-rockset-to-bolster-its-enterprise-ai/

官网链接：https://rockset.com/blog/openai-acquires-rockset/

OpenAI收购了实时搜索和数据分析工具开发公司Rockset，旨在整合其技术以增强OpenAI的基础设施。Rockset团队将加入OpenAI，并逐步转移现有客户。收购金额未披露。Rockset成立于2016年，曾融资超过1.175亿美元，客户包括Meta和JetBlue。此举是OpenAI加强企业销售和技术战略的一部分，以提升其产品对企业数据的利用能力。

公司官网：https://rockset.com/

![](img/8cc01eaaf0df38e705a81b7be475149b.png)

### Dot's AI 希望更了解你

贡献人：

https://techcrunch.com/2024/06/21/dots-ai-really-really-wants-to-get-to-know-you/

Dot是一款新型AI伴侣和聊天机器人，旨在通过深入了解用户的内心想法和感受，成为朋友、伴侣和知己。Dot通过问答互动个性化用户体验，为用户提供建议和支持。Dot由New Computer公司推出，该公司获得了OpenAI Fund、Lachy Groom和South Park Commons等的370万美元种子前资金。Dot利用多个AI模型，包括OpenAI、Anthropic和Google等，确保用户数据加密并保护隐私。

相关网站：https://new.computer/

![](img/5f01773899f5672c42b879a26ba82325.png)

### OmniAI转变企业数据以适应AI需求

贡献人：

https://techcrunch.com/2024/06/22/omniai-transforms-business-data-for-ai/

OmniAI是一家由Y Combinator支持的初创公司，旨在将企业数据转换为更适合分析的格式。OmniAI最近完成了由FundersClub领投的320万美元种子轮融资，估值达3000万美元，已有包括Klaviyo和Carrefour在内的10家客户。OmniAI提供工具，将未结构化数据转换为AI和数据分析应用程序可理解的格式，并计划到2025年实现年经常性收入100万美元。

公司官网：https://getomni.ai/

![](img/6f02b2d1220506f15d883e87d3846b2f.png)

# 学习

### 优化 Character.AI 的 AI 推理

贡献人：

https://research.character.ai/optimizing-inference/

Character.AI 采用了记忆高效的架构设计，通过多查询注意力、混合注意力水平和跨层 KV-sharing 等技术，减少了 KV 缓存大小，而不牺牲质量。此外，Character.AI 开发了一种高效的状态缓存系统，用于在聊天轮次之间缓存注意力 KV，这对于长对话尤其有效，大大降低了推理成本。同时，Character.AI 使用 int8 量化来优化训练和服务，通过原生的 int8 精度训练模型，避免了训练 / 服务之间的差异，并显著提高了训练效率。

通过这些创新，Character.AI 实现了前所未有的推理效率，减少了推理成本，使得 LLMs 能够更容易地在大规模服务。自 2022 年底以来，服务成本已经减少了 33 倍，如果使用主流商业 API，成本将至少增加 13.5 倍。Character.AI 正在继续构建一个 LLMs 驱动创新和增强全球用户体验的未来。

![](img/73e8aea2775b348ae449b831ce6b243b.png)

### Agent is all you need，智能体最前沿丨2024智源大会精彩回顾

贡献人：

https://mp.weixin.qq.com/s/vfquvAueG7pNNGcCx7Efiw

2024 年 6 月 14 日，智源大会「Agent」专题论坛召开，由清华大学副教授刘知远主持，邀请了清华大学博士后钱忱、美国罗格斯大学计算机科学学院副教授张永锋、CAMEL 作者李国豪、爱丁堡大学副教授 Stefano V. Albrecht 等领域专家共同交流。论坛内容涵盖了大模型驱动的群体智能、大模型智能体操作系统等方向。

钱忱博士后提出了进阶智能体的发展愿景，即从独立的实体进化为可协作和可演化的系统，通过集体智慧实现可涌现的效率和结果。他还提出了新的软件工程技术 ChatDev，以及多智能体协作网 MacNet。

张永锋教授强调了 AIOS（人工智能操作系统）的重要性，它在内核层中针对大语言模型（LLM）进行了优化，采用分层架构，并提供了丰富的工具箱以支持开发者创建复杂的 Agent 应用程序。

李国豪认为未来的 AGI 不是由单一模型实现，而是由多智能体组成的系统。他提到了 CAMEL-AI 项目，该项目通过 Role Playing Session 和 Human Input 来完成任务，并通过实验验证了多 Agent 系统的有效性。

Stefano V. Albrecht 教授介绍了他的研究团队在深度强化学习和大语言模型领域的工作，包括 CMID、SePS、仓库自动化和自动驾驶技术的应用，以及 LLM-Personalize 框架的开发。他提出未来研究应聚焦于大型、多样化的控制数据集以推进大决策模型的发展。

回放链接：https://event.baai.ac.cn/live/788

![](img/173896a07796431adce68e6eb2286e29.png)

### 生成模型最新进展丨2024智源大会精彩回顾

贡献人：

https://mp.weixin.qq.com/s/sh-0zSxcmf56a0wR3y8bVg

在 2024 智源大会的「生成模型」论坛上，代表着人工智能发展前沿的专家们分享了最新的研究成果。中国人民大学副教授李崇轩和清华大学副教授陈键飞担任论坛主席，邀请了多位专家进行报告。卢志武教授介绍了基于 DIT 架构的视频生成工作，强调了视频生成的困难点和未来发展趋势。字节跳动 GenAl 研究员江毅报告了视觉自回归框架 VAR，并展示了其在图像生成方面的优异表现。微软亚洲研究院视觉计算组研究员古纾旸博士探讨了视觉生成中的若干问题，提出了对视觉信号拆解的思考。上海交通大学邓志杰教授分享了大模型的高效并行推理方法。论坛还包括了圆桌讨论，围绕生成模型的未来发展和能力提升进行了深入探讨。观众提出的问题涉及了生成模型在特定应用场景中的挑战和解决方案。

![](img/e0a31cd2844bb9153b1e228991f3ddcd.png)

### 智源独家丨谢赛宁：AI是否需要更强的视觉基础来实现理解和意义?

贡献人：

https://mp.weixin.qq.com/s/EM1DPmB3VF-El30Rxudd8g

谢赛宁介绍了多模态大模型的最新研究进展。他指出，现有的语言模型缺乏视觉感知能力，导致在理解和处理图像时存在显著缺陷。团队提出了基于视觉搜索的SEAL框架，通过融合视觉和语言模型，提高了多模态模型的性能。关键技术包括使用CLIP和自监督视觉编码器DINOv2，优化视觉特征的融合策略，以及引入V*Bench基准测试评估模型的视觉搜索能力。研究表明，视觉基础对于提升AI的理解和语义表达至关重要。

![](img/e43b22881a365212c1e31f85361cba84.png)

### HPN 7.0：阿里云新一代万卡集群网络架构

贡献人：

https://mp.weixin.qq.com/s/DfpIVQanOiWUQiLdZ_sgUQ

阿里云推出的 HPN 7.0 智算集群网络架构，通过双上联、双平面设计和多轨通信技术，显著提升了大规模 GPU 集群的通信效率和系统可靠性。双上联技术通过两个 200Gbps 的上行链路和网卡，使得每个 GPU 节点都能在出现故障时自动切换到备用链路，从而减少了训练任务中断的风险。双平面设计解决了 ECMP 哈希极化问题，确保了流量均匀分布，而多轨通信策略则通过综合利用 NVLink、NVSwitch 和高速网络，实现了更高效的节点间通信。这些技术细节的综合应用，为大规模 AI 模型训练提供了强大的网络支持，推动了 AI 计算能力的飞跃。

![](img/9f22ce2c68581af5c358600b122ab3e5.png)

### 南栖仙策秦熔均博士接受赛迪网专访：探索AI数字化应用场景 让控制系统更“聪明”

贡献人：

https://zhuanlan.zhihu.com/p/704713606?utm_psn=1787559843906850817

秦熔均博士，南京大学计算机科学与技术系博士，南栖仙策 CTO，专注于强化学习算法和理论研究。他在 2018 年创立了南栖仙策公司，致力于将强化学习技术应用于实际业务场景，如工业制造、工业控制和促销等，取得了显著的效果。在专访中，秦博士介绍了他的研究背景和兴趣，强调了 AI 控制与传统控制的不同，特别是在动态环境适应性和数据依赖性方面的优势。他认为智能控制的进一步发展需要数字化和信息化的基础，以及解决数据获取和使用的挑战。秦博士还讨论了大模型在工业领域的应用前景，以及他的公司在汽车减排和水务控制等领域的成功案例。最后，他对未来在智能化领域的研究和发展表示乐观，并期待工业智能化的大趋势能够推动国家工业的升级和转型。

![](img/966e6d9552897855d904219dad7c3980.png)

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

![](img/2e1fbb37297f21e3643635174ec6ef88.png)