# part3-01-源码满血版下载部署

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/FIFTdgerKoCZU6xCK1pcw1LUnkt](https://kq4b3vgg5b.feishu.cn/docx/FIFTdgerKoCZU6xCK1pcw1LUnkt)

# Part 1：OpenClaw 源码满血版下载部署

## 引言

你想不想拥有一个比 Siri 聪明 100 倍的私人 AI 助手？

它能记住你说过的每一句话，能帮你操作电脑上的文件、浏览网页、分析数据，甚至能同时连接🛰️、飞书、Discord 等 20 多个聊天软件。更重要的是——它完全运行在你自己的电脑上，你的数据一个字节都不会泄露出去。

这个助手就是 **OpenClaw**，一个拥有 321,000+ GitHub Star 的开源 AI 助手操作系统。它不是又一个"ChatGPT 套壳"——而是一个真正运行在你本地的、可定制的、可编程的智能助手平台。

今天，我们就从零开始，把它装到你的 Mac 上。整个安装过程大约需要 15 分钟。完成之后，你会在终端上看到 OpenClaw 成功运行的第一个画面——那将是你拥有私人 AI 助手的起点。

---

## 1.1 OpenClaw 项目介绍

### OpenClaw 是什么

> 🔗 OpenClaw 官网：https://openclaw.ai/

> 🔗 OpenClaw GitHub：https://github.com/openclaw/openclaw

OpenClaw 的全名是 **Open Claw**（开源之爪），口号是：

> "Your own personal AI assistant. Any OS. Any Platform. The lobster way."

> （你的私人 AI 助手。任何操作系统，任何平台。龙虾之道。）

它是一个**本地优先**的个人 AI 助手操作系统。注意，不是"聊天机器人"，不是"API 套壳"，而是一个完整的操作系统——就像 macOS 管理你的电脑一样，OpenClaw 管理你的 AI 助手。

> 💡很多同学第一次听到"AI 助手操作系统"这个概念会觉得很抽象。打个比方：macOS 管理你电脑上的所有硬件和软件——屏幕、键盘、App Store 里的应用。而 OpenClaw 管理你 AI 助手的所有功能——大模型连接、技能扩展、消息通道、记忆存储。它就是"AI 世界的 macOS"。

来看一组关键数据：

| 项目 | 详情 |
| --- | --- |
| GitHub Stars | **321,000+**（截至 2026 年 3 月） |
| 开源协议 | MIT（完全免费，可商用） |
| 开发语言 | JavaScript / TypeScript |
| 支持平台 | macOS、Linux、Windows（WSL2） |
| 消息通道 | **Wha**tsApp、Telegram、Discord 等 20+ 种 |
| 内置技能 | **100+** 个（浏览器自动化、文件操作、代码执行等） |
| 当前最新版 | **2026.3.13** |

> **怎么看这张表****：重点关注两个数字——**3**21K **Stars 说明这是一个极度活跃的顶级开源项目（作为对比，Vue.js 约 210K Stars，React 约 230K Stars），MIT 协议意味着你可以完全免费使用，甚至拿去商用也没有法律风险。

OpenClaw 的核心能力可以用下面这张表概括：

| 核心能力 | 说明 | 类比 |
| --- | --- | --- |
| Gateway 架构 | 统一的 WebSocket 控制面，管理所有功能 | 酒店前台——所有请求通过它调度 |
| 多通道接入 | 同时对接 20+ 即时通讯软件 | 全渠道客服——🛰️、飞书、Discord 一个都不少 |
| 持久记忆 | 跨会话记住你是谁、你在做什么 | 私人管家——记得你的喜好和习惯 |
| 100+ 内置技能 | 浏览器操作、文件处理、代码执行、音乐控制… | App Store——需要什么能力就装什么 |
| Cron 定时任务 | 自动执行后台任务 | 闹钟——到点自动干活 |
| 语音交互 | 语音唤醒 + 对话模式 | Siri——但聪明 100 倍 |
| Skills 生态 | 社区技能市场（ClawHub）13,000+ 扩展 | App Store——开发者社区贡献的海量扩展 |

> **怎么看这张表**：左边是能力名称，中间是技术解释，右边是面向零基础用户的类比。如果你不理解技术解释，直接看类比列就够了——它们描述的是同一件事。

### 和你知道的 AI 工具有什么不同？

你可能已经用过 Siri、ChatGPT 或者 Dify，它们和 OpenClaw 有什么区别？一张表说清楚：

| 维度 | Siri / 小爱同学 | ChatGPT / Claude 网页版 | Dify / Coze | **OpenClaw** |
| --- | --- | --- | --- | --- |
| **运行位置** | 云端 | 云端 | 云端 / 自托管 | **你的电脑上** |
| **数据隐私** | 数据上传苹果/小米 | 数据在 OpenAI | 取决于部署 | **全部在本地** |
| **可定制性** | 几乎不能改 | 不能改 | 可视化搭建 | **完全可编程** |
| **持久记忆** | 无 | 有限 | 有限 | **Markdown 文件，随便改** |
| **多渠道** | 仅语音 | 仅网页/App | API | **20+ 消息渠道** |
| **系统操作** | 受限 | 不能 | 不能 | **读写文件、执行命令、浏览网页** |
| **定位** | 语音助手 | 聊天工具 | 应用开发平台 | **个人 AI 助手操作系统** |

---

## 1.2 OpenClaw 最新版本与功能对比

### 版本号命名规则

OpenClaw 采用日历版本（Calendar Versioning，简称 CalVer） 命名，格式为 vYYYY.M.D。

比如 2026.3.13 的含义是：**2026 年 3 月 13 日**发布的版本。这种命名方式的好处是，你一看版本号就知道这个版本有多新——不需要去查"v3.2.1 到底是什么时候的"。

> 💡日历版本在开源社区越来越流行了（Ubuntu、pip、Terraform 都在用）。它的核心理念是"时间就是版本号"。对比传统的语义版本号（如 v3.2.1），CalVer 的优势是直觉——你不需要理解"大版本 3、小版本 2、补丁 1"之间的含义差异。

OpenClaw 有三个更新频道：

| 频道 | 说明 | 适合人群 | 版本号特征 |
| --- | --- | --- | --- |
| **stable** | 稳定版（默认） | 所有用户 | 2026.3.13 |
| **beta** | 测试版 | 尝鲜用户 | 2026.3.14-beta.1 |
| **dev** | 开发版 | 贡献者 | 2026.3.14-dev.xxx |

> **怎么看这张表****：绝大多数用**户（包括你）只需要关注 stable 频道。beta 和 dev 是给想体验最新功能但愿意承担不稳定风险的用户准备的。

### 最新三个版本亮点

| 版本 | 发布日期 | 核心亮点 | 关注度 |
| --- | --- | --- | --- |
| **2026.3.13**（当前最新） | 2026-03-14 | 修复 compaction token 计数、Telegram SSRF 安全处理、Android UI 改版 | ⭐ 安全修复 |
| **2026.3.12** | 2026-03-13 | **Dashboard v2**（模块化视图 + 命令面板）、Provider-plugin 架构重构 | ⭐⭐⭐ 重大更新 |
| **2026.3.7** | 2026-03 | 支持 GPT-5.4、OOLONG 基准测试 74.8 分 | ⭐⭐ 能力提升 |

> **怎么看这张表**：2026.3.12 是近期最重要的版本——它带来了全新的 Dashboard v2 界面和 Provider-plugin 架构。2026.3.13 主要是安全修复，属于"必须更新但没有新功能"的维护版本。

可以看到，OpenClaw 的迭代速度非常快——平均每 2-3 天就有一个新版本。这也意味着你需要掌握升级方法（我们会在 Part 5 中详细讲解）。

---

## 1.3 Node.js 介绍与安装

### 为什么需要 Node.js？

**安装 Open**Claw 之前，你的 Mac 上必须先装好 Node.js。

Node.js 是 JavaScript 的运行环境。OpenClaw 是用 JavaScript/TypeScript 编写的，所以它需要 Node.js 才能运行。

打个比方：Node.js 就像"发动机"，OpenClaw 就像"汽车"——没有发动机，汽车跑不起来。你不需要理解发动机的内部构造（V6 还是 V8），只需要确保它装好了、能转就行。

> 💡如果你的 Mac 上已经装了 Node.js，可以跳到"验证 Node.js 安装"那一步直接检查版本号。但如果你从来没装过——没关系，跟着下面的步骤走就行，5 分钟搞定。

**版本要求**：

| 版本 | 推荐度 | 说明 |
| --- | --- | --- |
| **Node.js v24** | ⭐⭐⭐ 推荐 | 最新 LTS，性能最优 |
| **Node.js v22 LTS**（22.16+） | ⭐⭐ 可用 | 长期支持版，稳定性好 |
| Node.js v20 及以下 | ❌ 不推荐 | 可能缺少 OpenClaw 依赖的 API |

> **怎么看这张表**：只要你安装 v22 或 v24 就没问题。如果是全新安装，直接选 v24（官网默认推荐的版本）。

### 三种安装方式对比

| 安装方式 | 适合人群 | 优点 | 缺点 | 推荐指数 |
| --- | --- | --- | --- | --- |
| **官网安装器** | 完全零基础 | 一键安装、图形界面、傻瓜式 | 版本管理不便 | ⭐⭐⭐ |
| **Homebrew** | Mac 用户 | brew install node 一条命令 | 版本可能不是最新 | ⭐⭐ |
| **nvm** | 开发者 | 多版本并存管理 | 配置稍复杂 | ⭐ |

> **怎么看这张表**：如果你是第一次接触编程，**直接选第一行"官网安装器"**——它有图形界面，一路点"下一步"就完事了。Homebrew 和 nvm 是给有经验的开发者准备的，新手不需要纠结。

### 实操：通过官网安装 Node.js

第一步：访问 Node.js 官网

打开浏览器，访问 [nodejs.org](https://nodejs.org)。你会看到页面上有一个醒目的绿色下载按钮，显示当前推荐的 LTS 版本。

**第二步：下载并运行安装器**

点击下载按钮，下载 .pkg 安装包（macOS 专用）。下载完成后**，双击运行安装向导。整个过程**就是一**路点 "Continu**e"（**继续） → "Agree"**（同意） → "Install"（安装）。

> ⚠️安装过程中可能会要求输入你的 Mac 登录密码（不是 Apple ID 密码）——这是因为 Node.js 需要安装到系统目录 /usr/local/bin/，需要管理员权限。放心输入即可。

### 验证 Node.js 安装

安装完成后，打开终端（如何打开终端我们下一节会讲），运行以下两条命令验证：

```
# 查看 Node.js 版本
# node --version 的含义：
#   node    → Node.js 的可执行文件名
#   --version → 参数，表示&quot;显示版本号&quot;
$ node --version
v24.14.0

# 查看 npm 版本
# npm 是 Node.js 自带的包管理器，安装 Node.js 时会一起装好
$ npm --version
11.9.0
```

看到版本号输出，说明 Node.js 和 npm 都安装成功了。如果你看到 command not found 的报错，说明安装没有成功——回到上面的步骤重新安装，或者试试关闭终端窗口再重新打开。

> 💡把你看到的版本号记下来，后面排查问题时可能用到。node --version 和 npm --version 这两条命令是你以后最常用的"诊断命令"之一。

---

## 1.4 命令行介绍与打开方法

### 什么是命令行？

如果你从来没有用过命令行，别担心——它比你想象的简单得多。

命令行就是和电脑"用文字对话"的窗口。 你打一行指令，电脑执行一个操作。就像你对 Siri 说"帮我打开备忘录"，只不过命令行是用键盘打字而不是说话。

**在 macOS **上，命令行工具叫做 Terminal（终端）。打开它最快的方式：

1.  按下 Command + Space 打开 Spotlight 搜索

1.  输入 **Terminal**（或者输入"终端"也行）

1.  按回车

你会看到一个窗口，里面有一行光标在闪烁——这就是命令行，等着你输入指令。

> 💡我知道很多同学看到终端的黑色/白色窗口会有点紧张——"这也太程序员了吧"。但说真的，后面你会发现命令行其实比图形界面更高效。就像学开车时你觉得手动挡很难，但熟练了之后你会发现手动挡比自动挡有更精确的控制。不要怕它，它是你的朋友。

### 基础命令速览

后面的操作中你会用到一些基本命令，先认识一下：

| 命令 | 作用 | 示例 | 类比 |
| --- | --- | --- | --- |
| cd | 切换目录 | cd ~/Desktop（进入桌面） | 打开文件夹 |
| ls | 列出当前目录的文件 | ls（看看有什么文件） | Finder 中查看文件列表 |
| pwd | 显示当前所在路径 | pwd（我在哪？） | 看 Finder 窗口标题栏 |
| which | 查找命令在哪里 | which node（node 装在哪？） | 在 Finder 中搜索文件 |
| echo $PATH | 查看环境变量 PATH | echo $PATH（电脑去哪找程序？） | 查看系统设置 |

> **怎么看这张表**：左边是命令，中间是作用和示例，右边是 Finder（macOS 的文件管理器）中的类比操作。不需要背下来，用到的时候回来查就行。

下面我们在终端里实际运行一下这 5 个命令，直观感受它们的效果：

**cd**** — 切换目录**：cd ~/Desktop 进入桌面目录，再用 pwd 验证确实切换过去了。

**ls**** — 列出文件**：进入桌面后，ls 会列出桌面上所有的文件和文件夹，和你在 Finder 中看到的内容一致。

**pwd**** — 显示当前路径**：随时用 pwd 确认"我现在在哪个目录下"，就像看 Finder 窗口上方的路径栏。

**which**** — 查找命令位置**：which node 告诉你 Node.js 的可执行文件装在了哪个路径下。

**echo $PATH**** — 查看环境变量**：显示电脑搜索命令时会依次查找的所有目录路径（用 : 分隔）。

> 💡不需要记住这些命令的每个细节，后面用到的时候你可以随时回来翻阅。现在只需要有个印象：命令行就是"用文字告诉电脑做什么"，和你在 Finder 中点击操作本质上是一回事。

### 什么是 PATH 环境变量？

这个概念**非常重要**，因为后面安装 OpenClaw 时你会直接用到它。如果你只记住本节一个知识点，就记住这个。

**PATH 就像电脑的"黄页"**——当你在命令行输入一个命令时，电脑会按照 PATH 中列出的路径，一个一个去找对应的程序。如果所有路径都找遍了还没找到，就会报错：

```
zsh: command not found: xxx
```

> 💡command not found 可能是你学编程过程中遇到最多的错误信息之一。十有八九不是"没安装"，而是"安装了但电脑不知道在哪"。理解了 PATH，你就掌握了解决这类问题的万能钥匙。

来看一下你的 Mac 上 PATH 里有哪些路径：

```
# 查看 PATH 环境变量
# echo  → 打印/输出
# $PATH → PATH 变量的值（$ 符号表示&quot;取变量的值&quot;）
$ echo $PATH
/Users/你的用户名/.local/bin:/opt/homebrew/bin:/opt/homebrew/sbin:
/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin:...
```

看到那一长串用 : 分隔的路径了吗？每个路径都是一个"搜索目录"。当你输入 node 命令时，电脑会依次去这些目录里找 node 程序。

让我们验证一下。用 which 命令查看 node 装在哪：

```
# which 命令：查找命令的可执行文件位置
$ which node
/usr/local/bin/node

$ which npm
/usr/local/bin/npm
```

/usr/local/bin 正好在 PATH 列表中，所以 node 和 npm 命令能被找到、能被运行。

**记住这个概念**——等下安装 OpenClaw 时，你会遇到一个"经典坑"，而解决方案就是修改 PATH。到时候你回来看这一节，一切就豁然开朗了。

---

## 1.5 OpenClaw 下载与安装

终于到了本章的核心环节——安装 OpenClaw。

### 三种安装方式概览

| 安装方式 | 命令 | 适合人群 | 优点 | 缺点 |
| --- | --- | --- | --- | --- |
| **一键安装脚本** | curl -fsSL https://openclaw.ai/install.sh | bash | 新手首选 | 自动检测环境、一条命令搞定 | 过程不透明 |
| **npm 全局安装** | npm install -g openclaw@latest | 开发者推荐 | 过程透明、便于管理 | 需要手动配置 PATH |
| **Git 源码安装** | git clone ... + pnpm install | 高级用户 | 可修改源码 | 步骤最多 |

### 实操：使用 npm 安装 OpenClaw

打开终端，输入以下命令：

```
# npm install -g openclaw@latest 各部分含义：
#   npm       → Node.js 的包管理器（Package Manager）
#   install   → 安装
#   -g        → global，全局安装（整台电脑都能用，而非仅当前项目）
#   openclaw  → 要安装的软件包名称
#   @latest   → 安装最新版本（也可以指定版本号如 @2026.3.13）
$ npm install -g openclaw@latest
```

按下回车，等待安装完成。

安装过程中，你会看到 npm 在下载和解压软件包，有一些进度条和警告信息：

安装完成后，你会看到类似这样的输出：

```
npm warn deprecated node-domexception@1.0.0: Use your platform&#x27;s native DOMException instead

added 541 packages in 32s

89 packages are looking for funding
  run `npm fund` for details
```

> ⚠️不要被 warn deprecated 的警告信息吓到！这不是错误，只是告诉你有些底层依赖包使用了旧版 API。OpenClaw 团队会在后续版本中处理这些警告，完全不影响使用。

看到 added 541 packages in 32s，说明安装成功了——541 个软件包，32 秒搞定。npm 自动帮你下载了 OpenClaw 以及它所有的依赖库。

让我们来解读一下这几行关键输出：

| 输出内容 | 含义 |
| --- | --- |
| added 541 packages | 安装了 541 个软件包（OpenClaw + 全部依赖） |
| in 32s | 安装耗时 32 秒 |
| 89 packages are looking for funding | 89 个开源包在寻求资金支持（提示信息，可忽略） |

### ⚠️ 经典坑：command not found

安装完成了，迫不及待想试试？在终端输入：

```
$ openclaw
zsh: command not found: openclaw
```

**什么？刚装完就找不到了？**

别慌，这不是 bug，而是 PATH 的问题——还记得上一节讲的"黄页"吗？

### 问题分析：为什么找不到？

npm 把 OpenClaw 安装到了哪里？我们来查一下：

```
# npm prefix -g 的含义：
#   npm prefix → 查看 npm 包的安装前缀路径
#   -g        → 查看全局安装的路径（不加 -g 是查看当前项目的）
$ npm prefix -g
/Users/你的用户名/.npm-global
```

所以 OpenClaw 的可执行文件在 /Users/你的用户名/.npm-global/bin/openclaw。

但是你的 PATH 中有 /Users/你的用户名/.npm-global/bin** 这个**路径吗？回去看看 echo $PATH 的输出——没有！

这就是问题所在：程序装好了，但电脑不知道去哪里找它。

用一张流程图来理解整个过程：

```
flowchart LR
    A[&quot;你输入 openclaw&quot;] --&gt; B{&quot;电脑在 PATH 中逐个路径搜索&quot;}
    B --&gt; C[&quot;/usr/local/bin/ ❌ 没有&quot;]
    B --&gt; D[&quot;/usr/bin/ ❌ 没有&quot;]
    B --&gt; E[&quot;/opt/homebrew/bin/ ❌ 没有&quot;]
    B --&gt; F[&quot;... 其他路径 ❌ 都没有&quot;]
    F --&gt; G[&quot;💥 command not found&quot;]

    H[&quot;解决方案：把 ~/.npm-global/bin 加入 PATH&quot;] --&gt; I{&quot;电脑重新搜索&quot;}
    I --&gt; J[&quot;~/.npm-global/bin/ ✅ 找到了！&quot;]
    J --&gt; K[&quot;🎉 OpenClaw 启动&quot;]
```

### 解决方案：配置 PATH

解决方案很简单——把 npm 的全局安装路径添加到 PATH 中。依次执行以下三条命令：

```
# 第一步：确认 npm 全局安装路径（先看清楚路径是什么）
# npm prefix -g → 查看 npm 全局安装的前缀路径
#   -g         → global，表示查看&quot;全局&quot;路径而非当前项目
$ npm prefix -g

# 第二步：把 npm 全局 bin 目录添加到 PATH
# echo &#x27;...&#x27;   → 输出引号中的文字
# &gt;&gt;           → 追加到文件末尾（&gt;&gt; 是追加，&gt; 是覆盖，千万别搞混！）
# ~/.zshrc     → macOS 终端的配置文件（zsh shell 的启动配置）
$ echo &#x27;export PATH=&quot;$HOME/.npm-global/bin:$PATH&quot;&#x27; &gt;&gt; ~/.zshrc

# 第三步：让配置立即生效
# source → 重新读取并执行指定文件中的命令
# 如果不执行 source，需要关闭终端重新打开才能生效
$ source ~/.zshrc
```

来逐字解释 export PATH="$HOME/.npm-global/bin:$PATH" 这行配置：

| 部分 | 含义 |
| --- | --- |
| export | 声明环境变量（让它对所有子进程可见） |
| PATH= | 设置 PATH 变量的值 |
| &quot;$HOME/.npm-global/bin&quot; | npm 全局安装目录下的 bin 文件夹 |
| : | PATH 中的路径分隔符（macOS/Linux 用冒号） |
| $PATH | 原来的 PATH 值（把新路径加在最前面，这样优先搜索） |

> **怎么看这张表**：整条命令的效果是"在原有的 PATH 前面加上 npm** **全局安装路径"。这样电脑搜索命令时，会先去 ~/.npm-global/bin/ 找，找到了就直接用，找不到再去其他路径找。

> 💡~ 是你的home目录的缩写，在 macOS 上通常是 /Users/你的用**户名。.**zshrc 文件名以 . 开头，表示它是一个隐藏文件，在 Finder 中默认看不到（按 Command + Shift + . 可以显示隐藏文件）。

### 验证安装成功

现在再试一次：

```
# openclaw --version 的含义：
#   openclaw  → OpenClaw 的命令行工具
#   --version → 参数，显示当前安装的版本号
$ openclaw --version
OpenClaw 2026.3.13 (61d171a)
```

**成功了！** OpenClaw 2026.3.13——这就是目前的最新版本。括号里的 61d171a 是 Git commit 的短哈希值（标识代码版本的唯一 ID），可以忽略。

> 🎉 里**程碑时刻：**恭喜你！这是本课程的第一个重要里程碑——OpenClaw 已经成功安装在你的 Mac 上了。

### 运行系统诊断

OpenClaw 自带一个"体检"命令 openclaw doctor，可以全面检查系统状态。让我们来做一次"出厂体检"：

```
# openclaw doctor 的含义：
#   openclaw → OpenClaw 的命令行工具
#   doctor   → 子命令，运行全面的系统诊断
#              （类比：去医院做体检，检查各项指标是否正常）
$ openclaw doctor
```

命令执行后，你会看到一个漂亮的 ASCII Art Logo（龙虾图案），然后是一系列诊断结果：

```
▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
██░▄▄▄░██░▄▄░██░▄▄▄██░▀██░██░▄▄▀██░████░▄▄▀██░███░██
██░███░██░▀▀░██░▄▄▄██░█░█░██░█████░████░▀▀░██░█░█░██
██░▀▀▀░██░█████░▀▀▀██░██▄░██░▀▀▄██░▀▀░█░██░██▄▀▄▀▄██
▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀
                  🦞 OPENCLAW 🦞
```

别被这些提示吓到——这些都是**正常的**。因为我们只做了安装，还没有进行任何配置。来逐项解读：

| 诊断项 | 状态 | 含义 | 需要你做什么 |
| --- | --- | --- | --- |
| Gateway | ⚠️ mode unset | Gateway 还没配置运行模式 | Part 2 中配置 |
| Gateway auth | ⚠️ off | 认证还没设置 | Part 2 中配置 |
| State integrity | ⚠️ missing | 配置目录 ~/.openclaw 还没创建 | 首次配置时自动生成 |
| Security | ℹ️ no warnings | 没有安全警告 | 暂时不需要操作 |
| Skills status | ✅ 9 eligible | 9 个内置技能已就绪 | 正常 |
| Plugins | ✅ 7 loaded | 7 个插件已加载 | 正常 |
| Gateway | ℹ️ not running | Gateway 服务还没启动 | Part 2 中启动 |
| Gateway connection | ℹ️ ws://127.0.0.1:18789 | 将在本地 18789 端口运行 | 正常 |
| Gateway service | ℹ️ not installed | 系统服务还没安装 | Part 2 中安装 |

> **怎么看这张表**：左边是检查项，中间是当前状态和含义，右边告诉你需要做什么。凡是标注"Part 2 中配置"的，都是我们下一章要做的事情。现在全部是正常状态——因为我们才刚安装好，还没有配置任何东西。

核心信息就是：OpenClaw 已经成功安装，等待首次配置。配置的工作我们留到下一章。

> 💡openclaw doctor 是你以后排**查问题的第一站。**任何时候 OpenClaw 出了问题，先跑一次 openclaw doctor，它会告诉你哪里不对劲。就像去医院，先做个全面体检，再对症下药。

### 补充：sharp 编译问题处理

如果你在安装过程中遇到了类似这样的报错：

```
npm ERR! sharp: Installation error: ...
npm ERR! sharp: Please try to install with: SHARP_IGNORE_GLOBAL_LIBVIPS=1
```

这通常是因为你的 Mac 上通过 Homebrew 安装了 libvips 库，与 OpenClaw 的 sharp 模块（一个图像处理库）产生了冲突。

解决方案：

```
# SHARP_IGNORE_GLOBAL_LIBVIPS=1 的含义：
#   这是一个环境变量，告诉 sharp 模块&quot;忽略系统上已有的 libvips 库，
#   使用自己内置的版本&quot;
#   放在 npm install 前面，表示&quot;仅在这次命令执行时生效&quot;
$ SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g openclaw@latest
```

> ⚠️这个问题只有在你之前通过 Homebrew 安装过 libvips 或 vips 时才会出现。如果你的 Mac 是全新的、从来没装过 Homebrew，大概率不会遇到这个问题。遇到了也不要慌——上面一条命令就能解决。

---

## 本章小结

恭喜你完成了 OpenClaw 的安装！让我们回顾一下这一章走过的路：

| 步骤 | 完成状态 | 关键命令/操作 |
| --- | --- | --- |
| 了解 OpenClaw 是什么 | ✅ | 本地优先的 AI 助手操作系统，321K Stars |
| 了解最新版本 | ✅ | 2026.3.13，日历版本（CalVer）命名 |
| 安装 Node.js | ✅ | 官网下载安装器，v24.14.0 |
| 学习命令行基础 | ✅ | Terminal、PATH 环境变量概念 |
| 安装 OpenClaw | ✅ | npm install -g openclaw@latest |
| 解决 PATH 问题 | ✅ | 写入 ~/.zshrc，source 生效 |
| 验证安装 | ✅ | openclaw --version → 2026.3.13 |
| 运行系统诊断 | ✅ | openclaw doctor → 等待首次配置 |

> **怎么看这张表**：这是你本章学习进度的"成绩单"。所有步骤都打了 ✅，说明你已经完整走完了安装流程。右侧列出了每步的关键操作，方便你以后回顾查阅。

现在 OpenClaw 已经安装好了，但它还是个**"空壳"**——没有接入大模型的大脑，不知道该怎么思考，也没有配置好服务。就像一台刚组装好的新电脑，硬件齐全了，但还没装操作系统、没连上网。

**下一章**，我们就来给它"接上大脑"：注册大模型 API、完成首次配置、启动 Gateway 服务，然后通过 Dashboard 和你的 AI 助手进行第一次对话。那将是真正激动人心的时刻——你亲手搭建的 AI 助手，开口说出第一句话。

---

> **Part 1 核心命令速查**

| 命令 | 用途 |
| --- | --- |
| node --version | 查看 Node.js 版本 |
| npm --version | 查看 npm 版本 |
| npm install -g openclaw@latest | 全局安装 OpenClaw |
| npm prefix -g | 查看 npm 全局安装路径 |
| echo &#x27;export PATH=&quot;$HOME/.npm-global/bin:$PATH&quot;&#x27; &gt;&gt; ~/.zshrc | 添加 PATH 配置 |
| source ~/.zshrc | 使 PATH 配置立即生效 |
| openclaw --version | 查看 OpenClaw 版本 |
| openclaw doctor | 运行系统诊断 |