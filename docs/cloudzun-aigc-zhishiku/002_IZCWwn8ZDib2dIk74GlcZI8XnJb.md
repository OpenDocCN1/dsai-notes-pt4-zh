# 02-DeepSeek V3/R1 关键技术与系统协同机制深度解析

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/O6YCd0JWSoTMCEx3XKFcXELEnkh](https://zsyhjtnsa5.feishu.cn/docx/O6YCd0JWSoTMCEx3XKFcXELEnkh)

# 引言

近年来，大型语言模型（LLM）领域取得了显著进展，其中 DeepSeek AI 发布的 DeepSeek-V3 和 DeepSeek-R1 系列模型引起了广泛关注 （1）。这些模型不仅在多项基准测试中展现出与业界领先模型相媲美的性能，尤其在代码和数学推理方面表现突出 （3），更重要的是，它们以显著降低的训练成本和开源模式，对现有的大模型研发生态系统构成了挑战 （6）。DeepSeek 的成功并非偶然，而是建立在一系列精心设计的关键技术创新及其高效协同的基础之上。本报告旨在深入剖析 DeepSeek V3/R1 所采用的核心技术，包括其独特的混合专家（MoE）架构、多头隐式注意力（MLA）、多令牌预测（MTP）、FP8 低精度计算、高效通信库（DeepEP）、优化的注意力解码内核（Flash MLA）、先进的并行策略（DualPipe）以及高质量数据策略等，并阐述这些技术如何在训练和推理流程中协同工作，共同铸就 DeepSeek 模型的卓越性能与效率。

# 核心架构创新

DeepSeek V3/R1 的基础架构在标准 Transformer 之上进行了多项关键创新，旨在提升模型容量、效率和性能。

## 2.1. DeepSeekMoE：精细化混合专家与无辅助损失负载均衡

混合专家（MoE）架构允许模型拥有巨大的总参数量，但在处理每个输入时仅激活一小部分参数（专家），从而在保持高性能的同时降低计算成本 （9）。DeepSeek 在此基础上提出了 DeepSeekMoE 架构。

+   **精细化与共享专家设计：** 与传统 MoE 不同，DeepSeekMoE 采用了更细粒度的专家设计 （1）。它将前馈网络（FFN）层分为两类专家：共享专家（Shared Experts） 和 路由专家（Routed Experts） （1）。共享专家处理通用模式，对每个输入令牌都会被激活；而路由专家则负责处理更专门化的子问题，通过门控网络（Gating Network）动态选择 （1）。这种设计旨在促进知识的分解和专家的特化 （4）。DeepSeek-V3 具体包含 1 个共享专家和 256 个路由专家 （12）。

+   **门控与路由机制：** 对于输入 ut，门控网络计算其与每个路由专家 i 的质心向量 ei 的点积，并通过 Sigmoid 函数得到亲和度分数 si，t。随后，系统选择亲和度最高的 Top-Kr 个路由专家（在 DeepSeek-V3 中 Kr=8 （12））。

+   **无辅助损失负载均衡（Auxiliary-Loss-F**ree Load Balancing）： 传统的 MoE 模型通常依赖辅助损失（A**uxili**ary Loss）来强制专家负载均衡，但这可能损害模型性能 （4）。DeepSeek-V3 开创性地采用了一种无辅助损失的策略 （3）。该策略为每个路由专家引入一个偏置项 bi。在进行 Top-Kr 选择时，使用的是调整后的分数 si，t′=si，t+bi （4）。然而，在计算最终输出时，专家输出的加权系数仍基于原始的亲和度分数 si，t。在训练的每一步结束时，系统会监控整个批次的专家负载情况。如果专家 i 过载，其偏置 bi 会减小 γ；如果负载不足，则 bi 增加 γ（γ 是一个超参数，称为偏置更新速度）（4）。这种动态调整机制可以在不引入辅助损失的情况下有效维持专家负载均衡，避免了性能损失和训练不稳定的问题 （13）。

+   **补充性序列级平衡损失：** 为防止单个序列内部出现极端不平衡，DeepSeek-V3 还使用了一个小的序列级平衡损失 LBal。

+   节点限制路由（Node-Limited Routing）： 为了在分布式训练中控制通信开销，DeepSeek-V3 采用了节点限制路由策略。每个令牌最多被发送到 M 个节点进行专家计算（例如 M=4），这些节点是根据其上专家与该令牌的总亲和度（取节点上亲和度最高的 Kr/M 个专家计算）来选择的。这有助于在 MoE 训练中实现计算与通信的近乎完全重叠 （6）。

+   无令牌丢弃（No Token-Dropping）： 得益于高效的负载均衡策略，DeepSeek-V3 在训练和推理过程中不会因为专家容量限制而丢弃任何令牌。

这种精细化的 MoE 设计结合创新的无辅助损失负载均衡机制，使得 DeepSeek-V3 能够稳定地训练拥有 671B 总参数但每次推理仅激活 37B 参数的超大规模模型，实现了卓越性能与高效率的统一 （3）。

## 2.2. 多头隐式注意力（Multi-Head Latent Attention， MLA）

标准的多头注意力（MHA）机制在处理长序列时，其存储先前令牌的键（Key）和值（Value）向量（即 KV 缓存）会消耗巨大的内存 （12）。例如，对于 DeepSeek-V3 的 128K 上下文长度、61 层和 7k 嵌入维度，若使用标准 MHA，KV 缓存将高达约 213.5 GB （12）。为了解决这个问题，DeepSeek 采用了 MLA 架构 （3）。

+   **KV 压缩与隐式表示：** MLA 的核心思想是压缩 KV 向量 （17）。对于每个注意力头的输入 ht，MLA 使用一个下投影矩阵 WDKV 将其压缩成一个低维的隐式向量 ctKV。然后，通过上投影矩阵 WUK 和 WUV 从 ctKV 重构出压缩后的键 ktC 和值 vtC （17）。此外，还会生成一个解耦的键 ktR （17）。在推理（生成）过程中，只需要缓存低维的 ctKV 和解耦的 ktR，从而大幅减少内存占用 （17）。对于 DeepSeek-V3，MLA 将 KV 缓存需求从 MHA 的 213.5 GB 显著降低到约 7.6 GB （12）。MLA 比 Grouped-Query Attention （GQA） 需要更少的内存 （12）。

+   查询压缩（Query Compression）： 类似地，查询向量 qt 也通过下投影 WDQ 和上投影 WUQ 进行压缩和重构，得到 qtC，并结合解耦查询 qtR （17）。这有助于减少训练过程中的激活内存 （17）。

+   **V3 中的 MLA 改进：** DeepSeek-V3 对 MLA 进行了进一步优化 （1）：

+   **动态低秩投影（Dynamic L**ow-Rank Projection）： KV 向量的压缩维度不再固定，而是根据序列长度动态调整。短序列压缩较少以保持保真度，长序列（如 32K-128K）则进行更深的压缩以控制内存增长 （1）。

+   **自适应查询压缩（Adaptive** Query Compression）： 查询的压缩维度在不同层之间自适应缩放。浅层使用较高维度以增强表达能力，深层则更积极地压缩以节省激活内存 （1）。

+   **改进的 RoPE 处理（Impr**oved RoPE Handling）： 扩展了 V2 中部分解耦键的概念，通过追踪“解耦共享键”来减少在处理极长序列（128K）时的数值漂移，提升稳定性 （1）。

+   联合 KV 存储（Joint KV Storage）： 将压缩后的键和值合并存储为共享的压缩表示，进一步减少多节点推理时的内存访问流量 （1）。

+   层级自适应缓存（Layer-Wise Adaptive Cache）： 在模型的深层，会修剪（Prune）掉较旧的 KV 缓存条目，有助于在处理 128K 上下文时控制内存使用 （1）。

这些 MLA 的创新和改进使得 DeepSeek-V3 能够在支持 128K 超长上下文的同时，保持高效的推理速度和可控的内存占用。

## 2.3. 多令牌预测（Multi-Token Prediction， MTP）

为了进一步提升训练效率和模型性能，DeepSeek-V3 采用了多令牌预测（MTP）的训练目标 （3）。传统的语言模型训练通常只预测下一个令牌，而 MTP 允许模型在每个位置预测未来多个（例如 D 个）令牌 （4）。

+   **MTP 模块与顺序预测：** DeepSeek 的 MTP 实现包含 D 个顺序的 MTP 模块。每个模块 k 包含共享的嵌入层、输出头、一个 Transformer 块 TRMk 和一个投影矩阵 Mk。与一些并行预测的方法不同，DeepSeek 的 MTP 保持了因果链，通过顺序地结合前一深度 k−1 的表示 hik−1 和目标令牌 ti+k 的嵌入，再通过 TRMk 来预测第 k+1 个未来令牌 ti+k+1 的概率分布 Pi+k+1k （4）。

+   **训练目标：** MTP 的损失 LMTP 是所有 D 个预测深度的交叉熵损失 LkMTP 的加权平均，并被添加到主要的训练损失中。这提供了更密集的训练信号，提高了数据效率 （4）。

+   **推理应用：** 在标准推理中，MTP 模块可以被丢弃。然而，它们也可以被用于推测解码（Speculative Decoding），通过并行预测多个未来令牌并进行验证，从而加速文本生成过程 （6）。DeepSeek-V3 的 Hugging Face 版本包含了 14B 参数的 MTP 模块权重，与 671B 的主模型权重分开提供 （15）。

MTP 技术通过从每个训练样本中提取更多信息，有助于提升模型的学习效率和最终性能 （12）。

# 硬件与效率优化

除了架构创新，DeepSeek V3/R1 的成功还得益于一系列针对硬件和计算效率的深度优化，体现了 HPC Co-Design 的理念 （1）。

## 3.1. FP8 低精度计算与 DeepGEMM

为了在可接受的成本内训练和部署超大规模模型，DeepSeek-V3 广泛采用了 FP8（8 位浮点数）低精度计算技术 （1）。

+   **FP8 混合精度训练框架：** DeepSeek 设计并验证了一个 FP8 混合精度训练框架，这在大规模模型训练中尚属首次 （15）。该框架并非所有计算都使用 FP8，而是采取混合精度策略：

+   计算密集型操作（如通用矩阵乘法 GEMM）使用 FP8 执行，以加速计算并减少内存占用。

+   关键操作（如 Embedding、输出头、MoE 门控、归一化层、注意力计算的部分环节）保留使用更高精度（如 BF16 或 FP32）以保证数值稳定性。

+   模型的主权重、梯度和优化器状态也以更高精度存储。

+   精细化量化（Fine-Grained Quantization）： 为克服 FP8 有限的动态范围并处理异常值，采用了精细化量化策略。激活值按 1x128 的块进行分组和缩放，权重则按 128x128 的块进行分组和缩放。引入了沿 GEMM 内积维度的逐组缩放因子。

+   **提升累积精度：** 为解决 FP8 GEMM 中的下溢（underflow）问题，在 Tensor Core 的 MMA（Matrix Multiply-Accumulate）操作中，每隔 NC=128 个元素将中间累积结果提升到 FP32 寄存器进行累加。

+   **E4M3 格式选择：** DeepSeek-V3 选择 E4M3 FP8 格式（4 位指数，3 位尾数）用于所有张量，以获得更高的精度，并依靠精细化量化来弥补其动态范围的不足。

+   在线量化（Online Quantization）： 每个激活块或权重块的最大绝对值在运行时动态计算，用于确定缩放因子，简化了框架并确保了量化精度。

+   **低精度存储与通信：** 为了进一步减少内存占用和通信带宽，缓存的激活值和优化器状态被压缩成 BF16 或定制的 E5M6 格式。MoE 层上下投影前后的激活及其梯度也量化到 FP8 进行通信 （18）。

+   **DeepGEMM 计算库：** 底层的 FP8 矩阵乘法由 DeepGEMM 库高效执行 （User Query）。这是一个高度优化的 GEMM 库，专门针对 FP8 数据类型。它可能利用**即时编译（JIT）**技术，在运行时生成针对特定硬件（如 Hopper GPU）和矩阵尺寸优化的计算核心，以最大化计算效率 （User Query，）。

FP8 的应用显著降低了模型的显存占用和计算延迟，是实现 DeepSeek-V3 训练和推理效率的关键因素之一 （User Query（1））。

## 3.2. DeepEP：为 MoE 优化的通信库

MoE 模型训练，尤其是跨节点训练时，专家之间的数据交换（All-to-All 通信）是一个主要的性能瓶颈 （6）。DeepEP 是 DeepSeek 开发并开源的高效通信库，专门用于解决这个问题 （18）。

+   **核心功能：** DeepEP 提供高吞吐、低延迟的 All-to-All GPU 通信内核，用于 MoE 中的令牌分发（Dispatch）和结果聚合（Combine）操作 （18）。

+   **硬件利用：**** 它充分利用**现代 GPU 集群的硬**件特性，**如用于节点内通信的 NVLink 和用于节点间通信的 RDMA（Remote Direct Memory Access，通常通过 InfiniBand 实现）（18）。

+   **FP8 支持：** DeepEP 支持使用 FP8 格式进行数据传输（例如，令牌分发），进一步减少了通信带宽需求 （18）。

+   **内核类型：** DeepEP 提供两种类型的内核 （18）：

+   常规内核（Normal Kernels）： 针对高吞吐量场景（如训练、推理预填充）优化，同时使用 NVLink 和 RDMA。

+   低延迟内核（Low-Latency Kernels）： 针对延迟敏感的推理（解码）场景优化，使用纯 RDMA 以最小化延迟。

+   钩子式重叠（Hook-Based Overlapping）： DeepEP 引入了一种基于钩子（Hook）的计算-通信重叠方法 （18）。该方法允许 RDMA 网络传输在后台进行，而不需要占用 GPU 的流式多处理器（SM）资源，从而实现更高效的重叠 （18）。

+   **性能基准：** 在 H800 GPU 和 CX7 InfiniBand （400 Gb/s） 网络环境下测试：

+   **常规内核：** 节点内（NVLink，～160 GB/s 带宽）通信可达 153-158 GB/s；节点间（RDMA，～50 GB/s 带宽）通信在 16-32 个专家并行度下可达 43-47 GB/s （18）。

+   **低延迟内核：** 在典型的推理设置下（128 令牌批次，8 专家），分发延迟低至 163 微秒，同时保持 46 GB/s 的 RDMA 带宽 （18）。

+   **需求：** DeepEP 目前主要支持 Hopper 架构 GPU（如 H100/H800），需要 CUDA 12.3+，PyTorch 2.1+，以及配置好的 NVLink 和 RDMA 网络（推荐 InfiniBand）（18）。它还依赖一个修改版的 NVSHMEM。

+   **高级特性：** 支持通过 InfiniBand 虚拟通道（VL）进行流量隔离，以及（在低延迟内核中）自适应路由以缓解网络拥塞 （18）。DeepEP 甚至使用了一些未定义行为的 PTX 指令（如 ld.global.nc.L1：:no_allocate.L2：:256B）来追求极致性能，尽管这带来了潜在的风险 （18）。

DeepEP 通过对 MoE 通信模式的深度优化，并结合底层硬件特性和低精度技术，有效克服了通信瓶颈，是支撑 DeepSeek 大规模 MoE 模型高效训练和推理的关键组件。它的设计（如钩子式重叠）与 DualPipe 等并行策略紧密耦合，体现了系统协同优化的思想。

## 3.3. Flash MLA：加速注意力解码

在**自回归生****成文本的解码**阶段，注意力计算是主要的性能瓶颈之一。Flash MLA 是 DeepSeek 针对其 MLA 架构优化的解码内核，旨在显著提升推理速度，尤其是在处理对话等交互式场景中常见的可变长度序列时 （23）。

+   **优化目标：** 加速 MLA 在 Hopper GPU 上的解码过程 （User Query（23））。

+   **技术手段（推测）：** 基于其名称和目标，Flash MLA 很可能借鉴了 FlashAttention 的思想 （User Query）：

+   IO 感知算法（IO-aware）： 通过优化 GPU 内存（HBM、SRAM）之间的数据读写模式，减少访存次数 （1）。

+   核函数融合（Kernel Fusion）： 将多个计算步骤（如缩放、掩码、Softmax、值聚合）融合到更少的 GPU 内核中，减少内核启动开销。

+   **针对可变长度优化：** 特别优化以高效处理解码过程中长度不断变化的序列，更好地利用 GPU 并行能力。

+   **性能表现：** 在 H800 SXM5 GPU 上，Flash MLA 在内存受限配置下可达 3000 GB/s 的带宽，在计算受限配置下可达 580 TFLOPS （23）。它支持 BF16 和 FP16 数据类型，使用页式 KV 缓存（Paged KV Cache），块大小为 64 （24）。需要 CUDA 12.3+ 和 PyTorch 2.0+ （24）。

Flash MLA 通过对 MLA 解码过程的深度优化，显著降低了推理延迟，提升了用户体验。

# 优化的训练基础设施

训练像 DeepSeek-V3 这样的巨型模型需要高度优化的基础设施，包括并行策略和数据加载机制。

## 4.1. DualPipe：最大化流水线并行效率

对于跨越多 GPU 甚至多节点的超大模型，流水线并行是常用的训练策略。然而，传统的流水线并行存在“气泡”（Pipeline Bubbles）问题，即流水线启动和排空阶段部分 GPU 处于空闲状态，降低了硬件利用率。MoE 模型中的 All-to-All 通信进一步加剧了这一挑战。

+   **DualPipe 机制：** DeepSeek 提出了名为 DualPipe 的先进流**水线并行调度算法 （26）。其核心思想是双向**流水线调度（Bidirectional Pipeline Scheduling），即同时从流水线的两端（第一个和最后一个设备）注入微批次（Micro-batches）。同时，它将每个流水线块（Chunk）内的计算精心划分为更细的阶段，特别是将 MoE 计算分解为 All-to-All 分发、MLP 计算和 All-to-All 聚合等步骤。

+   **计算-通信重叠：** 通过巧妙地安排这些细粒度阶段的执行顺序（类似于 1F1B，即一个前向紧跟一个后向），DualPipe 使得不同微批次在流水线不同设备上的计算能够与通信（由 DeepEP 处理）高度重叠 （6）。例如，当一个设备在进行 MLP 计算时，另一个设备可能正在通过 DeepEP 进行 All-to-All 通信。

+   **优势：**

+   **减少流水线气泡：** 相比传统的流水线并行方法（如 GPipe 或 1F1B），DualPipe 显著减少了 GPU 的空闲时间。

+   **提升 GPU 利用率和吞吐量：** 通过最大化计算和通信的并行度，极大地提高了整体训练效率 （User Query（26））。

+   与 MoE/DeepEP 协同： DualPipe 的设计特别考虑了 MoE 模型的通信模式，能够有效利用 DeepEP 提供的高效通信原语来隐藏通信延迟。这再次体现了算法（并行策略）与框架/硬件（通信库）的协同设计思想 （1）。

DualPipe 通过复杂的调度机制，有效解决了大规模 MoE 模型训练中的流水线效率瓶颈。

## 4.2. 3FS：加速数据访问

训练 LLM 需要持续不断地向 GPU 输送海量数据。如果数据加载速度跟不上计算速度，GPU 就会处于等待状态，造成资源浪费并延长训练时间。3FS 是 DeepSeek 内部使用的工具/加速器，旨在优化训练数据的加载和访问 （User Query）。

+   **目标：** 提升训练数据吞吐量，确保计算单元能够持续获得“燃料”，缩短训练周期 （User Query）。

+   可能采用的技术（基于用户查询推测）：

+   高效预取（Prefetching）： 在 GPU 需要数据之前，提前将数据从存储系统加载到内存或缓存中。

+   **缓存（Caching）：** 将频繁访问的数据块存储在更快的存储介质（如内存或本地 SSD）中。

+   **优化的数据格式/布局：** 使用更适合并行读取或解压缩的数据格式，或优化数据在磁盘上的物理布局。

+   **并行 I/O：** 使用多个线程或进程并发地从存储系统中读取数据。

+   **注意：** 目前公开的资料中没有关于 3FS 的具体技术细节。

尽管细节未知，但 3FS 的存在表明 DeepSeek 在数据加载这一基础环节也进行了专门的优化，以保障整体训练流程的高效运行。

## 4.3. 基石：高质量数据策略

模型的能力上限很大程度上取决于训练数据的质量和构成。DeepSeek 在数据策略上投入了巨大资源，尤其侧重于代码和数学/推理相关的高质量数据 （4）。

+   **数据构成与规模：** DeepSeek-V3 在 14.8 万亿（Trillion）高质量、多样化的令牌上进行了预训练 （3）。数据集中代码、数学和推理相关内容的比例很高 （4）。

+   **数据处理：** 采用了严格的数据收集、清洗和筛选流程 （User Query）。特别地，对于代码数据，使用了 Fill-in-the-Middle （FIM） 的训练策略，让模型学习根据上下文预测缺失的代码段 （4）。

+   **SFT/RL 数据生成：** 在监督微调（SFT）和强化学习（RL）阶段，DeepSeek 构建了一个复杂的数据生成流水线：

+   **推理数据：** 使用专门优化的 R1 系列模型生成高质量的数学、代码、逻辑推理等数据（包含思维链 CoT），并通过拒绝采样（Rejection Sampling）确保质量 （4）。目标是将 R1 的推理能力“蒸馏”到 V3 中 （4）。

+   **奖励模型：** 对于可以客观评估的任务（如数学、代码），使用基于规则的奖励模型（例如，检查代码是否能编译通过、数学答案是否正确）（4）。对于其他任务，则使用基于人类偏好数据的模型奖励模型 （4）。

+   **通用数据：** 对于非推理类任务（如简单问答、创意写作），使用早期模型（如 DeepSeek-V2.5）生成响应，并由人类进行验证 （4）。

+   **数据重要性：** 高质量、有针对性的数据是 DeepSeek 模型在特定领域（尤其是代码和数学）达到 SOTA 水平的直接驱动力 （User Query）。这不仅仅是数据量的堆砌，更是数据工程上的深度投入和策略选择，包括利用已有模型进行数据增强和知识迁移，构成了模型能力的基础。

# 系统协同运作：DeepSeek 引擎实战

DeepSeek V3/R1 的强大并非单一技术的胜利，而是上述各项创新在一个精心设计的系统中协同运作的结果。

## 5.1. 集成化的训练流程

1.  **数据注入：**** 海量**的、侧重代码和数学的高质量训练数据 （4），通过 3FS 数据访问加速器被高效加载到计算集群 （User Query）。

1.  **并行调度：**** 对于巨大的 V**3/R1 模型，训练任务被 DualPipe 先进的流水线并行调度策略高效地分配到多个 GPU 上。DualPipe 通过减少流水线气泡和实现计算通信重叠，确保 GPU 资源得到最大化利用，显著提升训练吞吐量。

1.  **MoE 计算核心：** 当数据流经模型的 MoE 层时：

+   门控网络结合动态偏置调整 （4），为当前输入激活 Top-K（K=8）个最相关的路由专家 （12）。节点限制路由 确定通信范围。

+   **涉及跨设备专****家的数**据交换（All-to-All），由 DeepEP 通信库进行高度优化的通信 （18）。支持 FP8 低精度传输以减少带宽压力 （18）。DeepEP 的钩子式重叠机制允许计算在通信期间继续进行 （18）。

+   无辅助损失负载均衡策略 （4） 确保任务均匀分配给各专家，避免瓶颈，无需丢弃令牌。

1.  **底层计算加速：** 无论是 MoE 中的专家计算，还是模型其他部分的密集计算**（如注**意力中的矩阵乘法），都深度应用**了 FP8 低精**度**技术。**这些核心计算由 DeepGEMM 这个专门优化的库在底层高效执行 （User Query），充分利用现代 GPU（如 Hopper）的 FP8 计算能力。注意力计算本身则由 MLA 架构处理，其压缩表示减少了中间激活的内存占用 （17）。

1.  **MTP 目标计算：** 模型同时计算标准 Next T**oke**n Prediction 损失和 MTP 损失。

1.  **反向传播与更新：** 梯度通过网络反向传播。DualPipe 高效调度反向计算，继续重叠通信与计算。DeepEP 处理梯度聚合的 All-to-All 通信。优化器使用高精度存储的状态更新模型权重。

1.  **循环迭代：** 这个数据加载、并行调度、MoE 计算、FP8 核心运算、梯度计算与更新的流程不断循环迭代。整个过程高度稳定，没有发生不可恢复的损失尖峰或需要回滚 （3）。

这个集成的流程使得 DeepSeek 能够以相对较低的成本（总计约 2.788M H800 GPU 小时 （3），预训练约 2.664M 小时 （6），成本约 550 万美元 （6））高效、稳定地完成 671B 模型的训练。

## 5.2. 优化的推理流程

1.  **输入处理：** 用户输入首先经过分词（Tokenization）处理。

1.  **模型前向计算：** 处理后的输入 Token 进入模型进行前向传播：

+   在 **MoE 层**，门控网络为输入 Token 选择激活的专家进行计算，受益于 MoE 带来的推理效率优势（**仅激活 37**B 参数）（3）。推理时也需要考虑负载均衡。如果专家分布在不同设备，DeepEP 的低延迟内核可能参与通信 （18）。

+   核心的矩阵乘法运算利用 **FP8** 低精度，并通过 **DeepGEMM** 高效执行，降低推理延迟和资源占用 （User Query，）。

+   注意力计算通过 **MLA** 完成，其显著减少的 KV 缓存需求对于长上下文推理至关重要 （12）。

1.  **解码生成加速：** 在自回归生成文本的解码阶段（逐个预测下一个 Token）：

+   关键的注意力计算由 **Flash MLA** 这个高度优化的解码内核执行 （23）。它针对 Hopper GPU 和解码过程中常见的可变长度序列进行了优化，显著加速了解码速度，这对于提升交互式应用的响应速度至关重要 （23）。

+   （可选）如果启用，**MTP** 模块可用于推测解码，进一步加速生成 （6）。

1.  **输出：** 最终生成的 Token 序列被转换回自然语言文本，呈现给用户。

推理流程同样受益于 MoE 的稀疏激活、FP8 的计算加速、MLA 的内存效率以及 Flash MLA 的解码优化，实现了高性能和高效率的结合。

# DeepSeek-R1：专注于推理能力的特化

DeepSeek-R1 并非一个全新的架构，而是基于 DeepSeek-V3-Base 进行特化训练，专注于提升模型的复杂推理能力 （11）。

## 6.1. 与 DeepSeek-V3 的关系

R1 与 V3 共享相同的底层 MoE 和 MLA 架构 （12）。它们的主要区别在于模型的权重，这些权重是通过不同的训练后期阶段（Post-Training）获得的 （11）。DeepSeek 发布了 R1 的多个版本，包括 R1-Zero（早期探索纯 RL 方法）和 R1（采用更精细的多阶段训练流程）（11）。此外，为了普及应用，DeepSeek 还发布了基于 R1 能力蒸馏得到的多个较小模型（参数量从 1.5B 到 70B 不等）（2）。

## 6.2. 训练方法学：以 RL 为中心

R1 的训练过程是一个精心设计的多阶段流水线，旨在将强大的推理能力注入 V3-Base 模型 （11）：

1.  阶段一（冷启动 Cold Start）： 使用数千个结构化的思维链（Chain-of-Thought， CoT）样本对 V3-Base 模型进行初步微调，作为强化学习的初始策略模型 （27）。

1.  **阶段二（面向推理的 RL - GRPO）：** 进行大规模的强化学习。核心是采用了 Group Relative Policy Optimization （GRPO） 算法 （4）。GRPO 的一个关键特点是它不需要训练一个单独的评论家（Critic）或价值模型，而是通过比较同一问题生成的多个输出（一个组）的得分均值作为基线，从而显著减少了训练所需的资源 （4）。此阶段的奖励函数侧重于基于规则的评估，例如数学答案的准确性、代码的正确性以及输出格式和语言的一致性 （4）。

1.  **阶段三（SFT）：** 在 RL 模型收敛后，使用它生成大量高质量的推理数据（包含 CoT），并通过拒绝采样（以 V3 作为判断器）进行筛选，形成推理 SFT 数据集 （27）。结合部分 V3 的非推理 SFT 数据（也增强了 CoT），对模型进行最终的监督微调 （27）。

这种以 RL 为先（RL-first）的方法被认为可以减少对大规模人工标注数据的依赖，更早地引导出模型的反思（Reflection）和自我修正等复杂行为，并可能降低整体训练成本 （7）。R1 的训练方法本身就是一项重要的创新，展示了如何通过目标明确的 RL（GRPO）和精心策划的数据生成流程，将复杂的推理能力有效地赋予预训练好的基础模型。

## 6.3. 能力与蒸馏

Dee**pS**eek-R1 在需要逐步逻辑思考的任务上表现出色，如复杂的数学解题、代码生成与挑战、科学推理以及多步规划（适用于 Agent 工作流）（4）。它在生成答案前会显式地产生一步步的思维链 （11）。更重要的是，DeepSeek 成功地将 R1 的核心推理能力蒸馏到了参数量小得多的模型中（基于 Llama、Qwen 等开源模型进行微调）（7）。这不仅证明了 R1 所学到的推理模式是可迁移的，也极大地降低了高性能推理模型的应用门槛，使得更广泛的开发者和研究者能够接触和使用这些先进能力 （7）。

# 结论

DeepSeek V3 和 R1 的成功是系统性创新的成果，展示了通过软硬件协同设计（HPC Co-Design）和对模型生命周期各阶段的精细优化，可以在有限资源下实现顶尖性能。

+   **协同创新的力量：** 从 DeepSeekMoE 架构与无辅助损失负载均衡提供的可扩展性和稳定性，到 MLA 和 Flash MLA 对长上下文处理和推理速度的优化，再到 FP8 低精度计算与 DeepGEMM 对计算效率的压榨，以及 DeepEP 和 DualPipe 对 MoE 通信和并行效率的保障，最后由高质量、有针对性的数据策略奠定能力基础——这些关键技术环环相扣，相互促进，缺一不可。

+   **效率与性能的突破：** 这些技术的结合使得 DeepSeek 能够以显著低于竞争对手的成本训练出性能一流的超大规模模型，并在推理时保持高效率。FP8 的成功应用、高效的 MoE 通信与负载均衡、以及先进的并行策略是实现这一目标的核心。同时，训练过程的稳定性也证明了其工程实践的成熟度。

+   **对开源生态的贡献：** DeepSeek 不仅发布了强大的模型，还开源了其技术栈中的关键组件，如 DeepEP 通信库和 Flash MLA 解码内核，并详细阐述了其架构和训练方法（如 R1 的 GRPO 流程）。这为整个 AI 社区，特别是资源相对有限的开源社区和研究机构，提供了宝贵的经验和工具，有望推动未来 LLM 向着更高效、更易于访问的方向发展。

DeepSeek 的实践表明，通过在架构、算法、数据和基础设施层面进行全方位的深度优化和协同设计，构建和部署前沿大型语言模型不再是少数巨头的专利。随着这些技术的进一步发展和普及，我们有理由期待一个更加开放和繁荣的 AI 未来。

# 引用的著作

1.  The DeepSeek Series: A Technical Overv[i](https://martinfowler.com/articles/deepseek-papers.html)ew - Martin Fowler， 访问时间为 四月 22， 2025， https://martinfowler.com/articles/deepseek-papers.html

1.  D[eepSeek-R1 models now available o](https://aws.amazon.com/blogs/aws/deepseek-r1-models-now-available-on-aws/)n AWS， 访问时间为 四月 22， 2025， https://aws.amazon.com/blogs/aws/deepseek-r1-models-now-available-on-aws/

1.  D[eepSeek-V3 Technical Report - OpenReview， 访问时间为 四月 22， 2025， ](https://openreview.net/forum?id=flMS0vfo9P&referrer=%5Bthe+profile+of+Damai+Dai%5D(/profile?id%3D~Damai_Dai1))https://openreview.net/forum?id=flMS0vfo9P&referrer=%5Bthe%20profile%20of%20Damai%20Dai%5D(%2Fprofile%3Fid%3D~Damai_Dai1)

1.  D[e](https://vitalab.github.io/article/2025/02/11/DeepSeekV3.html)epSeek-V3 Technical Report - The VITALab website， 访问时间为 四月 22， 2025， https://vitalab.github.io/article/2025/02/11/DeepSeekV3.html

1.  [2412.19437] DeepSeek-V3 Technical Report - arXiv， 访问时间为 四月 22， 2025， https://arxiv.org/abs/2412.19437

1.  Deepseek V3 is officially released （code， paper， ben[ch](https://www.reddit.com/r/LocalLLaMA/comments/1hmmtt3/deepseek_v3_is_officially_released_code_paper/)mark results） : r/LocalLLaMA - Reddit， 访问时间为 四月 22， 2025， https://www.reddit.com/r/LocalLLaMA/comments/1hmmtt3/deepseek_v3_is_officially_released_code_paper/

1.  De[epSeek's reasoning AI sho](https://www.ibm.com/think/news/deepseek-r1-ai)ws power of small models， efficiently trained | IBM， 访问时间为 四月 22， 2025， https://www.ibm.com/think/news/deepseek-r1-ai

1.  DeepSeek R1 and V3: Chinese AI New Year sta[rted early - Nebius， 访问时间为 四月 22， 2025](https://nebius.com/blog/posts/deepseek-r1-v3-chinese-ai-new-year-started-early)， https://nebius.com/blog/posts/deepseek-r1-v3-chinese-ai-new-year-started-early

1.  Understanding DeepSeek-V3: A Deep Dive into the[ Paper and the Code， 访问时间为 四月 2](https://fanyangmeng.blog/understanding-deepseek-v3/)2， 2025， https://fanyangmeng.blog/understanding-deepseek-v3/

1.  De[epSeek MoE -- An Innovative M](https://oilbeater.com/en/2025/03/29/deepseek-moe/)oE Architecture | Oilbeater's Study Room， 访问时间为 四月 22， 2025， https://oilbeater.com/en/2025/03/29/deepseek-moe/

1.  Th[e ](https://www.bentoml.com/blog/the-complete-guide-to-deepseek-models-from-v3-to-r1-and-beyond)Complete Guide to DeepSeek Models: From V3 to R1 and Beyond - BentoML， 访问时间为 四月 22， 2025， https://www.bentoml.com/blog/the-complete-guide-to-deepseek-models-from-v3-to-r1-and-beyond

1.  The Inner Workings of DeepSeek-V[3](https://mccormickml.com/2025/02/12/the-inner-workings-of-deep-seek-v3/) · Chris McCormick， 访问时间为 四月 22， 2025， https://mccormickml.com/2025/02/12/the-inner-workings-of-deep-seek-v3/

1.  De[ep](https://www.reddit.com/r/LocalLLaMA/comments/1i9dvrk/deepseek_v3_will_make_moe_opensource_models_wayyy/)seek v3 will make MoE opensource models wayyy more common. - Reddit， 访问时间为 四月 22， 2025， https://www.reddit.com/r/LocalLLaMA/comments/1i9dvrk/deepseek_v3_will_make_moe_opensource_models_wayyy/

1.  Paper page - DeepSeek-V3 Technical Re[port - Hugging Face，](https://huggingface.co/papers/2412.19437) 访问时间为 四月 22， 2025， https://huggingface.co/papers/2412.19437

1.  d[eepseek-ai/DeepSeek-V3](https://github.com/deepseek-ai/DeepSeek-V3) - GitHub， 访问时间为 四月 22， 2025， https://github.com/deepseek-ai/DeepSeek-V3

1.  d[eepseek-ai/DeepSeek-V3 - H](https://huggingface.co/deepseek-ai/DeepSeek-V3)ugging Face， 访问时间为 四月 22， 2025， https://huggingface.co/deepseek-ai/DeepSeek-V3

1.  A Review of DeepSeek Models' Key Innovative Techniques - arXiv， 访问时间为 四月 22， 2025， https://arxiv.org/pdf/2503.11486?

1.  de[epseek-ai/DeepEP:](https://github.com/deepseek-ai/DeepEP) DeepEP: an efficient expert-parallel ... - GitHub， 访问时间为 四月 22， 2025， https://github.com/deepseek-ai/DeepEP

1.  Dee[pSeek AI Releases DeepEP: An Open-Source EP Communication Library](https://www.marktechpost.com/2025/02/24/deepseek-ai-releases-deepep-an-open-source-ep-communication-library-for-moe-model-training-and-inference/) for MoE Model Training and Inference - MarkTechPost， 访问时间为 四月 22， 2025， https://www.marktechpost.com/2025/02/24/deepseek-ai-releases-deepep-an-open-source-ep-communication-library-for-moe-model-training-and-inference/

1.  DeepSeek Open Source Week Day Two: The First Open-Source EP Communication Library for MoE Models - AIbase， 访问时间为 四月 22， 2025， https://www.aibase.com/news/15684

1.  Dee[pS](https://www.reddit.com/r/LocalLLaMA/comments/1ixkg22/deepseek_realse_2nd_bomb_deepep_a_communication/)eek Realse 2nd Bomb， DeepEP a communication library tailored for MoE model : r/LocalLLaMA - Reddit， 访问时间为 四月 22， 2025， https://www.reddit.com/r/LocalLLaMA/comments/1ixkg22/deepseek_realse_2nd_bomb_deepep_a_communication/

1.  Second DeepSeek Gift: DeepEP， optimized for NVLink communication | Large langua[ge](https://www.turtlesai.com/en/pages-2382/second-deepseek-gift-deepep-optimized-for-nvlink-c) models course udemy - Generative ai Certification Google， 访问时间为 四月 22， 2025， https://www.turtlesai.com/en/pages-2382/second-deepseek-gift-deepep-optimized-for-nvlink-c

1.  Deep[Seek Launches DeepEP， a Communication library for Mixture of Expert](https://analyticsindiamag.com/ai-news-updates/deepseek-launches-deepep-a-communication-library-for-mixture-of-experts-model-training-and-inference/)s Model Training and Inference | AIM Media House - Analytics India Magazine， 访问时间为 四月 22， 2025， https://analyticsindiamag.com/ai-news-updates/deepseek-launches-deepep-a-communication-library-for-mixture-of-experts-model-training-and-inference/

1.  Fl[as](https://champaignmagazine.com/2025/02/25/flashmla-and-deepep-days-1-2-of-deepseek-open-source-week/)hMLA and DeepEP: Days 1-2 of DeepSeek Open Source Week - Champaign Magazine， 访问时间为 四月 22， 2025， https://champaignmagazine.com/2025/02/25/flashmla-and-deepep-days-1-2-of-deepseek-open-source-week/

1.  DeepEP: A High Performance Communication Library Optimized [for Mixture of Experts ](https://www.youtube.com/watch?v=F7s7SYIdVPY)Models - YouTube， 访问时间为 四月 22， 2025， https://www.youtube.com/watch?v=F7s7SYIdVPY

1.  DeepSeek-V3 Technical Report - arXiv， 访问时间为 四月 22， 2025， https://arxiv.org/html/2412.19437v1

1.  A[ Technical Primer on DeepSeek | Booz Allen， 访](https://www.boozallen.com/content/dam/home/docs/ai/a-technical-primer-on-deepseek.pdf)问时间为 四月 22， 2025， https://www.boozallen.com/content/dam/home/docs/ai/a-technical-primer-on-deepseek.pdf

1.  D[eepSeek R1: All you need t](https://fireworks.ai/blog/deepseek-r1-deepdive)o know - Fireworks AI， 访问时间为 四月 22， 2025， https://fireworks.ai/blog/deepseek-r1-deepdive

1.  DeepSeek-R1 Release，[ 访问时间为 四月 22， 2025， https](https://api-docs.deepseek.com/news/news250120)://api-docs.deepseek.com/news/news250120