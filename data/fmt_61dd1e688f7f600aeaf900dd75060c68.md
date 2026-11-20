
# 2023-08-09用公司电脑访问奇怪的网站，被抓到了

> 来源：[https://xuqiwei1986.feishu.cn/docx/L04Jd8DFOoB1tdxAC5tcIoESnAg](https://xuqiwei1986.feishu.cn/docx/L04Jd8DFOoB1tdxAC5tcIoESnAg)

> 🔗 原文链接：https://mp.weixin.qq.com/s/N4SdZzGomvZs2oGW0Fo4mQ

程序员鱼皮 2023-08-09 13:31 发表于 上海

以下文章来源于魔术师卡颂，作者卡颂。

魔术师卡颂。

把复杂的东西讲有趣。

最近，群里一个刚入职的小伙因为用公司电脑访问奇怪的网站，被约谈了。他很困惑 —— 访问的都是 HTTPS 的网站，公司咋知道他访问了啥？

![](img/9f0f03f6ff3d39eff57a2342b9bf309c.png)

实际上，由于网络通信有很多层，即使加密通信，仍有很多途径暴露你的访问地址，比如：

+ DNS 查询：通常 DNS 查询是不会加密的，所以，能看到你 DNS 查询的观察者（比如运营商）是可以推断出访问的网站

+ IP 地址：如果一个网站的 IP 地址是独一无二的，那么只需看到目标 IP 地址，就能推断出用户正在访问哪个网站。当然，这种方式对于多网站共享同一个 IP 地址（比如 CDN ）的情况不好使

+ 流量分析：当访问一些网站的特定页面，可能导致特定大小和顺序的数据包，这种模式可能被用来识别访问的网站

+ cookies 或其他存储：如果你的浏览器有某个网站的 cookies ，显然这代表你曾访问过该网站，其他存储信息（比如 `localStorage` ）同理

除此之外，还有很多方式可以直接、间接知道你的网站访问情况。

本文将聚焦在 HTTPS 协议本身，聊聊只考虑 HTTPS 协议的情况下，你的隐私是如何泄露的。

## HTTPS简介

我们每天访问的网站大部分是基于 HTTPS 协议的，简单来说，`HTTPS = HTTP + TLS`，其中：

+ HTTP 是一种应用层协议，用于在互联网上传输超文本（比如网页内容）。由于 HTTP 是明文传递，所以并不安全

+ TLS 是一种安全协议。 TLS 在传输层对数据进行加密，确保任何敏感信息在两端（比如客户端和服务器）之间安全传输，不被第三方窃取或篡改

所以理论上，结合了 HTTP 和 TLS 特性的 HTTPS，在数据传输过程是被加密的。但是，TLS 建立连接的过程却不一定是加密的。

### TLS的握手机制

当我们通过 TLS 传递加密的 HTTP 信息之前，需要先建立 TLS 连接，比如：

+ 当用户首次访问一个 HTTPS 网站，浏览器开始查询网站服务器时，会发生 TLS 连接

+ 当页面请求 API 时，会发生 TLS 连接

建立连接的过程被称为「TLS握手」，根据 TLS 版本不同，握手的步骤会有所区别。

![](img/902e25ce347cd95320fe7301e035b799.png)

但总体来说，「TLS握手」是为了达到三个目的：

1. 协商协议和加密套件：通信的两端确认接下来使用的 TLS 版本及加密套件

2. 验证身份：为了防止“中间人”攻击，握手过程中，服务器会向客户端发送其证书，包含服务器公钥和证书授权中心（即 CA ）签名的身份信息。客户端可以使用这些信息验证服务器的身份

3. 生成会话密钥：生成用于「加密接下来数据传输」的密钥

## TLS握手机制的缺点

虽然 TLS 握手机制会建立安全的通信，但在握手初期，数据却是明文发送的，这就造成「隐私泄漏」的风险。

在握手初期，客户端、服务端会依次发送、接收对方的「打招呼信息」。首先，客户端会向服务端打招呼（发送「`Client Hello`消息」），该消息包含：

+ 客户端支持的 TLS 版本

+ 支持的加密套件

+ 一串称为「客户端随机数」(`client random`)的随机字节

+ SNI 等一些服务器信息

服务端接收到上述消息后，会向客户端打招呼（发送「server hello消息」），再回传一些信息。

其中，SNI（Server Name Indication，服务器名称指示）就包含了用户访问的网站域名。

那么，握手过程为什么要包含 SNI 呢？

这是因为，当多个网站托管在一台服务器上并共享一个 IP 地址，且每个网站都有自己的 SSL 证书时，那就没法通过 IP 地址判断客户端是想和哪个网站建立 TLS 连接，此时就需要「域名信息」辅助判断。

打个比方，快递员送货上门时，如果快递单只有收货的小区地址（IP 地址），没有具体的门牌号（域名），那就没法将快递送到正确的客户手上（与正确的网站建立 TLS 连接）。

所以，SNI 作为 TLS 的扩展，会在 TLS 握手时附带上域名信息。由于打招呼的过程是明文发送的，所以在建立 HTTPS 连接的过程中，中间人就能知道你访问的域名信息。

企业内部防火墙的访问控制和安全策略，就是通过分析 SNI 信息完成的。

> 虽然防火墙可能已经有授信的证书，但可以先分析 SNI ，根据域名情况再判断要不要进行深度检查，而不是对所有流量都进行深度检查

那么，这种情况下该如何保护个人隐私呢？

## Encrypted ClientHello

`Encrypted ClientHello` [1]（`ECH`）是 TLS 1.3 的一个扩展，用于加密 `Client Hello` 消息中的 SNI 等信息。

当用户访问一个启用 `ECH` 的服务器时，网管无法通过观察 SNI 来窥探域名信息。只有目标服务器才能解密 `ECH` 中的 SNI ，从而保护了用户的隐私。

> 当然，对于授信的防火墙还是不行，但可以增加检查的成本

开启 `ECH` 需要同时满足：

+ 服务器支持 TLS 的 `ECH` 扩展

+ 客户端支持 `ECH`

比如，cloudflare SNI测试页 [2] 支持 `ECH` 扩展，当你的浏览器不支持 `ECH` 时，访问该网站 `sni` 会返回 `plaintext` ：

![](img/cfdd2fb84e2f31e5d556ffaffde89d77.png)

对于 `chrome` ，在 `chrome://flags/#encrypted-client-hello` [3] 中，配置 `ECH` 支持：

![](img/e5183b5e5cd71443aea3849fff24dec0.png)

再访问上述网站，`sni` 如果返回 `encrypted` 则代表支持 `ECH` 。

## 总结

虽然 `HTTPS` 连接本身是加密的，但在建立 `HTTPS` 的过程中（`TLS` 握手），是有数据明文传输的，其中 SNI 中包含了服务器的域名信息。

虽然 `SNI` 信息的本意是解决「同一IP下部署多个网站，每个网站对应不同的SSL证书」，但也会泄漏「访问的网站地址」。

`ECH` 通过对 `TLS` 握手过程中的敏感信息（主要是 `SNI` ）进行加密，为用户提供了更强的隐私保护。

### 参考资料

[1] Encrypted ClientHello: https://blog.cloudflare.com/encrypted-client-hello/

[2] cloudflare SNI测试页: https://crypto.cloudflare.com/cdn-cgi/trace

[3] `chrome://flags/#encrypted-client-hello`: chrome://flags/#encrypted-client-hello

---

欢迎学编程的朋友们加入鱼皮的[编程导航](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247544329&idx=1&sn=8c268a0a40befcc2b470c75ced1cb1d6&chksm=e9c2c9fedeb540e848aa18c484a9674dc9dd28fc45bb510dc4be52b147dd4c1d8fd7fe6899df&token=1562751299&lang=zh_CN&scene=21#wechat_redirect)，和 2 万多名编程学习者共享知识、交流进步，学习鱼皮全程直播开发的原创项目、上千篇优质编程学习求职经验分享、并获取 1 对 1 答疑指导服务。

往期推荐

[我的学习小圈子](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247539132&idx=2&sn=45af016dee0c03491750f76ba8fdbd25&chksm=e9c2be4bdeb5375d3253155b4053263109a631620b7cb9074e2fe1b4a5b1604ef92c522b606e&scene=21#wechat_redirect)

公司用的技术不主流，想跑了...

[24 届秋招群，集合！](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247548742&idx=2&sn=8ce0a21e8a395caf3fd7765f69804b52&chksm=e9c2d8b1deb551a7e04475509d2d05b7a99fb4a14e029cc728112432a882e375d94f0226463c&scene=21#wechat_redirect)

简历共赏｜一位不愿透露姓名的技术专家

[一次很意外的网站故障经历。](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247548725&idx=1&sn=b5911b11254e32253d4dc63152d73bd6&chksm=e9c2d8c2deb551d4927cfd30e55c4361a51c80e47ad5faa14478359b04996254da0edeaefaf9&scene=21#wechat_redirect)

看完这个，我直接把 SQL 刷通了！
