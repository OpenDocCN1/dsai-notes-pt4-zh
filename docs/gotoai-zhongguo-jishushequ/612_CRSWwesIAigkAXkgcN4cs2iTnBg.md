# MultiModelInfer-开箱即用的多模型推理脚手架

> 来源：[https://gotoaiworld.feishu.cn/docx/WCB7dZeUcoqtQ8x9WKucuC7Gnch](https://gotoaiworld.feishu.cn/docx/WCB7dZeUcoqtQ8x9WKucuC7Gnch)

「具备基本的显存动态分配、自动释放、多模型共享」的核心逻辑，用户只需：

1.  配置模型路径 / 显存限制；

1.  启动服务 / 加载模型；

1.  关闭模型 / 服务时，项目自动释放显存（无需手动调用empty_cache()等）。

#### 1. MultiModelInfer（轻量级多模型推理脚手架）

+   **项目地址**：[https://github.com/zyd](https://github.com/zyddnys/MultiModelInfer)dnys/MultiModelInfer

+   **核心能力**：

+   封装了显存优化、自动释放、多模型服务化的核心逻辑；

+   支持 PyTorch/TensorFlow/ONNX 模型，一键启动多模型 API 服务；

+   内置显存监控，自动清理显存泄漏；

+   **简化点**：只需配置模型路径，无需写底层显存管理代码。