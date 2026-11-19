
# 开源AI视频工具，你只需要当导演，HuggingFace工程师打造

> 来源：[https://agijuejin.feishu.cn/docx/OuYkdFdaAoar0ZxrX5ncHSHwn2g](https://agijuejin.feishu.cn/docx/OuYkdFdaAoar0ZxrX5ncHSHwn2g)

> 编辑：泽南、佳琪 | 机器之心

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/ULWoggZpFcyyVp9ViFLPQg)qq.com/s/ULWoggZp...

> 用 Clapper 做视频，你只需要当导演就行了。

Sora 一出，视频领域似乎已经进入了生成式 AI 时代。不过直到今天，我们仍然没有用上 OpenAI 的官方视频生成工具，等不及的人们已经开始寻找其他的方法。

最近几周，一款开源的视频编辑工具 Clapper 引发了人们的关注。

![](img/e66b90453e47886ccfc426adc41f7902.png)

与很多科技公司提供的视频生成器不同，Clapper 是一款开源 AI 故事可视化工具，于一年前推出原型。它的设计目的并非取代传统视频编辑器，或使用 3D 场景作为输入的现代 AI 编辑器。

Clapper 的理念是集合各类生成式 AI 技术，让任何人都能通过交互式、迭代和直观的过程使用 AI 创建视频。无需外部工具、电影制作或 AI 工程技能。在 Clapper 中，你无需直接编辑视频和音频文件序列，而是通过调整高级、抽象的概念，如角色、位置、天气、时间段、风格等，基于 AI Agent 来迭代你的故事。

Clapper 的作者 Julian Bilcke 是 HuggingFace 的一名 AI 前端工程师。他表示，为了向这个方向继续努力，他还在开发导演模式：目标是让使用者可以全屏播放视频，舒适地坐在导演椅（或沙发）上，向 Agent 大喊命令，让 AI 制作电影。

![](img/ee0443c58655a2a2c2cdb33f1312f78b.png)

最近几天，Julian Bilcke 已经上线了使用大模型将任意文本转换为时间线等新功能。而 Clapper 的热度也水涨船高，在 GitHub 上已经拥有超过 1100 的 Star 量。

![](img/c70b33ba29d609dfae4d986f61b018f9.png)

+ GitHub 链接：https://github.com/jbilcke-hf/clapper
+ HuggingFace 链接：https://huggingface.co/spaces/jbilcke-hf/clapper/tree/main
+ 试用网址：https://clapper.app/

## 如何使用

既然是开源工具，我们主要看的当然是具体好不好用。

还记得 AI 大牛 Karpathy 创作 AI 短视频的体验吗？为了将《傲慢与偏见》开头的三句话变成动画版，足足花了这位顶尖高手一个小时。虽然只有三句话，三个场景，但这个工作流远比三句话要复杂。他先用 Claude 根据原文生成了一系列图像提示词，再将这些提示词输入文生图模型生成对应的图像，再交给视频模型制作动画，配音的任务分配给了 Elevenlabs，最后在 Veed Studio 里把所有片段整合起来。

于是，Karpathy 做完后发推吐槽，称：「创业者们，机会来了！市面上急需一个能够整合、简化这些流程的 AI 工具。」

Clapper 正是一个集成了所有这些功能的一站式平台。

![](img/bdc73e0f1db6d7898a14d2dbcc0509aa.png)

通常想做一条短视频，需要经历以下几个步骤。首先需要一个故事和脚本，然后根据脚本画出分镜，再根据分镜拍摄或寻找素材，将它们在剪辑软件拼合起来，添加动画效果和特效，再有选择性地配上口播、背景音乐或音效。因此，影视制作行业中编导、导演、摄像、剪辑、后期、配音等分工应运而生。

而在 Clapper，视频制作遵循另一条逻辑。它的每条轨道不像 Premier、剪映等剪辑软件一样对应着视频或图片素材，而是对应着一个具体的工种。

![](img/e7f6b5cd029093e73e7ba68644dfef7a.png)

剪映的素材轨道

![](img/191361fa5639a17d99d80e0b23cf2de.png)

Clapper 的轨道

在用 AI 做视频这件事上，我们就是 AI 的甲方。Clapper 就像一个由业内最强的 AI 组成的剧组。Clapper 内置像 GPT-4o，Claude 3.5（Sonnet）等一系列「顶流」大模型。它就像乙方的执行导演一样，负责将你的需求对接给对应的「AI 编导」。

![](img/e4161d112f38a3a7081d7938c4a727f6.png)

从上面的图中可以看出，第一条轨道代表着故事版，和 Clapper 内置的大模型对话，它将通过 API 调取文生图模型，让 AI 分镜老师来生成对应的图片，作为视频画面的基础。

![](img/bb280c52b0f0218ff38a600266105a2e.png)

通过 Clapper 可以访问以上文生图模型

以 Clapper 给出的样片为例，接下来的轨道依次对应场景、旁白、摄像机的视角、背景音乐以及音效。你可以要求 ElevenLabs 或 Fal.ai 为这个西部废土世界的故事生成一些废墟的风声或者枪战的爆炸声。

而 Clapper 还有一项功能，可能真的向「会说话就能拍电影」的愿望迈进了一大步。我们可以直接向 Clapper 导入剧本，并且在「故事」一栏细致地为你的主人公创造人设。

![](img/aa1e28da8226f9b491b7efecf851db7f.png)

以《绿野仙踪》为例，我们不仅可以为人物添加更加个性化的人物描述，还可以上传图片，为女主人公多萝西设置视觉形象。这意味着我们可以请世界上的任何演员来扮演这个角色，哪怕你想看 18 岁的小李子演多萝西，都可以实现。Clapper 的功能细致到你可以调整人物的年龄和音色，每个场景的陈设，多萝西的房间有什么家具，他们冒险的目的地「翡翠城」的房子长什么样子，都可以在 Clapper 中随心调整。

![](img/6466f53102cf9694665716e3594deb40.png)

当然，你也可以先用 AI 画一些气氛图出来，说不定能更进一步地激发你的灵感和创意。

不过，虽然 Clapper 的功能已经充分地考虑了做视频的需求，但是它的效果却有些差强人意。不仅画面中人物的动作有些「鬼畜」，不符合物理运动规律。视频的整体效果更像是会动的 PPT，缺乏镜头之间的转场和连续感，并且配乐也 AI 味十足，听起来没有旋律，还有些杂音。

![](img/4884a5703d40ea7ef66d8420fb012b2f.png)

[0bc3dyaaqaaalmaneqf2rntfahwdbapaacaa.f10002.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DuC1bFbZ3o8DBqxx25sc46dZnBd)

可能生成式 AI 想要改变视频制作的流程，还需要很长时间，但 Clapper 的出现或许能给还在给传统视频剪辑软件拓展 AI 功能的大厂们，提供了一条新的落地思路。

参考内容：

https://news.ycombinator.com/item?id=41221399

https://x.com/aigclink/status/1818111874531205216
