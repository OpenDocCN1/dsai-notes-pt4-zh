# AI作画，如何生成远看时看着与垫图相似，实则完全不同的画？(例如，各种抽象版cxk)?

> 来源：[https://agijuejin.feishu.cn/docx/Q84rdkUASogpyNxlgNZclzgmn6g](https://agijuejin.feishu.cn/docx/Q84rdkUASogpyNxlgNZclzgmn6g)

> 作者：德里克文

> 原文：[https://www.zhihu.](https://www.zhihu.com/question/614324100/answer/3142558941)com/question/6143...

使用Stable Diffusion不难实现，首先我来展示下效果(一次看个够)：

## 一、图片展示(仔细看系列）

### 1.钢铁侠

![](img/b2e72c847283c1505436b669fbea6dee.png)

![](img/1e48caec031d732d9f19226e2cf119aa.png)

![](img/4980b918f4902f73865f199bdea5a9c7.png)

![](img/02ed180cfa411773a6b1184176f0733a.png)

![](img/4f2b90d8e95d7fe78e480470cb6b91b8.png)

![](img/ebb3cf48a813021c5047885e0aa7a188.png)

![](img/312179e7aa0a7f80e9fd4487156ba248.png)

![](img/c4d2271a7613cf9c7003303c8f9d3e4f.png)

这些图片的Prompt是：

```
1 iron man, Best quality, masterpiece, ultra high res, (photorealistic:1.4), city lights, raw photo, cyberpunk edgerunners, solo, (cyberpunk\), looking at viewer, full body, ((masterpiece)), cyberpunk, neon trim, street, white hair, 8k uhd, dslr, soft lighting, high quality, film grain, photorealistic, dark studio, rim lighting, two tone lighting, (high detailed skin:1.2), 8k uhd, dslr, soft lighting, high quality, volumetric lighting, candid, Photograph, high resolution, 4k, 8k, Bokeh, mecha musume, mecha musume, mechanical parts, robot joints, headgear, full armor, :1&gt;, masterpiece, best quality, masterpiece, best quality, &lt;lora:服饰LoRA:Mecha:1&gt;, &lt;lora:A-Mecha-REN(color_theme:0.9524), masterpiece,extremely detailed CG unity 8k wallpaper,best quality,Amazing,finely detail,cinematic lighting,RAW Photo, 8k Ultra HD, SLR, High Quality,8k, intricate detail, reality,realistic light, reality, photo reality, Deconstruction,Without humans,, masterpiece,extremely detailed CG unity 8k wallpaper,best quality,Amazing,finely detail,cinematic lighting,RAW Photo, 8k Ultra HD, SLR, High Quality,8k, intricate detail, reality,realistic light, reality, photo reality, &lt;lora:add_detail:1&gt; , lens  flare, Cinematic  Lighting
```

### 2.城堡

![](img/17f637abbfb1ea7994d289eef449d955.png)

![](img/fc71d1b5fddfe4c783e3197be742d15f.png)

![](img/339e62427a1330f744b1d05ddf2acf24.png)

Prompt是：

```
Winding river valleys, steep cliffs, verdant vineyards, historic castles, picturesque landscapes, charming villages, BREAK,Comfortable clothing, walking shoes, light jacket, hat, sunglasses, sunscreen,pixel art, (intricate details:1.12), hdr, (intricate details, hyperdetailed:1.15), (natural skin texture, hyperrealism, soft light, sharp:1.2), game art, key visual, surreal，&lt;lora:画风LoRA：GhibliStyle:1&gt;, masterpiece,extremely detailed CG unity 8k wallpaper,best quality,Amazing,finely detail,cinematic lighting,RAW Photo, 8k Ultra HD, SLR, High Quality,8k, intricate detail, reality,realistic light, reality, photo reality, Deconstruction,Without humans,, masterpiece,best quality,
```

### 3. 崇山峻岭

![](img/7b0ba7b8d933aa5a6ee58cee3c0490bb.png)

![](img/960b498b2462023e5b6d94465ee40fa5.png)

![](img/5717cf2e5982064960b4ecca2cd72dbd.png)

![](img/749012313fbc7a63921e3fe7fe18def9.png)

![](img/9b968c30abc2cdaec18ae8dd0881c3fc.png)

![](img/004b91cbc9a63b325d1ac61cf99c1dc7.png)

除了上面的图片，还有另外一种玩法，你觉得怎么样：

##  二、图片展示(不言而喻系列)

### 1. [时空转移](https://www.zhihu.com/search?q=%E6%97%B6%E7%A9%BA%E8%BD%AC%E7%A7%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3142558941%7D)

![](img/90b8d1a41ac48e4347a5801892d91d3b.png)

### 2.像素世界

![](img/d741e55a08a7768df4a14cfae056ea78.png)

![](img/94802d9bba3a430b37f98b1cbd684ab6.png)

![](img/2d1a3042ee2440ae751d60901e6e2fda.png)

![](img/95316b10521068507d3da8bbe3687d2d.png)

### 3.多彩世界

![](img/db3e7f7464b8c845498875e7bd1ac79e.png)

![](img/c2fa347fc40bff04b3a9664b22c827a3.png)

### 4.认真打球系列

![](img/23360783c173775dd09b2c039d564245.png)

![](img/1253de723e160232d0bbbc5824dcb954.png)

![](img/d74271d9d46f31b91135356ed6e979ca.png)

## 制作原理

[这个并](https://www.zhihu.com/search?q=%E6%8F%92%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3142558941%7D)不复杂，生成原理挺简单的，就是通过Controlnet 插件 的Tile模型进行实现，关键步骤不在于Prompt，而在于Controlnet的应用，这个原理类似于我之前写的利用文字嵌入的文章，感兴趣可以点击查看：

[  ](https://zhuanlan.zhihu.com/p/643811027)

如果对于Stable Diffusion的操作不太熟悉，需要从头入门的新手，也可以查看下我这个教程哦：

[  ](https://zhuanlan.zhihu.com/p/645820130)

如果觉得好玩的话，记得点赞收藏哦~！

---

我是 [德里克文 ](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3120995466%7D)，一个对AI绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！