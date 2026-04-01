# Task1：跑通Mobile-Agent Demo

> 来源：[https://datawhaler.feishu.cn/docx/RPYbd4FUHoGP3VxNNbScymoAnOg](https://datawhaler.feishu.cn/docx/RPYbd4FUHoGP3VxNNbScymoAnOg)

> 亲爱的学习者，你想变强吗？

> 你想拥有一个贾维斯雏形吗？

> 你想通过一句指令，让智能体自动控制你的手机执行操作吗？

> 通过本场学习，你将有机会获得这个能力！

> 废话不多说，先来看几段Mobile-Agent演示案例——

>

> 切记：心动不如行动！速速开始下面的实践体验吧！

>

> PS：如果想参与更多本赛道学习文档共建，可联系 ~

![](img/d6bbc165f426b5c8f2f6efd4aab2d1ce.png)

[MobileAgent V1 ShowCase.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SVNNba580ohPHtxML3ucHu1jnMz)

# 200 分钟 ：Mobile-Agent Demo体验指南

> **教程贡献者说：**

> 咱们本次学习活动主要是想教会大家如何使用Mobile-Agent框架，然后理解其框架原理，最后做出更加有意思的大模型驱动Agent操控手机的应用~

>

> Mobile-Agent（移动智能体）正逐步成为连接现实世界与数字世界的桥梁，Mobile-Agent: 强大的移动设备操作助手家族是一个可以支持移动设备及其他终端设备与人工智能深度融合的多Agent框架。

>

> 当前环节可让你在 **200分钟**** **内，跑通 Mobile-Agent Demo！

> **千里之行，始于足下，从这**里，开启你的 Mobile-Agent 学习之旅吧！

> 记得在每次完成学习后，写笔记打卡哟~ 及时总结沉淀能让学习事半功倍~

## Step0：申领大模型API！(点击即可跳转)

> 链接：https://bailian.console.aliyun.com/

> **开**通 阿里云百炼平台 ，获得 qwen-vl-plus 的限时免费使用额度100w Token，有效期为30天。

+   登录并开通百炼平台

![](img/601bf08bb392018647074a05f2a86579.png)

![](img/177051df3dd9aeb75f77a88553f656ab.png)

![](img/d53f6229b36abdc1d31f99971210904b.png)

+   查看免费赠送额度

![](img/67ade7dfe120177fdbfcfd101208ccc6.png)

![](img/ea56ceb3f0b266437dd43a0f4f94092c.png)

+   创建API-KEY（记得保存）

![](img/19b308ef6409d35954b3e7fbd0cdb6e1.png)

![](img/abc779f5d7810a93644520619edee51c.png)

![](img/ab3c78fe9efe9bd3896699f4df494f8d.png)

注意：请大家存储好申请的APIkey，切记不要暴露给别人~

## **Step1：下载 A**ndroid Studio（大约需要15分钟）

> 下载链接：https://developer.android.google.cn/studio?hl=zh-cn

> 在官网下载即可，就是下载可能得花好一阵子

![](img/9f86b1944a97747e33b54b4835f73a53.png)

1.  下载安装包

![](img/1bd96d7dd52abd48928862dc8730d1cd.png)

![](img/91e532ce253cbd8955ed16a102f99334.png)

![](img/82ed2ae95804ba2819179b5ab5b6ce61.png)

1.  **安装Android **Studio（大约需要10分钟）

> 下载完成后，打开应用(Windows双击打开对应的exe安装程序，Mac在应用汇总界面单击)

> 一直点Next和Accept，直到Finish即可(这个步骤，Win&Mac相同)

> [关于](https://blog.csdn.net/qq_38436214/article/details/105073213)Windows安装更详细的内容，可查看右侧评论中的链接

![](img/93af7cb08c6910f7eff7f0e9f478b5c9.png)

![](img/262ce55f7c2c0188cef6af716fc9e014.png)

![](img/080a4a740e6789fcd81e8a3d1b87823d.png)

![](img/979b907b341804c41b35a8c7f055ccd2.png)

![](img/e6d850d9f8303a8b6c922f3d8f4d3160.png)

![](img/1afd1545ed787a62dababa8502977e4b.png)

![](img/aae4ee68c6563f460b385e2455563065.png)

![](img/d661fe0092c53625b71bd9755393ec59.png)

## Step2：新建一个虚拟手机**（大约需要30分钟）**

1.  完成安装后，打开Android Studio，新建一个项目**(大约需要20分钟)**

![](img/b5f5d3a86a20c080fbf120b0271b8a2a.png)

![](img/50f14c1c6e7617365dcfeb2267929101.png)

![](img/a7f65b1eea812093b158adc271845e41.png)

![](img/71ab1aa194eab16771a9972055e1374b.png)

![](img/b4f864e2a1bf36263036e74675f6fcd2.png)

![](img/2dc05b2dca79c040333966c9189ba9a6.png)

![](img/9d3fd7a959dc14eb6ba8be9caf968ece.png)

![](img/b0a5f13c1c813ba4bc68f660ac1a7540.png)

1.  创建虚拟手机

![](img/2e83666c289c8f46d6eea8d8d8d6b888.png)

![](img/129959e9ae4f9b7e9fad0122bb8251ad.png)

![](img/21670f220303ff4a5c8faf85ec71d9ba.png)

![](img/b0eb44b12b73aaad7295ed3929ed8efe.png)

![](img/ee6399cf2bf60107af4d0c0b0304ece8.png)

![](img/c5d5fc8125cb90a24c9c21c4db1d89c5.png)

1.  启动虚拟手机**（大概需要5分钟****）**

![](img/70a076b8bb522d4a4c9a997cdb80ff7f.png)

![](img/6810763e0eb4211df9744673471cd3ed.png)

![](img/3bdf0f46241446d564fd2489c6968cee.png)

![](img/5306afd6ea0b15c0f6d82494c8ce094f.png)

1.  将谷歌日历移动到桌面

1.  双指从下往上拉模拟手机使用，打开应用汇总界面

1.  长按Calendar应用将其拖拽至手机桌面

![](img/52e95d84c79a77191859b0c766283dd7.png)

## Step3：安装Mobile-Agent框架（大约需要30分钟）

> [在开始本步骤之前](https://www.anaconda.com/)，请确保你已经安装 VSCode 和 Anaconda，并配置好Python环境

1.  在VSCode中打开一个指定的文件夹

> 打开准备用于配置Mobile-Agent环境的文件夹，新建一个终端

![](img/62f21de3c104863fe75d83a9f3d03087.png)

![](img/a06a4cf14822f5516f5ed87db991ffeb.png)

1.  下载 Mobile-Agent Demo相关文件

> 复制下列命令在终端中执行

```
git lfs install
git clone https://www.modelscope.cn/datasets/Datawhale/MobileAgent_V2_Demo_qwenVL.git
cd MobileAgent_V2_Demo_qwenVL
```

![](img/8b8a2e794db5db841901e27e69230909.png)

1.  **安装 Mobile-**Agent框架所需依赖(大概需要20分钟)

1.  逐行复制下列命令在终端中执行

```
conda create -n moblieagent python=3.9.19

conda activate moblieagent
```

1.  复制如下命令在终端执行，安装相关依赖

+   **Mac电脑运行这个**：pip install -r mac_requirements.txt

>

+   **Win电脑运行这个：**pip install -r win_requirements.txt

![](img/c5e633b0c7f80964ddb98b134c03eb01.png)

1.  修改run.py

> 修改 adb_path 和 qwen_api 变量

> qwen_api应为在[Step0申请的API-KEY](https://datawhaler.feishu.cn/wiki/BbEuwzZMXiWwxbkfFuHcflwrneg#Sg1rdGARfoLSZzx6m8AcojLPnLf)

```
# windows的路径未修改过应为&quot;C:/Users/{your username}/AppData/Local/Android/Sdk/platform-tools/adb.exe&quot;

# Mac的路径应为&quot;/Users/用户名/Library/Android/sdk/platform-tools/adb&quot;
```

![](img/deb1e7a62079b53c61c14873667f83ce.png)

## Step4：**30分钟**体验 Mobile-Agent Demo！

> 在终端处执行 python run.py，大概需要等30分钟左右~

> **运行起来就说明所有环境就绪，就能看到我们的Age**nt在终端处返回自己的思考和动作记录啦~

> 至此，我们就完成了一个贾维斯雏形~

> 快试试修改instruction，以及了解更多背后的原理，思考如何做出更有意思的作品吧~

>

> PS：下图的示例可能包含不同instruction的结果示意~

![](img/7bc26f53ff963493e3d40ec3946b77c1.png)

![](img/19a188cd46cf710217e68665b314fb07.png)

![](img/d9d12be230e2e630e1316a68b698d66f.png)

![](img/8f58ef5c10f7e86f9ae5e118a0e9e12e.png)

虽然有点慢，但他确实自己动手了！

![](img/5bad8d4fdd8c2aabf55ed3fc0924533b.png)

[日期&打开PlayStore.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HbQSbV5oLo5dNQxBWBdcnzCpn0g)

这里还有一个不同instruction的效果视频

<https://player.bilibili.com/player.html?bvid=1XBeGeNEE6&vd_source=7ff60130015e02431439b2db7bf480a5>

> **至**此！我们就跑通了！！！！！ Mobile-Agent 的 Demo！！！！效果就在上面！！！你看到了吗！！！！

# 🎉 恭喜你跑通了Mobile-Agent的Demo！！！！！！！！！！！！！！

## 👍🏻 快来给自己点赞！

## 🛎️ 别忘记打卡&在群里接龙！

> 大家完成打卡后就可以在表格里填写组队信息，积极主动寻找合适的队友

>

1.  完成速通手册，保存学习截图，并填写打卡-组队表

【腾讯文档】夏令营第五期MobileAgent-找搭子辅助表-Task01打卡链接

https://docs.qq.com/sheet/DTW50em50a2FCSnlX?tab=BB08J2

>

1.  自由组队，组队表是所有学习群共用的，可以跨群组队的❗️

主动寻找队友，添加好友备注来源，然后语音沟通组队意见吧❗️

>

1.  完成打卡的小伙伴可以去学习群接龙啦！

## 📅 然后迎接后续的挑战吧！

> **教程贡献者说：**

> 咱们学习活动跟Mobile-Agent赛事有关联，通过学习本学习活动的内容，我们可以掌握Mobile-Agent的开发方法，并学习尝试基于Mobile-Agent进行自己应用的开发和场景实现。

>

> Mobile-Agent这个框架只要把环境搭建好，上手其实还是蛮简单的，但是想要效果好的话，可能需要使用效果更好的多模态大模型，这部分大家可以自己尝试去获取并探索其他多模态大模型的API的使用方法~

> 

> 后面的学习我们还会邀请到官方来进行相关赛事解读和框架开发经验分享~

> 敬请期待吧！预祝大家都能学得开心，找到志同道合的队友拥有一段愉快的应用开发之旅~

>

> **更多详情见如下学习规划表：**

| ## 从零入门 Mobile-Agent |  |  |  |
| --- | --- | --- | --- |
| **Task** | **Task 描述** | **相关知识点** | **学习安排** |
| **Task 1：4天** |  | 1.  赛事环境配置  | [**开营直播**](https://www.bilibili.com/video/BV1CtsLeTEFr/) |
| **Task 2：****4****天** |  | 1.  官方赛事解读与技术答疑 | Mobile-Agent框架原理直播 |
| **Task 3：****4****天** |  | 1.  应用开发 | 技术交流会-腾讯会议 |
|  |  |  |  |

> 打怪难度(1~3⭐️)：⭐️⭐️⭐️

1.  熟悉赛题背景+环境配置

1.  跑通baseline，了解提交要求

1.  完成 **Task1** 打卡，组队

1.  思考创新场景，参与**头脑风暴会**

> 打怪难度(1~3⭐️)：⭐️⭐️

1.  学习官方直播分享：Mobile-Agent框架原理与实践

1.  精读baseline并尝试迁移场景

1.  写笔记，完成 **Task2 **打卡

> **打怪难度：⭐️⭐️⭐️**

1.  基于场景进行应用开发

1.  参与**技术交流会**

1.  开发应用，**提交演示视频**

### 祝大家开发顺利，学习愉快~ 多在群里提问和交流哟！

+   赛事介绍页：**https://modelscope.cn/bra**nd/view/Mobile-Agent

+   赛事提交链接：[**https://survey.aliyun.com/ap**](https://survey.aliyun.com/apps/zhiliao/I8AXazRGp)ps/zhiliao/I8AXazRGp

+   赛事报名链接（暂未上线）：

+   提问前建议先查看** **[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/I0sywipHNiwyCJkXSsfcH71Mnsb)** ****是否早已收录你的**问题~ （QA文档可任意编辑，助教和教程贡献者会不定时查看）

+   更多学习活动问题可见：** **[从零上手Mobile-Agent-学习者手册](https://datawhaler.feishu.cn/wiki/S0SKwmgVbig5Sik2kJqcVuEsnPA)

![](img/c895971b273425a7abfd1411afa8bacb.png)

当前学习材料目录

# 🍰 下面正式来了解一下赛题！

| ## 赛事名称（赛事暂未正式上线，稍后更新） |  |
| --- | --- |
| **赛事链接** | 赛事介绍页：https://modelscope.cn/brand/view/Mobile-Agent |
| **赛事任务** | **赛题目标** |
| **评分规则介绍** | **评委主观评分** |
| **任务提交格式说明** | ##### 格式要求 |
| **赛程安排** |  |
| 赛事奖励 | ![](img/a2d6c7d71917df66ee3e13a901d303fe.png) |
| **赛事要点** | 1.  找到一个创新场景，兼具实用性，思考清楚交互方式和流程，让智能体能稳定高效进行思考和执行 |

> 后续赛事正式上线还需要在开放原子处报名

**初赛：晋级x支队伍**

+   2024.9.1~**2024.10.10**

+   2024.10.12公示入围情况

**决赛**

+   **2024.10.20**

+   决赛需路演，现场颁奖

# ✍🏻 然后开启更多深入的学习~

## 大模型智能体(Agent)简介

> 大模型智能体是一种相对大模型应用来说，能自主规划、执行的类人达成目标的自主实体，一般可以理解为：

>

> **Agent = LLM + Tools + Planni**ng + Action + Memory

![](img/c03ce1f23351a2890d420c673c38f8c5.png)

[大模型智能体系统-介绍.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LyBhbwNeioVIlPxX2TwcemPAnVb)

更多详情可以查看视频：[5.4构建多模态手机智能体Mobile](https://www.bilibili.com/video/BV1QS411c7kc/)-Agent_哔哩哔哩_bilibili

## Mobile-Agent简介

> Mobile-Agent框架是一种可用于操作手机的大模型智能体系统，可以实现一句话自动操作手机，是纯视觉方案，不受操作系统限制~

![](img/52a546bb4fe71ea8b146344e0a786ae5.png)

[Mobile-Agent简介.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QiJ1bPKQ1oil9KxPIduc5Z7lngc)

更多详情可以查看视频：[5.4构建多模态手机智能体Mobile](https://www.bilibili.com/video/BV1QS411c7kc/)-Agent_哔哩哔哩_bilibili

## Mobile-Agent-v2 run.py解析

![](img/58be633419d8f16333909d4390084b88.png)

> Mobile-Agent执行操作的主要方式是对屏幕进行截图，OCR识别图片，结合用户指令、历史操作Memory进行观察、思考和行动（可通过ADB连接执行点击屏幕等模拟操作）

> 具体run.py是调用了哪些能力，又是如何实现这些功能的呢？

> 一起来看看下面的解释吧——

1.  **加载相关模型**

1.  **加载图标描述模型**：该模型负责描述每个图标的形状及颜色。如果描述模型的运行方式是local，则根据caption_model的名称加载Qwen-VL-Chat或者Qwen-VL-Chat-Int4，如果是api则可以调用qwen-vl-plus或qwen-vl-max

1.  **加载ocr和图标检测模型**：

+   groundingdino_model：检测图标位置

+   ocr_detection：检测文本区域

+   ocr_recognition：识别区域文字

1.  **进行****推理：**

![](img/0b656f8e0f30f15766cf62ef06d341e2.png)

1.   首先定义**三个Agent****，**对话记录等初始变量

1.  进入执行循环（注：推理开始并不是如上图所示先进入Planning Stage的，而是以Decision Stage开始）

1.  第一轮则获取目前屏幕的描述信息并基于信息判断截图中是否有键盘存在

1.  Decision Stage：将任务以及手机截图、环境等相关信息传给大模型进行推理得到output_action（格式为Thought、Action、Operation），并从output_action中提取thought、summary、action

1.  如果memory_switch为True则将action存储到memory中，并在下一轮推理时使用

1.  执行action中的操作

1.  等待5s，等待结束获取屏幕更新的描述信息并判断截图中是否有键盘存在

1.  Reflection Stage：若reflection_switch为True，则根据任务、动作完成前后截图相关信息与执行的动作进行反思，反思结果有A、B、C三级

+   A:操作正确

+   B:操作导致页面错误，需返回上一页

+   C:操作未发生变化

Planning Stage：若结果为A则更新操作已完成的内容，并在下一轮推理时使用；如果为B则执行返回操作；如果为C则重新进行推理

## 其他知识点

### 可以在VS Code中启动Android Studio的虚拟机

1.  打开VS Code后点击右下角步骤一所示标记，在终端中输入cd path/to/save比如我想把该项目保存到桌面上所以输入了cd desktop，再输入git clone https://github.com/X-PLUG/Mobile-Agent.git将项目下载到本地

![](img/7a84491dcaecd786faeac8f409fef540.png)

1.  选择Mobile-Agent的下一级Mobile-Agent-v2后点击打开

![](img/3d6c7b1306f860ea9c5d2980afce7c5f.png)

1.  将requirements.txt改为如下内容

```
numpy==1.26.0
torch
torchvision
transformers
modelscope==1.15.0
tensorflow-macos==2.9
keras==2.9.0
git+https://github.com/openai/CLIP.git
opencv-python
matplotlib
pycocotools
timm
SentencePiece
tf_slim
tf_keras==2.15.0
pyclipper
shapely
supervision==0.21.0
dashscope
```

1.  在下方命令行中执行conda create -n Mobile-Agent python=3.9.11 -y，安装成功后执行conda activate Mobile-Agent,进入环境后执行pip install -r requirements.txt。

1.  安装Android iOS Emulator插件，本人设备已安装，未安装设备点击禁用位置的安装即可

![](img/09b84d55b97b6cf3e6b97d8638026f71.png)

1.  安装成功后VS Code右上角会出现手机形状，点击小手机后再点击View Android emulators

![](img/ee18107292a450e9e78cfbba79cc4690.png)

1.  再点击我们之前创建的Pixel 8

![](img/9407c07e0ef622b1b651b999bcbd74bc.png)

1.  之前创建的虚拟机就会弹窗运行

![](img/f05b4f5831a92c9fe91e27fcd608a18e.png)

1.  将虚拟机中上滑出现的Calendar挪到桌面，并登陆完成初始化

![](img/9d08224b7daa36e5b78e611937350e91.png)

![](img/56b9d822ed5c8ea388e3065e9ff652d3.png)

![](img/81f31d75de3c9ff63dd1f8afa0894ccb.png)

### ADB还有许多玩法

在 Mobile-Agent 中，我们使用 adb 来与模拟器连接交互

ADB，全称为 Android Debug Bridge，是一个用于安卓开发的命令行工具，它可以通过 USB 或 Wi-Fi 与安卓设备通信。ADB 提供了多种功能，允许开发者在设备上执行各种操作，例如安装和调试应用、传输文件、访问 Unix shell 等。

通过adb，我们可以实验各种与手机交互的操作，

如我们可以使用adb shell input tap 100 200来点击屏幕坐标 (100, 200)；

或使用adb shell input swipe 100 200 300 400 500来实现从屏幕坐标 (100, 200) 滑动到 (300, 400)，持续时间为 500 毫秒

总之，通过让大模型输出 adb 相关命令，我们就可以实现让大模型控制我们的手机完成各项任务

### 真机调试（仅安卓）

前面已经提到了，我们可以用 adb 来连接我们的虚拟手机来实现操控虚拟手机，我们的真机同样也支持使用 adb 连接控制

首先我们需要打开手机的开发者模式

这里以小米手机为例，打开我的设备

![](img/f3d407c3fac9202562d1abd474fad232.png)

点击全部参数与信息

![](img/58ece75b16c1e2ec3edbc0a4731d607f.png)

多次点击MIUI版本，即可打开开发者模式

点击设置->更多设置->开发者选项，然后下拉找到调试处

![](img/578c381c29ccd2d4b42fcd4322e77ad6.png)

我们可以通过两种方式来连接我们的手机，一种是通过 usb 连接，一种是通过连接同一局域网连接

使用 usb 连接，我们需要在我们的 电脑上 提前安装 adb

adb 获取方式（如果你在上面已经安装 Android Studio 可以不再安装）：

从如上链接安装 adb 至你的电脑中

+   usb 连接

找到数据线，连接手机与设备

打开手机上的 USB 调试，如果是HyperOS系统，则需要同时开启USB安全设置（Security Settings）

用数据线将手机连接到电脑，然后选择“传输文件”

使用命令 {your adb path} devices 查看你连接的设备，如果出现则证明连接成功

+   局域网连接

确保你的手机与电脑在同一局域网下，在手机上打开 adb 无线调试

允许无线调试后，选择使用配对码配对。记下显示的配对码、IP 地址和端口号

运行adb pair ip:port，使用上面的的 IP 地址和端口号

根据提示，输入配对码，系统会显示一条消息，表明你的设备已成功配对

运行 adb connect ip:port 就可以顺利连接到你的设备

完成连接后，你可以运行 {your adb path} shell input swipe 100 200 300 400 500 来观察手机是否滑动，如果成功，则连接成功

完成连接后，将上面 run.py 中的 adb 路径修改为你现在使用的 adb 路径，接着运行，就可以在你的真机上测试了

# 更多精彩还在[Mobile-Agent 技术详解](https://datawhaler.feishu.cn/wiki/J63xwDCGVipxSdk7GOFcJcDdn6c)

# 参考资料

1.  [Mo](https://arxiv.org/abs/2406.01014)bile-Agent-v2: Mobile Device Operation Assistant with Effective Navigation via Multi-Agent Collaboration

1.  [Mobile-Agent](https://github.com/X-PLUG/MobileAgent/tree/main)

# 别忘了[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/I0sywipHNiwyCJkXSsfcH71Mnsb)！

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/I0sywipHNiwyCJkXSsfcH71Mnsb)任何想法都可以在学习群里交流或在   进行填写哟~

> 本次赛事的学习难度较大，Mobile-Agent框架还有很多值得深入学习的点，

> 下一个Task，我们将邀请Mobile-Agent核心开发者为我们分享Mobile-Agent的框架原理，

> 我们一起学习进步，以赛促学！也祝愿大家都能做出让自己满意的作品~

>

> 本教程主要由       写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 ****署名—非商业性使用**—相同方式共享 4.0 协议国际版许可协议进行许可。