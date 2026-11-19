
# 【集成示例】钉钉端机器人

> 来源：[https://v11enp9ok1h.feishu.cn/docx/FyVfdKnx2ocGURx58rEcqsB9nde](https://v11enp9ok1h.feishu.cn/docx/FyVfdKnx2ocGURx58rEcqsB9nde)

## 一、准备工作

**重要**

[钉钉端可以零代码集成智能对话机器人](https://help.aliyun.com/zh/beebot/intelligent-dialogue-robot-tongyi-version/dingtalk-robot-deployment-guidelines)，详情请查看：钉钉机器人部署指引。

### 智能对话机器人配置

1. 进入智能对话机器人[控制台](https://chatbot.console.aliyun.com/)，选择左侧导航栏的**应用管理**->**机器人**->**新建机器人**。

2. 获取机器人ID，发布机器人（如无发布按钮则跳过）。

![](img/097376601470d9cdea1d94ff5f437758.png)

> 访问智能对话机器人[业务空间](https://alime.console.aliyun.com/?productCode=p_ics#/business_management/business_management)，获取业务空间`AgentKey`。

![](img/bc7ccae994f0553be2dd51f17d32da1e.png)

3. 访问[RAM控制台](https://ram.console.aliyun.com/manage/ak)，获取阿里云账号的`ACCESS_KEY`和`SECRET_KEY`。

### 钉钉应用和机器人配置

1. [创建应用](https://open.dingtalk.com/document/orgapp/create-an-application)。

2. 配置机器人，进入钉钉开放平台>**应用能力**>**机器人**>填写机器人名称等必填项后，配置消息接收模式为 `Stream` 模式。

3. 为应用申请钉钉开放接口权限，在**开发配置**>**权限管理**菜单中需要申请三个权限。

+ 企业内机器人发送消息权限。

+ 互动卡片实例写权限。

+ AI卡片流式更新权限。

4. 在**基础信息**>**凭证与基础信息**菜单中获取应用的 `Client ID` 和 `Client Secret`。

5. 发布机器人后再发布钉钉应用。

6. [搭建卡片模板](https://open.dingtalk.com/document/orgapp/typewriter-effect-streaming-ai-card#d44fa64bc7bjz)，获取卡片模板ID。

## 二、操作步骤

![](img/643be406bf1073ebc3831d6dfab54b1a.png)

1. [调用`ApplyForStreamAccessToken`接口](https://help.aliyun.com/zh/beebot/intelligent-dialogue-robot-tongyi-version/api-chatbot-2022-04-08-applyforstreamaccesstoken-tongyi)。参考文档：获取流式调用的连接信息。

2. `ApplyForStreamAccessToken`接口，返回的连接签名信息有效期2小时，必须在到期前重新获取。

3. [监听并接收钉钉机器人的消息](https://open.dingtalk.com/document/orgapp/receive-message)（用户问机器人的消息）：机器人接收消息。

4. 发送AI卡片消息：[创建并投放卡片](https://open.dingtalk.com/document/orgapp/create-and-deliver-cards)。

5. 计算会话接口签名。[会话接口签名](https://help.aliyun.com/zh/beebot/intelligent-dialogue-robot-tongyi-version/chat-api-signature)。

6. [SSE流式调用会话接口](https://help.aliyun.com/zh/beebot/intelligent-dialogue-robot-tongyi-version/stream-chat-api)，监听SSE事件，获取大模型流式输出。参考文档：流式会话接口。

7. 大模型流式接口会多次返回，每次返回都是全量的回复，渲染[端应该考虑用](https://help.aliyun.com/zh/beebot/intelligent-dialogue-robot-tongyi-version/stream-chat-api#957179221c3l3)新的覆盖旧的。响应参数参考：流式会话接口。

8. 更新AI卡片：[AI卡片流式更新](https://open.dingtalk.com/document/orgapp/api-streamingupdate)。

## 三、示例工程代码

### 源代码下载（JAVA）

`dingtalk-example.zip`

### 示例运行配置

1. [本示例工程的运行环境要求为](https://help.aliyun.com/zh/sdk/developer-reference/jdk-installation)JDK 8，参考文档：在Windows安装JDK。

2. 根据本文的准备工作，获取示例参数。

3. 修改启动类`DingtalkRobotChatApp`中调整相关参数。

4. 运行`com.aliyun.chatbot.channel.DingtalkRobotChatApp`这个类的主方法。

5. 打开钉钉，通过机器人名称搜索并打开对应的机器人，即可进行问答。或者把机器人添加到企业内部群中，@机器人进行提问。

### 示例参数

## 四、常见问题

### Q：发消息给机器人后，机器人无回复。

A：首先请检查权限是否配置完全。请参考：钉钉应用和机器人配置步骤三。其次创建AI卡片后，请确保卡片已保存并发布。

### Q：机器人无法搜索到。

A：请确认机器人已发布，且所在应用也已发布并可见性配置正确。

![](img/49d8cee7727f441769affeaed410f2bf.png)
