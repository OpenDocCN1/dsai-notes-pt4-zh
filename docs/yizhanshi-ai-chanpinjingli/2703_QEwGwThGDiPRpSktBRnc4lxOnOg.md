# 12🈷️10日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Y1iXdI9XroGM32xoTYDcHjzhnne](https://v11enp9ok1h.feishu.cn/docx/Y1iXdI9XroGM32xoTYDcHjzhnne)

> 🔔AI情报助手早报「12月10日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🌏

> 刚刚，DeepSeek梁文锋入选Natu...

>

+   🚀 DeepSeek创始人梁文锋入选《自然》2025年度十大人物，获称“科技颠覆者”，其开源推理模型R1以1/10成本对标美国顶尖模型，带动全球开源潮。

+   🧠 R1是首个开放权重的推理LLM，擅长数学/编程复杂任务，训练成本远低于Meta Llama3 405B，且公开技术细节供同行参考。

+   💻 DeepSeek储备1万块英伟达GPU，采用扁平化架构，重视潜力（如R1论文作者含中学生），梁文锋深度参与研究。

+   📱 R1融入中国生活：政府用其运营热线、填表，微信端日活千万，成为中国AI从模仿到原创的象征。

+   🎯 公司计划永久开源下一代模型R2（因硬件/数据问题推迟），榜单另有中科院杜梦然入选“深潜者”。

> 关键词：梁文锋，DeepSeek R1，Nature年度十大人物，开源LLM，AI创新

>

> 🔗 文中引用的链接

+   [原文章](https://mp.weixin.qq.com/s/Lv6MdCoHiNbs0Ck-IFvpaQ)

+   Nature's 10 2025专题

+   [梁文锋相关报道](https://www.nature.com/articles/d41586-025-03845-4)

> 2️⃣

> 🪘

> [Agent微调复活？英伟达开源8B新模型带飞GPT](https://mp.weixin.qq.com/s/AuFGz3jTQZU29MZRNGqgyA)-5：在HLE狂卷37分，还把成本打下来

>

+   🪘英伟达与香港大学提出Orchestrator-8B小模型（指挥家），通过强化学习解决提示词系统的自增强/他增强偏见，协调工具与大模型分工合作。

+   🎯ToolOrchestra框架统一工具为JSON接口，多轮编排+GRPO强化学习三重奖励（正确性、效率、用户偏好）提升效果与效率。

+   🏆Orchestrator-8B在HLE/FRAMES/τ²-Bench测试中超过GPT-5，成本仅对手三成或省2.5×计算成本。

+   🌐可落地于企业内部问答、研发调研、Agent工作流等场景，实现稳定可控的任务调度。

+   🔓论文、代码、模型、数据全部开源，方便学界与产业跟进研究与应用。

> 关键词：Orchestrator-8B，ToolOrchestra，强化学习，多工具协调，大模型调度

>

> 🔗 文中引用的链接

+   [论文](https://arxiv.org/abs/2511.21689)

+   [代码](https://github.com/NVlabs/ToolOrchestra/)

+   [模型](https://huggingface.co/nvidia/Orchestrator-8B)

+   [数据](https://huggingface.co/datasets/nvidia/ToolScale)

> 3️⃣

> 🪴

> 当我们在小红书聊 AI 时，我们在聊什么...

>

+   2025年小红书科技内容爆发，AI创业者（如Macaron AI、Plan Coach）通过社区评论迭代产品，平台成为AI创新创业新土壤🌱。

+   AMA互动形式在小红书走红，帮助内向科研学者拉新，也成为产品讨论空间，用户自发提建议甚至帮开发者回答问题🔍。

+   AI时代开发速度加快：独立开发者可快速重构、用投票定功能优先级，Agent工程师成稀缺岗位（需技术+产品+提示词能力）⚡️。

+   小红书科技社区氛围独特：拒绝焦虑、保持“活人感”，用户与产品趋向“共建”，开发者能验证PMF并寻找创业伙伴👥。

+   超级个体建议：早抛产品验证需求、做自己喜欢的东西、组队创业，团队应小而敏捷以适应AI技术快速迭代🎯。

> 关键词：AI创业，小红书生态，产品迭代，开发速度，社区共建

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/9_v_3qCdHA1dOrb6RGLNHg)

> 4️⃣

> 🔮

> [清华&快手团队提出SVG：既能语义对齐，又](https://mp.weixin.qq.com/s/mri4Y47Msi00WP8j5UK1Rg)能细节拉满，重新定义 LDM 特征提取器

>

+   清华&快手团队提出SVG，以冻结DINO做主特征+轻量残差分支补细节，替代VAE解决LDM语义纠缠、训练低效等问题。

+   SVG架构含冻结DINOv3编码器+残差分支（拼接特征）及ViT解码器，兼顾语义结构与细粒度细节（残差分支提升重建质量）。

+   SVG训练分两阶段：先训残差编码器和解码器（重建损失），再训扩散模型（Flow Matching+QK-Norm稳定）。

+   实验显示SVG在ImageNet256×256少步生成（5-10步）FID优于传统VAE，训练收敛快且scaling不饱和。

+   SVG特征语义分离，支持零样本编辑，可作为统一视觉生成/感知/理解的基础模型特征空间。

> 关键词：SVG，Latent Diffusion Model，DINO，残差分支，少步生成

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/mri4Y47Msi00WP8j5UK1Rg)

+   [论文地址](https://arxiv.org/pdf/2510.15301)

+   [项目主页](https://howlin-wang.github.io/svg/)

> 5️⃣

> 🔔

> 博士申请｜南京大学PCA Lab招收2026入学博士生

>

+   南京大学PCA Lab面向自动驾驶、具身智能、视觉生成方向招收2026年入学博士生，欢迎科研能力强且对前沿方向有热情的同学加入～

+   课题组科研资源丰富（承担国家级项目+与华为等企业深度合作）、2025年发表40余篇CCF A类论文、博士生有企业实习机会强化工程科研结合🎯

+   申请要求为相关方向发表过CCF A类论文，且对自动驾驶等方向有长期投入意愿📌

+   师资团队含谢晋教授等多位学者，每位老师均有个人学术主页供参考🤝

+   有意向者可发邮件至yingtai@nju.edu.cn联系，期待优秀同学加入🌈

> 关键词：南京大学PCA Lab，博士生招生，自动驾驶，具身智能，视觉生成

>