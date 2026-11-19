
# 注册/抓取曲奇

> 来源：[https://docs.feishu.cn/docx/OJEwdEWZYo5o53xrANocJoZXnjb](https://docs.feishu.cn/docx/OJEwdEWZYo5o53xrANocJoZXnjb)

本篇作者：Wiki目录：[Claude宝宝教程](https://sqivg8d05rm.feishu.cn/wiki/BBocw85QTiA8EXkNcUZcT2pCnIe#part-JnNhdPgTBodOs3x0bYGczU5knRb)

没有酒馆的请先搭建了酒馆再弄这些：

[部署Clewd和酒馆（方法汇总）](https://sqivg8d05rm.feishu.cn/wiki/OMBWwQFN0i44MKk9GURcEhYindg)

## 注册 Claude

视频教程：

+ PC 端：https://www.bilibili.com/video/BV1VN4y1f7Dd

+ Android 端：https://www.bilibili.com/video/BV1cu411u7Eg

+ iOS端：烦请各位各显神通，下载好小火箭之后参考Android教程（Alook浏览器段）

### 网络

注册需要一个流畅的，可访问 [Claude](https://claude.ai/chats) 官网的霍格沃兹网络环境。

### 邮箱

不建议使用临时邮箱，不推荐国内邮箱，推荐 Outlook 邮箱，Gmail目前封号严重。

[Ou](https://support.microsoft.com/zh-cn/office/%E5%9C%A8-outlook-com-%E4%B8%AD%E6%B7%BB%E5%8A%A0%E6%88%96%E5%88%A0%E9%99%A4%E7%94%B5%E5%AD%90%E9%82%AE%E4%BB%B6%E5%88%AB%E5%90%8D-459b1989-356d-40fa-a689-8f285b13f1f2)tlook 支持邮箱和短信验证，验证后单个邮箱每周可以添加两个别名，目前也不推荐别名注册。

### 手机号

一次性的手机短信验证，可在 [SMS Activate](https://sms-activate.org/cn) 购买虚拟手机号，支持支付宝。

选择激活，服务选择 Any other (不要选择 Claude)，地区选择除英美两国之外的其他国家。

例如：菲律宾、泰国、印度、印度尼西亚、智利等。

![](img/1c266c9db86ef09f2a2d3e17624cb6c8.png)

验证步骤：获取手机号--输入手机号并发送验证码--稍等片刻后点击勾 -输入验证码

### 注册

+ 打开 [Claude.ai](https://claude.ai/chats) 进行注册，遇到报错一般为魔法问题。不要开启翻译，会报错，魔法问题请先自行研究

+ [Clau](https://sqivg8d05rm.feishu.cn/wiki/RurcwCVikigBAik3orecihuhnke#part-IxH7dJBcqoQFwGxHklScFfvpnjd)de3在使用一定次数后会进入冷却时间，建议使用7-10个账号供clewd自动轮换使用。

## 获取Cookie

### PC（[视频教程](https://www.bilibili.com/video/BV1VN4y1f7Dd)）

+ 使用 无痕[浏览 / inpr](https://claude.ai/chats)ivate 登录 Claude.ai，打开开发人员工具/检查（ctrl + shift + I 或 F12）转到网络/Network选项卡，然后开启一个新聊天页面发送任意对话。

+ 找到 `completion` 并复制 Cookie 里的内容。或使用浏览器自带的Cookie查看器/Cookie Editer扩展等，将其中 `sessionKey` 项复制出，并按 `sessionKey=sk-ant-sid01-xxxxxAA` 格式组合保存。

![](img/83d6462dd33840d72a6425d817d8faab.png)

> 浏览器插件教程：[PC上使用谷歌浏览器插件获取 Cookie](https://sqivg8d05rm.feishu.cn/wiki/G2PZwRTjLihenMktMcwcNZ0rnPc)

> 多Cookie一键整理格式：https://clewd.pages.dev/

### Android

使用以下任意一个浏览器获取 Cookie。

#### Via（[视频教程](https://www.bilibili.com/video/BV1cu411u7Eg)）

打开 Via，点击右下角菜单–设置–通用–退出时清除数据–勾上 `Cookies` (登录状态)。

登录后点击左上角盾牌-查看 `Cookies`，复制 `sessionKey=sk-ant-sid01-xxxxx` 部分并保存，然后直接退出 Via 浏览器。

![](img/5678f980878d327fb1455d962673d8b7.png)

![](img/2cbf191a46db97e854b6803fe5aab6d7.png)

#### Alook

打开无痕浏览，登录后获取 Cookie。

![](img/8a3549bc29e6156986650557313abc10.png)

### iOS

同上方的Alook。

PS：由于浏览器开发者原因，App Store上的Alook浏览器需支付6软妹币以下载使用，此行为与我们无关。

## 常见问题

Q：Cookie会失效吗？

A：不使用7天左右 / 手动点击退出登录，都会导致 Cookie 失效(403)，需要重新获取。

Q：账号有次数限制吗？

A：一个账号在进行10-20条的回复后，会进入2.5-4.5小时的冷却时间，冷却时间过后恢复正常。

Q：啊啊啊啊啊，大佬你的教程不好用，无痕关掉之后再开启无痕还是会自动登录原来账号

A：不需要退出无痕，直接关掉整个网页，然后重新进入网页的无痕，网页会自动跳转到注册界面。

更多问题请大佬们补充….
