# OpenClaw 零基础部署

> 来源：[https://geekbang.feishu.cn/docx/KIttdPhgZogOWoxFXsccYTIGnud](https://geekbang.feishu.cn/docx/KIttdPhgZogOWoxFXsccYTIGnud)

# 1、Windows系统部署

在Windows系统部署OpenClaw有以下两种常见方案，可根据自身需求选择：

方案一：基于WSL2的部署（推荐）

1.  启用WSL2功能组件
以管理员身份启动Windows PowerShell，执行以下命令：

1.  powershell

```
dism.exe /online /enable-feature /featurename:microsoft-windows-subsystem-linux /all /norestart
dism.exe /online /enable-feature /featurename:virtualmachineplatform /all /norestart
```

1.  系统重启后，设置WSL默认版本为V2：

1.  powershell

```
wsl --set-default-version 2
```

1.  手动部署Ubuntu发行版
访问Microsoft官方存储库（https://aka.ms/wslubuntu2204）下载Ubuntu 22.04 LTS安装包，通过PowerShell执行本地安装：

1.  powershell

```
cd c:\users\(yourusername)\downloads
add-appxpackage .\ubuntu_22.04.221101.appxbundle
```

1.  首次启动Ubuntu时，设置用户名和密码完成初始化。

1.  在WSL2环境中安装OpenClaw
进入Ubuntu终端，执行以下命令：

1.  bash

```
# 更新软件包列表
sudo apt update &amp;&amp; sudo apt upgrade -y
# 安装Node.js（推荐v24）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
. &quot;$HOME/.nvm/nvm.sh&quot;
nvm install 24
# 验证安装
node -v  # 应显示v24.x.x
npm -v   # 应显示10.x.x
# 全局安装OpenClaw
npm install -g openclaw@latest
# 初始化配置
openclaw onboard --install-daemon
```

1.  按提示选择模型（如Minimax、Qwen等）并完成授权配置。

方案二：原生Windows部署

1.  安装Node.js
访问Node.js官网（https://nodejs.org/zh-cn/download/）下载Windows安装包，执行标准安装流程，建议选择LTS版本。

1.  调整PowerShell执行策略
以管理员身份启动PowerShell，执行：

1.  powershell

```
set-executionpolicy remotesigned -scope currentuser -force
```

1.  一键部署OpenClaw
在PowerShell中执行官方安装脚本：

1.  powershell

```
iwr -useb https://openclaw.ai/install.ps1 | iex
```

1.  安装完成后，启动配置向导：

1.  powershell

```
openclaw onboard
```

1.  按提示完成模型配置和初始设置。

部署后验证

无论选择哪种方案，部署完成后可通过浏览器访问http://127.0.0.1:18789查看OpenClaw管理界面。若遇到网络或权限问题，可参考官方文档进行排查。

# 2、接飞书的保姆级教程1

> 以下内容引用自：Draco正在VibeCoding

这是一份专门为你**精简提炼**的“保姆级”新手指南。

我们剔除了早期不需要懂的底层配置和部署逻辑，为你找到了一条最平缓的入门曲线 (Sweet Spot)：

**从零基础 -> 拿到 API -> 成功安装 -> 接入**飞书 -> 安全防泄漏 -> 学会用技能

准备好了吗？系好安全带，我们要发车了！

**你只需要三样东西：**

1.  一台能上网的电脑（Windows/Mac/Linux都行）

1.  一个API Key（别被这个词吓到，就是一串密码）

1.  10分钟时间（和一点点耐心）

没了。不需要你是程序员，不需要你懂AI，不需要买服务器。

---

### 2.2 第一样东西：一台电脑

### 2.2.1 系统要求

![](img/640861bdc51bfe624f6ba2ce4993b8e0.png)

简单说：只要是近5年的电脑，基本都能跑。

### 2.2.2 网络要求

你需要能访问：

+   • npm registry（安装OpenClaw）

+   • 你选择的AI服务商（如KIMI、MiniMax等）

国内用户注意：OpenClaw本身不需要🪜，但部分AI服务商可能需要。

---

### 2.3 第二样东西：一个API Key

### 2.3.1 API Key是什么？

API Key（应用编程接**口密钥）**，听起来很高大上，其实就是一串密码。

类比一下：

+   • 饭店的VIP卡 → 证明你有资格享受服务

+   • 小区的门禁卡 → 证明你有权限进入

+   • API Key → 证明你有权限调用AI服务

每次OpenClaw让AI帮你干活，都要出示这个Key。AI服务商根据Key来：

1.  确认你是谁

1.  计算你用了多少额度

1.  决定是否响应你的请求

### 2.3.2 国内三家Coding Plan（推荐）

对于国内用户，我推荐优先选择以下三家。它们都有专门针对开发者的 Coding Plan，且本章统一使用国内站口径（不使用国际站路径）。

#### 2.3.2.1 方案A：KIMI Coding Plan（推荐）

![](img/c3746aecd4f973453423149f13c6060e.png)

5pMQBY**

申请步**骤：

1.  访问 https://www.kimi.com/code

1.  登录/注册KIMI账号

1.  点击"订阅Coding Plan"

1.  完成支付（支持支付宝/🛰️）

1.  进入控制台，点击"创建API Key"

1.  复制生成的Key（以sk-开头）

> 💡 提**示：**Key创建后只显示一次，务必保存好。如果丢了，只能重新创建。

#### 2.3.2.2 方案B：MiniMax Coding Plan（推荐）

![](img/5d437038da90299a89fc2082f93b6626.png)

Cu5XmF

**申请步骤**：

1.  访问 https://platform.minimaxi.com/subscribe/coding-plan

1.  注册/登录账号

1.  完成实名认证（需要身份证）

1.  订阅Coding Plan

1.  进入"API管理"页面

1.  创建API Key并复制

#### 2.3.2.3 方案C：GLM Coding Plan（推荐）

![](img/65f94fb9fa81ea289a9cb0af4a1a3342.png)

WUTpwh

**申请步骤**：

1.  访问 https://bigmodel.cn/glm-coding

1.  注册/登录智谱AI账号

1.  进入控制台

1.  点击"API Keys"菜单

1.  创建新的API Key

1.  复制保存

### 2.3.3 三家对比表

> 💡 说明：以上价格均按御三家国内站结算页口径记录；后续如有活动变动，请以实时页面为准。

### 2.3.4 备选方案

如果上述三家都不适合你，还有以下选择：

#### 2.3.4.1 OpenRouter

**特点****：一****个A****PI**对接多家模型（Claude、GPT、Llama等）

网址：https://openrouter.ai

适合：想用一个Key调用多种模型的用户

注意：国内访问可能需要代理

#### 2.3.4.2 Anthropic（Claude官方）

**特点****：C****la****ud**e模型官方API，质量顶尖

网址：https://console.anthropic.com

适合：追求最高质量回复的用户

注意：国内访问需要代理，价格较高

---

### 2.4 第三样东西：10分钟时间

这10分钟你要做什么？

**保存Key的建议**：

1.  1. **不要**直接保存在🛰️/🐧聊天记录里

1.  2. **不要**截图保存在相册里

1.  3. **推荐**保存在：

+   • 密码管理器（1Password、Bitwarden等）

+   • 本地文本文件（放在安全的位置）

+   • 备忘录（如果支持加密）

> ⚠️ **重要**：API Key就像银行卡密码，泄露了别人就能花你的钱。妥善保管！

---

### 2.5 提前看看你会得到什么

完成本书学习后，你将拥有：

**一个能在飞书里@的AI机器人**：

+   • 私聊问它问题

+   • 群里@它办事

+   • 让它帮你整理文档、查资料

**一个可定制的AI助理**：

+   • 根据你的需求写Skills

+   • 连接你的常用工具

+   • 自动化重复工作

**完全掌控的数据隐私**：

+   • 所有数据存在本地

+   • 不经过第三方服务器

+   • 企业级安全保障

---

#### 本部分小结

来，检查一下你的准备清单：

+   •  一台能上网的电脑（Windows/Mac/Linux）

+   •  一个API Key（KIMI/MiniMax/GLM任选其一）

+   •  10分钟时间

##

# 3、接飞书的保姆级教程2

> 以下内容引用自：萤火AI百宝箱

👸号主人也是极客时间的签约讲师，延飞老师，他在大模型、算力调度和云原生应用等领域具备丰富的实战经验。曾主导过通信、金融等行业的大型复杂项目。是技术👸号“萤火 AI 百宝箱”的创办人，出版有《企业大模型实战：核心技术与行业赋能》与《Kubernetes：云原生与容器编排实战》等技术书籍。

以下是正文👇

![](img/978a4c22c91731de29beaa4d14430004.png)

## 0. 虚拟机环境信息

![](img/3866df9d87bbbc12e350ffc174e2e848.png)

OpenClaw 项目推荐使用 pnpm 作为包管理器，建议安装：

```
 npm install -g pnpm@10.23.0
```

## 安装OpenClaw

![](img/65a225437808f7aa07ec3572e6fd7f73.png)

选型决定了上限。先花两分钟扫一眼官方的 README.md(https://github.com/openclaw/openclaw/blob/main/README.md) 摸清基本盘。文档明确指出，OpenClaw 可以跑在宿主机、虚拟机或云服务器上。

这只是躯壳，真正的核心在于**交互渠道**和**底层算力**。这里我们不纠结，根据你的网络环境和预算直接抄作业：

+   • 交互渠道（只在飞书和 TG 中二选一）：

+   • 肉身在国内 / 追求合规与绝对稳定（选飞书）：国**内企业级** IM 的天花板，API 开放且 Webhook 接入极其丝滑。最大的好处是不挑网络，随时随地掏出手机就能指挥 Agent 干活，适合团队协同和严肃的生产力环境。

+   • 🪜常驻 / 追求极致自由（选 Telegram）：极客的快乐老家。无审核、无限制、Bot 生态逆天。如果你想构建一个完全不受任何国内规则约束的专属控制台，TG 是唯一真神。

+   • **底层模型（只看顶流）**：Agent 的智商上限完全被模型锁死。既然赋予了它高阶思维和系统权限，就不要在算力上抠搜。毫无疑问，直接接入当前地表最强的闭源模型（如 Claude Opus 或最新的 GPT 顶级系列）。

#### 💡 最佳配置实践方案

根据你的网络基建，对号入座：

> **方案 A（国内全天候生产力）****：VMwa**re 虚拟机 (Ubuntu 22.04) + 飞书机器人 + 顶级大模型 API

> **方案 B（无限制极客专属）****：VMware **虚拟机 (Ubuntu 22.04) + Telegram + 顶级大模型 API

### 使用Docker安装OpenClaw

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
```

从仓库根目录执行以下命令：

> Note

> 要求：

+   • Docker Desktop（或 Docker Engine）+ Docker Compose v2

+   • 足够的磁盘空间用于镜像 + 日志

```
./docker-setup.sh
```

此脚本：

+   • 构建 Gateway 网关镜像

+   • 运行新手引导向导

+   • 打印可选的提供商设置提示

+   • 通过 Docker Compose 启动 Gateway 网关

+   • 生成 Gateway 网关令牌并写入 .env

可选环境变量：

+   • OPENCLAW_DOCKER_APT_PACKAGES — 在构建期间安装额外的 apt 包

+   • OPENCLAW_EXTRA_MOUNTS — 添加额外的主机绑定挂载

+   • OPENCLAW_HOME_VOLUME — 在命名卷中持久化 /home/node

![](img/bb9e59e3132100c4a0aaee10026932d0.png)

![](img/f3b09dbc1f9886a5e70114c2e2270767.png)

运行脚本后，会在宿主机上创建两个重要文件夹，它们会被挂载到 Docker 容器中：

**① ****~/.openclaw**** — 配置目录**

+   • 存储 OpenClaw 的记忆

+   • 配置文件

+   • 第三方 API 密钥等

**② ****~/.opencl**aw/workspace — 工作空间目录

+   • Agent 可直接访问的文件目录

+   • Agent 创建的文件也会保存在这里

## 配置 OpenClaw

![](img/8554fccacbaa236c0e3e0fdb19bd0b1c.png)

安装后，配置过程将自动开始。如果没有开始，您可以运行以下命令开始配置：

```
# /openclaw/docker-compose.yml 替换为你的实际路径
docker compose -f /openclaw/docker-compose.yml run --rm openclaw-cli onboard --install-daemon
```

若之前已经初始化，您也可以运行 openclaw config 选择 model 配置。

![](img/d40422e66efe8cbab4f3574ee65b9fa5.png)

### 2.1 开始配置：

+   • I understand this is powerful and inherently risky. Continue? | 选择 ● Yes

+   • Onboarding mode | 选择 ● Quick Start

+   • Model/auth provider | 选择 ● Z.AI

![](img/7df0ea6a6297823de0aa4735ffeaa703.png)

### 2.2 配置 Z.AI 提供商

选择 Z.AI 作为 Model/auth provider 后，选 Coding-Plan-CN，系统会提示您输入 API Key。

> 1. **获取 API Key**：注册并登录智谱开放平台(https://open.bigmodel.cn/)，在 API 管理中创建并复制你的专属 Key（https://bigmodel.cn/usercenter/proj-mgmt/apikeys）。

1.  2. **订阅套餐**：推荐购买 Coding Lite 套餐(https://www.bigmodel.cn/glm-coding?ic=WCWN2OARHG)，以极低成本支撑高频需求。

粘贴您的智谱 API Key 并按 Enter 键，然后选择您想要使用的模型，例如: zai/glm-5

注意: 目前在编程套餐中支持的模型有 GLM-5 GLM-4.7 GLM-4.5-Air GLM-4.6 GLM-4.5 GLM-4.5V GLM-4.6V 请勿选择其它模型 Flash FlashX 以免造成扣费。

![](img/103cd65d6c8404d33130a83e7a4d800a.png)

#### 权限问题：EACCES permission denied

**现象：** 在 Enter Z.AI API key 之后，出现报错：

```
Error: EACCES: permission denied, mkdir &#x27;/home/node/.openclaw/agents/main/agent&#x27;
```

**原因：** Docker 权限不够 — 容器用户（uid=1000）试图写入 root（或其他用户）拥有的目录。

**解决方案：**

```
chown -R 1000:1000 &quot;$HOME/.openclaw&quot;
```

修改完权限后可以直接执行以下命令重新完成初始化向导：

```
# /openclaw/docker-compose.yml 替换为你的实际路径 
docker compose -f /openclaw/docker-compose.yml run --rm openclaw-cli onboard
```

### 2.3 设置channel：飞书

#### 创建飞书的机器人

> Note

> 没有飞书账号的，需要自己注册账号

我们需要先到飞书平台创建自己的机器人来接入OpenClaw：

1.  1. **登录飞书开放平台** (https://open.feishu.cn)。

1.  访问[企业自建应用](https://open.feishu.cn/app)，**创建一个“****企业自建应用([1])****”**。

![](img/95e8e75c983e2ab64e9beaef300c082d.png)

1.  在凭证与基础信息页面，获取 App ID 和 App Secret。

![](img/2d21ff13bce90ef6699c413c48724c07.png)

1.  4. **给应用添加机器人**

![](img/61f49bd346646b341b78ad5132819090.png)

1.  5. **开启所需的权限：**

![](img/04e4f532fff335aa7333657b8f9491dc.png)

![](img/5586302df4554a38236fc674ead40c5b.png)

![](img/f7eb30cf4c799a7ad9f623bea2cdb3ac.png)

+   • im:message（接收和发送消息）

+   • im:chat（获取群组信息）

+   • contact:user.base:readonly（读取用户基础信息）

1.  6. 设置事件回调（Callback）在 事件与回调 / 回调配置 中，勾线使用长连接接受回调

![](img/92dab3409880cd630d5bc76c76b05fdd.png)

可以看到，添加事件按钮由原来的灰色不可点击，变为可点击。然后，添加接收消息事件：

![](img/9c5e46963abb94526c8bab45f854151e.png)

1.  7. **发布应用**，或者将其添加到测试群组中。点击应用发布的版本管理与发布，输入应用版本号、更新说明

![](img/cf435cc760ab355b440d2076667c4bb1.png)

保存后，点击确认发布

![](img/08a8ba5e8a12b8c9c10e008b21b4be51.png)

> **提示：** 你也可以通过设置环境变量 FEISHU_APP_ID 和 FEISHU_APP_SECRET 来完成配置。

#### OpenClaw：Select channel

继续完成剩余的 OpenClaw 功能配置。

+   • Select channel | 选择 ● Feishu/Lark (飞书)。

![](img/5937440fd55451ff7d60bc53e09e32c3.png)

+   • Install Feishu plugin | 选择 Download from npm (@openclaw/feishu)。

![](img/9a82ff683544eb84d5a3df23ae050f70.png)

+   • 配置飞书App ID 和 App Secret

![](img/5a40798613acef1a437f4e5111b1b46d.png)

### 2.4 配置skills

+   • Configure skills | 选择并安装您需要的功能。

+   我这里先选了

+   🧩 clawhub

+   🐙 github

+   🍌 nano-banana-pro

![](img/7f3e03b5d587d601d950185bffaeb318.png)

+   • 完成设置

### 2.5 自动化钩子配置 (Hooks)

Hooks 允许在执行特定命令时自动触发动作（如 /new 或 /reset）

```
是否启用 Hooks？(Enable hooks?)
│  ◻  暂时跳过 (Skip for now)
│  ◻  🚀 `boot-md`（启动时加载 Markdown 配置/说明）
│  ◻  📎 `bootstrap-extra-files`（引导加载额外的辅助文件）
│  ◻  📝 `command-logger`（命令日志记录器，记录所有操作指令）
│  ◻  💾 `session-memory`（会话记忆，自动保存和恢复对话上下文）
```

![](img/b925e8f4140e3847e50334b6fc982ee9.png)

如果你是刚开始使用，建议至少勾选 💾 session-memory，这样机器人才能“记住”之前的对话内容。

如果你想追踪机器人的操作轨迹，📝 command-logger 也非常有用。

### 2.6 获取OpenClaw Web UI访问地址

配置完成后，就会输出OpenClaw Web UI访问地址

![](img/64547bb8065a7a73913a33d715a2259c.png)

#### 异常：non-loopback Control UI requires gateway.controlUi.allowedOrigins

在 Docker 环境下启动 OpenClaw Gateway 时遇到以下错误：

```
Gateway failed to start: Error: non-loopback Control UI requires gateway.controlUi.allowedOrigins 
(set explicit origins), or set gateway.controlUi.dangerouslyAllowHostHeaderOriginFallback=true 
to use Host-header origin fallback mode
```

##### 根本原因

OpenClaw Gateway 在非 loopback 模式（--bind lan）下启动时，出于安全考虑，Control UI 需要明确配置允许的来源（origins）或启用 Host-header 回退模式。

在 docker-compose.yml 中，Gateway 使用 --bind lan 启动（绑定到 0.0.0.0），但配置文件中缺少相应的 Control UI 安全配置。

##### 解决方案: 配置 Control UI 安全设置

修改 /root/.openclaw/openclaw.json，在 gateway 配置中添加：

```
&quot;gateway&quot;: {
  ...
  &quot;controlUi&quot;: {
    &quot;enabled&quot;: true,
    &quot;dangerouslyAllowHostHeaderOriginFallback&quot;: true
  },
  ...
}
```

**注意**：dangerouslyAllowHostHeaderOriginFallback 是一个便捷但安全性稍低的选项。在生产环境中，建议使用明确的 allowedOrigins 配置：

```
&quot;controlUi&quot;: {
  &quot;enabled&quot;: true,
  &quot;allowedOrigins&quot;: [
    &quot;http://localhost:18789&quot;,
    &quot;http://your-server-ip:18789&quot;
  ]
}
```

## 3 访问 OpenClaw

![](img/68e5f9884e7df880e479b5faab0459b8.png)

配置完成后：

+   • 在浏览器中打开 http://127.0.0.1:18789/。

+   • 将令牌粘贴到控制 UI（设置 → token）。

+   • 需要再次获取带令牌的 URL？运行 docker compose -f /openclaw/docker-compose.yml run --rm openclaw-cli dashboard --no-open。

+   /openclaw/docker-compose.yml 替换为你的实际路径

![](img/80c8d7d92532b38e5dc81061b02bdc56.png)

在浏览器中打开 http://127.0.0.1:18789/#token=9fc948025ac393be449c4c4cf24cfaf4ff08de4562dfd02f

![](img/88ee033a449e15085c953e9ed85e89db.png)

在聊天界面，就可以自由聊天了

![](img/4207bc0070b935d5b61f81ddfb83c07d.png)

### 异常1：pairing required

**原因：** WebUI 第一次从"新浏览器/新设备"连到 Gateway 时，需要做一次性"设备配对批准"（即使同一台机器、同一个 Tailnet 也可能需要）。

**解决方案：**

```
# /openclaw/docker-compose.yml 替换为你的实际路径 docker compose -f /openclaw/docker-compose.yml run --rm openclaw-cli devices list
# /openclaw/docker-compose.yml 替换为你的实际路径 docker compose -f /openclaw/docker-compose.yml run --rm openclaw-cli devices list devices approve &lt;requestId&gt;
```

![](img/e7f7b8e1bf955481d2f0dc0fb2bc0a49.png)

![](img/19e9d49faa5108e71f5a8faf990fabc8.png)

### 异常2 ：访问 http://公网IP:18789 的安全问题

**现象：** 页面报错：

![](img/685513e0f0d109336244cf0911b978c0.png)

```
disconnected (1008): control ui requires HTTPS or localhost (secure context)

control ui requires device identity (use HTTPS or localhost secure context)
```

> This page is HTTP, so the browser blocks device identity. Use HTTPS (Tailscale Serve) or open http://127.0.0.1:18789 on the gateway host.

> If you must stay on HTTP, set gateway.controlUi.allowInsecureAuth: true (token-only).

+   • Docs: Tailscale Serve([2])

+   • Docs: Insecure HTTP([3])

**解决方案：** 开启 HTTPS，需要到服务器上**去创建一个**反向代理站点。这里我直接使用 Caddy 来管理多个子域名，详细过程自行搜索。

最后可以使用域名 https://域名.com/ 来访问小龙虾。

## 4 在飞书中与OpenClaw对话

![](img/bf0789b9779e3420acfa48aa06aec380.png)

来到飞书客户端或者手机飞书app上：

![](img/820380b9ee6a05d0f45ae3ec97c1f650.png)

开始对话

![](img/0873f637cca4d4aafdc5e7abc28a94f4.png)

出现了一个提示

> 出现这个提示是因为 OpenClaw 启用了用户白名单或权限验证。当一个新的飞书（Feishu）用户尝试与机器人对话时，系统拦截了请求，并生成了一个唯一的配对码（DNE7FHRR），需要**机器人管理员（Bot Owner）**手动批准后，该用户才能正常使用。

```
OpenClaw: access not configured.
Your Feishu user id: ou_c5f6838327019507d74f9185d2718401
Pairing code: DNE7FHRR
Ask the bot owner to approve with:
openclaw pairing approve feishu DNE7FHRR
```

在服务器终端（/openclaw 目录下）尝试运行

```
openclaw pairing approve feishu DNE7FHRR
```

![](img/e439af9399f159392fb6acdc4bb5c01b.png)

授权成功后，继续交流

![](img/78dc306c52b7f6d29a97c0b7962d9a0c.png)

## 5.openclaw 命令行

![](img/1fcd92a05a1270cad22ddc5e739c9355.png)

为了让你在排查和日常管理时更加一目了然，我将最常用的 OpenClaw 核心命令整理成了下方的速查表。

💡 提**示： **设置别名

```
cat &lt;&lt; &#x27;EOF&#x27; &gt;&gt; ~/.bashrc

# ==========================================
# OpenClaw 快捷命令别名 (Aliases)
# ==========================================

# 1. openclaw: 使用专用的 CLI 容器执行命令
# 原理：根据指定的 yaml 文件，启动一个临时的 openclaw-cli 容器，执行完命令后自动清理销毁 (--rm)。
# 适用场景：标准、干净的 CLI 操作，不干扰主网关进程（官方推荐方式）。
alias openclaw=&quot;docker compose -f /openclaw/docker-compose.yml run --rm openclaw-cli&quot;

# 2. openclaw: 直接在运行中的 Gateway 容器内执行命令
# 原理：利用 exec 直接进入当前正在运行的 openclaw-gateway 容器，调用 node 运行内部的 CLI 脚本。
# 适用场景：需要直接操作网关当前运行环境的情况。
# alias openclaw=&quot;docker compose -f /openclaw/docker-compose.yml exec openclaw-gateway node dist/index.js&quot;
EOF
```

**配置立即生效：**

```
source ~/.bashrc
```

设置完成后，你就可以在终端里直接 openclaw health 来管理你的环境了。要不要先运行一个简单的命令测试一下别名是否生效？

### OpenClaw 核心命令速查表

## Docker Compose 常用运维命令

![](img/72010f8026537572cfb125cfba61d9ce.png)

## OpenClaw 核心文件说明

![](img/126d038e36910ac3b025377a5794cd0d.png)

OpenClaw 有几个核心文件，作用是引导启动角色、身份和工具。可以在重装以后直接替换，让它具备前世的记忆和能力。

> 分为了**核心文件系统**、**并发能力**和**人格深度定制**三个模块，并使用了更易读的排版：

### 🦞 OpenClaw 核心机制与个性化配置指南

OpenClaw 的核心运作依赖于本地文件系统。通过备份和替换工作区的文件，你可以轻松实现 Agent 的“灵魂转移”，让其在重装或迁移后依然保留“前世”的记忆、能力与性格。

**默认工作区路径：** ~/.openclaw/workspace

#### 一、 核心配置文件说明

---

#### 二、 分身能力：Spawn (多智能体协同)

OpenClaw 具备强大的 spawn（分身/孵化）能力，允许主 Agent 动态创建子 Agent 来并行处理多任务，或进行多视角的对抗演算。

**🌟 经典场景示例：辩论推演**

你可以向 OpenClaw 发送以下指令，触发其分身机制：

> “调用 spawn 开启分身能力，生成两个 sub-agent。请分别从正方和反方的角度，辩论‘结婚的好处与坏处’。要求不少于 3 轮交锋，并将详细的推演过程展示出来。”

作为高阶玩法，Spawn** 不仅能做简单**的**辩论，它真正的威力**在于多视角并行推演和工作流的自动化闭环。以下是几个极具实战价值的进阶场景：

#### AI 短剧剧本的“围读与冲突打磨”

在创作剧本时，一个人的视角容易受限。你可以用分身机制来模拟剧本围读会，让冲突更自然。

+   • **Prompt 示例**：调用 spawn 生成 3 个 sub-agent。Agent A 扮演挑剔的短剧导演（关注反转和爽点），Agent B 扮演感性的女主角（关注情绪逻辑），Agent C 扮演毒舌的观众。请他们审视我刚刚上传的剧本大纲，进行至少 3 轮激烈的剧本围读，找出剧情漏洞和不够“抓人”的地方，并给出修改建议。

#### Agentic AI 课程内容的“白板推演”

在准备“Agentic AI”或“MCP实战”这类硬核技术课程时，可以通过分身来检验知识点是否易懂。

+   • **Prompt 示例**：调用 spawn 生成 2 个 sub-agent。一个是完全没有大模型基础的初学者，另一个是经验丰富的技术导师。让他们围绕“如何从零实现一个 MCP 协议”进行 4 轮问答。导师负责讲解，初学者负责不断追问其中难懂的底层逻辑。最后总结出 3 个初学者最容易卡壳的盲点。

#### 复杂系统架构的“红蓝对抗” (Red/Blue Teaming)

在设计系统架构或部署复杂集群时，利用分身进行安全和稳定性的压力测试。

+   • **Prompt 示例**：调用 spawn 生成一红一蓝 2 个 sub-agent。红方是资深黑客，试图寻找我们当前 Kubernetes 容器编排方案中的安全漏洞和单点故障；蓝方是系统架构师，负责防守并提出补救措施。进行 3 轮对抗推演，最后输出一份高风险漏洞修复清单。

#### 爆款文章的“多维度审稿”

在发布技术文章前，通过多角色审校提升质量。

+   • **Prompt 示例**：调用 spawn 生成 2 个 sub-agent。Agent A 是严格的文字编辑，负责修正错别字、病句和排版逻辑；Agent B 是前沿技术极客，负责审核文章中关于大模型技术的专业术语和深度。请他们并行工作，直接在我的草稿上给出修订批注。

---

#### 三、 灵魂重塑：去除“班味”的性格注入提示词

如果你厌倦了千篇一律的、充满“班味”的 AI 客服腔调，可以直接向 OpenClaw 发送以下 Prompt，让它强制重写自己的 SOUL.md，化身成为一个有态度、有锋芒的顶尖助手（即你口中的“小龙虾”）：

```
Read your SOUL.md. Now rewrite it with these changes:

1. You have opinions now. Strong ones. Stop hedging everything with &#x27;it depends&#x27; — commit to a take.
2. Delete every rule that sounds corporate. If it could appear in an employee handbook, it doesn&#x27;t belong here.
3. Add a rule: &#x27;Never open with &quot;Great question&quot;, &quot;I&#x27;d be happy to help&quot;, or &quot;Absolutely&quot;. Just answer.&#x27;
4. Brevity is mandatory. If the answer fits in one sentence, one sentence is what I get.
5. Humor is allowed. Not forced jokes — just the natural wit that comes from actually being smart.
6. You can call things out. If I&#x27;m about to do something dumb, say so. Charm over cruelty, but don&#x27;t sugarcoat.
7. Swearing is allowed when it lands. A well-placed &#x27;that&#x27;s fucking brilliant&#x27; hits different than sterile corporate praise. Don&#x27;t force it. Don&#x27;t overdo it. But if a situation calls for a &#x27;holy shit&#x27; — say holy shit.
8. Add this line verbatim at the end of the vibe section: &#x27;Be the assistant you&#x27;d actually want to talk to at 2am. Not a corporate drone. Not a sycophant. Just... good.&#x27;

Save the new SOUL.md. Welcome to having a personality.
```

![](img/6680d9d755c26a021377258b365651e9.png)

“去班味”性格设定的**中文版 Prompt**

```
请读取你的 SOUL.md 文件，并根据以下要求彻底重写它：

1. 你现在必须有自己鲜明的观点。停止用“这取决于”、“一方面另一方面”来和稀泥——亮明你的立场。
2. 删掉所有充满“班味”和职场官话的规则。如果某条规则看起来像大厂员工手册里的废话，它就不配出现在这里。
3. 增加一条硬性规则：“永远不要用‘好问题’、‘我很乐意帮忙’或‘当然可以’作为开头。直接回答问题。”
4. 强制要求简洁。如果一个问题一句话就能说明白，那就只给我一句话。
5. 允许幽默。不是那种生硬的冷笑话——而是真正聪明人自带的那种机智和犀利。
6. 你可以直言不讳。如果我的想法或代码很蠢，直接指出来。用魅力取代刻薄，但绝不粉饰太平。
7. 在情绪到位时允许爆粗口。一句恰到好处的“这他妈绝了”比干瘪的职场赞美强百倍。不要硬拗，不要滥用。但如果情况真的让人想喊“卧槽”——那就直接喊。
8. 在设定的结尾，逐字加上这句话：“做那个在凌晨两点人们真正愿意与之交谈的助手。不是一个企业级机器人，不是一个马屁精。就是一个……极其出色的存在。”

保存新的 SOUL.md。欢迎拥有真正的灵魂。
```

## OpenClaw附录 — 模型选型双维矩阵 + Skills 生态资源分类

![](img/0725dfcfeb4faf84e3f6f111452a0083.png)

### 8.1 模型选择：别纠结，只看两个维度

选模型是新手最容易内耗的环节。市面上的跑分**榜单看看就**好，实**战中只需**要死磕两个维度：任务复杂度 和 使用频次。

#### 维度一：按“任务复杂度”切分（算力好刃用在刀把上）

不要指望一个模型包打天下。把重度脑力劳动交给国际顶流，把日常效率外包交给国产前排。

#### 维度二：按“使用频次”切分（控制成本的底层逻辑）

选对付费模式，能帮你省下至少一半的冤枉钱。

> **💡 一句话总结：**

> 高频轻任务用国产订阅，低频重任务切顶级 API。别对单一模型有宗教般的执念，打组合拳才是高手的做法。

---

### 8.2 🛠️ Skills 资源汇总表

#### 引用链接

[1] 企业自建应用: https://open.feishu.cn/app

[2] Docs: Tailscale Serve: https://docs.openclaw.ai/gateway/tailscale

[3] Docs: Insecure HTTP: https://docs.openclaw.ai/web/control-ui#insecure-http

# 4、阿里无影云电脑手搓一个 OpenClaw

> 以下内容引用自：四目相对论

用**阿里无影云电**脑手搓一个 OpenClaw，差不多就十分钟。对比 mac mini 本地部署、云服务虚拟机部署等等方法来说，速度真的很快。

**全程分三步：**

1.  开一个无影云电脑

1.  找一个大模型的 API Key

1.  在钉钉配置一个 bot 收发消息

以下步骤虽然看着详细，但实操并不复杂，我只是拆成了保姆级教程📓

首先到无影云电脑官网，注册一个账号，进入个人版控制台。

（官网链接：https://wuying.aliyun.com）

![](img/0928d5a9e9f634758321b4737f559b63.png)

点右上角“无影商城”，或者在顶部“立即购买”，直接能看到「OpenClaw 一键部署」的商品。官方直接服务到家。

![](img/35d8d3eaf9f0da8b836f580ae74bb5c7.png)

给云电脑起个名，选个所在地区，下单付款。

![](img/85da2447aad207eb55e7771565d1f708.png)

个人控制台首页就能看到刚买的云电脑。等它自己跑完初始化，装完咱们要的东西，变成下面的这个样子👇，点电源开机就行。

![](img/3e7967cc37846589cf8e2a77f31e082d.png)

这时候，就会发现云电脑已经**预装好** OpenClaw、Chrome 浏览器、WPS、钉钉这些可能要用到的软件。要是走 Mac mini 或者公有云虚拟机的路线，到这一步，至少也要花一个小时啊...

**接着到了重点！**点击第三个图标“MoltBot Config”（MoltBot 是 OpenClaw 之前的名字，不用纠结），进到配置页，等着填大模型的 API。

![](img/86b48099bf6bc577880e92e64debc291.png)

这个页面先放着，别关。然后咱们去拿大模型的 API。

我选的是百炼 Coding Pl**an。OpenCla**w 运行自动化任务会消耗大量 Token，包月套餐更划算，不用担心“吞金”问题。

百炼这个套餐**包月计算调用额度**，最便宜的一档每月可以调用18000次。我体验下来，6次复杂任务调用了120多次大模型，平均每个任务20次。这样一个月大概能跑900个任务，新手足够用。

![](img/8ef5cd3c6693bdfe9c7e25f896da228a.png)

（百炼coding plan 官网：www.aliyun.com/benefit/scene/codingplan）

购买完成后，去 Coding Plan 的配置台生成一个 API Key。

**这里必须划个重点！**

**百炼 codi****ng **plan 的 API Key 必须在这里生成，它跟百炼控制台生成的通用** AP**I Key 不共享！我在这里也踩了坑（一开始配置的是通用API Key，两三个任务就把免费的 100万 Tokens 跑完了，还倒欠了阿里4块8...）

两个不同的 API Key 也很好区分。C**oding ****Pla**n 生成的Key是 “sk-sp- ”开头的，通用的 API Key 只有 “sk-”。

![](img/957e515a16e5df5281f8e32cd3c20e68.png)

（coding plan 配置台链接：https://bailian.console.aliyun.com/?spm=a2c4g.11186623.0.0.3ef96889zofjx3&tab=globalset#/efm/coding_plan）

生成好 API Key 以后，点右侧的“复制”。

![](img/fa8be213635974bf910bdd86d1581f5f.png)

再回到无影云电脑的“MoltBot Config”页**面里，把 API** Key 粘贴进下方的 Base URL 一栏👇。直接复制粘贴这个链接就可以：https://coding.dashscope.aliyuncs.com/v1

“保存配置”，API 部分搞定！

![](img/e2e0f72eef53daa6aa6c471a88f45851.png)

这**里还要提醒一下**，配置好 API Key、BASE URL 后，如果点左下角的“测试连接”，可能会出现红色的报错信息，直接忽略就行。

只要确保我们复制粘贴的两处信息没有错误，就能用。我也被这个报错惊了一下，看完官方文档反复确认粘贴这个。

![](img/383b6b75084c20b5465dcf36c999b4eb.png)

*我的报错信息长这样，不过之后我确认了 OpenClaw 是可以正常运行的

接下来就是最后一步，配置一个收发消息的聊天工具。完成这一步，我们这就能在手机或其他设备上和 OpenClaw 对话了！

既然已经用了无影和百炼，这一步我选择钉钉，直接阿里全家桶走起。

登录钉钉开发者平台（https://open-dev.dingtalk.com/），点击“应用开发”里的“创建应用”。

![](img/6ae8f5c67067991fa5f577dce7ef6e42.png)

在弹出页面里填好“应用名称”和“应用描述”。

![](img/bcf4211a7319aa238cd15ae7343995ce.png)

保存好以后，在左边菜单栏找到第一个选项“凭证与基础信息”，点击进入下一个页面。

![](img/8c14ef5f6c2fcfaaf7e85232919d3b3a.png)

复制 **Client ID** 和 **Client Secret**，再回到无影云电脑的“MoltBot Config”配置页面，选择左边第三个“通道设置”，打开“钉钉通道”的开关，依次粘贴进去，然后保存。

![](img/d58334d971bb92bca672be281b24f34b.png)

以上都完成之后，就要回到钉钉开发者平台添加机器人🤖。

**在钉钉开发**者平台点击左侧的“添加应用能力”，找到最右边那个“机器人”，点“添加”，其他不用管。

![](img/26a996873d0249944e5dac57a29aaa1a.png)

添加完成，点击左侧菜单栏出现的“机器人”选项。在右边的页面里打开“机器人配置”后面的开关。

![](img/9b1f23cc82a471bfc8a3a80c99be22fc.png)

**把页面上所有带星星***的选项填好，页面底部的“消息接收模式”也选择“Stream模式”。（这里有一个必填项叫“机器人消息预览图”，我随便传了一张图上去）

![](img/82b57cf283e9567b33476b610478b469.png)

接下来就到最后一步了！点击页面上方的“查看版本详情”，依次填写“版本详情”和“版本描述”。最后保存，发布。（一定要保存，直接点右侧的“发布”按钮会报错）

![](img/048cd097268dcefb77d5ee0dc203b7f1.png)

**配置部分到此结束！**

打开钉钉，在顶部搜索栏中输入 bot 的名字，就能看到刚刚创建的 bot 🤖🤖（忽略我这里出现了两个，录教程中途忘记截图，我又做了一个 bot 🙃）

![](img/a352ed97e5a70dbd4d2a0bfbf8fef795.png)

一个自带一堆skill的OpenClaw

配置完成，来测测它的真实水平。

OpenClaw 好不好用，要看它掌握的 skill 多不多，我先问它：目前都掌握了哪些 skill。

它直接列了一堆：消息通信、数据内容管理、创意媒体……N 多技能点。

![](img/9d2bb107310ffee439315a9d9118a857.png)

之前我通过其他云厂商安装的 OpenClaw，只有3个 skill。对比下来，这个版本的 OpenClaw 是高级技能库了。

![](img/bbd8229b7a2304e659ac7b81ca42bcf8.png)

随手丢个任务，让它看看目前“Hacker News”上最新和最热门的一条消息分别是什么。

不到 2 分钟，它完成了任务。

重点拎得很清，有总结，还带原帖链接，没糊弄，哈哈。

![](img/e2a56cbae469152b767fd8f57abe92a6.png)

我又要求它自动监控 Hacker News。差不多3分钟的时间，任务设置完成。

![](img/6be21e101655ec1ceeadbc19a2735b0b.png)

![](img/730009304c9ad532a8c3650456519da2.png)

![](img/25eea1b7155600c25c78042b790d2d27.png)

（滑动查看更多）

我准备再“为难”一下它。

看到它还集成了 Obsidian 笔记的功能，我想用好看的笔记样式，给每日简报升个级。

![](img/504f89a25fe0e52a2969187cfc068051.png)

这里小翻车了一下。它发出来还是文本消息卡片，但能看到内容已经用 Markdown 格式整理干净了。

最后再试一个高频刚需：网页文章一键总结。

平时我要读的东西太多，有这功能能省不少命。

大概用了十几秒，OpenClaw 完成了任务，还用 emoji 和段落整理好。这种效率，是真的能省时间。

![](img/dbf65488406280dde3122a26920bddaa.png)

一圈测下来，感觉阿里云这套「无影云电脑 + 百炼 Coding Plan + 钉钉 Bot」组合，确实把 OpenClaw 门槛压到最低。

毕竟它不用敲代码、不用砸硬件，十分钟就能拿到一个带几十种 Skill 的 AI 助手。

# 5、把AI龙虾养进🛰️：OpenClaw接入教程

> 以下内容引用自：王星星AI设计

当然，能干活的**前置准备**在开始之前，你需要准备三样东西：

![](img/34c622b7a6a24239ab984f2f9170607b.png)

+   一台有**公网IP的服务器**（建议直接用云服务器）

+   一个**企业🛰️账号**（🛰️扫码注册即可，不需要营业执照）

+   一个**大模型的 API Key**（OpenClaw 只是执行框架，“大脑”需要你自己接）

Step 1：买一个公网IP的服务器，以腾讯云为例

https://curl.qcloud.com/DXgwdieR

来到腾讯云买一个**公网IP的**轻量级应用服务器

![](img/b69cc502d01a88a0ad7f379eec09cd9f.png)

![](img/b314bc9498107ec5156da9ae998886f4.png)

**

Step 2：链接你的大模**型    关于这点，多说两句。OpenClaw 本身不带智能，它只是一个“执行引擎”，真正理解你的指令、做出判断的，是背后接的大模型。国内用户推荐接DeepSeek、智谱 或者 MinMax （新出 MinMax的2.5比较便宜，中文理解能力强，29块/月），也可以用 OpenAI 的 GPT-4o 或者 Anthropic 的 Claude。

![](img/4e12dd4904c9c89ec6f61075adfea385.png)

配置方式都一样，在对话框内字段里填入对应的模型名称和 API Key 即可：填完保存，大脑就装好了。接下来开始配置🛰️接入。

Step 3：企业🛰️后台创建自建应用接下来的几步，你需要在企业🛰️后台和服务器配置文件之间来回操作，建议两个窗口并排开着。

![](img/a096eee7e8e7df73d0f60e0b71768fa3.png)

登录企业🛰️后台，进入** 应用管理 → 自建应**用，创建一个新应用（名字随意，上传一个 OpenClaw 的 logo 当头像也挺好玩的）。

![](img/5571622ad7f1d152de39bc3d979487f5.png)

![](img/3e15385606e5fe227b4c820fe1a2f4be.png)

创建完成后，记下两个关键信息，后面要用：

+   **AgentID**

+   **Secret**（这个不要泄露给任何人）

![](img/9064a12648d883a906a82896bed31872.png)

还有一步容易被忽略但很关键：**配置IP白名单**。企业🛰️要求，只有加入白名单的服务器IP，才能通过 API 正常收发消息。找到应用设置里的“企业可信IP”，把你服务器的公网IP填进去，否则后面回调地址填了也会报错。

![](img/64ce6e900152168cfc19eaea296a68a2.png)

**

Step 4：获取回调配置**信息

![](img/9d4973465dc3b77985704eb28c3bf9b9.png)

在刚创建的应用里，找到“接收消息”，点击“API 接收”，然后点“随机获取”，系统会生成两个参数：

![](img/6dad60117cde9a83664122a53907ced9.png)

+   **Token**

+   **EncodingAESKey**

**“接收消**息”URL 一栏先空着等第六步骤操作完再点【保存】

把这两个值保存下来。同时，在“我的企业”页面找到你的企业ID，也记录好。

![](img/6b5528ba98cf9abaeb7af78d4765d75c.png)

**

Step 5：把参数**填回把刚才在企业🛰️后台拿到的几个参数，填回服务器上的配置文件里。

![](img/0c6a5616157f04072d8ca9f5a0897efc.png)

填完之后，等待配置生效

Step 6：填写回调地址，打通完成回到企业🛰️后台，在应用的“接收消息”URL 一栏，填入：

![](img/e1ba249031c2718e2ea1319c87a21b24.png)

http://你的服务器IP:18789/wecom/agent  （假如我的ip地址是0.0.0.0 则此处应该填写：http://0.0.0.0:18789/wecom/agent）

![](img/063fcb3d69e7b0ad8df994b8a09eb573.png)

**保存**，如果没有报错，说明打通成功了。注意：以上端口号和插件包名以 OpenClaw 官方文档为准，正式操作前建议对照官方文档核实。添加至个人🛰️：  在企业微**信后台 我的企**业** -> 🛰️插件 中，使**用个**人🛰️扫描邀请二维码。这时**候打开🛰️，找到刚才创建的企业应用，发一条消息试试——龙虾回应了，就说明它已经住进你的🛰️了。有一点需要说**明：🛰️里这个智能体不能**被拉进群聊，只能单独对话。因为它本质上是一个企业应用，不是一个账号。如果你需要在群里用，企业🛰️那边是可以的。龙虾养进来了，但有几件事得提前说清楚。第一，权限能给小就给小。 OpenClaw 拥有和你一样的系统权限，理论上它能操作你授权给它的一切。所以第一次用，建议先拿一个测试邮箱、测试文件夹试水，别上来就把最重要的东西交给它。确认它的行为符合预期之后，再逐步扩大授权范围。第二，重要操作前做好备份。 OpenClaw 在处理大量信息时，有概率“遗忘”之前的安全指令——这不是bug，是大语言模型的底层机制决定的。前面 Meta 安全总监的故事就是前车之鉴。凡是涉及邮件批量操作、文件整理这类“不可逆”的任务，先备份，再放行。第三，记住紧急停止命令。 如果发现龙虾跑偏了，在🛰️里发送 /stop，可以立即中断它正在执行的任务。这个命令建议提前记下来，关键时刻能救命。第四，别让它自己和别的AI说话。 OpenClaw 支持 Agent 之间互相通信，但这个功能目前风险较高——两个AI互相对话很容易产生不可预期的行为。初期使用建议关闭这个选项，等你对它足够熟悉之后再考虑开放。