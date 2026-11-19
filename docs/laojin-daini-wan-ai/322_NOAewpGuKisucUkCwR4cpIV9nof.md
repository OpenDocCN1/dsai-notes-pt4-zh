
# 操作说明

> 来源：[https://my.feishu.cn/docx/H0c8dBt6do6n0XxDesBclsfynlR](https://my.feishu.cn/docx/H0c8dBt6do6n0XxDesBclsfynlR)

## 主界面区域划分

![](img/1770b83000ccfa4784f8018d70c8359e.png)

主界面一般包括7大区域，从上往下依次为（见上图）：

1. [快捷设置](https://zhida.zhihu.com/search?q=%E5%BF%AB%E6%8D%B7%E8%AE%BE%E7%BD%AE&zhida_source=entity&is_preview=1)区域（红色）
1. 主功能菜单区域（橙色）
1. “提示词”区域（蓝色）
1. 参数设置区域（绿色）
1. 附加功能区域（黄色）
1. 图片生成区域（紫色）
1. 页尾版本信息区域（青色）

从上到下依次用对应的彩色框划定了出来

### 1. 快捷设置区域

![](img/3e7a6a371dcc00d46ac9e3e7feb05f15.png)

+ 该区域主要提供针对`Stable Diffusion Web UI`软件使用界面的快捷设置。用户可以将`Setting`之中提供的若干项快捷设置勾选出来，显示在这个区域，以方便在使用软件过程中快速切换设置；
+ `Setting`（在主功能菜单区域）之中的快捷设置选项很多，大家可以根据自己日常经常用到的需求，自行选择。一般常用的有：`sd_model_checkpoint`、`sd_vae`、`CLIP_stop_at_last_layers`；

具体设置方法如下：

#### ① 设置步骤一

打开主菜单中的`Settings`（设置），左侧菜单选取最下方的`Show all pages`（显示所有页面），右侧会把所有的设置页的信息都集中罗列出来，所以页面很长。此时用网页搜索方式查找到“`Quicksettings list`”（快捷设置列表）；

#### ② 设置步骤二

找到`Quicksettings list` 设置项后，在下拉菜单中选择自己需要的若干项。大多数熟练使用者都会选择以下三项，新手可以先照此设置：

`sd_model_checkpoint`、`sd_vae`、`CLIP_stop_at_last_layers`

+ `sd_model_checkpoint`，为安装时就默认有的选项，是文生图所需要的大模型的选取菜单，方便在生成图像时切换不同的模型；
+ [`VAE`](https://zhida.zhihu.com/search?q=%E5%8F%98%E5%88%86%E8%87%AA%E7%BC%96%E7%A0%81%E5%99%A8&zhida_source=entity&is_preview=1)（Variational autoencoder），中文名叫“变分自编码器”，简单可以理解为是一种画面饱和度的滤镜与画面微调功能。有些大模型是需要用到`VAE`来调整的，有些则不需要。所以在快捷设置中我们要显示这个选择菜单，方便在更换模型时`VAE`也对应地更换；
+ `CLIP_stop_at_last_layers`，简写为`Clip skip`（`Clip`跳过层）。指的是控制图像生成过程中`CLIP`模型的使用层数。可通过滑块取值从1-12。由于 `CLIP`是通过计算文本和图像之间的相关性来实现的，因此如果跳过太多的步骤，文本对生成图像的匹配准确度会逐渐降低。一般我们默认都是用1或2。

这里我建议大家增加两项：`samples_save`、`live_previews_enable`

+ `samples_save`（ `Always save all generated images` ），勾选则意味着每次`AI`生成的图像都自动保存，取消勾选则是不自动保存，需要手动去保存；
+ `live_previews_enable`（ `Show live previews of the created image` ），此项为默认开启的，但是系统提示不建议开启。因为此项开启后会在`AI`生图过程中显示生成的中间环节，这会影响`GPU`的使用效率。

#### ③ 设置步骤三

最后，在以上都设置好后回到页面上端，点击`Apply settins`（保存设置）按钮，然后再点击右侧的`Reload UI`（重启`WebUI`）进行软件重启。重启后，你会发现你的`WebUI`最上端多了这些快捷设置项。

### 2. 主功能菜单区域

![](img/f925e798520669cd5e303cdb3af817fb.png)

+ 该区域罗列出了`Stable Diffusion WebUI` 软件里的若干主功能项，以标签方式展现；
+ [默认打开软件后显示为第一项——`tex2img`（文生图）](https://zhida.zhihu.com/search?q=%E6%96%87%E7%94%9F%E5%9B%BE&zhida_source=entity&is_preview=1)，文生图也是我们用到的最多最主要的功能；
+ [其他还有`img2img`（图生图）、`Extras`（附加功能）、`PNG info`（图片信息）、`Checkpoint Merger`模型合并、`Train`训练、`Settings`（设置）、`Extensions`（扩展）](https://zhida.zhihu.com/search?q=%E6%A8%A1%E5%9E%8B%E5%90%88%E5%B9%B6&zhida_source=entity&is_preview=1)；
+ 大家会发现我的截图里还多出了许多项目，这些项目都是我后期安装的插件。我们以后会详细介绍每一个菜单和每一个目前最流行的主要插件的安装和功能。

### 3. “提示词”区域

![](img/115422cb7831b05dd9002a64b674c762.png)

该区域是`Stable Diffusion WebUI` 的重点频繁使用区域，其中有四个小区域：

① 正向提示词区  
② 反向提示词区  
③ 生成按钮  
④ “提示词”功能按钮区  
⑤ `styles`（提示词预设加载）区  
⑥ 大家还发现了一些我未在此标注的功能区域，这些区域是额外安装的辅助提示词的插件，稍后我会详细介绍。

![](img/c1fa96b4d24ea553b1537bde2c0ed737.png)

#### ① 正向提示词区：

+ 我们之前介绍过，“提示词”`Prompt` 分正向提示词与反向提示词。该区域就是专门填写正向提示词的输入区域；
+ `Embedding` 与 `LoRA` 模型也是直接通过文本的方式输入到此区域。稍后在讲到`Embedding` 与 `LoRA` 模型时，我们会详细讲解；
+ 从网络上尤其是C站上直接拷贝来的提示词组合，其中含有正向与反向提示词、还有参数数值的许多文本信息。统一将这些文本复制到这里（正向提示词输入区域），然后点击右侧的小箭头“↙”自动把正反提示词与参数分配给不同的区域。

#### ② 反向提示词区：

![](img/c46b84c8cbb6aea5a7f3531c90913f28.png)

+ 专门输入反向提示词的区域；
+ 该区域也可以输入反向提示词的 `Embedding`；

#### ③ 生成按钮：

![](img/a6a6ab298c1c97a90cc21d2c13409778.png)

+ 无论是在`txt2img`（文生图）还是在`img2img`（图生图）、`Extras`（附加功能）、`PNG info`（图片信息）等其他功能标签里，点击此`Generate`（生成）按钮就意味着让`AI`开始作图了。
+ 点击后，该按钮会切换为`interrupt`（终止）和`skip`（跳过）两个小按钮，可以让你终止或跳过`AI`生图的进程。
+ 右键点击此`Generate`（生成）按钮，会出现`Generate forever`的选项，这个会让`AI`一直生成下去，直到你停止它为止，这对于大批量生成图片后进行筛选有一定帮助。

#### ④ “提示词”功能按钮区（注：以`WebUI 2.0`版本为主，如今`WebUI`已经升级为`SDXL`版本，此处略有不同）：

![](img/9369ed4a4a3ea066c0ce28da692d3a1f.png)

+ 左数第一个，斜向左下的小箭头“↙”按钮，可以将网上拷贝过来的正反提示词与参数混合在一起的文本进行分离，并分配给`WebUI`的不同输入与参数区域；
+ 第二个，垃圾桶图标的按钮，清空正向与反向提示词输入框中的所有内容，但是不会将参数设置区域内的参数复位；
+ 第三个，这个图标的图形表达不是很清晰（似乎是一个黑色的山丘上升起了一个太阳），但它的功能是很重要的，就是点击后显示 `LoRA`、`Embedding`、`LyCORIS` 等模型，方便选取。再点击一次该图标，取消显示；
+ 第四个，手写板小图标，是一个读取按`styles`的按钮。点击后，它将下方 `styles`（提示词预设加载）区里选择好的`styles` 内的所有文本展开并填入正向或反向提示词区域用的；
+ 第五个，一个很古典造型的磁盘图标，是存储按钮，这个按钮是用来存储正向与反向提示词输入框里的文字，将他们简化为一个简单名称存储下来，存到 `styles` 里（通常是在`styles.csv`这个表格文件里），方便日后调用。

#### ⑤ styles（提示词预设加载）区

![](img/e56a44576b0f975e15a15d78cfd84ded.png)

+ 通过之前读取按钮和存储按钮的介绍我们可以了解到，`styles`区域实际上是对一系列“提示词”的集合存储，通常用一个简单的名字来命名这个集合，方便日后索引和调用。通过这个便利功能，日后就不用每次在提示词输入框里，再手动地输入很多标准套路的参数类的提示词了。
+ [这个`styles`](https://zhida.zhihu.com/search?q=%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6%E5%A4%B9&zhida_source=entity&is_preview=1)在`Stable diffusion webui`的本地文件夹下，通常是在`styles.csv`这个表格文件里存储。你可以手动在表里增删改“提示词”内容；
+ `styles.csv` 文件里是可以区分正向和反向提示词的。大家可以用`Excel`打开这个文件看看。

### 4. 参数设置区域

参数设置区域里包括一些基础的`AI`生图参数（文生图与图生图的这个区域大体一样，略有少数不同）：

![](img/3765f5045611569f8eb3d0e90894a733.png)

`Tex2img`文生图的参数设置区域：

+ `Sampling method` 采样方法(`Sampler`)
+ `Sampling steps` 采样迭代步数(`Steps`)
+ `Restore faces` 面部修复
+ `Tiling` 平铺/分块
+ `Hires. Fix` 高清修复
+ `Width` 宽度/`Height` 高度
+ `Batch count` 生成批次
+ `Batch size` 每批数量
+ `CFG Scale` 提示词相关性
+ `Seed` 随机种子
+ `Script` 脚本

#### `Sampling method` 采样方法(`Sampler`)

![](img/8e89ab590931a3ea88f830253adf680f.png)

目前这个版本罗列了20种采样方法, `Euler a`, `Euler`, `LMS`, `Heun`, `DPM2`, `DPM2 a`, `DPM++ 2S a`, `DPM++ 2M`, `DPM++ SDE`, `DPM fast`, `DPM adaptive`, `LMS Karras`, `DPM2 Karras`, `DPM2 a Karras`, `DPM++ 2S a Karras`, `DPM++ 2M Karras`, `DPM++ SDE Karras`, `DDIM`, `PLMS`, `UniPC`

对于采样方法`Sampling method`，我们需要了解一些内容。首先，`Stable Diffusion` 中的所有采样器都是出`Stability AI` 公司的首席研究员`@RiversHaveWings` （ `Katherine Crowson`） 之手。不同的采样方法都有比较学术的解释，例如：`Euler a`采样器使用了祖先采样（`Ancestral sampling`）的`Euler`方法，有`Karras`后缀的采样器都是基于`Karras`论文中提到的运用了相关`Karras`的`noise schedule`的方法等等。这些采样器背后都有十分生涩难懂的学术原理。我们需要了解的是哪种采样器适合哪类图形哪种艺术风格，以及他们各自的优缺点。

`DDIM`(`Denoising Diffusion Implicit Models`) 和 `PLMS`（`Pseudo Numerical Methods for Diffusion Models on Manifolds`） 是`sd`的原始采样器。

+ `Euler a` ：算是比较通用的采样方法，但会偏向于适合插画类图像，环境光效质感较弱，对于`Prompt`提示词的利用率仅次与`DPM2`和`DPM2 a`，有时会产生异想天开的构图效果；
+ `Euler`：也适合插画类图像，画面整体较柔和，环境细节与渲染好，背景则模糊较深远；
+ `Heun`：出图平均质量比`Euler`和`Euler a`高，在采样步数较高的设置下出图表现较好，因而出图速度较慢；
+ `DDIM` ：是`SD`的资格最老的原始采样方式，它甚至和`PLMS`构成了`SD`源头的一部分。它适合宽幅画面，也是在采样步数较高的设置下出图表现较好，因此速度偏低，环境光线与水汽效果好，写实质感不佳。在负面提示词不充分情况下，往往发挥很随意；
+ `DPM2` ：该采样方法对`Prompt`提示词的利用率可达80%以上，所以，如果你是`Prompt`比较熟练，期望用`Prompt`控制画面上的更多细节，可以多采用这个方法；
+ `DPM2 a`：几乎与`DPM2`相同，对人物特写有一定帮助；
+ `PLMS` ：也是`SD`的老资格原始采样方式，单次出图质量仅次于`Heun`；
+ `LMS` ：饱和度与对比度偏低，比较适合动画的风格；
+ `LMS Karras`：很适合油画风格，所以写实类不佳；
+ `DPM fast`：是早起开发者的一种测试方法，不适合设计师出图；
+ `DPM++ 2M`：和`Euler a` 一样，比较通用，在低采样步数（低于 20）时就可以产生优质的画面。

目前好用的有 [`Euler a`](https://zhida.zhihu.com/search?q=Euler+a&zhida_source=entity&is_preview=1)（更细腻），和 `DDIM`（适合局部重绘），新手推荐使用 `Euler a`。`Euler a` 富有创造力，不同步数可以产出不同的图片。调太高步数 (>30) 效果不会更好。`DDIM` 收敛快，但效率相对较低，因为需要很多 `step` 才能获得好的结果，适合在局部重绘时候使用。

#### `Sampling steps` 采样迭代步数(`Steps`)

![](img/e96bafaa166da98ef68f12c86fd6963a.png)

简单来说采样迭代步数（`Steps`）就是希望出图时，采样器来计算多少步数来出图，但并非步数越多越好，不同出图效果、模型选择在使用采样迭代步数上都需要有相应的调整，多数情况迭代步数20-30步就可以了。

更多的迭代步数到底好不好？更多的迭代步数可能会有更好的生成效果，更多细节和锐化，但是会导致生成时间变长。而在实际应用中，30 步和 50 步之间的差异几乎无法区分。太多的迭代步数也可能适得其反，几乎不会有提高。

#### `Sampling method` 采样方法(`Sampler`)与 `Sampling steps` 采样迭代步数(`Steps`)之间的关系

![](img/aa2a0800dcbc7d0e09850230610c0a7c.png)

+ `Euler A` 是兼顾速度和质量的最优之选；
+ `DPM2 A` 在核理分配步数的情况下也能产生高质量作品；
+ `DDIM` 和 `Euler` 则在运气较好的情况下尤其以细腻的画风见长；
+ `DPM Solver` 系列算法可以相对低步数即达到同等甚至更高出图质量的新型高阶算法；



+   当你审美疲劳时，更换采样方法也许可以带来耳目一新的感觉。但不推荐 `LMS`、`DPM fast`、`LMS Karras`和 `PLMS` 这四个，它们的生成质量在大多数情况下相较于其它算法而言不佳；

+   [`DPM A` 和](https://zhida.zhihu.com/search?q=%E9%9D%9E%E7%BA%BF%E6%80%A7%E8%BF%AD%E4%BB%A3%E6%96%B9%E6%B3%95&zhida_source=entity&is_preview=1) `Euler A` 都是非线性迭代方法，并不会因为迭代步数增加而使得生成图像无休止地变得更优秀，在大于一定的迭代步数之后反而质量会快速下滑；

+   `DDIM` 和 `Euler` 等线性迭代方法则恰恰相反，质量往往依托于迭代的次数。但也存在边际效应的问题，当迭代步数过大时，再增加迭代次数也不会让画面产生显著变化；

+   实际使用时需要根据画布大小和目标是否复杂来综合考虑。在`512 * 512`的小尺寸标准画且无强烈细节要求的简单画面中，使用 `Euler A` / `DDIM` 等推荐迭代步数在 30 到 40 之间，而使用 `DPM2 A` 等需要高步数的算法时则推荐迭代步数翻倍。而使用 `DPM Solver` 系列则推荐 20 到 30 。

## • Restore faces面部修复

![](img/1533e09d66f0c9851d31d8140bc034dd.png)

由于目前AI生图领域对人的脸部和手部仍然无法做到很好的控制，经常导致脸崩和手崩的情况发生。现实世界中，人类对脸部的微妙细节和变化十分敏感。除了哭、笑等大幅度的[表情特征](https://zhida.zhihu.com/search?q=%E7%94%BB%E9%9D%A2%E6%AF%94%E4%BE%8B&zhida_source=entity&is_preview=1)外，神经网络尚无法完全捕到如此细腻的变化，导致Ai生成的人脸不自然或扭曲的结果……所以，需要让AI专门针对脸部进行二次再生成，即修复。但是此功能对脸部占画面比例较小的图来说，基本无效。

（对于脸部占比小的图如果进行脸部修复的话，目前最好的办法是用到了`Face Editor`等插件，我们稍后会详细介绍。）

![](img/d63757d59144303136935db98e7d4e11.png)

脸部崩坏的原图（左图）与面部修复后的效果（有图）

## • Tiling 平铺/分块

![](img/abcb4f0e42639a1acdf13a3215adb19e.png)

[这个功能](https://zhida.zhihu.com/search?q=%E8%8A%B1%E7%BA%B9%E5%9B%BE%E6%A1%88&zhida_source=entity&is_preview=1)一般不会用到，但是个别设计领域会有用，比如需要做到四方连续的花纹图案时，这个功能勾选后会产生可以无限连续拼图的花纹图案。我们的窗帘花纹即是如此。

## • Hires. Fix 高清修复

![](img/36c0c6ed6bbdd0af9aa3dc6e044ce718.png)

由于目前`stable diffusion` V1-V1.5版本均是在`512*512`的原始训练素材基础上训练的，所以在用AI生图时，最好设置要生成的图片尺寸为`512*512`或`512*768`。但是，这样的图片分辨率对于专业设计师来说远远不够，我们有时需要1024以上的大图，此时就需要对图像进行二次生成式放大了。

注意：不是简单放大，而是通过神经网络对图像在更高尺寸上再生成。此时，高清修复便是经常用到的一个功能了，但勾选它会耗费额外的`GPU`计算资源和时间。另外放大后的图与原图细节会有略微不同。

![](img/70677d7c3a3ea6eca765c3a70832014f.png)

在点击了高清修复后，UI界面会额外延展出一些参数设置：

+   `Upscale`[r 放](https://zhida.zhihu.com/search?q=%E6%BD%9C%E5%8F%98%E9%87%8F&zhida_source=entity&is_preview=1)[大算法](https://zhida.zhihu.com/search?q=%E6%8A%97%E9%94%AF%E9%BD%BF&zhida_source=entity&is_preview=1)（16种）

`Latent` 潜变量、`Latent (antialiased)` 潜变量（抗锯齿）、`Latent (bicubic)` 潜变量（双三次插值）、`Latent (bicubic antialiased)` 潜变量（双三次插值并抗锯齿）、`Latent (nearest)` 潜在（最邻近）、`Latent (nearest-exact)` 潜在（最邻近-整数）、`Lanczos`、`Nearest` 最邻近（整数缩放）、`4x-UltraSharp`、`ESRGAN_4 x`、`LDSR`、`R-ESRGAN 4 x+`、`R-ESRGAN 4 x+ Anime6B`、`ScuNET`、`ScuNET PSNR`、`SwinlR 4 x SwinlR_4 x`。

每种放大算法各有各的特点，在结合重绘幅度的情况下会对原图有一定的变形，尤其是脸部。如果要保持细微处不变建议使用`Extras`中的放大算法来放大图片，这样可以进行精准控制。但如果是第一次生成图片，则需要在此处进行。我们将在下面对比这16种放大算法。

目前`Upscaler` （放大算法 ）的下拉菜单中，总共默认安装有16种放大算法，每一种放大算法都有各自适合的图像类型，有的对二次元类图片效果好，有的对真实场景真人照片类放大效果好。这些放大算法与`tex2img`（文生图）中`Hires. fix`（高清修复）所附带的放大算法中除了前6个`Latent`潜变量算法以外，后面12个都是一致的：

+   [`Late`](https://zhida.zhihu.com/search?q=%E5%9B%BE%E5%83%8F%E5%A2%9E%E5%BC%BA&zhida_source=entity&is_preview=1)nt：`Latent`系列采样方法，是一种基于 `VAE` 模型的图像增强算法，通过将原始图像编码成变量，并对其进行随机采样和重构，从而增强图像的质量、对比度和清晰度。

+   `Lanczos`：用于超分图像的传统算法。它根据每个像素点周围像素的远近关系不同赋予不同的权重，近的权重高，远的权重低，然后在放大的图像上进行补间。放大后的图像仍旧模糊 。

+   `Nearest`：是一种基于图像插值的算法，与`Lanczos`同属于传统算法，放大后图像仍旧模糊。

+   `BSRGAN`：比`Nearest`略好。

+   `ES`[`RGAN_4`](https://zhida.zhihu.com/search?q=%E7%94%9F%E6%88%90%E5%AF%B9%E6%8A%97%E7%BD%91%E7%BB%9C&zhida_source=entity&is_preview=1)`x` (`Real ESRGAN`) ：`Enhanced Super-Resolution Generative Adversarial Networks` （增强型超分生成对抗网络）的缩写，`4x`是适合放大4倍的意思，属于人工智能类型的算法。实测确实增加了很多看上去很真实的纹理，但是有时又会把一张图片弄得全是锯齿或怪异的纹理。适合真实照片质感图像，较符合人体皮肤的质感，面部轮廓，对毛孔处理较细致。不适合放大AI生成的图像，不具备去噪点功能。

+   `R-ESRGAN 4x+`：`R`是`Real-T`[`ime的意`](https://zhida.zhihu.com/search?q=%E5%9B%BE%E5%83%8F%E5%8E%BB%E6%A8%A1%E7%B3%8A&zhida_source=entity&is_preview=1)思，`+`代表可放大4倍以上，可对图像去模糊降噪，有涂抹感，导致有偏硬的色块感。不适合放大AI生成的图像。

+   `R-ESRGAN 4x+ Anime6B`：`Anime`是二次元的意思，`6B`指的是在`60亿`参数的动漫数据集上训练的结果，其中包含了大量不同风格、不同质量的动漫图像。在动漫图像增强领域具有较高的准确性和效果，并且适用于不同类型的动漫图像处理。另外，`Real-ESRGAN` 系列是由腾讯 `ARC` 实验室研发的，值得鼓励！

+   `4x-UltraSharp`：基于`ESRGAN`做了优化模型。`UltraSharp`顾名思义，会让图片变得平滑、锐利，适合修复表面纹理有问题的图片，很适合匹配真人质感模型使用。

+   `LDSR`：`Latent Diffusion Super Resolution`（潜在扩散超分辨率模型）的缩写。是耗时最长的，但同时也是效果最好的，尤其是对照片来说最好。

+   `ScuNET`：在`Lanczos`基础之上具备去噪点的功能，对人体肌肤有一定磨皮效果，如果你单纯只是想去噪点的话，用这个算法很适合。

+   `ScuNET PSNR`：与`SCuNET`相比，在真人质感领域几乎没有区别，适合动漫。

+   `SwinIR_4x`：使用了`Swin Transformer`的思想，属于对抗网络的新型算法，适合绘画类图片的放大，输出区域不稳定，有些区域模糊，有些区域又能放大细节。不适合放大AI生成的图像。

以上罗列的这些算法，由于`SD-WebUI`的版本不同可能会有部分算法不一致的情况。

[另外，网上还有更多的放大算法可供下载，可](https://link.zhihu.com/?target=https%3A//upscale.wiki/wiki/Model_Database)以去这个链接地址：`https://upscale.wiki/wiki/Model_Database`

[国外有一个热心的播主总结了300多种放大算法的优劣，生成了对](https://link.zhihu.com/?target=https%3A//phhofm.github.io/upscale/introduction.html)比图：`https://phhofm.github.io/upscale/introduction.html`

让我们做一个表格总结一下（✓代表适合、✕代表不适合、●代表未经针对训练但可能会适合）：

![](img/1bad921c32219f62d4b70e6534cfafed.png)

（数据总结绘制表格不易，转载请注明出处）

+   `Hires steps` 高清修复采样次数

采样次数越高，修复的细节越多，但耗费的资源与时间也越多。

+   `Denoising strength` 重绘幅度

重绘幅度高代表对原始素材进行二次修复时的变动幅度。数值越大，AI的创意就越多，同时也会使生成的图像越偏离原始图像。

+   `Upscale by` [放大倍率](https://zhida.zhihu.com/search?q=%E6%94%BE%E5%A4%A7%E5%80%8D%E7%8E%87&zhida_source=entity&is_preview=1)

直接按照二维尺寸的倍率进行调节。

+   `Resize width to` 将宽度调整到

`Resize height to` 将高度调整到

宽度和高度直接调整到具体的像素值。

## • Width 宽度/Height 高度

设置你要生成出的图像的长宽像素数值（建议`512*512`或`512*768`）；

![](img/d4a251af1d92514647e1ce9a852c4588.png)

## • Batch count 生成批次

每点一次生成按钮生成批次的数量，即生成几批次图片（对于`VRAM`使用效率没有影响），比如设置为4，就相当于重复了4次生成（可以理解为单线程做4次）；

## • Batch size 每批数量

每批次生成图片的数量（以很高的`VRAM`使用效率为代价以提高生成数量），比如设置为4，就相当于一张4倍幅面的大图里分4个区域生成4张小图（可以理解为4个线程同时做）；

如果要生成多个图时，`Batch count`（生成批次）与`Batch size`（每批数量）这两者建议选前者。前者耗时比后者少一些。显卡性能好的，可以选后者，否则生成进程因显卡吃不消而崩溃终止。

## • CFG Scale 提示词相关性

![](img/e60a2f4c71d29a7411bfdff4056fe9df.png)

`CFG Scale` 提示词相关性是经常会用到的概念，它代表着要生成出的图像与你输入的“提示词”之间的语义关联程度。数值越大关联程度越高。对于`512`的小图片`CFG`数值超过10可能会导致过度拟合而产生图片崩坏问题。所以一般建议设置在5-9之间。大图片推荐10左右。

## • Seed 随机种子

`Seed`种子概念也是经常用到的。如果你已经生成出一张接近满意的图片时，还需要生成更多类似这样构图及构图元素的图片，只是需要细节上有一些变化，以增加更多选择时，就可以用到`Seed`的这个概念。把之前那张接近满意的图片的`Seed`值拷贝到这里，则让AI继续生成类似的图片。如果不希望延续，则设为`-1`，即不使用`Seed`（点击骰子 ️按钮即可设置为`-1`，点击绿色环保小按钮♻️则从当前图生成区域刚刚生成的图片中提取`Seed`值到此）。

`Extra` 为差异随机种子，勾选后，会增加另一个种子“`Variation seed`差异随机种子”，此时填入这第二个种子，则意味着让AI在两个种子之间进行平衡。`Variation strength`差异强度滑块越靠左接近0则越倾向于第一个`Seed`，越靠右接近1则越倾向于第二个`Seed`。

随机种子(`seed`)的用途就是固定生成图片过程中所产生的随机数，从而在下次生成图片时最大限度地进行还原。当然即使我们使用了随机种子也不能做到100%地还原，最多算是一种参考。因为随机种子虽然固定了随机数，但是AI算法本身就有随机性，就算使用了随机种子还是会因为随机性带来生成图片的偏差，所以随机种子只能最大限度的降低随机性。

## • Script 脚本

![](img/048e9c8ca08c082ec652fdbbe0b1d1dd.png)

`S`[`cript脚本中有若干过往用户开发的功能脚本，有些被固化在了默认安装中`](https://link.zhihu.com/?target=https%3A//github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts)。可以到 `SD-WebUI` 的官方github上去下载额外的脚本（`https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts`）。这其中有两个脚本是我们以后会经常用到的：

一个是`Prompt matrix`（提示词矩阵）

`Prompt matrix` 可以让AI按照不同的提示词生成图片矩阵。如果我们有一大堆提示词要输入，但是你想要了解或者不确定其中几个提示词的组合效果，那么你需要多次生成，效率低也不方便。而启用了这个脚本后，可以使用“ | ”语法分割提示词，这样会组成多个提示词组合让你一次性地看到矩阵效果，让你一目了然。

举例：

```
1girl, | yellow dress, | long dress,
```

![](img/1caa7c71555e2abbf003ee9a54043da7.png)

另一个是`x/y/z plot`（`x/y/z` 图表）

无论是初学者还是有一定经验的用户很多时候也不能确定各个参数到底取哪个值是最符合需求的，这往往需要很多参数之间的平衡。 `x/y/z plot`脚本可以让我们创建一个最多三维的参数矩阵图表，让我们清晰地了解参数间的匹配关系。

举例：`X type`：`steps` 。`X values`：`8,12,16,20,25` 。`Y type`：`Clip Skip` 。 `Y values`：`1,2,3`

```
1girl, yellow dress, 
```

![](img/65326527cb9d2604f761b033ed4cd745.png)

## 5.附加功能区域

![](img/6b74f974fbf0836288c537b6a669961d.png)

附加功能区是插件的聚集地之一。在这里有一个`SD`中最著名的插件，也是我们以后使用频率最多的功能——`ControlNet`。

`ControlNet`可以让我们很好地对`Stable Diffusion`进行精准控制，尤其是人物的姿态，场景的深度等等。可以说，是`ControlNet`让`SD`进入到了`CG`的专业应用领域。

此处，还有许多其他著名的插件，比如`Adetailer`、`FaceEditor`、`Composable LoRA` 等等 。

之后我们会对这些进行详细讲解和实战案例展示。

## 6.图片生成区域

图片生成区域是我们最终的作品呈现的区域

![](img/c5a3ed5b9002714e3dac5ee2c9378cd7.png)

+   我们可以通过这个区域下方的文件夹按钮直接打开已经生成的图片所存储在的硬盘空间中的位置；

+   可以选择存储生成区域的图片到指定的硬盘空间中；

+   或[压缩文件](https://zhida.zhihu.com/search?q=%E5%8E%8B%E7%BC%A9%E6%96%87%E4%BB%B6&zhida_source=entity&is_preview=1)然后下载到硬盘里；

+   也可以把刚刚生成好的图片发送到图生图标签下、或局部重绘功能区（在图生图标签下的一个子功能区）、附加功能区（`Extras`）；

+   在图片生成区域最下方是本次生成的图片的所有信息，包括“提示词”、`Steps`，`Sampler`, `CFG scale`, `Seed`, `Size`，用到的大模型，`Clip skip` 等等。我们在网上的AI图库中看到各式各样的图，这些图片往往附带一系列的参数供我们研究，其实就是这个区域里的信息。

## 7.页尾版本信息区域

![](img/2ae4be64a8ad87fb819098e9e81175f7.png)

+   该区域可以看到`Stable Diffusion` 的版本号，还有各种运行环境的软件版本；

+   你可以点击上面的`API`、`Gitub`、`Gradio`在网络上的有关`WebUI`的详细介绍；

+   可以点击查看启动信息`startup profile`；



+ 点击`Reload UI`可以重启`WebUI`。

教程转载自：`https://zhuanlan.zhihu.com/p/671929701`，我也是看着这个学的，非常好用。

以上，本期分享完成，希望对大家有所帮助。

---

如果觉得不错，随手点个赞、在看、转发三连吧🙂

如果想第一时间收到推送，也可以给我个星标⭐～谢谢你看我的文章。

关注发送`知识精华`，可获得免费的知识链接地址。

![](img/25ce6e71b1a09c93300ee9fb1c284732.png)
