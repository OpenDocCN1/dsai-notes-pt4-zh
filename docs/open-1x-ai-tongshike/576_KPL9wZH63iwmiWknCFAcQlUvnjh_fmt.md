
# 教程：AI小电影制作

> 来源：[https://datawhaler.feishu.cn/docx/D6r8dOXKdod2lWxrHjzclY8fnch](https://datawhaler.feishu.cn/docx/D6r8dOXKdod2lWxrHjzclY8fnch)

> ✍️  作者：[斯阳明的个人说明书](https://datawhaler.feishu.cn/wiki/BOWZwAjXdib52SkZLOEcZbwAnog)

AI小电影制作的流程就是AI聊天机器人、AI生图和AI生视频三类工具的组合。

当前的AI生图工具按照输入内容可以分为【文生图】和【图生图】两大类。以后可能还会出现【音频生图】工具。

当前的AI生视频工具按照输入内容可以分为【文生视频】、【图生视频】和【视频生视频】三大类。以后可能还会出现【音频生视频】工具。

此外，为了使制作出的视频效果更好，还会使用AI生音乐（配音、音效）、AI配音等工具。最终的视频编辑可以使用PR、剪映等实现。

## AI小电影创作的通用流程

**剧本创作：**

+ 撰写小电影剧本

+ 分镜场景的中英文描述词`prompt`：AI划分之后，人为进一步调整。

+ 分镜场景的字幕和旁白

**素材准备：**

+ 【文字】到【图片】素材到【视频】素材

+ 【文字】到【视频】素材

**后期处理：**

+ 使用剪映创建脚本或草稿

+ 为视频添加AI配音

+ （可选）为视频添加音乐或者特效，转场等。

输出最终视频，发布到各个平台。

### 剧本创作

AI小电影的剧本/剧本创作，可以使用大语言模型创作，并且还有基于大模型专门为写作优化的智能体。

使用示例：[视频文案创作示例](https://pyt1f0fi57.feishu.cn/docx/So3zdz1MfocQtxxSqLcc16U7nMe)

可以使用的剧本创作工具：

+ [文心一言](https://yiyan.baidu.com/)

+ [讯飞星火](https://xinghuo.xfyun.cn/)

+ [智谱清言](https://chatglm.cn)

+ [chatGPT](https://chat.openai.com)

#### 将剧本拆分为具体的场景分镜

[视频文案创作示例](https://pyt1f0fi57.feishu.cn/docx/So3zdz1MfocQtxxSqLcc16U7nMe)用之前的语言大模型将场景拆分为分镜描述，这里以 中chatGPT的输出结果为例，其他工具的使用是相通的。

> **你：**

> 帮我把这些转化为具体的动画场景描述，把每一个需要展示的画面都描绘出来，包括转场，每个场景只有3秒左右，就需要切换到下一个画面，所以需要尽可能的将画面拆的非常细。

> **ChatGPT**：

> 场景1：艾米莉亚在实验室里翻阅着古老的笔记本，她眉头微皱，目光专注。

> 转场：白光闪过。

> 场景2：镜头聚焦在艾米莉亚手中的笔记本上，一行行古老的文字清晰可见。

> 转场：白光再次闪过。

> 场景3：实验室中，艾米莉亚手持着工具，正在认真制作时间之门的雏形，周围是各种电线和仪器。

> ......

> 故事结束。

#### 将剧本转换为场景描述

将分镜场景转换为视频素材有两种方式：

注意事项：目前很多工具可能对中文提示词的支持不完善，因此可以借助AI转换为英文描述。

操作示例如下：

> **你：**

> 帮我把上面的场景描述，转化为英文的`prompt`

> **ChatGPT**：

> Scene 1: Emilya flipping through an ancient notebook in the laboratory, her brows furrowed, her gaze focused.

> Transition: A flash of white light.

> Scene 2: Close-up of the ancient text in Emilya's hands, the lines of old writing clear and legible.

> Transition: Another flash of white light.

> Scene 3: In the laboratory, Emilya holding tools, earnestly crafting the prototype of the Time Gate, surrounded by various wires and apparatus.

> .......

> End of story.

#### 生成场景分镜的字幕和旁白

> **你：**

> 结合上面的故事和场景，帮我给每个场是写一段旁白和字幕，估计3到4秒内能读完，请使用中文回答

> **ChatGPT**：

> 场景1：

> 旁白：在一间朴素的实验室里，一个充满好奇心的头脑沉浸在一本古老的笔记本中。

> 字幕：探索奥秘

> 场景2：

> 旁白：古老的文字跃然纸上，引发了好奇心和惊叹。

> 字幕：解开谜团

> 场景3：

> 旁白：怀着决心和创造力，年轻的科学家开始了她的创作。

> 字幕：打造时间之门

> ......

> 字幕：明日编年史

### 素材准备：从提示词到视频素材（🌟🌟🌟🌟🌟）

将分镜场景转换为视频素材有两种方式：

+ 第一种方式：首先通过【文生图】、【图生图】工具生成场景分镜的概念图，接着使用【图生视频】生成每个场景的视频素材。

+ 第二种方式：通过【文生视频】工具一步生成每个场景的视频素材。

这里主要按照AI视频生成工具需要的输入内容对工具进行区分，以方便大家能更有效率选择目标工具。

可以使用之前的AI工具将场景描述转化为制定工具的关键词描述，这里以【文生图】主流工具Midjourney为例，将之前的场景1描述转换为Midjourney提示词：

> **你：**

> 帮我把以下的场景描述，转化为Midjourney提示词：Scene 1: Emilya flipping through an ancient notebook in the laboratory, her brows furrowed, her gaze focused.

> **ChatGPT**：

> Midjourney提示词：

> Laboratory, ancient notebook, Emilya, furrowed brows, focused gaze

AI图片工具的参数比较多，能调整的部分也很多，思路是使用文字或图片作为输入，尽可以多的抽卡，找到最接近构建场景描述的图片，然后使用【图生图】不断迭代到自己满意的图片。

1. Midjourney参考资料：。

1. 官方文档：https://docs.midjourney.com/v1/en

1. 快速开始 | Midjourney官方文档-中文版

1. 这10+Midjourney提示词生成器，助你轻松拿捏AI绘画

1. 爆肝整理！超全500+Midjourney超全提示词合集！

1. Stable Diffusion：

1. 可以直接下载使用国内大神整理好的SD工具：https://www.bilibili.com/read/cv31254871/

**小技巧**：转换好的提示词`Prompt`可以应用于所有的AI工具的提示词部分，如果使用【文生视频】和【视频生视频】工具，则直接使用未转换前的场景描述词。

目前大部分的视频生成工具只能生成2s和4s的视频。AI视频生成工具的使用较为简单，只要输入想要的场景描述、图片或者概念视频即可生成，然后可以基于生成的视频（接近自己想象的场景），近一步迭代，最终生成自己需要的场景视频。

| 工具 | 功能 | 说明 |

| --- | --- | --- |

| [Midjourney](https://www.midjourney.com/home) | 【文生图】 【图生图】 | 当前主流工具 |

| [无界AI](https://www.wujieai.com/) | 【文生图】 【图生图】 【图生视频】 【文生视频】 【视频生视频】 | 国内的绘图工具，功能很丰富，使用简单，支持中英文提示词，免费30分钟，可以生成Midjourney风格的图片。 |

| 智谱清言的[AI画图](https://chatglm.cn/main/gdetail/65a232c082ff90a2ad2f15e2) | 【文生图】 | 可以用于儿童话本类小电影的制作。 |

| Stable Diffusion | 【文生图】 【图生图】 【文生视频】【图生视频】 | [功能强大，免费使用，可](https://github.com/guoyww/AnimateDiff)以自己本地部署。生成视频插件`AnimateDiff`。 |

| [runway](https://app.runwayml.com/) | 【文生图】 【图生图】 【图生视频】 【文生视频】 【视频生视频】 | 功能强大，使用简单。 |

| [PiKa](https://pika.art/home) | 【图生视频】 【文生视频】 【视频生视频】 【视频生音频】 | 特色：Sound effects工具为视频加上对应的音效 |

### 视频剪辑

将视频素材编辑为完整电影，推荐使用剪映工具（https://www.capcut.cn/）进行视频剪辑。

![](img/994078b35b65528cb57b47f67cf9c6da.png)

创作视频：直接编辑视频，随时导入需要的素材。

![](img/5bb77356554d6289a5f62acda88a4a97.png)

创建脚本：一次性按顺序导入素材，字幕和旁白，依次生成视频草稿。

![](img/ffa908c5736319f36375c29adc7b352b.png)

## AI电影的制作案例

[《逆转时光》制作流程](https://qixiangxingqiu.feishu.cn/wiki/EkhlwcPVpiX2NxkupH8cWHjRnwC)

## 网上AI小电影的优秀作品

首部AI全流程微短剧《中国神话》：https://www.bilibili.com/video/BV1gJ4m1j7BL/

[2024开篇王炸！AI还原震撼版《西游记》，](https://www.bilibili.com/video/BV1DS421N7Ph/)一个字，绝！_哔哩哔哩_bilibili

一位高产优质内容的pika导演：https://twitter.com/MatanCohenGrumi

AI春晚纯享版 ：https://space.bilibili.com/259768893/channel/collectiondetail?sid=2204653

用AI制作大闹天宫预告片：https://www.xiaohongshu.com/explore/65a23d88000000001000e200

阿里EMO，图片生视频：https://mp.weixin.qq.com/s/4KGYu-B4Te5FvPfDI804mg

AI眼中的"千山鸟飞绝，万径人踪灭"~：https://www.xiaohongshu.com/explore/659416be000000001a02ac00

CCTV6电影频道AI影像人才优选计划，做了一部AI短片，《玉覆荆楚》：https://mp.weixin.qq.com/s/wukpUyCpGSQue3MTUD91Xg
