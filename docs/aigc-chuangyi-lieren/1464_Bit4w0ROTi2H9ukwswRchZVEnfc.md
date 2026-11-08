# 20251330-一图胜千言，万物皆能可视化！深究谷歌Nano Banana Pro信息图表现能力

> 来源：[https://aigcreative.feishu.cn/docx/C1hsd4kZ3oHtlaxA70ScoAn9nng](https://aigcreative.feishu.cn/docx/C1hsd4kZ3oHtlaxA70ScoAn9nng)

一图胜千言。

我一直对各种信息的可视化视觉呈现、信息图都非常感兴趣。比如我经常会收集Visual Capitalist的精美可置化图表，学习他们的可视化以及信息的处理方式。

![](img/6f85ec1abe0a1f05c957b063a65cdfd2.png)

![](img/6f012b716f08816b133f862b2a57419e.png)

![](img/168cd485f509c95a8edbf1c82941b66d.png)

早在GPT-4o出现的时候就做过一些探索，但局限于GPT-4o的推理能力与接近于“零”中文表现力，视觉呈现效果不尽如人意，浅尝辄止了，可以看看那个时候生成的效果。

![](img/60abc8d96ced940c105622959196d0f7.png)

![](img/de55a95cf5e795712c160ec441493155.png)

![](img/3974dd64eff6d5cd36700fd8e745144f.png)

（以上是GPT-4o生成的图片）

最近，随着谷歌Nano Banana Pro的震撼发布，想必大家也多多少少看过了，Nano Banana Pro强大的推理能力及生图能力。常规的一些操作基本不在话下就：无论是平面、摄影、创意视觉表现、有趣整活以及其他设计美学方面的表现，都大大的超出了预料，可以说吊打GPT-4o也不为过。

前面也写了几期，做了相应的报道：

最近这几天也在深入的研究Nano Banana Pro在信息可视化信息图方面的表现力，能把这个信息图做好，是很考验一个图像大模型能力的。

Nano Banana Pro不令人失望，真的是每一次出图都被他强大的推理能力所震撼。

我昨天看到一篇👸号文章讲解字节跳动 Agent 实践全景的，内容很丰富全面，原文链接：https://mp.weixin.qq.com/s/ym31A2y45Q2GeN8mOjvOAg。

我想着能不能把它做成信息图：一张PPT、一张单页或者手机里的长图整体呈现一下，于是得到下面的结果：

> 比例：3:1

![](img/aea013474bf089a7abcdbae1cafadb39.png)

> 比例：2.35:1

![](img/a71b805e4d9fdb0728cb45e4e9c1f2cf.png)

> 比例：1:1

![](img/6294f1080b13afba2e8bb1787694d2b3.png)

> 比例：3:4

![](img/c4bfb4aecdd11e9aadb6963d5039fa77.png)

> 比例：1:3

![](img/2f37c853a34b09f5d6aeac7728d99382.png)

![](img/47c7c1be7f6673a072eaefc75dc6b4d0.png)

看到以上结果后我就直接震住了，有关注平面设计或者是了解平面设计的朋友，应该知道上面这一组图的杀伤力，这仅仅只是几分钟生成出来的啊，几分钟就将一份内容，进行了全尺寸的可视化信息图输出：

然后，我在群里了这句话：

![](img/330f5be32d6eb06d1323adab6d638155.png)

> 补充：通过上面几张图，也测试出了一个重要发现：

+   在Gemini里面出图，Nano Banana Pro出图比例极限：1:3~3:1，超出这个比例就直接报错了；

+   在NotebookLM里面，只有三种格式：16:9、1:1、9:16

![](img/d54bda9a62b4653a293a57f11e5d8af6.png)

回到今天我们的主题：**可视化**、**信息图**。

一个很经典的问题：为什么人喜欢看图？

![](img/434fd4a69de9d53d7890283ce2b0bd2e.png)

人脑处理图像的速度比文本快（虽然“快6万倍”是营销伪**科学，但快是**事实），但真正的核心价值在于降低认知负荷。

那干脆直接用一张信息图的优势作为开场：

![](img/08e969a06cc1fc6b4996fb8d6efa035c.png)

废话不多说，下面我们看看Nano Banana Pro如何做出一张不错的信息图：

> 本篇使用Nano Banana Pro的途径：Gemini、NotebookLM，软件详细的操作就不再细说了，前面推文有讲过，不清楚的可以查看往期文章。

> 没有谷歌账号的，也可以到其他的第三方平台（Lovart、TopNow等）使用也都行。

# 01.基础用法：0提示词，1键，万物皆能可视化

我从最简单的用法入手：0提示词，1键。啥也不用设置，一键出图。

![](img/9ec8002cf2402772a72ded30ecc6a349.png)

这是在NotebookLM里面的用法，在NotebookLM接入了Nano Banana Pro后，真是逆天了！在前面的文章也提过《》，还没看过的朋友可以看一下。

经过简单的一键，我们就能得到输入资料的可视化总结，就能得到以下形式还不错的图：

![](img/ab5ff24487766c717bb1e95adaa33b31.png)

![](img/e6a15751e315f724f2c933629c6009a9.png)

![](img/46fb1f9c9f21ed16252a528f56483eb6.png)

![](img/0eca3199979112d3786b2b04bac680a0.png)

虽然说结果已经基本上够用了，但是我个人还不太满意。你可能也是有想法，需要特定风格，我们接下来再看。

# 02.简单的提示词+风格，轻松拿捏信息图

接下来我们需要加点提示词来约束一下，也很简单，不要有心理压力。

得益于Gemini 3及Nano Banana Pro强大的推理能力，对提示词的要求其实已经弱化很多了。

就像平时我们给设计师提需求那样，各方面讲清楚就行了，剩下的全部交给AI。

今天的主题是探讨可视化信息图。

所以有两个很关键的词就是：可视化（Visualization）、信息图（Infographic）

一个简单的提示词框架：**需要处理的内容+可视化&信息图+语言+画面比例****+**风格+内容详细程度+（自己的版权信息）

这样就能够做到很精准的控制了。

很多人会疑问：这样够了吗？

绝对够了，你一定要相信Gemini 3和Nano Banana Pro强大的推理能力。

接着就可以愉快地出图了:

![](img/ae18ee9647bbf9216d629032f92d901a.png)

![](img/bce4398494673ed8b14aaf998b706c9c.png)

![](img/00c920f44bbe1496e2c8c01e9c681e8a.png)

![](img/32a22fbbe8ec51d442503904a9830f27.png)

![](img/910e09457b404dea3f26bb266370f89e.png)

![](img/eb93477af975e99eaeb8e59e29fce2a8.png)

![](img/3cd02a59484ba60cdaf3620ced1ea5a9.png)

![](img/50dc7e92a502c63c53369599cfd1fd12.png)

![](img/1c2b97c1f13a677ec52893ffc3220571.png)

![](img/9257997d72fd4b23a5d039675169c5d1.png)

![](img/98975394dcb5adec99d2b903ea5b0dd9.png)

![](img/eaa7d368680cd503a60d39948f55afba.png)

![](img/74d9e6912c1d7e242703ccb1a4cb9c86.png)

![](img/8c1cef1ca35fc2533a4c84d0bf67269d.png)

![](img/8f32e0b4757b71071e23f961d77431ca.png)

![](img/4bfdfbc14106e5fb5cda713f67770c70.png)

到这里很多朋友表示就办难了，我非专业人士，我也不知道有哪些设计风格啥的。

这个问题一方面就需要大家多去看，多去积累了。你的知识广度决定了AI的上限。

只要是风格都可以去尝试，不限于比如：苹果风、谷歌风、简约风、熊本熊风格、樱桃小丸子风格、乐高风格、变形金刚风格、迪士尼风格、赛博朋克风格、颓废风格、古典主义风格等等等等......大家能想到的都可以去尝试。

下面我也列举了一些常见的主要的信息图风格这张图本身也是信息图，仅供参考:

![](img/6cad7d513ff751877d0404117d034039.png)

# 03.高阶用法：借助AI，自己提炼风格

说人话就是：找参考，抄！

一些很好的设计图片，我们不知道风格也不知道怎么去描述。

那我们可以借助Gemini 3的强大的多模态能力，对它进行识别风格提炼，形成一个稳定的风格提示词。

比如：

我们找来下面4张图，直接丢给Gemini 3：

![](img/f8fc8051498f13c90ad9bb87f43db62d.png)

![](img/232f56d79447e588bec9dd696505b690.png)

可以看到，Gemini 3拆解得非常详细：

![](img/84b2248231d7f248ab9ba986772299fa.png)

![](img/6ded32227abee7221b8dffa3d51c921b.png)

可以提炼得到这种图片的风格为以下风格：

> 2.5D科技感轻拟态（Tech Glassmorphism）风格。

> 关键提示词：视觉上以**高明度的冰川蓝与纯白**为主色调，强调**毛玻璃（Frosted Glass）材质的**磨砂质感**与半透****明层级；通过柔**和的弥散光影与精细的内**发光营造出悬浮的立体空间；图**形语言多采用大圆角与胶囊状几何体，整体呈现出一种轻量化、洁净且具有未来秩序感的B端商务美学。

然后我们用这个风格来出一下图看看：

![](img/26e4092416ff0abd01fbc973d7a08e23.png)

不能说效果百分之百还原，大体风格还是模仿到了。

再来，也是找来两张我们不知道的图：

![](img/4571c7de1e4319f9bf238775bba5f7ed.png)

同样的办法，得到以下关键提示词：

> **暗黑微光玻璃拟态 (Dark Lumine**scent Glassmorphism) 风格：

> 关键提示词：暗黑模式App图标，烟熏玻璃材质，玻璃拟态，内部霓虹发光，生物发光感，次表面散射，半透明哑光黑表面，透出的鲜艳渐变光，极简主义，高对比度，纯黑背景，细微噪点纹理，赛博朋克美学。

然后我们用这个风格来出一下图看看：

![](img/340a26db88e67b52bf256cf12e287fa4.png)

再看看这个风格：

![](img/a3917de48e2e1a476289348634f40ee6.png)

> 悬浮磨砂 · 环境背光拟态 (Floating Frosted Ambient)风格

> 关键提示词：UI组件，胶囊形按钮，毛玻璃材质，强模糊效果，背景模糊滤镜，玻璃背后的发光球体，青色和紫色的环境光，半透明哑光黑，微妙的白色边缘光，悬浮元素，纯黑背景，极简设计，苹果设计风格，平滑渐变。

由于这个原始信息量实在是非常大，小字看有些不清了，理解万岁~~

但整体效果还是到位的！

![](img/b62ada0de8f4a384be818edccd191ad0.png)

# 04.高阶用法：风格杂交、融合

这一点就比较有意思了，可以进行多风格的杂交融合，创造更多奇特的视觉效果。

下面我们来试一试：

### 4.1.瑞士黏土主义 (Swiss Clay)

> =瑞士国际主义风格+粘土风

**Prompt 关键词参考：**Swiss design layout, massive negative space, Helvetica typography, 3D clay render icons, matte finish, soft pastel colors, high order, minimalist.

![](img/00258a756e94a679bb4dacdb1b4d139f.png)

### 4.2 赛博青花 (Cyber Porcelain)

> =赛博朋克风格+传统青花瓷风格

**Prompt 关键词参考：**Complex mechanical engine parts, made of white chinese porcelain, blue willow pattern details, glossy ceramic texture, cracked glaze, white and blue palette, surrealism.

![](img/889b68a3361ae26d85eeb03caf2b191c.png)

### 4.3 等距赛博金刚风” (Isometric Cyber-Former)

> =等距视角风格+赛博朋克风格+变形金刚风格

**Prompt 关键词参考：**Isometric view 、30-degree angle 、Miniature diorama 、Orthographic projection、Floating island 、Transformers aesthetic、Complex mecha structure、Cybertronian architecture、Industrial sci-fi 、Hard surface modeling

![](img/08899de9671de5b9f2efc5942ffb89b4.png)

这个风格确实有点怪异，但是他确实将三种风格做了一些融合。

直接描述的话，你可能描述不出来这种风格，因此可以做一些有趣的探索尝试。

# 05. 结语：AI 时代的“设计权力”转移

看完上面这些惊艳的案例，你可能会感叹：AI 真的要把设计师卷没了。

但在我看来，恰恰相反。

Nano Banana Pro 这样的工具，实际上是把设计的权力，从手交还给了脑。

以前，我们受困于软件操作、排版规则、配色原理，好的想法往往死在执行的路上。

现在，执行的门槛被夷为平地。

当画图不再是难事，**画什么****和****怎么画****就成了新的决胜点。**

正如文中提到的：你的知识广度，决定了 AI 的上限。

工具再强，它也需要一个懂得什么是美、什么是逻辑、什么是有效信息的指挥官。

**AI 可以**帮你生成无数种风格，但选择哪一种最能打动人心，依然取决于你的审美判断力。

所以，不要只满足于做 AI 的操作员。

去提高你的审美，去拓展你的视野，去学习那些经典的艺术流派。

因为在 AI 时代，**审美力，才是唯一的元能力****。**

![](img/c12b47c6b9ab43e60e0c604d03796dcd.png)

最后也用一张信息图来结束今天的分享。

下一期可能会讲会Nano Banana Pro在PPT领域的专业应用，也可能是下下一期...

我们下期见~~