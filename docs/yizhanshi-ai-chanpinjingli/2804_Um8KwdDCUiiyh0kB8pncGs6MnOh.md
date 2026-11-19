
# 3🈷️24日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/C4H4dsKEfodzCSxPeW5cuTSin5X](https://v11enp9ok1h.feishu.cn/docx/C4H4dsKEfodzCSxPeW5cuTSin5X)
>
> 🔔AI情报助手午报「03月24日」
>
> ✨✨✨✨✨✨✨✨

> ### 1️⃣ 🤖 扩散语言模型总是均匀发力，华为诺亚教它「抓重点」
>
> 华为诺亚针对DLLM训练均匀随机masking问题，提出信息密度感知噪声调度，优先mask代码/数学高信息区域（如关键分支、推理步骤）。
>
> 引入互补masking，同一条数据两种视角训练：抓逻辑骨架+保结构连贯，避免仅重重点丢语言组织能力。
>
> 基于LLaDA-2.0-mini实验，在HumanEval等4基准较基线提升约4%，仅10%数据处理即可获显著性能提升。
>
> 消融实验显示soft mask优于hard mask（避免contextual collapse），过度处理代码数据会影响数学推理泛化。
>
> 后续可探索AST规则提取、自适应mask或端到端对抗式mask等优化方向。
>
> 关键词：DLLM、Masking优化、信息密度感知、华为诺亚、训练范式
>
> 🔗 文中引用的链接
>
> + [论文链接](https://arxiv.org/abs/2603.15803)
> + [数据集链接](https://huggingface.co/datasets/malr07/opc-sft-stage2-dense-extracted)
> + [原文链接](https://mp.weixin.qq.com/s/1yTd36hev6o4OieTdOeYAw)

> ### 2️⃣ 🚀 刚刚，马斯克启动TERAFAB项目：万亿瓦算力工厂，瞄准太空
>
> 马斯克联合SpaceX、Tesla启动TERAFAB项目，目标年产能超1太瓦算力，80%用于太空、20%地面。
>
> 项目依托特斯拉（800万+交付、自动驾驶/Optimus等）、xAI（千兆瓦集群）、SpaceX（Starship运力）协同。
>
> 太空部署AI优势：无限太阳能、天然散热，预计2-3年内成本低于地面。
>
> 奥斯汀晶圆厂实现芯片设计-制造-测试闭环，支撑地面AI→轨道D3→星际扩展分层体系。
>
> 长期构想延伸至月球电磁发射器，目标PW级算力，构建星际文明基础设施。
>
> 关键词：TERAFAB，太空算力，太瓦级芯片，马斯克，星际文明
>
> 🔗 文中引用的链接
>
> + [原文链接](https://mp.weixin.qq.com/s/JOadccKoBIRur-F2IjrcQA)

> ### 3️⃣ 🤖 CVPR 2026 | 中科院、港中文等提出HiFi-Inpaint：无损还原物体细节，重塑Inpainting生成范式！
>
> 中科院、港中文等团队提出HiFi-Inpaint（CVPR 2026接收），解决参考图像修复中商品细节丢失、监督粗糙等痛点，实现高保真还原。
>
> 核心创新含共享增强注意力（SEA）模块，显式注入高频特征自适应保留细节；细节感知损失（DAL）精准约束高频像素成分。
>
> 构建HP-Image-40K数据集（4万+高质量样本），通过自动化合成过滤流程突破领域数据匮乏瓶颈。
>
> 定量评估中CLIP-I（0.950）、SSIM-HF（0.429）等指标达SOTA，复杂真实场景泛化能力优异。
>
> 为电商海报、商品虚拟试用等落地扫清障碍，启发显式高频信号+Data-Centric AI的研究方向。
>
> 关键词：HiFi-Inpaint，参考图像修复，CVPR 2026，高频特征，HP-Image-40K
>
> 🔗 文中引用的链接
>
> + [原文链接](https://mp.weixin.qq.com/s/xoIJU4fBcTW4UfnbFZAKYQ)
> + [论文链接](https://arxiv.org/abs/2603.02210)
> + [项目主页](https://correr-zhou.github.io/HiFi-Inpaint)

> ### 4️⃣ 🤖 Simon发布《代理工程模式》：当代码成本归零，工程师价值在哪里？
>
> Simon发布《Agentic Engineering Patterns》指南，指出代码生成成本归零后，工程师价值从编码转向问题定义、质量保证等认知能力。
>
> 核心是设计AI代理（如Claude Code）驱动的代码生成系统，涉及上下文流设计、子代理（应对LLM上下文限制）等模式。
>
> 关键实践含红绿TDD（防AI幻觉）、先运行测试、Agentic手动测试、`Git`高级用法等可落地工程模式。
>
> 指南将AI辅助编码从“魔法”转为可教学科，通过复合循环积累质量改进，用交互式解释解决认知债务。
>
> 该范式是敏捷后软件工程重要演进，人类核心竞争力为问题定义、系统思维等，而非编码速度。
>
> 关键词：代理工程模式、AI辅助编码、代码成本归零、工程师价值、LLM应用
>
> 🔗 文中引用的链接
>
> + Simon的《代理工程模式》指南
> + [原文链接](https://mp.weixin.qq.com/s/jMFNkhYIhkHjQCYq_R9Onw)

> ### 5️⃣ 🤖 模型不是关键，Harness 才是
>
> 2026年AI工程热点Harness Engineering，是AI Agent的“操作系统”，仅换Harness可让模型编程成功率从42%跃升至78%。
>
> OpenAI等团队实践中，Agent可生成百万行代码（含测试/文档），人类核心工作是设计Harness规则而非写代码。
>
> 行业分两阵营：Big Model（模型能力主导，Harness会简化）与Big Harness（Harness是约束核心，解空间越窄效率越高）。
>
> Harness实践关键杠杆：`AGENTS.md`文件、确定性约束、工具精简、反馈循环、CI两轮限速。
>
> 未来趋势：Harness成服务模板、技术栈收敛、反哺模型训练，AI工程师需转向“管理Agent”。
>
> 关键词：Harness Engineering、AI Agent、大模型、工程实践、约束系统
>
> 🔗 文中引用的链接
>
> + [原文链接](https://mp.weixin.qq.com/s/sVGeofV9uTgvhgR44q8pNA)
> + [论文地址](https://arxiv.org/abs/2603.05344)
