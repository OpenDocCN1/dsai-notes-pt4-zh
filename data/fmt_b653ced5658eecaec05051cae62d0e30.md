
### 图2：2阶段`WGMMA`-`softmax`流水线

⁶注意，重叠方案的阶段数受循环`SMEM`缓冲区中的阶段数`s`的限制，但不必等于`s`。

![](img/47484d31f02251b44c70da9d70305457.png)

算法2作为算法1中消费者路径的替代，构成了FP16精度的完整`FlashAttention-3`算法。在高层次上，我们使用`WGMMA`作为异步`GEMM`的代称。在主循环（第8到16行）中，迭代`j`的第二个`WGMMA`操作（第11行）与迭代`j+1`的`softmax`操作（第13行）重叠。

虽然上述流水线结构理论上提供了性能提升，但仍有几个实际方面需要考虑：

### 编译器重排序

伪代码表示理想化的执行顺序，但编译器（`NVCC`）通常会为优化重新排列指令。这可能会扰乱精心设计的`WGMMA`和非`WGMMA`操作流水线序列，可能导致意外行为或性能提升减少。对`SASS`代码的分析表明，编译器按预期生成重叠代码（见B.2节）。

### 寄存器压力

为保持最佳性能，应最小化寄存器溢出。然而，2阶段流水线需要额外的寄存器来存储中间结果并在阶段之间维持上下文。具体来说，必须在寄存器中保存一个额外的`S_next`，每个线程块额外使用`B_r × B_c × sizeof(float)`大小的寄存器。这种增加的寄存器需求可能与使用更大的块大小（另一种常见优化）相冲突，后者也需要大量寄存器。实践中，应根据性能分析结果进行权衡。

### 3阶段流水线

在上述2阶段算法的基础上，我们提出了一个3阶段变体，进一步将第二个`WGMMA`与`softmax`重叠。虽然这种方法可能提供更高的Tensor Core利用率，但由于流水线中增加了一个阶段，它需要更多的寄存器，使得在块大小和流水线深度之间的权衡更难平衡。3阶段算法的详细描述及其评估结果可在附录B.3中找到。

## 3.3 使用FP8的低精度

### 效率：布局转换

以`FP8`精度计算`FlashAttention-3`的前向传播在布局一致性方面带来了`FP16`所没有的额外挑战。

![](img/c32f2f8805fb77ad53074c97f470b79f.png)

图3：`FP32`累加器寄存器`WGMMA`布局 - 第0和8行，线程0-3，条目0-7。

![](img/006ca933aef8f5e1a438b614ec35fd63.png)

图4：`FP8`操作数`A`寄存器`WGMMA`布局 - 第0和8行，线程0-3，条目0-7。

首先，我们注意到输入张量`Q`、`K`和`V`通常在头维度上是连续的，而为了满足`FP8` `WGMMA`对第二个`GEMM`的`k`-major约束，我们需要`V`（或者更准确地说，`V`的块）在`SMEM`中在序列长度维度上连续。由于`TMA`加载本身无法改变连续维度，我们需要要么（1）在`GMEM`中预处理时转置`V`，要么（2）在将`V`的块加载到`SMEM`后在内核中进行转置。为实现选项（1），我们可以（1a）将转置融合到前一步骤（如旋转嵌入）的尾声中，或（1b）调用独立的预处理转置内核⁷来交换序列长度和头维度的步长。然而，（1a）难以集成到标准库中，而（1b）在内存受限的情况下太浪费。

相反，对于`FP8` `FlashAttention-3`，我们选择选项（2）。对于内核内转置，我们利用`LDSM`（`ldmatrix`）和`STSM`（`stmatrix`）指令，这些指令涉及一个warp的线程集体以128字节的粒度将`SMEM`加载到`RMEM`和将`RMEM`存储到`SMEM`⁸。`LDSM`/`STSM`指令既寄存器效率高，允许我们在生产者`warpgroup`中执行它们，又能够在进行内存复制时转置布局。此外，在第一次迭代后，我们可以安排下一个`V`块的转置在涉及前一个`V`和当前`K`块的两个`WGMMA`的阴影下执行。

其次，我们观察到，与`FP16`不同，`FP8` `WGMMA`的`FP32`累加器的内存布局与其在寄存器中保存时假设的操作数`A`的布局不同。我们在图3和图4中描绘了这两种布局的片段，其中条目按列出的顺序保存在每个线程的寄存器中。通过使用字节置换指令，我们可以将第一个`WGMMA`的累加器转换为适合第二个`WGMMA`的格式，并与内核内转置产生的`V`块布局兼容。具体来说，参考图3，我们将序列顺序改为

![](img/d917956da5133f241661925109134206.png)

这个寄存器置换然后每8字节重复一次。就`P`块的逻辑形状而言，这个操作置换了其列（例如，列`0189`现在成为前四列）。为了让`WGMMA`计算正确的输出块，我们可以相应地安排内核内转置以写出匹配的`V`块行置换⁹。

注：

⁷优化的转置内核将达到接近设备带宽的速度[45]。

