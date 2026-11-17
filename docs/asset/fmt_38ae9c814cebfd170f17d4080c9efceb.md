
# 1、手把手教你用 Stable Diffusion 写好提示词

> 来源：[https://keenchase.feishu.cn/docx/BbLGdGIkTo0ycExYNKacJMCint6](https://keenchase.feishu.cn/docx/BbLGdGIkTo0ycExYNKacJMCint6)

## 导语

Stable Diffusion 技术把 AI 图像生成提高到了一个全新高度，文生图 Text to image 生成质量很大程度上取决于你的提示词 Prompt 好不好。本文从“如何写好提示词”出发，从提示词构成、调整规则和 chatGPT 辅助工具等角度，对文生图的提示词输入进行归纳总结。

## 一 背景介绍

St[ab](https://km.woa.com/articles/show/574286)le Diffusion 是一种文生图 AI 模型，由互联网上数百万图像和文本描述对训练而来，通过理解文本描述与图像信息的内在关联，不断利用扩散过程进而得到满意的生成图片[1]。

比如，通过一串提示词，midjourney 会输出这样的情侣合照：

```
A **pa**ir of young Chinese lovers, wearing jackets and jeans, sitting on the roof, the background is Beijing in the 1990s, and the opposite building can be seen —v 5 —s 250 —q 2.
一对年轻的中国情侣，穿着夹克和牛仔裤，坐在屋顶上，背景是20世纪90年代的北京，可以看到对面的建筑
```

![](img/6983c5cdb5adaef03b16b401bf63b3a6.png)

图1 用midjourney v5生成的情侣图

没错，这两位并不是真人，而是由 AI 一键生成的，毫无违和感！你可能会想这有什么了不起的，随手一搜就有好几 G 嘛。这项技术的有趣之处在于，还可以让模型生成一些先前不存在具有组合元素的高质量图像。比如说，你可以生成不同画家风格的明星照片，下面以 19 世纪印象派画家 Vincent van Gogh 和 19 世纪美国画家画家 John Sargent 风格来画下 Emma Watson 肖像画（Emma Watson 是《哈利波特》中赫敏的饰演者）

![](img/845070d4cb0294a449c34c630c4ed50f.png)

![](img/b61a6c6c1c4a6738d146e11831110403.png)

图2 不同画家风格下的Emma Watson肖像

`prompt1`: Vincent van Gogh’s painting of Emma Watson; `prompt2`: John Sargent’s painting of Emma Watson

从图 2 可以看到，生成图片对面部和阴影控制得比较好，整体艺术风格还是比较协调的，能把画师风格和具体人物以一种比较和谐的方式融合到一起。

![](img/e216bdc4b6ec64132dd693388b1eac0e.png)

图3 Stable Diffusion组成结构

那计算机是如何理解输入文字呢？图 3 给出了 stable diffusion 整体结构示意图，利用文本编码器 `text encoder`（蓝色模块），把文字转换成计算机能理解的某种数学表示，它的输入是文字串，输出是一系列具有输入文字信息的语义向量。有了这个语义向量，就可以作为后续图片生成器 `image generator`（粉黄组合框）的一个控制输入。stable diffusion 更多技术介绍和文本编码器工作原理可以查看《【AI 绘画】十分钟读懂 Stable Diffusion 运行原理》。要想生成出满意照片，输入合适提示词就变得非常重要，接下来就从“如何写好提示词”出发，对文生图的提示词输入方法进行归纳总结。

## 二 如何写好提示词？

写出一份比较好的提示词是文生图技术的关键。但是，写出一份好的 prompt 并不容易，下面针对“如何写好提示词”这个问题，从提示词构成、调整规则和 chatGPT 辅助等角度，来介绍下如何优化输入提示词。

### 1 正面提示词

要写好一份提示词，遵循原则为尽可能详细并且具体，从不同角度进行详细描述。下面从 9 个角度来介绍输入关键词。

常用的关键词类别包括如下：

(1) 主体 subject

(2) 媒介 medium

(3) 风格 style

(4) 画家 artist

(5) website

(6) 分辨率 resolution

(7) 额外细节 additional details

(8) 色调 color

(9) 光影 lighting

当然，输入提示词时，不需要包括到每个类别，只需作为一个列表检查下哪些可以用到。下面通过添加每个类别的关键字来生成一些图像，来说明和介绍下每个类别。为单独观察提示词效果，实验时不会使用负面提示词（在下个模块会更详细介绍）。

#### (1) 主体

主体 subject，是指想在图像中看到的主体，要尽可能详细描述以避免出现描述不足的问题。假如要生成一个在施法的女巫，新手可能会这么写：

```
A sorceress
```

这个描述词也太简单了吧，要提到女巫长什么样，增加她形象相关的描述词，比如说她穿了什么？在施什么魔法？是站着，跑着，或者飘浮在天上？主体背景在哪里？

Stable diffusion 并不能猜到这些内容，我们要更详细地表达各个元素信息。画人物对象的一个常用技巧是使用名人的名字，名人由于在训练集中出现频次较大而训练充分，是控制生成人物形象的一种好办法。

作为演示，把女巫形象设置成 Emma Watson 的样子，她因饰演《哈利波特》中的赫敏一角红遍全球，也是 stable diffusion 里最常使用的一个关键词。把她想象成一个会使用闪电的神秘女巫，并增加一些形象要求，提示词如下：

```
Emma Watson as a powerful mysterious sorceress, casting lightning magic, detailed clothing
```

对应的生成图如下：

![](img/547e89825ca187d6c58ba963a143e50a.png)

图4 Emma Watson女巫图

利用名人名字来控制人物形象，直接原因就是训练时用到了一定量样本，包括不同角度和不同场景，使 Emma Watson 得到了充分训练。要是使用更早些或者小众点的演员，效果就不一定这么好了。

#### (2) 画风

画风 medium，是指生成图片的画风，包括插画 illustration、油画 oil painting 或摄影风 photography 等。这类描述词影响力很大，单独一个画风描述词就能很大程度地改变风格。比如添加下关键字“digital painting(数字绘画)”，

```
Emma Watson as a powerful mysterious sorceress, casting lightning magic, detailed clothing, digital painting
```

对应的生成图如下：

图5 Emma Watson女巫图+画风

可以看到，这些图像的画风从普通照片变成了数字绘画风。这里也列出一些常用画风，

#### (3) 风格

风格 style，是指主体形象的艺术风格，比如印象派、超现实主义、波普艺术等。

通过添加一些风格描述词 `hyperrealistic, fantasy, surrealist, full body`，

```
Emma Watson as a powerful mysterious sorceress, casting lightning magic, detailed clothing, digital painting, hyperrealistic, fantasy, Surrealist, full body
```

对应的生成图如下：

图6 Emma Watson女巫图+风格

这里感觉又多了一些变化，前面关键字已经包含了部分风格关键字导致变化不大，但是保留这些风格提示词也可以。这里也列出一些常用风格。

#### (4) 画家

画家 artist，类似于强修饰符，是指用特定画家作为参考来生成他们风格的图像。当然也可以使用多个画家名字来生成混合风格。

继续在提示词里再加上 19 世纪的超级英雄漫画家 Stanley Artgerm Lau 和肖像画家 Alphonse Mucha

```
Emma Watson as a powerful mysterious sorceress, casting lightning magic, detailed clothing, digital painting, hyperrealistic, fantasy, Surrealist, full body, by Stanley Artgerm Lau and Alphonse Mucha
```

对应的生成图如下：

图7 Emma Watson女巫图+不同画家

这里看到，把两位画家的风格融合到一起，效果还不错。这里也列出一些常用画家，

#### (5) 网站

有一些小众图片网站，比如说 [Artstation](https://www.artstation.com/) 和 Deviant Art 这样的网站收集了许多画风独特的图像。在提示词中添加网站名称，也可以把图像引向这些风格。

试下在提示词里再加上“artstation ”，

```
Emma Watson as a powerful mysterious sorceress, casting lightning magic, detailed clothing, digital painting, hyperrealistic, fantasy, Surrealist, full body, by Stanley Artgerm Lau and Alphonse Mucha, artstation
```

对应的生成图如下：

图7 Emma Watson女巫图+网站

图片变化不是很大，但看起来有点像 Artstation 网络下载下来的了。

#### (6) 分辨率

分辨率 Resolution，表示生成图像的清晰度和细节程度。继续添加这方面关键字“highly detailed, sharp focus”，

```
Emma Watson as a powerful mysterious sorceress, casting lightning magic, detailed clothing, digital painting, hyperrealistic, fantasy, Surrealist, full body, by Stanley Artgerm Lau and Alphonse Mucha, artstation, highly detailed, sharp focus
```

对应的生成图如下：

![](img/d9dc683cdaf10b902dc9141e55f254b2.png)

图8 Emma Watson女巫图+分辨率

看起来变化不大，因为前面生成图片已经非常清晰了，但补充一下也无妨。

#### (7) 额外细节

额外细节 additional details，可以继续用来修改图片。继续添加这方面关键字“sci-fi, stunningly beautiful, dystopian”，

```
Emma Watson as a powerful mysterious sorceress, casting lightning magic, detailed clothing, digital painting, hyperrealistic, fantasy, Surrealist, full body, by Stanley Artgerm Lau and Alphonse Mucha, artstation, highly detailed, sharp focus, sci-fi, stunningly beautiful, dystopian
```

对应的生成图如下：

图9 Emma Watson女巫图+额外细节

#### (8) 色调

色调 color，是指通过添加颜色关键字来控制图像整体颜色，可以把颜色应用到某个物品上或者是整体色调。

利用关键字 `iridescent gold` 让整张图片黄一点，

```
Emma Watson as a powerful mysterious sorceress, casting lightning magic, detailed clothing, digital painting, hyperrealistic, fantasy, Surrealist, full body, by Stanley Artgerm Lau and Alphonse Mucha, artstation, highly detailed, sharp focus, sci-fi, stunningly beautiful, dystopian, iridescent gold
```

对应的生成图如下，看起来就很黄了！

![](img/52f32ed37b55aa64f10a5e768dae5dfa.png)

图10 Emma Watson女巫图+色调

#### (9) 光照

光照 lighting，是指图像里的光照描述，改变光照可以对图像效果产生巨大影响。试试添加关键字“cinematic lighting, dark”，

```
Emma Watson as a powerful mysterious sorceress, casting lightning magic, detailed clothing, digital painting, hyperrealistic, fantasy, Surrealist, full body, by Stanley Artgerm Lau and Alphonse Mucha, artstation, highly detailed, sharp focus, sci-fi, stunningly beautiful, dystopian, iridescent gold, cinematic lighting, dark
```

对应的生成图如下，影视光照就出来了！

![](img/8e1774d1ef4c7a96795ba29099b6ea12.png)

图11 Emma Watson女巫图+光照

总结下，图片生成时针对主体添加一些关键字，就能够得到比较不错的图片；通常不需要填写很多提示词来获得高质量图片，比如画家、网站和风格在一定程度上是有些重合的。也可以借助一些 stable diffusion 提示词网站来获取更多灵感 Ai 画廊 - AI 关键词生成器

### 2 负面提示词

负面提示词和正面提示词是同等重要的，使用负面提示词也是生成迭代过程的一个重要环节。要解释负面提示词的工作原理，这里需要先理解不使用负面提示词时采样是如何工作的

#### 不使用负面提示词的采样过程

在 stable diffusion 采样阶段，① 首先用文本提示词作为指导条件，利用条件采样对图像进行去噪；② 采样器使用无条件采样对同一图像进行去噪，这里不使用文本指导，但它仍然会扩散到某一个图像，比如说下面的篮球或者红酒杯（它可以是任何随机主体）；③ 扩散过程中实际上是计算条件采样和无条件采样的差异，并按照采样步数重复这个过程。

图12 不使用负面提示词的采样过程

#### 使用负面提示词的采样过程

负面提示词是通过影响无条件采样实现的。利用负面提示词，在上面第 ② 步里就不是使用空提示来生成随机对象了。从技术上说，正面提示词会引导采样过程生成与文本相关的图像，负面提示词则会在采样过程来引导远离相关图像。需要说明，扩散过程是发生在 `latent space` 里，而不是图像空间，这里仅作为技术示意。

![](img/7fa8215938db542195ab08f7ac3f1049.png)

图13 使用负面提示词的采样过程

负面提示词，即填入不想要的主体或者身体部位，以在采样过程中避免出现。比如说，sd v1 版本不擅长生成手，就可以在负向提示词里输入 `hand` 或者 `extra limbs` 来隐藏或修正。下面会从 4 个角度来介绍可以使用负面提示词的应用场景，分别是：

(1) 移除物体 removing things

(2) 修改图片 modifying images

(3) 关键词切换 keyword switching

(4) 修改风格 modifying styles

#### (1) 移除物体

负面提示词的第一个直接用法是移除任何不想在图片中看到的内容。比如，在提示词输入“Portrait photo of a man”，得到下面两张图片，都看起来比较严肃。

![](img/b707bb25985e9a9a6f0fb27bd938d21f.png)

图14 提示词输入“Portrait photo of a man”的生成图片

接下来试着去掉他们胡子，看起来更年轻一点，因此在负向提示词里输入 `mustache`，就可以生成一些没有胡子的男人，如图 15 所示。当然，要想完全去除右边男人的胡子，可通过增强负向提示词 `(mustache:1.3)` 来告诉采样过程去除胡子操作的重要度提高 30%。

图15 在负向提示词里添加 `mustache` 的生成图片

#### (2) 修改图片

负向提示词也可以用于在得到较满意图像时，利用负面提示词进行微调。这里不需要移除任何东西，而只需要对主体做一些细微修改。

比如，利用下面提示词得到一张比较满意的唯美照片后，发现可能由于刮风导致头发都飘起来了，就可以添加负面提示词 `windy` 让头发正常下垂。

正面提示词输入如下，
