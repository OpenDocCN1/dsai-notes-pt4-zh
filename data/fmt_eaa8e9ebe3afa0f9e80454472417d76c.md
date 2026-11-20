
# 场景8：人物

> 来源：[https://my.feishu.cn/docx/JXLwdybrdoiVVExqixScKob4nNd](https://my.feishu.cn/docx/JXLwdybrdoiVVExqixScKob4nNd)

## 名人照片

这算是 V5 的新特性，Midjourney 在 V5 版本认识很多名人，你可以在描述主体的时候，加上名人的名字，就可以生成该名人的照片，我们生成几个电影里的经典照片：

![](img/58afca537b856b6bb6ab976f4eb3eeb8.png)

第一个是 Keanu Reeves，第二个是 Vito Corleone（我并没有输入主演人 Marlon Brando），两组图的 prompt 都非常简单，你可以尝试自己写一下😁

本章最后有答案。

## 衣服模特

做衣服类电商的朋友可能会比较需要这个功能：

1. 用 AI 生成一个虚拟人
2. 然后将货物照片（比如某件衣服）传给 AI
3. AI 将虚拟人和货物的照片融合在一起

坦率说来，我觉得 Midjourney 应该是可以的，但目前的效果还不太好，要想实现这个需求，只需要用到前面介绍的技巧九里提到的 blend 功能，详细方法就不在这赘述了，大致就是我将商品图和人物图用 blend 功能融合了下：

![](img/ad89d2a0e67c94bdaf7170dfc5a93c57.png)

为了验证一致性，我生成了几张 Reeves 的图分别尝试了下，最后输出的结果是这样的：

![](img/93ed90be5ca1dcde1fb848a1253c1ff3.png)

整体来看，好像还行，但真的用在商品描述图里，我觉得还不太可能，因为里面的衣服跟原图差距还比较大（可以细看里面的纽扣）。

但我觉得未来 Midjourney 可能会优化好这个功能，并且我觉得如果可以自己训练模型，这个功能实现起来应该不难。

## 老照片

这个算是名人照片的分支玩法，最近大家应该看到过很多类似的照片，其实 prompt 很简单，主体和背景大家用翻译软件搞搞就行：

用类似的 prompt 生成了两组图片：

![](img/bd165b805f27920ed4a2fb39dbabbb5e.png)

## 技巧十四：增加风格——年份

上面的老照片场景，我们用了 1990s 风格，严格地说，这个 1990s 更适合放在主体，或者环境里，放在风格里也不是不行，因为不仅仅希望主体是 1990s 还希望图片生成的风格也是 1990s。

这就是最后一种风格词用法：年代。

拿插画那一章的两个 prompt 修改一下，我都没在 prompt 里加艺术家的名字，而是改为 style of 1920s，玫瑰的图有点那味，猫的图右上角那张太逗了。

![](img/b98ffde4f1774d72eaeb4ba02ca04323.png)

## 名人照片答案

Reeves 很简单，就是说明主体，然后注明穿着标志性的衣服，走在大雨里：

```
Keanu Reeves, wearing a black long leather coat, walking down the street in the rain
```

然后教父的那张就更简单了：

```
Vito Corleone sitting on a chair
```

## 技巧十五：如何让 Midjourney 生成的人更具有多样性？

这里的多样性其实是指人的高矮胖瘦，甚至身体的一些特殊性等。

不知道各位有没有发现，大多数用 AI 生成的图片，人物都非常漂亮和英俊，但这并不好，这其实某种程度上代表了社会的偏见。

所以我想在本章重点介绍下，如何让 Midjourney 生成更多样的人体。

**方法一：Slider 方法**

在上一章，我介绍了增加权重的方法，这个方法还可以帮助你生成不同体型的人物，以下是 Midjourney 官方社区的案例：

```
Plus-sized ginger female dressed in bohemian vest, maxi skirt, espadrilles, walking across autumn campus courtyard, morning atmosphere:: plus-sized::-0.3 --ar 2:3
```

你可以控制 plus-sized 后面的参数，从而使画面中的人物变胖或者变瘦。这里需要注意，并不是负数就是越瘦的意思，而是削弱了 plus-sized 权重，如果你的 prompt 词是 thin 你加个负数 slider 参数那就是变胖了：

![](img/35c1fb3fa462ce2c6730479f323f2938.png)

**方法二：直接描述**

Midjourney 是知道一些多样性的词语的，比如：

+ blind：失明
+ deaf：聋
+ cerebral palsy：小儿麻痹
+ accessibility：行动不便

![](img/78e60cea80f54948960fe33492c948a1.png)
