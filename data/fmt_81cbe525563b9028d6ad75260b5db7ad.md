
# 7🈷️01日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/T279dAemcoNF1zx1gZDcOKI9n1a](https://v11enp9ok1h.feishu.cn/docx/T279dAemcoNF1zx1gZDcOKI9n1a)

> 🔔AI情报助手早报「07月01日」

## 1️⃣ ICML 2025 | 何恺明的“残差连...

彩云科技与北邮团队在ICML 2025论文中，针对Transformer中残差连接的瓶颈，提出多路动态密集连接（MUDD）。

MUDD通过密集化、动态化、多路性设计，给残差连接改造，实现深度方向多头注意力机制。

实验显示，MUDD以极小代价提升性能，28亿参数模型媲美甚至超越高计算量模型。

MUDD还改善了Transformer++层数增加收益递减问题，与MoE架构结合有额外增益。

团队开源了论文、代码和模型。

**关键词**：Transformer，残差连接，MUDD，模型架构，开源

### 🔗 文中引用的链接

- [Github开源地址](https://github.com/Caiyun-AI/MUDDFormer)
- [论文链接](https://arxiv.org/abs/2502.12170)
- [HuggingFace模型1](https://huggingface.co/Caiyun-AI/MUDDPythia-1.4B)
- [HuggingFace模型2](https://huggingface.co/Caiyun-AI/MUDDPythia-2.8B)
- [HuggingFace模型3](https://huggingface.co/Caiyun-AI/MUDDFormer-2.8B)

## 2️⃣ 谷歌杀疯！百万token神器免费开源，C...

谷歌开源AI编程工具Gemini CLI，免费且权限宽松，支持100万token超大上下文窗口，每天千次免费请求。

Gemini CLI与谷歌AI编码助手插件Gemini Code Assist同源，降低编程接口复杂度，使用便捷。

除编程外，还支持生成视频、撰写报告、获取实时信息等多种通用任务。

谷歌此举被视为与OpenAI、Anthropic正面交锋，已获18k星。

谷歌产品高级总监认为此类工具将主导未来十年工作方式。

**关键词**：谷歌，Gemini CLI，开源，AI编程，通用任务

### 🔗 文中引用的链接

- [https://www.bloomberg.com/news/articles/2025-06-25/google-debuts-gemini-ai-coding-tool-in-bid-to-entice-developers](https://www.bloomberg.com/news/articles/2025-06-25/google-debuts-gemini-ai-coding-tool-in-bid-to-entice-developers)
- [https://x.com/googleaidevs/status/1937861646082515205](https://x.com/googleaidevs/status/1937861646082515205)
- [https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/)
- [https://www.developer-tech.com/news/gemini-cli-google-brings-ai-smarts-terminal/](https://www.developer-tech.com/news/gemini-cli-google-brings-ai-smarts-terminal/)
- [https://techcrunch.com/2025/06/25/google-unveils-gemini-cli-an-open-source-ai-tool-for-terminals/](https://techcrunch.com/2025/06/25/google-unveils-gemini-cli-an-open-source-ai-tool-for-terminals/)
- [https://www.linkedin.com/feed/update/urn:li:activity:7343640552640344069/](https://www.linkedin.com/feed/update/urn:li:activity:7343640552640344069/)

## 3️⃣ 零开销，消除图像幻觉！基于零空间投影挖掘...

西安交大团队提出Nullu方法，通过提取「幻觉子空间」并零空间投影编辑模型权重，消除大型视觉语言模型的物体幻觉。

该方法核心是定位正常与幻觉样本特征差异，实现简单、无需训练、零推理开销，在多个任务表现出色。

研究验证了幻觉子空间存在，分析其与现有方法关系及有效性。

在多个数据集验证Nullu有效性，相比其他方法，速度更快、性能更好。

Nullu能在不增推理成本下缓解幻觉，且不损害模型整体能力。

**关键词**：视觉语言模型，物体幻觉，零空间投影，Nullu方法，幻觉子空间

### 🔗 文中引用的链接

- [论文地址](https://arxiv.org/abs/2412.13817)
- [代码地址](https://github.com/Ziwei-Zheng/Nullu)

## 4️⃣ 建筑生破解60年数学悬案，制成「永远同一面朝上」的单稳四面体

1966年数学家提出“均匀单稳态四面体”构想，后否定该猜想，康威猜测不均匀配重的单稳四面体存在但未证明。

2022年，建筑学者阿尔马迪借助计算机穷举搜索，证实了康威猜想，并找到单稳四面体的条件。

团队通过模拟，用碳纤维和碳化钨制成实物模型，经调整后成功实现单稳态。

此成果离不开计算机辅助，反映从手动解题到计算机辅助的转变。

目前团队致力于将成果迁移到航空工作中。

**关键词**：单稳四面体，数学猜想，计算机辅助，航空应用

### 🔗 文中引用的链接

- [1]
- [https://www.quantamagazine.org/a-new-pyramid-like-shape-always-lands-the-same-side-up-20250625/](https://www.quantamagazine.org/a-new-pyramid-like-shape-always-lands-the-same-side-up-20250625/)
- [2]
- [https://news.ycombinator.com/item?id=44381297](https://news.ycombinator.com/item?id=44381297)
- [3]
- [https://www.quantamagazine.org/john-conway-solved-mathematical-problems-with-his-bare-hands-20200420/](https://www.quantamagazine.org/john-conway-solved-mathematical-problems-with-his-bare-hands-20200420/)
- [论文地址](https://arxiv.org/abs/2506.19244)

## 5️⃣ 最低仅需2G显存，谷歌开源端侧模型刷新竞...

谷歌于6月27日凌晨官宣开源Gemma 3n模型，原生支持多模态，在大模型竞技场得分超1300分。

其通过MatFormer架构创新，实现低显存占用，两种型号5B、8B参数，VRAM占用与2B、4B相当。

采用逐层嵌入技术提升模型质量，引入KV缓存共享缩短首个Token生成时间。

语音采用基于USM的音频编码器，视觉采用全新MobileNet-V5-300M视觉编码器。

模型已在谷歌AI Studio等可用，权重可在Hugging Face下载。

**关键词**：谷歌，Gemma 3n，端侧模型，多模态，低显存

### 🔗 文中引用的链接

- [https://developers.googleblog.com/en/introducing-gemma-3n-developer-guide/](https://developers.googleblog.com/en/introducing-gemma-3n-developer-guide/)
- [https://huggingface.co/collections/google/gemma-3n-685065323f5984ef315c93f4](https://huggingface.co/collections/google/gemma-3n-685065323f5984ef315c93f4)

## 6️⃣ 90后清华博士厨房机器人融资数千万，拿下...

90后清华博士陈震创办的享刻智能完成数千万元Pre-A轮融资，投资方阵容豪华。

其LAVA机器人获北京市首张具身智能机器人食品经营许可证，已签千台海外订单。

该机器人能炸薯条、做汉堡，制作效率高、能耗低，还将围绕“三机一体”升级。

享刻智能与清华珠三角研究院共建联合研发平台，已累计完成亿元级融资。

团队成员经验丰富，公司专注垂直场景，有望推动机器人产业发展。

**关键词**：厨房机器人，融资，具身智能，技术升级，商业化

### 🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/ArubdbSxxBSDmNGWWYWQmw)
