# 环境配置和检查

> 来源：[https://v11enp9ok1h.feishu.cn/docx/FuwpdOrmsoPZYbxxN28crfj7nZb](https://v11enp9ok1h.feishu.cn/docx/FuwpdOrmsoPZYbxxN28crfj7nZb)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

本文档是开发组根据实践得出的硬件需求。

## 操作系统

CogVLM理论上可以在任何主流的操作系统中运行。但是，我们更推荐开发者在 Linux环境下运行我们的代码，以下说明也主要针对Linux系统。

关于[Windows](https://github.com/THUDM/ChatGLM3/discussions/250)和[MacOS](https://github.com/THUDM/ChatGLM3/discussions/251)的问题欢迎在讨论区提出。

## 硬件环境

CogAgent 是 基于 CogVLM 之后的下一代模型，两个模型之间所需要的配置并不相同，在这里，我们提供了一份配置文件列表，来写明不同模型之间载入的最低配置要求

|  | 模型 | CogAgent |  | CogVLM |  |
| --- | --- | --- | --- | --- | --- |
|  | 类型 | HuggingFace | SAT | HuggingFace | SAT |
| FP16版模型 | 内存 | 40G | 80G | 37G | 80G |
|  | 显存 | 40G | 40G | 40G | 40G |
| Int4模型 | 内存 | 40G | 80G | 40G | 80G |
|  | 显存 | 15G | 15G | 11G | 11G |
| Int4版模型 | 内存 | 15G | 15G | 11G | 11G |
|  | 显存 | 15G | 15G | 11G | 11G |

上表中的数据为峰值数据，这是因为：

+   使用 SAT 加载需要将模型和模型类构造都载入到内存，这将花费两倍模型的内存，因此，峰值内存是模型运行内存的两倍。

+   使用HuggingFace方案加载后，使用low_cpu_mem_usage=True将能将要将峰值内存减少一半。

+   使用 FP16 模型量化Int4的时候，仍然需要载入FP16模型，因此内存的峰值消耗是 FP16模型的消耗，经过处理之后，在推理阶段是 Int4模型的消耗。

+   上述表中数据仅保证能 **成功载入模型****，为了保证**模型能正常对话，推荐使用最低要求的 1.5倍 配置。

Ma[c开发者](https://developer.apple.com/metal/pytorch)无需关注GPU的限制。对于搭载了 Apple Silicon 或者 AMD GPU 的 Mac，可以使用 MPS 后端来在 GPU 上运行 CogVLM。需要参考 Apple 的 官方说明 安装 PyTorch-Nightly（正确的版本号应该是2.x.x.dev2023xxxx，而不是 2.x.x）。

## 软件环境

### Python环境

请开发者按照仓库中的requirements.txt来安装对应的依赖，并需要注意:

python 版本推荐3.10.12

transformers 库版本推荐为 4.33.1以上版本。

torch 推荐使用 2.0 及以上的版本，以获得最佳的推理性能