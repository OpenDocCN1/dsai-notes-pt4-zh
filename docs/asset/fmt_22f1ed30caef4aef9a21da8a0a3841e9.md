
## 3.5 种子（seed）`--seed`

`--seed` 接受的整数范围为 `0–4294967295` 且 `--seed` 的值仅影响初始图像网格。如果不指定种子，Midjourney 将使用随机生成的种子编号，每次使用 `Prompt` 关键词时都会生成多种选项。

`--seed` 值创建一个大的随机噪点画面，应用于初始网格中的所有图像。当指定 `--sameseed` 时，初始网格中的所有图像都使用相同的起始噪点画面，并将生成非常相似的生成图像。`--sameseed` 接受整数 `0–4294967295`，且 `--sameseed` 仅与模型版本 `1`、`2`、`3`、`test` 和 `testp` 兼容。所以平时大家在 `v5` 或者 `v5.1` 中是用不到 `--sameseed` 选项。

查找图像的种子编号可以使用 Discord 表情符号中的 ✉️ 信封表情符号做出反应，在 Discord 中找到图像的种子值。此处的操作方式与 `2.16` 中获取图像 ID 一致。

使用 `--seed` 或 `--sameseed` 参数，在提示词末尾添加 `--seed <种子值>` 或 `--sameseed <种子值>`。

![](img/fb56529af3f766458cf6265909fb54c6.png)

## 3.6 停止（stop）`--stop`

使用 `--stop` 参数在流程的中途完成图像。以较早的百分比停止图像生成，会产生更模糊、细节更少的生成结果。

`--stop` 可以接受的取值范围：`10–100`，默认值为 `100`。`--stop` 在放大图像时不起作用。

停止参数的使用效果对比

![](img/eaab66d3f2cd70e33e03114857ee1815.png)

使用 `--stop` 参数，将 `--stop <数值>` 添加到 `Prompt` 关键词的末尾。

![](img/fb76f1c591dc6bfc39f88833d8e75969.png)

## 3.7 平铺（tile）`--tile`

`--tile` 参数生成可用作重复拼贴的图像，用来创建布料、壁纸和纹理的无缝贴图图案。`--tile` 适用于模型版本 `1`、`2`、`3` 和 `5`。`--tile` 只生成一个可重复的图块。

使用 Midjourney V5 模型

提示词：`scribble of moss on rocks --v 5 --tile` 提示词：`watercolor koi --v 5 --tile`

![](img/7a4b2a724ce5c67b1f3e78292e889713.png)

使用平铺参数，直接将 `--tile` 添加到关键词的末尾。

![](img/92e8ae9ffa0c449cfb10f2327a17283e.png)

## 3.8 版本（version）`--version` 或 `--v`

Midjourney 定期发布新模型版本以提高效率、一致性和质量。默认为最新型号，但可以使用 `--version` 或 `--v` 参数或使用 `/settings` 命令并选择型号版本来使用其他型号。不同的模型擅长处理不同类型的图像。`--version` 接受值 `1`、`2`、`3`、`4` 和 `5`，`5.1`，`5.2`。`--version` 可以缩写 `--v`。

您可以使用 `--version` 或 `--v` 参数或使用 `/settings` 命令并选择模型版本来访问更早的 Midjourney 模型。不同的模型擅长处理不同类型的图像。例如：`/imagine prompt vibrant California poppies --v 1`

另外还有一个叫 Niji 的模型。该模型是 Midjourney 和 Spellbrush niji 之间的合作，经过调整可以制作动画和插图风格。该模型对动漫、动漫风格和动漫美学有更多的了解。一般来说，它在动态和动作镜头以及以角色为中心的构图方面表现出色。只需要在提示词的末尾添加 `--niji`。例如：`/imagine prompt vibrant California poppies --niji`

需要注意的是，Niji 不支持 `--stylize` 参数。使用 `/settings` 命令并选择 Style Med 重置为所有 `--niji` 提示的默认样式设置。

