
# HiFiGAN 损失函数

```
class DiscriminatorP(torch.nn.Module):
    def __init__(self, period, kernel_size=5, stride=3, use_spectral_norm=False):
        super(DiscriminatorP, self).__init__()
        self.period = period
        norm_f = weight_norm if use_spectral_norm == False else spectral_norm
        self.convs = nn.ModuleList([
            norm_f(Conv2d(1, 32, (kernel_size, 1), (stride, 1), padding=(get_padding(5, 1), 0))),
            norm_f(Conv2d(32, 128, (kernel_size, 1), (stride, 1), padding=(get_padding(5, 1), 0))),
            norm_f(Conv2d(128, 512, (kernel_size, 1), (stride, 1), padding=(get_padding(5, 1), 0))),
            norm_f(Conv2d(512, 1024, (kernel_size, 1), (stride, 1), padding=(get_padding(5, 1), 0))),
            norm_f(Conv2d(1024, 1024, (kernel_size, 1), 1, padding=(2, 0))),
        ])
        self.conv_post = norm_f(Conv2d(1024, 1, (3, 1), 1, padding=(1, 0)))
    def forward(self, x):
        fmap = []
        # 1d to 2d
        b, c, t = x.shape
        if t % self.period != 0: # pad first
            n_pad = self.period - (t % self.period)
            x = F.pad(x, (0, n_pad), "reflect")
            t = t + n_pad
        x = x.view(b, c, t // self.period, self.period)
        for l in self.convs:
            x = l(x)
            x = F.leaky_relu(x, LRELU_SLOPE)
            fmap.append(x)
        x = self.conv_post(x)
        fmap.append(x)
        x = torch.flatten(x, 1, -1)
        return x, fmap
```

`x` 是子判别器展平后的整体输出，大小为 `[`B`，`l`]`；`fmap` 是经过每一层卷积后的特征图（feature map），类型为 `list`，元素个数为卷积层数，上述代码中有 6 个卷积层，则 `fmap` 元素个数为 6，每个元素是大小为 `[`B`，`C`，`l'`，`period`]` 的张量。

## 损失函数简介

HiFiGAN 的损失函数主要包括三块，一个是 GAN 原始的生成对抗损失（GAN Loss）；第二是梅尔频谱损失（Mel-Spectrogram Loss），将生成音频转换回梅尔频谱之后，计算真实和生成音频对应梅尔频谱之间的 L1 距离；第三是特征匹配损失（Feature Match Loss），主要是对比真实和合成音频在中间卷积层上的差异。

## 生成对抗损失

HiFiGAN 仍然是一个生成对抗网络，判别器计算输入是真实样本的概率，生成器生成以假乱真的样本，最终达到生成器合成接近真实的样本，以致于判别器无法区分真实和生成样本。HiFiGAN 使用最小二乘 GAN（LS-GAN），将原始 GAN 中的二元交叉熵替换为最小二乘损失函数。判别器的生成对抗损失定义为：

对应的代码实现：

```
def discriminator_loss(disc_real_outputs, disc_generated_outputs):
    loss = 0
    r_losses = []
    g_losses = []
    for dr, dg in zip(disc_real_outputs, disc_generated_outputs):
        r_loss = torch.mean((dr-1)**2)
        g_loss = torch.mean(dg**2)
        loss += (r_loss + g_loss)
        r_losses.append(r_loss.item())
        g_losses.append(g_loss.item())
    return loss, r_losses, g_losses
```

生成器的生成对抗损失定义为：

其中，   表示真实音频，   表示梅尔频谱。

对应的生成器代码实现：

```
def generator_loss(disc_outputs):
    loss = 0
    gen_losses = []
    for dg in disc_outputs:
        l = torch.mean((dg-1)**2)
        gen_losses.append(l)
        loss += l
    return loss, gen_losses
```

