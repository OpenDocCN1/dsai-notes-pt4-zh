
```
emma watson as nature magic celestial, top down pose, long hair, soft pink and white transparent cloth, space, D&amp;D, shiny background, intricate, elegant, highly detailed, digital painting, artstation, concept art, smooth, sharp focus, illustration, artgerm, bouguereau
```

图16 添加负面提示词`windy`后的左右变化，右边为修改后图片

要是不想调整头发，而是想让头发遮住耳朵呢？通过添加带有不同强度的负面提示词`ear`来调整隐藏强度。下面是强度分别为 1.3、1.6 和 1.9 的效果，由于 `ear` 在强度为 1 时已被大比例遮住，继续增大时变化不多。但负面强度设置为 1.9 时，图像组成也发生了变化，这说明负面提示词对扩散过程的影响是比较大的。

![](img/d126b5bd4e48f27d2e703b7a0ee70272.png)

图17 负面提示词`ear`不同强度的生成图片

### (3) 关键词切换

要是真的想使用强度为 1.9 的负向提示词，有什么不改变图片总体结构的方法呢？用关键词切换技术，先使用一个无意义的单词作为负面提示词，再在后面的采样步骤里切换到`(ear:1.9)`。

比如，使用“the”作为无意义的负面提示词，可以把它放在负面提示词里先验证下它的无用性。在验证添加`the`的生成图像跟没有添加时几乎一样后，就可以使用这个`the`单词作为负面提示词，

```
the: (ear:1.9): 0.5
```

由于采样步数设置了 20，意味着在第 1~10 步时使用负面提示词`the`，第 11~20 步使用负面提示词`(ear:1.9)`。这是因为，扩散过程的初始阶段是确定图片主体结构，后面步骤只是在对细节进行更精细调整，比如用头发遮住耳朵。

图17 负面提示词`the: (ear:1.9): 0.5`的生成图片

通过这种方式，就对负面提示词使用更大强度 1.9，且不改变图片组成，得到了接近原始图像的微调图像，并且把耳朵用头发遮得更严实。

### (4) 修改风格

负面提示词不仅能用来修改图片内容，也可以用来修改图片风格。为什么要用负面提示词来修改图片风格？在正面提示里添加过多单词也会混淆扩散过程，把一部分信息放在负面提示词里的效果会更明显。

操作 1 Sharpening。为了让图片更清晰，除了在正面提示词里使用关键字“sharp”或者“focused”，也可以在负面提示词里使用`blur`，来让图片更为清晰。

![](img/25d30ff682426a69bd8c76b4ca6fb08f.png)

图18 添加负面提示词`blurry`的生成图片

操作 2 “Photorealistic”。使用负面提示词`painting`或者`cartoon`，使生成图片更倾向写实主义。当然，要是想保持原有图片结构，可继续使用前面提到的关键词切换，使用负面提示词`the: (painting cartoon:1.9): 0.3`，可得到图 19 中更接近原始图像但增加了写实主义风格的生成图像。

![](img/710c817a642b6b107e6d51dd43e9dbe9.png)

图19 负面提示词`the: (painting cartoon:1.9): 0.3`的生成图片

下面给出了一份通用的负面提示词，可以复用到你的场景，这些负面提示词可以让图片主体更突出，具有层次感。

```
ugly, tiling, poorly drawn hands, poorly drawn feet, poorly drawn face, out of frame, extra limbs, disfigured, deformed, body out of frame, bad anatomy, watermark, signature, cut off, low contrast, underexposed, overexposed, bad art, beginner, amateur, distorted face, blurry, draft, grainy
```

总结下，负面提示词也扮演着十分重要的作用，用负面提示词在采样过程引导远离相关图像。如上面介绍，可以用来微调生成图片，如移除物体、修改图片，或者修改风格等一系列操作。

## 3 微调提示词

利用正面和负面提示词还不能生成很满意的照片，还需要做进一步细节调整？还有一些提示词调整技巧，下面会从 3 个用法角度来介绍如何微调提示词，