使用版本或测试参数的时候，只需要将 `--v 1`、`--v 2`、`--v 3`、`--v 4`、`--v 5`，`--v 5.1`、`--v 5.2`、`--test`、`--testp`、`--test --creative`、`--testp --creative` 或添加 `--niji` 到提示的末尾。

![](img/920b4c2d3343b6654ad72c000045b430.png)

或者从对话框中键入 `/settings` 并选择相应的版本即可。

![](img/f8bdd9312dbac1528fe36149003d2236.png)

## 3.9 视频（video）`--video`

使用该 `--video` 参数创建正在生成的初始图像网格的短片。使用信封表情符号对完成的工作做出反应，Midjourney 将视频链接通过私信发送。

`--video` 仅适用于图像网格，且只适用于模型版本 `1`、`2`、`3`、`test` 和 `testp`。由于视频功能只能适用于较早期版本，生成的图片质量跟新版本没法比，所以很少用到，大家可以根据自己的需求去尝试。

获取视频链接的操作方式与 `2.16` 中获取图像 ID 一致。

提示示例：`/imagine prompt Vibrant California Poppies --video`

① 添加 `--video` 到提示的末尾；

② 任务完成后，单击添加反应；

③ 选择信封表情符号；

![](img/485c7bc1e304d99fefeafd3ede79a16b.png)

或者直接将 `--video` 添加到 `Prompt` 关键词的末尾。

![](img/bde4c05ebf58e368b4cb1e02f8e8a9aa.png)

## 3.10 风格化（stylize）`--stylize` 或 `--s`

Midjourney 机器人经过训练，可以生成更偏向于具有艺术感的色彩、构图和形式的图像。`--stylize` 或 `--s` 参数会影响此训练效果的应用强度。

低风格化参数值生成的图像会更加匹配提示词，但艺术性较弱。高风格化参数值创建的图像非常具有艺术感，但与提示词的匹配度会减弱。`--stylize` 的默认值为 `100`，并且在使用默认模型版本时接受 `0-1000` 的整数值。

不同的 Midjourney 版本模型具有不同的风格化范围。

![](img/436b40b623b2f93ccd765010d6078a59.png)

V5 模型效果

提示词示例：`/imagine colorful risograph of a fig --s 100`

![](img/bce7113b0f3ab13562e3f6cfdde900ad.png)

使用 `stylize` 参数的时候，只需要添加 `--stylize < value >` 或 `--s` 到提示的末尾。

![](img/282b7ae76961588c1b9e1082c1fd71eb.png)

或者从对话框中键入 `/settings` 并选择相应的风格化值。

![](img/2829c6c24047e1695983b993fa50f2a1.png)

## 3.11 重复参数 `--repeat` 或 `--r`

`--repeat` 或 `--r` 参数可以多次运行一个图像生成任务。将 `--repeat` 与其他参数结合使用（例如 `--chaos`）以加快视觉探索的速度。

需要注意的是，`--repeat` 仅适用于标准套餐用户（30 美金每月）和专业套餐用户（60 美金每月）且只能在 `/fast` 快速 GPU 模式下使用。

此外，标准套餐用户（30 美金每月）可以用 `--repeat` 重复生成的任务数量为 `2~10`。至于专业套餐用户（60 美金每月），可以用 `--repeat` 重复生成的任务数量为 `2~40`。

还有一点需要注意的是，在 `--repeat` 任务图片结果上使用 重做按钮只会重新运行一次提示词。

在提示词末尾添加 `--r <N>`，发送之后，Midjourney 会回复一条信息，确认是否需要重复。点击绿色【确认】按钮，等待生成 N 次，点击【否】则会取消这次指令。

![](img/c0c064d88dce9afabb40780c8b44798e.png)

![](img/36c36ed1973b87e04241c1e56da4e808.png)

