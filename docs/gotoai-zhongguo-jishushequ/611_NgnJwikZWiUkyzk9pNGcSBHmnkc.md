# LLMDeploy-开箱即用的多模型推理脚手架

> 来源：[https://gotoaiworld.feishu.cn/docx/VKiRdNVM6oghBuxUnqkcovFTnRd](https://gotoaiworld.feishu.cn/docx/VKiRdNVM6oghBuxUnqkcovFTnRd)

「具备基本的显存动态分配、自动释放、多模型共享」的核心逻辑，用户只需：

1.  配置模型路径 / 显存限制；

1.  启动服务 / 加载模型；

1.  关闭模型 / 服务时，项目自动释放显存（无需手动调用empty_cache()等）。

+   **项目地址**：[https://github.co](https://github.com/InternLM/LLMDeploy)m/InternLM/LLMDeploy

+   **核心能力**：

+   专为大模型设计，支持 vLLM/TensorRT-LLM 显存优化；

+   多模型动态切换，卸载时自动释放显存；

+   提供 WebUI/API/ 命令行三种调用方式；

+   **简化点**：一行命令部署多 LLM 模型：

+   bash

+   运行

```
# 启动多模型服务（自动管理显存）
llmdeploy serve --model-list vicuna-7b,baichuan-7b --gpu-memory 4G
```