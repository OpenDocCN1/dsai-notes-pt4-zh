
# AI绘画新软件——DragGan初步研究

> 来源：https://agijuejin.feishu.cn/docx/C70idt8Rto3sMcxFl1Scdxp1nkh
>
> 作者：德里克文
>
> 原文：https://zhuanlan.zhihu.com/p/640903548

## DragGAN 介绍

DragGAN是AI绘图领域一款备受瞩目的创新工具。该工具是由MPII、MIT、宾大、谷歌等机构的研究者联合开发的，并在获得了SIGGRAPH2023广泛认可。就在6月底引爆AI绘图圈的DragGAN，正式发布了开源代码！

### 1. 项目源代码下载地址链接：https://github.com/XingangPan/DragGAN

[  ](https://link.zhihu.com/?target=https%3A//github.com/XingangPan/DragGAN)

### 2. 网络服务器试玩链接：https://huggingface.co/spaces/radames/DragGan

[  ](https://link.zhihu.com/?target=https%3A//huggingface.co/spaces/radames/DragGan)

DragGAN的问世使得我们能够通过轻松拖拽的方式，实现高精度的图像编辑。

![](img/8c16e60b92fabb7450dabee49f15a478.png)

DragGAN主要包括以下两个主要组成部分:

1. 基于特征的运动监控，驱动操作点向目标位置移动
2. 一个新的点跟踪方法，利用区分性的GAN特征持续定位操作点的位置

通过DragGAN，任何人都可以对像素进行精确的控制，进而编辑如动物、汽车、人类、风景等多种类别的姿态、形状、表情和布局。

由于这些操作在GAN的学习生成图像流形上进行，因此它们倾向于生成逼真的输出，即使在富有挑战性的场景下也是如此，例如被遮挡的内容和变形的图像。

定性和定量的对比表明，DragGAN在图像操作和点跟踪任务中，都明显优于先前的方法。

![](img/3f7dc2c2eb24a06cda6814de94e9090b.png)

### 拖拽编辑

在DragGAN的加持下，用户只需要设置几个操作点（红点），目标点(蓝点)，以及可选的表示可移动区域的mask(亮区)。然后，模型就会自动完成后续的图像生成。

其中，动态监控步骤会让操作点（红点）向目标点(蓝点)移动，点追踪步骤则通过更新操作点来追踪图像中的对象。此过程会一直持续，直到操作点达到其相应的目标点。

具体来说，团队通过在 [生成器](https://zhida.zhihu.com/search?q=%E7%94%9F%E6%88%90%E5%99%A8&zhida_source=entity&is_preview=1) 的 [特征图上](https://zhida.zhihu.com/search?q=%E7%89%B9%E5%BE%81%E7%A9%BA%E9%97%B4&zhida_source=entity&is_preview=1) 的位移修补损失来实现动态监控，并在相同的特征空间中通过最近邻搜索进行点追踪。

好了，听完了前面的介绍，是不是跃跃欲试呢？

## DragGAN本地运行实测

我在网上下载了这个软件的本地部署包，运行：

![](img/8391076121f048e75d8314d4d4f2487c.png)

### 1. 猫咪转脸图片尝试

接下来我打开了 [软件界面](https://zhida.zhihu.com/search?q=%E8%BD%AF%E4%BB%B6%E7%95%8C%E9%9D%A2&zhida_source=entity&is_preview=1)，兴奋打开了第一张图片，是一张可爱的猫，我来尝试下创建拖动点吧！

我点击猫咪的鼻子，设立红色点，然后再次点击设置蓝色点，自动出现了箭头。接着我点击左侧菜单栏Drag里面的Start按钮开始

![](img/b9d670532268384e56a9809475d89ae5.png)

经过步骤的计算，猫真的易懂了位置，转向了侧脸，第一个实验成功了！

![](img/a0eb689ab4322abb1160fed63d9b923a.png)

### 2. 猫咪闭眼测试

我重新打开一张猫咪图像，想要测试下是否能够做出闭眼低头的动作

![](img/43118d73f3c564bb3c954aac2ddf6d42.png)

我分别在耳朵、眼睛和嘴巴处设立了移动点。接着我再次点击左侧菜单栏Drag里面的Start按钮开始

![](img/6f2f7b95bde27f97bf66ddbb62fa6ef8.png)

结果可能因为点太多啦，图像跑崩了，直接闪退……好吧，我重来，分几步。

首先是我在MASK区域选择眼睛，然后再点击ADD POINT设置眼睛的点，设置好闭眼的距离后，点击START

![](img/5d4dbb48fb94e49ccd9b6244dac7870d.png)

可以看到，虽然没有把眼睛闭上，但是已经从一个大眼睛的无邪喵变成了一个怀疑人生的谨慎喵

![](img/8f2439a9755d1918beb12850d56725ca.png)

很好，我再继续尝试整活，耳朵张开，张大嘴巴，试试

![](img/70005a72064552b28facf9c4afb7c72e.png)

Start开始，额，走到300多步的时候就已经发现好像有点不对劲，眼睛怎么都快没了

![](img/bd8241c42f6bb7475fc8c04ddf54aab8.png)

最后出来了一个异形……这有点吓人……虽然看得出是猫的样子，但是眼睛没张开，我想要的嘴巴张开也没有，我觉得失败了

![](img/60af90834bea08a8b472f15212d3f0db.png)

### 3. 帅气男生形象

猫不行，我试试人可不可以转头

首先放上我自己用Stable Diffusion做的英俊小生。

![](img/f3d4f14154db8e721bec11ed09e4876c.png)

也放上Prompt便于想要尝试的朋友进行复现吧：

(8k, RAW photo, best quality, masterpiece:1.2), (realistic, photo-realistic:1.37), ultra-detailed, (high detailed skin:1.2), 8k uhd, dslr, soft lighting, high quality, film grain, Fujifilm XT3, professional lighting, photon mapping, radiosity, physically-based rendering，photo of 45 y.o man,medieval armor, He was a valiant king, tall and sturdy, with broad shoulders and a wide chest, and a majestic demeanor. His face was firm and handsome, his eyes and brows sharp as swords, his nose high and straight, his lips thin and tight, revealing a sense of determination and authority. His hair was black and thick, tied into a long braid that fluttered in the wind. His skin was dark and healthy, showing a kind of iron-blooded and masculine spirit. His hands were rough and strong, holding a dragon-carved sword that glinted with cold light. He wore a brocade robe with golden dragon patterns, a black pair of pants, a pair of boots, and a golden belt around his waist. He had a jade pendant hanging on his chest,

Negative prompt: (painting by bad-artist-anime:0.9), (painting by bad-artist:0.9), watermark, text, error, blurry, jpeg artifacts, cropped, worst quality, low quality, normal quality, jpeg artifacts, (signature), watermark, username, artist name, (worst quality, low quality:1.4), bad anatomy

### 4. 正面人脸尝试

然后我先用软件读取，可以看到只能读取面部，不过足够我这次实验使用了

![](img/bd93078f875bfc687e2693a8bc2a2801.png)

我来设置转头吧，分别将眼睛、眉心、鼻子、嘴巴都朝同一个方向移动，试试效果

![](img/056c764c715e57d6c53095c6c1ae7a63.png)

快跑到目标的时候，虽然确实是转头了，但是看起来不太妙

![](img/8baee671164a67e55affca260b34c1b9.png)

啊！！这是什么，失败了！！我不要看到这个丑八怪……果断关掉！！

![](img/8371e7d00e1c83c368c2d6e2e0dfb0b9.png)

### 5. 侧面人脸尝试

是不是正面有问题，我再换上另外一个侧脸试试，先上帅照

![](img/9135723dbd8fcb231e0b283d877f7e7c.png)

读取人脸信息

![](img/d52bba50bf540777bde9df70d642fd90.png)

这次设置小变化，就移动眼睛看看

![](img/e218a4bcee51a4b5be6ad11d1029db4c.png)

额，怎么说呢。。头发怎么变成纹身了，眼睛也好奇怪。感觉还是失败了

![](img/953f2fba91d616eadfc4a278a1332949.png)

看来就目前的技术还不够成熟，没办法做到我自己想象中的替代三维的效果。

希望能尽快完善技术吧……

---

我是 [德里克文](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3098715893%7D)，一个对AI绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！
