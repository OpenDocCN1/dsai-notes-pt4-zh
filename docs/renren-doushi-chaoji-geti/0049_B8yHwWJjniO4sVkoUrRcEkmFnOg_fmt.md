
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



```
# 说明

我现在正在制作一个名为《技术平权：写给千万业务人员的AI编程实战手册》的专栏，以下是这个专栏的详细介绍：

## 专栏定位
本专栏是一套面向业务人员的“自然语言编程”实战指南，聚焦于“如何发现业务痛点”并“通过自然语言进行AI编程实现业务目标”。

目标受众： 拥有丰富业务经验但无代码能力的职场精英。他们清楚业务逻辑，但受限于技术门槛，无法将想法落地。

核心价值： 赋予用户“定义即交付”的能力。教用户如何敏锐捕捉工作中的低效痛点，通过“自然语言编程”将复杂的业务逻辑（如财务核算、销售管理）直接转化为可用的业务系统，彻底摆脱对 IT 排期的依赖。

作者经验： 基于作者 10 年 ERP 咨询与 AI 编程教学经验，提供从“痛点挖掘”到“系统交付”的标准 SOP。

## 自我介绍

背景经历： 我是前汉得信息 ERP 咨询顾问，拥有 9 年业务财务产品经理经验，一直在B端一线从事产品工作，有很强的业务场景理解和流程拆解能力。

教学经验： 资深 Vibe Coding 培训师，“生财有术”社区 AI 编程航海教练（主带 DeepSeek、MCP、ClaudeCode 航海）。线下已成功培训出 10+ 位能独立交付业务系统的 AI 工程师（覆盖小红书虚拟电商、自媒体工具、网站开发等领域）。

公众号付费专辑《ClaudeCode 编程入门》

## 推广思路

1. 冷启动策略： 利用我在“生财有术”社区的圈友资源，通过私聊精准触达进行首轮冷启动，建立种子用户群。

2. 裂变分销： 启动高佣金分销计划（初期 40% 佣金），激励种子用户裂变，目标冲刺 1000 份销量后恢复至 20% 佣金。

3. 公域引流： 朋友、微博和X同步进行 Write in Public (公开写作)，实时分享专栏写作进度与案例，打造围观效应。

4. 渠道合作： 利用个人网站 `enchenggc.org` 进行 SEO 引流；同时对接其他 AI 垂直社群负责人进行定向推广。

## 内容规划

第一个章节，我想把我这几次线下培训看到大家的心魔还有学习障碍整理出来，命名为元学习模块。

第二个章节，我想把之前我和AI讨论编写产品方案的过程教给大家，如何挖掘你自己的需求，编写和调试 AI 提示词，做一个你想做的东西，服务你自己或者你的客户、你的朋友、你的小孩，进而影响和你相同岗位或者相同工作内容的其他人。

第三个章节，开始做一些编程的实战内容，这里主要做单网页，比如做一个转盘，一个小游戏，一个批量化脚本。

第四个章节，我想把数据结构和功能结构给大家讲清楚，什么时候你可能只需要做一个网页就够，什么时候你可能要上一个网站，什么时候你可能需要接一个数据库。

第五个章节，开始深入研究氛围编程，不需要装环境，不需要设置环境变量，也不需要起本地服务，做出一个生产级可用的小系统。这个阶段并不一定靠口喷编程，有可能会借助一些零代码平台，比如飞书多维表格，coze 等。

## 用户体验

我希望这个手册对于用户来讲是一次全新的开始，是由好奇心驱动的一次快乐旅程，而不是因为生存焦虑带来的又一次知识付费。

# 目标
我需要制作一个 landing page 宣传并促进该专栏的成交。请以营销专家的角度和我讨论该如何制作这个 landing page，包括结构、内容等。任何有问题的地方请先提问再制作，不要随意猜想或捏造，我需要保持内容真实可信。
```

过了一会，给我扔了一大堆文案出来。

![](img/cd9f160484f10b0c3aab1919e6acb314.png)

经过了一系列对话之后，我们拿到了 landing page 的最终需求文档如下。

