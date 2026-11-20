
# TMA 内存栅栏与相关技术

内存栅栏（Memory Fence）在TMA Store中用于同步内存写操作。在TMA Store中，`tma_store_fence()`确保在CTA中的所有线程在SMEM中的写入完成后才能执行TMA store。为了保证多线程一致性，所有线程必须通过同步屏障（如`cluster_sync()`）等待数据传输完成。

TMA Store Reduce允许对GMEM中的数据进行归约操作，如加法、最大值或最小值的计算，通过`cp.reduce.async.bulk`指令直接在GMEM和SMEM之间进行。这一过程显著提高了Split-K GEMM和注意力机制等应用中的计算效率。

TMA Load Multicast是一项用于提高L2缓存命中率的扩展功能。它允许将GMEM中的数据块同时复制到多个CTA的SMEM中。通过设置特定的`multicast`掩码，TMA Load Multicast可以跨多个CTA共享加载操作，减少多次加载的冗余操作。

![](img/cfe2fa31a0bc41ad2b54f3718cc05b4b.png)

## 多模态大模型技术点总结

`https://zhuanlan.zhihu.com/p/717586003`

1. `BLIP-2` (2023.06):

   - `Q-Former`: 作为核心技术，Q-Former通过self-attention层实现与视觉特征的交互，使用Learnable query embeddings提取视觉信息。Q-Former使用两层Transformer子模块，其中cross-attention层每隔一层与冻结的`ViT-L/14`视觉编码器进行交互，确保视觉特征与语言表示的对齐。Q-Former的关键在于query embeddings能够在训练过程中学习到如何从视觉编码器中提取对语言有意义的信息。其预训练分为两个阶段，先学习视觉-语言相关性表示（ITM、ITG、ITC任务），再通过语言生成任务实现视觉到语言的转化。Q-Former输出的query embedding通过线性层投影至与LLM兼容的维度后，与LLM进行拼接，完成视觉提示的输入。

1. `InstructBLIP` (2023.06):

   - Instruction-aware Vision Model: InstructBLIP在`Q-Former`基础上进行了拓展，使其能够处理指令感知的视觉特征。通过将视觉特征作为软提示（soft prompt）传递给LLM，模型能够根据不同的指令生成相应的视觉表示。模型通过三阶段训练：第一阶段冻结视觉编码器，训练`Q-Former`和Projection Layer以捕获图像描述；第二阶段冻结LLM，训练Projection Layer以对齐视觉信息和文本生成；第三阶段进行指令微调，进一步训练`Q-Former`以增强对指令的响应能力。

1. `Qwen-VL` (2023.10):

   - `Cross-Attention`: Qwen-VL采用单层`Cross-Attention`模块，将图像特征压缩至固定长度256维，确保在处理多模态信息时能够保持细粒度的视觉理解。模型在`Cross-Attention`中引入了2D绝对位置编码，以提升空间信息处理能力。Qwen-VL的训练分为三个阶段，首先冻结LLM，训练视觉编码器和`Cross-Attention`模块；接着进行多任务预训练，实现全参数更新；最后通过指令微调，提升模型的对话能力。

1. `Qwen2-VL` (2024.09):

   - `2D-RoPE`: 引入了`2D-RoPE`编码技术，显著增强了模型在空间位置编码上的表现。视觉特征通过`ViT/14`转换后被压缩至66个token，并与LLM结合处理。模型支持更长的上下文序列（128k tokens）和视频处理能力，能够执行实时视频对话。这一架构使得模型在视频理解和长序列推理方面表现出色，同时实现了更高效的跨模态连接。

1. `LLaVA` (2023.12):

   - 线性投影层: LLaVA采用简单的线性层对视觉特征进行投影，使其与`LLaMA`语言模型兼容。训练阶段分为两个步骤：首先预训练线性投影层以对齐视觉和文本特征；随后进行端到端的微调，训练LLM和投影层以优化视觉-语言生成任务。

1. `MiniGPT-4` (2023.10):

   - `Q-Former`: 在`MiniGPT-4`中，`Q-Former`用于处理来自`ViT-G/14`的视觉特征，通过线性层将这些视觉特征与`Vicuna`语言模型对齐。`Q-Former`利用跨注意力机制实现视觉与文本的紧密集成，特别是在少量高质量指令数据的微调过程中进一步优化了模型对视觉提示的理解。

