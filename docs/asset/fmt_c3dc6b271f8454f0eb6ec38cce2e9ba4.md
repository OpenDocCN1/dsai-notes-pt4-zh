
# 使用ControlNet辅助生成图片

其中`Load Checkpoint`模块代表对`SD`模型的主要结构进行初始化（`VAE`，`U-Net`），`CLIP Text Encode`表示文本编码器，可以输入`prompt`和`negative prompt`，来控制图像的生成，`Load Image`表示输入的`ControlNet`需要的预处理图，`Empty Latent Image`表示初始化的高斯噪声，`Load ControlNet Model`表示对`ControlNet`进行初始化，`KSampler`表示调度算法以及`SD`相关生成参数，`VAE Decode`表示使用`VAE`的解码器将低维度的隐空间特征转换成像素空间的生成图像。

## 超分辨率重建

输入：`prompt`/（图像 + `prompt`）

输入：图像

![](img/5eefc6880454f416a7b1f50e53d3af50.png)

其中`Load Checkpoint`模块代表对`SD`模型的主要结构进行初始化（`VAE`，`U-Net`），`CLIP Text Encode`表示文本编码器，可以输入`prompt`和`negative prompt`，来控制图像的生成，`Empty Latent Image`表示初始化的高斯噪声，`Load Upscale Model`表示对超分辨率重建模型进行初始化，`KSampler`表示调度算法以及`SD`相关生成参数，`VAE Decode`表示使用`VAE`的解码器将低维度的隐空间特征转换成像素空间的生成图像，`Upscale Image`表示将生成的图片进行超分。

# Stable Diffusion训练过程

`Stable Diffusion`的整个训练过程在最高维度上可以看成是如何加噪声和如何去噪声的过程，并在针对噪声的“对抗与攻防”中学习到生成图片的能力。

具体地，在训练过程中，我们首先对干净样本进行加噪处理，采用多次逐步增加噪声的方式，直至干净样本转变成为纯噪声。

![](img/5eff34b102d075da48f59ced4f0fafd9.png)

SD训练时的加噪过程

接着，让`SD`模型学习去噪过程，最后抽象出一个高维函数，这个函数能在纯噪声中“优化”噪声，得到一个干净样本。

其中，将去噪过程具像化，就得到使用`U-Net`预测噪声，并结合调度算法逐步去噪的过程。

![](img/3c43b779bd7a49291b33497d42e7f080.png)

SD训练时的去噪过程

我们可以看到，加噪和去噪过程都是逐步进行的，我们假设进行步，那么每一步，`SD`都要去预测噪声，从而形成“小步快跑的稳定去噪”，类似于移动互联网时代的产品逻辑，这是足够伟大的关键一招。

与此同时，在加噪过程中，每次增加的噪声量级可以不同，假设有5种噪声量级，那么每次都可以取一种量级的噪声，增加噪声的多样性。

![](img/c0bd986402ff8f1ae409986776a69c11.png)

多量级噪声

那么怎么让网络知道目前处于的哪一步呢？这时就需要`Positional embeddings`了，通过位置编码，将步数也传入网络中，从而让网络知道现在处于哪一步，和`Transformer`中的操作类似：

![](img/54b914e69e14dda80e02c5b830e8aabc.png)

使用`Positional embeddings`对训练迭代的步数进行编码

# Stable Diffusion性能优化

## 使用TF32精度

```
import torch

torch.backends.cuda.matmul.allow_tf32 = True
```

`TF32`在性能和精度上实现了平衡。下面是`TF32`精度的一些作用和优势：

1. 加速训练速度：使用`TF32`精度可以在保持相对较高的模型精度的同时，加快模型训练的速度。

1. 减少内存需求：`TF32`精度相对于传统的浮点数计算（如`FP32`）需要更少的内存存储。这对于训练大规模的深度学习模型尤为重要，可以减少内存的占用。

1. 可接受的模型精度损失：使用`TF32`精度会导致一定程度的模型精度损失，因为低精度计算可能无法精确表示一些小的数值变化。然而，对于大多数深度学习应用，`TF32`精度仍然可以提供足够的模型精度。

## 使用FP16半精度

```
import torch
from diffusers import DiffusionPipeline

pipe = DiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16,
)
```

使用`FP16`半精度训练的优势：

减少了一半的内存占用，我们可以进一步将`batch`大小翻倍，并将训练时间减半。一些`GPU`如V100, 2080Ti等针对16位计算进行了优化，能自动加速3-8倍。

## 对注意力模块进行切片

