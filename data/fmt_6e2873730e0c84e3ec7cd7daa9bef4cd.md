
# AI 绘画提示词进阶用法

![](img/4b4985595210951f4b75bf00a3f1fa67.png)

每75个字符会被强制分为一组，组内元素的顺序不会影响权重差异。但正如我们之前提过的，提示词的顺序非常重要，顺序也会影响图像的生成效果。经过验证，在75个`Token`内，第一个`Token`的效果最强，接近第75个`Token`时效果最弱。如果描述超过75个字符，同一组提示词可能被分为两组，导致生成效果不理想。而我们常规的提示词往往超过75，例如下图，可见有111个字符，就会被自动分成2组

![](img/6a09b729ae7b62bc7553da66a2f1e3a1.png)

那么，如果我们的图像内容有分前中后景，很多需要表达的内容，我们又应该怎么描述才能更加精确呢？

我来介绍下打断语法（`Break`）的用法，`BREAK`是一个特殊提示，插入后它会强制将 `Token` 数 凑齐调整为75。这样我们就能够控制不同层次提示词的打断效果了。这个语法指令相当于手动分组，确保每组都能描述完整的画面特征。使用`BREAK`可以切断不同组之间的上下文关联，防止元素相互干扰。

![](img/a5f61af89835ccda89d78ce7674a0d61.png)

但是需要注意的是，一段描述中尽量不要超过3个`BREAK`，过多的分组会导致模型学习混乱，得不到想要的结果。

而关于`Break`在实际使用中的影响效果，可以详见

@董步云

大佬的这一篇文章，我就不再赘述

[https://zhuanlan.zhihu.com/p/622758456](https://zhuanlan.zhihu.com/p/622758456)

## 六、画质提示词用法

![](img/1206fdc71e74c73136da3d8074016388.png)

关于画质提示词，是提升画面表现力的重要工具，但是提示词不是越多越好，过多的提示词反而会导致程序忽略重要信息，因此我们需要学会选择如何使用合适的画质提示词。

在这一块，已经有

@董步云

大佬的文章写了很详细的分析文章，我就直接引用：

> 以下是一些推荐给新手的提高质量的 `Prompt` 和 `Negative prompt`！
>
> `Prompt`
>
> `best quality, masterpiece, ultra high res, (photorealistic:1.4), RAW photo`
>
> 喜欢的话可以放 `8k`
>
> `Negative prompt`
>
> `illustration, 3d, sepia, painting, cartoons, sketch, (worst quality:2), ((monochrome)), ((grayscale:1.2)), (backlight:1.2), analog, analogphoto`
>
> 以下的提示也推荐用来防止手臂或腿部的破綻，试着将它们与之前的提示组合使用吧！
>
> `bad anatomy, bad hands, double navel, collapsed eyeshadow, multiple eyebrows, freckles, signature, logo, 2faces`

大佬的原文分析在这里，希望进阶掌握的小伙伴可以点击下文查看

[https://zhuanlan.zhihu.com/p/621618198](https://zhuanlan.zhihu.com/p/621618198)

## 七、借助语言大模型进行主体内容优化

![](img/c4312b248446a0c0a8aa75d5d5a51192.png)

除了自己构思，如果碰到日常配图，想要偷懒快速生成图片，我们也可以借助`ChatGPT`类的语言大模型来进行提示词生成。

[接下来分享](https://www.zhihu.com/search?q=%E6%8F%90%E7%A4%BA%E8%AF%8D%E8%AF%AD&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3144267909%7D)一个我自己摸索出来的非常实用的 `prompt` 语句，这个提示词是我自己在别人分享的`prompt`基础上，进行调整调试而做的二次创新，效果还可以，记得最后还得自己加上画质提示词进行优化哦。

### PROMPT:

```
您的任务：根据 [毛绒绒的熊猫] 这个描述，最终输出一段30个单词的结果。要求：作为生成人工智能的提示生成器，您将为AI创建图像提示以可视化。我会给你一个概念，你会提供一个详细的提示，用于生成人工智能的图像。请遵守以下结构和格式，并根据所有提示生成一个完整的提示语：不要使用换行符，每行写一个提示。结构： `[1]` = `masterpiece`，`best quality`，`ultra high res`, `[2]` = `[PROMPT]` `[3]` = `[2]`的详细描述，包括具体的图像细节。 `[4]` =场景环境的详细描述。 `[5]` =场景气氛、感受和氛围的详细描述。 `[6]` = 专业的摄影镜头及构图语言的描述 格式：按照以下提示结构进行： `prompt`：`[1]`，`[2]`，`[3]`，`[4]`，`[5]`，`[6]`；最后提取每一行描述里面的关键信息，整合成一段 `Prompt`，请注意，只需要输出最终 `Prompt` 即可，所有输出为英文。
```

我采用 `文心一言` 做示意（不同大模型效果不一，可以多试一下），将结果分享给各位。

![](img/7dbf88d4e08cae10db0495a8934fe370.png)

![](img/e80b5501343358f04d2915ed0688d98b.png)

## 八、提示词模板参考网站

`https://link.zhihu.com/?target=https%3A//lexica.art/`

`https://link.zhihu.com/?target=https%3A//wolfchen.top/tag/`

`https://link.zhihu.com/?target=https%3A//moonvy.com/apps/ops/`

`https://link.zhihu.com/?target=https%3A//prompthero.com/`

[此外，还](https://www.zhihu.com/people/0f44b855a7109f09831bae3f0a39fc8b)可以参考一些优质博主的文章获取更多的`Prompt`灵感，比如 @董步云大佬的众多文章，推荐这一篇：`https://zhuanlan.zhihu.com/p/621255291`

[https://zhuanlan.zhihu.com/p/621255291](https://zhuanlan.zhihu.com/p/621255291)

下一篇文章我们将学习重要插件`Controlnet`的使用和图生图功能的应用，想要了解学习AI绘画，请继续关注我的作品吧。

---

我是德里克文，一个对AI绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！