(1) 关键词权重

(2) `()`和`[]`语法

(3) 关键词混合

### (1) 关键词权重

关键词权重，是指通过语法`(keyword: factor)`来调整关键词权重，其中 `factor` 为权重值，小于 1 表示减低重要度，大于 1 表示增大重要度。

下面这个例子调整了关键字`dog`的权重值。如下图所示，增大`dog`权重一般会生成更多狗，减少权重则降低了生成狗的数量。这种技术也可以应用到风格 `style` 或者光照 `lighting` 调整。

```
dog, autumn **in** paris, ornate, beautiful, atmosphere, vibe, mist, smoke, fire, chimney, rain, wet, pristine, puddles, melting, dripping, snow, creek, lush, ice, bridge, forest, roses, flowers, by stanley artgerm lau, greg rutkowski, thomas kindkade, alphonse mucha, loish, norman rockwell.
```

![](img/99dae5e1b5c0b0d3bbc3ecac78c91db5.png)

图20 不同权重提示词`dog`的生成图片

### (2) `()`和`[]`符号

调整关键词强度的另一种等效方法是使用`()`和`[]`符号。`(keyword)`把关键词强度增加到 1.1 倍，与`(keyword:1.1)`效果一致；`[keyword]`把关键词强度降低到 0.9 倍，与`[keyword:0.9]`效果一致。

这里可以使用多个，跟数学里的连乘操作是一样的。

```
(keyword): 1.1
((keyword)): 1.21
(((keyword))): 1.33
```

### (3) 关键词混合

这里还可以通过混合两个关键词来实现更有趣效果，使用语法为`[keyword1 : keyword2: factor]`，其中 `factor` 值控制了把 `keyword1` 切换到 `keyword2` 的步骤值，是一个介于 0 到 1 之间的数字。

举个例子，输入提示词`Oil painting portrait of [Joe Biden: Donald Trump: 0.5]`，采样步数设置为 30。这里指的是，第 1~15 步，提示词为`Oil painting portrait of Joe Biden`；第 16~30 步，提示词为`Oil painting portrait of Donald Trump`。解释一下，`factor` 值决定了关键词的切换节点，设置为 0.5 时指的是在 30*0.5 = 15 步时切换。

关键词融合技术还能用于生成高度相似的图片编辑。下面是使用相同提示词生成了两张图片，随机种子和迭代步数保持不变，只修改了`[apple: fire: factor]`里的 `factor` 权重。这背后的工作理论是，生成图片的总体组成是由早期扩散过程决定的，后面的一些关键词调整不会对图像整体产生很大影响，只会改变一小部分。

![](img/2e4fabab020de27aa958f002dfaaec56.png)

图22 关键词混合`[apple: fire: factor]`的生成图片

## 4 用 chatGPT 生成

手动改 prompt 很麻烦？能不能用 chatGPT 生成高质量提示词来实现稳定扩散？答案是可以的，但需要用一些技巧来引导。chatGPT 是 openAI 在 2022 年 11 月发布的聊天机器人，在 GPT3.5 大语言模型的基础上进行了微调。GPT3 系列使用了千亿级别文本训练出来的，chatGPT 还进一步引入了 RLHF 强化学习来学习人类偏好。因此，chatGPT 包含了大量人类知识，可以生成文章、诗歌、小说甚至是各种计算机代码。

那 chatGPT 了解 stable diffusion 技术吗？答案是不了解，因为它的训练数据只截止于 2021 年 9 月。幸运的是，它了解什么是文生图 text-to-image 和 DALL-E 模型，可以利用这块知识构建提示词工程。

![](img/9d1a0c43a37c4e6ea1949d5e1604badd.png)

图23 chatGPT是否了解stable diffusion、text-to-image和DALL-E技术？

