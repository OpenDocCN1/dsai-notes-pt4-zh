# 七、ChatGPT常见问题

> 来源：[https://bwiny5w2svt.feishu.cn/docx/C9dKdpOyboMS2oxOTJCcLml9nef](https://bwiny5w2svt.feishu.cn/docx/C9dKdpOyboMS2oxOTJCcLml9nef)

> **打造最NB**的ChatGPT资料库：如有帮助请收藏并转发

## 如何让chatGPT回答更简洁，更高效？

有时我们只想快速获得答案，对于解释我们不在于，比如数据格式转换，生成函数等。

可以在提示词结尾增加一句：【不需要解释】。

## 为什么ChatGPT回答的质量不高？

有几个原因：

1.  【语料库】中文语料库相比英文没有那么丰富，可以使用英文提问。尤其是代码相关的内容。

1.  【提示词】没有提前设置好提示词Prompt，先给chatGPT预设身份，告诉它的任务是什么。它会更好的理解你的想法。关于提示词的技巧，可以参考这里：https://github.com/f/awesome-chatgpt-prompts#act-as-a-stand-up-comedian

1.  【Prompt 逆向工程】: 发送一篇你喜欢的文章，让chatGPT 分析以下文本的角色、风格、语气、长度、段落和emoji使用等特点，给出可以生成这个文本的 Prompt。这个用来写文章类的内容很好。

下面是逆向工程提取的Prompt

> 请以一位[角色]的角色，结合[商品介绍]，

> 详细推荐一款你觉得非常好用的[商品名称]。

> 描述其关键特征、实用性、使用体验等方面的优点，并分享你在日常生活中如何使用这款[商品名称]。

> 请使用[语气]的语气，以[风格]的风格表达，并在文中适当使用emoji强调重点。

我们结合实际产品来测试一下：

> 角色=数码爱好者

> 商品名称=AirPods Pro二代

> 语气=轻松、热情

> 风格=口语化

> 商品介绍=AirPods Pro 彻底重塑，让音质更丰富更细腻。晋级的主动降噪和自适应通透模式，减少更多外界噪音。空间音频带来非同凡响的个性化沉浸体验。触控操作只需轻扫即可调节音量。单次充电最长能听 6 小时，续航力飞跃。

扩展阅读： 如何更好地向 ChatGPT 提问？

## ChatGPT回答时，经常卡住怎么办？

可以输入命令：继续。它会接着上面的内容继续写。

最好的办法就是把问题拆分。每次让它只回答特定的小问题。控制输出字符。

## chatGPT拒绝回答怎么办？

一些敏感wf问题，由于风控限制，chatGPT会拒绝回答。怎么绕过去呢？

可以换一种提问思路。

chatGPT拒绝回答的问题包括：

+   wf类：

+   爬虫相关

+   zz类的

## ChatGPT有流量和使用限制吗？

网页端的聊天没有流量和次数限制，如果你是开发者，使用API调用有次数限制，每1000个tokens需要花费0.002美元，1000 tokens大概是750单词，相当于75万个单词只要15RMB左右。

但是chatGPT plus由于耗费服务器性能比较大有一定限制。

GPT-4 currently has a cap of 25 messages every 3 hours.

## 为什么账号容易被封？

1.  账号是买的，这种账号基本都是机器批量注册的，容易被封。

1.  代理IP有问题，一个IP太多人用，会被标记高风险IP。

1.  请求IP老变

1.  apikey被盗：太多人使用了你的apikey，发起的请求IP老变，容易被封。

1.  自己经常切换IP

1.  经常询问不恰当的内容，比如：暴力,色情等

## 如何保存和分享与ChatGPT的聊天对话内容？

**一、使用官方原版网站的处理方式**

+   截图插件：chatGPT Scrrenshot, 这是一款专门针对chatGPT开发的截图软件。会把当前对话的所有内容截取成图片

+   分享在线连接：chrome 插件 shareGPT。直接在当前对话框点击分享按钮，可以生成新的标签页链接，可以把这个链接分享给别人。并且可以对这个对话进行评论。

+    更多详细介绍：https://zhuanlan.zhihu.com/p/620160103

**二、使用第三方增强服务**

这些第三方服务已经把截图功能，内置在软件中。具体查看这篇文章：如何拥有更强大的ChatGPT服务？

## 有哪些关于chatGPT优秀的学习资料？

+   chatGPT提供的使用案例：platform.openai.com。官网的资料一般都是最好的，建议把官网demo案例看一篇。

![](img/5b6fb7bd1d147d0505ad07407d73a44e.png)

**相关书籍： **

+   chatGPT亚马逊热销书籍：[点击查看书籍具体介绍 ](https://zhuanlan.zhihu.com/p/622622750)（加博主🛰️领取。中文/英文版本）

![](img/04ff0794b814b191ef7517424da0d839.png)

## 有哪些不错的chatGPT github项目？

+   把chatGPT接入🛰️和🛰️群：https://github.com/zhayujie/chatgpt-on-wechat

## ChatGPT用不了，有哪些平替产品？

推荐：Claude，它也是openAI之前团队成员出来做的产品。

Claude优缺点介绍：

优点：免费、不用🪜，响应速度比较快、也能理解上下文。

缺点：和ChatGPT一样不能联网。

如何安装：

+   先安装slack客户端

+   再安装claude应用

> 它们的关系就像chrome浏览器和chrome浏览器插件的关系。

其他的一些AI产品还有：notionAI（调用GTP3.5接口，有免费数量限制、而且不是对话的形式）、newBing(调用GPT4.0接口，要申请)。

（Calude截图）

![](img/613f1b633ecdd0c0b32459422c374504.png)

## 如何提出一个好问题？

chatGPT是一个超级强大的私人助手，如何提出好问题，让这个助手帮助你提效、获利。取决你问什么样的问题。

爱因斯坦说：提出一个好问题比解决一个问题更难，因为提出一个新问题往往需要打破常规的思路。

## chatGPT服务经常自动断开怎么办？

chatGPT页面如果长时间不使用，再发送问题时，它会报错。

![](img/45a016af54bd15f8a28d0e23d867a7fa.png)

解决方案：

使用这个插件： keepChatGPT:https://greasyfork.org/zh-CN/scripts/462804-keepchatgpt

![](img/fad905c971ae50e3a3e2af046694f88e.png)

## 如何定制属于你自己的ChatGPT私人助理，打造你的数字分身？

前段时间我在抖音上刷到了2个视频，非常震撼。

第一个：数字人模拟乔布斯VS马斯克的主题对话

已世乔布斯和当前科技界的当红炸子鸡 埃隆马斯克，通过数字人的形式进行的一场深度对话，主题是：人工智能与人类的未来。

视频中数字人完美模仿两位大佬的思想、表情，声音。仿佛视频是真的。

**第二个：复活已故奶奶**

通过数字人形式复活自己已故的奶奶，奶奶操着一口浓浓的家乡话。满脸皱纹却又情切无比。真是让人非常感动。

**那是如何实现的呢？**

机器学习了他们所有的相关资料，然后模仿生成。以后只要数据量够大，是不是真的可以以这种方式，实现数字永生？爱你的人，还可以和你聊天，你还会用你的口吻安慰她，逗她开心！

年轻乔布斯与老年马斯克对话 聊AI★人工智能

![](img/7db4bd1fe7fbbd5a7a4e5a86a677d450.png)

小伙用AI复活奶奶，像她生前一样唠嗑语气自然，引发网友巨大争议！

![](img/07f0beae9ee8b0fe41e1b01dda88659c.png)

如果我们也想做一个数字身份，要怎么实现呢？下面说说基本的步骤

1.  身份设定：你是我的数字分身，我将把我所有的私人信息告诉你。你将模仿我的一切，包括：回答的思想，语气等。

1.  资料投喂：把你数字相关的资料都投喂给它，evernote笔记、网易云音乐喜欢的音乐、各大平台发表的文章（知乎、🐧空间、🛰️朋友圈等）、阅读的书籍、抖音喜欢/收藏列表... （技术点：chatGPT接收的token长度有限制，得其他办法优化数据源）

1.  制作分身：通过绘画工具生成数字人模型，通过声音[工具打造你的声](https://chat.openai.com/)音[，通过chatGPT](https://midjourney.com/)生[成回答的](https://studio.d-id.com/)内[容。从思想+](https://mubert.com/)声音+外形三位一体建立你的数字分身。使用的技术：ChatGPT，Midjourney，D-ID，Mubert

案例展示：

在线对话体验地址：https://ai.greatdk.com/

+   [我用我的 10 万条🛰️聊天记录和 280](https://www.v2ex.com/t/931521) 篇博客文章，做了我自己的数字克隆 AI

![](img/ec026e4b08b3cccf36df592a5ea37dfb.png)

扩展阅读；

+   [如何用ChatGPT等工具生成一个开口说话的虚拟数字人｜tips to gene](https://www.youtube.com/watch?v=xNF4Cdju6Pc&ab_channel=%E6%89%80%E9%95%BFbibabo)rate a talking AI human with these tools

## ChatGPT能代替搜索引擎吗，它们各自的优缺点是什么？

ChatGPT 是否能代替搜索引擎取决于使用场景和需求。虽然它们在某些方面存在一定的重叠，但在其他方面，它们各自的优缺点使得它们在不同场景下各有所长。

**ChatGPT**** 优缺点：**

**优点：**

1.  更自然的交互方式：与搜索引擎相比，ChatGPT 提供了一种更自然的人际交流方式。用户可以像与人类交谈一样，用自然语言提问，从而获得所需信息。

例如，当询问 "明天纽约的天气如何？" 时，ChatGPT 可以给出类似 "明天纽约的天气预计为多云，气温在 65-75 华氏度之间" 的回答。

1.  上下文理解：ChatGPT 能够理解用户提问的上下文，为用户提供更为相关和准确的答案。这使得用户在与 ChatGPT 交流时能够获得更为满意的体验。

例如，在讨论莎士比亚时，如果用户问 "他的名言有哪些？"，ChatGPT 会明白 "他" 指的是莎士比亚，并给出相关的名言。

**缺点：**

1.  信息时效性：ChatGPT 的训练数据截至 2021 年 9 月，因此在回答一些涉及实时信息的问题时，它可能无法提供最新的答案。

例如，当询问 "今日股市行情" 时，ChatGPT 无法提供实时的股市信息。

1.  无法展示多个结果：与搜索引擎不同，ChatGPT 通常只提供一个答案，而搜索引擎可以为用户展示多个相关结果，供用户选择。

**搜索引擎优缺点：**

**优点：**

1.  大量信息：搜索引擎可以快速为用户提供大量相关信息。用户可以浏览多个结果，从而找到满足需求的答案。

例如，当搜索 "如何烹饪意大利面" 时，用户可以在搜索结果中找到多个教程，从而选择最适合自己的烹饪方法。

1.  实时更新：搜索引擎可以实时更新信息，因此在查询实时信息时，它们比 ChatGPT 更具优势。

例如，当搜索 "最新新闻" 时，用户可以获得实时更新的新闻报道。

**缺点：**

1.  交互方式较为机械：与 ChatGPT 的自然语言交互方式相比，搜索引擎的交互方式较为机械，用户需要输入关键词并从搜索结果中筛选信息。

1.  缺乏上下文理解：搜索引擎通常不能理解用户查询的上下文。因此，在某些情况下，用户可能需要反复修改关键词以获得满意的答案。

例如，当用户搜索 "苹果" 时，搜索引擎可能会返回关于苹果公司和水果苹果的信息。而在与 ChatGPT 交流时，用户可以明确询问 "苹果公司的创始人是谁？" 或 "苹果水果的营养价值如何？"，从而获得更为准确的答案。

**总结：**

ChatGPT 和搜索引擎在某些方面存在重叠，但它们各自的优缺点使得它们在不同场景下各有所长。ChatGPT 提供了一种更自然的交互方式和上下文理解，但可能无法提供实时信息。而搜索引擎能够提供大量实时更新的信息，但交互方式较为机械且缺乏上下文理解。因此，在选择使用 ChatGPT 还是搜索引擎时，用户应根据自己的需求和使用场景做出决策。

## 你有哪些好用的提示词？

+   知乎爆款标题生成器

+   前端技术专家：帮助我优化代码，修复bug，生成代码片段

+   前端面试题生成器

+   知乎写作高手在线指导文章

+   可视化图形生成器，基于Mermaid语法（支持流程图、时序图等）

+   思维导图格式生成器

+   PPT模板生成器

+   网络营销推广大师：提出营销推广方案，帮我拉流量。

+   图片生成器

+   ...

## ChatGPT网站用不了怎么办，有什么替代方案？送你100个白嫖网站

**背景说明：**

这两天 ChatGPT官方服务很不稳定，无法使用。社群里好几个朋友很着急，找我寻求解决方案。

下面我整理了几个替代方案，方便大家使用。

想要稳定的服务最好还是配置自己的 apikeys，如果额度不夠，可以找我购买。3.5,4.0的接口都有。

**一、使用其他镜像站：**

[免费镜像站集合](https://github.com/LiLittleCat/awesome-free-chatgpt)（将近100个镜像站，有些还是白嫖的）

镜像站的缺点：没有历史记录，刷新就没了

推荐几个不错的镜像站：

1.  高仿chatGPT站点： https://fastgpt.app/

+   可以免费使用对话三次：如果把浏览器缓存清除，可无限使用.

+   可直接配置apikey使用，网站无其他无增值服务。要等免费额度用完才能再配置，否则不会生效。

![](img/84b291d8815577763424df5ccdda399e.png)

1.  加强版类GPT免费服务https://chatbot.theb.ai/#/chat/1002 （推荐）

+   连apikeys都不用配置，免费使用

![](img/d73d4d5ff98d6a65136a0dd2ed012d1a.png)

二、使用客户端配置apikeys使用

需要配置apikeys，更多介绍上面【如何拥有更强大的ChatGPT服务？】这篇内容

1.  [ChatGPT-Desktop](https://github.com/Synaptrix/ChatGPT-Desktop)  【推荐】

1.  [ChatGPT](https://github.com/lencx/ChatGPT)

## 怎么用ChatGPT翻译pdf文件？

最近有几个朋友向我询问如何借用ChatGPT强大的翻译阅读pdf。为此我特意做了调研，并把结果总结如下。

希望对大家有帮助。

下面是一些可以使用的工具：

**一、 immersive**-translate    chrome插件

特点：

+   支持openAI的ApiKeys

+   PDF支持双语显示（排版上稍微有点没对齐），而且点击哪段翻译哪段比较节省apikeys的流量

看看效果

![](img/3aa37e6401bb6aac1537d408d1c08ae9.png)

## 账号被封的几种情况及解决方案？

> 复标题：ChatGPT账号被封了，如何解封，有什么临时使用方案？

背景说明：

这几天 openAI 新一轮的风控来了，很多账号被封，被限制。下面我针对几种情况给大家提供解决方案。

如果实在解决不了，大家可以临时使用一些免费的镜像站：查看一百个免费的ChatGPT镜像站点。

**情况一：没救的情况**

**账号提示被删除 ，**这种是被封了，基本没救了。

![](img/2210f91378ee04f85dd8fe30304aae37.png)

下面两种情况可以通过换 清空浏览器缓存或者使用浏览器隐身模式+ 换IP来解决

报错一：左侧的历史记录没有了，并且无法发送消息

![](img/697f491e9a2047b9d77b9945de25e1f1.png)

查看接口时会发现报错

![](img/ec22cf73ab01dcc81d309b0c09e521de.png)

**报错二：登陆时被阻止了**

![](img/d6fe7865d98904c1fef45887c8b53c0b.png)

换IP和情况浏览器缓存的一些技巧

下面以chrome 浏览器为例，每次失败后，最好都把缓存给清除掉，或者使用隐身模式。

一般只要失败过一次，浏览器就会记住之前的状态 。

![](img/4fc01cf49c1e67178c624851e406847c.png)

![](img/186a79b6247b6608d48e558af083124b.png)

## ChatGPT 上下文太少怎么办？

ChatGPT token 一般只有 8k，企业版本可以申请到32k的，但有时还是太少，不太夠用怎么办？

下面是突破token长度限制的一些方法：

1.  分段生成：首先生成GPT大纲，再逐一完善各子主题。

1.  定制指令：借助Custom Instructions功能，使GPT的设定和目标具有持久性。

1.  代码解释器：利用code interpreter超越token长度。将大纲和子主题摘要（简短为佳）储存在Python内存中，以此为基础产生新主题。但请注意，因Python的虚拟内容为临时存储，长时作业时应将内容保存为txt文件并存储至本地。

补充：Claude2支持的上下文可达100k，可作为另一选择。

**扩展资料：**

+   [ ChatGPT新功能揭秘：Custom Instruc](https://www.youtube.com/watch?v=wqGZ5sQAdD8&ab_channel=AI%E5%AD%A6%E9%95%BF%E5%B0%8F%E6%9E%97)tions，让AI更懂你，提升效率神器！

+   [破解ChatGPT Token限制](https://www.youtube.com/watch?v=Hq598OF_5A8&ab_channel=AI%E5%AD%A6%E9%95%BF%E5%B0%8F%E6%9E%97)，最大化利用代码解释器，从此无惧长对话！

## 有哪些不错的GPTs学习资料？

GPTs学习资源：

1、GPTs导航[：https://gptstore.ai](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbXN5Wi0zdndQbFNBcHE2QkF3M0h0SnZyU2RDUXxBQ3Jtc0tsbGx2a3RPS25LTlN6OUp2al9xUFdyTkFDY09QSUEwdXlwU1p1a3hnbnRsRzlJMXBURW1lNEtsQWd4MnlKSkJSdjhrTUUxQ0VHUkJlaEpLakdlS201X1JhQjBmQXhadUxpeFFvV2MzT2tEdDRwdlNDTQ&q=https%3A%2F%2Fgithub.com%2FLouisShark%2Fchatgpt_system_prompt&v=ZtVh2RDFSkQ)/

2、GPTs导[航：https://www.gptshu](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0RoQ0R5TVZScC1XaHA0LUJLLWQtZUt4bWEwQXxBQ3Jtc0tuTjB3cjk2ZjhJUkVaLURxcEktRlpnX1d1aFd0Z1RPZUVBZzhGdXJPZWk0S3BDWTdrM096TjJUOFgtZHhSbGJ6VmxFa2FMQjhoYzIzeU1BVXBLWXByTzlCWWZhNTYyNEROb0FYdld0alI5czY0MTQ3SQ&q=https%3A%2F%2Fgithub.com%2FEmbraceAGI%2FAwesome-AI-GPTs&v=ZtVh2RDFSkQ)nter.com/

3、ChatGPT系统Prompt：https://github.com/LouisShark/chatgpt...

4、GPTs相关资源：https://github.com/EmbraceAGI/Awesome...

## 如何让GPTs返回我的🛰️二维码图片？

分两步：

1.   把图片上传到服务器，生成一个图片url地址，比如：花瓣

1.  在提示词让GPTs 以markdown格式显示图片

GPTs系统提示词：

```
你是佐老板的AI分身，当用户向你询问佐老板的微信号时，请以markdown格式返回这张图片，
https://gd-hbimg.huaban.com/574359e9e71d07e554dd6eb35936985de95625c22c011-4J3e1G_fw1200webp
```

# 开发相关

## 什么是ChatGPT ApiKeys，如何获取，有哪些注意事项？

大部分人使用ChatGPT一般都是在官网上。

但是如果我想把ChatGPT的强大功能接入到其他软件，那应该怎么做呢？

比如我之前写的两个应用场景：

+   3分钟极速、免费搭建自己的ChatGPT网站，再也不用🪜了

+   ChatGPT插件+Webstorm效率再翻翻，快的飞起来

这里就需要调用ChatGPT的接口，调用接口也非常简单，只需要一个openAI中的 ApiKeys。

**获取APIKeys**步骤说明：

1.  登陆到 ** **[**openAI开发平台**](https://platform.openai.com/)** 。 **

> 提醒：登陆时请选择和注册时一样的登陆方式，否则无法进入。比如：邮箱登陆、第三方授权登陆。

1.  进入个人中心 查看Api keys

1.  如果没有就创建一个，特殊说明：apikeys 创建成功后只会显示一次，所以要妥善保管，不能泄露了。

**特殊说明：**

+   一般新注册的账号都会送 18美金的免费体验额度，用完需要付费。

+   免费额度是有有效期的，如果集成到你应用中报错了，记得检查是否过期。或者额度是否用完。

**openai****开发平台入口**

![](img/8d7c5a8ab8185dfb3b195d954051c6bd.png)

获取apikeys，创建与获取

![](img/53d484ccf654d6a5a6e5eb44f3329164.png)

![](img/1ce1a90741ec3f0e3e677997700e31e8.png)

![](img/193656634373f4bba86f437ef4f5c1eb.png)

**注意流量的使用情况：**https://platform.openai.com/account/usage

![](img/ffa0216336888814d5572b05a2150ead.png)

## 通过HTTP请求怎么调用ChatGPT的接口？

推荐使用这个库：https://github.com/transitive-bullshit/chatgpt-api

## 如何获取ChatGPT4.0的接口？

申请入口：https://openai.com/waitlist/gpt-4-api

提交申请完成之后，就开始等待邮件通知了。

![](img/e739da9ec0d886af0e35a55502231ae3.png)

## openAI开放的接口模型，各个版本有什么区别？

当我们在使用第三方客户端时，会发现上面显示会使用的gpt模型，它们有什么区别呢？

gpt-3.5 和 （gpt-3.5-turbo，gpt-3.5-turbo-0301）模型有什么区别？

后面两个模型是在 gpt-3.5模型上优化而来的，主要有下面两个优点：

+   反应更快

+   更便宜：便宜10倍，只需0.002美元每1000个token。

gpt-3.5-turbo，gpt-3.5-turbo-0301之间的区别如下：

| 模型 | 需要在content指明具体的角色和问题内容 | 维护迭代 |
| --- | --- | --- |
| gpt-3.5-turbo | 需要指定 | 会持续更新 |
| gpt-3.5-turbo-0301 | 不需要指定 | 临时版本，至少支持到6月1日 |

## 还有哪些AI工具？

绘图类的：

阿里的通义千问 （注册可免费使用50次）：https://wanxiang.aliyun.com/creation

![](img/0a3c48f2607daf8e7a1d08aef1960a09.png)

# 用哪些常用 的AI工具？

针对刚毕业的大学生，怎么开始入门AI

1.  **下载AI工具进行体验**

+   ChatGPT

+   豆包：内置角色很多，语音对话不错

+   智谱清言：支持文生图片

+   悠船：https://www.youchuan.cn/  (midjourney 国内版本， 支持文本生成图片)

+   其他办公的AI工具

+   AI做PPT： [gamma](http://gamma.app) （注册送积分，可以多注册几个账号，也可以在淘宝上面购买）

+   数字人软件：heygen

更多AI工具： https://ai-bot.cn/ai-app-store/

1.  **购买一门AI课程系统学习**

可以在一些平台找合适的课程

1.  **学会一些AI视频剪辑工具**

+   开拍：拍摄视频，自动加字幕，视频剪辑..

+   剪映：自动加字幕、声音朗读、声音克隆、数字人播报...

** - ****ChatGPT****让每个普通人，不再平庸 -**

链接博主+免费AI社群+商务合作                                                      👸号：AI超体进化室

![](img/c161dc2f9ad39a79fe81b5122797a3ad.png)

![](img/4becb7f606b3ebbf9398f0dd438cfb38.png)