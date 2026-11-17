
# Azure OpenAI API 微软中国区申请

> 来源：[https://my.feishu.cn/docx/CCJFdpcuZo6JtWxfvNRcGHLdnMd](https://my.feishu.cn/docx/CCJFdpcuZo6JtWxfvNRcGHLdnMd)

早在 2021 年 11 月，微软 Azure 就推出了 OpenAI 预览服务，允许企业用户通过 REST API 去访问 OpenAI 的语言模型，当时也只有非常有限的客户可以预览这个功能。在今年 1 月份，微软宣布 Azure OpenAI 面向全球客户正式开放，其中就包括了中国区，全球的企业用户都可以访问到世界先进的人工智能模型，可以使用 GPT-3.5、Codex、DALL E2 在内的人工智能模型，并可使用 API 打造自己的智能化创新应用。

人工智能是未来的趋势，但眼下 ChatGPT 对国内用户非常不友好，不仅申请麻烦，需要使用专用代理服务器，接验证码还得去注册接码平台的，而且在使用过程中还磕磕碰碰，一不小心没开代理调用了 API 接口，就有可能面临被封接口权限甚至直接封号。那么在国内有没有没那么麻烦的 AI 人工智能模型可以用呢？也许你会说百度有文心一言，Google 也有 Bard，还有 Notion AI，这些都是行业内的竞争者，但我认为就目前的体验来说，OpenAI ChatGPT 仍然是最优选。

因此，在本篇文章中，将简要介绍一下如何申请中国区 Azure OpenAI 服务，并以此代替原本 ChatGPT 的功能，在一些需要使用 OpenAI API 的场景中，可以直接替换。

## 目录

## 注册微软 Azure 用户

注册之前，建议先去注册一下[微软](https://azure.microsoft.com/zh-cn/)的邮箱，全球收发邮件都不受阻碍，而且有行业领先反垃圾邮件的系统，建议可以长期使用。邮箱的注册这里不展开。我们直接访问微软 Azure 平台进行申请。

![](img/7a26023b0e35fad6f505b94ca34e5d7e.png)

注意：中国区 Azure 是由“世纪互联”运营的，如果你填写资料时选择的国家是中国，那么就是由国内提供服务的。如果你选择了其他国家/地区，则是由微软国际版 Azure 运营的。

中国区 Azure 和国际版 Azure 存在一些政策上和使用上的差异，如果你只是为了体验 Azure OpenAI 的 API，那么没差别，申请下来的接口都可以用。

噢，对了，Azure 申请成功后，会告诉你有一些服务可以免费用 12 个月，也有一定数额的赠金。

我由于已经有 Azure 账号了，就不去申请再多的账号，直接跳到下一步，申请 API 接口。

## 申请 OpenAI API 接口权限

登陆 Azure 之后，直接在顶部搜索关键词“openai”，就可以找到

然后点击“Azure OpenAI”进入接口创建页面，在底下有提示，点击“创建 Azure OpenAI”

![](img/d53f249a9973975eeaae5831fe364db9.png)

在这个页面中，选择你的订阅，如果你刚注册的，应该就是免费试用的订阅，然后再创建一个资源组用于 OpenAI，这里就起名“OpenAI”好了

![](img/dee02f3daf13ff4d3666164ad45deab4.png)

注意看这个红框位置，OpenAI 服务需要额外申请，点击申请。会弹出一个多达 25 道题的问卷，需要仔细填好。

注意，这里要提供准确的订阅 ID

![](img/98830fefb39f89b1cbc506dc24175f8b.png)

这个 ID 在这里可以找到，注意这里不是目录 ID（租户 ID），而是要提交订阅 ID

在顶部搜索“订阅”，在打开的页面中，把订阅 ID 复制到上面的问卷中

![](img/4e7e72cca04aeb8d4e183821e2c2833f.png)

![](img/116e3561acbe1f4dbef82abc3dd628e7.png)

还有这个公司信息，不论国内版 Azure 还是国际版 Azure，OpenAI 都只面向企业服务，所以这里要填你的公司信息和公司邮箱，如果你使用一些常用的私人邮箱，申请会即刻被拒绝。

![](img/eed4e145a392ae9f62a0db5c0963df6a.png)

把这二十几道问卷题答完，就等着审核吧。大概要几天到十几天不等，最近申请人数开始增多，可能会延期。

![](img/e57f2883c585a58bb8cb2c2512c8ca3f.png)

我申请了有几天了，刚好又是周末，还没下来，先把大纲写了

等了有几天了，公司的邮箱收到一份验证邮件，复制链接到浏览器验证一下。

![](img/95c766a4d7c53131c1279b3a4e4c861f.png)

昨天凌晨收到邮件，拿到权限了！嘿嘿～

![](img/37a7050fccd58a81d0a8161b403aba06.png)

齐活了，可以开始接下来的配置。

## 配置 Azure OpenAI

打开这个页面：`https://portal.azure.com/?quickstart=true#create/Microsoft.CognitiveServicesOpenAI`

![](img/197f9a8096481443f873f3e97accf7a0.png)

选完订阅、资源组，再选择好区域、名称、定价层，直接下一步就好了。

![](img/3fc506b637019d5db780ae2bd2b974c6.png)

点击创建后，需要等待状态由 Creating 变为 Succeeded，这样就算部署完成了。

![](img/6492892252c2773ac4ab4c77d924559d.png)

然后点击这个资源，配置所需的资源版本，目前是 GPT-3.5，如下图所示：

Azure OpenAI 目前所使用的版本只有 GPT-3.5，如果需要 GPT-4，只能去申请 ChatGPT，直接开 Plus 订阅就有了。不过 GPT-3.5 已经足够使用啦。

创建完模型部署之后，就可以用了。

这时可以直接去 ChatGPT PlayGround 愉快玩耍了，你可以问它一些问题，就像 OpenAI ChatGPT 那样。

![](img/3cd0188487e445e5a9cbc3ced7d5d2fd.png)

![](img/32c852e569b4acf0a05e89d03a45d260.png)

在打开的页面中，可以看到有一些优化选项：

![](img/65bbfe0a0ab7ef56ac0bc58157f9b5ed.png)

开始调戏一下吧：

## 申请 OpenAI API

我们最终是需要申请得到 API 的使用权限的，所以回到刚刚的资源页面概览面板

![](img/f1e4b4fcb06328b6eaed8e62d0d2a2c0.png)

在概览面板，那个“终结点”（英文：endpoint）要记起来，后续对接需要用。或者直接在下面选开发 tab，也可以看到关于 api 的一些介绍。主要用到就是密钥 1，密钥 2 的用途看官方的介绍，其实就是个备用。功能一样。
