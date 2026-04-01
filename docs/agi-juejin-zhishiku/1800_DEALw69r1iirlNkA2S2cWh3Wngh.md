# AI绘画软件Fooocus教程

> 来源：[https://agijuejin.feishu.cn/docx/W4wHdsdMxo2rtAxRcD6c1JJynZe](https://agijuejin.feishu.cn/docx/W4wHdsdMxo2rtAxRcD6c1JJynZe)

> 作者：德里克文

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/656708919)hihu.com/p/656708...

**Fooocus可以被认为是简洁版S**[**tabl**](https://www.zhihu.com/search?q=%E5%85%8D%E8%B4%B9%E7%89%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)**e** Diffusion或者说是 免费版 的Mid Journey，它采用的是SDXL的内核，界面简洁类似Mid Journey一样的操作方式，它不需要很高的硬件条件就能运行。

![](img/38fcc9b91fa5b476fa12c2912d646ece.png)

Fooocus结合了Stable Diffusion和各自的优点，更采用多点选而非输入功能，它保留了Stable Diffusion的开源属性，可以免费部署到本地使用。

与此同时，它还向Midjourney学习，简化了操作界面，省去了复杂的参数调节，让用户可以更专注于提示和图像的创作。下面我来展示下怎么使用Fooocus这款软件吧，内容丰富充实有干货，建议收藏再看。

![](img/b1fb9eb7c284fdb40dd767c67c710e61.png)

## 一. Fooocus介绍

[Fooo](https://www.zhihu.com/search?q=%E5%BC%A0%E5%90%95%E6%95%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)cus是由大名鼎鼎的Controlnet 的作者的 张吕敏 （Github上用户名lllyasviel） 在 Github 上发布的一款全新的开源 AI 绘画工具，它可以和 Stable diffusion WebUI 一样部署到本地免费使用，同时还具备了同 Mid Journey一样便捷的操作界面。

![](img/29a3803f4163348e57ba3a81b83ad579.png)

[Fooo](https://www.zhihu.com/search?q=%E5%8E%8B%E7%BC%A9%E5%8C%85&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)cus[最新](https://link.zhihu.com/?target=https%3A//github.com/lllyasviel/Fooocus/releases/download/release/Fooocus_win64_2-0-50.7z)安装包下载链接（ 压缩包 ）： https:// github.com/lllyasviel/F ooocus/releases/download/release/Fooocus_win64_2-0-50.7z

下载文件后，请将其 [解压缩 ](https://www.zhihu.com/search?q=%E8%A7%A3%E5%8E%8B%E7%BC%A9&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)，然后运行“run.bat”。

![](img/7e269b95ed2e783ab3ea52a4920a86ce.png)

第一次启动软件时，它会自动下载基础的SDXL模型，你只需耐心等待即可：

> 它将 [从此处下载 sd_xl_base_1.0](https://link.zhihu.com/?target=https%3A//huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0_0.9vae.safetensors)_0.[9vae.safetensors 作为文件“F](https://link.zhihu.com/?target=https%3A//huggingface.co/stabilityai/stable-diffusion-xl-refiner-1.0/resolve/main/sd_xl_refiner_1.0_0.9vae.safetensors)ooocus\models\checkpoints\sd_xl_base_1.0_0.9vae.safetensors”。

> 它将 从此处下载 sd_xl_refiner_1.0_0.9vae.safetensors 作为文件“Fooocus\models\checkpoints\sd_xl_refiner_1.0_0.9vae.safetensors”。

如果你已经提前下载好了这些文件，也可以将它们复制到上述位置以加快安装速度。

如果你看到命令行出现 “Meta data Incomplete Buffer” ，则代表你的模型文件已损坏。请重新下载模型。

下面是在我的电脑，显卡为Nvidia 3060 ，GPU显存12G，RAM内存32G上进行的测试。可以看到运行SDXL毫无问题

![](img/aefea72b1db2468aa13253f483d3de75.png)

根据**作者所述，Fooocus最低要求是 4GB Nvidia GPU** 内存 (4GB VRAM) 和 8GB 系统内存 (8GB RAM)，这个要求比起SDXL来说简直是降低了太多了，可谓是电脑配置不好，但是希望在本地运行AI绘画小伙伴的大福音！

接下来让我们看下 Fooocus 的操作界面。

![](img/6a9e7fa1f7a82621fffa437f8f02be19.png)

![](img/1022eb572c7cc3f27ecc70809d5d6198.png)

它的基础界面非常简单，只有图像生成窗口、提示词输入窗口和生成按钮 3 项。另外如果你勾选“Advanced”的话，会弹出高级设置的窗口。

我[们可以在进阶设](https://www.zhihu.com/search?q=lora%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)置窗口里面调整各项数值，包括画面宽高比、风格、图像数量、随机种子数值、反向提示词、Checkpoint大模型、 lora模型 及权重比值、图像丰富程度等等内容。我会在接下来的内容进行详细说明。

![](img/d5ebfc99a4024b9303c55f6bad354634.png)

## 二. 基础操作

Fooocus基础操作极为简单，只需要输入提示词，点击生成即可，下面是我用不同提示词生成的不同的效果。

Prompt：

```
A monstera painted using the palette knife technique, deep greens with dappled sunlight yellow palette
```

![](img/41d458b06fc836fd8897efb3716845df.png)

Prompt：

```
Amazing seaside sunrise
```

![](img/d2210dbd1ca943f2a4d379c0598ee1f9.png)

Prompt：

> Selfie POV, Cat taking selfie from top of Burj Khalifa, Cinematic, ultra-realistic, highly detailed

![](img/2e7368f7377a6b4d21bd60ab71b3550a.png)

## 三. 进阶使用

接下来我来详细说明下进阶界面有什么内容。

![](img/a043f765d249fce56c348cbfd98b2e1c.png)

[首先是第一](https://www.zhihu.com/search?q=%E5%9B%BE%E5%83%8F%E8%B4%A8%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)列的设置，第一行是关于速度还是 图像质量 的选择，只需要做点选即可

第二行主要是图片的尺寸，和Stable Diffusion UI不同，Fooocus无法随意设置尺寸，只能从已经有的尺寸中进行选择，对应不同的图像比例，这样也确保了不会出现图像太大导致的异常，如果对于图片像素不满意，我们还可以使用Stable Diffusion进行图像放大。

第三行主要是一次生成的图像数量，最多可一次生成32张，虽然不如Stable Diffusion那么自由可以让机器自己跑几百张图不用管，但是32张也可以跑不少时间了。

第四行是 [反提示词 ](https://www.zhihu.com/search?q=%E5%8F%8D%E6%8F%90%E7%A4%BA%E8%AF%8D&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)，经过测试，SDXL相对比较稳定，反提示词不写出图质量也过得去。自己按需设置。

第五行是随机种子，可以固定种子数字，和Stable Diffusion是一样的。

![](img/27ebd0ae27192da8f3541cdabbaaef17.png)

第[二列第一](https://www.zhihu.com/search?q=%E7%94%9F%E6%88%90%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)行是选择是否设置RAW格式，这个RAW格式就和Mid Journey 5.1版本类似，RAW 模式可以通过在生成图像时减少图像 生成器 的“偏见”，从而提供更多的创作自由。这意味着生成的图像将更少受到预设的影响，更加接近原始输入的风格和意图。

第二行是 Style [预设风格 ](https://www.zhihu.com/search?q=%E9%A2%84%E8%AE%BE%E9%A3%8E%E6%A0%BC&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)，Fooocus 的 Style 预设风格设置里面包含了上百种不同的预设风格，涉及各个方面的效果，让用户可以轻松生成更多样、更具艺术性的图像。关于风格我在下一节将进行详细的举例说明。

![](img/fe21af809edaacc0abb72052ff4c0de9.png)

第三列里面的内容主要是关于模型的，我们依然可以和Stable Diffusion一[样，选择](https://www.zhihu.com/search?q=%E6%A0%B9%E7%9B%AE%E5%BD%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)不同的CheckPoint 、Lora、Vae等等模型。Fooocus 根目录 的 models 文件夹内很多内容和 SD WebUI 都是相似的，你如果有SD WebUI的经验，很容易就可以找到对应的文件夹。

![](img/64333603faa881f8acd4ff3d732e11e9.png)

如果你有在C站或者其他地方下载基于 SDXL的模型，你同样可以放进对应的文件夹，然后在 Fooocus 中进行使用。

![](img/b150e5f1b7a91c8cae64ab50d77de4cb.png)

## 四. 风格效果举例

我采用同样一个提示词，对预设风格进行了一个直观的渲染，将结果分享给大家，由于数量实在太多，我将逐步更新。

均采用同样的一组提示词和设置，仅仅是预制风格的不同，我按照风格的顺序进行排序。

采用的Prompt是:

```
Selfie POV, Cat taking selfie from top of Burj Khalifa, Cinematic, ultra-realistic, highly detailed
```

### 1. cinematic-default

![](img/28183b7e6428b602a621a1611eb5bd2b.png)

### 2.sai-3d-model

![](img/bd3b7c6fde66ea13fa725f4db0ba1fe6.png)

### 3.sai-analog film

![](img/d239bc8352bed9436433fe85a048a0dd.png)

### 4. [sai-anime](https://www.zhihu.com/search?q=sai-anime&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)

![](img/089df83c77830891c35c68816f05402f.png)

### 5.sai-cinematic

![](img/daf89cc1850785867ae664c037936bba.png)

### 6. [sai-comic book](https://www.zhihu.com/search?q=sai-comic%20book&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)

![](img/8ef0c6555531872507836dc841303628.png)

### 7.sai-craft clay

![](img/2c137c4302e74f70139170501ef5182d.png)

### 8.sai-digital art

![](img/8db856477fefd74f3998a43c871b3523.png)

### 9.sai-enhance

![](img/9d044be145835c22b87ea74c3a5da5e7.png)

### 10.sai-fantasy art

![](img/c440e805eaf5cb96ec66764fc2638219.png)

### 11.sai-isometric

![](img/3a38be59f01dddd8f362c5eeed59319f.png)

### 12.sai-line art

![](img/61f64f30f85349e1595fc86c310de2d7.png)

### 13.sai-lowpoly

![](img/3921021aef4dc4e8519770a35bf1258f.png)

### 14. [sai-neonpunk](https://www.zhihu.com/search?q=sai-neonpunk&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)

![](img/47a8dfdc719bbbea18c7af85992d3b6d.png)

### 15.sai-origami

![](img/ad3aa73316bf1378268b164b18dc56a3.png)

### 16.sai-photographic

![](img/ab21fcce88ace17708953bb820c2a05a.png)

### 17.sai-pixel art

![](img/157ada3e3eb5ae283ed4a18b585d8a0f.png)

### 18. [sai-texture](https://www.zhihu.com/search?q=sai-texture&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A656708919%7D)

![](img/cfe0d0b2279737a9cd0139ea190e1825.png)

Sai开头的已经完成，后续的等我有时间一一补上。

## 结语

总的来说，我认为Fooocus这个软件从操作上挺简单的，目前暂时没有ControlNet的操作界面导致可控性不如Stable Diffusion WebUI，但是考虑到开发者其实就是ControlNet的作者，同时在Fooocus的文件夹里已经预留了ControlNet，因此我觉得ControlNet的补充是迟早的事情。

![](img/6e6858d2f3f9ecbac72aa18b147eb012.png)

Fooocus的众多预设风格非常有趣，由于不需要特别复杂的提示词即可实现好的效果，特别适合不想折腾咒语的轻度AI绘画使用者，选择合适风格出图用于生成文章配图非常方便，推荐给大家。

---

我是 [德里克文 ](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3190897064%7D)[，一个对AI](https://www.zhihu.com/search?q=%E5%AE%A4%E5%86%85%E8%AE%BE%E8%AE%A1%E5%B8%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3207725131%7D)绘画，人工智能有强烈兴趣，从业多年的 室内设计师 ！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！