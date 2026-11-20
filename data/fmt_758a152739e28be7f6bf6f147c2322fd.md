
# 参数列表

> 来源：[https://my.feishu.cn/docx/ACOxdmTJ0oJjF5xs0BqcEzqMnbc](https://my.feishu.cn/docx/ACOxdmTJ0oJjF5xs0BqcEzqMnbc)

[参数是](https://docs.midjourney.com/aspect-ratios)添加到提示中的选项，用于更[改图像的](https://docs.midjourney.com/models)生成方式。参数可以更[改图像的纵横比、](https://docs.midjourney.com/upscalers)在 Midjourney模型版本之间切换、更改使用的Upscaler等等。

参数始终添加到提示的末尾。您可以向每个提示添加多个参数。

![](img/f82cfb63a6b9fc4da2342f7b1f64ac7d.png)

使用 Apple 设备？

许多 Apple 设备会自动将双连字符 (`--`) 更改为破折号 (`—`)。Midjourney 可以接受这两种字符！

---

## 基本参数

### [Aspect Ratios](https://docs.midjourney.com/aspect-ratios)

`--aspect`或`--ar`更改某一代的纵横比。

### [Chaos](https://docs.midjourney.com/chaos)

`--chaos <number 0–100>`改变结果的变化程度。值越高，产生的生成结果就越不寻常和出乎意料。

### Character Reference

在提示中使用图像作为字符引用，以在不同情况下创建同一字符的图像。

### [Fast](https://docs.midjourney.com/fast-relax)

`--fast`覆盖当前设置并使用快速模式运行单个作业。

### [Image Weight](https://docs.midjourney.com/image-prompts)

`--iw <0–3>`设置相对于文本权重的图像提示权重。默认值为 1。

### [No](https://docs.midjourney.com/multi-prompts)

`--no`负面提示`--no plants`会尝试从图像中移除植物。

### [Quality](https://docs.midjourney.com/quality)

`--quality <.25, .5, or 1>`，或者`--q <.25, .5, or 1>`您希望花费多少渲染质量时间。默认值为 1。值越高，使用的GPU 分钟数越多；值越低，使用的 GPU 分钟数越少。

### [Random](https://docs.midjourney.com/style-tuner)

`--style random`，将随机的 32 种基本样式 Style Tuner 代码添加到您的提示中。您还可以使用`--style random-16`使用其他长度的 Style Tuner 的随机结果。`--style random-64` `--style random-128`

### [Relax](https://docs.midjourney.com/fast-relax)

`--relax`覆盖当前设置并使用“放松模式”运行单个作业。

### [Repeat](https://docs.midjourney.com/repeat)

`--repeat <1–40>`或`--r <1–40>`从单个提示创建多个作业。`--repeat`对于快速多次重新运行作业很有用。

### [Seed](https://docs.midjourney.com/seeds)

`--seed <integer between 0–4294967295>`Midjourney 机器人使用种子号创建一个视觉噪声场（如电视静态图像），作为生成初始图像网格的起点。种子号是为每个图像随机生成的，但可以使用 `--seed` 或 `--sameseed` 参数指定。使用相同的种子号和提示将产生类似的最终图像。

### [Stop](https://docs.midjourney.com/stop)

`--stop <integer between 10–100>`使用该`--stop`参数在流程中途完成作业。以较早的百分比停止作业可能会导致结果更模糊、更不详细。

### [Style](https://docs.midjourney.com/style)

`--style <raw>`在 Midjourney 模型版本 5.1 和 5.2 之间切换。

`--style <4a, 4b, or 4c>`在 Midjourney 模型版本 4 之间切换。

`--style <cute, expressive, original, or scenic>`在 Niji 模型版本 5 之间切换。

使用`/tune`命令创建 Style Tuner 并生成自定义样式代码。

### [Style Reference](https://docs.midjourney.com/style-reference)

在提示中使用图像作为样式参考，以影响您希望 Midjourney 制作的图像的样式或美感。

### [Stylize](https://docs.midjourney.com/stylize)

`--stylize <number>`或`--s <number>`参数影响 Midjourney 默认美学风格应用于 Jobs 的强度。

### [Tile](https://docs.midjourney.com/tile)

`--tile`参数生成可用作重复图块以创建无缝图案的图像。

### [Turbo](https://docs.midjourney.com/fast-relax)

`--turbo`覆盖当前设置并使用 Turbo 模式运行单个作业。

### [Video](https://docs.midjourney.com/video)

`--video`保存正在生成的初始图像网格的进度视频。表情符号用 ✉️ 对完成的图像网格做出反应，以触发发送到您的直接消息的视频。`--video`在放大图像时不起作用。

### [Weird](https://docs.midjourney.com/weird)

`--weird <number 0–3000>`，或者`--w <number 0–3000>`用实验`--weird`参数探索不寻常的美学。

### 默认值（模型版本 5.2）

+   大于 2:1 的纵横比是实验性的，可能会产生不可预测的结果。

### 默认值（模型版本 6）

---

## 型号 版本 参数

Midjourney 定期发布新模型版本以提高效率、连贯性和质量。不同的模型擅长处理不同类型的图像。

### Niji

`--niji <4, or 5>`专注于动漫风格图像的替代模型。

### V

`--version <1, 2, 3, 4, 5.0, 5.1, 5.2, or 6>`或`--v <1, 2, 3, 4, 5.0, 5.1, 5.2, or 6>`使用不同版本的 Midjourney 算法。

---

## 旧参数

某些参数仅适用于较早的 Midjourney 模型。

请在此处阅读有关旧参数的更多信息。

---

## 兼容性

### 模型版本和参数兼容性

`https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc`
