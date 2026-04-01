# 7000字干货全面解析Midjourney参数和指令

> 来源：[https://xmgtoagi.feishu.cn/docx/T05WdD3PToCvghx8LeEcGVFWnFd](https://xmgtoagi.feishu.cn/docx/T05WdD3PToCvghx8LeEcGVFWnFd)

#### 一、初识 Midjourney

先来看一个案例：

![](img/982ca2f6792cea00409d5ae3c0a8ece0.png)

Prompt：a cute cat, 3d, octane render, blender --ar 16:9 --niji 5 --s 400 --style expressive

其中--ar 16:9 代表着图片的分辨率比例。长宽比为 16:9。

--niji 5 代表着 niji 5 版本模型。

--s 400 代表着艺术化风格化，对于 V5 版本来说，支持 0-1000。

![](img/84019c96e68a72ddd665394e34d675bb.png)

输入指令：/settings 发送后，出现下图设置项。

![](img/f968c84565ba25404df7164e78286363.png)

其中下图的 1-5 分别对应 Midjourney 的版本，MJ version 5.1 为当前最新版本。

![](img/ca6b07b8ba4966003c80aa9ed54921cb.png)

Niji version 4 和 5 代表着动漫风格的模型。

![](img/2c39ab70a5f0e262a333fc2e7a5d2087.png)

**MJ test 和 MJ test photo** 是 Midjourney 的测试模型。

![](img/31b79fb12f3561b0abbdb517b57cef2b.png)

**Style low、med、hight、very higt 分别代表着**艺术风格化程度，从低、中、高、非常高。Reset Settings 为重置设置。

![](img/fa5c00e5a816d1e739847fe0eafdd736.png)

**P**ublic mode 代表着公开模式，生成的图，会出现在其他地方。非 60 美元/月套餐，只能是该模式。

![](img/aca6f27c0b7a91ee15e0ec6c288226e7.png)

**Stealth mode 代表着秘密**模式，仅限 60 美元/月套餐用户使用。

Remix mode 为混音模式。

Fast mode 为快速模式。60 美元/月套餐，每个月有 30 个小时快速出图时间。30 美元/月，每个月有 15 个小时快速出图时间 。10 美元/月，每个月有 200 张快速出图套餐，200 张额度很快会用完，不推荐此套餐。

Relax mode 低速模式，快速模式一分钟左右出图，而低速模式可能需要 10 分钟左右，速度比较慢。

生成的图片下方，有 2 排按钮。

![](img/d6d8dee7d2bb7d58d549b553bf30ff59.png)

U = Upscale to max；V = Make Variations，后面的数字代表上方对应的图片序号。

这里大概说一下每个模型的区别：

v1 版本画的花朵，比较抽象。v2 版本，花朵细节增加。v3 版本，细节相对于 v2 更加丰富，且创意更好。

![](img/2d95a722572c4ddafaf139ee309db0c7.png)

在下图，v4 到 v5 版本可以看出猫咪的细节更加真实。v4 看上去像绘画，v5 就更像照片，Test Photo 模型是 Midjourney 照片测试模型：

![](img/22a8f81e9f3fa53c8b93e263541da2e3.png)

在下图中，从 Niji4 到 5 可以看出，动漫画风更加高级，更加二次元，从卡通走向成熟，Test 模型是测试模型：

![](img/93d0190ff5aee10672b796ce23abce48.png)

在 v4 版本中有 a、b 和 c（默认），

![](img/8dae4f83ef4b099f205f5920afb3bd62.png)

近期推出了 v5.1 模型，我们的测试模式已经没有了，v5.1 的模型相对 v5 效果更强，简单的关键词也可以产生很好的效果，改善边缘细节，并提升了画面清晰度，v5.1 下还有 raw 模式，该模式可以支持长 prompt，更能准确的理解关键词，生成的图片更加准确，如下图：

![](img/60bb90a4edf4f50ea760c92d69839a54.png)

因为遇到过有的小伙伴还不会设置自己的个人服务器，我这里也放一个动画示意，告诉大家如何添加服务器和 Midjourney 机器人：

![](img/799e4a0b5983921a42ad1662a5071f16.png)

添加 niji 5 机器人，如下示意：

![](img/1136e943593028d388f87644e0c95556.png)

使用 niji 5，输入 /settings 回车（注意要选择绿色图标的）。

![](img/8062987182b544aefcf60b2386eab36e.png)

niji 5 里又有 4 种风格：

![](img/7b488bedfafc0cc5c769182140e671d1.png)

分别是默认风格、表现力风格、可爱风格和最新出的景观风格，下面放一下风格之间的区别（因为景观风格是刚出的，这里没有放示意，大家可以自己尝试一下）。

![](img/0ec522be41f74b89408c83283592f8d1.png)

Remix 混音模式

首先打开混音模式，这里选择了一张官方的图，一堆艺术线稿样式的南瓜，然后选择“Make Variations”。

![](img/2052b6f567bb2c17248cc1436967cb67.png)

在弹出窗口中修改或输入新的提示。例如在输入框里输入 pile of cartoon owls（一堆卡通猫头鹰）。

![](img/9c724529493b01a1779af5b8cf052a75.png)

则对应的猫头鹰代替南瓜。

![](img/a25319078f4109e8bbe154805f32040b.png)

#### 二、常用命令拆解

常见的命令有：

--seed：种子值

--q：品质

--c：混乱

--no：负面提示

--iw：权重（0.5-2）

::（多重提示）

-- repeat（重复）

--stop（停止）

--title（无缝贴图：适用于模型版本 1、2、3、5）

--video（过程动画，适用于模型版本 1、2、3、test）

/blend（最多 5 张图片混合）

/describe（以图生文）

--uplight（升频器，放大图片使用，添加适量的细节和纹理，用 v1-v4 的模型时候，或者默认 niji 模型，可以将默认的 256*256 或 512*512，放大为 1024*1024，因为现在我们默认用 v5，已经是 1024*1024 了，所以一般用不到了）

--upbeta（ 同上，同样也是放大，放大后表面更光滑，且不会添加额外的细节。v4 和默认 niji 模型的时候可以放大到 2048*2048，v1-v3 还是 1024*1024，v5 不支持）

--upanime（同上，niji 4 的默认升频器，适合动漫风格的插画，也是可以放大到 1024*1024，适用于 v1-v4 模型，因为现在 niji 5 默认也是 1024*1024，一般也不用到了，三种升频器都是加到关键词最后，之后点击图片下的 u1、u2、u3、u4 进行使用）

--seed（种子值）

虽然我们的出图是随机的，但毕竟是程序化的控制，所以我们每张图片都对应着一个 seed 值，使用相同的 seed 值和关键词将产生几乎一样的图片（实际就是一样的）。使用方式“--seed xxxx ” 如“--seed 12345 ” 。

如下图所示：使用相同 seed 值，生成的两次图片，是同样的结果。

![](img/5f9388c6c6318a2aebcd7d2a30915534.png)

获取 seed 值的方式如下图所示：

第一步：右键-添加反应-显示更多，输入 envelope 选择邮件图标，点击后，在 Midjourney 私聊中获取 seed 值。

![](img/d049672d4a7b49e65afbe3adde9d859d.png)

Quality（品质）

使用方式“--q”或“--Quality” ，这里决定了我们最终生成的图片质量，更高的参数会产生更多的细节，类似我们做三维渲染一样，时间越长，效果会更好，当然同时也会消耗更多的 GPU 分钟数，注意这里的质量设置不会影响图片分辨率。

如下图所示，分别是：--q .25、--q .5 和--q 1。其中--q.5 对应的模式是 Half quality。-q 1 对应的模式是 Base quality。

![](img/148aececf8b27e1ca9acca85eb2aea27.png)

--q .25：最快得到结果。速度提高 4 倍数，GPU 渲染分钟数减少 3/4。

--q.5：减少细节。速度提高两倍，GPU 渲染分钟数减少 1/2。

--q 1：默认设置，细节和速度之间的平衡。

![](img/9c8eb181d4a3ab9a3d74beb8b5f5783a.png)

注意的是 v5 最高支持 --q 1，就算输入--q 2 也会向下兼容 q 1，如果想使用--q 2，可以使用 v1、2 或者 v3 模型。

![](img/3aeff61ca0f7eb60ff003385f41028e0.png)

stylize （艺术风格化)

