# 奇绩大模型日报（11月 5日）

> 来源：[https://miracleplus.feishu.cn/docx/PAHMd02sAooVAKxq9VocA5jDnOt](https://miracleplus.feishu.cn/docx/PAHMd02sAooVAKxq9VocA5jDnOt)

# 🔉潜空间活动报名

> 本期活动将在**11月9日** **10:00**开始，我们邀请到的嘉宾是**鱼哲**，Le**pton AI 创始成员，曾在阿里云担任高性能 AI** 平台产品负责人，专注于 AI 在多个行业的落地及应用。Lepton AI 致力于建立高效可用的AI 基础设施，让团队更关注于应用构建及落地。在本次分享中鱼哲将带来关于AI产品相关的思考，分享主题《Beyond Infra, what matters?—— 不同AI产品形态对团队的挑战》。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 GenAI 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/4be20474134f51fb89a92e1cab1b91e9.png)

# 资讯

### 腾讯3D大模型全面开源，文本图像10秒转3D资产，模型权重、推理代码全开放

https://mp.weixin.qq.com/s/2qWYraxSWo3xQMNp2ODpgg

3D生成开源界首个**同时支持文字、图像转3D**的模型来了，效果还是SOTA级别。

就在刚刚，腾讯宣布推出Hunyuan3D-1.0，一口气开源轻量版和标准版两个模型。

最快**10秒**就能端到端生成。

先生成6个多视角图像，再进行多视角重建，“啪”的一下360度无死角的3D资产就造出来了。

![](img/722dacc7f5854611601b16461e6ef083.png)

### 2024科学智能峰会在海淀举办

https://mp.weixin.qq.com/s/ait29Uun7FMy7GMr3DQSyQ

2024科学智能峰会于11月4日在北京海淀举行，吸引了多个领域的专家分享AI在科学研究和产业应用中的最新进展，特别是在物理、化学、材料研究和生命科学等领域的突破。论坛深入探讨了AI如何改造研究模式，提升研发效率和准确性。例如，北京大学张锦院士强调，AI可以通过自动化平台和开源模型，加速材料研究，特别是碳材料的催化剂发现和性能定制，缩短研发周期。复旦大学龚新高院士则讨论了AI在生命科学中的应用，提出需要通过构建跨层次的生物学大模型来发现新的科学原理和生物技术。

论坛还突出了AI基础设施的重要性，北京科学智能研究院张林峰院长提出，AI不仅推动科学研究的进展，还需要强大的基础设施支持，如算力和数据资源的优化配置，才能助力产业应用的广泛落地。在这一背景下，海淀区通过政策创新，为AI企业提供了支持，特别是在算力建设和大模型开发方面，逐步形成了一个产业集群。

海淀区近年来实施了多项政策，如对大模型算力的补贴计划，最高可提供1000万元资金支持。这些政策目标是推动AI技术在各行业的深度应用，推动产业升级，并力争到2026年成为全球具身智能和AI创新的核心基地。海淀区的战略包括建设AI产业高地，吸引顶尖企业和科研机构，如百度和旷视，汇聚了大量AI人才和科研资源，形成了强大的创新生态。

![](img/de631ac854cfedbb1aa1e73cf0a6d5af.png)

### ControlNet作者新作：AI打光玩得更溜了！细节保留能力远高于SD1.5

https://mp.weixin.qq.com/s/p43XIv_dveRzJ8rQne_jAg

IC-Light V2是“敏神”（张吕敏）开发的图像处理工具，旨在通过AI技术精确控制图像中的光照效果。相比于之前版本，IC-Light V2在训练方法上进行了改进，采用了基于FLUX的架构，拥有16通道VAE和原生高分辨率。这使得它能够在保持细节的同时，进行更自然的光照调节。

例如，用户输入一张照片并提供提示词“flicker 2008 photo of a woman, neon light, city, cars and people in the background”后，生成的效果几乎没有违和感，细节保存得非常好。IC-Light V2在人物细节保留和光照效果上比SD1.5更为出色。

