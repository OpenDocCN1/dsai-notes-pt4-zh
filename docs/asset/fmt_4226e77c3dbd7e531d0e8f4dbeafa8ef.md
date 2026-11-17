
# AI视频新力量——Dreamina内测测评

> 来源：[https://agijuejin.feishu.cn/docx/KhbudZJY6oKogGxBJiccdW2Hnkd](https://agijuejin.feishu.cn/docx/KhbudZJY6oKogGxBJiccdW2Hnkd)

> 作者：德里克文

> 原文链接：https://zhuanlan.zhihu.com/p/688091896

字节的Dreamina前段时间开启了AI视频测试，我很幸运地收到Dreamina视频的内测邀请，并针对这个软件的效果进行了一些测试。

![](img/62d74b8d5355173f296b462bf22e055a.png)

[f6099356-e685-11ee-b97d-5e139b0c4027-v8_f2_t1_vigufJRT.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ElFybipLQowYuSxsWqxcQEhAngh)

先上初测效果结论：

1. 界面操作友好，简单易上手
1. 动态效果表现优秀，动起来感觉明显，对比SVD仅仅是轻微的图片移动，动态效果要更好。
1. 对于图片内容的识别性高，在人物及动物的特色表情 动态方向 表现不错。
1. 稳定性尚可，但是会出现掉帧，在剧烈运动的时候会出现画面变化较大的情况。
1. 图像输出分辨率有待提高
1. 首尾帧功能富有特色

下面本篇我将逐项从不同维度分析下Dreamina的视频生成效果。

## 一、操作界面

虽然Dreamina的视频生成目前尚在内测，但是文生图功能已经开放，目前图片功能免费。

对AI绘画感兴趣的朋友可以先去尝试下AI绘画功能。

![](img/15ba46fa88335e7edd2e1f2c2c568371.png)

网址地址是：`https://dreamina.jianying.com/ai-tool/home`

[  ](https://link.zhihu.com/?target=https%3A//dreamina.jianying.com/ai-tool/home)

进入网站后，内测用户才有视频生成这个功能。

![](img/8d20c9f84bf284d824e0c9c5c5e85187.png)

### 1. 图片生成

先让我们看下图片生成操作界面，只需要输入简单的自然文字就可以生成画面，一次生成4个画面，然后可以选择其中一张进行放大。

左侧界面输入提示词，选择模型和尺寸大小

![](img/6ddffe87460846509224a6b5eb946b6a.png)

关于精细度，可以理解为是`Stable Diffusion`里面的步骤，步骤越多，生成的质量越好，细节越丰富，等待时间也更长。

![](img/4ea7a2aa7ce58b25bdd1457eb19c2b30.png)

比例上可以选择常规使用的尺寸，确定完这些内容就可以直接生成了，步骤简单易上手。

由于本文主要说视频效果，图片生成就不再展开，有兴趣的朋友可以上手自己试试。

### 2. 视频生成

视频生成界面与图片生成界面差不多，我来简单介绍下。

输入有两种，分别是图片生视频和文本生视频。

![](img/f49b7abd216bc8aa225a6ff9fe6e0a23.png)

其中图片生视频还有一个创新的独有功能，使用尾帧，可以让你开始画面衔接到最终的画面。

![](img/a396c38847a768c4d69ed6898a3ac14f.png)

而在左侧界面中，还有运镜控制、视频比例和运动速度可以调节。

![](img/26a5771730d7e8ec44e7d8cc7f77211e.png)

文生视频基本操作与图生视频差不多，只是不需要输入图片，不再赘述。

从界面来看，简单易上手。

## 二、视频效果评测

接下来，我将从以下几个维度进行逐一分析：需求完成度、画面稳定性、功能多样性及视频生成质量。

### 1. 需求完成度

在需求完成度方面，Dreamina对我来说效果出众，能够完成我提出的各种复杂要求，我来举几个例子。

首先是对提示词的执行程度，下面的视频我自己有做了一个对比，这个视频在我不输入提示词，只放入一张画面的情况下，仅仅是做了一个镜头的放大。

那么输入提示词后会有什么不同呢？

![](img/544e76d955611c5f6e2e8a57b94328fb.png)

[8ed1eef0-e68e-11ee-bd22-0e1dff369a69-v8_f2_t1_t2W1vQv5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Q8TybsCcVoBz9dxRvuxcYF4Gnrd)

而当我输入提示词“车辆疾驰开过”后，车根据我的需求开动了。

这说明模型能够理解并很好实现了我的需求。

再看看另外一个画面，我输入的提示词是：“机器人缓慢走向前方，背后有爆炸，火焰和烟尘”

这个动态效果太惊艳了！完全按照提示词的内容做出来。

再来看看我今天为“春分”这个节气做的视频效果。

![](img/c03bfca59cdaeafbf1d386b27295771e.png)

[8e6b8ef2-e68f-11ee-8d24-ceffdd76bd91-v8_f2_t1_cNCZsI16.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QDYfbJUFforomsxwuyvcUuCQnBh)

我的提示词是：“微风吹过，燕子飞走”，让我惊讶的是它可以很清楚识别出燕子，并且在前2秒完全理解了这个动作，只移动燕子飞走。

整体来看，对于提示词需求的语义理解还是比较到位。

### 2. 画面稳定性

画面稳定性是最后能不能用很关键的因素。如果出来的视频频闪、不连续，或者仅仅变化太大，对于最终的效果是影响很大的。

让我们看下画面稳定性功能如何。

这个效果让我非常惊艳，图像稳定，动态合理。

![](img/3752e51863e3d51ddfaff4006c26a713.png)

[2b89899a-e691-11ee-bdf7-c2982f51abfb-v8_f2_t1_FG7cRfS3.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IX2yb7KRboFuaXxeRTqcIGPOnDb)

它的提示词是：一个机器人抬起手缓慢向镜头走来，天空黑色云朵有闪电，背后有大楼爆炸倒塌。

虽然最后大楼爆炸倒塌没有实现，可能是过于复杂，但是前面的提示词效果很好，画面非常稳定。

接下来再看下人物微表情，我采用文生视频。

![](img/3f83a40f27de5a577192c8fed01aeda6.png)

[a015a6ae-e691-11ee-ad46-b6995f508c17-v8_f2_t1_KbBCtddT.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FeeebYLJUoIZP2xXGeycF38JnUd)

提示词是：中国古代美丽的女子在市集中漫步，她的目光流转，似乎在寻找着什么。

效果还不错，再看看没有提示词的情况下，会是什么效果，我用宠物狗作为实验。

![](img/d0f5f05ff9c82268b924794b2fd96973.png)

[df9cd69e-e691-11ee-983a-06533a5e4762-v8_f2_t1_3lyTZCP0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NsCFbws2YoQjPuxyAoic6jY4nIb)

这是没有任何提示词，我仅仅上传图片的效果，惟妙惟肖，没有错漏帧。

在视频的

### 3. 功能多样性

功能多样性影响到创作手法的可能性，在这一点上，Dreamina的首尾帧功能比较有新意。

来看看效果吧，我开始画面是一个茶园，结束画面是另外一个角度的茶园。

![](img/73b5398f2b06340c8161d20fa0ed642e.png)

[50a3907e-e694-11ee-9334-16dd0daffdec-v8_f2_t1_wQUxZdUL.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZKUdbgKjQoSD3JxT08qcoPrInRg)

我输入的开始画面和结尾画面其实差别还比较大，所以最后的效果有点像运镜。

那么如果是人类的流畅动作，能不能做出来呢？

![](img/e27007b05bcc35ce201406f1ede573a6.png)

[d224791c-e692-11ee-8530-32501ae8e09a-v8_f2_t1_EhX1pOig.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H9jhbCVp9ohmaLx9EiWcr3EKnsh)

可以看到动作是对了，虽然脸有点崩，但是如果后续有好的优化，应该还是能做出好的画面的。

这点上我也希望Dreamina能够增加更多的创新功能，包括不同的运镜效果，笔刷运动等等功能，让技术手法更加成熟。

### 4. 视频生成质量

视频生成质量是指视频的清晰度，色调等方面，这对最终效果有非常重大影响。

目前我对于Dreamina觉得不足的主要就是分辨率不够高，例如下面这个视频，视频本身三维旋转效果很好，可是清晰度不高直接影响了最终效果。

![](img/58140b85f06001602af883d33cf425f5.png)

[2b3cdfea-e692-11ee-a18e-de15ac66236a-v8_f2_t1_IhP1YNFZ.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QnUObXItLo1twqx9qBQcLI1snwS)

希望后续Dreamnia能够加强生成过程中的分辨率，或者推出视频高清提升的功能，这样才能有更好的效果。

## 三、生成视频效果赏析

由于回答问题上传视频数量的限制，我只能在文章中插入10个视频，想要看更多我做的Dreamina视频效果的，可以查看下面的视频文章，后续我也争取尽快制作一个完整的短片视觉作品，呈现给到大家。

+ https://www.zhihu.com/zvideo/1753822457683976192
+ https://www.zhihu.com/zvideo/1753188974036811776
+ https://www.zhihu.com/zvideo/1753182651723288576

## 结语

整体来看，Dreamina在动效方面的表现出色，真正让图片动了起来，在需要很强动态效果的画面可以直接采用Dreamina进行出图。
