
# --quality（质量）

> 来源：[https://my.feishu.cn/docx/D3xEd6aPQoGWWUxucf9cSb4An6d](https://my.feishu.cn/docx/D3xEd6aPQoGWWUxucf9cSb4An6d)

## --quality 使用或控制图像生成所需的时间（--q）

值越低 `--q`，图像生成速度越快。这些作业消耗的 GPU 时间越少，但生成的图像细节会越少。这对于尝试新提示很有用，这样您就可以更快地磨练自己的想法。

使用更高的 `--q` 值会花费更多的 GPU 时间并可以改善图像纹理和细节，但可能会降低图像的连贯性。

## 版本质量兼容性

+ `--quality` 不会影响生成的图像的分辨率，也不保证生成的图像会比使用更高或更低值生成的图像 [`“更好”`](https://docs.midjourney.com/v1/docs/fast-relax) 或“更差” - 它只会影响 Midjourney 创建它们所花费的 GPU 时间。

+ 默认 `--quality` 值为 1。

+ 其他值将四舍五入为您正在使用的模型版本的最接近的可用选项。

+ `--q` 仅影响初始图像生成，而不会影响 [变化](https://docs.midjourney.com/v1/docs/variations) 或 [升级](https://docs.midjourney.com/v1/docs/upscalers)。

## 质量比较

提示示例：`/imagine prompt` intricate woodcut of a peony

![](img/ab969ad13cf92b2d0edb68a8e2f74036.png)

## 如何使用质量参数

### 使用 `--quality` 或 `--q` 参数

将 `--quality <value>` 或添加 `--q <value>` 至提示的末尾。

![](img/e46bdd602a4ba4c7d8976c044215b7bf.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
