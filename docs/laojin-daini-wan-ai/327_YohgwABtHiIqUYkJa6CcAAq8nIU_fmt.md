
# 进阶教程

> 来源：[https://my.feishu.cn/docx/I6DedzCqTopeKkxZuVvcI3kRngc](https://my.feishu.cn/docx/I6DedzCqTopeKkxZuVvcI3kRngc)

前提条件：熟悉`ComfyUI`基本工作流程，安装`efficiency-nodes`和`Impact Pack`插件。

## 效率节点

添加`K采样器(效率)`节点和`效率加载器`节点，连接输入输出端口，调整好参数，这样只需要两个节点即可进行文生图。

![](img/9865d8d3e0617f8a2ee2604540274bf6.png)

## XY图表

在`K采样器(效率)`节点和`效率加载器`节点之间添加`XY图表`节点，连接依赖和脚本端口。

右键点击新建节点>`效率节点`>`XY输入`，选择一个输入节点，连接`X`端口。用同样的方式选择另一个输入节点，连接`Y`端口。

![](img/bf76c245156952500f7df4d5146782e1.png)

## 使用检测器

检测器根据模型检测特定区域并以`SEGS`的形式返回处理后的数据。`SEGS` 是一种综合数据格式，包含`Detailer` 操作所需的信息，例如遮罩、`bbox`、裁剪区域、置信度、标签和控制网信息。

### Ultralytics检测器

`Ultralytics`检测器使用`YOLO`模型自动检测目标，根据模型的不同，返回结果有`BBOX`和`SEGM`两种类型。`BBOX` 代表边界框（`Bounding Box`），它将检测区域捕获为矩形框。`SEGM` 代表分割，它将检测区域捕获为轮廓。

#### 使用YOLO模型自动检测

`ultralytics`发布有专门检测单个类型的`YOLO`模型，比如`face_yolov8m.pt`专门检测人脸。

添加`Segm 检测到Seg（SEGM Detector (SEGS)）`节点，注意这个节点的文本框里有且仅有一个`all`标签，不要修改。添加检测加载器（`UltralyticsDetectorProvider`）节点，加载识别单个类型的`YOLO`模型，此节点支持检测结果为`BBOX`和`SEGM`类型的`YOLO`模型。添加加载图像节点，上传包含人物的图片。把这些节点的输入输出端口连接完整。

![](img/70719a35a9a09317403acce06705952e.png)

示例工作流程：https://github.com/ltdrdata/ComfyUI-extension-tutorials/raw/Main/ComfyUI-Impact-Pack/images/detectors.png

此工作流程中，上半部分的节点使用了人脸检测模型，输出结果是人脸的矩形区域，下半部分的节点使用了人物检测模型，该模型是`SEGM`类型，所以输出结果是人物的轮廓。

你可以从此处下载 `Ultralytics` 模型：

+   `huggingface.co/Bingsu/adetailer`，人脸、人物和服装的检测模型

+   `Releases · ultralytics/assets`，除面部或人物之外的各种类型的检测模型

`Ultralytics`模型的路径：`ComfyUI\models\ultralytics`

#### 使用YOLO模型和标签检测

还是使用`Ultralytics`检测器，但是使用可以检测多种类型的`YOLO`模型。

在上一个的工作流程的基础上，把检测加载器（`UltralyticsDetectorProvider`）节点的`YOLO`模型，改成检测多种类型的`YOLO`模型，比如`YOLOv8m-seg`，再把`Segm 检测到Seg（SEGM Detector (SEGS)）`节点的文本框里的`all`标签改成你想识别的对象的标签。

![](img/09e70bd587762c4f5877a3787ddfee1e.png)

示例工作流程：https://github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/detector-labels.png

`YOLO`模型能够检测的类型很少，比如`YOLOv8m-seg`只能检测80种类型的物体，如果要检测`YOLO`模型范围以外的物体，可以使用`SAM`检测器。

### SAM检测器

`SAM`检测器使用`Segment Anything`技术，基于`GroundingDino` 和 `SAM`模型，检测目标并生成轮廓蒙版。

使用`SAM`检测器需要安装`ComfyUI Segment Anything`。

添加`G-DinoSAM语义分割（GroundingDinoSAMSegment）`节点，输入端口连接`SAM`加载器和`G-Dino`模型加载器。

![](img/4b6693481200f2d70c7809b57e650401.png)

`GroundingDino` 和 `SAM`模型可以检测非常多的物体，同时也导致模型非常大。

#### 在YOLO模型的基础上检测

`YOLO`模型把人脸检测为矩形框，如果要把人脸检测为轮廓，但是又没有`SEGM`类型的人脸检测模型时，就可以使用这个方法。

在开头的`Ultralytics`检测器的工作流的基础上，添加`SAM`检测合并（`SAMDetectorCombined`）节点，添加`SAM`加载器（`SAMLoader`）节点并加载 `SAM` 模型，连接`BBox`检测到`Seg`节点。

![](img/708543191f31762f836912570c4b9681.png)

示例工作流程：https://github.com/ltdrdata/ComfyUI-extension-tutorials/raw/Main/ComfyUI-Impact-Pack/images/detectors-sam.png

此工作流程先使用`YOLO`模型把人脸检测为`BBOX`矩形框，然后使用 `SAM` 模型检测矩形框中间的主体。

`SAM`检测合并（`SAMDetectorCombined`）中的检测提示（`detection_hint`）是一个说明符，用于指示在进行分割时应该将哪些点包含在分割中。`center[-1` 指定 `mask` 中心的一个点，`horizontal-2` 指定中心水平线上的两个点，`vertical-2` 指定中心垂直线上的两个点，`rectangle-4` 指定 `mask` 内部矩形形状内的四个点，`diamond-4` 指定以中心点为中心的菱形形状内的四个点。

#### 在蒙版的基础上检测

如果某些类型超出`YOLO`模型的范围，就可以手动选择一个大致的检测范围，然后使用`SAM`检测器检测。

在加载图像节点上传图片，在遮罩编辑器中打开图片，使用画笔在图片上涂抹，创建一个蒙版，然后连接遮罩到`Seg（mask to segs）`节点，再连接`SAM`检测合并（`SAMDetectorCombined`）节点。

![](img/7d29c67de6191351d0093f030c4c07b8.png)

示例工作流程：https://github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/tutorial/sam.md at Main · ltdrdata/ComfyUI-extension-tutorials

#### 交互式 SAM 检测器

这种方式也是手动选择检测范围，但是所有功能就集中到了一个界面里，不用添加任何节点。

右键单击“加载图像”的节点，点击“在 `SAM` 检测中打开”。

在图片上单击鼠标左键，会输入蓝色的正提示，表示应包含的区域。在图片上单击鼠标右键，会输入红色的负提示，表示应排除的区域。

![](img/d2698034db1fbf8b4bad2b1c511839f6.png)

选择完毕后，按“检测”按钮生成蒙版，点击`save to node`保存蒙版。

![](img/a9995328b20b1532c28dbfa6fab92468.png)

目前交互式 `SAM` 检测器检测出的轮廓比较粗糙，如果你要选择精确的轮廓，请谨慎使用。

## 局部修复

### 基于蒙版修复图像

这种方式先用画笔在需要修复的区域涂抹，然后使用遮罩细化节点修复这个对象，适合用来修复没有具体对象或对象边缘不明确的区域。

工作流程：https://github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/detailer-hook.png

![](img/226f827e732ca8e115bbf82c4941deea.png)

重点参数

引导大小：如果`蒙版区域`小于引导大小时，此功能会放大蒙版区域，使短边达到引导大小的值。如果蒙版区域大于引导大小，图片不会重绘。

引导目标：该参数决定放大图片时，是把裁剪区域放大到“引导大小”，还是把蒙版区域放大到“引导大小”。

最大尺寸：放大图片时，限制边长的最大尺寸。

`羽化`：将重绘后的图像合成到原始图像上时，边缘的柔和程度。

`裁剪系数`：扩展裁剪区域，把蒙版周围的像素包含进来。如果该值很小，则恢复可能效果不好，因为无法了解周围的环境。

### 基于SEGS修复图像

这种方式先使用检测模型检测图像中的物体，然后使用局部细化节点进行修复，适合用来修复轮廓清晰的区域。

我在前面写了使用多种方式检测图像中的物体，`Ultralytics`检测器将检测结果输出为`SEGS`，将`SEGS`连接到局部细化（`detailer`）节点即可修复图像，`SAM`检测器将检测结果输出为遮罩，使用遮罩到`Seg`（`mask to segs`）节点将遮罩转换为`SEGS`，再将`SEGS`连接到局部细化（`detailer`）节点即可修复图像。

工作流程：https://github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/basic-refine.png

![](img/12921af52ee50bb92925a7abf54d6427.png)

这是基于`SEGS`修复人脸的工作流程，修改此工作流程的检测模型，就可以用来自动检测和修复图片上的其他物体。

### 面部修复

面部修复本质上也是局部修复，只是修复的区域是人脸，所以可以使用前面的基于`SEGS`修复图像的工作流程。

除了局部细化以外，Impact Pack还有面部细化（`FaceDetailer`）节点，本质上是人脸检测节点和局部细化节点的结合。

工作流程：https://github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/simple.png

![](img/69a4ae7e69470d01c16ffbf15d69423f.png)

如果你喜欢节点简洁一点，就可以使用这个工作流程。

### 手部修复

手的关节很多，手势千变万化，手指之间还有遮挡，绘画模型很难画好，所以修复一些复杂手势的手需要使用`controlnet`控制。

方式一：



# 工作流程

工作流程：[`https://github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/dwpose-segs-handfix.png`](https://link.zhihu.com/?target=https%3A//github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/dwpose-segs-handfix.png)

![](img/7ac011baa2d8c70b00e8610067dcfd91.png)

方式二：

工作流程：[`https://github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/meshgraphomer_dwpose_tile-segs-handfix.png`](https://link.zhihu.com/?target=https%3A//github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/meshgraphomer_dwpose_tile-segs-handfix.png)

![](img/1ff2aa922cb71f80ba8dbefb67537cf0.png)

## 图片放大

工作流程：[`https://github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/upscale.png`](https://link.zhihu.com/?target=https%3A//github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/upscale.png)

![](img/f76000b9528c4586be23aa8a07ec8fa1.png)

`K采样像素缩放器（PixelKSampleUpscalerProvider）`节点是一个放大器，它使用`VAE`解码器将潜空间转换为像素，执行放大，再使用`VAE`编码器转换回潜在数据，执行采样。此节点可以连接到`Latent`迭代缩放等节点以供使用。

`Latent`迭代缩放（`IterativeLatentUpscale`）节点是将`latent`按缩放系数放大的节点。它可以把图片的放大过程分为多个步骤，比如把图片放大2倍，步骤数为4时，它就会逐渐把图片放大到原来的1.25倍、1.5倍、1.75倍、2倍。和直接放大2倍相比，迭代缩放可以减少不同分块之间不连贯的问题，但是使用的时间也更长。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
