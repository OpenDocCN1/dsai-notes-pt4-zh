# 什么是ControlNet

> 来源：[https://jlqyxzzvbk.feishu.cn/docx/S0S0dK5yao4SOOxtqkAcmQe4nwc](https://jlqyxzzvbk.feishu.cn/docx/S0S0dK5yao4SOOxtqkAcmQe4nwc)

ControlNet 是一种用于控制稳定扩散模型的神经网络模型。您可以将 ControlNet 与任何稳定扩散模型一起使用。

使用稳定扩散模型的最基本形式是文本到图像。它使用文本提示作为指导图像生成的条件，以便您生成与文本提示匹配的图像。

除了文本提示之外，ControlNet 还添加了一项条件。 ControlNet 中的额外调节可以采取多种形式。

让我向您展示 ControlNet 功能的两个示例：通过 (1) 边缘检测和 (2) 人体姿势检测控制图像生成。

Edge detection example 边缘检测示例

如下图所示，ControlNet 获取额外的输入图像并使用 Canny 边缘检测器检测其轮廓。然后将包含检测到的边缘的图像保存为控制图。它被输入 ControlNet 模型作为文本提示的额外条件。

![](img/f710a8cc3078f13040769ed5aa0386d5.png)

Stable Diffusion ControlNet with Canny edge conditioning.

具有 Canny 边缘调节功能的稳定扩散 ControlNet。

从输入图像中提取特定信息（在本例中为边缘）的过程称为注释（在研究文章中）或预处理（在 ControlNet 扩展中）。

人体姿态检测示例

边缘检测并不是图像预处理的唯一方法。 Openpose 是一种快速人体关键点检测模型，可以提取人体姿势，例如手、腿和头部的位置。请参阅下面的示例。

![](img/5ccabfe875944a57832cb23e06f8fc69.png)

Input image annotated with human pose detection using Openpose.

使用 Openpose 通过人体姿势检测注释的输入图像。

以下是使用 OpenPose 的 ControlNet 工作流程。使用 OpenPose 从输入图像中提取关键点，并将其保存为包含关键点位置的控制图。然后将其与文本提示一起作为额外条件馈送到稳定扩散。图像是根据这两个条件生成的。

![](img/07de6c9d8d550fa9b78867dc7d02c1f2.png)

使用 Canny 边缘检测和 Openpose 有什么区别？ Canny 边缘检测器可提取拍摄对象和背景的边缘。它往往会更忠实地翻译场景。你可以看到跳舞的男人变成了女人，但轮廓和发型都保留了。

OpenPose仅检测人体关键点，如头部、手臂等位置。图像生成更加自由，但遵循原始姿势。

上面的例子生成了一个左脚指向侧面的女人跳起来，与原始图像和 Canny Edge 例子中的图像不同。原因是OpenPose的关键点检测没有指定脚的方向。

## OpenPose 开放姿势

有多个 OpenPose 预处理器。

OpenPose 可检测人体关键点，例如头、肩膀、手的位置等。它对于复制人体姿势非常有用，而无需复制服装、发型和背景等其他细节。

所有 openpose 预处理器都需要与 ControlNet 模型下拉菜单中的 openpose 模型一起使用。

OpenPose 预处理器：

+   OpenPose：眼睛、鼻子、眼睛、颈部、肩膀、肘部、手腕、膝盖和脚踝。

+   OpenPose_face：OpenPose + 面部细节

+   OpenPose_hand：OpenPose + 手和手指

+   OpenPose_faceonly：仅面部细节

+   OpenPose_full：以上所有

+   dw_openPose_full：OpenPose_full的增强版本

## OpenPose 开放姿势

OpenPose 是基本的 OpenPose 预处理器，可检测眼睛、鼻子、眼睛、颈部、肩膀、肘部、手腕、膝盖和脚踝的位置。

![](img/0321c8a33712b6b7fd3d2d0ad2a60f5d.png)

![](img/feede77ea96a94be35035037ad23d6b0.png)

### OpenPose_face OpenPose_face

OpenPose_face 执行 OpenPose 处理器执行的所有操作，但会检测其他面部细节。

它对于复制面部表情很有用。

![](img/9eae6052c2ff804f8d511b5fd18d2d8e.png)

![](img/59be99557ab403055f425ef12b65f9b4.png)

Sample images: 示例图像：

![](img/a04ece13e0dd6d96ab10c3eb81634424.png)

![](img/aede87ee2f27f54b03bef22eadee0952.png)

### OpenPose_faceonly

OpenPose面部仅检测面部而不检测其他关键点。这对于仅复制面部而不是其他关键点很有用。

![](img/3c9648281623a92651e9a63fb270f727.png)

![](img/66c5a0397837e2518f080c50bbd77ad2.png)

请参阅下面从文本到图像的示例。身体不受约束。

![](img/024fce2e669a471c794bf832bc15153d.png)

![](img/3a2610497924b6de531f8174f5af0d55.png)

### OpenPose_hand

OpenPose_hand 将关键点检测为 OpenPose 以及手和手指。

![](img/21426be95f6f2346621411c13f9a6961.png)

![](img/207e9353ed78c18bb6890a1858bad772.png)

Sample images: 示例图像：

![](img/293907db0eed3380ac862025025694f8.png)

![](img/6499cf33c78134daf6093c6b9c9d1ae2.png)

### OpenPose_full

OpenPose 全面检测 openPose 面部和 openPose 手所做的一切。

![](img/e2bce077409b36999393e13753b439bf.png)

![](img/6337f7c00c25dd2d7b8a45a005ceb779.png)

Sample images: 示例图像：

![](img/d93e60e8d733dc0295bd027f1184476c.png)

![](img/c8cac4f9a4e399d0294d2e5f5f0357e2.png)

### dw_openpose_full

DWPose 是一种新的姿势检测算法，基于研究文章《Effective Whole-body Pose Estimation with Two-stage Distillation》。它完成与 OpenPose Full 相同的任务，但做得更好。您应该使用 dw_openpose_full 而不是 openpose_full。

> 

> 如果您在预处理器菜单中没有看到 dw_openpose_full，请更新 ControlNet。

![](img/d77ad3f6e0ab4493aa17679b748b97fb.png)

![](img/e99f58fdd92996602ab9d503036489bb.png)

Reference image for OpenPose and DW OpenPose.

OpenPose 和 DW OpenPose 的参考图像。

OpenPose Full

DW OpenPose 在检测手和手指方面做得更好。

## Reference

参考是一组新的预处理器，可让您生成与参考图像类似的图像。图像仍会受到稳定扩散模型和提示的影响。

参考预处理器不使用控制模型。您只需要选择预处理器，而不需要选择模型。 （事实上，选择参考预处理器后，模型下拉菜单将被隐藏。）

> 

> 有 3 个参考预处理器。

1.  [**Reference adain**](https://github.com/Mikubill/sd-webui-controlnet/discussions/1280): 通过自适应实例规范化进行风格传输。

1.  [**Reference only**](https://github.com/Mikubill/sd-webui-controlnet/discussions/1236): 将参考图像直接链接到注意层。

1.  Reference adain+attn: 以上的组合。

> 

> 下面是一个例子。

![](img/629cc9c9c63bb4a6a8ede4cbd7e81b23.png)

Reference 图像

> 

> 使用 CLIP 询问器猜测提示。

> a woman with pink hair and a robot suit on, with a sci – fi, Artgerm, cyberpunk style, cyberpunk art, retrofuturism

> 粉红色头发、穿着机器人套装的女人，科幻、Artgerm、赛博朋克风格、赛博朋克艺术、复古未来主义

> disfigured, ugly, bad, immature

> 毁容、丑陋、不好、不成熟

### Model: Protogen v2.2

#### Reference adain

![](img/dbc765d4cdb433a4d87c868fa49f5473.png)

#### Reference only

![](img/302dee4b665ce6a04e8f6d75cd2d698e.png)

#### Reference adain+attn

![](img/338c0206172ee04e8343429c90fa1c24.png)

以上图片均来自平衡模式。我认为改变风格保真度没有太大区别。

## Canny

Canny 边缘检测器是一种通用的老式边缘检测器。它提取图像的轮廓。它对于保留原始图像的构图很有用。

在预处理器和模型下拉菜单中选择要使用的canny。

![](img/db34fd88ba3be1ed50c64acc737c1809.png)

![](img/a76609d9c1d693645584ac103f8a5479.png)

生成的图像将遵循轮廓。

![](img/6041783e3efa507c9c80450ff0b6bae8.png)

![](img/5343e8cfdf2d537b6f97780990f4870c.png)

## Depth 深度

> 

> 深度预处理器从参考图像中猜测深度信息。

+   **Depth Midas**: 经典的深度估计器。也用于官方 v2 深度图像模型。

+   **Depth Leres**: 更多细节，但也倾向于渲染背景。

+   **Depth Leres++**:更多细节。

+   **Zoe**: 细节程度介于 Midas 和 Leres 之间。

### 参考图片：

![](img/eae11bad0f70fdcbf7163bbad29916d4.png)

深度图：

![](img/fbf6b41fd46fb1e2fb45ddffb3ce9f94.png)

Midas

![](img/b483f3cceeef1627391be4da7bce2ca0.png)

Leres

![](img/d97e46d6d2271779b71f42f825d91577.png)

Leres++

![](img/69b057679155b8852a9a2e4695b1985a.png)

Zoe

> 

> 提示和反向提示词提示：

> a woman retrofuturism

> 复古未来主义的女人

> disfigured, ugly, bad, immature

> 毁容、丑陋、不好、不成熟

![](img/3f76ac75e4d805020248029f2133cc48.png)

**         **                                               Depth Zoe的文本到图像 。

与更详细的Leres++比较：

![](img/f23185089ecab12862ca3e6805f6839a.png)

**

Depth Lere**s 的文本到图像。

## Line Art 线条艺术

线条艺术呈现图像的轮廓。它试图将其转换为简单的绘图。

> 

> 有一些线条艺术预处理器。

+   **Line art anime**:动漫风格的线条

+   Line art anime denoise: 细节较少的动漫风格线条。

+   **Line ****art**** realistic**:写实风格的线条

+   **Line ****art**** coarse**: 写实风格的线条，粗重。

> 下面的图像是在“控制权重”设置为 0.7 的情况下生成的。

#### Line Art Anime 线条艺术动漫

![](img/f8904e20b21beb4d7a9bdf7fb60619c4.png)

![](img/934014439bbd84ffb5f15622f5985486.png)

#### Line Art Anime Denoise 线条艺术动漫降噪

![](img/17579b99509df698eade44f7098e3d22.png)

![](img/f46f8be2cf2cfca66ce95d48ea825700.png)

#### Line Art Realistic 线条艺术写实

![](img/4302c746aa8e43f2150387bf7e433803.png)

![](img/2a3977f350e3fe4a7b212606b558b351.png)

#### Line Art Coarse 粗线艺术

![](img/0c4ec50868f4a2466445fbb35267ee87.png)

![](img/644e86c65d72073650210e3fa0b5063b.png)

## MLSD

M-LSD（移动线段检测）是一种直线检测器。它对于提取具有直边的轮廓非常有用，例如室内设计、建筑物、街景、相框和纸张边缘。

曲线将被忽略。

![](img/9f4369ac25db8c0b09460b23b0de3df5.png)

![](img/c9d279f1b05158618edcbb25ffc8d11d.png)

## Normal maps 法线贴图

法线贴图指定表面的方向。对于 ControlNet，它是一个图像，指定每个像素所在表面的方向。图像像素代表表面面向的方向，而不是颜色值。

法线贴图的用法与深度贴图类似。它们用于传输参考图像的 3D 合成。

> 法线贴图预处理器：

+   Normal Midas: 根据 Midas 深度图估计法线贴图。

+   **Normal Bae**: 使用 Bae 等人提出的法线不确定性方法估计法线图。

### Normal Midas

与 Midas 深度图一样，Midas 法线贴图非常适合将主体与背景隔离。

![](img/0105018a5fc5452046e8b4500ca07143.png)

![](img/3ef9abe9a63a1c53163418a37653f260.png)

### Normal Bae

Bae 法线贴图倾向于在背景和前景中渲染细节。

![](img/543a1cdf937c7a690073589cb02e5076.png)

![](img/672c175f6ee041a57b84d6506cf3f95c.png)

## Scribbles 涂鸦

涂鸦预处理器将图片变成涂鸦，就像手绘的一样。

+   **Scribble HED**: 整体嵌套边缘检测 (HED) 是一种边缘检测器，擅长生成像真人一样的轮廓。据 ControlNet 的作者称，HED 适合对图像进行重新着色和重新设计样式。

+   Scribble Pidinet:像素差异网络 (Pidinet) 检测曲线和直线边缘。其结果与 HED 类似，但通常会产生更清晰的线条和更少的细节。

+   **Scribble xdog**:扩展高斯差分 (XDoG) 是一种边缘检测方法技术。调整 xDoG 阈值并观察预处理器输出非常重要。

所有这些预处理器都应与涂鸦控制模型一起使用。

### Scribble HED

HED 产生粗糙的涂鸦线。

![](img/8568683ba2f64124700903c6e5161bf5.png)

![](img/fb8542f16591fd25a0b58c2fea1dc91b.png)

### Scribble Pidinet

Pidinet往往会产生粗线条，细节很少。它非常适合复制没有细节的电路板轮廓。

![](img/a199bdf9e3de704f45f3f443961621bd.png)

![](img/f520db901e0e0ee3a7ff5ed764629d3e.png)

### Scribble xDoG

通过调整 XDoG 阈值可以控制细节水平，使 xDoG 成为用于创建涂鸦的多功能预处理器。

![](img/f715552cde93618afc2bc8c32baa6fa6.png)

![](img/f8233e65c833e3da9d2f251907f169be.png)

## Segmentation 分割

分割预处理器标记参考图像中的对象类型。

下面是一个正在运行的分段处理器。

![](img/28fe49a553f0204c30ee0e594bec1487.png)

![](img/862fe97d086bb72fb76b652626ecde6d.png)

The buildings, sky, trees, people, and sidewalks are labeled with different and predefined colors.

建筑物、天空、树木、人和人行道都标有不同的预定义颜色。

> 

> 有几个分段选项

+   **ufade20k**: 在 ADE20K 数据集上训练的 UniFormer (uf) 分割。

+   **ofade20k**: 在 ADE20k 数据集上训练的 OneFormer（of）分割。

+   **ofcoco**: 在 COCO 数据集上训练的 OnFormer 分割。

请注意，ADE20k 和 COCO 分割的颜色图是不同的。

您可以使用分段预处理器来传输对象的位置和形状。

下面使用具有相同提示和种子的这些预处理器。

> Futuristic city, tree, buildings, cyberpunk

#### UniFormer ADE20k (ufade20k) UniFormer ADE20k (unfade20k)

Uniformer 在此示例中准确地标记了所有内容。

![](img/ee21a0bf4fa859c57f4bd41550901a0c.png)

![](img/e57b30f31abdca55850ed002cfa9e87d.png)

#### OneFormer ADE20k (ofade20k) OneFormer ADE20k (ofade20k)

在这种情况下，OneFormer 的噪点较多，但不会影响最终图像。

![](img/08037aba985d6a179c51ccf404c61909.png)

![](img/31d1b1cb9bbac5333ec381e00dbd7d11.png)

OneFormer COCO (ofcoco) 前COCO (ofcoco)

OneFormer COCO 的表现类似，但有一些标签错误。

![](img/886c34913097970970b8c5abdd390df8.png)

![](img/6097fe26872247529100df73894c5b5a.png)

细分是一项强大的技术。您可以进一步操作分割图以将对象放置在精确的位置。使用 ADE20k 的颜色图。

## Shuffle 随机

随机预处理器搅动输入图像。让我们看看随机播放的效果。

与Shuffle控制模型一起，Shuffle预处理器可用于传输参考图像的配色方案。

Input image:

![](img/8a5e015c344cd6bd8aae5534de300036.png)

**Shuffle** preprocessor: 洗牌预处理器：

![](img/e83044f34e3b36dab20d9edbf9d3ac06.png)

与其他预处理器不同，Shuffle 预处理器是随机的。它将受到您的种子价值的影响。

将 Shuffle 预处理器与 Shuffle 控制模型结合使用。 Shuffle 控制模型可以与或不与 Shuffle 预处理器一起使用。

下图是 ControlNet Shuffle 预处理器和 Shuffle 模型（与上一节相同的提示）。配色方案大致遵循参考图像。

![](img/c291fe4b80466cedf134d15b5b8fd462.png)

下图仅适用于 ControlNet Shuffle 模型（预处理器：无）。图像构图更接近原图。配色方案与随机排列类似。

供您参考，下图是在没有 ControlNet 的情况下具有相同提示的图像。配色方案截然不同。

![](img/7820510ebe40ff346d3061e5f8638c83.png)

## Color grid T2I adapter 色格T2I适配器

彩色网格 T2i 适配器预处理器将参考图像缩小 64 倍，然后将其扩展回原始大小。最终效果是局部平均颜色的网格状斑块。

原始参考图：

![](img/b9807e5bc0345b49510ec2c19eb17ec0.png)

使用 t2ia_color_grid 进行预处理：

![](img/f2b72a2de54848820009159ae3368c0a.png)

然后，预处理后的图像可以与 T2I 颜色适配器 ( t2iadapter_color ) 控制模型一起使用。

图像生成将在空间上松散地遵循配色方案。

> A modern living room

> 一个现代的客厅

![](img/4a6affb73f0702f6ea989fc3c6f6ff28.png)

增加 ControlNet 权重以使其更紧密地跟随。

您还可以为此 T2I 颜色模型使用预处理器 None。

在我看来，它与图像到图像非常相似。

剪辑视觉式 T2I 适配器

t2ia_style_clipvision 将参考图像转换为 CLIP 视觉嵌入。这种嵌入包含有关图像内容和风格的丰富信息。

您将需要使用控制模型 t2iadapter_style_XXXX。

看看这个惊人的风格转移的实际效果：

参考图片：

![](img/b8f98fc33a678c75ffc9befd70fcfbaf.png)

T2I 适配器 – CLIP 愿景：

> sci-fi girl 科幻女孩

![](img/0f21f9ff32a78952cbbb0614b47daede.png)

![](img/7cfb44097f41d03410603cc578d4e82a.png)

以下是关闭 ControlNet 时会生成的提示信息。

![](img/36282c49690269b5f25a28e1c8695e90.png)

该功能与 Reference ControlNet 非常相似，但我对 T2IA CLIP 视觉的评价更高。

## ControlNet Inpainting ControlNet 修复

ControlNet 修复可让您在修复中使用高降噪强度来生成较大的变化，而不会牺牲整个图片的一致性。

例如，我使用了针对现实人物的提示。

### Model: [HenmixReal v4](https://civitai.com/models/20282)

> photo of young woman, highlight hair, sitting outside restaurant, wearing dress, rim lighting, studio lighting, looking at the camera, dslr, ultra quality, sharp focus, tack sharp, dof, film grain, Fujifilm XT3, crystal clear, 8K UHD, highly detailed glossy eyes, high detailed skin, skin pores

> 年轻女子的照片, 突出头发, 坐在餐厅外, 穿着裙子, 边缘照明, 工作室照明, 看着相机, 单反相机, 超品质, 锐焦, 锐利, 自由度, 胶片颗粒, Fujifilm XT3, 晶莹剔透, 8K UHD 、高细节光泽眼睛、高细节皮肤、皮肤毛孔

> Negative prompt ：

> disfigured, ugly, bad, immature, cartoon, anime, 3d, painting, b&w

> 毁容、丑陋、不良、不成熟、卡通、动漫、3D、绘画、黑白

我用这张图像，想通过修复来重新生成脸部。

![](img/9b9a8e1e5cbea8a9748a0acb4a5bb770.png)

如果我用高去噪强度（> 0.4）修复脸部，结果可能会在全局上不一致。下面是去噪强度为1的未绘制图像。

![](img/e833e9b6c4c90c50e23ee40ab41aa771.png)

使用 ControlNet 修复：

1.  最好使用生成图像的相同模型。在txt2img页面生成图像后，单击发送到Inpaint，将图像发送到Img2img页面的Inpaint选项卡。

1.  使用画笔工具在要重新生成的区域上创建遮罩。如果您不熟悉，请参阅有关修复的初学者教程。

![](img/5bde945f7496a8f0a44d5938d6b422f6.png)

1.  将修复区域设置为仅遮罩。 （全图也可以）

1.  将降噪强度设置为 1。（如果没有 ControlNet，通常不会将其设置这么高。）

1.  在 ControlNet 部分设置以下参数。您不需要上传参考图像。

**Enable: Yes 启用**

预处理器：Inpaint_global_harmonious

**Model: inpaint**

![](img/9f549c22ffa3832cfccd710bee958582.png)

1.  按“生成”开始修复。

现在，即使在最大去噪强度 (1) 下，我也能得到与全局图像一致的新面孔！

![](img/e94b06ba0a99aa74f63ceab5437f2b86.png)

![](img/12dd34c3699ddb8984c20806b27be8b0.png)

> 

> 目前，有 3 个修复预处理器

+   Inpaint_global_harmonious: 提高全局一致性并允许您使用高去噪强度。

+   **Inpaint_only**: 不会改变未遮罩的区域。与 AUTOMATIC1111 中的 Inpaint_global_harmonious 相同。

+   Inpaint_only+lama: 用lama模型处理图像。它往往会产生更干净的结果，并且有利于物体去除。

![](img/1022319b44ddbcb6455c720b77b4be15.png)

![](img/4e435600fb5d448d502db06506ffa51b.png)

原图                                                    用于物体移除的 Inpaint+lama 模型。

使用了一个空提示。