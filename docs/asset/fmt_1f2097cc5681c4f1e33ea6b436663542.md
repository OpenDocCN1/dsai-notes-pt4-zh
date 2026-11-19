
# AI 时代接入 PayPal 订阅支付有多简单？我用三轮对话把整套系统跑通了（含 Webhook 调试）

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/Fx0ddueTsokAUFxw1IXcN5UZnKg](https://xatc0v8uz5m.feishu.cn/docx/Fx0ddueTsokAUFxw1IXcN5UZnKg)

> 🔗 原文链接：https://mp.weixin.qq.com/s/g4iGv65Gb0YdAs5cO2urgA

> ⏰ 剪存时间：2026-01-23 17:55:00 (UTC+8)

> ✂️ 本文档由 [飞书剪存 ](https://www.feishu.cn/hc/zh-CN/articles/606278856233?from=in_ccm_clip_doc)一键生成

原创 孟健的AI编程认知 孟健的AI编程认知 孟健AI编程 2026年1月23日 17:01

大家好，我是孟健。

前不久，我分享了 `PayPal.cn` 支持个人卖家注册，并且给大家了如何注册的详细指引。

很多小伙伴已经把个人卖家账户开通好了，但新的问题也来了：

账号有了，站点里到底怎么接入 `PayPal` 收款？

今天我把我最近的一次实战流程分享给大家。

## 00 前置条件：先把商业模式和权益设计清楚

我这个站点之前已经把「价格和权益」设计好了，页面上也有一个 `waitlist` 按钮。

**我**现在要做的事情非常明确：把 `waitlist` 按钮替换成真实的 `PayPal` 订阅按钮，并把支付结果落到数据库里。

如果你还没想清楚商业模式，建议先把权益和价格部分搞定，后面接入会顺很多。

## 01 在 `PayPal.cn` 拿到集成入口和密钥

第一步，登录 `PayPal.cn`，点击首页的「去集成」按钮：

![](img/f50ea5e670f83b10b433b46741f5e161.png)

然后会跳转到 `PayPal.com` 的 Checkout 介绍页面，直接点「立即开始」：

![](img/4b0277e61336b863ea3afd79227d7dec.png)

在引导页里，选择「在网站上自行集成 `PayPal`」：

![](img/15ae2d06a8c34ae2e9fba97264990826.png)

第二个引导页选择「获取标准版」即可：

![](img/294e9bb54ec813fd18357bdc47e7bcf7.png)

接下来你会拿到一组凭证，通常会同时给你：

+   `Sandbox`（沙箱）环境的 `Client ID` / `Secret`

+   `Live`（正式）环境的 `Client ID` / `Secret`

![](img/8b40f555fec9cc93ad20de8104460f50.png)

## 02 第 1 轮对话：让 AI 直接把 `PayPal` 集成进你的项目

我用的是 `Codex`（5.2 xhgh 模式），这里给一段我实际用的提示词结构，你可以按你的项目替换：

```
根据当前项目信息和价格权益设计，帮我完成 PayPal 的集成， 要求数据库设计通用（便于后续 Stripe 的接入）。  参考集成文档： https://docs.paypal.ai/reference/sdk/js/v6/reference https://developer.paypal.com/docs/subscriptions/integrate/ 
```

这一步 AI 通常会做几件事（理想情况是一步到位）：

+   设计通用的数据表（订单/订阅/支付记录/用户权益等）

+   接入 `PayPal SDK`（前端按钮 + 后端创建订阅/校验）

+   把支付结果写入数据库（并留好后续接 `Stripe` 的扩展点）

![](img/fb56e8305c5789a6f7513acbe90e834f.png)

我的体验是：现在的 `Codex` 真的不太需要你介入，它会把数据库、服务层、路由、页面按钮都连起来。

## 03 第 2 轮对话：写入环境变量 + 让 AI 创建 `Product` / `Plan`

接着，把刚刚拿到的 `CLIENT_ID` 和 `SECRET` 写到项目的 `.env` （或你的部署环境变量）里：

![](img/b1e7b0e60846ef126e3492e71cf00605.png)

然后继续对 AI 说：

```
CLIENT_ID 和 SECRET 我已填入 env。 你帮我根据以下文档，创建好 product 和 plan： https://developer.paypal.com/docs/subscriptions/integrate/ 
```

AI 这一步会通过 `OAuth` 拿 `token`，然后调用 `PayPal` 的订阅相关接口，帮你把：

+   `product_id`

+   `plan_id`

直接创建出来并返回给你（一般也会顺手写回配置里）。

![](img/3638804a86e7fce4c0b88cb09de6cd49.png)

你也可以到 `PayPal Developer Dashboard` 验证一下：

`https://developer.paypal.com/dashboard`

![](img/b148c520462835c9401e8bcaca8dc1c1.png)

## 04 第 3 轮对话：接 `Webhook` + 本地调试（`cf tunnel`）

订阅支付接入， `Webhook` 是绕不开的：你需要用它来接收订阅创建、支付成功、续费失败、取消等事件，确保你的数据库状态和 `PayPal` 保持一致。

在 `PayPal Developer Dashboard` 里，进入你的应用，在 `Webhooks` 区域添加一个 `webhook`：

![](img/91b9ae8d785ec32abc8b550f6b522fd6.png)

本地阶段怎么调试 `webhook`？

本地开发时没有公网回调地址，我的做法是直接让 AI 帮我挂一个代理（我这里用的是 `Cloudflare Tunnel`）：

```
我要调试 webhook，你帮我挂一下 cf 代理，本地 3000 端口 
```

![](img/86a75e7ca159553cfb89a4d07051a278.png)

AI 生成好公网 `URL` 后，把这个 `URL` 填到 `PayPal` 的 `webhook` 地址里。

如果你做的是订阅站，事件勾选上「订阅相关」的即可；不确定的话，本地调试阶段可以先全勾上，后面再收敛。

![](img/3fe601363ab7a02f081c554432c6cf25.png)

![](img/2d13ac492c7ec298d5e03593af012e74.png)

## 05 沙箱环境完整走一遍：拿测试账号下单验证

最后一步：用 `Sandbox` 把整条链路走完。

在左侧菜单找到「`Sandbox` 测试账号」，创建/查看一个测试买家账号：

![](img/c2b7154f7f91094a40d536c1f171976e.png)

![](img/458d928fc796cf432286d288029f3543.png)

在本地测试支付时，直接用这个账号的邮箱和密码登录并支付：

![](img/f1bdb68289b873d7a627c9dbe04f4d3e.png)

`PayPal` 会给你生成一个「虚拟用户」完成沙箱支付：

![](img/dd71a3e0ae81c70c70ef260876ae9d4f.png)

支付成功后，回到 `Webhook` 日志里，你就能看到事件回调记录。

看到回调 + 数据库状态正确更新，就意味着： `Sandbox` 支付链路已经接入完成。

![](img/9f4efd63d02888851c500a56f9991e7e.png)

## 06 切换到正式环境：替换密钥并重建 `Product` / `Plan`

沙箱没问题后，接下来就是切 `Live`（正式环境）：

1. 把 `.env` 里的 `CLIENT_ID` / `SECRET` 换成 `Live` 的

2. 确认 `API_BASE` （或请求域名）切到正式环境

3. 让 AI 在 `Live` 环境下重新创建 `product` 和 `plan`

我用的提示词是：

```
env 我已经改成正式环境了，你帮我改一下 `API_BASE`， 并且创建一下正式环境的 `product` 和 `plan` 吧 
```

![](img/6c60947e5b39e93933d22b50c9db9b45.png)

至此，整个 `PayPal` 订阅接入就完成了：提交代码、部署上线即可。

## 写在最后：AI 编程的时代太恐怖了

如果放在过去，订阅支付接入意味着：

+   读一堆文档

+   处理各种边界状态

+   做一套订单/订阅的数据模型

+   调通 `webhook` 和本地联调

但这次我的实际体验是：我和 AI 的对话大概就三轮，就把整套「支付订阅体系」接完了。

在 AI 编程的时代，你只需要把目标讲清楚，把凭证配好，把结果验收好。

天的分享就到这里了，大家一定要抓紧拥抱新的 AI 时代

——我预计今年的 AI 编程，会发展到一个我们现在很难想象的水平。

👉 推荐一个我一直关注的👸号：哥飞

如果你也在做 AI / 技术出海 / 个人项目，哥飞的很多思路都很实战，值得一看。

🚀 想要与更多AI爱好者交流，共同成长吗？

和一群志同道合的人，持续精进 AI 的每一天

![](img/aefed53dc462f350f8c76c4ad5f2a14c.png)

### 精选文章

[程序员的“死期”定了？](https://mp.weixin.qq.com/s?__biz=Mzk0ODM5NTEyNA==&mid=2247504082&idx=1&sn=d22c55a9e374a8f574b1961ad6191a94&scene=21#wechat_redirect)

[AI越学越快，人却越容易沉迷](https://mp.weixin.qq.com/s?__biz=Mzk0ODM5NTEyNA==&mid=2247504067&idx=1&sn=5f2ae5e631e25e408532644ae0601d4a&scene=21#wechat_redirect)

`AdSense` 被判无效流量？官方聊完后我总结了 8 条避坑

`字流 2.0` 发布：我把 14 个平台的发布流程压到 10 分钟

[出海收款门槛又低了：`PayPal` ](https://mp.weixin.qq.com/s?__biz=Mzk0ODM5NTEyNA==&mid=2247503579&idx=1&sn=9a27c0d31457c0ed4b633fa825f630ee&scene=21#wechat_redirect)支持个人卖家账户（亲测 30 分钟通过）

创业之后，一天只工作 8 小时，真的够吗

[出海圈都说好用的 `Claude Skills`，我用 `Codex` 试了一遍：UI/UX 直接起飞](https://mp.weixin.qq.com/s?__biz=Mzk0ODM5NTEyNA==&mid=2247503503&idx=1&sn=a7db40f9f58f13c0310b93608a9a1eb4&scene=21#wechat_redirect)

🌱 创业后，我的一天是这样度过的

AI 时代，程序员的“衡量标准”彻底变了

40+老登的出海复盘，纯靠广告自然流几天破百刀
