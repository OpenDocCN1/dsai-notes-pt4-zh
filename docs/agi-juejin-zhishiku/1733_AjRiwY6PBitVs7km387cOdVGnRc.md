# 黏土画风GET！采用Stable Diffusion的ComfyUI一键生成制作攻略!

> 来源：[https://agijuejin.feishu.cn/docx/SFrtdGUmjofdrZxVjH5czIQsnVg](https://agijuejin.feishu.cn/docx/SFrtdGUmjofdrZxVjH5czIQsnVg)

> 作者：德里克文

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/697061497)hihu.com/p/697061...

**黏**土画风太火了！以至于我打开社交媒体平台大家都在晒，我自助用Stable Diffusion也做了几个好有趣的黏土风格，快来看看吧！

![](img/b5f9dce42be3d1b0587a78a516c9af9e.png)

我用AI生成的帅哥来试试，丑萌丑萌的挺好玩的，再看看穿旗袍的美女！

![](img/1e8e0d33500e4e1640b52187eea6f503.png)

这个表情绝了，那个小眼神和嘴巴的表情太可爱了。

我再来试试正气十足的帅气小哥，这个黏土风的小表情真是把握了。

![](img/7ce99c19ab0c8e57337e01488c607e03.png)

是不是很有趣？难怪现在点开小某书的页面一搜，都是Remini的黏土风，热度也太高了。

![](img/3696dc04f3a164abbf7ea4358ab3f9de.png)

Remini这款AI修图App凭借“黏土风格”“瓷娃娃”这些滤镜在网络上迅速走红。

用户只需上传一张个人照片，即可借助AI技术变成有趣的黏土动画风格图像，这种风格的反差感迅速吸引了大家的注意。

在社交媒体如📕、🧣、抖音及朋友圈中，这个应用的黏土风格作品风靡一时，各种吃瓜群众的评价是“丑得迷人”，“超级可爱”，相关帖子热的发烫。

![](img/36dfc09d42adfa93b4c1740f23a079af.png)

对于国内大部分使用安卓系统的小伙伴来说，Remini需要Google Play，安装不便，而且收费也不便宜，订阅价格为68元人民币/周，大概一个月要300元人民币。

虽然新用户可以免费试用一周，但后续使用还是要付费的。

但是，我们采用Stable Diffusion的有ComfyUI工作流，可以一键复现这个效果，用最方便的方式和最少的费用来实现黏土自由！我在这里就教大家怎么操作吧！

## 一、成品效果

先来给大家展示下这个工作界面吧！

![](img/0b8b7b548c4472dff641cef3fce272e7.png)

这就是Stable Diffusion的ComfyUI工作流界面啦，别看上面很复杂的步骤，ComfyUI最大的优点就是全部集成好，只需要一键导入就自动配置好啦。

因此一点都不用担心配置麻烦，来看看我放入的美女在经过黏土风滤镜后是什么效果吧！

![](img/a5ed08baedf6a4929881a525e0ef0b76.png)

漂亮吧，接下来看看黏土风滤镜。

![](img/8c579f72e1b17f26a1200fb9c82b33b5.png)

是不是慵懒中带点可爱？无辜的小眼神和香肠嘴让人忍俊不已。

这个滤镜洋溢着那股特有的“拙朴美感”，让人耳目一新。

## 二、制作流程

接下来我就教大家Stable Diffusion的具体的操作流程。

### 1.准备工作

Stable Diffusion是一个开源免费软件，但是对电脑硬件有要求，建议显卡4070以上，显存至少16G，然后需要下载上百G的文件，同时还有繁琐的安装流程。

这个对于大部分只是想体验滤镜的小伙伴确实有点为难了。

![](img/fc4d4a3a38a0980c25d8396df676c77e.png)