使用方式“--s”或“--stylize” ，低风格化生成的图像与提示非常匹配，但艺术性较差。高风格化创建的图像非常具有艺术性，创意性更强，但与提示的联系较少。--stylize 的默认值为 100，并且在使用默认 [V4、V5 模型] 时接受 0-1000 的整数值。

如下图所示：随着风格化数值越高，则画面的细节和表现力越强，细节和层级也越复杂。

![](img/2a0de50830198ddf16515880ec066bf9.png)

v5 版本，风格化数值支持范围 0-1000，默认 100。

![](img/3c52430c49ea7e64391460cfa726a9d9.png)

官方标注这里有缺少，最新的 niji 5 中，支持最高到 400 的艺术风格化

#### 三、命令之间的组合运用

chaos (混乱)

使用方式“--c”或“--chaos” ，chaos 的数值影响初始图像，高 chaos 值将产生更多不寻常和意想不到的结果和组合。较低的 chaos 值具有更可靠、可重复的结果，搭配 seed 值使用，将产生相似的图片。

--chaos 数值范围为 0-100，默认值为 0。

如下图所示：混乱值越高，则生成一组图片之间的差异性组合越大，可能会出现新的图片，越低则越接近原图，可以用于生成相似人物，数值可以写 5 以内。

![](img/30e6656959dc85cdf59a21dd917fe4ac.png)

