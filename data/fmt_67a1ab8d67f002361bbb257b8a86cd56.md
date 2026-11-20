
# 如何通过AI绘画，将线稿迅速转化为画作？

> 来源：https://agijuejin.feishu.cn/docx/GxWedQcdZoXv9xxkPY8c2pPLneb
>
> 作者：德里克文
>
> 原文：https://zhuanlan.zhihu.com/p/676028523

AI绘画是一个利器，可以让有手绘基础的人，迅速掌握如何将自己的手稿（或者是已有的线稿）转化为画作，我这里分别用国内软件WHEE和开源软件 Stable Diffusion的Controlnet插件为例，来讲解一下如何将线稿迅速转化为画作。

## 一、WHEE线稿上色

### 1.步骤

首先我用最简便容易上手的国内软件WHEE做一个示范。先上出图效果

![](img/85720be3028df4afb4767ef298753e60.png)

这张图的原稿是我用DALLE生成的线稿图，我通过提示词让WHEE在我的图片基础上进行色彩的渲染

![](img/03a5c710748c5d277ddd9cac7c8cf196.png)

可以看到对比的效果，我采用的是画面控制里面的边缘检测，权重为0.6.

![](img/879b4a8116acb18b30fa76fbaf076f79.png)

### 2.其他效果

再看看其他几张的效果。

![](img/b7ed5aba5307d21d8d986a57f62e251c.png)

我采用的提示词是：

> 中国龙，老虎头，大师画作，色彩斑斓，照片真实，EOS,胶片颗粒，深色背景，氛围照明

![](img/fab915fec08cce8b80a6a35f54b5761d.png)

同样的，也可以采用平面感觉的色彩形式，只需要在提示词进行控制就可以

![](img/299405f25b9d308e760c2fc305062a6c.png)

![](img/20bec77a7cd73be4d9400f5cfd77a5aa.png)

甚至可以做出云雾的感觉

![](img/e0ba2e13259f35606e2e3d4b66458c57.png)

![](img/680475af8f7dcf3002df08a843eabfe3.png)

以上是简单易上手，出图效果也好的WHEE，WHEE虽然需要收费，但是出图效果是真的能打。

接下来我再来介绍下免费的Stable Diffusion如何进行线稿上色。

## 二、Stable Diffusion线稿上色

Stable Diffusion是一款开源免费的软件，可以直接下载到本地电脑上进行安装应用，但是对于电脑配置有一定的要求，建议是需要配置3060 12G的显卡作为基础，此外，对于想要熟练掌握还需要一些相应的只是，关于这个软件的安装及介绍，详见我的这下列文章做参考：

https://zhuanlan.zhihu.com/p/643656433

https://zhuanlan.zhihu.com/p/645820130

采用线稿进行上色处理，主要用的就是Stable Diffusion功能里面的ControlNet的Canny或者Lineart_Anime,下面我以Lineart为例，介绍下如何用图片或线稿转绘内容。

### 1.ControlNet单模型应用

接下来我采用lineart边缘检测举例，模式选择文生图，给大家做一个演示。例如我想生成一个芭蕾舞演员，我先去网上找了一张颇有美感，但是像素不足的图片，接下来我来演示怎么用ControlNet可控的生成多样高清的芭蕾舞演员。

![](img/1c49f3584d7b1240aa9a148922f554b4.png)

![](img/6bf90629e410a8f2b3901974ddd9afde.png)

颜色反转（Invert）：把底图中的黑色和白色交换。这个选项最主要的用途是，当你上传手绘的白底黑线的线稿图的时候，可以通过颜色反转来进行处理，因为ControlNet的控制默认是需要黑色为背景，控制线条是白色的，如果你是直接在操作界面画的，就不用管他。

![](img/ad806eab3ff5b9175d0aa23c86e601f0.png)

常规情况下我们选择Standard标准模式，本次由于我希望生成的是更具有动漫效果的感受，所以最后我选择了Lineart_Anime。这个根据自己想要的效果进行选择就好

然后控制权重选择1，开始步骤0，结束控制步骤1。然后输入提示词。

在这个图片中，我希望生成的是一个芭蕾舞演员，我的提示词是：

```
masterpiece, best quality, 1girl, ballerina, solo, ballet, tutu, ballet slippers, arms up, monochrome, white background, simple background, greyscale, full body, short hair, dress, from behind, dancing, tiptoes, standing, pantyhose, leotard,Vivid Colors, (EOS R8, 50mm, F1.2, 8K, RAW photo:1.2), physically-based rendering, sharp focus, extremely detailed CG unity 8k wallpaper, best quality, Amazing, finely detail, cinematic lighting, RAW Photo, 8k Ultra HD, SLR, High Quality, 8k, intricate detail, reality, realistic light, reality, photo reality, Deconstruction, masterpiece, extremely detailed CG unity 8k wallpaper, best quality, Amazing, finely detail, cinematic lighting, RAW Photo, 8k Ultra HD, SLR, High Quality, 8k, intricate detail, reality, realistic light, reality, photo reality, <lora:add_detail:1>, <lora:FilmG4:1>,
```

![](img/def7900685fd9ae3a582d0dfb6b4d99e.png)

![](img/b22ed4c3472a2f1759d3d0b8d0514019.png)

画面就生成好了，但是可以看到还是有比较多的缺陷，包括手指以及裙子生成并不是我们想要的结果，那怎么办办呢？我们可以增加ControlNet数量。

### 2. ControlNet多模型应用

在控制网络单元1中，我们继续增加插件，这次我选择用Openpose进行姿态检测，识别手指

![](img/e3f367519c622e3e6eca384c24db2f26.png)

![](img/6250173a2aafeec2405d3b1284c88cbe.png)

![](img/0ce7a2ee1ceecafc7f84a0b0787707a7.png)

可以看到，手指体态已经被修改正确了。

如果我想让生成的图像与原作更加接近，应该怎么做呢？

我们还可以再增加控制网3，为了让颜色和整体感觉更为接近，我启用了Tile模型

![](img/5e6e6c6475aa9dcb6accef4ac6c72174.png)

![](img/71f79927c286ca94d3580510463f6f48.png)

![](img/da4deeaf3f3dcecfcfc2e8052141ef9a.png)

怎么样，在几个控制网的作用下，对原作进行了各种诠释，通过对ControlNet的学习，我们能够更加好的控制画面效果，在成图后，只需要通过Photo Shop进行简单的后期调整即可出图。

而想要学好ControlNet这一进阶技巧，通过个人自己摸索可能会耗时耗力，我个人推荐想学AI绘画，有时间的小伙伴能够花0.1元去学习知乎知学堂的「AI绘画设计师实战营」，这门课程有AI绘画大佬来教你使用Midjourney、Stable diffusion等专业的AI绘画工具，通过这门课程的学习，对于一些进阶实用的技巧能够有更深刻的认知和掌握。

链接我就放在下面了：

AI绘画实战营｜送工具教程+提示词库+变现渠道

￥0.10 立即参加

在完整上完体验课后，你将获得「Stable Diffusion软件+安装教程」、「Midjourney常用咒语合集」等丰富的福利资料。记得在听完课后找助教老师领取哦！

![](img/a7f2055f95d841445db3eb56aa2a5cd6.png)

## 结语

通过ControlNET技术，可以采用自己的灵感手绘，快速生成一个具备画面冲击力的图片，你学到了吗？

---

我是德里克文，一个对AI绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！
