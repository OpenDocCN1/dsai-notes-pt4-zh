
# AI绘画：StableDiffusion 制作 AI 赛博机车图

> 来源：[https://xmgtoagi.feishu.cn/docx/VMYOdKWu2o44Rax1dG8cpXIjnlc](https://xmgtoagi.feishu.cn/docx/VMYOdKWu2o44Rax1dG8cpXIjnlc)

如果你具备 `Stable Diffusion` 的基础知识，那制作 AI 赛博机车图会更为顺畅。即便没有这个基础，只要严格按照教程的步骤执行，也能成功制作出来。

本教程的重点是指导操作过程，而不会深入解析 `Stable Diffusion` 的各项功能原理。

效果图预览：

![](img/90ff859409852275935812bd3ef07823.png)

## 完整流程

制作 AI 赛博图由以下 6 部分组成

① 模型选择

② 提示词

③ 基础参数

④ ControlNet 调节

⑤ 后期图片高清处理

⑥ 剪映制作成视频

所用到的模型、提示词等资料全部在网盘链接里都可以找到

下载链接：https://pan.quark.cn/s/a8e797fe793b

## 详细介绍

### 一、模型选择

首推 `comix v3.1` 模型。

除此之外，`白城主综合机甲`、`majicmixRealistic`、`revAnimated` 这三个模型也是不错的选择。

特别要强调的是：不存在绝对的最佳模型选择，每个大型模型都会呈现出不同的效果。因此，需要大家通过反复尝试和不断探索，找到最适合自己需求的模型。

![](img/8bae55b19c315809120adbcfd4d8a0fe.png)

### 二、提示词

我们提供了一些通用的提示词（如图所示）。

如果需要更精细的描述，可以根据图中的元素再添加其他的提示词。

这些提示词已经上传到了网盘链接中。需要注意的是，我提供的提示词可能并非最优选择，我们需要大家一起探索和改进。

![](img/73b83c5125760285030a84790ca99033.png)

### 三、基础参数

① 迭代步数：30（28-32 都可以试试看）

② 采样方法：`DPM++ 2M Karras` 或者 `DPM++ 2M SDE Karras`

③ 宽高比不用设置，后面会自动调节的

④ 总批数设置 4 批，抽卡概率高些

⑤ 随机数种子：`-1`

### 四、ControlNet 调节

① 把启用、`Pixel Perfect`（完美像素）、`Allow Preview`（允许预览）打开

② `ControlNet` 模型选择：`Lineart` 或者 `Tile`，前者的采样方法是线稿，后者的是先模糊再放大

两种会出来完全不一样的风格，一般是两个都试一下，看哪个效果好

③ 弄完点击蓝色框框可以把图片尺寸替换上去，但是要注意会不会过大，如果过大就要长和宽都除以 2

最后点击生成就能出图了，具体的方式可以看后面的视频教程会更好理解一些

![](img/a9c8a42841ed2e8a372527aa87c64b82.png)

### 五、图片高清处理

找到 `【附加功能/后期处理】` 选项

![](img/4fa13818a9208646a7c8dbea7bb105f2.png)

放大使用 `【R-ESRGAN 4x+】`，点击生成，即可把图片变高清

### 六、常见问题解决

#### 1. Stable Diffusion 怎么安装

① 可以看我这篇文章学习，也可以网上搜索引擎搜一搜都能找到解决办法

[AI 绘画：Stable Diffusion 终极宝典：从入门到精通](https://y3if3fk7ce.feishu.cn/docx/KqEMdhJigoFY8fxc9TPcwMninKf)

② 如果是 A 卡、mac 系统、电脑配置低等情况，可以考虑用云平台来操作 SD

#### 2. 大模型怎么安装

这里的 SD 根目录就是大家在下载时，存放 SD 的那个文件夹

![](img/98ba2a695f2d71e6db84adb8ed97dcfe.png)

#### 3. 宽高比插件怎么安装

① 找到 `【扩展】` 选项

② 点击加载扩展列表

③ 搜索 `【Aspect】`

④ 安装 `【Aspect Ratio Helper】`

#### 4. ControlNet 怎么安装

ControlNet 资料网盘预览：

链接：https://pan.quark.cn/s/47bc8c79892a

![](img/82f01c376a64e8ae10521956e84a9f1b.png)

① 下载预处理器

![](img/463cf7f8afb375c731de6546c5f4f135.png)

下载网盘里的《1. 预处理器》文件夹

里面有一个 `downloads` 文件夹

把整个文件夹复制到 SD 的文件夹里面

具体位置：`SD文件 > extensions > sd-webui-controlnet > annotator`

② 下载模型

![](img/9b41fcaefedcd42ddcaa82b78d2b680a.png)

下载网盘里的《2. 模型》文件夹

里面有 14 个模型，把这些模型复制到 SD 文件夹里

具体位置：`SD文件 > models > ControlNet`
