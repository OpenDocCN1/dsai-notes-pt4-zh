
# 模型微调

> 来源：[https://v11enp9ok1h.feishu.cn/docx/T1Yjdqv4UohcMnxj9wkcjnopnGf](https://v11enp9ok1h.feishu.cn/docx/T1Yjdqv4UohcMnxj9wkcjnopnGf)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

本章节，我们将带领开发者微调 `cogvlm2-llama3-chat-19B`。

请配合以下链接进行尝试：

https://github.com/THUDM/CogVLM2/tree/main/finetune_demo

> 本章节需要使用 A100 以上的高显存显卡，目前暂未有更好的解决方案。

> 本代码仅提供了 `huggingface` 版本模型 `cogvlm2-llama3-chat-19B` 的微调示例。

> 仅提供了微调语言模型的示例。

> 仅提供 `Lora` 微调示例。

> 仅提供对话模型微调示例。

> 暂不支持使用 `zero3` 微调，这可能出现模型无法读取的情况。

## 配置检查和依赖安装

[模型推理](https://zhipu-ai.feishu.cn/wiki/RJOTwSn2XiSw6CkFbplcQxhNnVe) 开始微调之前，确保你已经安装了所需要的依赖，然后，您还需要安装本文件夹下的依赖。

```
pip install -r requirements.txt
```

> `mpi4py` 可能需要安装别的 Linux 依赖包。请根据您的系统环境自行安装

## 数据集构建

数据集由 `images` 和 `labels` 两个文件夹组成（在 `CogVLM-SFT-311K` 中为 `labels_en` 和 `labels_zh` 分别对应中英文标签。在微调代码中，你可以修改这两行代码来修改文件夹名称。

```
self.image_dir = os.path.join(root_dir, 'images')
self.label_dir = os.path.join(root_dir, 'labels_en')  # or 'labels_zh' or 'labels' 可以自行修改.
```

`images` 文件夹中存放了图片文件，`labels` 文件夹中存放了对应的标签文件。图片和标签文件的名称一一对应。图片文件的格式为 `jpg`，标签文件的格式为 `json`。

每个标签文件中包含了一段对话，对话由 `user` 和 `assistant` 两个角色组成，每个角色的对话内容由 `roles` 和 `content` 两个字段组成。如下字段所示。

```
{
  "conversations": [
    {
      "role": "user",
      "content": "What can be inferred about the zebras' behavior and surroundings?"
    },
    {
      "role": "assistant",
      "content": "Based on the image, we can infer that the two zebras are likely seeking relief from the sun's heat, as they are standing side by side under the branches of a thorny tree. This shade-providing tree offers some respite from the sun, possibly during the hottest part of the day. The zebras are in a green field with grass, providing them with an ideal environment to graze and eat while staying near their source of shelter. This shows that the zebras' behavior is influenced by the conditions and available resources in their surroundings. It also highlights that these animals adopt strategies to adapt to the fluctuating conditions of their environment, such as cooperation and seeking shelter, to survive and thrive in their natural habitat."
    }
  ]
}
```

## 开始微调

我们提供了使用单机多卡（包含单卡）的微调脚本 `peft_lora.py`。您可以通过运行以下命令来启动微调。

```
deepspeed peft_lora.py --ds_config ds_config.yaml
```

下图展现了微调过程中的显存占用情况

```
`max_input_len`: 1024
`max_output_len`: 1024
`batch_size`: 2

shell
+-------------------------------------------------------------+
| Processes:                                                  |
|  GPU   GI   CI        PID   Type   Process name  GPU Memory |
|        ID   ID                                      Usage   |
|=============================================================|
|    0   N/A  N/A    704914      C   python          72442MiB |
|    1   N/A  N/A    704915      C   python          72538MiB |
|    2   N/A  N/A    704916      C   python          72538MiB |
|    3   N/A  N/A    704917      C   python          72538MiB |
|    4   N/A  N/A    704918      C   python          72538MiB |
|    5   N/A  N/A    704919      C   python          72538MiB |
|    6   N/A  N/A    704920      C   python          72538MiB |
|    7   N/A  N/A    704921      C   python          72442MiB |
+-------------------------------------------------------------+
```

在代码运行中，`Loss` 数据会被 `tensorboard` 记录，方便可视化查看 `Loss` 收敛情况。

```
tensorboard --logdir=output
```

> 我们强烈推荐您使用 `BF16` 格式进行微调，以避免出现 `Loss` 为 `NaN` 的问题。

## 推理微调后的模型

运行 `peft_infer.py`，你可以使用微调后的模型生成文本。您需要按照代码中的配置要求，配置微调后的模型地址。然后运行:

```
CUDA_VISIBLE_DEVICES=0 python peft_infer.py
```

即可使用微调的模型进行推理。您需要制定单张显卡，以保证模型的权重都在一张卡上。
