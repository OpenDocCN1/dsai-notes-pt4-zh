
`Last24.ai` 是一个人工智能搜索引擎，通过思维导图形式快速总结最新的新闻，帮助用户轻松获取重要信息。它从多个新闻来源提取内容，支持个性化编辑，并提供深入链接，适合希望快速了解新闻的视觉学习者。

![](img/28cd3ba105c3d5f68f11bb3f19766b9c.png)

# HuggingFace&Github

## `Wren Engine`

贡献人：

https://github.com/Canner/wren-engine

`Wren Engine` 是一个为大型语言模型（`LLMs`）设计的语义引擎，旨在与可组合数据系统兼容。它具备可嵌入性和互操作性，允许用户通过 `API` 重用语义上下文，并连接本地和云数据源。

![](img/243f3456ee914802e0a0b91f7bcf11cb.png)

![](img/0f23c4ea58a53d63c69a5371d2174d0d.png)

## `Table LLM`

贡献人：

https://github.com/RUCKBReasoning/TableLLM

`TableLLM` 可以有效地处理表格数据操作的任务。它包含两种不同规模的模型：`TableLLM-7B` 和 `TableLLM-13B`。`TableLLM` 根据不同的场景生成代码解决方案或直接文本答案来处理表格数据操作任务。

# 投融资

## Canva收购`Leonardo.ai`以增强其生成式`AI`能力

贡献人：

https://techcrunch.com/2024/07/29/canva-acquires-leonardo-ai-to-boost-its-generative-ai-efforts/

`Canva`收购了生成式`AI`内容和研究初创公司`Leonardo.ai`，以加深其在`AI`技术堆栈上的投资。此次交易的财务条款未披露，但`Canva`联合创始人兼首席产品官Cameron Adams表示，交易包括现金和股票。`Leonardo.ai`的全部120名员工将加入`Canva`，包括高管团队。`Leonardo.ai`此前获得了超过3880万美元的资本投资，投资方包括Smash Capital、Blackbird、Side Stage Ventures、TIRTA Ventures、Gaorong Capital和`Samsung Next`。此次收购旨在帮助`Leonardo.ai`扩大平台并加深用户增长，特别是在`API`业务和基础模型研发方面的投资。

公司官网：https://leonardo.ai/

![](img/3795cba54956b18b74eab8a727b99f87.png)

## 前`Squad`和`Twitter`高管筹集700万美元打造硬件`AI`助手

贡献人：

https://techcrunch.com/2024/07/29/former-squad-and-twitter-execs-raise-7m-to-build-a-hardware-ai-assistant/

前`Squad`和`Twitter`高管创立的`Bee AI`筹集了700万美元，以开发其可穿戴的`AI`助手，该助手通过聆听用户对话来学习。此次融资由`Exor`领投，包括之前筹集的150万美元种子资金，参与者还有`Greycroft`、`New Wave VC`、`Banana Capital`和投资人`Brian Bedol`。`Bee AI`的设备和应用程序旨在通过云手机镜像用户的手机，执行通知提醒、写邮件和购物建议等任务，并计划在黑色星期五前接受预订，售价49美元，每月订阅费用为19美元。

![](img/08fd1b8d1aa2fecb495f37abfd695fe1.png)

## `IntelePeer`获1.4亿美元融资，用`AI Agent`增强客户服务

贡献人：

https://intelepeer.ai/news-press/intelepeer-secures-140-million-strategic-growth-investment-to-drive-the-development-of-ai-agents-for-improved-customer-experiences

`IntelePeer`宣布获得由`Savant Growth LLC`和`VantagePoint Capital Partners`牵头的1.4亿美元战略增长投资和债务融资，目标是通过`AI`和自动化技术重塑联络中心并改善客户体验。此次融资将用于扩展`AI`代理产品组合，并加强产品开发、销售和营销团队。投资还包括`Vector Capital`的新成立的直接信贷策略`Vector Velocity`的债务融资部分。`IntelePeer`计划通过此投资继续推动其在`AI`业务上的快速增长，进一步实现客户自助服务自动化的愿景。

公司官网：https://intelepeer.ai/

![](img/e85a6662d53a5cea016e1fb152a65639.png)

# 学习

## 探索混合专家（`MoE`）模型预训练：开源项目实操

贡献人：

https://zhuanlan.zhihu.com/p/708804769

`MoE` 模型通过结构优化，实现了在推理时仅激活部分专家层，从而降低了计算成本。具体实现包括：调整输入形状并添加抖动噪声以增强模型鲁棒性；使用门控网络计算专家权重，并通过 `softmax` 函数选择 `top-k` 个专家层进行计算；创建专家掩码以便索引；循环计算每个专家层的输出并累加到最终隐藏状态；最后恢复形状并返回结果。例如，`Mixtral 8X7B` 模型在每次推理时只使用两个专家层，实际计算的参数量远小于总参数量。实验结果显示，`MoE` 模型在激活参数更少的情况下，能够在训练效果上超过 `Dense` 模型。`DeepSeek MoE` 模型通过增加专家层数量至 64 个并引入 share expert 机制，进一步提升了模型效率和效果。

