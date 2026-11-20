
# 工作流使用指南

按住 `Shift`+鼠标点击直接拔线插入到 图像列表 节点，然后把从加载图像的线连接过去。

![](img/cf633edf0bd3c4f9ce6b6d337f9c19db.png)

然后填入文件夹的图像目录路径：`./input/test`

最后添加一个保存图像的节点，并修改下文件名前缀。文件生成后会再 `output` 目录下，可以批量选中打包下载。

![](img/e0a3a9cc4d382ebf67b79aff608028cf.png)

## 运行

点击 `添加提示词队列`，运行一次。

耐心等待十几分钟，图片跑出来了，土豆总共跑了五张图片，点击 `x` 显示图片列表。

![](img/ca0fd765b547457c9c571d7eadfc80ca.png)

![](img/b2cc78bd7d0486108108b826f30e80f0.png)

左边是原图，右边是生成的图片。

![](img/b6c2a0bcaff2f60e0cc93f78955086e4.png)

![](img/1738d7b87ab11a58661af906e99195c6.png)

图像下载在 `output` 目录，可以多个图片打包下载。

![](img/0552b1cfee397bd4d8c5d113b6654cfb.png)

## 工作流Tips

### 如何保存图像

#### 保存预览图像

如果想直接保存预览图像，可以点击右键打开图像再手动保存。直接点保存图像可能不行。

![](img/55fe4ff1a8b77ac3f65354b8bc9489a0.png)

#### 添加保存图像节点

注意，预览图像节点是不会保存图像的，需要添加保存图像节点。

双击鼠标会弹出这个搜索菜单，找到保存图像，点击就是一个保存图像的节点。

然后把同为图像的蓝色节点连上线就可以保存图像了。这里你还会发现文件名前缀土豆这里是输入，另一个是连线端口，其实只需要右键选择转化为输入就行，可以随意转换。

![](img/6e9dd75e72fae9a9f0730b048c82a7bd.png)

### 解决常见的报错

#### 缺少节点

![](img/9a410952063d4dd2af319f29e9d28717.png)

像这种缺少节点的情况，需要安装缺失的节点，点击安装缺失节点，把相应的节点安装一下。

![](img/bca48b0b18adcc5e3978c29ed75c58a6.png)

![](img/2a48bc14b4cd4430830fa22aa5145c3d.png)

然后重启后重新运行。

![](img/4b12be6e2e9338b8fb43df0529507281.png)

如果还有缺少模型的情况，就需要下载相应 [SD-ComfyUI 文件管理 操作指引](https://cephalon.feishu.cn/wiki/JNAmw9j4DiW0XckwMj7ceNcenjf) 型上传到平台，可以参考官方的教程解决。

找到官方文档模型安装部分，推荐使用百度网盘上传，需要开通会员，速度最快

![](img/f8733f8fd83f44f634328fc5f1cd0ccb.png)

#### 运行报错

如果导入未报错而运行报错说明模型名称对不上，需要看下是否有相同的模型。有的话就可以直接替换，没有就需要下载再导入平台了。

![](img/ace4668fab1c505ae5f320ba1db88b75.png)

像这样的报错是因为模型的文件名使用的不一样，所以报错。我们直接选择对应模型就行了。

![](img/3b77da98d691030b5047e9f783c83e3a.png)

而这个 LoRA 加载器出错是因为没有对应的 Lora，需要单独下载才行。

![](img/75a2627497306996c82e480afbaaa543.png)

#### 清理显存

一定不要忘记清理显存。很多时候都是因为忘记清理显存导致运行错误。

![](img/81ffb65af391abe73b8b52a22dda3a76.png)

![](img/874c5a95b52a7ea639e3f512a352e377.png)

#### 重启大法

有些时候会因为爆显存或者网页多卡莫名其妙在运行中会出现错误。运行中如果有出现明确提示的错误就可以试试重启大法。

比如遇到提示 `allocation on device` 那就点击这个重启按钮解决吧，可以等待1分钟左右手动刷新下网页。

![](img/46d3a637f75be3acc5768e3e55fad3fa.png)

# 进阶学习

关于平台的任何使用问题推荐看看端脑 [新用户必读 `README`](https://cephalon.feishu.cn/wiki/DLf6wEV1HiNUQgkpvRycBpkLnIe) 官方手册文档，手册基本都会写的比较清楚

如果你接下来想系统性学习一下各个节点的使用以及自己独立完成一个工作流，土豆推荐你看下B站up主Nenly同学的ComfyUI视频合集，他的视频很精致也很干货，整个合集不到3小时。

ComfyUI新手指南：轻松掌握节点界面 Nenly同学的个人空间-Nenly同学个人主页-哔哩哔哩视频

端脑云-专业版里面的全部工作流 https://tudoujunh.lanzoub.com/iP0oM2fl7v2j

@红烧电饭煲 感谢圈友红烧电饭煲，由红烧电饭煲提供的工作流资源：

> 电饭煲个人咸鱼购入：https://pan.baidu.com/s/1lKpYMmOzgwMxf3rzFqZf9g?pwd=djnb

> 工作流搜索的一些平台：

> 国内：

> +   https://www.liblib.art/workflows

> 海外：

> +   https://github.com/yolain/ComfyUI-Yolain-Workflows

> +   https://openart.ai/workflows/all

> +   https://comfyworkflows.com/

> +   https://civitai.com/models