此外，IC-Light V2在处理带有艺术风格的图片时也表现出色。与基于SD1.5的版本不同，IC-Light V2能够更好地保留风格细节，例如处理超现实主义风格的图像时，AI能够更准确地生成符合提示的风格，而不会忽视风格要求。

目前，IC-Light V2已经上线HuggingFace平台，用户可以在线体验其效果。该工具目前包括一个前景条件模型，强调保留输入图像的细节。未来还将推出更多版本，包括支持更大修改的前景模型、集成背景和HDRI的模型等。

尽管当前模型仅在HuggingFace提供体验，但敏神表示，未来将发布更多的推理代码和权重。然而，与V1版本不同，新版本的许可证仅限于非商业用途。

![](img/41ea349cfd9526f8c1e70ed3c1118c1a.png)

### 无需参数访问！CMU用大模型自动优化视觉语言提示词

https://mp.weixin.qq.com/s/CRwLHAWKHSTEe8flVLKUsA

卡内基梅隆大学（CMU）研究团队提出了一种创新的“黑盒优化”策略，旨在通过大语言模型（如ChatGPT）自动调整提示词，优化视觉语言模型（如DALL-E 3、GPT-4o）的表现。这一方法无需触及模型内部的参数或特征嵌入，大大提高了优化的灵活性与速度，适合没有技术背景的用户进行模型性能提升。

传统的提示词优化依赖人工经验，且需根据模型反馈反复调整，费时费力。例如，OpenAI曾花费一年时间优化CLIP模型的提示词模板。而CMU团队的黑盒优化方法，通过自动化流程将正负反馈输入大语言模型，以更高效地优化提示词。优化过程类似“爬山法”，通过多轮迭代逐步改进提示词，最终得到最佳表现的提示词。

实验结果表明，CMU团队的方法在多个视觉识别任务中，取得了超越传统白盒优化的表现，并且无需人类工程师参与。在不同模型架构（如ResNet和ViT）之间，优化得到的提示词能够有效泛化，提升模型表现。例如，在食物识别任务中，ChatGPT自动调整提示词为“多样化的美食和原料”，显著提高了识别准确性。

该方法也在文本到图像生成（T2I）任务中表现出色，能够通过自动优化提示词生成更符合用户需求的高质量图像。同时，提示反演（Prompt Inversion）技术也得到了应用，帮助用户通过图像生成反向推测出合适的文本提示词。

CMU团队的黑盒优化方法打破了传统模型调优的限制，不仅适用于图像分类和生成任务，还展现了广泛的应用潜力。通过“文本梯度”优化，无需访问模型内部参数，具备强大的扩展性。这一策略可能在实时监控、自动驾驶、智能医疗等复杂场景中带来更高效的解决方案。

研究团队包括刘士弘（Shihong Liu）、林之秋（Zhiqiu Lin）和Deva Ramanan教授等，他们的工作已被CVPR 2024接收，并展示了黑盒优化在多模态模型调优中的应用前景。

![](img/091b2922c435c5758dca01fbba8c8809.png)

### Meta前硬件负责人Caitlin Kalinowski加盟OpenAI，领导机器人和消费硬件领域

https://techcrunch.com/2024/11/04/metas-former-hardware-lead-for-orion-is-joining-openai/

Caitlin Kalinowski，前Meta增强现实眼镜项目负责人，近日宣布加入OpenAI，担任机器人和消费硬件的负责人。Kalinowski在Meta的工作包括领导Orion增强现实原型的开发，以及领导虚拟现实眼镜硬件团队近九年。在此之前，她曾在苹果公司设计MacBook硬件。

Kalinowski在LinkedIn上表示：“我非常高兴地宣布加入OpenAI，领导机器人和消费硬件工作。在我的新角色中，我将首先聚焦OpenAI的机器人项目和合作伙伴关系，推动AI技术进入物理世界，释放其对人类的益处。”

她的加入可能与她曾在苹果工作的旧上司Jony Ive密切相关。Ive近期确认与OpenAI和其设计公司LoveFrom共同开发一款AI硬件产品，旨在提供比iPhone更少社会干扰的计算体验。

