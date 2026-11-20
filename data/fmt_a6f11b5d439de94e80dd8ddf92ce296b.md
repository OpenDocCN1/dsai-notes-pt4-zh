
# 从 RCNN 到 SSD，这应该是最全的一份目标检测算法盘点

**作者：Jonathan Hui**

**机器之心编译**

> 目标检测是很多计算机视觉任务的基础，不论我们需要实现图像与文字的交互还是需要识别精细类别，它都提供了可靠的信息。本文对目标检测进行了整体回顾，第一部分从 RCNN 开始介绍基于候选区域的目标检测器，包括 Fast R-CNN、Faster R-CNN 和 FPN 等。第二部分则重点讨论了包括 YOLO、SSD 和 RetinaNet 等在内的单次检测器，它们都是目前最为优秀的方法。

机器之心之前已经讨论过非常多的目标检测算法，对计算机视觉感兴趣的读者也可以结合以前的文章加强理解。

* [深度学习目标检测模型全面综述：Faster R-CNN、R-FCN 和 SSD](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650730940&idx=1&sn=2a6a8520176368d467ca87fbc2e04c66&chksm=871b35c2b06cbcd41593cf07aa5b7a7d0913f5d07de708fd554059b9f20f1b332190dd934025&scene=21#wechat_redirect)

* [从零开始 PyTorch 项目：YOLO v3 目标检测实现](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650741259&idx=1&sn=03dfb0fa3396e5464fc358b5a803e7bf&chksm=871ade75b06d5763a45f3c5da1ca62023a13c5cf7ce52a0a23e6c320f129f79bb9b3be4d2da0&scene=21#wechat_redirect)

* [像玩乐高一样拆解 Faster R-CNN：详解目标检测的实现过程](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650738167&idx=3&sn=dc26c3d273833192550290327d2c6220&chksm=871ac989b06d409f89bd2a1ea99ed415a4118482805f05e692bef968a65301fc596957606fde&scene=21#wechat_redirect)

* [后 RCNN 时代的物体检测及实例分割进展](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650736740&idx=3&sn=cdce446703e69b47cf48f12b3d451afc&chksm=871acc1ab06d450ccde3148df96436c98adb2de3b6a34559b95af322c5186513460329dc20bd&scene=21#wechat_redirect)

* [物体检测算法全概述：从传统检测方法到深度神经网络框架](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650725146&idx=3&sn=453e29cb6179e8e06df2133269c20812&chksm=871b1f64b06c967276274cee90f5a036c09b08cec9e52111af6744c7f0c19d5a8c7e0eb06e0b&scene=21#wechat_redirect)

## 基于候选区域的目标检测器

### 滑动窗口检测器

自从 AlexNet 获得 ILSVRC 2012 挑战赛冠军后，用 CNN 进行分类成为主流。一种用于目标检测的暴力方法是从左到右、从上到下滑动窗口，利用分类识别目标。为了在不同观察距离处检测不同的目标类型，我们使用不同大小和宽高比的窗口。

![](img/03082b6a784a3784457603940cffd820-fs8.png)

*滑动窗口（从右到左，从上到下）*

我们根据滑动窗口从图像中剪切图像块。由于很多分类器只取固定大小的图像，因此这些图像块是经过变形转换的。但是，这不影响分类准确率，因为分类器可以处理变形后的图像。

![](img/d2d6a61b01836b86ce60ec227ab4acc4-fs8.png)

*将图像变形转换成固定大小的图像*

变形图像块被输入 CNN 分类器中，提取出 4096 个特征。之后，我们使用 SVM 分类器识别类别和该边界框的另一个线性回归器。

![](img/f803c14ba28b05ca7a9a75cede1886be-fs8.png)

*滑动窗口检测器的系统工作流程图。*

下面是伪代码。我们创建很多窗口来检测不同位置的不同目标。要提升性能，一个显而易见的办法就是减少窗口数量。

```
for window in windows
    patch = get_patch(image, window)
    results = detector(patch) 
```

### 选择性搜索

我们不使用暴力方法，而是用候选区域方法（region proposal method）创建目标检测的感兴趣区域（ROI）。在选择性搜索（selective search，SS）中，我们首先将每个像素作为一组。然后，计算每一组的纹理，并将两个最接近的组结合起来。但是为了避免单个区域吞噬其他区域，我们首先对较小的组进行分组。我们继续合并区域，直到所有区域都结合在一起。下图第一行展示了如何使区域增长，第二行中的蓝色矩形代表合并过程中所有可能的 ROI。

![](img/07cc7494e42f89abbe62d32b3a5e9a30-fs8.png)

*图源：van de Sande et al. ICCV'11*

### R-CNN

R-CNN 利用候选区域方法创建了约 2000 个 ROI。这些区域被转换为固定大小的图像，并分别馈送到卷积神经网络中。该网络架构后面会跟几个全连接层，以实现目标分类并提炼边界框。

![](img/541063389142b0f6cf287e8d4feee8db-fs8.png)

*使用候选区域、CNN、仿射层来定位目标。*

以下是 R-CNN 整个系统的流程图：

![](img/821f8bc96c138826603931789f608bf2-fs8.png)

通过使用更少且更高质量的 ROI，R-CNN 要比滑动窗口方法更快速、更准确。

```
ROIs = region_proposal(image)
for ROI in ROIs
    patch = get_patch(image, ROI)
    results = detector(patch) 
```

#### 边界框回归器

候选区域方法有非常高的计算复杂度。为了加速这个过程，我们通常会使用计算量较少的候选区域选择方法构建 ROI，并在后面使用线性回归器（使用全连接层）进一步提炼边界框。

![](img/8440e153473ea171174771e5d086b6f0-fs8.png)

*使用回归方法将蓝色的原始边界框提炼为红色。*

### Fast R-CNN

R-CNN 需要非常多的候选区域以提升准确度，但其实有很多区域是彼此重叠的，因此 R-CNN 的训练和推断速度非常慢。如果我们有 2000 个候选区域，且每一个都需要独立地馈送到 CNN 中，那么对于不同的 ROI，我们需要重复提取 2000 次特征。

此外，CNN 中的特征图以一种密集的方式表征空间特征，那么我们能直接使用特征图代替原图来检测目标吗？

![](img/77b0b8d01c9d93988774924822862d34-fs8.png)

![](img/85f8e4a154b6425e5ad4ef83f627d31b-fs8.png)

*直接利用特征图计算 ROI。*

Fast R-CNN 使用特征提取器（CNN）先提取整个图像的特征，而不是从头开始对每个图像块提取多次。然后，我们可以将创建候选区域的方法直接应用到提取到的特征图上。例如，Fast R-CNN 选择了 VGG16 中的卷积层 `conv5` 来生成 ROI，这些关注区域随后会结合对应的特征图以裁剪为特征图块，并用于目标检测任务中。我们使用 ROI 池化将特征图块转换为固定的大小，并馈送到全连接层进行分类和定位。因为 Fast-RCNN 不会重复提取特征，因此它能显著地减少处理时间。

![](img/bc695e013980fe3360e3cb173a72d379-fs8.png)

*将候选区域直接应用于特征图，并使用 ROI 池化将其转化为固定大小的特征图块。*

以下是 Fast R-CNN 的流程图：

![](img/3894154fd0775cff6725c601508ab6fa-fs8.png)

在下面的伪代码中，计算量巨大的特征提取过程从 For 循环中移出来了，因此速度得到显著提升。Fast R-CNN 的训练速度是 R-CNN 的 10 倍，推断速度是后者的 150 倍。

```
feature_maps = process(image)
ROIs = region_proposal(feature_maps)
for ROI in ROIs
    patch = roi_pooling(feature_maps, ROI)
    results = detector2(patch) 
```

Fast R-CNN 最重要的一点就是包含特征提取器、分类器和边界框回归器在内的整个网络能通过多任务损失函数进行端到端的训练，这种多任务损失即结合了分类损失和定位损失的方法，大大提升了模型准确度。

#### ROI 池化

因为 Fast R-CNN 使用全连接层，所以我们应用 ROI 池化将不同大小的 ROI 转换为固定大小。

为简洁起见，我们先将 8×8 特征图转换为预定义的 2×2 大小。

* 下图左上角：特征图。

* 右上角：将 ROI（蓝色区域）与特征图重叠。

* 左下角：将 ROI 拆分为目标维度。例如，对于 2×2 目标，我们将 ROI 分割为 4 个大小相似或相等的部分。

* 右下角：找到每个部分的最大值，得到变换后的特征图。

![](img/9445324e347192dee26f3807cd104889-fs8.png)

*输入特征图（左上），输出特征图（右下），ROI (右上，蓝色框）。*

按上述步骤得到一个 2×2 的特征图块，可以馈送至分类器和边界框回归器中。

### Faster R-CNN

Fast R-CNN 依赖于外部候选区域方法，如选择性搜索。但这些算法在 CPU 上运行且速度很慢。在测试中，Fast R-CNN 需要 2.3 秒来进行预测，其中 2 秒用于生成 2000 个 ROI。

```
feature_maps = process(image)
ROIs = region_proposal(feature_maps)         # Expensive!
for ROI in ROIs
    patch = roi_pooling(feature_maps, ROI)
    results = detector2(patch) 
```

Faster R-CNN 采用与 Fast R-CNN 相同的设计，只是它用内部深层网络代替了候选区域方法。新的候选区域网络（RPN）在生成 ROI 时效率更高，并且以每幅图像 10 毫秒的速度运行。

![](img/642a5476bbbab09fcf1fff2dda993157-fs8.png)

*Faster R-CNN 的流程图与 Fast R-CNN 相同。*

![](img/38e843108f498903fdccacf0d81c4f4c-fs8.png)

*外部候选区域方法代替了内部深层网络。*

#### 候选区域网络

候选区域网络（RPN）将第一个卷积网络的输出特征图作为输入。它在特征图上滑动一个 3×3 的卷积核，以使用卷积网络（如下所示的 ZF 网络）构建与类别无关的候选区域。其他深度网络（如 VGG 或 ResNet）可用于更全面的特征提取，但这需要以速度为代价。ZF 网络最后会输出 256 个值，它们将馈送到两个独立的全连接层，以预测边界框和两个 objectness 分数，这两个 objectness 分数度量了边界框是否包含目标。我们其实可以使用回归器计算单个 objectness 分数，但为简洁起见，Faster R-CNN 使用只有两个类别的分类器：即带有目标的类别和不带有目标的类别。

![](img/eb1a945fbaa481df2b7b2fc9ab9eb36a-fs8.png)

对于特征图中的每一个位置，RPN 会做 `k` 次预测。因此，RPN 将输出 4×`k` 个坐标和每个位置上 2×`k` 个得分。下图展示了 8×8 的特征图，且有一个 3×3 的卷积核执行运算，它最后输出 8×8×3 个 ROI（其中 `k`=3）。下图（右）展示了单个位置的 3 个候选区域。

![](img/c170d19e1094e3d2f517d41997f46e4f-fs8.png)

此处有 3 种猜想，稍后我们将予以完善。由于只需要一个正确猜想，因此我们最初的猜想最好涵盖不同的形状和大小。因此，Faster R-CNN 不会创建随机边界框。相反，它会预测一些与左上角名为「锚点」的参考框相关的偏移量（如𝛿x、𝛿y）。我们限制这些偏移量的值，因此我们的猜想仍然类似于锚点。

![](img/3f8b7065cfe50c98510769add7ef3310-fs8.png)

要对每个位置进行 `k` 个预测，我们需要以每个位置为中心的 `k` 个锚点。每个预测与特定锚点相关联，但不同位置共享相同形状的锚点。

![](img/2e08bc3d22c1253aac01ad91c9cc80e9-fs8.png)

这些锚点是精心挑选的，因此它们是多样的，且覆盖具有不同比例和宽高比的现实目标。这使得我们可以以更好的猜想来指导初始训练，并允许每个预测专门用于特定的形状。该策略使早期训练更加稳定和简便。

Faster R-CNN 使用更多的锚点。它部署 9 个锚点框：3 个不同宽高比的 3 个不同大小的锚点框。每一个位置使用 9 个锚点，每个位置会生成 2×9 个 objectness 分数和 4×9 个坐标。

*![](img/3631b95bfb8d78b60dd48b448de86c37-fs8.png)图源：https://arxiv.org/pdf/1506.01497.pdf*

### R-CNN 方法的性能

如下图所示，Faster R-CNN 的速度要快得多。

![](img/0231ed36fdf41b6243586cffb669a7b2-fs8.png)

## 基于区域的全卷积神经网络（R-FCN）

假设我们只有一个特征图用来检测右眼。那么我们可以使用它定位人脸吗？应该可以。因为右眼应该在人脸图像的左上角，所以我们可以利用这一点定位整个人脸。

![](img/15bf51b2e11139521376994b86d6d9b9-fs8.png)

如果我们还有其他用来检测左眼、鼻子或嘴巴的特征图，那么我们可以将检测结果结合起来，更好地定位人脸。

现在我们回顾一下所有问题。在 Faster R-CNN 中，检测器使用了多个全连接层进行预测。如果有 2000 个 ROI，那么成本非常高。

```
feature_maps = process(image)
ROIs = region_proposal(feature_maps)
for ROI in ROIs
    patch = roi_pooling(feature_maps, ROI)
    class_scores, box = detector(patch)         # Expensive!
    class_probabilities = softmax(class_scores) 
```

R-FCN 通过减少每个 ROI 所需的工作量实现加速。上面基于区域的特征图与 ROI 是独立的，可以在每个 ROI 之外单独计算。剩下的工作就比较简单了，因此 R-FCN 的速度比 Faster R-CNN 快。

```
feature_maps = process(image)
ROIs = region_proposal(feature_maps)         
score_maps = compute_score_map(feature_maps)
for ROI in ROIs
    V = region_roi_pool(score_maps, ROI)     
    class_scores, box = average(V)                   # Much simpler!
    class_probabilities = softmax(class_scores) 
```

现在我们来看一下 5 × 5 的特征图 `M`，内部包含一个蓝色方块。我们将方块平均分成 3 × 3 个区域。现在，我们在 `M` 中创建了一个新的特征图，来检测方块的左上角（TL）。这个新的特征图如下图（右）所示。只有黄色的网格单元 `[2, 2]` 处于激活状态。

![](img/a0452133717ed1c77857913a4582b12c-fs8.png)

*在左侧创建一个新的特征图，用于检测目标的左上角。*

我们将方块分成 9 个部分，由此创建了 9 个特征图，每个用来检测对应的目标区域。这些特征图叫作位置敏感得分图（position-sensitive score map），因为每个图检测目标的子区域（计算其得分）。

![](img/27d0c3b20deff617661fea94c5d3d872-fs8.png)

*生成 9 个得分图*

下图中红色虚线矩形是建议的 ROI。我们将其分割成 3 × 3 个区域，并询问每个区域包含目标对应部分的概率是多少。例如，左上角 ROI 区域包含左眼的概率。我们将结果存储成 3 × 3 `vote` 数组，如下图（右）所示。例如，`vote_array[0][0]` 包含左上角区域是否包含目标对应部分的得分。

![](img/9de69d101465d234f65699fe2482e329-fs8.png)
