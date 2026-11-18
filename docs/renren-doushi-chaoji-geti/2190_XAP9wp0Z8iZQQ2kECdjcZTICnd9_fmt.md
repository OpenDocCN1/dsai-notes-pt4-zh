
# Sora 学习手册

> 来源：`https://xuqiwei1986.feishu.cn/docx/MvwNdG3imoztmmxwi6nc3h5GnOc`

> **适用人群：**互联网关注 AI 视频发展人群

> 首发时间：2024 年 2 月 18 日

> 更新时间：2024 年 12 月 10 日

> **内容出品人：**

> **使用建议：**如果需要快速定位到精确内容，可以使用快捷键 `Ctrl + F`/`Command + F` 的形式，搜索关键字/词，查找你想要的内容，PC 端查看学习效果更好。

> **持续更新 OpenAI 的 Sora 模型相关**资讯动态，研究报告，赚钱案例，实用场景等板块内容，永久免费在线查看，欢迎收藏转发支持，文末有惊喜，大国出品，必属精品，和 50W+读者共学。

> 🎯全网最接地气 AI 知识库：「**AI 实验室」知识库**（`https://yunyinghui.feishu.cn/wiki/space/7340161844864598017?ccm_open_type=lark_wiki_spaceLink&open_tab_from=wiki_home`）AI 绘画、AI 写作、AI 视频等应用技能）

## 更新日志

## 一、基本介绍

在 2024 年 2 月 16 日，Open AI 宣布推出全新的生成式人工智能模型“Sora”。据了解，通过文本指令，Sora 可以直接输出长达 60 秒的视频，并且包含高度细致的背景、复杂的多角度镜头，以及富有情感的多个角色。

这意味着，继文本、图像之后，OpenAI 将其先进的 AI 技术拓展到了视频领域。OpenAI 亦表示，Sora 是能够理解和模拟现实世界的模型的基础，这一能力将是实现 AGI（通用人工智能）的重要里程碑。

对于 OpenAI 视频生成模型的出现，业内其实早有预期，但仍有人评价称“比想象中来得更快”，亦有人振奋地表示“我们真的看到新工业革命来临”。

> Sora 的 60 秒视频基本上展示了它解决了以下 3 个 AI 视频技术难题：

> （1）真实物理世界的逼真模拟

> （2）角色的一致性

> （3）长镜头的稳定性

**Sora 官方网址：**

`https://openai.com/sora`

![](img/e3262846b6fddf21ce6aaa47c5706dbd.png)

**官方 Youtube 介绍视频：**

`https://www.youtube.com/watch?v=HK6y8DAPN_0`

**Sora 论文网址：**

`https://openai.com/research/video-generation-models-as-world-simulators`

> 目前 OpenAI 官网上已经更新了 48 个 Sora 生成的视频 demo，色彩艳丽，效果逼真。

> **官方网站：**`https://openai.com/sora?open_in_browser=true`

> 2024 年的真正红利点：文生视频。

> **友情提示：**目前 sora 没有对外开放，也没有内测申请，请不要相信任何账号买卖，课程培训！本文档多转一次，就会少一批人被割韭菜！在此一并鸣谢帮助我们转发的大 V，自媒体人和团队负责人，以及千千万万的读者。

### 正式发布

官方正式网址：`sora.com`

![](img/acdea770ed0609a1a6d31e1c455f446b.png)

OpenAI 推出文生视频模型 Sora 的新版本 Sora Turbo，称其比 2 月预览的 Sora 模型快得多。

Sora 可生成最高 1080p 分辨率、最长 20 秒、16:9 / 1:1 / 9:16 画面比例的视频，支持用户输入文字或上传图像，并上线全新 UI 界面，以便对生成视频进行修改、创建、扩展、循环、混合，或用文本生成全新的内容。

> OpenAI 在 Sora.com 上发布一个独立产品，免费提供给 ChatGPT Plus 和 Pro 用户。

> Plus 用户每月最多可以生成 50 个 480p 分辨率视频，或更少的 720p 分辨率、5 秒视频，对应月费 20 美元（折合人民币 145 元）。

> Pro 订阅者则最多可生成 500 个视频，并支持 20 秒时长、1080p 分辨率，可下载无水印版视频，对应月费 200 美元（折合人民币 1450 元）。

> 平摊下来生成一个视频花 2.9 元。

> 有网友发现，同为付费用户，ChatGPT Plus并不能生成带有人物的视频，只有ChatGPT Pro才可以。

![](img/0c3f6f220dc5d999ce910e5768d32262.png)

ChatGPT Plus 和 Pro 每月分别为用户提供 1000 和 10000 积分。其中 480p 视频需要 20-150 个积分，720p 视频需要 30-540 个积分，1080p 视频需要 100-2000 个积分。Pro 用户则享受无限量的 relaxed 视频。

![](img/1841017d6c27a4ed715944d95f408c04.png)

OpenAI 正在为不同类型的用户制定不同定价，计划于明年初推出。

### 正式发布时间预测

![](img/878c8519346e68bc6c6d414f1fd3c0b6.png)

【Sora 团队负责人：Sora 还不是一个产品 短期不会向公众开放】

3 月 13 日，Sora 核心团队三位负责人露面接受了专访。他们在采访中透露，Sora 目前还处于反馈获取阶段，还不是一个产品，短期内不会向公众开放。

除了这个发布时间之外，Mira 还披露了 Sora 的其他几个关键信息：

+ 关于 Sora 生成视频的时间：对于 720P 分辨率或 20 秒长视频，生成时间大概在几分钟，这取决于提示的复杂性。

+ 目前视频依然没有声音，但未来 Sora 可能会支持视频音效。

+ 在 Sora 的训练之中，Sora 的数据源是大家颇为关心的问题。当被问及 Sora 训练的素材是否包括 YouTube、Instagram 和 Facebook 上的视频时，Mira Murat 的回答是不确定的。她没有明确确认这些平台的视频是否被用作训练数据，称 Sora 的训练数据来自公开可用和授权的数据，最后结束后确认训练数据包括 Shutterstock 的内容。

+ 计算资源：Sora 的生成过程相比之前 ChatGPT 需要更多的计算资源，OpenAI9 正在致力于优化技术，相信未来应该也会和此前 Altman 所讲的 GPT 一样，成本得到大幅降低，最终能够极大地便于公众使用。

+ 安全性和可靠性：Sora 目前正在进行红队测试（测试工具的安全性、可靠性和缺陷），以识别潜在的漏洞、偏见和其他有害问题

+ 内容限制：与 DALL-E 类似、Sora 在生成内容时也会有一些限制、例如不生成公众人物的图像（但 DALL-E 很神奇的是能够生成公司 logo），以避免误导和滥用。

+ 裸露内容：关于裸露内容的处理，提到 OpenAI 正在与艺术家和创作者合作，以确定工具应该提供的灵活性水平和合理的限制，但没有具体提到如何区分艺术和色情的内容

+ 正在研究对视频进行水印标记，但是相对于水印，视频内容的审核限制更为重要、因为随着生成的视频更加真实、用户将无法分辨视频内容的真实性，防止生成误导内容。

### 央视报道

![](img/59f99d2c22c38e98c55849ea1c111885.png)

