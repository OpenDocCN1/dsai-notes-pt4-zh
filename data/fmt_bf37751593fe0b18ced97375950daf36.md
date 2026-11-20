
# AI模型部署 | onnxruntime部署RT-DETR目标检测模型

> 来源：[https://geekbang.feishu.cn/docx/Lr37d3zpnokFQGxQqphcxqPCnWb](https://geekbang.feishu.cn/docx/Lr37d3zpnokFQGxQqphcxqPCnWb)
>
> 👨🏻 作者：一天到晚潜水的鱼
>
> 📃 作者介绍：分享计算机视觉、机器学习、深度学习、无人驾驶等内容
>
> 🛖 👸号：DeepDriving
>
> ⏰ 收录时间：2024-05-27 11:20:58 (UTC+8)
>
> 🔗 原文链接：https://mp.weixin.qq.com/s?__biz=MzkwNjE2ODMyMQ==&mid=2247485069&idx=1&sn=1edb761f613d6c2bda1f5c6e1ef1fa1e&chksm=c0edd7e3f79a5ef588e3f256a9e4ba2557da13b5c192951f9a2027e85622a907c2ccd66864dc&token=1923064180&lang=zh_CN#rd

![](img/53001bd768d66d58deab5eb89e693118.png)

关注作者，获取更多AI知识

## 0. 引言

RT-DETR 是百度开源的一个基于 DETR 架构的实时端到端目标检测算法，在速度和精度上均超过了 YOLOv5 、 YOLOv8 等 YOLO 系列检测算法，目前在 YOLOv8 的官方代码仓库 ultralytics 中也已支持 RT-DETR 算法。

在上一篇文章《[AI模型部署 | onnxruntime部署YOLOv8分割模型详细教程](https://mp.weixin.qq.com/s?__biz=MzkwNjE2ODMyMQ==&mid=2247485038&idx=1&sn=839b4a9d8ae1cb06a46d6739dc0c4405&chksm=c0edd700f79a5e16c3b2c5096458346df9a0eb9bc12bc4530129ed9ade1322bb5fc12d5897cb&token=448277125&lang=zh_CN&scene=21#wechat_redirect)》中我介绍了如何使用`onnxruntime`框架来部署 YOLOv8 分割模型，本文将介绍如何使用 `onnxruntime` 框架来部署 RT-DETR 模型，代码还是采用 Python 实现。

## 1. 准备工作

### 安装 `onnxruntime`

`onnxruntime` 分为 GPU 版本和 CPU 版本，均可以通过 `pip` 直接安装：

```
pip install onnxruntime-gpu  # 安装GPU版本
pip install onnxruntime  # 安装CPU版本
```

**注意：** GPU 版本和 CPU 版本建议只选其中一个安装，否则默认会使用 CPU 版本。

### 下载 RT-DETR 模型

Ultralytics 官方提供了用 COCO 数据集训练的 RT-DETR 模型权重，我们可以直接从 GitHub 网站 `https://github.com/ultralytics/assets/releases` 下载使用，本文使用的模型为 `rtdetr-l.pt` 。

### 转换为 `onnx` 模型

调用下面的命令可以把 `rtdetr-l.pt` 模型转换为 `onnx` 格式的模型：

```
yolo export model=rtdetr-l.pt format=onnx opset=17 simplify=True
```

转换成功后得到的模型为 `rtdetr-l.onnx` 。这里设置 `opset=17` 是为了能够直接导出 `LayerNormalization` 算子，因为 `onnx` 从版本 17 开始才支持该算子。如果 `opset` 小于 17 ，那么导出模型的时候会把 `LayerNormalization` 算子拆分成一系列子算子进行导出。下面两幅图分别是设置 `opset=16` 和 `opset=17` 导出的 `onnx` 模型中的结构：

`opset=16`，红色框内是把 `LayerNormalization` 进行拆分的结果：

![](img/bcf3460b6ac9756238ef2e0708173fa0.png)

`layernormalization_opset16`

`opset=17`：

![](img/3cf65ce6316b3a7010ce5e2a0ac18314.png)

`layernormalization_opset17`

另外，这里设置 `simplify=True` 是为了调用 `onnx-simplifier` 工具对模型进行简化，否则生成的模型有一大堆零碎的算子，看起来就很麻烦。

## 2. 模型部署

### 2.1 加载 `onnx` 模型

首先导入 `onnxruntime` 包，然后调用其 API 加载模型即可：

```
import onnxruntime as ort
session = ort.InferenceSession("rtdetr-l.onnx", providers=["CUDAExecutionProvider"])
```

因为我使用的是 GPU 版本的 `onnxruntime` ，所以 `providers` 参数设置的是 `"CUDAExecutionProvider"` ；如果是 CPU 版本，则需设置为 `"CPUExecutionProvider"` 。

模型加载成功后，我们可以查看一下模型的输入、输出层的属性：

```
for input in session.get_inputs():
    print("input name: ", input.name)
    print("input shape: ", input.shape)
    print("input type: ", input.type)
for output in session.get_outputs():
    print("output name: ", output.name)
    print("output shape: ", output.shape)
    print("output type: ", output.type)
```

结果如下：

```
input name: images
input shape: [1, 3, 640, 640]
input type: tensor(float)
output name: output0
output shape: [1, 300, 84]
output type: tensor(float)
```

从上面的打印信息可以知道，模型有一个尺寸为 `[1, 3, 640, 640]` 的输入层和一个尺寸分别为 `[1, 300, 84]` 的输出层。

### 2.2 数据预处理

数据预处理采用 OpenCV 和 Numpy 实现，首先导入这两个包：

```
import cv2
import numpy as np
```

用 OpenCV 读取图片后，把数据按照与 YOLOv8 一样的要求做预处理：

```
image = cv2.imread("soccer.jpg")
image_height, image_width, _ = image.shape
input_tensor = prepare_input(image, model_width, model_height)
print("input_tensor shape: ", input_tensor.shape)
```

其中预处理函数 `prepare_input` 的实现如下：

```
def prepare_input(bgr_image, width, height):
    image = cv2.cvtColor(bgr_image, cv2.COLOR_BGR2RGB)
    image = cv2.resize(image, (width, height)).astype(np.float32)
    image = image / 255.0
    image = np.transpose(image, (2, 0, 1))
    input_tensor = np.expand_dims(image, axis=0)
    return input_tensor
```

处理流程如下：

```
1. 把OpenCV读取的BGR格式的图片转换为RGB格式；
2. 把图片resize到模型输入尺寸640x640；
3. 对像素值除以255做归一化操作；
4. 把图像数据的通道顺序由HWC调整为CHW；
5. 扩展数据维度，将数据的维度调整为NCHW。
```

经过预处理后，输入数据 `input_tensor` 的维度变为 `[1, 3, 640, 640]` ，与模型的输入尺寸一致。

### 2.3 模型推理

输入数据准备好以后，就可以送入模型进行推理：

```
outputs = session.run(None, {session.get_inputs()[0].name: input_tensor})
```

前面我们打印了模型的输入输出属性，可以知道模型只有一个输出分支，所以只要取 `outputs[0]` 的数据进行处理：

```
output = np.squeeze(outputs[0])
print("output shape: ", output.shape)
```

```
output shape:  (300, 84)
```

### 2.4 后处理

从前文知道模型输出的维度为 `300x84` ，其中 `300` 表示模型在一张图片上最多能检测的目标数量， `84` 表示每个目标的参数包含 4 个坐标属性（`cx`, `cy`, `w`, `h`）和 80 个类别置信度。每个目标的坐标信息都是做了归一化的，只要乘以原始图像的尺寸就可以把坐标恢复到在原始图像中的大小。 RT-DETR 的检测结果不需要再做 NMS 这些额外的后处理操作。后处理的代码如下：

```
for out in output:
    confidence = out[4:].max()
    if confidence < 0.5:
        continue
    class_id = out[4:].argmax()
    cx, cy, bw, bh = out[:4]
    xmin = (cx - 0.5 * bw) * image_width
    xmax = (cx + 0.5 * bw) * image_width
    ymin = (cy - 0.5 * bh) * image_height
    ymax = (cy + 0.5 * bh) * image_height
```

可以看到， RT-DETR 的后处理极其简单！

检测效果也是很不错的：

![](img/09fd06a3f06063cc3dd217d92d157036.png)

![](img/6aadf1644be10f29924fd6656e5d254c.png)

![](img/f1b32e4dda7133d2cfd49b527577e613.png)

## 4. 参考资料

+ 超越YOLOv8，飞桨推出精度最高的实时检测器RT-DETR
+ `https://docs.ultralytics.com/zh/models/rtdetr/`
