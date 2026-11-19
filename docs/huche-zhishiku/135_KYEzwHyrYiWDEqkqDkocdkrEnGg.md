
# GPT 4o 的图像生成 API 发布了，挺贵，支持 ComfyUI 原生节点

> 来源：https://huchegames.feishu.cn/docx/HUKudAQjeo0cuWxYq5hcFn6Snuc
>
> 《Image generation - OpenAI API》使用 GPT Image API 进行图像生成与编辑。
>
> 4o 的图像生成 API 发布了，挺贵，支持 ComfyUI 原生节点
>
> API 说明：https://platform.openai.com/docs/guides/image-generation?image-generation-model=gpt-image-1
>
> 【Zho】哇！ComfyUI 原生支持 GPT-Image 1 API（新节点！
>
> 还新增了用户系统直接结算 API，无需 OpenAI 账户，这下不用发愁 OpenAI API 不方便用的问题了！
>
> 这下可以在 ComfyUI 中用上 4o 图像生成了！这不就解决了 4o 对 ComfyUI 产生冲击的问题哈哈哈哈哈
>
> 官文：https://blog.comfy.org/p/comfyui-now-supports-gpt-image-1

先看看价格吧

![](img/34cdaf82498d8dc4ab87893e8c2eaf21.png)

## 图像生成（Image Generation）

OpenAI API 支持通过文本提示生成或编辑图像，使用的模型包括 GPT Image 和 DALL·E 系列。

目前，图像生成仅通过 [`Image API`](https://platform.openai.com/docs/api-reference/images) 提供，未来将扩展至 [`Responses API`](https://platform.openai.com/docs/api-reference/responses)。

### 三种图像 API 端点

1. `Generations`（生成）：根据文本提示从零开始生成图像。
1. `Edits`（编辑）：使用新提示修改已有图像，可局部或整体更改。
1. `Variations`（变体）：基于已有图像生成多个变体（仅适用于 DALL·E 2）。

你可以自定义输出图像的质量、尺寸、格式、压缩率，以及是否使用透明背景。

### 模型对比

使用 `gpt-image-1` 需要先完成 API 组织验证。

### 图像生成示例

你可以通过以下方式调用 API 来生成图像：

```
from openai import OpenAI
import base64
client = OpenAI()

result = client.images.generate(
    model="gpt-image-1",
    prompt="Draw a rocket in front of a blackhole in deep space"
)

image_base64 = result.data[0].b64_json
image_bytes = base64.b64decode(image_base64)

# Save the image to a file
with open("blackhole.png", "wb") as f:
    f.write(image_bytes)
```

### 图像编辑（Edits）

你可以：

+ 修改已有图像
+ 使用参考图像生成新图像
+ 使用遮罩（`mask`）局部编辑图像（称为 "inpainting"）

示例：使用多张参考图像生成新图像

将 4 张图像合成为一个礼品篮图像：

```
from openai import OpenAI
client = OpenAI()

result = client.images.edit(
    model="gpt-image-1",
    image=[
      open("body-lotion.png", "rb"),
      open("bath-bomb.png", "rb"),
      open("incense-kit.png", "rb"),
      open("soap.png", "rb"),
    ],
    prompt="Generate a photorealistic image of a gift basket on a white background labeled 'Relax & Unwind' with a ribbon and handwriting-like font, containing all the items in the reference pictures"
)

image_base64 = result.data[0].b64_json
image_bytes = base64.b64decode(image_base64)

# Save the image to a file
with open("gift-basket.png", "wb") as f:
    f.write(image_bytes)
```

![](img/7ba5bac39313ab3b1edc2e480881234d.png)

### 示例：使用遮罩局部编辑图像

遮罩图像的透明区域将被替换，黑色区域保持不变。

```
from openai import OpenAI
client = OpenAI()

result = client.images.edit(
    model="gpt-image-1",
    image=open("sunlit_lounge.png", "rb"),
    mask=open("mask.png", "rb"),
    prompt="A sunlit indoor lounge area with a pool containing a flamingo"
)

image_base64 = result.data[0].b64_json
image_bytes = base64.b64decode(image_base64)

# Save the image to a file
with open("composition.png", "wb") as f:
    f.write(image_bytes)
```

遮罩图像必须与原图尺寸一致，且包含 alpha 通道。

![](img/222032f42921327be0e183bb333dd707.png)

### 自定义图像输出

你可以设置以下参数：

+ **尺寸**：
  + `1024x1024`（方形）
  + `1024x1536`（纵向）
  + `1536x1024`（横向）
  + `auto`（默认）
+ **质量**：
  + `low`（低）
  + `medium`（中）
  + `high`（高）
  + `auto`（默认）
+ **格式**：`png`（默认）、`jpeg`、`webp`
+ **压缩率**：适用于 `jpeg` 和 `webp`，范围 0–100%
+ **背景**：`transparent`（透明）或默认不透明

### 成本与延迟

图像生成的成本与延迟取决于生成的图像 token 数量，受图像尺寸和质量影响。

此外，还需计算文本提示所用的输入 tokens。

### 限制说明

+ **延迟**：复杂提示可能需要最多 2 分钟生成图像。
+ **文字渲染**：虽然比 DALL·E 系列更好，但仍可能不够精确。
+ **一致性**：多次生成中可能难以保持角色或品牌元素一致。
+ **构图控制**：在结构化布局中可能难以精确放置元素。

### 内容审核

所有提示和生成图像都将经过内容审核。你可以通过 `moderation` 参数设置审核严格度：

+ `auto`（默认）：标准审核
+ `low`：较宽松审核

[OpenAI API 定价页面](https://openai.com/api/pricing/)
