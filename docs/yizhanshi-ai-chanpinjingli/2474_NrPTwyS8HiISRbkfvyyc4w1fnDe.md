# 04🈷️13日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/QmSddIYMnon2BCxy0slclgd8nQb](https://v11enp9ok1h.feishu.cn/docx/QmSddIYMnon2BCxy0slclgd8nQb)

> 🔔AI情报助手早报「04月13日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> [4090玩转大场景几何重建，RGB渲染和几何精度达](https://mp.weixin.qq.com/s/-xTYXeu_jz3qESHixYrQSA)SOTA｜上海AI Lab&西工大新研究

>

+   上海AI Lab和西工大提出CityGS-X，突破传统三维重建算力与精度限制。

+   构建并行化混合分层三维表征架构，训练速度提升一倍。

+   提出动态分配锚点并行机制，4卡4090可处理大场景。

+   采用渐进式联合训练方法，RGB渲染和几何精度达SOTA。

+   实验表明CityGS-X在重建精度、训练速度等方面表现出色。

> 关键词：几何重建，CityGS-X，并行机制，联合训练，SOTA

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/pdf/2503.23044)

+   [项目主页](https://lifuguan.github.io/CityGS-X/)

+   [项目代码](https://github.com/gyy456/CityGS-X)

> 2️⃣

> 🤖

> 下周三！20余位行业大佬共话AI，中国AIGC产业峰会最全攻略在此

>

+   第三届中国AIGC产业峰会将于4月16日在北京举办，20余位行业大佬将共话AI。

+   亮点一为场景爆发奇点，各行业代表将分享AI应用实践，还设有圆桌对话。

+   亮点二是技术底座革命，探讨从算力到安全可控的技术难题。

+   亮点三为AI全景生态，将颁出两类奖项并发布应用全景图谱。

+   峰会开启报名，可线下参会或线上直播。

> 关键词：AIGC产业峰会，AI应用，技术底座，全景生态，参会报名

>

> 🔗 文中引用的链接

+   [报名链接](https://mp.weixin.qq.com/s/RSg-rFA_ghM2aqsE5GtVAg)

> 3️⃣

> 🤖

> 刚刚，英伟达被曝投资Ilya新创企：0收入0产品，估值已超2000亿元

>

+   谷歌和英伟达投资OpenAI前首席科学家Ilya的创企SSI，该公司专注开发安全AI系统，尚无收入与产品。

+   SSI去年9月获10亿美元融资，今年3月完成20亿美元新一轮融资，估值超300亿美元。

+   SSI主要使用谷歌TPU研发，谷歌向其售芯片体现扩大外部销售战略。

+   亚马逊也在构建芯片，Anthropic既用亚马逊芯片也继续使用TPU。

+   云厂商掀起AI创企投资风潮，投资者关注专注基础AI研究的创企。

> 关键词：AI创企，投资，芯片，云厂商，安全AI系统

>

> 🔗 文中引用的链接

+   刚刚，英伟达被曝投资Ilya新创企：0收入0产品，估值已超2000亿元

> 4️⃣

> 🎮

> AI封神了！无剪辑一次直出60秒《猫和老鼠》片段，全网百万人围观

>

+   加州大学伯克利分校等机构联合制作的《猫和老鼠》AI短片火了，该短片由模型一次直出，无二次编辑。

+   为实现逼真效果，利用TTT层增强预训练Transformer并微调，还开发“片上张量并行”算法。

+   此前视频生成技术受长上下文限制，本文用TTT层应对挑战，其隐藏状态为神经网络。

+   测评中，TTT层生成视频更连贯，但也存在时间一致性、运动自然性和美学方面瑕疵。

+   数据集基于《猫和老鼠》动画片，强调复杂故事，有望转移到通用视频生成。

> 关键词：AI，视频生成，TTT层，Transformer，《猫和老鼠》短片

>

> 🔗 文中引用的链接

+   [项目主页](https://test-time-training.github.io/video-dit/)

+   [论文地址](https://arxiv.org/pdf/2504.05298)

> 5️⃣

> 🤖

> 我花了5分钟，成功部署阿里QwQ-32B，秒出的感觉太爽了

>

+   作者5分钟在本地电脑成功部署阿里QwQ-32B大模型，采用Ollama+Chatbox方案，仅需一台电脑和24G显卡。

+   QwQ-32B效果比肩DeepSeek-R1 671B满血版，但参数小20倍，不过使用中存在卡、慢等问题。

+   部署时Ollama拉取模型选32b-q4_K_M版本，因3090显卡24G显存限制。

+   安装Chatbox作为前端交互工具，配置后可与模型对话。

+   经测试，本地模型普通难度问题与官网无太大区别，复杂任务因显存不足会中断。

> 关键词：QwQ-32B，大模型部署，Ollama，Chatbox

>

> 🔗 文中引用的链接

+   [Ollama官网](http://www.ollama.com)

+   [Ollama的QWQ模型页面](https://ollama.com/library/qwq)

+   [Chatbox网站](https://chatboxai.app/zh)

> 6️⃣

> 🤖

> [Kimi-VL 开源](https://mp.weixin.qq.com/s/3WVvGU-ssapjVgWlPCKbRg)

>

+   Kimi开源两个MoE视觉理解大模型，总参数16.4B，激活参数2.8B，上下文长度128K。

+   模型架构由MoE语言模型、MoonViT和MLP映射层组成，预训练分4阶段，后训练分3阶段。

+   在榜单上多数超过Qwen2.5-7B模型。

+   给出模型使用代码示例。

+   旨在促进NLP、机器学习领域交流进步。

> 关键词：Kimi-VL，MoE视觉理解大模型，开源，NLP，机器学习

>

> 🔗 文中引用的链接

+   [Github](https://github.com/MoonshotAI/Kimi-VL)

+   [Paper](https://github.com/MoonshotAI/Kimi-VL/blob/main/Kimi-VL.pdf)