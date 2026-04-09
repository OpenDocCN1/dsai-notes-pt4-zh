# Bot智能体 | 用Coze实现【多模态资讯的跨平台推送】

> 来源：[https://waytoagi.feishu.cn/docx/OtApdNgPvo8FyrxGo2ucHDaKnce](https://waytoagi.feishu.cn/docx/OtApdNgPvo8FyrxGo2ucHDaKnce)

> 原文链接：https://developer.volcengine.com/articles/7388464468457750591

> 作者: AI研究室-宇哥

# **一【Bot 简介】**

在信息爆炸的时代，怎么破除信息茧房，高效获取到一手的最新资讯，实时跟进各行业的发展动向无疑是很重要的，同时也具有挑战的一件事情。现在有了扣子这样的AI agent产品，我们可以快速去搭建一个bot智能体，根据每个人不同需求和情况，去完成各种最新最热的资讯抓取，整理成文并推送到多个平台进行交互。真正体现AI Agent跨平台，多模态的自动化价值。

Bot视频演示地址: [https://www.bilibili.com/video/BV1RLaGeAE7Z/?vd](https://www.bilibili.com/video/BV1RLaGeAE7Z/?vd_source=baeb68724c9458d179387d4bce43931c)_source=baeb68724c9458d179387d4bce43931c

Bot体验地址: [https://www.coze.cn/store/bot/73868](https://www.coze.cn/store/bot/7386884942153957391?panel=1&bid=6d2j6kkj04019)84942153957391?panel=1&bid=6d2j6kkj04019

# **二【Bot 构思与效果】**

#### **1.创意构思**

创建这样一个Bot的初衷是让每个人都可以拥有一个专属的市场和资讯的助手，帮你把最新的一手资讯以最快最好的方式整理呈现出来，更加精准有效地筛选出有价值的信息。

![](img/55abad6a5ef75274cfa634a09c56d53e.png)

如果只是单独使用大语言模型，我们只能获取检索到它学到封装好的，以往公共领域的知识，却没法通过它直接获取到目前实时更新的，最新鲜的信息。但是通过coze这样的AI agent流程，可以轻松的获取到最新的资讯信息，并以多模态的形式（文字，图像，文档，语音等）自动推送到不同的平台上。

该Bot项目以扣子为中心平台，通过各种自研的插件，自研工作流和Coze API的创建链接了微信群，企业微信群，飞书云文档多维表格，实现不同平台的信息传递与流通。可根据用户定制化的检索需求（可在Bot或者微信群中以对话的方式直接输入需求或者获取飞书表格中的用户需求），去自动化抓取热点资讯，进一步做信息的分析处理，整合成资讯文档，返回bot对话界面。同步自动发送markdown文字和文档到微信群，企业微信群等不同的平台，让用户能够更加方便的在平时使用的聊天和沟通平台上去交互获取到有价值的一手信息。

特点：

1、实现跨平台联动，多模态形式内容的自动化传递；

2、针对不同客户的需求去进行检索；

3、检索内容的分析、总结和处理；

4、整理后的资讯按交互友好的格式输出，自动化生成资讯文档。

#### **2.效果呈现**

Coze Bot的调试与预览，通过不同的提问去触发不同的检索功能

![](img/6d07617bbaa32d0ba78fed95f23982c0.png)

企业微信群自动同步获得资讯检索的结果，实现多平台联动

![](img/5cbcf8829d98fc59b2ae417fdb918d98.png)

获取的资讯文档具体内容

![](img/1a66aa51401706c5be8bd6a61679652c.png)

Coze与企业微信群的信息同步联动

![](img/8f67e6826d4eec18cde330ba393ea6c8.png)

![](img/a9081376a2ab88a07000dd603c54a80b.png)

Coze Bot获取到飞书云文档多维表格中的用户需求

![](img/de509c6cdd4f7bf907b5175d23307ea6.png)

Coze API接入微信，在微信群聊中可以调用Coze Bot进行对话交互去检索相关需求的信息。

![](img/5dbbf9079a2b0a7f625f0eac7d1d3738.png)

![](img/f846d7d954961df6b97848af0972860d.png)

# **三【技术实现原理】**

## 1.Bot 搭建逻辑及编排界面介绍

Bot的创建部分包括3个自研插件及4个自研工作流，人设与回复逻辑，变量记忆，长期记忆，数据库，开场白，用户问题建议，快捷指令，语音等功能。其中工作量最大，也最具有难度和挑战的部分是自研插件和工作流的搭建，也是本Bot项目的核心部分。

![](img/7b474cec23e0c6132a1afa32e1f21b17.png)

## **2.**Bot 组成部分：

### 2.1 人设与回复逻辑

设定Bot智能体的角色，对4个工作流的详细描述和调用的规则（不同情况调用不同工作流去完成任务），其他创建的信息等等。

![](img/88632ea873879056aa3442935d80fdc8.png)

### 2.2 插件

插件已经结合具体流程内置到各个工作流当中，包括3个自研插件，不需要单独在编排界面中配置。

### 2.3 工作流

Bot由4个不同的工作流组成，在不同的情况下，Bot会根据用户的输入去调用不同的工作流完成自动化的任务。

![](img/0cda3f6f8f6c722816e835258cc0d2cb.png)

+   当用户“在Bot中直接输入查询需求去抓取热点”时，采用 hotspot_direct_output 工具。

+   当用户“在Bot中直接输入查询需求去抓取热点，并要求同时将结果发送企业微信群”时，

使用 hotspot_bot_file_wechat工具。

+   当用户“需要获取飞书中客户的查询需求去抓取热点，将结果发送企业微信群聊”时，采用

hotspot_feishu_file_wechat 工具。

+   当用户“需要提供检索需求和URL网页链接去抓取相关信息，将结果发送企业微信群聊”

时，采用 search_bot_file_wechat 工具。

实现从简单到复杂的不同流程的自动化，单一需求，单个平台界面的关键词检索，到多平台获取用户需求和发送结果（ 飞书 -coze-微信）。

### 2.4 变量、开场白、快捷指令设置

通过变量的设置，与工作流中的信息进行交互联动，方便后续的调用。开场白设置提供参考对话模板，提升用户交互体验。

![](img/637a2e0a89d850a1ef4e882e3ab38a21.png)

## **3.自研插件创建过程**

### 3.1 企业微信群聊机器人

共包括四个不同的插件，可以做到向特定的企业微信群发送文字消息，markdown文字，图文，文件等不同形式的内容。这里重点以其中发送文件，sent_file_message的插件来讲解。

![](img/1cbe99c8a0ca5e02007ffc6886215c9f.png)

首先，到语聚ai的第三方api集成平台上添加工具动作；

![](img/29897ca3c959cb8e5ffc55f3be488874.png)

接着在平台上测试相关动作，获得返回的API请求的python代码；

![](img/4fcf793aca5b51b60397fff48d672dd4.png)

最后按步骤集成到coze的插件创建平台中。

![](img/0dc97d8190f8ea5bdb52e6a4bc362c78.png)

![](img/4ba2a2855ff8b148e1a6927a7ddf96dc.png)

![](img/44caad6fa4798f954a9d8a6e9939b03c.png)

![](img/efe8d869cc5a32e7cc9154e84561d31f.png)

![](img/4f65e458bbc9230bce71dbd034f52dae.png)

![](img/6926b7dd3134d1b24e7e8266759fdb22.png)

### 3.2 飞书多维表格

使用coze在 Coze IDE中创建模式创建插件，实现获取飞书特定表格最新记录特定字段的内容

![](img/e3c9ebf4125ecfd9be48db23212231cb.png)

根据飞书开放者文档的要求在coze IDE平台中用handler的方式编写python代码，配置项目依赖

![](img/60d0946106e632f9021c6e974fbbfa2d.png)

metadata中配置输入和输入端信息

![](img/3bd03690f572383cb28731226a76ebe3.png)

最后测试发布成功

## **4.自研工作流搭建过程**

### 4.1 hotspot_direct_output（抓取热点-bot中用户需求-直接返回结果）

![](img/124baea5585c565107365d9bfcd3b923.png)

![](img/18846fc28f7d63bf47b3ea4c6683f9c3.png)

![](img/8419094f39dbf8c79b7d32b77f8bd740.png)

![](img/6d987251b3057b58c24d6ed2e4d6d26a.png)

### 4.2 hotspot_bot_file_wechat（抓取热点-bot中用户需求-发送资讯文档到企业微信群）

![](img/844aed9b9459623f7327dfdad02dab6a.png)

前面几步和工作流1类似，接下来如下图操作

![](img/8a5dba6eda3b312552c0277bdaa3e116.png)

### 4.3 hotspot_feishu_file_wechat（抓取热点-飞书表格用户需求-发送资讯文档到企业微信群）

![](img/9a81b8a7f0101db1cbb7ad26d3b31cab.png)

前面几步和工作流1类似，接下来如下图操作

![](img/00a61fafd5d87b7322e8791531ceabe1.png)

![](img/0c9759310abd20b02e566ef4633677a2.png)

### 4.4 search_bot_file_wechat（用户需求+网址提供- 网页内容总结- 全网搜索其他相关信息-生

1.  成信息文档返回 - 同步发送到企业微信群）

![](img/99a20fc328eb2c9a6f140590803b84f9.png)

![](img/6785d6263e9773b68d6f135f23f6da32.png)

![](img/fc90d3e9e63b9d005992e90d0935b99f.png)

![](img/d364c732c21130be8c58f60b5aac5967.png)

## 5.Coze API接入微信群Bot

通过调用Coze API的方式将coze的该多平台资讯推送的Bot机器人接入到微信和微信群当中，让客户能够方便的在微信群里和Bot进行交互，进行信息的高效检索和整理。

### 5.1 Coze API 设置

首先通过Bot的创建页面上端网址bot字段后BotID的数字获取到

![](img/34ac786a59f9bfb619425b008be993ec.png)

然后点击Bot发布的按钮后在页面的最下端勾选上Bot as API

![](img/39a0db6d9e8dd83b9cadf3476cc17456.png)

点击扣子API按键

![](img/918ba6ad20a98f4a684ffc5ca3b5c27e.png)

创建API令牌并获取到API密钥

![](img/ed8b04f0beca9c4a75b7a7ede87370ef.png)

这一步保持好以上的信息，之后在5.3当中需要用到BotID和Coze API

### 5.2 云服务器创建

在腾讯云服务上选择一个优惠便宜的服务器进行购买

![](img/a8bd0ba65247f180feed473ea60a6bd4.png)

选择宝塔Linux面板的镜像

![](img/deb72ff699218723420e049d4c7235c8.png)

### 5.3 Docker中配置COW组件

登录宝塔面板后，接下来需要在宝塔面板当中的来可视化的控制云服务器，在上面部署docker容器，启动COW项目与微信取得关联。

![](img/c9b3665d4e2f41a45b64991c834b59cc.png)

chatgpt-on-wechat（简称CoW）项目是基于大模型的智能对话机器人，可以接入微信公众号、企业微信应用、飞书、钉钉，可选择GPT3.5/GPT4.0/GPT4o/Claude/Gemini/文心一言/讯飞星火/通义千问/LinkAI/ChatGLM/月之暗面，能处理文本、语音和图片，通过插件访问操作系统和互联网等外部资源。我们用这个方式来与微信进行链接。

点击“Docker”中的“项目模板”中的“添加”按钮

![](img/c11f258d3846f3bb316a4978ec760817.png)

项目模板代码示例如下：

```
version: &quot;3&quot;
services:
  wc-andy-ai-bot:
    image: zhayujie/chatgpt-on-wechat
    container_name: wc-andy-ai-bot
    restart: always
    security_opt:
      - seccomp:unconfined
    environment:
      OPEN_AI_API_KEY: &quot;pat_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&quot;  &quot;填上你的Coze API&quot;
      MODEL: &quot;&quot;
      OPEN_AI_API_BASE: &quot;http://coze2openai:3000/v1&quot;
      PROXY: &quot;&quot;
      SINGLE_CHAT_PREFIX: &#x27;[&quot;&quot;]&#x27;
      SINGLE_CHAT_REPLY_PREFIX: &quot;[AI] &quot;
      GROUP_CHAT_PREFIX: &#x27;[&quot;@宇哥AI助手&quot;]&#x27;
      GROUP_NAME_WHITE_LIST: &#x27;[&quot;每日资讯推送&quot;]&#x27;
      IMAGE_CREATE_PREFIX: &#x27;[&quot;Drawing&quot;]&#x27;
      CONVERSATION_MAX_TOKENS: 8000
      SPEECH_RECOGNITION: &quot;False&quot;
      CHARACTER_DESC: &quot;你是基于AI研究室-宇哥的大语言模型智能助手，旨在回答并解决人们的资讯检索与查询需求，并且可以使用多种语言与人交流。&quot;
      EXPIRES_IN_SECONDS: 3600
      USE_GLOBAL_PLUGIN_CONFIG: &quot;True&quot;
      USE_LINKAI: &quot;False&quot;
      LINKAI_API_KEY: &quot;&quot;
      LINKAI_APP_CODE: &quot;&quot;
  coze2openai:
    image: andywuwu/coze2openai:latest
    container_name: coze2openai
    restart: always
    ports:
      - &quot;3000:3000&quot;
    environment:
      BOT_ID: &quot;xxxxxxxxxxxxxxxxxxx&quot; &quot;填上你的Coze BotID&quot;
      COZE_API_BASE: &quot;api.coze.cn&quot;
```

将上面编译好的内容复制进来

![](img/acfdede77894451ed5a58db5bf6fca15.png)

在容器中创建容器，选择容器编排，填入模板和名称，确定

![](img/0256afa4eea8cb9e9b1db85c9ec3b598.png)

现实运行成功

![](img/030a6aff4a35ccebd502f8968218906c.png)

点击容器后，可以看到运行的是两个服务，因为接口文档中启动了两个服务。

![](img/6f23beebc398b0bb7cc0bff46f69f0c7.png)

然后点击“wc-andy-ai-bot”后面的日志按钮，在弹出层中用提前预备好的微信进行扫码。

![](img/9d6feff1343877cc3bef2ea4acdf8acb.png)

这个界面不会实时更新显示，需要手动刷新一下。点击“刷新日志”，如果看到 WeChat login success，就成功将Bot接入微信中了。

![](img/da0317956bb42869711058dac21c0a90.png)

# **四【商业化场景】**

### **1.人工智能（AI）技术跟进**

在快速发展的AI领域，技术的更新迭代非常迅速。该Coze bot可以帮助AI研究人员和开发者跟进最新的研究成果、技术动态和行业报告。通过自动化检索和多平台推送，研究团队可以在第一时间获取到前沿资讯，从而快速调整研究方向和策略。

Coze视频链接:[htt](https://www.bilibili.com/video/BV1pm421L7fu/?spm_id_from=333.337.search-card.all.click&vd_source=baeb68724c9458d179387d4bce43931c)ps://www.bilibili.com/video/BV1pm421L7fu/?spm_id_from=333.337.search-card.all.click&vd_source=baeb68724c9458d179387d4bce43931c

应用效果：

+   实时更新AI技术动态，保持技术领先

+   汇总行业报告，支持研发决策

+   多平台推送，提升团队沟通效率

### **2.金融投资领域**

在金融投资领域，获取及时、准确的市场资讯和财务分析是投资决策的关键。通过该Coze bot，金融分析师和投资者可以自动接收最新的市场动态、股市新闻和财务报告。bot可以从多个金融资讯平台抓取数据，生成分析报告，并将这些报告推送到用户常用的交流平台，如企业微信群或飞书文档。

应用效果：

+   实时跟踪市场动向，捕捉投资机会

+   自动生成财务报告，节省人力成本

+   多平台同步推送，方便团队协作

### **3.医疗健康行业**

在医疗健康领域，及时获取最新的医学研究、政策变化和行业动态至关重要。该Coze bot可以为医疗机构、医生和研究人员提供最新的医学资讯和研究成果，并通过多模态形式推送到他们常用的平台，帮助他们更好地了解行业趋势，提升医疗水平。

应用效果：

+   实时获取医学研究和政策变化

+   自动整理医学报告，提升工作效率

+   多平台推送，方便医疗团队交流

### **4.教育培训行业**

在教育培训行业，了解最新的教育政策、教学方法和培训课程是提升教学质量的关键。Coze bot可以为教育工作者和培训机构提供最新的教育资讯和课程动态，并自动生成教学报告和资料，推送到相关平台，帮助他们更好地规划教学内容和课程设计。

应用效果：

+   实时更新教育政策和教学方法

+   自动整理教学报告，提升教学效率

+   多平台推送，便于教学团队沟通

### **5.科技创业与孵化器**

科技创业公司和孵化器需要实时掌握最新的科技动态和创业资讯。该Coze bot可以帮助他们获取最新的科技新闻、创业报告和行业分析，并通过多模态形式推送到他们常用的平台，帮助他们更好地了解市场需求，调整创业方向。

应用效果：

+   实时获取科技动态和创业资讯

+   自动生成创业报告，支持决策

+   多平台推送，提升团队协作

# **五【未来展望】**

随着AI技术的不断进步和市场需求的不断变化，该Coze bot将不断优化和升级，实现更加智能和个性化的资讯推送服务。未来，我们将进一步拓展该bot的应用场景，覆盖更多行业和领域，为用户提供更加全面和及时的资讯服务，助力各行业的发展与创新。

Bot体验地址: [https://www.coze.cn/store/bot/73868](https://www.coze.cn/store/bot/7386884942153957391?panel=1&bid=6d2j6kkj04019)84942153957391?panel=1&bid=6d2j6kkj04019

#