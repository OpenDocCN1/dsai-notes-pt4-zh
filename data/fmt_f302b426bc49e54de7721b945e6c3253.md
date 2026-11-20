
# MJ V6六个书写提示词的技巧

> 来源：[https://aigcreative.feishu.cn/docx/COg1dlNmloOf6lx47jocsNZjn0f](https://aigcreative.feishu.cn/docx/COg1dlNmloOf6lx47jocsNZjn0f)

V6更新后，生成的图片质量更高了，细节更加丰富，不知道大家有没有发现，Midjourney V6对于提示词的识别逻辑和之前完全不同了，他可以非常精准的识别提示词的每一个词语的描述。现在我们只需要输入画面中想要出现的主题元素就可以。所以我们使用V6时需要改掉之前书写提示词的习惯，今天就给大家分享6个书写提示词的技巧。

## 01.将文本括在引号中

![](img/f6966247bc862847f8f32dc75b68a94c.png)

> 关键词：Isometric number shape of “Welcome 2024” in neon light tube

> 翻译：霓虹灯管中“Welcome 2024”的等距数字形状

将希望出现在图片中的任何文本内容放入引号内。告知Midjourney需要将该特定文字呈现于图像中。

## 02.使用原始参数使文本更加突出

![](img/bd6840b36abdd47ab4664e4c4998a31f.png)

在 MidJourney V6 生成图片时，使用该 `--style raw` 参数可以提高提示词的准确性。使用这个这个参后数MidJourney 将提示词的优先度置于默认风格上。但是，带有这个参数的图像通常不太具有艺术性风格。

## 03.使用较低的风格化值 `--stylize`

> 关键词：A fun shoe advertisement, with the words “JUST DO IT”, featuring brightly colored sneakers untied with bouncing laces, with confetti in the background`--style raw` `--v 6` `--stylize 500/25`

> 翻译：一个有趣的鞋子广告，上面写着“JUST DO IT”，色彩鲜艳的运动鞋解开弹跳鞋带，背是五彩纸屑，`--style raw` `--v 6` `--stylize 500/25`

如果把风格值设置的过高，可能会出现文字无法被成功的识别出来，左边是`--stylize 500`，右边是`--stylize 25`，可以看见左图（just doit）这个单词就是比较完整的，但是风格值越高生成的画面效果也是越好的，明显可以看出右边的图比左边的高级。

## 04.描述字体样式

![](img/9a350e9db1793f582f11c261c511c976.png)

> 关键词：Text “Be Kind” printed in white font on the back of a denim jacket worn by a man walking down a city street

> 翻译：文字“Be Kind”以白色字体印在牛仔夹克的背面，由一名男子走在城市街道上

左图：用白色字体打印的文字“Be Kind”。右图：以白色草书字体打印的文字“Be Kind”

## 05.`vary`（strong）变化

![](img/9807348569a2dfea1e1124cf1e894759.png)

> 关键词：A realistic standup pouch product photo mockup decorated with honey, oats and chocolate with the word "CHOCO GRANOLA `--v 6.0`

> 翻译：一个真实的站立袋产品照片模型装饰与蜂蜜，燕麦和巧克力与单词“巧克力格兰诺拉

就比如左边，我生成了一个食品包装，我不想要这两个带子，就可以点击`vary`让图片重新生成一次。

## 06.放大图片像素

![](img/8d82023b896e03007e1d6f6811add8c8.png)

> 关键词：A beautiful Asian girl, real details, delicate skin, white background, photography, frontal photos, ID photos

> 翻译：一个美丽的亚洲女孩，真实的细节，娇嫩的皮肤，白色背景，摄影，正面照片，证件照

如果生成的图片不满意想要微调，我们可以使用`upscale（creative）`来放大图片并且对图片进行微调

## 最后

随着midjourney V6模型的更新，以前很多我们做不到，实现不了的效果现在都可以实现了，相信随着AI版本不断的迭代，我们使用起来也会越来越便捷。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
