
# Midjourney 参数说明

## 1）升频器选项

### 程序化（stylize）
`--stylize` 或 `--s` 参数会影响 Midjourney 的默认美学风格应用于 Jobs 的强度。

### 聚光灯（uplight）
`--uplight` 选择 U 按钮时使用替代的“轻型”升频器。结果更接近原始网格图像。放大后的图像细节更少，更平滑。

### 乌贝塔（upbeta）
`--upbeta` 选择 U 按钮时使用替代的 beta 升频器。结果更接近原始网格图像。放大后的图像添加的细节明显更少。

### 默认值（模型版本 4）

![](img/74940cd529ff66c448b66e1b30136aa9.png)

### 默认值（模型版本 5）

![](img/c1324e6644d9a7940ca331ecb133c323.png)

> 大于 2:1 的宽高比是实验性的，可能会产生不可预测的结果。

## 2）模型版本参数

### --niji
另一种模型专注于动漫风格的图像；

### --hd
使用早期的替代模型来生成更大、更不一致的图像。该算法可能适用于抽象和风景图像；

### --test
使用 Midjourney 特殊测试模型；

### --testp
使用 Midjourney 特殊的以摄影为重点的测试模型；

### --version / --v
`--version <1, 2, 3, 4, or 5>` 或 `--v <1, 2, 3, 4, or 5>` 使用不同版本的 Midjourney 算法。当前算法 (V4) 是默认设置。

## 3）其他参数

### --creative
修改 `test` 和 `testp` 模型更加多样化和创造性；

### --iw
设置相对于文本权重的图像提示权重。默认值为 `--iw 0.25`；

### --sameseed
种子值创建一个大的随机噪声场，应用于初始网格中的所有图像。当指定 `--sameseed` 时，初始网格中的所有图像都使用相同的起始噪声，并将生成非常相似的生成图像；

### --video
保存正在生成的初始图像网格的进度视频。表情符号使用 ☉️ 对完成的图像网格做出反应，以触发将视频发送到您的直接消息。`--video` 放大图像时不起作用。

## 4）模型版本和参数兼容性

![](img/4a10c4635026babca0b06147313142ca.png)

## 6. 常用后缀参数详解

### 1）Aspect Ratios（纵横比）

`--aspect` 参数 `--ar` 更改生成图像的纵横比。宽高比是图像的宽高比。它通常表示为用冒号分隔的两个数字，例如 `7:4` 或 `4:3`。

正方形图像具有相等的宽度和高度，描述为 `1:1` 的纵横比。图片可以是 `1000px × 1000px`，或者 `1500px × 1500px`，纵横比仍然是 `1:1`。计算机屏幕的比例可能为 `16:10`。宽度是高度的 1.6 倍。所以图像可以是 `1600px × 1000px`、`4000px × 2000px`、`320px x 200px` 等。

默认纵横比为 `1:1`。

`--aspect` 必须使用整数。使用 `139:100` 而不是 `1.39:1`。

纵横比影响生成图像的形状和组成。

放大时，某些宽高比可能会略有变化。

#### 最大纵横比

不同的 Midjourney 版本模型具有不同的最大纵横比。

![](img/152e79274cab9757965fed18a946b960.png)

`--ar` 参数将接受从 `1:1`（正方形）到每个模型的最大纵横比的任何纵横比。但是，在图像生成或放大过程中，最终输出可能会略有修改。示例：提示使用 `--ar 16:9` (`1.78`) 创建具有 `7:4` (`1.75`) 纵横比的图像。

大于 `2:1` 的宽高比是实验性的，可能会产生不可预测的结果。

![](img/e1471a180ac4c294990258025a741afe.png)

提示示例：`imagine/ prompt vibrant california poppies --ar 5:4`

常见的纵横比

`--aspect 1:1` 默认纵横比。

`--aspect 5:4` 常见的框架和打印比例。

`--aspect 3:2` 印刷摄影中常见。

`--aspect 7:4` 靠近高清电视屏幕和智能手机屏幕。

#### 如何更改纵横比

添加 `--aspect :<value>` 或 `--ar :<value>` 到提示的末尾。

