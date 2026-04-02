# CogVideoX transformer超参数

> 来源：[https://ai.feishu.cn/docx/YbKwd7DEPonwybx2EIJcRZPInLu](https://ai.feishu.cn/docx/YbKwd7DEPonwybx2EIJcRZPInLu)

#### Adam params

根据实现结果，关于Adam超惨总结有下面几个特点，按照梯度训练的规则，eps和weight_decay应该根据lr的大小动态调整

lr 在1E-3 时候 ，eps 是1E-8 ,weight_decay是 1E-4,

lr 在5E-4 时候 ，eps 是5E-8 ,weight_decay是 5E-6

1E-3： 更加适合新任务学习，在我们没有存在的能力上微调，可以获得更好的效果

5E-4：更适合细节学习，比如物体精细程度，人物穿戴，画面风格等任务

```


  optimizer:
    type: sat.ops.FusedEmaAdam
    params:
      lr: 5E-4 # Between 1E-3 and 5E-4 For Lora and 1E-5 For SFT
      betas: [ 0.9, 0.95 ]
      eps: 5E-8
      weight_decay: 5E-6
```

#### shift_scale

基于公开数据

2b：shift_scale=3.0

5b：shift_scale=1.0

但这个数字经过实验看来，往往跟timesteps有关，在cogvideo中scheduler的参数用于噪声叠加的推理，

在文章2.2 Noisy image spectra  提到了一点扩散模型是如何添加高斯噪声的，基于傅里叶变化的基本思想，频率是空间上一个物体瞬时状态的截面，傅里叶用它的公式证明了这一点，在这个想法下，我没可以认为加噪声的过程是在不同的时间点增加一个形变的程度，从而我们在更高维度（旁观）到低维投射的过程，这个过程被称为频率变换（傅里叶变换），总之傅里叶变换是一个观察过程，辅助我们认识数据变化过程的

而高斯噪声是一个**同等程度的加噪（损坏）过程， **那么是否控制timesteps加噪[的位置（](https://kexue.fm/archives/9181)时间点）就可以调整图片，这里基于微分方程的一些比较专业的猜测也许有助于你理解采样率（cfg）、迭代步数（steps）、采样步数（T）d之间的关系

```
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
```

#### text_length&max_length

这个参数是输入长度，采用了T5的token计算方法，用来限定输入内容的，理论上跟显存大小成正比

2B和5B模型公开的参数为 226

```

      modules:
        pos_embed_config:
          target: dit_video_concat.Basic3DPositionEmbeddingMixin
          params:
            text_length: 2048
            height_interpolation: 1.875
            width_interpolation: 1.875
  conditioner_config:
    target: sgm.modules.GeneralConditioner
    params:
      emb_models:
        - is_trainable: false
          input_key: txt
          ucg_rate: 0.1
          target: sgm.modules.encoders.modules.FrozenT5Embedder
          params:
            model_dir: &quot;/mnt/ceph/develop/jiawei/model_checkpoint/t5-v1_1-xxl&quot;
            max_length: 2048
```

#### lora_r

Lora的秩

2B是128

5B是256

```

        lora_config:
            target: sat.model.finetune.lora2.LoraMixin
            params:
                r: 128
```

#### video_size

H,W,参数 默认为[ 480, 720 ] ，调整它也许你需要修改vae的代码

```
data:
  target: data_video.SFTDataset
  params:
    video_size: [ 480, 720 ]
    fps: 8
    max_num_frames: 49
    skip_frms_num: 3.
```

#### loss_fn_config

```
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
```