# 操作说明

> 来源：[https://my.feishu.cn/docx/Rrt0dIDiMor1IYxOLimcpsK8nQQ](https://my.feishu.cn/docx/Rrt0dIDiMor1IYxOLimcpsK8nQQ)

## 基本操作

在空白处滑动鼠标滚轮放大和缩小画布。

左键单击并拖动空白处，可在工作区中移动。

左键单击并拖动节点，可移动节点。

双击空白区域，显示节点的搜索栏。

CTRL+拖动可选择多个节点，使用SHIFT+拖动将它们一起移动。

左键单击并拖动节点的输入或输出的端口可以创建连接线，只能在相同类型的输入端和输出端之间进行连接。

点击窗口右上角的“添加提示词队列”运行工作流程。

点击左边菜单栏上的“队列”，将显示当前正在运行和正在等待的任务。任务完成后，会显示为图像，右键点击图像，可以选择删除和加载工作流。

在 ComfyUI 中加载工作流程有两种方法。

1.  点击顶部的菜单栏的第一个按钮，再点击“浏览”按钮，选择使用ComfyUI创建的 .json 或 .png 文件。

1.  拖放 .json 或 .png 文件到ComfyUI窗口中以打开文件。

## [文生图](https://zhida.zhihu.com/search?q=%E6%96%87%E7%94%9F%E5%9B%BE&zhida_source=entity&is_preview=1)

ComfyUI的默认界面是最简单的文生图工作流。工作流程由节点和线组成，节点是矩形块，用于执行某种任务，线是连接节点之间的输入和输出的线。

![](img/1bbd3ecc235bcb5a5b7b5cb96c5c4588.png)

使用方法：

首先，在“Checkpoint[加载器](https://zhida.zhihu.com/search?q=%E5%8A%A0%E8%BD%BD%E5%99%A8&zhida_source=entity&is_preview=1)”节点中选择模型，单击模型名称会显示所有可用的模型。

[Check](https://zhida.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E7%BC%96%E7%A0%81%E5%99%A8&zhida_source=entity&is_preview=1)p[oint加](https://zhida.zhihu.com/search?q=%E6%AD%A3%E5%90%91%E6%8F%90%E7%A4%BA%E8%AF%8D&zhida_source=entity&is_preview=1)载器连接了两个名为“CLIP文本编码器”的节点，上面的节点连接到“K采样器”节点的正面条件，下面的节点连接到“K采样器”节点的负面条件，所以要在上面的节点输入正向提示词，在下面的节点输入反向提示词。

在“空Latent”节点中设置图片尺寸。

在“K采样器”节点中设置采样器、迭代步数等参数。

点击“添加提示词队列”运行工作流程，运行完毕后，你会看到生成的第一张图片。

## 图生图

下载[图生图工作流程图片](https://link.zhihu.com/?target=https%3A//comfyanonymous.github.io/ComfyUI_examples/img2img/img2img_workflow.png)，将此工作流程图片拖放到 ComfyUI 中进行加载，[或者单击右侧](https://zhida.zhihu.com/search?q=%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B%E6%96%87%E4%BB%B6&zhida_source=entity&is_preview=1)面板的“加载”按钮，然后选择工作流程文件。

手动创建图生图工作流：

加载默认的文生图工作流，

在“空Latent”节点上右键，点击”移除“，

在空白处右键，选择”新建节点“>“图像”>“加载图像”，

点击“加载图像”的图像输出端并拖动，选择”新建节点“>“图像”>“放大”>“图像按系数缩放”，

点击“图像按系数缩放”的图像输出端并拖动，选择”新建节点“>“Latent”>“VAE编码”，

把“VAE编码”节点的Latent输出连接到和“K采样器”节点的Latent输入。

![](img/7c1e3b958eec311a0697ef0adea398ea.png)

使用方法：选择模型，上传图片，修改提示词，调整K采样器节点中的参数，点击“添加提示词队列”开始生成。

## 局部修复

### 使用[标准模型](https://zhida.zhihu.com/search?q=%E6%A0%87%E5%87%86%E6%A8%A1%E5%9E%8B&zhida_source=entity&is_preview=1)进行修复

下载此[修复工作流程](https://link.zhihu.com/?target=https%3A//stable-diffusion-art.com/wp-content/uploads/2024/01/inpaint-sd-model.json)，将此工作流程拖放到 ComfyUI 中进行加载。

手动创建工作流：在图生图工作流的基础上添加“VAE编码”和“设置Latent噪波遮罩”节点，它们应位于“加载图像”和“K采样器”节点之间。

你可能需要先熟悉下载的工作流之后才能熟练的创建工作流。

![](img/0bc1d46b0cdfa6434df031db7e97825f.png)

使用方法：

选择模型，上传需要修复的图片。

右键单击图片，在菜单中选择“在[遮罩编辑器](https://zhida.zhihu.com/search?q=%E9%81%AE%E7%BD%A9%E7%BC%96%E8%BE%91%E5%99%A8&zhida_source=entity&is_preview=1)中打开”，在图片上使用画笔涂抹要修复的区域，使用鼠标滚轮控制画笔大小，按住ALT键可以擦除涂抹的区域。

![](img/ea530bc661d87534ba09dec5cc6c29c5.png)

单击“保存到节点”，调整提示词和参数，点击“添加提示词队列”，生成图片。

你可以在K采样器节点中调整重绘强度，但是设置得太高可能会导致图像不连贯。

点击“添加提示词队列”开始生成修复的图片。

此工作流程适用于低重绘强度，如果必须使用高重绘强度，则需要使用接下来的工作流程。

### 使用修复模型进行修复

修复模型是专门为填补缺失内容而设计的特殊模型，其模型架构与标准模型不同。上一节中的工作流程不适合与修复模型一起使用，效果不好。

下载此[修复工作流程](https://link.zhihu.com/?target=https%3A//stable-diffusion-art.com/wp-content/uploads/2024/01/inpaint-sd15-inpaint-model.json)，将此工作流程拖放到 ComfyUI 中进行加载。

手动创建工作流：在图生图工作流的基础上添加“VAE内补编码器”节点，该节点应位于“加载图像”和“K采样器”节点之间。

用法与第一种方法相同，这里不再赘述。

![](img/15078059f8dc2cf085c233bfc3d421d0.png)

![](img/8471886b2ed29c215e9ab4d90c24aec0.png)

请注意，此工作流程仅在重绘强度设置为 1 时才能达到最佳效果，请勿将其更改为任何其他值。

此工作流程也可以使用标准模型进行修复，重绘强度依然要设置为 1 。

### 面部修复

ADetailer是一个 AUTOMATMIC1111 扩展，可自动使用修复来修复面部。ComfyUI 中的对应节点是Face Detailer（也称为DDetailer），它包含在ComfyUI Impact pack中。ComfyUI Impact pack是一组自定义节点，可以极大地增强 ComfyUI 的功能。

文生图方式：

下载此[工作流程](https://link.zhihu.com/?target=https%3A//github.com/ltdrdata/ComfyUI-extension-tutorials/blob/Main/ComfyUI-Impact-Pack/workflow/simple.png)，在 ComfyUI 中加载。

选择模型，输入提示词，调整采样器参数，运行工作流程。

图生图方式：

下载此[工作流程](https://link.zhihu.com/?target=https%3A//stable-diffusion-art.com/wp-content/uploads/2024/01/inpaint-detailer-face-from-image.json)，在 ComfyUI 中加载。

![](img/236e265f6c9bc65c861cbfbeacf733cb.png)

将图片上传到“加载图像”节点，输入提示词，调整采样器参数，运行工作流程。

重点参数

引导大小：如果检测到的人脸区域小于引导大小时，此功能会放大人脸区域，使短边达到引导大小的值。如果人脸区域大于引导大小，图片不会重绘。

引导目标：该参数决定放大图片时，是把裁剪区域放大到“引导大小”，还是把检测到的矩形框放大到“引导大小”。

最大尺寸：放大人脸区域时，限制边长的最大尺寸。

羽化：将修复的人脸合成到原始图像上时，边缘的柔和程度。

仅生成遮罩：启用时，采样器仅重绘图像的遮罩区域。禁用时，采样器会重绘整个裁剪区域。

强制重绘：启用时，即使检测到的图片区域大于引导大小，采样器也会重绘图片。在这种情况下，检测区域不会被放大。

阈值：仅检测那些识别的置信度高于此设定值的对象。

膨胀：扩展遮罩区域。

[裁剪系数](https://zhida.zhihu.com/search?q=%E8%A3%81%E5%89%AA%E7%B3%BB%E6%95%B0&zhida_source=entity&is_preview=1)：扩展裁剪区域，把人脸周围的像素包含进来。如果该值很小，则恢复可能效果不好，因为无法了解周围的环境。

### 手部修复

最简单的手部修复工作流和前面的面部修复工作流一样，只需把“检测加载器”节点里的模型名称换成“hand_yolov8s.pt”即可。

你也可以在这里下载更复杂的手部修复工作流程：

[ht](https://link.zhihu.com/?target=https%3A//openart.ai/workflows/oliviosarikas/mesh-graphormer-hand-fixing-controlnet/NkhzwEW80FzCcvzzXEsH)tps://openart.ai/workflows/oliviosarikas/mesh-graphormer-hand-fixing-controlnet/NkhzwEW80FzCcvzzXEsH

此工作流程使用HandRefiner和ControlNet来检测手部区域和深度，然后重绘，修复效果更好。

![](img/70949c2c11cfd80f6c9ac359fef8e7e4.png)

[要正常运行这个工作流程，你需要在这里下载3个](https://link.zhihu.com/?target=https%3A//huggingface.co/hr16/ControlNet-HandRefiner-pruned/tree/main)模型：hr16/ControlNet-HandRefiner-pruned at main

把control_sd15_inpaint_depth_hand_fp16模型放入ComfyUI\models\controlnet，其他两个模型放入ComfyUI\custom_nodes\comfyui_controlnet_aux\ckpts\hr16\ControlNet-HandRefiner-pruned。

### 图片扩展

[扩展与修](https://zhida.zhihu.com/search?q=%E5%A4%96%E8%A1%A5%E7%94%BB%E6%9D%BF&zhida_source=entity&is_preview=1)复本质上是一样的，都是重绘某个区域，ComfyUI中有一个“外补画板”节点，可以自动给扩展的像素添加遮罩，然后重绘遮罩区域，从而实现图片扩展。

下载[此页面](https://link.zhihu.com/?target=https%3A//comfyanonymous.github.io/ComfyUI_examples/inpaint/)上的最后一张图片并将其拖放到 ComfyUI 中进行加载。

手动创建方式：在此[修复工作流程](https://link.zhihu.com/?target=https%3A//stable-diffusion-art.com/wp-content/uploads/2024/01/inpaint-sd15-inpaint-model.json)的基础上添加“外补画板”节点，该节点应位于“加载图像”和“VAE内补编码器”节点之间。

![](img/e1b7d430858ab54278a0341e0c2b5748.png)

使用方法：选择模型，输入提示词，调整采样器参数，在“外[补画板](https://zhida.zhihu.com/search?q=%E7%BE%BD%E5%8C%96%E5%80%BC&zhida_source=entity&is_preview=1)”节点设置每个方向需要扩展的像素和羽化值，运行工作流程。

## 图片放大

### 高分辨率修复（潜空间放大）

下载[此页面](https://link.zhihu.com/?target=https%3A//comfyanonymous.github.io/ComfyUI_examples/2_pass_txt2img/)上的第一张图片，并将其放入 ComfyUI 中，以加载高分辨率修复工作流程。

![](img/a89d234cf1348fb08a624e6744a5b64c.png)

前面的流程和文生图相同，然后，工作流程会放大潜空间中的图像并执行一些额外的采样，给图像添加一些初始噪声，并以一定的去噪强度对其进行去噪。然后，VAE解码器对较大的潜空间的图像进行解码以生成放大的图像。

[如果你](https://link.zhihu.com/?target=https%3A//comfyanonymous.github.io/ComfyUI_examples/2_pass_txt2img/)想使用放大算法模型放大潜空间中的图像，请下载此页面上的第二张图片，并将其放入 ComfyUI 中，以加载高分辨率修复工作流程。

### SD Ultimate Upscale

节点项目主页：[https://github.com/ssitu/Comfy](https://link.zhihu.com/?target=https%3A//github.com/ssitu/ComfyUI_UltimateSDUpscale)UI_UltimateSDUpscale

在默认的文生图工作流程中，右键单击空白处。选择“新建节点”>“图像”>“放大”>“SD放大”。

按如下方式连接：

+   图像输入端口连接VAE解码的图像或者连接加载图像的图像。

+   模型输入端口连接Checkpoint加载器的模型。

+   [正面条件和负面](https://zhida.zhihu.com/search?q=%E8%B4%9F%E9%9D%A2%E6%96%87%E6%9C%AC%E7%BC%96%E7%A0%81%E5%99%A8&zhida_source=entity&is_preview=1)条件输入端口分别连接正面文本编码器和负面文本编码器的条件

+   vae连接Checkpoint加载器的VAE。

+   放大模型连接放大模型加载器。

+   图像连接保存图像或预览图像。

结合ControlNet tile模型放大（推荐）

在前面的工作流基础上：

+   添加“ControlNet应用（高级） ”和“ControlNet加载器 ”节点，

+   条件输入端连接文本编码器的输出口，

+   ControlNet输入端连接ControlNet加载器的输出口，

+   图像输入端连接VAE解码的图像或者连接加载图像的图像。

![](img/b250cbb48e8c2d3ac8bf1f8dfb63c81e.png)

重点参数

模式类型：

+   线性：所有图块按逐列、逐行的顺序一一处理。

+   棋盘：所有分块以棋盘图案顺序处理，减少接缝的可能性。

+   无：禁用重绘。只运行接缝修复时才选择它，需要将放大的图像作为源图像。

分块宽度：要处理的分块的宽度。该值越大，最终图像中的接缝就越少。

分块高度：要处理的分块的高度。默认值为 0，在这种情况下它等于分块宽度。

蒙版模糊：分块图片的遮罩边缘的模糊值。在 512-768px 图块大小上将其设置为 12-16，如果还存在接缝，请增加该值。

填充：汉化插件翻译为“分块分区”。该值代表处理分块时将考虑周围区域的多少像素。

接缝修复模式：

+   Bands pass：它仅在接缝（行和列）上添加噪声并重绘。

+   Half tile：它添加了 2 个通道（如重绘通道），但具有半分块偏移。具有垂直梯度掩码的行的一次传递和具有水平梯度掩码的列的一次传递。此通道比Bands pass覆盖更大的区域，并且通常会产生更好的结果，但需要更多时间。

+   Half tile + intersections：它运行半分块偏移通道，然后在具有径向渐变掩模的交叉点上运行额外通道。

+   无：不使用接缝修复。

接缝修复降噪：接缝修复的重绘强度。

接缝修复宽度：重绘接缝的宽度。仅在“Bands pass”修复模式有效。

接缝修复填充：汉化插件翻译为“接缝修复分区”，该值代表处理分块时将考虑周围区域的多少像素。

接缝修复蒙版模糊：接缝的遮罩边缘的模糊值。在 512-768px 图块大小上将其设置为 12-16。如果还存在接缝，请增加该值。

强制统一分块：如果启用，当某个图块的宽度或高度小于分块宽度的设定值，该图块将使用图像的其余部分扩展图块，以保持和其他图块一样的宽度和高度。如果禁用，将使用图块的最小尺寸，这可能会使采样速度更快，但可能会由于不规则的图块尺寸而导致伪影。

### AI upscaler

AI upscaler是一种用于放[大图片并](https://zhida.zhihu.com/search?q=%E6%89%A9%E6%95%A3%E6%A8%A1%E5%9E%8B&zhida_source=entity&is_preview=1)填充细节的 AI 模型，它们不是扩散模型，而是经过训练以放大图片的神经网络，所以使用这种方法放大补充的细节很少，效果远不如前面两种方式。

下载此[放大工作流程](https://link.zhihu.com/?target=https%3A//comfyanonymous.github.io/ComfyUI_examples/upscale_models/)，将工作流程图片拖放到 ComfyUI 中进行加载。

![](img/5d317a3e2ad1cd93fac0050294315eaf.png)

这个工作流程只在文生图基础上添加了两个节点：

+   加载放大模型：用于加载 AI 放大模型。（该节点位于 添加节点>加载器）

+   放大图像（使用模型）：该节点位于 VAE 解码器和保存图像节点之间，它输入图像和放大模型，并输出放大的图像。（该节点位于 添加节点>图像>放大）

要使用此工作流程，你必须从[Upscaler Wiki](https://link.zhihu.com/?target=https%3A//upscale.wiki/w/index.php%3Ftitle%3DModel_Database%26oldid%3D1571)下载放大模型，并把模型放入文件夹models > upscale_models，或者将 ComfyUI 设置为共享 AUTOMATIC1111 的模型文件。

## 使用Embedding

要在 ComfyUI 中使用Embedding（也称为文本反转），请在正向或反向提示框中输入“embedding:模型名称”，例如：

```
embedding: BadDream
```

ComfyUI 将搜索文件夹ComfyUI > models > embeddings中具有相同文件名的Embedding。

输入完整的文件名很麻烦，还可能输错，如果你安装了ComfyUI-Custom-Scripts自定义节点，它会启用Embedding名称的自动查找功能，输入embedding后，旁边会显示可用的Embedding列表。

![](img/b173b45c25a73ea61f4894ced0caf893.png)

## 使用LoRA

下载此[LoRA 工作流程](https://link.zhihu.com/?target=https%3A//comfyanonymous.github.io/ComfyUI_examples/lora/lora.png)，将工作流程拖放到 ComfyUI 中进行加载。

手动创建方式：加载默认的文生图工作流，鼠标右键点击“Checkpoint加载器(简易)”，选择“添加LoRA”，“LoRA加载器”节点会在连接完好的情况下添加到工作流。这个快捷功能由ComfyUI-Custom-Scripts插件提供。

![](img/2b65e060c18408dd219b6b456efdfb57.png)

如果要同时使用多个LoRA，只需把多个[“LoRA](https://link.zhihu.com/?target=https%3A//comfyanonymous.github.io/ComfyUI_examples/lora/lora_multiple.png)加载器”串联起来即可，可以参考此工作流程。

## 使用ControlNet

手动创建ControlNet工作流：

加载默认工作流，

在“K采样器”前面添加“ControlNet应用(高级)”节点和“ControlNet加载器”节点。

添加“加载图像”节点。

正面条件和负面条件输入端分别连接两个文本编码器的输出端。

ControlNet输入端连接“ControlNet加载器”节点的输出端。

图像输入端连接“加载图像”节点的输出端。

以下是使用ControlNet的tile模型控制画面结构的示例。

![](img/658967bb37f39ab0c748ed34a694e2ad.png)

可以看到，如果输入的图片的比例和“空Latent”节点设置的比例不同，输入的图片会被裁剪。

如果图片需要预处理，需要在“加载图像”节点和“ControlNet应用(高级)”节点的连线之间插入一个预处理器节点。

预处理器需要使用comfyui_controlnet_aux提供的节点，官方没有ControlNet预处理器，就很奇怪。

以下是使用ControlNet的Canny模型控制线条的示例。

![](img/f583de66e2053c4ea2c9d5137116a499.png)

这里使用的预处理器节点为“Canny细致线预处理器”。不同的[预处理方式要](https://link.zhihu.com/?target=https%3A//github.com/Fannovel16/comfyui_controlnet_aux%3Ftab%3Dreadme-ov-file%23nodes-sections-are-categories-in-comfy-menu)用不同的节点，节点名称可参考插件项目文档。

如果要同时使用多个ControlNet，只需把多个“ControlNet应用(高级)”节点串联起来即可。

## 其他工作流程

### SDXL工作流程

SDXL是一种二阶段的级联扩散模型，包括Base模型和Refiner模型。其中Base模型的主要工作和Stable Diffusion 1.x-2.x一致，Refiner模型对Base模型生成的图像Latent特征进行精细化提升，其本质上是在做图生图的工作。

SDXL使用了两个文本编码器，分别是OpenCLIP ViT-bigG（1.39G）和OpenAI CLIP ViT-L（246M），从而大大增强了Stable Diffusion XL对文本的提取和理解能力。

如果你只使用基本模型，可以下载基本SDXL工作流程(仅基本模型)

![](img/f0d4a67549b24d5f2ddabe818eb8a5fd.png)

从工作流程中可以看到，这个工作流程和SD1.5工作流程的唯一区别在于，提示词输入框从文本编码器节点独立出来，并同时连接到了文本编码器节点的“G文本”和“L文本”，分别对应基本模型的OpenCLIP ViT-bigG和OpenAI CLIP ViT-L这两个文本编码器。

如果你要同时使用基本模型和Refiner模型，可以下载基本SDXL工作流程(基本模型+Refiner模型)

![](img/d4b6058dfaa1b4a3d24b77fae5418426.png)

从工作流程中可以看到，有两个模型加载器节点和两个K采样器节点，还有两个设置降噪步数的节点，分别是总步数和基本模型的结束步数（也是Refiner模型的开始步数），Refiner模型只有一个文本编码器，所以它的文本编码器节点和基本模型的文本编码器节点不一样。

SDXL的其他工作流程和SD1.5的工作流程是类似的，我就不介绍了。

### 使用外部VAE

添加“VAE加载器”节点（新建节点>加载器>VAE加载器），然后连接两个VAE端口。

以下是在文生图工作流程使用外部VAE的示例。

![](img/f4b03b74c20f87e9206eb1df9aaff9a1.png)

VAE模型负责将潜空间图像转换到像素空间，该过程是“有损的”，所以良好的 VAE 模型对于获得高质量图像至关重要。

### 设置CLIP停止层

在“Checkpoint加载器(简易)”节点和“CLIP文本编码器”节点之间插入“CLIP设置停止层”节点（新建节点>条件>CLIP设置停止层），然后连接CLIP输入端和输出端。

![](img/fc97906da738d549c1f7cb9d7b3a1bf8.png)

### 设置批次大小

文生图工作流程可以直接在“空Latent”节点设置批次大小。

在图生图工作流程中，需要在“VAE编码”节点和“K采样器”节点之间插入“复制Latent批次”节点（新建节点>Latent>批处理>复制Latent批次），然后连接Latent输入端和输出端。

![](img/32f609766848fbae794c332a9788730a.png)

注意，这里的批次大小是模型同时生成图片的数量，批次设置太大会消耗大量显存。如果要依次生成多张图片，可以点击“更多选项”>“批次数量”，也可以多次点击“添加提示词队列”。

## 常用快捷键和技巧

生成图像：按Ctrl+Enter将工作流程放入队列并生成图像。

复制节点：选择一个节点，按Ctrl+C进行复制，Ctrl+V进行粘贴，Ctrl+Shift+V可在输入连接完好的情况下进行粘贴。另一种方式是，按住Alt键，然后点击并拖动节点。

移动多个节点：创建一个组，将一组节点放到一起，然后移动组。或者，按住 Ctrl 并拖动鼠标，创建一个框以选择多个节点，你也可以按住 Ctrl 键选择多个节点，然后按住 Shift 并拖动鼠标。

忽略节点：选择一个节点，然后按 Ctrl+B 。没有键盘快捷键可以忽略一个组中的全部节点，你可以在右键菜单中选择“忽略该组节点”。忽略节点只是不执行该节点的功能，不影响连接线的完整性。

禁用节点：选择一个节点，然后按 Ctrl+M 。禁用节点会使该节点的所有连接线断开，你可能需要重新连接端口才能运行工作流。

最小化节点：单击节点左上角的点可将其最小化。

嵌入工作流程：ComfyUI生成图片时会把工作流程保存到图片文件的元数据中，你也可以把当前工作区保存为一个图片文件，鼠标右键>工作流图像>导出>PNG/SVG。

## 缓存图片的文件夹

用户上传到“加载图像”节点的图片位于ComfyUI\input，不会自动删除，记得定期清理。

“预览图像”节点的图片位于ComfyUI\temp，重启后清空。

“保存图像”节点的图片位于ComfyUI\output

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>