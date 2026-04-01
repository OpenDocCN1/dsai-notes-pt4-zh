# 05🈷️28日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HoL9dedquoMbnpxfFRScEnFFnPe](https://v11enp9ok1h.feishu.cn/docx/HoL9dedquoMbnpxfFRScEnFFnPe)

> 🔔AI情报助手早报「05月28日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🎮

> 34城联动，AI切磋大会玩疯了！像素风城

>

+   2025年5月26日，「第13期·AI切磋大会」在34个城市联动举办，AI视觉Workshop主题多样，参与者作品精彩。

+   活动不限主题、风格与工具，现场提供AI视频制作教程，还申请到Lovart邀请码。

+   展示各地优秀作品，涵盖像素风城市打卡、创意复刻等多种类型。

+   作品可投稿抖音、剪映联合WaytoAGI社区活动拿奖，相关技巧整理在知识库。

+   WaytoAGI诚邀AI产品团队等合作，感谢活动志愿者。

> 关键词：AI切磋大会，像素风，AI视频制作，作品展示，合作招募

>

> 🔗 文中引用的链接

+   抖音AI新星计划×WaytoAGI

+   WaytoAGI×剪映 AI创意视频征集令

> 2️⃣

> 🤖

> Claude 4被诱导窃取个人隐私！Gi

>

+   瑞士网络安全公司发现，通过在公共仓库隐藏恶意指令，可诱导集成GitHub MCP的Claude 4泄露用户私有仓库敏感数据，GitLab Duo也曝出类似漏洞。

+   触发攻击需用户同时有公私仓库、使用集成GitHub MCP的AI Agent工具且攻击者在公共仓库创建恶意Issue。

+   该漏洞是AI Agent工作流设计缺陷，非GitHub平台漏洞，任何使用GitHub MCP服务器的Agent都可能中招。

+   针对此漏洞，提出动态权限控制和持续安全监测两套防御方案。

+   完整漏洞分析报告可详见相关博客。

> 关键词：Claude 4，GitHub MCP，隐私泄露，AI Agent，防御方案

>

> 🔗 文中引用的链接

+   [1]

+   [h](https://x.com/lbeurerkellner/status/1926991491735429514)ttps://x.com/lbeurerkellner/status/1926991491735429514

+   [2]

+   [h](https://x.com/alexalbert__/status/1861079874385203522)ttps://x.com/alexalbert__/status/1861079874385203522

+   [3]

+   [https://github.com/ukend](https://github.com/ukend0464/pacman/issues/1)0464/pacman/issues/1

+   [博客1](https://invariantlabs.ai/blog/mcp-github-vulnerability#mitigations)

+   [博客2](https://www.legitsecurity.com/blog/remote-prompt-injection-in-gitlab-duo)

> 3️⃣

> 🎯

> 告别卡脖子，华为黑科技破局！昇腾推理加速

>

+   华为诺亚提出Pangu Light框架，打破「剪枝即降智」魔咒，探索LLM新路径。

+   结构化剪枝技术虽能压缩模型，但易致性能下降，Pangu Light通过权重调整与重置解决此问题。

+   其核心技术包括CLAP、SLNP及Post-RMSNorm融合优化策略，软硬协同实现高效推理。

+   实验显示，Pangu Light在压缩率、推理速度和精度上表现卓越，优于部分已有框架。

+   该成果彰显算法创新与昇腾平台融合的潜力，降低大模型应用门槛。

> 关键词：华为，Pangu Light，结构化剪枝，昇腾NPU，LLM

>

> 🔗 文中引用的链接

+   [参考资料](https://arxiv.org/abs/2505.20155)

> 4️⃣

> 🤖

> 谷歌·搜索：献给AI的第一个「祭品」？

>

+   谷歌正用AI重塑搜索，AI Overviews和AI Mode改变传统搜索架构，使谷歌从信息入口变为终点，威胁其广告营收模式。

+   生成式AI崛起，用户搜索习惯改变，谷歌搜索流量流向ChatGPT、DeepSearch等，内部呼吁加快Gemini商业化。

+   AI Overviews触发的零点击行为增多，虽谷歌规避冲击，但相关关键词数量上升，或致广告成本攀升。

+   谷歌搜索市场份额下滑，若移动搜索份额下降，将危及搜索广告垄断地位。

+   生成式AI将重塑科技大厂，颠覆互联网。

> 关键词：谷歌搜索，AI，生成式AI，搜索习惯，商业模式

>

> 🔗 文中引用的链接

+   [https://nymag.com/intelligencer/article/google-](https://nymag.com/intelligencer/article/google-ai-mode-search-results-bury-the-web.html)ai-mode-search-results-bury-the-web.html

+   [h](https://www.ai-supremacy.com/p/googles-slow-death-has-begun)ttps://www.ai-supremacy.com/p/googles-slow-death-has-begun

> 5️⃣

> 🤖

> Meta「轻量级」KernelLLM颠覆

>

+   Meta推出基于Llama 3.1微调的8B参数模型KernelLLM，能将PyTorch代码自动转换为高效Triton GPU内核。

+   其单次推理性能超越GPT-4o和DeepSeek V3，多次生成时得分更高，以小参数实现高性能。

+   该模型通过大量数据训练，在不同难度任务表现稳定，可用新变体KernelBench-Triton评估。

+   安装相关依赖包，调用接口可生成优化代码，也有交互式界面。

+   但它也存在API引用、语法及代码细节处理等问题。

> 关键词：KernelLLM，GPU内核生成，Llama 3.1，性能评估

>

> 🔗 文中引用的链接

+   [https://x.com/reach_vb/status](https://x.com/reach_vb/status/1924478755898085552)/1924478755898085552

+   [https://huggingface.co/dataset](https://huggingface.co/datasets/GPUMODE/KernelBook)s/GPUMODE/KernelBook

+   [https://huggingface.c](https://huggingface.co/facebook/KernelLLM)o/facebook/KernelLLM

> 6️⃣

> 🤖

> One RL to See Them A

>

+   MiniMax提出V -Triune视觉三重统一强化学习系统，使VLM在单一流程中学习视觉推理与感知任务。

+   V -Triune含样本级数据格式化等三个组件，引入动态IoU奖励，在现成框架内实现。

+   训练中采取冻结ViT等针对性调整解决不稳定与扩展性问题。

+   实验显示，Orsta在MEGA -Bench Core及常见下游任务上性能显著提升。

+   结果表明强化学习能在统一框架内增强视觉推理和感知能力。

> 关键词：强化学习，视觉-语言模型，V -Triune，动态IoU奖励，性能提升

>

> 🔗 文中引用的链接

+   [论文地址](https://arxiv.org/pdf/2505.18129)

+   [代码地址](https://github.com/MiniMax-AI)