[不过在互联](https://link.zhihu.com/?target=https%3A//www.lanrui-ai.com/register%3Finvitation_code%3D9071)网时代，我们还有云平台这个方便的工具，在 揽睿星舟 平台，已经有部署好的的一键云端启动，只需要点击后运行就可以了。

[  ](https://link.zhihu.com/?target=https%3A//www.lanrui-ai.com/register%3Finvitation_code%3D9071)

新用户享有2小时免费体验及完成新手任务后赠送的5元奖励，生成一张照片大概只需要8秒左右，2小时足够你白嫖生成好多张照片了。

注册完了以后，我们在首页进入，点击ComfyUI最新版

![](img/4114d0598a5097d20ed7fc1a0fb9a372.png)

然后输入名称便于辨别，建议点击自动停止，这样时间到了会自动停止。

![](img/d40e996aa1f029cb6e95f7256da400e4.png)

勾选“我已阅读和同意 平台协议”后，点击"立即创建"，等待一段启动时间（一般需要2~3分钟）等看到“打开应用”按钮变成彩色，就可以直接进入。

![](img/d1f7411ed076b3418c661ff671ce2f12.png)

### 2.Lora下载

进入以后，我们可以看到默认界面，看到这个内容是不是觉得有点懵逼，别着急一点都不难。

![](img/5f6d3295c55b74427bb7da4fb2d319db.png)

让我们先回到刚刚的那个界面，然后先把我们ComfyUI需要的工作流和Lora都安装上。

工作流文件我用网盘存好了，可以直接点击下面内容下载到本地电脑，准备好。

```
链接：https://pan.baidu.com/s/1B9_ryw92K7pwz9IFny5D0A?pwd=3nf0 
提取码：3nf0 
```

然后我们在网盘那里新建文件夹。


建立好文件夹以后，我们在文件夹放入资料，我们总共需要两个Lora，分别是

LoRA1：CLAYMATE - Claymation Style for SDXL，下载地址如下：

```
https://civitai-delivery-worker-prod.5ac0637cfd0766c97916cefa3764fbdf.r2.cloudflarestorage.com/model/2906/claymateV203.gkfW.safetensors?X-Amz-Expires=86400&amp;response-content-disposition=attachment%3B%20filename%3D%22CLAYMATE_V2.03_.safetensors%22&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=e01358d793ad6966166af8b3064953ad/20240510/us-east-1/s3/aws4_request&amp;X-Amz-Date=20240510T103751Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Signature=3d5bccf6ae4cf7a2796c173ee174759cb2171bbf0ad4ccf78f38a0f9d6a182c1
```

LoRA2：Doctor Diffusion's Claymation Style LoRA，下载地址如下：

```
https://civitai-delivery-worker-prod.5ac0637cfd0766c97916cefa3764fbdf.r2.cloudflarestorage.com/model/29747/ddMadeOfClayXLV2.RsHf.safetensors?X-Amz-Expires=86400&amp;response-content-disposition=attachment%3B%20filename%3D%22DD-made-of-clay-XL-v2.safetensors%22&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=e01358d793ad6966166af8b3064953ad/20240510/us-east-1/s3/aws4_request&amp;X-Amz-Date=20240510T103821Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Signature=7cb6d0fc6e5fcbc8decc112e44f9ec316cd5278fa3f1bef106370f20eba13997
```

揽睿星舟的数据传输，不需要自己下载到本地再上传，可以直接在线下载模型，只需要把我上面提供的下载地址按照下图所示，放入URL里面，点击创建下载任务就好了。


然后我们点击上方的“云”图标，看下下载进度，等2个Lora都下载好就可以了。

![](img/5fd8814f6862f8b5f53ec6efce311c94.png)

### 3.ComfyUI操作流程

回到ComfyUI界面，我们点击加载。

![](img/61ec93530af8de4028fafbbcb7323b58.png)

然后在弹出的界面选择第二部我们下载的文件。

![](img/d2038644749aab8f4d053374f587c8b3.png)

加载后的界面就变成这样，如果有没有的插件会出现红色框。

如果出现这个情况，我们可以点击管理器

![](img/a4f4e5f653784ebaa3ae4680d470e43a.png)

检查下是否有缺失的节点，正常情况下是没有的。

![](img/d9109f7b89a035a1162419c4c51162c7.png)

如果整个工作流就没有红色显示报错了，可以正常运行了。

### 4.参数设置

参数设置比较简单，几个地方设置下就好了。

首先是在“加载图像”的框内，点击图片上传，上传本地图片。

![](img/dc7d2f2e21679b1fa72daa407463646d.png)

然后在“Checkpoint&Lora”界面，选择大模型，我建议选择HELLOWORLD模型，更好贴近亚洲人像。

![](img/f4e76e34df1b3e2cfa3de150a1c98d25.png)

然后在Lora堆里面，分别选择两个Lora（就是我们之前下载的）

![](img/be92c0c74a7eabef6148f843732a7051.png)

接着来到IPAdapter这个界面，调整下宽度（建议保持默认1024像素），在加载器选择PLUS。

![](img/51bb1b7cf137c59f68f39b8fac0eebaf.png)

在ContrlNet这个界面，加载器那里选择 t2i-adapter-canny 这个模型就可以了

![](img/596dacd61fa57b98e311785289800d89.png)

最后点击执行，等待一小段时间，在图像界面就会出结果了，是不是很有趣。

![](img/665d8e0eda4cb59c7e50b9b6b1a0b6d0.png)

就这样，一个完整的黏土风流程就做完了，你不来尝试下吗?

## 结语

出来黏土风，还有很多很炫酷的工作流，在Stable Diffusion的ComfyUI界面都可以通过一键导入实现，快来云平台体验吧！

![](img/2ea73fc29fc6bf30a9b86968399564a6.png)

基本上我们看到的所有的AI绘画的应用，除了Mid Journey以外，都和Stable Diffusion这个伟大的开源平台不无关系，自己掌握Stable Diffusion这个工具，成为潮流的弄潮儿，你不来试试吗？

---

我是德里克文，一个对AI绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！