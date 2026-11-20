
# OpenClaw + Kimi 2.5 最新教程，附飞书接入指南和 700+ Skill资源

> 来源：https://qqs7y1hozd1.feishu.cn/docx/JG0md1JdUoHJbZxsaFCcFE2xnSh

前段时间，身边不少朋友测 Kimi 2.5 ，都夸前端审美优秀。

直到刷到下面帖子，震惊了！

Kimi 2.5 在 Design Arena 设计榜，竟然打败了 Gemini 3 Pro 和 Claude，这可是开源模型啊。

![](img/26a0ed8842917af1b57d9806e3d63a91.png)

看来前段时间研究 Clawbot，错过的东西有点多。

研究后，迅速把 Kimi 2.5 接入了 Clawdbot。

下面是一句话测试

> 写一个精美的Todolist网页

![](img/376fbaca00cccf230b42414126f79157.png)

出来效果如下：

![](img/d70f1a3b619736b7ce343ce9dc2328b8.png)

一句话直出，确实有点顶啊。

前端任务简单，又测了一个自己写的视频生成复杂 Skill。

流程很长，必须先调用 Listenhub 生成音频和字幕时间轴。

基于字幕生成图片提示词，调用即梦生图，再调用 `Manim` 生成透明文本动效，根据我的 IP 头像，生成片头和片尾。

最后用 `ffmpeg` 拼接，生成完整视频。

![](img/bd1bd40145f87f1b0ca018c0f1e6c835.png)

之前我只用 Claude Opus 4.5 ，别的模型经常出问题。

---

下面写个详细教程，教大家如何把 Kimi 2.5 接入 Clawdbot（OpenClaw）。

另外，真的要吐槽下，Clawdbot一周竟然改了三次名字：

Clawdbot -> Moltbot -> OpenClaw

猜是最后一次改名，本篇教程应该是全网最准确的教程之一了，哈哈哈！

![](img/cd4665cbaa9e184f4b8dd6158308a4e0.png)

为了接入 Kimi K2.5 ，先开个 Kimi 会员套餐。

> https://www.kimi.com/membership/pricing

![](img/17c037c7f3c08d96a47fbfec27f1829f.png)

订阅后点控制台创建 API key

> https://www.kimi.com/code/console

注意：API key 只显示一次，复制保存到安全的地方，后面要用。

![](img/2bd709135677802189e94d6c67d3e015.png)

## 安装 OpenClaw

以 Mac 电脑为例，打开终端（推荐 `Warp`，好像也有 Windows 版）

复制粘贴如下指令，回车。

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

![](img/cd775792a3aeecb77742de6eee6a67e2.png)

安装后进入设置流程。

![](img/5894d7fe601f42029690179e994a3507.png)

左右方向键选 Yes，回车。

Onboarding mode 选 QuickStart。

往下继续走，后面会看到 Model/auth Provider

![](img/6b8da2e3fb5f805a709c2bca7dba37b8.png)

选 Kimi Code API Key，套餐买的是这个。

![](img/f5c6ac7f7b5eb7ae4a4a6a57d7a490e0.png)

粘贴上面复制的 API key，回车。

![](img/6f2e42528e2906224f82d30cdde9d9ca.png)

选第一个，设为默认模型。

![](img/443c4a22dc3f3262c402c655d6ee5bd2.png)

跳过了 Telegram、Discord 的机器人配置，后面教大家配置飞书。

然后就到配置 Skill，根据自己需要勾选。

上下箭头移动，空格选中，都选好以后，回车确认，进入下一步。

![](img/85067e00b6699f4777c1a2fa872a1f41.png)

然后是一系列配置，没有 API 或暂时用不上，但 Hook 三个都有用，建议选上：

第一个是启动时注入 Markdown 文件，在会话开始时注入类似 `README` 的内容。

第二个是操作日志记录，记录本次会话中执行的命令与操作上下文。

第三个是开始新会话时保存当前会话上下文摘要，便于后续无缝衔接。

![](img/6be3eb4040d339cba6f9df9658650915.png)

如果你的电脑正在运行网关或以前安装过，建议选重启。

![](img/d0ba243aabaaff217ab8bfcf53848962.png)

问你想在哪用（孵化）你的机器人，两个选项：

1. TUI，意思是终端 UI，基本上就是命令行对话

2. Web UI，有一个对话的网页

![](img/15bc3e9793a1bd98baa8fdec5d030549.png)

默认推荐 TUI，其实我觉得对新手来说，可能 Web UI 更友好。

两者不是互斥关系，其实都会安装。

![](img/7aef29f086f45648253a222d5333c68d.png)

如果想进入 TUI，输入：`openclaw tui`

![](img/1f913fd8a7e89ee06c39652108af5165.png)

到这里，其实已经装好了。

以后用 Web UI 或 TUI 对话就行。

但是，OpenClaw 最有意思的地方是支持大量 IM 工具接入。

用熟悉的聊天工具跟 OpenClaw 说话，下指令，很像当领导的感觉。

下面介绍飞书等接入方法。

## 飞书接入 OpenClaw

有人开发了飞书的 OpenClaw 插件

