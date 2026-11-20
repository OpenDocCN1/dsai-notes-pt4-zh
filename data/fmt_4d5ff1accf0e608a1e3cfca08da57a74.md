
# ComfyUI | 5分钟部署最新Flux大模型

> 来源：https://aigcreative.feishu.cn/docx/Jq6Vdbes7ofiI8xq5L2cZboNn1c

> 🔗 原文链接：https://mp.weixin.qq.com/s/HcVCAS1sF7qwo7jW4rhKvQ

Midjourney 和 Stable Diffusion 都是目前流行的 AI 图像生成工具，它们能够根据文本描述生成高质量的图像。都是基于深度学习技术的文本到图像生成模型，但它们各自基于不同的大模型。

但最近推出了一款比前两者更强大，生成图像更加逼真，在细节上更符合现实世界的模型，就是FLUX！

## 01、什么是Flux？

Flux AI是由Black Forest Labs推出的一款最新的文本生成图像模型，这个团队是由从Stable Diffusion离开的原班人马，创立的全新用于AI绘图的大模型。Flux AI模型以其卓越的视觉质量、精确的提示词遵循、多样化的风格和复杂场景生成能力而著称。它包含三个版本：FLUX.1 [pro]、FLUX.1 [dev]和FLUX.1 [schnell]，分别针对不同的使用场景和需求。

### FLUX.1 Pro

是专为商业用途设计的闭源模型，提供最先进的图像生成性能。

### FLUX.1 Dev

是一个开源的引导蒸馏模型，适用于非商业应用。

### FLUX.1 Schnell

是专为本地开发和个人使用设计的快速版本。

Flux AI模型采用了创新的混合架构，结合了多模态处理能力和并行扩散机制的Transformer技术，并扩展至高达120亿个参数。这些模型在训练生成模型的方法上采用了流匹配技术，这种方法不仅通用性强，而且概念简洁，特别适用于包括扩散过程在内的各种情况。

官方网站：https://blackforestlabs.ai/

![](img/45e5f183e1049467acfadd5f5d2629e8.png)

官网展示图片样例：

![](img/b4179a2c3f2154b566357d3bf031ce00.png)

![](img/73402fe519fc14ac6c6f64bfc5062284.png)

![](img/54bee81c8166b19eaadf61b33857345f.png)

以下是Flux、Midjourney和Stable Diffusion模型的对比：

### 图像质量

Flux: 不需要其他插件的参与，本身就擅长生成高分辨率、细节丰富的图像，特别是在复杂场景和人体解剖学方面表现出色。

Midjourney: 以其艺术风格和高质量输出闻名，尤其在艺术性和风格多样性上有优势。

Stable Diffusion: 能够生成逼真的图像，适合需要真实感输出的项目。

### 速度和效率

Flux: 提供快速的图像生成，特别是schnell变体，适合需要快速原型制作和迭代设计的过程。大多数情况下不需要额外的下载风格模型，即可输出各种风格的图片。

Midjourney: 未明确提及速度，但通常商业模型可能会在云服务器上运行，可能存在排队等候的情况。

Stable Diffusion: 生成速度较慢，但在图像优化过程中提供更多控制。

### 处理复杂场景

Flux: 在处理复杂构图方面表现出色，得益于其先进的架构。特别是文字方面，flux可以输出带文字的图片，只要提示词足够的准确可以直接输出设计海报级别的图片。

Midjourney: 能够处理复杂场景，但在某些情况下可能需要更多的迭代来达到理想效果。

Stable Diffusion: 在处理复杂场景方面可能存在一些限制。

### 人体解剖渲染

Flux: 在渲染人体解剖方面表现出色，尤其是手部细节，更加的完整还原。

Midjourney: 未特别提及，但通常能够生成具有艺术感的人物图像，难以指定准确描绘。

Stable Diffusion: 可能在准确描绘人体特征方面遇到困难。需要额外的插件或者后期的处理才能勉强更正或基本达到效果

### 灵活性和集成

Flux: 提供多种变体，适合不同的使用场景和需求，包括开源和专业模型。

Midjourney: 作为商业工具，可能在定制化方面有一定的限制。

Stable Diffusion: 开源模型，社区驱动，提供了丰富的定制化和集成选项。

### 开源与商业模型

Flux: 提供开源模型，鼓励社区参与和创新。

Midjourney: 为商业模型，提供专业的图像生成服务。

Stable Diffusion: 开源模型，有活跃的社区支持和持续的改进。

### 特定应用

FluxI: 适合需要高细节和准确表现复杂场景的项目。

Midjourney: 适合艺术创作和设计，尤其是在需要艺术风格和创意表现的领域。

Stable Diffusion: 适合对最终图像控制至关重要的真实感输出。

### 总结来说flux

（1）画面更细节，效果更好

（2）图片文字支持更突出完整，输出更准确

（3）人物手部完整，符合现实，基本不会出错

（4）模型包含风格多，支持风格多样，不依赖额外的模型协助补充

（5）可忽略输入负面提示词，单靠正面提示词即可准确输出

通过以上对比，flux之所以更强，是因为它的训练参数是比前两者更大，stable diffusion 3是最高8B的训练参数，约80亿，而flux 1的开始就是12B的训练参数，足足120亿，多了好几倍！单个模型的大小就有23G大小，可想而知，Flux强大是很有实力的。

## 02、Flux模型的区别：主要分为下面三大模型

![](img/866df1d9d0a65744f97770a280c4b000.png)

![](img/e692417a89d684fb8978ae4d04455d93.png)

