
# ControlNet OpenPose：如何用 Stable diffusion 复制任何图像和姿势？

> 来源：[https://e0lrm2v2v1u.feishu.cn/docx/PKALdh30poBq0txCwZsc4Opdnkd](https://e0lrm2v2v1u.feishu.cn/docx/PKALdh30poBq0txCwZsc4Opdnkd)

## ControlNet 和 OpenPose 简介

`ControlNet` 是一种革命性的稳定扩散模型，旨在促进从参考图像复制构图或人体姿势。

使用 `ControlNet`，稳定扩散用户的挫败感得到了缓解，因为它提供了对主体放置和外观的精确控制。

在本文中，我们将深入研究 `ControlNet OpenPose` 的全面细节，包括：

`ControlNet OpenPose` 及其功能概述；各种用例的关键设置和实际应用。

准备好释放 `ControlNet OpenPose` 的全部潜力，将您的创意努力提升到新的高度。

![](img/420c10303ce9f7213e534bc610b2f09c.png)

## 什么是 `ControlNet OpenPose`？

`ControlNet OpenPose` 是指将 `ControlNet` 的功能与 `OpenPose`（用于人体姿势估计的高级计算机视觉库）相结合的特定组件或功能。通过将 `ControlNet` 与 `OpenPose` 集成，用户可以在稳定扩散框架内控制和操纵人体姿势。

通过利用 `ControlNet` 和 `OpenPose` 的组合功能，稳定扩散用户可以在生成或操作涉及人类受试者的作品时获得更可控和更有针对性的结果。

## 有哪些 `OpenPose` 预处理器可用？

`OpenPose` 预处理器包括：

+ `\`OpenPose\``: eyes, nose, eyes, neck, shoulder, elbow, wrist, knees, and ankles.

![](img/44770dd7b84e1e517236bc7aeb0b581d.png)

+ `\`OpenPose_face\``: `OpenPose` + facial details

![](img/86f68468843da494950bc979590dde7c.png)

+ `\`OpenPose_hand\``: `OpenPose` + hands and fingers

![](img/23f0655a931fb40f9fc46cb20b580fa6.png)

+ `\`OpenPose_faceonly\``: facial details only

+ `\`OpenPose_full\``: All of the above

![](img/a696990260cec5691ec582be5467371f.png)

## 如何使用 `ControlNet` 和 `OpenPose`

（1）在文本到图像选项卡上。

（2）将您的图片上传到 `ControlNet` 单张图片部分，如下所示

（3）通过选中启用复选框来启用 `ControlNet` 扩展。

（4）选择 `\`OpenPose\`` 作为控件类型

（5）选择 `\`openpose\`` 作为预处理器。`OpenPose` 检测人类的关键点，如头部、手臂等的位置。

（6）选择 `\`control_sd15_openpose\`` 作为 `ControlNet` 模型，与 `OpenPose` 兼容。

![](img/c93ed8d12687de872e79f1108c8b37ff.png)

然后，您可以键入正面和负面提示，然后单击生成按钮以开始使用 `ControlNet` 生成图像。

![](img/0b74c1c08717d95a04e58839821b4c45.png)

您将看到生成的图像跟随输入图像的姿势，最后一个图像显示检测到的关键点。

禁用 `ControlNet`：

完成 `ControlNet` 扩展后，取消选中启用复选框以将其禁用。

## `ControlNet` 设置

![](img/b27f8d530a889afedd6842a0deb2483d.png)

预处理器：

`ControlNet` 为不同的任务提供了各种预处理器。

若要启用预处理器输出的预览，请选择“允许预览”。

（可选）选择“像素完美”以生成从文本到图像的指定高度和宽度的预处理图像。

![](img/b15926f19fe95bfdc30b713131ee6cb9.png)

预处理器和型号：

`ControlNet` 需要选择预处理器和兼容型号。

预处理器修改原始图像，只有预处理的图像用于 `ControlNet`。

![](img/55da4b435ad510f06b8593ffe8cd1274.png)

要选择预处理器和型号：

单击预处理器下拉菜单旁边的爆炸图标。

从可用选项中选择所需的预处理器。

确保在 `ControlNet` 的“模型”下拉菜单中选择相应的模型。

现在我们已经介绍了使用 `ControlNet` 的功能和设置，让我们来看看每个模型可以做什么！

## 示例

### `OpenPose`

`OpenPose` 检测眼睛、鼻子、眼睛、颈部、肩部、肘部、手腕、膝盖和脚踝的人体关键点位置。它适用于复制人类姿势，同时排除服装、发型和背景等其他细节。

例如，如果我们上传一张处于某种姿势的男人的照片，我们可以选择控件类型为 `OpenPose`，作为模型的 `openpose` 和 `control_sd15_openpose` 的预处理器。

`Positive Prompts`: (a photograph of a beautiful girl jumping in the air wearing trainers), (ultra detailed face:1.3), makeup, shot on a Sony mirrorless camera, DSLR, 50mm lens f/2.8, ultra detailed, 8k, morning golden hour

`Negative Prompts`: deformed, disfigured, nudity, nsfw, blurry, lowres, cartoon, anime, multiple people, warped face

### `OpenPose Full`

`OpenPose_full` 检测所有打开姿势的面部和 `openPose_hand`。

`OpenPose_full` 结合了 `OpenPose` 检测到的所有关键点，包括面部细节和手/手指。

例如，如果我们上传一张男人挥手的图片，我们可以选择要 `openpose_full` 的预处理器并 `control_sd15_openpose` 作为模型。

`Positive Prompts`: Photograph of a man sat in a chair, waving, shot on a Sony mirrorless camera, DSLR, 50mm lens f/2.8, ultra detailed, 8k, [ at night / morning golden hour ]

`Negative Prompts`: Nudity, nsfw, blurry, lowres, cartoon, anime

### `OpenPose_face`

`OpenPose_face` 包括 `OpenPose` 检测到的所有关键点，以及其他面部细节。

它对于复制面部表情以及身体姿势很有用。

例如，如果我们上传一张男人挥手的图片，我们可以选择要 `openpose_face` 并 `control_sd15_openpose` 的预处理器作为模型。

`Positive Prompts`: Photograph of a lady sat in a chair, waving, shot on a Sony mirrorless camera, DSLR, 50mm lens f/2.8, ultra detailed, 8k, [ at night / morning golden hour ]

`Negative Prompts`: Nudity, nsfw, blurry, lowres, cartoon, anime

### `OpenPose_faceonly`

`OpenPose_faceonly` 只专注于检测面部细节，不包括其他关键点。

这有助于仅复制面部而不包括其他身体关键点。

例如，如果我们上传一张男人笑的图片，我们可以选择要 `openpose_faceonly` 的预处理器并 `control_sd15_openpose` 作为模型。

`Positive Prompts`: (a photograph of a man laughing:1.2), shot on a Sony mirrorless camera, DSLR, 50mm lens f/2.8, ultra detailed, 8k, morning golden hour

`Negative Prompts`: Nudity, nsfw, blurry, lowres, cartoon, anime

![](img/f091fec5e6dd0f09afd13b63c052c97d.png)

### `OpenPose_hand`

`OpenPose_hand` 使用 `OpenPose` 检测关键点，包括手和手指。这可能很有用，因为稳定的扩散有时真的很难生成逼真的手部姿势。

复制手部姿势和身体姿势是有益的。

例如，如果我们上传一张男人竖起大拇指的照片，我们可以选择要 `openpose_hand` 的预处理器并 `control_sd15_openpose` 作为模型。

`Positive Prompts`: (a photograph of a beautiful girl doing the thumbs up), shot on a Sony mirrorless camera, DSLR, 50mm lens f/2.8, ultra detailed, 8k, morning golden hour

`Negative Prompts`: deformed, disfigured, Nudity, nsfw, blurry, lowres, cartoon, anime, multiple people

这就是将 `ControlNet` 与 `OpenPose` 一起使用的所有内容！尝试使用不同的预处理器和模型，根据人类姿势和关键点生成图像。
