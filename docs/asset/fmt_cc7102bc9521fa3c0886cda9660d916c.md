
# 实例教程：头像转绘

> 来源：`https://my.feishu.cn/docx/CSf4ducQ4oURdmxMbCwcZVP4nDe`

## 程：头像转绘

| ![](img/d5629d18620aa551aa9a61a04435b533.png) | ![](img/971a52c4b3569b3581c8185eaca3b4ee.png) |

| --- | --- |

`Xyz` 重绘幅度测试效果（第二次图生图）

![](img/ffd78089ce1eb385d12551df0c2015df.png)

### 1、第一次图生图

垫的原图照片1024x1536（重绘尺寸512x768），使用反推出关键词，手动优化删减。

模型墨幽二次元，添加lora和lora触发词，重绘幅度0.5

![](img/747ef495ffcbc600c778c5db285d164a.png)

![](img/3b4acdbc2dc35714743cde4dc0d42269.png)

#### ControlNet的设置

图片和上面使用的同一张，开启`softedage`+`openpose`，权重如图所示，可以跟着效果进行调整

⚠️图片不同选择的预处理可以相应调整，比如选择`canny`，我这里只有脸部所以只需要开脸部，如果有手选择带手的`openpose`。

![](img/aa1ed51ae892fa02b2cd55315ab57edf.png)

![](img/452f5dbcf93210c1d4ccba52e0cf775c.png)

### 2、第二次图生图

使用第一次生成的图。发送至左边的图生图，重绘尺寸倍数2倍，重绘幅度0.25

⚠️关闭所有`ControlNet`

![](img/29dc0a6127f56de5d01ce1bf9fe0e056.png)

![](img/b6dcce62ac1e08e05ca7a7acfd5cc052.png)

使用的模型（模型-墨幽二次元）

lora（简易线条小插图）

`https://www.liblib.art/modelinfo/6a3e6e7d6f934e2c9b3bc3bc35e671df`