⁸在`PTX`文档中，`LDSM`/`STSM`被描述为复制8×8个16位条目的矩阵[39, §9.7.13.4.15-16]，但我们可以在`FP8`精度的上下文中一次打包两个8位条目来使用`LDSM`/`STSM`。然而，`LDSM`/`STSM`的转置版本无法分割打包的8位条目，这需要在`LDSM`和`STSM`之间进行某些寄存器移动以实际执行块级转置；我们省略了细节。

⁹通过执行内核内转置提供的这种额外自由度消除了使用`shuffle`指令来改变线程间寄存器所有权的需要，这是我们之前在[7]中描述过的。

### 准确性：块量化和非连贯处理

使用`FP8`（`e4m3`）格式，只使用3位存储尾数和4位存储指数。这导致比`FP16`/`BF16`更高的数值误差。此外，large models通常有比大多数其他值大得多的`outlier`值[20, 52]，使量化变得困难。人们通常对每个张量使用单个标量进行缩放[37]（例如，`Q`、`K`和`V`各一个）。为了减少`FP8`中注意力的数值误差，我们采用两种技术：

1. 块量化：我们对每个块保留一个标量，这样对于每个`Q`、`K`、`V`，我们将张量分成大小为`B_r × d`或`B_c × d`的块，并单独量化它们。这种量化可以与注意力之前的操作（如旋转嵌入）融合，无需额外的慢速（因为旋转嵌入受内存带宽限制）。由于`FlashAttention-3`算法自然在块上操作，我们可以以零计算成本为每个`S`块考虑这种块量化。

2. 非连贯处理：为了均衡异常值，我们在量化为`FP8`之前将`Q`和`K`乘以一个随机正交矩阵`M`。由于`M`是正交的，`MM^T = I`，因此`(QM)(KM)^T = QK^T`，即用`M`乘以`Q`和`K`不会改变注意力输出。这有助于"展开"异常值，因为`QM`或`KM`中的每个条目是`Q`或`K`中条目的随机和，从而减少量化误差。在实践中，我们遵循Chee等人[9]的方法，选择`M`为±1的随机对角矩阵和一个`Hadamard`矩阵的乘积，这可以在`O(d log d)`而不是`O(d²)`时间内相乘，并且还可以与旋转嵌入无额外计算成本融合。

我们在§4.3中验证这两种技术将数值误差减少了多达2.6倍。

## 4 实证验证

我们使用`CUTLASS`[55]中的原语，如`WGMMA`和`TMA`抽象来实现`FlashAttention-3`并评估其效率和准确性。

• 注意力基准测试。我们测量不同序列长度下`FlashAttention-3`的运行时间，并将其与PyTorch中的标准实现、`FlashAttention-2`、`Triton`中的`FlashAttention-2`（使用`H100`特定指令），以及`cuDNN`针对`H100` GPU优化的`FlashAttention-2`供应商实现进行比较。我们确认`FlashAttention-3`比`FlashAttention-2`快至2.0倍，比`Triton`中的`FlashAttention-2`快1.5倍。`FlashAttention-3`在`H100` GPU上达到了理论最大`TFLOPS/s`的75%，即740 `TFLOPS/s`。

• 消融研究。我们确认我们的算法改进，包括warp专门化和`GEMM`-`softmax`流水线，对`FlashAttention-3`的加速有贡献。

• `FP8`注意力的准确性。我们验证块量化和非连贯处理将`FP8` `FlashAttention-3`的数值误差减少了2.6倍。

### 4.1 注意力基准测试

我们在`H100` 80GB `SXM5` GPU上测量了不同注意力方法在不同设置下（有/无因果掩码，头维度64或128）对`FP16`输入的运行时间。我们在图5和图6中报告结果，显示`FlashAttention-3`在前向传播中比`FlashAttention-2`快约1.5-2.0倍，在反向传播中快1.5-1.75倍。与标准注意力实现相比，`FlashAttention-3`可以快至3-16倍。对于中等和长序列（1k及以上），`FlashAttention-3`甚至超过了为`H100` GPU优化的供应商库（`cuDNN` - 闭源）的速度。

基准测试设置：我们将序列长度变化为512、1k、...、16k，并设置批量大小使总token数为16k。我们将隐藏维度设为2048，头维度为64、128或256（即32头、16头或8头）。为计算前向传播的`FLOP`数，我们使用：

`4 · seqlen² · head_dimension · number_of_heads`

对于因果掩码，我们将此数除以2，因为大约只有一半的条目被计算。要获得反向传播的`FLOP`数，我们将前向传播的`FLOP`数乘以2.5（因为前向传播有2个矩阵乘法，而反向传播有5个矩阵乘法，due to recomputation）。

我们还在类似设置下测量了`FP8`前向传播的运行时间。我们在图7中报告了头维度256的结果，完整结果在附录C.2中给出。

### 4.2 消融研究：2阶段流水线实验

我们对非因果`FP16` `FlashAttention-3`进行2阶段`WGMMA`-`softmax`流水线和warp专门化的消融实验，固定参数`{batch, seqlen, nheads, hdim} = {4, 8448, 16, 128}`。表2中的结果确认了我们的算法改进（warp专门化的异步性和`GEMM`与`softmax`之间的重叠）带来显著加速，从570 `TFLOP/s`提高到661 `TFLOP/s`。

