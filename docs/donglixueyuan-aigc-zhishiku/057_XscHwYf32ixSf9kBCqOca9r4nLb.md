
# 【MJ应用系列课程 第五讲】一文学完 Midjourney AI 的基本操作与高级技巧

> 来源：[https://docs.feishu.cn/docx/TJE2dgigTokAgkxqfBmc0Sennsh](https://docs.feishu.cn/docx/TJE2dgigTokAgkxqfBmc0Sennsh)
> 原文链接：`https://mp.weixin.qq.com/s/Scp0EbVV9K3d7WAoxuJXmg`

在第一章中我们提到：官方文档是最佳的入门教程。因此，在撰写第二章时，感到相当矛盾。无论如何书写，都难以达到官方文档的精确度和及时性。若 Midjourney 的功能有所更新，这里无法实时同步，可能导致学习内容的缺失。再次强烈建议大家直接查阅官方文档（地址：`https://docs.midjourney.com/`）。

本文对 Midjourney 的操作使用方法进行了整理，涵盖了六个方面：注册登录、基本操作、基础指令、指令参数、高级指令和 Prompt 探索。基础指令、指令参数和高级指令这三部分内容主要来自官方文档，我对内容做了适当的整合，方便一文看完，不需要跳来跳去。一些翻译过来还是比较模糊的地方，查阅了一些资料，做了补充和解读。

从今天的课程开始，需要费一番周折的才能继续学下去，如果不具备访问和订阅条件，看个热闹也是可以的。

注：其实这一系列课程，也就是和大家一起看个热闹，长个见识。Midjourney 真正大规模商用起来，用于我们工作之中，还有很远的路要走。

# 注册登录

---

Midjourney 通过 Discord 的机器人来完成作品交付，所以，开始使用 Midjourney 的过程也是使用 Discord 的过程。

## 注册 discord

打开 Midjourney 官网（`https://www.midjourney.com/`），选择 Join the Beta 会跳转到 Midjourney 的 Discord。

![](img/d030a8421c4cc3db776552b2582f5a03.png)

![](img/19f8f4511a6c709e84b0ce51c5454662.png)

已经有 1569 万成员加入 Midjourney 的 Discord 社区了，按照提示完成注册即可。注意：注册后需要激活账户，登录你注册 Discord 时提供的邮箱，点击链接激活即可。使用 Discord 和 Discord 机器人完成注册后，我们重新打开官网，点击 Join the Beta 按钮，跳转到 Midjourney 的官方服务器邀请页，选择加入。加入后，在左侧任选一个新手频道（`newbies-#`）

![](img/98775d17881aee5d21c15f13793e6313.png)

在下方对方框中直接开始输入指令即可开始创作。这个房间是公共频道，大家都在这里创作，信息也是公开的，所以你创作的图像，也会显示在这个房间的信息流上，只有订阅 Midjourney 专业计划（最贵的那个计划），才可以选择隐藏作品。如果你想渲染式的发布创作，可以点击左上角的 Midjourney 私信图标，之后点击 Midjourney bot 机器人开始创作，这里只会显示你的创作信息，当然，这些创作也不是私有的，也会显示到房间的信息流上的。

注册和使用环节，这流程和我们平时使用的大部分互联网产品，不太一样，比较绕。总结一下

1. 先注册 Discord
2. 去邮箱激活账户
3. 加入 Midjourney 服务器
4. 选择新手房间，开始使用

订阅计划由于大量用户涌入，Midjourney 取消了新手用户的 25 张图片的试用额度，所以新加入的用户，要想使用，就需要订阅它的计划套餐。在对话框中，输入 `/subscribe`，回车即可显示你专属的订阅链接。

![](img/8b267c4c593b7154c9c98f907c62c7eb.png)

![](img/d8899b161b9abeef5a277771c9b14cdb.png)

按照提示，订阅即可。Midjourney 的付款比较简单，没有像 ChatGPT 那样拥有变态的访问和支付风控政策，让你有钱花不出去。但是，要想完成订阅支付，还是需要至少有一张国内的 VISA 双币信用卡或者国外的虚拟信用卡的，如果没有的话，可以试试求助万能的淘宝（注意：在淘宝担保交易，不要私下交易，不要被骗了）。

# 基本操作

---

## 图像生成

在机器人对话框，输入指令 `/imagine`，确认，输入描述（Prompt）即可生成图像。

![](img/13e75b309fbd76b8b7b5eca24740b900.png)

Prompt 使用英文，更精准。和人类理解语法、句子结构不同，Midjourney AI 会将这些描述分拆成多个小小的单元（`token`）去理解。所以 Prompt 很短也可以生成精美准确的结果图像，而特别长的 Prompt 并不一定能生成我们想要的结果，Prompt 专注于图像细节本身更重要（见下文）。

![](img/c1803573a651d2292cd762ee3e53a323.png)

## 图像操作

Midjourney AI 根据描述（Prompt）默认生成四格图像，生成图像后，我们可以对图像进行调整。

![](img/77b5e0251a7acecb58a8f2312094e9a6.png)

刷新按钮：可以重新生成新的图像。

`U1` `U2` `U3` `U4`：可以放大指定的图像，并添加细节，生成一幅图像。

`V1` `V2` `V3` `V4`：可以根据指定图像的风格，生成四个新的变体图像。图像位置图示如下。

![](img/b308a5bdf86aafc721bb749d40f41e97.png)

生成最终的图像后，可以进行一些新的操作。

![](img/284dd838b8009cc18116b79ae7122b77.png)

**Make Variations**: 根据此图像风格，创建新的变体图像。

`Beta/Light Upscale Redo`: 更换一个新的 Model 版本重新制作，如果你使用的已经是最新的 Model 5 的话，就不必使用此功能了。

**Web**: 打开此图像在 Midjourney 网站的外部访问地址。

![](img/e635be863ce16337ba34e0eb93e318e6.png)

![](img/98ba440643b0ea1eec2c1c7a8407ac4b.png)

图像生成很简单，下边，让我们了解下 Midjourney 完整的指令、参数，助力我们生成真正想要的图像。

# 基础指令

---

基本指令，只需要文本描述（Text Prompt）

![](img/1f2cb97f278d90bd091a62017ddfcc63.png)

高级的指令，可以包含一个或多个图像 URL（Image Prompts）、多个文本描述（Text Prompt）以及一个或多个参数（Parameters）。

![](img/69ff8e44d34c4c0cc3f839d40d339633.png)

**Image Prompts**：添加图像的 URL，让 AI 根据我们提供的图像，来影响生成图像的结果和样式。就是我们常听到的投喂图像，让 AI 学习。**Prompt Text**：要生成图像的文本描述，我们与 AI 交互的魔法咒语。这里边有许多细节可以学习，下文会逐步讲解。**Parameters**：通过参数来改变图像的生成结果，比如宽高比、指定模型等。下文有详解。指令清单在机器人对话框输入以下指令，可以激活对应的功能。

## `/ask`

询问一些问题，机器人可以提供一些与 Midjourney 相关的问题

## `/blend`

将几张图像融合在一起，最多可以融合 5 张图像。

![](img/f8815e64c8e12b71f4f6c96b1dc74b76.png)

## `/daily_theme`

官方文档对这个命令的解释比较模糊。用蹩脚的英文查阅了一些资料后，了解到付费用户可以加入 Midjourney 的每日挑战频道。这个命令用于开启/关闭官方的每日挑战提醒。

## `/docs`

官方文档的快速访问命令，使用命令后，可以快速选择你想要查阅的文档的链接，方便快速访问文档。

![](img/ba75f9999c15dc599f363abcfe34306b.png)

## `/faq`

快速查阅常见的问题及答案，内容来自 Midjourney Discord 官方服务器上

## `/fast`

切换到快速生成图像模式

## `/help`

获取关于 Midjourney Bot 机器人的有用信息

## `/imagine`

最重要的指令，用于生成图像

## `/info`

查看个人账户信息，包含余额、到期时间等等，同时会显示正在排队或运行中的作业的信息。

![](img/c8d9f7c2ebb8ad68a0e015b0fe4b35a4.png)

## `/stealth`

高级订阅用户专属，切换到隐身模式，在房间信息流上隐藏自己的创作信息。

## `/public`

高级订阅用户专属，切换到公开模式，在房间信息流上显示自己的创作信息。

## `/subscribe`

生成个人专属的订阅链接

## `/settings`

打开设置界面

![](img/29b36961c4c13715d61cfaebc4eb50ea.png)

`MJ Version 1`、`MJ Version 2`、`MJ Version 3`、`MJ Version 4`、`MJ Version 5`：设置 MJ 的模型版本，最新的是 `5.0`

`Niji Model`：基于 MJ 的动漫风格，可以生成动漫风格图像或者插画。

`Test Models`：MJ 官方会不定时的发布一新的模型功能，可以测试使用

`Quality` 和 `Style`：见下方对应的指令

## `/prefer option`

创建或管理自定义选项

## `/prefer option list`

查看自定义选项

## `/show`

使用指定的作业 ID，重新生成新的作业

## `/relax`

切换到 Relax 模式

## `/remix`

切换到 Remix 模式

# 指令参数

---

## Aspect Ratios 纵横比

参数使用：`--aspect` 或 `--ar` 设置图像的纵横比，如：`--aspect 4:3`，必须使用整数，以下是常用的纵横比示例。

## Chaos 混合参数

参数使用：`--chaos <0–100>` 格式：`--chaos 23`，0 到 100 的整数，默认值是 0。随机因子，在文本描述的基础上增加变化，数值越大，越能得到意想不到的结果和结合。

## `--no`

参数使用：`--no` 图像中不包含某些元素

## Quality 参数

参数使用：`--quality <.25, .5, 1, or 2>`，或 `--q <.25, .5, 1, or 2>` 格式：`--quality .5`，图像的生成质量，质量参数越大，生成的图像细节越多，处理的时间也越久（花费的时间余额越多），`5.0` 版本最大可设置参数为 1。

![](img/c668d9f771c43cc9aad45d71a1e55fb8.png)

## Seed 参数

参数使用：`--seed <0–4294967295>` Midjourney 生成的图像都包含一个 `seed` 参数，我们称之为图像种子。如果你想在此图像的基础上做一些优化，把这个参数加入到 prompt，即可生成极度相似的新图像。已生成的图像，怎么找到 `seed` 值呢？方法有些另类。在目标图像右上侧，鼠标放在表情反馈图标上

在弹出的表情框中输入 `envelope`，选中第一个图标即可，机器人稍后会发来这幅图像的标识信息，里边含有 `seed` 值

![](img/9dc3b4bfc9bce22f7d0874cc8f11ec3e.png)

## Stop 参数

参数使用：`--stop <10–100>` 指定图像生成工作在多少进度时停止，比如 `--stop 80`，图像会在生成执行到 80% 的时候，停止并输出生成结果，值越小，图像越模糊。理解起来会有些困难，我看下以下 gif 图片，一幅图像从开始生成到产生结果的过程，图片由模糊变清晰的过程。

![](img/fea646e47d570487bd0ff059b263e955.png)

## Style 参数

参数使用：`--style <4a, 4b or 4c>` `4.0` 模型有三个有些许差异的版本，使用 `--style` 参数，可以指定不同的版本

## Stylize 风格化参数

参数使用：`--stylize`，或 `--s` 用于图像的风格化，用法：`--stylize 500`，值越大，结果图像的风格化越严重，艺术性越强，但是也偏离描述越远。不同版本的默认值和取值范围

## Uplight 参数

参数使用：`--uplight` 选择 `U` 按钮对选中图像进行细节放大处理。`5.0` 后的模型默认生成的就是最大的 `1024*1024` 尺寸的处理，无需设置。

# 高级指令

---

## 图像 prompt

在创作时，可以配合图像来生成更优化的结果，图像可以影响图像生成工作的构图、风格、色彩，可以单独使用，也可以结合文字 prompt 使用。

![](img/b744c9191b41caecf0bd5146c3f39baf.png)

图像参数使用的是 URL，可以直接在 Discord 对话框中上传以获取图像 URL，如下图所示

![](img/c0d7d78689022659c13f3b9880911dc4.png)

官方示例，提供以下五张图片，进行组合作业。

![](img/428affa88b31944445327da78d71d7a1.png)

不同的组合生成不同的图像

![](img/373be5f694443bd95fe5729b9de690b3.png)

![](img/796a25a13eadd99548bb207f0b71d598.png)

三张图像组合

![](img/93ec13801b97c6b23fb10c3e499e9aa8.png)

## 图像权重

使用 `--iw` 参数来设置图像 prompt 在生成作业过程中的作用权重，不同版本模型的图像 prompt 权重默认值和可取值范围。

prompt 示例：`/imagine prompt flowers.jpg birthday cake --iw .5`，不同权重下，Prompt 图像对生成结果的影响。权重值越大，图片 prompt 对生成结果影响越大。

![](img/a87758837a3fae21d0d53c10ed6fbb71.png)

## 排列 Prompts

排列描述，允许在一个 Prompt 中，使用 `{}` 来指定多个不同的 prompt。比如，我们要生成一幅鸟的图像，可以通过排列 prompt 指定不同的颜色，不同的鸟类，一次生成多组不同的图像。指令：`/imagine prompt a naturalist illustration of a {pineapple, blueberry, rambutan, banana} bird` 将生成四组图像

![](img/f296e44519c6f596458aa484bd52b55f.png)

当然也可以设置几幅图像的比例如：`/imagine prompt a naturalist illustration of a fruit salad bird --ar {3:2, 1:1, 2:3, 1:2}` 设置使用模型如：`/imagine prompt a naturalist illustration of a fruit salad bird --{v 5, niji, test}` 到这里，我们基本把常用的指令和参数都介绍完了。指令和参数只是工具，要想生成我们想要的图像，除了想象力外，适合的 prompt 才是真正的魔法咒语，下边我们重点对 prompt 进行一些探索。

# Prompts 探索

---

首先，使用英文 prompt。用最简短的、有意义的汉语字词，将构想打出来，之后使用 Google 翻译、ChatGPT 等工具翻译成英文。当然还有许多方法可以探索。

## 赋予 Prompt 艺术风格

prompt 示例：`/imagine prompt style cat`

![](img/ade3f6a2485337bf1b45f61f315951d5.png)

![](img/05903c924215d78c9b57c22df1447c60.png)

## 赋予 Prompt 带精准描述的艺术风格

prompt 示例：`/imagine prompt <style> sketch of a cat`

![](img/5c7d36a1ce774065a436cb17d403c7ca.png)

## 赋予 Prompt 具体的年代感

同样的文字 prompt，赋予年代描述后，产生的风格，差异是很大的 prompt 示例：`/imagine prompt <decade> cat illustration`

![](img/415520241576281cff13999b3769c450.png)

## 赋予 Prompt 表情/情绪

添加表情说明 prompt 示例：`/imagine prompt <emotion> cat`

![](img/b5588bfbd39c95dd9b785e65081016d5.png)

## 赋予 Prompt 色彩

添加色彩描述 prompt 示例：`/imagine prompt <color word> colored cat`

![](img/9ca561f18e57904f3efd2add470cfcd7.png)

## 赋予 Prompt 周边环境

prompt 示例：`/imagine prompt <location> cat`

![](img/1196d1536c9d56ea5544bf0f9dccc10a.png)

以上这些是官方文档中已经给出的 Prompt 修饰，其实 Midjourney 对 prompt 的理解已经足够强大了。经过社区网友的实践，以下这些专业的 Prompt 词语也可以极大的影响创作结果。

## 媒介 prompts

## 视角 prompts

## 场景 prompts

## 材质 prompts

## 风格 prompts

## 精度 Prompts

更多查看网友收集文档：



`https://docs.qq.com/sheet/DTHRpUkRZeEpmSEVn?tab=zxwhq3OK`，Midjourney 指令参数和 Prompt 就先整理到这里。下章起，我们就开始使用这些基本功，去创作一些实用的商品作品，再尝试挑战绘制庄颜，敬请期待！（完）

## 文章参考资料

- `https://docs.midjourney.com/`
- `https://www.uisdc.com/midjourney-5`
- `https://learningprompt.wiki/docs/midjourney-learning-path`
- `https://aituts.com/midjourney-v5/`
