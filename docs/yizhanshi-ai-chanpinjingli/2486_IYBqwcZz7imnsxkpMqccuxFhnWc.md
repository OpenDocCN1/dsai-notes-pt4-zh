# 04🈷️27日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/LVrNdF1w4opqtpxijgPcN4Aqndf](https://v11enp9ok1h.feishu.cn/docx/LVrNdF1w4opqtpxijgPcN4Aqndf)

> 🔔AI情报助手早报「04月27日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> DeepSeek-V3/R1推理效率分析(2)

>

+   本文分析DeepSeek-V3/R1推理效率，因计算量大可能有误，代码函数可供修改。

+   介绍模拟仿真项目ShallowSim，用于研究模型架构及ScaleUP和ScaleOut需求。

+   对性能校准，修正MLA、MoE GEMM性能估计，统计Prefill、Decoding性能。

+   通过仿真分析不同GPU在不同并行策略下性能，及Seq_Len对性能的影响。

+   探讨先ScaleUp再ScaleOut是否成立，以及未来性能校准方向。

> 关键词：DeepSeek-V3/R1，推理效率，模型架构，性能分析，ScaleUp和ScaleOut

>

> 🔗 文中引用的链接

+   [ShallowSim项目地址](http://github.com/zartbot/shallowsim)

+   [性能天梯榜图片](https://github.com/zartbot/shallowsim/blob/main/figures/performance.png)

> 2️⃣

> 🤖

> [A](https://mp.weixin.qq.com/s/qYr4SWW6i5s2ADWCe8ogvw)I 功能延期被指虚假宣传，苹果面临集体诉讼；刘亦菲成为鸿蒙智行智界品牌大使；我国实现全球首例无线植入式脑机接口 | 极客早知道

>

+   苹果因Apple Intelligence功能延期被指虚假宣传，遭集体诉讼。

+   xAI上线图像生成API，每张图片收费0.07美元。

+   特斯拉计划年内为Cybertruck应用电池新技术，降低成本。

+   英伟达黄仁勋承认不应“唱衰”量子计算。

+   我国实现全球首例无线植入式脑机接口，助渐冻症患者重建交流能力。

> 关键词：苹果，xAI，特斯拉，英伟达，脑机接口

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/qYr4SWW6i5s2ADWCe8ogvw)

> 3️⃣

> 🤖

> [2025年99%代码AI生成！Ope](https://mp.weixin.qq.com/s/KY10uumJqpJTIQJUWlRX7g)nAI高管宣告没有退路，人类将被全面超越

>

+   OpenAI首席产品官Kevin Weil预测2025年底99%编码将实现AI自动化，今年AI在编程能力上会永久超越人类。

+   提升模型智能水平需扩大预训练规模与通过RL提升推理能力双管齐下，GPT-5正加紧研发。

+   AI可解放程序员的机械工作，让其专注创造性难题，但也引发对内容真实性、就业冲击等担忧。

+   利用私有数据训练AI模型存在大量创业机会，如法律行业已在尝试。

+   科技界对AI编写代码比例预测不一，但都认为AI正重塑软件开发未来。

> 关键词：AI编程，OpenAI，模型智能，私有数据，软件开发

>

> 🔗 文中引用的链接

+   https://youtu.be/SnSoMh9m5hc

+   [ht](https://fortune.com/2025/03/13/ai-transforming-software-development-jobs-meta-ibm-anthropic/)tps://fortune.com/2025/03/13/ai-transforming-software-development-jobs-meta-ibm-anthropic/

> 4️⃣

> 🤖

> [真·MoE？路由LLM最全面探索：一种笔记](https://mp.weixin.qq.com/s/hQ35F-OWPAVkIk-yiwiI8Q)本也能玩的大模型Scaling Up研究

>

+   研究人员收集8500+个LLM在12个Benchmark上2亿条性能记录，探索路由LLM，发现Model-level Scaling Up现象。

+   基于数据构建评测RouterEval，研究者可用少量计算资源参与路由LLM研究。

+   弱LLM组合也能有强性能，3 - 10个LLM候选性价比高，现有Router提升空间大。

+   路由LLM与推荐系统、LLM集成等范式有区别。

+   研究面临数据缺乏、Router性能保持等挑战。

> 关键词：路由LLM，Model-level Scaling Up，RouterEval，LLM组合，挑战

>

> 🔗 文中引用的链接

+   [代码地址](https://github.com/MilkThink-Lab/RouterEval)

+   [论文地址](https://arxiv.org/abs/2503.10657)

+   [论文合集](https://github.com/MilkThink-Lab/Awesome-Routing-LLMs)

> 5️⃣

> 🎯

> 全景解读 LLM Posting-Train（后训练）技术

>

+   后训练技术核心价值体现在知识精炼、能力对齐、推理增强三个维度，可从「微调」「强化学习」「测试时拓展」类别认识。

+   微调技术含全参数微调、参数高效微调（如LoRA系列及提示微调）、领域自适应微调，各有特点与应用场景。

+   强化学习通过奖励建模等，从对齐到推理，奖励分过程与结果奖励，有推理增强实践。

+   测试时扩展涉及主流推理增强技术、计算最优扩展策略及验证器增强推理。

+   该技术面临奖励误导等挑战，前沿方向有元认知机制等，还给出实践指南。

> 关键词：LLM，后训练技术，微调，强化学习，推理增强

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/HZJyGnWU28O4oKQG89MUSA)

+   [论文链接](https://arxiv.org/abs/2502.21321)

> 6️⃣

> 🤖

> 刚刚，OpenAI首发语音Agent，智能体实现重大突破

>

+   OpenAI发布三款语音模型，用于开发语音AI Agent，包括两款语音转文本和一款文本转语音模型。

+   开发者可控制GPT-40 Mini TTS语音情绪风格，适用于不同业务场景。

+   语音转文本API增添streaming模式，集成噪声消除等技术。

+   Agents SDK重大更新，模块化设计提升开发效率与系统扩展性。

+   这些语音模型已可在API和SDK中使用。

> 关键词：OpenAI，语音Agent，语音模型，API更新，SDK更新

>

> 🔗 文中引用的链接

+   [API地址](https://platform.openai.com/docs/guides/audio)

+   [展示地址](https://www.openai.fm/)