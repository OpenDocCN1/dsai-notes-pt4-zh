
# Triton Inference Server-企业级多模型推理平台（支持动态调度）

> 来源：[https://gotoaiworld.feishu.cn/docx/Sxk1d5Zglo1r5txjAKOc7XUcnhb](https://gotoaiworld.feishu.cn/docx/Sxk1d5Zglo1r5txjAKOc7XUcnhb)

具备基本的显存动态分配、自动释放、多模型共享的核心逻辑，用户只需：

1. 配置模型路径 / 显存限制；
2. 启动服务 / 加载模型；
3. 关闭模型 / 服务时，项目自动释放显存（无需手动调用`empty_cache()`等）。

## Triton Inference Server（NVIDIA 开源，工业级）

- **项目地址**：[https://github.com/triton-inference-server/server](https://github.com/triton-inference-server/server)

- **核心能力**：
  - 全自动显存管理：多模型共享 GPU，闲置模型自动卸载释放显存；
  - 支持所有主流框架（PyTorch/TensorFlow/ONNX/vLLM）；
  - 动态批处理、模型优先级调度，显存利用率最大化；

- **简化点**：只需配置模型仓库，无需写一行显存管理代码。

  运行以下命令启动服务器（自动管理多模型显存）：

  ```bash
  # 启动服务器（自动管理多模型显存）
  tritonserver --model-repository=/your/model/repo --gpu-memory-fraction=0.9
  ```

  模型仓库示例结构（每个模型独立配置，Triton 自动调度）：

  ```plaintext
  /your/model/repo/
  ├── model1/  # 模型1配置
  │   ├── config.pbtxt  # 显存限制、框架类型等
  │   └── 1/  # 模型权重
  └── model2/  # 模型2配置
      ├── config.pbtxt
      └── 1/
  ```
