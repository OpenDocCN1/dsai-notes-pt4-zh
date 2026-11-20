
# 面对 Agent 红海，依旧有惊喜｜Kimi OK Computer 实测

> 来源：[https://chatmemo.feishu.cn/docx/CM5HdksDZoekPpxWyxYcQuEAnAb](https://chatmemo.feishu.cn/docx/CM5HdksDZoekPpxWyxYcQuEAnAb)

近日 Kimi 也开始小规模内测一个 Agent 新品，名称在一众 Agent 友商中，很有人文味与自信，叫做「OK Computer」。

![](img/bcb64b8ce36cd956fc4026578270f6df.png)

不同于此前 Kimi 发布的 Researcher 专精 Deep Research 任务，「OK Computer」则是 Kimi 模型的通用 Agent 模式，配备了单独虚拟电脑，支持更多工具调用，能够完成网站开发、PPT 设计、数据可视化分析、深度研究、方案策划等任务。

自然，从 25 年 3 月 Manus 发布以来，我们已经见过了层出不穷的通用 Agent 产品，如 Claude Code、扣子空间、Skywork、Lovable 等等……

阅尽千帆，本文仍想通过一系列典型 Agent 任务实测，为你解析 Kimi OK Computer 的真实水平。

我也会将它的表现，和过往深度体验的 Manus 等顶尖 Agent 进行参照，来看 Kimi 这个，以 90 年代最伟大的摇滚专辑之一命名的 Agent，能否如其名般地带来惊喜。

![](img/202a08ec415389ad2488b99d2cb4f6d9.png)

---

## 📬 写在测试之前

与先前已发布的 Kimi Researcher 一脉相承，OK Computer 与已有通用 Agent 产品不同，继续延续了端到端强化学习的训练路线：模型即 Agent。

为了能够更好地真实探究 OK Computer 的水平，我已将测试设计为三部分：

> 1. **网页应用开发**：从 0-1 设计、开发 `Html` 宝可梦养成与对战游戏，考察 Agent 自主深度规划与长程执行
>
> 2. **复杂数据可视化**：给定数千条表格数据，呈现可视化分析
>
> 3. 以及其他 case 展示，包括绘制 AI PPT、制作个人博客等

并适当选择同类任务下备受用户好评的友商 Agent 进行参照，校准它当下的行业身位。

你可以带着对 Kimi Agent 的好奇，在实测中找到最终答案。

在文末，我也结合了测试结果，向你分享我所理解的，OK Computer 对于 Kimi 的特殊意义。

---

## 👉 真实实测表现

### 1️⃣ 网页应用开发：宝可梦网页游戏

要测试一个 Agent 的极限，比较好的方式就是给它足够复杂、多步骤、开放式的项目。

我选择让 Agent 从 0-1 自主设计 `Html` 版宝可梦养成对战游戏，并选择了擅长构建 Web 应用的 Manus、Lovable 作为参照系，进行共同测试对比。

此题难度在于，除了考验 Agent 的 Coding 能力外，更考验 Agent 自主规划的完整性，以及长程任务中的 推理-执行-反馈 的循环质量。

**测试 Prompt 如下：**

```
我想做一个 Html 版宝可梦养成与对战游戏，能够基于宝可梦游戏的主要玩法，设计一套完整、适合在 Html 游玩的游戏内容，请仔细推敲游戏玩法，使用合适的像素风格宝可梦素材，设计对应游戏 UI 与内容，并完成开发上线。要求实现移动端兼容。
```

我录了一下各个 Agent 生成的网页游戏的效果，你可以自行来看看效果：

1）Kimi - OK Computer：

![](img/a5b3426c0bc9e84b9542e45d76e8618f.png)

[OKC-宝可梦网页.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HbftbZufYoJSGlxY0uyczBe2nsh)

OK Computer 独立设计了一个适配 Web 玩法的全新 UI。生成的游戏系统更为完整，比起 Manus、Lovable，多了大厅、宝可梦图鉴，物品背包界面。

对战系统基本实现，但不能更换上阵宝可梦。另外还能查看宝可梦的详细属性，有丰富的疗伤物品、精灵球等不同物品的管理系统。

不过，虽然规划了区域探索、商店、成就系统的功能入口，但并没有一次完成开发，还是有些遗憾。

应用整体无恶性 bug，作为 one prompt 结果相当完整与稳定，在游戏方案的规划上给出了惊喜。

**2）Manus：**

![](img/f7e1cb0d2a88b87638ed99b74b94da2d.png)

[Manus-宝可梦网页.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KmNibz1DEoehR4xBfWncEI5vnWd)

Manus 作为我的老朋友厂商，表现一如既往的亮眼。虽然首页略微粗糙，但整个游戏的加载过场却是最为接近原版。

另外，Manus 唯一实现了地图探险功能（但地图显示有明显显示 Bug）。

不过你可以看到随着我点击方向键，是能触发出“草丛遇怪”的逻辑。也是唯一一个直接使用了外网宝可梦图像资源，作为游戏贴图的 Agent。

但比起 Kimi，其游戏性功能的实现熟练就少了许多，没有实现背包，也没有宝可梦栏管理等，算是各有侧重。

**3）Lovable：**

![](img/068accddd3be1475cb6dabbe9c8adc5c.png)

[Lovable-宝可梦.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RWDrbIWFAorBKZxTOPLceDEYn9g)

Lovable 在初始界面上给到了最风格化的效果。

但整体实现的玩法实现的是三者最少：仅支持选择宝可梦进行战斗。

且战斗系统，比起其他家，还少了道具使用、逃跑等基础功能。从这次测试来看，Agent 可能存在规划不足，执行轮次过少的问题。

另外，下图为移动端兼容效果对比：

![](img/207488865242b3485764c97622fe5b4c.png)

> **BTW：**关于各家选择的 UI 风格，我倒是觉得不算决定性差异，因为本身也没有在提示词中规定 UI 的风格一定要是像素宝可梦风格。作为 Web 游戏，并非要求 100% 复刻原版游戏，只要 UI 与玩法系统自洽，能稳定运行即可。

**综合列出效果对比如下：**

![](img/519db7c1aa939cfce44397ad8fbfab9e.png)

不难看出 OK Computer 继承了 `K2` 模型的优秀 Coding 能力，在另外两家使用 Claude 模型的情况下，仅凭自有国产模型，网页开发效果不弱下风。

另外，可能是得益于端到端强化学习路线，Kimi 也确实展现出 One prompt 下，极为优秀的 Agent 自主规划与多轮执行能力，以及熟练使用图片生成等工具能力。

### 2️⃣ 复杂数据可视化：洞察数千条 IMDB 剧集数据

在 OK Computer 的宣传视频中，着重宣传了支持海量数据上传，单次处理百万行数据的可视化分析能力。

为了能够测试 Agent 的复杂数据的关联洞察效果，我选择了让 AI 们，来尝试处理来自 IMDb 的数据集，每个数据表包含上千条电影和电视剧数据。

**测试 Prompt 如下：**

```
请基于我提供的5份IMDb数据（涵盖电影、电视剧与电子游戏），交付一份独立的HTML深度可视化分析报告。你的任务是超越数据本身，洞察其背后反映的文化趋势和社会现象，并形成一套完整、深刻的分析结论。请自主设计分析框架，运用具有顶级美感的多维信息图表，构建出逻辑清晰的报告结构和解读，最终呈现一份兼具专业深度与视觉冲击力的作品。
```

共找了 5 份数据集：

![](img/27a3fe01865f60d2c47f1bddd2d0fd92.png)

每一份数据表内容大概是这样的，以独立剧集的最小粒度数据组成：

![](img/30cb94de975c62dcf222af537fab97bc.png)

在接到任务后，Kimi 首先规划了任务执行的环节：

![](img/5c204aed4f5350300bca66d31ad49468.png)

随后，自行编写了 `python` 脚本，对数据进行清洗和处理（嗯，不是直接用 LLM 文本能力读取，有效避免幻觉问题，还是很聪明的）

![](img/17a5680775e9319c4004a42364aabd87.png)

在自主连续运行长达 31 步子任务后，OK Computer 输出了以下可视化数据分析页面：

![](img/889a8ace3e6cfd3c4e8e4cce9ecdd8ab.png)

[OKC-imdba.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Mce0b82ZZoQi94xw7JGcIusFnqb)

能够看出分析维度非常多元。

特别棒的是， OK Computer 能够根据分析数据的类型，自主选择太阳图、雷达图、三维散点图、小提琴图等不同合适的图表，来展项数据趋势。

这一点倒是在用过的 Agent 里，会更具有主动性。

![](img/83ed51ee7ce030bbde0d3f52fbc2a6c0.png)

另外，这是同提示下 Manus 的效果：

![](img/05e3b766297aa717f67e5ba26afe9957.png)

[manus-imdba.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Tck3b8ZOaoUryAxo99fcunt7n7e)

必须声明，Manus 在同类产品中，产出的质量已经是大幅领先。

但仅论可视化的数据呈现，Kimi OK Computer 明显在该类任务中，能够提供更加复杂、多维的效果。

另外，值得一提的是，在测试本轮任务中，我发现不同 Agent 产品对上传文件数量有着不同的控制。

实测中 Kimi、Manus、Skywork 没有限制文件上传数量，而 Minimax Agent、扣子空间 均限制上传数量为 10 个以内。

这项区别也足以影响部分用户的选择。

## 💡 其他的一些 Case

另外也跑了一些零散、但效果都挺不错的 Case，与你分享 Kimi OK Computer 的各项表现：

### 1）PPT 设计，样式美观，高自由度编辑

基于上面的 IMDb 剧集数据，我又新开对话，让其生成主题数据洞察 PPT。

```
阅读附件中的5份IMDb数据（涵盖电影、电视剧与电子游戏），洞察这些数据背后反映的文化趋势与大众品味的变迁，设计一份 20 页的汇报 PPT。
要求：
1. 只允许使用附件中的数据，不得胡编乱造。
2. 尽可能呈现 PPT 的全部要点、关键数据
3. 使用 chart 呈现图表，不要用 AI 图片生成来呈现图表
4. PPT 美观有设计感
```

OK Computer 效果如下：

![](img/bcd8b750774a17d0a993251324c3ec47.png)

整体来说，OK Computer 所输出的 PPT 设计不错，图文并茂，设计符合主题风格。部分元素会超出页面，需要和 Agent 沟通修改要求后，进行一次修复。

**同 Prompt 任务下，对比精通 PPT 的 Skywork 的效果：**

![](img/00755690922b8b44dd9e790ca40af7cd.png)

结论是，从设计角度来看 Kimi 胜出，幻灯片高度更一致，适合演讲呈现；

而内容丰富度 Skywork 则更胜一筹，更适合内部汇报。

强调一个特别好用的点，对于用户真实使用时，OK Computer 还支持了高自由度的编辑，不仅是文本替换，还支持更改格式、添加图片、流程图、新幻灯片，基本与完整 Office 功能等同。这一点在其他友商 Agent 中格外少见。

![](img/0be1fa36d39a8751c7cb2a274d0f0379.png)

导出到本地后，样式与网页 `Html` 版 PPT 基本一致，不会产生常见的排版变形问题。

![](img/98f92488f388ea66776f15ac5aff8552.png)

### 2）个人博客开发：设计效果非常 ok

![](img/110fb58c337fea994fe3a9b0f01f015b.png)

```
请为我的个人博客「一泽的沉思录」创建一个三栏式布局的网站原型。
风格： 我想要一种极简、沉静的黑白风格，字体要优雅。
内容：左栏是导航，包含 首页、文章、关于我；中栏是文章列表；右栏放一张能体现‘思考与科技’的黑白摄影风格的图片，请你帮我生成。
请确保整个网站的视觉风格高度统一。
```

诚然在测试过程中，我还是能观察到 OK Computer 待改进的一些问题。

譬如：暂时还不支持用户接管 `Browser`，所以无法让 Kimi 替自己调研一些需要登录的网站内容；

在给 PPT 配图时，有时也会过度依赖 AI 生图，而不是正确使用网页图表组件，创建数据图。

这些问题我也一一反馈给了在 Kimi 的朋友，期待他们的改进。

但我们回过头来不能忽略的一点是：与全球化 Agent 产品不同，Kimi OK Computer 始终使用的都只是 Kimi `K2` 这款自研 LLM 模型。

然而，不依靠 Claude 的 Coding 能力、不使用 Gemini 的分析规划能力，OK Computer 在众多友商 Agent 之中，依然能展示出其独到的优势，不得不说，在测试过程中，还是令我感到诧异与惊喜，也值得国内用户试用。

## 🎐 写在最后：OK Computer 于 Kimi 的特殊意义

此前，Kimi 早就发布了 Researcher 产品。

> “如果对国外资料检索没有诉求，Kimi Researcher 很可能就是国内研究质量最佳，分析结论常有意外惊喜的 Deep Research 产品”

![](img/b7a5b2c4100add7a6e2352bd2d908dc0.png)

> ⬆️ 余杭水质问题发生时，曾用 Researcher 进行事件分析，体感效果仅次于 Gemini DR，为国内最佳

月之暗面研究员 @冯一尘 此前在知乎，公开分享了内部洞察：

Researcher 研发过程中，Kimi 团队内部对于 Agent 保持了两个关键信念：一是要做“能长思考”的 Agent，二要用端到端强化学习。

他们认为，有别于传统 Agent 的 Workflow 拼装 或 SFT 训练方案， Kimi 通过端到端 RL 所训练的 Agent 模型，能够挣脱“固定提示”的束缚，更加灵活通用，具备更好的主动反思，多轮行动的积极效果。（此文不论两种技术线路的终局优劣，但这条路线下的 Kimi Researcher，也确实在用户群体中备受好评）

时至今日，Kimi 推出的「OK Computer」新 Agent 模式，依然遵循着这些理念：

借助端到端强化学习技术，将 Kimi `K2` 模型训练为原生 Agent，模型学会了 文件系统、浏览器、终端、代码/图片/音频生成 等 20 多种工具的使用方式，自然涌现出「完成一项复杂 Agent 任务」的内在知识。

实测证明，OK Computer 也确实兼具了 `K2` 模型与模型即 Agent 路线的优势，包括：

1. 继承 `K2` 的优秀 Coding 能力：生成具备强美观度的前端产物（网页、PPT 等）

1. One prompt 下更好的规划执行：发挥端到端强化学习的优势，在单一提示下，实现更多、更全面的推理，执行轮次更多、报错更少

1. **丰富的工具能力：**图片、音频生成；浏览器使用；使用虚拟电脑作为响应环境等

但，如果仅仅将这三点看作孤立的优势，我们就错过了 OK Computer 真正想讲述的故事。

这个故事的核心，是 Kimi 对模型智能的又一次扩展：

从文本模型，扩展为能「拆解任务目标、规划长程任务、通过行动-反馈达成更优结果」的全能型 Agent。

又名——模型即 Agent。

而对于我们用户，尤其是非技术的普通 AI 使用者而言，

则是在国内，多了一个在轻量网页开发、PPT 设计、数据分析等复杂任务中，表现都相当不错的，全新的全栈 AI 助理。

Ref：冯一尘，知乎下的 Kimi Researcher 回答：https://www.zhihu.com/question/1919712376204256921/answer/1920925901035644513

<https://chatmemo.feishu.cn/sync/OXhxdlqqcsuEkObLUgsceuRinef>