> https://github.com/m1heng/clawdbot-feishu

复制下面指令，粘贴到终端回车。

```
openclaw plugins install @m1heng-clawd/feishu
```

安装后会显示如下界面。

![](img/6341962eecdfe1e8b4034741121de4ab.png)

打开飞书开放平台

> https://open.feishu.cn/app?lang=zh-CN

点击「创建企业自建应用」，填写应用名称和描述

![](img/d8ed7f562034ed3d1a05aa96b25c95a3.png)

然后在“添加应用能力”->找到机器人，点击“添加”。（有些权限开通需要先有机器人能力）

![](img/ea2419a5234eba2709dc2a94783f9b01.png)

在应用的「凭证与基础信息」页面复制 `App ID` 和 `App Secret`。

![](img/8ddf86c74cf33baef01323c3f283f1b2.png)

在终端中输入

```
openclaw config set channels.feishu.appId "换成你的App ID"
openclaw config set channels.feishu.appSecret "换成你的App Secret"
openclaw config set channels.feishu.enabled true
```

![](img/dd7c300de982f09c838d9d6a3434a2fb.png)

然后需要重启网关，终端输入下面指令回车

```
openclaw gateway restart
```

上面操作很重要，否则下面飞书配置事件和回调时会出错。（未建立连接）

回到飞书应用「权限管理」页面，点击开通权限，输入 `im:message`。

![](img/09810621f5b568e2b45756df5842f809.png)

继续搜索关键词，把下面这些应用身份权限开通：

+ `contact:user.base:readonly`
+ `im:message`
+ `im:message.p2p_msg:readonly` （需要 `bot` 能力）
+ `im:message.group_at_msg:readonly` （需要 `bot` 能力）
+ `im:message:send_as_bot`
+ `im:resource`（上传图片或文件资源）

重点来了，很多人这里配置会出错。

事件配置和回调配置中，订阅方式都选“长链接”。

![](img/84713f488fd5d3a6f71822fdee81dee8.png)

![](img/89759ed11d0d16166c3fd4323b022d5a.png)

然后，在事件配置中点“添加事件”，把下面这几个加上

+ `im.message.receive_v1`（必需）
+ `im.message.message_read_v1`
+ `im.chat.member.bot.added_v1`
+ `im.chat.member.bot.deleted_v1`

![](img/458f434ae560a30c13727a3b6c0f361c.png)

配置完成后，在「版本管理与发布」页面创建版本并发布。

![](img/13fa7c548aaefa93127b80febc64f9fd.png)

这时打开飞书，搜索“OpenClaw”，就能找到应用机器人。

![](img/4091c6a095927f139971b16aa0e163a2.png)

别看截图多复杂，其实只需要花费几分钟就能搞定。

还有一种偷懒配置方法，就是让 AI 帮你配置

“帮我把飞书接入 openclaw，插件安装指令：`openclaw plugins install @m1heng-clawd/feishu`”

不过插件安装后，需要在终端输入下面命令重启网关。

```
openclaw gateway restart
```

然后，再发飞书 `App ID` 和 `Secret` 给它配置完成。

（注意：飞书开放平台还是需要自己手动配）

![](img/d9359195afc32e02a125b37a21058ebd.png)

OpenClaw 一大亮点是支持各种各样的 IM 软件。

常见的有 TG、Discord、WhatsApp、IMessage 等等。

个人感觉飞书对国人最友好，TG 最简单的，Discord 配置起来稍微复杂，但展示格式最好。

朋友写的 Discord 机器人配置教程。

> https://x.com/AppSaildotDEV/status/2016384987596206383

## 安装使用常见的坑

朋友 YC 写的教程，我读完，发现也遇到过不少。

> https://x.com/lyc_zh/status/2016984907226939820

另外，蝗虫群友发了一张 AI 生成的图，觉得对理解 OpenClaw 的运行机制有帮助。

从你说话，到机器人回复你，经过了很复杂的流程处理。

![](img/15629de68943d3e0bd69c95d86abe68d.png)

推荐大家记住几个常用的命令：

启动 OpenClaw 的 TUI

```
openclaw tui
```

重启网关

```
openclaw gateway restart
```

开启新对话

```
/new
```

添加备用模型

```
openclaw models fallbacks add [模型公司代号/模型名称]
```

例如：`openai-codex/gpt-5.2-codex`

设置默认模型

```
openclaw models set [模型公司代号/模型名称]
```

例如：`kimi-code/kimi-for-coding`

另外，既然已经连上 OpenClaw 了，不会的都可以问它。

比如我问如何给不同会话线程指定不同的模型，其实场景还挺多的。

![](img/3be70d6b291b10f02ed846d36d5c4072.png)

## 一点想法

发现很多人安装 OpenClaw，不知道用来做什么。

我觉得是缺乏场景和专属自己的 Skill。

OpenClaw 会自动加载 Claude 全局安装的 Skill，这点还挺方便的。

如果不会写 Skill，可以从模仿或使用别人的 Skill 开始。

分享一个 OpenClaw 精选 Skill 库，已收录 700 多个 Skill。

> https://github.com/VoltAgent/aw