此外，OpenAI近期也在招聘研究工程师，重启其机器人团队，专注于帮助合作伙伴将OpenAI的多模态AI集成到硬件中。OpenAI此前曾在2018年开发了一款能够独立学习抓取物体的机器人手，之后将硬件研究转向软件。

目前，已有多家公司将OpenAI的模型应用于硬件领域，包括苹果公司将推出与iPhone的ChatGPT集成，以及机器人公司Figure，其人形机器人01已利用OpenAI的软件进行自然语言对话。

![](img/a8b81001e9ed5f9ea936ac6ede63a43e.png)

# 推特

### xAI API公开上线：每月25美元免费额度，128k 令牌上下文

贡献人：

https://x.com/xai/status/1853505214181232828

xAI 的 API 已上线！- 立即试用 @ http://console.x.ai

• 128k 令牌上下文

• 支持函数调用

• 支持自定义系统提示词

• 兼容 OpenAI & Anthropic SDKs

• 直到年底，每月免费获得 $25 的额度

http://x.ai/api

---

您现在每月拥有 $25 的免费 API 额度，直到年底。

如果您已经购买了额度，您将获得相应数量的额外免费额度。

![](img/b31744b1b4dcfb9214c0631ca4eaab93.png)

### Claude 3.5 Haiku上线：迄今为止最快、最智能且具成本效益的模型

贡献人：

https://x.com/alexalbert__/status/1853498517094072783

Claude 3.5 Haiku 现已在 Anthropic API、Amazon Bedrock 和 Google Cloud 的 Vertex AI 上线。

Claude 3.5 Haiku 是我们迄今为止最快、最智能且具成本效益的模型。

![](img/95c0474aadbd0882d06ee17bcb43eed8.png)

### 全新Hume App推出：引入全新助手，结合语音语言模型 EVI 2 生成的声音和个性

贡献人：

https://x.com/hume_ai/status/1853540362599719025

全新 Hume App 重磅推出

引入了全新的助手，这些助手结合了我们的语音语言模型 EVI 2 生成的声音和个性，并整合了新款 Claude 3.5 Haiku 等补充性大型语言模型和工具（来自 @AnthropicAI）。

![](img/709364a970ca4a15a2b778f777c7b0f0.png)

[xChTmoI10LErVyRu.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KjdGb6OmFoU4OdxpH54cFeuMnQP)

### KOL线上求助AI代理，评论区立马构建：自动化 AI 系统，用于分析 AI 案例研究

贡献人：

https://x.com/arvidkahl/status/1853264865520976347

我想运行 AI 代理来抓取特定 URL 并进行数据提取，直到找到某种特定信息。就像一个 AI 调查员一样。

有哪些框架可以实现这种尖端功能？有没有我们应该使用的 AI 代理项目？

---

我已经为你构建好了！

这是一个自动化 AI 系统，用于分析 AI 案例研究（可以更换使用场景），以识别和记录企业级 AI 实施。

它从读取 CSV 文件中的 URL 开始，使用网页抓取（通过 WebLoader 或 Firecrawl）从每个案例研究中提取内容。

提取的内容会发送给 Claude 3.5 Sonnet，基于特定标准（如公司成熟度、实施规模和可衡量的业务成果）分析该案例是否是真正的企业级 AI 实施。

对于每个 URL，系统首先保存原始内容，然后执行初步的资格分析。

如果 Claude 判断案例符合企业级 AI 实施的标准，系统将生成详细分析，包括三种报告类型：

• 个别案例报告，包含执行摘要、AI 战略分析和业务影响评估等部分

• 多个案例的跨案例分析，识别模式和趋势

• 执行仪表板，汇总关键指标和见解

所有这些报告以结构化格式保存（个别报告为 markdown，跨案例分析和仪表板为 JSON）在各自的目录中。

如果某个案例不符合企业级 AI 实施的标准，系统会记录原因并转向下一个 URL。

整个过程是异步的，并在终端提供关于进展和决策的详细反馈。