no (负面提示)

使用方式“--no” ，例:--no plants 尝试从图像中移除植物。

例如生成了一个卧室的图。

![](img/dcbaed24d545012ef54bc0a8b2abdfe0.png)

当我不想要床出现在房间里时，则加入 --no bed. 则生成图不会出现床。

![](img/a885cbbb04648ea42bc4cad348c5b378.png)

stop (停止)

使用方式“--stop ” ，--stop 接受值:10-100。默认--stop 值为 100。--stop0 不起作用。

使用--stop 选择在什么进度结束。较小的百分比结束会产生更糊、不详细的结果。

下图中，添加--stop 不同的数值，得到对应进度生成图。当 --stop 100，则是就是 100%生成的正常图。

![](img/7ca0225636eae03712e74cfd4462ae31.png)

repeat (重复)

使用方式“--r”或“--repeat ” 。众所周知，MJ 属于抽卡模式，游戏里大家都知道可以十连抽，那我们的 mj 可以不可以呢？答案是可以的。这里用到的就是重复命令“--repeat”,用这个命令可以一次性生成多次图片，可以加速我们的抽卡环节。

--r 后输入我们需要出图的次数。要注意的是标准版 30 美元/月订阅者: 2–10 ，Pro版60美元/月订阅者:2–40。

![](img/741f39471b6bb587a58adf7fffdf0e66.png)

由于生成图片较多，需要点击 yes 确认；反悔的话，可以点击 cancel all，进行取消。

:: (多重提示)

当我们使用 :: 作为分隔符时，可以分别处理两个或多个单独的关键词， 而且还可以设置每个关键词的比重。0.5 的时候，0 可以省略，写成.5，如“::.5” 。

如果关键词是：hot dog，则出现热狗。

![](img/498cb01e6ca803b752ffe72554e7041a.png)

如果通过::输入，关键词是：hot:: dog (分开联想创作，热和狗) ：

![](img/e9672503ee3e0c6ae08ca82707d5451d.png)

这里是缩写，实际 hot:: dog = hot::1 dog::1，等于 1:1。也可以用“,”号来区分，如 hot,dog，也会分开联想，但还是有些区别。

第二个示意，输入关键词：baby corn（玉米笋）出现对应的玉米笋。

![](img/b49e166fd63471dcd61d3718748ab549.png)

将 baby corn 变为 baby:: corn。baby 和 corn 的权重是 1:1。生成的图如下图所示。

![](img/7a8f1ee5e35f682390f7024cd20a64f4.png)

当 baby::2 corn。即婴儿和玉米的权重是 2:1。则玉米出现的占比更小，如图生成出来的图片出现玉米糊。

![](img/1fc89ddae124fb456cca096ac9d02881.png)

当 baby::100 corn。即婴儿和玉米的权重是 100:1，生成的画面几乎没有玉米的画面。

![](img/66da2626695610d9ec7de91913eed1f1.png)

注意，这里::权重的总和必须是正数,最好大于 0.5,等于 0 的时候，生成的图片会出现与关键词无关

例:baby:: corn = baby::1 corn::1，1+1=2

由此可以得到：

baby:: corn::-2 ❌

baby:: corn::-.5 ✅

baby::2 corn::-2 ❌

baby3:: corn::-2 ❌

baby::1.5 corn::-2 ❌

baby::51 corn::-50 ✅

只是示意，大家平时不要写这么刁钻的，可以写成完整的，如::写成::1，-.5 写成-0.5；

我们用官方示意再解释一下，输入关键词：

vibrant tulip fields （生机勃勃的郁金香花田）得到如下图所示：

![](img/ae7bfd2d71fcb958ae474fab568c4993.png)

这里更改关键词：

vibrant tulip fields:: red::-.5（生机勃勃的郁金香花田,不要红色 ）得到如下图所示：

