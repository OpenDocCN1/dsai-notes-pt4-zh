
# 使用`SadTalker`让照片说话

> 来源：[https://xmgtoagi.feishu.cn/docx/AwFTd7J0PolUJXxIvgQcIKbZnfb](https://xmgtoagi.feishu.cn/docx/AwFTd7J0PolUJXxIvgQcIKbZnfb)

## 项目简介

利用目前主流的开源程序让照片说话唱歌，达到基本的唇形同步

`https://github.com/OpenTalker/SadTalker`

可以独立使用或者作为插件放入`stablediffusion`

## 视频教程

<https://player.bilibili.com/player.html?bvid=1Dc411W7V6&vd_source=35e62d366d8173e12669705f7aedd122>

但是对于编程、`python`、`conda`不熟的，强烈建议使用这个整合包：

**史上**最炸裂版AI工具箱来啦，`SD`-AI绘画、`VITS`文本转语音，`wav2lip`、`sadTalker`唇型同步，视频修复，支持`A卡`！

我用夸克网盘分享了`EZ-AI-Starter-v0.9.8.zip`，点击链接即可保存

链接：`https://pan.quark.cn/s/1d5ca9f57f5c`

视频地址：`https://www.bilibili.com/video/BV1ga4y1u7Ep/?spm_id_from=333.337.search-card.all.click&vd_source=35e62d366d8173e12669705f7aedd122`

## 具体步骤

![](img/dcd9f07969b6f87a89a82194fbf40b7d.png)

点我启动，在启动界面中，选择音频、图像口型同步的下载图标：

![](img/00fda9d877356aea0012fa4e5ee400b2.png)

下载完毕后如下：

![](img/1f4118df38bde816b01228864e7612be.png)

启动应用，等待

![](img/46af24c815badd4759abac32b645ab55.png)

会弹出一个独立的窗口（而不是你的默认浏览器）

![](img/970143e2618a4276f3238a5e4eabf819.png)

选择`sadtalker`同步器，分别导入图片和声音，

图片预处理方式中，`crop`只截取图片的头部，`full`就是保留整张照片，下面的勾选项已经有文字解释，自己可以试几次

点击`generate`

由于涉及到视频的推理和转换，输出时间要远远大于ai绘图和`sovits`的声音推理，做好等待的准备。

下图中，我导入了一个13秒的音频，预计需要3-5分钟（3060 12g显卡）

最终生成的视频长度与音频一致，如果想制作一个长视频，最好是剪成小段，分别推理，然后合并。

![](img/8694283a32513af84aea5e1c5a455d2e.png)

完成后，可以打开输出目录或者下载视频文件。

![](img/410ef30a9974f39862615ac1732f6a36.png)

看看效果

[tmp606i48sh##小星星-0-100.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DxybbSn8SootP2xjVkScEfaXn0K)

[tmpjl_co18w##赐我测试用-0-100.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WDwqbL60mosoFsxrK6dcrPM4nug)

带面部修复的效果：

![](img/0b2107dbcf4688c509350a00f13be70f.png)

[tmphuol9j0x##1_aj3_Vocals_5-0-100_enhanced.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WB8RbcqbIoxiUqxXEw0c3BbZnMf)

## 安装包下载

`https://pan.quark.cn/s/1d5ca9f57f5c#/list/share`