![](img/1fd5b12442fc2d69b8937c05e7c45165.png)

[CyvaSWmndlsrWPii.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IS0xbAGPgosiOYxmWSFcUb3SnIg)

### OuteTTS-0.1-350M - 零样本语音克隆，基于 LLaMa 架构，CC-BY 许可

贡献人：

https://x.com/reach_vb/status/1853475883706614232

Smol TTS 模型来了！OuteTTS-0.1-350M - 零样本语音克隆，基于 LLaMa 架构，CC-BY 许可！🔥

纯语言建模方法实现 TTS

零样本语音克隆

LLaMa 架构结合音频令牌（WavTokenizer）

亮点：支持 llama.cpp 实现设备端运行 ⚡

TTS 三步法：

使用 WavTokenizer 进行音频标记化（每秒 75 个令牌）

CTC 强制对齐，实现文字到音频令牌的映射

使用转录、时长和音频令牌创建结构化提示词

对于仅 350M 参数的模型表现令人印象深刻！向 @OuteAI 团队致敬，这一杰出成果令人赞叹——期待它能应用于更大的数据和更智能的骨干模型，比如 SmolLM 🤗

![](img/3bfd0ba10784691cbd5b2e588d60bf07.png)

[4Voqi3zA5DWM_omK.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UUxhbtz7goLhNQxSEI8cSwCDnQc)

### BLENDERGPT：生成 3D 资产并无缝导入 Blender 的最快方式，文本转 3D 仅需约 20 秒

贡献人：

https://x.com/gd3kr/status/1853645054721606100

推出 BLENDERGPT —— 生成 3D 资产并无缝导入 Blender 的最快方式。

文本转 3D 仅需约 20 秒。

访问 blendergpt dot org。

![](img/0fb2076f3518fa3b6b41fa44b773b0e2.png)

[OBAKN1vOv9S-a8Hd.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UGiRb2hiIoVVuFxajbUc7mrMnYc)

# 信号

### Deep Learning Through A Telescoping Lens: A Simple Model Provides Empirical Insights On Grokking, Gradient Boosting & Beyond

推荐人：

