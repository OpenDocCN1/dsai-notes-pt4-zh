
# ComfyUI详细入门系列教程

> 来源：[https://aigcreative.feishu.cn/docx/EDnRd0OpboIFCLxdUmzcX5XBnXf](https://aigcreative.feishu.cn/docx/EDnRd0OpboIFCLxdUmzcX5XBnXf)

> 内容整理自：👸号：AI艺术家

## SDXL模型最佳拍档！COMFY UI上手指南

> 链接：https://mp.weixin.qq.com/s/l0p64XduR4bgDyLy8tmTPA

SDXL 1.0 特点建立在由 3.5B 参数BASE模型和 6.6B 参数REFINE模型组成的两阶段创新架构之上。

理论上，SDXL 1.0 应该在具有 8GB VRAM 的 GPU 上有效工作。但sdwebui现在做不到（需要10G）,能做到的只有COMFY UI。

COMFYUI是一款易于上手的工作流设计工具，具有以下特点：基于工作流节点设计，可视化工作流搭建，快速切换工作流，对显存占用小，速度快，支持多种插件，如ADetailer、Controlnet和AnimateDIFF等。

![](img/2fec7b1abc412a7804303dcd4357f324.png)

ComfyUI 和stable-diffusion-webui的作用类似，安装方法也类似。我们为大家提供了特别的压缩包，只要解压软件，就算是安装成功了。

![](img/accaecf51fd31e606fbd6bba0f5f2a94.png)

下载SDXL模型和COMFYUI安装包，解压缩后，设置模型路径。然后，安装插件管理器，安装必要插件，最后导入工作流。

![](img/b1f488b807c3978121b5d45601fbe778.png)

安装软件之后，需要获取SDLX1.0的两个模型base和refiner。

还需要你把它们放在模型路径上。

![](img/2c7bc741476f847e665f3524b73666cd.png)

把其中的`extra_model_paths.yaml.example`改为`extra_model_paths.yaml`。并修改其中的模型目录，就可以实现自定义模型目录，或者和stable-diffusion-webui共用模型目录。

![](img/fc8d608949856ca349776b46439b5e73.png)

在使用上，COMFY-UI采用的是工作流模式：

![](img/dfc2f2281aac42835dc8a9c10edab2b1.png)

可以说，工作流就是COMFY的灵魂。

![](img/cb7f5964c7a408fd18282ee847ed83cf.png)

当然，工作流确实看起来很繁琐，之前没接触过的人，会看着一堆密密麻麻的线，望而却步。比如这样。

但是实际上，你只需要导入工作流。

![](img/6ef15920fb9b426c68ab6e21fa61ab3f.png)

如果是预设好的工作流，你只需要修改：

![](img/6c99b333df9234d321c91e0072f495f9.png)

然后，按一下任务排队执行：

![](img/ae9e6fe20973747202ae1160f7b96d8c.png)

任务就快速完成了。

![](img/390b4222f3239dde8ea63b7207bca580.png)

最关键的，您需要安装一个插件管理器，以便在COMFY中安装各种第三方的节点器。您可以按照以下步骤操作：进入命令行模式，在COMFY目录中输入指令，然后克隆插件管理器。

最后，重启COMFYUI即可开始使用。

![](img/449da0422c3a30dcf6ab3ca2e6d42fa9.png)

如果遇见上图这种缺模块的情况，直接通过管理器安装缺失模块即可。

![](img/73bb11aee33f89f5f709d56aa0f2f07c.png)

你也可以通过自己研究工作流，设计更多的顶级工作流。比如下面这些：

![](img/d18624009ea140c526037e0271b4edd2.png)

## COMFY UI使用教程基础篇1：节点安装和工作流

> 链接：https://mp.weixin.qq.com/s/FzZ66kwlwyRai4xGuce38w

这是关于如何从零开始使用COMFY UI工作流的简短指南，COMFY UI是典型的learn in doing,了解COMFY UI如何工作的最佳方法是制作自己的工作流程。

我同时使用ComfyUI和WebUI，我的意思是，为什么不呢？它们都有自己的优点！

COMFY UI的空白开始界面：

![](img/9d802c54a35dafe0a06efe0f5756db7b.png)

基础而又关键的操作：添加节点

![](img/7ddfae2f2fce7c45937ed2a1403306e3.png)

你也可以在空白区域双击鼠标左键，弹出搜索窗：

![](img/d9ec80af1c79e8e6aa7c9585bf76975e.png)

当然，目前来说，你看到的都是英文，但如果你是通过我的下载包来下载的，那么实际上里面已经带了管理器和汉化包。

![](img/adb028e0cfa95adb4c78018c9997fcc2.png)

只要切换了语言，界面马上就友好多了。

![](img/6feb3ceb14076db0d1e3398def80afc7.png)

COMFYUI依赖的是工作流。下面是一个最基础的工作流，你可以直接通过“加载默认”调用。

![](img/4ce06ef2dd0857746e19fa6bb4d10289.png)

工作流遵循的是SD模型的基本运行逻辑。也就是文本/图形prompt输入—潜空间（latent）解码这么一个过程。

![](img/532108381bf27e01aeeb68dd80fa274b.png)

![](img/ba744cf36ebf0a82500b46b9d559c065.png)

当然，我们也可以导入更多的工作流，通过菜单加载即可。

![](img/ebc2552b9b72c4ded3c0557c74136387.png)

工作流这玩意，一半靠收集，另一半靠自己设计。有很多神仙工作流，并不轻易外传。

举个例子，看看上面这个工作流的复杂程度，毕竟，一个好的工作流需要作者花非常多的心思投入。

![](img/143069cc1080fc189ccdc2f63f4d10b1.png)

和SDWEBUI相比，COMFYUI也有庞大的插件节点体系，每个人可以自行安装很多节点，目前能在节点管理器里面自行安装的节点有200多类，很多是有一定交叉重复的，不可能完全装上（完全装上还会冲突）；因此并不推荐上来就问：“是不是节点都全”

![](img/5fccb243eca1b98220627a92d5650636.png)

一般来说，节点都是按需安装，特别是你的工作流缺了啥就装啥；这里有个大原则，就是用得上才装；根据个人装过近百个节点的经验来说，贪多嚼不烂在这里是真理，能满足工作流需要的就是足够的。

![](img/21267137342e0a74022bff50b9341931.png)

这里有一个技巧，你可以安装浏览器上的翻译插件，通过翻译插件来直接翻译节点说明。

## COMFY-UI基础篇2：文生图/图生图节点教程

> 链接：https://mp.weixin.qq.com/s/gNUg31qej5nyO-Pgdev6CQ

首先，我们大致要理解每一个工作流的基本流程。下面这个图就是典型的1.5模型生成过程。

![](img/ca94b3bf0c8fc4e4dd2223a1c45643ca.png)

SD的核心来源于Latent Diffusion这个工作。

SD在UNet中引入text condition来实现基于文本生成图像。它先采用一个autoencoder将图像压缩到latent空间。然后用扩散模型来生成图像的latents，最后送入`VAE`的decoder模块就可以得到生成的图像。

![](img/bf0490e91ffd888c2a9c53b9a3b7b7e2.png)

一个典型的文生图工作流，也是完全遵循上述过程的。

在上面这个最简单的基础工作流里面，加载模型之后，有绿色的正向文本和红色的负向文本，还有一个紫色的空Latent带着目标尺寸和批次。

![](img/25612113c9146c2117a3871764e68ee8.png)

它们一起流向`KSampler`，采样器里面的参数，其实大家用过AI绘图的都会很熟悉。

![](img/8024fd9f7cb6b83c31bfcc1e7e97a884.png)

做个简单的总结，文生图的工作流基础万变不离其宗。怎么都脱离不了下面这个逻辑关系:

![](img/016ccd109233c28ff60e5d63f0c80e9c.png)

同理，我们马上可以类推出图生图的逻辑图关系：

![](img/ddb59a287d5d17e3f5b1c18758bbaed4.png)

显然，在上面的文生图工作流里面，我们只需要做一点点小改动，就能把它变成图生图的流程。

![](img/372e3ba3e4b16654a8a44d6754ad1c00.png)

这个流程的本身，就是加入了一个图像的参考。流程的本质就是让图像变成一个latent的输出。

![](img/782c9792ebf0d39794ff2430ab866855.png)

对比文生图的流程可以就可以发现，文生图的流程里面用到了上图的空latent，也就是潜空间信息是空的，而图生图里面则有很多来自图片的信息。

本质上，文生图的流程是主要把文本作为模型的参考生成；而图生图则是把图像的信息和文本的信息给到了模型去参考生成。

到这里，大家应该已经对文生图和图生图在COMFY-UI里面的基本流程有所认识了

## COMFY-UI基础篇3：LORA接入工作流

> 链接：https://mp.weixin.qq.com/s/_cM7lG_RHpFhsIiia0BPMw

![](img/71fa8c1019db85beab490a73ee8a9ac7.png)

我们今天用到的主要节点包括：

加载LORA模型：首先，您需要在加载主模型之后加载LORA模型。我涉及的参数还包括模型强度和`clip`强度，以及设置相关的强度参数。

从上面的工作流程图中，可以看到LORA模型被加载了两次，它们分别对应两个不同的lora模型。你也可以增加更多的LORA模型。

![](img/95ce2bfe0752acf9233f7a7e620b97ca.png)

文本编码：在这个例子中，提示是关于“一位坐在火焰中的女性，拥有黑色的头发，是中国道教的形象”。

加载两个LORA之后的`CLIP`信息作为最终节点和文本编码`CLIP`链接。

![](img/45086b33b9dc96325481d1e1c77cd7b5.png)

图像生成：编码后的文本提示将传递到`KSampler`，这是一个生成模型的组件。您可以设置种子、步骤数、配置等参数来影响生成的图像质量和样式。

图像解码：生成的潜在图像会传递到`VAE`解码器，将这些向量解码成可视的图像。

![](img/033ed13ebd3a1d49fb4028adf58c4b9e.png)

其他部分，和一般文生图模型一样，遵循`KSAMPER`到`VAE`解码到图像生成的流程。非常简单。

![](img/a4f853c0fc9fcb5607fc5701c5cca101.png)

![](img/157c7982715117dd0581155fffce3927.png)

![](img/6170e9077c568c707c543376df26c8b0.png)

最后，结果如上。可以看到，生成了一个坐在火中的道士，而后面的神影，则是其中一个lora的独有效果。

这个流程对于初学者来说很重要，因为初学者要想出好图，掌握lora是第一步哦。

![](img/6887d7bd405b80f0486aad6a351dbf66.png)

也许有人认为，要添加多个LORA的时候会很繁琐，其实在社区节点库里面也提供了简便选择。比如CR LORA STACK这种，一个节点就可以选择3个LORA，还带有开关。

所以，COMFY-UI很多功能的实现其实是多样化的，通往终点的路往往不止一条，大家掌握以后，慢慢就能自己参与到工作流的设计之中。

## COMFY-UI基础篇4：高清放大，学了永远不亏

> 链接：https://mp.weixin.qq.com/s/6HWmYkSi4FQgpDy16JlKvw

高清放大，可以说它是AI绘画/视频里面最简单最核心的基础特效之一。

在SDWEBUI里面，我们有一篇经典教程：

AI绘画教程进阶篇（7）:图为什么没别人好看？因为你不会高清放大！

COMFY-UI里面，你也需要学会如何实现高清放大。

![](img/283c1450fc548a3257e892eef10da553.png)

在webui里面，高清放大的经典菜单选项是上图这样的。

放大方法，主要分为了`LATENT`（潜空间）系的放大和外置模型的放大方法。

而在COMFY里面，这两种方法被区分得非常明显：

![](img/f85e7e00923db0425c056537537d13a5.png)

在`KSampler`和`VAE`之间简单增加upscale latent模块，就可以实现图像的1.5倍放大。

![](img/4e06180451cc4dc6e0836a6b77a02b56.png)

而如果使用外置模型来放大，流程会稍有不同，上面是一个参考的工作流。

可以看到，用外置模型放大，放大的步骤是放在`VAE` decoder之后，也就是先生成了低分的图，然后再基于图来放大。

以上，就是比较普通常用实用的放大方法。

那么，有没有更多更高级的玩法呢？有！

![](img/1ffe1e1950d899887a36a7318c2d1744.png)

在comfy UI里面，WEBUI上常见的各种方法，很多都是存在的，并已经集成到一个节点里面。比如，上面就是webui里面常见的 TILED 放大，也就是增加细节的放大。

那么，还有更复杂的吗？

![](img/e5b39b79778d4cd8b13d6138ca784c06.png)

其实也还是有的。比如，上面是用模型放大，不过是使用了两个模型分别放大，然后再用一个`imageblend`的融合算法，将分别放大的图片合成在一起，这样可以最大限度保留/增加细节。

而这种充满自由度和工程创意的操作，在WEBUI里面就很难实现了，这也充分体现了COMFYUI的优点所在。

## ComfyUI基础教程5 之如何在工作流中调用ControlNet

> 链接：https://mp.weixin.qq.com/s/QGBVFkFdbvi3z-dGH4_h0Q

大部分时候，我们在图生图、图生视频、视频生视频这些工作种，希望原图的一些内在元素能够“传递”给生成目标。这个时候，`ControlNet`是少不了的。

理论上来说，当我们使用`ControlNet`提取图像数据后，当我们想要进行描述时，`ControlNet`的处理会符合我们想要的结果，

但实际上，单独使用`ControlNet`时情况并不那么好。

以下是我使用的提示，

adult girl, (extreme detail face), (white bikini), bikini, short top, indoor, in front of sofa, bed, window, (bedroom),

然后我们看三个`ControlNet`的输出，

开放姿势（openpose)

