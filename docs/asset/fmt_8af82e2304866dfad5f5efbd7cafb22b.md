
实验表明，CacheBlend在生成质量和效率方面实现了平衡。相比Prefix Caching，其KV复用率更高，生成延迟更低；相比Full KV Reuse，交叉注意力的恢复保证了更高的生成质量。CacheBlend构建了一个多级存储系统，动态加载和计算缓存，为RAG场景提供了高效的解决方案

![](img/718431a3e4b83d4b585df5c3464b7f52.png)

## LLM评测Takeaway

https://zhuanlan.zhihu.com/p/7713134123

为了解决现有评测框架在融入自定义代码流程中的复杂性和拓展性问题，作者设计了一种针对不同Benchmark的独立评测流程。其核心在于为每个Benchmark单独建立文件夹，内含主函数和工具函数，通过灵活的参数配置、标准化的数据加载、定制化的生成后处理实现高效评测。以下为关键技术细节总结：

首先，为每个Benchmark创建独立文件夹，并设计包含`argparse`参数配置的主函数。主要参数包括模型路径（`checkpoint path`）、数据加载参数（如`num_workers`和`batch_size`）、生成配置（如`temperature`、`topk`、`topp`等）、以及其他模型独立参数。模型和分词器通过`from_pretrained`方法从指定路径加载，数据集根据Benchmark原始数据格式组织到`getitem`中，并通过自定义的`Dataloader`打包。

生成阶段中，`generation_config`设置了核心参数如`max_new_tokens`、`eos_token_id`等。对输入样本进行会话格式化时，可利用模型兼容的`Conversation` Class或`apply_chat_format`方法，根据模型需求配置`role`信息、分隔符和系统提示。生成结果通过`generate`函数推理，需注意`eos_token_id`的正确性，否则可能生成无关内容。

后处理对生成结果进一步解析。利用`batch_decode`将模型预测的`id`转为自然语言后，可根据具体Benchmark的要求执行正则匹配。例如，Math类任务可通过识别`final answer`句子后处理Latex语法或单位信息；Code类任务则需更复杂的解析逻辑。作者指出，对于zero-shot任务，可通过设计instruction引导模型用特定标记（如`\box{}`）输出答案，便于正则化抽取。

分布式场景下，作者通过自定义`InferenceSampler`和`collate_fn`实现多卡加速。`InferenceSampler`根据进程数划分数据范围，确保分布式数据分配的一致性；`collate_fn`则定义如何将每条数据组装成批次。`Dataloader`实例化时结合了自定义的`Sampler`和`collate`函数，支持高效的数据加载。

误差来源主要包括`generation_config`的设置、输入提示格式、预测后处理等。其中，后处理对于Math、Code等任务影响尤为显著，可能因正则逻辑不当导致测试精度上下浮动达15%。作者通过案例分析展示了后处理失败的典型场景，例如模型不同总结用词（如`final answer` vs therefore）导致对答案提取的偏差，highlight了针对不同模型的后处理逻辑适配的重要性。

作者总结了从`lm_eval`、`opencampass`等开源项目中获得的启发，并分享了相关代码库链接，表明开源资源在搭建和优化评测流程中的价值。

![](img/8c857255cb7a49ea0de6fb7bb7fdac27.png)

## Implementing a fast Tensor Core matmul on the Ada Architecture

https://www.spatters.ca/mma-matmul

在Ada架构上实现高效的Tensor Core矩阵乘法（matmul）内核

概述

本文详细描述了如何在NVIDIA Ada架构GPU上开发高效的Tensor Core矩阵乘法（matmul）内核，从最初的朴素实现到性能接近`cuBLAS`的优化版本。通过引入如`CUTLASS`库的技术、异步数据传输以及高效的共享内存布局，最终达到了高达RTX 4090峰值性能的93%。

技术细节

1. 问题定义：

+   矩阵规格为 `M=N=K=4096`，采用`fp16`输入与`fp32`累加。

+   单次计算涉及超过3300万次 `m16n8k16` 指令。

1. 性能优化过程：

+   `Kernel 1.0`: 基础实现，采用直接加载数据的方式，性能仅为`cuBLAS`的19%。

+   `Kernel 2.0`: 引入`CUTLASS`共享内存排列，减少银行冲突，将性能提升至82.9%。

+   `Kernel 3.0`: 利用`cp.async`指令实现多阶段异步流水线，进一步减少全局内存读取延迟。

1. 异步流水线：

+   通过将数据预加载到共享内存并利用循环缓冲实现流水线处理，显著减少了内存访问开销。

+   采用warp级别的指令（如`ldmatrix`和`mma.sync`），优化线程块计算效率。

1. 最终结果：

+   `Kernel 3.1`: 增加每个warp的计算量，通过4倍平铺技术（tiling），将性能提升至153.6 TFLOP/s，与`cuBLAS`持平。

工具与技术亮点

+   使用低级PTX指令如`mma`和`ldmatrix`直接操作Tensor Core。

