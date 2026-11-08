# 1🈷️03日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/UMG3d59uWoiqa2xD1tRci19NnDf](https://v11enp9ok1h.feishu.cn/docx/UMG3d59uWoiqa2xD1tRci19NnDf)

> 🔔AI情报助手午报「01月03日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🎯

> [《Easy-Vibe》项目正式发布](https://mp.weixin.qq.com/s/8ag9l96iXy5uyM38ZQA3ow)，一起学习真正的Vibe Coding！

>

+   🚀 2026年1月1日Datawhale联合清华深研院开源Easy-Vibe项目，提供零基础入门的AI原生应用实践教程，解决AI Coding入门的工具迷茫、能力断层、门槛偏高问题。

+   🧠 提出Vibe Coding新编程范式，以自然语言驱动、快速闭环为核心，让个人能同时承担前后端、AI算法、产品经理角色，成为"一人军队"。

+   📚 设计三大阶梯学习路径：Project主线（9个项目从游戏到原生应用）、Extra扩展（补全开发者必备知识）、Examples实战（完整应用开发全流程）。

+   💡 项目初心是消除技术傲慢，让更多人无门槛进入原生AI开发世界，强调"完成比完美重要"的理念。

+   🌱 未来计划覆盖不同经验开发者职能，建立初创公司应用模板及不同类别应用的完整构建Benchmark。

> 关键词：Easy-Vibe，Vibe Coding，AI原生应用，开源教程，一人军队

>

> 🔗 文中引用的链接

+   [Easy-Vibe中文地址](https://github.com/datawhalechina/easy-vibe)

+   [Easy-Vibe英文地址](https://github.com/THU-SIGS-AIID/ai-vibe-coding-101)

> 2️⃣

> 🔮

> 信息论证明，小模型跑在本地才是Agent...

>

+   斯坦福大学研究者引入香农信息论，为智能体系统的压缩器-预测器架构设计提供数学严谨的指导原则，解决了依赖试错法的痛点。

+   论文提出互信息作为衡量压缩器摘要质量的核心指标，通过代理模型机制（小模型生成+大模型评估）实现鲁棒计算，且不改变Scaling Law趋势。

+   实验发现压缩器扩容收益远超预测器：固定预测器时增大压缩器参数可带来陡峭准确率提升，而扩大预测器则边际收益递减。

+   大模型压缩器具有亚线性成本优势：更大模型生成的摘要更简洁且信息密度更高，总计算成本增长远低于预期。

+   智能体设计应遵循“算力前置”原则，用本地大模型（如7B参数）做压缩提纯数据，大幅降低云端API调用成本，混合模型家族搭配效果更佳。

> 关键词：信息论，智能体系统，压缩器-预测器架构，互信息，算力前置，亚线性成本

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/6M3WevMifieLd7Fw5u_qLQ)

+   论文标题：An Information Theoretic Perspective on Agentic System Design

+   Qwen-2.5模型系列

> 3️⃣

> 🔥

> [SebastianRaschka万字年](https://mp.weixin.qq.com/s/OWZUqf9vnXBZqjUFLSBnzQ)终复盘：2025，属于「推理模型」的一年

>

+   Sebastian Raschka复盘2025年AI领域，指出这是推理模型主导的一年，DeepSeek R1的RLVR和GRPO算法成技术风向标，其训练成本远低于预期（V3约500万美元，R1额外29.4万美元）。

+   GRPO成为年度研究宠儿，学术界和企业推出多种改进版本（如DAPO、Dr.GRPO等），有效提升训练稳定性和效果。

+   LLM架构收敛于MoE+高效注意力机制，同时出现文本扩散模型等替代方向，未来Transformer仍为主流但会加入更多效率调整。

+   推理扩展（如自一致性+自精炼）和工具使用推动进步，但Benchmaxxing（极限刷榜）导致基准测试失去参考价值。

+   作者分享自身工作：《从头构建大语言模型》受欢迎并被多语言翻译，续作《从头构建推理模型》正在撰写，关注推理模型训练技术。

> 关键词：推理模型，RLVR，GRPO，LLM架构，Benchmaxxing

>

> 🔗 文中引用的链接

+   Sebastian博客原文：

+   [h](https://magazine.sebastianraschka.com/p/state-of-llms-2025)ttps://magazine.sebastianraschka.com/p/state-of-llms-2025

+   RLVR理解文章：

+   [h](https://magazine.sebastianraschka.com/p/understanding-reasoning-llms)ttps://magazine.sebastianraschka.com/p/understanding-reasoning-llms

+   LLM架构比较：

+   [https://magazine.sebastianraschka.c](https://magazine.sebastianraschka.com/p/the-big-llm-architecture-comparison)om/p/the-big-llm-architecture-comparison

+   论文列表：

+   [https://magazine.sebastianraschka](https://magazine.sebastianraschka.com/p/llm-research-papers-2025-list-one).com/p/llm-research-papers-2025-list-one

+   原文链接：

+   [https://mp.weixin.qq.com/s/OW](https://mp.weixin.qq.com/s/OWZUqf9vnXBZqjUFLSBnzQ)ZUqf9vnXBZqjUFLSBnzQ

> 4️⃣

> 🔔

> 福利｜vLLM大模型推理工程师（实习生+正式员工）长期内推

>

+   vLLM社区长期内推大模型推理工程师（含实习生及正式员工），响应多家知名公司需求，因该领域工程师供不应求。

+   vLLM是大模型推理主流框架，被AWS、阿里云等云厂商及DeepSeek、Qwen等模型厂商广泛采用。

+   候选人需熟悉大模型RL框架、tool calling、constraint decoding、高性能kernel优化等任一项相关技术。

+   加分项含实现vLLM重点功能、适配社区合作项目等，薪资百万不设上限，工作地点覆盖中美大城市。

+   简历发送至talentpool@vllm.ai，可加入vLLM中英文社区（中文加小助手，英文申请Slack）。

> 关键词：vLLM，大模型推理工程师，内推，高性能kernel，分布式编程

>

> 🔗 文中引用的链接

+   [vLLM GitHub仓库](https://github.com/vllm-project/vllm)

+   [vLLM英文社区Slack](https://slack.vllm.ai)

+   [原文内推链接](https://mp.weixin.qq.com/s/-5fmjEfACqgDWCuB-hPjKQ)

+   vLLM中文社区小助手

+   （注：文中提及🛰️号vllm_project）

> 5️⃣

> 🤖

> [端侧智能体模型要来了？不靠蒸馏，腾讯优图](https://mp.weixin.qq.com/s/jwIS-C4Z-PLj-JqOCaBwQg)推出2B原生智能体模型Youtu-LLM

>

+   🤖腾讯优图推出1.96B参数原生智能体模型Youtu-LLM，不依赖蒸馏从零预训练，适合端侧部署，通用任务媲美4B模型，复杂agent任务实现超越。

+   🌈核心创新含Dense MLA架构（提升注意力与长上下文）、四阶段螺旋预训练、200B+token高质量智能体轨迹数据（五大领域）。

+   🚀性能超同规模模型，智能体能力在APTBench等基准显著优于同规模，部分任务比肩4B模型。

+   📊消融实验证实智能体中间训练阶段提升40%+解决率，关键作用突出。

+   🔮局限：比百亿参数模型有差距；未来方向：世界模型、扩散LLM优化推理、多模态扩展；模型已开源。

> 关键词：Youtu-LLM，轻量级LLM，原生智能体，Dense MLA，智能体轨迹数据，端侧部署

>

> 🔗 文中引用的链接

+   [论文链接](https://huggingface.co/papers/2512.24618)

+   [模型地址](https://huggingface.co/collections/tencent/youtu)

+   [PaperScope解读](http://paperscope.ai/2512.24618)

+   [代码仓库（暂未开放）](https://github.com/TencentCloudADP/youtu-tip/youtu-llm)

> 6️⃣

> 🎯

> 我的秋招经历，大厂AI岗位面试真题总结

>

+   作者备战2025秋招整理AI岗位面试真题合集，覆盖大模型、VLM、RLHF、Agent、RAG等全链路技术栈，问题均来自真实面试经历🎯

+   内容包含Transformer自注意力、ROPE位置编码、MHA/MQA/GQA区别等大模型核心理论，及开源框架创新点分析📚

+   涉及VLM多模态对齐、CLIP原理，RLHF流程与DPO替代方案，Agent的ReAct框架与记忆系统等关键技术问题🔍

+   提供各部分真题参考答案链接，建议读者先独立思考再对照查漏补缺，避免直接背诵💡

+   还包含LLM前景（如AGI距离）、Agent落地瓶颈等开放性问题，助力求职准备🌟

> 关键词：AI岗位面试，大模型，RLHF，Agent，RAG

>

> 🔗 文中引用的链接

+   [原文章链接](https://mp.weixin.qq.com/s/S584V51Vi4qLkejMQ1ibAw)

+   [大模型参考答案](https://github.com/datawhalechina/hello-agents/blob/main/Extra-Chapter/Extra01-%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88.md#1-llm-%E5%85%AB%E8%82%A1)

+   [VLM参考答案](https://github.com/datawhalechina/hello-agents/blob/main/Extra-Chapter/Extra01-%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88.md#2-vlm-%E5%85%AB%E8%82%A1)

+   [RLHF参考答案](https://github.com/datawhalechina/hello-agents/blob/main/Extra-Chapter/Extra01-%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88.md#3-rlhf-%E5%85%AB%E8%82%A1)

+   [Agent参考答案](https://github.com/datawhalechina/hello-agents/blob/main/Extra-Chapter/Extra01-%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88.md#4-agent)

+   [RAG参考答案](https://github.com/datawhalechina/hello-agents/blob/main/Extra-Chapter/Extra01-%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88.md#5-rag)

+   [模型评估参考答案](https://github.com/datawhalechina/hello-agents/blob/main/Extra-Chapter/Extra01-%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88.md#6-%E6%A8%A1%E5%9E%8B%E8%AF%84%E4%BC%B0%E4%B8%8E-agent-%E8%AF%84%E4%BC%B0)

> （注：所有要点均紧扣原文核心，emoji随机选取且贴合内容，关键词精准覆盖AI技术领域与场景，链接完整保留文中引用资源）

>