![](img/d50dd770bb66ebdfb6858abaa2d5bb5d.png)

深度

![](img/027200e27e96040a9c9ba31359987b80.png)

线条艺术(lineart)

![](img/52d79fa83ea85e1be301108b3c09dc43.png)

你会发现OpenPose对于体现文字提示是最有意义或者说最有力的，其次是Depth，它仅具有合理的背景，最后是Lineart，它仅具有合理的背景。

因为openpose保留的只是姿势，从而给了AI更大的发挥空间；而深度保持了人体整体轮廓和一部分衣服的深度信息质感；lineart保留得最多，特别是头发的光影细节甚至空中的雪花都有保留。

那么，怎么解决呢？

用过SDWEBUI的朋友都会轻车熟路：那就是联动使用多个`CONTROLNET`。

在之前的WEBUI案例，我们讲过这些：

而在COMFY-UI里面，思路是相同的，不过节点的连接有点考人。下面给个范例。

![](img/1b764786aaca9b79e35ba4f7ceed1024.png)

注意红箭头的工作流走向。蓝绿模组之间是串联。

其实，蓝绿的模块组是一样的，都是分别调用了一个`ControlNet`。我们把内部模块也用不同颜色标注之后，你会理解得更加清晰。

和在webui一样，我们不建议过多的开启`ControlNet`，一般2个足矣，3个已经是极限。

