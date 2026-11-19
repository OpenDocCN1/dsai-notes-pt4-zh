
# SD进阶教程模特换装

> 来源：[https://my.feishu.cn/docx/BUuYdrs2looGRCxnZEucKeV8nLh](https://my.feishu.cn/docx/BUuYdrs2looGRCxnZEucKeV8nLh)

本期带大家实操用SD如何给模特换装？话不多说，直接开始。

## 第一步：上传图片到 Inpaint Anything 中

进入我们云端SD 中，点击 Inpaint Anything 标签页下，将模特原图图片拖动到输入图像框中。

![](img/8913f96967e247cd76ce96bf4cbca331.png)

## 第二步：运行分割模型

等待图片上传完成后，点击“运行 Segment Anything”按钮，然后等待，你就能看到这样的语义分割图，不同的颜色代表了图片中识别出的不同对象。

![](img/4568ff636f184c1ca9adf951d54419fe.png)

## 第三步：创建蒙版

鼠标放在图片上时按键盘 S 键放大图片

![](img/1b1e54dadd21f02cda7fe96a19a3f1a6.png)

使用画笔在分割图中标识出想要重绘的裙子区域，不需要整件衣服涂黑，每件衣服每个部分点一个点就可以了

![](img/1baf0ce116c90c6b4ecfc2ada7a1cf43.png)

鼠标放在图片上时按键盘 R 复原，并点击Create Mask

![](img/a9e898d931f606dc939fedc07bb044ea.png)

得到如下图：

![](img/262210497af830e504d8ad04e107f372.png)

如果发现蒙版未覆盖您想要的所有区域，请返回分割图并绘制更多区域。

## 第四步：将蒙版发送至重绘

点击“仅蒙版”这个标签页，然后点击获取蒙版，就会获得如下一张黑白的蒙版页面，如果你有 Photoshop 使用经验的话，这个蒙版和 PS 中的蒙版是一样的。

![](img/d223f6b78e314cc324ad711ddef03b8f.png)

点击“发送到图生图”按钮

![](img/48b88aaf72ef1c03c1d374e437f0dd74.png)

## 第五步：出图

![](img/7f8b0853d18ea470513b4c236a517f3f.png)

![](img/28cfd2c88ca59d17d9e765fb6e7657ea.png)

这个过程就是需要不断调整提示词，多跑图，最后找到满意的。

![](img/fbfebeba657091a795d66309feb7cd14.png)

![](img/633de7bb598523f9b2b7dc2361ca8737.png)

![](img/50f450fb505b2fa4a88b7f3eda92c815.png)

![](img/235e523478658c84ce41bbba6785e230.png)

![](img/4ee7c83f83fe4bc3be235e39bda93fb3.png)
