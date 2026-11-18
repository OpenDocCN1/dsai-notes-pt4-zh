
# “瞬息全宇宙”-利用图片生成AI视频的保姆式教程

> 来源：[https://xuqiwei1986.feishu.cn/docx/VQZOdfQ3XoIzdQxMAZ5c6AVenBf](https://xuqiwei1986.feishu.cn/docx/VQZOdfQ3XoIzdQxMAZ5c6AVenBf)

## 开篇说明

1、看完本篇文章能得到什么？

+   学会如何利用SD插件`deforum`制作AI视频；

+   共同碰撞出创作的火花...

2、以下内容对电脑配置没有要求，不吃显卡；

3、建议使用电脑阅读，体验更佳。

4、即使以上几点都不满足，也没关系，文末提供了个平替尝鲜版解决方案。

## 起源

看到了星球里这两天 元峰 和 南山月 两位大佬分享的利用2D图片变成3D视频的文章，发现好多人对利用AI生成视频这块蛮感兴趣的；同时也发现了通过星球里大家的分享和共创，能碰撞出💥很多有意思的想法和实践。这是我特别喜欢的社群氛围，所以连夜赶出了这篇文章，希望对你们有帮助。

## 正文

本文分为四个部分：

1. 全网最简单最轻松的SD部署方法；
2. 生成AI视频的实操教学；
3. 平替尝鲜方法。
4. 应用场景探索

### 一、全网最简单最轻松的SD云部署方法

已经安装了SD的小伙伴可以直接跳过这部分。

网上有很多的SD一键部署安装包和方法，但是对于电脑显卡配置不够和Mac用户都不太友好，而且下载安装的过程也要花费不少时间。本次会教你一个从安装到使用仅需15分钟的方法。

**缺点：**最近白嫖算力的人太多了，导致不稳定，特别容易断开，且用且珍惜。所以有条件的话还是安装在自己电脑上，这里推荐一篇星球吴东子大佬的教程： [《耗时80小时!超详细的胎教级Stable Diffusion使用教程，看这一篇就够!》](https://wx.zsxq.com/dweb2/index/topic_detail/181484128555882)

> 注：不要被下面截图中密密麻麻的代码吓住，我们不用看代码，只要鼠标简单几个点击操作就行。

以下操作步骤：

1、提前准备好谷歌账号；

2、打开camenduru大神制作的一键`Colab`云端部署链接：https://github.com/camenduru/stable-diffusion-webui-colab；里面有很多很多模型，英文看不懂也没关系，接着往下看；

3、这里我个人常用`majicmix_realistic`模型，可以按Ctrl+F搜索“`majicmix_realistic`”找到，点击前面的nightly。（有经验的小伙伴也可以自行选择喜欢的模型）。

> 三者的区别如下：

> `lite` 具有稳定的 `WebUI` 和稳定的已安装扩展。

> `stable` 具有 `ControlNet v1.1`、稳定的 `WebUI` 和稳定的已安装扩展。

> `nightly` 具有 `ControlNet v1.1`、最新的 `WebUI` 和 `PyTorch 2.0`，以及每日安装的扩展更新。

![](img/29ff002ef90240f2b03095f7d7fe4767.png)

4、打开`Google Colab`界面，如果没有登录的需要先登录下`Google`账号，按图示123复制到自己云端硬盘。

![](img/b425aaaf3563fb37b5da546149602438.png)

5、复制完后会出现一个副本，点击红圈中的三角形运行。

![](img/df1054040df8313f828b9f2b355cfe5d.png)

6、运行时会提示一个警告⚠️，是因为白嫖的人多了，查的严，大家且用且珍惜，这里我们点“仍然继续”

![](img/aba4286376ca231d9a06fa88856c6de6.png)

7、运行时会判断有没有`GPU`资源，大家耐心等待即可。（如果分配不到资源，换个时间段再试）

![](img/751e585df38c73e6a655bc70b6f1d42e.png)

8、需要不时看下页面，中间可能会出现授权、验证什么的，大家按照要求点击即可，这里我没有出现就没法给大家截图了。

9、预计等待10-15分钟左右，会出现以下界面，点击最下方那个链接，就可以打开SD的`WebUI`操作界面了。

![](img/1b623cf45f0055da5c59d8be07bbd948.png)

10、恭喜你部署成功了！！！

![](img/a68b25f2119dc1fe139ad81417c62fbb.png)

11、下次使用只需要打开自己的`Google Drive`，找到下图这个文件双击打开后重复以上第5-10步即可。

![](img/1a306874c195a2d41fbb6e43794e2ef9.png)

### 二、图片生成AI视频的实操教学

#### 1、利用文生图获得一张5岁时的图片

这里我先用女神的头像作为参考图生成一张5岁时的照片，画红圈的部分是重点，其他保持默认即可。

![](img/31cf87e99f2a1fcd27d99d2820236838.png)

这里鼓励大家使用`zelinAI`生成，效果更好

![](img/c8a1806b69688738b2bafa1ae358d6bc.png)

同时给出我案例上使用的提示词，方便大家参考↓

正向提示词：

```
5-year-old girl, facing the camera, Smile,(best quality:1.4), ((masterpiece)),((realistic)), (detailed),(photorealistic:1.4)
```

反向提示词：

```
paintings, sketches, (worst quality:2.0),(normal quality:2.0), (low quality:2.0), lowres, ((monochrome)), ((grayscale))(monochrome:1.1), (shota:1.5), ((disfigured)), ((bad art)),((NSFW)), bad-hands-5
```

这里需要用到`Reference Only`插件，来实现人物相似度和风格的统一，参数设置如下。配置完就可以点击生成了，效果都还不错，我选了一张个人认为最像的。

![](img/03841c7491928748e2ba1d9f182344f4.png)

把`seed`值保存下来，等会要用到。

![](img/6affb6c67feb4d544b5514dad284a216.png)

#### 2、使用SD `WebUI`上的`deforum`工具

这里的配置项比较多，我都截图用红圈标出来，剩下的先保持默认即可。每个参数的含义我会在后面汇总给大家。

首先是运行选项卡：

![](img/ddfce27e4be1a005c962a8172ca58351.png)

关键帧选项卡：

![](img/d1190ddbdf14da48b6b669a6ac23e55a.png)

![](img/9195bd81695c6553ac46389b07f6439a.png)

提示词选项卡：（这里是关键）

相信眼尖的小伙伴已经看出来了，提示词和反向提示词都是沿用的之前第一步文生图的。只是分别在第0、30、60、90、120、150帧的时候设置不同的年龄，其他保持不变，需要按照它的格式来写。

![](img/bdf7b76bfc60cb35bc7452f7a9fa3b0e.png)

接下来3个选项卡我们先不管

![](img/1ebac0749a937cc110d910ef19af2c62.png)

最后的`output`：

![](img/8b7a2668abd151c9d2e1f008151ffcbd.png)

点击生成后，接下来就是等待了，一般需要4-6分钟不等，那我们就趁这个时间来看看之前那些配置项都是啥意思

![](img/762c9704ce38302488e77e0c49d49d4a.png)

#### 3、常用的参数配置项说明

运动设置`Motion`

> 运动设置是 `Deforum` 中最常用的一些选项。可以通过简单地更改它们和提示词来制作非常酷炫的视频。因此，我们应该很好地掌握运动设置的工作原理以及它们的作用。

> 以下介绍两种最常用的动画模式

> `2D` – 将图像视为 2D 并执行各种变换，如缩放和旋转以创建运动幻觉效果。

> `3D` – 将图像视为 3D 场景的视图。可以在任何 3D 操作中移动相机的角度。

> `2D` 运动设置

> `2D Zoom`：使用缩放功能放大或缩小图像。使用大于 1 的缩放值进行放大，使用小于 1 的缩放值进行缩小。该值离 1 越远，缩放速度越快。

> `2D Angle`：使用 2D 角度旋转图像。正值逆时针旋转图像，负值顺时针旋转图像。值越大，图像旋转速度越快。

> `2D Translation X`：使用平移 X 将图像横向移动。使用正值将图像向右移动，使用负值将图像向左移动。

> `2D Translation Y`：使用平移 Y 上下移动摄像机。使用正值下移图像，使用负值上移图像。

> `2D Transform Center`：变换中心用于更改缩放和/或旋转的焦点。X 和 Y 的默认值均为 0.5，这是图像的中心。（X， Y） = （0， 0） 是左上角，（1， 1） 是右下角。有关其他常见位置，请参阅下图。

![](img/f7ed9e9d9be6996d096511e3e50c8f26.png)

>

> `2D Perspective flip`：透视翻转对图像执行类似 3D 的转换，以创建一些很酷的效果。

>

> **3D 运动设置：**

> `3D Translation X`：平移 X 将摄像机侧向移动。正值会将摄像机向右移动。负值会将摄像机向左移动。

> `3D Translation Y`：平移 Y 上下移动摄像机。使用正值可向上移动相机。负值将向下移动相机。

> `3D Translation Z`：3D 中的平移 Z 类似于 2D 运动中的缩放。

> `3D rotation X`：旋转 X 使摄像机绕 X 轴旋转。

> `3D rotation Y` ：旋转 Y 使摄像机绕 Y 轴旋转。

> `3D rotation Z`：旋转 Z 轴使相机绕 Z 轴旋转。

看到这里，估计视频差不多生成好了！！

其他更多参数选项大家可以看这篇文章，写的非常详细：`Deforum` – 关于参数和设置的综合指南

#### 4、使用剪映配上音乐/字幕

等进度条跑完，点击这里即可预览视频，

![](img/0e1eca364e80568f1f3fd6e36c269e40.png)

预览没问题就可以把视频下载下来了：

![](img/b403c2963ca783ebe14b3cfd4f911a68.png)

然后打开剪映配上自己喜欢的音乐了，这里我用了大鱼海棠的背景音乐，也一起分享给大家：

![](img/e9f7900e745965843ece13658f783e39.png)

[这短短的一生.mp3](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SlZ0bEKVLo07XoxKI6RcVyB6nzg)

![](img/44623d7886bc2dc4dd69679faab0c6f9.png)

#### 5、成果展示：

[这短短一生-deforum制作.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CFvQb6iEho1kSAxHaQwc4t00nZf)

#### 6、One More Thing

你以为`deforum`仅仅只能做到上面这样吗？一起来看看更多作品吧，同时也期待大家一起碰撞出不一样的火花！

![](img/d849bc764bc5c850bc70d3a121d381f5.png)

[刘亦菲-星战变身2.m4v](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/U7BdbjuUOohHhexgjQIcS1Einke)

![](img/121b95b6f9549f1b16eb0990389bbdbc.png)

[国外大佬视频.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/L8sCbcnzdoOkrfx6q2EclCiWnCh)

第二个视频是国外一个大佬做的，眼里蕴含了一个世界，创意很棒。

### 三、平替尝鲜方案

因为上面的方法还是需要SD部署、算力资源...等等的一些门槛，如果想先简单试试效果的，那么这里我提供一个简单的利用剪映app制作的平替方案。

缺点在于受剪映模版限制，发挥空间较小。

步骤如下：

1、首先安装个手机剪映app

2、剪同款-搜“瞬息全宇宙”

![](img/f0f8f8d0c21335e927acab442cb6df1f.png)

3、官方有提供了八个模版，选择你喜欢的模版，上传图片

4、等待生成，每个人有3次免费体验的机会

![](img/5f8452e44438e516c4d22f3bbc6999c4.png)

5、生成完成，看看效果吧↓

![](img/488f9a9158c85c404c6fbe58f2c746b1.png)

[剪映版-瞬息全宇宙.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GtDZbD5KloregBxikvTc3XIwnZc)

### 四、应用场景探索

#### 1、制作事物演化的短视频

首先问`zelinAI`有哪些事物演化的场景->让AI把场景细化成几个片段->把每个小片段转化成`Stable Diffusion`的提示词->翻译成英文填入SD的`deforum`中->生成AI视频

![](img/d3331564459efebd2366d09edc9128e8.png)

![](img/4cd78693d0b873ad67965bb12c09667a.png)

![](img/fe2f53f44d9c7f0bde3b596e2100537f.png)

更改这部分提示内容↓

![](img/f8723a46d3f89140905e13a3bd3988f5.png)

#### 2、拍摄一镜到底短片

同样可以问下AI有哪些好玩的一镜到底场景，然后一步步追问，引导它给出提示词

#### 3、描绘一场梦境

甚至你可以随意组合每个帧数下的提示词，这些提示之间没有任何逻辑也没关系，`deforum`可能会给你一些惊喜

.....

一个人的力量是有限的，而一群人的力量无穷，期待大家更多有趣的想法和实践

> 总结下来，`deforum`虽然有一定的操作门槛，但是却有非常广阔的想象空间。大家可以先尝试模仿一些国内外大神的创作，逐步做出自己理想中的作品。

## 友情链接

[`LeiaPix Converter`：免费将图像 2D 转换为 3D](https://esouqpttmy.feishu.cn/docx/V3NWdI08Yo4R9lxcIdScEMwanWb)

[酷毙啦，孩子2D照片变3D视频，结合`zelinAI`等Ai工具剪辑成故事，孩子看了不止十遍](https://laqomfhmseb.feishu.cn/docx/JWkvdNcqRo3GNJxnY4rcqHuRnNb)
