
# OpenAI Agent发布会深度解读

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/T7gPdhE5voLhCJxpPyocCxfdnwh](https://kq4b3vgg5b.feishu.cn/docx/T7gPdhE5voLhCJxpPyocCxfdnwh)

正当人们还在热火朝天的讨论Manus的时候，OpenAI发布了两年以来最大的更新，重磅上线4大项Agent开发套件以及一个开源项目，从此开发者几行代码就可以创建一个专属的Manus。

![](img/d4cf93348c17ffef7b4ea81836c947a9.png)

这听起来非常的梦幻，距离发布会结束仅仅几个小时，X上就已经能看到铺天盖地的基于OpenAI Agent开发套件的琳琅满目的开发成果。有人开发了交互感不输Manus的通用Agent，

![](img/1682083871898ac46cf5a074caee4e97.png)

[20250312052821_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SAogb5RdWoyiaUxrZFPcrW3QnMg)

也有人实现了`Computer use`，全自动爬取网络信息，

![](img/75870146a80ff6e94cf842e24f7a3689.png)

[20250312041515_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RsFFb9R0ko00Wbx9IHKc35dTn9c)

并处理本地文件，

![](img/b405aa720b2d2cbf396ce64fc2a7171d.png)

[20250312062351_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TSRobLZnyob3jExyJDXcBwWknWc)

以及实现了大量的通用Agent创意功能：

![](img/06f635a0e4d1907f47445916a7da7205.png)

[20250312053006_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AHC5bAkoloNevYxJAArc7qLvnWb)

甚至，在这场发布会上，我们还看到了OpenAI的开源的诚意，首次发布了一款工业级Multi-Agent开发框架，`OpenAI Agents SDK`。

![](img/93d7a6b75cc3dec2fee75f116a142484.png)

可以说OpenAI这场发布会，对于Agent技术来说，就如23年11月的OpenAI开发者大会一样，意义非凡。

![](img/1242708248b02faaf611c3fc533d8371.png)

接下来，我就从一名技术人的角度，为大家详细解读下这场发布会的核心内容。

观前提醒，这场发布会信息量巨大，光是更新的技术文档就有3万多字，我尽量用通俗的语言帮大家梳理最高价值的信息，此外，我们团队第一时间翻译了本次OpenAI大更新的完整技术文档，大家感兴趣的话扫码即可领取。

![](img/26f2994b61e8195e6bd36a27ffec324e.png)

![](img/115b6a024828f051664c1845ab13ac6f.png)

## 发布会背景与核心更新

北京时间3月12号凌晨，OpenAI召开直播发布会，尽管没有提前预热，也没有类似GPT4.5这种噱头来镇场，但这次发布的内容，绝对足以颠覆现有Agent技术开发格局。

本次发布会由OpenAI产品负责人Kevin主持，短短的20分钟不到，总共发布了4项新的API工具，分别是`Web Search`、`File Search`、`Computer use`、`Response API`，以及一个开源项目`OpenAI Agents SDK`。

![](img/7eba6635622155f66525155df512b43c.png)

[20250312043553_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/En4TbcOgfo7V9SxEZZHcEno7npf)

这5个工具乍一看感觉并不惊艳，但事实并没有那么简单。首先来看这次发布的`Response API`，这是OpenAI两年来最大的底层API更新，从今天开始，开发者可以不再使用`chat.completions API`来调用模型，而是直接调用`responses API`来调用模型。要知道上一次底层API的更换，还是两年前，GPT3模型升级到GPT4模型的时候。

![](img/0d60b1c938280df15b2b2eb55b1d710f.png)

这就非常有趣了，要知道截止2025年2月，全球有超过200万开发者使用OpenAI API，而一旦底层API调用规则更新，必然会导致大量代码重写，甚至是流失大量的开发者。但OpenAI仍然一意孤行，大手一挥，就这么把底层API调用方法更新了，这是为什么呢？

最核心的原因，就是OpenAI希望自己的API不再是单纯的模型调用API，而是一个Agent开发API。其实从发布会一开始，Kevin团队就一直在以一种类似前后端一体的形式演示各项工具，也就是左侧是一个网页，而右侧是代码编辑器。

![](img/d606b046ec3c6f8bea8383cc7d45f799.png)

[20250312045228_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TwzpbyiV1oWAU3xqscDcMYkonLh)

同时，在右侧进行任何代码修改，都能在左边的浏览器中实时看到效果。这看着就非常酷炫，是的，这就是全新的`Response API`的功能效果，自此，AI开发进入全民级前后端一体式开发新时代。这么看，确实值得进行一次底层API的大更新。

## 三大核心能力详解

此外，新的`Response API`还能支持发布会上发布的其他三项工具，也就是`Web Search`、`File Search`和`Computer use`。尽管这三项功能听着耳熟，好像我们在别的地方也能看到这些功能，但OpenAI实现的程度，各个都是王炸级别。举个例子，普通的`Web Search`就是上网搜集并汇总信息，而OpenAI的`Web Search`是`Deep Research`的API实现版！什么意思呢，前段时间，大家被Manus的网络搜索和长文本编写能力所震撼，如搜索并编写一篇研报，

![](img/ffe40daceee2aa5935e7fc24a0ca5720.png)

[20250310141421_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/E2mmbCpxAoXM90xHTpCc9dUJnVc)

而现在，OpenAI的`Web Search`，只需要三行代码即可实现相同功能。搜索又快有准

![](img/b99a5fa7436956c415ddb752e186b8d8.png)

长文本编写也是一气呵成。

![](img/fb500911903358510954296c4ccb6245.png)

[20250312040632_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/C1JJbG7vQoGQCoxU958cu3GRnIC)

大家之前如果体验过ChatGPT的`DeepResearch`应该能感受到。

![](img/0f6a6a47d24bab577343d545c1edee0f.png)

![](img/36b765575286fd5f6872673fb98e8ecf.png)

再比如`File Search`，听着感觉就是RAG知识库检索功能，但实际上OpenAI的`File Search`最高支持100G海量文档检索，可以检索十几种主流类型的文件。这就不是简单的RAG系统了。

![](img/cc718671dfdb10acda3893d12cf6a91a.png)

值得一提的是，这个`File Search`就是OpenAI的`Assistant API`中的文件检索功能的升级版，这套RAG系统可谓是OpenAI的看家本领，我们并不知道OpenAI是如何对100G海量文档进行检索的，但之前我们在使用`Assistant API`的时候确实能感受到，这套检索系统的性能完全不输`GraphRAG`，可以说是我所用过的最强的文本RAG系统。

紧接着，第三个功能，也是这次发布会的最劲爆的功能，`Computer use`。谁能想到，这个由Claude母公司，Anthropic在去年10月提出的实验性质的功能，现在由OpenAI发布了工业级解决方案。

![](img/03deafaf8efeacf36ae36dcb531735aa.png)

[20250311133419.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Hhj5bOai8oiRcLxnHGbczXZvnlB)

所谓`Computer use`，指的是让大模型看到电脑屏幕，并在了解电脑界面操作逻辑的基础上，全自动的代替人来操作电脑。

![](img/88541f86a775cfaf51bab01ade62ae61.png)

大家其实能想象的到啊，电脑能力的边界，不就是现阶段人工智能的能力边界么？所以，自从`Computer use`技术概念诞生以来，就被视作是大模型Agent的终极形态。

之前爆火的Manus，其技术本质就是`Computer use`，只不过操作的不是本地的电脑，而是在沙盒环境中的云端的电脑。Manus的网页浏览、在线编程、进行文件管理等功能，本质上都是基于一个云端的电脑，借助`Computer use`功能来完成的。

![](img/97f88a8d3ae273acf29d56f1b5c495d3.png)

而现在，OpenAI开放`Computer use` API，让全体的开发者都借助这一功能来进行Agent开发。这个功能也被称作压垮Manus的最后一根稻草，不是，也被称作开启未来Agent发展之路的钥匙。

在OpenAI `Computer use`功能上线的第一时间，外网就有大量用户评测出炉，无论是操作网页还是处理本地数据，都非常惊艳。

![](img/0384ae59104a53cfca0d294c435aad13.png)

[20250312053333_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SWYIbrF8EoHYePxEpi6cdwM6nHb)

## 工具组装与任务编排

当然，以上所有的功能，`Web Search`、`File Search`和`Computer use`，在`response API`调度下，都可以自由组装来构成复杂问题的解决流程，比如我想要制定一个旅行计划，那就需要`Web Search`搜集信息、`File Search`来检索我的出行习惯、同时需要`Computer use`来帮我预定机票旅馆等，并且，`response API`还能监督每个环节的执行情况，遇到问题会自动`debug`，从而保障任务完成率。甚至OpenAI还提供了一个任务管理的后台，让开发者一目了然查看目前Agent执行进度。

![](img/ebf3aa29449c1b69e962b1d6f2516512.png)

[20250312052821_rec_.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OXdPbMTOPoXHPhxwkrXcK8Pbnwf)

## OpenAI Agents SDK：企业级开源框架

而最最最关键的是，OpenAI还开源了能实现`response API`全套功能的`OpenAI Agents SDK`，不同于去年开源的`swarm`是一个实验项目，现在的`Agents SDK`是一个真正意义的企业级Multi Agent开发工具，这也是OpenAI截止到目前为止最有诚意的开源项目。

![](img/1f4ffa8e0ba04c1c182e010d0bb1366f.png)

看到这么多重磅消息，恍惚间好像又回到了23年4月，那个GPT-4刚刚发布的月份，一切都是那么新奇，对未来是那么的期待。

其实早在今年年初，各大顶级科技大厂就预测，在大模型基座技术逐渐趋于成熟、外加市场需求井喷的当下，25年将会是Agent技术爆发元年，而OpenAI这轮发布会，无疑是吹响了冲锋的号角，接下来的几天，我还将深度使用OpenAI发布的各项工具，并在第一时间为大家带来教学和评测视频，相关内容会在赋范大模型技术社区中上线，大家扫码即可加入。

好了，以上就是本期的全部内容。
