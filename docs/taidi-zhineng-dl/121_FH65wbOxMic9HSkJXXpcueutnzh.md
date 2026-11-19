
# 寒武纪 - 软件栈

> 来源：[https://learning.feishu.cn/docx/Yb4xdpqguovR9Ex28Mpc4jX8n8f](https://learning.feishu.cn/docx/Yb4xdpqguovR9Ex28Mpc4jX8n8f)

魔改 `Pytorch`，用 `mlir` 做了个图编译器。

来自 https://zhuanlan.zhihu.com/p/592959661 Cambricon PyTorch 理论基础

![](img/182eaf136613639c462046943773a7a1.png)

架构图

![](img/2d3f6d97920e5b37e40896cb83bbadc1.png)

训练模式（`Eager` 推理时，左下角不用 `backward`）

`mlu`不支持的算子可以自动分发到`cpu`执行。

![](img/7d471ad01431f007765a7df56365f7a4.png)

`JIT` 推理模式

![](img/960c1fbbbd917745df75c0a0dc4c73c9.png)
