
# PaddleOCR Doc Parser 命令参数

PaddleOCR Doc Parser 命令参数如下表所示：

## 基础参数

| 参数名 | 说明 |
| --- | --- |
| `-i INPUT, --input INPUT` | 输入路径或 URL（必需） |
| `--save_path SAVE_PATH` | 输出目录路径 |

## 布局检测参数

| 参数名 | 说明 |
| --- | --- |
| `--layout_detection_model_name` | 布局检测模型名称 |
| `--layout_detection_model_dir` | 布局检测模型目录路径 |
| `--layout_threshold` | 布局检测模型的分数阈值 |
| `--layout_nms` | 是否在布局检测中使用 NMS（非极大值抑制） |
| `--layout_unclip_ratio` | 布局检测的扩展系数 |
| `--layout_merge_bboxes_mode` | 重叠框过滤方法 |

## VL识别模型参数

| 参数名 | 说明 |
| --- | --- |
| `--vl_rec_model_name` | VL 识别模型名称 |
| `--vl_rec_model_dir` | VL 识别模型目录路径（指定本地 `PaddleOCR-VL-0.9B` 模型路径） |
| `--vl_rec_backend` | VL 识别模块使用的后端（`native`, `vllm-server`, `sglang-server`） |
| `--vl_rec_server_url` | VL 识别模块使用的服务器 URL |
| `--vl_rec_max_concurrency` | VLM 请求的最大并发数 |

## 文档处理模型

| 参数名 | 说明 |
| --- | --- |
| `--doc_orientation_classify_model_name` | 文档图像方向分类模型名称 |
| `--doc_orientation_classify_model_dir` | 文档图像方向分类模型目录路径 |
| `--doc_unwarping_model_name` | 文本图像矫正模型名称 |
| `--doc_unwarping_model_dir` | 图像矫正模型目录路径 |

## 功能开关参数

| 参数名 | 说明 |
| --- | --- |
| `--use_doc_orientation_classify` | 是否使用文档图像方向分类 |
| `--use_doc_unwarping` | 是否使用文本图像矫正 |
| `--use_layout_detection` | 是否使用布局检测 |
| `--use_chart_recognition` | 是否使用图表识别 |
| `--format_block_content` | 是否将块内容格式化为 Markdown |
| `--use_queues` | 是否使用队列进行异步处理 |

## VLM生成参数

| 参数名 | 说明 |
| --- | --- |
| `--prompt_label` | VLM 的提示标签 |
| `--repetition_penalty` | VLM 采样中使用的重复惩罚系数 |
| `--temperature` | VLM 采样中使用的温度参数 |
| `--top_p` | VLM 采样中使用的 top-p 参数 |
| `--min_pixels` | VLM 图像预处理的最小像素数 |
| `--max_pixels` | VLM 图像预处理的最大像素数 |

## 硬件与性能参数

| 参数名 | 说明 |
| --- | --- |
| `--device` | 用于推理的设备，例如 `cpu`、`gpu`、`npu`、`gpu:0`、`gpu:0,1`。如果指定多个设备，将并行执行推理。注意：并非所有情况都支持并行推理。默认情况下，如果可用将使用 GPU 0，否则使用 CPU |
| `--enable_hpi` | 启用高性能推理 |
| `--use_tensorrt` | 是否使用 Paddle Inference TensorRT 子图引擎。如果模型不支持 TensorRT 加速，即使设置此标志也不会使用加速 |
| `--precision` | 使用 Paddle Inference TensorRT 子图引擎时的 TensorRT 精度（`fp32`, `fp16`） |
| `--enable_mkldnn` | 为推理启用 MKL-DNN 加速。如果 MKL-DNN 不可用或模型不支持，即使设置此标志也不会使用加速 |
| `--mkldnn_cache_capacity` | MKL-DNN 缓存容量 |
| `--cpu_threads` | 在 CPU 上用于推理的线程数 |
| `--paddlex_config` | PaddleX 管道配置文件路径 |

当然，上述代码也是`LangChain1.0` + OCR 多模态PDF解析实战项目中`PaddleOCR` 的核心代码逻辑：

# 五、`DeepSeek-OCR` `vLLM` 服务接口启动

`DeepSeek-OCR` 作为刚刚开源的小型多模态视觉模型，经过我们团队大量的测试，其不仅识别精度高，同时对中文的支持也非常出色，同时也能够输出结构化的 `Markdown` 格式文档。

