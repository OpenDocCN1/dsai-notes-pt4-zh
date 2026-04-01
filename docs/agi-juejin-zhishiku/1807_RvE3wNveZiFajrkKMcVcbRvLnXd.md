# 一键实现照片光影重现——IC Light工作流操作分享 - 知乎

> 来源：[https://agijuejin.feishu.cn/docx/D6YhdQCVfosXp2xLtyCcpvqsnSd](https://agijuejin.feishu.cn/docx/D6YhdQCVfosXp2xLtyCcpvqsnSd)

> 作者：德里克文

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/698541725)hihu.com/p/698541...

只需要一张照片，一键调整背景及光源！这么神奇的事情你要尝试下吗？

**我实测了AI绘画插件IC-Light在ComfyUI的**效果，下面让我来分享操作流程及工作流吧！

![](img/dc7cf261122d1aad8769ce976f18345a.png)

![](img/b5990d2ec91436e39c4b625287ef4c0c.png)

![](img/8982795d32ed7261800c8a75c6e8e375.png)

前一段Controlnet的作者张吕敏IC-Light这个插件的发布，让我们看到了直接用一张照片迅速调整光影及背景的可能性。

只需要在搭建好的工作流上调整提示词，整个画面效果由你操控，你不来试试吗？

![](img/a7b0c71a60b42d141341f0f9199810a4.png)

而现在这个功能已经在在线的云平台揽睿星舟上实装，我来分享下这个工作流及操作吧！

## 一、准备工作

[Stabl](https://www.zhihu.com/search?q=%E5%85%8D%E8%B4%B9%E8%BD%AF%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)e Diffusion是一个开源 免费软件 ，但是对电脑硬件有要求，建议显卡4070以上，显存至少16G，然后需要下载上百G的文件，同时还有繁琐的安装流程。

![](img/56ab46931c5deed48449442314e56ec1.png)

这[个对于大部](https://link.zhihu.com/?target=https%3A//www.lanrui-ai.com/register%3Finvitation_code%3D9071)分只是想体验滤镜的小伙伴确实有点为难了。不过在互联网时代，我们还有云平台这个方便的工具，在 揽睿星舟 平台，已经有部署好的的一键云端启动，只需要点击后运行就可以了。

[  ](https://link.zhihu.com/?target=https%3A//www.lanrui-ai.com/register%3Finvitation_code%3D9071)

新用户享有2小时 [免费体验 ](https://www.zhihu.com/search?q=%E5%85%8D%E8%B4%B9%E4%BD%93%E9%AA%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)及完成新手任务后赠送的5元奖励，生成一张照片大概只需要8秒左右，2小时足够你白嫖生成好多张照片了。

注册完了以后，我们在首页进入，点击ComfyUI最新版

![](img/6c53ad09cde05fb3192e83151917474b.png)

[然后输入名](https://www.zhihu.com/search?q=%E6%97%B6%E9%97%B4%E5%88%B0%E4%BA%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)称便于辨别，建议点击自动停止，这样 时间到了 会自动停止。

![](img/101b13a14f3e7f78f3438b1324bd9ac6.png)

勾选“我已阅读和同意 [平台协议 ](https://www.zhihu.com/search?q=%E5%B9%B3%E5%8F%B0%E5%8D%8F%E8%AE%AE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)”后，点击"立即创建"，等待一段启动时间（一般需要2~3分钟）等看到“打开应用”按钮变成彩色，就可以直接进入。

![](img/bf9ab411838f4fd499d09f6daa19acbf.png)

[让我们先](https://www.zhihu.com/search?q=%E5%B7%A5%E4%BD%9C%E6%B5%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)回到刚刚的那个界面，然后先把我们ComfyUI需要的 工作流 安装上。

工作流Workflow [网盘 ](https://www.zhihu.com/search?q=%E7%BD%91%E7%9B%98&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)链接：

```
[htt](https://www.zhihu.com/search?q=%E6%8F%90%E5%8F%96%E7%A0%81&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)ps://pan.baidu.com/s/19_2C4LRrFc_prbKOOBF7Yg?pwd=bzbl
提取码：bzbl 
```

我们先下载到本地电脑，记住储存位置。

## 二、ComfyUI操作流程

然后回到ComfyUI界面，我们点击加载。

![](img/36bc70a690835f8e0d9816e292f823a8.png)

然后在弹出的界面选择刚刚我们下载的文件。

![](img/99e406dbe6404b13f2eec89c0da89c71.png)

加载后的界面就变成这样，如果有没有的插件会出现红色框。

[如果出现](https://www.zhihu.com/search?q=%E7%AE%A1%E7%90%86%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)这个情况，我们可以点击 管理器 。

![](img/46a66d7420c3e6b4211b4d0e36bfeac9.png)

检查下是否有缺失的节点，正常情况下是没有的。

![](img/be1b99d26f03322056aff860aae08965.png)

如果整个工作流就没有红色显示报错了，可以正常运行了。

## 三、参数设置

参数设置比较简单，几个地方设置下就好了。

参数设置比较简单，几个地方设置下就好了。

首先是在“ [加载图像 ](https://www.zhihu.com/search?q=%E5%8A%A0%E8%BD%BD%E5%9B%BE%E5%83%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)[”的框](https://www.zhihu.com/search?q=%E5%9B%BE%E7%89%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)内，点击图片上传，上传本地 图片 。

![](img/6277efd77142c96a2b652be32fbf97ad.png)

然后在图像界面根据输入图像的像素输入对应的比例，例如3:4，你可以输入宽度900高度1200，或者1200*1600的尺寸。

![](img/c7bcd4455bad891f7a719f6ef4d78e55.png)

接着在提示词界面输入你想要的背景及光源。

![](img/0bb5cd9a6a328375e2f00a17f6d19f81.png)

我还设置了一个高清放大的工作流，默认放大2倍，你们也可以自己设置。

![](img/a88a5558f66e75ad3f5dd8ca9731841f.png)

然后其他参数保持默认不要动就好。

最后点击执行，等待一小段时间，在图像界面就会出结果了，是不是很有趣。

![](img/4531802e41735dd3826764201a514454.png)

就这样，一个完整的照片 [光影重现 ](https://www.zhihu.com/search?q=%E5%85%89%E5%BD%B1%E9%87%8D%E7%8E%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)就做完了，你要不要来试试?

## 结语

有了这个插件，人像摄影， [商品摄影 ](https://www.zhihu.com/search?q=%E5%95%86%E5%93%81%E6%91%84%E5%BD%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)，建筑室内设计等等需要主体和背景光源结合的内容，通过这个插件可以很轻易解决前景主体与背景的衔接，以及对光源的控制问题。

![](img/6b286a32c62b58d728aca4b045dd80df.png)

这将是 [AI绘画 ](https://www.zhihu.com/search?q=AI%E7%BB%98%E7%94%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)从玩具逐步走向生产力工具的重要拼图。

希望你能掌握好这个工具，创造更多的奇迹。

---

我是 [德里克文 ](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A698541725%7D)，一个对AI绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！