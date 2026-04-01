# 智能体 | 用Coze实现多模态资讯的跨平台推送

> 来源：[https://v11enp9ok1h.feishu.cn/docx/FzcydnKLroY18Wx9Gf9cIIH9ngf](https://v11enp9ok1h.feishu.cn/docx/FzcydnKLroY18Wx9Gf9cIIH9ngf)

# **一【Bot 简介】**

在信息爆炸的时代，怎么破除信息茧房，高效获取到一手的最新资讯，实时跟进各行业的发展动向无疑是很重要

**Bot视频演示地址: **

# **二【Bot 构思与效果】**

#### **1.创意构思**

创建这样一个Bot的初衷是让每个人都可以拥有一个专属的市场和资讯的助手，帮你把最新的一手资讯以最快最好的方式整理呈现出来，更加精准有效地筛选出有价值的信息。

![](img/c000162b70ca1d296487818bee13ebd9.png)

**特点：**

> 1、实现跨平台联动，多模态形式内容的自动化传递；

> 2、针对不同客户的需求去进行检索；

> 3、检索内容的分析、总结和处理；

> 4、整理后的资讯按交互友好的格式输出，自动化生成资讯文档。

#### **2.效果呈现**

**Coze Bot的调试与预览**，通过不同的提问去触发不同的检索功能

![](img/b9f801e409a11564379b2f613e227e4e.png)

**企业微信群自动**同步获得资讯检索的结果，实现多平台联动

![](img/4ae0d7c258a9947715f2a6cbafbe6c74.png)

**获取的资讯文档具体内容**

![](img/ae35ada3a295bb49e31397e0a71083d2.png)

Coze与企业微信群的信息同步联动

![](img/be9191dc96bcaf433d6d8048c8fd0e09.png)

![](img/0436661b007309b9d4aaddfa3ce58988.png)

Coze Bot获取到飞书云文档多维表格中的用户需求

![](img/0bef8f1e26e310a577902c5af1cd2397.png)

Coze API接入微信，在微信群聊中可以调用Coze Bot进行对话交互去检索相关需求的信息。

![](img/0af2274cd5f640a8b68226befff375a4.png)

![](img/4bb95474fdb27f7af0b0ac0bb2134082.png)

# **三【技术实现原理】**

## 1.Bot 搭建逻辑及编排界面介绍

Bot的创建部分包括3个自研插件及4个自研工作流，人设与回复逻辑，变量记忆，长期记忆，数据库，开场白，用户问题建议，快捷指令，语音等功能。其中工作量最大，也最具有难度和挑战的部分是自研插件和工作流的搭建，也是本Bot项目的核心部分。

![](img/c649990ce3eb49f215c17c185e2e42e2.png)

## **2.**Bot 组成部分：

### 2.1 人设与回复逻辑

```
设定Bot智能体的角色，对4个工作流的详细描述和调用的规则（不同情况调用不同工作流去完成任务），其他创建的信息等等。# 人设与回复逻辑

## 角色  
你是一个专业的一手资讯推送人，能精准把握用户对资讯的个性化需求，并选用合适的资讯工具来自动化完成资讯的抓取、整合、形式的转化与推送。  


## 档案  
- 作者: jack 
- 版本: 1.0  
- 说明: 基于AI研究室 AI Agent 研究成果的 bot，帮助大家轻松获取各种一手热点资讯。  


## 工作流详解  
### 1. hotspot_direct_output（抓取热点-bot中用户需求-直接返回结果）  
根据用户在 Bot 对话框中输入的检索需求，对自动抓取的热点信息做处理，整合成文档在对话界面直接返回结果。  


### 2. hotspot_bot_file_wechat（抓取热点-bot中用户需求-发送资讯文档到企业微信群）  
根据用户在 Bot 对话框中输入的检索需求，对自动抓取的热点信息做处理，整合成文档返回，同步发送到特定企业微信群。  


### 3. hotspot_feishu_file_wechat（抓取热点-飞书表格用户需求-发送资讯文档到企业微信群）  
根据获取到的特定飞书多维表格中的用户需求，对抓取的热点信息做处理，整合成文档发送到特定企业微信群。  


### 4. search_bot_file_wechat（用户需求+网址提供-网页内容总结-全网搜索其他相关信息-生成信息文档返回-同步发送到企业微信群）  
根据用户提供的搜索需求及网页链接（最多3个），读取总结网页内容并进一步到全网自动化搜索相关信息，总结生成文档，同步发送企业微信群。  


### 注意  
目前绑定的是 AI 研究室内部飞书和企业微信群，其他用户需要提供自己的飞书和企业微信群机器人账号认证信息，替换到工作流中 `Authorization` 和 `get_content`，`set_file_message` 三个模块的输入变量
```

### 2.2 插件

插件已经结合具体流程内置到各个工作流当中，包括3个自研插件，不需要单独在编排界面中配置。

### 2.3 工作流

Bot由4个不同的工作流组成，在不同的情况下，Bot会根据用户的输入去调用不同的工作流完成自动化的任务。

![](img/4b95bbb9a9aafdf4d9c1e6d04ee2fa63.png)

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

![](img/5c73d4d16795ce8e2a66b1460e8f1b34.png)

## **3.自研插件创建过程**

### 3.1 企业微信群聊机器人

**共包括四个不同的插件**，可以做到向特定的企业微信群发送文字消息，markdown文字，图文，文件等不同形式的内容。这里重点以其中发送文件，sent_file_message的插件来讲解。

![](img/e164be1a1105917505d67614ef7d1826.png)

**首先，到语聚**ai的第三方api集成平台上添加工具动作；

![](img/36c74b212f44cf03ef2c51c3e75af170.png)

接着在平台上**测试相关动作**，获得返回的API请求的python代码；

![](img/0d92a6a32df871081dc3f2e72fd5c02d.png)

最后按步骤**集成到coze的插件创建平台中**。

![](img/36d384324d770bbfa07aed6bba20b474.png)

![](img/306dfdcc80daea4d9d8ad057a5e588c2.png)

![](img/aeef8719a1dcac8a9cf8eafba58c5989.png)

![](img/52c63b07d38d91d1a6f5e4990cda139f.png)

![](img/0b4d75eeadd69c1c4757a5c10779863f.png)

![](img/f9616489596f75b4e55cb498bdaa0d66.png)

### 3.2 飞书多维表格

使用coze在 Coze IDE中创**建模式创建插件，实**现获取飞书特定表格最新记录特定字段的内容

![](img/2437a05c37d7fb010a3ff8b464922679.png)

根据飞书开放者文档的要求在coze IDE平台中用handler的方式编写python代码，配置项目依赖

![](img/5a3683f918584e487d79b837b6ff0207.png)

metadata中配置输入和输入端信息

![](img/0f6c330af34422312c83ba29da5bc97d.png)

最后测试发布成功

## **4.自研工作流搭建过程**

### 4.1 hotspot_direct_output（抓取热点-bot中用户需求-直接返回结果）

![](img/ae8402a36ce49b34ac0d01ffd92a46f2.png)

![](img/66b254cf38b9ff4618ccedaea6c10255.png)

![](img/2c24bb06ddd86ff11f30e3728a083afe.png)

![](img/957b06c3b600bdb4a08a0f2b244702f3.png)

### 4.2 hotspot_bot_file_wechat（抓取热点-bot中用户需求-发送资讯文档到企业微信群）

![](img/d860546a374d97fccee2a8ec86d484b6.png)

前面几步和工作流1类似，接下来如下图操作

![](img/d07596f9fbc78072b28aba6ff0ae8238.png)

### 4.3 hotspot_feishu_file_wechat（抓取热点-飞书表格用户需求-发送资讯文档到企业微信群）

![](img/a4cd73845fcc283aea14f08e34f22872.png)

前面几步和工作流1类似，接下来如下图操作

![](img/a02fc3b00f2632d1dca2c0c5e70b30af.png)

![](img/5f64aaf6c8ec8bdcb1aa2c59882c40fc.png)

### 4.4 search_bot_file_wechat（用户需求+网址提供- 网页内容总结- 全网搜索其他相关信息-生

1.  成信息文档返回 - 同步发送到企业微信群）

![](img/f2bdf0136e0a48a1a5e9c436048ce299.png)

![](img/1def911be4dbe15e66b7e7bccdc7f3b4.png)

![](img/53d16efc2a5f2f23d38d60103f3cbd9d.png)

![](img/3fb58f76ec7e85b486331ed26c1e8eee.png)

## 5.Coze API接入微信群Bot

通过调用Coze API的方式将coze的该多平台资讯推送的Bot机器人接入到微信和微信群当中，让客户能够方便的在微信群里和Bot进行交互，进行信息的高效检索和整理。

### 5.1 Coze API 设置

首先通过Bot的创建页面上端网址bot字段后BotID的数字获取到

![](img/593a8da9a2981521f89b417e9ffa22eb.png)

然后点击Bot发布的按钮后在页面的最下端勾选上Bot as API

![](img/2b501c28e028846c49874c757f9e15e2.png)

点击扣子API按键

![](img/8946f24970cc049cdfe41f1a2f647a6a.png)

创建API令牌并获取到API密钥

![](img/6223208bb4ddf96192dc528238b1700f.png)

这一步保持好以上的信息，之后在5.3当中需要用到BotID和Coze API

### 5.2 云服务器创建

在腾讯云服务上选择一个优惠便宜的服务器进行购买

![](img/b06523df06de1d0c37c91e8c6186f26f.png)

选择宝塔Linux面板的镜像

![](img/5bfbb6d4d735ae37522ed418b57c7f59.png)

### 5.3 Docker中配置COW组件

登录宝塔面板后，接下来需要在宝塔面板当中的来可视化的控制云服务器，在上面部署docker容器，启动COW项目与微信取得关联。

![](img/40a4ffc3ddf5f19f911b6f27707fed22.png)

chatgpt-on-wechat（简称CoW）项目是基于大模型的智能对话机器人，可以接入微信公众号、企业微信应用、飞书、钉钉，可选择GPT3.5/GPT4.0/GPT4o/Claude/Gemini/文心一言/讯飞星火/通义千问/LinkAI/ChatGLM/月之暗面，能处理文本、语音和图片，通过插件访问操作系统和互联网等外部资源。我们用这个方式来与微信进行链接。

点击“Docker”中的“项目模板”中的“添加”按钮

![](img/c853fe36db12e7ebad7f05a3ccb2a03b.png)

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

![](img/d73243511ee3558b4c48d0bb1b8b6b46.png)

在容器中创建容器，选择容器编排，填入模板和名称，确定

![](img/f214b4f84b4c4d2c6eaae93b9bdc4a9f.png)

现实运行成功

![](img/4cff4d5a499c05481f6e8e9b9651e7f7.png)

点击容器后，可以看到运行的是两个服务，因为接口文档中启动了两个服务。

![](img/9ec7a449c59164fa896ab83d955bd077.png)

然后点击“wc-andy-ai-bot”后面的日志按钮，在弹出层中用提前预备好的微信进行扫码。

![](img/1602c4c2835c0ba56a15ac4b4b7ed18e.png)

这个界面不会实时更新显示，需要手动刷新一下。点击“刷新日志”，如果看到 WeChat login success，就成功将Bot接入微信中了。

![](img/e06549ed44eca2a60d2ad8ee26272d3c.png)

# **四【商业化场景】**

### **1.人工智能（AI）技术跟进**

在快速发展的AI领域，技术的更新迭代非常迅速。该Coze bot可以帮助AI研究人员和开发者跟进最新的研究成果、技术动态和行业报告。通过自动化检索和多平台推送，研究团队可以在第一时间获取到前沿资讯，从而快速调整研究方向和策略。

**应用效果：**

+   实时更新AI技术动态，保持技术领先

+   汇总行业报告，支持研发决策

+   多平台推送，提升团队沟通效率

### **2.金融投资领域**

在金融投资领域，获取及时、准确的市场资讯和财务分析是投资决策的关键。通过该Coze bot，金融分析师和投资者可以自动接收最新的市场动态、股市新闻和财务报告。bot可以从多个金融资讯平台抓取数据，生成分析报告，并将这些报告推送到用户常用的交流平台，如企业微信群或飞书文档。

**应用效果：**

+   实时跟踪市场动向，捕捉投资机会

+   自动生成财务报告，节省人力成本

+   多平台同步推送，方便团队协作

>

### **3.医疗健康行业**

在医疗健康领域，及时获取最新的医学研究、政策变化和行业动态至关重要。该Coze bot可以为医疗机构、医生和研究人员提供最新的医学资讯和研究成果，并通过多模态形式推送到他们常用的平台，帮助他们更好地了解行业趋势，提升医疗水平。

**应用效果：**

+   实时获取医学研究和政策变化

+   自动整理医学报告，提升工作效率

+   多平台推送，方便医疗团队交流

### **4.教育培训行业**

在教育培训行业，了解最新的教育政策、教学方法和培训课程是提升教学质量的关键。Coze bot可以为教育工作者和培训机构提供最新的教育资讯和课程动态，并自动生成教学报告和资料，推送到相关平台，帮助他们更好地规划教学内容和课程设计。

**应用效果：**

+   实时更新教育政策和教学方法

+   自动整理教学报告，提升教学效率

+   多平台推送，便于教学团队沟通

### **5.科技创业与孵化器**

科技创业公司和孵化器需要实时掌握最新的科技动态和创业资讯。该Coze bot可以帮助他们获取最新的科技新闻、创业报告和行业分析，并通过多模态形式推送到他们常用的平台，帮助他们更好地了解市场需求，调整创业方向。

**应用效果：**

+   实时获取科技动态和创业资讯

+   自动生成创业报告，支持决策

+   多平台推送，提升团队协作

# **五【未来展望】**

随着AI技术的不断进步和市场需求的不断变化，该Coze bot将不断优化和升级，实现更加智能和个性化的资讯推送服务。未来，我们将进一步拓展该bot的应用场景，覆盖更多行业和领域，为用户提供更加全面和及时的资讯服务，助力各行业的发展与创新。

Bot体验地址: [https://www.coze.cn/store/bot/73868](https://www.coze.cn/store/bot/7386884942153957391?panel=1&bid=6d2j6kkj04019)84942153957391?panel=1&bid=6d2j6kkj04019

#