```
**# Project ****Requirem****ent: &quot;****Techn****ology Equit****y&quot; Landing** **Page****

## 1. Proj****ect Overvie****w

Goal**: Build a static, high-fi**delity l****anding page**** for a digit****al colu****mn titled &quot;Te****chnology Equi****ty: AI Pr****ogramming**** Practical ****Handboo****k for M****illions ****of Business**** Personnel****&quot;.
Core Phil****osophy: &quot;Wr****ite in ****Public&quot; ****(WIP). ****This is NOT**** a marketing pa**ge for a high-ticket boot**camp. I****t is a ****&quot;proje****ct d**ocumentation&quot; s**ite for ****an o**ngoing writing **experime****nt.
**Tone: Humble, **transpar****ent, ra****w, logic****al, l****ow-pr****essur****e.
Ke****y Metap****hor: A**** &quot;Change****log&quot; or &quot;Pr****oject Log&quot; ****rather t****han a** sales brochure.

## 2. Tech Stack

Framework: Next.js (App Router)
Styling: Tailwind CSS
Icons: Lucide React
Font: Inter (for UI) + JetBrains Mono (for code/tags)
Components: Shadcn UI (optional, for basic structure) or raw Tailwind.
Animation: NONE. The site must be static, fast, and crisp.

## 3. Visual Identity System (Theme: Tech-Rational)
Concept: Blueprint, Logic, Structure, Evolution.
Reference Style: Clean documentation sites, changelogs, engineering dashboards.

### 3.1 Color Palette
Backgrounds:
Main: #FFFFFF (White)
Section Alt: #F8FAFC (Slate-50)
Dark Accents: #0F172A (Slate-900 / Deep Navy)
Typography:
Primary Text: #0F172A (Slate-900)
Secondary Text: #64748B (Slate-500)
Code/Tags: #0EA5E9 (Sky-500) or #06B6D4 (Cyan-500)
Accents (The &quot;Tech&quot; feel):
Primary Action: #0F172A (Deep Navy) with Cyan hover effects.
Highlights: #06B6D4 (Cyan-500).
Borders: #E2E8F0 (Slate-200).

### 3.2 UI Elements
Shapes: Sharp corners (rounded-sm or rounded-none). NO large rounded corners.
Texture: Subtle CSS Grid patterns on backgrounds.
Indicators: Use solid squares (▪️) and empty squares (▫️) for lists.
Decorations: Thin 1px borders, dashed lines, &quot;terminal-like&quot; labels.

## 4. Component Specifications

### 4.1 Hero Section (&quot;The Construction Site&quot;)
Layout: Centered content with a subtle grid background pattern.
Key Elements:
Status Badge: A small terminal-style badge at the top: STATUS: WRITING... (Monospace font, Cyan text, dark background).
Logo Placeholder: A rectangular placeholder (w-64 h-auto) for the &quot;Structure/Logic&quot; logo.
H1 Title: &quot;我正在写一本给业务人员的 AI 编程手册&quot; (Plain, bold, Slate-900).
Subtitle: &quot;这是一个为期 2 个月的 Write in Public 实验。我负责输出 10 年经验，你负责围观这套 SOP 是如何诞生的。&quot;
CTA Button: [ 解锁阅读权限 (¥59) ]. Style: Deep Navy background, white text, sharp corners. Static hover effect (slight color shift).
Disclaimer (Crucial): Small text below button: &quot;无作业 · 无打卡 · 纯阅读&quot;.

### 4.2 Concept Clarity (&quot;This is NOT a Course&quot;)
Layout: Two columns (side-by-side on desktop, stacked on mobile).
Design: Two simple bordered boxes.
Left Box (The Anti-Pattern): Gray border (border-slate-200), Gray header background.
Right Box (The Project): Cyan border (border-cyan-500), Dark header background (bg-slate-900).
Content:
Left Title: &quot;这不是一个训练营 (Bootcamp)&quot;
Left List:
这里没有助教催你交作业
我不会手把手教你配置环境
我不承诺你能转行成为程序员
Right Title: &quot;这是一个公开写作项目 (Open Project)&quot;
Right List:
公开我和 AI 磨合产品方案的真实记录
只有资深顾问才懂的“业务-代码”翻译逻辑
像看专栏一样，通勤路上花 10 分钟阅读

### 4.3 The Roadmap (&quot;The Writing Log&quot;)
Layout: Vertical timeline list.
Visuals: A vertical line (border-l) connecting the items. Use square bullets (▪️) for nodes.
Header: &quot;Current Schedule / 写作进度表&quot;
List Items:
v1.0：元学习篇 (已发布)
Desc: 整理学员心理障碍。先破除“怕代码”的心魔。
v2.0：需求挖掘篇 (Writing...)
Desc: 还原“如何跟 AI 描述需求”的全过程，包含失败的 Prompt 案例。
v3.0：实战篇 (Planned)
Desc: 计划做几个小工具（转盘、脚本）。不求完美，能跑通就行。
v4.0：架构篇 (Planned)
Desc: 什么时候该用数据库？这部分比较枯燥，但很重要。
v5.0：交付篇 (Planned)
Desc: 探索云端部署。目标是不装本地环境也能跑起来。

### 4.4 Origins / Sample (&quot;Observations&quot;)
Layout: Grid (2x2 or 3x1).
Design: &quot;Data Cards&quot; or &quot;Field Notes&quot; style. Simple white cards with borders.
Content Strategy: Do NOT use &quot;Success Stories&quot;. Use &quot;Observations&quot; or &quot;Samples&quot;.
Cards:
Tag: 样本 A：小红书博主 -&gt; Content: &quot;她不懂代码，却逼着 AI 写了个分发工具。这个案例让我意识到：欲望才是第一生产力。&quot;
Tag: 样本 B：心理咨询师 -&gt; Content: &quot;她做的游戏代码写得很烂，但用户很喜欢。这启发了我：业务价值 &gt; 代码质量。&quot;
Tag: 样本 C：一线教师 -&gt; Content: &quot;为了省下点名的时间，他硬是用 AI 磨出了工具。这是典型的低成本技术平权。&quot;

### 4.5 Author Log (&quot;The Note&quot;)
Layout: Simple ID Card style (Avatar left, Text right).
Visuals: Grayscale avatar.
Copy:
Name: 智昊
Role: 资深 ERP 咨询顾问 / 10 年 B 端产品经理
Note: &quot;我不是什么技术大牛。刚好这几个月有点空，想把肚子里关于‘业务与 AI’的存货倒出来。定价 59 元是为了逼自己一把（不写完不好意思收钱）。欢迎花两杯咖啡钱，以此为门票，进来看我‘盖楼’。&quot;

### 4.6 Footer
Layout: Sticky bottom bar (Height: 64px).
Style: bg-slate-900 (Dark), White text.
Content:
Left: &quot;Project: Tech-Equity Handbook&quot;
Right Button: [ 围观专栏 (¥59) ] (Cyan accent).

##5. Development Guidelines for Agent
Strictly Static: Do not use framer-motion or complex JS animations. Use standard CSS/Tailwind hover states only.
Responsive: Ensure the Roadmap and Concept Comparison stack vertically on mobile screens.
Typography: Use a monospaced font for tags (e.g., Writing..., v1.0) to reinforce the &quot;Log&quot; aesthetic.
Spacing: Use generous padding (py-16 or py-24) between sections to keep it clean and readable.
Copy: Use the exact Chinese copy provided in Section 4. Do not replace with Lorem Ipsum.
```

可以看到，总共有六个部分。

如果你对我的对话感兴趣，你可以通过这个链接看我和 AI 的沟通过程：

`https://gemini.google.com/share/b0c21b7b4422`

至于上下文过程中的提示词管理，给大家一些经验：

1. 不要被 AI 牵着鼻子走，一旦发现内容跑偏要及时提醒纠正；

1. 提示 AI 如果有不清楚的地方要进行提问，这其实是在做头脑风暴；

1. 时不时总结上下文，将最新的文档内容覆盖到最新的位置，之前的内容忘了就忘了；

1. 最后，让它输出一段提示词，明确告知这段提示词是给 coding agent 使用的。

这样，我们的 Landing Page 的结构、内容设计就已经全部做完了。

## 填血肉：使用营销组件填充内容

由于很多组件实际上都很基础，我们想做出一个特别好看的网站，我们需要借助一些模板。这里，我推荐大家使用 `tailark` 营销组件，这是一个开源的 `shadcn` 营销组件。当然你也可以选择 `21st.dev` 里面的付费组件。

![](img/6e69f9356e4c671d8647ce334ed68126.png)

由于它的基础组件是 `shadcnUI`，我们可以考虑 AI 非常擅长的 SSR 网站框架：`NextJS`。

现在开始内容变得硬核了，大家注意。

![](img/63109c636740f10144ba164ee2e121c1.png)

### 在本地起一个 `nextjs` 项目

先找一个你看得顺眼的地方新建一个文件夹，然后给这个文件夹命名，比如我这里命名为“aiworkbook”。

然后打开你的代码编辑器，我这里以 `vscode` 为例。

在welcome页面点击“open”，选择你刚刚新建好的文件夹，现在开始它就是我们的项目文件夹了。

进去之后，点击“Yes, I trust the authors”。然后你就进入了IDE，IDE的内容如下。

现在什么都没有，我们需要开始起 `nextjs` 项目了。

点击右上角的底栏按钮。

![](img/87d926c47f4ef636d68190d494debd49.png)

底栏弹出后，点击 `Terminal`。这个 `Terminal` 栏就叫“终端”。

然后我们打开浏览器，进入 `nextjs.org` 官网：`nextjs.org`。



![](img/ca8e8f9bc9a2a83fb54add7d6406adac.png)

点击左上角的 Docs。

在官方文档的页面左端，点击 Installation，然后在 Installation 页面找到 Quick start，在 Terminal 的选项里选择 `pnpm`，`pnpm` 包管理器相对来讲安装更简单。

![](img/e4c83b1ea7d2edfb44ead14ed764e607.png)

我们把这段指令复制出来。

```
pnpm create next-app@latest my-app --yes
cd my-app
pnpm dev
```

这三行指令的意思分别是：

1. 使用 `pnpm` 这个包管理器新建一个 `next-app` 应用，`@latest` 表示版本号选择最新版本，`my-app` 表示在当前文件夹下新建一个项目文件夹叫 `my-app`，`--yes` 表示按照默认配置安装；
1. `cd my-app` 的意思是进入 `my-app` 项目文件夹；
1. `pnpm dev` 的意思是使用 `pnpm` 启动开发服务器。

由于我们已经新建过项目文件夹了，所以我们得改一下这个指令。

```
pnpm create next-app@latest . --yes
pnpm dev
```

这里我把 `my-app` 换成了 `.`，这样就在当前文件夹安装 `nextjs` 应用了。复制这段代码块，进入 `vscode` 的终端粘贴进去，回车。

过了一会，看到 `Success` 就表明安装成功了。然后左边就“嗖”地多了一堆新文件。

然后，我们要把这个仓库直接传到 `github` 上。点击左边第三个图标（分支管理），接着点击 Publish Branch。

