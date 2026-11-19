
# 【集成示例】企业🛰️

> 来源：[https://v11enp9ok1h.feishu.cn/docx/GZ1QdE574oSsjnxxaK8cK1TTnmh](https://v11enp9ok1h.feishu.cn/docx/GZ1QdE574oSsjnxxaK8cK1TTnmh)

## 一、准备工作

### 智能对话机器人配置

1. 进入智能对话机器人[控制台](https://chatbot.console.aliyun.com/)，选择左侧导航栏的应用管理->机器人->新建机器人。

2. 获取机器人ID，发布机器人（如无发布按钮则跳过）。

![](img/3c3b7fc0c57b1a2d4a3e33ccc7d51379.png)

3. 访问智能对话机器人[业务空间](https://alime.console.aliyun.com/?productCode=p_ics#/business_management/business_management)，获取业务空间`AgentKey`。

![](img/22e6b9d96de6708ea68f2479c0737a54.png)

4. 访问[RAM控制台](https://ram.console.aliyun.com/manage/ak)，获取阿里云账号的`ACCESS_KEY`和`SECRET_KEY`。

### 企业🛰️配置

+ 访问企业🛰️[后台](https://work.weixin.qq.com/wework_admin/frame#profile)，获取企业 ID。

![](img/cc807f517fc3c7c494ab9d689c64a7a6.png)

+ 登录[企业🛰️应用](https://work.weixin.qq.com/wework_admin/frame#apps)平台。

+ 选择应用管理菜单，然后创建应用。

![](img/e6d61bfad7093cc0281c65d8cae29c1f.png)

+ 设置API接收消息。

![](img/d9b7817eb0b281b3184e2553bfe5cca7.png)

![](img/3685d656d365935c95adfdd83ffd8255.png)

+ 格式为`https://{你的域名}/api/wechat/work/chat`。

+ `Token`和`EncodingAESKey`单击随机获取即可。

+ 您需要把企业 ID、`Token` 和 `EncodingAESKey`配置到代码中，并部署本示例，确保回调URL能被外网调用到，否则您单击完成会提示“openapi回调地址请求不通过”。

+ 上一步单击保存后，回到应用配置页，查看`Secret`。

![](img/0d6d9a25b3f515786e3453f9f2985011.png)

+ 在应用配置页面，您可以进行企业可信IP的管理与配置。

![](img/23634baf44a8ec849c2c2fd67211d7a8.png)

## 操作步骤

![](img/16a77032941b6c139196644a7555916a.png)

1. [您首先需要获取安全认证](https://help.aliyun.com/zh/beebot/intelligent-dialogue-robot-tongyi-version/api-chatbot-2022-04-08-applyforstreamaccesstoken-tongyi)，调用`ApplyForStreamAccessToken` 接口为后续操作做准备。详细信息请参考：获取流式调用的连接信息。

2. 在`ApplyForStreamAccessToken` 接口调用成功后会返回 `AccessToken`、`ChannelId` 和 `StreamSecret` 等后续使用的信息，信息有效期为2小时，到期后需重新调用接口获取。

3. [您需要监听并接收用户发送的消息](https://developer.work.weixin.qq.com/document/path/90238)，详情请参考：接收消息。

4. [然后需要解析接收的信息，得到用户的提问信息和用户信息](https://developer.work.weixin.qq.com/document/path/90239)，详情请参考：消息格式。

5. 接着您需要调用`GetToken` API获取🛰️的`AccessToken`，每次调用🛰️的接口都会用到。详情请参考：获取调用凭证`access_token`。

6. 得到企业🛰️`AccessToken`。

7. 接着您应该生成调用流式会话接口所需的签名。该签名应包含当前时间戳（`Timestamp`）和第二步接口返回的 `StreamSecret` 参数来计算生成。详情请参考：会话接口签名。

8. [您需要将 `AccessToken`、`ChannelId`、`Sign` 和 `Timestamp` 拼接成完整的 URL](https://help.aliyun.com/zh/beebot/intelligent-dialogue-robot-tongyi-version/stream-chat-api)，然后使用 SSE 流式调用会话接口，以监听 SSE 事件并获取大模型的流式输出详情请参考：流式会话接口。

9. 大模型流式接口会多次返回，每次返回都是全量的回复，[渲染端应该考虑用新的覆盖旧的](https://help.aliyun.com/zh/beebot/intelligent-dialogue-robot-tongyi-version/stream-chat-api#957179221c3l3)，详情请参考：流式会话接口。

10. 等待大模型回复完成，一次性将消息回复给用户。

## 二、示例工程代码

### 源代码下载（JAVA）

`wx-work-example.zip`

### 示例运行配置

1. [安装JDK，本示例工程的运行环境要求为JDK 8](https://help.aliyun.com/zh/sdk/developer-reference/jdk-installation)，参考文档：在Windows安装JDK。

2. 根据本文的上述操作，获取示例参数。

3. 修改项目的配置文件`application.properties`中调整相关参数。

4. 运行`com.aliyun.chatbot.channel.WebApplication`，也可打成jar包，通过`java -jar`命令运行。

### 示例参数

## 常见问题

**Q：** 配置回调接口信息时，直接单击完成会提示“openapi回调地址请求不通过”。

**A：** 需要把企业 ID、`Token` 和 `EncodingAESKey`配置到代码中，并部署本示例，确保回调URL能被外网调用到。