更重要的是，你要知道你开不同的`ControlNet`组合，是为了实现什么。

另外，开启了不同的`ControlNet`，它们的权重参数就变得重要；你完全可以前半段把openpose调高，下半段把lineart调高。

![](img/90bb1d0e75f65b9b5b7dd59f1aa474b4.png)

在下一节，我们会来介绍`IP-Adapter`，在某种程度上，它和`ControlNet`很相似，比如在webui里面，我们一直是在`ControlNet`的选项卡里面调用`IP-Adapter`模型的。

但是在comfyUI里面，`IP-Adapter`的应用范围远比WEBUI里面大；比如用一张参考图实现风格转绘的视频转绘，`IP-Adapter`就主导了整个风格转换的核心。

由于目前视频的转绘基本已经以comfyUI作为主流了，因此我们非常建议大家要掌握这些基本的核心模块操作。

## Comfy-UI基础教程6: CONTROLNET的组合使用

> 链接：https://mp.weixin.qq.com/s/bg_rd5fUjyZvJgafxhgb8Q

今天我们来讲讲高清放大。

这是一个被很多人忽视的核心功能。



# 图像高清放大（Upscale）基础

很多小白问，参数都一样，图为什么没别人好看？因为你不会高清放大！其实，很多人的作品能不能拿出去用，放大（`upscale`）很关键！

