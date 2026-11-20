
# Gemini Cli一键安装及好用的使用策略

> 来源：`https://my.feishu.cn/docx/Dzk4dPF8coXdnkx03Recnnron1f`

老金最近生了场病，就没写👸号了，今儿终于诈尸了！

这几天AI圈最大的新闻，估计就是Google的Gemini更新了。

相信不少家人们或多或少都看过了相关消息。

老金出品，向来是帮大家找最简单、直接的玩法。

今天，咱们不追热点，而是聊点实际的：怎么把`Gemini CLI`这个新工具，在咱们熟悉的Cursor里玩出花来。

这是一个很多人还没发现的、简单又方便的骚操作。

---

老规矩，Gemini目前还是封锁国区，所以你需要自己准备好能访问美区的网络环境。这个是基本操作哈。

## 安装教程

使用Cursor一键安装：

```
帮我安装 https://github.com/google-gemini/gemini-cli
```

它会贴心的把使用说明都给你弄好，需要的软件也会给你都安上（`Node.js`/`npm`等等），全部自动化，小白完全不用担心，老金自己就是个案例，这些玩意干啥的通通不懂，还是那句话，老金我根本看不懂代码。

完成后，直接打开终端，输入`gemini`就能启动了。

![](img/2d8717ec201831a8527b1c6979643753.png)

会进入这样个页面，无脑按回车，是进行主题设定的，不用管。

![](img/e25c26a5cc7cd8a776d86bd568814dd4.png)

然后弹出这个页面，点击登录谷歌。

![](img/c0393eca8c5649aa5817305d96326a4c.png)

## 常规启动教程

重点来了，很多人都卡在账号授权上面了，他会卡着一直转啊转，其实是回调失败了。

正常登录成功的话，会是这样的界面。

![](img/0d6a05df883cabc27e8b67f6d5a71dbf.png)

如果使用`CMD`登录，开了全局魔法，登录应该是不会有问题的。

**注**：不在cursor内，使用 Windows的 开始->`cmd`->打开命令提示符。

![](img/0e0d2bf9e1d0c00604eff974c8693e2d.png)

## 牛逼的来了，从这里开始！

接下来是今天的精髓了。

老金我给家人们准备了一个极简的脚本，找到你魔法的端口号，一般是西方标注1的地方这几个，你可以从7890开始尝试。

这个就是老金说的好玩的玩法了，它可以在Cursor里使用，和Cursor联动。

![](img/3e235b36e1b95e0230e0116f3151ec1b.png)

把下面这段复制到项目里运行就可以了，这个是上面的设定脚本。

```
# Gemini CLI Environment Setup for Cursor

Write-Host "==============================" -ForegroundColor Yellow
Write-Host " Gemini CLI Environment Setup" -ForegroundColor Yellow  
Write-Host "==============================" -ForegroundColor Yellow
Write-Host ""

Write-Host "Proxy Port Configuration:" -ForegroundColor Cyan
Write-Host "Common ports: 7890, 10809, 15236, 1080" -ForegroundColor Gray
$proxyPort = Read-Host "Enter your proxy port (default: 15236)"

if ([string]::IsNullOrWhiteSpace($proxyPort)) {
    $proxyPort = "15236"
}

Write-Host "Using port: $proxyPort" -ForegroundColor Green
Write-Host ""

Write-Host "API Key Configuration:" -ForegroundColor Cyan
Write-Host "Get your key from: https://aistudio.google.com/apikey" -ForegroundColor Gray
$apiKey = Read-Host "Enter your Gemini API Key"

Write-Host "API Key configured successfully" -ForegroundColor Green
Write-Host ""

$proxyUrl = "http://127.0.0.1:$proxyPort"
$env:HTTP_PROXY = $proxyUrl
$env:HTTPS_PROXY = $proxyUrl
$env:http_proxy = $proxyUrl
$env:https_proxy = $proxyUrl
$env:NO_PROXY = "localhost,127.0.0.1,::1"
$env:GEMINI_API_KEY = $apiKey

Write-Host "Setup Complete!" -ForegroundColor Green
Write-Host "===============" -ForegroundColor Green
Write-Host "Proxy URL: $proxyUrl" -ForegroundColor Cyan
Write-Host "API Key: Successfully configured" -ForegroundColor Cyan
Write-Host ""
Write-Host "You can now use:" -ForegroundColor Yellow
Write-Host "  gemini -p 'test connection'" -ForegroundColor White
Write-Host "  gemini" -ForegroundColor White
Write-Host ""
```

然后你就可以获得一个这样的界面！

再然后！无敌了！你就能同时使用Gemini和Cursor！

![](img/89edae9c3e8f78aa21789c33613c0085.png)

很多小伙伴该问了，这么做的原因是啥啊？

俩不都是改代码的吗？

NONONO，这么理解，格局就小了。

老金用简单直白的方式告你。

**Cursor = 单线程。**

`Gemini Cli` = 多线程。

比如，批量生产、批量修改、批量XXXXXX，你能想到啥批量的，用CLI就对了。

嗯？还不理解，老金再给你说明白点儿！

比如Cursor生成了一堆代码文件，你甚至都可以让`Gemini Cli`归类整理一遍！

再比如，你可以让Cursor爬下来的文章，使用`Gemini Cli`进行批量翻译！

然后老金我发现啊，很多小伙伴总把它拿来和 `Claude Code` 对比。

确实，如果你只是把它当作编程 CLI，那确实不如。

但是用它做其他用途很好用，比如整理文件、翻译、搜索、甚至子Agent等。

**关键！它免费！每天1000次！**

**老金我最后再唠叨几句：**

给家人们的建议：

+   **小白用户**：别怕，大胆跟着教程走，Cursor都给你自动化了，就登录那步留个心眼。

+   **硬核玩家**：赶紧试试，尤其是那个脚本，绝对能打开你新世界的大门，把CLI玩出花来！

你看，AI时代玩工具，不是说哪个模型、哪个软件就天下无敌了。

真正的牛逼，是把这些好家伙都变成你手里的牌，想怎么组合就怎么组合！

今天咱们把Cursor和`Gemini CLI`拧一块儿，就是这个道理。

别让工具限制了你的想象力，要主动去想怎么让它们给你打配合，这才是未来创作者的生存之道！

咱们不光要做工具的使用者，更要做个聪明的“攒局人”！

`https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc`