![](img/877159e999e40310e4849c1acda1b5f0.png)

### 2）Chaos（混乱）

参数影响初始图像网格 `--chaos` 的 `--c` 变化程度。高 `--chaos` 值将产生更多不寻常和意想不到的结果和组合。较低的 `--chaos` 值具有更可靠、可重复的结果。

`--chaos` 范围值 `0–100`。

默认 `--chaos` 值为 `0`。

chaos 的生成的效果

#### 低值 --chaos

使用较低的 `--chaos` 值或不指定值将生成每次运行作业时略有不同的初始图像网格。

提示示例：`imagine/ prompt watermelon owl hybrid`

![](img/a51f7f4d528868b20eafa4460ea77747.png)

#### 高值 --chaos

`--chaos` 每次运行作业时，使用较高的值将产生更多变化和意外的初始图像网格。

提示示例：`imagine/ prompt watermelon owl hybrid --c 50`

![](img/dfd558503969dae871cf1480e3a45066.png)

#### 非常高的值 --chaos

`--chaos` 每次运行作业时，使用极高的值将产生不同的初始图像网格，并且具有意想不到的构图或艺术媒介。

提示示例：`imagine/ prompt watermelon owl hybrid --c 100`

![](img/7980b0dd44b8fda8ec7269a52173790f.png)

#### 如何改变 Chaos 值

添加 `--chaos <value>` 或 `--c <value>` 到提示的末尾。

![](img/5accf506cd34238a9d28220e3a5fd7f6.png)

### 3）Quality（质量）

参数更改生成图像所花费 `--quality` 的 `--q` 时间。更高质量的设置需要更长的时间来处理并产生更多的细节。更高的值还意味着每个作业使用更多的 GPU 分钟数。质量设置不影响分辨率。

默认 `--quality` 值为 `1`。较高的值会使用更多订阅的 GPU 时间。

`--quality` 接受以下值：默认模型的 `.25`、`.5` 和 `1`。较大的值将向下舍入为 `1`。

`--quality` 仅影响初始图像生成。

`--quality` 适用于模型版本 `1`、`2`、`3`、`4`、`5` 和 `niji`。

质量设置

更高的 `--quality` 设置并不总是更好。有时较低的 `--quality` 设置可以产生更好的结果——这取决于您尝试创建的图像。较低的 `--quality` 设置可能最适合抽象外观。较高的 `--quality` 值可以改善受益于许多细节的建筑图像的外观。选择与您希望创建的图像类型最匹配的设置。

提示示例：`/imagine prompt woodcut birch forest --q .25`

![](img/1fec0fbdcc61bb0047fd28af17c37924.png)

![](img/90ab0d8c503d160d29fb2612d9268ac9.png)

版本质量兼容性

![](img/12c5a717bc64d6708755f9c03ffd371e.png)

#### 使用 --quality 或 --q 参数

添加 `--quality` 或 `--q` 到提示的末尾。

![](img/2ab5fea057638798dd00b1bf640679b5.png)

#### 使用设置命令

从菜单中 `/settings` 选择您的首选值。`quality`

![](img/fa6f3c40246dc97287ebb65537e6a94e.png)

### 4）Seed（种子）

用过 Midjourney 的同学会发现在发送提示词后，MJ 最开始的图像里会有一个非常模糊的噪点团，然后逐渐变得具体清晰，而这个噪点团的起点就是“Seed” 种子编号是为每个图像随机生成的，但可以使用 `--seed` 或 `--sameseed` 参数指定。使用相同的种子编号和提示将产生相似的结束图像。

`--seed` 接受整数 `0–4294967295`。

`--seed` 值仅影响初始图像网格。

`--seed` 值模型版本 `1`、`2`、`3`、`test` 和 `testp` 是不确定的，将产生相似但不相同的图像。在模型版本中使用相同的提示+种子+参数，将产生相同的图像。

seed 参数

如果未指定种子，Midjourney 将使用随机生成的种子编号，每次使用提示时都会生成多种选项。

使用随机种子运行 3 次

提示示例：`/imagine prompt celadon owl pitcher`

![](img/6ed8701e5ed82a54b1e145ad00d6a6fa.png)

使用指定种子运行 3 次