当模型中的注意力模块存在多个注意力头时，可以使用切片注意力操作，使得每个注意力头依次计算注意力矩阵，从而大幅减少内存占用，但随之而来的是推理时间增加约10%。

```
import torch
from diffusers import DiffusionPipeline

pipe = DiffusionPipeline.from_pretrained(
"runwayml/stable-diffusion-v1-5",

torch_dtype=torch.float16,
)

pipe = pipe.to("cuda")

切片注意力
pipe.enable_attention_slicing()
对VAE进行切片
```

和注意力模块切片一样，我们也可以对`VAE`进行切片，让`VAE`每次处理`Batch`（32）中的一张图片，从而大幅减少内存占用。

```
import torch
from diffusers import StableDiffusionPipeline

pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    
    torch_dtype=torch.float16,
)
pipe = pipe.to("cuda")

prompt = "a photo of an astronaut riding a horse on mars"
#切片VAE
pipe.enable_vae_slicing()
images = pipe([prompt] * 32).images
```

## 大图像切块

当想要生成4k或者更大的图像，并且内存不充裕时，可以使用图像切块的操作，让`VAE`的编码器与解码器对切块后的图像逐一处理，最后从容拼接生成大图。

```
import torch
from diffusers import StableDiffusionPipeline

pipe = StableDiffusionPipeline.from_pretrained(
"runwayml/stable-diffusion-v1-5",
torch_dtype=torch.float16,
)

pipe = pipe.to("cuda")

prompt = "a beautiful landscape photograph"

大图像切块
pipe.enable_vae_tiling()

image = pipe([prompt], width=3840, height=2224, num_inference_steps=20).images[0]
```

`CPU` <-> `GPU`切换

可以将整个`SD`模型或者`SD`模型的部分模块权重加载到`CPU`中，只有等推理时再将需要的权重加载到`GPU`。

```
import torch
from diffusers import StableDiffusionPipeline

pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5",
    torch_dtype=torch.float16,
)

#子模块CPU <-> GPU切换
pipe.enable_sequential_cpu_offload()

#整个SD模型CPU <-> GPU切换
pipe.enable_model_cpu_offload()
```

## 变换Memory Format

在CV领域，两种比较常见的memory format是`channels first(NCHW)`和`channels last(NHWC)`。将`channels first`转变成为`channels last`可能会提升推理速度，不过这也需要依AI框架和硬件而定。

在`Channels Last`内存格式中，张量的维度顺序为：(`batch_size`, `height`, `width`, `channels`）。其中，`batch_size`表示批处理大小，`height`和`width`表示图像或特征图的高度和宽度，`channels`表示通道数。

相比而言，`Channels First`是另一种内存布局，其中通道维度被放置在张量的第二个维度上。在`Channels First`内存格式中，张量的维度顺序为：(`batch_size`, `channels`, `height`, `width`）。

选择`Channels Last`或`Channels First`内存格式通常取决于硬件和软件平台以及所使用的深度学习框架。不同的平台和框架可能对内存格式有不同的偏好和支持程度。

在一些情况下，`Channels Last`内存格式可能具有以下优势：

1. 内存访问效率：在一些硬件架构中，如`CPU`和`GPU`，`Channels Last`内存格式能够更好地利用内存的连续性，从而提高数据访问的效率。

1. 硬件加速器支持：一些硬件加速器（如NVIDIA的`Tensor Cores`）对于`Channels Last`内存格式具有特定的优化支持，可以提高计算性能。

1. 跨平台兼容性：某些深度学习框架和工具更倾向于支持`Channels Last`内存格式，使得在不同的平台和框架之间迁移模型更加容易。

需要注意的是，选择内存格式需要根据具体的硬件、软件和深度学习框架来进行评估。某些特定的操作、模型结构或框架要求可能会对内存格式有特定的要求或限制。因此，建议在特定环境和需求下进行测试和选择，以获得最佳的性能和兼容性。

## 使用xFormers

使用`xFormers`插件能够让注意力模块优化运算，提升20%左右的运算速度。

```
print(pipe.unet.conv_out.state_dict()["weight"].stride())

变换Memory Format

pipe.unet.to(memory_format=torch.channels_last)

print(pipe.unet.conv_out.state_dict()["weight"].stride())

from diffusers import DiffusionPipeline

import torch

pipe = DiffusionPipeline.from_pretrained(
"runwayml/stable-diffusion-v1-5",
torch_dtype=torch.float16,
).to("cuda")

使用xFormers

pipe.enable_xformers_memory_efficient_attention()
```
