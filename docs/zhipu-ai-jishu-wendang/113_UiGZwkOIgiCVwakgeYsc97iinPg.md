# CogVideoX-Factory 低成本微调

> 来源：[https://ai.feishu.cn/docx/M6XwdkYelozjEHxukiJcsrJvnQd](https://ai.feishu.cn/docx/M6XwdkYelozjEHxukiJcsrJvnQd)

# 项目介绍

在 24GB GPU 内存下对 Cog 系列视频模型进行微调以实现自定义视频生成。相对于官方版本：

+   支持多种分辨率微调

+   支持多种视频长度微调

+   支持预处理数据集，T5 与 VAE Encoder 允许卸载。

+   全程使用 diffusers库进行微调。

+   单卡RTX3090 / RTX 4090 即可微调

# 下载框架和模型

CogVideoX-Factory 支持低成本微调 CogVideoX模型

# 准备数据

这里以迪士尼数据集作为示例：

数据集地址：https://huggingface.co/datasets/Wild-Heart/Disney-VideoGeneration-Dataset

你可以通过以下命令下载数据集

```
HF_ENDPOINT=https://hf-mirror.com  huggingface-cli download \
  --repo-type dataset Wild-Heart/Disney-VideoGeneration-Dataset \
  --local-dir video-dataset-disney
```

数据集包含69条迪士尼数据，分辨率 720 * 480， 帧率30，结构如下

```
├── prompts.txt
├── videos
│   ├── 0288f3d69c08e816d81b014da620db49.mp4
│   ├── 05a234b0164d015d468f2f53e771b4cf.mp4
│   ├── 05ccfa61ece031e881d173289761cf91.mp4
│   ├── 0bb5f6dbf8ed2e0060f0ac4164b24847.mp4
│   ├── 1094139d474e65852826d64a1b4aa520.mp4
│   ├── 1151c01bd77450dfc603a2eb7352822e.mp4
│   ├── 11982a10fc2ba10d295d23054f7f68c8.mp4
│   ├── 12e51adf1acbf7acbb703a96a464a39b.mp4
│   ├── 1d50a3d9703f152758d5422c8b48010f.mp4
│   ├── 2325e5f8e287753e50e47ab2fc2e8241.mp4
│   ├── 2c1ed5408882479b06681f7cf372916a.mp4
│   ├── 3108dd567bd8669967bc83e0bc50dab2.mp4
│   ├── 328fc12cf9cf3d540e67efadeb893f61.mp4
│   ├── 349059f6c41460df59778d5e6d3d4861.mp4
│   ├── 383cb4b496d17695554655f3ec79c587.mp4
│   ├── 38963e6acf01fdcc9b647ec9d63edc9a.mp4
│   ├── 3f0979e6cae25447f416372c49ad5e07.mp4
│   ├── 4010c114ce5fb6a86d2327e711669813.mp4
│   ├── 4461a0e8ddd86d615ccf73ef0ff0705b.mp4
│   ├── 46e9d133d051655c956c7089b672f519.mp4
│   ├── 46f4eee0864dd89c9225367d826a657f.mp4
│   ├── 485b43aa4524327f3c7a40d28e1cf7bc.mp4
│   ├── 4adbb3a2945c9edd78785daccfd23e80.mp4
│   ├── 4c918b917308ff03120e9e86650a2d3c.mp4
│   ├── 5383e4e3e8156787c43bc0cd839536c0.mp4
│   ├── 560c6472660330638c2809d823d59be3.mp4
│   ├── 58b88d44575e945cd7dcd11b3aac6ff0.mp4
│   ├── 5a0229ffdb3bd9d8e81dca7988d7cdbb.mp4
│   ├── 6049124903a8de3fa164bbb5bb2f586e.mp4
│   ├── 614cf13ae1974436cf4072a5cc7d7c57.mp4
│   ├── 6619a387964ad49eb1127079d25d17d8.mp4
│   ├── 6781b101e8bd362e9853749fcc2cd3e3.mp4
│   ├── 72306312252707a5ecf83a8eeed9b55a.mp4
│   ├── 7d6dcf13f5c3d45b85c5ea0544c429e4.mp4
│   ├── 7fe0c83572de828da1cab0c118dece14.mp4
│   ├── 81c5dab878d73e6c21181d18d83f2808.mp4
│   ├── 82219ab81b21299db9848ab0d55e2f82.mp4
│   ├── 8520a081252604e3c478f76f4dddddf8.mp4
│   ├── 894905168d8da4f4b0862e7b0f756e44.mp4
│   ├── 8adfde998361b1d7c6f38a35481667fd.mp4
│   ├── 8ae679ab483ab344c881d4a813e0cb51.mp4
│   ├── 8d616fee8e0a280d2d87e478b948a729.mp4
│   ├── 8e7722634784cf969c15f4a597f3af4d.mp4
│   ├── 907704ed979d5e5651fac98ccbec2d4b.mp4
│   ├── 93877aefdefcee7b2978d7fb90690c8f.mp4
│   ├── 96d342ea7c7cfddbe1106072bc34be5a.mp4
│   ├── a3c275fc2eb0a67168a7c58a6a9adb14.mp4
│   ├── a8369dff8c96925513892c6377bacb1c.mp4
│   ├── b3e3b147675a68de8a29ffa762d59adb.mp4
│   ├── b65565e3b61c97b3fa0643c6f45dc4de.mp4
│   ├── bbf1174e69f404009bba630332c8cbdb.mp4
│   ├── be20f3db31cfe8140f004e34a75d6ba4.mp4
│   ├── bf06573576ae0ea4d27a178b4d6e95a1.mp4
│   ├── c4903e10393ef7c42fc8131f05f3f233.mp4
│   ├── c789b44119c14c5732e835cae89bf7ac.mp4
│   ├── cb227eefdfcbcd3db945131b1d153273.mp4
│   ├── ceb2998f50feefef1844032e9cd19739.mp4
│   ├── d2cfac77af77bba35a6986beca2ae052.mp4
│   ├── d3af95bf8f9cce29e5c99e839e630c59.mp4
│   ├── d3d4ce6894178d7664effcc4eef1945b.mp4
│   ├── d8a33062ef6a446c168acb06bf45b77d.mp4
│   ├── e6a0293632a942712fd61867aeae5e9f.mp4
│   ├── ec80a7b9cfb3740e5cdf9300fdc8d8a9.mp4
│   ├── f0f570e5bc69d03af276a06a96cde800.mp4
│   ├── f6a95cd4397b6a102e82becdcd05d585.mp4
│   ├── fca1d53370805cf052bed822e2d20604.mp4
│   ├── fe0efaedf8c47812bff8da9951f77975.mp4
│   ├── fe1009b64da05dfea77928e501284868.mp4
│   └── fec5a5184b05acafc7904cd419cbb5a3.mp4
└── videos.txt

```

[如果你想自行标注数据，欢迎使用](https://huggingface.co/THUDM/cogvlm2-llama3-caption) CogVLM2-Caption 标注数据集。并整理为上述格式。

其中，videos.txt 包含每个视频的绝对路径，提示词存放在prompts.txt中，一行一个提示词，与videos.txt 的每行进行对应。

# 数据预处理

## 为什么需要数据预处理

+   对于显存较少的GPU，比如 RTX4090，直接使用 Disney-VideoGeneration-Dataset 视频数据集，会导致 VRAM 较小的 GPU 出现内存不足 (OOM) 问题，因为它需要加载 VAE （将视频编码到潜在空间）和大型T5-XXL文本编码器。

+   预处理数据集之后，dataloader可以离线载入数据，加速训练速度和提升显卡利用率。

## 如何进行数据预处理

所以，为了减少内存使用量，需先使用training/prepare_dataset.py脚本预先计算潜在和嵌入。

寻找到项目中的 prepare_dataset.sh ，修改部分参数

```
MODEL_ID=&quot;THUDM/CogVideoX-2b&quot; # 需要调整为你的模型名称和绝对路径
NUM_GPUS=8 # 需要调整为你的GPU数量，本脚本使用多卡并行策略。
DATA_ROOT=&quot;/path/to/my/datasets/video-dataset&quot;  # 修改成你的数据集的绝对路径
CAPTION_COLUMN=&quot;prompt.txt&quot; #如果使用 Disney-VideoGeneration-Datase 数据集，需要修改为 prompts.txt
VIDEO_COLUMN=&quot;videos.txt&quot; # 无需个更改
OUTPUT_DIR=&quot;/path/to/my/datasets/preprocessed-dataset&quot; # 输出的数据集绝对路径，这个是一会训练时使用的数据集路径。
HEIGHT_BUCKETS=&quot;480 720&quot; # 目标数据集的分辨率高缩放。
WIDTH_BUCKETS=&quot;720 960&quot; # 目标数据集分辨率宽缩放。
FRAME_BUCKETS=&quot;49&quot; # 目标数据集的帧率，为 帧率 *K + 1 的格式，K 为秒，整数，例如6秒。帧率为8帧 /秒 ，则该参数为49
MAX_NUM_FRAMES=&quot;49&quot; 
MAX_SEQUENCE_LENGTH=226 # 不建议修改，支持的最长文本输入，建议使用 CogVLM-Caption 完成数据打标
TARGET_FPS=8 # 帧率，可以修改，同步改动 FRAME_BUCKETS 和 MAX_NUM_FRAMES
BATCH_SIZE=1 # 转换中的Batch_Size, 在RTX 4090 下建议设置为1。
```

接着，运行：

```
 bash prepare_dataset.sh
```

这会消耗较短的时间。

# 微调脚本调整（LORA）

## 文生视频模型

LORA脚本支持 I2V 和 T2V 模型。在开始微调之前，我们要对脚本进行修改。

这里以 T2V 模型为例：

```
DATA_ROOT=&quot;/path/to/my/datasets/disney-dataset&quot; #预处理后数据集路径
CAPTION_COLUMN=&quot;prompts.txt&quot; # 提示词路径
VIDEO_COLUMN=&quot;videos.txt&quot; # 视频路径
MODEL_PATH=&quot;THUDM/CogVideoX-5b&quot; # 模型绝对路径

cmd=&quot;accelerate launch --config_file $ACCELERATE_CONFIG_FILE --gpu_ids $GPU_IDS training/cogvideox_text_to_video_lora.py \
  --pretrained_model_name_or_path $MODEL_PATH \
  --data_root $DATA_ROOT \
  --caption_column $CAPTION_COLUMN \ # Encode 后数据集位置
  --video_column $VIDEO_COLUMN \ # Encode 后数据集位置
  --id_token BW_STYLE \  # 无需修改
  --height_buckets 480 \ # 视频高
  --width_buckets 720 \ # 视频宽
  --frame_buckets 49 \ # 视频帧率
  --dataloader_num_workers 8 \  # num_workers
  --pin_memory \ # 默认
  --validation_prompt \&quot;BW_STYLE A black and white animated scene unfolds with an anthropomorphic goat surrounded by musical notes and symbols, suggesting a playful environment. Mickey Mouse appears, leaning forward in curiosity as the goat remains still. The goat then engages with Mickey, who bends down to converse or react. The dynamics shift as Mickey grabs the goat, potentially in surprise or playfulness, amidst a minimalistic background. The scene captures the evolving relationship between the two characters in a whimsical, animated setting, emphasizing their interactions and emotions:::BW_STYLE A panda, dressed in a small, red jacket and a tiny hat, sits on a wooden stool in a serene bamboo forest. The panda&#x27;s fluffy paws strum a miniature acoustic guitar, producing soft, melodic tunes. Nearby, a few other pandas gather, watching curiously and some clapping in rhythm. Sunlight filters through the tall bamboo, casting a gentle glow on the scene. The panda&#x27;s face is expressive, showing concentration and joy as it plays. The background includes a small, flowing stream and vibrant green foliage, enhancing the peaceful and magical atmosphere of this unique musical performance\&quot; \
  --validation_prompt_separator ::: \ #分割符
  --num_validation_videos 1 \  #验证集视频数
  --validation_epochs 10 \ # 10 轮验证一次
  --seed 42 \ # 训练和验证时候的种子
  --rank 128 \ # Lora Rank
  --lora_alpha 128 \ # Lora_Alpha
  --mixed_precision bf16 \ # 混合精度训练
  --output_dir $output_dir \ #输出地址
  --max_num_frames 49 \ #帧率
  --train_batch_size 1 \ # 对于 RTX4090，只能为1
  --max_train_steps $steps \ #最大训练数
  --checkpointing_steps 1000 \ # 保存点
  --gradient_accumulation_steps 1 \ # 梯度累计
  --gradient_checkpointing \ # 默认需要打开
  --learning_rate $learning_rate \ # 学习率
  --lr_scheduler $lr_schedule \ # 默认使用 ADAM
  --lr_warmup_steps 400 \ 
  --lr_num_cycles 1 \
  --enable_slicing \ #  对于RTX4090 必须开启
  --enable_tiling \ #  对于RTX4090 必须开启
  --enable_model_cpu_offload \ # 对于RTX4090 必须开启
  --load_tensors \ 载入预处理数据集， 对于RTX4090 必须开启
  --optimizer $optimizer \
  --beta1 0.9 \ 
  --beta2 0.95 \
  --weight_decay 0.001 \
  --max_grad_norm 1.0 \
  --allow_tf32 \
  --report_to wandb \  #可视化工具
  --nccl_timeout 1800&quot;
```

修改完脚本之后，我们运行微调脚本：

```
bash train_text_to_video_lora.sh
```

接着，你就能看到正常运行训练的截图

```
Running command: accelerate launch --config_file accelerate_configs/uncompiled_1.yaml --gpu_ids 0 training/cogvideox_text_to_video_lora.py           --pretrained_model_name_or_path THUDM/CogVideoX-5b           --data_root /root/disney_cogvideox_preprocessed-dataset           --caption_column prompts.txt           --video_column videos.txt           --id_token BW_STYLE           --height_buckets 480           --width_buckets 720           --frame_buckets 49           --dataloader_num_workers 8           --pin_memory           --validation_prompt &quot;BW_STYLE A black and white animated scene unfolds with an anthropomorphic goat surrounded by musical notes and symbols, suggesting a playful environment. Mickey Mouse appears, leaning forward in curiosity as the goat remains still. The goat then engages with Mickey, who bends down to converse or react. The dynamics shift as Mickey grabs the goat, potentially in surprise or playfulness, amidst a minimalistic background. The scene captures the evolving relationship between the two characters in a whimsical, animated setting, emphasizing their interactions and emotions:::BW_STYLE A panda, dressed in a small, red jacket and a tiny hat, sits on a wooden stool in a serene bamboo forest. The panda&#x27;s fluffy paws strum a miniature acoustic guitar, producing soft, melodic tunes. Nearby, a few other pandas gather, watching curiously and some clapping in rhythm. Sunlight filters through the tall bamboo, casting a gentle glow on the scene. The panda&#x27;s face is expressive, showing concentration and joy as it plays. The background includes a small, flowing stream and vibrant green foliage, enhancing the peaceful and magical atmosphere of this unique musical performance&quot;           --validation_prompt_separator :::           --num_validation_videos 1           --validation_epochs 10           --seed 42           --rank 128           --lora_alpha 128           --mixed_precision bf16           --output_dir ./cogvideox-lora__optimizer_adam__steps_3000__lr-schedule_cosine_with_restarts__learning-rate_1e-4/           --max_num_frames 49           --train_batch_size 1           --load_tensors           --max_train_steps 3000           --checkpointing_steps 1000           --gradient_accumulation_steps 1           --gradient_checkpointing           --learning_rate 1e-4           --lr_scheduler cosine_with_restarts           --lr_warmup_steps 400           --lr_num_cycles 1         --enable_model_cpu_offload           --enable_slicing           --enable_tiling           --optimizer adam           --beta1 0.9           --beta2 0.95           --weight_decay 0.001           --max_grad_norm 1.0           --allow_tf32           --report_to wandb           --nccl_timeout 1800
10/20/2024 15:57:27 - INFO - __main__ - Distributed environment: NO
Num processes: 1
Process index: 0
Local process index: 0
Device: cuda

Mixed precision type: bf16

You set `add_prefix_space`. The tokenizer needs to be converted from the slow tokenizers
Loading checkpoint shards: 100%|██████████| 2/2 [00:10&lt;00:00,  5.40s/it]
wandb: Using wandb-core as the SDK backend. Please refer to https://wandb.me/wandb-core for more information.
wandb: Tracking run with wandb version 0.18.3
wandb: W&amp;B syncing is set to `offline` in this directory.  
wandb: Run `wandb online` or set WANDB_MODE=online to enable cloud syncing.
===== Memory before training =====
memory_allocated=19.906 GB
max_memory_allocated=19.906 GB
max_memory_reserved=20.006 GB
***** Running training *****
  Num trainable parameters = 132120576
  Num examples = 13
  Num batches each epoch = 13
  Num epochs = 231
  Instantaneous batch size per device = 1
  Total train batch size (w. parallel, distributed &amp; accumulation) = 1
  Gradient accumulation steps = 1
  Total optimization steps = 3000
Steps:   2%|▏         | 59/3000 [11:21&lt;9:24:27, 11.52s/it, gradient_norm_after_clip=0.00647, gradient_norm_before_clip=0.00647, loss=0.174, lr=1.47e-5]
```

如果你使用单张RTX4090，对于本数据集，通常需要10个小时才能完成3000 steps的Lora 训练。

因此，你可以使用nohup进行管理：

```
nohup bash train_text_to_video_lora.sh &gt; $(date +&quot;%Y-%m-%d_%H-%M-%S&quot;)_cogvideox_lora.log 2&gt;&amp;1 &amp;
```

这样，你就可以后台运行了。

## 图生视频模型

图生视频的脚本与文生视频基本相同，主要区分点

+   需要使用CogVideoX-5B-I2V作为基座模型。

+   脚本会自动将数据集中所有视频第一帧设置为图像传入。

+   对于验证集，你需要单独准备一张图像作为验证。

其他部分的修改与文生视频相同。

## 模型使用

训练完成后，需先将 LoRA 保存。默认情况下，模型保存结果文件名为:

```
cogvideox-lora__optimizer_adam__steps_3000__lr-schedule_cosine_with_restarts__learning-rate_1e-4
```

其中，会包含训练的步数，学习率，学习策略等信息。进入文件夹，可以查看 adapter 模型权重。

在这里，我们提供了一个简单的脚本，用于部署微调模型的推理。

```
import torch
from diffusers import CogVideoXPipeline
from diffusers.utils import export_to_video

prompt = &quot;A black and white animated scene unfolds with an anthropomorphic goat surrounded by musical notes and symbols, suggesting a playful environment. Mickey Mouse appears, leaning forward in curiosity as the goat remains still. The goat then engages with Mickey, who bends down to converse or react. The dynamics shift as Mickey grabs the goat, potentially in surprise or playfulness, amidst a minimalistic background. The scene captures the evolving relationship between the two characters in a whimsical, animated setting, emphasizing their interactions and emotions&quot;
pipe = CogVideoXPipeline.from_pretrained(&quot;THUDM/CogVideoX-5b&quot;, torch_dtype=torch.bfloat16)
pipe.load_lora_weights(&quot;lora_1e_4/checkpoint-3000&quot;, weight_name=&quot;pytorch_lora_weights.safetensors&quot;,
                       adapter_name=&quot;cogvideox-lora&quot;)
pipe.set_adapters([&quot;cogvideox-lora&quot;], [1.0])
pipe.enable_sequential_cpu_offload()
pipe.vae.enable_slicing()
pipe.vae.enable_tiling()
video = pipe(
    prompt=prompt
).frames[0]
export_to_video(video, &quot;output.mp4&quot;, fps=8)
```