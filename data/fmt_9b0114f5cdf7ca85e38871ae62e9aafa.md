
# AI 资源与工具速递

[DecodingTrust: A Comprehensive Assessment of Trustworthiness in GPT Models](https://t.co/ZsmneX4jmr) paper page: https://huggingface.co/papers/2306.11698…

Generative Pre-trained Transformer (`GPT`) models have exhibited exciting progress in capabilities, capturing the interest of practitioners and the public alike. Yet, while the literature on the trustworthiness of `GPT` models remains limited, practitioners have proposed employing capable `GPT` models for sensitive applications to healthcare and finance - where mistakes can be costly. To this end, this work proposes a comprehensive trustworthiness evaluation for large language models with a focus on `GPT-4` and `GPT-3.5`, considering diverse perspectives - including toxicity, stereotype bias, adversarial robustness, out-of-distribution robustness, robustness on adversarial demonstrations, privacy, machine ethics, and fairness. Based on our evaluations, we discover previously unpublished vulnerabilities to trustworthiness threats. For instance, we find that `GPT` models can be easily misled to generate toxic and biased outputs and leak private information in both training data and conversation history. We also find that although `GPT-4` is usually more trustworthy than `GPT-3.5` on standard benchmarks, `GPT-4` is more vulnerable given jailbreaking system or user prompts, potentially due to the reason that `GPT-4` follows the (misleading) instructions more precisely. Our work illustrates a comprehensive trustworthiness evaluation of `GPT` models and sheds light on the trustworthiness gaps

[可将您的工作效率提高 10 倍的 8 大 AI 工具。](https://t.co/H7Ncerym3p)
https://t.co/ovF2aHsw2A

1. 人工智能语音发生器 http://lovo.ai
2. 人工智能网站建设者 http://10web.io
3. 图片魔术 http://magicstudio.com
4. 文字转视频 http://heygen.com
5. 演讲 http://tome.app

一篇论文公开了一种名为 `Vid2Avatar` 的技术，可以将 2D 动画中的人物和背景分离，制作成 3D 人物模型。 https://t.co/14jzXbP8FD https://twitter.com/hashtag/Vid2Avatar?src=hashtag_click https://modelinghappy.com/archives/50704 #Vid2Avatar #3D 人物模型

![](img/0954f5b65dd723f92030b35fec71bb57.png)

https://twitter.com/i/status/1670853301062213634

The top 8 AI tools to boost your productivity by 10x in 2023:

HomeRobot: Open Vocabulary Mobile Manipulation

https://github.com/s0md3v/sd-webui-roop

`baichuan-7B`

OpenAI functions

![](img/f5d39cff80af7ed07d6fddc8a1f71a2a.png)

Stable Diffusion generates beautiful images, but can it be used for open-world recognition? Try Demo! https://t.co/buFTIY6183 https://twitter.com/hashtag/CVPR2023?src=hashtag_click https://huggingface.co/spaces/xvjiarui/ODISE… Our #CVPR2023 paper shows that the pre-trained diffusion model indeed is a good image parser, allows for open-vocabulary segmentation and detection.

https://twitter.com/xiaolonw/status/1633929301073756161

https://huggingface.co/papers/2306.10998

Textbooks Are All You Need paper page: https://huggingface.co/papers/2306.11644… introduce `phi-1`, a new large language model for code, with significantly smaller size than competing models: `phi-1` is a Transformer-based model with 1.3B parameters, trained for 4 days on 8 A100s, using a selection of "textbook quality" data from the web (6B tokens) and synthetically generated textbooks and exercises with `GPT-3.5` (1B tokens). Despite this small scale, `phi-1` attains pass@1 accuracy 50.6% on `HumanEval` and 55.5% on `MBPP`. It also displays surprising emergent properties compared to `phi-1-base`, our model before our finetuning stage on a dataset of coding exercises, and `phi-1-small`, a smaller model with 350M parameters trained with the same pipeline as `phi-1` that still achieves 45% on `HumanEval`.

使用预训练的文本到图像扩散模型完成点云 纸页：https://t.co/AVwq8BQssZ https://huggingface.co/papers/2306.10533… 在实际应用程序中收集的点云数据通常是不完整的。由于从部分视点观察对象，数据通常会丢失，这些视点仅捕获特定的视角或角度。此外，由于遮挡和低分辨率采样，数据可能不完整。现有的完成方法依赖于预定义对象的数据集来指导嘈杂和不完整的点云的完成。但是，这些方法在对分布外（`OOD`）对象进行测试时表现不佳，这些对象在训练数据集中表现不佳。在这里，我们利用了文本引导图像生成方面的最新进展，从而在文本引导形状生成方面取得了重大突破。我们描述了一种称为 `SDS-Complete` 的方法，它使用预训练的文本到图像扩散模型，并利用对象的给定不完整点云的文本语义，以获得完整的表面表示。 `SDS-Complete` 可以使用测试时间优化完成各种对象，而无需昂贵的 3D 信息收集。我们在不完整的扫描对象上评估 `SDS-Complete`，这些对象由真实世界的深度传感器和 LiDAR 扫描仪捕获。我们发现它可以有效地重建常见数据集中不存在的对象，与当前方法相比，`Chamfer` 损失平均减少 50%。

by 宝玉

## 6 月 25 日

![](img/4e167aedf7ff1f79ece5b63eaf8f8226.png)

清华唐杰新作 `WebGLM`：参数 100 亿、主打联网搜索，性能超 OpenAI `WebGPT` `WebGLM`，一个参数 100 亿的联网问答聊天机器人（论文入选 KDD2023）。 据介绍，`WebGLM` 的目标是通过 Web 搜索和检索功能，增强预训练大语言模型，同时可以进行高效的实际部署。作者基于三种策略进行开发。 首先是大模型增强检索器。 它主要是用于增强模型相关网络内容的检索能力，在给定查询的情况下查找相关引用，以便后面更好地准确回答问题。 它有两个阶段：粗粒度 web 搜索和细粒度 `LLM` 增强密集检索。 其次是自举生成器。 它利用 `GLM`（比如清华之前发布的双语开源预训练模型 `GLM-130B`）的能力为问题生成回复，提供详细的答案。 利用该生成器，作者得到 `WebGLM-QA`——一个 `LLM` 自举引用和长程的 QA 数据集。 它通过上下文学习等策略进行清洗和过滤，最终包括 45k 的高质量过滤样本和 83k 的噪声样本。  `WebGLM` 的 backbone 就是一个在该数据集上训练的 `GLM` 模型。 最后是基于人类偏好的打分器。 它通过优先考虑人类偏好而非昂贵的专家反馈来评估生成回复的质量，确保系统能够产生有用和吸引人的内容。 以上三大组件最终按顺序形成 `WebGLM` 的 `pipeline`

打算深入一下 AGI，然后找到了一个非常适合初学者的课程 https://course.fast.ai。这个课程非常适合做上层应用开发的同学，原因是课程里面不会直接给你教授深度学习、神经网络的底层细节，而是教你如何使用开源模型，训练数据，微调参数。就像是编程，你可以把编译原理扔一边的感觉。

https://reddit.com/r/StableDiffusion/comments/14hbv5k/miss_multiverse_2023/

New open source text to video AI model `576x320` model: https://t.co/BYIUbFMDOp https://t.co/aFbEO6p62U https://huggingface.co/cerspense/zeroscope_v2_576w… 1024x576: https://huggingface.co/cerspense/zeroscope_v2_XL… `zeroscope_v2_576w`, A watermark-free Modelscope-based video model optimized for producing high-quality 16:9 compositions and a smooth video output. This model was trained using 9,923 clips and 29,769 tagged frames at 24 frames, 576x320 resolution. `zeroscope_v2_XL` is specifically designed for upscaling content made with `zeroscope_v2_576w` using `vid2vid` in the `1111 text2video` extension by kabachuha

![](img/d4e5163fb5df4b93dcf2157e8a2cec28.png)

Stable Diffusion 发布了新的图像模型 `SDXL 0.9`，这是文本到图像模型的一项突破性发展，相比前身在图像构成和细节上有显著的改进。 最主要的是 `SDXL 0.9` 可以在消费级 `GPU` 上运行，Windows 10 或 11，或 Linux 操作系统，16GB `RAM`，以及至少有 8GB `VRAM` 的 Nvidia GeForce RTX 20 显卡（或更高标准）即可运行。

https://t.co/eU7cm2WK99 https://t.co/eU7cm2WK99 https://struct.ai/blog/launching-struct-chat-platform…

ChatGPT and Bard cannot solve every problem for you.

1. YouTube Summaries → http://eightify.app
2. 3D Animations → http://moviebot.io
3. AI Assistant → http://zipzap.ai
4. Prompts → http://promptdrive.ai
5. How-to-videos → http://teachomatic.net

昨天 Midjourney 发布了 `zoom out` 功能，这个功能及其强大，可以创造出一些神奇的图片出来，甚至可以做视频。

AI 音乐（音乐生成等） 5 月流量一览

1. http://voicemod.net/text-to-song
2. http://musicfy.lol
3. http://soundraw.io
4. http://songtell.com 歌词分析
5. http://landr.com/en
6. http://my.brain.fm
7. http://voicify.ai 封面制作
8. http://boomy.com

https://openpm.ai

https://huggingface.co/psmathur/orca_mini_3b

@erhartford

[Midjourney V5.2 改进美学效果、连贯性、文本理解、更清晰的图像、更高的变异模式、缩小视角的外部绘画功能，以及一个新的 `/shorten` 命令用于分析你的提示词。](https://t.co/SJ7iWpSK4D) https://t.co/SJ7iWpSK4D https://mp.weixin.qq.com/s/D9xxP8QOLq5q7pNRhYCc3w… MJ 发布了 V5.2，SD 也不甘示弱！ https://mp.weixin.qq.com/s/rDWr0pkYsWKo83FxaMo1pA…

来看一下编程模型（`CoderLLM`）🚀

Open-source `#TextToVideo` community 🦒 colab 🥳

LLM As Chatbot update

https://inflection.ai/inflection-1

理论上最接近 ChatGPT 的模型，终于有人训练出来了， `orca_mini_3b`！

有人做了个 AI 川普和 AI 拜登

如何搭建情感陪伴 AI 机器人？

AI 自主代理的列表，包括开源和闭源项目。总结的很全面！

An early version of the amazing `zeroscope_v2 XL` text to video model is now available to use on Replicate:
