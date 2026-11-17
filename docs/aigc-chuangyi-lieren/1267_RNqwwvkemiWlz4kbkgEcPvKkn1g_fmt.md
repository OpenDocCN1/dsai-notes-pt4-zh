
# 20251007-OpenAI开发者大会：不止GPT-5 Pro，ChatGPT正在变成一个操作系统

> 来源：[https://aigcreative.feishu.cn/docx/SlrMd8bqvoFi9vxdBnfcKahanTe](https://aigcreative.feishu.cn/docx/SlrMd8bqvoFi9vxdBnfcKahanTe)

![](img/964bb9f3b71af3b75b1e089ef15d5ae5.png)

今天凌晨，OpenAI 的开发者大会，发布的东西确实不少。

![](img/682ffd81bed6d53c1d07ebc7546d14aa.png)

早上起来补了下课，仔细看了一下，想跟大家聊聊我的理解，看看这些新东西背后，OpenAI 到底在想什么，以及对我们普通人和专业开发者来说，到底意味着什么。

![](img/dec800d7ebff80b68cc89933abfac53e.png)

## 01.最重要的变化：ChatGPT 正在变成一个平台

**这次开发者大会，最核心的，就是** “Apps in ChatGPT”。

[Apps in ChatGPT.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JCZQbHMkMou1gQxsP8GcD9jCnzb)

我们可以回顾一下，最开始我们用 ChatGPT，它就是一个很厉害的问答工具，你问我答。

后来，OpenAI 推出了 GPTs，允许用户自己创建定制版的机器人，算是一次尝试，但没有真正普及开来。

而这次的 “Apps”，思路完全不一样了。

OpenAI 不再是让用户自己小打小闹，而是直接邀请了像 Booking、Coursera、Canva、Zillow、Figma 这样成熟的第三方应用，把它们的功能“集成”到 ChatGPT 内部。

![](img/2e453fb04a09cf99cae42515a939e8af.png)

![](img/f87d624970b3cbb33dae54863110c5e5.png)

![](img/e4b1a461259e9114bc5f8fa19b4ba19a.png)

![](img/6d044b32e714dc2888f04dabe51d5ff7.png)

这个思路，其实很像当年智能手机从功能机时代，迈向智能机时代的关键一步：推出 App Store。

举个例子，你在 ChatGPT 里说，根据上传的大纲文档生成PPT。它就可以直接调用 Canva 的能力，让你在聊天窗口里，通过说话来调整设计，直到你满意为止。整个过程，你不需要打开新的网页，也不需要切换应用。

![](img/bb923585bef561e43f13a94ce1ce5f1e.png)

这就是关键所在。

OpenAI 的目标，是想把 ChatGPT 从一个“问答工具”，变成一个“任务处理平台”或者说“AI 时代的操作系统入口”。

他们希望用户能停留在 ChatGPT 里，完成越来越多的日常任务，无论是工作还是生活。

他们还提到了一个“代理式商务协议”，这个词听起来有点复杂，其实就是为了以后在 ChatGPT 内部直接完成交易做准备。比如你用它找到了合适的商品，可以直接下单购买。

所以，这次发布会传递出的一个清晰信号是：ChatGPT 的定位正在发生根本性的转变。

## 02.为开发者铺路：让开发 AI 应用更简单

有了平台的想法，那下一步自然就是吸引开发者。这次 OpenAI 推出的 `AgentKit` 工具包，就是干这个的。

![](img/eb6104c18298cc71c3d58dd20420146d.png)

简单来说，这是一套为了降低 AI 代理（Agent）开发门槛的工具集合。

以前，开发者想要做一个能连接外部数据、能执行复杂任务的 AI 应用，需要自己处理很多繁琐的事情。现在 `AgentKit` 把很多工作都打包好了。

![](img/9115f6889b803d368128d8ffafdf62d2.png)

[Intro to Agent Builder (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IErzbDqvEoIGnyxVapzczQCpnJg)

比如里面的 `Agent Builder`，提供了一个可视化的界面，让开发者可以通过拖拽组件的方式来设计 AI 的工作流程，这对于代码能力没那么强的开发者来说，友好很多。

还有像 `ChatKit`，可以让你很方便地在自己的 App 里嵌入一个类似 ChatGPT 的聊天界面，省去了自己从头开发的麻烦。

我个人觉得特别实用的是 `Guardrails`（护栏）功能。它能帮助开发者更好地控制 AI 的行为，比如设定规则，防止它泄露用户的个人信息，或者输出不合规的内容。这在开发一个真正能上线给用户使用的产品时，是非常重要的。

总的来说，OpenAI 就是在告诉所有开发者：来我的平台上开发应用吧，我已经把路都给你们铺好了，你们只需要专注于创意就行。

## 03.模型更新：更强的 Pro 版和更实用的迷你版

当然，每次发布会都少不了模型本身的更新。

![](img/6bc57f8ef6bbca86705615b421a5bcb4.png)

GPT-5 Pro 来了，并且已经可以在 API 中使用。从 “Pro” 这个名字也能看出来，它的定位是处理那些更专业、更复杂的任务，尤其需要高精度推理和分析的场景。对普通用户日常聊天来说，可能感知不强，但在科研、工程、金融分析等领域，它应该会有很大的发挥空间。

![](img/b40ccfabf8b80f2f652bca6fd004a2ab.png)

视频模型 Sora 2 和 Sora 2 Pro 也开放了 API。这意味着开发者终于可以把顶级的 AI 视频生成能力，集成到自己的产品里了。

未来的很多内容创作工具、游戏、甚至影视制作流程，都可能会因此而改变。

![](img/54e255d1a2f1bab48b1c13bac1ed13fb.png)

不过，除了这些“大家伙”，我个人觉得，这次发布的两个mini模型也同样重要：一个是用于实时语音的 `gpt-realtime-mini`，另一个是用于图像生成的 `gpt-image-1-mini`。

它们最大的特点就是便宜和快。便宜了 70%-80%，这个降价幅度非常可观。成本降下来，就意味着更多中小型开发者或者个人爱好者，有能力去尝试开发一些有趣的语音助手和图像应用。

一个技术想要普及，成本永远是关键因素之一。

## 04.支撑一切的基础：算力

发布了这么多强大的模型和平台功能，背后都需要海量的计算资源来支撑。

![](img/c437078d108376f5ce348855bc4ff455.png)

这次 OpenAI 公开宣布了和 AMD 的大规模合作，要采购部署巨量的 GPU。这个消息其实也透露了他们的决心。

他们很清楚，随着 AI 应用越来越普及，对算力的消耗会呈指数级增长。现在大规模投入基础设施，就是为了保证未来的发展，不会因为算力不足而“断电”。

![](img/0f23821e803b7a4b4481db780869f907.png)

总的来看，这次开发者大会，OpenAI 的路线图已经非常清晰了。他们不只是在埋头研发更强大的大脑，更重要的是，他们在加速构建一个以 ChatGPT 为核心的应用生态。

从一个聊天工具，到一个包罗万象的应用平台，这步棋走得很大。

对所有从业者和用户来说，这其中既有新的机会，也确实带来了很多值得思考的新问题。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

## 知识库动态
