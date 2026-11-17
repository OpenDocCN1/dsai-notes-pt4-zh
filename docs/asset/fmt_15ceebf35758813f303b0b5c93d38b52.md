
# Stability AI（StableLM）

> 来源：[https://agispace.feishu.cn/docx/EnI6d2zyLo6wNxxbr2wcQiminwe](https://agispace.feishu.cn/docx/EnI6d2zyLo6wNxxbr2wcQiminwe)

![](img/e2436c834afe4e384db6abed7d5d9b08.png)

**GitHub 地址**:

`https://github.com/stability-AI/stableLM/`

**模型官网**:

`https://stability.ai/`

Huggingface Chat 地址:

`https://huggingface.co/spaces/stabilityai/stablelm-tuned-alpha-chat`

## 一、简介

StableLM是StabilityAI开源的一个大语言模型，于2023年4月20日公布，目前属于开发中状态，只公布了部分版本模型训练结果。它是StabilityAI的第一个开源的大语言模型，基于Pile数据训练，包含约1.5万亿tokens，数据集目前没有公开，但官方表示后续会在适当的时机会公布。模型训练的`context`长度是4096个tokens。StableLM系列包含2种模型，一个是基础模型，名字中包含`base`。另一种是使用斯坦福Alpaca的微调流程在5个对话数据集上的联合微调得到的结果，名字中包含`tuned`。StableLM的能力包括闲聊、正式写作、创意写作和写代码等。

## 二、特点

+ 开源和可访问性：StableLM在GitHub上开源，允许任何人下载并部署在本地，包括笔记本等小型设备。

+ 多语言支持：支持英语、西班牙语、德语等7种语言，展现了卓越的语言理解和生成能力。

+ 高效性能：尽管参数规模相对较小，但通过使用大量多样化的数据集进行预训练，StableLM在对话和编码任务中表现出惊人的高性能。

## 三、应用场景

+ 自然语言处理任务：包括机器翻译、情感分析、文本分类和问答系统等。

+ 智能客服和智能写作：帮助企业更好地处理用户反馈、生成优质内容等。

+ 移动设备上的应用：如Stable LM 3B模型，为移动平台设备带来了高性能体验的曙光。

## 四、实测体验

Stability AI 已经将 StableLM 发布在 GitHub 上托管，开发人员可以直接使用或经过微调后使用，同时还发布了用于研究的 RLHF-tuned 模型。StableLM 遵循着开源和对所有人开放的宗旨，将权力交还给开发者社区，目前 StableLM 仍处于 Alpha 阶段，只公布了 30 亿和 70 亿参数的模型，下一步将会推出 150 亿到 650 亿参数的更大参数模型。

![](img/9520f82b7de57f0c59f4230ed84ff092.png)

据介绍，StableLM 是在一个基于 The Pile 建立的新实验数据集上训练的，但数据集规模比原来大三倍，含有 1.5 万亿个 Token 的内容。

Stability AI 用下面的对话展示了他们的大模型 StableLM 生成文本与代码的能力，对于英文的问答确实比较得心应手，这些问答都是用 70 亿个参数微调模型生成的：

![](img/737f3d747c1279e837cd6e7354b75b87.png)

![](img/059533a53588529ef25c325768800d22.png)

而在与 StableLM 的对话体验中，小编还发现它可以用中文交流，能够基本理解我的语义（虽然细节还是有问题）。比较可惜的是，比起 ChatGPT 来说，还是有点距离。它懂点中文，但不多，聊着聊着就开始胡言乱语……

![](img/4d34033e11c3b5ba767d79a862186aa3.png)

![](img/b4e01c8cd6c5421bf04f04156353dcbd.png)

Stability AI 仍强调了希望每个人都能够参与其设计语言模型，像 StableLM 这样的模型展示了致力于透明、易于访问和支持性的 AI 技术：

+ **透明性**：开源模型能促进透明度、建立信任，并帮助研究人员开展可解释性技术工作，从而识别潜在风险并帮助开发保障措施。

+ **易用性**：模型易于访问，开放且细粒度的访问机会能使大众研究和学术界能够发展解释性和安全技术，超越了闭源模型的可能性。

+ **支持性**：旨在帮助用户提高生产率、释放创造力，并开启新的经济机遇。

最后，Stability AI 对于生成回答所存在的潜在问题提醒如下：

任何预训练的大型语言模型，在没有进一步微调和强化学习的情况下，其回复内容可能良莠不齐，包含有冒犯性的言论和观点。随着规模的扩大、更好的数据、社区反馈和优化，这种情况将有望得到改善。
