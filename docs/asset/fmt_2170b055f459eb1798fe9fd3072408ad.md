
# Midjourney V6 更新风格参考命令 2.0 `--sref`

> 来源：`https://aigcreative.feishu.cn/docx/MdWId3pYoo8OACxrfu7cRlotnef`
>
> 作者：努力的五花肉         👸号：RaDesign
>
> 链接：`https://mp.weixin.qq.com/s/b8EII5PQ3maA7csj5UK09A`

三天前，风格参考命令迎来了又一次更新，这次更新应该更精确地理解风格，而且在图像中不会泄露非风格相关的内容。MJ 还改进了权重参数 `--sw` 的工作方式，使其更有意义，范围也更大。

![](img/530607245ccbbf088ad077b9e1f4521b.png)

## Sref 命令 2.0 更新：什么是风格参考，我该如何使用它。以及初次更新 1.0 详细说明。

![](img/d25879d03b2c3d090c4e5486652658ef.png)

大家好，我们今天发布了“风格参考”的第二版功能。

**什么是风格参考**

风格参考让您能够参考另一张图像的风格，或者在许多图像之间保持一致的风格。它类似于图像提示，但专注于风格。

**我该如何使用它**

要使用风格参考，请在您的提示后面输入 `--sref URL`（URL 为当前图像您复制的链接），并附上一个图像的 URL。要增加风格的影响力，请应用接近 1000 的风格权重 (`--sw`)。要减弱风格的影响，请将权重设置为接近 `--sw 0`（默认为 100）。您也可以添加多个风格参考。

![](img/45e829ef22965c3d29b624dc40162d24.png)

## 初次更新 1.0 说明

2 月 1 日，MJ 初次发布了它们的首个“一致风格”测试算法。它们称这个功能为“风格参考”。它的工作方式类似于图像提示，您可以提供一个或多个描述您想要的一致风格的图像的 URL。

### 如何使用风格参考

在您的提示后面输入 `--sref`，以及一个指向图像的 URL（URL 为当前图像您复制的链接），例如 `--sref url A`（url A 为一条地址链接，比如有两条地址链接为 url A 和 url B）。MJ 的系统将会将图像 URL 视为“风格参考”，并尝试生成与其风格或美学相匹配的内容，这适用于 V6 和 Niji V6（不适用于 V5 等）。

### 高级设置

使用多个图像 URL，例如 `--sref urlA urlB urlC`（同上解释，地址链接 1、地址链接 2、地址链接 3）。设置风格的相对权重，例如 `--sref urlA::2 urlB::3 urlC::5`。通过 `--sw 100` 设置风格化的总体强度（100 是默认值，0 表示关闭，1000 表示最大值）。常规图像提示必须在 `--sref` 之前，例如：

```
/imagine cat ninja ImagePrompt1 ImagePrompt2 --sref stylePrompt1 stylePrompt2
```

### 请注意（2.0 已更新）

MJ 可能会在接下来的几周内更新此功能（可能会更改一些内容，因此在处于 alpha 阶段时请注意）。如果您的提示倾向于照片逼真，而您想要一种与之相冲突的插图风格，您可能仍然需要在提示中添加一些文本来说明。风格参考对图像提示没有直接影响，仅对至少包含一个文本提示的作业产生影响。我们计划稍后添加一个“一致角色”的功能，它与 `--cref` 参数的使用方式相同。

![](img/b34330c3e479c15ab0ecd630d182d7b8.png)

## 详细步骤

`--sref` 参数允许我们使用通过 URL（地址链接）提供的现有图像或图像的大致视觉特征、美学和氛围创建新图像。

**官方详细步骤地址：**`https://discord.com/channels/662267976984297473/1204491188539883571`

![](img/584a9d8cb9e323d0225502c69b035ad4.png)

请参阅以下的 1-2-3 步骤教程，以帮助您开始使用风格参考图像。

**图像地址链接：**`whatnostop_A_textured_impasto_expressive_oil_painting_depicting_d63d1644-49b4-4831-95b1-486d05452313.png`（也就是下方图片）

![](img/fc7288acce803f6454a956feb45083d3.png)

### 1. 确定一个或多个风格参考图像的 URL

这些 URL 以 jpg、gif、webp 或 png 等文件名结尾。

要获取我的风格参考图像的 URL，以便以后继续使用，我将制作一张我想要的风格的图像。您可以使用任何通过我们的审核筛选的图像 URL。注意：与发布的作业一起使用的风格图像将是公开的，请谨慎上传。

所以这是我用来制作风格参考图像的提示：

```
A textured, impasto, expressive oil painting depicting a solitary fox gazing at the full moon from a tree-covered cliff. The sky above is dark and spangled with starlight. The painting is rendered in a mixture of teals and blacks with the fox's coral body in contrast. The mood is suspenseful and mysterious. --v 6.0
```

