# Shorten（精简提示语）

> 来源：[https://my.feishu.cn/docx/CNhnduHwPoCrbCxG4asc2Uphnwc](https://my.feishu.cn/docx/CNhnduHwPoCrbCxG4asc2Uphnwc)

该/shorten命令会分析您的提示，突出显示提示中一些最有影响力的单词，并建议您可以删除的不必要单词。使用此命令，您可以通过关注重要术语来优化提示。

**/shorten**[与多提示](https://docs.midjourney.com/multi-prompts)或[参数](https://docs.midjourney.com/no)不兼容[**--no**](https://docs.midjourney.com/no)

---

## 分析提示**/shorten**

Midjourney Bot 会将您的提示分解为更小的单元（称为标记）来分析。这些标记可以是短语、单词，甚至是音节。Midjourney Bot 会将这些标记转换为它可以理解的格式。它会将这些标记与训练期间学到的关联和模式结合使用，以指导如何生成您的图像。可以将标记视为帮助 Midjourney Bot 理解输入并创建所需视觉输出的构建块。

长提示包含不必要的单词、冗长的描述、诗意的短语或直接称呼机器人（“请给我制作一张图片”、“谢谢你的帮助，Midjourney Bot！”）可能会导致意外元素添加到您的图像中。

该**/shorten**命令可以帮助您发现提示中最重要的单词以及可以省略的单词。

### /shorten 命令示例

**如果您想要制**作一堆撒满糖屑的甜甜圈，您可以尝试以下提示：

Please create a whimsical majestic tower of donuts, intricately crafted and adorned with a mesmerizing array of colorful sprinkles. Bring this sugary masterpiece to life, ensuring every detail is rendered in stunning magical realism. Thank you!

![](img/57e512d74350b3f53f4eadc2b06cca20.png)

如果您使用**/shorten**带有上述提示的命令，Midjourney Bot 将返回以下信息：

**重要标记**

请**用甜甜**圈制作一座奇异而雄伟**的**塔，精心制作，并装饰着令人着迷**的彩色糖**粒。让这**个甜蜜**的杰作栩栩如生，确保每个细节都以令人惊叹**的魔幻现实主**义呈现。谢谢！

**缩短提示**

1️⃣   雄伟的甜甜圈塔，精心制作，五彩缤纷的糖粒，甜蜜的杰作，渲染，魔幻现实主义

2️⃣   雄伟的甜甜圈塔，五颜六色的糖粒，甜蜜的杰作，渲染，魔幻现实主义

3️⃣   雄伟的甜甜圈塔，五颜六色的糖粒，甜蜜蜜，魔幻现实主义

4️⃣   雄伟的甜甜圈塔，五颜六色的糖粒，神奇的

5️⃣   甜甜圈塔，糖屑

提示中最重要的标记以**粗体突出**显示，最不重要的标记以划线突出显示。根据此信息，您还将获得 5 个可能的较短提示。

![](img/ce7d473bfbe662354793791cfedd4b15.png)

![](img/0ad9f70c1017b3e8fa7563e417a4417e.png)

### 分析结果

最短的提示**Option 5️⃣ : tower **of donuts, sprinkles产生了最接近原始目标的图像。许多填充词，如“异想天开”、“令人着迷”和“杰作”都可以省略。了解到“塔”和“神奇”被认为是重要的标记，有助于解释为什么有些图像是用童话城堡元素生成的。了解这一点提供了一个线索，如果目标是制作一堆美味的甜甜圈，那么应该从提示中删除“神奇”。

该**/shorten**命令是一个工具，可帮助您探索 Midjourney Bot 如何解释标记并尝试使用单词，但它可能不适用于所有主题和提示样式。

---

## 如何使用缩短

使用/shorten <your prompt>任意Bot 频道中的命令获取提示信息

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>