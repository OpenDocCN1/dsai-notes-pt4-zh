
# AI官网设计源码

> 来源：[https://xmgtoagi.feishu.cn/docx/YTwAdGDhnoQzJrx9YJIc3RfPnEc](https://xmgtoagi.feishu.cn/docx/YTwAdGDhnoQzJrx9YJIc3RfPnEc)

## 一、原版

大家好，我是小码哥，喜欢折腾AI的人，访问过`xmgai.cn`的网站都知道，首页还是比较low的，今天准备重新设计一下，在UI方面，我是不太擅长的，所以这次，还是借助AI来帮我完成官网首页的开发。先来看看我原来的官网，比较low，更多是图片效果的展示。

![](img/40b241794435a8d65bdb71117093e8db.png)

![](img/357e5c1ad1959c887abf8e708a10ad34.png)

![](img/69392a6bef0bd55ae0df5c11e4275e03.png)

![](img/d03e33ef47015fb56beb32fdf7eb664b.png)

![](img/844abe73e2a48dd0095b0b117405d480.png)

然后点击图片，跳转登录页面

![](img/98d5307056b2082bfd685772341f6c68.png)

### 1、通过媒体自适应移动端

![](img/c510da73a400639a87dff6552c739fb0.png)

### 2、源码

```javascript
{ src: "http://aipic.xmgai.cn/faceswap/11.png", description: "原图" },
{ src: "http://aipic.xmgai.cn/faceswap/22.jpg", description: "目标图" },
{ src: "http://aipic.xmgai.cn/faceswap/33.jpg", description: "成果" },
{ src: "http://aipic.xmgai.cn/faceswap/1.jpg", description: "原图" },
{ src: "http://aipic.xmgai.cn/faceswap/2.jpg", description: "目标图" },
{ src: "http://aipic.xmgai.cn/faceswap/3.jpg", description: "成果" },
{ previewImg: "http://aipic.xmgai.cn/model/5.png", name: "国风写实" },
{ previewImg: "http://aipic.xmgai.cn/model/23.png", name: "迷雾城堡" },
{ previewImg: "http://aipic.xmgai.cn/model/67.png", name: "薰衣草" },
{ previewImg: "http://aipic.xmgai.cn/model/2.png", name: "墨莲" },
{ previewImg: "http://aipic.xmgai.cn/model/97.png", name: "桃花" },
{ previewImg: "http://aipic.xmgai.cn/model/53.png", name: "怦然心动" },
{ previewImg: "http://aipic.xmgai.cn/model/6.png", name: "花嫁" },
{ previewImg: "http://aipic.xmgai.cn/model/4.png", name: "蝴蝶仙子" },
{ previewImg: "http://aipic.xmgai.cn/headpic/1-1.PNG", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/1-2.PNG", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/2-1.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/2-2.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/3-1.PNG", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/3-2.PNG", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/3-3.PNG", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/3-4.PNG", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/5-1.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/5-2.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/5-3.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/headpic/5-4.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/aipic/1-1.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/aipic/1-2.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/aipic/1-3.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/aipic/1-4.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/aipic/2-1.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/aipic/2-2.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/aipic/2-3.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/aipic/2-4.png", name: "点击图片制作" },
{ previewImg: "http://aipic.xmgai.cn/page/1.png", name: "AI绘画" },
{ previewImg: "http://aipic.xmgai.cn/page/2.png", name: "AI头像制作" },
{ previewImg: "http://aipic.xmgai.cn/page/3.png", name: "AI图生文" },
{ previewImg: "http://aipic.xmgai.cn/page/4.png", name: "AI换脸" },
{ previewImg: "http://aipic.xmgai.cn/page/5.png", name: "AI专属二维码" },
```