+   共享内存的排列避免了银行冲突，并配合向量化加载优化全局内存访问。

+   通过CUDA工具（如`nsight-compute`）精确分析性能瓶颈，如Tensor Core等待和同步开销。

![](img/324b85bd3e71fbea4ae0a634293050bc.png)

## Neurips 2024 Tutorial: Beyond Decoding: Meta-Generation Algorithms for Large Language Models

https://cmu-l3.github.io/neurips2024-inference-tutorial/

本教程将于 2024年12月10日 在温哥华的 NeurIPS 上举行，主题聚焦于大型语言模型（LLMs）推理阶段的生成算法，从传统解码技术到复杂的元生成算法，旨在提升生成质量、增加多样性，并实现资源有限环境下的高效生成。

核心内容

1. 生成算法演进：介绍从贪婪解码到复杂元生成算法的历程，探讨如何通过优化推理阶段的计算资源来提升输出质量。

1. 跨学科视角：结合自然语言处理、现代LLMs和机器学习系统三大研究领域的视角，提出高效生成的解决方案。

1. 资源高效化：重点讲解如何在降低生成成本和提高生成速度的同时，维持甚至提升生成质量。

1. 元生成算法应用：展示如何利用LLMs在更复杂任务（如多步骤推理、知识检索与生成）中优化表现。

融合理论与实践

教程将包括多位研究人员的讲解，从基本概念到高级技术逐步展开。附带的阅读材料和代码资源为参会者提供进一步探索的途径。

重要信息

+   时间：2024年12月10日，下午1:30 - 4:00（温哥华时间）。

+   地点：West Exhibition Hall C, NeurIPS。

+   主讲人：由来自卡耐基梅隆大学、南加州大学、Anthropic、Meta AI等机构的研究人员组成。

+   阅读材料：涵盖元生成算法最新研究和高效生成技术。

![](img/ead5bed102c478d583e79ceb700c227d.png)

## 统计方法在语言模型评估中的应用

https://www.anthropic.com/research/statistical-approach-to-model-evals

Anthropic近期发布了一篇关于语言模型评估（Evals）的研究论文，提出了一系列基于统计学的改进建议，以提升AI模型评估的科学性和准确性。文章从如何量化模型性能差异的真实性入手，提出了五项核心建议：

核心建议

1. 利用中心极限定理：

+   评估分数通常是大量问题得分的平均值，但研究强调应关注“问题宇宙”中的理论平均值，而非仅仅依赖观察值。

+   使用中心极限定理可以更稳健地估算标准误差（SEM），并计算置信区间，从而更准确地量化模型能力差异。

1. 聚类标准误差：

+   对非独立问题（如基于相同文本段的多个问题），建议采用聚类标准误差方法，以避免低估误差，确保结论的可靠性。

1. 减少问题内部方差：

+   通过多次生成和评分答案，或者使用语言模型的下一词概率作为评分标准，减少随机误差，从而提高评估精度。

1. 分析配对差异：

+   通过配对差异测试（paired-difference test）消除问题难度的影响，仅关注模型之间的响应差异。这种方法可以显著减少误差，并提供更清晰的比较结果。

1. 运用功效分析：

+   利用统计功效分析，计算评估所需的最少问题数量，以确保对模型能力差异的检测足够敏感。研究建议开发者在设计新评估时参考此方法。

研究意义

+   本文通过引入统计学理论和实验设计方法，明确了语言模型评估中数据分析和解释的标准化路径。

+   提出的改进方法将有助于研究人员更科学地设计评估、解释结果，并在对比不同模型性能时减少随机性和偏差的影响。

![](img/2195d73469dfd5ee9879e78caeca5de5.png)

# 招聘公告

---

![](img/8fd48099a102e889be2ba69b8e41a123.png)

奇绩创坛由陆奇博士于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

## 🚀F24实习岗位

[行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

行研-GenAI研究岗位-战略分析（偏产品）

Web / AI 全栈开发工程师实习生

## 火热开放中🚀

行研-GenAI研究岗位-前沿学术探索

行研-GenAI研究岗位-战略分析（偏咨询）

创新活动产品运营 （大模型系列活动产品运营）

> 点击相关岗位可查看对应岗位需求【JD】；相关岗位投递请扫描二维码

![](img/4733710c82455a90ad48fe5800474ed9.png)

> 欢迎推荐/自荐

> 期待在奇绩创坛遇见更好的你

---

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack；Wenyu；Chenyang；Zhiyuan；Millian；Xudong；Yichen；Shangze；Mingyue；Ollie；Jiaqi；Weibo

学术分析报告：`ResearchFlow` -- 奇绩F23校友的开发的深度研究产品，PC端进入`RFlow`的分析报告，可直接点击节点右侧的小数字展开节点，登录后可在节点上直接“询问AI”，进一步探索深度信息

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> 【日报知识库】这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/bc9b74cee0fd53c3a21e83f3f44afe1b.png)
