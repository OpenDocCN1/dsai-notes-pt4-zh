# 04🈷️10日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HIlOdiqaJonMUNx9vqBce1FenMb](https://v11enp9ok1h.feishu.cn/docx/HIlOdiqaJonMUNx9vqBce1FenMb)

> 🔔AI情报助手午报「04月10日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🎯

> AI Agent大变天！谷歌开源A2A，一夜改变智能体交互

>

+   2025年4月10日，谷歌在Google Cloud Next 25大会上开源首个标准智能体交互协议A2A，还开源Agent开发套件等。

+   A2A打破系统孤岛，基于HTTP等标准构建，支持多模态，遵循使智能体自然协作等五个原则。

+   其工作原理是促进客户端与远程智能体通信，通过“智能体卡”分配任务，支持协作与用户体验协商。

+   A2A获50多家著名企业加入，如埃森哲等，技术公司Atlassian等也参与。

+   谷歌欲借A2A统一智能体格局，打造新执行、交互标准。

> 关键词：谷歌，A2A，智能体，开源，企业合作

>

> 🔗 文中引用的链接

+   [开源地址](https://github.com/google/A2A)

+   [原文链接](https://mp.weixin.qq.com/s/ih2FLqmWhJiRvoXxLu68Ag)

> 2️⃣

> 🤖

> 斯坦福等开源代码定位AI Agent，极大提升开发、维护效率

>

+   斯坦福等高校研究人员联合开源用于代码定位的智能体LocAgent，可解决传统代码定位难题。

+   其核心是“图基代码”表示，将代码库解析为有向异构图，捕捉结构和依赖关系。

+   通过稀疏层次实体索引增强搜索能力，几秒钟内定位相关代码实体。

+   引入基于Agent的代码搜索，提供SearchEntity等三种工具辅助搜索推理。

+   LocAgent有助于提升软件开发和维护的效率。

> 关键词：代码定位，LocAgent，开源，AI智能体

>

> 🔗 文中引用的链接

+   [开源地址](https://github.com/gersteinlab/LocAgent?tab=readme-ov-file)

+   [原文链接](https://mp.weixin.qq.com/s/nQwYTXShMcsXFPLnMiRMvQ)

> 3️⃣

> 🤖

> 谷歌升级AI搜索，加入视觉搜索功能

>

+   谷歌升级AI搜索，将此前部分仅对Google One AI Premium订阅用户开放的功能扩大，并新增视觉搜索。

+   用户能通过文字、语音、图片等方式提问，利用Gemini多模态功能，系统可分析场景及物体关系属性。

+   例如对书架拍照问推荐，系统识别书并提供上下文信息。

+   实际使用发现，AI模式用户搜索内容更长，多用于复杂任务。

+   谷歌此次升级，提升了AI搜索能力，拓展了应用场景。

> 关键词：谷歌，AI搜索，视觉搜索，Gemini，多模态

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/bNND9a0naOdixLNR79yMmQ)

> 4️⃣

> 🤖

> 一文搞懂大模型文本到SQL（Text2SQL）

>

+   Text2SQL技术能让非技术用户通过自然语言与数据库交互，提升操作便捷性与效率。

+   主流Text2SQL数据集如Spider、WikiSQL和CHASE，可训练评估模型转换能力。

+   技术实现方式：基于Text2SQL模型结合业务微调，如SQLCoder模型；基于开箱即用的Text2SQL Agent整合到应用，如DB - GPT - Hub。

+   DB - GPT - Hub利用LLMs实现Text - to - SQL解析，降低训练成本，提高准确度。

+   多种技术助力围绕数据库构建大模型应用更简单。

> 关键词：Text2SQL，大模型，数据集，技术实现，DB - GPT - Hub

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/W9mxWU4b7Qsj5F0CDp66ZA)

> 5️⃣

> 🤖

> [CVPR 2025｜超强异常检测新方法！INP](https://mp.weixin.qq.com/s/kTOpzHErGcX28L6LmP2pYA)-Former 从单张图像中提取正常模式

>

+   清华和华科团队提出异常检测新方法INP-Former，从单张测试图像提取正常原型并重建实现检测。

+   核心创新包括独特的INP提取器、INP指导重建框架及损失函数优化。

+   在多个数据集上性能卓越，在少样本、单类别及零样本检测任务中表现出色。

+   该方法泛化、计算效率、鲁棒性强。

+   未来计划优化INP提取利用，结合先验知识提升复杂异常检测能力。

> 关键词：异常检测，INP-Former，CV，图像重建，泛化能力

>

> 🔗 文中引用的链接

+   [论文地址](https://arxiv.org/pdf/2503.02424)

+   [项目地址](https://github.com/luow23)

> 6️⃣

> 🤖

> [1比特KV量化，10倍吞吐提升无损性能：多模态适用的KV ca](https://mp.weixin.qq.com/s/UvY-wMEts4u5L1psvLSRfg)che量化策略来了，即插即用无需改原模型

>

+   提出CalibQuant，一种1比特多模态大模型KV cache量化方案，结合后缩放和校准方法，降低显存与计算成本。

+   该方案在通道维度量化基础上，优化反量化计算顺序与注意力权重，即插即用、无缝集成。

+   在InternVL-2.5模型上实现10倍吞吐量提升，模型性能几乎无损失。

+   在LLaVA和InternVL等模型不同任务测试中，多数指标优于其他方法。

+   解决多模态大模型KV cache量化难题，提升解码效率。

> 关键词：多模态大模型，KV cache量化，1比特量化，吞吐量提升，CalibQuant

>

> 🔗 文中引用的链接

+   [论文地址](https://arxiv.org/abs/2502.14882)

+   [代码地址](https://github.com/insuhan/calibquant)