推文链接：[h](https://x.com/Jeffaresalan/status/1853460948440240213)ttps://x.com/Jeffaresalan/status/1853460948440240213

原文链接：https://arxiv.org/abs/2411.00247

信号源：Ziming Liu

研究节点：模型体系-架构体系

这篇文章探讨了深度学习模型在应用中的一些意外表现，并旨在通过一个简单而准确的模型来深入理解这些现象。研究者们构建了一个基于一系列一阶近似的模型，将其作为实用分析的工具，并通过三个案例研究展示了该模型的有效性。该模型帮助揭示了多个著名现象的新的实证见解，如双下降现象、grokking、线性模式连通性以及在表格数据上应用深度学习的挑战。

文章强调，这种模型能够构建和提取度量标准，帮助预测和理解神经网络在预期之外的性能表现。此外，该模型提供了一种教育性的形式，使研究人员能够在复杂的现代环境中隔离训练过程的不同组件，从而更好地推理设计选择（如架构和优化策略）的影响。

最后，研究表明，深度学习模型的学习过程与梯度提升方法之间存在惊人的相似性，这为理解和改善深度学习模型的性能提供了新的视角。整体而言，文章为研究深度学习模型的复杂行为提供了重要的理论框架和实证支持。

![](img/9cbec350ff475c9f22190adf9d2dc1af.png)

### Multi-expert Prompting Improves Reliability, Safety, and Usefulness of Large Language Models

推荐人：

推文链接：[https://x.com/omarsar0/status](https://x.com/omarsar0/status/1853286452227899851)/1853286452227899851

原文链接：https://arxiv.org/abs/2411.00492

信号源：Elvis

研究节点：开发体系-Prompting优化

这篇文章介绍了“多专家提示”（Multi-expert Prompting），这是一种对“专家提示”（ExpertPrompting）的新改进，旨在提升大型语言模型（LLM）的生成能力。该方法通过模拟多个专家来引导LLM执行输入指令，聚合各个专家的响应，并在个体和聚合响应中选择最佳答案。整个过程通过七个精心设计的子任务完成，这些子任务源自于知名的决策制定框架——名义小组技术（Nominal Group Technique）。

研究评估显示，多专家提示在提升响应的真实度、事实性、信息量和实用性方面显著优于专家提示及其他比较基线，同时减少了有害性和冒犯性。尤其是在使用ChatGPT时，该方法在真实度方面的表现超过了最佳基线8.69%，显示了其卓越的性能。

此外，多专家提示方法不仅高效且具有可解释性，且能够适应多种场景，消除了手动构建提示的需求。这使得该方法在实际应用中更具灵活性，能够为用户提供更高质量的输出。

![](img/c2f58ad8824a0687705437275f532165.png)

### Centaur: a foundation model of human cognition

推荐人：

推文链接：[h](https://x.com/IntuitMachine/status/1853331246211858676)ttps://x.com/IntuitMachine/status/1853331246211858676

原文链接：[https://osf.io/prep](https://osf.io/preprints/psyarxiv/d6jeb)rints/psyarxiv/d6jeb

信号源：Carlos E. Perez

研究节点：模型体系-架构体系

这篇文章提出了一种名为“Centaur”的计算模型，旨在模拟和预测人类在各种实验中的行为。研究人员通过在一个名为Psych-101的大规模数据集上微调现有的语言模型创建了Centaur。该数据集包含超过60000名参与者在160项实验中所做的超过1000万次选择，使得模型可以在多种实验情境下进行行为模拟。

与现有的认知模型相比，Centaur在预测未参与训练的受试者的行为方面表现更佳。此外，Centaur还具有很强的泛化能力，能够适应不同的实验结构、故事情节以及全新的任务领域。研究还发现，通过微调，该模型的内部表示与人类神经活动的相似度进一步提高。

综合来看，Centaur被视为首个能够提供统一认知理论的模型，有望对认知科学产生颠覆性影响。这一模型的成功展示了在认知建模领域中采用先进计算技术的潜力，并对传统的计算模型开发范式提出了挑战。

![](img/a5b734e60ea1d977a3af2d4b7308626a.png)

# 产品

### Daytona

贡献人：

https://openhands.daytona.io/

Daytona是一个提供无缝的云端开发体验的平台，特别是在人工智能编码方面。它允许用户快速访问和使用AI工具，简化开发过程，消除了传统开发中的等待名单和复杂设置。

OpenHands在Daytona平台上推出，无需等待和复杂设置的AI编码工具，用户可以立即访问。该工具允许用户修改和调试代码、浏览网络、调用API、运行终端命令，并从StackOverflow复制代码片段。

![](img/9715bc5b4da9d3747c094bee259cbdac.png)

### FullContext

贡献人：

https://www.fullcontext.ai/

FullContext是一个基于人工智能的工具，通过自然语言快速构建市场推广工作流程。它提供互动产品演示、资格审核、会议安排等功能，能够识别访客身份并展示相关资源，从而提升销售效率和客户体验。

![](img/ea7b6a367eb623466cf75e7bb477cf68.png)

# HuggingFace&Github

### screenshot-to-code

贡献人：

https://github.com/abi/screenshot-to-code

这个项目是一个简单的工具，利用人工智能将屏幕截图、原型和Figma设计转换为干净、功能性的代码，支持多种前端技术栈（如HTML + Tailwind、React + Tailwind、Vue + Tailwind）。它可以使用Claude Sonnet 3.5和GPT-4o两种AI模型进行转换，并提供实验性功能，将网站的视频/屏幕录像转换为可运行的原型。

![](img/550d8856241569d1d3a15e920037bbde.png)

### AnythingLLM

贡献人：

https://github.com/Mintplex-Labs/anything-llm?tab=readme-ov-file

AnythingLLM是一个全栈应用程序，允许用户将任何文档、资源或内容转换为上下文，以供任何大型语言模型（LLM）在聊天中参考。该应用支持自定义AI代理、多模态功能和多用户管理（仅限Docker版本），并提供工作区内的代理功能（如浏览网页、运行代码）。它支持多种文档类型（如PDF、TXT、DOCX），具有简单直观的聊天界面，支持拖放和清晰引用。

![](img/1d79fa0bab76da831d977cd1f1b6b94e.png)

# 投融资

### Spot AI推出首个面向物理世界的视频AI代理，融资近1亿美元

https://www.spot.ai/blog/spot-ai-introduces-first-video-ai-agents-for-the-physical-world-as-it-nears-100-million-in-funding-to-date

Spot AI宣布推出其创新的视频AI代理，旨在将数字世界中的代理AI能力带入物理世界。这些视频AI代理能够帮助组织即时识别和解决安全、安防和运营中的事件，提升制造业、教育、零售和汽车服务等行业的投资回报率。同时，Spot AI也成功完成了3100万美元的新一轮融资，使其总融资额达到9300万美元。本轮融资由Qualcomm Ventures领投，Scale Venture Partners、StepStone Group、Redpoint Ventures、Bessemer Venture Partners等现有投资者参与。

Spot AI的最新产品——视频AI代理，将传统的视频监控系统从被动的记录转变为主动的解决方案，能够自动识别问题并触发响应，如警报、数据分析、控制设备等。Spot AI目前服务超过1000家客户，覆盖17个行业，处理的录像数据量是YouTube日常上传量的两倍。借助先进的边缘处理技术，Spot AI的视频AI代理可以实现更强大的计算能力，并将高质量视频数据转化为可操作的资源，推动安全性、生产力和营收的提升。

公司官网：https://www.spot.ai/

![](img/7ad71c2c7cedf244b35aa4e1794a0780.png)

### 月泉仿生获近亿元Pre-A轮融资

https://36kr.com/p/3022212992869895

月泉仿生，一家专注于人形机器人研发和产业化的企业，近期成功完成了近亿元的Pre-A轮融资。本轮融资由洪泰基金领投，长兴基金和中关村启航基金跟投，浪潮资本担任独家财务顾问。资金将用于加速人形机器人研发，强化公司的人才和技术壁垒，并推动商业化进程。

月泉仿生主要从事人形机器人本体、核心零部件及仿生智能装备的研发，凭借其联合创始人任雷教授的自主研发技术，公司已实现全链条自研，包括机器人整机结构、核心件及动力系统。目前，月泉仿生在仿生机器人领域拥有超过300项国内外专利，并推出了仿生拉压体灵巧手、机械臂等多个产品，正在研发新一代人形机器人。

在商业化方面，月泉仿生已取得显著进展，2024年，公司已获得数千万元的新增订单，产品成功应用于多个领域。特别是在能源领域，公司与国家电投建立战略合作，将仿生机器人技术应用于无人机和海上风电作业。同时，公司还与防务科技集团合作，推动特种作业机器人在高危环境中的应用，预期未来将拓展至汽车、航空、科研等多个行业。

### Coatue筹集10亿美元加码AI投资

https://techcrunch.com/2024/11/04/coatue-is-raising-1b-for-ai-bets/

Coatue Management是一家在疫情期间积极投资科技初创企业的对冲基金，目前正计划筹集10亿美元用于AI领域的投资。根据彭博社的报道，这笔资金将主要来自机构投资者，此外，通过Raymond James和Associates经纪公司账户的高净值个人也有机会参与投资。

Coatue目前管理着接近500亿美元的资产，2021年曾投资超过170家风险投资支持的企业。尽管在随后的两年里，Coatue大幅减少了对初创企业的投资步伐，2022年仅支持了81家公司，2023年更是只有30家公司，但在2024年，Coatue已经投资了29家初创公司。近期，Coatue的AI领域投资包括Glean、Scale AI和Skild AI等公司，其中Skild AI正在开发一款通用型AI机器人。Coatue的创始人Philippe Laffont特别看好AI驱动的类人机器人，并对此充满期待。

Coatue的AI投资将进一步推动该基金在这一新兴领域的布局，并为未来的创新技术提供资本支持。

公司官网：https://www.coatue.com/

![](img/e1d60c2e29b270db8b1e8c5b5acfc4e1.png)

# 学习

### DistilQwen2：通义千问大模型的知识蒸馏实践

贡献人:

https://mp.weixin.qq.com/s/cnTUlGOw3F-ytsgWje509g

在当前人工智能的快速发展中，大语言模型（LLM）已成为研究的热点。Qwen系列大模型凭借其强大的语言理解和生成能力，在开源社区引起了广泛关注。然而，随着这些模型应用场景的不断扩大，如何提高其计算效率、降低部署成本成为了亟待解决的难题。知识蒸馏（Knowledge Distillation）作为一种有效的模型压缩技术，通过将大模型的知识转移到小模型中，从而减少计算资源消耗并提升推理速度，成为解决该问题的重要手段。

本研究提出了DistilQwen2，这是基于Qwen2大模型，通过知识蒸馏增强指令遵循能力并减少参数量的小型语言模型。我们通过对Qwen大模型的深度分析、指令数据增强、优化和多种知识蒸馏算法的探索，成功将大模型的复杂知识转移到轻量级模型中，从而提高了指令遵循效果。特别是在资源受限的环境中，如移动设备和边缘计算，轻量级模型显得尤为重要。

DistilQwen2的技术流程包括对指令数据的多样性处理，尤其是任务、长度、语种多样性的优化。数据质量的提升通过使用LLM-as-a-Judge范式进行模型拟合难度评分（MFD Score），并结合自蒸馏方法，进一步提高了蒸馏数据的质量和模型的精度。此外，我们使用SFT（监督微调）和DPO（直接偏好优化）进一步微调学生模型，以更好地拟合教师模型的输出分布。

在效果评测方面，DistilQwen2展示了显著的性能提升。通过在多个指令遵循评测基准（如AlpacaEval 2.0、MT-Bench和IFEval）上的测试，DistilQwen2在1.5B和7B参数规模下均取得了优于原始Qwen2的成绩，尤其在小参数模型上更为明显。此外，DistilQwen2在MMLU、CEval等标准知识评测任务中的表现与Qwen2接近，证明了其在保持高效性的同时，未发生灾难性遗忘。

DistilQwen2模型的开源和下载通过HuggingFace和ModelScope平台进行，用户可以方便地下载并在实际应用中使用。我们提供了详细的示例代码和使用教程，确保开发者能够轻松集成该模型。通过这种方式，DistilQwen2为开发更智能、更高效的自然语言处理应用提供了技术支持，助力更多开发者和企业通过技术创新实现商业价值。

![](img/3c7ee9389d7864a8248dfb13e4e58de6.png)

### Agent 智能体开发框架选型指南

https://mp.weixin.qq.com/s/0jB157hXY2AwIcji0jywBQ

本文通过作者的实践对比发现，框架的选择应基于项目具体需求和团队特点，而不是简单追求某个特定框架。不同框架各有优势：

1.  无框架方案实施最为简单直接，代码结构清晰，适合理解智能体原理，但随着项目复杂度增加可能变得难以维护。

1.  LangGraph提供完整的智能体结构规范，特别适合团队协作和智能体结构新手，但框架限制较多，如不认同其理念可能面临较大调试挑战。

1.  LlamaIndex Workflows采用事件驱动架构，在框架约束和开发自由度之间取得平衡，对框架依赖较少，但其固有的异步特性可能增加某些场景的复杂度。

框架选择需要考虑三个关键因素：项目是否已深度集成了特定框架、团队对智能体架构的熟悉程度、是否有可供参考的相似项目案例。

![](img/d1b1c5d6e89e515b86340550af9d843f.png)

### 百川智能：深度学习大模型推理性能优化策略

https://mp.weixin.qq.com/s/ViSg7THcwMHVp024L1gQuQ

本文探讨了通过四个优化专项提升大模型推理框架性能的技术细节：量化、投机采样、TTFT与TPOT优化、以及通信优化。

1.  量化
量化是通过将模型的参数从浮点数转化为低比特定点数，减少计算强度和内存消耗的技术。量化有助于减少显存占用，降低访存量，并提高计算资源的利用效率。主要优化方法包括：

+   Weight-int8 + KV_cache_int8：将权重和KV缓存量化为int8，显著降低显存占用。

+   Activation int8：对GEMM相关计算的输入激活量化，减少运算耗时，首token时间可下降50%。

+   Weight-int4 + KV_cache-int4：进一步降低显存占用，支持较长序列和更大batch，降低成本30%。

+   通信量化：减少低端卡的通信耗时，提升效率。

+   Attention QKV int8：将Q、K、V的GEMM计算量化为int8，提升计算效率。

1.  投机采样
投机采样通过利用解码过程中的算力冗余，生成多个候选token并行验证，从而提高算力利用率，减少延迟。设计包括：

+   Clover模型：采用前几层信息预测，并通过MLP整合全局信息，提升效果。通过动态构建候选token树，使用贪心搜索策略，提升命中率和推理速度。

+   Clover2模型：在Clover的基础上，增加数据蒸馏loss、提前加入transformer block等，提升全局信息提取能力和后期head预测能力，进一步提高推理效果。

1.  TTFT与TPOT优化

+   TTFT（首个token响应时间）：优化首个token生成速度，尤其在实时交互中，需尽量减少延迟。通过切块式prefill和Split fused技术降低decode间隔时间。

+   TPOT（每个输出token的时间）：通过不同的推理任务分布，优化prefill与decode的平衡。采用PD分离策略，将prefill和decode阶段分开，以充分利用算力和内存，提升吞吐量。

1.  通信优化
通信在大模型推理中占据显著比重，尤其在低性能卡（如4090）上，通信耗时可能会影响计算效率。优化策略包括：

+   计算通信重叠：通过GEMM与通信的重叠，减少通信等待时间，提升GPU算力利用率。

+   优化策略：采用GEMM切块策略、分段计算策略和复杂的计算通信重叠方法，提高整体推理效率。优化后的通信重叠策略可在不同GPU上显著提高性能，4090卡在四卡配置中提升40%。

![](img/6180a76e88e8c653cf8faf1b7e225d49.png)

### 《Graph+AI：大模型浪潮下的图计算》白皮书正式发布

https://mp.weixin.qq.com/s/JmlBy7B8-ZvSqGvK1M-PnA

白皮书聚焦图计算技术与人工智能（AI）技术在大模型时代的融合与创新应用，分析了大模型技术兴起所带来的新机遇与挑战，探讨了图计算与AI如何在数据、算法、应用层面相互补足。白皮书还系统梳理了图数据处理、图神经网络、图基础模型、知识图谱工程、Text2GQL、GraphRAG等图计算与人工智能结合的关键技术，企业智能决策、图安全风控、知识增强生成等图计算与人工智能结合解决方案，以及图计算与人工智能在能源、金融、电商、科学研究等众多领域的结合应用案例，以期为学术界和产业界提供参考。

![](img/1da7b0438fbe14263239f041fddec721.png)

# 招聘公告

---

![](img/10663442721542514b80a19d64501a53.png)

**奇绩创坛****由陆奇博士**于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

## 🚀F**24实习岗位**

##### [行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

##### 行研-GenAI研究岗位-战略分析（偏产品）

##### Web / AI 全栈开发工程师实习生

## **火热开放中🚀**

##### 行研-GenAI研究岗位-前沿学术探索

##### 行研-GenAI研究岗位-战略分析（偏咨询）

##### 创新活动产品运营 （大模型系列活动产品运营）

> **点击**相关岗位可查看对应**岗位需求【JD】；**相关岗位投递请**扫描二维码**

![](img/71234fa839c9294c19cc3f4b4dc9a237.png)

> 欢迎**推荐/自荐**

> 期待在**奇绩创坛**遇见更好的你

---

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

![](img/817c60cdd4a094a97d3189ec3ec2f420.png)