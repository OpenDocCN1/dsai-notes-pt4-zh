
# 基础知识

> 来源：[https://geekbang.feishu.cn/docx/BqHkd3hJWosPAkxXn6gcyTaWncg](https://geekbang.feishu.cn/docx/BqHkd3hJWosPAkxXn6gcyTaWncg)

2014年，Ian Goodfellow及其同事首次介绍了生成对抗网络，开启了使用深度学习生成图像的新时代

> 生成对抗网络（GAN，Generative Adversarial Networks）是一种由Ian Goodfellow在2014年提出的深度学习模型框架，它通过让两个神经网络——生成器（Generator）和鉴别器（Discriminator）——相互竞争来生成新的、尽可能接近真实的数据样本。

# Stable Diffusion 概述

Stable Diffusion是一个深度学习文本到图像生成模型，它于2022年8月22日首次发布，由StabilityAI、CompVis与Runway合作开发，并得到EleutherAI和LAION的支持。截至2022年10月，StabilityAI已经筹集了1.01亿美元的资金。Stable Diffusion是一种潜在（Latent variable model）扩散模型，主要用于根据文本的描述产生详细图像，尽管它也可以应用于其他任务，如内补绘制、外补绘制，以及在提示词指导下产生图生图的转变。

Stable Diffusion的源代码和模型权重已分别公开发布在GitHub和Hugging Face，可以在大多数配备有适度GPU的电脑硬件上运行。这与以前的专有文生图模型（如DALL-E和Midjourney）不同，后者只能通过云端运算服务访问。

Stable Diffusion模型支持通过使用提示词来产生新的图像，描述要包含或省略的元素，以及重新绘制现有的图像，其中包含提示词中描述的新元素（该过程通常被称为“指导性图像合成”）通过使用模型的扩散去噪机制（diffusion-denoising mechanism）。此外，该模型还允许通过提示词在现有的图中进行内补绘制和外补绘制来部分更改，当与支持这种功能的用户界面使用时，其中存在许多不同的开源软件。

Stable Diffusion建议在 10GB以上的显存（GDDR或HBM）下运行，但是显存较少的用户可以选择以`float16`的精度加载权重，而不是默认的`float32`，以降低显存使用率。与DALL-E等模型不同，Stable Diffusion提供其源代码以及预训练的权重。用户拥有其生成的图像的权利，并可自由地将其用于商业用途。

# 开始上手

> 一般都是使用 UI 界面操作，如果熟悉 `python` 的话，可以直接使用 `diffusers` 进行开发

## Stable Diffusion WebUI

> 官方地址: https://github.com/AUTOMATIC1111/stable-diffusion-webui

Stable Diffusion WebUI 是一个基于 Web 的用户界面，用于访问和操作 Stable Diffusion 系统。Stable Diffusion 是一个用于生成高质量图像的技术，它通过模拟物理过程来创建逼真的图像。WebUI 提供了一个直观的界面，使用户可以轻松地上传图像、设置参数、启动生成过程，并在完成后查看结果。

Stable Diffusion WebUI 的主要特点包括：

+ 直观的用户界面：用户可以通过易于使用的界面上传图像、选择生成参数，以及查看生成过程和结果。

+ 强大的生成能力：利用 Stable Diffusion 技术，用户可以生成高质量、逼真的图像。

+ 灵活的设置：用户可以根据需要调整生成参数，以达到最佳的生成效果。

+ 实时反馈：在生成过程中，用户可以实时查看生成的图像，并根据需要进行调整。

Stable Diffusion WebUI 是一个强大的工具，适用于需要生成高质量图像的各种应用场景，包括艺术创作、游戏开发、广告设计等。通过提供直观的用户界面和强大的生成能力，Stable Diffusion WebUI 使得用户可以轻松地利用 Stable Diffusion 技术，创造出令人惊叹的图像作品。

相关教程和资料链接:

[1] https://zhuanlan.zhihu.com/p/609068441

[2] https://juejin.cn/post/7250667613021782053

[3] https://blog.csdn.net/Taobaojishu/article/details/132241942

[4] https://www.swiftcafe.io/post/stable-diffusion-webui

[5] https://www.stablediffusion-cn.com/sd/sd-knowledge/3535.html

[6] https://openai.wiki/stable-diffusion.html

[7] https://www.yizz.cn/2332.html

[8] https://www.tjsky.net/tutorial/488

[9] https://zhuanlan.zhihu.com/p/570954565

[10] https://civitai.com/articles/1299/stable-diffusion-webui

## ComfyUI

> 官网地址: https://github.com/comfyanonymous/ComfyUI

Stable Diffusion ComfyUI 是一个基于节点流程式的 Stable Diffusion AI 绘图工具 WebUI，它可以被视为集成了 Stable Diffusion 功能的 Substance Designer。通过将 Stable Diffusion 的流程拆分成节点，实现了更加精准的工作流定制和完善的可复现性。然而，节点式的工作流也提高了一部分使用门槛。ComfyUI 的内部生成流程做了优化，生成图片时的速度相较于 `webui` 有了 10%~25% 的提升，生成大图片的时候不会爆显存，只是图片太大时，会因为切块运算的导致图片碎裂。ComfyUI 虽然自带了 `PyTorch` 和 Stable Diffusion 环境，但是不包括模型，所以需要将模型放置到 ComfyUI 对应目录中。

ComfyUI 提供了一系列插件来增强其功能，包括基于 ComfyUI 的更加定制化的 UI 界面 `CushyStudio`，`Davemane42's Custom Node for ComfyUI`，`ComfyUI-Custom-Scripts`，`ComfyUI-extra-nodes` - quality of life，`ComfyUI Vextra Nodes`，`ControlNet Preprocessors for ComfyUI`，`Derfuu_ComfyUI_ModdedNodes` 等。这些插件提供了图片自动分割，历史缩略图显示，锁定节点或组，`lora` 文件夹视图，图片自动生成关键字，添加 ComfyUI 的平板触控功能，一键自动排版等功能，以及对图片的后处理，如生成时播放音乐，在图片中添加文字，添加 Instagram 滤镜等。

ComfyUI 的基础流程包括模型输入，图片输入，`clip` 跳过，`clip` 编码器，`lora`，`ControlNet`，`ksampler` 节点，负责在潜空间生成图片，`Vae` 解码阶段，将生成的图片从潜空前转换成 RGB 色彩空间，以及保存和后处理阶段。用户可以通过点击面板右侧的工具栏中的 `Queue Prompt` 开始图片生成。

相关教程和资料链接:

[1] https://zhuanlan.zhihu.com/p/620297462

[2] https://www.uisdc.com/stable-diffusion-comfyui

[3] https://www.stablediffusion-cn.com/sd/4449.html

[4] https://www.youtube.com/watch?v=zyIukd4TOdA

[5] https://www.uisdc.com/stable-diffusion-comfyui-2

[6] https://www.comflowy.com/zh-CN/basics/comfyui-foundation

[7] https://www.wehelpwin.com/m_article/4506

[8] https://openai.wiki/comfyui-install.html

[9] https://aws.amazon.com/cn/blogs/china/stable-diffusion-comfyui-deployment-solution-based-on-amazon-eks/

[10] https://hjcenry.com/archives/ai%E7%BB%98%E7%94%BB-stablediffusion-comfyui%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B
