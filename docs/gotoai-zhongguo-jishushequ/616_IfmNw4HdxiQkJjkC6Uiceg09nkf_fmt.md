
# GPU Scheduler-GPU 资源调度开源项目（动态分配 / 释放）

> 来源：[https://gotoaiworld.feishu.cn/docx/IXZ5dHJ76ohV88x99NAc2iBvnKc](https://gotoaiworld.feishu.cn/docx/IXZ5dHJ76ohV88x99NAc2iBvnKc)

「具备基本的显存动态分配、自动释放、多模型共享」的核心逻辑，用户只需：

1. 配置模型路径 / 显存限制；
1. 启动服务 / 加载模型；
1. 关闭模型 / 服务时，项目自动释放显存（无需手动调用`empty_cache()`等）。

## gpu-scheduler（K8s GPU 显存调度插件）

**项目地址**：[https://github.com/4paradigm/gpu-scheduler](https://github.com/4paradigm/gpu-scheduler)

**核心能力**：

+ 为 K8s 集群提供 GPU 显存精细化调度，支持按显存大小分配 GPU；
+ 模型 Pod 退出时自动释放显存，支持显存抢占；
+ 兼容 NVIDIA GPU，开箱即用；

**简化点**：部署后 K8s 自动管理 GPU 显存，无需手动干预。
