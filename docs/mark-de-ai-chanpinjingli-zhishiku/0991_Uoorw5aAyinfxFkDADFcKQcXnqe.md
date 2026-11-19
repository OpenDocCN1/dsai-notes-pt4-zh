
# 飞书接入OpenClaw，打造7*24小时AI员工(附完整教程)

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/FzWRdSnPVoAqs0x34Gzc8s1bnZb](https://qqs7y1hozd1.feishu.cn/docx/FzWRdSnPVoAqs0x34Gzc8s1bnZb)

目前`ClawdBot`主要支持连接海外常见的聊天软件：WhatsApp、Telegram、Slack、Discord、Google Chat、iMessage、macOS、iOS/Android。

但由于项目开源，现在国内的通讯软件也开始被集成进来，比如飞书。

昨天花了大半天，在一台测试电脑上部署起来了。不需要什么Mac Mini，就普通的电脑就可以。windows/linux/mac都支持。

可以轻松实现，直接在飞书里给它下指令，它就在电脑上吭哧吭哧的干活

![](img/8c7c4a370eb98c9f3b165b447fdfc7a3.png)

以下主要分享下如何安装以及如何和飞书打通。

步骤写得比较详细，按照这个操作，也不用去买什么教程了

## 01

### 安装软件

我是在一台ubuntu电脑上安装的

安装命令如下，注意，需要网络支持

`curl -fsSL https://clawd.bot/install.sh | bash`

![](img/f80ebe121ac1ceaae2742463a11ef523.png)

等待一会安装好后，点击yes就可以了

![](img/a4aa142d9c607ea183b6d860eb4104ae.png)

这里需要选择一个大模型，软件集成了Qwen，可以不要钱。

![](img/dbfb0181814c2e38f2cc13f246f9bcb2.png)

然后会跳出Qwen的认证界面，认证通过就可以了

![](img/585e5ef2b2816bca52d41ad5f81ac5f3.png)

接下来是配置`channel`，也就是配置具体的应用。可以看到海外主流的通信软件

但是飞书不在这里面，点击Skip for now，跳过配置

![](img/bed2d11fc7d4595cbfd3e8558d0aad42.png)

Skills配置这里可以选No，等安装好后，可以在后台界面去配置

![](img/097119087960ff67954fac8cac3c4b0a.png)

后面的Hooks也选Skip for Now。用处不大

> Hatch in TUI (recommended)：意思是在终端界面运行

> Open the Web UI：网页版界面

最后一个是 “以后再说”。这里选默认的就行了。

![](img/f3fbafc86780b38e97e13017fa484873.png)

所有都配置完后，终端界面就可以进行聊天，也可以在网页中输入红框的地址进行后台配置

![](img/bf050e05358f77536c30355cae6cc9f8.png)

在后台中能看到各种各样的配置，所以前面安装的时候，其实都可以忽略。

![](img/333be61324adf93fc55a321992f00d9f.png)

测试一下对话，能正常工作

![](img/cde72b54ccc960b86df65a28ceb6dca1.png)

## 02

### 配置飞书

首先要在飞书管理后台建一个机器人应用

进入飞书开发者后台：

`https://open.feishu.cn/app?lang=zh-CN`

点击创建企业应用

![](img/e36b496f9c2f31732465773871965662.png)

应用名字是: `clawbot`机器人

![](img/dde04385887833c490bf14caddec9d70.png)

接下来，在应用中创建一个机器人

![](img/ded064f8ffb959cc4db55e4c14056706.png)

先点创建版本发布，这就算发布了第一版的机器人应用

![](img/9978d125afae8dc0b34861496c05f787.png)

接下来是配置`Clawdbot`和飞书之间的链接

运行下面几条命令，飞书的`appid`和`appSecret`，在飞书后台中可以获取到

### 1 安装飞书插件

`clawdbot plugins install @m1heng-clawd/feishu`

### 2 配置飞书的`appid`和`appSecret`

`clawdbot config set channels.feishu.appId "appid"`

`clawdbot config set channels.feishu.appSecret "Secret"`

### 3 打开飞书

`clawdbot config set channels.feishu.enabled true`

### 4 重启`clawdbot`

`clawdbot gateway restart`

![](img/a894fff4377df2140d0ddaa84f1c8cba.png)

完成后，在`Clawdbot`的后台`channel`里面，能看到飞书已经安装成功

![](img/bf1ed61a6cb1d9e67b8067d5db5fa814.png)

再次回到飞书，配置机器人的事件以及回调，都选择长连接的方式

![](img/86601fc78ec78234a36cbf871cf22446.png)

![](img/3c2461ed4031409b391acda25e18f9ad.png)

在事件配置中添加接收消息的事件，否则收不到用户的消息

![](img/3efe50e51f822651f0f2f3a48a58cc30.png)

最后进入权限管理界面，打开如下权限

![](img/f3e1017373f962f8b04a024932ff61ff.png)

这一切都完成后，记得再次发布一次版本才能生效

再回到`Clawdbot`的界面，按照下图配置好

![](img/41d7e45b05a15d2d1d2ee8ccd3ea4f9d.png)

整个配置就完成了

在飞书中搜索刚才配置的机器人名字。

![](img/9d87d1bd5f8af982b15476e39315f4dc.png)

开始对话

![](img/12e623339d513d91d9326babc5e9602a.png)

![](img/2809a1be516deae804f7db70664e2f7a.png)

让它帮我写一篇文章，并保存到本地

![](img/3e78d5bdd6558d9c7abcccd976ce37cf.png)

果然在本地就已经建好了一个文档。

![](img/0a813dbaa2704f0fc21e91de2928121e.png)
