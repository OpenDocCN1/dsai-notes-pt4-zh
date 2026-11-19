
# 网易｜使用AI工具辅助虚拟演播厅设计方案落地

> 来源：[https://aiyanxishe.feishu.cn/docx/QdM7dFQT7oE3XPxuxurcMEUcnCe](https://aiyanxishe.feishu.cn/docx/QdM7dFQT7oE3XPxuxurcMEUcnCe)

相信大家已经看过很多AI辅助设计流程的文章，AI正在渗透着各行各业。

本文主要介绍如何通过AI工具辅助游戏虚拟演播厅视觉概念稿，通过Midjourney和Stable Diffusion的组合，实现项目快速落地。

![](img/8ebb50a6827618c985615c21482f7e8f.png)

全篇内容主要包括三部分：

01.拆解主题元素
02.分区生成元素
03.合成细化

![](img/04bf14f90c2bc7bf8f53ed89b74c22c4.png)

## 拆解主题元素

首先需求方提出这次的主题“龙虎争霸”，希望有对抗的氛围。经常做演播厅的同学应该知道，演播厅常分为三个部分：解说区域，背景板区域以及背景。

对此，我们可将区域拆解分开制作，提高效率。先根据主题和KV构思下场景的组成：龙，虎和山海，国风的主题比较贴合。

![](img/fa3ee70ba6d71b98669beb90a595b1d1.png)

游戏KV

![](img/57bbb55daf9f2532eb8416be94296ae5.png)

## 分区生成元素

确定元素我们就可以开始在Midjourney生成一系列符合条件的元素。首先从背景的龙和虎开始。

prompt：

```
Chinese ink style,Chinese Gongbi style,China's sacred animal, The mythical tiger stands on the cliff looking up and roaring, in the style of otherworldly paintings, majestic ports, maritime scenes, layered imagery with subtle irony, mote kei, cybermysticsteampunk, exotic landscapes --ar 121:68 --s 750 --niji 5
```

![](img/6ab1706adb7071417f4ef791ea129974.png)

也可以用Niji模式直接生成，有时会有意想不到的结果。对于龙虎，我想加入一些风格化的感觉，所以在提示词加上水墨和工笔风格，大家也可以多多尝试。

prompt：

```
Chinese ink style,Chinese Gongbi style,China's sacred animal, The mythical tiger stands on the cliff looking up and roaring, in the style of otherworldly paintings, majestic ports, maritime scenes, layered imagery with subtle irony, mote kei, cybermysticsteampunk, exotic landscapes --ar 121:68 --niji 5
```

![](img/41175c65f4ce300bac04a9fb8cc68948.png)

prompt：

```
Chinese ink style,Chinese Gongbi style,black and white dragon flying in the air, in the style of otherworldly paintings, majestic ports, maritime scenes, layered imagery with subtle irony, mote kei, cybermysticsteampunk, exotic landscapes --ar 121:68 --s 750 --niji 5
```

![](img/0e2bcc5aeee92bba26930e6d30383b28.png)

其实可以看到，龙虎的提示词都差不多，也就改了主体元素而已。

对于解说区域，大致在PhotoShop处理一下想要的效果后（不用特别注重细节，主要是轮廓和大结构，直接生成很难达到想要的效果，所以需要人工处理），处理完进入Stable diffusion里面细化。

1. PhotoShop预处理

利用一些简单的素材在PhotoShop搭个解说区域大体的框架，如下图所示：

![](img/1b9d9426f75e1bd512ceaabd7bc6ea5b.png)

2. 进入Stable Diffusion，选择模型，填写提示词，设置ControlNet和常用Lora

正向：

```
world masterpiece theater,highres,best quality,ultra detailed, official art,novel illustration,8k wallpaper,concept art,Chinese style, jade round base, metal ring, blue jade carving, flower pattern, high precision texture, steps, jade, wood grain, marble, flat base interpretation Zhuo, table, backdrop, shaped, reference artstation, wave structure, reef, sea, waves, white water, soft light, fine material, blue and light green color scheme
```

（世界杰作影院,高清,极品,超细腻,官方艺术,小说插画,8k 壁纸,概念艺术,中国风,玉石圆底,金属环,青玉雕刻,花纹,高精度纹理,台阶,玉石,木纹,大理石,平底演绎,桌子,背景墙,异形,参考艺术站,波浪结构,礁石,大海,波浪,白色水域,柔和的光线,精细的材料,蓝色和浅绿色的配色方案）

反向：

```
EasyNegative, (worst quality, low quality:1.4), (realistic, lip, nose, tooth, rouge, lipstick, eyeshadow:1.0), (dusty sunbeams:1.0),, (abs, muscular, rib:1.0), (depth of field, bokeh, blurry:1.4), (greyscale, monochrome:1.0), text, title, logo, signature,watermark
```

（常用的反向提示词）

![](img/29b9ae7d46982235473f4ca2c43c6422.png)

![](img/dd44524411f34de846d40bb45b4bcad1.png)

打开高分辨率修复

![](img/b744b72f0b6955c6992d42523b8d6444.png)

设置Lora（这里选择中式建筑和细节添加）

![](img/27e54c0086ba8c353d33b5b5aaa5c31b.png)

ControlNet设置

设置完成点击生成，多生成几张，最后挑选1张比较适合的作为备用，最终选择了下图：

![](img/7b95809c2dd8b6d41f08b6771621678f.png)

可以看到对于白色区域AI识别出现了错误，不过我们想要的主体区域已经出来，最后处理一下就好。

同样对于背景氛围这一块也是进行了上述同样的操作（这里不再赘述，可以多多尝试）：先手动使用PhotoShop预处理再AI细化，下图中的一些建筑用到了游戏里的素材。

![](img/49f5c18c110f5747174a2412c47f5d63.png)

PhotoShop预处理

![](img/38187d25e3cad38ceade8cd22d4d2a32.png)

AI细化

![](img/edf4bbc3d2466e83f85a9e5e1b2e6b4e.png)

## 合成细化

最后就是将我们上面得到的元素进行合成处理，统一氛围色调，再加入一些想要的点缀结构即可。下图是最终的视觉稿。

![](img/8d862ddce2ba411c4ba578b42715b941.png)

视觉稿

在确定视觉稿之后就可以在UE4进行场景的搭建和氛围的调整啦，附上最终UE4效果图（UE4效果图设计非本文重点，这里不做阐述），见下图。

![](img/0f5fc1cf08658fc01e54f18c7fb7fb81.png)

UE4效果

以上就是整个虚拟演播厅设计方案流程分享，欢迎大家留言讨论。

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>
