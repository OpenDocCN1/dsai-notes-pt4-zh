# 环境配置和检查

> 来源：[https://v11enp9ok1h.feishu.cn/docx/VGJbd354zojwW2xcmsEc7uJankf](https://v11enp9ok1h.feishu.cn/docx/VGJbd354zojwW2xcmsEc7uJankf)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

本文档是开发组根据实践得出的硬件需求。

## 操作系统

CharacterGLM-6B 理论上可以在任何主流的操作系统中运行。CharacterGLM开发组已经为主流操作系统做了一定的适配。

但是，我们更推荐开发者在 Linux环境下运行我们的代码，以下说明也主要针对Linux系统。

## 硬件环境

最低要求：

为了能够流畅运行 Int4 版本的 CharacterGLM-6B，我们在这里给出了最低的配置要求：

内存：>= 8GB

显存:  >= 5GB（1060 6GB,2060 6GB）

为了能够流畅运行 FP16 版本的，CharacterGLM-6B，我们在这里给出了最低的配置要求：

内存：>= 16GB

显存:  >= 13GB（4080 16GB）

Ma[c开发者](https://developer.apple.com/metal/pytorch)无需关注GPU的限制。对于搭载了 Apple Silicon 或者 AMD GPU 的 Mac，可以使用 MPS 后端来在 GPU 上运行 CharacterGLM-6B。需要参考 Apple 的 官方说明 安装 PyTorch-Nightly（正确的版本号应该是2.x.x.dev2023xxxx，而不是 2.x.x）。

> 如果使用CPU加载，可以忽略显存的要求，但是速度非常慢

## 软件环境

### Python环境

请开发者按照仓库中的requirements.txt来安装对应的依赖，并需要注意:

python 版本推荐3.10 或者 3.11

transformers 库版本推荐为 4.36.2

torch 推荐使用 2.0 及以上的版本，以获得最佳的推理性能