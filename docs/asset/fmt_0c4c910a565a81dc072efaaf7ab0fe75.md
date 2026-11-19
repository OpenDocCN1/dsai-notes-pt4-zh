
![](img/38131fda5bb639b930ea8692c3397eb6.png)

4. 然后就打开啦，一键启动就好了。

![](img/de6b9c7c2458d0a5392e15df5fea72e6.png)

### Mac 版本

DiffusionBee 提供了一个基于 stable diffusion 的应用程序，无需复杂部署，正常安装就可以使用。完全免费

**官网地址：** `https://diffusionbee.com/`

**安装及使用流程参考：**

**硬件要求参考：**

1. 建议：8GB M1 MacBook Air 以上
2. DiffusionBee 适用于基于 Intel 的机器。如果你没有专用的图形芯片，它与 M1 机器相比非常慢。
3. DiffusionBee 需要更高版本的 macOS 12.5.1，推荐使用 Apple 芯片的机器

## 云端部署

1. 免费部署 Stable Diffusion 的云平台飞浆
2. 网页版（免安装，点击即用）

**点击访问：** `https://shorturl.at/kvyNY`

无需魔法，国内可用，注册后即可免费试用，非常适合新手尝鲜（没有任何设备要求和安装过程，登录即可使用）

100% 原版 stable diffusion webUI，目前网上有的插件，官方都会测试优化好后陆续添加进去，比如 openpose3d、mov2mov 等等，好处就是不需要自己部署和维护，随便到哪，甚至用手机和平板就可以畅享 A10 工业显卡级别的作图体验。里面已经有非常多的大模型和 LORA 模型，并且有程序员在进行 BUG 维护。

