# 类似D-ID的开源数字人项目SadTalker搭建教程及效果演示

> 来源：[https://aiyanxishe.feishu.cn/docx/L5v9dkFbtoYcEIxgq15cPtfMnWf](https://aiyanxishe.feishu.cn/docx/L5v9dkFbtoYcEIxgq15cPtfMnWf)

## 生成效果演示：

图片：

![](img/33ddfbcdaf07c14899d9e105d5fa0307.png)

音频：

![](img/acb1dcc20a6527fdebeb44b18111d4ca.png)

[chinese_poem2.wav](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EMZ7bCs3Ko63F8xlImsc9unSnzc)

下面是生成的视频效果：

![](img/04779e3bdcddce4ecddc6bf10b4ca905.png)

[iu.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HtJFb44f8o83lsxaawZcCSGbn7b)

因为ai驱动会改变口形眼神，有时候跟原图看起来不太像，但是总体来说SadTalker是开源数字人项目里效果比较好的了，特别是他的口形，中文还是比较像的，感觉比D-ID要像一些。其他方面，比如自然程度，画质比起D-ID要差一些，当然更别说国内几家大厂的收费数字人项目了，他们身体，手势都有，挺像真人的，所以需要自己判断下使用场景。

## SadTalker的安装及使用方法：

SadTalker主页：https://github.com/Winfredy/SadTalker

### 安装步骤：

#### 1、安装NVIDIA cuda11.3

安装方法看这篇文章：https://blog.csdn.net/zzjcymbq/article/details/125040993

注意不要这篇文章最后一步 pytorch配置，后面在“项目下载和运行环境配置”会安装pytorch

![](img/bd2e9ca81fd5306c48c4e8c69d3c3292.png)

#### 2、安装 anaconda

由于这个项目是python开发的所以需要先安装一个方便管理python的工具anaconda

安装方法可以看这边教程：https://blog.csdn.net/qq_45344586/article/details/124028689

#### 3、项目下载和运行环境配置

##### (1)下载文件并解压

因为github以及相关模型直接下载会比较慢，我打包上传到百度网盘了

项目源码百度盘：https://pan.baidu.com/s/1_2qdL8OAFO-RBrtBpicAqQ?pwd=sm6w

从网盘下载后解压到电脑某个盘，这里举例就下载解压到D盘了

##### (2)命令行安装

点击开始菜单找到前面已经安装好的 Anaconda Prompt

![](img/c750ea51d9d254617d0556d008aec1e5.png)

挨着输入下面的命令，注意“#”后面的文字是注释，不要在命令行里输入

每一行命令输完后按回车，有的步骤安装过程比较久需要耐心等待，有几步安装过程中会询问是否安装，需要输入 “y”确认

```
#进入D盘
D:

#进入SadTalker项目目录
cd SadTalker

#创建一个python3.8 名为sadtalker的虚拟环境
conda create -n sadtalker python=3.8

#激活名为sadtalker的虚拟环境
conda activate sadtalker

#pip切换到清华源提高下载速度
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

#安装pytorch及相关包
pip install torch==1.12.1+cu113 torchvision==0.13.1+cu113 torchaudio==0.12.1 --extra-index-url https://download.pytorch.org/whl/cu113

#安装视频处理工具ffmpeg
conda install ffmpeg

#安装项目相关依赖
pip install -r requirements.txt
```

![](img/e6abd56de028692c5a56d1b5c9a26dce.png)

##### (3) 复制gfpgan权重文件到虚拟环境

在刚才的命令行工具里输入命令查看虚拟环境路径

```
conda info
```

把SadTalker源代码里面的：gfpgan\weights\GFPGANv1.4.pth 剪切到虚拟环境的 Lib\site-packages\gfpgan\weights 目录下

![](img/7dfd395308cad6bb2f0775a674c9ae20.png)

举例：

我的虚拟环境就是截图红框路径，所以需要把gfpgan\weights\GFPGANv1.4.pth 剪切到虚拟环境的  D:\ProgramData\Anaconda3\envs\sadtalker\Lib\site-packages\gfpgan\weights 目录下

### 使用方法：

提前准备好语音文件和一张图片，这里假设你已经准备好语音和图片了（需要做某个人物声音的，可以用相关的声音克隆项目克隆声音，空了我再写一篇声音克隆的教程）

把语音文件放到SadTalker\examples\driven_audio目录下

把图片放到SadTalker\examples\source_image目录下

#### 1、基础使用命令

然后输入下面的命令

```
#基础使用
#python inference.py --driven_audio &lt;audio.wav&gt; --source_image &lt;picture.png&gt; --enhancer gfpgan
#--driven_audio后面需要写音频文件路径，--source_image后面写图片的路径，下面是一个例子

python inference.py  --driven_audio  D:\SadTalker\examples\driven_audio\bus_chinese.wav --source_image D:\SadTalker\examples\source_image\full_body_2.png --enhancer gfpgan
```

等待执行完成后，就可以在SadTalker\results下面的文件夹里面找到生成的结果了

![](img/5f6400b008fe048b282b76fcef151e7d.png)

效果如下：

![](img/d7c4e88598cf029e4b667aa469a7090f.png)

[full_body_2##bus_chinese_enhanced.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F3rGbxBnooSiwvxuNhocydA7nIe)

#### 2、更多命令参数

首先看下相关参数：

细心地同学可能会发现一个问题，上面的基础使用，用到的图片是一张半身照，但是生成的视频是只有头部了

对，这个是需要参数控制的，接下来我们加上--preprocess full 和 --still这两个参数

```
python inference.py  --driven_audio  D:\SadTalker\examples\driven_audio\bus_chinese.wav --source_image D:\SadTalker\examples\source_image\full_body_2.png --enhancer gfpgan --preprocess full --still
```

--preprocess full 表示完整图片

--still 可以减少头部运动

第一个参数好理解，为什么会有第二个参数呢？ 原因是，项目在用完整图片生成视频的时候，头部在动，但是肩膀会保持不动，交接处就会产生比较扭曲的现象，加上--still后就可以防止交接处扭曲，但是整个视频就几乎只有眼睛和口型的变化了，看起来没那么自然。其他参数，也可以自己试一试。

![](img/fe5e4797b85438295526375cb674027d.png)

[full_body_2##bus_chinese_enhanced.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QXvob0vbLoFI6vxvvHYcU1qqnbW)

![](img/b03739532dd565adff6cbafad868c5d2.png)

[full_body_2##bus_chinese_enhanced2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VETTbw3k0oDfgvxhSLpc97JHngh)

## 总结：

整体来说，对质量要求不那么高的场景，SadTalker还是能用一用，毕竟是免费的。然后参数的话想要自然就用基础命令，想要完整图片生成就加上那两个参数。

最后这个环境如果是小白，搭建其实还是比较麻烦的，我想的是租一台云服务器，然后搭建好webui版的，大家可以直接上传语音和图片直接生成，甚至可以把语音克隆功能也搭建好。

有需要的小伙伴可以留言，要是人多我们可以搭建个试试。

## 再来几个演示：

![](img/b6e257c07cde087fb24d7533d36cda1b.png)

[full_body_1##chinese_poem2_enhanced.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DELVbBm1noXavuxxILTcxemfngd)

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>