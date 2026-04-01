# 超级简单的AI换脸

> 来源：[https://xmgtoagi.feishu.cn/docx/YPFCdUIwQoAxZXx5Qy9c9UgjnD3](https://xmgtoagi.feishu.cn/docx/YPFCdUIwQoAxZXx5Qy9c9UgjnD3)

> 作者：Lucas Al

> 原文：https://mp.weixin.qq.com/s/90s0VnPV7ah_V1qs5a0ksg

Ea**syPhoto是Stable Diffusion的一个**底层功能，其实就是一个WebUI的扩展，安装部署好Stable Diffusion之后，使用这个工具只需要复制GitHub网址：https://github.com/aigc-apps/sd-webui-EasyPhoto

![](img/b723d81c1876dfa54fde3476c71b5fe8.png)

到这个Git页面下载压缩包，然后将压缩包解压到SD-WEBUI的Extension文件夹即可

![](img/a6658f5fc3ceed302c5e2ba46355cfeb.png)

![](img/4f08a083460238ec45a73a5cf48f175f.png)

安装好后记得重启整个WebUI，这样会自动下载其他所需的环境

在一切都搞完之后EasyPhoto就会出现在WebUI界面上

![](img/8eaa0de578c02b5141b0e251e4e3fb65.png)

虽然说不需要额外再下载别的，但是还是得有ControlNet在WebUI中，因为后续的操作会用到ControlNet来辅助。设置中ConrtolNet的数量最好开到3，也就是能同时开3个ControlNet Unit

![](img/faa764d85bd0b09c056854aeb9a54ff2.png)

EasyPhoto主要分为训练和推理

上传同一个人的照片，大约5-20张照片可以帮助AI很好的记住上传的照片中人物的样貌特征。

在照片都上传了之后就可以在右侧进行参数的设置：

![](img/9d80dae91119c115532c00d0317e7953.png)

其他的参数都可以不进行变动，主要是base checkpoint的选择，这个模型会决定了之后出的图整体风格是哪一类。

一般选择Realistic Vision会比较好，因为这类模型比较泛用不容易翻车。

然后点击下方的Start Training，系统会跳出一个提示框要求输入User id，这个id用来标记照片训练的模型

![](img/75bf7859df1b792c62e04df530383115.png)

初次训练所需要的时间可能会比较久，大约得花上十分钟左右，在这个过程可以看下方的训练日志（Training Logs）来观察进度。

在完成训练之后就到了出图也就是推理的步骤了：

![](img/567cb22592dfec254e4e52b7c3f2153b.png)

可以看到EasyPhoto已经提供了很多的模板图像，例如证件照、艺术照等画风的图片。

在下方的参数中可以看到有很多选项

模型和上面训练时选择的一样就行，User id则是刚刚训练时输入的用户名，如果后续训练了多个不同的模型那么可以在这里进行选择，然后点击下方的生成后系统就会开始生成照片。

根据设备的不同大约会花上几十秒到几分钟不等，这个过程还是比较快的。

在训练结束后如果觉得照片不像本人的话，还可以用自己上传照片的方式让AI更好的认识到你。

![](img/1381a7aa1ce3e97b6342376d14f16ef5.png)

可以先用AI生成一个符合自己预期的场景、身形、服饰等的图像，然后再将这张照片上传到EasyPhoto中，EasyPhoto就会根据这张照片进行重绘，得出来的结果也会更接近自己的预期。

EasyPhoto更像是进行了一个智能抠图+美颜的操作，然后根据这些图像的预设参数和标签进行训练。