![](img/0c122e939c7bdd17bb62719f967eaed9.png)

## `NaplesPU`或`NPU`技术开发文档合成全部

贡献人：

https://zhuanlan.zhihu.com/p/711324957?utm_psn=1801516909289684993

文章深入浅出地阐述了 `NPU` 的技术细节，包括其架构设计、编程模型、性能优化以及实际应用。`NPU` 的数据流架构通过优化数据传输和处理，实现了高效的并行计算。编程模型中，`NPU` 的指令集和编译器协同工作，确保了计算任务的高效执行。性能优化部分，文档详细介绍了如何通过调试工具和编程技巧，充分发挥 `NPU` 的性能潜力。此外，文档还探讨了 `NPU` 在实际应用中的挑战与应对策略，以及未来技术发展的方向。

![](img/f97769ac2a72215160fc8c8f194bf153.png)

## 算能端侧 `AI` 盒子 `Stable Diffusion` 一秒一张图：`AirBox BM1684X`

贡献人：

https://zhuanlan.zhihu.com/p/711640323?utm_psn=1801516600261742592

算能端侧 `AI` 盒子 `AirBox BM1684X` 是一款集成了 `Tensor Computing Processor BM1684X` 的 `AI` 计算设备，能够在低功耗（约 20～30 瓦）的条件下快速生成图像。该设备支持多种数据类型的推理，包括 `FP32`、`BF16`、`FP16` 和 `INT8`，配备了 32 路视频硬件解码和 12 路视频硬件编码能力。`AirBox BM1684X` 具备 `PCIe3 x16` 接口，双千兆以太网口，并支持最大 16GB 的 `DDR4 4266` 内存。硬件设计上，该设备优化了供电接口、散热方式，增加了 `USB` 接口以便扩展，并且提供了两种调试方式：`TTY` 和 千兆网络。在软件层面，`AirBox BM1684X` 预装了 `Ubuntu Server 20.04` 系统，并集成了 `Sophon SDK`，方便开发者部署 `AI` 应用。此外，设备内置了 `Docker`，并且支持高速的软件源，如清华源。

![](img/c04c1fac9088e43b4983cdf6691328d9.png)

## `SGLang`：`LLM`推理引擎发展新方向

贡献人：

https://zhuanlan.zhihu.com/p/711378550?utm_psn=1801272975619657729

`SGLang` 作为新一代 `LLM` 推理引擎，在 `vLLM` 的基础上进行了显著改进，它支持更复杂的 `LLM` 应用场景，如多轮对话和与外部环境的交互。`SGLang` 引入了 `LM Programs` 概念，允许多个 `LLM` 调用，并支持结构化输入和输出，提高了任务质量和整体性能。技术上，`SGLang` 采用了编译器设计，优化了 `KV` 缓存的重用，通过 `RadixAttention` 技术提高了前缀搜索和缓存感知调度的效率。此外，`SGLang` 通过压缩有限状态机实现了高效的约束解码，并通过 `API` 推测执行减少了多调用的延迟和成本。性能测试显示，`SGLang` 在不使用特定优化的前提下，相比 `vLLM` 有明显加速，甚至在某些案例中超越了 `TRT-LLM`。

![](img/9e6ef4730f8cd118144268d0a6c0d195.png)

## Context is Everything: 为什么最大序列长度很重要

贡献人：

https://www.cerebras.net/chip/context-is-everything-why-maximum-sequence-length-matters/

`Cerebras` 的文章详细阐述了序列长度在自然语言理解（`NLU`）中的重要性，并介绍了其 `Wafer-Scale Engine`（`WSE`）如何通过处理高达 50,000 个词素的长序列，显著提升 `NLU` 模型的性能。`WSE` 芯片拥有 850,000 个处理元素，每个元素都有本地内存，通过高速通信网络相互连接，使得密集注意力机制在长序列处理上具备高效率。与传统 `GPU` 相比，`Cerebras` 的技术可以在不牺牲准确性的前提下，显著增加序列长度，这对于提高模型如 `GPT-2` 在长文本理解和生成上的准确性至关重要。文章还指出，之前的解决方案如 `Longformer`、`SMITH` 和 `Linformer` 等，通过稀疏化、层次化或近似注意力机制来减少计算复杂度，但这些方法可能会限制模型学习数据的真实模式。`Cerebras` 的技术不仅在 `NLU` 领域有应用，还在药物发现和基因组学等领域展现出巨大的潜力，因为它能够处理长链接的生物数据，为科学研究提供新的视角。通过 `weight streaming` 特性，`Cerebras` 的系统能够有效地处理长序列，为研究人员提供了一种新的工具，以探索和解决复杂的科学问题。

