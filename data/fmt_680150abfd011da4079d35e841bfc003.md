
# Midjourney 基础参数详解

> 来源：[https://my.feishu.cn/docx/Iaiqd4Y93ofi2txZEeqclMx9nvb](https://my.feishu.cn/docx/Iaiqd4Y93ofi2txZEeqclMx9nvb)

## 基本参数

### 1.长宽比

可以设置图片的纵横比。按照需求可以选择不同的尺寸，也可以自定义。

![](img/abfce46a5cb70afe82e997b4895260d5.png)

注意：`--ar`必须使用整数。使用139：100代替1.39：1。

长宽比会影响生成图像的形状和构图。

在放大时，某些长宽比可能会稍微改变。

较旧的Midjourney版本模型可能不支持所有长宽比。

常见的Midjourney长宽比：

`--ar 1:1` 默认长宽比。

`--ar 5:4` 常见的相框和打印比例。

`--ar 3:2` 常见于印刷摄影。

`--ar 7:4` 接近高清电视屏幕和智能手机屏幕的比例。

![](img/912ed92c426ef2803b305e23e4c50cba.png)

### 2.`--c`

`--c`值范围1-100，较高的`--chaos`值会产生更不寻常和意想不到的结果和构图。较低的`--chaos`值会产生更可靠、可重复的结果。

![](img/319d6b357ace82ab864223dc4b72a5f8.png)

相同提示词下不同的`--c`值的对比：a silver seashell inlaid with pink and green accents

`--c 0`                                                       `--c10`

![](img/b3ab9d42239e0d745336aa0488803e95.png)

`--c 25`

![](img/93c0211c6454a10e5bf0f43b5e2ad586.png)

![](img/f62870e22b28ef9503b6e749c411396f.png)

`--c 50`

![](img/481ac5b28187c7cd950c93b9b6bd1dfd.png)

`--c 80`

![](img/418dbe0386672de5461ba15f4c1d273e.png)

### 3.`--iw`

`--iw <0–2>`设置相对于文本权重的图像提示权重。默认值为 1。

![](img/dfc79b83c99e6a965fc1de24ca85ca6e.png)

### 4.`--No`

`--no`负面提示`--no plants`会尝试从图像中移除植物。

![](img/3fe2b02b4bda927c72fcecfe0c48ef27.png)

`--No`比较

still life gouache painting                      still life gouache painting `--no fruit`

![](img/42676b090bfd3bb850347af0851d1ef3.png)

### 5.`--Q`

`--quality <.25, .5, or 1>`，或者`--q <.25, .5, or 1>`默认值为 1。值越高，使用的GPU 分钟数越多；值越低，使用的 GPU 分钟数越少。

![](img/46933e2ab40365913e572a6d7311f74d.png)

注意

质量设置不会影响分辨率。

默认的`--quality`数值为1。

当前模型只接受数值为.25, .5和1的`--quality`。较大的数值会被四舍五入为1。

`--quality`只影响初始图像生成。

### 6.`--seed`

种子号是为每个图像随机生成的，可以使用 `--seed` 参数指定。如果未指定种子，Midjourney将使用随机生成的种子数，每次使用提示时都会产生各种不同的选项，使用相同的种子号和提示将产生类似的最终图像。

![](img/f8507c0ca307f65720114e9431d58e11.png)

![](img/ecd98197517ac1a3793ec508e959adf0.png)

注意：

`--seed`接受0-4294967295之间的整数。

`--seed`数值只影响初始图像网格。

在模型版本1、2、3、test和testp中使用相同的`--seed`数值会产生具有相似构图、颜色和细节的图像。

在模型版本4、5、6和niji中使用相同的`--seed`数值会产生几乎相同的图像。

种子数字并非静态不变的，不应依赖于它在不同会话之间保持不变。

### 7.`--stop`

范围10-100，值越低结果更模糊、越不详细。

![](img/46886b41a69b5f7cd1ddab111e53025a.png)

同一提示词不同的stop值比较：

![](img/6d35f89c509dc5a5bb60cc163906193a.png)

### 8.`--S`

Midjourney Bot经过训练，生成的图像更偏向艺术化的色彩、构图和形态。`--stylize`或`--s`参数影响这种训练的强度。较低的艺术化数值生成的图像更接近提示，但艺术性较低。较高的艺术化数值生成的图像更具艺术性，但与提示联系较少。

![](img/09bd5820a8c367b354b5c15474c53e7d.png)

相同提示词不同的`--s`值对比：`/imagine prompt` child's drawing of a cat `--s 100`

![](img/f0a46c63e48e76bddbbcf2bcf4ec100c.png)

### 9.`--tile`

`--tile`参数生成可用作重复图块以创建无缝图案的图像。

`--tile`参数生成的图像可用作重复瓷砖，用于制作无缝图案的织物、壁纸和纹理。

`--tile`适用于模型版本1、2、3、测试、测试p、5、5.1、5.2和6。

`--tile`仅生成单个瓷砖。使用类似无缝图案检查器的图案制作工具来查看瓷砖的重复。

![](img/1b822110de95bdd6f41b1a036670c713.png)

案例：prompt a pattern of pink and blue striped river stones `--tile`

![](img/d88f8470473f08ffbea25c1963edecc6.png)

### 10.`--weird`

范围0-3000，这个参数为生成的图像引入古怪和别致的特质，产生独特和意想不到的效果。

![](img/8ad0e1bedc32bdb5b68cb7f6c06135c8.png)

注意：`--weird`兼容Midjourney模型版本5、5.1、5.2、6、niji 5和niji 6。

`--weird`与种子不完全兼容。

案例：`/imagine prompt` cyanotype cat `--weird 250`

![](img/5f5752fcc332627353df9b5d0ee2df79.png)