## 3.12 关键词权重 `::`

关键词权重是以分隔符 `::` 标注，主要有两个功能。一个是让 Midjourney 分别考虑两个或多个单独的关键词概念，另一个是让它了解我们对某个关键词的倾向性。

先来看第一个功能——让 Midjourney 分别考虑两个或多个单独的关键词概念。

`::` 在提示词中添加双冒号向 Midjourney 表明它应该分别考虑提示词的每个部分。类似于拼音输入法中用单引号来隔开两个音节一样，比如 `xian` 和 `xi'an`。

如果还没有看明白，请看下面的示例。正常情况下，`hot dog` 所有单词都被考虑在一起，Midjourney 会根据 `hot dog` 生成美味热狗的图像。如果将提示分成两部分，`hot:: dog` 则将两个概念分开考虑，从而创建温暖的狗的图片。

![](img/f423ed1f77adce1438668a2b9fac8e25.png)

再来看第二个功能——让它了解我们对某个关键词的倾向性。

当使用双冒号 `::` 将提示分成不同的部分时，在双冒号后添加一个数字，以分配提示的该部分的相对重要性。在下面的示例中，提示 `hot:: dog` 生成了一只热狗。将提示更改为使“热”`hot::2 dog` 一词的重要性是“狗”一词的两倍，从而产生了一只非常热的狗的图像！需要注意的是，V1, 2, 3 只接受整数作为权重，V4 可以接受权重的小数位，非指定权重默认为 `1`。

![](img/8daa45c6e818d3fbbf2e79514f4285cd.png)

还有一个有趣的地方是，可给权重添加负数 `::-0.5`，功能与 `--no` 一致。将负权重添加到提示中以删除不需要的元素，但所有权重的总和必须是正数。

![](img/8cce7a17e48dce9e4545d5c82dc65173.png)

提示词权重具体使用方法如下：

1. 默认权重为 `1`，`hot::1 dog::1`；
1. 如果想给某个提示词不一样的权重，比如 A 权重是 B 权重 2 倍，则可以写成 `A::2 B`；
1. 不同提示词之间数字如果同时按比例增大或者缩小，那权重比例不变。比如 `hot::2 dog::2 = hot::4 dog::4`，`hot::1 dog::2 = hot::2 dog::4`；
1. 可给权重添加负数 `::-0.5`，功能与 `--no` 一致。比如 `lots of floweers::1 red::-0.5 = lots of flowers --no red`；
1. 整个描述词出现的权重必须为正，不能为负，所以当画面出现 `::-0.5` 的时候，权重 `1` 也必须写出来。比如 `lots of flowers::1 red::-0.5` 是对的，但 `lots of flowers red:-0.5` 是错的。

## 3.13 垫图权重 `--iw`

在 `2.1` 中提到使用 `/image` 命令可以通过垫图的方式来达到以图生图的效果。

使用 `--iw` 参数可以改变参考图片的权重，图像权重数值可设置为：`0.5`、`0.75`、`1`、`1.25`、`1.5`、`1.75`、`2`，默认值为 `0.25`。数值越高，参考图的影响越大。较高的 `--iw` 值适合对参考图风格及内容比较看重的操作，比如按照客户提供的参考图来生成图片。

提示词示例：`/image prompt flowers.jpg birthday cake --iw .5`

![](img/c7decfee627b97153c2e42c02ee2b68e.png)

使用垫图权限参数的时候，只需要在提示词末尾添加 `--iw <数值>`。

## 3.14 并列提示词（Permutation Prompts）

并列提示词的功能主要是允许使用单个 `/imagine` 命令来快速生成提示词的变体。

通过在提示词中的大括号 `{ }` 内包含以英文逗号 `,` 分隔的选项列表，可使用这些选项的不同组合创建多个版本的提示词，比如 `/imagine prompt a {red, green, yellow} bird` 创建并处理 3 个提示词，分别为：

