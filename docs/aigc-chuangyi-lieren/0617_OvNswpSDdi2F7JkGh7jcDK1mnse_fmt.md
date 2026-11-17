
# Midjourney手把手详细入门教程

> 来源：[https://aigcreative.feishu.cn/docx/EgibdYmMFo82BbxJbKccD5vvnrh](https://aigcreative.feishu.cn/docx/EgibdYmMFo82BbxJbKccD5vvnrh)

> 资料来源于互联网，经创意猎人整理优化，侵删。

## 一、基础

### 1.1 使用准备

Midjouney 是一个建立在 Discord上的服务，所以想要使用 Midjourny，就必须要有 Discord 账

#### 1.1.1 注册

进入 discord 官网，点击「在您的浏览器中打开 Discord|按钮，跟随引导完成 Discord 账号的注册。

![](img/48e6e4b03b30bb913ed5cfe763065370.png)

注册完成后，进入 Midjourney 官网，点击 [join the Beta」按钮，使用刚刚注册的 Discord账号加入 Midjouney 官方频道。

![](img/0fc5459c5bb1003c3a7e0e2379512589.png)

![](img/bdebaaaa3b45d28d059970553a3fa054.png)

#### 1.1.2 在 Midjourney 频道中进行绘画

刚刚进入 Midjourney 频道的账号，可以在[newbee-xxx」子频道进行绘画

![](img/aa5802b631f150ed198897518966dc87.png)

方法是先在输入框中输入 `/imagine` ，后按一下空格，会出现 Prompt 填写区域，在 Prompt 区域输入英文 Prompt，回车发送即可等待 Midjourney Bot 返回图片。

![](img/126dc755bca312487d665aaa1411624d.png)

#### 1.1.3 将 Bot 加入自己的频道

官方频道人多且杂，很容易错过 Bot 放返回给你的图片，这时候可以选将 Bot 添加入自己的频道。首先，我们需要创建一个自己的频道，点击左侧导航栏中的「+」号，按照引导完成频道创建的流程。

![](img/b8a3f7a66f9c644ff51ddd3ba66d054f.png)

然后在 Midjourney的[Newbee-xxx」子频道中找到 Midjourney Bot，点击头像，点击「添加至 APP」，依据指引加入刚刚创建的频道。

![](img/30abe9237fc068be7f2110d43409ffdc.png)

![](img/75e5e0322cee19589ee92cf0658717d7.png)

之后就可以在自己的频道中，使用 `/imagine` 命令生成图像了，使用方法与之前一致.。

![](img/cbd543ef3d7a41b5075da5c92dbda4f2.png)

之后就可以在自己的频道中，使用 `/imagine` 命令生成图像了，使用方法与之前一致。

### 1.2 付费方法

#### 1.2.1 订阅计划

想要管理自己的订阅，或者査看订阅权益，只需要向 Midiourney Bot发送 `/Subscribe` 命令，Bot 会生成一个指向当前账号订阅信息的链接，点击跳转至 Midiourney 订阅信息页面。

![](img/e8f5d4e0092e2bdc4ce2aef1e70cba90.png)

还未订阅的用户会打开 Midjourney 订阅档次的页面，我将把,几个不同的订阅等级主要的差异列在下方表格中。

![](img/e1c7c642a50aac8ea98edb55ed386294.png)

![](img/4d630152ad29ae579f5227ae1681ea8d.png)

其中，快速模式(Fast GPU Time)和慢速模式(Relax GPU Time)的区别在于Bot 收到指令到完成图像所用的时间快速模式总共的生成时间将在十几秒每次以内，而慢速模式则会长非常多，会员计划中的快速模式每月时长指的就是每月快速生成占用 GPU的总长度。

选定想要的计划之后，点击对应计划下方[Subscribe」按钮即可进入付款流程，付款方式支持信用卡和支付宝，信用卡可用 Visa、Mastercard 等外币卡或双币卡结算，没有的同学可以选择支付宝。

#### 1.2.2 退订计划

进入 Midjourney 官网 中，点击右下角的「Sign in」按钮，可以进入自己账号的社区和管理。

![](img/83752dfefe0ac0fba76f3a34565e7b6a.png)

点击左下角昵称后的三个小点，进入「Manage Subscription」页面。

![](img/99909f71f1b69c5c8427947276e2922c.png)

在这个页面，你可以进行:

+   查看快速模式时长:Fast hour 下的 included

+   购买更多快速模式时长:「Buy more Fast hours」

+   按钮取消计划:[Cancel Plan」按钮

+   升级、降级计划:[Change Plan」按钮

![](img/e1bcd9b3f166e076ae0c79075286b1a7.png)

### 1.3 基础设置

向 Midjourney Bot 发送 `/Settings` 命令，Bot 会返回 Midjourney 可配置的设置

![](img/559e15885b40dc1ba4476ac5d4ae20a0.png)

我将从上至下进行介绍，同一组的设置会放一起。

#### 1.3.1 模型版本控制

第一行的下拉框中，可以选择 Midjourney 绘图模型的不同版本。各个版本之间绘图倾向和参数数值会略有差异。其中分成 Midjourney Model 和 Niji Model 两大类。

+   Midjourney Model:Midjourney 本家的大模型，风格多变且擅长写实图片、风格化图片的绘制。目前已更新到 V5.2，该版本也是 Midjourney 的默认模型版本

+   Niji Model:Midjourney和 SpellBrush 之间的合作大模型，更偏向于二次元动漫和插图的生成，目前已更新到 V5。

总得来说，版本越高性能越强，画风越多变。

![](img/b58aeb92ce2264b47ae1b6680bd97024.png)

#### 1.3.2 风格化控制

第二行的五个按钮属于风格化控制。在 Midjourney 中，风格化越强代表着成片的艺术感、失控感越强。

+   RAW mode:等于参数 `--style raw`，原始图像模式，减少 A| 自动的风格化处理，风格相对简洁。仅在Midjourney Model V5.1和 V5.2 中可用。

+   Stylize low:等于参数`--stylize 50`，低风格化。

+   Stylize med:等于参数 `--stylize 100`，中风格化，默认风格化等级。

+   Stylize high:等于参数`--stylize 250`，强风格化。。

+   Stylize very high:等于参数`--stylize 750`，超强风格化。

![](img/3578effc99bc69f724f70240e989687e.png)

#### 1.3.3 隐私设置

Public mode (公开模式)下用户哪怕在自己的频道中让 Bot 生成图像，图像依然会公开在 Midiourney 社区。只有Pro Plan 用户可以将此模式关闭，Basic 和 Standard Plan 用户均无法关闭此选项。

![](img/262ed1ec11420cbcfcf3e87cae549dd1.png)

#### 1.3.4 混合设置

混合设置包含三个按钮，Remix mode、High variation mode、Low variation mode，后两者仅在 Remix Mode 开启的情况下起作用。

+   Remix mode:混合模式，允许用户在做V操作(`pscale` 和 `Variantion`)及 Re roll(重绘)操作时重新编.辑 Prompt，关闭该模式则默认使用原始 Prompt 再生成.。

+   High variation mode:高变差模式，仅在 Remix mode 开启时生效，在 Remix 时倾向以更大的变化完成重绘。

+   Low variation mode:低变差模式，仅在 Remix mode 开启时生效，在 Remix 时倾向以更小的变化完成重绘。

![](img/647302a4d3fd10d9145233da11c58b66.png)

#### 1.3.5 固定样式

Sticky Style 允许用户固定上一次个人后缀中的 `--style` 参数，方便用户不必在以后的提示中重复该代码。此功能与 使用 Tune 命令生成不同风格的图片 相关联，可结合参考,

![](img/efd49df0ffd359df037219de737206f9.png)

#### 1.3.6 GPU生成模式

Midjourney 可使用两种不同的 GPU 模式，分别对应着不同的出图速度，即在 Midjourney付费方法 中提及的 Fastmode 和 Relax mode。不同订阅计划下每月可用的Fast 时间不同，请结合订阅详情参看。

+   Fast mode:快速生成模式，消耗订阅中的Fast 时间，每幅图大概,几秒至十几秒左右。

+   Relax mode:慢速生成模式，在队列中等待 GPU 可用时进行生成，不消耗 Fast 时间，平均一幅图 0-10分钟

+   动态变化。

+   Turbo mode:超频模式，使用的依然是 Fast 时间，但速度比, Fast mode 快四倍，耗时计算是 fast mode的两倍。

![](img/5d31c7b2b94f8ff8c21d5f308f25d9b0.png)

#### 1.3.7 重置设置

重置所有设置至默认值。

![](img/c2f1a0161fb0cd4f10d33a93681156eb.png)

### 1.4 命名介绍

Midjourney 使用指令来控制图像生成过[Midjourney常用指令](https://aigcreative.feishu.cn/wiki/HKtVwhbb7ij1ZukAu4FcAxOznvi?from=from_copylink)。指令可以分为以下几类：（亦可见文档： ）

#### 1.4.1 基础指令

+   `/settings`: 查看和修改 Midjourney 设置

+   `/help`: 显示帮助信息

+   `/info`: 显示当前版本信息

+   `/about`: 显示关于 Midjourney 的信息

+   `/quit`: 退出 Midjourney

#### 1.4.2 生成图像指令

+   `/imagine`: 生成图像

+   `/prompt`: 指定图像描述

+   `/seed`: 指定随机种子

+   `/variations`: 生成图像变体

+   `/upscale`: 提高图像分辨率

+   `/style`: 指定图像风格

+   `/quality`: 指定图像质量

+   `/aspect`: 指定图像宽高比

+   `/mask`: 使用遮罩生成图像

#### 1.4.3 高级指令

+   `/blend`: 混合两张图像

+   `/inpaint`: 修复图像

+   `/edit`: 编辑图像

+   `/animate`: 生成动画

+   `/loop`: 生成循环动画

+   `/slerp`: 生成图像插值

+   `/checkpoint`: 保存模型检查点

+   `/restore`: 恢复模型检查点

#### 1.4.4 其他指令

+   `/join`: 加入聊天室

+   `/leave`: 离开聊天室

+   `/mute`: 静音聊天室

+   `/unmute`: 取消静音聊天室

+   `/clear`: 清除聊天记录

#### 1.4.5 以下是一些常用的 Midjourney 指令示例

+   `/imagine a photorealistic portrait of a young woman with long brown hair and green eyes, wearing a red dress`: 生成一张年轻女子半身肖像，长棕色头发，绿色眼睛，穿红色连衣裙

+   `/prompt a landscape painting of a mountain range in the style of Bob Ross`: 生成一幅山脉风景画，风格模仿 Bob Ross

+   `/seed 12345`: 使用随机种子 12345 生成图像

+   `/variations 4`: 生成 4 张图像变体

+   `/upscale 2x`: 将图像分辨率提高 2 倍

+   `/style anime`: 使用动漫风格生成图像

#### 1.4.6 有关 Midjourney 指令的更多信息，请参阅以下资源

+   [Midjourney 官方文档: https://](https://docs.midjourney.com/docs/command-list)docs.midjourney.com/docs/command-list

+   [M](https://tenten.co/learning/midjourney-beginner-prompt/)idjourney 中文教程: https://tenten.co/learning/midjourney-beginner-prompt/

#### 1.4.7 注意

+   Midjourney 指令区分大小写

+   指令可以组合使用

+   指令的具体效果取决于图像描述和设置

### 1.5 Prompt基础结构

Midjourney 中完整的 Prompt 由三部分组成:命令、提示词、参数。 其中命令和提示词是必须要写的，且命令不能打错字母，否则无法呼出 prompt 输入框，而参数则不用，什么都不写会调用 Midjourney 默认的参数设置。

![](img/ac1804c2f413a14d185cfcc49389f1ee.png)

+   命令 Commands:能够后接 Prompt 的命令只有 `/imagine` 一个，这也是最常用的一个命令。更多命令参看Midjourney 命令介绍。

+   提示词 Prompt:向模型描述你所想要的图像时所用的短语、句子和图片链接，必须使用英文，且短句与短句之间须用「;」 隔开。

+   参数 Paraments:以 `--参数类型` `--数值` 为格式的后缀参数，可以控制图像生成过程中的各项参数例如:长宽比、模型版本、风格化等等。更多参数参看 Midjourney Prompt 参数介绍。

+   将以上的内容回车发送给 Midiourney Bot 之后，Bot 会返回一个消息。图像将从模糊状态逐渐变清晰，直到完全完成。

![](img/12fcb7311d68bca2cc42a4c0c580682e.png)

### 1.6 Prompt参数介绍

参数 Paraments，是添加到 Prompt 后的一系列控制代码，用来控制图像生成的系列，例如:长宽比、风格化程度、模型版本等等。

参数的基本格式为在提示词后添加一个空格，并继续输入 `--参数` `--值` ，参数和值之间同样添加一个空格;继续添加则按下空格后继续输入类似的参数格式即可。

![](img/b544ff8299b8a1208dd7dae0c34c02ca.png)

有些参数没有值，只要打上就是开启该选项，例如`--fast`，表示在该此任务中使用快速生成模式。

（亦可见文档：[Midjourney常用参数](https://aigcreative.feishu.cn/wiki/WqZgwaD2ZioXKQkn4p6cGy6cnVc?from=from_copylink))

#### 1.6.1 通用参数

+   `--seed`：指定随机种子，范围为 0 到 4294967295。

+   `--chaos`：控制图像的混乱程度，范围为 0 到 100，数值越高，图像越混乱。

+   `--quality`：控制图像质量，范围为 0.25 到 1，数值越高，图像质量越高，但渲染时间越长。

+   `--aspect`：控制图像宽高比，默认值为 1:1，可以设置为其他比例，例如 16:9 或 4:3。

+   `--style`：控制图像风格，可以指定具体的艺术家或风格名称，例如 "梵高" 或 "蒸汽朋克"。

+   `--no`：排除某些元素，例如 "--no 人脸" 可以生成没有人脸的图像。

#### 1.6.2 高级参数

+   `--iw`：控制图像权重，范围为 0 到 2，数值越高，图像权重越高。

+   `--fast`：使用快速模式生成图像，但质量可能较低。

+   `--repeat`：重复生成图像，可以指定重复次数。

+   `--relax`：使用放松模式生成图像，可以提高图像质量，但渲染时间更长。

+   `--stylize`：控制图像风格化程度，范围为 0 到 1，数值越高，图像风格化程度越高。

+   `--mask`：使用遮罩生成图像，可以指定遮罩图片路径。

#### 1.6.3 其他参数

+   `--prompt`：指定图像描述，这是最重要的参数，可以包含文本、表情符号、特殊符号等。

+   `--v`：指定模型版本，目前最新版本为 v5。

+   `--upscaler`：指定图像放大器，默认值为 "ESRGAN"。

+   `--prompt_weights`：指定文本权重，可以为不同的文本块指定不同的权重。

+   `--cfg_scale`：控制配置文件缩放比例，可以调整图像的整体风格。

#### 1.6.4 以下是一些常用的 Midjourney 参数示例

+   `--seed 12345`: 使用随机种子 12345 生成图像

+   `--chaos 50`: 生成具有中等混乱程度的图像

+   `--quality 1`: 生成最高质量的图像

+   `--aspect 16:9`: 生成宽高比为 16:9 的图像



+   `--style` 梵高：生成梵高风格的图像

+   `--no` 人脸：生成没有人脸的图像

### 1.6.5 有关 Midjourney 参数的更多信息，请参阅以下资源:

+   [Midjourney 官方文档: https://docs.midjourney.com/docs/parameter-list](https://docs.midjourney.com/docs/parameter-list)

+   [Midjourney 中文教程](https://tenten.co/learning/midjourney-beginner-prompt/)

### 1.6.6 注意:

+   Midjourney 参数区分大小写

+   参数可以组合使用

+   参数的具体效果取决于图像描述和设置

## 1.7 Upscale&Variantion

### 1.7.1 基础操作

在完成的任务下方会出现一排按钮。

![](img/d7a95a04f63fe7831ad3b9019d6c12ee.png)

这排按钮主要有三个作用

+   `U`:Upscale，上采样，将该编号的图片从网格中裁切出来，并超采样到高清。

+   `V`:Variantion，变体，使用该编号的图片作为图片 Prompt，加上原始 Prompt 重新开始一个任务

+   `Reroll`:以原始 Prompt 重新开始一次任务。

数字分别对应着上方图片四宫格中的四张图片，编号如下:

![](img/6ccc19324503872463a5a9f817105fd5.png)

开启 Remix Mode 之后，点击 `V` 操作和 `Reroll`，会出现重新填写 Prompt 的输入框，此时 Midjourney 会以原图作为框架，重新生成一份新 Prompt 的图片网格。

![](img/9438c5e25aa78b679af036787e584afa.png)

### 1.7.2 Upscale 的后续操作

Upscale 后的图片还能进行进一步的处理。

![](img/07031a910289ae7fba27222577df7d60.png)

+   `Upscale(2x,4x)`：以放大 2 倍、4 倍为目标继续进行上采样;

+   `Vary (Subtle)`：类似 Low Variation Mode，可以根据该图重新生成低变差的变体,.

+   `Vary (Strong)`：类似 High Variation Mode，可以根据该图重新生成高变差的变体。

+   `Vary(Region)`：针对某个区域进行编辑，并重新生成图像，例如可以删除图中的某个元素.

+   `Zoom Out(2x,1.5x)`：缩放，将镜头远离2倍、1.5倍。

+   `Custom Zoom`：自定义缩放。

+   上下左右：镜头移动，左右上下

![](img/25b38813c05e3c97b0a9e7c63ec22876.png)

# 二、进阶

## 2.1 Prompt参数补充说明

本节将详细介绍一些参数和值对图像生成造成的影响，这些参数在使用 Midjourney 中经常会用到.

### 2.1.1 Quality 质量

`--quality` 或 `--9` 参数并不能增加图片的分辨率，也就是并不会让图片变得更清晰，它优化的是图片的细节细腻程度

+   `--quality` 参数默认值为 1

+   `-quality` 参数只允许0.25、0.5 和1 三个值，更高的值一律视为 1

![](img/8200f2b52a199a7b168aceaf841e3ca5.png)

### 2.1.2 Stylize 风格化

Midjourney Bot 经过训练，可以生成偏向艺术色彩、构图和形式的图像。`--stylize` 或 `--s` 参数会影响这种训练的应用程度。低 Stylize 值生成的图像与提示非常接近，但艺术性较低:高 Stvlize 值产生的图像非常艺术化，但与提示的联系较少。

+   `--stylize` 默认值是 100，可以输入 0-1000 内的整数值。

```
 /imagine Prompt A cute cat with a cute dog; Stroke art -s 0 -seed 108115050
```

![](img/0b62655ac11fe3b2f34f44b887b73c4d.png)

### 2.1.3 Chaos 混乱度

`--chaos` 或 `--c` 参数影响初始图像网格中四幅图像的变差程度。高 `--chaos` 值会产生更多不同的、出平意料的结果和构图。而较低的 `--chaos` 值则会产生更可靠、更可重复的结果

+   `--chaos` 的值默认为 θ，可以从 0-100 中取值。

```
 /imagine prompt a cut cat --c 0 --seed 2713316719
```

![](img/4e52ddb02449b9bfc451b537dd479990.png)

### 2.1.4 Seed 种子

Midjourney Bot 会使用一个种子编号来创建一个视觉噪点--像电视无信号时的雪花噪声一样--作为生成初始图像网格的起点。每个图像的种子数都是随机生成的，但可以用 `--seed` 参数指定。如果使用相同的种子号和提示，就会得到几乎一致的最终图像。

+   `--seed` 可以接受 0-4294967295 之间的整数。

+   `--seed` 值只影响初始图像网格。

你可以在多次不同的任务中使用 `--seed` 数字 来指定任务的种子编号。

```
 /imagine Prompt A cute cat with a cut dog; Stroke art --seed 123
```

![](img/bf88b1cfbf431b30808507745f8320d0.png)

如果想要获得一次任务的种子号，只需要在 Midjourney Bot 的消息上右键--添加反应--选择信封，Bot 就会私信给你该任务的 Seed 号。

如果找不到这个信封，可点击显示更多，在所有反应中搜索「envelope」。

![](img/34e99db1a8ba3ed155f01a057df840a6.png)

![](img/1c67c264ab7d5866697131e5b545b857.png)

### 2.1.5 Wired 怪异化

用实验性的 `--wired` 或 `--w` 参数探索非传统美学。此参数会为您生成的图像引入离奇的内容，从而产生独特和意想不到的结果。

+   `--wired` 默认值为 0，可接受 0-3000 之间的数字

+   `--wired` 是一种高度实验性的功能。可能随着版本的迭代而变更，

+   `--wired` 与种子不完全兼容。

想要获得最好的 `wired` 效果，需要不断地尝试。可以从较小的值例如 250 或 500 开始，然后从这个值向上/向下慢慢探索。如果你想让这一次生成既有传统的吸引力又古怪，就试着把更高的 `--stylize` 值和 `--wired` 混合在一起。

```
 /imagine prompt cyanotype cat -wired 250
```

![](img/68bfa359c353018775128a31f9ca8d63.png)

```
 /imagine prompt clockwork chicken -weird 250
```

![](img/8ba5599933957f69b5cb17369bd78187.png)

## 2.2 使用图像 Prompt

Midjourney 允许你使用图像作为 Prompt 的一部分，用来影响最后产出的样式、构图和色彩，与 `--iw` 参数相结合可以改变图片进行影响的程度。

在 Midjourney 中，图像 Prompt 以 ur 的形式存在，你需要在 Prompt 中填写图片的 ur 才能生效，ur 必须以.png、.gif 或 jpg 等扩展名结尾。添加图像地址后，再添加其他文本和参数，就可以完成 prompt。

![](img/7522138deeb962a07c4503883389962b.png)

+   图像 Prompt 位于文本 Prompt 的前面

+   Prompt 必须至少有两个图像或一个图像加一个文本才能工作。

+   图像 url 必须是指向在线图像的直接链接。

+   您的 url 应以 .png、.gif、.webp、.jpg 或 .jpeg 结尾在大多数浏览器中，右击或长按图像并选择复制图像地址即可获取 url。

+   `/Blend` 命令是针对移动用户优化的简化图像 Prompt 命令.

### 2.2.1 上传图像至 Discord

可以将图像直接上传至 Discord 并获得其 url，操作如下

1.  将图片粘贴到与 Midjourney Bot 的聊天框中，回车发送图片

1.  上传完成之后，点击图片放大，右键选择复制图像地址

![](img/202cd37b1b5c307df7a5daa86f76408c.png)

然后在 `/imagine` 命令中粘贴即可，discord 会自动简化 url 长度，Midjourney 可识别。

![](img/167eb35de158cb5b7575ffb2760f1ae8.png)

### 2.2.2 图像 Prompt 使用示例

单纯的图像 Prompt 会让两张或多张图片等比重的出现在结果中。

![](img/04410177c9f4cbe41b503fd3618b5187.png)

## 2.3 使用多重 Prompt

Midjourney Bot 可以使用 `::` 作为多重提示的分隔符，混合多个概念。使用多重提示符可以为提示词中的概念赋予相对权重，帮助你更好地控制提示词的混合效果。

### 2.3.1 多重提示基本概念

在提示词中添加双冒号 `::` 表示 Midjourney Bot 应单独考虑提示词的每一部分。例如 `space ship` 的提示词，连在-起表示科幻概念的宇宙飞船，但如果在中间添加 `::`-- 即 `space:: ship` --那么 Bot 会分别绘制 `space` 和 `ship`，并将两者混合在一起。(注意双冒号 `::` 后面要加一个空格)

![](img/397bbc8ac62a73cbf5ae56b605066bde.png)

### 2.3.2 提示词权重

在 `::` 后可以紧跟着添加一个数字，来代表左侧提示词的权重。例如 `space: ship` 平等得考虑 `space` 和 `ship` 两个部分，而 `space::2 ship` 则意味着 `space` 比 `ship` 重要两倍。

+   V1、V2、V3 版本的模型只接受整数作为权重,

+   V4、niji V4、niji V5、V5、V5.1 和 V5.2，可接受小数作为权重。

+   没有特别定义权重，默认视为 1，即 `space::2 ship` 等于 `space::2 ship::1`。

![](img/0c6b906ffce2d809ab358185cbb1af9d.png)

需要注意的是，权重仅仅计算前后两个权重的比值，与绝对值无关。`space::2 ship` 等于 `space::4 ship::2` 等于 `space::100 ship::50` 等等。

### 2.3.3 负提示词权重

负权重可以用来在图像中扣除或减少某一提示词的内容。例如 `still life gouache painting: fruit::-.5` 中，水果的权重就被大幅降低了。(`-.5` 即 `-0.5`)

![](img/f8f45e349a0f2055599c25eca7e354ac.png)

## 2.4 使用 /Blend 命令融图

使用 `/blend` 命令可以快速上传 2-5 张图片，然后査看每张图片的概念和美感，将它们合成一张新的图片。

+   `/blend` 与使用 `/imagine` 的纯多图像 Prompt 有相同的效果，但界面经过优化便于在移动设备上使用。

+   `/blend` 最多可使用 5 张图片。要在一个提示符中使用多于 5 张图片，请使用带有 `/imagine` 的图片 Prompt.

+   `/blend` 不能与文本 Prompt 一起使用。要同时使用文本和图像 Prompt，请使用 `/imagine` 命令并使用文本+图像 Prompt。

输入 `/blend` 命令后，Discord 会提示上传两张照片，你可以从硬盘拖放图片，或使用移动设备时从图片库添加图片。

想要上传更多图片，则可以点击 `image2` 右侧「增加 4」，在选项中添加 `image3`、`image4` 和 `image5`.

![](img/1b95c20a4a410287cf7909b7d24b22ea.png)

为了获得最好的效果，生成图像的长宽比最好与上传图片的长宽比保持一致。

![](img/e11c151cdea0e2d74922f9f4271480a5.png)

## 2.5 使用 /Describe 命令获取图片 Prompt

通过 `/describe` 命令，你可以上传一张图片，并根据该图片生成四种可能的 Prompt。

+   `/describe` 生成具有启发性和提示性的 Prompt，它不能用于准确重新生成上载的图像。

+   `/describe` 返回上传图像的长宽比。

在 Discord 的输入框中输入 `/describe` 并按一下空格，机器人会提示你上传一张图片。从本地上载一张图片即可。

![](img/81ca1460f6b4082e41ebc953586f6ddc.png)

上传完图片之后，机器人会返回几条可供参考的 Prompt。

![](img/ecefe3aed8db2059bf34e63631938b51.png)

下方的「1」、「2」、「3」、[4」按钮可以直接根据对应编号的 Prompt 进行一次生成，或点击 [`imagine all`」 按钮全部进行一次生成。

需要注意的是这些 Prompt 仅仅能作为一个参考，你并不能直接使用这些 Prompt 生成一副与原来一模一样的图片。

## 2.6 使用 /Tune 命令生成不同风格的图片

### 2.6.1 如何使用 Styler Tuner?

使用 Style Tuner 个性化 Midjourney 图像的风格。使用 `/tune` 命令生成一系列样例图像，以根据 Prompt 显示不同的视觉风格。之后需要在列表中选择最喜欢的图片，这样你就会收到一个独特的代码，你可以用这个代码来定制未来任务的风格。

+   `/tune` 命令和对应的代码仅可用于 Midjourney Model 5.2.

+   `/tune` 命令仅在 `Fast mode` 下可用。

+   Styler Tuner 生成的 `--style` 参数，与 20-1000 的 `--stylize` 值兼容

### 2.6.2 Step1.输入 Prompt

在输入框中输入 `/tune` 并按一下空格。在 Prompt 中输入一个 Prompt。

![](img/2e6b1ad178171bb6f27f295796737a5a.png)

### 2.6.3 Step2.确认生成参数

回车发送之后，Midjourney Bot 会返回一个表单，你可以在这里选择生成 Style Tuner 的参数。

![](img/efce206bdf71ab91cb3c241ccc636cea.png)

+   `32 Style Directions`:这个选项可以选择一共生成多少对风格供你挑选，32 即生成 32 对共 64 副图像，还可以选择 64 对、128 对，数字越高需要消耗的 `Fast hours` 就越多。

+   `Default Mode`:可选择 `Default mode` 和 `Raw mode`，`Raw mode` 即全部以原始图像模式生成，按需选择即可。

选择完成后点击 [`Submit`」按钮等待 Bot 提交至 Midiourney 服务器。等待一段时间之后，Bot 会返回一个链接，在此链接中你可以挑选喜欢的风格。

![](img/d72cdf255b057dfc59c6f2a077b0d0a7.png)

### 2.6.4 Step3.挑选喜欢的风格

点击进入链接，Midiourney 在此页面中列出了所有生成的 32/64/128 对图像，这些图像并不是每一行都需要挑选一个更喜欢的，Midjourney 在此处的建议是:

+   选择你强烈喜欢的图像

+   其他的都留在中间的空位置。

![](img/8c2cc60abb1b16152974593a2ae833cc.png)

挑选完成之后，复制后方的 `style` 参数和代码，即可在其他图像的生成中添加类似的风格。

![](img/784be7d14e76f18f7ff7a69c53920ef3.png)

```
 /imagine prompt A cute sheep drink wine -style cMb99AKACiwh
```

![](img/391aacd0d64c5df0e9ab80447fa01d14.png)

## 2.7 使用 Remix 模式混合 Prompt

### 2.7.1 Remix 模式使用流程

`Remix mode` 允许你在一次生成之后进行变体时更改提示、参数、模型版本或长宽比。`Remix` 将获取你初始图像的总体构图，并将其用作新任务的一部分。

+   启用 `Remix` 模式后，`Variation` 按钮在使用时会变为绿色，而不是蓝色。

+   在 `Remix` 模式中，你可以更改模型版本。

### 2.7.2 Step1.打开 Remix 模式

在 `/settings` 命令中打开 `Remix mode`，或者使用 `/prefer remix` 命令打开 `remix mode`。

![](img/289487596da3d07d029385140d968c07.png)

### 2.7.3 Step2.在 Variation 时修改或填写新的 Prompt

![](img/3a33f4d394aa93d73c77150a20b672c6.png)

除了完全更改 Prompt，也可以对原本的 Prompt 进行微调，以对图像中的细节进行调整

![](img/e847a1c0d67455fe03500cbc8d13bb9c.png)

## 2.8 使用 Permutation Prompt



## 2.8.1 Permutation Prompt 示例

在提示词中加入一个`{ }`对，并在其中填写并列的形容词或名词，以“， ”分隔。例如：

+   `/imagine Prompt a fblack, yellow} Shiba lnu`

这段 Prompt 就等于将大括号内的内容分别分配给后续的 Prompt，并各进行一次任务，就像乘法分配律一样：

+   `/imagine Prompt a black Shiba inu`

+   `/imagine Prompt a yellow Shiba Inu`

### 1. 文本排列

+   `/imagine prompt a naturalist illustration of a {pineapple, blueberry, rambutan, banana} bird`

会创建四个图像网格任务。

![](img/b5fa2c68c233d15c121787d63b38f9b6.png)

### 2. 参数排列

+   `/imagine prompt a naturalist illustration of a fruit salad bird -ar {3:2,1:1,2:3,1:2}`

将会分别生成四个拥有不同长宽比的图像网格任务。

![](img/e7a6553b3a5895a953c534e168c2837f.png)

### 3. 多重嵌套排列

当 Prompt 中存在两组平行的 Permutation Prompt 时，它们会相互交叉排列，例如：

+   `/imagine prompt a {red, green} bird in the {jungle, desert}`

等于创建了4(2x2)个交叉结合的任务：

+   `/imagine prompt a red bird in the jungle`

+   `/imagine prompt a red bird in the desert`

+   `/imagine prompt a green bird in the jungle`

+   `/imagine prompt a green bird in the desert`

同时，你还可以尝试在一组 Permutation Prompt 中嵌套另一组 Permutation Prompt，例如：

+   `/imagine prompt A {sculpture, painting} of a {seagull fon a pier, on a beach}, poodle {on a sofa, in atruck}}`

等于创建了8 (2x2^2)个排列组合后的任务

+   `/imagine prompt A sculpture of a seagull on a pier`

+   `/imagine prompt A sculpture of a seagull on a beach`

+   `/imagine prompt A sculpture of a poodle on a sofa`

+   `/imagine prompt A sculpture of a poodle in a truck`

+   `/imagine prompt A painting of a seagull on a pier`

+   `/imagine prompt A painting of a seagull on a beach`

+   `/imagine prompt A painting of a poodle on a sofa`

+   `/imagine prompt A painting of a poodle in a truck`

### 4. 排除字符

有时你想输入的并非是三个完全并列的提示词，其中两个实则是同一组描述，这是可以在“”前加上反斜杠“”来排除这一交又分组。例如：

+   `/imagine prompt {red, pastel ,yellow} bird`

只会生成两组图像网格，分别是：

+   `/imagine prompt a red bird`

+   `/imagine prompt a pastel, yellow bird`

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
