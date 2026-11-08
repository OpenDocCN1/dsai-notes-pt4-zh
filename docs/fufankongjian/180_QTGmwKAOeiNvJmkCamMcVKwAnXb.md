# ClawdBot本地部署指南，零门槛搭建个人专属AI贾维斯！

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/XKhHdtDBVoS8P2xBSh3cgwjtnab](https://kq4b3vgg5b.feishu.cn/docx/XKhHdtDBVoS8P2xBSh3cgwjtnab)

# ClawdBot本地部署指南

## 环境搭建

本次指南以目前最流行的MacOS操作系统为例进行介绍，学完你也可以用MacMini部署一个7*24小时的专属贾维斯，并用iMessage进行远程调用。当然相关流程也可以直接复用到其他操作系统中进行部署。

首先我们需要先安装Node.js基础运行环境，登陆nodejs.org即可下载对应操作系统的安装包，

![](img/ea0d8d1eb27011943db3e85b341cb30f.png)

[2026-01-26 23-45-16.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VRV8byJfboRudpx1m8Dc0YDinmf)

除此之外，也可以在课件网盘中下载对应的安装包。

![](img/fd35c2dd9982ea5500bf5bc592b54237.png)

> 百度网盘课件：

> 链接: https://pan.baidu.com/s/1mse1VXimYfdvk8mSDM-zGQ?pwd=hu6d 提取码: hu6d

具体的Node.js的安装过程非常简单，根据提示，一路点击下一步即可

![](img/b10ca52878a940dbf2dee535d66e5d9a.png)

安装完成后按住command+空格，搜索并打开终端

![](img/e6ab4419bfc380df10a3b7187fc7188b.png)

先输入node -v确认Node.js的版本号，需要确保大于V22

```
node -v
```

![](img/87af151b0597f4a28fb1f6dd786abe58.png)

然后输入npm install命令，安装ClawdBot，

```
node -v
npm install -g clawdbot@latest
```

![](img/0ed72a582e5c28b052397f3172a32eae.png)

ClawdBot是一个封装程度非常高的项目，因此基础安装过程并不会太复杂。

## 国外在线模型API Key获取

等待npm安装完成后，即可对其进行设置。为了能够接入更多不同类型的模型，这里我们需要先注册OpenRouter并获取API-KEY，OpenRouter是目前全球最大的大模型中转商，能够提供稳定的模型中转调用服务，国内也可以借助OpenRouter调用Gemini、GPT和Claude模型，

![](img/72dc6f189be5a472998f37ba2b3b988c.png)

其注册过程也非常简单，只需要登陆OpenRouter官网，https://openrouter.ai，点击登陆，

![](img/4c7bf27fe939c8783eb5067721b8496e.png)

然后选择使用GitHub或谷歌账号登陆，也可以使用比如🐧邮箱进行注册，

![](img/d415679ace8a48b7b1d5c3e93edd40bc.png)

完成注册后即可点击Credits并进行充值了，点击Add Credits，

![](img/b5d4adf7ba04c6f5b5d35839ad7f232d.png)

在弹出的选项框中输入银行卡号即可绑定银行卡进行充值，

![](img/7be799edb55731ab868b83f3a060b35b.png)

此外也可以上淘宝找商家代充。充值结束后，选择API Keys，创建并保存OpenRouter的API-KEY。

![](img/c9de4c7981607c857c34b98812c354a0.png)

## ClawBot保姆级部署教程

然后回到命令行中，输入clawdbot onboard命令即可进入设置流程

```
clawdbot onboard
```

首先在设置模式（Onboarding mode）中选择QuickStart快速设置

![](img/bbfddf270ab0ac5eba62a38024e7a69b.png)

然后在Model provider模型供应商里面，选择OpenRouter，然后选择OpenRouter API Key，并复制OpenRouter API Key。

![](img/f288feafa65b803ecf3f3ef1838be668.png)

然后继续选择模型，对ClawdBot支持最好的肯定是Claude模型，但偶尔可能会遇到模型调用不稳定的情况，因此出于稳定性考虑，也可以选择DeepSeek-V3.2。

然后在Channel选项中选择iMessage，方便不同设备间通信

![](img/c9fccfca1caee00c8d98198c42ce79a0.png)

然后在iMessage CLI path部分，我们先保留默认imsg的设置，之后再进行修改。

![](img/7d02cb7f77daa6c2bbba44d14c3a7610.png)

然后再选择添加skills，并使用pnpm进行安装，能看到ClawdBot内置了非常多的skills，大家可以根据需求进行安装，

![](img/79c0df55c52869bac82c0c6f379b6e5d.png)

这里我们在一众默认的skills中选择apple-notes进行安装并尝试使用，之后在实际运行过程中，我们还能添加其他的skills

![](img/794a079f925377cbf78b76aa607d028c.png)

安装完成后，ClawdBot还会提示是否需要其他的模型或者API支持，例如是否设置谷歌地图API，加入后即可访问谷歌地图数据，实现行程规划、预定酒店餐厅等功能，或者设置Gemini-API-KEY来调用Nano Banana Pro文生图模型，或者设置OpenAI-API-KEY来实现文字转语音功能等等，这里可以都跳过，不影响使用，

![](img/aa7666a31cdf2e8d7079ded8f48cf951.png)

紧接着设置ClawdBot的记忆模式，这里推荐选择保留每次对话的记忆，也就是session-memory模式，然后就会进入到一段安装过程中，

![](img/26be19504b709c0ef2d37b000ee72ad0.png)

![](img/9600b8a36d46ba1f6154d2541e2c4d43.png)

等待安装完成后即可选择对话模式，这里推荐使用TUI，也就是同时在命令行和UI页面中进行对话

![](img/392eaf3f767d5812ac44351ce6acb2e2.png)

点击后就会弹出Web对话页面

![](img/1096b807dee98e40c9acca285407af24.png)

而下次运行时，我们可以通过输入clawdbot gateway --port 18789来开启ClawdBot，通过输入clawdbot gateway stop来关闭ClawdBot。

```
clawdbot gateway --port 18789
clawdbot gateway stop
```

接下来让我们回到弹出的Web页面中，ClawdBot默认端口是18789，首次运行时会自动输入一个User Message，Wake up，my friend！

![](img/02345cbab0a240ab65d09f996b86f2a8.png)

然后就能看到回复了，由于我这里此前运行了很久，所以带入了很多之前的记忆，回复的内容比较多，但只要能看到Assistant有回复，就说明已经运行成功了。 此外，也可以在命令行中看到回复内容。

![](img/6323734174a2d3395250ece406636d91.png)

接下来就可以在网页端自由进行对话了，由于我们预装在了一些skills，以及其本身就有一些原生能力，比如Python代码解释器、BrowserUse网页操作能力、Apple笔记功能等等，所以能看见ClawdBot会表示自己能做很多事情。

![](img/5e63a7205652a56a6bbd02b29ffb916b.png)

例如我们可以让ClawdBot对我们的Apple备忘录的进行汇总统计，生成回复结果如图所示，

![](img/e417a4cc73a78f70226f7a3ec29ae49c.png)

再比如我们让ClawdBot进行数据分析可视化，最终创建的3D热力分布图如图所示。

![](img/67526fcece4aabcf98449eb09187fd4d.png)

ClawdBot自带了一套完整的监控、运维、组装Skills的全部功能，例如我们可以在左侧的Overview中查看智能体运行整体状况

![](img/e3ea76c809a3cf2e42e1323560bb880c.png)

在Channels中查看已经链接的如iMessage等远程通信应用

![](img/a6394d741f88145f60b7d6e383d85230.png)

可以在Sessions中查看会话列表

![](img/f636ad5f44f2e208bcb48df26720e8a5.png)

也可以在Skills中自由组装各种能力

![](img/0eac597832d259ffb639f00250444b4e.png)

同时在Nodes中查看外链的设备，比如麦克风摄像头等

![](img/e4620e56e973072098806b9a34a834a1.png)

而在Config中，我们能修改ClawdBot的各项基本配置

![](img/327e447c2b01242a45818b461501d577.png)

而在Logs中能查看历史运行日志。

![](img/bd8fd817407e8d4db547f672dd421d4f.png)

可以说这个UI页面就是集对话、功能组装、系统设置、运维监控于一体的完整功能前端，整体非常惊艳。

## 通过远程iMessage远程调用ClawBot

紧接着，让我们继续尝试用iMessage远程调用ClawdBot，这样你就可以随时随地对家里的贾维斯发布命令了。这里我们首先需要做些准备工作，需要先安装imsg这个小工具。我们再打开一个命令行，输入如下命令即可进行安装，

```
brew install steipete/tap/imsg
```

![](img/3ae0c606de4cbcb7346476b051612a95.png)

安装完成后，输入which imsg来查看工具安装的位置，记住这个地址，

```
which imsg
```

![](img/b49582c782a3204b05ee8833f051b136.png)

然后继续输入如下命令打开ClawdBot的配置文件，

```
open -e ~/.clawdbot/clawdbot.json
```

找到channels中的cliPath，并写入刚才我们看到的imsg的程序路径/opt/homebrew/bin/imsg，然后保存并关闭配置文件。

![](img/54d02b5bee942c8498d7fd73289384da.png)

然后还需要进行授权。打开设置，找到隐私与安全性，找到完全磁盘访问权限，点击+，

![](img/23112c2643784fbb74a3ac888d5aeb94.png)

在弹出的选项框中输入终端，点击选择

![](img/9e13e1c814d05679db3eb1d57ea76d1f.png)

然后选择开启终端的完全磁盘访问权限，并根据提示重启终端。

![](img/037fe545fc6a9b2c740a67229571dfdb.png)

最后我们需要准备两个Apple ID，一个是运行在MacMini服务器上的Apple ID，假设为123@qq.com，第二个则是你手机端的Apple ID，假设为456@qq.com，然后打开MacMini上的iMessage信息，

![](img/d0ab7e96fd8adff7d9c2946fd3a8bab7.png)

![](img/00080d96fa89a9e540c27ddc7abf90c0.png)

同时打开手机端的iMessage信息，选择123@qq.com为收件人，然后发送一条信息，

![](img/a8acc282510a909946cf36831c4cd02d.png)

紧接着，在电脑端就能看到一条授权信息，其中包含了配对码，

![](img/653090ee510566ba6efe9a012038250f.png)

我们需要打开命令行，然后输入如下命令（需要替换配对码）完成配对，

```
clawdbot pairing approve imessage 配对码
```

![](img/61d31038c4a69d7122ccd6ff79cc8a19.png)

完成配对后即可在手机端使用iMessage和ClawdBot进行对话了，所有的信息都能在手机端和ClawdBot中同步，这样你就可以有一个7*24小时远程待命的贾维斯了，你可以随时和你的AI助手沟通，甚至远程发布任务让它完成GitHub代码审查合并等各项工作。

![](img/1e08ecd9161ab857f6f8d9b961389fe6.png)

![](img/7bc2d0e25e358f85f2bea9d4340c7b6e.png)

![](img/970c1f497367eec7c6bf80979911131a.png)

这里也帮大家梳理了X上热门的ClawdBot玩法：

> ClawdBot玩法：[Clawdbot 热门玩法终极盘点](https://kq4b3vgg5b.feishu.cn/wiki/M4b9wGznYijsbLk6jQzcg9hrnWh?from=from_copylink)