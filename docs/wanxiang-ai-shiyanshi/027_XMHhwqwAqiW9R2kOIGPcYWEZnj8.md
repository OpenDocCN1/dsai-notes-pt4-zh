# AIGC 学习手册

> 来源：[https://yunyinghui.feishu.cn/docx/H2Epdx2HTogxV4x8CZ2cIUacnTd](https://yunyinghui.feishu.cn/docx/H2Epdx2HTogxV4x8CZ2cIUacnTd)

> **适用人群：****互联网关注 AIGC 发展人群**

> 首发时间：2023 年 3 月 16 日

> 更新时间：2024 年 3 月 18 日

> **内容出品人：**** **

> **内容出品方：**[玩赚 AI 实验室](https://yunyinghui.feishu.cn/wiki/WhelwZnxQi55mpkOzatc3Xran5b?from=from_copylink)

> **持续更新 AIGC 的****相关资讯动态****，研究报告，赚钱案例**，实用场景等 10 个板块内容，永久免费在线查看，欢迎收藏转发支持，文末有惊喜，大国出品，必属精品。

> **使用建议：**如果需要快速定位到精确内容，可以使用快捷键 Ctrl +F/Command +F 的形式，搜索关键字/词，查找你想要的内容。

>

> **内容更新日志：**[内容更新日志](https://yunyinghui.feishu.cn/docx/DIcTdHjcco4Gb4xiEt9cNNjtnLe)** **

# 前言

[AIGC 学习手册](https://nujuo8y1qx.feishu.cn/docx/RleadAwrAo4O1FxhuoGcDV86nBe?from=from_copylink)手册为知识库备份文档，原手册为： ，已经停止更新。原文档学习人数 8 万，阅读人次 14 万。

# 概述：AIGC 知识科普

## 基本概念

**AIGC 即 ****AI** Generated Content，是指利用人工智能技术来生成内容，AIGC 也被认为是继 UGC、PGC 之后的新型内容生产方式，AI 绘画、AI 写作等都属于 AIGC 的分支。

进一步理解时，AIGC 有狭义和广义概念之分。

狭义的 AIGC 与普通用户更为贴近，更关注图像、文本、音频、视频等内容生成，和 Generative AI（生成式 AI） 、Synthetic media（合成式媒体）等概念类似。

论广义概念，还包含策略生成（如 Game AI 中游戏策略生成）、代码生成（GitHub Copilot）、蛋白质结构生成等。

![](img/f96598bb20d5f70bd401d0880c6a6dab.png)

## AIGC 发展历程

![](img/64acac1e611156ccc70a723fb5ebe8e1.png)

## AIGC 产业发展阶段

**目前，我**国还处于 AIGC 发展的初期阶段，竞争趋势不明显，需要调整开发、资金等投入，寻求整体生态的快速搭建。至 2030 年，AIGC 产业发展共分三个阶段：

**1、培育摸索期**（2023-2025）：整体均处于业务场景验证和变现探索期。

底层大模型发展加速，中间层尚未出现相关玩家，基于 Stable Diffusion 等开源模型的上层应用迅速出现，但受底层大模型接口限制，大部分技术尚未达到稳定进入实际生产环节的水平。

**2、应用蓬勃期**（2025-2027）：基本价值创作路径和技术思路得到确认。

行业普遍尝试应用人机共创，且内容资讯、娱乐传媒等领域利用 AIGC 产生确定性价值。这一时期，底层大模型和中间层模型主要玩家基本确定，开放 API 增加，整体入局玩家增多，尤其是大量应用层玩家。

**3、整体加速期**（2028-2030）：产业生态完善。

报告指出，2028 年往后，AIGC 在个性化、实时化、自主迭代等方向上的延展价值得到充分发挥，和其他业务系统进行紧密连接，会有相关初创公司产生完整解决方案。这一阶段 AIGC 成为内容领域基础设施，预计会催生出完全不同的新业态。

![](img/6884abe23285f1f750e7a017d1b1f544.png)

图源：公众号量子位

## AIGC 商业化的 5 种营收模式

全球 AIGC 赛道持续走热，如何跑通商业模式产生实际营收，成为市场另一个关注焦点，目前来看一共有 5 种不同的营收模式：

**1、MaaS**( Model as Service)

适用于底层大模型和中间层进行变现，按照数据请求量和实际计算量计算。

到 2027 年，MaaS 模式占市场规模比例将从 5%增长至 47%。

**2、按产出内容量收费**

适用于应用层变现，如按图片张数、请求计算量、模型训练次数等收费。到 2027 年，该模式市场规模占比将从 60%下降至 32%。

该模式的关键在于如何从单次好奇驱动的行为切入，保证产品长期的复购率。

整个过程会受到具体属性影响，如版权授予（支持短期使用权、长期使用权、排他性使用权和所有权多种合作模式，拥有设计图案的版权）、是否支持商业用途（个人用途、企业使用、品牌使用等）、透明框架和分辨率等。

**3、软件订阅付费**

第三种模式即 ChatGPT Plus 现有的盈利方式之一：每月向用户收取 20 美元费用。该模式在现有市场占有 10%左右的比例。按照这种模式，Jasper 等初创企业已经产生了营收。展开来说，Jasper 以类 SaaS 服务的形式进行收费，分为初级、高级和订制三个模式，成立当年营收额就达到 4500 万美元，并收获了 7 万名用户。

这意味着 AI 正以越来越显性的方式产生商业模式。国内部分领先的 AIGC 公司，在用户规模、内容生成量上在 2022 年亦快速起步，2023 年开始产生营收及盈利并非难事，但能否形成 SaaS 订阅模式尚待观察。

**4、模型训练费用**

即最为传统的项目开发制度，如今占据市场营收份额的 25%，到 2027 年，预计将有 12%左右的下滑。

**5、其他模式**

包括广告/流量模式，依靠产品获取用户点击，从中获得广告流量，这种营收模式的关键在于产品如何获得复购。

由于属于小型项目，在市场规模测算中暂不单独考虑。

![](img/afac68677626e4209e82995f606eea5c.png)

图源：公众号量子位

## AIGC 对不同行业的变革影响

首先是**线上游戏行业**，它目前在 AIGC 领域中算相对最成熟、接受度最高的部分。

其应用从前期制作到后期运营以及游戏周边内容制作的全流程中均有涉及。其中的核心生产要素为：AI Bot（游戏操作策略生成）、NPC 相关生成（包括逻辑和剧情）和相关资产生成（包括地图等）。

报告认为，这一行业是**目前 AIGC 变现最为清晰**的行业。

接下来是**影视传媒**，由于它和内容挂钩更为直接，AIGC 对这一行业的整体影响更为明显。

具体受影响的细分领域包括电影及长视频（换脸、背景渲染、广告自动植入等）、网络直播（虚拟人）、短视频（影视作品剪辑）、在线音乐（自动编曲、作曲、AI 唱歌）、图片版权（AI 生图、AI 修图）、网络文学（小说续写）等。

**内容资讯行业**则不必多言，主要是在新闻写作，目前落地应用也已较成熟。

**电子商务行业**也会受到影响，主要来自数字人直播带货及个性化营销这两方面。

**办公软件行业**同样受到冲击。最具代表性的当属微软 Office 全家桶的 Copilot 功能了，它被誉为一项“重新发明生产力”的重磅更新。

除此之外，相关独角兽企业如 Notion AI 也表现出色，截止 2022 年，该公司用户已达到 3000 万。

最后会引起变革的行业还包括训练数据（主要集中在自动驾驶和医疗行业）、社交软件（比如用特定性格的 AI NPC 设定满足线上社交功能）和在线教育等。

![](img/54f080c78e79c0d851a687e16a593fd0.png)

图源：公众号量子位

# 一、AI 绘画

## 绘画工具：Midjourney

![](img/27ad9b878fe4e912ab65a320aa783609.png)

### 简介

Midjourney 是一个由同名研究实验室开发的人工智能程式，可根据文本生成图像，于 2022 年 7 月 12 日进入公开测试阶段，使用者可透过 Discord 的机器人指令进行操作，该研究实验室由 Leap Motion 的创办人大卫·霍尔兹负责领导。

### **官方网站**

**传送门：**https://www.midjourney.com/home

### MJ 中国（目前和腾讯合作）

MJ 官方 QQ 频道，QQ 扫码可以加，如果过期，可以自行去 QQ 频道搜索加入，目前已经满人了，何时开放等通知。

![](img/60fde933139b0108199bc993ad6b1b96.png)

### 用途和使用场景

**主要用途：**可用于壁纸、插画、漫画、平面设计、logo、APP 图标等的灵感创作。

**适合人群：****设计师 插画师** 美术指导 创意总监 自媒体人 创意行业 视觉行业 一切内容创作者 AI 爱好者 行业投资人

可以延伸职业：AI 图像生成师/ AI 视觉创意指导 /AI 艺术家

### 注册方法

![](img/daab07f7b65caf45889993ad495aa4f6.png)

[注册Midjourney账号SOP.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Z9h0bz0LAoyL6RxFxPmcfOREn6c)

### 使用手册

[Midjourney 官方指导](https://docs.midjourney.com/)

### 学习教程

[【Midjourney】从入门到精](https://space.bilibili.com/630876766/channel/collectiondetail?sid=1045607)通·全网最通俗易懂·AI 绘画保姆级教程

AI 绘画神器 Midjourney 基础使用手册

AI 绘画神器 Midjourney 进阶使用手册

**AI 绘画完整版演示教程**

![](img/2415baddcc85506185f7f45493a891c0.png)

[mmexport1680137250522.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OIPAb9CoxoWpkCx7Ii8cjjjKnrb)

### **MJ 关键词提示网站**

如果你一开始不知道输入什么关键词才好，我建议你可以直接到 Midjourney 官网的大佬专区（ https://www.midjourney.com/showcase ）参考大佬的关键词，或者在其他房间参考别人的关键词，也可以利用谷歌翻译把自己想说的按照下面的公式翻译后出图，关键词尽量具体，AI 出图也会更加具体。

1.通过输入文字自动转换成 prompt，提供艺术风格，颜色，灯光等：https://prompt.noonshot.com/midjourney

2.输入图片帮助提取出关键词：https://huggingface.co/spaces/doevent/prompt-generator

3.通过输入关键词生成 prompt，页面比较简单：https://midjourney-prompt-helper.netlify.app/

4.卡通位置的 prompt：https://publicprompts.art/

5.种类较齐全的 prompt：https://openart.ai/

6.魔咒百科词典：https://aitag.top/

![](img/78de8c25afee32be45084f7a83581ba0.png)

[Midjourney技巧.xlsx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GX9qbdULxoQrbRxCXGKcmS2unrd)

![](img/2381dc67ebe0d543d0d80fb755e0f161.png)

**视频教程**

![](img/0b4aa12cf2491659b449d6cec9fd8d6a.png)

[mmexport1680137480619.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ODZZb01zkoh2FAxOL8McEa9enof)

### MJ 自学资源大全

+   查看官方使用指南，熟悉指令和功能：https://docs.midjourney.com/

+   [提升创意灵感，可以查看官方每日佳展示区：htt](https://www.midjourney.com/showcase/recent/)ps://www.midjourney.com/showcase/recent/

+   管理自己的创作文件夹：https://discord.com/channels/@me/

+   探索官方社区，最新活动资讯：[https://www.midjourney.](https://www.midjourney.com/showcase/recent/)com/showcase/recent/

+   懒人资源包：[https://sites.google.com/view/mid](https://sites.google.com/view/midjourney-resource-tips/%E9%A6%96%E9%A0%81)journey-resource-tips/%E9%A6%96%E9%A0%81

+

![](img/67f9f0383b1a5f878c2132d001bd4261.png)

[基于Midjourney的提示词启蒙全书  作者：Aitu Ma.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TyQcbUzP9oeI6yx7d87c8WKfnxc)

### Midjourney 无限免费出图的方法

![](img/45ec2322f2713c37b3083a23a497710a.png)

[Midjourney无限免费出图的方法.MOV](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Wq2hbx9Q9oAWbbxhFENcDXHinhb)

### 实战案例展示

1.  2 天 300+作品，Midjourney 将彻底颠覆食品包装设计！

1.  AI 绘画生成模特穿衣图片案例教程

![](img/6a94f97f72139bcdd1f9ff7161e85485.png)

[mmexport1680137340605.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TlI6bAhzyoCmp0xcPszcHgLVnPc)

**3.卡通 IP**

![](img/7bfb9cff9c4012f04567bc9ea34eeb65.png)

**核心词：**

Pixar（皮克斯艺术）

POPMART blind box（泡泡马特盲盒）

**关键技巧：**

1.  主体物：可以换成男孩，Cute girl （可爱的女孩）；艺术风格：Pixar（皮克斯风格）是核心词。配合 3D rander 使用；

1.  补充单词：背景：warm color background（暖色背景）。

1.  版本参数：V5

**完整描述：**

Cute girl with bangs short hair, blue dress, white apron, holds blender, pink headband, big eyes, frontal view of the character, waving action, brown color palette, double layer cake, biscuits, warm color background, Pixar, 3d rendering

**4.剪纸风格**

![](img/dfc68925006354917371ff4de52bc374.png)

**核心词：**

paper art（剪纸艺术）

**关键技巧：**

1.  主体物：可以换成任意主角，Chinese illustration （中国风插画）；艺术风格：paper art （剪纸艺术）是核心词，可以延伸词组为：（multi dimensional paper quilling）多维剪纸，配合构图形式使用；

1.  版本参数：v4，不要用新版本的 v 5，效果没有 v 4 好；

1.  补充单词：构图：elevationperspective（立面透视图）；色彩：morandi color（莫兰迪色）；灯光设置：warm light（暖光）。

**完整描述：**

Chinese illustration on blue background,auspicious clouds, 3d, ravine stream,mountain range,Many houses,paper art, exquisite details, warm light,elevationperspective, morandi color --v 4

### 玩好 MJ 需要具备的能力

**

英语**：Midjourney 是纯英文操作，输入英文关键词进行 Ai 绘图，需要你至少英语 4 级水平，能看懂基本的英语指令，输入英语关键词，进行 AI 绘画创作。

**知识管理：** 关键词决定了你输图的质量，需要你具备知识管理能力，建立积累关键词资源库的能力，比如用印象笔记，EXCEL 表格 等工具建立自己的专属图库。

**艺术鉴赏：** 你至少应该懂得基本的绘画风格，摄影参数， 光影效果，3D 技术，超现实主义，波普艺术，立体主义，达达主义，未来主义，现实主义，极简主义，抽象派，新艺术，超现实主义，表现主义，构成主义 各种等各种艺术风格流派

**修图能力：****在 AI **绘画的基础上能修图改图，二次创作创新。青出于蓝而胜于蓝。

搜索能力：能够熟练使用 google wiki chatgtp 搜索，谷歌翻译等 创建关键词词库。

**逻辑思维：**兵器是身体的延伸，AI 是思维的衍生，需要你具备逻辑树形思维创建图像

**创意思维：**需要你脑洞打开，无远弗届，创意--创造--创世。

### MJ 升级到 V5 发布的变化

V5 基本型有什么新特点？

- 风格范围更广，对提示的反应更灵敏

- 图像质量高得多（分辨率提高 2 倍），动态范围得到改善

- 图像更细致。细节更有可能是正确的。更少的不需要的文本。

- 改进了图像提示的性能

- 支持--tile 参数，用于无缝拼接（实验性的）。

- 支持--ar 长宽比大于 2：1（试验性）。

- 支持--iw，以权衡图像提示与文本提示的关系

V5 的风格和提示

- 今天的测试基本上是该模型的 "专业 "模式。

- 它比 V3 和 V4 更 "无主见"，并被调整为提供多种多样的输出，对你的输入反应非常灵敏。

- 这里的权衡是，它可能更难使用。短的提示语可能不那么好用。你应该尝试写更长、更明确的文字，说明你想要什么（即："具有戏剧性灯光的电影照片"）。

- 请在提示聊天室里互相交流，以了解如何使用 v5。

- 我们希望在将 v5 切换为默认样式之前，能有一个 "友好 "的默认样式。当这种情况发生时，我们仍然会让你把它关掉，回到像今天这种'原始'模式。

请注意：

- 这是一个 alpha 测试，事情会发生变化。不要依赖这个确切的模式在未来可用。当我们把 V5 推向全面发布时，它将被大幅修改。

- 现在还没有 V5 的升频器，V5 的默认分辨率与升频后的 V4 相同。如果你点击升尺度，它就会立即给你那张图片本身。

## 绘画工具：Stable Diffusion

![](img/866103bf7a3d96ba0828e1a11b8cf104.png)

**简介：**Stable Diffusion 是一种潜在的文本到图像扩散模型，能够在给定任何文本输入的情况下生成照片般逼真的图像，培养自主自由以产生令人难以置信的图像，使数十亿人能够在几秒钟内创造出令人惊叹的艺术。

**官方网站**

**传送门：**https://stablediffusionweb.com/

### 学习教程

最强 AI 绘画工具——Stable Diffusion 超详细教程

Stable Diffusion 绘画专辑

[AI 绘画指南 stable diffusion web](https://www.tjsky.net/tutorial/488)ui （SD webui）如何设置与使用

### **SD 关键词**

https://arthub.ai/

https://lexica.art/

[h](https://www.krea.ai/?continueFlag=6591d07b3186f4c7e58de1a4bcfaefb0)ttps://www.krea.ai/?continueFlag=6591d07b3186f4c7e58de1a4bcfaefb0

[h](https://promptomania.com/stable-diffusion-prompt-builder/)ttps://promptomania.com/stable-diffusion-prompt-builder/

## 绘画工具：Disco Diffusion

![](img/3d7f4cc3772b41e266266b2033a6121f.png)

### 官方网站

**传送门：**https://colab.research.google.com/github/alembics/disco-diffusion/blob/main/Disco_Diffusion.ipynb#scrollTo=1YwMUyt9LHG1

### 简介

Disco Diffusion 是一个 AI 绘图工具，通过一句话就可以让 AI 画出符合描述的图片，网上可以看到很多令人惊叹的结果。发布于 Google Colab 平台的一款利用人工智能深度学习进行数字艺术创作的工具，它是基于 MIT 许可协议的开源工具，可以在 Google Drive 直接运行，也可以部署到本地运行

### 学习教程

AI 绘画小白神器-disco diffusion（入门篇）

最全 AI 绘画 Disco Diffusion 教程

Disco Diffusion 快速入门：https://blog.csdn.net/qq_42635142/article/details/125481410

小白请进 · Disco Diffusion · 教程合集 ： https://www.douban.com/group/topic/264612340/

AI 画图 Disco-diffusion 本地搭建测试： https://blog.csdn.net/WYT6889688/article/details/124674630

## 绘画工具：LeonardoAI

![](img/2bd8175915f60f3cf68841f25f8060e4.png)

### 官方网站

**传送门：**https://app.leonardo.ai/

### 简介：

1、创作界面可一键生成 prompts；

2、给图片一个特定的姿势；

3、一键延展原图片，一个骑兵变成八个骑兵行不行？

4、一键抠图、涂鸦任何图案，图案可以输入关键词 AI 生成

5、【重磅】可以单独喂喜欢的风格图片（如宫崎骏）大约十几张图就能让大师为你画图了

这个平台审核大约需要 1 周左右时间，对白嫖党也很友好，每天可以免费做大约 250 张图。

## 绘画工具：**Image Creator**

**简介：**微软推出了自家的 AI 绘画工具：Image Creator，一款帮助用户使用 DALL-E 生成 AI 图像的产品。用户只需要给定文本提示， AI 将生成一组与该提示匹配的图像，不过提示文字之目前支持英文，任何用户都可以直接使用这款 AI 绘画工具，不用加入等候名单，需要登录微软账号，而且初始免费额度 25 次，后期可以通过使用 Microsoft Rewards 兑换额外的额度，还是挺良心的。

![](img/d46b2593b1a44be60f515d7c0fd950b7.png)

**传送门：**https://cn.bing.com/create

## 绘画工具：VegaAI

![](img/fbe2132faeb3319def1a3b7164493fa6.png)

### **官方网站**

**传送门：**[Vega AI 创作平台](https://rightbrain.art/text2Image)

### **官方教程**

**指导文件：**[Vega AI 创作平台](https://hf514kmkak.feishu.cn/docx/T0Dldi73eo4NHNxvEs9cqh0Pngf)

## 绘画工具：Draft

![](img/8421fd73eda0942afc45d757537015c2.png)

### **简介**

国内 AI 生成图片社区，傻瓜式操作。

### **官方网站**

**传送门：**[Draft](https://draft.art/explore)

## 绘画工具：文心一格（百度旗下）

![](img/a0c9a5374a57c72c5bcbb95d1dacbe46.png)

**传送门：**https://yige.baidu.com/

新用户有 50 点的免费生成额度。

### **国外绘画软件合集**

###

### **国内绘画软件盘点**

## AI 绘画生成模型共享中心网站

![](img/4f970c2e2fe48d9ebebc529e47be1e66.png)

### 简介

Civitai，是 AI 艺术作品生成社区唯一的模型共享中心，并且免费使用，开源，并不断改进，网站可以查看其他用户发布的 AI 训练生成模型和模型示例照片，对于普通用户可以当作查看作品的网站来使用，上面汇聚大量的 AI 艺术作品，当然也有大量的 19 岁作品，非常赞。

**传送门：**https://civitai.com/

## 提高创作审美的书籍推荐

+   《完美配色》

+   《世界艺术史》

+   《如何看懂艺术》

+   《中国美术五千年》

+   《艺术与视知觉》

+   《艺术的故事》

+   《美的沉思》

+   《美的历史》

+   《西方美学家论美和美感》

+   《谈美》

+   《审美是一生的修行》

+   《感知艺术》

+   《现代艺术的源代码》

+   《艺术的慰藉》

+   《画家之眼》

+   《木马沉思录》

+   《加德纳艺术通史》

+   《时尚简史》

+   《故宫的古画之美》

+   《大话西方艺术史》

# 二、AI 写作

## 写作工具：NotionAI

![](img/1269c758817e61072e0b1575ac09619e.png)

### **简介**

Notion 于 2022 年 11 月首次宣布其基于 AI 的文案写作助手 Notion AI，并于 23 年 2 月 22 日 推出正式版本，具备生成内容、修改内容的功能，可运用在文学、会议日程、新闻稿、销售文案等 内容的辅助创作中，用户可通过网页端、移动端、PC 端登陆，据 Notion 官网截至 2 月 22 日累计用户 超 200 万。 生成内容方面，可精细化便捷修改。用户可在指令框内输入希望写作的内容，或直接从指令框的选 项中选择，Notion AI 将快速生成相应内容，用户在此基础上翻译为特定语言，或对特定段落的行文 风格、段落长度、标点符号等进行精细化的修改。此外，Notion AI 还具备检查拼写和语法错误、创建想法列表的能力，以及写 Excel 公式、写 Python 代码等复杂任务的处理能力。 已开启订阅变现。Notion AI 目前允许用户免费输入 20 次指令，超出 20 次或 23 年 4 月 5 日后，用户使 用时需付费订阅，年度套餐为每年 96 美元，或每月 8 美元，月度套餐则为每月 10 美元，均提供无限 指令次数。

### **官方网站**

**传送门：**https://www.notion.so/product/ai

### **NovelAI 专业**

**小说 AI 元素魔法全收录**

[https://docs.qq.com/d](https://docs.qq.com/doc/DWHl3am5Zb05QbGVs)oc/DWHl3am5Zb05QbGVs

http://wolfchen.top/标签/

https://aitag.top/

https://tags.novelai.dev/

[元素法典——Novel AI 元素魔法全收](https://docs.qq.com/doc/DWHl3am5Zb05QbGVs)录：https://docs.qq.com/doc/DWHl3am5Zb05QbGVs

**NovelAI 方法解析：**https://spell.novelai.dev/

## **写作工具：****Copy.ai**

![](img/59517fb36f4ff3ab4fd1a595cf7d623f.png)

### **简介**

Copy.ai 是一家为企业客户建立人**工**智能驱动的文案写作工具的初创公司。这些自动化工具使用户能够在几秒钟内生成营销副本。CopyAI 的平台具有社交媒体主要文本、标题、链接描述、博客创意等功能。Copy.ai 主要利用 GTP-3 技术来实现，GPT-3 的模型包含了 1750 亿个不同的参数。这个巨大的模型训练了数以万亿计的单词和短语，只需要输入文章的标题和核心内容。通过 AI 生成和人工修正，它能不断地训练出更优秀的人工智能。

## 官方网站

**传送门：**https://www.copy.ai/

## **写作工具：Jasper**

![](img/a99081bb6ce95b1675b272403a1b3829.png)

### **简介**

AI 文案生**成平台 Jasper 2022 年 12 月刚**刚宣布完成 1.25 亿美元 A 轮融资。用户只需在 Jasper 平台输入主题和关键字，便可利用人工智能技术自动生成相应文本。Jasper 可以生成推送广告、博客、促销邮件、信件等多种文本，应用范围十分广泛。Jasper 还能做到帮助企业优化 SEO，提升 Google 搜索排名。

Jasper 目前支持超过 25 种语言，拥有 5 万名活跃用户，共生成超过 60 亿字文本。Jasper 的订阅套餐每月 24 美元起，包括 2 万字的文本生成量。

## 官方网站

**传送门：**https://www.jasper.ai/

## 写作工具：NovelAI

![](img/ad2a79c14c60c73edf826160521bea08.png)

### **简介**

一种文本创作工具，它可以生成基于用户输入的文本内容的小说、故事和诗歌等。用户可以输入一些关键词、主题或简要的情节描述，然后 novelAI 会自动生成一个完整的故事情节。NovelAI 本来是个 AI 写文章的网站，今年训练了二次元专属的 AI 模型。相对于一些通用的 AI 绘画生成器来说，NovelAI 在二次元图上更加的准确。

### **官方网站**

**传送门：**https://novelai.net/

### 学习教程

[【NovelAI】全网最强二次元作画 AI ](https://space.bilibili.com/692627441/channel/collectiondetail?sid=773403)网站 NovelAI 画图使用教程及演示

### 写作工具：effidit（腾讯旗下）

![](img/676f05a346aa83380042fdeb195a3fad.png)

### **简介：**

智能创作助手 Effidit（Efficient and Intelligent Editing） 是由腾讯 AI Lab 开发的一个研究性原型系统，探索用 AI 技术提升写作者的写作效率和创作体验。

### **官方网站**

**传送门：**https://effidit.qq.com/

# 三、国内公司研究动态

[AI 每日大纪实（2024 年度）](https://yunyinghui.feishu.cn/wiki/IrYsw36oril3aokHVFXcR6AOnhc)

[AI事件每日大纪实](https://yunyinghui.feishu.cn/docx/IVeidlyAdoDZFExb1eucDfcan4b)** **

## 2023 年 3 月 30 日

> 【腾讯正式发布 AI 智能创作助手“腾讯智影”】

> 3 月 30 日，腾讯正式发布全新的 AI 智能创作助手“腾讯智影”，推出了智影数字人、文本配音、文章转视频等 AI 创作工具。其中，智影数字人能实现“形象克隆”和“声音克隆”，创作者通过上传少量图片、视频和音频素材，就能得到自己的数字人分身和定制音色，进而快速生成自己的数字人播报视频。

> **传送门：**https://zenvideo.qq.com/activity/invitee?invitation_code=1V5l7C2hruVixj2v

## 2023 年 3 月 29 日

> 【钟薛高推出由 AI 打造的雪糕，定价 3.5 元】

> 3 月 29 日，在钟薛高新品发布会上，创始人林盛介绍，钟薛高推出一款由 AI 打造的雪糕 Sa’Saa，雪糕名字是 AI 起的，包装是 AI 设计的，口味是 AI 建议的。据介绍，该款雪糕定价 3.5 元。

> 钟薛高一直是高端雪糕的代表品牌，此前还被调侃为“雪糕刺客”。该品牌大部分产品单价集中在 10-20 元的区间，最高的雪糕单价曾到达 66 元一支。而今年的钟薛高有了新的转变。

>

> **点评：**文心一言和钟薛高合作了，还能创作文案，这个案例，可能是 AI 创造商业化传播的第一个案例了。

![](img/b30a3e885d3f37288f05b184903f4bac.png)

## 2023 年 3 月 22 日

> **【掌阅科技：公司正在测试相关的 AIGC 技术**，努力通过 AIGC 提升内容创作效率】

> 掌阅科技（603533.SH）3 月 22 日在投资者互动平台表示，根据公司的业务发展需要，公司将会接入并测试各种 AI 模型。公司正在测试相关的 AIGC 技术，努力通过 AIGC 提升内容创作效率，降低内容创作成本。公司将利用在内容版权资源、创作者生态和海量用户资源等优势，将先进的人工智能技术在内容生产、营销推广、丰富产品形态等多方面深化应用。

## 2023 年 3 月 17 日

> **【国内首创房产营销 AIGC 产品“原境空间”发布**，商汤科技携手中原地产加速 AI 布局】

> 3 月 17 日，由中原地产联合国内领先的 AI 科技公司商汤科技举办的“数智时代，原动未来——AIGC 下的地产数智化之路”高峰论坛活动在深圳举办，国内首创的房产营销 AIGC 产品“原境空间”正式发布。

> 原境空间作为 AIGC 首个落地的产物，究竟如何打破时间和空间的限制？深圳中原二级市场战略发展中心总经理钱蛟为大家进行了深入解读。原境空间是中原地产和商汤科技联合打造的房产营销 AIGC 解决方案，产品通过商汤科技 AI+MR 领域领先的数字化技术，为地产行业开创线上与线下一体、虚拟与现实融合的全新营销模式。将助力整个营销流程和客户服务链路的数字化升级，推进房地产营销全流程更加智慧化。

## 2023 年 3 月 6 日

> **【中科院院士潘云鹤：从 ChatGPT **谈 AI 发展方向，建议布局 AIGC】

> 3 月 6 日，中国青年报杭州电消息，中国工程院院士、浙江大学教授、计算机应用专家潘云鹤主讲 2023 年首期“之江院士讲坛”，从大热的人工智能聊天程序 ChatGPT 出发，剖析未来 AI 技术发展的趋势。

> 潘院士分析，ChatGPT 火热的原因，首先是因为它回答问题的水平超乎人们的预期，并且表现出实事求是的特点；其次，ChatGPT 可能会对记者、编辑、作家等文字工作者，以及画家、篆刻家、作曲家等创造文化工作者有影响；此外，OpenAI 的限时开放使用造就了巨大的人气，也引发了许多大企业的竞争，比如微软入股 OpenAI，谷歌宣布开发 Bard，Meta 推出 LLaMa。

> 但 AIGC 一定不会只用于聊天、画画，而是会转向更有价值的应用领域，因此我们要提前布局实体经济的 AIGC、文化艺术的 AIGC、城乡发展的 AIGC。他举例说，比如布局新药物的智能设计生成、儿童教育的智能内容生成、智能社区和生态推演的智能模拟生成。

## 2023 年 3 月 1 日

> 【国内首个 AI 生成内容检测平台正式公测，准确率超 90%】

> 3 月 1 日，人民网消息，AIGC-X 正式开始全网公测。据悉，这是由人民日报社主管、依托人民网建设的传播内容认知全国重点实验室，中国科学技术大学，合肥综合性国家科学中心人工智能研究院联合推出的国内首个 AI 生成内容检测工具。据介绍，AIGC-X 可快速区分机器生成文本与人工生成文本，目前对中文文本检测的准确率已超过 90%。

> 从 AI 绘画、AI 作曲到虚拟人，再到近期爆火的 ChatGPT， 伴随人工智能生成内容（AIGC）技术发展加速，内容生产方式迎来巨大变革。与此同时，在不断“生长”的互联网内容世界，找到一把明晰虚实、分辨真假的技术标尺，也成为关乎行业长远健康发展的关键。

## 2023 年 2 月 13 日

> 【政策支持落地，AIGC 有望迎来加速发展】

> 2023 年 2 月 13 日，北京市经信局表示：“支持头部企业打造对标 ChatGPT 的大模型，着力构建开源框架和通用大模型的应用生态。加**强人工智能算力基础设施布局。加速人工智能基础数据供给。支持人工智能优势**企业在自动驾驶、智能制造、智慧城市等优势领域开展创新应用，全面构筑人工智能场景创新高地”。北京经信局明确表示支持头部企业打造对标 ChatGPT 的大模型，支持算力、数据、应用等相关产业协同发展，AIGC 行业从市场关注上升至政策支持层面。

# 四、国外公司研究动态

## 2023 年 3 月 21 日

> 【**Adobe 的“萤火虫”飞入 A**IGC 战局 落地图片生成 AI 服务】

> 3 月 21 日晚，图像软件公司 Adobe 宣布推出名为“萤火虫”（Firefly）的创意生成式 AI 服务，入局 AIGC。目前 Firefly 已经集成在 Adobe 的企业级创意工具 Adobe Express 中进行内测，开放的是艺术字功能。官方承诺，未来将全面扩展到旗下的 Photoshop、Illustrator 以及 Premiere 等。

## 2023 年 3 月 16 日

> **【Midjourney V5 正**式公测：2 分钟自动生成专业画师级图片】

> 3 月 16 日，著名 AI 自动生成图片平台 Midjourney 宣布 V5 版本正式公测。（测试地址：https://discord.com/channels/662267976984297473/@home）

> 用户通过简单的命令在 Midjourney 平台，2 分钟便能快速生成媲美专业画师级的图片，涵盖漫画、油画、赛博、3D、素描、写实、朋克、水墨等所有主流画作类型。并且还可以根据需求对生成的图画进行二次修改，极大节省了创作时间。

# 五、AIGC 相关工具导航

AIGC 即人工智能自动生成内容，被认为是继专业生产内容（PGC）、用户生产内容（UGC）之后的新型内容创作方式。目前主要用在文字、图像、视频、音频、游戏以及虚拟人上等，具体来看：

**1）文字创作。**

AIGC 生成文字目前主要被应用于新闻的撰写、给定格式的撰写以及风格改写。比如用户可以通过输入一段对于目标文章的描述或者要求，系统会自动抓取数据，根据我们描述的指令进行创作。

**2）图像创作。**

技术平台降低了艺术绘画创作的门槛，用户只需要通过输入文字描述，计算机将会自动生成一张作品。

**3）视频创作。**

例如 Google 推出了 AI 视频生成模型 Phenaki 能够根据文本内容生成可变时长视频的技术，在公布的 DEMO 中，Phenaki 基于几百个单词组成一段前后逻辑连贯的视频只需两分钟。

**4）音频剪辑。**

AIGC 生成音频早被应用于我们的日常生活当中，比如常用的手机导航中的声音。更深层次的应用将会是虚拟人领域，AIGC 不仅可以生成虚拟人的声音，并可以创造出说的内容。

**5）游戏开发。**

AIGC 在游戏当中的应用可分为两方面，一方面用于场景和故事的搭建，另一方面玩家可以通过 AIGC 的平台工具来创建自己的虚拟人，可以用于游戏中的打金等活动。

**6）代码生成。**

资料显示，2022 年 AIGC 发展速度惊人，迭代速度更是呈现指数级爆发，其中深度学习模型不断完善、开源模式的推动、大模型探索商业化的可能，成为 AIGC 发展的“加速度”。

市场空间方面，Gartner 预计到 2025 年，生成式人工智能将占所有生成数据的 10%。根据《Generative AI：A Creative New World》的分析，AIGC 有潜力产生数万亿美元的经济价值。

而国泰君安表示，未来五年或将有 10%-30%的图片内容由 AI 参与生成，相应或将有 600 亿以上的市场规模。

![](img/0e0a301e60e107b1eb29bf88929a2aa8.png)

1.

![](img/32419881f7b25518cf0c4ea28c829202.png)

[AI工具汇总（AIGC时代：超级个体的崛起）.xlsx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RzfNbAoLyoaQqYxZs49cBnofn5d)

1.  AI 帮个忙 | 多功能 AI 小帮手

![](img/33881e98fd6a335a6f0b19906c54f855.png)

1.  **传送门：**https://open-gpt.app/

简介：立即使用海量的 ChatGPT 应用，或在几秒钟内创建属于自己的应用。

![](img/0c7d738b4dce9b65ec69ddcbdf1ea76b.png)

**4.AI 工具导航**

**传送门：**https://ai-bot.cn

![](img/759577250fa30bf5f41203e887a04963.png)

# 六、投融资相关

**【对话式 AI 自动化平台 Uniphore 收购 **Red Box，以增强 AIGC 能力】

2023 年 3 月 14 日著名对话式 AI 自动化平台 Uniphore 宣布收购，拥有 35 年历史的语音识别厂商 Red Box，以增强 AIGC（人工智能生成内容）能力。通过此次收购，Uniphore 将增强实时通话后的数据捕获和转化，同时保证每一个语音通话和屏幕的数据安全。将业内领先的对话式智能自动化与领先的语音识别相结合，可以从语音客服的对话中提取更高的商业价值。

**【**提供类“ChatGPT”功能，将文本自动生成歌曲，Voicemod 获得 1450 万美元 A+轮融资】

2023 年 3 月初，AIGC（人工智能生成内容）平台 Voicemod 获得 1450 万美元 A+轮融资，本次由 Kfunds growth fund 领投，Bitkraft Ventures 和 Discord 前首席营销官 Eros Resmini 跟投。Voicemod 是一家聚焦在 AIGC 音频领域的老玩家（创立于 2014 年），主要提供文字自动生成歌曲、AI 变声器等功能。用户只需要输入文本（只支持拉丁字符），简单 4 步便能迅速生成流行、古典、摇滚等类型歌曲。

**【正在关注 AIGC 早期企业，红**杉中国种子基金完成 4.8 亿美元募资】

2023 年 2 月 28 日，红杉种子宣布，旗下第三期种子基金（Sequoia China Seed Fund III）已于 2022 年完成 4.8 亿美元募资，现已正式进入投资期。此次募资到位后，红杉种子目前管理近 150 亿人民币，是国内管理规模最大的早期基金之一。

“过去几年来，从投资的领域来看，我们重点布局了新能源与双碳、EV（电动汽车）产业链、创新医疗技术、合成生物、金融科技、基础软件、机器人、设计和内容平台、效率工具和新消费等具有科技驱动、结构性创新和高成长属性的领域。”红杉种子强调，“当下，我们也在密切关注并开始布局 AIGC（即 AI Generated Content，是指利用人工智能技术来生成内容）领域的早期企业。”

【橙麦智能软件系统近日拿到数千万元 A 轮融资】

2023 年 2 月 6 日，AI 数字人技术及 AI 智能语音技术开发商橙麦智能软件系统近日拿到数千万元 A 轮融资。该笔融资将用于旗下产品「橙麦智能」的更新迭代、品牌宣传及市场推广。「澄麦智能」软件系统的开发及运营团队 广州橙麦科技有限公司成立于 2021 年，主要从事软件和信息技术的开发运营，主要研发方向包含人工智能、互联网大数据、物联网等。

【国内外 AIGC 公司融资情况】

![](img/336d28a146b2b1e1b84983378a7a0934.png)

![](img/fed0e656de974bf5289426259716e3b8.png)

![](img/826468a1ff3446d8cb70bee8fd42787b.png)

![](img/c3f6dd9bab2e63ec34c0bd903104936c.png)

![](img/5a94b8079227bddc4b5494a28ad8d37d.png)

【维观 TMT】解析 AIGC 的商业应用、投资与风险

中信建投：AIGC 应用研究专题

# 七、相关行业报告

## **【AIGC 模型十大开发机构】**

![](img/132c05d061bda6feab4963fc3978477b.png)

![](img/d96c4987f960a1485fce1a9c8e0c5e23.png)

## **【中国 AIGC 市场地图】**

![](img/6db8d67ed449ff6ae2d96c630f7d8cc2.png)

![](img/ed7a589b513cbf9836c2cb70d2f47a12.png)

## 【国外 AIGC 市场地图】

![](img/26c2cd2b502bdaf576062aa7a5d065df.png)

## 【AIGC 技术场景的内容分子及具体应用】

![](img/f28dd756d40f2a94193331c22642609f.png)

## 【部分国内外科技公司布局生成式 AI 部分】

![](img/1af7a9fd2c8d25be5919d3cafe5e210e.png)

## 【中国 AIGC 领域最值得关注的 50 家公司】

![](img/97f9cf5a0be269d407c4e8a6f916ac0c.png)

## 【AIGC 国内理论市场空间测算】

![](img/071da9342ffa442317e74afe9be77262.png)

## 【AIGC 赛道估值最高的公司】

![](img/57a3c7f8782a59db4ed0011ff8d97f89.png)

## 【2022 年度 AIGC 应用创新 TOP30】

![](img/44dd4412a005e69b5c0216dcec982a41.png)

![](img/2299e72ff9f3ea68189bbd73ec5dcc38.png)

![](img/adafa9c8f3ae5b9d65482b8a7c0a9252.png)

1.

![](img/47f96cbc9d1896cd33d841ffc871a96c.png)

[腾讯研究院AIGC发展趋势报告2023.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YoNpbvSAAotaAkxSj31cyX9Onqb)

1.

![](img/b0dd99758c496185bca3f3b8340da8c4.png)

[【中国信通院】人工智能生成内容（AIGC）白皮书（2022年）【洞见研报DJyanbao.com】.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SZbGbafQvojmaWxq5pXclwvgneo)

1.

![](img/458b32b8313f3ecd2d675a6e066c36a1.png)

[2023 AIGC人才趋势报告.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RpLRbWX5ooESLWxu4oEc7SIIn4d)

1.  2023 年 3 月 AIGC 产业大会和清华研究院报告

1.

![](img/b41137cbb71fc562e9526effa3fbf29c.png)

[20230330-中国AIGC产业全景图报告.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Xp9DbIfOEoM3iAxTmNTcxzsJnKd)

1.

![](img/07efcaea95e21ea8fabc3ea308ec782d.png)

[中国AIGC产业大会——量子位（部分节选）.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RYvcbfKJeoN0n4xrXTIcn5Skn2e)

1.

![](img/03c362b69775e1224fb3b806f5566ca1.png)

[清华-AIGC（GPT-4）赋能通信行业应用白皮书-20230330.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MBZKbWulxoDKxixM3qKcdhVAnKM)

1.

![](img/4f597ef388621048a45850385c065b23.png)

[【招商证券】AIGC：打开元宇宙大门的新魔法棒.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OBtibVWfQooyQlxd4jec0HXcnlb)

1.

![](img/72ba058935b4b3419501aa941d508ab8.png)

[【华鑫证券】传媒行业深度报告：AIGC将如何影响传媒行业.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CmJybzdPYooVWFxGYvCcB4Handc)

1.

![](img/4ff215a6434e42a5f314b7b08cd0555e.png)

[【量子位智库】AIGC_AI内容生成深度产业报告.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CyICblTTMozcFmx8iHUcztqHnmd)

1.

![](img/cf78752383202a667d82626a59e5b456.png)

[【浦银国际】序幕揭开：ChatGPT将AIGC推向“平民化”时代.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CDatbvUizoSwwwxlsWIcHxGnnyj)

1.

![](img/e19e5ea78b5c210e82d3db6437717631.png)

[【国盛证券】Web3时代的生产力工具：AIGC.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XF19bE5Enou48VxOgBjcsjpsnpc)

1.

![](img/6f06a1b7d693d63d93b16f6e265a3977.png)

[【招商证券】ChatGPT快速流行，重构AI商业模式AIgc投资机会梳理.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PAhPbHxTwoCNfUxpDnQchNrHnPd)

1.

![](img/5144b7e58059327a125e4f8a17b230c7.png)

[【东吴证券】商业贸易：落地场景与发展方向探讨.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DZU8b3dzHo7JmJxxxLfcJgdsnUd)

1.

![](img/07331d0bf73b3d8cd3fe9372a3bc03bc.png)

[【西部证券】AIGC行业专题报告：2023年有望成为AIGC的拐点.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Bgkjb17P0oByYSxkB27c3PtYnVf)

1.

![](img/7bc7d1b5ca80c5f1b66f78026087453d.png)

[【华福证券】计算机行业：AIGC&ChatGPT发展报告.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Wnz9b9bpBotQvgx1R3BcdiyUnEh)

1.

![](img/23c1acd1e938c4f79b37956c839899ed.png)

[【浙商证券】互联网电商：AIGC之AI绘画：技术与应用双突破，生产力变革在即.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Q4XpbOZojoYc5XxbiHecO4gCnbf)

1.

![](img/e2a56732672274231851ffaf993297b2.png)

[【招商证券】AIGC：打开元宇宙大门的新魔法棒 .pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OsQcbzyzLoSbfCxHWpVc4WnNnCc)

1.

![](img/ca57c40d69538df4f81cbed3d15397fc.png)

[甲子光年-2023AIGC应用与实践展望报告-2023.4-47页.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VadQb6PKRoTUzJxMaSyc56RYnwf)

## 【行业峰会相关】

1.  [国内首场高规格 AIGC 峰会盛况出圈！万字干货热聊 ](https://mp.weixin.qq.com/s/m0rAXvFWabXzyRNzipfrqA)GPT-4 时代，浓缩 21 位大牛演讲

# 八、赚钱相关案例

## 【AI 绘画抖音平台关键字引流变现】

通过汇总 AI 绘画关键词引流，小程序挂 AI 绘画课程变现。

该模式已经是机构矩阵玩法（且 X 学院），批量出视频，视频+直播，轰炸的很厉害。

现在已经变现 6 位数～

![](img/143810e109854d3c401f0f9d6316e070.png)

## 【 AI 绘画应用：专门用 AI 生成纹身的网站】

采用订阅制的方式来购买图片。网站：www.tattoosai.com。

![](img/96afd1c2286ec96b8f4e22992ca928ad.png)

# 九、AIGC 赚钱风向标

## 【用 AI 工具生成奶奶的虚拟数字人】

B 站一 up 主结合 AI 绘画、ChatGPT、智能语音等 AI 工具，以手把手教学教程的形式制作了已去世奶奶的虚拟数字人视频。教程非常详细，列举了使用的各种工具，可以跟着一步一步制作虚拟人视频。另外，已故老人的虚拟人视频绝对是一个巨大是隐形市场需求，很多人都会有需要。跟着 up 主学习一下，制作出来可以马上发到公域平台引流变现。

用 AI 工具生成我奶奶的虚拟数字人_哔哩哔哩_bilibili

![](img/a32e2b285693c80828168af86aaad000.png)

【MJ+ChatGPT+line 贴图=表情包变现】

mj 用来做 line 贴图，可以直接销售。提示词也用 ChatGPT 生成，已经有人几天直接变现了一万多日元

上架后就是躺赚，比百度文库好多了，ai 绘画非常适合发散性的内容制作，用来做微信表情包，也可以引流。

![](img/b8150b337279295eaa68d193db5ebbc9.png)

## 【AI 绘画技术做壁纸或头像】

通过关键词，分享给身边的朋友，去做头像，朋友看到上面的字就分享出来，一波接一波的分享，带来的引流量会变大。

![](img/b24c0e54e5c05f3f84fc2d3ed9285e0c.png)

## 【AI 绘画师成为最新的工种】

BOSS 直聘上面开始出现 AI 绘画师的工种，目前游戏行业，已经越来越普遍了。

![](img/bbd01f20a0b7b5fffefa62e075514af6.png)

## 【AIGC 相关研发岗位逐渐增多】

三年工作经验即可斩获百万元年薪，AIGC 领域上演抢人大战

![](img/2dace5cf481f560f96df9207a661ad4a.png)

![](img/0b2fc59189fe2d1207f8899f1e4b879d.png)

# 十、实际场景案例

## 【「小库设计云」AI 生成实际住区拿地方案】

在建筑产业的投研阶段，2021 年出台的 “两集中” 政策（集中供应土地和集中拍地）使大量土地集中在一个月内推出，开发企业需要在短时间内完成对每一块土地的投资评估，其中最核心的是如何在一块地上找到最优的建筑规划方案，获得最大的产品货值和投资回报测算。原本完成一个住宅规划概念方案的时间至少需要 3-5 天，无法满足业务需要，如此就提出了对投前建筑规划方案的极致效率的需求。

小库团队推出 AIGC 的建筑规划方案，只需原来 30% 左右的时间就可以输出初步方案。更重要的是，AI 可以生成和优化一些人没有想到过或难以靠手动穷举推敲出的方案，从而获得性能或经济性方面更优的成果。如在中国金茂的某江西项目中， AI 生成的方案不仅在时间上仅为原有方式的 20%，项目总货值上比原有方案增加了 5600 万。在 2021 年 9 个月的地产拍地市场中，团队累积完成了近千个项目及近万个方案，协助客户成功拿地数十块。

![](img/2f5a952be3d6bf6b29957be34d376b25.png)

## 【彩云小梦用 AIGC 生产内容在 B 站出圈】

彩云小梦是一款 AI 写书软件。你可以通过给出一些基本的世界设定，让你的彩云 AI 围绕故事主题去进行联想创作。彩云小梦由彩云科技出品，因为有了彩云天气和彩云小译这两款成功产品的开发运营经验，彩云小梦自身研发素质过硬，曾风靡 B 站创作圈。

彩云小梦创始人兼 CEO 袁行远在访谈中说道：「就像游戏让我们体验各种光怪陆离的世界，看电影是对生命的延长。人只能活一辈子，这是很遗憾的，但电影、小说都是让我们体验不一样的人生。而现在有 AI 技术，可以让你更沉浸式的体验，不仅可以阅读基督山伯爵，也可以扮演基督山伯爵，而且是自由的扮演。它也是对生命的延长，以前所未有的一种形式。」

![](img/48b3b3e1e8c2395c6ffbca15cad32a8a.png)

## 【AI 绘画结合电商详情页效率大大提升】

AI 模特结合服装，可以直接做图。

![](img/64006140432903469cff25fd727101e7.png)

## 【腾讯网易等大厂实践案例】

走在前面的大厂，及互联网企业是如何借力 AIGC 的，这些内容给你参考学习！

运用 AIGC 人工智能生产内容-腾讯

AIGC|探索 AIGC 在网易严选中的应用

---

# AIGC 学习交流社群

1.持续关注并更新 AI 的**相关资讯动态，研究报告，赚钱案**例，常见问题，永久免费在线查看，无收费内容，目标是成为最大的免费 AI 学习社群。

2.已建立社群聊聊 **AI** 发展及应用，**没进群的可以扫码加入，群满可加 AI 研究爱好者大国微信 ID：zyg90s（备**注：AI），已经在社群内的无需重复加入，开源知识库及社群学习全部为公开免费分享。

3.**关于文档负责人大国**：高帆文化CEO，曾就职于教培大厂，目前创业，主攻研究AI落地应用、企业流量增长及达人IP孵化。

**5000人飞书学习交流群**

![](img/beae661cf14f9df06c22d8f0babafd53.png)

**飞书交流群**

![](img/e56330178689ab4e50a596c7ce4aa668.png)

微信好友（ID：zhangyuguo94s）

**微信入群渠道**

进群或者加好友了解更多 AIGC 实战案例

**觉得有用欢迎留个赞**