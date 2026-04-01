# 教程：AI 小动物换装

> 来源：[https://datawhaler.feishu.cn/docx/KI8Dd0gUDo7rjExy8BGcuEGun4g](https://datawhaler.feishu.cn/docx/KI8Dd0gUDo7rjExy8BGcuEGun4g)

> ✍️  作者：[纪伦的个人说明书](https://datawhaler.feishu.cn/wiki/AKxvw88QMiyg6NkxtYzc9evGnfc?from=from_copylink)

## 学前须知

+   **学习时间**：3~10天

+   **学习难度**：可易可难，具体看需求

+   **学习方式**：

1.  针对不同能力基础和需求的用户，在场景细分图中，找到自己需要的场景

1.  找到对应的案例学习（参考小作业与参考教程）

1.  学习优秀打卡案例，查漏补缺

1.  选择感兴趣的小作业，跟着案例自己尝试做一遍。

1.  尝试熟悉流程后，应用到实际工作生活中

>

> 打卡核实后**「可获得证书」**，优秀打卡内容将被**「展出」**，特别优秀者会邀请来**「一起完善学习文档」**

> 具体打卡要求详见：[【说明文档】打卡（非必须）](https://datawhaler.feishu.cn/wiki/UR4bwTSjJiAKFLkfqdCc8Dlgnld?from=from_copylink)

## 场景细分图

人物模特目前换装技术相较成熟（有一些不错的模型），小动物目前稍微欠缺点，大多流程与人类模特换装类似，少数不同。

## 参考小作业

多维表格（CQt0bl3FzaLJNjshjmqcYl58n9f_tbliJjWLCFoWTAAN）

## 参考教程

### 【场景一】动物随机+服装固定

#### 【方案一】局部重绘

**SD WebUI**

[自己安装一个S](https://space.bilibili.com/12566101)D WebUI，需要电脑上有显卡，可以用。B站上秋葉aaaki做了方便小白入手的整合包，具体安装可以看看教程。https://www.bilibili.com/video/BV1iM4y1y7oA/

![](img/cf1c0017c261596b4495653d96f5cd13.png)

参考人类模特换装对应方案

无界AI宠物狗台图转狗模特工作流

玩具狗模特->图生图->真狗试穿效果

（需要准备和狗狗体型相似的玩具狗进行尝试，大体转出来相似，但是一些细节可能会有问题）

![](img/8e8ea93799e1a70ecae79977e4f21391.png)

![](img/d477a7fd5af4897ad88ab1b967113eff.png)

这个难度不高，可以自己尝试，无界AI有相关工作流，试用地址：https://www.wujieai.net/workflow-plaza/1474183

#### 【方案二】训练服装类Lora

**SD WebUI+Lora**

[自己安装一个S](https://space.bilibili.com/12566101)D WebUI，需要电脑上有显卡，可以用。B站上秋葉aaaki做了方便小白入手的整合包，具体安装可以看看教程。https://www.bilibili.com/video/BV1iM4y1y7oA/

![](img/91ab00e981cba2e9c7e8de1267fedbec.png)

具体参考人类模特换装对应方案

#### 【方案三】垫图+prompt

> 这里用midjourney测试了一下，还原度不是很好，不建议使用。

单纯一张狗穿衣服的参考图+prompt达到换狗的效果。狗换的挺好，但衣服还原度不算高，需要抽卡(https://s.mj.run/P276L5GNrsw A corgi dog wearing a blue shirt and white headphones)

![](img/8a050e54bdbc9cca2e56ea67ecfc9fcc.png)

![](img/0b2ef8e3bcd9af9a2ea848de31a25ccb.png)

![](img/5aadad19de76eee54d8fc32a71c44832.png)

### 【场景二】人物固定+服装固定

#### 【方案一】训练服装Lora+训练动物Lora -> Lora融合

**训练服装**Lora+训练动物Lora ->Multi-LoRA-Composition融合方案

参考人类模特换装对应方案

#### 【方案二】PS叠加动物+服装->图生图

**Midjourney图生图**

利用P图叠加+midjourney图生图功能，参考人类模特换装对应方案

### 【场景三】动物固定+服装元素固定

一般情况下，定制服装为了避免天马行空，随机出一些做不出来的东西，可能会希望服装元素稍微固定一些。目前效果不确定，如有更好的方案，欢迎联系或评论。

#### 【方案一】训练元素Lora+训练动物Lora -> 多Lora融合

参考人类模特对应部分内容

### 【场景四】动物固定+服装随机

比如想要给小宠物拍天马行空的虚拟创意写真

#### 【方案一】利用角色一致性，实现换装

**Midjourney**

细节百分之百还原有难度，相似性还没那么高。但是大体差的不多。

动物主体保持一致性参考 Midjourney 重大更新！深度解析「角色一致性」命令https://www.uisdc.com/midjourney-49

服装和背景就用prompt随便描绘即可

比如，给下面的狗狗换个服装，主要是通过prompt控制背景和狗的颜色，--cref和--cw控制狗的形象连贯

> a white dog wears blue denim jacket. a solid blue color background, with professional photography and studio lighting, in a hyper realistic and detailed style. --cref https://s.mj.run/i2PZTeekSe8 --cw 0

![](img/6089e24eaf196861450ce11fdd337c80.png)

![](img/0ec4373a4f596b5ad5d053a477bfff93.png)

#### 【方案二】局部重绘

参考人类模特对应场景

#### 【方案三】训练人物Lora+prompt

参考人类模特对应场景

## AI工具表

> 持续添加中，欢迎大家在文档处评论分享一些自己使用觉得不错的工具。

## 参考资料

> 公域上一些不错的，对大家学习用AI处理数据，可能有帮助的内容，也欢迎大家评论补充

+   Stable Duffusion WebUI安装

[自己安装一个S](https://space.bilibili.com/12566101)D WebUI，需要电脑上有显卡，可以用。B站上秋葉aaaki做了方便小白入手的整合包，具体安装可以看看教程。https://www.bilibili.com/video/BV1iM4y1y7oA/

![](img/0dd5771c3c21986b519789bff5f405fe.png)

+   Stable Duffusion WebUI使用

SD使用系列学习视频：https://www.bilibili.com/video/BV1e7421K7HF/

![](img/d1d15b451a05191070024089ee6717c1.png)

+   Lora训练打标技巧

自己训练的需要注意参数配置和提前给训练图片打标签，方法如下：

需要注意一些打标签的方式，6min小视频，可以简单看看了解一下

![](img/a0485f2a7c999e2c0d7e1614886d4131.png)

[【LoRA训练课】标签怎么打？触发词？优化？为什么？模型训练炼丹新手入门喂饭级教程.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FFAAbzcdioH7Irxx7FDcteVLnPg)

来源：https://www.youtube.com/watch?v=nqWZAgCZD5s

+   对于Lora训练的技巧

LoRA训练和使用进阶：让LoRA实现真人幻化式换装，AI美少女养成(包含脸部、服装、Reg——更新Controlnet解决脸崩、直出高清图) https://nga.178.com/read.php?tid=35774713&rand=141

LORA模型训练-探秘数据集整理 https://blog.csdn.net/umbato/article/details/135567026

## 优秀学习案例

> 非常值得一看的内容！如果你也觉得不错，记得给他们点个赞。