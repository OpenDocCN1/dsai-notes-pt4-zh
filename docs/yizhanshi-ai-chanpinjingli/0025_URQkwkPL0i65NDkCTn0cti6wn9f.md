# 微调模型

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HvGEd5musoFRBSxfaw2cC0qJnke](https://v11enp9ok1h.feishu.cn/docx/HvGEd5musoFRBSxfaw2cC0qJnke)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

本仓库旨在带领开发者上手 GLM-4-9B 开源模型的微调。

配合以下代码库中微调部分代码进行上手练习

https://github.com/THUDM/GLM-4/tree/main/finetune_demo

> 本 Demo 必须使用 Linux 操作系统，不支持 WSL。

> 本 Demo 必须使用 NVIDIA GPU设备。

## 硬件检查

**本文档的数据均在以下硬件环境测试,实际运行环境需求和运行占用的显存略有不同，请**以实际运行环境为准。微调的资源占用均按照 configs 文件夹中的配置文件设置 测试硬件信息:

+   OS: Ubuntu 22.04

+   Memory: 512GB

+   Python: 3.10.12 / 3.12.3 (如果您使用 Python 3.12.3 目前需要使用 git 源码安装 nltk)

+   CUDA Version:  12.3

+   GPU Driver: 535.104.05

+   GPU: NVIDIA A100-SXM4-80GB * 8

### 微调显存占用

| 微调模型 | 微调方案 | 显存占用 | 权重保存点大小 |
| --- | --- | --- | --- |
| GLM-4-9B-Chat | lora (PEFT) | 22G | 17M |
| GLM-4-9B-Chat | p-tuning v2 (PEFT) | 21G | 121M |
| GLM-4-9B-Chat | SFT (Zero3 method) | 80G (Each GPU，需要使用8张GPU) | 20G |
| GLM-4V-9B | lora (PEFT), 包含视觉模块 | 75G | 37M |
| GLM-4V-9B | SFT | 本代码不支持 | 28G |

GLM-4V-9B 微调无法可能正常使用 deepspeed,官方微调脚本仅做最基础的微调方案，更多优化需要开发者自行探索

## 环境配置

首先，你需要在安装好 [快速使用（推理部署）](https://zhipu-ai.feishu.cn/wiki/PCDWwC2Ndiuj39kryJZcsTDbnlg) 中提到的依赖的基础下，安装本 Demo 需要的依赖

```
cd finetune_demo
pip install -r requirements.txt
```

## 准备数据集和配置文件

[提示词 / 数据集模板](https://zhipu-ai.feishu.cn/wiki/L1jpwBEqCiHocmkT3VzcQv5Znrg)方 demo 不再提供 数据集准备的代码和示例，您可以参考   的数据集格式要求，自行构建数据。

配置文件与 [ChatGLM3模型微调](https://zhipu-ai.feishu.cn/wiki/AqvXwAwf5ilHqmkdk56cQlX2niR)中相似，无需修改。

## 开始微调

通过以下代码执行 **单机多卡/多机多卡** 运行，这是使用 deepspeed 作为加速方案的，您需要安装 deepspeed。接着，按照此命令运行：

```
OMP_NUM_THREADS=1 torchrun --standalone --nnodes=1 --nproc_per_node=8  finetune_hf.py  data/AdvertiseGen/  THUDM/glm-4-9b-chat  configs/lora.yaml # For Chat Fine-tune
OMP_NUM_THREADS=1 torchrun --standalone --nnodes=1 --nproc_per_node=8  finetune_vision.py  data/CogVLM-311K/  THUDM/glm-4v-9b  configs/lora.yaml  # For VQA Fine-tune
```

通过以下代码执行 **单机单卡** 运行。

```
python finetune.py  data/AdvertiseGen/  THUDM/glm-4-9b-chat  configs/lora.yaml # For Chat Fine-tune
python finetune.py  data/CogVLM-311K/  THUDM/glm-4v-9b configs/lora.yaml # For VQA Fine-tune
```

## 从保存点进行微调

如果按照上述方式进行训练，每次微调都会从头开始，如果你想从训练一半的模型开始微调，你可以加入第四个参数，这个参数有两种传入方式:

1.  yes, 自动从最后一个保存的 Checkpoint开始训练

1.  XX, 断点号数字 例 600 则从序号600 Checkpoint开始训练

```
python finetune.py  data/AdvertiseGen/  THUDM/glm-4-9b-chat  configs/lora.yaml yes
```

## 推理微调后的模型

您可以在 `finetune_demo/inference.py` 中使用我们的微调后的模型，仅需要一行代码就能简单的进行测试。

```
python inference.py your_finetune_path
```

这样，得到的回答就微调后的回答了。

> 官方仓库不提供合并 Lora 权重的脚本 和 使用 vL[LM 载入 微调](https://docs.vllm.ai/en/stable/models/lora.html)模型的脚本，请自行通过 vLLM官方文档 的方案进行合并。