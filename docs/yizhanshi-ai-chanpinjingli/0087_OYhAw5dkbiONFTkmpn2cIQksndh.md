
# Diffusers框架量化

> 来源：[https://v11enp9ok1h.feishu.cn/docx/SGDqdPef8oYmqXxu6lxcv1hfnGe](https://v11enp9ok1h.feishu.cn/docx/SGDqdPef8oYmqXxu6lxcv1hfnGe)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

`Diffuser`作为一个优秀的推理和训练的框架，涵盖了所有主流模型，这小节我们介绍`BitsAndBytes`库在`Diffusers`的能力

该库包括用于 8 位和 4 位操作的量化封装，通过`bitsandbytes.nn.Linear8bitLt`和`bitsandbytes.nn.Linear4bit`以及通过`bitsandbytes.optim`模块的 8 位优化器。

通过`transformers`的`.from_pretrained()`方法中提供的`quantization_config`，设置`load_in_8bit`参数。

## 量化方式

### int8

+`BitsAndBytesConfig`

`llm_int8_skip_modules`这个参数主要用于跳过一些不需要量化的模块，例如`embedding`

```
from diffusers import CogVideoXTransformer3DModel, BitsAndBytesConfig
import torch


quantization_config = BitsAndBytesConfig(load_in_8bit=True, 
                                         llm_int8_skip_modules=["time_embedding", "patch_embed.proj", "patch_embed", "norm_final", "norm_out", "proj_out"])
```

+`CogVideoXTransformer3DModel`

`save_pretrained`方法可以保存量化后的文件到指定目录

```
transformer = CogVideoXTransformer3DModel.from_pretrained("/mnt/ceph/develop/jiawei/model_checkpoint/CogVideoX-5b-I2V",
                                                          subfolder="transformer",
                                                          quantization_config=quantization_config,
                                                          torch_dtype=torch.bfloat16)
transformer.save_pretrained("CogVideoX-5b-I2V-int8-1")
```

### nf4

+`BitsAndBytesConfig`

`llm_int8_skip_modules`这个参数主要用于跳过一些不需要量化的模块，例如`embedding`

```
from diffusers import CogVideoXTransformer3DModel, BitsAndBytesConfig
import torch


quantization_config = BitsAndBytesConfig(load_in_4bit=True,
                                         bnb_4bit_compute_dtype=torch.bfloat16,
                                         bnb_4bit_quant_type="nf4", 
                                         llm_int8_skip_modules=["time_embedding", "patch_embed.proj", "patch_embed", "norm_final", "norm_out", "proj_out"]
                                         )
```

+`CogVideoXTransformer3DModel`

`save_pretrained`方法可以保存量化后的文件到指定目录

```
transformer = CogVideoXTransformer3DModel.from_pretrained("/mnt/ceph/develop/jiawei/model_checkpoint/CogVideoX-5b-I2V",
                                                          subfolder="transformer",
                                                          quantization_config=quantization_config,
                                                          torch_dtype=torch.bfloat16)
transformer.save_pretrained("CogVideoX-5b-I2V-nf4")
```

## 加载量化文件

```
transformer = CogVideoXTransformer3DModel.from_pretrained("/mnt/ceph/develop/jiawei/diffusers_fork_zmf/examples/cogvideo",
                                                        subfolder="CogVideoX-5b-I2V-int8-1",
                                                        torch_dtype=torch.bfloat16)
pipe = CogVideoXImageToVideoPipeline.from_pretrained("/mnt/ceph/develop/jiawei/model_checkpoint/CogVideoX-5b-I2V", 
                                        torch_dtype=torch.bfloat16, 
                                        transformer=transformer).to("cuda")
```
