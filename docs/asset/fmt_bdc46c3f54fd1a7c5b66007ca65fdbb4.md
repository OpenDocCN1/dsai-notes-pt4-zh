
# 保姆级教程：花2小时做一个能成交的单页网站

> 来源：[https://xuqiwei1986.feishu.cn/docx/GnWQd2EtBoJXGhxKMDQcLKusnOc](https://xuqiwei1986.feishu.cn/docx/GnWQd2EtBoJXGhxKMDQcLKusnOc)

大家好，我是本次`claudecode`航海的教练智昊，我的目标是在2026年做出超越小排老师的AI编程课。

本次航海中我直播了两次，一次是做一个落地页，一次是做一个游戏门户，这两次直播内容都比较多，也有不少圈友感兴趣。因此我决定写两篇帖子详细讲一下这两个项目怎么玩。

为什么用`claudecode`？因为这种看起来是静态的东西基本出不了什么bug，还可以多开几个组件一起跑，只有`claudecode`、`gemini-cli`或者`droid`这类`cli` coding工具可以胜任，`codex`杀鸡用牛刀，`cursor`没办法多开。

而且，本教程没有使用到较为复杂的subagent、skill或者SDK相关的内容，就是和`claudecode`纯聊天，复刻难度极低，但是效果极好，在这个网站上线之后不到24小时成交了4单，要知道这个期间内我暂时还没有发一条朋友圈或者做一次群发消息。

而且，做了这么多次教程开发，我认为这类网站demo是唯一一个让我们迅速了解`nextjs`框架，学会组件使用和调整，弄清网站发布全流程的最优解。无论是之后做AI工具站还是做个人官网，有了这次开发经验后续的学习难度都呈指数级下降，这可比之前从纯前端`html`+`css`+`js`跳到`nextjs`一脸懵可好太多了。

说的太多了，就先说到这，让我们开始这一次的教程吧！

本次开发出来的demo使用的是`claudecode`，GLM4.7作为底模完成的开发。

先看成品：

![](img/9ed3e0b66b59c06fdcc3a833e0a9fa16.png)

如果你也想做一个可以带来成交的landing page，那么这篇文章非常适合你。

在这篇文章里，我会告诉你：

1. 什么是Landing page
2. 怎么制作自己的Landing Page
3. 怎么发布自己的Landing Page

## Landing Page是什么？

顾名思义，Landing Page 叫落地页，你可以理解成用户进入你网站的时候看到的第一个页面，就像一个人要进商场购物时被塞到手里的传单。

它有两个核心特征：

+ 单一入口：它是用户从外链（🧣、👸号、X、reddit 等等）进到网站的第一个页面；
+ 单一目标：整个页面的设计、文案、按钮，都在大声地对用户说同一件事——“请做这个动作”。这个动作可能是“输入邮箱”、“购买课程”、“下载软件”或者“加入等待名单”。

![](img/b09651df8bc8297629ff550c82d0ef40.png)

为了让用户愿意买我们的东西，我们会塞上一大堆自夸的东西。一般来讲会包括：

+ 官网标题：告诉用户我们是谁，我们的 slogan 是啥；
+ 信任背书：有多少用户已经用了我们的产品或者服务；
+ 核心价值：我们为用户提供的价值是什么
+ 业务流程：如果你买我们的服务，我们会怎么为你服务
+ 用户评价：买过我们服务的用户都说我们好；
+ 客户召唤：来买我们吧！

## 谁需要Landing Page

![](img/48cc673c5a33bc9902e8761ccda6f079.png)

你的业务可能不需要一个网站，但是你的业务肯定需要一个 landing page，毕竟谁会拒绝给自己的产品做一次如此精细化的宣传呢？根据我之前的培训和服务经验，一般 Landing Page 的出现场合在以下几个场景：

+ 企业官网：比如你有一家公司是提供 `xxxx` 服务的，你需要一个网页去给用户介绍自己；
+ 社交 Club：我们做了一个社群，我需要让别人知道我们的社群，加入我们；
+ 专业咨询服务：特别是那种摸不到实物就需要做各种佐证向用户证明自己厉害选自己的业务，落地页做的一个比一个花里胡哨；
+ 个人开发者/知识博主：比如你要卖一个训练营，或者介绍一下自己的作品，你会做一个这样的网页来向用户推销自己；
+ 需求验证：这个在很多 Micro SaaS 出海的圈友那里出现，就是什么东西都还没有的情况下，先做个 Landing Page 让别人留下联系方式或者索性直接就开卖，如果有好多人买就做。

我们容易发现，Landing Page 实际上是一个设计，或者说文案要求远大于技术要求的工作，也就是说，我们的大部分时间实际上都在琢磨怎么让用户信任自己买自己的东西。

## 怎么制作自己的Landing Page？

了解了什么是 Landing Page 之后我们不难发现，落地页并没有什么特别复杂的交互，无非就是一个特别长但是特别漂亮的网页，然后放两个按钮，一个是“买我！”，另一个也是“买我！”。

![](img/84ea443ebc878af58ec89fa16c6409ac.png)

既然这样，我们就可以让 AI 帮我们设计落地页的文案，然后选择一些好看的营销组件把内容填充进去，最后给落地页披上一件好看的外衣，这么着，Landing Page 就做完了。是不是很简单！

为了让这个 Landing Page 的案例看起来更加真实，我用我自己的业务为例。在航海结束当天，我一拍脑袋决定做一个《业务 AI 编程》手册，我要采取 write in public 策略，也就是还没写就先卖，这个天生符合 landing page 的应用场景。现在，就让我们为这个手册搭一个 Landing Page。

### 开始前的准备工作

在正式开始前，我们要先做一些准备工作，这是清单。

+ `Git`安装
+ `nodejs`安装
+ `vscode`安装
+ `Github`注册和登录
+ `Vercel`注册和登录

如果这些东西你都做完了，可以直接跳过。如果没有，就先根据下面的指南先做完吧，挑你还没做的做完。

#### Git安装

如果要使用 `github` 管理代码，首先我们需要安装 `git`。

让我们进入 `git` 官网：`https://git-scm.com/downloads`

![](img/87ab647bed222c6024fbd64d9545b5eb.png)

下载你系统版本的 `git`，`windows` 的就下载 `windows` 的，`mac` 的就下载 `macOS` 的，`linux` 的就下载 `Linux` 的。

系统会根据实际情况自动弹出安装方法，比如 `macOS` 要用 `brew` 进行安装，`windows` 直接下载安装包安装。

##### mac 安装

`mac` 需要先安装 `homebrew`。进入 `homebrew` 官网：`https://brew.sh/`，然后点下面命令行右边的复制。

![](img/85117778d37a53ebd877ff6630ac67de.png)

打开终端，把刚才复制的代码粘贴到终端里，回车。按提示输入开机密码，回车；在确认安装，回车。然后它就开始安装或者升级了。

![](img/0377746e383b81172f27cd0717a0fda8.png)

过了一会就安装好了。安装好了之后在终端输入`brew install git`，回车。过了一会 `brew` 就会提示 `git` 安装好了。

![](img/3b3579f9d1e692ae56216da9fa5149b8.png)

安装好或者检查自己电脑是否有 `git`，直接打开终端，输入`git --version`，回车即可。如果你看到了版本号，说明安装成功了。

![](img/2b38a13c6f052abe6769567c5d09605d.png)

##### windows 安装

到达 `windows` 的安装界面，根据电脑的型号选择安装的版本。绝大多数情况，`amd` 或者 `intel` 的 `cpu` 都可以选择 `x64` 的版本，所以无脑选 `x64`，如果确认不了安装什么版本，你可以问一下 AI。

![](img/5375530d32f5b9cfab505a3ba8a10717.png)

下载好了之后就一路安装往前点击下一步就行。

注意，到了下面这个选择组件页面的时候，如果你不知道选择什么，你就全选，全选准没错。

![](img/c26e5852544754c230ecd0e829eadfa3.png)

安装好或者检查自己电脑是否有 `git`，直接打开终端，输入`git --version`，回车即可。如果你看到了版本号，说明安装成功了。

![](img/8b884e90bd717be22984595e8fbd4634.png)

#### nodejs安装

首先我们进入 `nodejs` 官网：`nodejs.org`。

进入官网后，点击官网页面上的 Get Node.js 进入 `nodejs` 的下载页面。

![](img/bf552e3b6ef31931c59f218c817bbe48.png)

进入下载页面后，不要用上面的命令行安装，对于一般同学来说还是太繁琐，你就直接往下拉找到一键安装包。

![](img/a5ecaa69d66d56af73a8e6aaf07760e0.png)

##### macOS 安装

如果你是 Mac 电脑的话，如果是 M 芯片你就选 `ARM64`，如果是 Intel 芯片你就选 `x64`。同样，选择 `installer` 进行下载。

![](img/40e74601a4ccc8528ca82bb49a95938d.png)

下载完成后进行安装，安装方式也极为简单。一直点继续就可以了。

然后到这，就安装好了。

![](img/1436222ce9c78615b8c732dd5d1de804.png)

##### windows 电脑安装

如果你的电脑是 `windows` 电脑，一般情况下选择 `x64`（64 位系统），如果你的电脑比较老，那就选择 `x86`（32 位系统），我说的老，就是那种内存条插不到 8 个 G 的老电脑，否则你都选择 `x64`，就像下面这么选。选完了之后选择下载。`msi` 的 `installer` 文件。

![](img/3945ccf09be97f29606895bf5319871b.png)

下载完成之后直接点击安装，进入安装页面。

![](img/4fc81f9e371877c572c7a178361446d5.png)

`windows` 电脑也是一样一直继续就行。

安装完成之后，像验证 `python` 一样，进入终端，然后输入`node -v`，回车。

进入终端的方法再强调一次：

1. `Windows` 系统 ：按 `Win+R` 键，输入 `cmd` 后回车打开终端。
2. `macOS` 系统 ：按 Command（⌘）+空格键打开 Spotlight 搜索，输入`Terminal`或`终端`并回车。

![](img/2dd4b1e7842df19b354e4fb8517855ef.png)

如果正常返回了版本号，则表示 `node` 安装成功了。

#### vscode安装

接下来我们继续安装 `vscode`。首先进入 `vscode` 官网：`https://code.visualstudio.com/`

![](img/f335c5332e4e92c2921137bfa0dd95b8.png)

然后直接点击画面中间的 Download for `xxx`，如果你是 `windows` 电脑，它就会显示 download for `windows`，如果你是 `mac` 电脑，它就会显示 download for `macOS`。

![](img/d86866efae484ff9e0e7bebfb052c92b.png)

你看到下面这个画面之后，它就会自己下载安装 `vscode` 了。

![](img/b20ef9b130045e38fd5c95eff4b44a12.png)

##### mac 安装

`macOS` 会相对简单，打开直接就是 `vscode` 的启动包，把启动包直接拖动到应用中心即可。

注意！一定要拖到应用中心里面！不能直接打开，直接打开有很多权限问题！

![](img/a6acd02d7508772ad8891b7807c96b8d.png)

如果你在页面的个人收藏里看不到应用程序，可以通过设置打开。

![](img/f1761d63996b94315d90901017962ed2.png)

首先，打开访达。在状态栏点击“设置”。

![](img/bd4f69d921fef8c39b32bec531ce7c3f.png)

在访达设置中，勾选“应用程序”。

![](img/8f14d8114445a65ca01c0d1caab2ae1f.png)

这个时候你就能在访达中看到它了。然后将 `vscode` 拖入应用程序文件夹即可。

安装完成后，在桌面或者启动台找到 `vscode` 并打开。打开之后长这样。

![](img/f632c74596f10692d320ce4a3575e472.png)

至此，`vscode` 就安装好了。

PS：如果你第一次使用 `vscode`，它让你照指南配置，你可以跟着它的配置去做，或者直接不管。不管也没什么问题。

![](img/4247ed8175bdb8e7c7ba2b6b7bd76fe3.png)

##### windows 安装

`windows` 下载完成之后根据指引直接安装，这里就直接一直按下一步就行。尽量安装在系统盘。

![](img/3006f057a190b7216e171c9ae0cdef9c.png)

默认选择的时候，注意把`PATH`给加上，不过它默认本身就装上的。

![](img/8e22f6933a88e0d12a7768742d885f8c.png)

为了环境变量能够正常生效，安装完成后建议重启电脑。

#### Github注册与登录

##### 注册 github 并登录

打开 `github` 官网：`https://github.com/`，点击右上角的 sign up。如果你已经有账号了，点击 sign in 登录就行。

这里不推荐直接使用 `google` 账号登录，建议先注册 `github` 再绑定 `google` 账号。

![](img/77c1de50a101e5f140ead3b88a59d4d8.png)

输入你的注册信息，什么姓名、邮箱、地区这些，输完了之后按 `create account`。

![](img/c5c1eeefc77d0e2b1b2fef829044a605.png)

它会进行校验。完事后点击 `create account` 即可。

![](img/adb1b6dd47d29350b125a4fd814848cf.png)

然后进行邮箱认证。

![](img/d1e340dd448f6369626c8b7b826969b9.png)

到你的邮箱里去看一下验证码。

![](img/eefb6a76bf171c415b9073c1dbd009dc.png)

把验证码输入回到认证的页面，输入完成之后注册就成功了。系统会自动跳转到 `github` 登录页。输入你刚刚注册的账号密码，点击 sign in。

![](img/70ea138212e5d26d7d079ebef94762e4.png)

当你看到下面这个页面，你就登录成功了。

![](img/f47f7f43f301212aac5c192027bc97dd.png)

记住你 `github` 的 email address 和 `username`，接下来我们要通过 `git config` 进行认证。

##### Git 配置

如果没有进行 `git` 配置指定 email 和 `username`，在 `vscode` 当中上传 `git` 分支的时候就会报错。因此，我们需要提前进行 `git` 配置。

```
# 给git配置username
git config --global user.name 你的用户名
```

![](img/c06963e1e37c153f864b08b4ea3c3843.png)

看到如上所示，表明配置成功了，可以继续往下。

```
# 给git配置email
git config --global user.email 你的邮箱@example.com
```

![](img/bfa965a213a5a7a17a1585d1fd2e074f.png)

```
# 检查配置是否成功
git config --global --list
```

![](img/8e4380d8a70d75befaedbc12060e7ed8.png)

当 `user.email` 和 `user.name` 的时候，就成功了。注意，这个地方的邮箱和姓名要和你在 `github` 上面注册的邮箱和姓名保持一致。

#### Vercel注册与登录

`Vercel`的注册与登录相对简单，我们通过`github`账号登录就可以了。

进入 `Vercel`（官网链接：`vercel.com`），点击右上角的 log in。

![](img/321fd1e9bd43517e287bc311eb664111.png)

选择 log in with `github`。我们强烈推荐所有与代码相关的网站都用 `github` 登录（`bolt`、`v0`、`vercel`、`lovable`、`same` 全部都用 `github` 登录）

![](img/ce653d940e348f02e056404ccd40f8b3.png)

登录完成之后，进入 `vercel` 的工作台。工作台会展示你所有的网站。

![](img/21fd490240f079e9373d2c93cd59e4fa.png)

### 定骨架：和 AI 讨论 Landing Page 的结构和内容

打开 Gemini，你选择 chatGPT 或者 claude 或者 deepseek 或者 kimi 或者 glm 或者 Qwen 都行，我用 gemini 比较多，我就用它举例。

![](img/1fe223e1a1aeed743c8f348ff0992e32.png)

简单描述一下我现在要做的事情，然后让它帮我出出主意。我先把专栏的定位，我的个人介绍，还有推广思路，以及专栏的内容规划一并发给他，请他帮我出主意。
