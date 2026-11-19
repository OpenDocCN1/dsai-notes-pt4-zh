

# 用 SAT 进行模型微调

> 来源：[https://v11enp9ok1h.feishu.cn/docx/S4u0dOCGMoxjdPx0uJocEdyDnNg](https://v11enp9ok1h.feishu.cn/docx/S4u0dOCGMoxjdPx0uJocEdyDnNg)

> 该方案支持 `CogVideoX-2B`、`CogVideoX-5B` Text2Videos 微调。

在开始微调前，确保已完成模型推理——用 SAT 进行视频生成推理中下载和 `transformers`、`VAE` 和 `T5` 模型权重的部分。

## 准备数据集

按照以下结构来组织数据集。每个 `.txt` 文件应对应一个视频，并包含该视频的标签。每个 `.mp4` 文件应对应一个标签。确保每个视频与标签一一对应。做风格方面的微调，至少要准备 50 个风格相似的视频和标签。

```
.
├── labels
│   ├── 1.txt
│   ├── 2.txt
│   ├── ...
└── videos
    ├── 1.mp4
    ├── 2.mp4
    ├── ...
```

## 准备环境

由于 `pip` 软件包中尚未提供所需的功能，因此需要从源代码安装 `SwissArmyTransformer` (SAT) 以进行适当的微调。

```
git clone https://github.com/THUDM/SwissArmyTransformer.git
cd SwissArmyTransformer
pip install -e .
```

## 安装依赖

```
pip install -r requirements.txt
```

## 下载模型权重

首先，前往 `SAT` 镜像下载模型权重，然后到清华云盘下载模型并解压。

### 2B模型下载

```
mkdir CogVideoX-2b-sat
cd CogVideoX-2b-sat
wget https://cloud.tsinghua.edu.cn/f/fdba7608a49c463ba754/?dl=1
mv 'index.html?dl=1' vae.zip
unzip vae.zip
wget https://cloud.tsinghua.edu.cn/f/556a3e1329e74f1bac45/?dl=1
mv 'index.html?dl=1' transformer.zip
unzip transformer.zip
```

### 5B模型下载

对于 `CogVideoX-5B` 模型，请按照如下方式下载(`VAE`文件相同):

```
mkdir CogVideoX-5b-sat
cd CogVideoX-5b-sat
wget https://cloud.tsinghua.edu.cn/f/fdba7608a49c463ba754/?dl=1
mv 'index.html?dl=1' vae.zip
unzip vae.zip
```

[`transformer权重`](https://cloud.tsinghua.edu.cn/d/fcef5b3904294a6885e5/?p=%2F&mode=list)，需要前往清华云盘下载我们的模型，并进行解压。

经过整理，两个模型的完整模型结构应如下所示：

```
.
├── transformer
│   ├── 1000 (or 1)
│   │   └── mp_rank_00_model_states.pt
│   └── latest
└── vae
    └── 3d-vae.pt
```

模型权重的文件比较大，推荐使用 `git lfs` 来下载，安装可参考[这里](https://github.com/git-lfs/git-lfs?tab=readme-ov-file#installing)。

## 克隆 T5 模型

下一步，克隆 `T5` 模型，虽然不用于训练和微调，但必须使用。`CogVideoX-2B` 和 `CogVideoX-5B` 的 `T5` 模型相同，不需要重复此步骤。您可以选择从 [Huggingface](https://huggingface.co/THUDM/CogVideoX-2b.git) 或 [Modelscope](https://modelscope.cn/models/ZhipuAI/CogVideoX-2b) 下载，并且将 `text_encoder` 和 `tokenizer` 放入 `t5-v1_1-xxl` 目录中。

```
git clone https://huggingface.co/THUDM/CogVideoX-2b.git
mkdir t5-v1_1-xxl
mv CogVideoX-2b/text_encoder/* CogVideoX-2b/tokenizer/* t5-v1_1-xxl
```

现在，我们已经获得 `safetensor` 格式的 `T5` 文件，确保在 `Finetune` 中将其加载到 `Deepspeed` 时没有错误。`t5-v1_1-xxl` 目录的内容如下：

```
├── added_tokens.json
├── config.json
├── model-00001-of-00002.safetensors
├── model-00002-of-00002.safetensors
├── model.safetensors.index.json
├── special_tokens_map.json
├── spiece.model
└── tokenizer_config.json

0 directories, 8 files
```

## 修改 YAML 文件

首先，找到 `configs/sft.yaml` 文件，把 `transformer` 路径换成本地绝对路径。然后把训练集和验证集也换成本地绝对路径，训练集和验证集可以是同一个文件。

```
args:
  checkpoint_activations: True ## using gradient checkpointing
  model_parallel_size: 1
  experiment_name: lora-disney
  mode: finetune
  load: "{absolute path to model}/cogvideox-2b-sat/transformer"
  no_load_rng: True
  train_iters: 1000 # Suggest more than 1000 For Lora and SFT For 500 is enough
  eval_iters: 1
  eval_interval: 100
  eval_batch_size: 1
  save: ckpts_2b_lora
  save_interval: 100
  log_interval: 20
  train_data: ["absolute path to your training data"] # Train data path
  valid_data: ["absolute path to your validation data"] # Validation data path, can be the same as train_data(not recommended)
  split: 1,0,0
  num_workers: 8
  force_train: True
  only_log_video_latents: True
```

在同一个文件中，确保数据类型 `bf16` 设定为 `False`，`fp16` 设定为 `True`。

```
  bf16:
      enabled: False  # For CogVideoX-2B Turn to False and For CogVideoX-5B Turn to True
  fp16:
      enabled: True  # For CogVideoX-2B Turn to True and For CogVideoX-5B Turn to False
```

下一步，找到 `configs/cogvideox_2b.yaml` 文件，将 `model_dir` 和 `ckpt_path` 改成本地的绝对路径

```
  conditioner_config:
    target: sgm.modules.GeneralConditioner
    params:
      emb_models:
        - is_trainable: false
          input_key: txt
          ucg_rate: 0.1
          target: sgm.modules.encoders.modules.FrozenT5Embedder
          params:
            model_dir: "{absolute_path/to/your/t5-v1_1-xxl}/t5-v1_1-xxl" # Absolute path to the CogVideoX-2b/t5-v1_1-xxl weights folder
            max_length: 226

  first_stage_config:
    target: vae_modules.autoencoder.VideoAutoencoderInferenceWrapper
    params:
      cp_size: 1
      ckpt_path: "{absolute_path/to/your/model}/CogVideoX-2b-sat/vae/3d-vae.pt" # Absolute path to the CogVideoX-2b-sat/vae/3d-vae.pt folder
      ignore_keys: [ 'loss' ]
```

## Lora 微调配置

如果希望使用 `Lora` 进行微调，找到 `configs/congvideox_2b_lora.yaml`，将 `not_trainable_prefixes: ['all']` 取消注释：

```
model:
  scale_factor: 1.15258426
  disable_first_stage_autocast: true
  not_trainable_prefixes: ['all'] ## Using Lora
  log_keys:
    - txt
```

然后，把 `T5` 模型和 `VAE` 模型换成本地绝对路径。

```
  conditioner_config:
    target: sgm.modules.GeneralConditioner
    params:
      emb_models:
        - is_trainable: false
          input_key: txt
          ucg_rate: 0.1
          target: sgm.modules.encoders.modules.FrozenT5Embedder
          params:
            model_dir: "{absolute path to your T5}/t5-v1_1-xxl"
            max_length: 226

  first_stage_config:
    target: vae_modules.autoencoder.VideoAutoencoderInferenceWrapper
    params:
      cp_size: 1
      ckpt_path: "{absolute path to your model}/cogvideox-2b-sat/vae/3d-vae.pt"
      ignore_keys: [ 'loss' ]

      loss_config:
        target: torch.nn.Identity
```

## 微调和验证

运行推理代码开始微调，您可以选择单 GPU 或多 GPU。

### 单GPU

```
bash finetune_single_gpu.sh
```

### 多GPU

```
bash finetune_multi_gpus.sh
```

## 使用微调模型

找到 `configs/inference.yaml`，把原先的 `transformer` 路径注释掉，然后取消第二行的注释并替换本地存微调结果的 `lora` 文件夹。这个文件夹的路径可以在微调时设定的 `configs/sft.yaml` 中的 `save` 和 `experiment_name` 找到，并且加上微调开始时的日期和时间，例如原始文件的 `lora` 文件夹路径是 `ckpts_2b_lora/lora-disney/{date-and-time}`。

```
args:
  latent_channels: 16
  mode: inference
  # load: "{absolute_path/to/your}/transformer" # Absolute path to the CogVideoX-2b-sat/transformer folder
  load: "{your lora folder} such as zRzRzRzRzRzRzR/lora-disney-08-20-13-28" # This is for Full model without lora adapter

  batch_size: 1
  input_type: txt # You can choose txt for pure text input, or change to cli for command line input
  input_file: configs/test.txt # Pure text file, which can be edited
  sampling_num_frames: 13  # Must be 13, 11 or 9
  sampling_fps: 8
  fp16: True # For CogVideoX-2B
  # bf16: True # For CogVideoX-5B
  output_dir: outputs/ 
  force_inference: True
```

## 转换为 Huggingface Diffusers 支持的权重

`SAT` 权重格式与 Huggingface 的权重格式不同。使用以下命令转换权重：

```
python ../tools/convert_weight_sat2hf.py
```

## 从SAT权重文件 导出Huggingface Diffusers lora权重

支持了从 `SAT` 权重文件

在经过上面这些步骤训练之后，我们得到了一个 `sat` 带 `lora` 的权重，在 `{args.save}/1000/1000/mp_rank_00_model_states.pt` 你可以看到这个文件

导出的 `lora` 权重脚本在 `CogVideoX` 仓库 `tools/export_sat_lora_weight.py`，导出后使用 `load_cogvideox_lora.py` 推理

+   导出命令

```
python tools/export_sat_lora_weight.py --sat_pt_path {args.save}/{experiment_name}-09-09-21-10/1000/mp_rank_00_model_states.pt --lora_save_directory   {args.save}/export_hf_lora_weights_1/
```

训练主要修改了下面几个模型结构，下面列出了 转换为 `HF` 格式的 `lora` 结构对应关系，可以看到 `lora` 将模型注意力结构上增加一个低秩权重，

```
'attention.query_key_value.matrix_A.0': 'attn1.to_q.lora_A.weight',
'attention.query_key_value.matrix_A.1': 'attn1.to_k.lora_A.weight',
'attention.query_key_value.matrix_A.2': 'attn1.to_v.lora_A.weight',
'attention.query_key_value.matrix_B.0': 'attn1.to_q.lora_B.weight',
'attention.query_key_value.matrix_B.1': 'attn1.to_k.lora_B.weight',
'attention.query_key_value.matrix_B.2': 'attn1.to_v.lora_B.weight',
'attention.dense.matrix_A.0': 'attn1.to_out.0.lora_A.weight',
'attention.dense.matrix_B.0': 'attn1.to_out.0.lora_B.weight'
```

通过 `export_sat_lora_weight.py` 将它转换为 `HF` 格式的 `lora` 结构

