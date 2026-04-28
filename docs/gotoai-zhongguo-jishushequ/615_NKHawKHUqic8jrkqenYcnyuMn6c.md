# MLServer-企业级多模型推理平台（支持动态调度）

> 来源：[https://gotoaiworld.feishu.cn/docx/F6R8dT2Fios4uQxnIKIc8dignqc](https://gotoaiworld.feishu.cn/docx/F6R8dT2Fios4uQxnIKIc8dignqc)

#### MLServer（Seldon 开源，轻量级企业级）

+   **项目地址**：[https://github.c](https://github.com/SeldonIO/MLServer)om/SeldonIO/MLServer

+   **核心能力**：

+   兼容 OpenAI API，支持多模型动态加载 / 卸载；

+   内置显存监控，模型服务停止时自动释放显存；

+   支持模型量化、显存限制配置；

+   **简化点**：通过配置文件管理多模型，一键启动服务：

+   bash

+   运行

```
# 启动MLServer（加载多模型，自动管理显存）
mlserver start ./model-configs/
```