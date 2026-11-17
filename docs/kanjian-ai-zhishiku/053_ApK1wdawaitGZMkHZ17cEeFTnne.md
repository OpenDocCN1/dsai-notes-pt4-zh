# Aimwise WaytoAGI 百家号文旅分享

> 来源：[https://jlqyxzzvbk.feishu.cn/docx/LrHjdcge7oEaSrxtQwzc4ryWnph](https://jlqyxzzvbk.feishu.cn/docx/LrHjdcge7oEaSrxtQwzc4ryWnph)

# **奉旨拆家**

![](img/1607a62c7f1bf686fc6344f272facb03.png)

![](img/5ac92a76d1f7f3ec88a01229e05cbdcf.png)


![](img/b69682faea9f86530502e351172ac3bb.png)

![](img/4c625fc878ffa986799525a141394703.png)

![](img/cbd3d1a99ce5531eb965d5347a4509fe.png)



![](img/defebfac641978e4dc9df14ab71371fb.png)

![](img/161d313c76a50ea1df566c7d5a78f460.png)

![](img/53a64c89b2bbcb02b9ed5b6d687a7e34.png)

# 通过地方特色和文献总结归纳文案的小技巧

## 了解城市，并探索可行性文案

+   确定你所使用的大语言模型 知道你所说的地方

![](img/0a219fb1a77538961917e6bc50b87801.png)

+   确定之后 就开始讨论 把他当个人看

![](img/269e186147b63e2638b25c5569d940c3.png)

+   给他一点参考，让他按照参考来模仿对应的文案

![](img/197e55a286c28908e59d573db2285239.png)

## 把文案写成视频执行脚本

![](img/6d1aa509375399d3d054799faccf900a.png)

+   https://chatgpt.com/g/g-0NDPWPZ9v-video-script

+   把我们刚才的生成的大致表现方向和文案方向丢进去细化成执行脚本

![](img/fa6520b5dd64430756db93b1a8ad00c1.png)

+   你可以大致看一下，确保是你想要的，注意这里镜头并没有按照旁白一句句区分开，你可以继续要求他细分到每一句每一个文案。

然后把你整理的最初版本的文案可执行脚本

复制到飞书

于是你就拥有了下面这个文档

[《AI我河南 璀璨华夏》](https://ax6cdbm88uc.feishu.cn/docx/M918dwlUroQVXHxBSwechcrJnsf?from=from_copylink)

Tips：这时候我们就需要一个有摄影/剪辑/后期经验的导演来把关这个执行脚本中的镜头调度，衔接，转场

也就是所谓的镜头语言

我们可以轻而易举的做出画面内容，但不是所有人都能掌握镜头语言的表现

我们经常能看到好的电影解说中不仅仅解析剧本

还会解析几个特别重要的镜头里交代的一些情绪，信息，以及剧情走向

这就是我们经常能听说的 镜头蒙太奇

# 特殊表现图片的生成方式技巧

![](img/5acff39bcc6c1f83da2e64e84eb78eff.png)


![](img/656a535112720e44601ef3f07d55b517.png)

![](img/acfd29c55b404bd390ce6a9a95bbeb74.png)

Tips:字体和元素

这些设计都有一个共同点

关键词：IPadpter，PhotoShop，图生图重绘

![](img/0f5e591d2e351e4e62ee0401bcc6a142.png)

![](img/15fb5b979ebf5530cfb4ad5d6c32d34f.png)

![](img/b0737b75def4765a2042a4fa8e2b82ee.png)

![](img/e4c94c06bc70efccf2fd2b532599241d.png)

![](img/29f133499f68da7b5922b113f66f58e5.png)

## 第一个方法：边缘类处理模型生成

## 训练目的与要求

要求：尽量实现多材质或者背景的海报。

目的：练习熟悉Controlnet的不同处理模型的效果

![](img/9d7eec53fc3629f33965ac434159a4a4.png)

![](img/20e15f0918d1dd666a1b436422bcb6d3.png)

## 选择文生图，输入关键词

+   参考正面提示词：提示词可以根据自己的需要自己输入，不要照搬。

gmasterpiece, best quality,artist logo,3D,steampunk, machine detail,cinematic texture,ground projection,product lighting,solid background,depth of field

+   如果效果很差，可以考虑换更换大模型。

+   当关键词效果不好时，可以适当使用lora提升画面细节和美感

## 启用：Controlnet的Canny/Lineart等边缘类处理模型

+   选择Controlnet插件，点击启用。

+   在Controlnet图框中导入需要处理的Logo。

+   这里可以尝试不同的处理的模型，也可以进行叠加。但是并不是意味着叠加的Controlnet模型越多越好，过多的控制可能会导致Ai的可能性被限制住。

![](img/47cbdff9d58f9370ca2310e1cce3bcaf.png)

## 生成图片

---

## 第二个方法：光影处理模型

## 要求/目的

传统上想要生成创意文字/logo需要运用c4d等耗时费力的工具，运用stable diffusion我们可以达到相同甚至超越的效果。

理解不同controlnet模型的原理，根据不同的情况进行灵活运用；

注意前期的创意策划工作；

操作过程中，利用控制变量的方法进行参数调整，

## 案例图展示




![](img/bfb2ee9a2e63c39a83718faf0919b236.png)

## 文生图/大模型选用

+   在文生图模式下进行

+   根据创意策划，选用适合的底模型

+   根据需要使用Lora模型

+   案例模型选用：CoMix_v3.1科幻plus_3.1（以首图为例）

## 关键词编辑

+   根据创意策划，编写关键词

+   关键词不求一次成型，可根据出图效果进行调整

+   案例关键词

![](img/f46612daed12ed6c36dc3cd3671bdaa4.png)

## 基本参数设定

+   较小的迭代步数以及图片分辨率有利于更快出图来测试参数

+   推荐参数设定

![](img/e5a7d451fe77e662c8a137262dbe101e.png)

## controlnet的运用-素材准备

+   在Photoshop中新建一个与stable diffusion当中输出分辨率相同的画布

+   将我们需要生成的logo/文字放入到画布当中，根据创意策划调整文字大小和位置

+   根据需要使用白底黑字/黑底白字（简单理解：当主体设定为黑色在图片中为阴影，当主体设定为白色则为发光的部分）

+   文字选用特别的字体会产生更好的效果

+   过于复杂/线条过多的logo难以用该模式生成效果

![](img/d0a21da8af15f376bf2eb927b24dc861.png)

## controlnet的运用-参数设定

+   我们运用brightness和qrcode_master两个cn模型

+   分别上传我们准备的文字/logo素材

+

+

+   预处理器选用“无”；或可根据需要选择invert，即反转素材当中的黑白区域

+   案例参数如下

![](img/61aeac12743b7c00ff58f7593a981ad4.png)

![](img/08cfd05909776b306febfeb344698cb3.png)

## controlnet的运用-参数调整

+   根据大量测试，brightness的控制权重在0.3-0.5较为合适，让引导区间处于中段或中后段，有利于更好的让主体风格化。而qrcode_master需要从0到1进行全程引导，更有利于确保字体/logo的轮廓能够清晰辨识。而控制权重的最佳数值所在范围较大，受到底模型及关键词的影响，这个数值分布在从0.6-1.5的范围内。

## 第三个方法：风格迁移模型

## 找参考

找到一个想要模仿的风格参考图，（注意参考图与想要生成的图片比例尽量一致）进行关键词反推后发送到文生图

![](img/e10244e0fdaa7b92de138f09a88bbb94.png)

## 基础操作

选择与参考图画风相似的大模型和lora，设定合适的参数

## 开启controlnet边缘类模型

导入需要处理的logo，调整合适的cn参数

![](img/e6578f6944341ef42cd7fd96fb1c6dd8.png)

## 开启controlnet IP adapater功能性模型

导入风格参考图，调整合适的cn参数

![](img/f031a331146d25b14c7b26871bf489ac.png)

## 生成图片


![](img/5f11cd16965f62b3dde93dd11a3b961d.png)

该方法可以快速进行风格迁移和融合，提升出图效率

---

# Deforum的快速转场制作小技巧

Tips:如果不能一次性解决问题，就分开解决

这个方法最经典的案例就是 86年西游记 大圣跳上山的拍摄手法

## 插件效果

生成类似"瞬息全宇宙"的炫酷转场视频

## 安装地址

网址：https://github.com/deforum-art/sd-webui-deforum.git

![](img/c2391b18d755f574510c0cba84c833ac.png)

## 参数设置

## 一、运行

![](img/964328372b6f19af19865c035948871c.png)

+   "采样方法"可更换

+   "step"步数、宽度、高度、随机种子值可自由设置

+   "批次名称"为输出文件夹名，默认保存在图生图文件夹内，名称为插件名+时间

## 二、关键帧

注意所有帧的数值设置必须从0帧开始，否则会报错

![](img/34a2a9ae94765995f4d0218c69023e61.png)

### "动画模式"

下选择制作动画的方式

推荐使用"3D"模式进行控制

### "边缘模式"下选择图像像素生成方式

"replicate复制"模式为直接拉伸生成；"wrap包装"模式为从相对边缘提取像素进行生成，推荐选择该模式

### "最大帧数"设置动画生成所需的总帧数

![](img/e4fba4cb499c56b5a638e7b8067e70ca.png)

### "Strength"设置下一帧被上一帧控制的强度，可以保持内容连贯性

"CFG、seed、subseed、step、sampler、checkpoint"数值可随帧数改变而改变

填写方法为 -- 关键帧：（该帧的内容数值）

### 运动参数

![](img/c61f2fd362f8c8a2f232d372cb63c2a0.png)

"运动参数"为摄像机的运动参数，填写方法为 -- 关键帧：（该帧的内容数值）

+   缩放参数：大于1放大，小于1缩小，等于1不发生变化

+   平移x：正数摄影机往右，负数摄像机往左，零不发生变化

+   平移y：正数摄像机往上，负数摄像机往下，零不发生变化

+   平移z：正数摄像机向前，负数摄像机向后，零不发生变化

+   旋转3D X轴：正数摄像机往上，负数摄像机往下，零不发生变化

+   旋转3D Y轴：正数摄影机往右，负数摄像机往左，零不发生变化

+   旋转3D Z轴：正数摄像机顺时针旋转，负数摄像机逆时针旋转，零不发生变化

### 噪点参数

![](img/254c99d45553efde092ec6070ca89664.png)

"噪点参数"为图片生成时的噪点值变化，数值越小，画面越稳定，数值越大，画面的多样变化性越大

### 一致性

![](img/25763d00ada9083ea0138e41b787f767.png)

"一致性"为保持颜色连贯性的算法

+   LAB：默认线性算法，接近人眼效果，推荐使用该算法

+   LSV：平衡鲜艳颜色，具有一定诡异性

+   RGB：在红、绿、蓝通道中强制执行无偏差颜色较量，可能导致彩色伪影

### 防模糊

![](img/9f2f272de25a392cff01fa638044eefa.png)

"防模糊"默认数值较小，当镜头速度较快时，适当调高数值有利于画面衔接

### Depth Warping&FOV

![](img/11640e3b2ebf8394eb2557d33548ccfc.png)

"Depth Warping&FOV"，仅在3D动画模式下能启用

FOV数值越接近180，图像深度越小，摄像机运动速度越慢；数值越接近0，图像深度越大，摄像机运动速度越快

## 三、提示词

![](img/4aaf4100e8bb6c60e7f61c6885e58ec9.png)

+   在提示词框内按JSON格式输入不同帧数时的提示词

+   可在该网站进行格式检测，提示红色会报错，绿色为正确格式，网站：https://odu.github.io/slingjsonlint/

+   在正向、反向提示词框内输入动画生成时无需改变的通用提示词

## 四、初始

![](img/1eec273801379bac061b78405fe49f5a.png)

设置初始参考图，勾选"使用初始化"进行启用

将图片链接复制到"初始化图像"内

注意链接外部如有引号需去除，内部不要有中文，数值间的横杠为反斜杠/

## 五、Controlnet

![](img/d70a709d60363aff9af7858c1655b7af.png)

勾选"启用"后，开始使用controlnet,默认有5个controlnet模型可以启用，

选择合适的cn预处理器、模型、权重等参数

（注意无法预览图片，需要将图片链接复制到路径文本框内！注意有图片蒙版路径，但是不能使用）

## 六、输出

![](img/ea83c339cd887d3c85c98e72ca0cbe51.png)

FPS：帧速率，代表每秒出多少张图，最大帧数/帧速率等于生成动画的时长

可设置添加背景音乐、放大图片、删除图片等操作，不建议在deforum中进行操作

![](img/4c6bcb2d20d1fb4bd0dc09ebdd3095d4.png)

视频生成完成后，点击"生成后点击这里显示视频"，可以在网页进行预览，默认保存至图生图文件夹

## 七、预设使用

![](img/0af4fdfece8893c3aca455d0b89991d8.png)

+   复制预设txt文件路径，粘贴到图片预览窗口底部的预设文件地址栏处

+   点击“load all settings”加载预设

![](img/58dce739b25dd9a4981d79f131f09484.png)

预设加载完成后，可替换模型、初始化图像、提示词内容等信息

![](img/86f2286cf3f8d39767849aae399cf654.png)

保存调整后的预设，保存到已有的文件夹路径，点击“储存设置”后，自动创建预设文本，需手动保存初始图片

---

# AnimateDiff转场

![](img/8f004d09e801d03058632c9cb49b233b.png)

![](img/f2c4f56530ecd1f60e988b04a61f4e86.png)

[Animatediff转场.json](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SeHEbjvw7oJ0AgxIgC7cXnZin9d)

# EBS丝滑转绘的小技巧

## 前期准备

## ffmpeg安装

网址：https://www.gyan.dev/ffmpeg/builds/

下载更新的安装包

![](img/4631440121e253229f79768683a0f786.png)

解压安装包，解压到合适位置

复制ffmpeg文件夹的bin文件路径，win+R打开运行，输入指令sysdm.cpl

![](img/34b4abf47f09eaef5b4c5478133d0884.png)

![](img/be60cf7eb97970164b66e150a7194f62.png)

高级 -- 环境变量

系统变量 -- Path -- 编辑

![](img/7ec948d424d9dde5109c131aad66cb27.png)

![](img/e1cc64d1a5c4f16273231286bafb0156.png)

新建 -- 将ffmpeg路径复制其中

确定 -- 确定 完成该步骤后，重启电脑

win+R打开运行，输入指令cmd，打开命令指示符 -- 输入ffmpeg -version，如果能弹出内容证明安装成功，如果没弹出，重启电脑或重复上述步骤

![](img/52ebeca6703bed04a8f2573159a56278.png)

![](img/b27da52ed6484069c9d58cbc332bdc83.png)

## EbSynth安装

网址：https://ebsynth.com/

下载后解压即可

![](img/b0709b5fe02c03a73e478650b4a695bc.png)

拓展插件安装：

https://github.com/CiaraStrawberry/TemporalKit

## 开始使用

预处理

![](img/c3ab39f6f7cf03adf0f0c955fa958872.png)

+   上传视频

+   每边张数 1 ，高度设置为原视频的高度（当电脑显存不够时可降低高度）

+   帧率设置为原视频帧率，开启ebsynth模式

+   设置目标文件夹

+   开启批量处理（边缘关键帧数默认2，可调为0）

+   Ebsynth-分割视频

![](img/39448dfaf80dd76e46bf599c451efc8b.png)

## 图生图

将分割视频的第一帧拖入图生图界面，选择合适的模型和参数，反推关键词，调整重绘幅度

![](img/adf47df2a8ca13988c074fe709c5c913.png)

开启合适的cn模型，调整合适的cn参数

![](img/7a7932bed66421dbddf134f42de987a0.png)

## 批量处理

当图生图单帧图片效果满意时，保持参数不变，种子值固定，进行单帧图片的批量图生图，保证输入和输出图片名称对应

![](img/14cdc8b18fa2a5f382c4a2e9eea4bc25.png)

## ebsynth流程

![](img/c1cc794cbc3a48800bbba3fac8351624.png)

+   选择文件夹

+   导入视频

+   设置帧率，调整边数，设置视频高度，设置最大帧数（可在剪辑软件查看，或计算帧率*秒）

+   点击进行预处理

## 使用ebsynth

打开ebsynth

将keys文件夹拖入keyframes

将frames文件夹拖入video

点击run all生成视频

如果视频被拆分为多个文件夹，则需重复上述步骤

![](img/4f04d648d7d71a068e74af45e6bb523d.png)

回到temporal kit

点击重组ebsynth，生成视频

![](img/acecd2f4faa62b8dd258080b0a7c993e.png)

# 首位帧提示词技巧

![](img/cc38ad55643f46480f1a3ff2a2b782a6.png)

https://chatgpt.com/g/g-zAYi4ILox-yun-jing-da-shi

![](img/22a908e7269c48ccf50e557df9fdc35d.png)

![](img/5fdb33767572f9adfb12437411f49ae7.png)

Tips:省钱提效小技巧

# 使用AE制作现阶段无法AI呈现的动画动态小技巧

目前市面上所有adobe软件建议直接买正版

TB搜 500一年 很便宜 全家桶

![](img/2b4df637989cb46a954aafb2311bcba5.png)

https://www.talkae.com/

AE学的不是软件 是插件 上千款插件 百万计模板

学会AE插件套模板 你已经比别人领先99%了

但是你要是插件全买齐 估计得要个大几万 没必要

几十块钱能搞定得事  别浪费太多时间

![](img/372ec2b85d1b1d28c6e1ee2c84bb3827.png)

## 投机取巧案例1

![](img/df1d366a56b68b9d812b12002abcf075.png)

[案例mp4.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UKABblrk4oYrCBxaHLQcpSKhnAf)

AE模板+自定义文物内容+EBS稳定重绘+Deforum穿梭过度

![](img/c2d6dd57e775cdbcfc42a75b02fb2f76.png)

## 投机取巧案例2

![](img/8d79296104c3237345d5ffe0904da2eb.png)

[诗画中国_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ERqtb5iI7oUmXZxzhuJcWwCunpb)

PS将图像分层，越多越好，在AE中利用AE三维图层和焦点得变化，做假三维出前后景的效果

最后给大家几个关键词可以针对学一下

AE 图钉动画 用于简单的将静态图层动起来

AE Generate Fill 用于动画图层的修复 也可以用来简单的去水印之类的

KeyFast 图层简单动画预设

![](img/35459b5aeb35afc3897a82f181606514.png)