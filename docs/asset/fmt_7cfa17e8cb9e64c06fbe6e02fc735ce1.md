
| 初级：60分，提见识 | + 无 | 1. 知道怎么解读一个赛事 | 1. [学习如何解读一个赛事](https://datawhaler.feishu.cn/wiki/CMYkwKow0ihxOikg1G9cd9Zhnng#part-KzfQdxy3OogDmUxvdbdc8OH8nPd) |

| 进阶：80分，会应用 | + 熟悉 Python | 1. 通过代码出图 | 1. [学习文生图原理](https://datawhaler.feishu.cn/wiki/CMYkwKow0ihxOikg1G9cd9Zhnng) |

| 高级：90+分，能创新 | + `LoRA` | 1. 通过代码制作高质量的图片 | 1. 跑通`baseline` |

### 个人学习计划（复制到自己的文档中自行填写即可~）

| 序号 | 日期 | 今日目标 | 今日学习资料 & 学习计划 |
| --- | --- | --- | --- |
| Day1 | 8月11日 |  |  |
| Day2 | 8月12日 |  |  |
| Day3 | 8月13日 |  |  |
| Day4 | 8月14日 |  |  |
| Day5 | 8月15日 |  |  |
| Day6 | 8月16日 |  |  |
| Day7 | 8月17日 |  |  |

# 进阶知识提要

## 零代码文生图Lora模型训练

> 刚刚我们体验了`lora`风格模型训练的全流程，现在给大家介绍一款界面化模型训练工具。无需任何代码，即可完成模型微调。

魔搭平台现推出的零代码图像模型训练工具，您只需提供几张图片，即可训练出专属的`lora`风格模型，并直接使用该模型生成新的图片。让我们一键开始使用魔搭的图像模型训练工具（点击跳转工具）

**训练步骤如下：**

![](img/20704ded87f88890c7990d2c6101ec3a.png)

![](img/131cc47a52742b43f9c32c57ef726cbb.png)

![](img/8d35596abf3124e579699b46c1c95d04.png)

> 注意📢：

1. 该模型训练工具上传的图片数量最多128张，相比于代码开发的模式，有一定的限制。故在模型效果上比我们上边代码生成的结果差。

1. 数据集例子使用了的`/mnt/workspace/kolors/data/lora_dataset/train/`文件夹中的部分图片，大家也可以自行选择一下其他的图片。

## 魔搭热门文生图AI应用集锦

现在我们来看看在文生图领域，有哪些好用的工具：

+ 更多好玩有用的AI应用，大家可以去魔搭[AIGC专区](https://www.modelscope.cn/aigc/home)自己探索！

| 工具类型 | 应用名称 | 应用简介 | 体验效果 | 更多内容详见 |
| --- | --- | --- | --- | --- |
| 基础图片生成工具 | [**FLUX文生图模型体验空间**](https://www.modelscope.cn/studios/muse/flux_dev) | 该应用由**阿里muse团队**推出，是目前最强的开源文生图模型之一，在文字生成、复杂指令遵循和人手生成上具备优势。 | ![](img/6cac07cc33df4909ce646a064e719b54.png) | 消费级显卡，17G显存，玩转图像生成模型`FLUX.1`！ |

|  | [**可图文生图**](https://www.modelscope.cn/studios/AI-ModelScope/Kolors) | 可图是快手开源的一种名为`Kolors`（可图）的文本到图像生成模型，该模型具有对英语和汉语的深刻理解，并能够生成高质量、逼真的图像。 | ![](img/9748813f5c26fbb1ce29278ae469c77f.png) | AI绘画社区新成员：快手开源可图文生图模型，支持双语，可写中文！ |

|  | `Stable Diffusion XL 1.0` | 所有文生图应用的祖师爷，不必多说，YYDS。继`SDXL 0.9`的有限、仅供研究发布之后，`SDXL`的完整版本是目前最好的开源图像生成模型。 | ![](img/e159297a6ccf638f7fcd3a7125a8f7f0.png) |  |

| 办公提效工具 | [**锦书 - 创新艺术字**](https://modelscope.cn/studios/WordArt/WordArt/summary) | 创意艺术字生成。通过自定义的概念，对文字进行变形和纹理生成，构建富有创意的个性化字形和纹理。 | ![](img/1733bb98ef5d8f7ac1c5d64257395f59.png) |  |

|  | [**创意海报生成**](https://modelscope.cn/studios/iic/PosterGenius/summary) | 做海报不求人！创意海报生成，自由设计，瞬间点亮创意生活。 |  |  |

![](img/6891597fdcbfc60d1e4b4e27b6c9baf4.png)

![](img/1d07ec07c023cc099ee73fc1d8c26fec.png)

## 来了解一下baseline代码的大致结构

> 简要概述代码做了什么

1. **导入库**：首先，代码导入了需要用到的库，包括 `data-juicer` 和微调的工具 `DiffSynth-Studio`

1. **数据集构建**：下载数据集`kolors`，处理数据集

1. **模型微调**：模型微调训练，以及加载训练后的模型

1. **图片生成**：调用训练好的模型生成图片

代码详情

1. 环境安装

```
!pip install simple-aesthetics-predictor

!pip install -v -e data-juicer

!pip uninstall pytorch-lightning -y
!pip install peft lightning pandas torchvision

!pip install -e DiffSynth-Studio
```

1. 下载数据集

```
#下载数据集
from modelscope.msdatasets import MsDataset

ds = MsDataset.load(
    'AI-ModelScope/lowres_anime',
    subset_name='default',
    split='train',
    cache_dir="/mnt/workspace/kolors/data"
)

import json, os
from data_juicer.utils.mm_utils import SpecialTokens
from tqdm import tqdm

os.makedirs("./data/lora_dataset/train", exist_ok=True)
os.makedirs("./data/data-juicer/input", exist_ok=True)
with open("./data/data-juicer/input/metadata.jsonl", "w") as f:
    for data_id, data in enumerate(tqdm(ds)):
        image = data["image"].convert("RGB")
        image.save(f"/mnt/workspace/kolors/data/lora_dataset/train/{data_id}.jpg")
        metadata = {"text": "二次元", "image": [f"/mnt/workspace/kolors/data/lora_dataset/train/{data_id}.jpg"]}
        f.write(json.dumps(metadata))
        f.write("\n")
```

1. 处理数据集，保存数据处理结果

```
data_juicer_config = """
# global parameters
project_name: 'data-process'
dataset_path: './data/data-juicer/input/metadata.jsonl'  # path to your dataset directory or file
np: 4  # number of subprocess to process your dataset

text_keys: 'text'
image_key: 'image'
image_special_token: '<__dj__image>'

export_path: './data/data-juicer/output/result.jsonl'

# process schedule
# a list of several process operators with their arguments
process:
    - image_shape_filter:
        min_width: 1024
        min_height: 1024
        any_or_all: any
    - image_aspect_ratio_filter:
        min_ratio: 0.5
        max_ratio: 2.0
        any_or_all: any
"""
with open("data/data-juicer/data_juicer_config.yaml", "w") as file:
    file.write(data_juicer_config.strip())

!dj-process --config data/data-juicer/data_juicer_config.yaml


import pandas as pd
import os, json
from PIL import Image
from tqdm import tqdm


texts, file_names = [], []
os.makedirs("./data/lora_dataset_processed/train", exist_ok=True)
with open("./data/data-juicer/output/result.jsonl", "r") as file:
    for data_id, data in enumerate(tqdm(file.readlines())):
        data = json.loads(data)
        text = data["text"]
        texts.append(text)
        image = Image.open(data["image"][0])
        image_path = f"./data/lora_dataset_processed/train/{data_id}.jpg"
        image.save(image_path)
        file_names.append(f"{data_id}.jpg")
data_frame = pd.DataFrame()
data_frame["file_name"] = file_names
data_frame["text"] = texts
data_frame.to_csv("./data/lora_dataset_processed/train/metadata.csv", index=False, encoding="utf-8-sig")
data_frame
```

1. lora微调

```
# 下载模型
from diffsynth import download_models
download_models(["Kolors", "SDXL-vae-fp16-fix"])

#模型训练
import os

cmd = """
python DiffSynth-Studio/examples/train/kolors/train_kolors_lora.py \
  --pretrained_unet_path models/kolors/Kolors/unet/diffusion_pytorch_model.safetensors \
  --pretrained_text_encoder_path models/kolors/Kolors/text_encoder \
  --pretrained_fp16_vae_path models/sdxl-vae-fp16-fix/diffusion_pytorch_model.safetensors \
  --lora_rank 16 \
  --lora_alpha 4.0 \
  --dataset_path data/lora_dataset_processed \
  --output_path ./models \
  --max_epochs 1 \
  --center_crop \
  --use_gradient_checkpointing \
  --precision "16-mixed"
""".strip()

os.system(cmd)
```

1. 加载微调好的模型

```
from diffsynth import ModelManager, SDXLImagePipeline
from peft import LoraConfig, inject_adapter_in_model
import torch


def load_lora(model, lora_rank, lora_alpha, lora_path):
    lora_config = LoraConfig(
        r=lora_rank,
        lora_alpha=lora_alpha,
        init_lora_weights="gaussian",
        target_modules=["to_q", "to_k", "to_v", "to_out"],
    )
    model = inject_adapter_in_model(lora_config, model)
    state_dict = torch.load(lora_path, map_location="cpu")
    model.load_state_dict(state_dict, strict=False)
    return model


# Load models
model_manager = ModelManager(torch_dtype=torch.float16, device="cuda",
                             file_path_list=[
                                 "models/kolors/Kolors/text_encoder",
                                 "models/kolors/Kolors/unet/diffusion_pytorch_model.safetensors",
                                 "models/kolors/Kolors/vae/diffusion_pytorch_model.safetensors"
                             ])
pipe = SDXLImagePipeline.from_model_manager(model_manager)

# Load LoRA
pipe.unet = load_lora(
    pipe.unet,
    lora_rank=16, # This parameter should be consistent with that in your training script.
    lora_alpha=2.0, # lora_alpha can control the weight of LoRA.
    lora_path="models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt"
)
```

1. 图片生成