翻译：一幅质感丰富、厚涂、富有表现力的油画，描绘了一只孤独的狐狸凝视着从树木覆盖的悬崖上的满月。天空上方是黑暗的，布满星光。这幅画用混合的青绿色和黑色呈现，狐狸的珊瑚色身体形成鲜明对比。气氛悬念且神秘。--v 6.0

规则是 URL 必须以 jpg、gif、webp 或 png 等文件名结尾。但是你会看到 @Midjourney Bot 将其缩短为类似于 `https://s.mj.run/9DFZsjKwkyE` 的内容，尽管它在技术上并没有以文件名结尾，但仍然可用。您还会看到 Discord 在图像 URL 的末尾添加噪音。您可以将其留在那里，也可以编辑回 `.png` 文件名，如下所示。

**图像地址链接：**`whatnostop_A_textured_impasto_expressive_oil_painting_depicting_edd4d432-150c-4478-8a25-74681f45d598.png`

![](img/36c6eb70e6276b2b0f3bae9be54be90c.png)

### 2. 将 --sref 和 URL(s) 添加到您的提示的末尾。

在这一步中，我们将以我们的风格参考图像的视觉风格创建一幅新图像。我从上面的狐狸网格中选择了 U4。使用“风格参考”图像 (`--sref`) 的魔力在于，Midjourney 将努力只捕捉参考图像的美学特质，而不是其语义内容。从这个意义上讲，我们可以将 `--sref` 视为一种文本扩展类型的快捷方式。想想创建狐狸图像的提示。由于 `--sref`，对于我的新图像，我将不必重新输入“描绘了一幅质感丰富、厚涂、富有表现力的油画”、“画作呈现了青绿色和黑色的混合”或“气氛悬念且神秘”，因为从某种意义上说，Midjourney 已经为我完成了这些。重要的是，新提示本身没有美学内容。这有助于 sref 的执行！我将使用狐狸图像作为我的 `--sref`，以提供美学内容。

**提示词：**

```
A young man stands at the edge of the forest cliff, looking over the ocean below. --sref https://s.mj.run/9DFZsjKwkyE --v 6.0
```

翻译：一个年轻人站在森林悬崖的边缘，俯瞰下方的海洋。

```
--sref https://s.mj.run/9DFZsjKwkyE --v 6.0
```

然后我们得到了：`whatnostop_A_young_man_stands_at_the_edge_of_the_forest_cliff_l_82470548-cc11-4cdc-abac-bab5091ba111.png`

![](img/aa7d42d983e86bc982883a8aa8cf5395.png)

**侧边栏：**多个 URL。如果您想使用多个风格参考图像，请在 `--sref` 后面放置所有 URL，并用空格分隔，就像这样：

```
A young man stands at the edge of the forest cliff, looking over the ocean below. --sref https://url.com/earth-style.png https://url.com/wind-style.png https://url.com/fire-style.png https://url.com/air-style.png --v 6.0
```

**侧边栏：**具有权重的多个 URL。如果您想要让其中一个风格参考图像比其他图像更加突出，可以给它一个相对于其他 URL 权重的权重：

```
A young man stands at the edge of the forest cliff, looking over the ocean below. --sref https://url.com/earth-style.png::2 https://url.com/wind-style.png::1 --v 6.0
```

在这个例子中，earth-style 的影响力是 wind-style 的两倍。与 Midjourney 中的所有权重一样，精确的数字并不重要，重要的是它们之间的关系。您也可以使用 `::1.0` 和 `::0.5` 或 `::100` 和 `::50`。

### 3. 清理掉任何不需要的细节

请参考下方常见问题。

以下是同时执行这三项操作的示例：

**提示词：**

```
In the bright afternoon, a young man stands at the edge of the forest cliff in the sunlight, looking over the sunlit ocean below:: night::-0.5 --sw 20 --sref https://s.mj.run/9DFZsjKwkyE --v 6.0
```

翻译：在明亮的午后，一个年轻人站在阳光下的森林悬崖边，俯瞰着下方的阳光海洋。

```
night::-0.5 --sw 20 --sref https://s.mj.run/9DFZsjKwkyE --v 6.0
```

这比之前的图像更加明亮。但是，尽管如此，您可能无法精确地控制从风格参考图像带入的所有细节。（最后一个选择是完全使用不同风格的图像参考。你可以根据他们正在做的任何你不喜欢的事情来缩小选择哪种样式的图像参考。）

图像地址链接：`whatnostop_In_the_bright_afternoon_a_young_man_stands_at_the_ed_2df8d431-e112-4751-8d72-50d16f8b4bb8.png`

![](img/2062162f546b3ec3712491fb01160266.png)