![](img/2807941e3a7244ec5354909300c43cdd.png)

## 大模型训练：`Megatron-Kwai`中的内存优化

贡献人：

https://zhuanlan.zhihu.com/p/710296768

`Megatron-Kwai` 是一个针对大规模模型训练优化的框架，它在 `Megatron` 的基础上进行了改进。文章首先分析了大模型训练过程中的内存瓶颈问题，然后详细介绍了 `Megatron-Kwai` 如何通过优化内存使用来解决这一问题。具体优化措施包括：减少模型参数的副本数量、使用更高效的数据类型、采用内存池技术以及优化内存分配策略。这些优化不仅减少了内存消耗，还提高了训练速度和模型的加速比例。

## 使用`ChatTTS`和`LLM`让`deeplearning.ai`课程说上流畅中文

贡献人：

https://mp.weixin.qq.com/s/Ud6yY4amOGmuYeQ7Jz0Eyw

本文详细介绍了如何利用 `ChatTTS` 和 `LLM` 技术将 `deeplearning.ai` 的 `ChatGPT Prompt Engineering for Developers` 课程翻译成流畅的中文普通话。作者首先下载了视频和字幕文件，并通过 `LLama3` 和反思策略对字幕进行翻译。在这一过程中，作者强调了保持字幕格式和内容的准确性，以及如何通过反思机制不断优化翻译质量。接着，文章展示了如何使用 `ChatTTS` 将翻译后的字幕转换为自然流畅的普通话音频，并解决了在不同硬件环境下的音频生成和保存问题。此外，文章还讲解了如何调整音频长度以匹配视频字幕时间，并通过 `FFmpeg` 将字幕、音频和视频最终合并。

![](img/ed84399b5198f090faf59c2ae5f1194c.png)

## An Open Course on LLMs, Led by Practitioners

贡献人：

https://hamel.dev/blog/posts/course/

`Mastering LLMs` 是一门针对大型语言模型（`LLMs`）的免费调查课程，由 25 位以上的行业专家授课，他们在信息检索、机器学习、推荐系统、`MLOps` 和数据科学等领域具有专业知识。课程内容超过 40 小时，包括工作坊和讲座，涵盖评估、检索增强生成（`RAG`）、微调等主题，适合技术个体贡献者（`ICs`），如工程师和数据科学家。课程提供了按主题分类的内容组织、章节摘要、笔记、幻灯片和资源，以帮助学生导航和深入学习。建议学生在学习过程中参考相关资料，并将所学应用于个人项目。

![](img/021cb3fc8348092d42ec425d557e9ceb.png)

# 📡潜空间活动预告

> `潜空间系列活动第三季全面升级！`

>

> 第三季第二期将在8月11日 11:00 am开始，嘉宾是陈贝迪。活动包括嘉宾分享、访谈`Q&A`、`cofounder matching`餐会。活动现场还可以体验到咱们奇绩校友的最新产品，期待大家大力支持！

>

> 陈贝迪-卡内基梅隆大学助理教授，斯坦福大学人工智能博士后学者，前 `Meta AI` 研究科学家；致力于在现代硬件上设计和优化算法和模型，以加速大型机器学习系统。

>

> 童晟邦- 纽约大学博士，伯克利人工智能实验室（`BAIR`）研究员， `OpenAI` 超对齐奖学金获得者；

>

> 闫俊杰-`MiniMax` 创始人兼 `CEO`。是中国第一梯队的大模型创业者，目前 `MiniMax` 估值超 25 亿美元。

>

> 宋佳铭-`Luma AI` 首席科学家，斯坦福大学计算机科学博士、博士后；曾任职英伟达（`DIR`）小组科学家 ，创建了最早的扩散模型加速算法，该算法广泛用于最近的生成式 `AI` 系统，包括 `DALL-E 2`、`Imagen`、`Stable Diffusion` 和 `ERNIE-ViLG 2.0`。

>

> 本季活动同时开放北京线下、上海线下和线上场次，欢迎校友和外部创业者扫描右方二维码报名！

![](img/dc7bc09671f25459daf2e9eeec01a5e7.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

`ChatGPT`；`Angela`；`Zijia Liu`；`Alex`；`Zeqi Tan`；`Tiezhen`（`HuggingFace`）；`Naixu`；`Lizhi`；`Peter`；`Suoman`；`Jack`

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> `【日报知识库】`这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/e8bb29d5e99ff82e298db16c825c2c70.png)
