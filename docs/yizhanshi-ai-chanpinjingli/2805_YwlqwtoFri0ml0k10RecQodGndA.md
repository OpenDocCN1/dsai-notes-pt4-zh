
# 3🈷️25日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BDR1dl5jcoXt1cx4RlTcUXJCnph](https://v11enp9ok1h.feishu.cn/docx/BDR1dl5jcoXt1cx4RlTcUXJCnph)

> 🔔AI情报助手晚报「03月25日」

## 1️⃣ SGLang 开发、优化、debug 技巧

记录下 `SGLang` 开发、优化、debug 的技巧之大 `SKILL` 时代已来临

- `SKILL` 时代来临，Codex/GPT5.4 等 Agent 通过 `SGLang SKILL` 可完成 kernel 编写、优化、`CUDA` Crash debug 等工作。
- 借助 Agent，`SGLang Diffusion` 的 `Z-Image` 单卡速度提升 40%，`Qwen/Qwen-Image-2512` 单卡速度提升 20%+。
- 优化 Agent 流程需蒸馏开发者经验成通用 `SKILL`，参考 `Flashinfer` API logging 做了 `SGLang CUDA Crash debug` skill。
- 需警惕 Agent 修改偏离方向的破坏性后果，不能完全依赖其结果不审核。
- 人的价值转向定义问题、把关 Agent 产出，成为 Vibe Coding 高手是当前出路。

关键词：`SGLang`，`Coding Agent`，`SKILL`，kernel 优化，CUDA debug

### 🔗 文中引用的链接

- `SGLang` kernel fuse pattern PR
- [`AKO4ALL` Agent框架](https://github.com/TongmingLAIC/AKO4ALL)
- `SGLang CUDA Crash debug` skill PR
- [`autokernel`优化框架](https://github.com/RightNow-AI/autokernel)

## 2️⃣ RXD 大会首发北京：当硅谷还在谈论物理A...

西门子 RXD 大会（Reality meets Digital）首发北京，聚焦 AI 如何落地物理世界成为工业变革基石，而非快闪奇观。

- 西门子推出含 26 款新品的全栈技术栈（贯通硬件、软件、数据），以数字孪生 + AI Agent 重构生产全生命周期逻辑。
- 工业 AI 依赖西门子 170 年沉淀的行业数据/知识，经训练后问题解决命中率提升至近 95%，突破通用模型精度瓶颈。
- 西门子联合英伟达（仿真算力跃升）、阿里云（中国生态落地）构建工业 AI 生态，赋能合作伙伴规模化应用。
- 中国作为全球最大制造业经济体，是工业 AI 最佳试验场，西门子借大会加速 AI 工业场景硬核着陆。

关键词：工业 AI，西门子 RXD 大会，数字孪生，工业全栈技术，物理 AI 落地

### 🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/Tk4TaItYzS4J6U8tRPD0Og)

## 3️⃣ 世界模型最新综述！中科院联合 MBZ、NTU、Oxford 系统梳理前沿进展

中科院等联合多机构发布 2026 年 1 月截止的世界模型综述，系统梳理建模范式、方法及应用。

- 综述将现有世界模型归纳为观测层生成式、潜空间、强化学习驱动、对象中心四大分支。
- 应用覆盖机器人、自动驾驶、科学发现、虚拟游戏、`GUI` 智能体及可解释可信领域。
- 补充了基准数据集、评测指标、仿真平台及跨模型性能对比。
- 指出未来需突破长期一致性、因果推理、物理语义约束等通用智能障碍。

关键词：世界模型，AI 综述，通用智能，基准评测，挑战方向

### 🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/RYATYwUDgLnGm29RH1fr_Q)
- [项目地址](https://github.com/JiahuaDong/Awesome-World-Models)
- [论文地址](https://www.techrxiv.org/doi/full/10.36227/techrxiv.177274570.09578608/v1)

## 4️⃣ 绝了！做了个龙虾知识库，还能副业变现（附教程）

文章介绍用飞书 AI 知识库功能，零代码搭建专属龙虾知识库，支持电脑/手机多端访问。

- 上传文档后开启 AI 问答，能基于上传资料精准回答问题，标注引用来源防幻觉，还可联网搜索补充信息。
- 飞书内置支付系统，设置付费加入并绑定收款账户，实现全自动知识变现，无需繁琐流程。
- 搭建步骤极简：创建共享知识库 → 拖拽上传文档 → 开启 AI 问答 → 配置变现，全程无需编程。
- 适合技术博主等人群，打造 24 小时自动服务的 AI 知识分身，零成本构建数字资产。

关键词：飞书 AI 知识库、知识变现、零代码搭建、AI 问答、龙虾知识库

### 🔗 文中引用的链接

- [飞书 AI 知识库主页](https://ask.feishu.cn/shared-space/center)
- [郭震 AI 龙虾知识库链接](https://ask.feishu.cn/shared-space/7619866034513660866?referrer=guozhen)

## 5️⃣ 我们正在目睹 SaaS 的死亡

软件股蒸发 1 万亿美元，SaaS 估值倍数从疫情高峰 18.5x 跌至 4.8x，同期 AI 市场从 500 亿涨至 5390 亿，预计 2033 年达 3.5 万亿。

- 2027 年左右 AI 估值将完全超越 SaaS，核心因 AI Agent 按任务收费（每次 <1 美元）取代 SaaS 按席位收费（每月 50 美元/席）。
- 仅依赖按席位定价且无真实数据优势的 SaaS 公司最脆弱，并非所有 SaaS 都会消亡。
- 多数 SaaS 创始人未察觉危机，因接受现实需重构业务逻辑。
- AI Agent 的任务收费模式对 SaaS 传统定价体系构成颠覆性冲击。

关键词：SaaS 估值，AI Agent，任务收费，按席位定价，AI 市场规模

### 🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/4rsSp79eEc3E3IRK_mSmsg)
