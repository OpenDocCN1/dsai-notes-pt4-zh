
# 零代码 一键部署 ChatGPT 到企业🛰️应用

> 来源：[https://aiyanxishe.feishu.cn/docx/S57ldmaERogZbPxWAkucdpP3nde](https://aiyanxishe.feishu.cn/docx/S57ldmaERogZbPxWAkucdpP3nde)

## 项目介绍

本教程可以实现一键部署 ChatGPT 到企业🛰️中，使 ChatGPT 与企业🛰️完美融合，手机或电脑上，打开企业🛰️，就可以使用强大的 ChatGPT 智能问答。截止目前，本项目可以提供两个能力：

1. 功能集成，将 ChatGPT 问答功能集成到企业🛰️中，借助企业**🛰️权限**功能，可以将 ChatGPT 共享到企业应用当中，此功能要求简单，有企业🛰️管理员权限即可，方法参见下面的部署方法。
2. 更强大的功能扩展，本项目为开源项目，有开发能力的小伙伴可以 Fork 到自己的仓库，根据自己企业业务需要，比如结合企业🛰️开放的 API，二次开发一些其他功能。
3. 其他功能，后续更新。

![](img/f83282a36fbe7f55aa5752c5ef7d299c.png)

## 部署方法

### 创建企业🛰️应用

第一步，创建应用，操作方法：企微管理员，电脑端上的企业🛰️ –> 头像 –> 管理企业 –> 应用管理 –>（最下面）创建应用 –> 应用 logo + 填入基本信息。

![](img/c50ce290be818967ba1ddb7183fcbd33.png)

### 第二步，配置应用

操作方法：接收消息一栏 –> 设置 API 接收 –> 未完等待第四步。

![](img/16a947c68e3fa4f497919fd192638dde.png)

### 第三步，配置参数

记录和准备下列字段信息：`corpid`, `agentid`, `secret`, `token`, `aeskey`, `open-api-key`。

- `corpid`：电脑端上的企业🛰️ –> 头像 –> 管理企业 –> 我的企业（下方） 图略
- `agentid`, `secret`：第一步里完成后可见。

![](img/5d4fad8f1025f1005de10d23f3ae4727.png)

- `token`, `aeskey`：在设置 API 接收里（上面第二步）
- `open-api-key`：这个需要在 ChatGPT 账号里生成，（如果没有 ChatGPT 账号也可以让别人生成一个，但是这个会产生费用）

![](img/4f55a70b45e558475d27d0f8c080bd53.png)

[申请网址 API KEY](https://platform.openai.com/account/api-keys)

### 第四步，一键部署到 Render

（代理服务器，免费，可以升级付费）

![](img/8c69416d2eacf7f40ad8e6187e4bb0ce.png)

如图所示，将上面的字段信息填入，然后点击 Apply。需要等 3-5 分钟部署，完成后复制生成的服务的 URL，如下图，然后拷贝 URL 后面拼接上 `/message`，比如 URL 是 `https://abc.com`，拼接成 `https://abc.com/message`，粘贴到上面第二步页面里，然后点保存，看到提示类似“保存成功”，代表服务已经配置成功了！

![](img/7d94c6aa01cfc1fa8ab00a8631f04d10.png)

### 最后一步：企业可信 IP 配置

应用页的最下方，配置可信 IP。

![](img/9d5235fa539a8a912777e5ff944a4056.png)

IP 地址如下图，Render -> Connect -> Outbound。

![](img/0dfa854219c1ad88b9b6dd83cd00802d.png)

将这三个 IP 地址配置到可信 IP 里，至此所有配置工作完成。

### 亲测有效

💯 下面就可以直接体验了，手机或电脑上企业🛰️进入自己创建的应用，可以和 ChatGPT 聊起来了。

经过一段时间的测试，如果想提高回复速度，办法一是升级 Render 为付费，另外一个最重要的因素是 ChatGPT 回复有延迟，因为用的是 GPT 3.5 Turbo，如果是 PLUS 用户的 API Key 会快很多，这是本人的测试情况，供参考。

本文由 [AI 研习社](https://zl49so8lbq.feishu.cn/wiki/wikcnu3kSXr75E50w8PFefhJMYf) 整理发布，AI 研习社是一个 AI 行业研究专题知识兴趣小组，目前分享包含：AI 工具使用、AI 商业模式研究、AI 行业研究报告、AI 经验分享、大厂动态等专题内容，包含 ChatGPT、MidJourney 等主流工具使用说明，通过知识手册的方式帮助大家构建系统化的 AI 知识库，让更多人可以轻松学习使用 AI 技术。

![](img/3c9834ec8749ea79f90e23cb492a9cae.png)

扫描二维码，加入 [AI 研习社](https://zl49so8lbq.feishu.cn/wiki/wikcnu3kSXr75E50w8PFefhJMYf)。
