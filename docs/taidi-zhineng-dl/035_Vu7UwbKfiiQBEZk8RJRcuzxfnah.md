
# LLVM - 指令选择

> 来源：[https://learning.feishu.cn/docx/EAgmd1CNLoH0bDxcUjWc0w3HnKc](https://learning.feishu.cn/docx/EAgmd1CNLoH0bDxcUjWc0w3HnKc)

## 指令选择概述

instruction selection

将 IR 映射到目标机器的 ISA 上。

codegen 的第 1 阶段。

指令选择，隐式依赖寄存器分配器。一般先用无限寄存器的算法，用一个 virtual register 保存 1 个 variable。

## 模式匹配

2 种模式匹配进行寄存器选择：

1. 树模式匹配。龙书 第 8.9 节。第 8.11 讨论动态规划生成最优化的代码序列。
2. 窥孔优化。

树模式匹配，依赖 Compiler IR 和 目标机 ISA 的高层次的树表示法。

窥孔优化。将 compiler IR 转成一种底层线形 IR，然后进行系统化的改进。然后再映射到目标机 ISA。

最简单的做法，IR 的一条 instruction 翻译成 目标机 ISA 的一个序列，类似模版展开。

可以生成正确的代码，但一般效率较差。所以，需要系统化的指令选择方法。

## 复杂度决定因素

1. IR 层级。高层次的 IR，lowering 起来就比较难。LLVM IR 一般是比较方便的。
2. ISA 本身的特性。特别是 ISA 的统一性和完备性。另外是，指令的速度 & 机器特有的用法。
3. codegen 的质量。代码运行速度、code size。

高性能的代码，就必须知道指令的代价，但，经常难以得到准确的代价信息。

## 后端流程

后端流程图（《Getting》P134）：

![](img/f2e74702e7cd1e99572edf6e715e0757.png)

### Instruction selection

Instruction selection：将 LLVM IR 变换为 target 的 `SelectionDAG`，在用代码生成程序库中基于树的模式匹配指令选择算法（经过调整后能作用在 DAG 上）将 DAG 的所有节点变换为目标机器指令。

### Instruction scheduling

Instruction scheduling：也称为前寄存器分配调度。做指令排序，使更多的指令能并行执行，最后将指令变换为 `MachineInstr`（Machine Instruction，缩写为 MI）三地址表示。

### Register allocation

Register allocation：在它之前有一个性质一直保存：LLVM IR 状态下分配的虚拟寄存器集是无限的。它将无限的虚拟寄存器集转换为有限的 target 的真实的物理寄存器集，寄存器不够 spill 到内存。

### Instruction scheduling（后）

Instruction scheduling：也称为后寄存器分配调度。

### Code emission

Code emission：代码输出将 `MachineInstr` 表示变换为 `MCInst`（Machine Code Instruction）实例，变换后的新表示更适合汇编器和链接器，与 LLVM IR 一样有两种编码：汇编文件和二进制目标文件。

## 相关流程图

Instruction selection 之前的所有环节（《Getting》P150）：

![](img/928b7c101c85d0a857ab97dbbf42439e.png)

Code emission 流程（《Getting》P170）：

![](img/7254f082eb7fb38eff468b79984881fb.png)
