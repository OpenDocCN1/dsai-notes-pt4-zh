
# Clawdbot 保姆级教程，国内最佳部署方案！

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Oe0yd6yIAo47KnxAKxYcYI2Qnxe](https://qqs7y1hozd1.feishu.cn/docx/Oe0yd6yIAo47KnxAKxYcYI2Qnxe)

第一时间尝试了下 `Clawdbot`，网上虽然也有一些教程了，但是踩的很多坑并没有细说，比如碰到这样的报错怎么解决：

![](img/0372e4b72c6889817de73e6d2a2108a1.png)

再比如网站 UI 打开了，但是仍有红色报错，怎么解决？要用什么 API 好呢？等等，非常多的坑。

![](img/a54bc2a17be1850abc01ddae83f78472.png)

所以，今天继续保姆级教学，带大家一起玩玩 `Clawdbot`。

## 一、硬件环境

首先，你不需要一台不怕泄露隐私，不怕被折腾坏的电脑。因为安装好 `Clawdbot`，它会完全接管你的电脑，操作你的电脑。

不建议用你自己平时工作或者学习用的电脑，容易翻车。

当然，也别上来就买台全新的 Mac Mini 部署 `Clawdbot`，成本太高了，这也能炒起来，我是万万没想到。

![](img/2012e3037c7e8111052147ff86fbfbcb.png)

可以先用虚拟机软件 `VMware`、`VirtualBox`，安装一个 `windows` 系统，让 `Clawdbot` 在里面尽情折腾就好。真有 24 小时待命需求的时候，再买也不迟。

![](img/c0c438b6f7273db26a713893672121aa.png)

我手里有一台之前 `AMD` 品牌方送我的小主机，也是 `Windows` 操作系统的，正好就拿来部署 `Clawdbot` 玩一玩了。

![](img/481cbb722e344323fde2e649be9ca198.png)

## 二、软件安装

直接来到小龙虾官网：

`https://clawd.bot/`

![](img/9e3cb92a6f92abd4fee0cf98f36158f7.png)

在 `Quick Start` 这里，复制对应的指令，我是用 `Windows` 的 `Powershell`。

第一次用这种命令行安装软件的朋友，大概率会碰到如下报错：

![](img/a2e5df5725535e82a3a4e41d3dc256cc.png)

这里有两个要先解决的问题：

1）需要🪜，而且需要开启 `Tun` 模式()，要让 `powershell` 也能🪜。

2）需要修改下脚本执行的权限，这个可以详细讲解下。

在 `powershell` 中输入指令 `Get-ExecutionPolicy -List`，可以查看到可执行脚本的权限设置，如果是这样 `Restricted`，表示不允许运行任何脚本，那么需要修改一下设置。

![](img/5bec21f39760433037c02f7d4a6f7eb7.png)

使用管理员权限打开 `PowerShell`：

![](img/4206d744174c075f0d9af5ad88a9d982.png)

输入指令：

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

这个命令行的意思是，允许本地运行的脚本，但如果脚本是从 Internet 下载的，则必须由受信任的发布者签名。

像这样，就修改好了。

![](img/10d90fc2fea6679a933b40104cb4e8f3.png)

再次运行指令：`iwr -useb https://molt.bot/install.ps1 | iex`，就能看到安装页面了：

![](img/a2658294ca7165606fb88f195254ff7c.png)

这里需要选择 `Yes`，才能继续安装，意思是你清楚它的强大且危险不，安装方式选择 `QuickStart`：

![](img/4511351a2a2cec0458473f115a9ea14d.png)

然后会让你选择，选择什么 API，支持 `OpenAI`、`Anthropic`、`MiniMax`、`Qwen` 等主流 API：

![](img/022f78624cbd65a0654e512f32652296.png)

我这里跟 `Clawdbot` 项目作者 `Peter Steinberger` 一样，选择他推荐的 `MiniMax` API：

![](img/67dc29fe720fffbaf1a20d65f40bdb8d.png)

![](img/50094360233b4b6e129de0b3ca25be8c.png)

`Clawdbot` 项目作者也推荐 `MiniMax` 的原因在于性格上 `M2.1` 更像 `claude`，执行任务简单啰嗦不直接，对国内用户很友好。并且速度快能够支持 `clawdbot-24h` 持续运行、心跳机制（定时任务）上比 `claudecode max` 抗用。

来到 `MiniMax` 的 API 开放平台：

`https://platform.minimaxi.com/subscribe/coding-plan?code=HzS5Dbszf8`

选择一个 `Coding Plan` 套餐就行，我用的是 49 元/月，那一档的。

![](img/8b1dfb7f287b52b1afea438a5ad9f7cd.png)

然后来到 `MiniMax` 后台的 `Coding Plan`，点击 `API Key` 这里的重置并复制。

![](img/aa5e0b419603978358dd840cecfa4c00.png)

然后回到 `Powershell`，选择 `MiniMax`，`MiniMax M2.1`，输入刚刚复制的 `API Key`，然后选择 `Keep Current` 即可。

![](img/33043be20f2cd5c3c5c75f73fa07f8b6.png)

`Select Channel` 的意思是，选择什么对话工具，我们可以先 `Skip for now` 跳过，后面再配置。

![](img/c557e191cb48ffbae1414adf2765c5e4.png)

然后让你安装 `skill`，这些技能也可以选择先不安装，后面用到的时候，会自行安装，所以也可以选择 `Skip for now` 跳过。

![](img/4ee3cfb675ae6fb5b63bfec10553848c.png)

然后就是 `Google`、`Gemini`、`OpenAI` 的API，都可以选择 `No`，不设置。

![](img/3d0566afceffd307471dbfe4c0c58cf6.png)

然后会看到是否启动 `hooks`，这里可以把这 3 个都选上，启动下。

![](img/2f447b2f7710fcb5e6c0c3cae6df82b2.png)

至此，初步安装就完成了，然后使用指令：

```
clawdbot gateway --verbose
```

启动 `clawdbot`，你会看到如下界面：

![](img/23d55b970b361838926ba91441924439.png)

在浏览器里打开这个网站：

```
http://127.0.0.1:18789/chat
```

就能看到小龙虾界面了，但是可能还会碰到这个问题：

![](img/a0ebdfed6c74e5ab30af89ad25f3b4e6.png)

来到 `Overview` 这里，能看到如下指令：

![](img/8a2f80bc2bbdcf205580f70cdc42c18f.png)

在 `Powershell` 里输入如下指令：

```
clawdbot dashboard --no-open
```

![](img/7897271df60791b081d359930898dfec.png)

浏览器会自动打开一个网页，把 `token=` 后面的字符串复制一下。

![](img/ab2b754de13ed3dff4741f89d3660196.png)

粘贴到 `Gateway Token` 这里，点击 `Conect`：

![](img/9496f3e84fdff46d452740726b19a6c2.png)

页面右上角的状态变为 `OK`，就标明成功了：

![](img/1edf5ba89f6b404d2505d0c9044e3d3f.png)

然后直接这时候直接对话，你跟它对话，`Web` 页面看不到回复：

![](img/937e860a60badcbeb0b5f92556284ebe.png)

这个就可能是 API 请求地址没有配置对，`clawdbot` 默认使用的是国际版 `MiniMax` API 地址。

国内和海外是不同的，需要和订阅的 `CodingPlan` 保持一致。

在 `Powershell` 命令行输入（国内）：

```
clawdbot config set models.providers.minimax.baseUrl "https://api.minimaxi.com/anthropic"
```

![](img/796a6bbdad7476a5d8aab0617876466d.png)

外海的需要这样设置：

```
clawdbot config set models.providers.minimax.baseUrl "https://api.minimaxi.io/anthropic"
```

如果你想重新设置 `CodingPlan` 的 `API key`，可以使用指令：

```
clawdbot config set models.providers.minimax.apiKey "你的MINIMAX_API_KEY"
```

这样重启 `clawdbot`，就能愉快的对话了。

![](img/e103679ec27278626cb293c31d0429cc.png)

如果你想在飞书里用 `clawdbot`，可以安装一个飞书插件，开源项目地址：

`https://github.com/m1heng/Clawdbot-feishu`

![](img/d8d73cde02460c5e74e6458aa6c8fb0c.png)

这个时候，你就可以让 `clawdbot` 帮你干活了，输入指令：

```
帮我安装插件，指令是：clawdbot plugins install @m1heng-clawd/feishu
```

![](img/f14871ee0a1f1ed372ae9ce2b04f06c7.png)

瞧，这样就安装好了。

![](img/84592e678151c5bcaa88afe0e4e00089.png)

甚至还可以让它帮你配置 `API Key`：

![](img/04474919a1b41df29ee8ca88a8aa733d.png)

![](img/358065d36e1add994228b906ff8ad2af.png)

在你安装好 `clawdbot` 的那一刻，`clawdbot` 就是你兢兢业业帮你干活的助理了。

此外，在使用 `clawdbot` 的过程中，如果遇到什么问题，可以尝试使用 `doctor` 帮你排查：

```
clawdbot doctor
```