点击之后 `vscode` 会让你登录 `github` 账号，我们就正常登录就行。登录完成之后，顶上会提示要发布成什么仓库，这里我选择发布为 `private` 仓库（私有仓库），下面那个 `public` 仓库选项是公开仓库，大家可以自己按需选择。

过了一会，发布好了，我们可以在左下角看到一个 `main` 分支右边一个紫色的云朵就表示已上传到云端仓库。

![](img/f90b9cb6172f4c8dc8cc18756ff77916.png)

至此，`nextjs` 项目就起好了，我们可以进行下一步了。

## 安装 shadcn/ui

接下来我们要安装 `shadcn/ui`。首先进入 `shadcn/ui` 的官网：`ui.shadcn.com`

![](img/6d4077fef0fe5ad3d38547f21dbff0f8.png)

点击左上角的 Docs。

![](img/559a263c87aef9608ae5e76f98b74cde.png)

进入官方文档后，默认会进入 Installation 页面，然后在 Pick Your Framework 里面选择 `Next.js`。

![](img/c2df592086e6ef4abbd558b08cdf1ef4.png)

进入 `nextjs` 的页面后，在 Create projext 里对 `shadcn/ui` 进行初始化。我们在安装 `nextjs` 的时候选择的是 `pnpm`，所以我们在这里也使用 `pnpm`。复制这段命令。

![](img/fb684371cb7bc4874cee744dcb559197.png)

回到 `vscode`，把这段复制下来的命令粘贴到终端里，回车。

随便选一个，反正后面都要换。按上下选择，回车确认。

过了一会，安装好了。

这样 `shadcn/ui` 就安装好了。

## 新建 docs 文件夹，把 requirement.md 放进去

在根目录下点击第二个图标“新建文件夹”。

然后在新建的文件夹空白名字这里填上 `docs`，回车。

选中 `docs` 文件夹，点击第一个图标“新建文件”。

在新建文件的名字里输上 `requirement.md`，回车。

把在第定骨架那个步骤里 AI 给你提供的 `coding agent requirement` 提示词粘贴进去。

注意看，粘贴进去之后右边会有一个圆点，这个原点表示你还没保存。在没有保存的情况下给 `claudecode` 它读不出来，所以一定要保存。使用 `Ctrl+S`（Mac 电脑是 `Command+S`）快捷键进行保存。

保存完成后，原点就变成一个叉了，这个时候 `claudecode` 就能读到了。

> 说明：有同学可能会问，这个文件的绿色和黄色代表什么意思，绿色的意思就是这个文件自上次上传代码到 `github` 后新增的文件，文件的右边会显示 `U`；黄色的意思就是这个文件自上次代码上传到云端 `GitHub` 后，被修改过的文件的右边会显示 `M`。

## 初始化 claudecode

接下来，我们就可以初始化 `claudecode` 了。我们使用 `coding Agent` 的时候，一定要先做初始化。

点击右上角的 `Claudecode` 插件按钮，如果你没有使用插件，你可以在终端输入 `claude`，回车。我习惯使用 `native mode` 的 `claudecode` 版本，所以我的插件打开仍然是一个终端。

打开之后，输入 `/init`，然后把初始化的提示词输入进去。在我这个项目里，初始化提示词就是让他基于 `requirement.md` 进行初始化。所以我的提示词如下：

```
/init 请基于 requirement.md 初始化本项目
```

把初始化提示词弄进去，然后回车。紧接着就是 `claude` 一顿输出。

然后 `CLAUDE.md` 就做好了。

打开 `CLAUDE.md` 看一下是不是我们想要的。

页面结构正确，工作流程正确，配色风格正确。

注意：一定要去看 `CLAUDE.md` 的内容是否正确，看不懂就复制出来翻译出来检查，检查出有欠缺的地方一定要补充进去，补充的方式就直接给 `claudecode` 提就行。

比如这里，我发现我没有提 `tailark` 营销组件，于是我让 `claudecode` 把这部分内容塞到 `CLAUDE.md` 里。

![](img/d52f764dc8d5f4b558e8aafb74ce1098.png)

它改好了之后我们再检查一下。workflow 正常。

![](img/dc48d44bbfc9183f3096ec3dd2da3941.png)

编码规范正常。

![](img/94719dac5eb678093128e458d689fb81.png)

`globals.css` 规范要求正常。

![](img/68052bd100b520232a302f78694fd978.png)

这样，初始化就结束了。

> TIPS：大家会发现我给 `claude` 喂的提示词会带有技术性，这是系统学习了 `nextjs` 之后的结果，这也是为什么我一直和大家说入门了 AI 编程之后要尽快补齐 `coding` 能力的短板，这样在和 AI 协作的时候就不至于抓瞎。

## 注入 tailark 营销组件并按 requirement.md 修改文案

初始化完成后，我们就可以开始到超市买东西了。打开 `tailark` 官网：`tailark.com`

在官网首页，点击 “Explore blocks”，进入超市。

![](img/7e967a7b926e789bddbdae61d86a1b84.png)

我们能看到这上面有很多东西，什么 `Hero Section`、`Logo Cloud`、`Features`、`Integrations`、`Content`... 这些东西都是可以使用的组件，我们根据我们自己的 `Landing Page` 要求，选择适合我们内容的组件。

