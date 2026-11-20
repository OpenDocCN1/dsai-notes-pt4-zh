
# 利用`Stable Diffusion`进行文字嵌入图像的教程

> 来源：[https://agijuejin.feishu.cn/docx/HqiGdLrYGo5PHBxHFjVcTpIUnod](https://agijuejin.feishu.cn/docx/HqiGdLrYGo5PHBxHFjVcTpIUnod)

> 作者：德里克文

> 原文：[https://zhuanlan.zhihu.com/p/643811027](https://zhuanlan.zhihu.com/p/643811027)

最近，一种新的AI绘画创意形式开始引起大众的关注，它将文字和图像完美地融合在一起，形成了一种全新的表现方式。

这种创意让字与图相互交织，呈现出独特的视觉效果，引人注目，它所采用的就是`Stable Diffusion`软件下的`Controlnet`插件！

举个例子，就像我创作的这幅作品所展示的那样。接下来这篇文章将教你如何快速掌握文字嵌入图像的方法，内容很充实，建议你可以先收藏回看哦！

## 效果展示

![](img/ced09947712e8f9f2bbcbfdd7665512b.png)

![](img/3a7710b24981036a7c1c982eae672671.png)

![](img/99d35784fc0bc35eb67332e0a66ded35.png)

![](img/2936e903dbc54104234441476d33c776.png)

![](img/e9eeef142d1ce09c0d1e956eb7d11694.png)

![](img/dc16a32c86bc04e12ce4e14a55e91def.png)

大图不明显，可是当这个图片作为表情包或者小图出现在窗口的时候，是这样的效果

![](img/87e9a4662df7d7e17247af8d3a640d2f.png)

添加到🛰️或者🐧，用作表情包是不是很赞呢？

今天我就和大家分享下如何利用`Stable Diffusion`这个AI绘画工具与文字相结合，进行图像的创作。

另外需要说明的是，这个图片的进阶版本是制作艺术二维码。

艺术二维码是同样的原理制作，只不过二维码由于黑点的内容多，需要在识别性和整体效果上做一些妥协和调试，会更加复杂。需要对这个教程有初步认识后，采用同样的原理进行调试即可。

如果可识别性不够，可能就会出现下图效果。

![](img/20da7158767a4e25cd114f5327462905.png)

## 制作原理

主要是采用`Stable Diffusion`里面的`Controlnet`插件进行图片的制作，而在具体插件应用上，有多个`Controlnet`模型可以达到这个目的，但是出图效果不一，我个人觉得最为简单保险的是采用`Tile`模型进行超分输出。

![](img/6ecae6e8c082f8db07864280c192d92f.png)

## 制图步骤

下面我就来介绍下制作的步骤和过程

### 1. 制作文字底图

你可以采用`PhotoShop`、`美图秀秀`甚至`WORD`打字，将文字导出为一个图片文件（建议采用`PNG`格式，`JPG`也是可以的），可以是黑底白字，也可以是白底黑字。

主要起到的作用是类似于黑白蒙版的效果，如果你希望你的图片背景是深色的，就采用黑底白字。

![](img/47044dcb4b2d674efe7d931bed1a08f7.png)

需要注意的是，我建议大家生成一个1:1的图片，这样无论是后续横图还是竖图，都能够很好的居中应用。

### 2. 启动`Stable Diffusion`

进入`Stable Diffusion`这个软件后，我们可以输入我们想要生成的图片内容。

在这次，我希望生成的是一个赛博朋克风格的美女像，同时有霓虹灯便于凸显文字。

图像步骤我一般选择25就够了，想要出的更加精细可以到30。采样我选`DPM++`系列，分辨率设置为768*768。

这幅图片的`Prompt`也给到大家，便于大家出图尝试：

```
(Best quality:1.1),((masterpiece)), masterpiece, ultra high res, 1girl, (photorealistic:1.4), raw photo, solo, (cyberpunk:1.5), bare shoulders, black eyes, looking at viewer, smile, jacket, solo, turtleneck leotard, full body, （neon trim:1.2）, street, white hair, 8k uhd, dslr, soft lighting, high quality, film grain, photorealistic, dark studio, rim lighting, two tone lighting, (high detailed skin:1.2), volumetric lighting, candid, Photograph, high resolution, 8k, Bokeh, mecha musume, &lt;lora:A-Mecha-REN[color_theme,mecha musume, mechanical parts, robot joints,headgear,full armor]:1&gt;,
Negative prompt: NSFW, (worst quality:2), (low quality:2), (normal quality:2), lowres, normal quality, ((monochrome)), ((grayscale)), skin spots, acnes, skin blemishes, age spot, (ugly:1.331), (duplicate:1.331), (morbid:1.21), (mutilated:1.21), (tranny:1.331), mutated hands, (poorly drawn hands:1.5), blurry, (bad anatomy:1.21), (bad proportions:1.331), extra limbs, (disfigured:1.331), (missing arms:1.331), (extra legs:1.331), (fused fingers:1.61051), (too many fingers:1.61051), (unclear eyes:1.331), lowers, bad hands, missing fingers, extra digit, bad hands, missing fingers, (((extra arms and legs))), ng_deepnegative_v1_75t, badhandv4, lowres,bad anatomy,bad hands,text,error,missing fingers,extra digit,fewer digits,cropped,worst quality,low quality,No nipple，normal quality,jpeg artifacts,signature,watermark,username,blurry,bad feet,Wrong multiplayer structure,Confusing human body structure,Unclear human body,
```

### 3. 设置`Controlnet`参数

接下来我们在`Controlnet`界面插入刚刚做好的文字图片。

点选启用，完美像素，允许预览，如果显存在8G及以下的小伙伴可勾选低显存模式。

预处理器选择`Tile`，点击中间的爆炸图标，模型选择带`Tile`的。

控制权重需要自己根据出图情况调整，我这里建议选择0.65，开始控制步骤选择0.25，结束控制步骤0.75。

![](img/247a85c02a818a3f8143d27f2e1e08a1.png)

一切设置完成后，点击生成出结果。

![](img/c64a50a4748a2ff0a5f2d5d926830e03.png)

### 4. 放大分辨率

由于刚刚设置出图的分辨率是768，对于在电脑上看，这个精细度是不太够的，所以我们可以采用接力的方式提高分辨率。

读取将刚刚生成的图片信息，进行图生图，采用同样的提示词语及相同的随机种子。将重绘比例设置为0.3

![](img/b35d230a57b7e431060bf4c35258acd2.png)

然后点击`Tiled Diffusion`和`Tiled VAE`插件的`Enable`进行启动。

在`Tiled Diffusion`界面设置放大算法比例，我这里设置为2倍，即1536*1536分辨率的尺寸。记得在放大算法中选择`R-ESRGAN 4x+`。

![](img/84746f939f5acaa3fcd0ceb524f82d2c.png)

![](img/ec72a579b467ecb6b8d56c29dfffe037.png)

然后在`Controlnet`中再次采用`Tile`模型，进行加强控制

![](img/7b27ef516502670f05808976ccf85d2d.png)

![](img/bdd23fd93e58320a28ac74c72f74df5d.png)

最后点击出图

![](img/2ca7415f111e64465d3fb0cec1d44ab5.png)

![](img/78fce7293f6ae4f0858db308af285a1c.png)

就这样一副嵌入文字的图像就完成了，是不是很棒呢？

## 结语

如果喜欢这篇文章，觉得有收获的话，请不要吝啬您手中的赞同，感谢您的认可！

如果对AI绘画感兴趣，欢迎关注我，我会实时更新AIGC领域最新的相关讯息及分享我的AI画作！

![](img/06dcc539578d2c620758b8f5e8f29764.png)

![](img/0fb57c2118751fcf388277859a340906.png)

---

我是 [德里克文](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3052677444%7D)，一个对AI绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！
