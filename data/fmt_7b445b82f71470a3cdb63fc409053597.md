
# DeepSeek 服务器繁忙？这里有免费解决方案

> 来源：[https://huchegames.feishu.cn/docx/NalBdAYC3oUiTxx6HKGcBxQZn3b](https://huchegames.feishu.cn/docx/NalBdAYC3oUiTxx6HKGcBxQZn3b)

当地铁里都能听到人谈论 DeepSeek 的时候，服务器可能已经不堪重负了。

![](img/b11f53702b441e33460884fa766fb668.png)

这两天尝试 DeepSeek 的同学，经常会碰到「服务器繁忙，请稍后再试」的提醒。

DeepSeek 的 API 开放平台，曾老师从大年初二开始就无法访问。即使运气好进去了，也无法充值：

![](img/79b929da0b09a670986f56445e18bac8.png)

有没有办法解决这个问题，让我们可以继续畅快使用 DeepSeek 呢？

依然是托 DeepSeek 开源的福，主流和不主流的 AI 平台都已经部署了 `DeepSeek-R1` 模型。即使无法在 DeepSeek 官方购买，我们也可以在开放了 DeepSeek 模型的平台购买 API 服务，然后将 API 服务进行配置，即可畅快使用 DeepSeek。

下面将从易到难，从免费到付费，介绍三个平台的使用方法。

## 秘塔AI搜索：免费专业还不卡

这是值得吹爆的国产AI工具。接入 `DeepSeek-R1` 长思考模型后，给出的回答真慢，但真的很专业！而且完全免费！一点也不卡！

![](img/4fdccd393db13f412ff9605ae9383f4b.png)

注意：对于 AI 搜索工具，「你会聊天吗？」这种蠢问题有点侮辱大模型的智商了。建议掌握一些基础的搜索技巧再来提问。

[你可以把 AI 大模型当成人类](https://mp.weixin.qq.com/s?__biz=MzA5NzQ3NDM3MA==&mid=2656292128&idx=1&sn=942304a646b5d70bc21b1080028ac79e&scene=21#wechat_redirect)，所以这篇文章依旧有用：提问的智慧。

对于秘塔 AI 搜索，简单问题可以关闭「长思考」，并使用「简洁」模式，得到结果会更快。

以下是曾老师的问题，开启了「长思考」，使用「深入」模式。

> 推荐 `Stable Diffusion`、`Flux` 的提示词生成器。从易用性、学习曲线、专业功能三个角度，同时考虑使用成本、通用程度两个因素。

秘塔搜索了 15 篇网页，给出了下面的综合建议：

![](img/e582d4595f971e46350c2debd326631b.png)

「深入」模式的完整的推理过程和建议，可以访问下面的链接：

https://metaso.cn/s/r5mp6Gv

当使用「研究」模式时，秘塔搜索了 61 篇网页，给出了更多建议：

「研究」模式完整的推理过程和建议，可以访问下面的链接：

https://metaso.cn/s/476wZyK

## 硅基流动：薅羊毛体验

硅基流动应该是最早在春节假期期间部署 DeepSeek 大模型的平台。

注册硅基流动之后，就可以直接在其网页上与 DeepSeek 大模型对话，一点也不卡！

![](img/f551ea944e82f1ff6ea5db2da85ff70c.png)

注册硅基流动可以获得 2000 万 Tokens（相当于 14 元人民币）的配额。识别下面的二维码注册，你和曾老师都能白拿 2000 万。

![](img/48ef4db03e3aa79b9d2a146dda69c839.png)

硅基流动支持🛰️充值，用多少充多少，没啥心理负担。

![](img/97c2cdd9a9319ea1e971336aab782699.png)

但是，在硅基流动的文本聊天界面中，我们无法上传文件，也不能开启联网模式。

请在「API密钥」菜单中生成一个 API 密钥，下面我们就来解决联网模式的问题。

## Chatbox：花点小钱更流畅

无论是 DeepSeek，秘塔AI，还是 ChatGPT，使用的都是专有界面。有没有一个通用的工具，可以方便接入市面上其它的大模型呢？

我推荐 Chatbox。

https://chatboxai.app/

Chatbox 的界面中支持开启联网模式，支持图片和文档上传，也支持添加网页链接。

![](img/bd056bad4023aa6a6e632e23c42770eb.png)

可以下载 Chatbox 桌面版本。也可以通过下面的链接直接访问 Web 界面，你甚至不需要登录！

（Web 界面可能需要🪜）

https://web.chatboxai.app/

把硅基流动中申请的 API 密钥配置在 Chatbox 中，就可以选择 `DeepSeek-R1` 大模型。

![](img/30c492f538e19f9273befe20f1e81a1e.png)

刚才询问秘塔 AI 搜索的问题，也可以来问问硅基流动中的 `DeepSeek-R1`。遗憾的是，硅基流动中部署的 `DeepSeek-R1` 不支持联网问答，但我们依然得到了不错的答案。

![](img/bedf2372f36317f205c43bbf1d054bda.png)

DeepSeek 贴心建议先使用免费工具，Chatbox 在答案最后也给出了 Token 消耗。

![](img/365eec8bb6fe1f24029e53316ec86a6d.png)

到硅基流动后台看了下，回答这个问题还不至于让曾老师破产。

## Chatbox AI 购买

既然刚才 Chatbox 提醒说自己的大模型可以支持联网搜索，那我们就买它试试看。

将 Chatbox 切换到中文界面，可以使用🛰️和支付宝购买。Lite 买断一个月大约 30 人民币。

![](img/0698dda7aacd26edd0b096e0f89b99ed.png)

购买后 Chatbox 会将 License 发到邮箱，激活后可以选择 `DeepSeek R1` 模型。

![](img/a1d2b66b942aae1922731583584cb200.png)

询问同样的问题，结果还是挺令人满意的。

![](img/46c2595b3f682f53fa1181242db30c98.png)

## 和大模型玩角色互换游戏

文章开始的截图，是昨天的网络流行梗图。我们把同样的问答用在了 腾讯元宝、豆包和 KIMI 身上，看看它们的反应是什么：

![](img/d765d26fea8ecf03a4d36b21b629267e.png)

![](img/dfeccdd7dd649c31a70650a286092c99.png)

![](img/7cd05c8856bd7a729fb35395968a0c90.png)

<https://huchegames.feishu.cn/sync/Hzv6dRSGJsmHgOb4eSHcN1BHntg>