毕竟，SD 的基础模型大小也就 512*512，哪怕你用了最新的模型，不崩坏的极限也就是 1024*768。

在这种情况下，高清放大就非常重要了。毕竟大部分商用需求，是需要 2K 以上分辨率的。

目前，常用的放大方法主要有：

## 一、通过 EXTRAS 附加功能菜单放大

![](img/30c58065f7d413f1ae6b680cb2328a92.png)

这里面可以用到一堆模型实现最大 4 倍的缩放。

这种方法的优点是，不用教，你也能会。

缺点自然是效果，在最好的算法下，能无损扩大图片；但并不能有效解决尺寸放大后带来的细节缺失问题。

于是，我们又有了第二种常用的放大方法。

## 二、HIRES.FIX

这是一个可以在生成阶段就实现高质量生产的功能。

![](img/f68eb685f4a6da6b2261fa69635ecf2a.png)

这里面最特殊的，是引入了潜变量高分。

![](img/fa2857f0bf3bdb6ac1888fa96a6b2f78.png)

和传统的其他 AI 放大算法不同，潜变量的高分是在生成过程中不断调整的，本质上是一个用图生图的方式，重绘扩大的过程。

因此，从它的结果来说，它相当于让很多显存不够的显卡，也能通过多步走的方式生成大图。

