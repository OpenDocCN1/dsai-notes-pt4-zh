
# --weird（非传统美学）

> 来源：[https://my.feishu.cn/docx/L0MLd4VUEoSJD0xpNKvc2RHwn6c](https://my.feishu.cn/docx/L0MLd4VUEoSJD0xpNKvc2RHwn6c)

使用实验 `--weird` 或 `--w` 参数探索非传统美学。此参数会为您生成的图像引入古怪而另类的特质，从而产生独特而出乎意料的结果。

`--weird` 接受值：0–3000。

默认的 `--weird` 值为 `0`。`--weird` 是一项高度实验性的功能，可能会随着时间的推移而改变，与 Midjourney 模型版本 5、5.1、5.2、6、niji 5 和 niji 6。`--weird` 与种子不完全兼容。

---

## 怪异对工作的影响

最佳 `--weird` 值取决于提示，需要进行实验。尝试从较小的值开始，例如 `250` 或 `500`，然后从那里增加/减少。如果您希望一代人具有传统吸引力和怪异性，请尝试将更高的 `--stylize` 值与 `--weird` 混合。尝试从两者的相似值开始。示例 `/imagine prompt cyanotype cat --stylize 250 --weird 250`。

提示示例：`/imagine prompt cyanotype cat --weird 250`

![](img/275c7218e44a0524c1e723545f1cf4a6.png)

提示示例：`/imagine prompt lithograph potato --weird 250`

![](img/0f150c481b023ec0709e5145476da0de.png)

提示示例：`/imagine prompt clockwork chicken --weird 250`

![](img/83bbf20e31c029e771d7f70c40d634be.png)

---

## --weird、--chaos 和 --stylize 之间有什么区别？

`--chaos` 控制初始网格图像彼此之间的差异。

`--stylize` 控制 Midjourney 默认美学的应用强度。

`--weird` 控制图像与之前的 Midjourney 图像相比的不寻常程度。

---

## 如何使用奇怪

### 使用奇怪的参数

在 Discord 或网站的想象栏中，将 `--weird <value>` 或 `--w <value>` 添加到提示的末尾：

![](img/7c61112600057d15e90585ac6665622c.png)

### 网站设置

您可以使用想象栏中的“设置”按钮设置默认的怪异程度：

![](img/ad1d23d6240bcfcf7b38f54699afdb22.png)

向左调整滑块可获得较低的值，向右调整滑块可获得较高的值。

您在此处设置的值将应用于所有提示，除非您在单个提示中使用 `--weird` 或 `--w` 参数指定不同的值。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
