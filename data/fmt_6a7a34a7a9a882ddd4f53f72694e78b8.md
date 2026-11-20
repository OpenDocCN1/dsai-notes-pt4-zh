
# 6🈷️14日

> 来源：https://v11enp9ok1h.feishu.cn/docx/WpXadMmjdoYk3JxUGfLcBxftn6b
>
> 🔔AI情报助手早报「06月14日」
>
> ✨✨✨✨✨✨✨✨

## 1️⃣ Triton编程：vLLM Merge Attention States Kernel

- 文章介绍vLLM中Triton Merge Attention States Kernel实现，相比PyTorch原生实现，该Triton kernel最高可实现3 - 5倍以上算子加速。
- 阐述Merge Attention States概念，即对分块Attention进行最终校准，在多种场景有意义。
- 对比PyTorch实现与Triton基础算子，Triton将操作融合到一个kernel，性能更高。
- 分析Triton算子存在的问题及生成的PTX代码，指出可通过手写CUDA算子优化访存。
- 性能评估显示Triton Kernel能减少访存开销，提升kernel性能。

关键词：vLLM，Triton，Merge Attention States，算子加速，CUDA

### 🔗 文中引用的链接

- [https://zhuanlan.zhihu.com/p/1904937907703243110](https://zhuanlan.zhihu.com/p/1904937907703243110)
- [https://zhuanlan.zhihu.com/p/1892966682634473987](https://zhuanlan.zhihu.com/p/1892966682634473987)
- [https://github.com/xlite-dev/LeetCUDA](https://github.com/xlite-dev/LeetCUDA)
- [https://zhuanlan.zhihu.com/p/1902778199261291694](https://zhuanlan.zhihu.com/p/1902778199261291694)
- [https://zhuanlan.zhihu.com/p/1899562146477609112](https://zhuanlan.zhihu.com/p/1899562146477609112)
- [https://zhuanlan.zhihu.com/p/695799736](https://zhuanlan.zhihu.com/p/695799736)
- [https://www.arxiv.org/pdf/2501.01005](https://www.arxiv.org/pdf/2501.01005)
- [https://github.com/sgl-project/sglang/pull/5428](https://github.com/sgl-project/sglang/pull/5428)
- [https://github.com/vllm-project/vllm/blob/main/vllm/attention/ops/triton_merge_attn_states.py](https://github.com/vllm-project/vllm/blob/main/vllm/attention/ops/triton_merge_attn_states.py)

## 2️⃣ ListenHub 深度探索模式上线

- ListenHub 1.3.0更新，上线深度探索模式，播客输出时长超10分钟，内容更透彻。
- 采用世界上最好的模型及用心的AI Agent链路制作消费级音频内容。
- 播客生成设置优化，语言和音色选择合并进输入框，修改更便捷。
- iOS同步支持「深度探索」模式及相关更新，版本审核中。
- 此次更新围绕深度探索模式及播客生成设置，提升用户体验。

关键词：ListenHub，深度探索模式，AI Agent，播客生成设置，iOS更新

### 🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/Ug_RL3cO6IDhxXu1fmbqBg)

## 3️⃣ NICE69期 | 安全/可信论文分享

- 6月18日NICE将举办安全/可信论文专题分享，涉及ICML和ACL 2025年多篇论文。
- 论文聚焦大语言模型安全，如隐私记忆、微调安全、长文本安全等问题。
- 提出AGrail、Safe Delta等方法增强模型安全性，提升防御攻击能力。
- 探讨大模型知识遗忘、越狱攻击迁移性及事实性表达等研究方向。
- 多位来自知名高校的研究者将分享其相关研究成果。

关键词：大语言模型，安全，可信，论文分享，隐私

### 🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/fXDA9ciaRwGJ49wWe-9_vQ)
- [b站直播间](https://live.bilibili.com/27784098)
- [NICE主页](https://nice-nlp.github.io)
- [NICE海外](https://nice-intl.github.io)
- [b站](https://space.bilibili.com/507524288)
- [Youtube](https://www.youtube.com/@NLPAcademicExchangePlatform)

## 4️⃣ 今日开源（2025-06-12）：VJEPA2 等开源项目

- VJEPA2-AC世界模型利用VJEPA2自监督编码器及互联网规模数据，训练机器人运动理解。
- E²GraphRAG框架提高图检索增强生成效率，GreaterPrompt工具包优化提示提升模型性能。
- Crab统一音视频场景理解模型在CVPR 2025被接受，提供多任务解决方案。
- ContentV框架加速视频生成模型训练，GiGL开源库专注超大规模图神经网络训练推理。
- 这些项目展示了AI在机器人操作、音视频理解、模型训练等方面的开源进展。

关键词：AI，开源项目，世界模型，图神经网络，视频生成

### 🔗 文中引用的链接

- [VJEPA2一键收藏](https://sota.jiqizhixin.com/project/vjepa2)
- [E²GraphRAG一键收藏](https://sota.jiqizhixin.com/project/e-2graphrag)
- GreaterPrompt一键收藏
- [Crab一键收藏](https://sota.jiqizhixin.com/project/crab)
- [ContentV一键收藏](https://sota.jiqizhixin.com/project/contentv)
- [GiGL一键收藏](https://sota.jiqizhixin.com/project/gigl)

## 5️⃣ AI都开始“教课”了？秘塔这波操作真的有...

- 秘塔AI新产品「今天学点啥」可用于知识学习，能上传论文、提问，对接AI研究成果。
- 可让AI按选定页数、风格和深度“讲解”知识，自动生成PPT、逐字稿与语音讲解，还能导出。
- 上传资料后，能帮用户分解知识点，梳理知识脉络，辅助精读综述论文。
- 应用场景广泛，适合知识型、学习型人群，支持AI互动提问。
- 使用时论文选十页内为宜，可对照原文、调倍速，注意多音字读音。

关键词：秘塔AI，今天学点啥，知识学习，AI讲解，应用场景

### 🔗 文中引用的链接

- https://metaso.cn/study

## 6️⃣ 2个人不到1年做到月入百万，这个AI网文...

- 猛哥的AI网文创业项目，2人在不到1年时间实现月入百万，月活2万+，付费率18.44%。
- 选对赛道是关键，网文市场潜力大，大厂看不上细分领域，且AI能力不断提升。
- 极简创业，快速验证，靠社群运营、提示词生态、增长飞轮等打造护城河。
- 构建知识付费多层进阶体系，形成用户、推广者、作者、讲师多方共赢生态。
- 该案例表明时机、用户、生态、正反馈循环对AI应用创业很重要。

关键词：AI网文，创业，社群运营，提示词生态，知识付费

### 🔗 文中引用的链接

- [猛哥的星月写作网站](https://xingyuexiezuo.com/)
- [原文链接](https://mp.weixin.qq.com/s/deJQvoqlvVKe9vTpko3OqQ)
