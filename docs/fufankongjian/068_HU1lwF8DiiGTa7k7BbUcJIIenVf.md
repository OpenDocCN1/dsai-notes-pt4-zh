# DeepSeek V3（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/EKxAddLovovkeVx2rNNcIYeqnSh](https://kq4b3vgg5b.feishu.cn/docx/EKxAddLovovkeVx2rNNcIYeqnSh)

# 模型介绍

DeepSeek V3 是一款由 DeepSeek AI 开发的开源大型语言模型（LLM），于 2024 年 12 月发布，专注于数学和编码任务。研究表明，DeepSeek V3 在 MATH 500 和 Codeforces 等基准测试中表现出色，数学问题解决能力约 95%，编码任务表现接近人类顶尖水平。它与闭源模型如 OpenAI 的 GPT-4o 竞争，特别适合需要复杂推理的开发场景。

DeepSeek V3 采用 Mixture of Experts (MoE) 架构，与 DeepSeek-V2 类似。这种架构允许模型拥有 6710 亿总参数，但每次仅激活 370 亿参数，从而实现高效计算。以下是架构相关细节的表格：

| **模型** | **架构细节** | **上下文长度** | **总参数数** | **激活参数数** |
| --- | --- | --- | --- | --- |
| DeepSeek-V3 | MoE，基于 DeepSeek-V2 | 128,000 | 6710 亿 | 370 亿 |

> 这种设计使模型能够在保持高性能的同时，降低计算资源需求，特别适合需要实时推理的应用程序。

关键架构特征包括：

+   Multi-Head Latent Attention (MLA)：通过同时关注输入的多个方面，增强推理效率。

+   **DeepSeekMoE**：定制的 MoE 架构，优化训练成本和性能。

+   Multi-Token Prediction (MTP)：预测多个令牌的目标，可能加速推理。

DeepSeek V3 的训练过程分为多个阶段，旨在平衡推理能力和输出质量：

+   **预训练**：使用 14.8 万亿多样化、高质量令牌进行预训练，确保模型对各种任务的广泛理解。

+   **训练技术**：采用 FP8 混合精度训练，减少内存使用并加速计算；引入辅助损失免费的负载平衡策略，维持性能。

+   **后训练**：包括监督微调 (SFT) 和强化学习 (RL) 阶段，以优化模型能力和用户对齐。

> 训练成本仅为 2.788 百万 H800 GPU 小时，总计约 550 万美元，这一成本效率远低于同类模型（如 Llama 3 405B 使用 3080 万 GPU 小时）。

> **关键引用**：

+   DeepSeek V3 GitHub 存储库详细介绍

+   DeepSeek V3 Hugging Face 页面

+   DeepSeek API 文档和定价信息

