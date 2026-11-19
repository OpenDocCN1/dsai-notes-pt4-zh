
# GPU Driver & Runtime

> 来源：`https://learning.feishu.cn/docx/EyLGdgvjEoysxBxx7EFcU9Yrnsd`

## SPIR-V

设计为硬件驱动的输入程序表示

`Standard Portable Intermediate Representation (SPIR)`

1. 稳定的字节码。注重于字节码的高效处理并提供强兼容性。
2. GPU 领域专用。着眼于 GPU 领域的行业标准规范

是为表示 `OpenCL` 设备程序 (`kernel`) 而设计的，它锁定了某一版本的 `LLVM IR`，使用 `LLVM` 内联函数 (`intrinsic`) 和元数据 (`metadata`) 来定义 `OpenCL` 的计算原语以及定义。但 `Khronos Group` 逐渐意识到 `LLVM IR` 实在不适合这种任务，遂转向了设计与开发 `SPIR-V`。

`SPIR-V` 是多个 `Khronos API` 共用的中间语言，包括 `Vulkan`、`OpenGL` 以及 `OpenCL`。

完整的 `GPU` 编译器被分为两部分

1. 通过离线工具链从高层次源代码生成 `SPIR-V`
2. 通过驱动内部编译器将 `SPIR-V` 在线编译成机器码。

`GPU Driver` 内要有 `compiler` 的。

`SPIR-V` 更侧重于驱动内部二次编译的高效，核心是字节码。

其编码有很多简化驱动二次编译的考量，像是用各种提前的显示声明来避免运行时复杂的分析。

`SPIR-V` 并没有在规范中指定内存表示或者文本表示，这些都是实现 `SPIR-V` 标准规范的工具链自行定义的。比如 `SPIRV-Tools` 有其自己的内存表示和文本表示，同样 `MLIR` 中的 `SPIR-V dialect` 也是。

`SPIR-V` 的 `IR` 部分和 `LLVM IR` 相差并不太大。`SPIR-V` 借鉴了很多 `LLVM IR` 的设计——它同样是由控制流、基本块、以及静态单赋值来表示程序。指令的粒度和 `LLVM IR` 也相差不大。

`SPIR-V` 中独特的部分在于对很多 `GPU` 概念的原生支持。这种支持通过很多 `SPIR-V` 独有的机制来实现，比如 `decorations`、`builtins`，以及特殊的指令（像是导数计算、图像取样）。另外为了支持图形图像和高性能计算的两种使用场景，`SPIR-V` 中有许多执行模型和模式。当然，对图形图像也有 `structured control flow` 的特殊需求。

一个 `GPU` 为主的标准规范需要原生支持各种 `GPU` 概念，能够提供不同等级的扩展需求，以及提供稳定和兼容的字节码。这些需求并不符合 `LLVM IR` 的设计理念，所以 `Khronos Group` 推出了 `SPIR-V`。但是设计一套中间表示只是个开始，围绕其开发和维护整套工具链需要持续不断的工程投入。`SPIR-V` 与 `LLVM IR` 完全无关，`SPIR-V` 的编译器栈无法利用现有的 `LLVM` 库。所以 `SPIR-V` 的整个栈是从头开始独立开发的，从汇编、反汇编，一步步到各种语言的编译器和优化。如果我们当时能够有一套帮助开发编译器的基础设施——

## Reference

`https://www.xilinx.com/products/technology/ai-engine.html`

`https://www.xilinx.com/content/dam/xilinx/support/documents/white_papers/EW2020-Deep-Learning-Inference-AICore.pdf`

`https://www.xilinx.com/content/dam/xilinx/support/documents/white_papers/wp506-ai-engine.pdf`

`https://docs.nvidia.com/cuda/cuda-driver-api/api-sync-behavior.html`

![](img/226f0d475fba8e32d60586af75a129e5.png)

`cuda` 的 `runtime` 和 `driver API` 看起来都是不开源的。

`AMD` 的开源。

浅谈 `Cuda driver API`

`https://zhuanlan.zhihu.com/p/111602648`

`https://dl.acm.org/doi/abs/10.1145/3542929.3563467`

2022

`Arax`: a runtime framework for decoupling applications from heterogeneous accelerators

Today, using multiple heterogeneous accelerators efficiently from applications and high-level frameworks, such as `Tensor-Flow` and `Caffe`, poses significant challenges in three respects: (a) sharing accelerators, (b) allocating available resources elastically during application execution, and (c) reducing the required programming effort.

In this paper, we present `Arax`, a runtime system that decouples applications from heterogeneous accelerators within a server. First, `Arax` maps application tasks dynamically to available resources, managing all required task state, memory allocations, and task dependencies. As a result, `Arax` can share accelerators across applications in a server and adjust the resources used by each application as load fluctuates over time. Additionally, `Arax` offers a simple `API` and includes `Autotalk`, a stub generator that automatically generates stub libraries for applications already written for specific accelerator types, such as `NVIDIA GPUs`. Consequently, `Arax` applications are written once without considering physical details, including the number and type of accelerators.

Our results show that applications, such as `Caffe`, `TensorFlow`, and `Rodinia`, can run using `Arax` with minimum effort and low overhead compared to native execution, about 12% (geometric mean). `Arax` supports efficient accelerator sharing, by offering up to 20% improved execution times compared to `NVIDIA MPS`, which supports `NVIDIA GPUs` only. `Arax` can transparently provide elasticity, decreasing total application turn-around time by up to 2X compared to native execution without elasticity support.

`GPU` 通过 `Host Interface` 读取 `CPU` 指令，`GigaThread Engine` 将特定的数据从 `Host Memory` 中拷贝到内部的 `Framebuffer` 中。随后 `GigaThread Engine` 创建并分发多个 `Thread Blocks` 到多个 `SM` 上。多个 `SM` 彼此独立，并独立调度各自的多个 `Thread Wraps` 到 `SM` 内的 `CUDA Cores` 和其他执行单元上执行。

+ `SM`: 对应于上图中的 `SM` 硬件实体，内部有很多的 `CUDA Cores`

+ `Thread Block`: 一个 `Thread Block` 包含多个线程（比如几百个），多个 `Blocks` 之间的执行完全独立，硬件可以任意调度多个 `Block` 间的执行顺序，而 `Block` 内部的多个线程执行规则由程序员决定，程同时程序员可以决定一共有多少个 `Blocks`

+ `Thread Warp`: 32 个线程为一个 `Thread Warp`，`Warp` 的调度有特殊规则，本文后面会继续深入

上图存在 16 个 `SMs`，每个 `SM` 带 32 个 `Cuda Cores`，一共 512 个 `Cuda Cores`。这些数量不是固定的，和具体的架构和型号相关。

每个 `CUDA Core` 含有一个 `Integer arithmetic logic unit (ALU)` 和一个 `Floating point unit(FPU)`。并且提供了对于单精度和双精度浮点数的 `FMA` 指令。

`SM` 内还有 16 个 `LD/ST` 单元，也就是 `Load/Store` 单元，支持 16 个线程一起从 `Cache/DRAM` 存取数据。

4 个 `SFU`，是指 `Special Function Unit`，用于计算 `sin/cos` 这类特殊指令。每个 `SFU` 每个时钟周期只能一个线程执行一条指令。而一个 `Warp`(32 线程)就需要执行 8 个时钟周期。`SFU` 的流水线是从 `Dispatch Unit` 解耦的，所以当 `SFU` 被占用时，`Dispatch Unit` 会去使用其他的执行单元。
