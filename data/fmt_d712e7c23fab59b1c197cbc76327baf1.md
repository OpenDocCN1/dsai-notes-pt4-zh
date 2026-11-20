
# `gh CLI`批量改仓库直接封号！老金亲历GitHub解封4步流程含3个避坑指南

> 来源：[https://my.feishu.cn/docx/GPYkdw47voh8XoxL3DccXsaunzb](https://my.feishu.cn/docx/GPYkdw47voh8XoxL3DccXsaunzb)

昨天老金我用 `gh CLI` 批量修改仓库配置，几十个仓库一个循环跑下来，操作完一刷新页面，整个人愣住了。

自己账号登录还能看见，但别人看都是404了，GitHub直接给我suspend了。

![](img/d10929e8e5625a6b412d06b1ac4942db.png)

用了这么多年GitHub，头一回被封号。

那种感觉，就像你辛辛苦苦攒了几年的仓库，突然告诉你"暂时不让你碰了"。

老金我折腾了大半天，终于摸清了完整的解封流程。

里面有3个坑，不提前知道你大概率会卡住。

等解封同时，顺便给大家讲讲原因和处理方案的坑。

## 为什么会被封

原因很简单：短时间内通过 `gh CLI` 发起了大量API调用。

老金我当时在批量修改仓库的配置文件，几十个仓库一个脚本循环跑完。

GitHub的自动风控系统判定为异常行为，直接suspend。

这里要说一下，`gh CLI`是GitHub官方的命令行工具，不是什么第三方脚本。

但官方工具批量用起来，照样触发风控。

GitHub的风控机制不看你用什么工具，只看请求频率和行为模式。

## 解封第一步：X上@官方

查了网上的经验帖，第一步是去X（原Twitter）上@github，带上 #GitHubSupport 标签。

往下看的小伙伴可能有疑惑了，老金你不是英语不好吗？

对的。。全程是CC给我提供的所有填写内容。。。包含X的，包含工单的。

具体操作：发一条推文，@github，写清楚你的账号名和问题描述，语言用英文，加上#GitHubSupport标签。

> @github My account [username] has been suspended due to automated `gh CLI` operations for repository management. Please review. #GitHubSupport

![](img/e7bdc9fb2974a7ac7c632a425c2afd43.png)

为什么要这样做？

GitHub的support系统响应速度参差不齐，X上@他们相当于多开一条通道。

社区里有人反馈说@了之后处理速度明显加快。

## 解封第二步：提交工单

正式渠道是 `support.github.com`，在这里创建工单。

这是目前唯一可靠的官方申诉渠道。

### 重要提醒：发邮件这条路走不通

很多AI工具会告诉你发邮件到`support@github.com`申诉，老金我明确告诉你：这条路是死的。

GitHub早就废弃了邮件支持，`support@github.com`不再接受邮件，发过去会被退回。

已知靠谱的方案只有两个：X上@官方带#GitHubSupport，以及`support.github.com`提交工单。

但老金我差点卡死在提工单这一步。

用被封的账号登录support页面，系统会弹出一个手机号验证。

老金我心想这不简单嘛，输个手机号验证一下就行了。

结果点开国家选择框一看——没有中国（+86）选项。

你没看错。

GitHub的手机验证不支持+86中国手机号。

![](img/b1f825cdea8c9e5a92fb1fc6e78f9eb1.png)

账号被封了，想提工单解封，结果连工单都提不了。

这是整个解封流程里最大的坑。

GitHub社区里大量中国开发者都反馈了这个问题，到现在官方也没解决。

## 解封第三步：用备用账号破局

老金我最后想到了一个办法——用备用账号登录。

### 关键发现

用没有被风控的账号登录`support.github.com`，系统不会要求手机号验证。

所以流程变成了：

1、用备用账号登录support页面

2、进入 `support.github.com/contact/account`

3、选择"Appeal an account suspension"

4、工单里写清楚要解封的是哪个主账号

5、详细说明触发原因和申诉理由

![](img/6d13d3d4fb6213a4111234db1a5cccbb.png)

这里老金我要强调一件事：每个开发者都应该有一个备用GitHub账号。

不是搞什么违规操作，而是主号出问题的时候，备用号能帮你：

提交工单申诉

`fork`重要仓库做备份

继承项目维护权限

保持开源项目持续更新

注意：备用账号只用于沟通和申诉，不要用它来提交代码或绕过封禁，这样做会让情况更糟。

注册一个副号几分钟的事。等你真正需要的时候，你会感谢自己的。

## 解封第四步：写工单

老金我的经验，工单写得越详细，解封越快。

### 建议包含以下内容

被封账号的用户名和注册邮箱

触发原因的详细描述（比如：使用`gh CLI`批量修改了多少个仓库的什么配置）

操作截图（如果有的话）

明确提到手机验证不支持+86，所以用备用账号提交

表明你愿意遵守GitHub的使用政策

### 语气方面

温和、客观、有理有据。说清楚事实就好。

![](img/c2ee71a6f8cabafa95c0b715890dd53a.png)

老金我的工单大概是这个意思：

![](img/f940a57eabf55b37e2e2bb86376d411a.png)

## 等待和催促

GitHub官方说 Trust & Safety团队 会在1-3个工作日内回复。

但老金我要提醒你，社区里有人反馈等了2个月还没消息。实际体验因人而异。

如果超过7个工作日还没消息，可以温和地催促一下。

回复原工单就行，简单说"Hi, just following up on my previous request. Would appreciate an update when possible."

同时可以在X上再发一次，带上工单编号和#GitHubSupport标签，双管齐下。

目前老金我的账号还在等待解封中。

等解封了第一时间在群里同步。

## 3个避坑总结

### 坑1：`gh CLI`批量操作一定要控速

不要一次性操作太多仓库。在脚本里加个`sleep`间隔，比如每操作5个仓库暂停10秒。

GitHub的API有速率限制（每小时5000次），但即使没超限，短时间高频请求也可能触发风控。

### 坑2：提前注册备用账号

今天老金我的亲身教训。

没有备用号，你连申诉入口都找不到。

建议现在就去注册一个，绑定不同的邮箱。

### 坑3：发邮件申诉是死路

很多AI工具会给你一个GitHub support的邮箱地址让你发邮件申诉。

老金我验证过了，那些邮箱要么是错的，要么已经废弃了。

GitHub早就不接受邮件申诉了，唯一靠谱的渠道就是X上@官方带#GitHubSupport，以及`support.github.com`提交工单。别浪费时间在邮件上。

## 最后

老金我的开源项目暂时访问不了了，等账号恢复后第一时间同步。

想看教程的兄弟们，可以先去飞书开源知识库，链接在下方。

等账号解封后，老金我会在群里第一时间通知大家。

这次被封，老金我也算长了个教训。

以后批量操作一定加速率控制，备用账号也要定期维护。

你们有没有遇到过类似的情况？评论区聊聊。

## 参考资料

GitHub官方申诉政策：https://docs.github.com/en/site-policy/acceptable-use-policies/github-appeal-and-reinstatement

无法登录时的申诉入口：https://support.github.com/contact/cannot_sign_in

+86手机号问题社区讨论：https://github.com/orgs/community/discussions/141322

GitHub SMS支持国家列表：https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/countries-where-sms-authentication-is-supported

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
