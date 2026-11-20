
# 使用Deforum创建很酷的AI动画

> 来源：[https://e0lrm2v2v1u.feishu.cn/docx/QR9xdRbXWovG7Txruxgcel9XnVb](https://e0lrm2v2v1u.feishu.cn/docx/QR9xdRbXWovG7Txruxgcel9XnVb)

## 什么是Deforum

Deforum是一个强大的工具，使您能够使用稳定扩散创建动画视频。在本教程中，我们将引导您完成制作 Deforum 视频的过程。

## Deforum能做什么

+ Deforum是一个用于制作动画的开源免费软件。
+ 它利用稳定扩散的图像到图像功能生成一系列图像，然后将其拼接在一起形成视频。
+ 通过对每个图像帧应用小的变换，Deforum创造了连续视频的错觉。

### “Keyframes”选项卡

+ 找到页面下半部分的“`Motion`”选项卡，然后单击它。
+ 您将在此处为视频设置相机参数。
+ `Max frames`选项决定了视频中的帧数。值越高，视频越长。

![](img/b287d9a8f57ce3b2a8f378597986142f.png)

### Prompts 设置

+ 切换到“`Prompts`”选项卡。
+ 您将看到一个提示列表，每个提示都有一个相应的数字，指示提示生效的帧。
+ 例如，第一个提示从视频的开头开始有效，第二个提示在第 30 帧生效，第三个提示在第 60 帧生效，依此类推。
+ 您可以根据所需的效果修改提示。下面显示的是首次进入提示选项卡时看到的默认提示

![](img/b4ea369e9011fa103ff0383300534bb6.png)

### 生成视频

+ 单击“生成”按钮以启动视频生成过程。
+ 生成完成后，单击“生成”按钮上方的按钮以预览视频。
+ 要将视频保存到本地存储，请单击右下角的三个垂直点，或在“`img2img-images`”文件夹下的输出目录中找到视频。

![](img/0d7b1f55eef76ebbf3f0e96d85fa5608.png)

## Motion Settings 运动设置

运动设置在Deforum中起着至关重要的作用。通过修改它们以及提示，您可以创建视觉上令人惊叹的效果。

我们将介绍两种最常用的动画模式：2D 和 3D。

### 2D Motion Settings 2D 运动设置

+ `2D Zoom`:调整缩放功能以放大或缩小图像。使用大于 1 的值进行放大，使用小于 1 的值进行缩小。该值离 1 越远，缩放速度越快。
+ `2D Angle`:使用 2D 角度设置旋转图像。正值使图像逆时针旋转，而负值使图像顺时针旋转。值越大，旋转速度越快。
+ `2D Translation X`:使用平移 X 参数将图像横向移动。正值将图像向右移动，负值将图像向左移动。
+ `2D Translation Y`:调整平移 Y 参数以上下移动图像。正值将图像向下移动，负值将其向上移动。
+ `Transform Center`:此设置允许您更改缩放和旋转的焦点。

### 3D Motion Settings 3D 运动设置

3D 运动可作为 2D 运动的替代方法。想象一下，拿着相机，向任何方向自由移动和旋转。

+ `3D Translation X`: 使用平移 X 参数将摄像机横向移动。正值将相机向右移动，而负值将其向左移动。
+ `3D Translation Y`: 调整平移 Y 参数以上下移动摄像机。正值将相机向上移动，负值将其向下移动。
+ `Translation Z`: 与 2D 动作中的缩放类似，平移 Z 控制 3D 中的缩放效果。
+ `Rotation X`, `Rotation Y`, `Rotation Z`: 这些参数允许您分别围绕 X、Y 和 Z 轴旋转摄像机。

### Motion Schedule 运动时间表

+ 运动设置使用以下格式定义：帧 1：（值 1）、帧 2：（值 2）、帧 3：（值 3） 等。
+ 每个条目由两个用冒号分隔的数字组成：动作生效时的帧编号和括在括号中的相应值。
+ 通过组合不同的运动设置并随时间进行调整，您可以创建动态视觉效果。

## 使用Deforum创建我们自己的3D视频的示例

让我们按照以下方法创建自己的视频：

### “Run”选项卡设置

+ 将分辨率设置为 768 x 768
+ 采样方法选择 `Euler a`
+ 步数35

![](img/b2fe6809d069fdd79b1a677292121c70.png)

### “Keyframes”选项卡设置

+ `animation mode` 选择 3D
+ Border mode 选择 `Wrap`
+ `max frames` 选择 `360`

![](img/76a37b81cd64f3027bfd1c2cfba8d78c.png)

### 运动设置参数

对于运动设置，我们将使用以下值：

+ `Translation X` - `0:(0), 30:(15), 210:(15), 300:(0)`
+ `Translation Y` - `0:(0)`
+ `Translation Z` - `0:(0.2),60:(10),300:(15)`
+ `Rotation 3D X` - `0:(0),60: (0),90: (0.5),180: (0.5),300:(0.5)`
+ `Rotation 3D Y` - `0:(0).30:(-3.5).90:(0.5).180:(-2.8).300:(-2).420:(0)`
+ `Rotation 3D Z` - `0:(0,60:(0.2),90:0 ,180:(-0.5 ,300:(0)`

![](img/e5da8c8b09b404626cfd6cc905338211.png)

### `noise schedule`

`noise schedule`选择中，我们可以输入以下内容：

![](img/78cca85ffbee7c630c92f1c7648407dd.png)

### `anti blur`

对于`anti blur`选项卡，我们将`Amount schedule`调整为 `0：（0.05）`

![](img/bc589a301221672129ed6133b998b641.png)

### `video output`

对于`video output`选项卡，我们将按如下方式放大视频：

![](img/546d79110856e979e9d43214e3900151.png)

### 提示词

现在，我们已准备好输入提示！：

```json
{
"0": "ultra detailed, 8k, photorealism, dramatic lighting, ultra detailed A robot is walking through a dystopian bleak society with dark moody lighting",

"100": "ultra detailed, 8k, photorealism, dramatic lighting, ultra detailed Robots start to fight each other with flames and smoke surrounding the robots"

"200": "ultra detailed, 8k, photorealism, dramatic lighting, ultra detailed Buildings start to collapse around the robots",

"300": "ultra detailed, 8k, photorealism, dramatic lighting, ultra detailed, derelict buildings"

}
```

您现在可以单击生成来创建您的视频！

通过这些步骤，您可以使用稳定扩散和Deforum创建令人印象深刻的视频。请记住尝试不同的设置和提示，以释放您的创造力。