• `/imagine prompt a red bird`
• `/imagine prompt a green bird`
• `/imagine prompt a yellow bird`

单个提示词中可以在使用多组括号，比如：`/imagine prompt a {red, green} bird in the {jungle, desert}` 会创建并处理 4 个生成任务，分别为：

• `/imagine prompt a red bird in the jungle`
• `/imagine prompt a red bird in the desert`
• `/imagine prompt a green bird in the jungle`
• `/imagine prompt a green bird in the desert`

单个提示词中，括号内的选项集也可以嵌套在其他括号内，比如：`/imagine prompt A {sculpture, painting} of a {seagull {on a pier, on a beach}, poodle {on a sofa, in a truck}}` 会创建并处理 8 个生成任务，分别为：

• `/imagine prompt A sculpture of a seagull on a pier`
• `/imagine prompt A sculpture of a seagull on a beach`
• `/imagine prompt A sculpture of a poodle on a sofa`
• `/imagine prompt A sculpture of a poodle in a truck`
• `/imagine prompt A painting of a seagull on a pier`
• `/imagine prompt A painting of a seagull on a beach`
• `/imagine prompt A painting of a poodle on a sofa`
• `/imagine prompt A painting of a poodle in a truck`

使用并列关键词时，大括号内可以输入文本、图像提示、参数或提示权重、模型版本、尺寸比例等一切 Midjourney 能支持的指令参数。同时允许多个大括号同时出现，这意味着排列组合会增加更多的可能性。

需要注意的是，并列关键词功能仅适用于使用 `Fast` 快速模式下的标准套餐用户（30 美金每月）和专业套餐用户（60 美金每月）专业套餐用户（60 美金每月）。此外，Midjourney 将每个并列关键词的变体作为一个单独的任务处理，每个任务单独消耗 GPU 时长。在开始生成之前在对话框中会收到一条确认消息。

![](img/d245ff35dd06c81b96c1727c20998a8d.png)

并列关键词示例如下，`/imagine prompt a naturalist illustration of a {pineapple, blueberry, rambutan, banana} bird` 将创建并处理 4 组图像。

![](img/073904954dcd42eaed7f1b5b82b2d84d.png)

`/imagine prompt a naturalist illustration of a fruit salad bird --ar {3:2, 1:1, 2:3, 1:2}` 将创建并处理 4 组具有不同宽高比的图像。

![](img/f4139f7603daa558e6894b09d023461f.png)

`/imagine prompt a naturalist illustration of a fruit salad bird --{v 5, niji, test}` 将使用不同的模型版本创建和处理 3 组图像。

![](img/a064a5b2ae051a0ebe64f6bae04de348.png)

# Midjourney 提示词学习

## 4.1 提示词的结构组成

基本提示词：一个基本的提示可以简单到一个单词、短语或表情符号。

![](img/85db78df418282d00416706d37ba2e2c.png)

高级提示词：可以包括一个或多个图像链接、多个文本短语或单词，以及一个或多个后缀参数。

![](img/859fce640f7cc183ba0655724ad0aa57.png)

当我们想让 Midjourney 画一张比较复杂场景的图片时，需要更长更精准的提示词。那如何写更长更精准的提示词呢？其实，写提示词也是有套路和模板的。

关键词的常用结构是：主题内容+环境描述+艺术风格+构图镜头+图像设定。

![](img/b8fe62383ff18d0377688da816d33420.png)

关键词的语法顺序

1. 主题：人、动物、人物、地点、物体等。
1. 媒介：照片、绘画、插图、雕塑、涂鸦、挂毯等。
1. 环境：室内、室外、月球上、纳尼亚、水下、翡翠城等。
1. 照明：柔和、环境、阴天、霓虹灯、工作室灯等。
1. 颜色：充满活力、柔和、明亮、单色、彩色、黑白、柔和等。
1. 情绪：稳重、平静、喧闹、精力充沛等。