![](img/6b8ad5f4db1081d450182b973291de99.png)

我们先回顾一下 `landing page` 的内容，打开 `requirement.md`，看看第一屏。它的名字叫 `Hero Section`。

```
**### 4.1 Hero Section (&quot;The** Construction Site&quot;)
Layout: Centered content with a subtle grid background pattern.
Key Elements:
Status Badge: A small terminal-style badge at the top: STATUS: WRITING... (Monospace font, Cyan text, dark background).
Logo Placeholder: A rectangular placeholder (w-64 h-auto) for the “Structure/Logic” logo.
H1 Title: “我正在写一本给业务人员的 AI 编程手册” (Plain, bold, Slate-900).
Subtitle: “这是一个为期 2 个月的 Write in Public 实验。我负责输出 10 年经验，你负责围观这套 SOP 是如何诞生的。”。
CTA Button: [ 解锁阅读权限 (¥59) ]. Style: Deep Navy background, white text, sharp corners. Static hover effect (slight color shift).
Disclaimer (Crucial): Small text below button: “无作业 · 无打卡 · 纯阅读”。
```

这里有一个标签，然后有一个 `Logo Placeholder`，就是我们那张图，然后是 `H1 Title` 和 `Subtitle`，最后还有一个 `CTA button`。我们找一个看起来趁手的家伙事。

找了一圈，我发现 `Hero Section` 底下的第八个，这个看起来不错，有标签，有 `logo Placeholder`，然后 `H1` 和 `subtitle` 的位置也有，还有一个 `start building` 的按钮，和我们的 `CTA Button` 只有一个很合适。

![](img/ec49964a9009bdcb360e3c7e0238d93a.png)

直接点击这个组件右上角的命令，点击之后它会自动复制。

![](img/d52729658043f4e1a023c518605aab83.png)

还记得一开始吗，我们用的是 `pnpm` 包管理器，所以我们要看一下命令是不是 `pnpm ...`，如果不是，我们需要切换。

点击命令左边的那个图标，然后会弹出四个，选择我现在用的这个和俄罗斯方块一样的图标，这个就是 `pnpm`。

![](img/75cd9f259248b2d15b09c03ac96ac17b.png)

接着，我们就可以让 AI 帮我们把 `hero-section` 部分的内容给完成了。

回到 `claudecode`，我们已经给它过工作流了，所以直接说我们的诉求就行。

先重新开一个 `claudecode`，或者输入 `/clear` 清空上下文。然后开始大搞特搞。

```
请帮我把 Hero Section 部分的内容完成，使用的组件安装指令是pnpm dlx shadcn add @tailark/hero-section-8
```

提示词写好之后，直接回车。

![](img/ade8939fe2d3859ee21c50c605471e50.png)

过了一会它搞好了。

![](img/4031ecf15846d9b6d158731b67917db8.png)

接着我们让它启动服务器。

![](img/6d9b9c8245a6784446cdeb2078bd2b62.png)

服务器启动成功！我们根据它给我们提供的访问地址，打开浏览器看一下长啥样。

![](img/7ad201a792fb9703f8a981acd0900fcc.png)

打开浏览器，输入 `localhost:3000`，回车。

![](img/a39f03b25ae3d5fe71890c3ea1ecdc9a.png)

看起来像是那么回事。这个 `logo` 先不管了等下再补。

既然它干一个做的不错，我们索性生下来的让它一次全干完。我们看一下剩下的页面有哪些。然后去 `tailark` 给它一个个选完。

```
**### 4.2 Concept Clarity (&quot;Th**is is NOT a Course&quot;)
Layout: Two columns (side-by-side on desktop, stacked on mobile).
Design: Two simple bordered boxes.
Left Box (The Anti-Pattern): Gray border (border-slate-200), Gray header background.
Right Box (The Project): Cyan border (border-cyan-500), Dark header background (bg-slate-900).
Content:
Left Title: “这不是一个训练营 (Bootcamp)”
Left List:
这里没有助教催你交作业
我不会手把手教你配置环境
我不承诺你能转行成为程序员
Right Title: “这是一个公开写作项目 (Open Project)”
Right List:
公开我和 AI 磨合产品方案的真实记录
只有资深顾问才懂的“业务-代码”翻译逻辑
像看专栏一样，通勤路上花 10 分钟阅读
```

这个组件看起来是一个对比组件。左边是 “不是这样的东西”，右边是 “是这样的东西”。让我们找找合适的。

这个看起来不错，狂拽酷炫吊炸天。安装指令是：

```
pnpm dlx shadcn add @tailark/features-10
```

![](img/6efa7be8b9d77e01c75887b109430c6b.png)

再看下一个。