提示示例：`/imagine prompt celadon owl pitcher --seed 123`

![](img/e0c9878d1e34c6ccd8c361ba51406618.png)

同数值参数

`--seed` 值创建一个大的随机噪声场，应用于初始网格中的所有图像。指定时 `--sameseed`，初始网格中的所有图像都使用相同的起始噪声，并将生成非常相似的生成图像。

![](img/4f10617a4436480c8ac1d2146cc05c3f.png)

如何查找工作的种子编号

使用 Discord 表情符号反应，通过对工作使用 ☉️ 信封表情符号做出反应，在不和谐中找到工作的种子编号。

![](img/f95ea6598f30f8bee7f3396f3efed897.png)

如何更改种子编号

使用 `--seed` 或 `--sameseed` 参数，添加 `--seed` 或 `--sameseed` 到提示的末尾。

![](img/f521e12bce17bfac61a17fca462a5967.png)

### 5）Shop（停止）

使用 `--stop` 参数在流程中途完成作业。以较早的百分比停止作业会产生更模糊、更不详细的结果。

`--stop` 接受值：`10–100`。

默认 `--stop` 值为 `100`。

`--stop` 时不起作用。

#### 不同 Shop 的对比

![](img/26d0cd433aaafb04a316d48bdf6b1cb3.png)

shop 和 Beta Upscale

放大时，该 `--stop` 参数不会影响作业。但是，停止会产生更柔和、细节更少的初始图像，这将影响最终放大结果的细节水平。下面的放大图像使用了 Beta Upscaler。

![](img/d0ffa5115eb030216acab5aedee7d5d7.png)

#### 使用 --stop 参数

添加 `--stop <value>` 到提示的末尾。

![](img/2b4313f3ba5a1d39964dbadd0090ab86.png)

### 6）Stylize（程序化）

Midjourney Bot 经过训练可以生成有利于艺术色彩、构图和形式的图像。`--stylize` 或 `--s` 参数影响该训练应用的强度。低程式化值生成的图像与提示非常匹配，但艺术性较差。高程式化值创建的图像非常具有艺术性，但与提示的联系较少。

`--stylize` 的默认值为 `100`，并且在使用默认 `V4` 模型时接受 `0-1000` 的整数值。

不同的 Midjourney 版本模型具有不同的风格化范围。

![](img/f59f2772d18daa33a7a75b673a3180b7.png)

通用风格化设置

V4 模型

提示示例：`/imagine prompt illustrated figs --s 100`

![](img/ee3f30ad33dad32fb8c861734b9d2874.png)

V5 模型

提示示例：`/imagine prompt colorful risograph of a fig --s 100`

![](img/a0ece5e3397c81a470404c44f1c7bb05.png)

#### 使用 stylize 参数

添加 `--stylize <value>` 或 `--s` 到提示的末尾。

![](img/bedb1072d390ede86c40b492217dcec2.png)

#### 使用设置命令

`/settings` 从菜单中键入并选择您喜欢的风格化值。

![](img/51cd6f7ec3fafaaab3df15a32c990ac6.png)

### 7）Tile（平铺）

`--tile` 参数生成可用作重复拼贴的图像，以创建织物、壁纸和纹理的无缝图案。

`--tile` 适用于模型 版本 `1`、`2`、`3`、`5`

`--tile` 只生成一个 tile。使用像这种无缝模式检查器这样的模式制作工具来查看拼贴重复。

Tile 示例

使用 Midjourney 的模型测试/Testp

![](img/dbf155eecdcc178fc2af17bbd4d8efd2.png)

使用 Midjourney 模型 5

![](img/124ebae02136ed18d4de285f5db36f70.png)

#### 如何使用 tile 参数

添加 `--tile` 到提示的末尾。

![](img/905f87fcbf202c2251373c758ecac14d.png)

### 8）Version（版本）

Midjourney 定期发布新模型版本以提高效率、一致性和质量。默认为最新型号，但可以使用 `--version` 或 `--v` 参数或使用 `/settings` 命令并选择型号版本来使用其他型号。不同的模型擅长处理不同类型的图像。

`--version` 接受值 `1`、`2`、`3`、`4` 和 `5`。