![](img/5617ef1bdebf353159c580b718a4b389.png)

![](img/643a1bd998b954f0323bb3ecd02dacd1.png)

*注意：* 如果使用`GGUF`和`NF4`，需额外安装插件：

GGUF节点：https://github.com/city96/ComfyUI-GGUF

![](img/a79ca5e8a0de00428b48825fcfe7532a.png)

NF4节点：https://github.com/comfyanonymous/ComfyUI_bitsandbytes_NF4

![](img/416edc9f092e68fb64be78d7e99f82e5.png)

## 03、如何部署安装flux？

1、保证你的`comfyui`是最新的版本。（本文使用秋叶老师的启动器）

点击启动器的版本，看到顶部的内核、扩展，依次点击右上角的一键更新按钮，更新到最新的即可。因为flux是8月前发布的，保证自己的`comfyui`是更新到8月1日之后的最新版本哦。

![](img/eab94836057a4c0d906f478446bd20ba.png)

2、进入官网的汉堡主页 https://huggingface.com/black-forest-labs/flux.1-dev

![](img/7e625183da07e7a9c1ba0db6041ea8a1.png)

下载 `ae.safetensors`（即`vae`）、`flux1-dev.safetensors`（即`unet`）

![](img/8f0ddd032102deb340bce752dad507aa.png)

3、下载clip文件，进入`comfyui`的github主页找到flux：https://github.com/comfyanonymous/ComfyUI_examples/tree/master/flux

![](img/1d5561c6a787f0867b29079f2bd1cfc5.png)

从这里进入，看到`clip_l.safetensors`、`t5xxl_fp16.safetensors`、`t5xxl_fp8_e4m3fn.safetensors`这三个文件下载。

![](img/7a74541fc4bd540898a9cb7aee4bd84c.png)

![](img/7da5cbbfdc9ac70210c86fd6216d0657.png)

4、文件放置位置：

![](img/7bc3373fba12c4b401a32ffe7bd37c1f.png)

5、其他资源包括`controlnet`及`lora`，在xlab-ai主页 https://huggingface.co/XLabs-AI

![](img/3a3331df8ad20ed0e7232e47f6d55acb.png)

![](img/d2bd9ac2e72369395ee6856f70129c40.png)

*注意* 如想使用`flux`模型的`controlnet`、`flux`模型的`iPadapter`等功能，，还需要在`comfyui`里面下载安装`XLabs-Ai`插件。在`comfyui`管理器里打开节点管理，然后搜索该插件并安装，然后重启`comfyui`即可。

![](img/527b90aa81dd7d04af6165799c9684dc.png)

6、系统内存设置

注意采用的模型运行占内存比较大时，建议打开系统的虚拟内存：系统设置-高级-性能设置-高级-虚拟内存-勾选自动管理。

![](img/2d206632a35c954a2d30de9e60c04aef.png)

## 04、如何快速使用？

官方给出了相关使用的工作流，只需要把官方github的案例图片拖放到`comfyui`即可。看图片命名对应使用，值得我们学习。

![](img/507a88a72ce00fd1b8678c724fd36fd8.png)

官方案例工作流

如用`flux_dev_example`工作流为例子，将图片拖放到`comfyui`

![](img/425a025aa9061dba8b5c0f865746ed48.png)

得到下面的工作流：

![](img/da59336b96bdff696762cb050e34a138.png)

把`flux_schnell_example`拖入则得到如下：

![](img/1b642669eeb35a1d92220d1ca29d34a1.png)

划分结构：

![](img/51ac86371efbe55f39f140abcfacc7d3.png)

`flux_schnell_example`是最为简单的，4步骤即可出图。

从简单的入手，在`flux_schnell_example`工作流里面尝试使用下面提示词：

一个穿着校服的女孩，拿着粉笔在黑板上写着开心文字，真实摄影，学校教室，半身构图，电影灯光，细节丰富，日式低饱和度

A girl wearing a school uniform, holding chalk to write happy words on the blackboard, real photography, school classroom, half body composition, movie lighting, rich details, Japanese low saturation

得到的出图：

![](img/a360cfa5f7019e878f8addf8d21d9dd2.png)

（1）黑板上的字准确体验

（2）人物的手结构正常

（3）质感效果按提示词的真实风格符合

（4）出图效率高，无等待感，跟提示词吻合

接着我们在模型之间插入一个`lora`加载器，并且在网上下载了一个黑神话悟空的`lora`模型，加载进去（`lora`来自`liblib.art`平台作者：AI游戏课堂熊熊）

![](img/30151d25debf294834b258742c3de919.png)

模型先使用`fp8` `schnell`，使用`lora`作者提供的描述词，然后启动队列，看到作者电脑32G内存，可以去到85%到98%上，显卡是英伟达2080。

![](img/01034c01fa3c2d44035a148de59526dd.png)

输入提示词：

悟空，在绘画室内，绘画，素描，画笔，写字，努力工作，（大文字“不想工作”）

Wukong,in the painting studio,painting,sketching,using a paintbrush, writing,and working hard (big text says' don't want to work ')

得到图像：

![](img/172c1f627ea3fadc3bf93f67375b009f.png)

简单的描述，无需负面提示词，添加`lora`后人物手部，文字等都很好。

总的来说`flux`除了大模型需要强配置的电脑，较小的模型其实效果也错不了多少，对于普通的自媒体应用、方案创意等足够使用了。

另外除了简单的添加`lora`等，熟悉`comfyui`的话，添加放大增加细节的节点，甚至使用`controlnet`等高级应用都可以产生更多更好的效果。
