
# 7🈷️22日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Hqj0diQxHoBo1nxDcxfcLicBnwb](https://v11enp9ok1h.feishu.cn/docx/Hqj0diQxHoBo1nxDcxfcLicBnwb)

> 🔔AI情报助手早报「07月22日」

## 1️⃣ 🎯 Manus 内部的 `Context` 工程

`Manus` 团队分享构建 Agent 的 `Context` 工程经验，共 6 条。

- 围绕 `KV缓存` 设计，提升命中率以降低延迟和成本。
- 用状态机遮蔽而非移除工具，优化动作选择。
- 将文件系统作上下文，采用可恢复压缩策略。
- 通过“复述”操控注意力，避免目标偏离。
- 保留出错记录，增加多样性避免 `Few - Shot` 陷阱。

关键词：`Context` 工程，AI 智能体，上下文学习，工具管理，注意力操控

### 🔗 文中引用的链接

- [`Manus`](https://manus.im/)
- `In - context learning`
- [`BERT`](https://arxiv.org/abs/1810.04805)
- [`Open information extraction`]
- [`GPT - 3`](https://arxiv.org/abs/2005.14165)
- [`Flan - T5`](https://arxiv.org/abs/2210.11416)
- [`A typical agent`](https://arxiv.org/abs/2210.03629)
- [`KV - cache`](https://medium.com/@joaolages/kv%20-%20caching%20-%20explained%20-%20276520203249)
- [`Autoregressive`](https://en.wikipedia.org/wiki/Autoregressive_model)
- [`vLLM`](https://github.com/vllm%20-%20project/vllm)
- prefix / prompt caching
- [`MCP`](https://modelcontextprotocol.io/introduction)
- [`RAG`](https://en.wikipedia.org/wiki/Retrieval%20-%20augmented_generation)
- [`Constrained decoding`]
- [`State machine`](https://en.wikipedia.org/wiki/Finite%20-%20state_machine)
- [`Hermes format`](https://github.com/NousResearch/Hermes%20-%20Function%20-%20Calling)
- [`Neural Turing Machines`]
- [`Temperature`](https://arxiv.org/abs/2405.00492)
- `Few - shot Prompting`

## 2️⃣ 🤖 同构视角下的异构计算：谈谈 AI 的软硬件

文章探讨 AI 软硬件交付界面，分析 `NV` 与 `Ascend` 架构，认为构建“同构视角下的异构计算”是关键。

- `NV` 发展有从 `DSA` 到可编程再到 `DSA` 的周期，在 `CUDA` 生态做易用性开发，现走向基于任务的张量计算抽象。
- `Ascend` 希望用同构 `ISA` 支持不同处理器，面临软件并行性、调度等难题。
- 生态和习惯很重要，破坏用户习惯代价大。
- 架构师需上下兼顾，异构计算要长期主义，尊重生态。

关键词：AI，异构计算，`NV` 架构，`Ascend` 架构，生态

### 🔗 文中引用的链接

- [`Apple Backs Project to Bridge its MLX Framework with Nvidia’s CUDA Ecosystem`](https://winbuzzer.com/2025/07/15/apple-backs-project-to-bridge-its-mlx-framework-with-nvidias-cuda-ecosystem-xcxwbn/)
- [`An Outlook for New Homogeneous Computing System`](https://www.chaspark.com/#/stw/media/720435943085228032)
- [`Jim Keller：使用 RISC-V 构建 AI —— 61DAC Keynote`](https://www.bilibili.com/video/BV19u8zeNER8/)
- [`Demystifying Datapath Accelerator Enhanced Off-path SmartNIC`](https://arxiv.org/html/2402.03041v2)

## 3️⃣ 🎯 AI 智能体的上下文工程：`Manus` 的构建

`Manus` 项目选择基于前沿模型的“上下文学习”能力构建智能代理，将赌注押在“上下文工程”上。

- 上下文工程实践包括围绕 `KV缓存` 设计，保持提示前缀稳定等；使用掩码而非移除工具来管理动作空间。
- 将文件系统视为终极上下文，通过可恢复的压缩策略缩减上下文长度。
- 通过“复述”操控注意力，保留错误信息以让模型适应，避免“小样本”带偏。
- 上下文工程对智能代理系统至关重要，精心设计上下文决定智能代理行为。

关键词：AI 智能体，上下文工程，`Manus`，上下文学习，智能代理

### 🔗 文中引用的链接

- [`Manus`](https://manus.im/app)
- [`BERT`](https://arxiv.org/abs/1810.04805)
- [`GPT-3`](https://arxiv.org/abs/2005.14165)
- [`Flan-T5`](https://arxiv.org/abs/2210.11416)
- [`典型的智能代理`](https://arxiv.org/abs/2210.03629)
- [`KV 缓存`](https://medium.com/@joaolages/kv-caching-explained-276520203249)
- [`自回归`](https://en.wikipedia.org/wiki/Autoregressive_model)
- [`vLLM`](https://github.com/vllm-project/vllm)
- [`前缀/提示缓存`](https://docs.vllm.ai/en/stable/design/v1/prefix_caching.html)
- [`MCP`](https://modelcontextprotocol.io/introduction)
- [`检索增强生成（RAG）`](https://en.wikipedia.org/wiki/Retrieval-augmented_generation)
- [`约束解码`](https://platform.openai.com/docs/guides/structured-outputs)
- [`状态机`](https://en.wikipedia.org/wiki/Finite-state_machine)
- [`Hermes 格式`](https://github.com/NousResearch/Hermes-Function-Calling)
- [`神经图灵机`](https://arxiv.org/abs/1410.5401)
- [`温度`](https://arxiv.org/abs/2405.00492)

## 4️⃣ 🤖 一个重要的发布被忽略！`Decart` 发布实时...

`Decart` 发布全球首个实时、无限长度 AI 视频模型 `MirageLSD`，基于“实时流扩散”技术，可将想象实时注入视频流。

- 创始人展示其低延迟实力，响应时间低于 40 毫秒，实现“零延迟”，支持流畅视频流。
- `Decart` 攻克“错误累积”与“极限压榨 `GPU`”难题，实现无限生成与零延迟。
- `MirageLSD` 不仅用于特效，还将成为新计算平台入口，`Decart` 已上线平台并计划升级。
- 创始人目标打造“千亿级独角兽”，掀起视觉革命。

关键词：AI 视频模型，`MirageLSD`，实时流扩散，零延迟，无限生成

### 🔗 文中引用的链接

- [`参考链接`](https://about.decart.ai/publications/mirage)

## 5️⃣ 🤖 京东、饿了么被监管约谈；宇树科技开启上市...

7 月 18 日，市场监管总局约谈饿了么、美团、京东，要求规范促销、理性竞争。

- `Meta` 又从苹果挖来两名 AI 研究人员，此前已挖走苹果 AI 基础模型团队负责人。
- 小米深圳总部大厦启用，宇树科技开启上市辅导，理想 `i8` 与小米 `YU7` 发布会地址相同。
- 网传比亚迪将赞助中国足协中国之队，大疆 `Osmo 360` 全景相机包装盒曝光。
- 2026 插混 `V8` 款奥迪 `RS6 Avant` 旅行车纽北测试视频曝光，乔丹豪宅上线爱彼迎。

关键词：企业动态，AI 人才，科技产品，体育赞助，高端房产

### 🔗 文中引用的链接

- 央视新闻
- 新浪财经
- 新浪科技
- 新京报
- IT之家
- 财联社

## 6️⃣ 🤖 线下实战 | AI 智能体，2025 企业生...

2025 年企业数字化转型深入，AI 智能体成企业生存必修课。

- 7 月 31 日 14:00 - 17:00，`AIGC` 开放社区将举办北京线下实战课，由小安老思主讲。
- 课程针对流量获客等 4 大业务痛点，提供系统性培训等 3 大核心价值。
- 适合多行业企业负责人等参与，填写问卷可领专家咨询，到场送课程手册。
- 扫码报名，咨询邹老师，活动详情见海报。

关键词：AI 智能体，企业数字化转型，线下实战课，业务痛点，课程福利

### 🔗 文中引用的链接

- [`原文链接`](https://mp.weixin.qq.com/s/vs9hHivphtHCvxQAT12xOA)