```
**### 4.3 The Roadmap** (&quot;The Writing Log&quot;)
Layout: Vertical timeline list.
Visuals: A vertical line (border-l) connecting the items. Use square bullets (▪️) for nodes.
Header: “Current Schedule / 写作进度表”
List Items:
v1.0：元学习篇 (已发布)
Desc: 整理学员心理障碍。先破除 “怕代码” 的心魔。
v2.0：需求挖掘篇 (Writing...)
Desc: 还原 “如何跟 AI 描述需求” 的全过程，包含失败的 Prompt 案例。
v3.0：实战篇 (Planned)
Desc: 计划做几个小工具（转盘、脚本）。不求完美，能跑通就行。
v4.0：架构篇 (Planned)
Desc: 什么时候该用数据库？这部分比较枯燥，但很重要。
v5.0：交付篇 (Planned)
Desc: 探索云端部署。目标是不装本地环境也能跑起来。
```

这是一个 `roadmap`，大概就是一个时间轴。我们找找。

这个叠起来的玩意可以一个一个展开的玩意看起来非常不错，就用它了。

![](img/2d4f8a7e0732f2dd0b1ca545b99ad9c6.png)

指令复制下来：

```
pnpm dlx shadcn add @tailark/features-12
```

再看下一个。

```
**### 4.4 Origins / Sample (&quot;Observations&quot;)
Layout: Grid (2x2 or 3x1).
Design: “Data Cards” or “Field Notes” style. Simple white cards with borders.
Content Strategy: Do NOT use “Success Stories”. Use “Observations” or “Samples”.
Cards:
Tag: 样本 A：小红书博主 -&gt; Content: “她不懂代码，却逼着 AI 写了个分发工具。这个案例让我意识到：欲望才是第一生产力。”
Tag: 样本 B：心理咨询师 -&gt; Content: “她做的游戏代码写得很烂，但用户很喜欢。这启发了我：业务价值 &gt; 代码质量。”
Tag: 样本 C：一线教师 -&gt; Content: “为了省下点名的时间，他硬是用 AI 磨出了工具。这是典型的低成本技术平权。”
```

这是一个用户样本。用 `Testimonials` 准没错。这个看起来就蛮好。

![](img/30515d3977270280331ebbc60e43dbab.png)

安装指令复制下来。

```
pnpm dlx shadcn add @tailark/testimonials-5
```

再看作者简介。

```
### 4.5 Author Log (&quot;The Note&quot;)
Layout: Simple ID Card style (Avatar left, Text right).
Visuals: Grayscale avatar.
Copy:
Name: 智昊
Role: 资深 ERP 咨询顾问 / 10 年 B 端产品经理
Note: “我不是什么技术大牛。刚好这几个月有点空，想把肚子里关于 ‘业务与 AI’ 的存货倒出来。定价 59 元是为了逼自己一把（不写完不好意思收钱）。欢迎花两杯咖啡钱，以此为门票，进来看我 ‘盖楼’。”
```

这个用 `Team` 模块就很好。

用这个吧，以后要是有人愿意往手册里帮我写点别的，给人弄个大头照。

![](img/5d5445f70facbb87f8fb3e314ef434f9.png)

安装指令是这个：

```
pnpm dlx shadcn add @tailark/team-2
```

最后是 `footer`，这个看起来不是 `footer`，是 `CTA`。

```
**### 4.6 Footer**
Layout: Sticky bottom bar (Height: 64px).
Style: bg-slate-900 (Dark), White text.
Content:
Left: “Project: Tech-Equity Handbook”
Right Button: [ 围观专栏 (¥59) ] (Cyan accent).
```

用这个吧。

![](img/f66dd2df58063e1a88c91f576b75ab40.png)

组件命令是：



# Landing Page 构建与部署全流程

```
pnpm dlx shadcn add @tailark/call-to-action-1
```

选完了。

然后就是让AI把它全部做完。

```
请帮我**把剩下部分的内容依次完成。
1. Concep**t Clar**ity (&quot;This is NO**T a Co**urse&quot;) 用** pnpm dlx shadcn add @tailark/features-10
2. The Roadmap (&quot;The Writing Log&quot;) 用 pnpm dlx shadcn add @tailark/features-12
3. Origins / Sample (&quot;Observations&quot;) 用 pnpm dlx shadcn add @tailark/testimonials-5 这一块案例不多，你可以按照真实案例仿照一些你认为的确存在的业务AI编程场景填充进去。
4. Author Log (&quot;The Note&quot;) 用 pnpm dlx shadcn add @tailark/team-2
5. Footer 用pnpm dlx shadcn add @tailark/call-to-action-1
```

粘贴进去，一个回车。

![](img/1ef3f30986ed749e140a4fd56a2853b4.png)

我们来看一下它能做成啥样子。

待办看起来像是那么回事。

![](img/e2a43e7687bab6241dbac6164fa70434.png)

（20 minutes later……）

![](img/3acbba7bffec64e66f1dbe9fc7a3c60c.png)

好像不是我们想的样子，我看了一下记录，发现它居然帮我基于已有的`tailark`组件做了一些创新和重构以满足`requirement.md`的内容，`tailark`里面的动画和结构都被保留了。

然后我们发现有一些图片没有，比如我的照片还有写作进度表里面也少图了。我们去搞点图片放进来。

先找`claudecode`帮我把图的要求弄出来。

![](img/2121f30ba0d74d6c61d753e9128b0a23.png)

然后我到之前在`gemini`里的聊天记录，让它帮我生成一下图片提示词。

![](img/24c16fbea4b51e13364b8a3a0e6ecd6b.png)