3. [stable diffusion 腾讯云部署](https://hcb97zmcrv.feishu.cn/docx/KHTedk3vVoP4Xvx0tmXc33aUn2f)

[全网最简单的 stable diffusion 腾讯云部署教程（视频版）](https://www.bilibili.com/video/BV1cV4y1Z7iw/?spm_id_from=333.337.search-card.all.click&vd_source=05604b6063f398633af513a2e9a7c804)

4. Stable Diffusion 阿里云云端部署教程（视频版）

5. [【AI 制图】5 分钟 Colab 云端部署 stable diffusion 教程](https://www.bilibili.com/video/BV15j411c7fE/?spm_id_from=333.337.search-card.all.click&vd_source=05604b6063f398633af513a2e9a7c804)

**附件：** [Colab 快速部署 Stable Diffusion](https://hesn0bipt0.feishu.cn/docx/VrTHdeIm5oKT9AxzI7CcRxqjnTh)

## 版本汉化

### Stable Diffusion WebUI 汉化

1. 安装中文扩展插件：点击 Extensions 选择 Install from URL，输入 `https://github.com/VinsonLaro/stable-diffusion-webui-chinese`，点击 Install，并重启 WebUI。
2. 切换到中文模式：在 Settings 面板中，将 User interface 中的 Localization 设置成 Chinese 中文模式，重启 WebUI 即可切换到中文界面。

![](img/34c1cb0dd6642c9967351a63e2c314e6.png)

---

# 三、使用教程

> 以下文章教程均属于智设团队创作，点击链接蓝色标题可跳转原文

## 第一讲：基本功能及基础操作

### 大模型的切换

首先是这个界面左上角，在这里可以切换你所安装的模型。

![](img/10ef360d3a0b01b4195c9a94f1c885a3.png)

这里我先推荐一下我常用的模型网站：C 站：`https://civitai.com/`

![](img/6a604e073d007293612e7228796fc64e.png)

然后我们在 C 站上下载一些模型，下载需要网络哦。

![](img/0ef58f6568ee3fc1bfb4a036eb99b06f.png)

接下来就是安装了，很简单，把下载的模型放到这个路径文件夹中。然后刷新 web UI 界面（就是 SD 操作界面）

![](img/62a1f713dc91e9c88a6257f6933bc6d4.png)

### 模型的 VAE（Sd 的 VAE）

VAE 的全称是 Variational Auto-Encoder，翻译过来是变分自动编码器，本质上是一种训练模型，Stable Diffusion 里的 VAE 主要是模型作者将训练好的模型“解压”的解码工具。

这里可以切换 VAE。一般情况下我们就选择第一个自动就行了。

![](img/89c3a5f89bf8792e868afdf080734580.png)

在 C 站（stable diffusion）下载模型，在下载页面会有文件显示，如图。

![](img/558a9cbf8f2e6a3c010dcebc258d1efe.png)

VAE 的安装路径看我这里，把下载的文件复制粘贴，然后把 web UI 界面刷新一下（就是 SD 操作界面）

![](img/4d8e03819e98c711bf1d3be09e0ce48f.png)

**言川总结：**

1. 总的来说就是你在 C 站上下载的模型有些内置就有 VAE 的属性（不需要下载 VAE 文件），有些没有内置下载的时候我们就把 VAE 文件下载。（VAE 文件一般大小在几百兆左右）
2. VAE 的作用就是没有加载 VAE 的图片是灰灰的，加载之后就会产生一个润色的效果，色彩更加丰富。

### 功能选项

这里我挑选几个重要的功能讲解下。

**文生图：** 这里与 Midjourney 不同的是，SD 有一个反向关键词（意思是不要图片出现什么），文生图的关键词格式与 Midjourney 基本一致，不知道的朋友可以去看我的往期文章《奶奶看了都会的 Midjourney 入门教程！！》

![](img/a051dac2677f057afc4bb4ff1266c66e.png)

下面这块参数就是设置图片的一些基本参数，分辨率、尺寸等等，具体参数见下文。

![](img/9634c1b75d225dbdfebb1bd2bf0e245d.png)

**图生图：** 这个也很好理解，放参考图然后输入提示词，与其他 AI 绘画软件一致。

![](img/0d104c9819bb696e97171752ca3cc14f.png)

**图片信息：** 意思是你用 SD 画的图，放在这里来，右边会显示这个图片的一些参数，包括关键词。

![](img/6ab32da51269047bb2d5797619406389.png)

然后你可以选择右下角的一些功能，比如“文生图”。

![](img/3883a549e86e9d5f686168bf5ae58b31.png)

**模型合并：** 这个功能很厉害，后面我出一篇关于这个功能的教学，大致就是把多个模型混合起来。

![](img/5d051940e80a6e5d3c5aa95023540367.png)

**训练：** 这个就是训练自己的模型，新手用的比较少，后面可用作工作流的部署。（这个才是我推荐的核心哦）

![](img/9cb59e528627206f369a95654025d834.png)

**设置：** 然后就是设置，大家自己去研究下就好了。

![](img/feaa6f1d264c53c4a311e53f588944ce.png)

**扩展：** 这个就是跟插件扩展相关的了，后面再讲。

![](img/0d5563929b213ec4260b0a1f24203634.png)

### 6.4 提示词和反向提示词

**言川提示：**

提示词内输入的东西就是你想要画的东西，反向提示词内输入的就是你不想要画的东西。

提示框内只能输入英文，所有符号都要使用英文半角，词语之间使用半角逗号隔开。

![](img/13f47b066f8c1927de0db8a1922bca62.png)

一般来说越靠前的词汇权重就会越高，比如我这张图的提示词：

The personification of the Halloween holiday in the form of a cute girl with short hair and a villain's smile, (((cute girl)))cute hats, cute cheeks, unreal engine, highly detailed, artgerm digital illustration, woo tooth, studio ghibli, deviantart, sharp focus, artstation, by Alexei Vinogradov bakery, sweets, emerald eyes.

> 万圣节假期的拟人化形式是一个留着短发和恶棍笑容的可爱女孩，可爱的帽子，可爱的脸颊，虚幻的引擎，高度详细，艺术种子数字插图，woo tooth，吉卜力工作室，deviantart，锐利的焦点，artstation，由 Alexei Vinogradov 面包店，糖果，绿宝石般的眼睛。

第一句关键词词组：万圣节假期的拟人化形式是一个留着短发和恶棍笑容的可爱女孩。那生成的图片主体画面就会是万圣节短发可爱笑容女孩

![](img/9b1bacd4002509ae41273cd305588cff.png)

这里可能有用过 Midjourney 的小伙伴们就有疑问了，(((cute girl))) 是什么意思，为啥有这么多括号，我来为大家解答下，这个是权重调节，类似 Midjourney 的 ::

1. 最直接的权重调节就是调整词语顺序，越靠前权重越大，越靠后权重越低，上面说过。
2. 可以通过下面的语法来对关键词设置权重，一般权重设置在 0.5～2 之间，可以通过选中词汇，按 ctrl+↑↓ 来快速调节权重，每次调节为 0.1，也可以直接输入。

![](img/f9b21f455b9ace51044fc5d273c05ab2.png)

3. 加英文输入的（），一个括号代表这组关键词的权重是 1.1，两个括号是 1.1*1.1 的权重，不要加太多了哈。可以结合第二点固定权重，比如（（（cute girl:1.2））），那这个关键词的权重就很高了。

### 6.5 功能按钮

我们来介绍下右边这几个功能，生成就不说了，输入关键词生成就跑图了。

![](img/c5db98d5a2e71a01b3555195305947a0.png)

第一按钮的意思是读取你上一张图的所有参数信息（包括关键词），比如你画一张图之后，关掉了软件，再次启动点击这个就会把参数复制进来。

第二个按钮就是删除了，清空关键词；这里可以和第一个按钮结合用哦。

第三个按钮就是模型选择管理

![](img/6e45ca969da65cbeef0cc875e4cb92fe.png)

这个按钮就是点击就会出现这些功能，你安装的模型和 `Lora` 都可以在这边调整。

言川小知识：这里可以把你生成的图片添加到模型的封面上，方便你后面看这个模型的效果，看我怎么做：

找到这个模型的名称，然后我这里是跑出来一张图片的，点击这个按钮（`lora` 一样）

![](img/f1827ee96edec6e24e6a6475e873fe84.png)

最终效果：

![](img/4554977dc273db888cf8de500a541d4c.png)

后面两个就是一个提示词模板功能：

![](img/21f9d887037fc721f22a8beebdfe2c95.png)

我们可以先点最后一个按钮，把现在的关键词创建一下，取个名字

![](img/46a775e780b42a9fea4a14957d7ff642.png)

然后在模板风格这里选中我们刚刚创建的模板（可以多选）

![](img/5cd558f9858f37ade0e5635784615b3a.png)

选中之后按一下第四个按钮，我们创建的关键词就被填入进去了，这块收工！

![](img/394502a4af77c7c23313139c380d7bbf.png)

### 6.6 采样步数

一般来说大部分时候采样部署只需要保持在 20～30 之间即可，更低的采样部署可能会导致图片没有计算完全，更高的采样步数的细节收益也并不高，只有非常微弱的证据表明高步数可以小概率修复肢体错误，所以只有想要出一张穷尽细节可能的图的时候才会使用更高的步数。（加大这个会导致出图变慢）

![](img/3dfdf07963b81cd33fc35f0ae0574c9e.png)

### 6.7 采样方法

Stable diffusion webui 是 Stable diffusion 的 GUI，是将 stable diffusion 实现可视化的图像用户操作界面，它本身还集成了很多其它有用的扩展脚本。

SD 的采样方法有很多，大家有兴趣了解的可以去知乎看看@刘登宇的一篇文章《stable diffusion webui 如何工作以及采样方法的对比》。这里我们只介绍两个常用的。

![](img/e0dd1786593ea849fe90d93b1ed23879.png)

这里的采用方式有很多，但是我们常用的就那几个，我们可以去设置里面把不需要用到的关掉，不展示在这个界面中。然后点击右边的重启 UI 就好了。

![](img/f189c920314b62e98983ba9f1d2e41b5.png)

### 6.8 高清修复和图片尺寸参数

前面两个做个简单的介绍，面部修复对写实的风格有点效果，可平铺就不要用了，生成的图片很奇怪，可以自己试试。

![](img/188ee90cb432c23d68bc5dd5a72eced7.png)

我们说说高清修复，高清修复的意思是把基础生成的图片，按照你选择放大的倍率放大到指定分辨率之后再重新绘制图片，受到你重复幅度调节的数值影响。

![](img/4e1555629357e2c09890cb95ced5856c.png)

宽度、高度就是图片的长宽比，很容易理解，生成批次和数量就是几批，一批几张图，配置好的可以直接拉满，增加效率哦。

提示词相关性：图像与你的提示的匹配程度。增加这个值将导致图像更接近你的提示（根据模型），但它也在一定程度上降低了图像质量。可以用更多的采样步骤来抵消。

默认未选中高清修复是这样的，当前图片的参数。

![](img/e65fad746a30cbf08b347820e0091f95.png)

### 6.9 随机种子（seed）

这个功能与 Midjourney 的 Seed 功能一样。

随机数的起点。保持这个值不变，可以多次生成相同（或几乎相同，如果启用了 `xformers`）的图像。没有什么种子天生就比其他的好，但如果你只是稍微改变你的输入参数，以前产生好结果的种子很可能仍然会产生好结果。

![](img/d9e59467b756fd7b6f9ac6ca34a95ab4.png)

输入不同的种子，生成的图片不一样，`seed` 值一样，那生成的图片也是一样。

差异随机种子，需要勾选这个按钮

![](img/87a62d56abaa0a47e799a1ceac96700c.png)

这个功能是固定一个种子，然后调整差异强度来生成图片，因为是固定的 `seed` 值，所以生成的图风格都是类似的。

直接看图吧。

![](img/816279d72eda76de324a986d0f3203f6.png)

![](img/f7b1f340fc11db7142890e975ec6708c.png)

这边的几个功能都很简单，大家尝试下就能理解了，那基础操作我们就先讲到这里，大家赶紧玩起来吧！！

![](img/daff6e5a2e3524217589f4cd7a03b5d8.png)

---

![](img/c2de4e6397e1b4bdd8a031053f264537.png)

> 以下插件教程均为长篇文章，大家需点击链接跳转至原文地址查阅。

## 第二讲：主流的插件介绍（进阶）

姥姥看了都会的 `Controlnet` 插件功能教程

`ControlNet` 模型的工作原理与应用场景（附案例解析）

模型管理插件 `Civitai Helper`

一键生成多种风格图片，自动填充关键词插件 `Dynamic Prompt`

脸部修复及一键生成表情包插件 `After Detailer`

[StableDiffusion 制作中文字效工作流揭秘与画质提升的多种方法](https://mp.weixin.qq.com/s/GNPDAfuFX9jGUKZf65_bXw)

如何在 PS 中使用 SD？一文带你走进 PS 的 AI 创作世界

---

![](img/29e42e3e03913d795ef346eafb2ce883.png)

> 以下商业设计教程均为长篇文章，大家需点击链接跳转至原文地址查阅。

## 第三讲：商业设计实战

[AI 商业教程-儿童节海报丨 Stable diffusion 线稿上色](https://mp.weixin.qq.com/s/oeiKWk3l4wU9iB39MT3d_Q)

StableDiffusion 在多个设计场景中的探索与应用

StableDiffusion 如何实现模特精准换装以及如何替换模特

# 四、绘画提示词

## 提示词网站

### Lexica

**简介：** Lexica 是一个专注于 Stable Diffusion 模型的 prompt 搜索引擎，其搜索方式非常简单，集搜索、绘图、关键词为一体 AI 的绘画工具。除此之外，lexica 还提供了 AI 图像生成功能，用户可以对图片的 prompt 进行重新编辑并生成新的图片，注：每月有 100 张免费可供作画。

**传送门：** `https://lexica.art/`

![](img/2e2c4b19a6680ddfe070f63ce7512c96.png)
