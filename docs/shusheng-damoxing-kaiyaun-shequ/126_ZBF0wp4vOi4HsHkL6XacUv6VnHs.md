# 沐曦 XTuner 微调 InternLM  赛事 Baseline

> 来源：[https://aicarrier.feishu.cn/docx/CA0JdJMMroCCS1xvFVhcyHiUnDD](https://aicarrier.feishu.cn/docx/CA0JdJMMroCCS1xvFVhcyHiUnDD)

# 操作视频

https://aicarrier.feishu.cn/minutes/obcni337zj434hm7175kz3k8?from=from_copylink

# 环境介绍

GPU：沐曦MXC500（32G）

CPU：8核心

内存：96G

# DaoCloud平台操作

上海六区地址：https://console.d.run/zestu/market?regionId=sh-06

## 初始化存储

操作步骤：文件存储—>选择《上海六区》->初始化文件存储

说明：文件存储将会挂载在/root/data 目录下 ，免费容量为20G

特别注意：初始化完成后，请在web页面上传一个空文件,否则无法挂载成功

## 选择算力云->算力市场->[上海六区](https://console.d.run/zestu/market?regionId=sh-06)（如下图选择MXC500）立即购买

![](img/0743270d3497c0d2644206d794cf0867.png)

## **选择镜像 **

下拉框1 :  基础镜像->Dlinfer

下拉框2：选择带有Xtuner的镜像版本（如下图）

![](img/24aa6f713df28c4b4896d58dba3f5480.png)

## 创建并启动容器实例

1.选择JuypterLab 进入开发机

![](img/39806caa920dbe54cdd36c2ef129c00d.png)

# 开发机介绍

## 文件夹介绍

默认进入/root 目录下

data文件夹为初始化挂载文件夹（读写）

Public-model 为平台共享模型仓库

![](img/9e04c2e3b723b93a07b9e73fa79e6e3a.png)

## 终端介绍

![](img/e2c0000018eb60c282f62e723cebc045.png)

## 环境检查

镜像中已经内置Xtuner

```

pip show xtuner

#可以看到环境中已经有了xtuner
#Name: xtuner
#Version: 0.1.23
#Summary: An efficient, flexible and full-featured toolkit for fine-tuning large models
#Home-page: https://github.com/InternLM/xtuner
#Author: XTuner Contributors
#Author-email: openmmlab@gmail.com
#License: Apache License 2.0
#Location: /workspace/xtuner
#Editable project location: /workspace/xtuner
#Requires: bitsandbytes, datasets, einops, lagent, mmengine, openpyxl, peft, scikit-image, scipy, SentencePiece, tiktoken, torch, torchvision, transformers, transformers_stream_generator
#Required-by: 

```

## GPU信息监控

mx-smi  与 nvidia-smi 的功能一致

```
root@ins-5njhw-645f976478-djpwb:~# mx-smi
mx-smi  version: 2.1.9

=================== MetaX System Management Interface Log ===================
Timestamp                                         : Tue May 20 10:48:51 2025

Attached GPUs                                     : 1
+---------------------------------------------------------------------------------+
| MX-SMI 2.1.9                        Kernel Mode Driver Version: 2.9.8           |
| MACA Version: 2.31.0.6              BIOS Version: 1.20.3.0                      |
|------------------------------------+---------------------+----------------------+
| GPU         NAME                   | Bus-id              | GPU-Util             |
| Temp        Power                  | Memory-Usage        |                      |
|====================================+=====================+======================|
| 0           MXC500 VF              | 0000:3b:00.2        | 0%                   |
| N/A         N/A                    | 618/32768 MiB       |                      |
+------------------------------------+---------------------+----------------------+

+---------------------------------------------------------------------------------+
| Process:                                                                        |
|  GPU                    PID         Process Name                 GPU Memory     |
|                                                                  Usage(MiB)     |
|=================================================================================|
|  no process found                                                               |
+---------------------------------------------------------------------------------+

End of Log
root@ins-5njhw-645f976478-djpwb:~# 
```

# 补充安装包

## 安装mpi4py

```
pip install mpi4py

pip install transformers==4.48.0
```

建议安装 ，否则报错

## 链接模型位置

```
ln -s /root/public-model/models/Shanghai_AI_Laboratory/internlm2_5-1_8b-chat ./
```

## 数据集（已经处理好）

![](img/9b9bcf7ca6a537eb96d6f664e7340b5c.png)

[pretrain.jsonl](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MCX8bDkBqoNVI2xX7OocQiLOnBf)

![](img/951afee7365af35f4f0811269487ac68.png)

[sftdata.jsonl](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Q3OwbytMeoO9SKxx6gFck8GMn2b)

```
apt install tree
tree -L 2
```

可以将项目目录结构组织成如下图所示：

![](img/17c3de758f533b8da6fe6cd11fbca545.png)

# 开启训练

## 预训练

```
xtuner train pretrain.py --deepspeed deepspeed_zero1
```

注意替换模型和数据集的位置分别位于23行27行

```
pretrained_model_name_or_path = &#x27;./internlm2_5-1_8b-chat&#x27;

data_files = [&#x27;./datasets/pretrain.jsonl&#x27;]
```

```
import torch
from datasets import load_dataset
from mmengine.dataset import DefaultSampler
from mmengine.hooks import (CheckpointHook, DistSamplerSeedHook, IterTimerHook,
                            LoggerHook, ParamSchedulerHook)
from mmengine.optim import AmpOptimWrapper, CosineAnnealingLR, LinearLR
from peft import LoraConfig
from torch.optim import AdamW
from transformers import AutoModelForCausalLM, AutoTokenizer,BitsAndBytesConfig

from xtuner.dataset import process_hf_dataset
from xtuner.dataset.collate_fns import default_collate_fn
from xtuner.dataset.map_fns import pretrain_map_fn
from xtuner.engine.hooks import (DatasetInfoHook, EvaluateChatHook,
                                 VarlenAttnArgsToMessageHubHook)
from xtuner.engine.runner import TrainLoop
from xtuner.model import SupervisedFinetune

#######################################################################
#                          PART 1  Settings                           #
#######################################################################
# Model
pretrained_model_name_or_path = &#x27;./internlm2_5-1_8b-chat&#x27;
use_varlen_attn = False

# Data
data_files = [&#x27;./datasets/pretrain.jsonl&#x27;]
max_length = 2048
pack_to_max_length = False

# Scheduler &amp; Optimizer
batch_size = 8  # per_device
accumulative_counts = 32  # bs = 1 GPU * 1 batch_size_per_device * 16 acc
dataloader_num_workers = 0
max_epochs = 1
optim_type = AdamW
lr = 2e-5
betas = (0.9, 0.999)
weight_decay = 0
max_norm = 1  # grad clip
warmup_ratio = 0.03

# Save
save_steps = 500
save_total_limit = 2  # Maximum checkpoints to keep (-1 means unlimited)

# Evaluate the generation performance during the training
evaluation_freq = 500
SYSTEM = &#x27;&#x27;
evaluation_inputs = [&#x27;上海是&#x27;, &#x27;Shanghai is&#x27;]

#######################################################################
#                      PART 2  Model &amp; Tokenizer                      #
#######################################################################
tokenizer = dict(
    type=AutoTokenizer.from_pretrained,
    pretrained_model_name_or_path=pretrained_model_name_or_path,
    trust_remote_code=True,
    padding_side=&#x27;right&#x27;)

model = dict(
    type=SupervisedFinetune,
    use_varlen_attn=use_varlen_attn,
    llm=dict(
        type=AutoModelForCausalLM.from_pretrained,
        pretrained_model_name_or_path=pretrained_model_name_or_path,
        trust_remote_code=True,
        quantization_config=dict(
            type=BitsAndBytesConfig,
            load_in_4bit=True,
            load_in_8bit=False,
            llm_int8_threshold=6.0,
            llm_int8_has_fp16_weight=False,
            bnb_4bit_compute_dtype=torch.float16,
            bnb_4bit_use_double_quant=True,
            bnb_4bit_quant_type=&#x27;nf4&#x27;)
    ),
    lora=dict(
        type=LoraConfig,
        r=64,
        lora_alpha=16,
        lora_dropout=0.1,
        bias=&#x27;none&#x27;,
        task_type=&#x27;CAUSAL_LM&#x27;)
)


#######################################################################
#                      PART 3  Dataset &amp; Dataloader                   #
#######################################################################
train_dataset = dict(
    type=process_hf_dataset,
    dataset=dict(type=load_dataset, path=&#x27;json&#x27;, data_files=data_files),
    tokenizer=tokenizer,
    max_length=max_length,
    dataset_map_fn=pretrain_map_fn,
    template_map_fn=None,
    remove_unused_columns=True,
    shuffle_before_pack=False,
    pack_to_max_length=pack_to_max_length,
    use_varlen_attn=use_varlen_attn)

train_dataloader = dict(
    batch_size=batch_size,
    num_workers=dataloader_num_workers,
    dataset=train_dataset,
    sampler=dict(type=DefaultSampler, shuffle=True),
    collate_fn=dict(type=default_collate_fn, use_varlen_attn=use_varlen_attn))

#######################################################################
#                    PART 4  Scheduler &amp; Optimizer                    #
#######################################################################
# optimizer
optim_wrapper = dict(
    type=AmpOptimWrapper,
    optimizer=dict(
        type=optim_type, lr=lr, betas=betas, weight_decay=weight_decay),
    clip_grad=dict(max_norm=max_norm, error_if_nonfinite=False),
    accumulative_counts=accumulative_counts,
    loss_scale=&#x27;dynamic&#x27;,
    dtype=&#x27;float16&#x27;)

# learning policy
# More information: https://github.com/open-mmlab/mmengine/blob/main/docs/en/tutorials/param_scheduler.md  # noqa: E501
param_scheduler = [
    dict(
        type=LinearLR,
        start_factor=1e-5,
        by_epoch=True,
        begin=0,
        end=warmup_ratio * max_epochs,
        convert_to_iter_based=True),
    dict(
        type=CosineAnnealingLR,
        eta_min=0.0,
        by_epoch=True,
        begin=warmup_ratio * max_epochs,
        end=max_epochs,
        convert_to_iter_based=True)
]

# train, val, test setting
train_cfg = dict(type=TrainLoop, max_epochs=max_epochs)

#######################################################################
#                           PART 5  Runtime                           #
#######################################################################
# Log the dialogue periodically during the training process, optional
custom_hooks = [
    dict(type=DatasetInfoHook, tokenizer=tokenizer),
    dict(
        type=EvaluateChatHook,
        tokenizer=tokenizer,
        every_n_iters=evaluation_freq,
        evaluation_inputs=evaluation_inputs,
        system=SYSTEM)
]

if use_varlen_attn:
    custom_hooks += [dict(type=VarlenAttnArgsToMessageHubHook)]

# configure default hooks
default_hooks = dict(
    # record the time of every iteration.
    timer=dict(type=IterTimerHook),
    # print log every 10 iterations.
    logger=dict(type=LoggerHook, log_metric_by_epoch=False, interval=10),
    # enable the parameter scheduler.
    param_scheduler=dict(type=ParamSchedulerHook),
    # save checkpoint per `save_steps`.
    checkpoint=dict(
        type=CheckpointHook,
        by_epoch=False,
        interval=save_steps,
        max_keep_ckpts=save_total_limit),
    # set sampler seed in distributed evrionment.
    sampler_seed=dict(type=DistSamplerSeedHook),
)

# configure environment
env_cfg = dict(
    # whether to enable cudnn benchmark
    cudnn_benchmark=False,
    # set multi process parameters
    mp_cfg=dict(mp_start_method=&#x27;fork&#x27;, opencv_num_threads=0),
    # set distributed parameters
    dist_cfg=dict(backend=&#x27;nccl&#x27;),
)

# set visualizer
visualizer = None

# set log level
log_level = &#x27;INFO&#x27;

# load from which checkpoint
load_from = None

# whether to resume training from the loaded checkpoint
resume = False

# Defaults to use random seed and disable `deterministic`
randomness = dict(seed=None, deterministic=False)

# set log processor
log_processor = dict(by_epoch=False)

```

## 模型转换pth_to_hf

```
xtuner convert pth_to_hf pretrain.py \
                         ./work_dirs/pretrain/iter_xx.pth \
                         ./work_dirs/pretrain/hf
```

## 模型合并merge

```
xtuner convert merge ./internlm2_5-1_8b-chat \
                     ./work_dirs/pretrain/hf \
                     ./work_dirs/pretrain/merged \
                     --max-shard-size 2GB

```

## SFT训练

```
xtuner train sft.py --deepspeed deepspeed_zero1
```

注意替换模型和数据集的位置分别位于34行38行

```
pretrained_model_name_or_path = &quot;./work_dirs/pretrain/merged&quot;

alpaca_en_path = &quot;./datasets/sftdata.jsonl&quot;#可以换成自己的数据路径
```

```
# Copyright (c) OpenMMLab. All rights reserved.
import torch
from datasets import load_dataset
from mmengine.dataset import DefaultSampler
from mmengine.hooks import (
    CheckpointHook,
    DistSamplerSeedHook,
    IterTimerHook,
    LoggerHook,
    ParamSchedulerHook,
)
from mmengine.optim import AmpOptimWrapper, CosineAnnealingLR, LinearLR
from peft import LoraConfig
from torch.optim import AdamW
from transformers import AutoModelForCausalLM, AutoTokenizer, BitsAndBytesConfig

from xtuner.dataset import process_hf_dataset
from xtuner.dataset.collate_fns import default_collate_fn
from xtuner.dataset.map_fns import alpaca_map_fn, template_map_fn_factory
from xtuner.engine.hooks import (
    DatasetInfoHook,
    EvaluateChatHook,
    VarlenAttnArgsToMessageHubHook,
)
from xtuner.engine.runner import TrainLoop
from xtuner.model import SupervisedFinetune
from xtuner.parallel.sequence import SequenceParallelSampler
from xtuner.utils import PROMPT_TEMPLATE, SYSTEM_TEMPLATE

#######################################################################
#                          PART 1  Settings                           #
#######################################################################
# Model
pretrained_model_name_or_path = &quot;./work_dirs/pretrain/merged&quot;
use_varlen_attn = False

# Data
alpaca_en_path = &quot;./datasets/sftdata.jsonl&quot;#换成自己的数据路径
prompt_template = PROMPT_TEMPLATE.internlm2_chat
max_length = 2048
pack_to_max_length = False

# parallel
sequence_parallel_size = 1

# Scheduler &amp; Optimizer
batch_size = 1  # per_device
accumulative_counts = 1
accumulative_counts *= sequence_parallel_size
dataloader_num_workers = 0
max_epochs = 3
optim_type = AdamW
lr = 2e-4
betas = (0.9, 0.999)
weight_decay = 0
max_norm = 1  # grad clip
warmup_ratio = 0.03

# Save
save_steps = 500
save_total_limit = 2  # Maximum checkpoints to keep (-1 means unlimited)

# Evaluate the generation performance during the training
evaluation_freq = 500
SYSTEM = SYSTEM_TEMPLATE.alpaca
evaluation_inputs = [&quot;请给我介绍五个上海的景点&quot;, &quot;Please tell me five scenic spots in Shanghai&quot;]

#######################################################################
#                      PART 2  Model &amp; Tokenizer                      #
#######################################################################
tokenizer = dict(
    type=AutoTokenizer.from_pretrained,
    pretrained_model_name_or_path=pretrained_model_name_or_path,
    trust_remote_code=True,
    padding_side=&quot;right&quot;,
)

model = dict(
    type=SupervisedFinetune,
    use_varlen_attn=use_varlen_attn,
    llm=dict(
        type=AutoModelForCausalLM.from_pretrained,
        pretrained_model_name_or_path=pretrained_model_name_or_path,
        trust_remote_code=True,
        torch_dtype=torch.float16,
        quantization_config=dict(
            type=BitsAndBytesConfig,
            load_in_4bit=True,
            load_in_8bit=False,
            llm_int8_threshold=6.0,
            llm_int8_has_fp16_weight=False,
            bnb_4bit_compute_dtype=torch.float16,
            bnb_4bit_use_double_quant=True,
            bnb_4bit_quant_type=&quot;nf4&quot;,
        ),
    ),
    lora=dict(
        type=LoraConfig,
        r=64,
        lora_alpha=16,
        lora_dropout=0.1,
        bias=&quot;none&quot;,
        task_type=&quot;CAUSAL_LM&quot;,
    ),
)

#######################################################################
#                      PART 3  Dataset &amp; Dataloader                   #
#######################################################################
alpaca_en = dict(
    type=process_hf_dataset,
    dataset=dict(type=load_dataset, path=&#x27;json&#x27;, data_files=alpaca_en_path),
    tokenizer=tokenizer,
    max_length=max_length,
    dataset_map_fn=alpaca_map_fn,
    template_map_fn=dict(type=template_map_fn_factory, template=prompt_template),
    remove_unused_columns=True,
    shuffle_before_pack=True,
    pack_to_max_length=pack_to_max_length,
    use_varlen_attn=use_varlen_attn,
)

sampler = SequenceParallelSampler if sequence_parallel_size &gt; 1 else DefaultSampler
train_dataloader = dict(
    batch_size=batch_size,
    num_workers=dataloader_num_workers,
    dataset=alpaca_en,
    sampler=dict(type=sampler, shuffle=True),
    collate_fn=dict(type=default_collate_fn, use_varlen_attn=use_varlen_attn),
)

#######################################################################
#                    PART 4  Scheduler &amp; Optimizer                    #
#######################################################################
# optimizer
optim_wrapper = dict(
    type=AmpOptimWrapper,
    optimizer=dict(type=optim_type, lr=lr, betas=betas, weight_decay=weight_decay),
    clip_grad=dict(max_norm=max_norm, error_if_nonfinite=False),
    accumulative_counts=accumulative_counts,
    loss_scale=&quot;dynamic&quot;,
    dtype=&quot;float16&quot;,
)

# learning policy
# More information: https://github.com/open-mmlab/mmengine/blob/main/docs/en/tutorials/param_scheduler.md  # noqa: E501
param_scheduler = [
    dict(
        type=LinearLR,
        start_factor=1e-5,
        by_epoch=True,
        begin=0,
        end=warmup_ratio * max_epochs,
        convert_to_iter_based=True,
    ),
    dict(
        type=CosineAnnealingLR,
        eta_min=0.0,
        by_epoch=True,
        begin=warmup_ratio * max_epochs,
        end=max_epochs,
        convert_to_iter_based=True,
    ),
]

# train, val, test setting
train_cfg = dict(type=TrainLoop, max_epochs=max_epochs)

#######################################################################
#                           PART 5  Runtime                           #
#######################################################################
# Log the dialogue periodically during the training process, optional
custom_hooks = [
    dict(type=DatasetInfoHook, tokenizer=tokenizer),
    dict(
        type=EvaluateChatHook,
        tokenizer=tokenizer,
        every_n_iters=evaluation_freq,
        evaluation_inputs=evaluation_inputs,
        system=SYSTEM,
        prompt_template=prompt_template,
    ),
]

if use_varlen_attn:
    custom_hooks += [dict(type=VarlenAttnArgsToMessageHubHook)]

# configure default hooks
default_hooks = dict(
    # record the time of every iteration.
    timer=dict(type=IterTimerHook),
    # print log every 10 iterations.
    logger=dict(type=LoggerHook, log_metric_by_epoch=False, interval=10),
    # enable the parameter scheduler.
    param_scheduler=dict(type=ParamSchedulerHook),
    # save checkpoint per `save_steps`.
    checkpoint=dict(
        type=CheckpointHook,
        by_epoch=False,
        interval=save_steps,
        max_keep_ckpts=save_total_limit,
    ),
    # set sampler seed in distributed evrionment.
    sampler_seed=dict(type=DistSamplerSeedHook),
)

# configure environment
env_cfg = dict(
    # whether to enable cudnn benchmark
    cudnn_benchmark=False,
    # set multi process parameters
    mp_cfg=dict(mp_start_method=&quot;fork&quot;, opencv_num_threads=0),
    # set distributed parameters
    dist_cfg=dict(backend=&quot;nccl&quot;),
)

# set visualizer
visualizer = None

# set log level
log_level = &quot;INFO&quot;

# load from which checkpoint
load_from = None

# whether to resume training from the loaded checkpoint
resume = False

# Defaults to use random seed and disable `deterministic`
randomness = dict(seed=None, deterministic=False)

# set log processor
log_processor = dict(by_epoch=False)

```

## 模型转换pth_to_hf

```
xtuner convert pth_to_hf sft.py \
                         ./work_dirs/sft/iter_xx.pth \
                         ./work_dirs/sft/hf
```

## 模型合并

```
#与原始模型合并
xtuner convert merge ./internlm2_5-1_8b-chat \
                     ./work_dirs/sft/hf \
                     ./work_dirs/sft/merged \
                     --max-shard-size 2GB
```

训练完后work_dirs目录结构参考：

![](img/ca89608f2fc62aebb4367a54208404d8.png)

# 上传到[魔乐](https://modelers.cn/models/new)

## 魔乐上传配置

1.  创建组织

![](img/d51fc33a00b16e70d9a1f1928139b13c.png)

1.  在组织内创建模型

![](img/d4e98fac5e4c6a42450765883e7e204d.png)

![](img/ca2d761a09b584de5f7c700c911806f6.png)

1.  为了保证比赛公正，本次使用的魔乐社区托管权重，请使用组织功能，并邀请JimmyMa-99 用户加入团队，同时请将上传的是否公开为私有模型。

![](img/e24d4ead854d5f7744eefee451bd8180.png)

1.  一定要邀请米佬，等同意 。如在 24 小时内没有同意邀请，请联系微信`小镇数字青年`

![](img/b74ce2a6324d4c9c90ed06a4dbcac1e9.png)

## 上传模型及日志

1.  下载所需环境包

```
python3.10 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install openmind_hub[all]
pip install &quot;openmind[pt]&quot; --extra-index-url https://download.pytorch.org/whl/cpu
```

1.  运行上传脚本

```
from openmind_hub import upload_folder

upload_folder(
    token=&quot;a5cxxe&quot;,
    folder_path=&quot;xxx&quot;,#模型位置，
    repo_id=&quot;xxx&quot;,#模型空间名字
)
```

```
python upload_model.py
```

log原本在 sft 目录下，需要把这个 log 放到 merge 然后 传 merge

这样/root/work_dirs/sft/merged 文件下面不仅有模型信息，还需要有训练的日志信息（包含硬件、loss 等），如下图所示：

![](img/66b000d8325e9e34cc7d8e6b4cebe504.png)

运行脚本上传完成，魔乐空间会如如下所示：

![](img/84bafa72711ae551b33dd7a3b4bc68a4.png)

## 提交评测

1.  确保JimmyMa-99 用户加入团队

![](img/0f43534708fbd3713977632207ca6bd6.png)

1.  提交测评的[入口](https://aicarrier.feishu.cn/share/base/form/shrcnCR6hPPtl3v3y2ZVXomhME9)：https://aicarrier.feishu.cn/share/base/form/shrcnCR6hPPtl3v3y2ZVXomhME9，输入 uid 和对应的微调权重的模型 ID。

提交测评前请仔细检查是否**顺序**执行 6、7 节！

提交后不能更改模型空间文件！

![](img/4060085f4188663abdd6c3bfa15c7ba3.png)

## 7.4 查询成绩

提交完成，自动等评测榜单更新(大概等十分钟)。

查询测评的分数[入口](https://aicarrier.feishu.cn/share/base/query/shrcn3UPdvR9e0D1REcjQ9LLNub)：https://aicarrier.feishu.cn/share/base/query/shrcn3UPdvR9e0D1REcjQ9LLNub 输入 uid 即可查询。

![](img/2134aa0f0644bb9656f2942c171adb6e.png)

# 本文参考

[L1G4-InternLM 论文分类实践（沐曦版）](https://aicarrier.feishu.cn/wiki/IXKnwboj6i5PKzkwiEtce2xkn4c)