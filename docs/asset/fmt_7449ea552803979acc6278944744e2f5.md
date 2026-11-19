
# 8🈷️02日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/JHsxdz6CVoXVoBxRomycpSOfnwb](https://v11enp9ok1h.feishu.cn/docx/JHsxdz6CVoXVoBxRomycpSOfnwb)
>
> 🔔AI情报助手晚报「08月02日」
>
> ✨✨✨✨✨✨✨✨

## 1️⃣ 🤖 谷歌最强模型 `Deep Think` 正式上线

- 2025年8月2日，谷歌最强模型`Gemini 2.5 Deep Think`正式上线，仅向`Gemini Ultra`用户开放，月费约1800元。
- 其核心武器是`Parallel Thinking`，通过并行探索不同解法、交叉验证来得出稳定答案，在数学等复杂场景优势大。
- `Deep Think`性能强大，在基准测试中表现出色，远超同类模型。
- 应用场景包括迭代设计、科研推理、高难度算法开发等硬核任务。
- 它更面向专业工作流，对相关领域或成高价值投资。

**关键词：** 谷歌，`Deep Think`，`Gemini Ultra`，平行思考，硬核任务

**🔗 文中引用的链接**

- [原文链接](https://mp.weixin.qq.com/s/5bmfqm_qgmz5h-99mZhKMA)

## 2️⃣ 🎨 抛开数学，轻松学懂`VAE`

- 本文绕过数学推导，从计算机科学角度讲解`VAE`核心原理，并给出`PyTorch`实现代码。
- 介绍图像生成方式及自编码器概念，指出其过拟合问题。
- `VAE`是正则化自编码器，编码器输出分布，通过`KL`散度约束分布相似度解决过拟合。
- 文中给出`VAE`在`CelebA`数据集上的模型搭建、训练及生成图片的完整代码。
- `VAE`虽重建和生成效果有限，但常作大模型子模块，学习它仍有必要。

**关键词：** `VAE`，自编码器，生成模型，`PyTorch`，图像生成

**🔗 文中引用的链接**

- [`VAE`讲解文章](https://towardsdatascience.com/understanding-variational-autoencoders-vaes-f70510919f73)
- `VAE` `PyTorch`实现参考仓库
- [项目网址](https://github.com/SingleZombie/DL-Demos/tree/master/dldemos/VAE)
- [`CelebA`数据集官网](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)

## 3️⃣ 🤖 CVPR'25｜华科&精测强强携手：`AnomalyNCD`

- 华科与精测提出`AnomalyNCD`，用自监督+掩膜引导策略实现工业缺陷多类别自主分类，无需人工标注。
- 该方法含主元素二值化、掩膜引导表征学习、区域面积加权合并三项革新技术。
- 其在`MVTecAD`、`MTD`数据集上性能提升显著，还能与主流异常检测模型无缝结合。
- `AnomalyNCD`代码已开源，论文及项目地址已给出。
- 华科慢工团队此前提出的`MuSc`与`AnomalyNCD`结合，实现高迁移性工业质检新体系。

**关键词：** 工业质检，`AnomalyNCD`，自监督学习，缺陷分类，`MuSc`

**🔗 文中引用的链接**

- [论文地址](https://arxiv.org/abs/2410.14379)
- [项目地址](https://github.com/HUST-SLOW/AnomalyNCD)
- [`MuSc`论文](https://arxiv.org/abs/2401.16753)
- [`MuSc`代码](https://github.com/xrli-U/MuSc)

## 4️⃣ 🤖 `NVIDIA GPU` Prefetch行为分析

- 作者因工作变动关注算子性能优化，在优化`MoE`算子时尝试使用`NVIDIA GPU`的`level::prefetch_size`特性，却未获预期性能提升。
- 实验对比`A100`、`L4`、`H20`三种架构`GPU`，设计程序观察Prefetch行为，发现多数情况下`level::prefetch_size`不起作用，仅`H20`特定条件下会预取数据。
- 构造`H20`上的`Pointwise tanh`激活和`Sum`操作实验，验证预取对性能影响，使用预取时`Kernel`运行时间缩短，`L2 Cache`命中率提高。
- 使用预取时，`Warp State`统计中`Stall Long Scoreboard`周期降低，提升了`Kernel`性能。
- 总结`level::prefetch_size`只是性能提示，作者后续可能研究其在异步拷贝指令的预取行为。

**关键词：** `NVIDIA GPU`，Prefetch，性能优化，`L2 Cache`，`CUDA Kernel`

**🔗 文中引用的链接**

- [https://mp.weixin.qq.com/s/tyopQEdjj8kBu4kP_qlyIQ](https://mp.weixin.qq.com/s/tyopQEdjj8kBu4kP_qlyIQ)
- [https://zhuanlan.zhihu.com/p/16266665042](https://zhuanlan.zhihu.com/p/16266665042)
- [https://docs.nvidia.com/cuda/parallel-thread-execution/#data-movement-and-conversion-instructions-ld](https://docs.nvidia.com/cuda/parallel-thread-execution/#data-movement-and-conversion-instructions-ld)
- [https://zhuanlan.zhihu.com/p/692445145](https://zhuanlan.zhihu.com/p/692445145)
- [https://docs.nvidia.com/cuda/parallel-thread-execution/#cache-operators](https://docs.nvidia.com/cuda/parallel-thread-execution/#cache-operators)
- [https://docs.nvidia.com/cuda/parallel-thread-execution/#cache-eviction-priority-hints](https://docs.nvidia.com/cuda/parallel-thread-execution/#cache-eviction-priority-hints)
- [https://docs.nvidia.com/nsight-compute/ProfilingGuide/index.html#id28](https://docs.nvidia.com/nsight-compute/ProfilingGuide/index.html#id28)
- [https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#coalesced-access-to-global-memory](https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#coalesced-access-to-global-memory)
- [https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#effective-bandwidth-calculation](https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html#effective-bandwidth-calculation)
- [https://github.com/HydraQYH/cuda_prefetch_experiment](https://github.com/HydraQYH/cuda_prefetch_experiment)
- [https://docs.nvidia.com/cuda/parallel-thread-execution/#data-movement-and-conversion-instructions-cp-async](https://docs.nvidia.com/cuda/parallel-thread-execution/#data-movement-and-conversion-instructions-cp-async)

## 5️⃣ 🤖 中国在AI领域超越美国已是板上钉钉？吴恩...

- 吴恩达分析中美AI竞争态势，认为中国凭借活跃开源模型生态与半导体领域进取，具备超越美国的潜在路径。
- 从排行榜看，美国主导顶尖闭源大模型，中国领先开源模型，且开源生态知识流动快。
- 半导体领域，华为推出系统与英伟达竞争，虽面临挑战，但加大自主研发动力。
- 吴恩达、黄仁勋均认可中国创新步伐。
- 文章指出中美在大模型发展理念上，中国开源做大蛋糕，美国闭源保持领先。

**关键词：** 中美AI竞争，开源模型，半导体，创新，发展理念

**🔗 文中引用的链接**

- [https://hai.stanford.edu/ai-index/2025-ai-index-report](https://hai.stanford.edu/ai-index/2025-ai-index-report)
- [https://x.com/AndrewYNg/status/1950941108000964654](https://x.com/AndrewYNg/status/1950941108000964654)
- [https://www.deeplearning.ai/the-batch/issue-312/](https://www.deeplearning.ai/the-batch/issue-312/)
- [https://mp.weixin.qq.com/s/7ZcFG6R9u7bMGxnPzoKqcA](https://mp.weixin.qq.com/s/7ZcFG6R9u7bMGxnPzoKqcA)
- [https://tv.cctv.com/2025/07/20/VIDE9OWCon6rhQmPtH3xwXJu250720.shtml](https://tv.cctv.com/2025/07/20/VIDE9OWCon6rhQmPtH3xwXJu250720.shtml)

## 6️⃣ 🤖 研发了八年基础软件，57岁的我在AI...

- 作者陶建辉研发`TDengine`八年后，推出`AI`原生的物联网、工业数据管理平台`TDengine IDMP 1.0`，让数据实现“无问智推”。
- 曾尝试开发`Chat BI`，后参考`PI System`，做好数据目录、标准化、情景化，将`TDengine`改造成`AI-Ready`数据平台。
- 借助`AI`大语言模型，自动推荐可视化面板、创建实时分析任务，降低业务洞察门槛，推动数智化转型。
- `TDengine`创新推动数据消费范式转变，从“拉取”变为“推送”，赋能`AI`应用挖掘数据价值。
- 作者坚信借助`AI`和中国工业市场，`TDengine`能颠覆传统工业数据管理平台。

**关键词：** `TDengine`，`AI`，工业数据管理，数据消费范式，无问智推

**🔗 文中引用的链接**

- [原文链接](https://mp.weixin.qq.com/s/q-1y41s5Byaw1633oMLXog)
