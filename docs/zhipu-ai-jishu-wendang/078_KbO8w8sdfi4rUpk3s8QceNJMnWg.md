
# 模型下载和安装

> 来源：[https://ai.feishu.cn/docx/SfJId3xSmo1rPUxdVZfcVnVYnps](https://ai.feishu.cn/docx/SfJId3xSmo1rPUxdVZfcVnVYnps)

## 免安装在线尝鲜

> 无需下载和注册，可以在Huggingface Space在线体验CogVideoX模型：

> 链接：CogVideoX Huggingface Space

## 源码安装

下载模型

+   [下载](https://github.com/git-lfs/git-lfs?utm_source=gitlfs_site&utm_medium=installation_link&utm_campaign=gitlfs#installing)模型文件前请先确保`git lfs`命令已安装，安装教程请参考这里。

+   模型文件已上传至 Huggingface, Modelsope, WiseModel 三个平台，用户可以快速安装模型。

+   从 Huggingface 下载模型

```
git clone https://huggingface.co/THUDM/CogVideoX-5b
```

+   从 Modelscope 下载模型

```
git clone https://modelscope.cn/models/ZhipuAI/CogVideoX-5b
```

+   下载示例代码

```
git clone https://github.com/THUDM/CogVideo
```

## 最低配置要求

单 GPU 推理的 VRAM 消耗：

`BF16`：26GB（使用 `SAT`）/20.7GB*（使用 `Diffusers`）

`INT8`：11.4GB*（使用 `Diffusers` 和 `torchao`）

多 GPU 推理的 VRAM 消耗：

`BF16`：15GB*（使用 `Diffusers`）

微调 VRAM 消耗（每个 GPU）：

63 GB（`bs=1`，`LORA`）

80 GB（`bs=2`，`LORA`）

75GB（`bs=1`，`SFT`）
