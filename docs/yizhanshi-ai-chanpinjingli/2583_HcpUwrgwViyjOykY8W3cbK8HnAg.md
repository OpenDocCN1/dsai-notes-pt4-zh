# 8🈷️02日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/JHsxdz6CVoXVoBxRomycpSOfnwb](https://v11enp9ok1h.feishu.cn/docx/JHsxdz6CVoXVoBxRomycpSOfnwb)

> 🔔AI情报助手晚报「08月02日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> 谷歌最强模型 Deep Think 正式...

>

+   2025年8月2日，谷歌最强模型Gemini 2.5 Deep Think正式上线，仅向Gemini Ultra用户开放，月费约1800元。

+   其核心武器是Parallel Thinking，通过并行探索不同解法、交叉验证来得出稳定答案，在数学等复杂场景优势大。

+   Deep Think性能强大，在基准测试中表现出色，远超同类模型。

+   应用场景包括迭代设计、科研推理、高难度算法开发等硬核任务。

+   它更面向专业工作流，对相关领域或成高价值投资。

> 关键词：谷歌，Deep Think，Gemini Ultra，平行思考，硬核任务

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/5bmfqm_qgmz5h-99mZhKMA)

> 2️⃣

> 🎨

> [抛开数学，轻松学懂VAE](https://mp.weixin.qq.com/s/LilXkI_j2X63PaHkoxFwOw)

>

+   本文绕过数学推导，从计算机科学角度讲解VAE核心原理，并给出PyTorch实现代码。

+   介绍图像生成方式及自编码器概念，指出其过拟合问题。

+   VAE是正则化自编码器，编码器输出分布，通过KL散度约束分布相似度解决过拟合。

+   文中给出VAE在CelebA数据集上的模型搭建、训练及生成图片的完整代码。

+   VAE虽重建和生成效果有限，但常作大模型子模块，学习它仍有必要。

> 关键词：VAE，自编码器，生成模型，PyTorch，图像生成

>

> 🔗 文中引用的链接

+   [VAE讲解文章](https://towardsdatascience.com/understanding-variational-autoencoders-vaes-f70510919f73)

+   VAE PyTorch实现参考仓库

+   [项目网址](https://github.com/SingleZombie/DL-Demos/tree/master/dldemos/VAE)

+   [CelebA数据集官网](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)

> 3️⃣

> 🤖

> CVPR'25｜华科&精测强强携手：An...

>

+   华科与精测提出AnomalyNCD，用自监督+掩膜引导策略实现工业缺陷多类别自主分类，无需人工标注。

+   该方法含主元素二值化、掩膜引导表征学习、区域面积加权合并三项革新技术。

+   其在MVTecAD、MTD数据集上性能提升显著，还能与主流异常检测模型无缝结合。

+   AnomalyNCD代码已开源，论文及项目地址已给出。

+   华科慢工团队此前提出的MuSc与AnomalyNCD结合，实现高迁移性工业质检新体系。

> 关键词：工业质检，AnomalyNCD，自监督学习，缺陷分类，MuSc

>

> 🔗 文中引用的链接

+   [论文地址](https://arxiv.org/abs/2410.14379)

+   [项目地址](https://github.com/HUST-SLOW/AnomalyNCD)

+   [MuSc论文](https://arxiv.org/abs/2401.16753)

+   [MuSc代码](https://github.com/xrli-U/MuSc)

> 4️⃣

> 🤖

> NVIDIA GPU Prefetch行为分析

>

+   作者因工作变动关注算子性能优化，在优化MoE算子时尝试使用NVIDIA GPU的level::prefetch_size特性，却未获预期性能提升。

+   实验对比A100、L4、H20三种架构GPU，设计程序观察Prefetch行为，发现多数情况下level::prefetch_size不起作用，仅H20特定条件下会预取数据。

+   构造H20上的Pointwise tanh激活和Sum操作实验，验证预取对性能影响，使用预取时Kernel运行时间缩短，L2 Cache命中率提高。

+   使用预取时，Warp State统计中Stall Long Scoreboard周期降低，提升了Kernel性能。

+   总结level::prefetch_size只是性能提示，作者后续可能研究其在异步拷贝指令的预取行为。

> 关键词：NVIDIA GPU，Prefetch，性能优化，L2 Cache，CUDA Kernel

>

> 🔗 文中引用的链接

+   [https://mp.weixin.qq.com/s/ty](https://mp.weixin.qq.com/s/tyopQEdjj8kBu4kP_qlyIQ)opQEdjj8kBu4kP_qlyIQ

+   [https://zhuanlan.zhi](https://zhuanlan.zhihu.com/p/16266665042)hu.com/p/16266665042

+   [ht](https://docs.nvidia.com/cuda/parallel-thread-execution/#data-movement-and-conversion-instructions-ld)tps://docs.nvidia.com/cuda/parallel-thread-execution/#data-movement-and-conversion-instructions-ld

+   [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/692445145)hihu.com/p/692445145

+   [h](https://docs.nvidia.com/cuda/parallel-thread-execution/#cache-operators)ttps://docs.nvidia.com/cuda/parallel-thread-execution/#cache-operators

+   [https://docs.nvidia.com/cuda/parallel-thread-](https://docs.nvidia.com/cuda/parallel-thread-execution/#cache-eviction-priority-hints)execution/#cache-eviction-priority-hints

+   [h](https://docs.nvidia.com/nsight-compute/ProfilingGuide/index.html#id28)ttps://docs.nvidia.com/nsight-compute/ProfilingGuide/index.html#id28

+   [ht](https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#coalesced-access-to-global-memory)tps://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#coalesced-access-to-global-memory

+   [ht](https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#effective-bandwidth-calculation)tps://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#effective-bandwidth-calculation

+   [h](https://github.com/HydraQYH/cuda_prefetch_experiment)ttps://github.com/HydraQYH/cuda_prefetch_experiment

+   [ht](https://docs.nvidia.com/cuda/parallel-thread-execution/#data-movement-and-conversion-instructions-cp-async)tps://docs.nvidia.com/cuda/parallel-thread-execution/#data-movement-and-conversion-instructions-cp-async

> 5️⃣

> 🤖

> 中国在AI领域超越美国已是板上钉钉？吴恩...

>

+   吴恩达分析中美AI竞争态势，认为中国凭借活跃开源模型生态与半导体领域进取，具备超越美国的潜在路径。

+   从排行榜看，美国主导顶尖闭源大模型，中国领先开源模型，且开源生态知识流动快。

+   半导体领域，华为推出系统与英伟达竞争，虽面临挑战，但加大自主研发动力。

+   吴恩达、黄仁勋均认可中国创新步伐。

+   文章指出中美在大模型发展理念上，中国开源做大蛋糕，美国闭源保持领先。

> 关键词：中美AI竞争，开源模型，半导体，创新，发展理念

>

> 🔗 文中引用的链接

+   [h](https://hai.stanford.edu/ai-index/2025-ai-index-report)ttps://hai.stanford.edu/ai-index/2025-ai-index-report

+   [https://x.com/AndrewYNg/status](https://x.com/AndrewYNg/status/1950941108000964654)/1950941108000964654

+   [https://www.deeplearning.ai/](https://www.deeplearning.ai/the-batch/issue-312/)the-batch/issue-312/

+   [https://mp.weixin.qq.com/s/7Z](https://mp.weixin.qq.com/s/7ZcFG6R9u7bMGxnPzoKqcA)cFG6R9u7bMGxnPzoKqcA

+   [h](https://tv.cctv.com/2025/07/20/VIDE9OWCon6rhQmPtH3xwXJu250720.shtml)ttps://tv.cctv.com/2025/07/20/VIDE9OWCon6rhQmPtH3xwXJu250720.shtml

> 6️⃣

> 🤖

> 研发了八年基础软件，57岁的我在AI...

>

+   作者陶建辉研发TDengine八年后，推出AI原生的物联网、工业数据管理平台TDengine IDMP 1.0，让数据实现“无问智推”。

+   曾尝试开发Chat BI，后参考PI System，做好数据目录、标准化、情景化，将TDengine改造成AI-Ready数据平台。

+   借助AI大语言模型，自动推荐可视化面板、创建实时分析任务，降低业务洞察门槛，推动数智化转型。

+   TDengine创新推动数据消费范式转变，从“拉取”变为“推送”，赋能AI应用挖掘数据价值。

+   作者坚信借助AI和中国工业市场，TDengine能颠覆传统工业数据管理平台。

> 关键词：TDengine，AI，工业数据管理，数据消费范式，无问智推

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/q-1y41s5Byaw1633oMLXog)