使用 `hires.fix` 的最大好处是方便，从操作上说，设好参数，点一下即可。但潜变量的算法众多，从实际使用效果来说，在放大倍数在 2 倍以下时效果最佳。（参数请自行摸索）。

一般而言，`hires.fix` 的价值在于，它可以比较好的基于 512*768 规模的图像输出 1024*1536 的大图，在 1024*1536 的基础上，再通过 `EXTRAS` 里面放大，就可以实现 4K 级别大图的效果。

![](img/4324a892753077088d66f4e17c380b69.png)

1K 级别原图

![](img/acd27f67b5ac88aac3de64ba882c6308.png)

1K 原图

![](img/86a8b90bd9c3414554586e1fa7a53d3f.png)

经过 2 次放大，达到 4K 级别的原图细节，仍然丝毫不丢失。

目前，`hires.FIX` + `EXTRAS` 放大是常用的主流玩法，如果是对高清图有需求的，可以考虑用 `comfy.UI`，它有专门的流程，供小白们使用。

后面，我们还会介绍另一种新的放大算法，效果同样是爆炸级别的。

近期，大厂新出的功能非常多，`chatGPT` 热度高，很多小白入场，还有很多赚流量的人，学了两三天就出来讲课割韭菜。但我们还是那句话，AIGC 虽然已经很简易了，但他是一门全新的学科。`webui` 也不是傻瓜式的软件，是需要付出学习成本的，并不是说每一个人一上手就能跑出这么好的效果。每个人要从入门到精通都是需要付出时间和精力的。

# 8.comfyUI 基础教程 7：IPAdapter，从风格转换到无所不能

`IPAdapter` 是 SD 开源社区自 2023 年下半年推出的风格迁移系列模型，由于能力非常强大，现在已经成为 `comfyUI` 体系里面实现各种功能不可或缺的关键模块之一。

包括国内大厂开源的 `photomaker` 和 `instantID`，本质上都是

举个例子，和之前讲过的 `ControlNet` 结合使用，`IP-Adapter`（用于风格）和 `ControlNet`（用于构图），能够实现视频完美的转换风格。

首先，使用 `IP-Adapter` 在 `/ComfyUI/models/ipadapter` 目录中安装所需的 `IP-Adapter` 模型。

![](img/96d77fc25e8173f4cb4b828935465157.png)

![](img/3accf73f3edff72420720301342dc8b2.png)

可以看到，`IP-Adapter` 模型数量非常多，初学者会看晕。

详细的介绍如下：

+   `/ComfyUI/models/clip_vision`

+   `CLIP-ViT-H-14-laion2B-s32B-b79K.safetensors`，下载并重命名

+   `CLIP-ViT-bigG-14-laion2B-39B-b160k.safetensors`，下载并重命名

+   `/ComfyUI/models/ipadapter`，如果不存在则创建它

+   `ip-adapter_sd15.safetensors`，基本模型，平均强度

