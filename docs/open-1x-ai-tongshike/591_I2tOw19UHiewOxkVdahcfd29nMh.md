# AI数字人通用学习手册

> 来源：[https://datawhaler.feishu.cn/docx/UXHQd0856oe4kuxrVJQcA5mxnwF](https://datawhaler.feishu.cn/docx/UXHQd0856oe4kuxrVJQcA5mxnwF)

> ✍️  作者：[华泽的个人说明书](https://datawhaler.feishu.cn/wiki/QIguwXRlgiCOIzk5Aecc2Ifln9c)

## 学前须知

+   **学习时间**：1~3天

+   **学习难度**：如果直接使用商业工具，难度低，如果使用开源方案，难度较高

+   **学习方式**：

1.  根据技术能力和需求选择开源/闭源的工具

1.  选择想要做的数字人，跟着案例自己尝试做一遍。

1.  熟悉流程后，发挥想象力，应用到自己需要的数字人场景制作中

>

> 打卡核实后**「可获得证书」**，优秀打卡内容将被**「展出」**，特别优秀者会邀请来**「一起完善学习文档」**

> 具体打卡要求详见：[【说明文档】打卡（非必须）](https://qixiangxingqiu.feishu.cn/wiki/KaAEwvKHxiZfozkf0cnclM7YnvQ?from=from_copylink)

数字人制作主要分为以下三个阶段:

> 从开源的技术来看， 主要分为声音克隆和面部重建两部分; 肢体动作控制和表情控制这方面突破较少,暂不过多讨论

# 开源部分介绍:

## 声音克隆

> 好消息：有支持win、mac的启动包，支持webui

> 坏消息：需要显卡，好的效果需要抽卡， 无法直接商用

### 1.1 so-vits-svc-fork

https://github.com/voicepaw/so-vits-svc-fork

支持一键安装和gui，好的效果需要抽卡，提供的音频质量和长度越高--相对效果越好

![](img/7a200bdcc2905ab734312aefb478d366.png)

### 1.2 [**GPT-SoVITS**](https://github.com/RVC-Boss/GPT-SoVITS)

https://github.com/RVC-Boss/GPT-SoVITS/blob/main/docs/cn/README.md

GPT-SoVITS主页的功能介绍：

1.  **零样本文本到语音（TTS）：** 输入 5 秒的声音样本，即刻体验文本到语音转换。

1.  **少样本 TTS：** 仅需 1 分钟的训练数据即可微调模型，提升声音相似度和真实感。

1.  **跨语言支持：** 支持与训练数据集不同语言的推理，目前支持英语、日语和中文。

1.  **WebUI 工具：** 集成工具包括声音伴奏分离、自动训练集分割、中文自动语音识别(ASR)和文本标注，协助初学者创建训练数据集和 GPT/SoVITS 模型。

1.  如果你是 Windows 用户（已在[ win>](https://huggingface.co/lj1995/GPT-SoVITS-windows-package/resolve/main/GPT-SoVITS-beta.7z?download=true)=10 上测试），可以直接下载预打包文件，解压后双击 go-webui.bat 即可启动 GPT-SoVITS-WebUI。

更多演示可以参考：

+   GPT-SoVITS-WebUI一键整合包及使用教程

+   [GPT-SOVITS进阶使用(w](https://www.bilibili.com/video/BV15p421o7Ji/)ebui篇)_哔哩哔哩_bilibili

---

以下不做学习要求，了解即可。

### 1.3 fish-speech

https://github.com/fishaudio/fish-speech

![](img/d59338258f187330a40a7b569f55a2ab.png)

综合框架的tts项目，生成音色质量较高，两种训练模式可以完成各种音频训练需求，主观来说质量有超过GPT-SoVIts。

可以体验下效果：

https://speech.fish.audio/samples/#2

视频：

## 面部重建 (含音唇同步)

解释：一种用任意音频驱动/重建人类面部图片或视频，使其作出符合对应音频发音时的面部表情、动作、音唇同步等要求的技术。

下面介绍几种尽可能开箱即用的技术：

### SadTalker

单照片说话技术

> 好消息：支持webui，支持sd插件集成; 泛化性较好; 无需训练

> 坏消息：音唇同步一般; 牙齿模糊或者缺失; 无法商用;

https://github.com/OpenTalker/SadTalker

![](img/35ee26f3697b2fdd65287275f1f51a7f.png)

### Wav2Lip

最早的视频说话技术、音唇同步效果较好的开源项目之一

具体可以见:

https://github.com/Rudrabha/Wav2Lip

> 好消息：轻量，泛化性较好; 音唇同步较好; 无需训练

> 坏消息：不支持webui,需要动手装; 牙齿模糊或者缺失; 无法商用;

![](img/e577a7ee848075ceedb0e4d4215f1261.png)

### **VideoReTalking**

https://github.com/OpenTalker/video-retalking

视频说话技术

> 好消息：无需训练既可以推理; 音唇同步较好

> 坏消息：不支持webui,需要动手装环境; 牙齿模糊或者缺失; 无法商用; 泛化性差

![](img/2029d70dbf3c36c6bd47f78eb255e857.png)

---

注意：以下为扩展知识，不做学习要求

###  [AniPortrait](https://github.com/Zejun-Yang/AniPortrait)

腾讯项目，缺点明显就是慢！质量不错，基于sd算法。无需训练。

![](img/8155b03647c855dcef68dcc1eeaf8a23.png)

### Real3DPortrait

https://github.com/yerfor/Real3DPortrait/blob/main/README-zh.md

Geneface++团队旗下项目，单图驱动。速度一般，效果不错。如果着急用是sadtalker的优质平替。无需训练。

![](img/56613e22798656695f6654128fa03a60.png)

### **SyncTalk**

https://github.com/ZiqiaoPeng/SyncTalk?tab=readme-ov-file

目前nerf的先驱项目，2024cvpr。但是是缝合怪。

缺点：nerf的模型普遍问题就是训练时间漫长。也需要贴回。

![](img/8a16e115b04fd62b7c22538599cfdb63.png)

### ER-NeRF

（20230717）高质量nerf项目，目前速度可以做到realtime。

缺点：nerf的模型普遍问题就是训练时间漫长。也需要贴回。

https://github.com/Fictionarry/ER-NeRF

### Geneface++

（20240203）【Geneface团队】部分nerf

https://github.com/yerfor/GeneFacePlusPlus

缺点：nerf的模型普遍问题就是训练时间漫长。也需要贴回。

![](img/e963e00ce790ac024c0abb86acdaabd7.png)

## 肢体动作控制

|  |  |
| --- | --- |
|  |  |

开源效果普遍很差，这里简单放几个例子：

### 3.1 Make-Your-Anchor

GitHub - ICTMCG/Make-Your-Anchor

代码未开源，无法使用；演示视频也有较多缺陷，这里不抱太多期望

### 3.2 MuseV

https://github.com/TMElyralab/MuseV/blob/main/README-zh.md

![](img/cbccf13cf91b5aaf1a2cc16805cdcc43.png)

可以做人像动作，辅助musetalk，通过museV用图片生成视频后用musetalk生成高质量数字人视频。

![](img/8aaee796a4707d1e5973bdf244dea5ac.png)

## 表情控制

开源效果普遍很差，这里暂不过多讨论

# 闭源部分介绍 （可白嫖体验）:

> 可白嫖体验的数字人APP， 至少支持1个数字人的训练

## Heygen  （国外）

> 支持免费体验

> 支持图片说话、真人数字人

> 地址： https://demo.heygen.com/free-video

> 效果评价：画质、清晰度、音唇同步都很强，支持公模和定制数字人；免费版为公模数字人体验

![](img/d7362bd025ca903002be7f39b3211a75.png)

## D-ID （国外）

> 支持免费体验

> 目前只支持单照片说话数字人，不如heygen的视频的真人数字人效果好

> 地址：https://studio.d-id.com/editor

> 效果：支持漫画头像，音唇同步和牙齿合成效果一般般

![](img/e5fb6af26bb60818a30bd83a83bedc70.png)

## 闪剪APP （手机端）

> 支持免费体验

> 真人数字人免费版支持定制一个数字人和一个声音支持; 照片数字人在APP其他的板块里, 自行查找

> 地址：https://shanjian.tv/child-custom-digital/list

> 效果：音唇同步效果很强, 免费版本只支持720P; 支持自动文案、自动配乐、自动字幕等功能

![](img/9f912fe5f918a353e23c28e68b934c08.png)

闪剪VIP权益明细如下： [闪剪VIP权益明细 链接](https://mp.weixin.qq.com/s?__biz=MzUzNDk5OTM5Ng==&mid=2247492416&idx=1&sn=a46edfc8a81d57152d47e5d47588a931&chksm=fa8e9b4bcdf9125d2850a4f39f89326dd73520e9a82ade53b729b23c5208dcea53d76a9dffb7#rd)

![](img/c2b594da57fcb17195bab29e4ed085ba.png)

## 通义千问emo（手机端）

https://mp.weixin.qq.com/s/P4Anq6KmuXkR99LlVmjXNw

打开通义千问APP，输入EMO后，就跳转到数字人界面。可以做动作克隆和数字人唇形驱动视频两种。

![](img/33dbfe4a2b2c9640947f85261ab104d2.png)

![](img/db99be7550de1b3359c06fb9b194ac92.png)

效果展示：

## 剪映专业版 （电脑端）

> 支持免费体验，任意新建文本或者上传音频后，点击添加数字人，即可部分免费使用右侧公模的数字人、很快就可以生成口播视频了；不支持定制数字人

> 下载地址：https://www.capcut.cn/

![](img/7d74e70cb7666dfd17c06296b9839b81.png)

![](img/191982cf3344301c20a46cf530d6d399.png)

效果展示：

![](img/dd59517b9371e665f373c6f08c17716d.png)

[4月7日 (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IqfOblh2Fo9VJAxtT63cUFICnEo)

![](img/1d616809fc45778a30560618a8784b85.png)

[4月7日 (1)(1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PiwCbgE6HoWKyZxZbWucTdCInyh)

# 简单照片数字人制作案例：

[[案例]一张照片生成新年数字人拜年](https://datawhaler.feishu.cn/wiki/J23CwDwEYicIg2kc44DcKPfVnpg)