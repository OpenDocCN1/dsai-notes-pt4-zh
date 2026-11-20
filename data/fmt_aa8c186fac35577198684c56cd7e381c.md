
# 用 SAT 进行视频生成推理

> 来源：[https://ai.feishu.cn/docx/Vy7IdPjJ5o9I1RxL1YJcCam5nsg](https://ai.feishu.cn/docx/Vy7IdPjJ5o9I1RxL1YJcCam5nsg)

## 安装依赖

```
pip install -r requirements.txt
```

## 下载模型权重

首先，前往 `SAT` 镜像下载模型权重，然后到清华云盘下载模型并解压。

```
mkdir CogVideoX-5b-sat
cd CogVideoX-5b-sat
wget https://cloud.tsinghua.edu.cn/f/fdba7608a49c463ba754/?dl=1
mv &#x27;index.html?dl=1&#x27; vae.zip
unzip vae.zip
wget https://cloud.tsinghua.edu.cn/f/556a3e1329e74f1bac45/?dl=1
mv &#x27;index.html?dl=1&#x27; transformer.zip
unzip transformer.zip
```

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

下一步，克隆 `T5` 模型，虽然不用于训练和微调，但必须使用。`CogVideoX-2B` 和 [CogVideoX-5B](https://huggingface.co/THUDM/CogVideoX-2b.git) 的 `T5` 模型相同，不需要重复此步骤。您可以选择从 [Huggingface](https://huggingface.co/) 或 [Modelscope](https://modelscope.cn/models/ZhipuAI/CogVideoX-2b) 下载，并且将 `text_encoder` 和 `tokenizer` 放入 `t5-v1_1-xxl` 目录中。

```
git clone https://huggingface.co/THUDM/CogVideoX-5b.git
mkdir t5-v1_1-xxl
mv CogVideoX-5b/text_encoder/* CogVideoX-5b/tokenizer/* t5-v1_1-xxl
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

### 修改 `configs/cogvideox_5b.yaml`

找到 `configs/cogvideox_5b.yaml` 文件，将 `model_dir` 和 `ckpt_path` 改成本地的绝对路径

```
model:
  scale_factor: 1.15258426
  disable_first_stage_autocast: true
  log_keys:
    - txt

  denoiser_config:
    target: sgm.modules.diffusionmodules.denoiser.DiscreteDenoiser
    params:
      num_idx: 1000
      quantize_c_noise: False

      weighting_config:
        target: sgm.modules.diffusionmodules.denoiser_weighting.EpsWeighting
      scaling_config:
        target: sgm.modules.diffusionmodules.denoiser_scaling.VideoScaling
      discretization_config:
        target: sgm.modules.diffusionmodules.discretizer.ZeroSNRDDPMDiscretization
        params:
          shift_scale: 3.0

  network_config:
    target: dit_video_concat.DiffusionTransformer
    params:
      time_embed_dim: 512
      elementwise_affine: True
      num_frames: 49
      time_compressed_rate: 4
      latent_width: 90
      latent_height: 60
      num_layers: 30
      patch_size: 2
      in_channels: 16
      out_channels: 16
      hidden_size: 1920
      adm_in_channels: 256
      num_attention_heads: 30

      transformer_args:
        checkpoint_activations: True ## using gradient checkpointing
        vocab_size: 1
        max_sequence_length: 64
        layernorm_order: pre
        skip_init: false
        model_parallel_size: 1
        is_decoder: false

      modules:
        pos_embed_config:
          target: dit_video_concat.Basic3DPositionEmbeddingMixin
          params:
            text_length: 226
            height_interpolation: 1.875
            width_interpolation: 1.875

        patch_embed_config:
          target: dit_video_concat.ImagePatchEmbeddingMixin
          params:
            text_hidden_size: 4096

        adaln_layer_config:
          target: dit_video_concat.AdaLNMixin
          params:
            qk_ln: True

        final_layer_config:
          target: dit_video_concat.FinalLayerMixin

  conditioner_config:
    target: sgm.modules.GeneralConditioner
    params:
      emb_models:
        - is_trainable: false
          input_key: txt
          ucg_rate: 0.1
          target: sgm.modules.encoders.modules.FrozenT5Embedder
          params:
            model_dir: &quot;{absolute_path/to/your/t5-v1_1-xxl}/t5-v1_1-xxl&quot; # Absolute path to the CogVideoX-2b/t5-v1_1-xxl weights folder
            max_length: 226

  first_stage_config:
    target: vae_modules.autoencoder.VideoAutoencoderInferenceWrapper
    params:
      cp_size: 1
      ckpt_path: &quot;{absolute_path/to/your/model}/CogVideoX-5b-sat/vae/3d-vae.pt&quot; # Absolute path to the CogVideoX-2b-sat/vae/3d-vae.pt folder
      ignore_keys: [ &#x27;loss&#x27; ]

      loss_config:
        target: torch.nn.Identity

      regularizer_config:
        target: vae_modules.regularizers.DiagonalGaussianRegularizer

      encoder_config:
        target: vae_modules.cp_enc_dec.ContextParallelEncoder3D
        params:
          double_z: true
          z_channels: 16
          resolution: 256
          in_channels: 3
          out_ch: 3
          ch: 128
          ch_mult: [ 1, 2, 2, 4 ]
          attn_resolutions: [ ]
          num_res_blocks: 3
          dropout: 0.0
          gather_norm: True

      decoder_config:
        target: vae_modules.cp_enc_dec.ContextParallelDecoder3D
        params:
          double_z: True
          z_channels: 16
          resolution: 256
          in_channels: 3
          out_ch: 3
          ch: 128
          ch_mult: [ 1, 2, 2, 4 ]
          attn_resolutions: [ ]
          num_res_blocks: 3
          dropout: 0.0
          gather_norm: False

  loss_fn_config:
    target: sgm.modules.diffusionmodules.loss.VideoDiffusionLoss
    params:
      offset_noise_level: 0
      sigma_sampler_config:
        target: sgm.modules.diffusionmodules.sigma_sampling.DiscreteSampling
        params:
          uniform_sampling: True
          num_idx: 1000
          discretization_config:
            target: sgm.modules.diffusionmodules.discretizer.ZeroSNRDDPMDiscretization
            params:
              shift_scale: 3.0

  sampler_config:
    target: sgm.modules.diffusionmodules.sampling.VPSDEDPMPP2MSampler
    params:
      num_steps: 50
      verbose: True

      discretization_config:
        target: sgm.modules.diffusionmodules.discretizer.ZeroSNRDDPMDiscretization
        params:
          shift_scale: 3.0

      guider_config:
        target: sgm.modules.diffusionmodules.guiders.DynamicCFG
        params:
          scale: 6
          exp: 5
          num_steps: 50
```

### 修改 `configs/inference.yaml`

找到 `configs/inference.yaml` 文件：

1. 把 `transformer` 换成本地的绝对路径
2. 如果输入单条 `prompt` 把 `input_type` 改成 `cli`，用命令行输入。如果需要多条 `prompt`，把 `input_type` 改成 `txt`，并修改 `configs/test.txt`，每行为一个 `prompt`。
3. 设定 `bf16: True`，注释掉 `fp16: True`。
4. 把 `output_dir` 设定为想要存储输出的路径

```
args:
  latent_channels: 16
  mode: inference
  load: &quot;{absolute_path/to/your}/transformer&quot; # Absolute path to the CogVideoX-2b-sat/transformer folder
  # load: &quot;{your lora folder} such as zRzRzRzRzRzRzR/lora-disney-08-20-13-28&quot; # This is for Full model without lora adapter

  batch_size: 1
  input_type: txt # You can choose txt for pure text input, or change to cli for command line input
  input_file: configs/test.txt # Pure text file, which can be edited
  sampling_num_frames: 13  # Must be 13, 11 or 9
  sampling_fps: 8
  # fp16: True # For CogVideoX-2B
  bf16: True # For CogVideoX-5B
  output_dir: outputs/ 
  force_inference: True
```

## 运行推理

```
bash inference.sh
```
