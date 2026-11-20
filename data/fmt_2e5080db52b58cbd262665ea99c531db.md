
# --sref（风格参考）

> 来源：[https://my.feishu.cn/docx/TiOqdE1dloiRkfx6hd1c7Wtgnnf](https://my.feishu.cn/docx/TiOqdE1dloiRkfx6hd1c7Wtgnnf)

您可以在提示中使用图像作为样式参考，以影响您希望 Midjourney 制作的图像的样式或美感。

要向提示添加样式引用，请使用 `--sref` 参数和在线存储图像的网址 (`URL`)：`--sref URL`

![](img/d991aa2035c46e7c27a94770a36af0f4.png)

样式引用仅适用于包含文本的提示 - 它们不能与仅有图像的提示一起使用。

+ 您还可以使用提示文本来强化您的风格参考，并且可能需要使用棘手的风格。
+ 任何常规图像提示都必须在 `--sref`
+ 您可以使用多种样式引用：`--sref URL1 URL2 URL3`
+ 您还可以使用[`--sref random`](https://docs.midjourney.com/v1/docs/style-reference#random-style-references)随机样式来应用到您的图像。
+ `--sref`可与 Midjourney 和 Niji Version 6 一起使用

## 隐私声明

+ 使用 Midjourney Bot 在您的直接消息中上传图像，以防止其他服务器用户看到图像。
+ [除非您以隐身模式](https://docs.midjourney.com/v1/docs/stealth)生成，否则图像提示和参考资料在 Midjourney 网站上可见。

请参阅[图像提示](https://docs.midjourney.com/v1/docs/image-prompts)页面以获取有关在 Discord [提示中使用图像 URL](https://docs.midjourney.com/v1/docs/using-images) 的帮助，或参阅网页上的使用图像引用页面以获取有关在 midjourney.com 上使用各种图像引用的帮助。

## 示例

![](img/5524514ba3086359d6517b6e27835ca8.png)

---

## 样式权重参数

使用样式权重参数 `--sw` 来设置风格化的强度。`--sw` 接受从 0 到 1000 的值 `--sw 100` 是默认值。

提示示例：`/imagine prompt a cat --sref https://url.com/CatSketch.png`

![](img/7d3803afcafc1a6c370bf4e1ed9505b2.png)

各个样式引用也可以分配不同的权重：`--sref URL1::2 URL2::1 URL3::1`

[有关提示各部分之间相对重要性的更多信息](https://docs.midjourney.com/v1/docs/multi-prompts)，请参阅多提示页面。

---

## 样式参考版本

样式引用功能有四个版本。使用 `--sv` 参数在它们之间进行选择。`--sv 4` 为默认值。

`--sv 1` 原始的风格引用算法，更加“有氛围”

`--sv 2` 风格引用的第二次迭代

`--sv 3` 更加“有氛围”的更新版本 `--sv 1`

`--sv 4` 更新版本 `--sv 2`

![](img/7d1a4f32d31ebad6e4b18fa1e41ac692.png)

---

## 随机风格参考

您可以使用 `--sref random` 从 Midjourney 知道如何生成的抽象范围中随机选择一种样式。

+ 重新滚动🔄或使用 `--sref random` 对作业进行变体将生成具有与父网格相同随机样式的不同图像。
+ [来自排列](https://docs.midjourney.com/v1/docs/permutations) 或 [使用 `--sref random`](https://docs.midjourney.com/v1/docs/repeat) 提示的每个作业 `--repeat` 将使用不同的随机样式。
+ 提交提示后，“随机”将被替换为[样式参考种子](https://docs.midjourney.com/v1/docs/style-reference#style-reference-seeds)，您可以使用它在将来的提示中复制该样式。
+ `--sw` 与随机样式参考一起使用来控制随机样式对结果的影响程度。
+ 您不能将随机样式引用或样式引用种子相互组合或与 `URL` 样式引用组合。

### 例子

![](img/c6aae2a3437e30aedf06039ee7e6f3f1.png)

### 风格参考种子

在您完成的作业中，“随机”值将被替换为样式参考种子号：`--sref random`➡️ `--sref 762351716`。您可以在未来的提示中使用该样式参考种子来生成相同样式的图像。

![](img/881eeb2144ba1cae0e0cf072bc45ee47.png)

`https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc`
