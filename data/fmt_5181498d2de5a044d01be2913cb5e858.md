
# 6🈷️15日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/JhrrdvuPDo10t7xFBpUczplln3f](https://v11enp9ok1h.feishu.cn/docx/JhrrdvuPDo10t7xFBpUczplln3f)

> 🔔AI情报助手早报「06月15日」

## 1️⃣ 🤖 Linkloud沙龙第三十一期：AI A...

6月18日Linkloud沙龙第三十一期将在东京举办，聚焦`AI Agent`前沿议题。

`AI Agent`解决了日本传统SaaS产品无法适配客户独特需求的问题，还改变了SaaS商业模式与定价策略。

它可能大幅压缩日本企业服务成本，缩短开发周期，成为全球化SaaS与`AI Agent`初创公司机遇。

活动邀请了多位企业服务与AI领域专家，探讨`AI Agent`对日本SaaS市场影响等话题。

活动为线下沙龙，面向出海公司及东京相关从业者等，6月16日截止报名。

**关键词**：`AI Agent`，日本企服，SaaS市场，Linkloud沙龙

**🔗 文中引用的链接**

+ [原文链接](https://mp.weixin.qq.com/s/JXOtm2dUqetZAGcDUMPjNg)

## 2️⃣ 🤖 AI 彻底摆脱人类！Anthropic让...

Anthropic研究人员找到让语言模型自我微调的方法`ICM`，模型可通过评估自身答案提升能力，突破人类标注瓶颈。

`ICM`结合相互可预测性与逻辑一致性定义评分系统，采用自我监督搜索过程，还有`ConsistencyFix`算法修复不一致。

在`Llama 3.1`等模型上，`ICM`表现出色，标注成本适中，应用于`Claude 3.5 Haiku`奖励模型得分超人类标注基线。

`ICM`也有局限，如基础模型缺概念或任务输入超长时会失败。

社区对`ICM`展开讨论，相关研究也对其与其他工作进行区分。

**关键词**：AI，自我微调，`ICM`，Anthropic，模型评估

**🔗 文中引用的链接**

+ [论文原文](https://alignment-science-blog.pages.dev/2025/unsupervised-elicitation/paper.pdf)
+ [项目博客](https://alignment-science-blog.pages.dev/2025/unsupervised-elicitation/)
+ [代码仓库](https://github.com/Jiaxin-Wen/Unsupervised-Elicitation)
+ [arXiv论文](https://arxiv.org/abs/2506.10139)

## 3️⃣ 🤖 Google研究发现：Multi-Age...

Google与剑桥大学研究发现，多智能体系统中提示设计与拓扑结构构建面临挑战，提示对性能影响显著，有效拓扑结构占比小。

为此提出`Mass`框架，通过块级提示优化、工作流拓扑优化、工作流级提示优化三阶段优化多智能体系统。

实验使用`Gemini 1.5 Pro`和`Flash`模型，对比多种方法，`Mass`在多任务上性能提升超10%。

`Mass`各优化阶段均有性能提升，协同优化提示与拓扑结构效果更佳。

`Mass`相比现有自动设计方法，样本效率和成本效益更高。

**关键词**：多智能体系统，`Prompt`设计，`Mass`框架，性能提升，成本效益

**🔗 文中引用的链接**

+ [论文地址](https://arxiv.org/pdf/2502.02533)
+ [原文链接](https://mp.weixin.qq.com/s/2YthX3NTZ4XjnaEVAEZAfg)

## 4️⃣ 🤖 苹果《思考的错觉》再挨批，`Claude`与...

苹果《思考的错觉》论文因质疑大模型推理能力遭批，Gary Marcus指出7点缺点。

Anthropic和Open Philanthropy发表《思考的错觉的错觉》，AI为第一作者，指出苹果论文3个关键缺陷。

包括实验设计忽略模型输出约束、自动评估框架分类错误、基准测试含数学上无解实例。

不同表示方法测试模型可区分推理与格式限制，原论文复杂度指标混淆概念。

未来研究应设计合理评估方法，验证难题可解性，选合适复杂度指标，考虑多种解答表示。

**关键词**：大模型，推理能力，评估方法，实验设计，复杂度指标

**🔗 文中引用的链接**

+ [Gary Marcus的文章](https://garymarcus.substack.com/p/seven-replies-to-the-viral-apple)
+ [《思考的错觉的错觉》论文地址](https://arxiv.org/pdf/2506.09250v1)
+ [相关复现研究](https://x.com/scaling01/status/1931817022926839909)
+ [《](https://arxiv.org/abs/2407.07612v1)Faith and fate: Limits of transformers on compositionality》
+ [《River Crossing Problems:](https://arxiv.org/abs/1802.09369) Algebraic Approach
+ [相关测试结果](https://x.com/janekm/status/1933481060416799047)
+ [读者对作者合作模式的讨论](https://x.com/lxrjl/status/1932557168278188517)
+ [相关调侃](https://x.com/OrganicGPT/status/1932502854960366003)

## 5️⃣ 🤖 `Qwen3 Embedding` - 合成...

`Qwen-Embedding`基于`GTE`升级，内容不算新颖，采用大规模训练、高质量微调与模型合成。

合成数据分阶段，第一阶段用1.5亿合成数据，适应`MTEB`多种任务，合成步骤分两步。

第二阶段用近2000万高质量pair数据，筛选第一阶段高分数据与开源QA数据。

第三阶段merge几个`ckpt`提分。训练架构常规，取`EOS`向量，`reranking`做二分类。

训练损失`InfoNCE`，加`mask`忽略疑似假阴性负样本。

**关键词**：`Qwen-Embedding`，合成数据，训练架构，`InfoNCE`

**🔗 文中引用的链接**

+ [原文链接](https://mp.weixin.qq.com/s/yfOCH0tyEn_IhoOlF-_vdw)

## 6️⃣ 🎯 职场人速看！快速掌握 `DeepSeek`，...

6月14日晚8点《1小时AI公开课》，陈志强老师主讲，从底层逻辑拆解`DeepSeek`大模型优势。

课程涵盖`DeepSeek`使用技巧，如精准对话挖掘潜力，结合PPT制作、数据处理等场景实操演示。

直播间抽取50个幸运名额送免费1对1应用专家咨询。

适合职场办公族、自由职业者、学生等群体。

直播中讲师将现场答疑，还可进交流群获取课件等资源。

**关键词**：`DeepSeek`，AI公开课，大模型，职场应用，直播福利

**🔗 文中引用的链接**

+ [原文链接](https://mp.weixin.qq.com/s/5UFjP4qR8_o2QcEQUiqwgQ)
