# AI制作绘本故事

> 来源：[https://docs.feishu.cn/docx/EAZtdkNI8oYEw7xAoPRcD8uynre](https://docs.feishu.cn/docx/EAZtdkNI8oYEw7xAoPRcD8uynre)

# 小镇美食家

> https://mp.weixin.qq.com/s/DnJ4yKFbJd5hnAIHpiVvXQ

面对AI工具的强大创作能力，设计师一直担心工作被AI所取代，然而AI真的会取代设计师吗？本篇文章从比较简单的绘本故事下手，整理一套全流程AI创作的方法。

![](img/1a79f082a36db88a0cff94f4c6a782f2.png)

![](img/9c5637fcb6bf053bafebcfafc130ee03.png)

## **工具选择**

创作一个绘本故事，主要分成故事和画面两个部分，故事部分自然使用今年最火的ChatGPT来完成。而绘画方面，如果想精准控制画风和构图动态，按说应该使用Stable Diffusion。但Midjourney在上手方面优势巨大，且比Stable Diffusion更容易做出吸睛的画作，所以我这次先使用Midjourney的Niji模式来尝试。

![](img/853523fd48fd0a0ddc13ff066047d3e8.png)

## **剧本**

使用ChatGPT，随便想一个主题、地点和主角，然后ChatGPT就会帮我们输出一个故事。

![](img/1d50148b97ff70920f6f6defdfaab45d.png)

ChatGPT输出的故事内容有些过长了。我改用对**话的方式**让C**hatGPT**修改**，向它提出“简短**一些”“加入一些反转”“加一些温馨的内容”的要求，它就像一个世界上最好的乙方，非常耐心的修改故事，最终得到了我觉得还OK的版本。如下图所示：

![](img/ee78a0b779a4eb5aa1b46e00baf183c6.png)

但是上图这个故事还得变成**我们绘本分镜的段落语****言才能使用，于**是对ChatG**PT发号施令**“用分镜的方式描述出来”，便可以得到带序号的段落文字。我们可以对ChatGPT说“增加/减少画面”或直接指定”变成8个画面“，非常好控制。如下图所示：

![](img/ada6fe0d60f1403fddfec88d6aaa06fa.png)

这些简单的分镜语言我们可以通过翻译变成英文作为Midjourney里的主体描述词，

![](img/457d2ff6fc8ca1e488882f944bb0e10e.png)

![](img/63cfb869599c0ab3b56e558236a3019d.png)

但是绘本的配文不可能使用如此苍白的语言，同样指挥ChatGPT丰富镜头的配文“给每个分镜用绘本的方式写一段故事文案“

![](img/325dc907c5292afb80d49017f4c9943b.png)

如此我们就得到最终绘本里的配文。

![](img/1705e87356fc94987b286c215805685e.png)

## **跑图思路**

我们要完成一篇整体的绘本故事，得保证画风统一、角色正确以及画面能和文案搭配。（1）因为描述故事场景的文案很长，所以为了统一画风，我们需要尽量简短的关键词来控制画风，我这里使用的是Mike Mignola + Tove Jansson的风格，这两位一个是美漫画家，另一个是儿童画画家，这样就融合成了一个很特别的儿童画风。关键词如下：【The old man in the white tall hat carefully made delicious food in the restaurant, but the business was not satisfactory】（场景主体描述）, Inspired by Mike Mignola + Tove Jansson（画家风格） --ar 3:4 --niji 5 --s 400 --style expressive（NIJI5 expressive模式）通过上面的关键词，生成图片如下：

![](img/9eb43116205457cdf4355653b420ff80.png)

（2）其次为了相对固定人物，需要给主角一些外貌上的特征，比如我这里面设定了一个带白色厨师帽的老人和红发穿着绿衣的年轻人，当然Midjourney的跑偏率还是比较高，但是这样相对能让你需要的“统一人物”概率高一些。

![](img/e0406b31ba72790e23e42015103ee6ae.png)

（3）虽然ChatGPT帮我们写好了分镜的描述，但是我们还是尽量人工优化一下，尽量输出给Midjourney简单直接的描述，不要有复杂的语句，如果跑出的图和你想要的画面南辕北辙，尝试把自己的描述换一个方法说给Midjourney听。

![](img/3d57e4b55f6728abd7c9b2b7bd1eedd3.png)

## **整合**

将跑出的图和文案（ChatGPT写的配文）结合。首先挑出合适的图，如果有些画面很好但是跑偏的地方可以用PS微调一下，接着使用PS将文案融入图片里，再简单排版一下，一个简单的绘本故事就创作完成啦！下面的图片为整个完整的绘本故事。

![](img/e42c8c3c35c65327485f696d9d68a9ac.png)

![](img/c66e8b49e205e2572cb20ae386f8e5f8.png)

![](img/45926fc8003f0b7c748c3765f741f3dc.png)

![](img/1fde98435db0b2a71f63e745af7fc910.png)

![](img/2c24f7bf3c5a5198c2db9abf09017473.png)

![](img/ac28f3a6060a8f7768e49fc92dfcf17e.png)

![](img/67d4d05257cdb2c20b5b4d689d703066.png)

以上是整个流程的制作过程，用类似的方法可以创作出更长更完整的故事，也可以创作出漫画或插画。

我们也不用苛求全过程只使用AI来创作。现阶段我依旧视AI为辅助工具，可以更高效的帮助你完成自己的工作或创作。

![](img/15203fb450ed6a9601de184b8fb28722.png)

## **如何更进一步**

Stable Diffusion+Controlnet出图更好控制一些，你**可以完全**文生图，也可以绘制草图后让Stable Diffusion去画，利用好“局部重绘”功能可以更好的完善细节。

下图就是手绘稿通过Stable Diffusion得到的生成图。

![](img/6a14a34e9452cbef5e18cf14f51c30ef.png)

现在的Stable Diffusion出图质量非常依赖模型，如果需要的画风没有对应的模型，而Midjourney又能跑出想要的画风，不妨尝试让Midjourney生成的图，再到Stable Diffusion炼制自己的专属模型，这样达成的效果会很好。

## 限时免费领取资料

**更**多学习资料请按以下方式 扫码 进群 或  添加助理 蜗牛 “john_zhang4415”  免费限时领取

![](img/dce3f65a1809edee61dcc50747291cb9.png)

AIGC学习交流①群

![](img/412766793b5058d111e340df38308df4.png)

微信号

![](img/a3406839e2e68ce7e30bf7eeafee015b.png)

公众号 汽车新科技研习社

## 部分学习手册内容

![](img/394b43c4421da9b24ffc3e8a48b4c0a1.png)