[更详尽关于 GAN ](https://zhuanlan.zhihu.com/p/58812258)的理论参见：GAN 万字长文综述

## 梅尔频谱损失

借鉴 Parallel WaveGAN 等前人工作，向 GAN 中引入重建损失和梅尔频谱损失可以提高模型训练初期的稳定性、生成器的训练效率和合成语音的自然度。具体来说，梅尔频谱损失就是计算合成和真实语音对应频谱之间的 L1 距离：

其中，   表示将语音转换为梅尔频谱的映射函数。

对应的损失函数实现：

```
loss_mel = F.l1_loss(y_mel, y_g_hat_mel)
```

上述代码中，`y_mel` 表示真实语音对应的梅尔频谱，`y_g_hat_mel` 表示梅尔频谱合成语音之后，合成语音又转换回来得到的梅尔频谱。

## 特征匹配损失

特征匹配损失是用来度量神经网络从真实和合成语音中提取的特征差异，具体来说，就是计算真实和合成语音经过特征提取层之后输出之间的 L1 距离：

其中，   表示判别器中特征提取层的层数，   表示提取的特征，   表示第   层判别器网络提取的特征数量。对应的代码为：

```
def feature_loss(fmap_r, fmap_g):
    loss = 0
    for dr, dg in zip(fmap_r, fmap_g):
        for rl, gl in zip(dr, dg):
            loss += torch.mean(torch.abs(rl - gl))
    return loss
```

## 整体损失

### 生成器整体损失

生成器的整体损失为：

其中，   和   分别为特征匹配和梅尔频谱损失的加权系数，实验中 $\lambda_{fm}=2，\lambda_{mel}=45$。

因为 HiFiGAN 的判别器是由多尺度判别器和多周期判别器组成，因此生成器的总体损失又可以写作：

其中，   为多尺度判别器和多周期判别器的个数，   表示第   个 MPD 和 MSD 的子判别器。

对应的代码为：

```
# L1 Mel-Spectrogram Loss
loss_mel = F.l1_loss(y_mel, y_g_hat_mel) * 45
y_df_hat_r, y_df_hat_g, fmap_f_r, fmap_f_g = mpd(y, y_g_hat)
y_ds_hat_r, y_ds_hat_g, fmap_s_r, fmap_s_g = msd(y, y_g_hat)
loss_fm_f = feature_loss(fmap_f_r, fmap_f_g)
loss_fm_s = feature_loss(fmap_s_r, fmap_s_g)
loss_gen_f, losses_gen_f = generator_loss(y_df_hat_g)
loss_gen_s, losses_gen_s = generator_loss(y_ds_hat_g)
loss_gen_all = loss_gen_s + loss_gen_f + loss_fm_s + loss_fm_f + loss_mel
```

### 判别器整体损失

判别器的整体损失为：

类似于生成器，由于 HiFiGAN 拥有多个判别器，因此判别器的整体损失可以写作：

其中，   为多尺度判别器和多周期判别器的个数，   表示第   个 MPD 和 MSD 的子判别器。

对应的代码为：

```
# MPD
y_df_hat_r, y_df_hat_g, _, _ = mpd(y, y_g_hat.detach())
loss_disc_f, losses_disc_f_r, losses_disc_f_g = discriminator_loss(y_df_hat_r, y_df_hat_g)
# MSD
y_ds_hat_r, y_ds_hat_g, _, _ = msd(y, y_g_hat.detach())
loss_disc_s, losses_disc_s_r, losses_disc_s_g = discriminator_loss(y_ds_hat_r, y_ds_hat_g)
loss_disc_all = loss_disc_s + loss_disc_f
```

# 语音合成的总体知识体系

语音合成知识结构如下。

## 基本组成

语音合成（Speech Synthesis）将文本转换为可听的声音信息，它赋予了像人一样说话的能力，是人机交互的重要接口。一般来说，语音合成的概念比文语转换（Text-to-Speech，TTS）的涵盖范围更广，语音合成包括 TTS、歌唱合成等领域，但大多数情况下可以混用。`awesome-tts-samples` 提供了一些端到端语音合成模型的样例，可以简单感受下目前语音合成的发展。

人类可以通过一整套发音器官合成语音，具体来说，肺相当于动力源，喉相当于调制器，声道相当于滤波器，口唇相当于扩音器。研究人员提出了以源-滤波器（source-filter）模型为代表的多种模型建模该过程，语音中存在清音和浊音，分别由声带周期性振动对应的周期声源和声带不振动时紊乱气流对应的非周期声源产生。

当代工业界主流语音合成系统包括文本前端和声学后端两个部分。文本前端将输入文本转换为层次化的语音学表征，主要包括文本规范化、韵律分析和文本转音素等模块。声学后端基于文本前端给出的层次化语言学表征（linguistics feature）来生成语音，主要技术路线包括单元挑选波形拼接、统计参数和端到端语音合成方法，当代主要采用端到端声学后端。端到端声学后端一般包括声学模型和声码器两部分，同时也出现了直接从音素映射为波形的完全端到端语音合成系统。声学模型负责将语言学特征转换为中间声学特征，比如梅尔频谱，直接决定合成语音的韵律；声码器将中间声学特征转换为语音波形，直接决定合成语音的音质。

语音合成与语音识别、机器翻译等问题类似，本质是序列到序列的建模问题，另一方面，语音合成是生成类问题，因此自回归生成、VAE、GAN、Flow、扩散模型等生成模型在语音合成上均有应用。

## 声音转换

狭义上说，将一个 [说话人](https://arxiv.org/abs/2008.03648) 的语音转换到另一个目标说话人（target speaker）的语音，这种语音处理技术称之为声音转换（Voice Conversion，VC）。声音转换包括平行和非平行两种，平行语音转换指的是源、目标语音的内容相同，相对应地，非平行语音转换只改变语音的音色，而对语音内容没有要求。声音转换可以借鉴图像中风格转换的成果，比如 StarGAN-VC 等模型。更有效的方法是利用语音识别模型提取说话人无关的中间表征，将该中间表征和说话人信息输入生成模型中，生成具有目标说话人音色的语音，可以借鉴 TTS 的研究成果，将 VITS 等作为高质量合成模型。在声音转换中，基频是一个重要特征。参见：An Overview of Voice Conversion and its Challenges: From Statistical Modeling to Deep Learning

## 多语种语音合成

多语种语音合成的难点主要有五点：

1. 音库制作和构建。

1. 文本前端的构建。需要制定适用于一个语种语音合成的音素体系，构建该语种的文本规范化规则等，特别地，比如俄语、波斯语、阿拉伯语等语种有性数格的变化，提高了文本前端的复杂性。不同语种间文本前端模块也会有所不同，比如中泰越日等语种需要添加分词模块。

1. 语种中特殊发音现象。西班牙语、俄语等语种的升降调对合成语音自然度影响显著，具体来说，在标点处发音会产生较为明显的上扬、下降、先升后降、先降后升和平调现象。因此文本前端最好给出该升降调信息，声学后端需要将该升降调信息作为重要特征，输入到模型的多个模块中，以协助模型产生准确自然的升降调语音。与此同时，由于升降调是一个语块整体的表现，而非仅仅一个字母或者单词的发音变化，比如西班牙语可以将升降调信息标注到整个 `L3` 块上，而俄语更适合于仅仅标注到中心词上。

1. 跨语种语音合成。当代语种混杂的语言现象愈发明显，比如中文中夹杂英语单词，这就需要实现语种切换（code-switch）和跨语种（cross-lingual）语音合成。如果语料中存在单说话人跨语种语料，一般来说，直接训练就可以产生跨语种合成效果。实践中，如果跨语种语料较少，比如仅有 200 句跨语种语料，多个语种混训时共享语种间的音素能够获得更好的合成效果；如果跨语种语料较多，可以仅采用主要语种和跨语种语料进行训练，加入其它语种进行混训相反可能会产生较差的效果。如果没有跨语种语料，可以尝试数据增强方法伪造跨语种语料，比如对单语种语料进行拼接，构造跨语种语料；利用跨语种语音合成大模型伪造训练语料，训练跨语种语音合成模型。

1. 低资源语音合成。[语料数量的增加](https://zhuanlan.zhihu.com/p/413274637)可显著提升合成语音的效果，但世界上的语种数量较多，大部分语种并不存在大量高质量语音合成语料。主要的解决方案有跨语种迁移、跨说话人迁移、语音识别和合成对偶学习，以及无监督训练。参见论文速览：低资源语音合成。

1. Latorre J, Bailleul C, Morrill T, et al. Combining Speakers of Multiple Languages to Improve Quality of Neural Voices[J]. arXiv preprint arXiv:2108.07737, 2021.

## 进阶

语音合成存在比较多的扩展应用，比如：

1. 高表现力语音合成。

1. 个性化语音合成。

1. 情感合成和韵律迁移。

1. 方言迁移。

1. 歌唱合成。

1. 有声人脸合成（talking face synthesis）。

1. 语音合成的稳定性。

1. 语音合成的效率优化。

本部分可参考相关论文和 [A Survey on Neural Speech Synthesis](https://arxiv.org/abs/2106.15561) 了解具体技术方案。

### 高表现力语音合成

在交互、小说阅读等应用场景中，对合成语音的表现力要求较高，而表现力由内容、说话人音色、韵律、情感和风格等多个因素决定，因此高表现力语音合成实际涵盖了内容、音色、韵律、情感和风格的建模、分离和控制。

语音中包含的信息可以分为如下四类：

1. 字符或音素，也就是语音的内容。可以通过预训练词嵌入增强合成语音的表现力和质量，或者加入一些额外的信息，比如升降调信息，采用 `fulllab` 能够增强模型表现力和稳定性。`fulllab` 及传统语音合成采用的文本、声学特征参见：jsut lab、`HTS Data README`。

1. 说话人或音色。多说话人语音合成模型可以通过说话人嵌入向量或单独的说话人编码器（`speaker encoder`）对音色特征进行建模。

1. 韵律、风格和情感。这些特征表示 "如何说出文本"，表征语音中的语调、重音和说话节奏，韵律、风格和情感是高表现力语音合成的建模重点。

1. 录音设备和环境噪音。这些倒是与语音内容、韵律无关，但会显著影响语音质量，因此可以尝试对语音中的噪音等进行控制和分离。对训练语料本身可以提前进行去噪处理，在模型中可以标识带噪语料，以便在合成语音中去除噪音部分。

利用模型建模这些信息的方法很多，有语种、说话人、风格嵌入向量以及音高、时长、能量编码器等显式建模方法，也有 `reference encoder`、`VAE`、`GAN/Flow/Diffusion`、文本预训练等隐式建模方法。

### 个性化语音合成

个性化语音合成是语音合成中较为热门的方向，主要应用于定制化场景中，利用任意用户的音色合成语音，又称为语音自适应（Voice Adaptation）、语音克隆（Voice Cloning）、定制化语音（Custom Voice）等，通常要求利用 1 分钟以内甚至几秒钟的语料，就可以实现合成目标说话人的任意语音。目前个性化语音合成的难点如下：
