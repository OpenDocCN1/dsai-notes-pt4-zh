
本库仅包含 GEMM 内核。它要求左侧矩阵（LHS）缩放因子必须进行 TMA 对齐并转置，同时仅支持 NT 格式（未转置的 LHS 和转置的 RHS）。对于转置或其他 FP8 类型转换操作，请独立实现或融合到前置内核中。虽然本库提供了一些简单的 PyTorch 工具函数，但这些可能会导致性能下降，因此我们的主要关注点是优化 GEMM 内核本身。

## 常规密集型 GEMM（非分组）

要执行基本的非分组 FP8 GEMM，请调用 `deep_gemm.gemm_fp8_fp8_bf16_nt` 函数。更多详情，请参考函数文档。

## 分组 GEMM（连续布局）

与 CUTLASS 中的传统分组 GEMM 不同，DeepGEMM 仅对 M 轴进行分组，而 N 和 K 必须保持固定。该设计专为 MoE 模型中的专家共享相同形状的场景而量身定制。

对于训练的前向传播或推理的预填充，在每个专家可能处理不同数量的 token 的情况下，我们将这些 token 拼接成一个单一的张量，称为“连续”布局。请注意，每个专家段必须与 GEMM 的 M 块大小对齐（`get_m_alignment_for_contiguous_layout()`）。

有关更多信息，请参阅 `m_grouped_gemm_fp8_fp8_bf16_nt_contiguous` 函数文档。

## 分组 GEMM（掩码布局）

在推理解码阶段，当启用 CUDA 图且 CPU 无法得知每个专家接收的 token 数量时，我们支持掩码分组 GEMM。通过提供掩码张量，内核仅计算有效部分。

为此，请使用 `m_grouped_gemm_fp8_fp8_bf16_nt_masked`，并参阅相关文档。一个示例用法是使用 DeepEP 低延迟内核的输出作为输入。

## 工具函数

除了上述内核外，本库还提供了一些工具函数：

+   `deep_gemm.set_num_sms`: 设置使用的最大 SM 数量

+   `deep_gemm.get_num_sms`: 获取当前 SM 最大数量

+   `deep_gemm.get_m_alignment_for_contiguous_layout`: 获取分组连续布局的组级对齐要求

+   `deep_gemm.get_tma_aligned_size`: 获取所需的 TMA 对齐大小

+   `deep_gemm.get_col_major_tma_aligned_tensor`: 获取列主序 TMA 对齐的张量

本库还提供了一些环境变量，可能会对你有所帮助：

+   `DG_CACHE_DIR`: 字符串，存储编译内核的缓存目录，默认为 `$HOME/.deep_gemm`

+   `DG_NVCC_COMPILER`: 字符串，指定的 NVCC 编译器路径；默认为从 `torch.utils.cpp_extension.CUDA_HOME` 查找

+   `DG_DISABLE_FFMA_INTERLEAVE`: 0 或 1，禁用 FFMA 交错优化

+   `DG_PTXAS_VERBOSE`: 0 或 1，显示详细的 PTXAS 编译器输出

+   `DG_PRINT_REG_REUSE`: 0 或 1，打印 FFMA 交错的详细信息

+   `DG_JIT_PRINT_NVCC_COMMAND`: 0 或 1，打印 NVCC 编译命令

+   `DG_JIT_DEBUG`: 0 或 1，打印更多调试信息

有关更多示例和详细信息，请参考测试代码或查看相应的 Python 文档。

## 优化

我们使用 🐳 标记了从 CUTLASS 中排除的技术。

## 持久化 warp 专化

按照 CUTLASS 设计，DeepGEMM 中的内核采用 warp 专化，使数据移动、张量核心 MMA 指令和 CUDA 核心的累加操作能够重叠。以下是一个简化的图示，说明这一过程：

![](img/0d8421e833d08295974fa419628a3e1a.png)

## Hopper TMA 特性

Tensor Memory Accelerator（TMA）是 Hopper 架构引入的一项新硬件特性，旨在加速异步数据传输。具体来说，我们使用 TMA 进行：

+   TMA 加载 LHS、LHS 缩放因子和 RHS 矩阵

+   TMA 存储输出矩阵

+   TMA 多播（仅限 LHS 矩阵）

+   TMA 描述符预取

## 常见的细节优化

+   利用 `stmatrix` PTX 指令

+   针对不同 warpgroups 的寄存器计数控制

+   尽可能重叠操作，例如重叠 TMA 存储和非 TMA RHS 缩放因子加载 🐳

+   统一且优化的块调度器

+   所有非分组和分组内核共用一个调度器

+   光栅化以增强 L2 缓存重用

+   完全 JIT 设计 🐳

## 完全 JIT 设计

DeepGEMM 采用完全的 Just-In-Time (JIT) 设计，在安装时无需编译。所有内核都在运行时使用轻量级 JIT 实现进行编译。这种方法提供了几个优势：

+   GEMM 形状、块大小和管道阶段的数量作为编译时常量处理

+   节省寄存器

+   编译器可能会做更多优化

+   自动选择块大小、warpgroups 数量、最优管道阶段和 TMA 集群大小

+   但没有自动调优，最优的选择是通过确定性方式选择的

+   完全展开 MMA 管道，为编译器提供更多优化机会

+   对于小形状尤其重要

+   详情请参考内核文件中的 `launch_k_iterations`

总体来说，JIT 显著提高了小形状的性能，类似于 Triton 编译器的方法。

## 非对齐的块大小 🐳

对于某些形状，块大小对齐到 2 的幂可能会导致 SM 的利用率不足。例如，在 `M=256`，`N=7168` 时，典型的块大小分配 `BLOCK_M=128`，`BLOCK_N=128` 会导致仅有 `(256 / 128) * (7168 / 128) = 112` 个 SM 被利用。为了解决这个问题，我们支持像 `112` 这样的非对齐块大小，从而使得 `(256 / 128) * (7168 / 112) = 128` 个 SM 能够在这种情况下工作。将这种技术与精细化缩放结合使用需要精心优化，但最终会带来性能提升。

## FFMA SASS 交错 🐳

我们观察到，在 NVCC 12.2 和 12.3 之间，CUTLASS FP8 内核的性能有所改善。通过比较编译后的 SASS，我们发现一系列 `FADD` 指令中的一个位在交错模式下被翻转。通过参考一些开源 CUDA 汇编实现，我们发现该位控制 `yield`，这可能会增强 warp 层级的并行性（仅为猜测，意味着让当前的 warp 退出，允许其他 warp 工作）。

为了利用这一点，我们开发了一个类似的脚本来修改编译二进制中的 `FFMA` 指令。除了简单地修改 `yield` 位外，我们还翻转了 `reuse` 位（如果 warp 被挂起，寄存器不能被重用）。这种调整通过创造更多重叠 `MMA` 指令与累加 `FFMA` 指令的机会，提升了性能（在某些情况下提升超过 10%）。

## 更多大模型技术内容学习

扫码添加助理英英，回复“大模型”，了解更多大模型技术详情哦👇

![](img/645b4fb1c5b80ac59239ec0f1988995d.png)

扫码回复“入群”，即可加入大模型技术社群：海量硬核独家技术干货内容+无门槛技术交流！
