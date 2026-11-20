
# Gemini 3来了！手把手教你薅Google羊毛：免费用上最强AI

> 来源：[https://my.feishu.cn/docx/HDlsdL34YolpfgxbD7ccrgEUnD8](https://my.feishu.cn/docx/HDlsdL34YolpfgxbD7ccrgEUnD8)

## 写在前面的大实话

这两天小道消息铺天盖地的说，Google放出消息说`Gemini 3`马上就要上线了。

老金我这几天一直在折腾`Gemini CLI`，为啥？因为这玩意儿是真香啊！

跟`ChatGPT Plus`那每月20刀比起来，`Gemini CLI`几乎是免费的（只要你有Google账号）。

而且现在的`Gemini 2.5`已经相当强了，很多任务完全够用。

更关键的是，等`Gemini 3`一上线，咱们第一时间就能用上。

你可能会问：直接用`API key`不是更简单吗？

老金我告诉你，Google给`API key`的免费额度太小了，每天只有100次左右，重度用户根本不够用。

但通过`OAuth`网页登录，你能拿到10倍的免费额度，每天1000次左右。为了这个额度，折腾一次绝对值！

但是吧，在国内用`Gemini CLI`有个让人头疼的问题：你得开魔法才能访问Google，可开了魔法之后登录又会出问题。

老金我前几天就被这个问题折腾得够呛，浏览器登录完了，`CLI`那边死活收不到回调，一直`503`错误。

后来搞明白了，其实就是个环境变量配置的问题。

今天就把这个坑填上，让你5分钟搞定登录，安心等`Gemini 3`上线。

## 到底是哪儿出了问题？

咱们先说清楚问题在哪儿，不然后面的方案你可能看不懂。

你用`Gemini CLI`的时候，它会让你在浏览器里登录Google账号。这个过程其实挺复杂的：

1、你运行`gemini`命令，它会给你一个Google登录链接

2、你点开链接，浏览器跳转到Google登录页

3、你输入账号密码，完成授权

4、浏览器跳转回 `http://localhost:xxxxx/oauth2callback`，把授权码传给`CLI`

5、`CLI`拿到授权码，再去Google换取真正的访问令牌

听起来挺简单对吧？

但问题就出在第4步和第5步。

如果你的魔法设置不对，第4步的`localhost`回调会被拦截，`CLI`收不到。或者第5步`CLI`去换令牌的时候没走魔法，直接`503`超时。

老金我当时就是这样，浏览器显示"无法访问此网站"，或者显示`HTTP ERROR 503`，试了好几次都不行。

## 解决方案其实很简单

折腾了一晚上后，老金发现其实就是设置几个环境变量的事儿。

核心思路就是：

1、让`Gemini CLI`访问Google的时候走魔法

2、但`localhost`的回调不走魔法，直接本地连接

具体怎么操作呢？我给你准备了三种方案，从简单到自动化，你根据自己的习惯选。

---

## 方案一：临时命令（适合偶尔用一次）

如果你就是偶尔用一下`Gemini`，不想搞复杂的配置，那这个方案最简单。

每次用之前，在`PowerShell`里运行这一行命令：

```powershell
$env:HTTP_PROXY="http://127.0.0.1:15236"; $env:HTTPS_PROXY=$env:HTTP_PROXY; $env:NO_PROXY="localhost,127.0.0.1,::1,.localhost"; gemini
```

**注意：** 把 `15236` 改成你自己魔法软件的端口号。某C软件默认是`7890`，某V软件默认是`10809`，具体看你用的啥。

![](img/0085798fb8aaca42a6e349b9ea78344e.png)

这个命令干了啥？

1、告诉`Gemini`：访问外网就走`127.0.0.1:15236`

2、但是`localhost`、`127.0.0.1`这些本地地址直接连

这些设置只在当前终端窗口生效，关了窗口就失效了。所以叫"临时方案"。

---

## 方案二:PowerShell函数（推荐，一劳永逸）

如果你经常用`Gemini`，那这个方案最爽。配置一次，以后直接输入`gemini`就自动走魔法，再也不用每次手动设置。

而且这个方案不会影响`npm`升级，也不会影响系统其他软件，可以说是最优雅的解决方案。

### 怎么配置？

**第1步：** 打开`PowerShell`配置文件

```powershell
if (!(Test-Path $PROFILE)) { New-Item -Type File -Force $PROFILE | Out-Null }
notepad $PROFILE
```

会弹出记事本，可能是空白的，别管它。

![](img/ad75e9f99b75d0d5e38dd900bca976dd.png)

**第2步：** 在文件最后面加上这段代码

```powershell
# Gemini CLI 自动魔法配置
function gemini {
  param([Parameter(ValueFromRemainingArguments=$true)] [string[]] $Args)

  # 这里改成你的魔法端口
  $proxy = "http://127.0.0.1:15236"

  # 备份原来的环境变量
  $bakHTTP  = $env:HTTP_PROXY
  $bakHTTPS = $env:HTTPS_PROXY
  $bakNO    = $env:NO_PROXY

  try {
    # 临时设置魔法
    $env:HTTP_PROXY  = $proxy
    $env:HTTPS_PROXY = $proxy
    $env:NO_PROXY    = "localhost,127.0.0.1,::1,.localhost"

    # 调用真正的gemini
    & (Get-Command gemini -CommandType Application | Select-Object -First 1 -ExpandProperty Source) @Args
  } finally {
    # 用完了恢复环境变量
    $env:HTTP_PROXY  = $bakHTTP
    $env:HTTPS_PROXY = $bakHTTPS
    $env:NO_PROXY    = $bakNO
  }
}
```

记得把第6行的`15236`改成你的魔法端口！

![](img/89570d9b660ab985f3f03794a1aba228.png)

**第3步：** 保存并关闭记事本

**第4步：** 让配置立即生效

```powershell
. $PROFILE
```

![](img/5f61c06d56a2f46b26d261363fbf8b3b.png)

**第5步：** 以后直接用就行

```powershell
gemini
gemini --yolo
gemini "帮我写个Python爬虫"
```

### 这个方案为啥好？

老金我现在就用这个方案，真的很爽：

1、自动化：不用每次手动设置环境变量，输入`gemini`就自动走魔法

2、隔离性：只影响`Gemini`，不会搞乱系统其他软件的网络配置

3、可升级：`npm update`升级`Gemini`的时候不受影响

4、可撤销：随时可以删掉这段代码恢复原样

---

## 方案三：批处理文件（适合喜欢双击图标的人）

如果你不习惯命令行，喜欢双击图标启动软件，那可以用这个方案。

### 怎么配置？

**第1步：** 新建一个文本文件，命名为 `启动Gemini.bat`

**第2步：** 用记事本打开，粘贴这些内容：

```bat
@echo off
setlocal
set HTTP_PROXY=http://127.0.0.1:15236
set HTTPS_PROXY=%HTTP_PROXY%
set NO_PROXY=localhost,127.0.0.1,::1,.localhost
gemini %*
pause
endlocal
```

记得把`15236`改成你的魔法端口！

**第3步：** 保存，以后双击这个`bat`文件就能启动`Gemini`了

你也可以给它创建个桌面快捷方式，换个好看的图标，伪装成真正的应用程序。

---

## 完整登录流程演示

配置好上面任意一个方案后，咱们来实际登录一次：

**第1步：启动`Gemini`**

随便用上面哪个方案启动都行，比如：

```powershell
gemini
```

**第2步：复制登录链接**

终端会显示这样的信息：

```
Code Assist login required.

Attempting to open authentication page in your browser.
Otherwise navigate to:

https://accounts.google.com/o/oauth2/v2/auth?redirect_uri=...
```

浏览器应该会自动打开。如果没打开，就手动复制这个链接到浏览器。

**第3步：浏览器登录**

1、输入你的Google账号密码（如果已经登录就跳过）

2、点击"允许"授权`Gemini CLI`访问

**第4步：自动回调**

浏览器会跳转到一个`localhost`地址，可能会短暂显示"正在连接..."

然后`CLI`那边会显示`Login successful!`

**第5步：开始使用**

恭喜，可以开始薅Google羊毛了！

```powershell
gemini "你好，测试一下"
gemini --yolo   # YOLO模式，不用每次确认
```

---

## 遇到问题怎么办？

老金把自己踩过的坑都列出来了，你遇到问题先对照着查。

### 问题1：浏览器显示"无法访问此网站"

这是最常见的问题，原因是你的浏览器也走了全局魔法，把`localhost`请求也转发了。

**解决办法：**

方法A：检查你的魔法软件有没有勾选"绕过本地地址"或`Bypass LAN`

方法B：临时用专门的浏览器启动命令（只影响这次登录）

`Chrome`用户：

```powershell
Start-Process chrome.exe -ArgumentList @(
  '--proxy-server=http=127.0.0.1:15236;https=127.0.0.1:15236',
  '--proxy-bypass-list=localhost;127.0.0.1;*.localhost;<-loopback>',
  "--user-data-dir=$env:LOCALAPPDATA\Temp\ChromeGemini"
)
```

`Edge`用户：

```powershell
Start-Process msedge.exe -ArgumentList @(
  '--proxy-server=http=127.0.0.1:15236;https=127.0.0.1:15236',
  '--proxy-bypass-list=localhost;127.0.0.1;*.localhost;<-loopback>',
  "--user-data-dir=$env:LOCALAPPDATA\Temp\EdgeGemini"
)
```

### 问题2：浏览器回调显示"HTTP ERROR 503"

这说明`CLI`在跟`Google`交换令牌的时候没走魔法，连接失败了。

**检查环境变量是不是设置对了：**

```powershell
echo $env:HTTP_PROXY
echo $env:HTTPS_PROXY
echo $env:NO_PROXY
```

应该显示：

```
http://127.0.0.1:15236
http://127.0.0.1:15236
localhost,127.0.0.1,::1,.localhost
```

如果不对，重新按照方案一或方案二设置。

### 问题3：显示`Error 400 (Bad Request)`

这是`OAuth`令牌过期了。`OAuth`的授权码是一次性的，用过就作废。

**解决办法：**

1、关闭浏览器标签页

2、重新运行`gemini`

3、用新生成的登录链接

别反复刷新同一个链接，没用的。

### 问题4：魔法端口不知道是多少

不同魔法软件的默认端口不一样：

1、某C软件：通常是`7890`

2、某V软件：通常是`10809`或`1080`

3、某C软件（`Mac`）：通常是`7890`

4、某S软件：通常是`1080`

你也可以打开魔法软件的设置页面，里面会显示"HTTP端口"或"本地端口"。

---

## 为什么值得折腾？

可能有人会问：这么麻烦，直接用网页版不行吗？

老金我实话实说，如果你只是偶尔问几个问题，确实网页版更方便。

但如果你是经常用`AI`的人，特别是用`AI`写代码、处理文本，那`CLI`真的香太多了：

**1、速度快**

不用打开浏览器，直接在终端输命令就行。

老金我现在写代码遇到问题，直接在`VSCode`的终端里`gemini "这段代码什么意思"`，秒出结果。

**2、免费额度大**

`ChatGPT Plus`每月20刀，`Claude Pro`每月也是20刀。

`Gemini CLI`只要你有`Google`账号就能用。

这里重点说一下：为什么要用`OAuth`网页登录而不是`API key`？

很多人可能觉得直接用`API key`不是更简单吗？

问题是`Google`给的`API key`免费额度很小，每天只有100次左右，重度用户根本不够用。

但通过`OAuth`网页登录（就是本文教的这个方法），你的账号额度能达到每天1000次左右，整整10倍！

对于经常用`AI`的人来说，这个差距太关键了。

所以虽然配置麻烦一点，但为了这 10倍的免费额度，绝对值得折腾一次。

**3、可编程**

你可以把`gemini`集成到自己的脚本里，批量处理任务。

比如老金我写了个脚本，一键把整个项目的代码结构让`Gemini`分析一遍。

4、第一时间用上`Gemini 3`

等`Gemini 3`一发布，`CLI`用户应该是第一批能用上的。网页版可能要晚几天甚至几周。

---

## 老金的使用建议

配置好之后，老金我建议你这么用：

**日常快速提问：**

```powershell
gemini "Python怎么读取CSV文件"
gemini "这段代码有什么问题" < code.py
```

`YOLO`模式（自动确认，适合信任度高的问题）：

```powershell
gemini --yolo "总结一下这篇文章" < article.txt
```

**多轮对话：**

```powershell
gemini
> 你好，我想学习React
> 从哪里开始？
> 能给个简单的例子吗？
> exit
```

**代码审查：**

```powershell
gemini "审查这段代码的安全性" < vulnerable.py
```

---

## 写在最后

老金我折腾这个`Gemini CLI`代理登录问题的时候，网上的教程要么写得太复杂，要么根本不管用。所以决定把自己的实战经验整理出来，希望能帮你少走弯路。

`Gemini 3`马上就要来了，据说性能直接对标`GPT-5 Turbo`，甚至在某些任务上更强。这波羊毛不薅就亏了。

配置过程如果遇到问题，可以在评论区留言，老金我看到会回复的。

最后说一句：`Google`这次是真的下血本了，`Gemini 3`的`API`定价比`GPT-5`便宜好几倍，还有这么大的免费额度。趁着现在赶紧配置好，等`Gemini 3`一上线就能第一时间体验。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