+   `ip-adapter_sd15_light_v11.bin`，轻型影响模型

+   `ip-adapter-plus_sd15.safetensors`，Plus 模型，非常强大

+   `ip-adapter-plus-face_sd15.safetensors`，脸部模型，肖像

+   `ip-adapter-full-face_sd15.safetensors`，更强的人脸模型，不一定更好

+   `ip-adapter_sd15_vit-G.safetensors`，基础模型，需要 `bigG` 剪辑视觉编码器

+   `ip-adapter_sdxl_vit-h.safetensors`，SDXL 模型

+   `ip-adapter-plus_sdxl_vit-h.safetensors`，SDXL Plus 型号

+   `ip-adapter-plus-face_sdxl_vit-h.safetensors`，SDXL 人脸模型

+   `ip-adapter_sdxl.safetensors`，vit-G SDXL 模型，需要 `bigG` 剪辑视觉编码器

此外：

大多数 `FaceID` 型号都需要 `LoRA`。如果您使用它，`IPAdapter Unified Loader FaceID` 并且遵循命名约定，它将自动加载。否则，您必须手动加载它们，请注意每个 `FaceID` 模型都必须与其自己特定的 `LoRA` 配对。

+   `/ComfyUI/models/loras`

+   `ip-适配器-faceid_sd15_lora.safetensors`

+   `ip-适配器-faceid-plusv2_sd15_lora.safetensors`

+   `ip-adapter-faceid_sdxl_lora.safetensors`，SDXL `FaceID` `LoRA`

+   `ip-adapter-faceid-plusv2_sdxl_lora.safetensors`，SDXL 加 v2 `LoRA`

但总体来说，无非分为两类，一类是早期不带 `PLUS` 的，基本已经弃用。

另一类是带 `PLUS` 的，目前主力；其中又分为普通的 `PLUS`，和带 `face`（主攻面部特征）的。

`PLUS` 里面，又根据 `SD` 模型体系分为了 1.5 体系和 XL 体系，分别适配两代模型。

所以，你只要记住，用风格迁移，就用不带 `FACE` 的，希望换脸，就用带 `FACE` 的。大致就可以了。

那么，`IP-Adapter` 应该怎么用呢？

需要重点指出的是，在 3 月份，`IP-Adapter` 在 `COMFYUI` 的代码经历过一次全面重写，导致前后不再兼容，3 月之前称之为旧版，3 月之后称之为新版。

![](img/9d1debb71a8cfbdb29ad61293081651c.png)

以上是旧版的 `faceID-V2` 的适配，由于我的节点已经更新过，老工作流里面的 `V1` 节点已经不再获得支持。无法再使用。

![](img/85dfbc04ddf19964d5561f6162f34e10.png)

新版本实现同样功能的节点，明显更加简洁。用户无需再自行设置难记的模型搭配。

![](img/6ab5d39c2d66a183cdf15becc2f656d0.png)

只需要选择不同的用法，节点内已经自行配置好了各种用法搭配。

其实，关键还是要找准办法。在 `IP-Adapter` 的节点文件夹里面，我们可以找到 `example`，这里面，作者已经给出了示例。

![](img/2cf22c7da0323ed81db83383c5f93cdf.png)

导入示例工作流，即可看到不同的搭配用法。

一般来说，我们常用的主要有下面几种功能：

![](img/3b58a329a2ea05564940bbb975d6491d.png)

![](img/615809f69668093a424974a7c3932908.png)

![](img/f72cf2790768eac645726256072cd168.png)

![](img/53941b52ee346a70a344da59fcd52d40.png)

以上工作流都可以在 `openart.ai` 找到。当然，访问这个网址需要魔法。

它的好处是什么呢？

显而易见的一点是，有了 `IPAdapter`，我们不一定需要训练风格 `LORA` 了。

`IPAdapter` 的效果越强，训练 `LORA` 的必要性就越低。

另外，在工作流体系里面，它可以变化的玩法太多了。以后我们出视频拆解工作流的时候，再逐一细讲。本篇作为基础教程，就讲到这里。

要熟悉 `IP` 的玩法，就必须自己去把示例工作流看一遍，因为它还是一个经常频繁更新的技术模块，因为保持一定的更新了解是必要的。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