![](img/b6be1cf3ec10a798a820fa99a24160ee.png)

图5: Attention forward speed (`FP16`/`BF16`) on `H100` GPU

### 4.3 数值误差验证

由于人们对`FlashAttention`的数值误差[21]感兴趣，

`FlashAttention-3`以及`FP64`中针对参考实现的注意标准实现。到

模拟LLM中的异常特征和激活[20, 52]，我们生成`Q`、`K`、`V`的条目，如下所示：

![](img/448fc8ea7ec15a6b4a539f935bad7e03.png)

图6: Attention backward speed (`FP16`/`BF16`) on `H100` GPU

![](img/eb6ffbcd828b6803fdd141a94cfa3e67.png)

图7: Attention forward speed (`FP8`) on `H100` GPU

表 2: Pipelining ablation measurements

![](img/dac92ce371ad2fa074e41d97a5665259.png)

分布：

`N (0, 1) + N (0, 100) · Bernoulli(0.001)`

也就是说，每个条目都是正态分布的，均值为零，标准差为1，但对于0.1%的条目，我们添加一个标准偏差为10的正态分布的独立项。然后我们测量根均值平方误差（`RMSE`）见表3。在`FP16`中，`FlashAttention-2`和`FlashAttention 3`都实现了1.7×与标准实现相比，`RMSE`更低，因为中间结果（`softmax`）保存在`FP32`中。这个`FP8`中的基线注意力使用每个张量缩放，`FP32`中使用`matmul`累加器和中间`softmax`结果保存在`FP16`中。由于块量化和非相干处理，`FP8`中的`FlashAttention-3`是2.6×比这个基线更准确。

![](img/3f46e86939d1e5b97a83156fa1c75b69.png)

表3：`FP16`和`FP8`（`e4m3`）中的数值误差比较。

## 5 讨论、局限性和结论

通过`FlashAttention-3`，我们展示了新的编程技术和硬件特性（如异步和低精度）可以对注意力的效率和准确性产生显著影响。与`FlashAttention-2`相比，我们能够将注意力速度提高1.5-2.0倍，并将`FP8`数值误差减少2.6倍（相比标准的每张量量化）。我们希望在未来解决的一些局限性包括：优化LLM推理、将持久内核设计集成到`FP8`内核中¹⁰，以及理解大规模训练中低精度注意力的影响。尽管本工作聚焦于`Hopper` GPU，我们预计这里开发的技术将适用于其他硬件加速器。我们希望像注意力这样更快、更准确的基元将为长上下文任务开启新的应用。

## 致谢

我们感谢NVIDIA `CUTLASS`团队（特别是Haicheng Wu、Aniket Shivam和Cris Cecka）帮助我们理解`Hopper`的编程模型，并为他们的库提供清晰而强大的构建块，用于实现`FlashAttention-3`。我们感谢`cuDNN`团队提出`FP8`的内核转置想法。我们感谢Driss Guessous将`FlashAttention`集成到`PyTorch`中。`FlashAttention-3`受益于与Horace He就不同注意力变体的有见地的讨论，与Hao Liu和Phil Wang就分布式注意力的讨论，以及与Daniel Haziza和Chris De Sa就量化的讨论。我们感谢Meta、Together AI和普林斯顿语言与智能研究所（`PLI`）提供的计算支持。

## 引用

[1] Ahmad Abdelfattah, Azzam Haidar, Stanimire Tomov, and Jack Dongarra. Performance, design, and autotuning of batched gemm for gpus. pages 21–38, 06 2016. ISBN 978-3-319-41320-4. doi: 10.1007/978-3-319-41321-1_2. [2] AI21. Introducing jamba: Ai21’s groundbreaking ssm-transformer model. AI21 blog, 2024. [3] Joshua Ainslie, James Lee-Thorp, Michiel de Jong, Yury Zemlyanskiy, Federico Lebrón, and Sumit Sanghai. Gqa: Training generalized multi-query transformer models from multi-head checkpoints. arXiv preprint arXiv:2305.13245, 2023.

[4] Michael Bauer, Henry Cook, and Brucek Khailany. CudaDMA: Optimizing GPU Memory Bandwidth via Warp Specialization. In Proceedings of 2011 International Conference for High Performance Computing, Networking, Storage and Analysis, SC ’11, New York, NY, USA, 2011. Association for Computing Machinery. ISBN 9781450307710. doi: 10.1145/2063384.2063400. URL https://doi.org/10.1145/2063384.2063400.

[5] Maximilian Beck, Korbinian Pöppel, Markus Spanring, Andreas Auer, Oleksandra Prudnikova, Michael Kopp, Günter Klambauer, Johannes Brandstetter, and Sepp Hochreiter. xlstm: Extended long short-term memory. arXiv preprint arXiv:2405.04517, 2024.

[6] Iz Beltagy, Matthew E Peters, and Arman Cohan. Longformer: The long-document transformer. arXiv preprint arXiv:2004.05150, 2020.

...

# 背后团队盘点