## 5.1 环境配置要求

在开始部署之前，我们首先需要确认自己的服务器是否满足运行条件。

| 硬件类型 | 最低要求 | 推荐配置 | 说明 |
| --- | --- | --- | --- |
| GPU | `NVIDIA RTX 3090` | `RTX 4090` 或更高 | 必须是NVIDIA显卡，支持CUDA |
| 显存 | 24GB | 24GB+ | 显存越大，处理速度越快 |
| 内存 | 32GB | 64GB+ | 用于图像预处理和数据缓存 |
| 硬盘 | 50GB可用空间 | 100GB+ | 模型文件较大，需要足够空间 |

除了硬件，我们还需要准备相应的软件环境：主要包括操作系统：建议使用 `Ubuntu 22.04`，同时需要 `CUDA 12.0+`，而 `Python` 环境建议使用 `Python 3.10+`。

课程使用的配置环境为：`Ubuntu 22.04` + 四卡 `RTX 3090`。大家在使用 `vLLM` 启动`DeepSeek-OCR`时，首先需要做如下配置校验：

```
# 检查 GPU 是否正常工作
nvidia-smi
```

![img/082f7732e67288c32fd462e2b2320ec5.png]

执行上述命令后，如果看到类似下面的输出，说明GPU正常工作：

接下来检查 Conda 是否安装，执行如下命令：

```
conda --version
```

![img/a76c62fbbbc0af650645584907e33685.png]

如果看到类似 `conda` 的版本号，说明`conda`已经安装。如果提示命令不存在，需要先安装 `miniconda` 或者 `anaconda3`。

## 5.2 DeepSeek-OCR模型下载

环境准备好之后，接下来我们需要下载 DeepSeek-OCR 模型文件，并进行相应的配置。这一步是整个部署过程的核心。国内用户强烈建议大家使用 `ModelScope` 下载模型，没有网络限制，同时速度也会快很多。

首先，我们需要创建一个独立的 Python 虚拟环境。虚拟环境可以隔离项目依赖，避免与系统其他 Python 项目产生冲突。执行如下命令

```
conda create --name deepseek-ocr-vllm python==3.10
```

![img/090d42133f20b1eaefe3c32aac1f3407.png]

其中：

+ `--name deepseek-ocr-vllm`：虚拟环境名称，可以自定义

+ `python==3.10`：指定 Python 版本为 3.10（`PaddleOCR` 推荐版本）

执行效果如下图所示：

接下来激活虚拟环境：

```
conda activate deepseek-ocr-vllm
```

![img/f390eb541e489e4a7d2188f2f6f9b947.png]

![img/000c407ac6913a723f0f04ade78ea262.png]

激活后，命令行提示符前会显示 `(deepseek-ocr-vllm)`，表示已进入虚拟环境。

接下来，在`ModelScope` 平台下载 `DeepSeek-OCR` 模型文件。地址：`https://modelscope.cn/models/deepseek-ai/DeepSeek-OCR`

`ModelScope` 是阿里云的模型托管平台，国内访问速度快。首先执行如下命令：

```
pip install modelscope
```

![img/8904a044e7c44a1d37dbc06d8ce62678.png]

然后执行如下代码：

```
from modelscope import snapshot_download

model_dir = snapshot_download('deepseek-ai/DeepSeek-OCR', local_dir='.')
```

其中：

+ `'deepseek-ai/DeepSeek-OCR'`：模型仓库 ID

+ `local_dir='.'`：下载到当前目录（也可以指定其他路径，如 `'./models'`）

接下来执行如下代码进行模型权重安装：

```
python download_deepseek_ocr.py
```

![img/f864240ef94af2f51fd0609f64e5cd92.png]

![img/d456d1aa4d4d85873fbfc4bc5fc52212.png]

![img/d5d51f495236ed0bee4edaf94d2d3359.png]

![img/43291baaf20d33878cd9e85bd4822e88.png]

![img/162603ec0b6d541715dbd3d8d06acdf4.png]

![img/2e0c444567d93a8a22530b72b541928b.png]

![img/430144f88986e05dd4bfeb8965ef48f3.png]

![img/aac2b5efdc59d1fda85a60db6ba81973.png]

![img/8b1579d13b95a51304a5cd7508ca3939.png]

![img/ab33658f89cdaa35252db23d99c259d7.png]

