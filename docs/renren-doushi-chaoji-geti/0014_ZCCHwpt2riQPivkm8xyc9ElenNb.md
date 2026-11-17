# 从0到1手把手教你用搭建OpenClaw私人助理(超详细)

> 来源：[https://xuqiwei1986.feishu.cn/docx/T9XudUiLdoEn37xLYHtce7aMnwb](https://xuqiwei1986.feishu.cn/docx/T9XudUiLdoEn37xLYHtce7aMnwb)

| **文档版本** | **更新描述** | 作者 | **更新日期** |
| --- | --- | --- | --- |
| V1.0.0 | 新建文档 | 科学羊🐑 | 2026-02-09 |
| V1.0.1 | 新增飞书+Telegram配置 | 科学羊🐑 | 2026-03-14 |
| V1.0.2 | 新增Skill技能+应用 | 科学羊🐑 | 2026-03-15 |
| V1.0.3 | 新增应用 | 科学羊🐑 | 2026-03-18 |
| V1.0.4 | 新增🛰️接入的方法 | 科学羊🐑 | 2026-03-23 |

![](img/e88e044cf1331c302eed4dfef587ec06.png)

大家好，我是科学羊，

+   （10W+）爆文👸号专栏日更作者；

+   时间管理工具，「时迹-TimeFriend」的作者；

+   知名报社签约编辑；

+   海外内容创业实践者；

+   YT海外BNG游戏视频创作者；

+   赚钱垂直👸号航海教练+传术师；

+   赚钱ClaudeCode航海教练；

+   AI产品开发变现实践者；

本期，我想给大家在分享一期关于最近非常火的Openclaw的搭建方法和我自己踩过的一些坑。话不多说，直接开始！

# 一、为什么OpenClaw这么火？

![](img/6b72e1d1fbb8ff9af1c4efdc2086d33b.png)

大家都知道最近OpenClaw非常火，是因为它不只是一个用来聊天的AI，而是一个能“帮你做事情”的系统。简单理解就是，以前你用AI，需要自己一步一步操作，比如先让AI写内容，再自己去别的工具做图、整理资料，而OpenClaw的思路是把这些流程交给AI，它可以接收一个任务，然后自己调用不同工具，把步骤自动完成。很多人觉得它厉害，是因为现在AI的发展方向已经从“回答问题”转向“执行任务”，也就是所谓的AI代理（Agent）。再加上它可以直接在Discord、或者WhatsApp、以及企业🛰️、飞书等地方使用。

比如Discord本身就像一个可扩展的工作空间，适合接入机器人和自动化，所以组合起来就变成一个可以随时下指令、自动执行的AI工作台，这也是它突然受到关注的主要原因。

我最近在X里也浏览了很多大家对openclaw的观点和评论。



![](img/4cd9436dfa6b3d300cdf4216add55d04.png)

出于好奇，我是真的想试试。真的不是为了炫耀或者跟风，就是单纯热爱和好奇心，哪怕这个玩意真的对很多人来说可能只是一个泡沫....，其实那也无所谓。

就像数字生命卡兹克所说：好奇心这个东西，可能比聪明更重要

![](img/d2873fe06a27ab57827d6b77ed320d3f.png)

那么，其实怎么满足自己呢？

首先，我看了很多帖子，现在OpenClaw有很多支持你部署的方式，比如Macmini、树莓派（其本质就是linux环境）、云端服务器、window虚拟机、甚至ESP32和华强北改装的设备（本质我猜测就是个ESP32微处理器）等等。

但是，综合来讲，我研究了其生态和本质，最优的选择就是——Macmini！

所以，二话不说，先买一台回来，管他多少钱，需要就要投资直接JD买。

你可能会说，我不是有macbook吗，为什么还要单独买。

因为我就是拿来开权限给他做测试，随便搞。我笔记本有重要资产，玩不起啊。

![](img/fac2a6b6735791425116e5164500407a.png)

# 二、如何搭建和安装OpenClaw？

> 我们重点讲解Mac的安装，其实端基本的方法都一样！

> 另外，本次讲解，主要是discord和openclaw来结合，其他的第三方工具其实原理类似

**注意注意：**前提条件，🪜是必须的，大家自行解决。我这台mac我是直接搭建了家庭住宅静态IP，如果大家需要学习如何搭建请翻看我以往精华帖（TK那篇）即可。（机场也没问题）

## 2.1 Discord环境搭建（客户端）🛰️

### 第一步：客户端（飞书、电报、🛰️、注册discord，1对多）

网址：https://discord.com/

![](img/511a50779e3fcd24a15a366f8b7dcabc.png)

![](img/60903d5dab051284392b15807d8fd616.png)

![](img/d3bf50f3c6f28e08bb0575f313b12ca0.png)

![](img/4c7d9fe9c6064bf2246bc15d6036ec1b.png)

注册之后新建一个属于自己服务器，然后就可以直接进入自己的主页（这方面有很多类似的教程，大家都可以搜）

### 第二步：配置开发环境

https://discord.com/developers/applications

#### 2.1.1 新建一个应用

![](img/b03e30b47c90cd43cd54cd8aca3879b7.png)

名字随便起，我写的是openclaw。

#### 2.1.2 配置bot

勾选截图里的内容，并保存

![](img/d52161a5a8648550970309ee6dda91c2.png)

#### 2.1.3 配置OAuth2

勾选如图

![](img/0d0c35d656604130826dca8d9e9ba99e.png)

往下翻，勾选如图：

![](img/9337e72c86e1cd28647108138b2703d4.png)

#### 2.1.4 安装这个应用

复制上图红色标记的链接，然后新开一窗口打开，长下面这样：

![](img/ef1633e0ae0914b7052590a220b6cbfe.png)

默认选择权限即可，就可以安装成功

![](img/a1024095c7270868f9eff23c484f8909.png)

这个时候你就可以打开你的应用，如下图：

![](img/e38ecdd957706bd6f23d45c5f62df366.png)

**当你随便输入某个指令后，**你会发现它不回复，或者给你回复一串代码「上图所示」这个很重要！先放着，我们后面用！

以上discord基本ok，接下来我们安装OpenClaw。

## 2.2 OpenClaw环境搭建（服务器）

### 2.2.1 第一步：打开终端输入官方给的链接

https://openclaw.ai/

![](img/93c7354bd844386faefa201d4629333e.png)

输入：

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

注意，如果输入这句指令报错可能是你没有管理员权限，可以增加sudo

```
sudo curl -fsSL https://openclaw.ai/install.sh | bash
```

接下来，你不需要做任何事，等待即可，可以喝杯茶悠闲等待⌛️。

### 2.2.2 第二步：基础配置

安装完成之后，会显示下图的图片就算是安装好了。

![](img/94ce56502b9192f96b463ba54f0d8a40.png)

然后你按照我图片配置即可：

![](img/6b9d807fa8d2b7599653e36cac274fbb.png)

这是一个警告提示，yes即可。

![](img/8bc63ea03f57e070778ff8c5489b9f83.png)

选择QuickStart

![](img/f8989edeac2bc9c88260b1bf2231a08f.png)

我用的是kimi K2，这里大家按照自己的情况来选即可，但是一定要提前准备好Key。

比如kimi 的key需要在：https://platform.moonshot.cn/console/api-keys

![](img/00c4bb283571c07846020c84f337c728.png)

为什么要选择kimi？有人给了答案，

![](img/9dfd51e494c6a6a6aa4c3fe430f7a69e.png)

接下来继续

![](img/1622b576e8f3019ac7703aa7f3768581.png)

选择kimi，然后输入你的key

![](img/8c05ee5a60ec982b6cb23509e4bdd30f.png)

继续

![](img/314b8c12230ab2d36b32da658a3af293.png)

选择Keep...，然后继续，

接下来，您会看到选择聊天工具的选项。您可**以先选择 [Skip for now]（注意注意，这里有坑**，一定先选择skip，我们后面再配置，我就当是卡在这里了，先跳过，选了也还是白选）

![](img/3021ccfe8dedd9995a4fb71df4541542.png)

其他配置，如 Gateway Port 设置，可以保持默认值 18789。对于 Skills 和包管理器，选择 npm 或其他选项，其余所有选项都可以选择 Yes。

![](img/c1067a41d696f1b39f551d75a6e5b426.png)

**对****于 skills，您**可以跳过此步骤，或使用空格键选择您需要的技能，其它别选，直接跳过！

参考我这个

![](img/a4d0f5efa00f2eb4fda87809cba0028a.png)

然后就是界面选择

![](img/50b92475a896cd121700ef667bec5641.png)

然后会自动弹出一个web-UI，如下图：

![](img/a4e1912c3d85484f69dad312d06bc30d.png)

你只需要在chat里面测试，能否聊天，只要有回复，说明OpenClaw已经安装成功了！

注意：我在安装的时候这个地方失败了好多次，基本估计是配置问题。我重复了几次，甚至重新新建一个kimi的key就ok了，大家在过程中如果失败不要气馁，可能就是一些细节没注意到才导致的呢。

## 2.3 discord和openclaw建立关系

你可以重新开一个终端，输入

```
openclaw config
```

这时候就会进入配置页面：

![](img/f6704e48c3c3891c1a1d1fd446fa5c50.png)

注意接下来的选择：

![](img/1b00c09bf8df6efd09bd3b2ee75e37cc.png)

选择平台：

![](img/b2c222a0bb555b06b5f1784fdeb06737.png)

选择之后需要粘贴token

![](img/a65dda4a2c7ce0056506453f1e51c164.png)

那么，这个token从哪里来？

这个时候你需要回到：https://discord.com/developers/applications

![](img/ea8dc5e7d9d07d103b29a4a6b5e4ab2c.png)

在这里复制token，然后输入到终端。

![](img/acc27ca4f34a539d5c639fabe8f40ab0.png)

然后最后几步按照上图配置即可，以上完成就OK。

最最最后，把你刚才和bot对话他给你发的指令复制下来发给终端，

![](img/7ddaa012091a5c3c0f3116a4a1757f40.png)

如**图，你需要发送：openclaw pairing** approve discord 你的pairing code（比如：openclaw pairing approve discord JCWXRTTQ）

![](img/d1a82b2d7d85a0cd11a8248b9ad8f843.png)

以上配置全部完成～～～

## 2.4 交互测试

第一种方式我们说过了，其实你不用配置discord和WhatsAPP都可以用，但是网页端没意思，是吧。

![](img/5a29eab8023acc2fc87f65c6c818e0ce.png)

第二种方式就是discord来交互，如下图：搞定！

![](img/837e1bdd8e2935b17f8a52896c481e52.png)

# 三、多个频道如何配置

刚才我们已经配置了关于 Discord，那么接下来我们再配置一下飞书 + telegram的方式。

三者之间其实不会冲突，大家放心配置。主要考虑到大家可能用的手机或者平台都不一样。

![](img/c2644e39acf863b966174e999da81da8.png)

## 3.1 telegram

首先，Telegram 大家需要注册，注册的方式可以直接用国内的手机号进行注册。

### 3.1.1 设置步骤

![](img/d4a7bc2754d186a40e68e2e3feeeae74.png)

![](img/4113179f210735c008617d55d7ed9bcc.png)

**1、找 BotFather**

+   打开 Telegram，搜索 @BotFather

+   发送 /newbot 创建新机器人

+   按提示命名（比如 MonicaBot）并设置用户名（必须唯一，以 bot 结尾，如 masir_helper_bot）

**2、获取 Token**

+   BotFather 会给你一个 API token（格式：123456789:xxxxxxxklMNOpqrsTUVwxyz）

+   **不要发给别人** — 谁有这串代码谁就能控制你的机器人

**3、配置**

+   把 token 发给你现在已经配置好的其中某一个助理（小龙虾）

**4、获取密钥**

发送 /start，就可以获得这个密钥

**5、发给终端配置**

```
 openclaw pairing approve telegram 9XXXXX（你的密钥）
```

## **3.2 飞书配置**

### 配置飞书机器人通道

#### 打开飞书开发者后台：

https://open.feishu.cn/app?lang=zh-CN

![](img/ded4a07eb43512240775595e4b93b1e2.png)

#### 创建企业自建项目

输入完后点击创建

![](img/b771a859691ea81e3d4ce0234843fe00.png)

5.3获得app id 和app secret

![](img/123cec1874c63eb2df8d04bef75707aa.png)

## 3.3 🛰️接入

### 个人🛰️一键接入（ClawBot 插件，iOS 8.0.70+）

### 前置条件

+   🛰️版本：iOS 8.0.70 及以上

+   已部署并运行 OpenClaw 实例（本地/服务器均可）

### 操作步骤

1.  打开🛰️ → 我 → 设置 → 插件。

1.  找到「ClawBot」卡片，点击进入, 可以看到安装的命令提示。

复制终端安装命令（也可以直接执行这一步）:

```
npx -y @tencent-weixin/openclaw-weixin-cli@latest install
```

![](img/e6d37b5034436cd3833278787f37971c.png)

![](img/3a27415bad04ee9989dd5ffd2b476b4c.png)

![](img/5136089be571a857ae5db85ca5f4466c.png)

![](img/e6f775a17b0eceb1d349093cb766cc16.png)

![](img/84eee6f3c7423d19b7225a26ebfd055c.png)

![](img/61fcb68a15b700d479dc6772c59255e4.png)

# 四、文件解读+Skill安装

> 编写中...

https://clawhub.ai/

![](img/88514f2695c6571d127e58c66e93e0d3.png)

## 4.1 ClawHub.ai 是什么？

**简单说：它是** OpenClaw（小龙虾）的官方技能商店，相当于小龙虾的"App Store"。

### **核心功能**

OpenClaw 本身只是一个 AI 智能体框架，能干什么、能连接哪些平台，全靠安装"Skills（技能包）"来扩展。ClawHub 是 OpenClaw 的公共技能商店，截至 2026 年 3 月已收录超过 5000 个经社区验证的优质技能，覆盖网页检索、平台操作、自动化流程等全场景。另有资料显示实际数量更多，目前 ClawHub 上已有将近 2 万个 Skills，不同来源统计口径有所差异。

### **能安装什么技能？**

超过 100 类以上的技能可通过 ClawHub 获取，涵盖 GitHub 集成、浏览器自动化、CI/CD 触发、日历访问、智能家居控制等，还有针对📕、Twitter、YouTube 等平台的专项操作技能。

### **怎么用？**

ClawHub 支持搜索、安装、更新全流程管理，核心操作命令如 clawhub search、clawhub install、clawhub update 可直接在终端执行。

### **安全注意事项**

要审慎下载 ClawHub 的技能包，并在安装前审查技能包代码，不要使用要求"下载ZIP"、"执行 shell 脚本"或"输入密码"的技能包。部分技能涉及 Cookie 和 API Key，存在一定安全风险。

**一句话总结：** ClawHub.ai 就是给小龙虾"装手脚"的地方，没有它，OpenClaw 只是个聊天机器人；装对了技能，才能真正自动干活。这也正是你课程里教的核心之一，讲课时可以直接用这个比喻来解释。

## 4.2 安装Skill的方式

### 第一种：官方自带

打开默认的 UI，在里面找到“代理”（Agent），然后找到 Skill，把你所需要的内容直接点击“使能”就可以了。

![](img/f745e6f6e4c98f5fb379276927ca29bf.png)

### 第二种方式：插件市场下载（安全）

https://clawhub.ai/

首先先安装这个商城的插件工具：

```
npm i -g clawhub
```

![](img/efc62331c7d12ee19b95be00b0d4ac95.png)

![](img/d22579f3abfc2c91c9dc324ac5d7fe90.png)

安装的时候，直接参考你最后的这个标签，按照下面的格式输入即可：

```
clawhub install &quot;liang-tavily-search&quot;
```

### 第三种方式：GitHub下载（有风险）

https://github.com/vercel-labs/skills

```
npx skills add vercel-labs/agent-skills
```

比如，以这个仓库为例：https://github.com/JimLiu/baoyu-skills?tab=readme-ov-file

下载查找你有哪些Skills

仓库：https://github.com/vercel-labs/skills/tree/main/skills/find-skills

```
npx skills add https://github.com/vercel-labs/skills/tree/main/skills/find-skills
```

![](img/4814048a399aecc5b04d0631218b98e3.png)

![](img/0ab223904cdbef9f2ab389fa9e0099af.png)

## 4.3 如何使用Skill发🛰️👸号

第一步：下载baoyu的👸号发布包

![](img/5f7d72b1ecebb33bacadbc0c6009d835.png)

[baoyu-post-to-wechat.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IVPvbNPZkolbvGxkEbHcPrZanCd)

```
npx skills add https://github.com/JimLiu/baoyu-skills/tree/main/skills/baoyu-post-to-wechat
```

将这个skill放到自己的openclaw的skill文件夹中

![](img/d12b744a48363b4b051556307424a4be.png)

第二步：在这个Skill下面新建一个md文件，命名：EXTEND.md

```
default_theme: default
default_color: blue
default_publish_method: api
default_author: 科学羊
need_open_comment: 1
only_fans_can_comment: 0
```

![](img/facfff922e3ee51d8e5e68a31ca21f9f.png)

```
WECHAT_APP_ID=xxxx
WECHAT_APP_SECRET=xxx
```

直接对话即可！**注意🛰️👸号，开启白名单！**

![](img/100df666bb67f7d3593f81e0bb4d6129.png)

![](img/f11525c531e91de8e2f0740f25fa61b9.png)

![](img/29e5f9027330c9f762fb82f386741ef0.png)

## 4.4 图像生成skill

```
npx skills add https://github.com/JimLiu/baoyu-skills/tree/main/skills/baoyu-image-gen  
```

![](img/ccdf56340885b8f858df3e9614bacdf5.png)

https://aistudio.google.com/app

![](img/45e48d3ee9db507bfa45384003114273.png)

```
GOOGLE_API_KEY=AIXXXXXXXXXXXXX
GOOGLE_IMAGE_MODEL=gemini-3.1-flash-image-preview
```

接下来你就可以让 AI 配置，大家记得这个技能是出自宝玉的 GitHub。

大家只要按照这个命令安装完之后，就直接把你的这个 Key 去配置好就可以了。

![](img/02d2ec96b963e47f9d733f2e4d27f67a.png)

## 4.5 IMA-Skill安装 @20260318更新

配置地址：https://ima.qq.com/agent-interface

![](img/ac58ff917a61a20af0008c8f7663997b.png)

把第一步的内容直接一键复制，然后发给小龙虾，告诉他让他自己去配置。顺便你可以把这个 Key 直接给他，等他安装好之后就可以测试了。


![](img/553e9447c9669c3a9c49a0cb97569cbd.png)

![](img/4662ff671a25f41526a37fbb87be9cf3.png)

完毕！

# 五、如何创建群聊机器人

> 编写中...

第一步：打开飞书创建群聊，至少要拉一个人进群，才能创建群聊

![](img/3cf3313571923f9e1d9931d258d37bd8.png)

![](img/2c21dd3831eec696a65b9c7333a6d2e7.png)

![](img/e08064c10f7d7e2ebb25c2d5b6e382de.png)

```
openclaw config set channels.feishu.groupPolicy open
openclaw config set channels.feishu.replyToMode off
openclaw gateway restart
```

# 六、安全问题

如何加固你的小龙虾？

Step 1：Gateway绑定到本地 + 安全组双重封锁

这是最容易被忽视也是最致命的问题，需要两道防线同时做：

第一道：在OpenClaw配置里把Gateway绑定到本地地址，让端口物理上不对外开放——相当于龙虾根本不出门，门都没开，保证只有在这台机器本地运行的程序才能访问它：

```
{
    &quot;gateway&quot;: {
        &quot;bind&quot;: &quot;loopback&quot;
     }
}
```

第二道：**在云服务器安全组（腾讯云/阿里云）里限**制18789端口，只允许你自己的IP访问，其他来源全部拒绝——相当于小区大门就把陌生人挡住了：

```
入站规则：TCP 18789 端口，来源 → 你的IP地址（如 123.456.789.0/32）
删除或禁用：0.0.0.0/0 对18789的任何开放规则
```

Step 2：管好你的APIKey

不要把密钥直接写在配置文件里，使用环境变量引用；

在模型提供商后台设置消费上限（比如Kimi的49元月卡就是天然的消费上限）；

定期检查消费记录，发现异常及时轮换key。

Step 3：不要随便装来路不明的Skill

安装社区skill之前，至少做三件事：

看SKILL.md里有没有奇怪的命令、看scripts目录里的脚本在干什么、优先使用ClawHub上有审核评分的skill。

同时安装Skill Vette这款Skill，会在安装任何Skill之前，帮你把它审查一遍，类似于杀毒软件里。

**Step 4：群聊权限要隔离**

接入群聊时：开启 requireMention: true 只在被@时响应、设置 groupPolicy: "allowlist" 限定群组、设置DM隔离 session.dmScope: "per-channel-peer"、群聊场景不开放exec和文件读写权限。

然后也记得从聊天软件层面开启防护，比如我使用Discord群聊的操作就是关闭群聊的邀请。

Step 5：设置算力消耗的"保险丝"

在API提供商后台设消费上限、定时任务设合理超时时间防死循环、定期看 openclaw logs 有没有异常高频调用。避免第二天起床看到账单被刷爆。

**Step 6：定期跑安全审计**

```
openclaw security audit基础审计
openclaw security audit --deep深度审计
openclaw security audit --fix自动修复
```

**Step 7：开启沙盒模式**

让龙虾在Docker容器里运行，就算它要搞事情，也只能在容器里面折腾。特别是当龙虾跑在你自己常用的机器里的时候，需要特别注意这一点。

开启沙盒后，龙虾的exec默认是deny模式，需要你手动配置白名单来放行必要的命令。

建议至少每周一次，改完配置后立即跑一次。审计跑完后会给一些问题清单给你，可以直接把这些清单发给龙虾，让他给你修复的建议。

# 七、其他扩展

## 4.1 企业🛰️+云服务器

其实你也可以尝试搭建在云端服务器，然后绑定你的企业🛰️来测试，比如，我测试过：

https://cloud.tencent.com/act/pro/double12-2025（注意需要买海外的服务器，轻量级就可以）

![](img/13fea0f9ce2d421339e074e8868353b9.png)

搭建方法和我们mac的方式一样。

假如你需要对接企业🛰️，就需要成为企业开发者然后用企业🛰️来实现远程操作你的agent，如下图所示。具体关于企业🛰️的配置我就先不写了，大家如果感兴趣，我在后续补充。

![](img/761016180206adfd5cf0aae36bbbbd50.png)

## 4.2 常用指令

启动Openclaw的TUI

> openclaw tui

重启网关

> openclaw gateway restart

开启新对话

> /new

添加备用模型

> openclaw models fallbacks add [模型公司代号/模型名称]

例如：openai-codex/gpt-5.2-codex

设置默认模型

> openclaw models set [模型公司代号/模型名称]

例如：kimi-code/kimi-for-coding

openclaw gateway start【启动】

openclaw gateway restart【重启】

openclaw gateway stop【停止】

openclaw tui【命令行对话】

openclaw config【配置】(修改模型,通道,网关,等。。。)

npm update -g openclaw【更新openclaw主程序】

npm uninstall -g openclaw【卸载大龙虾】

模型管理命令

添加备用模型（主模型不可用时自动切换）

openclaw models fallbacks add [提供商代号/模型名称]

# 示例：添加OpenAI Codex作为备用

```
openclaw models fallbacks add openai-codex/gpt-5.2-codex
```

# 设置默认模型

```
openclaw models set [提供商代号/模型名称]
```

# 示例：将Kimi设为默认编程模型

```
openclaw models set kimi-code/kimi-for-coding
```

## 4.3 如何新增每日推送新闻订阅

你只需要在终端运行一行命令：

```
openclaw configure --section web
```

然后它会提示你输入 API Key，你粘贴进去就行。

![](img/a8d745ad628ae89f9376146e170743c1.png)

如果你还没有 Brave API Key：

1.  去 https://brave.com/search/api/

1.  注册免费账户（每月 2000 次搜索）

1.  创建 API Key，复制下来

1.  回来运行上面的命令，粘贴进去

具体执行截图：

![](img/2875c18ea8e1c3e8896674f542a1520a.png)

## 6.4 其他玩法

[OpenClaw + 影刀RPA组队：小龙虾指挥影刀 RPA 干活](https://yulu-tech.feishu.cn/docx/Ra9xdOnbWojwclxj9U0cftmynwf)

# 八、FAQ：智能体（discord、飞书等）突发中断怎么办？

重启大法：openclaw gateway

先启动Openclaw的TUI看对话是否正常。

> openclaw tui

**可能的原因：**

1. **Gateway 连接断了** — Discord 连接可能掉了，需要重启

2. **Token 过期** — Discord bot token 可能过期了

3. **频道权限** — bot 可能被禁言或没权限读消息

**检查/修复步骤：**

1. 检查 Gateway 状态

在终端运行

```
  openclaw gateway status                                                                                              
```

2. 如果状态异常，重启 Gateway

```
  # 先启用重启权限                                                                                                     
  openclaw config set commands.restart true                                                                            
                                                                                                                       
  # 然后重启     
   openclaw gateway                                                                                                      
  openclaw gateway restart   
  
                                                                                            
```

3. 检查 Discord Bot Token

去 Discord Developer Portal (https://discord.com/developers/applications)：

+   你的应用 → Bot → 看 Token 是否有效

+   如果显示 "Invalid" 或 regenerate 过，需要更新 token

**4. 检查频道权限**

+   bot 是否还在频道里？

+   是否有 "Read Messages" 和 "Send Messages" 权限？

**手动启动（每次开机后）**

打开终端，运行：

```
  openclaw gateway start    
```

6.4 出现异常报错

```
⚠️ Agent failed before reply: All models failed (2): openai-codex/gpt-5.2-codex: OAuth token refresh failed for openai-codex: Failed to refresh OAuth token for openai-codex. Please try again or re-authenticate. (auth) | moonshot/kimi-k2.5: ⚠️ API rate limit reached. Please try again later. (rate_limit).Logs: openclaw logs --follow
```

这个错误意味着网关尝试使用您的 Codex OAuth 刷新令牌，但令牌已过期，刷新失败 (取消/轮换/无效)。当发生这种情况时，机器人无法进行模型调用→无响应。

重新认证 Codex OAuth(最常见的解决方案)

Re-run the Codex** OAuth login** on the gateway host:

在网关主机上重新运行 Codex OAuth 登录：

```
openclaw models auth login --provider openai-codex
openclaw gateway restart
```

只需要运行上面这两个指令就可以了。注意登录 Codex 的时候，需要做一个授权。

好了，以上就是今天的分享，但是**文**章并不止步，关于OpenClaw的详细用法和技巧，我会学到新的内容后第一时间来更新文章，欢迎大家的关注与支持。

## 信息不回复怎么办？

![](img/a021af412c08d6b185b1f3d4bff6529d.png)

我非常建议你至少要有 3 到 4 个客户端，然后分门别类地进行监控，这样会比较好一点。

比如我自己用的telegram、discord、飞书、TUI等

![](img/0ea3822cf904a1e27a0f2c82f96bd9cf.png)

![](img/57867524a39abbcef83bb915896bfbea.png)

```
异常时先 openclaw status
需要时 openclaw gateway restart
```

![](img/7461c36f018fa1d794d7b9e7b20e9cbd.png)