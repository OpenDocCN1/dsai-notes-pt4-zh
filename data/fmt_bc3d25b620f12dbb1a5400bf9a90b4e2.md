
# 二维码也能变成AI艺术？如何用`Stable Diffusion WebUI`玩出新花样

> 来源：[https://e0lrm2v2v1u.feishu.cn/docx/ZNpPde7reoTMTJxpLsncMdGJnxb](https://e0lrm2v2v1u.feishu.cn/docx/ZNpPde7reoTMTJxpLsncMdGJnxb)

二维码虽然方便实用，但是它的外观实在太普通了，很难引起人们的注意和兴趣。如果你能把二维码变成一幅AI艺术作品，那么无论是用于品牌传播还是商品生产，都会让人眼前一亮，增加点击率和转化率。

那么，如何用AI绘画给二维码换个新装呢？上一期我们介绍了使用Midjourney画出AI艺术二维码的方法。这一篇我们介绍另一个方法，使用`Stable Diffusion WebUI`的`ControlNet`插件。

## 第一步：下载并安装`Stable Diffusion WebUI`

`Stable Diffusion WebUI`是一个基于稳定扩散算法的图像生成工具，它可以根据文本或图像输入生成高质量的图像输出。你可以从Automatic1111 github `https://github.com/AUTOMATIC1111/stable-diffusion-webui`下载它，并按照链接中的说明安装它。

## 第二步：安装`ControlNet`扩展

安装好WebUI后，你还需要安装一个叫做`ControlNet`的扩展，它可以让你控制图像生成的过程和效果。你只要进入`extensions tabs`标签页，然后进入`available`标签页。找到`ControlNet`，点击"安装"，然后重启你的WebUI。

重启后，进入`settings`页面，找到`ControlNet`，修改`Multi ControlNet: Max models amount`为2，再重启你的WebUI。

![](img/ffc5c8fe4fcbf54a8f43ea9e9c16af1f.png)

## 第三步：生成一个二维码

接下来，我们需要生成一个二维码作为输入图像。这里我们不用AI来生成二维码，而是用一个现成的网站来生成。有很多免费的网站可以选择，我使用这个网站 `https://qrbtf.com/`，你也可以用你喜欢的任何一个。

## 第四步：下载两个模型

为了让二维码变成AI艺术作品，我们还需要下载两个模型来控制图像生成的风格和亮度。从这个链接下载`control_v11p_sd15_lineart.pth`模型，下载链接 `https://huggingface.co/lllyasviel/ControlNet-v1-1/tree/main`

`control_v1p_sd15_brightness.safetensors`模型，下载链接 `https://huggingface.co/ioclab/ioc-controlnet/tree/main/models`

## 第五步：开始生成吧！

现在一切准备就绪，我们就可以开始生成带有可读二维码的AI艺术作品了！

首先，在两个`controlnet`部分中都上传我们刚刚生成的二维码，并在每个部分中勾选`enable`。

然后，在每个部分中都选择预处理器`inpaint_global_harmonious`，在第一个部分中选择模型`control_v11p_sd15_lineart.pth`，在第二个部分中选择模型`control_v1p_sd15_brightness.safetensors`。

接着，我们要调整控制权重和控制步骤，这可以让我们控制图像生成的过程和效果。在第一个部分中，选择控制权重0.35，开始控制步骤0，结束控制步骤1。在第二个部分中，选择控制权重0.65，开始控制步骤0.35，结束控制步骤0.75。

![](img/c6d7f6ad27fb623424984e684acfeb41.png)

![](img/208b9e3fdba5349f00d5157fc7f63a23.png)

采样方法可以选择`DPM++2M Karras`或`Euler A`。

现在，我们就可以进入`text2img`标签页，输入我们想要生成的图像的文本提示（和负面提示），然后按生成按钮，就可以看到我们的成果了！

![](img/de7998f2ddbc0a1276da647eccba9f5a.png)

发几张效果图：

![](img/12beb499dcf784b104120a936b57d2a8.png)

![](img/2a8c6c5f1842b2d7964e477225ca466b.png)

![](img/0aa332c6c1c71dfe5f54f3db0688d778.png)

![](img/650b76d1a31c3394aa1075ea8ff47e7a.png)

你可以用不同的文本提示和负面提示来生成不同风格的AI艺术作品，只要你有创意，就能玩出新花样。而且，你生成的图像中的二维码还是可以扫描的，可以把它链接到你想要推广的内容或产品上，让人们好奇地去扫一扫。