![](img/aca2ef0faae15a6a770786f56e3d6336.png)

这里“red::-0.5”等同“--no red” 如果“red::2”则“画面全是红色的郁金香”

iw (图像权重，图生图)

使用方式，例“--iw 2” 。

--iw 的数值，会影响我们上传的图像与文本之间的比重； 默认--iw 数值为 1，较高的 --iw 值意味着我们上传的图片对生成的结果，影响更大。 数值范围为 0.5-2。

![](img/b4d6f1d77ca9ad070873554f35e96830.png)

垫一张上面的图，加上一点描述:a cute girl sitting in a fantasy world. 当--iw 1（默认）时如下图所示：

![](img/9ee502b7f4a297026dfae11c40bc59d3.png)

会看到生成的图片动作相近，略有差距

当--iw 2 时。

![](img/dea4c895d5e188636ca2a1a61dbc2612.png)

这里会看到生成的图片动作比较接近。

当--iw 0.5 时候

![](img/d950a5b25929f0507fcfff46e7ba6ed2.png)

此时生成的图片差距比较大。

/describe (图生文)

首先，这里我们找一张模特照片（图片来自于 unsplash）：

![](img/ebcd0425c1dd7cb25fc10d08b144f648.png)

使用方式，输入框中输入“/describe” ，添加图片后，会得到对应的四组关键词。

![](img/32aaa74fa38436ccd0fbb42ee75e0177.png)

选择其中一组关键词生成如下图：

![](img/20ddb7357c182a85cea846e2059ebfcc.png)

配合前面提到的 seed 值，使用相同 seed 值 + chaos,生成微调画面。如下图所示：

![](img/2a67dc084928fe1f0fd0930e9a9ed030.png)

这里要注意的是，如果开始了混音模式，点图片下的 V 键，生成四组图片时，生成的图片效果效果可能不是很好，卡通人物的话不是很明显。

下图左侧图片未开启 remix 模式。右侧是开启了 remix 模式。

![](img/26e0e64a14c4427f0422127351b09741.png)

然后还是选择一组关键词，用图生图的方式生成图片，加上“--iw 2”

原图如下：

![](img/33b9fcdf6e2aae4140a4ef98737901bb.png)

生成图如下：

![](img/1df2c3fbf3a03bd96dba3257c676fec7.png)

网上很多 mj 控图大法，可以控制生成相同角色的教程，实际效果其实不是很准确。

Midjourney 的可控性还没那么强。 无非就是 seed 值，加 iw 权重，垫图，再 chaos 进行微调，但只能有点相似，细看还是有区别的，做不到一模一样（有个插件可以进行换脸，名字叫 InsightFaceSwap）。

但可以使用一些命令，在同一张图中，生成相似的图如：

4 panels withdifferent poses （可以生成 4 个不同表情 ）

![](img/93a3a444d831d002b5235741b76d6fb1.png)

character sheet（可以生成一组相似动漫角色）：

![](img/1d2d77525fabba2879669e10050dba14.png)

three views of an image, generate three views, namely the front view, the side view, and the back view. （三视图，还是需要垫一张三视图，效果才会出来）

/blend (混合)

使用方式，输入框中输入“/blend” 。该命令下，可以上传 2-5 张图像，并将它们合并成一个新的图像。

![](img/7742abb5fb32aaf3b77be99b0d801f6f.png)

例如上传一张空鱼缸图片和一张金鱼，就可以生成一张有鱼的鱼缸。

![](img/4f52bf8f6682b7c3c7de18908573438e.png)

tile (无缝贴图)

使用方式“--tile ” ，可用作重复拼贴的图像，创建壁纸或纹理的无缝图案。 适用于模型版本 v 1、2、3 和 5。

![](img/51145bd393a60bb93b7653415140698e.png)

video (动画)

使用方式“--video” ，用于获取图像生成的过程动画，效果如下

![](img/75bfa8b60f0ba100c5fa764e18cf939e.png)

{} (排列组合)

使用方式，例 {red,yellow} ，可以修改部分关键词的同时，快速创建多次作业 最多可以创建 40 个作业 。

假如我们要生成三张不同颜色的花，正常需要输入三组命令：

a red flower.

a green flower.

a yellow flower.

这里只需要输入：

a {red, green, yellow} flower

则会生成三组图片；也可用于尺寸 ar、模型版本、艺术风格等，如：

--ar {3:4,16:9} --v {3,4,5,niji} --s {50,250,750} --iw {.5,1.5,2}

或者组合使用，如：

Prompt: a {red, green, yellow} flower. --ar {3:4,16:9} --s {50,100}

