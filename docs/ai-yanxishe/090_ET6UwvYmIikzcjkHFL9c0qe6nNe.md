# Sora学习笔记（持续更新版.....)

> 来源：[https://aiyanxishe.feishu.cn/docx/Ay9gdVJeKo7rBbxs1DScgsoon2g](https://aiyanxishe.feishu.cn/docx/Ay9gdVJeKo7rBbxs1DScgsoon2g)

![](img/fefbefa4b48a65ec4098916380a36bec.png)

<https://aiyanxishe.feishu.cn/sync/KGQ3dArdRsnl77bJBG4camxWnAg>

![](img/5ec34837cda1eb1b9fc27c57056d2e02.png)

# 一、Sora介绍

OpenAI2024年2月16日发布了 Sora，一种文字生成视频的技术，效果相当炸裂。能够生成1分钟的视频，目前最强的AI视频生成技术，请看下面的视频：

![](img/3e4e3bf17a237e61c2378b14682214d3.png)

[2月18日.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OYoLbSBGno4hrexX9GVcE9QknWf)

Sora是一个基于大规模训练的文本控制视频生成扩散模型。它能够生成长达 1 分钟的高清视频，涵盖广泛的视觉数据类型和分辨率。Sora通过在视频和图像的压缩潜在空间中训练，将其分解为时空位置补丁，实现了可扩展的视频生成。Sora还展现出一些模拟物理世界和数字世界的能力，如三维一致性和交互，揭示了继续扩大视频生成模型规模来发展高能力模拟器的前景。

## **体验入口**

👉点击前往：OpenAI Sora体验入口

## 需求人群

+   生成扩散模型视频内容

+   提供视觉模能力的应用开发

+   创建和编辑视频内容的工具

## 使用场景示例

+   在视频生成应用中使用Sora生成符合文本提示的内容

+   结合Sora在游戏或模拟器中生成高保真纹理和环境

+   通过Sora实现图像和视频的无缝编辑和过渡

## 产品特色

+   基于文本提示生成高质量视频

+   生成变化分辨率、长度和纵横比的视频

+   基于图像和视频继续生成

+   模拟物理和数字世界的某些行为

## 官方Demo案例

Sora可以模拟真实世界的运动方式，根据提示词生成长达一分钟的视频，同时保证画质和连续性。官方发布视频demo片段均由该模型生成,未经后期编辑和修改。

👉点击前往

## Sora模型弱点

OpenAI表示，**Sora并不是一个完美的模型**，可能会出现空间混淆、物理交互不准确等情况。

👉点击前往

## Sora与其他视频模型PK

在和主流视频生成模型的对比中，Sora在时长和效果上可以说横扫市面上所有的对手。

👉点击前往

## Sora权威信息源

👉信**息源头**

1、Sora官网 (视频样例在这里) https://openai.com/sora

[2](https://openai.com/research/video-generation-models-as-world-simulators)、技术报告(详细解释在这里) https://openai.com/research/video-generation-models-as-world-simulators

[3](https://twitter.com/OpenAI/status/1758192957386342435?s=20)、OpenAl twitter：https://twitter.com/OpenAI/status/1758192957386342435?s=20

4、Sam Altman twitter: https://twitter.com/sama

👉S[ora背后的研发团队](https://faqocxi7k2.feishu.cn/wiki/BWdjw90V7ikENKkPf2WcjigfnKM)

👉Sora的两个核心leader:

Tim Brooks  twitter(https://twitter.com/_tim_brooks)

Bill Pebbles  twitter(https://twitter.com/billpeeb)

两人都是23年从伯克利（Berkeley）毕业的哲学博士（PhD）

## 央视报道Sora

![](img/9421a96ff48deb49e70cff1bec7f58a1.png)

[2月20 央式介绍Sora.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/N9yPbn6h3o5ScaxDpVvcGhxdnif)

## Sora 内测账号申请方式

下面这张图是Sam发的推特，招募Sora红队网络人员进行内测。

给大家讲申请流程之前，废话不多说，我们先解释一下什么是红队。

英文：OpenAI Red Teaming Network

中文：OpenAI 红队网络

**What is the OpenAI **Red Teaming Network?

The term red teaming has been used to encompass a broad range of risk assessment methods for AI systems, including qualitative capability discovery, stress testing of mitigations, automated red teaming using language models, providing feedback on the scale of risk for a particular vulnerability, etc. In order to reduce confusion associated with the term “red team”, help those reading about our methods to better contextualize and understand them, and especially to avoid false assurances, we are working to adopt clearer terminology, as advised in Khlaaf, 2023, however, for simplicity and in order to use language consistent with that we used with our collaborators, we use the term “red team”.

**Red teaming** is an integral part of our iterative deployment process. Over the past few years, our red teaming efforts have grown from a focus on internal adversarial testing at OpenAI, to working with a cohort of external experts to help develop domain specific taxonomies of risk and evaluating possibly harmful capabilities in new systems. You can read more about our prior red teaming efforts, including our past work with external experts, on models such as DALL·E 2 and GPT-4.

Today, we are launching a more formal effort to build on these earlier foundations, and deepen and broaden our collaborations with outside experts in order to make our models safer. Working with individual experts, research institutions, and civil society organizations is an important part of our process. We see this work as a complement to externally specified governance practices, such as third party audits.

The OpenAI Red Teaming Network is a community of trusted and experienced experts that can help to inform our risk assessment and mitigation efforts more broadly, rather than one-off engagements and selection processes prior to major model deployments. Members of the network will be called upon based on their expertise to help red team at various stages of the model and product development lifecycle. Not every member will be involved with each new model or product, and time contributions will be determined with each individual member, which could be as few as 5–10 hours in one year.

Outside of red teaming campaigns commissioned by OpenAI, members will have the opportunity to engage with each other on general red teaming practices and findings. The goal is to enable more diverse and continuous input, and make red teaming a more iterative process. This network complements other collaborative AI safety opportunities including our Researcher Access Program and open-source evaluations.

什么是 OpenAI 红队网络？

**红队**一词已被用来涵盖人工智能系统的广泛风险评估方法，包括定性能力发现、缓解措施的压力测试、使用语言模型的自动化红队、提供特定漏洞风险规模的反馈等为了减少与“红队”一词相关的混淆，帮助那些阅读我们的方法的人更好地结合上下文并理解它们，特别是为了避免错误的保证，我们正在努力采用更清晰的术语，正如 Khlaaf, 2023 中建议的那样，然而，为了简单起见，并且为了使用与我们与合作者使用的语言一致的语言，我们使用术语“红队”。

**红队**是我们迭代部署过程中不可或缺的一部分。在过去的几年里，我们的红队工作已经从专注于 OpenAI 的内部对抗性测试发展到与一群外部专家合作帮助开发特定领域的风险分类法并评估新系统中可能有害的功能。您可以详细了解我们之前的红队工作，包括我们过去与外部专家在 DALL·E 2 和 GPT-4 等模型上的合作。

今天，我们正在发起一项更正式的努力，以这些早期的基础为基础，加深和扩大我们与外部专家的合作，以使我们的模型更安全。与个别专家、研究机构和民间社会组织合作是我们流程的重要组成部分。我们认为这项工作是对外部指定治理实践（例如第三方审计）的补充。

OpenAI 红队网络是一个由值得信赖且经验丰富的专家组成的社区，可以帮助更广泛地为我们的风险评估和缓解工作提供信息，而不是在主要模型部署之前进行一次性参与和选择流程。该网络的成员将根据其专业知识被要求在模型和产品开发生命周期的各个阶段为红队提供帮助。并非每个成员都会参与每个新模型或产品，并且时间贡献将由每个成员决定，一年内可能只有 5-10 小时。

除了 OpenAI 委托的红队活动之外，成员还将有机会就一般红队实践和调查结果进行相互交流。目标是实现更加多样化和持续的输入，并使红队成为一个更加迭代的过程。该网络补充了其他协作人工智能安全机会，包括我们的研究人员访问计划和开源评估。

##

**Why join the OpenAI **Red Teaming Network?

This network offers a unique opportunity to shape the development of safer AI technologies and policies, and the impact AI can have on the way we live, work, and interact. By becoming a part of this network, you will be a part of our bench of subject matter experts who can be called upon to assess our models and systems at multiple stages of their deployment.

为何加入 OpenAI 红队网络？

该网络提供了独特的机会来塑造更安全的人工智能技术和政策的发展，以及人工智能对我们生活、工作和互动方式的影响。通过成为该网络的一部分，您将成为我们主题专家的一员，他们可以被要求在部署的多个阶段评估我们的模型和系统。

Seeking diverse expertise

Assessing AI systems requires an understanding of a wide variety of domains, diverse perspectives and lived experiences. We invite applications from experts from around the world and are prioritizing geographic as well as domain diversity in our selection process.

**寻求多元化的专业知识**

评估人工智能系统需要了解广泛的领域、不同的观点和生活经验。我们邀请来自世界各地的专家提出申请，并在我们的选择过程中优先考虑地理和领域的多样性。

Compensation and confidentiality

All members of the OpenAI Red Teaming Network will be compensated for their contributions when they participate in a red teaming project. While membership in this network won’t restrict you from publishing your research or pursuing other opportunities, you should take into consideration that any involvement in red teaming and other projects are often subject to Non-Disclosure Agreements (NDAs) or remain confidential for an indefinite period.

**报酬和保密**

OpenAI 红队网络的所有成员在参与红队项目时都将获得贡献补偿。虽然该网络的成员资格不会限制您发表研究成果或寻求其他机会，但您应该考虑到，参与红队和其他项目通常需要遵守保密协议 (NDA) 或无限期保密。时期。

##

**How to apply**

申请网址：https://openai.com/form/red-teaming-network

**如何申请**

申请网址：https://openai.com/form/red-teaming-network

按照要求填写，提交等待审核即可。

# 二、技术拆解

## 官方技术报告文 (中英文对照版)

👉OpenAI Sora视频生成模型技术报告中英全文+总结+影响分析

## 腾讯科技拆解文

👉OpenAI王炸模型引爆科技圈，我们第一时间深读了官方技术报告

## 阿里巴巴分析文

👉复刻Sora有多难？一张图带你读懂Sora的技术路径

👉一[文看Sora技术推演](https://mp.weixin.qq.com/s/3RnrO7fSMizEl3mN3SXG5w)

## 素人笔记

👉Sora是怎么训练出来的 | 笔记

## 参考论文

👉S[ora参考论文](https://huggingface.co/collections/pxiaoer/sora-65d0e2db17e2b305e0fc572e)

# 三、研究报告

## 券商研报

短短三日，已有超过14家券商发布逾19份相关研报，主要围绕Sora的发布及其对AI技术、市场、行业应用等方面的影响进行了深入分析和讨论，算力有望成投资新热门。

![](img/69d135c35666841b1d326dac358942c3.png)

[SORA免费学习资源.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KL6vb6HQloUO4GxdMm3cTMIYnqo)

![](img/f7aabceed97972285f803cbbef8934f5.png)

[《 绝世好 Sora 》.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QjpUbVJCFoQPxbxPul7cNEVcnid)

![](img/e39a0a0b8a3431f71e9879143e5417b8.png)

[多模态AI大模型点评：OpenAI发布首款文生视频大模型Sora，训练算力需求大幅提升.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PFgtbWJXso4anKxohSwcxZDWnGc)

![](img/a4fa1ada2da570e4ba47d81f3e868d37.png)

[计算机行业点评报告：Sora重磅发布，AI产业迈入新阶段.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/O9MYbNt6foNHrUx3d52cmAzVnnb)

![](img/9beca6fb3c1f09cbdf2c688f518ccd14.png)

[传媒互联网行业点评：Sora横空出世，关注内容资产价值重估下的两条投资主线.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WLiZbigPloqZLzxXSPec2x2Rnxt)

![](img/94bf292a6d83b9d8745f6a4df8ab0d2b.png)

[全球科技行业周报：OpenAI发布首个AI视频模型Sora，影视、旅游及本地消费高景气.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MRs9bC8dRoBb7Ext2prcm9abnfJ)

![](img/c7e96837a6a74d57f01d71b1e0d4ee4d.png)

[计算机行业跟踪报告：OpenAI推出首个文生视频大模型Sora，引领AI文生视频行业跨越式发展.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KbTPbxLYpoSRcDx5QVEcYv5vnff)

![](img/d90c501d58e62745bad2eb2dbd222042.png)

[通信行业研究周报：通信行业高质量发展稳步推进，Open AI大模型持续巩固优势.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PqdIbowGaoFxVCxUphfckmgQnNM)

![](img/928ba46fdec5cf912f7152f6efbcd5bc.png)

[信息技术产业行业研究：Open AI文生视频Sora惊艳亮相，看好Ai受益产业链.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PAPRbwO3KoMkglxLLFLchEDanmd)

![](img/035826fae4243a8a2bb3872bb2cd00e2.png)

[木头姐2024年度重磅AI技术革命Big Ideas 2024（中文编译版）.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OvHObK9efojLObx2UDRcQD8OnEg)

![](img/3805bd03420dc3df58c0a21bd6fa65bd.png)

[计算机：Sora实现文生视频跨越式突破，AIGC持续正反馈.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PQribf4JHocKMCxKRhFctNIenNh)

![](img/32b757d28b84eb679eba9e8766a52224.png)

[【中泰传媒】OpenAI发布首款文生视频模型Sora，多模态推动多行业发展可期.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EdhRblPFqon53NxIMDkcHZD0n7I)

## 研报分析

1、**Sora的发布和影响**：Sora是OpenAI发布的首个视频生成大模型，被认为是AI产业的一个重要里程碑。券商们普遍认为，Sora的发布加速了AGI时代的到来，对视频行业乃至整个人工智能领域都将产生深远影响

2、**AI技术的发展**：券商研报强调了Sora模型在AI多模态领域的飞跃式发展，预示着AI创作等相关领域将迎来深度变革。这包括多模态相关的训练及推理应用的扩大，以及AI赋能范围的进一步扩大

3、**市场反应和投资机会**：券商们普遍给予Sora模型极高评价，认为其将为AI视频行业带来巨大变革，并有望成为投资新热门。特别是在算力需求方面，算力有望成为新的投资热点

4、**行业应用前景**：券商研报还关注到Sora在影视行业的潜在应用，认为OpenAI通过教AI理解和模拟现实中的运动，可能会使影视行业迎来巨变时刻。Sora是一种通用的模型，能够理解和模拟现实世界中的各种场景，帮助人类解决与现实交互的问题

5、**技术特点和能力**：Sora模型能够通过文本生成长达1分钟的长视频，展现了其在AI视频生成方面的跨越式突破。此外，它还具备一键成片、AI美术设计、文生音乐、音频增强、音效分析等核心能力

这些研报普遍看好Sora的未来发展潜力，并认为其将为相关行业的变革带来积极影响。

| # **券商公司对Sora的研报** |  |
| --- | --- |
| ####### **券商公司** | ####### **研报观点** |
| **浙商证券** | 浙商证券：有望开启多模态大模型的新一轮浪潮 |
| **国泰君安** | 国泰君安：AI生成视频的里程碑。国泰君安在研报中指出，2月16日OpenAI发布文生视频模型Sora，AI视频生成能力实现跨越式发展，模型强大的视频生成能力推高行业天花板，产业赋能的深度和广度有望超预期拓展。 |
| **东吴证券** | 东吴证券：多模态应用有望加速 |
| **天风证券** | 天风证券：下一个亿级用户互联网平台雏形已然出现。天风证券认为，在内容创作工具，OpenAI已经和其他产业出现代际差，并带动行业大幅进展，未来一年商业化预计大幅推进。 |
| **国盛证券** | 国盛证券指出，影视动画行业的颠覆性时刻到来，AI算力有望继续成为新一年的热门投资方向； |
| **浙商证券** | 浙商证券认为，有望开启多模态大模型的新一轮浪潮；华金证券建议重点关注视频生成及强相关、率先落地的领域，开源证券建议布局AI应用及算力方向。 |
| **招商证券** | 招商证券指出，Sora显著领先于其他文生视频模型，推动AI视频生成进入了一个全新的时代； |
| **华泰证券** | 华泰证券认为，AI视频进入大规模应用前夜。 |
| **华金证券** | 华金证券给出的投资建议是，Sora的发布意味着继文字生成和图像生成后，视频生成技术取得了突破性的进展，而今年也将成为视频生成的元年。 |

## 海外Sora研报32份

![](img/d922925d8e2eb0ccaf1b048b242cfbb4.png)

[Zero-Shot Text-to-Image Generation.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Vr8oblUswoKd2sxKZuMcj9LOnoe)

![](img/ebcf2a213ed6905e06f61daa618542ae.png)

[World Models.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SxctbHti2o7658xveYrcEoJjnMc)

![](img/fe6977d984fc2594594aa0e0fce2d8b1.png)

[VideoGPT- Video Generation using VQ-VAE and Transformers.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ku2vb7Z7goeMq3xbxPJcczRLnpc)

![](img/1b7e508fe6a33718f0ede625fe6e04a0.png)

[Unsupervised Learning of Video Representations using LSTMs.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/P3ZhbQ4Q7oiya1x9G7tcObY0nWc)

![](img/85af855d6ef31a553049b1fe0a064efe.png)

[SDEdit- Guided Image Synthesis and Editing with Stochastic Differential Equations .pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PZTGbOAzwoyaJKxZKqFck5ALnkh)

![](img/45101dcbb99279562f483859f9fd020a.png)

[Recurrent Environment Simulators.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UFNubQ15komdFVxeKKscQdZpnmh)

![](img/e1c683b08ab76d8938e1f24ee26e4ec3.png)

[Scaling Autoregressive Models for Content-Rich Text-to-Image Generation.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DemfbKjFmo4Fe3xTPKbcOJ1Gnjg)

![](img/5d07068802b2bd18c26982dbbde0a5f1.png)

[Photorealistic Video Generation with Diffusion Models.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/G1DfbBgEQoJd0bx1zLMc0CbwnRb)

![](img/c0e72dce47d0c6231756ef24a3b2b1ee.png)

[Scalable Diffusion Models with Transformers .pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OpS3bGBBxo62IExmpt5cFBvznJi)

![](img/de2bcec76da9d8f70db8275f8040c842.png)

[Patch n' Pack- NaViT, a Vision Transformer for any Aspect Ratio and Resolution.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/L3o7btKyco9v8LxvWdxcKZ9hnKb)

![](img/c99212ec9f8089bc5635a2eb9ca05aa4.png)

[Improved Denoising Diffusion Probabilistic Models.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IATlb1a79o2imExu41OcYg6XnQb)

![](img/f8c0b478e8ebd400e6906ed2e09e5692.png)

[NÜWA- Visual Synthesis Pre-training for Neural visUal World creAtion .pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AXPbb2VUeoh5rRxfwlucqVUwnki)

![](img/5f86dc1e8548a50a12b8132a358857ef.png)

[Masked Autoencoders Are Scalable Vision Learners.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VzunbtQEVopNqQx5BbRcFSjNnmc)

![](img/56a81325f77c22b2ffb9a0be2e1bab1b.png)

[Language Models are Few-Shot Learners.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HlSXb4xHvoqUx1xNfAtcdgConah)

![](img/c36c05bbbbdb20b259d72151666615ec.png)

[MoCoGAN- Decomposing Motion and Content for Video Generation.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LZnGbQ4jnohE7AxtUDBcPPv9n1c)

![](img/939d10fbefacafed3200329d06bb120e.png)

[Improving Image Generation with Better Captions.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OzLGbUWZsolY4gxWqRBcatr0nHc)

![](img/c9e1c872eb6429e81f13502110df4db7.png)

[Imagen Video- High Definition Video Generation with Diffusion Models.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IgsabmrZCorG7TxjsKIcKqFynsf)

![](img/6af1f1566d2fa77f7ed1ea4018afc2b7.png)

[Hierarchical Text-Conditional Image Generation with CLIP Latents.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WzINb09LUopoZjxHAKCc0EtbnAg)

![](img/fd9b7a2548bf2a2b1eaa6cfdba553c0b.png)

[High-Resolution Image Synthesis with Latent Diffusion Models.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ThxSbL0SEo1eCaxUzcQc6rgYnaf)

![](img/6b7d31d1fb734723cc5d7fb922806659.png)

[Generative Pretraining From Pixels.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MJEZbjrV4oKbncxHvfccANdRngb)

![](img/fd696a131e0535d5269a23389c9c402a.png)

[Generating Long Videos of Dynamic Scenes.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BKg3b3Qd9omJFrx8RGRcJ2Vzn1d)

![](img/3d13ff9d9a86987640c96fa7f4acdd75.png)

[Generating Videos with Scene Dynamics.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FKUNbPTcdoZLOwxFMbacW2pUn0b)

![](img/aef5a5dcffa62db211cf4724de2345b3.png)

[Elucidating the Design Space of Diffusion-Based Generative Models.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FdjcbNCvdo8lqRxFlv2cEI2ZnFb)

![](img/cb62e2e475a7d99298d1730bc2232bb3.png)

[Diffusion Models Beat GANs on Image Synthesis.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FzM8bFwdVooPDwxBUkeccgu5nzf)

![](img/4bb6c40f662f796969b40da1c3fa2059.png)

[Denoising Diffusion Probabilistic Models.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DOo6bXSsvoBJmWxOFDVce8GtnCg)

![](img/6ba8a684d6f2cc2a7c19eb0f0da4fc9c.png)

[Deep Unsupervised Learning using Nonequilibrium Thermodynamics.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UBxDbVnvWogV4fxiy0FcSt6rnLW)

![](img/864481b42f36f799b2b4a53ae20b0167.png)

[Auto-Encoding Variational Bayes.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EG7MbAUnZoA75QxMSg5cgv3vnYd)

![](img/f6902f79e954d53570554320d7cd23e1.png)

[Adversarial Video Generation on Complex Datasets.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XOT0by4KzohXnCxPHCPcGyXdnhb)

![](img/8d6da15374ae8b27b37719c26449ed3c.png)

[An Image is Worth 16x16 Words- Transformers for Image Recognition at Scale.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LKNwbM32EoYFYlxkEDXc6Hsgn3g)

![](img/67ce3cd34c14f29f3236f39f6f945a2a.png)

[Align your Latents- High-Resolution Video Synthesis with Latent Diffusion Models.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JpnHbzDdDo6G9uxDW8kcSryxnxd)

![](img/781d5112b5e828c156a0dfe34173fcf0.png)

[Attention Is All You Need.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ai0RbsrGFoI1VCx1jp2cOzTVnOe)

# 四、Demo案例

## 最新视频案例（持续更新..）

👉点击观看全部40+Sora最新完整视频

**2024年2月16日凌**晨，Open AI发布了一个名为Sora的视频生成模型。Sora可以模拟真实世界的运动方式，根据提示词生成长达一分钟的视频，同时保证画质和连续性。以下视频demo片段均由该模型生成,未经后期编辑和修改。

提示：一位时尚的女人走在东京的街道上，街道上到处都是温暖的发光霓虹灯和动画城市标志。她身穿黑色皮夹克，红色长裙，黑色靴子，背着一个黑色钱包。她戴着墨镜，涂着红色口红。她自信而随意地走路。街道潮湿而反光，营造出五颜六色的灯光的镜面效果。许多行人四处走动。

提示：几只巨大的毛茸茸的猛犸象踩着白雪皑皑的草地走近，它们长长的毛茸茸的毛毛在风中轻轻吹拂，远处白雪皑皑的树木和壮观的白雪皑皑的山脉，午后的光线与薄薄的云层和远处高高的太阳营造出温暖的光芒，低相机视角令人惊叹，用美丽的摄影捕捉了大型毛茸茸的哺乳动物， 景深。

提示：电影预告片，讲述了 30 岁的太空人戴着红色羊毛针织摩托车头盔的冒险经历，蓝天，盐沙漠，电影风格，用 35 毫米胶片拍摄，色彩鲜艳。

提示：无人机视角，海浪拍打着大苏尔加雷角海滩崎岖的悬崖。汹涌澎湃的蓝色海水掀起白色的波浪，而夕阳的金色光芒照亮了岩石海岸。远处有一个带灯塔的小岛，绿色的灌木丛覆盖着悬崖的边缘。从公路到海滩的陡峭落差是一项戏剧性的壮举，悬崖的边缘突出在海面上。这张照片捕捉到了海岸的原始美景和太平洋海岸公路崎岖的景观。

👉点击观看全部40+Sora最新完整视频

👉磨铁文化归纳140+Sora视频

# 五、16个普通人赚钱方法

开年王炸，这几天OpenAI旗下的文生视频工具Sora爆火，大家都在疯狂讨论。AI热度又一次暴涨，Sora也被称为“实现AGI的重要里程碑”。Runway、pika、pixVerse和它相比，完全不是一个量级，几乎全方位碾压其他所有AI视频生成工具。

那么普通人如何在Sora正式发布后抢占流量洼地，分一杯羹？

AI研习社合伙人已经通过Sora掘金社区，日赚2000元，欢迎大家加入分销。

以下是汇集了16个普通人赚钱方法，懂的已经开始准备了！

## **1、搞流量**

做自媒体的人都知道，流量=钱，AI领域出现新产品时，必然会吸引所有人的关注。Sora在视频生成领域的出现堪比大型语言模型GPT-4.0，此时的流量生意往往能够最先取得成果。如今有大量的👸号涌现出热门文章，各平台出现爆款短视频，用以获取非常可观的流量。

## 2、卖账号

利用Sora的早期访问限制，可以通过售卖账号或邀请码来获取利润。目前的Sora模型还未对普通用户开放，但大家都迫不及待地期待着。就像当年的ChatGPT一样，OpenAI的账号销售肯定会继续大卖。只是国内的大语言模型和国外的相比，许多普通人并不能完全区分它们。等到真正的Sora对普通用户开放时，国内外的差距可能就会变得更加明显。

## **3、卖提示词prompt**

去年AI绘画提示词交易很火，今年AI文生视频的提示词肯定也会有市场，从提示词打包交易到提示词交易平台。对于熟悉AI的人来说，好的prompt往往容易在prompt交易平台上售出。在AI绘画领域，类似的交易平台如promptbase、prompthero等。

## **4、制作/代生成AI视频**

有两类用户可能需要帮助，一类是想要体验AI视频但不愿注册或深入研究的用户，另一类是有定制需求的用户。如果能剪辑出具有独特风格的短视频内容，可以在社交平台上吸引潜在客户。

## **5、做自媒体账号**

如果你是短视频或自媒体从业者，利用Sora生成的视频内容，发布到自媒体平台，如抖音、快手等，可以吸引流量和粉丝。

## **6、卖Sora的素材**

生成视频素材，然后将其放到各大素材网站上出售，可以赚取使用费。设计师或视频剪辑师应该都了解这一点，因为许多精美的素材，如果想要商用，都需要付费获取。

## **7、社媒视频广告**

制作白噪音视频/治愈系视频，跟AI生成音频结合。这种类型的视频需求量很大，即使是看似没有什么特别内容的视频，播放量往往能轻松达到几百万。

## **8、Sora知识付费**

制作关于Sora的教程或课程，并通过知识付费方式赚钱是一个不错的主意。

过去一年，AI领域最经典的商机之一就是知识付费，无论是写文章、制作教程还是录制课程视频，都是不错的选择。例如：AI研习社合伙人沐凡老师做的

## **9、做电商**

围绕Sora相关的搜索词做生意在电商平台上销售与Sora相关的工具、教程或课程，利用精准的用户群体，是一个不错的商机。

## **10、套壳**

众所周知的原因，国内乃至全球都存在大量的套壳网站和应用。只要你拥有一定的流量或推广渠道，套壳网站就能持续为你带来收益。目前的套路是注册含有关键词"Sora"的域名，同时在应用商店提交与Sora相关的应用，同时接入Runway等产品的API，以获取流量。等到OpenAI发布Sora API时，再进行切换，实现顺畅过渡。

## **11、制作AI小说推广**

去年AI小说推文很受欢迎，今年可能会直接跨越到AI视频领域。尽管制作成本较高，但这可能会引发新一轮的爆发。

## **12、直播带货**

如果没有产品制作能力，可以考虑通过直播售卖。短视频的生成能够吸引用户的注意力，因此推广Sora相关的付费产品可能会更容易成功。

## **13、投资**

如果你是二级市场的投资客，关注AI行业可能会让你赚到一大笔，但在中国股市除外。Sora的推出可能会对Adobe造成利空影响。例如，根据Sora发布的消息，Adobe在2月16日当日的跌幅达到了7.41%。因此，如果之前你做空了Adobe，那么可能会赚上一笔。

例如：

## **14、剧本创作**

文生视频提示词的升级相当于视频剧本的创作，无论是长视频还是短视频，都更依赖于剧本的创作。这一切更加依赖于创作者的能力和创造力。从这个角度来看，Sora的发布对内容创作者来说是非常利好的。

## **15、Sora+电商**

利用Sora生成各类电商产品宣传视频投流，或者制作各类短视频，嵌入产品进行推广，可以借鉴AI模特变装、虚拟试衣等技术。

## **16、开发Sora 相关工具**

针对具备技术背景的人士，开发与Sora相关的网站或工具，如导航网站或AI写真工具，可以用以获取收益。

这些机会涵盖了从直接销售账号、提供生成服务、内容创作、知识分享到电商平台销售等多个方面，为不同背景和技能的人提供了多样化的盈利途径。

有没有启发到你？抓紧行动起来，加入核心会员群 抱团取暖，互通信息，做吃螃蟹的第一波人。

# 六、名人点评

## 360董事长周鸿祎

![](img/41e921ca692cc656116b790e06f57dc7.png)

![](img/e5a76c5103412d55218f65bdc55ba317.png)

## NVIDIA高级科学家Jim Fan

![](img/eb715b93b083d5edb1ab17df19569117.png)

![](img/44c68a5a4c61d07aa1efdba4dd386023.png)

![](img/a7c7ffb44dca13afdbad80ce5ca5acf7.png)

**评价推文一（左）：原文地址**https://twitter.com/DrJimFan/status/1758210245799920123

如果你以为OpenAI Sora仅仅是个类似于DALLE的创意玩意儿……那你可能要重新思考了。Sora实际上是一个基于数据的物理引擎，它能模拟出无数个真实或虚构的世界。这一模拟器通过一些降噪和梯度计算方法，学习到了复杂的图像渲染技术、“直觉式”的物理规律、长远的逻辑推理以及语义的根基。

我一点也不会感到惊讶，如果Sora是在大量的合成数据上，借助于虚幻引擎5（Unreal Engine 5）进行训练的。这是必然的路线！

我们来深入分析下这段视频。给定的提示是：“一杯咖啡中，两艘海盗船相互交战的高度逼真的特写视频。”

+   模拟器创建了两艘具有不同装饰的精细3D海盗船模型。Sora必须在其底层空间中隐式地解决从文本到3D的转换问题。

+   这些3D模型能够在保持彼此距离的同时进行航行，展现连贯的动画效果。

+   对于咖啡的流体动力学处理，连同船只周围形成的泡沫，都是对的。流体模拟是计算机图形领域的一个专业分支，通常需要复杂的算法和方程式来实现。

+   视频的逼真程度，几乎达到了光线追踪渲染的效果。

+   考虑到杯子相比于海洋的小尺寸，模拟器采用了倾斜移位摄影技术，营造出一种微观世界的感觉。

+   尽管这一场景在现实世界中并不存在，但引擎依然能够按照我们所期待的物理规律准确地实现它。

**评价推文二（右）：原文地址****h**ttps://twitter.com/DrJimFan/status/1758233040806125632?s=20

如果真有某个超越我们认知的存在编写了构成我们现实世界的模拟程序代码，我们甚至可以尝试估算这个程序编译后的二进制文件大小。以Meta AI开发的Emu Video为例，它拥有60亿参数。假如说有一个名为Sora的系统，其规模是Emu Video的10倍，并采用了bfloat16格式，那么这位“创造者”的程序大小可能不会超过111Gb。

但这里有几点需要注意:

+   实际上，这样的代码可能比我们设想的Sora系统要简单得多，因为Sora离达到Kolmogorov复杂度（一种衡量对象信息量的方法，代表了最短的无损压缩数据长度）还有很长的路要走；

+   Sora系统的设计不仅仅是为了模拟我们所知的世界，而是能够包含所有可能存在的世界。换句话说，我们所经历的现实只是Sora能够模拟出的众多可能性之一；

+   就像在游戏设计中，使用虚幻引擎5（Unreal Engine 5）创建逼真场景时不需要渲染出每一个细节，我们的物理世界也可能在我们未观察它之前的某些部分实际上是不存在的。

## 媒体人胡锡进

![](img/236eddb5c4310bd3101d437b326ddad4.png)

## 特斯拉 CEO 马斯克

![](img/ec423d85791c654a0eea92f0cd850901.png)

**humans翻译为人类；gg**为Good Games缩写，代指“打得好，我认输” 。

## Runway CEO Cristobal Valenzuela

感慨后浪拍前浪，以前需要花费一年的进展，变成了几个月就能实现，又变成了几天、几小时,

![](img/3f8f1580492cde62f561aca3a2bd4738.png)

## 出门问问 CEO 李志飞

![](img/9a96e97db704eaaae25ee7527640a354.png)

## 前阿里副总裁贾扬清

评价简单粗暴:“真的非常牛。”他认为“对标 OpenAl 的公司有一波被其他大厂 fomo (害怕错过)收购的机会”“源还需要一段时间才能 catch up（跟上来）”，算法小厂“要不就算法上媲美 OpenAl，要不就垂直领域深耕应用，要不就开源”，并预言“infra（基础设施） 的需求继续会猛增”。

## 纽约大学助理教授谢赛宁

![](img/963acb510bfb341535879c398964174a.png)

# 七、媒体报道

## 商业博主刘润

刘润: OpenAl 全新发布的 Sora，到底意味着什么?

## 政事堂2019

[Sora带来的矩阵革命](https://mp.weixin.qq.com/s/FQF8E7uhZNupotrRDJvZ9g)

## 量子位

[效果炸裂!OpenAI 首个视频生成模型发布，](https://mp.weixin.qq.com/s/K6ZXhH6xHsw0Nfj5TnKCXw)1分钟流畅高清，网友: 整个行业 RIP

## 钵媒体

许四清: Sora 进一步拉大了中美 AI差距

## 新智元

[OpenAI 首个 AI 视频模型炸裂登场，彻底端掉行业饭碗](https://mp.weixin.qq.com/s/93z4Ta91yLv7PB1pnBM9mg)!60 秒一镜到底惊人，世界模型真来了?

## 清华大学

OpenAI发布文生视频模型 Sora，AIGC 视频时代正加速到来

## 央视网

OpenAI 首个文生视频模型亮相，你觉得咋样?

## 知名中文IT技术平台CSDN

OpenAI大杀四方，Sora 彻底革了视频的命

# 八、沐凡叨叨叨

> **Sora 大模型就像是那颗最亮的**星，不仅带火了技术圈，还改变了好多行业。我们这个资料库，就是想把全网关于 Sora 的智慧都聚在一起，让大家能更好地搞懂它、用好它。我们的目标很简单！！！分享最酷的知识、深挖干货、互帮互助、看向未来。

>

> 如[果你对 AI 和 S](https://xiaobot.net/p/AIGC2024?refer=fd9ed25f-2954-4de4-9e99-88e1999bf395)ora 兴趣满满，想要深入挖掘、并且愿意加入这场技术革命，我们非常欢迎你的学习🎯 Sora 掘金手册，并加入Sora学习群，那里已经有350多位Sora爱好者加入了，期待你进来一起交流。

声明：整个飞书里面的内容虽然不是100%原创内容，但却是我耗费大量时间搜集整理的，希望能帮助大家，如果有任何版权问题，请添加🛰️xiyou2046说明，我将把有问题的部分进行删除。

# 九、延伸学习

🔥A[**I研习社知识星球**](https://t.zsxq.com/17UpQr5Cu)

💥C[**hatGPT从入门到精通**](https://faqocxi7k2.feishu.cn/wiki/OcFkw0cE5i6iMCkvLE9cWBphnbc?from=from_copylink)

🖌️MidJourney从入门到精通

💰AI副业掘金风向标更新中....

🚀A[**I掘金工具库更新中...**](https://faqocxi7k2.feishu.cn/wiki/MXLvwWBQYiPyCUk60JqcRl3enKR?from=from_copylink)

# AI研习社学习交流群

1、AI研习社是一个社区型AI研究知识小组，将持续更新AI资讯动态、商业模式研究、研究报告、赚钱案例、AI经验分享等专题内容，希望通过结构化的方式帮助你构建动态的AI专题知识库，促进更多相关知识的分享与传播提升行业知识的流转效率，共同拥抱AI大时代，不错过用AI赚钱的机会。

2、AI研习社拥有**免费的AI飞书知识库**、性价比极高的专题小册字（AI副业掘金手册、Sora掘金手册）和体系化专业化的知识星球（AI研习社），我们深知AI是不可逆转的趋势，所以如果你想加入我们，可以根据自己的需要进行选择，我们希望能够找到志同道合的朋友，陪伴你一起从入门到精通，从小白到高手，用最低的成本学习到AI的技能，获得百倍的收益。

3、现在没进群的可以扫码免费加入AI学**习交流群，**提前了解最新**学习内容，群满可加沐凡🛰️ **ID: mufansay123 (备注: AI)，如果添加不了可加备用🛰️ID：xiyou2046，加好友送《加强版AI副业掘金工具库》哦～

![](img/416fa0cfc2f91cb50fda9b35ecab9c07.png)

🛰️扫码👉免费领价值999工具资料

![](img/fddc96bbee999655e397fc333c362cb1.png)

**🛰️扫码加好友**

（🛰️ID: mufansay123）

**看到这的你真是太棒了**✌️

**为热爱学习的自己点个赞吧❤️**