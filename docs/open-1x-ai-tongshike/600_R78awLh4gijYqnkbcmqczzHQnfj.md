
# 教程：AI 人类模特换装

> 来源：[https://datawhaler.feishu.cn/docx/WYQJdN0xio4Bzmxielzcqgeindf](https://datawhaler.feishu.cn/docx/WYQJdN0xio4Bzmxielzcqgeindf)

> ✍️ 作者：[纪伦的个人说明书](https://datawhaler.feishu.cn/wiki/AKxvw88QMiyg6NkxtYzc9evGnfc?from=from_copylink)

## 学前须知

+ **学习时间**：1~10天

+ **学习难度**：可易可难，具体看需求

+ **学习方式**：

1. 针对不同能力基础和需求的用户，在场景细分图中，找到自己需要的场景
1. 找到对应的案例学习（参考小作业与参考教程）
1. 学习优秀打卡案例，查漏补缺
1. 选择感兴趣的小作业，跟着案例自己尝试做一遍。
1. 尝试熟悉流程后，应用到实际工作生活中

> 打卡核实后「可获得证书」，优秀打卡内容将被「展出」，特别优秀者会邀请来「一起完善学习文档」

> 具体打卡要求详见：[【说明文档】打卡（非必须）](https://datawhaler.feishu.cn/wiki/UR4bwTSjJiAKFLkfqdCc8Dlgnld?from=from_copylink)

## 场景细分图

人物与动物/静物换装整体流程类似，但人物换装有一些现成的模型实现了无需特别训练一键换装，相对更成熟和完善一些

## 参考小作业

多维表格（A5CLbUyGGaRQBRsZ9GMc0QWin3g_tblVSXErrLPr8NKD）

## 参考教程

### 【场景一】人物随机+服装固定

#### 【方案一】人台图+局部重绘

SD WebUI

[自己安装一个S](https://space.bilibili.com/12566101)D WebUI，需要电脑上有显卡，可以用。B站上秋葉aaaki做了方便小白入手的整合包，具体安装可以看看教程。https://www.bilibili.com/video/BV1iM4y1y7oA/

![](img/72e3e24c5195b73a95a5c7ff85da2406.png)

这里需要先给人台模特穿上成品服装，然后用局部重绘来把图片变成真人穿衣。（需要抽卡和精调）

具体操作方法参考如下：

https://www.bilibili.com/video/BV1E84y1R7kf/

https://www.uisdc.com/stable-diffusion-19

![](img/c94d76801316496d0e06c8c3c3b6accf.png)

无界AI人台图转模特工作流

![](img/fd7ce93edbf065949c8ab7fa1aa65469.png)

![](img/6385d06a30e3c1b10fbcad881d4490b4.png)

具体使用见链接：https://www.wujieai.net/workflow-plaza/1364091

#### 【方案二】训练服装类Lora

SD WebUI+Lora

[自己安装一个S](https://space.bilibili.com/12566101)D WebUI，需要电脑上有显卡，可以用。B站上秋葉aaaki做了方便小白入手的整合包，具体安装可以看看教程。https://www.bilibili.com/video/BV1iM4y1y7oA/

![](img/84c3ba906053807eeffa9bd840df79b0.png)

Lora训练，大体能还原，花纹细节等难还原，需要抽卡，能实现的效果参考

> 左为训练素材图，右为生成效果图

![](img/53a08a72a3030e044dd749201d2a9948.png)

![](img/d77008d14ab35a8d4c5225b6f54a1fd0.png)

> 左为训练素材图，右为生成效果图

![](img/cf0370d70d03d918c22f05630643c7bd.png)

![](img/c913278128ab255090cb594010dc07d0.png)

参考资料：

https://mp.weixin.qq.com/s/8w8sqjB3aWNJPsgjKGdtkA

https://www.bilibili.com/video/BV1914y1Q7CE/

训练需要注意：图片清晰度较高，服装最好有上身图，各种角度的为好

### 【场景二】人物固定+服装固定

#### 【方案一】训练服装Lora+训练人物Lora -> Lora融合

训练服装Lora+训练人物Lora ->Multi-LoRA-Composition融合方案

训练Lora参考前面的内容。

Lora融合，训模特的Lora和衣服的Lora，需要训练与实验，目前效果未必稳定，但是只需要两个Lora融合，成功概率会比多Lora高一点。需要每种狗和每种衣服都训练Lora模型，需要较多的标注数据

融合Lora参考方案Multi-LoRA-Composition(最近开源，案例效果好于一般的Lora融合，但比较新，没有多少第三方的效果尝试，如有尝试效果欢迎投稿补充)：

https://github.com/maszhongming/Multi-LoRA-Composition

![](img/ebc093c34f45061e745bd73df2b49b54.png)

训练服装Lora+训练人物Lora ->Mu普通的Lora融合方案

训练Lora见参考资料。

具体融合插件可见：

https://www.zhihu.com/zvideo/1637944442459037696

#### 【方案二】专门的换装模型，无需训练，一键换装

OOTDiffusion

目前此方案开源，且可免费使用demo(人多需排队，且使用需🪜)

https://huggingface.co/spaces/levihsu/OOTDiffusion

开源仓库：https://github.com/levihsu/OOTDiffusion

无需训练，一张清晰模特全身照+一张服装平面图可实现：全身换装，只换上装，换上装+下装

比如这种看起来效果还不错

![](img/f111152f132cc868d6edb2485b2087a7.png)

Emmm..但有时也会生成这种

![](img/f0743f1d1b2926a1b22a722dbe07fe29.png)

Anydoor

目前此方案开源，且可免费使用demo(人多需排队，且使用需🪜)，但不是专门针对换装训练的，可能稳定性与效果没有OOTDiffusion效果稳定

免费使用链接（需要🪜）：https://huggingface.co/spaces/xichenhku/AnyDoor-online

代码开源仓库：https://github.com/ali-vilab/AnyDoor

![](img/04cf9ba2680f00de33031abcce3f7bd5.png)

#### 【方案三】PS叠加人+服装->图生图

Midjourney图生图

利用P图叠加+midjourney图生图功能

也不能做到很稳定，需要不断抽卡选到合适的图

参考1：https://www.puxiang.com/articles/f3220499744a4de7009c13ab96eb49fe

![](img/12b355d94c19ae2d10401de52dbeec56.png)

![](img/bed9e9f344e76ac277299f64c73665e2.png)

参考2：https://zhuanlan.zhihu.com/p/630535647

![](img/d263b99b0ec863ff68b70422430f4244.png)

![](img/f00352c2c7f9e4c9fa813c740da2da49.png)

### 【场景三】人物固定+服装元素固定

一般情况下，定制服装为了避免天马行空，随机出一些做不出来的东西，可能会希望服装元素稍微固定一些。目前效果不确定，如有更好的方案，欢迎联系或评论。

#### 【方案一】训练元素Lora+训练人物Lora -> 多Lora融合

训练Lora参考前面的内容。

Lora融合，训练人的Lora和元素的Lora，需要训练与实验，目前效果未必稳定，在多元素的情况下，一般Lora融合效果可能不够好。

融合Lora参考方案Multi-LoRA-Composition(最近开源，论文提到>2的多Lora的融合效果好于一般的Lora融合，但比较新，没有多少第三方的效果尝试，如有尝试效果欢迎投稿补充)：

https://github.com/maszhongming/Multi-LoRA-Composition

![](img/caf995032cb4743384e5303cdb0b4cc7.png)

### 【场景四】人物固定+服装随机

比如想要给人物拍天马行空的虚拟创意写真

#### 【方案一】利用人物/角色一致性，实现换装

Midjourney

但目前对于Midjourney生成出的人物照片保持一致性效果较好，自己的真人照片还原度略低一点

![](img/ab46845b5abebe6278e32fa1ac57a029.png)

参考 Midjourney 重大更新！深度解析「角色一致性」命令https://www.uisdc.com/midjourney-49

InstanceID

开源免费，人物还原度高，但有时候需要排队。

InstantID 使用链接：https://www.modelscope.cn/studios/instantx/InstantID/summary

![](img/095e909a40961c52945a6abbac6d4738.png)

参考 用InstantID来做一致的人物角色 https://www.jiqizhixin.com/articles/2024-01-22

SD WebUI

安装SD WebUI见参考资料

【SD】一致性角色 - 同一人物 不同姿势 https://blog.csdn.net/m0_73883919/article/details/135314693

Dalle

类似midjourney，目前对Dalle生成的照片保持角色一致性较好，自己的照片不行。

用`Seed`值让Dall·E画出一致的人物！(有GPT4的看这里) https://zhuanlan.zhihu.com/p/675446051

#### 【方案二】局部重绘

类似场景一方案一，只不过服装与人的关系交换

#### 【方案三】训练人物Lora+`prompt`

参考前面的Lora训练

## AI工具表

> 持续添加中，欢迎大家在文档处评论分享一些自己使用觉得不错的工具。

## 参考资料

> 公域上一些不错的，对大家学习用AI处理数据，可能有帮助的内容，也欢迎大家评论补充

+ Stable Duffusion WebUI安装

[自己安装一个S](https://space.bilibili.com/12566101)D WebUI，需要电脑上有显卡，可以用。B站上秋葉aaaki做了方便小白入手的整合包，具体安装可以看看教程。https://www.bilibili.com/video/BV1iM4y1y7oA/

![](img/0cfb55fd60e72b4ecb55733774fb5806.png)

+ Stable Duffusion WebUI使用

SD使用系列学习视频：https://www.bilibili.com/video/BV1e7421K7HF/

![](img/14c9015322a43fc829507da760590da3.png)

+ Lora训练打标技巧

自己训练的需要注意参数配置和提前给训练图片打标签，方法如下：

需要注意一些打标签的方式，6min小视频，可以简单看看了解一下

![](img/c61cae86dc9f5eb4ce79dfdeea6ed125.png)

[【LoRA训练课】标签怎么打？触发词？优化？为什么？模型训练炼丹新手入门喂饭级教程.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QLuHbxOBZo1RT6xtNujc2ayhnzd)

来源：https://www.youtube.com/watch?v=nqWZAgCZD5s

+ 对于Lora训练的技巧

LoRA训练和使用进阶：让LoRA实现真人幻化式换装，AI美少女养成(包含脸部、服装、Reg——更新Controlnet解决脸崩、直出高清图) https://nga.178.com/read.php?tid=35774713&rand=141

LORA模型训练-探秘数据集整理 https://blog.csdn.net/umbato/article/details/135567026

## 优秀学习案例

> 非常值得一看的内容！如果你也觉得不错，记得给他们点个赞。
