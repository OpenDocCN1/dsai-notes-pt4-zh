# 240204 - 0129｜张鹏对话 Pika 创始人 Demi：AGI 时代的产品，需要「少年气」和为用户「留白」；Google Bard 更名 Gemini；像人类一样在批评中学习成长，1317 条评语让 LLaMA2 胜率飙升 30 倍；更新后的 GPT-3.5 Turbo 模型，降低了 50%的输入定价，25%的输出定价；比肩 GPT-4，商汤日日新大幅升级 4.0，多模态能力领先一步；苹果 iOS 史上最大更新！Siri 要上大模型了；马斯克：Neuralink 首次将芯片植入人体，产品已在路上 ...

> 来源：[https://leonzhang.feishu.cn/docx/QLc3d2xUUonAwZx24qocQ68mn1o](https://leonzhang.feishu.cn/docx/QLc3d2xUUonAwZx24qocQ68mn1o)

## 🎞️ 上周二三事

### 张鹏对话 Pika 创始人 Demi：AGI 时代的产品，需要「少年气」和为用户「留白」

> https://mp.weixin.qq.com/s/fWD7CYq2X2nkXYCTQmuYpw

2023 年 11 月底，视频生成产品 Pika 发布 1.0 版本，同时宣布了 5500 万美元的融资消息。近期，极客公园创始人张鹏与 Pika 创始人 Demi Guo 在硅谷进行了一次深度交流，期间谈到了视频生成的技术现状，Demi 本人对 AI 产品、应用创业的思考，Pika 团队的优势和壁垒所在，以及她对 AI 初创团队的组织思考等等。

### 大的要来了：谷歌 Bard 被曝将更名为 Gemini，Ultra 1.0 更强但收费、安卓 App 也会来

> https://mp.weixin.qq.com/s/RC9yCU1HWNk1Hk87Hw7UgA

谷歌 Bard 预计将从以下两个方面进行调整：一是 Bard 将正式更名为 Gemini，并亮相 Gemini Ultra 1.0 模型；二是 Gemini 安卓应用程序也将到来。对于这份日志更新，有网友期待地表示，谷歌 Gemini 几天后将大变身，GPT-5 也可能会在今年发布，这下 AI 圈又该热闹起来了。

### 像人类一样在批评中学习成长，1317 条评语让 LLaMA2 胜率飙升 30 倍

> https://mp.weixin.qq.com/s/nyXJanCZYpZhRBNtTT1DJg

最近，香港中文大学和腾讯 AI Lab 的研究者们提出了一项名为对比式非似然训练（Contrastive Unlikelihood Learning，CUT）的创新研究，利用语言反馈来对齐语言模型，让模型像人类一样从不同的批评意见中学习成长。

CUT 简单有效。仅凭 1317 条语言反馈数据，CUT 就能使 LLaMA2-13b 在 AlpacaEval 上的 win rate 从 1.87% 飙升至 62.56%，击败 175B 的 DaVinci003。更令人兴奋的是，CUT 能像其他 RLHF 框架一样形成探索 -> 批评 -> 改进的反复迭代，其中批评可由自动的评语模型来完成，实现整个系统“自产自评自提升”。作者对 LLaMA2-chat-13b 进行了四轮迭代，将模型在 AlpacaEval 上的性能从 81.09% 逐步提升至 91.36%。相较于基于分数反馈的对齐技术（DPO），CUT 在同等数据规模下表现更佳。此研究揭示了语言反馈在对齐领域所具备的巨大发展潜力，为未来的对齐研究开辟了新的可能。

### 击败 OpenAI，权重、数据、代码全开源，能完美复现的嵌入模型 Nomic Embed 来了

> https://mp.weixin.qq.com/s/EMWNf9TiDwuxxkwwqsl73g

一周前，OpenAI 给广大用户发放福利，在下场修复 GPT-4 变懒的问题后，还顺道上新了 5 个新模型，其中就包括更小且高效的 text-embedding-3-small 嵌入模型。我们知道，嵌入是表示自然语言或代码等内容中概念的数字序列。嵌入使得机器学习模型和其他算法更容易理解内容之间的关联，也更容易执行聚类或检索等任务。可见，嵌入在 NLP 领域是非常重要的。

不过，OpenAI 的嵌入模型并不是免费给大家使用的，比如 text-embedding-3-small 的收费价格是每 1k tokens 0.00002 美元。现在，比 text-embedding-3-small 更好的嵌入模型来了，并且还不收费。AI 初创公司 Nomic AI 宣布推出 Nomic Embed，这是首个开源、开放数据、开放权重、开放训练代码、完全可复现和可审核的嵌入模型，上下文长度为 8192，在短上下文和长上下文基准测试中击败 OpenAI text-embeding-3-small 和 text-embedding-ada-002。

### 更新后的 GPT-3.5 Turbo 模型，降低了 50%的输入定价，25%的输出定价

> https://x.com/OfficialLoganK/status/1753122474597011958?s=20

更新后的 GPT-3.5 Turbo 模型现已上线。

它降低了 50%的输入定价，25%的输出定价，并包括各种改进，包括以请求的格式响应时更高的准确性。

### 比肩 GPT-4，商汤日日新大幅升级 4.0，多模态能力领先一步

> https://mp.weixin.qq.com/s/Z7sx4a6B77gauPhotdwb2g

商汤的大模型体系「日日新 SenseNova」今天刚刚发布了 4.0 版，不**论语言能力还是文生图能****力都有全面升级，**还自带低**门槛的落地工具**。新一代 SenseNova 不仅在大语言模型、文生图模型等方面进行了重大升级，部分垂直领域能力超越 GPT-4，还发布了全新多模态大模型，并面向数据分析、医疗等场景提供了全新版本，让大模型通用能力适配到了更多领域。

### 赶超 Gemini Pro，提升推理、OCR 能力的 LLaVA-1.6 太强了

> https://mp.weixin.qq.com/s/WKWD3cBR7X0r57CwS3rvxA

去年 4 月，威斯康星大学麦迪逊分校、微软研究院和哥伦比亚大学研究者共同发布了 LLaVA（Large Languageand Vision Assistant）。尽管 LLaVA 是用一个小的多模态指令数据集训练的，却在一些样本上展示了与 GPT-4 非常相似的推理结果。10 月，LLaVA-1.5 重磅发布，通过对原始 LLaVA 的简单修改，在 11 个基准上刷新了

SOTA. 现在，研究团队宣布推出 LLaVA-1.6，主要改进了模型在推理、OCR 和世界知识方面的性能。LLaVA-

1.6 甚至在多项基准测试中超越了 Gemini Pro。

### 苹果 iOS 史上最大更新！Siri 要上大模型了

> https://mp.weixin.qq.com/s/4p4ZIt3kAY_kgnuaKIuAnw

根据彭博社报道，苹果将在今年 6 月的 WWDC 中推出带有重磅 AI 功能的 iOS 18，其分析师 Mark Gurman 更是将 iOS 18 视为「苹果历史上最大的 iOS 更新之一，甚至是最大的更新」。此前在去年 11 月，彭博社就报道称，苹果希望 iOS 18 能成为其多年来最「雄心勃勃、最引人注目」的更新。

**但更引人关注的，则是 1 月 28 日，有开发者在 iOS 17.4 **的开发者预览版 Beta 中发现了与大模型相关的技术代码。这些代码表明，苹果正在开发一个由大模型支持的新版本 Siri。

### [讯飞星火开源 -13B 大模型发布，针](https://www.ithome.com/0/748/030.htm)对国产软硬件环境深度优化 - IT 之家

> https://www.ithome.com/0/748/030.htm

IT 之家 1 月 30 日消息，在今日下午的讯飞星火认知大模型 **V3.5 升级**发布会上，科大讯飞推出了“星火开源大模型”。

据介绍，星火开源大模型是根据去年 5 月的星火 1.0 版本（13B）改进而来，增加了数据、能力、针对应用场景的工具链，并且针对国产安全可控进行了更系统性的设计。根据科大讯飞官方公布的测试，星火开源 -13B 在多个方面都领先其他同尺寸开源模型 20%+。

### 小扎官宣 Code Llama 重量级更新，新增 70B 版本，但还有能力限制

> https://mp.weixin.qq.com/s/od_YI7MVh_gThffcSM4xAg

据 Meta AI 博客文章的介绍，Code Llama 70B 可比其他版本处理更多查询，这意味着开发人员可以在编程时向其提供更多提示，并且可以更加准确。在 HumanEval 基准测试中，Code Llama 70B 的准确率达到了 53%，优于 GPT-3.5 的 48.1%，进一步接近 OpenAI 论文报告中 GPT-4 的 67%。

### RWKV-5

> https://x.com/BlinkDL_AI/status/1751542433039651304?s=20

RWKV-5 “Eagle” 7B：在多语言方面超越 Mistral-7B，在英语方面达到 Llama2-7B 水平，同时是 100%无需注意力的 RNN，并且仅训练了 1.1T 个令牌。Gradio 演示：

https://huggingface.co/spaces/BlinkDL

2 RWKV-6 “Finch”1B5 约在 10 天内，3B 约在 30 天内。

### 百川智能上新超千亿大模型 Baichuan 3，冲榜成绩：若干中文任务超车 GPT-4

> https://mp.weixin.qq.com/s/GGhbt93eS9-JkM9qyv1WmQ

百川智能发布超千亿参数的 Baichuan 3，在中文任务中超越 GPT-4，医疗和文学创作能力强。

⤴️ 性能提升 Baichuan 3 在各方面性能大幅提升，超越 GPT-4 水平。

❤️ 医疗能力 Baichuan 3 在医疗领域表现超过 GPT-4 水准，具备医疗知识和逻辑推理能力。

✏️ 文学创作 Baichuan 3 在唐诗宋词创作方面能力大幅提升，超越其他模型。

⚡️ 技术创新 Baichuan 3 采用创新的训练和优化方法，解决千亿参数模型训练中的稳定性和收敛性问题。

### [业内首个代码大模型标准发布，中国工商银行、科大](https://www.ithome.com/0/748/138.htm)讯飞、华为、阿里云等参编 - IT 之家

> https://www.ithome.com/0/748/138.htm

IT 之家 1 月 31 日消息，据人工智能产业发展联盟 AIIA 消息，2024 年 1 月 25 日，中国信通院联合中国工商银行牵头发布《智能化软件工程技术和应用要求 第 1 部分：代码大模型》标准，该标准适用于企业在代码大模型的研发、评估和验收等过程中，为代码大模型能力的建设和改进提供参考，为代码大模型的技术选型提供指引。

## 📱 好玩推荐

### 中文创意写作能力超 GPT-4，「最会写」的中文大模型 Weaver 来了

> https://mp.weixin.qq.com/s/4xbZ5JYoBNj9WOjMHWo9UA

ChatGPT 等通用大模型支持的功能成百上千，但是对于普通日常用户来说，智能写作一定是最常见的，也是大模型最能真正帮上忙的使用场景之一。尽管大模型经常能写出看起来像模像样的文字，但是大多数情況下内容的创意程度和文风都经不起深究。尤其是在创作领域，大模型常见的“GPT 文风”更是让利用大模型进行创意写作看起来简单，实际却困难重重。近日，波形智能的大模型团队发布了一款专精 AI 写作的专业大模型

Weaver. 通过写作领域专业预训练和一套创新性的数据生成和 Alignment 算法，Weaver 在写作领域的各种任务上均取得了领先 GPT-4 和众多中文通用大模型的效果，尤其是在生成内容的创意性和文风质量上大幅领先，是一款更能写出“人话”的大模型。

### Faith

> https://namastefaith.com/

Faith 是一个 AI 提供支持的心灵导师，通过各种宗教和非宗教精神作品的深刻见解来提供指导和智慧。它的目标是为人们提供对各种信仰和观点的理解和指导，无论他们是虔诚的宗教信仰者、休闲的灵性追求者，还是只是对生活中更大的问题感到好奇。Faith 在宗教层面为用户提供心灵指引。

### OpenArt

> https://openart.ai/

OpenArt 是一个为每个人提供 AI 图像生成工具的平台。他们致力于打开 AI 图像生成的黑匣子，让人们可以通过简单的文本提示来实现他们的创意。OpenArt 与其他 AI 图像平台不同，它策划了由世界各地独立开发者构建的应用程序，包括动画 GIF 生成器、草图到图像、RPG 人像制作者、逼真的图像转换器、换脸、AI 漫画店、AI 标志生成器、AI 贴纸生成器等。用户可以免费在 OpenArt 上创建令人惊叹的图像，并且还可以训练自己的自定义模型。

### 📕开源「InstantID」效果炸裂，被 Yann LeCun 点赞，迅速蹿上 Github 热榜

> https://mp.weixin.qq.com/s/Etl0HUVRdxwsgcM0ErqHjA

最近，有一群来自📕的 95 后神秘团队，自称 InstantX，搞了个大动作 —— 开源「InstantID」项目。

InstantID 凭借着高质量的图像生成能力，在开源界掀起了一股热潮：不仅获得了众多技术大佬的点赞，更是在 GitHub 热榜上迅速飙升，成为焦点。

这个「出片神器」，让用户只需上传一张照片，就能轻松定制出多种风格的 AI 写真。

### 刚刚，字节版 GPTs「扣子」上线了

> https://mp.weixin.qq.com/s/efNjbeK8Zul39nLzQuawCg

在持续一年的大模型热潮之后，「智能体」成为了科技公司们新的押注方向之一。近日，字节跳动正式推出「Coze 扣子」Al Bot 开发平台。任何用户都可以快速、低门槛地搭建自己的 Chatbot，且平台支持用户将其一键发布到飞书、🛰️👸号、豆包等渠道。当然，除了可以创建自己的 Chatbot，Coze 官方还提供了 Bots 商店和插件。链接：https://www.coze.cn/

### 这款国产免费编程工具火了！清华博士团队开发，响应延迟短、准确率高

> https://mp.weixin.qq.com/s/jMFJWptOjLVPEdwddKnFyA

**近日，非十科技推出了一**款基于自研代码大模**型打造的 AI 代码助手** ——Fitten Code，它可以帮助程序员更迅捷、更准确、更高质量地完成编码任务，大幅提升编码效率，并且向用户免费开放使用！

产品官网地址：https://code.fittentech.com/

## 🤖 智能硬件

### 马斯克：Neuralink 首次将芯片植入人体，产品已在路上

> https://mp.weixin.qq.com/s/dsDgkgG8zt1TELbGTmveAA

马斯克的 Neuralink 公司首次将芯片植入人体，产品已在路上

✏️ 产品名字 首个商用产品名字为 Telepathy

⭐️ 里程碑 Neuralink 首次将脑机接口设备植入人体

⤴️ 目标 帮助恢复人类视力和控制智能设备

## 💰 上周融资

### 百页 PPT，回顾 VC/PE 的 2023

> https://mp.weixin.qq.com/s/CfbomK_klau70Ru2VBmK3A

2023 年中国股权投资市场受外部环境影响，整体呈现下行趋势。新募集基金数量和规模同比下滑，外币基金尤为明显。投资案例数和披露投资金额也有所减少，国有背景投资机构在半导体、新能源等领域保持活跃。退出案例数下降，A 股 IPO 案例数大幅减少，市场平均发行回报倍数下调。清科研究中心提供详细报告分析。

### 蓝驰创投、西湖科创投相中这家 AIGC 公司 联手浙大团队共同研发

> https://www.chinastarmarket.cn/detail/1583827

波形智能宣布完成千万元级 Pre-A 轮融资，由蓝驰创投领投，西湖科创投和藕舫天使跟投。波形智能成立于 2023 年，专注于 AIGC 领域，其自研的大语言模型 Weaver 在小说创作、营销文案、短视频脚本和游戏 NPC 塑造等方面取得突破。Weaver 大模型由波形智能联合 APUS 和浙江大学陈华钧教授团队共同研发，特别强调其在中文内容生成上的优势。波形智能创始人姜昱辰强调，Weaver 大模型专为创作而生，通过预训练和工程师调整，提升了写作内容的“人味”。公司计划在 C 端和 B 端市场推出产品，B 端开放 API，C 端推出 AI 辅助创作工具蛙蛙写作 1.0。蓝驰创投认为，AIGC 垂直大模型需聚焦垂直领域，且应考虑出海战略。

## 📚 学习资料

### Meta 官方的 Prompt 工程指南：Llama 2 这样用更高效

> https://mp.weixin.qq.com/s/Xy8HdQih2d3lpNigCkOWIA

Llama 2 是 Meta 的 LLM 模型，有多种部署方式和托管 API。

⭐️ Llama 2 模型 Meta 推出的 LLM 模型，有不同参数规模。

⤴️ 部署 LLM 可通过自托管、云托管或托管 API 进行部署。

🌐 托管 API 提供了两个主要端点：completion 和 chat_completion。

📄 token LLM 以 token 的形式处理输入和输出。

### AI 领域精选文章（2024 年汇总版）！

> https://mp.weixin.qq.com/s/-3imOzA-qqIBc4rQ4Vblrg

这篇文章是 2024 年 AI 领域的精选文章汇总，涵盖了机器学习、深度学习、时间序列等多个板块。文章提供了丰富的技术资源，包括 Python 人工智能学习路线、机器学习模型总结、优化算法、可视化技术、特征工程、模型可解释性、自动化机器学习库等。同时，还涉及了深度学习领域的图神经网络、强化学习、时间序列预测等前沿话题。此外，还包括了 AI 大模型、生成式人工智能（AIGC）以及 ChatGPT 等大规模语言模型的综述。这些内容旨在帮助读者深入理解 AI 技术，提升研究和开发能力。

### [GPU 深度学习性能的三驾马车：Ten](https://zhuanlan.zhihu.com/p/669987669?utm_medium=social&utm_oi=56635854684160&utm_psn=1736330574372118528&utm_source=wechat_timeline)sor Core、内存带宽与内存层次结构

> https://zhuanlan.zhihu.com/p/669987669?utm_medium=social&utm_oi=56635854684160&utm_psn=1736330574372118528&utm_source=wechat_timeline

本篇文章讨论了 GPU 在深度学习性能中的三个关键因素：Tensor Core、内存带宽与内存层次结构。Tensor Core 对于矩阵乘法运算至关重要，可以大幅提升性能。由于矩阵乘法是深度神经网络中计算量最大的部分，因此这种计算核心显得格外重要。内存带宽的大小直接影响 Tensor Core 的数据获取速度，进而影响整体性能；而内存层次结构，尤其是二级缓存的大小，决定着训练过程中数据加载的次数。了解这些性能指标有助于选用合适 GPU、优化深度学习应用的运行效率。

### DeepLearning Al：新短期课程“使用向量数据库构建应用程序”

> https://x.com/DeepLearningAI/status/1752730997936771398?s=20

DeepLearning.Al： 查看与 @Pinecone 合作创建的新短期课程：使用向量数据库构建应用程序。

学习如何构建六个使用向量数据库的应用程序，包括 RAG、面部相似性和异常检测。

立即注册：https://hubs.la/Q02jhPPgO

### [论文阅读：RWKV Architecture & Linear ](https://zhuanlan.zhihu.com/p/680322418?utm_medium=social&utm_oi=56635854684160&utm_psn=1735334404703043584&utm_source=wechat_timeline)Transformer & Attention Free Transformer

> https://zhuanlan.zhihu.com/p/680322418?utm_medium=social&utm_oi=56635854684160&utm_psn=1735334404703043584&utm_source=wechat_timeline

知乎专栏文章介绍了几种线性复杂度的 Transformer 架构：RWKV（Rethinking Weight Key Value）、Linear Transformer 和 Attention Free Transformer（AFT）。RWKV 结构采用了相对位置编码，通过 token shift 机制实现序列间的信息传递，优化了模型的运算效率。Linear Transformer 使用 kerne|函数简化传统注意力机制，从而将计算复杂度从平方级降至线性。而 AFT 摒弃了 Attention 机制，利用门控函数和位置信息的加权和改进了模型结构，同时简化了计算。这些架构在推理效率和存储空间上均有显著改进。

### 写在跨年之前：聊聊 LLM Agents 的现状，问题与未来

> https://zhuanlan.zhihu.com/p/679177488

作者在知乎专栏文章中讨论了 LLM Agents 的当前状况、问题和未来展望。首先给出了三种理解 LLM Agents 的视角，包括面向用户的实用定义、直观定义以及生产者视角的学术定义。接着深入讲解了构建 LLM Agents 时会使用到的一些技术，如 RAG、COT、多模态等，并探讨了各技术的细节、应用场景和潜在问题。此外，还涉及了如意图识别与执行、数据通路与行动框架等概念。最后，作者分享了对 LLM Agents 可能会遇到的问题的反思，以及未来的愿景，特别是在游戏领域中的应用。整篇文章围绕大型语言模型的智能体展开深入的分析和展望。

### 随机 Transformer

> https://mp.weixin.qq.com/s/4WtoHGegZY6o4Jaa3bz66Q

Transformer 模型数学原理解释

📄 文本嵌入 将输入 token 转换为向量

1️⃣ 位置编码 表示单词在句子中位置的信息

↔️ 自注意力 模型控制输入不同部分的重要程度