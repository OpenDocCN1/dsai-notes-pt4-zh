
# 实操教程：高清图像放大+面部修复+一键抠图

> 来源：[https://xmgtoagi.feishu.cn/docx/I9GHdq0F9oyuk5xaEI2cOoNGnGe](https://xmgtoagi.feishu.cn/docx/I9GHdq0F9oyuk5xaEI2cOoNGnGe)

一提起后期处理，我猜你可能立马想到的就是图像放大。

今天，我要向你展示的后期处理手法，以及优化技巧。

## 图片放大算法

如果你常用的是秋叶大佬的整合包，那么你对`R-ESRGAN 4x+`和`R-ESRGAN 4x+ Anime6B`应该不会陌生。

`R-ESRGAN 4x+`：写实图片

`R-ESRGAN 4x+ Anime6B`：二次元图片

然而，你是否曾在C站或者其他地方，见到大佬们晒出的作品中出现什么“4x-UltraSharp”之类的高级算法。

然后你在自己的SD中确找不到？

原因很简单，是你没有下载安装这些高端的算法。

所以，我在这里为你推荐整理四款更为强大的放大算法：

`4x-UltraSharp`、`8x_NMKD-Superscale_150000_G`、`8x_NMKD-Typescale_175k`、`16xPSNR`

网盘链接：https://pan.quark.cn/s/f7d53bd18095

下载后放入`……\sd-webui-aki-v4.2\models\ESRGAN`路径下，再重启一下软件.就可以愉快的使用了

![](img/0d48f17bd32039a45d2d5f175cedbb36.png)

然后就到了最令人期待的部分：看看这些新晋算法的能力。

我拿了一张模糊点的图片作为实验材料

分别使用了传统算法`Lanczos`、常用算法`R-ESRGAN 4x+`，以及刚才下载的四个新算法进行了对比。

![](img/ff485899aeff42afc84167b30dfb16ed.png)

图片太小，大家可能看不清细节，我直接来做一下总结。

总结如下：

1.`Lanczos`只是简单地放大了像素，那就是，放大而已。

2.`R-ESRGAN 4x+`则展现出了他的王者风范，放大后的图片有更多的细节和更清晰的轮廓。

3.`4x-UltraSharp`表现得更加出色，它拥有更清晰的细节，而且没有过度锐化的部分。

4.`8x`、`16x`系列的放大器，更是展现出了他们对细节的处理能力，他们不仅仅追求美观，更重视真实，因此会增加更多的噪点和毛孔细节。但是处理速度会变慢

5.在四个新算法中，`4x-UltraSharp`表现出色，`8x_NMKD-Superscale_150000_G`色调略显冷淡，`8x_NMKD-Typescale_175k`的皮肤噪点比`4x`的更多，而`16xPSNR`虽然处理速度较慢，效果并没有比`8x`有明显提升，因此并不推荐。

## 背景去除（抠图）

这个功能需要再安装一个插件`REMBG`

安装地址是https://github.com/AUTOMATIC1111/stable-diffusion-webui-rembg.git

![](img/67713fe564a4900036e430b44f4d1783.png)

安装过程大约需要1分钟时间

然后点击【已安装】，列表里显示已经安装完`stable-diffusion-webui-rembg`插件的话

点击上方的【应用并重启用户界面】按钮

![](img/84126ed1f0aa4d56cf57a362155560a1.png)

下面实际操作，进行图片去除背景的演示

导航栏：附加功能

### `U2Net`

![](img/bf06f663442724cf7b1c5509e975088d.png)

点击生成  然后控制台可以看到相应的进度

遇到的问题：我这里控制台下载速度非常的慢，于是手动去下载，然后放到对应的目录内

![](img/942c1a1bccba42bba30d102a7c5503c5.png)

最终效果：

![](img/bee30e8798ee10b12c5e26dd0c727105.png)

它这里面支持多种模式可选，每种模式都是单独下载模型才行

有网速不好的人可能下载不了，我这里把模型上传到夸克网盘里

有需要的可以去网盘下载，将下载的模型文件放到`AI安装目录\.cache\u2net` 文件夹内，如下图所示。

网盘链接：https://pan.quark.cn/s/f7d53bd18095

![](img/99de6a800999ed37d1c5e255b2a68bf5.png)

另外，点击这个按钮，还可以直接生成蒙版。

![](img/85dde1cbe34999dd6bbe7292ab2db6fd.png)

![](img/c6a85d3da0dbcbb045680f5b536b6065.png)

### `u2net_cloth_seg`

这里还有一个专门做服装抠图的模型，可以一键识别出模特的服装。

## 面部修复

我们接着来看一下这个`GFPGAN`面部修复，我找一张模糊人像来修复一下。

![](img/dbff69c253dc7197859d4a8f2e9cf0fe.png)

其他参数不变，直接将`GFPGAN`参数拉到1，就可以了。

效果对比：

![](img/e798ac19cff23ca5e703eca888c569ae.png)

![](img/edface4b85225b4041bd1be815ee87e9.png)

看一下面部修复效果，宛如重生。当然这个功能只能修复脸部，所以头发、衣服、背景等没有变化。
