
# 「通用世界模型」Sora技术报告深度解读！

> 来源：[https://aiyanxishe.feishu.cn/docx/DGr5dezNEoJSd6xbhADcTYJbnZf](https://aiyanxishe.feishu.cn/docx/DGr5dezNEoJSd6xbhADcTYJbnZf)
>
> <https://aiyanxishe.feishu.cn/sync/KGQ3dArdRsnl77bJBG4camxWnAg>

2月16日一早OpenAI平地一声雷，悄无声息的发布了**第一个 Text to Video 大模型——Sora**。

从演示看，效果还是相当不错的。具体演示案例可以查看：

![](img/dd6f7a3e805af4c68c84d35c46457367.png)

Sora 的强大之处在于其能够根据文本描述，生成长达60秒的视频，其中包含精细复杂的场景、生动的角色表情以及复杂的镜头运动。这无异于给正在处于春节长假的尾巴国内AI圈丢下了一颗重磅炸弹。这篇文章，我将快速的带大家解读一下OpenAI在官方release的技术报告，希望本文可以让大家能从一头雾水中，寻找和思考一些未来文生视频和文生图领域的技术发展趋势。另外本文不会逐字逐句的去翻译技术报告原文，对原文感兴趣的读者，可以在以下链接中自行食用。

技术报告原文链接：[https://openai.com/research/video-generation-models-as-world-simulators](https://openai.com/research/video-generation-models-as-world-simulators)

![](img/4d773229ede07a972408a4d1331cae36.png)

导读：昨天OpenAI发布了正在封闭测试的Sora，其是文本生成图像的大模型产品。本文为它的生成模型技术报告。

OpenAI 探索了视频数据生成模型的大规模训练。具体来说，研究人员在可变持续时间、分辨率和宽高比的视频和图像上联合训练了一个文本条件扩散模型。作者利用对视频和图像潜在代码的时空补丁进行操作的 transformer 架构，其最大的模型 Sora 能够生成长达一分钟的高质量视频。

OpenAI 认为，新展示的结果表明，扩展视频生成模型是构建物理世界通用模拟器的一条有前途的途径。

We explore large-scale training of generative models on video data. Specifically, we train text-conditional diffusion models jointly on videos and images of variable durations, resolutions and aspect ratios. We leverage a transformer architecture that operates on spacetime patches of video and image latent codes. Our largest model, Sora, is capable of generating a minute of high fidelity video. Our results suggest that scaling video generation models is a promising path towards building general purpose simulators of the physical world.

![](img/01efbdfbfa3a9c46f84f8b85b86397c9.png)

技术报告地址：[https://openai.com/research/video-generation-models-as-world-simulators](https://openai.com/research/video-generation-models-as-world-simulators)，时长00:59

**OpenAI 在技术报告中重点展示了**：（1）将所有类型的视觉数据转化为统一表示，从而能够大规模训练生成模型的方法；以及（2）对 Sora 的能力和局限性进行定性评估。

This technical report focuses on (1) our method for turning visual data of all types into a unified representation that enables large-scale training of generative models, and (2) qualitative evaluation of Sora’s capabilities and limitations. Model and implementation details are not included in this report.

令人遗憾的是，OpenAI 的报告不包含模型和训练的细节。最近一段时间，视频生成是 AI 领域的重要方向，先前的许多工作研究了视频数据的生成建模方向，包括循环网络、生成对抗网络、自回归 transformer 和扩散模型。这些工作通常关注一小类视觉数据、较短的视频或固定大小的视频。

Much prior work has studied generative modeling of video data using a variety of methods, including recurrent networks, generative adversarial networks,4,5,6,7 autoregressive transformers,8,9 and diffusion models.10,11,12 These works often focus on a narrow category of visual data, on shorter videos, or on videos of a fixed size. Sora is a generalist model of visual data—it can generate videos and images spanning diverse durations, aspect ratios and resolutions, up to a full minute of high definition video.

与之不同的是，OpenAI 的 Sora 是视觉数据的通用模型，它可以生成不同时长、长宽比和分辨率的视频和图像，而且最多可以输出长达一分钟的高清视频。

## 视觉数据转为 Patches

大型语言模型通过在互联网规模的数据上进行训练，获得了出色的通用能力，OpenAI 从这一点汲取了灵感。LLM 得以确立新范式，部分得益于创新了 token 使用的方法。研究人员们巧妙地将文本的多种模态 —— 代码、数学和各种自然语言统一了起来。在这项工作中，OpenAI 考虑了生成视觉数据的模型如何继承这种方法的好处。大型语言模型有文本 token，而 Sora 有视觉 patches。此前的研究已经证明 patches 是视觉数据模型的有效表示。OpenAI 发现 patches 是训练生成各种类型视频和图像的模型的可扩展且有效的表示。

![](img/7972200fb1e7b6535186b2441077b15b.png)

**在更高层面上**，OpenAI 首先将视频**压缩到较低维的潜在空间**，然后将表示分解为时空 patches，从而将视频转换为 patches。

We take inspiration from large language models which acquire generalist capabilities by training on internet-scale data.13,14 The success of the LLM paradigm is enabled in part by the use of tokens that elegantly unify diverse modalities of text—code, math and various natural languages. In this work, we consider how generative models of visual data can inherit such benefits. Whereas LLMs have text tokens, Sora has visual patches. Patches have previously been shown to be an effective representation for models of visual data.15,16,17,18 We find that patches are a highly-scalable and effective representation for training generative models on diverse types of videos and images.

### 视频压缩网络

OpenAI 训练了一个降低视觉数据维度的网络。该网络将原始视频作为输入，并输出在时间和空间上压缩的潜在表示。Sora 在这个压缩的潜在空间中接受训练，而后生成视频。OpenAI 还训练了相应的解码器模型，将生成的潜在表示映射回像素空间。

We train a network that reduces the dimensionality of visual data.20 This network takes raw video as input and outputs a latent representation that is compressed both temporally and spatially. Sora is trained on and subsequently generates videos within this compressed latent space. We also train a corresponding decoder model that maps generated latents back to pixel space.

### 时空潜在 patches

给定一个压缩的输入视频，OpenAI 提取一系列时空 patches，充当 Transformer 的 tokens。该方案也适用于图像，因为图像可视为单帧视频。OpenAI 基于 patches 的表示使 Sora 能够对不同分辨率、持续时间和长宽比的视频和图像进行训练。在推理时，OpenAI 可以通过在适当大小的网格中排列随机初始化的 patches 来控制生成视频的大小。

Given a compressed input video, we extract a sequence of spacetime patches which act as transformer tokens. This scheme works for images too since images are just videos with a single frame. Our patch-based representation enables Sora to train on videos and images of variable resolutions, durations and aspect ratios. At inference time, we can control the size of generated videos by arranging randomly-initialized patches in an appropriately-sized grid.

## 扩展Transformer用于视频生成（Scaling transformers for video generation）

Sora is a diffusion model21,22,23,24,25; given input noisy patches (and conditioning information like text prompts), it’s trained to predict the original “clean” patches. Importantly, Sora is a diffusion transformer.26 Transformers have demonstrated remarkable scaling properties across a variety of domains, including language modeling,13,14 computer vision,15,16,17,18 and image generation.27,28,29

Sora 是一个扩散模型；给定输入的噪声块（和像文本提示这样的条件信息），它被训练来预测原始的“干净”块。重要的是，Sora 是一个扩散变换器。变换器在包括语言建模、计算机视觉和图像生成等多个领域展现了显著的扩展属性。

![](img/b67d804ec2017cb1c00fbca9c430d747.png)

In this work, we find that diffusion transformers scale effectively as video models as well. Below, we show a comparison of video samples with fixed seeds and inputs as training progresses. Sample quality improves markedly as training compute increases.

在这项工作中，我们发现扩散变换器作为视频模型也能有效地扩展。下面，我们展示了训练进展过程中，使用固定种子和输入的视频样本比较。随着训练计算量的增加，样本质量显著提高。

![](img/ee4c8a67b757bc9027a5a1eeb2cf926e.png)

## 可变持续时间、分辨率、宽高比（Variable durations, resolutions, aspect ratios）

Past approaches to image and video generation typically resize, crop or trim videos to a standard size – e.g., 4 second videos at 256x256 resolution. We find that instead training on data at its native size provides several benefits.

过去在图像和视频生成中的方法通常会将视频调整大小、裁剪或剪辑到一个标准尺寸——例如，4秒长的视频，分辨率为256x256。我们发现，直接在数据的原始尺寸上进行训练可以带来几个好处。

### 采样灵活性（Sampling flexibility）

Sora can sample widescreen 1920x1080p videos, vertical 1080x1920 videos and everything inbetween. This lets Sora create content for different devices directly at their native aspect ratios. It also lets us quickly prototype content at lower sizes before generating at full resolution—all with the same model.

Sora 可以采样宽屏1920x1080p视频、竖屏1080x1920视频以及介于两者之间的所有格式。这使得 Sora 能够直接按照不同设备的原生宽高比创建内容。它还允许我们在使用同一模型生成全分辨率内容之前，快速原型化较小尺寸的内容。

![](img/a085282bb11d280b358ca5ae678b2234.png)

### 改进的构图和画面组成（Improved framing and composition）

We empirically find that training on videos at their native aspect ratios improves composition and framing. We compare Sora against a version of our model that crops all training videos to be square, which is common practice when training generative models. The model trained on square crops (left) sometimes generates videos where the subject is only partially in view. In comparison, videos from Sora (right) have improved framing.

我们通过实证发现，在视频的原始宽高比上进行训练可以改善构图和取景。我们将 Sora 与一个版本的模型进行了比较，该模型将所有训练视频裁剪成正方形，这是训练生成模型时的常见做法。在正方形裁剪上训练的模型（左侧）有时会生成主体只部分出现在视野中的视频。相比之下，来自 Sora 的视频（右侧）具有改善的取景。

![](img/2902e82af5e6bb1e74da3ae3c3e0f191.png)

## 语言理解（Language understanding）
