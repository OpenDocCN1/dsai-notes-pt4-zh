# 实现让照片中的人说话

> 来源：[https://xmgtoagi.feishu.cn/docx/NTZwdDNKtoteEDxm8S3cxJ7Pngd](https://xmgtoagi.feishu.cn/docx/NTZwdDNKtoteEDxm8S3cxJ7Pngd)

项目地址（内含详细说明文档）：[https://github.com/](https://github.com/OpenTalker/SadTalker)OpenTalker/SadTalker

先上效果：输入一张照片和一段音频，实现让照片中的人说话

![](img/e1ff68376b99cf774c1ba932d83f4452.png)

[demo.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PsoEb3eVeoVuKHxHtcTcgKpkn0g)

### 运行方式一：Windows直接安装

####

#### 零、前置工作

需要git，科学上网， python3.8+ 环境。如果你不知道这些是什么的话，那这个方法不适合你。

####

#### 一、下载ffmpeg

1.  登录 https://ffmpeg.org/download.html，点击Windows，选gyan.dev

![](img/9878e2bd858dd8c24d3e10ea06ec44a1.png)

1.  下载Windows构建全版本的FFmpeg

![](img/9e861c6d5e40db0be54772923183936e.png)

1.  解压，重命名解压后文件夹为FFmpeg

p.s. 解压需要[安装7zip](https://www.7-zip.org/download.html)，好用的轻量级压缩工具。

1.  配置PATH环境变量，添加完后记得点确定

![](img/d3b3626767270117c35aab0b10e0677b.png)

![](img/f2d7b35894bf4dbc8f91c2c35f7403ed.png)

1.  检查是否安装成功

```
打开【命令提示符】，输入ffmpeg，如图所示
```

![](img/7acdb7b4ec0d3f1268a84202d8419ad2.png)

#### 二、下载SadTalker代码

git clone [https://github.com/Wi](https://github.com/Winfredy/SadTalker.git)nfredy/SadTalker.git

![](img/823c91ca847337c047b009020fca6716.png)

#### 三、下载SadTalker模型文件

参照[《官方文档》](https://github.com/OpenTalker/SadTalker#-2-download-trained-models)有三种方式，其中百度网盘是不用翻墙的，模型文件涉及以下两个文件夹：

+   [checkpoints](https://pan.baidu.com/s/1P4fRgk9gaSutZnn8YW034Q?pwd=sadt), 提取码: sadt

+   [gfpgan](https://pan.baidu.com/s/1kb1BCPaLOWX1JJb9Czbn6w?pwd=sadt), 提取码: sadt

把下载好的模型文件放到checkpoints和weights下面

![](img/14a36d4d8f278abaea0348c3806e3f27.png)

![](img/8e5b6a113c225521828dd9ebb276c600.png)

####

#### 四、启动webUI

第一次运行需要下载依赖，会有点慢

![](img/36cb3881e4d46b7a12fb31e1aa9d02fa.png)

####

#### 五、浏览器运行demo

默认地址为http://127.0.0.1:7860，拖入图像和语音，点击Generate即可进行合成（注：SadTalker/example下有样例可以拖进来试）

![](img/b5c3208805dbaeff5ba7f241a71c35f8.png)

####

#### 常见问题：

1.  点击Generate后右上角提示【Expecting value: line 1 column 1 (char 0) 】

关掉代理服务器，请参考[[Bug]: Something went wrong Expecti](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/9174)ng value: line 1 column 1 (char 0) #9174

1.  脚本内拉取模型文件失败

解决方法：手动下载好模型文件，放[在w](https://github.com/OpenTalker/SadTalker#-2-download-trained-models)eights下面。模型文件下载请参考这里。

![](img/f88278b186ae5201328fb6984ff721b2.png)

###

### 运行方式二：Colab（需科学上网）

1.  访问 SadTalcker 的Colab链接，复制到自己的云端硬盘

![](img/7d88ccb310e3cbbb0879f80cec4b4144.png)

1.  点击不同单元格，依次运行

![](img/e63a9f75fd9396364eedd527e1e7be08.png)

1.  选择图片，运行，即可完成合成

![](img/aa3c2c4fd5392220f40071ebf54d696a.png)

1.  运行

![](img/8805e28e391cfd2f8aadaf617ac9187b.png)

1.  预览结果

![](img/d490e2a583ee79ecc5f73f3e0f9f21ba.png)

####

#### 常见问题

1.  pip install 时提示 python3.8: command not found

![](img/2650153de729cc31f450b875380bd43a.png)

解决方法：手动安装下python3.8

```
!wget -O mini.sh https://repo.anaconda.com/miniconda/Miniconda3-py38_4.8.2-Linux-x86_64.sh
!chmod +x mini.sh
!bash ./mini.sh -b -f -p /usr/local
```

![](img/b3faa72f75e883e9d3fb68fac4ab59cf.png)