
# 老照片修复教程

> 来源：[https://my.feishu.cn/docx/Jwhxd3O8GobyHhxasiNcmgT5nF0](https://my.feishu.cn/docx/Jwhxd3O8GobyHhxasiNcmgT5nF0)

## 案例一：老照片修复上色

### 原图与效果图

原图：

![](img/fa8d99d4cd5a0b29f4a44b9bf9b2bf2f.png)

效果图：

![](img/e019badea75e87ca5321ae7041cd4b71.png)

### 步骤一：打开在线`SD`

![](img/679e9bae830a3de67233caef73d7788a.png)

### 步骤二：图像高清修复

这里我们先用附加功能对图片就行高清修复一次。

这里我们选择图像放大`R-ESRGAN 4x+`算法，大部分图片都可以用这个算法，下面的`GFPGAN`是面部修复，这里也启用。得到的图像我们先保存下来

![](img/6ec6882f3b9d84195b9c75d3bc4685a1.png)

### 步骤三：文生图设置

来到文生图界面，先写提示词，设置采样方法和迭代步数。

正面提示词：a girl,black hair,wearing military green clothes,high-quality,real,

负面提示词：NSFW,text,blurry,low quality,bad anatomy,sketches,monochrome,grayscale,worstquality,

Sampler: DPM++ 2M SDE   Steps: 30

![](img/8c75ccbb2e44add3223d561236f09fdc.png)

### 步骤四：`ControlNet`重着色

接下来用`ControlNet`的重着色(`Recolor`)，上传修复好的照片。

![](img/0ba01466a5ebff081ee74a065d4ed302.png)

这就是老照片修复+上色的整个流程啦！图中重点也给大家标记了。

当然，顺序并不一定完全按照这个流程，你也可以先上色后修复，也可以先修复后上色！！！
