# Lora微调Diffusers 示例

> 来源：[https://ai.feishu.cn/docx/GAZwd9BB5of9fUxIPRMccjRBn5b](https://ai.feishu.cn/docx/GAZwd9BB5of9fUxIPRMccjRBn5b)

#### diffusers框架介绍

🤗 Diffusers框架是huggface在扩散模型相关的开源方案，通过模块工具，可以做到方便快捷的集成自定义框架，在模型训练方向Diffusers有accelerate加速支持，兼容常见的推理框架

在视频教学维度的实际测试发现，我们的CogVideoX在主题一致、动态信息一致、背景一致、物体信息一致、运动流畅、色彩、场景、外观风格、时间风格等维度上取得了不错的效果，但在人体动作、空间关系、多物体等维度上无法取得很好的效果。

通过微调训练弥补这方面的差距，对效果欠缺的数据集通过Diffusers框架进行训练，

#### 数据准备

首先准备两个文件，其中一个文件包含每行为提示词内容，另一个文件包含视频数据的每行为路径（视频文件的路径必须相对于您在指定 --instance_data_root 时传递的路径）。让我们看一个例子来更好地理解这一点！

假设您已将 --instance_data_root 指定为 /dataset，并且此目录包含以下文件：prompts.txt 和 videos.txt。

prompts.txt 文件应包含以行分隔的提示：

```
A black and white animated sequence featuring a rabbit, named Rabbity Ribfried, and an anthropomorphic goat in a musical, playful environment, showcasing their evolving interaction.
A black and white animated sequence on a ship&#x27;s deck features a bulldog character, named Bully Bulldoger, showcasing exaggerated facial expressions and body language. The character progresses from confident to focused, then to strained and distressed, displaying a range of emotions as it navigates challenges. The ship&#x27;s interior remains static in the background, with minimalistic details such as a bell and open door. The character&#x27;s dynamic movements and changing expressions drive the narrative, with no camera movement to distract from its evolving reactions and physical gestures.
...
```

videos.txt 文件应包含指向视频文件的行分隔路径。请注意，该路径应相对于 --instance_data_root 目录。

```
videos/00000.mp4
videos/00001.mp4
...
```

总体而言，如果您在数据集根目录上运行 tree 命令，您的数据集将如下所示：

```
/dataset
├── prompts.txt
├── videos.txt
├── videos
    ├── videos/00000.mp4
    ├── videos/00001.mp4
    ├── ...
```

使用此格式时，必须prompts.txt --caption_column 并且必须videos.txt --video_column。

在huggingface上有一些公开的数据集，

例如https://huggingface.co/datasets/Wild-Heart

#### 训练

您需要通过安装必要的要求来设置开发环境。需要以下软件包：

+   Torch 2.0 或更高版本，基于您正在使用的训练功能（可能需要最新版本或夜间版本才能进行量化/深度训练）

+   pip install diffusers transformers accelerate peft huggingface_hub 适用于建模和训练相关的所有事物

+   pip install datasets decord 用于加载视频训练数据

+   pip install bitsandbytes 用于使用 8 位 Adam 或 AdamW 优化器进行内存优化训练

+   pip install wandb （可选）用于监控训练日志

+   pip insta[ll deepsp](https://github.com/microsoft/DeepSpeed)eed （可选） 用于 DeepSpeed 训练

+   pip install prodigyopt （可选）如果您想使用 Prodigy 优化器进行训练

> 建议从源代码安装并保持安装最新， 请在新的虚拟环境测试我们的训练代码

在运行脚本之前，请确保从源安装库：

```
git clone https://github.com/huggingface/diffusers
cd diffusers
pip install -e .
```

然后导航到包含训练脚本的 example 文件夹，并安装您正在使用的脚所需的依赖项：

```
cd examples/cogvideo
pip install -r requirements.txt
```

确保按照[数据准备](https://zhipu-ai.feishu.cn/wiki/XWdgwnzu2iHXJmkLv4wc6vRXnyg#DsFBdhmOPo0AAnx78SqciOs9nXc)中所述准备数据。准备好后，您就可以开始训练了！

假设您正在训练 50 个类似概念的视频，我们发现 1500-2000 个步骤效果很好。但是，官方建议是 100 个视频，总共 4000 个步骤。假设您正在 --train_batch_size 为 1 的单个 GPU 上进行训练

+   50 个视频上的 1500 个步骤对应于 30 个num_train_epochs

+   100 个视频上的 4000 个步骤对应于 40 个num_train_epochs

```
#!/bin/bash

GPU_IDS=&quot;0&quot;

accelerate launch --gpu_ids $GPU_IDS examples/cogvideo/train_cogvideox_lora.py \
  --pretrained_model_name_or_path THUDM/CogVideoX-2b \
  --cache_dir &lt;CACHE_DIR&gt; \
  --instance_data_root &lt;PATH_TO_WHERE_VIDEO_FILES_ARE_STORED&gt; \
  --dataset_name my-awesome-name/my-awesome-dataset \
  --caption_column &lt;CAPTION_COLUMN&gt; \
  --video_column &lt;PATH_TO_VIDEO_COLUMN&gt; \
  --id_token &lt;ID_TOKEN&gt; \
  --validation_prompt &quot;&lt;ID_TOKEN&gt; Spiderman swinging over buildings:::A panda, dressed in a small, red jacket and a tiny hat, sits on a wooden stool in a serene bamboo forest. The panda&#x27;s fluffy paws strum a miniature acoustic guitar, producing soft, melodic tunes. Nearby, a few other pandas gather, watching curiously and some clapping in rhythm. Sunlight filters through the tall bamboo, casting a gentle glow on the scene. The panda&#x27;s face is expressive, showing concentration and joy as it plays. The background includes a small, flowing stream and vibrant green foliage, enhancing the peaceful and magical atmosphere of this unique musical performance&quot; \
  --validation_prompt_separator ::: \
  --num_validation_videos 1 \
  --validation_epochs 10 \
  --seed 42 \
  --rank 64 \
  --lora_alpha 64 \
  --mixed_precision fp16 \
  --output_dir /raid/aryan/cogvideox-lora \
  --height 480 --width 720 --fps 8 --max_num_frames 49 --skip_frames_start 0 --skip_frames_end 0 \
  --train_batch_size 1 \
  --num_train_epochs 30 \
  --checkpointing_steps 1000 \
  --gradient_accumulation_steps 1 \
  --learning_rate 1e-3 \
  --lr_scheduler cosine_with_restarts \
  --lr_warmup_steps 200 \
  --lr_num_cycles 1 \
  --enable_slicing \
  --enable_tiling \
  --optimizer Adam \
  --adam_beta1 0.9 \
  --adam_beta2 0.95 \
  --max_grad_norm 1.0 \
  --report_to wandb
```

为了更好地跟踪我们的训练实验，我们在上面的命令中使用以下标志：

+   --report_to wandb 将确保在 Weights 和 Biases 上跟踪训练运行。要使用它，请务必使用 pip install wandb 安装 wandb。

+   validation_prompt 和 validation_epochs 来允许脚本执行一些验证推理运行。这使我们能够定性地检查培训是否按预期进行。

| **Attribute** | **CogVideoX-2B** | **CogVideoX-5B** |
| --- | --- | --- |
| **Model Name** | CogVideoX-2B | CogVideoX-5B |
| Inference Precision | FP16* (Recommended), BF16, FP32, FP8*, INT8, Not supported INT4 | BF16 (Recommended), FP16, FP32, FP8*, INT8, Not supported INT4 |
| Single GPU Inference VRAM | FP16: Using diffusers 12.5GB* INT8: Using diffusers with torchao 7.8GB* | BF16: Using diffusers 20.7GB* INT8: Using diffusers with torchao 11.4GB* |
| Multi GPU Inference VRAM | FP16: Using diffusers 10GB* | BF16: Using diffusers 15GB* |
| **Inference Speed** | Single A100: ~90 seconds, Single H100: ~45 seconds | Single A100: ~180 seconds, Single H100: ~90 seconds |
| Fine-tuning Precision | FP16 | BF16 |
| Fine-tuning VRAM Consumption | 47 GB (bs=1, LORA) 61 GB (bs=2, LORA) 62GB (bs=1, SFT) | 63 GB (bs=1, LORA) 80 GB (bs=2, LORA) 75GB (bs=1, SFT) |

#### 更多

更多训练说明请查看[diffusers官方文档](https://huggingface.co/docs/diffusers/index)