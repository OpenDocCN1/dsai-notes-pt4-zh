
# 大模型LLM微调经验总结&项目更新

> 来源：`https://geekbang.feishu.cn/docx/JYwDdkmxeoIUgSxlBuUcLUC0nrf`

> 👨🏻 作者：刘聪NLP

> 📃 作者介绍：AIGC前沿知识分享&落地经验总结

> 🛖 知乎主页：`https://www.zhihu.com/people/LiuCongNLP`

> ⏰ 收录时间：2024-05-23 11:28:42

> 🔗 链接：`https://mp.weixin.qq.com/s/wBWpjoMSLgUXammnv0kuAw`

![](img/3e42e2b3608ea73dc8e0c980c07c65d5.png)

关注作者👸号，获取更多AI知识

## 写在前面

大家好，我是刘聪NLP。

为了庆祝首篇千赞文章，首个千Star项目，周末对大模型微调项目代码进行了重构，支持ChatGLM和ChatGLM2模型微调的切换，增加了代码的可读性，并且支持Freeze方法、Lora方法、P-Tuning方法、「全量参数方法」微调。

PS：在对Chat类模型进行SFT时，一定要遵循模型原始的系统指令，否则会出现严重的遗忘或微调效果不明显现象。

```
GitHub: https://github.com/liucongg/ChatGLM-Finetuning
```

![](img/43625b939071e5cbdde945f065d645a9.png)

```
大模型微调经验分享&amp;总结：https://zhuanlan.zhihu.com/p/620885226
```

![](img/9e409192d15606f68e2436d675ca1994.png)

## 更新说明

为什么要更新？

其实一开始这个项目是ChatGLM刚刚出来，笔者进行单卡微调的代码（写的会比较随意），主要是为了帮助大家跑通整个SFT的流程，更加理解代码。没想到获得了这么多关注，并且ChatGLM2也出了，很多网友都提问是否支持，因此做了项目的更新，代码的重构。（后面可能会支持更多模型吧）

相比于V0.1版本，目前版本做了如下更新：

+ 项目仍然采用非`Trainer`的写法，虽然`Trainer`代码简单，但不易修改，大模型时代算法工程师本就成为了数据工程师，因此更需了解训练流程及步骤。

+ 不仅支持单卡训练，也支持多卡训练。

+ 代码中关键内容增加了中文注释。

+ 数据格式已经更新为广泛使用的`{"instruction": instruction, "input": input, "output": output}`格式。

+ 不仅支持微量参数训练，也支持全量参数训练（至少两块A40）。

+ 由于ChatGLM官方代码和模型之前一直在更新，目前代码和模型使用的是最新版本（20230806）。

+ 训练数据构建过程，与ChatGLM、ChatGLM2推理一致，见`utils.py`文件内容，并且在采用单指令集方式，使得模型并没有出现严重的灾难性遗忘。

+ 统计了不同方法显存占用情况。

## Freeze方法

模型微调时，如果遇到显存不够的情况，可以开启`gradient_checkpointing`、`zero3`、`offload`等参数来节省显存。

本文章对`gradient_checkpointing`、`zero3`、`offload`暂时不做过多介绍，后面会进行专项介绍，或者大家可以自行搜索其原理。

Freeze方法，即参数冻结，对原始模型部分参数进行冻结操作，仅训练部分参数，以达到在单卡或多卡，不进行TP或PP操作就可以对大模型进行训练。

微调代码，见`train.py`，核心部分如下：

```
freeze_module_name = args.freeze_module_name.split(&quot;,&quot;)
for name, param in model.named_parameters():
        if not any(nd in name for nd in freeze_module_name):
                param.requires_grad = False
```

针对模型不同层进行修改，可以自行修改`freeze_module_name`参数配置，例如`layers.27.,layers.26.,layers.25.,layers.24.`。训练代码均采用DeepSpeed进行训练，可设置参数包含`train_path`、`model_name_or_path`、`mode`、`train_type`、`freeze_module_name`、`ds_file`、`num_train_epochs`、`per_device_train_batch_size`、`gradient_accumulation_steps`、`output_dir`等，可根据自己的任务配置。

ChatGLM单卡训练

```
CUDA_VISIBLE_DEVICES=0 deepspeed --master_port 520 train.py \
                --train_path data/spo_0.json \
                --model_name_or_path ChatGLM-6B/ \
                --per_device_train_batch_size 1 \
                --max_len 1560 \
                --max_src_len 1024 \
                --learning_rate 1e-4 \
                --weight_decay 0.1 \
                --num_train_epochs 2 \
                --gradient_accumulation_steps 4 \
                --warmup_ratio 0.1 \
                --mode glm \
                --train_type freeze \
                --freeze_module_name &quot;layers.27.,layers.26.,layers.25.,layers.24.&quot; \
                --seed 1234 \
                --ds_file ds_zero2_no_offload.json \
                --gradient_checkpointing \
                --show_loss_step 10 \
                --output_dir ./output-glm
```

ChatGLM四卡训练，通过`CUDA_VISIBLE_DEVICES`控制具体哪几块卡进行训练，如果不加该参数，表示使用运行机器上所有卡进行训练

