# 2🈷️24日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Z1sxdGmNcodfFJxpwcbcxspRnLc](https://v11enp9ok1h.feishu.cn/docx/Z1sxdGmNcodfFJxpwcbcxspRnLc)

> 🔔AI情报助手早报「02月24日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🔥

> [神仙打架！OpenClaw 惨遭封杀，Gemini 3.1 推理](https://mp.weixin.qq.com/s/wnPatpxBJNSiSPq3y-5KBQ)翻倍，Sonnet 4.6 以小博大！| AI Weekly 2.16-2.22

>

+   谷歌Gemini3.1 Pro推理得分从31.1%跃至77.1%，API价格仅为Opus4.6一半，但办公场景评估仍落后Claude。

+   Anthropic Sonnet4.6上下文翻倍至100万tokens，定价不变，办公任务反超Opus4.6成为默认模型。

+   阿里Qwen3.5激活170亿参数（总3970亿），推理吞吐量提升19倍，API低至0.8元/百万token，多项基准超GPT5.2和Claude4.5。

+   OpenAI招揽OpenClaw创始人押注个人智能体，同时OpenClaw因4万+漏洞实例遭多家企业封杀。

+   xAI Grok4.2用4个智能体协作，幻觉率降65%，支持基于用户反馈持续迭代。

> 关键词：大模型更新，AI智能体，安全漏洞，推理性能，多模态

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/wnPatpxBJNSiSPq3y-5KBQ)

> 2️⃣

> 🔍

> ThinkingwithImages为什么（不）work？

>

+   复现Thinking with Images工作发现，去掉interleaved images对模型benchmark表现影响小，该结果与arXiv2510.23482报告一致。

+   注意力rollout显示模型更多关注输入图像，且生成interleaved images前已得答案，先验匹配事实助力推理，否则易产生幻觉。

+   用deepeyes RL数据做纯文本SFT可匹配性能，arXiv2511.22586也指出纯文本RL能达到类似效果，说明先验知识更关键。

+   近期VLM中物体scale影响不大，感知基准困难在长上下文干扰，建议用Vicrop/Hide等仅引入目标物体crop的方法优化context。

+   未来方向包括调用外部工具（如图像搜索）、纯视觉反馈（如video tracking）及带反馈的grounding任务。

> 关键词：Thinking with Images，VLM，先验知识，幻觉，context management

>

> 🔗 文中引用的链接