或 Prompt: A {girl,boy,cat} is standing in the {classroom,bedroom} --ar {3:4,16:9} --s {50,100,800}

如何需要用到 2 个单词来形容一组画面，在排列组合里,可以用反斜杠“\”来分割 ，如下：

Prompt: a {red,Cartoon \, yellow} flower.

则会生成一朵红色花，和一黄色卡通花：

![](img/bde69cd1b9e629bc810f7a09be8aebea.png)

/prefer suffix (默认后缀)

使用方式，输入框输入“/prefer suffix” ，指定文字或命令，添加到关键词末尾，再次输入“/prefer suffix”，取消设置 。可以定义每次出图的后缀。

![](img/1870b7a014255f0b5b20468cf53a2803.png)

如输入关键词，Prompt: a cat. 则会自动加上后缀：

Prompt: a cat, 8k, high detail. --ar 3:4

/settings 可以看到我们设置默认后缀，如下图：

![](img/12135c97ad0bfcff52c1a01826b14dd5.png)

缺点:但不能在关键词后加入任何命令，如“a cat. --s 200”,否则无法出图。

群里看到很多人遇到过，明明关键词没问题，别人都正常出图，就自己不行，这里可能就是自己不小心误操作，设置了默认后缀。

因为我们的 MJ 默认命令只能在最后，如果你加入了“--s 250”，那出来的结果关键词就是 --s 250 8k,high detail,则不能识别。

/prefer option set (预设)

上面的方式局限性比较大，但我们还可以用预设来方便我们生成图片。使用方式，输入框输入“/prefer suffix”

创造一个预设，option 设置名称，如 fg1(风格 1，可以自定义习惯的英文，或者贴合 mj，如 P、K)，value 类型里写--s 250 --ar 3:4。

![](img/79f177c503dac3ec7cc0f5279c4041a0.png)

这样只要后缀输入我们预设，则会自动加上设置的后缀命令，使用方法：

Prompt: a cat --fg1 = Prompt: a cat --s 250 --ar 3:4

可以创建多个预设，省去输入的时间。

注：最多可以设置 20 个预设， /prefer option list 可以查看所有预设。

#### 四、延展

Midjourney 会员购买：

访问官网，或者输入框输入“/info” ，回车后打开得到的链接。点击右侧的 Manage Sub（订阅管理）。

![](img/07a9ec31885931132e293e828d7f7815.png)

**会有三中类型的会员选择，10 美刀可以生成大约 200 张图片，30 美刀可以生成越 1000 张，这**里推荐个人买 30 刀的，公司可以买 60 刀的，可以支持 Relaxed 模式，无限低速模式出图，下图为各类型的区别：

![](img/4804f06e0983d98c39da87209ac4058f.png)

**关于支付方式，之前支持银联的信用卡，目前有的人已经不行了，推荐使用 visa，国内的 **visa 卡就行，有信用卡的小伙伴可以直接再申请一张免年费的单币 visa 卡。

![](img/8c8ad2d9a1d7b1bb28604b61bc1c91fe.png)

**最后给大家推荐几个网站。**

**由**于 Midjourney 生成的尺寸大小有限制，通常默认 1024*1024，或者 2048*2048，图为官方尺寸示意：

![](img/835b253367ea734e57f7822ba5ebb6ab.png)

如果想要图片分辨率变得更大，则可以下面两个网站。

**h**ttps://bigjpg.com/ ：每月免费次数 20 张，支持 png。付费用户可以使用 8 倍和 16 倍放大，最低 500 张每个月。

https://www.upscayl.org/ ：免费、本地，适用于公司设计稿不能对外发的场景。

**素材管理软件，除了我们常用的 Eagle，也可以使用 Billfish（免费）和 Connecter（免费）。其中 C**onnecter 本地目录，不创建文件夹，可预览图片或 3d 文件格式，如 psd、tiff 和 c4d、max 等等。

![](img/f9a046d228ea927399662edb4bf51fa9.png)

**Connecter 也可以快速预览多个文件，如果我们有很多个文件夹，里面有若干个图片文件，**这样就可以用软件，一键预览所有文件，更加方便我们找到素材，我这里做了个示意动画：

![](img/8f8bb2c46f4de9a246302cc8d606f599.png)

**有**一些 logo 或图标，格式是 jpg 或者 png 格式。可以使用 https://vectorizer.ai/ 快速图片转矢量。

**快速抠图网站：**

https://pixian.ai/

https://www.remove.bg/zh

![](img/618a998580f97bfd5c5ed75b21297518.png)