拿到提示词了：

```
**第一章：元学习 (Mindset)
Prompt:A floating 3D cube transitioning from chaotic pixelated noise on the left to a clea****n, smooth, transparent glass structure on the right. Representing data defragmentation and mental clarity. [Ins****ert Universal Style Suffix]
第二章：需求挖掘 (Logic)
Prompt:Scattered glowing data nodes connecting together**** to fo****rm a perfect, organized straight line or grid structure. Concept of logic organization and workflow op**timization. Schematic diagram style. [Insert Universal Style Suffix]
第三章：编程实战 (Code)
Prompt:Modular voxel blocks being assembled vertically, appearing like a building under construction. Concept of coding and simple tool building. Abstract geometric shapes floating around. [Insert Universal Style Suffix]
第四章：架构思维 (Structure)
Prompt:An isometric multi-layered platform structure. Heavy solid base foundation representing database, connected by pipes to a light floating top layer. Structural engineering concept, system architecture diagram. [Insert Universal Style Suffix]
第五章：氛围编程 (Evolution/Delivery)
Prompt:A glowing, finished artifact cube floating inside a digital cloud mesh network. Sending signal lines outwards to the global network. Concept of cloud deployment and software delivery. [Insert Universal Style Suffix]
```

然后直接去生图，拿到下面这些图。

拿到图之后，全部下载下来然后放到`public`文件夹下，在`public`文件夹下新增`images`文件夹，然后把生成的图片放进去。

![](img/68610eb18287ad658c6839718632638d.png)

然后让`claudecode`替换掉。

![](img/cbd6994a0feed1a5b5346dcc2095b928.png)

最后再放一个我的个人照片。

![](img/bf0d632375606947ac4095e137cc6787.png)

告诉`claudecode`。

![](img/adb85bf7a0285c2bf15dcd6bf0ad550b.png)

至此，我们的landing page就写完了，接下来进行一些走查。

## 检查组件中被写死颜色和字体的 Tailwind 代码，让 AI 修正

因为我们等下可能要换衣服，我们现在让AI去修正我们的代码，看一下是否有被写死颜色和字体的`Tailwind CSS`。

先`/clear`清除掉上下文，因为这是一个新任务。

![](img/29d764da39e3665886b9d734.png)

然后让AI去检查组件修正可能存在写死颜色的代码。

![](img/7cfa49a5efac7c17cd721cf66c16c602.png)

然后一通修改之后就结束了。

![](img/48ef503c1c8d37db99117afa553485f2.png)

这样，当我们选择修改配色直接在`global.css`里面进行代码替换的时候，就会直接作用于整个网站而不会遗漏个别组件。所以这一步必须得做。

记住它现在的样子，等下我们就要把它改掉了。

![](img/00130d137e2b26ca057a66964b3d63d3.png)

## 穿衣服：自己配一个好看的颜色和字体

接下来，我们就可以根据我们的喜好给这个网站配上喜欢的颜色。我们使用`tweakcn`这个开源配色库。

先登录`tweakcn`官网：`https://tweakcn.com/`

![](img/a03531b39c904f382b0bae02c302191f.png)

然后点击官网左下角的 Start Customizing。

进来了之后，在`dashboard`的左上角去选你喜欢的主题。

![](img/da51763093ec3900f77a3fb08bc946e5.png)

比如，我想要一个小报童的主题。看起来下面这个Mocha Mousse蛮好。

![](img/91178ca4093069b663d18dc2b99a450f.png)

但是颜色好像并不是特别好。这个是小报童的朱砂红，上面这个更偏棕色。那我们就来改一下它的颜色。

![](img/ab8756603bcab29f62b9dc1ea7b20007.png)

把`Primary`的颜色换成小报童的。

![](img/84b2fa051b9ed49535b5763163dceb99.png)

一通调整之后，预览长这样。

![](img/a79f85ce34469d0ebac039c5bb03afe7.png)

有点小报童的感觉了，这个时候我们就可以导出`css`了。点击右上角的`code`。

弹出了Theme Code的弹窗。

![](img/363b96b0fe6ad277bcd31804be86c897.png)

再检查一下我们`globals.css`的颜色编码方式。可以看到我们的编码样式是`oklch`。

`globals.css`的文件位置在`app`文件夹下面。

再看一下我们的`Tailwind`版本。我这里是`V4`。

![](img/15fe975ddabb47b0010247ad026922bf.png)

当然如果你看不来，也可以直接问AI。

![](img/50ae2b8d1058f353af9d8023f367d1f9.png)

过了一会，也给了我们正确的结果。

![](img/7defed35c1e39c430d8c78bb13177127.png)

选择`Tailwind V4`和`oklch`，点击`Copy`。

![](img/9611056f97aa07eb56a1db6c96e63750.png)

然后粘贴给`claudecode`，让它帮我们更新颜色和主题。这里Pasted text是我们刚刚copy的完整`css`参数。

![](img/2763670c09946ae3a452c48d2bb038a2.png)

然后`claudecode`一顿操作之后改好了。长下面这样。

![](img/9df1da2fb95b4f6256d993fd999f8160.png)

