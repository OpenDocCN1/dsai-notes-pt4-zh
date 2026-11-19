
# ComfyUI教程 副本

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/X2Gld0gOeoA16SxESPbcN1aDnym](https://xatc0v8uz5m.feishu.cn/docx/X2Gld0gOeoA16SxESPbcN1aDnym)

## ComfyUI基础介绍

ComfyUI主要是一个基于节点的操作流程，我们可以通过修改某一部分的节点对画面实现更精准的控制，再通过各个节点的不同组合，形成各种各样的生成方式。并且ComfyUI可以保存自己的工作流程再分享给其他人，其他人可以通过这个流程复现自己的工作流，相比Web UI，ComfyUI拥有更高的自由度、出图更快，但操作也相对更复杂。

ComfyUI是一个为Stable Diffusion专门设计的基于节点的图形用户界面（GUI）。它使用户能够通过链接不同的块（称为节点）来构建复杂的图像生成工作流程。这些节点可以包括各种任务，如加载检查点模型、输入提示、指定采样器等。

官方介绍：https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file

![](img/254e353818a4a0f418aa3845bf897471.png)

![](img/271aac5660c235c191e39e4fee0dd461.png)

## 如何云端启动ComfyUI

第一步点击应用商店中的ComfyUI镜像

![](img/71f53759929e74774a0e6fb8e0f3a271.png)

![](img/b15f58711beba2052a08bc368e301755.png)

![](img/f2dd32f1e6aa4bd5190916420aa665c3.png)

![](img/7174b3a8a778e2941a677492a495079c.png)

+ 基础文生图工作流

与Web UI相似：选择模型→输入提示词→设置参数→生成图片

![](img/092ed099fd210eafa6ee784d5c96c3d0.png)

+ 相关参数

`control_after_generate`（运行后操作）：控制种子生成

`fixed`（固定）：固定种子

`increment`（增加）：原有基础上+1

`decrement`（减少）：原有基础上-1

`randomize`（随机）：随机种子

![](img/591ce34284b44c4222ce9f6625d027b3.png)

`scheduler`（调度器）：一般选择`normal`或者`karras`

`sampler_name`（采样器）：设置采样算法

`denoise`（降噪）：文生图时表示去噪强度，值越大对图片产生的影响和变化越大

![](img/16943f5c1360c0a67ea00f56a4080a93.png)

尺寸推荐：

SD1.5模型推荐512*512

SDXL模型推荐1024*1024

![](img/4a2926f6fc2d60c8536abf564fbf7834.png)

### 如何安装各类模型

首先我们先来说下，在ComfyUI中如何安装大模型、Lora等一系列模型。（以Lora模型为例）

方法一

![](img/d2de8146de71fef6554860446839c92a.png)

![](img/14ee2f39fc91a9cf7e9dec418c496471.png)

![](img/de98022471d2fc88afc9673db3730ef0.png)

方法二

![](img/e3debc8056a9a87d2c2e290c33cb90f2.png)

![](img/13b25a455b033043d6697aeb0d655314.png)

![](img/38c66e119b7e48708e35db58e8767db4.png)

### ComfyUI中的节点

节点是ComfyUI工作流中的核心组件，它们代表了不同的操作和功能。节点可以是采样器、模型、提示词等，通过节点之间的连接，我们可以构建出复杂的工作流程。节点的配置和连接方式直接影响最终的输出结果。

而节点有分为两种，分别是官方原生节点以及用户开发的自定义节点，这些节点共同构成了ComfyUI的节点工作流，使得用户可以设计和执行各种基于Stable Diffusion的复杂流程，接下来我为大家详细介绍下这两种节点。

#### 原生节点

官方原生节点是ComfyUI开发团队提供的一系列基础节点，它们为用户提供了广泛的功能和工具，以支持图像生成和其他相关任务。比如

采样器节点、模型节点、提示词节点、`VAE`节点等。

![](img/6acca5f30f405bb6010cda122b8e3be8.png)

简单来说原生节点是不需要我们进行二次安装的，ComfyUI安装成功后直接可以使用。

#### 自定义节点

ComfyUI的强大之处在于其可扩展性，用户可以根据自己的需求开发自定义节点。目前来自全世界各地的社区成员已经贡献了600多个自定义节点，极大地丰富了工作流的设计和优化选择。

![](img/25f2651c4f642df1a39ac934db9a54b5.png)

并且官方原生节点和用户开发的自定义节点可以结合使用，创建高度定制化的工作流。用户可以利用官方节点构建基础流程，然后通过自定义节点添加特定的功能或优化现有流程。

#### 缺失节点安装

1. 大部分情况下，拖入工作流，于管理器处安装缺失节点。

![](img/78bad9575693a387c2343bda42ea7a7c.png)

2. 如果某些节点提示安全设置限制，禁止安装。则入`/usrdata/chenyu/ComfyUI/models/custom_nodes`

手动安装

![](img/ff73b49a89b4bae379ee3eaf7970e166.png)

上传完节点后记得要重启镜像。

### 快捷键介绍

这里我们参考官方给到的快捷键，官方地址：https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file

![](img/2526fbdfe4cc3fd9d1fffe5fb3ba3d5d.png)

## 基础功能讲解

### 文生图工作流

#### 创建文生图工作流

第一步：添加核心节点：采样器

单击右键：`Add Node`（新建节点）→`sampling`（采样）→`KSampler`（K采样器）

![](img/7e668c4043c184de35264e6a32d1393f.png)

第二步：拉出采样器节点→添加对应节点

`model`（模型）→`CheckpointLoaderSimple`（Checkpoint加载器（简易））

`positive`（正面条件）→`CLIPTextEncode`（CLIP文本编码器）

`negative`（负面条件）→`CLIPTextEncode`（CLIP文本编码器）

`latent_image`（Latent）→`EmptyLatentImage`（空Latent）

`LATENT`（Latent）→`VAEDecode`(VAE解码)

`IMAGE`（图像）→`SaveImage`（保存图片）

![](img/09e072486f1940add6a82b924e42f262.png)

第三步：根据对应颜色连接剩余节点

将剩余没有连接的节点根据颜色连接上

![](img/71a35f5b3f98e4ce0c01401ca79e78c1.png)

#### 开始文生图

在添加了Lora以及`Clip Skip`基础上，需要再次将节点根据顺序以及相同颜色重新连接

![](img/ee3b31c76ca1593248fdd4f90512d589.png)

![](img/f8512a98fe8c903de19028e45307cf89.png)

第一步：选择大模型

![](img/99e5b1587e460c86de12641723bc6a48.png)

第二步：选择Lora，调整权重

![](img/a0aa32f16506647d4a0fd8080ac2a95c.png)

第三步：设置`Clip Skip`，一般设置为-2

![](img/c5ae25374868d06f6613ff57cc84680f.png)

第四步：填写提示词

![](img/2f43c5d1632f1d8667bb421836a45606.png)

第五步：设置相关参数

因为这里选择的SDXL模型，因此采样步骤设置在40左右，图片尺寸1024*1024

![](img/5a89c14b7f42edd6cc334b2043505b00.png)

第六步：点击创作

![](img/2dd345e97a6e5afa3f6fc3d8324d05c2.png)

第七步：查看结果

![](img/a11df8c65fdbdfb0358aaee7b7623d25.png)

### 图生图+局部重绘工作流

#### 基础图生图

图生图可以在文生图基础上进行调整，需要加入`Load image`和`VAE Encode`两个节点

![](img/5605c7117430222a10bdef10f52eba57.png)

因为输入的图像只是像素图像，不能直接放进潜空间，因此需要vae编码器对图片进行编码，让潜空间能够识别，这里的最终生成尺寸与原图一致

`Empty Latent Image`：之前的文生图需要在空的噪点图降噪，然后生成新的图片，现在添加了图片，也就不需要这个空的噪点图了

操作流程：上传图片→选择模型→输入提示词→调整参数→创作

参数：

`denoise`：相当于重绘幅度，可以在0-1之间调整

使用图生图可以实现更换画风、添加细节、图像扩展、对图片高清修复等功能

#### 图片预处理

![](img/b95a63bf307cc6f122b7343416b0ff88.png)

通过添加不同的节点，对图片进行缩放或裁剪，也可以不添加

`Upscale Image`/`Upscale Image By`: 对图片进行缩放

`ImageCrop`: 对图片进行裁剪

#### 局部重绘

四种方式：`VAE Encode（for Inpainting）`、`Set Latent Noise Mask`、`ControINet Inpaint`、`Clip SEG`

1. `VAE Encode（for Inpainting）`

添加`VAE Encode（for Inpainting）`，连接`mask`，右键单击图片，选择`Open in MaskEditor`绘制蒙版，如果对绘制的部分有问题，可以按住鼠标右键擦除

![](img/e8f2626103ea6b90fb0c843a98801f10.png)

![](img/867da69c855f7323dd4c72b1622371f7.png)

![](img/ae273a54044118f6d36dba73a9a50434.png)

操作流程：选择与原图相近的模型，输入蒙版部分提示词

`VAE Encode（for Inpainting）`：对原始蒙版部分不会做任何保留，相当于重新绘制，随机性比较大

1. `Set Latent Noise Mask`

先将图片经过VAE编码变成潜空间能够识别的内容，再将蒙版部分变成噪波内容重新生成一次。

`Set Latent Noise Mask`：会参考原图进行重绘，能更好的理解生成的内容，生成错误图片的概率较低，适用微调，保持与原图的相似性

![](img/850934fa21631c13a8b400f14a79f713.png)

1. `ControINet Inpaint`

添加inpaint的控制网，控制网需要有模型以及对图片进行预处理

注意：图片需要添加`VAE`编码器进入潜空间

![](img/fd07e13c3d4ecf479d4166bab9010850.png)

1. `clipseg`

输入提示词自动划分蒙版区域，代替手动涂抹蒙版，可以与`Set Latent Noise Mask`一起使用

参数：

`text`：输入想要重绘的地方

`threshold`: 识别内容的精细程度

`dilation_factor`: 识别内容的扩散程度

输出：

`Heatmap Mask`: 热度图

`BW Mask`: 灰度图

可以分别预览识别到的蒙版区域

四种重绘方式的区别：

1. `VAE Encode（for Inpainting）`：相当于清空再重绘制，随机性较大，适合无中生有

1. `Set Latent Noise Mask`：会参考原图进行重绘，与原图会有一定相似性，适合用于微调

1. `ControINet Inpaint`：相对比较稳定和精致

1. `Clipseg`：自动识别蒙版区域，不需要手动涂抹，比较方便

### 核心节点

#### Image 图像

1. `Pad Image for Outpainting`（扩展图像）

填充扩展图像，类似智能扩图，先增加图片尺寸，再将扩充区域作为蒙版进行绘制，建议搭配`VAE Encode（for Inpainting）`节点，保证原始图像不会变化

参数：

`left`、`top`、`right`、`bottom`：上下左右的填充量

`feathering`：边缘羽化程度

![](img/a55d386e9c410810bde2091af9190dc6.png)

1. `save Image` （保存图像）

![](img/c81040090f01e9593d38badc8935bfa2.png)

1. `load Image` （加载图像）

![](img/d4f1c0ebe45d84e4ee3a61d9fd79e41b.png)

1. `ImageBlur` （图像模糊）

为图像加上模糊的效果

参数：

`sigma`：值越小，模糊就越集中于中心像素

![](img/f308eec8b6e66cad1c03311f8de215e6.png)

1. `Image Blend` （图像混合）

可以通过透明度将两张图像混合

参数：

`blend_factor`：第二个图像的不透明度

`blend_mode`：图像混合的方式

![](img/772821276505a60caa780f7b58b5a875.png)

1. `Image Quantize` （图像量化）

减少图像中的颜色数量

参数：

`colors`：量化图像中的颜色数量，当为1时，图像只有一个颜色

`dither`：是否使用抖动使量化图像看起来更平滑

![](img/8015d992fac32d98db879500b9603ee1.png)

1. `Image Sharpen` （图像锐化）

参数：

`sigma`：值越小，锐化就越集中于中心像素

`alpha`：锐化的强度

![](img/d7145c808011b995f4252d4b4a4994e0.png)

1. `Invert Image` （图像反转）

反转图像的颜色

![](img/dd56ac246c5c1eb82dc2a9d990efda4e.png)

1. `Upscaling` （图像放大）

（1）使用模型放大:`Upscale Image` （Using Model）

![](img/af6b587b015c8be702676fc33fa6f6a1.png)

（2）图像放大：`Upscale Image`

参数：

`upscale_method`：选择像素填充方法

`width`：调整后的图像宽度

`height`：调整后的图像高度

`crop`：是否对图片进行裁剪

![](img/bd708fa4ef58e7a3d872ad908197db31.png)

1. `Preview Image` （预览图像）

![](img/d9ccc0b70e08cd90fa279dbe6ec06bbc.png)

#### Loaders 加载器

1. `Load CLIP Vision CLIP` （视觉模型节点）

将图片解码形成描述（提示词），最后再转换为条件输入到采样器，根据解码到的描述（提示词），生成新的类似图片，可以多个节点一起使用。适合转换概念、抽象的东西，搭配`Clip Vison Encode`使用

![](img/dac417826d4a70d9d985412b84d4fd55.png)

1. `Load CLIP` （CLIP 节点）

`Load CLIP`节点可用于加载特定的`CLIP`模型，`CLIP`模型用于编码指导扩散过程的文本提示。

注意：条件扩散模型是使用特定的`CLIP`模型进行训练的，使用与其训练所用模型不同的模型不太可能产生良好的图像。`Load Checkpoint`节点会自动加载正确的`CLIP`模型。

![](img/9ea892a0b1cfeae44f966ebcec3f9562.png)

1. `unCLIP Checkpoint Loader` （检查点加载器节点）

`unCLIP`检查点加载器节点可用于加载专门用于与`unCLIP`配合使用的扩散模型。`unCLIP`扩散模型用于去噪潜在特征，不仅提供文本提示，还包括提供的图像。该节点还将提供适当的`VAE`和`CLIP`及`CLIP`视觉模型。

注意：尽管此节点可用于加载所有扩散模型，但并非所有扩散模型都与`unCLIP`兼容。

![](img/9dcf62a1b3d1ede6e089cdf9738fcf8f.png)

1. `load controInet model ControlNet` （模型节点）

`Load ControlNet Model`节点可用于加载`ControlNet`模型，与`Apply ControlNet`配合使用。

![](img/5d725daec7f1d052bb5d06155efc90d6.png)

1. `Load LoRA` （LoRA 节点）

![](img/a552cef6153922bc19e4aa490bff5409.png)

1. `Load VAE` （VAE 节点）

![](img/d6404781a7646a5d8e7f9b298836b7f9.png)

1. `Load Upscale Model` （放大模型节点）

![](img/7fc26666312d1168636ff6f70125a945.png)



1. `Load Upscale Model` （放大模型节点）

![](img/c5fc2016ee38ec3627effbde3fd045f5.png)

1. `Load Style Model` （风格模型节点）

`Load Style Model`节点可用于加载风格模型。风格模型可用于向扩散模型提供视觉提示，指示去噪潜在特征应该具有何种风格。

当前仅支持`T2IAdaptor`风格模型

![](img/3214d16367f542157dca5563de824493.png)

1. `Hypernetwork Loader` （网络加载器节点）

`Hypernetwork Loader`节点可用于加载超网络。与`LoRAs`类似，它们用于修改扩散模型，改变潜在特征去噪的方式。典型用法包括向模型添加以特定风格生成的能力，或更好地生成特定主题或动作。甚至可以将多个超网络链接在一起，进一步修改模型。

![](img/44f02192961faa112ac25ad061e6d4ee.png)

## Conditioning 条件假设

1. `Apply ControlNet`（应用`ControlNet`节点）

加载`ControlNet`模型，可以连接多个控制网节点

参数：

`strength`：权重程度，数值越高对图片约束越强

控制网图像应该是相应的预处理图，例如`canny`对应`canny`的预处理图，因此需要在原始图像与控制网之间添加相应节点将其处理为预处理图

![](img/c1dec61308256ecd1a0033b7f63df2ce.png)

1. `CLIP Text Encode (Prompt)`（文本编码节点）

输入文本提示词,包括正向提示词和负向提示词

![](img/d24606422073b7c77a27b41e4f8ff118.png)

1. `CLIP Vision Encode`（CLIP 视觉编码节点）

将图片解码形成描述（提示词），最后再转换为条件输入到采样器，根据解码到的描述（提示词），生成新的类似图片，可以多个节点一起使用。适合转换概念、抽象的东西，搭配`Load Clip Vision`使用

![](img/e19b831ac10d8c604fb3f781a240e99a.png)

1. `CLIP Set Last Layer`（CLIP 设置最后一层节点）

`Clip Skip`，通常设置为-2

![](img/690c918195ec42ba1438225cbef6fe06.png)

1. `GLIGEN Textbox Apply`（GLIGEN文本框应用节点）

引导提示词在图像的指定部分生成

`ComfyUI`中坐标系统的原点位于左上角

![](img/bf93ad7e1cfacd189ce237ab91c28ff4.png)

1. `unCLIP Conditioning`（unCLIP条件化节点）

通过`CLIP`视觉模型编码的图像为`unCLIP`模型提供额外的视觉指导，此节点可以串联以提供多个图像作为指导

![](img/96aaca1059fdee6ec84b7bd56e04d866.png)

1. `Conditioning (Average)` （平均调节节点）

根据强度对两条信息进行混合，当`conditioning_to_strngth`为1时，扩散将只受`conditioning_to`的影响，当`conditioning_to_strngth`为0时，图像扩散将只受`conditioning_from`的影响

![](img/05ded11a57cdca968318e1634ce9b741.png)

1. `Apply Style Model` （应用样式模型节点）

可用于为扩散模型提供进一步的视觉指导，特别是关于生成图像的风格

![](img/bcb11ab0b2ddd05e31bd59729c4f7f4a.png)

1. `Conditioning (Combine)` 调节（合并）节点

将两条信息进行混合

![](img/522b229eb631fda1b92cae6b86fafdb9.png)

1. `Conditioning (Set Area)` 调节（设置区域）节点

`Conditioning (Set Area)`节点可用于将影响区域限制在图像的指定区域内。与`Conditioning (Combine)`节点一起使用，可以更好地控制最终图像的构成。

参数：

`width`：控制区域的宽度

`height`：控制区域的高度

`x`：控制区域原点的`x`坐标

`y`：控制区域原点的`y`坐标

`strength`：条件信息的强度

坐标原点位于图片左上角

![](img/2ad93c5d229b7b02662c9e0813cb1ed5.png)

如图：将左边设置为猫，右边设置为狗

1. `Conditioning (Set Mask)` 调节（设置遮罩）节点

`Conditioning (Set Mask)`可用于将一个调节限制在指定的遮罩内。与`Conditioning (Combine)`节点一起使用，可以更好地控制最终图像的构成

![](img/5def0bdf5c1d38658bb337f543961039.png)

## Latent 潜空间

1. `VAE Encde`（for Inpainting）（VAE 编码节点（用于修复））

适用于局部重绘，右键单击，通过涂抹蒙版实现局部重绘

![](img/f30d78b70898fa919d568cc901a9300d.png)

1. `Set Latent Noise Mask` （设置潜在噪声遮罩节点）

局部重绘的第二种方式，先将图片通过`vae`编码器变成潜空间能识别的内容，再在潜空间处将遮罩部分重新生成一次

相比`VAE Encde`（for Inpainting）方式，能更好的理解需要重新生成的内容，生成错误图片的概率更低，会参考图片进行重新绘制

![](img/3c8c00814ef7e2a4ab5ddf831d850b1f.png)

1. `Rotate Latent` （旋转潜像节点）

将图像顺时针旋转

![](img/e9a7a514c1b16d071c0d1a10c4095aab.png)

1. `Flip Latent`（翻转潜像节点）

水平或垂直翻转图像

![](img/c2e96b29559ee0ca0a484186c336a3b0.png)

1. `Crop Latent`（裁剪潜像节点）

用于将潜在图像裁剪成新的形状

![](img/fc6ae58a4e9c0a2962adea228c11548f.png)

1. `VAE Encode`（VAE 编码节点）

![](img/054414968040508c2d802107e71c9b01.png)

1. `VAE Decode`（VAE 解码节点）

![](img/fd7cf93975e67af96564ec1c615aa8f7.png)

1. `Latent From Batch`（批次中提取潜在图像）

从批次中提取潜像图，`Latent From Batch` 节点可以用于从批次中选择一个潜像图或图像片段。这在工作流中需要隔离特定的潜像图或图像时非常有用

![](img/99f1139ff0f7405ca22e10592b00d1c5.png)

1. `Repeat Latent Batch`（重复潜在图像批次处理节点）

重复一批图像，可用于图像到图像的工作流程中创建一个图像的多个变体

![](img/27b61ed5735f6c2601e554faa6147ffa.png)

1. `Rebatch Latents`（重新批处理潜像节点）

可以用于拆分或合并批量的潜在空间图像

![](img/8b6306707c6009861c816a190f5a7470.png)

1. `Upscale Latent`（放大潜在图像节点）

对潜空间图像进行分辨率调整，仅使用数学方式进行像素填充

参数：

`upscale_method`：表示像素填充的方法

`width`：调整后潜空间图像的宽度

`height`：调整后潜空间图像的高度

`crop`：表示是否对图像进行裁剪

潜空间放大的图像，通过`VAE`解码后会出现毁坏的情况，可以使用`KSampler`采样器进行二次采样，从而修复图像

![](img/c87ea4888a47ce1e3b7ae915353e752e.png)

1. `Latent Composite`（潜在复合节点）

将一个图像叠加到另一个上

参数：

`x`：上层图层叠加位置的的`x`坐标

`y`：上层图层叠加位置的的`y`坐标

图像需要经过编码（`VAE Encode`）进去潜空间

![](img/fc44c336ebeb41c0383c0417330ec090.png)

1. `Latent Composite Masked`（潜在复合遮罩节点）

将一个带遮罩的图像叠加到另一个上，仅叠加遮罩部分

输入：

`destination`：底层潜空间图像

`source`：上层潜空间图像

参数：

`x`：表示叠加区域的`x`坐标

`y`：表示叠加区域的`y`坐标

`resize_source`：表示是否对蒙版区域进行分辨率调整

![](img/28b85876705a552b9b51de7019b7be2d.png)

1. `Empty Latent Image`（空潜在图像节点）

空潜在图像节点可用于创建一组新的空潜在图像。这些潜在图像随后可以在例如`text2image`工作流中通过使用采样节点对它们进行加噪和去噪来使用

![](img/dcb00ea2160117631d7acc5882e862da.png)

## Mask 遮罩

1. `Load Image As Mask`（加载图像作为遮罩节点）

![](img/3b84b9bd3b29729cddeafc2960bdcc0a.png)

1. `Invert Mask`（反转遮罩节点）

![](img/d358bf8d947d354a38832395211adab2.png)

1. `Solid Mask`（实心遮罩节点）

相当于生成图片的画布，可以与`Mask Composite`结合

![](img/4004b9a15c81ffeb6a2dc2f4815230bc.png)

1. `Convert Mask To Image`（将遮罩转换为图像节点）

![](img/6666b29c7e2ae9f2bd9153a5df12b6fc.png)

1. `Convert Image To Mask`（将图像转换为遮罩节点）

将遮罩转换为灰度图像

![](img/35cab23def0c2da9a51452a5cf1b37d7.png)

1. `Feather Mask`（羽化遮罩节点）

对遮罩进行羽化处理

![](img/cfd78d43fffb0b68ea3ba7fdcd3bca3e.png)

1. `Crop Mask`（裁剪遮罩节点）

将遮罩裁剪为新的形状

![](img/bab827316177845bdb5750532093d873.png)

1. `Mask Composite`（遮罩合成节点）

将一个遮罩粘贴到另一个遮罩中，连接`Solid Mask`，`Value`为0表示黑色，不会被绘制，`Value`为1表示白色，会被绘制，连接的两个`Solid Mask`中的`Value`需要不一样，否则蒙版不会生效

输入：

`destination`（1）：要粘贴进去的遮罩，相当于最后生成图片的尺寸

`source`（0）：要被粘贴的遮罩

参数：

`X`,`Y`：对`source`的位置进行调整

`operation`（混合方法）: `source`为0时使用`multiply`，为1时使用`add`

![](img/087b2b9007c64213cc0b2f3a61c8cf73.png)

## Sampler 采样

1. `KSampler`（K采样器）

输入：

`latent_image`：将被去噪的潜在图像

输出：

`LATENT`：去噪后的潜在图像

![](img/7a83554182468e0bc638f4933fca179e.png)

1. `KSampler Advanced`（高级K采样器节点）

可以手动控制噪声

![](img/ae49036bf587ebafc20aa6960301c002.png)

## 高级节点

1. `Load Checkpoint` (With Config) 【加载`Checkpoint`（带配置）】

根据提供的配置文件加载扩散模型

![](img/d768b6e73c169f473aa85901ca90b6f9.png)

## 其他节点

1. `AIO Aux Preprocessor`（Aux集成预处理器）

选择不同预处理器生成相应图像

![](img/7653c3a219af1121e321969cb4732291.png)

## 小知识

### 修改节点标题

右键单击选择`Tile`

![](img/fab24f22fbc175e34683842ed2714f48.png)

### 生图出现系统取消

可能原因：

1. 节点是否全部连接

2. 节点顺序是否连接正确

3. 是否选择模型

### 搜索节点

在空白处双击鼠标左键

![](img/28a00fe5227139f34d96a3cd864520f9.png)

### 创建组

将多个节点创建为一组，可以一起拖动

单击右键选择`Add Group`，右下角拉动`Group`框选节点

![](img/def1afcf39c593d7747e677c6e863c09.png)

### 缩小节点页面

点击节点左上角的灰色按钮

![](img/dca5cf8ef955d7888898d83bc0722373.png)

![](img/bb42624e451ddfc2cbdb139b87efebf0.png)