[`555903b68f3a424f83a20c2d80ece3.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RWqQbQKBWoq3P3x0F9HcXYAkn1c)

中国首部 AI 动画片《千秋诗颂》

2 月 23 日晚上举行的中国首部 AI 动画片《千秋诗颂》启播暨中央广播电视总台人工智能工作室揭牌仪式上，上海人工智能实验室研发的文生视频大模型“书生·筑梦”亮相。这个 AI 模型已经开源，授权用户单位免费商用。它的参数量超过 30 亿，可根据输入的提示词生成有故事性、含多镜头的分钟级视频，具有转场流畅、故事连贯、画质高清等特点。

![](img/3293118e564707328467e57a05c7809e.png)

[`029d2c64-70b3-43d8-a3ee-844b196af83f.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JLsQb8ySEoUbWyxEYhdcNbxOnAc)

由于数据、算力等资源限制，'筑梦’在视频生成能力、时长、质量上与 Sora 还存在差距。”上海人工智能实验室领军科学家林达华博士说，“但另一方面，随着同总台的深度合作，实验室研发的型在对中华文化的理解表现，以及与媒体编创实际生产流程的融合上，逐渐显露出特色优势。这类产研结合的新技术发展模式，将使我国 AI 技术发展走出差异化道路，并形成我们的文化特色。

### 博主解读

全网最详细 Sora 解析，OpenAI 又一王炸！【Git 源宝】

