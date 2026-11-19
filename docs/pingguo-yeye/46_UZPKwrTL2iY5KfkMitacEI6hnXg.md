
# Stable diffusion中照明的应用

> 来源：[https://e0lrm2v2v1u.feishu.cn/docx/UPiMdm9L9o7MRQxzTHJcSM7qnQc](https://e0lrm2v2v1u.feishu.cn/docx/UPiMdm9L9o7MRQxzTHJcSM7qnQc)

照明（`Lighting`）是摄影的命脉，赋予您的图像迷人的品质，并赋予它们所需的情绪。对光线的巧妙处理使您能够提升主题，注入深度和维度，唤起情感并突出关键细节。在本综合指南中，我们将深入研究对照明进行绝对控制的无数技术，使您能够制作出令人惊叹的图像，从而产生持久的影响。

## 正文

### 无照明的示例

我们可以从生成图像开始，而无需深入研究照明，这样一旦我们在提示中注入关键的照明词，我们就可以看到差异。

使用了大模型`dreamshaper_5`，采样器是`Euler a`，采样步骤30和`CFG`值11。

> Positive Prompt:
>
> a full body shot of a young beautiful woman

![](img/ee7a103a390ea45979bbcd95712c1abb.png)

我们可以看到`stable diffusion`会产生逼真的图像，但我们可以通过增加对照明的控制来增强这一点。

### 照明关键词

+ `Volumetric lighting`: 为您的提示注入“体积”的魔力，以唤起空灵的光束
+ `Rim lighting`: 通过调用“边缘照明”来增加一丝神秘感，以柔和的光芒勾勒出您的主题
+ `Sunlight`: 通过包含“阳光”来唤起黄金时段的温暖，将您的图像沐浴在大自然的光辉中
+ `Backlight`: 通过将光源放置在主体后面并强调其轮廓来创造令人惊叹的效果
+ `Dimly lit`: 通过在提示中编织“昏暗的灯光”来引入阴谋和微妙的气氛
+ `Crepuscular rays`: 通过召唤“黄昏射线”来点燃视觉上的辉煌，用戏剧性的光束绘制你的构图

### 使用照明关键字的示例

#### Volumetric lighting 体积照明

> Positive Prompt:
>
> solo girl, a full body shot of a young beautiful woman, `volumetric lighting`

> Negative Prompt:
>
> Watermark, Text, censored, deformed, bad anatomy, disfigured, poorly drawn face

#### Rim lighting 轮辋照明

> Positive Prompt:
>
> solo girl, a full body shot of a young beautiful woman, `rim lighting`

> Negative Prompt:
>
> Watermark, Text, censored, deformed, bad anatomy, disfigured, poorly drawn face

![](img/7e7f499dcc9af79a9e6e61d23478fe71.png)

#### Sunlight 阳光

> Positive Prompt:
>
> solo girl, a full body shot of a young beautiful woman, `sunlight`

> Negative Prompt:
>
> Watermark, Text, censored, deformed, bad anatomy, disfigured, poorly drawn face

![](img/d1c02183653b16a93d7e29c3e42d502e.png)

#### Backlight 背光

> Positive Prompt:
>
> solo girl, a full body shot of a young beautiful woman, `backlight`

> Negative Prompt:
>
> Watermark, Text, censored, deformed, bad anatomy, disfigured, poorly drawn face

![](img/9e350a9fd928a007b1f0882b0f71126f.png)

#### Dimly lit 昏暗

> Positive Prompt:
>
> solo girl, a full body shot of a young beautiful woman, `dimly lit`

> Negative Prompt:
>
> Watermark, Text, censored, deformed, bad anatomy, disfigured, poorly drawn face

![](img/3bb752cf275d448631314ca6417b2f8d.png)

#### Crepuscular rays 黄昏射线

> Positive Prompt:
>
> solo girl, a full body shot of a young beautiful woman, `crepuscular rays`

> Negative Prompt:
>
> Watermark, Text, censored, deformed, bad anatomy, disfigured, poorly drawn face

![](img/0e5535f95c758ac74a1fe71ca5f1e90a.png)

### 使用区域提示器 `Regional Prompter`

可以通过从 `GitHub` 复制以下 URL 来安装`Regional Prompter`，并 `https://github.com/hako-mikan/sd-webui-regional-prompter`

单击安装，然后转到已安装选项卡，单击应用并重新启动 UI 以使更改生效。如果在 `txt2Img` 选项卡上看不到区域提示符，则可能需要完全重新启动`stable diffusion`实例。

![](img/77b31deb36800da48c04337ac235059c.png)

完成后，您将看到我们有一个名为区域提示器 `regional prompter` 的下拉选项

## 使用`regional prompter`的示例

我们可以使用区域提示器来帮助我们进行光影提示。在 `txt2Img` 选项卡上，我将输入以下设置：

+ (1) 选中活动复选框并
+ (2) 检查 使用常用提示 复选框
+ (3) 拆分模式选择水平`horizontal`
+ (4) 并输入2，3作为值
+ (5) 选中禁用将`AND`转换为`BREAK`
+ (6) 单击可视化和制作模板以确保我们有正确的输出

![](img/24767574983f58af4d8ca5a51c62c90b.png)

> Positive Prompt:
>
> solo girl, a full body shot of a young beautiful woman, `(detailed facial features)`, `(realistic)`, `(hyperdetailed:1.15)`, `detailed eyes`, `(full body:1.5)`, `photorealistic`, `8k`, `uhd`, `dslr`, `high quality`, `file grain`, `masterpiece`

> `BREAK`

> `(hard light:1.4)`, `(volumetric:1.5)`, `(well-lit:1.3)`

> `BREAK`

> `(dimly lit:1.8)`

> Negative Prompt:
>
> Watermark, Text, censored, deformed, bad anatomy, disfigured, poorly drawn face

现在，我们可以通过使用带有中断子句的区域提示器来进一步控制光影，以专门照亮图像的左侧，并在图像的右侧设置较暗的主题。

![](img/36cf1f0557c63ac4281b7943ee7ad2ba.png)
