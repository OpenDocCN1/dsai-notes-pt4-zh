
# 05🈷️13日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Tc6Td1GlroR7nrxcPmxc3RTrnth](https://v11enp9ok1h.feishu.cn/docx/Tc6Td1GlroR7nrxcPmxc3RTrnth)

> 🔔AI情报助手早报「05月13日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ [AI也需要“记笔记”：Karpathy从Claude 1.6万字提示词中看到的未来](https://mp.weixin.qq.com/s/RcnTih0hJz7-F4kR1epo7Q)

- Claude新系统提示词泄露，长达16739个单词，含工具定义、用户偏好等内容，且有临时修改痕迹。
- Karpathy受其启发，提出“系统提示学习”，认为LLM应像人类“记笔记”，存储策略经验，而非仅依赖参数更新。
- 该学习方式具更高效利用数据、更强泛化能力等优势，但也面临自动编辑优化等问题。
- 与AI交流要明确指令、善用结构化表达，“提示工程”普通人也能掌握。
- “系统提示学习”目前可部分半自动实现。

> 关键词：LLM，系统提示学习，Claude，提示工程

> 🔗 文中引用的链接

- [Claude提示词内容](https://github.com/asgeirtj/system_prompts_leaks/blob/main/claude.txt)
- [Karpathy相关动态](https://x.com/karpathy/status/1921368644069765486)
- [相关文章](https://www.dbreunig.com/2025/05/07/claude-s-system-prompt-chatbots-are-more-than-just-models.html)
- 理解Claude系统提示内容的网页

## 2️⃣ 国产大模型「五强争霸」，决战AGI！

- 中国基础大模型市场形成字节、阿里、阶跃星辰、智谱、DeepSeek「五强」格局。
- 五强要么有钱，要么有人，比拼的是模型能力，或全才或专才。
- 各强特色鲜明，如阿里开源王者，字节综合能力强，阶跃星辰多模态领先等。
- 竞争焦点转向追求更高「智能上限」和突破「多模态能力」。
- 五强的全面博弈，引领中国AI逼近世界前沿，终点或为AGI曙光。

> 关键词：国产大模型，五强格局，AGI，智能上限，多模态能力

> 🔗 文中引用的链接

- https://qwenlm.github.io/zh/blog/
- https://seed.bytedance.com/zh/
- https://platform.stepfun.com/
- https://www.deepseek.com/
- https://zhipuai.cn/
- https://chat.z.ai

## 3️⃣ 全球首次实测通过！CMU华人用AI设计的乐高「不翻车」

- CMU研究人员提出LegoGPT，通过微调Meta的LLaMA模型结合超47000个稳定结构数据集，能将文本输入转化为物理稳定的乐高设计，成功率达98.8%。
- 该模型经物理分析和软件工具验证稳定性，采用回滚方法处理不稳定组件。
- 数据集、代码和模型均已开源，其技术可用于设计3D打印组件。
- 目前LegoGPT存在空间和积木类型限制，团队计划扩展积木库和训练数据集。
- 作者Kangle Deng和Ruixuan Liu均为CMU博士生。

> 关键词：LegoGPT，AI设计乐高，物理稳定，开源，局限性

> 🔗 文中引用的链接

- [论文地址](https://www.arxiv.org/abs/2505.05469)
- [项目地址](https://avalovelace1.github.io/LegoGPT/)
- [参考资料1](https://arxiv.org/html/2505.05469v1)
- [参考资料2](https://arstechnica.com/ai/2025/05/new-ai-model-generates-buildable-lego-creations-from-text-descriptions/)

## 4️⃣ [92ms极速Token响应！VITA团队开源实时语音大模型，推理效率暴增5倍](https://mp.weixin.qq.com/s/WbGvS7rnUWP2PkQPa89ptA)

- VITA团队开源实时语音大模型VITA-Audio，从接收文本到输出首个可解码片段仅需92ms，推理效率比同规模模型提升3 - 5倍。
- 其提出轻量级MCTP模块，减少自回归循环次数，降低首个音频片段生成延迟。
- 采用四阶段渐进式训练策略解决多模块训练不稳定问题。
- 提供四种推理范式，满足不同场景需求。
- 该模型在多项任务中表现卓越，为实时语音生成树立新范式。

> 关键词：实时语音大模型，VITA-Audio，MCTP模块，推理加速，开源

> 🔗 文中引用的链接

- [论文地址](https://arxiv.org/pdf/2505.03739)
- [代码仓库](https://github.com/VITA-MLLM/VITA-Audio)

## 5️⃣ [CVPR 2025 | “点级+提示级”双采样黑科技！MICAS重新定义3D点云上下文学习](https://mp.weixin.qq.com/s/dc0GrkCDKauoOTgVymIMHQ)

- 论文MICAS提出多粒度自适应采样方法，从“点级”和“提示级”提升ICL在3D任务的稳健性与适应性。
- 两大核心模块为任务自适应点采样和查询特定提示采样，通过提示理解、Gumbel采样等步骤实现。
- 实验使用ShapeNet数据集，以CD和mIOU为指标，MICAS在各任务中显著超越现有技术。
- 对比实验和消融研究验证了MICAS的鲁棒性，展示各组件协调作用。
- 模型通过分步骤训练降低复杂度，实现训练和推理可视化。

> 关键词：3D点云，上下文学习，MICAS，双采样，CVPR 2025

> 🔗 文中引用的链接

- [论文链接](https://arxiv.org/abs/2411.16773)
