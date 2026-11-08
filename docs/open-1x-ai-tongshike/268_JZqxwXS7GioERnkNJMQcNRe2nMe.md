# Mobile-Agent 开发经验分享

> 来源：[https://datawhaler.feishu.cn/docx/THOPdmVvOoRItZxiAOfc1O1jnMd](https://datawhaler.feishu.cn/docx/THOPdmVvOoRItZxiAOfc1O1jnMd)

# 1、我们队伍做的是什么？

在8月25号那场头脑风暴会议上，就看着这个idea5感觉有搞头，于是就加入了这个队伍，就此组成了一个5人的草台班子，刚开始的idea是这样写的，总之就是聚焦在在线会议上，但后面讨论时就发散了思维，找到了个更能落地的场景。

![](img/c608f9e2fe2cdec9253384109989d6ad.png)

这上面的内容其实大多都已有现成的产品了，再用Mobile-Agent做一遍其实意义不大。当时我灵光一现想到了Datawhale的一个比较有需求的应用场景。

下面这些红框内的需求主要是我提的：

![](img/e3ee85bc33d28d384c1a52d541970bf6.png)

我之所以想到这些功能，是因为Datawhale的直播和别人的直播不大一样，Datawhale的直播主要是转播的会议室内的内容，这中间有很大的gap：

1.  做分享的嘉宾没法直接看到大家在B站，视频号的反馈，主持人也不方便直接打断问问题，往往都是结束后统一回答的。

1.  我作为观众时，不知道什么时候讲完，怕跳过了回答我问题的时间。

而我就想到，那些游戏主播，屏幕上其实都有个小框框来展示观众们的弹幕的，如果能把这个小框框带入到直播间的会议室里就方便多了。

# 2、对Mobile-Agent的一些开发经验

## 先复现出论文效果

在参加这期Mobile-Agent的夏令营前，我就已经久仰其大名了，对它的水平也有了解，但跟着教程走完整个流程后，我试着复现论文里的例子，但是发现这模型做不了什么高级的动作，其水平和论文展示的差了很远。

![](img/9c684f03644b758fa4d83e35d887cdb0.png)

[336334739-d907795d-b5b9-48bf-b1db-70cf3f45d155.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Tqgjb7ZRSoJZEHx7QqscrlMen8d)

最后翻阅论文，发现我原先用于Agent决策的Qwen-VL-Max的能力连GPT-4V的一半都不到，于是就换了GPT-4o来跑Agent，最终复现出了论文中“牛逼轰轰”的效果。

总之，只有你复现出了论文的效果，才算是把代码跑成功了。

![](img/89abb26eb53d5021e0c2e13fadd7e3ec.png)

## 如何添加自己的tool

尽管这个Agent能模拟人工进行一些操作，但我还是建议，能让Agent调用API实现的，就不模拟人工操作的就不模拟人工操作。

这里，我给Agent提供了个tool，让Agent获取到弹幕内容后就调用这个tool来传递弹幕信息。

而这个tool就是自动插入飞书表格的tool，通过调用飞书的api，将消息传递到飞书表格上，这个飞书api的获取就略了，这里将下我是怎么让飞书和Mobile-Agent对接上的。

首先展示下我都修改了哪些文件

![](img/c1f019a9637a2bcdcdb17a5e95921b2d.png)

### 在prompt里教会Agent该怎么做

首先，我在prompt中声明了函数Insert ()，告诉模型一旦获取到弹幕，就调用这个函数插入我的数据库（飞书表格）

#### run.py

![](img/aa4a914dfb65c2ba551454beb7ee1abf.png)

#### prompt.py

但是在上面run.py上定义后，Agent依旧不会调用我给它的函数，于是我找到了另一个存prompt的地方，这个是prompt.py，这里声明了Agent各个函数的定义，只有将你自定义的函数声明在这才算数。

![](img/8e8741e0435b2c587859ed3adac2d142.png)

### 实现自己的自定义函数

#### controller.py

在这里添加你自己想添加的tool，这里的insert函数用于往飞书表格上插入

![](img/f4ad4c018da704eec1eb284362661644.png)

#### run.py

把刚才在 controller 添加的insert函数导进来

from MobileAgent.controller import get_screenshot, tap, slide, type, back, home, insert

![](img/91356917906fd33a904fa6f4f911ab4e.png)

上面这个是用来解析模型的输出的，就是先检测有没有Insert，有就解析输出然后调用函数来插入。

## 目前效果

![](img/8c85105c95153298748eba2be552f294.png)

![](img/5a6139833760eb916de04c9b63a19786.png)

[2024-09-02 22-15-03.mkv](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WBnXbit6CoGQjAxRMXzcMjrunmc)

## 我们当前的一些代码

https://github.com/ybai34/Mobile-Agent-Development

里面的api key可以试用，大概跑一小时花费1$

![](img/52d8fecc770707b689d9f64f95c55288.png)

## 分享我目前用的国内中转站

目前也只有GPT-4能完成这些复杂的操作，这个就建议用国内中转站完成了。

我目前用的中转站：https://api.chatllm.vip/

建议用国内中转站是有原因的……不过这个中转站在充钱之前建议先加客服🛰️，因为它在充钱方面经常出BUG

![](img/7953a95dbb4445bdd97c31ef9dcabb18.png)

# 3、一些Mobile-Agent框架的介绍

![](img/478b9ba2291fa01fdda4cb503895eefc.png)

我的知乎文章

https://zhuanlan.zhihu.com/p/716838305

# 4、分享自己的模拟器

雷电模拟器

https://www.ldmnq.com/

# 5、一些可以改进的点

## 图像的**Grounding**能力（感觉很有搞头）

其Grounding模型的论文：https://arxiv.org/abs/2303.05499

其他仓库链接

https://modelscope.cn/models/AI-ModelScope/GroundingDINO

https://github.com/IDEA-Research/GroundingDINO

### Mobile-Agent的**Grounding**能力是什么？

简单的说，就是安装文本描述，找到图片中对应内容的位置。

现在非常火热的VLM模型也具有一定的Grounding能力，但只能描述出目标在图像中大概得位置，不能准确的给出其xy坐标。

但如果想操作手机，就必须得有对应的xy坐标才能操作。

![](img/3790dda1a2bf6c094d46bb847228bd36.png)

而Mobile-Agent的gounding是用icon做提示词来定位的，作者希望其定位出的都是可以点击的按钮。

![](img/63f6de5bb3ceec3491aeed94e8094dea.png)

其在显示场景中的效果如下所示。

![](img/d0b89c9b3b36a45b79a59f2092ffccd0.png)

### 找到icon之后的流程

用Qwen-VL来描述**所有**的icon（耗时长），然后做选择，选出可能需要点击的那个

如果能利用好gounding模型的能力，可以加快Mobile-Agent的速度。

### 改进建议

默认用来定位的prompt是icon，这其实可以通过调整prompt搞出更多花样的，比如只识别上半部分的icon，识别能关闭广告的 x 的icon。

另外也可以试着自己打标签微调，如果数据够多，说不定能直接定位提交按钮、聊天框等地方。

## 文本区分的准确度/速度

一般而言，单纯OCR文本识别技术已经非常成熟了，其性能已经无限接近可商用的水平了，个人感觉折腾这块用处不大。难的是页面上的文本往往不是同一段的，怎么区分它们？

我让Mobile-Agent帮我获取弹幕内容，但其对发言人的判断被干扰了。这里我不确定是大模型没理解其B站弹幕格式，还是在ocr时就已经搞混了。

![](img/6b588f5c1642fad61a4bdc600376d26a.png)

其使用了2个模型，

![](img/af80a671dbc88ffc4caf74e24a1c4d93.png)

其链接见下

https://modelscope.cn/docs/cv_resnet18_ocr-detection

https://modelscope.cn/docs/convnextTiny_ocr-recognition

这2个模型一个是文本识别，一个是文本定位

ocr-detection是用来定位图片上的文字的，其输出格式见下。

![](img/c8922d574ebf1bb56f74b60f030e7d67.png)

而ocr-recognition是用来识别刚刚ocr-detection识别出的文本的。

另外这2个模型都不支持微调

![](img/7fd998f0be71cc9855f1c14aad11d18e.png)

### 改进建议

这个OCR是Two-stage的，即先定位，再检测，俺直觉上感觉应该会耗费比较长的时间，可以先实验看下Agent的每步操作都花了多少时间在这，然后在想办法改进。

而这个加速OCR过程的方法，敢折腾的可以参考下我之前看过的一篇工作（不确定效果会怎么样）

https://github.com/clovaai/donut

![](img/524742495d05f9fb88cf9d7cc39663b6.png)

这个Donut模型的特点就是端到端同时实现了文本定位（带bbox的那种）和文本识别，推理速度快。

![](img/fb9143c3438f3ce91ac66e0c524a8a3a.png)