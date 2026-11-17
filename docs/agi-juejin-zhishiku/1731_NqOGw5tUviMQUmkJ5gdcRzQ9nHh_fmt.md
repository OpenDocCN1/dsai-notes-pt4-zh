
# Stable Diffusion 有什么推荐的 Checkpoint 模型、LoRA？

> 来源：[https://agijuejin.feishu.cn/docx/OtHPdgcAhomcRUxptLRcRJWin5c](https://agijuejin.feishu.cn/docx/OtHPdgcAhomcRUxptLRcRJWin5c)

> 作者：德里克文

> 原文：[https://www.zhihu.com/question/603580055/answer/3140053192](https://www.zhihu.com/question/603580055/answer/3140053192)

我这里来推荐下各种实用的 `Checkpoint` 模型、`LoRA` 吧！

![](img/2931b9a75e0eda1788a6cf8a96b86ca0.png)

## 一. 模型下载

### 1. 下载地址

Stable Diffusion 最著名和通用的模型下载地址是：

`https://civitai.com/` （需要 魔法 ），里面包括各类模型下载，俗称 C 站。

国内的模型分享平台包括：

可在线生图的 AI 模型分享社区，还是免费的！ `tusi.art/`

[LiblibAI·哩布哩布AI-中国领先原创AI模型分享社区](https://link.zhihu.com/?target=https%3A//www.liblibai.com/) `www.liblibai.com/`

### 2. 存放目录

点击进入网站，下载需要的模型文件后，需要放置不同的模型到各自的目录。

![](img/a97a6db33539c00336731cdc55ae82a6.png)

`Checkpoint` 模型存放位置：SD 主程序目录 `/models/Stable-diffusion`

VAE 模型存放位置：SD 主程序目录 `/models/VAE`

`Embedding` 存放位置：SD 主程序目录 `/embeddings`

`Hypernetworks` 存放位置：SD 主程序目录 `/models/hypernetworks`

`LoRA` 存放位置：SD 主程序目录 `/models/LoRA`

## 二. Checkpoint 大模型介绍

`Checkpoint` 是主模型，是决定整体出图质量最重要的模型。每个 `Checkpoint` 大模型都有自己独特的风格，按照我自己的喜好分类，我将模型分成这四大类：真实照片类、2.5D 仿真画风类、CG 动画画风类、二次元平面画风类（另外还有一个 NSFW 类，因为经常容易渲染出各种限制级画面，我单独分类，这就不展开说了）。

![](img/79863690c97d9dd89d401cacee72974b.png)

`Checkpoint` 文件是训练过程中保存的模型状态快照，我们可以理解为一种 风格滤镜 ，用于将模型输出结果转换为特定的风格。在下载 `Checkpoint` 文件时，建议查看下相应的模型简介和说明事项，作者通常会提供文件和对应的使用说明，以帮助使用者更好地理解和使用。

我推荐我常用常用的 `Checkpoint` 模型及不同的分类（对于初学者来说，建议每一种选我给的前 3 个即可）：

### 真实照片类

`Chilloutmix` (NSFW),

`ReallisticVision`,

`Deliberate`,

（`MajicmixRealistic`、`BeautifulRealistic`、`Lofi`、`ProtogenX34Photorealistic`）

![](img/c555ca5a31760604373ac27e5ef56bde.png)

### 2.5D 仿真画风类

`3Guofeng3`,

`ghostmix`,

`Dreamshaper`,

（`lyriel`、`NeverendingDream`、`ProgenAnime`）

![](img/937c7b5159db6d9bd045b65e3435e461.png)

### CG 动画画风类

`Abyssorangemix`,

`CetusMix`,

`DisneyPixarCartoon`,

`DarksushiMixMix`

![](img/e39a052841a5501f6f1935cb0ad8cfee.png)

### 二次元平面画风类

`Anything`,

`Counterfeit`,

`MixProColorbox`,

（`Meinamix`、`CardosAnime`、`DreamlikeDiffusion`）

![](img/8d388eef3a93eeb0b56fe692201bdb1f.png)

## 三. VAE 模型介绍

VAE 可被看作对模型的微调滤镜，主要给人的直观感受是色彩方面的调整，不同的 `VAE` 可能在细节上有一些差异，但不会对输出效果产生影响。

![](img/8e7de56058e21ef57fd8e2e2327ca977.png)

`VAE` 它可以提高模型的性能，有些模型预设了 `VAE` 效果，因此除了模型配套的 `VAE` 文件外，无需专门下载，可以选择自动模式进行匹配，即可正常使用。

![](img/d7bd1083c296e3a1891c97e283d98fcc.png)

## 四. Embedding 介绍

在 Stable Diffusion 中，`Embedding` 就像是一本使用说明，它能够将输入数据转换为向量表示，使得模型能够更方便地进行处理和生成。

如果拿菜谱举例，类似于我们想要做一份 地三鲜 ，在没有 `Embedding` 的情况下，我们在提示词里需要提到所有的细项特征，比如茄子、土豆、青椒、耗油、盐、香油、蒜、姜等配料。但是如果我们有了 `Embedding`，我们只需要在提示词中输入：“地三鲜”，系统就能够通过 `Embedding` 自动读取关于地三鲜的相关所有详细特征描述，这样我们就不需要每次都输入很多单词来控制生成的画面了。

![](img/c97667463143121c85ac56c5f61a48fc.png)

调用 `Embedding` 需要在提示词中进行描绘，这样就能启用相应的 `Embedding` 效果了。

`Embedding` 与其他模型（比如 `LoRA`）相比，`Embedding` 文件的大小只有几十 KB，而不是几百兆或几 GB。虽然还原度可能相对 `LoRA` 稍差一些，但在存储和使用方面更加方便。

需要注意的是有些 `Embedding` 是用在正面提示词，有些是用在负面提示词，例如 `EasyNegative`、`ng_deepnegative`、`verybadimagenegative` 这几个是用于负面的

![](img/e7fec7db7befb31383206b80715650cf.png)

我这里也介绍一些实用的 `Embedding`: `EasyNegative`，`ng_deepnegative`，`verybadimagenegative`

## 五. Hypernetworks（略过）

这个模型现在基本没人用，又麻烦效果上也比不上 `LoRA`，我就不介绍了，对于日常应用，忽略就好。

## 六. LoRA 介绍

`LoRA` 与 `embedding` 在本质上有相似之处，都携带着大量的训练数据。因此，`LoRA` 能够更加细腻地复制人物和细节特征。

### 1. 启动 LoRA

通常每个 `LoRA` 模型都有对应训练的 `Checkpoint` 大模型和相应的触发词汇，我们可以通过查看 `LoRA` 作者提供的相关内容来获取信息，并选择一些提示词，来更好的生成图片。

![](img/620466041eb4ad4f1bc135ccc027a210.png)

调用 `LoRA` 需要在提示词中进行描绘，`LoRA` 也可以叠加重复采用，例如我在下面的图片示例的，在提示词中加入 `<lora:画风LoRA:GhibliStyle:0.8>`，`<lora:add_detail:1>` 这样就能启用相应的 `LoRA` 效果了，`0.8` 和 `1` 则分别代表了这个的 `LoRA` 启用的权重。

![](img/dfc038ccc8f635bc8be7fa5d1cb9414f.png)

需要特别注意，每个 `LoRA` 模型都可以在提示词中设置权重属性。权重设置越大，对画面的影响就越大。一般来说，我建议 `LoRA` 提示词的权重保持在 0.3 到 1 之间。权重过低没有效果，权重过高，有可能出现过于拟和，影响出图质量。

我这里也介绍一些我自己常用的 `LoRA`: `add_detail`、`blindbox`、`animeiLineart`

这几个 `LoRA` 分别有什么效果呢，让我简单介绍下吧

### 2. add_detail

有了这个 `LoRA`，可以给你的画面增加更多的细节，我来用图片举个例子吧。

下面两张图片我用了相同的提示词和参数，仔细看看不同在哪里，细节更加丰富，错误更少了。上方的图是没有加 `add_detail` 这个 `LoRA` 的，下方的图我设置 `add_detail` 权重为 1，是不是就精致多了呢？你也可以自己试下。

![](img/46252d6e6179aa6445d983ce1aa9407d.png)

### 3. blindbox

最好用的生成盲盒类 `LoRA`，可以直接当生产力使用，不需言语，就是强烈推荐

![](img/4c140fed0d26973012a7eeb03164ca0a.png)

### 4. animeiLineart

这个 `LoRA` 最大的好处就是可以生成线稿图啦，可以很轻松制作漫画风格哦

![](img/a5870a5e4d8b1ae3d3076b30acf74c8d.png)

好了，关于模型分类你是否了解了呢，如果不清楚可以评论区留言给我哦，我会进行解答。

---

我是 [德里克文](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3140053192%7D)，一个对 AI 绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！
