
# 12月17日 AI情报助手晚报

> 来源：[https://v11enp9ok1h.feishu.cn/docx/UcNjdr1qkorTzVxJ6fBcZBrKneb](https://v11enp9ok1h.feishu.cn/docx/UcNjdr1qkorTzVxJ6fBcZBrKneb)

## 1️⃣ 商汤「办公小浣熊3.0」：AI办公新体验

🪄 还在手搓PPT？试完这款AI，我连夜卸载...

- 商汤升级的「办公小浣熊3.0」实现脱胎换骨，可生成可用PPT、分析百万行数据、写视频脚本等，成为超级办公搭子。
- 其PPT生成通过人机协作（定义场景、投喂资料、大纲确认）解决过去AI生成的瑕疵问题，支持在线编辑元素，实现从草稿到交付的质变。
- 它是首个在线支持百万行数据分析的AI，企业侧精度达95%，能处理复杂数据，缩短业务分析周期90%，无需专业技能。
- 办公小浣熊APP即将上线，支持手机端处理数据、生成分析报告及可视化图表，适配移动办公场景。
- 它推出教育专项权益（305所高校5000+师生使用），实现全链路国产化适配，未来将布局AI盒子、AI PC。

### 关键词
办公小浣熊3.0，AI办公助手，PPT生成，百万行数据分析，国产化适配

### 引用链接
- [办公小浣熊官方入口](https://office.xiaohuanxiong.com/)

## 2️⃣ 英伟达开源大模型Nemotron3

🔥 黄仁勋不想只「卖铲...

- 2025年12月英伟达发布开源大模型家族Nemotron3（含Nano/Super/Ultra），融合Mamba、Transformer、MoE架构，Nano已上线，Super/Ultra计划2026上半年推出。
- Nemotron3 Nano参数30B（推理激活仅3B），主打边缘计算，吞吐量提升4倍，支持100万上下文窗口。
- 混合架构解决Transformer瓶颈：Mamba线性复杂度处理长文本+Transformer精准推理+MoE高效算力分配。
- NVFP4格式（Blackwell专属）压缩模型3.5倍且精度无损，实现软硬件锁定策略。
- 英伟达从卖硬件转向定义AI标准，通过开源推广Mamba架构，构建闭环生态应对竞品。

### 关键词
Nemotron3，Mamba架构，MoE，NVFP4，英伟达战略转型

### 引用链接
- [Mamba论文地址](https://arxiv.org/pdf/2312.00752)
- [NVFP4论文链接](https://arxiv.org/html/2509.25149v1)
- [英伟达新闻原文](https://nvidianews.nvidia.com/news/nvidia-debuts-nemotron-3-family-of-open-models)
- [VentureBeat报道](https://venturebeat.com/ai/nvidia-debuts-nemotron-3-with-hybrid-moe-and-mamba-transformer-to-drive)
- [Wired报道](https://www.wired.com/story/nvidia-becomes-major-model-maker-nemotron-3/)

## 3️⃣ Nature：深度学习x符号学习，是AGI唯一路径！

🔮 Nature重磅发文：深度学习x符号学习，是AGI唯一路径！

- 多数AI研究者认为仅靠神经网络无法实现AGI，神经符号融合或为唯一可行路径。
- 符号AI曾主导领域后被边缘化，2021年起两者融合升温，兼顾统计学习与显式推理。
- 神经网络速度快有创造力但易幻觉泛化差，符号系统推理清晰但难处理模糊概念需互补。
- 典型案例：AlphaGeometry解奥数题、Stockfish结合神经与符号树、AlphaGo用神经剪枝搜索树。
- IBM押注神经符号技术，多数学者认可其为AI注入逻辑推理的最佳方法。

### 关键词
神经符号融合，AGI，神经网络，符号AI，AlphaGeometry

### 引用链接
- [原文链接](https://mp.weixin.qq.com/s/nKCliLM6XMp7n-JEQxbiSg)
- [Nature参考资料](https://www.nature.com/articles/d41586-025-03856-1)

## 4️⃣ 猫娘是怎样炼成的——记录一次曲折的大模型微调经历

🐱 猫娘是怎样炼成的——记录一次曲折的大模型微调经历

- 作者因舍友用系统提示词构建的猫娘角色易被带偏，决定用NecoQA-10K猫娘对话数据集对Qwen3-14B进行监督微调（SFT）。
- 在Windows11环境下使用unsloth框架，解决CUDA版本不匹配、Unicode解码错误等问题，最终采用全量模型成功微调。
- 采用QLoRA（秩16）微调1个epoch，将模型转GGUF格式导入Ollama时因模板缺失效果差，修正后恢复正常。
- 微调后模型在unsloth推理效果良好，但仍被攻击性提示词带偏，推测因LoRA秩低、数据集局限性等原因。
- 总结大模型训练需经验与高质量数据集，微调后指令遵循能力下降，但有瞬间真实感，期待未来技术提升。

### 关键词
大模型微调，SFT，NecoQA-10K数据集，unsloth框架，Ollama部署

### 引用链接
- [原文链接](https://mp.weixin.qq.com/s/ZntU4A1nfw2MNvAYPS_k8g)
- [知乎授权来源](https://zhuanlan.zhihu.com/p/1980670280922861774)
- [NecoQA-10K数据集](https://zhuanlan.zhihu.com/p/1934983798233231689)
- [unsloth框架](https://github.com/unslothai/unsloth.git)
- [PyTorch官网](https://pytorch.org/)
- [llama.cpp相关](https://github.com/ggerganov/llama.cpp)
- [unsloth安装教程](https://github.com/unslothai/unsloth.git)
- [Qwen3模型](https://hf-mirror.com/unsloth/Qwen3-14B)

## 5️⃣ XY-Serve：华为&清华团队在昇腾NPU上实现95%吞吐提升

🏆 [聊聊大模型推理系统之 XY-Serve：华为&清华团队如何在昇腾NPU上实现95%吞吐提升？](https://mp.weixin.qq.com/s/AgbBSZm-gwpJOmM_js0B2g)

- 华为与清华联合推出XY-Serve，原生适配昇腾NPU的通用LLM推理系统，端到端吞吐量最高提升95%，Linear/Attention算子平均提速14.6%/21.5%，MFU/MBU与NVIDIA A800持平或领先。
- XY-Serve针对大模型推理动态性（输入输出长度、掩码、多阶段调度）问题，提出硬件-算法-系统协同方案，将动态负载抽象为硬件友好的元原语。
- 核心创新包括任务分解与重排序（均衡调度元原语）、Meta-Attention（统一处理所有Attention场景，优化流水线）、SmoothGEMM（虚拟填充支持任意形状GEMM）。
- 性能上，XY-Serve比Ascend-vLLM端到端最高提升89%，工业级负载提升95%，迁移至GPU平台仍获延迟降低和利用率提升。
- XY-Serve推动国产AI加速器生态建设，助力人工智能基础设施自主可控，为动态性推理提供新思路并兼容前沿优化技术。

### 关键词
XY-Serve，昇腾NPU，大模型推理，动态性优化，软硬件协同

### 引用链接
- [原文链接](https://mp.weixin.qq.com/s/AgbBSZm-gwpJOmM_js0B2g)
- [论文链接](https://doi.org/10.1145/3760250.3762228)

## 6️⃣ 企业级智能体落地：四大坑与无问芯穹解决方案

🔔 用企业级智能体落地，还有谁没踩这四种大坑...

- 企业落地智能体常遇四大坑：效果易衰减、规模化稳定性差、成本高企、商业化闭环难。
- 无问芯穹推出智能体服务平台，提供全栈能力解决上述问题，成为智能体时代基础设施。
- 平台通过模板化能力、模型追踪优化效果，弹性扩缩容保障规模化运行稳定。
- 平台深度整合模型推理优化（如千亿模型效率提升3-5倍），助企业平衡效果与成本。
- 平台支持工具链、渠道及支付接入，助力商业化闭环，SysCoding Agent案例验证价值。

### 关键词
智能体，企业级落地，Agent Infra，无问芯穹智能体服务平台，成本优化

### 引用链接
- [原文链接](https://mp.weixin.qq.com/s/7RoXgYAN4NvJmKGHETJ5xg)
