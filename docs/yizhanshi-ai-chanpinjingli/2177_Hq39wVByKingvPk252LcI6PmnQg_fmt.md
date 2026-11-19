
# 第33期 AI行业周报（2025-01.27-2025-02.02）

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Knu4donfmoGKPaxbV67cUqZ2nQj](https://v11enp9ok1h.feishu.cn/docx/Knu4donfmoGKPaxbV67cUqZ2nQj)

## OpenAI上线`o3-mini`

2月1日凌晨，OpenAI正式上线了`o3-mini`系列模型。`o3-mini`是`o1-mini`模型的继任者，主打快速推理，具有更高的性价比。OpenAI研究科学家Noam Brown表示，`o3-mini`在多项评估中性能优于`o1`，能够彻底改变成本与智能之间的关系。

`o3-mini`系列模型的亮点包括

+ 快速推理能力，平均响应时间为7.7秒，较`o1-mini`的10.16秒快了24%。

+ 支持联网搜索，能够实时获取最新答案并附带相关网页链接。

+ 支持函数调用、结构化输出和开发者消息等高级功能。

+ 提供低、中、高三种推理强度选项，用户可根据需求选择。

此外，`o3-mini`还首次向免费用户开放推理模型的使用权限，用户在ChatGPT消息输入框下方选择“Reason”按钮即可使用。付费用户方面，ChatGPT Plus、Team和Pro用户现已可以使用`o3-mini`，企业版将在一周后开放访问。OpenAI还提升了Plus和Team用户的每日消息限制，从`o1-mini`的50条提升至150条。Pro用户可享受`o3-mini`以及`o3-mini-high`的无限次访问。

在性能测试中，`o3-mini`在高等推理模式下，2024年AIME数学竞赛中达到87.3%的准确率，博士级别的科学问答任务GPQA Diamond得分高达77.2%。在软件工程领域，`o3-mini`在SWE-bench验证测试中，高等推理模式下的准确率达到49.3%，使用内部工具框架时，准确率攀升至61%。

`o3-mini`采用思维链推理训练方法，提升了安全性和防越狱测试的表现。在跨语言处理能力测试中，`o3-mini`支持14种主流语言，较`o1-mini`有明显提升。在成本方面，`o3-mini`显著低于`o1`，缓存输入的成本是标准输入费用的一半。

![](img/a9b51ca07f392c878029e38af66a6eb3.png)

## Qwen开源首个1M长度长文本新模型

1月27日，阿里云Qwen开源了一个长文本新模型，首次将上下文扩展到1M长度。此次发布的新模型包括`Qwen2.5-7B-Instruct-1M`和`Qwen2.5-14B-Instruct-1M`，它们在处理长文本任务中已实现稳定超越`GPT-4o-mini`，并且在处理百万级别长文本输入时可实现近7倍的提速。

在上下文长度高达100万Tokens的Passkey Retrieval任务中，`Qwen2.5-1M`系列模型展现出卓越性能，能够精准地从长度为1M的文档里检索出隐藏信息。在长上下文理解任务中，`Qwen2.5-14B-Instruct-1M`模型在多个数据集上的测评成绩更为突出，具备一定优势。

`Qwen2.5-1M`系列模型在短文本任务上的表现与其128K版本相当，确保了基本能力没有因为增加了长序列处理能力而受到影响。与`GPT-4o-mini`相比，`Qwen2.5-14B-Instruct-1M`和`Qwen2.5-Turbo`在短文本任务上实现了相近的性能，同时上下文长度是`GPT-4o-mini`的八倍。

Qwen新模型背后的关键技术主要包括长上下文训练、长度外推和稀疏注意力机制。通过逐步扩展长度的方法，团队将`Qwen2.5-1M`的上下文长度从4K扩展到256K。为了将其扩展到1M，团队采用了长度外推技术，引入了`Dual Chunk Attention (DCA)`。此外，团队还引入了基于`MInference`的稀疏注意力优化，提升了推理速度。

目前，Qwen新模型相关的推理框架和技术报告等内容均已经发布，感兴趣的用户可以通过以下链接体验和了解：

+ HuggingFace体验地址：

```
https://huggingface.co/spaces/Qwen/Qwen2.5-1M-Demo
```

+ 魔塔社区体验地址：

```
https://www.modelscope.cn/studios/Qwen/Qwen2.5-1M-Demo
```

+ 技术报告：

```
https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen2.5-1M/Qwen2_5_1M_Technical_Report.pdf
```

![](img/ab612125522373130a058150c5f6c816.png)

## 阿里除夕发布`Qwen2.5-Max`，多基准测试反超`DeepSeek V3`

1月29日，阿里通义Qwen在除夕夜发布了`Qwen2.5-Max`模型，在多个基准测试中超越了`DeepSeek V3`。`Qwen2.5-Max`在Arena-Hard、LiveBench、LiveCodeBench、GPQA-Diamond等基准测试中表现优异，整体优于`DeepSeek V3`、`Llama-3.1-405B`以及闭源模型`GPT-4o`，与`Claude-3.5-Sonnet`不相上下。

`Qwen2.5-Max`是一个超大规模的`MoE`模型，经过超过20万亿token的预训练数据及精心设计的`SFT+RLHF`训练方案训练而成。该模型不仅在知识理解和推理能力上表现出色，还在多模态能力方面有显著提升。

目前，`Qwen2.5-Max`已在`Qwen Chat`中上线，同时在Hugging Face上有Demo可体验，并且可通过阿里云服务使用API。感兴趣的用户可以尝试体验`Qwen2.5-Max`的强大功能。

![](img/c5993e7f1daa6cbe1367de7d71b332a3.png)

## DeepSeek开源新模型

本周，DeepSeek又开源了两个新模型。它们是视觉模型`Janus-Pro`和多模态理解模型`JanusFlow-1.3B`。

`Janus-Pro`是去年10月发布的`Janus`的升级版，实现了三大改进：训练策略优化、训练数据扩展、扩展到了更大模型。`Janus-Pro`在多模态理解和文生图指令遵从能力上收获了显著提升，在多个基准上超越了`DALL-E 3`与`Stable Diffusion`，同时文生图的稳定性也得到了加强。此次发布的7B和1B两个版本，均在Hugging Face上开放试用。

`JanusFlow-1.3B`则是一个将图像理解和生成统一在一个模型中的强大框架，引入了一种极简主义架构，将自回归语言模型与校正流相结合。研究表明，校正流可以在大型语言模型框架内直接训练，无需进行复杂的架构修改。该模型在不同的领域中实现了与专门模型相当或更优异的性能，同时在标准基准上明显优于现有的统一方法。

DeepSeek的开源行动不仅提升了市场焦虑，也加剧了全球AI竞争压力。其开源的低成本、高性能模型，为AI社区和投资市场带来了新的挑战和机遇。

![](img/05a9cf4075bb1e619df7b7d4b2e3a7ea.png)

## DeepSeek超越ChatGPT登顶中美AppStore

1月27日，DeepSeek发布的iOS应用超越了ChatGPT的官方应用，直接登顶中美AppStore。这一成就不仅标志着DeepSeek在市场上的巨大影响力，也反映了其超强的技术实力和极高的用户认可度。

DeepSeek的开源不仅推动了AI技术的发展，也加剧了全球AI市场的竞争。随着DeepSeek的崛起，AI行业的格局正在发生转变，开源与闭源的较量愈发激烈。DeepSeek的开源模型正为AI社区提供更多的可能性，也为开发者和企业提供了更高效、更经济的解决方案。

![](img/602cba8548284d8aa5256c6581faa992.png)

## 字节开源首款10分钟级长视频深度估计模型——`Video Depth Anything`

1月27日，字节跳动智能创作AR团队联合豆包大模型团队开源了`Video Depth Anything`（`VDA`），这是首款10分钟级长视频深度估计模型，性能达到SOTA。

`VDA`基于`Depth Anything V2`，融合了高效的时空头、精简的时域一致性损失函数，以及新颖的基于关键帧长视频推理策略，可在不牺牲泛化能力、细节生成能力和计算效率的前提下，完成长视频的深度估计任务。

`VDA`在视频数据集的精度和稳定性指标上均取得SOTA，精度提升超过10个百分点，推理速度远快于此前同类模型，速度是此前最高精度模型的10倍以上。在V100下，较小版本`VDA`模型推理速度可达30FPS。`VDA`的开源，为单目深度估计在视频领域的应用提供了全新解决方案。

`VDA`的设计包括：

+ 使用`Depth Anything V2`模型作为编码器，固定编码器参数，降低训练成本并保留已学习到的特征。

+ 设计轻量级时空头，包含四个时间注意力层，独立进行时间维度信息融合。

+ 提出时序梯度匹配损失，直接使用相邻帧中相同坐标深度来计算损失，约束时序一致性。

+ 采用关键帧对齐和重叠区域插值方法，处理任意视频长度，减少累积的尺度漂移

此外，豆包大模型团队还开源了`Prompt Depth Anything`技术，实现了4K分辨率下的高精绝对深度估计，具备广泛的下游应用空间。

![](img/a1afb37bbf7edc05d1d5ce204f1771de.png)

## 微软推出针对NPU优化的`DeepSeek-R1`

1月30日，微软宣布推出针对NPU优化的`DeepSeek-R1`，首批将面向搭载高通骁龙X芯片的`Copilot+ PC`推出，随后扩展到英特尔酷睿Ultra 200V等平台。首个版本`DeepSeek-R1-Distill-Qwen-1.5B`将在`AI Toolkit`中提供，7B和14B版本也将很快推出，助力开发者充分利用`Copilot+ PC`中的强大NPU配置。

微软表示，`Copilot+ PC`上的NPU开启了一种新的范式，生成式AI不仅可以在被调用时执行，还可以支持半连续运行的服务。这使开发者能够利用AI推理引擎构建主动和持续的体验。通过在`Phi Silica`上的工作，微软实现了高效的推理，在首次生成token的时间和吞吐率方面表现出色，同时对电池续航和PC资源消耗的影响降至最低。

![](img/f4dbe3da4583c23342cc26be60a7b276.png)