下载完成后的模型目录结构如下所示：

## 5.3 Deepseek-OCR vLLM 项目文件

`DeepSeek-OCR`通过 `vLLM` 平台启动，我们是借助`DeepSeek-OCR`官方提供的项目代码，并做了一些优化和调整，其官方源码下载地址：`https://github.com/deepseek-ai/DeepSeek-OCR/tree/main/DeepSeek-OCR-master/DeepSeek-OCR-vllm`

我们将项目文件下载到本地并上传到服务器上：

其中`deepseek_ocr.py` 和 `flash_attn-2.7.3+cu12torch2.6cxx11abiFALSE-cp310-cp310-linux_x86_64.whl` 使我们给大家提供的两个核心文件，一个用于封装 DeepSeek OCR API 接口，另一个则是离线安装的 `flash_attn` 的离线安装包。

接下来一键安装 `vLLM` 运行 `DeepSeek-OCR`的依赖包：

安装结束后，使用我们给大家提供的`deepseek_ocr.py`文件，启动 `DeepSeek OCR` API 接口 服务。

启动命令如下：

```
python ocr_client.py --model-path /home/data/nongwa/workspace/model/OCR/DeepSeek-OCR --gpu-id 3 --port 8797 --host 192.168.110.131
```

![img/b85ada97553b13e191a55c69a06be5f2.png]

![img/c1cbd0f1434970fb79b49b088c96c216.png]

![img/1ab4e131f31ab4c03ff8cad0ba4744b4.png]

启动成功后，会显示如下：

接下来，便可以在网页端查看到 `FastAPI` 接口：

## 5.4 DeepSeek-OCR API 连接测试

前面我们完成了服务的部署和启动，接下来我们要学习如何在实际项目中调用这个 OCR 服务。本节将通过完整的代码示例，带领大家掌握图片识别和 PDF 文档处理的全流程。

在开始编写调用代码之前，我们先来了解一下 OCR 服务提供的 API 接口规范。

OCR API 接口规范

| 项目 | 说明 |
| --- | --- |
| 接口地址 | `http://localhost:8797/ocr` |
| 请求方式 | `POST` |
| 内容类型 | `multipart/form-data` |
| 文件参数 | `file`（支持 jpg, png, pdf 等格式） |
| 可选参数 | `enable_description`（是否生成图片描述，默认 `False`） |

返回的数据格式如下：

```
{
    "markdown": "识别出的文本内容（Markdown格式）",
    "page_count": 1,
    "processing_time": 2.34
}
```

返回的 `markdown` 字段就是我们需要的识别结果，它保留了原文档的格式结构，包括标题、列表、表格等。

下面我们通过一个完整的代码示例，来演示如何调用 OCR 服务。

```
#!/usr/bin/env python3

# -*- coding: utf-8 -*-

"""
测试 DeepSeek-OCR API

直接调用 API 并保存返回的 markdown 和图像数据
"""

import requests

import json

import sys

from pathlib import Path

def test_deepseek_ocr(pdf_path: str, output_dir: str = "./test_output"):
    """
    测试 DeepSeek-OCR API

    Args:
        pdf_path: PDF 文件路径
        output_dir: 输出目录
    """

    # API 配置

    api_url = "http://192.168.130.4:8797/ocr"

    # 确保输出目录存在

    output_path = Path(output_dir)

    output_path.mkdir(parents=True, exist_ok=True)

    print(f"测试文件: {pdf_path}")

    print(f"API 地址: {api_url}")

    print(f"输出目录: {output_dir}")

    print("-" * 60)

    # 读取 PDF 文件

    try:

        with open(pdf_path, 'rb') as f:

            files = {'file': (Path(pdf_path).name, f, 'application/pdf')}

            # API 参数

            data = {

                'enable_description': 'false',  # 是否生成图片描述

            }

            print("发送请求到 DeepSeek API...")

            # 发送请求

            response = requests.post(

                api_url,

                files=files,

                data=data,

                timeout=300

            )

            if response.status_code != 200:

                print(f"API 返回错误: {response.status_code}")

                print(f"错误信息: {response.text[:500]}")

                return False

            # 解析响应

            result = response.json()

            print(f"API 响应成功")

            print(f"响应包含的字段: {list(result.keys())}")

            print("-" * 60)

            # 提取数据

            markdown_content = result.get("markdown", "")
```