```
CUDA_VISIBLE_DEVICES=0,1,2,3 deepspeed --master_port 520 train.py \
                --train_path data/spo_0.json \
                --model_name_or_path ChatGLM-6B/ \
                --per_device_train_batch_size 1 \
                --max_len 1560 \
                --max_src_len 1024 \
                --learning_rate 1e-4 \
                --weight_decay 0.1 \
                --num_train_epochs 2 \
                --gradient_accumulation_steps 4 \
                --warmup_ratio 0.1 \
                --mode glm \
                --train_type freeze \
                --freeze_module_name &quot;layers.27.,layers.26.,layers.25.,layers.24.&quot; \
                --seed 1234 \
                --ds_file ds_zero2_no_offload.json \
                --gradient_checkpointing \
                --show_loss_step 10 \
                --output_dir ./output-glm
```

ChatGLM2单卡训练

```
CUDA_VISIBLE_DEVICES=0 deepspeed --master_port 520 train.py \
                --train_path data/spo_0.json \
                --model_name_or_path ChatGLM2-6B/ \
                --per_device_train_batch_size 1 \
                --max_len 1560 \
                --max_src_len 1024 \
                --learning_rate 1e-4 \
                --weight_decay 0.1 \
                --num_train_epochs 2 \
                --gradient_accumulation_steps 4 \
                --warmup_ratio 0.1 \
                --mode glm2 \
                --train_type freeze \
                --freeze_module_name &quot;layers.27.,layers.26.,layers.25.,layers.24.&quot; \
                --seed 1234 \
                --ds_file ds_zero2_no_offload.json \
                --gradient_checkpointing \
                --show_loss_step 10 \
                --output_dir ./output-glm2
```

ChatGLM2四卡训练，通过`CUDA_VISIBLE_DEVICES`控制具体哪几块卡进行训练，如果不加该参数，表示使用运行机器上所有卡进行训练

```
CUDA_VISIBLE_DEVICES=0,1,2,3 deepspeed --master_port 520 train.py \
                --train_path data/spo_0.json \
                --model_name_or_path ChatGLM2-6B/ \
                --per_device_train_batch_size 1 \
                --max_len 1560 \
                --max_src_len 1024 \
                --learning_rate 1e-4 \
                --weight_decay 0.1 \
                --num_train_epochs 2 \
                --gradient_accumulation_steps 4 \
                --warmup_ratio 0.1 \
                --mode glm2 \
                --train_type freeze \
                --freeze_module_name &quot;layers.27.,layers.26.,layers.25.,layers.24.&quot; \
                --seed 1234 \
                --ds_file ds_zero2_no_offload.json \
                --gradient_checkpointing \
                --show_loss_step 10 \
                --output_dir ./output-glm2
```

PS：ChatGLM微调时所用显存要比ChatGLM2多，详细显存占比如下：

![](img/dfe9b4b6baaa82ae2e738db99512c931.png)

## PT方法

PT方法，即P-Tuning方法，参考ChatGLM官方代码，是一种针对于大模型的soft-prompt方法。

![](img/1156558607ef03d35ac21a10284d9b40.png)

+ `P-Tuning`仅对大模型的`Embedding`加入新的参数。

+ `P-Tuning-V2`，将大模型的`Embedding`和每一层前都加上新的参数。

```
P-Tuning: https://arxiv.org/abs/2103.10385
P-Tuning-V2: https://arxiv.org/abs/2110.07602
```

微调代码，见`train.py`，核心部分如下：

```
config = MODE[args.mode]["config"].from_pretrained(args.model_name_or_path)
config.pre_seq_len = args.pre_seq_len
config.prefix_projection = args.prefix_projection
model = MODE[args.mode]["model"].from_pretrained(args.model_name_or_path, config=config)
for name, param in model.named_parameters():
        if not any(nd in name for nd in ["prefix_encoder"]):
                param.requires_grad = False
```

当`prefix_projection`为`True`时，为`P-Tuning-V2`方法，在大模型的`Embedding`和每一层前都加上新的参数；为`False`时，为`P-Tuning`方法，仅在大模型的`Embedding`上新的参数。

训练代码均采用DeepSpeed进行训练，可设置参数包含`train_path`、`model_name_or_path`、`mode`、`train_type`、`pre_seq_len`、`prefix_projection`、`ds_file`、`num_train_epochs`、`per_device_train_batch_size`、`gradient_accumulation_steps`、`output_dir`等，可根据自己的任务配置。

ChatGLM单卡训练

```
CUDA_VISIBLE_DEVICES=0 deepspeed --master_port 520 train.py \
                --train_path data/spo_0.json \
                --model_name_or_path ChatGLM-6B \
                --per_device_train_batch_size 1 \
                --max_len 768 \
                --max_src_len 512 \
                --learning_rate 1e-4 \
                --weight_decay 0.1 \
                --num_train_epochs 2 \
                --gradient_accumulation_steps 4 \
                --warmup_ratio 0.1 \
                --mode glm \
                --train_type ptuning \
                --seed 1234 \
                --ds_file ds_zero2_no_offload.json \
                --gradient_checkpointing \
                --show_loss_step 10 \
                --pre_seq_len 16 \
                --prefix_projection True \
                --output_dir ./output-glm
```

ChatGLM四卡训练，通过`CUDA_VISIBLE_DEVICES`控制具体哪几块卡进行训练，如果不加该参数，表示使用运行机器上所有卡进行训练