+   [DeepSeek 官方网站](https://www.deepseek.com/)

+   DeepSeek V3 技术报告

# 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️** **实际评测

⭐️DeepSeek-V3-0324模型深度解析（课件）

+   [最强Agent大模型！DeepSeek-V3-](https://www.bilibili.com/video/BV1zrZNYYEyA)0324模型详细介绍与深度评测！（视频）

+   【深度】DeepSeek v3性能评测与技术解析（视频）

⭐️DeepSeek- V3.1 模型性能实测【提示词模板】

+   [DeepSeek更新！6分钟速览V3.1性能，DeepSee](https://www.bilibili.com/video/BV18XePzAEWC)k VS Claude一手实测！（视频）

⭐️[DeepSeek-V3.2-Exp：利用 DeepSeek 稀](https://kq4b3vgg5b.feishu.cn/wiki/SEZjwoh4qiBI6nkOn8acgsWrnpc?from=from_copylink)疏注意力机制提升长上下文效率（中文论文）

+   [架](https://b23.tv/4QoKpXJ)构大突破，DeepSeek-V3.2重磅发布！成本暴降50%！原论文解读+模型特性介绍+DeepSeek稀疏注意力原理介绍！（视频）

+   [性能暴涨！DeepSeek-V3.2模型解读与实战评测！追平Gemini 3.0 Pr](https://www.bilibili.com/video/BV13rSHBBEoG)o，模型编程、数学、Agent开发能力跻身全球第一梯队，开源新SOTA！（视频）

### **☑️ **部署调用

⭐️DeepSeek v3 API接入指南（课件）

⭐️DeepSeek v3 Function calling（课件）

⭐️Open-WebUI接入DeepSeek v3流程详解（课件）

+   [DeepSeek-v3调用指南与Func](https://www.bilibili.com/video/BV1JuwVewELc)tion calling技术实战（视频）

⭐️DeepSeek v3本地部署流程（课件）

+   [DeepSeek v3本地部署与](https://www.bilibili.com/video/BV1jjwVe4EKn)transformer库调用流程（视频）

### **☑️ **进阶之路

1、DeepSeek v3+GraphRAG实战

⭐️DeepSeek借助GraphRAG搭建知识库问答机器人（课件）

+   [DeepSeek v3+Grap](https://www.bilibili.com/video/BV1Xwc6eoEW5)hRAG知识谱图检索增强技术实战（视频）

2、LangGraph+DeepSeek 开发实战

⭐️[LCEL快速入门与实战（课件）](https://kq4b3vgg5b.feishu.cn/wiki/PduuwYX6CiPsHQkUOeecHLj0nYf?from=from_copylink)

⭐️LangGraph 基础入门与 Agent 开发实战（课件）

⭐️【补充资料】Windows安装Docker（课件）

⭐️【补充资料】Serper API使用与数据预处理拆解（课件）

⭐️【补充材料】Windows安装Mysql数据库（课件）

+   [【8小时合集】DeepSeek v3 构建RAG丨可视](https://www.bilibili.com/video/BV1FRFNerEnh/?)化Multi-Agent开发实战（视频）

**3、智能体开发**

⭐️[DeepSeek借助Swarm搭建M](https://kq4b3vgg5b.feishu.cn/wiki/J5dqwMC1civtduk6AwwcrKkwnzh?from=from_copylink)ulti-Agent多代理智能体（课件）

+   [DeepSeek v3借助Swaem搭建M](https://www.bilibili.com/video/BV1T8czeSEf1)ulti-Agent技术入门实战（视频）

**4、模型训练**

⭐️MiniDeepSeek分词器训练流程（课件）

⭐️MiniDeepSeek预训练（课件）

⭐️[MiniDeepSeek后训练：](https://kq4b3vgg5b.feishu.cn/wiki/DVxAwaZTjib2JhkodoAcw7YonHf?from=from_copylink)全量指令微调+DPO强化学习微调（课件）

+   [手动复现DeepSeek v3训练过程丨模型预训练+](https://www.bilibili.com/video/BV1KtwueYE54)全量指令微调+DPO强化学习微调（视频）

5、Function calling功能详解

⭐️DeepSeek Function calling功能详解（课件）

6、借助MCP进行Agent开发实战

⭐️DeepSeek Function calling功能详解（课件）

⭐️[MCP+DeepSeek-V3-032](https://kq4b3vgg5b.feishu.cn/wiki/EjmSwWPjHiL0YLkImmec7RjXnwh?from=from_copylink)4搭建miniManus开发实战（课件）

⭐️DeepSeek Function calling响应模式介绍（课件）

⭐️DeepSeek企业级智能体MiniManus开发实战（课件）

+   [D](https://www.bilibili.com/video/BV1L3ZDYDEnE)eepSeek智能体开发实战，从零手搓Mini Manus！4小时从零入门到实操企业级Agent项目（视频）

**7、****DeepAgents ****实战**

⭐️ LangChain DeepAgents 项目介绍（课件）

⭐️ [DeeSeek-v3.2 + Dee](https://kq4b3vgg5b.feishu.cn/wiki/IP2cwC46ai8UlCkSjOIcQYhJnNb?from=from_copylink)pAgents 构建AI深度代理（课件）

⭐️ DeepAgents 应用开发实战（课件）

+   [1](https://www.bilibili.com/video/BV1V42kBZEkB):1复刻Manus！LangChain1.0的最新框架DeepAgents+DeepSeek-v3.2构建深度研究智能体！（视频）

---

# ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/17c635e788dabaeb956c90648ccfabc1.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流氛围浓厚，**不定期开设硬**核干货&前沿技术公开课噢**~

---