[Sora来了，我们离AI模拟世界还有多远](https://caaiartlab.feishu.cn/docx/LgmFdXTRwo0m5yxkQRIcO6RGnRf)**【详细分析解读】**

关于 Sora，我有十个小白问题

### Sora 怎么申请使用

**渠道一（非官方）：**

需要强调给大家的一点是，截止至 2024 年 2 月 18 日，Sora 目前没有公开测试，也没有内测申请渠道，国内是没有渠道可以去尝试的，下面这个信息源不一定真实，大家仅供参考。

![](img/1701d2bd70156be62c03fd1118d53cac.png)

![](img/f2e78767b8a7b4293b024e5d5952a75b.png)

**渠道二（官方）：**

`https://forum.openai.com`

OpenAl 的官方论坛，据消息透露加入论坛有机会获得内测资格

![](img/3182969f388c06b92b9669f706e56e5a.png)

**渠道三（官方）：**

Red Team 开始公开招募了，要求比较高。Sam 之前说过，如果进入到红队，可以有 Sora 的内测机会 `https://openai.com/form/red-teaming-network`

![](img/76533eb5d6e2bedc83094240448b4be7.png)

### 模型效果对比

![](img/26ef584bb9a5372217caa737151fe957.png)

![](img/4dd45df8a976264738fdc4129f252541.png)

![](img/0f1031fbd0dfae13f92d1c0e27128f9d.png)

[`05f698ff0d851448e2749474649b3b.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/K3WbbLJk2oeWXyxljZTcOXGFnog)

![](img/f8e2189483b5e44b76a064e747e06fbe.png)

### AI 视频玩家盘点

据不完全统计，过去一年来全球 AI 生成视频类赛道的融资已经超过数 10 亿美元，其中大部分的公司都以种子轮、A 轮为主。就在今年 1 月底，A16Z 还专门针对 AI 视频生成领域做了一个全面的回顾和展望，并盘点了市场上超过 20 个明星产品。

![](img/11df8267e09193a865409bad38fd4a0e.png)

2023 年 AI 视频生成行业的参与者盘点

![](img/3f5f97be474bcf7ffebdc6d8ff1eb989.png)

![](img/7eac3332d12588e6999ba8544384a25f.png)

![](img/8388c3bc944ffc83572753c892d4d3e1.png)

### 大厂文生视频布局动态

大厂在视频领域的动作不断，基本上在推进语言模型业务的同时也布局了视频业务：

+ 字节旗下剪映最近动作频频，目前，剪映已经在邀请一些博主内测旗下 AI 创作平台 Dreamina 的视频生成功能。

+ 阿里通义实验室目前开发了开源视频生成模型和代码系列 VGen。去年年底，阿里发布了最新 AI 应用：Animate Anyone，可以根据一张人物照片，生成人体动画视频。

+ 百度在年初推出了一款视频生成模型 UniVG，能够处理各种文本和图像的组合输入。

+ 腾讯在 2023 年 12 月与北大和香港科技大学联合发布了视频生成模型 AnimateZero，并在今年发布了一个可以提升视频质量的视频处理工具 VideoCrafter2

### 中泰证券：文生 3D——人工智能的下一站

中泰证券表示，以大模型、生成式为代表的新一代人工智能技术与产业变革如火如荼，从 Chat GPT 代表的文生文，到 DALL·E 代表的文生图，再到 Sora 代表的文生视频，“暴力美学”在持续突破技术的天花板，多模态也成为了共识的发展趋势，继文本、代码、图片、视频之后，中泰证券认为下一个有可能实现突破的模态大概率是 3D。目前海外在 AI+3D 技术上主要分为工业场景探索与非工业场景探索。非工业场景应用探索主要以谷歌的 DreamFusion 和英伟达的 Magic3D 为代表，主要面向游戏、元宇宙中的 3D 资产设计；工业场景应用则主要以衍生式设计软件为主，如 PTC 的 Creo 以及 Autodesk 的 Fushion 360 均提供衍生式设计能力。投资建议：产业视角建议持续跟踪关注文生 3D 建模领域的进展。

### Sora 的 6 大优势

《每日经济新闻》记者经过对官方技术报告的梳理，总结出了 Sora 的 6 大优势：

**（1）准确性和多样性：**Sora 的显著特征之一是能够准确解释长达 135 个单词的长提示。它可以准确地解释用户提供的文本输入，并生成具有各种场景和人物的高质量视频剪辑。这一新工具可将简短的文本描述转化成长达 1 分钟的高清视频。它涵盖了广泛的主题，从人物和动物到郁郁葱葱的风景、城市场景、花园，甚至是水下的纽约市，可根据用户的要求提供多样化的内容。

**（2）强大的语言理解：**OpenAI 利用 Dall-E 模型的 `re-captioning`（重述要点）技术，生成视觉训练数据的描述性字幕，不仅能提高文本的准确性，还能提升视频的整体质量。此外，与 DALL·E 3 类似，OpenAI 还利用 GPT 技术将简短的用户提示转换为更长的详细转译，并将其发送到视频模型。这使 Sora 能够精确地按照用户提示生成高质量的视频。

**（3）以图/视频生成视频：**Sora 除了可以将文本转化为视频，还能接受其他类型的输入提示，如已经存在的图像或视频。这使 Sora 能够执行广泛的图像和视频编辑任务，如创建完美的循环视频、将静态图像转化为动画、向前或向后扩展视频等。OpenAI 在报告中展示了基于 DALL·E 2 和 DALL·E 3 的图像生成的 demo 视频。这不仅证明了 Sora 的强大功能，还展示了它在图像和视频编辑领域的无限潜力。

**（4）视频扩展功能：**由于可接受多样化的输入提示，用户可以根据图像创建视频或补充现有视频。作为基于 `Transformer` 的扩散模型，Sora 还能沿时间线向前或向后扩展视频。从 OpenAI 提供的 4 个 demo 视频看，都从同一个视频片段开始，向时间线的过去进行延伸。因此，尽管开头不同，但视频结局都是相同的。

**（5）优异的设备适配性：**Sora 具备出色的采样能力，从宽屏的 1920x1080p 到竖屏的 1080x1920，两者之间的任何视频尺寸都能轻松应对。这意味着 Sora 能够为各种设备生成与其原始纵横比完美匹配的内容。而在生成高分辨率内容之前，Sora 还能以小尺寸迅速创建内容原型。

**（6）场景和物体的一致性和连续性：**Sora 可以生成带有动态视角变化的视频，人物和场景元素在三维空间中的移动会显得更加自然。Sora 能够很好地处理遮挡问题。现有模型的一个问题是，当物体离开视野时，它们可能无法对其进行追踪。而通过一次性提供多帧预测，Sora 可确保画面主体即使暂时离开视野也能保持不变。

### Sora 可能带来的影响

如果 Sora 能够真正意义上实现文生视频，可能会带来哪些影响呢？

1、没有演员的影视作品出现，对演员来说是个小挑战，将出现真正的“虚拟偶像”，此前的二次元人物并没有真正达到“偶像”的级别。

2、利好编剧行业，剧本、文本创作力成为核心竞争力。

3、Sora 可能才是真正的文生视频，此前的文生视频大多只有 2 秒，仅仅是对象的小幅度移动。

4、OpenAI 继续拉大领先程度，对众多还在进行大模型测试打分 pk 的厂商，构成压力。

5、直接的影响是影视行业，特别是特效行业。使用 AI 来制作一些特效和高风险的镜头，可以大幅降低拍摄成本，也可以避免很多危险。

6、摄影师行业也会受到影响，用文本来生成一些视频，可以省去很多拍摄工作。

7、短视频流行开以后，视频剪辑师也随之成为一个热门职业。如果视频剪辑的工作可以用 AI 来代替，可能会有很多视频剪辑师失业。

8、对于很多短视频创作者来说，用 AI 来替代繁琐的剪辑工作，可以大幅提高工作效率。

9、很多歌手拍摄 MV 都是大成本制作，如果可以用 AI 来生成所需要的 MV 画面，也可以省去很大一部分制作成本。

10、另外，如果真正意义上的文生视频得以实现，可能会有不法份子利用这项技术实施新手段的违法犯罪。

不过，从 Sora 官网目前展示的视频画面效果来看，效果还没那么逼近真实，短期内不会产生让行业失业，但会有辅助作用，做个动画片应该问题不大。



# Sora 分析报告

在 Sora 官网，OpenAI 表示，Sora 是能够理解和模拟现实世界的模型的基础，OpenAI 相信这一能力将是实现 AGI 的重要里程碑。

至于 Sora 文生视频到底能带来多大影响，会不会翻车，还需要“让子弹飞一会儿”。

## 关联阅读

**笔记侠：**Sora 降世，5 大行业一夜变天！

**中央政法委长安剑：**Sora 来了，哪些行业将受冲击？

**任泽平：**Sora 横空出世，会颠覆哪些行业？

**中国电影报：**大模型 Sora 对影视创作的影响

![](img/b087f3f7a1bb0f0c716a488a6f129b9d.png)

## 对投资市场的机会

### 1. Sora 将为 AI 赛道带来的投资机会

止于至善投资总经理何理：Sora 的发布有望再度掀起 AI 上涨热潮。尽管 Sora 的视频时间仅有一分钟，但按照 OpenAI 的迭代速度，生产出几十分钟的 AI 视频也不远了。未来数年内，将对整个影视制作和短视频行业带来颠覆性冲击。目前 AI 热潮已进入第二阶段，重点在于产品层面的 AI 创新以及数据层面的业绩检验。未来行情的爆发属于应用端与基建端并举，应用端重点关注与 AI 融合较好的 TOB 端应用，基建端主要关注 GPU、光模块、液冷、数据要素等。

中信建投证券：AI 仍然是当前科技投资的主线，海外 AI 大模型和应用保持较快迭代速度，随着 Sora、Gemini 1.5 Pro 等大模型的发布，多模态能力显著增强，应用场景进一步拓宽，这将带动算力基础设施的投资，建议关注光模块、ICT 基础设施等板块。

申万宏源：对于短视频 UGC 平台，借助 AI 升级剪辑工具拉动创作者经济的进一步发展，但也需关注是否会有竞争的变化。对于深度内容受益，IP 价值提升，对浅显低门槛内容竞争加剧。对创意工具，应对竞争的要素是 B 端客户粘性和版权库积累。AI 造假或影响新闻生产和传播，识别 AI 视频将是内容审核关键环节。可关注光通信产业链、算力设备产业链、多模态算法布局、有丰富 IP 储备的深度内容创作、商业化 AI 应用场景的美图等方向。

招商基金：AI 赛道大概率仍是未来一段时间的主线之一，后续仍可重点关注 AI 产业链内算力与应用的配置机会。

博时基金：受海外 AI 应用取得突破性进展的催化，AI 板块走势强劲。当前 AI 板块的产业逻辑较为顺畅，后续的增长持续性或有望提升，业绩也有可能超预期，相对看好其未来表现。

![](img/2023d2fc140b1ea30337b73a5a9ced22.png)

### 2. 上市公司反应

据财联社不完全统计，截至发稿，包括岭南股份、名臣健康、因赛集团、首都在线、中科信息、萤石网络、凌云光、芒果超媒、华是科技、东港股份、实丰文化、世纪天鸿、天创时尚、创源股份、巨人网络、会畅通讯、千方科技、北信源、智微智能和掌阅科技在内的 20 家上市公司近两日在互动平台回复 Sora 相关情况。其中，北信源表示，公司与 Open AI 已对接完成文生图的技术研究工作，Sora（文生视频）是 Open AI 最新发布的技术，目前公司正在做技术相关方面对接和研究。

![](img/a5f4ad61462c9afe0139939441795d6a.png)

此外，因赛集团、云从科技、当虹科技、易点天下、汉王科技、博汇科技、海康威视等上市公司还通过其他方式回复 Sora 相关情况。其中，据媒体报道，因赛集团工作人员表示，按计划，公司 AIGC 项目团队将在三月进行文生视频功能的开发，等待时机成熟后投入公测。另据科创板日报报道，当虹科技相关负责人表示，文生视频技术已取得一定进展，在生成视频时长和效果方面，还在逐渐突破。

![](img/2d953f185e8556c57ce7db314ebdda94.png)

### 蓝色光标：预计 2024 年由 AI 驱动的收入有望实现 10 倍以上增

财联社 2 月 22 日，近期，文生视频模型 Sora 引发市场极大关注，蓝色光标 CEO 潘飞在接受财联社记者采访时表示，蓝色光标在去年已经在大量实践用 AI 的方式生成视频，团队已能熟练运用包括 Runway、Pika、Wonder Studio 在内的多种模型，根据内容需要生产 AI 视频。Sora 全面应用后，一定会让视频创作得到新的智能涌现，视频案例也将呈现井喷式发展。公司已有过千个案例（含出海业务）是通过生成式 AI 赋能创造出来的，其中深度驱动并带来规模化收入的案例约在 1/3 以上，AI 驱动的收入在 1 个亿以上。预计 2024 年由 AI 驱动的收入有望实现 10 倍以上的增长。（财联社记者 崔铭）

### 中文在线：公司海量 IP 未来或可运用 Sora 或类似应用生成短剧/短视频

财联社 2 月 21 日，中文在线接受调研时表示，Sora 是 OpenAI 发布的人工智能文生视频大模型。公司拥有海量的文字 IP，以及 IP 衍生开发的成熟商业化链条。从目前公开信息来看，Sora 在相当程度上会进一步提高生产效率、降低制作成本，公司拥有海量的 IP 未来或可运用 sora 或类似应用生成短剧/短视频，通过公司现有成熟短剧或短视频营销等商业路径进行规模化、商业化的变现。

### 华策影视：目前技术储备已可实现 4 秒时长的文生视频

财联社 2 月 21 日，华策影视在互动平台表示，公司高度重视各类 AI 技术的研发和应用，对以 Sora 为代表的新兴 AI 文生视频技术平台保持密切关注。公司去年组建了 AIGC 应用研究院，自研的编剧助手、剧本评估、视频检索、AI 剧照等功能已在内部应用或内测。通过对开源技术的学习吸收和消化，目前技术储备已可实现 4 秒时长的文生视频。公司将积极探索文生视频等新兴 AI 技术在传媒领域的落地应用与业态创新。公司在 AI 领域的技术储备短期内不会直接贡献业绩。

### 国投智能：公司 AI-3300“慧眼”视频图像鉴真工作站可鉴别 Sora 文生视频

财联社 2 月 20 日，有投资者提问国投智能：“公司的 AI-3300 慧眼视屏鉴真工作站能否鉴别 Sora 文生视频，并出鉴定报告？”对此，国投智能在互动平台表示，公司的 AI-3300“慧眼”视频图像鉴真工作站，可以鉴别该类视频，并支持出具鉴定报告文书。国投智能还表示，该产品已经向客户销售。

目前 AI 视频有两个较大的门槛，一是算力的掣肘，视频领域对算力需求更高；另一方面是竞争环境的激烈，当前头部科技巨头基本都已入局，只是产品尚未全面公测。值得注意的是，从 W.A.L.T 模型的例子来看，不能忽视算法的演进对 AI 视频领域的影响，曹旭特建议重点关注：视觉算法领先的虹软科技、当虹科技、格灵深瞳。同时可关注应用侧的金山办公、万兴科技、福昕软件等。

据不完全统计，涉及 AI 视频视觉算法相关业务的上市公司有当虹科技、虹软科技、深科达、格灵深瞳、天准科技、博杰股份等，具体情况如下：

![](img/15aec7ba860520abd4c86a1db0f46f4e.png)

### 3. 国内人工智能 AI 大模型发展

2024 年 2 月 19 日，国内 AI 创业公司北京月之暗面完成新一轮超 10 亿美金融资的信息传出，业界震惊。

这是自 ChatGPT 掀起全球浪潮以来，国内 AI 大模型公司迄今获得的单轮最大金额融资。

投资方包括红杉中国、📕、美团、阿里，月之暗面也因此离跻身全球最顶流 AI 公司更进一步。

![](img/7181a9f99173aa3facd2b033530025ef.png)

## Sora 团队信息资料

Sora 模型背后的开发团队备受瞩目，团队成立不到 1 年，核心团队 15 人，包括来自伯克利的研发负责人 Tim Brooks 和 Bill Peebles，团队亦不乏华人身影。

### 1、团队成员

公开资料显示，两位研发负责人 Tim Brooks 和 Bill Peebles 均博士毕业于伯克利人工智能研究所，导师同为 Alyosha Efros。

Tim Brooks 在伯克利期间发明了 InstructPix2Pix，曾在谷歌从事为 Pixel 手机摄像头提供支持的人工智能工作，并在英伟达从事视频生成模型的工作。同时，他也是 OpenAI 文生图模型 DALL·E 的主要研究员。

![](img/584107112dff0ebc662f07fc5c602b3a.png)

Tim Brooks

除了技术，Tim Brooks 的摄影作品曾获得国家地理、自然的最佳摄影和国家野生动物联合会的奖项。此外还在纽约百老汇的灯塔剧院演出过，还曾获得 Beatbox 的国际奖项。

他表示：“我对人工智能充满热情，幸运的是，它与我对摄影、电影和音乐的热爱融为一体。”

William （Bill） Peebles（Bill 系 William 昵称）本科毕业于麻省理工学院，师从 Antonio Torralba，曾在 Adobe 和英伟达有过实习经历。

![](img/829ae8fea6337b8ac0aa2d98eed16c7a.png)

而在开发团队里面，不仅仅是 00 后不少，还有包括北大校友在内的几位华人研究员，以及 20 岁就加入 OpenAI 的天才工程师。

![](img/9469141b71e0e2fa371c994408c80ec5.png)

其中 Li Jing 是 DALL-E 3 的共同一作，2014 年本科毕业于北京大学物理系，2019 年获得 MIT 物理学博士学位。在 Meta 做了 2 年多博士后之后，Li Jing 于 2022 年加入 OpenAI。

![](img/f197995c4b0c7772a8c372f402a9d2c5.png)

2010 年，靖礼代表中国队参加第四十一届国际奥林匹克物理竞赛斩获金牌，并与其他三位金牌选手（俞颐超，吴俊东，张涌良）一起保送至北京大学（除吴俊东入读信息学院外，其余均被物理学院录取）。2014 年本科毕业后，赴麻省理工学院攻读物理学博士学位。

![](img/28bfd63ca090e3a797abfa7d99ceb818.png)

据悉，靖礼曾在 Meta 工作两年，并于 2022 年 10 月加入 Open AI 从事研究工作，研究领域包括多模态学习和生成模型，曾参与 DALL·E 3 的开发。2019 年，靖礼以 Lightelligence 联合创始人的身份，入选 2019 福布斯中国 30 位 30 岁以下精英榜。

![](img/18b6b692f3652cda91db9dd65b4d7110.png)

![](img/11b4551ac4b38493c06dfa7de70ac213.png)

华人作者中还有 Ricky Wang，今年一月刚刚从 Meta/Instagram 跳槽到 OpenAI。

**延展阅读：**[解密 Sora 13 人团队：北大校友在内 3](https://mp.weixin.qq.com/s/2kxcmTTYrmmiSs9gl1NFzQ) 名华人，应届博士带着 21 岁天才少年

### 2、谢赛宁辟谣

而之前有部分自媒体传的“Sora 作者之一”的上交大校友谢赛宁，也发文紧急辟谣。

![](img/68ad162201caaa4206995c1b9b437327.png)

虽然谢赛宁并非 Sora 的研发团队成员，不过 Sora 技术报告中引用的一项研究成果 Dit 模型，正是谢赛宁与 Bill Peebles 合著。

从这个辟谣的细节也可以看出 Sora 开放领队 Bill Peebles 的团队工作状态，基本不睡觉高强度工作了近一年，这也从侧面否定了之前传闻猜测“Sora 早就开发完成”，Sora 是 OpenAI 实打实的最新研究成果。

只不过 Sam Altman 特地挑 Gemini 1.5 Pro 发布后 2 小时，公布 Sora，这绝对是进行策划的。

要知道本次 Gemini 1.5 Pro 的文本长度几乎无限（最多可达 1 千万 tokens，付费用户可以使用 100K 的 Tokens），这绝对是非常强大的功能。

![](img/dc4b7ed948207369c69f52219e37f259.png)

本以为有如此王炸级别的更新，Google 志得意满想要收获热搜，没想到才过几小时，Sora 不可思议的视频进步，占据了整个社交媒体的热搜。

这种情况，很难认为这是碰巧。

![](img/f24ccd7f8a992d357820568988db4a85.png)

事实上，Sam 精于此道，据说 ChatGpt 的火爆，正是他得知竞争对手 Anthropic 准备推出 Chatbot，他敏锐意识到这一类人工智能聊天工具的趋势，于是让所有的员工都停下手头工作，整合之前的研究成果，在 2 周内迅速调整并赶出了 ChatGPT 3 并进行发布，迅速占领了舆论并促成了 ChatGPT 的火爆。

### 3、Open AI 的工作状态

而这一群优秀的科学家和工程师，他们的工作状态也是十分拼的，优秀的人会按照自己的意愿行事，下面是推特上一位 OPENAI 技术人员自述一天的状态：

![](img/2ec457b5eded0aa57d9fadc03611a026.png)

我们先来看下 Jason Wei 的作息时间：

[9:00am] 起床；
[9:30am] 搭乘 Waymo 前往 Mission SF，途中在 Tartine 买个牛油果吐司；
[9:45am] 背诵 OpenAI 章程，向优化之神祈祷，学习《The Bitter Lession》；
[10:00am] 在 Google Meet 上开会，讨论如何在更多数据上训练更大的模型；
[11:00am] 敲代码，在更多数据上训练更大的模型；
[12:00pm] 去食堂吃午饭（素食，无麸质）；
[1:00pm] 真正开始在大量数据上训练大模型；
[2:00pm] 调试基础设施问题；
[3:00pm] 监控模型训练进度，顺便玩一下 Sora；
[4:00pm] 对前面提到的训练的模型进行提示工程；
[4:30pm] 短暂休息，坐在牛油果椅子上，思考 Gemini Ultra 的性能到底有多强；
[5:00pm] 头脑风暴，思考改进模型的潜在算法；
[5:05pm] 得出结论：修改算法风险太高，只扩展计算和数据更安全；
[6:00pm] 吃完饭；
[7:00pm] 通勤回家；
[8:00pm] 喝一杯酒，继续写代码，鲍尔默峰即将到来；
[9:00pm] 分析实验结果；
[10:00pm] 启动实验，让其运行一晚上，第二天查看结果；
[1:00am] 实验真正开始运行；
[1:15am] 上床睡觉；在纳德拉和黄仁勋的守护下入梦。压缩才是关键。晚安

单从时间来看，Jason Wei 从起床到睡觉，横跨了 16 个小时多，除了吃饭、通勤、睡觉，都在说工作，称其“卷”并不过分。

## 二、技术拆解

### 官方技术报告文（中英文对照）

![](img/f7eff733facf6810d65ac450d47bc0c3.png)

[OPENAI-SORA+技术文档总结+中英对照原稿.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FWnzbnCEaotgSjxIzLxcoSBHnue)

Sora 模型基于 Transformer 架构，建立在 DALL·E 3 和 GPT 模型之上，使用文本到视频的合成技术，按文本提示生成视频。这种技术将自然语言转换为视觉表示形式——图像或视频。

![](img/12ba356a4adcf5a74a35dca5b135501f.png)

![](img/217c38825c9e6e806caba4840e6bfe7f.png)

OpenAI 在官网展示 Sora 的生成逻辑

深度神经网络依然是 Sora 的基础，它是一个带有 Transformer 骨架的扩散（Diffusion）模型，AI 从数据中学习并执行复杂的任务。Sora 就是从“学习”的大型视频数据集里学会了各种风格、主题和流派。

Transformer 模型本质是一个编码器-解码器，输入原始语言，生成目标语言。扩散模型的原理是先给数据添加高斯噪声，再反向去除，从中恢复数据本貌。

简单粗暴地理解 Sora 的原理，就是翻译器+搜索引擎+概率制作（内容）。

首先，拿到提示词后，Sora 先分析文本，提取关键字，比如主题、动作、地点、时间和情绪，再从它的数据集里搜索与关键字匹配的、最合适的视频。

其次，Sora 将数据集里合适的视频混合在一起，重新创建一个符合要求的视频。在“创造”的过程中，它要“知道”场景中有哪些对象和角色，它们的外形，它们如何运动，对象如何交互，以及受到环境影响后如何表现。

根据用户的喜好，Sora 会修改视频的风格。假如用户想要一个 35 毫米胶片样式的视频，Sora 会调整效果，更改图像的亮度、色彩和摄像机角度。这一点和 Midjourney 等“文生图”应用类似。

Sora 可以生成分辨率 1920x1080 的视频，也可以基于静止图片创建视频，使用新素材扩展现有素材。比如用户给它一张森林图片，它可以帮你加上鸟、兽、人。给它一张汽车行驶图，它能加上道路、交通灯、沿途建筑物和风景。

![](img/0da322865fe3cb92aa3c0bea03ed1e41.png)

Sora 将两段视频结合后产生的奇幻景象/OpenAI

### SORA 的超详细解读文

最强文生视频模型 SORA 超详细解读

### 腾讯科技拆解文

OpenAI 王炸模型引爆科技圈，我们第一时间深读了官方技术报告

### 阿里巴巴分析文

复刻 Sora 有多难？一张图带你读懂 Sora 的技术路径

[一文看 Sora 技术推演](https://mp.weixin.qq.com/s/3RnrO7fSMizEl3mN3SXG5w)

### 素人笔记

Sora 是怎么训练出来的 | 笔记

万字长文 | Sora 技术解析报告

### 技术解读（思维导图版）

![](img/24d6ad579d431721d37bc6ea0f2f43e5.png)

### 参考论文

[Sora 参考论文](https://huggingface.co/collections/pxiaoer/sora-65d0e2db17e2b305e0fc572e)



# 三、研究报告

## 券商报告

![](img/b892a30d6c8dc802c4f5a0ae0f0df52f.png)

![](img/4d31808d673fa4225000ecc26c0200f0.png)

[传媒互联网行业点评：Sora横空出世，关注内容资产价值重估下的两条投资主线.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/R5PYbV8m7oDvVXxsDvOcMGPznyb)

![](img/4d1d1f4c3193dc90b256a481f29e58da.png)

[计算机行业点评报告：Sora重磅发布，AI产业迈入新阶段.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OLQZbuX7vo3Lobxr1lRc7Dr0nRc)

![](img/87bc692bd5556c7ce40414ef00282066.png)

[全球科技行业周报：OpenAI发布首个AI视频模型Sora，影视、旅游及本地消费高景气.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Yd7lbYBIEoh8QfxaK9wcoG9LnZf)

![](img/7a89e1c1f9afce8f7a860b2dc7318e9e.png)

[【万联证券】计算机行业跟踪报告：OpenAI推出首个文生视频大模型Sora，引领AI文生视频行业跨越式发展.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FIaobA1vioyA1ZxvTDZcwbfJndg)

![](img/1a195c5dd1793e245918a038b5bedb38.png)

[【国信证券】多模态AI大模型点评：OpenAI发布首款文生视频大模型Sora，训练算力需求大幅提升.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FKTbbb5hBo7Ki8xPYH5cJa0Pndc)

![](img/36b3dc1371dc33038ab2c3d7c6546507.png)

[【国投证券】行业周报：文生视频模型Sora有望引领AI新景气领先大市-A.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YDCXbUqDSojuX5xw4uMczpFMnJd)

![](img/36af3450137925fa80fafa4ab1ab799a.png)

[【天风证券】通信行业周报：谷歌发布Gemini+Pro1.5模型，OpenAI视频生成“Sora”重磅登场.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LkjIbUbWdo1X4kx6pPbcMWwunxe)

![](img/060e3b9166adb5cbd3d9360e82c8b117.png)

[【华创证券】计算机行业重大事项点评：视频模型Sora发布，或为AGI重要里程碑.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AVzNb6yqloYfLVxUv9mcoJwHndd)

![](img/ea8320b8bbf0dd7bbe84dbe7790275b7.png)

[【源达信息】人工智能专题研究系列四：OpenAI发布Sora文生视频模型，AI行业持续高速发展.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FyssbGC57o9kcmxvA6Wcc1FVn7g)

![](img/f617f7b9a5b48fc35febc49244e51a30.png)

[【国海证券】点评报告：Sora重磅来袭，万兴天幕拥抱AI视频新时代.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JOkdbB4ISoD7rGxk63Mczaz9nP0)

![](img/3d50da92afe6baf05df2d6e365f8133a.png)

[【天风证券】传媒行业第7周观点：OpenAI发布首个文生视频模型Sora，春节档票房再创新高.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/T5sVb5DpwoqXyMxYJakcqLXLnyg)

![](img/8ca07b837ce235364470d894b3494e2a.png)

[【华安证券】全球科技行业周报：OpenAI发布首个AI视频模型Sora，影视、旅游及本地消费高景气.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Jk9ZbrLWqosWZ9x3ujjc2aWEnIh)

![](img/4c3f5ca16a5d8bf388598319dfe5a787.png)

[【中国银河】计算机行业：OpenAI发布文生视频模型Sora，加速迈进AGI时代.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CbMzbJrG3ocEgexeFh0cfrbqnBg)

![](img/f0e9f8eadcc9f471cc97802556d57db4.png)

[【中泰证券】Sora重磅发布，AI生成视频迎巨大创新.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TqFwb6ytuo8nLbxLhRhcyPWAnge)

![](img/b6f1723e49589471ab8bd0669a9d41b0.png)

[【中泰证券】通信行业周报：海外AI创新涌现，Sora提升算力与带宽需求.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Lyrsb5uNjoCVi3xUmnicUUJVnXu)

![](img/5ddf302e30654f51b5a6feef52593e32.png)

[【中泰证券】传媒行业点评报告：OpenAI发布首款文生视频模型Sora，多模态推动多行业发展可期.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RxdRbEcuzo6UfqxjLpIct0mMn7b)

![](img/af610629ff17059770b5884909471b11.png)

[【太平洋】传媒互联网：Sora横空出世，关注内容资产价值重估下的两条投资主线.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Fcw8bZm6woElvOxMk6fc9mhynPh)

![](img/0c8dddde18a42051a227e2f14dea62f7.png)

[【华福证券】传媒行业动态跟踪：Sora产业链投资机会.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XRmgbdQX8oamtYxgmp4cYFXvnZf)

![](img/63e79bde8a5f708e12df8e4241ec0b91.png)

[【国盛证券】计算机行业点评：Sora惊艳发布，文生视频跨越式突破.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/L7y7bBaZeo8Qtyxk1ifcoMlSnke)

![](img/2eba860e08ce33ef68fa9a6605e99384.png)

[【德邦证券】计算机：Openai发布文生视频模型Sora，AI视频商用不再遥远.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ICTIb2CT1oiapox8odNcczNrn4A)

![](img/1a70c9021770347e6377f31bae2b811e.png)

[【国海证券】计算机行业事件点评：Sora震撼发布，加速AGI时代到来.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Yn0SbVA2YoON6gxcvuScxi0LnRg)

![](img/aed2a01579ec1506d417a5ea68a21fa4.png)

[【国联证券】计算机：Sora实现文生视频跨越式突破，AIGC持续正反馈.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VwZVbLUKNoiQUZxWVzhc2UwxnLb)

![](img/6cdee67967f5a87925ff552df824aa81.png)

[【华鑫证券】传媒行业周报：AI应用Sora有望助推多模态AI热度+2024年春节档收官.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ER82b7xUcoVPPnxO1xwcOeVVnZd)

![](img/e680ed5ef399d93c213b60440ed06778.png)

[【华西证券】AIGC行业深度报告（12）：Sora算力倍增，国产架构+生态崛起.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ll6ebnCZwo2tSgxToWpc3pQHn4b)

![](img/a61add7e9bdd818ab111f5541d273814.png)

[【华鑫证券】传媒行业深度报告：从Sora看AI应用发展探索 新质生产力有望推动TMT再下一城.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PUIab4BKkoeienx2Keic3N5On3b)

## 投资纪要

![](img/aa4144bb4e022d26db172b7b2bc7ca16.png)

[解读Sora最佳受益方向电话会纪要-20240218.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/STKrbosGRoNVMBxokeLcJyjqnGe)

## 行业报告

![](img/436e68f1ea4084ce22d524fa2cc2f9bc.png)

[甲子光年：2024AIGC视频生成：走向AI创生时代.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Bab0bkqvjoKMAkxA4XxcUsH5n8b)

持续更新......

# 四、演示案例

## 视频案例合集

**传送门：**`http://mianfei.chat/sora`

![](img/560f6ee6a961f8defe1683fc4d5c888d.png)

**传送门：**`https://www.aisora.org/`

![](img/0b73ffa45aae8b3102f408c0b1a1a822.png)

**传送门：**`https://sorahub.video/`

每天自动抓取网络上的 Sora 视频，支持下载视频，涵盖视频提示词！

![](img/c3fcbc9602e29a236ff167175cbe8586.png)

## 官方示例

**提示：**几只巨大的毛茸茸的猛犸象踩着白雪皑皑的草地走近，它们长长的毛茸茸的毛毛在风中轻轻吹拂，远处白雪皑皑的树木和壮观的白雪皑皑的山脉，午后的光线与薄薄的云层和远处高高的太阳营造出温暖的光芒，低相机视角令人惊叹，用美丽的摄影捕捉了大型毛茸茸的哺乳动物，景深。

![](img/32d8005e7fdbe5ca2e97772e194d2ec9.png)

[2ad70fa0af3f48790b735ad1f1aeaf76.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Tdb7b8pjFoAHKIx1h5HciXKMnCe)

**提示：**一段精美的自制视频，展示 2056 年尼日利亚拉各斯的人们。用手机摄像头拍摄。

![](img/bb77a2e5cea6cfb92e26be80bdcb0cca.png)

[OpenAI 的 Sora 模型确实牛P  它的确能模拟真实世界，包括学习了摄影师和导演的表达手法：  提示：一段精美的自制视频，展示 2056 年尼日利亚拉各斯的人们。用手机….mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Kd8TbuO0aoRNWQxSOOrcKA73n8d)

**提示：**一位时尚的女人走在东京的街道上，街道上到处都是温暖的发光霓虹灯和动画城市标志。她身穿黑色皮夹克，红色长裙，黑色靴子，背着一个黑色钱包。她戴着墨镜，涂着红色口红。她自信而随意地走路。街道潮湿而反光，营造出五颜六色的灯光的镜面效果。许多行人四处走动。

![](img/d959e93e59906caca89c16ca2e4a1475.png)

[演示.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Of4LbYuQLoS9ZUxTuVZcrxLUnTV)

**提示词：**一只蚂蚁在蚁巢内部移动的视角镜头

![](img/aa556a59f6645bb9d7882117530e9f1f.png)

[WeChat_20240218175600.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UZu7bDXWzoYPQkxpPN3cwLDCnsd)

**提示词：**一座巨大的教堂里全是猫。到处都是猫一个男人走进大教堂，向坐在宝座上的巨型猫王鞠躬

![](img/e0a0c9d58f9648ab884e68e8d0f9fa4e.png)

[案例1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/X8RCbieduoBZDvxCdsNcQM3Cn8c)

**提示词：**一段逼真的蝴蝶视频，它可以在水下游过美丽的珊瑚礁

![](img/5b929c0729fedc1f12e31b73e65f4784.png)

[WeChat_20240218211204.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PN17bjgnrolXAFxfXOycEzswn4b)

**提示词：**一片叶子的宏观照片，显示微小的火车在其静脉中移动

![](img/39be2d17991b98ea6ea47206924d8ed1.png)

[WeChat_20240218211217.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ox3BbJsEvoQ0aVxUthPcLpdNndH)

## 提示词三方研究人员

Sora 研究人员 Bill Peebles 发布视频展示：Sora 能够同时生成多个视频，这些视频并排显示，提供了不同的视角。

在他分享的示例中，一个单独的视频里就包含了五个不同的视角，这显示了 Sora 的先进技术能力，它能够在不需要人工编辑的情况下，自主决定展示多个视角。

![](img/3105ff77fbe91ee337d8c2032bddd0ba.png)



# Sora 研究与演示

[Sora研究人员Bill Peebles 发布视频展示：Sora 能够同时生成多个视频，这些视频并排显示，提供了不同的视角。在他分享的示例中，一个单独的视频里就包含了五个….mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BTubbhMNZoiRcFxPRZ9cxc9CnCb)

## 磨铁文化归纳

[Sora Demo视频](https://xiron.feishu.cn/base/TJnebiI1Va1NpqssjJKc71ISnJf?table=tblI6BB9euPRujUG&view=vewXK2FL7K)

## Sora 与 AI 合成音效

@ elevenlabsio 的合成音效服务给 Sora 生成的视频自动配了音，原始通过提示词＋ Video Pixel 识别！我们离 AI 为我们生成的沉浸式世界的未来越来越近

![](img/1a12fee428b9a0851f7f3232ccdd3989.png)

[WeChat_20240225122359.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HCRSbNRTooz2qSxLsxScs5xJnNb)

## OpenAI 官方 TikTok

### 2 月 24 日合集

![](img/479dfc8f96820f2521a68b2971d2dba6.png)

[WeChat_20240225122522.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Mv5AbK4pwoNjE5xhV2TcjgsmniR)

提示词：“一只乌龟的身体是由玻璃制成的，裂纹已用 kintsugi 修复，正在日落时的黑沙滩上行走”

![](img/93a1df2737b9325898e263e12a98fe1e.png)

[WeChat_20240224100229.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UUUHbK9suo2oXKxBOGVc0NiXnte)

### 2 月 21 日

最新 Sora 视频，由 OpenAI TikTok 号发布。

![](img/2dc966e4ff25debf7ea7c2e629c9993f.png)

[WeChat_20240225122347.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/L6MPbYpYEo8CGYxdgHscBNlBnAg)

提示词：一只穿着黑色连帽运动衫的电脑黑客拉布拉多猎犬坐在电脑前，当他快速打字时，屏幕的眩光照射在狗的脸上。

![](img/88b4b1b72f7a8f5adfed71ac7a921915.png)

[7a654892637bcfcc306afd5a995984ed.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HbxybGPDXoK6l4xtWvgcWnN3nHd)

提示词：低质量、视觉效果令人失望的超级碗广告

![](img/726a51253c5d43563104dd7dc7990952.png)

[453c235c4b0275a5701af00e55205671.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RnLxbVIMpo1swTxjSsKcscYqnuh)

提示词：披萨做的匹萨斜塔

![](img/aab0d1fa8f122a74b0e0612947d07e9f.png)

[e8d90be3a652d264c05dfef87472b5cf.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Zfvkb7DwYoLenExO90PcbzB0nCP)

### 2 月 20 日

提示：“低至地面的摄像机密切跟踪丛林中的蚂蚁，深入地下进入它们的世界”

![](img/09a56ab63a3ba2f36eff192a0cf90081.png)

[f697a4812a0db9e28911b4962f16ff43.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RmUNbmPG5oymEax852NchRCNnQ9)

Sora 可以根据文本创建逼真且富有想象力的视频，但 Sora 也可以带来静态图像，例如这些创作，走向生活。

提示 1：“小土豆国王戴着雄伟的王冠，坐在王座上，监督着他们广阔的土豆王国，里面充满了土豆臣民和土豆城堡。”

提示 2：“咖啡馆的小地图立体模型，装饰着室内植物。木梁在上方纵横交错，冷萃咖啡站里摆满了小瓶子和玻璃杯。”

提示 3：“一张写有“SORA”的写实云朵图像。”

![](img/098e97b0fd4290d30593b8ff735e019e.png)

[335068c586656c8231c5f3ea1edcbd12.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/B021b7hPKoXz91xK9TnciM25nDc)

### Sora 震撼视频合集剪辑合成

![](img/3aa01f09719cdda73e0c31ca6437bb1f.png)

[WeChat_20240225123243.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RQ7EbdCfSopoNcx3VIZcn3MjnKc)

# 五、赚钱方法

## 1、搞流量

流量=钱，AI 领域新产品的出现总会伴随着所有人的关注，Sora 在视频生成领域的出现不亚于大语言模型的 GPT-4.0，此时的流量生意总是能最先拿到结果的生意，现在大把的 Sora 👸号爆文，爆款短视频，用来获取流量非常可观，咱们先不管是捧还是骂，话题度在。

## 2、卖账号

目前的 Sora 模型还没有对普通用户开放，不过大家都已经迫不及待了。就像当年的 ChatGPT 一样，OpenAI 的账号销售一定能继续大卖。只是国内的大语言模型和国外的相比，许多普通人并不能进行完整地分辨，等到文生图的时候，差距逐渐显现，等到真正 Sora 对普通用户开放的时候，国内外的差距就比较明显了。这个时候，会有更多的人想用上 Sora。

## 3、卖课程

课程永远是 AI 行业不缺的赚钱渠道，目前 AI 行业做课程赚的最多的当属李一舟，据说年入 5000 多万，而 Sora 还没开放账号，已经有课程上线了，这就是“搞钱”的原动力！AI 工具使用的课程会长期成为 AI 行业的赚钱赛道，也算是先行者的付费布道，课程要在有产品以后发布。

![](img/d6125a646ce0b1789b78d7ed79d706b2.png)

**友情提示：仅示例，请勿购买**

## 4、套壳

众所周知的原因，国内乃至全国的套壳网站和应用众多，只要你有一定的流量或者推广渠道，套壳网站会为你持续带来收益。目前 Sora 的推出，已经有人开始通过套壳网站赚到第一笔年费了。

![](img/a74b545acd2663fb9ec9926ba320b145.png)

目前的套路是注册含有关键词 sora 的域名，同时应用商店提交 Sora 相关的 app，同时接入 Runway 等产品的 API，先把流量跑出来，等到 OpenAI 发布 Sora API 的时候，再进行切换，从而丝滑过渡。

## 5、投资

如果你是二级市场投资客，那么关注 AI 行业一定能让你赚到（中国股市可能除外），Sora 的推出，利空哪家公司呢？

Adobe!

受 Sora 发布的影响，Adobe 2 月 16 日当日跌幅达到 7.41%。如果此前做空 Adobe，那是要赚上一笔了。

![](img/4e0d761994db6d26d4a71eb3c9be8ec1.png)

## 6、提示词交易

去年的 AI 绘画提示词交易很火，今年 AI 文生视频的提示词一定也会有市场，从提示词打包交易到提示词交易平台。

日光之下并不新事。

## 7、剧本创作

文生视频提示词的升级是视频的剧本创作，无论是长视频还是短视频，更依赖剧本的创作，这更依赖于创作者。

从这个角度来看，Sora 的发布是非常利好内容创作者。如果把未来的视频创作比如电影创作的话，那么编剧的权重在进一步强化，或者是编剧、导演和制片在进一步融合。大团队面临挑战，而小团队的机会在进一步上升。

## 8、视频制作

视频生成这个业务勿庸多言，Sora 就是干这个的，大家只要充分地学习和试错，做出更好的视频（质量和数量），赚钱的机会一定有。在 AI 绘画时代已经切入的团队和个体会更有机会，毕竟前期的铺垫已经足够了，学习成本很低。

## 9、原业务优化

AI 工具对生产力的提升是毋庸置疑的，如何直接用 AI 工具进行商业变现，不如考虑在原有的业务上进行 AI 化。

商业的竞争说到底还是生产率的竞争，成本打下来了，自然就有价格调整的空间了，也就有更大的获客和竞争优势了。只要是涉及视频类的公司（我说的是“涉及”，这与 08 部分并不一致），都应该重视 Sora 的使用，考虑如何将其融入进原业务。

## 10、企业服务

部分做传统行业的公司并没有自我 AI 化的能力，那么就可以有相关的 AI 团队来进行咨询服务，从做事的本质上来讲有点像以前的咨询公司，只是这样的咨询公司对 AI 理解和使用更深入，同时这样的咨询公司一定是高度关注和践行 RPA（机器人流程自动化）。

除此以外，企业服务的另一种商业化形式就是商业化的软件工具，在行业领域内融合专业知识和生产流程，再利用像 Sora 这样的 API 实现相应的生产功能，最终商业变现。

## 11. Sora+电商

用 Sora 生成各类电商产品宣传视频投流，或者制作各类短视频，嵌入产品进行推广，参考 AI 模特变装、虚拟试衣等。

## 12. 开发与 Sora 相关的网站或工具

针对具备技术背景的人士，开发与 Sora 相关的网站或工具，如导航网站或 AI 写真工具，以此获取收益，当然也可以储备 Sora 相关的域名。

# 六、名人观点

## 360 董事长周鸿祎

![](img/cd88db4e1a789b4d7de7aecfa46ca0ee.png)

![](img/4faf0257955893fd95f3dd200fd45217.png)

![](img/d57b7051ac056421feeb15db615c5bb6.png)

## 中央电视台台长慎海雄

![](img/7bb6408f4bc7e29f7173bbecdea2b612.png)

## 媒体人胡锡进

![](img/be5dcfb42d0d19276e40ff885951acc2.png)

## 特斯拉 CEO 马斯克

![](img/2ac7669ec1129b31946099dba9da9b3f.png)

## Runway 联合创始人兼 CEO Cristóbal Valenzuela

感慨后浪拍前浪，以前需要花费一年的进展，变成了几个月就能实现，又变成了几天、几小时。

![](img/24c9ef19e70f62956f09a7e58cd527d7.png)

## 出门问问 CEO 李志飞

![](img/8899c0233dd5114cff840830e7c2dcbe.png)

## 前阿里副总裁贾扬清

评价简单粗暴：“真的非常牛。”他认为“对标 OpenAI 的公司有一波被其他大厂 fomo（害怕错过）收购的机会”，“开源还需要一段时间才能 catch up”，算法小厂“要不就算法上媲美 OpenAI，要不就垂直领域深耕应用，要不就开源”，并预言“infra 的需求继续会猛增”。

# 七、相关报道

## 商业博主刘润

刘润：OpenAI 全新发布的 Sora，到底意味着什么？

## AI 博主卡兹克

OpenAI 全新发布文生视频模型 Sora - 现实，不存在了

## 量子位

[效果炸裂！OpenAI 首个视频生成模型发布](https://mp.weixin.qq.com/s/K6ZXhH6xHsw0Nfj5TnKCXw) 1 分钟流畅高清，网友：整个行业 RIP

## 钛媒体

许四清：Sora 进一步拉大了中美 AI 差距

## 新智元

[OpenAI 首个 AI 视频模型炸裂登场，彻底端掉行业饭碗](https://mp.weixin.qq.com/s/93z4Ta91yLv7PB1pnBM9mg)！60 秒一镜到底惊人，世界模型真来了？

## 清华大学

OpenAI 发布文生视频模型 Sora，AIGC 视频时代正加速到来

## 央视网

OpenAI 首个文生视频模型亮相，你觉得咋样？

## 新华社

Sora 来了，会砸掉谁的饭碗？

## CSDN

OpenAI 大杀四方，Sora 彻底革了视频的命

# 八、个人思考

> 大国玩 AI 一年多，几个心得体会分享给大家：
>
> 1，AI 不是风口，是未来深入到生活，工作，乃至思考的必备工具。
>
> 2，AI 的技术迭代日新月异，你可以怀疑当前的缺陷，但你不能怀疑 AI 不断智能的趋势。
>
> 3，AI 进化速度太快，其实会有人为干预，你无法想象 AI 来的太快，社会还没反应过来，政府要面临怎样的压力。
>
> 4，普通人多研究 AI 应用，把 AI 当做自己的思考一部分，这样才能保持不被淘汰。
>
> 5，不要过度恐慌 AI 带来的变革，但是容易被替代的岗位也不要太乐观。
>
> 6，普通人完全可以参与 AI 带来的机遇，也许你不懂技术，但是你可以认真研究应用场景，甚至探索新的应用场景，给更多的人做示范案例。
>
> 7，多看看国外的 AI 技术变革，国内一直在追国外的技术，其实差距还不止一点点，每次革命性的 AI 节点都是国外技术推动的。
>
> 8，在 AI 赛道创业，明显的是出海的创业项目增多了，这是很好的一个选择，海外市场比国内大得多，这是少有的国内国外一致进行的技术革命。
>
> 9，国内的各大内容平台，在迎合 AI 生产内容，甚至是不得不接受 AI 生产内容，因为这是趋势。
>
> 10，谁掌握了先进的生产工具，谁就是金字塔顶尖的那批人，这就是 AI 带来的机遇。

# 附、延伸学习

**AI 相关学习内容拓展：**

[Claude 学习手册（持续更新 ing...）](https://yunyinghui.feishu.cn/wiki/DMiGwm3wmixK1lkZe44clpXynbf?from=from_copylink)

[大厂AI实战应用案例库](https://yunyinghui.feishu.cn/wiki/L9Ppw5Qb1ine1wkzpCXcmH1onOg?from=from_copylink)

[GPT常见问题学习手册](https://yunyinghui.feishu.cn/docx/OpRwdhea3oKTaaxnIhicbcbMnwb)

---

# AI 学习交流社群

1.持续关注并更新 AI 的**相关资讯动态，研究报告，赚钱案例**，常见问题，永久免费在线查看，无收费内容，目标是成为最大的免费 AI 学习社群。

2.已建立社群聊聊 **AI** 发展及应用，**没进群的可以扫码加入，群满可加 AI 研究爱好者大国🛰️ ID：zyg90s（备注：AI）**，已经在社群内的无需重复加入，开源知识库及社群学习全部为公开免费分享。

3.**关于文档负责人大国**：高帆文化 CEO，曾就职于教培大厂，目前创业，主攻研究 AI 落地应用、企业流量增长及达人 IP 孵化。联合作者德里克文，知乎 AI 领域大 V，同济大学毕业。

**飞书 AI 视频学习交流群**

![](img/4718389bba88da58217eb84340d97a92.png)

好友（ID：zhangyuguo94s）

**扫码加🛰️入🛰️群**

![](img/0eed71408c97bd7f31baf7415cfd4c10.png)

**🛰️扫码加好友**

（🛰️ ID：derekdevin）

欢迎扫码了解更多 Sora 参考案例

**觉得有用欢迎留个赞**