如何调用 chatGPT 呢？最直接的方式是上`chat.openai.com`使用，内网也有一些中转方式，比如用企微里的 chatGPT 机器人或者搭建一些内部平台，这里不过多介绍。下面介绍使用 chatGPT 来生成一个人物特写照片，这是比较难的一个任务。

让 chatGPT 生成高质量提示词，也是需要一些技巧的，包括以下三点：(1) 指定图片画风，比如 oil-painting 油画风等；(2) 确定图片主体，如前面提到用一个明星名字是个不错选择；(3) 给出具体例子的话往往学的更快。

综合一下，输入 ChatGPT 的提示词如下：

```
**Exa**m**ple**s of high quality prompt for stunning close-up photorealistic illustration of Ana de Armas for text-to-image models (Stable Diffusion, midjourney or Dalle2) are
– portrait of beautiful happy young ana de armas, ethereal, realistic anime, trending on pixiv, detailed, clean lines, sharp lines, crisp lines, award winning illustration, masterpiece, 4k, eugene de blaas and ross tran, vibrant color scheme, intricately detailed
– alberto seveso and geo2099 style, A highly detailed and hyper realistic portrait of a gorgeous young ana de armas, lisa frank, trending on artstation, butterflies, floral, sharp focus, studio photo, intricate details, highly detailed, by Tvera and wlop and artgerm
Give me more examples.
```

chatGPT 的回复为：

```
A** p**hotorealistic, highl**y **detailed illustration of Ana de Armas in a vintage Hollywood style, reminiscent of the golden age of cinema, with a focus on glamour and elegance.
A close-up illustration of Ana de Armas in a neo-noir style, reminiscent of the film noir genre, with a focus on shadows, contrasts, and a sense of danger.
```

把这些提示词输入到 `Stable Diffusion v1.5` 里，使用 `DPM++ 2M Karras` 采样器，迭代步数为 25，图片分辨率为 502×704。负面提示词使用上面提到的通用版本。

chatGPT 提示词 1：

```
A** p**hotorealistic, highly detailed illustration of Ana de Armas in a vintage Hollywood style, reminiscent of the golden age of cinema, with a focus on glamour and elegance.
```

![](img/7a15265e9767f65210581b0c3ac9416b.png)

图24 chatGPT提示词1的生成图片

chatGPT 提示词 2：

```
A close-up illustrat**io**n of Ana de Armas in a neo-noir style, reminiscent of the film noir genre, with a focus on shadows, contrasts, and a sense of danger.
```

![](img/676fe2367d794c7aa5b4396f748bf0ac.png)

图25 chatGPT提示词2的生成图片

这里只进行了初步尝试，生成效果看起来还不错，可以后续进一步深入挖掘。

此外，还可以使用 magicPrompt 模型。基于 GPT-2 模型，它使用了大约 8 万条 stable diffusion 优质提示词训练而来，旨在为 stable diffusion 生成完善提示词。只要输入图片主体，就能补全各种细节，体验网址如下：

[`M`](https://huggingface.co/spaces/Gustavosta/MagicPrompt-Stable-Diffusion)agicPrompt Stable Diffusion - a Hugging Face Space by Gustavosta

![](img/bd0275e785a21a27a18cda0b81963d91.png)

图26 magicPrompt功能示例

## 三 本文小结

AI 绘画各种技术和应用不断涌现，也很大程度上提高了不少行业产出效率。这篇文章主要介绍 stable diffusion 里的提示词 prompt，从提示词构成、调整规则和 chatGPT 辅助等角度出发，介绍如何更好地输入提示词，才能更好地控制 AI 绘画生成。

### 参考链接

1、[`GitHub - CompVis/stable-diffusion`](https://github.com/CompVis/stable-diffusion): A latent text-to-image diffusion model

2、[`Denoising Diffusion Probabilistic Models`](https://arxiv.org/abs/2006.11239)

3、[`How to come up with good prompts for Stable Diffusion - Stable Diffusion Art`](https://stable-diffusion-art.com/how-to-come-up-with-good-prompts-for-ai-image-generation/)
