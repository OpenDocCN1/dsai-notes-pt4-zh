
# 工作流 ComfyUI

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/LBgqdbH7QoMhW1x0GP7cRYcEnYd](https://xatc0v8uz5m.feishu.cn/docx/LBgqdbH7QoMhW1x0GP7cRYcEnYd)

[https://learn.modo.art/watch/11491831](https://learn.modo.art/watch/11491831)

![](img/d3be1554bcb41c8d7c4204d2ea57a018.png)

## 基础操作

双击屏幕搜索

`KSampler`

![](img/9808e43c1f2fb15689f07a5f8cdb8289.png)

![](img/85d1f378f2a3783670de45e60ff1de9d.png)

## 种子与步数

种子，随机种子

种子决定了生成结果的随机基础

![](img/bca748f0fa1f3a35a9132c45d7855e38.png)

步数，20-30，迭代次数

DFG，4-7，听话程度

![](img/15438aa0320f70b3cf894e4663b82413.png)

## 采样器

采样器

`SD1.5` 和 `XL` 模型生图，基本上都是用 `euler` 和 `DPM++SDE`

动漫风格用 `eulera` 效果通常更好

写实画风，用 `DPM++` 系列

常用的 `Euler` `eulera` 稳定可靠

`eulera` 适合动画

![](img/26b2b2d06274c37261b9c2529b298643.png)

`DPM++` 系列是对基础 `DPM` 的改进

![](img/daeb02026d7dcd111610cec2c285aab4.png)

在这里 `P` 就是 `Plus` 加强 `+` 的意思

`DPM++SDE` 则是进一步结合了随机微分方程，使得画面看起来更加流畅

`SDE` 是让模型有更多的发挥空间

## 调度器

调度器

它是一个收敛性质的调度器，决定了降噪的曲线

刚开始的时候，可以先尝试一下 `DPM++SDE` 和 `karras`

## CheckpointLoaderSimple

`CheckpointLoaderSimple`

![](img/b865a213103f4dff8cecfc9f0c97bba0.png)

`CheckpointLoaderSimple` 是 ComfyUI 中用于加载模型权重（`checkpoint`）的节点。

在 ComfyUI 里，`CheckpointLoaderSimple` 是一个基础节点，主要功能是加载 Stable Diffusion 或其他兼容模型的 checkpoint 文件（如 `.ckpt` 或 `.safetensors` 格式）。这个节点通常是 ComfyUI 工作流的起点之一。

主要作用

- 加载模型权重：将本地存储的模型文件加载到 ComfyUI 的工作流中。
- 支持多种格式：常见的如 `.ckpt`、`.safetensors` 等 Stable Diffusion 相关模型文件。
- 便于切换模型：你可以通过这个节点快速切换不同的模型权重，适应不同的生成需求。

使用场景

1. 在 ComfyUI 的工作流中添加 `CheckpointLoaderSimple` 节点。
2. 选择你想要加载的模型文件（通常放在 ComfyUI 的 `models/checkpoints` 目录下）。
3. 该节点会输出模型权重，供后续的图像生成节点使用。

![](img/faa4f60a254173e9f0468421ee286b33.png)

![](img/34b6d9671bc11d46f2807d11e8a717b4.png)

## CLIP TextEncode

`CLIP TextEncode` 文本编码器的组件，是人类语言和大模型之间的翻译器。

![](img/efed893f865e64b71d0408d4609feead.png)

![](img/04c57a35a4c150904d9f1c4541df1be6.png)

浅空间，相对于像素空间，是大模型内部用于计算的表达方式

## EmptyLatentImage

`EmptyLatentImage`

![](img/c1e0ff8e3544cde1690e741c59f81c13.png)

## VAEDecode

`VAEDecode`，大模型将数据转换成人类能看懂的图像，这个解码器就叫做 `VAE`

一般大模型 `1.5` 和 `XL`

![](img/9fa5df224cd1d2a4b3122369a98a5cd1.png)

![](img/d98386d86bc926453d9759f6cb103776.png)

![](img/9b5742b06af2d4b4c73454ad1ab9ab51.png)

## 工具网站

1. 哩布哩布 AI (LibLibAI)

   - 主要功能：这是一个专注于 AI 模型和作品分享的社区，可以看作是面向国内用户的“C 站”（Civitai）。
   - 作用：用户可以在这里下载和分享 AI 绘画的“基础大模型（`Checkpoints`）”和 `LoRA` 模型，以及查看和学习他人的作品与提示词（`Prompt`）。它的服务器在国内，访问速度快，内容也更符合国内用户的使用习惯。
   - 网址：https://www.liblib.ai/

1. NovelAI

   - 主要功能：AI 辅助故事写作和二次元风格图像生成。
   - 作用：NovelAI 最初以其强大的 AI 续写故事功能闻名。后来，它推出的二次元动漫风格的图像生成模型因其独特的画风和高质量的生成效果，对整个 AI 绘画社区产生了深远影响。许多模型的画风都受到了它的启发。这是一个付费订阅制的服务。
   - 网址：https://novelai.net/

1. Prompthero (图片中写为 PromIib)

   - 主要功能：AI 绘画提示词（`Prompt`）搜索引擎。
   - 作用：当你不知道如何写出好的提示词来生成想要的画面时，可以来这个网站寻找灵感。它收录了海量由 Midjourney、Stable Diffusion、DALL-E 等不同模型生成的图像，并附带了生成这些图像所使用的完整提示词。
   - 网址：https://prompthero.com/

1. Civitai (俗称 C 站)

   - 主要功能：全球最大、最活跃的 AI 绘画模型分享社区。
   - 作用：这是目前下载 Stable Diffusion 相关模型（包括基础大模型、`LoRA`、`LyCORIS`、`Embeddings` 等）的首选网站。社区非常活跃，模型种类极其丰富，覆盖了写实、动漫、3D、特定角色、画风等几乎所有你能想到的领域。
   - 网址：https://civitai.com/

1. GitHub

   - 主要功能：全球最大的代码托管和开源项目协作平台。
   - 作用：在 AI 绘画领域，GitHub 是所有核心软件和工具的“家”。例如，我们常用的 `ComfyUI`、Stable Diffusion WebUI (AUTOMATIC1111) 等软件的源代码都托管在这里。开发者在这里发布更新，用户也可以在这里找到各种插件、脚本和前沿的技术实现。
   - 网址：https://github.com/

1. Hugging Face (抱抱脸)

   - 主要功能：面向机器学习的“GitHub”，是模型、数据集和代码的中心枢纽。
   - 作用：许多官方或研究机构发布的原始、foundational 的 AI 模型会首发在 Hugging Face 上，例如 Stability AI 官方发布的 Stable Diffusion 系列模型。它更偏向于研究和开发者，是寻找最源头、最权威模型和数据集的地方。
   - 网址：https://huggingface.co/

1. JC

   - 主要功能：一个中文 AI 绘画提示词分享和学习社区。
   - 作用：和 Prompthero 类似，但它是一个中文社区，用户可以在这里分享自己的作品和完整的 `Prompt`（包括模型、参数等），方便其他用户学习和模仿，快速提升出图效果。
   - 网址：https://www.jc.house/

## 下载与调试

comfyUI 下载

https://www.comfy.org/zh-cn/download

20250902

![](img/f087036a32dc37249ce607511aa56fb3.png)

![](img/fa1d136197588dcbaa5f98b89ce07c27.png)

![](img/dc884a5109059f00d5dcf9864d5007ea.png)

![](img/92446e6d9135750cb5f1bf3a06aa0224.png)

![](img/0bafbc4698ad343bdde67a1b9d5739e5.png)

![](img/2593b4624a50f26fe5f5f48a819514ec.png)

调试 2

![](img/44a75576791b71d1f1a83dc5fd55790a.png)

![](img/0e531447337f566e41dc5cf3e3705747.png)

调试 3

![](img/e9c856d226a4711784e0c590da8f63bd.png)

![](img/fbc122e5f7383ccf0eb596af5102a13f.png)

![](img/40f5c8e6659e5108dd3b3883f833b7d7.png)

![](img/a64d613f698f66fc5d1d4b7df3326d39.png)

![](img/1d72621b3e5378549118e0b1d2b934ae.png)

![](img/256c47bc3d70e386cb8dfdbb5ea367b1.png)

![](img/d94419aa452aedbfb14bb66baa18b8e2.png)

![](img/ae38bd102f01c754636e4b37241ca693.png)

你应该可以搜索到并添加以下节点了：

- `Efficient Loader` (效率加载器)
- `KSampler (Efficient)` (K 采样器（效率）)