`--version` 可以缩写 `--v`

最新模型（V5）

Midjourney V5 模型是最新最先进的模型，于 2023 年 3 月 15 日发布。要使用此模型，请将参数添加 `--v 5` 到提示末尾，或使用 `/settings` 命令并选择 `5️⃣ MJ Version 5`

该模型具有非常高的 Coherency，擅长解释自然语言提示，分辨率更高，并支持高级功能，例如重复模式 `--tile`

![](img/7423aaf50cecdc4d06b921f5e2481337.png)

V4 模型

Midjourney V4 模型是由 Midjourney 设计并在新的 Midjourney AI 超级集群上训练的全新代码库和全新 AI 架构。最新的 Midjourney 模型拥有更多关于生物、地点、物体等的知识。它更擅长正确处理小细节，并且可以处理包含多个角色或对象的复杂提示。第 4 版模型支持图像提示和多提示等高级功能。

该模型具有非常高的 Coherency 并且在 Image Prompts 方面表现出色。

![](img/73f8e58db23703bb4708bb07ac9a2d4f.png)

V4 的 样式 4a、4b 和 4c

Midjourney Model Version 4 具有三种略有不同的“风格”，对模型的风格调整进行了细微调整。通过在 V4 提示末尾添加 `--style 4a`、`--style 4b` 或 `--style 4c` 来试验这些版本。

`--v 4 --style 4c` 是当前默认值，不需要添加到提示末尾。

关于样式 4a 和 4b 的注释

`--style 4a` 且 `--style 4b` 仅支持 `1:1`、`2:3` 和 `3:2` 纵横比。

`--style 4c` 支持高达 `1:2` 或 `2:1` 的纵横比。

![](img/ec10062618fa53110cebe1e6fff9d079.png)

以前的模型

您可以使用 `--version` 或 `--v` 参数或使用 `/settings` 命令并选择模型版本来访问更早的 midjourney 模型。不同的模型擅长处理不同类型的图像。

提示示例：`/imagine prompt vibrant California poppies --v 1`

![](img/e25e2a93c114c56ce51bc5797c9b2f8b.png)

Niji 模型

该模型是 Midjourney 和 Spellbrushniji 之间的合作，经过调整可以制作动画和插图风格。该模型对动漫、动漫风格和动漫美学有更多的了解。一般来说，它在动态和动作镜头以及以角色为中心的构图方面表现出色。`--niji`

提示示例：`/imagine prompt vibrant California poppies --niji`

![](img/976458cedf2f4f6fb5f148011047c77b.png)

--niji 型号 说明

Niji 不支持 `--stylize` 参数。使用 `/settings` 命令并选择 `Style Med` 重置为所有 `--niji` 提示的默认样式设置。Niji 支持多提示或图像提示。

Test 模型

偶尔会临时发布新模型以供社区测试和反馈。目前有两种可用的测试模型：`--test` 和 `--testp`，它们可以与 `--creative` 参数结合使用以获得更多不同的成分。

提示示例：`/imagine prompt vibrant California poppies --testp --creative`

![](img/b049ed3f72bebeba8614eb60927229d0.png)

当前测试模型的注释 --test 和 --testp

测试模型仅支持 `--stylize` `1250–5000` 之间的值。

测试模型不支持多提示或图像提示

测试模型的最大纵横比为 `3:2` 或 `2:3`。

当宽高比为 `1:1` 时，测试模型仅生成两个初始网格图像。

当纵横比不是 `1:1` 时，测试模型只生成一张初始网格图像。

靠近提示前面的词可能比靠近后面的词更重要。

使用版本或测试参数

将 `--v 1`, `--v 2`, `--v 3`, `--v 4`, `--v 4 --style 4a`, `--v4 --style 4b`, `--test`, `--testp`, `--test --creative`, `--testp --creative` 或添加 `--niji` 到提示的末尾。

![](img/4df5dd3779cbb187546d34c1193f3bea.png)

使用设置命令

输入 `/settings` 并从菜单中选择您喜欢的版本。

![](img/8ef89ee687ac20918ef00d83cfc393fd.png)

### 9）Video（视频）
