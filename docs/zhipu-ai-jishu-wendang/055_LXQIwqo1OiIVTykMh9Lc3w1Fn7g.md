# CogVLM 技术文档

> 来源：[https://ai.feishu.cn/docx/T1LZdqfnMoDOiNxlkticaf3Xn0f](https://ai.feishu.cn/docx/T1LZdqfnMoDOiNxlkticaf3Xn0f)

+   作者：智谱团队

+   Github 官方仓库：https://github.com/THUDM/CogVLM

+   技术支持：请到 github issue 提问

### 模型介绍

CogVLM 模型由四个基本组件组成：视觉转换器 （ViT） 编码器、MLP 适配器、预训练大型语言模型 （GPT） 和视觉专家模块。

CogVLM 是一个功能强大的开放视觉语言模型。CogVLM-17B 版本具有 170 亿参数，其中 100 亿用于处理视觉信息，70 亿用于处理语言信息。它能够理解和处理 490*490 分辨率的图像，并能够进行多轮对话。从已有的预训练语言模型我们在保证纯语言模型相同的NLP性能前提下，使用视觉专家（Visual expert module）转换图像特征，向LLM模型每一层添加一个Visual expert module对齐不同的头部，从而实现深度融合。通过ViT编码器(EVA2-CLIP-E) 在word embedding latent 之后通过使用了一个 MLP 适配器，一个两层 MLP(SwiGLU)用于图像特征在语言模型中共享相同的位置 ID，以进行对比学习  。

CogVLM有着卓越性能和鲁棒泛化性，源于我们对一系列多模态基准进行了定量评估。这些基准可分为三大领域，涵盖全面的测量范围Image Captioning、Visual Question Answering、LVLM Benchmarks，在这些benchmarks中，Image Captioning相比同模型全面实现了SOTA或兼容性能,Visual Question Answering基准明显优于类似参数规模的模型，在所有7个LVLM Benchmarks中都取得了最先进的结果，明显超过了所有其他模型。它还优于使用较大语言模型的多模态模型

CogAgent是一个经过CogVLM升级的开源视觉和语言模型。CogAgent-18B这个版本有110亿个用于处理图像的参数和70亿个用于处理语言的参数，它可以理解1120*1120分辨率的高清图像。除了CogVLM原有的功能外，CogAgent-18B还能处理图形用户界面（GUI）的图像。

CogAgent-18B在9个跨模态基准测试中达到了当前最先进的水平（SOTA），这些测试包括VQAv2、OK-VQA、TextVQA、ST-VQA、ChartQA、infoVQA、DocVQA、MM-Vet和POPE。同时，它在处理图形用户界面（GUI）操作的数据集上，比如AITW和Mind2Web，表现远超其他现有模型。

### **论文地址**

> [C](https://arxiv.org/abs/2311.03079)ogVLM: Visual Expert for Pretrained Language Models

> [CogAgent: A Visual Language ](https://arxiv.org/abs/2312.08914)Model for GUI Agents

### 模型动态

> 更新细节：

> 🔥 N**ews:2**023/12/26 我们公开了 CogVLM-SFT-311K 数据集，它包含了超过15万条我们用于训练 CogVLM v1.0(仅该模型) 的数据。欢迎关注和使用。

> 🔥 N**ews:2**023/12/18新的 Streamlit用户界面 已经上线！我们已经基于Streamlit推出了新的网页用户界面，用户可以在我们的界面上轻松与CogVLM，CogAgent交谈。带来更好的用户体验。

> 🔥 N**ews:2**023/12/15 CogAgent上线。CogAgent 是在CogVLM基础上开发并优化的图像理解模型。它具备基于视觉的图形用户界面（GUI）代理能力，并且在图像理解方面有更进一步的提升。CogAgent 可以处理 1120*1120 分辨率的图像输入，并具备图像多轮对话、GUI 代理以及定位等多种能力。

> **🔥 News: 2**023/12/8 c[og](https://github.com/THUDM/CogVLM/blob/main/README_zh.md#%E7%AE%80%E4%BB%8B)vlm-grounding-generalist 更新 v1.1，该版本在训练时加入图像数据增强，表现更加鲁棒。点击查看更多细节。

> 🔥 N**ews:** 2023/12/7 CogVLM 支持4-bit量化。

> 🔥 N**ews:** 2023/11/20 c[og](https://github.com/THUDM/CogVLM/blob/main/README_zh.md#%E7%AE%80%E4%BB%8B)vlm-chat 更新 v1.1 版本，该版本同时支持对话和问答，在多项数据集刷新 SOTA 效果。点击查看更多细节。

> 🔥 N**ews:** 2023/10/27 [CogV](https://chatglm.cn/)LM 中英双语版正式登录 智谱清言

> 🔥 N**ews:** 2023/11/20 CogVLM 的 🤗huggingface 版已开源

# 📚 信息汇总

官方网站：https://www.zhipuai.cn/

智谱清言：https://chatglm.cn

API开放平台：https://bigmodel.cn/

Github仓库：https://github.com/THUDM

# 👋  价值观

+   让机器像人一样思考。

---

# 🏠 👸号

![](img/ad28021854c6f81d35c0ef2fb5b5345a.png)

# ❤️ 交流群

![](img/180d2cab6afbe5dafad32965bdcdebda.png)

---

# 💡  Help

本 wiki 将尽可能完备地提供关于 CogVLM 相关问题的解决方案，但在这是一个不断完善的过程，必然会有许多错误或遗漏的地方，您在使用过程如有任何建议，欢迎随时联系我们。

### 错误反馈

+   如果 wiki 文档中有局部错误，请「划线」评论；

+   如果您觉得某个段落或某个文档有较大错误，或需要优化，请再文档下方进行评论留言，或联系管理员（见下方）

+   [常见问题](https://lslfd0slxc.feishu.cn/wiki/QpqawLISmihwfBkXjQZcTS3qn9w)果本 wiki 内容无法解决您的问题，请在   下方留下您希望解决的问题。但在提问之前，请务必确保本文档内容确实不包含您所需要解决的问题。

### 合作/贡献

如果您有下列需求，请联系管理员：

+   [做出贡献](https://lslfd0slxc.feishu.cn/wiki/WFx8wHev5ilQ9Tk7ERGcKa8MnVd) 如果您想要对本当做出更多贡献（例如添加某个文档），请联系管理员。

+   技术/商业合作需求

+   生态合作需求，包括且不限于模型适配、推广合作、课程合作等