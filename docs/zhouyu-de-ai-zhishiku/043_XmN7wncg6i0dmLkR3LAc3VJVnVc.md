
# 手机上也能用 `Claude Code` 和 `Codex`！很方便，一键搞定

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/I7L9dHAKwo1A7Yx9SizcsmiGn5c](https://xatc0v8uz5m.feishu.cn/docx/I7L9dHAKwo1A7Yx9SizcsmiGn5c)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/L5eyMWHZ74QwyRBSuiIjwA)q.com/s/L5eyMWHZ...

> ⏰ 剪存时间：2026-01-21 16:59:28 (UTC+8)

> ✂️ 本文档由 [飞书剪存 ](https://www.feishu.cn/hc/zh-CN/articles/606278856233?from=in_ccm_clip_doc)一键生成

原创 刘小排 2026年1月19日 22:41

哈喽，大家好，我是刘小排。

我经常遇到这样的场景，不知道你遇到过没？

+ 让 `Claude Code` 跑一个复杂的任务，需要很长时间，突然朋友叫我出去吃饭，我要马上走，但是又舍不得丢下还没跑完的任务。

+ 在外面玩呢，想看看代码跑到哪一步了？

+ 正在马桶上蹲着，手痒，想打开 `Claude Code` 玩一玩。

+ 躺床上，突然想和 `Claude Code` 讨论代码方案

本来想自己搭建一个小产品来解决这个痛点，正调研着呢，发现已经有一个成熟的开源项目，可以直接用了。它就是 `Happy Coder`。

## Happy Coder 能做什么？

`Happy Coder` 是一个移动端和网页客户端，让你可以在手机上实时查看和控制电脑上运行的 `Claude Code` 或 `Codex`。

### 核心功能

+ 跨设备无缝切换：手机上查看进度、发送消息，回到电脑前按下键盘就能接管操作

+ 实时语音通讯：支持语音交互

+ 端到端加密：代码数据全程加密，隐私有保障

+ 消息推送：AI 需要授权或遇到错误时，手机立即收到通知

再强调一次！！不用担心安全问题。因为 `Happy` 支持端到端加密，你的代码数据全程加密，不会泄露。

## 如何使用？

### 前提条件

你的电脑上要有能够运行的 `Claude Code`（或 `Codex`）。

因为 `Happy Coder` 的原理，并不是在手机上运行 `Claude Code`，而是让你通过手机，连接电脑上的 `Claude Code`。

### 第一步：在电脑上安装并打开 `Happy Coder`

```
npm i -g happy-coder
```

如果你要使用 `Claude Code`，接下来在命令行输入

```
happy
```

如果你喜欢 `Codex`，那需要在命令行输入

```
happy codex
```

接下来你会看到一个二维码

![](img/d7aa7f9921abf1366dcaf8e8d851d853.png)

### 第二步：在手机上安装 `Happy` 的手机客户端

无论是 iPhone 还是 Android，都可以安装。

**iOS App 地址：**

https://apps.apple.com/us/app/happy-claude-code-client/id6748571505

**Android App 地址：**

https://play.google.com/store/apps/details?id=com.ex3ndr.happy

如果你不想安装 App，还可以使用网页版客户端。

**网页版：**

用浏览器打开 https://app.happy.engineering/

不过我感觉还是 App 更好用一点。

### 第三步：打开手机上的 `Happy` App，扫描二维码即可完成配对，建立和电脑的连接

连接成功后，你就能在手机上：

+ 实时查看 `Claude Code` 的运行状态

+ 发送消息与 AI 对话

+ 接收进度通知

![](img/d8d6f571bad7d9c2515c894d9ac80a70.png)

### 第四步：开始使用！！

直接在手机 App 里就可以聊天了

![](img/1daceb672b81912bfb0f93e40388ef1c.png)

对了，是可以同时打开多个 `Session` 的，多个任务可以并行做。

![](img/a9a5f82faf1d041810102d37eb552b2a.png)

当有手机客户端连接电脑时，电脑终端类似下图这样的待机状态。

如果要想回到用电脑，随时按空格键就能切换回电脑操作。

不过，我习惯的方式是：电脑开两个终端，一个终端给手机 `Happy` 用，一个电脑用，两个都不耽误。

![](img/b5abfc6a92194f2b5e578f8148ac5249.png)

小Tip：记得在手机上安装「豆包语音输入法」，体验最佳！生僻的单词（比如 `Anthropic`、`Next.js`）也能正确识别！无论你的普通发有多么不飘准，都能识别！

祝你玩得开心！

期待评论区交流
