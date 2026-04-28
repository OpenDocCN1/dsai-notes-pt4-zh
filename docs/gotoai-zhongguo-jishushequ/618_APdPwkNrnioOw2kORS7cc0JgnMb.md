# SkyPilot-跨云 / 弹性调度，GPU 资源调度开源项目（动态分配 / 释放）

> 来源：[https://gotoaiworld.feishu.cn/docx/Bkokd4fKLoBrgdxiyBWcNx0Gnnh](https://gotoaiworld.feishu.cn/docx/Bkokd4fKLoBrgdxiyBWcNx0Gnnh)

「具备基本的显存动态分配、自动释放、多模型共享」的核心逻辑，用户只需：

1.  配置模型路径 / 显存限制；

1.  启动服务 / 加载模型；

1.  关闭模型 / 服务时，项目自动释放显存（无需手动调用empty_cache()等）。

#### SkyPilot（UC Berkeley 开源，跨云 / 单机 GPU 调度）

+   **项目地址**：[https://github.com/s](https://github.com/skypilot-org/skypilot)kypilot-org/skypilot

+   **核心能力**：

+   单机 / 集群 GPU 资源统一调度，支持显存限制、自动释放；

+   一键部署多模型推理服务，自动处理显存不足问题；

+   支持模型自动量化、显存优化；

+   **简化点**：通过 YAML 配置多模型，一键启动：

+   yaml

```
# model.yamlresources:accelerators: V100:1memory: 8Gi  # 限制GPU显存run: |
  python -m fastchat.serve.model_worker --model-path vicuna-7b-v1.5
```

+   启动命令：sky launch model.yaml