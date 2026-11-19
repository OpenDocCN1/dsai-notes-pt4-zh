
# Clawdbot 一夜爆红，腾讯云教你一键秒级部署 7×24h 核动力“牛马”！

> 来源：https://yunyinghui.feishu.cn/docx/Z0qrd8fEoorIgSx03rNcBuzknSe
>
> 来源👸号：腾讯云开发者
>
> 发布时间：2026-01-27 18:19:08
>
> 收录开源AI知识库：[万象 AI 实验室](https://yunyinghui.feishu.cn/wiki/WhelwZnxQi55mpkOzatc3Xran5b?from=from_copylink)

![](img/7406019b9e5459e2512f230a9a6c9650.png)

一夜之间，从海外到国内，从 X、Reddit 到中文技术圈，Clawdbot 的爆红成了 AI 时代又一个现象级的产品。

整个硅谷都为它疯魔，连带着 Mac mini 的紧俏度也像内存一样水涨船高，成为了电子茅台般的产物。

但相信还有很多同学不清楚，Clawdbot 究竟是什么？能做什么事？具体应该怎么部署？才能最大化发挥它的作用，实现一个 7×24h 永不下班的“核动力牛马”？

这篇教程，你可要仔细码住了！

Clawdbot 是一个 AGI 雏形下的 AI 智能体，不仅会思考，拥有永久记忆，更能通过 iMessage、WhatsApp 实时聊天。它的作者 Peter 在介绍中反复强调：Clawdbot 的初心是一个“生活助理”，用 WhatsApp 发消息，转到 Claude Code，再把结果发回来……

![](img/40e985e2a725bdcb92bd709d758eae16.png)

究其引爆技术圈的原因，可能不仅在于它的代码 100%由 AI 完成，更可能是它超出人们预期的“人机协作方式”：

Clawdbot 运行在使用者自己的环境中（比如本地电脑、个人服务器等等），并且交互的方式直接回归到了用户熟悉的聊天软件之中，就像是与一位同事或朋友交谈那样。

此外它同时拥有完整的操作权限与超长记忆，这意味着它是一个专属于个人的 AI Agent，能力范围被无限拓宽的同时，也无需担心数据落入少数“大公司”之手。

由于 Clawdbot 目前仅支持海外市场的一些社交软件，身处国内的我们想要体验可能需要略费一些周章，接下来的文章将带大家完整跑一遍部署流程。

## 01 Clawdbot 适合的运行环境

伴随着 Clawdbot 的爆火，Mac Mini 也一举跃升为“理财产品”，最有震撼力的消息就是社区内有非常多的先行者声称购入了大量 Mac Mini 来运行 Clawdbot。

不过随着更多人了解到这个项目，也出现了另一种声音：超高权限下，Clawdbot 更适合运行在一个和主力电脑相隔离的环境下，或许是你的一个旧电脑（macOS，目前支持的程度最好），又或许是一台云服务器（操作系统为 Linux，同样支持，并且环境也与本地强隔离）。

社区交流下的一个共识是，建议不要把 Clawdbot 部署在主力电脑中，否则可能对本地数据的安全造成影响。

一台旧的 macOS 电脑也好，亦或是效仿社区大牛紧急购入 Mac Mini 也好（涨价了！涨价了！涨价了！），都需要花费不少时间和资金成本，更别说长时间运行还得产生一些电费和更高的设备故障概率。

而使用一台云服务器目前看来，是一个更加迅速（几分钟内即可上手）、成本更加友好（几十块钱即可开始）的选择，并且云服务器天然支持 7*24 小时运行，与 Clawdbot 的定位可谓是天作之合。

就是你了，出来吧——轻量应用服务器 Lighthouse！

> 轻量应用服务器 Lighthouse 是腾讯云推出的一款面向轻量级应用场景的云服务器产品，无需开发者理解复杂的云计算技术概念，提供了高性价比的服务器套餐，同时还支持开发者基于预置的操作系统及运行组件，快速部署开源应用。

问题来了，如何在 Lighthouse 上快速部署并配置 Clawdbot？

目前 Lighthouse 已经同步上线了 Clawdbot 应用模板，预置了 Clawdbot 运行所需的环境，无需手动安装，接下来就跟随本篇教程的步骤开启吧！

## 02 基于应用模板一键安装 Clawdbot

目前 Lighthouse 支持通过两种方式使用应用模板来一键安装 Clawdbot：选购一台新实例或重装一台吃灰的实例，同时选择可以选择不同的腾讯云站点购买海外服务器（如硅谷、新加坡节点）进行部署。

+ 腾讯云国内站：适合国内开发者，可直接购买海外地域的服务器资源。
+ 腾讯云国际站：如果您的公司主体、业务及目标用户均位于海外，可通过国际站进行注册和购买。右滑动阅读国际站版教程>>>

### 2.1 方式一：选购一台 Lighthouse 实例

前往 Lighthouse 购买页，配置项按照下图直接选择即可，或者点击文章最下方“阅读原文 ” 一键直达。

![](img/731970908d8fd27dac605307095d4f3a.png)

扫码直达购买页面

+ 应用创建方式：应用模板 > AI 智能体 > Clawdbot
+ 地域：优先选择海外地域，如硅谷、弗吉尼亚、新加坡等
+ 套餐
+ 套餐类型：锐驰型（推荐）、入门型、通用型
+ 套餐配置：2C2GB 或以上均可
+ 服务器名称、登录方式等按需配置即可。

![](img/de3613118c205f309e017fe95d5f55cc.png)

配置完成后，点击页面右下角“立即购买”，按照页面引导完成支付即可。

![](img/c99725a8f48b68bc62745139bdd38d07.png)

### 2.2 方式二：重装一台“吃灰机”

在 Lighthouse 控制台内，找到自己账号下长期吃灰的那台 Lighthouse 实例，在实例页面内点击“…”或“更多”按钮，找到并点击重装系统。

注意：Clawdbot 应用模板仅上架至中国香港、其他海外地域。因此未上架地域的重装系统页无法看到 Clawdbot 的选项。

![](img/3c732f3487ea5dc863ebb5f5c26f3294.png)

在重装系统的页面，选择应用模板 > AI 智能体 > Clawdbot，登录凭证可以先直接选择重装后设置。

备份选项处，建议优先选择备份后重装，以免原实例内还存放部分重要数据。

![](img/ba975db9cc277cb0369811775354ad51.png)

点击页面内的确认按钮，待重装完成后，继续进行后续步骤即可～

### 2.3 登入服务器完成后续步骤

前往 Lighthouse 控制台即可查看刚刚选购或重装完成的 Clawdbot 实例，点击页面内的“登录按钮”：

![](img/a73f873b50bdda2c61f2f5dcbf815f83.png)

点击登录后，在登录工具（OrcaTerm）的页面内，选择免密连接，点击登录即可：

![](img/0aa7dc4715ec425937428c40a23b8e8b.png)

登录成功的界面：

![](img/925fc711b052a7c0c4765c64231a3fb4.png)

## 03 后续配置

Clawdbot 与常见的应用模板不同，官方提供了若干需用户自行手动配置的步骤，在首次登入服务器后，输入并回车运行如下命令开始配置：

```
clawdbot onboard
```

运行 `clawdbot onboard` 后，需要通过键盘来完成后续配置动作，关键操作：方向键控制选项，回车表示选择并确认。

### 3.1 同意免责声明

运行上面的命令后，将会出现一个问题：是否知晓风险，选择 Yes 就行。

![](img/7fae6758fba017ea4b961574697a28cd.png)

“我明白它功能强大，但也存在固有风险。是否继续？”

### 3.2 配置模式选择：快速入门

接下来需要选择 `Onboarding` 的模式，我们选择 `QuickStart`。

![](img/4a2f8cc063c7ae8cc6be9c82e1bfadf9.png)

### 3.3 模型配置

紧接着的一步是选择 `Model/auth` 提供商，如果想省时省心的话优先推荐选择国内的厂商（MiniMax、Qwen、Moonshot AI、Z.AI/GLM），本文选择 Moonshot AI（月之暗面/Kimi，点击直达 `API Key` 管理页）进行演示，`Moonshot AI auth method` 选择 `Moonshot AI API key` 即可，然后填入自己的 `API Key`（点此获取 `https://platform.moonshot.cn/console/api-keys` ），再粘贴自己的 `Moonshot API Key`，默认模型选择 `Keep current`。

![](img/bff7337d56b6feda64135a35178d3be8.png)

PS：挖个坑，嘉钰后续研究研究能不能配置中转服务以实现海外模型接入，这里先按下不表。

### 3.4 聊天软件配置

这一步将决定后续在什么界面下与 Clawdbot 进行交互，此处官方支持的聊天软件几乎是我们日常不会使用的，本教程我们选择以 Discord 作为演示。更多聊天软件的配置参见官方指引：Channels（ `https://docs.clawd.bot/channels` ）。

在配置流程中选择 `Discord (Bot API)` ，选择完成后会提示如何获取 `Bot token` （挺贴心的）。

![](img/eb88168a0efdaa62d9ec60185eb9fb33.png)

1、前往 `Discord Developer Portal` > `Application` > `New Application`

![](img/74703ab36e5c68e924cc7829d4284df9.png)

2、跟随页面指引创建完 `Application` 后，在左侧导航找到并进入 `Bot` > `Add Bot` > `Reset Token` > `copy token`，在页面内复制 `Bot token` 后，返回并输入：

![](img/03af753593008389b9eda5003b7e203e.png)

3、在页面内打开 `Message Content Intent` 的选项并保存：

![](img/3bf78936e9e53fa7fd415b61d075df6d.png)

4、进入 `OAuth2` 选项，往下滑动页面：

1. 在 `OAuth2 URL Generator` 中勾选 `bot`
1. 在 `Bot Permissions` 中勾选 `Send Messages` 和 `Read Message History`

![](img/1ed9cf6fbaf6dbb55cd52a5583f599f1.png)

5、把页面滚动至底部，复制并在浏览器中打开生成好的链接，选择邀请进入的 `server` 后，点击 `Continue` 后进行授权，即可邀请 bot 加入：

![](img/7cca890c0a48ad7165bdc2ff3e65e86f.png)

![](img/798a3060ff80d8d333be6c22614ee8ea.png)

添加完成的效果如下图所示：

![](img/fa2c88a66d1377cdcdf3cfb8de657a81.png)

之后返回服务器内的配置流程中，其余的一些配置如下图所示来选择即可：

![](img/672a97f81415d4c11a4dad6b9a449672.png)

![](img/ec9ae00dc806c147dd90584185c4172c.png)

注意，`Enable hooks` 的选项选择 `session-memory`：

![](img/058289bd34cfde3e9c4f825edc908294.png)

接下来，我们在服务器命令行这里粘贴并运行如下命令，启动 `Gateway`：

```
clawdbot gateway --port 18789 --verbose
```

启动后的效果如图：

![](img/4e006bd1ff2104d0a169b6cd85df5293.png)

启动成功后，我们返回 Discord，与 bot 进行对话后拿到配对码：

切记这里一定需要和 bot 进行私聊！！！

紧接着返回服务器命令行，按下 `Ctrl+C`（Windows）或者 `Command+C`（MacOS）终止 `Gateway`，然后粘贴并运行如下命令进行配对：

把 `<code>` 替换为上图中的“Pairing code”后面的内容。

```
clawdbot pairing approve discord <code>
```

执行完成后再次执行如下命令运行 `Gateway`，然后回到 Discord 与 bot 进行对话，如果正常回复则说明部署成功：

```
clawdbot gateway --port 18789 --verbose
```

-END-

<<< Swipe for Chinese

![](img/a0bcb0805c63b129fd784c13c8c7f2c9.png)

On January 26, a product called Clawdbot suddenly went viral across both international and Chinese tech communities. From X and Reddit to Chinese developer forums, it quickly became one of the most discussed AI projects.

![](img/6919eecd7be0e3a403337f047558e18e.png)

Its creator, Peter, repeatedly emphasized that Clawdbot was originally designed as a “life assistant”:

you send a message via WhatsApp, the request is forwarded to Claude Code, and the result is sent back to you.

Clawdbot is not the first AI Agent capable of performing real tasks. However, only two or three weeks after its release on Claude Cowork, it unexpectedly reignited excitement in the AI community. The reason behind this renewed hype may not lie solely in the fact that its code is 100% AI-generated, but more importantly in its unexpected human–AI collaboration model.

Clawdbot runs inside the user’s own environment — such as a personal computer or private server — and interacts through familiar chat applications. Communicating with it feels like talking to a colleague or a friend. In addition, it has full system-level permissions and long-term memory, which effectively turns it into a truly personal AI Agent. This dramatically expands its potential capabilities while also avoiding the risk of sensitive data being centralized in large corporations.

## Where should Clawdbot run?

With Clawdbot’s rapid popularity, Mac mini has almost become a “financial product” in the community. Many early adopters reported buying multiple Mac minis specifically to run Clawdbot.

At the same time, another perspective emerged:

because Clawdbot operates with very high privileges, it is better suited to an environment isolated from one’s primary computer — such as an old Mac (macOS currently has the best support), or a cloud server running Linux, which is also fully supported.

Note: Clawdbot’s Windows support is still limited, so Windows is not recommended for early experimentation.

## Why Lighthouse?



# Deploying Clawdbot on Tencent Cloud Lighthouse

Whether you use an old Mac or rush to buy a Mac mini like many community pioneers, both options require significant time and financial investment. Long-term operation also incurs electricity costs.

In contrast, a `cloud server` offers a faster and more cost-effective solution:

you can get started within minutes, with entry-level costs of only a few dollars per month. Cloud servers are naturally suited for 24/7 operation, making them a perfect match for Clawdbot’s always-on assistant design.

This is where Tencent Cloud Lighthouse comes in.

Lighthouse is a lightweight cloud server product designed for lightweight application scenarios. It eliminates the need for developers to understand complex cloud concepts, provides cost-effective server packages, and supports rapid deployment of open-source applications using preconfigured operating systems and runtime environments.

So how can Clawdbot be deployed quickly on Lighthouse?

Lighthouse now offers an official Clawdbot application template, which comes with all required runtime dependencies pre-installed — no manual setup needed. In the following steps, we will walk you through the entire process of deploying and configuring Clawdbot on Lighthouse, step by step.

## 1. Create a Lighthouse Server

One click:

👉 https://buy.tencentcloud.com/lighthouse?blueprintType=APP_OS&blueprintOfficialId=lhbp-8hq35xoy&regionId=15&zone=na-siliconvalley-1&bundleId=bundle_rs_nmc_lin_med2_01&loginSet=AUTO&rule=true&from=lh-console

Special Offers:

👉https://www.tencentcloud.com/act/pro/lighthouse?from_qcintl=112010201

### 2. Create a new instance.

![](img/eed131132bab4c6a0bad08c878f9f132.png)

![](img/a056c0dba3950615b9ac8a9ef35cb400.png)

### 3. Select the following options:

Please choose according to your actual needs. The following configuration is recommended.

+ Operating system: Clawdbot
+ Region: Singapore or Tokyo
+ Instance size: 2 vCPU, 2GB RAM
+ Billing period: 1 month

### 4. Save the following information:

+ Public IP address
+ Login password (or reset it in the console)

## 2. Log in to the Server

Open a terminal on your local computer and run:

```
ssh ubuntu@YOUR_PUBLIC_IP
```

![](img/acc91c56a33da72868340fc8383d5b78.png)

When prompted:

```
Are you sure you want to continue connecting?
```

Type:

```
yes
```

Then enter your password (input will not be visible — this is normal).

Note: On Ubuntu images, the default login user is `ubuntu`, not `root`.

![](img/7273021e97a02af42f215b09d01df63b.png)

## 3. Install Clawbot

Run:

```
clawdbot onboard
```

![](img/f923af43643cb12defb7931efeb62e39.png)

Wait approximately 2–3 minutes for installation to complete.

## 4. Configuration Choices (Recommended Settings)

During installation, Clawbot will present several interactive options.

Below are the official recommended selections for first-time users:

### (1) Risk Acknowledgement

Select: Yes

Rationale:

Clawbot is capable of executing commands and accessing system resources.

This confirmation ensures the user understands its powerful capabilities before proceeding.

![](img/73c531d536fb0251267316c5828c3160.png)

### (2) Onboarding Mode

Select: QuickStart

Rationale:

QuickStart uses official defaults and minimizes configuration complexity.

It is optimized for first-time deployment and reduces error probability.

![](img/bdcafe3b47765d08dc656d4c566a4047.png)

### (3) Model/Auth Provider

#### (3.a) Gemini Authentication Method

Select: Google

Rationale:

Google (Gemini) is chosen because it provides an official and stable API, good model quality for general reasoning and agent tasks, and straightforward integration using an API key without complex custom configuration.

![](img/fd31a496b82b3c03fa08b2a7686922c2.png)

![](img/605ac649e7ef17442bd6d4dfe7d07667.png)

Recommended model:

`google/gemini-3-pro-preview`

![](img/94ebbae6b5a1bbefed042350d35c6c94.png)

Open the following website:

👉 https://aistudio.google.com

Steps:

1. Sign in with your Google account
2. Click Get API key (top right corner)
3. Choose Create API key
4. Copy the generated API key

#### (3.b) Qwen Authentication Method

Select: Qwen OAuth

Rationale:

OAuth authentication avoids manual endpoint configuration and simplifies setup by using the official authorization flow.

![](img/284266a4ea73615ecd8cadcda3bad1c3.png)

![](img/529ac36d17a378a0c852f85c46340dbe.png)

### (4) Communication Channel

+ If you use Telegram or WhatsApp: select the corresponding platform
+ Otherwise: select Skip for now

![](img/e48274f950415de433232b00083af752.png)

![](img/74981e81360532d6e86c53661a793a9d.png)

![](img/ced104b07db386ff205481193e3a8eb9.png)

### (5) Configure Skills

Select: No

Rationale:

Skills enable system-level automation (file access, browser control, shell execution).

For initial deployment, limiting permissions improves stability and security.

![](img/c5dce7764b221a7f03741eb850573c61.png)

### (6) Enable Hooks

Select only: `session-memory`

Rationale:

+ Enables conversation memory
+ Does not execute commands
+ Minimal security risk
+ Required to proceed past this step

Do not select:

+ `boot-md` (auto-run scripts on startup)
+ `command-logger` (command tracking and logging)

![](img/212e27f1bb3410e13209aa50509b4ad3.png)

### (7) Hatch Mode

Select: Hatch in TUI

Rationale:

+ TUI (Terminal UI) launches Clawbot directly in the terminal: No additional ports required
+ No web interface configuration
+ Immediate verification of successful startup

![](img/456110d96a02c031b1530cc4a398ce21.png)

## 5. Verify Successful Deployment

If you see:

```
Wake up，my firend!
```

This confirms:

+ Clawbot is running
+ Model connection is active
+ Deployment is successful 🎉

![](img/6c88dcccb35f89fbc3b59d3a3aeb73e7.png)

## 6. First Interaction

Example prompt:

```
You are Clawbot. Your main responsibility isto help mewith research, writing, and analysis tasks.
```

![](img/854db0e76a6ec2e8b24138810a1add9f.png)

## 7. Functional Test

Send:

```
Explain what an AI agent isin simple terms.
```

If Clawbot responds normally, the system is fully operational.

![](img/6ba267b684aede44770cfe6cc755f6a8.png)

## 8. Security Recommendation

Clawbot is capable of:

+ Executing shell commands
+ Reading and writing files
+ Running automated tasks

Recommended usage:

+ Run only on VPS or cloud instances
+ Avoid personal machines
+ Do not grant access to banking, wallet, or critical accounts
+ Treat as an AI automation sandbox

## Summary

You have successfully:

+ Deployed Clawbot on Tencent Cloud Lighthouse
+ Configured a stable baseline environment
+ Launched an AI agent capable of continuous operation

-END-
