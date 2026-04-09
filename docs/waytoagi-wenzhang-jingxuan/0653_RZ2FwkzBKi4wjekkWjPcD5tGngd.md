# 《万物皆可meme》-扣子复刻wojak

> 来源：[https://waytoagi.feishu.cn/docx/RXrVdchCLoaEg8xruw3cO23fn0c](https://waytoagi.feishu.cn/docx/RXrVdchCLoaEg8xruw3cO23fn0c)

# 简介

万物皆可meme，基于扣子能力复刻版，非套壳，欢迎体验：https://www.coze.cn/store/bot/7385188509868605491

> 因为使用了非豆包官方模型，豆包APP内无法分享，可以通过搜索名称直达：

![](img/a7e13f3030c596ea869daaafdff1112b.png)

# 梗图集

| ![](img/acf96b73b05da630797ef9d74aea11dd.png) | ![](img/3b2f7f371df54fad770842558d5a2233.png) | ![](img/b5afdf2d63d336ace602bae3186aa701.png) |
| --- | --- | --- |
| ![](img/73635c57c10fb5370caea62af1b9b321.png) | ![](img/3ba27af7e568f6c4ddbe0eb33e297bce.png) | ![](img/40e23ea939ced7a1db1ebad76b3dafb8.png) |
| ![](img/85edc55f2d5627d1dfaddf354d783e01.png) | ![](img/3ccfcd5a31d95740c8a92ffe8f7f4202.png) | ![](img/fb99d086b66791aeb7336705a033af7f.png) |
| ![](img/c5a8e3b4d3fe2297097670df690c2a7a.png) | ![](img/723e8b59b1a6b4b4145324d554ba16dd.png) | ![](img/68829ed27f92a6e0ad790c027b42baa8.png) |
|  |  |  |

# 技术原理

![](img/c3c72986eb793f6eb2e3b7ef4c77779c.png)

从原作者推文可以看到，wojak meme generator基于wojak lora借助Co**mfyUI实现了图像流。为**了致敬&扣子复刻原作，最重要的是需要实现三个核心功能：梗文、梗图、meme风格化。但扣子目前的能力为了极大简化Agent构建过程，所以定制化并没有那么强，特别是图像流还比较简单，所以在实现上采取了一些取巧的做法。

V1.0版本，只复刻了梗文，梗图，并没有实现meme风格化，大致效果如下：

![](img/5f6308dbc07f70109a2a25a5e79a952a.png)

文字和图片的排版也使用扣子自带的UI卡片，后来有道友指点扣子图像流已经支持了meme风格化节点，所以连夜又爆肝做了升级。所以你看到了上面的V2.0版本。

![](img/c3641044880faf3a22236124a302a481.png)

Bot整体流程可以从上面进度播报大致了解，主要包括：

+   **梗文创作**：借助Bing搜索（两个并行调用，各50条）+ Moonshot模型

![](img/a15bc586c037c3bceaf4f3c3ed692284.png)

+   **梗图创作**：基于Bing搜索结果 + Moonshot模型生成提示词 + ByteArtist文生图插件

+   **meme风格化**：基于图像流-风格滤镜节点，对梗图做了二创

![](img/76c88b7cab19926ca37b839b60b63ebc.png)

+   **出图**：依然使用很好用的fastposter插件排版，复刻wojak meme风格

![](img/1cbbd1c870d177b50649c78d91e7f69c.png)

> [fa](https://www.coze.cn/store/plugin/7366608383249743922?from=plugin_space)stposter插件已开放商家扣子市场，点击链接去看看。

> [**看到**](https://www.coze.cn/store/bot/7385188509868605491)这里如果你觉得还不错，请为扣子Bot（链接）打call，收藏+评论，多玩多分享。

## 梗文不够扎心怎么办

+   换模型，实测moonshot最好，豆包完全不work

+   加大Bing搜索结果剂量

+   指示降低不同梗点的相似度

+   提供高质量few shot

# 我也想玩

> 这里是一个共享表格，随便添加吧

[【任意编辑】一起玩梗吧](https://hqexj12b0g.feishu.cn/wiki/Yviawxz1ei5qsRkf4T1crydxnhe?from=from_copylink)

> 这里还有道友们有趣的Bot大集合，看看有没有你中意的

【腾讯文档】一起捏扣子Bot：https://docs.qq.com/sheet/DZndTcWlSdW1sWlVl

# 问题反馈

![](img/91c440bd1664cecde38226404044a8ce.png)