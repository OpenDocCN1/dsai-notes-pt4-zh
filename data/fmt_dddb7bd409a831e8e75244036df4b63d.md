
# Deforum | AI辅助设计，概念短片利器

> 来源：[https://aigcreative.feishu.cn/docx/Ai5adYlD7oBbUJxn4DIcxJSgnNM](https://aigcreative.feishu.cn/docx/Ai5adYlD7oBbUJxn4DIcxJSgnNM)

> 作者：leolee

> 链接：`https://zhuanlan.zhihu.com/p/684791788`

最近`deforum`支持`forge` `web ui`了，运行更加流畅了。官方也推出在线的`deforum` `stduio`，可以在线创作。

![](img/dee07e5b1338f41e3f4a2e55286ecd95.png)

## 什么是deforum？

大家可能还不知道`deforum`，这里简单介绍一下，它是一个AI生成视频的解决方案，主要擅长概念类型的视频创作，效果比较梦幻和艺术化。

这套AI视频解决方案，可控性高，输出的视频足够长，已经被不少广告公司用于商业化。风格非常适合做创意的包装，在商业项目中，客户也接受。已经有广告公司利用这个技术，包装广告片，成本低、效果好。大家有兴趣，可以在某书上搜索一下这类视频和商业项目。

但凡是工具，入门的门槛还是挺高的，`deforum`复杂的参数会让新手望而却步。我花了不少时间，研究了一下，帮大家踩了坑，本文将通过实际的案例形式，给大家讲讲利用`deforum`制作概念广告片的过程和步骤。

先看看效果，由于我的主业不是搞视频的，视频大佬不要见笑。视频效果仅为演示，如果要获得更好的效果，还是需要优化提示词和参数等。但是大概的意思有了。

![](img/0e76843d8e2f99c266ea4c37170b878c.png)

![](img/8dae0b46540f74bedb73681edba6427e.png)

重新播放

![](img/86dbd6094fedfe3c399acbfea9cb540d.png)

重新播放

项目将讲解制作流程。

## 制作流程框架

还是那句，非专业流程，仅供大家参考。

![](img/4906a65901ca6a23d785f69520af9af1.png)

## 用到的工具

以下是我用到的工具，如何下载和安装大家可以自行搜索。如果有需要的化，可以在本文留言，我会告诉大家如何下载。

1. `stable-diffusion-webui-forge`。为什么不用`a111`的`webui`，因为这个`forge`的效率更高，不卡顿。

1. `deforum`插件。装好`forge`后，直接在插件安装即可。

1. `After effect`插件 `motion bro` 4，用于设计镜头运动。

1. 剪影。简单的剪辑。

## 制作流程讲解

### 素材准备

我在网上找到了一个某奢侈品牌的手表视频，将作为概念和视频源输入。

![](img/a31703c21f55d96006526f263958544f.png)

### 整体概念构思

根据产品的视觉元素，我抽象了几个关键字：

+ 色彩：红、黑、少量的蓝；

+ 元素：绸缎、玻璃、金属、时间、钟表机械、绸缎、钻石；

+ 灯光：摄影室灯光，侧光、背光

为了表现艺术和技术的碰撞，同时带上奢侈元素，概念上还可以加上玫瑰和金做的蝴蝶。

以上构思仅为演示，望大家别见笑。

### 分镜头概念

有了主题概念，我们就可以进行分镜头的设计。根据关键字和风格，我们用`SD`分别输出相关的概念图。以下是部分概念图和提示语。

```
red lights are illuminated in a black  Conceptual painting,in the style of organic and flowing forms,metal,ue5,uhd image,marcel breuer,intense close-ups,layered surfaces,light-filled,incredibly absurdres,
```

![](img/0c4d8b56af72fa3e219a6ed5cdb63580.png)

```
Internal mechanical structure of the watch render by mike schmidt in photoshop,in the style of dark silver and light red,macro zoom,animated gifs,daniel arsham,canon 50mm f/0.95,aleksandr deyneka,deeply symbolic,
```

![](img/30dcc14a19bbbe2d43ef5b3a63394543.png)

```
a close up view of A round red diamond with red light,Placed on a metal surface,in the style of crystal cubism,video montages,intaglio,anamorphic lens flare,polished craftsmanship,kodak vision3,circular shapes,
```

![](img/62aa841e00df9507d3fa11105bb0c10d.png)

### 撰写`deforum`专用提示语

有了分镜后，写出`deforum`专属的提示语。

```
{
    &quot;0&quot;: &quot;red lights are illuminated in a black  Conceptual painting,in the style of organic and flowing forms,metal,ue5,uhd image,marcel breuer,intense close-ups,layered surfaces,light-filled,incredibly absurdres,&quot;,
    &quot;60&quot;: &quot;red strip lighting abstract pattern design red light beam nhd stock videos &amp; royaltyfree footage, in the style of cyberpunk futurism, detailed cross-hatching, puzzle-like elements, metallic rectangles, symmetrical, smokey background, lightbox,&quot;,
    &quot;120&quot;: &quot;a red abstract background image set,in the style of soft tonal transitions,uhd image,dark crimson and black,flowing fabrics,utilizes,abstract whispers,captivating,&quot;,
    &quot;180&quot;: &quot;the screen of a watch with red Ancient Roman clockt,with some black lines surrounding,in the style of anamorphic lens flare,uhd image,energetic lines,richard wright,intense close-ups,red and orange,&quot;,
    &quot;240&quot;: &quot;Internal mechanical structure of the watch render by mike schmidt in photoshop,in the style of dark silver and light red,macro zoom,animated gifs,daniel arsham,canon 50mm f/0.95,aleksandr deyneka,deeply symbolic,&quot;,
    &quot;300&quot;: &quot;Super macro shooting,Geometric concept patterns,in the style of digital art techniques,ron arad,light black and red,panel composition mastery,petros afshar,urban signage,interactive,&quot;,
    &quot;360&quot;: &quot;Under macro photography, take a top-down shot of a pile of burgeoning deep red roses in a dark style with a pure black background and two studio lights,&quot;,
    &quot;420&quot;: &quot;Red gold edged, intertwined, flowing silk ribbons, backlit shooting, black background,&quot;,
    &quot;480&quot;: &quot;a close up view of A round red diamond with red light,Placed on a metal surface,in the style of crystal cubism,video montages,intaglio,anamorphic lens flare,polished craftsmanship,kodak vision3,circular shapes,&quot;,
    &quot;540&quot;: &quot;Under macro photography, from above, a red mechanical butterfly flaps its transparent wings against a black but shimmering golden background. Backlit shots reveal golden patterns, reminiscent of the golden age,&quot;
}
```

这段提示语的意思是，分别在0、60....540，这几处关键帧，设置不同的画面元素。

要非常注意，这是`json`格式，务必注意书写的规范，否则会报错。

### `deforum`设置

这里仅列出我用到的设置。

#### 大模型选择

我选择了`dremshaperXL light`模型，该模型带`turbo`，可以在10步采样即可出图，大大提升效率

#### 运行设置

因为用了特殊模型，注意采样方法和迭代步数的参数。同时要注意视频的尺寸，后面`AE`输出镜头运动，需要用到。

![](img/35c92f3c64440b54ddadd73c84c52607.png)

#### 关键帧设置

#### 一般设置

最大帧数，根据自己需要的视频长度，自行设置就可以了，

![](img/79ae93c18c614950d64fa06bb6088d7d.png)

#### 运动

非常关键的设置，填入的参数就是`AE`生成镜头运动参数，如何生成，看下文，这里先记住填入的位置。

![](img/fbb9c0d1f0f7c48f1446252520f0a2d0.png)

#### 提示词设置

填入上文的提示词即可，正向和反向提示词，可以填入公共的提示词。

![](img/ecf0e071620a97b8f1576c94e2f2f890.png)

#### 初始化设置

勾选启用初始化，在图像链接，填入图像地址。初始化的意思，可以看我视频的开头，会以实际的产品变化到`deforum`动画。

![](img/5e793f85aa6c8e7ed9643439fe67b4bf.png)

#### 输出

这里主要是选择帧率。

![](img/e5d2a4f224a7bf1f3d8b357e4152803f.png)

### AE镜头运动

#### 新建合成

这里的设置，需要跟上文`deforum`的设置一致。

#### 添加`motion bro`的`3D`预设

将`3D`预设添加到合成中。

#### 设置运动镜头

添加完成后，`AE`会自动创建一些图层，一般我们就设置这里就可以了。

![](img/7804bf6dc4cff72ce08ba7d16e9653c1.png)

后面就是拖拉关键帧，设置曲线的，熟悉`AE`的朋友应该不会陌生，这里就不展开了。设置完成后，可以预览看看效果。

![](img/121a58d208ef93b1e92268145808c779.png)

#### 将镜头运动参数复制到`deforum`

将这些参数分别复制和粘贴到`deforum`中。

![](img/23b1a4a11d686afdf4d9d590a5715311.png)

最在`forge`点击生成，就可以进行生成了。最后要注意，`deforum`生成后，需要手动点击以下`show the video`按钮，才能看到视频。

![](img/3b3bf68f448f1737666975d4081e9bb3.png)

### 剪映剪辑

剪辑比较简单，把原视频加上，然后加上合适的转场，就可以了，这里不过多赘述。

![](img/8fc20e72919d991afd401de200c0f630.png)

好了，今天的分享就到这里。

`https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg`
