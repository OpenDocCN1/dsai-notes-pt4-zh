
# 12🈷️10日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Y1iXdI9XroGM32xoTYDcHjzhnne](https://v11enp9ok1h.feishu.cn/docx/Y1iXdI9XroGM32xoTYDcHjzhnne)

> 🔔AI情报助手早报「12月10日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ 🌏 DeepSeek 梁文锋入选 Nature 2025 年度十大人物

刚刚，DeepSeek 梁文锋入选 Natu...

+ 🚀 DeepSeek 创始人梁文锋入选《自然》2025 年度十大人物，获称“科技颠覆者”，其开源推理模型 R1 以 1/10 成本对标美国顶尖模型，带动全球开源潮。
+ 🧠 R1 是首个开放权重的推理 LLM，擅长数学/编程复杂任务，训练成本远低于 Meta Llama3 405B，且公开技术细节供同行参考。
+ 💻 DeepSeek 储备 1 万块英伟达 GPU，采用扁平化架构，重视潜力（如 R1 论文作者含中学生），梁文锋深度参与研究。
+ 📱 R1 融入中国生活：政府用其运营热线、填表，🛰️ 端日活千万，成为中国 AI 从模仿到原创的象征。
+ 🎯 公司计划永久开源下一代模型 R2（因硬件/数据问题推迟），榜单另有中科院杜梦然入选“深潜者”。

关键词：梁文锋，DeepSeek R1，Nature 年度十大人物，开源 LLM，AI 创新

🔗 文中引用的链接

+ [原文章](https://mp.weixin.qq.com/s/Lv6MdCoHiNbs0Ck-IFvpaQ)
+ Nature's 10 2025 专题
+ [梁文锋相关报道](https://www.nature.com/articles/d41586-025-03845-4)

## 2️⃣ 🪘 Agent 微调复活？英伟达开源 8B 新模型带飞 GPT

[Agent微调复活？英伟达开源8B新模型带飞GPT](https://mp.weixin.qq.com/s/AuFGz3jTQZU29MZRNGqgyA)-5：在 HLE 狂卷 37 分，还把成本打下来

+ 🪘 英伟达与香港大学提出 Orchestrator-8B 小模型（指挥家），通过强化学习解决提示词系统的自增强/他增强偏见，协调工具与大模型分工合作。
+ 🎯 ToolOrchestra 框架统一工具为 JSON 接口，多轮编排 + GRPO 强化学习三重奖励（正确性、效率、用户偏好）提升效果与效率。
+ 🏆 Orchestrator-8B 在 HLE/FRAMES/τ²-Bench 测试中超过 GPT-5，成本仅对手三成或省 2.5× 计算成本。
+ 🌐 可落地于企业内部问答、研发调研、Agent 工作流等场景，实现稳定可控的任务调度。
+ 🔓 论文、代码、模型、数据全部开源，方便学界与产业跟进研究与应用。

关键词：Orchestrator-8B，ToolOrchestra，强化学习，多工具协调，大模型调度

🔗 文中引用的链接

+ [论文](https://arxiv.org/abs/2511.21689)
+ [代码](https://github.com/NVlabs/ToolOrchestra/)
+ [模型](https://huggingface.co/nvidia/Orchestrator-8B)
+ [数据](https://huggingface.co/datasets/nvidia/ToolScale)

## 3️⃣ 🪴 当我们在📕聊 AI 时，我们在聊什么...

2025 年📕科技内容爆发，AI 创业者（如 Macaron AI、Plan Coach）通过社区评论迭代产品，平台成为 AI 创新创业新土壤🌱。

+ AMA 互动形式在📕走红，帮助内向科研学者拉新，也成为产品讨论空间，用户自发提建议甚至帮开发者回答问题🔍。
+ AI 时代开发速度加快：独立开发者可快速重构、用投票定功能优先级，Agent 工程师成稀缺岗位（需技术+产品+提示词能力）⚡️。
+ 📕 科技社区氛围独特：拒绝焦虑、保持“活人感”，用户与产品趋向“共建”，开发者能验证 PMF 并寻找创业伙伴👥。
+ 超级个体建议：早抛产品验证需求、做自己喜欢的东西、组队创业，团队应小而敏捷以适应 AI 技术快速迭代🎯。

关键词：AI 创业，📕 生态，产品迭代，开发速度，社区共建

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/9_v_3qCdHA1dOrb6RGLNHg)

## 4️⃣ 🔮 清华&快手团队提出 SVG：既能语义对齐，又能细节拉满，重新定义 LDM 特征提取器

[清华&快手团队提出 SVG：既能语义对齐，又](https://mp.weixin.qq.com/s/mri4Y47Msi00WP8j5UK1Rg)能细节拉满，重新定义 LDM 特征提取器

+ 清华&快手团队提出 SVG，以冻结 DINO 做主特征 + 轻量残差分支补细节，替代 VAE 解决 LDM 语义纠缠、训练低效等问题。
+ SVG 架构含冻结 DINOv3 编码器 + 残差分支（拼接特征）及 ViT 解码器，兼顾语义结构与细粒度细节（残差分支提升重建质量）。
+ SVG 训练分两阶段：先训练残差编码器和解码器（重建损失），再训练扩散模型（Flow Matching + QK-Norm 稳定）。
+ 实验显示 SVG 在 ImageNet256×256 少步生成（5-10 步）FID 优于传统 VAE，训练收敛快且 scaling 不饱和。
+ SVG 特征语义分离，支持零样本编辑，可作为统一视觉生成/感知/理解的基础模型特征空间。

关键词：SVG，Latent Diffusion Model，DINO，残差分支，少步生成

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/mri4Y47Msi00WP8j5UK1Rg)
+ [论文地址](https://arxiv.org/pdf/2510.15301)
+ [项目主页](https://howlin-wang.github.io/svg/)

## 5️⃣ 🔔 博士申请｜南京大学 PCA Lab 招收 2026 入学博士生

南京大学 PCA Lab 面向自动驾驶、具身智能、视觉生成方向招收 2026 年入学博士生，欢迎科研能力强且对前沿方向有热情的同学加入～

+ 课题组科研资源丰富（承担国家级项目 + 与华为等企业深度合作）、2025 年发表 40 余篇 CCF A 类论文、博士生有企业实习机会强化工程科研结合🎯
+ 申请要求为相关方向发表过 CCF A 类论文，且对自动驾驶等方向有长期投入意愿📌
+ 师资团队含谢晋教授等多位学者，每位老师均有个人学术主页供参考🤝
+ 有意向者可发邮件至 yingtai@nju.edu.cn 联系，期待优秀同学加入🌈

关键词：南京大学 PCA Lab，博士生招生，自动驾驶，具身智能，视觉生成
