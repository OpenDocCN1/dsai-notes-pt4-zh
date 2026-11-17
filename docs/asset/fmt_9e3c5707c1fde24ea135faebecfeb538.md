
# 4.Stable Diffusion 阿里云云端部署教程（视频版）

[**5.【AI 制图】5 分钟 Colab 云端部署**](https://www.bilibili.com/video/BV15j411c7fE/?spm_id_from=333.337.search-card.all.click&vd_source=05604b6063f398633af513a2e9a7c804) stable diffusion 教程

**附件：**[Colab 快速部署Stable Diffusion](https://hesn0bipt0.feishu.cn/docx/VrTHdeIm5oKT9AxzI7CcRxqjnTh)

## 版本汉化

### Stable Diffusion WebUI 汉化

1. 安装中文扩展插件：点击 `Extensions` 选择 `Install from URL`，输入 `https://github.com/VinsonLaro/stable-diffusion-webui-chinese`，点击 `Install`，并重启 `WebUI`。

2. 切换到中文模式：在 `Settings` 面板中，将 `User interface` 中的 `Localization` 设置成 `Chinese` 中文模式，重启 `WebUI` 即可切换到中文界面。

![](img/8c5171efe50e596d35a91ff4cb060b80.png)

## 三、使用教程

> **以下文章教程均属于智设团队**创作，点击链接蓝色标题可跳转原文

### 第一讲：基本功能及基础操作

#### 1.1 大模型的切换

首先是这个界面左上角，在这里可以切换你所安装的模型

![](img/aa60ae3f0c65aedeeccbcc4a2d764530.png)

这里我先推荐一下我常用的模型网站：C 站：`https://civitai.com/`

![](img/244aaeebda78245137b09ae8ec02e934.png)

然后我们在 C 站上下载一些模型，下载需要网络哦。

![](img/d27cbe9cd33ed260e0ea93ed0d9a5993.png)

接下来就是安装了，很简单，把下载的模型放到这个路径文件夹中。然后刷新 `WebUI` 界面（就是 SD 操作界面）

![](img/c77e207a7823862243399df8f16088b4.png)

#### 1.2 模型的 VAE（Sd 的 VAE）

`VAE` 的全称是 `Variational Auto-Encoder`，翻译过来是变分自动编码器，本质上是一种训练模型，`Stable Diffusion` 里的 `VAE` 主要是模型作者将训练好的模型“解压”的解码工具。

这里可以切换 `VAE`。一般情况下我们就选择第一个自动就行了。

![](img/ffa165d1af3ce88301fe89dacdfe3e67.png)

在 C 站（`stable diffusion`）下载模型，在下载页面会有文件显示，如图。

![](img/6be343c628cf6c9094fc22d6672888cd.png)

`VAE` 的安装路径看我这里，把下载的文件复制粘贴，然后把 `WebUI` 界面刷新一下（就是 SD 操作界面）​

![](img/373c70f158aef7a6c482b50db951cd4f.png)

**言川总结：**

1、总的来说就是你在 C 站上下载的模型有些内置就有 `VAE` 的属性（不需要下载 `VAE` 文件），有些没有内置下载的时候我们就把 `VAE` 文件下载。（`VAE` 文件一般大小在几百兆左右）​

2、`VAE` 的作用就是没有加载 `VAE` 的图片是灰灰的，加载之后就是会产生一个润色的效果，色彩更加丰富。

#### 1.3 功能选项

这里我挑选几个重要的功能讲解下。

**文生图，**这里与 `Midjourney` 不同的是，`SD` 有一个反向关键词（意思是不要图片出现什么），文生图的关键词格式与 `Midjourney` 基本一致，不知道的朋友可以去看我的往期文章《奶奶看了都会的 `Midjourney` 入门教程！！》

![](img/a35aba9e1e3c66a5e9341a8119055663.png)

下面这块参数就是设置图片的一些基本参数，分辨率、尺寸等等，具体参数见下文

![](img/cf2244a29046c97ebedf27640a75c4d1.png)

**图生图，**这个也很好理解，放参考图然后输入提示词，与其他 AI 绘画软件一致。

![](img/731fd0d1e934559f9a07a04b60c109b5.png)

**图片信息，**意思是你用 `SD` 画的图，放在这里来，右边会显示这个图片的一些参数，包括关键词。

![](img/dc94307d56a9b8969714bcbcdec22523.png)

然后你可以选择右下角的一些功能，比如“文生图”。

![](img/17d1a9fd97af23f369fe2e793edb7477.png)

**模型合并，**这个功能很厉害，后面我出一篇关于这个功能的教学，大致就是把多个模型混合起来。

![](img/4f5f609e7eb63e88cc191ca12f97d012.png)

**训练，**这个就是训练自己的模型，新手用的比较少，后面可用作工作流的部署。（这个才是我推荐的核心哦）

![](img/ff81e89d3d92ebaf0fb17e68681963de.png)

**设置，**然后就是设置，大家自己去研究下就好了。

![](img/b761d53bd973679df3dd84126c77e81b.png)

**扩展，**这个就是跟插件扩展相关的了，后面再讲。

![](img/cbd466d861932df61ae6af342afcea32.png)

#### 6.4 提示词和反向提示词

**言川提示：**

提示词内输入的东西就是你想要画的东西，反向提示词内输入的就是你不想要画的东西。

提示框内只能输入英文，所有符号都要使用英文半角，词语之间使用半角逗号隔开。

![](img/40c6321cfc9ddbdacd07bbcf1489805c.png)

一般来说越靠前的词汇权重就会越高，比如我这张图的提示词：

The personification of the Halloween holiday in the form of a cute girl with short hair and a villain's smile, (((cute girl)))cute hats, cute cheeks, unreal engine, highly detailed, artgerm digital illustration, woo tooth, studio ghibli, deviantart, sharp focus, artstation, by Alexei Vinogradov bakery, sweets, emerald eyes.

万圣节假期的拟人化形式是一个留着短发和恶棍笑容的可爱女孩，可爱的帽子，可爱的脸颊，虚幻的引擎，高度详细，艺术种子数字插图，woo tooth，吉卜力工作室，deviantart，锐利的焦点，artstation，由 Alexei Vinogradov 面包店，糖果，绿宝石般的眼睛。

第一句关键词词组：万圣节假期的拟人化形式是一个留着短发和恶棍笑容的可爱女孩。那生成的图片主体画面就会是万圣节短发可爱笑容女孩

![](img/2d6ee267f983b28e8f23e0452299748c.png)

这里可能有用过 `Midjourney` 的小伙伴们就有疑问了，`（（（cute girl））`是什么意思，为啥有这么多括号，我来为大家解答下，这个是权重调节，类似 `Midjourney` 的 `::`

1、最直接的权重调节就是调整词语顺序，越靠前权重越大，越靠后权重越低，上面说过。

2、可以通过下面的语法来对关键词设置权重，可以通过选中词汇，按 `ctrl+↑↓`来快速调节权重，每次调节为 `0.1`，也可以直接输入。

![](img/c509277f25629c53141023e715579d7a.png)

3、加英文输入的（），一个括号代表这组关键词的权重是 `1.1`，两个括号是 `1.1*1.1` 的权重，不要加太多了哈。可以结合第二点固定权重，比如 `（（（cute girl:1.2））`，那这个关键词的权重就很高了。

#### 6.5 功能按钮

我们来介绍下右边这几个功能，生成就不说了，输入关键词生成就跑图了

![](img/3b1eb5cbde6d30f718769227c0759a4e.png)

第一按钮的意思是读取你上一张图的所有参数信息（包括关键词），比如你画一张图之后，关掉了软件，再次启动点击这个就会把参数复制进来。

第二个按钮就是删除了，清空关键词；这里可以和第一个按钮结合用哦

第三个按钮就是模型选择管理

![](img/8c0781a742d6b090f4e70bc4aa92ce34.png)

这个按钮就是点击就会出现这些功能，你安装的模型和 `Lora` 都可以在这边调整。

言川小知识：这里可以把你生成的图片添加到模型的封面上，方便你后面看这个模型的效果，看我怎么做：​

找到这个模型的名称，然后我这里是跑出来一张图片的，点击这个按钮（`lora` 一样）

![](img/6625ac4a44bc2bf7069758d7eca8054c.png)

最终效果：

![](img/601ac87f81461b4ea630b3db0f157b9b.png)

后面两个就是一个提示词模板功能：

![](img/5b28cd14f361324e4198c75301baaa86.png)

我们可以先点最后一个按钮，把现在的关键词创建一下，取个名字

![](img/cbd4066639046c2a8077852d4209b998.png)

然后在模板风格这里选中我们刚刚创建的模板（可以多选）

![](img/473f2768ffe332fb65982f42aebe2ff3.png)

选中之后按一下第四个按钮，我们创建的关键词就被填入进去了，这块收工！

![](img/b969e8ff69f6b9c7ca4ff8282def1ec1.png)

#### 6.6 采样步数

一般来说大部分时候采样部署只需要保持在 `20～30` 之间即可，更低的采样部署可能会导致图片没有计算完全，更高的采样步数的细节收益也并不高，只有非常微弱的证据表明高步数可以小概率修复肢体错误，所以只有想要出一张穷尽细节可能的图的时候才会使用更高的步数。（加大这个会导致出图变慢）

![](img/8e274ab929a44d7c6498a5140336a48b.png)

#### 6.7 采样方法

`Stable diffusion WebUI` 是 `Stable diffusion` 的 `GUI` 是将 `stable diffusion` 实现可视化的图像用户操作界面，它本身还集成了很多其它有用的扩展脚本。

`SD` 的采样方法有很多，大家有兴趣了解的可以去知乎看看@刘登宇的一篇文章《`stable diffusion webui` 如何工作以及采样方法的对比》。这里我们只介绍两个种常用的。

![](img/7015543d91c5cf735fa5858c42da8adc.png)

这里的采用方式有很多，但是我们常用的就那几个，我们可以去设置里面把不需要用到的关掉，不展示在这个界面中。然后点击右边的重启 `UI` 就好了。

![](img/f0bec1a3e51a84150f718f0564eb8076.png)

#### 6.8 高清修复和图片尺寸参数

前面两个做个简单的介绍，面部修复对写实的风格有点效果，可平铺就不要用了，生成的图片很奇怪，可以自己试试。

![](img/ab1253487d15d8e9f050b7f98f57ba95.png)

我们说说高清修复，高清修复的意思是把基础生成的图片，按照你选择放大的倍率放大到指定分辨率之后再重新绘制图片，受到你重复幅度调节的数值影响。

![](img/b3d4e2ef96b6061ad25ff896e41fda54.png)

宽度、高度就是图片的长宽比，很容易理解，生成批次和数量就是几批，一批几张图，配置好的可以直接拉满，增加效率哦。

提示词相关性：图像与你的提示的匹配程度。增加这个值将导致图像更接近你的提示（根据模型），但它也在一定程度上降低了图像质量。可以用更多的采样步骤来抵消。

默认未选中高清修复是这样的，当前图片的参数。

![](img/451107f039a8769e124232ad6aca3891.png)

#### 6.9 随机种子（`seed`）

这个功能与 `Midjourney` 的 `Seed` 功能一样。

随机数的起点。保持这个值不变，可以多次生成相同（或几乎相同，如果启用了 `xformers`）的图像。没有什么种子天生就比其他的好，但如果你只是稍微改变你的输入参数，以前产生好结果的种子很可能仍然会产生好结果。

![](img/8dfbfe1e4071fc43728c60e10ce7aecf.png)

输入不同的种子，生成的图片不一样，`seed` 值一样，那生成的图片也是一样。

差异随机种子，需要勾选这个按钮

![](img/ea9f888301f8499a42565fb580597bf8.png)

这个功能是固定一个种子，然后调整差异强度来生成图片，因为是固定的 `seed` 值，所以生成的图风格都是类似的。

直接看图吧。

![](img/4ec20f5711d1c707ba1dd8f3e732d845.png)

![](img/988de50d3f247612d557061d995b6334.png)

---

![](img/5b3e8ff96b33f2819dba32eeab51a10b.png)

> **以下插件教程均为长篇文章**，大家需点击链接跳转至原文地址查阅。

## 第二讲：主流的插件介绍（进阶）

[姥姥看了都会的 `Controlnet` 插件功能教程](https://keenchase.feishu.cn/wiki/FurMwi0rEimtm5kc7vwcD0SXnOg)

[一键生成多种风格图片，自动填充关键词插件 `Dynamic Prompt`](https://keenchase.feishu.cn/wiki/Jgybw0SCbir6rek8l4dcmP02n8g)

---

![](img/62e08916643f6780b7d42186a374f0dc.png)

> **以下商业设计教程均为长篇文章**，大家需点击链接跳转至原文地址查阅。

## 第三讲：商业设计实战

[`StableDiffusion` 在多个设计场景中的探索与应用](https://keenchase.feishu.cn/wiki/Z4KLwUqNGi9Tlfk3DCEcNIVDnKc)

[`StableDiffusion` 如何实现模特精准换装以及如何替换模特](https://keenchase.feishu.cn/wiki/EcjjwPErhigvP4khOuwcDvFonWd)

## 四、绘画提示词

### 提示词网站

#### Lexica

**简介：**`Lexica` 是一个专注于 `Stable Diffusion` 模型的 `prompt` 搜索引擎，其搜索方式非常简单，集搜索、绘图、关键词为一体 `AI` 的绘画工具。除此之外，`lexica` 还提供了 `AI` 图像生成功能，用户可以对图片的 `prompt` 进行重新编辑并生成新的图片，注：每月有 `100` 张免费可供作画。

**传送门：**`https://lexica.art/`

![](img/12c9595184c14035a88c1944cc17748e.png)

#### Prompthero

**简介：**提示工程排名第一的网站。通过 `Stable Diffusion`、`Midjourney` 等模型搜索数百万 `AI` 艺术图像。

**传送门：**`https://prompthero.com/`

![](img/cf6fcc73f243e97140259258e8c0b2e1.png)

#### PromptoMANIA

**简介：**人性化的提示生成器将为您提供帮助。您也可以尝试使用其他扩散模型，例如 `DALL-E 2`、`Disco Diffusion`、`WOMBO Dream` 或任何扩散模型。

**传送门：**`https://promptomania.com/stable-diffusion-prompt-builder/`

![](img/61e02670bb29487f9f6be63415eb4ef6.png)

### Prompt 中文输入

1. 下载提示词中文扩展插件：`https://github.com/butaixianran/Stable-Diffusion-Webui-Prompt-Translator` ，将项目作为 `zip` 文件下载，解压后放到 `stable-diffusion-webui/extensions` 目录中，重启 `WebUI`

2. 调用百度翻译 API：去 `api.fanyi.baidu.com` 申请一个免费 `API Key`，并将翻译服务开通。在管理控制台的开发者信息页中确认 `APP ID` 和 `密钥`

3. 在 `Stable Diffusion WebUI` 的 `Prompt Translator` 面板中，选择百度翻译引擎，并将申请的 `APP ID` 和 `密钥`填写进去，点击保存

4. 使用：在 `Stable Diffusion WebUI` 页面顶部会出现一个翻译工具栏，我们在提示词输入框中输入中文，点击工具栏中的翻译就能自动把提示词替换成英文

![](img/ae8481fc068cbbc864b7f4541c936587.png)

### 提示词类型

提示词也就是我们对 `AI` 的指令。
