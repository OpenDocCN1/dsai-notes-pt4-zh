# 微学习03 | 原理介绍：提示词

> 来源：[https://datawhaler.feishu.cn/docx/O3BhdPVULoXx3rx7eN3cvPqznQb](https://datawhaler.feishu.cn/docx/O3BhdPVULoXx3rx7eN3cvPqznQb)

# 告别低效沟通：提示词框架助你玩转deepseek

## 一、什么是提示词和提示词框架？

#### 提示词（Prompt）

提示词是用户向AI模型（如ChatGPT）输入的指令或请求，用于引导模型生成特定类型的输出。简单来说，**它是人类与AI对话的“钥匙”**，决定了AI回答的方向、风格和深度。

**举例**：

+   普通提问：“写一首诗。”

+   优化后的提示词：“以李白的豪放风格，写一首关于秋天落叶的五言绝句。”

#### 提示词框架（Prompt Framework）

提示词框架是一套结构化的模板，通过**分模块、分场景的标准化设计**，帮助用户更高效地生成精准的提示词。

**核心作用**：

+   降低沟通歧义

+   提升任务完成效率

+   适应复杂场景需求

---

### 二、为什么需要提示词框架？

#### **1. 突破AI的“思维局限”**

AI模型的输出质量高度依赖输入的清晰度。框架通过结构化信息，帮助AI更准确地理解用户意图。

**常见问题**：

+   模糊指令：“帮我写个文案” → AI可能生成不相关的广告语。

+   框架优化：明确目标受众、产品卖点、风格要求。

#### **2. 节省反复调试的时间**

据统计，使用框架后，用户与AI的对话轮次平均减少60%。 **案例**：

+   未用框架：需多次调整语气、补充背景信息。

+   使用框架（如CRISPE）：一次性提供角色、背景、格式要求，直接生成可用结果。

#### **3. 适配专业场景需求**

不同领域（如编程、营销、教育）需要不同的沟通逻辑，框架提供**场景化解决方案**。

**示例**：

+   技术文档写作 → ICIO框架（强调输入输出格式）

+   儿童教育辅导 → CRISPE框架（注重角色扮演与互动方式）

---

### 三、怎么使用提示词框架？

#### 3步上手法

1.  **明确任务类型**

+   内容创作？数据分析？角色模拟？

+   **匹配框架**：创意类任务用BROKE框架，技术类任务用ICIO框架。

1.  按模块填充信息以CRISPE框架为例：

+   角色（Capacity and Role）→ “资深营养师”

+   背景（Insight）→ “用户是健身新手，需制定减脂食谱”

+   格式（Personality）→ “步骤清晰，附带卡路里计算”

1.  **迭代优化**

+   根据AI输出结果，调整框架中的模糊描述（如将“正式风格”改为“商务报告风格”）。

#### 避坑指南

+   **避免信息过载**：框架不是“越复杂越好”，需根据任务复杂度选择。

+   **灵活组合**：可跨框架混用模块（如BROKE的“改进机制”+CRISPE的“角色设定”）。

---

### 四、主流的提示词框架有哪些？

以下是10大热门框架及其适用场景对比：

| 框架名称 | 核心模块 | 适用场景 | 典型案例 |
| --- | --- | --- | --- |
| **ICIO** | 任务、背景、输入、输出 | 数据处理、技术文档 | 翻译商务报告 |
| **CRISPE** | 角色、背景、任务、风格、实验 | 角色扮演、教育辅导 | 模拟数学老师解题 |
| **BROKE** | 背景、角色、目标、关键结果、改进 | 项目管理、创意设计 | 制定软件开发计划 |
| **RASCEF** | 角色、行动、步骤、上下文、示例 | 专业咨询、流程管理 | 设计邮件营销策略 |
| **CO-STAR** | 上下文、目标、风格、语气、受众 | 广告文案、社交媒体 | 生成爆款产品文案 |
| **BORE** | 背景、目标、结果、实验 | 数据分析、市场调研 | 用户行为分析报告 |
| **APE** | 行动、目的、期望 | 快速任务执行 | 环保产品推广策略 |
| **TRACE** | 任务、请求、操作、上下文、示例 | 邮件营销、活动策划 | 年终促销邮件设计 |
| **ROSES** | 角色、目标、场景、解决方案、步骤 | 项目管理、战略规划 | SEO优化方案制定 |
| **CLEVER** | 清晰度、语言、效率、价值、评估 | 全领域内容优化 | 高转化率广告文案 |

---

### 五、拓展知识

#### 1. 动态框架（Dynamic Framework）

+   **概念**：根据对话上下文自动切换框架模块。

+   **案例**：初始使用ICIO生成技术文档 → 后续根据用户反馈自动启用BROKE的“改进机制”。

#### **2. 多模态框架**

+   **趋势**：整合文本、图像、语音指令的混合框架。

+   **应用**：“生成📕配图文案+视觉风格建议”（结合DALL·E与ChatGPT）。

#### **3.PromptWizard**

+   PromptWizard是微软推出的自动化提示优化框架，改进大型语言模型（LLMs）在特定任务中的表现。

+   基于自我演变和自我适应机制，PromptWizard用反馈驱动的批评和合成过程，在探索和利用之间找到平衡，迭代地优化提示指令和上下文示例，提高模型的准确性和效率，减少API调用和令牌使用，降低成本。

+   **项目官网**：microsoft.github.io/PromptWizard

+   **GitHub仓库**：[https://github.com/mi](https://github.com/microsoft/PromptWizard)crosoft/PromptWizard

+   **arXiv技术论文**：https://arxiv.org/pdf/2405.18369

#### **4 .MindSearch**

+   MindSearch是由上海人工智能实验室推出的一款开源AI搜索引擎框架，旨在模拟人类的思维过程进行复杂信息搜集与整合。

+   用户可以使用闭源LLM（如GPT、Claude）或开源LLM（如InternLM2.5系列模型）来部署自己的搜索引擎。

---

### 参考资料

https://blog.csdn.net/DEVELOPERAA/article/details/145052230 | 产品经理必须懂得AI：prompts提示工程之五大经典框架（附教程）_提示工程框架 实现代码-CSDN博客

https://developer.aliyun.com/article/1490356 | 你要牢记的四个常用AI提示词框架：ICIO、CRISPE、BROKE、RASCEF，有助于获取更加稳定和高质量的内容-阿里云开发者社区

https://xiangyugongzuoliu.com/prompt-framework-guide-2025/ | 2025年提示词框架完全指南：18个实用框架让你的AI对话效率大幅提升 - 翔宇工作流

https://ai-bot.cn/promptwizard/ | PromptWizard - 微软开源的 AI 提示词自动化优化框架 | AI工具集

https://www.aisharenet.com/mindsearch/ | MindSearch：开源AI搜索引擎框架，部署您自己的 Perplexity 搜索引擎！-首席AI分享圈

https://zhuanlan.zhihu.com/p/642893513 | 提示词工程100问：chatGPT提示词框架最全总结（1） - 知乎

https://www.53ai.com/news/gerentixiao/2024062432465.html | 还不会写提示词？这份提示词大赛冠军的框架拿去照抄！ - 大模型知识库|大模型训练|开箱即用的企业大模型应用平台|智能体开发|53AI

https://zhuanlan.zhihu.com/p/648357075 | 9 个简单但功能强大的提示词框架 - 知乎

https://www.wehelpwin.com/article/5276 | 一文汇总市面上所有prompts提示词框架！ - AI魔法学院

https://mp.weixin.qq.com/s/F76c46rMjyRsp8ZmHvkOcw | 搭载冠军提示词框架，这个工具，带你一步步写出更好AI提示词]

https://cloud.tencent.com/developer/article/2493723 | 秒懂！DeepSeek提示词手册-腾讯云开发者社区-腾讯云

https://www.huxiu.com/article/3985270.html | DeepSeek R1之后，提示词技巧还有用吗？-虎嗅网

https://juejin.cn/post/7442133028962975781 | 面向前端人的提示词工程指南大家好，我是 lv (lv.liu)。 本篇文章，带你深入提示词工程，包括从技术的角度看提示词 - 掘金

https://agijuejin.feishu.cn/wiki/Qt2IwpoSsiKZMRkekpycGuIVnTf | 全网最全的13个提示词框架 - 飞书云文档

https://blog.csdn.net/m0_56255097/article/details/144232522 | 一文搞懂Prompt提示工程五大框架_prompt 提示词框架-CSDN博客

https://blog.csdn.net/m0_63171455/article/details/139982110 | Prompt提示词工程全攻略：15+Prompt框架一网打尽、学会提示词让大模型更高效_李继刚 prompt-CSDN博客

https://cloud.tencent.com/developer/article/2400512 | Prompt工程全攻略：15+Prompt框架一网打尽（BROKE、COAST、LangGPT）、学会提示词让大模型更高效-腾讯云开发者社区-腾讯云

https://blog.csdn.net/qq_46094651/article/details/144915389 | 一文讲完17种提示词工程(prompt engineering)方法-CSDN博客

https://blog.csdn.net/weixin_44555174/article/details/145338105 | 提示词的艺术 ---- AI Prompt 进阶（提示词框架）-CSDN博客

https://www.explinks.com/blog/wx-exploring-llm-application-development-26-prompt-agent-frameworks-like-autogpt-and-autogen/ | 一文探秘LLM应用开发(26)-Prompt(架构模式之Agent框架AutoGPT、AutoGen等) - 幂简集成

https://www.jianshu.com/p/e62d39b4d2ba | 提示词框架（Prompt Frameworks） - 简书