+   [arXiv2510.23482](https://arxiv.org/abs/2510.23482)

+   [arXiv2510.17771](https://arxiv.org/abs/2510.17771)

+   [arXiv2511.22586](https://arxiv.org/abs/2511.22586)

+   [arXiv2510.00054](https://arxiv.org/abs/2510.00054)

+   [arXiv2502.17422](https://arxiv.org/abs/2502.17422)

+   arXiv2511.05271v2

+   [arXiv2601.10611](https://arxiv.org/abs/2601.10611)

+   [arXiv2509.23746](https://arxiv.org/abs/2509.23746)

> 3️⃣

> 🎯

> 无需微调：模型如何主动利用上下文实现持续...

>

+   针对现有长上下文方法被动、无法积累经验及性能成本问题，提出Agentic Context Engineering（ACE）范式，将上下文视为可演化的学习与记忆载体。

+   ACE通过生成-反思-筛选循环机制，自动识别高价值经验并凝练为可复用上下文剧本，无需更新模型参数即可实现推理时学习与持续适应。

+   该工作在多智能体及领域任务（长期任务执行、跨任务迁移等）中评估，ACE性能稳定优于强基线，且推理成本和上下文开销更高效。

+   相关论文《Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models》已被ICLR 2026接收，论文地址为

+   https://arxiv.org/abs/2510.04618

+   。

+   斯坦福博士生张奇正将在NICE学术直播分享该研究，直播覆盖多时区，平台包括🛰️视频号、B站、Youtube。

> 关键词：ACE，长上下文学习，持续学习，推理时学习，ICLR2026

>

> 🔗 文中引用的链接

+   [原文章链接](https://mp.weixin.qq.com/s/nmoDC--v0wpmvffkUnbXqQ)

+   [论文地址](https://arxiv.org/abs/2510.04618)

+   [嘉宾张奇正个人主页](https://alex-q-z.github.io/)

+   [NICE主页](https://nice-nlp.github.io)

+   [NICE海外主页](https://nice-intl.github.io)

+   [NICE B站空间](https://space.bilibili.com/507524288)

+   [NICE Youtube频道](https://www.youtube.com/@niceaitalk)

> 4️⃣

> 🔮

> 深度｜Gemini 3预训练负责人揭秘G...

>

+   Gemini3的巨大提升源于团队协作与多维度改进，采用基于Transformer的混合专家架构，核心是分离计算量与参数规模。

+   行业从“数据无限”转向“数据有限”范式，合成数据需谨慎使用，架构改进可减少数据需求。

+   规模是预训练性能提升的重要因素，但架构和数据创新的重要性更甚，长上下文能力等方向值得关注。

+   Sebastian Bourgeaud强调评估在预训练中的关键与难度，曾参与Retro项目（检索增强模型）。

+   预训练团队需整合多方面工作，研究品味（协作性、控制复杂性）对研究成功至关重要。

> 关键词：Gemini3，混合专家架构，数据有限范式，预训练评估，Retro项目

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/sXBek7aOrpSNl4YBK0qQGA)

+   [原视频链接](https://www.youtube.com/watch?v=cNGDAqFXvew)

> 5️⃣

> 🔥

> 谷歌突发Gemini3.1Pro！首次采...

>

+   谷歌2026年2月21日发布Gemini3.1Pro，首次用“.1”小版本号，推理性能较3Pro翻倍，ARC-AGI-2验证分数达77.1%。

+   支持1M上下文（知识截止2025年1月），多模态生成、语义理解、代码能力同步增强，Arena评测分数较3Pro高13分。

+   可生成互动可视化内容（如航空仪表盘）、模拟城市、3D椋鸟群飞交互效果，网友用其制作《我的世界》场景、个人网站等。

+   已在Gemini应用和API上线，Google AI Pro/Ultra用户可在NotebookLM使用该模型。

+   定价与3Pro持平，成本效益更优，推动大模型向帕累托前沿（最小成本最大化性能）发展。

> 关键词：Gemini3.1Pro，推理性能翻倍，多模态增强，成本优化，大模型升级

>

> 🔗 文中引用的链接

+   [谷歌官方博客](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-1-pro/)

+   DeepMind Gemini Pro页面

> 6️⃣

> 🔔

> [喝点VC｜a16z：广告是AI产品走向十亿用户](https://mp.weixin.qq.com/s/qXyp0pcZb627QuZkG7JVCA)的最佳方式，细数大模型的七种潜在变现路径

>

+   a16z合伙人Bryan Kim认为广告是AI触达十亿用户的最佳方式，可覆盖生态成本，需融入体验而非破坏。

+   多数AI用户用于个人生产力场景（如写邮件），订阅模式难覆盖所有用户，广告是规模化关键。

+   大模型七种变现路径：搜索/意图广告、上下文广告、联盟电商、游戏、目标竞价、陪伴订阅、按token定价。

+   OpenAI计划为免费用户推广告，标注为赞助内容，未来或整合agent端到端购买能力。

+   互联网历史证明广告是平台核心变现方式，AI变现模式待完善，但广告终将占据重要位置。

> 关键词：AI广告，大模型变现，LLM，订阅模式，联盟电商

>

> 🔗 文中引用的链接

+   [原文链接](https://a16z.com/putting-ads-in-ai/)

+   [🛰️文章链接](https://mp.weixin.qq.com/s/qXyp0pcZb627QuZkG7JVCA)