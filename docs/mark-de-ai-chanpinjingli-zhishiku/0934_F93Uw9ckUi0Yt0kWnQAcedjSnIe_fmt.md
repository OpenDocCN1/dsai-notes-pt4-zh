
# Nano banana 全实操指南

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/QimRdK5gcoEousxucNgc0fSenRc](https://qqs7y1hozd1.feishu.cn/docx/QimRdK5gcoEousxucNgc0fSenRc)

相比较传统的AI图像生成模型比如Midjourney，`Nano banana`最大的特点在于不光能高精度生成图片，还可以理解图片中的物体，像神来之手般编辑图像，并将多个图片进行融合，宛如PS高手。`Nano banana`确实拓宽了以往多模态图像大模型的能力边界，使得不再是被动接受AI随机的生成图像，让用户能用精确的指令按需生成和编辑，这是很大的创新啊。最近`Nano banana`火了起来，朋友圈出现各种真人手办图，那就是`Nano banana`生成的，非常之逼真，以至于我以为是3D打印出来的。

![](img/2adbb6d86fa7f73e4e745e870b857758.png)

## 什么是 Nano banana

什么是`Nano banana`呢？它是Google在8月发布的一款多模态大模型，支持图像生成和编辑，其真正的大名叫`models/gemini-2.5-flash-image-preview`，`Nano banana`只是它的代号，但由于代号太火，以至于直接代替大名了。

![](img/b719c32659dc918037539ffd70d12114.png)

## 核心特点

相比较传统的AI图像生成模型比如Midjourney，`Nano banana`最大的特点在于不光能高精度生成图片，还可以理解图片中的物体，像神来之手般编辑图像，并将多个图片进行融合，宛如PS高手。

![](img/725bdc5bd6afee6ba65f750b3d1e4bc9.png)

我做了测试，确实能很丝滑的编辑图片，而且效果并没有什么违和感。

![](img/55dd59ff7b777888e500bd6db91e6323.png)

![](img/db9807715b74233443c18bc462017384.png)

这种效果对于我这种PS初级选手来说已经是叹为观止，图像编辑很自然，新增的内容能与原图背景融为一体。

## 使用方式

怎么使用`Nano banana`呢？

用`Nano banana`画图有两种方式，一个是直接在`Gemini`中直接使用图片生成模式，还有一种是在`Google AI studio`选择`Nano banana`模式。

我比较喜欢直接在`Gemini`中用图片模式来调用`Nano banana`，挺简单的。

![](img/65e373c34b295e0654bf45ea649cdbed.png)

除了编辑图片之外，`Nano banana`还有个非常让我惊讶的能力，就是根据用户提供的知识物料生成科普图片，非常适合自媒体传播或者教学科研。

比如我用`Nano banana`生成的两个案例，反正我是画不出来。

1. 解释如何搭建一个`ai agent`

![](img/52e8d252f772fb57c0bf2b58ed890f9e.png)

2. 介绍历代苹果手机（`iPhone1-16`）发展历史和特色

![](img/797e9612a3f05d906d88e85c6f2753b0.png)

这绝对不是网上找的图片，都是设计`prompt`让`Nano banana`生成AI图像。

## 实战操作

那怎么做呢？

以苹果手机科普图为例，首先是设计`prompt`，当然不需要自己去描述图像内容结构形式，我是让`DeepSeek`来生成`Nano banana`所需的`prompt`。

给到`DeepSeek`的任务如下：

> 我想用`Nano Banana`绘制科普图，请你帮我生成绘图`prompt`，内容是科普苹果历代手机（`iPhone1-16`）发展历史和特色，需要生动有趣简洁明了

`DeepSeek`根据需求生成适合`Nano banana`的`prompt`，而且给力多种方案。

![](img/caf60429c6fbdbc21e3ba4dde3e442df.png)

然后我把`prompt`复制到`Gemini`对话框，选择`Nano banana`绘图模式，它就直接生成刚刚看到的图像。

![](img/3ad5d776bafcd73bcd8fbfa64ca5908e.png)

我看到还有不少人制作复杂的科研图表，也是采用类似的方法，不过具体内容需要自己输入。

### 制作 3D 手办图

对于朋友圈那些好玩的手办图，其实做起来也很简单。

首先找到一张人物图，如下：

![](img/ae55cdb4518e88a63dc11bdbd2cbc7b3.png)

上传人物图让`Nano banana`生成3D手办。

`prompt`：

> Create a 1/7 scale commercialized figurine of the characters in the picture, in a realistic style, in a real environment. The figurine is placed on a computer desk. The figurine has a round transparent acrylic base, with no text on the base. The content on the computer screen is the Zbrush modeling process of this figurine.Next to the computer screen is a packaging box with rounded corner design and a transparent front window, the figure inside is clearly visible

![](img/94a270aa9d41828675c281add776540f.png)

你看很简单，不过可能需要更多的`prompt`微调，能手办看起来更加精细，和背景图融合的更自然。

## 总结

`Nano banana`确实拓宽了以往多模态图像大模型的能力边界，使得不再是被动接受AI随机的生成图像，让用户能用精确的指令按需生成和编辑，这是很大的创新啊。
