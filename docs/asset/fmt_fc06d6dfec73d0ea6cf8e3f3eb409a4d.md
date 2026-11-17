
# 深入浅出解析Stable Diffusion完整核心基础知识 |【算法兵器谱】

> 来源：[https://keenchase.feishu.cn/docx/NoQLdK93hoZj0JxF1mDc8IVqnDb](https://keenchase.feishu.cn/docx/NoQLdK93hoZj0JxF1mDc8IVqnDb)
>
> 点我返回👉【杰夫AI知识库｜首页】
>
> [点我跳转👉【AI工具库】](https://keenchase.feishu.cn/wiki/TSyWwNmPsi2CZTk4fqMc2XQgnkd)

2022年，Stable Diffusion横空出世，成为AI行业从传统深度学习时代过渡至AIGC时代的标志模型，并为工业界和投资界注入了新的活力，让AI再次性感。

本文中，Rocky将深入浅出的讲解Stable Diffusion的核心知识，例举最有价值的应用场景，对Stable Diffusion的训练过程进行通俗易懂的分析，并尝试对其性能进行优化，Rocky希望我们能更好的入门Stable Diffusion及其背后的AIGC领域。

话不多说，在Rocky毫无保留的分享下，让我们开始学习吧！

So，enjoy（与本文的BGM一起食用更佳哦）：

## 正文开始

## 目录先行

### Stable Diffusion核心基础内容
1. Stable Diffusion模型原理
1. Stable Diffusion模型的核心组件
1. Stable Diffusion推理流程

### Stable Diffusion经典应用场景
1. 文本生成图片
1. 图片生成图片
1. 图片inpainting
1. 使用controlnet辅助生成图片
1. 超分辨率重建

### Stable Diffusion训练过程

### Stable Diffusion性能优化

## Stable Diffusion核心基础内容

### 【一】Stable Diffusion模型原理

Stable Diffusion（SD）模型和GAN模型一样，是生成式模型，了解GAN模型的朋友都知道，生成式模型能够生成和训练集分布相似的输出结果（拟合数据分布），在计算机视觉领域是图片，在自然语言处理领域是文字。

下面是主流生成式模型各自的生成逻辑：

![](img/1fab50cdf75aa203b6d825d55ffcb327.png)

生成式模型的主流架构

Rocky在这里拿GAN详细展开讲讲，由于篇幅原因，`VAE`和`Flow-based models`这里就不过多介绍，感兴趣的朋友可以找Rocky私下交流。

GAN由生成器和判别器组成。其中，生成器主要负责生成相应的样本数据，输入一般是由高斯分布随机采样得到的噪声。而判别器的主要职责是区分生成器生成的样本与样本，输入一般是样本与相应的生成样本，我们想要的是对样本输出的置信度越接近越好，而对生成样本输出的置信度越接近越好。与一般神经网络不同的是，GAN在训练时要同时训练生成器与判别器，所以其训练难度是比较大的。

我们可以将GAN中的生成器比喻为印假钞票的犯罪分子，判别器则被当作警察。犯罪分子努力让印出的假钞看起来逼真，警察则不断提升对于假钞的辨识能力。二者互相博弈，随着时间的推移，都会越来越强。在图像生成任务中也是如此，生成器不断生成尽可能逼真的假图像。判别器则判断图像是图像，还是生成的图像。二者不断博弈优化，最终生成器生成的图像使得判别器完全无法判别真假。

好的，讲完GAN，让我们回到SD模型。SD模型有两个最基本也是最经典的应用，分别是文生图（`txt2pic`）和图生图（`pic2pic`）。

![](img/6a92e8c51050027b1cc9cfe066b8af03.png)

SD文生图过程

文生图是指将一段文字输入到SD模型中，经过一定的迭代次数，SD模型输出一张符合输入文字描述的图片。比如上图中输入了“天堂，巨大的，海滩”，于是生成了一个美丽沙滩的图片。

![](img/88ad2aeb92f3f747a39ede75b992f6d2.png)

SD图生图过程

而图生图在输入文字的基础上，再输入一张图片，SD模型将根据文字的提示，将图片进行丰富，比如上图中，SD模型将“海盗船”添加在之前那个美丽的沙滩上。

Rocky在这里也分享一些朋友生成的图片，整体上都非常逼真：

![](img/5935c309606e712789ecf0082b5b91f0.png)

感受了SD模型强大的生成能力，大家可能会想到生成式领域上一个霸主模型GAN，与GAN模型不同的是，SD模型是属于扩散模型，并且是基于`latent`的扩散模型。

什么是扩散模型呢？用通俗的话来讲，就是将图像的生成过程分多步进行，逐步完善图像内容，经过20-50次的“扩散”循环，最终输出精致的图像。

下面是一个直观的例子，将随机高斯噪声矩阵通过SD模型的`Inference`过程，逐步去燥，最后生成一个小别墅的图片。

![](img/8a46b58c4b5f5ef4482bcf3b70d4c688.png)

SD模型的Inference过程

那`latent`又是什么呢？基于`latent`的扩散模型可以在低维度的隐空间上进行“扩散”过程而不是在实际`pixel`空间，这样一来大大降低了内存占用和计算复杂性，这是常规扩散模型和`latent`扩散模型之间的主要区别。同样的，在训练中，`latent`扩散模型也将训练过程聚焦在`latent`空间中。

举个例子，如果SD模型中将数据矩阵缩小的倍数设为，那么原本尺寸为的数据矩阵就会进入的`latent`隐空间中，内存和计算量直接缩小倍，整体效率大大提升。

到这里，大家应该对SD模型的基本概念有一个清晰的认识了，Rocky这里再帮大家总结一下：

1. SD模型是生成式模型，与GAN模型有很多相似的地方，输入可以是图片，也可以是文字，输出是图片。
1. SD模型属于扩散模型，扩散模型的整理逻辑是生成过程分步化与迭代化，这给整个生成过程引入更多约束与优化提供了可能。
1. SD模型是基于`latent`的，将生成空间压缩到`latent`空间中，比起常规扩散模型，大大提高计算效率的同时，降低了内存占用，成为了SD模型爆发的关键一招。
1. 站在CTO视角，将维度拉到最高维，Rocky认为SD模型的整体流程是一个优化噪声的艺术。

### 【二】Stable Diffusion模型的核心组件

SD模型主要由自动编码器（`VAE`），`U-Net`以及文本编码器三个核心组件构成。

![](img/709c8cc281758480bdd1cb5af7debeeb.png)

SD模型文生图流程

1. 自动编码器（`VAE`）：`VAE`的编码器能够将输入图像转换为低维特征，作为`U-Net`的输入。`VAE`的解码器将隐特征升维解码成完整图像。不同的`VAE`结构能够为生成图片带来不同的细节与整体颜色。

![](img/df6e72a0fc8b38732544c6db123561f0.png)

VAE的主要工作

为什么`VAE`可以将图像压缩到一个非常小的潜空间后能再次对图像还原呢？虽然整个过程可以看作是一个有损压缩，但自然图像并不是随机的，它们具有很高的规律性：比如说一张脸上的眼睛、鼻子、脸颊和嘴巴之间遵循特定的空间关系，又比如说一只猫有四条腿，并且是一个特定的生物结构。所以如果我们生成的图像尺寸在之上时，其实特征损失带来的影响非常小。

1. `U-Net`：预测噪声残差，结合调度算法（`PNDM`，`DDIM`，`K-LMS`等）进行噪声重构，逐步将随机高斯噪声转化成图片的隐特征。`U-Net`整体结构一般由`ResNet`模块构成，并在`ResNet`模块之间添加`CrossAttention`模块用于接收文本信息。

![](img/d6b862d1bd74cd1421eeb3222fa6aeb6.png)

常规`U-Net`结构

![](img/a96ced5829afd56f8e1b250c88e5e607.png)

`ResNet`模块与`CrossAttention`模块的结合

1. 文本编码器：将输入`prompt`进行编码，输出`token embeddings`向量（语意信息），通过`CrossAttention`方式送入扩散模型的`U-Net`中作为`condition`，对生成图像内容进行一定程度上的控制，目前SD默认的是`CLIP text encoder`。

### 【三】Stable Diffusion推理流程

想要运行Stable Diffusion（SD），我们可以直接使用`diffusers`的完整`pipeline`流程。

```
# 首先，安装相关依赖
pip install diffusers transformers scipy ftfy accelerate
# 读取diffuers库
from diffusers import StableDiffusionPipeline
# 初始化SD模型，加载预训练权重
pipe = StableDiffusionPipeline.from_pretrained("runwayml/stable-diffusion-v1-5")
# 使用GPU加速
pipe.to("cuda")
# 如GPU的内存少于10GB，可以加载float16精度的SD模型
pipe = StableDiffusionPipeline.from_pretrained("runwayml/stable-diffusion-v1-5", revision="fp16", torch_dtype=torch.float16)
# 接下来，我们就可以运行pipeline了
prompt = "a photograph of an astronaut riding a horse"
image = pipe(prompt).images[0]
```

由于没有固定`seed`，每次运行代码，我们都会得到一个不同的图片。

我们打开下载的预训练文件夹，可以看到预训练模型主要由以下几个部分组成：

`text_encoder`和`tokenizer`，`scheduler`，`unet`，`vae`。

其中`text_encoder`，`scheduler`，`unet`，`vae`分别代表了上面讲到过的SD模型的核心结构。

同时我们还可以看到`Tokenizer`文件夹，表示标记器。`Tokenizer`首先将`Prompt`中的每个词转换为一个称为标记（`token`）的数字，符号化（`Tokenization`）是计算机理解单词的方式。然后，通过`text_encoder`将每个标记都转换为一个768值的向量，称为嵌入（`embedding`），用于`U-Net`的`condition`。

![](img/55213aa716ebd3629484deaa9f30eafc.png)

`Tokenizer`的作用

有时候我们运行完`pipeline`之后，会出现纯黑色图片，这表示我们本次生成的图片触发了`NSFW`机制，出现了一些违规的图片，我们可以修改`seed`重新进行生成。

我们可以自己设置`seed`，来达到对图片生成的控制。

```
import torch

#manual_seed(1024)：每次使用具有相同种子的生成器时，都会获得相同的图像输出。
generator = torch.Generator("cuda").manual_seed(1024)

image = pipe(prompt, guidance_scale=7.5, generator=generator).images[0]
```

将`pipeline`的完整结构梳理好之后，我们再对一些核心参数进行讲解：

1. `num_inference_steps`。`num_inference_steps`表示我们对图片进行噪声优化的次数。一般来说，我们可以选择`num_inference_steps = 20/25/50`，数值越大，图片生成效果越好，但同时生成所需的时间就越长。
1. `guidance_scale`，代表无分类指引（`Classifier-free guidance`，`guidance_scale`，`CFG`）是一个控制文本提示对扩散过程的影响程度的值。简单来说就是在加噪阶段将条件控制下预测的噪音和无条件下的预测噪音组合在一起来确定最终的噪声。通常`guidance_scale`可以选7-8.5之间，如果使用非常大的值，图像可能看起来不错，但多样性会降低。

![](img/2c16f82bfac794c4bb419d31a96685ff.png)

其中代表`CFG`，当越大时，`condition`起的作用越大，即生成的图像更和输入文本一致，当被设置为时，图像生成是无条件的，文本提示会被忽略。

1. 输出尺寸

SD在默认情况下会输出尺寸的图片。我们也可以自定义设置图片尺寸，Rocky建议如下：

1. 建议`height`和`width`都是8的倍数。
1. 低于可能会导致图像质量较低。
1. 创建非正方形图像的推荐方法是在一维中使用，另一个使用更大的值。

```
prompt = "a photograph of an astronaut riding a horse"
Number of denoising steps steps = 25
Scale for classifier-free guidance CFG = 7.5
image = pipe(prompt, guidance_scale=CFG, height=512, width=768, num_inference_steps=steps).images[0]
```

除了将预训练模型整体加载，我们还可以将SD模型的不同组件单独加载：

```
from transformers import CLIPTextModel, CLIPTokenizer
from diffusers import AutoencoderKL, UNet2DConditionModel, PNDMScheduler
from diffusers import LMSDiscreteScheduler

# 单独加载VAE模型
vae = AutoencoderKL.from_pretrained("runwayml/stable-diffusion-v1-5", subfolder="vae")

# 单独加载CLIP模型和tokenizer
tokenizer = CLIPTokenizer.from_pretrained("openai/clip-vit-large-patch14")
text_encoder = CLIPTextModel.from_pretrained("openai/clip-vit-large-patch14")

# 单独加载U-Net模型
unet = UNet2DConditionModel.from_pretrained("runwayml/stable-diffusion-v1-5", subfolder="unet")

# 单独加载调度算法
scheduler = LMSDiscreteScheduler(beta_start=0.00085, beta_end=0.012, beta_schedule="scaled_linear", num_train_timesteps=1000)
```

## Stable Diffusion经典应用场景

### 文本生成图像

输入：`prompt`

输入：图像

其中`Load Checkpoint`模块代表对SD模型的主要结构进行初始化（`VAE`，`U-Net`），`CLIP Text Encode`表示文本编码器，可以输入`prompt`和`negative prompt`，来控制图像的生成，`Empty Latent Image`表示初始化的高斯噪声，`KSampler`表示调度算法以及SD相关生成参数，`VAE Decode`表示使用`VAE`的解码器将低维度的隐空间特征转换成像素空间的生成图像。

### 图片生成图片

输入：图像 + `prompt`

输出：图像

其中`Load Checkpoint`模块代表对SD模型的主要结构进行初始化（`VAE`，`U-Net`），`CLIP Text Encode`表示文本编码器，可以输入`prompt`和`negative prompt`，来控制图像的生成，`Load Image`表示输入的图像，`KSampler`表示调度算法以及SD相关生成参数，`VAE Encode`表示使用`VAE`的编码器将输入图像转换成低维度的隐空间特征，`VAE Decode`表示使用`VAE`的解码器将低维度的隐空间特征转换成像素空间的生成图像。

与文字生成图片的过程相比，图片生成图片的预处理阶段，先把噪声添加到隐空间特征中。我们设置一个去噪强度（`Denoising strength`）控制加入多少噪音。如果它是0，就不添加噪音。如果它是1，则添加最大数量的噪声，使潜像成为一个完整的随机张量，如果将去噪强度设置为1，就完全相当于文本转图像，因为初始潜像完全是随机的噪声。

![](img/958bf86216411e6f648e6afb2953bc64.png)

去噪强度（`Denoising strength`）控制噪音的加入量

### 图片inpainting

输入：图像 + `mask` + `prompt`

输出：图像

其中`Load Checkpoint`模块代表对SD模型的主要结构进行初始化（`VAE`，`U-Net`），`CLIP Text Encode`表示文本编码器，可以输入`prompt`和`negative prompt`，来控制图像的生成，`Load Image`表示输入的图像和`mask`，`KSampler`表示调度算法以及SD相关生成参数，`VAE Encode`表示使用`VAE`的编码器将输入图像和`mask`转换成低维度的隐空间特征，`VAE Decode`表示使用`VAE`的解码器将低维度的隐空间特征转换成像素空间的生成图像。

下面就是如何进行图像`inpainting`的直观过程：

![](img/00f031ad7a6e1fe771f2575e2a7edf80.png)

### 使用controlnet辅助生成图片

输入：素描图 + `prompt`

输出：图像

![](img/12fd614e20bcbe59356d673cc6e86677.png)
