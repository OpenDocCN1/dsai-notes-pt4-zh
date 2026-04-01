# 04🈷️25日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/P2red2Wz8oJGqCxr1KVcGx8Tneh](https://v11enp9ok1h.feishu.cn/docx/P2red2Wz8oJGqCxr1KVcGx8Tneh)

> 🔔AI情报助手早报「04月25日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> [用纳米AI的MCP工具箱，爆改小红书笔记+播](https://mp.weixin.qq.com/s/JqBt8jgmB0ZKSwPWGJMAyg)客内容，内附50个智能体应用场景及提示词

>

+   纳米AI MCP万能箱上线，集成工具与信息源，无需代码知识与搭建服务，内置多智能体与工具。

+   官方有众多智能体案例，可快速搭建自制智能体，文中整理了50个应用场景及提示词。

+   测评了深度研究智能体、小红书浏览机器人、动态海报生成器三个智能体，展示其功能与效果。

+   教用户5分钟搭建自动抓取新闻并生成播报音频的智能体，介绍搭建步骤。

+   鼓励内容创作者等尝试新AI工具，提升生产力。

> 关键词：纳米AI，MCP工具箱，智能体，内容创作，效率提升

>

> 🔗 文中引用的链接

+   [纳米AI MCP万能箱下载链接](https://bot.n.cn/download?src=AIBotCode)

+   [深度研究智能体链接](https://bot.n.cn/tools/aiagent/chat/9113bbe12b834c298ab65f7e54edbfd0?share=1)

+   [小红书浏览机器人链接](https://bot.n.cn/tools/aiagent/chat/aiagent007?share=1)

+   [动态海报生成器链接](https://bot.n.cn/tools/aiagent/chat/103af684cff34f72a1d0f5c09bb3b4b5?share=1)

+   [黄金产业链分析网站](https://gewpsm.n.cn/)

> 2️⃣

> 🎸

> 一文搞懂主流模型格式（.pth/.pt和.bin）

>

+   在深度学习开发中，模型文件格式选择是难题，本文剖析PyTorch原生格式（.pth/.pt）和通用二进制格式（.bin）。

+   .pth/.pt基于pickle模块序列化，与PyTorch框架深度融合，适用于模型保存、迁移与共享。

+   .bin是通用二进制格式，只存储模型权重数据，文件紧凑，适用于模型部署和跨框架兼容。

+   文章通过代码示例展示两种格式在模型保存与加载方面的操作。

+   帮助开发者了解不同模型格式原理与应用，扫清模型应用障碍。

> 关键词：深度学习，模型格式，.pth/.pt，.bin，模型部署

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/xUBuFh2scbJpqf-Vlxxozg)

> 3️⃣

> 🎯

> [【博客翻译】CUDA中的索引](https://mp.weixin.qq.com/s/Z0pMzG5XXxNX1-_81-B0WQ)

>

+   解释CUDA中行优先格式矩阵，2D数组坐标映射为i  N + j ，3D数组为i  M2  M3 + j  M3 + l 。

+   以2D Block Tiling代码为例，分析CUDA kernel索引，如As、Bs共享内存填充及索引推导。

+   As被解释为(BM/TM, TM, BK) 3D数组，Bs为(BK, BN/TN, TN) ，以实现内存块Tiling。

+   算法将(BM, BK)和(BK, BN)矩阵相乘，在BK维度循环，累积结果到(TM, TN)数组。

+   结果写回内存，C被解释为(M/TM, TM, N/TN, TN)矩阵，以Tiling方式写回。

> 关键词：CUDA，索引，行优先格式，2D Block Tiling，矩阵相乘

>

> 🔗 文中引用的链接

+   2D Block Tiling解释

+   [完整代码](https://github.com/siboehm/SGEMM_CUDA/blob/master/src/kernels/5_kernel_2D_blocktiling.cuh)

+   [原始地址](https://veitner.bearblog.dev/indexing-in-cuda/)

> 4️⃣

> 🤖

> 大模型评估排障指南 | 关于推理

>

+   模型运行慢，可调整batch size、数据并行、优化代码、调整精度。

+   内存占用大，可估算内存需求，按公式计算。

+   一个GPU装不下，可量化、模型并行、CPU减负。

+   模型装进GPU仍报OOMs错误，可能是上下文大小问题，可加载dummy数据测试、降低batch size等。

+   文章为大模型评估排障系列首篇，聚焦推理相关问题。

> 关键词：大模型，推理，评估，排障

>

> 🔗 文中引用的链接

+   [h](https://pytorch.org/serve/performance_checklist.html)ttps://pytorch.org/serve/performance_checklist.html

+   https://github.com/pytorch/PiPPy

+   [h](https://hf.co/docs/transformers/v4.15.0/en/parallelism)ttps://hf.co/docs/transformers/v4.15.0/en/parallelism

+   https://arxiv.org/abs/2101.06840

+   [https://raw.githubusercontent.com/huggingface/evaluation-guidebook/refs/](https://raw.githubusercontent.com/huggingface/evaluation-guidebook/refs/heads/main/translations/zh/contents/troubleshooting/troubleshooting-inference.md)heads/main/translations/zh/contents/troubleshooting/troubleshooting-inference.md

> 5️⃣

> 🤖

> 扣子空间+自定义MCP，我的学习搭子来了！（附邀请码）

>

+   扣子空间是集成AI Agent并内置MCP服务的智能工作平台，零代码、全自动完成任务，周末上线爆火。

+   其更新后支持自定义MCP，可结合Datawhale AI春训营学习，搭建AI学习搭子，解决理解需求、代码注释等学习难点。

+   扣子有探索和规划两种模式，利用deepsearch边想边做，还能与飞书文档等打通写笔记。

+   除官方MCP服务，还能用开发平台插件和个人DIY工作流自定义MCP。

+   文末提供了试用邀请码。

> 关键词：扣子空间，自定义MCP，AI学习搭子，飞书文档，邀请码

>

> 🔗 文中引用的链接

+   [扣子空间](https://space.coze.cn/)

+   [新能源发电功率预测大赛项目背景](http://competition.sais.com.cn/competitionDetail/532315/format)

+   [baseline代码](https://www.modelscope.cn/datasets/loutianao/new_energy_power_forecast/file/view/master?id=91148&status=1&fileName=power_pred_baseline.ipynb)

+   [新能源发电功率预测大赛数据](https://www.modelscope.cn/datasets/loutianao/new_energy_power_forecast/summary)

+   [新能源发电功率预测大赛比赛介绍](http://competition.sais.com.cn/competitionDetail/532315/competitionData)

> 6️⃣

> 🤖

> 一天拿下3.4k star，这个1.6B开源模型火了，合成对话超逼真

>

+   Dia-1.6B是Nari Labs开源的1.6B参数文本转语音模型，能生成逼真语音及情绪声音，上线GitHub不到24小时获超3.4k star。

+   与其他逼真模型对比，Dia-1.6B在情绪表达上优势明显，合成简单英语对话表现卓越，但理解指令和音频质量有不足。

+   模型仅支持英语生成，仅在GPU测试，企业级GPU可实时生成音频，完整版本需约10GB显存。

+   Nari Labs是小团队，成员为本科生，目标是构建媲美知名TTS模型，计划将Dia做成B2C应用。

+   可扫码申请加入技术交流群。

> 关键词：Dia-1.6B，开源模型，文本转语音，Nari Labs

>

> 🔗 文中引用的链接

+   [GitHub](https://github.com/nari-labs/dia/)

+   [Hugging Face](https://huggingface.co/nari-labs/Dia-1.6B)

+   [试用地址](https://huggingface.co/spaces/nari-labs/Dia-1.6B)

+   [等待列表](https://tally.so/r/meokbo)