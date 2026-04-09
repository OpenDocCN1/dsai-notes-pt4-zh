# 像高手一样编写 GPT：从基础到实践，学习使用 GPT 的 Action

> 来源：[https://waytoagi.feishu.cn/docx/GRi9dupovoz6wDx4UMkcgFUjn3O](https://waytoagi.feishu.cn/docx/GRi9dupovoz6wDx4UMkcgFUjn3O)

![](img/2d157ce3bb83fcf166b4e28be9d8c08c.png)

**全文约6000字**，阅读此文大约需要⏰ 30分钟，但你将有以下收获：

+   理清 **AI-Agent**、**GPT** 和 **Action** 之间的联系，打下坚实的认知基础；

+   亲身实践配置不同的 **GPT**** ****Actions**，直观感受其独特性；

+   探索 **Action** 的实现机制，为后续提升指明方向。

**本文为直播回放的整理，**视频版可以在B站搜索 “LongTalk宇龙” 找到

![](img/8e29d1a887459413173db12921dc3324.png)

```
提到 **Action**，就必须先了解一下 **Agent**。在人工智能领域，**Agent** 一般被翻译为 “代理”，“智能代**理”，“智能体”。****在计算机领域**的构思里：智能体 Agent 是一种特别的计算机程序，他通过感知其环境，自主采取行动（Action）以实现目标，不需要人时刻告诉它该做什么。
```

![](img/636fcf4dd7ace9cdf9e6e5c356ed0d64.png)

1.  智能体通过【传感器】感知环境判断【当前是什么状况】**根据预设的【**条件/行为规则】**，发起行动（**Acti**on）调用【执行器】，完成**任务，最终影响环境在这个框架下， Action 泛指了所有基于行为规则下的行动，拿扫地机器人举个例子：

1.  扫地机器人通过红外感知器感知环境

1.  它发现了灰尘或者杂物

1.  **它根据预设的**清理规则，判定需要前往清理（Action）

1.  它驱动轮子和吸尘器以及扫把，前往并吸入灰尘，影响了环境（干净了）

1.  判断环境中是否还有灰尘 / 判断接下来要如何前进……（继续循环）

![](img/ec22542ef19bae3c691101f8a339c27b.png)

《钢铁侠》里的人工智能助手 Jarvis 就在 Agent 的构想框架下，在设定里，Jarvis 具有的能力包括：

1.  辅助托尼·斯塔克（钢铁侠）进行日常工作和战斗

1.  管理、控制和运行斯塔克大厦和钢铁侠内部的系统

1.  复杂的数据处理、决策制定和问题解决能力

1.  快速的语音交互反应，高度的交互性

**看起来是不是很厉害？**我们加粗一些关键词：

1. **辅助**托尼·斯塔克（钢铁侠）进行**日常工作**和战斗

2. **管理、控制**和运行斯塔克大厦和钢铁侠内部的**系统**

1.  复杂的**数据处理**、**决策制定**和**问题解决**能力

1.  快速的**语音交互**反应，高度的交互性

![](img/6aebd808aa174c484f5e9cd051d92622.png)

看看另一个 **Agent**，**Agent** Smith，来自《黑客帝国》的史密斯探员，能力大概有：

1.  抓人

1.  打人

1.  Ctr**l+C,C****trl+V**,Ctrl+V**,Ct**rl+V,Ctrl+V,Ctrl+V,Ctrl+V,Ctrl+V,Ctrl+V,Ctrl+V……和能发射火箭能战斗的 Jarvis 相比 —— 他只会出拳和开枪，所以我姑且认为他要弱一点。假如 Jarvis 代表了人类对未来 Agent 想象的一个高标准，那么他和 GPT 中间可能大概隔了 n 个 Agent Smith，GPT 和他们相比基本就是 Baby：

当然，这位 Baby **Agent** 也是有很多能力的。

![](img/1f22200f17bdf1ca36bf085ea3ec5b90.png)

Lilian Weng() —— OpenAI的应用人工智能研究负责人，也是GPT-4的开发人员。

![](img/be95a6bc057c14ecc7877d8f30039335.png)

她在个人博客的博文《大语言模型驱动的自动智能体（Agent）》**提到了关于**他们（OpenAI）构思中的 Agent 系统：

![](img/b6cf24acbc1656dc46d64e6bf7e93407.png)

> **“ ****大型语言模型的潜**力不仅仅在于创作出流畅的文本、故事、论文和程序；

>

> 它还可以被视为一个强大的通用问题解决工具。”

和开头提到的计算机领域的通用

**比，Lil**ian 进一步具体化了 Agent 的能力模型 ：

![](img/a7b23437fdd185b3f6ac9500662fed22.png)

在 Lilian 看来， **Agent** 的能力被具象为三大类：

1.  **规划**

1.  **记忆**

1.  **使用工具（工具 + **** ****行为 ****Action****）**你可能有印象 —— 在之前的**GPT-4**，画图工具Dall-E，Bing搜索，插件，和代码解释器**，都需要切**换不同的会**话/窗口实现的。后来GPT**-**4 成为了 **GPT：All-Tools，可以在一个会话里根据用户需求和上下文自行选择工具，并且做出行动 —— 这个行动，就是 OpenAI 定义的“Action”，如下图：

![](img/d19e9dbb94dd5e5b5abace936e2e90f5.png)

当你在让它搜索，画图的时候，**GPT** 会先思考决定怎么使用工具，然后会出现一个小图示，告诉你他尝试使用工具来完成你的任务。

![](img/aaa75675b68b35ba13f7ccb3d6cd9ac1.png)

现在你应该能够认清 **AI-Agent**、**GPT** 和 **Action** 之间的联系了：**GPT**（All-Tools，也是现在大家正在开发的“G**PTs”）****，是 Op**enAI 之于传统 Agent 定义交上的一份答卷 —— 具体的描绘了在大语言模型（LLM）驱动下 Agent 的系统实现方式。

**即：使用 L**LM 驱动，通过外部 API **获取信息和执**行的行动（Action），也是本文后半部  提到的 Action 的定义，如下:

![](img/c73fa4a0f8b3e2c3835c9a04c34ad808.png)

**如果要选择一个最****容易**上手的 Action ，Webpilot() 应当首当其冲：

![](img/e6e01fd569bc29748669246bd231bb1f.png)

它能够让你的 GPT 支持比原本的“Web Browsing”更加实时的联**网功能，**同时还能获取网络上的**文本和部分链接，使用方法也**很简单，只需要 30 秒：第一步：在新建的GPT里把 Web Browsing 勾掉，因为他们的作用都是让 GPT 可以访问网络上的资源，不勾选可能会有冲突。

![](img/4c98f429002011356dfd1d725a6e6b40.png)

**第二步**，点击 添加 Actions（Add actions）：

![](img/012debe5baf8aa1c7328e444fd856219.png)

**第三步，**点击 import from URL，填入：https://gpts.webpilot.ai/gpts-openapi.yaml

![](img/6d17c94a925ca8664c375b158cea4989.png)

你会发现  Schema()（组织和结构）这里出现了一大堆文字，内容先不用管。继续在最下面的隐私政策（Privacy Policy），填入：https://gpts.webpilot.ai/privacy_policy.html ，如下图：

![](img/8fcb163833373e3eec06f509a1f34136.png)

之后点击左上角的返回保存即可，你甚至都不用写 Prompt，直接保存就可以使用。可以测试一下效果

![](img/6b8d9de5a0e6060727944baf5d8184d7.png)

我们询问同样的问题：“保亭有哪些正在上映的电影”。Webpilot 和 GPT-4 自带的搜索一样，它们会通过先搜索找到豆瓣电影的页面然后把页面内容总结给你，只是 GPT-4 点内容相比之下明显不够完整，而 Webpilot 的版本可以返回15条电影结果。

因此当你在你的 GPT 加入 Webpilot 后，大部分网上的文本你都可以作为原材料而进一步加工了，比如你可以 使用以下 Prompt 做一个菜谱助手，让他先搜索，然后处理信息：

![](img/e09ddd8dd39a5567a70be4f58497acd3.png)

![](img/cd0b50ec36bd8690680d7f8845c4c7c5.png)

**但 Webpilot**** 的缺点也**很明显，它只能查看网页的所有纯文本内容。

可如果我们想要更进一步 —— 比如需要更加精确的信息和内容怎么办？这就需要我们自己制作一个 Action 了。

**编写 Action**

**你有没有试过问 GPT 某部电**影讲了什么？比如：“奥本海默”这部电影讲了什么？

![](img/adc3295ecf50b4d56824af2d84d81cfe.png)

GPT-4会这样回答：里面有概览、开头、展开、总结，很明显的 GPT 风格回复，你还可以看到链接里有有蓝色的引号，说明这是通过 Bing 搜索后获得的知识。

**再看另**一个版本：这是我做的一个“找电影”的 GPT Demo：

![](img/f69ee369cc4fc580fbb0f9126aa85a59.png)

感觉怎样，是不是看起来丰富多了？除了电影信息，背景知识，还有主视觉图。当然，还有用户评论。

![](img/2a10fec308c8cbf30228108aa4e490d4.png)

和上一个相同的地方是：顶部也有一个和** GPT 使用**搜索和画图一样的小图示，“**Talk to**”，意为和某人聊天 —— 它在和谁聊天？在回答这个问题之前，我们先看一下 Prompt：

你可能注意到了，这里面有两个英文单词

![](img/8e2f0ef581fde55f7fab9f039585ac49.png)

他们对应了我在这个 GPT 里设置的两个 Action：一个代表 themoviedb 的某条数据，一个代表使用webpilot这个工具。

# api.themoviedb.org

看起来是不是像个网址？没错，就是一个网址：

# **api.****themoviedb.org**

![](img/01f3615efbe6e95b1ab524c05c38d193.png)

themoviedb.org 是一个电影数据库，api 是他的二级域名 —— 就像是门牌号一样，让 GPT 可以来这里取数据，也就刚刚 “Talk to” 的对象。接下来，我教大家如何快速上手这个 Action 。

**完成“找电影”的 GPT**

![](img/bcb6f765ff81661aae66ffac689c2fbb.png)

因为调用 TMDB 是需要 API KEY 的，所以我们需要先前往 themoviedb.org  注册后申请一个  API KEY 。在你注册后，还要点击邮箱验证邮件里的链接后才可以使用，注册流程就不赘述了

![](img/1c0df7379f9688b798ae1243fc7536c8.png)

![](img/7d485cbbb676f13d444c1d1607a16861.png)

![](img/c8574b86a74c264023ba0cc862c86e39.png)

注册后，依次点击右上角头像 - 账户设置 - API - 请求 API 密钥 - click here

![](img/fbb98a309c96be8073c3f4e4f7e39bf4.png)

选择 Developer 开发者，协议拉到最底下然后 Accept 接受。

![](img/4300485d84a10026d2f191501c617550.png)

使用类型：**网站**。应用名称随便写，URL 填 ChatGPT() 官网就行，简介必须写英文的，下面的除了邮箱外（因为要收验证码），大概填填提交就好，支持中国手机号。

![](img/784e51fc1f414fb755df2b6f923b5a32.png)

填完了就可以获得 **API 密钥**和** API 读访问令牌****了，找个小本本先记**下来下面的 ‘API 读访问令牌’ 备用。

**构建 GPT**

**新创建一个 GPT，名字**描述随便写，Instructions 使用以下内容：

使用中文对话，当用户想找电影时，输出以下内容：

+   详细介绍和主视觉图（取自'backdrop_path'）

+   主要剧情

+   观影前需要了解的背景和知识

+   搜索网上的评论并总结，你可以使用webPilot来寻找

添加一个上一步的 Webpilot Action。

**继续添加一个**新的 Action，在 Schema 里粘贴以下内容：

+   openapi: "3.1.0"
info:
  title: "The movie finder"
  description: "Finding movies in themoviedb"
  version: "v1.0.0"
server**s:**
url: "ht**tps:**//api.themoviedb.org"
paths:
  /3/search/movie**:
**    get:
      description: "Find m**ovie** for a query"
      operationId: "SearchMovie"
      parameters:
        - name: "query"
          in: "query"
          description: "Keywords for searching movies"
          required: true
          schema:
            type: "string"
        - name: "language"
          in: "query"
          description: "languages (ISO 639-1 tags) , e.g. 中国='zh-CN'"
          required: true
          schema:
            type: "string"

点击 **Authentication** 的齿轮，勾选 **API KEY，Bearer。**

![](img/3698558c4fd69da6349c06bcb5eb92e7.png)

**在中间的 API **KEY 填入 你之前记在小本本上的 API 读访问令牌，点击 Save 保存。

最后，在 **Privacy policy** 里填入：https://www.themoviedb.org/privacy-policy

如果你配置的没错，GPT 的内容应该如下：

![](img/818237905c5ec3dcc5cb6d16bcda3dfa.png)

完成！保存你的 GPT，输入一个电影试试看，如果有用，可以评论一下。

**如何实现的？**

大家现在每天使用微信，看视频，新闻获取信息，我们都知道是因为“上网”了，是因为我们接入了“互联网” 获得这些信息。

当你在使用线上 —— 也就是网上的产品时，不同软件和服务之间就通过 API 接口来让不同的应用程序进行主要的数据交换。

![](img/1ed10c6e91d5c35a44e930653ad358e7.png)

# API 就像是一个信差，它接受一端的请求，告诉那边的系统你想要做的事情，然后把返回的信息发回给你。

![](img/3a323de48c92a393e9d7048963875b7d.png)

在这个例子里， GPT 就通过预先配置的Action 里的 TMDB API ，获取了电影相关的专有信息。

![](img/7e83ba9308e350a600bb3c4ec9acb057.png)

当我问“奥本海默”这部电影讲了什么之后：GPT 识别到了我的意图，按照 Prompt 进行第一个任务。

![](img/714e93ab2fcacd42498e4ea886b303bd.png)

使用 Action去 The Mov**ie DB 寻找相关数据，****从数据中取出 ba**ckdrop_path 这个数据作为主视觉图，然后处理剧情数据、生成背景知识。最后使用 webpilot 搜索评价，返回整理后的数据，也就是最终你看到的结果：

![](img/cebc1a0a507e169af31c267f3e546e6f.png)

接下来，我们继续深入GPT Action 的配置项：

![](img/f7cbf8ef694a07d98ea08625f70482d4.png)

配置一个 Action ，你需要：

+   **Schema****：（下图）通过**导入或者**直接填写**，**相当于**给** Baby G**PT 的一个📓 操作手册，告诉它可以去哪，干什么，需要准备什么。这个示例的版本很简单，真正的会更复杂一些。

![](img/71911890011e04f91ac320e3d13b9ac0.png)

![](img/21bf53d4da2ca10f62a6fbdd21ede8b1.png)

+   Available actio**ns：（上图****）可用行动，在你输入 **Sc**hem**a** 后会自**动生成，展示大纲中可以使用的行动，对应 operationId ，get，path —— 加入操作手册是本魔法书，Available actions 就是可以用的魔法

![](img/970dba25b697cd0b639279081ed995fc.png)

+   **Authentication：**（上图）认证，可以理解成身份证，没带证件不能请求数据

![](img/8a5ad5f0a7ae2938f85ac48c353641a9.png)

+   **Privacy policy**：（上图）隐私政策，因为 Baby GPT 会携带用户的需求访问别的服务器，所以需要一个隐私声明，这个一般在网站底部都可以找到，把网址填进去就行。

然后来看 Schema 的结构：

![](img/916d271e1d8e2275b4e3eb92289ec3e3.png)

是不是有点**懵**？

懵就对了，这已经是**前端工程师****相关领域**的知识了，我们的知识体系里还没有建立这些信息的映射关系。

![](img/be2281ead05095be541691344ea70178.png)

**这个页面**大家应该一**眼就能看**明白，这是一个搜索结**果页，搜索的关键词是“奥本**海默”，返回了两个结果，每个结果的信息都有海报、名称、上映时间、描述。

这个页面的网址是这样的：**https**:**//ww**w.themoviedb.org/search?query=奥本海默

![](img/e9c555d7b44f17ca37eeb8517a7b2120.png)

域名 (www.themoviedb.org)：域名指定了网站的位置，类似地图上的地址。

在这个URL中，www.themoviedb.or**g 是** themoviedb.**or**g 网站的域名，用于定位和访问网站。为了方便理解，我把 URL 和 Schema 里的红框部分做一个对应关系，你会发现他们不一样，这是因为一个是面向普通用户，一个是面向开发者的。就和 ChatGPT 网页版和 ChatGPT API 的关系差不多。

![](img/fba400b4afbdb43d1eefd2a92632d791.png)

路径 (/search): 路径指定了在网站上特定页面的位置。在这个例子中，路径是 /search，通常表示网站的搜索页面或功能。

![](img/79861855eb35d72d55aa3d358ccf39ff.png)

**query**，就是一个“查询**参数”，其**中 query 是参数的名称，奥本海默 是分配给该参数的值。

这个网址完整的意思是：访问 themoviedb.org 网站上的搜索功能，执行一个搜索 “奥本海默” 关键词的操作 —— 我要找一个名叫“奥本海默”的电影。

![](img/11599fb65732e7aed6e5ff6ecf4eaa5d.png)

有些同学在写提示词的时候会用这种等号或者冒号的方式，其实都是一种变量的形式，请记住这个感觉，后面我们会继续用到这个概念。

![](img/185e8e135456eb37a6a41008437e4c92.png)

也就是说：刚刚 TMDB 的 Action 实际上就是访问了下面的网址，提供了 query 和 language 参数，然后发起了请求，获取数据，如下：

![](img/f3532fbdd2902bc83e43d9c3a6a2c02c.png)

**但虽然我们大致理解**了GPT发送请求是怎么回事，你可能有个问题一直在想：我要如何才能知道去哪请求，查询参数怎么写呢？

寻找 API 文档

![](img/1acbb02eca9f476c6a4d26ad6943d4b7.png)

很简单，去官网找，API 的规则一般都会写在网站的开发者相关的页面里或者 API 文档里，比如你可以从 TMDB 首页的“更多”进入，其它网站也大同小异。

这个 Action 用到了 TMDB 的搜索电影 API ，文档的网址是：https://developer.themoviedb.org/reference/search-movie ，下面是这个网址的内容：

![](img/30c5293df45f2f19d9e49a54db96ac55.png)

API 文档都会很详细的告诉你如何使用他们的API，在左侧的导航部分你可以看到还有很多其他的 API 可以使用，我们先忽略，专注到右侧部分，点击图片可以放大。

![](img/e7eca215456cd3bce4684828f270b467.png)

你可以在右上角的认证里看到你的 API 读访问令牌。也可以看到和我配置文件里**相同的部分，包括 **url**，请求方法 get（红框**），对应需要的查询参数 query（蓝框） 和 language（黄框）。这个 API 的作用就是：Search for movies by their original, translated and alternative titles. —— 输入 query 关键词，如果成功，就会返回符合关键词的电影列表。

![](img/cc4cb53ee55a93a2f0a4367648716a2f.png)

在右侧 RESPONSE（响应）这里也展示了当我们请求后响应的范例。

我们可以在左边 query 输入“奥本海默**”，Langu**age填zh-CN，点一下 Try it，就会“啪”的一下出来一大堆数据。

![](img/a52608ee4cd7d2793d11ed5cb72f0e74.png)

这**种数据格式叫做JSON，你只需**要知道这种写法可以让程序很方便的读写就好，看起来有点复杂。 —— 不过你仔细看一下，就会发现这个数据其实和脑图没有太大区别。还记得之前提到的键和值么？

![](img/a9c19c5a19e047f8a01a98dc4b93fb24.png)

可以看到这里面返回了两条结果，是不是有点眼熟？没错，这个API返回的数据，应该和搜索结果页是一样的。只不过网页拿到了这些数据，做了样式的美化展示。

![](img/307429a6cf53948ba2400be65ece82dc.png)

这是第一个结果的数据，对应了搜索结果页的内容，你会发现展现的内容远比数据维度要少 —— 这是因为有些数据虽然不展现，但为了支持筛选所保留的。

![](img/bfc4959024e6563241700eed910ad8af.png)

注意看，这里出现了‘backdrop_path’，记得原先 Prompt 是怎么写的吗：取出 backdrop_path 这个数据作为主视觉图 —— 所以这里是一个图片地址()，但它是不完整的，只有后半部分。就像是一个分成两块的藏宝图，两部分拼起来才是完整的图片地址。TMDb 的官方 API 文档（https://developer.themoviedb.org/docs/image-basics）里就写了如何构造完整的图片 URL：

![](img/e30a242bff0b4811c4ef69c606de8e27.png)

**但是因为我们的 Baby** GPT 已经是一个成熟的 GPT 了。实际上，它已经有 TMDb 的训练数据了（相当于裤兜里已经有半拉地图了） —— 所以即使你不说，它也会自己完成构建完整的图片地址。

![](img/c26dcd7730646b63d694585192eed2ad.png)

对于一些它不熟悉的 API，还是需要在 Prompt 里告诉他要怎样使用。比如剩**下的数据也可以用类似的方**式让 Cha**tGPT 来处理，比如让**他列出：上映时间** 'release_**date'、评分 'vote_average'、评分人数 'vote_count'：

![](img/87b58a76ea465da658e138680c180990.png)

它就能很+

![](img/b18015e30fe1971cf5ad9785a7b9c3a4.png)

我们最后提炼一下 Action 的工作流：

![](img/790c7df29efd5a035a306527a303181f.png)

+   **首先，**想一下你想要做一个什么样的 GPT，以及是否需要外部数据 —— 这两个谁先谁后不重要；

+   **然后，**去你需要的外部数据寻找 API 文档，或者基于你的需求，自己开发一个 API，寻找市面上可以直接用的 Action ；

+   **最后，**基于 API 文档，编写 Action 里的 Schema，和 Prompt（如何处理取回来的信息）

**

写在最**后

![](img/40192756ac68d47a26ca7ad076021c44.png)

**总结一下：**今天我**们的讨论起始****于人工智能**中的“Agent & Action”，然后转向OpenAI 对智能体（Agent）能力模型的定义。接着，我们深入探讨了 ChatGPT 中的 Action（搜索、画图、代码解释器），以及GPT系列中的不同 Action。使用了一个容易上手的 Action Webpilot，用于访问网页获取实时的文本内容。

最后，我们初步了解了 API 的概念，以及 GPT 如何通过 Action 与外部数据进行交互和使用。

如果对 Action 很感兴趣，你可以从以下方向开始继续前进：

+   系统的了解和学习 API 相关的知识

+   去网上寻找可以用的 API 来练习

+   发掘 GPT Action 更多的潜力

以上是我认为相对容易入门的知识框架，但是我们都知道：

**你不可能只在一次分享 / 一篇文**章中就获得完整的 Action 相关知识

学习虽然没有终点，但有阶段性目标。

大家可以通过后续看到的不同的教程和资料，识别这些知识之间的共性和逻辑关系，然后继续深化对这个主题的理解，构建你自己的知识体系。

如果你对 GPT 开发有兴趣，想要认识更多玩 GPT 的朋友，欢迎加入 GPTGeeker 的星球：

![](img/a082a97a53af259ac469d567b451af21.png)