看起来像那么回事了，哈哈。

![](img/6debe12cdda38d786ec6608723366f85.png)

## 做成交：把 Landing Page 上的 CTA 按钮指向到你的成交页

还记得Landing Page是干嘛的吗？

> 它有两个核心特征：
>
> +   **单一入口：** 它是用户从外链（🧣、👸号、X、reddit 等等）进到网站的第一个页面；
> +   **单一目标：** 整个页面的设计、文案、按钮，都在大声地对用户说同一件事——“请做这个动作”。这个动作可能是“输入邮箱”、“购买课程”、“下载软件”或者“加入等待名单”。

注意**“请做这个动作”**的含金量，你做了这么多，实际上就是为了用户去点你要的那个按钮，从始自终都是这个目的。

在我这个案例里，我希望用户买我的小报童专栏，于是我的选择就是用户点击解锁阅读权限的按钮后跳转到我的小报童专栏页。

进入小报童，点击复制链接。

![](img/cbe7999f9e1bcd8574ea7eeafa715dbf.png)

复制出来了，直接发给`claudecode`。

`https://xiaobot.net/p/cnzhihao-001?refer=7db16d14-e55f-471e-accc-909453053e1f`

![](img/83bbadf80b14b18ac67666a4e5f7e702.png)

然后就做好了，现在在网站上点击CTA的按钮就会直接跳转到小报童了。

至此，我们的landing page就全部做完了，发布到`GitHub`上去。先输入一个`commit`的说明，比如我这里输入的是“completed”，然后点击`commit`。

`commit`完了点击 Sync Changes。

![](img/2196be35356e4c374acc697c26340177.png)

仓库就更新好了，接着我们就可以发布了。

## 怎么发布自己的 Landing Page？

全部做完之后，我们就可以发布自己的网站了。首先进入`Vercel`，如果你没有注册`Vercel`的话，请参考开始前的准备工作部分注册并登录`Vercel`，我们就直接从`Vercel`的`dashboard`开始了。

### Vercel 部署

在`Vercel`的`dashboard`上点击 add new。

选择Project。

![](img/92c9d733a87873aa2ad3858d3e8462de.png)

找到我们刚才上传好的仓库，点击import。

![](img/34fd56535664d51d9ba3c96d4a9885d0.png)

然后在弹窗里看一下内容。root directory是根目录，没问题。

![](img/1ed864a6031e5c683f29fe416dc436d7.png)

Build and Output Settings这里要自己填一下，因为我们用的是`pnpm`包管理器，所以要填一下，不能用默认设置。这里我们没有设置输出格式，使用默认的`.next`就行。剩下的按下图填。

![](img/f9334037c68dce1fe9b9c518ade07e71.png)

完事之后点一下下面的Deploy就行了。

![](img/5dd2f2c1d161f1ede7270a128bdddffc.png)

过了一会就好了，到`dashboard`上去看看。

![](img/254fb5f75a56f65dee4ba4661bcb3787.png)

`https://aiworkbook.vercel.app/` 有了。

![](img/90a7040584f532e7c483bd2074322966.png)

但是`vercel.app`的域名是被BAN的，怎么办呢？我们可以选择自己买一个域名。

### 买一个域名

这里就不专门讲买域名的事情了，我推荐使用`spaceship`进行购买，买域名就和逛淘宝一样简单，就输入你想要的域名，然后search，然后add to cart，然后buy。

官网是：`spaceship.com`

![](img/694d3efc2ee070d7d91635b2f1296d70.png)

### 把你的域名配置到 Vercel 上

点击这里的domains。

![](img/da65e32320980572c7190727518137a5.png)

点击add domain。

![](img/1a95596e69eaf53c2b4377d1c11bc35e.png)

把你刚买的域名输进去。

这里我输入了一个二级域名，然后它报错了。报错是很正常的，因为我没有配置DNS。也别管DNS是什么了，`Vercel`能帮我们解决。

![](img/a0e1eb60949c172c202bc3a5115f6839.png)

点击这里的Learn More。

![](img/8ca385a57111c387e0c22fc36ba925e0.png)

在弹出的下拉里选择Configure Automatically。

![](img/d4b107bc5dbe3b40dd452c8060871c7b.png)

我这个域名是挂载在`Cloudflare`上的，所以它就跳转到`Cloudflare`了，如果你是在`spaceship`上管理的DNS，它就会自己跳到`spaceship`上。

登录之后自动就到授权页了，点击“授权”。

![](img/a5841d99b34246baab550ee3988dd8db.png)

然后就自己跳回来了，`Vercel`会自动帮我们加SSL证书，做DNS优化，balabala…这些专有名词就别看了，等之后学会了再回来看吧，现在就只管上站就行了。

![](img/b7f81d77cbd10351224f58dca2dfdc03.png)

过了一会，好了。显示valid configuration。

![](img/01a3a835314f9a84309f34d459754193.png)

然后我们通过`book.enchenggc.org` 就可以直接进入这个landing page了。

![](img/40dbef0d961fc76204ff5cb00060a084.png)

是不是很好玩？而且不用🪜就能访问喔！

**快去玩吧！期待你的作业！**