![](img/8f32d24b99f6ce8604c5501654584655.png)

## 常见问题

如果我想要的新图像和 sref 图像之间有很大的不同怎么办？它还能起作用吗？

### 1. MJ 给出了共三种办法

1. **修改提示：**· 语义细节太多：编写一个提示，用您认为可取的细节替换不需要的细节。· 风格不够：修改您的提示，使其更符合您追求的风格。如果您想要质感丰富、厚涂、富有表现力的油画但是显示效果不够强烈，可以在提示中加入这些词以与您的风格参考图像保持一致。
2. **更改您的 `--sw`**：从默认值 100 开始：
   - 语义细节太多：将 `--sw` 从默认值 100 降低到不那么显著的值，例如 `--sw 20`。
   - 风格不够：将 `--sw` 从默认值 100 提高到更显著的值，例如 `--sw 800`（最大值为 1000）。
3. **使用小权重来强调或弱化您认为不需要的画布上的元素**。突出与淡化说明地址：`https://discord.com/channels/662267976984297473/1194021959806238821`

在这个例子中，我降低了 `--sw` 值：

**提示词：**

```
A man stands in a parking lot of futuristic cars against a neon cyberpunk cityscape. --sref https://s.mj.run/9DFZsjKwkyE --v 6.0 --sw 22
```

翻译：一个男人站在未来车辆的停车场里，背景是霓虹的赛博朋克城市景观。

```
--sref https://s.mj.run/9DFZsjKwkyE --v 6.0 --sw 22
```

图像地址链接：`whatnostop_A_man_stands_in_a_parking_lot_of_futuristic_cars_aga_545c455e-6226-4011-9957-8f4d2888af63.png`

![](img/1178b5ac4b983885c156bb403830f6b0.png)

### 2. 您可以在使用 Vary Region 期间使用 sref 来增强美学匹配/混合

以下是如何在使用 Vary Region 时使用 `--sref` 图像来帮助加强风格的方法：

- 确保 remix 已打开。放大 - 点击 [Vary Region]。
- 保留提示中确定画布总体构图的部分。尽量保留尽可能多的部分，但不要耗尽内存。如果您的细节没有显示出来，那可能是内存问题。编辑提示以删除一些不必要的细节，以腾出空间添加新的细节。
- 将您的细节添加到提示中。记得用一些详细的描述（至少 5-7 个词）描述它。
- 现在，您可以通过将基本图像用作 sref 来加强您的添加部分的风格（视觉美学）。右键单击放大后的图像中心，从下拉菜单中选择复制链接。将该链接添加为您的 `--sref`。

![](img/16ab40f653544500cf8f9b140a98aa07.png)

### 3. 关于 'sref' 权重，你能告诉我最小/最大权重范围吗？此外，它们必须用整数表示，还是可以使用小数值以获得更轻的效果（例如，.25、.5 等）？

它们的工作原理与 Midjourney 中的其他权重相同。它们只与彼此进行比较。它们是相互关系中的比例，因此权重为 1.0 和权重为 0.5 就相当于权重为 100 和权重为 50。因此没有最小或最大值。它们是相对的数字。它们可以用小数表示，但后端可能会进行四舍五入。（这样用户就可以随意输入任何内容，仍然可以获得大致符合其预期效果的结果，而不会出现错误消息。）

### 4. 在 Midjourney 中使用外部图像有三种方式

1. **图像参考**（有时称为图像提示）大致传输构图、主题和一些风格到您的画布上。
   - 图像参考是提示的前面的 URL。主题（名词）和一些风格被传递到您的画布上，这由提示本身和 `--iw`（图像权重）的设置来帮助，设置值小于 1.0 表示影响较小，最大可设置为 3.0 表示影响较大。
2. **风格参考**大致仅将美学风格传输到您的画布上。
   - 风格参考是包含在 `--sref` 后面的 URL。风格被传输到您的画布上，这由提示本身和 `--sw`（风格权重）的设置来帮助，设置值小于 100 表示影响较小，最大可设置为 1000 表示影响较大。
3. **角色参考**大致将关键角色属性传输到您的画布上，如果在工作流程中引入得很晚，它们将被映射到已经存在于画布上的角色身上，或者创建一个新的角色。
   - 角色参考是包含在 `--cref` 后面的 URL。角色的特征和一些风格被传输到您的画布上。提示可以帮助锚定细节，但提示也可能干扰传输的“纯度”，因此我们必须在之间取得平衡。
   - 我们还可以使用 `--cw`（角色权重），但它并不控制影响，而是控制角色形体的“锁定”程度。身体和面部的锁定值为 100，随着值逐渐降低到 0，身体逐渐解锁，到达 0 时，只有面部被锁定。

![](img/ece77e172145680a0f6d18a4308eae0e.png)

`https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg`
