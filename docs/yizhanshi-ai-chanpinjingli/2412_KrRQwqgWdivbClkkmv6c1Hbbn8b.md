# 02🈷️12日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Ut4RdElNHoe4zVxlWHoczo9GnQd](https://v11enp9ok1h.feishu.cn/docx/Ut4RdElNHoe4zVxlWHoczo9GnQd)

> 🔔AI情报助手午报「02月12日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🧬

> [A](https://mp.weixin.qq.com/s/nS_lal0ZlYvazTuYAIWNqg)AAI 2025 | 中山大学提出AIQViT：面向视觉Transformer的结构感知后训练量化方法

>

+   中山大学提出了一种新的后训练量化（PTQ）方法AIQViT，专为视觉Transformer（ViTs）设计，通过结构感知的低秩补偿机制和动态聚焦量化器来提升量化效率和性能。

+   AIQViT通过引入可学习的低秩权重来补偿权重量化引起的性能下降，并针对Softmax层激活的不平衡分布设计了动态聚焦量化器，以提高量化分辨率。

+   该方法在五个视觉任务上进行了验证，包括图像分类、目标检测等，与现有方法相比，在低比特量化情况下仍能保持较好的性能。

+   通过消融实验，验证了AILoC、DFQ和CL等关键组件的有效性，特别是在Softmax后激活处理和权重量化方面。

+   结论指出，AIQViT通过结构感知低秩补偿和动态注意力量化器，有效提高了ViTs的量化效率和模型泛化能力。

> 关键词：AIQViT，后训练量化，视觉Transformer，模型压缩，动态聚焦量化器

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/abs/2502.04628)

> 2️⃣

> 🪂

> [上海AI Lab、天津大学发布M](https://mp.weixin.qq.com/s/nyevmhHOElP3YMGfTA4DLw)IS数据集，揭示多模态大模型安全微调瓶颈

>

+   上海AI Lab和天津大学发布了MIS数据集，旨在提升多模态视觉语言模型（VLMs）的安全微调能力。

+   研究指出现有安全微调方法在有用性和安全性之间的平衡难以把握，尤其在复杂安全场景下防御失效。

+   通过构建MIS数据集，包括训练集和测试集，研究者们旨在提高模型的安全视觉感知和推理能力。

+   论文《Rethinking Bottlenecks in Safety Fine-Tuning of Vision Language Models》提出了MIRage方法，通过Safety CoT提升模型的安全表现。

+   实验结果表明，MIRage微调后的模型在多图安全能力上有显著提升，且对通用能力无负面影响。

> 关键词：多模态大模型，安全微调，MIS数据集，视觉语言模型，安全视觉推理

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/pdf/2501.18533)

+   [项目主页](https://dripnowhy.github.io/MIS/)

+   [代码链接](https://github.com/DripNowhy/MIS)

+   [数据集链接](https://huggingface.co/collections/Tuwhy/mis-679ae8748aa3744dfb0d453e)

+   [模型链接](https://huggingface.co/collections/Tuwhy/mirage-679875ff7bb3855234c94b42)

> 3️⃣

> 🚀

> Agentic GLM全面登陆三星最新款手机Galaxy S25

>

+   Agentic GLM技术首次登陆三星Galaxy S25系列手机，提供AI音视频通话体验。

+   Galaxy S25系列通过GLM-Realtime技术实现图片和视频输入，具备低延时流式推理能力。

+   支持多轮记忆能力，提供翻译、搜索等智能服务，增强用户交互体验。

+   社交媒体文案帮写功能，基于图片素材自动生成📕、🧣等平台文案。

+   Galaxy S25手机搜索功能调用智谱清言智能体，提供即时、精炼、准确的AI搜索结果。

> 关键词：AI音视频通话，多模态数据处理，低延时推理，社交媒体文案，AI搜索

>

> 🔗 文中引用的链接

+   [Agentic GLM技术介绍](https://mp.weixin.qq.com/s/y59k8SklLescUcNuJnNAnQ)

> 4️⃣

> 🧠

> AI情报助手解读open-r1代码解析

>

+   DeepSeek R1的训练分为两个阶段，第一阶段纯RL，第二阶段SFT+RL，通过蒸馏到小模型提升推理性能。

+   OpenR1项目通过蒸馏复刻R1，使用小模型SFT构造推理思维链数据，数据集来源广泛，包括中国高中数学练习和国际数学奥林匹克竞赛问题。

+   SFT训练中使用了标签平滑技术，以及多种奖励函数来评估模型输出的准确性、格式和推理步骤。

+   GRPO训练中，通过计算模型输出与参考模型输出的KL散度和advantage来优化策略模型。

+   实现了多种奖励函数，包括准确率奖励、格式奖励、推理步骤奖励、长度奖励和重复ngram惩罚。

> 关键词：AI，强化学习，蒸馏，推理思维链，标签平滑

>

> 🔗 文中引用的链接

+   [SFT训练参考](https://github.com/huggingface/transformers/blob/main/src/transformers/trainer_pt_utils.py#L554)

+   [GRPO训练参考](https://github.com/huggingface/trl/blob/main/trl/trainer/grpo_trainer.py#L459)

> 5️⃣

> 🧚🏻‍♀️

> [DeepSeek算力卡脖子，高校A](https://mp.weixin.qq.com/s/-ZmXXRX84JOh_zjWidFRiw)I研究遇瓶颈？华为联合15校给出最强解法

>

+   华为联合15所高校打造科教创新卓越/孵化中心，通过产教融合破解高校科研算力困局。📌

+   AI人才紧缺，全球AI工作岗位增长显著，亚洲增长尤为突出。🌏

+   华为昇腾系列AI处理器和CANN计算架构，助力构建自主完整的国产算力生态。🛠️

+   华为昇思MindSpore深度学习框架，实现国内科研创新和产业应用的自主可控。🧠

+   华为昇腾MindIE推理引擎，提供全栈优化的AI推理解决方案，加速AI与行业融合。🚀

> 关键词：AI算力，产教融合，国产技术栈，深度学习框架，AI推理引擎

>

> 🔗 文中引用的链接

+   [华为昇腾官网](https://www.huawei.com/cn/ict/portals/huawei-ascend)

> 6️⃣

> 🌏

> 李飞飞巴黎峰会开幕演讲：AI正自我觉醒！马克龙AI换脸现场「整活」

>

+   李飞飞博士在巴黎AI行动峰会上强调了AI的自我觉醒和空间智能、具身智能的重要性。

+   法国总统马克龙通过AI生成的视频为峰会造势，强调AI在医疗保健、能源和社会变革中的作用。

+   李飞飞提出了AI以人为本的概念，强调尊严、能动性和社区三大核心价值。

+   AI技术的发展不仅推动了算法和数据的进步，也带来了计算力的飞跃，尤其是在图像和语言模型方面。

+   李飞飞呼吁构建更健康的AI生态系统，强调科学评估AI能力与局限，以及多方协作的重要性。

> 关键词：AI觉醒，空间智能，具身智能，人本AI，AI治理

>

> 🔗 文中引用的链接

+   [参考资料](https://techcrunch.com/2025/02/10/ai-pioneer-fei-fei-li-says-governments-should-embrace-a-pragmatic-attitude-instead-of-an-ideological-one-on-ai/)