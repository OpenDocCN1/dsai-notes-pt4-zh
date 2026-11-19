
# 模型微调

> 来源：[https://ai.feishu.cn/docx/FZbnd4kIEoJpypxi9JCcy5l8nkg](https://ai.feishu.cn/docx/FZbnd4kIEoJpypxi9JCcy5l8nkg)

`SwissArmyTransformer` 是清华大学推出的一种基于`Transformers`的架构。在本章节，我们将带领大家了解如何使用`STA`框架进行微调。

## 微调参数解释

在微调过程中，你讲需要了解这些参数

1. `--experiment-name finetune-$MODEL_TYPE`: 用于指定实验的名称，这里 `$MODEL_TYPE` 是一个变量，表示你正在微调的模型类型。

1. `--model-parallel-size ${MP_SIZE}`: 设置模型并行的大小。`MP_SIZE` 是并行处理模型时使用的GPU数量。

1. `--mode finetune`: 指定操作模式为微调(finetuning)，这意味着你将在已有的模型基础上进行进一步的训练。

1. `--train-iters 2000`: 设置训练迭代的次数，这里是 2000 次迭代。

1. `--resume-dataloader`: 如果设置了这个选项，数据加载器将从上次保存的状态继续加载数据。

1. `$MODEL_ARGS`: 这个是一个占位符，用于传入其他的模型相关参数。

1. `--train-data ${train_data}`: 指定用于训练的数据集。

1. `--valid-data ${valid_data}`: 指定用于验证的数据集。

1. `--distributed-backend nccl`: 设置分布式后端为 `NCCL`，这是优化GPU之间通信的一个库。

1. `--lr-decay-style cosine`: 设置学习率衰减的风格为余弦衰减。

1. `--warmup .02`: 设置预热期，这里是训练的前 2% 用于逐渐增加学习率。

1. `--checkpoint-activations`: 启用激活检查点，有助于节省内存。

1. `--vit_checkpoint_activations`: 类似于上面的检查点，但专门用于`ViT`（Vision Transformer）结构。

1. `--save-interval 200`: 设置保存模型的间隔，这里是每 200 个迭代保存一次。

1. `--eval-interval 200`: 设置评估间隔，也是每 200 个迭代进行一次评估。

1. `--save "./checkpoints"`: 指定模型保存的路径。

1. `--eval-iters 10`: 设置在每次评估时进行的迭代次数，这里是 10 次。

1. `--eval-batch-size 1`: 设置评估时的批处理大小，这里是 1。

1. `--split 1.`: 这个参数通常用于指定数据集的分割比例，但这里的值为 1，可能意味着使用全部数据。

1. `--deepspeed_config test_config_bf16.json`: 指定`Deepspeed`配置文件，`Deepspeed`是一个优化大规模训练的库。

1. `--skip-init`: 跳过初始化步骤。

1. `--seed 2023`: 设置随机种子，以确保实验的可重复性。

## 执行微调脚本（Base）

在这里，我们给出一个微调的示例进行展示。

1. 首先，下载 Captcha Images dataset 并解压，然后，执行数据集切分

```
python utils/split_dataset.py
```

1. 接着，直接运行脚本，即可开始微调

```
bash finetune_demo/finetune_(cogagent/cogvlm)_lora.sh
```

1. 如果使用模型并行训练，可以这么设置(将 这里的 4 替换为 你的 `MP_SIZE`)

```
torchrun --standalone --nnodes=1 --nproc-per-node=4 utils/merge_model.py --version base --bf16 --from_pretrained ./checkpoints/merged_lora_(cogagent/cogvlm490/cogvlm224)
```

1. 最后，验证微调后你的模型

```
bash finetune_demo/evaluate_(cogagent/cogvlm).sh
```
