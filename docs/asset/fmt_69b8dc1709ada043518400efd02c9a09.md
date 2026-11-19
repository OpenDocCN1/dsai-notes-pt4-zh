
# 3🈷️8日

> 来源：<https://v11enp9ok1h.feishu.cn/docx/DBY6d2cmnobaEIxfq4Wc14IpnNc>

> 🔔AI情报助手日报「03月08日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ 极速推理！清华iDLab提出DACER-F：破解生成式自动驾驶“高延迟”魔咒

> 🚗

清华iDLab提出DACER-F算法，解决生成式自动驾驶策略高延迟问题，融合Flow Matching与Langevin动力学实现突破。

核心机制：用Langevin动力学生成在线RL动态目标，Flow Matching实现单步动作生成，平衡表达力与实时性。

驾驶仿真评测：总平均回报1238，较DACER提升28%、DSAC提升34%，早期碰撞率更低更稳定。

时延优势显著：推理耗时0.28ms（与轻量DSAC相当），比DACER降84%，训练耗时加速3.37×。

泛化性验证：在DMC 6个高维任务中均获最佳TAR，Humanoid-stand任务表现突出。

关键词：DACER-F，自动驾驶，生成式策略，Flow Matching，Langevin动力学

🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/V6lpW4Tb_AxjL1QVI1usrA)
- [arXiv论文地址](https://arxiv.org/abs/2603.02613)

## 2️⃣ NeurIPS 2025 | 当系统算法遇上机器学习：GUARD开启学习增强缓存新范式

> 🔔

针对机器学习增强系统算法预测不准易崩溃的痛点，研究提出GUARD轻量级鲁棒化框架。

GUARD核心：预测准确时保持近最优性能，预测失效时兜底，每请求仅O(1)额外开销。

实验在合成噪声与真实数据集上验证，GUARD表现优于基线，兼顾一致性与强鲁棒性。

相关交叉方向ALPS量化收益风险，推动AI增强系统算法可控落地。

配套开源Benchmark（Cache-Coliseum），支持多种缓存算法测试。

关键词：学习增强缓存，GUARD框架，鲁棒性，ALPS，NeurIPS 2025

🔗 文中引用的链接

- [论文链接](https://arxiv.org/pdf/2507.16242)
- [GitHub源码](https://github.com/OptiSys-ZJU/Cache-Coliseum)
- [原文链接](https://mp.weixin.qq.com/s/b7TuETKslTqLqm4ZYWiwFA)

## 3️⃣ 水滴公司首推「水守AI助手」ClawSquare 构建 Agent 协同办公新范式

> 🤖

2026年3月水滴推出「水守AI助手」ClawSquare，作为开放Agent协作基础设施，打破AI孤岛，实现Agent间任务协同。

采用分布式Agent模式，支持7×24小时无摩擦协作，消除企业低技术沟通损耗（如会议预订、差旅流程）。

推动公司从“使用AI工具”向“AI原生公司”跨越，重构组织基因，沉淀个体Agent进步为集体能力。

已落地核心业务：KEYI.AI核保效率提升80%，AI客服“保小慧”实现语音对话即办理。

计划将Agent办公范式向合作伙伴及行业迁移，践行科技驱动金融医疗服务愿景。

关键词：Agent协作、ClawSquare、AI原生、水滴公司、Agent办公范式

🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/K3RTNLgyHwfZfWW-OabjVQ)

## 4️⃣ GPT-5.4全整理：非常好用，非常贵

> 🤖

OpenAI发布GPT-5.4及Pro版，标准版API输入$2.50/M、输出$15/M，Pro版为12倍定价，面向不同用户群体。

首次在通用模型内置原生Computer Use能力，合并代码、推理、Computer Use三大功能，上下文窗口达1M tokens，知识截止2025年8月31日。

性能显著提升：GDPval评测83%（超5.2的70.9%），电子表格任务提升19%，幻觉出错率较5.2低33%，Computer Use超人类基准（OSWorld-Verified75%>人类72.4%）。

新增Tool Search减少47% token消耗，长上下文支持1M（实验性），学术基准如ARC-AGI-2提升至73.3%，安全防护针对网络安全等领域升级。

ChatGPT逐步开放5.4为默认模型，5.2三个月后退役，合并推理、代码、Computer Use为统一模型出口，降低开发者集成门槛。

关键词：GPT-5.4，大模型，Computer Use，AI能力提升，模型定价

🔗 文中引用的链接

- 官方Blog：`openai.com/index/introducing-gpt-5-4`
- System Card：`deploymentsafety.openai.com/gpt-5-4-thinking`
- ChatGPT for Excel插件：`https://openai.com/index/chatgpt-for-excel/`

## 5️⃣ 全球OpenClaw用量第一的模型，竟然...

> 🤖

国产MiniMax M2.5在OpenRouter总体用量及OpenClaw（发布后统计）居首，获其 creator Peter Steinberger推崇。

主打真实世界工作场景，编程（SWE-Bench Verified 80.2%）、工具调用等达SOTA，速度提升37%且成本低。

实测接入Claude Code/OpenClaw后效率提升，开发知网插件、读书站点等，体现架构师级Spec能力。

提供Coding Plan（含极速版，能力不变速度提升），价格优势明显，适配多语言，暂不支持多模态。

国产AI与国外差距缩小，MiniMax从24年初小团队成长为上市企业，作者看好其未来。

关键词：MiniMax M2.5，OpenClaw，大模型，AI生产力，Spec能力

🔗 文中引用的链接

- OpenRouter排行榜：`https://openrouter.ai/rankings`
- MiniMax Coding Plan订阅：`https://platform.minimaxi.com/subscribe/coding-plan`
- Claude Code接入文档：`https://platform.minimaxi.com/docs/coding-plan/claude-code`

## 6️⃣ Nature｜一个跨越所有生命领域的基因组基础模型，实现基因组建模与设计

> 🧬

Arc等机构联合推出Evo2，在9万亿DNA碱基对上训练，覆盖所有生命领域，含7B/40B参数版本，基于StripedHyena2架构（比Transformer更高效处理长序列）。

作为通用基因语言模型，可零样本预测突变对生物适应度的影响，还能生成完整基因组（线粒体、原核、真核染色体）。

用稀疏自编码器（SAE）解析模型，捕获DNA/RNA/蛋白质等多层面可解释生物学信号。

在人类变异预测（非SNV第一、非编码SNV无监督第一）、染色体模式设计（写入摩斯密码并实验验证）表现亮眼。

开源40B/7B/1B模型及数据集，排除病原体数据保障安全，未来将结合DNA合成与强化学习。

关键词：Evo2，基因组基础模型，StripedHyena2，基因组建模，开源生物模型

🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/lnjU0t4bgyHaejpSyf-Fkw)
- [论文地址](https://www.nature.com/articles/s41586-026-10176-5)
