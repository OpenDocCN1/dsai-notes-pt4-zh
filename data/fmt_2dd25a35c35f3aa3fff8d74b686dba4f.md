
# OpenClaw 零基础部署

> 来源：[https://geekbang.feishu.cn/docx/KIttdPhgZogOWoxFXsccYTIGnud](https://geekbang.feishu.cn/docx/KIttdPhgZogOWoxFXsccYTIGnud)

## 1、Windows系统部署

在Windows系统部署OpenClaw有以下两种常见方案，可根据自身需求选择：

### 方案一：基于WSL2的部署（推荐）

1. 启用WSL2功能组件

以管理员身份启动Windows PowerShell，执行以下命令：

```powershell
dism.exe /online /enable-feature /featurename:microsoft-windows-subsystem-linux /all /norestart
dism.exe /online /enable-feature /featurename:virtualmachineplatform /all /norestart
```

2. 系统重启后，设置WSL默认版本为V2：

```powershell
wsl --set-default-version 2
```

3. 手动部署Ubuntu发行版

访问Microsoft官方存储库（`https://aka.ms/wslubuntu2204`）下载Ubuntu 22.04 LTS安装包，通过PowerShell执行本地安装：

```powershell
cd c:\users\(yourusername)\downloads
add-appxpackage .\ubuntu_22.04.221101.appxbundle
```

4. 首次启动Ubuntu时，设置用户名和密码完成初始化。

5. 在WSL2环境中安装OpenClaw

进入Ubuntu终端，执行以下命令：

```bash
# 更新软件包列表
sudo apt update && sudo apt upgrade -y
# 安装Node.js（推荐v24）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
. "$HOME/.nvm/nvm.sh"
nvm install 24
# 验证安装
node -v  # 应显示v24.x.x
npm -v   # 应显示10.x.x
# 全局安装OpenClaw
npm install -g openclaw@latest
# 初始化配置
openclaw onboard --install-daemon
```

6. 按提示选择模型（如Minimax、Qwen等）并完成授权配置。

### 方案二：原生Windows部署

1. 安装Node.js

访问Node.js官网（`https://nodejs.org/zh-cn/download/`）下载Windows安装包，执行标准安装流程，建议选择LTS版本。

2. 调整PowerShell执行策略

以管理员身份启动PowerShell，执行：

```powershell
set-executionpolicy remotesigned -scope currentuser -force
```

3. 一键部署OpenClaw

在PowerShell中执行官方安装脚本：

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

4. 安装完成后，启动配置向导：

```powershell
openclaw onboard
```

5. 按提示完成模型配置和初始设置。

### 部署后验证

无论选择哪种方案，部署完成后可通过浏览器访问`http://127.0.0.1:18789`查看OpenClaw管理界面。若遇到网络或权限问题，可参考官方文档进行排查。

## 2、接飞书的保姆级教程1

> 以下内容引用自：Draco正在VibeCoding

这是一份专门为你`精简提炼`的“保姆级”新手指南。

我们剔除了早期不需要懂的底层配置和部署逻辑，为你找到了一条最平缓的入门曲线（Sweet Spot）：

`从零基础 -> 拿到 API -> 成功安装 -> 接入飞书 -> 安全防泄漏 -> 学会用技能`

准备好了吗？系好安全带，我们要发车了！

你只需要三样东西：

1. 一台能上网的电脑（Windows/Mac/Linux都行）
2. 一个API Key（别被这个词吓到，就是一串密码）
3. 10分钟时间（和一点点耐心）

没了。不需要你是程序员，不需要你懂AI，不需要买服务器。

---

### 2.2 第一样东西：一台电脑

#### 2.2.1 系统要求

![](img/640861bdc51bfe624f6ba2ce4993b8e0.png)

简单说：只要是近5年的电脑，基本都能跑。

#### 2.2.2 网络要求

你需要能访问：

- `npm registry`（安装OpenClaw）
- 你选择的AI服务商（如KIMI、MiniMax等）

国内用户注意：OpenClaw本身不需要🪜，但部分AI服务商可能需要。

### 2.3 第二样东西：一个API Key

#### 2.3.1 API Key是什么？

API Key（应用编程接口密钥），听起来很高大上，其实就是一串密码。

类比一下：

- 饭店的VIP卡 → 证明你有资格享受服务
- 小区的门禁卡 → 证明你有权限进入
- API Key → 证明你有权限调用AI服务

每次OpenClaw让AI帮你干活，都要出示这个Key。AI服务商根据Key来：

1. 确认你是谁
2. 计算你用了多少额度
3. 决定是否响应你的请求

#### 2.3.2 国内三家Coding Plan（推荐）

对于国内用户，我推荐优先选择以下三家。它们都有专门针对开发者的 Coding Plan，且本章统一使用国内站口径（不使用国际站路径）。

##### 2.3.2.1 方案A：KIMI Coding Plan（推荐）

![](img/c3746aecd4f973453423149f13c6060e.png)

5pMQBY**

申请步骤：

1. 访问 `https://www.kimi.com/code`
2. 登录/注册KIMI账号
3. 点击"订阅Coding Plan"
4. 完成支付（支持支付宝/🛰️）
5. 进入控制台，点击"创建API Key"
6. 复制生成的Key（以`sk-`开头）

> 💡 提示：Key创建后只显示一次，务必保存好。如果丢了，只能重新创建。

##### 2.3.2.2 方案B：MiniMax Coding Plan（推荐）

![](img/5d437038da90299a89fc2082f93b6626.png)

Cu5XmF

申请步骤：

1. 访问 `https://platform.minimaxi.com/subscribe/coding-plan`
2. 注册/登录账号
3. 完成实名认证（需要身份证）
4. 订阅Coding Plan
5. 进入"API管理"页面
6. 创建API Key并复制

##### 2.3.2.3 方案C：GLM Coding Plan（推荐）

![](img/65f94fb9fa81ea289a9cb0af4a1a3342.png)

WUTpwh

申请步骤：

1. 访问 `https://bigmodel.cn/glm-coding`
2. 注册/登录智谱AI账号
3. 进入控制台
4. 点击"API Keys"菜单
5. 创建新的API Key
6. 复制保存

#### 2.3.3 三家对比表

> 💡 说明：以上价格均按御三家国内站结算页口径记录；后续如有活动变动，请以实时页面为准。

#### 2.3.4 备选方案

如果上述三家都不适合你，还有以下选择：

##### 2.3.4.1 OpenRouter

特点：一个API对接多家模型（Claude、GPT、Llama等）

网址：`https://openrouter.ai`

适合：想用一个Key调用多种模型的用户

注意：国内访问可能需要代理

##### 2.3.4.2 Anthropic（Claude官方）

特点：Claude模型官方API，质量顶尖

网址：`https://console.anthropic.com`

适合：追求最高质量回复的用户

注意：国内访问需要代理，价格较高

---

### 2.4 第三样东西：10分钟时间

这10分钟你要做什么？

保存Key的建议：

1. **不要**直接保存在🛰️/🐧聊天记录里
2. **不要**截图保存在相册里
3. **推荐**保存在：

- 密码管理器（1Password、Bitwarden等）
- 本地文本文件（放在安全的位置）
- 备忘录（如果支持加密）

> ⚠️ **重要**：API Key就像银行卡密码，泄露了别人就能花你的钱。妥善保管！

### 2.5 提前看看你会得到什么

完成本书学习后，你将拥有：

一个能在飞书里@的AI机器人：

- 私聊问它问题
- 群里@它办事
- 让它帮你整理文档、查资料

一个可定制的AI助理：

- 根据你的需求写Skills
- 连接你的常用工具
- 自动化重复工作

完全掌控的数据隐私：

- 所有数据存在本地
- 不经过第三方服务器
- 企业级安全保障

---

#### 本部分小结

来，检查一下你的准备清单：

- 一台能上网的电脑（Windows/Mac/Linux）
- 一个API Key（KIMI/MiniMax/GLM任选其一）
- 10分钟时间

## 3、接飞书的保姆级教程2

> 以下内容引用自：萤火AI百宝箱

👸号主人也是极客时间的签约讲师，延飞老师，他在大模型、算力调度和云原生应用等领域具备丰富的实战经验。曾主导过通信、金融等行业的大型复杂项目。是技术👸号“萤火 AI 百宝箱”的创办人，出版有《企业大模型实战：核心技术与行业赋能》与《Kubernetes：云原生与容器编排实战》等技术书籍。

以下是正文👇

![](img/978a4c22c91731de29beaa4d14430004.png)

### 0. 虚拟机环境信息

OpenClaw 项目推荐使用 `pnpm` 作为包管理器，建议安装：

```
npm install -g pnpm@10.23.0
```

### 安装OpenClaw

![](img/65a225437808f7aa07ec3572e6fd7f73.png)

选型决定了上限。先花两分钟扫一眼官方的 `README.md`（`https://github.com/openclaw/openclaw/blob/main/README.md`）摸清基本盘。文档明确指出，OpenClaw 可以跑在宿主机、虚拟机或云服务器上。

这只是躯壳，真正的核心在于**交互渠道**和**底层算力**。这里我们不纠结，根据你的网络环境和预算直接抄作业：

- 交互渠道（只在飞书和 TG 中二选一）：
- 肉身在国内 / 追求合规与绝对稳定（选飞书）：国内企业级 IM 的天花板，API 开放且 Webhook 接入极其丝滑。最大的好处是不挑网络，随时随地掏出手机就能指挥 Agent 干活，适合团队协同和严肃的生产力环境。
- 🪜常驻 / 追求极致自由（选 Telegram）：极客的快乐老家。无审核、无限制、Bot 生态逆天。如果你想构建一个完全不受任何国内规则约束的专属控制台，TG 是唯一真神。
- **底层模型（只看顶流）**：Agent 的智商上限完全被模型锁死。既然赋予了它高阶思维和系统权限，就不要在算力上抠搜。毫无疑问，直接接入当前地表最强的闭源模型（如 Claude Opus 或最新的 GPT 顶级系列）。

#### 💡 最佳配置实践方案

根据你的网络基建，对号入座：

> **方案 A（国内全天候生产力）**：VMware 虚拟机 (Ubuntu 22.04) + 飞书机器人 + 顶级大模型 API
>
> **方案 B（无限制极客专属）**：VMware 虚拟机 (Ubuntu 22.04) + Telegram + 顶级大模型 API

#### 使用Docker安装OpenClaw

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
```

从仓库根目录执行以下命令：

> Note
>
> 要求：
>
> - Docker Desktop（或 Docker Engine）+ Docker Compose v2
> - 足够的磁盘空间用于镜像 + 日志

```
./docker-setup.sh
```

此脚本：

- 构建 Gateway 网关镜像
- 运行新手引导向导
- 打印可选的提供商设置提示
- 通过 Docker Compose 启动 Gateway 网关
- 生成 Gateway 网关令牌并写入 `.env`

可选环境变量：

- `OPENCLAW_DOCKER_APT_PACKAGES` — 在构建期间安装额外的 apt 包
- `OPENCLAW_EXTRA_MOUNTS` — 添加额外的主机绑定挂载
- `OPENCLAW_HOME_VOLUME` — 在命名卷中持久化 `/home/node`

![](img/f3b09dbc1f9886a5e70114c2e2270767.png)

运行脚本后，会在宿主机上创建两个重要文件夹，它们会被挂载到 Docker 容器中：

**① `~/.openclaw` — 配置目录**

- 存储 OpenClaw 的记忆
- 配置文件
- 第三方 API 密钥等

**② `~/.openclaw/workspace` — 工作空间目录**

- Agent 可直接访问的文件目录
- Agent 创建的文件也会保存在这里

### 配置 OpenClaw

![](img/8554fccacbaa236c0e3e0fdb19bd0b1c.png)

安装后，配置过程将自动开始。如果没有开始，您可以运行以下命令开始配置：

```
# /openclaw/docker-compose.yml 替换为你的实际路径
docker compose -f /openclaw/docker-compose.yml run --rm openclaw-cli onboard --install-daemon
```

若之前已经初始化，您也可以运行 `openclaw config` 选择 model 配置。

#### 2.1 开始配置：

- I understand this is powerful and inherently risky. Continue? | 选择 ● Yes
- Onboarding mode | 选择 ● Quick Start
- Model/auth provider | 选择 ● Z.AI

![](img/7df0ea6a6297823de0aa4735ffeaa703.png)

#### 2.2 配置 Z.AI 提供商

选择 Z.AI 作为 Model/auth provider 后，选 Coding-Plan-CN，系统会提示您输入 API Key。

1. **获取 API Key**：注册并登录智谱开放平台（`https://open.bigmodel.cn/`），在 API 管理中创建并复制你的专属 Key（`https://bigmodel.cn/usercenter/proj-mgmt/apikeys`）。
2. **订阅套餐**：推荐购买 Coding Lite 套餐（`https://www.bigmodel.cn/glm-coding?ic=WCWN2OARHG`），以极低成本支撑高频需求。

粘贴您的智谱 API Key 并按 Enter 键，然后选择您想要使用的模型，例如: `zai/glm-5`

注意：目前在编程套餐中支持的模型有 GLM-5 GLM-4.7 GLM-4.5-Air GLM-4.6 GLM-4.5 GLM-4.5V GLM-4.6V 请勿选择其它模型 Flash FlashX 以免造成扣费。

![](img/103cd65d6c8404d33130a83e7a4d800a.png)

##### 权限问题：EACCES permission denied

**现象：** 在 Enter Z.AI API key 之后，出现报错：

```
Error: EACCES: permission denied, mkdir '/home/node/.openclaw/agents/main/agent'
```

**原因：** Docker 权限不够 — 容器用户（uid=1000）试图写入 root（或其他用户）拥有的目录。

**解决方案：**

```
chown -R 1000:1000 "$HOME/.openclaw"
```

修改完权限后可以直接执行以下命令重新完成初始化向导：

```
# /openclaw/docker-compose.yml 替换为你的实际路径 
docker compose -f /openclaw/docker-compose.yml run --rm openclaw-cli onboard
```

#### 2.3 设置channel：飞书

##### 创建飞书的机器人

> Note
>
> 没有飞书账号的，需要自己注册账号

我们需要先到飞书平台创建自己的机器人来接入OpenClaw：
