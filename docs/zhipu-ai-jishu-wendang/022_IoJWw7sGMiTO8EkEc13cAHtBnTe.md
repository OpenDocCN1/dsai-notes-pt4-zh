# 环境配置和检查

> 来源：[https://ai.feishu.cn/docx/IQo5dU9sOog6GfxFKbYcMD7OnKe](https://ai.feishu.cn/docx/IQo5dU9sOog6GfxFKbYcMD7OnKe)

本文档是开发组根据实践得出的硬件需求。

## 操作系统

GLM-4-9B 系列模型理论上可以在任何主流的操作系统中运行。GLM开发组已经为主流操作系统做了一定的适配。但是，我们更推荐开发者在 Linux 环境下运行我们的代码。

> 本开源仓库所有的设备均在 Ubuntu 22.04 操作系统下运行，没有在其他操作系统进行过兼容测试。

> 部分仓库中的代码仅能在 Linux 操作系统下运行，请仔细阅读对应Demo的文档。

## 硬件环境

### CPU 设备

GLM-4-9B-Chat 支持使用 CPU 进行推理。请确保您的设备满足：

+   支持 x86 指令集 -- 移动端 等设备可能出现问题

+   您的设备至少拥有 32GB 运行内存

+   如果您的设备支持 OpenVINO 版本 2024.2.0，推理速度可以有一定提升，否则，推理速度非常慢。

+   MLX,Llama.cpp,ollama框架不受支持，因此，mac 设备推理运行非常慢。

### 非CUDA的GPU / NPU 设备

GLM-4-9B-Chat 已经在众多非 CUDA设备上测试通过，以下产品已经受到支持。

+   AMD -- AMD Instinct™ MI300 Series  等支持ROCm 版本 6.1.2 以上的设备。

+   Intel -- Intel® Gaudi® Al Accelerators  等 支持 OpenVINO 版本 2024.2.0 以上的设备。

+   HUAWEI --  Atlas 900 AI 集群等设备。

请注意，上述非CUDA设备的 模型载入部分代码没有在开源仓库提供，开源仓库仅提供使用 CUDA 和 CPU设备载入的方案。

### CUDA设备

如果您使用 NVIDIA 的 CUDA设备，请确保您的设备满足：

+   支持 BF16精度 -- V100,2080TI等20系以下的设备可能在推理时出现问题。

+   支持 x86 指令集 -- Jetson Nano,GH200等设备可能出现问题。

## 显存 / 内存消耗表

您可以根据下表来查询不同模型和使用量下的显存需求。需要注意：

1.  以下表格仅使用 单请求，顺发推理。

1.  以下表格使用 Ubuntu 22.04 系统，A100 / H100 集群进行测试，实际数据可能有一定误差。

#### GLM-4-9B-Chat

| 精度 | 显存占用 | Prefilling | Decode Speed | Remarks |
| --- | --- | --- | --- | --- |
| BF16 | 19 GB | 0.2s | 27.8 tokens/s | 输入长度为 1000 |
| BF16 | 21 GB | 0.8s | 31.8 tokens/s | 输入长度为 8000 |
| BF16 | 28 GB | 4.3s | 14.4 tokens/s | 输入长度为 32000 |
| BF16 | 58 GB | 38.1s | 3.4  tokens/s | 输入长度为 128000 |

| 精度 | 显存占用 | Prefilling | Decode Speed | Remarks |
| --- | --- | --- | --- | --- |
| INT4 | 8 GB | 0.2s | 23.3 tokens/s | 输入长度为 1000 |
| INT4 | 10 GB | 0.8s | 23.4 tokens/s | 输入长度为 8000 |
| INT4 | 17 GB | 4.3s | 14.6 tokens/s | 输入长度为 32000 |

#### GLM-4-9B-Chat-1M

若需要执行长文本推理，可以使用此版本模型，对硬件要求较高。

| 精度 | 显存占用 | Prefilling | Decode Speed | Remarks |
| --- | --- | --- | --- | --- |
| BF16 | 75 GB | 98.4s | 2.3 tokens/s | 输入长度为 200K |
| BF16 | 8 x 80G （8卡H100） | 14分钟 | 2.0 tokens/s | 输入长度为 1M |

#### GLM-4V-9B

| 精度 | 显存占用 | Prefilling | Decode Speed | Remarks |
| --- | --- | --- | --- | --- |
| BF16 | 28 GB | 0.1s | 33.4 tokens/s | 输入长度为 1000 |
| BF16 | 33 GB | 0.7s | 39.2 tokens/s | 输入长度为 8000 |

| 精度 | 显存占用 | Prefilling | Decode Speed | Remarks |
| --- | --- | --- | --- | --- |
| INT4 | 10 GB | 0.1s | 28.7 tokens/s | 输入长度为 1000 |
| INT4 | 15 GB | 0.8s | 24.2 tokens/s | 输入长度为 8000 |

## 软件环境

### Python环境

请开发者按照仓库中不同 demo 对应 的 requirements.txt来安装对应的依赖:

python 版本推荐3.10 和 3.12

### CUDA设备环境

请使用 535.54.03 版本以上的 Nvidia GPU 驱动。

请使用 12.2 以上的 CUDA版本。

### Pip依赖项

pip依赖项请参照github仓库中的requirements.txt文件