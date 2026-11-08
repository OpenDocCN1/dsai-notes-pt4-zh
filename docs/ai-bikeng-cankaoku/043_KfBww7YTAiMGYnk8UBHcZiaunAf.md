# OBS软件安装/插件配置

> 来源：[https://xodnytdcaw.feishu.cn/docx/LF06dQZtQoEzH0xAmefczY0gnPc](https://xodnytdcaw.feishu.cn/docx/LF06dQZtQoEzH0xAmefczY0gnPc)

## 一.OBS下载地址

官网https://obsproject.com/

历史版本https://github.com/obsproject/obs-studio/tags

![](img/de9f7b6ab87ec8bc669e0942fabebf5d.png)

## 二.OBS设置参数

> 我电脑2K  N卡  仅作参考  版本31.1.2

### 输出-直播

![](img/e87078d437ca7bc98fc6c13c15b33369.png)

### 输出-录制（高清参数）

> 配置说明

+   勾选 “生成没有空格的文件名”，避免部分播放器 / 剪辑软件识别文件时出错。

+   NVIDIA NVENC HEVC （相比 H.264，它的核心优势是相同画质下体积小约 40%）

+   禁用重新缩放输出设为2560×1440，此时不需要启用缩放算法，因此无画面压缩

+   比特率25000 Kbps  就是码率25 Mb/s  画质足够清晰，同时减少存储占用

+   预设P7（最高质量）+ 二次编码，H.264 编码兼容性更好

+   H.264 的main配置兼容性更广，且画质损失可忽略

> ⚠️另外我在源-显示器-滤镜-锐化：0.18

![](img/2303f4606abf5e8d50702c68ee8aa699.png)

### 视频(输出分辨率）

![](img/3bef551c839a2ca5ee341e2d46e44f46.png)

### 声音调优

![](img/ccc3fbb237857a61c056b4df3bdd857e.png)

![](img/06d892914d0140ff634c2f9a2b4fb9f2.png)

> 剪映人物说话声音处理

> 1.声音上调 **80%波峰出现橘色警示线**

> **2，观察音量指示器，红**线代表播放过的最高音量，调整策略：整体保持0 不要超过6 否则听起来过曝

> 3，对于声音波动大的，点击**“响度统一”**

>

> 参考（音频-音量直接输入再微调）

> 人声6以下（7db）

> 背景音乐-20~-12（-7.7db）

![](img/fbf3d403b777d9df8837bc01ce25d590.png)

## 三.OBS插件/辅助软件推荐

> ⚠️此脚本不支持最新版OBS，建议使用31.1.2以下版本

### 缩放脚本：obs-zoom-to-mouse

下载地址：https://github.com/EasyChris/obs-zoom-to-mouse/blob/main/obs-zoom-to-mouse.lua

教程https://blog.csdn.net/2402_82616859/article/details/152797555

下图是我汉化（2026/01/21[**）后的界面，如需1对**](http://742112.xyz)1远程协助，请前往：742112.xyz

![](img/3cd61c696d3621b1daa4c232171b2c07.png)

![](img/4bdb4e6008d97181142cf0237fd53000.png)

### 源单独录制插件：Source Record

> 下载地址https://obsproject.com/forum/resources/source-record.1285/

> 注意下载便携版，将压缩包的文件直接拖入Obs安装文件夹即可

> **在源里添**加显示器与摄像头，给摄像头使用插件后，这样你即使录制的是显示器，摄像头的画面也会单独录制，可以在后期里用，加圆形蒙版，替换无需操作的一些片段内容。

> 使用插件

> 摄像头-滤镜-效果滤镜**-**Source Record

> **⚠️注意：文件命名格式前面要加点东西（如单**独），不加的话插件就是你添加了也不会生效

![](img/22f857787c8906d8806befe1a2edd02b.png)

![](img/faae501d4cd22e333b4b7df958d2b55b.png)

### 摄像移动插件：motion-effect

motion-effect：https://github.com/CatxFish/motion-effect

保姆级教程：https://blog.csdn.net/2402_82616859/article/details/157098663

| **阶段** | **坐标** | **移动描述** | **移动距离** |
| --- | --- | --- | --- |
| 初始待机位置 | (1725, 489) | 摄像头原始初始位置（未触发热键时） | - |
| 第一次按快捷键（前进） | (1735, 539) | 向右+向下移动（XY 轴同步偏移） | X：10px；Y：50px |
| 第二次按快捷键（后退） | (1725, 489) | 向左+向上返回初始位置（沿原路径往返） | X：10px；Y：50p |

![](img/3efa5dd584b7664f07963065a7046f7b.png)

### 辅助软件：iVCam

在obs调用手机作为摄像头

手机版：需要加**🛰️dqtx33****有优惠**

Windows下载：https://www.e2esoft.cn/ivcam/

添加视频采集器-e2eSoft iVCam

滤镜-效果滤镜-图像蒙版/混合-Alpha 蒙版（Alpha 通道）

![](img/83843a13b2cb4ced84ee7a6c94f5402c.png)

路径上传图片如下

![](img/f6fe9c479744bb2d73f810e25e2b6796.png)

在视频采集器下加效果图

![](img/85604c9405096c258b013df908e87d6c.png)

![](img/71a06fd0f54761353d7ff833140ce7fa.png)

### 辅助软件：Broadcast

下载地址https://www.nvidia.com/en-us/geforce/broadcasting/broadcast-app/

打开Broadcast视频-虚拟背景-替换，上传你的背景图

![](img/3a94133410713eb71f33204b5b9950f5.png)

![](img/4e29a22e822ed783cf07bd9b87f91196.png)

### 辅助软件：SwordSoft Mousetrack

> 官网：https://www.swordsoft.idv.tw/mousetrack/

> 一款 Windows 平台演示增强工具，核心功能为鼠标聚光灯、点击动画、屏幕标注、局部缩放与击键显示

>

+   免费版（无法保存设置），

+   付费完整版（可保存配置，微软商店购买）6.99美元 差不多50大洋

> 绿色版找dqtx33

### 辅助软件： Zoomlt屏幕标注

![](img/49e7d07dcd76da2da6f23e74e6bdef30.png)

| 快捷键 | 功能 |
| --- | --- |
| Alt+K | 开启缩放模式 |
| 单击鼠标左键（在缩放模式下） | 开始绘图 |
| 双击鼠标右键（在缩放模式下） | 停止绘图 |
| 按住 Ctrl 键（缩放+绘图模式下） | 绘制矩形 |
| 按住选项卡（缩放+绘图模式下） | 绘制椭圆 |
| Ctrl + Shift（缩放+绘图模式下） | 画一个箭头 |
| W（缩放+绘图模式下+输入英A） | 白板 |
| K  (缩放+绘图模式下） | 黑板 |
| 空格键（缩放+绘图模式下） | 将光标居中 |
| R（缩放+绘图模式下） | 红笔 |
| Ctrl + Z、E（拼音状态下） | 擦除最后/全部 |
| Shift + G | 绿色高光笔 |
| T | 输入文本 |

### 辅助硬件：6位数自定义键盘

https://www.bilibili.com/video/BV1yyqUBTEyh/?spm_id_from=333.1387.upload.video_card.click&vd_source=206031f494850e57fd6c92ace02b1bed

## 四.常见问题

### 1.声音与画面不同

声音里面调节延迟

![](img/3efed7fd54c9220f2fc51dcd5ae84890.png)

### 2,截屏时“截不到”OBS界面常见原因与解决办法

![](img/1f656830e6982ca139820e793f250b0e.png)

### 3,obs虚拟摄像头不见了

导航到OBS安装目录下data\obs-plugins\win-dshow，双击virtualcam-install.bat虚拟摄像头就看见了

![](img/7a0f7541d750699f5b67c43ec8679a7a.png)

### 4.OBS插件汇总

+   iVCam---摄像头调用

+   Broadcast----摄像头自定义虚拟背景

+   SwordSoft Mousetrack---聚光灯

+   obs-zoom-to-mouse.lua---鼠标缩放脚本

+   Source Record----单独录制插件

+   其他材料：背景图  摄像头圆形蒙版（可选）