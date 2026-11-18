
# Meta音频AI三件套爆火：一句话生成音乐音效，还能高保真压缩音频

> 来源：[https://xuqiwei1986.feishu.cn/docx/WWoOdht13o2wbixuHucc3Bx3nge](https://xuqiwei1986.feishu.cn/docx/WWoOdht13o2wbixuHucc3Bx3nge)
> 🔗 原文链接：`https://www.toutiao.com/article/7263360477252682252/?log_from=d2612f5d569a_1691147408920`

2023-08-04 15:11 · 量子位

> 萧箫 发自 凹非寺
> 量子位 | 👸号 QbitAI

Meta突然放大招，直接给一系列音频AI模型搞了个“全家桶”。

从文本生成音乐、文本生成音效、到高质量音频压缩，音频编辑和音频生成的AI工具全都有，命名为 `AudioCraft`。

`AudioCraft`中的所有模型，全部开源。

![](img/3ea87b489907e4c2bf544ce45b3f729d.png)

值得一提的是，Meta刻意强调自己所有AI的训练数据都是经过授权，或是从公开渠道获取的。

有网友闻讯赶来尝试：

> 这太疯狂了，刚刚试了一下，生成的声音效果真不错！

![](img/8136fdc6ed424a0783a539ae7da41677.png)

还有网友调侃，Meta这是要与OpenAI“划界限”：

> 很明显，Meta试图将自己与OpenAI区分开来，“我们不使用没授权的数据”。

![](img/aa4208038c7e93904b390102f8eb929e.png)

所以，这个音频AI“全家桶”里有些啥，实际生成编辑效果又如何？

## 从生成到编辑，音频AI三件套

`AudioCraft`是一个音频AI开源库，目前包含`MusicGen`，`AudioGen`和`EnCodec`三个音频AI工具。

据Meta介绍，为了发布`AudioCraft`，他们这两天还特意更新了一版`EnCodec`模型，让它的输出质量更高。

具体来说，这三个模型分别用于文本生成音乐、文本生成音效和音频压缩：

+ `MusicGen`：基于文本输入生成音乐，使用Meta拥有和专门授权的音乐进行训练
+ `AudioGen`：基于文本输入生成音效（雨点声、狗吠、警笛等），使用公共音效训练
+ `EnCodec`：压缩音频，以较低的音损保持音频的高质量

首先是`AudioGen`，这是一个自回归生成模型。

![](img/5c4efe3403b1bfc1432d43fbfd40d898.png)

`AudioGen`基于10个公开的音效数据集训练，里面包括狗吠、汽车鸣喇叭或木地板的脚步声等各种音效。

然后是`MusicGen`模型，一共包含300M、1.5B、3.3B三个不同参数量的自回归`Transformer`。

![](img/83faa3268c36d7e70f8a8a9dcaf24a4d.png)

`MusicGen`使用了20000小时的音乐来训练，包含10000条内部搜集的高质量音轨，以及ShutterStock和Pond5素材库中的数据，后两者的数据量分别为2.5万和36.5万。

这些音乐数据在32kHz下被重新采样，都配有流派、BPM等基本信息和复杂一些的文字说明。

最后是`EnCodec`神经音频编解码器（neural audio codec）。

编码器能从要压缩的音频信号中学习离散的音频token；随后，基于一个自回归语言模型，将音频信号压缩到目标大小；最后，基于解码器，就能将压缩的信号高保真重建回音频。

基于这种压缩效果，音频能被压缩到比MP3格式还要小10倍。

![](img/af4d789107f6974a5031298571f9d1f8.png)

## 可直接上手试玩

目前这几个模型都已经开源，框架都是基于`PyTorch`打造。

![](img/9b42305d25522ab05a3a55d3ee35b20e.png)

包括`MusicGen`、`AudioGen`和`EnCodec`的论文细节，可以在GitHub项目中找到：

![](img/992a2e9c8776cc51eccf025fc9f1aea1.png)

不过，训练代码并非全部开源，目前可以看到只有`EnCodec`、`MusicGEN`和`Multi Band Diffusion`是开源的：

![](img/b8fae1db29be1cd3145f048eac1b9996.png)

其中`MusicGEN`还开启了Demo试玩，我们之前也测试过：

![](img/b62d0ea6002531007e9c8f354d58d178.png)

不过对于`AudioCraft`的发布，网友们的评价也是褒贬不一。

有网友觉得，这样音频生成就变得更加大众化了，所有人都可以上手尝试：

![](img/4963de129e9700b90bee0222806bd738.png)

但也有网友认为，这势必导致人类连音频的真假都区分不清：

> 10年后，说不定我们就分辨不清过去的声音、图片、视频了。

![](img/57f1a8145e0a132a5dd7dbfd41b3b22f.png)

## One More Thing

最近，音频生成AI确实很火，就连效果都卷起来了。

这两天，一个论文和代码都还在准备的模型`AudioLDM2`，刚放出demo就已经在网上传开了来：

![](img/25b5834e86476b1b23e04d3443edd0e5.png)

作者Haohe Liu表示，这个模型在生成音效、音乐和可理解语音三个领域中均达到了SOTA。

感兴趣的小伙伴们，可以蹲一波后续了~

`MusicGEN`试玩地址：

`https://huggingface.co/spaces/facebook/MusicGen`

参考链接：

[1] `https://ai.meta.com/blog/audiocraft-musicgen-audiogen-encodec-generative-ai-audio/`
[2] `https://github.com/facebookresearch/audiocraft`
[3] `https://twitter.com/LiuHaohe/status/1686782804518973440`
[4] `https://news.ycombinator.com/item?id=36972347`

— 完 —

量子位 QbitAI · 头条号签约

关注我们，第一时间获知前沿科技动态
