
# 微调训练

> 来源：[`https://ai.feishu.cn/docx/Edz9dxjuEoqk6ExiYECcMxJ7n6b`](https://ai.feishu.cn/docx/Edz9dxjuEoqk6ExiYECcMxJ7n6b)

本仓库旨在带领开发者上手`GLM-Edge`开源模型的微调。

请配合以下代码库中微调部分代码进行上手练习:

`https://github.com/THUDM/GLM-Edge/tree/main/finetune`

## 微调参数

### 配置文件

我们将微调的参数统一放置在`finetune/configs`下，将会有四个文件

```
.
├── deepspeed.json
├── lora.yaml
└── sft.yaml
```

分别为`deepspeed`运行参数，`lora.yaml`配置文件和全参微调的配置文件。

### 参数解释

每一份配置文件除了在细节上有一定区别，其他配置大同小异，我们针对每个参数做了简单的解释。

+ `data_config` 这些参数是关于数据集的位置

+ `train_file` 训练集地址

+ `val_file` 验证集地址

+ `test_file` 测试集地址

+ `max_input_length`微调模型输入的最大长度，对于`glm-edge-v`系列模型，由于图片占位token为584，因此值需要设置大些，默认为1024。

+ `max_output_length` 微调模型输出的最大长度，默认为1024。

+ `peft_config` Huggingface PEFT 框架的相关参数，`peft_type` 选择高效微调的方式，可以为`LORA` ，并需要搭配对应的参数。

+ 如果你选择使用 `LORA`，则这些参数是必须的

+ `LORA_RANK`：这个参数决定了模型参数修改的复杂度和灵活性。较低的秩意味着更少的参数和更快的训练速度，但可能减少模型的灵活性。较高的秩可以提高模型的灵活性，但会增加参数数量和计算负担。我们默认设置为8。

+ `lora_alpha`是控制LoRA调整幅度的参数。它决定了对原始模型参数的修改程度。较高的`lora_alpha`值意味着对原始模型参数的更大调整，这可能有助于模型更好地适应新的任务或数据，但也可能导致过拟合。较低的值则意味着较小的调整，可能保持模型的泛化能力，但可能不足以充分适应新任务。我们默认设置为32

+ `lora_dropout`指的是在LoRA层应用的dropout比率。这意味着在训练过程中，网络的一部分连接会随机断开，以防止模型过度依赖于训练数据中的特定模式。较高的dropout比率可以增加模型的泛化能力，但也可能导致学习效率降低。我们设置为0.1。

+ `target_modules`（不可修改的参数）: 调整`GLM-Edge`系列模型必须使用`["q_proj","k_proj", "v_proj"]`，这里没有在配置文件写出。

+ `training_args`这些参数是关于训练中通用的参数

+ `output_dir`: 训练过程中输出文件的目录，默认是 `./output`。

+ `max_steps`: 训练的最大步数，这里设置为 3000。

+ `per_device_train_batch_size`: 每个设备上的训练批量大小，默认是 4。

+ `dataloader_num_workers`: 数据加载时使用的工作进程数，默认值是 16。

+ `remove_unused_columns`: 是否移除未使用的列，默认设置为 `False`。

+ `save_strategy`: 保存策略，默认是按步数保存（`steps`）。

+ `save_steps`: 保存步数，默认设置为每 500步保存一次。

+ `log_level`: 日志等级，默认设置为 `info`,可改为`debug`。

+ `logging_steps`: 日志记录步数，默认为每 10 步记录一次。

+ `per_device_eval_batch_size`: 每个设备上的评估批量大小，默认值为16。

+ `evaluation_strategy`: 评估策略，默认是按步数评估（`steps`）。

+ `eval_steps`: 评估步数，默认设置为每 500 步进行一次评估。

+ `predict_with_generate`: 是否在预测时生成输出，这里设置为`True`。

## 数据集格式

微调数据集采用 `GLM-Edge` 对话格式约定，对不同角色添加不同 `loss_mask` 从而在一遍计算中为多轮回复计算 `loss` 。对于数据文件，样例采用如下格式：

对于`glm-edge-chat`系列模型，您应该按照以下格式整理数据。

```
[
  {
    &quot;messages&quot;: [
      {
        &quot;role&quot;: &quot;system&quot;,
        &quot;content&quot;: &quot;&lt;system prompt text&gt;&quot;,
      },
      {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: &quot;&lt;user prompt text&gt;&quot;
      },
      {
        &quot;role&quot;: &quot;assistant&quot;,
        &quot;content&quot;: &quot;&lt;assistant response text&gt;&quot;
      },
      // Multi_turns
      {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: &quot;&lt;user prompt text&gt;&quot;
      },
      {
        &quot;role&quot;: &quot;assistant&quot;,
        &quot;content&quot;: &quot;&lt;assistant response text&gt;&quot;
      },
    ]
  }
]
```

对于`glm-edge-v`系列模型，您应该按照以下格式整理数据。

```
[
    {
      &quot;messages&quot;: [
        {
          &quot;role&quot;: &quot;user&quot;,
          &quot;content&quot;: [
            {
              &quot;type&quot;: &quot;image&quot;,
              &quot;image&quot;: &quot;path/to/image&quot;
            },
            {
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;图片中的狗在做什么？&quot;
            }
          ]
        },
        {
          &quot;role&quot;: &quot;assistant&quot;,
          &quot;content&quot;: [
            {
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;zRzRzRzRzRzRzR!这只狗躺在公寓客厅的绿色沙发上。&quot;
            }
          ]
        },
        {
          &quot;role&quot;: &quot;user&quot;,
          &quot;content&quot;: [
            {
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;这只狗是什么颜色的？&quot;
            }
          ]
        },
        {
          &quot;role&quot;: &quot;assistant&quot;,
          &quot;content&quot;: [
            {
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;zRzRzRzRzRzRzR!这只狗是棕色和白色的。&quot;
            }
          ]
        }
      ]
    }
]
```

## 开始微调

通过以下代码执行 单机多卡/多机多卡 运行，这是使用 `deepspeed` 作为加速方案的，您需要安装 `deepspeed`。接着，按照此命令运行：

```
OMP_NUM_THREADS=1 torchrun --standalone --nnodes=1 --nproc_per_node=8  finetune.py  data/AdvertiseGen/  THUDM/glm-edge-4b-chat  configs/lora.yaml # For Chat Fine-tune
OMP_NUM_THREADS=1 torchrun --standalone --nnodes=1 --nproc_per_node=8  finetune_vision.py  data/CogVLM-311K/  THUDM/glm-edge-v-5b  configs/lora.yaml  # For VQA Fine-tune
```

通过以下代码执行 单机单卡 运行。

```
python finetune.py  data/AdvertiseGen/  THUDM/glm-edge-4b-chat  configs/lora.yaml # For Chat Fine-tune
python finetune_vision.py  data/CogVLM-311K/  THUDM/glm-edge-v-5b configs/lora.yaml # For VQA Fine-tune
```

## 从保存点进行微调

如果按照上述方式进行训练，每次微调都会从头开始，如果你想从训练一半的模型开始微调，你可以加入第四个参数，这个参数有两种传入方式:

1. `yes`, 自动从最后一个保存的 `Checkpoint`开始训练

1. `XX`, 断点号数字 例 `600` 则从序号`600` `Checkpoint`开始训练

```
python finetune.py  data/AdvertiseGen/  THUDM/glm-edge-4b-chat  configs/lora.yaml yes
python finetune_vision.py  data/CogVLM-311K/  THUDM/glm-edge-4b-chat  configs/lora.yaml yes
```

## 推理微调后的模型

您可以在 `finetune/inference.py` 中使用我们的微调后的模型，仅需要一行代码就能简单的进行测试。

```
python inference.py your_finetune_path
```

这样，得到的回答就微调后的回答了。