1. `MiniGPT-v2` (2023.11):

   - 多任务训练: `MiniGPT-v2`采用基于`ViT`的视觉编码器，并通过线性层进行投影。该版本通过混合弱监督数据和高质量细粒度数据进行多任务训练，提升模型在多任务上的表现。

![](img/7f6b8a79284e6149f4a4f248447ff4c2.png)

## LLM之位置编码算法总结

`https://zhuanlan.zhihu.com/p/717358390`

本文笔记围绕`RoPE`（旋转位置编码）展开，涵盖了其实现及优化，同时探讨了LLM中涉及的`RoPE`扩展。

1. `Sinusoidal`位置编码：一种非训练的绝对位置编码，使用正弦和余弦函数，根据不同维度产生不同频率的编码。

1. `RoPE`旋转位置编码：其核心思想是通过绝对位置编码实现相对位置编码。`RoPE`利用二维旋转矩阵，在保持位置关系的基础上，减少计算复杂度，使self-attention得分仅依赖于相对位置。

1. 远程衰减特性：`RoPE`具备远程衰减，即位置距离越远，attention得分越低。本文通过阿贝尔变换对这一特性进行了数学推导和绘图验证。

1. `PI`位置插值：`PI`（位置插值）方法通过拟合得到attention score，保证在训练时和推理时不同长度下的表现一致，但插值精度会随频率增高下降。

1. `NTK-aware RoPE`：通过调整频率基底改善`RoPE`的表现，低频时使用内插，高频时外推。它解决了长序列推理中的频率失真问题。

1. `Dynamic NTK`：动态调整`NTK-aware RoPE`的scale参数，基于序列长度动态缩放，适用于不断增长的序列推理任务。

1. `YaRN`：结合了`RoPE`、`PI`、`NTK-aware`等方法，通过频率分段实现更平衡的外推与内插，优化了各维度的波长处理。

1. `Alibi`：通过在query-key dot product中添加线性偏置，而不是显式位置编码，简化了模型结构，并增强局部注意力。

![](img/97f657ea20adc52d46bca09f7dee49b8.png)

## PyTorch Conference 2024 有哪些值得关注的技术或趋势？

`https://www.zhihu.com/question/667521637/answer/3635362174`

PyTorch自发布以来不断迭代，技术改进重点围绕简洁、可调试、扩展性以及性能优化。`PyTorch 2.0` 引入的`torch.compile()`极大提升了AI模型生成的效率，特别是生成式AI领域，支持动态形状、自动导出等。PyTorch LLM推理框架如`gpt-fast`进一步优化LLM推理无需自定义内核，简化了开发。

大模型如`TorchTitan`支持多种并行模式（如数据、张量、流水线并行），结合`FSDP v2`减低峰值内存，提升训练效率。PyTorch的新API如`torch.pipelining`简化流水线并行，改进`TCPStore`服务器和异步检查点提升训练可靠性。

社区贡献持续丰富，如`QLoRA`等微调方法提升了多模态LLM支持。PyTorch还推出`torch.export`和`AOTInductor`，实现模型导出和优化。`torchao`库专注于低精度优化，支持大规模参数的训练和推理。

此外，`FSDP v2`支持`FP8`通信，大规模模型使用的混合专家模型通过`DTensor`并行优化内存和速度。

# 招聘公告

---

![](img/fcddb6ee787d8ccfde7044f949abddce.png)

奇绩创坛由陆奇博士于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

## 🚀F24实习岗位

##### [行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

##### 行研-GenAI研究岗位-战略分析（偏产品）

##### Web / AI 全栈开发工程师实习生

## 火热开放中🚀

##### 行研-GenAI研究岗位-前沿学术探索

##### 行研-GenAI研究岗位-战略分析（偏咨询）

##### 创新活动产品运营 （大模型系列活动产品运营）

> 点击相关岗位可查看对应岗位需求【JD】；相关岗位投递请添加🛰️💻【`l_ivresse29`】

> ‼️欢迎推荐/自荐，期待在奇绩创坛遇见更好的你‼️

---

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> 【日报知识库】这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/cb85d86654ae81bdc22504f5a27ad2b1.png)
